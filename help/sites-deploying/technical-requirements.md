---
title: Tekniska krav
description: En lista över de klient- och serverplattformar som stöds för Adobe Experience Manager.
topic-tags: platform
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
exl-id: f65dd129-9e28-4de1-acca-dd31eaf3c19b
source-git-commit: b9b5492b1bf5f717dec6a48ffbe808bf75cbce6a
workflow-type: tm+mt
source-wordcount: '2981'
ht-degree: 0%

---

# Tekniska krav{#technical-requirements}

Adobe stöder (AEM) Adobe Experience Manager på de plattformar som beskrivs i följande information i det här dokumentet.

Kontakta plattformsleverantören om du har frågor som rör plattformen.

>[!NOTE]
>
>Beroende på vilken plattform du installerar AEM på kan det finnas olika uppsättningar krav för användarhantering.

## Förutsättningar {#prerequisites}

Lägsta krav för installation av Adobe Experience Manager:

* Installerade Java™ Platform, Standard Edition JDK eller andra [Java™ Virtual Machines som stöds](#java-virtual-machines)
* Experience Manager QuickStart-fil (fristående JAR eller WAR för webbapplikationsdistribution)

### Krav för minsta storlek {#minimum-sizing-requirements}

Lägsta krav för Adobe Experience Manager:

* 5 GB ledigt utrymme i installationskatalogen
* 2 GB minne

>[!NOTE]
>
>* Användningsexempel för digitala resurser kräver mer basminne. Mer information finns i [Distribuera och underhålla](/help/sites-deploying/deploy.md#default-local-install).
>* [AEM Forms-tilläggspaket](/help/forms/using/installing-configuring-aem-forms-osgi.md) kräver 15 GB temporärt utrymme.
>

Mer information finns i [Riktlinjerna för maskinvarustorlek](/help/managing/hardware-sizing-guidelines.md).

### Supportnivåer {#support-levels}

I det här dokumentet visas vilka klient- och serverplattformar som stöds för Adobe Experience Manager. Adobe har flera supportnivåer, både för rekommenderade konfigurationer och andra konfigurationer.

### Konfigurationer som stöds {#supported-configurations}

Adobe rekommenderar dessa konfigurationer och ger fullständig support som en del av standardavtalet för programunderhåll.

<table>
 <tbody>
  <tr>
   <td>Supportnivå</td>
   <td>Beskrivning<br /> </td>
  </tr>
  <tr>
   <td><strong>A: Stöds</strong></td>
   <td>Adobe ger fullständig support och underhåll för den här konfigurationen. Den här konfigurationen omfattas av Adobe kvalitetssäkringsprocess.</td>
  </tr>
  <tr>
   <td><strong>R: Begränsat stöd</strong></td>
   <td>För att våra kunder ska lyckas erbjuder Adobe full support i ett begränsat supportprogram, vilket kräver att specifika villkor uppfylls. Support på R-nivå kräver en formell kundförfrågan och en bekräftelse från Adobe. Mer information får du av Adobe kundtjänst.</td>
  </tr>
 </tbody>
</table>

### Konfigurationer som inte stöds {#unsupported-configurations}

| Supportnivå | Beskrivning |
|---|---|
| **Z: Stöds inte** | Konfigurationen stöds inte. Adobe har inga programsatser om huruvida konfigurationen fungerar eller inte. |

## Plattformar som stöds {#supported-platforms}

### Java™ Virtual Machines {#java-virtual-machines}

Programmet kräver att en Java™ Virtual Machine körs, vilket tillhandahålls av Java™ Development Kit-distributionen (JDK).

Adobe Experience Manager fungerar med följande versioner av Java™ Virtual Machines:

>[!CAUTION]
>
>Spåra säkerhetsbulletiner från Java™-leverantören. Detta garanterar produktionsmiljöernas säkerhet och säkerhet. Installera även alltid de senaste Java™-uppdateringarna.

| **Plattform** | **Supportnivå** | **Länk** |
|---|---|---|
| Oracle Java™ SE 17 JDK | A: `[1]` stöds |
| Oracle Java™ SE 21 JDK | A: `[1]` stöds |
| IBM® Semeru J9 VM - bygge 17.0.13.0 | A: `[2]` stöds |
| IBM® Semeru J9 VM - bygge 21.0.6.0 | A: `[2]` stöds |

1. Oracle har övergått till&quot;LTS-modell&quot; (Long Term Support) för Oracle Java™ SE-produkter. Java™ 9, Java™ 10, Java™ 12, Java™ 13, Java™ 14, Java™ 15m Java™ 16 är icke-LTS-versioner från Oracle (se [Oracle Java™ SE support roadmap](https://www.oracle.com/technetwork/java/eol-135779.html)). För att driftsätta AEM i en produktionsmiljö tillhandahåller Adobe endast stöd för LTS-versionerna av Java™. Stöd för och distribution av Oracle Java™ SE JDK, inklusive alla underhållsuppdateringar av LTS-releaser som ligger utanför de offentliga uppdateringarna, stöds av Adobe direkt för alla AEM-kunder som använder Oracle Java™ SE-tekniken. Se [Java™-supportpolicyn för Adobe Experience Manager](assets/Java_Policy_for_Adobe_Experience_Manager.pdf).
   **Den här versionen stöder Oracle Java™ 17 och Oracle Java™ 21.**

1. IBM® JRE stöds endast tillsammans med WebSphere® Application Server.

### Lagring och beständighet {#storage-persistence}

Det finns olika alternativ för att distribuera Adobe Experience Manager-databasen. Se följande lista för de tekniker och lagringsalternativ som stöds.

| **Plattform** | **Beskrivning** | **Supportnivå** |
|---|---|---|
| **Filsystem med TAR-filer** `[1]` | Databas | A: Stöds |
| **Filsystem med datalager** `[1]` | Binärfiler | A: Stöds |
| Lagra binärfiler i TAR-filer i filsystemet `[1]` | Binärfiler | Z: Stöds inte för produktion |
| Amazon S3 | Binärfiler | A: Stöds |
| Microsoft® Azure Blob Storage | Binärfiler | A: Stöds |
| MongoDB Enterprise 6.0 och 7.0 | Databas | A: `[3, 4]` stöds |
| **Apache Lucene (inbyggd Quickstart)** | Söktjänst | A: Stöds |

1. &#39;Filsystem&#39; inkluderar blocklagring som är POSIX-kompatibel. Innehåller nätverkslagringsteknik. Tänk på att filsystemets prestanda kan variera och påverka den övergripande prestandan. Läs in test-AEM med nätverks-/fjärrfilsystemet.
1. MongoDB-delning stöds inte i AEM.
1. MongoDB-lagringsmotorn WiredTiger stöds endast.

>[!NOTE]
>
>MongoDB är ett program från tredje part och ingår inte i AEM licenspaket. Mer information finns på sidan [MongoDB-licenspolicy](https://www.mongodb.com/licensing/server-side-public-license/faq).
>
>För att få ut så mycket som möjligt av er AEM-distribution med MongoDB rekommenderar Adobe att man licensierar MongoDB Enterprise-versionen för att få tillgång till professionell support. Mer information finns i [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md#prerequisites-and-recommendations-when-deploying-aem-with-mongomk).
>
>Licensen innehåller en standarduppsättning av repliker, som består av en primär och två sekundära instanser som kan användas för antingen författaren eller publiceringsdistributionerna.
>
>Om du vill köra både författaren och publicera på MongoDB måste du köpa två separata licenser.
>
>Adobe kundtjänst hjälper dig att hantera kvalificeringsproblem i samband med användningen av MongoDB med AEM.
>
>Mer information finns på sidan [MongoDB för Adobe Experience Manager](https://www.mongodb.com/lp/contact/mongodb-adobe-experience-manager).

### Servletmotorer/programservrar {#servlet-engines-application-servers}

Adobe Experience Manager kan köras antingen som en fristående server (snabbstart-JAR-filen) eller som ett webbprogram i en tredjepartsprogramserver (WAR-filen).

Den lägsta servlet API-version som krävs är Servlet 3.1. Dessutom stöder AEM Jakarta servlet 5 för jar och krig kan köras i programservrar som implementerar Jakarta servlet API 5/6.

| Plattform | Supportnivå |
|---|---|
| **Quickstart inbyggd servermotor (11.0.x)** | A: Stöds |
| IBM® WebSphere® Application Server Continuous Delivery (LibertyProfile) with Web Profile 24.0.0.7 and IBM® Sumeru open JRE® 17/21 | R: Begränsad support för nya kontrakt `[1]` |
| Apache Tomcat 10.0.x/10.1.x | R: Begränsad support för nya kontrakt `[1]` |

1. När AEM 6.5-distributioner startas på programservrar övergår stödet till begränsad support. Befintliga kunder kan uppgradera till AEM 6.5 och fortsätta använda programservrar. För nya kunder innehåller det supportkriterier och ett supportprogram enligt beskrivningen ovan.

### Operativsystem för servrar {#server-operating-systems}

Adobe Experience Manager fungerar med följande serverplattformar för produktionsmiljöer:

| **Plattform** | **Supportnivå** |
|---|---|
| **Linux®, baserat på Red Hat®-distributionen** | A: Stöds `[1]` `[2]` |
| Linux®, baserat på Debian-distribution inkl. Ubuntu | A: `[1]` stöds |
| Linux®, baserat på SUSE®-distribution | A: `[1]` stöds |
| Microsoft® Windows Server 2022 | R: Stöds |

1. Linux® Kernel 5. x och 6. x innehåller derivat från distributionen av Red Hat®, inklusive Red Hat® Enterprise Linux®, CentOS, Oracle Linux® och Amazon Linux®.
1. Linux®-distribution stöds av Adobe Managed Services.

   >[!NOTE]
   >
   >För Linux-baserad server kräver AEM Forms add-on körningsberoenden som:
   >* glibc.x86_64 (2.17-196)
   >* libX11.x86_64 (1.6.7-4)
   >* zlib.x86-64 (1.2.7-17)
   >* libxcb.x86_64 (1.13-1.el7)
   >* libXau.x86_64 (1.0.8-2.1.el7)
   >* glibc-locale.x86_64 (2.17 eller senare)


### Virtuella miljöer och molnmiljöer {#virtual-cloud-computing-environments}

Adobe Experience Manager stöds när det körs i en virtuell dator i molnmiljöer. Dessa miljöer omfattar Microsoft® Azure och Amazon Web Services (AWS), som körs i enlighet med de tekniska krav som anges på den här sidan och i enlighet med Adobe standardsupportvillkor.

Om du har en molnbaserad miljö kan du titta på det senaste erbjudandet från AEM produktlinje: Adobe Experience Manager as a Cloud Service. Mer information finns i [Adobe Experience Manager as a Cloud Service-dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service.html).

Adobe erbjuder även Adobe Managed Services att distribuera AEM på Azure eller AWS. Adobe Managed Services förser experterna med erfarenhet och kunskaper av att driftsätta och använda AEM i dessa molnmiljöer. Se [ytterligare dokumentation om Adobe Managed Services](https://business.adobe.com/products/experience-manager/managed-services.html?aemClk=t).

I alla andra fall där AEM distribueras på Azure eller AWS, eller i någon annan molndatormiljö, finns support från Adobe i den virtuella datormiljön. Den virtuella miljön måste köras i enlighet med de tekniska specifikationer som anges på den här sidan. Alla rapporterade fel som rör AEM som körs i någon av dessa molnmiljöer måste kunna reproduceras oberoende av alla molntjänster som är specifika för molndatormiljön. Det vill säga, om inte molntjänsten stöds som en del av de tekniska krav som anges på den här sidan, till exempel Azure Blob-lagring eller AWS S3.

Adobe rekommenderar att du arbetar direkt med molnleverantören för rekommendationer om hur du distribuerar AEM på Azure eller AWS utanför Adobe Managed Services. Eller i samarbete med Adobe partners som stöder driftsättningen av AEM i den molnmiljö du föredrar. Den valda molnleverantören eller partnern ansvarar för storleksspecifikationer, utformning och implementering av arkitekturen för att uppfylla dina specifika krav på prestanda, belastning, skalbarhet och säkerhet.

### Dispatcher Platforms (webbservrar) {#dispatcher-platforms-web-servers}

Dispatcher är en komponent för cachelagring och lastbalansering. [Hämta den senaste Dispatcher-versionen](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/release-notes.html). Experience Manager 6.5 kräver Dispatcher version 4.3.2 eller senare.

Följande webbservrar kan användas med Dispatcher version 4.3.2:

| Plattform | Supportnivå |
|---|---|
| **Apache httpd 2.4.x** `[1,2]` | A: Stöds |
| Microsoft® IIS 10 (Internet Information Server) | A: Stöds |
| Microsoft® IIS 8.5 (Internet Information Server) | Z: Stöds inte |

1. Webbservrar som byggts utifrån Apache httpd-källkoden har lika mycket stöd som den version av httpd som den baseras på. Om du är osäker kan du be Adobe om en bekräftelse av den supportnivå som gäller respektive serverprodukt. Följande fall:

   1. HTTP-servern byggdes med enbart officiella källdistributioner av Apache, eller
   1. HTTP-servern levererades som en del av det operativsystem där den körs. Exempel: IBM® HTTP Server, Oracle HTTP Server

1. Dispatcher finns inte för Apache 2.4.x för Windows.

## Klientplattformar som stöds {#supported-client-platforms}

### Webbläsare som stöds för redigeringsgränssnittet {#supported-browsers-for-authoring-user-interface}

Adobe Experience Manager användargränssnitt fungerar med följande klientplattformar. Alla webbläsare testas med standarduppsättningen med plugin-program och tillägg.

AEM användargränssnitt är optimerat för större skärmar (vanligen bärbara och stationära datorer) och surfplattor (t.ex. Apple iPad eller Microsoft® Surface). Telefonformfaktorn stöds inte.

>[!NOTE]
>
>**Stöd för webbläsare med snabba releasecykler:**
>
>Uppdateringar för Mozilla Firefox, Google Chrome och Microsoft® Edge var sjätte månad. Adobe tillhandahåller uppdateringar för Adobe Experience Manager för att upprätthålla den supportnivå som anges nedan för kommande versioner av dessa webbläsare.

<table>
 <tbody>
  <tr>
   <td><strong>Webbläsare</strong></td>
   <td><strong>Stöd för användargränssnitt<br /> </strong></td>
   <td><strong>Stöd för Classic UI</strong></td>
  </tr>
  <tr>
   <td><strong>Google Chrome (Evergreen)</strong></td>
   <td>A: Stöds</td>
   <td>A: Stöds</td>
  </tr>
  <tr>
   <td>Microsoft® Edge (Evergreen)</td>
   <td>A: Stöds</td>
   <td>A: Stöds</td>
  </tr>
  <tr>
   <td>Microsoft® Internet Explorer 11</td>
   <td>Z: Stöds inte</td>
   <td>Z: Stöds inte</td>
  </tr>
  <tr>
   <td>Mozilla Firefox (Evergreen)</td>
   <td>A: Stöds</td>
   <td>A: Stöds</td>
  </tr>
  <tr>
   <td>Mozilla Firefox last ESR [1]</td>
   <td>A: Stöds</td>
   <td>A: Stöds</td>
  </tr>
  <tr>
   <td>Apple Safari på macOS (Evergreen)</td>
   <td>A: Stöds</td>
   <td>A: Stöds</td>
  </tr>
  <tr>
   <td>Apple Safari 11.x på macOS</td>
   <td>Z: Stöds inte</td>
   <td>Z: Stöds inte</td>
  </tr>
  <tr>
   <td>Apple Safari på iOS 12.x</td>
   <td>A: Stöds [2]</td>
   <td>Z: Stöds inte</td>
  </tr>
  <tr>
   <td>Apple Safari på iOS 11.x</td>
   <td>Z: Stöds inte</td>
   <td>Z: Stöds inte</td>
  </tr>
 </tbody>
</table>

1. Extended Support Release of Firefox [Läs mer om mozilla.org](https://www.mozilla.org/en-US/firefox/enterprise/)
1. Stöd för Apple iPad

### Webbläsare som stöds för webbplatser {#supported-browsers-for-websites}

I allmänhet är webbläsarstöd för webbplatser som återges av AEM Sites beroende av implementeringen av AEM sidmallar, design och komponentutdata, och det är därför den part som implementerar dessa delar som bestämmer.

## Information om ytterligare plattformar {#additional-platform-notes}

I det här avsnittet finns specialanteckningar och mer detaljerad information om hur du kör Adobe Experience Manager och dess tillägg.

### IPv4 och IPv6 {#ipv-and-ipv}

Alla element i Adobe Experience Manager (Instance, Dispatcher) kan installeras i både IPv4- och IPv6-nätverk.

Åtgärden är smidig eftersom ingen speciell konfiguration krävs. Om det behövs anger du en IP-adress i det format som passar nätverkstypen.

När en IP-adress måste anges kan du välja (efter behov) bland följande:

* En IPv6-adress. Exempel: `https://[ab12::34c5:6d7:8e90:1234]:4502`

* En IPv4-adress. Exempel: `https://123.1.1.4:4502`

* Ett servernamn. Exempel: `https://www.yourserver.com:4502`

* Standardfallet `localhost` tolkas för både IPv4- och IPv6-nätverksinstallationer. Exempel: `https://localhost:4502`

### Krav för AEM Dynamic Media Add-on {#requirements-for-aem-dynamic-media-add-on}

AEM Dynamic Media är inaktiverat som standard. Se här för att [aktivera dynamiska media](/help/assets/config-dynamic.md#enabling-dynamic-media).

När Dynamic Media är aktiverat gäller följande ytterligare tekniska krav.

>[!NOTE]
>
>Dessa systemkrav gäller **endast** om du använder Dynamic Media - hybridläge; Dynamic Media - hybridläge har en inbäddad bildserver, som bara är certifierad på vissa operativsystem.
>
>För Dynamic Media-kunder som kör Dynamic Media - Scene7-läge (d.v.s. körningsläget **dynamicmedia_scene7** ) finns det inga ytterligare systemkrav, utan endast samma systemkrav som AEM. Dynamic Media - Scene7-lägesarkitekturen använder den molnbaserade bildtjänsten och inte den tjänst som är inbyggd i AEM.

#### Maskinvara {#hardware}

Följande maskinvarukrav gäller för både Linux® och Windows:

* Intel Xeon® eller AMD® Opteron CPU med minst fyra kärnor
* Minst 16 GB RAM

#### Linux® {#linux}

Om du använder Dynamic Media i Linux® måste följande krav vara uppfyllda:

* Red Hat® Enterprise 7 eller CentOS 7 och senare med de senaste korrigeringsfilerna
* 64-bitars operativsystem
* Växling inaktiverad (rekommenderas)
* SELinux är inaktiverat (se anm. nedan)

>[!NOTE]
>
>Om språkinställningen är inställd så att LC_CTYPE inte är lika med `en_US.UTF-8` förhindrar den att Dynamic Media fungerar. Om du vill se vilket värde det har skriver du&quot;locale&quot; i kommandotolken. Om den inte är korrekt inställd anger du LC_CTYPE-miljövariabeln till den tomma strängen genom att skriva &quot;export LC_CTYPE=&quot; innan du kör AEM.

>[!NOTE]
>
>**Inaktiverar SELinux:** Bildservern fungerar inte med SELinux aktiverat. Det här alternativet är aktiverat som standard. Du kan åtgärda problemet genom att redigera filen **/etc/selinux/config** och ändra SELinux-värdet från:
>
>`SELINUX=enforcing` **till** `SELINUX=disabled`

>[!NOTE]
>
>**NUMA-arkitektur:** System med processorer med AMD64 och Intel® EM64T är vanligtvis konfigurerade som icke-enhetliga minnesarkitekturplattformar (NUMA). Det innebär att kärnan konstruerar flera minnesnoder vid start i stället för att konstruera en enda minnesnod.
>
>Konstruktionen för flera noder kan resultera i minnesöverbelastning på en eller flera av noderna innan andra noder töms. När minnesöverbelastning inträffar kan kärnan bestämma sig för att avsluta processer (till exempel Image Server eller Platform Server) trots att det finns tillgängligt minne.
>
>Adobe rekommenderar därför att du stänger av NUMA med startalternativet **numa=off** om du kör ett sådant system så att du undviker kerneldödandet.

>[!NOTE]
>
>**Servervärdnamnet måste matcha:** Kontrollera att serverns värdnamn kan matchas till en IP-adress. Om det inte är möjligt lägger du till det fullständiga, kvalificerade värdnamnet och IP-adressen till **/etc/hosts**:
>
>`<ip address> <fully qualified hostname>`

#### Windows {#windows}

* Microsoft® Windows Server 2016
* Växla utrymme motsvarande minst dubbelt så mycket fysiskt minne (RAM)

Om du vill använda Dynamic Media i Windows installerar du Microsoft® Visual Studio 2010, 2013 och 2015 som kan återdistribueras för x64 och x86.

För Windows x64:

* Få Microsoft® Visual Studio 2010 återdistribuerbart på [https://www.microsoft.com/en-us/download/details.aspx?id=26999](https://www.microsoft.com/en-us/download/details.aspx?id=26999)
* Få Microsoft® Visual Studio 2013 återdistribuerbart på [https://www.microsoft.com/en-us/download/details.aspx?id=40784](https://www.microsoft.com/en-us/download/details.aspx?id=40784)
* Få Microsoft® Visual Studio 2015 återdistribuerbart på [https://www.microsoft.com/en-us/download/details.aspx?id=48145](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

För Windows x86:

* Få Microsoft® Visual Studio 2010 återdistribuerbart på [https://www.microsoft.com/en-us/download/details.aspx?id=26999](https://www.microsoft.com/en-us/download/details.aspx?id=26999)
* Få Microsoft® Visual Studio 2013 återdistribuerbart på [https://www.microsoft.com/en-in/download/details.aspx?id=40769](https://www.microsoft.com/en-in/download/details.aspx?id=40769)
* Få Microsoft® Visual Studio 2015 återdistribuerbart på [https://www.microsoft.com/en-us/download/details.aspx?id=52685](https://www.microsoft.com/en-us/download/details.aspx?id=52685)

#### macOS {#macos}

* 10.9.x och senare
* Stöds endast i demos- och testversioner

### Krav för AEM Forms PDF Generator {#requirements-for-aem-forms-pdf-generator}

### Programsupport för PDF Generator {#software-support-for-pdf-generator}

<table>
 <tbody>
  <tr>
   <th><p><strong>Produkt</strong></p> </th>
   <th><p><strong>Format som stöds för konvertering till PDF</strong></p> </th>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/acrobat/release-note/release-notes-acrobat-reader.html">Acrobat 2020 Classic track</a> senaste versionen</td>
   <td>XPS, bildformat (BMP, GIF, JPEG, JPG, TIF, TIFF, PNG, JPF, JPX, JP2, J2K, J2C, JPC), HTML, HTM, DWG, DXF och DWF</td>
  </tr>
  <tr>
   <td>Microsoft® Office 2019</td>
   <td>DOC, DOCX, XLS, XLSX, PPT, PPTX, RTF och TXT</td>
  </tr>
  <tr>
   <td>WordPerfect 2020<br /> </td>
   <td>WP, WPD</td>
  </tr>
  <tr>
   <td>Microsoft® Publisher 2019<br /> </td>
   <td>PUB</td>
  </tr>
  <tr>
   <td>OpenOffice 4.1.10</td>
   <td>ODT, ODP, ODS, ODG, ODF, SXW, SXI, SXC, SXD, XLS, XLSX, DOC, DOCX, PPT, PPTX, bildformat (BMP, GIF, JPEG, JPG, TIF, TIFF, PNG, JPF, JPX, JP2, J2K, JPC, HTML, HTM, RTF och TXT</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>PDF Generator har endast stöd för engelska, franska, tyska och japanska versioner av de operativsystem och program som stöds.
>
>Dessutom
>
>* PDF Generator kräver en 32-bitarsversion av [Acrobat 2020 Classic track version 20.004.30006](https://helpx.adobe.com/acrobat/release-note/release-notes-acrobat-reader.html) eller Acrobat 2017 version 17.011.30078 för att kunna utföra konverteringen.
>* PDF Generator har endast stöd för 32-bitarsversionen av Microsoft® Office Professional Plus och andra program som krävs för konvertering.
>* Installationen av Microsoft® Office Professional Plus kan använda volymlicenser baserade på Retail eller MAK/KMS/AD.
>* Om en Microsoft® Office-installation inaktiveras eller inte licensieras av någon anledning, t.ex. en volymlicensierad installation som inte kan hitta en KMS-värd inom en angiven period, kan konverteringen misslyckas tills installationen har licensierats på nytt och återaktiverats.
>* PDF Generator stöder 32- och 64-bitarsversionerna av OpenOffice i Linux®.
>* PDF Generator stöder inte Microsoft® Office 365.
>* PDF Generator-konverteringar för OpenOffice stöds endast i Windows och Linux®.
>* Funktionerna OCR PDF, Optimize PDF och Export PDF stöds endast i Windows.
>* En version av Acrobat medföljer AEM Forms för att aktivera PDF Generator-funktioner. Programmatiskt få tillgång till den paketerade versionen endast med AEM Forms under AEM Forms-licensens löptid för användning med AEM Forms PDF Generator. Mer information finns i AEM Forms produktbeskrivning enligt din distribution ([On-Premise](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-manager-on-premise.html) eller [Managed Services](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-manager-managed-services.html))
>* PDF Generator-tjänsten stöder inte Microsoft® Windows 10.
>* PDF Generator kan inte konvertera filer med Microsoft® Visio 2019. Du kan fortsätta använda Microsoft® Visio 2016 för att konvertera `.VSD`- och `.VSDX`-filer.
>* PDF Generator kan inte konvertera filer med Microsoft® Project 2019. Du kan fortsätta använda Microsoft® Project 2016 för att konvertera `.VSD`- och `.VSDX`-filer.
>

### Krav för AEM Forms Designer {#requirements-for-aem-forms-designer}

* Microsoft® Windows® 2016 Server, Microsoft® Windows® 2019 Server, Microsoft® Windows® 10 eller Windows® 11
* 1 GHz eller snabbare processor med stöd för PAE, NX och SSE2.
* 1 GB RAM för 32-bitars eller 2 GB RAM för 64-bitars operativsystem
* 16 GB diskutrymme för 32-bitars eller 20 GB för 64-bitars operativsystem
* Grafikminne - 128 MB GPU (256 MB rekommenderas)
* 2,35 GB ledigt hårddiskutrymme
* Bildskärmsupplösning på 1 024 x 768 pixlar eller högre
* Maskinvaruacceleration för video (valfritt)
* Acrobat Pro DC, Acrobat Standard DC eller Adobe Acrobat Reader DC
* Administrativ behörighet för att installera Designer
* Microsoft Visual C++ 2019 (VC 14.28 eller senare) 32-bitars körningsmiljö för 32-bitars AEM Forms Designer
* Microsoft Visual C++ 2019 (VC 14.28 eller senare), 64-bitars körningsmiljö för 64-bitars AEM Forms Designer

[Installera och konfigurera AEM Forms designer](/help/forms/using/installing-configuring-designer.md)

### Krav för återskrivning av AEM Assets XMP-metadata {#requirements-for-aem-assets-xmp-metadata-write-back}

XMP-återskrivningsfunktionen stöds och är aktiverad för följande plattformar och filformat:

* **Operativsystem:**

   * Linux® (stöd för 32-bitars och 32-bitars program i 64-bitarssystem). Anvisningar om hur du installerar 32-bitars klientbibliotek finns i [Så här aktiverar du XMP-extrahering och återskrivning på 64-bitars Red Hat® Linux®](https://helpx.adobe.com/experience-manager/kb/enable-xmp-write-back-64-bit-redhat.html).

   * Windows Server
   * macOS X (64 bitar)

* **Filformat**: JPEG, PNG, TIFF, PDF, INDD, AI och EPS.

### Krav för AEM Assets att bearbeta metadataintensiva resurser i Linux® {#assetsonlinux}

XMPFilesProcessor-processen kräver att biblioteket GLIBC_2.14 fungerar. Använd en Linux®-kärna som innehåller GLIBC_2.14, till exempel Linux®-kärna version 3.1.x. Prestandan för bearbetning av resurser som innehåller en stor mängd metadata förbättras, till exempel PSD-filer. Om du använder en tidigare version av GLIBC uppstår fel i loggar som börjar med `com.day.cq.dam.core.impl.handler.xmp.NCommXMPHandler Failed to read XMP`.
