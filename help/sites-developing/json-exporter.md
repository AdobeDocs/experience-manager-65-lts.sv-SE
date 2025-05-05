---
title: JSON-exporterare för innehållstjänster
description: AEM Content Services är utformat för att generalisera beskrivningen och leveransen av innehåll i/från AEM utöver fokus på webbsidor. De levererar innehåll till kanaler som inte är traditionella AEM webbsidor, med standardiserade metoder som kan användas av alla kunder.
contentOwner: User
content-type: reference
topic-tags: components
products: SG_EXPERIENCEMANAGER/6.5/SITES
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 8c66b978-872e-4f5e-8f64-1e2dfb7d7dde
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# JSON-exporterare för innehållstjänster{#json-exporter-for-content-services}

AEM Content Services är utformat för att generalisera beskrivningen och leveransen av innehåll i/från AEM utöver fokus på webbsidor.

De levererar innehåll till kanaler som inte är traditionella AEM webbsidor, med standardiserade metoder som kan användas av alla kunder. Dessa kanaler kan omfatta:

* [Enkelsidiga program](spa-walkthrough.md)
* Inbyggda mobilprogram
* andra kanaler och kontaktpunkter utanför AEM

Med innehållsfragment som använder strukturerat innehåll kan du tillhandahålla innehållstjänster genom att använda JSON-exporteraren för att leverera innehållet på alla AEM-sidor i JSON-datamodellsformat. Den här metoden kan sedan användas av dina egna program.

>[!NOTE]
>
>De funktioner som beskrivs här är tillgängliga för alla kärnkomponenter sedan [version 1.1.0 av kärnkomponenterna](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=sv-SE).

## JSON-exporterare med kärnkomponenter för innehållsfragment {#json-exporter-with-content-fragment-core-components}

Med AEM JSON-exporteraren kan du leverera innehållet på alla AEM-sidor i JSON-datamodellformat. Den här metoden kan sedan användas av dina egna program.

I AEM uppnås leveransen med tillägget väljare `model` och `.json`.

`.model.json`

1. En URL som:

   ```shell
   http://localhost:4502/content/we-retail/language-masters/en.model.json
   ```

1. Levererar innehåll som:

   ![chlimage_1-192](assets/chlimage_1-192.png)

Du kan också leverera innehållet i ett strukturerat innehållsfragment genom att specifikt rikta in det på det.

Använd hela sökvägen till fragmentet (med hjälp av `jcr:content`), till exempel med ett suffix som det.

`.../jcr:content/root/responsivegrid/contentfragment.model.json`

Sidan kan innehålla antingen ett enda innehållsfragment eller flera komponenter av olika typer. Du kan också använda funktioner som listkomponenter för att automatiskt söka efter relevant innehåll.

* En URL som:

  ```shell
  http://localhost:4502/content/we-retail/language-masters/en/manchester-airport/jcr:content/root/responsivegrid/contentfragment.model.json
  ```

* Levererar innehåll som:

  ![chlimage_1-193](assets/chlimage_1-193.png)

  >[!NOTE]
  >
  >Du kan [anpassa dina egna komponenter](/help/sites-developing/json-exporter-components.md) för att komma åt och använda dessa data.

  >[!NOTE]
  >
  >Även om det inte är en standardimplementering stöds [flera väljare, ](json-exporter-components.md#multiple-selectors) men `model` måste vara den första.

### Ytterligare information {#further-information}

Se även:

* ASSETS HTTP API

   * [ASSETS HTTP API](/help/assets/mac-api-assets.md)

* Sling Models:

   * [Sling Models - Associerar en modellklass med en resurstyp sedan 130](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)

* AEM med JSON:

   * [Hämta sidinformation i JSON-format](/help/sites-developing/pageinfo.md)

## Relaterad dokumentation {#related-documentation}

Mer information finns i:

* Avsnittet [Innehållsfragment i användarhandboken för Assets](/help/assets/content-fragments/content-fragments.md)

* [Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md)
* [Skapa med innehållsfragment](/help/sites-authoring/content-fragments.md)
* [Aktivera JSON-export för en komponent](/help/sites-developing/json-exporter-components.md)

* [Kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=sv-SE) och komponenten [Innehållsfragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/content-fragment-component.html?lang=sv-SE)
