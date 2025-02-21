---
title: Aktivera JSON-export för en komponent
description: Komponenter kan anpassas för att generera JSON-export av deras innehåll baserat på ett modellramverk.
contentOwner: User
content-type: reference
topic-tags: components
products: SG_EXPERIENCEMANAGER/6.5/SITES
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Aktivera JSON-export för en komponent{#enabling-json-export-for-a-component}

Komponenter kan anpassas för att generera JSON-export av deras innehåll baserat på ett modellramverk.

## Ökning {#overview}

JSON-exporten baseras på [Sling Models](https://sling.apache.org/documentation/bundles/models.html) och ramverket [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130) (som i sin tur är beroende av [Jackson-anteckningar](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)).

Det innebär att komponenten måste ha en Sling-modell om den måste exportera JSON. Följ därför de här två stegen för att aktivera JSON-export för alla komponenter.

* [Definiera en segmentmodell för komponenten](/help/sites-developing/json-exporter-components.md#define-a-sling-model-for-the-component)
* [Anteckna gränssnittet för segmenteringsmodellen](#annotate-the-sling-model-interface)

## Definiera en delningsmodell för komponenten {#define-a-sling-model-for-the-component}

Först måste en segmentmodell definieras för komponenten.

>[!NOTE]
>
>Ett exempel på hur du använder segmentmodeller finns i [Developing Sling Model Exporters in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

Implementeringsklassen för Sling-modellen måste kommenteras med följande:

```java
@Model(... adapters = {..., ComponentExporter.class})
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
@JsonSerialize(as = MyComponent.class)
```

Detta garanterar att din komponent kan exporteras fristående med väljaren `.model` och tillägget `.json`.

Detta anger dessutom att klassen Sling Model kan anpassas till gränssnittet `ComponentExporter`.

>[!NOTE]
>
>Jackson-anteckningar anges inte på klassnivå för Sling Model, utan på gränssnittsnivå för Model. Detta för att säkerställa att JSON-exporten betraktas som en del av komponent-API:t.

>[!NOTE]
>
>Klasserna `ExporterConstants` och `ComponentExporter` kommer från paketet `com.adobe.cq.export.json`.

### Använda flera väljare {#multiple-selectors}

Även om det inte är ett standardanvändningsfall går det att konfigurera flera väljare förutom väljaren `model`.

```
https://<server>:<port>/content/page.model.selector1.selector2.json
```

I så fall måste väljaren `model` vara den första väljaren och tillägget måste vara `.json`.

## Anteckna gränssnittet för segmenteringsmodellen {#annotate-the-sling-model-interface}

Modellgränssnittet bör implementera gränssnittet `ComponentExporter` (eller `ContainerExporter` om det finns en behållarkomponent) för att JSON-exportramverket ska kunna användas.

Motsvarande Sling Model-gränssnitt ( `MyComponent`) kommenteras sedan med [ Jackson-anteckningar ](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations) för att definiera hur det ska exporteras (serialiseras).

Modellgränssnittet måste vara korrekt kommenterat för att definiera vilka metoder som ska serialiseras. Som standard serialiseras alla metoder som respekterar den vanliga namnkonventionen för get-ters och hämtar JSON-egenskapsnamnen naturligt från get-namnen. Detta kan förhindras eller åsidosättas med `@JsonIgnore` eller `@JsonProperty` för att byta namn på JSON-egenskapen.

## Exempel {#example}

Core Components har stöd för JSON-export sedan version [1.1.0 av kärnkomponenterna](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) och kan användas som referens.

Ett exempel finns i Sling Model-implementeringen av Image Core-komponenten och dess kommenterade gränssnitt.

KOD PÅ GITHUB

Koden för den här sidan finns på GitHub

* [Öppna aem-core-wcm-components-projekt på GitHub](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components)
* Hämta projektet som [en ZIP-fil](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/archive/master.zip)

## Relaterad dokumentation {#related-documentation}

Mer information finns i följande:

* Avsnittet [Innehållsfragment i användarhandboken för Assets](https://helpx.adobe.com/experience-manager/6-4/assets/user-guide.html?topic=/experience-manager/6-4/assets/morehelp/content-fragments.ug.js)

* [Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md)
* [Skapa med innehållsfragment](/help/sites-authoring/content-fragments.md)
* [JSON-exporterare för innehållstjänster](/help/sites-developing/json-exporter.md)
* [Kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) och komponenten [Innehållsfragment](https://helpx.adobe.com/experience-manager/core-components/using/content-fragment-component.html)
