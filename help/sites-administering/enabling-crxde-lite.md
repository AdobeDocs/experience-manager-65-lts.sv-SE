---
title: Aktivera CRXDE Lite i AEM
description: Lär dig hur du aktiverar CRXDE Lite i Adobe Experience Manager.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Aktivera CRXDE Lite i AEM{#enabling-crxde-lite-in-aem}

För att AEM-installationerna ska vara så säkra som möjligt rekommenderar checklistan [att WebDAV](/help/sites-administering/security-checklist.md#disable-webdav) inaktiveras i produktionsmiljöer.

CRXDE Lite är dock beroende av att paketet `org.apache.sling.jcr.davex` fungerar som det ska, så inaktiveras även CRXDE Lite om du inaktiverar WebDAV.

När detta inträffar visas en tom rotnod när du bläddrar till `https://serveraddress:4502/crx/de/index.jsp` och alla HTTP-begäranden till CRXDE Lite-resurser misslyckas:

```xml
404 Resource at '/crx/server/crx.default/jcr:root/.1.json' not found: No resource found
```

Den här rekommendationen är avsedd att minska attackytorna så mycket som möjligt, men systemadministratörer kan ibland behöva åtkomst till CRXDE Lite för att bläddra bland innehåll eller felsöka problem i produktionsinstanser.

Du kan aktivera CRXDE Lite med antingen [OSGi-inställningar](#enabling-crxde-lite-osgi) eller med ett [cURL-kommando](#enabling-crxde-lite-curl).

>[!WARNING]
>
>På grund av små skillnader i hur dessa metoder fungerar bör du använda ***antingen*** OSGI ***eller*** cURL.
>
>De två metoderna är ***inte*** utbytbara.

## Aktivera CRXDE Lite med OSGI {#enabling-crxde-lite-osgi}

Om du avaktiverar det här alternativet kan du aktivera CRXDE Lite på följande sätt:

1. Gå till OSGi Components-konsolen på `http://localhost:4502/system/console/components`
1. Sök efter följande komponent:

   * `org.apache.sling.jcr.davex.impl.servlets.SlingDavExServlet`

1. Klicka på skiftnyckelsikonen bredvid den för att visa dess konfigurationsalternativ:

   ![chlimage_1-80](assets/chlimage_1-80a.png)

1. Skapa följande konfiguration:

   * **Rotsökväg:** `/crx/server`
   * Markera rutan under **Använd absoluta URI:er**.

1. När du är klar med CRXDE Lite måste du inaktivera WebDAV igen.

## Aktivera CRXDE Lite med cURL {#enabling-crxde-lite-curl}

Du kan även aktivera CRXDE Lite via cURL genom att köra (båda) följande två kommandon:

* Aktivera `create-absolute-uri`:

  ```shell
  curl -u admin:admin 'http://localhost:4502/system/console/configMgr/org.apache.sling.jcr.davex.impl.servlets.SlingDavExServlet' --data-raw 'apply=true&action=ajaxConfigManager&%24location=&dav.create-absolute-uri=true&propertylist=dav.create-absolute-uri'
  ```

* Definiera `alias`:

  ```shell
  curl -u admin:admin 'http://localhost:4502/system/console/configMgr/org.apache.sling.jcr.davex.impl.servlets.SlingDavExServlet' --data-raw 'apply=true&action=ajaxConfigManager&%24location=&alias=/crx/server&propertylist=alias'
  ```

## Andra resurser {#other-resources}

Mer information om säkerhetsfunktionerna i AEM 6 finns på följande sidor:

* [AEM Security Checklist](/help/sites-administering/security-checklist.md)
* [Köra AEM i produktionsklart läge](/help/sites-administering/production-ready.md)
