---
title: Aktivera Assets Insights via DTM
description: Lär dig hur du använder Adobe Dynamic Tag Management (DTM) för att aktivera Assets Insights.
contentOwner: AG
role: User, Admin
feature: Asset Insights,Asset Reports
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 1%

---

# Aktivera Assets Insights via DTM {#enable-asset-insights-through-dtm}

Adobe Dynamic Tag Management är ett verktyg som aktiverar era verktyg för digital marknadsföring. Det tillhandahålls kostnadsfritt till Adobe Analytics-kunder. Du kan antingen anpassa din spårningskod för att göra det möjligt för andra CMS-lösningar att använda Assets Insights eller så kan du använda DTM för att infoga Assets Insights-taggar. Insikter stöds endast och tillhandahålls för bilder.

>[!CAUTION]
>
>Adobe DTM är ersatt med [!DNL Adobe Experience Platform] och kommer snart att nå [slutet av livet](https://medium.com/launch-by-adobe/dtm-plans-for-a-sunset-3c6aab003a6f). Adobe rekommenderar att du [använder [!DNL Adobe Experience Platform] för resursinsikter](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html).

Utför dessa steg för att aktivera Assets Insights via DTM.

1. Klicka på Experience Manager logotyp och gå till **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Insights Configuration]**.
1. [Konfigurera Experience Manager-driftsättning med DTM Cloud Service](/help/sites-administering/dtm.md)

   API-token bör vara tillgänglig när du har loggat in på [https://dtm.adobe.com](https://dtm.adobe.com/) och går till **[!UICONTROL Account Settings]** i användarprofilen. Detta steg är inte nödvändigt ur Assets Insights-synpunkt eftersom integrationen av Experience Manager Sites med Assets Insights fortfarande pågår.

1. Logga in på [https://dtm.adobe.com](https://dtm.adobe.com/) och välj ett företag efter behov.
1. Skapa eller öppna en befintlig webbegenskap

   * Välj fliken **[!UICONTROL Web Properties]** och klicka sedan på **[!UICONTROL Add Property]**.

   * Uppdatera fälten och klicka på **[!UICONTROL Create Property]**. Se [dokumentation](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).

   ![Skapa webbegenskapen Redigera](assets/Create-edit-web-property.png)

1. På fliken **[!UICONTROL Rules]** väljer du **[!UICONTROL Page Load Rules]** i navigeringsrutan och klickar på **[!UICONTROL Create New Rule]**.

   ![chlimage_1-58](assets/chlimage_1-194.png)

1. Expandera **[!UICONTROL JavaScript /Third Party Tags]**. Klicka sedan på **[!UICONTROL Add New Script]** på fliken **[!UICONTROL Sequential HTML]** för att öppna skriptdialogrutan.

   ![chlimage_1-59](assets/chlimage_1-195.png)

1. Klicka på Experience Manager logotyp och gå till **[!UICONTROL Tools]** > **[!UICONTROL Assets]**.
1. Klicka på **[!UICONTROL Insights Page Tracker]**, kopiera spårningskoden och klistra sedan in den i skriptdialogrutan som du öppnade i steg 6. Spara ändringarna.

   >[!NOTE]
   >
   >* `AppMeasurement.js` har tagits bort. Den förväntas bli tillgänglig via DTM:s Adobe Analytics-verktyg.
   >* Anropet till `assetAnalytics.dispatcher.init()` har tagits bort. Funktionen förväntas anropas när inläsningen av DTM:s Adobe Analytics-verktyg är klar.
   >* Beroende på var Assets Insights-sidspåraren finns (till exempel Experience Manager, CDN och så vidare) kan skriptkällans ursprung behöva ändras.
   >* För sidspåraren som är värd för Experience Manager ska källan peka på en publiceringsinstans med hjälp av värdnamnet för dispatcher-instansen.

1. Åtkomst till `https://dtm.adobe.com`. Klicka på **[!UICONTROL Overview]** i webbegenskapen och klicka på **[!UICONTROL Add Tool]** eller öppna ett befintligt Adobe Analytics-verktyg. När du skapar verktyget kan du ange **[!UICONTROL Configuration Method]** till **[!UICONTROL Automatic]**.

   ![Lägg till Adobe Analytics-verktyg](assets/Add-Adobe-Analytics-Tool.png)

   Välj rapportsviter för mellanlagring/produktion efter behov.

1. Expandera **[!UICONTROL Library Management]** och kontrollera att **[!UICONTROL Load Library at]** är inställd på **[!UICONTROL Page Top]**.

   ![chlimage_1-61](assets/chlimage_1-197.png)

1. Expandera **[!UICONTROL Customize Page Code]** och klicka på **[!UICONTROL Open Editor]**.

   ![chlimage_1-62](assets/chlimage_1-198.png)

1. Klistra in följande kod i fönstret:

   ```Java
   var sObj;
   
   if (arguments.length > 0) {
     sObj = arguments[0];
   } else {
     sObj = _satellite.getToolsByType('sc')[0].getS();
   }
   _satellite.notify('in assetAnalytics customInit');
   (function initializeAssetAnalytics() {
     if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
       _satellite.notify('assetAnalytics ready');
       /** NOTE:
           Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
           Be mindful about changing the AppMeasurement object as retrieved above.
       */
       assetAnalytics.dispatcher.init(
             "",  /** RSID to send tracking-call to */
             "",  /** Tracking Server to send tracking-call to */
             "",  /** Visitor Namespace to send tracking-call to */
             "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, for example, 'listVar1' */
             "",  /** eVar to put Asset ID for Asset Click Events in, for example, 'eVar3' */
             "",  /** event to include in tracking-calls for Asset Impression Events, for example, 'event8' */
             "",  /** event to include in tracking-calls for Asset Click Events, for example, 'event7' */
             sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
             );
       sObj.usePlugins = true;
       sObj.doPlugins = assetAnalytics.core.updateContextData;
       assetAnalytics.core.optimizedAssetInsights();
     }
     else {
       _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
     }
   })();
   ```

   * Sidinläsningsregeln i DTM innehåller bara koden `pagetracker.js`. Alla `assetAnalytics`-fält betraktas som åsidosättningar för standardvärden. De är inte obligatoriska som standard.
   * Koden anropar `assetAnalytics.dispatcher.init()` efter att ha kontrollerat att `_satellite.getToolsByType('sc')[0].getS()` har initierats och att `assetAnalytics,dispatcher.init` är tillgänglig. Du kan därför hoppa över att lägga till den i steg 11.
   * Som framgår av kommentarerna i Insights Page Tracker-koden (**[!UICONTROL Tools > Assets > Insights Page Tracker]**) är de tre första argumenten (RSID, Tracking Server och Visitor Namespace) irrelevanta när sidspåraren inte skapar ett `AppMeasurement` -objekt. Tomma strängar skickas i stället för att markera detta.\
     De återstående argumenten motsvarar det som har konfigurerats på sidan Insights-konfiguration (**[!UICONTROL Tools > Assets > Insights Configuration]**).
   * AppMeasurement-objektet hämtas genom att fråga `satelliteLib` om alla tillgängliga SiteCatalyst-motorer. Om flera taggar har konfigurerats ändrar du indexvärdet för arrayväljaren på rätt sätt. Posterna i arrayen ordnas enligt de SiteCatalyst-verktyg som finns i DTM-gränssnittet.

1. Spara och stäng fönstret Kodredigeraren och spara sedan ändringarna i verktygskonfigurationen.
1. Godkänn båda väntande godkännanden på fliken **[!UICONTROL Approvals]**. DTM-taggen är klar att infogas på webbsidan.
