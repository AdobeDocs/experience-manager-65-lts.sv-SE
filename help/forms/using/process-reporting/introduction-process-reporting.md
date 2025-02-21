---
title: Introduktion till processrapportering
description: Introduktion och viktiga funktioner i AEM Forms om JEE Process Reporting
content-type: reference
topic-tags: process-reporting
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Introduktion till processrapportering{#introduction-to-process-reporting}

![processrapportering](assets/process-reporting.png)

Process Reporting är ett webbläsarbaserat verktyg som du använder för att skapa och visa rapporter om AEM Forms processer och uppgifter.

Processrapportering innehåller en uppsättning användningsklara rapporter som gör att du kan filtrera, visa information om långvariga processer, processers varaktighet och arbetsflödesvolym.

Dessutom innehåller Process Reporting ett gränssnitt för att köra ad hoc-frågor och för att integrera anpassade rapportvyer i användargränssnittet för Process Reporting.

En lista över webbläsare som stöds finns i [AEM Forms-plattformar som stöds](/help/forms/using/aem-forms-jee-supported-platforms.md).

Processrapportering bygger på moduler som:

* Läs processdata från AEM Forms-databasen
* Publicera processdata till en inbäddad Process Reporting-databas
* Tillhandahåller ett webbläsarbaserat användargränssnitt för att visa rapporter

## Nyckelfunktioner {#key-capabilities}

### Alltid aktiverad rapportering {#always-on-reporting}

![platshantering](assets/site-management.png)

Visa en lista över processer som körs länge, tidsscheman för processer och kör anpassade frågor med filter.

Med Process Reporting kan du också exportera rapport- och frågedata i CSV-format.

### Ad hoc-rapporter {#adhoc-reports}

![print-&amp;-color](assets/print-&-colour.png)

Använd filter för att få en specifik vy över dina data.

Du kan söka efter processer eller uppgifter efter ID, varaktighet, start- och slutdatum, processinitierare och så vidare.

Du kan kombinera flera filter för att skapa specifika rapporter.

Du kan sedan spara rapportfiltren så att de kan köras vid ett senare datum eller en senare tidpunkt.

### Process-/aktivitetshistorik {#process-task-history}

![filhantering](assets/file-management.png)

AEM Forms-servrar kör flera processer parallellt. Dessa processer fortsätter att gå från ett läge till ett annat. Genom att publicera Forms-data till Process Reporting-databasen med regelbundna intervall, bevarar Process Reporting övergångsinformationen om de processer som körs i AEM Forms.

### Åtkomstkontroll {#access-control-br}

![namnlös](assets/untitled.png)

Processrapportering ger behörighetsbaserad åtkomst till användargränssnittet.

Det innebär att bara användare med rapportbehörigheter har åtkomst till användargränssnittet för processrapportering.
