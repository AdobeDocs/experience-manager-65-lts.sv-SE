---
title: Migrering till tillägget AEM Commerce integration framework (CIF)
description: Så här migrerar du till tillägget AEM Commerce integration framework (CIF) från en gammal version.
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
exl-id: 847c33c1-17d6-447a-9f2c-91f2a81a3f04
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Migreringshandbok för Experience Manager Add-on {#cif-migration}

Den här guiden hjälper dig att identifiera de områden du behöver uppdatera för migrering av Experience Manager-tillägg.

## CIF Add-on

CIF-tillägg är tillgängligt för AEM 6.5 via [portalen för programvarudistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html). Det är kompatibelt och innehåller samma funktioner som CIF-tillägget för Experience Manager as a Cloud Service.

Se [Komma igång med AEM Content och Commerce](getting-started.md).

Adobe tillhandahåller [AEM CIF Core Components](https://github.com/adobe/aem-core-cif-components) som stöd för projekt som distribuerar CIF.

## Produktkatalog

Import av produktkatalogdata stöds inte av CIF-tillägget. Med hjälp av CIF tilläggsobjekt kan produkt- och katalogförfrågningar efterfrågas på begäran via realtidsanrop till en extern handelslösning. Gå till kapitlet Integrating för att lära dig mer om att integrera en e-handelslösning.

>[!TIP]
>
>Om det inte finns några API:er i realtid bör en extern produktcache med API:er användas för integreringen. Exempel: [Magento öppen källkod](https://business.adobe.com/products/magento/open-source.html).

## Produktkatalogupplevelser med AEM Rendering

Om du använder katalogutkast med Classic CIF måste du uppdatera produktkatalogarbetsflödet. CIF-tillägget återger nu produktkatalogupplevelser direkt med AEM katalogmallar. Du behöver inte längre replikera produktdata eller produktsidor.

## Icke-cacheable Data and Shopping Interaction

Begäranden på klientsidan om icke-cachelagrade data och interaktioner (t.ex. tillägg i kundvagnen, sökning) ska gå direkt till slutpunkten för e-handeln (antingen e-handelslösningen eller integreringslagret) via CDN/Dispatcher. Ta bort alla samtal där AEM bara var en proxy.
