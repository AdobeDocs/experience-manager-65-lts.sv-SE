---
title: Metodtips för frågor och indexering
description: Den här artikeln innehåller riktlinjer för hur du optimerar index och frågor.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
exl-id: 3ffa7c80-ce59-41cf-bb50-c6caf77d9baa
source-git-commit: 09f3d38e9f9c7f882d8b03dcf86db68cb8885a08
workflow-type: tm+mt
source-wordcount: '4196'
ht-degree: 0%

---

# Metodtips för frågor och indexering{#best-practices-for-queries-and-indexing}

Förutom övergången till Oak i AEM 6 har några viktiga förändringar gjorts i hur frågor och index hanteras. Under Jackrabbit 2 indexerades allt innehåll som standard och kunde läsas fritt. I Oak måste index skapas manuellt under noden `oak:index`. En fråga kan köras utan index, men för stora datauppsättningar kommer den att köras långsamt eller till och med avbrytas.

I den här artikeln beskrivs när index ska skapas och när de inte behövs, hur man undviker att använda frågor när de inte är nödvändiga, och hur man optimerar index och frågor så att de fungerar så optimalt som möjligt.

Läs även [Oak-dokumentationen om hur du skriver frågor och index](/help/sites-deploying/queries-and-indexing.md). Förutom att index är ett nytt koncept i AEM 6 finns det syntaktiska skillnader i Oak-frågor som måste beaktas när du migrerar kod från en tidigare AEM-installation.

## När frågor ska användas {#when-to-use-queries}

### Databas- och taxonomidesign {#repository-and-taxonomy-design}

När du utformar taxonomin för en databas måste flera faktorer beaktas. Dessa omfattar bland annat åtkomstkontroller, lokalisering, komponent och arv av sidegenskaper.

När du utformar en taxonomi som tar upp dessa problem är det också viktigt att tänka på hur&quot;genomskinlighet&quot; är i indexdesignen. I det här sammanhanget är möjligheten att gå igenom en taxonomi som gör att innehållet kan läsas på ett förutsägbart sätt baserat på dess sökväg. Detta ger ett mer prestandasystem som är enklare att underhålla än ett som kräver att man kör många frågor.

När du utformar en taxonomi är det dessutom viktigt att tänka på om det är viktigt att beställa. Om explicit ordning inte krävs och många noder på samma nivå förväntas, är det bättre att använda en oordnad nodtyp som `sling:Folder` eller `oak:Unstructured`. I de fall där beställning krävs är `nt:unstructured` och `sling:OrderedFolder` lämpligare.

### Frågor i komponenter {#queries-in-components}

Eftersom frågor kan vara en av de mer beskattningsbara åtgärder som utförs i ett AEM-system, är det en bra idé att undvika dem i dina komponenter. Om flera frågor körs varje gång en sida återges kan det ofta försämra systemets prestanda. Det finns två strategier som kan användas för att undvika att köra frågor vid återgivning av komponenter: **gå igenom noder** och **förhämta resultat**.

#### Går igenom noder {#traversing-nodes}

Om databasen är utformad på ett sådant sätt att det går att förhandsgranska platsen för de data som krävs, kan kod som hämtar dessa data från de nödvändiga sökvägarna distribueras utan att behöva köra frågor för att hitta dem.

Ett exempel på detta är att återge innehåll som passar inom en viss kategori. Ett sätt är att ordna innehållet med en kategoriegenskap som kan efterfrågas för att fylla i en komponent som visar objekt i en kategori.

Ett bättre sätt är att strukturera innehållet i en taxonomi efter kategori så att det kan hämtas manuellt.

Om innehållet till exempel lagras i en taxonomi som liknar:

```xml
/content/myUnstructuredContent/parentCategory/childCategory/contentPiece
```

Det går bara att hämta noden `/content/myUnstructuredContent/parentCategory/childCategory`, dess underordnade noder kan tolkas och användas för att återge komponenten.

När du har att göra med en liten eller homogen resultatuppsättning kan det dessutom vara snabbare att gå igenom databasen och samla ihop de noder som behövs, i stället för att skapa en fråga som returnerar samma resultatmängd. Generellt sett bör frågor undvikas där det är möjligt att göra detta.

#### Förhämtningsresultat {#prefetching-results}

Ibland tillåter inte innehållet eller kraven runt komponenten att nodgenomgång används som ett sätt att hämta nödvändiga data. I dessa fall måste de nödvändiga frågorna köras innan komponenten återges, så att optimala prestanda säkerställs för slutanvändaren.

Om de resultat som krävs för komponenten kan beräknas när den redigeras och det inte finns någon förväntad tid för att innehållet ska ändras, kan frågan köras när författaren tillämpar inställningarna i dialogrutan.

Om data eller innehåll ändras regelbundet, kan frågan köras enligt ett schema eller via en avlyssnare för uppdateringar av underliggande data. Sedan kan resultaten skrivas till en delad plats i databasen. Alla komponenter som behöver dessa data kan sedan hämta värden från den här noden utan att behöva köra en fråga vid körning.

## Frågeoptimering {#query-optimization}

När du kör en fråga som inte använder ett index loggas varningar om nodgenomgång. Skapa ett index om detta är en fråga som kommer att köras ofta. Du bör använda verktyget [Förklara fråga](/help/sites-administering/operations-dashboard.md#explain-query) för att avgöra vilket index en viss fråga använder. Om du vill ha mer information kan du aktivera DEBUG-loggning för de relevanta söknings-API:erna.

>[!NOTE]
>
>När du har ändrat en indexdefinition måste indexet återskapas (omindexeras). Beroende på storleken på indexet kan det ta en stund att slutföra detta.

När du kör komplexa frågor kan det finnas fall där frågan delas upp i flera mindre frågor och data kopplas till kod efter att uppgiften har blivit mer presterande. Rekommendationen för dessa fall är att jämföra prestandan för de två metoderna för att avgöra vilket alternativ som är bäst för det aktuella användningsfallet.

AEM tillåter skrivfrågor på ett av tre sätt:

* Via [QueryBuilder API:er](/help/sites-developing/querybuilder-api.md) (rekommenderas)
* Använda XPath (rekommenderas)
* Använda SQL2

Medan alla frågor konverteras till SQL2 innan de körs är overheadkostnaden för frågekonvertering minimal, vilket innebär att det största problemet när man väljer frågespråk är läsbarhet och komfort från utvecklingsteamet.

>[!NOTE]
>
>När du använder QueryBuilder bestäms resultatet som standard, vilket är långsammare i Oak jämfört med tidigare versioner av Jackrabbit. Du kan kompensera för detta genom att använda parametern [gissningTotal](/help/sites-developing/querybuilder-api.md#using-p-guesstotal-to-return-the-results).

### Verktyget Förklara fråga {#the-explain-query-tool}

Precis som med andra frågespråk är det första steget för att optimera en fråga att förstå hur den kommer att köras. Om du vill aktivera den här aktiviteten kan du använda verktyget [Förklara fråga](/help/sites-administering/operations-dashboard.md#explain-query) som är en del av kontrollpanelen för åtgärder. Med det här verktyget kan en fråga kopplas in och förklaras. En varning visas om frågan kommer att orsaka problem med en stor databas och körningstid samt de index som används. Verktyget kan även läsa in en lista med långsamma och populära frågor som sedan kan förklaras och optimeras.

### DEBUG-loggning för frågor {#debug-logging-for-queries}

Om du vill ha mer information om hur Oak väljer vilket index som ska användas och hur frågemotorn faktiskt kör en fråga, kan en **DEBUG**-loggningskonfiguration läggas till för följande paket:

* org.apache.jackrabbit.oak.plugins.index
* org.apache.jackrabbit.oak.query
* com.day.cq.search

Se till att du tar bort den här loggen när du är klar med felsökningen av frågan. Det brukar generera en stor mängd aktivitet och kan till slut fylla upp disken med loggfiler.

Mer information om hur du gör detta finns i [Loggningsdokumentationen](/help/sites-deploying/configure-logging.md).

### Indexstatistik {#index-statistics}

Lucene registrerar en JMX-böna som ger information om indexerat innehåll, inklusive storlek och antal dokument som finns i varje index.

Du kan nå den via JMX-konsolen på `https://server:port/system/console/jmx`

När du har loggat in på JMX-konsolen söker du efter **Lucene-indexstatistik** för att hitta den. Annan indexstatistik finns i **IndexStats** MBean.

För frågestatistik kan du titta på MBean med namnet **Oak Query Statistics**.

Om du vill gå in i indexen med ett verktyg som [Luke](https://code.google.com/archive/p/luke/) måste du använda Oak-konsolen för att dumpa indexet från `NodeStore` till en filsystemkatalog. Instruktioner om hur du gör detta finns i [Lucene-dokumentationen](https://jackrabbit.apache.org/oak/docs/query/lucene.html).

Du kan också extrahera indexvärdena i systemet i JSON-format. Du måste ha åtkomst till `https://server:port/oak:index.tidy.-1.json` för att kunna göra detta

### Frågegränser {#query-limits}

**Under utveckling**

Ange låga tröskelvärden för `oak.queryLimitInMemory` (till exempel 10000) och Oak. `queryLimitReads` (till exempel 5000) och optimera den dyra frågan när du klickar på ett UnsupportedOperationException som säger&quot;Frågan läser fler än x noder..&quot;

Detta hjälper till att undvika resurskrävande frågor (dvs. som inte backas upp av något index eller backas upp av mindre täckande index). En fråga som till exempel läser 1 miljon noder skulle leda till ökad I/O och negativt påverka programmets totala prestanda. Alla frågor som misslyckas på grund av att gränserna överskrids bör analyseras och optimeras.

#### **Efter distribution** {#post-deployment}

* Övervaka loggarna för frågor som utlöser genomgång av stora noder eller stor minnesförbrukning i stackar: &quot;

   * `*WARN* ... java.lang.UnsupportedOperationException: The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.`
   * Optimera frågan för att minska antalet genomgående noder

* Övervaka loggarna för frågor som utlöser stor minnesförbrukning i stackar:

   * `*WARN* ... java.lang.UnsupportedOperationException: The query read more than 500000 nodes in memory. To avoid running out of memory, processing was stopped`
   * Optimera frågan för att minska heap-minnesanvändningen

I AEM 6.0-6.2 kan du justera tröskelvärdet för nodgenomgång via JVM-parametrar i AEM startskript för att förhindra att stora frågor överbelastar miljön.

Rekommenderade värden är:

* `-Doak.queryLimitInMemory=500000`
* `-Doak.queryLimitReads=100000`

Från och med AEM 6.3 är de två ovanstående parametrarna förkonfigurerade i körklart läge och kan behållas via OSGi QueryEngineSettings.

Mer information finns under: [https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Slow_Queries_and_Read_Limits](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Slow_Queries_and_Read_Limits)

## Tips om hur du skapar effektiva index {#tips-for-creating-efficient-indexes}

### Ska jag skapa ett index? {#should-i-create-an-index}

Den första frågan som ska ställas när index skapas eller optimeras är om de behövs för en viss situation. Om du bara ska köra frågan en gång eller endast då och då och vid låg belastning för systemet genom en gruppbearbetning, är det kanske bättre att inte skapa ett index alls.

När du har skapat ett index måste indexvärdet också uppdateras varje gång indexerade data uppdateras. Eftersom detta påverkar systemets prestanda bör index endast skapas när de behövs.

Index är bara användbara om de data som finns i indexet är unika nog att motivera det. Tänk på ett index i en bok och de ämnen den omfattar. När du indexerar en uppsättning ämnen i en text kommer det oftast att finnas hundratals eller tusentals poster, vilket gör att du snabbt kan gå till en delmängd av sidor och snabbt hitta den information du söker efter. Om indexet bara har två eller tre poster, där var och en pekar på flera hundra sidor, skulle indexet inte vara användbart. Samma sak gäller för databasindex. Om det bara finns ett par unika värden är indexet inte användbart. Med andra ord kan ett index också bli för stort för att vara användbart. Mer information om indexstatistik finns i [Indexstatistik](/help/sites-deploying/best-practices-for-queries-and-indexing.md#index-statistics) ovan.

### Lucene eller Property Index? {#lucene-or-property-indexes}

Lucene-index introducerades i Oak 1.0.9 och innehåller kraftfulla optimeringar av egenskapsindexen som introducerades i samband med den första starten av AEM 6. När du bestämmer dig för att använda Lucene-index eller egenskapsindex ska du ta hänsyn till följande:

* Lucene-index har många fler funktioner än egenskapsindex. Ett egenskapsindex kan till exempel bara indexera en enda egenskap, medan ett Lucene-index kan innehålla många. Mer information om alla funktioner som är tillgängliga i Lucene-index finns i [dokumentationen](https://jackrabbit.apache.org/oak/docs/query/lucene.html).
* Lucene-index är asynkrona. Detta ger en avsevärd prestandaökning men kan även medföra en fördröjning mellan när data skrivs till databasen och när indexet uppdateras. Om det är viktigt att frågorna returnerar 100 % korrekta resultat krävs ett egenskapsindex.
* Eftersom Lucene-index är asynkront kan det inte framtvinga unika begränsningar. Om detta är obligatoriskt måste ett egenskapsindex sättas in.

Vanligtvis rekommenderar vi att du använder Lucene-index, såvida det inte finns ett övertygande behov av att använda egenskapsindex så att du kan dra nytta av bättre prestanda och flexibilitet.

### Designöverväganden {#design-considerations}

I Oak-dokumentationen för Lucene-index finns flera saker att tänka på när du designar index:

* Om frågan använder olika sökvägsbegränsningar använder du `evaluatePathRestrictions`. Detta gör att frågan kan returnera delmängden av resultaten under den angivna sökvägen och sedan filtrera dem baserat på frågan. Annars söker frågan efter alla resultat som matchar frågeparametrarna i databasen och filtrerar dem sedan baserat på sökvägen.
* Om frågan använder sortering har du en explicit egenskapsdefinition för den sorterade egenskapen och ställer in `ordered` på `true` för den. Detta gör att resultaten kan sorteras som sådana i indexet och sparas på kostsamma sorteringsåtgärder vid körning av frågor.

* Placera bara det som behövs i indexet. Om du lägger till funktioner eller egenskaper som inte behövs växer indexet och prestandan blir långsam.
* I ett egenskapsindex kan det vara till hjälp att minska storleken på ett index om du har ett unikt egenskapsnamn, men för Lucene-index bör du använda `nodeTypes` och `mixins` för att få sammanhängande index. Att fråga en specifik `nodeType` eller `mixin` kommer att vara mer prestandaförbättrande än att fråga `nt:base`. När du använder den här metoden definierar du `indexRules` för `nodeTypes` i fråga.

* Om dina frågor bara körs under vissa sökvägar skapar du dessa index under dessa sökvägar. Index behöver inte finnas i databasens rot.
* Använd ett enda index när alla egenskaper som indexeras är relaterade så att Lucene kan utvärdera så många egenskapsbegränsningar som möjligt internt. Dessutom kommer en fråga endast att använda ett index, även när en koppling görs.

### CopyOnRead {#copyonread}

Om `NodeStore` lagras på fjärrbasis kan ett alternativ med namnet `CopyOnRead` aktiveras. Alternativet gör att fjärrindexet skrivs till det lokala filsystemet när det läses. Detta kan förbättra prestanda för frågor som ofta körs mot dessa fjärrindex.

Detta kan konfigureras i OSGi-konsolen under tjänsten **LuceneIndexProvider** och aktiveras som standard från och med Oak 1.0.13.

### Tar bort index {#removing-indexes}

När du tar bort ett index rekommenderar vi alltid att du tillfälligt inaktiverar indexet genom att ange egenskapen `type` till `disabled` och sedan testar för att se till att programmet fungerar korrekt innan du tar bort det. Ett index uppdateras inte när det är inaktiverat, så det kanske inte har rätt innehåll om det är återaktiverat och kan behöva indexeras om.

När du har tagit bort ett egenskapsindex för en tarMK-instans måste komprimeringen köras för att frigöra diskutrymme som användes. För Lucene-index finns det faktiska indexinnehållet i BlobStore, vilket innebär att en skräpinsamling för datalagring krävs.

När du tar bort ett index för en MongoDB-instans är borttagningskostnaden proportionerlig till antalet noder i indexet. Eftersom det kan uppstå problem när du tar bort ett stort index rekommenderar vi att du inaktiverar indexet och tar bort det endast under en underhållsperiod, med ett verktyg som **oak-mongo.js**. Observera att den här metoden inte bör användas för vanligt nodinnehåll eftersom det kan medföra inkonsekvenser i data.

>[!NOTE]
>
>Mer information om oak-mongo.js finns i avsnittet [Kommandoradsverktyg](https://jackrabbit.apache.org/oak/docs/command_line.html) i Oak-dokumentationen.

### JCR-frågecheblad {#jcrquerycheatsheet}

[JCR-frågechebladet](assets/JCR_query_cheatsheet-v1.1.pdf) kan hämtas och användas som referens under utvecklingen, vilket ger stöd för att skapa effektiva JCR-frågor och indexdefinitioner. Den innehåller exempelfrågor för QueryBuilder, XPath och SQL-2, som omfattar flera scenarier som beter sig på olika sätt när det gäller frågeprestanda. Här finns också rekommendationer för hur du skapar eller anpassar Oak-index. Innehållet i detta Cheat Sheet gäller AEM 6.5, AEM 6.5 LTS och AEM as a Cloud Service.

## Omindexering {#re-indexing}

I det här avsnittet beskrivs de **endast** godtagbara anledningarna att indexera om Oak-index.

Om du startar omindexeringar av Oak-index ändrar **inte** beteendet eller löser problem och ökar inläsningen i onödan på AEM.

Omindexering av Oak-index bör undvikas såvida inte detta anges i nedanstående tabeller.

>[!NOTE]
>
>Innan du läser igenom tabellerna nedan för att avgöra om omindexering är användbar bör du **alltid** verifiera:
>
>* frågan är korrekt
>* frågan löses till det förväntade indexvärdet (med [Förklara fråga](/help/sites-administering/operations-dashboard.md#diagnosis-tools))
>* indexeringsprocessen har slutförts
>

### Konfigurationsändringar för Oak-index {#oak-index-configuration-changes}

De enda tillåtna felskrivningsvillkoren för omindexering av Oak-index är om konfigurationen av ett Oak-index har ändrats.

*Omindexering bör alltid ske med vederbörlig hänsyn till hur den påverkar AEM allmänna prestanda och utföras under perioder med låg aktivitet eller underhållsperioder.*

Här följer information om möjliga problem tillsammans med lösningar:

* [Ändring av egenskapsindexdefinition](#property-index-definition-change)
* [Ändring av Lucene-indexdefinition](#lucene-index-definition-change)

#### Ändring av egenskapsindexdefinition {#property-index-definition-change}

* Gäller för/om:

   * Alla Oak-versioner
   * Endast [egenskapsindex](https://jackrabbit.apache.org/oak/docs/query/property-index.html)

* Symtom:

   * Noder som fanns före egenskapsindexets definitionsuppdatering saknas i resultaten

* Verifiera:

   * Kontrollera om saknade noder skapades/ändrades innan den uppdaterade indexdefinitionen distribuerades.
   * Verifiera egenskaperna `jcr:created` eller `jcr:lastModified` för noder som saknas mot indexets ändringsdatum

* Så här löser du:

   * [Indexera om &#x200B;](/help/sites-deploying/best-practices-for-queries-and-indexing.md#how-to-re-index) lucenindexet
   * Du kan även trycka (utföra en benign write-åtgärd) på de noder som saknas

      * Kräver manuell beröring eller anpassad kod
      * Kräver att uppsättningen saknade noder är känd
      * Kräver att egenskaper på noden ändras

#### Ändring av Lucene-indexdefinition {#lucene-index-definition-change}

* Gäller för/om:

   * Alla Oak-versioner
   * Endast [lucene-index](https://jackrabbit.apache.org/oak/docs/query/lucene.html)

* Symtom:

   * Lucene-indexet innehåller inte förväntade resultat
   * Frågeresultat återspeglar inte det förväntade beteendet för indexdefinitionen
   * Frågeplanen rapporterar inte förväntade utdata baserat på indexdefinition

* Verifiera:

   * Kontrollera att indexdefinitionen ändrades med Lucene Index-statistik, JMX Mbean (LuceneIndex), metod `diffStoredIndexDefinition`.

* Så här löser du:

   * Oak-versioner före 1.6:

      * [Indexera om &#x200B;](#how-to-re-index) lucenindexet

   * Oak version 1.6+

      * Om befintligt innehåll inte påverkas av ändringarna behövs bara en uppdatering

         * [Uppdatera](https://jackrabbit.apache.org/oak/docs/query/lucene.html#stored-index-definition) lucene-indexet genom att ange [oak:queryIndexDefinition]@refresh=true

      * Annars [indexerar om](#how-to-re-index) lucene-indexet

         * Obs! Indexläget från den senaste fungerande omindexeringen (eller den inledande indexeringen) används tills en ny omindexering aktiveras

### Felsökning och exceptionella situationer {#erring-and-exceptional-situations}

I följande tabell beskrivs den enda godtagbara felsökningen och exceptionella situationer där omindexering av Oak-index löser problemet.

Om ett problem uppstår med AEM som inte matchar villkoren som beskrivs nedan bör du **inte** indexera om index, eftersom problemet inte kan lösas.

Här följer information om möjliga problem tillsammans med lösningar:

* [Lucene-indexbinärfilen saknas](#lucene-index-binary-is-missing)
* [Lucene-indexbinärfilen är skadad](#lucene-index-binary-is-corrupt)

#### Lucene-indexbinärfilen saknas {#lucene-index-binary-is-missing}

* Gäller för/om:

   * Alla Oak-versioner
   * Endast [lucene-index](https://jackrabbit.apache.org/oak/docs/query/lucene.html)

* Symtom:

   * Lucene-indexet innehåller inte förväntade resultat

* Verifiera:

   * Felloggfilen innehåller ett undantag som anger att en binär fil saknas i Lucene-indexet

* Så här löser du:

   * Genomför en genomgång av databasen, till exempel:

     [http://localhost:4502/system/console/repositorycheck](http://localhost:4502/system/console/repositorycheck)

     genom att gå igenom databasen avgör om andra binära filer (förutom lucene-filer) saknas

   * Om andra binärfiler än lucene-index saknas kan du återställa från en säkerhetskopia
   * I annat fall [omindexera](#how-to-re-index) *alla* lucene-index
   * Obs!

     Det här villkoret indikerar ett felkonfigurerat datalager som kan resultera i att ett binärt värde (till exempel binära resurser) saknas.

     I det här fallet återställer du till den senast fungerande versionen av databasen för att återställa alla binära filer som saknas.

#### Lucene-indexbinärfilen är skadad {#lucene-index-binary-is-corrupt}

* Gäller för/om:

   * Alla Oak-versioner
   * Endast [lucene-index](https://jackrabbit.apache.org/oak/docs/query/lucene.html)

* Symtom:

   * Lucene-indexet innehåller inte förväntade resultat

* Verifiera:

   * `AsyncIndexUpdate` (var femte sekund) kommer att misslyckas med ett undantag i error.log:

     `...a Lucene index file is corrupt...`

* Så här löser du:

   * Ta bort den lokala kopian av lucene-indexet

      1. Stoppa AEM
      1. Ta bort den lokala kopian av lucene-indexet vid `crx-quickstart/repository/index`
      1. Starta om AEM

   * Om detta inte löser problemet och `AsyncIndexUpdate` undantag kvarstår:

      1. [Indexera om](#how-to-re-index) felindexet
      1. Skicka även en anmälan till [Adobe Support](https://helpx.adobe.com/se/support.html)

### Indexera om {#how-to-re-index}

>[!NOTE]
>
>I AEM 6.5 LTS är [oak-run.jar den ENDA metoden &#x200B;](/help/sites-deploying/indexing-via-the-oak-run-jar.md#reindexingapproachdecisiontree) som stöds för omindexering i MongoMK- eller RDBMK-databaser.

#### Indexerar om egenskapsindex {#re-indexing-property-indexes}

* Använd [oak-run.jar](/help/sites-deploying/oak-run-indexing-usecases.md#usecase3reindexing) för att indexera om egenskapsindexet
* Ange egenskapen async-reindex till true för egenskapsindexet

   * `[oak:queryIndexDefinition]@reindex-async=true`

* Indexera om egenskapsindexet asynkront med webbkonsolen via **PropertyIndexAsyncReindex** MBean;

  till exempel

  [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DPropertyIndexAsyncReindex](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DPropertyIndexAsyncReindex)

#### Indexerar om Lucene-egenskapsindex {#re-indexing-lucene-property-indexes}

* Använd [oak-run.jar om du vill indexera &#x200B;](/help/sites-deploying/oak-run-indexing-usecases.md#usecase3reindexing) om Lucene-egenskapsindexet.
* Ställ in egenskapen async-reindex på true i egenskapsindexet lucene

   * `[oak:queryIndexDefinition]@reindex-async=true`

>[!NOTE]
>
>I det föregående avsnittet sammanfattas och bildrutas riktlinjerna för omindexering av Oak från [dokumentationen för Apache Oak](https://jackrabbit.apache.org/oak/docs/query/indexing.html#reindexing) i AEM-sammanhang.

### Textförextrahering av binärfiler {#text-pre-extraction-of-binaries}

Textförextrahering är processen att extrahera och bearbeta text från binärfiler, direkt från datalagret via en isolerad process, och direkt exponera den extraherade texten för efterföljande omindexeringar av Oak-index.

* Oak textpreextrahering rekommenderas för omindexering/indexering av Lucene-index i databaser med stora volymer filer (binärfiler) som innehåller extraherbar text (t.ex. PDF-filer, Word-dokument, PPT-filer, TXT och så vidare) som är kvalificerade för fulltextsökning via distribuerade Oak-index, t.ex. `/oak:index/damAssetLucene`.
* Förextrahering av text ger endast fördelar vid omindexering/indexering av Lucene-index och NOT Oak-egenskapsindex, eftersom egenskapsindex inte extraherar text från binärfiler.
* Textförextraheringen har stor positiv effekt när du omindexerar text i fulltext till stora binärfiler (PDF, Doc, TXT och så vidare), medan en databas med bilder inte har samma effektivitet eftersom bilder inte innehåller text som kan extraheras.
* Textförextrahering utför extraheringen av fulltextsöksrelaterad text på ett extra effektivt sätt och exponerar den för Oak-processen för omindexering/indexering på ett sätt som är extra effektivt att använda.

#### När kan förextrahering av text användas? {#when-can-text-pre-extraction-be-used}

Indexerar om ett **befintligt**-indexvärde med binär extrahering aktiverad

* Omindexering av bearbetningen av **all**-kandidatinnehåll i databasen. När binärfilerna som ska extraheras är många eller komplexa läggs en ökad arbetsbörda vid extraheringen av fullständig text till i AEM. Textextrahering flyttar det&quot;beräkningsmässigt kostsamma arbetet&quot; med textredigering till en isolerad process som direkt använder AEM datalager och undviker problem med overhead och resurser i AEM.

Stöd för distribution av ett **nytt**-lucenindex till AEM med binär extrahering aktiverad

* När ett nytt index (med binär extrahering aktiverad) distribueras till AEM indexerar Oak automatiskt allt kandidatinnehåll vid nästa asynkrona fulltextindexkörning. Av samma skäl som beskrivs i omindexering ovan kan detta resultera i en onödig belastning på AEM.

#### När kan förextrahering av text INTE användas? {#when-can-text-pre-extraction-not-be-used}

Textförextrahering kan inte användas för nytt innehåll som läggs till i databasen och är inte heller nödvändigt.

Nytt innehåll läggs till i databasen naturligt och inkrementellt efter den asynkrona fulltextindexeringsprocessen (som standard var femte sekund).

Om AEM används normalt, till exempel när Assets överförs via webbgränssnittet eller Assets programmatiska import, indexeras det nya binära innehållet automatiskt och stegvis. Eftersom datamängden är inkrementell och relativt liten (ungefär den datamängd som kan sparas i databasen på 5 sekunder) kan AEM utföra fulltextextraheringen från binärfilerna under indexeringen utan att påverka den övergripande systemprestandan.

#### Förutsättningar för att använda förextrahering av text {#prerequisites-to-using-text-pre-extraction}

* Du kommer att indexera om ett indexvärde som utför binär extrahering i heltext eller distribuera ett nytt index som kommer att innehålla binära fulltextindex
* Innehållet (binärfiler) som texten ska förextraheras från måste finnas i databasen
* Ett underhållsfönster där CSV-filen och den slutliga omindexeringen genereras
* Oak-version: 1.0.18+, 1.2.3+
* [oak-run.jar](https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-run/)version 1.7.4+
* En mapp/resurs i filsystemet för lagring av extraherad text som är tillgänglig från indexerade AEM-instanser

   * OSGi-konfigurationen för förextrahering av text kräver en filsystemsökväg till de extraherade textfilerna, så de måste vara tillgängliga direkt från AEM-instansen (lokal enhet eller filresursmontering)

#### Så här utför du förextrahering av text {#how-to-perform-text-pre-extraction}

>[!NOTE]
>
>***Kommandona oak-run.jar som beskrivs nedan räknas upp fullständigt på [https://jackrabbit.apache.org/oak/docs/query/pre-extract-text.html](https://jackrabbit.apache.org/oak/docs/query/pre-extract-text.html)***
>
>Diagrammet och stegen nedan beskriver och kompletterar de tekniska textförextraheringsstegen som beskrivs i dokumentationen för Apache Oak.

![Processflöde för förextrahering av text](assets/chlimage_1-139.png)

**Generera en lista över innehåll som ska extraheras i förväg**

*Kör steg 1(a-b) under en underhållsperiod/en period med låg användning när nodarkivet gås igenom under den här åtgärden, vilket kan medföra betydande belastning på systemet.*

1a. Kör `oak-run.jar --generate` för att skapa en lista över noder som har sin text extraherad.

1b. Lista över noder (1a) lagras i filsystemet som en CSV-fil

Hela nodarkivet gås igenom (enligt sökvägarna i kommandot ekakning) varje gång `--generate` körs och en **ny** CSV-fil skapas. CSV-filen återanvänds **inte** mellan diskreta exekveringar av textförextraheringsprocessen (steg 1-2).

**Förextrahera text till filsystemet**

*Steg 2(a-c) kan köras under en normal åtgärd av AEM, om det bara interagerar med datalagret.*

2a. Kör `oak-run.jar --tika` för att extrahera text för binära noder som räknas upp i CSV-filen som genereras i (1b)

2b. Den process som initieras i (2a) får direkt åtkomst till binära noder som definieras i CSV-filen i datalagret och extraherar text.

2c. Extraherad text lagras i ett filsystem i ett format som kan användas i Oak omindexeringsprocess (3a)

Förextraherad text identifieras i CSV-filen av det binära fingeravtrycket. Om den binära filen är densamma kan samma extraherade text användas i alla AEM-instanser. Eftersom AEM Publish vanligtvis är en delmängd av AEM Author kan den förextraherade texten från AEM Author ofta användas för att indexera om AEM Publish också (förutsatt att AEM Publish har tillgång till de extraherade textfilerna i filsystemet).

Förutextraherad text kan läggas till stegvis över tid. Extrahering av text i förväg hoppar över extrahering för tidigare extraherade binärfiler, så det är bäst att behålla den extraherade texten om omindexering måste ske igen i framtiden (förutsatt att det extraherade innehållet inte är oöverkomligt stort). Om den är det, bör du utvärdera om innehållet i mellanrummet ska zippa, eftersom texten komprimeras väl).

**Indexera om Oak-index, hämta fulltext från extraherade textfiler**

*Kör omindexering (steg 3a-b) under en underhålls-/låganvändningsperiod när nodarkivet gås igenom under den här åtgärden, vilket kan medföra betydande belastning på systemet.*

3a. [Indexera om](#how-to-re-index) av Lucene-index anropas i AEM.

3b. Apache Jackrabbit Oak DataStore PreExtractTextProvider OSGi-konfigurationen (som är konfigurerad att peka på den extraherade texten via en filsystemsökväg) instruerar Oak att hämta fulltexttext från de extraherade filerna och undviker att direkt slå och bearbeta data som lagras i databasen.
