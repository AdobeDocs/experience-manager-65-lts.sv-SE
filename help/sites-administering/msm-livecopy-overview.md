---
title: Översiktskonsol för Live Copy
description: Lär dig grunderna i Live Copy Overview Console.
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: site-features
content-type: reference
feature: Multi Site Manager
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 32184455f3c74605a11c20152cf9c8879101a5c2
workflow-type: tm+mt
source-wordcount: '529'
ht-degree: 0%

---

# Översiktskonsol för Live Copy{#live-copy-overview-console}

Med **Live Copy-översikt** kan du:

* Visa/hantera arv på en webbplats:

   * Visa det blå trädet och motsvarande Live-kopia-struktur, tillsammans med deras arvsstatus
   * Ändra arvsstatus, till exempel göra uppehåll, återuppta
   * Visa egenskaper för utkast och live-kopia

* Utför utrullningsåtgärder

## Öppna Live Copy-översikt {#opening-the-live-copy-overview}

Du kan öppna Live-kopieringsöversikt från:

* [Panelen Referenser till en översiktssida (Sites console)](#opening-live-copy-overview-references-for-a-blueprint-page)
* [Egenskaper för en ritningssida](#opening-live-copy-overview-properties-of-a-blueprint-page)

### Öppna Live Copy-översikt - referenser för en designsida {#opening-live-copy-overview-references-for-a-blueprint-page}

Översikt över **Live-kopian** kan öppnas från sidopanelen **Referenser** i **Sites**-konsolen:

1. I konsolen **Platser** [navigerar du till din ritningssida och markerar den](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Öppna panelen **[Referenser](/help/sites-authoring/basic-handling.md#references)** och välj **Live-kopior**.

   ![Referenspanelen - Live-kopior](assets/chlimage_1-359.png)

   >[!NOTE]
   >
   >Du kan också öppna Referenser först och sedan välja en plan.

1. Välj **Översikt över Live-kopia** om du vill visa och använda översikten över alla live-kopior som hör till den valda planen.
1. Använd **Close** för att avsluta och återgå till konsolen **Sites**.

### Öppna Live Copy-översikt - egenskaper för en designsida {#opening-live-copy-overview-properties-of-a-blueprint-page}

Översikt över **Live-kopian** kan öppnas när du visar egenskaper för en ritningssida:

1. Öppna **Egenskaper** för lämplig ritningssida.
1. Öppna fliken **Utskrift** - alternativet **Översikt över Live-kopia** visas i det övre verktygsfältet:

   ![Fliken Utskrift - Live-kopieringsöversikt](assets/chlimage_1-360.png)

1. Välj **Översikt över Live-kopia** om du vill visa och använda översikten över alla live-kopior som hör till den aktuella planen.

1. Använd **Close** för att avsluta och återgå till konsolen **Sites**.

## Använda Live Copy-översikt {#using-the-live-copy-overview}

Översikt över **Live-kopian** kan även användas för att utföra åtgärder på den aktiva kopian:

1. Öppna **Live Copy-översikten**.
1. Välj önskad skiss- eller Live copy-sida - verktygsfältet uppdateras för att visa tillgängliga åtgärder. Vilka [åtgärder](/help/sites-administering/msm.md#terms-used) som är tillgängliga beror på om du väljer en [översiktssida](#actions-for-a-blueprint-page) eller [live-kopia](#actions-for-a-live-copy-page):

### Åtgärder för en designsida {#actions-for-a-blueprint-page}

När du väljer en ritningssida är följande åtgärder tillgängliga:

![Utskrift vald - tillgängliga åtgärder](assets/chlimage_1-361.png)

* Redigera

   * Öppna ritningssidan för redigering.

* [Utrullning](/help/sites-administering/msm.md#rollout-and-synchronize)

   * Utför en utrullning för att föra över ändringar från källan till livecopy.

### Åtgärder för en Live Copy-sida {#actions-for-a-live-copy-page}

När du väljer en live-kopieringssida är följande åtgärder tillgängliga:

![Live copy-sida vald - åtgärder tillgängliga](assets/chlimage_1-362.png)

* Redigera

   * Öppna kopieringssidan för redigering.

* [Relationsstatus](#relationship-status)

   * Visa information om status och arv.

* [Synkronisera](/help/sites-administering/msm.md#rollout-and-synchronize)

   * Synkronisera en live-kopia för att dra ändringar från källan till livecopy.

* [Återställ](/help/sites-administering/msm-livecopy.md#resetting-a-live-copy-page)

   * Återställ en live-kopieringssida om du vill ta bort alla arvsannulleringar och återställa sidan till samma läge som källsidan.

* [Gör uppehåll](/help/sites-administering/msm.md#suspending-and-cancelling-inheritance-and-synchronization)

   * Inaktiverar tillfälligt relationen mellan en live-kopia och dess planeringssida.

* [Återuppta](/help/sites-administering/msm-livecopy.md#resuming-inheritance-for-a-page)

   * Med Fortsätt kan du återskapa en pausad relation.

* [Koppla loss](/help/sites-administering/msm.md#detaching-a-live-copy)

   * Tar permanent bort den aktiva relationen mellan en live-kopia och dess designsida.

## Relationsstatus {#relationship-status}

Konsolen **Relationsstatus** har två flikar med en rad funktioner:

* [Statusinformation för relation](#relationship-status-information)
* [Live Copy-information](#live-copy-information)

### Statusinformation för relation {#relationship-status-information}

På den här fliken finns detaljerad information om statusen för relationen mellan ritningen och den aktiva kopian:

![Statusinformation för relation](assets/chlimage_1-363.png)

### Live Copy-information {#live-copy-information}

På den här fliken kan du visa och redigera konfigurationen av live-kopian:

![Live copy-information](assets/chlimage_1-364.png)
