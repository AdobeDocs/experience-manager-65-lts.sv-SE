---
title: Headless Content Delivery using Content Fragments with GraphQL
description: Lär dig använda AEM Content Fragments med GraphQL för leverans av headless-material.
feature: Content Fragments,Headless,GraphQL
role: User,Developer
solution: Experience Manager, Experience Manager Assets
exl-id: 8d0271c0-a795-4ff6-a2ae-72329f05a401
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Headless Content Delivery using Content Fragments with GraphQL {#headless-content-delivery-using-content-fragments-with-graphQL}

Med Adobe Experience Manager (AEM) kan du använda innehållsfragment tillsammans med AEM GraphQL API (en anpassad implementering som baseras på GraphQL-standard) för att leverera strukturerat innehåll som ska användas i dina program. Möjligheten att anpassa en enda API-fråga gör att du kan hämta och leverera det specifika innehåll som du vill ha/behöver återge (som svar på en enskild API-fråga).

<!--
>[!NOTE]
>
>See [Headless and AEM](/help/implementing/developing/headless/introduction.md) for an introduction to Headless Development for AEM Sites.
-->

>[!NOTE]
>
>GraphQL används för närvarande i två (separata) scenarier i Adobe Experience Manager (AEM):
>
>* [AEM Commerce använder data från en e-handelsplattform via GraphQL](/help/commerce/cif/integrating/magento.md).
>* [AEM Content Fragments fungerar tillsammans med AEM GraphQL API (en anpassad implementering som baseras på standard-GraphQL) för att leverera strukturerat innehåll som ska användas i dina program](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md).

## Headless CMS {#headless-cms}

Ett Headless Content Management System (CMS) är:

* &quot;*Ett headless-system för innehållshantering, eller headless CMS, är ett backend-system för innehållshantering (CMS) som byggts från grunden som en innehållsdatabas som gör innehåll tillgängligt via ett API för visning på alla enheter.*

  Se [Wikipedia](https://en.wikipedia.org/wiki/Headless_content_management_system).

När det gäller utveckling av innehållsfragment i AEM innebär detta följande:

* Du kan använda Innehållsfragment för att skapa innehåll som inte primärt är avsett att publiceras direkt (1:1) på formaterade sidor.

* Innehållet i dina innehållsfragment kommer att struktureras på ett förutbestämt sätt, enligt modellerna för innehållsfragment. Detta förenklar åtkomsten för dina program, som kommer att bearbeta innehållet ytterligare.

## GraphQL - en översikt {#graphql-overview}

GraphQL är:

* &quot;*...ett frågespråk för API:er och en körningsmiljö för att utföra dessa frågor med dina befintliga data.*&quot;.

  Se [GraphQL.org](https://graphql.org)

Med [AEM GraphQL API](#aem-graphql-api) kan du utföra (komplexa) frågor på dina [innehållsfragment](/help/assets/content-fragments/content-fragments.md), där varje fråga följer en viss modelltyp. Det returnerade innehållet kan sedan användas av dina program.

## AEM GRAPHQL API {#aem-graphql-api}

För Adobe Experience har en anpassad implementering av GraphQL standard-API utvecklats. Mer information finns i [AEM GraphQL API för användning med innehållsfragment](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md).

API-implementeringen för AEM GraphQL baseras på [GraphQL Java-biblioteken](https://graphql.org/code/#java).

## Innehållsfragment för användning med AEM GraphQL API {#content-fragments-use-with-aem-graphql-api}

[Innehållsfragment](#content-fragments) kan användas som bas för GraphQL för AEM-frågor som:

* Med dem kan du utforma, skapa, strukturera och publicera sidoberoende innehåll.
* [Modeller för innehållsfragment](#content-fragments-models) tillhandahåller den struktur som krävs med hjälp av definierade datatyper.
* [Fragmentreferensen](#fragment-references), som är tillgänglig när du definierar en modell, kan användas för att definiera ytterligare lager i strukturen.

![Innehållsfragment för användning med GraphQL](assets/cfm-nested-01.png "Innehållsfragment för användning med GraphQL")

### Innehållsfragment {#content-fragments}

Innehållsfragment:

* Innehåller strukturerat innehåll.

* De baseras på en [Content Fragment Model](#content-fragments-models) som fördefinierar strukturen för det resulterande fragmentet.

### Modeller för innehållsfragment {#content-fragments-models}

Dessa [modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md):

* Används för att generera [scheman](https://graphql.org/learn/schema/), en gång **Enabled**.

* Ange de datatyper och fält som krävs för GraphQL. De ser till att programmet bara begär det som är möjligt och får det som förväntas.

* Datatypen **[Fragmentreferenser](#fragment-references)** kan användas i modellen för att referera till ett annat innehållsfragment, vilket innebär att ytterligare strukturnivåer införs.

### Fragmentreferenser {#fragment-references}

**[Fragmentreferens](/help/assets/content-fragments/content-fragments-models.md#fragment-reference-nested-fragments)**:

* Är av särskilt intresse tillsammans med GraphQL.

* Är en specifik datatyp som kan användas när en innehållsfragmentmodell definieras.

* Refererar till ett annat fragment, beroende på en viss innehållsfragmentmodell.

* Gör att du kan hämta strukturerade data.

   * När det definieras som en **multifeed** kan flera delfragment refereras (hämtas) av det primära fragmentet.

### JSON Preview {#json-preview}

Om du vill ha hjälp med att designa och utveckla dina modeller för innehållsfragment kan du förhandsgranska [JSON-utdata](/help/assets/content-fragments/content-fragments-json-preview.md).

## Lära sig använda GraphQL med AEM - exempelinnehåll och frågor {#learn-graphql-with-aem-sample-content-queries}

Se [Lära dig använda GraphQL med AEM - exempelinnehåll och frågor](/help/sites-developing/headless/graphql-api/content-fragments-graphql-samples.md) för en introduktion till att använda AEM GraphQL API.

## Självstudiekurs - Komma igång med AEM Headless och GraphQL

Söker du en praktisk självstudiekurs? Ta en titt på [Komma igång med AEM Headless och GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html) - en komplett självstudiekurs som visar hur du bygger upp och exponerar innehåll med AEM GraphQL API:er och som används av en extern app i ett headless CMS-scenario.
