---
title: Problem med installation av AEM Forms JEE 6.5.15.0 Service Pack i JBoss® Linux®-miljö
description: AEM Forms JEE 6.5.15.0-tjänstpaketet är inte korrekt installerat i JBoss® Linux®-miljön. Eventuella korrigeringsändringar tillämpas inte på programservern. Lägg till filen RUP_BOM.xml i XML-katalogen.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,AEM Forms on JEE
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Installationsproblem med AEM Forms 6.5.15.0 JEE Service Pack i JBoss®-miljö {#aem-forms-installation-issue-environment}

## Problem {#issue}

Service Pack för AEM Forms JEE 6.5.15.0 är inte korrekt installerat i JBoss® Linux®-miljön. Loggposten `[AEM_Forms_JEE_DIR]/patch/AEMForms-6.5.0-0057/xml/RUP_BOM.xml not found! Assuming this component is not in the installation. Skipping Processing` loggas i filen `PatchInstallerProcessing[1-9*].log`. Den här posten anger att installationen av Service Pack för AEM Forms JEE 6.5.15.0 inte lyckas.

## Gäller för {#applies-to}

Denna lösning gäller
* JBoss® Linux® Environment

>[!NOTE]
>
> Kontrollera att AEM Forms JEE 6.5.15.0-tjänstpaketet är installerat på programservern minst en gång innan du utför stegen i [lägger till filen RUP_BOM.xml i XML-katalogen](#solution-solution).

## Lösning {#solution}

Om du vill åtgärda installationsproblemet med AEM Forms JEE 6.5.15.0-tjänstpaketet lägger du till filen `RUP_BOM.xml` i XML-katalogen:
1. Navigera till mappen där du extraherade korrigeringen `AEMForms-6.5.0-0057_jboss_linux.tar.gz`.
1. Navigera till platsen `/CDROM_Installers/Linux/Disk1/InstData` och leta upp filen `Resource1.zip`.
1. Kopiera filen `Resource1.zip` på en annan plats utanför den extraherade mappen och packa upp filen `Resource1.zip`.
1. Navigera till `/C_/builds/dev_releng/branches/rrt/aem6.5.0_rollup/tier1/install/patch/fileset_dir/xml` och kopiera filen `RUP_BOM.xml`.
1. Klistra in filen RUP_BOM.xml på `[aem_forms_jee_installation_dir]/patch/AEMForms-6.5.0-0057/xml`.
1. Installera om [AEM Forms JEE 6.5.15.0 Service Pack ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html).
