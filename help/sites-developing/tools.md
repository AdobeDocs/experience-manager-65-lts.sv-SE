---
title: Testnings- och spårningsverktyg
description: AEM tillhandahåller ett ramverk för testning av komponentgränssnitt och en mekanism för testning och felsökning av komponenter
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: testing
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 4aa0f10d-e915-4ad2-a886-080ed8b9b10f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 0%

---

# Testnings- och spårningsverktyg{#testing-and-tracking-tools}

## Testning {#testing}

AEM tillhandahåller:

* [ett ramverk för testning av komponentens användargränssnitt](/help/sites-developing/hobbes.md).
* [en mekanism för att testa och felsöka komponenter](/help/sites-developing/developer-mode.md).

Följande två testverktyg är öppna för Source:

**Selen**

Selenium används för funktionstestning i en webbläsare med en användare per aktivitet. Här registreras teststeg (klick) som antingen HTML-tabeller eller Java™-klasser.

Mer information finns i [https://www.selenium.dev/](https://www.selenium.dev/).

**JMeter**

JMeter används för att spåra förfrågningar och kan användas för funktions-, prestanda- och stresstester.

Mer information finns i [https://jmeter.apache.org/](https://jmeter.apache.org/).

Det finns också många egna verktyg för att automatisera tester och hantera testplaner.

### Spårning {#tracking}

Följande verktyg är enkelt tillgängliga. Men ett nyckelproblem i alla fall är att alla medlemmar i projektteamet - partner och kund - har tillgång till data.

**Bugzilla**

Ett felsökningssystem som kan konfigureras efter dina egna behov.

**Kalkylblad**

Även om kalkylblad inte är specifikt ett felsökningsverktyg, används ofta *mis* för detta ändamål eftersom de är lätta att förstå och de flesta användare har erfarenhet av sin funktionalitet.

Om de här kalkylbladen används för spårning:

* de ska vara enkla.
* Antalet enskilda kalkylblad bör begränsas till ett minimum.
* måste uppdateras regelbundet.
* endast en primär kopia ska bevaras och alla ska veta var den primära kopian finns.
* De ska vara tillgängliga för alla projektmedlemmar.
* Om säkerhet är ett problem (ofta i stora företag) och gemensam åtkomst inte är möjlig, kan kopior distribueras så länge alla förstår att dessa kalkylblad är kopior och inte kan uppdateras.

Det finns också många egna verktyg för att spåra buggar och funktionsbehov.
