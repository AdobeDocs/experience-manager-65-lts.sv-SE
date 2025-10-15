---
title: Migrering till tillägget AEM Commerce integration framework (CIF)
description: Så här migrerar du till tillägget AEM Commerce integration framework (CIF) från en gammal version.
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
exl-id: 847c33c1-17d6-447a-9f2c-91f2a81a3f04
source-git-commit: 981b175b039fd7ffbddf558a77d2da2fed52ad79
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 0%

---

# Migreringshandbok för Experience Manager Add-on {#cif-migration}

Den här guiden hjälper dig att identifiera de områden du behöver uppdatera för migrering av Experience Manager-tillägg.

## CIF Add-on

CIF-tillägget är tillgängligt via [Programdistributionsportalen](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=commerce*&2_group.propertyvalues.property=.%2Fjcr%3aContent%2Fmetadata%2FDc%3Aversion&2_group.propertyvalues.operation=equals&2_group.propertyvalues.0_values=target-version%3AEM%2F6-5-lts&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=t desc&layout=list&p.offset=0&p.limit=16). Det är kompatibelt och innehåller samma funktioner som CIF-tillägget för Experience Manager as a Cloud Service.

Se [Komma igång med AEM Content och Commerce](getting-started.md).

Adobe tillhandahåller [AEM CIF Core Components](https://github.com/adobe/aem-core-cif-components) som stöd för projekt som distribuerar CIF.

## Produktkatalog

Import av produktkatalogdata stöds inte av CIF-tillägget. Med hjälp av CIF tilläggsobjekt kan produkt- och katalogförfrågningar efterfrågas på begäran via realtidsanrop till en extern handelslösning. Gå till kapitlet Integrating för att lära dig mer om att integrera en e-handelslösning.

>[!TIP]
>
>Om det inte finns några API:er i realtid bör en extern produktcache med API:er användas för integreringen. Exempel: [Magento öppen källkod](https://business.adobe.com/se/products/magento/open-source.html).

## Produktkatalogupplevelser med AEM Rendering

Om du använder katalogutkast med Classic CIF måste du uppdatera produktkatalogarbetsflödet. CIF-tillägget återger nu produktkatalogupplevelser direkt med AEM katalogmallar. Du behöver inte längre replikera produktdata eller produktsidor.

## Icke-cacheable Data and Shopping Interaction

Begäranden på klientsidan om icke-cachelagrade data och interaktioner (t.ex. tillägg i kundvagnen, sökning) ska gå direkt till slutpunkten för e-handeln (antingen e-handelslösningen eller integreringslagret) via CDN/Dispatcher. Ta bort alla samtal där AEM bara var en proxy.
