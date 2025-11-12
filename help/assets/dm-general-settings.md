---
title: Konfigurera allmänna inställningar för dynamiska media
description: Lär dig hantera allmänna inställningar i Dynamic Media. Du kan ange namnet på publiceringsservern och namnet på den ursprungliga servern här och ange ett alternativ för att skriva över bilder. Det finns också standardalternativ för överföring av oskarp maskering av bilder och överföringsalternativ för hur du vill bearbeta PostScript-, Adobe Photoshop-, PDF- och Adobe Illustrator-filer.
feature: Image Profiles
role: User, Admin
mini-toc-levels: 4
solution: Experience Manager, Experience Manager Assets
exl-id: 99cd5f46-f1aa-46f5-b112-311724e00490
source-git-commit: 298d0505a6c0561202e77008b3c352066d4e72b7
workflow-type: tm+mt
source-wordcount: '2307'
ht-degree: 0%

---

# Konfigurera allmänna inställningar för dynamiska media

Konfigurationen av **[!UICONTROL Dynamic Media General Settings]** är bara tillgänglig om:

* Du kör Dynamic Media i Scene7-läge. Se [Aktivera dynamiska media i Scene7-läge](/help/assets/config-dms7.md#enabling-dynamic-media-in-scene-mode).
* Du har en *befintlig* **[!UICONTROL Dynamic Media Configuration]** (i **[!UICONTROL Cloud Services]**). Se [Skapa en dynamisk mediekonfiguration i molntjänster](/help/assets/config-dms7.md#configuring-dynamic-media-cloud-services).
* Du är systemadministratör för Experience Manager med administratörsbehörighet.

Dynamic Media General Settings är avsedd att användas av erfarna webbplatsutvecklare och programmerare. Adobe Dynamic Media rekommenderar användare som ändrar dessa publiceringsinställningar att känna till Dynamic Media i Adobe Experience Manager och grundläggande bildbehandlingsteknik.

När du skapar ett konto tillhandahåller Adobe Dynamic Media automatiskt de servrar du har tilldelats. De här servrarna används för att skapa URL-strängar för din webbplats och dina program. Dessa URL-anrop är specifika för ditt konto.

På sidan Dynamic Media Publish Setup anges standardinställningar som avgör hur resurser levereras från Adobe Dynamic Media-servrar till webbplatser eller program. Om ingen inställning har angetts levererar Adobe Dynamic Media-servern en resurs enligt en standardinställning som har konfigurerats på sidan Dynamic Media Publish Setup.

Se även [Valfritt - Konfigurera och konfigurera Dynamic Media - Scene7-lägesinställningar](/help/assets/config-dms7.md#optional-setup-and-configuration-of-dynamic-media-scene7-mode-settings) för fler valfria konfigurationsuppgifter.

>[!NOTE]
>
>Vill du uppgradera från Dynamic Media Classic till Dynamic Media på Adobe Experience Manager? Sidan Allmänna inställningar och sidan [Publiceringsinställningar](/help/assets/dm-publish-settings.md) i Dynamic Media är förifyllda med värdena från ditt Dynamic Media Classic-konto. Undantag är alla värden som listas under **[!UICONTROL Default upload options]** på sidan Allmänna inställningar. Värdena finns redan i Experience Manager. Alla ändringar du gör under **[!UICONTROL Default upload options]** på någon av de fem flikarna, via Experience Manager användargränssnitt, visas i Dynamic Media, inte i Dynamic Media Classic. Alla andra inställningar och värden på sidan Allmänna inställningar och på sidan [Publiceringsinställningar](/help/assets/dm-publish-settings.md) underhålls mellan Dynamic Media Classic och Dynamic Media på Experience Manager.

**Så här konfigurerar du allmänna inställningar för dynamiska media:**

1. I Experience Manager Author mode väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen.
1. Välj verktygsikonen i den vänstra listen och gå sedan till **[!UICONTROL Assets]** > **[!UICONTROL Dynamic Media General Settings]**.
1. Ange **[!UICONTROL Published Server Name]** och **[!UICONTROL Origin Server Name]** på serversidan och använd sedan de fem flikarna för att konfigurera standardalternativ för överföring för bildredigering och för PostScript-, Photoshop-, PDF- och Illustrator-filer.

   * [Server](#server-general-setting)
   * [Överför till program](#upload-to-application)
   * Fliken [Bildredigering](#image-editing-tab)
   * Fliken [PostScript](#postscript-tab)
   * Fliken [Photoshop](#photoshop-tab)
   * Fliken [PDF](#pdf-tab)
   * Fliken [Illustrator](#illustrator-tab)

   ![Sidan Allmänna inställningar för dynamiska media](/help/assets/assets-dm/dm-general-settings.png)
   *Sidan Allmänna inställningar för dynamiska media, med fliken **[!UICONTROL Image Editing]**&#x200B;markerad.*<br><br>

1. När du är klar väljer du **[!UICONTROL Save]** i sidans övre högra hörn.

## Server {#server-general-setting}

När du skapar ett konto tillhandahåller Adobe Dynamic Media automatiskt de servrar du har tilldelats. De här servrarna används för att skapa URL-strängar för din webbplats och dina program. Dessa URL-anrop är specifika för ditt konto.

| Alternativ | Beskrivning |
| --- | --- |
| **[!UICONTROL Published Server Name]** | Obligatoriskt.<br>Namnet måste använda `https://` i sökvägen.<br>Den här servern är CDN-servern (Content Deliver Network) som används i alla systemgenererade URL-anrop som är specifika för ditt konto. Ändra inte det här servernamnet om du inte har fått instruktioner om att göra det av Adobe tekniska support. |
| **[!UICONTROL Origin Server Name]** | Obligatoriskt.<br>Den här servern används endast för kvalitetstestning. Ändra inte det här servernamnet om du inte har fått instruktioner om att göra det av Adobe tekniska support. |

## Överför till program {#upload-to-application}

* **[!UICONTROL Overwrite Images]**

  Adobe Dynamic Media tillåter inte att två filer har samma namn. Varje objekts Adobe Dynamic Media ID (bildnamnet minus filnamnstillägget) måste vara unikt. På grund av den här regeln har **[!UICONTROL Upload to Application]** en överskrivning. Den exakta effekten av det här alternativet beror på det angivna alternativet Skriv över bilder som du har valt. Dessa alternativ anger hur ersättningsbilder överförs: om de ersätter originalbilderna eller blir dubblettbilder. Duplicerade bilder har bytt namn med ett `-1`. `chair.tif` har till exempel bytt namn till `chair-1.tif`. De här alternativen påverkar bilder som har överförts till en annan mapp än den ursprungliga eller bilder med ett annat filnamnstillägg än den ursprungliga, till exempel JPG, TIF eller PNG.

  >[!NOTE]
  >
  >Om du vill behålla konsekvensen med Experience Manager väljer du alternativet Skriv över bilder **[!UICONTROL Overwrite in current folder, same base name/extension]**.

  | Skriv över bilder, alternativ | Beskrivning |
  | --- | --- |
  | **[!UICONTROL Overwrite in current folder, same base name/extension]** | *Standard* endast för nya Dynamic Media-konton.<br>Det här alternativet är den striktaste regeln för ersättning. Det kräver att du överför ersättningsbilden till samma mapp som originalbilden och att ersättningsbilden har samma filnamnstillägg som originalbilden. Om dessa krav inte uppfylls skapas en dubblett.<br>*Välj det här alternativet* om du vill behålla konsekvensen med Experience Manager. |
  | **[!UICONTROL Overwrite in current folder, same base name regardless of extension]** | Kräver att du överför ersättningsbilden till samma mapp som originalet, men filnamnstillägget kan skilja sig från originalet. Till exempel ersätter stol.tif stol.jpg. |
  | **[!UICONTROL Overwrite in any folder, same base asset name/extension]** | Kräver att ersättningsbilden har samma filnamnstillägg som den ursprungliga bilden (t.ex. måste stol.jpg ersätta stol.jpg, inte stol.tif). Du kan dock överföra ersättningsbilden till en annan mapp än den ursprungliga. Den uppdaterade bilden finns i den nya mappen. Det går inte längre att hitta filen på den ursprungliga platsen. |
  | **[!UICONTROL Overwrite in any folder, same base asset name regardless of extension]** | Det här alternativet är den mest omfattande ersättningsregeln. Du kan överföra en ersättningsbild till en annan mapp än den ursprungliga, överföra en fil med ett annat filnamnstillägg och ersätta den ursprungliga filen. Om originalfilen finns i en annan mapp finns ersättningsbilden i den nya mappen som den överfördes till. |

* **[!UICONTROL Preserve Crop]**

  Kontrollerar bevarande av befintliga manuella beskärningsdefinitioner.

  Se även `preserveCrop` i [UploadPostJob](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-production-api/data-types/r-upload-post-job.html) och [ReprocessAssetsJob](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-production-api/data-types/r-reprocess-assets-job.html), båda i referenshandboken för Dynamic Media Viewer.

## Standardalternativ för överföring {#default-upload-options}

### Fliken Bildredigering {#image-editing-tab}

Med det här filtret kan du finjustera en skärpefiltereffekt på den slutliga nedsamplade bilden. Det hjälper dig att styra intensiteten i effekten, radien för effekten (mätt i pixlar) och ett tröskelvärde för kontrast som ignoreras.

För effekten Oskarp mask används samma alternativ som för filtret Oskarp mask i Photoshop. Till skillnad från vad namnet antyder är Oskarp mask ett skärpefilter.

| Oskarp mask, alternativ | Beskrivning |
| --- | --- |
| **[!UICONTROL Amount]** | Obligatoriskt.<br>Styr mängden kontrast som tillämpas på kantpixlar.<br>Tänk på det som intensiteten i effekten. Den största skillnaden mellan mängden oskarp mask i Adobe Dynamic Media och mängden i Adobe Photoshop är att Photoshop har ett intervall på 1 till 500 %. I Adobe Dynamic Media är värdeintervallet `0.0` till `5.0`. Värdet 5.0 i Adobe Dynamic Media är den ungefärliga motsvarigheten till 500 % i Photoshop, värdet 0,9 motsvarar 90 % och så vidare. |
| **[!UICONTROL Radius]** | Obligatoriskt.<br>Styr radien för effekten.<br>Värdet är `0` till `250`. Effekten körs på alla pixlar i en bild och strålar ut från alla pixlar i alla riktningar. Radien mäts i pixlar. Om du till exempel vill få en liknande skärpeeffekt för en bild på 2 000 x 2 000 pixlar och en bild på 500 x 500 pixlar anger du en radie på två pixlar för bilden på 2 000 x 2 000 pixlar. Ange sedan radien 1 pixel på bilden med 500 x 500 pixlar. Ett större värde används för en bild som har fler pixlar. |
| **[!UICONTROL Threshold]** | Obligatoriskt.<br>Tröskelvärde är ett kontrastintervall som ignoreras när filtret Oskarp mask används. Den här effekten är viktig så att inget &quot;brus&quot; uppstår i en bild när filtret används. Värdeintervallet är `0` - `255`, vilket är antalet intensitetssteg i en gråskalebild. `0`=svart, `128`=50 % grått och `255`=vitt.<br>Ett tröskelvärde på `12` ignorerar små variationer i hudtonens ljusstyrka för att undvika att lägga till brus, men lägger ändå till kantkontrast i kontrastområden som där ögonfransarna möts av hud.<br>Om du har ett foto av någons ansikte påverkar Oskarp mask de kontrastrika delarna av bilden. Till exempel där ögonfransar och hud möts för att skapa ett tydligt kontrastområde och den utjämnade huden. Även den jämnaste huden uppvisar subtila förändringar i intensitetsvärden. Om du inte använder ett tröskelvärde framhäver filtret dessa subtila ändringar i hudpixlar. I sin tur skapas en högljudd och oönskad effekt medan kontrasten på ögonfransarna ökar, vilket ökar skärpan.<br>För att undvika det här problemet introduceras ett tröskelvärde som anger för filtret att ignorera pixlar som inte ändrar kontrast dramatiskt, till exempel mjuk hud.<br>I zippargrafiken som visades tidigare, lägg märke till texturen bredvid zipporna. Bildbrus visas eftersom tröskelvärdena var för låga för att undertrycka bruset. |
| **[!UICONTROL Monochrome]** | Markera för att få bildintensiteten oskarp mask (intensitet).<br>Avmarkera om du vill avmarkera varje färgkomponent för att göra dem skarpa. |

Se även [Öka skärpan i bilder i Adobe Dynamic Media och på Image Server](/help/assets/assets/sharpening_images.pdf).

### PostScript tab {#postscript-tab}

Du kan rastrera Adobe PostScript®-filer, behålla genomskinliga bakgrunder, välja en upplösning och välja en färgrymd.

Du kan använda Adobe PostScript®-filer (EPS) i Adobe Dynamic Media. I Adobe Dynamic Media finns kommandon för att konfigurera dessa filer när du överför dem.

När du överför PostScript-bildfiler (EPS) kan du formatera dem på olika sätt. Du kan rastrera filerna, behålla den genomskinliga bakgrunden, välja en upplösning och välja en färgrymd.

| PostScript | Beskrivning |
| --- | --- |
| **[!UICONTROL Processing]** | Välj Rastrera om du vill konvertera vektorgrafik i filen till bitmappsformat. |
| **[!UICONTROL Maintain transparent background in rendered images]** | Bevarar filens bakgrundsgenomskinlighet. |
| **[!UICONTROL Resolution (pixel/inch)]** | Anger upplösningsinställningen. Den här inställningen avgör hur många pixlar som visas per tum i filen. |
| **[!UICONTROL Color space]** | ・ **[!UICONTROL Detect automatically]** - Behåller filens färgrymd.<br> ・ **[!UICONTROL Force as RGB]** - Konverterar till RGB-färgrymden.<br> ・ **[!UICONTROL Force as CMYK]** - Konverterar till CMYK-färgrymden.<br> ・ **[!UICONTROL Force as Grayscale]** - Konverterar till färgmodellen Gråskala. |

### Photoshop tab {#photoshop-tab}

Du kan skapa mallar från Adobe® Photoshop®-filer, underhålla lager, ange hur lager ska namnges, extrahera text och ange hur bilder ska förankras i mallar.

| Photoshop | Beskrivning |
| --- | --- |
| **[!UICONTROL Maintain layers]** | Rippar lagren i PSD, om det finns några, till enskilda resurser. Resurslagren är fortfarande kopplade till PSD. Du kan visa dem genom att öppna PSD-filen i detaljvyn och välja lagerpanelen. Se Visa och redigera lager i en PSD-fil. |
| **[!UICONTROL Create template]** | Skapar en mall från lagren i PSD-filen. |
| **[!UICONTROL Extract text]** | Extraherar texten så att användare kan söka efter text i ett visningsprogram. |
| **[!UICONTROL Extend layers to background size]** | Utökar storleken på överlappade bildlager till storleken på bakgrundslagret. |
| **[!UICONTROL Layer naming]** | Utökar storleken på överlappade bildlager till storleken på bakgrundslagret.<br> ・ **[!UICONTROL Layer name]** - Namnger bilderna efter deras lagernamn i PSD-filen. Ett lager med namnet Price Tag i den ursprungliga PSD-filen blir till exempel en bild med namnet Price Tag. Om lagernamnen i PSD-filen däremot är Photoshop standardlagernamn (Bakgrund, Lager 1, Lager 2 och så vidare) får bilderna namn efter sina lagernummer i PSD-filen. <br> ・ **[!UICONTROL Photoshop and layer number]** - Namnger bilderna efter deras lagernummer i PSD-filen och ignorerar de ursprungliga lagernamnen. Bilderna namnges med Photoshop-filnamnet och ett nummer på lagret som läggs till. Det andra lagret i en fil med namnet `Spring Ad.psd` får till exempel namnet `Spring Ad_2` även om det har ett icke-standardnamn i Photoshop.<br> ・ **[!UICONTROL Photoshop and layer name]** - Namnger bilderna efter PSD-filen följt av lagernamnet eller lagernumret. Lagernumret används om lagernamnen i PSD-filen är Photoshop standardlagernamn. Ett lager med namnet `Price Tag` i en PSD-fil med namnet `SpringAd` får till exempel namnet `Spring Ad_Price Tag`. Ett lager med standardnamnet Lager2 kallas `Spring Ad_2`. |
| **[!UICONTROL Anchor]** | Ange hur bilder ska förankras i mallar som genereras från lagerkompositionen som skapas från PSD-filen. Som standard är ankarpunkten i mitten. Med en central ankarpunkt kan ersättningsbilder bäst fylla samma område, oavsett ersättningsbildens proportioner. Bilder med en annan aspekt som ersätter den här bilden upptar i själva verket samma utrymme när de refererar till mallen och använder parameterersättning. Ändra till en annan inställning om ditt program kräver att ersättningsbilderna fyller ut det tilldelade utrymmet i mallen. |

### PDF tab {#pdf-tab}

Du kan välja att rastrera filerna, extrahera sökord och länkar, ange upplösning och välja en färgrymd.

| PDF | Beskrivning |
| --- | --- |
| **[!UICONTROL Processing]** | ・ **[!UICONTROL None]** - Ingen bearbetning av PDF utförs.<br> ・ **[!UICONTROL Thumbnail]** - Rippar varje sida i PDF-filen och konverterar den till en miniatyrbild.<br> ・ **[!UICONTROL Rasterize]** - Rippar ned sidorna i PDF-filen och konverterar vektorgrafik till bitmappsbilder. Välj det här alternativet om du vill skapa en e-katalog. |
| **[!UICONTROL Extract]** | ・ **[!UICONTROL None]** - Inga sökord eller länkar extraheras från PDF.<br> ・ **[!UICONTROL Search words]** - Extraherar sökord från PDF-filen så att filen kan genomsökas efter nyckelord i en eCatalog Viewer.<br> ・ **[!UICONTROL Links]** - Extraherar länkar från PDF-filerna och konverterar dem till Image Maps som används i en eCatalog Viewer.<br> ・ **[!UICONTROL Search words and links]** - Extraherar både sökord och länkar för användning i ett eCatalog-visningsprogram. |
| **[!UICONTROL Resolution (pixel/inch)]** | Anger upplösningsinställningen. Den här inställningen avgör hur många pixlar som visas per tum i PDF-filen. Standardvärdet är 150. |
| **[!UICONTROL Color space]** | ・ **[!UICONTROL Detect automatically]** - Behåller färgrymden i PDF-filen.<br> ・ **[!UICONTROL Force as RGB]** - Konverterar till RGB-färgrymden.<br> ・ **[!UICONTROL Force as CMYK]** - Konverterar till CMYK-färgrymden.<br> ・ **[!UICONTROL Force as Grayscale]** - Konverterar till färgmodellen Gråskala. |

### Illustrator tab {#illustrator-tab}

Du kan rastrera Adobe Illustrator®-filer, behålla genomskinliga bakgrunder, välja en upplösning och välja en färgrymd.

Du kan använda Adobe® Illustrator®-filer (AI) i Adobe Dynamic Media. I Adobe Dynamic Media finns kommandon för att konfigurera dessa filer när du överför dem.

När du överför Illustrator-bildfiler (AI) kan du formatera dem på olika sätt. Du kan rastrera filerna, behålla den genomskinliga bakgrunden, välja en upplösning och välja en färgrymd. Formateringsalternativ för PostScript- och Illustrator-filer finns på överföringsskärmen under PostScript-alternativ och Illustrator-alternativ i rutan Alternativ för överföringsjobb.


| Illustrator | Beskrivning |
| --- | --- |
| **[!UICONTROL Processing]** | Välj Rastrera om du vill konvertera vektorgrafik i filen till bitmappsformat. |
| **[!UICONTROL Maintain transparent background in rendered images]** | Bevarar filens bakgrundsgenomskinlighet. |
| **[!UICONTROL Resolution (pixel/inch)]** | Anger upplösningsinställningen. Den här inställningen avgör hur många pixlar som visas per tum i filen. |
| **[!UICONTROL Color space]** | ・ **[!UICONTROL Detect automatically]** - Behåller filens färgrymd.<br> ・ **[!UICONTROL Force as RGB]** - Konverterar till RGB-färgrymden.<br> ・ **[!UICONTROL Force as CMYK]** - Konverterar till CMYK-färgrymden.<br> ・ **[!UICONTROL Force as Grayscale]** - Konverterar till färgmodellen Gråskala. |
