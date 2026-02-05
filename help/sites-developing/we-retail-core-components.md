---
title: Testa de centrala komponenterna i We.Retail
description: Lär dig hur du arbetar med kärnkomponenter i Adobe Experience Manager med We.Retail.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 62b6d299-f44e-4af3-b5e1-b0e92ca0598a
source-git-commit: cc96a14ebaf9f895a798b5f4904f5b4769b990bb
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 3%

---

# Testa kärnkomponenterna i We.Retail{#trying-out-core-components-in-we-retail}

De centrala komponenterna är moderna, flexibla komponenter som är enkla att utöka och som gör det enkelt att integrera i projekten. Kärnkomponenterna har byggts kring flera viktiga designprinciper, som HTML, användbarhet, färdig installation, konfigurerbarhet, versionshantering och utbyggbarhet. Webbplatsen `We.Retail` är byggd på kärnkomponenter.

## Prova {#trying-it-out}

1. Starta Adobe Experience Manager (AEM) med exempelinnehållet `We.Retail` och öppna [komponentkonsolen](/help/sites-authoring/default-components-console.md).

   **Global navigering > Verktyg > Komponenter**

1. När du öppnar rälen i komponentkonsolen kan du filtrera efter en viss komponentgrupp. Kärnkomponenterna finns i

   * `.core-wcm`: Standardkärnkomponenterna
   * `.core-wcm-form`: Kärnkomponenter för att skicka formulär

   Välj `.core-wcm`.

   ![chlimage_1-162](assets/chlimage_1-162.png)

1. Alla kärnkomponenter använder namnet **v1** för att ange den första versionen av varje komponent. Regelbundna versioner planeras att lanseras framöver, som är versionskompatibla med AEM och gör det enkelt att uppgradera så att du kan utnyttja de senaste funktionerna.
1. Klicka på **Text (v1)**.

   Se till att komponentens **resurstyp** är `/apps/core/wcm/components/text/v1/text`. Kärnkomponenter finns under `/apps/core/wcm/components` och versionsindelas per komponent.

   ![chlimage_1-163](assets/chlimage_1-163.png)

1. Klicka på fliken **Dokumentation** för att se utvecklardokumentationen för komponenten.

   ![chlimage_1-164](assets/chlimage_1-164.png)

1. Återgå till komponentkonsolen. Filtrera för gruppen **`We.Retail`** och markera komponenten **Text**.
1. Se till att **Resurstypen** pekar på en komponent som förväntat under `/apps/weretail`, men **Resurssupertypen** pekar tillbaka på kärnkomponenten `/apps/core/wcm/components/text/v1/text`.

   ![chlimage_1-165](assets/chlimage_1-165.png)

1. Klicka på fliken **Live-användning** för att se vilka sidor den här komponenten används på. Klicka på den första **Tack**-sidan för att redigera sidan.

   ![chlimage_1-166](assets/chlimage_1-166.png)

1. På sidan Tack markerar du textkomponenten och klickar på ikonen Avbryt arv på komponentens Redigera-meny.

   [`We.Retail` har en global webbplatsstruktur &#x200B;](/help/sites-developing/we-retail-globalized-site-structure.md) där innehåll överförs från den primära språkwebbplatsen till [aktiva kopior via en mekanism som kallas arv](/help/sites-administering/msm.md). Arvet måste därför avbrytas om en användare ska kunna redigera text manuellt.

   ![chlimage_1-167](assets/chlimage_1-167.png)

1. Klicka på **Ja** för att bekräfta annulleringen.

   ![chlimage_1-168](assets/chlimage_1-168.png)

1. När arvet har avbrutits och du väljer textkomponenterna finns det många fler alternativ. Klicka på **Redigera**.

   ![chlimage_1-169](assets/chlimage_1-169.png)

1. Nu kan du se vilka redigeringsalternativ som är tillgängliga för textkomponenten.

   ![chlimage_1-170](assets/chlimage_1-170.png)

1. Välj **Redigera mall** på menyn **Sidinformation**.
1. Klicka på ikonen **Policy** för textkomponenten i **Layoutbehållaren** på sidan i mallredigeraren på sidan.

   ![chlimage_1-171](assets/chlimage_1-171.png)

1. Med huvudkomponenterna kan mallskapare konfigurera vilka egenskaper som är tillgängliga för sidförfattarna. Egenskaperna innehåller funktioner som tillåtna inklistringskällor, formateringsalternativ och tillgängliga styckeformat.

   Sådana designdialogrutor är tillgängliga för många viktiga komponenter och fungerar tillsammans med mallredigeraren. När de är aktiverade är de tillgängliga för författaren via komponentredigerarna.

   ![chlimage_1-172](assets/chlimage_1-172.png)

## Se även {#further-information}

Mer information om kärnkomponenter finns i redigeringsguiden [Core Components](https://experienceleague.adobe.com/sv/docs/experience-manager-core-components/using/introduction) för en översikt över funktioner. I guiden [Utveckla kärnkomponenter](https://experienceleague.adobe.com/sv/docs/experience-manager-core-components/using/developing/overview) finns en teknisk översikt.



Mer information om kärnkomponenterna finns i redigeringsdokumentet [Core Components](https://experienceleague.adobe.com/sv/docs/experience-manager-core-components/using/introduction). Där finns en översikt över kärnkomponentfunktioner, och i utvecklardokumentet [Developing Core Components](https://experienceleague.adobe.com/sv/docs/experience-manager-core-components/using/developing/overview) finns teknisk information.

Du kanske också vill undersöka [redigerbara mallar](/help/sites-developing/we-retail-editable-templates.md). Mer information om redigerbara mallar finns i redigeringsdokumentet [Skapa sidmallar](/help/sites-authoring/templates.md) eller i utvecklardokumentet Sida [Mallar - Redigerbar](/help/sites-developing/page-templates-editable.md).
