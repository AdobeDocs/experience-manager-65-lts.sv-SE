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
exl-id: aeb8e954-dd6c-4e18-bb78-6eaac86fa4b9
source-git-commit: cc96a14ebaf9f895a798b5f4904f5b4769b990bb
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Aktivera JSON-export för en komponent{#enabling-json-export-for-a-component}

Komponenter kan anpassas för att generera JSON-export av deras innehåll baserat på ett modellramverk.

## Ökning {#overview}

JSON-exporten baseras på [Sling Models](https://sling.apache.org/documentation/bundles/models.html) och ramverket [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130) (som i sin tur är beroende av [Jackson-anteckningar](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)).

Detta innebär att komponenten måste ha en Sling-modell om den måste exportera JSON. Följ därför de här två stegen för att aktivera JSON-export för alla komponenter.

* [Definiera en segmentmodell för komponenten](/help/sites-developing/json-exporter-components.md#define-a-sling-model-for-the-component)
* [Anteckna gränssnittet för segmenteringsmodellen](#annotate-the-sling-model-interface)

## Definiera en segmentmodell för komponenten {#define-a-sling-model-for-the-component}

Först måste en delningsmodell definieras för komponenten.

>[!NOTE]
>
>Ett exempel på hur du använder segmentmodeller finns i [Developing Sling Model Exporters in AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter).

Implementeringsklassen för Sling-modellen måste kommenteras med följande:

```java
@Model(... adapters = {..., ComponentExporter.class})
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
@JsonSerialize(as = MyComponent.class)
```

Om du gör det kan du se till att din komponent exporteras fristående med `.model`-väljaren och `.json`-tillägget.

Dessutom anges att klassen Sling Model kan anpassas till gränssnittet `ComponentExporter`.

>[!NOTE]
>
>Jackson-anteckningar anges inte på klassnivå för Sling Model, utan på gränssnittsnivå för Model. Detta är för att säkerställa att JSON-exporten betraktas som en del av komponent-API:t.

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

För att JSON-exportramverket ska kunna bearbeta det måste modellgränssnittet implementera gränssnittet `ComponentExporter` (eller `ContainerExporter` för en behållarkomponent).

Motsvarande Sling Model-gränssnitt ( `MyComponent`) kommenteras sedan med [ Jackson-anteckningar ](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations) för att definiera hur det ska exporteras (serialiseras).

Modellgränssnittet måste kommenteras korrekt för att definiera vilka metoder som ska serialiseras. Som standard serialiseras alla metoder som respekterar den vanliga namnkonventionen för get-ters och härleder deras JSON-egenskapsnamn naturligt från get-namnen. Den här metoden kan förhindras eller åsidosättas med `@JsonIgnore` eller `@JsonProperty` för att byta namn på JSON-egenskapen.

## Exempel {#example}

Core Components har stöd för JSON-export sedan version [1.1.0 av kärnkomponenterna](https://experienceleague.adobe.com/en/docs/experience-manager-core-components/using/introduction) och kan användas som referens.

Ett exempel finns i Sling Model-implementeringen av Image Core-komponenten och dess kommenterade gränssnitt.

KOD PÅ GITHUB

Koden för den här sidan finns på GitHub

* [Öppna aem-core-wcm-components-projekt på GitHub](https://github.com/adobe/aem-core-wcm-components)
* Hämta projektet som [en ZIP-fil](https://codeload.github.com/adobe/aem-core-wcm-components/zip/main)


## Relaterad dokumentation {#related-documentation}

* Avsnittet [Innehållsfragment i användarhandboken för Assets](https://experienceleague.adobe.com/en/docs/experience-manager-64/assets/home#)
* [Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md)
* [Skapa med innehållsfragment](/help/sites-authoring/content-fragments.md)
* [JSON-exporterare för innehållstjänster](/help/sites-developing/json-exporter.md)
* [Kärnkomponenter](https://experienceleague.adobe.com/en/docs/experience-manager-core-components/using/introduction) och komponenten [Innehållsfragment](https://experienceleague.adobe.com/en/docs/experience-manager-core-components/using/wcm-components/content-fragment-component)
