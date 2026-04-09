---
title: Uppgraderar AEM 6.5 LTS i JBoss EAP 8 (Windows)
description: Den här guiden innehåller stegvisa instruktioner för uppgradering av en befintlig Adobe Experience Manager (AEM) 6.5 LTS-installation från JBoss EAP 7.4 till JBoss EAP 8 i Windows med JDK 21.
source-git-commit: 835530039678bc16a6de87b8d580be91a2026f94
workflow-type: tm+mt
source-wordcount: '1374'
ht-degree: 0%

---

# Uppgraderar AEM 6.5 LTS i JBoss EAP 8 (Windows)

## Översikt

Den här guiden innehåller stegvisa instruktioner för uppgradering av en befintlig Adobe Experience Manager (AEM) 6.5 LTS-installation från JBoss EAP 7.4 till JBoss EAP 8 i Windows med JDK 21.

**Uppgraderingssökväg:** JBoss EAP 7.4 (JDK 11) → JBoss EAP 8 (JDK 21)

## Viktiga meddelanden

>[!NOTE]
>
>Det här är en kritisk uppgraderingsprocedur. Utför alltid uppgraderingen i en icke-produktionsmiljö först och upprätthåll fullständiga säkerhetskopior.
>
> **&#x200B; FÖRUTSÄTTNINGAR:** Fullständig säkerhetskopiering av systemet och en dokumenterad återställningsplan är obligatoriska innan du fortsätter.

## Krav före uppgradering

### Systemkrav

| Komponent | Krav |
|-----------|-------------|
| Operativsystem | Windows Server 2016 eller senare (64-bitars) |
| Source Environment | JBoss EAP 7.4 med AEM 6.5 LTS |
| Målmiljö | JBoss EAP 8.x |
| Java Development Kit | JDK 21 (Oracle eller OpenJDK) |
| AEM Version | AEM 6.5 Service Pack (senaste rekommenderas) |
| Diskutrymme | Minst 50 GB ledigt utrymme för uppgraderingsprocessen |

### Nödvändiga nedladdningar

Innan du påbörjar uppgraderingen bör du skaffa följande:

1. **JBoss EAP 8.0-distribution**\
   Hämta från: [https://developers.redhat.com/products/eap/download](https://developers.redhat.com/products/eap/download)

2. **JDK 21 Installer**\
   Ladda ned Oracle JDK 21 eller OpenJDK 21 för Windows (64 bitar)

3. **AEM 6.5 LTS WAR-fil**\
   Hämta senaste AEM 6.5 Service Pack WAR från Adobe Software Distribution

## Steg 1: Skapa fullständig säkerhetskopiering

>[!IMPORTANT]
>
> Utför omfattande säkerhetskopiering innan du fortsätter.

### Checklista för säkerhetskopiering

- [ ] Fullständig säkerhetskopiering av befintlig JBoss EAP 7.4-installationskatalog
- [ ] Säkerhetskopiering av mappen `crx-repository`
- [ ] Säkerhetskopiering av mappen `crx-quickstart`
- [ ] Exportera alla anpassade konfigurationer
- [ ] säkerhetskopiering av databas (om extern databas används)
- [ ] Dokumentera aktuellt systemtillstånd och aktuella konfigurationer

### Skapa säkerhetskopia

```cmd
# Example backup location
C:\AEM-Backups\Pre-Upgrade-<date>

# Copy entire JBoss 7.4 directory
xcopy "C:\jboss-eap-7.4" "C:\AEM-Backups\Pre-Upgrade-<date>\jboss-eap-7.4" /E /I /H
```

**Rekommenderas:** Lagra säkerhetskopior på en separat enhet eller nätverksplats.

## Steg 2: Installera JBoss EAP 8

### Extrahera JBoss EAP 8

1. Extrahera JBoss EAP 8 ZIP-distributionen till din målinstallationskatalog:

   ```
   C:\jboss-eap-8.0
   ```

2. Observera den här katalogsökvägen som `<JBOSS_HOME>` som kan användas i den här guiden.

### Replikera katalogstruktur

Se till att den nya JBoss EAP 8-installationen har samma anpassade katalogstruktur som den tidigare JBoss EAP 7.4-installationen, särskilt:

- Anpassade distributionskataloger
- Extern konfigurationsmapp
- Loggfilsplatser
- Alla anpassade moduler eller bibliotek

## Steg 3: Migrera databasdata

### Copy CRX Repository

1. Navigera till din befintliga JBoss EAP 7.4-installation:

   ```
   <OLD_JBOSS_HOME>\bin\crx-repository
   ```

2. Kopiera hela mappen `crx-repository` till den nya JBoss EAP 8-installationen:

   ```cmd
   xcopy "C:\jboss-eap-7.4\bin\crx-repository" "C:\jboss-eap-8.0\bin\crx-repository" /E /I /H
   ```

**Viktigt!** Den här mappen innehåller din innehållsdatabas och måste överföras helt.

### Verifiera arkivkopia

Efter kopiering kontrollerar du att databasens storlek och struktur matchar källan:

```cmd
dir "C:\jboss-eap-8.0\bin\crx-repository" /s
```

## Steg 4: Stoppa AEM-instans

Kontrollera att AEM är helt stoppat innan du gör några ändringar.

### Stoppa via Windows Services

1. Öppna **Tjänster** (Kör: `services.msc`)
2. Hitta AEM/JBoss-tjänsten
3. Högerklicka och välj **Stopp**
4. Vänta tills tjänsten stoppas helt

### Stoppa via kommandorad

Om AEM startades manuellt:

1. Gå till JBoss-konsolfönstret
2. Tryck på `Ctrl+C`
3. Vänta på att den försiktiga avstängningen slutförs

### Verifiera avstängning

Kontrollera att Java-processen inte längre körs:

```cmd
tasklist | findstr java
```

## Steg 5: Rensa äldre AEM-filer

Ta bort inaktuella filer från katalogen `crx-quickstart` för att säkerställa en ren uppgradering.

>[!CAUTION]
>
> Ta bara bort de filer och mappar som listas nedan. Ta inte bort andra konfigurationsfiler, anpassad kod eller databasdata.

### 5.1 Ta bort startmapp för startfönstret

**Plats:**

```
<JBOSS_HOME>\bin\crx-repository\crx-quickstart\launchpad\startup
```

**Åtgärd:**

```cmd
rd /s /q "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\launchpad\startup"
```

**Syfte:** Den här mappen innehåller gamla OSGi-paket som genereras om under uppgraderingen.

### 5.2 Ta bort bas-JAR-fil

**Plats:**

```
<JBOSS_HOME>\bin\crx-repository\crx-quickstart\launchpad\org.apache.sling.launchpad.base.jar
```

**Åtgärd:**

```cmd
del "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\launchpad\org.apache.sling.launchpad.base.jar"
```

**Syfte:** Denna JAR kommer att ersättas med versionen från den nya WAR-filen.

### 5.3 Ta bort Bootstrap kommandofil

**Plats:**

```
<JBOSS_HOME>\bin\crx-repository\crx-quickstart\launchpad\felix\bundle0\BootstrapCommandFile_*.txt
```

**Åtgärd:**

```cmd
del "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\launchpad\felix\bundle0\BootstrapCommandFile_*.txt"
```

**Syfte:** Bootstrap-kommandon genereras om för den nya miljön.

### 5.4 Ta bort sling.options-fil

**Plats:**

```
<JBOSS_HOME>\bin\crx-repository\crx-quickstart\launchpad\felix\sling.options.file
```

**Åtgärd:**

```cmd
del "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\launchpad\felix\sling.options.file"
```

### 5.5 Ta bort sling_bootstrap.txt-fil

**Plats:**

```
<JBOSS_HOME>\bin\crx-repository\crx-quickstart\launchpad\sling_bootstrap.txt
```

**Åtgärd:**

```cmd
del "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\launchpad\sling_bootstrap.txt"
```

### 5.6 Säkerhetskopiera och ta bort sling.properties-fil

Den här filen innehåller miljöspecifika konfigurationer som kan behöva sammanfogas senare.

**Plats:**

```
<JBOSS_HOME>\bin\crx-repository\crx-quickstart\conf\sling.properties
```

**Åtgärd:**

1. **Skapa säkerhetskopia:**

   ```cmd
   copy "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\conf\sling.properties" "C:\AEM-Backups\sling.properties.backup"
   ```

2. **Ta bort original:**

   ```cmd
   del "C:\jboss-eap-8.0\bin\crx-repository\crx-quickstart\conf\sling.properties"
   ```

**Syfte:** En ny `sling.properties` genereras. Granska säkerhetskopian för att återställa anpassade konfigurationer efter uppgraderingen.

## Steg 6: Installera och konfigurera JDK 21

### Installera JDK 21

1. Kör installationsprogrammet för JDK 21 för Windows
2. Installera på en standardplats (t.ex. `C:\Program Files\Java\jdk-21`)
3. Slutför installationsguiden

### Konfigurera miljövariabler

#### Ange JAVA_HOME

1. Öppna **Systemegenskaper** → **Avancerat** → **Miljövariabler**
2. Under **Systemvariabler** klickar du på **Nytt**
3. Uppsättning:
   - Variabelnamn: `JAVA_HOME`
   - Variabelvärde: `C:\Program Files\Java\jdk-21`
4. Klicka på **OK**

#### Uppdatera PATH-variabel

1. I **Systemvariabler** väljer du `Path` och klickar på **Redigera**
2. Lägg till ny post:

   ```
   %JAVA_HOME%\bin
   ```

3. Flytta den här posten högst upp i listan för att säkerställa att JDK 21 har företräde
4. Klicka på **OK** i alla dialogrutor

### Verifiera Java-installation

1. Öppna en **ny**-kommandotolk (för att läsa in uppdaterade miljövariabler)
2. Verifiera Java-version:

   ```cmd
   java -version
   ```

   Förväntade utdata:

   ```
   java version "21.0.x"
   Java(TM) SE Runtime Environment (build 21.0.x+...)
   Java HotSpot(TM) 64-Bit Server VM (build 21.0.x+..., mixed mode, sharing)
   ```

3. Verifiera JAVA_HOME:

   ```cmd
   echo %JAVA_HOME%
   ```

## Steg 7: Konfigurera JVM-inställningar

Innan du distribuerar AEM måste du konfigurera lämpliga JVM-minnesinställningar för produktion.

### Redigera fristående.conf.bat

1. Navigera till:

   ```
   <JBOSS_HOME>\bin
   ```

2. Öppna `standalone.conf.bat` i en textredigerare (som administratör)

3. Leta reda på eller lägg till `JAVA_OPTS`-konfigurationen:

   ```batch
   rem # AEM Production JVM Settings
   set "JAVA_OPTS=-Xms4096m -Xmx4096m -XX:MaxMetaspaceSize=768m"
   set "JAVA_OPTS=%JAVA_OPTS% -Djava.net.preferIPv4Stack=true"
   set "JAVA_OPTS=%JAVA_OPTS% -Dfile.encoding=UTF-8"
   set "JAVA_OPTS=%JAVA_OPTS% -server"
   ```

4. Spara och stäng filen

**Rekommenderade inställningar:**

| Parameter | Rekommenderat värde | Beskrivning |
|-----------|-------------------|-------------|
| `-Xms` | 4 096-8 192 m | Ursprunglig stackstorlek |
| `-Xmx` | 4 096-8 192 m | Maximal stackstorlek |
| `-XX:MaxMetaspaceSize` | 768 m - 1 024 m | Metaspace-gräns |

**Obs!** Justera värden baserat på serverns tillgängliga minnes- och arbetsbelastningskrav.

## Steg 8: Distribuera AEM 6.5 LTS WAR

### Förbered WAR-fil

Kontrollera att AEM WAR-filen är korrekt konfigurerad enligt distributionsguiden:

- `jboss-deployment-structure.xml` finns
- `web.xml` innehåller inställningar för flera konfigurationer
- WAR-filen packas om om några ändringar gjorts

### Distribuera till JBoss

1. Kopiera AEM 6.5 LTS WAR-filen till installationskatalogen:

   ```cmd
   copy "C:\AEM-Downloads\cq-quickstart-6.5.xx.war" "C:\jboss-eap-8.0\standalone\deployments\cq-quickstart.war"
   ```

**Viktigt!** Kontrollera att WAR-filnamnet matchar den önskade URL-kontextsökvägen.

## Steg 9: Starta JBoss EAP 8 med AEM

### Starta servern

1. Öppna **Kommandotolken** som **administratör**

2. Navigera till bin-katalogen för JBoss:

   ```cmd
   cd C:\jboss-eap-8.0\bin
   ```

3. Starta JBoss EAP 8:

   ```cmd
   standalone.bat -b 0.0.0.0 -bmanagement 0.0.0.0
   ```

### Startar bildskärmen

Titta på konsolutdata för:

1. **WAR-distribution:**

   ```
   Deployed "cq-quickstart.war" (runtime-name : "cq-quickstart.war")
   ```

2. **Initieringsmeddelanden för AEM:**

   ```
   Apache Sling Application Launcher
   Sling Home: crx-repository/crx-quickstart
   ```

3. **Databasuppgradering (om tillämpligt):**

   ```
   Performing repository migration...
   ```

**Förväntad starttid:** 5-15 minuter beroende på databasstorlek och systemresurser.

## Steg 10: Verifiera att uppgraderingen lyckades

### Kontrollera AEM Startup

Övervaka JBoss-konsolen för det slutliga startmeddelandet:

```
**** AEM started successfully ****
```

### Öppna AEM-gränssnitt

1. Öppna en webbläsare
2. Navigera till:

   ```
   http://localhost:8080/cq-quickstart
   ```

3. Logga in med administratörsuppgifter:
   - Användarnamn: `admin`
   - Lösenord: `admin` (eller ditt anpassade lösenord)

### Verifiera systeminformation

1. Navigera till **Verktyg** → **Åtgärder** → **Webbkonsol**

   ```
   http://localhost:8080/cq-quickstart/system/console
   ```

2. Klicka på **Systeminformation**

3. Verifiera:

   - **JVM-version:** Ska visa Java 21
   - **JBoss-version:** Ska visa EAP 8.x
   - **AEM-version:** ska visa 6.5.xx

### Kontrollera systemhälsa

Navigera till **Verktyg** → **Åtgärder** → **Diagnos** om du vill köra hälsokontroller:

- Paketstatus: Alla paket ska vara &quot;Aktiv&quot;
- Resursupplösning: Ska visa felfri status
- Frågeprestanda: Granska eventuella försämringar

## Uppgifter efter uppgradering

### Återställ anpassade konfigurationer

1. Granska den säkerhetskopierade `sling.properties`-filen
2. Återställ anpassade körningslägen eller konfigurationer till den nya filen:

   ```
   <JBOSS_HOME>\bin\crx-repository\crx-quickstart\conf\sling.properties
   ```

3. Starta om AEM om konfigurationerna ändrats

### Uppdatera replikeringsagenter

1. Navigera till **Verktyg** → **Distribution** → **Replikering** → **Agenter på författare**
2. Granska och testa alla replikeringsagenter
3. Uppdatera eventuella hårdkodade referenser till gamla serversökvägar

### Testa kritiska funktioner

- [ ] Skapa och publicera innehåll
- [ ] Överföring och bearbetning av resurser
- [ ] Arbetsflödeskörning
- [ ] användarautentisering
- [ ] integreringsslutpunkter
- [ ] anpassade komponenter och mallar

### Prestandaoptimering

1. Granska och rensa tillfälliga cacheminnen
2. Övervaka systemprestanda under den inledande användningen
3. Justera JVM-inställningar vid behov baserat på faktiska användningsmönster

## Felsökning

### Vanliga problem

| Problem | Möjlig orsak | Lösning |
|-------|---------------|----------|
| AEM startar inte | Felaktig Java-version | Verifiera `JAVA_HOME` punkter till JDK 21 |
| Fel på grund av databasfel | Ofullständig databaskopia | Återställ från säkerhetskopierings- och återkopieringsarkivet |
| OutOfMemoryError | Otillräckligt stackminne | Öka `-Xmx` i `standalone.conf.bat` |
| Paket i läget &quot;Installerad&quot; | Beroenden saknas | Kontrollera paketberoenden i webbkonsolen |
| Port 8080 används redan | En annan tjänst använder port | Stoppa tjänsten i konflikt eller ändra JBoss-port |

### Loggfilsplatser

- **JBoss-serverlogg:**\
  `<JBOSS_HOME>\standalone\log\server.log`

- **AEM-fellogg:**\
  `<JBOSS_HOME>\bin\crx-repository\crx-quickstart\logs\error.log`

- **AEM Access Log:**\
  `<JBOSS_HOME>\bin\crx-repository\crx-quickstart\logs\access.log`

### Återställningsprocedur

Om uppgraderingen misslyckas och inte kan lösas:

1. Stoppa JBoss EAP 8
2. Återställ den fullständiga säkerhetskopian av JBoss EAP 7.4
3. Återställ mappen `crx-repository`
4. Verifiera `JAVA_HOME` punkter till JDK 11 (vid återställning)
5. Starta föregående miljö

## Bästa praxis

### Före produktionsdistribution

- [ ] Testa den fullständiga uppgraderingsprocessen i en utvecklingsmiljö
- [ ] Testa i en staging-miljö med produktionsliknande data
- [ ] Dokumentera alla anpassade konfigurationer och integreringar
- [ ] Skapa en detaljerad återställningsplan
- [ ] Schemalägg uppgradering under underhållsperioden
- [ ] Meddela alla intressenter om planerade driftstopp

### Efter lyckad uppgradering

- [ ] Övervaka systemloggar i 48-72 timmar
- [ ] Utför inläsningstestning för att identifiera prestandaproblem
- [ ] Uppdatera systemdokumentation
- [ ] Utbilda team i skillnader i JBoss EAP 8
- [ ] Arkivera all uppgraderingsdokumentation och säkerhetskopior

## Relaterad dokumentation

- [Migreringshandbok för JBoss EAP 8](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/8.0/html/migration_guide/)
- [Adobe Experience Manager 6.5 - uppgraderingshandbok](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/upgrading/upgrade.html?lang=sv-SE)
- [AEM installerar Service Pack](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/sp-release-notes.html?lang=sv-SE)

## Dokumentinformation

| Fält | Värde |
|-------|-------|
| Dokumentversion | 1,0 |
| Senast uppdaterad | Februari 2026 |
| AEM Version | 6,5 LTS |
| Source Platform | JBoss EAP 7.4 / JDK 11 |
| Målplattform | JBoss EAP 8.x / JDK 21 |
| Operativsystem | Windows Server |

**Juridiskt meddelande:** Adobe, Adobe Experience Manager och AEM är registrerade varumärken som tillhör Adobe Inc. JBoss och Red Hat är registrerade varumärken som tillhör Red Hat, Inc.
