---
title: Komponentkonsol
description: Med komponentkonsolen kan du bläddra igenom alla komponenter som definierats för instansen och visa nyckelinformation för varje komponent.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: e19f9c2b-dc69-4077-a038-d8eb25a1ad6a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 15%

---

# Komponentkonsol{#components-console}

Med komponentkonsolen kan du bläddra igenom alla komponenter som definierats för instansen och visa nyckelinformation för varje komponent.

Den kan nås via **Verktyg >** **Allmänt >** **Komponenter**. I konsolen finns kortvyn och listvyn. Eftersom det inte finns någon trädstruktur för komponenter är kolumnvyn inte tillgänglig.

![screen-shot_2019-03-05at113145](assets/screen-shot_2019-03-05at113145.png)

>[!NOTE]
>
>Komponentkonsolen visar alla komponenter i systemet. [Komponentbläddraren](/help/sites-authoring/author-environment-tools.md#components-browser) visar komponenter som är tillgängliga för författare och döljer komponentgrupper som börjar med en punkt ( `.`).

## Sök {#searching}

Med ikonen **Endast innehåll** (överst till vänster) kan du öppna **sökpanelen** och söka efter och/eller filtrera komponenterna:

![screen-shot_2019-03-05at113251](assets/screen-shot_2019-03-05at113251.png)

### Komponentinformation {#component-details}

Om du vill visa information om en viss komponent klickar du på den önskade resursen. Tre flikar innehåller:

* **Egenskaper**

  ![screen_shot_2018-03-27at165847](assets/screen_shot_2018-03-27at165847.png)

  På fliken Egenskaper kan du:

   * Visa komponentens allmänna egenskaper.
   * Visa hur ikonen eller förkortningen [ har definierats ](/help/sites-developing/components-basics.md#component-icon-in-touch-ui) för komponenten.

      * Om du klickar på ikonens källa kommer du till den komponenten.

   * Visa komponentens **Resurstyp** och **Resurssupertyp** (om den är definierad).

      * Om du klickar på Resurssupertypen kommer du till den komponenten.

  >[!NOTE]
  >
  >Eftersom `/apps` inte kan redigeras under körning är komponentkonsolen skrivskyddad.

* **Profiler**

  ![Profiler](assets/chlimage_1-169.png)

* **Live-användning**

  ![Live-användning](assets/chlimage_1-170.png)

  >[!CAUTION]
  >
  >På grund av den typ av information som samlas in för den här vyn kan det ta en stund att sortera/visa informationen.

* **Dokumentation**

  Om utvecklaren har tillhandahållit [dokumentation för komponenten](/help/sites-developing/developing-components.md#documenting-your-component) visas den på fliken **Dokumentation**. Om det inte finns någon tillgänglig dokumentation visas inte fliken **Dokumentation**.

  ![Dokumentation](assets/chlimage_1-171.png)
