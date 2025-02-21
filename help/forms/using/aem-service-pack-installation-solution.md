---
title: Fel som inte är tillgängliga för CRX/bundle och Start-sidtjänsten när det senaste 6.5.15.0-tjänstpaketet har installerats
description: Fel som inte är tillgängliga för CRX/bundle och Start-sidtjänsten när det senaste 6.5.15.0-tjänstpaketet har installerats
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms,AEM Forms on JEE,AEM Forms on OSGi
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Tjänsten är inte tillgänglig efter installation av AEM (6.5.15.0) Service Pack {#steps-to-resolve-error-after-installing-service-pack}

## Problem {#issue}

När du har installerat [AEM 6.5.15.0 Service Pack](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack/aem-service-pkg-6.5.15.0.zip) inträffar felet som:
* FEL [FelixDispatchQueue] org.apache.sling.scripting.console FrameworkEvent FEL (org.osgi.framework.BundleException: Det går inte att matcha org.apache.sling.scripting.console

När du har installerat Service Pack för AEM 6.5.15.0 visas fel som inte är tillgängliga på CRX-/bundle- och startsidan.

## Gäller för {#applies-to}

Denna lösning gäller
* AEM Forms på alla JEE-servrar utom de som körs på JBoss EAP 7.4.0

## Lösning {#solution}

>[!NOTE]
>
>Felsökningsstegen gäller alla programservrar utom JBoss EAP 7.4.

När du har installerat [AEM 6.5.15.0 Service Pack](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack/aem-service-pkg-6.5.15.0.zip) och CRX/bundle och startsidan visar fel som inte är tillgängliga utför du följande steg:

1. Stoppa programservern.
1. Navigera till `[aem-forms root]\crx-repository\launchpad\felix\bundle52`.
1. Leta reda på filen `bundle.info`.
1. Öppna filen `bundle.info` i redigeraren för ant-text och sök efter paketnamnet som `org.apache.felix.http.bridge`.

   >[!NOTE]
   >
   >Om `bundle.info` under `bundle52` inte innehåller paketet `org.apache.felix.http.bridge` kontrollerar du paketnumret inom hakparentes bredvid `org.apache.felix.http.bridge`. Navigera sedan till [aem-forms root]\crx-repository\launchpad\felix\bundle[x] och utför nästa steg på den här platsen.

1. Navigera till URL: `[aem-forms root]\crx-repository\launchpad\felix\bundle[x]\version0.1`.
1. Sök efter `bundle.jar` och byt namn på `bundle.jar` till `bundle.jar.bak`.
1. Kopiera `Bundle for AEM 6.5 Forms on JEE Service Pack 15` på den här platsen från [Programvarudistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/featurepack/bundle.jar).
1. Starta programservern, vänta på att loggarna ska stabiliseras och kontrollera paketläget.
1. När alla paket är aktiva installerar du [fragmentet för AEM 6.5 Forms på JEE Service Pack 15](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/featurepack/org.apache.felix.http.servlet-api-1.2.0_fragment_full.jar) från `system/console/bundles` och väntar på att programservern ska stabiliseras.
1. Stoppa programservern.
1. Navigera till `[aem-forms root]\crx-repository\launchpad\felix\bundle52\version0.1` och ta bort `bundle.jar`.
1. Byt namn på `bundle.jar.bak` till `bundle.jar`.
1. Starta programservern.
