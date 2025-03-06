---
title: SAML 2.0-autentiseringshanterare
description: Läs mer om SAML 2.0 Authentication Handler i AEM.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Security
role: Admin
exl-id: ee438c55-88cd-4f55-873e-16376b36fa7b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '814'
ht-degree: 0%

---

# SAML 2.0-autentiseringshanterare{#saml-authentication-handler}

AEM levereras med en [SAML](https://saml.xml.org/saml-specifications)-autentiseringshanterare. Hanteraren stöder [SAML](https://saml.xml.org/saml-specifications) 2.0 Authentication Request Protocol (Web-SSO-profil) med bindningen `HTTP POST`.

Den stöder:

* signering och kryptering av meddelanden
* automatiskt skapa användare
* synkronisera grupper med befintliga i AEM
* Tjänsteleverantören och identitetsleverantören initierade autentisering

Den här hanteraren lagrar det krypterade SAML-svarsmeddelandet i användarnoden ( `usernode/samlResponse`) för att underlätta kommunikationen med en tredjeparts tjänsteleverantör.

>[!NOTE]
>
>Se [en demonstration av AEM- och SAML-integrering](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17481.html).

## Konfigurera autentiseringshanteraren för SAML 2.0 {#configuring-the-saml-authentication-handler}

[Webbkonsolen](/help/sites-deploying/configuring-osgi.md) ger åtkomst till autentiseringshanterarkonfigurationen [SAML](https://saml.xml.org/saml-specifications) 2.0 med namnet **Adobe Granite SAML 2.0 Authentication Handler**. Följande egenskaper kan anges.

>[!NOTE]
>
>Autentiseringshanteraren för SAML 2.0 är inaktiverad som standard. Aktivera hanteraren genom att ange minst en av följande egenskaper:
>
>* Identitetsleverantörens POST-URL eller IDP-URL.
>* Tjänstleverantörens enhets-ID.
>

>[!NOTE]
>
>SAML-försäkringar är signerade och kan eventuellt krypteras. För att detta ska fungera måste du tillhandahålla minst det offentliga certifikatet för identitetsleverantören i TrustStore. Mer information finns i [Lägga till IdP-certifikatet i avsnittet TrustStore](/help/sites-administering/saml-2-0-authenticationhandler.md#add-the-idp-certificate-to-the-aem-truststore).

**Sökväg** Databassökväg som den här autentiseringshanteraren ska användas för av Sling. Om detta är tomt inaktiveras autentiseringshanteraren.

**Rankning av tjänst** OSGi Framework Service Ranking-värde som anger i vilken ordning tjänsten ska anropas. Detta är ett heltalsvärde där högre värden anger högre prioritet.

**IDP-certifikatalias** Aliaset för IdP-certifikatet i det globala förtroendelagret. Om den här egenskapen är tom inaktiveras autentiseringshanteraren. Se kapitlet&quot;Add the IdP Certificate to the AEM TrustStore&quot; nedan om hur du konfigurerar det.

**IDP URL** för IDP där SAML-autentiseringsbegäran ska skickas. Om den här egenskapen är tom inaktiveras autentiseringshanteraren.

>[!CAUTION]
>
>Identitetsproviderns värdnamn måste läggas till i OSGi-konfigurationen för **Apache Sling Referrer Filter** . Mer information finns i avsnittet [Webbkonsol](/help/sites-deploying/configuring-osgi.md).

**Tjänstleverantörens enhets-ID** som unikt identifierar den här tjänstprovidern med identitetsprovidern. Om den här egenskapen är tom inaktiveras autentiseringshanteraren.

**Standardomdirigering** Standardplatsen att omdirigera till efter lyckad autentisering.

>[!NOTE]
>
>Den här platsen används bara om cookien `request-path` inte har angetts. Om du begär en sida under den konfigurerade sökvägen utan en giltig inloggningstoken, lagras den begärda sökvägen i en cookie
>och webbläsaren kommer att omdirigeras till den här platsen igen när autentiseringen är klar.

**Användar-ID-attribut** Namnet på attributet som innehåller det användar-ID som används för att autentisera och skapa användaren i CRX-databasen.

>[!NOTE]
>
>Användar-ID:t hämtas inte från noden `saml:Subject` i SAML-försäkran utan från denna `saml:Attribute`.

**Använd kryptering** Om den här autentiseringshanteraren förväntar sig krypterade SAML-kontroller eller inte.

**Skapa CRX-användare automatiskt** Om icke-befintliga användare ska skapas automatiskt i databasen efter lyckad autentisering eller inte.

>[!CAUTION]
>
>Om CRX-användare inte kan skapas automatiskt måste de skapas manuellt.

**Lägg till i grupper** Om en användare automatiskt ska läggas till i CRX-grupper efter lyckad autentisering eller inte.

**Gruppmedlemskap** Namnet på saml:Attribute som innehåller en lista över CRX-grupper som den här användaren ska läggas till i.

## Lägg till IdP-certifikatet i AEM TrustStore {#add-the-idp-certificate-to-the-aem-truststore}

SAML-försäkringar är signerade och kan eventuellt krypteras. För att detta ska fungera måste du ange minst det offentliga certifikatet för IdP i databasen. För att göra detta måste du:

1. Gå till *http:/serveraddress:serverport/libs/granite/security/content/truststore.html*
1. Tryck på **[!UICONTROL Create TrustStore link]**
1. Ange lösenordet för TrustStore och tryck på **[!UICONTROL Save]**.
1. Klicka på **[!UICONTROL Manage TrustStore]**.
1. Överför IdP-certifikatet.
1. Notera certifikatet Alias. Aliaset är **[!UICONTROL admin#1436172864930]** i exemplet nedan.

   ![chlimage_1-372](assets/chlimage_1-372.png)

## Lägg till tjänstleverantörens nyckel och certifikatkedja i AEM nyckelbehållare {#add-the-service-provider-key-and-certificate-chain-to-the-aem-keystore}

>[!NOTE]
>
>Nedanstående steg är obligatoriska, annars kommer följande undantag att genereras: `com.adobe.granite.keystore.KeyStoreNotInitialisedException: Uninitialised system trust store`

1. Gå till: [http://localhost:4502/libs/granite/security/content/useradmin.html](http://localhost:4502/libs/granite/security/content/useradmin.html)
1. Redigera användaren `authentication-service`.
1. Skapa en KeyStore genom att klicka på **Create KeyStore** under **Kontoinställningar**.

>[!NOTE]
>
>Stegen nedan är bara obligatoriska om hanteraren ska kunna signera eller dekryptera meddelanden.

1. Skapa certifikat/nyckelpar för AEM. Kommandot för att generera det via openssl bör likna exemplet nedan:

   `openssl req -newkey rsa:2048 -new -x509 -days 3652 -nodes -out certificate.crt -keyout key.pem`

1. Konvertera nyckeln till PKCS#8-format med DER-kodning. Detta är det format som krävs av AEM nyckelbehållare.

   `openssl pkcs8 -topk8 -inform PEM -outform DER -in key.pem -out key.der -nocrypt`

1. Överför filen med den privata nyckeln genom att klicka på **Välj filen med den privata nyckeln**.
1. Ladda upp certifikatfilen genom att klicka på **Välj certifikatkedjefiler**.
1. Tilldela ett alias enligt nedan:

   ![chlimage_1-373](assets/chlimage_1-373.png)

## Konfigurera en loggare för SAML {#configure-a-logger-for-saml}

Du kan konfigurera en loggare för att felsöka problem som kan uppstå när SAML felkonfigureras. Du kan göra detta genom att:

1. Gå till webbkonsolen, på *http://localhost:4502/system/console/configMgr*
1. Sök efter och klicka på posten **Konfiguration för Apache Sling Logger**
1. Skapa en loggare med följande konfiguration:

   * **Loggnivå:** Felsökning
   * **Loggfil:** logs/saml.log
   * **Logger:**.com.adobe.granite.auth.saml
