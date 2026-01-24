---
title: Konfigurera LDAP med AEM 6
description: Lär dig hur du använder och konfigurerar LDAP-tjänster med AEM.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Security
role: Admin
exl-id: d19750c4-9477-4bcb-b225-5f089b43194d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# Konfigurera LDAP med AEM 6 {#configuring-ldap-with-aem}

LDAP (protokollet **L** ightweight **D** Directory **A** Access **P**) används för åtkomst till centraliserade katalogtjänster. Det minskar den arbetsinsats som krävs för att hantera användarkonton eftersom de kan nås av flera program. En sådan LDAP-server är Active Directory. LDAP används ofta för att uppnå enkel inloggning, vilket gör att en användare kan få åtkomst till flera program efter inloggning en gång.

Användarkonton kan synkroniseras mellan LDAP-servern och databasen med LDAP-kontoinformation som sparas i databasen. Den här funktionen gör att konton kan tilldelas till databasgrupper för att tilldela de behörigheter och behörigheter som krävs.

Databasen använder LDAP-autentisering för att autentisera sådana användare, där inloggningsuppgifterna skickas till LDAP-servern för validering, vilket krävs innan åtkomst till databasen tillåts. För att förbättra prestandan kan validerade inloggningsuppgifter cachas av databasen med en tidsgräns för förfallodatum så att omvalidering sker efter en lämplig period.

När ett konto tas bort från LDAP-servern beviljas inte längre någon validering och åtkomst till databasen nekas. Information om LDAP-konton som sparas i databasen kan också rensas.

Användningen av sådana konton är transparent för användarna. Det innebär att de inte ser någon skillnad mellan användar- och gruppkonton som skapats från LDAP och konton som skapats enbart i databasen.

I AEM 6 har LDAP-stödet en ny implementering som kräver en annan typ av konfiguration än i tidigare versioner.

Alla LDAP-konfigurationer är nu tillgängliga som OSGi-konfigurationer. De kan konfigureras via webbhanteringskonsolen på:
`https://serveraddress:4502/system/console/configMgr`

Om du vill att LDAP ska fungera med AEM måste du skapa tre OSGi-konfigurationer:

1. En LDAP-identitetsleverantör (IDP).
1. En synkroniseringshanterare.
1. En extern inloggningsmodul.

>[!NOTE]
>
>Titta på [Oak modul för extern inloggning - autentiserar med LDAP och bortom](https://experienceleague.adobe.com/docs/experience-manager-gems-events/gems/gems2015/aem-oak-external-login-module-authenticating-with-ldap-and-beyond.html?lang=sv-SE) för att kunna identifiera externa inloggningsmoduler.
>
>Ett exempel på hur du konfigurerar Experience Manager med Apache DS finns i [Konfigurera Adobe Experience Manager 6.5 så att den använder Apache Directory Service.](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/configuring-adobe-experience-manager-6-to-use-apache-directory/m-p/183805?profile.language=sv)

## Konfigurera LDAP-identitetsleverantören {#configuring-the-ldap-identity-provider}

LDAP-identitetsprovidern används för att definiera hur användare hämtas från LDAP-servern.

Den finns i hanteringskonsolen under **Apache Jackrabbit Oak LDAP Identity Provider** -namnet.

Följande konfigurationsalternativ är tillgängliga för LDAP-identitetsleverantören:

<table>
 <tbody>
  <tr>
   <td><strong>LDAP-providernamn</strong></td>
   <td>Namnet på den här LDAP-providerkonfigurationen.</td>
  </tr>
  <tr>
   <td><strong>LDAP-servervärdnamn</strong><br /> </td>
   <td>Värdnamn för LDAP-servern</td>
  </tr>
  <tr>
   <td><strong>LDAP-serverport</strong></td>
   <td>Port för LDAP-servern</td>
  </tr>
  <tr>
   <td><strong>Använd SSL</strong></td>
   <td>Anger om en SSL-anslutning (LDAP) ska användas.</td>
  </tr>
  <tr>
   <td><strong>Använd TLS</strong></td>
   <td>Anger om TLS ska startas på anslutningar.</td>
  </tr>
  <tr>
   <td><strong>Inaktivera certifikatkontroll</strong></td>
   <td>Anger om servercertifikatvalidering ska inaktiveras.</td>
  </tr>
  <tr>
   <td><strong>Bind DN</strong></td>
   <td>Unikt namn för användaren för autentisering. Om fältet lämnas tomt utförs en anonym bindning.</td>
  </tr>
  <tr>
   <td><strong>Bind lösenord</strong></td>
   <td>Användarens lösenord för autentisering</td>
  </tr>
  <tr>
   <td><strong>Timeout för sökning</strong></td>
   <td>Tid tills sökningen tar slut</td>
  </tr>
  <tr>
   <td><strong>Admin pool max aktiv</strong></td>
   <td>Maximal aktiv storlek för administratörsanslutningspoolen.</td>
  </tr>
  <tr>
   <td><strong>Användarpoolen är max aktiv</strong></td>
   <td>Den maximala aktiva storleken för användaranslutningspoolen.</td>
  </tr>
  <tr>
   <td><strong>Användar-bas-DN</strong></td>
   <td>DN för användarsökningar</td>
  </tr>
  <tr>
   <td><strong>Användarobjektklasser</strong></td>
   <td>Listan med objektklasser som en användarpost måste innehålla.</td>
  </tr>
  <tr>
   <td><strong>Användar-id-attribut</strong></td>
   <td>Namnet på det attribut som innehåller användar-ID:t.</td>
  </tr>
  <tr>
   <td><strong>Extra filter för användare</strong></td>
   <td>Extra LDAP-filter som används vid sökning efter användare. Det sista filtret formateras som: (&amp;(&lt;idAttr&gt;=&lt;userId&gt;)(objectclass=&lt;objectClass&gt;)&lt;extraFilter&gt;) (user.extraFilter)</td>
  </tr>
  <tr>
   <td><strong>Användarens DN-sökvägar</strong></td>
   <td>Anger om det unika namnet ska användas för att beräkna en del av den mellanliggande sökvägen.</td>
  </tr>
  <tr>
   <td><strong>Gruppbasens unika namn</strong></td>
   <td>Basens unika namn för gruppsökningar.</td>
  </tr>
  <tr>
   <td><strong>Gruppobjektklasser</strong></td>
   <td>Listan med objektklasser som en grupppost måste innehålla.</td>
  </tr>
  <tr>
   <td><strong>Gruppnamnsattribut</strong></td>
   <td>Namnet på det attribut som innehåller gruppnamnet.</td>
  </tr>
  <tr>
   <td><strong>Gruppera extra filter</strong></td>
   <td>Extra LDAP-filter som används vid sökning efter grupper. Det sista filtret är formaterat enligt följande: (&amp;(&lt;nameAttr&gt;=&lt;groupName&gt;)(objectclass=&lt;objectClass&gt;)&lt;extraFilter&gt;)</td>
  </tr>
  <tr>
   <td><strong>Gruppera DN-sökvägar</strong></td>
   <td>Anger om det unika namnet ska användas för att beräkna en del av den mellanliggande sökvägen.</td>
  </tr>
  <tr>
   <td><strong>Gruppmedlemsattribut</strong></td>
   <td>Gruppattribut som innehåller en eller flera gruppmedlemmar.</td>
  </tr>
 </tbody>
</table>

## Konfigurera synkroniseringshanteraren {#configuring-the-synchronization-handler}

Synkroniseringshanteraren definierar hur identitetsleverantörens användare och grupper synkroniseras med databasen.

Den finns under **Apache Jackrabbit Oak-standardhanterare för synkronisering** i hanteringskonsolen.

Följande konfigurationsalternativ är tillgängliga för Synkroniseringshanteraren:

<table>
 <tbody>
  <tr>
   <td><strong>Namn på synkroniseringshanterare</strong></td>
   <td>Namn på synkroniseringskonfigurationen.</td>
  </tr>
  <tr>
   <td><strong>Förfallotid för användare</strong></td>
   <td>Varaktighet tills en synkroniserad användare går ut.</td>
  </tr>
  <tr>
   <td><strong>Automedlemskap för användare</strong></td>
   <td>Lista över grupper som en synkroniserad användare automatiskt läggs till i.</td>
  </tr>
  <tr>
   <td><strong>Mappning av användaregenskaper</strong></td>
   <td>Definition av listmappning av lokala egenskaper från externa.</td>
  </tr>
  <tr>
   <td><strong>Användarsökvägsprefix</strong></td>
   <td>Det sökvägsprefix som används när användare skapas.</td>
  </tr>
  <tr>
   <td><strong>Förfallotid för användarmedlemskap</strong></td>
   <td>Tid efter vilket medlemskapet upphör.<br /> </td>
  </tr>
  <tr>
   <td><strong>Inkapslingsdjup för användarmedlemskap</strong></td>
   <td>Returnerar det maximala djupet för gruppkapsling när medlemsrelationer synkroniseras. Värdet 0 inaktiverar sökning efter gruppmedlemskap. Värdet 1 lägger bara till en användares direktgrupper. Det här värdet har ingen effekt vid synkronisering av enskilda grupper endast när en användares medlemskapshistorik synkroniseras.</td>
  </tr>
  <tr>
   <td><strong>Förfallotid för grupp</strong></td>
   <td>Varaktighet tills en synkroniserad grupp förfaller.</td>
  </tr>
  <tr>
   <td><strong>Automatiskt gruppmedlemskap</strong></td>
   <td>Lista över grupper som en synkroniserad grupp automatiskt läggs till i.</td>
  </tr>
  <tr>
   <td><strong>Mappning av gruppegenskaper</strong></td>
   <td>Definition av listmappning av lokala egenskaper från externa.</td>
  </tr>
  <tr>
   <td><strong>Prefix för gruppsökväg</strong></td>
   <td>Det sökvägsprefix som används när grupper skapas.</td>
  </tr>
 </tbody>
</table>

## Den externa inloggningsmodulen {#the-external-login-module}

Den externa inloggningsmodulen finns under **Apache Jackrabbit Oak External Login Module** under hanteringskonsolen.

>[!NOTE]
>
>Apache Jackrabbit Oak External Login Module implementerar Java™ Authentication and Authorization Server (JAAS)-specifikationerna. Mer information finns i [Oracle Java™ Security Reference Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jaas/JAASRefGuide.html).

Dess uppdrag är att definiera vilken identitetsleverantör och synkroniseringshanterare som ska användas, vilket effektivt binder de två modulerna.

Följande konfigurationsalternativ är tillgängliga:

| **JAAS-rankning** | Ange rankningen (d.v.s. sorteringsordningen) för den här inloggningsmodulposten. Posterna sorteras i fallande ordning (dvs. högre rangordnade konfigurationer kommer först). |
|---|---|
| **JAAS-kontrollflagga** | Egenskap som anger om en LoginModule är OBLIGATORISK, REQUISITE, TILLRÄCKLIG eller VALFRI. Mer information om innebörden av dessa flaggor finns i dokumentationen om JAAS-konfigurationen. |
| **JAAS-sfär** | Sfärnamnet (eller programnamnet) som LoginModule är registrerad mot. Om inget sfärnamn anges registreras LoginModule med en standardsfärk som konfigurerats i Felix JAAS-konfigurationen. |
| **Identitetsleverantörens namn** | Identitetsleverantörens namn. |
| **Namn på synkroniseringshanterare** | Synkroniseringshanterarens namn. |

>[!NOTE]
>
>Om du planerar att ha fler än en LDAP-konfiguration med din AEM-instans måste separata identitetsleverantörer och synkroniseringshanterare skapas för varje konfiguration.

## Konfigurera LDAP över SSL {#configure-ldap-over-ssl}

AEM 6 kan konfigureras för att autentisera med LDAP via SSL genom att följa nedanstående procedur:

1. Markera kryssrutorna **Använd SSL** eller **Använd TLS** när du konfigurerar [LDAP-identitetsprovidern](#configuring-the-ldap-identity-provider).
1. Konfigurera Synkroniseringshanteraren och modulen för extern inloggning enligt inställningarna.
1. Installera SSL-certifikaten i din virtuella Java™-dator om det behövs. Installationen kan göras med hjälp av nyckelverktyget:

   `keytool -import -alias localCA -file <certificate location> -keystore <keystore location>`

1. Testa anslutningen till LDAP-servern.

### Skapa SSL-certifikat {#creating-ssl-certificates}

Självsignerade certifikat kan användas när AEM konfigureras för autentisering med LDAP via SSL. Nedan visas ett exempel på ett arbetssätt för att generera certifikat som ska användas med AEM.

1. Kontrollera att du har ett SSL-bibliotek installerat och att det fungerar. I den här proceduren används OpenSSL som exempel.

1. Skapa en anpassad OpenSSL-konfigurationsfil (cnf). Du kan göra den här konfigurationen genom att kopiera standardkonfigurationsfilen **openssl.cnf &#x200B;** och anpassa den. På UNIX®-system är den på `/usr/lib/ssl/openssl.cnf`

1. Fortsätt till att skapa certifikatutfärdarens rotnyckel genom att köra följande kommando i en terminal:

   ```
   openssl genpkey -algorithm [public key algorithm] -out certificatefile.key -pkeyopt [public key algorithm option]
   ```

1. Skapa sedan ett självsignerat certifikat:

   `openssl req -new -x509 -days [number of days for certification] -key certificatefile.key -out root-ca.crt -config CA/openssl.cnf`

1. Kontrollera att allt är i rätt ordning genom att kontrollera det nya certifikatet:

   `openssl x509 -noout -text -in root-ca.crt`

1. Kontrollera att alla mappar som anges i certifikatkonfigurationsfilen (.cnf) finns. Om inte, skapar du dem.
1. Skapa ett slumpmässigt startvärde genom att köra:

   `openssl rand -out private/.rand 8192`

1. Flytta de skapade .pem-filerna till de platser som är konfigurerade i .cnf-filen.

1. Lägg slutligen till certifikatet i Java™-nyckelbehållaren.

## Aktivera felsökningsloggning {#enabling-debug-logging}

Felsökningsloggning kan aktiveras för både LDAP-identitetsprovidern och den externa inloggningsmodulen för att felsöka anslutningsproblem.

Om du vill aktivera felsökningsloggning måste du göra följande:

1. Gå till webbhanteringskonsolen.
1. Sök efter&quot;Konfiguration av loggningslogg för Apache Sling&quot; och skapa två loggare med följande alternativ:

* Loggnivå: Felsökning
* Loggfil logs/ldap.log
* Meddelandemönster: &lbrace;0,date,`dd.MM.yyyy` `HH:mm:ss.SSS` &ast;{4}&ast; {2} {3} {5}
* Logger: org.apache.jackrabbit.oak.security.authentication.ldap

* Loggnivå: Felsökning
* Loggfil: logs/external.log
* Meddelandemönster: &lbrace;0,date,`dd.MM.yyyy` `HH:mm:ss.SSS` &ast;{4}&ast; {2} {3} {5}
* Logger: org.apache.jackrabbit.oak.spi.security.authentication.external

## Ett ord om gruppanknytning {#a-word-on-group-affiliation}

Användare som synkroniseras via LDAP kan ingå i olika grupper i AEM. Dessa grupper kan vara externa LDAP-grupper som läggs till i AEM som en del av synkroniseringsprocessen. De kan dock också vara grupper som läggs till separat och inte ingår i det ursprungliga LDAP-koncerntillhörighetsschemat.

Vanligtvis läggs dessa grupper till av en lokal AEM-administratör eller av någon annan identitetsleverantör.

Om en användare tas bort från en grupp på LDAP-servern återspeglas ändringen på AEM-sidan vid synkroniseringen. Alla andra grupptillhörigheter för användaren som inte lades till av LDAP finns dock kvar.

AEM identifierar och hanterar rensning av användare från externa grupper med egenskapen `rep:externalId`. Den här egenskapen läggs till automatiskt för alla användare eller grupper som synkroniseras med Synchronization Handler och den innehåller information om den ursprungliga identitetsleverantören.

Se Apache Oak-dokumentation om [användar- och gruppsynkronisering](https://jackrabbit.apache.org/oak/docs/security/authentication/usersync.html).

## Kända fel {#known-issues}

Om du tänker använda LDAP över SSL måste du kontrollera att de certifikat du använder skapas utan alternativet Netscape-kommentar. Om det här alternativet är aktiverat misslyckas autentiseringen med ett SSL-handskakningsfel.
