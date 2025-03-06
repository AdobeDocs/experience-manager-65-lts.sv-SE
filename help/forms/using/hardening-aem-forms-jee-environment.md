---
title: Förbättra din AEM Forms på JEE-miljö
description: Lär dig en mängd olika säkerhetsinställningar för att förbättra säkerheten för AEM Forms på JEE som körs i ett intranät.
content-type: reference
topic-tags: Security
products: SG_EXPERIENCEMANAGER/6.4
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Document Security,Adaptive Forms
exl-id: 3de38e4d-6a12-470e-aded-7eb75a9cdcd8
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '7605'
ht-degree: 0%

---

# Förbättra din AEM Forms på JEE-miljö {#hardening-your-aem-forms-on-jee-environment}

Lär dig en mängd olika säkerhetsinställningar för att förbättra säkerheten för AEM Forms på JEE som körs i ett intranät.

Artikeln beskriver rekommendationer och bästa praxis för att skydda servrar som kör AEM Forms på JEE. Det här är inte ett omfattande dokument för värdskydd för ditt operativsystem och dina programservrar. I den här artikeln beskrivs i stället en rad olika säkerhetsinställningar som du bör implementera för att förbättra säkerheten för AEM Forms på JEE som körs i ett intranät för företag. För att säkerställa att AEM Forms på JEE-programservrar förblir säkra bör du även implementera säkerhetsövervakning, identifiering och svarsprocedurer.

Artikeln beskriver härdningstekniker som ska användas under följande faser under installations- och konfigurationscykeln:

* **Förinstallation:** Använd dessa tekniker innan du installerar AEM Forms på JEE.
* **Installation:** Använd de här teknikerna under installationen av AEM Forms i JEE.
* **Efter installation:** Använd dessa tekniker efter installationen och regelbundet därefter.

AEM Forms på JEE är mycket anpassningsbart och kan fungera i många olika miljöer. Vissa av rekommendationerna kanske inte passar organisationens behov.

## Förinstallation {#preinstallation}

Innan du installerar AEM Forms på JEE kan du använda säkerhetslösningar på nätverkslagret och operativsystemet. I det här avsnittet beskrivs några problem och rekommendationer ges för att minska säkerhetsluckor i dessa områden.

**Installation och konfiguration på UNIX och Linux**

Du bör inte installera eller konfigurera AEM Forms på JEE med ett rotskal. Som standard installeras filer under katalogen /opt och den användare som utför installationen behöver alla filbehörigheter under /opt. Alternativt kan en installation utföras under en enskild användares /användarkatalog där användaren redan har alla filbehörigheter.

**Installation och konfiguration i Windows**

Du bör utföra installationen i Windows som administratör om du installerar AEM Forms på JEE på JBoss med hjälp av körningsmetoden eller om du installerar PDF Generator. När du installerar PDF Generator i Windows med inbyggt programstöd måste du köra installationen som samma Windows-användare som installerade Microsoft Office. Mer information om installationsprivilegier finns i dokumentet* Installera och distribuera AEM Forms på JEE* för programservern.

### Säkerhet för nätverkslager {#network-layer-security}

Säkerhetsluckor för nätverk är bland de första hoten mot alla Internetanslutna eller intranätriktade programservrar. I det här avsnittet beskrivs processen att härja värdar i nätverket mot dessa sårbarheter. Den behandlar nätverkssegmentering, TCP/IP-stackhärdning (Transmission Control Protocol/Internet Protocol) och användning av brandväggar för värdskydd.

I följande tabell beskrivs vanliga processer som minskar säkerhetsluckorna i nätverket.

<table> 
 <thead> 
  <tr> 
   <th><p>Problem</p> </th> 
   <th><p>Beskrivning</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>DMZ (Demilitarized Zone)</p> </td> 
   <td><p>Driftsätt blankettservrar inom en demilitariserad zon (DMZ). Segmentering bör finnas på minst två nivåer med den programserver som används för att köra AEM Forms på JEE bakom den inre brandväggen. Skilj det externa nätverket från det DMZ som innehåller webbservrarna, som i sin tur måste separeras från det interna nätverket. Använd brandväggar för att implementera separationslagren. Kategorisera och kontrollera den trafik som passerar genom varje nätverkslager för att säkerställa att endast absolut minimum av nödvändiga data tillåts.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Privata IP-adresser</p> </td> 
   <td><p>Använd NAT (Network Address Translation) med privata RFC 1918-IP-adresser på AEM Forms-programserver. Tilldela privata IP-adresser (10.0.0.0/8, 172.16.0.0/12 och 192.168.0.0/16) för att göra det svårare för en angripare att dirigera trafik till och från en NAT:s interna värd via Internet.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Brandväggar</p> </td> 
   <td><p>Använd följande kriterier för att välja en brandväggslösning:</p> 
    <ul> 
     <li><p>Implementera brandväggar som stöder proxyservrar och/eller <em>tillståndskänslig inspektion</em> i stället för enkla paketfilterlösningar.</p> </li> 
     <li><p>Använd en brandvägg som har stöd för <em>Neka alla tjänster förutom de som uttryckligen tillåts</em> säkerhetsparadigmer.</p> </li> 
     <li><p>Implementera en brandväggslösning som är dubbel-homed eller multihomed. Arkitekturen ger den högsta säkerhetsnivån och hjälper till att förhindra att obehöriga kringgår brandväggens säkerhet.</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>Databasportar</p> </td> 
   <td><p>Använd inte standardlyssningsportar för databaser (MySQL - 3306, Oracle - 1521, MS SQL - 1433). Mer information om hur du ändrar databasportar finns i dokumentationen för databasen.</p> <p>Om du använder en annan databasport påverkas den övergripande AEM Forms för JEE-konfigurationen. Om du ändrar standardportar måste du göra motsvarande ändringar i andra konfigurationsområden, till exempel datakällorna för AEM Forms på JEE.</p> <p>Information om hur du konfigurerar datakällor i AEM Forms på JEE finns i Installera och uppgradera AEM Forms på JEE eller Uppgradera till AEM Forms på JEE för programservern i <a href="/help/forms/using/introduction-aem-forms.md" target="_blank">AEM Forms användarhandbok</a>.</p> </td> 
  </tr> 
 </tbody> 
</table>

### Operativsystemsäkerhet {#operating-system-security}

I följande tabell beskrivs några möjliga strategier för att minimera säkerhetsbrister som finns i operativsystemet.

<table> 
 <thead> 
  <tr> 
   <th><p>Problem</p></th> 
   <th><p>Beskrivning</p></th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>Säkerhetsuppdateringar</p></td> 
   <td><p>Det finns en ökad risk för att en obehörig användare får åtkomst till programservern om leverantörens säkerhetsuppdateringar och uppgraderingar inte tillämpas i tid. Testa säkerhetsuppdateringar innan du använder dem på produktionsservrar.</p><p>Du kan också skapa principer och procedurer för att regelbundet kontrollera och installera korrigeringsfiler.</p></td> 
  </tr> 
  <tr> 
   <td><p>Virusskydd</p></td> 
   <td><p>Virusskannrar kan identifiera infekterade filer genom att skanna efter en signatur eller genom att titta efter ovanligt beteende. Skannrar sparar sina virussignaturer i en fil som vanligtvis lagras på den lokala hårddisken. Eftersom nya virus upptäcks ofta bör du ofta uppdatera den här filen för virusskannern för att identifiera alla aktuella virus.</p></td> 
  </tr> 
  <tr> 
   <td><p>NTP (Network Time Protocol)</p></td> 
   <td><p>För kriminalteknisk analys bör du hålla rätt tid på formulärservrarna. Använd NTP för att synkronisera tiden på alla system som är anslutna direkt till Internet.</p></td> 
  </tr> 
 </tbody> 
</table>

Mer säkerhetsinformation för ditt operativsystem finns i [&quot;Säkerhetsinformation för operativsystem&quot;](https://helpx.adobe.com/aem-forms/6-1/hardening-security/general-security-considerations.html#operating_system_security_information).

## Installation {#installation}

I det här avsnittet beskrivs tekniker som du kan använda under AEM Forms-installationen för att minska säkerhetsluckorna. I vissa fall använder dessa tekniker alternativ som ingår i installationsprocessen. I följande tabell beskrivs dessa tekniker.

<table> 
 <thead> 
  <tr> 
   <th><p>Problem</p> </th> 
   <th><p>Beskrivning</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>Behörighet</p> </td> 
   <td><p>Använd det minsta antalet privilegier som krävs för att installera programvaran. Logga in på datorn med ett konto som inte finns i gruppen Administratörer. I Windows kan du använda kommandot Kör som för att köra AEM Forms på JEE-installationsprogrammet som en administrativ användare. På UNIX- och Linux-system använder du ett kommando som <code>sudo</code> för att installera programvaran.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Programvara</p> </td> 
   <td><p>Hämta eller kör inte AEM Forms på JEE från källor som inte är betrodda.</p> <p>Skadliga program kan innehålla kod som bryter mot säkerheten på flera sätt, t.ex. datastöld, ändring och borttagning samt denial of service. Installera AEM Forms på JEE från Adobe DVD eller endast från en betrodd källa.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Diskpartitioner</p> </td> 
   <td><p>Placera AEM Forms på JEE på en dedikerad diskpartition. Disksegmentering är en process som sparar specifika data på servern på separata fysiska diskar för ökad säkerhet. Genom att ordna data på det här sättet minskar risken för kataloggenombrottsattacker. Planera att skapa en partition som är skild från den systempartition där du kan installera AEM Forms på JEE-innehållskatalogen. (I Windows innehåller systempartitionen katalogen system32 eller startpartitionen.)</p> </td> 
  </tr> 
  <tr> 
   <td><p>Komponenter</p> </td> 
   <td><p>Utvärdera befintliga tjänster och inaktivera eller avinstallera de som inte behövs. Installera inte onödiga komponenter och tjänster.</p> <p>Standardinstallationen av en programserver kan innehålla tjänster som du inte behöver. Du bör inaktivera alla onödiga tjänster före distributionen för att minimera ingångspunkter för en attack. På JBoss kan du till exempel kommentera bort onödiga tjänster i META-INF/jboss-service.xml.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Domänöverskridande principfil</p> </td> 
   <td><p>Förekomsten av en <code>crossdomain.xml</code>-fil på servern kan omedelbart försvaga den servern. Vi rekommenderar att du gör listan över domäner så restriktiv som möjligt. Placera inte filen <code>crossdomain.xml</code> som användes under utvecklingen i produktion när du använder stödlinjerna <em>(borttagen) </em>. För en guide som använder webbtjänster behövs ingen <code>crossdomain.xml</code>-fil alls om tjänsten finns på samma server som guiden finns på. Men om tjänsten finns på en annan server, eller om kluster ingår, behövs en <code>crossdomain.xml</code>-fil. Mer information om filen crossdomain.xml finns i <a href="https://kb2.adobe.com/cps/142/tn_14213.html">https://kb2.adobe.com/cps/142/tn_14213.html</a>.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Säkerhetsinställningar för operativsystem</p> </td> 
   <td><p>Om du behöver använda 192-bitars eller 256-bitars XML-kryptering på Solaris-plattformar måste du installera <code>pkcs11_softtoken_extra.so</code> i stället för <code>pkcs11_softtoken.so</code>.</p> </td> 
  </tr> 
 </tbody> 
</table>

## Steg efter installation {#post-installation-steps}

När du har installerat AEM Forms på JEE är det viktigt att regelbundet upprätthålla miljön ur ett säkerhetsperspektiv.

I följande avsnitt beskrivs i detalj de olika åtgärder som rekommenderas för att skydda den distribuerade Forms Server.

### AEM Forms säkerhet {#aem-forms-security}

Följande rekommenderade inställningar gäller för AEM Forms på JEE-servern utanför det administrativa webbprogrammet. Om du vill minska säkerhetsriskerna för servern ska du tillämpa de här inställningarna omedelbart efter att du har installerat AEM Forms på JEE.

**Säkerhetsuppdateringar**

Det finns en ökad risk för att en obehörig användare får åtkomst till programservern om leverantörens säkerhetsuppdateringar och uppgraderingar inte tillämpas i tid. Testa säkerhetsuppdateringarna innan du använder dem på produktionsservrar för att säkerställa kompatibilitet och tillgänglighet för program. Du kan också skapa principer och procedurer för att regelbundet kontrollera och installera korrigeringsfiler. AEM Forms på JEE-uppdateringar finns på Enterprise-produktnedladdningsplatsen.

**Tjänstkonton (endast JBoss-körningsnyckel i Windows)**

AEM Forms på JEE installerar som standard en tjänst med hjälp av kontot LocalSystem. Det inbyggda LocalSystem-användarkontot har hög tillgänglighet och ingår i gruppen Administratörer. Om en arbetarprocessidentitet körs som ett LocalSystem-användarkonto har den arbetsprocessen fullständig åtkomst till hela systemet.

Följ dessa anvisningar för att köra den programserver där AEM Forms on JEE distribueras med ett specifikt icke-administrativt konto:

1. I Microsoft Management Console (MMC) skapar du en lokal användare som Forms Server-tjänsten kan logga in som:

   * Välj **Användaren kan inte ändra lösenordet**.
   * På fliken **Medlem i** kontrollerar du att gruppen **Användare** visas.

   >[!NOTE]
   >
   >Du kan inte ändra den här inställningen för PDF Generator.

1. Välj **Start** > **Inställningar** > **Administrationsverktyg** > **Tjänster**.
1. Dubbelklicka på JBoss för AEM Forms på JEE och stoppa tjänsten.
1. På fliken **Logga in** väljer du **Det här kontot**, bläddrar efter det användarkonto du har skapat och anger lösenordet för kontot.
1. Öppna **Lokala säkerhetsinställningar** i MMC och välj **Lokala principer** > **Tilldelning av användarrättigheter**.
1. Tilldela följande behörigheter till användarkontot som Forms Server körs under:

   * Neka inloggning via Terminal Services
   * Neka lokal inloggning
   * Logga in som tjänst (bör vara inställd)

1. Ge det nya användarkontot behörighet att ändra i följande kataloger:
   * **GDS-katalog (Global Document Storage)**: Platsen för GDS-katalogen konfigureras manuellt under AEM Forms-installationsprocessen. Om platsinställningen är tom under installationen blir platsen som standard en katalog under programserverinstallationen på `[JBoss root]/server/[type]/svcnative/DocumentStorage`
   * **CRX-Databaskatalog**: Standardplatsen är `[AEM-Forms-installation-location]\crx-repository`
   * **AEM Forms temporära kataloger**:
      * (Windows) TMP- eller TEMP-sökväg som angetts i miljövariablerna
      * (AIX, Linux eller Solaris) Inloggad användares arbetskatalog
På UNIX-baserade system kan en icke-rotanvändare använda följande katalog som tillfällig katalog:
      * (Linux) /var/tmp eller /usr/tmp
      * (AIX) /tmp eller /usr/tmp
      * (Solaris) /var/tmp eller /usr/tmp
1. Ge det nya användarkontot skrivbehörighet i följande kataloger:
   * [JBoss-katalog]\fristående\distribution
   * [JBoss-katalog]\fristående\
   * [JBoss-directory]\bin\

   >[!NOTE]
   >
   >Standardinstallationsplatsen för JBoss Application Server:
   >
   >* Windows: C:\Adobe\Adobe_Experience_Manager_Forms\jboss
   >* Linux: /opt/jboss/

1. Starta programservern.

**Inaktiverar Configuration Manager-startservern**

Configuration Manager använde en serverlet som distribuerats på programservern för att starta AEM Forms på JEE-databasen. Eftersom Configuration Manager har åtkomst till den här servern innan konfigurationen är klar, har åtkomst till den inte skyddats för behöriga användare, och den bör inaktiveras efter att du har använt Configuration Manager för att konfigurera AEM Forms på JEE.

1. Zippa upp filen adobe-livecycle-[appserver].ear.
1. Öppna filen META-INF/application.xml.
1. Sök efter avsnittet adobe-bootstrapper.war:

   ```java
   <!-- bootstrapper start --> 
   <module id="WebApp_adobe_bootstrapper"> 
       <web> 
           <web-uri>adobe-bootstrapper.war</web-uri> 
           <context-root>/adobe-bootstrapper</context-root> 
       </web> 
   </module> 
   <module id="WebApp_adobe_lcm_bootstrapper_redirector"> 
       <web> 
           <web-uri>adobe-lcm-bootstrapper-redirector.war</web-uri> 
           <context-root>/adobe-lcm-bootstrapper</context-root> 
       </web> 
   </module> 
   <!-- bootstrapper end-->
   ```

1. Stoppa AEM Forms-servern.
1. Kommentera adobe-bootstrapper.war och katalogen adobe-lcm-bootstrapper-redirectory. Krigsmoduler enligt följande:

   ```java
   <!-- bootstrapper start --> 
   <!-- 
   <module id="WebApp_adobe_bootstrapper"> 
       <web> 
           <web-uri>adobe-bootstrapper.war</web-uri> 
           <context-root>/adobe-bootstrapper</context-root> 
       </web> 
   </module> 
   <module id="WebApp_adobe_lcm_bootstrapper_redirector"> 
       <web> 
           <web-uri>adobe-lcm-bootstrapper-redirector.war</web-uri> 
           <context-root>/adobe-lcm-bootstrapper</context-root> 
       </web> 
   </module> 
   --> 
   <!-- bootstrapper end-->
   ```

1. Spara och stäng META-INF/application.xml.
1. Zippa upp EAR-filen och distribuera den på nytt till programservern.
1. Starta AEM Forms-servern.
1. Skriv URL:en nedan i en webbläsare för att testa ändringen och se till att den inte längre fungerar.

   https://&lt;localhost>:/adobe-bootstrapper/bootstrap

**Låsa fjärråtkomst till Trust Store**

Med Configuration Manager kan du överföra autentiseringsuppgifter för Acrobat Reader DC-tillägg till AEM Forms på JEE-förtroendearkivet. Detta innebär att åtkomst till pålitlighetslagerautentiseringstjänsten via fjärrprotokoll (SOAP och EJB) har aktiverats som standard. Den här åtkomsten behövs inte längre när du har överfört rättighetsinformationen med Configuration Manager eller om du bestämmer dig för att använda administrationskonsolen senare för att hantera autentiseringsuppgifter.

Du kan inaktivera fjärråtkomst till alla Trust Store-tjänster genom att följa stegen i avsnittet [Inaktiverar icke nödvändig fjärråtkomst till tjänster](https://helpx.adobe.com/aem-forms/6-1/hardening-security/configuring-secure-administration-settings-aem.html#disabling_non_essential_remote_access_to_services).

**Inaktivera all icke nödvändig anonym åtkomst**

Vissa Forms Server-tjänster har åtgärder som kan anropas av en anonym anropare. Om anonym åtkomst till de här tjänsterna inte krävs inaktiverar du den genom att följa stegen i [Inaktiverar onödvändig anonym åtkomst till tjänster](https://helpx.adobe.com/aem-forms/6-1/hardening-security/configuring-secure-administration-settings-aem.html#disabling_non_essential_anonymous_access_to_services).

#### Ändra standardadministratörslösenordet {#change-the-default-administrator-password}

När AEM Forms on JEE är installerat konfigureras ett standardanvändarkonto för superadministratör/inloggnings-id-administratör med standardlösenordet *password*. Du bör omedelbart ändra det här lösenordet med Configuration Manager.

1. Skriv följande URL i en webbläsare:

   ```java
   https://[host name]:[port]/adminui
   ```

   Standardportnumret är något av följande:

   **JBoss:** 8080

   **WebLogic-server:** 7001

   **WebSphere:** 9080.

1. Skriv `administrator` i fältet **Användarnamn** och `password` i fältet **Lösenord**.
1. Klicka på **Inställningar** > **Användarhantering** > **Användare och grupper**.
1. Skriv `administrator` i fältet **Sök** och klicka på **Sök**.
1. Klicka på **Superadministratör** i listan över användare.
1. Klicka på **Ändra lösenord** på sidan Redigera användare.
1. Ange det nya lösenordet och klicka på **Spara**.

Du bör dessutom ändra standardlösenordet för CRX Administrator genom att utföra följande steg:

1. Logga in på `https://[server]:[port]/lc/libs/granite/security/content/useradmin.html` med standardanvändarnamnet/standardlösenordet.
1. Skriv Administratör i sökfältet och klicka på **Gå**.
1. Välj **Administratör** i sökresultatet och klicka på ikonen **Redigera** längst ned till höger i användargränssnittet.
1. Ange det nya lösenordet i fältet **Nytt lösenord** och det gamla lösenordet i fältet **Ditt lösenord**.
1. Klicka på ikonen Spara längst ned till höger i användargränssnittet.

#### Inaktivera WSDL-generering {#disable-wsdl-generation}

WSDL-generering (Web Service Definition Language) ska endast aktiveras för utvecklingsmiljöer där WSDL-generering används av utvecklare för att skapa klientprogram. Du kan välja att inaktivera WSDL-generering i en produktionsmiljö för att undvika att visa tjänstens interna information.

1. Skriv följande URL i en webbläsare:

   ```java
   https://[host name]:[port]/adminui
   ```

1. Välj **Inställningar > Systeminställningar > Konfigurationer**.
1. Avmarkera **Aktivera WSDL** och välj sedan **OK**.

### Programserversäkerhet {#application-server-security}

I följande tabell beskrivs några tekniker för att skydda programservern efter att AEM Forms on JEE-programmet har installerats.

<table> 
 <thead> 
  <tr> 
   <th><p>Problem</p> </th> 
   <th><p>Beskrivning</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>Administrationskonsol för programserver</p> </td> 
   <td><p>När du har installerat, konfigurerat och distribuerat AEM Forms på JEE på programservern bör du inaktivera åtkomsten till programserverns administrationskonsoler. Mer information finns i programserverns dokumentation.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Inställningar för programserverns cookie</p> </td> 
   <td><p>Programcookies styrs av programservern. När du distribuerar programmet kan programserveradministratören ange cookie-inställningar på en server- eller programspecifik basis. Som standard prioriteras serverinställningarna.</p> <p>Alla sessionscookies som genereras av programservern ska innehålla attributet <code>HttpOnly</code>. Om du till exempel använder JBoss-programservern kan du ändra SessionCookie-elementet till <code>httpOnly="true"</code> i <code>WEB-INF/web.xml</code>-filen.</p> <p>Du kan begränsa vilka cookies som ska skickas med enbart HTTPS. Därför skickas de inte okrypterade via HTTP. Programserveradministratörer bör aktivera säkra cookies för servern globalt. Om du till exempel använder JBoss-programservern kan du ändra anslutningselementet till <code>secure=true</code> i <code>server.xml</code>-filen.</p> <p>Mer information om inställningar för cookies finns i dokumentationen för programservern.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Katalogbläddring</p> </td> 
   <td><p>När någon begär en sida som inte finns eller begär namnet på en direktör (begärandesträngen avslutas med ett snedstreck (/)), ska programservern inte returnera innehållet i den katalogen. Du kan förhindra detta genom att inaktivera katalogbläddring på programservern. Du bör göra detta för administrationskonsolprogrammet och för andra program som körs på servern.</p> <p>För JBoss anger du värdet för listings-initieringsparametern för egenskapen <code>DefaultServlet</code> till <code>false</code> i filen web.xml, vilket visas i följande exempel:</p> <p>&lt;servlet&gt;</p> <p>&lt;servlet-name&gt;standard&lt;/servlet-name&gt;</p> <p>&lt;servlet-class&gt;</p> <p>org.apache.catalina.servlets.DefaultServlet</p> <p>&lt;/servlet-class&gt;</p> <p>&lt;init-param&gt;</p> <p>&lt;param-name&gt;listor&lt;/param-name&gt;</p> <p>&lt;param-value&gt;false&lt;/param-value&gt;</p> <p>&lt;/init-param&gt;</p> <p>&lt;load-on-startup&gt;1&lt;/load-on-startup&gt;</p> <p>&lt;/servlet&gt;</p> <p>För WebSphere anger du egenskapen <code>directoryBrowsingEnabled</code> i filen ibm-web-ext.xmi till <code>false</code>.</p> <p>För WebLogic anger du egenskaperna för indexkataloger i filen weblogic.xml till <code>false</code>, vilket visas i det här exemplet:</p> <p>&lt;container-descriptor&gt;</p> <p>&lt;index-directory-enabled&gt;false</p> <p>&lt;/index-directory-enabled&gt;</p> <p>&lt;/container-descriptor&gt;</p> </td> 
  </tr> 
 </tbody> 
</table>

### Databassäkerhet {#database-security}

När du skyddar din databas bör du implementera de åtgärder som beskrivs av din databasleverantör. Du bör tilldela en databasanvändare med de lägsta databasbehörigheter som krävs och som AEM Forms tillåter för JEE. Använd till exempel inte ett konto med databasadministratörsbehörighet.

I Oracle behöver det databaskonto du använder bara behörigheterna CONNECT, RESOURCE och CREATE VIEW. Liknande krav för andra databaser finns i [Förbereder installation av AEM Forms på JEE (Single Server)](https://www.adobe.com/go/learn_aemforms_prepareInstallsingle_64).

#### Konfigurera integrerad säkerhet för SQL Server i Windows för JBoss {#configuring-integrated-security-for-sql-server-on-windows-for-jboss}

1. Ändra [JBOSS_HOME]\\standalone\configuration\lc_{datasource.xml} för att lägga till `integratedSecurity=true` i anslutnings-URL:en, vilket visas i det här exemplet:

   ```java
    jdbc:sqlserver://<serverhost>:<port>;databaseName=<dbname>;integratedSecurity=true
   ```

1. Lägg till filen sqljdbc_auth.dll i Windows-systemsökvägen på den dator som kör programservern. Filen sqljdbc_auth.dll finns med drivrutinsinstallationen för Microsoft SQL JDBC 6.2.1.0.
1. Ändra egenskapen JBoss Windows-tjänst (JBoss för AEM Forms på JEE) för inloggning som från det lokala systemet till ett inloggningskonto som har AEM Forms-databas och en minimiuppsättning behörigheter. Om du kör JBoss från kommandoraden i stället för som en Windows-tjänst behöver du inte utföra det här steget.
1. Ange säkerhet för SQL Server från läget **Blandad** till **Endast Windows-autentisering**.

#### Konfigurera integrerad säkerhet för SQL Server i Windows för WebLogic {#configuring-integrated-security-for-sql-server-on-windows-for-weblogic}

1. Starta administrationskonsolen för WebLogic Server genom att skriva följande URL på URL-raden i en webbläsare:

   ```java
   https://[host name]:7001/console
   ```

1. Klicka på **Lås och redigera** under Ändringscenter.
1. Under Domänstruktur klickar du på *[base_domain]* > **Tjänster** > **JDBC** > **Datakällor** och sedan på **IDP_DS** i den högra rutan.
1. På nästa skärm klickar du på fliken **Konfiguration** på fliken **Anslutningspool** och skriver `integratedSecurity=true` i rutan **Egenskaper** .
1. Under Domänstruktur klickar du på **[base_domain]** > **Tjänster** > **JDBC** > **Datakällor** och sedan på **RM_DS** i den högra rutan.
1. På nästa skärm klickar du på fliken **Konfiguration** på fliken **Anslutningspool** och skriver `integratedSecurity=true` i rutan **Egenskaper** .
1. Lägg till filen sqljdbc_auth.dll i Windows-systemsökvägen på den dator som kör programservern. Filen sqljdbc_auth.dll finns med drivrutinsinstallationen för Microsoft SQL JDBC 6.2.1.0.
1. Ange säkerhet för SQL Server från läget **Blandad** till **Endast Windows-autentisering**.

#### Konfigurera integrerad säkerhet för SQL Server i Windows för WebSphere {#configuring-integrated-security-for-sql-server-on-windows-for-websphere}

På WebSphere kan du bara konfigurera integrerad säkerhet när du använder en extern JDBC-drivrutin för SQL Server, inte den JDBC-drivrutin för SQL Server som är inbäddad med WebSphere.

1. Logga in på administrationskonsolen för WebSphere.
1. Klicka på **Resurser** > **JDBC** > **Datakällor** i navigeringsträdet och klicka på **IDP_DS** i den högra rutan.
1. Klicka på **Anpassade egenskaper** under Ytterligare egenskaper i den högra rutan och klicka sedan på **Nytt**.
1. I rutan **Namn** skriver du `integratedSecurity` och `true` i rutan **Värde**.
1. Klicka på **Resurser** > **JDBC** > **Datakällor** i navigeringsträdet och klicka på **RM_DS** i den högra rutan.
1. Klicka på **Anpassade egenskaper** under Ytterligare egenskaper i den högra rutan och klicka sedan på **Nytt**.
1. I rutan **Namn** skriver du `integratedSecurity` och `true` i rutan **Värde**.
1. På den dator där WebSphere är installerat lägger du till filen sqljdbc_auth.dll i Windows systemsökväg (C:\Windows). Filen sqljdbc_auth.dll finns på samma plats som drivrutinsinstallationen för Microsoft SQL JDBC 1.2 (standard är *[InstallDir]*/sqljdbc_1.2/enu/auth/x86).
1. Välj **Start** > **Kontrollpanelen** > **Tjänster**, högerklicka på Windows-tjänsten för WebSphere (IBM WebSphere Application Server &lt;version> - &lt;nod>) och välj **Egenskaper**.
1. Klicka på fliken **Logga in** i dialogrutan Egenskaper.
1. Välj **Det här kontot** och ange den information som krävs för att ange det inloggningskonto som du vill använda.
1. Ange säkerhet på SQL Server från **Blandat** läge till **Endast Windows-autentisering**.

### Skydda åtkomst till känsligt innehåll i databasen {#protecting-access-to-sensitive-content-in-the-database}

AEM Forms databasschema innehåller känslig information om systemkonfiguration och affärsprocesser och bör döljas bakom brandväggen. Databasen bör beaktas inom samma förtroendegräns som Forms Server. För att skydda mot informationsexponering och stöld av affärsdata måste databasen konfigureras av databasadministratören så att endast behöriga administratörer får åtkomst till databasen.

Som en extra försiktighetsåtgärd bör du överväga att använda databasleverantörsspecifika verktyg för att kryptera kolumner i tabeller som innehåller följande data:

* Rights Management Document Keys
* HSM-PIN-krypteringsnyckel för Trust Store
* Hash för lokalt användarlösenord

Mer information om leverantörsspecifika verktyg finns i [&quot;Databassäkerhetsinformation&quot;](https://helpx.adobe.com/aem-forms/6-1/hardening-security/general-security-considerations.html#database_security_information).

### LDAP-säkerhet {#ldap-security}

En LDAP-katalog (Lightweight Directory Access Protocol) används vanligtvis av AEM Forms på JEE som källa för företagsanvändar- och gruppinformation och ett sätt att utföra lösenordsautentisering. Du bör se till att LDAP-katalogen är konfigurerad att använda SSL (Secure Socket Layer) och att AEM Forms on JEE är konfigurerat att komma åt LDAP-katalogen med hjälp av SSL-porten.

#### LDAP - denial of service {#ldap-denial-of-service}

En vanlig attack med LDAP innebär att en angripare avsiktligt misslyckas med att autentisera flera gånger. Detta tvingar LDAP-katalogservern att låsa ut en användare från alla LDAP-beroende tjänster.

Du kan ange antalet misslyckade försök och efterföljande låsningstid som AEM Forms implementerar när en användare upprepade gånger inte kan autentisera till AEM Forms. Välj låga värden i administrationskonsolen. När du väljer antalet misslyckade försök är det viktigt att du förstår att AEM Forms efter alla försök låser ut användaren innan LDAP-katalogservern gör det.

#### Ange automatisk låsning av konton {#set-automatic-account-locking}

1. Logga in på administrationskonsolen.
1. Klicka på **Inställningar** > **Användarhantering** > **Domänhantering**.
1. Under Inställningar för låsning av automatiskt konto anger du **maximalt antal misslyckade sekventiella autentiseringar** till ett lågt antal, till exempel 3.
1. Klicka på **Spara**.

### Granskning och loggning {#auditing-and-logging}

En korrekt och säker användning av programgranskning och loggning kan bidra till att säkerställa att säkerheten och andra avvikande händelser spåras och upptäcks så snabbt som möjligt. Effektiv användning av granskning och loggning i ett program omfattar bland annat att spåra genomförda och misslyckade inloggningar och viktiga programhändelser som skapande eller borttagning av nyckelposter.

Du kan använda granskning för att identifiera många typer av attacker, bland annat:

* Tydliga lösenordsattacker
* Denial of service-attacker
* Insprutning av fientliga indata och relaterade klasser av skriptattacker

I den här tabellen beskrivs teknik för granskning och loggning som du kan använda för att minska serverns sårbarheter.

<table> 
 <thead> 
  <tr> 
   <th><p>Problem</p> </th> 
   <th><p>Beskrivning</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>Loggfils-ACL:er</p> </td> 
   <td><p>Ange lämplig AEM Forms i JEE-loggfilens åtkomstkontrollistor.</p> <p>Genom att ange rätt autentiseringsuppgifter förhindrar du att angripare tar bort filerna.</p> <p>Säkerhetsbehörigheterna i loggfilskatalogen bör vara Full kontroll för administratörer och SYSTEM-grupper. AEM Forms-användarkontot bör endast ha läs- och skrivbehörighet.</p> </td> 
  </tr> 
  <tr> 
   <td><p>Loggfilsredundans</p> </td> 
   <td><p>Om resurserna tillåter det, skickar du loggar till en annan server i realtid som inte är tillgänglig för angriparen (skrivskyddad) med hjälp av Syslog, Tivoli, Microsoft Operations Manager (MOM) Server eller någon annan mekanism.</p> <p>Skydda loggar på det här sättet hjälper till att förhindra manipulering. Dessutom underlättar lagring av loggar i ett centralt arkiv korrelation och övervakning (t.ex. om flera blankettservrar används och en lösenordsgissningsattack utförs på flera datorer där varje dator efterfrågas efter ett lösenord).</p> </td> 
  </tr> 
 </tbody> 
</table>

### Ge en användare som inte är administratör behörighet att köra PDF Generator

Du kan aktivera användare som inte är administratörer för att använda PDF Generator. Normalt kan endast användare med administratörsbehörighet använda PDF Generator. Gör så här för att göra det möjligt för en icke-administratörsanvändare att köra PDF Generator:

1. Skapa en miljövariabel med namnet PDFG_NON_ADMIN_ENABLED.

1. Ange värdet för variabeln till TRUE.

1. Starta om AEM Forms.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

## Konfigurera AEM Forms på JEE för åtkomst utanför företaget {#configuring-aem-forms-on-jee-for-access-beyond-the-enterprise}

När du har installerat AEM Forms på JEE är det viktigt att regelbundet upprätthålla säkerheten i din miljö. I det här avsnittet beskrivs de åtgärder som rekommenderas för att upprätthålla säkerheten för din AEM Forms på JEE-produktionsservern.

### Konfigurera en omvänd proxy för webbåtkomst {#setting-up-a-reverse-proxy-for-web-access}

En *omvänd proxy* kan användas för att se till att en uppsättning URL:er för AEM Forms på JEE-webbprogram är tillgängliga för både externa och interna användare. Den här konfigurationen är säkrare än att tillåta användare att ansluta direkt till programservern som AEM Forms på JEE körs på. Den omvända proxyn utför alla HTTP-begäranden för den programserver som kör AEM Forms på JEE. Användare har bara nätverksåtkomst till den omvända proxyn och kan bara försöka ansluta till URL som stöds av den omvända proxyn.

**AEM Forms på JEE-rot-URL:er för användning med omvänd proxyserver**

Följande URL för programrot för varje AEM Forms på JEE-webbprogram. Du bör bara konfigurera din omvända proxy så att URL:er för webbprogramfunktioner som du vill ge slutanvändarna visas.

Vissa URL:er markeras som användarvänliga webbprogram. Du bör undvika att exponera andra URL:er för Configuration Manager för åtkomst till externa användare via den omvända proxyn.

<table> 
 <thead> 
  <tr> 
   <th><p>Rotadress</p> </th> 
   <th><p>Syfte och/eller tillhörande webbprogram</p> </th> 
   <th><p>Webbaserat gränssnitt</p> </th> 
   <th><p>Slutanvändaråtkomst</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>/ReaderExtensions/*</p> </td> 
   <td><p>Acrobat Reader DC-tillägg för slutanvändarens webbprogram för användning i PDF-dokument</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/edc/*</p> </td> 
   <td><p>Rights Management webbprogram för slutanvändare</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/edcws/*</p> </td> 
   <td><p>Webbtjänst-URL för Rights Management</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/pdfgui/*</p> </td> 
   <td><p>PDF Generator administrationswebbprogram</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/workspace/*</p> </td> 
   <td><p>Workspace webbprogram för slutanvändare</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/workspace-server/*</p> </td> 
   <td><p>Workspace-servrar och datatjänster som Workspace klientprogram kräver</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/adobe-bootstrapper/*</p> </td> 
   <td><p>Servlet för att starta AEM Forms i JEE-databasen</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/soap/*</p> </td> 
   <td><p>Informationssida för Forms Server-webbtjänster</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/soap/services/*</p> </td> 
   <td><p>Webbtjänst-URL för alla Forms Server-tjänster</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/edc/admin/*</p> </td> 
   <td><p>Rights Management administrationswebbprogram</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/adminui/*</p> </td> 
   <td><p>Startsida för administrationskonsolen</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/TruststoreComponent/</p> <p>skyddad/*</p> </td> 
   <td><p>Administrationssidor för Trust Store-hantering</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/FormsIVS/*</p> </td> 
   <td><p>Forms IVS-program för testning och felsökning av formuläråtergivning</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/OutputIVS/*</p> </td> 
   <td><p>Använd IVS-program för testning och felsökning av utdatatjänst</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/rmws/*</p> </td> 
   <td><p>REST URL för Rights Management</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/OutputAdmin/*</p> </td> 
   <td><p>Utdataadministrationssidor</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/FormServer/*</p> </td> 
   <td><p>Forms webbprogramfiler</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/FormServer/GetImage</p> <p>Servlet</p> </td> 
   <td><p>Används för att hämta JavaScript under HTML-omvandling</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/FormServerAdmin/*</p> </td> 
   <td><p>Forms administrationssidor</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/databas/*</p> </td> 
   <td><p>URL för WebDAV-åtkomst (felsökning)</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/AACComponent/*</p> </td> 
   <td><p>Användargränssnittet Program och tjänster</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/WorkspaceAdmin/*</p> </td> 
   <td><p>Workspace administrationssidor</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/rest/*</p> </td> 
   <td><p>Övriga supportsidor</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/CoreSystemConfig/*</p> </td> 
   <td><p>AEM Forms på inställningssidan för JEE Core-konfiguration</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/um/</p> </td> 
   <td><p>Autentisering av användarhantering</p> </td> 
   <td><p>Nej</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
  <tr> 
   <td><p>/um/*</p> </td> 
   <td><p>Administrationsgränssnitt för användarhantering</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Nej</p> </td> 
  </tr> 
  <tr> 
   <td><p>/DocumentManager/*</p> </td> 
   <td><p>Överföra och ladda ned dokument som ska behandlas vid åtkomst till fjärranslutningspunkter, SOAP WSDL-slutpunkter och Java SDK över SOAP transport eller EJB-transport med HTTP-dokument aktiverade.</p> </td> 
   <td><p>Ja</p> </td> 
   <td><p>Ja</p> </td> 
  </tr> 
 </tbody> 
</table>

## Skydda dig mot attacker med förfalskade förfrågningar på webbplatser {#protecting-from-cross-site-request-forgery-attacks}

En CSRF-attack (Cross-Site Request Forgery) utnyttjar det förtroende som en webbplats har för användaren för att överföra kommandon som är otillåtna och oavsedda av användaren. Anfallet konfigureras genom att en länk, ett skript eller en URL-adress läggs till i ett e-postmeddelande för att komma åt en annan webbplats som användaren redan har autentiserats på.

Du kan till exempel vara inloggad på administrationskonsolen samtidigt som du bläddrar på en annan webbplats. En av webbsidorna kan innehålla en HTML-bildtagg med ett `src`-attribut för ett serverskript på offrets webbplats. Genom att använda den cookie-baserade sessionsautentiseringsmekanismen som tillhandahålls av webbläsare kan angripande webbplats skicka skadliga förfrågningar till detta skript på servern, som masquerading är den legitima användaren. Mer exempel finns i [https://owasp.org/www-community/attacks/csrf#Examples](https://owasp.org/www-community/attacks/csrf#Examples).

Följande egenskaper är gemensamma för CSRF:

* Involvera webbplatser som förlitar sig på en användares identitet.
* Utnyttja webbplatsens förtroende för den identiteten.
* Testa användarens webbläsare för att skicka HTTP-begäranden till en målplats.
* Involvera HTTP-begäranden som har sidoeffekter.

AEM Forms på JEE använder funktionen Refererarfilter för att blockera CSRF-attacker. Följande termer används i det här avsnittet för att beskriva mekanismen för referensfiltrering:

* **Tillåten referent:** En referent är adressen till källsidan som skickar en begäran till servern. För JSP-sidor eller -formulär är referensen vanligtvis föregående sida i webbläsarhistoriken. Referent för bilder är vanligtvis de sidor som bilderna visas på. Du kan identifiera den referent som har behörighet till serverresurserna genom att lägga till dem i listan över tillåtna referenter.
* **Tillåtna referensundantag:** Du kanske vill begränsa åtkomstmöjligheterna för en viss referent i listan över tillåtna referenter. Om du vill tillämpa den här begränsningen kan du lägga till enskilda sökvägar för den referenten i listan med tillåtna undantag för referenten. Begäranden som kommer från sökvägar i listan över tillåtna referensundantag förhindras från att anropa någon resurs på Forms Server. Du kan definiera tillåtna referensundantag för ett visst program och även använda en global lista med undantag som gäller för alla program.
* **Tillåtna URI:er:** Det här är en lista över resurser som ska hanteras utan att refererarhuvudet kontrolleras. Resurser, t.ex. hjälpsidor som inte leder till statusändringar på servern, kan läggas till i den här listan. Resurserna i listan Tillåtna URI:er blockeras aldrig av referensfiltret oavsett vem som refererar.
* **Null-referens:** En serverbegäran som inte är associerad med eller inte kommer från en överordnad webbsida betraktas som en begäran från en Null-referens. När du till exempel öppnar ett nytt webbläsarfönster, skriver en adress och trycker på Retur är den referent som skickas till servern null. Ett skrivbordsprogram (.NET eller SWING) som gör en HTTP-begäran till en webbserver skickar även en Null-referens till servern.

### Referentfiltrering {#referer-filtering}

Refererarfiltreringsprocessen kan beskrivas så här:

1. Forms Server kontrollerar HTTP-metoden som används för anrop:

   1. Om det är POST utför Forms Server rubrikkontrollen.
   1. Om det är GET åsidosätter Forms Server referenskontrollen, såvida inte *CSRF_CHECK_GETS* är inställd på true. I så fall utförs referenthuvudkontrollen. *CSRF_CHECK_GETS* anges i filen *web.xml* för ditt program.

1. Forms Server kontrollerar om den begärda URI:n finns i tillåtelselista:

   1. Om URI:n är tillåtslista accepterar servern begäran.
   1. Om den begärda URI:n inte är tillåtslista hämtar servern referenten för begäran.

1. Om det finns en referent i begäran kontrollerar servern om det är en tillåten referent. Om det är tillåtet söker servern efter ett referensundantag:

   1. Om det är ett undantag blockeras begäran.
   1. Om det inte är ett undantag skickas begäran.

1. Om det inte finns någon referent i begäran kontrollerar servern om en null-referent är tillåten:

   1. Om en Null-referens tillåts, skickas begäran.
   1. Om Null-referens inte tillåts, kontrollerar servern om den begärda URI:n är ett undantag för Null-referenten och hanterar begäran i enlighet med detta.

### Hantera referensfiltrering {#managing-referer-filtering}

AEM Forms på JEE tillhandahåller ett referensfilter för att ange vilken referent som har behörighet till serverresurserna. Som standard filtrerar inte referensfiltret begäranden som använder en säker HTTP-metod, till exempel GET, såvida inte *CSRF_CHECK_GETS* har värdet true. Om portnumret för en post med tillåten referens är 0, tillåter AEM Forms på JEE alla förfrågningar från den värden oavsett portnummer. Om inget portnummer anges tillåts endast begäranden från standardporten 80 (HTTP) eller port 443 (HTTPS). Referensfiltrering är inaktiverat om alla poster i listan över tillåtna referenter tas bort.

När du först installerar Document Services uppdateras listan över tillåtna referenter med adressen till den server där Document Services är installerat. Posterna för servern omfattar servernamnet, IPv4-adressen, IPv6-adressen om IPv6 är aktiverat, loopback-adressen och en localhost-post. Namnen som läggs till i listan över tillåtna referenter returneras av värdoperativsystemet. En server med IP-adressen 10.40.54.187 innehåller till exempel följande poster: `https://server-name:0, https://10.40.54.187:0, https://127.0.0.1:0, http://localhost:0`. Tillåtelselista uppdateras inte för alla okvalificerade namn som returneras av värdoperativsystemet (namn som inte har IPv4-adress, IPv6-adress eller kvalificerat domännamn). Ändra listan över tillåtna referenter så att den passar din affärsmiljö. Distribuera inte Forms Server i produktionsmiljön med standardlistan Tillåten referent. När du har ändrat någon av de tillåtna referenserna, referensundantagen eller URI:erna måste du starta om servern för att ändringarna ska börja gälla.

**Hantera listan över tillåtna referenter**

Du kan hantera listan Tillåten referent från användarhanteringsgränssnittet i administrationskonsolen. Med användarhanteringsgränssnittet kan du skapa, redigera och ta bort listan. Se avsnittet * [Förhindra CSRF-attacker](/help/forms/using/admin-help/preventing-csrf-attacks.md)* i *administrationshjälpen* för mer information om hur du arbetar med listan över tillåtna referenter.

**Hantera listor över tillåtna referensundantag och tillåtna URI:er**

AEM Forms på JEE innehåller API:er för att hantera listan över tillåtna referensundantag och listan över tillåtna URI:er. Du kan använda dessa API:er för att hämta, skapa, redigera eller ta bort listan. Nedan följer en lista över tillgängliga API:er:

* createAllowedURIsList
* getAllowedURIsList
* updateAllowedURIsList
* deleteAllowedURIsList
* addAllowedRefererExceptions
* getAllowedRefererExceptions
* updateAllowedReferenceExceptions
* deleteAllowedRefererExceptions

Mer information om API:erna finns i* AEM Forms on JEE API Reference*.

Använd listan ***LC_GLOBAL_ALLOWED_REFERER_EXCEPTION*** för tillåtna referensundantag på global nivå, d.v.s. för att definiera undantag som gäller för alla program. Den här listan innehåller bara URI:er med antingen en absolut sökväg (till exempel `/index.html`) eller en relativ sökväg (till exempel `/sample/`). Du kan också lägga till ett reguljärt uttryck i slutet av en relativ URI, till exempel `/sample/(.)*`.

List-ID:t ***LC_GLOBAL_ALLOWED_REFERER_EXCEPTION*** definieras som en konstant i klassen `UMConstants` i namnutrymmet `com.adobe.idp.um.api` som finns i `adobe-usermanager-client.jar`. Du kan använda AEM Forms API:er för att skapa, ändra eller redigera den här listan. Om du till exempel vill skapa listan Global Allowed Referrer Exceptions använder du:

```java
addAllowedRefererExceptions(UMConstants.LC_GLOBAL_ALLOWED_REFERER_EXCEPTION, Arrays.asList("/index.html", "/sample/(.)*"))
```

Använd listan ***CSRF_ALLOWED_REFERER_EXCEPTIONS*** för programspecifika undantag.

**Inaktiverar referensfiltret**

Om referensfiltret helt blockerar åtkomsten till Forms Server och du inte kan redigera listan Tillåten referent, kan du uppdatera serverns startskript och inaktivera Referensfiltrering.

Inkludera JAVA-argumentet `-Dlc.um.csrffilter.disabled=true` i startskriptet och starta om servern. Se till att du tar bort JAVA-argumentet efter att du har konfigurerat om listan över tillåtna referenter.

**Referensfiltrering för anpassade WAR-filer**

Du kan ha skapat anpassade WAR-filer för att arbeta med AEM Forms i JEE för att uppfylla dina affärskrav. Om du vill aktivera Referensfiltrering för dina anpassade WAR-filer inkluderar du ***adobe-usermanager-client.jar*** i klassökvägen för WAR och inkluderar en filterpost i filen* web.xml* med följande parametrar:

**CSRF_CHECK_GETS** styr referenskontrollen på GET-begäranden. Om den här parametern inte är definierad ställs standardvärdet in på false. Ta endast med den här parametern om du vill filtrera dina GET-begäranden.

**CSRF_ALLOWED_REFERER_EXCEPTIONS** är ID:t för listan över tillåtna referensundantag. Refererarfiltret förhindrar att begäranden som kommer från referenter i listan som identifieras av list-ID anropar någon resurs på Forms Server.

**CSRF_ALLOWED_URIS_LIST_NAME** är ID:t för listan Tillåtna URI:er. Refererarfiltret blockerar inte begäranden för någon av resurserna i listan som identifieras av list-ID, oavsett värdet på referensrubriken i begäran.

**CSRF_ALLOW_NULL_REFERER** styr beteendet för referensfiltret när referenten är null eller inte finns. Om den här parametern inte är definierad ställs standardvärdet in på false. Inkludera bara den här parametern om du vill tillåta Null-referenser. Om null-referenter tillåts kan vissa typer av attacker av typen Cross Site Request.

**CSRF_NULL_REFERER_EXCEPTIONS** är en lista över de URI:er för vilka ingen referenskontroll utförs när referenten är null. Den här parametern är bara aktiverad när *CSRF_ALLOW_NULL_REFERER* har värdet false. Avgränsa flera URI:er i listan med kommatecken.

Här följer ett exempel på filterposten i filen *web.xml* för en ***SAMPLE*** WAR-fil:

```java
<filter> 
       <filter-name> filter-name </filter-name> 
       <filter-class> com.adobe.idp.um.auth.filter.RemoteCSRFFilter </filter-class> 
     <!-- default is false --> 
     <init-param> 
      <param-name> CSRF_ALLOW_NULL_REFERER </param-name> 
      <param-value> false </param-value> 
     </init-param> 
     <!-- default is false --> 
     <init-param> 
      <param-name> CSRF_CHECK_GETS </param-name> 
      <param-value> true </param-value> 
     </init-param> 
     <!-- Optional --> 
     <init-param> 
       <param-name> CSRF_NULL_REFERER_EXCEPTIONS </param-name> 
       <param-value> /SAMPLE/login, /SAMPLE/logout  </param-value> 
     </init-param> 
     <!-- Optional --> 
     <init-param> 
      <param-name> CSRF_ALLOWED_REFERER_EXCEPTIONS </param-name> 
      <param-value> SAMPLE_ALLOWED_REF_EXP_ID </param-value> 
     </init-param> 
     <!-- Optional --> 
     <init-param> 
      <param-name> CSRF_ALLOWED_URIS_LIST_NAME </param-name> 
      <param-value> SAMPLE_ALLOWED_URI_LIST_ID     </param-value> 
     </init-param> 
</filter> 
    ........ 
    <filter-mapping> 
      <filter-name> filter-name </filter-name> 
      <url-pattern>/*</url-pattern> 
    </filter-mapping>
```

**Felsökning**

Om berättigade serverförfrågningar blockeras av CSRF-filtret kan du försöka med något av följande:

* Om den avvisade begäran har en referensrubrik bör du lägga till den i listan över tillåtna referenter. Lägg bara till den referent som du litar på.
* Om den avvisade begäran inte har något referenshuvud ändrar du klientprogrammet så att det innehåller ett referenshuvud.
* Om klienten kan arbeta i en webbläsare provar du den distributionsmodellen.
* Som en sista utväg kan du lägga till resursen i listan över tillåtna URI:er. Detta är inte en rekommenderad inställning.

## Säker nätverkskonfiguration {#secure-network-configuration}

I det här avsnittet beskrivs de protokoll och portar som krävs för AEM Forms på JEE och innehåller rekommendationer för att distribuera AEM Forms på JEE i en säker nätverkskonfiguration.

### Nätverksprotokoll som används av AEM Forms i JEE {#network-protocols-used-by-aem-forms-on-jee}

När du konfigurerar en säker nätverksarkitektur enligt beskrivningen i föregående avsnitt krävs följande nätverksprotokoll för interaktion mellan AEM Forms på JEE och andra system i företagsnätverket.

<table> 
 <thead> 
  <tr> 
   <th><p>Protokoll</p> </th> 
   <th><p>Använd</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>HTTP</p> </td> 
   <td> 
    <ul> 
     <li><p>Webbläsaren visar Configuration Manager och slutanvändarens webbprogram</p> </li> 
     <li><p>Alla SOAP-anslutningar</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>SOAP</p> </td> 
   <td> 
    <ul> 
     <li><p>Webbtjänstklientprogram, t.ex. .NET-program</p> </li> 
     <li><p>Adobe Reader® använder SOAP för AEM Forms på JEE-serverns webbtjänster</p> </li> 
     <li><p>Adobe Flash®-program använder SOAP för Forms Server-webbtjänster</p> </li> 
     <li><p>AEM Forms på JEE SDK anropar när de används i SOAP-läge</p> </li> 
     <li><p>Workbench-designmiljö</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>RMI</p> </td> 
   <td><p>AEM Forms på JEE SDK anropar när de används i Enterprise JavaBeans-läge (EJB)</p> </td> 
  </tr> 
  <tr> 
   <td><p>IMAP/POP3</p> </td> 
   <td> 
    <ul> 
     <li><p>E-postbaserad inmatning till en tjänst (e-postslutpunkt)</p> </li> 
     <li><p>Meddelanden om användaruppgifter via e-post</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>UNC-fil-I</p> </td> 
   <td><p>AEM Forms on JEE-övervakning av bevakade mappar för indata i en tjänst (bevakad mappslutpunkt)</p> </td> 
  </tr> 
  <tr> 
   <td><p>LDAP</p> </td> 
   <td> 
    <ul> 
     <li><p>Synkroniseringar av organisationsanvändar- och gruppinformation i en katalog</p> </li> 
     <li><p>LDAP-autentisering för interaktiva användare</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>JDBC</p> </td> 
   <td> 
    <ul> 
     <li><p>Fråga efter och bearbeta anrop till en extern databas när en process körs med JDBC-tjänsten</p> </li> 
     <li><p>Intern åtkomst till AEM Forms i JEE-arkivet</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>WebDAV</p> </td> 
   <td><p>Möjliggör fjärrsurfning av AEM Forms i JEE-databasen (formulär, fragment osv.) av alla WebDAV-klienter</p> </td> 
  </tr> 
  <tr> 
   <td><p>AMF</p> </td> 
   <td><p>Adobe Flash-program, där AEM Forms på JEE-servertjänster har konfigurerats med en fjärrslutpunkt</p> </td> 
  </tr> 
  <tr> 
   <td><p>JMX</p> </td> 
   <td><p>AEM Forms på JEE visar MBeans för övervakning med JMX</p> </td> 
  </tr> 
 </tbody> 
</table>

### Portar för programservrar {#ports-for-application-servers}

I det här avsnittet beskrivs standardportarna (och alternativa konfigurationsintervall) för varje typ av programserver som stöds. Portarna måste aktiveras eller inaktiveras på den inre brandväggen, beroende på vilken nätverksfunktion du vill tillåta för klienter som ansluter till programservern som kör AEM Forms på JEE.

>[!NOTE]
>
>Som standard visas flera JMX MBeans under namnutrymmet adobe.com. Endast information som är användbar för serverhälsoövervakning visas. För att förhindra att information röjs bör du dock förhindra att anropare i ett otillförlitligt nätverk söker upp JMX MBeans och får tillgång till hälsostatistik.

**JBoss-portar**

<table> 
 <thead> 
  <tr> 
   <th><p>Syfte</p> </th> 
   <th><p>Port</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>Tillgång till webbprogram</p> </td> 
   <td><p>[JBOSS_Root]/standalone/configuration/lc_[database].xml</p> <p>HTTP/1.1 Connector port 8080</p> <p>AJP 1.3 Connector port 8009</p> <p>SSL/TLS Connector port 8443</p> </td> 
  </tr> 
  <tr> 
   <td><p>Stöd för CORBA</p> </td> 
   <td><p>[JBoss-rot]/server/all/conf/jacorb.properties</p> <p>OAPort 3528</p> <p>OASSLPort 3529</p> </td> 
  </tr> 
 </tbody> 
</table>

**WebLogic-portar**

<table> 
 <thead> 
  <tr> 
   <th><p>Syfte</p> </th> 
   <th><p>Port</p> </th> 
  </tr> 
 </thead> 
 <tbody>
  <tr> 
   <td><p>Tillgång till webbprogram</p> </td> 
   <td> 
    <ul> 
     <li><p>Admin Server-lyssningsporten: standard är 7001</p> </li> 
     <li><p>SSL-lyssningsporten för administratörsservern: standard är 7002</p> </li> 
     <li><p>Port konfigurerad för hanterad server, till exempel 8001</p> </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td><p>Administrationsportar för WebLogic krävs inte för åtkomst till AEM Forms på JEE</p> </td> 
   <td> 
    <ul> 
     <li><p>Hanterad serverlyssningsport: Kan konfigureras från 1 till 65534</p> </li> 
     <li><p>SSL-lyssningsporten för hanterad server: Kan konfigureras från 1 till 65534</p> </li> 
     <li><p>Nodhanterarens lyssningsport: standard är 5556</p> </li> 
    </ul> </td> 
  </tr> 
 </tbody> 
</table>

**WebSphere-portar**

Mer information om WebSphere-portar som krävs för AEM Forms på JEE finns i Portnummerinställning i WebSphere Application Server UI.

### Konfigurerar SSL {#configuring-ssl}

Med referens till den fysiska arkitekturen som beskrivs i avsnittet [AEM Forms på den fysiska JEE-arkitekturen](hardening-aem-forms-jee-environment.md#aem-forms-on-jee-physical-architecture) bör du konfigurera SSL för alla anslutningar som du tänker använda. Alla SOAP-anslutningar måste göras via SSL för att förhindra exponering av användaruppgifter i ett nätverk.

Instruktioner om hur du konfigurerar SSL på JBoss, WebLogic och WebSphere finns i&quot;Configuring SSL&quot; i [administrationshjälpen](https://www.adobe.com/go/learn_aemforms_admin_64).

Instruktioner om hur du importerar certifikat till JVM (Java Virtual Machine) som har konfigurerats för en AEM Forms-server finns i avsnittet om ömsesidig autentisering i [hjälpen för AEM Forms Workbench](https://www.adobe.com/go/learn_aemforms_workbench_65).

### Konfigurerar SSL-omdirigering {#configuring-ssl-redirect}

När du har konfigurerat programservern så att den stöder SSL måste du se till att all HTTP-trafik till program och tjänster framtvingas för att använda SSL-porten.

Information om hur du konfigurerar SSL-omdirigering för WebSphere eller WebLogic finns i dokumentationen för programservern.

1. Öppna kommandotolken, navigera till katalogen /JBOSS_HOME/standalone/configuration och kör följande kommando:

   `keytool -genkey -alias jboss7 -keyalg RSA -keystore server.keystore -validity 10950`

1. Öppna JBOSS_HOME/standalone/configuration/standalone.xml för redigering.

   Lägg till följande information efter &lt;subsystem xmlns=&quot;urn:jboss:domain:web:1.1&quot; native=&quot;false&quot; default-virtual-server=&quot;default-host&quot;>-elementet:

   &lt;connector name=&quot;https&quot; protocol=&quot;HTTP/1.1&quot; scheme=&quot;https&quot; socket-binding=&quot;https&quot; enabled=&quot;true&quot; secure=&quot;true&quot;/>

1. Lägg till följande kod i https-anslutningselementet:

   ```xml
   <connector name="https" protocol="HTTP/1.1" scheme="https" socket-binding="https" secure="true" enabled="true"> 
    <ssl name="jboss7_ssl" key-alias="jboss71" password="Tibco321" certificate-key-file="../standalone/configuration/server.keystore" protocol="TLSv1"/> 
    </connector>
   ```

   Spara och stäng filen standalone.xml.

## Windows-specifika säkerhetsrekommendationer {#windows-specific-security-recommendations}

Det här avsnittet innehåller säkerhetsrekommendationer som är specifika för Windows när de används för att köra AEM Forms på JEE.

### JBoss-tjänstkonton {#jboss-service-accounts}

AEM Forms på JEE-körkortsinstallationen konfigurerar som standard ett tjänstkonto med hjälp av kontot Lokalt system. Det inbyggda användarkontot för det lokala systemet har hög tillgänglighet. Det ingår i gruppen Administratörer. Om en arbetsprocessidentitet körs som det lokala systemanvändarkontot har den arbetsprocessen fullständig åtkomst till hela systemet.

#### Kör programservern med ett icke-administrativt konto {#run-the-application-server-using-a-non-administrative-account}

1. I Microsoft Management Console (MMC) skapar du en lokal användare som Forms Server-tjänsten kan logga in som:

   * Välj **Användaren kan inte ändra lösenordet**.
   * På fliken **Medlem i** ser du till att gruppen Användare visas.

1. Välj **Inställningar** > **Administrationsverktyg** > **Tjänster**.
1. Dubbelklicka på programservertjänsten och stoppa tjänsten.
1. På fliken **Logga in** väljer du **Det här kontot**, bläddrar efter det användarkonto du har skapat och anger lösenordet för kontot.
1. I fönstret Lokala säkerhetsinställningar, under Tilldelning av användarrättigheter, ger du följande rättigheter till användarkontot som Forms Server körs under:

   * Neka inloggning via Terminal Services
   * Neka inloggning locallyxx
   * Logga in som tjänst (bör vara inställd)

1. Ge det nya användarkontot behörighet att ändra i följande kataloger:
   * **GDS-katalog (Global Document Storage)**: Platsen för GDS-katalogen konfigureras manuellt under AEM Forms-installationsprocessen. Om platsinställningen är tom under installationen blir platsen som standard en katalog under programserverinstallationen på `[JBoss root]/server/[type]/svcnative/DocumentStorage`
   * **CRX-Databaskatalog**: Standardplatsen är `[AEM-Forms-installation-location]\crx-repository`
   * **AEM Forms temporära kataloger**:
      * (Windows) TMP- eller TEMP-sökväg som angetts i miljövariablerna
      * (AIX, Linux eller Solaris) Inloggad användares arbetskatalog
På UNIX-baserade system kan en icke-rotanvändare använda följande katalog som tillfällig katalog:
      * (Linux) /var/tmp eller /usr/tmp
      * (AIX) /tmp eller /usr/tmp
      * (Solaris) /var/tmp eller /usr/tmp
1. Ge det nya användarkontot skrivbehörighet i följande kataloger:
   * [JBoss-katalog]\fristående\distribution
   * [JBoss-katalog]\fristående\
   * [JBoss-directory]\bin\

   >[!NOTE]
   >
   >Standardinstallationsplatsen för JBoss Application Server:
   >
   >* Windows: C:\Adobe\Adobe_Experience_Manager_Forms\jboss
   >* Linux: /opt/jboss/.

1. Starta programservertjänsten.

### Filsystemsäkerhet {#file-system-security}

AEM Forms på JEE använder filsystemet på följande sätt:

* Lagrar tillfälliga filer som används vid bearbetning av dokumentindata och -utdata
* Lagrar filer i den globala arkivbutiken som används för att stödja de installerade lösningskomponenterna
* Bevakade mappar lagrar släppta filer som används som indata till en tjänst från en filsystemmapplats

När du använder bevakade mappar som ett sätt att skicka och ta emot dokument med en Forms Server-tjänst bör du vidta extra försiktighetsåtgärder när det gäller filsystemsäkerhet. När en användare släpper innehåll i den bevakade mappen visas innehållet i den bevakade mappen. I det här fallet autentiserar tjänsten inte den faktiska slutanvändaren. I stället förlitar det sig på ACL- och Share-nivåsäkerhet som ställs in på mappnivå för att avgöra vem som effektivt kan anropa tjänsten.

## JBoss-specifika säkerhetsrekommendationer {#jboss-specific-security-recommendations}

Det här avsnittet innehåller programserverkonfigurationsrekommendationer som är specifika för JBoss 7.0.6 när de används för att köra AEM Forms på JEE.

### Inaktivera JBoss-hanteringskonsolen och JMX-konsolen {#disable-jboss-management-console-and-jmx-console}

Åtkomst till JBoss Management Console och JMX Console är redan konfigurerad (JMX-övervakning är inaktiverad) när du installerar AEM Forms på JEE på JBoss med körklar installationsmetod. Om du använder en egen JBoss-programserver kontrollerar du att åtkomsten till JBoss Management Console och JMX-övervakningskonsolen är skyddade. Åtkomst till JMX-övervakningskonsolen ställs in i JBoss-konfigurationsfilen som kallas jmx-invoker-service.xml.

### Inaktivera katalogbläddring {#disable-directory-browsing}

När du har loggat in på administrationskonsolen går det att bläddra i konsolens kataloglista genom att ändra URL:en. Om du till exempel ändrar URL-adressen till någon av följande URL-adresser kan en kataloglista visas:

```java
https://<servername>:8080/adminui/secured/ 
https://<servername>:8080/um/
```

## WebLogic-specifika säkerhetsrekommendationer {#weblogic-specific-security-recommendations}

Det här avsnittet innehåller rekommendationer för programserverkonfiguration för att skydda WebLogic 9.1 när AEM Forms körs på JEE.

### Inaktivera katalogbläddring {#disable_directory_browsing-1}

Ange egenskaperna för index-directories i filen weblogic.xml till `false`, vilket visas i det här exemplet:

```xml
<container-descriptor> 
    <index-directory-enabled>false 
    </index-directory-enabled> 
</container-descriptor>
```

### Aktivera WebLogic SSL-port {#enable-weblogic-ssl-port}

WebLogic aktiverar som standard inte SSL-lyssningsporten, 7002. Aktivera den här porten i WebLogic Server Administration Console innan du konfigurerar SSL.

## WebSphere-specifika säkerhetsrekommendationer {#websphere-specific-security-recommendations}

Det här avsnittet innehåller rekommendationer för programserverkonfiguration för att skydda WebSphere som kör AEM Forms på JEE.

### Inaktivera katalogbläddring {#disable_directory_browsing-2}

Ange egenskapen `directoryBrowsingEnabled` i filen ibm-web-ext.xml till `false`.

### Aktivera administrationssäkerhet för WebSphere {#enable-websphere-administrative-security}

1. Logga in på administrationskonsolen för WebSphere.
1. Gå till **Säkerhet** > **Global säkerhet** i navigeringsträdet
1. Välj **Aktivera administrativ säkerhet**.
1. Avmarkera både **Aktivera programsäkerhet** och **Använd Java 2-säkerhet**.
1. Klicka på **OK** eller **Använd**.
1. Klicka på **Spara direkt i huvudkonfigurationen** i rutan **Meddelanden**.
