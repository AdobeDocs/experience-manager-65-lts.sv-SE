---
title: '[!DNL Adobe Camera Raw]-stöd för bearbetning av digitala resurser'
description: Lär dig hur du aktiverar  [!DNL Adobe Camera Raw] stöd i [!DNL Adobe Experience Manager Assets]
contentOwner: AG
role: Admin
feature: Developer Tools
solution: Experience Manager, Experience Manager Assets
exl-id: 8cf34359-b6e0-4c84-84ec-d9d2b27edc6c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 3%

---

# Bearbeta bilder med [!DNL Adobe Camera Raw] {#camera-raw-support}

Du kan aktivera [!DNL Adobe Camera Raw]-stödet för att bearbeta råfilsformat, som CR2, NEF och RAF, och återge bilderna i JPEG-format. Funktionen stöds i [!DNL Adobe Experience Manager Assets] med [Camera Raw-paketet](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/aem630/product/assets/aem-assets-cameraraw-pkg) som är tillgängligt från programvarudistribution.

>[!NOTE]
>
>Funktionen stöder endast JPEG-renderingar. Det stöds i Windows 64-bitars, Mac OS och RHEL 7.x.

Så här aktiverar du stöd för [!DNL Camera Raw] i [!DNL Experience Manager Assets]:

1. Hämta [[!DNL Camera Raw] paketet](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/product/assets/aem-assets-cameraraw-pkg-1.4.8.zip) från [!DNL Software Distribution].
1. Åtkomst till `https://[aem_server]:[port]/workflow`. Öppna arbetsflödet **[!UICONTROL DAM Update Asset]**.
1. Redigera steget **[!UICONTROL Process Thumbnails]**.
1. Ange följande konfiguration på fliken **[!UICONTROL Thumbnails]**:

   * **[!UICONTROL Thumbnails]**: `140:100:false, 48:48:false, 319:319:false`
   * **[!UICONTROL Skip Mime Types]**: `skip:image/dng, skip:image/x-raw-(.*)`

   ![chlimage_1-128](assets/chlimage_1-334.png)

1. Ange `audio/mpeg, video/(.*), image/dng, image/x-raw-(.*)` i fältet **[!UICONTROL Skip List]** på fliken **[!UICONTROL Web Enabled Image]**.

   ![chlimage_1-129](assets/chlimage_1-335.png)

1. Lägg till steget **[!UICONTROL Camera Raw/DNG Handler]** under steget **[!UICONTROL Process Thumbnails]** från sidopanelen.
1. I steget **[!UICONTROL Camera Raw/DNG Handler]** lägger du till följande konfiguration på fliken **[!UICONTROL Arguments]**:

   * **[!UICONTROL Mime Types]**: `image/dng` och `image/x-raw-(.*)`
   * **[!UICONTROL Command]**:

      * `DAM_Raw_Converter ${directory}/${filename} ${directory} cq5dam.web.1280.1280.jpeg 1280 1280`
      * `DAM_Raw_Converter ${directory}/${filename} ${directory} cq5dam.thumbnail.319.319.jpeg 319 319`
      * `DAM_Raw_Converter ${directory}/${filename} ${directory} cq5dam.thumbnail.140.100.jpeg 140 100`
      * `DAM_Raw_Converter ${directory}/${filename} ${directory} cq5dam.thumbnail.48.48.jpeg 48 48`

   ![chlimage_1-130](assets/chlimage_1-336.png)

1. Klicka på **[!UICONTROL Save]**.

>[!NOTE]
>
>Kontrollera att ovanstående konfiguration är samma som **[!UICONTROL Sample DAM Update Asset With Camera RAW and DNG Handling Step]**-konfigurationen.

Nu kan du importera Camera Raw-filer till Assets. När du har installerat Camera Raw-paketet och konfigurerat det arbetsflöde som krävs visas alternativet **[!UICONTROL Image Adjust]** i listan över sidorutor.

![chlimage_1-131](assets/chlimage_1-337.png)

*Figur: Alternativ i sidopanelen.*

![chlimage_1-132](assets/chlimage_1-338.png)

*Figur: Använd alternativet om du vill göra små ändringar i dina bilder.*

När du har sparat redigeringarna i en [!DNL Camera Raw]-bild skapas en ny återgivning `AdjustedPreview.jpg` för bilden. För andra bildtyper förutom [!DNL Camera Raw] återspeglas ändringarna i alla återgivningar.

## God praxis, kända problem och begränsningar {#best-practices}

Funktionen har följande begränsningar:

* Funktionen stöder endast JPEG-renderingar. Det stöds på 64-bitars Windows, Mac OS och RHEL 7.x.
* Metadatatillbakaskrivning stöds inte för RAW- och DNG-format.
* Biblioteket [!DNL Camera Raw] har begränsningar för det totala antalet pixlar som kan bearbetas samtidigt. För närvarande kan programmet bearbeta maximalt 65 000 pixlar på den långa sidan av en fil eller 512 MP, oavsett vilket villkor som påträffas först.
