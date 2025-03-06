---
title: Använd förinställningar för dynamiska mediabilder
description: Lär dig hur du kan aktivera resurser för att dynamiskt kunna leverera bilder i olika storlekar, i olika format eller med andra bildegenskaper som genereras dynamiskt.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
feature: Image Presets
role: User,Admin
solution: Experience Manager, Experience Manager Assets
exl-id: f4d3a5f1-9348-433f-9c9f-84075a7ab912
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 3%

---

# Använda förinställningar för dynamiska mediebilder {#applying-image-presets}

Med bildförinställningar kan resurser dynamiskt leverera bilder i olika storlekar, i olika format eller med andra bildegenskaper som genereras dynamiskt. Du kan välja en förinställning när du exporterar bilder. I förinställningen formateras bilderna om enligt de specifikationer som administratören har angett.

Du kan dessutom välja en bildförinställning som är responsiv (anges av knappen **[!UICONTROL RESS]** när du har markerat den).

I det här avsnittet beskrivs hur du använder bildförinställningar. [Administratörer kan skapa och konfigurera bildförinställningar](managing-image-presets.md).

>[!NOTE]
>
>Smart bildbehandling fungerar med befintliga bildförinställningar och använder intelligens under de sista millisekunderna i leveransen för att ytterligare minska bildfilens storlek baserat på webbläsarens eller nätverkets anslutningshastighet. Mer information finns i [Smart bildbehandling](imaging-faq.md).

Du kan använda en bildförinställning på en bild när du vill förhandsvisa den.

>[!NOTE]
>
>I Dynamic Media - Scene7-läget stöds endast bildförinställningar för bildresurser.

**Så här använder du förinställningar för dynamiska mediebilder:**

1. Öppna resursen och välj sedan **[!UICONTROL Renditions]** i den vänstra listen.

   >[!NOTE]
   >
   >* Statiska återgivningar visas i den övre halvan av rutan. Dynamiska återgivningar visas i den nedre halvan. Om du bara använder dynamiska återgivningar kan du använda URL-adressen för att visa bilden. Knappen **[!UICONTROL URL]** visas bara om du väljer en dynamisk återgivning. Knappen **[!UICONTROL RESS]** visas bara om du väljer en responsiv bildförinställning.
   >
   >* Systemet visar flera återgivningar när du väljer **[!UICONTROL Renditions]** i detaljvyn för en resurs. Du kan öka antalet förinställningar som visas. Se [Öka antalet bildförinställningar som visas](managing-image-presets.md#increasing-or-decreasing-the-number-of-image-presets-that-display).

   ![chlimage_1-208](assets/chlimage_1-208.png)

1. Gör något av följande:

   * Välj en dynamisk återgivning så att du kan förhandsgranska bildförinställningen.
   * Om du vill visa popup-fönstret väljer du **[!UICONTROL URL]**, **[!UICONTROL Embed]** eller **[!UICONTROL RESS]**.

   >[!NOTE]
   >
   >Om resursen *och* inte har publicerats än är knappen **[!UICONTROL URL]** (eller knapparna **[!UICONTROL URL]** och **[!UICONTROL RESS]** om tillämpligt) inte tillgänglig.
   >
   >Observera också att bildförinställningar automatiskt publiceras på en Dynamic Media-server.
