---
title: Installera workbench
description: Lär dig hur du installerar, avinstallerar, konfigurerar, administrerar eller distribuerar AEM Forms Workbench.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
role: Admin, User, Developer
solution: Experience Manager, Experience Manager Forms
feature: Workbench,Adaptive Forms
source-git-commit: 887dc1d6d7e11672b62ef5ca5463ea6181ff0320
workflow-type: tm+mt
source-wordcount: '2184'
ht-degree: 0%

---

# Installera Workbench {#install-workbench}

Det här dokumentet innehåller anvisningar om hur du installerar och konfigurerar AEM Forms Workbench. Installationsprogrammet installerar även Forms Designer.

## Vem ska läsa det här dokumentet? {#who-should-read-this-doc}

Det här dokumentet är avsett för administratörer och utvecklare som ansvarar för att installera, konfigurera, administrera eller distribuera Workbench. Här finns även information om hur du konfigurerar systemet för att stödja dina uppgraderade AEM Forms-processer. Informationen baseras på antagandet att alla som läser det här dokumentet känner till Microsoft® Windows®.

## Ytterligare information {#additional-information}

Resurserna i den här tabellen kan hjälpa dig att lära dig mer om och komma igång med att använda AEM Forms.
<table>
 <tbody>
  <tr>
   <td><p><strong>Mer information om</strong></p> </td>
   <td><p><strong>Se</strong></p> </td>
  </tr>
  <tr>
   <td><p>Procedurinformation för Workbench</p> </td>
   <td><p><a href="https://helpx.adobe.com/content/dam/help/en/experience-manager/6-5/forms/pdf/WorkbenchHelp.pdf">Workbench - hjälp</a><br /> <br /> </p> </td>
  </tr>
  <tr>
   <td><p>Allmän information om AEM Forms och hur det integreras med andra Adobe-produkter</p> </td>
   <td><p><a href="https://experienceleague.adobe.com/docs/experience-manager-65-2025/forms/getting-started/introduction-aem-forms.html?lang=en">AEM Forms - översikt</a><br /> <br /> </p> </td>
  </tr>
  <tr>
   <td><p>All tillgänglig dokumentation för AEM Forms</p> </td>
   <td><p><a href="https://experienceleague.adobe.com/docs/experience-manager-65-2025/forms/getting-started/introduction-aem-forms.html?lang=en">AEM Forms-dokumentation</a><br /> <br /> </p> </td>
  </tr>
  <tr>
   <td><p>Lappa uppdateringar, tekniska anteckningar och ytterligare information om den här produktversionen</p> </td>
   <td><p>Kontakta Adobe Enterprise Support</a><br /> <br /> </p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Flex Workspace används inte i AEM Forms. Den finns för AEM Forms.

## Före installationen {#before-you-install}

### Installationsöversikt för Workbench {#workbench-installation-overview}

Workbench är en integrerad utvecklingsmiljö (IDE) som utvecklare och formulärförfattare använder för att skapa automatiserade affärsprocesser och formulär. Det används också för att hantera de resurser och tjänster som processerna och formulären använder.

Följande bild visar Workbench-installationen:
* Processdesign med Workbench
* Formulärdesign med Designer

>[!NOTE]
>
>AEM Forms Server kräver ett separat installationsprogram. Mer information finns i installationsdokumentationen för AEM Forms on JEE.

![default-render-form](assets/installing-workbench.png)

## Systemkrav {#system-prerequisites}

I det här avsnittet beskrivs maskinvaru- och programvarukraven samt vilka plattformar som stöds.

### Lägsta maskin- och programvarukrav {#minimum-hardware-software-requirements}

**Workbench**
Följande krav rekommenderas som minimum:
Diskutrymme för installation:
* 680 MB endast för Workbench.
* 2,15 GB på en enda enhet för en fullständig installation av Workbench, Designer och exempelpaketet.
* 400 MB för temporära installationskataloger - 200 MB i användarens \temp-katalog och 200 MB i Windows temporära katalog.

>[!NOTE]
>
>Om alla dessa platser finns på en enda enhet måste det finnas 1,5 GB ledigt utrymme under installationen. De filer som kopieras till de tillfälliga katalogerna tas bort när installationen är klar.

* Maskinvarukrav: Intel® Pentium® 4- eller AMD®-motsvarighet, 1 GHz-processor.
* Java™ Runtime Environment (JRE) 7.0 update 51 or later updates to 7.0.
* Minst 1 024 × 768 pixlar eller högre bildskärmsupplösning med 16-bitars färg eller högre.
* TCP/IPv4- eller TCP/IPv6-nätverksanslutning till AEM Forms Server.
* Installera Visual C++ Redistributable runtime Packages 2012 32-bit.
* Installera Visual C++ Redistributable runtime Packages 2013 32-bit.

>[!NOTE]
>
>Du måste ha administratörsbehörighet för att installera Workbench. Om du installerar med ett icke-administratörskonto uppmanas du att ange autentiseringsuppgifter för ett lämpligt konto.

### Plattformar som stöds {#supported-platforms}

Se den fullständiga listan över plattformar som stöds för Workbench på [AEM Forms Supported Platforms](https://www.adobe.com/go/learn_aemforms_supportedplatforms_65).

## Installationsfrågor för Designer {#designer-installation-considerations}

Workbench-installationen innehåller som standard en motsvarande version av Designer som endast är för engelska. Om Workbench-installationsprogrammet upptäcker en befintlig version av Designer på datorn kan installationen avslutas och du måste ta bort den aktuella versionen av Designer innan du kan fortsätta.
Tabellen nedan innehåller en fullständig lista över möjliga installationsscenarier för Designer som du kan stöta på, och eventuella åtgärder som du måste vidta, när du installerar Workbench.

<table>
 <tbody>
  <tr>
   <td><p><strong>Installerad version av Designer</strong></p> </td>
   <td><p><strong>Nödvändiga åtgärder</strong></p> </td>
  </tr>
  <tr>
   <td><p>Acrobat Pro eller Acrobat Pro Extended (inkluderar Designer)</p> </td>
   <td><p>Ingen.<br /> 
Workbench-installationen identifierar en instans av Designer på datorn som installerats med antingen Acrobat Pro eller Acrobat Pro Extended.<br />
Olika versioner av Designer kan samexistera i samma system, till exempel Designer 6.4.x för Workbench 6.4 och Designer 6.5.0.x för Workbench 6.5. Du behöver inte avinstallera den version av Designer som har installerats med Acrobat 10 Pro eller Acrobat 10 Pro Extended eller senare.
<br /></p> </td>
  </tr>
  <tr>
   <td><p>Designer (fristående)</p> </td>
   <td><p>Ingen. <br />Den version av Designer som ingår i Workbench är endast engelsk. <br />Installationsprogrammet för Workbench installerar inte om en ny version av Designer. Istället patchas en uppdaterad version som medföljer installationsprogrammet för Workbench. Detta gör även att du kan använda din lokaliserade version av Designer i Workbench.<br /> </p> </td>
  </tr>
 </tbody>
</table>

### Avinstallera Designer (fristående) i Windows 10 {#uninstall-designer-standalone-windows10}

1. Gå till **Kontrollpanelen > Program > Program och funktioner**
1. I listan Installerade program väljer du **Adobe Designer**.
1. Klicka på **Avinstallera** och sedan på **Ja**.

## Installerar Workbench {#installing-workbench}

I det här kapitlet beskrivs hur du installerar Workbench.

### Installera och köra Workbench {#installing-and-running-workbench}

Innan du installerar Workbench måste du se till att din miljö innehåller den programvara och maskinvara som krävs för att köra den (se avsnittet **Innan du installerar**).

**Så här installerar och kör du Workbench:**

1. Gör något av följande:
   * Navigera till katalogen \workbench på installationsmediet och dubbelklicka på filen run_windows_installer.bat.
   * Ladda ned och expandera Workbench till filsystemet. När filen har laddats ned går du till katalogen \workbench och dubbelklickar på filen run_windows_installer.bat.

   >[!IMPORTANT]
   >
   >Installationsprogrammet för Workbench körs bara från en lokal enhet. Den kan inte köras från en fjärrplats.

   >[!NOTE]
   >
   >Om du får felmeddelandet&quot;Could not create the Java™ Virtual Machine&quot; skapar du en miljövariabel med namnet _JAVA_OPTIONS med värdet -Xmx512M och kör installationsprogrammet.

1. Klicka på Nästa på skärmen Introduktion.
1. Läs produktlicensavtalet, välj Jag accepterar villkoren i licensavtalet och klicka sedan på Nästa.
1. (Valfritt) Välj Installera Adobe Designer om du behöver det här verktyget för att skapa och ändra formulär.

   >[!NOTE]
   >
   >Du kan fortsätta använda Designer som installerats med Acrobat 10 genom att lämna det här alternativet avmarkerat.

1. Acceptera standardkatalogen enligt listan eller klicka på Välj och navigera till den katalog där du vill installera Workbench. Klicka sedan på Nästa.

   >[!NOTE]
   >
   >Installationskatalogsökvägen får inte innehålla tecknen # (pund) och $ (dollar).

1. Granska förinstallationssammanfattningen och klicka på Installera. Installationsprogrammet visar installationsförloppet.
1. Granska installationssammanfattningen. Välj Starta AEM Forms Workbench så att du kan starta Workbench och klicka sedan på Nästa.
1. Granska versionsinformationen och klicka på Klar.
1. Följande objekt är nu installerade på datorn:
   * **Workbench**: Om du vill köra Workbench från Start-menyn väljer du Alla program > AEM Forms > Workbench, om du väljer att spara genvägsmappen där. För information,   se <a href="https://helpx.adobe.com/content/dam/help/en/experience-manager/6-5/forms/pdf/WorkbenchHelp.pdf">Använda Workbench</a> -dokumentationen.
   * **Designer**: Du kan komma åt Designer inifrån Workbench. Mer information finns i avsnittet Komma igång i <a href="https://helpx.adobe.com/content/dam/help/en/experience-manager/6-5/forms/pdf/using-designer.pdf">Designer-hjälpen</a>.
   * **AEM Forms SDK**: Mer information om hur du använder SDK finns i <a href="https://helpx.adobe.com/pdf/aem-forms/6-3/programming-with-aem-forms.pdf">Programmering med AEM Forms</a>.

## Uppgraderingsprocesser {#upgrading-processes}

AEM Forms på JEE-processer kan uppgraderas till AEM Forms-program med hjälp av uppgraderingsguiden. Mer information finns i Uppgradera dokumentation om artefakter i Workbench-hjälpen.

### Konfigurera och logga in på en server {#configuring-and-logging-server}

Om du vill använda Workbench måste du ha en instans av AEM Forms igång, vanligtvis på en separat dator. Du måste ha ett användarnamn och lösenord för att kunna logga in på AEM Forms, samt information om var servern finns.

>[!NOTE]
>
>Om du har konfigurerat AEM Forms att använda EMC Documentum®- eller IBM® FileNet-databasprovidern och du vill logga in i en annan databas än den som är konfigurerad som standard i AEM administrationskonsol, anger du användarnamnet som username@Repository.

### Konfigurera timeout-inställningar {#configuring-timeout-settings}

Workbench gör som standard en timeout efter två timmar, oavsett aktivitet eller inaktivitet. Mer information om hur du redigerar timeout-inställningen finns i&quot;Konfigurera användarhantering > Konfigurera avancerade systemattribut&quot; i hjälpen för <a href="https://experienceleague.adobe.com/docs/experience-manager-65-2025/forms/administrator-help/configure-user-management/configure-advanced-system-attributes.html">administrationskonsolen</a>.

### Konfigurera Workbench för anslutning via HTTPS {#configuring-workbench-to-connect-over-HTTPS}

Om du vill ansluta Workbench till en AEM Forms-server via HTTPS måste du se till att den certifikatutfärdare (CA) som utfärdade den offentliga nyckeln tolkas som tillförlitlig av Workbench. Om certifikatet inte identifieras som att det kommer från en betrodd källa måste du uppdatera cacert-filen i katalogen [Workbench_HOME]/workbench/jre/lib/security.

>[!NOTE]
>
>[Workbench_HOME] representerar katalogen där du installerade Workbench. Standardplatsen är C:\Program (x86)\Adobe Experience Manager forms Workbench.

Kontrollera att du ansluter till HTTPS med det namn som anges i certifikatet. Det här namnet är vanligtvis det fullständiga, kvalificerade värdnamnet.

**Så här uppdaterar du cacert-filen**:
1. Kontrollera att du har en kopia av SSL-certifikatet (Secure Sockets Layer). Kontakta antingen administratören som konfigurerade SSL-servern eller exportera certifikatet med en webbläsare.

   >[!NOTE]
   >
   >Om du vill exportera certifikatet öppnar du en webbläsare och loggar in på administrationskonsolen. Installera certifikatet i webbläsaren och exportera sedan certifikatet från webbläsaren till en tillfällig lagringsplats (eller direkt till katalogen [Workbench_HOME]/workbench/jre/lib/security).

1. Kopiera certifikatet till katalogen [Workbench_HOME]/workbench/jre/lib/security.

1. Öppna ett kommandotolkfönster, navigera till [Workbench_HOME]/workbench/jre/bin och skriv sedan följande kommando:
   `keytool -import -storepass changeit -file [Workbench_HOME]\workbench\jre\lib\security\ssl_cert_for_certname.cer -keystore [Workbench_HOME]\workbench\jre\lib\security\cacerts -alias example`
Var:
   * `changeit` är standardlösenordet för kontots nyckelbehållare.
   * certname är det certifikat som du valde i steg 1.
   * exempel är det alias som du väljer för certifikatet. Det här värdet kan ändras.

1. När du uppmanas att lita på certifikatet skriver du Yes (Ja) och trycker på Enter. Nyckelverktyget fortsätter att importera kontofilen till katalogen [Workbench_HOME]/workbench/jre/lib/security.

1. Stäng och starta om Workbench för att tillämpa ändringarna.

### Konfigurera cacheinställningar för dynamiskt genererade mallar {#configuring-cache-settings-for-dynamically-generated-templates}

Följande aspekter av cacheåtgärden bör beaktas om programmet genererar unika mallar direkt genom att automatiskt uppdatera XFA-innehåll. I själva verket använder varje transaktion en ny, unik mall.

När formulärgeneratorn eller utdataanvändaren söker efter, eller uppdaterar, poster i cachen efter en viss formulärmall, används flera nyckelvärden för att hitta den specifika cachepost som är tillgänglig.

* **Mallfilnamn**: Plats och filnamn för mallen som används som primär unik identifierare för det cachelagrade formuläret.
* **Tidsstämpel**: Mallfilen innehåller en tidsstämpel som används för att bestämma formulärets senaste uppdateringstid.
* **Mall-UID**: Designer infogar en unik identifierare (UUID) för formuläret och dess version i varje mall. Varje gång formuläret uppdateras uppdateras det inbäddade UUID:t. En XDP-mall kan till exempel visa följande innehåll:

  `<?xml version="1.0" encoding="UTF-8"?>`
  `<?xfa generator="AdobeAEM formsDesignerES_V8.2" APIVersion="2.6.7185.0"?><xdp:xdp xmlns:xdp=https://ns.adobe.com/xdp/ timeStamp="2008-07-29T21:22:12Z" uuid="823e538f-ff6c-4961-b759-f7626978a223"><template xmlns="https://www.xfa.org/schema/xfa-template/2.6/">`

* **Återgivningsalternativ**: I den återgivna formulärcachen lagras cacheinnehållet separat för varje uppsättning med unika återgivningsalternativ.


Forms-tjänsten tar emot mallar utifrån filnamn eller databasplats, eller efter värde som ett XML-objekt i minnet.

* **Mallar som skickas med referens**: Innehållsroten och formulärnamnet används. Om unika mallar med olika filnamn skickas i varje begäran med den här metoden växer diskcachen oändligt och återanvänds aldrig. För att förhindra detta bör unika mallar skickas med samma filnamn för att säkerställa att samma cache uppdateras för alla begäranden.
* **Mallar som skickas av värde**: Använder mallbyte som skickas tillsammans med data med parametern inDataDoc. Om unika mallar med olika UUID skickas med den här metoden växer diskcachen oändligt och återanvänds aldrig. För att förhindra detta bör UUID-attributet tas bort från alla mallar för att säkerställa att ingen cache skapas för mallen. Om du skickar samma UUID som inte är null kan cacheobjekten skapas, men det säkerställer att samma cache uppdateras vid varje begäran.

För att förhindra att cacheminnet växer i oändlighet bör du överväga följande faktorer för att återge dynamiskt genererade mallar med de nya AEM Forms-API:erna renderHTMLForm2 och renderPDFForm2.

När du använder de nya API:erna skickas mallen som ett dokumentobjekt, som hanteras i Forms-tjänsten baserat på om den är passiv eller inte.

För passiva dokument där UUID och innehållsroten fungerar som cachenyckel bör du tänka på följande:

* Cachen skapas inte för passiva indatamallar utan UUID.
* Om fler än en passiv indatamall med samma UUID och innehållsrot skickas, skrivs samma cache över.

För icke-passiva dokument där filnamnet och innehållsroten fungerar som cachenyckel bör du tänka på följande:

* För icke-passiva indatamallar beror cachelagringen på innehållsroten och filnamnet som dokumentet genererades från.
Samma cache används endast för begäranden med samma innehållsrot och mallfilnamn.
Med följande metodtips kan du vara säker på att cacheminnet inte växer i oändlighet när dynamiskt genererade mallar skickas till Forms-tjänsten:
   * Ta bort UUID eller skicka samma UUID i alla dynamiskt genererade mallar.
   * Generera dokumentet antingen från mallbyte eller från samma filnamn på disken.

### Avinstallerar Workbench {#uninstalling-workbench}

Använd funktionen Lägg till eller ta bort program på Kontrollpanelen så att du kan starta avinstallationsprogrammet. Workbench- och Designer-programmen har separata avinstallationsprogram.

## Konfigurera AEM Forms XDC Editor {#configuring-aem-forms-xdc-editor}

Med XDC-redigeringsprogrammet kan nätverksskrivaradministratörer skapa och ändra XDC-filer (XML Forms Architecture Device Configuration). XDC-filer beskriver skrivarens funktioner, till exempel skrivarens språk eller korrelationen mellan pappersstorlek och fackets placering.

Innan administratören för nätverksskrivaren använder XDC-redigeringsprogrammet ska du flytta XDC-exempelfilerna och läsa Skapa enhetsprofiler med XDC-redigeringsprogrammet.

**Så här hämtar du XDC-exempelfilerna**:

1. På AEM Forms Server letar du reda på XDC-mappen i [AEM Forms root]\sdk\samples\Output\IVS.
1. Kopiera innehållet i den här mappen till en katalog som är tillgänglig från Workbench- eller Eclipse-systemet.

**Så här får du hjälp om XDC Editor**:

1. Gå till AEM Forms dokumentationswebbplats.
1. Klicka på fliken **Framkalla** och navigera till Skapa enhetsprofiler med XDC-redigeringsprogrammet. Hämta filen xdc_editor_help_web.zip och installera hjälpfilerna genom att följa instruktionerna i Viktigt-filen.
