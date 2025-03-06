---
title: Provar kärnkomponenter i We.Retail
description: Lär dig hur du provar kärnkomponenter i Adobe Experience Manager med hjälp av We.Retail.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 62b6d299-f44e-4af3-b5e1-b0e92ca0598a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 4%

---

# Provar kärnkomponenter i We.Retail{#trying-out-core-components-in-we-retail}

De centrala komponenterna är moderna, flexibla komponenter som är enkla att utöka och som gör det enkelt att integrera i projekten. Kärnkomponenterna har byggts kring flera viktiga designprinciper, som HTML, användbarhet, färdig installation, konfigurerbarhet, versionshantering och utbyggbarhet. Vi.Retail har byggts på kärnkomponenter.

## Prova {#trying-it-out}

1. Starta Adobe Experience Manager (AEM) med exempelinnehållet We.Retail och öppna [Components Console](/help/sites-authoring/default-components-console.md).

   **Global navigering > Verktyg > Komponenter**

1. När du öppnar rälen i komponentkonsolen kan du filtrera efter en viss komponentgrupp. Kärnkomponenterna finns i

   * `.core-wcm`: Standardkärnkomponenterna
   * `.core-wcm-form`: Kärnkomponenter för att skicka formulär

   Välj `.core-wcm`.

   ![chlimage_1-162](assets/chlimage_1-162.png)

1. Alla kärnkomponenter har namnet **v1**, vilket visar att detta är den första versionen av kärnkomponenten. Vi kommer att lansera de senaste versionerna, som är versionskompatibla med AEM och som gör det enkelt att uppgradera så att du kan dra nytta av de senaste funktionerna.
1. Klicka på **Text (v1)**.

   Se till att komponentens **resurstyp** är `/apps/core/wcm/components/text/v1/text`. Kärnkomponenter finns under `/apps/core/wcm/components` och versionsindelas per komponent.

   ![chlimage_1-163](assets/chlimage_1-163.png)

1. Klicka på fliken **Dokumentation** för att se utvecklardokumentationen för komponenten.

   ![chlimage_1-164](assets/chlimage_1-164.png)

1. Återgå till komponentkonsolen. Filtrera för gruppen **We.Retail** och välj komponenten **Text**.
1. Se till att **Resurstypen** pekar på en komponent som förväntat under `/apps/weretail`, men **Resurssupertypen** pekar tillbaka på kärnkomponenten `/apps/core/wcm/components/text/v1/text`.

   ![chlimage_1-165](assets/chlimage_1-165.png)

1. Klicka på fliken **Live-användning** för att se vilka sidor den här komponenten används på. Klicka på den första **Tack**-sidan för att redigera sidan.

   ![chlimage_1-166](assets/chlimage_1-166.png)

1. På sidan Tack markerar du textkomponenten och klickar på ikonen Avbryt arv på komponentens Redigera-meny.

   [Vi.Retail har en global webbplatsstruktur](/help/sites-developing/we-retail-globalized-site-structure.md) där innehåll överförs från språkmallar till [live-kopior via en mekanism som kallas arv](/help/sites-administering/msm.md). Arvet måste därför avbrytas för att användaren ska kunna redigera text manuellt.

   ![chlimage_1-167](assets/chlimage_1-167.png)

1. Bekräfta annulleringen genom att klicka på **Ja**.

   ![chlimage_1-168](assets/chlimage_1-168.png)

1. När arvet har avbrutits och du väljer textkomponenterna finns det många fler alternativ. Klicka på **Redigera**.

   ![chlimage_1-169](assets/chlimage_1-169.png)

1. Nu kan du se vilka redigeringsalternativ som är tillgängliga för textkomponenten.

   ![chlimage_1-170](assets/chlimage_1-170.png)

1. Välj **Redigera mall** på menyn **Sidinformation**.
1. Klicka på ikonen **Policy** för textkomponenten i **Layoutbehållaren** på sidan i mallredigeraren på sidan.

   ![chlimage_1-171](assets/chlimage_1-171.png)

1. Med huvudkomponenterna kan mallskapare konfigurera vilka egenskaper som är tillgängliga för sidförfattarna. Det kan vara funktioner som tillåtna inklistringskällor, formateringsalternativ och tillgängliga styckeformat.

   Sådana designdialogrutor är tillgängliga för många viktiga komponenter och fungerar tillsammans med mallredigeraren. När de är aktiverade är de tillgängliga för författaren via komponentredigerarna.

   ![chlimage_1-172](assets/chlimage_1-172.png)

## Ytterligare information {#further-information}

Mer information om kärnkomponenterna finns i redigeringsdokumentet [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) för en översikt över kärnkomponenternas funktioner och i utvecklardokumentet [Developing Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/overview.html) för en teknisk översikt.

Du kanske också vill undersöka [redigerbara mallar](/help/sites-developing/we-retail-editable-templates.md) ytterligare. Mer information om redigerbara mallar finns i redigeringsdokumentet [Creating Page Templates](/help/sites-authoring/templates.md) eller i utvecklardokumentet Page [Templates - Editable](/help/sites-developing/page-templates-editable.md).
