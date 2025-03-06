---
title: Konfigurera komponenten Video
description: Lär dig hur du använder videokomponenten i Adobe Experience Manager för att placera en fördefinierad, färdig videoresurs på sidan.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
exl-id: 0eac1568-cddb-4d26-b04c-78a2681cf44a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Konfigurera komponenten Video {#configure-the-video-component}

Med [videokomponenten](/help/sites-authoring/default-components-foundation.md#video) kan du placera en fördefinierad, färdig videoresurs på sidan.

För att korrekt omkodning ska ske installerar administratören FFmpeg separat. Se [Installera MPEG och konfigurera AEM](#install-ffmpeg). Administratörer [Konfigurera videoprofiler](#configure-video-profiles) för användning med HTML5-element.

>[!CAUTION]
>
>Den här Foundation-komponenten har tagits bort. Adobe rekommenderar att du använder [Core Components Embed Component](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/embed.html) i stället.

>[!CAUTION]
>
>Den här komponenten förväntas inte längre fungera utan omfattande anpassning på projektnivå.

## Konfigurera videoprofiler {#configure-video-profiles}

Om du vill använda HTML5-element definierar du videoprofiler. De som väljs här används i ordning. Använd [Designläge](/help/sites-authoring/default-components-designmode.md) (endast Classic UI) och välj fliken **[!UICONTROL Profiles]** för att få åtkomst:

![chlimage_1-317](assets/chlimage_1-317.png)

I den här dialogrutan kan du även konfigurera designen för videokomponenten och parametrarna för [!UICONTROL Playback], [!UICONTROL Flash] och [!UICONTROL Advanced].

## Installera och konfigurera AEM {#install-ffmpeg}

Videokomponenten använder öppen källkod-produkten från tredje part för omkodning av videoklipp. Hämtat från [https://ffmpeg.org/](https://ffmpeg.org/). När du har installerat mpeg konfigurerar du AEM att använda en viss ljudkodek och specifika körningsalternativ.

Så här installerar du MPEG på **Windows**:

1. Hämta den kompilerade binärfilen som `ffmpeg.zip`.
1. Avarkivera till en mapp.
1. Ange systemmiljövariabeln `PATH` till &lt;*your-ffmpeg-location*>`\bin`.
1. Starta om AEM.

Så här installerar du MPEG på **macOS X**:

1. Installera Xcode på [developer.apple.com/xcode](https://developer.apple.com/xcode/).
1. Installera på [XQuartz](https://www.xquartz.org) om du vill hämta [X11](https://support.apple.com/en-us/100724).
1. Installera MacPorts som finns på [www.macports.org](https://www.macports.org/).
1. Kör `sudo port install ffmpeg`-kommandot på konsolen och följ instruktionerna på skärmen. Kontrollera att sökvägen till den körbara filen `FFmpeg` har lagts till i systemvariabeln `PATH`.

Så här installerar du FFmpeg på **macOS X 10.6** med den förkompilerade versionen:

1. Hämta den förkompilerade versionen.
1. Avarkivera den i katalogen `/usr/local`.
1. Kör `sudo ln -s /usr/local/Cellar/ffmpeg/0.6/bin/ffmpeg /usr/bin/ffmpeg` i konsolen. Ändra sökvägen efter behov.

Så här **konfigurerar du AEM**:

>[!NOTE]
>
>Dessa steg är bara nödvändiga om du behöver anpassa kodekarna ytterligare.

1. Öppna [!UICONTROL CRXDE Lite] i webbläsaren. Gå till [http://localhost:4502/crx/de](http://localhost:4502/crx/de).
2. Markera noden `/libs/settings/dam/video/format_aac/jcr:content` och kontrollera att nodegenskaperna är följande:

   * `audioCodec` är `aac`.
   * `customArgs` är `-flags +loop -me_method umh -g 250 -qcomp 0.6 -qmin 10 -qmax 51 -qdiff 4 -bf 16 -b_strategy 1 -i_qfactor 0.71 -cmp chroma -subq 8 -me_range 16 -coder 1 -sc_threshold 40 -b-pyramid normal -wpredp 2 -mixed-refs 1 -8x8dct 1 -fast-pskip 1 -keyint_min 25 -refs 4 -trellis 1 -direct-pred 3 -partitions i8x8,i4x4,p8x8,b8x8`.

3. Om du vill anpassa konfigurationen skapar du en övertäckning i noden `/apps/settings/` och flyttar samma struktur under noden `/conf/global/settings/`. Den kan inte redigeras i noden `/libs`. Om du till exempel vill täcka över sökvägen `/libs/settings/dam/video/fullhd-bp` skapar du den på `/conf/global/settings/dam/video/fullhd-bp`.

   >[!NOTE]
   >
   >Täck över och redigera hela profilnoden och inte bara den egenskap som behöver ändras. Sådana resurser löses inte via SlingResourceMerger.

4. Om du har ändrat någon av egenskaperna klickar du på **[!UICONTROL Save All]**.

>[!NOTE]
>
>Ändringar i standardarbetsflödesmodellerna som är färdiga bevaras inte när du uppgraderar din AEM-instans. Adobe rekommenderar att du kopierar de ändrade arbetsflödesmodellerna innan du redigerar dem. Du kan till exempel kopiera modellen [!UICONTROL DAM Update Asset] som inte finns i kartongen innan du redigerar steget för FMPEG-omkodning i modellen [!UICONTROL DAM Update Asset] för att välja videoprofilnamn som fanns före uppgraderingen. Sedan kan du täcka över noden `/apps` så att AEM kan hämta anpassade ändringar till modellen som är klar att användas.
