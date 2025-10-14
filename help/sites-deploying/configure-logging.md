---
title: Loggning
description: Lär dig hur du konfigurerar globala parametrar för den centrala loggningstjänsten, specifika inställningar för enskilda tjänster eller hur du begär dataloggning.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 86613671-dacd-487e-b6ff-88365289e591
source-git-commit: 408f6aaedd2cc0315f6e66b83f045ca2716db61d
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 0%

---

# Loggning{#logging}

AEM ger dig möjlighet att konfigurera:

* globala parametrar för den centrala loggningstjänsten
* begär dataloggning; en särskild loggningskonfiguration för begärandeinformation
* specifika inställningar för de enskilda tjänsterna, till exempel en enskild loggfil och ett format för loggmeddelandena

Det här är alla [OSGi-konfigurationer](/help/sites-deploying/configuring-osgi.md).

>[!NOTE]
>
>Inloggning i AEM baseras på Sling-principer. Mer information finns i [Sling Logging](https://sling.apache.org/site/logging.html).

## Global loggning {#global-logging}

[Loggningskonfiguration för Apache Sling](/help/sites-deploying/osgi-configuration-settings.md) används för att konfigurera rotloggaren. Detta definierar de globala inställningarna för inloggning i AEM:

* loggningsnivån
* platsen för den centrala loggfilen
* antalet versioner som ska behållas
* versionsrotation; antingen maximal storlek eller ett tidsintervall
* det format som ska användas när loggmeddelanden skrivs

## Loggare och skribenter för enskilda tjänster {#loggers-and-writers-for-individual-services}

Förutom de globala loggningsinställningarna kan du i AEM konfigurera specifika inställningar för en enskild tjänst:

* den specifika loggningsnivån
* platsen för den enskilda loggfilen
* antalet versioner som ska behållas
* versionsrotation; antingen maximal storlek eller tidsintervall
* det format som ska användas när loggmeddelanden skrivs
* loggaren (OSGi-tjänsten som tillhandahåller loggmeddelanden)

På så sätt kan du kanalisera loggmeddelanden för en enskild tjänst till en separat fil. Detta kan vara särskilt användbart under utveckling eller testning, till exempel när du behöver en högre loggnivå för en viss tjänst.

AEM använder följande för att skriva loggmeddelanden till filen:

1. En **OSGi-tjänst** (logger) skriver ett loggmeddelande.
1. En **loggningsloggare** tar emot det här meddelandet och formaterar det enligt din specifikation.
1. Ett **loggningsskrivprogram** skriver alla dessa meddelanden till den fysiska filen som du har definierat.

Dessa element är länkade med följande parametrar för de relevanta elementen:

* **Logger (loggningslogg)**

  Definiera de tjänster som genererar meddelandena.

* **Loggfil (loggningslogg)**

  Definiera den fysiska filen för lagring av loggmeddelanden.

  Detta används för att länka en loggningslogg till en loggningsförfattare. Värdet måste vara identiskt med samma parameter i loggningsskrivarens konfiguration för att anslutningen ska kunna upprättas.

* **Loggfil (loggningsförfattare)**

  Definiera den fysiska fil som loggmeddelandena ska skrivas till.

  Detta måste vara identiskt med samma parameter i loggningsskrivarkonfigurationen, annars görs ingen matchning. Om det inte finns någon matchning skapas ett implicit skrivprogram med standardkonfigurationen (daglig loggrotation).

### Standardloggare och -författare {#standard-loggers-and-writers}

Vissa loggare och skrivprogram ingår i en standardinstallation av AEM.

Det första är ett specialfall eftersom det styr både `request.log`- och `access.log`-filerna:

* Loggaren:

   * Dataloggning för anpassningsbara Apache Sling-begäranden

     (org.apache.sling.engine.impl.log.RequestLoggerService)

   * Skriv meddelanden om begärandeinnehåll till `request.log`.

* Länkar till:

   * Apache Sling Request Logger

     (org.apache.sling.engine.impl.log.RequestLogger)

   * Skriver meddelanden till antingen `request.log` eller `access.log`.

Dessa kan anpassas vid behov, även om standardkonfigurationen är lämplig för de flesta installationer.

De andra paren följer standardkonfigurationen:

* Loggaren:

   * Konfiguration av loggningsloggare för Apache Sling

     (org.apache.sling.Commons.log.LogManager.factory.config)

   * Skriver `Information` meddelanden till `logs/error.log`.

* Länkar till skrivprogrammet:

   * Konfiguration av skrivprogram för Apache Sling Logging

     (org.apache.sling.Commons.log.LogManager.factory.writer)

* Loggaren:

   * Konfiguration av loggningsloggare för Apache Sling
(org.apache.sling.Commons.log.LogManager.factory.config.649d51b7-6425-45c9-81e6-2697a03d6be7)

   * Skriver `Warning` meddelanden till `../logs/error.log` för tjänsten `org.apache.pdfbox`.

* Länkar inte till ett specifikt skrivprogram, så skapar och använder ett implicit skrivprogram med standardkonfiguration (daglig loggrotation).

### Skapa egna loggare och författare {#creating-your-own-loggers-and-writers}

Du kan definiera ett eget par för loggare/skrivare:

1. Skapa en instans av fabrikskonfigurationen [Konfiguration av loggningsloggare för Apache Sling](/help/sites-deploying/osgi-configuration-settings.md).

   1. Ange loggfilen.
   1. Ange loggaren.
   1. Konfigurera de andra parametrarna efter behov.

1. Skapa en instans av fabrikskonfigurationen [Konfiguration för Apache Sling Logging Writer &#x200B;](/help/sites-deploying/osgi-configuration-settings.md).

   1. Ange loggfilen - den måste matcha den som har angetts för loggaren.
   1. Konfigurera de andra parametrarna efter behov.

>[!NOTE]
>
>Under vissa omständigheter kanske du vill skapa en [anpassad loggfil](/help/sites-deploying/monitoring-and-maintaining.md#create-a-custom-log-file).
