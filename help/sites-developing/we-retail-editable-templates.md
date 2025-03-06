---
title: Provar Editable Templates in We.Retail
description: Lär dig hur du provar redigerbara mallar i Adobe Experience Manager med We.Retail.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: f1141b8c-12a2-44a0-8c15-b614398b5174
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 1%

---

# Provar Editable Templates in We.Retail{#trying-out-editable-templates-in-we-retail}

Med de redigerbara mallarna är det inte längre bara en uppgift för utvecklare att skapa och underhålla mallar. En typ av avancerade användare, som kallas mallskapare, kan nu skapa mallar. Utvecklare måste fortfarande konfigurera miljön, skapa klientbibliotek och skapa komponenter som ska användas, men när dessa grunder väl är på plats kan mallskaparen skapa och konfigurera mallar utan ett utvecklingsprojekt.

Alla sidor i We.Retail baseras på redigerbara mallar, vilket gör att icke-utvecklare kan anpassa och anpassa mallarna.

## Prova {#trying-it-out}

1. Redigera sidan Utrustning i huvudgrenen för språk.

   http://localhost:4502/editor.html/content/we-retail/language-masters/en/equipment.html

1. Lägesväljaren har inte längre något designläge. Alla sidor för webben.Detaljhandel baseras på redigerbara mallar och för att ändra designen av redigerbara mallar måste de redigeras i mallredigeraren.
1. Välj **Redigera mall** på menyn **Sidinformation**.
1. Nu redigerar du Hero Page-mallen.

   Med hjälp av sidans strukturläge kan du ändra mallens struktur. Detta inkluderar till exempel de komponenter som är tillåtna i layoutbehållaren.

   ![chlimage_1-138](assets/chlimage_1-138.png)

1. Konfigurera principerna för Layoutbehållaren för att definiera vilka komponenter som tillåts i behållaren.

   Profiler motsvarar designkonfigurationer.

   ![chlimage_1-139](assets/chlimage_1-139.png)

1. I layoutdialogrutan för layoutbehållaren kan du

   * Välj en befintlig princip eller skapa en profil för behållaren
   * Välj vilka komponenter som tillåts i behållaren
   * Definiera de standardkomponenter som ska placeras när en resurs dras till behållaren

   ![chlimage_1-140](assets/chlimage_1-140.png)

1. I mallredigeraren kan du redigera principen för textkomponenten i layoutbehållaren.

   På så sätt kan du:

   * Välj en befintlig princip eller skapa en profil för behållaren
   * Definiera de funktioner som är tillgängliga för sidförfattaren när den här komponenten, som

      * Tillåtna inklistringskällor
      * Formateringsalternativ
      * Tillåtna styckeformat
      * Specialtecken tillåts

   Många komponenter som bygger på kärnkomponenterna gör det möjligt att konfigurera alternativ på komponentnivå med hjälp av redigerbara mallar, vilket eliminerar behovet av anpassning av utvecklare.

   ![chlimage_1-141](assets/chlimage_1-141.png)

1. I mallredigeraren kan du använda lägesväljaren för att ändra till läget **Inledande innehåll** för att definiera vilket innehåll som krävs på sidan.

   **Layout**-läget kan användas på samma sätt som på en normal sida för att definiera mallens layout.

## Mer information {#more-information}

Mer information om redigerbara mallar finns i redigeringsdokumentet [Skapa sidmallar](/help/sites-authoring/templates.md) eller i utvecklardokumentet Sida [Mallar - Redigerbar](/help/sites-developing/page-templates-editable.md) .

Du kanske också vill undersöka [kärnkomponenter](/help/sites-developing/we-retail-core-components.md). En teknisk översikt finns i redigeringsdokumentet [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) om du vill ha en översikt över funktionerna i kärnkomponenterna och i utvecklardokumentet [Developing Core Components](https://helpx.adobe.com/experience-manager/core-components/using/developing.html) .
