---
title: Aktivera enkel inloggning i AEM-formulär
description: Lär dig hur du aktiverar enkel inloggning (SSO) med HTTP-huvuden och SPNEGO.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
hide: true
hidefromtoc: true
exl-id: ba02f9b1-209e-42f2-b1df-2ed64fc9fdbc
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1716'
ht-degree: 0%

---

# Aktivera enkel inloggning i AEM-formulär{#enabling-single-sign-on-in-aem-forms}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

AEM-formulär erbjuder två sätt att aktivera enkel inloggning (SSO) - HTTP-huvuden och SPNEGO.

När enkel inloggning är implementerad är inloggningssidorna för AEM-formulär inte obligatoriska och visas inte om användaren redan är autentiserad via sin företagsportal.

Om AEM-formulär inte kan autentisera en användare på något av dessa sätt dirigeras användaren om till en inloggningssida.

* [Aktivera enkel inloggning med HTTP-huvuden](#enable-sso-using-http-headers)
* [Aktivera enkel inloggning med SPNEGO](#enable-sso-using-spnego)
* [Tilldela roller till användare och grupper](#assign-roles-to-users-groups)

## Aktivera enkel inloggning med HTTP-huvuden {#enable-sso-using-http-headers}

Du kan använda sidan Portal Configuration för att aktivera enkel inloggning (SSO) mellan program och alla program som stöder överföring av identiteten via ett HTTP-huvud. När enkel inloggning är implementerad är inloggningssidorna för AEM-formulär inte obligatoriska och visas inte om användaren redan är autentiserad via sin företagsportal.

Du kan även aktivera enkel inloggning med SPNEGO. (Se [Aktivera enkel inloggning med SPNEGO](enabling-single-sign-on-aem.md#enable-sso-using-spnego).)

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Konfigurera portalattribut.
1. Välj Ja om du vill aktivera enkel inloggning. Om du väljer Nej är de återstående inställningarna på sidan inte tillgängliga.
1. Ange de återstående alternativen på sidan efter behov och klicka på OK:

   * **SSO-typ:** (obligatoriskt) Välj HTTP Header för att aktivera enkel inloggning med HTTP-huvuden.
   * **HTTP-huvud för användarens identifierare:** (Obligatoriskt) Namnet på det huvud vars värde innehåller den inloggade användarens unika identifierare. Användarhantering använder det här värdet för att hitta användaren i databasen för användarhantering. Värdet som hämtas från den här rubriken ska matcha den unika identifieraren för den användare som synkroniseras från LDAP-katalogen. (Se [Användarinställningar](/help/forms/using/admin-help/adding-configuring-users.md#user-settings).)
   * **Identifierarvärdet mappas till användarens användar-ID i stället för användarens unika identifierare:** Mappar användarens unika identifierarvärde till användar-ID:t. Välj det här alternativet om användarens unika identifierare är ett binärt värde som inte enkelt kan spridas via HTTP-huvuden (till exempel objectGUID om du synkroniserar användare från Active Directory).
   * **HTTP-huvud för domänen:** (Inte obligatoriskt) Namn på huvudet vars värde innehåller domännamnet. Använd bara den här inställningen om ingen enskild HTTP-rubrik unikt identifierar användaren. Använd den här inställningen för fall där det finns flera domäner och den unika identifieraren bara är unik inom en domän. I det här fallet anger du rubriknamnet i den här textrutan och anger domänmappning för flera domäner i rutan Domänmappning. (Se [Redigera och konvertera befintliga domäner](/help/forms/using/admin-help/editing-converting-existing-domains.md#editing-and-converting-existing-domains).)
   * **Domänmappning:** (obligatoriskt) Anger mappning för flera domäner i formatet *header value=domännamn*.

     Tänk dig till exempel en situation där HTTP-huvudet för en domän är domainName och kan ha värdena domain1, domain2 och domain3. I det här fallet använder du domänmappning för att mappa domainName-värden till domännamn för användarhantering. Alla mappningar måste finnas på olika rader:

     domain1=UMdomain1

     domain2=UMdomain2

     domain3=UMdomain3

### Konfigurera tillåtna referenser {#configure-allowed-referers}

Anvisningar om hur du konfigurerar tillåtna referenter finns i [Konfigurera tillåtna referenter](/help/forms/using/admin-help/preventing-csrf-attacks.md#configure-allowed-referers).

### Tilldela roller till användare och grupper

Klicka om du vill veta hur du [tilldelar roller till användare och grupper](/help/forms/using/admin-help/enabling-single-sign-on-aem.md#assign-roles-to-users-and-groups-assign-roles-to-users-groups).

## Aktivera enkel inloggning med SPNEGO {#enable-sso-using-spnego}

Du kan använda enkel och skyddad GSSAPI-förhandlingsmekanism (SPNEGO) för att aktivera enkel inloggning (SSO) när du använder Active Directory som LDAP-server i en Windows-miljö. När enkel inloggning är aktiverad krävs inte inloggningssidorna för AEM-formulär och de visas inte.

Du kan även aktivera enkel inloggning med HTTP-huvuden. (Se [Aktivera enkel inloggning med HTTP-huvuden](enabling-single-sign-on-aem.md#enable-sso-using-http-headers).)

>[!NOTE]
>
>AEM Forms på JEE stöder inte konfigurering av enkel inloggning med Kerberos/SPNEGO i flera underordnade domänmiljöer.

1. Bestäm vilken domän som ska användas för att aktivera enkel inloggning. AEM Forms Server och användarna måste tillhöra samma Windows-domän eller betrodda domän.
1. I Active Directory skapar du en användare som representerar AEM Forms Server. (Se [Skapa ett användarkonto](enabling-single-sign-on-aem.md#create-a-user-account).) Om du konfigurerar mer än en domän att använda SPNEGO måste du se till att lösenorden för var och en av dessa användare är olika. Om lösenorden inte är olika fungerar inte SPNEGO SSO.
1. Mappa tjänstens huvudnamn. (Se [Mappa ett SPN (Service Principal Name)](enabling-single-sign-on-aem.md#map-a-service-principal-name-spn).)
1. Konfigurera domänkontrollanten. (Se [Förhindra Kerberos-integritetskontrollfel](enabling-single-sign-on-aem.md#prevent-kerberos-integrity-check-failures).)
1. Lägg till eller redigera en företagsdomän enligt beskrivningen i [Lägga till domäner](/help/forms/using/admin-help/adding-domains.md#adding-domains) eller [Redigera och konvertera befintliga domäner](/help/forms/using/admin-help/editing-converting-existing-domains.md#editing-and-converting-existing-domains). Utför följande uppgifter när du skapar eller redigerar företagsdomänen:

   * Lägg till eller redigera en katalog som innehåller din Active Directory-information.
   * Lägg till LDAP som autentiseringsprovider.
   * Lägg till Kerberos som autentiseringsprovider. Ange följande information på sidan Ny eller Redigera autentisering för Kerberos:

      * **Autentiseringsprovider:** Kerberos
      * **DNS-IP:** DNS-IP-adressen för den server där AEM-formulär körs. Du kan fastställa den här IP-adressen genom att köra `ipconfig/all` på kommandoraden.
      * **KDC-värd:** Fullständigt kvalificerat värdnamn eller IP-adress för Active Directory-servern som används för autentisering
      * **Tjänstanvändare:** Tjänstens huvudnamn (SPN) som skickades till KtPass-verktyget. I det exempel som användes tidigare är tjänstanvändaren `HTTP/lcserver.um.lc.com`.
      * **Tjänstsfär:** Domännamn för Active Directory. I exemplet som användes tidigare är domännamnet `UM.LC.COM.`
      * **Lösenord för tjänst:** Lösenord för tjänstanvändare. I det exempel som användes tidigare är tjänstlösenordet `password`.
      * **Aktivera SPNEGO:** Aktiverar användning av SPNEGO för enkel inloggning (SSO). Välj det här alternativet.

1. Konfigurera inställningar för SPNEGO-klientwebbläsare. (Se [Konfigurera inställningar för SPNEGO-klientwebbläsare](enabling-single-sign-on-aem.md#configuring-spnego-client-browser-settings).)

### Skapa ett användarkonto {#create-a-user-account}

1. I SPNEGO registrerar du en tjänst som användare i Active Directory på domänkontrollanten som representerar AEM-formulär. På domänkontrollanten går du till Start-menyn > Administrationsverktyg > Active Directory - användare och datorer. Om Administrationsverktyg inte finns på Start-menyn använder du Kontrollpanelen.
1. Klicka på mappen Användare för att visa en lista över användare.
1. Högerklicka på användarmappen och välj Nytt > Användare.
1. Skriv förnamn/efternamn och användarnamn och klicka sedan på Nästa. Ange till exempel följande värden:

   * **Förnamn**: umspnego
   * **Användarnamn för inloggning**: spnegodemo

1. Ange ett lösenord. Ange det till exempel till *password*. Kontrollera att Lösenordet aldrig förfaller är markerat och att inga andra alternativ är markerade.
1. Klicka på Nästa och sedan på Slutför.

### Mappa ett SPN (Service Principal Name) {#map-a-service-principal-name-spn}

1. Hämta KtPass-verktyget. Det här verktyget används för att mappa ett SPN till en REALM. Du kan hämta KtPass-verktyget som en del av Windows Server Tool Pack eller Resource Kit. (Se [Supportverktyg för Windows Server 2003 Service Pack 1](https://support.microsoft.com/kb/892777).)
1. Kör `ktpass` i en kommandotolk med följande argument:

   `ktpass -princ HTTP/`*host* `@`*REALM* `-mapuser`*user*

   Skriv till exempel följande text:

   `ktpass -princ HTTP/lcserver.um.lc.com@UM.LC.COM -mapuser spnegodemo`

   Värdena som du måste ange beskrivs enligt följande:

   **host:** Fullständigt kvalificerat namn för Forms Server eller en unik URL. I det här exemplet är det inställt på lcserver.um.lc.com.

   **REALM:** Active Directory-sfären för domänkontrollanten. I det här exemplet är det inställt på UM.LC.COM. Se till att du anger sfären med versaler. Utför följande steg för att fastställa sfären för Windows 2003:

   * Högerklicka på Den här datorn och välj Egenskaper
   * Klicka på fliken Datornamn. Domännamnsvärdet är sfärnamnet.

   **användare:** Inloggningsnamnet för det användarkonto du skapade i föregående uppgift. I det här exemplet är det inställt på spnegodemo.

Om det här felet inträffar:

```shell
DsCrackNames returned 0x2 in the name entry for spnegodemo.
ktpass:failed getting target domain for specified user.
```

prova att ange användaren som spnegodemo@um.lc.com:

```shell
ktpass -princ HTTP/lcserver.um.lc.com@UM.LC.COM -mapuser spnegodemo
```

### Förhindra fel i Kerberos-integritetskontroll {#prevent-kerberos-integrity-check-failures}

1. På domänkontrollanten går du till Start-menyn > Administrationsverktyg > Active Directory - användare och datorer. Om Administrationsverktyg inte finns på Start-menyn använder du Kontrollpanelen.
1. Klicka på mappen Användare för att visa en lista över användare.
1. Högerklicka på användarkontot som du skapade i en tidigare uppgift. I detta exempel är användarkontot `spnegodemo`.
1. Klicka på Återställ lösenord.
1. Skriv och bekräfta samma lösenord som du angav tidigare. I det här exemplet är det inställt på `password`.
1. Avmarkera Ändra lösenord vid nästa inloggning och klicka sedan på OK.

### Konfigurera inställningar för SPNEGO-klientwebbläsare {#configuring-spnego-client-browser-settings}

För att SPNEGO-baserad autentisering ska fungera måste klientdatorn vara en del av domänen som användarkontot skapas i. Du måste också konfigurera klientwebbläsaren så att den tillåter SPNEGO-baserad autentisering. Dessutom måste webbplatsen som kräver SPNEGO-baserad autentisering vara en betrodd plats.

Om servern används med datornamnet, till exempel https://lcserver:8080, krävs inga inställningar för Internet Explorer. Om du anger en URL som inte innehåller några punkter (&quot;.&quot;) behandlas platsen som en lokal intranätsplats i Internet Explorer. Om du använder ett fullständigt kvalificerat namn för webbplatsen måste webbplatsen läggas till som en betrodd plats.

**Konfigurera Internet Explorer 6.x**

1. Gå till Verktyg > Internetalternativ och klicka på fliken Säkerhet.
1. Klicka på ikonen Lokalt intranät och sedan på Webbplatser.
1. Klicka på Avancerat och skriv webbadressen till din Forms-server i rutan Lägg till den här webbplatsen i zonen. Skriv till exempel `https://lcserver.um.lc.com`
1. Klicka på OK tills alla dialogrutor är stängda.
1. Testa konfigurationen genom att gå till webbadressen för din AEM Forms Server. Skriv t.ex. `https://lcserver.um.lc.com:8080/um/login?um_no_redirect=true` i URL-adressrutan i webbläsaren

**Konfigurera Mozilla Firefox**

1. Skriv `about:config` i rutan Webbläsarens URL

   Dialogrutan about:config - Mozilla Firefox visas.

1. Skriv `negotiate` i rutan Filter
1. I listan som visas klickar du på network.gotiate-auth.trusted-uri och anger något av följande kommandon som passar din miljö:

   `.um.lc.com`- Konfigurerar Firefox så att SPNEGO tillåts för alla URL:er som slutar med um.lc.com. Se till att du tar med punkten (&quot;.&quot;) i början.

   `lcserver.um.lc.com` - Konfigurerar Firefox så att SPNEGO endast tillåts för din specifika server. Börja inte det här värdet med en punkt (&quot;.&quot;).

1. Testa konfigurationen genom att använda programmet. Välkomstsidan för målprogrammet ska visas.

Klicka om du vill veta hur du [tilldelar roller till användare och grupper](/help/forms/using/admin-help/enabling-single-sign-on-aem.md#assign-roles-to-users-and-groups-assign-roles-to-users-groups).

## Tilldela roller till användare och grupper {#assign-roles-to-users-groups}

1. Logga in på din AEM Forms i JEE-miljö.
1. Klicka på Inställningar > Användarhantering > Domänhantering i administrationskonsolen.
1. Välj domänkonfiguration, till exempel LDAP, och klicka på den. Du hittar alla skapade användare och grupper i katalogen. Om det behövs kan du skapa nya användare eller grupper.
   ![Domänhanteringssida](/help/forms/using/assets/domain-mgmt-page.png)
1. Klicka på Autentisering på den nya sidan och välj en autentiseringsprovider, till exempel LDAP.
1. Gå till sidan Domänhantering, välj LDAP och klicka på **Synkronisera nu** för att synkronisera katalogen med det autentiseringsschema som du har konfigurerat för AEM-åtkomst.
   ![Synkronisera ldap](/help/forms/using/assets/sync-ldap.png)
1. Gå till Användarhantering och klicka på Användare och grupper.
1. Sök efter användare eller grupper med deras namn, vilket visas i bilden nedan.
   ![Sök efter användargrupp](/help/forms/using/assets/search-user-group.png)
1. Tilldela rollerna till användarna eller grupperna efter behov.
   ![Tilldelning av användarroll](/help/forms/using/assets/user-role-assign.png)
