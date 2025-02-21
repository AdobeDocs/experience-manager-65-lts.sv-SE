---
title: Installera och konfigurera dokumentsäkerhetsservern
description: Använd dokumentskydd för att på ett säkert sätt distribuera information som du har sparat i ett format som stöds. Endast behöriga användare har åtkomst till skyddade dokument.
contentOwner: khsingh
role: Admin
solution: Experience Manager, Experience Manager Forms
feature: Interactive Communication,Document Security
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# Installera och konfigurera dokumentsäkerhetsservern {#installing-and-configuring-the-document-security-server}

Använd dokumentskydd för att på ett säkert sätt distribuera information som du har sparat i ett format som stöds. Endast behöriga användare har åtkomst till skyddade dokument.

Adobe Experience Manager Forms dokumentsäkerhet säkerställer att endast behöriga användare kan använda dina dokument. Med dokumentsäkerhet kan du distribuera all information som du har sparat i ett format som stöds. Filformat som stöds är bland annat Adobe Portable Document Format (PDF) och Microsoft Word-, Excel- och PowerPoint-filer.

Du kan skydda dokument med hjälp av profiler. De sekretessinställningar du anger i en profil avgör hur en mottagare kan använda ett dokument som du tillämpar profilen på. Du kan till exempel ange om mottagarna ska kunna skriva ut eller kopiera text, redigera text eller lägga till signaturer och kommentarer i skyddade dokument.

Profilerna lagras på dokumentsäkerhetsservern. Du tillämpar profilerna på dokument via ditt klientprogram. När du tillämpar en profil på ett dokument skyddar sekretessinställningarna som anges i profilen den information som dokumentet innehåller. Du kan distribuera det profilskyddade dokumentet till mottagare som har behörighet enligt profilen.

Dokumentsäkerhet ger även klienter, tittare och indexerare möjlighet att skydda dokument, visa skyddade dokument och indexera skyddade dokument. Mer information om dokumentsäkerhet finns i [Om dokumentsäkerhet](/help/forms/using/admin-help/document-security.md).

## Distributionstopologi  {#deployment-topology}

Dokumentsäkerhetsfunktionerna är bara tillgängliga i AEM Forms på JEE. Du behöver en instans av AEM Forms på JEE. Du kan också skapa ett kluster eller en grupp med AEM Forms-servrar om det behövs. Följande topologi är en indikativ topologi för att köra dokumentsäkerhetsfunktionen. Mer information om topologin finns i [Arkitektur och distributionstopologier för AEM Forms](aem-forms-architecture-deployment.md).

<!--fix above link-->

![Topologi för dokumentsäkerhetsserver](do-not-localize/document-security-server_topology.png)

Bilden nedan visar den typiska arkitekturen för AEM Forms Document Security:

![Standardmiljö för dokumentsäkerhet](do-not-localize/document-security-typical-environment.png)

## Installera AEM Forms på JEE {#installing-aem-forms-on-jee}

Så här installerar och konfigurerar du AEM Forms på JEE:

1. Hämta installationsprogrammet för AEM 6.5 Forms på JEE från [Adobe Licensing Website (LWS)](https://licensing.adobe.com/). Du behöver ett giltigt Maintenance &amp; Support-avtal för att ladda ned installationsprogrammet.
1. Läs dokumentet [AEM Forms på JEE-plattformar som stöds](/help/forms/using/aem-forms-jee-supported-platforms.md) och kontrollera att du har programvaran, maskinvaran, operativsystemen, programservern, databaser, JDK:er och annan infrastruktur redo att installera AEM Forms på JEE.
1. (Endast icke-nyckelinstallationer) Läs [Förbereder installation av AEM Forms single server](https://www.adobe.com/go/learn_aemforms_prepareInstallsingle_64) eller [Förbereder installation av AEM Forms serverkluster](https://www.adobe.com/go/learn_aemforms_prepareInstallcluster_64) och klar för installation och konfigurering av AEM Forms på JEE.
1. Beroende på din miljö och programserver väljer du något av följande dokument och följer instruktionerna för att slutföra installationen

   * [Installera och distribuera AEM Forms på JEE med körningsnyckeln JBoss](https://www.adobe.com/go/learn_aemforms_installTurnkey_64)
   * [Installera och distribuera AEM Forms på JEE för JBoss](https://www.adobe.com/go/learn_aemforms_installJBoss_64)
   * [Installera och distribuera AEM Forms på JEE för WebLogic](https://www.adobe.com/go/learn_aemforms_installWebLogic_64)
   * [Installera och distribuera AEM Forms på JEE för WebSphere](https://www.adobe.com/go/learn_aemforms_installWebSphere_64)
   * [Konfigurerar AEM Forms på JEE i JBoss-kluster](https://www.adobe.com/go/learn_aemforms_clusterJBoss_64)
   * [Konfigurerar AEM Forms på JEE i WebLogic-kluster](https://www.adobe.com/go/learn_aemforms_clusterWebLogic_64)
   * [Konfigurerar AEM Forms på JEE i WebSphere-kluster](https://www.adobe.com/go/learn_aemforms_clusterWebSphere_64)

   >[!NOTE]
   >
   >Välj alternativet Dokumentsäkerhet på skärmen Modulval i AEM Forms i konfigurationshanteraren för JEE. Alternativet Dokumentskydd kräver inte att någon annan modul är markerad.

## Nästa steg {#next-steps}

* [Konfigurera klient- och serveralternativ](/help/forms/using/admin-help/configuring-client-server-options.md)
* [Skapa och hantera policyer](/help/forms/using/admin-help/creating-policies.md)
* [Skapa och hantera principuppsättningar](/help/forms/using/admin-help/creating-policy-sets.md)
