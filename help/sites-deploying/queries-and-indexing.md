---
title: Oak Queries and Indexing
description: Lär dig konfigurera index i Adobe Experience Manager (AEM) 6.5.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
legacypath: /content/docs/en/aem/6-0/deploy/upgrade/queries-and-indexing
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '3034'
ht-degree: 0%

---

# Oak Queries and Indexing{#oak-queries-and-indexing}

>[!NOTE]
>
>Den här artikeln handlar om att konfigurera index i AEM 6. Mer information om hur du optimerar fråga- och indexeringsprestanda finns i [Bästa metoder för frågor och indexering](/help/sites-deploying/best-practices-for-queries-and-indexing.md).

## Introduktion {#introduction}

Till skillnad från Jackrabbit 2 indexerar Oak inte innehåll som standard. Anpassade index måste skapas när det behövs, ungefär som med traditionella relationsdatabaser. Om det inte finns något index för en viss fråga kan många noder gå igenom. Frågan kanske fortfarande fungerar, men den är förmodligen långsam.

Om Oak påträffar en fråga utan index skrivs ett loggmeddelande på WARN-nivå ut:

```xml
*WARN* Traversed 1000 nodes with filter Filter(query=select ...) consider creating an index or changing the query
```

## Frågespråk som stöds {#supported-query-languages}

Oak frågemotor har stöd för följande språk:

* XPath (rekommenderas)
* SQL-2
* SQL (utgått)
* JQOM

## Indexerartyper och kostnadsberäkning {#indexer-types-and-cost-calculation}

Med Apache Oak-baserad backend kan olika indexerare kopplas in i databasen.

En indexerare är **egenskapsindexet**, som indexdefinitionen lagras i själva databasen.

Implementeringar för **Apache Lucene** och **Solr** är också tillgängliga som standard, vilket båda stöder fulltextindexering.

**Traversal Index** används om ingen annan indexerare är tillgänglig. Det innebär att innehållet inte är indexerat och att innehållsnoderna gås igenom för att hitta matchningar med frågan.

Om det finns flera tillgängliga indexerare för en fråga beräknar varje tillgänglig indexerare kostnaden för att köra frågan. Oak väljer sedan indexeraren med den lägsta uppskattade kostnaden.

![chlimage_1-148](assets/chlimage_1-148.png)

Diagrammet ovan är en högnivårepresentation av frågekörningsmekanismen i Apache Oak.

Först tolkas frågan i ett abstrakt syntaxträd. Sedan kontrolleras frågan och omvandlas till SQL-2, som är det ursprungliga språket för Oak-frågor.

Därefter används varje index för att beräkna kostnaden för frågan. När det är klart hämtas resultaten från det lägsta indexvärdet. Slutligen filtreras resultaten, både för att säkerställa att den aktuella användaren har läsåtkomst till resultatet och att resultatet matchar hela frågan.

## Konfigurera index {#configuring-the-indexes}

>[!NOTE]
>
>För en stor databas är det tidskrävande att skapa ett index. Detta gäller både när ett index skapas första gången och när ett index indexeras om (när definitionen ändrats). Se även [Felsöka Oak-index](/help/sites-deploying/troubleshooting-oak-indexes.md) och [Förhindra långsam omindexering](/help/sites-deploying/troubleshooting-oak-indexes.md#preventing-slow-re-indexing).

Om omindexering behövs i stora databaser, särskilt när du använder MongoDB och fulltextindex, bör du överväga att extrahera text och använda eko-run för att skapa det ursprungliga indexet och indexera om.

Index konfigureras som noder i databasen under noden **Oak:index** .

Indexnodens typ måste vara **oak:QueryIndexDefinition.** Det finns flera konfigurationsalternativ tillgängliga för varje indexerare som nodegenskaper. Mer information finns i konfigurationsinformationen för varje indexerartyp nedan.

### Egenskapsindexet {#the-property-index}

Egenskapsindexet är användbart för frågor som har egenskapsbegränsningar men inte är fulltextfrågor. Den kan konfigureras genom att följa proceduren nedan:

1. Öppna CRXDE genom att gå till `http://localhost:4502/crx/de/index.jsp`
1. Skapa en nod under **oak:index**
1. Namnge noden **PropertyIndex** och ange nodtypen till **oak:QueryIndexDefinition**
1. Ange följande egenskaper för den nya noden:

   * **type:** `property` (av typen String)
   * **propertyNames:** `jcr:uuid` (av typen Name)

   I det här exemplet indexeras egenskapen `jcr:uuid`, vars jobb är att visa den universellt unika identifieraren (UUID) för noden som den är kopplad till.

1. Spara ändringarna.

Egenskapsindexet har följande konfigurationsalternativ:

* Egenskapen **type** anger typen av index, och i det här fallet måste den anges till **property**

* Egenskapen **propertyNames** anger listan med egenskaper som lagras i indexet. Om det saknas används nodnamnet som referensvärde för egenskapsnamnet. I det här exemplet läggs egenskapen **jcr:uid**, vars jobb är att visa den unika identifieraren (UUID) för noden, till i indexet.

* Flaggan **unique** som, om den är inställd på **true**, lägger till en unik begränsning i egenskapsindexet.

* Med egenskapen **DeclaringNodeTypes** kan du ange en viss nodtyp som indexet bara gäller för.
* Flaggan **reindex** som, om den är inställd på **true**, utlöser en omindexering av fullständigt innehåll.

### Orderat index {#the-ordered-index}

Det sorterade indexet är ett tillägg till egenskapsindexet. Den har dock tagits bort. Index av den här typen måste ersättas med [Lucene-egenskapsindex](#the-lucene-property-index).

### Fullständigt Lucene-textindex {#the-lucene-full-text-index}

En fulltextindexerare baserad på Apache Lucene finns i AEM 6.

Om ett fulltextindex är konfigurerat kommer alla frågor som har ett fulltextvillkor att använda fulltextindexet, oavsett om det finns andra villkor som är indexerade eller oavsett om det finns en sökvägsbegränsning.

Om inget fulltextindex är konfigurerat fungerar inte frågor med fulltextvillkor som förväntat.

Eftersom indexet uppdateras med hjälp av en asynkron bakgrundstråd är vissa textsökningar inte tillgängliga för ett litet tidsfönster förrän bakgrundsprocesserna har slutförts.

Du kan konfigurera ett fulltextindex för Lucene enligt följande procedur:

1. Öppna CRXDE och skapa en nod under **oak:index**.
1. Namnge noden **LuceneIndex** och ange nodtypen till **oak:QueryIndexDefinition**
1. Lägg till följande egenskaper i noden:

   * **type:** `lucene` (av typen String)
   * **async:** `async` (av typen String)

1. Spara ändringarna.

Lucene-indexet har följande konfigurationsalternativ:

* Egenskapen **type** som anger typen av index måste anges till **lucene**
* Egenskapen **async** som måste anges till **async**. Detta skickar indexuppdateringsprocessen till en bakgrundstråd.
* Egenskapen **includePropertyTypes** som definierar vilken delmängd av egenskapstyper som ingår i indexet.
* Egenskapen **excludePropertyNames** som definierar en lista med egenskapsnamn - egenskaper som ska uteslutas från indexet.
* Flaggan **reindex** som utlöser en omindexering av fullständigt innehåll när den anges till **true**.

### Förstå fulltextsökning {#understanding-fulltext-search}

Dokumentationen i det här avsnittet gäller till exempel Apache Lucene, Elasticsearch och fulltextindex för PostgreSQL, SQLite och MySQL. Följande exempel är för AEM / Oak / Lucene.

<b>Data som ska indexeras</b>

Startpunkten är de data som måste indexeras. Ta följande dokument som exempel:

| <b>Dokument-ID</b> | <b>Sökväg</b> | <b>Fulltext</b> |
| --- | --- | --- |
| 100 | /content/rubik | &quot;Rubik är ett finskt varumärke.&quot; |
| 200 | /content/rubiksCube | &quot;Rubiks kub uppfanns 1974.&quot; |
| 300 | /content/cube | &quot;En kub är ett tredimensionellt objekt.&quot; |


<b>Inverterat index</b>

Indexeringsfunktionen delar upp fulltexten i ord som kallas &quot;tokens&quot; och skapar ett index som kallas &quot;inverterat index&quot;. Indexet innehåller en lista med dokument där det finns för varje ord.

Korta, vanliga ord (kallas även&quot;stopwords&quot;) indexeras inte. Alla variabler konverteras till gemener och ordstam används.

Specialtecken som *-* är inte indexerade.

| <b>Token</b> | <b>Dokument-ID</b> |
| --- | --- |
| 194 | ..., 200,... |
| varumärke | ..., 100,... |
| kub | ..., 200, 300,... |
| dimension | 300 |
| avsluta | ..., 100,... |
| uppfinna | 200 |
| object | ..., 300,... |
| rubik | ..., 100, 200,... |

Listan med dokument sorteras. Det här är praktiskt när du frågar.

<b>Söker</b>

Nedan visas ett exempel på en fråga. Observera att alla specialtecken (som *&#39;*) ersattes med ett blanksteg:

```
/jcr:root/content//element(\*; cq:Page)`[` jcr:contains('Rubik s Cube')`]`
```

Orden tokeniseras och filtreras på samma sätt som vid indexering (enstaka teckenord tas till exempel bort). Så i det här fallet är sökningen för:

```
+:fulltext:rubik +:fulltext:cube
```

Indexet läser listan med dokument för dessa ord. Om det finns många dokument kan listan vara stor. Anta att de innehåller följande:


| <b>Token</b> | <b>Dokument-ID</b> |
| --- | --- |
| rubik | 10, 100, 200, 1000 |
| kub | 30, 200, 300, 2000 |


Lucene bläddrar fram och tillbaka mellan de två listorna (eller rundrobat `n`-listor, när du söker efter `n` ord):

* Läs in&quot;rubik&quot; hämtar den första posten: hittar 10
* Läsning i kuben hämtar den första posten `>` = 10. 10 hittas inte och nästa är 30.
* Läs i rubik-filen hämtar den första posten `>` = 30: 100 hittas.
* Läsning i kuben hämtar den första posten `>` = 100: 200 hittas.
* Läs i rubik-filen hämtar den första posten `>` = 200. 200 hittades. Så dokument 200 är en matchning för båda villkoren. Det här kommer ihåg.
* Läs in &quot;rubik&quot; hämtar nästa post: 1000.
* Läsning i kuben hämtar den första posten `>` = 1000: 2000 hittas.
* Läs i rubik-filen hämtar den första posten `>` = 2000: slutet av listan.
* Slutligen kan du sluta söka.

Det enda dokument som innehåller båda villkoren är 200, som i exemplet nedan:

| 200 | /content/rubiksCube | &quot;Rubiks kub uppfanns 1974.&quot; |
| --- | --- | --- |

När flera poster hittas sorteras de sedan efter poäng.

### Egenskapsindexet Lucene {#the-lucene-property-index}

Sedan **Oak 1.0.8** kan Lucene användas för att skapa index som innehåller egenskapsbegränsningar som inte är fulltext.

För att uppnå ett Lucene-egenskapsindex måste egenskapen **fulltextEnabled** alltid anges till false.

Ta följande exempelfråga:

```xml
select * from [nt:base] where [alias] = '/admin'
```

Om du vill definiera ett Lucene-egenskapsindex för frågan ovan kan du lägga till följande definition genom att skapa en nod under **`oak:index`:**

* **Namn:** `LucenePropertyIndex`
* **Typ:** `oak:QueryIndexDefinition`

När noden har skapats lägger du till följande egenskaper:

* **typ:**

  ```xml
  lucene (of type String)
  ```

* **asynkron:**

  ```xml
  async (of type String)
  ```

* **fulltextEnabled:**

  ```xml
  false (of type Boolean)
  ```

* **includePropertyNames:** `["alias"] (of type String)`

>[!NOTE]
>
>Jämfört med det vanliga egenskapsindexet är Lucene-egenskapsindexet alltid konfigurerat i asynkront läge. Resultatet som returneras av indexet kanske därför inte alltid återspeglar databasens senaste status.

>[!NOTE]
>
>Mer information om Lucene-egenskapsindexet finns på [dokumentationssidan för Apache Jackrabbit Oak Lucene](https://jackrabbit.apache.org/oak/docs/query/lucene.html).

### Lucene Analyzers {#lucene-analyzers}

Sedan version 1.2.0 stöder Oak Lucene-analysatorer.

Analysatorer används både när ett dokument indexeras och vid frågetillfället. En analyserare undersöker texten i fälten och genererar en tokenström. Lucene-analysatorerna består av en serie tokeniserings- och filterklasser.

Analysatorerna kan konfigureras med noden `analyzers` (av typen `nt:unstructured`) inuti definitionen `oak:index`.

Standardanalysatorn för ett index är konfigurerad i den underordnade `default`-noden för analysatornoden.

![chlimage_1-149](assets/chlimage_1-149.png)

>[!NOTE]
>
>En lista över tillgängliga analysatorer finns i API-dokumentationen för Lucene-versionen som du använder.

#### Ange klassen Analyzer direkt {#specifying-the-analyzer-class-directly}

Om du vill använda någon av de färdiga analysverktygen kan du konfigurera den enligt följande procedur:

1. Leta reda på det index som du vill använda analyseraren med under noden `oak:index`.

1. Skapa en underordnad nod med namnet `default` av typen `nt:unstructured` under indexet.

1. Lägg till en egenskap i standardnoden med följande egenskaper:

   * **Namn:** `class`
   * **Typ:** `String`
   * **Värde:** `org.apache.lucene.analysis.standard.StandardAnalyzer`

   Värdet är namnet på den analysklass som du vill använda.

   Du kan också ange att analyseraren ska användas med en specifik lucene-version genom att använda den valfria strängegenskapen `luceneMatchVersion`. En giltig syntax för användning med Lucene 4.7 är:

   * **Namn:** `luceneMatchVersion`
   * **Typ:** `String`
   * **Värde:** `LUCENE_47`

   Om `luceneMatchVersion` inte anges används den version av Lucene som levereras med Oak.

1. Om du vill lägga till en stoppordsfil i analyskonfigurationerna kan du skapa en nod under `default` med följande egenskaper:

   * **Namn:** `stopwords`
   * **Typ:** `nt:file`

#### Skapa analysatorer med hjälp av komposition {#creating-analyzers-via-composition}

Analysatorer kan också sättas samman baserat på `Tokenizers`, `TokenFilters` och `CharFilters`. Du kan göra detta genom att ange en analysator och skapa underordnade noder till dess tillvalstokeniserare och filter som tillämpas i listordning. Se även [https://cwiki.apache.org/confluence/display/solr/AnalyzersTokenizersTokenFilters#Specifying_an_Analyzer_in_the_schema](https://cwiki.apache.org/confluence/display/solr/AnalyzersTokenizersTokenFilters#Specifying_an_Analyzer_in_the_schema)

Se den här nodstrukturen som ett exempel:

* **Namn:** `analyzers`

   * **Namn:** `default`

      * **Namn:** `charFilters`
      * **Typ:** `nt:unstructured`

         * **Namn:** `HTMLStrip`
         * **Namn:** `Mapping`

      * **Namn:** `tokenizer`

         * **Egenskapsnamn:** `name`

            * **Typ:** `String`
            * **Värde:** `Standard`

      * **Namn:** `filters`
      * **Typ:** `nt:unstructured`

         * **Namn:** `LowerCase`
         * **Namn:** `Stop`

            * **Egenskapsnamn:** `words`

               * **Typ:** `String`
               * **Värde:** `stop1.txt, stop2.txt`

            * **Namn:** `stop1.txt`

               * **Typ:** `nt:file`

            * **Namn:** `stop2.txt`

               * **Typ:** `nt:file`

Namnet på filtren, charFilters och tokenizers formas genom att fabrikssuffixen tas bort. Således

* `org.apache.lucene.analysis.standard.StandardTokenizerFactory` blir `standard`

* `org.apache.lucene.analysis.charfilter.MappingCharFilterFactory` blir `Mapping`

* `org.apache.lucene.analysis.core.StopFilterFactory` blir `Stop`

Alla konfigurationsparametrar som krävs för fabriken anges som egenskapen för noden i fråga.

För exempelvis inläsning av stoppord där innehåll från externa filer måste läsas in, kan innehållet anges genom att en underordnad nod av typen `nt:file` skapas för filen i fråga.

### Solr-index {#the-solr-index}

Syftet med Solr-indexet är fulltextsökning, men det kan också användas för indexsökning efter sökväg, egenskapsbegränsningar och primära typbegränsningar. Det innebär att Solr-indexet i Oak kan användas för alla typer av JCR-frågor.

Integreringen i AEM sker på databasnivå så att Solr är ett av de möjliga index som kan användas i Oak, den nya databasimplementeringen som levererades med AEM.

Den kan konfigureras för att fungera som en fjärrserver med AEM-instansen.

### Konfigurera AEM med en enda fjärransluten Solr-server {#configuring-aem-with-a-single-remote-solr-server}

AEM kan även konfigureras för att fungera med en fjärrserver för Solr:

1. Hämta och extrahera den senaste versionen av Solr. Mer information om hur du gör detta finns i [dokumentationen för installation av Apache Solr](https://solr.apache.org/guide/6_6/installing-solr.html).
1. Skapa nu två Solr-kort. Du kan göra detta genom att skapa mappar för varje delning i mappen där Solr har packats upp:

   * Skapa mappen för det första delfönstret:

   `<solrunpackdirectory>\aemsolr1\node1`

   * Skapa mappen för den andra delningen:

   `<solrunpackdirectory>\aemsolr2\node2`

1. Leta reda på exempelinstansen i Solr-paketet. Den finns i en mapp med namnet `example` i paketets rot.
1. Kopiera följande mappar från exempelinstansen till de två delade mapparna ( `aemsolr1\node1` och `aemsolr2\node2`):

   * `contexts`
   * `etc`
   * `lib`
   * `resources`
   * `scripts`
   * `solr-webapp`
   * `webapps`
   * `start.jar`

1. Skapa en mapp med namnet `cfg` i var och en av de två delade mapparna.
1. Placera dina Solr- och Zookeeper-konfigurationsfiler i de `cfg` nyligen skapade mapparna.

   >[!NOTE]
   >
   >Mer information om Solr- och ZooKeeper-konfigurationen finns i [dokumentationen för Solr-konfiguration](https://cwiki.apache.org/confluence/display/solr/ConfiguringSolr) och i [Starthandboken för ZooKeeper](https://zookeeper.apache.org/doc/r3.1.2/zookeeperStarted.html).

1. Starta den första delningen med stöd för ZooKeeper genom att gå till `aemsolr1\node1` och köra följande kommando:

   ```xml
   java -Xmx2g -Dbootstrap_confdir=./cfg/oak/conf -Dcollection.configName=myconf -DzkRun -DnumShards=2 -jar start.jar
   ```

1. Starta den andra delningen genom att gå till `aemsolr2\node2` och köra följande kommando:

   ```xml
   java -Xmx2g -Djetty.port=7574 -DzkHost=localhost:9983 -jar start.jar
   ```

1. När båda delarna har startats testar du att allt är igång genom att ansluta till Solr-gränssnittet på `http://localhost:8983/solr/#/`
1. Starta AEM och gå till webbkonsolen på `http://localhost:4502/system/console/configMgr`
1. Ange följande konfiguration under **Fjärrserverkonfigurationen för Oak Solr**:

   * HTTP-URL för Solr: `http://localhost:8983/solr/`

1. Välj **Fjärrserver** i listrutan under **Oak Solr**-serverprovidern.

1. Gå till CRXDE och logga in som administratör.
1. Skapa en nod med namnet **solrIndex** under **oak:index** och ange följande egenskaper:

   * **type:** solr (av typen String)
   * **async:** async (av typen String)
   * **reindex:** true (av typen Boolean)

1. Spara ändringarna.

#### Rekommenderad konfiguration för Solr {#recommended-configuration-for-solr}

Nedan visas ett exempel på en baskonfiguration som kan användas med alla tre Solr-distributioner som beskrivs i den här artikeln. Den innehåller de dedikerade egenskapsindexen som redan finns i AEM. Använd inte med andra program.

Om du vill använda det på rätt sätt måste du placera innehållet i arkivet direkt i Solr Home Directory. Om det finns distributioner med flera noder bör den placeras direkt under rotmappen för varje nod.

Rekommenderade Solr-konfigurationsfiler

[Hämta fil](assets/recommended-conf.zip)

### AEM indexeringsverktyg {#aem-indexing-tools}

AEM 6.1 integrerar även två indexeringsverktyg som finns i AEM 6.0 som en del av Adobe Consulting Services Commons-verktygen:

1. **Förklara fråga**, ett verktyg som är utformat för att hjälpa administratörer att förstå hur frågor utförs;
1. **Oak Index Manager**, ett webbanvändargränssnitt för att underhålla befintliga index.

Nu kan du nå dem genom att gå till **Verktyg - Åtgärder - Kontrollpanel - Diagnos** från välkomstskärmen i AEM.

Mer information om hur du använder dem finns i [dokumentationen för kontrollpanelen för åtgärder](/help/sites-administering/operations-dashboard.md).

#### Skapa egenskapsindex med OSGi {#creating-property-indexes-via-osgi}

ACS Commons-paketet visar även OSGi-konfigurationer som kan användas för att skapa egenskapsindex.

Du kan komma åt den från webbkonsolen genom att söka efter **Kontrollera Oak-egenskapsindex**.

![chlimage_1-150](assets/chlimage_1-150.png)

### Felsöka indexeringsproblem {#troubleshooting-indexing-issues}

Det kan uppstå situationer när frågor tar lång tid att köra och den allmänna svarstiden för systemet är långsam.

I det här avsnittet ges rekommendationer om vad som måste göras för att spåra orsaken till sådana problem och råd om hur man löser dem.

#### Förbereder felsökningsinformation för analys {#preparing-debugging-info-for-analysis}

Det enklaste sättet att få den information som krävs för frågan som körs är via verktyget [Förklara fråga](/help/sites-administering/operations-dashboard.md#explain-query). På så sätt kan du samla in exakt den information som behövs för att felsöka en långsam fråga utan att behöva läsa loggnivåinformationen. Detta är önskvärt om du känner till frågan som felsöks.

Om detta inte är möjligt av någon anledning, kan du samla indexeringsloggarna i en enda fil och använda den för att felsöka just det problemet.

#### Aktivera loggning {#enable-logging}

Om du vill aktivera loggning måste du aktivera **DEBUG**-nivåloggar för de kategorier som gäller Oak-indexering och frågor. Dessa kategorier är:

* org.apache.jackrabbit.oak.plugins.index
* org.apache.jackrabbit.oak.query
* com.day.cq.search

Kategorin **com.day.cq.search** kan bara användas om du använder verktyget QueryBuilder från AEM.

>[!NOTE]
>
>Det är viktigt att loggarna bara är inställda på DEBUG så länge frågan som du vill felsöka körs. I annat fall genereras många händelser i loggarna över tid. På grund av detta växlar du tillbaka till INFO-nivåloggning för de kategorier som nämns ovan när de obligatoriska loggarna har samlats in.

Du kan aktivera loggning genom att följa den här proceduren:

1. Peka webbläsaren på `https://serveraddress:port/system/console/slinglog`
1. Klicka på knappen **Lägg till ny loggare** i den nedre delen av konsolen.
1. Lägg till de kategorier som nämns ovan på den nyligen skapade raden. Du kan använda **+**-tecknet för att lägga till mer än en kategori i en enskild loggare.
1. Välj **DEBUG** i listrutan **Loggnivå**.
1. Ställ in utdatafilen på `logs/queryDebug.log`. Detta korrelerar alla DEBUG-händelser till en enda loggfil.
1. Kör frågan eller återge sidan som använder frågan som du vill felsöka.
1. När du har kört frågan går du tillbaka till loggningskonsolen och ändrar loggnivån för den nyligen skapade loggningsfunktionen till **INFO**.

#### Indexkonfiguration {#index-configuration}

Hur frågan utvärderas påverkas i hög grad av indexkonfigurationen. Det är viktigt att få indexkonfigurationen analyserad eller skickad till support. Du kan antingen hämta konfigurationen som ett innehållspaket eller hämta en JSON-återgivning.

Oftast lagras indexkonfigurationen under noden `/oak:index` i CRXDE. Du kan hämta JSON-versionen på:

`https://serveraddress:port/oak:index.tidy.-1.json`

Om indexet är konfigurerat på en annan plats ändrar du sökvägen i enlighet med detta.

#### MBean output {#mbean-output}

Ibland kan det vara praktiskt att ange utdata för indexrelaterade MBeans för felsökning. Du kan göra detta genom att:

1. Gå till JMX-konsolen på:
   `https://serveraddress:port/system/console/jmx`

1. Sök efter följande MBeans:

   * Lucene-indexstatistik
   * CopyOnRead - supportstatistik
   * Oak Query-statistik
   * IndexStats

1. Klicka på varje MBeans så att du kan få statistik över prestanda. Skapa en skärmbild eller anteckna dem om en supportanmälan behövs.

Du kan också hämta JSON-varianten av statistiken på följande URL:er:

* `https://serveraddress:port/system/sling/monitoring/mbeans/org/apache/jackrabbit/oak/%2522LuceneIndex%2522.tidy.-1.json`
* `https://serveraddress:port/system/sling/monitoring/mbeans/org/apache/jackrabbit/oak/%2522LuceneIndex%2522.tidy.-1.json`
* `https://serveraddress:port/system/sling/monitoring/mbeans/org/apache/jackrabbit/oak/%2522LuceneIndex%2522.tidy.-1.json`
* `https://serveraddress:port/system/sling/monitoring/mbeans/org/apache/jackrabbit/oak/%2522LuceneIndex%2522.tidy.-1.json`

Du kan också tillhandahålla konsoliderade JMX-utdata via `https://serveraddress:port/system/sling/monitoring/mbeans/org/apache/jackrabbit/oak.tidy.3.json`. Detta inkluderar all Oak-relaterad MBean-information i JSON-format.

#### Annan information {#other-details}

Du kan samla in ytterligare information som kan hjälpa dig att felsöka problemet, till exempel:

1. Den Oak-version som instansen körs på. Du kan se detta genom att öppna CRXDE och titta på versionen i det nedre högra hörnet av välkomstsidan, eller genom att kontrollera versionen av `org.apache.jackrabbit.oak-core`-paketet.
1. Felsökningsutdata för QueryBuilder-felsökningsfrågan. Felsökaren kan nås på: `https://serveraddress:port/libs/cq/search/content/querydebug.html`
