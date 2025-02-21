---
title: Redigeringsövningar
description: När en startsida har skapats för en sida (eller en uppsättning sidor) kan du redigera innehållet i startkopian av sidorna.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
legacypath: /content/docs/en/aem/6-0/author/site-page-features/launches
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# Redigeringsövningar{#editing-launches}

## Redigera startsidor {#editing-launch-pages}

När en startsida har skapats för en sida (eller en uppsättning sidor) kan du redigera innehållet i startkopian av sidorna.

1. Öppna sidan för redigering.
1. I Sidekick väljer du fliken **Versionshantering** och expanderar sedan gruppen **Startar** . Titeln på den programstart som redigeras använder ett fetstilt teckensnitt.

   ![chlimage_1-13](assets/chlimage_1-13.jpeg)

1. Välj den start som du vill arbeta med och klicka sedan på **Byt**.
1. Börja redigera.

   >[!NOTE]
   >
   >Du kan använda fliken **Sida** i sidosparken för att utföra åtgärder som **Skapa underordnad sida**, bland annat.

## Redigera en startkonfiguration {#editing-a-launch-configuration}

När du har skapat en programstart kan du ändra startnamnet och startdatumet. Du kan också ange en bild som ska associeras med starten.

1. Öppna administrationssidan för starter ([http://localhost:4502/libs/launches/content/admin.html](http://localhost:4502/libs/launches/content/admin.html)).

1. Välj önskad start och klicka på **Redigera** för att öppna dialogrutan:

   * På fliken **Allmänt** kan du redigera:

      * **Titel**
      * **Live Date**: detta motsvarar startdatumet
      * **Produktionsklar**

     Se [Startar - Ordning för händelser](/help/sites-authoring/launches.md#launches-the-order-of-events) för information om syftet med och interaktionen med dessa fält.

   * På fliken **Bild** kan du överföra en bildfil.

1. Klicka på **Spara**.

## Identifiera startstatus för en sida {#discovering-the-launch-status-of-a-page}

När du redigerar en start av en sida visas information om starten längst ned på fliken **Versioning** i Sidekick:

* Startnamnet.
* Tiden sedan den senaste ändringen.
* Den användare som utförde den senaste ändringen.
* Status för flaggan **Produktionsklar** (orange=inte inställd; green=set).

![chlimage_1-186](assets/chlimage_1-186.png)
