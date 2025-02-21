---
title: Konfigurera resurstaggning med Smart Content Service
description: Lär dig hur du konfigurerar smart taggning och förbättrad smart taggning i  [!DNL Adobe Experience Manager] med hjälp av Smart Content Service.
role: Admin
feature: Tagging,Smart Tags
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1914'
ht-degree: 12%

---

# Förbered [!DNL Assets] för smart taggning {#configure-asset-tagging-using-the-smart-content-service}

Innan du kan börja tagga dina resurser med smarta innehållstjänster måste du integrera [!DNL Experience Manager Assets] med Adobe Developer Console för att använda den smarta tjänsten i [!DNL Adobe Sensei]. När konfigurationen är klar kan du utbilda tjänsten med några bilder och en tagg.

<!--
>[!NOTE]
>
>* Smart Content Services is no longer available to new [!DNL Experience Manager Assets] On-Premise customers. Existing On-Premise customers, who already have this capability enabled, can continue using Smart Content Services.
>* Smart Content Services is available for existing [!DNL Experience Manager Assets] Managed Services customers, who already have this capability enabled.
>* New Experience Manager Assets Managed Services customers can follow the instructions mentioned in this article to set up Smart Content Services.
>* For Service Pack 20 and older, you need to perform the workaround steps for SCS to support Oauth integration. See [Troubleshooting smart tags for OAuth credentials](config-oauth.md).
>* To support the Oauth integration on Service Pack 21, you need to install the [Hotfix for SP 21](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Fproduct%2Fassets%2Fcq-6.5.0-hotfix-40772-1.2.zip). 
>* For Existing SCS configuration, the process is the same as setting up a new OAuth integration. Any legacy configuration will be automatically cleaned up.
-->

Innan du använder tjänsten för smart innehåll bör du kontrollera följande:

* [Integrera med Adobe Developer Console](#integrate-adobe-io).
* [Logga in på tjänsten för smart innehåll](#training-the-smart-content-service).

* Installera den senaste [[!DNL Experience Manager] Service Pack](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html).

## SCS-uppgradering med stöd för Oauth för Adobe Managed Services {#scs-upgrade-oauth-managed-services}

**Nya användare**

Installera Service Pack 2. Om du vill ha stöd för OAuth-integrering i Service Pack 21 måste du installera [hotfix för Service Pack 21](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Fproduct%2Fassets%2Fcq-6.5.0-hotfix-40772-1.2.zip).

Följ instruktionerna i den här artikeln för att konfigurera tjänster för smart innehåll.

**Befintliga användare**

Om du har uppgraderat till Service Pack 21 installerar du [hotfixen för SP 21](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Fproduct%2Fassets%2Fcq-6.5.0-hotfix-40772-1.2.zip) som stöd för Oauth-integrering. Alla befintliga konfigurationer tas bort automatiskt. Följ instruktionerna i den här artikeln för att konfigurera tjänster för smart innehåll.

För Service Pack 20 och äldre måste du utföra de tillfälliga åtgärderna för SCS för att stödja Oauth-integrering. Se [Felsöka smarta taggar för OAuth-autentiseringsuppgifter](config-oauth.md).

## SCS-uppgradering med stöd för Oauth för lokala användare {#scs-upgrade-oauth-on-premise}

**Nya användare**

Smarta innehållstjänster är inte längre tillgängliga för nya [!DNL Experience Manager Assets] On-Premise-användare.

**Befintliga användare**

Befintliga lokala användare, som redan har den här funktionen aktiverad, kan fortsätta använda smarta innehållstjänster.

Om du har uppgraderat till Service Pack 21 installerar du [hotfixen för SP 21](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Fproduct%2Fassets%2Fcq-6.5.0-hotfix-40772-1.2.zip) som stöd för Oauth-integrering. Alla befintliga konfigurationer tas bort automatiskt. Följ instruktionerna i den här artikeln för att konfigurera tjänster för smart innehåll.

För Service Pack 20 och äldre måste du utföra de tillfälliga åtgärderna för SCS för att stödja Oauth-integrering. Se [Felsöka smarta taggar för OAuth-autentiseringsuppgifter](config-oauth.md).


## Integrera med Adobe Developer Console {#integrate-adobe-io}

När du integrerar med Adobe Developer Console autentiserar servern [!DNL Experience Manager] dina tjänstinloggningsuppgifter med Adobe Developer Console gateway innan du vidarebefordrar din begäran till Smart Content Service. För att kunna integrera behöver du ett Adobe ID-konto som har administratörsbehörighet för organisationen och som har köpts och aktiverats för din organisation.

Så här konfigurerar du tjänsten Smart Content:

1. Skapa en integrering i [Adobe Developer Console](#create-adobe-io-integration).

1. Skapa [IMS-konfiguration för tekniskt konto](#create-ims-account-config) med API-nyckeln och andra autentiseringsuppgifter från Adobe Developer Console.

1. [Konfigurera Smart Content-tjänsten](#configure-smart-content-service).

1. [Testa konfigurationen](#validate-the-configuration).

<!--
To configure the Smart Content Service, follow these top-level steps:

1. To generate a public key, [Create a Smart Content Service] (#obtain-public-certificate) configuration in [!DNL Experience Manager]. 

1. Optionally, [enable auto-tagging on asset upload](#enable-smart-tagging-in-the-update-asset-workflow-optional).

   <!--1. [Obtain public certificate](#obtain-public-certificate) for OAuth integration.
   1. [Create an integration in Adobe Developer Console](#create-adobe-i-o-integration) and upload the generated public key.

   1. [Configure your deployment](#configure-smart-content-service) using the API key and other credentials from Adobe Developer Console.

   1. [Test the configuration](#validate-the-configuration).-->

### Integrering med Adobe Developer Console {#create-adobe-io-integration}

Om du vill använda API:er för smarta innehållstjänster skapar du en integrering i Adobe Developer Console för att få [!UICONTROL API Key] (som genereras i fältet [!UICONTROL CLIENT ID] för Adobe Developer Console-integrering), [!UICONTROL ORGANIZATION ID] och [!UICONTROL CLIENT SECRET] för [!UICONTROL Assets Smart Tagging Service Settings] av molnkonfigurationen i [!DNL Experience Manager].

1. Gå till [https://developer.adobe.com](https://developer.adobe.com/) i en webbläsare. Välj rätt konto och verifiera att den associerade organisationsrollen är system **administrator**.

1. Skapa ett projekt med valfritt namn. Klicka på **[!UICONTROL Add API]**.

1. På sidan **[!UICONTROL Add an API]** markerar du **[!UICONTROL Experience Cloud]** och väljer **[!UICONTROL Smart Content]**. Klicka på **[!UICONTROL Next]**.

1. Välj **[!UICONTROL OAuth Server-to-Server]**. Klicka på **[!UICONTROL Next]**.
Mer information om hur du gör den här konfigurationen finns i Developer Console-dokumentationen, beroende på dina krav:

   * Översikt
      * [Server till server-autentisering](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/)

   * Skapa en ny OAuth-autentiseringsuppgift:
      * [Implementeringshandbok för autentiseringsuppgifter för OAuth Server-till-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/)

   * Migrera en befintlig JWT-autentiseringsuppgift till en OAuth-autentiseringsuppgift:
      * [Migrerar från JWT-autentiseringsuppgifter (Service Account) till autentiseringsuppgifter för OAuth Server-till-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/)


1. Välj **[!UICONTROL Smart Content Services]** på sidan **[!UICONTROL Select product profiles]**. Klicka på **[!UICONTROL Save configured API]**.

   En sida visar mer information om konfigurationen. Håll den här sidan öppen för att kopiera och lägga till dessa värden i [!UICONTROL Assets Smart Tagging Service Settings] av molnkonfigurationen i [!DNL Experience Manager] för att konfigurera smarta taggar.

   ![OAuth-autentiseringsuppgifter i Developer Console](assets/ims-configuration-developer-console.png)

### Skapa konfiguration av tekniskt IMS-konto {#create-ims-account-config}

Du måste skapa en teknisk kontokonfiguration för IMS enligt stegen nedan:

1. I användargränssnittet för [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Security]** > **[!UICONTROL Adobe IMS Configurations]**.

1. Klicka på **[!UICONTROL Create]**.

1. Använd följande värden i dialogrutan Konfiguration av IMS-konto:

   ![Konfigurationsfönstret för Adobe IMS](assets/adobe-ims-config.png)

   | Fält | Beskrivning |
   | -------- | ---------------------------- |
   | Molnlösning | Välj **[!UICONTROL Smart Tags]** i listrutan. |
   | Titel | Lägg till namnet på det konfigurerande IMS-kontot. |
   | Auktoriseringsserver | Lägg till `https://ims-na1.adobelogin.com` |
   | Klient-ID | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |
   | Klienthemlighet | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |
   | Omfång | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |
   | Organisations-ID | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |

1. Markera konfigurationen som du har skapat och klicka på **[!UICONTROL Check Health]**.

1. Bekräfta dialogrutan för att kontrollera hälsa och klicka på Stäng när konfigurationen är i felfritt läge.

### Skapa en ny konfiguration {#configure-smart-content-service}

<!--
>[!CAUTION]
>
>Previously, configurations that were made with JWT Credentials are now subject to deprecation in the Adobe Developer Console. You cannot create new JWT credentials after June 3, 2024. Such configurations can no longer be created or updated, but can be migrated to OAuth configurations.
> See [Setting up IMS integrations for AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/setting-up-ims-integrations-for-aem-as-a-cloud-service)
>See [Steps to configure OAuth for on-premise users](#config-oauth-onprem)
> See [Troubleshooting smart tags for OAuth credentials](#config-smart-tagging.md)
-->

Om du vill konfigurera integreringen använder du värdena för fälten [!UICONTROL TECHNICAL ACCOUNT ID], [!UICONTROL ORGANIZATION ID], [!UICONTROL CLIENT SECRET] och [!UICONTROL CLIENT ID] från Adobe Developer Console-integreringen. Om du skapar en molnkonfiguration för smarta taggar kan API-begäranden från distributionen [!DNL Experience Manager] autentiseras.

1. I [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Cloud Service]** > **[!UICONTROL Smart Tag]** för att öppna [!UICONTROL Smart Tag Configurations].

1. Klicka på **[!UICONTROL Create]** om du vill skapa en ny konfiguration. Annars klickar du på **[!UICONTROL Properties]** för att uppdatera den befintliga konfigurationen.

1. Fyll i följande fält:

   ![Konfiguration av smarta taggar](assets/smart-tags-config.png)

   | Fält | Beskrivning |
   | -------- | ---------------------------- |
   | Titel | Lägg till namnet på det konfigurerande IMS-kontot. |
   | Associerad Adobe IMS-konfiguration | Välj konfiguration i listrutan. |
   | Tjänst-URL | `https://smartcontent.adobe.io/<region where your Experience Manager author instance is hosted>`. Exempel: `https://smartcontent.adobe.io/apac`. Du kan ange `na`, `emea` eller `apac` som de områden där din Experience Manager-författarinstans finns. |

   >[!NOTE]
   >
   >Om Experience Manager hanterade tjänst etableras före 1 september 2022 använder du följande tjänst-URL:
   >`https://mc.adobe.io/marketingcloud/smartcontent`

1. Klicka på **[!UICONTROL Save & Close]**.

### Validera konfigurationen {#validate-the-configuration}

När du har slutfört konfigurationen kan du använda en JMX MBean för att validera konfigurationen. Följ de här stegen för att validera.

1. Gå till din [!DNL Experience Manager]-server på `https://[aem_server]:[port]`.

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]** för att öppna OSGi-konsolen. Klicka på **[!UICONTROL Main]>[!UICONTROL JMX]**.

<!--
1. Click `com.day.cq.dam.similaritysearch.internal.impl`. It opens **[!UICONTROL SimilaritySearch Miscellaneous Tasks]**.-->

1. Klicka på `com.day.cq.dam.similaritysearch.internal.impl (SCS)`.

   ![Mbean-fönster](assets/mbean.png)

1. Klicka på `validateConfigs()`. I dialogrutan **[!UICONTROL Validate Configurations]** klickar du på **[!UICONTROL Invoke]**.

Valideringsresultaten visas i samma dialogruta.

<!--
### Obtain public certificate by creating Smart Content Service configuration {#obtain-public-certificate}

A public certificate lets you authenticate your profile on Adobe Developer Console.

1. In the [!DNL Experience Manager] user interface, access **[!UICONTROL Tools]** > **[!UICONTROL Cloud Services]** > **[!UICONTROL Legacy Cloud Services]**.

1. In the Cloud Services page, click **[!UICONTROL Configure Now]** under **[!UICONTROL Assets Smart Tags]**.

1. In the **[!UICONTROL Create Configuration]** dialog, specify a title and name for the Smart Tags configuration. Click **[!UICONTROL Create]**.

1. In the **[!UICONTROL AEM Smart Content Service]** dialog, use the following values:

   **[!UICONTROL Service URL]**: `https://smartcontent.adobe.io/<region where your Experience Manager author instance is hosted>`

   For example, `https://smartcontent.adobe.io/apac`. You can specify `na`, `emea`, or, `apac` as the regions where your Experience Manager author instance is hosted. 

   >[!NOTE]
   >
   >If the Experience Manager Managed Service is provisioned before September 01, 2022, use the following Service URL:
   >`https://mc.adobe.io/marketingcloud/smartcontent`

   **[!UICONTROL Authorization Server]**: `https://ims-na1.adobelogin.com`

   Leave the other fields blank for now (to be provided later). Click **[!UICONTROL OK]**.

   ![Experience Manager Smart Content Service dialog to provide content service URL](assets/aem_scs.png)


   *Figure: Smart Content Service dialog to provide content service URL*

   >[!NOTE]
   >
   >The URL provided as [!UICONTROL Service URL] is not accessible via browser and generates a 404 error. The configuration works OK with the same value of the [!UICONTROL Service URL] parameter. For the overall service status and maintenance schedule, see [https://status.adobe.com](https://status.adobe.com).

1. Click **[!UICONTROL Download Public Certificate for OAuth Integration]**, and download the public certificate file `AEM-SmartTags.crt`.

   ![A representation of the settings created for the smart tagging service](assets/smart-tags-download-public-cert.png)


   *Figure: Settings for smart tagging service.*

#### Reconfigure when a certificate expires {#certrenew}

After a certificate expires, it is no longer trusted. You cannot renew an expired certificate. To add a certificate, follow these steps.

1. Log in your [!DNL Experience Manager] deployment as an administrator. Click **[!UICONTROL Tools]** > **[!UICONTROL Security]** > **[!UICONTROL Users]**.

1. Locate and click **[!UICONTROL dam-update-service]** user. Click **[!UICONTROL Keystore]** tab.

1. Delete the existing **[!UICONTROL similaritysearch]** keystore with the expired certificate. Click **[!UICONTROL Save & Close]**.

   ![Delete the existing similarity search entry in Keystore to add a security certificate](assets/smarttags_delete_similaritysearch_keystore.png)


   *Figure: Delete the existing `similaritysearch` entry in Keystore to add a security certificate.*

1. Navigate to **[!UICONTROL Tools]** > **[!UICONTROL Cloud Services]** > **[!UICONTROL Legacy Cloud Services]**. Click **[!UICONTROL Asset Smart Tags]** > **[!UICONTROL Show Configuration]** > **[!UICONTROL Available Configurations]**. Click the required configuration.  

1. To download a public certificate, click **[!UICONTROL Download Public Certificate for OAuth Integration]**.

1. Access [https://console.adobe.io](https://console.adobe.io) and navigate to the existing Smart Content Services on the **[!UICONTROL Integrations]** page. Upload the new certificate. For more information, see the instructions in [Create Adobe Developer Console integration](#create-adobe-i-o-integration).

### Create Adobe Developer Console integration {#create-adobe-i-o-integration}

To use Smart Content Service APIs, create an integration in Adobe Developer Console to obtain [!UICONTROL API Key] (generated in [!UICONTROL CLIENT ID] field of Adobe Developer Console integration), [!UICONTROL TECHNICAL ACCOUNT ID], [!UICONTROL ORGANIZATION ID], and [!UICONTROL CLIENT SECRET] for [!UICONTROL Assets Smart Tagging Service Settings] of cloud configuration in [!DNL Experience Manager].

1. Access [https://console.adobe.io](https://console.adobe.io/) in a browser. Select the appropriate account and verify that the associated organization role is system administrator.

1. Create a project with any desired name. Click **[!UICONTROL Add API]**.

1. On the **[!UICONTROL Add an API]** page, select **[!UICONTROL Experience Cloud]** and select **[!UICONTROL Smart Content]**. Click **[!UICONTROL Next]**.

1. Select **[!UICONTROL Upload your public key]**. Provide the certificate file downloaded from [!DNL Experience Manager]. A message [!UICONTROL Public key(s) uploaded successfully] is displayed. Click **[!UICONTROL Next]**.

   [!UICONTROL Create a new Service Account (JWT) credential] page displays the public key for the service account.

1. Click **[!UICONTROL Next]**.

1. On the **[!UICONTROL Select product profiles]** page, select **[!UICONTROL Smart Content Services]**. Click **[!UICONTROL Save configured API]**.

   A page displays more information about the configuration. Keep this page open to copy and add these values in [!UICONTROL Assets Smart Tagging Service Settings] of cloud configuration in [!DNL Experience Manager] to configure smart tags.

   ![In the Overview tab, you can review the information provided for integration.](assets/integration_details.png)


   *Figure: Details of integration in Adobe Developer Console*

### Configure Smart Content Service {#configure-smart-content-service}

>[!CAUTION]
>
>Previously, configurations that were made with JWT Credentials are now subject to deprecation in the Adobe Developer Console. You cannot create new JWT credentials after June 3, 2024. Such configurations can no longer be created or updated, but can be migrated to OAuth configurations.
> See [Setting up IMS integrations for AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/setting-up-ims-integrations-for-aem-as-a-cloud-service)
>See [Steps to configure OAuth for on-premise users](#config-oauth-onprem)
> See [Troubleshooting smart tags for OAuth credentials](#config-smart-tagging.md)

To configure the integration, use the values of [!UICONTROL TECHNICAL ACCOUNT ID], [!UICONTROL ORGANIZATION ID], [!UICONTROL CLIENT SECRET], and [!UICONTROL CLIENT ID] fields from the Adobe Developer Console integration. Creating a Smart Tags cloud configuration allows authentication of API requests from the [!DNL Experience Manager] deployment.

1. In [!DNL Experience Manager], navigate to **[!UICONTROL Tools]** > **[!UICONTROL Cloud Service]** > **[!UICONTROL Legacy Cloud Services]** to open the [!UICONTROL Cloud Services] console.

1. Under the **[!UICONTROL Assets Smart Tags]**, open the configuration created above. On the service settings page, click **[!UICONTROL Edit]**.

1. In the **[!UICONTROL AEM Smart Content Service]** dialog, use the pre-populated values for the **[!UICONTROL Service URL]** and **[!UICONTROL Authorization Server]** fields.

1. For the fields [!UICONTROL Api Key], [!UICONTROL Technical Account ID], [!UICONTROL Organization ID], and [!UICONTROL Client Secret], copy and use the following values generated in [Adobe Developer Console integration](#create-adobe-i-o-integration).

   | [!UICONTROL Assets Smart Tagging Service Settings] | [!DNL Adobe Developer Console] integration fields |
   |--- |--- |
   | [!UICONTROL Api Key] | [!UICONTROL CLIENT ID] |
   | [!UICONTROL Technical Account ID] | [!UICONTROL TECHNICAL ACCOUNT ID] |
   | [!UICONTROL Organization ID] | [!UICONTROL ORGANIZATION ID] |
   | [!UICONTROL Client Secret] | [!UICONTROL CLIENT SECRET] |

### Configure OAuth for on-premise users {#config-oauth-onprem}

#### Prerequisites {#prereqs-config-oauth-onprem}

An authorization scope is an OAuth string that contains the following prerequisites:

* Create a new OAuth integration in the [Developer Console](https://developer.adobe.com/console/user/servicesandapis) using `ClientID`, `ClientSecretID`, and `OrgID`.
* Add the following files at this path `/apps/system/config in crx/de`:
   * `com.adobe.granite.auth.oauth.accesstoken.provider.<randomnumbers>.config`
   * `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl.<randomnumber>.config`

#### Configure OAuth for on-premise users {#steps-config-oauth-onprem}

1. Add or update the below properties in `com.adobe.granite.auth.oauth.accesstoken.provider.<randomnumbers>.config`:

   * `auth.token.provider.authorization.grants="client_credentials"`
   * `auth.token.provider.orgId="<OrgID>"`
   * `auth.token.provider.default.claims=("\"iss\"\ :\ \"<OrgID>\"")`
   * `auth.token.provider.scope="read_pc.dma_smart_content,\ openid,\ AdobeID,\ additional_info.projectedProductContext"`
     `auth.token.validator.type="adobe-ims-similaritysearch"`
   * Update the `auth.token.provider.client.id` with the Client ID of the new OAuth configuration.
   * Update `auth.access.token.request` to `"https://ims-na1.adobelogin.com/ims/token/v3"`
2. Rename the file to `com.adobe.granite.auth.oauth.accesstoken.provider-<randomnumber>.config`.
3. Perform the steps below in `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl.<randomnumber>.config`:
   * Update the property auth.ims.client.secret with the Client Secret from the new OAuth integration.
   * Rename the file to `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl-<randomnumber>.config`
4. Save all the changes in content repository development console, for example, CRXDE.
5. Navigate to `/system/console/configMgr` and replace the OSGi configuration from `.<randomnumber>` to `-<randomnumber>`.
6. Delete the old configuration for `"Access Token provider name: adobe-ims-similaritysearch"` in `/system/console/configMgr`.
7. Restart the console.

### Validate the configuration {#validate-the-configuration}

After you have completed the configuration, you can use a JMX MBean to validate the configuration. To validate, follow these steps.

1. Access your [!DNL Experience Manager] server at `https://[aem_server]:[port]`.

1. Go to **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]** to open the OSGi console. Click **[!UICONTROL Main] > [!UICONTROL JMX]**.

1. Click `com.day.cq.dam.similaritysearch.internal.impl`. It opens **[!UICONTROL SimilaritySearch Miscellaneous Tasks]**.

1. Click `validateConfigs()`. In the **[!UICONTROL Validate Configurations]** dialog, click **[!UICONTROL Invoke]**.

The validation results are displayed in the same dialog.
-->

### Aktivera smart taggning i arbetsflödet [!UICONTROL DAM Update Asset] (valfritt) {#enable-smart-tagging-in-the-update-asset-workflow-optional}

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]** i [!DNL Experience Manager].

1. Välj arbetsflödesmodellen **[!UICONTROL DAM Update Asset]** på sidan **[!UICONTROL Workflow Models]**.

1. Klicka på **[!UICONTROL Edit]** i verktygsfältet.

1. Expandera sidopanelen för att visa stegen. Dra steget **[!UICONTROL Smart Tag Asset]** som finns i avsnittet med DAM-arbetsflöden och placera det efter steget **[!UICONTROL Process Thumbnails]**.

   ![Lägg till resurssteget för smarta taggar efter steget med processminiatyrer i arbetsflödet för DAM-uppdatering av resurser](assets/smart-tag-in-dam-update-asset-workflow.png)

1. Öppna egenskaperna för steget för att ändra informationen. Under **[!UICONTROL Advanced Settings]** kontrollerar du att alternativet **[!UICONTROL Handler Advance]** är markerat.

   ![Konfigurera arbetsflödet för DAM-uppdatering och lägg till smart tagg](assets/smart-tag-step-properties-workflow1.png)

1. Välj **[!UICONTROL Ignore Errors]** på fliken **[!UICONTROL Arguments]** om du vill att arbetsflödet ska slutföras även om steget med automatisk taggning misslyckas.

   Om du dessutom vill tagga resurser när de överförs, oavsett om smart taggning är aktiverat för mappar, väljer du **[!UICONTROL Ignore Smart Tag Flag]**.

   ![Konfigurera arbetsflödet för DAM-uppdatering av resurs för att lägga till smart tagg och markera hanterarframsteg](assets/smart-tag-step-properties-workflow2.png)

1. Klicka på den ![färdiga ikonen](assets/do-not-localize/check-ok-done-icon.png) för att stänga processsteget.

1. Klicka på **[!UICONTROL Sync]** för att spara arbetsflödet.

## Utbilda tjänsten Smart Content {#training-the-smart-content-service}

För att Smart Content Service ska känna igen din företagsklonomi kan du köra den på en uppsättning resurser som redan innehåller taggar som är relevanta för ditt företag. För att effektivt märka upp varumärkesbilderna kräver Smart Content Service att utbildningsbilderna följer vissa riktlinjer. Efter utbildning kan tjänsten tillämpa samma taxonomi på en liknande uppsättning resurser.

Du kan utbilda tjänsten flera gånger för att förbättra dess förmåga att använda relevanta taggar. Efter varje utbildningscykel kör du ett taggningsarbetsflöde och kontrollerar om dina resurser är taggade på rätt sätt.

Du kan utbilda Smart Content Service regelbundet eller efter behov.

>[!NOTE]
>
>Utbildningsarbetsflödet kan endast användas på mappar.

### Riktlinjer för utbildning {#guidelines-for-training}

För bästa resultat uppfyller bilderna i din utbildningsuppsättning följande riktlinjer:

**Kvantitet och storlek:** Minst 30 bilder per tagg. Minst 500 pixlar på den längre sidan.

**Sammanhang**: Bilder som används för en viss tagg liknar varandra visuellt.

Det är till exempel ingen bra idé att tagga alla dessa bilder som `my-party` (för träning) eftersom de inte är visuellt lika.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/coherence.png)

**Täckning**: Använd tillräcklig variation i bilderna i kursen. Tanken är att ge några exempel som är ganska olika, men som ändå är ganska olika, så att Experience Manager lär sig att fokusera på rätt saker. Om du använder samma tagg på bilder som ser olika ut bör du ta med minst fem exempel av varje typ.

För taggen *model-down-pose* kan du t.ex. inkludera fler utbildningsbilder som liknar den markerade bilden nedan så att tjänsten kan identifiera liknande bilder mer exakt under taggningen.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/coverage_1.png)

**Distraktion/obstruktion**: Tjänsten tränar bättre på bilder som har mindre störning (framträdande bakgrunder, icke-relaterade komponenter, t.ex. objekt/personer med huvudmotivet).

För taggen *casual-shoe* är till exempel den andra bilden inte en bra träningskandidat.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/distraction.png)

**Fullständighet:** Om en bild kvalificerar sig för mer än en tagg lägger du till alla tillämpliga taggar innan du inkluderar bilden för träning. För taggar, till exempel `raincoat` och `model-side-view`, lägger du till båda taggarna i den kvalificerade resursen innan du inkluderar den för utbildning.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/completeness.png)

>[!NOTE]
>
>Möjligheten för Smart Content Service att lära sig taggar och använda dem på andra bilder beror på kvaliteten på de bilder du använder i utbildningen. För bästa resultat rekommenderar Adobe att du använder visuellt liknande bilder för att utbilda tjänsten för varje tagg.

### Periodisk utbildning {#periodic-training}

Du kan aktivera tjänsten Smart Content Service för att med jämna mellanrum utbilda resurser och tillhörande taggar i en mapp. Öppna sidan [!UICONTROL Properties] i resursmappen, välj **[!UICONTROL Enable Smart Tags]** på fliken **[!UICONTROL Details]** och spara ändringarna.

![enable_smart_tags](assets/enable_smart_tags.png)

När det här alternativet har valts för en mapp kör [!DNL Experience Manager] ett utbildningsarbetsflöde automatiskt för att utbilda Smart Content Service i mappresurserna och deras taggar. Som standard körs utbildningsarbetsflödet varje vecka kl. 12.30 på lördagar.

### On-demand-utbildning {#on-demand-training}

Du kan utbilda tjänsten för smart innehåll när det behövs från arbetsflödeskonsolen.

1. I gränssnittet [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. Välj arbetsflödet **[!UICONTROL Smart Tags Training]** på sidan **[!UICONTROL Workflow Models]** och klicka sedan på **[!UICONTROL Start Workflow]** i verktygsfältet.
1. I dialogrutan **[!UICONTROL Run Workflow]** bläddrar du till nyttolastmappen som innehåller de taggade resurserna för att utbilda tjänsten.
1. Ange en rubrik för arbetsflödet och lägg till en kommentar. Klicka sedan på **[!UICONTROL Run]**. Resurserna och taggarna skickas in för utbildning.

   ![workflow_dialog](assets/workflow_dialog.png)

>[!NOTE]
>
>När materialet i en mapp har behandlats för utbildning behandlas endast de ändrade resurserna i efterföljande utbildningscykler.

### Visa utbildningsrapporter {#viewing-training-reports}

Om du vill kontrollera om Smart Content Service är utbildad i dina taggar i övningsresurserna kan du läsa rapporten om utbildningsarbetsflödet i rapportkonsolen.

1. I gränssnittet [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Reports]**.
1. Klicka på **[!UICONTROL Create]** på sidan **[!UICONTROL Asset Reports]**.
1. Välj rapporten **[!UICONTROL Smart Tags Training]** och klicka sedan på **[!UICONTROL Next]** i verktygsfältet.
1. Ange en titel och beskrivning för rapporten. Under **[!UICONTROL Schedule Report]** låter du alternativet **[!UICONTROL Now]** vara markerat. Om du vill schemalägga rapporten till ett senare tillfälle väljer du **[!UICONTROL Later]** och anger ett datum och en tid. Klicka sedan på **[!UICONTROL Create]** i verktygsfältet.
1. På sidan **[!UICONTROL Asset Reports]** markerar du rapporten som du skapat. Om du vill visa rapporten klickar du på **[!UICONTROL View]** i verktygsfältet.
1. Granska informationen i rapporten.

   Rapporten visar träningsstatusen för de taggar du har tränat. Den gröna färgen i kolumnen **[!UICONTROL Training Status]** anger att smarta innehållstjänster har tränats för taggen. Gul färg anger att tjänsten inte är helt tränad för en viss tagg. I det här fallet lägger du till fler bilder med just den taggen och kör träningsarbetsflödet för att träna tjänsten helt för taggen.

   Om du inte ser dina taggar i den här rapporten kör du utbildningsarbetsflödet igen för dessa taggar.

1. Om du vill hämta rapporten markerar du den i listan och klickar på **[!UICONTROL Download]** i verktygsfältet. Rapporten hämtas som ett Microsoft Excel-kalkylblad.

## Begränsningar {#limitations}

* Förbättrade smarta taggar bygger på inlärningsmodeller för bilder och taggar. Dessa modeller är inte alltid perfekta när det gäller att identifiera taggar. Den aktuella versionen av Smart Content Service har följande begränsningar:

   * Oförmåga att identifiera små skillnader i bilder. Till exempel tunna eller jämna skjortor.
   * Oförmåga att identifiera taggar baserat på små mönster/delar av en bild. Till exempel logotyper på T-shirts.
   * Taggning stöds i de språkområden som [!DNL Experience Manager] stöds i.

* Om du vill söka efter resurser med smarta taggar (vanliga eller förbättrade) använder du [!DNL Assets] Omnissearch (fulltextsökning). Det finns inget separat sökpredikat för smarta taggar.

>[!MORELIKETHIS]
>
>* [Översikt och utbildning av smarta taggar](enhanced-smart-tags.md)
>* [Felsökning av smarta taggar för OAuth-autentiseringsuppgifter](config-oauth.md)
>* [Videosjälvstudiekurs om smarta taggar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
