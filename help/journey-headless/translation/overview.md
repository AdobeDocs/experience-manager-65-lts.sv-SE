---
title: AEM Headless Translation Journey
description: Börja här för en guidad resa genom att översätta ditt headless-innehåll med AEM kraftfulla översättningsverktyg.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,Language Copy
role: Admin, Architect,Data Architect,Developer,User,Leader
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1037'
ht-degree: 0%

---

# AEM Headless Translation Journey {#aem-headless-translation-journey}

Börja här för en guidad resa genom att översätta ditt headless-innehåll med AEM kraftfulla översättningsverktyg.

## Introduktion {#introduction}

Headless-implementering blir allt viktigare för att leverera upplevelser till er målgrupp, oavsett var de finns och oavsett kanal, region eller språkområde.

Den Headless-implementeringen förskjuter hantering av sidor och komponenter på samma sätt som traditionella lösningar med fullständiga stackar och fokuserar på att skapa kanalneutrala, återanvändbara fragment av innehåll och deras flerkanalsleverans. Med AEM kraftfulla översättningsverktyg kan dessa återanvändbara fragment enkelt översättas och levereras till er målgrupp oavsett var den befinner sig.

Den här guiden leder dig igenom de viktigaste rubrikfria översättningsämnena så att du när du är klar:

* Få en översikt över vad headless content delivery är.
* Lär dig AEM headless-funktioner.
* Förstå AEM översättningsfunktioner och hur de relaterar till headless-innehåll.
* Har möjlighet att börja översätta sitt eget headless-innehåll.

Målet är att ge er en bred förståelse för headless-teknik, hur AEM hanterar headless-innehåll och hur ni kan översätta det. Om du inte är bekant med något av dessa ämnen är detta den idealiska platsen att börja på.

Om du redan är bekant med AEM, headless och translation, kanske du redan har grundläggande kunskaper om den här resan. Överväg att referera till vår tekniska dokumentation som är länkad under avsnittet [ytterligare resurser nedan.](#additional-resources)

## AEM Documentation Journeys {#documentation-journeys}

[En dokumentationsresa](/help/journey-documentation/home.md) knyter ihop många olika och kanske komplicerade ämnen och funktioner genom att tillhandahålla en berättarröst som hjälper läsaren, som kan vara nybörjare på AEM, att förstå och lösa ett affärsproblem från början till slut, samtidigt som man antar minimala tidigare ämnesområden eller AEM-kunskaper.

Dokumentationsresor har utformats utifrån principer för god praxis som bygger på Adobe senaste forskning, beprövade implementeringserfarenheter från Adobe konsulter och feedback från kundprojekt.

Om du vill veta hur Adobe rekommenderar att du löser vanliga affärsärenden med AEM är det [AEM Headless Journeys](/help/journey-headless/overview.md) du som börjar.

## Målgrupp {#audience}

Den här resan är utformad för översättningsspecialistpersonal, som ofta kallas översättningsprojektledare eller TPM. Den här resan innehåller krav, steg och tillvägagångssätt för att översätta headless-innehåll i AEM. Resan kan definiera ytterligare personligheter som översättningsspecialisten måste interagera med, men utgångspunkten för resan är översättningsspecialistens.

På den här resan förutsätts att läsaren har erfarenhet av att översätta innehåll i ett stort CMS-system, men man förutsätter inte att man känner till headless-teknik eller AEM.

Följande personer interagerar på den här resan.

| Persona | Beskrivning | Roll på resan |
|---|---|---|
| Översättningsspecialist | Definierar vilket innehåll som ska översättas och hanterar dessa arbetsflöden | Målgrupp för denna resa |
| Innehållsförfattare | Skapar och hanterar innehåll som levereras utan problem | Innehållsförfattare skapar innehåll som översättningsspecialisten måste översätta. |
| Administratör | Hanterar grundkonfiguration och konfiguration av AEM | Översättningsexperten arbetar tillsammans med administratören för att göra konfigurationsändringar som behövs för översättning, till exempel installation av en översättningsanslutning. |
| Innehållsarkitekt | Analyserar kraven för de data som måste levereras utan huvud och definierar strukturen för dessa data | Översättningsspecialister arbetar med innehållsarkitekten för att definiera hur innehållet ska ordnas så att det enkelt kan översättas. |

Information i den här resan kan vara användbar för alla personer, men viss information kan vara överflödig för vissa roller. Stanna kvar på [kommande resor som omfattar ytterligare roller.](/help/journey-documentation/home.md#journeys)

## The Headless Translation Journey {#the-journey}

Du kommer att utforska många ämnen under den här resan. I följande artiklar får du grundläggande kunskaper om översättning av headless-innehåll i AEM och länkar till detaljerad teknisk dokumentation.

Även om du kan gå direkt till en viss del av resan bygger många koncept på en del i tidigare artiklar. Om du inte har använt rubrikfri översättning i AEM rekommenderar Adobe att du börjar i början och fortsätter sekventiellt.

| # | Artikel | Beskrivning |
|---|---|---|
| 0 | AEM Headless Translation Journey | Det här dokumentet |
| 1 | [Lär dig mer om headless-innehåll och hur du översätter det i AEM](learn-about.md) | Lär dig headless concepts, how they map to AEM, and the theof AEM translation. |
| 2 | [Kom igång med AEM headless translation](getting-started.md) | Lär dig hur du organiserar rubrikfritt innehåll och hur AEM översättningsverktyg fungerar. |
| 3 | [Konfigurera översättningsintegreringen](configure-connector.md) | Lär dig hur du ansluter AEM till en översättningstjänst. |
| 4 | [Konfigurera översättningsregler](translation-rules.md) | Lär dig hur du definierar översättningsregler för att identifiera innehåll för översättning. |
| 5 | [Översätt innehåll](translate-content.md) | Använd översättningsintegrering och regler för att översätta ert headless-innehåll. |
| 6 | [Publicera översatt innehåll](publish-content.md) | Lär dig hur du publicerar översatt innehåll och uppdaterar översättningen när det underliggande innehållet uppdateras. |

## What&#39;s Next {#what-is-next}

Nu är du redo att sätta igång med din Adobe headless-översättningsresa. Vi rekommenderar att du fortsätter till nästa del av resan och läser artikeln [Lär dig mer om headless-innehåll och hur du översätter det i AEM](learn-about.md)

## Ytterligare resurser {#additional-resources}

Dokumentationsresor visar hur AEM löser ett affärsproblem genom att tillhandahålla en berättarröst som vägleder dig genom komplexa, samhörande processer och funktioner. En resa visar hur flera funktioner fungerar tillsammans för att tillgodose ett enda affärsbehov.

Resorna är därför utformade för att stå på egen hand. Men flera kan vara relaterade till varandra. På de här extraresorna finns mer information om hur AEM kraftfulla funktioner fungerar tillsammans.

* [Headless Authoring Journey](/help/journey-headless/author/overview.md) - Börja här för en guidad resa med de kraftfulla och flexibla headless-funktionerna i AEM, deras funktioner och hur du modellerar ditt innehåll i ditt första headless-projekt.
* [Headless Architect Journey](/help/journey-headless/architect/overview.md) - Börja här för en introduktion till de kraftfulla och flexibla headless-funktionerna i Adobe Experience Manager och hur du modellerar innehåll för ditt projekt.
* [AEM Headless Developer Journey](/help/journey-headless/developer/overview.md) - Börja här för en guidad resa genom de kraftfulla och flexibla headless-funktionerna i AEM, deras funktioner och hur du använder dem i ditt första utvecklingsprojekt.
* [AEM tekniska dokumentation](https://experienceleague.adobe.com/docs/experience-manager-65.html) - Om du redan har en god förståelse för AEM och headless-teknik kan det vara bra att läsa våra tekniska dokument direkt.
   * En [introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)
* [AEM Headless-självstudiekurser](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html) - Om du föredrar att lära dig genom att göra och är tekniskt inriktad kan du ta del av våra praktiska självstudiekurser ordnade efter API och ramverk, som utforskar hur du skapar och använder program som bygger på AEM Headless.
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html)
