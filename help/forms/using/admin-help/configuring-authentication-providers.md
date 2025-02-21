---
title: Konfigurerar autentiseringsproviders
description: Lägg till, redigera eller ta bort autentiseringsproviders, ändra autentiseringsinställningar och läs om just-in-time-etablering av användare.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/setting_up_and_managing_domains
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '1592'
ht-degree: 0%

---

# Konfigurerar autentiseringsproviders {#configuring-authentication-providers}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Hybriddomäner kräver minst en autentiseringsprovider och företagsdomäner kräver minst en autentiseringsprovider eller katalogprovider.

Om du aktiverar enkel inloggning med SPNEGO lägger du till en Kerberos-autentiseringsprovider med SPNEGO aktiverat och en LDAP-provider som säkerhetskopia. Den här konfigurationen aktiverar användarautentisering med ett användar-ID och lösenord om SPNEGO inte fungerar. (Se [Aktivera enkel inloggning med SPNEGO](/help/forms/using/admin-help/enabling-single-sign-on-aem.md#enable-sso-using-spnego).)

## Lägg till en autentiseringsprovider {#add-an-authentication-provider}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på en befintlig domän i listan. Om du lägger till autentisering för en ny domän läser du [Lägg till en företagsdomän](/help/forms/using/admin-help/adding-domains.md#add-an-enterprise-domain) eller [Lägg till en hybriddomän](/help/forms/using/admin-help/adding-domains.md#add-a-hybrid-domain).
1. Klicka på Lägg till autentisering och välj en leverantör i listan Autentiseringsprovider, beroende på vilken autentiseringsmetod din organisation använder.
1. Ange eventuell ytterligare information som krävs på sidan. (Se [Autentiseringsinställningar](configuring-authentication-providers.md#authentication-settings).)
1. (Valfritt) Klicka på Testa för att testa konfigurationen.
1. Klicka på OK och sedan på OK igen.

## Redigera en befintlig autentiseringsprovider {#edit-an-existing-authentication-provider}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på lämplig domän i listan.
1. På sidan som visas väljer du lämplig autentiseringsprovider i listan och gör önskade ändringar. (Se [Autentiseringsinställningar](configuring-authentication-providers.md#authentication-settings).)
1. Klicka på OK.

## Ta bort en autentiseringsprovider {#delete-an-authentication-provider}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på lämplig domän i listan.
1. Markera kryssrutorna för de autentiseringsleverantörer som ska tas bort och klicka på Ta bort.
1. Klicka på OK på bekräftelsesidan som visas och klicka på OK igen.

## Autentiseringsinställningar {#authentication-settings}

Följande inställningar är tillgängliga, beroende på vilken typ av domän och typ av autentisering du väljer.

### LDAP-inställningar {#ldap-settings}

Om du konfigurerar autentisering för en företagsdomän eller hybriddomän och väljer LDAP-autentisering, kan du välja att använda den LDAP-server som anges i din katalogkonfiguration eller välja en annan LDAP-server att använda för autentisering. Om du väljer en annan server måste användarna finnas på båda LDAP-servrarna.

Om du vill använda den LDAP-server som anges i katalogkonfigurationen väljer du LDAP som autentiseringsprovider och klickar på OK.

Om du vill använda en annan LDAP-server för autentisering, markerar du LDAP som autentiseringsprovider och markerar kryssrutan Anpassad LDAP-autentisering. Följande konfigurationsinställningar visas.

**Server:** (obligatoriskt) Fullständigt kvalificerat domännamn (FQDN) för katalogservern. För en dator som till exempel heter x i example.com är FQDN x.example.com. En IP-adress kan användas i stället för FQDN-servernamnet.

**Port:** (obligatoriskt) Den port som katalogservern använder. Vanligtvis 389, eller 636, om SSL-protokollet (Secure Sockets Layer) används för att skicka autentiseringsinformation över nätverket.

**SSL:** (obligatoriskt) Anger om katalogservern använder SSL när data skickas över nätverket. Standardvärdet är Nej. Om du anger Ja måste motsvarande LDAP-servercertifikat betraktas som tillförlitligt av JRE (Java™ runtime environment) på programservern.

**Bindning** (obligatoriskt) Anger hur du ska få åtkomst till katalogen.

**Anonym:** Inget användarnamn eller lösenord krävs.

**Användare:** Autentisering krävs. Ange namnet på den användarpost som har åtkomst till katalogen i rutan Namn. Det är bäst att ange det fullständiga unika namnet (DN) för användarkontot, till exempel cn=Jane Doe, ou=användare, dc=can, dc=com. Ange det associerade lösenordet i rutan Lösenord. Dessa inställningar krävs när du väljer Användare som bindningsalternativ.

**Hämta bas-DN:** (inte obligatoriskt) Hämtar bas-DN:n och visar dem i listrutan. Den här inställningen är användbar när du har flera bas-DN och behöver välja ett värde.

**Bas-DN:** (obligatoriskt) Används som startpunkt för synkronisering av användare och grupper från LDAP-hierarkin. Det är bäst att ange ett bas-DN på den lägsta nivån i hierarkin som omfattar alla användare och grupper som behöver synkroniseras för tjänster. Inkludera inte användarens unika namn i den här inställningen. Om du vill synkronisera en viss användare använder du inställningen Sökfilter.

**Fyll sidan med:** (Inte obligatoriskt) Om du väljer det här alternativet fylls attribut på användar- och gruppinställningssidorna med motsvarande LDAP-standardvärden.

**Sökfilter:** (obligatoriskt) Sökfiltret som ska användas för att hitta posten som är associerad med användaren. Se Syntax för sökfilter.

### Kerberos-inställningar {#kerberos-settings}

Om du konfigurerar autentisering för en företagsdomän eller hybriddomän och väljer Kerberos-autentisering är följande inställningar tillgängliga.

**DNS-IP:** DNS-IP-adressen för den server där AEM-formulär körs. I Windows kan du fastställa den här IP-adressen genom att köra ipconfig /all på kommandoraden.

**KDC-värd:** Fullständigt kvalificerat värdnamn eller IP-adress för den Active Directory-server som används för autentisering.

**Tjänstanvändare:** Om du använder Active Directory 2003 är det här värdet mappningen som skapas för tjänstens huvudnamn i formatet `HTTP/<server name>`. Om du använder Active Directory 2008 är det här värdet användar-ID:t för tjänstens huvudnamn. Anta till exempel att tjänstens huvudnamn heter um spnego, att användar-ID är spnegodemo och att mappningen är HTTP/example.yourcompany.com. Med Active Directory 2003 anger du tjänstanvändaren till HTTP/example.yourcompany.com. Med Active Directory 2008 anger du tjänstanvändaren till spnegodemo. (Se Aktivera enkel inloggning med SPNEGO.)

**Tjänstsfär:** Domännamn för Active Directory

**Lösenord för tjänst:** Lösenord för tjänstanvändare

**Aktivera SPNEGO:** Aktiverar användning av SPNEGO för enkel inloggning (SSO). (Se Aktivera enkel inloggning med SPNEGO.)

### SAML-inställningar {#saml-settings}

Om du konfigurerar autentisering för en företagsdomän eller hybriddomän och väljer SAML-autentisering är följande inställningar tillgängliga. Mer information om ytterligare SAML-inställningar finns i [Konfigurera inställningar för SAML-tjänstleverantör](/help/forms/using/admin-help/configure-saml-service-provider-settings.md#configure-saml-service-provider-settings).

**Välj metadata för SAML-identitetsleverantör
fil som ska importeras:** Klicka på Bläddra för att välja en SAML-metadatafil för identitetsleverantör som genererats från din IDP och klicka sedan på Importera. Information från IDP visas.

**Titel:** Alias för URL:en som anges av EntityID. Titeln visas också på inloggningssidan för Enterprise-användare och lokala användare.

**Identitetsprovidern stöder grundläggande klientautentisering:** Grundläggande klientautentisering används när IDP använder en SAML-artefaktmatchningsprofil. I den här profilen ansluter Hantering av användare tillbaka till en webbtjänst som körs på IDP för att hämta SAML-försäkran. IDP kan kräva autentisering. Om IDP kräver autentisering markerar du det här alternativet och anger ett användarnamn och lösenord i rutorna.

**Anpassade egenskaper:** Du kan ange ytterligare egenskaper. De ytterligare egenskaperna är namn=värde-par avgränsade med nya rader.

Följande anpassade egenskaper krävs om artefaktbindning används.

* Lägg till följande anpassade egenskap för att ange ett användarnamn som representerar AEM formulärtjänstleverantören, som används för att autentisera till tjänsten IDP Artifact Resolution.
  `saml.idp.resolve.username=<username>`

* Lägg till följande anpassade egenskap för att ange lösenordet för användaren som anges i `saml.idp.resolve.username`.
  `saml.idp.resolve.password=<password>`

* Lägg till följande anpassade egenskap så att tjänstleverantören kan ignorera certifikatverifieringen när anslutningen med tjänsten Artefaktmatchning upprättas via SSL.
  `saml.idp.resolve.ignorecert=true`

### Anpassade inställningar {#custom-settings}

Om du konfigurerar autentisering för en företagsdomän eller hybriddomän och väljer Anpassad autentisering, markerar du namnet på den anpassade autentiseringsprovidern.

## Etablering i realtid av användare {#just-in-time-provisioning-of-users}

Etablering i realtid skapar automatiskt en användare i databasen för användarhantering efter att användaren har autentiserats via en autentiseringsprovider. Relevanta roller och grupper tilldelas också dynamiskt till den nya användaren. Du kan aktivera just-in-time-etablering för Enterprise-domäner och hybriddomäner.

Här beskrivs hur traditionell autentisering fungerar i AEM-formulär:

1. När en användare försöker logga in på AEM-formulär skickar användarhanteringen sina inloggningsuppgifter sekventiellt till alla tillgängliga autentiseringsleverantörer. (Inloggningsuppgifterna omfattar kombination av användarnamn/lösenord, Kerberos-biljett, PKCS7-signatur och så vidare.)
1. Autentiseringsprovidern validerar inloggningsuppgifterna.
1. Autentiseringsprovidern kontrollerar sedan om användaren finns i databasen för användarhantering. Följande statusvärden är möjliga:

   **Finns** Om användaren är aktuell och olåst returnerar Hantering av användare autentiseringen. Om användaren inte är aktuell eller låst returneras ett autentiseringsfel.

   **Finns inte** Användarhantering returnerar ett autentiseringsfel.

   **Ogiltig** Användarhantering returnerar ett autentiseringsfel.

1. Resultatet som returneras av autentiseringsprovidern utvärderas. Om autentiseringsprovidern returnerade autentiseringen kan användaren logga in. Annars kontrolleras användarhanteringen med nästa autentiseringsprovider (steg 2-3).
1. Autentiseringsfel returneras om ingen tillgänglig autentiseringsprovider validerar inloggningsuppgifterna.

När bara-i-tid-etablering är aktiverad skapas nya användare dynamiskt i användarhantering om någon av autentiseringsleverantörerna validerar sina autentiseringsuppgifter. (Efter steg 3 i proceduren ovan.)

Autentiseringen misslyckas om en användare autentiseras utan att etablera just-in-time, men inte hittas i databasen för användarhantering. Etablering i realtid lägger till ett steg i autentiseringsproceduren för att skapa användaren och tilldela roller och grupper till användaren.

### Aktivera just-in-time-etablering för en domän {#enable-just-in-time-provisioning-for-a-domain}

1. Skriv en tjänstbehållare som implementerar gränssnitten IdentityCreator och AssignmentProvider. (Se [Programmering med AEM-formulär](https://www.adobe.com/go/learn_aemforms_programming_63).)
1. Distribuera tjänstbehållaren till Forms Server.
1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.

   Välj en befintlig domän eller klicka på Ny företagsdomän.

1. Om du vill skapa en domän klickar du på Ny företagsdomän eller Ny hybrid-domän. Om du vill redigera en befintlig domän klickar du på domänens namn.
1. Markera Aktivera etablering i realtid.

   ***Obs!**Om kryssrutan Aktivera tidsprovisionering för just in-tid saknas klickar du på Hem > Inställningar > Användarhantering > Konfiguration > Avancerade systemattribut och sedan på Läs in igen.*

1. Lägg till autentiseringsproviders. När du lägger till autentiseringsproviders väljer du en registrerad identitetsskapare och tilldelningsprovider på skärmen Ny autentisering. (Se [Konfigurera autentiseringsproviders](configuring-authentication-providers.md#configuring-authentication-providers).)
1. Spara domänen.
