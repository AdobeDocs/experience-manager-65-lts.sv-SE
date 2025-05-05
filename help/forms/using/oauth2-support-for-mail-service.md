---
title: Konfigurera OAuth2-baserad autentisering för Microsoft&reg (Forms JEE OAuth); Office 365 e-postserverprotokoll
description: Konfigurera OAuth2-baserad autentisering för Microsoft&reg (Forms JEE OAuth); Office 365 e-postserverprotokoll
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: a9790625-af8d-4416-b96f-4724a025260b
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Integrera AEM Forms med e-postserverprotokoll från Microsoft® Office 365 {#oauth2-support-for-the-microsoft-mail-server-protocols}

AEM Forms erbjuder OAuth 2.0-stöd för integrering med Microsoft® Office 365-protokoll för e-postservrar, så att organisationer kan uppfylla kraven för e-post. Du kan använda Azure Active Directory (Azure AD) OAuth 2.0-autentiseringstjänsten för att ansluta till olika protokoll som IMAP, POP eller SMTP och få åtkomst till e-postdata för Office 365-användare. Nedan finns stegvisa instruktioner för hur du konfigurerar e-postserverprotokoll för Microsoft® Office 365 för autentisering med hjälp av tjänsten OAuth 2.0:

1. Logga in på [https://portal.azure.com/](https://portal.azure.com/) och sök efter **Azure Active Directory** i sökfältet och klicka på resultatet.
Du kan även bläddra direkt till [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)
1. Klicka på **Lägg till** > **Appregistrering** > **Ny registrering**.

   ![Appregistrering](/help/forms/using/assets/outh_outlook_microsoft_azure.png)

1. Fyll i informationen enligt dina krav och klicka sedan på **Registrera**.
   ![Konto som stöds](/help/forms/using/assets/azure_suuportedaccountype.png)
I ovanstående fall är alternativet **Konton i valfri organisationskatalog (valfri Azure AD-katalog - Multitenant) och personliga Microsoft®-konton (till exempel Skype, Xbox)** valt.

   >[!NOTE]
   >
   > * För **konton i alla organisationskataloger (alla Azure AD-kataloger - Multitenant)**-program rekommenderar Adobe att du använder ett arbetskonto i stället för ett personligt e-postkonto.
   > * **Endast personliga Microsoft®-konton** stöds inte.
   > * Adobe rekommenderar att du använder programmet **Multi-tenant och Personal Microsoft® Account** .

1. Gå sedan till **Certifikat och hemligheter**, klicka på **Ny klienthemlighet** och följ stegen på skärmen för att skapa en hemlighet. Observera detta hemliga värde för senare bruk.

   ![Hemlig nyckel](/help/forms/using/assets/azure_secretkey.png)

1. Om du vill lägga till behörigheter går du till det nya programmet och väljer **API-behörigheter** > **Lägg till en behörighet** > **Microsoft® Graph** > **Delegerade behörigheter**.
1. Markera kryssrutorna för behörigheterna nedan för appen och klicka på **Lägg till behörighet**:

   * `IMAP.AccessUser.All`
   * `Mail.Read`
   * `offline_access`
   * `POP.AccessAsUser.All`
   * `SMTP.Send`
   * `User.Read`

   ![API-behörighet](/help/forms/using/assets/azure_apipermission.png)

1. Välj **Autentisering** > **Lägg till en plattform** > **Webb** och lägg till någon av URI:erna nedan (Universal Resource Identifier) som i avsnittet **Omdirigerings-URL**:
   * `https://login.microsoftonline.com/common/oauth2/nativeclient`
   * `http://localhost`

   I det här fallet används `https://login.microsoftonline.com/common/oauth2/nativeclient` som omdirigerings-URI.

1. Klicka på **Konfigurera** när du har lagt till varje URL och konfigurera inställningarna enligt dina krav.
   ![Omdirigerings-URI](/help/forms/using/assets/azure_redirecturi.png)

   >[!NOTE]
   >
   > Det är obligatoriskt att markera kryssrutorna **Åtkomsttoken** och **ID-token**.

1. Klicka på **Översikt** i den vänstra rutan och kopiera värdena för **Program (klient)-ID**, **Directory (klient)-ID** och **Klienthemlighet** för senare bruk.

   ![Översikt](/help/forms/using/assets/azure_overview.png)

## Generera auktoriseringskoden {#generating-the-authorization-code}

Därefter måste du generera behörighetskoden som beskrivs i följande steg:

1. Öppna följande URL i webbläsaren när du har ersatt `clientID` med `<client_id>` och `redirect_uri` med programmets omdirigerings-URI:

   ```https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=[clientid]&scope=IMAP.AccessAsUser.All%20POP.AccessAsUser.All%20SMTP.Send%20User.Read%20Mail.Read%20offline_access&response_type=code&redirect_uri=[redirect_uri]&prompt=login```

   >[!NOTE]
   >
   > Om det finns ett single tenant-program ersätter du `common` med `[tenantid]` i följande URL för att generera auktoriseringskod: `https://login.microsoftonline.com/[tenantid]/oauth2/v2.0/authorize?client_id=[[clientid]]&scope=IMAP.AccessAsUser.All%20POP.AccessAsUser.All%20SMTP.Send%20User.Read%20Mail.Read%20openid%20offline_access&response_type=code&redirect_uri=[redirect_uri]&prompt=login`

1. När du skriver ovanstående URL omdirigeras du till inloggningsskärmen:
   ![Inloggningsskärm](/help/forms/using/assets/azure_loginscreen.png)

1. Ange e-postadressen, klicka på **Nästa** och behörighetsskärmen för appen visas:

   ![Tillåt behörighet](/help/forms/using/assets/azure_permission.png)

1. När du tillåter behörighet omdirigeras du till en ny URL som: `https://login.microsoftonline.com/common/oauth2/nativeclient?code=<code>&session_state=[session_id]`

1. Kopiera värdet för `<code>` från ovanstående URL från `0.ASY...` till `&session_state` i ovanstående URL.

## Genererar uppdateringstoken {#generating-the-refresh-token}

Därefter måste du generera en uppdateringstoken, som beskrivs i följande steg:

1. Öppna kommandotolken och använd följande cURL-kommando för att hämta refreshToken.

1. Ersätt `clientID`, `client_secret` och `redirect_uri` med värdena för programmet tillsammans med värdet `<code>`:

   `curl -H "ContentType application/x-www-form-urlencoded" -d "client_id=[client-id]&scope=https%3A%2F%2Foutlook.office.com%2FIMAP.AccessAsUser.All%20https%3A%2F%2Foutlook.office.com%2FPOP.AccessAsUser.All%20https%3A%2F%2Foutlook.office.com%2FSMTP.Send%20https%3A%2F%2Foutlook.office.com%2FUser.Read%20https%3A%2F%2Foutlook.office.com%2FMail.Read%20offline_access&code=[code]&grant_type=authorization_code&redirect_uri=[redirect_uri]&client_secret=[secretkey_value]" -X POST https://login.microsoftonline.com/common/oauth2/v2.0/token`

   >[!NOTE]
   >
   > I ett single tenant-program använder du följande cURL-kommando för att generera en uppdateringstoken och ersätter `common` med `[tenantid]` i:
   >`curl -H "ContentType application/x-www-form-urlencoded" -d "client_id=[client-id]&scope=https%3A%2F%2Foutlook.office.com%2FIMAP.AccessAsUser.All%20https%3A%2F%2Foutlook.office.com%2FPOP.AccessAsUser.All%20https%3A%2F%2Foutlook.office.com%2FSMTP.Send%20https%3A%2F%2Foutlook.office.com%2FUser.Read%20https%3A%2F%2Foutlook.office.com%2FMail.Read%20offline_access&code=[code]&grant_type=authorization_code&redirect_uri=[redirect_uri]&client_secret=[secretkey_value]" -X POST https://login.microsoftonline.com/[tenantid]/oauth2/v2.0/token`

1. Notera uppdateringstoken.

## Konfigurera e-posttjänst med stöd för OAuth 2.0 {#configureemailservice}

Konfigurera nu e-posttjänsten på den senaste JEE-servern genom att logga in på Admin-gränssnittet:

1. Gå till **Hem** > **Tjänst** > **Program och tjänster** > **Tjänsthantering** > **E-posttjänst**. Fönstret **E-posttjänst för konfiguration** visas, konfigurerat för grundläggande autentisering.

   >[!NOTE]
   >
   > Om du vill aktivera Autentiseringstjänsten för Auth 2.0 måste du markera kryssrutan **Om SMTP-servern kräver autentisering (SMTP-autentisering)**.

1. Ange **autentiseringsinställningar för autentisering med version 2.0** som `True`.
1. Kopiera värdena för **Klient-ID** och **Klienthemlighet** från Azure Portal.
1. Kopiera värdet för den genererade **Uppdatera token**.
1. Logga in på **Workbench** och sök i **E-post 1.0** från **Aktivitetsväljaren**.
1. Tre alternativ är tillgängliga under E-post 1.0 som:
   * **Skicka med dokument**: Skickar e-post med enstaka bilagor.
   * **Skicka med karta över bifogade filer**: Skickar e-post med flera bifogade filer.
   * **Ta emot**: Tar emot ett e-postmeddelande från IMAP.

   >[!NOTE]
   >
   >* Transportsäkerhetsprotokollet har följande giltiga värden: blank, SSL eller TLS. Ange värden för **SMTP-transportsäkerhet** och **Ta emot transportsäkerhet** till **TLS** för att aktivera autentiseringstjänsten för autentisering.
   >* **POP3-protokollet** stöds inte för OAuth när e-postslutpunkter används.

   ![Anslutningsinställningar](/help/forms/using/assets/oauth_connectionsettings.png)

1. Testa programmet genom att välja **Skicka med dokument**.
1. Ange **TO**- och **From**-adresser.
1. Anropa programmet och ett e-postmeddelande skickas med autentiseringen 0Auth 2.0.

   >[!NOTE]
   >
   >Om du vill kan du ändra autentiseringsinställningen Auth 2.0 till grundläggande autentisering för en viss process i en workbench. Om du vill göra det anger du värdet **OAuth 2.0 Authentication** som Falskt under **Använd globala inställningar** på fliken **Anslutningsinställningar**.

## Så här aktiverar du autenticeringsmeddelanden {#enable_oauth_task}

1. Gå till **Hem** > **Tjänster** > **Formulärarbetsflöde** > **Serverinställningar** > **E-postinställningar**
1. Markera kryssrutan **Aktivera autentisering** om du vill aktivera autenticeringsmeddelanden.
1. Kopiera värdena för **Klient-ID** och **Klienthemlighet** från Azure Portal.
1. Kopiera värdet för den genererade **Uppdatera token**.
1. Klicka på **Spara** för att spara informationen.

   ![Aktivitetsmeddelande](/help/forms/using/assets/task_notification.png)

   >[!NOTE]
   >
   > [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-65-lts/content/forms/administrator-help/configuring-email-endpoints.html?lang=sv-SE#create-an-email-endpoint-for-the-complete-task-service) om du vill veta mer om aktivitetsmeddelanden.

## Konfigurera e-postslutpunkt {#configure_email_endpoint}

1. Gå till **Hem** > **Tjänster** > **Program och tjänster** > **Slutpunktshantering**
1. Om du vill konfigurera e-postslutpunkten anger du **autentiseringsinställningarna för autentisering** till `True`.
1. Kopiera värdena för **Klient-ID** och **Klienthemlighet** från Azure Portal.
1. Kopiera värdet för den genererade **Uppdatera token**.
1. Klicka på **Spara** för att spara informationen.

   ![Anslutningsinställningar](/help/forms/using/assets/oauth_emailendpoint.png)

   >[!NOTE]
   >
   > Om du vill ha mer information om hur du konfigurerar e-postslutpunkter klickar du på [Konfigurera en e-postslutpunkt](https://experienceleague.adobe.com/docs/experience-manager-65-lts/content/forms/administrator-help/configuring-email-endpoints.html?lang=sv-SE).

## Felsökning {#troubleshooting}

* Om e-posttjänsten inte fungerar som den ska kan du försöka återskapa `Refresh Token` enligt beskrivningen ovan. Det tar några minuter innan det nya värdet distribueras.

* Ett fel uppstod när e-postserverinformation konfigurerades i e-postslutpunkten med Workbench. Försök att konfigurera slutpunkten med hjälp av administratörsgränssnittet i stället för Workbench.
