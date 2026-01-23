---
title: 360/VR-video
description: Lär dig hur du arbetar med och använder 360- och VR-video (Virtual Reality) i Dynamic Media.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
feature: 360 VR Video
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 17e45464-3de4-40a8-b102-ccc9eaba92a3
source-git-commit: f27795b9acf834101d82937d9f9f142361816735
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# 360/VR-video {#vr-video}

360-gradersvyer spelar in en vy i alla riktningar samtidigt. De filmas med en omdirigerad kamera eller en samling kameror. Vid uppspelning på en platt skärm har användaren kontroll över visningsvinkeln. Uppspelning på mobila enheter använder vanligtvis de inbyggda gyroskopkontrollerna.

Dynamic Media - Scene7-läget har inbyggt stöd för leverans av 360 videomaterial. Som standard krävs ingen ytterligare konfiguration för visning eller uppspelning. Du levererar 360-video med standardvideotillägg som .mp4, .mkv och .mov. Den vanligaste kodeken är H.264.

I det här avsnittet beskrivs hur du arbetar med 360/VR Video Viewer för att återge ekvirektangulär video för en engagerande tittarupplevelse av ett rum, en egenskap, plats, landskap, medicinska procedurer och så vidare.

Spatial audio stöds inte för närvarande. Om ljudet blandas i stereo ändras inte balansen (L/R) när kunden ändrar kamerans visningsvinkel.

Se även [Hantera visningsförinställningar](/help/assets/managing-viewer-presets.md).

## 360 Video in action {#video-in-action}

Välj [Space Station 360](https://s7d1.scene7.com/s7viewers/html5/Video360Viewer.html?asset=Viewers/space_station_360-AVS) om du vill öppna ett webbläsarfönster och titta på en 360-gradersvideo. Under videouppspelningen drar du muspekaren till en ny plats för att ändra visningsvinkeln.

![360-videosampling med den internationella rymdstationen flytande i yttre rymden och jorden och solen bakom.](assets/6_5_360videoiss_simplified.png)
*Videobildruta från Space Station 360*

## 360/VR Video och Adobe Premiere Pro {#vr-video-and-adobe-premiere-pro}

Du kan använda Adobe Premier Pro för att visa och redigera 360/VR-film. Du kan till exempel placera logotyper och text på rätt sätt i en scen och använda effekter och övergångar som är särskilt utformade för ekvirektangulära medier.

Se [Redigera 360/VR-video](https://helpx.adobe.com/premiere-pro/how-to/edit-360-vr-video.html).

## Ladda upp material som ska användas med 360-videovisningsprogrammet {#uploading-assets-for-use-with-the-video-viewer}

360 videomaterial som överförs till Adobe Experience Manager kallas **Multimedia** på en Assets-sida, ungefär som vanliga videomaterial.

![6_5_360video-selectToReview](assets/6_5_360video-selecttopreview.png)
*En överförd 360-videobasch som visas i kortvyn. Resursen är märkt som Multimedia.*

**Överför resurser som ska användas med 360-videovisningsprogrammet:**

1. Skapa en mapp som är dedikerad till ditt 360-videomaterial.
1. [Använd en adaptiv videoprofil i mappen](/help/assets/video-profiles.md#applying-a-video-profile-to-folders).

   Om du återger 360-videoinnehåll ställs högre krav på källvideoupplösning och för kodad återgivningsupplösning än standardvideoinnehåll som inte är 360.

   Du kan använda den färdiga adaptiva videoprofilen som redan finns i Dynamic Media. Det ger dock betydligt lägre 360-videokvalitet än vad du skulle få för icke-360-video som är kodad med samma inställningar som återges med ett icke-360-videovisningsprogram. Om 360-video med hög kvalitet krävs gör du därför följande:

   * Det bästa är att ditt ursprungliga 360-videomaterial har någon av följande upplösningar:

      * 1080p - 1920 x 1080, känd som Full HD eller FHD upplösning eller
      * 2160p - 3840 x 2160, känd som upplösningen 4k, UHD eller Ultra HD. Den här stora skärmupplösningen finns oftast på tv-apparater och datorskärmar. Upplösningen 2160p kallas ofta för&quot;4k&quot; eftersom bredden är nästan 4 000 pixlar. Med andra ord har den fyra gånger så många pixlar som 1080p.

   * [Skapa en anpassad adaptiv videoprofil](/help/assets/video-profiles.md#creating-a-video-encoding-profile-for-adaptive-streaming) med återgivningar av högre kvalitet. Skapa till exempel en adaptiv videoprofil som innehåller följande tre inställningar:

      * width=auto; height=720; bitrate=2 500 kbit/s
      * width=auto; height=1080; bitrate=5000 kbit/s
      * width=auto; height=1440; bitrate=6600 kbit/s

   * Bearbeta 360-videomaterial i en mapp som är exklusivt dedikerad till 360-videomaterial.

   Detta tillvägagångssätt ställer större krav på nätverket och CPU för slutanvändaren.

1. [Överför videon till mappen](/help/assets/managing-video-assets.md#upload-and-preview-video-assets).

## Åsidosätt standardproportionerna för 360-videofilmer  {#overriding-the-default-aspect-ratio-of-videos}

För att en överförd resurs ska kvalificera sig som en 360-video som du tänker använda med 360-videovisningsprogrammet måste resursen ha proportionerna 2.

Som standard identifierar Experience Manager video som&quot;360&quot; om proportionerna (bredd/höjd) är 2.0. Om du är administratör kan du åsidosätta standardinställningen för proportioner på 2 genom att ange den valfria egenskapen `s7video360AR` i CRXDE Lite enligt följande:

* `/conf/global/settings/cloudconfigs/dmscene7/jcr:content`

   * **Egenskapstyp** - dubbel
   * **Värde** - flyttalsproportioner, standard 2.0.

När du har angett den här egenskapen börjar den gälla omedelbart för både befintliga videoklipp och nyligen överförda videoklipp.

Proportionerna gäller för 360 videoresurser för sidan med resursinformation och [Video 360 Media WCM-komponenten](/help/assets/adding-dynamic-media-assets-to-pages.md#dynamic-media-components).

Börja med att ladda upp 360 videor.

## Förhandsgranska 360-video {#previewing-video}

Du kan använda Förhandsgranska för att se hur 360-videon ser ut för kunderna och kontrollera att den beter sig som förväntat.

Se även [Redigera visningsförinställningar](/help/assets/managing-viewer-presets.md#editing-viewer-presets).

När du är nöjd med 360-videon kan du publicera den.

Se [Bädda in video- eller bildvisningsprogrammet på en webbsida](/help/assets/embed-code.md).
Se [Länka URL:er till ditt webbprogram](/help/assets/linking-urls-to-yourwebapplication.md). Den URL-baserade länkningsmetoden är inte möjlig om det interaktiva innehållet har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.
Se [Lägg till Assets för dynamiska media på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md).

**Så här förhandsgranskar du 360-video:**

1. I **[!UICONTROL Assets]** navigerar du till en befintlig 360-video som du har skapat. Välj 360-videomaterialet så att du kan öppna det i förhandsgranskningsläge.

   ![6_5_360video-selecttopreview-1](assets/6_5_360video-selecttopreview-1.png)

   Välj en 360-videoresurs så att du kan förhandsgranska videon.

1. Markera listrutan på förhandsgranskningssidan, nära det övre vänstra hörnet på sidan, och välj sedan **[!UICONTROL Viewers]**.

   ![6_5_360video-preview-viewers](assets/6_5_360video-preview-viewers.png)

   Välj **[!UICONTROL Video360_social]** i visningslistan och gör sedan något av följande:

   * Dra muspekaren över videon om du vill ändra visningsvinkeln för den statiska scenen.
   * Markera videoklippets **[!UICONTROL Play]**-knapp om du vill påbörja uppspelningen. När videon spelas upp drar du muspekaren över videon för att ändra visningsvinkeln.

   ![En skärmbild av den internationella rymdstationen som flyter i yttre rymden med jorden och solen i bakgrunden &#x200B;](assets/6_5_360video-preview-video360-social.png)*En 360-videobandbild.*

   * Välj **[!UICONTROL Video360VR]** i visningslistan.

     VR-video (Virtual Reality) är engagerande videomaterial som nås via virtuella verklighetshuvuden. Precis som med vanliga videor skapar du VR-videor i början när en video spelas in eller spelas in med 360-graderskameror.

   ![En skärmbild av en närbild av den internationella rymdstationen som flyter i yttre rymden med jorden och solen delvis synliga i bakgrunden](assets/6_5_360video-preview-video360vr.png)
   *En 360 VR-videobildbild.*

1. Välj **[!UICONTROL Close]** uppe till höger på förhandsgranskningssidan.

## Publicera 360-video {#publishing-video}

Publicera 360-videon så att du kan använda den. När du publicerar en 360-video aktiveras URL:en och Bädda in kod. Dessutom publiceras 360-videon i Dynamic Media Cloud, som är integrerat med ett CDN för skalbar och prestandaoptimerad leverans.

Mer information om hur du publicerar 360-video finns i [Publicera dynamiska medieresurser](/help/assets/publishing-dynamicmedia-assets.md).
Se även [Bädda in video- eller bildvisningsprogrammet på en webbsida](/help/assets/embed-code.md).
Se även [Länka URL:er till ditt webbprogram](/help/assets/linking-urls-to-yourwebapplication.md). Den URL-baserade länkningsmetoden är inte möjlig om det interaktiva innehållet har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.
Se även [Lägg till dynamiska medieresurser på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md).
