---
title: Testa innehållsfragment i webb.detaljhandel
description: Lär dig hur du provar innehållsfragment i Adobe Experience Manager med hjälp av We.Retail.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Content Fragments,Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Testa innehållsfragment i webb.detaljhandel{#trying-out-content-fragments-in-we-retail}

Med innehållsfragment kan du skapa kanalneutralt innehåll tillsammans med (eventuellt kanalspecifika) variationer. **We.Retail** (som tillgängligt i en körklar instans av Adobe Experience Manager) innehåller fragmentet **Arktisk surfning i Lofoten** som ett grundläggande exempel. Detta visar att:

* Adobe Experience Manager (AEM) innehållsfragment [skapas och hanteras som sidoberoende resurser](/help/assets/content-fragments/content-fragments.md). Med dem kan du skapa kanalneutralt innehåll tillsammans med (eventuellt kanalspecifika) variationer.

   * Se [Var du kan hitta resurser för innehållsfragment i We.Retail](#where-to-find-content-fragments-in-we-retail)

* Du kan sedan [använda dessa fragment och deras varianter när du redigerar](/help/sites-authoring/content-fragments.md) dina innehållssidor.

   * Se [Var innehållsfragment används i Web.Retail](#where-content-fragments-are-used-in-we-retail)

Den fullständiga dokumentationen om hur du skapar, hanterar, använder och utvecklar innehållsfragment finns i:

* Se [Mer information](#further-information)

>[!NOTE]
>
>**Innehållsfragment** och **[Upplevelsefragment](/help/sites-authoring/experience-fragments.md)** är olika funktioner i AEM:
>
>* **Innehållsfragment** är redaktionellt innehåll, främst text och relaterade bilder. De är rent innehåll, utan design och layout.
>* **Upplevelsefragment** är helt utformat för innehåll, ett fragment av en webbsida.
>
>Upplevelsefragment kan innehålla innehåll i form av innehållsfragment, men inte tvärtom.

## Var kan man hitta innehållsfragment i webb.butik {#where-to-find-content-fragments-in-we-retail}

Det finns flera exempelinnehållsfragment i We.Retail; navigera via **Assets**, **Files**, **We.Retail**, **English**, **Experiences**.

Detta inkluderar **Arktisk surfning i Lofoten**, ett fragment tillsammans med relaterade visuella resurser:

* Navigera med hjälp av **Assets**, **Filer**, **We.Retail**, **English**, **Experiences**, **Arktisk surfning i Lofoten**:

   * [http://localhost:4502/assets.html/content/dam/we-retail/en/experiences/arctic-surfing-in-lofoten](http://localhost:4502/assets.html/content/dam/we-retail/en/experiences/arctic-surfing-in-lofoten)

![cf-44](assets/cf-44.png)

Du kan markera och redigera **arktisk surfning i fragmentet Lofoten**:

* [http://localhost:4502/editor.html/content/dam/we-retail/en/experiences/arctic-surfing-in-lofoten/arctic-surfing-in-lofoten](http://localhost:4502/editor.html/content/dam/we-retail/en/experiences/arctic-surfing-in-lofoten/arctic-surfing-in-lofoten)

Här kan du [redigera och hantera](/help/assets/content-fragments/content-fragments.md) dina fragment med hjälp av flikarna (vänsterpanelen):

<!--![cf-45-aa](do-not-localize/cf-45-aa.png) ![cf-45-a](do-not-localize/cf-45-a.png) ASSET does not exist-->

* **[Variationer](/help/assets/content-fragments/content-fragments-variations.md)** inklusive [Markdown](/help/assets/content-fragments/content-fragments-markdown.md)
* **[Associerat innehåll](/help/assets/content-fragments/content-fragments-assoc-content.md)**
* **[Metadata](/help/assets/content-fragments/content-fragments-metadata.md)**

![cf-46](assets/cf-46.png)

## Där innehållsfragment används i webb.butik {#where-content-fragments-are-used-in-we-retail}

För att illustrera [sidredigering med ett innehållsfragment](/help/sites-authoring/content-fragments.md) finns det flera exempelsidor under, till exempel:

* [http://localhost:4502/sites.html/content/we-retail/language-masters/en/experience](http://localhost:4502/sites.html/content/we-retail/language-masters/en/experience)

Innehållsfragmentet **Arktisk surfning i Lofoten** refereras till på sidan Platser:

* Navigera via **Sites**, **We.Retail**, **Language Masters**, **English**, **Experience**. Öppna sedan **Arktisk surfning i Lofoten** för redigering:

   * [http://localhost:4502/editor.html/content/we-retail/language-masters/en/experience/arctic-surfing-in-lofoten.html](http://localhost:4502/editor.html/content/we-retail/language-masters/en/experience/arctic-surfing-in-lofoten.html)

![cf-53](assets/cf-53.png)

## Ytterligare information {#further-information}

Mer information finns i:

* [Arbeta med innehållsfragment](/help/assets/content-fragments/content-fragments.md)

   * Lär dig hur du skapar, redigerar och hanterar resurser för innehållsfragment.

* [Sidredigering med innehållsfragment](/help/sites-authoring/content-fragments.md)

   * Använd ditt innehållsfragment när du redigerar en sida.

* [Utveckla AEM - komponenter för innehållsfragment](/help/sites-developing/components-content-fragments.md)

   * En översikt över komponenterna för innehållsfragment.

* [Utveckla och utöka innehållsfragment](/help/sites-developing/customizing-content-fragments.md)

   * Information som hjälper dig att utveckla och utöka innehållsfragment.
