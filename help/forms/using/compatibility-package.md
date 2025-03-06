---
title: Kompatibilitetspaket
description: Genom att installera Kompatibilitetspaketet på AEM Forms 6.5 LTS kan du använda Correspondence Management-resurser från AEM Forms 6.5 och tidigare versioner samt föråldrade adaptiva formulärmallar och sidor
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: 3a529a82-e2fd-423c-96c1-a5accc87775e
source-git-commit: 2e0cbe62754866d31de69547f9af1f2f63930f2c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Kompatibilitetspaket{#compatibility-package}

## Ökning {#overview}

Interaktiv kommunikation är standardmetoden och rekommenderas för att skapa kundkommunikation i AEM Forms 6.5 LTS. Om du vill fortsätta använda bokstäver i AEM Forms 6.5 LTS måste du installera det senaste [AEMFD-kompatibilitetspaketet](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases).

Med AEMFD-kompatibilitetspaketet kan du även [använda följande resurser från AEM Forms 6.5.22.0, 6.4, 6.3 och 6.2 i AEM Forms 6.5 LTS](../../forms/using/compatibility-package.md#add-support-for-aem-forms-and-assets-in-aem-forms)

* Dokumentfragment
* Bokstäver
* Dataordlistor
* Mallar och sidor för anpassade formulär har tagits bort

Mer information finns i [Assets har blivit kompatibelt med AEM Forms 6.5 genom att installera kompatibilitetspaketet](../../forms/using/compatibility-package.md#assetsmadecompatible).

## Lägg till stöd för AEM Forms 6.5, 6.4, 6.3 och 6.2-resurser i AEM Forms 6.5 LTS {#add-support-for-aem-forms-and-assets-in-aem-forms-6.5.lts}

När du har utfört en uppgradering gör du följande för att installera AEMFD-kompatibilitetspaketet och göra dina resurser kompatibla med 6.5:

Kontrollera att [AEM-kompatibilitetspaketet](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases) är förinstallerat.

1. Installera det senaste AEM 6.5 LTS [kompatibilitetspaketet](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases).

   Mer information om hur du överför och installerar paketet finns i [Arbeta med paket](/help/sites-administering/package-manager.md).

1. Starta om servern när loggarna har stabiliserats.
1. Använd flyttningsverktyget för att göra dina resurser kompatibla med 6.5 LTS.

   >[!NOTE]
   >
   > Vi rekommenderar att du använder kommandot `Ctrl + C` för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

   Mer information finns i [Migreringsverktyget](../../forms/using/migration-utility.md).

## Assets har gjorts kompatibelt med AEM Forms 6.5 LTS genom att installera kompatibilitetspaketet {#assetsmadecompatible}

Genom att installera Kompatibilitetspaketet kan du göra följande resurser och mallar kompatibla med AEM Forms 6.5 LTS:

* Correspondence Management Assets från AEM 6.4 och tidigare:

   * [Bokstäver](../../forms/using/create-letter.md)
   * [Dataordlistor](/help/forms/using/data-dictionary.md)
   * Dokumentfragment

* Mallar som inte längre används för anpassade formulär:

   * /libs/fd/af/templates/blankTemplate2
   * /libs/fd/af/templates/simpleEnrollmentTemplate
   * /libs/fd/af/templates/simpleEnrollmentTemplate2
   * /libs/fd/af/templates/surveyTemplate
   * /libs/fd/af/templates/surveyTemplate2
   * /libs/fd/af/templates/tabbedEnrollmentTemplate
   * /libs/fd/af/templates/tabbedEnrollmentTemplate2
   * /libs/fd/afaddon/templates/advancedEnrollmentTemplate
   * /libs/fd/afaddon/templates/advancedEnrollmentTemplate2

* Sidor med adaptiva formulär har tagits bort:

   * /libs/fd/af/components/page/survey
   * /libs/fd/af/components/page/tabbedenrollment
   * /libs/fd/afaddon/components/page/advancedRotering
