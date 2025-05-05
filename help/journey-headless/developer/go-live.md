---
title: Så här Live med ditt headless-program
description: I den här delen av AEM Headless Developer Journey får du lära dig hur du driftsätter en headless-applikation live.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin, Developer
exl-id: 8837e7cd-c949-46cc-9c39-3c7a82cc1daf
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1803'
ht-degree: 0%

---

# Så här Live med ditt headless-program {#go-live}

I den här delen av [AEM Headless Developer Journey](overview.md) får du lära dig hur du distribuerar ett headless-program live.

## Story hittills {#story-so-far}

I det föregående dokumentet om AEM resa [How to Update Your Content via AEM Assets APIs](update-your-content.md) lärde du dig att uppdatera ditt befintliga headless-innehåll i AEM via API, och du bör nu:

* Förstå AEM Assets HTTP API.

Den här artikeln bygger vidare på dessa grundprinciper så att du förstår hur du förbereder ett eget headless-projekt för AEM att publicera.

## Syfte {#objective}

Det här dokumentet hjälper dig att förstå AEM headless Publishing pipeline och de prestandaöverväganden du måste vara medveten om innan du publicerar programmet.

* Läs mer om AEM SDK och de utvecklingsverktyg som krävs
* Konfigurera en lokal utvecklingsmiljö för att simulera ditt innehåll innan du publicerar det
* Förstå AEM Content Replication och Caching Basics
* Säkra och skala programmet före start
* Övervaka prestanda och felsökning

## AEM SDK {#the-aem-sdk}

AEM SDK används för att skapa och distribuera anpassad kod. Det är huvudverktyget som du måste utveckla och testa din headless-applikation innan du kan börja använda den. Den innehåller följande artefakter:

* Quickstart jar - en körbar jar-fil som kan användas för att ställa in både författare och publiceringsinstans
* Dispatcher-verktyg - Dispatcher-modulen och dess beroenden för Windows och UNIX-baserade system
* Java™ API Jar - Java™ Jar/Maven Dependency som visar alla Java™-API:er som kan användas för att utveckla mot AEM
* Javadoc jar - javadocs for the Java™ API jar

## Ytterligare utvecklingsverktyg {#additional-development-tools}

Förutom AEM SDK behöver du ytterligare verktyg som gör det lättare att utveckla och testa kod och innehåll lokalt:

* Java™
* Git
* Apache Maven
* The Node.js library
* Den utvecklingsmiljö du vill använda

Eftersom AEM är ett Java™-program måste du installera Java™ och Java™ SDK för att stödja utvecklingen av AEM as a Cloud Service.

Git är det ni använder för att hantera källkontroll och för att checka in ändringarna i Cloud Manager och sedan distribuera dem till en produktionsinstans.

AEM använder Apache Maven för att bygga projekt som genererats från AEM Maven Project Archetype. Alla större utvecklingsmiljöer har stöd för integrering av Maven.

Node.js är en JavaScript-körningsmiljö som används för att arbeta med de främsta resurserna i ett AEM-projekts `ui.frontend`-underprojekt. Node.js distribueras med npm, som är de facto-pakethanteraren Node.js, som används för att hantera JavaScript-beroenden.

## Lathund för komponenter i ett AEM-system {#components-of-an-aem-system-at-a-glance}

Nu ska vi titta närmare på de olika delarna av en AEM-miljö.

En komplett AEM-miljö består av en författare, en publiceringsversion och en Dispatcher. Samma komponenter är tillgängliga i den lokala utvecklingsmiljön för att göra det enklare för dig att förhandsgranska kod och innehåll innan du publicerar.

* **Författartjänsten** är den plats där interna användare skapar, hanterar och förhandsgranskar innehåll.

* **Publiceringstjänsten** betraktas som Live-miljön och är vanligtvis den slutanvändare som interagerar med. När innehållet har redigerats och godkänts av författartjänsten distribueras det (replikeras) till publiceringstjänsten. Det vanligaste distributionsmönstret med AEM headless-program är att ha produktionsversionen av programmet ansluten till en AEM Publish-tjänst.

* **Dispatcher** är en statisk webbserver som utökas med modulen AEM Dispatcher. Den cachelagrar webbsidor som skapats av publiceringsinstansen för att förbättra prestandan.

## Arbetsflödet för lokal utveckling {#the-local-development-workflow}

Det lokala utvecklingsprojektet bygger på Apache Maven och använder Git för källkontroll. För att uppdatera projektet kan utvecklarna använda den integrerade utvecklingsmiljö de föredrar, till exempel Eclipse, Visual Studio Code eller IntelliJ.

Distribuera uppdateringarna till den lokala AEM-miljön för att testa kod- eller innehållsuppdateringar som är inkapslade av ditt headless-program. Det gäller lokala instanser av AEM författare och publiceringstjänster.

Observera skillnaden mellan de olika komponenterna i den lokala AEM-miljön, eftersom det är viktigt att testa uppdateringarna där de är som viktigast. Testa till exempel innehållsuppdateringar på författaren eller testa ny kod på publiceringsinstansen.

I ett produktionssystem placeras en Dispatcher- och en http Apache-server alltid framför en publiceringsinstans i AEM. De tillhandahåller cachelagring och säkerhetstjänster för AEM-systemet, så det är oerhört viktigt att testa kod- och innehållsuppdateringar mot Dispatcher också.

## Förhandsgranska kod och innehåll lokalt med den lokala utvecklingsmiljön {#previewing-your-code-and-content-locally-with-the-local-development-environment}

Förbered AEM headless-projekt för lansering genom att se till att alla projektdelar fungerar som de ska.

För att göra det måste ni sätta ihop allt: kod, innehåll och konfiguration, och testa det i en lokal utvecklingsmiljö för att kunna vara redo att användas live.

Den lokala utvecklingsmiljön består av tre huvudområden:

1. AEM Project - innehåller all kod, konfiguration och innehåll som AEM-utvecklare kommer att arbeta med
1. Local AEM Runtime - lokala versioner av AEM-författaren och publiceringstjänster som används för att distribuera kod från AEM-projektet
1. Den lokala Dispatcher-miljön - en lokal version av Apache htttpd-webbservern som innehåller Dispatcher-modulen

När den lokala utvecklingsmiljön har konfigurerats kan du simulera innehåll som skickas till React-appen genom att distribuera en statisk nodserver lokalt.

Mer information om hur du konfigurerar en lokal utvecklingsmiljö och alla beroenden som behövs för innehållsförhandsgranskning finns i [Dokumentation för produktionsdistribution](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/overview.html?lang=sv-SE).

## Förbered AEM Headless Application for GoLive {#prepare-your-aem-headless-application-for-golive}

<!-- Start of CDN Review -->

Nu är det dags att göra AEM headless-applikationen klar för lansering enligt de rutiner som beskrivs nedan.

### Skydda ditt Headless-program innan du startar {#secure-and-scale-before-launch}

1. Förbered [autentiseringen](/help/sites-developing/headless/graphql-api/graphql-authentication-content-fragments.md) för dina GraphQL-begäranden

### Modellstruktur jämfört med GraphQL Output {#structure-vs-output}

* Undvik att skapa frågor som genererar mer än 15 kB JSON (gzip-komprimerad). Långa JSON-filer är resurskrävande för att klientprogrammet ska kunna analysera.
* Undvik fler än fem kapslade nivåer av fragmenthierarkier. Ytterligare nivåer gör det svårt för innehållsförfattare att ta hänsyn till effekten av deras ändringar.
* Använd frågor med flera objekt i stället för att modellera frågor med beroendehierarkier i modellerna. På så sätt blir det flexiblare på lång sikt att strukturera om JSON-utdata utan att behöva göra många innehållsändringar.

### Maximera CDN-cacheträffrekvens {#maximize-cdn}

* Använd inte direkta GraphQL-frågor, såvida du inte begär direktinnehåll från ytan.
   * Använd beständiga frågor när det är möjligt.
   * Tillhandahåll CDN TTL över 600 sekunder så att CDN kan cachelagra dem.
   * AEM kan beräkna effekten av en modelländring av befintliga frågor.
* Dela JSON-filer/GraphQL-frågor mellan låg och hög förändringsfrekvens för innehåll för att minska klienttrafiken till CDN och tilldela högre TTL. Om du gör det minimeras antalet CDN som validerar JSON med den ursprungliga servern.
* Om du vill göra innehåll från CDN ogiltigt använder du Mjuk borttagning. På så sätt kan CDN ladda ned innehållet på nytt utan att orsaka ett cacheminne.

>[!NOTE]
>
>Mer information om CDN och cachelagring finns i [Ytterligare resurser](#additional-resources).

### Förkorta nedladdningstiden för headless Content {#improve-download-time}

* Kontrollera att HTTP-klienter använder HTTP/2.
* Kontrollera att HTTP-klienter accepterar rubrikbegäran för gzip.
* Minimera antalet domäner som används som värd för JSON och refererade artefakter.
* Använd `Last-modified-since` för att uppdatera resurser.
* Använd `_reference`-utdata i JSON-filen för att börja hämta resurser utan att behöva tolka fullständiga JSON-filer.

<!-- End of CDN Review -->

## Distribuera till produktion {#deploy-to-production}

Distributionen till Production kan vara beroende av om du har en *traditionell* AEM-instans som distribuerar med Maven, eller om du använder Adobe Managed Services (AMS) och därför använder Cloud Manager.

## Distribuera till produktion med Maven {#deploy-to-production-maven}

En *traditionell*-distribution (ej AMS) med Maven finns i [WKND-självstudiekursen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html?lang=sv-SE#build) för en översikt.

## Distribuera till produktion med Cloud Manager {#deploy-to-production-cloud-manager}

Om du är AMS-kund och använder Cloud Manager kan du, efter att du har kontrollerat att allt fungerar som det ska, överföra dina koduppdateringar till en [centraliserad Git-databas i Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/managing-code/git-integration.html?lang=sv-SE).

När uppdateringarna har överförts till Cloud Manager distribuerar du dem till AEM med [Cloud Manager CI/CD-pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=sv-SE).

<!-- Cannot find a parallel link -->
<!--
You can start deploying your code by using the Cloud Manager CI/CD pipeline, which is covered extensively - see the [Overview](/help/implementing/deploying/overview.md) to start.
-->

## Prestandaövervakning {#performance-monitoring}

För att användarna ska få bästa möjliga upplevelse när de använder AEM headless är det viktigt att du övervakar nyckeltal enligt beskrivningen nedan:

* Validera förhandsgransknings- och produktionsversionerna av appen
* Verifiera AEM statussidor för aktuell status för tillgänglighet
* Få resultatrapporter
   * Leveransprestanda
      * Ursprungsservrar - antal anrop, felfrekvens, CPU-belastning, nyttolaststrafik
   * Författarprestanda
      * Kontrollera antal användare, förfrågningar och inläsning
* Åtkomst till program- och utrymmesspecifika prestandarapporter
   * Kontrollera om de allmänna måtten är gröna/orange/röda när servern är på plats och identifiera sedan specifika appproblem
   * Öppna samma rapporter ovan filtrerade till program eller utrymme (t.ex. Photoshop desktop, paywall)
   * Använd API:er för Splunk-logg för att få åtkomst till service- och programprestanda
   * Kontakta kundsupport om det finns andra problem.

## Felsökning {#troubleshooting}

### Felsökning {#debugging}

Följ dessa metodtips som ett allmänt tillvägagångssätt vid felsökning:

* Validera funktionalitet och prestanda med förhandsgranskningsversionen av programmet
* Validera funktionalitet och prestanda med programmets produktionsversion
* Validera med JSON-förhandsvisningen i Content Fragment Editor
* Om du vill kontrollera om det finns problem med klientprogram eller leverans kontrollerar du JSON i klientprogrammet
* Om du vill kontrollera om det finns problem med cachelagrat innehåll eller AEM kontrollerar du JSON med GraphQL

### Logga ett fel med support {#logging-a-bug-with-support}

Om du behöver mer hjälp kan du logga ett fel med Support på ett effektivt sätt:

* Ta skärmbilder av problemet, om det behövs
* Dokumentera ett sätt att återskapa problemet
* Dokumentera innehållet som problemet återger med
* Logga ett problem via AEM supportportal med rätt prioritet

## Resan slutar - eller gör det? {#journey-ends}

Grattis! Du har slutfört AEM Headless Developer Journey! Nu bör du förstå:

* Skillnaden mellan headless och headful content delivery.
* AEM headless-funktioner.
* Så här organiserar du och AEM Headless-projekt.
* Så här skapar du headless-innehåll i AEM.
* Så här hämtar och uppdaterar du headless-innehåll i AEM.
* Så här lever du i ett AEM Headless-projekt.
* Vad du ska göra när du är klar.

Antingen har du redan startat ditt första AEM Headless-projekt eller så har du alla kunskaper du behöver för att göra det. Snyggt jobb!

### Utforska Single Page-program {#explore-spa}

Men du behöver inte stoppa de headless butikerna i AEM. I [Getting Started-delen av kundresan](getting-started.md#integration-levels) diskuterades hur AEM inte bara stöder headless-leverans och traditionella helstacksmodeller, utan även hybridmodeller som kombinerar fördelarna med båda.

Om den här typen av flexibilitet är något du behöver för ditt projekt kan du fortsätta med den valfria ytterligare delen av resan, [Så här skapar du SPA-program (Single Page Applications) med AEM.](create-spa.md)

## Ytterligare resurser {#additional-resources}

* [Utvecklingshandbok för AEM](https://experienceleague.adobe.com/docs/experience-manager-65-lts/developing/introduction/the-basics.html)

* [WKND, självstudiekurs](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=sv-SE)

* [Cloud Manager för AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=sv-SE)

* CDN-cache

   * [Kontrollera en CDN-cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=sv-SE#controlling-a-cdn-cache)

   * Konfigurerar [CDN Rewriter](https://experienceleague.adobe.com/docs/experience-manager-65-lts/deploying/configuring/osgi-configuration-settings.html) (*sök efter CDN Rewriter*)

* [Introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)
* [Självstudiekurser för Headless i AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE)
