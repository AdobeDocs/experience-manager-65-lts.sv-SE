---
title: Lägg till vattenstämpel i dina digitala resurser
description: Lär dig hur du använder funktionen Vattenstämpel för att lägga till en digital vattenstämpel till resurser.
contentOwner: AG
role: User, Admin
feature: Asset Management
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 6c8b4ff5-28ac-4655-b310-4f0b0417bd63
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---

# Vattenstämpla era digitala resurser {#watermarking}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/watermark-assets.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Med [!DNL Adobe Experience Manager Assets] kan du lägga till en digital vattenstämpel till resurser som hjälper användarna att verifiera autenticiteten och copyrightäganderätten för resurserna. [!DNL Experience Manager Assets] stöder text som ska användas som vattenstämpel i PNG- och JPEG-filer.

Om du vill kunna använda vattenstämpel på resurser lägger du till vattenstämpelsteget i [!UICONTROL DAM Update Asset]-arbetsflödet.

1. Gå till användargränssnittet för [!DNL Experience Manager] och gå till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. Välj arbetsflödet **[!UICONTROL DAM Update Asset]** på sidan **[!UICONTROL Workflow Models]** och klicka på **[!UICONTROL Edit]**.

1. Dra **[!UICONTROL Add Watermark]**-steget från sidopanelen till arbetsflödet i [!UICONTROL DAM Update Asset].

   ![Dra [!UICONTROL Add Watermark]-steget och lägg till i [!UICONTROL DAM Update Asset]-arbetsflödet](assets/add_watermark_step_aem_assets.png)

   *Figur: Dra steget [!UICONTROL Add Watermark] och lägg till i arbetsflödet för [!UICONTROL DAM Update Asset].*

   >[!NOTE]
   >
   >Placera steget [!UICONTROL Add Watermark] var som helst före steget [!UICONTROL Process Thumbnail].

1. Öppna steget **[!UICONTROL Add Watermark]** för att visa dess egenskaper.
1. På fliken **[!UICONTROL Arguments]** anger du giltiga värden i de olika fälten, inklusive text, teckensnittstyp, storlek, färg, position, orientering och så vidare. Bekräfta ändringarna genom att klicka på **[!UICONTROL Done]**.

   ![Ange argumenten i steget Lägg till vattenstämpel i [!DNL Assets]](assets/arguments_add_watermark_aem_assets.png)

   *Bild: Ange argumenten i steget Lägg till vattenstämpel i [!DNL Assets].*

1. Spara arbetsflödet **[!UICONTROL DAM Update Asset]** med vattenstämpelsteget.
1. Ladda upp en exempelresurs från användargränssnittet [!DNL Assets]. Vattenstämpeln visas med teckensnittsstorlek, färg o.s.v. på den plats som du konfigurerade i ovanstående steg.

Om du vill lägga till en vattenstämpel i PDF-dokument med programkod eller med dynamisk information bör du överväga att använda erbjudandet [Experience Manager Document Services](/help/forms/using/overview-aem-document-services.md) .

## Tips och begränsningar {#tips-limitations}

* Endast textbaserade vattenstämplar stöds. Bilder används inte som vattenstämplar, även om du kan överföra bilder när du skapar [!UICONTROL Add Watermark Process].
* Endast PNG- och JPEG-filer kan ha vattenstämplar. Andra resursformat har ingen vattenstämpel.
