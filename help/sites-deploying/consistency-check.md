---
title: Konsekvenskontroll och genomgående kontroll
description: Lär dig hur du utför konsekventa och stegvisa kontroller.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Konsekvenskontroll och genomgående kontroll{#consistency-and-traversal-checks}

När du uppgraderar kan det uppstå problem på grund av inkonsekvenser i arbetsytan. Du kan antingen köra en testuppgradering för att se om detta är ett problem eller köra konsekvenskontrollerna som en förebyggande åtgärd.

Om du kör en testuppgradering som misslyckas på grund av inkonsekvenser i arbetsytan ser du liknande poster i crx-quickstart/logs/crx/error.log:

```xml
*ERROR* TarPersistenceManager: No bundle found for uuid 'deadbeef-cafe-babe-cafe-babecafebabe'
 ...
*ERROR* RepositoryImpl: Failed to initialize workspace 'crx.default'
javax.jcr.RepositoryException: Error indexing workspace: Error indexing workspace: Error indexing workspace
...
```

## Utför en konsekvenskontroll {#perform-a-consistency-check}

Om du vill utföra en konsekvenskontroll går du till administrationssidan för JMX Mbean **com.adobe.granite (Repository)**. På AEM huvudskärm går du till

**Verktyg > Webbkonsol > Huvudmeny (på menyraden) > JMX > com.adobe.granite (databas)**

I en standardinstallation finns den här: **[|Visa mig|](http://localhost:4502/system/console/jmx/com.adobe.granite%3Atype%3DRepository)**

I avsnittet **Åtgärder** på sidan finns två metoder: **`traversalCheck`** och **`consistencyCheck`**. Om du vill utföra en kontroll klickar du på åtgärden och anger önskade parametrar.

![chlimage_1-117](assets/chlimage_1-117.png)
