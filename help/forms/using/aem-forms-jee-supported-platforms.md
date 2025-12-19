---
title: Plattformar som stöds för AEM Forms på JEE
description: Lista över infrastrukturkomponenter som krävs och stöds för installation av AEM Forms i JEE
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
geptopics: SG_AEMFORMS/categories/jee
docset: aem65
role: Admin
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,AEM Forms on JEE,Platform Matrix
source-git-commit: 9a2fca4b6bd2efae268206596a178ad90e8ac80e
workflow-type: tm+mt
source-wordcount: '3094'
ht-degree: 0%

---


# Plattformar som stöds för AEM Forms på JEE {#supported-platforms-for-aem-forms-on-jee}

## Supportnivåer {#support-levels}

AEM Forms på JEE-server kan konfigureras med valfri kombination av operativsystem, programservrar, databaser, databasdrivrutiner, JDK, LDAP-servrar och e-postservrar som stöds.

I det här dokumentet visas vilka klient- och serverplattformar som stöds för AEM Forms på JEE. Adobe har flera supportnivåer, både för Adobe rekommenderade konfigurationer och för andra konfigurationer. I dokumentet listas även andra program som stöds och deras version, undantag, korrigeringsdefinitioner och supportregler för programfixar från tredje part.

>[!NOTE]
>
>- En fullständig lista över undantag för serverplattformar som stöds finns i [Undantag för serverplattformar som stöds](../../forms/using/aem-forms-jee-supported-platforms.md#p-exceptions-to-supported-server-platforms-p).
>- AEM Forms på JEE har endast stöd för engelska, franska, tyska och japanska versioner av de operativsystem och program som stöds.

### Policy för uppgradering och support

#### Full Installer

- **Uppgraderingsstöd för fullständiga installationsprogram**: Ett fullständigt installationsprogram släpps med varje sjätte AEM Service Pack. Fullständiga installationsbaserade uppgraderingar stöds endast från AEM 6.5.23.0.

- **Borttagning och borttagning**: Plattformsstödet uppdateras med varje fullständig installationsversion. Alla program som markerats som borttagna i plattformsmatrisen under en fullständig installationsversion har rätt att tas bort från den plattformsmatris som stöds i en efterföljande fullständig installationsversion, vilket anger att stödet för programmet har upphört.

#### Service Pack

- **Service Pack-täckning**: Adobe ger teknisk support för AEM Forms-miljöer med hjälp av något av de senaste sex servicepaketen. Om din nuvarande version är äldre än de senaste sex servicepaketen rekommenderar Adobe starkt att du uppgraderar till den senaste versionen för optimala prestanda, säkerhet och kontinuerlig support.

- **Riktlinjer för installation av korrigeringsfiler**: När du uppdaterar med hjälp av installationsprogram för korrigeringsfiler måste du kontrollera att den underliggande fullständiga installationsversionen inte är mer än två versioner gammal. Kontrollera till exempel att den underliggande fullständiga installationsversionen är 6.5.19.0 eller 6.5.18.0 under installationen av Service Pack 6.5.12.0.

- **Stöd för korrigeringsuppgradering**: Du kan fortsätta uppgradera till det senaste Service Pack-versionen tills du uppgraderar till de senaste plattformarna som stöds. Det går till exempel att uppgradera från Service Pack 6.5.12.0 till 6.5.19.0, förutsatt att du går över till en plattformskombination som stöds för 6.5.19.0.

### Rekommenderade konfigurationer {#recommendedconfigurations}

Adobe rekommenderar dessa konfigurationer och ger fullständig eller begränsad support som en del av standardavtalet för programunderhåll:

<table>
 <tbody>
  <tr>
   <th>Supportnivå</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>A: Stöds<br /> </td>
   <td>Adobe ger fullständig support och underhåll för den här konfigurationen. Den här konfigurationen omfattas av Adobe kvalitetssäkringsprocess.</td>
  </tr>
  <tr>
   <td>R: Begränsat stöd</td>
   <td>Adobe ger fullständigt stöd för den här konfigurationen när vissa förutsättningar är uppfyllda. Kontakta Adobe support för företag om du vill veta mer om förutsättningarna och begära support.</td>
  </tr>
  <tr>
   <td>L: Begränsat stöd</td>
   <td>Adobe ger fullständigt stöd och underhåll för den här konfigurationen när vissa förutsättningar är uppfyllda. Alla funktioner är inte tillgängliga i konfigurationen. Kontakta Adobe Enterprise Support om du vill veta mer om förutsättningarna och begära support.<br /> </td>
  </tr>
 </tbody>
</table>

### Konfigurationer som inte stöds {#unsupported-configurations}

| Supportnivå | Beskrivning |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| E: Arbetet förväntas | Konfigurationen förväntas fungera och det finns inga rapporter om motsatsen. |
| Z: Stöds inte | Konfigurationen stöds inte. Adobe har inga programsatser om huruvida konfigurationen fungerar eller inte. |

>[!NOTE]
>
>För att hjälpa AEM Forms-kunder att minska ägandekostnaden, förenkla driftsättningsarkitekturen och modernisera utvecklingsstacken går Adobe Experience Manager Enterprise-plattform bort från applikationsserverbaserad driftsättning till förmån för fristående OSGi-baserade driftsättningar. Adobe fortsätter att stödja AEM Forms JEE-stacken med en reducerad matris med infrastrukturkomponenter.
>För nya installationer rekommenderar vi att man där det är möjligt driftsätter AEM Forms i den moderna OSGi-stacken för att använda de senaste innovationerna kring responsiv Adaptiv Forms för mobiler, interaktiv kommunikation i flera kanaler och dataintegrering i serverdelen med hjälp av Form Data Model.
>
>Adobe känner igen att befintliga användare måste fortsätta att distribuera AEM Forms på JEE-stacken. I sådana fall kräver Adobe att AEM Forms JEE distribueras på den infrastruktur som stöds enligt beskrivningen i den här dokumentationen. Om du uppgraderar till AEM 6.5 Forms och använder en plattform som inte stöds i den tidigare AEM Forms-versionen kan du kontakta Adobe support för hjälp med att uppgradera till en plattform som stöds.

### Java™ Virtual Machines (JVM) {#java-virtual-machines-jvm}

Adobe Experience Manager Forms kräver att en Java™ Virtual Machine körs, vilket ingår i Java™ Development Kit-distributionen (JDK). Adobe Experience Manager fungerar med följande versioner av Java™ Virtual Machines:

<table>
 <tbody>
  <tr>
   <th><p><strong>Plattform</strong></p> </th>
   <th><p><strong>Supportnivå</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
   <tr> 
   <td><p>Oracle Java™ SE 21 (64 bitar) <sup> [8] </sup> </p>  </td>
   <td><p>A: Stöds</p> </td>
   <td><p>Mindre releaser och uppdateringar </p> </td>
  </tr>
  <tr> 
   <td><p>Oracle Java™ SE 17 (64 bitar) <sup> [8] </sup> </p>  </td>
   <td><p>A: Stöds</p> </td>
   <td><p>Mindre releaser och uppdateringar </p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>- Spåra säkerhetsbulletiner från Java™-leverantören för att säkerställa säkerheten i produktionsmiljöer och installera de senaste Java™-uppdateringarna.
>- AEM Forms på JEE har endast stöd för 64-bitars JVM i produktionsmiljöer.

### Databaser och CRX Persistence {#databases-and-crx-persistence}

<table>
 <tbody>
  <tr>
   <td><p><strong>Plattform</strong></p> </td>
   <td><p><strong> Beskrivning</strong></p> </td>
   <td><p><strong>Supportnivå</strong></p> </td>
  </tr>
  <tr>
   <td><p>Filsystem</p> </td>
   <td><p>Repository Microkernel (TAR MK-filer)</p> </td>
   <td><p>Stöds</p> </td>
  </tr>
  <tr>
   <td><p> MongoDB Enterprise 9.0 </p> </td>
   <td><p>Databasmikrokernel</p> </td>
   <td><p>Stöds</p> </td>
  </tr>
  <tr>
   <td><p> MongoDB Enterprise 8.0</p> </td>
   <td><p>Databasmikrokernel</p> </td>
   <td><p>Stöds</p> </td>
  </tr>
    <tr>
   <td><p> MongoDB Enterprise 7.0 </p> </td>
   <td><p>Databasmikrokernel</p> </td>
   <td><p>Stöds</p> </td>
  </tr>
   <tr>
   <td>Oracle Database 19c (Standard, Real Application Clusters (RAC) och Enterprise Edition) </td>
   <td>-</td>
   <td>Stöds</td>
  </tr>
  <tr>
   <td><p>Microsoft® SQL Server 2022 </p> </td>
   <td><p>-</p> </td>
   <td><p>Stöds</p> </td>
  </tr>
  <tr>
   <td>MySQL 8.4</td>
   <td>-</td>
   <td>R: Begränsat stöd</td>
  </tr>
 </tbody>
</table>

- MongoDB är en tredjepartsprogramvara som inte ingår i AEM licenspaket. Mer information finns i [MongoDB-licenspolicy](https://www.mongodb.org/about/licensing/).
- För att få ut så mycket som möjligt av er AEM rekommenderar Adobe att man licensierar MongoDB Enterprise-versionen för att få tillgång till professionell support.
- Adobe kundtjänst hjälper dig att hantera kvalificeringsproblem i samband med användningen av MongoDB med AEM. Mer information finns på sidan [MongoDB för Adobe Experience Manager](https://www.mongodb.com/lp/contact/mongodb-adobe-experience-manager).
- &#39;Filsystem&#39; inkluderar blocklagring som är POSIX-kompatibel. Detta inkluderar nätverkslagringsteknik. Tänk på att filsystemets prestanda kan variera och påverka den övergripande prestandan. Vi rekommenderar att du läser in test-AEM med nätverks-/fjärrfilsystemet.
- Endast WiredTiger stöds för lagringsmotorn MongoDB.
- MongoDB-delning stöds inte i AEM.
- Dokumentsäkerhetsmodulen använder inte innehållsdatabas. Det innebär att om du bara använder dokumentsäkerhet och inte tänker använda HTML Workspace-, HTML5-formulär eller adaptiva formulär ska du inte installera Content Repository.

### Databasdrivrutiner {#database-drivers}

<table>
 <tbody>
  <tr>
   <th>Databas </th>
   <th><p><strong>Plattform</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td>MySQL</td>
   <td><p>MySQL Connector/J 8.4</p> </td>
   <td><p>Tillhandahålls med AEM Forms i JEE-installation</p> </td>
  </tr>
  <tr>
   <td>Microsoft® SQL Server<br /> </td>
   <td><p>Microsoft® SQL Server JDBC-drivrutin 12.10.0<br /> </p> <p>sqljdbc12.10.0.jar</p> </td>
   <td><p>Ladda ned från Microsoft® webbplats.</p> </td>
  </tr>
  <tr>
   <td>Oracle</td>
   <td><p>Oracle Database 19.3.0.0.0 JDBC driver</p> <p>jodbc8.jar (version 19.3.0.0.0)<br /> </p> </td>
   <td><p>Hämta från <a href="https://www.oracle.com/database/technologies/appdev/jdbc-ucp-19c-downloads.html">Oracle webbplats</a>.</p> </td>
  </tr>
 </tbody>
</table>

### Programservrar {#application-servers}

<table>
 <tbody>
  <tr>
   <td><p><strong> Plattform</strong></p> </td>
   <td><p><strong>Supportnivå</strong></p> </td>
   <td><p><strong>Patch-definitioner som stöds</strong></p> </td>
  </tr>
  <tr>
   <td><p>JBoss® Enterprise Application Platform (EAP) 8.0.6 <sup>[2] [3] [7]</sup> </p> </td>
   <td><p>A: Stöds</p> </td>
   <td><p>Patchar och kumulativa patchar för den EAP-version som stöds</p> </td>
  </tr>
 </tbody>
</table>

### Serveroperativsystem {#server-operating-systems}

#### Produktionsmiljöer {#production-environments}

<table>
 <tbody>
  <tr>
   <th><p><strong> Plattform</strong></p> </th>
   <th><p><strong>Supportnivå</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
     <tr>
   <td>Microsoft® Windows Server 2022 (64-bitars)</td>
   <td>A: Stöds</td>
   <td>Servicepaket och viktiga uppdateringar</td>
  </tr>
  <tr>
   <td>Ubuntu 20.04</td>
   <td>A: Stöds</td>
   <td>Servicepaket och viktiga uppdateringar</td>
  </tr>
  <tr>
   <td><p>Red Hat® Enterprise Linux® 9 (Kernel 5.x) (64-bitars)</p> </td>
   <td><p>A: Stöds</p> </td>
   <td><p>Mindre releaser, kumulativa uppdateringar och viktiga uppdateringar</p> </td>
  </tr>
  <tr>
   <td><p>Red Hat® Enterprise Linux® 8 (Kernel 4.x) (64-bitars)</td>
   <td><p>A: Stöds</p> </td>
   <td><p>Mindre releaser, kumulativa uppdateringar och viktiga uppdateringar</p> </td>
  </tr>
  <tr>
   <td><p>SUSE® Linux® Enterprise Server 15-SP6 (64-bitars)</p> </td>
   <td><p>A: Stöds</p> </td>
   <td><p>Service Pack, kumulativa patchar och viktiga säkerhetsuppdateringar</p> </td>
  </tr>
 </tbody>
</table>

#### Virtualiserad miljö {#virtualized-environment}

Du kan köra AEM Forms på JEE på en fysisk dator eller en virtuell miljö. Om du råkar ut för något problem med AEM Forms i en virtuell miljö kan du försöka replikera problemet på en fysisk dator. Om problemet kvarstår på den fysiska datorn kontaktar du Adobe support för att få en lösning. Kontakta din leverantör av virtuell miljö om du inte kan replikera på en fysisk dator.

#### Utvecklingsmiljöer {#development-environments}

<table>
 <tbody>
  <tr>
   <th><p><strong>Plattform (grundversion)</strong></p> </th>
   <th>Supportnivå</th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td><p>Microsoft® Windows® 10 64-bitars</p> </td>
   <td>E: Arbetet förväntas</td>
   <td><p>Service Pack och viktiga uppdateringar</p> </td>
  </tr>
 </tbody>
</table>

### Undantag för serverplattformar som stöds {#exceptions-to-supported-server-platforms}

Tänk på följande undantag när du väljer en plattform för att konfigurera AEM Forms på JEE-servern.

1. CRX-databasen stöder beständighet av typen tarMK och MongoDB.
1. AEM Forms på JEE stöder inte rollbaserad åtkomstkontroll JBoss® (RBAC).

<!-- 
1. [!DNL Microsoft&reg; Windows Server 2019] does not support [!DNL MySQL 5.7] and [!DNL JBoss&reg; EAP 7.1], [!DNL Microsoft&reg; Windows Server 2019] does not support turnkey installations for [!DNL Experience Manager Forms Service Pack 6.5.10.0 and later]. (CQDOC-18312) 
-->

Tänk dessutom på följande när du väljer program för Adobe AEM Forms i JEE-distributioner:

- AEM Forms på JEE har stöd för uppdateringar, patchar och korrigeringspaket utöver den angivna större och mindre versionen av den programvara som stöds. Uppdatering till nästa större eller mindre version stöds dock inte om det inte anges.
- Klusterbaserade installationer stöder inte TjärMK-beständighet. Mer information om stöd för beständighet finns i [Välja en beständig typ för en AEM Forms-installation](/help/forms/using/choosing-persistence-type-for-aem-forms.md).
- AEM Forms på JEE stöder olika tredjepartsprogram enligt Adobe [Supportpolicy för tredjepartsprogram](../../forms/using/aem-forms-jee-supported-platforms.md#p-third-party-patch-support-policy-p).
- AEM Forms på JEE-supportplattformar enligt support från tredjepartsleverantörer. Vissa kombinationer kanske inte tillåts av tredjepartsleverantörer. Till exempel har många leverantörer inte certifierat sina programservrar med Oracle. Därför stöder inte AEM Forms på JEE dessa kombinationer. Se även till att du väljer vilka programversioner som stöds i supportmatrisen för tredjepartsleverantörer.
- AEM Forms på JEE stöder inte TjärMK Cold Standby.
- AEM Forms på JEE har inte stöd för vertikal klustring.
- AEM Forms på JEE stöder inte MySQL-databaser i en klustrad miljö.
- En lista över borttagna eller uppdaterade plattformar finns i dokumentet [AEM 6.5 Forms New Feature Summary](../../forms/using/whats-new.md) .

### LDAP-servrar (tillval) {#ldap-servers-optional}

<table>
 <tbody>
  <tr>
   <th><p><strong>Produkt (grundversion)</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td>Microsoft® Active Directory 2022</td>
   <td>Underhållsrelease och programfix</td>
  </tr>
  <tr>
   <td><p>IBM® Tivoli Directory Server 6.4</p> </td>
   <td><p>Funktionspaket och tillfälliga korrigeringar</p> </td>
  </tr>
 </tbody>
</table>

### E-postservrar (valfritt) {#email-servers-optional}

| Produkt |
| ----------------------- |
| Microsoft® Exchange 2013 |
| Microsoft® Office 365 |

### Innehållshanterare och motsvarande kopplingar {#content-managers-and-corresponding-connectors}

<table>
 <tbody>
  <tr>
   <td><strong>Produkt<br /> </strong></td>
   <td><strong>Version</strong></td>
  </tr>
  <tr>
   <td>EMC Documentum®</td>
   <td>7,3</td>
  </tr>
  <tr>
   <td>IBM® FileNet</td>
   <td>5.5.2</td>
  </tr>
  <tr>
   <td>IBM® Content Manager Server (borttagen) </td>
   <td>8.5 Fixpaket 2</td>
  </tr>
  <tr>
   <td> IBM® Content Manager Client</td>
   <td>8,7 </td>
  </tr>
  <tr>
   <td> IBM® Content Manager Client (borttagen)</td>
   <td>8,5 </td>
  </tr>
   <td>Microsoft® Sharepoint </td>
   <td>2019<br /> </td>
  </tr>
 </tbody>
</table>

### Stöd för Cordova {#support-for-cordova}

AEM Forms App har nu stöd för Apache Cordova. Följande plattformsspecifika versioner av Cordova stöds:

- Apache Cordova 6.4.0
- Cordova iOS 4.3.0
- Cordova Android™ 6.0.0
- Cordova Windows 4.4.3

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
   <td>Microsoft® Project 2019<br /> </td>
   <td>MPP</td>
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
>Dessutom:
>
>- PDF Generator har endast stöd för 32-bitarsversionen av Microsoft® Office Professional Plus och andra program som krävs för konvertering.
>- Installationen av Microsoft® Office Professional Plus kan använda volymlicenser baserade på Retail eller MAK/KMS/AD.
>- Om en Microsoft® Office-installation inaktiveras eller inte licensieras av någon anledning, t.ex. en volymlicensierad installation som inte kan hitta en KMS-värd inom en angiven period, kan konverteringen misslyckas tills installationen har licensierats på nytt och återaktiverats.
>- PDF Generator stöder inte Microsoft® Office 365.
>- PDF Generator-konverteringar för OpenOffice stöds endast i Windows och Linux®.
>- Funktionerna OCR PDF, Optimize PDF och Export PDF stöds endast i Windows.
>- En version av Acrobat medföljer AEM Forms för att aktivera PDF Generator-funktioner. Programmeringsversionen får endast användas med AEM Forms under AEM Forms-licensens löptid för användning med AEM Forms PDF Generator. Mer information finns i AEM Forms produktbeskrivning för din distribution ([On-Premise](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-manager-on-premise.html) eller [Managed Services](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-manager-managed-services.html))
>- PDF Generator-tjänsten stöder inte Microsoft® Windows 10.
>- PDF Generator kan inte konvertera filer med Microsoft® Visio 2019.
>- PDF Generator kan inte konvertera filer med Microsoft® Project 2019.

<!-- Removed lines: >- PDF Generator fails to convert files using Microsoft&reg; Visio 2019. You can continue to use Microsoft&reg; Visio 2016 to convert .VSD and .VSDX files.
>- PDF Generator fails to convert files using Microsoft&reg; Project 2019. You can continue to use Microsoft&reg; Project 2016 to convert .MPP files.-->

### Undantag från tillgänglighetsstöd {#exceptions-to-accessibility-support}

Följande delsystem i AEM Forms är inte [508](https://www.section508.gov/)-kompatibla:

- Anpassat gränssnitt för Forms-redigering
- Redigeringsgränssnittet för Forms Manager
- Redigeringsgränssnitt för korrespondenshantering
- Administratörsgränssnitt (administrationskonsolens användargränssnitt)

## Systemkrav för AEM Forms i JEE {#system-requirements-for-aem-forms-on-jee}

### Lägsta maskinvarukrav {#minimum-hardware-requirements}

<table>
 <tbody>
  <tr>
   <td>Plattform</td>
   <td>Minsta maskinvarukrav</td>
  </tr>
  <tr>
   <td>Microsoft® Windows Server</td>
   <td>Intel Xeon® E5-2680, 2,4 GHz-processor eller motsvarande <br /> VMWare ESX 5.1 eller senare<br /> RAM: 6 GB (64-bitars operativsystem med 64-bitars JVM) <br /> Ledigt diskutrymme: 15 GB temporärt utrymme plus 22 GB <br /> för AEM Forms på JEE</td>
  </tr>
  <tr>
   <td>SUSE® Linux® Enterprise Server</td>
   <td>Intel Xeon® E5-2670v2, 1 vCPU, 2,5 GHz-processor<br /> AWS m3.medium (3 ecu)<br /> RAM: 6 GB (64-bitars operativsystem med 64-bitars JVM) <br /> Ledigt diskutrymme: 6 GB temporärt utrymme plus 22 GB <br /> för AEM Forms på JEE</td>
  </tr>
  <tr>
   <td>Red Hat® Enterprise Linux®</td>
   <td>Intel Xeon® E5-2670v2, 1 vCPU, 2,5 GHz-processor<br /> AWS m3.medium (3 ecu)<br /> RAM: 6 GB (64-bitars operativsystem med 64-bitars JVM) <br /> Ledigt diskutrymme: 6 GB temporärt utrymme plus 22 GB <br /> för AEM Forms på JEE <br /> </td>
  </tr>
  <tr>
   <td>Maskinvarukrav för en liten produktionsmiljö</td>
   <td>
    <ul>
     <li><strong>Intel®-baserad miljö</strong>: Intel Xeon® E5-2680, 2,4 GHz eller högre. Om du använder en processor med dubbla kärnor förbättras prestandan ytterligare</li>
     <li><strong>Minne: </strong> 4 GB <br /> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

Ytterligare krav finns i:

- [Systemkrav för en enda server-AEM Forms för JEE-distribution](https://www.adobe.com/go/learn_aemforms_sysreq_single_65)
- [Systemkrav för en klustrad AEM Forms för JEE-distribution](https://www.adobe.com/go/learn_aemforms_sysreq_cluster_65)

### Adobe Acrobat och Adobe Reader {#adobe-acrobat-and-adobe-reader}

<table>
 <tbody>
  <tr>
   <th><p><strong>Acrobat och Adobe Reader (bas)</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td>Acrobat 2020 (klassiskt spår)</td>
   <td>Version 20.004.30006 eller senare<br /> </td>
  </tr>

</tbody>
</table>

>[!NOTE]
>
>Acrobat DC-produktfamiljen innehåller två spår för både Acrobat och Reader som är olika produkter:&quot;Classic&quot; och&quot;Continuous&quot;. Mer information och en jämförelse av de två spåren finns på [https://www.adobe.com/devnet-docs/acrobatetk/tools/AdminGuide/whatsnewdc.html](https://www.adobe.com/devnet-docs/acrobatetk/tools/AdminGuide/whatsnewdc.html).

## Klienter som stöds för AEM Forms i JEE {#supported-clients-for-aem-forms-on-jee}

### Workbench {#workbench}

<table>
 <tbody>
  <tr>
   <th><p><strong>Plattform</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td><p>Microsoft® Windows® 10 (Enterprise, Pro, Basic)</p> <p>32- eller 64-bitarsversion</p> <p> </p> </td>
   <td>Servicepaket och viktiga uppdateringar</td>
  </tr>
  <tr>
   <td>Microsoft® Windows® 2022 Server</td>
   <td>Servicepaket och viktiga uppdateringar</td>
  </tr>
 </tbody>
</table>

- Diskutrymme för installation: 1,7 GB för Workbench, 2,7 GB på en enda enhet för en fullständig installation av Workbench, Designer och exempelpaketet innehåller 400 MB för temporära installationskataloger - 200 MB i användarens temp-katalog och 200 MB i Windows temporära katalog. Om alla dessa platser finns på en enda enhet måste det finnas 1,5 GB ledigt utrymme under installationen. De filer som kopieras till de tillfälliga katalogerna tas bort när installationen är klar.

- Minne för att köra Workbench: 2 GB RAM
- Maskinvarukrav: Intel® Pentium® 4 eller AMD®-motsvarighet, 1 GHz processor
- Minst 1 024 × 768 pixlar eller högre bildskärmsupplösning med 16-bitars färg eller högre
- TCP/IPv4- eller TCP/IPv6-nätverksanslutning till AEM Forms på JEE-server
- Du måste ha administratörsbehörighet för att installera Workbench i Windows. Om du installerar med ett icke-administratörskonto uppmanas du att ange autentiseringsuppgifter för ett lämpligt konto.

### Designer {#designer}

- Microsoft® Windows® 2016 Server, Microsoft® Windows® 2019 Server, Microsoft® Windows® 10 eller Windows® 11
- 1 GHz eller snabbare processor med stöd för PAE, NX och SSE2.
- 1 GB RAM för 32-bitars eller 2 GB RAM för 64-bitars operativsystem
- 16 GB diskutrymme för 32-bitars eller 20 GB för 64-bitars operativsystem
- Grafikminne - 128 MB GPU (256 MB rekommenderas)
- 2,35 GB ledigt hårddiskutrymme
- Bildskärmsupplösning på 1 024 x 768 pixlar eller högre
- Maskinvaruacceleration för video (valfritt)
- Acrobat Pro DC, Acrobat Standard DC eller Adobe Acrobat Reader DC
- Administrativ behörighet för att installera Designer
- Microsoft® Visual C++ 2019 (VC 14.28 eller senare) 32-bitars runtime

### Webbläsare {#browsers}

#### Stationära datorer {#desktops}

<table>
 <tbody>
  <tr>
   <th><p><strong>Webbläsare (bas)</strong></p> </th>
   <th><p><strong>Supportnivå</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td><p>Microsoft® Edge (Evergreen)</p> </td>
   <td><p>A: Stöds</p> </td>
   <td><p>Service Pack och uppdateringar</p> </td>
  </tr>
  <tr>
   <td><p>Mozilla Firefox (Evergreen)</p> </td>
   <td><p>A: Stöds</p> </td>
   <td>Alla uppdateringar</td>
  </tr>
  <tr>
   <td>Mozilla Firefox ESR</td>
   <td>E: Arbetet förväntas</td>
   <td> Alla uppdateringar</td>
  </tr>
  <tr>
   <td><p>Google Chrome (Evergreen)</p> </td>
   <td><p>A: Stöds</p> </td>
   <td>Alla uppdateringar</td>
  </tr>
  <tr>
   <td>Apple Safari på macOS</td>
   <td>A: Stöds</td>
   <td>Alla uppdateringar</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Vissa webbläsarrelaterade undantag för stationära datorer är följande:
>
>- Safari stöds bara på Macintosh OS X.
>- Workspace stöder Safari 5.1 i Macintosh OS X 10.6 och 10.7 med Acrobat DC eller senare. Mer information om Safari 5.1-kompatibilitet med Adobe Reader, Acrobat, finns på [https://helpx.adobe.com/x-productkb/multi/safari-5-1-incompatible-reader.html](https://helpx.adobe.com/x-productkb/multi/safari-5-1-incompatible-reader.html).
>- Administration Console stöds inte i Safari.
>- Correspondence Management stöder inte Windows® Internet Explorer 9.0 för AEM 6.1-formulär.
>- Forms Portal stöder JAWS 14.0 skärmläsarprogram i Internet Explorer 11.

#### Mobila kunder {#mobile-clients}

<table>
 <tbody>
  <tr>
   <th><p><strong>Webbläsare (bas)</strong></p> </th>
   <th><p><strong>Patch-definitioner som stöds</strong></p> </th>
  </tr>
  <tr>
   <td><p>Chrome på Android™ 4.1.2 och senare</p> </td>
   <td><p>Alla uppdateringar</p> </td>
  </tr>
  <tr>
   <td>Safari på iOS 15.1 och senare</td>
   <td>Alla uppdateringar</td>
  </tr>
  <tr>
   <td>Microsoft® Edge<br /> </td>
   <td>Alla uppdateringar<br /> </td>
  </tr>
  <tr>
   <td>Inbyggd Android™-webbläsare i Android™ 4.4 och senare</td>
   <td>Alla uppdateringar</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>- Forms Portal stöds endast i Safari på iPad.

### AEM Forms {#aem-forms-workspace-app}

#### Stöd för mobila enheter {#mobile-device-support}

AEM Forms finns på följande plattformar:

| **Plattform** | **Enheter som stöds** |
| --- | --- |
| Apple iOS | Apple iPhone, iPad, iPad Air och iPad mini med iOS 15.1 och senare. |
| Google Android™ | Android™ 5.1 och senare. AEM Forms-appen är certifierad på 7-tums och 10-tums Samsung Galaxy-surfplattor och populära smarttelefoner. |
| Microsoft® Windows | Microsoft® Surface-enheter, surfplattor, bärbara datorer och stationära datorer med operativsystemet Microsoft® Windows 10. |

### Adobe Document Security Extension for Microsoft® Office {#adobe-rights-management-extension-for-microsoft-office}

Klicka [här](https://www.adobe.com/products/livecycle/rightsmanagement/extension/downloads.html) för att se systemkraven för Adobe Document Security Extension för Microsoft® Office.

### Undantag från klientsupport {#exceptions-to-client-support}

AEM Forms på JEE har stöd för uppdateringar, patchar och korrigeringspaket utöver den angivna större och mindre versionen av den programvara som stöds. Uppdatering till nästa större eller mindre version stöds dock inte om det inte anges.

## Supportpolicy för korrigeringsstöd från tredje part {#third-party-patch-support-policy}

Tredjepartskraven för AEM Forms på JEE beskrivs i avsnittet Systemkrav i respektive produktdokument. Få tillgång till all dokumentation från [https://adobe.com/go/learn_aemforms_documentation_65](https://adobe.com/go/learn_aemforms_documentation_65) .

AEM Forms på JEE:s referensplattformar från tredje part anger den specifika korrigeringsnivån för tredjepartsinfrastruktur som var aktuell under utvecklingen och lanseringen av AEM Forms på JEE, och den minsta korrigerings-/servicepaketnivån för den infrastruktur som stöds av den versionen av AEM Forms på JEE.

Adobe stöder brådskande eller rekommenderade korrigeringsfiler som utfärdas av tredjepartsleverantörer när de släpps, förutsatt att tredjepartsleverantörer garanterar bakåtkompatibilitet med de versioner som AEM Forms på JEE stöder. Adobe stöder endast korrigeringsfiler som släpps efter den miniminivå som anges i AEM Forms för JEE-dokumentationen.

Ibland saknar Adobe stöd för tredjepartsuppdateringar som ändrar viktiga funktioner och därför inte stöder fullständig bakåtkompatibilitet. Mer information om vilka uppdateringar som stöds finns i [Patchdefinitioner som stöds](https://helpx.adobe.com/aem-forms/aem-forms-third-party-software-patch.html) för specifika leverantörsprodukter och korrigeringstyperna som Adobe stöder.

Under omständigheter som ligger utanför Adobe kontroll kan korrigeringsfiler från tredje part som gör anspråk på bakåtkompatibilitet ha negativ inverkan på Adobe produkter eller kundmiljöer. I sådana fall rekommenderar Adobe att man utvärderar effekten av brådskande korrigeringar från tredje part innan man lägger in dem i viktiga system. Adobe samarbetar med tredje part genom att göra rimliga ansträngningar för att lösa sådana problem, antingen genom vanliga Adobe-supportprogram eller genom att tredje part åtgärdar problemet i lagningen. Detta garanterar inte att en nyligen släppt korrigeringsfil från tredje part som stöds av Adobe fungerar som den dokumenterats av leverantören eller med AEM Forms på JEE.

Adobe förbehåller sig rätten att ändra de referensplattformar från tredje part som stöds av en AEM Forms on JEE-release och de korrigeringsdefinitioner som stöds vid varje given tidpunkt.

Ytterligare information om patchar från tredje part finns också på Adobe Enterprise Support-webbplatsen för kunskapsdatabasartiklar om din produkt.

<!--

## Platform updates {#platform-updates}

The following platforms are marked as deprecated with AEM Forms 6.5.18.0 release on August 31, 2023:

- Microsoft&reg; Windows Server 2019 (64-bit)
- Microsoft&reg; Active Directory 2016

The following platforms are marked as deprecated with AEM Forms 6.5.13.0 release on June 2, 2022:
- Microsoft&reg; SharePoint 2016

The following platforms are marked as deprecated with AEM Forms 6.5.10.0 release on September 7, 2021:

- Adobe Acrobat 2017 - [Core support for Adobe Acrobat 2017 ends on June 6, 2022](https://helpx.adobe.com/support/programs/eol-matrix.html).
- Red Hat&reg; Enterprise Linux&reg; 7 (Kernel 3.x) (64-bit)
- Microsoft&reg; Windows Server 2016 (64-bit) 
- Microsoft&reg; Office 2016
- OpenOffice 4.1.2

-->


<!--## Revision History {#revision-history}-->

<!--

- 6.5.18.0 (Aug 31, 2023)
  - **Added support**: [!DNL Adobe Experience Manager Forms] on JEE has added support for the following platforms:
    - MongoDB Enterprise 4.4
    - Oracle WebLogic Server 14c
    - My SQL JDBC connector 8
    - Active Directory 2022
    - Microsoft&reg; Windows Server 2022 (64-bit)

  - **Removed support**: [!DNL Adobe Experience Manager Forms] on JEE has removed support for the following platforms:
    - Windows Server 2016 (64-bit)
    - MongoDB Enterprise 4.0
    - Oracle Database 12c Release 2 (12.2.0.1.0)
    - MySQL 5.7.35
    - Microsoft&reg; SQL Server 2016
    - JBoss&reg; EAP 7.1.4
    - My SQL JDBC connector 5.1.44
    - Microsoft&reg; SQL Server JDBC driver 6.2.1.0
    - Microsoft&reg; SQL Server JDBC driver 6.2.2.0
    - Microsoft&reg; JDBC Driver 8.x for SQL Server

    The release has also removed support for the following platforms for PDF Generator and in-general:
    - Microsoft&reg; Sharepoint 2016
    - Microsoft&reg; Office 2016
    - Microsoft&reg; Office Visio 2016 
    - Microsoft&reg; Publisher 2016
    - Microsoft&reg; Project 2016
    - OpenOffice 4.1.2
    - Acrobat 2017 (Classic track) Version 17.011.30078 or later

  - **Deprecated support**: [!DNL Adobe Experience Manager Forms] on JEE has deprecated the following platforms:
    - Microsoft&reg; Windows Server 2019 (64-bit)
    - Microsoft&reg; Active Directory 2016
    
- 6.5.13.0 (June 2, 2022)

  The following platforms are deprecated with AEM Forms 6.5.13.0 release on:
 
  - Microsoft&reg; SharePoint 2016

- 6.5.12.0 (March 3, 2022)

    - **Platform Updates**: [!DNL Adobe Experience Manager Forms] on JEE has removed support for the following platforms:
      - IBM&reg; J9 Virtual Machine (build 2.8, JRE 1.8.0)
      - Oracle Database 12c Release 1
      - Oracle Database 18c
      - Oracle Unified Directory (OUD) 11g Release 2
      - IBM&reg; Lotus Domino 9.0
      - IBM&reg; FileNet 5.2
      - Adobe Flash Player

    - **Platform Updates**: [!DNL Adobe Experience Manager Forms] on JEE has deprecated the following platforms:

      - MongoDB Enterprise 4.0
      - MongoDB Enterprise 4.2
      - IBM&reg; DB2&reg; 11.1
      - Oracle Database 12c Release 2
      - MySQL 5.7.35
      - Microsoft&reg; SQL Server JDBC driver 6.2.1.0
      - JBoss&reg; Enterprise Application Platform (EAP) 7.1.4
      - IBM&reg; Content Manager Server 8.5 Fix pack 2
      - IBM&reg; Content Manager Client 8.5
      - Microsoft&reg; SQL Server 2016

- 6.5.10.0 (Sep 01, 2022)

  - **Added support**: [!DNL Adobe Experience Manager Forms] on JEE has added support for the following platform:
  
    - Oracle Java&trade; SE 11 (64 bit) SDK for application server JBoss&reg; EAP 7.4.
  - **Deprecated support**: [!DNL Adobe Experience Manager Forms] on JEE has deprecated the following platforms:

    - Adobe Acrobat 2017 - [Core support for Adobe Acrobat 2017 ends on June 6, 2022](https://helpx.adobe.com/support/programs/eol-matrix.html).
    - Red Hat&reg; Enterprise Linux&reg; 7 (Kernel 3.x) (64-bit)
    - Microsoft&reg; Windows Server 2016 (64-bit) 
    - Microsoft&reg; Office 2016
    - OpenOffice 4.1.2


-->
<!--
### Release 6.5.19.1 (Dec 15, 2023)

| Added Support | Removed Support | Deprecated Support |
| -------------- | --------------- | ------------------- |
| MongoDB Enterprise 6.0 |MongoDB Enterprise 4.4   |  |
| MongoDB Enterprise 5.0 |  |  |
|  | |  |

### Release 6.5.18.0 (Aug 31, 2023)

| Added Support | Removed Support | Deprecated Support |
| -------------- | --------------- | ------------------- |
| MongoDB Enterprise 4.4 | Windows Server 2016 (64-bit) | Microsoft&reg; Windows Server 2019 (64-bit) |
| Oracle WebLogic Server 14c | MongoDB Enterprise 4.0 | Microsoft&reg; Active Directory 2016 |
| My SQL JDBC connector 8 | Oracle Database 12c Release 2 (12.2.0.1.0) |  |
| Active Directory 2022 | MySQL 5.7.35 |  |
| Microsoft&reg; Windows Server 2022 (64-bit) | Microsoft&reg; SQL Server 2016 |  |
|  | JBoss&reg; EAP 7.1.4 |  |
|  | My SQL JDBC connector 5.1.44 |  |
|  | Microsoft&reg; SQL Server JDBC driver 6.2.1.0 |  |
|  | Microsoft&reg; SQL Server JDBC driver 6.2.2.0 |  |
|  | Microsoft&reg; JDBC Driver 8.x for SQL Server |  |
|  |  |  |
|  | **Removed Support (PDF Generator and In-General):** |  |
|  | Microsoft&reg; Sharepoint 2016 |  |
|  | Microsoft&reg; Office 2016 |  |
|  | Microsoft&reg; Office Visio 2016 |  |
|  | Microsoft&reg; Publisher 2016 |  |
|  | Microsoft&reg; Project 2016 |  |
|  | OpenOffice 4.1.2 |  |
|  | Acrobat 2017 (Classic track) Version 17.011.30078 or later |  |


### Release 6.5.13.0 (June 2, 2022)

| Added Support | Removed Support | Deprecated Support |
| -------------- | --------------- | ------------------- |
|  |  | Microsoft&reg; SharePoint 2016 |


### Release 6.5.12.0 (March 3, 2022)

| Added Support | Removed Support | Deprecated Support |
| -------------- | --------------- | ------------------- |
|  | IBM&reg; J9 Virtual Machine (build 2.8, JRE 1.8.0) | MongoDB Enterprise 4.0 |
|  | Oracle Database 12c Release 1 | MongoDB Enterprise 4.2 |
|  | Oracle Database 18c | IBM&reg; DB2&reg; 11.1 |
|  | Oracle Unified Directory (OUD) 11g Release 2 | Oracle Database 12c Release 2 |
|  | IBM&reg; Lotus Domino 9.0 | MySQL 5.7.35 |
|  | IBM&reg; FileNet 5.2 | Microsoft&reg; SQL Server JDBC driver 6.2.1.0 |
|  | Adobe Flash Player | JBoss&reg; Enterprise Application Platform (EAP) 7.1.4 |
|  | | IBM&reg; Content Manager Server 8.5 Fix pack 2 |
|  | | IBM&reg; Content Manager Client 8.5 |
|  | | Microsoft&reg; SQL Server 2016 |
|  | | Microsoft&reg; Windows Server 2016 |

### Release 6.5.10.0 (September 01, 2022)

| Added Support | Removed Support | Deprecated Support |
| -------------- | --------------- | ------------------- |
| Oracle Java&trade; SE 11 (64 bit) SDK for application server JBoss&reg; EAP 7.4. | | [Adobe Acrobat 2017 - Core support for Adobe Acrobat 2017 ends on June 6, 2022.](https://helpx.adobe.com/support/programs/eol-matrix.html)|
|  | Red Hat&reg; Enterprise Linux&reg; 7 (Kernel 3.x) (64-bit)| |
|  | | Microsoft&reg; Windows Server 2016 (64-bit)|
|  | | Microsoft&reg; Office 2016 |
|  | | OpenOffice 4.1.2 |


>[!NOTE]
>
> A deprecated platform continue to receive support until the next full installer release or until third-party vendor support for the platform reaches its end-of-life, whichever is earlier.
-->
<!-- 
- Oct 10, 2021

  - Changed supported version of iOS for AEM Forms App to iOS 15.1. The previous version was iOS 12.

- Sep 07, 2021
  - **Platform Updates**: [!DNL Adobe Experience Manager Forms] on JEE has added support for the following platforms:
    - [!DNL Adobe Acrobat 2020]
    - [!DNL Ubuntu 20.04]
    - [!DNL Open Office 4.1.10]
    - [!DNL Microsoft&reg;&reg; Office 2016]
    - [!DNL Microsoft&reg;&reg; Windows Server 2016]
    - [!DNL RHEL8]

- Dec 03, 2020
  - Support added with AEM Forms 6.5.7.0 or later for the following platform:
    - [!DNL Microsoft&reg;&reg; SQL Server 2019]

- Sep 09, 2020

    - Changed supported version of iOS for AEM Forms App to iOS 12. The previous version was iOS 11.

-->
