---
title: Utvecklingspraxis
description: Bästa tillvägagångssätt för att utveckla på Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 0%

---

# Utvecklingspraxis{#development-practices}

## Arbeta enligt definition av Klar (DoD) {#work-according-to-a-definition-of-done}

Varje team har en egen definition av vad&quot;gjort&quot; innebär, men det är viktigt att ha en sådan och se till att en berättelse uppfyller de definierade kriterierna innan den godkänns.

Några kriterier som vanligtvis anges av team är:

* Kod granskad för formatering
* Kommentarer/Javadoc har lagts till
* Uppfyller obligatoriska testtäckningsnivåer
* Pass-enhets- och integrationstester
* Validerad i QA-miljön
* Lokalisering implementerad

Utan en väldefinierad DoD är det lätt att hamna i en situation där mycket görs halvvägs och inget är helt färdigt.

### Definiera och följ kodnings- och formateringskonventioner {#define-and-adhere-to-coding-and-formatting-conventions}

Det kanske inte verkar viktigt med indragsnivåer och tomt utrymme, men att ha rätt formaterad kod går långt i fråga om läsbarhet och underhåll. Konventioner bör diskuteras och godkännas som ett team och sedan följas i koden.

### Syfte med hög testtäckning  {#aim-for-high-test-coverage}

När en projektimplementering växer i storlek ökar också den tid som krävs för att testa den. Utan god testtäckning kan testteamet inte skalas om och utvecklarna blir till slut begravda i buggar.

Utvecklare bör öva på testdriven utveckling (TDD) och skriva felaktiga enhetstester innan den produktionskod som uppfyller deras krav. Kvalitetssäkring bör skapa en automatiserad uppsättning godkännandetester för att säkerställa att systemet fungerar som väntat på en hög nivå.

Det finns anpassade ramverk, till exempel Jackalope och Prosper, som gör det enklare för utvecklare att maska av JCR-API:er när de skriver enhetstester.

### Håll dig redo för demo {#stay-demo-ready}

Systemet bör vara tillgängligt för demonstration till företaget i slutet av varje upprepning. Genom att hålla systemet i ett demo-färdigt tillstånd kommer teamet alltid att vara i ett skick där det är produktionsfärdigt och tekniska skulder kan hållas på en stabil nivå.

### Implementera en kontinuerlig integreringsmiljö och använd den {#implement-a-continuous-integration-environment-and-use-it}

Genom att implementera en kontinuerlig integreringsmiljö kan du enkelt och upprepade gånger köra enhetstester och integrationstester. Det skiljer också åt driftsättningen från utvecklingsteamet, vilket gör att andra delar av teamet kan bli mer effektiva och gör driftsättningen mer stabil och förutsägbar.

### Håll utvecklingscykeln snabb genom att hålla byggtiderna låga {#keep-the-development-cycle-fast-by-keeping-build-times-low}

Om enhetstester tar lång tid att köra undviker utvecklarna att köra dem och de förlorar sitt värde. Om det tar lång tid att skapa koden och distribuera den gör man det mindre ofta. Att göra korta byggtider till en prioritet säkerställer att den tid du har investerat i testtäckning och CI-infrastruktur fortsätter att göra teamet mer produktivt.

### Finjustera Sonar och andra verktyg för statisk kodanalys och agera utifrån deras rapporter {#fine-tune-sonar-and-other-static-code-analysis-tools-and-act-on-their-reports}

Verktyg för kodanalys kan vara värdefulla, men bara om deras rapporter leder till åtgärder från utvecklingsteamets sida. Utan att finjustera den analys som dessa verktyg ger blir rekommendationerna att de genererar irrelevanta och förlorar sitt värde.

### Följ filmklippsregeln Boy Scout {#follow-the-boy-scout-rule}

Pojkscouterna har en regel: &quot;Låt det vara bättre än du hittade det.&quot; Så länge alla medlemmar i utvecklingsteamet följer den här regeln och lagar något när de stöter på en enda röra, kommer koden hela tiden att förbättras.

### Undvik att implementera YAGNI-funktioner {#avoid-implementing-yagni-features}

YAGNI-funktioner (du kommer inte att behöva det) är saker som implementeras när vi förväntar oss att vi kommer att behöva något i framtiden, även om vi inte behöver det nu. Vi bör implementera det enklaste som kommer att fungera idag och använda kontinuerlig omfaktorisering för att säkerställa att systemarkitekturen utvecklas med de krav som ställs över tid. Detta gör att vi kan fokusera på det som är viktigt och förhindra att koden blottar och krypar funktioner.
