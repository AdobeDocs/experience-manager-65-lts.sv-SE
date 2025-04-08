---
title: Frågor och svar (frågor och svar)
description: Frågor och svar om AEM 6.5 LTS.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
source-git-commit: 2352420843c613884ad3cae487ed048bd775e294
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# AEM 6.5 LTS Technical FAQ {#technical-faq}

Den här sidan är avsedd att besvara några vanliga tekniska frågor om AEM 6.5 LTS.

## Tekniska frågor och svar

### Slutpunkten `/systemalive` är inte längre tillgänglig i AEM 6.5 LTS.

Felix System Ready-paketet som konfigurerats för att tillhandahålla slutpunkten `/systemalive` har tagits bort och ersatts av Apache Felix Health Checks. Paketet ingår inte längre i AEM 6.5 LTS.

Den nya hälsokontrollsslutpunkten är tillgänglig på `/system/health` och implementeras med Apache Felix Health Checks.

Detaljerad dokumentation om Felix Health Check-ramverket finns i [felix-dokumentationen](https://github.com/apache/felix-dev/blob/master/healthcheck/README.md).

### Stöd för AEM Groovy-konsol

AEM Groovy-konsolversionen som användes i AEM 6.5 kanske inte fungerar i AEM 6.5 LTS på grund av att guava-beroenden saknas. Den nya versionen av AEM Groovy-konsolen som stöds är [19.0.8](https://mvnrepository.com/artifact/be.orbinson.aem/aem-groovy-console/19.0.8).

## Hämta ytterligare hjälp

Om du stöter på problem som inte beskrivs här:
* Granska [versionsinformationen](/help/release-notes/release-notes.md) för information om kända fel.
* Kontakta Adobe Support om du behöver hjälp.
