---
title: Lösningsintegrering
description: Läs mer om hur du integrerar Adobe Experience Manager (AEM) med andra Adobe- eller tredjepartstjänster.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: ac7f2ea1-4e0c-44da-8d1d-d65c65d817cb
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---

# Lösningsintegrering{#solutions-integration}

* [Integrera med Adobe Experience Cloud](/help/sites-administering/marketing-cloud.md)
* [Integrera med tredjepartstjänster](/help/sites-administering/third-party-services.md)
* [Analyser med externa leverantörer](/help/sites-administering/external-providers.md)
* [Förstå, använda och strukturera smarta taggar](/help/assets/enhanced-smart-tags.md)

Följande information finns om hur du integrerar AEM med andra tjänster från Adobe eller tredje part:

>[!NOTE]
>
>Om du använder en anpassad proxykonfiguration tillsammans med integreringen måste du konfigurera båda HTTP-klientproxykonfigurationerna eftersom vissa funktioner i AEM använder 3.x-API:erna och några andra 4.x-API:er:
>
>* 3.x har konfigurerats med [http://localhost:4502/system/console/configMgr/com.day.commons.httpclient](http://localhost:4502/system/console/configMgr/com.day.commons.httpclient)
>* 4.x har konfigurerats med [http://localhost:4502/system/console/configMgr/org.apache.http.proxyconfigurator](http://localhost:4502/system/console/configMgr/org.apache.http.proxyconfigurator)
>
