---
title: '[!DNL Assets] - startsida'
description: Anpassa  [!DNL Experience Manager Assets] hemsidan för en rik välkomstskärm, inklusive en ögonblicksbild av nyligen gjorda aktiviteter runt resurser.
contentOwner: AG
feature: Asset Management
role: Admin, User
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 1%

---

# [!DNL Adobe Experience Manager Assets] - startsidesupplevelse {#aem-assets-home-page-experience}

Anpassa startsidan för [!DNL Adobe Experience Manager Assets] om du vill få en bra välkomstskärmsupplevelse, inklusive en ögonblicksbild av nyligen gjorda aktiviteter runt resurser.

Hemsidan för [!DNL Assets] innehåller en omfattande och personlig välkomstskärm, som innehåller en ögonblicksbild av de senaste aktiviteterna, till exempel resurser som nyligen har visats eller överförts.

Hemsidan [!DNL Assets] är inaktiverad som standard. Så här aktiverar du den:

1. Öppna [!DNL Experience Manager] Configuration Manager `https://[aem_server]:[port]/system/console/configMgr`.
1. Öppna tjänsten **[!UICONTROL Day CQ DAM Event Recorder]**.
1. Välj **[!UICONTROL Enable this service]** för att aktivera aktivitetsinspelning.

   ![chlimage_1-250](assets/chlimage_1-250.png)

1. I listan **[!UICONTROL Event Types]** väljer du vilka händelser som ska spelas in och sparar ändringarna.

   >[!CAUTION]
   >
   >Om du aktiverar alternativen Visa, Visa projekt och Samlingar, ökar antalet inspelade händelser avsevärt.

1. Öppna tjänsten **[!UICONTROL DAM Asset Home Page Feature Flag]** från Configuration Manager `https://[aem_server]:[port]/system/console/configMgr`.
1. Välj alternativet `isEnabled.name` om du vill aktivera funktionen [!DNL Assets] hemsida. Spara ändringarna.

   ![chlimage_1-251](assets/chlimage_1-251.png)

1. Öppna dialogrutan **[!UICONTROL User Preferences]** och välj **[!UICONTROL Enable Assets Home Page]**. Spara ändringarna.

   ![Aktivera startsidan för resurser i dialogrutan Användarinställningar](assets/Annotation-color.png)

När du har aktiverat startsidan för [!DNL Assets] går du till användargränssnittet för [!DNL Assets] från navigeringssidan eller öppnar det direkt från URL:en `https://[aem_server]:[port]/aem/assetshome.html/content/dam`.

![Konfigurera upplevelselänk i Assets användargränssnitt](assets/config-experience-link.png)

Klicka på **[!UICONTROL Click here to configure your experience link]** för att lägga till ditt användarnamn, din bakgrundsbild och din profilbild.

Hemsidan för [!DNL Assets] innehåller följande avsnitt:

* Välkomstavsnitt
* Widget Section

**Välkomstavsnitt**

Om din profil finns visas ett välkomstmeddelande adresserat till dig i välkomstavsnittet. Dessutom visas din profilbild och en välkomstbild (om den redan är konfigurerad).

Om din profil är ofullständig visas ett allmänt välkomstmeddelande och en platshållare för din profilbild i välkomstavsnittet.

**Widget Section**

Det här avsnittet visas under välkomstavsnittet och visar widgetar som inte finns i kartongen under följande avsnitt:

* Aktivitet
* Senaste
* Upptäck

**Aktivitet**: I det här avsnittet visar widgeten **[!UICONTROL My Activity]** de senaste aktiviteterna som den inloggade användaren har utfört med resurser (inklusive resurser utan återgivningar), till exempel överföringar, hämtningar, skapande av resurser, redigeringar, kommentarer, anteckningar och delningar.

**Senaste**: Widgeten **[!UICONTROL Recently Viewed]** under det här avsnittet visar nyligen använda entiteter av den inloggade användaren, inklusive mappar, samlingar och projekt.

**Upptäck**: Widgeten **[!UICONTROL New]** under det här avsnittet visar resurser och återgivningar som nyligen har överförts till distributionen [!DNL Assets].

Aktivera **[!UICONTROL DAM Event Purge Service]** från Configuration Manager om du vill aktivera rensning av användaraktivitetsdata. När du har aktiverat den här tjänsten tas aktiviteter av den inloggade användaren som överskrider ett angivet nummer bort av systemet.

Välkomstskärmen innehåller enkla navigeringshjälpmedel, t.ex. ikoner i verktygsfältet för att komma åt mappar, samlingar och kataloger.

>[!NOTE]
>
>Om du aktiverar tjänsterna [!UICONTROL Day CQ DAM Event Recorder] och [!UICONTROL DAM Event Purge] ökas skrivåtgärderna till JCR och sökindexering, vilket avsevärt ökar inläsningen på servern [!DNL Experience Manager]. Den extra belastningen på servern [!DNL Experience Manager] kan påverka serverns prestanda.

>[!CAUTION]
>
>Att hämta, filtrera och rensa användaraktiviteter som krävs för hemsidan [!DNL Assets] medför höga prestanda. Därför bör administratörer konfigurera hemsidan effektivt för målanvändare.
>
>Adobe rekommenderar att administratörer och användare som utför gruppåtgärder undviker att använda funktionen Startsida för resurser för att förhindra att användaraktiviteter ökar. Dessutom kan administratörer exkludera inspelningsaktiviteter för specifika användare genom att konfigurera [!UICONTROL Day CQ DAM Event Recorder] från [!UICONTROL Configuration Manager].
>
>Om du använder funktionen rekommenderar Adobe att du schemalägger tömningsfrekvensen baserat på serverbelastningen.
