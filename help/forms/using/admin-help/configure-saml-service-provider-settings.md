---
title: Konfigurera inställningar för SAML-tjänstleverantör
description: Du kan konfigurera inställningarna för SAML-tjänsteleverantören så att användare kan logga in och autentisera till AEM-formulär via en angiven tredjepartsidentitetsleverantör (IDP).
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 0f1b39e7-5de5-4b54-b622-61774ce839db
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# Konfigurera inställningar för SAML-tjänstleverantör{#configure-saml-service-provider-settings}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

SAML (Security Assertion Markup Language) är ett av de alternativ som du kan välja när du konfigurerar auktorisering för en företagsdomän eller hybriddomän. SAML används främst för att stödja enkel inloggning över flera domäner. När SAML är konfigurerat som din autentiseringsleverantör loggar användare in och autentiserar till AEM-formulär via en angiven tredjepartsidentitetsleverantör (IDP).

En förklaring av SAML finns i [Security Assertion Markup Language (SAML) V2.0 Technical Overview](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html).

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Inställningar för SAML-tjänstleverantör.
1. Ange ett unikt ID som ska användas som identifierare för implementeringen av AEM-formulärtjänstleverantören i rutan Tjänstleverantörens enhets-ID. Du kan också ange detta unika ID när du konfigurerar din IDP (till exempel `um.lc.com`.) Du kan också använda den URL som används för att komma åt AEM-formulär (till exempel `https://AEMformsserver`).
1. Ange bas-URL:en för din Forms-server (till exempel `https://AEMformsserver:8080`) i rutan Tjänstleverantörens bas-URL.
1. (Valfritt) Gör så här för att aktivera AEM-formulär för att skicka signerade autentiseringsbegäranden till IDP:

   * Använd Pålitlighetshanteraren för att importera en autentiseringsuppgift i PKCS #12-format med Dokumentsigneringsuppgifter som har valts som pålitlighetslagertyp. (Se [Hantera lokala autentiseringsuppgifter](/help/forms/using/admin-help/local-credentials.md#managing-local-credentials).)
   * I aliaslistan för tjänstleverantörens nyckel för autentiseringsuppgifter markerar du det alias som du tilldelade autentiseringsuppgifterna i Trust Store.
   * Klicka på Exportera så att du kan spara URL-innehållet i en fil och sedan importera filen till din IDP.

1. (Valfritt) Välj det namnformat som IDP använder för att identifiera användaren i en SAML-försäkran i listan över namn-ID för tjänsteleverantören. Alternativen är Unspecified, Email och Windows Domain Qualified Name.

   >[!NOTE]
   >
   >Namnformaten är inte skiftlägeskänsliga.

1. (Valfritt) Välj Aktivera autentiseringsfråga för lokala användare. När det här alternativet är markerat visas två länkar:

   * en länk till inloggningssidan för SAML-identitetsleverantören från tredje part, där användare som tillhör en företagsdomän kan autentisera.
   * en länk till inloggningssidan för AEM-formulär, där användare som tillhör en lokal domän kan autentisera.

   När det här alternativet inte är markerat dirigeras användare direkt till inloggningssidan för SAML-identitetsleverantören från tredje part, där användare som tillhör en Enterprise-domän kan autentisera.

1. (Valfritt) Markera Aktivera artifakt-bindning om du vill aktivera stöd för artefaktbindning. POST-bindning används som standard med SAML. Om du har konfigurerat Artefaktbindning väljer du det här alternativet. När det här alternativet är markerat skickas inte den faktiska användarbekräftelsen via webbläsarbegäran. I stället skickas en pekare till kontrollen och försäkran hämtas med ett serverdelsanrop för webbtjänsten.
1. (Valfritt) Markera Aktivera omdirigeringsbindning om du vill ha stöd för SAML-bindningar som använder omdirigeringar.
1. (Valfritt) Ange ytterligare egenskaper i Anpassade egenskaper. De ytterligare egenskaperna är namn=värde-par avgränsade med nya rader.

   * Du kan konfigurera AEM-formulär så att de utfärdar en SAML-försäkran för en giltighetsperiod som matchar giltighetsperioden för en tredjepartsförsäkran. Lägg till följande rad i Anpassade egenskaper för att efterleva tidsgränsen för SAML-försäkran från tredje part:

     `saml.sp.honour.idp.assertion.expiry=true`

   * Lägg till följande anpassade egenskap för att använda RelayState för att bestämma den URL där användaren ska omdirigeras efter lyckad autentisering.

     `saml.sp.use.relaystate=true`

   * Lägg till följande anpassade egenskap så att du kan konfigurera URL:en för Java™ Server Pages (JSP), som används för att återge den registrerade listan över identitetsleverantörer. Om du inte har distribuerat något anpassat webbprogram används standardsidan för användarhantering för att återge listan.

   `saml.sp.discovery.url=/custom/custom.jsp`

1. Klicka på Spara.
