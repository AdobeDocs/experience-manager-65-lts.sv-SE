---
title: Säkerhetschecklista
description: Läs om de olika säkerhetsaspekterna när du konfigurerar och distribuerar AEM.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
docset: aem65
feature: Security
solution: Experience Manager, Experience Manager Sites
role: Admin,Developer
source-git-commit: 1d011ae238f213459c97e921740db10640935022
workflow-type: tm+mt
source-wordcount: '2958'
ht-degree: 0%

---

# Säkerhetschecklista {#security-checklist}

I det här avsnittet beskrivs olika åtgärder som du bör vidta för att se till att din AEM-installation är säker när du distribuerar den. Checklistan ska användas uppifrån och ned.

>[!NOTE]
>
>Ytterligare information finns också om de farligaste säkerhetshot som publicerats av [Open Web Application Security Project (OWASP)](https://owasp.org/www-project-top-ten/).

>[!NOTE]
>
>Det finns ytterligare [säkerhetsaspekter](/help/sites-developing/dev-guidelines-bestpractices.md#security-considerations) som kan användas i utvecklingsfasen.

## Viktiga säkerhetsåtgärder {#main-security-measures}

### Kör AEM i produktionsklar läge {#run-aem-in-production-ready-mode}

Mer information finns i [Köra AEM i produktionsklar läge](/help/sites-administering/production-ready.md).

### Aktivera HTTPS för transportlagersäkerhet {#enable-https-for-transport-layer-security}

Det är obligatoriskt att aktivera HTTPS-transportlagret på både författare- och publiceringsinstanser för att en säker instans ska kunna användas.

>[!NOTE]
>
>Mer information finns i avsnittet [Aktivera HTTP över SSL](/help/sites-administering/ssl-by-default.md).

### Installera snabbkorrigeringar för säkerhet {#install-security-hotfixes}

Kontrollera att du har installerat de senaste [säkerhetsuppdateringarna från Adobe](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html).

### Ändra standardlösenord för administratörskonton för AEM och OSGi Console {#change-default-passwords-for-the-aem-and-osgi-console-admin-accounts}

Adobe rekommenderar att du efter installationen ändrar lösenordet för de behöriga [**AEM** `admin`-kontona](#changing-the-aem-admin-password) (i alla instanser).

Dessa konton omfattar:

* AEM `admin`-kontot

  När du har ändrat lösenordet för AEM-administratörskontot använder du det nya lösenordet när du öppnar CRX.

* Lösenordet `admin` för OSGi-webbkonsolen

  Den här ändringen tillämpas även på det administratörskonto som används för att komma åt webbkonsolen, så använd samma lösenord när du kommer åt det.

De här två kontona använder separata autentiseringsuppgifter och det är viktigt att ha tydliga, starka lösenord för var och en av dem för att driftsättningen ska vara säker.

#### Ändra AEM-administratörslösenord {#changing-the-aem-admin-password}

Lösenordet för AEM-administratörskontot kan ändras via konsolen [Bevilja åtgärder - användare](/help/sites-administering/granite-user-group-admin.md).

Här kan du redigera `admin`-kontot och [ändra lösenordet](/help/sites-administering/granite-user-group-admin.md#changing-the-password-for-an-existing-user).

>[!NOTE]
>
>Om du ändrar administratörskontot ändras även OSGi-webbkonsolens konto. När du har ändrat administratörskontot bör du ändra OSGi-kontot till något annat.

#### Viktigt att ändra lösenordet för OSGi-webbkonsolen {#importance-of-changing-the-osgi-web-console-password}

Förutom AEM `admin`-kontot kan följande hända om du inte ändrar standardlösenordet för OSGi-webbkonsolen:

* Serverns exponering med ett standardlösenord vid start och avstängning (som kan ta minuter för stora servrar).
* Exponering av servern när databasen är avstängd/startad och OSGI körs.

Mer information om hur du ändrar lösenord för webbkonsolen finns i [Ändra administratörslösenordet för OSGi-webbkonsolen](/help/sites-administering/security-checklist.md#changing-the-osgi-web-console-admin-password) nedan.

#### Ändra administratörslösenordet för OSGi-webbkonsolen {#changing-the-osgi-web-console-admin-password}

Ändra lösenordet som används för att komma åt webbkonsolen. Använd en [OSGI-konfiguration](/help/sites-deploying/configuring-osgi.md) för att uppdatera följande egenskaper för **Apache Felix OSGi Management Console**:

* **Användarnamn** och **lösenord**, autentiseringsuppgifter för åtkomst till själva webbhanteringskonsolen för Apache Felix.
Lösenordet måste ändras *efter* den första installationen för att garantera instansens säkerhet.

>[!NOTE]
>
>Mer information om hur du konfigurerar OSGi-inställningar finns i [OSGI-konfiguration](/help/sites-deploying/configuring-osgi.md).

**Så här ändrar du administratörslösenordet för OSGi-webbkonsolen**:

1. Använd menyn **Verktyg**, **Åtgärder**, öppna **webbkonsolen** och gå till avsnittet **Konfiguration**.
Till exempel vid `<server>:<port>/system/console/configMgr`.
1. Navigera till och öppna posten för **Apache Felix OSGi Management Console**.
1. Ändra **användarnamnet** och **lösenordet**.

   ![chlimage_1-3](assets/chlimage_1-3.png)

1. Välj **Spara**.

### Implementera anpassad felhanterare {#implement-custom-error-handler}

Adobe rekommenderar att du definierar anpassade felhanterarsidor, särskilt för 404- och 500 HTTP Response-koder för att förhindra informationsexponering.

>[!NOTE]
>
>Mer information finns i [Så här skapar jag egna skript eller felhanterare](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/custom-error-page.html).

### Fullständig checklista för Dispatcher {#complete-dispatcher-security-checklist}

AEM Dispatcher är en viktig del av er infrastruktur. Adobe rekommenderar att du slutför [Dispatcher checklista](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/security-checklist.html).

>[!CAUTION]
>
>Med Dispatcher måste du inaktivera &quot;.form&quot;-väljaren.

## Verifieringssteg {#verification-steps}

### Konfigurera replikerings- och transportanvändare {#configure-replication-and-transport-users}

En standardinstallation av AEM anger `admin` som användare för transportautentiseringsuppgifter inom [replikeringsagenterna](/help/sites-deploying/replication.md) som standard. Administratörsanvändaren används också för att hämta replikeringen på författarsystemet.

Av säkerhetsskäl bör båda ändras för att återspegla det aktuella användningsfallet, med följande två aspekter i åtanke:

* **transportanvändaren** får inte vara administratörsanvändaren. I stället anger du en användare i publiceringssystemet som bara har behörighet till de relevanta delarna av publiceringssystemet och använder användarens inloggningsuppgifter för transporten.

  Du kan starta från den paketerade replikeringsmottagaren och konfigurera den här användarens åtkomsträttigheter så att de matchar din situation

* **replikeringsanvändaren** eller **agentens användar-ID** får inte vara administratörsanvändare, utan en användare som bara kan se innehåll som replikeras. Replikeringsanvändaren används för att samla in det innehåll som ska replikeras på författarsystemet innan det skickas till utgivaren.

### Kontrollera säkerhetshälsokontrollerna för instrumentpanelen för åtgärder {#check-the-operations-dashboard-security-health-checks}

AEM 6 introducerar den nya kontrollpanelen för åtgärder, som är avsedd att hjälpa systemansvariga att felsöka problem och övervaka en instans hälsa.

Kontrollpanelen innehåller också en samling säkerhetskontroller. Vi rekommenderar att du kontrollerar statusen för alla säkerhetshälsokontroller innan du publicerar med produktionsinstansen. Mer information finns i dokumentationen för [Operations Dashboard](/help/sites-administering/operations-dashboard.md).

### Kontrollera om exempelinnehåll finns {#check-if-example-content-is-present}

Allt exempelinnehåll och alla användare (till exempel Geometrixx-projektet och dess komponenter) ska avinstalleras och tas bort helt i ett produktionssystem innan det görs tillgängligt för allmänheten.

>[!NOTE]
>
>Exempelprogrammen `We.Retail` tas bort om den här instansen körs i [Produktionsklart läge](/help/sites-administering/production-ready.md). Om så inte är fallet kan du avinstallera exempelinnehållet genom att gå till Pakethanteraren och sedan söka efter och avinstallera alla `We.Retail`-paket.

Se [Arbeta med paket](package-manager.md).

### Kontrollera om CRX utvecklingspaket finns {#check-if-the-crx-development-bundles-are-present}

Dessa OSGi-utvecklingspaket bör avinstalleras både på författaren och publicera produktionssystem innan de blir tillgängliga.

* Stöd för Adobe CRXDE (com.adobe.granite.crxde-support)
* Adobe Granite CRX Explorer (com.adobe.granite.crx-explorer)
* Adobe Granite CRXDE Lite (com.adobe.granite.crxde-lite)

### Kontrollera om Sling-utvecklingspaketet finns {#check-if-the-sling-development-bundle-is-present}

[AEM Developer Tools](/help/sites-developing/aem-eclipse.md) distribuerar installationsprogrammet för Apache Sling Tooling (org.apache.sling.tooling.support.install).

Detta OSGi-paket bör avinstalleras både på författaren och publicera produktionssystem innan de blir tillgängliga.

### Skydda mot smidning av förfrågningar på olika webbplatser {#protect-against-cross-site-request-forgery}

#### CSRF Protection Framework {#the-csrf-protection-framework}

AEM 6.1 levereras med en mekanism som hjälper till att skydda mot attacker med attacker med smidning av korswebbplatser som kallas **CSRF Protection Framework**. Mer information om hur du använder den finns i [dokumentationen](/help/sites-developing/csrf-protection.md).

#### Sling Referer-filtret {#the-sling-referrer-filter}

Om du vill åtgärda kända säkerhetsproblem med CSRF (Cross-Site Request Forgery) i CRX WebDAV och Apache Sling lägger du till konfigurationer så att referensfiltret kan använda det.

Refererarfiltertjänsten är en OSGi-tjänst som gör att du kan konfigurera följande:

* vilka http-metoder som ska filtreras
* om en tom referensrubrik tillåts
* och en lista över servrar som ska tillåtas utöver servervärden.

  Som standard finns alla varianter av localhost och de värdnamn som servern är bunden till i listan.

Så här konfigurerar du referenspunktsfiltertjänsten:

1. Öppna Apache Felix-konsolen (**Configurations**) på:

   `https://<server>:<port_number>/system/console/configMgr`

1. Logga in som `admin`.
1. Välj:****

   `Apache Sling Referrer Filter`

1. I fältet `Allow Hosts` anger du alla värdar som tillåts som referent. Varje tävlingsbidrag måste ha blanketten

   &lt;protocol>://&lt;server>:&lt;port>

   Till exempel:

   * `https://allowed.server:80` tillåter alla begäranden från den här servern med den angivna porten.
   * Om du även vill tillåta https-begäranden måste du ange en andra rad.
   * Om du tillåter alla portar från den servern kan du använda `0` som portnummer.

1. Markera fältet `Allow Empty` om du vill tillåta tomma/saknade hänvisningsrubriker.

   >[!CAUTION]
   >
   >Adobe rekommenderar att du anger en referent när du använder kommandoradsverktyg som `cURL` i stället för att tillåta ett tomt värde eftersom det kan exponera systemet för CSRF-attacker.

1. Redigera de metoder som det här filtret använder för kontroller med fältet `Filter Methods`.

1. Klicka på **Spara** för att spara ändringarna.

### OSGI-inställningar {#osgi-settings}

Vissa OSGI-inställningar ställs in som standard för att underlätta felsökning av programmet. Ändra sådana inställningar för publiceringsinställningarna och författarens produktivitetsinstanser för att undvika att intern information läcker till allmänheten.

>[!NOTE]
>
>Alla inställningar nedan, förutom **Dagen CQ WCM Debug Filter**, täcks automatiskt av [Produktionsklart läge](/help/sites-administering/production-ready.md). Därför rekommenderar Adobe att du granskar alla inställningar innan du distribuerar instansen i en produktiv miljö.

För var och en av följande tjänster måste de angivna inställningarna ändras:

* [Adobe Granite HTML Library Manager](/help/sites-deploying/osgi-configuration-settings.md#day-cq-html-library-manager):

   * aktivera **Minify** (för att ta bort CRLF- och whitespace-tecken).
   * aktivera **Gzip** (om du vill tillåta att filer grupperas och nås med en begäran).
   * inaktivera **Felsök**
   * inaktivera **Timing**

* [Dag CQ WCM-felsökningsfilter](/help/sites-deploying/osgi-configuration-settings.md#day-cq-wcm-debug-filter):

   * avmarkera **Aktivera**

* [Dag CQ WCM-filter](/help/sites-deploying/osgi-configuration-settings.md):

   * endast vid publicering, ställ in **WCM-läge** till &quot;inaktiverat&quot;

* [Apache Sling JavaScript Handler](/help/sites-deploying/osgi-configuration-settings.md#apache-sling-javascript-handler):

   * inaktivera **Generera felsökningsinformation**

* [Apache Sling JSP Script Handler](/help/sites-deploying/osgi-configuration-settings.md#apache-sling-jsp-script-handler):

   * inaktivera **Generera felsökningsinformation**
   * inaktivera **Mappat innehåll**

Se [OSGi-konfigurationsinställningar](/help/sites-deploying/osgi-configuration-settings.md).

När du arbetar med AEM finns det flera metoder för att hantera konfigurationsinställningarna för sådana tjänster. Mer information och rekommenderade tillvägagångssätt finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

## Ytterligare läsningar {#further-readings}

### Minska DoS-attacker (Denial of Service) {#mitigate-denial-of-service-dos-attacks}

En denial of service-attack (DoS) är ett försök att göra en datorresurs otillgänglig för de avsedda användarna. Den här attacken utförs ofta genom att resursen överbelastas, till exempel:

* En flod av förfrågningar från en extern källa.
* En begäran om mer information än systemet kan leverera.

  Till exempel en JSON-representation av hela databasen.

* Genom att begära en innehållssida med ett obegränsat antal URL-adresser kan URL-adressen innehålla ett handtag, vissa väljare, ett tillägg och ett suffix, som alla kan ändras.

  `.../en.html` kan till exempel också begäras som:

   * `.../en.ExtensionDosAttack`
   * `.../en.SelectorDosAttack.html`
   * `.../en.html/SuffixDosAttack`

  Alla giltiga variationer (till exempel returnera ett `200`-svar och är konfigurerade att cachelagras) cachas av Dispatcher, vilket till slut leder till ett fullständigt filsystem och ingen tjänst för ytterligare begäranden.

Det finns många konfigurationspunkter för att förhindra sådana attacker, men endast de punkter som rör AEM behandlas här.

**Konfigurerar Sling för att förhindra DoS**

Sling är *innehållscentrerad*. Bearbetningen är inriktad på innehållet eftersom varje (HTTP) begäran mappas till innehåll i form av en JCR-resurs (en databasnod):

* Det första målet är den resurs (JCR-nod) som innehåller innehållet.
* För det andra finns återgivaren, eller skriptet, från resursegenskaperna med vissa delar av begäran (till exempel väljare och/eller tillägget).

Mer information finns i [Bearbetning av delningsbegäran](/help/sites-developing/the-basics.md#sling-request-processing).

Sling är en kraftfull och flexibel metod, men som alltid är det den flexibilitet som måste hanteras noggrant.

För att förhindra missbruk kan du göra följande:

1. Lägg in kontroller på programnivå. På grund av antalet möjliga variationer går det inte att konfigurera som standard.

   I ditt program bör du:

   * Kontrollera väljarna i programmet så att du *endast* kan hantera de explicita väljarna som behövs och returnera `404` för alla andra.
   * Förhindra utdata från ett obegränsat antal innehållsnoder.

1. Kontrollera konfigurationen av standardåtergivningsprogrammen, som kan vara ett problemområde.

   * JSON-renderaren omformar trädstrukturen över flera nivåer.

     Exempelvis begäran:

     `http://localhost:4502/.json`

     kan dumpa hela databasen i en JSON-representation vilket kan orsaka betydande serverproblem. Därför anger Sling en gräns för hur många resultat som får maximalt användas. Om du vill begränsa djupet i JSON-återgivningen anger du värdet för följande:

     **JSON-maxresultat** ( `json.maximumresults`)

     i konfigurationen för [Apache Sling GET Server](/help/sites-deploying/osgi-configuration-settings.md#apache-sling-get-servlet). När den här gränsen överskrids komprimeras återgivningen. Standardvärdet för Sling inom AEM är `1000`.

   * Som en förebyggande åtgärd bör du inaktivera andra standardåtergivare (HTML, oformaterad text, XML). Återigen genom att konfigurera [Apache Sling GET Server](/help/sites-deploying/osgi-configuration-settings.md#apache-sling-get-servlet).

   >[!CAUTION]
   >
   >Inaktivera inte JSON-renderaren eftersom den krävs för att AEM ska fungera normalt.

1. Använd en brandvägg för att filtrera åtkomsten till instansen.

   * Du måste använda en brandvägg på operativsystemnivå för att filtrera åtkomst till punkter i din instans, vilket kan leda till denial of service-attacker om den inte skyddas.

**Korrigera mot DoS på grund av att formulärväljare används**

>[!NOTE]
>
>Denna begränsning bör endast utföras i AEM-miljöer som inte använder Forms.

Eftersom AEM inte tillhandahåller körklara index för `FormChooserServlet` kan formulärväljare i frågor utlösa en kostsam databasövergång, vilket vanligen gör att AEM-instansen stannar. Formulärväljare kan identifieras med **&amp;ast;.form.&amp;ast;** sträng i frågor.

Du kan åtgärda det här problemet genom att göra följande:

1. Gå till webbkonsolen genom att peka din webbläsare till *https://&lt;serveradress>:&lt;serverport>/system/console/configMgr*

1. Sök efter **dagars CQ WCM-formulärväljarserver**
1. När du har klickat på posten inaktiverar du **Advanced Search Require** i följande fönster.

1. Klicka på **Spara**.

**Minskar mot DoS på grund av hämtningsservern**

Med standardservern för hämtning av resurser kan autentiserade användare skicka godtyckligt stora, samtidiga hämtningsbegäranden för att skapa ZIP-filer med resurser. Om du skapar stora ZIP-arkiv kan servern och nätverket överbelastas. För att minska en potentiell denial of service-risk (DoS) som orsakas av detta beteende är `AssetDownloadServlet` OSGi-komponenten inaktiverad som standard på [!DNL Experience Manager] -publiceringsinstansen. Det är aktiverat på författarinstansen [!DNL Experience Manager] som standard.

Om du inte behöver nedladdningsfunktionen kan du inaktivera servern för författare och publicera distributioner. Om din installation kräver att funktionen för hämtning av resurser är aktiverad finns mer information i [Hämta resurser från Adobe Experience Manager](/help/assets/download-assets-from-aem.md). Du kan också ange en maximal hämtningsgräns som din distribution kan stödja.

### Inaktivera WebDAV {#disable-webdav}

Inaktivera WebDAV både i skribent- och publiceringsmiljöer genom att stoppa rätt OSGi-paket.

1. Anslut till **Felix Management Console** som körs på:

   `https://<*host*>:<*port*>/system/console`

   Exempel: `http://localhost:4503/system/console/bundles`.

1. I listan med paket hittar du paketet med namnet:

   `Apache Sling Simple WebDAV Access to repositories (org.apache.sling.jcr.webdav)`

1. Om du vill stoppa det här paketet klickar du på stoppknappen i åtgärdskolumnen.

1. I paketlistan hittar du även här paketet:

   `Apache Sling DavEx Access to repositories (org.apache.sling.jcr.davex)`

1. Klicka på stoppknappen om du vill stoppa det här paketet.

   >[!NOTE]
   >
   >Du behöver inte starta om AEM.

### Verifiera att du inte avslöjar personligt identifierbar information i användarens hemsökväg {#verify-that-you-are-not-disclosing-personally-identifiable-information-in-the-users-home-path}

Det är viktigt att skydda dina användare genom att se till att du inte visar någon personligt identifierbar information i databasanvändarens hemsökväg.

Sedan AEM 6.1 har sättet som användar-ID-nodnamn (kallas även auktoriseringsbara) lagras på ändrats med en ny implementering av `AuthorizableNodeName`-gränssnittet. Det nya gränssnittet visar inte längre användar-ID:t i nodnamnet, utan genererar i stället ett slumpmässigt namn.

Ingen konfiguration måste utföras för att aktivera den eftersom det nu är standardsättet att generera auktoriserbara ID:n i AEM.

Även om det inte rekommenderas kan du inaktivera det om du behöver den gamla implementeringen för bakåtkompatibilitet med dina befintliga program. För att göra det måste du göra följande:

1. Gå till webbkonsolen och ta bort posten** org.apache.jackrabbit.oak.security.user.RandomAuthorizableNodeName** från egenskapen **requiredServicePids** i **Apache Jackrabbit Oak SecurityProvider**.

   Du kan också hitta Oak-säkerhetsprovidern genom att leta efter PID:t **org.apache.jackrabbit.oak.security.internal.SecurityProviderRegistration** i OSGi-konfigurationerna.

1. Ta bort **Apache Jackrabbit Oak Random Authorizable Node Name** OSGi-konfigurationen från webbkonsolen.

   För enklare sökning är PID:t för den här konfigurationen **org.apache.jackrabbit.oak.security.user.RandomAuthorizableNodeName**.

>[!NOTE]
>
>Mer information finns i Oak-dokumentationen om [generering av auktoriseringsbara nodnamn](https://jackrabbit.apache.org/oak/docs/security/user/authorizablenodename.html).

### Anonymt behörighetskontrollpaket {#anonymous-permission-hardening-package}

Som standard lagrar AEM systemmetadata, till exempel `jcr:createdBy` eller `jcr:lastModifiedBy` som nodegenskaper, bredvid regelbundet innehåll i databasen. Beroende på konfigurationen och åtkomstkontrollkonfigurationen kan detta i vissa fall leda till exponering av personligt identifierbar information (PII), till exempel när sådana noder återges som rå JSON eller XML.

Precis som alla databasdata förmedlas dessa egenskaper av Oak behörighetsstack. Åtkomsten till dem bör begränsas i enlighet med principen om minst privilegium.

Som stöd för detta tillhandahåller Adobe ett behörighetskontrollerande paket som kunderna kan bygga vidare på. Det fungerar genom att installera en &quot;deny&quot;-åtkomstkontrollpost i databasroten, vilket begränsar anonym åtkomst till vanliga systemegenskaper. Paketet kan [hämtas](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/helper/anonymous-permissions-pkg-0.1.2.zip) och installeras på alla versioner av AEM som stöds.

För att illustrera ändringarna kan vi jämföra nodegenskaperna som kan visas anonymt innan paketet installeras:

![Innan paketet installeras](/help/sites-administering/assets/before_resized.png)

med de som kan visas efter att paketet har installerats, där `jcr:createdBy` och `jcr:lastModifiedBy` inte är synliga:

![Efter installation av paketet](/help/sites-administering/assets/after_resized.png)

Mer information finns i versionsinformationen för paketet.

### Förhindra clickjacking {#prevent-clickjacking}

För att förhindra clickjacking rekommenderar Adobe att du konfigurerar webbservern så att HTTP-huvudet `X-FRAME-OPTIONS` anges till `SAMEORIGIN`.

Mer information om clickjacking finns på [OWASP-webbplatsen](https://www.owasp.org/index.php/Clickjacking).

### Se till att du replikerar krypteringsnycklar korrekt vid behov {#make-sure-you-properly-replicate-encryption-keys-when-needed}

Vissa AEM-funktioner och autentiseringsscheman kräver att du replikerar dina krypteringsnycklar i alla AEM-instanser.

Innan du gör det utförs nyckelreplikering på olika sätt i olika versioner, eftersom det sätt som nycklarna lagras på skiljer sig åt mellan 6.3 och äldre versioner.

Mer information finns nedan.

#### Replikeringsnycklar för AEM 6.3 {#replicating-keys-for-aem}

I äldre versioner lagrades replikeringsnycklarna i databasen, med början från AEM 6.3, men de lagras i filsystemet.

Om du vill replikera dina nycklar mellan instanser kopierar du dem från källinstansen till målinstansens plats i filsystemet.

Mer specifikt måste du göra följande:

1. Få tillgång till AEM-instansen - vanligtvis en författarinstans - som innehåller det nyckelmaterial som ska kopieras,
1. Leta reda på paketet com.adobe.granite.crypto.file i det lokala filsystemet. Under den här sökvägen:

   * `<author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle21`

   `bundle.info`-filen i varje mapp identifierar paketnamnet.

1. Navigera till datamappen. Till exempel:

   * `<author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle21/data`

1. Kopiera HMAC- och mallfilerna.
1. Gå sedan till den målinstans som du vill duplicera HMAC-nyckeln till och navigera till datamappen. Till exempel:

   * `<publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle21/data`

1. Klistra in de två filer som du kopierade tidigare.
1. Uppdatera Crypto Bundleom målinstansen redan körs.
1. Upprepa stegen ovan för alla förekomster som du vill replikera nyckeln till.

#### Replikeringsnycklar för AEM 6.2 och äldre versioner {#replicating-keys-for-aem-and-older-versions}

I AEM 6.2 och tidigare lagras nycklarna i databasen under noden `/etc/key`.

Det rekommenderade sättet att på ett säkert sätt replikera nycklarna mellan dina instanser är att bara replikera den här noden. Du kan selektivt replikera noder via CRXDE Lite:

1. Öppna CRXDE Lite genom att gå till *`https://&lt;serveraddress&gt;:4502/crx/de/index.jsp`*
1. Markera noden `/etc/key`.
1. Gå till fliken **Replikering**.
1. Tryck på knappen **Replikering**.

### Utför ett penetrationstest {#perform-a-penetration-test}

Adobe rekommenderar att du gör ett penetrationstest av din AEM-infrastruktur innan du börjar producera.

### Bästa praxis för utveckling {#development-best-practices}

Det är viktigt att ny utveckling följer [Bästa praxis för säkerhet](/help/sites-developing/security.md) för att säkerställa att AEM-miljön skyddas.
