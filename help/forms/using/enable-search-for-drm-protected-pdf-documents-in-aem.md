---
title: Möjliggör för AEM att söka efter säkerhetsskyddade PDF-dokument
description: Lär dig hur du aktiverar AEM-sökning för att utföra fulltextsökning i DRM-skyddade PDF-dokument.
contentOwner: khsingh
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
geptopics: SG_AEMFORMS/categories/working_with_document_security
docset: aem65
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: ad86398d-0dc9-4168-b409-4d231b8d586b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 0%

---

# Möjliggör för AEM att söka efter säkerhetsskyddade PDF-dokument{#enable-aem-to-search-document-security-protected-pdf-documents}

AEM-sökning kan användas för att söka efter och hitta AEM-resurser och för textsökning i olika vanliga dokumentformat, t.ex. oformaterade textfiler, Microsoft Office-dokument och PDF-dokument. Du kan också utöka den inbyggda sökningen för att utföra fulltextsökning på [PDF-dokument som skyddas med AEM Document Security](../../forms/using/admin-help/document-security.md). Så här gör du för att AEM ska kunna utföra fulltextsökning i sådana dokument:

1. Upprätta en säker anslutning
1. Indexera ett exempelpolicyskyddat PDF-dokument

## Förutsättningar {#prerequisites}

* Om du använder AEM Forms på OSGi:

   * Installera [AEM Forms Document Security Indexer-paketet](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html) på AEM Forms-servern.

   * Kontrollera att en AEM Forms på JEE-server är igång och att dokumentsäkerheten är installerad på motsvarande AEM Forms på JEE-server. AEM-formuläret på JEE-servern krävs för att indexera det skyddade dokumentet.

* Om du bara använder AEM Forms på JEE-servern är indexeringspaketet redan installerat.
* Se till att alla paket är igång. Om alla paket inte är aktiva väntar du tills alla paket är igång.

   * För AEM Forms i OSGi listas paketen på https://&#39;[server]:[port]/system/console/bundles.
   * För AEM Forms på JEE listas paketen på https://&#39;[server]:[port]/[context-path]/system/console/bundles. Exempel: https://localhost:8080/lc/system/console/bundles.

* Lägg till paketet *sun.util.calendar* i tillåtelselista. Så här lägger du till paketet i tillåtelselista:

   1. Öppna AEM Web Console. URL:en är https://&#39;[server]:[port]&#39;/system/console/configMgr.
   1. Leta reda på och öppna **Konfiguration av brandväggen för deserialisering**.

   1. Lägg till paketet sun.util.calendar i fältet för Tillåtslista klasser eller paketprefix och klicka på **Spara**.

### Skapa en säker anslutning mellan AEM Forms JEE- och OSGi-stackar {#establish-a-secure-connection-between-aem-forms-jee-and-osgi-stacks}

Du kan använda någon av följande metoder för att upprätta en säker anslutning:

* Konfigurera Adobe LiveCycle Client SDK Bundle med inloggningsuppgifter för AEM Forms på JEE-administratörer
* Konfigurera Adobe LiveCycle Client SDK Bundle med hjälp av ömsesidig autentisering

#### Konfigurera Adobe LiveCycle Client SDK Bundle med inloggningsuppgifter för AEM Forms på JEE-administratörer {#configure-adobe-livecycle-client-sdk-bundle-with-aem-forms-on-jee-admin-credentials}

1. Öppna AEM Web Console. URL:en är https://&#39;[server]:[port]&#39;/system/console/configMgr.
1. Leta upp och öppna **Adobe LiveCycle Client SDK Bundle**. Ange värde för följande fält:

   * **Server-URL:** Ange HTTPS-URL för AEM Forms på JEE-server. Om du vill aktivera kommunikation över https startar du om servern med parametern -Djavax.net.ssl.trustStore=&lt;sökväg till AEM Forms i JEE-nyckelfil>.
   * **Tjänstnamn**: Lägg till RightsManagementService i listan över angivna tjänster.
   * **Användarnamn:** Ange användarnamnet för det AEM Forms på JEE-konto som ska användas för att initiera anrop från AEM-servern. Det angivna kontot måste ha behörighet att starta dokumenttjänster på AEM Forms på JEE-servern.
   * **Lösenord**: Ange lösenordet för det AEM Forms på JEE-konto som anges i fältet Användarnamn.

   Klicka på **Spara**. AEM har aktiverats för att söka efter säkerhetsskyddade PDF-dokument.

#### Konfigurera Adobe LiveCycle Client SDK Bundle med hjälp av ömsesidig autentisering {#configure-adobe-livecycle-client-sdk-bundle-using-mutual-authentication}

1. Aktivera ömsesidig autentisering för AEM Forms på JEE. Mer information finns i [CAC och ömsesidig autentisering](https://helpx.adobe.com/livecycle/kb/cac-mutual-authentication.html).
1. Öppna AEM Web Console. URL:en är https://&#39;[server]:[port]&#39;/system/console/configMgr.
1. Leta upp och öppna paketet **Adobe LiveCycle Client SDK**. Ange värde för följande egenskaper:

   * **Server-URL**: Ange HTTPS-URL för AEM Forms på JEE-server. Om du vill aktivera kommunikation över https startar du om AEM-servern med parametern -Djavax.net.ssl.trustStore=&lt;sökväg till AEM Forms på JEE-nyckelfil>.
   * **Aktivera tvåvägs SSL**: Aktivera alternativet Aktivera tvåvägs SSL.
   * **KeyStore-fil-URL**: Ange URL:en för nyckelbehållarfilen.
   * **TrustStore-filens URL**: Ange URL:en för förvaltarfilen.
   * **KeyStore-lösenord**: Ange lösenordet för nyckelfilen.
   * **TrustStorePassword**: Ange lösenordet för förvaltarfilen.
   * **Tjänstnamn**: Lägg till RightsManagementService i listan över angivna tjänster.

   Klicka på **Spara**. AEM har aktiverats för att söka efter säkerhetsskyddade PDF-dokument

### Indexera ett exempelpolicyskyddat PDF-dokument {#index-a-sample-policy-protected-pdf-document}

1. Logga in på AEM Assets som administratör.
1. Skapa en mapp i AEM Digital Asset Manager och överför profilskyddade PDF-dokument till den nya mappen.

   Nu kan du söka i profilskyddade dokument med AEM Search.

   >[!NOTE]
   >
   > Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.
