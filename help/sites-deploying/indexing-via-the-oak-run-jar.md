---
title: Indexering med hjälp av Oak-runda JAR
description: Lär dig hur du utför indexering med hjälp av Oak-runda Jar.
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
solution: Experience Manager, Experience Manager Sites
feature: Configuring
role: Admin
exl-id: a6344463-7796-4ee3-8b2e-b3bfd2aec99a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 0%

---

# Indexering med hjälp av Oak-runda JAR {#indexing-via-the-oak-run-jar}

Oak-run har stöd för alla indexeringsanvändningsfall på kommandoraden utan att JMX-nivån behöver användas. Fördelarna med att använda ekon är:

1. Det är en ny indexeringsverktygslåda för AEM 6.4
1. Det minskar time to re-index-värdet vilket påverkar omindexeringstiden positivt i större databaser
1. Det minskar resursförbrukningen vid omindexering i AEM, vilket ger bättre systemprestanda för andra AEM-aktiviteter
1. Oak-runda har stöd för out-of-band: Om produktionsvillkoren inte tillåter att du kör omindexering på produktionsinstanser kan en klonad miljö användas för omindexering för att undvika en kritisk prestandapåverkan.

Nedan visas en lista med användningsfall som kan användas när du utför indexeringsåtgärder med verktyget `oak-run`.

## Konsekvenskontroller för index {#indexconsistencychecks}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Använd fall 1 - Kontroll av indexkonsekvens](/help/sites-deploying/oak-run-indexing-usecases.md#usercase1indexconsistencycheck).

* `oak-run.jar`avgör snabbt om Lucene Oak-index är skadade.
* Det är säkert att köra på en AEM-instans som används för konsekvenskontrollnivå 1 och 2.

![Kontroller av indexkonsekvens](assets/screen_shot_2017-12-14at135758.png)

## Indexstatistik {#indexstatistics}

>[!NOTE]
>
>Mer information om det här scenariot finns i [Använd fall 2 - Indexstatistik](/help/sites-deploying/oak-run-indexing-usecases.md#usecase2indexstatistics)

* `oak-run.jar` dumpar alla indexdefinitioner, viktiga indexvärden och indexinnehåll för offlineanalys.
* Körs säkert på en AEM-instans som används.

![image2017-12-19_9-47-40](assets/image2017-12-19_9-47-40.png)

## Beslutsträd för omindexering av metod {#reindexingapproachdecisiontree}

Diagrammet är ett beslutsträd för när olika omindexeringsmetoder ska användas.

![oak_-_reindexingwithoak-run](assets/oak_-_reindexingwithoak-run.png)

## Omindexering av MongoMK / RDMBMK {#reindexingmongomk}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Använd fall 3 - Omindexering](/help/sites-deploying/oak-run-indexing-usecases.md#usecase3reindexing).

### Textförextrahering för SegmentNodeStore och DocumentNodeStore {#textpre-extraction}

[Textförextrahering](/help/sites-deploying/best-practices-for-queries-and-indexing.md#how-to-perform-text-pre-extraction) (en funktion som fanns i AEM 6.3) kan användas för att minska tiden för omindexering. Textförextrahering kan användas med alla omindexeringsmetoder.

Beroende på indexeringsmetoden för `oak-run.jar` finns det olika steg på båda sidor av steget Utför omindexering i diagrammet nedan.

![Textförextrahering för SegmentNodeStore och DocumentNodeStore](assets/4.png)

>[!NOTE]
>
>Orange betecknar aktiviteter där AEM måste finnas i en underhållsperiod.

### Omindexering online för MongoMK eller RDBMK med oak-run.jar {#onlinere-indexingformongomk}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Indexera om - DocumentNodeStore](/help/sites-deploying/oak-run-indexing-usecases.md#reindexdocumentnodestore).

Detta är den rekommenderade metoden för omindexering av AEM-installationer med MongoMK (och RDBMK). Ingen annan metod bör användas.

Kör bara den här processen mot en enda AEM-instans i klustret.

![Omindexering online för MongoMK eller RDBMK med oak-run.jar](assets/5.png)

## Omindexering av tarMK {#re-indexingtarmk}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Reindex - SegmentNodeStore](/help/sites-deploying/oak-run-indexing-usecases.md#reindexsegmentnodestore).

* **Väntelägesöverväganden (tarMK)**

   * Det finns inget särskilt att tänka på när du använder vänteläge i Cold. Väntelägesinstanserna i Cold synkas som vanligt.

* **AEM Publish Farms (AE Publish Farms ska alltid vara tarMK)**

   * För publiceringsgrupper måste det göras för alla ELLER för att utföra stegen på en enda publicering. Sedan klonar du konfigurationen för andra (med alla vanliga försiktighetsåtgärder när du klonar AEM-instanser; sling.id - ska länka till något här).

### Omindexering online för tarMK {#onlinere-indexingfortarmk}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Online Reindex - SegmentNodeStore](/help/sites-deploying/oak-run-indexing-usecases.md#onlinereindexsegmentnodestore).

Detta är den metod som användes innan de nya indexeringsfunktionerna i oak-run.jar introducerades. Detta görs genom att ange egenskapen `reindex=true` i Oak-indexet.

Den här metoden kan användas om tids- och prestandaeffekterna som ska indexeras är godtagbara för kunden. Detta gäller ofta små till medelstora AEM-installationer.

![Omindexering online för tarMK](assets/6.png)

### Omindexering online av tarMK med oak-run.jar {#onlinere-indexingtarmkusingoak-run-jar}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Online Reindex - SegmentNodeStore - AEM-instansen körs](/help/sites-deploying/oak-run-indexing-usecases.md#onlinereindexsegmentnodestoretheaeminstanceisrunning).

Omindexering online av tarMK med hjälp av oak-run.jar är snabbare än [Omindexering online för tarMK](#onlinere-indexingfortarmk) som beskrivs ovan. Den kräver dock även körning under ett underhållsfönster, med en hänvisning till att fönstret är kortare och att fler steg krävs för att utföra omindexeringen.

>[!NOTE]
>
>Orange betecknar verksamhet där AEM måste utföras under en underhållsperiod.

![Omindexering av tarMK online med oak-run.jar](assets/7.png)

### Offline omindexing tarMK med oak-run.jar {#offlinere-indexingtarmkusingoak-run-jar}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Online Reindex - SegmentNodeStore - AEM-instansen är avstängd](/help/sites-deploying/oak-run-indexing-usecases.md#onlinereindexsegmentnodestoreaeminstanceisdown).

Omindexering offline för tarMK är den enklaste `oak-run.jar`-baserade omindexeringsmetoden för tarMK eftersom den kräver en enda `oak-run.jar`-kommentar. AEM-instansen måste dock stängas av.

>[!NOTE]
>
>Rött anger åtgärder där AEM måste avslutas.

![Omindexering av tarMK offline med oak-run.jar](assets/8.png)

### Omindexerings-TarMK utanför band med oak-run.jar  {#out-of-bandre-indexingtarmkusingoak-run-jar}

>[!NOTE]
>
>Mer detaljerad information om det här scenariot finns i [Omindexering utanför intervall - SegmentNodeStore](/help/sites-deploying/oak-run-indexing-usecases.md#outofbandreindexsegmentnodestore).

Omindexering utan band minimerar effekten av omindexering på AEM-instanser som används.

>[!NOTE]
>
>Rött anger åtgärder där AEM kan avslutas.

![Out-of-band Re-Indexing tarMK med oak-run.jar](assets/9.png)

## Uppdaterar indexeringsdefinitioner {#updatingindexingdefinitions}

>[!NOTE]
>
>Mer information om det här scenariot finns i [Använd fall 4 - Uppdatera indexdefinitioner](/help/sites-deploying/oak-run-indexing-usecases.md#usecase4updatingindexdefinitions).

### Skapa och uppdatera indexdefinitioner på tarMK med hjälp av ACS Kontrollera index {#creatingandupdatingindexdefinitionsontarmkusingacsensureindex}

>[!NOTE]
>
>ACS Kontrollera att Index är ett projekt som stöds av communityn och inte stöds av Adobe Support.

Detta tillåter indexdefinitioner för leverans via innehållspaketet, vilket senare resulterar i omindexering genom att omindexeringsflaggan ställs in på `true`. Detta fungerar för mindre uppsättningar där omindexering inte tar lång tid.

Mer information finns i [dokumentationen ](https://adobe-consulting-services.github.io/acs-aem-commons/features/ensure-oak-index/index.html) för att kontrollera ACS-index.

### Skapa och uppdatera indexdefinitioner på tarMK med oak-run.jar {#creatingandupdatingindexdefinitionsontarmkusingoak-run-jar}

Om tiden eller prestandaeffekten av omindexering med icke-`oak-run.jar`-metoder är för hög, kan följande `oak-run.jar`-baserade metod användas för att importera och indexera om Lucene-indexdefinitioner i en TarmMK-baserad AEM-installation.

![Skapa och uppdatera indexdefinitioner på tarMK med oak-run.jar](assets/10.png)

### Skapa och uppdatera indexdefinitioner på MonogMK med oak-run.jar {#creatingandupdatingindexdefinitionsonmonogmkusingoak-run-jar}

Om tiden eller prestandaeffekten av omindexering med icke-`oak-run.jar`-metoder är för hög kan följande `oak-run.jar`-baserade metod användas för att importera och indexera om Lucene-indexdefinitioner i MongoMK-baserade AEM-installationer.

![Skapa och uppdatera indexdefinitioner på MonogMK med oak-run.jar](assets/11.png)
