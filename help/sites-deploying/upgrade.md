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
exl-id: ebc34847-dc3d-41ed-b0d6-f004c3debcd9
source-git-commit: 57bf39aa914bddca05d526b46b581579965069d6
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# Uppgradera till Adobe Experience Manager (AEM) 6.5 LTS {#upgrading-to-aem}

>[!NOTE]
>Uppgraderingen till AEM 6.5 LTS är tillgänglig för alla 6.5 Service Packs som stöds.

>[!NOTE]
>
>Ur ett tekniskt perspektiv är uppgraderingsprocessen från AEM 6.5 LTS till AEM 6.5 LTS Service Pack utformat för att vara en sömlös [på plats-uppgradering](/help/sites-deploying/in-place-upgrade.md). Den här processen kräver vanligtvis inga kodändringar från kunderna, såvida det inte uttryckligen anges i versionsinformationen.

Detta avsnitt omfattar uppgradering av en AEM-installation till AEM 6.5 LTS:

<!-- Alexandru: drafting for now 

* [Planning Your Upgrade](/help/sites-deploying/upgrade-planning.md)
* [Assessing the Upgrade Complexity with Pattern Detector](/help/sites-deploying/pattern-detector.md)
* [Backward Compatibility in AEM 6.5](/help/sites-deploying/backward-compatibility.md)
-->

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

Foundation layer har nu stöd för Java 17 och Java 21, som innehåller de senaste open-source-paketen från Apache Sling, Felix och Jackrabbit Oak. Dessutom har förpackningen till AEM 6.5 LTS uber-burken ändrats. Dessutom har några äldre funktioner tagits bort från AEM 6.5 LTS. Mer information finns i [Versionsinformation](/help/release-notes/release-notes.md#whats-new-what-s-new) och [Lista över föråldrade paket som avinstallerats efter uppgraderingen](/help/sites-deploying/obsolete-bundles.md)

AEM 6.5 LTS har ett starkt fokus på bakåtkompatibilitet för funktioner och levereras med ett analysverktyg. Se [Utvärdera uppgraderingskomplexiteten med AEM Analyzer](/help/sites-deploying/aem-analyzer.md) för en bedömning av komplexiteten när du börjar [planera för uppgraderingen](/help/sites-deploying/upgrade-planning.md).
