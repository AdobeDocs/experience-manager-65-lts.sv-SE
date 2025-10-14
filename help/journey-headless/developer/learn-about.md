---
title: Läs om CMS Headless Development
description: I den här delen av AEM Headless Developer Journey kan du lära dig mer om headless-teknik och varför du skulle använda den.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin, Developer
exl-id: 39e05b80-a11b-41ff-8157-dc74f071c514
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1601'
ht-degree: 0%

---

# Läs om CMS Headless Development {#learn-about}

I den här delen av [AEM Headless Developer Journey &#x200B;](overview.md) får du lära dig mer om headless-teknik och varför du skulle använda den.

## Syfte {#objective}

Det här dokumentet hjälper dig att förstå hur headless-innehåll levereras och varför det ska användas. När du har läst bör du:

* Förstå de grundläggande begreppen och terminologin för leverans av headless-innehåll
* Förstå varför och när headless krävs
* Lär dig på en hög nivå hur headless-koncept används och hur de fungerar ihop

## Leverans av högklassigt innehåll {#full-stack}

Ända sedan de lättanvända, storskaliga CMS:erna (Content Management System) började användas har organisationer använt dem som en central plats för att hantera meddelanden, varumärken och kommunikation. Genom att använda CMS som en central punkt för att administrera upplevelser blir ni effektivare eftersom ni slipper utföra samma uppgifter i olika system.

![Den klassiska högklassiga CMS](assets/full-stack.png)

I en CMS i full hög finns all funktionalitet för att hantera ditt innehåll i CMS. Systemfunktionerna består av olika komponenter i CMS-stacken. Lösningen i full hög har många fördelar.

* Du har ett system att underhålla.
* Innehållet hanteras centralt.
* Alla tjänster i systemet är integrerade.
* Det går smidigt att skapa innehåll.

Om du vill lägga till en ny kanal eller stödja nya typer av upplevelser kan du infoga en (eller flera) ny komponent i högen och bara ha en plats att göra ändringarna på.

![Lägger till en ny kanal i högen](assets/adding-channel.png)

Komplexiteten i beroendena i högen blir snabbt tydlig eftersom du ser att andra objekt i högen kan behöva justeras för att kunna anpassa ändringarna.

## Gränser för leverans i fullhög {#limits}

Det helstatiska arbetssättet skapar en silo där alla upplevelser finns i ett system. Ändringar eller tillägg i en komponent i silon kräver ändringar i andra komponenter, vilket kan göra ändringar som är tidskrävande och kostsamma.

Detta gäller särskilt presentationssystemet, som i traditionell form ofta är starkt knutet till CMS. Alla nya kanaler innebär vanligtvis en uppdatering av presentationssystemet, vilket kan påverka alla andra kanaler.

![Komplexiteten ökar när kanaler läggs till i en hög](assets/presentation-complexity.png)

Begränsningarna i den här naturliga silon kan bli tydligare när du lägger mer kraft på att koordinera ändringar i alla komponenter i högen.

Användarna förväntar sig engagemang oavsett plattform eller kontaktyta och behöver flexibilitet i hur ni levererar era upplevelser.  Detta flerkanalsarbetssätt är standarden för digitala upplevelser och en helhetsstrategi kan under vissa omständigheter visa sig vara stelbent.

## Huvudet utan huvud {#the-head}

Huvudet för alla system är vanligtvis systemets utdatarenderare, vanligtvis i form av ett grafiskt gränssnitt eller andra grafiska utdata.

En headless-server sitter till exempel förmodligen i ett rack i ett serverrum någonstans och har ingen övervakare ansluten. För att få tillgång till den måste du fjärransluta till den. I det här fallet är monitorn huvudet när den tar hand om återgivningen av serverns utdata. Du som kund för tjänsten kan ge dig ett eget huvud (bildskärmen) när du fjärransluter till den.

När vi talar om ett headless CMS hanterar CMS innehållet och fortsätter att leverera det till konsumenterna. Men om **content** endast levereras på ett standardiserat sätt utesluter ett headless CMS den slutliga utdatarenderingen, vilket innebär att **presentationen** av innehållet lämnas till den förbrukande tjänsten.

![Headless CMS](assets/headless-cms.png)

De konsumerande tjänsterna, oavsett om de är AR-upplevelser, en webshop, mobilupplevelser, progressiva webbappar (PWA) och så vidare, tar in innehåll från det headless CMS och erbjuder sin egen rendering. De ser till att kunna erbjuda sina egna huvuden för ert innehåll.

Om du utelämnar huvudet förenklas CMS genom att komplexiteten försvinner. När du gör det flyttas även ansvaret för att återge innehållet till de tjänster som faktiskt behöver innehållet och som ofta är bättre lämpade för sådan återgivning.

## Frikoppling {#decoupling}

Headless-leverans är möjlig genom att en uppsättning robusta och flexibla API:er (Application Programming Interface) som alla era upplevelser kan välja mellan visas. API fungerar som ett gemensamt språk mellan tjänsterna och binder ihop dem på innehållsnivå genom standardiserad innehållsleverans, men ger dem flexibilitet att implementera sina egna lösningar.

Headless är ett exempel på hur du frigör innehåll från presentationen. Eller i mer generiska avseenden: koppla loss den främre änden från den bakre änden av servicestacken. I en headlessmiljö är presentationssystemet (huvudet) fristående från innehållshanteringen (svansen). De två interagerar bara via API-anrop.

Den här frikopplingen innebär att varje konsumtionstjänst (frontend) kan bygga sin upplevelse baserat på samma innehåll som levereras via API:erna, vilket säkerställer återanvändning och enhetlighet. Genom att använda tjänster kan de sedan implementera sina egna presentationssystem, vilket gör att innehållshanteringsstacken (back end-komponenten) enkelt kan skalas vågrätt.

## Teknisk bakgrund {#technology}

Med ett headless-tillvägagångssätt kan ni bygga en tekniklösning som enkelt och snabbt kan anpassas till framtida krav på digitala upplevelser.

Tidigare var API:er för CMS:er vanligtvis REST-baserade. REST (Representational State transfer) tillhandahåller resurser som text på ett statslöst sätt. Detta gör att resurserna kan läsas och ändras med en fördefinierad uppsättning åtgärder. REST möjliggör stor interoperabilitet mellan tjänster på webben genom att säkerställa en tillståndslös representation av innehållet.

Det finns fortfarande ett behov av stabila REST API:er. REST-begäranden kan dock vara stora och detaljerade. Om ni har flera konsumenter som gör REST-anrop för alla era kanaler kan detta påverka deras mångsidighet och prestanda.

Huvudlös innehållsleverans använder ofta GraphQL API:er. GraphQL tillåter en liknande tillståndslös överföring, men ger fler riktade frågor, minskar det totala antalet frågor som krävs och förbättrar prestanda. Det är vanligt att lösningar blandas med REST och GraphQL, vilket i huvudsak är det bästa verktyget för det aktuella arbetet.

Oavsett vilket API du väljer kan du, genom att definiera ett headless-system baserat på gemensamma API:er, använda den senaste webbläsaren och andra webbtekniker som progressiva webbappar (PWA). API:er skapar ett standardgränssnitt som är enkelt att utöka och anpassa.

Normalt återges innehållet på klientsidan. Detta innebär normalt att någon anropar ditt innehåll på en mobil enhet, att ditt CMS levererar innehållet och att den mobila enheten (klienten) sedan ansvarar för återgivningen av det innehåll som du betjänade. Om enheten är gammal eller långsam på något annat sätt är den digitala upplevelsen också långsam.

Att frigöra innehåll från presentation innebär att det kan finnas mer kontroll över sådana prestandaproblem på klientsidan. SSR-återgivning (Server-side rendering) överför ansvaret för återgivningen av innehållet från klientens webbläsare till servern. På så sätt kan ni som innehållsleverantör erbjuda en garanterad prestandanivå för er målgrupp om det är vad som krävs.

## Organisationsutmaningar {#organization}

Headless öppnar upp en värld av flexibilitet för att leverera digitala upplevelser. Men denna flexibilitet kan också utgöra en utmaning.

Att ha många olika kanaler kan innebära att de har sina egna presentationssystem. Även om de alla använder samma innehåll via samma API:er kan upplevelsen vara annorlunda tack vare de olika presentationerna. Man måste bekymra sig om och se till att kundupplevelsen är enhetlig.

Genom att implementera noggranna designsystem, dela mönsterbibliotek och använda återanvändbara designkomponenter och etablerade, öppna ramverk på klientsidan kan enhetliga upplevelser säkerställas, men detta måste planeras.

## Framtiden är Headless och framtiden är nu {#future}

Digitala upplevelser fortsätter att definiera hur varumärken interagerar med kunderna. Det som är spännande med headless-design är den flexibilitet som den ger oss att svara på kundernas föränderliga förväntningar.

Det är omöjligt att förutsäga framtiden, men utan headless ger er möjlighet att reagera på vad framtiden kan föra med sig.

## AEM och Headless {#aem-and-headless}

Under utvecklarresan kommer du att lära dig hur AEM stöder headless-leverans tillsammans med sina leveransmöjligheter i fullstacksformat.

Adobe är ledande inom digital upplevelsehantering och inser att den idealiska lösningen på verkliga utmaningar som upplevelseskapare står inför sällan är ett binärt val. Därför stöder AEM inte bara båda modellerna, utan också den sömlösa kombinationen av de två, som kombinerar fördelarna med headless och full stack, så att ni kan hjälpa era kunder på bästa sätt, oavsett var de befinner sig.

Den här resan fokuserar på den headless-baserade modellen för innehållsleverans. Men när ni väl har fått den här grundläggande kunskapen kan ni utforska hur ni kan använda båda modellernas kraftfulla funktioner.

## What&#39;s Next {#what-is-next}

Tack för att du kommer igång med din AEM resa utan trassel! Nu när du läser det här dokumentet bör du:

* Förstå de grundläggande begreppen och terminologin för leverans av headless-innehåll.
* Förstå varför och när headless behövs.
* Lär dig på en hög nivå hur headless-koncept används och hur de hänger ihop.

Bygg vidare på den här kunskapen och fortsätt din AEM resa utan att förlora resurser genom att gå igenom dokumentet [Komma igång med AEM Headless](getting-started.md) där du får lära dig hur du konfigurerar de verktyg som behövs och hur du börjar fundera på hur AEM tar sig an leverans av headless-innehåll och dess förutsättningar.

## Ytterligare resurser {#additional-resources}

Vi rekommenderar att du går vidare till nästa del av den headless-utvecklingsresan genom att granska dokumentet [Komma igång med AEM Headless](getting-started.md), men följande är ytterligare, valfria resurser som gör en djupdykning i vissa koncept som nämns i det här dokumentet, men de behöver inte fortsätta den headless-resan.

* En [introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)
* [AEM Headless-självstudiekurser](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE) - Använd de här praktiska självstudiekurserna för att utforska hur du kan använda de olika alternativen för att leverera innehåll till headless-slutpunkter med AEM och välja det som passar dig bäst.
