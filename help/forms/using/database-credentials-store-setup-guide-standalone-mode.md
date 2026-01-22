---
title: Installationsguide för databasarkiv (fristående läge)
description: Hitta databasens inställningar för arkiv med autentiseringsuppgifter för AEM Forms JEE på JBoss/Red Hat EAP i fristående läge.
solution: Experience Manager
feature: Deploying
role: User,Admin,Developer
hide: true
index: false
hidefromtoc: true
source-git-commit: 5d020671efaa4527a5f6dbb4b779c7a3351888a4
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---


# Installationsguide för databasarkiv (fristående läge)

## Ökning

Den här guiden täcker **databasens konfiguration för autentiseringsuppgifter** för AEM Forms JEE på JBoss/Red Hat EAP i **fristående läge**. Detta krävs vid manuell installation.

**Den här guiden täcker:**

- Skapar databasens autentiseringsuppgiftslager (`cred-store.p12`)
- Lägga till lösenordsalias för databaser på ett säkert sätt
- Konfigurera JBoss att använda arkivet för autentiseringsuppgifter

**ALLVARLIGT:** Dessa skript fungerar i **ENDAST OFFLINE-LÄGE**. JBoss måste stoppas innan dessa skript körs. Skripten använder läget `embed-server`, vilket kräver att servern stoppas.

**Obs!** Detta är **inte** en fullständig fristående installationsguide. Det här dokumentet förutsätter:

- JBoss är redan installerat
- Fristående konfigurationsfiler (`lc_oracle.xml`, `lc_mysql.xml` eller `lc_mssql.xml`) har redan konfigurerats
- Databasen har konfigurerats och är tillgänglig

Om du behöver fullständiga anvisningar för fristående installation, se huvudinstallationsguiden.

## Förutsättningar

Innan du kör dessa skript bör du kontrollera följande:

1. **JBoss MÅSTE stoppas**
   - Dessa skript fungerar i **ENDAST OFFLINE-LÄGET**
   - Skripten använder `embed-server`, vilket kräver att servern stoppas
   - Om JBoss körs misslyckas skripten
   - Kontrollera om JBoss körs:
      - Windows: Kontrollera aktivitetshanteraren för processen `java.exe`
      - Linux: `ps aux | grep jboss` eller `ps aux | grep java`
   - Stoppa JBoss om du kör:
      - Tryck på `Ctrl+C` i terminalen där JBoss körs
      - Eller avsluta processen manuellt

2. **Databaslösenordet är klart**

3. **Du har bestämt dig för ett säkert lösenord för arkivet med autentiseringsuppgifter**

4. **Du vet vilken databaskonfigurationsfil du använder:**
   - `lc_oracle.xml` (för Oracle-databas)
   - `lc_mysql.xml` (för MySQL-databas)
   - `lc_mssql.xml` (för Microsoft SQL Server-databas)

## Installationssteg

### Steg 1: Skapa arkiv för databasautentiseringsuppgifter

Använd de angivna skripten för att skapa databasens autentiseringsuppgiftslager och lägga till alla lösenordsalias som krävs.

#### I Windows:

**Skriptplats:** `create-elytron-cred-standalone.bat`

`batch cd path\to\script\location create-elytron-cred-standalone.bat`

**Skriptet frågar efter:**
1. **JBOSS_HOME-sökväg** (t.ex. `C:\Adobe\Adobe_Experience_Manager_Forms\jboss`)
2. **Konfigurationsfilnamn** (t.ex. `lc_oracle.xml`, `lc_mysql.xml` eller `lc_mssql.xml`)
3. **Lösenord för arkivering av autentiseringsuppgifter** (detta skyddar nyckelfilen - kom ihåg det här lösenordet)
4. **Lösenord för databas** (ditt faktiska lösenord för databas)

**Vad skriptet gör:**

- Skapar arkiv för autentiseringsuppgifter på: `JBOSS_HOME\standalone\configuration\cred-store.p12`
- Ändrar konfigurationsfilen tillfälligt för att aktivera skapande av arkiv för autentiseringsuppgifter
- Lägger till följande alias med ditt databaslösenord:
   - `EncryptDBPassword`
   - `EncryptDBPassword_IDP_DS`
   - `EncryptDBPassword_EDC_DS`
   - `EncryptDBPassword_AEM_DS`
- Återställer konfigurationsfilen till ursprungligt läge
- Verifierar att alla alias har lagts till

#### I Linux:

**Skriptplats:** `create-elytron-cred-standalone.sh`

`bash cd /path/to/script/location chmod +x create-elytron-cred-standalone.sh./create-elytron-cred-standalone.sh`

**Skriptet frågar efter:**

1. **JBOSS_HOME-sökväg** (t.ex. `/opt/Adobe/Adobe_Experience_Manager_Forms/jboss`)
2. **Konfigurationsfilnamn** (t.ex. `lc_oracle.xml`, `lc_mysql.xml` eller `lc_mssql.xml`)
3. **Lösenord för arkivering av autentiseringsuppgifter** (detta skyddar nyckelfilen - kom ihåg det här lösenordet)
4. **Lösenord för databas** (ditt faktiska lösenord för databas)

**Vad skriptet gör:**

- Skapar arkiv för autentiseringsuppgifter på: `JBOSS_HOME/standalone/configuration/cred-store.p12`
- Ändrar konfigurationsfilen tillfälligt för att aktivera skapande av arkiv för autentiseringsuppgifter
- Lägger till följande alias med ditt databaslösenord:
   - `EncryptDBPassword`
   - `EncryptDBPassword_IDP_DS`
   - `EncryptDBPassword_EDC_DS`
   - `EncryptDBPassword_AEM_DS`
- Återställer konfigurationsfilen till ursprungligt läge
- Verifierar att alla alias har lagts till

**Förväntade utdata:**

```
=== JBoss Elytron Credential Store Setup ===
Enter JBOSS_HOME path (e.g. /opt/jboss): /opt/Adobe/Adobe_Experience_Manager_Forms/jboss
Enter configuration file name (e.g. lc_oracle.xml): lc_oracle.xml
Enter credential store password: ********
Confirm credential store password: ********
Enter database password: ********
Creating credential store using JBoss CLI...
Adding aliases to credential store...
Verifying credential store aliases...

All 4 aliases verified successfully!
- EncryptDBPassword
- EncryptDBPassword_IDP_DS
- EncryptDBPassword_EDC_DS
- EncryptDBPassword_AEM_DS

Credential store setup completed successfully!
```

### Steg 2: Uppdatera fristående konfigurationsfil

När du har kört skriptet måste du konfigurera JBoss för att läsa lösenordet för arkivet med autentiseringsuppgifter vid start.

#### I Windows:

**Filplats:** `<JBOSS_HOME>\bin\standalone.conf.bat`

Exempel: `C:\Adobe\Adobe_Experience_Manager_Forms\jboss\bin\standalone.conf.bat`

Lägg till eller uppdatera följande rad:

```batch
set "JAVA_OPTS=%JAVA_OPTS% -DCS_PASS=YourActualPassword123"
```

Ersätt `YourActualPassword123` med det **lösenord för arkivering av autentiseringsuppgifter** som du använde i steg 1.

#### I Linux:

**Filplats:** `<JBOSS_HOME>/bin/standalone.conf`

Exempel: `/opt/Adobe/Adobe_Experience_Manager_Forms/jboss/bin/standalone.conf`

Lägg till eller uppdatera följande rad:

```bash
JAVA_OPTS="$JAVA_OPTS -DCS_PASS=YourActualPassword123"
```

Ersätt `YourActualPassword123` med det **lösenord för arkivering av autentiseringsuppgifter** som du använde i steg 1.

### Steg 3: Starta JBoss

När du är klar med inställningarna för arkivet med autentiseringsuppgifter startar du JBoss med rätt konfigurationsfil.

**Obs!** Exakt hur du startar JBoss i fristående läge med hjälp av startkommandon och procedurer finns i **huvudinstallationsguiden**. Startkommandona kan variera beroende på din konfiguration och databastyp (`lc_oracle.xml`, `lc_mysql.xml` eller `lc_mssql.xml`).

## Verifiering

### Kontrollera serverloggar

**Loggplats:**

- Windows: `<JBOSS_HOME>\standalone\log\server.log`
- Linux: `<JBOSS_HOME>/standalone/log/server.log`

**Leta efter lyckade startmeddelanden:**

```
INFO  [org.jboss.as.server] WFLYSRV0025: JBoss EAP started
INFO  [org.jboss.as.connector.deployers.jdbc] Bound data source [java:/AdobeDataSource]
```

**Inga fel relaterade till:**

- Inläsning av arkiv för autentiseringsuppgifter
- Databasanslutning
- Alias saknas

## Felsökning

### Problem 1: Det gick inte att hitta något arkiv för autentiseringsuppgifter

**Felmeddelande:**

```
ERROR Unable to load credential store
```

**Lösning:**

1. Kontrollera att arkivfilen för autentiseringsuppgifter finns:
   - Windows: `dir <JBOSS_HOME>\standalone\configuration\cred-store.p12`
   - Linux: `ls -l <JBOSS_HOME>/standalone/configuration/cred-store.p12`
2. Om det saknas kör du skriptet för att skapa autentiseringsuppgifter igen (steg 1)

### Problem 2: Fel lösenord för arkivering av autentiseringsuppgifter

**Felmeddelande:**

```
ERROR Unable to load credential store - Invalid password
```

**Lösning:**
Kontrollera att lösenordet i `standalone.conf.bat` / `standalone.conf` (steg 2) matchar lösenordet som användes när arkivet skapades (steg 1).

**Så här korrigerar du:**
Redigera `standalone.conf.bat` / `standalone.conf` och uppdatera lösenordet:

```
set "JAVA_OPTS=%JAVA_OPTS% -DCS_PASS=CorrectPassword"
```

### Problem 3: Databasanslutningen misslyckades

**Felmeddelande:**

```
ERROR Failed to obtain connection
```

**Lösning:**

1. Kontrollera att det databaslösenord som används i arkivet för autentiseringsuppgifter är korrekt
2. Kontrollera att datakällans konfiguration refererar till rätt alias
3. Verifiera nätverksanslutning till databasservern

**Så här återskapar du arkivet för autentiseringsuppgifter:**

1. Stoppa JBoss
2. Ta bort det befintliga arkivet för autentiseringsuppgifter:
   - Windows: `del <JBOSS_HOME>\standalone\configuration\cred-store.p12`
   - Linux: `rm <JBOSS_HOME>/standalone/configuration/cred-store.p12`
3. Kör skriptet för att skapa autentiseringsuppgifter igen med rätt lösenord för databasen

### Problem 4: Skriptet misslyckas under körning

**Felmeddelande:**

```
ERROR: jboss-cli.bat is not found
```

**Lösning:**
Kontrollera att sökvägen till JBOSS_HOME är korrekt och pekar på JBoss-installationskatalogen.

**Felmeddelande:**

```
ERROR: Configuration file not found
```

**Lösning:**

1. Kontrollera att konfigurationsfilens namn är korrekt
2. Kontrollera att filen finns i katalogen `JBOSS_HOME\standalone\configuration\`
3. Kontrollera att du använder rätt databasspecifika konfigurationsfil

## Snabbreferens

### Databasautentiseringsuppgiftslager (fristående läge)

**Syfte:** Lagra lösenord för databas på ett säkert sätt

**Skript:**

- Windows: `create-elytron-cred-standalone.bat`
- Linux: `create-elytron-cred-standalone.sh`

**Skapar:**

- Fil: `standalone/configuration/cred-store.p12`
- Alias: `EncryptDBPassword`, `EncryptDBPassword_IDP_DS`, `EncryptDBPassword_EDC_DS`, `EncryptDBPassword_AEM_DS`

**Konfiguration:**

- Variabel: `-DCS_PASS=password`
- Fil: `standalone.conf.bat` (Windows) eller `standalone.conf` (Linux)

