---
title: Skapa startprogram
description: Skapa en startsida för att möjliggöra uppdatering av en ny version av befintliga webbsidor för framtida aktivering. När du skapar en Launch anger du en titel och källsidan.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
legacypath: /content/docs/en/aem/6-0/author/site-page-features/launches
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: 02fd32c8-7def-45d4-ba3b-d4cb346f5103
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 14%

---

# Skapa startprogram{#creating-launches}

Skapa en startsida för att möjliggöra uppdatering av en ny version av befintliga webbsidor för framtida aktivering. När du skapar en Launch anger du en titel och källsidan:

* Titeln visas i **Sidekick** där författare kan komma åt dem och arbeta med dem.
* Källsidans underordnade sidor inkluderas som standard i starten. Du kan bara använda källsidan om du vill.
* Som standard uppdaterar [Live Copy](/help/sites-administering/msm.md) startsidorna automatiskt när källsidorna ändras. Du kan ange att en statisk kopia ska skapas för att förhindra automatiska ändringar.

Du kan också ange **startdatum** (och starttid) för att definiera när startsidorna ska befordras och aktiveras. **Startdatumet** fungerar dock endast i kombination med flaggan **Produktionsklar** (se [Redigera en startkonfiguration](/help/sites-classic-ui-authoring/classic-launches-editing.md#editing-a-launch-configuration)). För att åtgärderna ska köras automatiskt måste båda anges.

## Skapa en Launch {#creating-a-launch}

Följande procedur skapar en start.

1. Öppna sidan Webbplatsadministration ([http://localhost:4502/siteadmin](http://localhost:4502/siteadmin)).
1. Klicka på **Ny..** och sedan på **Ny start..**.
1. I dialogrutan **Skapa start** anger du värden för följande egenskaper:

   * **Starttitel**: Namnet på starten. Namnet ska vara meningsfullt för författare.
   * **Source Page**: Sökvägen till den sida som starten ska skapas för. Som standard inkluderas alla underordnade sidor.
   * **Uteslut undersidor**: Välj det här alternativet om du bara vill skapa startsidan för källsidan och inte för de underordnade sidorna. Som standard är det här alternativet inte markerat.
   * **Synkronisera inte**: Välj det här alternativet om du vill att innehållet på startsidorna ska uppdateras automatiskt när källsidorna ändras. Detta uppnås genom att starta en [live-kopia](/help/sites-administering/msm.md).
   * **Startdatum**: Det datum och den tidpunkt då startkopian ska aktiveras (beroende på flaggan **Produktionsklar**; se [Startar - ordning för händelser](/help/sites-authoring/launches.md#launches-the-order-of-events)).

   ![chlimage_1-99](assets/chlimage_1-99a.png)

1. Klicka på **Skapa**.

## Ta bort en start {#deleting-a-launch}

Du kan även ta bort en programstart.

1. Välj önskad start i [startkonsolen](/help/sites-classic-ui-authoring/classic-launches.md).
1. Klicka på **Ta bort** - bekräftelse krävs:

   ![chlimage_1-100](assets/chlimage_1-100a.png)

   >[!CAUTION]
   >
   >När du tar bort kapslade starter bör du ta bort de lägre nivåerna först.
