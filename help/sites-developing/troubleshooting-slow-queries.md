---
title: Felsöka långsamma frågor
description: Lär dig felsöka långsamma frågor i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2237'
ht-degree: 0%

---

# Felsöka långsamma frågor{#troubleshooting-slow-queries}

## Långsam frågeklassificering {#slow-query-classifications}

Det finns tre huvudklassificeringar av långsamma frågor i AEM, ordnade efter allvarlighetsgrad:

1. **Indexlösa frågor**

   * Frågor som **inte** löser sig till ett index och går igenom JCR-innehållet för att samla in resultat

1. **Dåligt begränsade (eller begränsade) frågor**

   * Frågor som leder till ett index, men som måste gå igenom alla indexposter för att samla in resultat

1. **Stora resultatuppsättningsfrågor**

   * Frågor som returnerar ett stort antal resultat

De två första klassificeringarna av frågor (indexlösa och dåligt begränsade) är långsamma. De är långsamma eftersom de tvingar Oak-frågemotorn att inspektera varje **potentiell**-resultat (innehållsnod eller indexpost) för att identifiera vilket som tillhör **faktisk**-resultatmängden.

Att inspektera varje möjligt resultat kallas att gå igenom.

Eftersom varje potentiellt resultat måste kontrolleras, växer kostnaden för att fastställa det faktiska resultatet linjärt med antalet möjliga resultat.

Genom att lägga till frågebegränsningar och justera index kan indexdata lagras i ett optimerat format som ger snabb resultathämtning och minskar eller eliminerar behovet av linjär kontroll av potentiella resultatuppsättningar.

I AEM 6.3 misslyckas frågan som standard när en genomgång på 100 000 nås och ett undantag genereras. Den här gränsen finns inte som standard i AEM-versioner före AEM 6.3, men den kan anges med hjälp av konfigurationen OSGi för Apache Jackrabbit Query Engine Settings och JMX-böna för QueryEngineSettings (egenskapen LimitReads).

### Identifiera indexlösa frågor {#detecting-index-less-queries}

#### Under utveckling {#during-development}

Förklara **alla**-frågor och kontrollera att deras frågeplaner inte innehåller förklaringen **/&amp;ast; gå igenom** i dem. Exempel på genomgång av frågeplan:

* **PLAN:** `[nt:unstructured] as [a] /* traverse "/content//*" where ([a].[unindexedProperty] = 'some value') and (isdescendantnode([a], [/content])) */`

#### Efter distribution {#post-deployment}

* Övervaka `error.log` för indexlösa genomgångsfrågor:

   * `*INFO* org.apache.jackrabbit.oak.query.QueryImpl Traversal query (query without index) ... ; consider creating and index`
   * Det här meddelandet loggas bara om det inte finns något index tillgängligt och om frågan eventuellt går igenom många noder. Meddelanden loggas inte om ett index är tillgängligt, men mängden att gå igenom är liten och därmed snabb.

* Besök AEM [frågeprestanda](/help/sites-administering/operations-dashboard.md#query-performance) åtgärdskonsol och [Förklara](/help/sites-administering/operations-dashboard.md#explain-query) långsamma frågor som letar efter en genomgång eller inga indexfrågeförklaringar.

### Identifierar dåligt begränsade frågor {#detecting-poorly-restricted-queries}

#### Under utveckling {#during-development-1}

Förklara alla frågor och kontrollera att de matchar ett index som justerats för att matcha frågans egenskapsbegränsningar.

* Den idealiska frågeplandisponeringen har `indexRules` för alla egenskapsbegränsningar och minst för de tätaste egenskapsbegränsningarna i frågan.
* Frågor som sorterar resultat bör matchas till ett Lucene-egenskapsindex med indexregler för de sorterade efter egenskaper som anger `orderable=true.`

#### Standardvärdet `cqPageLucene` har till exempel ingen indexregel för `jcr:content/cq:tags` {#for-example-the-default-cqpagelucene-does-not-have-an-index-rule-for-jcr-content-cq-tags}

Innan indexregeln cq:tags läggs till

* **cq:tags indexregel**

   * Finns inte i lådan

* **Fråga** i Frågebyggaren

  ```js
  type=cq:Page
  property=jcr:content/cq:tags
  property.value=my:tag
  ```

* **Frågeplan**

  `[cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* where [a].[jcr:content/cq:tags] = 'my:tag' */`

Den här frågan matchar indexet `cqPageLucene`, men eftersom det inte finns någon egenskapsindexregel för `jcr:content` eller `cq:tags` kontrolleras alla poster i indexet `cqPageLucene` för att avgöra en matchning när begränsningen utvärderas. Om indexet innehåller 1 miljon `cq:Page` noder kontrolleras 1 miljon poster för att fastställa resultatet.

När indexregeln cq:tags lagts till

* **cq:tags indexregel**

  ```js
  /oak:index/cqPageLucene/indexRules/cq:Page/properties/cqTags
  @name=jcr:content/cq:tags
  @propertyIndex=true
  ```

* **Fråga** i Frågebyggaren

  ```js
  type=cq:Page
  property=jcr:content/cq:tags
  property.value=myTagNamespace:myTag
  ```

* **Frågeplan**

  `[cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) jcr:content/cq:tags:my:tag where [a].[jcr:content/cq:tags] = 'my:tag' */`

Genom att lägga till indexRule för `jcr:content/cq:tags` i `cqPageLucene` index kan `cq:tags`-data lagras på ett optimerat sätt.

När en fråga med begränsningen `jcr:content/cq:tags` utförs kan indexet söka efter resultat efter värde. Det innebär att om 100 `cq:Page`-noder har `myTagNamespace:myTag` som värde returneras bara de 100 resultaten och de andra 999 000 undantas från begränsningskontrollerna, vilket förbättrar prestanda med en faktor på 10 000.

Fler frågebegränsningar minskar antalet giltiga resultatuppsättningar och optimerar frågeoptimeringen ytterligare.

Utan en extra indexregel för egenskapen `cq:tags` skulle även en fulltextfråga med en begränsning för `cq:tags` fungera dåligt eftersom resultaten från indexet skulle returnera alla fulltextmatchningar. Begränsningen för cq:tags filtreras efter den.

En annan orsak till postindexfiltrering är Access Control Lists, som ofta missas under utvecklingen. Kontrollera att frågan inte returnerar sökvägar som kanske inte är tillgängliga för användaren. Det kan göras genom bättre innehållsstruktur och relevanta sökvägsbegränsningar för frågan.

Ett praktiskt sätt att identifiera om Lucene-indexet returnerar många resultat för att returnera en liten delmängd som frågeresultat är att aktivera DEBUG-loggar för `org.apache.jackrabbit.oak.plugins.index.lucene.LucenePropertyIndex`. På så sätt kan du se hur många dokument som läses in från indexet. Antalet slutliga resultat jämfört med antalet inlästa dokument bör inte vara oproportionerligt. Mer information finns i [Loggning](/help/sites-deploying/configure-logging.md).

#### Efter distribution {#post-deployment-1}

* Övervaka `error.log` för genomgångsfrågor:

   * `*WARN* org.apache.jackrabbit.oak.spi.query.Cursors$TraversingCursor Traversed ### nodes ... consider creating an index or changing the query`

* Besök AEM [frågeprestanda](/help/sites-administering/operations-dashboard.md#query-performance) åtgärdskonsol och [Förklara](/help/sites-administering/operations-dashboard.md#explain-query) långsamma frågor som söker efter frågeplaner som inte löser frågeegenskapsbegränsningar till indexegenskapsregler.

### Identifiera stora resultatuppsättningsfrågor {#detecting-large-result-set-queries}

#### Under utveckling {#during-development-2}

Ange låga tröskelvärden för oak.queryLimitInMemory (t.ex. 1000) och oak.queryLimitReads (t.ex. 5000) och optimera den dyra frågan när du klickar på ett UnsupportedOperationException som säger&quot;Frågan läser fler än x noder..&quot;.

Genom att ange låga tröskelvärden undviker du resurskrävande frågor (dvs. inte backas upp av något index eller backas upp av mindre täckningsindex). En fråga som till exempel läser en miljon noder skulle leda till mycket IO och negativt påverka programmets prestanda. Alla frågor som inte godkänns på grund av ovanstående gränser bör därför analyseras och optimeras.

#### Efter distribution {#post-deployment-2}

* Övervaka loggarna för frågor som utlöser genomgång av stora noder eller stor minnesförbrukning i stackar: &quot;

   * `*WARN* ... java.lang.UnsupportedOperationException: The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.`
   * Optimera frågan så att du minskar antalet genomskurna noder.

* Övervaka loggarna för frågor som utlöser stor minnesförbrukning i stackar:

   * `*WARN* ... java.lang.UnsupportedOperationException: The query read more than 500000 nodes in memory. To avoid running out of memory, processing was stopped`
   * Optimera frågan så att du minskar minnesanvändningen för stacken.

I AEM 6.0-6.2 kan du justera tröskelvärdet för nodgenomgång via JVM-parametrar i AEM startskript för att förhindra att stora frågor överbelastar miljön. Rekommenderade värden är:

* `-Doak.queryLimitInMemory=500000`
* `-Doak.queryLimitReads=100000`

I AEM 6.3 är ovanstående två parametrar förkonfigurerade som standard och kan ändras via OSGi QueryEngineSettings.

Mer information finns under: [https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Slow_Queries_and_Read_Limits](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Slow_Queries_and_Read_Limits)

## Justering av frågeprestanda {#query-performance-tuning}

Motto för prestandaoptimering av frågor i AEM är:

**&quot;Ju fler begränsningar, desto bättre.&quot;**

Följande utkast rekommenderar justeringar för att säkerställa frågeprestanda. Finjustera först frågan, en mindre diskret aktivitet och finjustera sedan indexdefinitionerna om det behövs.

### Justera frågeuttryck {#adjusting-the-query-statement}

AEM har stöd för följande frågespråk:

* Frågebyggaren
* JCR-SQL2
* XPath

I följande exempel används Query Builder eftersom det är det vanligaste frågespråket som används av AEM-utvecklare, men samma principer gäller för JCR-SQL2 och XPath.

1. Lägg till en nodetypbegränsning så att frågan löses till ett befintligt Lucene-egenskapsindex.

* **Ooptimerad fråga**

  ```js
  property=jcr:content/contentType
  property.value=article-page
  ```

* **Optimerad fråga**

  ```js
  type=cq:Page
  property=jcr:content/contentType
  property.value=article-page
  ```

  Frågor som saknar en nodetype-begränsning tvingar AEM att anta nodtypen `nt:base`, som alla noder i AEM är en undertyp till, vilket resulterar i att nodtype inte begränsas.

  Om du anger `type=cq:Page` begränsas den här frågan till endast `cq:Page` noder och frågan matchas till AEM cqPageLucene, vilket begränsar resultatet till en delmängd av noder (endast `cq:Page` noder) i AEM.

1. Justera frågans nodetypbegränsning så att frågan matchar ett befintligt Lucene-egenskapsindex.

* **Ooptimerad fråga**

  ```js
  type=nt:hierarchyNode
  property=jcr:content/contentType
  property.value=article-page
  ```

* **Optimerad fråga**

  ```js
  type=cq:Page
  property=jcr:content/contentType
  property.value=article-page
  ```

  `nt:hierarchyNode` är den överordnade nodtypen för `cq:Page`. Om `jcr:content/contentType=article-page` bara tillämpas på `cq:Page`-noder via det anpassade Adobe-programmet returnerar den här frågan bara `cq:Page` noder där `jcr:content/contentType=article-page` finns. Detta flöde är dock en suboptimal begränsning eftersom:

   * Annan nod ärver från `nt:hierarchyNode` (till exempel `dam:Asset`) och lägger i onödan till uppsättningen med möjliga resultat.
   * Det finns inget AEM-angivet index för `nt:hierarchyNode`, men eftersom det finns ett angivet index för `cq:Page`.

  Om du anger `type=cq:Page` begränsas den här frågan till endast `cq:Page` noder och frågan matchas till AEM cqPageLucene, vilket begränsar resultatet till en delmängd av noder (endast cq:Page-noder) i AEM.

1. Eller justera egenskapsbegränsningarna så att frågan matchas mot ett befintligt egenskapsindex.

* **Ooptimerad fråga**

  ```js
  property=jcr:content/contentType
  property.value=article-page
  ```

* **Optimerad fråga**

  ```js
  property=jcr:content/sling:resourceType
  property.value=my-site/components/structure/article-page
  ```

  Om du ändrar egenskapsbegränsningen från `jcr:content/contentType` (ett anpassat värde) till den välkända egenskapen `sling:resourceType` kan frågan matchas till egenskapsindexet `slingResourceType` som indexerar allt innehåll med `sling:resourceType`.

  Egenskapsindex (till skillnad från Lucene-egenskapsindex) används bäst när frågan inte identifieras av nodetype, och en enskild egenskapsbegränsning dominerar resultatuppsättningen.

1. Lägg till den striktaste möjliga sökvägsbegränsningen för frågan. Använd till exempel `/content/my-site/us/en` framför `/content/my-site` eller `/content/dam` över `/`.

* **Ooptimerad fråga**

  ```js
  type=cq:Page
  path=/content
  property=jcr:content/contentType
  property.value=article-page
  ```

* **Optimerad fråga**

  ```js
  type=cq:Page
  path=/content/my-site/us/en
  property=jcr:content/contentType
  property.value=article-page
  ```

  Om du omsluter sökvägsbegränsningen från `path=/content` till `path=/content/my-site/us/en` kan indexen minska antalet indexposter som måste undersökas. När frågan kan begränsa sökvägen bra, bortom bara `/content` eller `/content/dam`, kontrollerar du att indexet har `evaluatePathRestrictions=true`.

  Obs! Om du använder `evaluatePathRestrictions` ökas indexstorleken.

1. Undvik, när det är möjligt, frågefunktioner och frågeåtgärder som `LIKE` och `fn:XXXX` när deras kostnader skalas med antalet begränsningsbaserade resultat.

* **Ooptimerad fråga**

  ```js
  type=cq:Page
  property=jcr:content/contentType
  property.operation=like
  property.value=%article%
  ```

* **Optimerad fråga**

  ```js
  type=cq:Page
  fulltext=article
  fulltext.relPath=jcr:content/contentType
  ```

  Villkoret LIKE tar lång tid att utvärdera eftersom inget index kan användas om texten börjar med ett jokertecken (&quot;%..&quot;). jcr:contains-villkoret tillåter att ett fulltextindex används och är därför att föredra. Det matchade Lucene-egenskapsindexet måste ha indexRule för `jcr:content/contentType` med `analayzed=true`.

  Det kan vara svårare att optimera att använda frågefunktioner som `fn:lowercase(..)` eftersom det inte finns snabbare motsvarigheter (utanför mer komplexa och diskreta indexanalysatorkonfigurationer). Det är bäst att identifiera andra omfångsbegränsningar för att förbättra den övergripande frågeprestandan, vilket kräver att funktionerna arbetar på den minsta möjliga uppsättningen möjliga resultat.

1. ***Den här justeringen är Query Builder-specifik och gäller inte JCR-SQL2 eller XPath.***

   Använd [Query Builder&#39; gissningssumma](/help/sites-developing/querybuilder-api.md#using-p-guesstotal-to-return-the-results) när hela resultatuppsättningen **inte** behövs omedelbart.

   * **Ooptimerad fråga**

     ```js
     type=cq:Page
     path=/content
     ```

   * **Optimerad fråga**

     ```js
     type=cq:Page
     path=/content
     p.guessTotal=100
     ```

   För fall där frågekörningen är snabb men där antalet resultat är stort är p. `guessTotal` en viktig optimering för frågor i Query Builder.

   `p.guessTotal=100` säger åt Query Builder att endast samla in de första 100 resultaten. Och om du anger en boolesk flagga som anger om det finns minst ett resultat till (men inte hur många fler, eftersom det blir långsamt om du räknar det här talet). Den här optimeringen är utmärkt för sidnumrering eller oändlig inläsning, där endast en resultatdelmängd visas stegvis.

## Justering av befintligt index {#existing-index-tuning}

1. Om den optimala frågan löses till ett egenskapsindex finns det inget kvar att göra eftersom egenskapsindex är minimalt justeringsbara.
1. Annars bör frågan matchas mot ett Lucene-egenskapsindex. Om inget index kan tolkas går du till Skapa ett index.
1. Konvertera frågan till XPath eller JCR-SQL2 efter behov.

   * **Fråga** i Frågebyggaren

     ```js
     query type=cq:Page
     path=/content/my-site/us/en
     property=jcr:content/contentType
     property.value=article-page
     orderby=@jcr:content/publishDate
     orderby.sort=desc
     ```

   * **XPath genererad från frågan i Query Builder**

     ```js
     /jcr:root/content/my-site/us/en//element(*, cq:Page)[jcr:content/@contentType = 'article-page'] order by jcr:content/@publishDate descending
     ```

1. Ange XPath (eller JCR-SQL2) till Oak Index Definition Generator på `https://oakutils.appspot.com/generate/index` så att du kan generera den optimerade definitionen av Lucene-egenskapsindex. <!-- The above URL is 404 as of April 24, 2023 -->

   **Indexdefinition för Lucene-egenskap har genererats**

   ```xml
   - evaluatePathRestrictions = true
   - compatVersion = 2
   - type = "lucene"
   - async = "async"
   - jcr:primaryType = oak:QueryIndexDefinition
       + indexRules
       + cq:Page
           + properties
           + contentType
               - name = "jcr:content/contentType"
               - propertyIndex = true
           + publishDate
               - ordered = true
               - name = "jcr:content/publishDate"
   ```

1. Sammanfoga manuellt den genererade definitionen i det befintliga Lucene-egenskapsindexet på ett additivt sätt. Var försiktig så att du inte tar bort befintliga konfigurationer eftersom de kan användas för att tillgodose andra frågor.

   1. Leta reda på det befintliga Lucene-egenskapsindex som omfattar cq:Page (med hjälp av Indexhanteraren). I det här fallet `/oak:index/cqPageLucene`.
   1. Identifiera konfigurationsförändringen mellan den optimerade indexdefinitionen (steg 4) och det befintliga indexet (/oak:index/cqPageLucene) och lägg till de saknade konfigurationerna från det optimerade indexet till den befintliga indexdefinitionen.
   1. Enligt AEM Best Practices för omindexering är det antingen en uppdatering eller omindexering i ordning, baserat på om befintligt innehåll kan påverkas av den här indexkonfigurationsändringen.

## Skapa ett nytt index {#create-a-new-index}

1. Kontrollera att frågan inte matchar ett befintligt Lucene-egenskapsindex. Om så är fallet, se avsnittet ovan om justering och befintligt index.
1. Konvertera frågan till XPath eller JCR-SQL2 efter behov.

   * **Fråga** i Frågebyggaren

     ```js
     type=myApp:Author
     property=firstName
     property.value=ira
     ```

   * **XPath genererad från frågan i Query Builder**

     ```js
     //element(*, myApp:Page)[@firstName = 'ira']
     ```

1. Ange XPath (eller JCR-SQL2) till Oak Index Definition Generator på `https://oakutils.appspot.com/generate/index` så att du kan generera den optimerade definitionen av Lucene-egenskapsindex. <!-- The above URL is 404 as of April 24, 2023 -->

   **Indexdefinition för Lucene-egenskap har genererats**

   ```xml
   - compatVersion = 2
   - type = "lucene"
   - async = "async"
   - jcr:primaryType = oak:QueryIndexDefinition
       + indexRules
       + myApp:AuthorModel
           + properties
           + firstName
               - name = "firstName"
               - propertyIndex = true
   ```

1. Distribuera den genererade indexdefinitionen för Lucene-egenskap.

   Lägg till XML-definitionen från Oak Index Definition Generator för det nya indexet i det AEM-projekt som hanterar Oak indexdefinitioner (kom ihåg att behandla Oak indexdefinitioner som kod, eftersom koden är beroende av dem).

   Distribuera och testa det nya indexet efter den vanliga AEM-utvecklingscykeln och kontrollera att frågan matchar indexvärdet och att frågan är korrekt.

   I den initiala distributionen av detta index fyller AEM i indexet med nödvändiga data.

## När är det okej med indexlösa frågor och genomgående frågor? {#when-index-less-and-traversal-queries-are-ok}

På grund av AEM flexibla innehållsarkitektur är det svårt att förutsäga och se till att genomgången av innehållsstrukturer inte förändras över tid och blir oacceptabla.

Se därför till att index uppfyller frågor, förutom om kombinationen av sökvägsbegränsning och nodetypbegränsning garanterar att **färre än 20 noder någonsin har gått igenom.**

## Frågeutvecklingsverktyg {#query-development-tools}

### Stöd för Adobe {#adobe-supported}

* **Felsökning för frågeverktyget**

   * Ett WebUI-program som används för att köra Query Builder-frågor och generera den XPath som stöds (för användning i Förklara fråga eller Oak Index Definition Generator).
   * På AEM på [/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

* **CRXDE Lite - Frågeverktyget**

   * Ett WebUI för att köra XPath- och JCR-SQL2-frågor.
   * På AEM på [/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp) > Verktyg > Fråga..

* **[Förklara fråga](/help/sites-administering/operations-dashboard.md#explain-query)**

   * En AEM Operations-kontrollpanel som ger en detaljerad förklaring (frågeplan, frågetid och antal resultat) för en given XPATH- eller JCR-SQL2-fråga.

* **[Långsamma/populära frågor](/help/sites-administering/operations-dashboard.md#query-performance)**

   * En AEM Operations-kontrollpanel med en lista över de senaste långsamma och populära frågorna som körts på AEM.

* **[Indexhanteraren](/help/sites-administering/operations-dashboard.md#the-index-manager)**

   * Ett AEM Operations WebUI-gränssnitt som visar indexen för AEM-instansen, gör det lättare att förstå vilka index som finns, kan användas som mål eller ökas.

* **[Loggar](/help/sites-administering/operations-dashboard.md#log-messages)**

   * Loggning i Query Builder

      * `DEBUG @ com.day.cq.search.impl.builder.QueryImpl`

   * Loggning av frågekörning i Oak

      * `DEBUG @ org.apache.jackrabbit.oak.query`

* **Inställningar för Apache Jackrabbit Query Engine OSGi Config**

   * OSGi-konfiguration som konfigurerar felbeteende för att gå igenom frågor.
   * På AEM på [/system/console/configMgr#org.apache.jackrabbit.oak.query.QueryEngineSettingsService](http://localhost:4502/system/console/configMgr#org.apache.jackrabbit.oak.query.QueryEngineSettingsService)

* **NodeCounter JMX Mbean**

   * JMX MBean används för att beräkna antalet noder i innehållsträd i AEM.
   * På AEM på [/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3DnodeCounter%2Ctype%3DNodeCounter](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3DnodeCounter%2Ctype%3DNodeCounter)

### Community-stöd {#community-supported}

* **Oak Index Definition Generator på`https://oakutils.appspot.com/generate/index`** <!-- The above URL is 404 as of April 24, 2023 -->

   * Generera ett optimalt Lucence-egenskapsindex från XPath- eller JCR-SQL2-frågesatser.

* **_AEM Chrome Plug-in_** <!-- For whatever reason, the URL to this extension was causing too many redirects when doing the request so it was removed entirely to get rid of the error; users can easily look up the extension in Google instead. DO NOT ADD THE URL AGAIN!-->

   * _AEM Chrome-plugin-programmet_ är ett Google Chrome-webbläsartillägg som visar loggdata per begäran, inklusive körningsfrågor och tillhörande frågeplaner, i webbläsarens konsol för utvecklingsverktyg.
   * Kräver att du installerar och aktiverar [Sling Log Tracer 1.0.2+](https://sling.apache.org/downloads.cgi) på AEM.
