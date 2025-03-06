---
title: Återanvända resurser med MSM
description: Använd resurser på flera sidor/mappar som är härledda från och länkade till överordnade resurser. Resurserna hålls synkroniserade med en primär kopia och med några klick får du uppdateringar från överordnade resurser.
contentOwner: AG
mini-toc-levels: 1
role: User, Admin, Architect
feature: Asset Management,Multi Site Manager
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 29365a8a-7d29-41b2-9a54-d12fe802f6ed
source-git-commit: 2e0cbe62754866d31de69547f9af1f2f63930f2c
workflow-type: tm+mt
source-wordcount: '3152'
ht-degree: 9%

---

# Återanvänd resurser med MSM för [!DNL Assets] {#reuse-assets-using-msm-for-assets}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/reuse-assets-using-msm.html?lang=en) |
| AEM 6.5 | Den här artikeln |

MSM-funktionaliteten (Multi Site Manager) i [!DNL Adobe Experience Manager] gör att användare kan återanvända innehåll som har skapats en gång och återanvänds på flera webbplatser. Samma sak är tillgängligt för digitala resurser som MSM för funktionen [!DNL Assets]. Med MSM för [!DNL Assets] kan du:

* Skapa resurser en gång och gör sedan kopior av dessa resurser för återanvändning i andra delar av webbplatsen.
* Håll flera kopior synkroniserade och uppdatera originalkopian en gång för att överföra ändringarna till de underordnade kopiorna.
* Gör lokala ändringar genom att tillfälligt eller permanent avbryta länkningen mellan överordnade och underordnade resurser.

## Förstå fördelarna och begreppen {#concepts}

### Så fungerar det och fördelarna {#how-it-works-and-the-benefits}

Mer information om användningsscenarierna för återanvändning av samma innehåll (text och resurser) på flera webbplatser finns i [möjliga MSM-scenarier](/help/sites-administering/msm.md). [!DNL Experience Manager] underhåller en länk mellan den ursprungliga resursen och dess länkade kopior, som kallas live-kopior. Tack vare den bevarade länken kan centraliserade ändringar skickas till många aktiva kopior. Detta ger snabbare uppdateringar och eliminerar samtidigt begränsningarna med att hantera duplicerade kopior. Spridningen av ändringar är felfri och centraliserad. Funktionen ger utrymme för uppdateringar som är begränsade till valda live-kopior. Användare kan koppla loss länken, d.v.s. bryta arv, och göra lokala redigeringar som inte skrivs över nästa gång den primära kopian uppdateras och ändringarna introduceras. Frånkopplingen kan göras för ett fåtal metadatafält eller för en hel resurs. Det ger flexibilitet att lokalt uppdatera resurser som ursprungligen ärvts från en primär kopia.

MSM upprätthåller en aktiv relation mellan källresursen och dess livekopior så att:

* Ändringar av källresurserna tillämpas (utlöses) även på live-kopior, det vill säga de live-kopior som synkroniseras med källan.
* Du kan uppdatera live-kopiorna genom att avbryta direktrelationen eller ta bort arvet för några få begränsade fält. Ändringarna i källan tillämpas inte längre på den aktiva kopian.

### Ordlista för MSM för [!DNL Assets] termer {#glossary}

**Source:** De ursprungliga resurserna eller mapparna. Primär kopia som live-kopior härleds från.

**Live-kopia:** Kopian av källresurserna/källmapparna som är synkroniserad med källan. Live-kopior kan vara en källa till fler live-kopior. Se hur du skapar LC:er.

**Arv:** En länk/referens mellan en resurs/mapp för en live-kopia och dess källa som systemet använder för att komma ihåg var uppdateringarna ska skickas. Arv finns på detaljnivå för metadatafält. Arv kan tas bort för selektiva metadatafält samtidigt som den aktiva relationen mellan källan och dess aktiva kopia bevaras.

**Utrullning:** En åtgärd som överför ändringar som gjorts i källan nedåt till dess aktiva kopior. Det går att uppdatera en eller flera live-kopior på en gång med en utrullningsåtgärd. Se utrullning.

**Utrullningskonfiguration:** Regler som avgör vilka egenskaper som synkroniseras, hur och när. Dessa konfigurationer används när du skapar live-kopior, kan redigeras senare och ett underordnat objekt kan ärva utrullningskonfigurationen från sin överordnade resurs. Använd bara standardkonfigurationen för utrullning för MSM för [!DNL Assets]. De andra rollout-konfigurationerna är inte tillgängliga för MSM för [!DNL Assets].

**Synkronisera:** En annan åtgärd, förutom utrullning, som skapar paritet mellan källan och dess live-kopia genom att skicka uppdateringarna från källan till live-kopiorna. En synkronisering initieras för en viss live-kopia och åtgärden hämtar ändringarna från källan. Om du använder den här åtgärden kan du bara uppdatera en av live-kopiorna. Se synkroniseringsåtgärd.

**Gör uppehåll:** Ta tillfälligt bort den aktiva relationen mellan en live-kopia och dess källresurs/källmapp. Du kan återuppta relationen. Se göra uppehåll i åtgärd.

**Återuppta:** Återuppta direktrelationen så att en live-kopia får uppdateringarna från källan igen. Se åtgärd för att återuppta.

**Återställ:** Återställningsåtgärden gör live-kopian igen till en replik av källan genom att skriva över lokala ändringar. Den tar också bort arvsannulleringar och återställer arv i alla metadatafält. Om du vill göra lokala ändringar i framtiden måste du ångra arvet av specifika fält. Se lokala ändringar i LC.

**Koppla loss:** Ta oåterkalleligt bort liverelationen för en resurs/mapp för en live-kopia. Efter att ha frigjort en åtgärd kan live-kopiorna aldrig få uppdateringar från källan och de slutar vara en live-kopia längre. Se Ta bort relation.

## Skapa en live-kopia av en resurs {#createlc}

Om du vill skapa en live-kopia av en eller flera källresurser eller mappar gör du något av följande:

* Metod 1: Välj källresurserna och klicka på **[!UICONTROL Create]** > **[!UICONTROL Live Copy]** i verktygsfältet högst upp.
* Metod 2: I användargränssnittet för [!DNL Experience Manager] klickar du på **[!UICONTROL Create]** > **[!UICONTROL Live Copy]** i gränssnittets övre högra hörn.

Du kan skapa live-kopior av en resurs eller en mapp åt gången. Du kan skapa live-kopior som är härledda från en resurs eller en mapp som är en live-kopia. Content Fragments (CF) stöds inte för användningsfallet. När de försöker skapa sina live-kopior kopieras CF-filer som de är utan någon relation. De kopierade CF:erna är en ögonblicksbild i tid och uppdateras inte när ursprungliga CF:er uppdateras.

Så här skapar du live-kopior med den första metoden:

1. Välj källmaterial eller mappar. Klicka på **[!UICONTROL Create]** > **[!UICONTROL Live Copy]** i verktygsfältet.

   ![Skapa live-kopia från [!DNL Experience Manager] interface](assets/create_lc1.png)

   *Figur: Skapa en live-kopia från [!DNL Experience Manager]-gränssnittet.*

1. Välj en målmapp. Klicka på **[!UICONTROL Next]**.
1. Ange titel och namn. Assets har inga barn. När du skapar en live-kopia av mappar kan du välja att ta med eller exkludera underordnade.
1. Välj en utrullningskonfiguration. Klicka på **[!UICONTROL Create]**.

Så här skapar du live-kopior med den andra metoden:

1. I [!DNL Experience Manager]-gränssnittet klickar du på **[!UICONTROL Create]** > **[!UICONTROL Live Copy]** i det övre högra hörnet.

   ![Skapa live-kopia från [!DNL Experience Manager] interface](assets/create_lc2.png)

   *Figur: Skapa en live-kopia från [!DNL Experience Manager]-gränssnittet.*

1. Välj källresurs eller källmapp. Klicka på **[!UICONTROL Next]**.
1. Välj målmapp. Klicka på **[!UICONTROL Next]**.
1. Ange titel och namn. Assets har inga barn. När du skapar en live-kopia av mappar kan du välja att ta med eller exkludera underordnade.
1. Välj en utrullningskonfiguration. Klicka på **[!UICONTROL Create]**.

>[!NOTE]
>
>När en källa eller en live-kopia flyttas behålls relationerna. När en live-kopia tas bort tas relationerna bort.

## Visa olika egenskaper och statusvärden för käll- och livekopia {#properties}

Du kan visa information och MSM-relaterade statusar för live-kopior som relation, synkronisering, rollouts och mer från de olika områdena i användargränssnittet för [!DNL Experience Manager].

Följande två metoder fungerar för resurser och mappar:

* Välj en live-kopia och hitta informationen på sidan Egenskaper.
* Välj källmapp och hitta detaljerad information om varje live-kopia från [!UICONTROL Live Copy Console].

>[!TIP]
>
>Om du vill kontrollera status för några separata live-kopior använder du den första metoden för att kontrollera sidan **[!UICONTROL Properties]**. Om du vill kontrollera status för många live-kopior använder du den andra metoden för att kontrollera sidan **[!UICONTROL Relationship Status]**.

### Information om och status för en live-kopia {#statuslcasset}

Följ de här stegen för att kontrollera information och status för en live-kopia-resurs eller en mapp.

1. Välj en live-kopia eller en mapp. Klicka på **[!UICONTROL Properties]** i verktygsfältet. Du kan också använda kortkommandot `p`.
1. Klicka på **[!UICONTROL Live Copy]**. Du kan kontrollera sökvägen till källan, avbrottsstatus, synkroniseringsstatus, det senaste utrullningsdatumet och den användare som gjorde den senaste utrullningen.

   ![Live-kopieringsinformation och -status visas i en konsol i Egenskaper](assets/lcfolder_info_properties.png)

   *Figur: Information om och status för Live-kopia.*

1. Du kan aktivera eller inaktivera om underordnade resurser lånar konfigurationen för live-kopia.

1. Du kan välja att live-kopian ska ärva utrullningskonfigurationen från den överordnade kopian eller ändra konfigurationen.

### Information om och status för alla live-kopior av en mapp {#status-lc-folder}

[!DNL Experience Manager] innehåller en konsol för att kontrollera status för alla live-kopior av en källmapp. Den här konsolen visar status för alla underordnade resurser.

1. Välj en källmapp. Klicka på **[!UICONTROL Properties]** i verktygsfältet. Du kan också använda kortkommandot `p`.
1. Klicka på **[!UICONTROL Live Copy Source]**. Klicka på **[!UICONTROL Live Copy Overview]** för att öppna konsolen. På den här kontrollpanelen visas status på den översta nivån för alla underordnade resurser.

   ![Visa status för live-kopior i Live Copy-konsolen för källan](assets/livecopy-statuses.png)

   *Figur: Visa status för live-kopior i [!UICONTROL Live Copy Console] källan.*

1. Om du vill visa detaljerad information om alla resurser i mappen med live-kopian markerar du en resurs och klickar på **[!UICONTROL Relationship Status]** i verktygsfältet.

   ![Detaljerad information och status för en underordnad resurs i en mapp](assets/livecopy_relationship_status.png)

   Detaljerad information om och status för en underordnad live-kopia i en mapp

>[!TIP]
>
>Du kan snabbt se status för live-kopior av andra mappar utan att behöva bläddra för mycket. Ändra mappen från den övre mellersta delen av gränssnittet **[!UICONTROL Live Copy Overview]**.

### Snabbåtgärder från referensfältet för källan {#refrailsource}

För en källresurs eller källmapp kan du se följande information och utföra följande åtgärder direkt från referensfältet:

* Se sökvägarna för live-kopior.
* Öppna eller visa en specifik live-kopia i användargränssnittet för [!DNL Experience Manager].
* Synkronisera uppdateringarna till en specifik live-kopia.
* Pausa relationen eller ändra utrullningskonfiguration för en specifik live-kopia.
* Få åtkomst till översiktskonsolen för live-kopian.

Markera källresursen eller källmappen, öppna den vänstra listen och klicka på **[!UICONTROL References]**. Du kan också markera en resurs eller mapp och använda kortkommandot `Alt + 4`.

![Åtgärder och information som är tillgänglig i referensfältet för den valda källan](assets/referencerail_source.png)

*Bild: Åtgärder och information som är tillgänglig i referensfältet för den valda källan.*

Klicka på **[!UICONTROL Edit Live Copy]** om du vill pausa relationen eller ändra utrullningskonfigurationen för en specifik live-kopia.

![För en specifik live-kopia är alternativet att pausa relationen eller ändra utrullningskonfiguration tillgängligt från referenslinjen när källresurs har valts](assets/referencerail_editlc_options.png)

*Bild: Pausa relationen eller ändra utrullningskonfigurationen för en specifik live-kopia.*

### Snabbåtgärder från referensfältet för live-kopia {#ref-rail-lc}

För en resurs eller mapp för en live-kopia kan du se följande information och utföra följande åtgärder direkt från referenslisten:

* Se källans sökväg.
* Öppna eller visa en specifik live-kopia i användargränssnittet för [!DNL Experience Manager].
* Fyll i uppdateringarna.

Välj en resurs eller mapp med en live-kopia, öppna den vänstra rutan och klicka på **[!UICONTROL References]**. Du kan också markera en resurs eller mapp och använda kortkommandot `Alt + 4`.

![Åtgärder som är tillgängliga i referensrutan för den valda live-kopian](assets/referencerail_livecopy.png)

*Bild: Åtgärder som är tillgängliga i referenslinjen för den valda Live-kopian.*

## Sprid ändringar från källa till live-kopior {#rolloutsync}

När en källa har ändrats kan ändringarna spridas till live-kopiorna med hjälp av en synkroniseringsåtgärd eller en utrullningsåtgärd. Mer information om skillnaden mellan de båda åtgärderna finns i [ordlista](#glossary).

### Åtgärd för utrullning {#rollout}

Du kan initiera en utrullningsåtgärd från källresursen och uppdatera alla eller några utvalda live-kopior.

1. Välj en live-kopia eller en mapp. Klicka på **[!UICONTROL Properties]** i verktygsfältet. Du kan också använda kortkommandot `p`.
1. Klicka på **[!UICONTROL Live Copy Source]**. Klicka på **[!UICONTROL Rollout]** i verktygsfältet.
1. Markera de live-kopior som du vill uppdatera. Klicka på **[!UICONTROL Rollout]**.
1. Välj **[!UICONTROL Rollout Source and all Children]** om du vill ta med uppdateringarna av de underordnade resurserna.

   ![Ta bort ändringarna av källan till några eller alla live-kopior](assets/livecopy_rollout_page.png)

   *Bild: Rulla ut ändringarna av källan till några eller alla live-kopior.*

>[!NOTE]
>
>Ändringar som görs i en källresurs rullas endast ut till direkt relaterade live-kopior. Om en live-kopia kommer från en annan live-kopia, rullas inte ändringarna ut till den härledda live-kopian.

Du kan också starta en utrullningsåtgärd från referenslinjen när du har valt en specifik live-kopia. Mer information finns i [Snabbåtgärder från referensfältet för live-kopia](#ref-rail-lc). I den här metoden för utrullning uppdateras endast den markerade live-kopian och eventuellt dess underordnade.

![Rulla ut ändringarna av källan till den markerade aktiva kopian](assets/livecopy_rollout_dialog.png)

*Bild: Rulla ut ändringarna av källan till den markerade Live-kopian.*

### Om synkroniseringsåtgärd {#about-sync}

Med en synkroniseringsåtgärd hämtas ändringarna från en källa endast till den markerade Live-kopian. Synkroniseringsåtgärden respekterar och underhåller lokala ändringar som gjorts efter att arv har annullerats. De lokala ändringarna skrivs inte över och arvet som avbryts återupprättas inte. Du kan initiera en synkroniseringsåtgärd på tre sätt.

| Var i [!DNL Experience Manager]-gränssnittet | När och varför ska du använda | Så här använder du |
|---|---|---|
| [!UICONTROL References] spår | Synkronisera snabbt när du redan har markerat källan. | Se [Snabbåtgärder från referensfältet för källan](#refrailsource) |
| Verktygsfält på sidan [!UICONTROL Properties] | Starta en synkronisering när du redan har live-kopieringsegenskaperna öppna. | Se [Synkronisera en live-kopia](#sync-lc) |
| Konsol för [!UICONTROL Live Copy Overview] | Synkronisera snabbt flera resurser (inte nödvändigtvis alla) när källmappen är markerad eller [!UICONTROL Live Copy Overview]-konsolen redan är öppen. Synkroniseringsåtgärden initieras för en resurs i taget men är ett snabbare sätt att synkronisera flera resurser på en gång. | Se [Åtgärder för många resurser i en live-kopiemapp](#bulk-actions) |

### Synkronisera en live-kopia {#sync-lc}

Om du vill starta en synkroniseringsåtgärd öppnar du sidan **[!UICONTROL Properties]** för en live-kopia, klickar på **[!UICONTROL Live Copy]** och klickar sedan på önskad åtgärd i verktygsfältet.

Om du vill se status och information om en synkroniseringsåtgärd läser du [Information om och status för en live-kopia](#statuslcasset) samt [Information och status för alla live-kopior av en mapp](#status-lc-folder).

![Synkroniseringsåtgärden hämtar de ändringar som gjorts i källan](assets/livecopy_sync.png)

*Bild: Funktionen Synkronisera hämtar de ändringar som har gjorts i källan.*

>[!NOTE]
>
>Om relationen är inaktiverad är synkroniseringsåtgärden inte tillgänglig i verktygsfältet. Synkroniseringsåtgärden är tillgänglig i referensfältet, men ändringarna sprids inte ens när en lyckad utrullning har slutförts.

## Pausa och återuppta relationen {#suspend-resume}

Du kan tillfälligt inaktivera relationen för att förhindra att en live-kopia tar emot ändringar som gjorts i källresursen eller källmappen. Relationen kan även återupptas för live-kopiering för att börja ta emot ändringarna från källan.

Om du vill pausa eller återuppta öppnar du sidan **[!UICONTROL Properties]** för en live-kopia, klickar på **[!UICONTROL Live Copy]** och klickar sedan på önskad åtgärd i verktygsfältet.

Du kan också snabbt pausa eller återuppta relationer för flera resurser i en mapp med live-kopior på konsolen **[!UICONTROL Live Copy Overview]**. Se [Utföra åtgärder för många resurser i mappar med live-kopior](#bulk-actions).

## Göra lokala ändringar i en live-kopia {#local-mods}

En live-kopia är en kopia av den ursprungliga källan när den skapas. Metadatavärdena för en live-kopia ärvs från källan. Metadatafälten behåller enskilt arv med respektive fält i källresursen.

Du kan dock göra lokala ändringar i en live-kopia för att ändra vissa egenskaper. Om du vill göra lokala ändringar avbryter du arvet av den önskade egenskapen. När arvet efter ett eller flera metadatafält avbryts behålls resursens live-relation och arvet efter de andra metadatafälten. Synkronisering eller utrullning skriver inte över lokala ändringar. Om du vill göra det öppnar du sidan **[!UICONTROL Properties]** för en live-kopia av en resurs och klickar på alternativet **[!UICONTROL cancel inheritance]** bredvid ett metadatafält.

Du kan ångra alla lokala ändringar och återställa resursen till källans läge. Återställ åtgärd oåterkalleligt och omedelbart åsidosätter alla lokala ändringar och återupprättar arv på alla metadatafält. Om du vill återgå klickar du på **[!UICONTROL Reset]** från verktygsfältet på sidan **[!UICONTROL Properties]** för en live-kopia.

![Återställningsåtgärden skriver över lokala redigeringar och delar av den aktiva kopian med källan skrivs över.](assets/livecopy_reset.png)

*Bild: Åtgärden Återställ skriver över lokala redigeringar och delar in live-kopian i källfilen.*

## Ta bort liverelation {#detach}

Du kan ta bort relationen mellan en källa och en live-kopia helt med åtgärden Koppla loss. Den aktiva kopian blir en fristående resurs eller mapp när den har kopplats loss. Den visas som en ny resurs i [!DNL Experience Manager]-gränssnittet omedelbart efter frånkoppling. Följ de här stegen för att koppla loss en live-kopia från källan.

1. Välj en resurs eller mapp för en live-kopia. Klicka på **[!UICONTROL Properties]** i verktygsfältet. Du kan också använda kortkommandot `p`.

1. Klicka på **[!UICONTROL Live Copy]**. Klicka på **[!UICONTROL Detach]** i verktygsfältet. Klicka på **[!UICONTROL Detach]** i den dialogruta som visas.

   ![Kopplingsåtgärden tar bort relationen mellan källan och den aktiva kopian helt](assets/livecopy_detach.png)

   *Bild: Kopplingsåtgärden tar bort relationen mellan källan och den aktiva kopian.*

   >[!CAUTION]
   >
   >Relationen tas bort omedelbart när du klickar på **[!UICONTROL Detach]** i dialogrutan. Du kan inte ångra den genom att klicka på **[!UICONTROL Cancel]** på sidan Egenskaper.

Du kan också snabbt frigöra flera resurser i en live-kopieringsmapp från **[!UICONTROL Live Copy Overview]**-konsolen. Se [Utföra åtgärder för många resurser i mappar med live-kopior](#bulk-actions).

## Massåtgärder i en mapp för live-kopior {#bulk-actions}

Om du har flera resurser i en live-kopieringsmapp kan initieringsåtgärder för varje resurs vara långsamma. Du kan snabbt initiera grundläggande åtgärder för många resurser från [!UICONTROL Live Copy Console]. Ovanstående metoder fortsätter att fungera för enskilda resurser.

1. Välj en källmapp. Klicka på **[!UICONTROL Properties]** i verktygsfältet. Du kan också använda kortkommandot `p`.
1. Klicka på **[!UICONTROL Live Copy Source]**. Klicka på **[!UICONTROL Live Copy Overview]** för att öppna konsolen.
1. På den här kontrollpanelen väljer du en live-resurs från en live-mapp. Klicka på önskade åtgärder i verktygsfältet. De tillgängliga åtgärderna är **[!UICONTROL Synchronize]**, **[!UICONTROL Reset]**, **[!UICONTROL Suspend]** och **[!UICONTROL Detach]**. Du kan snabbt initiera dessa åtgärder för alla resurser i valfritt antal kopiamappar som finns i en direktrelation med den valda källmappen.

   ![Uppdatera enkelt många resurser i live-kopiemappar från Live Copy-översiktskonsolen](assets/livecopyconsole_update_many_assets.png)

   *Bild: Uppdatera enkelt många resurser i live-kopiemappar från [!UICONTROL Live Copy Overview]-konsolen.*

## Effekter av tillgångshanteringsåtgärder på live-kopior {#manage-assets}

Live-kopior och källor är resurser eller mappar som i viss utsträckning kan hanteras som digitala resurser. Vissa resurshanteringsaktiviteter i [!DNL Experience Manager] har en specifik effekt på livekopiorna.

* När du kopierar en live-kopia skapas en live-kopia med samma källa som den första live-kopian.
* När du flyttar en källa eller dess livekopia behålls relationen.
* Redigeringsåtgärden fungerar inte för live-kopieringsresurser. Om källan till en live-kopia är en live-kopia i sig, fungerar inte redigeringsåtgärden för den.
* Utcheckningsåtgärden är inte tillgänglig för livekopieringsresurser.
* För källmappen är alternativet att skapa granskningsåtgärder tillgängligt.
* När du visar resursexemplet i listvyn och kolumnvyn visas live-kopia av en resurs eller mapp. Det gör det enklare att identifiera live-kopior i en mapp.

## Jämför MSM för [!DNL Assets] och [!DNL Sites] {#comparison}

I fler scenarier matchar MSM för [!DNL Assets] beteendet hos MSM för platsfunktioner. Några viktiga skillnader är:

* Blueprint i MSM för [!DNL Sites] kallas Live Copy-källa i MSM för [!DNL Assets].
* I Sites kan du jämföra en utkast och dess live-kopia, men det är inte möjligt i [!DNL Assets] att jämföra en källa med dess live-kopia.
* Du kan inte redigera en live-kopia i [!DNL Assets].
* Platser har vanligtvis underordnade, men det har inte [!DNL Assets]. Alternativet att inkludera eller exkludera underordnade objekt finns inte när du skapar live-kopior av enskilda resurser.
* Det går inte att ta bort kapitelsteget i guiden Skapa plats i MSM för [!DNL Assets].
* Konfigurering av MSM-lås på sidegenskaper (användargränssnitt med pekfunktioner) stöds inte i MSM för [!DNL Assets].
* Använd bara **[!UICONTROL Standard rollout config]** för MSM för [!DNL Assets]. De andra rollout-konfigurationerna är inte tillgängliga för MSM för [!DNL Assets].

## God praxis {#best-practices}

Några av de bästa sätten att använda MSM är:

* Planera relationer mellan överordnade och underordnade för resurser och innehåll innan implementeringen startas.

## Begränsningar och kända problem med MSM för [!DNL Assets] {#limitations}

Följande är en begränsning för MSM för [!DNL Assets].

* Content Fragments (CF) stöds inte för användningsfallet. När de försöker skapa sina live-kopior kopieras CF-filer som de är utan någon relation. De kopierade CF:erna är en ögonblicksbild i tid och uppdateras inte när de ursprungliga CF:erna uppdateras.

* MSM fungerar inte när återkoppling av metadata är aktiverat. Vid tillbakaskrivning avbryts arvet.
