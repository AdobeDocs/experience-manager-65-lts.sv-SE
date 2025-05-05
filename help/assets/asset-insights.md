---
title: Assets Insights
description: Lär dig hur du med funktionen Assets Insights kan spåra användarbetyg och användningsstatistik för bilder som används på tredjepartswebbplatser, marknadsföringskampanjer och Adobe kreativa lösningar.
contentOwner: AG
role: User, Admin
feature: Asset Insights,Asset Reports
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 6f6cb4fb-d9c7-4f44-b4ba-adcad74f3a59
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 3%

---

# Assets Insights {#asset-insights}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

Med funktionen Assets Insights kan du spåra användarbetyg och användningsstatistik för bilder som används på tredjepartswebbplatser, marknadsföringskampanjer och Adobe kreativa lösningar. Det hjälper till att få insikter om deras prestanda och popularitet.

[!DNL Assets] Insikter fångar upp användaraktivitetsinformation, t.ex. hur många gånger en bild klassificeras, klickas och hur många gånger en bild läses in på webbplatsen. Det tilldelar poäng till bilder baserat på denna statistik. Du kan använda resultat och prestandastatistik för att välja populära bilder som ska ingå i kataloger, marknadsföringskampanjer och så vidare. Man kan till och med utforma arkiverings- och licensförnyelseregler baserat på denna statistik.

För att [!DNL Assets] insikter ska kunna samla in användningsstatistik för bilder från en webbplats måste du inkludera inbäddningskoden för bilden i webbplatskoden.

Om du vill att Assets Insights ska visa användningsstatistik för mediefiler måste du först konfigurera funktionen för att hämta rapportdata från Adobe Analytics. Mer information finns i [Konfigurera Assets Insights](/help/assets/configure-asset-insights.md). Om du vill använda den här funktionen i en lokal installation måste du köpa [!DNL Adobe Analytics] separat. Kunder på [!DNL Managed Services] får [!DNL Analytics] licens som paketerats med [!DNL Experience Manager]. Se [Managed Services produktbeskrivning](https://helpx.adobe.com/se/legal/product-descriptions/adobe-experience-manager-managed-services.html).

>[!NOTE]
>
>Insikter stöds endast och tillhandahålls för bilder.

## Visa statistik för en bild {#viewing-statistics-for-an-image}

Du kan visa Assets Insights-poängen från metadatasidan.

1. I användargränssnittet för [!DNL Assets] markerar du bilden och klickar sedan på **[!UICONTROL Properties]** i verktygsfältet.
1. Klicka på fliken **[!UICONTROL Insights]** på sidan Egenskaper.
1. Granska användningsinformationen för resursen på fliken **[!UICONTROL Insights]**. Avsnittet **[!UICONTROL Score]** beskriver den totala resursanvändningen och prestandan för en resurs.

   Användningspoäng beskriver hur många gånger resursen används i olika lösningar.

   **[!UICONTROL Impressions]**-poängen är antalet gånger som resursen läses in på webbplatsen. Siffran som visas under **[!UICONTROL Clicks]** är antalet gånger som användaren klickar på resursen.

1. Granska avsnittet **[!UICONTROL Usage Statistics]** för att ta reda på vilka enheter resursen var en del av och vilka kreativa lösningar som nyligen har använt den. Ju högre användning, desto större chans att resursen är populär bland användarna. Användningsdata visas under följande rubriker:

   * **Resurs**: Antalet gånger som resursen var en del av en samling eller en sammansatt resurs
   * **Webb och mobil**: Antalet gånger som resursen ingick i webbplatser och appar
   * **Socialt**: Antalet gånger som resursen användes i lösningar som Adobe Social och Adobe Campaign
   * **E-post**: Antalet gånger som resursen användes i e-postkampanjer

   ![usage_Statistics](assets/usage_statistics.png)

   >[!NOTE]
   >
   >Eftersom Assets Insights-funktionen vanligtvis hämtar data från Adobe Analytics i lösningar med regelbundna intervall, kanske inte avsnittet Lösningar visar de senaste data. Den tidsperiod som data visas för beror på schemat för hämtningsåtgärden som Assets Insights kör för att hämta Analytics-data.

1. Om du vill visa prestandastatistik för resursen grafiskt över en tidsperiod väljer du period i avsnittet **[!UICONTROL Performance Statistics]**. Detaljer, inklusive klick och visningar, visas som trendlinjer i ett diagram.

   ![chlimage_1-3](assets/chlimage_1-3.jpeg)

   >[!NOTE]
   >
   >Till skillnad från data i avsnittet Lösningar visar avsnittet Prestandastatistik de senaste data.

1. Om du vill hämta inbäddningskoden för resursen som du inkluderar på webbplatser för att få prestandadata klickar du på **[!UICONTROL Get Embed Code]** under miniatyrbilden för resursen. Mer information om hur du inkluderar din inbäddningskod på webbsidor från tredje part finns i [Använda sidspåraren och bädda in kod på webbsidor](/help/assets/use-page-tracker.md).

   ![chlimage_1-98](assets/chlimage_1-303.png)

## Visa sammanställd statistik för bilder {#viewing-aggregate-statistics-for-images}

Du kan visa bakgrundsmusik för alla resurser i en mapp samtidigt med **[!UICONTROL Insights View]**.

1. Navigera till mappen som innehåller resurserna som du vill visa insikter för i användargränssnittet för [!DNL Assets].
1. Klicka på Layout i verktygsfältet och välj sedan **[!UICONTROL Insights View]**.
1. På sidan visas användningsresultat för resurserna. Jämför omdömen om de olika tillgångarna och få insikter.

## Schemalägg bakgrundsjobb {#scheduling-background-job}

Assets Insights hämtar regelbundet användningsdata för resurser från Adobe Analytics rapporteringsprogram. Som standard kör Assets Insights ett bakgrundsjobb var 24:e timme för att hämta data. Du kan dock ändra både frekvens och tid genom att konfigurera tjänsten **[!UICONTROL Adobe CQ DAM Asset Performance Report Sync Job]** från webbkonsolen.

1. Klicka på logotypen [!DNL Experience Manager] och gå till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.
1. Öppna tjänstkonfigurationen för **[!UICONTROL Adobe CQ DAM Asset Performance Report Sync Job]**.

   ![chlimage_1-99](assets/chlimage_1-304.png)

1. Ange önskad schemaläggningsfrekvens och starttid för jobbet i egenskapsschemaläggaruttrycket. Spara ändringarna.
