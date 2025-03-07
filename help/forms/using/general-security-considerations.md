---
title: Allmänna säkerhetsfrågor för AEM Forms i JEE
description: Lär dig hur du förbereder dig för att härska din AEM Forms i JEE-miljö.
content-type: reference
topic-tags: Security
products: SG_EXPERIENCEMANAGER/6.4
docset: aem65
hide: true
hidefromtoc: true
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Document Security,Adaptive Forms
exl-id: 5d0ba475-06be-47fd-8ea2-9160c2d86b07
source-git-commit: 060bb23d64a90f0b2da487ead4c672cbf471c9a8
workflow-type: tm+mt
source-wordcount: '1030'
ht-degree: 0%

---

# Allmänna säkerhetsfrågor för AEM Forms i JEE{#general-security-considerations-for-aem-forms-on-jee}

Den här artikeln innehåller introduktionsinformation som hjälper dig att förbereda dig för att förbättra din AEM Forms-miljö. Det innehåller information om krav på AEM Forms på JEE, operativsystem, programserver och databassäkerhet. Granska informationen innan du fortsätter att låsa miljön.

## Leverantörsspecifik säkerhetsinformation {#vendor-specific-security-information}

Det här avsnittet innehåller säkerhetsrelaterad information om operativsystem, programservrar och databaser som ingår i din AEM Forms i JEE-lösning.

Använd länkarna i det här avsnittet för att hitta leverantörsspecifik säkerhetsinformation för operativsystemet, databasen och programservern.

### Säkerhetsinformation för operativsystem {#operating-system-security-information}

När du skyddar ditt operativsystem bör du noga överväga att implementera de åtgärder som beskrivs av din leverantör av operativsystem, bland annat följande:

* Definiera och styra användare, roller och behörigheter
* Övervakningsloggar och verifieringskedja
* Tar bort onödiga tjänster och program
* Säkerhetskopiera filer

Säkerhetsinformation om operativsystem som stöds av AEM Forms på JEE finns i resurserna i tabellen:

<table>
 <thead>
  <tr>
   <th><p>Operativsystem</p> </th>
   <th><p>Säkerhetsresurs</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>IBM® AIX® 7.2</p> </td>
   <td><p><a href="https://www.ibm.com/support/knowledgecenter/ssw_aix_72/com.ibm.aix.security/security-kickoff.htm" target="_blank">IBM® AIX® - säkerhetsfördelar</a></p> </td>
  </tr>
  <tr>
   <td><p>Microsoft® Windows Server® 2016 </p> </td>
   <td><p><a href="https://cloudblogs.microsoft.com/windowsserver/2017/08/22/now-available-windows-server-2016-security-guide/">Säkerhetshandbok för Windows Server 2016</a></p> </td>
  </tr>
  <tr>
   <td><p>Red Hat® Linux® AP eller ES</p> </td>
   <td><p><a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/pdf/security_guide/Red_Hat_Enterprise_Linux-7-Security_Guide-en-US.pdf" target="_blank">Red Hat® Enterprise Linux® Security Guide</a></p> </td>
  </tr>
  <tr>
   <td><p>Sun Solaris™ 11</p> </td>
   <td><p><a href="https://docs.oracle.com/cd/E53394_01/html/E54807/index.html" target="_blank">Riktlinjer för säkerhet och skärpning</a></p> </td>
  </tr>
  <tr>
   <td>Oracle Linux® 7 Update 3</td>
   <td><a href="https://docs.oracle.com/en/operating-systems/oracle-linux/7/security/" target="_blank">Säkerhetshandbok för version 7</a><br /> </td>
  </tr>
  <tr>
   <td>CentOS 7<sup> </sup></td>
   <td><a href="https://wiki.centos.org/HowTos/OS_Protection" target="_blank">Skyddsdokumentation</a></td>
  </tr>
 </tbody>
</table>

### Säkerhetsinformation för programserver {#application-server-security-information}

När du skyddar programservern bör du noga överväga att implementera de åtgärder som beskrivs av serverleverantören, bland annat följande:

* Använda ett användarnamn som inte är uppenbart för administratören
* Onödiga tjänster inaktiveras
* Skydda konsolhanteraren
* Aktivera säkra cookies
* Stänger portar som inte behövs
* Begränsa klienter efter IP-adresser eller domäner
* Använda Java™ Security Manager för att programmässigt begränsa behörigheter

Säkerhetsinformation om programservrar som stöds av AEM Forms på JEE finns i resurserna i den här tabellen.

<table>
 <thead>
  <tr>
   <th><p>Programserver</p> </th>
   <th><p>Säkerhetsresurs</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>Oracle WebLogic®</p> </td>
   <td><p>Sök efter Understanding WebLogic Security på <a href="https://docs.oracle.com/">https://docs.oracle.com/</a>.</p> </td>
  </tr>
  <tr>
   <td><p>IBM® WebSphere®</p> </td>
   <td><p><a href="https://www.ibm.com/developerworks/websphere/zones/was/security/" target="_blank">Skydda applikationer och deras miljö</a></p> </td>
  </tr>
  <tr>
   <td><p>Red Hat® JBoss®</p> </td>
   <td><p><a href="https://docs.jboss.org/author/display/AS7/Security%20subsystem%20configuration.html">Konfiguration av säkerhetsundersystem</a></p> </td>
  </tr>
 </tbody>
</table>

### Databassäkerhetsinformation {#database-security-information}

När du skyddar din databas bör du överväga att implementera de åtgärder som beskrivs av din databasleverantör, bland annat följande:

* Begränsa åtgärder med åtkomstkontrollistor
* Använda portar som inte är standard
* Dölja databasen bakom en brandvägg
* Kryptera känsliga data innan du skriver dem till databasen (se databastillverkarens dokumentation)

Säkerhetsinformation om databaser som stöds av AEM Forms på JEE finns i resurserna i den här tabellen.

<table>
 <thead>
  <tr>
   <th><p>Databas</p> </th>
   <th><p>Säkerhetsresurs</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>IBM® DB2® 11.1</p> </td>
   <td><p><a href="https://www-01.ibm.com/software/data/db2/library/">DB2® produktfamiljsbibliotek</a></p> </td>
  </tr>
  <tr>
   <td><p>Microsoft® SQL Server 2016</p> </td>
   <td>Sök på webben efter"SQL Server 2016: Security"</td>
  </tr>
  <tr>
   <td><p>MySQL 5</p> </td>
   <td><p><a href="https://dev.mysql.com/doc/refman/5.0/en/security.html">Allmänna säkerhetsproblem i MySQL 5.0</a></p> <p><a href="https://dev.mysql.com/doc/refman/5.1/en/security.html">Allmänna säkerhetsproblem i MySQL 5.1</a></p> </td>
  </tr>
  <tr>
   <td><p>Oracle® 12c</p> </td>
   <td><p>Se kapitlet om säkerhet i <a href="https://docs.oracle.com/database/121/TDPSG/GUID-6E2F4E53-5D87-4FCD-9C9C-6792217D7014.htm#TDPSG94426" target="_blank">Oracle 12g-dokumentationen</a></p> </td>
  </tr>
 </tbody>
</table>

I den här tabellen beskrivs de standardportar som krävs för att vara öppna under konfigurationsprocessen för AEM Forms on JEE. Om du ansluter via https kan du justera portinformationen och IP-adresserna i enlighet med detta. Mer information om hur du konfigurerar portar finns i dokumentet *Installera och distribuera AEM Forms på JEE* för programservern.

<table>
 <thead>
  <tr>
   <th><p>Produkt eller tjänst</p> </th>
   <th><p>Portnummer</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>JBoss®</p> </td>
   <td><p>8080</p> </td>
  </tr>
  <tr>
   <td><p>WebLogic</p> </td>
   <td><p>7001</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>Hanterad WebLogic-server</p> </td>
   <td><p>Ange av administratör under konfiguration</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>WebSphere®</p> </td>
   <td><p>9060, om Global Security är aktiverat är SSL-portvärdet 9043 som standard.</p> <p>9080</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>BAM-server</p> </td>
   <td><p>7001</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>SOAP</p> </td>
   <td><p>8880</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>MySQL</p> </td>
   <td><p>3306</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>Oracle</p> </td>
   <td><p>1521</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>DB2®</p> </td>
   <td><p>50000</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>SQL Server</p> </td>
   <td><p>1433</p> </td>
  </tr>
  <tr>
   <td>&gt;<p>LDAP</p> </td>
   <td><p>Porten som LDAP-servern körs på. Standardporten är vanligtvis 389. Om du väljer SSL-alternativet är standardporten normalt 636. Kontrollera med LDAP-administratören vilken port som ska anges.</p> </td>
  </tr>
 </tbody>
</table>

### Konfigurera JBoss® så att en HTTP-port som inte är standard används {#configuring-jboss-to-use-a-non-default-http-port}

JBoss® Application Server använder 8080 som standard-HTTP-port. JBoss® har även förkonfigurerade portarna 8180, 8280 och 8380, som kommenteras ut i filen jboss-service.xml. Om du har ett program på datorn som redan använder den här porten ändrar du den port som AEM Forms på JEE använder genom att följa de här stegen:

1. Öppna följande fil för redigering:

   Single-Server-installation: [JBoss® root]/standalone/configuration/standalone.xml

   Klusterinstallationer: [JBoss® root]/domain/configuration/domain.xml

1. Ändra värdet för attributet **port** i taggen **&lt;socket-binding>** till ett anpassat portnummer. I följande exempel används port 8090:

   &lt;socket-binding name=&quot;http&quot; port=&quot;8090&quot;/>

1. Spara och stäng filen.
1. Starta om JBoss®-programservern.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

## AEM Forms om JEE - säkerhetsfrågor {#aem-forms-on-jee-security-considerations}

I det här avsnittet beskrivs några av AEM Forms om JEE-specifika säkerhetsfrågor som du bör känna till.

### E-postautentiseringsuppgifterna är inte krypterade i databasen {#email-credentials-not-encrypted-in-database}

De e-postautentiseringsuppgifter som lagras av program krypteras inte innan de lagras i AEM Forms i JEE-databasen. När du konfigurerar en tjänstslutpunkt att använda e-post krypteras inte lösenordsinformation som används som en del av den slutpunktskonfigurationen när den lagras i databasen.

### Känsligt innehåll för Rights Management i databasen {#sensitive-content-for-rights-management-in-the-database}

AEM Forms on JEE använder AEM Forms on JEE-databasen för att lagra känslig dokumentnyckelinformation och annat kryptografiskt material som används för policydokument. Skydda känslig information genom att skydda databasen mot intrång.

### Lösenord i klartextformulär {#password-in-clear-text-format-in-adobe-ds-xml}

Programservern som används för att köra AEM Forms på JEE kräver en egen konfiguration för åtkomst till databasen via en datakälla som är konfigurerad på programservern. Se till att programservern inte visar databaslösenordet i klartext i sin datakällkonfigurationsfil.

Filen lc_[database].xml får inte innehålla lösenord i klartextformat. Kontakta programserverleverantören om hur du krypterar dessa lösenord för programservern.

>[!NOTE]
>
>Installationsprogrammet för AEM Forms på JEE JBoss® med nyckelord krypterar databaslösenordet.

IBM® WebSphere® Application Server och Oracle WebLogic Server kan kryptera lösenord för datakällor som standard. Du bör dock kontrollera med programserverns dokumentation att det händer.

### Skydda den privata nyckeln som lagras i Trust Store {#protecting-the-private-key-stored-in-trust-store}

De privata nycklarna eller autentiseringsuppgifterna som importeras i Trust Store lagras i AEM Forms i JEE-databasen. Vidta lämpliga försiktighetsåtgärder för att skydda databasen och begränsa åtkomsten till endast de administratörer som har utsetts.
