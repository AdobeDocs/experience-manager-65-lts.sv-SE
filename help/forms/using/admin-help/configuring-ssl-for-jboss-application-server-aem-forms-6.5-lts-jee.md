---
title: Konfigurera SSL för JBoss Application Server (AEM 6.5 LTS JEE)
description: Lär dig hur du konfigurerar SSL för JBoss Application Server.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_ssl
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Document Security
role: User, Developer
exl-id: 8a2fbfdc-2b6a-4c9d-beab-1908a9261003
source-git-commit: e48d0604cc8fd1cf745aafa9c70aab17944f4882
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Konfigurera SSL för JBoss Application Server (AEM 6.5 LTS JEE)

## Ökning

Om du vill konfigurera SSL på JBoss Application Server för Adobe Experience Manager (AEM) 6.5 LTS som körs på Java EE måste du aktivera säker HTTPS-kommunikation. Om SSL aktiveras krypteras data som utbyts mellan klienter och servern, vilket gör det till ett viktigt säkerhetskrav för all AEM Forms-distribution i produktionen.

Processen består av två huvudsteg:

- **Hämtar en SSL-autentiseringsuppgift** - antingen genom att generera ett självsignerat certifikat eller begära ett från en certifikatutfärdare (CA).
- **Aktivera SSL på JBoss** - med Elytron-undersystemet via JBoss CLI-kommandon.

I den här guiden används följande platshållarvärden:

- `[appserver root]` - arbetskatalogen för JBoss-programservern som kör AEM Forms.
- `[type]` - ett mappnamn som varierar beroende på vilken typ av installation som har utförts.
- `[JAVA_HOME]` - den katalog där JDK är installerat.

## Del 1: Skapa en SSL-autentiseringsuppgift (självsignerad)

Om du inte har något certifikat från en certifikatutfärdare kan du generera en självsignerad SSL-autentiseringsuppgift med Java-verktyget `keytool`. Detta är lämpligt för utvecklings- och testmiljöer.

### Steg 1 - Generera nyckelbehållaren

Navigera till `[JAVA_HOME]/bin` i en kommandotolk och kör följande kommando och ersätt värdena med dem som passar din miljö. Värdnamnet måste vara det fullständiga domännamnet (FQDN) för programservern:

```bash
keytool -genkey -dname "CN=Host Name, OU=Group Name, O=Company Name, L=City Name, S=State, C=Country Code" \
  -alias "AEMForms Cert" -keyalg RSA \
  -keypass key_password -keystore keystorename.keystore
```

Ange `keystore_password` när du uppmanas till detta. Observera att lösenordet för nyckelbehållaren och nyckeln måste vara identiska.

### Steg 2 - Kopiera nyckelbehållaren till konfigurationskatalogen

Kopiera den genererade nyckelbehållaren till rätt konfigurationsmapp för installationstypen:

```bash
# Windows Single Server
copy keystorename.keystore [appserver root]\standalone\configuration

# Windows Server Cluster
copy keystorename.keystore [appserver root]\domain\configuration

# Linux Single Server
cp keystorename.keystore [appserver root]/standalone/configuration

# Linux Server Cluster
cp keystorename.keystore [appserver root]/domain/configuration
```

### Steg 3 - Exportera certifikatfilen

Exportera certifikatet från nyckelbehållaren med något av följande kommandon:

```bash
# Single Server
keytool -export -alias "AEMForms Cert" -file AEMForms_cert.cer \
  -keystore [appserver root]/standalone/configuration/keystorename.keystore

# Server Cluster
keytool -export -alias "AEMForms Cert" -file AEMForms_cert.cer \
  -keystore [appserver root]/domain/configuration/keystorename.keystore
```

Ange `keystore_password` när du uppmanas till det.

### Steg 4 - Kopiera och verifiera certifikatet

Kopiera `AEMForms_cert.cer` till konfigurationskatalogen och verifiera sedan dess innehåll:

```bash
# Copy (Linux Single Server example)
cp AEMForms_cert.cer [appserver root]/standalone/configuration

# Verify certificate contents (Single Server)
keytool -printcert -v -file [appserver root]/standalone/configuration/AEMForms_cert.cer

# Verify certificate contents (Server Cluster)
keytool -printcert -v -file [appserver root]/domain/configuration/AEMForms_cert.cer
```

### Steg 5 - Importera certifikatet till Java Truststore

Kontrollera att filen `cacerts` är skrivbar innan du importerar den:

```bash
# Windows: Right-click cacerts → Properties → deselect Read-only

# Linux:
chmod 777 [JAVA_HOME]/jre/lib/security/cacerts
```

Importera sedan certifikatet:

```bash
keytool -import -alias "AEMForms Cert" -file AEMForms_cert.cer \
  -keystore [JAVA_HOME]/jre/lib/security/cacerts
```

När du uppmanas att ange lösenordet skriver du `changeit` (standardlösenordet för Java-förtroendearkivet - kontakta administratören om detta har ändrats). När du tillfrågas **Lita på det här certifikatet? [no]**, typ `yes`. Ett bekräftelsemeddelande ska visas: *Certifikatet har lagts till i nyckelbehållaren.*

>[!NOTE]
>
> Om du ansluter till AEM Forms via SSL från Workbench måste du även installera certifikatet på Workbench-datorn.

## Del 2: Aktivera SSL på JBoss med hjälp av Elytron-delsystemet

Med SSL-autentiseringsuppgifterna på plats kan du nu aktivera HTTPS på JBoss med hjälp av Elytron-säkerhetsundersystemet via JBoss CLI. Kontrollera att nyckelfilen finns i rätt konfigurationskatalog innan du fortsätter.

>[!NOTE]
>
> I Windows måste varje CLI-kommando anges som en enda rad utan radbrytningar. Ersätt `keystorename.keystore` med ditt faktiska filnamn och `changeit` med ditt nyckelbehållare/nyckellösenord.

### Steg 6a - Skapa ett nyckelarkiv för Elytron

```bash
/subsystem=elytron/key-store=aemKeyStore:add(
  path="keystorename.keystore",
  relative-to=jboss.server.config.dir,
  type="JKS",
  credential-reference={clear-text="changeit"})
```

Ersätt `JKS` med `PKCS12` om nyckelbehållaren använder det formatet.

### Steg 6b - Skapa nyckelhanteraren Elytron

```bash
/subsystem=elytron/key-manager=aemKeyManager:add(
  key-store=aemKeyStore,
  credential-reference={clear-text="changeit"})
```

Om nyckelbehållaren innehåller flera certifikatposter anger du alias:

```bash
/subsystem=elytron/key-manager=aemKeyManager:add(
  key-store=aemKeyStore,
  alias="AEMForms Cert",
  credential-reference={clear-text="changeit"})
```

### Steg 6c - Uppdatera serverns SSL-kontext

```bash
/subsystem=elytron/server-ssl-context=applicationSSC:write-attribute(
  name=key-manager,
  value=aemKeyManager)
```

### Steg 6d - Konfigurera underliggande HTTPS-avlyssnare

Anslut SSL-kontexten till Undertw (JBoss-webbservern) för att aktivera HTTPS-avlyssnaren:

```bash
/subsystem=undertow/server=default-server/https-listener=https:add(
  socket-binding=https,
  ssl-context=applicationSSC)
```

## Del 3: Starta om programservern

När du har slutfört Elytron-konfigurationen startar du om JBoss för att tillämpa ändringarna.

### Nyckelfärdiga installationer (Windows Services)

- Öppna **Kontrollpanelen > Administrationsverktyg > Tjänster**.
- Välj **JBoss för Adobe Experience Manager-formulär**.
- Välj **Åtgärd > Stoppa** och vänta tills tjänsten stoppas.
- Välj **Åtgärd > Start**.

### Förkonfigurerade eller manuella JBoss-installationer

Navigera från en kommandotolk till `[appserver root]/bin`:

```bash
# Stop the server
Windows: shutdown.bat -S
Linux:   ./shutdown.sh -S

# Wait until JBoss fully shuts down, then start the server
Windows: run.bat -c <profile>
Linux:   ./run.sh -c <profile>
```

## Del 4: Begär en inloggningsinformation från en certifikatutfärdare

För produktionsmiljöer bör du använda ett certifikat som har utfärdats av en betrodd certifikatutfärdare (CA). Processen innebär att skapa ett nyckelpar, skapa en CSR (Certificate Signing Request) och sedan importera det CA-signerade certifikatet.

### Skapa nyckelbehållaren och skapa en CSR

Navigera till `[JAVA_HOME]/bin` och generera nyckelbehållaren:

```bash
keytool -genkey -dname "CN=Host Name, OU=Group Name, O=Company Name, L=City Name, S=State, C=Country Code" \
  -alias "AEMForms Cert" -keyalg RSA \
  -keypass key_password -keystore keystorename.keystore
```

Generera sedan CSR för att skicka till din certifikatutfärdare:

```bash
keytool -certreq -alias "AEMForms Cert" \
  -keystore keystorename.keystore \
  -file AEMFormscertRequest.csr
```

Skicka filen `.csr` till din certifikatutfärdare. När du har fått tillbaka det signerade certifikatet följer du stegen nedan.

### Importera det CA-signerade certifikatet

Importera certifikatutfärdarens rotcertifikat först:

```bash
keytool -import -trustcacerts -file rootcert.pem \
  -keystore keystorename.keystore -alias root
```

Om rotcertifikatet inte redan är betrott av webbläsaren importerar du det även där. Importera sedan det CA-signerade certifikatet:

```bash
keytool -import -trustcacerts -file CACertificateName.crt \
  -keystore keystorename.keystore
```

>[!NOTE]
>
> Det importerade CA-signerade certifikatet ersätter alla befintliga självsignerade offentliga certifikat om det finns något i nyckelbehållaren.

När certifikatutfärdarcertifikatet har importerats fortsätter du med **steg 6a-6d** i del 2 (Elytron-konfiguration), startar om servern (del 3) och verifierar SSL-åtkomst.

## Verifiera SSL-åtkomst

När du har startat om servern kontrollerar du att SSL fungerar som det ska genom att gå till AEM Forms administrationskonsol via HTTPS. Standardporten för SSL för JBoss är `8443`:

```
https://[host name]:8443/adminui
```

Om administrationskonsolen läses in över HTTPS har SSL konfigurerats korrekt. Använd port `8443` för alla efterföljande SSL-anslutningar till AEM Forms.
