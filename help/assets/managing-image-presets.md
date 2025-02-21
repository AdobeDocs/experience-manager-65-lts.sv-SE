---
title: Hantera förinställningar för dynamiska media-bilder
description: Förstå förinställningar för Dynamic Media-bilder och lär dig hur du skapar, ändrar och hanterar bildförinställningar.
mini-toc-levels: 3
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
legacypath: /content/docs/en/aem/6-0/administer/integration/dynamic-media/image-presets
feature: Image Presets
role: User, Admin
solution: Experience Manager, Experience Manager Assets
source-git-commit: 887dc1d6d7e11672b62ef5ca5463ea6181ff0320
workflow-type: tm+mt
source-wordcount: '3650'
ht-degree: 6%

---

# Hantera förinställningar för dynamiska media-bilder{#managing-image-presets}

Med bildförinställningar kan Adobe Experience Manager Assets dynamiskt leverera bilder i olika storlekar, i olika format eller med andra bildegenskaper som genereras dynamiskt. Varje bildförinställning representerar en fördefinierad samling kommandon för storleksändring och formatering för visning av bilder. När du skapar en bildförinställning väljer du en storlek för bildleverans. Du kan också välja formateringskommandon så att bildens utseende optimeras när bilden levereras för visning.

Administratörer kan skapa förinställningar för att exportera resurser. Användarna kan välja en förinställning när de exporterar bilder, vilket även innebär att bilderna formateras om enligt de specifikationer som administratören anger.

Du kan också skapa bildförinställningar som är responsiva. Om du använder en responsiv bildförinställning på dina resurser ändras de beroende på vilken enhet eller skärmstorlek de visas på. Du kan konfigurera bildförinställningar så att de använder CMYK i färgrymden förutom RGB och Grå.

I det här avsnittet beskrivs hur du skapar, ändrar och i allmänhet hanterar bildförinställningar. Du kan använda en bildförinställning på en bild när du vill förhandsvisa den. Se [Använda bildförinställningar](/help/assets/image-presets.md).

>[!NOTE]
>
>Smart bildbehandling fungerar med befintliga bildförinställningar och använder intelligens under de sista millisekunderna i leveransen för att ytterligare minska bildfilens storlek baserat på webbläsarens eller nätverkets anslutningshastighet. Mer information finns i [Smart bildbehandling](/help/assets/imaging-faq.md).

## Förinställningar för Dynamic Media-bilder {#understanding-image-presets}

Precis som ett makro är en bildförinställning en fördefinierad samling kommandon för storleksändring och formatering som sparats under ett namn. Om du vill förstå hur bildförinställningar fungerar kan du anta att webbplatsen kräver att varje produktbild visas i olika storlekar, olika format och komprimeringsgrader för datorer och mobila enheter.

>[!NOTE]
>
>I Dynamic Media - Scene7-läget stöds endast bildförinställningar för bildresurser.

Du kan skapa två bildförinställningar: en med 500 x 500 pixlar för skrivbordsversionen och 150 x 150 pixlar för den mobila versionen. Du skapar två bildförinställningar, en med namnet `Enlarge` om du vill visa bilder med 500 x 500 pixlar och en med namnet `Thumbnail` om du vill visa bilder med 150 x 150 pixlar. För att leverera bilder i storleken `Enlarge` och `Thumbnail` letar Experience Manager upp definitionen av förinställningen Förstora bild och miniatyrbildsförinställningen. Sedan genererar Experience Manager dynamiskt en bild med samma storlek och formateringsspecifikationer som varje bildförinställning.

Bilder som minskar i storlek när de levereras dynamiskt kan förlora i skärpa och detaljer. Därför innehåller varje bildförinställning formateringskontroller för optimering av en bild när den levereras i en viss storlek. Med dessa kontroller kan du vara säker på att dina bilder är skarpa och tydliga när de levereras till din webbplats eller ditt program.

Administratörer kan skapa bildförinställningar. Om du vill skapa en bildförinställning kan du börja från början eller så kan du börja från en befintlig och spara den under ett nytt namn.

## Hantera förinställningar för dynamiska media-bilder {#managing-image-presets-1}

Du hanterar dina bildförinställningar i Experience Manager genom att trycka på eller klicka på Experience Manager logotyp för att komma åt den globala navigeringskonsolen och sedan trycka eller klicka på verktygsikonen och navigera till **[!UICONTROL Assets > Image Presets]**.

![6_5_tools-assets-imagepresets](assets/6_5_tools-assets-imagepresets.png)

>[!NOTE]
>
>Alla bildförinställningar som du skapar är också tillgängliga som dynamiska återgivningar när du förhandsvisar eller levererar resurser.
>
>I *Dynamiska medier - Scen7-läge* behöver du *inte* publicera bildförinställningar eftersom bildförinställningar publiceras automatiskt.
>
>I *Dynamiska media - hybridläge* måste du publicera bildförinställningar manuellt.
>
>Se [Publicera bildförinställningar](#publishing-image-presets).

>[!NOTE]
>
>I systemet visas olika återgivningar när du väljer **[!UICONTROL Renditions]** i en tillgångs detaljvy. Du kan öka eller minska antalet bildförinställningar som visas. Se [Öka antalet bildförinställningar som visas](#increasing-or-decreasing-the-number-of-image-presets-that-display).

### Smarta beskärningar, filformaten Adobe Illustrator (AI), Postscript (EPS) och PDF {#adobe-illustrator-ai-postscript-eps-and-pdf-file-formats}

>[!NOTE]
>
>Det här avsnittet gäller endast för Dynamic Media - hybridläge.

Om du tänker ge stöd för att lägga in AI-, EPS- och PDF-filer så att du kan generera dynamiska återgivningar av dessa filformat bör du granska följande information innan du skapar bildförinställningar.

Adobe Illustrator filformat är en sorts PDF. De största skillnaderna i Experience Manager Assets är följande:

* Adobe Illustrator-dokument består av en sida med flera lager. Varje lager extraheras som en PNG-delresurs under Illustrator huvudresurs.
* PDF-dokument består av en eller flera sidor. Varje sida extraheras som en PDF-delresurs på en sida under PDF-huvuddokumentet med flera sidor.

Delresurserna skapas av komponenten `Create Sub Asset process` i det övergripande `DAM Update Asset`-arbetsflödet. Om du vill visa den här processkomponenten i arbetsflödet väljer du **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]** > **[!UICONTROL DAM Update Asset]** > **[!UICONTROL Edit]**.

Se även [Visa sidor i en flersidig fil](/help/assets/managing-linked-subassets.md#view-pages-of-a-multi-page-file).

Du kan visa delresurserna eller sidorna när du öppnar resursen, välja Innehåll-menyn och välja **[!UICONTROL Subassets]** eller **[!UICONTROL Pages]**. Deltillgångarna är verkliga tillgångar. Det innebär att PDF-sidor extraheras av arbetsflödeskomponenten `Create Sub Asset`. De lagras sedan som `page1.pdf`, `page2.pdf` och så vidare, under huvudresursen. När de har lagrats bearbetar arbetsflödet `DAM Update Asset` dem.

Om du vill använda Dynamic Media för att förhandsgranska och generera dynamiska renderingar för AI-, EPS- eller PDF-filer måste du utföra följande steg:

1. I arbetsflödet `DAM Update Asset` rastrerar processkomponenten `Rasterize PDF/AI Image Preview Rendition` den första sidan i den ursprungliga resursen - med den konfigurerade upplösningen - till en `cqdam.preview.png`-rendering.

1. Återgivningen av `cqdam.preview.png` optimeras sedan till en PTIFF-fil av processkomponenten `Dynamic Media Process Image Assets` i arbetsflödet.

>[!NOTE]
>
>I arbetsflödet [!UICONTROL DAM Update Asset] genererar steget **[!UICONTROL EPS thumbnails]** miniatyrer för EPS-filer.

#### PDF/AI/EPS-metadataegenskaper {#pdf-ai-eps-asset-metadata-properties}

| **Metadataegenskap** | **Beskrivning** |
|---|---|
| `dam:Physicalwidthininches` | Dokumentets bredd i tum. |
| `dam:Physicalheightininches` | Dokumenthöjd i tum. |

Du kommer åt alternativen för processkomponenten `Rasterize PDF/AI Image Preview Rendition` via arbetsflödet `DAM Update Asset`.

I det övre vänstra hörnet väljer du Adobe Experience Manager och går till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**. På sidan Arbetsflödesmodeller väljer du **[!UICONTROL DAM Update Asset]** och sedan **[!UICONTROL Edit]** i verktygsfältet. Dubbelmarkera `Rasterize PDF/AI Image Preview Rendition` -processkomponenten på arbetsflödessidan [!UICONTROL DAM Update Asset] för att öppna dialogrutan Stegegenskaper.

#### Rastrera återgivningsalternativen PDF/AI Image Preview {#rasterize-pdf-ai-image-preview-rendition-options}

![Argument för rastrering av PDF- eller AI-arbetsflöde](assets/rasterize_pdf_ai_image_preview.png)

Argument för att rastrera arbetsflödet i PDF eller AI

<table>
 <tbody>
  <tr>
   <td><strong>Processargument</strong></td>
   <td><strong>Standardinställning</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>Mime-typer</td>
   <td><p>application/pdf</p> <p>application/postscript</p> <p>program/illustrator<br /> </p> </td>
   <td>Lista över dokumentMIME-typer som anses vara PDF- eller Illustrator-dokument.<br /> </td>
  </tr>
  <tr>
   <td>Maximal bredd</td>
   <td>2048</td>
   <td>Maximal bredd för den genererade förhandsvisningsåtergivningen, i pixlar.<br /> </td>
  </tr>
  <tr>
   <td>Maximal höjd</td>
   <td>2048</td>
   <td>Maximal höjd för den genererade förhandsvisningsåtergivningen, i pixlar.<br /> </td>
  </tr>
  <tr>
   <td>Upplösning</td>
   <td>72</td>
   <td>Upplösning för att rastrera den första sidan, i ppi (pixlar per tum).</td>
  </tr>
 </tbody>
</table>

Med standardprocessargumenten rastreras den första sidan i ett PDF/AI-dokument med 72 ppi och den genererade förhandsvisningsbilden med storleken 2 048 x 2 048 pixlar. För en typisk driftsättning kanske du vill öka upplösningen till minst 150 ppi. Ett dokument med US-teckenstorlek på 300 ppi kräver t.ex. en maximal bredd och höjd på 2 550 x 3 300 pixlar.

Maximal bredd och Maximal höjd begränsar upplösningen som rastreras. Om maxvärdena t.ex. är oförändrade och upplösningen är 300 ppi rastreras ett US Letter-dokument med 186 ppi. Dokumentet är alltså 1 581 x 2 046 pixlar.

Processkomponenten `Rasterize PDF/AI Image Preview Rendition` har en definierad maxgräns för att säkerställa att den inte skapar för stora bilder i minnet. Sådana stora bilder kan flöda över minnet som Java™ Virtual Machine (Java™ Virtual Machine) har fått. Man måste se till att JVM får tillräckligt med minne för att hantera det konfigurerade antalet parallella arbetsflöden, där var och en har möjlighet att skapa en bild med den högsta konfigurerade storleken.

### InDesign (INDD), filformat {#indesign-indd-file-format}

Om du tänker ge stöd för inmatning av INDD-filer så att du kan generera en dynamisk återgivning av det här filformatet, kanske du vill granska följande information innan du skapar bildförinställningar.

För InDesign-filer extraheras underresurser endast om Adobe InDesign Server är integrerat med Experience Manager. Refererade resurser länkas baserat på deras metadata. InDesign Server krävs inte för länkning. De refererade resurserna måste dock finnas i Experience Manager innan InDesign-filerna bearbetas för att länkarna ska skapas mellan InDesign-filerna och de refererade resurserna.

Se [Integrera Experience Manager Assets med InDesign Server](/help/assets/indesign.md).

Processkomponenten för medieextrahering i arbetsflödet `DAM Update Asset` kör flera förkonfigurerade Extend Scripts för att bearbeta InDesign-filer.

![ExtendScript-sökvägarna i argumenten i medieextraheringsprocessen](assets/6_5_mediaextractionprocess.png)

ExtendScript-sökvägarna i argumenten för processkomponenten för medieextrahering i arbetsflödet [!UICONTROL DAM Update Asset].

Följande skript används av integreringen med Dynamic Media:

<table>
 <tbody>
  <tr>
   <td><strong>ExtendScript name</strong></td>
   <td><strong>Standard</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>ThumbnailExport.jsx</td>
   <td>Ja</td>
   <td>Skapar en 300 ppi <code>thumbnail.jpg</code>-rendering som är optimerad och omvandlad till en PTIFF-rendering av <code>Dynamic Media Process Image Assets</code>-processkomponenten.<br /> </td>
  </tr>
  <tr>
   <td>JPEGPagesExport.jsx</td>
   <td>Ja</td>
   <td>Skapar en 300 PPI JPEG-underresurs för varje sida. Underresursen JPEG är en verklig tillgång som lagras under InDesign-resursen. Den är också optimerad och omvandlad till en PTIFF av arbetsflödet <code>DAM Update Asset</code>.<br /> </td>
  </tr>
  <tr>
   <td>PDFPagesExport.jsx</td>
   <td>Nej</td>
   <td>Skapar en PDF-underresurs för varje sida. PDF underresurs bearbetas enligt beskrivningen ovan. Eftersom PDF endast innehåller en sida genereras inga underresurser.<br /> </td>
  </tr>
 </tbody>
</table>

## Konfigurera miniatyrstorlek för bild {#configuring-image-thumbnail-size}

Du kan konfigurera storleken på miniatyrbilder genom att konfigurera inställningarna i arbetsflödet för **[!UICONTROL DAM Update Asset]**. I arbetsflödet finns två steg där du kan konfigurera miniatyrstorlek för bildresurser. Även om (**[!UICONTROL Dynamic Media Process Image Assets]**) används för dynamiska bildobjekt och (**[!UICONTROL Process Thumbnails]**) används för att generera statiska miniatyrbilder, eller när alla andra processer inte kan generera miniatyrbilder, måste *båda* ha samma inställningar.

I steget **[!UICONTROL Dynamic Media Process Image Assets]** genereras miniatyrbilder av bildservern och den här konfigurationen är oberoende av den konfiguration som används i steget **[!UICONTROL Process Thumbnails]**. Att skapa miniatyrbilder med steget **[!UICONTROL Process Thumbnails]** är det långsammaste och mest minneskrävande sättet att skapa miniatyrbilder.

Storleksändring för miniatyrbilder har definierats i följande format: **`width:height:center`**, till exempel `80:80:false`. Bredden och höjden bestämmer storleken i pixlar på miniatyrbilden. Mittvärdet är antingen false eller true, och om värdet är true betyder det att miniatyrbilden har exakt den storlek som anges i konfigurationen. Om den storleksändrade bilden är mindre centreras den i miniatyrbilden.

>[!NOTE]
>
>* Miniatyrbildsstorlekar för EPS-filer konfigureras i steget **[!UICONTROL EPS thumbnails]** på fliken **[!UICONTROL Arguments]** under Miniatyrbilder.
>
>* Miniatyrstorlekar för videoklipp konfigureras i steget **[!UICONTROL FFmpeg thumbnails]** på fliken **[!UICONTROL Process]** under **[!UICONTROL Arguments]**.
>

**Så här konfigurerar du miniatyrbildens storlek:**

1. Välj **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]** > **[!UICONTROL DAM Update Asset]** > **[!UICONTROL Edit]**.
1. Välj steget **[!UICONTROL Dynamic Media Process Image Assets]** och klicka på fliken **[!UICONTROL Thumbnails]**. Ändra miniatyrstorleken efter behov och välj sedan **[!UICONTROL OK]**.

   ![6_5_dynamicmediaprocessimageassets-thumbnailstab](assets/6_5_dynamicmediaprocessimageassets-thumbnailstab.png)

1. Markera steget **[!UICONTROL Process Thumbnails]** och välj sedan fliken **[!UICONTROL Thumbnails]**. Ändra miniatyrstorleken efter behov och välj sedan **[!UICONTROL OK]**.

   >[!NOTE]
   >
   >Värdena i thumbnails-argumentet i steget **[!UICONTROL Process Thumbnails]** måste matcha thumbnails-argumentet i steget **[!UICONTROL Dynamic Media Process Image Assets]**.

1. Välj **[!UICONTROL Save]** om du vill spara ändringarna i arbetsflödet.

### Öka eller minska antalet dynamiska mediebildförinställningar som visas {#increasing-or-decreasing-the-number-of-image-presets-that-display}

De bildförinställningar du skapar är tillgängliga som dynamiska återgivningar när du förhandsgranskar resurser. Experience Manager visar olika dynamiska återgivningar när en resurs från **[!UICONTROL Detail View > Renditions]** visas. Du kan öka eller minska gränsen för de återgivningar som visas.

**Öka eller minska antalet förinställningar för dynamiska media som visas:**

1. Gå till CRXDE Lite ([https://localhost:4502/crx/de](https://localhost:4502/crx/de)).
1. Navigera till noden med bildförinställningar på `/libs/dam/gui/coral/content/commons/sidepanels/imagepresetsdetail/imgagepresetslist`

   ![increase_decasethenumberofimagepresetsthatdisplay](assets/increase_decreasethenumberofimagepresetsthatdisplay.png)

1. I egenskapen **[!UICONTROL limit]** ändrar du **[!UICONTROL Value]**, som är inställt på 15 som standard, till önskat tal.
1. Navigera till bildförinställningens datakälla på `/libs/dam/gui/coral/content/commons/sidepanels/imagepresetsdetail/imgagepresetslist/datasource`

   ![chlimage_1-495](assets/chlimage_1-495.png)

1. I egenskapen limit ändrar du talet till önskat tal, till exempel `{empty requestPathInfo.selectors[1] ? "20" : requestPathInfo.selectors[1]}`
1. Välj **[!UICONTROL Save All]**.

## Skapa en förinställning för Dynamic Media-bilder {#creating-image-presets}

När du skapar en förinställning för en bild i Dynamic Media kan du använda dessa inställningar på alla bilder när du förhandsgranskar eller publicerar.

>[!NOTE]
>
>Om du använder Internet Explorer 9 visas inte den förinställning du har skapat i förinställningslistan direkt när du har sparat. Du kan lösa problemet genom att inaktivera cacheminnet för IE9.

Om du tänker ge stöd för att lägga in AI-, PDF- och EPS-filer så att du kan generera dynamisk återgivning av dessa filformat bör du granska följande information innan du skapar bildförinställningar.
Se [Adobe Illustrator (AI), PostScript (EPS) och PDF ](#adobe-illustrator-ai-postscript-eps-and-pdf-file-formats).

Om du tänker ge stöd för inmatning av INDD-filer så att du kan generera en dynamisk återgivning av det här filformatet, kanske du vill granska följande information innan du skapar bildförinställningar.
Se [InDesign-filformat (INDD)](#indesign-indd-file-format).

>[!NOTE]
>
>Om du vill skapa förinställningar för Dynamic Media-bilder måste du ha administratörsbehörighet som Experience Manager-administratör eller Admin Console-administratör.

**Så här skapar du en förinställning för en dynamisk mediabild:**

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och väljer sedan **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Image Presets]**.
1. Klicka på **[!UICONTROL Create]**. Fönstret **[!UICONTROL Edit Image Preset]** öppnas.

   ![chlimage_1-496](assets/chlimage_1-496.png)

   >[!NOTE]
   >
   >Om du vill att den här bildförinställningen ska vara responsiv raderar du värdena i fälten **[!UICONTROL width]** och **[!UICONTROL height]** och lämnar dem tomma.

1. Ange önskade värden på flikarna **[!UICONTROL Basic]** och **[!UICONTROL Advanced]**, inklusive ett namn. Alternativen beskrivs i [Alternativ för bildförinställningar](#image-preset-options). Förinställningarna visas i den vänstra rutan och kan användas direkt tillsammans med andra resurser.

   ![6_5_imagepreset-edit](assets/6_5_imagepreset-edit.png)

1. Klicka på **[!UICONTROL Save]**.

## Skapa en förinställning för responsiv bild {#creating-a-responsive-image-preset}

Om du vill skapa en responsiv bildförinställning utför du stegen i [Skapa bildförinställningar](#creating-image-presets). När du anger höjd och bredd i fönstret **[!UICONTROL Edit Image Preset]** raderar du värdena och låter dem vara tomma.

Om du lämnar dem tomma visas information om att den här bildförinställningen är responsiv. Du kan justera de andra värdena efter behov.



>[!NOTE]
>
>Om du vill visa knapparna **[!UICONTROL URL]** och **[!UICONTROL RESS]** när du använder en bildförinställning på en resurs måste resursen publiceras.
>
>![chlimage_1-79](assets/chlimage_1-498.png)
>
>I Dynamic Media - Scene7-läge publiceras bildförinställningar och bildresurser automatiskt.
>
>I Dynamic Media - hybrid-läge måste du manuellt publicera bildförinställningar och bildresurser.

### Alternativ för bildförinställning {#image-preset-options}

När du skapar eller redigerar bildförinställningar finns alternativen som beskrivs i det här avsnittet. Dessutom rekommenderar Adobe att du börjar med följande alternativ:

* **[!UICONTROL Format]** (**[!UICONTROL Basic]** tab) - Välj **[!UICONTROL JPEG]** eller ett annat format som uppfyller dina krav. Alla webbläsare har stöd för JPEG-bildformatet. Det ger en bra balans mellan små filstorlekar och bildkvalitet. I bilder med JPEG-format används dock förstörande komprimering, som kan ge upphov till oönskade bildartefakter om komprimeringsinställningen är för låg. Därför rekommenderar Adobe att du ställer in komprimeringskvaliteten på 75. Den här inställningen ger en bra balans mellan bildkvalitet och liten filstorlek.

* **[!UICONTROL Enable Simple Sharpening]** – Markera inte **[!UICONTROL Enable Simple Sharpening]** (det här skärpefiltret ger mindre kontroll än inställningarna för Oskarp mask).

* **[!UICONTROL Sharpening: Resampling Mode]** - Välj **[!UICONTROL Sharp2]**.

#### Alternativ på fliken Grundläggande {#basic-tab-options}

<table>
 <tbody>
  <tr>
   <td><strong>Fält</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td><strong>Namn</strong></td>
   <td>Ange ett beskrivande namn utan blanksteg. Inkludera bildstorleksspecifikationen i namnet så att användarna lättare kan identifiera den här bildförinställningen.</td>
  </tr>
  <tr>
   <td><strong>Bredd och höjd</strong></td>
   <td>Ange bildstorleken i pixlar. Bredd och höjd måste vara större än 0 pixlar. Om något av värdena är 0 skapas ingen förinställning. Om båda värdena är tomma skapas en responsiv bildförinställning.</td>
  </tr>
  <tr>
   <td><strong>Format</strong></td>
   <td><p>Välj ett format på menyn.</p> <p>Om du väljer <strong>JPEG</strong> får du tillgång till följande alternativ:</p>
    <ul>
     <li><strong>Kvalitet</strong> - Styr JPEG-komprimeringsnivån. Den här inställningen påverkar både filstorlek och bildkvalitet. JPEG kvalitetsskala är 1-100. Skalan visas när du drar i skjutreglaget.</li>
     <li><strong>Aktivera JPG krominance-nedsampling</strong> - Eftersom ögat är mindre känsligt för högfrekvent färginformation än högfrekvent luminans delas bildinformationen upp i JPEG-bilder i luminans- och färgkomponenter. När en JPEG-bild komprimeras lämnas luminanskomponenten i full upplösning, medan färgkomponenterna nedsamplas genom att medelvärdet av grupper med pixlar ökas. Nedsampling minskar datavolymen med en halv eller en tredjedel utan att det påverkar den upplevda kvaliteten. Nedsampling kan inte användas för gråskalebilder. Den här tekniken minskar mängden komprimering som är användbar för bilder med hög kontrast (till exempel bilder med överlagrad text).</li>
    </ul>
    <div>
      Välja
     <strong>GIF</strong> eller
     <strong> GIF med alfa </strong> innehåller dessa ytterligare
     Alternativ för <strong> GIF-färgkvantifiering</strong> :
    </div>
    <ul>
     <li><strong>Typ </strong>- Välj <strong>Adaptiv</strong> (standard), <strong>Webb</strong> eller <strong>Macintosh</strong>. Om du väljer <strong>GIF med Alpha</strong> är Macintosh-alternativet inte tillgängligt.</li>
     <li><strong>Gitter</strong> - Välj <strong>Diffusera</strong> eller <strong>Av</strong>.</li>
     <li><strong>Antal färger </strong>- Ange ett tal mellan 2 och 256.</li>
     <li><strong>Färglista</strong> - Ange en kommaavgränsad lista. Ange till exempel <code>000000,888888,ffffff</code> för vitt, grått och svart.</li>
    </ul>
    <div>
      Välja
     <strong>PDF</strong>,
     <strong>TIFF</strong> eller
     <strong>TIFF med alfa</strong> innehåller ytterligare ett alternativ:
    </div>
    <ul>
     <li><strong>Komprimering</strong> - Välj en komprimeringsalgoritm. Algoritmalternativen för PDF är <strong>Ingen</strong>, <strong>Zip</strong> och <strong>Jpeg</strong>. För TIFF är alternativen <strong>Ingen</strong>, <strong>LZW</strong>, <strong>Jpeg</strong> och <strong>Zip</strong> och för TIFF med Alpha är <strong>Ingen</strong> 15}, <strong>LZW</strong> och <strong>Zip</strong>.</li>
    </ul> <p>Om du väljer <strong>PNG</strong>, <strong> PNG med Alpha,</strong> eller <strong>EPS</strong> finns inga ytterligare alternativ.</p> </td>
  </tr>
  <tr>
   <td><strong>Skärpa</strong></td>
   <td>Välj alternativet <strong>Aktivera enkel skärpa</strong> om du vill använda ett grundläggande skärpefilter på bilden när all skalning har gjorts. Skärpa kan kompensera för oskärpa som kan uppstå när du visar en bild i en annan storlek. </td>
  </tr>
 </tbody>
</table>

#### Avancerade flikalternativ {#advanced-tab-options}

<table>
 <tbody>
  <tr>
   <td><strong>Fält</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td><strong>Färgmodell</strong></td>
   <td>Välj <strong>RGB, CMYK,</strong> eller <strong>Gråskala</strong> som färgrymd.</td>
  </tr>
  <tr>
   <td><strong>Färgprofil</strong></td>
   <td>Välj den utdatafärgrymdsprofil som resursen ska konverteras till om den skiljer sig från arbetsprofilen.</td>
  </tr>
  <tr>
   <td><strong>Återgivningsmetod</strong></td>
   <td>Du kan åsidosätta standardåtergivningsmetoden. Återgivningsmetoden avgör vad som händer med färger som inte kan återges i målfärgprofilen (ej tryckbart). Återgivningsmetoden ignoreras om den inte är kompatibel med ICC-profilen.
    <ul>
     <li>Välj <strong>Perceptuell</strong> om du vill komprimera det totala färgomfånget från en färgrymd till en annan när en eller flera färger i den ursprungliga bilden är utanför färgomfånget för målfärgrymden.</li>
     <li>Välj <strong>Relativa färgvärden</strong> när en färg i den aktuella färgrymden inte är tryckbar i målfärgrymden. Och du vill mappa den till närmaste möjliga färg inom färgomfånget för målfärgrymden utan att påverka några andra färger. </li>
     <li>Välj <strong>Mättnad</strong> om du vill återge den ursprungliga bildens färgmättnad när du konverterar till målfärgrymden. </li>
     <li>Välj <strong>Absolut färgvärde</strong> om du vill matcha färgerna exakt utan justering för vitpunkt eller svartpunkt som skulle ändra bildens intensitet.</li>
    </ul> </td>
  </tr>
  <tr>
   <td><strong>Svartpunktskompensation</strong></td>
   <td>Välj det här alternativet om utdataprofilen har stöd för den här funktionen. Svartpunktskompensation ignoreras om den inte är kompatibel med den angivna ICC-profilen.</td>
  </tr>
  <tr>
   <td><strong>Gitter</strong></td>
   <td>Välj det här alternativet om du vill undvika eller minska färgränder. </td>
  </tr>
  <tr>
   <td><strong>Skärpetyp</strong></td>
   <td><p>Välj <strong>Ingen</strong>, <strong>Skärpa</strong> eller <strong>Oskarp mask</strong>. </p>
    <ul>
     <li>Välj <strong>Inget</strong> om du vill inaktivera skärpa.</li>
     <li>Välj <strong>Skärpa</strong> om du vill använda ett grundläggande skärpefilter på bilden när all skalning har gjorts. Skärpa kan kompensera för oskärpa som kan uppstå när du visar en bild i en annan storlek. </li>
     <li>Välj <strong> Oskarp mask </strong> om du vill finjustera en skärpefiltereffekt på den slutliga nedsamplade bilden. Du kan styra intensiteten för effekten, radien för effekten (mätt i pixlar) och ett tröskelvärde för kontrast som ignoreras. Den här effekten använder samma alternativ som Photoshop Oskarp mask-filter.</li>
    </ul> <p>I <strong>Oskarp mask</strong> har du följande alternativ:</p>
    <ul>
     <li><strong>Mängd</strong> - Anger mängden kontrast som används på kantpixlar. Standardvärdet för reella tal är 1,0. För högupplösta bilder kan du öka den till upp till 5.0. Tänk på Mängd som ett mått på filterintensiteten.</li>
     <li><strong>Radie</strong> - Anger antalet pixlar runt kantpixlarna som påverkar skärpan. För högupplösta bilder anger du ett reellt tal mellan 1 och 2. Med ett lågt värde ökas skärpan endast för kantpixlarna. Med ett högt värde ökas skärpan för ett bredare intervall av pixlar. Vilket värde som är korrekt beror på bildens storlek.</li>
     <li><strong>Tröskelvärde</strong> - Anger det kontrastintervall som ska ignoreras när filtret för oskarp mask används. Med andra ord, det här alternativet avgör hur olika de pixlar som ska göras skarpare måste vara från det omgivande området innan de betraktas som kantpixlar och blir skarpare. Experimentera med heltalsvärden mellan 2 och 20 för att undvika brus. </li>
     <li><strong>Gäller för </strong> - Avgör om den oskarpa inställningen gäller för varje färg eller intensitet.</li>
    </ul>
    <div>
      Skärpa beskrivs i
     <a href="https://experienceleague.adobe.com/docs/experience-manager-65-2025/assets/sharpening_images.pdf">Skärpa bilder</a>.
    </div> </td>
  </tr>
  <tr>
   <td><strong>Omsamplingsläge</strong></td>
   <td>Välj ett <strong>omsamplingsläge</strong> -alternativ. Dessa alternativ gör bilden skarpare när den nedsamplas:
    <ul>
     <li><strong>Dubbellinjär</strong> - den snabbaste omsamplingsmetoden. Vissa aliaseringsartefakter är märkbara.</li>
     <li><strong>Bi-Cubic</strong> - Ökar CPU-användningen men ger skarpare bilder med mindre framträdande aliasartefakter.</li>
     <li><strong>Sharp2</strong> - Kan ge något tydligare resultat än bikubisk, men till en ännu högre CPU-kostnad.</li>
     <li><strong>Bi-Sharp</strong> - Väljer Photoshop standardomsampling för att minska bildstorleken, som kallas <strong>bikubisk skarpare</strong> i Adobe Photoshop.</li>
     <li><strong>Varje färg</strong> och <strong>Intensitet</strong> - varje metod kan baseras på färg eller intensitet. Som standard är <strong>Varje färg</strong> markerad.</li>
    </ul> </td>
  </tr>
  <tr>
   <td><strong>Utskriftsupplösning</strong></td>
   <td>Välj en upplösning för utskrift av den här bilden. 72 pixlar är standard.</td>
  </tr>
  <tr>
   <td><strong>Bildmodifierare</strong></td>
   <td><p>Förutom de vanliga bildinställningarna i användargränssnittet har Dynamic Media stöd för många avancerade bildändringar som du kan ange i fältet <strong>Bildmodifierare</strong>. De här parametrarna definieras i <a href="https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/c-command-reference.html#image-serving-api">Image Server Protocol-kommandoreferensen</a>.</p> <p>Viktigt: Följande funktioner i API:t stöds inte:</p>
    <ul>
     <li>Grundläggande kommandon för mallar och textåtergivning: <code>text= textAngle= textAttr= textFlowPath= textFlowXPath= textPath=</code> och <code>textPs=</code></li>
     <li>Localization commands: <code>locale=</code> och <code>req=xlate</code></li>
     <li><code>req=set</code> är inte tillgängligt för allmän användning.</li>
     <li><code>req=mbrset</code></li>
     <li><code>req=saveToFile</code></li>
     <li><code>req=targets</code></li>
     <li><code>template=</code></li>
     <li>Icke-kärnbaserade dynamiska medietjänster: SVG, bildåtergivning och web-to-Print</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## Definiera förinställningsalternativ för bilder med bildmodifierare {#defining-image-preset-options-with-image-modifiers}

Förutom alternativen på flikarna Grundläggande och Avancerat kan du definiera bildmodifierare som ger dig fler alternativ när du definierar bildförinställningar. Bildåtergivning är beroende av bildåtergivnings-API:t som definieras i detalj i [HTTP-protokollreferensen](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/c-command-reference.html#image-serving-api).

Nedan följer några grundläggande exempel på vad du kan göra med bildmodifierare.

>[!NOTE]
>
>Vissa bildmodifierare [kan inte användas i Experience Manager](#advanced-tab-options).

* [op_invert](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/r-op-invert.html#image-serving-api) - Inverterar varje färgkomponent för en negativ bildeffekt.

  ```xml
  &op_invert=1
  ```

  ![6_5_imagepreset-edit-invert](assets/6_5_imagepreset-edit-invert.png)

* [op_blur](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/r-op-blur.html#image-serving-api) - Använder ett oskärpefilter på bilden.

  ```xml
  &op_blur=7
  ```

  ![6_5_imagepreset-edit-blur](assets/6_5_imagepreset-edit-blur.png)

* Kombinerade kommandon - op_blur och op-invert

  ```xml
  &op_invert=1&op_blur=7
  ```

  ![chlimage_1-80](assets/chlimage_1-501.png)

* [op_brightness](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/r-op-brightness.html#image-serving-api) - Minskar eller ökar intensiteten.

  ```xml
  &op_brightness=58
  ```

  ![6_5_imagepreset-edit-brightness](assets/6_5_imagepreset-edit-brightness.png)

* [opac](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/r-opac.html#image-serving-api) - Justerar bildens opacitet. Du kan minska förgrundens opacitet.

  ```xml
  opac=29
  ```

  ![6_5_imagepreset-edit-opacity](assets/6_5_imagepreset-edit-opacity.png)

## Redigera bildförinställningar {#modifying-image-presets}

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och väljer sedan **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Image Presets]**.

   ![6_5_imagepreset-editpreset](assets/6_5_imagepreset-editpreset.png)

1. Välj en förinställning och klicka sedan på **[!UICONTROL Edit]**. Fönstret **[!UICONTROL Edit Image Preset]** öppnas.
1. Gör ändringar och klicka på **[!UICONTROL Save]** om du vill spara ändringarna eller på **[!UICONTROL Cancel]** om du vill avbryta ändringarna.

## Publicera förinställningar för Dynamic Media-bilder {#publishing-image-presets}

Om du använder läget Dynamic Media - Hybrid måste du publicera bildförinställningar manuellt.

(Om du kör Dynamic Media - Scene7-läge publiceras bildförinställningar automatiskt åt dig. Du behöver inte utföra de här stegen.)

**Så här publicerar du bildförinställningar i Dynamic Media - hybrid-läge:**

1. I Experience Manager klickar du på Experience Manager logotyp för att komma åt den globala navigeringskonsolen, klickar på verktygsikonen och navigerar till **[!UICONTROL Assets]** > **[!UICONTROL Image Presets]**.
1. Välj bildförinställningen eller flera bildförinställningar i listan med bildförinställningar och klicka på **[!UICONTROL Publish]**.
1. När bildförinställningen har publicerats ändras statusen från opublicerad till publicerad.

   ![chlimage_1-81](assets/chlimage_1-505.png)

## Ta bort förinställningar för dynamiska media-bilder {#deleting-image-presets}

1. I Experience Manager klickar du på Experience Manager logotyp för att öppna den globala navigeringskonsolen.
1. Välj ikonen **[!UICONTROL Tools]** och gå sedan till **[!UICONTROL Assets]** > **[!UICONTROL Image Presets]**.
1. Välj en förinställning och klicka sedan på **[!UICONTROL Delete]**. Dynamic Media bekräftar att du vill ta bort det. Välj **[!UICONTROL Delete]** om du vill ta bort eller välj **[!UICONTROL Cancel]** om du vill avbryta.
