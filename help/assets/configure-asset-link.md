---
title: Konfigurera Experience Manager Assets för Adobe Asset Link
description: Konfigurera Experience Manager Assets för användning med tillägget Adobe Asset Link för Creative Cloud-program.
contentOwner: Vishabh Gupta
role: Admin
feature: Asset Management
solution: Experience Manager, Experience Manager Assets
source-git-commit: 238ff31ed4e5e833a14cb64e3eae75377038a857
workflow-type: tm+mt
source-wordcount: '2852'
ht-degree: 0%

---

# Konfigurera Experience Manager Assets för Adobe Asset Link {#adobe-asset-link}

[Adobe Asset Link (AAL)](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html) effektiviserar samarbetet mellan kreatörer och marknadsförare när innehåll skapas. Den kopplar samman Adobe Experience Manager Assets med Creative Cloud datorprogram Adobe InDesign, Adobe Photoshop och Adobe Illustrator. På Adobe Asset Link-panelen kan kreativa användare komma åt och ändra innehåll som lagras i AEM Assets utan att lämna de kreativa program de är mest bekanta med.

Implementera följande uppgifter om du vill konfigurera Experience Manager Assets att användas med Asset Link. Använd Experience Manager administratörskonto för att göra konfigurationen:

1. Installera paketen efter behov. Information finns i [förutsättningarna](#prerequisites).

1. Konfigurera Experience Manager [manuellt](#manual-configuration) eller med ett [paket](#configure-using-package).

1. Om du vill mappa Creative Cloud-licensierade användare till Experience Manager-användare hanterar du [åtkomstkontrollen](#user-access).

1. Skapa [anpassat frågeindex](#create-custom-index), konfigurera [FPO-återgivningar](/help/assets/configure-fpo-renditions.md) för InDesign, konfigurera [Adobe Stock-integrering](/help/assets/aem-assets-adobe-stock.md) och konfigurera [visuell eller liknande sökning](https://experienceleague.adobe.com/docs/experience-manager-65-2025/assets/using/search-assets.html#configvisualsearch).

## Förutsättningar och stöd för olika funktioner {#prerequisites}

Kontrollera att du har installerat rätt Service Pack och paket efter behov. Se följande krav för respektive Experience Manager-version och för specifika funktioner.

| Assets | Experience Manager-version och supportkrav |
|--- |--- |
| Resurslänk fungerar som standard | Experience Manager 6.5 och 6.5.2 eller senare. </br> Experience Manager 6.4.4 och 6.4.6 eller senare. </br> Adobe rekommenderar att du installerar det senaste [Experience Manager Service Pack (SP)](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html) innan du använder AAL. |
| Resurslänk fungerar efter installation av ett paket | För Experience Manager 6.4.0 - 6.4.3 installerar du paketet [adobe-asset-link-support](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq640/featurepack/adobe-asset-link-support). |
| Integrering med Adobe Stock | Experience Manager 6.4.2 eller senare |
| Visuell eller liknande sökning | Experience Manager 6.5.0 eller senare |


## Konfigurera Experience Manager med konfigurationspaketet {#configure-using-package}

Adobe rekommenderar att du installerar konfigurationspaketet [adobe-asset-link-config](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq640/product/assets/adobe-asset-link-config) för att automatisera de flesta konfigurationsåtgärderna, följt av några manuella åtgärder. Du kan även [konfigurera manuellt](#manual-configuration).

>[!CAUTION]
>
>Använd inte konfigurationspaketet om din Experience Manager-instans har konfigurerats för användarinloggning med Adobe IMS-konton. [konfigurera i stället](#manual-configuration) din Experience Manager-instans manuellt.

1. Öppna Package Manager genom att gå till **[!UICONTROL Tools]** > **[!UICONTROL Deployment]** > **[!UICONTROL Package Share]** i Experience Manager webbgränssnitt. Installera paketet `adobe-asset-link-config`.

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**. Leta reda på konfigurationen för **[!UICONTROL Adobe Granite OAuth IMS Provider]** och klicka för att redigera den.

   Ange följande egenskaper och spara ändringarna.

   * [!UICONTROL Group Mappings]: Lämna tomt om du inte vill. Mer information finns i [Gruppmappning](#group-mapping).
   * [!UICONTROL Organization]: Ange det organisations-ID som du använder i Adobe Admin Console. Mer information om organisations-ID:n finns i [Skapa användargrupp](https://helpx.adobe.com/enterprise/using/create-aal-user-group.html).

1. Leta reda på konfigurationen för **[!UICONTROL Adobe Granite Bearer Authentication Handler]** och klicka för att redigera den.

   Lägg till **[!UICONTROL InDesignAem2]** klient-ID:n i konfigurationsegenskapen **[!UICONTROL Allowed OAuth client ids]**.


## Konfigurera Experience Manager manuellt {#manual-configuration}

Konfigurera Experience Manager manuellt om du väljer att inte använda ett konfigurationspaket eller om din Experience Manager-distribution är konfigurerad att stödja användarinloggning med Adobe IMS-konton.

Så här konfigurerar du Experience Manager manuellt:

1. Om du vill komma åt konfigurationshanteraren går du till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**. Välj **[!UICONTROL OSGi]** > **[!UICONTROL Configuration]** på menyn längst upp.

1. Leta reda på **[!UICONTROL Adobe Granite OAuth IMS Provider]**-konfigurationen och klicka för att redigera den.

   Ange följande konfiguration och klicka på **[!UICONTROL Save]**.

   * [!UICONTROL Authorization Endpoint]: ` https://ims-na1.adobelogin.com/ims/authorize/v1`
   * [!UICONTROL Token Endpoint]: ` https://ims-na1.adobelogin.com/ims/token/v1`
   * [!UICONTROL Profile Endpoint]: ` https://ims-na1.adobelogin.com/ims/profile/v1`
   * [!UICONTROL Validation URL]: ` https://ims-na1.adobelogin.com/ims/validate_token/v1`
   * [!UICONTROL Organization]: Ange som organisations-ID i [Adobe Admin Console](https://adminconsole.adobe.com/).
   * [!UICONTROL Group Mappings]: Lämna tomt om du inte har ett specialfall. Mer information finns i [Gruppmappning](#group-mapping).

1. Leta reda på konfigurationen för **[!UICONTROL Adobe Granite Bearer Authentication Handler]** och klicka för att redigera den.

   Lägg till följande klient-ID i konfigurationsegenskapen **[!UICONTROL Allowed OAuth client ids]**: `InDesignAem2, cc-europa-desktop_0_1, cc-europa-desktop_1_0, cc-europa-desktop_2_0, cc-europa-desktop_3_0, cc-europa-desktop_4_0, cc-europa-desktop_5_0, cc-europa-desktop_6_0, cc-europa-desktop_7_0, cc-europa-desktop_8_0, cc-europa-desktop_9_0, and cc-europa-desktop_10_0`.

   Klicka på `+` om du vill lägga till varje `Client ID`. Klicka på **[!UICONTROL Save]** när du har lagt till alla ID.

1. Granska de befintliga **[!UICONTROL Adobe Granite OAuth Application and Provider]**-instanserna i konfigurationen **[!UICONTROL Adobe Granite OAuth Application and Provider]**. Om du hittar en instans med värdet `Config ID` för `ims` kan du använda den för instruktionerna i den här proceduren. Annars klickar du på `+` för att skapa en konfigurationsinstans. Ange följande egenskapsvärden och klicka på **[!UICONTROL Save]**.

   * [!UICONTROL Client ID]: Ändra inte
   * [!UICONTROL Client Secret]: Ändra inte
   * [!UICONTROL Config ID]: ` ims`
   * [!UICONTROL Scope]: `AdobeID, OpenID, read_organizations` (andra värden kan också finnas i konfigurationen)
   * [!UICONTROL Provider ID]: ` ims`
   * [!UICONTROL Create users]: ` Checked`
   * [!UICONTROL User ID Property]: `Email` för den nya konfigurationen. I annat fall ska du inte ändra.

1. Leta reda på **[!UICONTROL Apache Jackrabbit Oak Default Sync Handler]**-konfigurationen med **[!UICONTROL Sync Handler Name]** `ims` och klicka för att redigera den.

   Ange följande konfigurationsegenskaper och klicka på **[!UICONTROL Save]**.

   * [!UICONTROL User Expiration Time and User Membership Expiration]: Tid i minuter följt av m utan utrymme. Till exempel `15m` i 15 minuter. Mer information finns i [Gruppmappning](#group-mapping).
   * [!UICONTROL User auto membership]: Ändra inte
   * [!UICONTROL User Dynamic Membership]: ` Deslect`

1. Leta reda på **[!UICONTROL Adobe Granite OAuth Authentication Handler]**-konfigurationen och klicka för att redigera den. Klicka på **[!UICONTROL Save]** utan att göra några ändringar.

1. Om du vill justera den relativa prioriteten för hanteraren för innehavarautentisering går du till `/apps/system/config` i CRXDE. Leta reda på `com.adobe.granite.auth.oauth.impl.BearerAuthenticationHandler.config` och öppna dess konfiguration. Lägg till `service.ranking=I"-10"` i slutet. Spara ändringarna.

   >[!NOTE]
   >
   >Varje begäran som autentiseras med en innehavartoken medför att tre anrop till Adobe IMS, användarsynkronisering och skapandet av en inloggningstoken görs i Experience Manager. Adobe Asset Link fångar upp den inloggningstoken som returnerades i svaret från Experience Manager och skickar den med efterföljande förfrågningar. För att den här processen ska fungera måste den relativa prioriteten för hanteraren för innehavarautentisering justeras.

1. (Valfritt) Om Experience Manager-användarna har versaler eller blandade domännamn i sina e-post-ID:n väljer du **[!UICONTROL Change Locking User to Lower Case]** i **[!UICONTROL Adobe Granite ACP Platform Configs]** i Experience Manager Web Console.

## Ytterligare konfiguration efter migrering till affärsprofiler {#configure-migration-activity}

Adobe Asset Link-användare kan ansluta till Experience Manager för att tillåta IMS-inloggning från huvudorganisationen för Creative Cloud for Enterprise (CCE). Experience Manager använder klient-ID för att identifiera den tillåtna IMS-organisationen. Efter migrering till affärsprofiler måste du konfigurera klient-ID och hemlig nyckel för IMS-organisationen i Experience Manager för Bearer Authentication Handler. Mer information om affärsprofiler finns i [Introduktion till Adobe-profiler](https://helpx.adobe.com/enterprise/kb/introducing-adobe-profiles.html).

Ytterligare konfiguration krävs endast om du använder olika Adobe IMS-organisationer för Experience Manager och Creative Cloud for Enterprise (CCE) och en domänförtroenderelation upprättas mellan dessa två organisationer.

>[!NOTE]
>
>* Korrigering för företagsprofiler tillhandahålls i Experience Manager 6.5.11.0.
>* Den befintliga konfigurationen fortsätter att fungera om du använder samma Adobe IMS-organisation med Experience Manager och CCE.


**Förutsättningar**

1. En Experience Manager-instans som körs och som har Bearer Authentication konfigurerat för AAL.
1. Installera följande paket (Service Pack 11) på din Experience Manager 6.5-instans.

   [Hämta Experience Manager 6.5.11.0](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack/aem-service-pkg-6.5.11.zip)

1. Kontakta [!UICONTROL Customer Support] om du vill hämta klient-ID och hemlig nyckel för Bearer-autentisering för din IMS-organisation.

Följande ytterligare konfigurationer krävs efter migrering till affärsprofiler:

1. I **[!UICONTROL Adobe Granite OAuth IMS Configuration Provider]** (`com.adobe.granite.auth.ims.impl.ImsConfigProviderImpl`), ange:

   * OAuth Configuration ID (`oauth.configmanager.ims.configid`): `ims` (Verifiera en gång, du kanske redan har det konfigurerat)

   * IMS-ägarentitet (`ims.owningEntity`): ID för IMS-organisation

   ![IMS-konfigurations-ID](assets/bearer-authentication1.png)

1. Öppna konfigurationen **[!UICONTROL Bearer Authentication Handler]** och lägg till klient-ID:t som hämtats från [!UICONTROL Customer Support] i listan med **[!UICONTROL Allowed OAuth client ids]**.

   ![Lägg till klient-ID](assets/add-clientid-bearer-auth.png)

1. Öppna **[!UICONTROL Adobe Granite OAuth Application and Provider]**-konfigurationen och lägg till **[!UICONTROL Client ID]** och **[!UICONTROL Client Secret]** (hemlig nyckel) som hämtats från kundsupporten.

   Kontrollera att fältet **[!UICONTROL Config ID]** (`oauth.config.id`) innehåller samma värde som anges i fältet **[!UICONTROL OAuth configuration ID]** (`oauth.configmanager.ims.configid`) ovan.

   ![Verifiera klient-ID](assets/clientid-secretkey.png)

1. Öppna **[!UICONTROL Adobe Granite IMS Cluster Exchange Token Preprocessor]**-konfigurationen och ange den till `enable`.

## Hantera åtkomstkontroll för användare {#user-access}

I det här avsnittet beskrivs hur du hanterar användare och deras åtkomst till Experience Manager-databasen.

### Gruppmappning {#group-mapping}

Gruppmappning avgör hur grupper i Experience Manager motsvarar grupper i Adobe IMS. Det spelar en viktig roll när det gäller hur Adobe Asset Link-användare ges behörighet att komma åt Experience Manager Assets.

När Experience Manager används med Adobe Asset Link delegeras användarhanteringsfunktioner till Adobe IMS. Användare och grupper som motsvarar användare och grupper i Adobe IMS skapas automatiskt. Dessutom synkroniseras användare, grupper och gruppmedlemskap i Experience Manager så att de matchar dem i Adobe IMS.

Tänk dig till exempel ett scenario där Adobe Asset Link-användare är medlemmar i Adobe IMS-gruppen assetlink-users. I det här fallet skapas en synkroniserad grupp med namnet assetlink-users i Experience Manager när en användare från den Adobe IMS-gruppen ansluter till Adobe Asset Link för första gången. Varje ny användare i Adobe IMS-gruppen läggs till i motsvarande grupp i Experience Manager när de ansluter till Experience Manager via Adobe Asset Link för första gången.

Grupper i Experience Manager som motsvarar och synkroniseras med grupper i Adobe IMS kan beviljas åtkomst direkt eller genom att de blir medlemmar i en annan grupp. Här är ett exempel på hur behörigheter kan hanteras.

![gruppexempel](assets/group-examples.png)

Följande regler gäller för gruppmappningar i Experience Manager:

* Kontrollera att egenskapen **[!UICONTROL Group Mappings]** i konfigurationen **[!UICONTROL Adobe Granite OAuth IMS Provider]** är tom.
* Medlemskap i användargruppen Adobe Asset Link utvärderas när användaren autentiserar sig och tidsperioden i egenskapen **[!UICONTROL User Expiration Time]** i konfigurationen **[!UICONTROL Apache Jackrabbit Oak Default Sync Handler]** har gått ut. För närvarande kan användare läggas till i och tas bort från grupper i Experience Manager för att synkronisera med det som finns i Adobe IMS.
* Undvik gruppnamnskonflikter. Kontrollera att namnen som används för grupper som skapats i Adobe IMS (för att hantera användare) inte är samma som alla Experience Manager systemgruppnamn.

  Kontrollera till exempel att de skiljer sig från gruppen `dam-users` och de grupper som skapas av Experience Manager-administratören.

  En Adobe IMS-grupp vars namn står i konflikt med namnet på en Experience Manager-systemgrupp eller grupp som skapats manuellt används inte för att styra användarbehörigheter.
* Om en Adobe IMS-användare ansluter till en Experience Manager-instans, där användarens namn står i konflikt med en tidigare skapad Experience Manager-användare, får Adobe IMS-användaren ett annat namn med siffror som läggs till så att det blir unikt.

**Konfigurera förstagångskontroll för åtkomst**

Användare som ansluter via Adobe Asset Link kan bara visa och interagera med resurser efter att de har fått den behörighet som krävs. I avsnittet [Gruppmappning](#group-mapping) ovan beskrivs hur användargrupper som skapats i Experience Manager, som motsvarar och synkroniseras med användargrupper i din organisation inom Adobe IMS, är skapade i. Vi rekommenderar att Experience Manager-administratörerna använder dessa grupper för att hantera åtkomstkontroll för Adobe Asset Link-användare.

För varje Experience Manager-grupp som synkroniseras med en Adobe IMS-grupp (som används för att hantera åtkomstkontroll):

1. Kontrollera att gruppen har en medlem som kan användas för en första anslutning från Adobe Asset Link.
1. Använd den användaren för att logga in på Adobe Asset Link och ansluta till Experience Manager. Den här anslutningen förväntas misslyckas.
1. I Experience Manager letar du reda på den grupp som motsvarar gruppen i Adobe IMS och ger den önskad åtkomstkontroll. Den nya gruppen blir till exempel medlem i gruppen dam-users.
1. Stäng Adobe Asset Link och starta om Creative Cloud.
1. Kontrollera att användaren har rätt åtkomst genom att öppna Adobe Asset Link igen.

När de här stegen har utförts kan andra användare i samma grupp ansluta till Experience Manager med Adobe Asset Link vid första försöket. De får automatiskt samma behörigheter som de andra användarna i gruppen.

## Hantera Experience Manager-användare för Adobe Asset Link {#manage-users}

Adobe Asset Link-användare kan ansluta till Experience Manager när de är inloggade i sina Creative Cloud-program. Denna autentisering använder Adobe IMS-teknik och skapar användarinformation i Experience Manager, om den inte finns. Det är vanligt att Experience Manager Enterprise-kunder hanterar sina användare med en extern identitetsleverantör som är integrerad med Experience Manager. Identitetsleverantörer inkluderar Adobe IMS och andra produkter som använder SAML- och LDAP-protokoll. Du kan också skapa och hantera användare lokalt i Experience Manager.

Användare som ansluter till Experience Manager från Adobe Asset Link har ingen konflikt med befintlig användarinformation som lagras i Experience Manager från tidigare direktinloggning, om:

* Alla användarnamn som används för direkt inloggning på Experience Manager skiljer sig från användarnamn som används i Adobe IMS för Creative Cloud-inloggning.
* Adobe IMS används som identitetsleverantör för direkt inloggning från Experience Manager.
* Användare ansluter till Experience Manager från Adobe Asset Link innan de direkt loggar in på Experience Manager med samma konto.


Å andra sidan måste användarinformationen som skapas som ett resultat av direkt inloggning från Experience Manager uppdateras för att fungera med Adobe Asset Link i följande scenarier:

* Samma användarnamn, t.ex. användarens e-postadress, används för båda - kontot i Creative Cloud, som använder Adobe IMS, och kontot i en annan extern identitetsleverantör än Adobe IMS.
* Samma användarnamn används för båda, dvs. kontot i Creative Cloud och ett lokalt Experience Manager-konto.
* Creative Cloud-konton i Adobe IMS är Federated ID:n, som hanteras av samma externa identitetsleverantör som är integrerad med Experience Manager för direkt inloggning.

De användare som skapas med dessa scenarier har ingen egenskap som krävs för användare, som synkroniseras med Adobe IMS.

Så här uppdaterar du användare i Experience Manager så att de kan arbeta med Adobe Asset Link:

1. Leta reda på konfigurationen för **[!UICONTROL Apache Jackrabbit Oak External PrincipalConfiguration]** i Experience Manager webbkonsol och klicka för att redigera den. Avmarkera kryssrutan **[!UICONTROL External Identity Protection]** och klicka på **[!UICONTROL Save]**.
1. Om du vill komma åt användarhanteringsgränssnittet i Experience Manager går du till **[!UICONTROL Tools]** > **[!UICONTROL Security]** > **[!UICONTROL Users]**. Markera den användare som du vill uppdatera och notera sedan slutet på webbläsarens URL-sökväg för den användaren, med början `/home/users`. Du kan också söka efter användarnamnet i CRXDE. Ett exempel på användarsökväg: `/home/users/x/xTac082TDh-guJzzG7WM`.
1. I CRXDE navigerar du till användarsökvägen, markerar användarnoden och visar nodens egenskaper genom att välja fliken **[!UICONTROL Properties]** i området längst ned i mitten. Den här noden har `jcr:primaryType`-egenskapsvärdet `rep:User`.
1. Längst ned på fliken **[!UICONTROL Properties]** anger du `Name` värdet `rep:externalId`, `Type` värdet `String` och `Value` värdet `rep:authorizableId`;`ims`, där `rep:authorizableId` är värdet på egenskapen `rep:authorizableId` för noden. (Ett semikolon används utan blanksteg för att skilja värdet `rep:authorizableId` från `ims`.)
1. Klicka på knappen **[!UICONTROL Add]** till höger om det nya inlägget och klicka sedan på **[!UICONTROL Save All]**.
1. Upprepa steg 2 till 5 för alla andra användare som du vill uppgradera för att arbeta med Adobe Asset Link.
1. Leta reda på konfigurationen för **[!UICONTROL Apache Jackrabbit Oak External PrincipalConfiguration]** i Experience Manager webbkonsol och klicka för att redigera den. Avmarkera kryssrutan **[!UICONTROL External Identity Protection]** och klicka på **[!UICONTROL Save]**.

>[!NOTE]
>
>Om tjänsterna inte återställs på några minuter startar du om Experience Manager för att tillåta lyckade autentiseringar.

Efter den här ändringen kan en uppdaterad Experience Manager-användare ansluta till Adobe Asset Link och fortsätta att använda den metod för direktinloggning till Experience Manager som användes före uppdateringen. När autentiseringen med Adobe IMS är klar synkroniseras Experience Manager användarprofilinformation med användarprofilen i Adobe IMS.

Det finns en metod som gör att flera Experience Manager-användare kan migreras satsvis för att de ska kunna arbeta med Adobe Asset Link. Kontakta Adobe Care för mer information och hjälp med att aktivera det här alternativet.

Som ett alternativ till dessa steg kan en Adobe Asset Link-användare under vissa omständigheter få snabb åtkomst till Experience Manager. I sådana fall hittas och raderas den befintliga användarinformationen med Experience Manager användarhantering eller Experience Manager CRXDE innan de ansluter till Adobe Asset Link. Ny användarinformation skapas i Experience Manager efter anslutningen. Använd bara den här metoden om du är säker på att det inte finns några viktiga data som läggs till som underordnad till användarnoden. Sådana extra data är alla noder som är underordnade användarnoden förutom noderna `tokens`, `preferences`, `profile`, `profiles`, `profiles/public` och `rep:policy/*`.

## Starta arbetsflödet automatiskt för att bearbeta resurser på villkor {#auto-start-workflow}

I Experience Manager 6.4 och Experience Manager 6.5 kan administratörer konfigurera arbetsflöden så att resurser automatiskt körs och bearbetas baserat på fördefinierade villkor.

Konfigurationen är användbar för företagsanvändare och marknadsförare, till exempel för att skapa ett anpassat arbetsflöde i några specifika mappar. Anta att alla resurser från en reklambyrås foton kan vara vattenstämplade eller att alla resurser som överförts av en frilansare kan bearbetas för att skapa specifika renderingar.

Mer information och information om Experience Manager-konfiguration finns i [autokörningsarbetsflöde för resurser](https://experienceleague.adobe.com/docs/experience-manager-65-2025/assets/using/assets-workflow.html#auto-execute-workflow-on-some-assets).


## Skapa ett anpassat index i Experience Manager 6.4.x {#create-custom-index}

Experience Manager innehåller index som används för frågor. Skapa följande anpassade index för den angivna versionen. Experience Manager 6.5.0 innehåller detta index som standard. Adobe Asset Link kräver detta index för att avgöra vilka resurser en användare har checkat ut.

1. Leta reda på noden `/oak:index` i CRXDE. Skapa en nod med namnet `cqDrivelock` och ställ in dess `Type` på `oak:QueryIndexDefinition`.

1. Lägg till följande egenskaper i den nya noden och spara ändringarna:

   * `Name: type; Type: string; Value: property`

   * `Name: propertyNames; Type: Name[] (click the "Multi" button); Value: cq:drivelock`


## Konfigurera visuell sökning eller likhetssökning {#configure-visual-similarity-search}

Med funktionen för visuell sökning kan du söka efter visuellt liknande resurser i AEM Assets-databasen med hjälp av panelen Adobe Asset Link. Funktionen är tillgänglig i version 6.5.0 eller senare och endast indexerade resurser söks igenom. Mer information finns i [Konfigurera visuell sökning](https://experienceleague.adobe.com/docs/experience-manager-65-2025/assets/using/search-assets.html#configvisualsearch).

## Generera renderingar endast för placering för Adobe InDesign {#fpo-renditions}

Experience Manager tillhandahåller renderingar som endast används för placering (FPO). Dessa FPO-återgivningar har en liten filstorlek men har samma proportioner. Om det inte finns någon FPO-återgivning tillgänglig för en resurs använder Adobe InDesign den ursprungliga resursen i stället. Denna reservfunktion säkerställer att det kreativa arbetsflödet fortsätter utan avbrott. Mer information finns i [generera FPO-återgivningar](/help/assets/configure-fpo-renditions.md).


## Integrera med Adobe Stock {#adobe-stock-integration}

Organisationer integrerar sina Adobe Stock-konton med Experience Manager Assets. Det hjälper marknadsförarna att göra licensierade högkvalitativa, royaltyfria foton, vektorer, illustrationer, videor, mallar och 3D-resurser tillgängliga för sina kreativa projekt och marknadsföringsprojekt. Kreatörer kan använda dessa resurser med hjälp av panelen Resurslänk.

Mer information om hur du integrerar med Adobe Stock finns i [Adobe Stock-resurser i Experience Manager Assets](/help/assets/aem-assets-adobe-stock.md). Experience Manager 6.4.2 eller senare krävs för integrering med Adobe Stock.


## Felsöka problem relaterade till Experience Manager {#troubleshoot}


Om du får problem när du konfigurerar eller använder Adobe Asset Link kan du försöka med följande:

* Se till att driftsättningen uppfyller kraven. Kontrollera i synnerhet att rätt funktionspaket eller funktionspaket är installerade.
* Kontakta din organisations partner eller systemintegratör.
* Om dina Creative Cloud-användare inte kan verifiera de utcheckade resurserna ska du kontrollera om domännamnen finns i e-post-ID:n. Mer information finns i [manuell konfiguration](#manual-configuration).
* Mer information finns i [felsökning av resurslänk](https://helpx.adobe.com/enterprise/kb/asset-link-troubleshooting.html).


>[!MORELIKETHIS]
>
>* [Om Adobe Asset Link](https://helpx.adobe.com/enterprise/using/adobe-asset-link.html)
>* [Använd resurslänk i Creative Cloud-datorprogrammet och hantera resurser](https://helpx.adobe.com/se/enterprise/using/manage-assets-using-adobe-asset-link.html)
>* [Konfigurera Adobe Experience Manager Assets as a Cloud Service](https://helpx.adobe.com/enterprise/using/configure-aem-assets-for-asset-link.html).
