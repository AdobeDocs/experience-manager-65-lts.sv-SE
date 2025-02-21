---
title: Valfritt - Så här skapar du SPA-program (Single Page Applications) med Adobe Experience Manager
description: I den här valfria fortsättningen av Adobe Experience Manager (AEM) Headless Developer Journey får du lära dig hur AEM kan kombinera headless-leverans med traditionella CMS-funktioner i full hög och hur du kan skapa redigerbara SPA-program med AEM SPA Editor-ramverk.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments
role: Admin, Developer
source-git-commit: 71b7f46de3619605c8a5aedc496cfb85ed582f59
workflow-type: tm+mt
source-wordcount: '1248'
ht-degree: 0%

---

# Skapa SPA-program (Single Page Applications) med AEM {#create-spa}

I den här valfria fortsättningen av [AEM Headless Developer Journey](overview.md) får du lära dig hur Adobe Experience Manager (AEM) kan kombinera headless-leverans med traditionella CMS-funktioner i full hög och hur du kan skapa redigerbara SPA-program med AEM SPA Editor-ramverk samt integrera externa SPA-program, vilket möjliggör redigeringsfunktioner efter behov.

## Story hittills {#story-so-far}

Nu ska du ha slutfört hela [AEM Headless Developer Journey](overview.md) och förstå grunderna för headless-leverans i AEM, inklusive en förståelse för:

* Skillnaden mellan headless och headful content delivery.
* AEM headless-funktioner.
* Så här organiserar du och AEM Headless-projekt.
* Så här skapar du headless-innehåll i AEM.
* Så här hämtar och uppdaterar du headless-innehåll i AEM.
* Så här lever du i ett AEM Headless-projekt.

Så nu har du antingen gått live med ditt första AEM Headless-projekt eller har kunskapen att göra det. Grattis!

Varför läser du den här extra, valfria fortsatta resan? I [Getting Started](getting-started.md#integration-levels) diskuterades troligen inte bara hur AEM stöder headless-leverans och traditionella fullstacksmodeller, utan även hybridmodeller som kombinerar fördelarna med båda. Även om det inte är den traditionella headless-modellen kan sådana hybridmodeller ge oöverträffad flexibilitet till vissa projekt.

Den här artikeln bygger på dina kunskaper om AEM Headless genom att ingående utforska hur du kan skapa egna SPA-program (single page applications) som kan redigeras i AEM. På så sätt kan du skapa innehåll och skicka det direkt till en SPA, men SPA förblir redigerbart i AEM.

## Syfte {#objective}

Det här dokumentet hjälper dig att förstå hur Single Page-program utvecklas med AEM SPA Editor. När du har läst det här dokumentet bör du:

* Förstå SPA-redigerarens grundläggande funktioner.
* Ta reda på vad som krävs för att skapa en fullt redigerbar SPA för AEM.
* Förstå hur externa SPA kan integreras i AEM.
* Förstå hur återgivning på serversidan ska implementeras eller inte.

## Krav och krav {#requirements-prerequisites}

Det finns flera krav innan du börjar arbeta med SPA i AEM.

### Kunskap {#knowledge}

* Utvecklingserfarenhet som skapar SPA med React- eller Angular-ramverk
* AEM grundläggande kunskaper i att skapa innehållsfragment och använda redigeraren
* Granska dokumentet [Headful and Headless in AEM](/help/sites-developing/headful-headless.md) för att förstå de olika nivåerna av SPA-integrering som är möjliga.

### verktyg {#tools}

* Sandlådeåtkomst för testning av driftsättning av ditt projekt
* Lokal utvecklingsinstans för datamodellering och -testning
* Befintlig extern SPA (valfritt, beroende på vilken integrationsmodell som väljs)
* AEM Project Archetype

## Vad är en SPA? {#what-is-a-spa}

Ett SPA-program (single-page application) skiljer sig från en konventionell sida genom att det återges på klientsidan och i första hand är JavaScript-styrt, beroende på att Ajax anropar för att läsa in data och dynamiskt uppdatera sidan. Det mesta eller allt innehåll hämtas en gång på en sida, och ytterligare resurser läses in asynkront efter behov baserat på användarinteraktionen med sidan.

Detta minskar behovet av siduppdatering och ger användaren en upplevelse som är smidig, snabb och som känns mer som en appupplevelse.

Med AEM SPA Editor kan gränssnittsutvecklare skapa SPA som kan integreras i en AEM-webbplats, vilket gör att innehållsförfattarna kan redigera SPA-innehållet lika enkelt som annat AEM-innehåll.

## Varför en SPA? {#why-spa}

Genom att vara snabbare, smidigare och mer som ett systemspecifikt program blir en SPA en attraktiv upplevelse inte bara för besökaren på webbsidan, utan även för marknadsförare och utvecklare på grund av hur SPA fungerar.

En fullständig beskrivning av SPA och varför du skulle använda dem finns i avsnittet [ytterligare resurser](#additional-resources) för länkar till mer utförlig dokumentation.

## Hur AEM hanterar SPA

Utveckla single page-applikationer på AEM förutsätter att frontutvecklaren följer vedertagna standarder när han skapar en SPA. Om du som gränssnittsutvecklare följer dessa allmänna bästa metoder och några AEM-specifika principer kommer ditt SPA att fungera med AEM och dess funktioner för innehållsutveckling.

* **Portabilitet** - Precis som med andra komponenter bör SPA-komponenterna byggas så att de är så portabla som möjligt. SPA bör byggas med rörliga och återanvändbara komponenter.
* **AEM Drives Site Structure** - front-end-utvecklaren skapar komponenter och äger sin interna struktur, men använder AEM för att definiera webbplatsens innehållsstruktur.
* **Dynamisk återgivning** - All återgivning ska vara dynamisk.
* **Dynamisk routning** - SPA ansvarar för routningen och AEM lyssnar på den och hämtar baserat på den. Alla routningar ska också vara dynamiska.

En fullständig beskrivning av hur AEM hanterar SPA finns i avsnittet [ytterligare resurser](#additional-resources) för länkar till mer utförlig dokumentation.

## AEM SPA Editor {#aem-spa-editor}

Webbplatser som byggts med vanliga SPA-ramverk som React och Angular läser in sitt innehåll via dynamisk JSON och tillhandahåller inte den HTML-struktur som krävs för att AEM Page Editor ska kunna placera redigeringskontroller.

För att göra det möjligt att redigera SPA i AEM krävs en mappning mellan JSON-utdata för SPA och innehållsmodellen i AEM-databasen för att spara ändringar i innehållet.

Stöd för SPA i AEM introducerar ett tunt JS-lager som interagerar med SPA JS-koden när den läses in i sidredigeraren. Händelserna kan skickas och platsen för redigeringskontrollerna kan aktiveras för redigering i sitt sammanhang. Den här funktionen bygger på API-slutpunktskonceptet för innehållstjänster eftersom innehållet från SPA måste läsas in via innehållstjänster.

En fullständig beskrivning av AEM SPA Editor finns i avsnittet [ytterligare resurser](#additional-resources) för länkar till mer utförlig dokumentation.

## Anpassa befintliga SPA {#existing-spas}

Om du har ett befintligt SPA-program kan AEM bädda in det i AEM så att det blir synligt för dina innehållsförfattare i AEM Editor. Det här kan vara användbart om du vill visa innehållet som de skapar via innehållsfragment i slutprogrammet där det kommer att användas.

Med endast små ändringar kan du dessutom aktivera vissa redigeringsmöjligheter för den externa SPA-filen i AEM Editor.

RemotePage-komponenten tillåter återgivning av en extern SPA i AEM.

En fullständig beskrivning av hur du gör en extern SPA redigerbar i AEM finns i avsnittet [ytterligare resurser](#additional-resources) för länkar till mer ingående dokumentation.

## What&#39;s Next {#what-is-next}

Kom igång med att utveckla egna SPA för AEM genom att läsa följande dokument:

* [SPA WKND - självstudiekurs](/help/sites-developing/spa-wknd.md)
* [Komma igång med React](/help/sites-developing/spa-getting-started-react.md)
* [Komma igång med Angular](/help/sites-developing/spa-getting-started-angular.md)

Om du måste anpassa en befintlig SPA för att använda den i AEM kan du läsa följande dokument:

* [RemotePage-komponenten](/help/sites-developing/spa-remote-page.md)
* [Redigera en extern SPA i AEM](/help/sites-developing/spa-edit-external.md)

Nedan finns [ytterligare resurser](#additional-resources) som kan fördjupa dig i SPA-ämnen i AEM.

## Ytterligare resurser {#additional-resources}

Nedan följer ytterligare resurser som ger en djupdykning i några koncept som nämns i det här dokumentet.

* [Headful and Headless in AEM](/help/sites-developing/headful-headless.md) - A description of the different delivery models available in AEM
* [SPA-introduktion och genomgång.](/help/sites-developing/spa-walkthrough.md) - En bra introduktion till SPA i AEM
* [Utveckla SPA för AEM](/help/sites-developing/spa-architecture.md) - Riktlinjer för hur du utvecklar SPA för AEM
* [Översikt över SPA-redigeraren](/help/sites-developing/spa-overview.md) - Information om hur SPA-redigeraren fungerar
* [SPA-referensdokument](/help/sites-developing/spa-reference-materials.md) - JavaScript API-referenser och länkar till AEM SPA GitHub-projekt med öppen källkod
* [Innehållsfragment](/help/assets/content-fragments/content-fragments.md) - Skapa innehållsfragment
* [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) - Maven-mall som skapar ett minimalt, metodbaserat Adobe Experience Manager-projekt (AEM) som utgångspunkt för din webbplats
