---
title: Söka efter sidanalysdata för att mäta hur effektivt sidinnehållet är
description: Använd data från sidanalys för att mäta hur effektivt deras sidinnehåll är
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: b47591ac-10ff-4b87-b75d-68ed635ceca8
source-git-commit: 87f0e0530b14c8cb7d25ee139ec89d75551fb16e
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Visa sidanalysdata{#seeing-page-analytics-data}

Använd data från sidanalys för att mäta hur effektivt sidinnehållet är.

## Analyser som visas från konsolen {#analytics-visible-from-the-console}

![spad-01](assets/spad-01.png)

Sidanalysdata visas i [listvyn](/help/sites-authoring/basic-handling.md#list-view) i webbplatskonsolen. När sidorna visas i listformat är följande kolumner tillgängliga som standard:

* Sidvyer
* Unika besökare
* Tid på sidan

Varje kolumn visar ett värde för den aktuella rapporteringsperioden och anger också om värdet har ökat eller minskat sedan den föregående rapporteringsperioden. De data du ser uppdateras var tolfte timme.

>[!NOTE]
>
>[Konfigurera importintervallet](/help/sites-administering/adobeanalytics-connect.md#configuring-the-import-interval) om du vill ändra uppdateringsperioden.

1. Öppna konsolen **Platser**, till exempel [https://localhost:4502/sites.html/content](https://localhost:4502/sites.html/content)
1. Klicka på ikonen längst till höger i verktygsfältet (det övre högra hörnet) för att välja **Listvy** (ikonen som visas beror på den [aktuella vyn](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources)).

1. Klicka på ikonen längst till höger i verktygsfältet (övre högra hörnet) och välj sedan **Visa inställningar**. Dialogrutan **Konfigurera kolumner** öppnas. Gör nödvändiga ändringar och bekräfta med **Uppdatera**.

   ![spad-02](assets/spad-02.png)

### Välja rapporteringsperiod {#selecting-the-reporting-period}

Välj den rapporteringsperiod för vilken analysdata visas på webbplatskonsolen:

* Data för de senaste 30 dagarna
* Data för de senaste 90 dagarna
* Årets data

Den aktuella rapportperioden visas i verktygsfältet i webbplatskonsolen (till höger i det övre verktygsfältet). Använd listrutan för att välja önskad rapporteringsperiod.

![aa-05](assets/aa-05.png)

### Konfigurera tillgängliga datakolumner {#configuring-available-data-columns}

Medlemmar i användargruppen analytics-administrators kan konfigurera konsolen Sites så att författare kan se extra analyskolumner.

>[!NOTE]
>
>När ett sidträd innehåller underordnade sidor som är kopplade till olika Adobe Analytics molnkonfigurationer, kan du inte konfigurera tillgängliga datakolumner för sidorna.

1. I listvyn använder du vyväljarna (höger om verktygsfältet), väljer **Visa inställningar** och sedan **Lägg till anpassade analysdata**.

   ![spad-03](assets/spad-03.png)

1. Markera de mätvärden som du vill visa för författare i webbplatskonsolen och klicka sedan på **Lägg till**.

   De kolumner som visas hämtas från Adobe Analytics.

   ![aa-16](assets/aa-16.png)

### Öppna innehållsinsikter från webbplatser {#opening-content-insights-from-sites}

Öppna [Content Insight](/help/sites-authoring/content-insights.md) från Sites-konsolen för att ytterligare undersöka hur effektiva sidorna är.

1. I webbplatskonsolen väljer du den sida som du vill se innehållsinsikter för.
1. Klicka på ikonen Analytics (Analyser och rekommendationer) i verktygsfältet.

   ![Ikon för analyser och rekommendationer](do-not-localize/chlimage_1-14.png)

## Analyser visas från sidredigeraren (Activity Map) {#analytics-visible-from-the-page-editor-activity-map}

>[!CAUTION]
>
>På grund av säkerhetsändringar i Adobe Analytics API är det inte längre möjligt att använda den version av Activity Map som ingår i AEM.
>
>Plugin-programmet [ActivityMap som tillhandahålls av Adobe Analytics](https://experienceleague.adobe.com/docs/analytics/analyze/activity-map/getting-started/get-started-users/activitymap-install.html) bör nu användas.
