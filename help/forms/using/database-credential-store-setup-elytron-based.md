---
title: Lagringsinställningar för databasautentiseringsuppgifter (Elytron-baserad)
description: JBoss EAP 8 har stöd för Elytron-autentiseringsarkiv för säker lösenordshantering i AEM Forms, med automatiserade skript för konfiguration av domänläge.
solution: Experience Manager
feature: Deploying
role: User,Admin,Developer
hide: true
index: false
hidefromtoc: true
source-git-commit: 5d020671efaa4527a5f6dbb4b779c7a3351888a4
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---


# Lagringsinställningar för databasautentiseringsuppgifter (Elytron-baserad)

## Konfigurera databasens autentiseringsuppgiftslagring med Elytron

JBoss EAP 8 använder **Elytron-autentiseringslager** för att hantera databaslösenord säkert för AEM Forms-distributioner. Adobe tillhandahåller **automatiserade skript** som gör det enklare att skapa och konfigurera det Elytron-baserade arkivet med autentiseringsuppgifter i domänläge.

Installationen måste slutföras **innan JBoss-domänkontrollanten** startas.

### Förutsättningar

* **JBoss-servern måste stoppas helt** innan skriptet för att skapa autentiseringsuppgifter körs.
* Skapande av arkiv med autentiseringsuppgifter måste utföras i **offlineläge endast**.

Så här stoppar du JBoss om den körs:

* **Windows**

  ```
  <JBOSS_HOME>\bin\jboss-cli.bat --connect command=:shutdown
  ```

* **Linux/UNIX**

  ```
  <JBOSS_HOME>/bin/jboss-cli.sh --connect command=:shutdown
  ```

### Hämta skript

Hämta rätt skript baserat på ditt operativsystem:

| Script Name | Operativsystem |
| -------------------------------- | ---------------- |
| `create-elytron-cred-domain.bat` | Windows |
| `create-elytron-cred-domain.sh` | Linux/UNIX |

För Linux gör du skriptet körbart:

```
chmod +x create-elytron-cred-domain.sh
```

### Konfigurationssteg

#### Steg 1: Hämta och montera skriptet

Hämta lämpligt skript och placera det i följande katalog:

```
<JBOSS_HOME>/bin
```

#### Steg 2: Kör skriptet

* **Windows**

  ```
  cd <JBOSS_HOME>\bin
  create-elytron-cred-domain.bat
  ```

* **Linux/UNIX**

  ```
  cd <JBOSS_HOME>/bin
  ./create-elytron-cred-domain.sh
  ```

#### Steg 3: Ange nödvändig information

Under körningen uppmanas skriptet att ange följande indata:

1. **JBOSS_HOME-sökväg**
Ange den fullständiga sökvägen till JBoss-installationskatalogen.

2. **Namn på databaskonfigurationsfil**
Ange något av följande baserat på din databas:

   * `domain_oracle.xml`
   * `domain_mysql.xml`
   * `domain_mssql.xml`

3. **Lösenord för arkivering av autentiseringsuppgifter**
Ange ett starkt lösenord för att skydda arkivet med autentiseringsuppgifter.

   > Lösenordet är dolt vid inmatning och måste sparas i senare steg.

4. **Databaslösenord**
Ange det faktiska lösenordet för databasanslutningen.

#### Steg 4: Skriptkörning och validering

Skriptet utför följande åtgärder automatiskt:

* Skapar `cred-store.p12` i:

  ```
  <JBOSS_HOME>/domain/configuration/
  ```

* Skapar följande autentiseringsalias:

   * `EncryptDBPassword`
   * `EncryptDBPassword_IDP_DS`
   * `EncryptDBPassword_EDC_DS`
   * `EncryptDBPassword_AEM_DS`
* Verifierar att alla alias har lagts till

Körningen bekräftar att arkivet med autentiseringsuppgifter har skapats och att alias har verifierats.

#### Steg 5: Konfigurera JVM-alternativ

Uppdatera JBoss-domänens startkonfiguration för att ange lösenordet för arkivet med autentiseringsuppgifter.

* **Linux**
Redigera:

  ```
  <JBOSS_HOME>/bin/domain.conf
  ```

  Lägg till:

  ```
  JAVA_OPTS="$JAVA_OPTS -DCS_PASS=YourCredStorePassword"
  ```

* **Windows**
Redigera:

  ```
  <JBOSS_HOME>/bin/domain.conf.bat
  ```

  Lägg till:

  ```
  set "JAVA_OPTS=%JAVA_OPTS% -DCS_PASS=YourCredStorePassword"
  ```

Ersätt `YourCredStorePassword` med det lösenord som angavs när arkivet med autentiseringsuppgifter skapades.

Domänkonfigurationsfilerna refererar till det här värdet med variabeln `${CS_PASS}`.


#### Steg 6: Verifiera domänkonfiguration

Öppna konfigurationsfilen för databasdomänen:

```
<JBOSS_HOME>/domain/configuration/<domain_*.xml>
```

Kontrollera att datakällorna refererar till Elytron-autentiseringsuppgiftslagringen:

```xml
<security>
    <user-name>your_database_username</user-name>
    <credential-reference store="db-creds" alias="EncryptDBPassword_IDP_DS"/>
</security>
```

Varje datakälla använder ett specifikt alias:

* **IDP_DS:** `EncryptDBPassword_IDP_DS`
* **EDC_DS:** `EncryptDBPassword_EDC_DS`
* **AEM_DS:** `EncryptDBPassword_AEM_DS`
* **DefaultDS / ExampleDS:** `EncryptDBPassword`

Alla alias refererar till samma databaslösenord som lagras i arkivet för autentiseringsuppgifter.

>[!NOTE]
>
>* Konfigurera bara arkivet för autentiseringsuppgifter på den primära noden.
>* Sekundära noder använder automatiskt den domänkonfiguration som synkroniseras från den primära noden.
