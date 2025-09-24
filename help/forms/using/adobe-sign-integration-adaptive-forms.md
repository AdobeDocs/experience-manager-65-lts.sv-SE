---
title: Integrera Adobe Sign med AEM Forms
description: Lär dig konfigurera Adobe Sign för din AEM Adaptive Forms. Adobe Sign förbättrar arbetsflödet och bearbetar dokumenten inom juridik, försäljning, löneadministration, personaladministration och många andra områden.
feature: Adaptive Forms,Foundation Components,Acrobat Sign
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: fdf95738-3075-43d6-9d51-64c83cf0f0b7
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '1896'
ht-degree: 0%

---

# Integrera [!DNL Adobe Sign] med AEM [!DNL Forms]{#integrate-adobe-sign-with-aem-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/services/adobe-sign-integration-adaptive-forms.html?lang=en#adobe-acrobat-sign-for-government).

[!DNL Adobe Sign] aktiverar e-signaturarbetsflöden för anpassningsbara formulär. E-signaturer förbättrar arbetsflödena för att bearbeta dokument inom juridik, försäljning, löneadministration, personaladministration och många andra områden.

I ett typiskt [!DNL Adobe Acrobat Sign]- och Adaptivt Forms-scenario fyller en användare i ett adaptivt formulär som kan användas för en tjänst. Till exempel kan en kreditkortsansökan och en medborgare få förmåner. När en användare fyller i, skickar och signerar ansökningsformuläret skickas formuläret till tjänsteleverantören för ytterligare åtgärder. Tjänsteleverantören granskar programmet och använder [!DNL Adobe Acrobat Sign] för att markera programmet som godkänt. AEM Forms stöder både Adobe Acrobat Sign och Adobe Acrobat Sign Solutions for Government. Beroende på licens och krav kan du integrera eller ansluta AEM Forms med någon av lösningarna:

* [Anslut AEM Forms till Adobe Acrobat Sign](#adobe-sign)
* [Koppla AEM Forms till Adobe Acrobat Sign Solutions för myndigheter](#adobe-acrobat-sign-for-government)

## Anslut AEM Forms till Adobe Acrobat Sign {#adobe-sign}

Om du vill ansluta **[!DNL AEM Forms]** till **[!DNL Adobe Acrobat Sign]** ställer du in programvaran och kontona som listas i avsnittet Krav och ansluter Adobe Sign till alla AEM Forms Author- och Publish-instanser:

## Förutsättningar {#prerequisites}

Du behöver följande för att integrera [!DNL Adobe Sign] med AEM [!DNL Forms]:

* Ett aktivt [Adobe Sign-utvecklarkonto.](https://www.adobe.com/acrobat/business/developer-form.html)
* En [SSL-aktiverad](/help/sites-administering/ssl-by-default.md) AEM [!DNL Forms]-server.
* Ett [Adobe Sign API-program](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/gstarted/create_app.md).
* Autentiseringsuppgifter (klient-ID och klienthemlighet) för API-programmet [!DNL Adobe Sign].
* När du konfigurerar om tar du bort den befintliga [!DNL Adobe Sign]-konfigurationen från både författare- och publiceringsinstanser.
* Använd [identisk krypteringsnyckel](/help/sites-administering/security-checklist.md#make-sure-you-properly-replicate-encryption-keys-when-needed) för författare och publiceringsinstanser.

## Konfigurera [!DNL Adobe Sign] med AEM [!DNL Forms] {#configure-adobe-sign-with-aem-forms}

När förutsättningarna är uppfyllda utför du följande steg för att konfigurera [!DNL Adobe Sign] med AEM [!DNL Forms] på författarinstansen:

1. I AEM [!DNL Forms]-författarinstansen går du till **Verktyg** ![hammer](assets/hammer.png) > **[!UICONTROL General]** > **[!UICONTROL Configuration Browser]**.
1. Välj **[!UICONTROL Configuration Browser]** på sidan **[!UICONTROL Create]**.
   * Mer information finns i dokumentationen för [Configuration Browser](/help/sites-administering/configurations.md).
1. I dialogrutan **[!UICONTROL Create Configuration]** anger du **[!UICONTROL Title]** för konfigurationen, aktiverar **[!UICONTROL Cloud Configurations]** och väljer **[!UICONTROL Create]**. En konfigurationsbehållare skapas.
1. Navigera till **Verktyg** ![hammer](assets/hammer.png) > **[!UICONTROL Cloud Services]** > **[!UICONTROL Adobe Sign]** och markera den konfigurationsbehållare som du skapade i steget ovan.

   >[!NOTE]
   >
   >Du kan antingen köra steg 1-4 för att skapa en konfigurationsbehållare och skapa en [!DNL Adobe Sign]-konfiguration i behållaren eller använda den befintliga `global`-mappen i **Verktyg** ![hammer](assets/hammer.png) > **[!UICONTROL Cloud Services]** > **[!UICONTROL Adobe Sign]**. Om du skapar konfigurationen i den nya konfigurationsbehållaren måste du ange behållarnamnet i fältet **[!UICONTROL Configuration Container]** när du skapar ett anpassat formulär.

   >[!NOTE]
   >
   >Kontrollera att URL:en för konfigurationssidan för molntjänster börjar med **HTTPS**. Om inte [aktiverar du SSL](/help/sites-administering/ssl-by-default.md) för AEM [!DNL Forms]-servern.


1. Tryck på **[!UICONTROL Create]** på konfigurationssidan för att skapa [!DNL Adobe Sign]-konfigurationen i AEM [!DNL Forms].
1. På fliken **[!UICONTROL General]** på sidan **[!UICONTROL Create Adobe Sign Configuration]** anger du **[!UICONTROL Name]** för konfigurationen och trycker på **[!UICONTROL Next]**. Du kan också ange en titel och bläddra för att välja en miniatyrbild för konfigurationen.
1. Nu kan du **[!UICONTROL Select solution]** välja [!DNL Adobe Acrobat Sign].

   ![Adobe Acrobat Sign Solutions](/help/forms/using/assets/adobe-sign-solution.png)

1. Kopiera URL-adressen i det aktuella webbläsarfönstret till en anteckningsruta och ta bort delen /`ui#/aem` från URL-adressen. Den ändrade URL:en krävs sedan för att konfigurera [!DNL Adobe Acrobat Sign]-programmet med [!DNL AEM Forms], i ett senare steg. Tryck på [!UICONTROL Next].

1. På fliken **[!UICONTROL Settings]**
   * fältet **[!UICONTROL OAuth URL]** innehåller standard-URL:en som innehåller Adobe Sign-databasdelning. URL-formatet är:

     `https://<shard>/public/oauth/v2`

     Till exempel:
     `https://secure.na1.echosign.com/public/oauth/v2`

   * fältet **[!UICONTROL Access token URL]** innehåller standard-URL:en som innehåller Adobe Sign-databasdelning. URL-formatet är:

     `https://<shard>/oauth/v2/token`

     Till exempel:
     `https://api.na1.echosign.com/oauth/v2/token`

   där:

   **na1** refererar till standarddatabasdelningen. Du kan ändra värdet för databasdelningen. Kontrollera att [!DNL &#x200B; Adobe Acrobat Sign]-molnkonfigurationerna pekar på [rätt kort](https://helpx.adobe.com/sign/using/identify-account-shard.html).

   >[!NOTE]
   >
   >* Håll sidan **Skapa Adobe Acrobat Sign-konfiguration** öppen. Stäng den inte. Du kan hämta **klient-ID** och **klienthemlighet** efter att OAuth-inställningarna för [!DNL Adobe Acrobat Sign]-programmet har konfigurerats, vilket beskrivs i kommande steg.
   > * När du har loggat in på ditt Adobe Sign-konto går du till **[!UICONTROL Acrobat Sign API]** > **[!UICONTROL API Information]** > **[!UICONTROL REST API Methods Documentation]** > **[!UICONTROL OAuth Access Token]** för att komma åt information som rör Adobe Sign OAuth-URL och Access Token URL.

1. Konfigurera OAuth-inställningar för programmet [!DNL Adobe Sign]:

   1. Öppna ett webbläsarfönster och logga in på utvecklarkontot för [!DNL Adobe Sign].
   1. Markera programmet som är konfigurerat för AEM [!DNL Forms] och välj **[!UICONTROL Configure OAuth for Application]**.
   1. Kopiera **[!UICONTROL Client ID]** och **[!UICONTROL Client Secret]** till en anteckningsruta.
   1. Lägg till HTTPS-URL:en som kopierades i föregående steg i rutan **[!UICONTROL Redirect URL]**.
   1. Aktivera följande OAuth-inställningar för programmet [!DNL Adobe Sign] och klicka på **[!UICONTROL Save]**.

   * agreement_read
   * agreement_write
   * agreement_send
   * widget_write
   * workflow_read

   Stegvis information om hur du konfigurerar OAuth-inställningar för ett [!DNL Adobe Sign]-program och hämtar nycklarna finns i [Konfigurera autentiseringsinställningar för programmets ](https://developer.adobe.com/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/configure_oauth.md) utvecklardokumentation.

   ![OAuth-konfiguration](assets/oauthconfig_new.png)

<!--
1. Go back to the **[!UICONTROL Create Adobe Sign Configuration]** page. In the **[!UICONTROL Settings]** tab, the **[!UICONTROL OAuth URL]** field mentions the  default URL. The format of the URL is:

   `https://<shard>/public/oAuth/v2`

   For example: 
   `https://secure.na1.echosign.com/public/oauth/v2`

   where:

   **na1** refers to the default database shard.

   You can modify the value for the database shard. Restart the server to be able to use the new value for the database shard.

   >[!NOTE]
   >
   >Ensure that your author and publish instance configurations point to the same shard. If you create multiple Adobe Sign configurations for an organization, ensure all the configurations utilize the same shard. -->

1. Gå tillbaka till sidan **[!UICONTROL Create Adobe Sign Configuration]**. På fliken **[!UICONTROL Settings]** anger du **Klient-ID** (kallas även program-ID) och **Klienthemlighet**. Använd [klient-ID och klienthemlighet för Adobe Sign-program](https://opensource.adobe.com/acrobat-sign/developer_guide/helloworld.html#get-the-app-id-and-secret) som skapats för AEM Forms.

1. Välj alternativet **[!UICONTROL Enable Adobe Sign for attachments also]** om du vill bifoga filer som är kopplade till ett adaptivt formulär till motsvarande [!DNL Adobe Sign]-dokument som skickats för signering.

1. Välj **[!UICONTROL Connect to Adobe Sign]**. När du uppmanas att ange autentiseringsuppgifter anger du användarnamn och lösenord för kontot som används när programmet [!DNL Adobe Sign] skapas.

   ![Konfigurationen av Adobe Acrobat Sign Cloud lyckades](assets/adobe-sign-cloud-configuration-success.png)

1. Tryck på **[!UICONTROL Create]** för att skapa [!DNL Adobe Sign]-konfigurationen.
1. Öppna AEM Web Console. URL:en är `https://'[server]:[port]'/system/console/configMgr`
1. Öppna **[!UICONTROL Forms Common Configuration Service].**
1. I fältet **[!UICONTROL Allow]** **select** Alla användare - Alla användare, anonyma eller inloggade, kan förhandsgranska bilagor, verifiera och signera formulär och klicka på **[!UICONTROL Save].** Författarinstansen är konfigurerad att använda [!DNL Adobe Sign].
1. Publicera konfigurationen.
1. Använd [replikering](/help/sites-deploying/replication.md) om du vill skapa en identisk konfiguration för motsvarande publiceringsinstanser.

[!DNL Adobe Sign] är nu integrerat med AEM [!DNL Forms] och klart att användas i anpassningsbara formulär. Om du vill [använda Adobe Sign-tjänsten i ett adaptivt formulär](../../forms/using/working-with-adobe-sign.md#configure-adobe-sign-for-an-adaptive-form) anger du den konfigurationsbehållare som skapas ovan i adaptiva formuläregenskaper.

>[!NOTE]
>
> Om du vill konfigurera Adobe Sign-sandlådan kan du följa samma konfigurationssteg som beskrivs i [Adobe Sign](#adobe-sign).

## Koppla AEM Forms till Adobe Acrobat Sign Solutions för myndigheter {#adobe-acrobat-sign-for-government}

Att knyta AEM Forms till Adobe Acrobat Sign Solutions för myndigheter är en flerstegsprocess. Den innehåller följande uppgifter:

* Skapa omdirigerings-URL för dina AEM-instanser
* Dela omdirigerings-URL:er och omfattningar med Adobe Sign-lösningar för myndighetsteamet
* Få inloggningsuppgifter från Adobe Sign-teamet
* Använda inloggningsuppgifterna för att ansluta AEM Forms till Adobe Acrobat Sign Solutions för myndigheter

![adobe-acrobat-sign-govt-workflow](/help/forms/using/assets/adobe-acrobat-sign-govt-workflow.png)

### Innan du börjar {#prerequisites-for-adobe-sign-for-acrobat-sign-for-government}

Innan du börjar ansluta AEM Forms med Adobe Acrobat Sign Solution,

* Kontrollera att ditt [Adobe Acrobat Sign Solutions for Government](https://opensource.adobe.com/acrobat-sign/signgov/gstarted.html#account-provisioning)-konto har etablerats.
* Dina AEM [!DNL Forms]-servrar är [SSL-aktiverade](/help/sites-administering/ssl-by-default.md) .
* Dina AEM [!DNL Forms]-servrar använder [identisk krypteringsnyckel](/help/sites-administering/security-checklist.md#make-sure-you-properly-replicate-encryption-keys-when-needed) för författare och publiceringsinstanser.

### Koppla AEM Forms till Adobe Acrobat Sign Solutions för myndigheter {#connect-adobe-acrobat-sign-for-government}

#### Skapa en omdirigerings-URL för din AEM-instans

1. På din AEM Forms-instans går du till **[!UICONTROL Tools]** ![hammer](assets/hammer.png) > **[!UICONTROL General]** > **[!UICONTROL Configuration Browser]**.
1. Välj **[!UICONTROL Configuration Browser]** på sidan **[!UICONTROL Create]**.
1. I dialogrutan **[!UICONTROL Create Configuration]** anger du **[!UICONTROL Title]** för konfigurationen, aktiverar **[!UICONTROL Cloud Configurations]** och väljer **[!UICONTROL Create]**. En konfigurationsbehållare skapas. Kontrollera att behållar-/mappnamnet inte innehåller något utrymme.

1. Navigera till **[!UICONTROL Tools]** ![hammer](assets/hammer.png) > **[!UICONTROL Cloud Services]** > **[!UICONTROL Adobe Acrobat Sign]** och öppna konfigurationsbehållaren som du skapade i föregående steg. När du skapar ett adaptivt formulär anger du behållarnamnet i fältet **[!UICONTROL Configuration Container]**.
1. På konfigurationssidan väljer du **[!UICONTROL Create]** för att skapa [!DNL Adobe Acrobat Sign]-konfigurationen i AEM Forms.
1. Kopiera URL-adressen för det aktuella webbläsarfönstret till en anteckningsruta från URL-adressen. Denna URL kallas `re-direct URL`. I nästa avsnitt delar du `re-direct URL` och `Scopes` med Adobe Sign-teamet och begär inloggningsuppgifter (klient-ID och klienthemlighet).

>[!NOTE]
>
>
> * En `re-direct URL` ska innehålla en [toppnivå](https://en.wikipedia.org/wiki/Top-level_domain)-domän. Exempel: `https://adobe.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/global`
> * Använd inte en lokal URL som `re-direct URL`. Exempel: `https://localhost:4502/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/global`.


#### Dela omdirigerings-URL:en och omfattningarna med Adobe Sign-teamet och få inloggningsuppgifter

Adobe Acrobat Sign for Government Solutions-teamet kräver att `re-direct URL` och vissa omfattningar är aktiverade för ditt Adobe Acrobat Sign-program (se nedan) för att generera autentiseringsuppgifter (klient-ID och klienthemlighet) som gör att du kan ansluta AEM Forms till Adobe Acrobat Sign Solutions för myndigheter.

Dela `scopes` (visas nedan) och `re-direct URL` skapade och noterade det sista steget i föregående avsnitt med din Adobe Acrobat Sign for Government Solution-representant [Adobe Professional Services-teammedlem](https://opensource.adobe.com/acrobat-sign/signgov/gstarted.html#password).

**_Omfång_**

* [!DNL agreement_read]
* [!DNL agreement_write]
* [!DNL agreement_send]
* [!DNL widget_read]
* [!DNL widget_write]
* [!DNL workflow_read]
* [!DNL offline_access]

Representanten genererar och delar uppgifter med dig. I nästa avsnitt använder du inloggningsuppgifterna (Klient-ID och Klienthemlighet) för att ansluta AEM Forms till Adobe Acrobat Sign Solutions för myndigheter.

#### Använd de inloggningsuppgifter som tas emot för att ansluta AEM Forms till Adobe Acrobat Sign Solutions för offentlig sektor

1. Öppna `re-direct URL` i webbläsaren. Du skapade och noterade `re-direct URL` i det sista steget i avsnittet [Skapa en omdirigerings-URL i din AEM-instans](#create-redirect-url).

1. På fliken **[!UICONTROL General]** på sidan **[!UICONTROL Create Adobe Sign Configuration]** anger du **[!UICONTROL Name]** för konfigurationen och väljer **[!UICONTROL Next]**. Du kan också ange en **[!UICONTROL Title]** och bläddra för att välja en **[!UICONTROL Thumbnail]** för konfigurationen. Klicka på **[!UICONTROL Next]**.

1. Välj **[!UICONTROL Settings]** för alternativet **[!UICONTROL Create Adobe Sign Configuration]** på fliken **[!UICONTROL Select solution]** på sidan [!DNL Adobe Acrobat Sign Solutions for Government].

   ![Adobe Acrobat Sign Solutions för offentlig sektor](/help/forms/using/assets/adobe-sign-for-govt.png)

1. I fältet **[!UICONTROL Email]** anger du den e-postadress som är associerad med ditt Adobe Acrobat Sign Solutions for Government-konto.

1. På fliken **[!UICONTROL Settings]**
   * fältet **[!UICONTROL OAuth URL]** innehåller standard-URL:en som innehåller Adobe Sign-databasdelning. URL-formatet är:

     `https://<shard>/api/gateway/adobesignauthservice/api/v1/authorize`

     Till exempel:
     `https://secure.na1.adobesign.us/api/gateway/adobesignauthservice/api/v1/authorize`

   * fältet **[!UICONTROL Access token URL]** innehåller standard-URL:en som innehåller Adobe Sign-databasdelning. URL-formatet är:

     `https://<shard>/api/gateway/adobesignauthservice/api/v1/token`

     Till exempel:
     `https://secure.na1.adobesign.us/api/gateway/adobesignauthservice/api/v1/token`

   där:

   **na1** refererar till standarddatabasdelningen. Du kan ändra värdet för databasdelningen. Kontrollera att [!DNL &#x200B; Adobe Acrobat Sign]-molnkonfigurationerna pekar på [rätt kort](https://helpx.adobe.com/sign/using/identify-account-shard.html).

   >[!NOTE]
   >
   > * När du har loggat in på ditt Adobe Sign-konto går du till **[!UICONTROL Acrobat Sign API]** > **[!UICONTROL API Information]** > **[!UICONTROL REST API Methods Documentation]** > **[!UICONTROL OAuth Access Token]** för att få tillgång till information om Adobe Sign-autentiserings-URL och åtkomsttoken-URL.

1. Använd de inloggningsuppgifter som delas av Adobe Acrobat Sign for Government Solution manager ([Adobe Professional Services team member]) i föregående avsnitt som [**[!UICONTROL Client ID]** och **[!UICONTROL Client Secret]**].

1. Välj alternativet **[!UICONTROL Enable Adobe Acrobat Sign for attachments]** om du vill lägga till filer som är kopplade till ett adaptivt formulär i motsvarande [!DNL Adobe Acrobat Sign] -dokument som skickats för signering.

1. Välj **[!UICONTROL Connect to Adobe Sign]**. När du uppmanas att ange autentiseringsuppgifter anger du användarnamn och lösenord för kontot som används när programmet [!DNL Adobe Acrobat Sign] skapas. När du tillfrågas om att bekräfta åtkomst för `Adobe Acrobat Sign for Government Solutions` och klickar du på **[!UICONTROL Allow Access]**. Om inloggningsuppgifterna är korrekta och du tillåter [!DNL AEM Forms] att få åtkomst till ditt [!DNL Adobe Acrobat Sign]-utvecklarkonto, visas ett meddelande om att åtgärden lyckades som det nedan.

   ![Konfigurationen av Adobe Acrobat Sign Cloud lyckades](/help/forms/using/assets/adobe-sign-cloud-configuration-success.png)

   När du uppmanas att ange autentiseringsuppgifter anger du användarnamn och lösenord för kontot som används när programmet [!DNL Adobe Acrobat Sign] skapas. När du ombeds att bekräfta åtkomst för `your account` och klickar på **[!UICONTROL Allow Access]**.

1. Välj **[!UICONTROL Create]** för att skapa konfigurationen.
1. Öppna AEM Web Console. URL:en är `https://'[server]:[port]'/system/console/configMgr`
1. Öppna **[!UICONTROL Forms Common Configuration Service].**
1. I fältet **[!UICONTROL Allow]** **select** Alla användare - Alla användare, anonyma eller inloggade, kan förhandsgranska bilagor, verifiera och signera formulär och klicka på **[!UICONTROL Save].** Författarinstansen är konfigurerad att använda [!DNL Adobe Sign].

1. Publicera konfigurationen.
1. Använd [replikering](/help/sites-deploying/replication.md) om du vill skapa en identisk konfiguration för motsvarande publiceringsinstanser.

Nu kan du [använda Adobe Acrobat Sign-fält i ett adaptivt formulär](working-with-adobe-sign.md) eller [AEM Workflow](/help/forms/using/aem-forms-workflow-step-reference.md#sign-document-step-sign-document-step). Se till att du lägger till konfigurationsbehållaren som används för Cloud Service-konfigurationen i alla adaptiva Forms som aktiveras för [!DNL Adobe Acrobat Sign]. Du kan ange en konfigurationsbehållare bland egenskaperna för ett adaptivt formulär.


## Konfigurera schemaläggaren för [!DNL Adobe Sign] för att synkronisera signeringsstatusen {#configure-adobe-sign-scheduler-to-sync-the-signing-status}

Ett anpassat formulär som har aktiverats av [!DNL Adobe Sign] skickas endast när alla signerare har slutfört signeringsprocessen. Som standard schemaläggs [!DNL Adobe Sign]-schemaläggningstjänsten att kontrollera (avfråga) signerarens svar efter 24 timmars intervall. Du kan ändra standardintervallet för din miljö. Utför följande steg för att ändra standardintervallet:

1. Logga in på AEM [!DNL Forms]-servern med administratörsuppgifter och gå till **Verktyg** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.

   Du kan även öppna följande URL-adress i ett webbläsarfönster:
   `https://[localhost]:'port'/system/console/configMgr`

1. Leta reda på och öppna alternativet **[!UICONTROL Adobe Sign Configuration Service]**. Ange ett [cron-uttryck](https://en.wikipedia.org/wiki/Cron#CRON_expression) i fältet **[!UICONTROL Status Update Scheduler Expression]** och klicka på **[!UICONTROL Save]**. Om du till exempel vill köra konfigurationstjänsten varje dag klockan 00:00 anger du :00 i fältet `0 0 0 1/1 * ? *`.**[!UICONTROL Status Update Scheduler Expression]**

Standardintervallet för synkroniseringsstatus för [!DNL Adobe Sign] har ändrats.

## Relaterade artiklar {#related-articles}

* [Använda Adobe Sign i ett anpassat formulär](../../forms/using/working-with-adobe-sign.md)
* [Adobe Sign med formulärcentrerade arbetsflöden](/help/forms/using/aem-forms-workflow-step-reference.md#sign-document-step-sign-document-step)
* [Använda Adobe Sign med AEM Forms (video)](https://helpx.adobe.com/experience-manager/kt/forms/using/adobe-sign-integration-feature-video.html)
