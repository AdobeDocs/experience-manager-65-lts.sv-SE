---
title: Arbeta med 3D-resurser i Dynamic Media
description: Lär dig hur du kan överföra, hantera, visa och leverera 3D-resurser i Dynamic Media som en engagerande upplevelse.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: introduction
content-type: reference
feature: 3D Assets,Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 1%

---

# Arbeta med 3D-resurser i Dynamic Media {#working-with-three-d-assets-dm}

Med Dynamic Media kan du överföra, hantera, visa och leverera 3D-resurser som engagerande upplevelser.

* Publicering med ett klick (med **[!UICONTROL Quick Publish]** i verktygsfältet) av 3D-resurser för att generera en URL.
* Optimerat stöd för 3D-material med den högkvalitativa, interaktiva Dimensional-visningsförinställningen som drivs av Adobe Dimension.
* Med 3D Media WCM-komponenten kan du enkelt lägga till 3D-resurser på dina Adobe Experience Manager Sites-sidor.

Det krävs ingen ytterligare konfiguration för att använda 3D-resurser i Dynamic Media.

![Visa i 3d](/help/assets/assets-dm/3d-dimensional-viewer-quickpublish-url-embed2.png) *Detaljsida för en tredimensionell sko.*

<!-- See also [Dynamic Media 3D Release Notes](/help/release-notes/aem3d-release-notes.md). -->

## 3D-format som stöds i Dynamic Media {#supported-three-d-file-formats-in-dm}

Dynamic Media har stöd för följande 3D-format.

Se även [3D-format som stöds](/help/assets/assets-formats.md).

| 3D-filtillägg | Filformat | MIME-typ | Anteckningar |
|---|---|---|---|
| GLB | Binär GL-överföring | model/gltf-binary | Materialen och texturerna inkluderas som en enda resurs. |
| OBJ | WaveFront 3D-objektfil | application/x-tgif |  |
| STL | Stereolithografi | application/vnd.ms-pki.stl |  |
| USDZ | Zip-arkiv för universell scenbeskrivning | model/vnd.usdz+zip | *Stöd endast för inmatning; ingen visning eller interaktion är tillgänglig.* USDZ är ett tillverkarspecifikt 3D-format som kan visas direkt av Safari- och iOS-enheter. |

>[!NOTE]
>
>3D Media WCM-komponenten och 3D-förhandsvisningen på en tillgångs informationssida är inte kompatibla med den senaste versionen av Chrome (97.x). Om du i stället vill arbeta med 3D-resurser använder du Firefox eller Safari, eller en tidigare version av Chrome (96.x).

## Snabbstart: 3D-resurser i Dynamic Media {#quick-start-three-d}

Följande steg-för-steg-beskrivning av arbetsflödet är utformad för att hjälpa dig att komma igång snabbt med 3D-resurser i läget Dynamic Media - Scene7.

>[!IMPORTANT]
>
>3D-resurser stöds inte i läget Dynamic Media - hybrid.

Innan du arbetar med 3D-resurser i Dynamic Media måste du kontrollera att Experience Manager-administratören redan har aktiverat och konfigurerat Dynamic Media Cloud Services i läget Dynamic Media - Scene7.

Se [Konfigurera E Dynamic Media Cloud-tjänster](/help/assets/config-dms7.md#configuring-dynamic-media-cloud-services) i Konfigurera dynamiska media - Scene7-läge och [Felsök dynamiska media - Scene7-läge](/help/assets/troubleshoot-dms7.md).

1. **Överför 3D-resurser**

   * [Överför dina 3D-resurser för användning i dynamiska media](/help/assets/manage-assets.md#uploading-assets).
   * [3D-filformat som stöds för överföring i dynamiska media](#supported-three-d-file-formats-in-dm).

1. **Hantera 3D-resurser**

   * Ordna och söka i 3D-resurser

      * [Ordna digitala resurser](/help/assets/organize-assets.md#organize-digital-assets).
      * [Sök efter 3D-resurser](/help/assets/search-assets.md).
      * [Använd anpassade predikat för att filtrera sökresultaten](/help/assets/search-assets.md#custompredicates).

   * Visa 3D-resurser

      * [Visa och interagera med 3D-resurser](#viewing-three-d-assets).
      * [Hantera förinställningen för Dimensionellt visningsprogram](/help/assets/managing-viewer-presets.md).

   * Arbeta med metadata för 3D-resurser

      * [Hantera metadata för digitala resurser](/help/assets/metadata.md).
      * [Metadata-scheman](/help/assets/metadata-schemas.md).

1. **Publicera 3D-resurser**

   * [Publicera statiska 3D-resurser för dynamiska media](#publishing-three-d-assets)
   * [Alternativa metoder för publicering av 3D-resurser i Dynamic Media med Dimensional Viewer](#alternate-publish-methods)

## Visa och interagera med 3D-resurser {#viewing-three-d-assets}

I det här avsnittet beskrivs hur du visar och interagerar med 3D-resurser på två olika sätt: från sidan med resursinformation och från komponenten 3D Media i Experience Manager Sites.

Det interaktiva 3D-visningsprogrammet innehåller bland annat en samling interaktiva kamerakontroller där du kan omforma, zooma och panorera 3D-resursen.

Hur lång tid det tar att öppna en 3D-resurs i vyn Resursinformation beror på flera faktorer. Bland dessa faktorer finns följande:

* Bandbredd till servern.
* Latenser till servern
* Bildens komplexitet.

Dessutom är funktioner i klientdatorn, t.ex. en arbetsstation, bärbar dator eller en mobil pekenhet, också viktiga att tänka på när du manipulerar kameran interaktivt. Ett relativt kraftfullt system med bra grafikfunktioner kan göra den interaktiva 3D-visningen smidigare och mer gynnsam.

>[!TIP]
>
>Du kan öppna Dimensional Viewer-förinställningen i Viewer Preset Editor för att öva på att navigera i en 3D-resurs utan att först behöva överföra några 3D-filer. Förinställningen för Dimensional Viewer har en inbyggd 3D-resurs som du kan interagera med.
>
>Se [Hantera visningsförinställningar](/help/assets/managing-viewer-presets.md).

## Visa och interagera med en 3D-resurs från sidan med resursinformation {#viewing-three-d-assets-from-asset-details-page}

Se även [Förhandsgranska resurser med programgränssnittet](/help/assets/previewing-assets.md).

**Så här visar och interagerar du med en 3D-resurs från sidan med resursinformation:**

1. Se till att du har överfört 3D-resurser till Experience Manager.

   Se [Överför dina 3D-resurser för användning i dynamiska media](/help/assets/manage-assets.md#uploading-assets).

1. Gå till **[!UICONTROL Assets]** > **[!UICONTROL Files]** från Experience Manager på sidan **[!UICONTROL Navigation]**.
1. I den nedrullningsbara listan **[!UICONTROL View]** i det övre högra hörnet på sidan väljer du **[!UICONTROL Card View]**.
1. Navigera till en 3D-resurs som du vill visa.
1. Välj kortet för 3D-resursen.
1. Gör något av följande på informationssidan för 3D-resursen:

   | Visa | Beskrivning | Musåtgärd | Åtgärd på pekskärmen |
   | --- | --- | --- | --- |
   | **Vrid kameran** | Ordna vyn runt 3D-scenen och objekt. | Vänsterklicka och dra. | Tryck med ett finger och dra. |
   | **Panorera kameran** | Panorera vyn åt vänster, åt höger, uppåt eller nedåt. | Högerklicka och dra. | Tryck med två fingrar och dra. |
   | **Zooma kameran** | Flytta in och ut från områden i 3D-scenen. | Rullningshjul. | Nyp med två fingrar. |
   | **Ange kameran igen** | Centrera kameran igen till en punkt på ett objekt i 3D-scenen. | Dubbelklicka. | Dubbelmarkera. |
   | **Återställ** | I närheten av det nedre högra hörnet av sidan väljer du ikonen Återställ om du vill återställa målpunkten till mitten av 3D-resursen. Återställ flyttar också kameran närmare eller längre bort för att visa resursen i dess helhet och med en rimlig visningsstorlek. |   |   |
   | **Helskärmsläge** | Om du vill aktivera helskärmsläget väljer du Helskärmsikonen längst ned till höger på sidan. |   |   |

1. I det övre högra hörnet av sidan väljer du **[!UICONTROL Close]** för att gå tillbaka till Assets-sidan.

## Visa och interagera med en 3D-resurs inuti en 3D-mediekomponent {#interacting-with-asset-inside-three-d-media-component}

När en webbsida är i läget **[!UICONTROL Edit]** går det inte att interagera med en 3D-resurs. Om du vill göra resursen interaktiv kan du använda funktionen **[!UICONTROL Preview]** för att visa webbsidan i sidredigeraren med fullständig åtkomst till funktionerna i 3D Media-komponenten.

>[!IMPORTANT]
>
>Du kan bara utföra den här åtgärden när du har lagt till en 3D-mediekomponent på en webbsida och tilldelat en 3D-resurs till komponenten. Se [Lägga till 3D-mediekomponenten på en webbsida](#adding-the-three-d-media-component-to-a-web-page) och [Tilldela en 3D-resurs till en 3D-mediekomponent](#assigning-a-three-d-asset-to-the-component).

Se även [Förhandsgranska resurser med programgränssnittet](/help/assets/previewing-assets.md).

**Så här visar och interagerar du med en 3D-resurs i en 3D-mediekomponent:**

1. Gör något av följande när en webbsida är i läget **[!UICONTROL Edit]**:

   * I närheten av sidans övre högra hörn väljer du **[!UICONTROL Preview]** för att gå in i **[!UICONTROL Preview]**-läget.
   * Ta bort `/editor.html` från sidans URL i webbläsaren.

   ![3D-resurs som visas inuti 3D-mediekomponenten](/help/assets/assets-dm/3d-asset-in-3d-media.png)
En helt interaktiv 3D-resurs som visas i **[!UICONTROL Preview]** -läge.

1. Gör något av följande i **[!UICONTROL Preview]**-läget:

   | Visa | Beskrivning | Musåtgärd | Åtgärd på pekskärmen |
   | --- | --- | --- | --- |
   | **Vrid kameran** | Ordna vyn runt 3D-scenen och objekt. | Vänsterklicka och dra. | Tryck med ett finger och dra. |
   | **Panorera kameran** | Panorera vyn åt vänster, åt höger, uppåt eller nedåt. | Högerklicka och dra. | Tryck med två fingrar och dra. |
   | **Zooma kameran** | Flytta in och ut från områden i 3D-scenen. | Rullningshjul. | Nyp med två fingrar. |
   | **Ange kameran igen** | Centrera kameran igen till en punkt på ett objekt i 3D-scenen. | Dubbelklicka. | Dubbelmarkera. |
   | **Återställ** | I närheten av det nedre högra hörnet av sidan väljer du ikonen Återställ om du vill återställa målpunkten till mitten av 3D-resursen. Återställ flyttar också kameran närmare eller längre bort för att visa resursen i dess helhet och med en rimlig visningsstorlek. |   |   |
   | **Helskärmsläge** | Om du vill aktivera helskärmsläget väljer du Helskärmsikonen längst ned till höger på sidan. |   |   |

## Arbeta med 3D Media-komponenten {#working-with-three-d-media-component}

Dynamic Media innehåller en 3D Media-komponent för Dynamic Media som du kan använda i Adobe Experience Manager Sites för att aktivera interaktiv visning av 3D-modeller på dina webbsidor.

* [Lägga till komponenten 3D Media i sidmallen](#adding-three-d-media-component-to-page-template)
* [Lägga till komponenten 3D Media på en webbsida](#adding-the-three-d-media-component-to-a-web-page)
   * [Valfritt - Konfigurera komponenten 3D Media](#configuring-the-three-d-component)
* [Tilldela en 3D-resurs till 3D-mediekomponenten](#assigning-a-three-d-asset-to-the-component)

## Lägga till komponenten 3D Media i sidmallen {#adding-three-d-media-component-to-page-template}

1. Navigera till **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL Templates]**.
1. Navigera till den sidmall som du vill aktivera 3D-komponenten i och markera mallen.
1. Välj **[!UICONTROL Edit]** så att du kan öppna mallen.
1. I den nedrullningsbara menyn uppe till höger på sidan väljer du läget **[!UICONTROL Structure]**, om det inte redan är aktivt.

   ![3d-media-component-structure](/help/assets/assets-dm/3d-media-component-structure.png)

1. Markera ett tomt område i **[!UICONTROL Layout Container]**-regionen så att du markerar det och öppnar det tillhörande verktygsfältet.
1. I verktygsfältet väljer du ikonen **[!UICONTROL Policy]** för att öppna **[!UICONTROL Policy Editor]**.
1. I avsnittet **[!UICONTROL Properties]**, under fliken **[!UICONTROL Allowed Components]**, bläddrar du till **[!UICONTROL Dynamic Media]**, expanderar sedan listan och kontrollerar **[!UICONTROL 3D Media]**.
1. Välj **[!UICONTROL Done]** om du vill spara ändringarna och stänga **[!UICONTROL Policy Editor]**.

   Nu kan du placera komponenten Dynamic Media 3D Media på alla sidor som använder den här mallen.

## Lägga till komponenten 3D Media på en webbsida {#adding-the-three-d-media-component-to-a-web-page}

Om du använder Experience Manager som webbinnehållshanteringssystem kan du lägga till 3D-resurser på dina webbsidor med 3D Media-komponenten.

Se även [Lägg till dynamiska medieresurser på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md).

**Så här lägger du till komponenten 3D Media på en webbsida:**

1. Öppna Experience Manager Sites och markera den webbsida där du vill lägga till komponenten Dynamic Media 3D Media.
1. Välj ikonen **[!UICONTROL Edit]** (penna) så att du kan öppna sidan i sidredigeraren. Kontrollera att läget **[!UICONTROL Edit]** är markerat i sidans övre högra hörn.

   ![3d-media-component-add](/help/assets/assets-dm/3d-media-component-edit.png)

1. I verktygsfältet väljer du ikonen för panelen Sida för att växla eller aktivera visningen av panelen.

1. I sidopanelen väljer du plustecknet för att öppna listan **[!UICONTROL Components]**.

   ![3d-media-component-drag-drop](/help/assets/assets-dm/3d-assets-filter.png)

1. Dra **[!UICONTROL 3D Media]**-komponenten från listan **[!UICONTROL Components]** till den plats på sidan där du vill att 3D-visningsprogrammet ska visas.

Nu kan du tilldela en 3D-resurs till komponenten.

Se [Tilldela en 3D-resurs till 3D-mediekomponenten](#assigning-a-three-d-asset-to-the-component).

### Valfritt - Konfigurera komponenten 3D Media {#configuring-the-three-d-component}

1. I sidredigeraren i Experience Manager Sites väljer du den **[!UICONTROL 3D Media Viewer]**-komponent som du tidigare har lagt till på sidan.
1. Välj ikonen **[!UICONTROL Configuration]** (skiftnyckel) så att du kan öppna dialogrutan för komponentkonfiguration.

   ![3d-media-component-config](/help/assets/assets-dm/3d-media-component-config.png)

1. I dialogrutan 3D-media väljer du **[!UICONTROL Dimensional]** i listrutan Visningsförinställning för att tilldela komponenten förinställningen för Dimensional Viewer.

   ![3d-media-component-edit-config](/help/assets/assets-dm/3d-media-component-edit-config.png)

1. Markera bockmarkeringen i det övre högra hörnet för att spara ändringarna.

## Tilldela en 3D-resurs till 3D-mediekomponenten {#assigning-a-three-d-asset-to-the-component}

När du har lagt till en 3D-mediekomponent på en webbsida kan du tilldela den en 3D-resurs.

Se [Lägga till komponenten 3D Media på en webbsida](#adding-the-three-d-media-component-to-a-web-page).

**Så här tilldelar du en 3D-resurs till 3D-mediekomponenten:**

1. I sidredigeraren i Experience Manager Sites väljer du ikonen **[!UICONTROL Assets]** för att öppna **[!UICONTROL Assets]** på sidpanelen.
1. I listrutan väljer du **[!UICONTROL 3D]** om du bara vill visa filtyper för 3D-resurser.
1. På sidopanelen söker du efter eller bläddrar till den 3D-resurs som du vill visa på sidan som redigeras.
1. Dra 3D-resursen från sidopanelen i Assets och släpp den på komponenten **[!UICONTROL 3D Media]** som du tidigare lade till på sidan.

   ![Tilldela 3D-resurs till 3D-mediekomponenten](/help/assets/assets-dm/3d-asset-add.png)

>[!NOTE]
>
>När en webbsida är i Experience Manager Sites **[!UICONTROL Edit]**-läge visar 3D-mediekomponenten 3D-resursen, men ingen interaktion med resursen är möjlig. Om du vill göra resursen interaktiv kan du använda funktionen **[!UICONTROL Preview]** för att visa webbsidan i sidredigeraren med fullständig åtkomst till funktionerna i 3D Media-komponenten.

## Publicera statiska 3D-resurser för dynamiska media {#publishing-three-d-assets}

Dynamic Media accepterar olika 3D-filformat som stöds som *statiskt innehåll* i Dynamic Media. Statiskt innehåll innebär att du kan överföra och publicera 3D-resurser, men det finns inget stöd för *variabel bildåtergivning* eller bildåtergivning som är associerat med 3D-resursen. Orsaken är att Dynamic Media Imaging Server inte känner igen 3D-format. När du har publicerat en 3D-resurs i Dynamic Media får du en direkt URL som du kan kopiera. URL:en för 3D-resursen följer den vanliga URL-strukturen för dynamiska media. Du kan dock inte redigera några parametrar i resursens URL, till skillnad från traditionella bildresurser i Dynamic Media.

Se även [Hämta en URL för en statisk resurs](/help/assets/linking-urls-to-yourwebapplication.md#obtaining-a-url-for-a-static-asset).

I **[!UICONTROL Card View]** visas en liten global ikon direkt under namnet på en resurs och till vänster om dess datum och tid för att ange att den är publicerad. I **[!UICONTROL List View]** anger kolumnen **[!UICONTROL Published]** vilka resurser som har publicerats och inte.

Om du använder Experience Manager som WCM-fil kan du använda den här publiceringsmetoden för att lägga till 3D-resurser för dynamiska media direkt på webbsidan.

Se även [Publicera dynamiska medieresurser](publishing-dynamicmedia-assets.md).

Se även [Publicera sidor](/help/sites-authoring/publishing-pages.md).

**Så här publicerar du statiska 3D-resurser för dynamiska media:**

1. Öppna en 3D-resurs (GLB-, OBJ- eller STL-filformat) så att du kan visa den på sidan med tillgångsinformation.
1. Välj **[!UICONTROL Quick Publish]** i verktygsfältet.

   ![3d-asset-quick-publish](/help/assets/assets-dm/3d-asset-quick-publish.png)

1. Välj **[!UICONTROL Close]** om du vill stänga dialogrutan och gå tillbaka till sidan med resursinformation.
1. Välj **[!UICONTROL Renditions]** i listrutan till vänster om 3D-resursens filnamn.

   ![3d-asset-renditions](/help/assets/assets-dm/3d-asset-renditions.png)

1. Välj **[!UICONTROL original]**. När en 3D-resurs publiceras (eller&quot;aktiveras&quot;) visas knappen **[!UICONTROL URL]** i det nedre vänstra hörnet på sidan om alla följande 3D-resursvillkor uppfylls:
   * 3D-resursen har ett format som stöds (GLB, OBJ, STL och USDZ).
   * 3D-resursen har importerats till Dynamic Media Image Production System (IPS).
   * 3D-resursen publiceras.

   ![3d-asset-url](/help/assets/assets-dm/3d-asset-url.png)

1. Välj **[!UICONTROL URL]** så att du kan visa 3D-resursens URL för direktproduktion som du kan kopiera och använda på webbsidor.

### Alternativa metoder för publicering av 3D-resurser i Dynamic Media med Dimensional Viewer {#alternate-publish-methods}

Använd följande två metoder för att publicera 3D-resurser i Dynamic Media om du *inte* använder Experience Manager som WCM-fil.

* **[!UICONTROL URL]** - Använd **[!UICONTROL URL]** om du använder ett tredjepartssystem för hantering av webbinnehåll och vill länka 3D-resurser för dynamiska media till dina webbsidor med Dimensional Viewer.

  Se [Länka URL:er till ditt webbprogram](/help/assets/linking-urls-to-yourwebapplication.md#obtaining-a-url-for-an-asset).

* **[!UICONTROL Embed]** - Använd **[!UICONTROL Embed]** när du vill visa en 3D-resurs för dynamiska media som är inbäddad på en webbsida med Dimensional Viewer. Du kopierar inbäddningskoden till Urklipp så att du kan klistra in den på webbsidorna. Det är inte tillåtet att redigera koden i dialogrutan **[!UICONTROL Embed]**.

  Se [Bädda in Dynamic Media Video, Image Viewer eller Dimensional Viewer på en webbsida](/help/assets/embed-code.md#embedding-the-video-or-image-viewer-on-a-web-page).
