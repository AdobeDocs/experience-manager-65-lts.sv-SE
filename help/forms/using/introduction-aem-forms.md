---
title: Introduktion till AEM Forms
description: Använd den här AEM 6.5 LTS-användarhandboken för att skapa, hantera, publicera och uppdatera digitala formulär. Hitta hjälp med att installera, uppgradera och konfigurera dem och lär dig mer om att skapa adaptiva formulär.
feature: Adaptive Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: e9549ac9-0ada-4661-969a-709f0ed3b133
source-git-commit: d9f6415aabcbc55af23824d9e0ab33c418f6761f
workflow-type: tm+mt
source-wordcount: '950'
ht-degree: 3%

---

# Introduktion till AEM Forms{#introduction-to-aem-forms}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/forms-overview/home.html) |
| AEM 6.5 | Den här artikeln |

Information om de senaste funktionerna och förbättringarna i AEM Forms finns i [Nyheter i AEM Forms](../../forms/using/whats-new.md).

## Om AEM Forms {#about-aem-forms}

Adobe Experience Manager (AEM) är en lättanvänd lösning för att skapa, hantera, publicera och uppdatera komplexa digitala formulär och samtidigt integrera med back-end-processer, affärsregler och data.

AEM Forms kombinerar framtagning, hantering och publicering av blanketter med funktioner för korrespondenshantering, dokumentsäkerhet och integrerad analys för att skapa engagerande helhetsupplevelser. AEM Forms är utformat för att fungera i både webb- och mobilkanaler och kan effektivt integreras i era affärsprocesser, minska antalet pappersprocesser och fel samtidigt som effektiviteten förbättras.

I stora företag skapas formulär ofta en gång och återanvänds genom att man kopierar till ett innehållshanteringssystem. Det kan vara en stor utmaning att hålla en stor databas med blanketter uppdaterad och kunna upptäckas. AEM har en anpassningsbar Forms Portal som ser till att kunderna hittar och har tillgång till de formulär de behöver via både webben och mobila kanaler.

AEM Forms har verktyg för blanketthantering som inte bara gör det möjligt att hantera adaptiva formulär, utan även XFA-formulär, PDF forms och relaterade resurser. Mer information finns i [Introduktion till formulärhantering](../../forms/using/introduction-managing-forms.md).

>[!NOTE]
>
>Den adaptiva Forms-funktionen, som finns i [AEM 6.5 QuickStart](/help/sites-deploying/deploy.md), är endast avsedd för utforsknings- och utvärderingsändamål. För produktion krävs en giltig licens för AEM Forms, eftersom Adaptive Forms-funktionaliteten kräver rätt licensiering.

![AEM-formulärfunktioner](do-not-localize/4th-draft-updated.gif)

### Viktiga funktioner {#key-capabilities}

Sammanfattningsvis har AEM Forms kraftfulla funktioner för blanketthantering, som följande, som minskar antalet manuella processer och ger nöjdare kunder.

* En central Forms-portal för utformning och driftsättning av dynamiska blanketter, som PDF, HTML5 och adaptiva blanketter
* Ett lättanvänt grafiskt användargränssnitt med vilket man enkelt kan importera, hantera, förhandsgranska och publicera blanketter
* En responsiv formulärkatalog med kraftfulla sökfunktioner med nyckelord, taggar och metadata
* Dynamisk identifiering av en användares enhet och plats för att återge formuläret på rätt sätt via webben och mobila kanaler
* Integrering med Adobe Analytics för effektiv mätning av formuläranvändningsstatistik
* Integrering med Adobe Document Cloud eSign-tjänster eller Scribble för att elektroniskt signera dokument som innehåller konfidentiell information
* Automatiserade funktioner för blankettpublicering och möjlighet att leverera skräddarsydd kommunikation i rätt tid via flera kanaler

## AEM-formulärtyper {#aem-form-types}

Med AEM Forms kan du utöka nya och befintliga formulär för att skapa:

* pixelperfekt, paginerad HTML och PDF forms som ser ut nästan som papper, eller
* anpassningsbara formulär som automatiskt återges för användarens enhet och webbläsare.

**PDF forms**

PDF forms kan fyllas i offline, sparas lokalt och formulärdata skickas nästa gång du är online. Du kan använda 2D-streckkoder för att hämta in formulärdata och använda digitala signaturer för att validera användarnas autenticitet.

**HTML-formulär**

Webbläsarbaserade HTML5-formulär kan visas både på mobila enheter och i webbläsare på datorer. Du kan signera HTML-formulär elektroniskt med Scribble- eller eSign-tjänster.

**Anpassningsbara formulär**

Anpassningsbara formulär kan dynamiskt anpassa sig till användarsvar genom att lägga till eller ta bort fält eller avsnitt efter behov. Med AEM kan du återanvända Adobe XML-formulärmallar för att skapa anpassningsbara formulär.

### Funktioner som stöds {#supported-features}

Alla formulärtyper stöder följande funktioner:

* Dynamisk layout
* Validering av formulärfält
* Kontextkänslig hjälp
* Skript och XML-datahantering
* Utformning och kontroll av tillgänglighet
* Möjlighet att spara blanketter på serversidan
* Stöd för bifogade filer
* Integrering med HTML Workspace for Data Capture

## Datainsamling offline {#offline-data-collection}

När blankettdata har skickats kopplar Adobe Experience Manager blankettdata till befintliga system, affärsregler och personer.

AEM Forms erbjuder Forms Workspace, en mobilapplikation som utvidgar era digitala affärsprocesser till mobila enheter. Med Forms Workspace kan ni samla in och registrera data även offline. Forms Workspace använder funktionerna i din mobila enhet och gör att du kan ta foton, videor och samla in data som tidsstämplar och annan information. Nästa gång du ansluter till ett nätverk kan du synkronisera insamlade data.

Att samla in data offline och synkronisera dem nästa gång du returnerar online är särskilt användbart för personer på fältet. Det förbättrar produktiviteten och minskar antalet fel.

**Fördelar med att använda Forms Workspace för datainsamling offline**

* Lättanvänt program för HTML arbetsyta för uppdragstilldelning och spårning
* Arbetsflödesdesignmiljö med dra-och-släpp
* ECM-kontakter (Enterprise Content Management)
* Stöd för öppna standarder, inklusive XML och SOAP för att koppla blankettdata till affärssystemen
* Körklar HTML rapporterar eftersläpningar, arbetsköer och KPI (Key Performance Indicators)
* Anpassningsbara instrumentpaneler för realtidsinsikter i affärsverksamheten
* API för att ansluta till rapporteringsverktyg från tredje part

![Tredje utkastet](do-not-localize/3rd-draft.gif)

## Personlig kommunikation {#personalized-communication}

En viktig komponent i en effektiv självbetjäningsdigital upplevelse är att förmedla skräddarsydd information som användarna kommer åt var de än är och på vilken enhet de vill. Personlig kommunikation i rätt tid kan förbättra både konverteringsgraden och användarnöjdheten.

Med AEM Forms kan man skapa övertygande personaliserade användarupplevelser genom att anpassa dokumentmallar, lägga in information från affärsprocesserna och inkludera interaktiva komponenter. Ett intuitivt användargränssnitt hjälper icke-tekniska användare att ta fram affärsregler som bestämmer när ett meddelande ska skapas baserat på en fråga eller initiera ett användargenererat svar.

Personaliserade dokument som kvitton, kvitton, välkomstpaket och kontoutdrag kan enkelt skickas via flera kanaler. Organisationer kan driva trafik till personaliserade webbportaler, vilket leder till registrering eller inköp av ytterligare tjänster.

**Viktiga funktioner**

* Korrespondensredigeringsmiljö med stöd för mallar, innehållsblock, affärsregler med mera
* Dokumentkonvertering och sammanställning
* Stöd för on-demand- eller batchdistribution av dokument via flera kanaler, inklusive webb, e-post och papper
* Granskningsspår med ändringshistorik
* Stöd för digitala signaturer för att validera innehållets integritet och undertecknarens identitet
* Dokumentsäkerhetstillägg för AEM Forms, inklusive kryptering, användarprofiler, spårning och granskning

![Layout två](do-not-localize/layout-02.png)

Effektivt arbetsflöde för skräddarsydd kommunikation
