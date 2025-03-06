---
title: Integrera med Adobe Experience Cloud
description: Lär dig integrera Adobe Experience Manager med Adobe Experience Cloud.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: 456bcdf5-3d43-43d8-b243-70095e0cf58c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 0%

---

# Integrera med Adobe Experience Cloud{#integrating-with-the-adobe-marketing-cloud}

[Adobe Experience Cloud](https://business.adobe.com/products/marketing-cloud/main.html) innehåller kraftfulla webbanalyser och webboptimeringsprodukter som levererar användbara realtidsdata och insikter för att driva framgångsrika onlineinitiativ. Det erbjuder en integrerad och öppen plattform för optimering av onlineverksamhet. Molnet består av integrerade program som samlar in och släpper loss kraften i kundinsikterna för att optimera kundvärvning, konvertering och lojalitet samt skapar och distribuerar innehåll.

Med Adobe Experience Manager (AEM) kan du smidigt integrera med följande Adobe Experience Cloud-produkter:

* Adobe Analytics förser marknadsförarna med användbar realtidsinformation om onlinestrategier och marknadsföringsinitiativ.
* Adobe Target ger marknadsförarna möjlighet att kontinuerligt göra sitt webbinnehåll mer relevant för sina kunder, vilket ger större konverteringsgrad.
* Adobe Dynamic Media Classic automatiserar mediehanteringen, effektiviserar webbpubliceringen och förbättrar webbupplevelserna - allt i en hostingmiljö.
* Adobe Dynamic Tag Management ger marknadsförarna intuitiva verktyg för att snabbt och enkelt hantera ett obegränsat antal taggar från Adobe och externa leverantörer.
<!-- Search&Promote is end of life as of September 1, 2022 * Adobe Search&Promote gives marketers the ability to control and optimize the search results on their sites. -->
* Med Adobe Campaign kan ni hantera e-postleveransen direkt i Adobe Experience Manager.

Dessutom kan du [integrera AEM med Creative Cloud](/help/assets/aem-cc-integration-best-practices.md) och med [tredjepartstjänster](/help/sites-administering/third-party-services.md).

## Integrera med Adobe Analytics {#integrating-with-adobe-analytics}

[Adobe Analytics](https://business.adobe.com/products/analytics/adobe-analytics.html) är den branschledande lösningen som ger digitala marknadsförare en plats där de kan mäta, analysera och optimera integrerade data från alla onlineinitiativ över flera marknadsföringskanaler. Det ger marknadsförarna användbar webbanalysinformation i realtid om digitala strategier och marknadsföringsinitiativ. Adobe Analytics hjälper marknadsförarna att snabbt identifiera de mest lönsamma vägarna via en webbplats, segmentera trafiken för att hitta värdefulla webbbesökare, avgöra var besökarna navigerar bort från webbplatsen och identifiera viktiga framgångsmått för webbkampanjer.

Du kan använda Adobe Analytics för att analysera data från dina webbplatser.

Genom att integrera med Adobe Analytics kan du göra följande:

* Aktivera användarspårning i Analytics.
* Mappa körningslägena (till exempel författare, publicera) till olika rapportsviter.
* Skicka klientkontextvariabler som konverteringsvariabler eller trafikegenskaper.
* Använd fördefinierade variabelmappningar.
* Konfigurera hela webbplatsavsnitt samtidigt.
* Spåra anpassade händelser.

Mer information om hur du integrerar AEM med Analytics finns i [Integrera med Adobe Analytics](/help/sites-administering/adobeanalytics.md).

Du kan också använda guiden [Anmäl dig](/help/sites-administering/opt-in.md) för att enkelt utföra integreringen.

## Integrera med Adobe Target {#integrating-with-adobe-target}

[Adobe Target](https://business.adobe.com/products/target/adobe-target.html) används av marknadsförare för att utforma och köra onlinetester, skapa direktsända målgruppssegment (baserat på beteende) och automatisera målgruppsanpassningen för innehåll och onlineupplevelser.

Dagens onlinekonsumenter har ständigt nya behov och förväntar sig relevant, till och med personaliserat innehåll från en mängd olika webbplatser och innehållskällor som de kan välja bland. För att engagera en webbpublik är det viktigt att marknadsförarna snabbt kan identifiera vilka erbjudanden och vilket innehåll som är relevanta och övertygande för deras målgrupper. Med denna kunskap i ryggen behöver marknadsförarna möjlighet att kontinuerligt utveckla sina webbplatser och rikta lämpligt innehåll till olika målgrupper.

[Integreringen med Adobe Target](/help/sites-administering/target.md) förklarar hur du integrerar din webbplats med Target.

Du kan också använda guiden [Anmäl dig](/help/sites-administering/opt-in.md) för att enkelt utföra integreringen.

## Anmäl dig till Analytics och Target {#opting-in-to-analytics-and-target}

AEM erbjuder en enkel anmälningsprocedur för integrering med Adobe Analytics och Adobe Target. När du loggar in som administratör och går till projektkonsolen visas en anmälningsguide.

![chlimage_1-107](assets/chlimage_1-107a.png)

Anmäl dig till integreringen med Analytics och/eller Target för att göra det möjligt att använda deras funktioner för sidspårning och sidanalys samt personaliseringsfunktioner. När du väljer att logga in anger du din användarkontoinformation och de sidor som spåras.

Mer information finns i [Öppna Adobe Analytics och Adobe Target.](/help/sites-administering/opt-in.md)

## Integrera med Adobe Dynamic Media Classic {#integrating-with-scene}

Adobe Dynamic Media Classic är en värdbaserad lösning för publicering, hantering, förbättring och leverans av dynamiskt marknadsföringsmaterial och visuell marknadsföring på webben, mobiler, e-post, sociala medier, internetanslutna displayer och tryck.

I Adobe Experience Manager kan du publicera digitala resurser direkt från Adobe Experience Manager till Dynamic Media Classic och du kan publicera digitala resurser från Dynamic Media Classic till Adobe Experience Manager.

Dessutom kan du visa Adobe Experience Manager-resurser som publicerats i Dynamic Media Classic i olika visningsprogram, till exempel Grundläggande zoom och Video.

Mer information om hur Adobe Experience Manager kan integreras med Dynamic Media Classic finns i [Integrera med Dynamic Media Classic](/help/sites-administering/scene7.md) -dokumentationen.

## Integrera med Adobe Dynamic Tag Management {#integrating-with-adobe-dynamic-tag-management}

[Adobe Dynamic Tag Management](https://business.adobe.com/products/experience-platform/adobe-experience-platform.html) ger marknadsförarna intuitiva verktyg för att snabbt och enkelt hantera ett obegränsat antal taggar från Adobe och tredje part. Du får större kontroll och flexibilitet att optimera praktiskt taget allt på webben samtidigt som du minskar beroendet av IT-resurser.

[Integrera Adobe Dynamic Tag Management](/help/sites-administering/dtm.md) med AEM så att du kan använda dina dynamiska Tag Management-webbegenskaper för att spåra AEM webbplatser.

## Integrera med Adobe Audience Manager {#integrating-with-adobe-audience-manager}

Integreringen med Audience Manager har tagits bort i AEM 6.3.

<!-- Search&Promote is end of life as of September 1, 2022 ## Integrating with Search&Promote {#integrating-with-search-promote} -->

<!-- Search&Promote is end of life as of September 1, 2022 Adobe Search&Promote enables marketers to optimizehow visitors browse, find, compare, and select relevant products and content on web and mobile sites. Businesses can easily promote priority items based on business objectives and visitor intent, and automate merchandising and promotions activity via KPI-based triggers or metrics. -->

<!-- Search&Promote is end of life as of September 1, 2022 Adobe Search&Promote is a reliable and scalable hosted site search application, capable of scaling to millions of pages or products, for heavily visited online businesses ranging from retail to news sites. It offers unprecedented levels of marketer control and metrics-based relevance. -->

<!-- Search&Promote is end of life as of September 1, 2022 For information about integrating AEM and Search&Promote, see [Integrating with Adobe Search&Promote](/help/sites-administering/search-and-promote.md). -->

## Integrera med Adobe Campaign {#integrating-with-adobe-campaign}

Med [Adobe Campaign](https://business.adobe.com/products/campaign/adobe-campaign.html) kan du hantera e-postleveransinnehåll direkt i Adobe Experience Manager.

Mer information om hur AEM integreras med Adobe Campaign finns i [Integrera med Adobe Campaign](/help/sites-administering/campaignstandard.md).
