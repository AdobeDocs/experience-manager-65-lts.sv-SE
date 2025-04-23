---
title: Oak-run.jar - Exempel på indexering
description: Lär dig mer om de olika användarexemplen för att utföra indexering med verktyget Oak-kör.
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
noindex: true
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
hide: true
hidefromtoc: true
exl-id: a7a8a20a-e513-43df-80b7-1e6daf957f20
source-git-commit: 6b5e576debcd3351e15837727d2bc777b0e0c6f2
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 0%

---

# Oak-run.jar - Exempel på indexering{#oak-run-jar-indexing-use-cases}

Oak-körning stöder indexering av användningsfall på kommandoraden utan att dessa användningsfall behöver ordnas via AEM JMX-konsol.

De största fördelarna med att använda indexkommandot oak-run.jar för att hantera Oak-index är:

1. Oak-kommandot index har en ny uppsättning indexeringsverktyg sedan AEM 6.4.
1. Oak-körning minskar tiden för omindexering vilket minskar omindexeringstiden i större databaser.
1. Oak-körning minskar resursförbrukningen vid omindexering i AEM, vilket ger bättre systemprestanda.
1. Oak-runtime ger omindexering utan band, vilket ger stöd för situationer där produktionen måste vara tillgänglig och klarar inte underhåll eller driftavbrott som annars skulle behövas för omindexering.

I avsnitten nedan finns exempelkommandon. Oak-kommandot för att köra index stöder alla NodeStore- och BlobStore-inställningar. Exemplen nedan handlar om inställningar som har FileDataStore och SegmentNodeStore.

## Användningsfall 1 - Kontroll av indexöverensstämmelse {#usercase1indexconsistencycheck}

Det här är ett användningsexempel som rör skadade index. Ibland gick det inte att avgöra vilken av indexen som är skadad. Därför har Adobe tagit fram verktyg som:

1. Utför konsekvenskontroller av index för alla index och tillhandahåller en rapport om vilka index som är giltiga och vilka som inte är giltiga.
1. Verktygen är användbara även om AEM inte är tillgängligt.
1. Den är lätt att använda.

Kontroll av skadade index kan utföras med åtgärden `--index-consistency-check`:

```shell
java -jar oak-run*.jar index --fds-path=/path/to/datastore  /path/to/segmentstore/ --index-consistency-check
```

Detta genererar en rapport i `indexing-result/index-consistency-check-report.txt`. Nedan finns ett exempel på en rapport:

```
Valid indexes :
        - /content/oak:index/enablementResourceName
        - /oak:index/cqProjectLucene
        - /oak:index/cqTagLucene
        - /oak:index/lucene
        - /oak:index/ntBaseLucene
        - /oak:index/socialLucene
    Invalid indexes :
        - /oak:index/atDamIndex
        - /oak:index/atIndex
        - /oak:index/cqPageLucene
        - /oak:index/damAssetLucene
        - /oak:index/groups
        - /oak:index/slingeventJob
        - /oak:index/users
        - /oak:index/workflowDataLucene
    Ignored indexes as these are not of type lucene:
        - /oak:index/acPrincipalName
        - /oak:index/active
```

### Fördelar {#uc1benefits}

Den här verktygen kan nu användas av supporten och systemadministratören för att snabbt avgöra vilka index som är skadade och sedan indexera om dem.

## Användningsfall 2 - Indexstatistik {#usecase2indexstatistics}

För diagnostisering av vissa av fallen kring frågeprestanda behövde Adobe ofta en befintlig indexdefinition, indexrelaterad statistik från kundkonfigurationen. Hittills har denna information spridits över flera resurser. För att underlätta felsökningen har Adobe skapat verktyg som gör att

1. Dumpa alla indexdefinitioner som finns i systemet i en enda JSON-fil.

1. Dumpa viktig statistik från befintliga index,

1. Dumpa indexinnehåll för offlineanalys.

1. Kan användas även om AEM inte är tillgängligt

Ovanstående åtgärder kan nu utföras med följande kommandon för åtgärdsindex:

* `--index-info` - Samlar in och dumpar diverse statistik som hör till indexen

* `--index-definitions` - Samlar in och dumpar indexdefinitioner

* `--index-dump` - Dumpar indexinnehåll

Nedan visas ett exempel på hur kommandona fungerar i praktiken:

```shell
java -jar oak-run*.jar index --fds-path=/path/to/datastore  /path/to/segmentstore/ --index-info --index-definitions --index-dump
```

Rapporterna genereras i `indexing-result/index-info.txt` och `indexing-result/index-definitions.json`

Dessutom tillhandahålls samma information via webbkonsolen och skulle ingå i konfigurationsdumpens zip. De kan nås på följande plats:

`https://serverhost:serverport/system/console/status-oak-index-defn`

### Fördelar {#uc2benefits}

Med det här verktyget kan du snabbt samla in all information som behövs för indexering eller frågeproblem och minska tiden för extrahering av informationen.

## Användningsfall 3 - omindexering {#usecase3reindexing}

Beroende på [scenarierna](https://jackrabbit.apache.org/oak/docs/query/indexing.html#reindexing) kan omindexering behöva utföras. Omindexeringen görs för närvarande genom att flaggan `reindex` anges till `true` i indexdefinitionsnoden med hjälp av CRXDE eller med hjälp av användargränssnittet i Indexhanteraren. När flaggan har angetts utförs omindexering asynkront.

Några punkter att tänka på när det gäller omindexering:

* Omindexering är mycket långsammare för `DocumentNodeStore`-uppsättningar jämfört med `SegmentNodeStore`-uppsättningar där allt innehåll är lokalt.

* När den aktuella designen omindexeras blockeras den asynkrona indexeraren och alla andra asynkrona index blir inaktuella och uppdateras inte under indexeringen. På grund av detta kanske användarna inte ser aktuella resultat om systemet används.
* Omindexering innebär att hela databasen gås igenom vilket kan belasta AEM-installationen och därmed påverka slutanvändarnas upplevelse.
* Om en `DocumentNodeStore`-installation där omindexering kan ta lång tid, och anslutningen till Mongo-databasen misslyckas under åtgärden, måste indexeringen startas om från början.

* Ibland kan omindexering ta lång tid på grund av textrahering. Detta gäller för inställningar som har många PDF-filer, där den tid som går åt för textrahering kan påverka indexeringstiden.

För att uppnå dessa mål har indexverktygen för ekningskörning stöd för olika sätt för omindexering som kan användas efter behov. Indexkommandot för ekning ger följande fördelar:

* **omindexering utanför band** - omindexering vid ekning kan göras separat från en AEM-konfiguration som körs och minimerar därmed påverkan på den AEM-instans som används.

* **omindexering utanför intervallet** - Omindexeringen utförs utan att indexeringen påverkas. Detta innebär att den asynkrona indexeraren kan fortsätta att indexera andra index.

* **Förenklad omindexering för DocumentNodeStore-installationer** - För `DocumentNodeStore`-installationer kan omindexering göras med ett enda kommando som ser till att omindexering görs på det optimala sättet.

* **Stöder uppdatering av indexdefinitioner och introduktion av nya indexdefinitioner**

### Indexera om - DocumentNodeStore {#reindexdocumentnodestore}

För `DocumentNodeStore`-installationer kan omindexering göras med ett enda ekupkommando:

```shell
java -jar oak-run*.jar index --reindex --index-paths=/oak:index/lucene --read-write --fds-path=/path/to/datastore mongodb://server:port/aem
```

Detta ger följande fördelar

* Minimal påverkan på körning av AEM-instanser. De flesta läsningar kan göras från sekundära servrar och körning av AEM-cacher påverkas inte negativt på grund av all den genomgång som krävs för omindexering.
* Användare kan också tillhandahålla en JSON för ett nytt eller uppdaterat index med alternativet `--index-definitions-file`.

### Indexera om - SegmentNodeStore {#reindexsegmentnodestore}

För `SegmentNodeStore`-installationer kan omindexering göras på något av följande sätt:

#### Online Reindex - SegmentNodeStore {#onlinereindexsegmentnodestore}

Följ det etablerade sättet där omindexering sker genom att ange flaggan `reindex`.

#### Online Reindex - SegmentNodeStore - AEM-instansen körs {#onlinereindexsegmentnodestoretheaeminstanceisrunning}

För `SegmentNodeStore`-installationer kan bara en process få åtkomst till segmentfiler i läs- och skrivläge. På grund av detta kräver vissa åtgärder vid körning av ekning att ytterligare manuella åtgärder vidtas.

Detta skulle innebära följande:

1. Stegtext
1. Anslut `oak-run` till samma databas som används av AEM i skrivskyddat läge och utför indexering. Ett exempel på hur man uppnår detta:

   ```shell
   java -jar oak-run-1.7.6.jar index --fds-path=/Users/dhasler/dev/cq/quickstart/target/crx-quickstart/repository/datastore/ --checkpoint 26b7da38-a699-45b2-82fb-73aa2f9af0e2 --reindex --index-paths=/oak:index/lucene /Users/dhasler/dev/cq/quickstart/target/crx-quickstart/repository/segmentstore/
   ```

1. Importera slutligen de skapade indexfilerna som en åtgärd av typen `IndexerMBean#importIndex` från sökvägen där körningen av omvänd körning sparade indexeringsfilerna efter att ovanstående kommando körts.

I det här fallet behöver du inte stoppa AEM-servern eller etablera en ny instans. När indexering innebär att hela databasen genomgås, ökar dock I/O-belastningen på installationen, vilket påverkar körningens prestanda negativt.

#### Online Reindex - SegmentNodeStore - AEM-instansen är avstängd {#onlinereindexsegmentnodestoreaeminstanceisdown}

För `SegmentNodeStore`-installationer kan omindexering göras med ett enda ekupkommando. AEM-instansen måste dock avslutas.

Du kan aktivera omindexering med följande kommando:

```shell
java -jar oak-run*.jar index --reindex --index-paths=/oak:index/lucene --read-write --fds-path=/path/to/datastore  /path/to/segmentstore/
```

Skillnaden mellan det här sättet och det som förklaras ovan är att skapande av kontrollpunkter och indeximport sker automatiskt. Nackdelen är att AEM måste vara nere under processen.

#### Out of Band Reindex - SegmentNodeStore {#outofbandreindexsegmentnodestore}

I det här fallet kan du indexera om en klonad installation för att minimera påverkan på den AEM-instans som körs:

1. Skapa en kontrollpunkt genom en JMX-åtgärd. Du kan göra detta genom att gå till [JMX-konsolen](/help/sites-administering/jmx-console.md) och söka efter `CheckpointManager`. Klicka sedan på åtgärden **createCheckpoint(long p1)** med ett högt värde för att förfalla i sekunder (till exempel **2592000**).
1. Kopiera mappen `crx-quickstart` till en ny dator
1. Utför omindexering med hjälp av indexkommandot för ekvering

1. Kopiera de genererade indexfilerna till AEM-servern

1. Importera indexfilerna via JMX.

I det här fallet antas det att datalagret är tillgängligt på en annan instans, vilket kanske inte är möjligt om `FileDataStore` placeras i en molnbaserad lagringslösning som EBS. Detta utesluter scenariot där `FileDataStore` också klonas. Om indexdefinitionen inte utför fulltextindexering krävs ingen åtkomst till `DataStore`.

## Användningsfall 4 - Uppdaterar indexdefinitioner {#usecase4updatingindexdefinitions}

För närvarande kan du skicka ändringar av indexdefinitioner med hjälp av paketet [Kontrollera index](https://adobe-consulting-services.github.io/acs-aem-commons/features/ensure-oak-index/index.html) för ACS. Detta gör det möjligt att leverera indexdefinitioner via innehållspaket som senare kräver omindexering genom att ange flaggan `reindex` till `true`.

Detta fungerar bra för mindre installationer där omindexering inte tar lång tid. För stora databaser sker dock omindexering på betydligt längre tid. I sådana fall kan vi nu använda indexverktygen för ekakning.

Oak-run har nu stöd för att tillhandahålla indexdefinitioner i JSON-format och att skapa index i out-of-band-läge där inga ändringar görs i en live-instans.

Processen som ska beaktas för detta användningsfall är:

1. En utvecklare skulle uppdatera indexdefinitionerna på en lokal instans och sedan generera en JSON-fil för indexdefinitioner med alternativet `--index-definitions`

1. Den uppdaterade JSON-versionen ges sedan till systemadministratören
1. Systemadministratören följer out-of-band-metoden och förbereder indexet för en annan installation
1. När detta är klart importeras de genererade indexfilerna till en AEM-installation som körs.
