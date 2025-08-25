---
title: Frågor och svar (frågor och svar)
description: Frågor och svar om AEM 6.5 LTS.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: 051244f1-cc67-4222-bd45-0c135c28bb15
source-git-commit: f983fc1edc613feaa070c4e82a92aabab9d50cbb
workflow-type: tm+mt
source-wordcount: '247'
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

AEM Groovy-konsolversionen som användes i AEM 6.5 kanske inte fungerar i AEM 6.5 LTS på grund av att guava-beroenden saknas. Den nya versionen av AEM Groovy-konsolen som stöds är [19.0.8](https://github.com/orbinson/aem-groovy-console/releases/download/19.0.8/aem-groovy-console-all-19.0.8.zip).

### Har AEM 6.5 LTS stöd för användarsynkronisering?

Ja, AEM 6.5 LTS stöder användarsynkronisering. Funktionen för användarsynkronisering mellan AEM 6.5 och 6.5 LTS förändras inte.

### Uber JAR på Maven Central verkar vara skadad - vad är problemet?

Kontrollera att du använder Uber JAR med klassificeraren `apis`. Observera att paketeringsstrukturen för Uber JAR har ändrats i AEM 6.5 LTS. Mer information finns i [Uppdatera AEM Uber Jar-versionen](/help/sites-deploying/upgrading-code-and-customizations.md#update-the-aem-uber-jar-version).

## Hämta ytterligare hjälp

Om du stöter på problem som inte beskrivs här:
* Granska [versionsinformationen](/help/release-notes/release-notes.md) för information om kända fel.
* Kontakta Adobe Support om du behöver hjälp.
