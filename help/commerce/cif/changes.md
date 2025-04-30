---
title: Betydande förändringar av tillägget Commerce integration framework (CIF)
description: Betydande förändringar av tillägget Commerce integration framework (CIF) jämfört med tidigare CIF-versioner.
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
exl-id: aced89a0-dec1-49fe-afbc-3ddf1318b900
source-git-commit: 79cce324382bada2e9aec107b8e494723bf490e9
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# Observerbara ändringar i tillägget Commerce integration framework (CIF){#notable-changes}

Det här dokumentet belyser de viktiga skillnaderna mellan Commerce integration framework (CIF) och gamla CIF-versioner, främst kända som CIF Classic (Quickstart) och CIF Open-source.

## Installation och uppdateringar

AEM CIF-tilläggspaketet installeras och uppdateras med AEM Package Manager.

**Tidigare CIF-versioner**

* CIF Classic: CIF var en del av QuickStart och behövde inte installeras. CIF-uppdateringar ingick i AEM- eller Service Pack-uppdateringar
* CIF Open-source: Installation via GitHub. Uppdateringarna var en del av det manuella uppdaterings-/underhållsarbetet.

## Konfiguration av slutpunkt

Slutpunkten konfigureras via OSGi-konsolen.

**Tidigare CIF-versioner**

* CIF Classic: Via OSGi-konfiguration i AEM
* CIF Open-source: Via CIF Configuration Browser

## Driftsättning av CIF Venia Project

Projektet är tillgängligt på [GitHub AEM Guides - CIF Venia Project](https://github.com/adobe/aem-cif-guides-venia) och distributionen görs via AEM Package Manager.

**Tidigare CIF-versioner**

* CIF Classic: Installera AEM-paket

## Produktkatalogdata

Produktkatalogdata efterfrågas vid behov via realtidsanrop till en extern slutpunkt som stöder de GraphQL API:er som krävs. Dessa API:er har stöd för åtkomst till livedata eller mellanlagrade data vid ett visst datum. Ingen replikering behövs.

**Tidigare CIF-versioner**

* CIF Classic: Live-produktdata och mellanlagrade produktdata importeras och lagras i JCR på AEM Author via import av komplett- eller deltaprodukt. Live-produktdata replikeras till AEM Publish.

## Produktkatalogupplevelser med AEM rendering

AEM återger produktkatalogupplevelser direkt med AEM katalogmallar som har tilldelats produkter och kategorier. Ingen replikering behövs.

**Tidigare CIF-versioner**

* CIF Classic: AEM Author skapar en AEM-sida för varje kategori/produkt med hjälp av katalogritningsverktyget. Dessa sidor replikeras till AEM Publish.

>[!NOTE]
>
>Mer information om hur du använder CIF med AEM Managed Service eller AEM On-Premise finns i [Commerce integration framework](https://developer.adobe.com/apis/experiencecloud/commerce-integration-framework/getting-started.html)
