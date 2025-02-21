---
title: Konfigurera komponenter i designläge
description: När AEM-instansen är installerad i en körklar version är ett urval av komponenter omedelbart tillgängliga i sidosparken. Förutom dessa finns även andra komponenter tillgängliga. Du kan använda designläget för att aktivera/inaktivera sådana komponenter.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Konfigurera komponenter i designläge{#configuring-components-in-design-mode}

När AEM-instansen är installerad i en körklar version är ett urval av komponenter omedelbart tillgängliga i sidosparken.

Förutom dessa finns även andra komponenter tillgängliga. Du kan använda designläget för att [aktivera/inaktivera sådana komponenter](#enabledisablecomponentsusingdesignmode). När det är aktiverat och finns på sidan kan du sedan använda designläget för att [konfigurera komponentens design](#configuringcomponentsusingdesignmode) genom att redigera attributparametrarna.

>[!NOTE]
>
>Försiktighet måste iakttas vid redigering av dessa komponenter. Designinställningarna är ofta en viktig del av designen för hela webbplatsen, så de bör bara ändras av någon med rätt behörighet (och upplevelse), ofta administratör eller utvecklare. Mer information finns i [Utveckla komponenter](/help/sites-developing/components.md).

Detta innebär att lägga till, eller ta bort, de komponenter som är tillåtna i sidans styckesystem. Styckesystemet ( `parsys`) är en sammansatt komponent som innehåller alla andra styckekomponenter. Med styckesystemet kan författare lägga till komponenter av olika typer på en sida eftersom det innehåller alla andra styckekomponenter. Varje stycketyp representeras som en komponent.

Innehållet på en produktsida kan till exempel innehålla ett styckesystem som innehåller följande:

* En bild av produkten (i form av en bild eller ett textimagestycke)
* Produktbeskrivningen (som ett textstycke)
* En tabell med tekniska uppgifter (som ett tabellstycke)
* En formuläranvändare fyller i (när ett formulär börjar, formulärelement och slutstycke för formulär)

>[!NOTE]
>
>Mer information om `parsys` finns i [Utveckla komponenter](/help/sites-developing/components.md#paragraphsystem) och [Riktlinjer för användning av mallar och komponenter](/help/sites-developing/dev-guidelines-bestpractices.md#guidelines-for-using-templates-and-components).

## Aktivera/inaktivera komponenter {#enable-disable-components}

I designläget minimeras sidosparken och du kan konfigurera komponenter som är tillgängliga för redigering:

1. Öppna en sida för redigering och använd ikonen Sidekick för att öppna designläget:

   ![Designläge](do-not-localize/chlimage_1.png)

1. Klicka på **Redigera** i styckesystemet (**Design av stycke**).

   ![screen_shot_2012-02-08at102726am](assets/screen_shot_2012-02-08at102726am.png)

1. En dialogruta öppnas där komponentgrupperna som visas i Sidekick listas tillsammans med de enskilda komponenterna som de innehåller.

   Välj vad som krävs för att lägga till, eller ta bort, de komponenter som ska vara tillgängliga i sidosparken.

   ![screen_shot_2012-02-08at103407am](assets/screen_shot_2012-02-08at103407am.png)

1. Sidekick minimerar i designläge. Genom att klicka på pilen kan du maximera Sidekick och återgå till redigeringsläget:

   ![Sidekick minimerat](do-not-localize/sidekick-collapsed.png)

## Konfigurera designen för en komponent {#configuring-the-design-of-a-component}

I designläget kan du även konfigurera attribut för de enskilda komponenterna. Varje komponent har sina egna parametrar visas i följande exempel komponenten **Image**:

1. Öppna en sida för redigering och använd ikonen Sidekick för att öppna designläget:

   ![Designläge - Sidekick](do-not-localize/chlimage_1-1.png)

1. Du kan konfigurera komponenternas design.

   Om du till exempel klickar på **Redigera** i bildkomponenten (**Design of image**) kan du konfigurera komponentspecifika parametrar:

   ![chlimage_1-5](assets/chlimage_1-5.png)

1. Klicka på **OK** om du vill spara ändringarna.

1. Sidekick minimerar i designläge. Genom att klicka på pilen kan du maximera Sidekick och återgå till redigeringsläget:

   ![Sidekick minimerat](do-not-localize/sidekick-collapsed-1.png)
