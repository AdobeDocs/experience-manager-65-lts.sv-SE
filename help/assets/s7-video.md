---
title: Video
description: Läs om den centraliserade hanteringen av videoresurser i Adobe Experience Manager Assets där du kan ladda upp videor för automatisk kodning till Dynamic Media Classic och få tillgång till Dynamic Media Classic-videor direkt från Experience Manager Assets. Tack vare Dynamic Media Classic videointegration kan optimerad video även användas på alla skärmar.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: managing-assets
content-type: reference
role: User, Admin
mini-toc-levels: 3
feature: Video
solution: Experience Manager, Experience Manager Assets
exl-id: a54d39c3-e3eb-4d09-b79e-b5284e6e3f0b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# Video {#video}

Adobe Experience Manager Assets har en centraliserad hantering av videor där du kan ladda upp videor direkt till Assets för automatisk kodning till Dynamic Media Classic och få tillgång till Dynamic Media Classic-videor direkt från Assets för framtagning av webbsidor.

Tack vare Dynamic Media Classic videointegration kan optimerad video även användas på alla skärmar (automatisk enhets- och bandbreddsidentifiering).

* Komponenten **[!UICONTROL Scene7 Video]** utför automatiskt enhets- och bandbreddsidentifiering för att spela upp video i rätt format och med rätt kvalitet på datorer, surfplattor och mobila enheter.
* Assets - Du kan inkludera adaptiva videouppsättningar i stället för bara enskilda videoresurser. En adaptiv videouppsättning innehåller alla videoåtergivningar som krävs för att spela upp video sömlöst på flera skärmar. En adaptiv videouppsättning grupperar versioner av samma video som är kodade med olika bithastigheter och format som 400 kbit/s, 800 kbit/s och 1 000 kbit/s. Du använder en adaptiv videouppsättning, tillsammans med S7-videokomponenten, för adaptiv videoströmning på flera skärmar, inklusive stationära datorer, iOS, Android™, BlackBerry® och Windows mobila enheter.
<!-- See [Scene7 documentation about adaptive video sets for more information](https://help.adobe.com/en_US/scene7/using/WS53492AE1-6029-45d8-BF80-F4B5CF33EB08.html). -->

## Om FFMPEG och Dynamic Media Classic {#about-ffmpeg-and-scene}

Standardprocessen för videokodning bygger på den FFMPEG-baserade integrationen med videoprofiler. Därför innehåller det körklara arbetsflödet för DAM-inmatning följande två åtgärder för ffmpeg-baserade arbetsflöden:

* FFMPEG-miniatyrbilder
* FFMPEG-kodning

Om du aktiverar och konfigurerar Dynamic Media Classic-integreringen tas inte dessa två arbetsflödessteg bort automatiskt från det körklara arbetsflödet för DAM-import. Om du redan använder den FFMPEG-baserade videokodningen i Adobe Experience Manager är det troligt att du har FFMPEG installerat i dina redigeringsmiljöer. I det här fallet kodas en ny video som har importerats med DAM två gånger: en gång från FFMPEG-kodaren och en gång från Dynamic Media Classic-integreringen.

Om du har konfigurerat och installerat den FFMPEG-baserade videokodningen i Experience Manager rekommenderar Adobe att du tar bort de två FFMPEG-arbetsflödena från arbetsflödena för DAM-inhämtning.

## Format som stöds {#supported-formats}

Följande format stöds för Video-komponenten Scene7:

* F4V H.264
* MP4 H.264

## Bestäm var videon ska överföras {#deciding-where-to-upload-your-video}

Hur du avgör var du ska överföra dina videoresurser beror på följande:

* Behöver du ett arbetsflöde för videoresursen?
* Behöver du versionskontroll för videoresursen?

Om svaret är ja på någon av eller båda dessa frågor överför du videon direkt till Adobe DAM. Om svaret är nej på båda frågorna överför du videon direkt till Dynamic Media Classic. Arbetsflödet för varje scenario beskrivs i följande avsnitt.

### Om du överför videon direkt till Adobe DAM {#if-you-are-uploading-your-video-directly-to-adobe-dam}

Om du behöver ett arbetsflöde eller en versionshantering för dina resurser ska du först överföra dem till Adobe DAM. Här följer det rekommenderade arbetsflödet:

1. Ladda upp videomaterialet till Adobe DAM och koda och publicera automatiskt till Dynamic Media Classic.
1. I Experience Manager får du åtkomst till videoresurser i WCM på fliken **[!UICONTROL Movies]** i Content Finder.
1. Skapa med komponenten **[!UICONTROL Scene7 Video]** eller **[!UICONTROL Foundation Video]**.

### Om du överför din video till Dynamic Media Classic {#if-you-are-uploading-your-video-to-scene}

Om du inte behöver ett arbetsflöde eller en versionshantering för dina resurser överför du dina resurser till Scene7. Här följer det rekommenderade arbetsflödet:

1. I Dynamic Media Classic konfigurerar [en schemalagd FTP-överföring och -kodning till Scene7 (automatiskt system)](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=sv-SE#upload-files-using-via-ftp).
1. I Experience Manager får du åtkomst till videoresurser i WCM på fliken **[!UICONTROL Scene7]** i Content Finder.
1. Skapa med komponenten **[!UICONTROL Scene7 Video]**.

## Konfigurera integrering med Scene7 Video {#configuring-integration-with-scene-video}

1. I **[!UICONTROL Cloud Services]** går du till din **[!UICONTROL Scene7]**-konfiguration och väljer **[!UICONTROL Edit]**.
1. Välj fliken **[!UICONTROL Video]**.

   ![chlimage_1-363](assets/chlimage_1-363.png)

   >[!NOTE]
   >
   >Fliken **[!UICONTROL Video]** visas inte om sidan inte har någon molnkonfiguration.

1. Välj den adaptiva videokodningsprofilen, en färdig videokodningsprofil eller en anpassad videokodningsprofil.

   >[!NOTE]
   >
   >Mer information om vad videoförinställningarna betyder finns i [Dynamic Media Classic-dokumentationen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=sv-SE#video-presets-for-encoding-video-files).
   >
   >Adobe rekommenderar att du antingen väljer båda adaptiva videouppsättningar när du konfigurerar de universella förinställningarna eller väljer alternativet **[!UICONTROL Adaptive Video Encoding]**.

1. De valda kodningsprofilerna tillämpas automatiskt på alla videoklipp som överförs till CQ DAM-målmappen som du konfigurerar för den här Scene7-molnkonfigurationen. Du kan konfigurera flera Scene7-molnkonfigurationer med olika målmappar för att tillämpa olika kodningsprofiler efter behov.

## Uppdatera förinställningar för visningsprogram och kodning {#updating-viewer-and-encoding-presets}

Om du vill uppdatera visnings- och kodningsförinställningarna för video eftersom förinställningarna uppdaterades i Scene7 går du till Scene7-konfigurationen i molnkonfigurationen och väljer **[!UICONTROL Update the viewer and encoding presets]**.

![chlimage_1-364](assets/chlimage_1-364.png)

## Överför din primära källvideo till Scene7 från Adobe DAM {#uploading-your-master-video}

1. Navigera till målmappen för CQ DAM där du har konfigurerat din molnkonfiguration med kodningsprofiler för Scene7.
1. Välj **[!UICONTROL Upload]** om du vill överföra primärt källvideoklipp. Överföringen och kodningen av videon har slutförts när arbetsflödet [!UICONTROL DAM Update Asset] har slutförts och **[!UICONTROL Publish to Scene7]** har en bock.

   >[!NOTE]
   >
   >Det tar tid innan videominiatyrbilderna genereras.

   Genom att dra den primära DAM-källvideon till videokomponenten får du tillgång till *alla* Scene7-kodade proxyåtergivningar för leverans.

## Foundation Video Component vs Scene7 Video Component {#foundation-video-component-versus-scene-video-component}

När du använder Experience Manager har du tillgång till både videokomponenten som är tillgänglig i Sites och Scene7-videokomponenten. Dessa komponenter är inte utbytbara.

Scene7-videokomponenten fungerar bara för Scene7-videofilmer. Grundeskomponenten fungerar med videor som lagras från Experience Manager (med ffmpeg) och Scene7.

I följande matris förklaras när du ska använda vilken komponent:

![chlimage_1-365](assets/chlimage_1-365.png)

>[!NOTE]
>
>Som standard använder S7-videokomponenten den universella videoprofilen. Du kan dock hämta den HTML5-baserade videospelaren som ska användas av Experience Manager. I Scene7 kopierar du inbäddningskoden för den färdiga HTML5-videospelaren och placerar den på din Experience Manager-sida.

## Experience Manager Video Component {#aem-video-component}

Även om det rekommenderas att du använder Scene7-videokomponenten för att visa Scene7-videofilmer beskrivs det här avsnittet av tydlighetsskäl hur du använder Scene7-videofilmer med Foundation Video Component i Experience Manager.

### Experience Manager Video and Scene7 Video Comparison {#aem-video-and-scene-video-comparison}

I följande tabell visas en högnivåjämförelse av funktioner som stöds mellan videokomponenten i Experience Manager Foundation och Video-komponenten i Scene7:

|   | Experience Manager Foundation Video | Scene7 Video |
|---|---|---|
| Metod | HTML5, första sättet. Flash används bara för andra objekt än HTML5. | Flash på de flesta datorer. HTML5 används för mobiler och surfplattor. |
| Leverans | Progressiv | Adaptiv strömning |
| Spårning | Ja | Ja |
| Utbyggbarhet | Ja | Nej |
| Mobilvideo | Ja | Ja |

### Konfigurera {#setting-up}

#### Skapa videoprofiler {#creating-video-profiles}

De olika videokodningarna skapas enligt de kodningsförinställningar för S7 som valts i molnkonfigurationen för S7. För att den grundläggande videokomponenten ska kunna använda dem måste en videoprofil skapas för varje vald S7-kodningsförinställning. Med den här metoden kan videokomponenten välja DAM-återgivningar utifrån detta.

>[!NOTE]
>
>Nya videoprofiler och ändringar av dem måste aktiveras för publicering.

1. I Experience Manager väljer du **[!UICONTROL Tools]** > **[!UICONTROL Configuration Console]**.
1. I **[!UICONTROL Configuration Console]** går du till **[!UICONTROL Tools]** > **[!UICONTROL DAM]** > **[!UICONTROL Video Profiles]** i navigeringsträdet.
1. Skapa en S7-videoprofil. I **[!UICONTROL New]**. väljer du **[!UICONTROL Create Page]** och sedan mallen Scene7-videoprofil. Ge den nya videoprofilsidan ett namn och välj **[!UICONTROL Create]**.

   ![chlimage_1-366](assets/chlimage_1-366.png)

1. Redigera den nya videoprofilen. Välj molnkonfigurationen först. Välj sedan samma kodningsförinställning som du valde i molnkonfigurationen.

   ![chlimage_1-367](assets/chlimage_1-367.png)

   | Egenskap | Beskrivning |
   |---|---|
   | Scene7 Cloud Config | Den molnkonfiguration som ska användas för kodningsförinställningarna. |
   | Scene7 Encoding Preset | Den kodningsförinställning som den här videoprofilen ska kopplas till. |
   | HTML5-videotyp | Med den här egenskapen kan du ange värdet för type-egenskapen i videokällelementet för HTML5. Den här informationen finns inte i S7-kodningsförinställningarna, men krävs för att återge videoklippen med videoelementet HTML5 på rätt sätt. En lista över vanliga format tillhandahålls, men kan skrivas över för andra format. |

   Upprepa det här steget för alla kodningsförinställningar som är markerade i molnkonfigurationen och som du vill använda i videokomponenten.

#### Konfigurera design {#configuring-design}

Komponenten **[!UICONTROL Foundation Video]** måste känna till vilka videoprofiler som ska användas för att skapa listan över videokällor. Öppna dialogrutan för videokomponentdesign och konfigurera komponentdesignen för användning av de nya videoprofilerna.

>[!NOTE]
>
>Om du använder komponenten **[!UICONTROL Foundation Video]** på en mobilsida upprepar du de här stegen i designen av mobilsidan.

>[!NOTE]
>
>Ändringar som görs i designen kräver att designen aktiveras för publicering.

1. Öppna **[!UICONTROL Foundation Video]**-komponentens designdialogruta och ändra till fliken **[!UICONTROL Profiles]**. Ta sedan bort färdiga profiler och lägg till de nya videoprofilerna för S7. Ordningen på profillistan i designdialogrutan definierar ordningen på videokällelementet vid återgivning.
1. I webbläsare som inte stöder HTML5 kan du konfigurera ett Flash-återfall med videokomponenten. Öppna dialogrutan för design av videokomponenter och ändra till fliken **[!UICONTROL Flash]**. Konfigurera Flash Player-inställningarna och tilldela en reservprofil för Flash Player.

#### Checklista {#checklist}

1. Skapa en S7-molnkonfiguration. Kontrollera att förinställningarna för videokodning är angivna och att importeraren körs.
1. Skapa en S7-videoprofil för varje videokodningsförinställning som har valts i molnkonfigurationen.
1. Videoprofilerna måste aktiveras.
1. Konfigurera designen för komponenten **[!UICONTROL Foundation Video]** på sidan.
1. Aktivera designen när du är klar med designändringarna.
