---
title: Dokumentsäkerhet
description: Läs om olika verktyg och funktioner i AEM Document Security.
contentOwner: khsingh
geptopics: SG_AEMFORMS/categories/working_with_document_security
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 6653649a-5076-48e3-a7ed-5b74d4d2e8e1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 0%

---

# Dokumentsäkerhet{#document-security-offerings}

Adobe Experience Manager Forms dokumentsäkerhet säkerställer att endast behöriga användare kan använda dina dokument. Med dokumentsäkerhet kan du distribuera all information som du har sparat i ett format som stöds. De filformat som stöds är bland annat Adobe Portable Document Format (PDF) och Microsoft® Word-, Excel- och PowerPoint-filer.

Du kan skydda dokument med hjälp av profiler. De sekretessinställningar som du anger i en profil avgör hur en mottagare kan använda ett dokument som du tillämpar profilen på. Du kan till exempel ange om mottagarna ska kunna skriva ut eller kopiera text, redigera text eller lägga till signaturer och kommentarer i skyddade dokument.

Profilerna lagras på dokumentsäkerhetsservern. Du tillämpar profilerna på dokument via klientprogrammet. När du tillämpar en profil på ett dokument skyddar sekretessinställningarna som anges i profilen den information som dokumentet innehåller. Du kan distribuera det profilskyddade dokumentet till mottagare som har behörighet enligt profilen.

Bilden nedan visar den typiska arkitekturen för AEM Forms Document Security:

![Dokumentsäkerhet - rekommenderad arkitektur](do-not-localize/document_security_architecture.png)

## Dokumentsäkerhetsklienter {#document-security-clients}

Dokumentskydd ger olika klienter för att skydda dokument, visa och redigera skyddade dokument och indexerare för att möjliggöra fulltextsökning i skyddade dokument. Du kan välja en klient baserat på dina krav och klientens funktioner.

Document Security Server är den centrala komponenten som Document Security utför transaktioner som användarautentisering, hantering av profiler i realtid och tillämpning av sekretess. Servern har också ett centralt arkiv för policyer, granskningsposter och annan relaterad information.

På dokumentsäkerhetsservern finns ett webbaserat gränssnitt (webbsida) för att skapa profiler, hantera principskyddade dokument och övervaka händelser som är kopplade till principskyddade dokument. Administratörer kan också konfigurera globala alternativ som användarautentisering, granskning och meddelanden för inbjudna användare samt hantera inbjudna användarkonton.

Servern ingår i AEM Forms Document Security-tillägget. Du kan kontakta AEM Forms [säljteam](https://business.adobe.com/request-consultation/experience-cloud.html?s_osc=70114000002JNwKAAW&s_iid=70114000002JHs3AAG) för att köpa Document Security-tillägget.

### Skydda dokument {#protect-documents}

AEM Forms Document Security har olika verktyg för att tillämpa skyddsprofiler. Du kan välja ett verktyg enligt dina krav och specifikationer.

![dokumentsäkerhetserbjudanden](assets/document-security-offerings.png)

Du kan använda Document Security SDK, Adobe Acrobat, Document Security Extension för Microsoft® Office eller Portable Protection Library för att tillämpa och spåra säkerhetsprofilerna:

* **Document Security SDK:** SDK är en funktionsrik klient. Du kan använda SDK Document Security för att få åtkomst till dokumentserverfunktioner, öppna policyskyddade dokument och utveckla anpassade tillägg, plugin-program eller program. Du kan till exempel utveckla tillägg för att skydda anpassade filformat eller integrera SDK med DLP-lösningar (Data Loss Prevention). Tillägg, program och plugin-program som utvecklats med Document Security SDK för att skicka dokument till den avsedda AEM Forms Server och där reglerna tillämpas på servern. AEM Forms dokumentskyddsklient SDK (CSDK) kan inte ta bort skyddet för dokument som skyddas med PPL (Portable Protection Library) och omvänt.

  Document Security SDK finns för både Java™ och C++. Java™ SDK ingår i AEM Forms Document Security och är installerat på att distribuera AEM-formulär på JEE. Kontakta [AEM kundsupport](https://experienceleague.adobe.com/sv?support-solution=General&support-tab=home#support) om du vill köpa C++ SDK. C++ SDK kan kompileras med Microsoft® Visual Studio 2013. Besök [API-dokumentationen för dokumentsäkerhet](https://help.adobe.com/en_US/livecycle/11.0/Services/WS92d06802c76abadb76c48dfe12dbeb3e281-7ff0.2.html) där du kan läsa om och använda funktioner i SDK.

* **Adobe Acrobat:** Du kan använda Adobe Acrobat för att tillämpa skyddsprofiler på PDF-dokument som skapats med vanliga skrivbordsprogram, till exempel Microsoft® Office, webbläsare eller andra program som stöder utskrift i PDF-format.

  Du kan köpa och hämta Adobe Acrobat från [Adobe webbplats](https://www.adobe.com/acrobat/free-trial-download.html). Adobe Acrobat-artikeln [Konfigurera skyddsprofiler för PDF-filer](https://helpx.adobe.com/se/acrobat/using/setting-security-policies-pdfs.html) innehåller detaljerad information om hur du skapar och tillämpar skyddsprofiler i Adobe Acrobat.

* **Document Security Extension for Microsoft® Office**: Du kan använda Document Security Extension for Microsoft® Office för att tillämpa fördefinierade profiler på dina Microsoft® Office-filer inifrån Microsoft® Office-programmen. Tillägget säkerställer att endast behöriga personer kan använda policyskyddade Microsoft® Word-, Excel- och PowerPoint-filer. Endast behöriga användare som har plugin-programmet installerat kan använda principskyddade filer.

  Tillägget Dokumentsäkerhet är tillgängligt som ett Microsoft® Office-plugin. Kontakta [AEM kundsupport](https://helpx.adobe.com/ca/marketing-cloud/contact-support.html) om du vill köpa tillägget. Senare kan du gå till [Document Security Extension for Microsoft® Office](https://experienceleague.adobe.com/docs/experience-manager-document-security/using/download-installer.html?lang=sv-SE) om du vill veta mer om hur du installerar, konfigurerar och använder tillägget.

* **Portable Protection Library:** PPL-filen skyddar ett dokument lokalt utan att skicka dokumentet till AEM Forms Server. Det är bara säkerhetsreferenser och principinformation som rör sig över nätverket. Med PPL kan du även begränsa åtkomst till profiler för enbart inloggade användare. Du kan hämta profiler med kontexten för den användare som är inloggad i AEM.

  Tillsammans med ovanstående har PPL alla funktioner i Document Security SDK. Du kan använda SDK Document Security för att få åtkomst till dokumentserverfunktioner, öppna policyskyddade dokument och utveckla anpassade tillägg, plugin-program eller program. PPL kan inte ta bort skyddet för dokument som skyddas med AEM Forms Document Security Client SDK (CSDK) och omvänt.

  PPL finns för Java™ och C++ i 32- och 64-bitarsversioner. Det finns också som ett OSGi-paket för AEM Forms på OSGi. C++ PPL kan kompileras med Microsoft® Visual Studio 2013. Om du har licensierat AEM Forms Document Security-tillägget kan du kontakta [AEM Forms Document Security](https://experienceleague.adobe.com/sv?support-solution=General&support-tab=home#support)-supportteamet för att beställa PPL:en. Senare kan du använda PPL-hjälpen (medföljer biblioteket) för att konfigurera och använda PPL-filen.

### Visa eller redigera skyddade dokument {#view-or-edit-protected-documents}

* För **PDF-dokument** kan du använda Adobe Acrobat DC, Acrobat Reader och Acrobat Reader Mobile för att visa skyddade PDF-dokument. De flesta användare har redan Acrobat Reader installerat på sina enheter, så de behöver inte skaffa eller lära sig ytterligare programvara för att kunna visa skyddade dokument. Du kan även hämta Acrobat Reader från [Acrobat Reader webbplats för hämtning](https://get.adobe.com/reader/).

* För **Microsoft® Office-dokument** krävs Microsoft® Office- och AEM Forms Document Security-tillägg för Microsoft® Office. Tillägget Dokumentsäkerhet är tillgängligt som ett Microsoft® Office-plugin. Du kan hämta tillägget från Adobe webbplats.

### Indexera skyddade dokument {#index-protected-documents}

Microsoft® Windows fulltextsökmotorer (SharePoint Index-servern) och Adobe Experience Manager (AEM) kan utföra fulltextsökning i vanliga dokumentformat som vanliga textfiler, Microsoft® Office-dokument och PDF-dokument. Du kan använda dokumentsäkerhetsindexerare för att aktivera fulltextsökmotorer för att söka efter skyddade PDF-dokument:

* **iFilter-indexerare:** Du kan använda iFilter-indexeraren för att indexera skyddade PDF-dokument och aktivera fulltextsökmotorer för Microsoft® Windows (Desktop Indexing Service och SharePoint Index Server) för att söka i skyddade PDF-dokument. Mer information finns i [AEM SharePoint IFilter för skyddade dokument](assets/sharepoint-ifilter-doc-security.pdf).

* **AEM Forms Document Security Indexer:** Du kan använda AEM Forms Document Security-indexeraren för att indexera skyddade PDF-dokument och aktivera Adobe Experience Manager för att söka efter skyddade PDF-dokument. Indexerarna ingår i AEM Forms Document Security. Dessa ingår i AEM Forms på JEE-installationsprogram.
