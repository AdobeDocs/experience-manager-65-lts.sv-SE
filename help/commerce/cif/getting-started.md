---
title: Komma igång med AEM Content och Commerce
description: Lär dig hur du driftsätter AEM Content och Commerce.
topics: Commerce
feature: Commerce Integration Framework
solution: Experience Manager,Commerce
role: Admin, Developer
exl-id: 15face30-3039-49a0-bfee-56bff21e5c27
source-git-commit: 0b337740dffb9e6421e6d2c44a1ae222dfa37711
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Komma igång med AEM Content och Commerce {#start}

För att komma igång med AEM Content och Commerce måste du installera AEM Content och Commerce Add-On för AEM 6.5.


## Onboarding {#onboarding}

Introduktionen av AEM Content och Commerce är en tvåstegsprocess:

1. Installera AEM Content and Commerce Add-on for AEM 6.5 LTS

2. Koppla samman AEM med e-handelslösningen

### Installera AEM Content and Commerce Add-on for AEM 6.5 LTS {#install-add-on}

Hämta och installera AEM Commerce Add-On för AEM 6.5 LTS från portalen [Software Distribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html).

Starta och installera AEM 6.5 LTS Service Pack. Vi rekommenderar att du installerar det senaste tillgängliga Service Pack-paketet.

>[!NOTE]
>
>Detta görs av CSE för AEM Managed Service-kunder.

### Anslut AEM till ditt Commerce-system {#connect}

AEM kan anslutas till alla affärssystem som har en tillgänglig GraphQL-slutpunkt för AEM. Dessa slutpunkter är vanligtvis offentligt tillgängliga eller kan anslutas via privata VPN-anslutningar eller lokala anslutningar beroende på de enskilda projektinställningarna.

Autentiseringshuvudet kan också anges om du vill använda ytterligare CIF-funktioner som kräver autentisering.

Projekt som har skapats av [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) och [AEM Venia Reference Store](https://github.com/adobe/aem-cif-guides-venia) som redan ingår i [standardkonfigurationen](https://github.com/adobe/aem-cif-guides-venia/blob/main/ui.config/src/main/content/jcr_root/apps/venia/osgiconfig/config/com.adobe.cq.commerce.graphql.client.impl.GraphqlClientImpl~default.cfg.json) måste justeras.

Ersätt värdet för `url` i `com.adobe.cq.commerce.graphql.client.impl.GraphqlClientImpl~default.cfg.json` med slutpunkten för GraphQL i e-handelssystemet. Denna konfiguration kan göras via OSGI-konsolen eller genom att distribuera OSGI-konfigurationen via projektet. Olika konfigurationer för testnings- och produktionssystem stöds med olika körningslägen för AEM.

AEM Content och Commerce Add-On och CIF Core Components använder både AEM serversides- och klientsidesanslutningar. Klientsidans CIF Core Components och CIF Add-On-verktyg ansluter som standard till `/api/graphql`. Detta kan vid behov justeras via CIF Cloud Service-konfigurationen (se nedan).

CIF Add-On tillhandahåller en GraphQL-proxyserver på `/api/graphql` som kan användas för [lokal utveckling](develop.md). För produktionsdistributioner rekommenderar vi starkt att du skapar en omvänd proxy till e-handelsplatsen GraphQL via AEM Dispatcher eller andra nätverkslager (som CDN).

## Konfigurera butiker och kataloger {#catalog}

Tillägget och [CIF Core Components](https://github.com/adobe/aem-core-cif-components) kan användas på flera av AEM webbplatsstrukturer som är anslutna till olika e-handelsbutiker (eller butiksvyer osv.). Som standard distribueras CIF Add-On med en standardkonfiguration som ansluter till Adobe Commerce standardbutik och -katalog.

Den här konfigurationen kan justeras för projektet via CIF Cloud Service-konfigurationen enligt följande steg:

1. I AEM går du till Verktyg > Molntjänster > CIF-konfiguration

2. Välj den e-postkonfiguration som du vill ändra

3. Öppna konfigurationsegenskaperna via åtgärdsfältet

![Konfiguration av CIF Cloud-tjänster](/help/commerce/cif/assets/cif-cloud-service-config.png)

Följande egenskaper kan konfigureras:

- GraphQL Client - Välj den konfigurerade GraphQL-klienten för e-handelskommunikation. Detta bör normalt vara kvar som standard.
- Butiksvy - butiksvyns identifierare. Om den är tom används standardbutiksvyn.
- GraphQL Proxy Path - den URL-sökväg som GraphQL Proxy i AEM använder för proxybegäranden till GraphQL-slutpunkt för e-handel.

  >[!NOTE]
  >
  >I de flesta inställningar får standardvärdet `/api/graphql` inte ändras. Endast avancerade inställningar som inte använder den angivna GraphQL-proxyn bör ändra den här inställningen.

- Aktivera Catalog UID Support - aktivera stöd för UID i stället för ID i e-handelssamtal med GraphQL.

  >[!NOTE]
  >
  >Stöd för UID introducerades i Adobe Commerce 2.4.2. Aktivera bara detta om e-handelsbackend har stöd för ett GraphQL-schema av version 2.4.2 eller senare.

- Katalogrotkategoriidentifierare - identifieraren (UID eller ID) för arkivkatalogens rot

  >[!CAUTION]
  >
  >Från och med CIF Core Components version 2.0.0 har stödet för `id` tagits bort och ersatts med `uid`. Om ditt projekt använder CIF Core Components version 2.0.0 måste du aktivera Catalog UID Support och använda en giltig UID-kategori som&quot;Katalogens rotkategoriidentifierare&quot;.

Konfigurationen som visas ovan är för referens. Projekten ska ha egna konfigurationer.

Mer komplexa inställningar som använder flera olika webbplatsstrukturer på AEM i kombination med olika e-handelskataloger finns i självstudiekursen [Commerce Multi-Store Setup](configuring/multi-store-setup.md).

## Ytterligare resurser {#additional-resources}

- [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)
- [AEM Venia Reference Store](https://github.com/adobe/aem-cif-guides-venia)
- [Installation av Commerce Multi-Store](configuring/multi-store-setup.md)
