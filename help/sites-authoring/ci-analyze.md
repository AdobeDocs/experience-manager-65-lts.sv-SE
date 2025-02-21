---
title: Analyserar sidprestanda
description: Använd sidan Content Insight för att analysera prestandan för sidan som du redigerar
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Integration
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 1%

---

# Analyserar sidprestanda{#analyzing-page-performance}

Öppna sidan [Content Insight](/help/sites-authoring/content-insights.md) om du vill analysera prestandan för sidan som du redigerar. Konfigurera rapporteringsperioden så att analysen fokuseras.

## Öppna analyser och rekommendationer för en sida {#opening-analytics-and-recommendations-for-a-page}

Använd följande procedur för att se Analytics and Recommendations för en sida:

1. Navigera till sidan som du vill analysera.
1. Klicka på **Analyser och rekommendationer** i verktygsfältet.

   >[!NOTE]
   >
   >Analyser och rekommendationer för en sida visas bara om du har konfigurerat AEM att [integrera med Adobe Analytics](/help/sites-administering/adobeanalytics-connect.md).

   ![screen-shot_2019-03-05at115319](assets/screen-shot_2019-03-05at115319.png)

### Ändra rapporteringsperioden {#changing-the-reporting-period}

Ändra följande tidsrelaterade aspekter av analysrapporterna:

* Den tidsperiod som ska rapporteras.
* Datas granularitet.

Verktygen för att ändra de tidsrelaterade aspekterna av rapporterna visas högst upp på sidan Innehållsinsikter. ![chlimage_1-126](assets/chlimage_1-126.png)

#### Ändra rapporteringsperioden {#changing-the-reporting-period-1}

Ändra rapporteringsperioden för Content Insight-sidan så att analysen av sidaktiviteten fokuseras till en viss tidsperiod. När du ändrar rapporteringsperioden uppdateras rapporterna automatiskt. Det skuggade området i tidsramen representerar rapporteringsperioden. Datumen i tidsramen ökar från vänster till höger.

![chlimage_1-127](assets/chlimage_1-127.png)

Så här ändrar du rapporteringsperioden för en Content Insight-sida:

1. Om tidsramen inte visas överst på sidan klickar du på ikonen Växla tidsram.

   ![Växla tidsram](do-not-localize/chlimage_1-22.png)

1. Om du vill ändra startdatumet för rapportperioden drar du cirkeln som visas till vänster om det skuggade området till önskat startdatum.

   Om du inte kan se den vänstra sidan av det skuggade området använder du rullningslisten för att visa det.

1. Om du vill ändra slutdatumet för rapportperioden drar du cirkeln som visas till höger om det skuggade området till önskat slutdatum.

#### Ändra rapportperiodens kornighet {#changing-the-granularity-of-the-reporting-period}

Ändra den tid som varje datapunkt sträcker sig över i en rapport. Om du t.ex. markerar granulariteten för vecka, representerar varje datapunkt i rapporten Vyer antalet vyer under en vecka.

![screen_shot_2017-11-29at141001](assets/screen_shot_2017-11-29at141001.png)

Granulariteten påverkar rapporter som anger data i förhållande till tid, t.ex. rapporter om Vyer och Genomsnittligt engagerade sidminuter. Kornigheten påverkar också skalan för tidsramen.

1. Om granularitetskontrollen inte visas klickar du på ikonen Växla granularitet.

   ![chlimage_1-128](assets/chlimage_1-128.png)

1. Klicka på önskad granularitet. När du har valt det här alternativet uppdateras rapporten automatiskt så att den avspeglar granulariteten.

### Tilldela uppgifter för SEO-rekommendationer {#assigning-tasks-for-seo-recommendations}

Använd SEO-rekommendationsrapporten för att skapa uppgifter för att förbättra synligheten på sidan för sökmotorer. För varje rekommendation i rapporten som inte har någon bock kan du skapa en uppgift som du tilldelar en användare för att utföra det arbete som krävs.

![chlimage_1-129](assets/chlimage_1-129.png)

Statusen för SEO-rekommendationen anger när uppgiften har skapats men inte slutförts än.

![chlimage_1-130](assets/chlimage_1-130.png)

När uppgiften skapas visas den i användarens uppgiftslista. Mer information om uppgifter finns i [Arbeta med uppgifter](/help/sites-authoring/task-content.md).

Använd följande procedur för att skapa en uppgift för en SEO-rekommendation.

1. Klicka på informationsikonen för SEO-rekommendationen.

   ![Informationsikon](do-not-localize/chlimage_1-23.png)

1. Klicka på den inringade triangeln som visas bredvid informationsikonen.

   ![chlimage_1-131](assets/chlimage_1-131.png)

1. Fyll i formulärfälten som visas och välj Skapa:

   * Projekt: Välj i vilket projekt uppgiften ska skapas.
   * Namn: Namnet som identifierar uppgiften. Standardnamnet är titeln på SEO-rekommendationen.
   * Tilldela till: Välj den användare som ska tilldelas uppgiften. Börja skriva användarens namn för att filtrera listan.
   * Beskrivning: En beskrivning av aktiviteten som krävs för att slutföra uppgiften. Standardbeskrivningen är den information som medföljer SEO-rekommendationen.
   * Aktivitetsprioritet: Uppgiftens prioritet.
   * Förfallodatum: Det datum då uppgiften ska vara slutförd.

   **Obs!** Den uppgift som skapas innehåller även sökvägen till sidan som SEO-rekommendationen gäller för.

1. Klicka på Klar för att stänga meddelandet Aktiviteten har skapats.
