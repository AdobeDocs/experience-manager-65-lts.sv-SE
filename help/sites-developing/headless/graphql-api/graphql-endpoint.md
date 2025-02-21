---
title: Hantera GraphQL-slutpunkter i AEM
description: Lär dig hur du hanterar GraphQL slutpunkter i Adobe Experience Manager för leverans av headless-material.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin,Architect,Data Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# Hantera GraphQL-slutpunkter i AEM {#graphql-aem-endpoint}

Slutpunkten är den sökväg som används för att komma åt GraphQL för AEM. Med den här sökvägen kan du (eller din app):

* tillgång till GraphQL schema,
* skicka dina GraphQL-frågor
* ta emot svaren (på dina GraphQL-frågor).

Det finns två typer av slutpunkter i AEM:

* Global
   * Tillgängligt för alla webbplatser.
   * Den här slutpunkten kan använda alla modeller för innehållsfragment från alla platskonfigurationer (definieras i [Konfigurationsläsaren](/help/assets/content-fragments/content-fragments-configuration-browser.md#enable-content-fragment-functionality-in-configuration-browser)).
   * Om det finns några modeller för innehållsfragment som ska delas mellan platskonfigurationer, ska dessa skapas under de globala platskonfigurationerna.
* Platskonfigurationer:
   * Motsvarar en platskonfiguration, enligt definitionen i [Konfigurationsläsaren](/help/assets/content-fragments/content-fragments-configuration-browser.md#enable-content-fragment-functionality-in-configuration-browser).
   * Specifikt för en angiven plats/ett angivet projekt.
   * En platskonfigurationsspecifik slutpunkt använder innehållsfragmentmodellerna från den specifika platskonfigurationen tillsammans med de från den globala platskonfigurationen.

>[!CAUTION]
>
>Innehållsfragmentsredigeraren kan tillåta att ett innehållsfragment av en platskonfiguration refererar till ett innehållsfragment av en annan platskonfiguration (via principer).
>
>I så fall går det inte att hämta allt innehåll med en platskonfigurationsspecifik slutpunkt.
>
>Innehållsförfattaren bör styra det här scenariot. Det kan till exempel vara bra att överväga att placera delade modeller för innehållsfragment under konfigurationen för globala webbplatser.

Databassökvägen för den globala slutpunkten för GraphQL för AEM är:

`/content/cq:graphql/global/endpoint`

För vilken ditt program kan använda följande sökväg i URL:en för begäran:

`/content/_cq_graphql/global/endpoint.json`

Om du vill aktivera en slutpunkt för GraphQL för AEM måste du:

* [Aktivera din GraphQL-slutpunkt](#enabling-graphql-endpoint)
* [Publicera din GraphQL-slutpunkt](#publishing-graphql-endpoint)

## Aktivera din GraphQL-slutpunkt {#enabling-graphql-endpoint}

Om du vill aktivera en GraphQL-slutpunkt måste du först ha en lämplig konfiguration. Se [Innehållsfragment - Konfigurationsläsaren](/help/assets/content-fragments/content-fragments-configuration-browser.md).

>[!CAUTION]
>
>Om [användningen av innehållsfragmentmodeller inte har aktiverats](/help/assets/content-fragments/content-fragments-configuration-browser.md) är alternativet **Skapa** inte tillgängligt.

Så här aktiverar du motsvarande slutpunkt:

1. Gå till **Verktyg**, **Assets** och välj sedan **GraphQL**.
1. Välj **Skapa**.
1. Dialogrutan **Skapa ny GraphQL-slutpunkt** öppnas. Här kan du ange:
   * **Namn**: slutpunktens namn. Du kan ange valfri text.
   * **Använd GraphQL-schema från**: använd listrutan för att välja önskad plats/önskat projekt.

   >[!NOTE]
   >
   >Följande varning visas i dialogrutan:
   >
   >* *GraphQL-slutpunkter kan medföra problem med datasäkerhet och prestanda om de inte hanteras med omsorg. Kontrollera att du har angett rätt behörigheter när du har skapat en slutpunkt.*

1. Bekräfta med **Skapa**.
1. Dialogrutan **Nästa steg** kommer att innehålla en direktlänk till säkerhetskonsolen så att du kan kontrollera att den nyskapade slutpunkten har rätt behörigheter.

   >[!CAUTION]
   >
   >Slutpunkten är tillgänglig för alla. Detta kan - särskilt när det gäller publiceringsinstanser - utgöra ett säkerhetsproblem, eftersom GraphQL-frågor kan belasta servern mycket.
   >
   >Du kan ställa in åtkomstkontrollistor, som passar ditt användningsfall, på slutpunkten.

## Publicera din GraphQL-slutpunkt {#publishing-graphql-endpoint}

Markera den nya slutpunkten och **Publicera** för att göra den helt tillgänglig i alla miljöer.

>[!CAUTION]
>
>Slutpunkten är tillgänglig för alla.
>
>På publiceringsinstanser kan detta utgöra ett säkerhetsproblem, eftersom GraphQL-frågor kan medföra en stor belastning på servern.
>
>Ställ in åtkomstkontrollistor som passar ditt användningsfall på slutpunkten.
