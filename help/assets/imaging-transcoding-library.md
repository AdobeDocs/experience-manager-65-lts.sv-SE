---
title: Konverteringsbibliotek för bildbehandling
description: Lär dig hur du konfigurerar och använder Adobe Imaging Transcoding Library, en bildbehandlingslösning som kan utföra grundläggande bildhanteringsfunktioner, inklusive kodning, omkodning, bildomsampling och storleksändring.
contentOwner: AG
role: Admin
feature: Renditions,Developer Tools,Asset Processing
solution: Experience Manager, Experience Manager Assets
exl-id: fb24c331-55c3-4166-bd4f-c26cece902fc
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# Konverteringsbibliotek för bildbehandling {#imaging-transcoding-library}

Adobe Imaging Transcoding Library är en tillverkarspecifik bildbehandlingslösning som kan utföra grundläggande bildhanteringsfunktioner, som:

* Kodning
* Omkodning (konverterar format som stöds)
* Bildomsampling, använda PS- och Intel IPP-algoritmer
* Bevara bitdjup och färgprofil
* JPEG-komprimering
* Storleksändring av bild

Bildkonverteringsbiblioteket har stöd för CMYK och fullt stöd för alfa, utom CMYK-Alpha.

Förutom stöd för ett stort antal filformat och profiler har Imaging Transcoding Library betydande fördelar jämfört med andra tredjepartslösningar när det gäller prestanda, skalbarhet och kvalitet. Här är några av fördelarna med att använda Imaging Transcoding Library:

* **Skalförändrar med ökad filstorlek eller upplösning**: Skalförändringen uppnås huvudsakligen genom den patenterade möjligheten hos Imaging Transcoding Library att ändra storlek samtidigt som filer avkodas. Detta gör att minnesanvändningen vid körning alltid är optimal och inte är en kvadratisk funktion som ökar filstorleken eller upplösningsmegapixlar. Konverteringsbiblioteket för bildbehandling kan bearbeta större och högupplösta filer (som innehåller högre megapixlar). Tredjepartsverktyg som ImageMagick kan inte hantera stora filer och krascher när sådana filer bearbetas.
* **Komprimerings- och storleksändringsalgoritmer för Photoshop**: Kompatibilitet med branschstandard vad gäller kvalitet för nedsampling (jämn, skarp och automatisk bikubisk) och komprimeringskvalitet. Imaging Transcoding Library utvärderar dessutom kvalitetsfaktorn för indatabilden och använder intelligent optimala tabeller och kvalitetsinställningar för utdatabilden. Detta ger filer av optimal storlek utan att den visuella kvaliteten äventyras.
* **Hög genomströmning:** Svarstiden är lägre och genomströmningen är konsekvent högre än ImageMagick. Därför bör Imaging Transcoding Library minska väntetiden för användare och kostnaden för värdtjänster.
* **Skalförändra bättre med samtidig inläsning:** Konverteringsbiblioteket för bildåtergivning fungerar optimalt under samtidiga inläsningsförhållanden. Den ger hög genomströmning med optimala CPU-prestanda, minnesanvändning och låg svarstid, vilket minskar kostnaderna för värdtjänster.

## Plattformar som stöds {#supported-platforms}

Bildkonverteringsbiblioteket är endast tillgängligt för distributionerna RHEL 8, RHEL 7 och CentOS 7.

>[!NOTE]
>
>Mac OS och andra *nix-distributioner (till exempel Debian och Ubuntu) stöds inte.

## Användning {#usage}

Kommandoradsargumenten för Imaging Transcoding Library kan innehålla följande:

```shell
 -destMime PNG/JPEG: Mime type of output rendition
 -BitDepth 8/16: Preserves Bit Depth. Bitdepth '4' is automatically converted to '8'
 -preserveBitDepth: Downscales Bit Depth (No upscaling)
 -preserveCMYK: Preserves CMYK color space
 -jpegQuality: Provides jpeg quality parameter (0-12 , corresponding to Photoshop qualities)
 -ResamplingMethod BiCubic/Lanczos/PSBicubic: Provides resampling methods. PSBicubic is a Photoshop quality resampling method.
 -resize
```

Du kan konfigurera följande alternativ för parametern `-resize`:

* `X`: Fungerar ungefär som [!DNL Experience Manager]. Till exempel -resize 319.
* `WxH`: Proportionerna bevaras inte, till exempel `-resize 319x319`.
* `Wx`: Korrigerar bredden och beräknar höjden med bibehållna proportioner. Exempel: `-resize 319x`.
* `xH`: Korrigerar höjden och beräknar bredden med bibehållna proportioner. Exempel: `-resize x319`.

```shell
 -AllowUpsampling (Resizes smaller images)
 -input <fileName>
 -output <fileName>
```

## Konfigurera bildkonverteringsbibliotek {#configuring-imaging-transcoding-library}

Om du vill konfigurera ITL-bearbetning skapar du en konfigurationsfil och uppdaterar arbetsflödet för att köra det.

### Skapa konfigurationsfil för extraherat paket {#create-conf-file}

Om du vill konfigurera biblioteket skapar du en CONF-fil som anger biblioteken med följande steg. Du behöver administratörs- eller rotbehörigheter.

1. Hämta paketet [Imaging Transcoding Library från Software Distribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/aem630/product/assets/aem-assets-imaging-transcoding-library-pkg) och installera det med Package Manager. Paketet är kompatibelt med [!DNL Experience Manager] 6.5.

1. Om du vill veta ett paket-ID för `com.day.cq.dam.cq-dam-switchengine` loggar du in på webbkonsolen och klickar på **[!UICONTROL OSGi]** > **[!UICONTROL Bundles]**. Du kan även öppna paketkonsolen genom att gå till URL:en för `https://[aem_server:[port]/system/console/bundles/`. Leta reda på paketet `com.day.cq.dam.cq-dam-switchengine` och dess ID.

1. Kontrollera att alla nödvändiga bibliotek extraheras genom att kontrollera mappen med kommandot `ls -la /aem65/author/crx-quickstart/launchpad/felix/bundle<id>/data/binaries/`, där mappnamnet skapas med paket-ID:t. Till exempel är kommandot `ls -la /aem65/author/crx-quickstart/launchpad/felix/bundle588/data/binaries/` om paket-ID är `588`.

1. Skapa filen `SWitchEngineLibs.conf` för att länka till biblioteket.

   ```shell
   cd `/etc/ld.so.conf.d`
   touch SWitchEngineLibs.conf
   vi SWitchEngineLibs.conf
   ```

1. Lägg till sökvägen `/aem65/author/crx-quickstart/launchpad/felix/bundle<id>/data/binaries/` till conf-filen med kommandot `cat SWitchEngineLibs.conf`.

1. Kör kommandot `ldconfig` om du vill skapa nödvändiga länkar och cacheminne.

1. Redigera `.bash_profile`-filen i det konto som används för att starta [!DNL Experience Manager]. Lägg till `LD_LIBRARY_PATH` genom att lägga till följande.

   ```shell
   LD_LIBRARY_PATH=.
   export LD_LIBRARY_PATH
   ```

1. Använd kommandot `echo $LD_LIBRARY_PATH` om du vill vara säker på att värdet för sökvägen är `.`. Utdata ska bara vara `.`. Om värdet inte är `.` startar du om sessionen.

### Konfigurera arbetsflödet för [!UICONTROL DAM Update Asset] {#configure-dam-asset-update-workflow}

Uppdatera arbetsflödet [!UICONTROL DAM Update Asset] så att biblioteket används för bearbetning av bilder.

1. I användargränssnittet för [!DNL Experience Manager] väljer du **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.

1. Öppna arbetsflödesmodellen **[!UICONTROL DAM Update Asset]** på sidan **[!UICONTROL Workflow Models]** i redigeringsläge.

1. Öppna arbetsflödessteget **[!UICONTROL Process Thumbnails]**. På fliken **[!UICONTROL Thumbnails]** lägger du till de MIME-typer som du vill hoppa över standardprocessen för generering av miniatyrbilder för i listan **[!UICONTROL Skip Mime Types]**.
Om du till exempel vill skapa miniatyrer för en TIFF-bild med hjälp av Imaging Transcoding Library, anger du `image/tiff` i fältet **[!UICONTROL Skip Mime Types]**.

1. Lägg till de MIME-typer som du vill hoppa över standardprocessen för generering av webbåtergivning för på fliken **[!UICONTROL Web Enabled Image]** i **[!UICONTROL Skip List]**. Om du till exempel hoppade över MIME-typen `image/tiff` i ovanstående steg lägger du till `image/tiff` i hopplistan.

1. Öppna steget **[!UICONTROL EPS thumbnails (powered by ImageMagick)]** och gå till fliken **[!UICONTROL Arguments]**. Lägg till de MIME-typer som du vill att Imaging Transcoding Library ska bearbeta i listan **[!UICONTROL Mime Types]**. Om du till exempel hoppade över MIME-typen `image/tiff` i ovanstående steg lägger du till `image/jpeg` i listan **[!UICONTROL Mime Types]**.

1. Ta bort eventuella standardkommandon.

1. Växla sidopanel och lägg till **[!UICONTROL SWitchEngine Handler]** från listan med steg.

1. Lägg till kommandon i [!UICONTROL SwitchEngine Handler] baserat på dina anpassade krav. Finjustera parametrarna för de kommandon som du anger så att de uppfyller dina krav. Om du till exempel vill bevara färgprofilen för din JPEG-bild lägger du till följande kommandon i listan **[!UICONTROL Commands]**:

   * `SWitchEngine -input ${file} -destMime PNG -resize 48 -output ${directory}cq5dam.thumbnail.48.48.png`
   * `SWitchEngine -input ${file} -destMime PNG -resize 140x100 -output ${directory}cq5dam.thumbnail.140.100.png`
   * `SWitchEngine -input ${file} -destMime PNG -resize 319 -output ${directory}cq5dam.thumbnail.319.319.png`
   * `SWitchEngine -input ${file} -destMime JPEG -resize 1280 -preserveCMYK -output ${directory}cq5dam.web.1280.1280.jpeg`

   ![kalkering](assets/chlimage_1-199.png)

1. (Valfritt) Generera miniatyrbilder från en mellanrendering med ett enda kommando. Den mellanliggande återgivningen fungerar som källa för att generera statiska återgivningar och webbåtergivningar. Den här metoden är snabbare än den tidigare metoden. Du kan emellertid inte använda anpassade parametrar för miniatyrbilder med den här metoden.

   ![kalkering](assets/chlimage_1-200.png)

1. Konfigurera parametrar på fliken **[!UICONTROL Web-Enabled Image]** om du vill generera webbåtergivningar.

1. Synkronisera den uppdaterade arbetsflödesmodellen för [!UICONTROL DAM Update Asset]. Spara arbetsflödet.

Du kontrollerar konfigurationen genom att överföra en TIFF-avbildning och övervaka filen error.log. Du kommer att märka `INFO` meddelanden med omnämnanden av `SwitchEngineHandlingProcess execute: executing command line`. Loggarna anger de återgivningar som genereras. När arbetsflödet har slutförts kan du visa de nya återgivningarna i [!DNL Experience Manager].

>[!MORELIKETHIS]
>
>* [MIME-typer som stöds, artikel](assets-formats.md#supported-image-transcoding-library)
