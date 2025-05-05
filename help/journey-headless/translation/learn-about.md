---
title: Läs om headless content och hur du översätter det i AEM
description: Lär dig headless concepts, how they map to AEM, and the theof AEM translation.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,Language Copy
role: Admin, Architect,Data Architect,Developer,User,Leader
exl-id: b81293da-772a-4ff1-8606-cec92d8cbd72
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '747'
ht-degree: 0%

---

# Läs om headless content och hur du översätter det i AEM {#learn-about}

Lär dig headless concepts, how they map to AEM, and the theof AEM translation.

## Syfte {#objective}

Det här dokumentet hjälper dig att förstå hur headless-innehåll levereras, hur AEM stöder headless och hur sådant innehåll kan översättas. När du har läst bör du:

* Förstå de grundläggande begreppen för leverans av headless-innehåll.
* Läs om hur AEM hanterar headless och översättning.

## Leverans av högklassigt innehåll {#full-stack}

Ända sedan de lättanvända, storskaliga CMS:erna (Content Management System) började användas har organisationer använt dem som en central plats för att hantera meddelanden, varumärken och kommunikation. Genom att använda CMS som en central punkt för att administrera upplevelser blir ni effektivare eftersom ni slipper utföra samma uppgifter i olika system.

![Den klassiska högklassiga CMS](/help/journey-headless/developer/assets/full-stack.png)

I en CMS i full hög finns all funktionalitet för att hantera innehåll i CMS. Systemfunktionerna består av olika komponenter i CMS-stacken. Lösningen i full hög har många fördelar.

* Det finns ett system att underhålla.
* Innehållet hanteras centralt.
* Alla tjänster i systemet är integrerade.
* Det går smidigt att skapa innehåll.

Om en ny kanal måste läggas till eller stöd för nya typer av upplevelser krävs, kan en (eller flera) ny komponent infogas i högen och det finns bara en plats att göra ändringar på.

![Lägger till en ny kanal i högen](/help/journey-headless/developer/assets/adding-channel.png)

Men komplexiteten i beroendena i högen blir snabbt uppenbar eftersom andra objekt i högen måste justeras för att passa ändringarna.

## Huvudet utan huvud {#the-head}

Huvudet för alla system är vanligtvis systemets utdatarenderare, vanligtvis i form av ett grafiskt gränssnitt eller andra grafiska utdata.

När vi talar om ett headless CMS hanterar CMS innehållet och fortsätter att leverera det till konsumenterna. Men om **content** endast levereras på ett standardiserat sätt utesluter ett headless CMS den slutliga utdatarenderingen, vilket innebär att **presentationen** av innehållet lämnas till den förbrukande tjänsten.

![Headless CMS](/help/journey-headless/developer/assets/headless-cms.png)

De konsumerande tjänsterna, oavsett om de är AR-upplevelser, en webbutik, mobilupplevelser, progressiva webbappar (PWA) och så vidare, tar in innehåll från det headless CMS och erbjuder sin egen rendering. De ser till att kunna erbjuda sina egna huvuden för ert innehåll.

Om du utelämnar huvudet förenklas CMS genom att komplexiteten försvinner. När du gör det flyttas även ansvaret för att återge innehållet till de tjänster som faktiskt behöver innehållet och som ofta är bättre lämpade för sådan återgivning.

## Översätta Headless-innehåll i AEM {#translating-in-aem}

Förutom kraftfulla verktyg för att skapa, hantera och leverera traditionella webbsidor i fullskärmsläge erbjuder AEM även möjligheten att skapa fristående innehållsval och leverera dem utan problem.

Med kraften i AEM kan man leverera material i antingen headlessform, full-stack eller i båda modellerna samtidigt. För översättningsexperten kan samma uppsättning översättningsverktyg användas för båda typerna av innehåll, vilket ger en enhetlig metod för översättning av ditt innehåll.

Under kundresan kommer du att lära dig mer om hur AEM översätter innehåll, men på en hög nivå är konceptet enkelt:

1. Definiera en anslutning till en översättningstjänst genom att konfigurera översättningsintegreringsramverket.
1. Definiera vilket innehåll som ska översättas med översättningsregler.
1. Skapa ett översättningsprojekt för att hämta innehållet, skicka det till översättningstjänsten och få resultaten.
1. Granska och publicera det översatta innehållet.

## What&#39;s Next {#what-is-next}

Tack för att du kom igång med din AEM headless-översättningsresa! Nu när du läser det här dokumentet bör du:

* Förstå de grundläggande begreppen för leverans av headless-innehåll.
* Läs om hur AEM hanterar headless och översättning.

Bygg vidare på den här kunskapen och fortsätt din arbetslösa översättningsresa med AEM genom att gå igenom dokumentet [Kom igång med AEM headless translation](getting-started.md) där du får en översikt över hur AEM hanterar headless-innehåll och lär dig dess översättningsverktyg.

## Ytterligare resurser {#additional-resources}

Vi rekommenderar att du går vidare till nästa del av den headless-översättningsresan genom att granska dokumentet [Kom igång med AEM headless-översättning](getting-started.md), men följande är ytterligare, valfria resurser som gör en djupdykning i vissa koncept som nämns i det här dokumentet, men de behöver inte fortsätta på den headless-resan.

* [MSM och översättning](/help/sites-administering/msm-and-translation.md) - Information om AEM Multi-Site Manager och hur det fungerar med översättningsverktygen
* En [introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)
* [Självstudiekurser för Headless i AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE)
