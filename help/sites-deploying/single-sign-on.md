---
title: Enkel inloggning
description: Lär dig konfigurera enkel inloggning (SSO) för en instans av Adobe Experience Manager (AEM).
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring, Security
content-type: reference
feature: Security
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '723'
ht-degree: 0%

---

# Enkel inloggning {#single-sign-on}

Med enkel inloggning (SSO) kan en användare få åtkomst till flera system efter att ha angett inloggningsuppgifter (till exempel användarnamn och lösenord) en gång. Ett separat system (kallas betrodd autentiserare) utför autentiseringen och förser Experience Manager med inloggningsuppgifterna. Experience Manager kontrollerar och verkställer användarens åtkomstbehörigheter (d.v.s. avgör vilka resurser användaren har åtkomst till).

Tjänsten Hanterare för SSO-autentisering ( `com.adobe.granite.auth.sso.impl.SsoAuthenticationHandler`) bearbetar autentiseringsresultaten som den betrodda autentiseraren tillhandahåller. Hanteraren för SSO-autentisering söker efter en SSO-identifierare (SSID) som värde för ett specialattribut på följande platser i den här ordningen:

1. Begäranrubriker
1. Cookies
1. Begärandeparametrar

När ett värde hittas avslutas sökningen och det här värdet används.

Konfigurera följande två tjänster för att identifiera namnet på attributet som lagrar SSID:

* Inloggningsmodulen.
* Tjänsten för SSO-autentisering.

Ange samma attributnamn för båda tjänsterna. Attributet ingår i `SimpleCredentials` som tillhandahålls `Repository.login`. Attributets värde är irrelevant och ignoreras, och bara dess närvaro är viktig och verifierad.

## Konfigurerar enkel inloggning {#configuring-sso}

Om du vill konfigurera enkel inloggning för en AEM-instans konfigurerar du [autentiseringshanteraren för enkel inloggning](/help/sites-deploying/osgi-configuration-settings.md#adobegranitessoauthenticationhandler):

1. När du arbetar med AEM finns det flera metoder för att hantera konfigurationsinställningarna för sådana tjänster. Mer information och rekommenderade tillvägagångssätt finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

   För NTLM:

   * **Sökväg:** efter behov, till exempel `/`
   * **Rubriknamn**: `LOGON_USER`
   * **ID-format**: `^<DOMAIN>\\(.+)$`

     Där `<*DOMAIN*>` ersätts av namnet på din egen domän.

   För CoSign:

   * **Sökväg:** efter behov, till exempel `/`
   * **Rubriknamn**: remote_user
   * **ID-format:** AsIs

   För SiteMinder:

   * **Sökväg:** efter behov, till exempel `/`
   * **Rubriknamn:** SM_USER
   * **ID-format**: AsIs

1. Bekräfta att enkel inloggning fungerar som det ska, inklusive behörighet.

>[!CAUTION]
>
>Kontrollera att användare inte har åtkomst till AEM direkt om enkel inloggning har konfigurerats.
>
>Genom att kräva att användare går via en webbserver som kör SSO-systemets agent, säkerställs det att ingen användare direkt kan skicka en rubrik, cookie eller parameter som gör att användaren är betrodd av AEM, eftersom agenten filtrerar sådan information om den skickas utifrån.
>
>Alla användare som har direktåtkomst till din AEM-instans utan att gå via webbservern kan agera som alla användare genom att skicka rubriken, cookien eller parametern om namnen är kända.
>
>Se även till att du bara konfigurerar den som krävs för SSO-konfigurationen för rubriker, cookies och begär parameternamn.
>

>[!NOTE]
>
>Enkel inloggning används ofta med [LDAP](/help/sites-administering/ldap-config.md).

>[!NOTE]
>
>Om du även använder [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) med Microsoft® Internet Information Server (IIS) krävs ytterligare konfiguration i:
>
>* `disp_iis.ini`
>* IIS
>
>I `disp_iis.ini`-uppsättningen:
>(Mer information finns i [installera Dispatcher med Microsoft® Internet Information Server](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html#microsoft-internet-information-server))
>
>* `servervariables=1` (vidarebefordrar IIS-servervariabler som begärandehuvuden till fjärrinstansen)
>* `replaceauthorization=1` (ersätter en rubrik med namnet &quot;Authorization&quot;, annan än &quot;Basic&quot;, med motsvarande &quot;Basic&quot;)
>
>I IIS:
>
>* inaktivera **anonym åtkomst**
>
>* aktivera **integrerad Windows-autentisering**
>

Du kan se vilken autentiseringshanterare som tillämpas på valfri del av innehållsträdet med alternativet **Autentiserare** i Felix Console, till exempel:

`http://localhost:4502/system/console/slingauth`

Hanteraren som bäst matchar banan efterfrågas först. Om du till exempel konfigurerar handler-A för sökvägen `/` och handler-B för sökvägen `/content` kommer en begäran till `/content/mypage.html` att fråga handler-B först.

![screen_shot_2012-02-15at21006pm](assets/screen_shot_2012-02-15at21006pm.png)

### Exempel {#example}

För en cookie-begäran (med URL `http://localhost:4502/libs/wcm/content/siteadmin.html`):

```xml
GET /libs/cq/core/content/welcome.html HTTP/1.1
Host: localhost:4502
Cookie: TestCookie=admin
```

Använda följande konfiguration:

* **Sökväg**: `/`

* **Rubriknamn**: `TestHeader`

* **Cookie-namn**: `TestCookie`

* **Parameternamn**: `TestParameter`

* **ID-format**: `AsIs`

Svaret skulle vara:

```xml
HTTP/1.1 200 OK
Connection: Keep-Alive
Server: Day-Servlet-Engine/4.1.24
Content-Type: text/html;charset=utf-8
Date: Thu, 23 Aug 2012 09:58:39 GMT
Transfer-Encoding: chunked

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "https://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <title>Welcome to Adobe&reg; CQ5</title>
....
```

Detta fungerar även om du begär:
`http://localhost:4502/libs/cq/core/content/welcome.html?TestParameter=admin`

Du kan också använda följande curl-kommando för att skicka `TestHeader`-rubriken till `admin:`
`curl -D - -H "TestHeader: admin" http://localhost:4502/libs/cq/core/content/welcome.html`

>[!NOTE]
>
>När du använder parametern request i en webbläsare visas bara en del av HTML - utan CSS. Detta beror på att alla förfrågningar från HTML görs utan parametern request.

## Ta bort AEM-utloggningslänkar {#removing-aem-sign-out-links}

När du använder enkel inloggning hanteras inloggning och utloggning externt, så att AEM egna utloggningslänkar inte längre kan användas och bör tas bort.

Utloggningslänken på välkomstskärmen kan tas bort med följande steg.

1. Överlägg `/libs/cq/core/components/welcome/welcome.jsp` till `/apps/cq/core/components/welcome/welcome.jsp`
1. ta bort följande del från jsp.

   `<a href="#" onclick="signout('<%= request.getContextPath() %>');" class="signout"><%= i18n.get("sign out", "welcome screen") %>`

Så här tar du bort den utloggningslänk som är tillgänglig på användarens personliga meny i det övre högra hörnet:

1. Överlägg `/libs/cq/ui/widgets/source/widgets/UserInfo.js` till `/apps/cq/ui/widgets/source/widgets/UserInfo.js`

1. Ta bort följande del från filen:

   ```
   menu.addMenuItem({
       "text":CQ.I18n.getMessage("Sign out"),
       "cls": "cq-userinfo-logout",
       "handler": this.logout
   });
   menu.addSeparator();
   ```
