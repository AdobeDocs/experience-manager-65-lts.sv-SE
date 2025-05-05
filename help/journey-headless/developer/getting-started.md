---
title: Kom igång med AEM Headless
description: I den här delen av AEM Headless Developer Journey kan du lära dig mer om AEM Headless-kraven.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin, Developer
exl-id: 2bae83cc-ad0e-4a6a-a56e-1aa6533bde7e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2999'
ht-degree: 0%

---

# Kom igång med AEM Headless {#getting-started}

I den här delen av [AEM Headless Developer Journey ](overview.md) får du lära dig mer om vad som krävs för att du ska kunna komma igång med ditt eget projekt med AEM Headless.

## Story hittills {#story-so-far}

I det tidigare dokumentet om AEM resa utan rubriker [Lär dig mer om CMS Headless Development](learn-about.md) lärde du dig grunderna i vad ett headless CMS är och du bör nu:

* Förstå de grundläggande begreppen och terminologin för leverans av headless-innehåll
* Förstå varför och när headless krävs
* Lär dig på en hög nivå hur headless-koncept används och hur de fungerar ihop

Den här artikeln bygger vidare på dessa grunder så att du förstår hur du kan använda AEM för att implementera en headless-lösning.

## Syfte {#objective}

Det här dokumentet hjälper dig att förstå AEM Headless i ditt projekt. När du har läst bör du:

* Förstå grunderna i AEM headless-funktioner.
* Lär dig grunderna för AEM headless-funktioner.
* Var medveten om AEM avancerade integrationsnivåer.
* Du kan definiera projektet utifrån dess omfång.

## AEM Basics {#aem-basics}

Innan du kan definiera ett headless-projekt i AEM är det viktigt att du förstår några grundläggande AEM-koncept.

### Författarinstans {#author}

AEM består av en författarinstans och en [publiceringsinstans](#publish) som fungerar tillsammans för att skapa, hantera och publicera ditt innehåll.

Innehållet börjar på författarinstansen. Här skapar innehållsförfattare sitt innehåll. I författarmiljön finns olika verktyg som författare kan använda för att skapa, ordna och återanvända sitt innehåll.

### Publiceringsinstans {#publish}

När innehållet har skapats i författarinstansen måste det publiceras för att vara tillgängligt för andra tjänster att använda. En publiceringsinstans innehåller allt innehåll som har publicerats.

### Replikering {#replication}

Replikering innebär att överföra innehåll från författarinstansen till publiceringsinstansen. Detta görs automatiskt av AEM när en författare eller annan användare med rätt behörighet publicerar innehåll.

### AEM Basics Summary {#aem-basics-summary}

På den enklaste nivån krävs följande steg för att skapa digitala upplevelser i AEM:

1. Dina innehållsförfattare skapar ditt headless-innehåll i författarinstansen.
1. När innehållet är klart replikeras det till publiceringsinstansen.
1. API:er kan sedan anropas för att hämta det här innehållet.

AEM Headless bygger vidare på denna tekniska grund genom kraftfulla verktyg för att hantera headless-innehåll, som [beskrivs i nästa avsnitt.](#aem-headless-basics)

## Grundläggande om AEM Headless {#aem-headless-basics}

AEM headless-funktioner bygger på några viktiga funktioner. Dessa förklaras i detalj i senare delar av resan. Det är nu bara viktigt att veta vad de gör och vad de kallas.

### Modeller för innehållsfragment {#content-fragment-models}

Modeller för innehållsfragment definierar strukturen för data och innehåll som du skapar och hanterar i AEM. De fungerar som en sorts ställningar för ert innehåll. När du väljer att skapa innehåll väljer författarna bland de innehållsfragmentsmodeller du definierar, som vägleder dem när de skapar innehåll.

### Innehållsfragment {#content-fragments}

Med Content Fragments kan du utforma, skapa, strukturera och publicera sidoberoende innehåll. De gör att du kan förbereda innehåll för användning på flera platser och i flera kanaler.

Innehållsfragment innehåller strukturerat innehåll och kan levereras i JSON-format.

### API:er för GraphQL och REST {#apis}

AEM erbjuder två robusta API:er för att ändra ert innehåll utan problem.

* Med GraphQL API kan du skapa förfrågningar om åtkomst till och leverans av innehållsfragment.
* Med Assets REST API kan du skapa och ändra innehållsfragment (och andra resurser).

Du får lära dig mer om dessa API:er och hur du kan använda dem i en senare del av AEM resa utan avbrott. Du kan också läsa avsnittet [ytterligare resurser](#additional-resources) nedan om du vill ha mer information.

## Headless Integration Levels {#integration-levels}

AEM har stöd för både den fullständiga headless-modellen och den traditionella fullstacksmodellen eller headful-modellen i en CMS. AEM erbjuder dock inte bara dessa två exklusiva alternativ, utan även möjligheten att stödja hybridmodeller som kombinerar fördelarna med båda, vilket ger unik flexibilitet för ditt headless-projekt.

För att du ska förstå headless-koncept fokuserar AEM Headless Developer Journey på den rena headless-modellen så att du kommer igång så fort som möjligt utan kodning i AEM.

Du bör dock vara medveten om de extra hybridmöjligheterna som är öppna för dig när du väl har förstått AEM headless-funktioner. Dessa fall beskrivs nedan för att du ska vara medveten om dem. I slutet av resan kommer du att få en mer detaljerad introduktion till dessa koncept om en sådan flexibilitet krävs för ditt projekt.

### Du har redan ett externt gränssnitt med headless-innehåll, till exempel ett enda program (SPA). {#already-have-a-spa}

Låt oss anta att ditt grundläggande krav är åtminstone att leverera innehåll från AEM till en befintlig, extern tjänst.

#### Nivå 1: Integrering av innehållsfragment - traditionell Headless-modell {#level-1}

Den här integreringsnivån är den traditionella headless-modellen, som gör att skribenterna kan skapa innehåll i AEM och leverera det utan problem till valfritt antal externa tjänster med GraphQL eller redigera det från externa tjänster med Assets API. Ingen kodning krävs i AEM.

I den här modellen används AEM bara för att skapa och leverera innehåll med AEM Content Fragments. Återgivning och interaktion med innehållet delegeras till det uppladdande externa programmet, ofta ett SPA-program (single page application).

#### Nivå 2: Bädda in SPA i AEM - hybridmodell {#level-2}

Den här integreringsnivån bygger på den första nivån, men tillåter även att det externa programmet (SPA) bäddas in i AEM så att innehållsförfattarna kan visa innehållet i det externa programmet i AEM. Programmet har även stöd för begränsad redigering av det externa programmet i AEM.

Denna nivå har fördelen att man på ett flexibelt sätt kan skapa innehåll i AEM med sitt innehåll presenterat i sitt sammanhang med en inbyggd extern SPA, samtidigt som innehållet fortfarande levereras utan problem.

#### Nivå 3: Bädda in och aktivera SPA fullständigt i AEM - hybridmodell {#level-3}

Denna nivå av integration bygger på nivå två genom att göra det möjligt att redigera det mesta av det externa SPA-innehållet inom AEM.

### Du har ännu inte någon extern konsument av det Headless-innehåll som ett SPA-program (single page application). {#do-not-have-a-spa}

Om målet är att skapa en SPA som konsumerar innehåll i bakgrunden från AEM kan du använda funktioner som Content Fragments för att hantera ditt headless-innehåll och även skapa en SPA med AEM SPA Editor-ramverk.

Med SPA Editor konsumerar SPA inte bara innehåll från AEM, utan kan även redigeras fullt ut i AEM av innehållsförfattarna, vilket ger dig både flexibiliteten med headlessleverans och kontextredigering i AEM.

## Krav och krav {#requirements-prerequisites}

Det finns flera krav innan du börjar ditt headless AEM-projekt.

### Kunskap {#knowledge}

* GraphQL
* Utvecklingserfarenhet som skapar SPA med React- eller Angular-ramverk
* AEM grundläggande kunskaper i att skapa innehållsfragment och använda redigeraren

### verktyg {#tools}

* Sandlådeåtkomst för testning av driftsättning av ditt projekt
* Lokal utvecklingsinstans för datamodellering och -testning
* Befintlig extern SPA eller annan konsument av ditt headless AEM-innehåll

## Definiera ditt projekt {#defining-your-project}

För varje framgångsrikt projekt är det viktigt att tydligt definiera inte bara projektets krav, utan även roller och ansvar.

### Omfång {#scope}

Det är viktigt att ha ett tydligt definierat utrymme för projektet. Med Omfång får du information om acceptanskriterier och du kan definiera det du gjort.

Den första frågan du måste ställa är&quot;Vad försöker jag uppnå med AEM Headless?&quot; Svaret bör i allmänhet vara att du har eller kommer att ha ett upplevelseprogram som du har skapat med dina egna utvecklingsverktyg, inte med AEM. Det här upplevelseprogrammet kan vara en mobilapp, en webbplats eller någon annan kundupplevelseapplikation som vänder sig till slutanvändaren. Målet med AEM Headless är att förse ditt upplevelseprogram med innehåll som skapas, lagras och hanteras i AEM med toppmoderna API:er som anropar AEM Headless för att hämta innehåll eller till och med fullständigt CRUD-innehåll direkt från ditt upplevelseprogram. Om detta inte är vad du vill göra, vill du antagligen [gå tillbaka till AEM-dokumentationen](https://experienceleague.adobe.com/docs/experience-manager-65.html?lang=sv-SE) och hitta det avsnitt som passar bättre för det du vill uppnå.

### Roller och ansvarsområden {#roles-responsibilities}

Rollerna för enskilda projekt varierar, men det är viktigt att tänka på följande när det gäller innehållet i AEM headless Development:

* [Administratör](#administrator)
* [Innehållsförfattare](#content-author)
* [Innehållsarkitekt](#content-architect)
* [Developer](#developer)

#### Administratör {#administrator}

Administratören ansvarar för systemets grundkonfiguration och konfiguration. Administratören kan t.ex. konfigurera din organisation i Adobe användarhanteringssystem, som det hänvisas till Identity Management System (IMS). Administratören är den första användaren i organisationen som får en e-postinbjudan från Adobe när din organisation har skapats av Adobe i IMS. Administratören kan logga in på IMS och lägga till användare från andra profiler.

När användarna har konfigurerats av administratören får de behörighet att få tillgång till alla AEM-resurser för att kunna utföra sitt arbete som medarbetare på att leverera upplevelseprogrammet med AEM Headless.

Administratören ska vara den användare som konfigurerar AEM och förbereder körningsmiljön så att [innehållsförfattare](#content-author) kan skapa och uppdatera innehåll, och [utvecklare](#developer) kan använda API:er som hämtar eller ändrar innehåll för sina upplevelseprogram.

#### Innehållsförfattare {#content-author}

Innehållsförfattare skapar och hanterar det material som levereras headless av AEM. Innehållsförfattare använder AEM-funktioner som Content Fragments och Assets Console för att hantera sitt innehåll.

Innehållsförfattare bör ha följande i åtanke:

#### Översättningsplan {#translation}

Planera för översättning i början av projektet. Se&quot;Översättningsspecialist&quot; som en separat person vars ansvar är att definiera vilket innehåll som ska översättas och vad som inte ska översättas, och vilket översatt innehåll som kan ändras av regionala eller lokala innehållsproducenter.

Skapa en plan för vilken innehållsöversättning du behöver.

* Behöver ni olika språk eller språk för att anpassa er till regionala särdrag?
* Behöver multimediematerial som bilder och videor vara olika för olika språkområden?

Var tydlig med arbetsflödet för innehållsuppdatering. Vilken godkännandeprocess måste systemet stödja? Kan AEM arbetsflöden användas för att automatisera denna process?

Observera att din [innehållshierarki](#content-hierarchy) kan användas för att göra översättningen enklare.

I avsnittet [ytterligare resurser](#additional-resources) finns mer information om AEM-arbetsflöden och översättningsverktyg, inklusive länkar till AEM Headless Translation Journey.

##### Utnyttja innehållshierarkin {#content-hierarchy}

Mapphierarkin kan hantera två viktiga problem när det gäller innehållshantering:

* [Översättning](#translation) - AEM hanterar översättning av innehåll genom att underhålla kopior av innehåll i språkspecifika mappar.
* Organisation - Mappar används för att definiera en innehållshierarki som krävs för översättningsbehov och för att logiskt hantera innehållsfragment.

AEM möjliggör en flexibel innehållsstruktur och en hierarki kan vara godtyckligt stor. Det är dock viktigt att komma ihåg att ändringar i mappstrukturen kan få oönskade konsekvenser för befintliga frågor som [är beroende av innehållssökvägen.](#developer) En väldefinierad hierarki som är tydligt angiven i förväg kan därför vara till hjälp för innehållsförfattarna.

Mappar kan även begränsas till att endast tillåta vissa typer av innehåll (baserat på modeller för innehållsfragment). Vi rekommenderar att du alltid uttryckligen anger vilka modeller som tillåts för alla mappar i hierarkin. Ange tillåtet innehåll för en viss mapp:

* Förhindrar att innehållsförfattare skapar innehåll som inte tillhör mappen.
* Optimerar processen för att skapa innehåll genom att filtrera de innehållstyper som tillåts i mappen under skapandet så att endast giltiga typer av innehåll visas.

Genom att skapa en lämplig innehållsstruktur blir det enklare att samordna redigering av headless-innehåll i olika kanaler för att maximera återanvändningen av innehåll. Genom att utnyttja innehåll i flera kanaler blir innehållsproduktionen effektivare och ändringshanteringen effektivare.

##### Upprätta praktiska namnkonventioner {#naming-conventions}

Namn på innehållsfragment måste vara beskrivande för innehållsförfattare. AEM hanterar på ett genomskinligt sätt flytning och/eller trunkering av de namn som används i ID:n på databasnivå. Därför bör de logiska namn som tillhandahålls av innehållsförfattarna alltid vara läsbara och representera innehållet.

* Felaktigt namn: `cta_btn_1`
* Bra namn: `Call To Action Button`

Mer information om namngivningskonventioner för AEM-sidor finns i avsnittet [ytterligare resurser](#additional-resources).

##### Öka inte innehållets kapsling {#content-nesting}

[Innehållsfragment](#content-fragments) används i AEM för att skapa rubrikfritt innehåll. AEM stöder upp till tio nivåer av innehållkapsling för innehållsfragment. Det är dock viktigt att komma ihåg att AEM måste tolka varje referens som definieras i det överordnade innehållsfragmentet iterativt och sedan kontrollera om det finns några underordnade referenser i alla jämställda. De här åtgärderna kan snabbt bli ett prestandaproblem.

Som en allmän tumregel får inte Content Fragment-referenser kapslas över fem nivåer.

#### Innehållsarkitekt {#content-architect}

Innehållsarkitekterna analyserar kraven för de data som måste levereras utan problem och definierar strukturen för dessa data. Dessa strukturer kallas [Content Fragment Models](#content-fragment-models) i AEM. Modeller för innehållsfragment används som bas för de innehållsfragment som innehållsförfattarna skapar.

Ett användbart sätt att definiera modeller för innehållsfragment är att skapa modeller som mappar till UX-komponenterna i de program som använder innehållet.

Eftersom innehållsförfattarna interagerar med modellerna kontinuerligt när de skapar nytt innehåll kan man genom att anpassa modellerna till användargränssnittet visualisera den digitala upplevelsen. Om du går ett steg längre kan du tilldela ikoner till de modeller för innehållsfragment som representerar UX-elementet så att författarna intuitivt kan välja rätt modell baserat på visuella tecken.

#### Developer {#developer}

Utvecklarna ansvarar för att sammanfoga det material som skapas direkt i AEM med konsumenten, som ofta kan vara ett SPA-program (single page application), progressiv webbapp (PWA), webshop eller någon annan tjänst utanför AEM.

GraphQL är&quot;limmet&quot; mellan AEM och konsumenterna av headless-innehåll. GraphQL är det språk som används för att fråga AEM om nödvändigt innehåll.

Utvecklare bör tänka på några grundläggande rekommendationer när de planerar sina frågor:

* Frågor ska inte förlita sig på en fast sökväg (`ByPath`) för att hämta innehållsfragment.
   * [Innehållsförfattare har fullständig kontroll över innehållets fragmenthierarki](#content-hierarchy) och kan göra ändringar som skulle bryta en sådan fråga.
   * Frågor ska i stället välja om innehållsfragmentmodellreferenser med dynamiska frågeparametrar ska filtrera resultaten för att generera önskad nyttolast.
* Använd alltid beständiga frågor i AEM för bästa resultat. Dessa diskuteras senare under resan.
* GraphQL är deklarativt och följer motto&quot;Fråga efter exakt det du behöver och få exakt det&quot;. Det innebär att du alltid undviker `select *`-typfrågor som du kan skapa i en relationsdatabas när du skapar GraphQL-frågor.

För en [vanlig headless-implementering med AEM ](#level-1) behöver utvecklaren ingen kodkunskap om AEM.

### Prestandakrav {#performance-requirements}

För att ett projekt ska lyckas måste prestanda beaktas innan något innehåll skapas.

Se till att ni förstår vad era användare/besökare förväntar sig och hur ni utformar dem. Ange servicenivåmål (SLO) och mät dem för att förstå om du uppfyller användarens förväntningar.

#### Trafikmönster {#traffic-patterns}

Att förstå trafik- och trafikmönster börjar med att samla det ni vet från det förflutna och sedan projicera till den förväntade tillväxten under de närmaste åren. Några av de viktigaste variablerna att tänka på:

* Hur många API-anrop per timme/dag/månad förväntar du dig och finns det risk för kryddor och säsongsvariation?
* Hur många olika innehållsförfattare finns det?
* Hur många innehållsförfattare tror du kommer att arbeta samtidigt?
* Hur ofta uppdateras innehållet?
* Hur många innehållsmodeller behövs?
* Hur många förekomster av modeller behövs?

#### Uppdateringsfrekvens {#update-frequency}

Olika delar av upplevelsen har ofta olika frekvens för innehållsuppdateringar. Att förstå detta är viktigt för att kunna finjustera CDN och cachekonfigurationer. Detta är också viktigt för [innehållsarkitekterna](#content-architects) när de utformar modeller för ditt innehåll. Fundera:

* Måste vissa typer av innehåll förfalla efter en viss period?
* Finns det element som är användarspecifika och därför inte kan cachas?

## What&#39;s Next {#what-is-next}

Nu när du är klar med den här delen av AEM Headless Developer Journey bör du:

* Förstå grunderna i AEM headless-funktioner.
* Lär dig grunderna för AEM headless-funktioner.
* Var medveten om AEM avancerade integrationsnivåer.
* Du kan definiera projektet utifrån dess omfång.

Du bör fortsätta din resa utan AEM genom att nästa gång granska dokumentet [Path to Your First Experience Using AEM Headless](path-to-first-experience.md) där du får lära dig hur du konfigurerar de verktyg som behövs och hur du börjar fundera på att modellera data i AEM.

## Ytterligare resurser {#additional-resources}

Vi rekommenderar att du går vidare till nästa del av den Headless-utvecklingsresan genom att granska dokumentet [Path to Your First Experience Using AEM Headless](path-to-first-experience.md), men följande är ytterligare, valfria resurser som gör en djupdykning i vissa koncept som nämns i det här dokumentet, men de behöver inte fortsätta på den headless-resan.

* [Headful and Headless in AEM](/help/sites-developing/headful-headless.md) - A complete talk of the headless integration levels available in AEM

* En [introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)

* [AEM Headless Translation Journey](/help/journey-headless/translation/overview.md) - Den här dokumentationsresan ger dig en bred förståelse för headless-teknik, hur AEM fungerar som headless-innehåll och hur du kan översätta det.

* [AEM Headless-självstudiekurser](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE) - Använd de här praktiska självstudiekurserna för att utforska hur du kan använda de olika alternativen för att leverera innehåll till headless-slutpunkter med AEM och välja det som passar dig bäst.
* [Headless Content Management Using GraphQL API:er](https://experienceleague.adobe.com/sv?Solution=Experience+Manager&amp;Solution=Experience+Manager+Sites&amp;Solution=Experience+Manager+Forms&amp;Solution=Experience+Manager+Screens&amp;launch=ExperienceManager-D-1-2020.1.headless#courses) - Följ den här kursen för en översikt över GraphQL API:t som implementerats i AEM. Autentisering via AdobeID krävs.
* [AEM Guides WKND - GraphQL](https://github.com/adobe/aem-guides-wknd-graphql) - Det här GitHub-projektet innehåller exempelprogram som framhäver AEM GraphQL API:er.
* [Authoring Concepts](/help/sites-authoring/author.md) - Teknisk dokumentation för redigeringsmiljön i AEM inklusive information om konfiguration för författarpublicering
* [Publicera sidor](/help/sites-authoring/publishing-pages.md) - teknisk dokumentation för publicering av innehåll på AEM
* [Namngivningskonventioner](/help/sites-developing/naming-conventions.md) - Teknisk dokumentation om sidnamnsbegränsningar i AEM
* [Multi Site Manager och Translation](/help/sites-administering/msm-and-translation.md) - teknisk dokumentation om AEM kraftfulla översättningsfunktioner
* [AEM-arbetsflöden](/help/sites-authoring/workflows.md) - teknisk dokumentation om hur du automatiserar arbetsflöden i AEM
* [Innehållsfragment](/help/assets/content-fragments/content-fragments.md) - teknisk dokumentation för innehållsfragment.
* [Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md) - teknisk dokumentation för modeller för innehållsfragment.
* [GraphQL tekniska dokumentation](https://graphql.org) - GraphQL-definitionen (extern länk)
* [GraphQL API](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md) - teknisk dokumentation som förklarar hur du skapar begäranden om åtkomst och leverans av innehållsfragment
* [Assets REST API](/help/assets/assets-api-content-fragments.md) - Teknisk dokumentation som förklarar hur du skapar och ändrar innehållsfragment (och andra resurser)
* [Beständiga frågor](/help/sites-developing/headless/graphql-api/persisted-queries.md) - Teknisk dokumentation för beständiga frågor i AEM
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)
