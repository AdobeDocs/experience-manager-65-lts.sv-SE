---
title: Importera och hantera program
description: Lär dig hur du importerar och hanterar program. Ett program är en behållare för lagring av resurser som krävs för att implementera en AEM-formulärlösning.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/importing_and_managing_applications_and_archives
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# Importera och hantera program{#import-and-manage-applications}

I AEM-formulär är ett *program* en behållare för att lagra resurser som krävs för att implementera en AEM-formulärslösning. Exempel på resurser är formulärdesigner, formulärfragment, bilder, processer, DDX-filer, formulärguider, HTML-sidor och SWF-filer. Under ett projekts utvecklingsfas kan Workbench-användare distribuera program direkt från programvyn i Workbench. När programmen har distribuerats visas de i administrationskonsolen på fliken Program på sidan Programhantering.

När ett program är klart för distribution till en produktionsserver paketerar Workbench-användaren programmet i en *AEM-formulärprogramfil* (.lca). Därefter använder en administratör administrationskonsolen för att importera och distribuera programfilen på fliken Program på sidan Programhantering.

Du kan också använda arkivfliken på sidan Programhantering för att importera LCA-konton som har skapats med workbench 8.x.

>[!NOTE]
>
>Det finns ett känt problem med att LCA-filer från en framtida version inte nödvändigtvis är bakåtkompatibla. Även om det kan vara möjligt att visa och importera LCA-filer från en framtida version av AEM-formulär (till exempel en förhandsversion), stöds inte detta och kan leda till avvikande beteenden.

Använd fliken Program för att importera och hantera program som har skapats i Workbench. Programadministratörer kan också exportera runtime-konfigurationen för ett program. Genom att exportera runtime-konfigurationen elimineras behovet av att manuellt konfigurera om inställningarna i produktionsmiljön innan distribuerade program startas. Konfigurationsfilen för miljön innehåller:

* inställningar för tjänstkonfiguration
* inställningar för poolkonfiguration
* konfigurationsinställningar för slutpunkt
* säkerhetsprofiler

## Importera ett program eller arkiv {#import-an-application-or-archive}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering.
1. Klicka på Importera.
1. Klicka på Bläddra, markera den .lca-fil som ska importeras och klicka på Förhandsgranska. På sidan Förhandsgranska program visas information om programmet.
1. (Valfritt) Om du vill visa en lista över resurserna i programmet klickar du på Visa Assets.
1. (Valfritt) Om du vill distribuera resurserna till körningsmiljön väljer du Distribuera Assets till körningsmiljö när importen är klar. Om du inte väljer det här alternativet kan du distribuera resurserna senare.
1. Klicka på Importera. Programmet visas på fliken Program.
1. Logga in i CRX-databasen med administratörsuppgifter.
1. Navigera till content/dam/lcapplications

   >[!NOTE]
   >
   >De importerade programmen visas i noden lcapplications.

1. Klicka på något av de importerade programmen.

   Egenskaperna för den valda CRX-noden visas på fliken Egenskaper till höger.

   Egenskapen **syncState** anger status för synkronisering av data mellan AEM Forms-servern och CRX-databasen. Så snart importprocessen börjar är det här läget inställt på 0 (noll). Det här läget anger att data inte är synkroniserade just nu. När data synkroniseras ställs läget in på 1.

## Distribuera ett program {#deploy-an-application}

Du kan distribuera program som du har importerat eller som Workbench-användare har importerat från Workbench.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering.
1. Markera kryssrutan bredvid programmet som du vill distribuera och klicka på Distribuera.
1. Klicka på OK i bekräftelsedialogrutan som visas.

## Avdistribuera ett program {#undeploy-an-application}

Du kan avdistribuera program från körningsmiljön.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering.
1. Markera kryssrutan bredvid programmet som du vill ta bort distributionen för och klicka på Avdistribuera.
1. Klicka på OK i bekräftelsedialogrutan som visas.

## Ta bort ett program från servern {#remove-an-application-from-the-server}

Avdistribuera programmet innan det tas bort från servern.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering.
1. Markera kryssrutan bredvid programmet som du vill ta bort och klicka på Ta bort.
1. Klicka på OK i bekräftelsedialogrutan som visas.

## Importera ett programs körtidskonfiguration {#import-an-application-s-runtime-configuration}

Om en programadministratör har exporterat runtime-konfigurationen för ett program kan du importera den till det distribuerade programmet. Du kan importera den antingen via administrationskonsolen eller via skriptad LCA-distribution.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering.
1. Klicka på programmets namn.
1. Klicka på Importera runtime-konfiguration.
1. Klicka på Bläddra och välj den XML-fil som innehåller körningskonfigurationen.
1. Klicka på Importera.

## Exportera ett programs körtidskonfiguration {#export-an-application-s-runtime-configuration}

Du kan exportera konfigurationsinformation vid körning för distribuerade program.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Programhantering.
1. Klicka på programmets namn.
1. Klicka på Exportera runtime-konfiguration och spara konfigurationsfilen (XML) som skapas.

## Skriptstyrd driftsättning av AEM blankettapplikationer {#scripted-deployment-of-aem-forms-applications}

Du kan också använda ett skriptbaserat distributionsverktyg för att distribuera programfiler, inklusive en settings.xml-fil som anger följande inställningar:

* inställningar för tjänstkonfiguration
* inställningar för poolkonfiguration
* konfigurationsinställningar för slutpunkt
* säkerhetsprofiler

Med skriptbaserad driftsättning slipper du manuellt konfigurera om inställningarna i produktionsmiljön innan du startar distribuerade program.

1. Navigera från en kommandotolk till *[aem-forms root]*/sdk/misc/Foundation/ArchiveManagement.
1. Läs filen ReadMe.txt om du vill ha mer detaljerade anvisningar.
1. Ändra filen scriptedDeploy.bat och sample-files/sample.xml manuellt enligt beskrivningen i filen readme.txt.
1. Kör filen scriptedDeploy.bat. Den här åtgärden distribuerar AEM-formulärarkivfilen med åsidosättningsinställningarna.
