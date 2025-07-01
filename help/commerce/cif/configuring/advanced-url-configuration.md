---
title: Avancerade URL-konfigurationer
description: Lär dig hur du anpassar URL:er för produkt- och kategorisidor. Detta gör att implementeringar kan optimera URL:er för sökmotorer och främja identifiering.
sub-product: Commerce
doc-type: technical-video
activity: setup
audience: administrator
feature: Commerce Integration Framework
kt: 4933
thumbnail: 34350.jpg
solution: Experience Manager,Commerce
role: Admin, Developer
exl-id: 5f6171f8-20ca-4c31-a99f-a5bc07a63baf
source-git-commit: e4cf6ae3392cef2ffd7e8fff3226b50c95f5a248
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 2%

---

# Avancerade URL-konfigurationer {#url}

>[!NOTE]
>
>Sökmotoroptimering (SEO) har blivit en viktig fråga för många marknadsförare. Därför måste SEO-frågor behandlas i många AEM-projekt. Mer information finns i [Bästa metoder för SEO- och URL-hantering](/help/managing/seo-and-url-management.md).

[AEM CIF Core Components](https://github.com/adobe/aem-core-cif-components) innehåller avancerade konfigurationer för att anpassa URL:er för produkt- och kategorisidor. Många implementeringar anpassar dessa URL:er för sökmotoroptimering (SEO). Följande video visar hur du konfigurerar tjänsten `UrlProvider` och funktionerna i [Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html) för att anpassa URL:er för produkt- och kategorisidor.

>[!VIDEO](https://video.tv.adobe.com/v/34350/?quality=12)

## Konfiguration {#configuration}

Om du vill konfigurera tjänsten `UrlProvider` enligt SEO-kraven och behöver ett projekt måste du tillhandahålla en OSGI-konfiguration för konfigurationen för CIF URL Provider.

>[!NOTE]
>
>Sedan version 2.0.0 av AEM CIF Core Components innehåller URL-providerkonfigurationen endast fördefinierade url-format, i stället för det kostnadsfria format som kan konfigureras i 1.x-versioner. Dessutom har användningen av väljare för att skicka data i URL-adresser ersatts med suffix.

### URL-format för produktsida {#product}

Detta konfigurerar URL:erna för produktsidorna och stöder följande alternativ:

* `{{page}}.html/{{sku}}.html#{{variant_sku}}` (standard)
* `{{page}}.html/{{url_key}}.html#{{variant_sku}}`
* `{{page}}.html/{{sku}}/{{url_key}}.html#{{variant_sku}}`
* `{{page}}.html/{{url_path}}.html#{{variant_sku}}`
* `{{page}}.html/{{sku}}/{{url_path}}.html#{{variant_sku}}`

Om det finns [Venias referensarkiv](https://github.com/adobe/aem-cif-guides-venia):

* `{{page}}` ersätts med `/content/venia/us/en/products/product-page`
* `{{sku}}` ersätts med produktens SKU, till exempel `VP09`
* `{{url_key}}` ersätts med produktens `url_key`-egenskap, till exempel `lenora-crochet-shorts`
* `{{url_path}}` ersätts med produktens `url_path`, till exempel `venia-bottoms/venia-pants/lenora-crochet-shorts`
* `{{variant_sku}}` ersätts med den valda varianten, till exempel `VP09-KH-S`

Eftersom `url_path` har tagits bort använder de fördefinierade produkt-URL-formaten en produkts `url_rewrites` och väljer den med de flesta sökvägssegmenten som alternativ om `url_path` inte är tillgänglig.

Med exempeldata ovan ser en produktvariant-URL som formaterats med standardformatet för URL ut som `/content/venia/us/en/products/product-page.html/VP09.html#VP09-KH-S`.

### URL-format för kategorisida {#product-list}

Detta konfigurerar URL:erna för kategorierna eller produktlistsidorna och stöder följande alternativ:

* `{{page}}.html/{{url_path}}.html` (standard)
* `{{page}}.html/{{url_key}}.html`

Om det finns [Venias referensarkiv](https://github.com/adobe/aem-cif-guides-venia):

* `{{page}}` ersätts med `/content/venia/us/en/products/category-page`
* `{{url_key}}` ersätts av kategorins `url_key`-egenskap
* `{{url_path}}` ersätts av kategorins `url_path`

Med exempeldata ovan ser en kategorisidas URL som är formaterad med standardformatet för URL ut som `/content/venia/us/en/products/category-page.html/venia-bottoms/venia-pants.html`.

>[!NOTE]
> 
>`url_path` är en sammanfogning av `url_keys` för en produkts eller kategorins överordnade och produktens eller kategorins `url_key` avgränsade med `/` snedstreck.

### Specifik kategori-/produktsida {#specific-pages}

Det går att skapa [flera kategorier och produktsidor](multi-template-usage.md) för endast en viss delmängd av kategorier eller produkter i en katalog.

`UrlProvider` är förkonfigurerad för att generera djuplänkar till sådana sidor på instanser på författarnivå. Detta är användbart för redigerare som bläddrar på en webbplats i förhandsgranskningsläge, navigerar till en viss produkt- eller kategorisida och växlar tillbaka till redigeringsläget för att redigera sidan.

Vid publiceringsnivåinstanser å andra sidan bör katalogsidans URL-adresser hållas stabila så att de inte förlorar vinster på rankningar för sökmotorer. På grund av detta kommer instanser av publiceringsnivån inte att återge djuplänkar till specifika katalogsidor per standard. Om du vill ändra det här beteendet kan du konfigurera _CIF URL Provider Specific Page Strategy_ så att den alltid genererar särskilda sidadresser.

## Anpassade URL-format {#custom-url-format}

Om du vill ange ett anpassat URL-format som ett projekt kan implementera antingen [`ProductUrlFormat`](https://javadoc.io/doc/com.adobe.commerce.cif/core-cif-components-core/latest/com/adobe/cq/commerce/core/components/services/urls/ProductUrlFormat.html) eller [`CategoryUrlFormat`](https://javadoc.io/doc/com.adobe.commerce.cif/core-cif-components-core/latest/com/adobe/cq/commerce/core/components/services/urls/CategoryUrlFormat.html)-tjänstgränssnittet och registrera implementeringen som en OSGI-tjänst. Dessa implementeringar, om de är tillgängliga, ersätter det konfigurerade fördefinierade formatet. Om det finns flera registrerade implementeringar ersätter den med den högre rangordningen dem med den lägre rangordningen.

Implementeringarna av det anpassade URL-formatet måste implementera ett par metoder för att skapa en URL från angivna parametrar och för att tolka en URL för att returnera samma parametrar.

## Kombinera med delningskartor {#sling-mapping}

Förutom `UrlProvider` går det också att konfigurera [Kopplingsmappningar](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html) så att URL:er skrivs om och bearbetas. AEM Archetype-projektet innehåller även [en exempelkonfiguration](https://github.com/adobe/aem-cif-project-archetype/tree/master/src/main/archetype/samplecontent/src/main/content/jcr_root/etc/map.publish) som konfigurerar vissa Sling Mappings för port 4503 (publicera) och 80 (Dispatcher).

## Kombinera med AEM Dispatcher {#dispatcher}

URL-omskrivningar kan också göras med hjälp av AEM Dispatcher HTTP-server med modulen `mod_rewrite`. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) innehåller en referenskonfiguration för AEM Dispatcher som redan innehåller grundläggande [omskrivningsregler](https://github.com/adobe/aem-project-archetype/tree/master/src/main/archetype/dispatcher.cloud) för den genererade storleken.

## Exempel

Projektet [Venias referensarkiv](https://github.com/adobe/aem-cif-guides-venia) innehåller exempelkonfigurationer som visar hur anpassade URL:er används för produkt- och kategorisidor. På så sätt kan varje projekt skapa individuella URL-mönster för produkt- och kategorisidor efter sina SEO-behov. En kombination av CIF `UrlProvider` och Sling Mappings används enligt beskrivningen ovan.

>[!NOTE]
>
>Den här konfigurationen måste justeras med den externa domän som används av projektet. Samlingsmappningarna fungerar baserat på värdnamnet och domänen. Den här konfigurationen är därför inaktiverad som standard och måste aktiveras före distributionen. Om du vill göra det byter du namn på mappen för kopplingsmappning `hostname.adobeaemcloud.com` i `ui.content/src/main/content/jcr_root/etc/map.publish/https` enligt det domännamn som används och aktiverar den här konfigurationen genom att lägga till `resource.resolver.map.location="/etc/map.publish"` i projektkonfigurationen `JcrResourceResolver`.

## Ytterligare resurser

* [Referensarkiv för Venedig](https://github.com/adobe/aem-cif-guides-venia)
* [AEM Resource Mapping](/help/sites-deploying/resource-mapping.md)
* [Kopplingsmappningar](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)
