---
title: Använda GraphiQL IDE i AEM
description: Lär dig använda GraphiQL IDE i Adobe Experience Manager.
solution: Experience Manager, Experience Manager Sites
feature: Content Fragments,GraphQL API
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 0%

---

# Använda GraphiQL IDE {#graphiql-ide}

En implementering av standardmiljön [GraphiQL](https://graphql.org/learn/serving-over-http/#graphiql) är tillgänglig för användning med GraphQL API i Adobe Experience Manager (AEM).

>[!NOTE]
>
>GraphiQL ingår i alla AEM-miljöer (men är bara tillgängligt/synligt när du konfigurerar slutpunkterna).
>
>I tidigare versioner behövdes ett paket för att installera GraphiQL IDE. Om du har installerat den kan den nu tas bort.

>[!NOTE]
>Du måste ha [konfigurerat dina slutpunkter](/help/sites-developing/headless/graphql-api/graphql-endpoint.md) i [konfigurationsläsaren](/help/assets/content-fragments/content-fragments-configuration-browser.md) innan du kan använda GraphiQL IDE.

Med verktyget **GraphiQL** kan du testa och felsöka dina GraphQL-frågor genom att aktivera:

* välj den **slutpunkt** som passar den platskonfiguration som du vill använda för dina frågor
* direkt mata in nya frågor
* skapa och få åtkomst till **[beständiga frågor](/help/sites-developing/headless/graphql-api/persisted-queries.md)**
* köra dina frågor för att omedelbart se resultatet
* hantera **frågevariabler**
* spara och hantera **beständiga frågor**
* publicera, eller avpublicera, **beständiga frågor** (till exempel till/från `dev-publish`)
* visa **historiken** för dina tidigare frågor
* använd **Dokumentationsutforskaren** för att få tillgång till dokumentationen och för att lära dig mer och förstå vilka metoder som är tillgängliga.

Du kan öppna frågeredigeraren från:

* **Verktyg** > **Allmänt** > **GraphQL Query Editor**
* direkt, till exempel `http://localhost:4502/aem/graphiql.html`

![Gränssnittet GraphiQL](assets/cfm-graphiql-interface.png "Gränssnittet GraphiQL")

Du kan använda GraphiQL på datorn så att frågor kan begäras av klientprogrammet med hjälp av GET-begäranden och för publiceringsfrågor. För produktionsanvändning kan du sedan [flytta dina frågor till din produktionsmiljö](/help/sites-developing/headless/graphql-api/persisted-queries.md#transfer-persisted-query-production). Till att börja med måste produktionsförfattaren validera nyskrivet innehåll med frågorna och till sist publicera produktionen för direktkonsumtion.

## Markera slutpunkten {#selecting-endpoint}

Som ett första steg måste du välja den **[slutpunkt](/help/sites-developing/headless/graphql-api/graphql-endpoint.md)** som du vill använda för frågorna. Slutpunkten passar den platskonfiguration som du vill använda för dina frågor.

Det här är tillgängligt i listrutan högst upp till höger.

## Skapa och behålla en ny fråga {#creating-new-query}

Du kan ange den nya frågan i redigeraren, som finns i den vänstra panelen, direkt under GraphiQL-logotypen.

>[!NOTE]
>
>Om du redan har markerat en beständig fråga och visar den i redigeringspanelen väljer du `+` (bredvid **Beständiga frågor**) för att tömma redigeraren som är klar för den nya frågan.

Börja skriva - redigeraren:

* använder muspekaren för att visa ytterligare information om element
* innehåller funktioner som syntaxmarkering, automatisk komplettering och automatisk föreslå

>[!NOTE]
>
>GraphQL-frågor inleds vanligtvis med ett `{`-tecken.
>
>Rader som börjar med `#` ignoreras.

Använd **Spara som** om du vill behålla den nya frågan.

## Uppdaterar din beständiga fråga {#updating-persisted-query}

Markera frågan som du vill uppdatera i listan på panelen **[Beständiga frågor](/help/sites-developing/headless/graphql-api/persisted-queries.md)** (längst till vänster).

Frågan visas på redigeringspanelen. Gör de ändringar du behöver och använd sedan **Spara** för att genomföra uppdateringarna i den beständiga frågan.

## Köra frågor {#running-queries}

Du kan köra en ny fråga direkt eller läsa in och köra en beständig fråga. Om du vill läsa in en beständig fråga väljer du den i listan. Frågan visas i redigeringspanelen.

I båda fallen är frågan som visas på redigeringspanelen den fråga som kommer att köras när du antingen:

* klicka på ikonen **Kör fråga**
* använd tangentbordskombinationen `Control-Enter`

## Frågevariabler {#query-variables}

<!-- more details needed here? -->

Med GraphiQL IDE kan du också hantera dina [frågevariabler](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md#graphql-variables).

Till exempel:

![GraphQL-variabler](assets/cfm-graphqlapi-03.png "GraphQL-variabler")

<!--
## Managing cache for your persisted queries {#managing-cache}

[Persisted queries](/help/headless/graphql-api/persisted-queries.md) are recommended as they can be cached at the dispatcher and CDN layers, ultimately improving the performance of the requesting client application. By default AEM will invalidate the Content Delivery Network (CDN) cache based on a default Time To Live (TTL).

>[!NOTE]
>
>Custom rewrite rules on the Dispatcher might override defaults from AEM publish. 
>
>In the case that you are sending TTL-based cache-control headers from the dispatcher, based on a location match pattern then, if necessary, you might want to exclude `/graphql/execute.json/*` from the matches.

Using GraphQL you can configure the HTTP Cache Headers  to control these parameters for your individual persisted query.

1. The **Headers** option is accessible via the three vertical dots to the right of the persisted query name (far left panel):

   ![Persisted Query HTTP Cache Headers](assets/cfm-graphqlapi-headers-01.png "Persisted Query HTTP Cache Headers")

1. Selecting this opens the **Cache Configuration** dialog box:

   ![Persisted Query HTTP Cache Header Settings](assets/cfm-graphqlapi-headers-02.png "Persisted Query HTTP Cache Header Settings")

1. Select the appropriate parameter, then adjust the value as required:

   * **cache-control** - **max-age**
     Caches can store this content for specified number of seconds. Typically this is the browser TTL (Time To Live).
   * **surrogate-control** - **s-maxage**
     Same as max-age but applies specifically to proxy caches.
   * **surrogate-control** - **stale-while-revalidate**
     Caches may continue to serve a cached response after it becomes stale, for up to the specified number of seconds.
   * **surrogate-control** - **stale-if-error**
     Caches may continue to serve a cached response if there is an origin error, for up to the specified number of seconds.

1. Select **Save** to persist the changes.
-->

## Publicera beständiga frågor {#publishing-persisted-queries}

När du har valt din [beständiga fråga](/help/sites-developing/headless/graphql-api/persisted-queries.md) i listan (den vänstra panelen) kan du använda åtgärderna **Publicera** och **Avpublicera**. Detta aktiverar dem i din publiceringsmiljö (till exempel `dev-publish`) så att de blir lätta att komma åt när du testar program.

>[!NOTE]
>
>Definitionen av den beständiga frågans cache `Time To Live` {&quot;cache-control&quot;:&quot;parameter&quot;:value} har standardvärdet 2 timmar (7 200 sekunder).

## Kopiera URL för direktåtkomst till frågan {#copy-url}

Med alternativet **Kopiera URL** kan du simulera en fråga genom att kopiera den URL som används för att få direkt åtkomst till den beständiga frågan och se resultatet. Detta kan sedan användas för testning, t.ex. genom att gå till i en webbläsare:

<!--
  >[!NOTE]
  >
  >The URL will need [encoding before using programmatically](/help/headless/graphql-api/persisted-queries.md#encoding-query-url).
  >
  >The target environment might need adjusting, depending on your requirements.
-->

Till exempel:

`http://localhost:4502/graphql/execute.json/global/article-list-01`

Genom att använda den här URL:en i en webbläsare kan du bekräfta resultatet:

![GraphiQL - kopiera URL](assets/cfm-graphiql-copy-url.png "GraphiQL - kopiera URL")

Alternativet **Kopiera URL** är tillgängligt via de tre lodräta prickarna till höger om det beständiga frågenamnet (längst till vänster):

![GraphiQL - kopiera URL](assets/cfm-graphiql-persisted-query-options.png "GraphiQL - kopiera URL")

## Tar bort beständiga frågor {#deleting-persisted-queries}

Alternativet **Ta bort** är även tillgängligt via de tre lodräta prickarna till höger om det beständiga frågenamnet (längst till vänster).

<!-- what happens if you try to delete something that is still published? -->


## Installera den beständiga frågan i produktionen {#installing-persisted-query-production}

När du har utvecklat och testat din beständiga fråga med GraphiQL är det slutliga målet att [överföra den till din produktionsmiljö](/help/sites-developing/headless/graphql-api/persisted-queries.md#transfer-persisted-query-production) för användning i dina program.

## Kortkommandon {#keyboard-shortcuts}

Det finns ett urval av kortkommandon som ger direktåtkomst till åtgärdsikoner i den integrerade utvecklingsmiljön:

* Pretify-fråga: `Shift-Control-P`
* Kopplingsfråga: `Shift-Control-M`
* Kör fråga: `Control-Enter`
* Komplettera automatiskt: `Control-Space`

>[!NOTE]
>
>På vissa tangentbord är tangenten `Control` märkt som `Ctrl`.
