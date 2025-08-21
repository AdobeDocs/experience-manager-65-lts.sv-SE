---
title: Versionsinformation för  [!DNL Adobe Experience Manager] 6.5 LTS
description: Hitta aktuella versionsuppgifter för Adobe Experience Manager 6.5 LTS.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: 70436606-d95c-4208-94f6-e33f3eefdf66
source-git-commit: 7f9f24f173604640b454449b389da9fcdcf7017d
workflow-type: tm+mt
source-wordcount: '1068'
ht-degree: 1%

---

# Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS {#release-notes}

## Versionsinformation {#release-information}

| Produkt | [!DNL Adobe Experience Manager] |
|---|---|
| Version | 6,5 LTS |
| Typ | Större release |
| Allmän tillgänglighet | 7 mars 2025 |

## Nyheter {#what-s-new}

[!DNL Adobe Experience Manager] 6.5 LTS är en uppgraderingsversion till [!DNL Adobe Experience Manager] 6.5-kodbasen. Det innehåller viktiga kundkorrigeringar, högprioriterade kundförbättringar och allmänna felkorrigeringar som är inriktade på produktstabilisering. Den innehåller även [!DNL Adobe Experience Manager] 6.5 Service Pack-versioner upp till SP22.

Listan nedan innehåller en översikt, medan de efterföljande sidorna innehåller fullständig information.

### [!DNL Experience Manager Foundation] {#experience-manager-foundation}

Plattformen för [!DNL Adobe Experience Manager] 6.5 LTS bygger på uppdaterade versioner av det OSGi-baserade ramverket (Apache Sling och Apache Felix) och Java™ Content Repository: Apache Jackrabbit Oak 1.68.x.

Eclipse Jetty 11.0.x används som servermotor för QuickStart.

#### Stöd för Java™  {#java-support}

* Stöd för Java™ 17 och Java™ 21.
* För optimala prestanda bör du åsidosätta standardvärdena för GC med andra värden. Mer information finns i avsnittet [Installera och uppdatera](/help/sites-deploying/custom-standalone-install.md).
* Adobe distribuerar Java™ 17- och Java™ 21-underhållsuppdateringar för kundanvändning i AEM-relaterade projekt, när dessa inte är tillgängliga för allmänheten från Oracle.

#### Uberjar-förpackning {#uber-jar-packaging}

* Det finns en liten skillnad i förpackningen till AEM 6.5 LTS. Mer information finns i [Uppdatera AEM Uber Jar-versionen](/help/sites-deploying/upgrading-code-and-customizations.md#update-the-aem-uber-jar-version).

#### Uppgradera {#upgrade}

* Mer information om uppgraderingsproceduren finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

## Installera och uppdatera {#install-update}

Installationsanvisningar finns i [Installationsanvisningar](/help/sites-deploying/custom-standalone-install.md).

Detaljerade instruktioner finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

>[!NOTE]
>
> För nya AEM 6.5 LTS-installationer måste indexdefinitioner installeras separat. Mer information finns i [den här artikeln](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#index-definitions).

## Plattformar som stöds {#supported-platforms}

Hitta den fullständiga matrisen med plattformar som stöds, inklusive supportnivå på [AEM 6.5 LTS Technical Requirements](/help/sites-deploying/technical-requirements.md).

>[!NOTE]
>
>Java™ 17/Java™ 21 rekommenderas för AEM 6.5 LTS.

## Föråldrade och borttagna funktioner {#deprecated-and-removed-features}

Adobe granskar kontinuerligt produktfunktionerna för att förbättra kundens värde genom att modernisera eller ersätta äldre funktioner. Dessa ändringar görs med stor noggrannhet för bakåtkompatibilitet.

Följande regler gäller för att informera om den förestående borttagningen eller ersättningen av Adobe Experience Manager-funktioner (AEM):

1. Föråldringsanmälan kommer först. Funktionerna är fortfarande tillgängliga, men har inte förbättrats ytterligare.
1. Borttagning av föråldrade funktioner sker tidigast i följande större version. Det faktiska måldatumet för borttagning planeras att tillkännages senare.

Den här processen ger kunderna minst en releasecykel för att anpassa implementeringen till en ny version eller en efterföljare till en borttagningsfunktion, innan den faktiska borttagningen.

### Föråldrade funktioner {#deprecated-features}

I det här avsnittet listas funktioner som Adobe har ersatt i AEM 6.5 LTS. Normalt tar Adobe bort funktioner innan de tas bort i en framtida version och erbjuder ett alternativ.


Kunderna rekommenderas att granska om de använder funktionen/funktionen i den aktuella distributionen och planera för att ändra implementeringen så att den använder det alternativ som erbjuds.

| Område | Funktion | Ersättning | Version (SP) |
|---|---|---|---|
| Sites | [SPA-redigerare](/help/sites-developing/spa-overview.md) | De redigerare som rekommenderas för att hantera headless-innehåll i AEM är:<br>- [Universell redigerare](/help/sites-developing/universal-editor/introduction.md) för visuell redigering.<br>- [Innehållsfragmentredigeraren](/help/assets/content-fragments/content-fragments-managing.md) för formulärbaserad redigering. | 6,5 LTS GA |

### Borttagna funktioner {#removed-features}

I det här avsnittet listas funktioner som har tagits bort från AEM 6.5 LTS. Tidigare versioner hade dessa funktioner markerats som föråldrade.

| Område | Funktion | Ersättning | Version (SP) |
|--- |--- |--- |--- |
| Commerce | AEM CIF Classic stöds inte. | Migrera till [AEM CIF](/help/commerce/cif/migration.md). | 6,5 LTS GA |
| Lösningar | Social/Communities stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Screens | Screens stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Assets | `dam-pim` och `dam-rating` stöds inte eftersom paket är beroende av sociala medier. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Assets | `com.day.cq.dam.scene7.api.model.Scene7ViewerConfig#getSettings()` har tagits bort. | Använd den alternativa API `com.day.cq.dam.scene7.api.model.Scene7ViewerConfig#getSettingsList()` som har lagts till. | 6,5 LTS GA |
| Portal | AEM Portal Director stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | Paketet `com.adobe.granite.socketio` har tagits bort. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | `com.adobe.granite.crx-explorer` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | `crx2oak` stöds inte. | Välj den relevanta versionen av [Oak-upgrade](https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-upgrade) | 6,5 LTS GA |
| Adobe | `com.adobe.cq.cq-searchpromote-integration` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Guava | Alla guava-beroenden har nu tagits bort i AEM och därför ingår inte paketet `com.adobe.granite.osgi.wrapper.guava-15.0.0-0002` i AEM. | Kunderna kan själva lägga till guava om de är beroende av guava eller ersätta guava-kod med java-samlingar eller andra alternativ om det är möjligt. | 6,5 LTS GA |
| `We.Retail` | `We-retail` exempelplats stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | Paketet `oak-solr-osgi` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | `org.apache.servicemix.bundles.abdera-parser`, `org.apache.servicemix.bundles.jdom` och `org.apache.sling.atom.taglib` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | `org.apache.commons.io` paket exporteras nu från `org.apache.commons.commons-io`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `javax.mail` paket exporteras från paketet `com.sun.javax.mail`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `org.apache.jackrabbit.api` paket exporteras nu från paketet `org.apache.jackrabbit.oak-jackrabbit-api`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `com.github.jknack.handlebars` stöds inte | Välj relevant [version](https://mvnrepository.com/artifact/com.github.jknack/handlebars) | 6,5 LTS GA |

## Kända fel {#known-issues}

### Problem med JSP-skriptpaket i AEM 6.5.21-6.5.23 och AEM 6.5 LTS GA

AEM 6.5.21, 6.5.22, 6.5.23 och AEM 6.5 LTS GA levereras med paketet `org.apache.sling.scripting.jsp:2.6.0`, som innehåller ett känt fel. Problemet är vanligtvis under hög belastning när AEM-instansen hanterar många samtidiga begäranden.

När det här problemet inträffar kan ett av följande undantag visas i felloggarna tillsammans med referenser till `org.apache.sling.scripting.jsp:2.6.0`:

* `java.io.IOException: classFile.delete() failed`
* `java.io.IOException: tmpFile.renameTo(classFile) failed`
* `java.lang.ArrayIndexOutOfBoundsException: Index 0 out of bounds for length 0`
* `java.io.FileNotFoundException`

Det finns en hotfix [cq-6.5.lts.0-hotfix-NPR-42640](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-NPR-42640-1.2.zip) som åtgärdar det här problemet.

### Dispatcher-anslutningsfel med SSL-funktion {#ssl-only-feature}

När du aktiverar SSL-funktionen i AEM-distributioner finns det ett känt problem som påverkar anslutningen mellan Dispatcher- och AEM-instanserna. När du har aktiverat den här funktionen kan hälsokontroller misslyckas och kommunikationen mellan Dispatcher- och AEM-instanser kan avbrytas. Det här problemet inträffar specifikt när kunder försöker ansluta via `https + IP` från Dispatcher till AEM-instanser. Det är relaterat till SNI-valideringsproblem (Server Name Indication).

**Effekt:**

* Misslyckade hälsokontroller med HTTP 400-svarskoder
* Trafiken mellan Dispatcher och AEM har brutits
* Innehåll kan inte hanteras på rätt sätt via Dispatcher
* Anslutningsfel vid användning av HTTPS med IP-adresser i Dispatcher-konfiguration
* HTTP 400: Felet &quot;Invalid SNI&quot; vid anslutning via HTTPS + IP

**Berörda miljöer:**

* AEM installerar med Dispatcher-konfigurationer
* System där SSL-funktionen har aktiverats
* Dispatcher-konfigurationer som använder anslutningsmetoden `https + IP` till AEM-instanser

**Lösning:**
Kontakta Adobe kundsupport om du får det här problemet. Det finns en snabbkorrigering [cq-6.5.lts.0-hotfix-CQ-4359803](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-CQ-4359803-1.0.2.zip) som åtgärdar det här problemet. Försök inte aktivera SSL-funktioner förrän du har implementerat den nödvändiga snabbkorrigeringen.

## Begränsade webbplatser{#restricted-sites}

Dessa webbplatser är bara tillgängliga för kunder. Kontakta din kontoansvarige på Adobe om du är kund och behöver åtkomst.

* [Nedladdning av produkt på licensing.adobe.com](https://licensing.adobe.com/)
* [Kontakta Adobe kundsupport](https://experienceleague.adobe.com/en/docs/customer-one/using/home).