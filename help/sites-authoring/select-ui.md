---
title: Välja användargränssnitt i AEM
description: Konfigurera vilket gränssnitt du använder för Adobe Experience Manager 6.5.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 508f9dfb-1a4e-45bd-acdd-48cc910bdd0f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 1%

---

# Välja användargränssnitt{#selecting-your-ui}

Det pekaktiverade användargränssnittet i Adobe Experience Manager (AEM) är standardgränssnittet. Det kan dock finnas tillfällen då användaren vill växla till det [klassiska användargränssnittet](/help/sites-classic-ui-authoring/classicui.md). Det finns flera alternativ för att göra detta.

Det finns olika platser där du kan definiera vilket användargränssnitt som ska användas:

* [Konfigurerar standardgränssnittet för din instans](#configuring-the-default-ui-for-your-instance)
Detta anger standardgränssnittet som ska visas vid användarinloggning. Användaren kan åsidosätta detta och välja ett annat användargränssnitt för sitt konto eller den aktuella sessionen.

* [Anger klassisk gränssnittsredigering för ditt konto](/help/sites-authoring/select-ui.md#setting-classic-ui-authoring-for-your-account)
Detta anger att användargränssnittet ska vara standard när sidor redigeras, men användaren kan åsidosätta detta och välja ett annat användargränssnitt för sitt konto eller den aktuella sessionen.

* [Växlar till det klassiska användargränssnittet för den aktuella sessionen](#switching-to-classic-ui-for-the-current-session)
Växlar till det klassiska användargränssnittet för den aktuella sessionen.

* När det gäller [sidredigering gör systemet vissa åsidosättningar i relation till användargränssnittet ](#ui-overrides-for-the-editor).

>[!CAUTION]
>
>Olika alternativ för att växla till det klassiska användargränssnittet är inte omedelbart tillgängliga. De måste konfigureras för din instans.
>
>Mer information finns i [Aktivera åtkomst till det klassiska användargränssnittet](/help/sites-administering/enable-classic-ui.md).

>[!NOTE]
>
>Instanser som uppgraderats från en tidigare version behåller det klassiska användargränssnittet för sidredigering.
>
>Efter uppgraderingen växlas inte sidredigering automatiskt till det beröringsaktiverade användargränssnittet, men du kan konfigurera detta med [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) i **tjänsten för användargränssnittsläge för WCM-redigering** ( `AuthoringUIMode`-tjänsten). Se [Gränssnittsåsidosättningar för redigeraren](#ui-overrides-for-the-editor).

## Konfigurera standardgränssnittet för din instans {#configuring-the-default-ui-for-your-instance}

En systemadministratör kan konfigurera användargränssnittet som visas vid start och inloggning med [Rotmappning](/help/sites-deploying/osgi-configuration-settings.md#daycqrootmapping).

Detta kan åsidosättas av användarens standardinställningar eller sessionsinställningar.

## Ange klassisk gränssnittsredigering för ditt konto {#setting-classic-ui-authoring-for-your-account}

Varje användare har tillgång till sina egna [användarinställningar](/help/sites-authoring/user-properties.md#userpreferences) för att definiera om de vill använda det klassiska användargränssnittet för sidredigering, i stället för standardgränssnittet.

Detta kan åsidosättas av sessionsinställningarna.

## Växla till Classic UI för den aktuella sessionen {#switching-to-classic-ui-for-the-current-session}

När du använder det beröringsaktiverade användargränssnittet kanske datoranvändare vill återgå till det klassiska användargränssnittet (endast datorer). Det finns flera metoder för att växla till det klassiska användargränssnittet för den aktuella sessionen:

* **Navigeringslänkar**

  >[!CAUTION]
  >
  >Det här alternativet för att växla till det klassiska användargränssnittet är inte omedelbart tillgängligt. Det måste konfigureras för din instans.
  >
  >
  >Mer information finns i [Aktivera åtkomst till det klassiska användargränssnittet](/help/sites-administering/enable-classic-ui.md).

  Om det här alternativet är aktiverat visas en ikon (en bildskärmssymbol) när du för musen över en tillämplig konsol. Om du trycker/klickar på det här alternativet öppnas rätt plats i det klassiska användargränssnittet.

  Länkarna från **Platser** till **platadmin**:

  ![syui-01](assets/syui-01.png)

* **URL**

  Det klassiska användargränssnittet kan nås via URL:en för välkomstskärmen på `welcome.html`. Till exempel:

  `https://localhost:4502/welcome.html`

  >[!NOTE]
  >
  >Det beröringskänsliga användargränssnittet kan nås via `sites.html`. Till exempel:
  >
  >
  >`https://localhost:4502/sites.html`

### Växla till Classic UI när du redigerar en sida {#switching-to-classic-ui-when-editing-a-page}

>[!CAUTION]
>
>Det här alternativet för att växla till det klassiska användargränssnittet är inte omedelbart tillgängligt. Det måste konfigureras för din instans.
>
>Mer information finns i [Aktivera åtkomst till det klassiska användargränssnittet](/help/sites-administering/enable-classic-ui.md).

Om det här alternativet är aktiverat är **Öppna det klassiska användargränssnittet** tillgängligt i dialogrutan **Sidinformation**:

![syui-02](assets/syui-02.png)

### Gränssnittsåsidosättningar för redigeraren {#ui-overrides-for-the-editor}

De inställningar som definieras av en användare eller systemadministratör kan åsidosättas av systemet om det finns sidredigering.

* När du redigerar sidor:

   * Den klassiska redigeraren måste användas när sidan öppnas med `cf#` i URL:en. Till exempel:
     `https://localhost:4502/cf#/content/geometrixx/en/products/triangle.html`

   * Användning av den pekaktiverade redigeraren är tvingad när `/editor.html` används i URL:en eller när en pekenhet används. Till exempel:
     `https://localhost:4502/editor.html/content/geometrixx/en/products/triangle.html`

* Alla tvång är tillfälliga och gäller endast för webbläsarsessionen

   * En cookie-uppsättning anges beroende på om beröringsaktiverad ( `editor.html`) eller klassisk ( `cf#`) används.

* När du öppnar sidor via `siteadmin` görs en kontroll av om följande finns:

   * Kakan
   * En användarinställning
   * Om ingen av dem finns används de definitioner som angetts i [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) i **tjänsten för gränssnittsläge för WCM-redigering** ( `AuthoringUIMode`-tjänsten) som standard.

>[!NOTE]
>
>Om [en användare redan har definierat en inställning för sidredigering](#settingthedefaultauthoringuiforyouraccount), åsidosätts den inte genom att OSGi-egenskapen ändras.

>[!CAUTION]
>
>På grund av användningen av cookies, vilket redan har beskrivits, rekommenderas inte att du antingen:
>
>* Redigera URL:en manuellt - En URL som inte är standard kan resultera i en okänd situation och bristande funktionalitet.
>* Ha båda redigerarna öppna samtidigt - till exempel i separata fönster.
