---
title: Hur startar jag om AEM SDK?
description: De bästa sätten att starta om AEM SDK
role: Admin, Developer, User
feature: Adaptive Forms,AEM Forms on JEE,AEM Forms on OSGi
solution: Experience Manager, Experience Manager Forms
exl-id: 68935045-89b1-4219-b111-88a4600200df
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---

# Starta om AEM SDK

Om du startar om AEM SDK genom att stoppa Java™-processerna kan det leda till inkonsekvenser i AEM utvecklingsmiljö. Ett fel inträffar:

`javax.jcr.RepositoryException: Applying repoinit operation failed despite retry; set loglevel to DEBUG to see all exceptions. Last exception message was: Failed to set ACL (javax.jcr.ValueFormatException: Invalid type: 0) AclLine ALLOW {principals=[forms-xfa-writers], privileges=[jcr:modifyProperties]} restrictions=[rep:glob=[*/jcr:content/*], rep:itemNames=[xfaForm], fd:condition=[xfaForm, 1]]`

![Restart-aem-sdk-error](/help/forms/using/assets/restart-sdk-error.png)

## Lösning

Om du vill starta om AEM SDK går du till det aktiva kommandofönstret och trycker på `Ctrl + C` för att starta om SDK.

Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel genom att stoppa Java™-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.
