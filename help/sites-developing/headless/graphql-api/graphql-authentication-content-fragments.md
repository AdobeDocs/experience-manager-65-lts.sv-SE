---
title: Autentisering av Adobe Experience Manager GraphQL-fjärrfrågor för innehållsfragment
description: Förstå den autentisering som krävs för fjärrfrågor till Adobe Experience Manager GraphQL för att säkra din headless-leverans av innehåll.
feature: Content Fragments,GraphQL API
solution: Experience Manager, Experience Manager Sites
role: Developer
exl-id: 8891b13d-5d7d-4ac5-99ba-bde8bff58a6d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '110'
ht-degree: 0%

---

# Autentisering av Adobe Experience Manager GraphQL-fjärrfrågor för innehållsfragment {#authentication-for-remote-aem-graphql-queries-on-content-fragments}

Ett primärt användningsfall för [Adobe Experience Manager (AEM) GraphQL API for Content Fragment Delivery](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md) är att ta emot fjärrfrågor från tredjepartsprogram eller -tjänster. Dessa fjärrfrågor kan kräva autentiserad API-åtkomst för säker leverans av headless-innehåll.

>[!NOTE]
>
>För testning och utveckling kan du även komma åt AEM GraphQL API direkt via [GraphiQL-gränssnittet](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md#graphiql-interface).

För autentisering måste tredjepartstjänsten autentiseras med användarnamnet och lösenordet för AEM-kontot.

<!-- 6.5.10.0 - does this content/page need to be migrated? -->

<!--
For authentication the third-party service needs to [retrieve an Access Token](#retrieving-access-token), that can then be [used in the GraphQL Request](#use-access-token-in-graphql-request).

## Retrieving an Access Token {#retrieving-access-token}

See [Generating Access Tokens for Server Side APIs](/help/sites-developing/generating-access-tokens-for-server-side-apis.md) for full details.

## Using the Access Token in a GraphQL Request {#use-access-token-in-graphql-request}

For a third-party service to connect with an AEM instance it needs to have an *Access Token*. The service must then add this token to the `Authorization` header on the POST request. 

For example, a GraphQL Authorization Header:

```xml
Authorization: Bearer <access_token>
```

## Permission Requirements {#permission-requirements}

All requests made using the access token will actually be made *by the user account that generated the token*. 

This means that you need to check that the account has the permissions required to run GraphQL queries. 

You can check this by using GraphiQL on the local instance.
-->
