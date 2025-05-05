---
title: Konfigurera resurstaggning med Smart Content Service
description: Lär dig hur du konfigurerar smart taggning och förbättrad smart taggning i  [!DNL Adobe Experience Manager] med hjälp av Smart Content Service.
role: Admin
feature: Tagging,Smart Tags
solution: Experience Manager, Experience Manager Assets
exl-id: 26371d15-b0e1-4892-9c52-bc9829e462ca
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '884'
ht-degree: 15%

---

# Felsöka smarta taggar för OAuth-autentiseringsuppgifter {#oauth-config}

En öppen auktoriseringskonfiguration krävs för att godkänna att programmet [!DNL Adobe Experience Manager] interagerar med smarta innehållstjänster på ett säkert sätt.

>[!NOTE]
>
> Du kan inte skapa nya JWT-autentiseringsuppgifter från och med juni 2024. Härifrån skapas endast OAuth Server-till-Server-autentiseringsuppgifter.
> Integreringen av JWT fortsätter att fungera fram till januari 2025 endast för befintliga AMS-användare och lokala användare.

## OAuth-konfiguration för de nya AMS-användarna {#oauth-config-existing-ams-users}

Mer information om konfigurationen av OAuth-tjänster för en ny användare finns i [konfigurationen av smarta innehållstjänster](#integrate-adobe-io). Följ dessa [steg](#prereqs-config-oauth-onprem) när du är klar.

>[!NOTE]
>
>Om det behövs kan du skicka in en supportanmälan efter [supportprocessen](https://experienceleague.adobe.com/sv?lang=en&amp;support-tab=home#support).

## OAuth-konfiguration för befintliga AMS-användare {#oauth-config-new-ams-users}

Innan du utför något av stegen i den här metoden måste du implementera följande:

### Förutsättningar {#prereqs-config-oauth-onprem}

En OAuth-konfiguration kräver följande krav:

* Skapa en ny OAuth-integrering i [Developer Console](https://developer.adobe.com/console/user/servicesandapis). Använd egenskaperna `ClientID`, `ClientSecret`, `OrgID` och andra egenskaper i stegen nedan:
* Följande filer finns på sökvägen `/apps/system/config in crx/de`:
   * `com.**adobe**.granite.auth.oauth.accesstoken.provider.<randomnumbers>.config`
   * `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl.<randomnumber>.config`

### OAuth-konfiguration för befintliga AMS- och On-prem-användare {#steps-config-oauth-onprem}

Stegen nedan kan utföras av systemadministratören. AMS-kunder kan kontakta Adobe eller lämna in en supportanmälan efter [supportprocessen](https://experienceleague.adobe.com/sv?lang=en&amp;support-tab=home#support).

1. Lägg till eller uppdatera nedanstående egenskaper i `com.adobe.granite.auth.oauth.accesstoken.provider.<randomnumbers>.config`:

   * `auth.token.provider.authorization.grants="client_credentials"`
   * `auth.token.provider.orgId="<OrgID>"`
   * `auth.token.provider.default.claims=("\"iss\"\ :\ \"<OrgID>\"")`
   * `auth.token.provider.scope="read_pc.dma_smart_content,\ openid,\ AdobeID,\ additional_info.projectedProductContext"`

     `auth.token.validator.type="adobe-ims-similaritysearch"`
   * Uppdatera `auth.token.provider.client.id` med klient-ID:t för den nya OAuth-konfigurationen.
   * Uppdatera `auth.access.token.request` till `"https://ims-na1.adobelogin.com/ims/token/v3"`
1. Byt namn på filen till `com.adobe.granite.auth.oauth.accesstoken.provider-<randomnumber>.config`.
1. Utför stegen nedan i `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl.<randomnumber>.config`:
   * Uppdatera egenskapen auth.ims.client.secrets med klienthemligheten från den nya OAuth-integreringen.
   * Byt namn på filen till `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl-<randomnumber>.config`
1. Spara alla ändringar i utvecklingskonsolen för innehållsdatabasen, till exempel CRXDE.
<!--
1. Navigate to `/system/console/configMgr` and replace the OSGi configuration from `.<randomnumber>` to `-<randomnumber>`.
1. Delete the old OSGi configuration for `"Access Token provider name: adobe-ims-similaritysearch"` in `/system/console/configMgr`.
-->
1. I `System/console/configMgr` tar du bort de gamla konfigurationerna för `com.adobe.granite.auth.ims.impl.IMSAccessTokenRequestCustomizerImpl` och Access Token Provider-namnet `adobe-ims-similaritysearch`.
1. Starta om konsolen.

## Validera konfigurationen {#validate-the-configuration}

När du har slutfört konfigurationen kan du använda en JMX MBean för att validera konfigurationen. Följ de här stegen för att validera.

1. Gå till din [!DNL Experience Manager]-server på `https://[aem_server]:[port]`.

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]** för att öppna OSGi-konsolen. Klicka på **[!UICONTROL Main]>[!UICONTROL JMX]**.

1. Klicka på `com.day.cq.dam.similaritysearch.internal.impl`. Den öppnas **[!UICONTROL SimilaritySearch Miscellaneous Tasks]**.

1. Klicka på `validateConfigs()`. I dialogrutan **[!UICONTROL Validate Configurations]** klickar du på **[!UICONTROL Invoke]**.

Valideringsresultaten visas i samma dialogruta.

## Integrera med Adobe Developer Console {#integrate-adobe-io}

När du integrerar med Adobe Developer Console autentiserar servern [!DNL Experience Manager] dina tjänstinloggningsuppgifter med Adobe Developer Console gateway innan du vidarebefordrar din begäran till Smart Content Service. För att kunna integrera behöver ni ett Adobe ID-konto som har administratörsbehörighet för organisationen och en licens för Smart Content Service som köpts och aktiverats för organisationen.

Så här konfigurerar du tjänsten Smart Content:

1. Om du vill generera en offentlig nyckel [skapar du en konfiguration för Smart Content Service](#obtain-public-certificate) i [!DNL Experience Manager]. [Hämta ett offentligt certifikat](#obtain-public-certificate) för OAuth-integrering.

1. *[Gäller inte om du är en befintlig användare]* [skapar en integrering i Adobe Developer Console](#create-adobe-i-o-integration).

1. [Konfigurera distributionen](#configure-smart-content-service) med API-nyckeln och andra autentiseringsuppgifter från Adobe Developer Console.

1. [Testa konfigurationen](#validate-the-configuration).

## Hämta ett publikt certifikat genom att skapa konfigurationen för Smart Content Service {#download-public-certificate}

Med ett offentligt certifikat kan du autentisera din profil på Adobe Developer Console.

1. I användargränssnittet för [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Cloud Services]** > **[!UICONTROL Legacy Cloud Services]**.

1. Klicka **[!UICONTROL Configure Now]** under **[!UICONTROL Assets Smart Tags]** på sidan för molntjänster.

1. I dialogrutan **[!UICONTROL Create Configuration]** anger du en rubrik och ett namn för konfigurationen av smarta taggar. Klicka på **[!UICONTROL Create]**.

1. Använd följande värden i dialogrutan **[!UICONTROL AEM Smart Content Service]**:

   **[!UICONTROL Service URL]**: `https://smartcontent.adobe.io/<region where your Experience Manager author instance is hosted>`

   Exempel: `https://smartcontent.adobe.io/apac`. Du kan ange `na`, `emea` eller `apac` som de områden där din Experience Manager-författarinstans finns.

   >[!NOTE]
   >
   >Om Experience Manager hanterade tjänst etableras före 1 september 2022 använder du följande tjänst-URL:
   >`https://mc.adobe.io/marketingcloud/smartcontent`

   **[!UICONTROL Authorization Server]**: `https://ims-na1.adobelogin.com`

   Lämna de andra fälten tomma (kommer senare). Klicka på **[!UICONTROL OK]**.

   ![Dialogrutan Experience Manager Smart Content Service för att tillhandahålla innehållstjänstens URL](assets/aem_scs12.png)

   *Figur: Dialogrutan Smart innehållstjänst innehåller URL:er för innehållstjänsten*

   >[!NOTE]
   >
   >URL:en som angetts som [!UICONTROL Service URL] är inte tillgänglig via webbläsaren och genererar ett 404-fel. Konfigurationen fungerar korrekt med samma värde som parametern [!UICONTROL Service URL]. Den övergripande servicestatusen och underhållsschemat finns på [https://status.adobe.com](https://status.adobe.com).

1. Klicka på **[!UICONTROL Download Public Certificate for OAuth Integration]** och hämta den offentliga certifikatfilen `AEM-SmartTags.crt`. Dessutom behöver du inte längre överföra det här certifikatet till Adobe utvecklarkonsol.

   ![En representation av de inställningar som har skapats för den smarta taggningstjänsten](assets/smart-tags-download-public-cert1.png)

   *Bild: Inställningar för tjänsten för smart taggning.*

## Integrering med Adobe Developer Console {#create-adobe-i-o-integration}

Om du vill använda API:er för smarta innehållstjänster skapar du en integrering i Adobe Developer Console för att få [!UICONTROL API Key] (som genereras i fältet [!UICONTROL CLIENT ID] för Adobe Developer Console-integrering), [!UICONTROL TECHNICAL ACCOUNT ID], [!UICONTROL ORGANIZATION ID] och [!UICONTROL CLIENT SECRET] för [!UICONTROL Assets Smart Tagging Service Settings] av molnkonfigurationen i [!DNL Experience Manager].

1. Gå till [https://developer.adobe.com/console/](https://developer.adobe.com/console/) i en webbläsare. Välj lämpligt konto och kontrollera att den associerade organisationsrollen är systemadministratör.

1. Skapa ett projekt med valfritt namn. Klicka på **[!UICONTROL Add API]**.

1. På sidan **[!UICONTROL Add an API]** markerar du **[!UICONTROL Experience Cloud]** och väljer **[!UICONTROL Smart Content]**. Klicka på **[!UICONTROL Next]**.

1. Välj autentiseringsmetoden **[!UICONTROL OAuth Server-to-Server]**.

1. Lägg till/ändra **[!UICONTROL Credential Name]** efter behov. Klicka på **[!UICONTROL Next]**.

1. Välj produktprofilen **[!UICONTROL Smart Content Services]**. Klicka på **[!UICONTROL Save Configured API]**. OAuth API läggs till under de anslutna autentiseringsuppgifterna för ytterligare användning. Du kan kopiera [!UICONTROL API key (Client ID)] eller [!UICONTROL Generate access token] från den.
<!--
1. On the **[!UICONTROL Select product profiles]** page, select **[!UICONTROL Smart Content Services]**. Click **[!UICONTROL Save configured API]**.

   A page displays more information about the configuration. Keep this page open to copy and add these values in [!UICONTROL Assets Smart Tagging Service Settings] of cloud configuration in [!DNL Experience Manager] to configure smart tags.

   ![In the Overview tab, you can review the information provided for integration.](assets/integration_details.png)


   *Figure: Details of integration in Adobe Developer Console*
-->

![oauth config](assets/oauth-config.png)
*Bild: Konfigurerad OAuth Server-till-server i Adobe Developer Console*

## Konfigurera Smart Content Service {#configure-smart-content-service}

Om du vill konfigurera integreringen använder du värdena för fälten [!UICONTROL TECHNICAL ACCOUNT ID], [!UICONTROL ORGANIZATION ID], [!UICONTROL CLIENT SECRET] och [!UICONTROL CLIENT ID] från Adobe Developer Console-integreringen. Om du skapar en molnkonfiguration för smarta taggar kan API-begäranden från distributionen [!DNL Experience Manager] autentiseras.

1. I [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Cloud Service]** > **[!UICONTROL Legacy Cloud Services]** för att öppna [!UICONTROL Cloud Services]-konsolen.

1. Öppna konfigurationen som skapats ovan under **[!UICONTROL Assets Smart Tags]**. Klicka på **[!UICONTROL Edit]** på tjänstinställningssidan.

1. I dialogrutan **[!UICONTROL AEM Smart Content Service]** använder du de förifyllda värdena för fälten **[!UICONTROL Service URL]** och **[!UICONTROL Authorization Server]**.

1. Kopiera och använd följande värden som genererats i [Adobe Developer Console-integrering](#create-adobe-i-o-integration) för fälten [!UICONTROL Api Key], [!UICONTROL Technical Account ID], [!UICONTROL Organization ID] och [!UICONTROL Client Secret].

   | [!UICONTROL Assets Smart Tagging Service Settings] | Integrationsfält för [!DNL Adobe Developer Console] |
   |--- |--- |
   | [!UICONTROL Api Key] | [!UICONTROL CLIENT ID] |
   | [!UICONTROL Technical Account ID] | [!UICONTROL TECHNICAL ACCOUNT ID] |
   | [!UICONTROL Organization ID] | [!UICONTROL ORGANIZATION ID] |
   | [!UICONTROL Client Secret] | [!UICONTROL CLIENT SECRET] |

>[!MORELIKETHIS]
>
>* [Översikt och utbildning av smarta taggar](enhanced-smart-tags.md)
>* [Konfigurera smart taggning](config-smart-tagging.md)
>* [Videosjälvstudiekurs om smarta taggar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html?lang=sv-SE)
