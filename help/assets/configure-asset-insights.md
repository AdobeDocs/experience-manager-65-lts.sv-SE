---
title: Konfigurera Assets Insights för att få analyser.
description: Konfigurera Assets Insights i  [!DNL Adobe Experience Manager Assets].
contentOwner: AG
role: Developer,Admin
feature: Asset Insights,Asset Reports
solution: Experience Manager, Experience Manager Assets
exl-id: ce0e3ebd-9a72-458c-8bb9-80f00d2f1a74
source-git-commit: e3106e87f72484568667873c1772abd30a108e51
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 2%

---

# Konfigurera Assets Insights {#configure-asset-insights}

[!DNL Adobe Experience Manager Assets] hämtar användningsdata om digitala resurser som används av tredjepartswebbplatser från [!DNL Adobe Analytics]. Om du vill att Assets Insights ska kunna hämta dessa data och generera insikter måste du först konfigurera funktionen för integrering med [!DNL Adobe Analytics]. Om du vill använda den här funktionen i en lokal installation måste du köpa [!DNL Adobe Analytics] separat. Kunder på [!DNL Managed Services] får [!DNL Analytics] licens som paketerats med [!DNL Experience Manager]. Se [Managed Services produktbeskrivning](https://helpx.adobe.com/se/legal/product-descriptions/adobe-experience-manager-managed-services.html).

>[!NOTE]
>
>Insikter stöds endast och tillhandahålls för bilder.

1. I [!DNL Experience Manager] klickar du på **[!UICONTROL Tools]** > **[!UICONTROL Assets]**.

   ![chlimage_1-72](assets/chlimage_1-210.png)

1. Klicka på kortet **[!UICONTROL Insights Configuration]**.
1. Välj ett datacenter i guiden och ange dina autentiseringsuppgifter, inklusive namnet på din organisation, användarnamn och delad hemlighet.

   ![Konfigurera Adobe Analytics för Assets Insights i Experience Manager](assets/insights_config2.png)

   *Figur: Konfigurera [!DNL Adobe Analytics] för Assets Insights i [!DNL Experience Manager].*

1. Klicka på **[!UICONTROL Authenticate]**.
1. När [!DNL Experience Manager] har autentiserat dina autentiseringsuppgifter väljer du en **[!UICONTROL Report Suite]**-rapportsserie från listan [!DNL Adobe Analytics] där du vill att Assets Insights ska hämta data. Klicka på **[!UICONTROL Add]**.
1. Klicka på [!DNL Experience Manager] när **[!UICONTROL Done]** har konfigurerat rapportsviten.

## Sidspårare {#page-tracker}

När du har konfigurerat ditt [!DNL Adobe Analytics]-konto genereras sidspårningskoden åt dig. Om du vill att Assets Insights ska kunna spåra [!DNL Experience Manager] resurser som används på tredjepartswebbplatser, inkluderar du sidspårningskoden i webbplatskoden. Använd verktyget [!UICONTROL Page Tracker] i [!DNL Experience Manager Assets] för att generera sidspårningskod. Mer information om hur du inkluderar Page Tracker-koden på webbsidor från tredje part finns i [Använda sidspåraren och bädda in kod på webbsidor](/help/assets/use-page-tracker.md).

1. I [!DNL Experience Manager] klickar du på **[!UICONTROL Tools]** > **[!UICONTROL Assets]**.

   ![chlimage_1-73](assets/chlimage_1-214.png)

1. Klicka på **[!UICONTROL Navigation]**-kortet på sidan **[!UICONTROL Insights Page Tracker]**.
1. Klicka på **[!UICONTROL Download]** för att hämta sidspårningskod.
