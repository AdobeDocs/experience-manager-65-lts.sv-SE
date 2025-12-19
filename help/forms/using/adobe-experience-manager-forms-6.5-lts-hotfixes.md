---
title: Programfixar för Adobe Experience Manager Forms 6.5 LTS SP1
description: Innehåller information om hur du hämtar och installerar en programfix för AEM Forms 6.5 LTS.
exl-id: 37287332-3c8d-4ddc-a77e-3c5ee332898b
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
source-git-commit: 6ca845ce5f4b97bfc5a360b3426f7284fb9cd401
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# Programfixar för Adobe Experience Manager Forms 6.5 LTS{#aem-form-hotfix}

I den här artikeln listas de viktiga korrigeringar som har implementerats för att åtgärda kända fel, förbättra systemstabiliteten och förbättra prestanda i AEM Forms 6.5 LTS.

>[!NOTE]
>
> Programfixarna är avsedda att vara kumulativa och omfattar alla föregående korrigeringar. När du tillämpar den senaste snabbkorrigeringen på en release åtgärdar den inte bara det senaste problemet utan även alla tidigare felkorrigeringar och förbättringar.

## Programfixar för AEM Forms 6.5 LTS {#hotfix-for-aem-forms}

<table>
  <tbody>
  <tr>
    <td><strong>Datum</strong></td>
    <td><strong>Länk för hämtning av snabbkorrigeringar (länken AEM Software Distribution)</strong></td>
    <td><strong>Åtgärdade problem</strong></td>
  </tr>
  <tr>
    <td>
      <strong>9 sept 2025</strong><br>
    <td>
    <ul>
    <li>Windows- <a href="https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?pack[...]1-hotfix-on-add-on/adobe-aemfd-win-pkg-6.1.176-RHF-002.zip">Programfix2 för AEM Service Pack 6.5 LTS i Windows</a></li>
    <li>Linux- <a href="https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?pack[...]hotfix-on-add-on/adobe-aemfd-linux-pkg-6.1.176-RHF-002.zip">Hotfix2 för AEM Service Pack 6.5 LTS i Linux</a></li>
     <li>MacOS- <a href="https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?pack[...]1-hotfix-on-add-on/adobe-aemfd-osx-pkg-6.1.176-RHF-002.zip">Programfix2 för AEM Service Pack 6.5 LTS på MacOS</a></li>
    <td>
    <ul>
    <li>Förbättrad tillförlitlighet när det gäller inskickning av formulär genom att åtgärda ett problem där inskickning kan misslyckas när serversidesvalidering (SSV) har aktiverats Om du får problem kontaktar du [Adobe Experience Manager Forms Support](https://business.adobe.com/in/support/main.html)
    </li>
    </ul>
    </td>    
  </tr>
    </ul>
    </td>    
  </tr>
  <tbody>
</table>

## Hämta och installera en OSGi-hotfix {#download-install-hotfix}

Utför följande steg för att hämta och installera programfixen:

1. Hämta [snabbkorrigering](#hotfix-for-adaptive-forms) från länken för programdistribution.
1. Extrahera Hotfix-arkivfilen så att du kan hämta Experience Manager-paketfiler (.zip) och paketfiler (.jar).
1. Överför och installera paketet (.zip) via [Package Manager](https://experienceleague.adobe.com/docs/experience-manager-65/content/sites/administering/contentmanagement/package-manager.html?lang=es#accessing).
1. Öppna konfigurationshanterarpaketen `https://server:host/system/console/bundles`, ladda upp och installera paketet (.jar). Programfixen är installerad.
