---
title: Konfigurera för AEM-appar
description: Lär dig hur du använder Adobe Experience Manager-appar för att uppdatera innehållet i OTA-programmet (i stället för AIR).
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Configuring
role: Admin
exl-id: 4f36487c-45a2-4c18-b3cc-bb9284d68f49
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 1%

---

# Konfigurera för AEM-appar{#configuring-for-aem-apps}

Med Adobe Experience Manager Apps kan du uppdatera innehållet i OTA-koden (i stället för AIR). Det uppdaterade innehållet lagras på publiceringsinstansen. Om du vill att appen på enheten ska kunna ansluta till publiceringsinstansen och söka efter uppdateringar, måste publiceringsinstansen konfigureras så att en tom referensrubrik tillåts.

## Konfigurerar tomt referenshuvud {#configuring-empty-referrer-header}

Så här konfigurerar du referenspunktsfiltertjänsten:

* Öppna Apache Felix-konsolen (**Configurations**) på:
* https://&lt;server>:&lt;portnummer>/system/console/configMgr
* Logga in som administratör.
* På menyn **Konfigurationer** väljer du: *Refererarfilter för Apache Sling*
* Markera fältet Tillåt tomt så att du kan tillåta tomma/saknade hänvisningsrubriker.
* Klicka på **Spara** för att spara ändringarna.

![chlimage_1-58](assets/chlimage_1-58a.png)

Mer information finns i [OSGI-konfigurationsinställningarna](/help/sites-deploying/osgi-configuration-settings.md) och [ Säkerhetschecklista - Problem med förfalska begäran mellan webbplatser](/help/sites-administering/security-checklist.md#protect-against-cross-site-request-forgery).
