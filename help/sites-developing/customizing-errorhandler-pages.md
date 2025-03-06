---
title: Anpassa sidor som visas av felhanteraren
description: Adobe Experience Manager har en standardfelhanterare för hantering av HTTP-fel.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 4f98853d-306f-4d11-a3d8-83122b372b2d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 0%

---

# Anpassa sidor som visas av felhanteraren{#customizing-pages-shown-by-the-error-handler}

Adobe Experience Manager (AEM) har en standardfelhanterare för hantering av HTTP-fel, till exempel genom att visa:

![chlimage_1-67](assets/chlimage_1-67a.png)

Det finns systemtillhandahållna skript (under `/libs/sling/servlet/errorhandler`) för att svara på felkoder. Som standard är följande tillgängliga med en standard-CQ-instans:

* 403.jsp
* 404.jsp

>[!NOTE]
>
>AEM baseras på Apache Sling. Se [Felhantering](https://sling.apache.org/documentation/the-sling-engine/errorhandling.html) om du vill ha mer information om hantering av delningsfel.

>[!NOTE]
>
>På en författarinstans är [CQ WCM-felsökningsfiltret](/help/sites-deploying/osgi-configuration-settings.md) aktiverat som standard. Detta resulterar alltid i svarskoden 200. Standardfelhanteraren svarar genom att skriva den fullständiga stackspårningen till svaret.
>
>I en publiceringsinstans är CQ WCM-felsökningsfiltret *alltid* inaktiverat (även om det har konfigurerats som aktiverat).

## Anpassa sidor som visas av felhanteraren {#how-to-customize-pages-shown-by-the-error-handler}

Du kan utveckla egna skript för att anpassa sidorna som visas i felhanteraren när ett fel inträffar. Dina anpassade sidor skapas under `/apps` och täcker över standardsidorna (som finns under `/libs`).

>[!NOTE]
>
>Mer information finns i [Använda övertäckningar](/help/sites-developing/overlays.md).

1. Kopiera standardskripten i databasen:

   * från `/libs/sling/servlet/errorhandler/`
   * till `/apps/sling/servlet/errorhandler/`

   Eftersom målsökvägen inte finns som standard måste du skapa den första gången du gör det.

1. Navigera till `/apps/sling/servlet/errorhandler` och gör något av följande:

   * redigera det befintliga skriptet så att du kan ange den information som behövs.
   * skapa och redigera ett nytt skript för den kod som behövs.

1. Spara ändringarna och testa.

>[!CAUTION]
>
>Hanterarna 404.jsp och 403.jsp har utformats för att hantera CQ5-autentisering, särskilt för att tillåta systeminloggning om något av dessa fel inträffar.
>
>Därför bör dessa två hanterare bytas ut med stor försiktighet.

### Anpassa svaret till HTTP 500-fel {#customizing-the-response-to-http-errors}

HTTP 500-fel orsakas av undantag på serversidan.

* **[500 Internt serverfel](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)**
Servern påträffade ett oväntat tillstånd som gjorde att den inte kunde slutföra begäran.

När bearbetningen av begäran resulterar i ett undantag, Apache Sling-ramverket (som AEM är byggt på):

* loggar undantaget
* returnerar:

   * HTTP-svarskod 500
   * stackspårning för undantag

  i svarets brödtext.

Genom att [anpassa sidorna som visas av felhanteraren](#how-to-customize-pages-shown-by-the-error-handler) kan ett `500.jsp`-skript skapas. Den används dock bara om `HttpServletResponse.sendError(500)` körs explicit, det vill säga från en undantagskatalog.

Annars är svarskoden inställd på 500, men skriptet `500.jsp` körs inte.

Om du vill hantera 500 fel måste filnamnet för felhanterarskriptet vara detsamma som undantagsklassen (eller superklassen). Om du vill hantera alla sådana undantag kan du skapa ett skript `/apps/sling/servlet/errorhandler/Throwable.js`p eller `/apps/sling/servlet/errorhandler/Exception.jsp`.

>[!CAUTION]
>
>På en författarinstans är [CQ WCM-felsökningsfiltret](/help/sites-deploying/osgi-configuration-settings.md) aktiverat som standard. Detta resulterar alltid i svarskoden 200. Standardfelhanteraren svarar genom att skriva den fullständiga stackspårningen till svaret.
>
>För en anpassad felhanterare krävs svar med kod 500, så [CQ WCM Debug Filter måste inaktiveras](/help/sites-deploying/osgi-configuration-settings.md). Detta garanterar att svarskoden 500 returneras, vilket i sin tur utlöser rätt Sling-felhanterare.
>
>I en publiceringsinstans är CQ WCM-felsökningsfiltret *alltid* inaktiverat (även om det har konfigurerats som aktiverat).
