---
title: Uppdatera dina innehållsfragment för optimerad GraphQL-filtrering
description: Lär dig hur du uppdaterar innehållsfragment för optimerad GraphQL-filtrering i Adobe Experience Manager för leverans av headless-innehåll.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin,Architect,Data Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---

# Uppdatera dina innehållsfragment för optimerad GraphQL-filtrering {#updating-content-fragments-for-optimized-graphql-filtering}

Om du vill optimera prestanda för dina GraphQL-filter kör du en procedur för att uppdatera dina innehållsfragment.

>[!NOTE]
>
>När du har uppdaterat dina innehållsfragment kan du följa rekommendationerna för [Optimera GraphQL-frågor](/help/sites-developing/headless/graphql-api/graphql-optimization.md).

## Förutsättningar {#prerequisites}

Kontrollera att du har minst version 6.5.17.0 av AEM.

## Uppdatera dina innehållsfragment {#updating-content-fragments}

Så här kör du proceduren:

1. [Konfigurera OSGi-inställningarna](/help/sites-deploying/configuring-osgi.md) för **jobbkonfigurationen för migrering av innehållsfragment**:

   ![Migreringsjobbskonfiguration för OSGi-innehållsfragment](assets/cfm-graphql-update-01.png "Migreringsjobbskonfiguration för OSGi-innehållsfragment")

1. I dialogrutan ställer du in följande två parametrar:

   * **ContentFragmentMigration:Enabled** : `1`
   * **ContentFragmentMigration:Enforce** : `1`

1. **Spara** specifikationerna - uppdateringsproceduren startar.

1. Vänta tills proceduren har slutförts. Proceduren är slutförd när egenskapen `cfGlobalVersion` visas på `/content/dam` och är inställd på `1`.

1. Återgå till OSGi-konfigurationen för att inaktivera proceduren.

   I dialogrutan för **konfigurationen för migreringsjobbet för innehållsfragment** anger du följande två parametrar:

   * **ContentFragmentMigration:Enabled** : `0`
   * **ContentFragmentMigration:Enforce** : `0`

## Begränsningar {#limitations}

Tänk på följande begränsningar:

* Optimering av prestanda för GraphQL-filter är endast möjligt efter en fullständig uppdatering av alla dina innehållsfragment (vilket anges av närvaron av egenskapen `cfGlobalVersion` för JCR-noden `/content/dam`)

* Om innehållsfragment importeras från ett innehållspaket (med `crx/de`) efter att uppdateringsproceduren har körts, kommer dessa innehållsfragment inte att beaktas i GraphQL-frågeresultat förrän uppdateringsproceduren körs igen.
