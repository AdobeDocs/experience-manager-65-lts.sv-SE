---
title: AEM Repo Tool
description: AEM Repo Tool är en enkel lösning för att överföra JCR-innehåll mellan det lokala filsystemet och AEM-servern via kommandoraden som kan jämföras med FTP. AEM Repo Tool liknar Jackrabbit FileVault-verktyget, men är snabbare, har minimalt med beroenden och är ett enkelt basskript.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# AEM Repo Tool{#aem-repo-tool}

AEM Repo Tool är en enkel lösning för att överföra JCR-innehåll mellan det lokala filsystemet och AEM-servern via kommandoraden som kan jämföras med FTP. AEM Repo-verktyget liknar [Jackrabbit FileVault-verktyget](/help/sites-developing/ht-vlttool.md), men är snabbare, har minimalt med beroenden och är ett enkelt basskript.

Det här verktyget förenklar filöverföringen för utvecklaren och kan även integreras i IntelliJ och Eclipse för att göra utvecklingen ännu effektivare.

## Ökning {#overview}

För en given sökväg i en `jcr_root`-fillevaultstruktur i filsystemet skapar AEM Repo Tool ett paket med ett enda filter för hela underträdet och överför det till servern (liknande FTP `put`), hämtar det från servern ( `get`) eller jämför skillnaderna ( `status` och `diff`).

Verktyget stöder inte flera filtersökvägar eller FileVault-objektets `filter.xml`.

>[!CAUTION]
>
>AEM Repo-verktyget skriver alltid över hela filen eller katalogen som anges.

## Ladda ned och dokumentation {#download-and-documentation}

[AEM Repo Tool är tillgängligt på GitHub via den här länken](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) tillsammans med detaljerade installations- och användningsanvisningar.

Om du vill hämta källan till AEM Repo Tool läser du GitHub-projektet som är länkat nedan.

KOD PÅ GITHUB

Koden för den här sidan finns på GitHub

* [Öppna verktygsprojekt på GitHub](https://github.com/Adobe-Marketing-Cloud/tools)
* Hämta projektet som [en ZIP-fil](https://github.com/Adobe-Marketing-Cloud/tools/archive/master.zip)
