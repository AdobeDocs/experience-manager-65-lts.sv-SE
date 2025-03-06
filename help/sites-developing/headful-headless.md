---
title: Headless and Headless in AEM
description: AEM-projekt kan implementeras i en headful och headless-modell, men valet är inte binärt. AEM ger flexibilitet att utnyttja fördelarna med båda modellerna i ett och samma projekt.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin,Architect,Data Architect,Developer
exl-id: ba7f8ad9-807b-48d9-a4eb-da0a60d2494a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1023'
ht-degree: 0%

---

# Headless and Headless in AEM {#headful-headless}

Adobe Experience Manager-projekt kan implementeras i både headful och headless-modeller, men valet är inte binärt. AEM ger flexibilitet att utnyttja fördelarna med båda modellerna i ett och samma projekt. Det här dokumentet innehåller en översikt över de olika modellerna och en beskrivning av nivåerna av SPA-integrering.

## Ökning {#overview}

AEM har kraftfulla verktyg för att hantera både framtagning av innehåll och distribution av det på en och samma plattform. Detta är en traditionell&quot;headful&quot; modell för innehållshantering, där innehållsförfattare och utvecklare arbetar på samma plattform för att leverera upplevelserna till innehållskunderna.

AEM kan också användas för att enkelt hantera innehåll, vilket gör att presentationen och leveransen av materialet kan hanteras av en annan plattform. Det här är den&quot;headless&quot; modellen för innehållshantering, där innehållsförfattare och utvecklare arbetar på olika plattformar för att leverera upplevelser till innehållskunderna.

Men detta behöver inte vara ett binärt val. AEM erbjuder oöverträffad flexibilitet så att du kan utnyttja fördelarna med båda modellerna i ditt projekt.

![AEM implementeringsmodeller](/help/sites-developing/headless/getting-started/assets/aem-implementation-models.png)

I en modell med headful eller full stack hanteras innehållet i AEM-databasen och AEM-komponenter som baseras på Java, HTML och så vidare används för att återge innehållet för användarupplevelsen. I den här modellen skapas innehållet, formateras, presenteras och levereras allt i AEM.

I en headlessmodell hanteras innehållet i AEM-databasen, men levereras via API:er som REST och GraphQL till ett annat system för att återge innehållet för användarupplevelsen. I den här modellen skapas innehåll i AEM, men formge det, presentera det och leverera det på en annan plattform.

Single Page Applications (SPA) är ofta målet för innehåll som levereras utan extra kostnad av AEM. Dessa produktresuméer behöver dock inte vara helt externa för AEM. Med AEM kan ni avgöra i vilken utsträckning era SPA-program är integrerade i AEM. Låt oss ta ett exempel.

## Exempel på Web Shop {#web-shop-example}

Säg att du har en befintlig webbutik för ditt företag som SPA. Där finns all produktinformation och alla bilder. Sedan introducerar ni AEM som stöd för era marknadsföringssatsningar, som reklamwebbplatser, bloggar och kampanjinnehåll. Hur integrerar man de två? AEM har ett stort antal alternativ:

* **Tillåt att systemen fungerar oberoende.**
* **Tillhandahåll webbbutiken med begränsat innehåll från AEM via GraphQL.**-innehåll kan skapas av författare i AEM, men bara visas via SPA-filen för webbutiken.
* **Bädda in webshop-SPA i AEM.**-innehåll kan skapas av författare i AEM och visas i AEM i webbutikens kontext, men inte ändras.
* **Bädda in webshop-SPA i AEM och aktivera redigerbara punkter.** Innehåll kan skapas av författare i AEM och visas i AEM i webbutikens kontext, och författarna har begränsad möjlighet att ändra innehållet i Web shop SPA i AEM.
* **Bädda in webshop-SPA i AEM och aktivera hela zoner för redigering.** Innehåll kan skapas av författare i AEM och visas i AEM i webbutikens kontext, och författarna har begränsad möjlighet att ändra innehållet i Web shop SPA i AEM.

I nästa avsnitt beskrivs dessa integreringsnivåer mer ingående.

>[!NOTE]
>
>Du kan förstås även återimplementera SPA för webben som en fullt fungerande AEM SPA [ med AEM SPA Editor-ramverket.](/help/sites-developing/spa-walkthrough.md) Om du redan har AEM och vill skapa en webshop eller annan SPA rekommenderas den här metoden, men det är utanför omfånget för det här dokumentet.

## SPA-integreringsnivåer {#integration-levels}

SPA-integreringen faller på ett spektrum av fyra nivåer i AEM.

* **Nivå 0: Ingen integrering**
   * SPA och AEM finns separat och utbyter ingen information.
   * Innehållet skapas, hanteras och levereras oberoende av varandra i två olika system.
* **Nivå 1: Integrering av innehållsfragment**
   * [Innehållsfragment](/help/assets/content-fragments/content-fragments.md) används i AEM för att skapa och hantera begränsat innehåll för SPA.
   * SPA hämtar det här innehållet via AEM [GraphQL API.](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md)
   * En del innehåll hanteras i AEM och andra i ett externt system.
   * Innehåll kan bara visas i SPA-filen.
* **Nivå 2: Bädda in SPA i AEM**
   * [Innehållsfragment](/help/assets/content-fragments/content-fragments.md) används i AEM för att skapa och hantera innehåll för SPA.
   * SPA hämtar det här innehållet via AEM [GraphQL API.](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md)
   * En del innehåll hanteras i AEM och andra i ett externt system.
   * Innehåll kan visas i sitt sammanhang i AEM.
   * Begränsat innehåll kan redigeras i AEM.
* **Nivå 3: Bädda in och aktivera SPA helt i AEM**
   * [Innehållsfragment](/help/assets/content-fragments/content-fragments.md) används i AEM för att skapa och hantera innehåll för SPA.
   * SPA hämtar det här innehållet via AEM [GraphQL API.](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md)
   * Innehåll kan visas i sitt sammanhang i AEM.
   * De flesta typer av innehåll kan redigeras i AEM.

Nivå 1 är ett exempel på en typisk headless-implementering. Innehållsförfattare kan dock bara visa sitt innehåll i sitt sammanhang inom SPA. AEM är bara ett redigeringsverktyg.

Fördelen och flexibiliteten med AEM blir tydlig med nivåerna 2 och 3 samtidigt som fördelarna med SPA-programmen bibehålls. Innehållsförfattare kan skapa sitt innehåll i AEM, men de kan också se det direkt i AEM. SPA-avtalet ger möjlighet att skrivas i AEM, men kan fortfarande levereras som ett SPA-avtal.

## Implementera integreringsnivåer {#implementing}

Det finns olika verktyg i AEM beroende på vilken integrationsnivå du väljer. Varje nivå bygger på de verktyg som använts tidigare. Följande lista länkar till relevanta resurser.

* **Nivå 1:** Innehållsfragment och [AEM headless Framework](/help/sites-developing/headless/introduction.md) kan användas för att leverera AEM-innehåll till SPA.
* **Nivå 2:** Förutom nivå ett:
   * [RemotePage-komponenten](/help/sites-developing/spa-remote-page.md) kan användas för att bädda in den externa SPA-filen i AEM där AEM-innehåll kan visas i sitt sammanhang.
   * Vissa punkter i SPA kan också aktiveras för att [tillåta begränsad redigering i AEM.](/help/sites-developing/spa-edit-external.md)
* **Nivå 3:** Förutom nivå två:
   * Hela zoner i SPA kan aktiveras för omfattande redigering i AEM.
