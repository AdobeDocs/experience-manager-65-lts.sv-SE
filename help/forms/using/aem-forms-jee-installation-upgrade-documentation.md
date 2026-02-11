---
title: Installations- och uppgraderingsarbetsflöde för AEM Forms i JEE
description: Installations- och uppgraderingsarbetsflöde för AEM Forms 6.5 LTS on JEE (JBoss), med en konsoliderad lista över relevanta PDF-filer och deras syfte.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: installing
geptopics: SG_AEMFORMS/categories/jee
docset: aem65
role: Admin
solution: Experience Manager, Experience Manager Forms
feature: AEM Forms on JEE,AEM Forms Upgrade
exl-id: 6d8c0e24-7f08-4e66-bb12-2cf1cfe1d5d3
source-git-commit: fb9f6ef794da7f3b242e9e81a6c2505692c16cd8
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 1%

---


# Installations- och uppgraderingsarbetsflöde för AEM Forms i JEE {#aem-forms-jee-installation-upgrade-documentation}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Använd följande arbetsflöde och tabeller för att välja rätt guide/guider för ditt scenario. Vissa dokument publiceras som PDF-filer. Ytterligare dokument kan läggas till när de blir tillgängliga.

## Arbetsflöde för installation och uppgradering {#installation-upgrade-workflow}

1. Granska [Plattformar som stöds för AEM Forms på JEE](/help/forms/using/aem-forms-jee-supported-platforms.md) och kontrollera att systemet uppfyller de programvaru-/maskinvarukombinationer som krävs.
2. Bestäm om du utför en **ny installation** eller en **uppgradering**.
3. För den valda sökvägen följer du den sekvens som beskrivs nedan (vissa scenarier kräver mer än en stödlinje).

## Före installation och föruppgradering {#start-here}

| Guide | Beskrivning |
| --- | --- |
| [Plattformar som stöds för AEM Forms på JEE](/help/forms/using/aem-forms-jee-supported-platforms.md) | Visar program- och maskinvarukombinationer som stöds för AEM Forms i JEE. Använd detta för att validera förutsättningarna innan du påbörjar en installation eller uppgradering. |
| [Arkitektur och distributionstopologier för AEM Forms](/help/forms/using/aem-forms-architecture-deployment.md) | Beskriver rekommenderade arkitekturer och distributionstopologier så att du kan välja en metod som passar din miljö (till exempel en server jämfört med ett kluster). |
| [Välja en beständig typ för en AEM Forms-installation](/help/forms/using/choosing-persistence-type-for-aem-forms.md) | Hjälper dig att välja en lämplig beständighetstyp för installationstopologin innan du börjar. |

## Hur installerar jag Adobe Experience Manager Forms (AEM Forms) på JEE på JBoss? {#installing-aem-forms-jee-jboss}

### Turnkey {#install-jee-jboss-turnkey}

| Guide | Beskrivning |
| --- | --- |
| [Installera och distribuera AEM Forms 6.5 LTS på JEE med JBoss Turnkey (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/install-turnkey.pdf) | Använd för en **ny körklar installation** på JBoss. Det här dokumentet innehåller anvisningar om hur du installerar och distribuerar AEM Forms på JEE med hjälp av körklar JBoss-distribution. |

### En server (ej nyckelord) {#install-jee-jboss-single-server}

| Guide | Beskrivning |
| --- | --- |
| [Förbereder installation av AEM Forms (en server) (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/prepare-install-single-server.pdf) | Använd **före** en **ny installation av en enda server (ej nyckelord)**. I det här dokumentet finns en lista över krav och steg för miljöförberedelser för installation av AEM Forms på JEE i en servertopologi. |
| [Installera och distribuera AEM Forms på JEE för JBoss (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/install-jboss.pdf) | Använd för **stegvis installation och distribution** av AEM Forms på JEE på JBoss (**ej körklar**). Om du har installerat med en server följer du den här guiden **när** har slutfört *Förbereda för installation av AEM Forms (en server)*. |

<!--
| Preparing to Install AEM Forms (Server Cluster) (PDF) (**TBD**) | Use **before** a **fresh cluster installation**. Describes prerequisites and environment preparation steps for installing AEM Forms on JEE in a server cluster topology. *(Link will be added once the PDF is available.)* |
| [Configuring AEM Forms on JEE on JBoss Cluster (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/cluster-jboss.pdf) | Use when setting up and configuring a **JBoss cluster topology** for AEM Forms on JEE. |
-->

## Hur uppgraderar jag Adobe Experience Manager Forms (AEM Forms) på JEE på JBoss? {#upgrading-aem-forms-jee-jboss}

### Turnkey {#upgrade-jee-jboss-turnkey}

| Guide | Beskrivning |
| --- | --- |
| [Uppgraderar till AEM Forms 6.5 LTS i JEE för JBoss Turnkey (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/upgrade-turnkey.pdf) | Använd för en **nyckeluppgradering**. Det här dokumentet innehåller anvisningar om hur du uppgraderar en befintlig AEM Forms på en körklar JBoss-installation för JEE. |

### En server {#upgrade-jee-jboss-single-server}

| Guide | Beskrivning |
| --- | --- |
| [Förbereder uppgradering av AEM Forms (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/prepare-upgrade.pdf) | Använd **före** en **uppgradering för en server**. Beskriver hur du förbereder miljön innan du uppgraderar till AEM 6.5 LTS Forms. Det gäller miljöer där AEM Forms körs på JEE i ett enda serverinstallationsläge. |
| [Uppgraderar till AEM Forms på JEE för JBoss (PDF)](https://helpx.adobe.com/content/dam/help/en/experience-manager/65LTS/forms/upgrade-jboss.pdf) | Använd för uppgraderingsproceduren **steg-för-steg** på JBoss i installationsläget **för en enda server**. Följ den här guiden **när** har slutfört *Förbereder uppgradering av AEM Forms*. |

<!--
| Preparing to Install AEM Forms (Server Cluster) (PDF) (**TBD**) | Use **before** a **cluster upgrade**. Describes how to prepare the environment for a server cluster before upgrading to AEM 6.5 LTS Forms. It applies to environments running AEM Forms on JEE in a server cluster installation mode. *(Link will be added once the PDF is available.)* |
| Upgrading to AEM Forms on JEE for JBoss (Cluster) (PDF) (**TBD**) | Use for the **step-by-step upgrade procedure** on JBoss in a **clustered** installation mode. Follow this guide **after** completing *Preparing to Install AEM Forms (Server Cluster)*. *(Link will be added once the PDF is available.)* | -->

