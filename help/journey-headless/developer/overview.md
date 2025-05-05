---
title: AEM Headless Developer Journey
description: AEM Headless CMS Documentation. Börja här för en guidad resa med de kraftfulla och flexibla headless-funktionerna i AEM, deras funktioner och hur du använder dem i ditt första utvecklingsprojekt.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin, Developer
exl-id: 1425c1b4-3c47-47ff-b2ef-408e889ddb34
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1204'
ht-degree: 1%

---

# AEM Headless Developer Journey {#aem-headless-developer-journey}

Börja här för en guidad resa med de kraftfulla och flexibla headless-funktionerna i AEM, deras funktioner och hur du använder dem i ditt första headless-utvecklingsprojekt. Den här resan ger dig all den AEM Headless Documentation du behöver för att utveckla din första headless-applikation.

## Introduktion {#introduction}

Den Headless-implementeringen förskjuter hantering av sidor och komponenter på samma sätt som traditionella lösningar med fullständiga stackar och fokuserar på att skapa kanalneutrala, återanvändbara fragment av innehåll och deras flerkanalsleverans. Det är ett modernt och dynamiskt utvecklingsmönster för implementering av digitala upplevelser.

Den här guiden leder dig igenom de mest rubrikfria implementeringsämnena i AEM så att du när du är klar:

* Få en fullständig förståelse för vad headless content delivery är och dess fördelar.
* Förstå AEM headless-funktioner och hur de fungerar tillsammans för att leverera en headless-upplevelse.
* Har möjlighet att ta de första stegen för att implementera ditt första headless-projekt från AEM.

## AEM Documentation Journeys {#documentation-journeys}

[En dokumentationsresa](/help/journey-documentation/home.md) knyter ihop många olika och kanske komplicerade ämnen och funktioner genom att tillhandahålla en berättarröst som hjälper läsaren, som kan vara nybörjare på AEM, att förstå och lösa ett affärsproblem från början till slut, samtidigt som man antar minimala tidigare ämnesområden eller AEM-kunskaper.

Dokumentationsresor har utformats utifrån principer för god praxis som bygger på Adobe senaste forskning, beprövade implementeringserfarenheter från Adobe konsulter och feedback från kundprojekt.

Om du vill veta hur Adobe rekommenderar att du löser vanliga affärsärenden med AEM är det [AEM Headless Journeys](/help/journey-headless/overview.md) du som börjar.

>[!TIP]
>
>Om du föredrar att **lära dig genom att göra** och är tekniskt inriktad kan du gå till självstudiekurserna för AEM Headless, som är ordnade i API och ramverk och som finns tillgängliga i avsnittet [Ytterligare resurser](#additional-resources) i slutet av det här dokumentet.

## Målgrupp {#audience}

Den här resan är utformad för utvecklare och innehåller krav, steg och tillvägagångssätt för ett AEM Headless-projekt ur utvecklarens perspektiv. Resan definierar ytterligare personligheter som utvecklaren måste interagera med för ett framgångsrikt projekt, men utgångspunkten för resan är utvecklarens.

Följande personer interagerar på den här resan.

| Persona | Beskrivning | Roll på den här resan |
|---|---|---|
| Utvecklare (målgrupp) | Har erfarenhet av att utveckla headless-program som använder innehåll från olika källor | Målgrupp för den här resan |
| Innehållsförfattare | Skapar och hanterar innehåll som levereras utan problem | Innehållsförfattare skapar innehåll som utvecklaren själv levererar. |
| Administratör | Hanterar grundkonfiguration och konfiguration av AEM | Utvecklaren arbetar tillsammans med administratören för att göra de konfigurationsändringar som behövs för utvecklingen. |
| Innehållsarkitekt | Analyserar kraven för de data som måste levereras utan huvud och definierar strukturen för dessa data | Utvecklarna arbetar tillsammans med innehållsarkitekten för att förstå datastrukturen och kraven för att kunna leverera den utan problem. |

Information i den här resan kan vara användbar för alla personer, men viss information kan vara överflödig för vissa roller. Stanna kvar på [kommande resor som omfattar ytterligare roller.](/help/journey-documentation/home.md#journeys)

## The Headless Developer Journey {#the-journey}

Du kommer att utforska många ämnen under den här resan. I följande artiklar får du grundläggande kunskaper om headless i AEM och kan länka till detaljerad teknisk dokumentation.

Även om du kan gå direkt till en viss del av resan bygger många koncept på en del i tidigare artiklar. Därför rekommenderar Adobe att du startar i början och fortsätter sekventiellt om du inte är van vid headless i AEM.

| # | Artikel | Beskrivning |
|---|---|---|
| 0 | AEM Headless Developer Journey | Det här dokumentet |
| 1 | [Läs om CMS Headless Development](learn-about.md) | Läs mer om Headless Technology och när du ska använda den. |
| 2 | [Komma igång med AEM Headless](getting-started.md) | Läs om kraven för AEM Headless |
| 3 | [Sökväg till din första upplevelse med AEM Headless](path-to-first-experience.md) | Konfigurera utvecklingsmiljön och lär dig hur du integrerar en enkel app med AEM Headless |
| 4 | [Så här modellerar du innehåll](model-your-content.md) | Lär dig modellera innehållsstrukturen. Förverkliga sedan strukturen för Adobe Experience Manager (AEM) med Content Fragments Models och Content Fragments, för återanvändning i alla kanaler. |
| 5 | [Åtkomst till ditt innehåll via AEM-API:er](access-your-content.md) | Lär dig hur du använder GraphQL-frågor för att komma åt innehåll i innehållsfragment. |
| 6 | [Så här uppdaterar du innehåll via AEM Assets API:er](update-your-content.md) | Lär dig hur du använder REST API för att komma åt och uppdatera innehåll i innehållsfragment. |
| 7 | [Så här sätter du samman allt - din app och ditt innehåll i AEM Headless](put-it-all-together.md) | Lär dig hur du tar ditt AEM-projekt och förbereder det för publicering med AEM Headless SDK. |
| 8 | [Så här publicerar du med ditt headless-program](go-live.md) | Lär dig hur du distribuerar program live och tar din lokala kod i Git och flyttar den till Cloud Manager Git för CI/CD-överföring. |
| 9 | [Valfritt - Så här skapar du SPA-program (single page applications) med AEM](create-spa.md) | När du väl har förstått AEM headless-funktioner kan du testa hur du kombinerar headful och headless-leverans och lära dig hur du kan skapa redigerbara SPA med AEM SPA Editor-ramverk. |

## What&#39;s Next {#what-is-next}

Nu är du redo att sätta igång med din Adobe Headless-resa. Vi rekommenderar att du fortsätter till nästa del av resan och läser artikeln [Läs mer om CMS Headless Development.](learn-about.md)

### Välj din egen Adventure {#choose-your-path}

Adobe vill dock att du ska lyckas när du börjar med ditt AEM Headless-projekt, oavsett din inlärningsstil. Så tänk på de här två alternativen.

* Om du föredrar att **lära dig mer om headless-koncept och AEM headless-tekniker** bör du fortsätta din AEM-resa så som du rekommenderas i nästa granskning av dokumentet [Så här modellerar du ditt innehåll som AEM Content Models](model-your-content.md) där du får lära dig att modellera din innehållsstruktur i AEM.
* Om du föredrar att **lära dig genom att göra** kan du gå direkt till självstudiekursen [Komma igång med AEM Headless ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=sv-SE) där du kommer direkt till AEM Headless-utvecklingen genom att implementera ett enkelt projekt för att visa AEM headless-innehåll.

## Ytterligare resurser {#additional-resources}

Dokumentationsresor visar hur AEM löser ett affärsproblem genom att tillhandahålla en berättarröst som vägleder dig genom komplexa, samhörande processer och funktioner. En resa visar hur flera funktioner fungerar tillsammans för att tillgodose ett enda affärsbehov.

Resorna är därför utformade för att stå på egen hand. Men flera kan vara relaterade till varandra. På de här extraresorna finns mer information om hur AEM kraftfulla funktioner fungerar tillsammans.

* [AEM Headless-självstudiekurser](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE) - Om du föredrar att lära dig genom att göra och är tekniskt inriktad kan du ta del av våra praktiska självstudiekurser ordnade efter API och ramverk, som utforskar hur du skapar och använder program som bygger på AEM Headless.
* [AEM Headless Translation Journey](/help/journey-headless/translation/overview.md) - Den här dokumentationsresan ger dig en bred förståelse för headless-teknik, hur AEM fungerar som headless-innehåll och hur du kan översätta det.
* [Headless Authoring Journey](/help/journey-headless/author/overview.md) - Börja här för en guidad resa med de kraftfulla och flexibla headless-funktionerna i AEM, deras funktioner och hur du modellerar ditt innehåll i ditt första headless-projekt.
* [Headless Architect Journey](/help/journey-headless/architect/overview.md) - Börja här för en introduktion till de kraftfulla och flexibla headless-funktionerna i Adobe Experience Manager och hur du modellerar innehåll för ditt projekt.
* [AEM tekniska dokumentation](https://experienceleague.adobe.com/docs/experience-manager-65.html?lang=sv-SE) - Om du redan har en god förståelse för AEM och headless-teknik kan det vara bra att läsa våra tekniska dokument direkt.

   * En [introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)

* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)
