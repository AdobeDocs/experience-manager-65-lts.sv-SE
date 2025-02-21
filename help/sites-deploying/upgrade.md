---
title: Uppgradera till Adobe Experience Manager 6.5
description: Lär dig grunderna i hur du uppgraderar en äldre Adobe Experience Manager-installation (AEM) till AEM 6.5.
contentOwner: sarchiz
topic-tags: upgrading
content-type: reference
docset: aem65
targetaudience: target-audience upgrader
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: f66bb283e5c2a746821839269e112be8c2714ba7
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Uppgradera till Adobe Experience Manager (AEM) 6.5 LTS {#upgrading-to-aem}

>[!NOTE]
>Uppgraderingen till AEM 6.5 LTS stöds från de senaste 6 servicepaketen.

Detta avsnitt handlar om uppgradering av en AEM-installation till AEM 6.5:

<!-- Alexandru: drafting for now 

* [Planning Your Upgrade](/help/sites-deploying/upgrade-planning.md)
* [Assessing the Upgrade Complexity with Pattern Detector](/help/sites-deploying/pattern-detector.md)
* [Backward Compatibility in AEM 6.5](/help/sites-deploying/backward-compatibility.md)
  This was drafted before: * [Using Offline Reindexing To Reduce Downtime During an Upgrade](/help/sites-deploying/upgrade-offline-reindexing.md)-->

<!--
* [Upgrade Procedure](/help/sites-deploying/upgrade-procedure.md)
* [Upgrading Code and Customizations](/help/sites-deploying/upgrading-code-and-customizations.md)
* [Pre-Upgrade Maintenance Tasks](/help/sites-deploying/pre-upgrade-maintenance-tasks.md)
* [Performing an In-Place Upgrade](/help/sites-deploying/in-place-upgrade.md)
* [Post Upgrade Checks and Troubleshooting](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md)
* [Sustainable Upgrades](/help/sites-deploying/sustainable-upgrades.md)
* [Lazy Content Migration](/help/sites-deploying/lazy-content-migration.md)

-->

För att underlätta referensen till de AEM-instanser som ingår i dessa procedurer används följande termer i alla dessa artiklar:

* *source*-instansen är den AEM-instans som du uppgraderar från.
* *target*-instansen är den som du uppgraderar till.

## Vad har ändrats? {#what-has-changed}

### Uppdateringar {#updates}

Nedan följer viktiga ändringar av anmärkningar under de senaste versionerna av AEM:

1. Foundation-lagret har uppgraderats för att stödja Java 17 (som omfattar lager med öppen källkod från Apache Sling, Apache Felix och Apache Jackrabbit Oak)

1. AEM 6.5 LTS jar-paketet har nu stöd för specifikationer för Jarkarta Servlet API:er 5 och krigsförpackningar kan distribueras till serverbehållare som implementerar specifikationer för Jakarta Servlet API 5/6

1. Förpackningen av AEM 6.5 LTS uber-jar har ändrats. Mer information finns i [Uppgradera kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md).

### Äldre funktioner/artefakter har tagits bort {#removed-legacy-features-artifacts}

Följande äldre lösningar har tagits bort från AEM 6.5 LTS. Mer information finns i TBD: link to release notes and [List of Obsolete Bundles Uninstalled After the Upgrade](/help/sites-deploying/obsolete-bundles.md) (på engelska)

1. Social
1. Commerce
1. Screens
1. Vi-butik
1. Integration av sökning och marknadsföring

**Borttagna artefakter**

1. CRX-explorer
1. Crx2oak
1. Google guava (borttaget på grund av säkerhetsluckor)
1. Abdera-parser (borttagen på grund av säkerhetsluckor)
1. jdom (`org.apache.servicemix.bundles.jdom`) (borttagen på grund av säkerhetsluckor)
1. `com.github.jknack.handlebars` (borttagen på grund av säkerhetsproblem)

AEM 6.5 LTS har ett starkt fokus på bakåtkompatibilitet för funktioner och levereras med ett analysverktyg. Se [Utvärdera uppgraderingskomplexiteten med AEM Analyzer](/help/sites-deploying/pattern-detector.md) för en bedömning av komplexiteten när du börjar planera för uppgraderingen. Mer information om vad mer som har ändrats finns i den fullständiga versionsinformationen här. TBD: Länk till versionsinformation om AEM 6.5 LTS