---
title: Arbeta med loggar
description: Lär dig hur du felsöker AEM genom att arbeta med loggar.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---

# Arbeta med loggar{#working-with-logs}

I det här avsnittet finns detaljerad information om loggar som du kan använda för att felsöka.

>[!NOTE]
>
>Mer information om loggar finns i:
>
>* [Underhåll av granskningslogg i AEM](/help/sites-administering/operations-audit-log.md)
>* [Arbeta med granskningsposter och loggfiler](/help/sites-deploying/monitoring-and-maintaining.md#working-with-audit-records-and-log-files)

CRX registrerar detaljerade loggar. När du har packat upp och startat Quickstart finns loggarna på följande platser:

* crx-quickstart/launchpad/logs
* crx-quickstart/server/logs
* crx-quickstart/logs

## Aktivera loggnivån för FELSÖKNING {#activating-the-debug-log-level}

Standardloggnivån är INFO, d.v.s. DEBUG-meddelanden loggas inte.

Om du vill aktivera DEBUG-loggnivån använder du CRX Explorer och anger

```xml
/libs/sling/config/org.apache.sling.commons.log.LogManager/org.apache.sling.commons.log.level
```

egenskap som ska felsökas. Lämna inte loggen på DEBUG-loggnivån längre än nödvändigt eftersom den genererar många loggar.

En rad i felsökningsfilen börjar oftast med DEBUG och anger sedan loggnivån, installationsåtgärden och loggmeddelandet. Till exempel:

```xml
DEBUG 3 WebApp Panel: WebApp successfully deployed
```

Loggnivåerna är följande:

| 0 | Allvarligt fel | Åtgärden misslyckades och installationsprogrammet kan inte fortsätta. |
|---|---|---|
| 1 | Fel | Åtgärden misslyckades. Installationen fortsätter, men en del av CRX installerades inte korrekt och kommer inte att fungera. |
| 2 | Varning | Åtgärden har slutförts men problem uppstod. CRX kanske inte fungerar som det ska. |
| 3 | Information | Åtgärden har slutförts. |

## Detaljerat alternativ som används för felsökning {#verbose-option-used-for-troubleshooting}

När du startar CRX kan du lägga till alternativet -v (utförligt) på kommandoraden som i:

` java -jar crx-<*version*>-<*edition*>.jar -v`

Använd det utförliga alternativet för felsökning eftersom det här alternativet visar några av snabbstartsloggens utdata på konsolen.
