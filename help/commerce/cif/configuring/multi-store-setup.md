---
title: Installation av Commerce Multi-Store
description: Lär dig att mappa olika butiksvyer från Adobe Commerce till AEM. Detta gör att projekt kan stödja flerspråkiga och flerspråkiga användningsområden.
sub-product: Commerce
doc-type: technical-video
activity: setup
audience: administrator
feature: Commerce Integration Framework
solution: Experience Manager,Commerce
role: Admin, Developer
exl-id: 3a5d10d2-4ef8-4f85-942e-47ece6538acb
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Installation av Commerce Multi-Store {#multi-store}

AEM CIF Core Components kan användas på flera av AEM webbplatsstrukturer och den underliggande GraphQL-klientimplementeringen kan ansluta till olika Adobe Commerce butiker/butiksvyer. Detta gör att projekt kan implementera komplexa flerbutiks-/flerplatsinställningar.

En videogenomgång av olika alternativ för integrering av flera Adobe Commerce Store-vyer med Adobe Experience Manager Sites.

>[!VIDEO](https://video.tv.adobe.com/v/28952/?quality=12)

AEM funktioner för hantering av flera webbplatser i Live Copy och Language Copy används tillsammans med Commerce integration framework för att globalt hantera webbplatser i olika regioner och på olika språk.

Den rekommenderade konfigurationen är att använda en 1:1-relation mellan AEM webbplats och Adobe Commerce Store-vyn.

Följ stegen nedan för att ansluta en AEM-webbplats och AEM CIF Core Components så att även en dedikerad butiksvy kan visas:

## Konfiguration {#configuration}

1. Konfigurera flera butiker och butiksvyer enligt mönstret som beskrivs i [Adobe Commerce webbplatser, butiker och vyer](https://experienceleague.adobe.com/docs/commerce-admin/start/setup/websites-stores-views.html?lang=sv-SE)

2. Kontrollera att anslutningen mellan AEM och Adobe Commerce fungerar.

3. Skapa en underordnad konfiguration av CIF Cloud Service-konfigurationen enligt följande:

   * I AEM går du till Verktyg > Allmänt > [Konfigurationsläsaren](/help/sites-administering/configurations.md#using-configuration-browser)
   * Välj den baskonfiguration som du har skapat
   * Skapa en konfiguration med stegen som beskrivs i punkt 2 ovan

   Den nya konfigurationen skapas som en underordnad konfiguration till den grundläggande konfigurationen. Du kan nu gå till Verktyg > Allmänt > Konfigurationsläsaren och skapa konfigurationsinställningarna.

   >[!TIP]
   >
   >Commerce-kataloger kan hanteras med ID:n eller UID:n. UID introducerades i Adobe Commerce 2.4.2. Aktivera bara detta om e-handelsbackend har stöd för ett GraphQL-schema av version 2.4.2 eller senare.

4. Tilldela den underordnade konfigurationen till en AEM-plats

   * Gå till AEM Sites Console
   * Navigera till regionen eller språkroten i platsstrukturen, t.ex. /content/venia/us _eller_ /content/venia/us/en för exempelsidan i Venia
   * Markera sidan och öppna sidegenskaperna
   * Välj fliken Avancerat
   * I avsnittet `Configuration` väljer du konfigurationen som du skapade i steg

## Ytterligare resurser

* [Adobe Commerce webbplatser, butiker och vyer](https://experienceleague.adobe.com/docs/commerce-admin/start/setup/websites-stores-views.html?lang=sv-SE)
* [AEM CIF Core Components - konfiguration av flera butiker/platser](https://github.com/adobe/aem-core-cif-components#multi-store--site-configuration)
* [Använder Multi-Site Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/translation/multi-site-manager-feature-video-use.html?lang=sv-SE)
* [Återanvända innehåll: Multi Site Manager och Live Copy](/help/sites-administering/msm.md)
