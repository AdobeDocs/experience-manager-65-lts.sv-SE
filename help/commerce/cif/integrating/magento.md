---
title: AEM och Adobe Commerce Integration med Commerce integration framework
description: AEM och Adobe Commerce är helintegrerade med Commerce integration framework (CIF). Med CIF kan AEM få åtkomst till en Adobe Commerce-instans och kommunicera med Adobe Commerce via GraphQL. Man kan också använda produktväljare och kategoriväljare samt produktkonsolen för att bläddra bland de produkt- och kategoridata som hämtas on demand från Adobe Commerce. Dessutom har CIF en färdig butik som kan snabba upp affärsprojekt.
thumbnail: aem-magento-architecture.jpg
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Integrering med AEM och Adobe Commerce (Magento) med Commerce integration framework {#aem-commerce-framework}

Experience Manager och Adobe Commerce är helintegrerade med Commerce integration framework (CIF). Med CIF kan AEM komma åt och kommunicera direkt med handelsinstansen med Adobe Commerce [GraphQL API:er](https://devdocs.magento.com/guides/v2.4/graphql/).

>[!NOTE]
>
>Den lägsta GraphQL API-version som stöds är 2.3.5. Vissa funktioner stöds endast i nyare versioner eller bara i Adobe Commerce.

## Arkitektur - översikt {#overview}

Den övergripande arkitekturen är följande:

![Översikt över CIF-arkitektur](../assets/AEM_Magento_Architecture.png)

I CIF finns stöd för kommunikationsmönster på serversidan och klientsidan.
API-anrop på serversidan implementeras med hjälp av den generiska [GraphQL-klienten](https://github.com/adobe/commerce-cif-graphql-client) i kombination med en [uppsättning genererade datamodeller](https://github.com/adobe/commerce-cif-magento-graphql) för Commerce GraphQL-schemat. Dessutom kan valfri GraphQL-fråga eller mutation i GQL-format användas.

För klientkomponenterna, som byggs med [React](https://reactjs.org/), används [Apollo-klienten](https://www.apollographql.com/docs/react/).

## AEM CIF Core Component Architecture {#cif-core-components}

![AEM CIF Core Component Architecture](../assets/cif-component-architecture.jpg)

[AEM CIF Core Components](https://github.com/adobe/aem-core-cif-components) följer designmönster och vedertagna standarder som [AEM WCM Core Components](https://github.com/adobe/aem-core-wcm-components).

Affärslogik och backend-kommunikation med Adobe Commerce för AEM CIF Core Components implementeras i Sling Models. Om det är nödvändigt att anpassa den här logiken för att uppfylla projektspecifika krav kan delegeringsmönstret för segmenteringsmodeller användas.

>[!TIP]
>
>Sidan [Anpassa AEM CIF Core Components](../customizing/customize-cif-components.md) innehåller ett detaljerat exempel och bästa praxis för hur du anpassar CIF Core Components.

I projekt kan AEM CIF Core Components och anpassade projektkomponenter enkelt hämta den konfigurerade klienten för en Adobe Commerce-butik som är kopplad till en AEM-sida via Sling Context-Aware-konfiguration.
