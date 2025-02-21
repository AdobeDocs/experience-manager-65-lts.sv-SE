---
title: Kompatibilitetspaket
description: Genom att installera Kompatibilitetspaketet på AEM Forms 6.5 kan du använda Correspondence Management-resurser från AEM Forms 6.4 och tidigare versioner samt inaktuella adaptiva formulärmallar och sidor
contentOwner: gtalwar
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management, installing
geptopics: SG_AEMFORMS/categories/jee
docset: aem65
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 0%

---

# Kompatibilitetspaket{#compatibility-package}

## Ökning {#overview}

Interaktiv kommunikation är standardmetoden och rekommenderas för att skapa kundkommunikation i AEM Forms 6.5. Om du vill fortsätta använda bokstäver i AEM Forms 6.5 måste du installera det senaste [AEMFD-kompatibilitetspaketet](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html).

Med AEMFD-kompatibilitetspaketet kan du även [använda följande resurser från AEM Forms 6.4, 6.3 och 6.2 i AEM Forms 6.5:](../../forms/using/compatibility-package.md#add-support-for-aem-forms-and-assets-in-aem-forms)

* Dokumentfragment
* Bokstäver
* Dataordlistor
* Mallar och sidor för anpassade formulär har tagits bort

Mer information finns i [Assets har blivit kompatibelt med AEM Forms 6.5 genom att installera kompatibilitetspaketet](../../forms/using/compatibility-package.md#assetsmadecompatible).

## Lägg till stöd för AEM Forms 6.4, 6.3 och 6.2-material i AEM Forms 6.5 {#add-support-for-aem-forms-and-assets-in-aem-forms}

När du har utfört en uppgradering gör du följande för att installera AEMFD-kompatibilitetspaketet och göra dina resurser kompatibla med 6.5:

Kontrollera att [AEM-kompatibilitetspaketet](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html) är förinstallerat.

1. Installera det senaste [kompatibilitetspaketet](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html) för 6.5.

   Mer information om hur du överför och installerar paketet finns i [Arbeta med paket](/help/sites-administering/package-manager.md).

1. Starta om servern när loggarna har stabiliserats.
1. Använd flyttningsverktyget för att göra dina resurser kompatibla med 6.5.

   >[!NOTE]
   >
   > Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

   Mer information finns i [Migreringsverktyget](../../forms/using/migration-utility.md).

## Assets har gjorts kompatibelt med AEM Forms 6.5 genom att installera kompatibilitetspaketet {#assetsmadecompatible}

Genom att installera Kompatibilitetspaketet kan du göra följande resurser och mallar kompatibla med AEM Forms 6.5:

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
