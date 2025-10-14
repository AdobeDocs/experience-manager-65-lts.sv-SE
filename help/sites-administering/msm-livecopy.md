---
title: Skapa och synkronisera Live-kopior
description: Lär dig hur du skapar och synkroniserar Live-kopior i Adobe Experience Manager.
feature: Multi Site Manager
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 47128d86-ee8d-4a15-ba3e-4cf2e2ec6191
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '4174'
ht-degree: 0%

---

# Skapa och synkronisera Live-kopior{#creating-and-synchronizing-live-copies}

Du kan skapa en live-kopia från en sida eller en plantryckskonfiguration och sedan hantera arv och synkronisering.

## Hantera layoutkonfigurationer {#managing-blueprint-configurations}

En plankonfiguration identifierar en befintlig webbplats som du vill använda som källa för en eller flera live-kopieringssidor.

>[!NOTE]
>
>Med designkonfigurationer kan du överföra innehållsändringar till live-kopior. Se [Live-kopior - Source, utkast och layoutkonfigurationer](/help/sites-administering/msm.md#source-blueprints-and-blueprint-configurations).

När du skapar en ritningskonfiguration väljer du en mall som definierar den interna strukturen för ritningen. Standardmallen för utkast förutsätter att källwebbplatsen har följande egenskaper:

* Webbplatsen har en rotsida.
* De direkt underordnade sidorna i roten är språkgrenar på webbplatsen. När du skapar en live-kopia visas språken som valfritt innehåll som ska inkluderas i kopian.
* Roten för varje språkgren har en eller flera underordnade sidor. När du skapar en live-kopia visas underordnade sidor som kapitel som du kan inkludera i live-kopian.

>[!NOTE]
>
>En annan struktur kräver en annan designmall.

När du har skapat en ritningskonfiguration konfigurerar du följande egenskaper:

* **Namn**: Namnet på designkonfigurationen.
* **Source-sökväg**: Sökvägen till rotsidan på webbplatsen som du använder som källa (utkast).
* **Beskrivning**. (Valfritt) En beskrivning av ritningskonfigurationen. Beskrivningen visas i listan med designkonfigurationer att välja mellan när du skapar en plats.

När du använder din ritningskonfiguration kan du associera den med en utrullningskonfiguration som bestämmer hur live-kopiorna av källan/ritningen synkroniseras. Se [Ange vilka utrullningskonfigurationer som ska användas](/help/sites-administering/msm-sync.md#specifying-the-rollout-configurations-to-use).

### Skapa en designkonfiguration {#creating-a-blueprint-configuration}

Så här skapar du en ritningskonfiguration:

1. [Navigera](/help/sites-authoring/basic-handling.md#global-navigation) till menyn **Verktyg** och välj sedan menyn **Platser** .
1. Välj **utkast** för att öppna konsolen **Skapa utkast**:

   ![Utskriftskonfigurationer](assets/blueprint-configurations.png)

1. Välj **Skapa**.
1. Välj en ritningsmall och sedan **Nästa** för att fortsätta.
1. Välj den källsida som ska användas som utkast och **Nästa** för att fortsätta.
1. Definiera:

   * **Titel**: obligatorisk titel för ritningen
   * **Beskrivning**: En valfri beskrivning som ger mer information.

1. **Skapa** skapar en ritningskonfiguration baserat på din specifikation.

### Redigera eller ta bort en utkastkonfiguration {#editing-or-deleting-a-blueprint-configuration}

Du kan redigera eller ta bort en befintlig ritningskonfiguration:

1. [Navigera](/help/sites-authoring/basic-handling.md#global-navigation) till menyn **Verktyg** och välj sedan menyn **Platser** .
1. Välj **utkast** för att öppna konsolen **Skapa utkast**:

   ![Utskriftskonfigurationer](assets/blueprint-configurations.png)

1. Välj önskad konfiguration av utkast - lämpliga åtgärder blir tillgängliga i verktygsfältet:

   * **Egenskaper**; du kan använda detta för att visa och sedan redigera egenskaperna för konfigurationen.
   * **Ta bort**

## Skapa en Live Copy {#creating-a-live-copy}

### Skapa en Live-kopia av en sida {#creating-a-live-copy-of-a-page}

Du kan skapa en live-kopia av vilken sida eller gren som helst. När du skapar en live-kopia kan du ange vilka rollout-konfigurationer som ska användas för att synkronisera innehållet:

* De valda rollout-konfigurationerna gäller för live-kopieringssidan och dess underordnade sidor.
* Om du inte anger några utplaceringskonfigurationer avgör MSM vilka utplaceringskonfigurationer som ska användas. Se [Ange vilken utrullningskonfiguration som ska användas](/help/sites-administering/msm-sync.md#specifying-the-rollout-configurations-to-use).

Du kan skapa en live-kopia av vilken sida som helst:

* Sidor som refereras av en [blå konfiguration](#creating-a-blueprint-configuration).
* Och sidor som inte har någon anslutning till en konfiguration.
* AEM har också stöd för att skapa en live-kopia på sidorna i en annan live-kopia.

Den enda skillnaden är att tillgängligheten för kommandot **Rollout** på käll-/plantryckssidorna är beroende av om källan refereras av en plantryckskonfiguration:

* Om du skapar den aktiva kopian från en källsida som **är** refererad till i en ritningskonfiguration, kommer kommandot Rollout att vara tillgängligt på käll-/ritningssidan.
* Om du skapar den aktiva kopian från en källsida som **inte** refereras till i en ritningskonfiguration, kommer kommandot Rollout inte att vara tillgängligt på käll-/ritningssidan.

Så här skapar du en live-kopia:

1. I konsolen **Platser** väljer du **Skapa** och sedan **Live-kopia**.

   ![Skapa Live-kopia](assets/chlimage_1-212.png)

1. Välj källsidan och klicka sedan på **Nästa**. Till exempel:

   ![Välj källsida](assets/chlimage_1-213.png)

1. Ange målsökvägen för live-kopian (öppna den överordnade mappen/sidan för live-kopian) och klicka sedan på **Nästa**.

   ![Ange mål](assets/chlimage_1-214.png)

   >[!NOTE]
   >
   >Målsökvägen får inte finnas i källsökvägen.

1. Ange:

   * en **rubrik** för sidan.
   * ett **namn** som används i URL:en.

   ![Ange titel och namn](assets/chlimage_1-215.png)

1. Använd kryssrutan **Uteslut undersidor**:

   * Markerad: skapa en live-kopia av den markerade sidan (endast begränsad live-kopia)
   * Inte markerad: skapa en live-kopia som innehåller alla underordnade till den markerade sidan (djup live-kopia)

1. (Valfritt) Om du vill ange en eller flera rollout-konfigurationer som ska användas för livecopy använder du listrutan **Rollout Configs** för att välja dem. Markerade konfigurationer visas under listrutan.
1. Klicka på **Skapa**. Ett bekräftelsemeddelande visas. Här kan du välja antingen **Öppna** eller **Klar**.

### Skapa en Live-kopia av en plats från en designkonfiguration {#creating-a-live-copy-of-a-site-from-a-blueprint-configuration}

Skapa en live-kopia med hjälp av en ritningskonfiguration för att skapa en webbplats baserad på innehållet i ritningen (källan). När du skapar en live-kopia från en ritningskonfiguration väljer du en eller flera språkgrenar i den ritningskälla som ska kopieras och sedan markerar du de kapitel som ska kopieras från språkgrenarna. Se [Skapa en designkonfiguration](/help/sites-administering/msm-livecopy.md#creating-a-blueprint-configuration).

Om du utelämnar vissa språkgrenar eller kapitel från live-kopian kan du lägga till dem senare. Mer information finns i [Skapa en Live-kopia i en Live-kopia (utkast-konfiguration)](#creating-a-live-copy-inside-a-live-copy-blueprint-configuration).

>[!CAUTION]
>
>När ritningskällan innehåller länkar och referenser som avser ett stycke i en annan gren, uppdateras inte målen på sidorna för den aktiva kopian, utan de är fortfarande kopplade till det ursprungliga målet.

När du skapar platsen anger du värden för följande egenskaper:

* **Ursprungliga språk**: De språkgrenar i den blå källan som ska inkluderas i den aktiva kopian.
* **Inledande kapitel**: De underordnade sidorna för grenarna för utkast-språk som ska inkluderas i live-kopian.
* **Målsökväg**: Platsen för den aktiva kopieringswebbplatsens rotsida.
* **Titel**: Titeln på rotsidan på den publicerade kopiewebbplatsen.
* **Namn**: (Valfritt) Namnet på den JCR-nod som lagrar den aktiva kopians rotsida. Standardvärdet baseras på titeln.
* **Webbplatsägare**: (valfritt)
* **Live-kopia**: Välj det här alternativet om du vill skapa en direktrelation med källwebbplatsen. Om du inte markerar det här alternativet skapas en kopia av ritningen, men den synkroniseras inte med källan.
* **Utrullningskonfigurationer**: (Valfritt) Välj en eller flera utrullningskonfigurationer som ska användas för synkronisering av live-kopian. Som standard ärvs rollout-konfigurationerna från ritningen. Mer information finns i [Ange att rollout-konfigurationer ska användas](/help/sites-administering/msm-sync.md#specifying-the-rollout-configurations-to-use).

Så här skapar du en live-kopia av en webbplats från en planeringskonfiguration:

1. I konsolen **Platser** väljer du **Skapa** och sedan **Plats** i listrutan.
1. Välj den plantryckskonfiguration som ska användas som källa för live-kopian och fortsätt med **Nästa**:

   ![Välj en ritningskonfiguration som källa för live-kopia](assets/blueprint-configuration-select.png)

1. Använd väljaren **Inledande språk** för att ange vilka språk som ska användas för den publicerade kopian på webbplatsen för utkast.

   Alla tillgängliga språk är markerade som standard. Om du vill ta bort ett språk klickar du på **X** som visas bredvid språket.

   Till exempel:

   ![Välj initiala språk](assets/chlimage_1-217.png)

1. Använd listrutan **Inledande kapitel** för att välja vilka avsnitt i ritningen som ska ingå i den aktiva kopian. Alla tillgängliga kapitel inkluderas som standard, men kan tas bort.
1. Ange värden för de återstående egenskaperna och välj sedan **Skapa**. I bekräftelsedialogrutan väljer du **Klar** för att återgå till konsolen **Platser** eller **Öppna platsen** för att öppna webbplatsens rotsida.

### Skapa en Live-kopia i en Live-kopia (utkast-konfiguration) {#creating-a-live-copy-inside-a-live-copy-blueprint-configuration}

När du skapar en live-kopia i den befintliga live-kopian (skapad med en ritningskonfiguration) kan du infoga en eller flera språkversioner som inte fanns med när live-kopian ursprungligen skapades.

## Övervaka din Live Copy {#monitoring-your-live-copy}

### Se status för en Live-kopia {#seeing-the-status-of-a-live-copy}

Egenskaperna för en live-kopia visar följande information om live-kopian:

* **Source**: Källsidan för den aktiva kopieringssidan.
* **Status**: Synkroniseringsstatusen för den aktiva kopian. Statusen omfattar huruvida den aktiva kopian är uppdaterad med källan, när den senaste synkroniseringen inträffade och vem som utförde synkroniseringen.
* **Konfiguration**:

   * Anger om sidan fortfarande omfattas av live-kopiarv.
   * Anger om konfigurationen ärvs från den överordnade sidan.
   * Alla utrullningskonfigurationer som används för live-kopian.

Så här visar du egenskaperna:

1. I konsolen **Platser** markerar du live-kopieringssidan och öppnar egenskaperna.
1. Välj fliken **Live-kopia**.

   Till exempel:

   ![Välj Live-kopia](assets/chlimage_1-218.png)

### Visa Live-kopior av en blå sida {#seeing-the-live-copies-of-a-blueprint-page}

Utskriftssidor (som refereras i en ritningskonfiguration) ger dig en lista över de live-kopieringssidor som använder den aktuella (blå) sidan som källa. Använd den här listan för att hålla reda på live-kopior. Listan visas på fliken **Utskrift** i [sidegenskaperna](/help/sites-authoring/editing-page-properties.md).

![Fliken Utskrift](assets/chlimage_1-219.png)

## Synkronisera din Live-kopia {#synchronizing-your-live-copy}

### Rulla ut ett utkast {#rolling-out-a-blueprint}

Rulla ut en ritningssida för att överföra innehållsändringar till live-kopior. En **utrullningsåtgärd** kör de utrullningskonfigurationer som använder utlösaren [Vid utrullning](/help/sites-administering/msm-sync.md#rollout-triggers).

>[!NOTE]
>
>Konflikter kan uppstå om nya sidor med samma sidnamn skapas både i den blå grenen och i en beroende livekopiegren.
>
>Sådana [konflikter måste hanteras och lösas vid utrullning](/help/sites-administering/msm-rollout-conflicts.md).
>

#### Skapa en skiss från Sidegenskaper {#rolling-out-a-blueprint-from-page-properties}

1. I konsolen **Platser** markerar du sidan i planen och öppnar egenskaperna.
1. Öppna fliken **Utskrift**.
1. Välj **Rollout**.

   ![Välj utrullning](assets/chlimage_1-220.png)

1. Ange sidorna och eventuella underordnade sidor och bekräfta sedan med bockmarkeringen:

   ![Ange sidor och undersidor](assets/chlimage_1-221.png)

1. Ange om utrullningsjobbet ska köras omedelbart (**nu**) eller vid ett annat datum/tid (**senare**).

   ![Utrullningsutkast](assets/rollout-blueprint.png)

Utsläppen bearbetas som asynkrona jobb och kan kontrolleras på [**kontrollpanelen för asynkrona jobb** &#x200B;](asynchronous-jobs.md#monitor-the-status-of-asynchronous-operations) på **Global navigering** > **Verktyg** > **Åtgärder** > **Jobs**

>[!NOTE]
>
>Asynkron rollout-bearbetning kräver AEM 6.5.3.0 eller senare. I tidigare versioner bearbetades sidorna direkt och synkront.

#### Rulla ut en skiss från referensspåret {#roll-out-a-blueprint-from-the-reference-rail}

1. I konsolen **Platser** markerar du sidan i den aktiva kopian och öppnar panelen **[Referenser](/help/sites-authoring/basic-handling.md#references)** (i verktygsfältet).
1. Välj alternativet **Utskrift** i listan om du vill visa de utkast som är associerade med den här sidan.
1. Välj önskad rityta i listan.
1. Klicka på **Rollout**.
1. Du ombeds bekräfta informationen om utrullningen:

   * **Utrullningsomfång**:

     Ange om omfånget gäller enbart för den valda sidan eller om det ska omfatta underordnade sidor.

   * **Schema**:

     Ange om utrullningsjobbet ska köras omedelbart (**nu**) eller vid ett senare datum/tid (**senare**).

     ![Ange schemat](assets/rollout-live-copy.png)

1. När du har bekräftat informationen väljer du **Rollout** för att utföra åtgärden.

Utsläppen bearbetas som asynkrona jobb och kan kontrolleras på [**kontrollpanelen för asynkrona jobb** &#x200B;](asynchronous-jobs.md#monitor-the-status-of-asynchronous-operations) på **Global navigering** > **Verktyg** > **Åtgärder** > **Jobs**

>[!NOTE]
>
>Asynkron rollout-bearbetning kräver AEM 6.5.3.0 eller senare. I tidigare versioner bearbetades sidor omedelbart och synkront såvida inte alternativet **Bakgrundssammanslagning** har markerats.

#### Rulla ut en utkast från Live Copy-översikt {#roll-out-a-blueprint-from-the-live-copy-overview}

Åtgärden [Utrullning är även tillgänglig från Live-kopieringsöversikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) när en utskrivningssida är markerad.

1. Öppna [Live Copy Overview](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) och välj en designsida.
1. Välj **Utrullning** i verktygsfältet.
1. Ange sidorna och eventuella underordnade sidor och bekräfta sedan med bockmarkeringen:

   ![Markera sidor och undersidor](assets/chlimage_1-223.png)

1. Ange om utrullningsjobbet ska köras omedelbart (**nu**) eller vid ett annat datum/tid (**senare**).

   ![Utrullningsutkast](assets/rollout-blueprint.png)

Utsläppen bearbetas som asynkrona jobb och kan kontrolleras på [**kontrollpanelen för asynkrona jobb** &#x200B;](asynchronous-jobs.md#monitor-the-status-of-asynchronous-operations) på **Global navigering** > **Verktyg** > **Åtgärder** > **Jobs**

>[!NOTE]
>
>Asynkron rollout-bearbetning kräver AEM 6.5.3.0 eller senare. I tidigare versioner bearbetades sidorna direkt och synkront.

### Synkronisera en Live-kopia {#synchronizing-a-live-copy}

Synkronisera en live-kopia för att dra innehållsändringar från källan till live-kopian.

#### Synkronisera en Live-kopia från Sidegenskaper {#synchronize-a-live-copy-from-page-properties}

Synkronisera en live-kopia för att dra ändringar från källan till livecopy.

>[!NOTE]
>
>Synkronisering kör de utrullningskonfigurationer som använder utlösaren [Vid utrullning](/help/sites-administering/msm-sync.md#rollout-triggers).

1. I konsolen **Platser** markerar du live-kopieringssidan och öppnar egenskaperna.
1. Öppna fliken **Live-kopia**.
1. Klicka på **Synkronisera**.

   ![Synkronisera](assets/chlimage_1-224.png)

   Bekräftelse kommer att begäras. Använd **Synkronisera** för att fortsätta.

#### Synkronisera en Live-kopia från Live Copy-översikt {#synchronize-a-live-copy-from-the-live-copy-overview}

Åtgärden [Synkronisera är också tillgänglig från Live-kopieringsöversikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) när en Live-kopia-sida är markerad.

1. Öppna [Live Copy-översikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) och välj en Live-kopieringssida.
1. Välj **Synkronisera** i verktygsfältet.
1. Bekräfta **utrullningsåtgärden** i dialogrutan efter att du har angett om du vill inkludera:

   * **Sidor och undersidor**
   * **Endast sida**

   ![Bekräfta utrullning](assets/chlimage_1-225.png)

## Ändra innehåll i Live Copy {#changing-live-copy-content}

Om du vill ändra innehållet i en live-kopia kan du:

* Lägg till stycken på sidan.
* Uppdatera befintligt innehåll genom att bryta arvet av live-kopior för alla sidor eller komponenter.

>[!NOTE]
>
>Om du skapar en sida manuellt i den aktiva kopian är den lokal för den aktiva kopian, vilket innebär att den inte har någon motsvarande källsida att koppla till.
>
>Det bästa sättet att skapa en lokal sida som ingår i relationen är att skapa den i källan och göra en (djup) utrullning. Då skapas sidan lokalt som live-kopior.

>[!NOTE]
>
>Konflikter kan uppstå om nya sidor med samma sidnamn skapas både i den blå grenen och i en beroende livekopiegren.
>
>Sådana [konflikter måste hanteras och lösas vid utrullning](/help/sites-administering/msm-rollout-conflicts.md).
>

### Lägga till komponenter på en Live Copy-sida {#adding-components-to-a-live-copy-page}

Lägg till komponenter på en live-kopia när som helst. Arvsstatus för live-kopian och dess styckesystem styr inte möjligheten att lägga till komponenter.

När den aktiva kopieringssidan synkroniseras med källsidan ändras inte de tillagda komponenterna. Se även [Ändra ordning på komponenter på en Live-kopieringssida](#changing-the-order-of-components-on-a-live-copy-page).

>[!NOTE]
>
>Ändringar som görs lokalt för en komponent som markerats som en behållare skrivs inte över av innehållet i ritningen i en utrullning. Mer information finns i [Bästa praxis för MSM](/help/sites-administering/msm-best-practices.md#components-and-container-synchronization).

### Pausa arv för en sida {#suspending-inheritance-for-a-page}

När du skapar en live-kopia sparas live-kopiekonfigurationen på rotsidan för de kopierade sidorna. Alla underordnade sidor på rotsidan ärver konfigurationerna för live-kopior. Komponenterna på livecopy-sidorna ärver också konfigurationen för live-kopiering.

Du kan göra uppehåll i arvet av live-kopior för en live-kopieringssida så att du kan ändra sidegenskaper och komponenter. När du gör uppehåll i arv synkroniseras inte längre sidegenskaperna och komponenterna med källan.

>[!NOTE]
>
>Du kan också [koppla loss en live-kopia](#detaching-a-live-copy) från dess plan om du vill ta bort alla anslutningar. Frigör är permanent och icke-reversibel.

>[!NOTE]
>
>Om komponenten är markerad som en behållare gäller inte åtgärderna för att avbryta och göra uppehåll för dess underordnade komponenter. Se även [MSM Best Practices](/help/sites-administering/msm-best-practices.md#components-and-container-synchronization) för mer information.

#### Pausa arv från Sidegenskaper {#suspending-inheritance-from-page-properties}

Så här gör du uppehåll i arv på en sida:

1. Öppna egenskaperna för live-kopieringssidan antingen med kommandot **Visa egenskaper** i konsolen **Webbplatser** eller med **Sidinformation** i verktygsfältet för sidan.
1. Klicka på fliken **Live-kopia**.
1. Välj **Gör uppehåll** i verktygsfältet. Du kan sedan välja något av följande:

   * **Gör uppehåll**: endast aktuell sida
   * **Gör uppehåll med underordnade**: aktuell sida tillsammans med eventuella underordnade sidor

1. Välj **Gör uppehåll** i bekräftelsedialogrutan.

#### Pausa arv från Live Copy-översikt {#suspending-inheritance-from-the-live-copy-overview}

Åtgärden [Gör uppehåll är även tillgänglig från Live-kopieringsöversikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) när en Live-kopia-sida är markerad.

1. Öppna [Live Copy-översikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) och välj en Live-kopieringssida.
1. Välj **Gör uppehåll** i verktygsfältet.
1. Välj lämpligt alternativ från:

   * **Gör uppehåll**
   * **Pausa med underordnade**

   ![Välj lämpligt uppehållsalternativ](assets/chlimage_1-226.png)

1. Bekräfta åtgärden **Gör uppehåll** i dialogrutan **Gör uppehåll i Live-kopia**:

   ![Skjut upp åtgärd](assets/chlimage_1-227.png)

### Återuppta arv för en sida {#resuming-inheritance-for-a-page}

Att skjuta upp live-kopiarv för en sida är en tillfällig åtgärd. När åtgärden **Återuppta** har pausats blir den tillgänglig så att du kan återskapa den aktiva relationen.

När du återaktiverar arv synkroniseras inte sidan automatiskt med källan. Du kan begära en synkronisering, om detta krävs, antingen:

* I dialogrutan **Återuppta**/**Återgå** , till exempel:

  ![Återuppta eller återställ](assets/chlimage_1-228.png)

* I ett senare skede, genom att manuellt välja synkroniseringsåtgärden.

>[!CAUTION]
>
>När du återaktiverar arv synkroniseras inte sidan automatiskt med källan. Du kan begära en synkronisering manuellt om det behövs, antingen vid återupptagningen eller senare.

#### Återuppta arv från Sidegenskaper {#resuming-inheritance-from-page-properties}

När [har pausats](#suspending-inheritance-from-page-properties) ändras åtgärden **Återuppta** i verktygsfältet för sidegenskaperna:

![Fortsätt](assets/chlimage_1-229.png)

När du väljer det här alternativet visas dialogrutan. Om det behövs kan du välja en synkronisering och sedan bekräfta åtgärden.

#### Återuppta en Live Copy-sida från Live Copy-översikten {#resume-a-live-copy-page-from-the-live-copy-overview}

Åtgärden [Återuppta är också tillgänglig från Live-kopieringsöversikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) när en Live-kopia-sida är markerad.

1. Öppna [Live Copy Overview](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) och välj en Live Copy-sida som har pausats. Den visas som **INHERITANCE CANCELED**.
1. Välj **Fortsätt** i verktygsfältet.
1. Ange om du vill synkronisera sidan när arvet har återställts och bekräfta sedan åtgärden **Återuppta** i dialogrutan **Återuppta Live-kopia**.

### Ändra arvsdjup (grund/djup) {#changing-inheritance-depth-shallow-deep}

På en befintlig live-kopia kan du ändra siddjupet, det vill säga om underordnade sidor ska tas med.

* Byt till en ytlig live-kopia:

   * Kommer att ha omedelbar effekt och är icke-reversibel.

      * Underordnade sidor kopplas explicit från live-kopian. Ytterligare ändringar av underordnade kan inte bevaras om de ångras.

      * Alla underordnade `LiveRelationships` tas bort, även om det finns kapslade `LiveCopies`.

* Byt till en djup live-kopia:

   * Underordnade sidor ändras inte.
   * Om du vill se effekten av switchen kan du göra en utrullning, så tillämpas alla innehållsändringar enligt utrullningskonfigurationen.

* Byt till en ytlig live-kopia och sedan tillbaka till djupet:

   * Alla underordnade objekt till den (tidigare) ytliga, aktiva kopian behandlas som om de skapats manuellt och därför flyttas bort med `[oldname]_msm_moved name`.

Så här anger eller ändrar du djup:

1. Öppna egenskaperna för live-kopieringssidan antingen med kommandot **Visa egenskaper** i konsolen **Webbplatser** eller med **Sidinformation** i verktygsfältet för sidan.
1. Klicka på fliken **Live-kopia**.
1. I avsnittet **Konfiguration** anger eller avmarkerar du alternativet **Live Copy-arv** beroende på om underordnade sidor inkluderas:

   * checked - a deep copy (the child pages are included)
   * clear - a shallow live copy (child pages are exclude)

   >[!CAUTION]
   >
   >Att byta till en ytlig live-kopia har omedelbar effekt och går inte att ångra.
   >
   >Mer information finns i [Live-kopior - Disposition](/help/sites-administering/msm.md#live-copies-composition).

1. Klicka på **Spara** om du vill behålla uppdateringarna.

### Avbryta arv för en komponent {#cancelling-inheritance-for-a-component}

Avbryt arvet av live-kopia för en komponent så att komponenten inte längre är synkroniserad med källkomponenten. Du kan aktivera arv vid ett senare tillfälle om det behövs.

>[!NOTE]
>
>Om komponenten är markerad som en behållare gäller inte åtgärderna för att avbryta och göra uppehåll för dess underordnade komponenter. Se även [MSM Best Practices](/help/sites-administering/msm-best-practices.md#components-and-container-synchronization) för mer information.

>[!NOTE]
>
>När du återaktiverar arv synkroniseras inte komponenten automatiskt med källan. Du kan begära en synkronisering manuellt om det behövs.

Avbryt arv för att ändra komponentinnehållet eller ta bort komponenten:

1. Klicka på den komponent som du vill avbryta arvet för.

   ![Välj komponent för att avbryta arvsåtgärd](assets/chlimage_1-230.png)

1. Klicka på ikonen **Avbryt arv** i komponentens verktygsfält.

   ![Avbryt arv](do-not-localize/chlimage_1-8.png)

1. Bekräfta åtgärden med **Ja** i dialogrutan Avbryt arv.

   Komponentverktygsfältet uppdateras med alla (lämpliga) redigeringskommandon.

### Återaktivera arv för en komponent {#re-enabling-inheritance-for-a-component}

Om du vill aktivera arv för en komponent klickar du på ikonen **Aktivera arv igen** i komponentens verktygsfält.

![Återaktivera arv](do-not-localize/chlimage_1-9.png)

### Ändra ordning på komponenter på en Live Copy-sida {#changing-the-order-of-components-on-a-live-copy-page}

Om en live-kopia innehåller komponenter som är en del av ett styckesystem följer arvet av det styckesystemet följande regler:

* Komponenternas ordning i ett ärvt styckesystem kan ändras, även om arv är etablerat.
* Vid utrullning återställs komponenternas ordning från ritningen. Om nya komponenter lades till i live-kopian innan den lanserades, kommer de att ordnas om tillsammans med de komponenter ovanför vilka de lades till.
* Om arvet av styckesystemet avbryts återställs inte komponenternas ordning vid utrullning och förblir som i den aktiva kopian.

>[!NOTE]
>
>När du återställer ett annullerat arv i ett styckesystem återställs inte ordningen på komponenterna **automatiskt** från ritningen. Du kan begära en synkronisering manuellt om det behövs.

Använd följande procedur för att avbryta arv av styckesystemet.

1. Öppna sidan med live-kopian.
1. Dra en befintlig komponent till en ny plats på sidan.
1. Bekräfta åtgärden med **Ja** i dialogrutan **Avbryt arv**.

### Åsidosätta egenskaper för en Live Copy-sida {#overriding-properties-of-a-live-copy-page}

Sidegenskaperna för en Live-kopia-sida ärvs (och kan inte redigeras) från källsidan som standard.

Du kan avbryta arv av en egenskap när du behöver ändra egenskapsvärdet för live-kopian. En länkikon anger att arv är aktiverat för egenskapen.

![Avbryt arv av egenskap](assets/chlimage_1-231.png)

När du avbryter arv kan du ändra egenskapsvärdet. En ikon med en bruten länk anger att arvet har avbrutits.

![Ändra egenskap när arv bryts](assets/chlimage_1-232.png)

Du kan senare återaktivera arv för en egenskap om det behövs.

>[!NOTE]
>
>När du återaktiverar arv synkroniseras inte egenskapen för live-kopieringssidan automatiskt med egenskapen source. Du kan begära en synkronisering manuellt om det behövs.

1. Öppna egenskaperna för live-kopieringssidan med alternativet **Visa egenskaper** i konsolen **Webbplatser** eller med ikonen **Sidinformation** i verktygsfältet för sidan.
1. Om du vill avbryta arvet av en egenskap klickar du på länkikonen som visas till höger om egenskapen.

   ![Avbryt arv av egenskap](do-not-localize/chlimage_1-10.png)

1. Klicka på **Ja** i bekräftelsedialogrutan **Avbryt arv**.

### Återställa egenskaper för en Live Copy-sida {#revert-properties-of-a-live-copy-page}

Om du vill aktivera arv för en egenskap klickar du på ikonen **Återställ arv** som visas bredvid egenskapen.

![Återgå arv](do-not-localize/chlimage_1-11.png)

### Återställa en Live Copy-sida {#resetting-a-live-copy-page}

Återställ en live-kopia till:

* Ta bort alla annulleringar av arv och
* Returnera sidan till samma läge som källsidan.

Återställningen påverkar ändringar som du har gjort i sidegenskaperna, styckesystemet och komponenterna.

#### Återställ en Live Copy-sida från Sidegenskaperna {#reset-a-live-copy-page-from-the-page-properties}

1. I konsolen **Webbplatser** markerar du live-kopieringssidan och väljer **Visa egenskaper**.
1. Öppna fliken **Live-kopia**.
1. Välj **Återställ** i verktygsfältet.

   ![Återställ](assets/chlimage_1-233.png)

1. I dialogrutan **Återställ Live-kopia** bekräftar du med **Återställ**.

#### Återställ en Live Copy-sida från Live Copy-översikten {#reset-a-live-copy-page-from-the-live-copy-overview}

Åtgärden [Återställ är även tillgänglig från Live-kopieringsöversikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) när en Live-kopieringssida är markerad.

1. Öppna [Live Copy-översikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) och välj en Live-kopieringssida.
1. Välj **Återställ** i verktygsfältet.
1. Bekräfta åtgärden **Återställ** i dialogrutan **Återställ Live-kopia**:

   ![Bekräfta återställning](assets/chlimage_1-234.png)

## Jämföra en Live Copy-sida med en designsida {#comparing-a-live-copy-page-with-a-blueprint-page}

Om du vill spåra de ändringar du har gjort kan du visa planeringsidan i **Referenser** och jämföra den med den aktiva kopiesidan:

1. I konsolen **Platser** navigerar [till en ritnings- eller Live-kopia och markerar den](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Öppna panelen **[Referenser](/help/sites-authoring/basic-handling.md#references)** och välj:

   * **Utskrift** (när en live-kopieringssida har valts)
   * **Live-kopior** (när en ritningssida har valts)

1. Välj en specifik live-kopia och sedan antingen:

   * **Jämför med utkast** (när en live-kopieringssida har valts)
   * **Jämför med Live-kopia** (när en ritningssida har valts)

   Till exempel:

   ![Jämför](assets/chlimage_1-235.png)

1. De två sidorna (live copy och plan) öppnas sida vid sida.

   Mer information om hur du använder den här funktionen finns i [Sidskillnad](/help/sites-authoring/page-diff.md).

## Koppla loss en Live-kopia {#detaching-a-live-copy}

Koppla loss permanent tar bort den aktiva relationen mellan en live-kopia och dess käll-/ritningssida. Alla MSM-relevanta egenskaper tas bort från den aktiva kopian och de aktiva kopieringssidorna blir en fristående kopia.

>[!CAUTION]
>
>Du kan inte återskapa den aktiva relationen när du har frigjort den aktiva kopian.
>
>Om du vill ta bort den aktiva relationen med alternativet att återställa den senare kan du [avbryta arv av live-kopia](#suspending-inheritance-for-a-page) för sidan.

Det påverkar var i trädet du använder **Koppla loss**:

* **Koppla loss från en rotsida i en LiveCopy**

  När den här åtgärden utförs på rotsidan för en live-kopia tas den aktiva relationen mellan alla sidor i ritningen och dess livecopy bort.

  Ytterligare ändringar av sidor i ritningen (som tidigare) **påverkar inte** livecopy (som tidigare).

* **Koppla loss på en undersida till en LiveCopy**

  När den här åtgärden utförs på en undersida (eller en gren) i en live-kopia:

   * direktrelationen tas bort för den undersidan (eller grenen)
   * och (sub-)sidorna i livekopiegrenen behandlas som om de skapats manuellt.

  *Undersidorna* är dock fortfarande beroende av den överordnade grenens aktiva relation, vilket innebär att en ytterligare utrullning av ritningssidorna kommer att göra båda:

   1. Byt namn på den eller de frånkopplade sidorna:

      * Detta beror på att MSM betraktar dem som manuellt skapade sidor som orsakar en konflikt eftersom de har samma namn som de livecopy-sidor som de försöker skapa.

   1. Skapa en (livecopy) sida med det ursprungliga namnet som innehåller ändringarna från utrullningen.

  >[!NOTE]
  >
  >Mer information om sådana situationer finns i [MSM-utrullningskonflikter](/help/sites-administering/msm-rollout-conflicts.md).

### Frigöra en Live Copy-sida från Sidegenskaper {#detach-a-live-copy-page-from-the-page-properties}

Så här frigör du en live-kopia:

1. I konsolen **Webbplatser** markerar du live-kopieringssidan och klickar på **Visa egenskaper**.
1. Öppna fliken **Live-kopia**.
1. Välj **Koppla loss** i verktygsfältet.

   ![Koppla loss](assets/chlimage_1-236.png)

1. En bekräftelsedialogruta visas. Välj **Koppla loss** för att slutföra åtgärden.

### Frigöra en Live Copy-sida från Live Copy-översikten {#detach-a-live-copy-page-from-the-live-copy-overview}

Åtgärden [Koppla loss är även tillgänglig från Live-kopieringsöversikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) när en Live-kopieringssida är markerad.

1. Öppna [Live Copy-översikten](/help/sites-administering/msm-livecopy-overview.md#using-the-live-copy-overview) och välj en Live-kopieringssida.
1. Välj **Koppla loss** från verktygsfältet.
1. Bekräfta åtgärden **Koppla loss** i dialogrutan **Koppla loss Live-kopia**:

   ![Bekräfta frånkoppling](assets/chlimage_1-237.png)
