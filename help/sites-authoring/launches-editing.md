---
title: Redigera startprogram
description: När du har skapat en startsida för sidan (eller en uppsättning sidor) kan du redigera innehållet i startkopian av sidorna.
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
docset: aem65
legacypath: /content/docs/en/aem/6-0/author/site-page-features/launches
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Launches
role: User,Admin,Architect,Developer
exl-id: 7b032487-a084-4403-a0d3-e5de62748769
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 17%

---

# Redigeringsövningar{#editing-launches}

## Redigera startsidor {#editing-launch-pages}

När en startsida har skapats för en sida (eller en uppsättning sidor) kan du redigera innehållet i startkopian av sidorna).

1. Gå till [Starta från referenser (Sites-konsolen)](/help/sites-authoring/launches.md#launches-in-references-sites-console) för att visa tillgängliga åtgärder.
1. Välj **Gå till sidan** om du vill öppna sidan för redigering.

>[!NOTE]
>
>Du får inte flytta en sida inom en start. Om du försöker utföra den här åtgärden utlöses ett varningsmeddelande:
>
>* Varning! Den här sidan är källan till en start. Det är inte tillåtet att flytta sidan.

### Redigera startsidor som är beroende av en Live-kopia {#editing-launch-pages-subject-to-a-live-copy}

Om din start baseras på en [live-kopia](/help/sites-administering/msm.md) gör du följande:

* se låssymboler (små hänglås) när du redigerar en komponent (innehåll och/eller egenskaper).
* se fliken **Live-kopia** i **Sidegenskaper**

En live-kopia används för att synkronisera innehåll *från* källgrenen *till* startgrenen (för att hålla startsidan uppdaterad med ändringarna i källan).

Du kan göra ändringar på samma sätt som du kan redigera en vanlig Live-kopia, till exempel:

* Om du klickar på ett stängt hänglås bryts synkroniseringen och du kan göra nya uppdateringar av innehållet när du startar programmet. När du har låst upp (öppet hänglås) skrivs inte ändringarna över av ändringar som gjorts på samma plats i källgrenen.
* **Gör uppehåll i** (och **återuppta**) arv för en viss sida.

Mer information finns i [Ändra innehåll för Live-kopia](/help/sites-administering/msm-livecopy.md#changing-live-copy-content).

## Jämföra en startsida med dess Source-sida {#comparing-a-launch-page-to-its-source-page}

Om du vill spåra de ändringar du har gjort kan du visa startsidan i **Referenser** och jämföra startsidan med dess källsida:

1. I konsolen **Platser** navigerar [till startsidan och markerar den](/help/sites-authoring/basic-handling.md#viewingandselectingyourresources).
1. Öppna panelen **[Referenser](/help/sites-authoring/basic-handling.md#references)** och välj **Startar**.
1. Välj en specifik start och **Jämför med Source**:

   ![screen-shot_2019-03-05at121952](assets/screen-shot_2019-03-05at121952.png)

1. De två sidorna (start och källa) öppnas sida vid sida.

   Mer information om hur du använder den här funktionen finns i [Sidskillnad](/help/sites-authoring/page-diff.md).

## Ändra använda Source-sidor {#changing-the-source-pages-used}

Du kan när som helst lägga till eller ta bort sidor till/från intervallet med källsidor för en start:

1. Öppna och välj programstart från:

   * [Startar konsolen](/help/sites-authoring/launches.md#the-launches-console):

      * Välj **Redigera**.

   * [Referenser (platskonsolen)](/help/sites-authoring/launches.md#launches-in-references-sites-console) för att visa tillgängliga åtgärder:

      * Välj **Redigera start**.

   Källsidorna visas.

1. Gör önskade ändringar och bekräfta sedan med **Spara**.

   >[!NOTE]
   >
   >Om du vill lägga till sidor i en programstart måste de ligga under en gemensam språkrot, det vill säga inom en enda plats.

## Redigera en startkonfiguration {#editing-a-launch-configuration}

Du kan när som helst redigera egenskaperna för en start:

1. Öppna och välj programstart från:

   * [Startar konsolen](/help/sites-authoring/launches.md#the-launches-console):

      * Välj **Egenskaper**.

   * [Referenser (platskonsolen)](/help/sites-authoring/launches.md#launches-in-references-sites-console) för att visa tillgängliga åtgärder:

      * Välj **Redigera egenskaper**.

   Detaljerna visas.

1. Gör önskade ändringar och bekräfta sedan med **Spara**.

   Avsnittet [Startsidor och händelseordning](/help/sites-authoring/launches.md#launches-the-order-of-events) innehåller information om syftet med och interaktionen mellan fälten **Startdatum** och **Produktionsklar**.

## Identifiera startstatus för en sida {#discovering-the-launch-status-of-a-page}

Statusen visas när du väljer en specifik start på fliken Inställningar (se [Startar i Referenser (platskonsolen)](/help/sites-authoring/launches.md#launches-in-references-sites-console)).

![screen-shot_2019-03-05at121901](assets/screen-shot_2019-03-05at121901.png)
