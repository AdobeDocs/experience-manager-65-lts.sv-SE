---
title: Microsoft Dynamics OData-konfiguration
description: Lär dig hur du använder, integrerar och arbetar med Microsoft Dynamics onlinetjänster och lokala tjänster via formulärdatamodell.
topic-tags: integration
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
feature: Form Data Model
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 6c3c4d7f-fc4c-44ad-886f-f76d0532d91a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 0%

---

# Microsoft Dynamics OData-konfiguration{#microsoft-dynamics-odata-configuration}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/ms-dynamics-odata-configuration.html) |
| AEM 6.5 | Den här artikeln |

![dataintegrering](assets/data-integeration.png)

Microsoft Dynamics är en CRM- och ERP-programvara (Enterprise Resource Planning) som används för att skapa och hantera kundkonton, kontakter, leads, möjligheter och ärenden. [AEM Forms Data Integration](../../forms/using/data-integration.md) tillhandahåller en OData-molntjänstkonfiguration som kan integrera Forms med både online- och lokal Microsoft Dynamics-server. Det gör att du kan skapa en formulärdatamodell som bygger på de enheter, attribut och tjänster som definieras i Microsoft Dynamics-tjänsten. Formulärdatamodellen kan användas för att skapa anpassade formulär som interagerar med Microsoft Dynamics-servern för att möjliggöra affärsarbetsflöden. Till exempel:

* Fråga Microsoft Dynamics-servern efter data och förifylla adaptiva formulär
* Skriva data i Microsoft Dynamics när blanketter skickas in på ett flexibelt sätt
* Skriv data i Microsoft Dynamics via anpassade entiteter som definierats i formulärdatamodellen och omvänt

AEM Forms tilläggspaket innehåller även OData-konfiguration för referens som du kan använda för att snabbt integrera Microsoft Dynamics med AEM Forms.

När paketet är installerat är följande enheter och tjänster tillgängliga på din AEM Forms-instans:

* MS Dynamics OData Cloud Service (OData-tjänsten)
* Formulärdatamodell med förkonfigurerade Microsoft Dynamics-enheter och -tjänster.

Förkonfigurerade Microsoft Dynamics-enheter och -tjänster i en formulärdatamodell är bara tillgängliga på din AEM Forms-instans om körningsläget för AEM-instansen är inställt på `samplecontent` (standard). MS Dynamics OData Cloud Service (OData Service) är även tillgänglig med andra körningslägen. Mer information om hur du konfigurerar körningslägen för en AEM-instans finns i [Körningslägen](/help/sites-deploying/configure-runmodes.md).

## Förutsättningar {#prerequisites}

Innan du börjar konfigurera och konfigurera Microsoft Dynamics måste du se till att du har:

* [AEM Forms tilläggspaket](../../forms/using/installing-configuring-aem-forms-osgi.md) har installerats
* Konfigurerade Microsoft Dynamics 365 online eller installerade en instans av någon av följande Microsoft Dynamics-versioner:

   * Microsoft Dynamics 365 lokalt
   * Lokal Microsoft Dynamics 2016

* [Registrerade programmet för Microsoft Dynamics onlinetjänst med Microsoft Azure Active Directory](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/walkthrough-register-dynamics-365-app-azure-active-directory). Notera värdena för klient-ID (kallas även program-ID) och klienthemlighet för den registrerade tjänsten. Dessa värden används när [konfigurerar molntjänsten för din Microsoft Dynamics-tjänst](../../forms/using/ms-dynamics-odata-configuration.md#configure-cloud-service-for-your-microsoft-dynamics-service).

## Ange svars-URL för registrerat Microsoft Dynamics-program {#set-reply-url-for-registered-microsoft-dynamics-application}

Gör så här för att ange svars-URL:en för ett registrerat Microsoft Dynamics-program:

>[!NOTE]
>
>Använd bara den här proceduren när du integrerar AEM Forms med Microsoft Dynamics onlineserver.

1. Gå till Microsoft Azure Active Directory-kontot och lägg till följande URL för molntjänstkonfiguration i **Svara-URL:er**-inställningarna för det registrerade programmet:

   `https://'[server]:[port]'/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html`

   ![Azure-katalog](assets/azure_directory_new.png)

1. Spara konfigurationen.

## Konfigurera Microsoft Dynamics för IFD {#configure-microsoft-dynamics-for-ifd}

Microsoft Dynamics använder anspråksbaserad autentisering för att ge externa användare åtkomst till data på Microsoft Dynamics CRM-servern. Om du vill aktivera detta gör du så här för att konfigurera Microsoft Dynamics för Internet-baserad distribution (IFD) och konfigurera anspråksinställningar.

>[!NOTE]
>
>Använd bara den här proceduren när du integrerar AEM Forms med en lokal Microsoft Dynamics-server.

1. Konfigurera Microsoft Dynamics lokala instans för IFD enligt beskrivningen i [Konfigurera IFD för Microsoft Dynamics](https://technet.microsoft.com/en-us/library/dn609803.aspx).
1. Kör följande kommandon med Windows PowerShell för att konfigurera anspråksinställningar för IFD-aktiverade Microsoft Dynamics:

   ```shell
   Add-PSSnapin Microsoft.Crm.PowerShell
    $ClaimsSettings = Get-CrmSetting -SettingType OAuthClaimsSettings
    $ClaimsSettings.Enabled = $true
    Set-CrmSetting -Setting $ClaimsSettings
   ```

   Mer information finns i [Appregistrering för lokal CRM (IFD)](https://msdn.microsoft.com/sl-si/library/dn531010(v=crm.7).aspx#bkmk_ifd).

## Konfigurera OAuth-klient på AD FS-dator {#configure-oauth-client-on-ad-fs-machine}

Gör följande för att registrera en OAuth-klient på AD FS-datorn (Active Directory Federation Services) och bevilja åtkomst på AD FS-datorn:

>[!NOTE]
>
>Använd bara den här proceduren när du integrerar AEM Forms med en lokal Microsoft Dynamics-server.

1. Kör följande kommando:

   `Add-AdfsClient -ClientId "<Client-ID>" -Name "<name>" -RedirectUri "<redirect-uri>" -GenerateClientSecret`

   Var:

   * `Client-ID` är ett klient-ID som du kan generera med valfri GUID-generator.
   * `redirect-uri` är URL:en till Microsoft Dynamics OData-molntjänsten på AEM Forms. Standardmolntjänsten som installeras med AEM Forms-paketet distribueras på följande URL:
     `https://'[server]:[port]'/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html`

1. Kör följande kommando för att bevilja åtkomst på AD FS-datorn:

   `Grant-AdfsApplicationPermission -ClientRoleIdentifier "<Client-ID>" -ServerRoleIdentifier <resource> -ScopeNames openid`

   Var:

   * `resource` är Microsoft Dynamics organisations-URL.

1. Microsoft Dynamics använder HTTPS-protokoll. Om du vill anropa AD FS-slutpunkter från Forms-servern installerar du Microsoft Dynamics platscertifikat till Java-certifikatarkivet med kommandot `keytool` på den dator som kör AEM Forms.

## Konfigurera molntjänsten för din Microsoft Dynamics-tjänst {#configure-cloud-service-for-your-microsoft-dynamics-service}

Konfigurationen **MS Dynamics OData Cloud Service (OData Service)** levereras med OData-standardkonfigurationen. Så här konfigurerar du den för att ansluta till din Microsoft Dynamics-tjänst.

1. Navigera till **[!UICONTROL Tools > Cloud Services > Data Sources]** och markera konfigurationsmappen `global`.
1. Välj konfigurationen **MS Dynamics OData Cloud Service (OData Service)** och välj **[!UICONTROL Properties]**. Dialogrutan för konfigurationsegenskapen för molntjänster öppnas.

   På fliken **Autentiseringsinställningar**:

   1. Ange värdet för fältet **Tjänstrot**. Gå till Dynamics-instansen och navigera till **Resurser för utvecklare** för att visa värdet för fältet Tjänstrot. Till exempel https://&lt;tenant-name>/api/data/v9.1/

   1. Ersätt standardvärdena i fälten **Klient-ID** (kallas även **Program-ID**), **Klienthemlighet**, **OAuth URL**, **Uppdatera token-URL**, **Åtkomsttoken-URL** och **Resurs** med värden från din Microsoft Dynamics-tjänstkonfiguration. Det är obligatoriskt att ange den dynamiska instansens URL i fältet **Resurs** för att konfigurera Microsoft Dynamics med en formulärdatamodell. Använd tjänstens rot-URL för att härleda den dynamiska instansens URL. Exempel: [https://org.crm.dynamics.com](https://org.crm.dynamics.com/).

   1. Ange **openid** i fältet **Auktoriseringsomfång** för auktoriseringsprocess på Microsoft Dynamics.

   ![Autentiseringsinställningar](assets/dynamics_authentication_settings_new.png)

1. Klicka på **[!UICONTROL Connect to OAuth]**. Du omdirigeras till Microsoft Dynamics inloggningssida.
1. Logga in med dina Microsoft Dynamics-inloggningsuppgifter och godkänn för att tillåta molntjänstkonfigurationen att ansluta till Microsoft Dynamics-tjänsten. Det är en engångsuppgift att upprätta en anslutning mellan molntjänsten och tjänsten.

   Du omdirigeras sedan till konfigurationssidan för molntjänsten, som visar ett meddelande om att OData-konfigurationen har sparats.

Molntjänsten MS Dynamics OData Cloud Service (OData Service) är konfigurerad och ansluten till Dynamics-tjänsten.

## Skapa formulärdatamodell {#create-form-data-model}

När du installerar AEM Forms-paketet distribueras en formulärdatamodell, **Microsoft Dynamics FDM**, på din AEM-instans. Som standard använder formulärdatamodellen Microsoft Dynamics-tjänst som konfigurerats i MS Dynamics OData Cloud Service (OData Service) som datakälla.

När formulärdatamodellen öppnas för första gången ansluter den till den konfigurerade Microsoft Dynamics-tjänsten och hämtar enheter från din Microsoft Dynamics-instans. Enheterna&quot;contact&quot; och&quot;lead&quot; från Microsoft Dynamics har redan lagts till i formulärdatamodellen.

Gå till **[!UICONTROL Forms > Data Integrations]** om du vill granska formulärdatamodellen. Välj **Microsoft Dynamics FDM** och klicka på **Redigera** för att öppna formulärdatamodellen i redigeringsläge. Du kan även öppna formulärdatamodellen direkt från följande URL:

`https://'[server]:[port]'/aem/fdm/editor.html/content/dam/formsanddocuments-fdm/ms-dynamics-fdm`

![default-fdm-1](assets/default-fdm-1.png)

Därefter kan du skapa ett anpassat formulär baserat på formulärdatamodellen och använda det i olika varianter av formuläranvändningen, till exempel:

* Fyll i anpassat formulär i förväg genom att fråga information från Microsoft Dynamics enheter och tjänster
* Anropa Microsoft Dynamics serveråtgärder som definieras i en formulärdatamodell med hjälp av adaptiva formulärregler
* Skriv skickade formulärdata till Microsoft Dynamics-enheter

Vi rekommenderar att du skapar en kopia av den formulärdatamodell som medföljer AEM Forms-paketet och konfigurerar datamodeller och tjänster så att de passar dina behov. Det ser till att eventuella framtida uppdateringar av paketet inte åsidosätter formulärdatamodellen.

Mer information om hur du skapar och använder formulärdatamodell i arbetsflöden finns i [Dataintegrering](../../forms/using/data-integration.md).
