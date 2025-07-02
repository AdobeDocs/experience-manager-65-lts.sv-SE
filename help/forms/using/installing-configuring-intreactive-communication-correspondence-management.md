---
title: Installera och konfigurera interaktiv kommunikation
description: Installera och konfigurera AEM Forms Interactive Communications för att skapa affärskorrespondenser, dokument, kontoutdrag, förmånsmeddelanden, marknadsföringsmejl, fakturor och välkomstpaket.
role: Admin, User, Developer
solution: Experience Manager, Experience Manager Forms
feature: Interactive Communication,Correspondence Management
exl-id: d03965e1-4fa3-414c-80b6-c9fca281bee4
source-git-commit: bd33420307a7be6664b6bbb52677af66edaa9c0e
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 0%

---

# Installera och konfigurera interaktiv kommunikation{#install-and-configure-interactive-communications}

## Introduktion {#introduction}

Med AEM Form kan man centralisera framtagning, sammanställning, hantering och distribution av säkra och interaktiva dokument, t.ex. affärskommunikation, dokument, kontoutdrag, förmånsmeddelanden, mejl, räkningar och välkomstpaket. Den här funktionen kallas interaktiv kommunikation. Funktionen ingår i AEM Forms tilläggspaket. Tilläggspaketet distribueras på en Author- eller Publish-instans av AEM.

Du kan använda den interaktiva kommunikationsfunktionen för att producera kommunikation i flera format. Till exempel webb och PDF. Ni kan integrera interaktiv kommunikation med AEM Workflow för att bearbeta och leverera den sammanställda kommunikationen till kunderna via valfri kanal. Du kan till exempel skicka en kommunikation till slutanvändaren via e-post.

Om du uppgraderar från en tidigare version och redan har investerat i korrespondenshantering kan du installera [kompatibilitetspaketet](../../forms/using/installing-configuring-intreactive-communication-correspondence-management.md#install-compatibility-package) för att fortsätta använda korrespondenshantering. Mer information om skillnaderna mellan interaktiv kommunikation och korrespondenshantering finns i [Översikt över interaktiv kommunikation](/help/forms/using/interactive-communications-overview.md#interactive-communications-vs-correspondence-management).

AEM Forms är en kraftfull plattform för större företag. Interaktiv kommunikation är bara en av AEM Forms funktioner. En fullständig lista över funktioner finns i [Introduktion till AEM Forms](../../forms/using/introduction-aem-forms.md).

## Distributionstopologi {#deployment-topology}

AEM Forms tilläggspaket är ett program som distribueras till AEM. Du behöver bara minst en instans av AEM Author och Processing för att kunna köra funktionen Interactive Communications. Följande topologi är en indikativ topologi för att köra AEM Forms Interactive Communications, Correspondence Management, AEM Forms datainhämtning och Forms-Centric-arbetsflöden för OSGi-funktioner. Mer information om topologin finns i [Arkitektur och distributionstopologier för AEM Forms](/help/forms/using/aem-forms-architecture-deployment.md).

![rekommenderad-topologi](assets/recommended-topology.png)

AEM Forms Interactive Communications kör administratörs-, redigerings- och agentanvändargränssnitt i Author-instanserna av AEM Forms. Publiceringsinstanserna är värdar för den slutliga versionen av interaktiv kommunikation som är klar att användas av slutanvändarna.

## Systemkrav {#system-requirements}

Innan du börjar installera och konfigurera interaktiva kommunikations- och korrespondenshanteringsfunktioner i AEM Forms bör du se till att:

* Maskinvaru- och programvaruinfrastruktur finns på plats. En detaljerad lista över maskinvara och programvara som stöds finns i [Tekniska krav](/help/sites-deploying/technical-requirements.md).

* Installationssökvägen för AEM-instansen innehåller inte blanksteg.
* En AEM-instans körs. I AEM-terminologi är &quot;instance&quot; en kopia av AEM som körs på en server i författar- eller publiceringsläge. Du behöver minst en AEM-instans (Författare eller Bearbetning) för att kunna köra AEM Forms interaktiva kommunikations- och korrespondenshanteringsfunktioner:

   * **Författare**: En AEM-instans användes för att skapa, överföra och redigera innehåll och för att administrera webbplatsen. När innehållet är klart att publiceras replikeras det till publiceringsinstansen.
   * **Bearbetning:** En bearbetningsinstans är en [härdad AEM Author](/help/forms/using/hardening-securing-aem-forms-environment.md)-instans. Du kan ställa in en Author-instans och göra den oskarp efter att du har utfört installationen.

   * **Publicera**: En AEM-instans som skickar det publicerade innehållet till allmänheten via Internet eller ett internt nätverk.

* Minneskraven är uppfyllda. AEM Forms tilläggspaket kräver:

   * 15 GB temporärt utrymme för Microsoft® Windows-baserade installationer.
   * 6 GB temporärt utrymme för UNIX-baserade installationer.

* Extra krav för UNIX-baserade system: Om du använder det UNIX-baserade operativsystemet installerar du följande paket från installationsmediet för respektive operativsystem.

<table>
 <tbody>
  <tr>
   <td>exponat</td>
   <td>libxcb</td>
   <td>freetype</td>
   <td>libXau</td>
  </tr>
  <tr>
   <td>libSM</td>
   <td>zlib</td>
   <td>libICE</td>
   <td>libuuid</td>
  </tr>
  <tr>
   <td>glibc</td>
   <td>libXext</td>
   <td><p>nss-softokn-free-bl</p> </td>
   <td>fontconfig</td>
  </tr>
  <tr>
   <td>libX11</td>
   <td>libXrender</td>
   <td>libXrandr</td>
   <td>libXinerama</td>
  </tr>
 </tbody>
</table>

## Installera AEM Forms-tilläggspaket {#install-aem-forms-add-on-package}

AEM Forms tilläggspaket är ett program som distribueras till AEM. Paketet innehåller interaktiv kommunikation med AEM Forms, korrespondenshantering och andra funktioner. Så här installerar du tilläggspaketet:

1. Öppna [Programvarudistribution](https://experience.adobe.com/downloads). Du behöver en Adobe ID för att logga in på Software Distribution.
1. Välj **[!UICONTROL Adobe Experience Manager]** som finns på rubrikmenyn.
1. I avsnittet **[!UICONTROL Filters]**:
   1. Välj **[!UICONTROL Forms]** i listrutan **[!UICONTROL Solution]**.
   2. Välj version och typ för paketet. Du kan också använda alternativet **[!UICONTROL Search Downloads]** för att filtrera resultaten.
1. Välj det paketnamn som gäller för ditt operativsystem, välj **[!UICONTROL Accept EULA Terms]** och välj **[!UICONTROL Download]**.
1. Öppna [Pakethanteraren](/help/sites-administering/package-manager.md) och klicka på **[!UICONTROL Upload Package]** för att överföra paketet.
1. Markera paketet och klicka på **[!UICONTROL Install]**.

   Du kan även hämta paketet via den direktlänk som visas i artikeln [AEM Forms-utgåvor](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE).

1. När paketet har installerats uppmanas du att starta om AEM-instansen. **Starta inte om servern omedelbart.** Innan du stoppar AEM Forms Server väntar du tills ServiceEvent REGISTERED- och ServiceEvent UNREGISTERED-meddelandena inte längre visas i filen [AEM-Installation-Directory]/crx-quickstart/logs/error.log och loggen är stabil.

   >[!NOTE]
   >
   > Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

1. Upprepa steg 1-7 för alla författare- och publiceringsinstanser.

## Konfiguration efter installation {#post-installation-configurations}

AEM Forms har några obligatoriska och valfria konfigurationer. De obligatoriska konfigurationerna är bland annat att konfigurera BouncyCastle-bibliotek och serialiseringsagent. De valfria konfigurationerna är bland annat att konfigurera Dispatcher och Adobe Target.

### Obligatoriska konfigurationer efter installation {#mandatory-post-installation-configurations}

#### Konfigurera RSA- och BouncyCastle-bibliotek  {#configure-rsa-and-bouncycastle-libraries}

Utför följande steg på alla författare- och publiceringsinstanser för att starta delegeringen av biblioteken:

1. Stoppa den underliggande AEM-instansen.
1. Öppna [AEM installationskatalog]\crx-quickstart\conf\sling.properties för redigering.

   Om du använde [AEM installationskatalog]\crx-quickstart\bin\start.bat för att starta AEM redigerar du sling.properties på [AEM_root]\crx-quickstart\.

1. Lägg till följande egenskaper i filen sling.properties:

   ```shell
   sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*
   ```

1. Spara och stäng filen och starta AEM-instansen.
1. Upprepa steg 1-4 för alla författarinstanser och publiceringsinstanser.

#### Konfigurera serialiseringsagenten {#configure-the-serialization-agent}

Utför följande steg på alla Author- och Publish-instanser för att lägga till paketet i tillåtelselista:

1. Öppna AEM Configuration Manager i ett webbläsarfönster. Standardwebbadressen är https://&#39;[server]:[port]&#39;/system/console/configMgr.
1. Sök efter och öppna **Konfiguration av brandvägg för deserialisering**.
1. Lägg till paketet **sun.util.calendar** i fältet **tillåtelselista**. Klicka på Spara.
1. Upprepa steg 1-3 för alla författare- och publiceringsinstanser.

### Ytterligare konfigurationer efter installation {#optional-post-installation-configurations}

#### Installera kompatibilitetspaket {#install-compatibility-package}

Interaktiv kommunikation är standardmetoden och rekommenderas för att skapa kundkommunikation i AEM 6.5 Forms. Om du har uppgraderat eller migrerat från en tidigare version och tänker fortsätta använda brev (Correspondence Management) installerar du [AEMFD-kompatibilitetspaketet](/help/forms/using/compatibility-package.md).

Med AEMFD-kompatibilitetspaketet kan du använda följande resurser från AEM 6.4 Forms, AEM 6.3 Forms och AEM 6.2 Forms på AEM 6.5 Forms:

* Dokumentfragment
* Bokstäver
* Dataordlistor
* Mallar och sidor för anpassade formulär har tagits bort

#### Konfigurera Dispatcher {#configure-dispatcher}

Dispatcher är Adobe Experience Manager verktyg för cachelagring och belastningsutjämning som används med en webbserver i företagsklass. Om du använder [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=sv-SE) ska du göra följande konfigurationer för AEM Forms:

1. Konfigurera åtkomst för AEM Forms:

   Öppna filen dispatcher.any för redigering. Navigera till filteravsnittet och lägg till följande filter i filteravsnittet:

   `/0025 { /type "allow" /glob "* /bin/xfaforms/submitaction*" } # to enable AEM Forms submission`

   Spara och stäng filen. Mer information om filter finns i [Dispatcher-dokumentation](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=sv-SE).

1. Konfigurera tjänsten för refererarfilter:

   Logga in som administratör i konfigurationshanteraren för Apache Felix. Konfigurationshanterarens standard-URL är https://&#39;server&#39;:[port_number]/system/console/configMgr. Välj alternativet **Refererarfilter för Apache Sling** på menyn **Konfigurationer**. I fältet Tillåt värdar anger du värdnamnet för Dispatcher så att det kan användas som referent och klickar på **Spara**. Posten har formatet https://&#39;[server]:[port].

#### Integrera Adobe Target {#integrate-adobe-target}

Kunderna överger troligen interaktiv kommunikation om upplevelsen inte är engagerande. Även om det är frustrerande för kunderna ökar det även supportvolymen och kostnaderna för organisationen. Det är viktigt och utmanande att identifiera och tillhandahålla rätt kundupplevelse som ökar konverteringsgraden. AEM-blanketter är nyckeln till detta problem.

AEM blanketter kan integreras med Adobe Target, en Adobe Experience Cloud-lösning, för att leverera personaliserade och engagerande kundupplevelser i flera digitala kanaler. [Integrera Adobe Target med AEM Forms](../../forms/using/ab-testing-adaptive-forms.md#setupandintegratetargetinaemforms) om du vill anpassa en interaktiv kommunikation med Adobe Target.

#### Konfigurera SSL-kommunikation för formulärdatamodell  {#configure-ssl-communcation-for-form-data-model}

Du kan aktivera SSL-kommunikation för formulärdatamodellen. Om du vill aktivera SSL-kommunikation för formulärdatamodellen ska du lägga till certifikat i Java™ Trust Store för alla instanser innan du startar en AEM Forms-instans. Du kan köra följande kommando för att lägga till certifikaten:

`keytool -import -alias <alias-name> -file <pathTo .cer certificate file> -keystore <<pathToJRE>\lib\security\cacerts>`

## Nästa steg {#next-steps}

Du har konfigurerat en miljö för interaktiv kommunikation och korrespondenshantering. Stegen för att använda funktionen är nu:

* [Översikt över korrespondenshantering](/help/forms/using/interactive-communications-overview.md)

* [Skapa interaktiv kommunikation](../../forms/using/create-interactive-communication.md)

* [Skapa ett brev för korrespondenshantering](../../forms/using/create-letter.md)
