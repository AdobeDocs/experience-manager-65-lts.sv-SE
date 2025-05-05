---
title: Lägg till Dynamic Media Classic-funktioner (Scene7) på sidan
description: Adobe Dynamic Media Classic (Scene7) är en värdbaserad lösning för att hantera, förbättra, publicera och leverera multimediematerial för webben, mobiler, e-post och internetanslutna skärmar och för tryck.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: authoring
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: e452c343-3bba-4774-b153-c5ba05f24362
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3409'
ht-degree: 1%

---

# Lägg till Dynamic Media Classic-funktioner (Scene7) på sidan{#adding-scene-features-to-your-page}

[Adobe Dynamic Media Classic (Scene7)](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/home.html) är en värdlösning för att hantera, förbättra, publicera och leverera mediefiler till webb, mobiler, e-post och Internet-anslutna skärmar och utskrifter.

Du kan visa Experience Manager-resurser som publicerats i Dynamic Media Classic (Scene7) i olika visningsprogram:

* Zooma
* Utfällbar
* Video
* Bildmall
* Bild

Du kan publicera digitala resurser direkt från Experience Manager till Dynamic Media Classic (Scene7) och du kan publicera digitala resurser från Dynamic Media Classic (Scene7) till Experience Manager.

I det här dokumentet beskrivs hur du publicerar digitala resurser från Experience Manager till Dynamic Media Classic (Scene7) och omvänt. Visningsprogrammen beskrivs också i detalj. Mer information om hur du konfigurerar Experience Manager för Dynamic Media Classic (Scene7) finns i [Integrera Dynamic Media Classic (Scene7) med Experience Manager](/help/sites-administering/scene7.md).

Se även [Lägg till bildscheman](/help/assets/image-maps.md).

Mer information om hur du använder videokomponenter med Experience Manager finns i:

* [Video](/help/sites-classic-ui-authoring/manage-assets-classic-s7-video.md)

>[!NOTE]
>
>Om Dynamic Media Classic (Scene7)-resurser inte visas som de ska kontrollerar du att Dynamic Media är [inaktiverat](/help/assets/config-dynamic.md#disabling-dynamic-media) och uppdaterar sedan sidan.

## Publicera manuellt till Dynamic Media Classic (Scene7) från Assets {#manually-publishing-to-scene-from-assets}

Du kan publicera digitala resurser till Dynamic Media Classic (Scene7) antingen från Assets-konsolen i det klassiska användargränssnittet eller direkt från resursen.

>[!NOTE]
>
>Experience Manager publicerar till Dynamic Media Classic (Scene7) asynkront. När du har valt **[!UICONTROL Publish]** kan det ta flera sekunder för resursen att publicera till Dynamic Media Classic (Scene7).
>

### Publicera från Assets Console {#publishing-from-the-assets-console}

Du kan publicera till Dynamic Media Classic (Scene7) från Assets-konsolen om resurserna finns i en Dynamic Media Classic-målmapp (Scene7).

1. I det klassiska användargränssnittet för Experience Manager väljer du **[!UICONTROL Digital Assets]** för att komma åt den digitala resurshanteraren.

1. Markera resursen (eller resurserna) eller mappen i målmappen som du vill publicera till Dynamic Media Classic (Scene7), högerklicka och välj **[!UICONTROL Publish to Dynamic Media Classic (Scene7)]**. Du kan också välja **[!UICONTROL Publish to Dynamic Media Classic (Scene7)]** på menyn **[!UICONTROL Tools]**.

   ![chlimage_1-48](assets/chlimage_1-48.png)

1. Gå till Dynamic Media Classic (Scene7) och bekräfta att resurserna är tillgängliga.

   >[!NOTE]
   >
   >Om resurserna inte finns i en synkroniserad Dynamic Media Classic-mapp (Scene7) visas **[!UICONTROL Publish to Dynamic Media Classic (Scene7)]** på båda menyerna, men inaktiveras.

### Publicera från en resurs {#publishing-from-an-asset}

Du kan publicera en resurs manuellt så länge som resursen finns inuti den synkroniserade Dynamic Media Classic-mappen (Scene7).

>[!NOTE]
>
>Om resursen inte finns i den synkroniserade mappen Dynamic Media Classic (Scene7) visas inte länken till **[!UICONTROL Publish to Dynamic Media Classic (Scene7)]**.

Så här publicerar du till Dynamic Media Classic (Scene7) direkt från en digital resurs:

1. I Experience Manager väljer du **[!UICONTROL Digital Assets]** för att komma åt den digitala resurshanteraren.

1. Dubbelklicka för att öppna en resurs.

1. Välj **[!UICONTROL Publish to Dynamic Media Classic (Scene7)]** i rutan med resursinformation.

   ![screen_shot_2012-02-22at34828pm](assets/screen_shot_2012-02-22at34828pm.png)

1. Länken ändras till **[!UICONTROL Publishing ...]** och sedan **[!UICONTROL Published]**. Gå till Dynamic Media Classic (Scene7) och bekräfta att resursen är tillgänglig.

   >[!NOTE]
   >
   >Om resursen inte publiceras korrekt på Dynamic Media Classic (Scene7) ändras länken till **[!UICONTROL Publishing Failed]**. Om resursen redan har publicerats till Dynamic Media Classic (Scene7) läses **[!UICONTROL Re-Publish to Dynamic Media Classic (Scene7)]** av länken. Med ompublicering kan du ändra resurser i Experience Manager och publicera dem igen.

### Publicera resurser utanför CQ-målmappen {#publishing-assets-from-outside-the-cq-target-folder}

Adobe rekommenderar att du endast publicerar resurser till Dynamic Media Classic (Scene7) från resurser i målmappen för Dynamic Media Classic (Scene7). Om du däremot måste överföra resurser från en mapp utanför målmappen kan du göra det genom att överföra dem till en on-demand-mapp på Dynamic Media Classic (Scene7). Först konfigurerar du molnkonfigurationen för sidan där du vill att resursen ska visas. Sedan lägger du till en Dynamic Media Classic-komponent (Scene7) på sidan och drar och släpper en resurs på komponenten. När sidegenskaperna har angetts för den sidan visas en **[!UICONTROL Publish to Dynamic Media Classic (Scene7)]**-länk som utlöser överföring till Dynamic Media Classic (Scene7) när den har valts.

>[!NOTE]
>
>Assets som finns i on-demand-mappen visas inte i Dynamic Media Classic (Scene7) Content Browser.

**Så här publicerar du resurser utanför CQ-målmappen:**

1. I Experience Manager i det klassiska användargränssnittet väljer du **[!UICONTROL Websites]** och navigerar till den webbsida där du vill lägga till en digital resurs som ännu inte har publicerats i Dynamic Media Classic (Scene7). (Normala sidarvsregler gäller.)

1. I sidosparken markerar du ikonen **[!UICONTROL Page]** och väljer **[!UICONTROL Page Properties]**.

1. Välj **[!UICONTROL Cloud Services]**.
1. Välj **[!UICONTROL Add services]**.
1. Välj **[!UICONTROL Dynamic Media Classic (Scene7)]**.
1. Välj önskad konfiguration i listrutan **[!UICONTROL Adobe Dynamic Media Classic (Scene7)]** och välj **[!UICONTROL OK]**.

   ![chlimage_1-49](assets/chlimage_1-49.png)

1. Lägg till en Dynamic Media Classic-komponent (Scene7) på önskad plats på webbsidan.
1. Dra en digital resurs från innehållssökaren till komponenten. Du ser en länk till **[!UICONTROL Check Dynamic Media Classic (Scene7) Publication Status]**.

   >[!NOTE]
   >
   >Om den digitala resursen finns i CQ-målmappen visas ingen länk till **[!UICONTROL Check Dynamic Media Classic (Scene7) Publication Status]**. Resurserna placeras i komponenten.

   ![chlimage_1-50](assets/chlimage_1-50.png)

1. Välj **[!UICONTROL Check Dynamic Media Classic (Scene7) Publication Status]**. Om resurserna inte publiceras publicerar Experience Manager resursen till Dynamic Media Classic (Scene7). När resursen har överförts finns den i mappen on-demand. Som standard finns mappen on demand i mappen **[!UICONTROL name_of_the_company/CQ5_adhoc]**. Du kan [konfigurera on demand-mappen om det behövs](#configuringtheadhocfolder).

   >[!NOTE]
   >
   >Om resursen inte finns i en synkroniserad Dynamic Media Classic-mapp (Scene7) och det inte finns någon Dynamic Media Classic (Scene7)-molnkonfiguration kopplad till den aktuella sidan misslyckas överföringen.

## Dynamic Media Classic-komponenter (Scene7) {#scene-components}

Följande Dynamic Media Classic-komponenter (Scene7) är tillgängliga i Experience Manager:

* Zooma
* Utfällbar (zoom)
* Bildmall
* Bild
* Video

>[!NOTE]
>
>De här komponenterna är inte tillgängliga som standard och måste markeras i designläge innan du kan använda dem.

När de är tillgängliga i designläge kan du lägga till komponenterna på sidan precis som andra Experience Manager-komponenter. Assets som ännu inte har publicerats till Dynamic Media Classic (Scene7) publiceras till Dynamic Media Classic (Scene7) om de finns i en synkroniserad mapp, på en sida eller med en Dynamic Media Classic (Scene7) Cloud Configuration.

>[!NOTE]
>
>Om du skapar och utvecklar anpassade S7-visningsprogram och använder Content Finder måste du lägga till parametern `allowfullscreen` explicit.

### Meddelande om att Flash-visningsprogrammet har upphört att gälla {#flash-viewers-end-of-life-notice}

Från och med den 31 januari 2017 upphörde Adobe Dynamic Media Classic (Scene7) officiellt stödet för Flash-visningsprogramplattformen.

### Lägga till en Dynamic Media Classic-komponent (Scene7) på en sida {#adding-a-scene-component-to-a-page}

Att lägga till en Dynamic Media Classic-komponent (Scene7) på en sida är detsamma som att lägga till en komponent på en sida. Dynamic Media Classic-komponenter (Scene7) beskrivs i detalj i följande avsnitt.

Så här lägger du till en Dynamic Media Classic-komponent (Scene7)/visningsprogram på en sida i det klassiska användargränssnittet:

1. Öppna den sida där du vill lägga till Dynamic Media Classic-komponenten (Scene7) i Experience Manager.

1. Om det inte finns några Dynamic Media Classic-komponenter (Scene7) markerar du linjalen i sidosparken för att öppna **designläget**, väljer **[!UICONTROL Edit]** och väljer alla **[!UICONTROL Dynamic Media Classic (Scene7)]** -komponenter för att göra dem tillgängliga.

1. Gå tillbaka till **redigeringsläget** genom att välja pennan i sidosparken.

1. Dra en komponent från gruppen **[!UICONTROL Dynamic Media Classic (Scene7)]** i sidosparken till sidan på önskad plats.

1. Välj ***[!UICONTROL Edit]** så att du kan öppna komponenten.

1. Redigera komponenten efter behov och välj **[!UICONTROL OK]** för att spara ändringarna.

### Lägga till interaktiva tittarupplevelser på en responsiv webbplats {#adding-interactive-viewing-experiences-to-a-responsive-website}

Responsiv design för dina resurser innebär att dina resurser anpassas beroende på var de visas. Med responsiv design kan samma resurser visas effektivt på flera enheter.

Så här lägger du till en interaktiv visningsupplevelse på en responsiv webbplats i det klassiska användargränssnittet:

1. Logga in på Experience Manager och kontrollera att du har [konfigurerat Adobe Dynamic Media Classic (Scene7) Cloud Services](/help/sites-administering/scene7.md#configuring-scene-integration) och att Dynamic Media Classic-komponenterna (Scene7) är tillgängliga.

   >[!NOTE]
   >
   >Om Dynamic Media Classic (Scene7) WCM-komponenter inte är tillgängliga måste du aktivera dem i designläge.

1. På en webbplats där Dynamic Media Classic-komponenterna (Scene7) är aktiverade drar du ett **[!UICONTROL Image]**-visningsprogram till sidan.
1. Redigera komponenten och justera brytpunkterna på fliken **[!UICONTROL Dynamic Media Classic (Scene7) Settings]**.

   ![chlimage_1-51](assets/chlimage_1-51.png)

1. Bekräfta att tittarna ändrar storlek rejält och att alla interaktioner är optimerade för datorer, surfplattor och mobiler.

### Gemensamma inställningar för alla Dynamic Media Classic-komponenter (Scene7) {#settings-common-to-all-scene-components}

Även om konfigurationsalternativen varierar är följande vanligt för alla Dynamic Media Classic-komponenter (Scene7):

* **Filreferens**- Bläddra till en fil som du vill referera till. Filreferensen visar resurs-URL:en och inte nödvändigtvis den fullständiga Dynamic Media Classic-URL:en (Scene7) inklusive URL-kommandon och -parametrar. Du kan inte lägga till URL-kommandon och parametrar för Dynamic Media Classic (Scene7) i det här fältet. De måste i stället läggas till via motsvarande funktioner i komponenten.
* **Bredd** - Ange bredden.
* **Höjd** - Du kan ange höjd.

Du anger dessa konfigurationsalternativ genom att öppna (dubbelklicka) en Dynamic Media Classic-komponent (Scene7), till exempel när du öppnar en **Zoom** -komponent:

![chlimage_1-52](assets/chlimage_1-52.png)

### Zooma {#zoom}

HTML5 Zoom-komponenten visar en större bild när du trycker på +-knappen.

Resursen har zoomverktyg längst ned. Markera **[!UICONTROL +]** om du vill förstora. Välj **[!UICONTROL -]** om du vill minska. Om du väljer **[!UICONTROL x]** eller zoompilen återställs bilden till den ursprungliga storlek som den importerades som. Markera de diagonala pilarna så att du kan göra den i helskärmsläge. Välj **[!UICONTROL Edit]** så att du kan konfigurera komponenten. Med den här komponenten kan du konfigurera [inställningar som är gemensamma för alla Dynamic Media Classic-komponenter (Scene7)](#settings-common-to-all-scene-components).

![Bild på tulpanblommor i HTML5 Zoom-komponenten.](do-not-localize/chlimage_1-3.png)

### Utfällbar {#flyout}

I den utfällbara HTML5-komponenten visas resursen som en delad skärm. Lämna resursen i den angivna storleken. Höger visas zoomdelen. Välj **[!UICONTROL Edit]** så att du kan konfigurera komponenten. Med den här komponenten kan du konfigurera [inställningar som är gemensamma för alla Dynamic Media Classic-komponenter (Scene7)](/help/sites-administering/scene7.md#settingscommontoallscene7components).

>[!NOTE]
>
>Om den utfällbara komponenten använder en anpassad storlek, används den anpassade storleken och responsiv konfiguration av komponenten inaktiveras.
>
>Om den utfällbara komponenten använder standardstorleken, enligt inställningarna i designvyn, används standardstorleken. Komponenten sträcks ut så att den passar sidlayoutens storlek med responsiv konfiguration av komponenten aktiverad. Tänk dock på att det finns en begränsning för responsiv konfiguration av komponenten. När du använder den utfällbara komponenten med responsiv konfiguration bör du inte använda den med full sidsträckning. I annat fall kan den utfällbara menyn sträcka sig utanför sidans högra kant.

![chlimage_1-53](assets/chlimage_1-53.png)

### Bild {#image}

Med bildkomponenten i Dynamic Media Classic (Scene7) kan du lägga till Dynamic Media Classic-funktioner (Scene7) i dina bilder, till exempel Dynamic Media Classic-modifierare (Scene7), bild- eller visningsinställningar samt skärpa. Dynamic Media Classic-bildkomponenten (Scene7) liknar andra bildkomponenter i Experience Manager med speciella Dynamic Media Classic-funktioner (Scene7). I det här exemplet används URL-modifieraren för Dynamic Media Classic (Scene7), `&op_invert=1`.

![Bild på en sfär i Dynamic Media Classic-bildkomponenten (Scen 7)](do-not-localize/chlimage_1-4.png)

**Titel, Alt-text** - På fliken Avancerat lägger du till en titel i bilden och alternativ text för de användare som har grafik inaktiverad.

**URL, Öppna i** - Du kan ange en resurs från för att öppna en länk. Ange URL:en och Öppna i anger om du vill att den ska öppnas i samma fönster eller i ett nytt fönster.

![chlimage_1-54](assets/chlimage_1-54.png)

**Visningsförinställning** - Välj en befintlig visningsförinställning i listrutan. Om den visningsförinställning du söker efter inte visas måste du göra den synlig. Se Hantera förinställningar för visningsprogram. Du kan inte välja en visningsförinställning om du använder en bildförinställning och omvänt.

**Dynamic Media Classic (Scene7) Configuration** - Välj den Dynamic Media Classic-konfiguration (Scene7) som du vill använda för att hämta aktiva bildförinställningar från SPS.

**Bildförinställning** - Välj en befintlig bildförinställning i listrutan. Om den bildförinställning du söker inte syns måste du göra den synlig. Se Hantera bildförinställningar. Du kan inte välja en visningsförinställning om du använder en bildförinställning och omvänt.

**Utdataformat** - Välj bildens utdataformat, till exempel jpeg. Beroende på vilket utdataformat du väljer kan det finnas ytterligare konfigurationsalternativ. Mer information finns i Bästa tillvägagångssätt för förinställda bilder.

**Skärpa** - Välj hur du vill öka skärpan i bilden. Skärpa förklaras i detalj i Bästa praxis för bildförinställningar och Bästa praxis för skärpa.

**URL-modifierare** - Du kan ändra bildeffekter genom att ange ytterligare S7-bildkommandon. Dessa kommandon beskrivs i Bildförinställningar och Kommandoreferensen.

**Brytpunkter** - Om webbplatsen är responsiv vill du justera brytpunkterna. Brytpunkter måste avgränsas med kommatecken (,).

### Bildmall {#image-template}

Dynamic Media Classic (Scene7) Bildmallar är Photoshop-innehåll i lager som importerats till Dynamic Media Classic (Scene7), där innehåll och egenskaper parametriserades för variabilitet. Med komponenten **[!UICONTROL Image template]** kan du importera bilder och ändra texten dynamiskt i Experience Manager. Dessutom kan du konfigurera komponenten **[!UICONTROL Image template]** så att värden från klientkontexten används, så att varje användare upplever bilden på ett personligt sätt.

Välj **[!UICONTROL Edit]** - för att konfigurera komponenten. Du kan konfigurera [inställningar som är gemensamma för alla Dynamic Media Classic-komponenter (Scene7)](/help/sites-administering/scene7.md#settingscommontoallscene7components) och andra inställningar som beskrivs i det här avsnittet.

![chlimage_1-55](assets/chlimage_1-55.png)

**Filreferens, bredd, höjd** - Se [inställningar som är gemensamma för alla Dynamic Media Classic-komponenter (Scene7)](/help/sites-administering/scene7.md#settingscommontoallscene7components).

>[!NOTE]
>
>Dynamic Media Classic (Scene7) URL-kommandon och -parametrar kan inte läggas till direkt i filreferensens URL. De kan bara definieras i komponentgränssnittet på panelen **[!UICONTROL Parameter]**.

**Titel, Alt-text** - På fliken Bildmall för Dynamic Media Classic (Scene7) lägger du till en titel i bilden och alternativ text för de användare som har grafik inaktiverad.

**URL, Öppna i** - Du kan ange en resurs från för att öppna en länk. Ange URL:en och Öppna i anger om du vill att den ska öppnas i samma fönster eller i ett nytt fönster.

![chlimage_1-56](assets/chlimage_1-56.png)

**Parameterpanel** - Vid import av en bild fylls parametrarna i med information från bilden. Om det inte finns något innehåll som kan ändras dynamiskt är det här fönstret tomt.

![chlimage_1-57](assets/chlimage_1-57.png)

#### Ändra text dynamiskt {#changing-text-dynamically}

Om du vill ändra texten dynamiskt anger du ny text i fälten och väljer **[!UICONTROL OK]**. I det här exemplet är **Price** nu $50 och frakten 99 cent.

![chlimage_1-58](assets/chlimage_1-58.png)

Texten i bilden ändras. Du kan återställa texten till det ursprungliga värdet genom att välja **[!UICONTROL Reset]** bredvid fältet.

![chlimage_1-59](assets/chlimage_1-59.png)

#### Ändra text så att värdet för en klientkontext återspeglas {#changing-text-to-reflect-the-value-of-a-client-context-value}

Om du vill länka ett fält till ett klientkontextvärde väljer du **[!UICONTROL Select]** för att öppna klientsnabbmenyn, markerar klientkontexten och väljer **[!UICONTROL OK]**. I det här exemplet ändras namnet baserat på att namnet länkas till det formaterade namnet i profilen.

![chlimage_1-60](assets/chlimage_1-60.png)

Texten återspeglar namnet på den inloggade användaren. Du kan återställa texten till det ursprungliga värdet genom att välja **[!UICONTROL Reset]** bredvid fältet.

![chlimage_1-61](assets/chlimage_1-61.png)

#### Gör Dynamic Media Classic-bildmallen (Scene7) till en länk {#making-the-scene-image-template-a-link}

Du kan göra Dynamic Media Classic-bildmallskomponenten (Scene7) till en klickbar länk.

1. Välj **[!UICONTROL Edit]** på sidan med Dynamic Media Classic-bildmallskomponenten (Scene7).
1. I fältet **[!UICONTROL URL]** anger du den URL som användarna går till när de klickar på bilden. I fältet **[!UICONTROL Open in]** väljer du om du vill att målet ska öppnas (ett nytt fönster eller samma fönster).

   ![chlimage_1-62](assets/chlimage_1-62.png)

1. Välj **[!UICONTROL OK]**.

### Videokomponent {#video-component}

Komponenten Dynamic Media Classic (Scene7) **[!UICONTROL Video]** (tillgänglig från sektionen Dynamic Media Classic (Scene7) i sidsparken) använder enhets- och bandbreddsidentifiering för att leverera rätt video till varje skärm. Den här komponenten är en HTML5-videospelare. Den är ett enda visningsprogram som kan användas över flera kanaler.

Den kan användas för adaptiva videouppsättningar, en enda MP4-video eller en enda F4V-video.

Mer information om hur videofilmer fungerar med Dynamic Media Classic-integrering (Scene7) finns i [Video](/help/sites-classic-ui-authoring/manage-assets-classic-s7-video.md). Se även hur [Dynamic Media Classic-videokomponenten **(Scene7)** jämförs med **videokomponenten**](/help/sites-classic-ui-authoring/manage-assets-classic-s7-video.md) som utgör grunden.

![chlimage_1-63](assets/chlimage_1-63.png)

### Kända begränsningar för videokomponenten {#known-limitations-for-the-video-component}

Adobe DAM och WCM visar om en primär källvideo har överförts. De visar inte följande proxyresurser:

* Dynamic Media Classic (Scene7)-kodade återgivningar
* Dynamic Media Classic (Scene7) adaptiva videouppsättningar

När du använder en adaptiv videouppsättning med videokomponenten i Dynamic Media Classic (Scene7) måste komponentens storlek ändras så att den passar videofilens mått.

## Dynamic Media Classic (Scene7) content browser {#scene-content-browser}

Med Dynamic Media Classic (Scene7) webbläsare kan du visa innehåll från Dynamic Media Classic (Scene7) direkt i Experience Manager. Om du vill få åtkomst till innehållsläsaren väljer du **Dynamic Media Classic (Scene7)** i det pekoptimerade användargränssnittet eller **S7** -ikonen i det klassiska användargränssnittet. Funktionen är identisk mellan båda användargränssnitten.

Om du har flera konfigurationer visar Experience Manager som standard [standardkonfigurationen](/help/sites-administering/scene7.md#configuring-a-default-configuration). Du kan välja olika konfigurationer direkt i Dynamic Media Classic (Scene7) innehållsläsare i listrutan.

>[!NOTE]
>
>* Assets i on-demand-mappen visas inte i Dynamic Media Classic (Scene7) innehållsläsare.
>* När [Säker förhandsvisning är aktiverat](/help/sites-administering/scene7.md#configuring-the-state-published-unpublished-of-assets-pushed-to-scene) visas både publicerade och opublicerade resurser på Dynamic Media Classic (Scene7) i innehållsläsaren i Dynamic Media Classic (Scene7).
>* Om du inte ser **[!UICONTROL Dynamic Media Classic (Scene7)]** eller ikonen **[!UICONTROL S7]** som ett alternativ i webbläsaren måste du [konfigurera Dynamic Media Classic (Scene7) så att det fungerar med Experience Manager](/help/sites-administering/scene7.md).
>* För video har Dynamic Media Classic (Scene7) innehållsläsare stöd för:
>   * Adaptiva videouppsättningar: behållare för alla videoåtergivningar som behövs för sömlös uppspelning på flera skärmar
>   * Enkel MP4-video
>   * Enkelt F4V-videoklipp

### Bläddra i innehåll {#browsing-content-in-the-classic-ui}

Bläddra bland innehåll i Dynamic Media Classic (Scene7) genom att välja fliken **[!UICONTROL S7]**.

Du kan ändra konfigurationen som du använder genom att välja konfigurationen. Mapparna ändras beroende på den valda konfigurationen.

![chlimage_1-64](assets/chlimage_1-64.png)

Precis som med innehållssökaren för Assets kan du söka efter resurser och filtrera resultat. Till skillnad från Assets Finder börjar filnamnet **med** strängen som du angav när du anger ett nyckelord på fliken **S7**, i stället för att **innehålla** nyckelordet i filnamnet.

Som standard visas resurser efter filnamn. Du kan även filtrera resultat efter resurstyp.

>[!NOTE]
>
>För video har Dynamic Media Classic (Scene7) Content Browser i WCM stöd för:
>
>* Adaptiva videouppsättningar: behållare för alla videoåtergivningar som behövs för sömlös uppspelning på flera skärmar
>* Enkel MP4-video
>* Enkelt F4V-videoklipp
>

### Sök efter Dynamic Media Classic-resurser (Scene7) med hjälp av innehållsläsaren {#searching-for-scene-assets-with-the-content-browser}

Att söka efter Dynamic Media Classic-resurser (Scene7) påminner om att söka efter Experience Manager-resurser. Undantaget är att när du söker ser du en fjärrvy över resurserna i Dynamic Media Classic-systemet (Scene7) i stället för att importera dem direkt till Experience Manager.

Du kan använda det klassiska användargränssnittet eller det pekoptimerade användargränssnittet för att både visa och söka efter resurser. Beroende på gränssnittet är sökningen något annorlunda.

När du söker i något av användargränssnitten kan du filtrera efter följande villkor (visas här i det pekoptimerade användargränssnittet):

**Ange nyckelord** - Du kan söka efter resurser efter namn. När du söker efter nyckelorden anger du vilket filnamn som börjar med. Om du t.ex. skriver ordet &quot;simning&quot; söker du efter alla resursfilnamn som börjar med de bokstäverna i den ordningen. Var noga med att välja `Enter` när du har skrivit in termen för att hitta resursen.

![chlimage_1-65](assets/chlimage_1-65.png)

**Mapp/sökväg** - Mappens namn baseras på den konfiguration som du har valt. Du kan gå ned till lägre nivåer genom att markera mappikonen och markera en undermapp. Markera sedan markeringen för att markera den.

Om du anger ett nyckelord och väljer en mapp söker Experience Manager igenom den mappen och eventuella undermappar. Om du inte anger några nyckelord när du söker efter, visas bara resurserna i den mappen om du markerar mappen, och inga undermappar inkluderas.

Som standard söker Experience Manager igenom den markerade mappen och alla undermappar.

![chlimage_1-66](assets/chlimage_1-66.png)

**Typ av resurs** - Välj Dynamic Media Classic (Scene7) om du vill bläddra i Dynamic Media Classic-innehåll (Scene7). Det här alternativet är bara tillgängligt om Dynamic Media Classic (Scene7) har konfigurerats.

![chlimage_1-67](assets/chlimage_1-67.png)

**Konfiguration** - Om fler än en Dynamic Media Classic-konfiguration (Scene7) har definierats i molntjänster kan du välja den här. Därför ändras mappen baserat på den konfiguration du väljer.

![chlimage_1-68](assets/chlimage_1-68.png)

**Resurstyp** - I webbläsaren Dynamic Media Classic (Scene7) kan du filtrera resultaten så att de omfattar något av följande: bilder, mallar, videor och anpassningsbara videouppsättningar. Om du inte väljer någon resurstyp söker Experience Manager som standard i alla resurstyper.

![chlimage_1-69](assets/chlimage_1-69.png)

>[!NOTE]
>
>* I det klassiska användargränssnittet kan du även söka efter **Flash** och **FXG**. Det går inte att filtrera efter dessa två termer i det pekoptimerade användargränssnittet.
>
>* När du söker efter video söker du efter en enskild återgivning. Resultatet returnerar den ursprungliga återgivningen (endast &#42;.mp4) och den kodade återgivningen.
>* När du söker i en adaptiv videouppsättning söker du i mappen och i alla undermappar, men bara om du har lagt till ett nyckelord i sökningen. Om du inte har lagt till något nyckelord söker Experience Manager inte igenom undermapparna.
>

**Publiceringsstatus** - Du kan filtrera efter resurser baserat på publiceringsstatus: Opublicerad eller Publicerad. Om du inte väljer någon publiceringsstatus söker Experience Manager som standard igenom alla publiceringsstatusar.

![chlimage_1-70](assets/chlimage_1-70.png)
