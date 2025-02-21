---
title: Product Cockpit
description: Arbeta med Product Cockpit som ger en enhetlig översikt över länkade produktkataloger och tillhörande innehåll.
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# Product Cockpit {#product-cockpit}

## Ökning {#overview}

I Product Cockpit finns en enhetlig översikt över länkade produktkataloger och tillhörande innehåll. Allt associerat innehåll har länkar som snabbt kommer åt det från cockpit.

Mellanlagrade produktdata inkluderar eventuell mutation i framtiden, t.ex. nya kategorier, produkter eller uppdaterade egenskaper.

>[!NOTE]
>
>Termen produktkatalog är utbytbar med e-handelsbutiker, butiksvy och liknande uttryck.

## Konfiguration {#configuration}

Produktkataloger måste konfigureras i AEM. Mer information finns i [konfigurera butik och kataloger](/help/commerce/cif/getting-started.md#catalog).

Aktivering av mellanlagrade katalogfunktioner kräver autentisering. Mer information finns i [Komma igång](/help/commerce/cif/getting-started.md).

>[!NOTE]
>
>Mellanlagrade katalogfunktioner är bara tillgängliga med Adobe Commerce- och tredjepartsanslutningar som stöder tokenbaserad autentisering.

## Öppnar produktdockan {#opening-product-cockpit}

Det enklaste sättet att komma åt produktdockan är via menyn Commerce i AEM huvudmeny. Det går också att använda Omnissearch (sök efter Commerce) eller öppna `https://<yourAEMInstance>/commerce.html`.

![AEM-menyn](/help/commerce/cif/assets/aem-menu.png)

## Bläddra i produktkataloger {#browsing-product-catalogs}

Produktkatalogen ordnas hierarkiskt efter produktkatalogstrukturen. Den första nivån visar katalogrotnivån för alla konfigurerade produktkataloger, inklusive metainformation om e-handelsserverdelen.

![Konfigurerade kataloger](/help/commerce/cif/assets/catalog-overview.png)

När du klickar på en kategori läses de underordnade objekten för den valda kategorin in.

![Kategoriunderordnade](/help/commerce/cif/assets/catalog-category-children.png)

När du klickar på en produkt läses produktvariationer in om sådana finns.

![Produktvariationer](/help/commerce/cif/assets/catalog-product-variation.png)

>[!NOTE]
>
>Produktkatalogdata i AEM är data som hämtas i realtid via den konfigurerade slutpunkten för e-handel. Inga produktkatalogdata lagras i AEM.

## Söker produktkataloger {#searching-product-catalog}

En textsökning i hela produktkatalogen finns på den vänstra filterfliken för att snabbt hitta produkter.

![sök](/help/commerce/cif/assets/search-cockpit.png)

## Bläddrar i den mellanlagrade produktkatalogen {#staged-product-catalogs}

Som standard visas data i produktkatalogen i produktcockpiten. Om du använder&quot;STAGED CATALOG&quot; på den vänstra filterfliken läses produktkatalogen in för ett valt datum.

![mellanlagrad katalog](/help/commerce/cif/assets/staged-cockpit.png)

## Egenskaper för produktkatalog {#catalog-properties}

Om du klickar på egenskapsikonen för en produkt eller kategori öppnas egenskapsvyn för det markerade objektet. Öppna egenskaper för en produktvariant är lika med öppna de huvudsakliga produktegenskaperna.

### Commerce Tabs {#tabs}

På flikarna Allmänt och Variant visas fördefinierade handelsegenskaper som kommer från e-handelsservern. Denna information (inkl. varianter) är skrivskyddade data i AEM eftersom arkivsystemet är e-handelsserverdelen. Variantfliken visas bara för produkter med varianter och visar en lista med alla varianter.

![katalogegenskaper](/help/commerce/cif/assets/catalog-properties.png)

### AEM Content Tabs {#content-tabs}

Dessa flikar, grupperade efter AEM innehållstyper (Experience Fragments, Content Fragments, Associated Assets), visar AEM-innehåll som är associerat med handelsobjektet. Åtgärden Visa detaljer öppnar en ny webbläsarflik med det valda innehållet.

![innehållsegenskaper](/help/commerce/cif/assets/content-properties.png)
