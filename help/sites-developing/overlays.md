---
title: Övertäckningar
description: Adobe Experience Manager använder principen om övertäckningar för att utöka och anpassa konsoler och andra funktioner.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Övertäckningar{#overlays}

Adobe Experience Manager (AEM) - och tidigare CQ - har länge använt principen med övertäckningar för att du ska kunna utöka och anpassa [konsolerna](/help/sites-developing/customizing-consoles-touch.md) och andra funktioner (till exempel [sidredigering](/help/sites-developing/customizing-page-authoring-touch.md)).

Övertäckning är en term som används i många sammanhang. I det här sammanhanget (utökning av AEM) innebär en övertäckning att använda de fördefinierade funktionerna och lägga in egna definitioner över dessa (för att anpassa standardfunktionerna).

I en standardinstans finns de fördefinierade funktionerna under `/libs` och vi rekommenderar att du definierar övertäckningen (anpassningar) under grenen `/apps`. AEM använder en söksökväg för att hitta en resurs, söker först i grenen `/apps` och sedan i grenen `/libs` (söksökvägen [kan konfigureras](#configuring-the-search-paths)). Den här mekanismen innebär att övertäckningen (och de anpassningar som definieras där) har prioritet.

Sedan AEM 6.0 har ändringar gjorts i hur övertäckningar implementeras och används:

* AEM 6.0 och senare - för [Granite](https://developer.adobe.com/experience-manager/reference-materials/6-5/granite-ui/api/jcr_root/libs/granite/ui/index.html)-relaterade övertäckningar (det vill säga det beröringskänsliga användargränssnittet)

   * Metod

      * Rekonstruera lämplig `/libs`-struktur under `/apps`.

        Det här kräver ingen 1:1-kopia, [Sling Resource Merger](/help/sites-developing/sling-resource-merger.md) används för att korsreferera de ursprungliga definitioner som krävs. Med Sling Resource Merger får du tillgång till och kan sammanfoga resurser med olika mekanismer.

      * Gör eventuella ändringar under `/apps`.

   * Fördelar

      * Mer robust för ändringar under `/libs`.
      * Definiera bara om vad som krävs.

* Övertäckningar och övertäckningar som inte är granitövertäckningar före AEM 6.0

   * Metod

      * Kopiera innehållet från `/libs` till `/apps`

        Kopiera hela undergrenen, inklusive egenskaper.

      * Gör eventuella ändringar under `/apps`.

   * Nackdelar

      * Även om dina ändringar inte går förlorade när något ändras under `/libs` kanske du måste återskapa vissa ändringar som inträffar i övertäckningen under `/apps`.

>[!CAUTION]
>
>[Samling av resurser](/help/sites-developing/sling-resource-merger.md) och de relaterade metoderna kan bara användas med [Granite](https://developer.adobe.com/experience-manager/reference-materials/6-5/granite-ui/api/jcr_root/libs/granite/ui/index.html). Det innebär att det bara är lämpligt att skapa en övertäckning med en skelettstruktur för det pekaktiverade standardgränssnittet.
>
>Övertäckningar för andra områden (inklusive det klassiska användargränssnittet) innebär att rätt nod och hela understruktur kopieras och att nödvändiga ändringar görs.

Övertäckningar rekommenderas för många ändringar, till exempel [konfigurera konsoler](/help/sites-developing/customizing-consoles-touch.md#create-a-custom-console) eller [skapa din markeringskategori för resursläsaren på sidpanelen](/help/sites-developing/customizing-page-authoring-touch.md#add-new-selection-category-to-asset-browser) (används vid redigering av sidor). De krävs enligt följande:

* ***Gör inte* ändringar i `/libs`-grenen **Alla ändringar du gör kan gå förlorade eftersom den här grenen kan ändras när du:

   * uppgradera till din instans
   * tillämpa en snabbkorrigering
   * installera ett funktionspaket

* De koncentrerar dina ändringar på en plats, vilket gör det enklare för dig att spåra, migrera, säkerhetskopiera eller felsöka ändringarna efter behov.

## Konfigurera sökvägar {#configuring-the-search-paths}

För övertäckningar är den levererade resursen en sammanställning av resurser och egenskaper som har hämtats, beroende på sökvägar som kan definieras:

* Resursen **Sökväg för lösare** enligt definitionen i [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) för **Apache Sling Resource Resolver Factory**.

   * Sökvägarnas övre och nedre ordning anger deras respektive prioriteringar.
   * I en standardinstallation är de primära standardvärdena `/apps`, `/libs`, vilket innebär att innehållet i `/apps` har högre prioritet än innehållet i `/libs` (d.v.s. *overlays*).

* Två tjänstanvändare behöver JCR:READ-åtkomst till den plats där skripten lagras. Dessa användare är: components-search-service (används av com.day.cq.wcm.coreto-åtkomst/cache-komponenter) och sling-scripting (används av org.apache.sling.servlets.resolver för att hitta servrar).
* Följande konfiguration måste även konfigureras efter var du placerade dina skript (i det här exemplet under /etc, /libs eller /apps).

  ```
  PID = org.apache.sling.jcr.resource.internal.JcrResourceResolverFactoryImpl
  resource.resolver.searchpath=["/etc","/apps","/libs"]
  resource.resolver.vanitypath.whitelist=["/etc/","/apps/","/libs/","/content/"]
  ```

* Slutligen måste även Serverlösaren konfigureras (i det här exemplet lägger du till /etc)

  ```
  PID = org.apache.sling.servlets.resolver.SlingServletResolver
  servletresolver.paths=["/bin/","/libs/","/apps/","/etc/","/system/","/index.servlet","/login.servlet","/services/"]
  ```

## Exempel på användning {#example-of-usage}

Några exempel beskrivs när:

* [Anpassa konsolerna](/help/sites-developing/customizing-consoles-touch.md)
* [Anpassa sidredigering](/help/sites-developing/customizing-page-authoring-touch.md)
