---
title: Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS
description: Detta är den aktuella versionsinformationen för Adobe Experience Manager 6.5 LTS.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: b5a8f555-c061-4fe2-a100-cc01335959cb
source-git-commit: 8f6d152ceeae12cdadd0096e114584ce2a63a2ac
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 5%

---

# Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS {#release-notes}

## Versionsinformation {#release-information}

| Produkt | [!DNL Adobe Experience Manager] |
|---|---|
| Version | 6,5 LTS |
| Typ | Större release |
| Allmänt tillgänglighetsdatum | 7 mars 2025 |

## Nyheter {#what-s-new}

[!DNL Adobe Experience Manager] 6.5 LTS är en uppgraderingsversion till [!DNL Adobe Experience Manager] 6.5-kodbasen. Det innehåller viktiga kundkorrigeringar, högprioriterade kundförbättringar och allmänna felkorrigeringar som är inriktade på produktstabilisering. Den innehåller även [!DNL Adobe Experience Manager] 6.5 Service Pack-versioner upp till SP22.

Listan nedan innehåller en översikt, medan de efterföljande sidorna innehåller fullständig information.

### [!DNL Experience Manager Foundation] {#experience-manager-foundation}

Plattformen för [!DNL Adobe Experience Manager] 6.5 LTS bygger på uppdaterade versioner av det OSGi-baserade ramverket (Apache Sling och Apache Felix) och Java™ Content Repository: Apache Jackrabbit Oak 1.68.x.

Quickstart använder Eclipse Jetty 11.0.x som servermotor.

#### Stöd för Java™  {#java-support}

* Stöd för Java™ 17 och Java™ 21.
* För optimala prestanda bör du åsidosätta GC-standardvärden med andra värden. Mer information finns i avsnittet [Installera och uppdatera](/help/sites-deploying/custom-standalone-install.md).
* Underhållsuppdateringar för Java™ 17 och Java™ 21 distribueras av Adobe för användning i AEM-relaterade projekt, när de inte är tillgängliga för allmänheten från Oracle.

#### Paketering med Uberjar {#uber-jar-packaging}

* Det finns en liten skillnad i förpackningen till AEM 6.5 LTS. Mer information finns i [Uppdatera AEM Uber Jar-versionen](/help/sites-deploying/upgrading-code-and-customizations.md#update-the-aem-uber-jar-version).

#### Uppgradera {#upgrade}

* Mer information om uppgraderingsproceduren finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

## Installera och uppdatera {#install-update}

Installationsanvisningar finns i [Installationsanvisningar](/help/sites-deploying/custom-standalone-install.md).

Detaljerade instruktioner finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

## Plattformar som stöds {#supported-platforms}

Hitta den fullständiga matrisen med plattformar som stöds, inklusive supportnivå på [AEM 6.5 LTS Technical Requirements](/help/sites-deploying/technical-requirements.md).

>[!NOTE]
>
>Java™ 17/Java™ 21 rekommenderas för AEM 6.5 LTS.

## Föråldrade och borttagna funktioner {#deprecated-and-removed-features}

Adobe utvärderar ständigt produktfunktioner för att så småningom förnya eller ersätta äldre funktioner med modernare alternativ för att förbättra det totala kundvärdet, alltid med noggrant övervägande av bakåtkompatibilitet.

Följande regler gäller för att informera om den förestående borttagningen eller ersättningen av Adobe Experience Manager-funktioner (AEM):

1. Föråldringsanmälan kommer först. Funktionerna är fortfarande tillgängliga, men har inte förbättrats ytterligare.
1. Borttagning av föråldrade funktioner sker tidigast i följande större version. Faktiskt måldatum för borttagning meddelas senare.

Den här processen ger kunderna minst en releasecykel för att anpassa implementeringen till en ny version eller en efterföljare till en borttagningsfunktion, innan den faktiska borttagningen.

### Föråldrade funktioner {#deprecated-features}

I det här avsnittet listas funktioner som har markerats som borttagna i AEM 6.5 LTS. I allmänhet är funktioner som är planerade att tas bort i en framtida version först inaktuella, med ett alternativ.

Kunderna rekommenderas att granska om de använder funktionen/funktionen i den aktuella distributionen och planera för att ändra implementeringen så att den använder det alternativ som erbjuds.

| Område | Funktion | Ersättning | Version (SP) |
|---|---|---|---|
| Sites | [SPA-redigerare](/help/sites-developing/spa-overview.md) | De redigerare som rekommenderas för att hantera headless-innehåll i AEM är:<br>- [Universell redigerare](/help/sites-developing/universal-editor/introduction.md) för visuell redigering.<br>- [Innehållsfragmentredigeraren](/help/assets/content-fragments/content-fragments-managing.md) för formulärbaserad redigering. | 6,5 LTS GA |

### Borttagna funktioner {#removed-features}

I det här avsnittet listas funktioner som har tagits bort från AEM 6.5 LTS. Tidigare versioner hade dessa funktioner markerats som föråldrade.

| Område | Funktion | Ersättning | Version (SP) |
|--- |--- |--- |--- |
| Commerce | AEM CIF Classic stöds inte. | Du bör migrera till [AEM CIF](/help/commerce/cif/migration.md). | 6,5 LTS GA |
| Lösningar | Social/Communities stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Screens | Screens stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Assets | `dam-pim` och `dam-rating` stöds inte eftersom paket är beroende av sociala medier. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Assets | `com.day.cq.dam.scene7.api.model.Scene7ViewerConfig#getSettings()` har tagits bort. | Använd den alternativa API `com.day.cq.dam.scene7.api.model.Scene7ViewerConfig#getSettingsList()` som har lagts till. | 6,5 LTS GA |
| Portal | AEM Portal Director stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | Paketet `com.adobe.granite.socketio` har tagits bort. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | `com.adobe.granite.crx-explorer` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | `crx2oak` stöds inte. | Välj relevant version av [oak-upgrade](https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-upgrade) | 6,5 LTS GA |
| Adobe | `com.adobe.cq.cq-searchpromote-integration` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Guava | Alla guava-beroenden har nu tagits bort i AEM och därför ingår inte paketet `com.adobe.granite.osgi.wrapper.guava-15.0.0-0002` i AEM. | Kunderna kan själva lägga till guava om de är beroende av guava eller ersätta guava-kod med java-samlingar eller andra alternativ om det är möjligt. | 6,5 LTS GA |
| We.Retail | Exempelsajten för webben stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | Paketet `oak-solr-osgi` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | `org.apache.servicemix.bundles.abdera-parser`, `org.apache.servicemix.bundles.jdom` och `org.apache.sling.atom.taglib` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | `org.apache.commons.io` paket exporteras nu från `org.apache.commons.commons-io`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `javax.mail` paket exporteras från paketet `com.sun.javax.mail`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `org.apache.jackrabbit.api` paket exporteras nu från paketet `org.apache.jackrabbit.oak-jackrabbit-api`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `com.github.jknack.handlebars` stöds inte | Välj relevant [version](https://mvnrepository.com/artifact/com.github.jknack/handlebars) | 6,5 LTS GA |

## Kända fel {#known-issues}

### Dispatcher-anslutningsfel med SSL-funktion {#ssl-only-feature}

När du aktiverar SSL-funktionen i AEM-distributioner finns det ett känt problem som påverkar anslutningen mellan dispatchern och AEM-instanserna. När den här funktionen har aktiverats kan hälsokontroller misslyckas och kommunikationen mellan dispatchern och AEM-instanser kan störas.

**Effekt:**
* Misslyckade hälsokontroller med HTTP 500-svarskoder
* Trafiken mellan dispatcher och AEM-instanser har brutits
* Innehållet kan inte hanteras korrekt via dispatchern

**Berörda miljöer:**
* AEM distribuerar med dispatcherkonfigurationer
* System där SSL-funktionen har aktiverats

**Lösning:**
Kontakta Adobe kundsupport om du får det här problemet. Det finns en snabbkorrigering [cq-6.5.lts.0-hotfix-CQ-4359803](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-CQ-4359803-1.0.0.zip) som åtgärdar det här problemet. Försök inte aktivera SSL-funktioner förrän du har implementerat den nödvändiga snabbkorrigeringen.

## Begränsade webbplatser{#restricted-sites}

Dessa webbplatser är bara tillgängliga för kunder. Kontakta din kontoansvarige på Adobe om du är kund och behöver åtkomst.

* [Nedladdning av produkt på licensing.adobe.com](https://licensing.adobe.com/)
* [Kontakta Adobe kundsupport](https://experienceleague.adobe.com/en/docs/customer-one/using/home).

