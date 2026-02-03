---
title: Installation av sekundär nodautentisering (Elytron-baserad)
description: JBoss EAP 8 använder Elytron för säker kommunikation och registrering av sekundära noder med den primära domänkontrollanten.
solution: Experience Manager
feature: Deploying
role: User,Admin,Developer
source-git-commit: 259cb81eb9652405dc7270535cbf9deb996ad2ac
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Installation av sekundär nodautentisering (Elytron-baserad)

## Konfigurera autentisering av sekundär nod med Elytron

JBoss EAP 8 använder **Elytron** för att autentisera kommunikationen mellan **primära och sekundära noder** i en klustrad distribution. Den här konfigurationen säkerställer säker registrering och kommunikation av sekundära noder med den primära domänkontrollanten.

Det finns två installationsalternativ beroende på miljö och säkerhetskrav.

## Förutsättningar

* En **hanteringsanvändare med namnet`secondary`** måste skapas på den **primära noden**.
* Utför den här konfigurationen **endast på sekundära noder**.
* Upprepa konfigurationen för **varje sekundär nod** i klustret.
* **JBoss måste vara helt stoppad** på både primära och sekundära noder.
* Alla åtgärder för arkivering av autentiseringsuppgifter måste utföras i **offlineläge**.

Så här stoppar du JBoss om den körs:

* **Windows**

  ```
  <JBOSS_HOME>\bin\jboss-cli.bat --connect command=:shutdown
  ```

* **Linux/UNIX**

  ```
  <JBOSS_HOME>/bin/jboss-cli.sh --connect command=:shutdown
  ```

## Välj ett inställningsalternativ

* **Alternativ 1: Snabbinställning använder standardarkivet för autentiseringsuppgifter**
Rekommenderas för lägre miljöer och testning.

* **Alternativ 2: Inställningar för anpassad lagring av autentiseringsuppgifter**
Rekommenderas för produktion och säkra miljöer.

## Alternativ 1: Snabbinställning med standardarkivet för autentiseringsuppgifter

**Passar bäst för:** Utvecklings-, testnings- och snabbinstallationsscenarier.

### Ökning

* En standardfil för autentiseringsuppgifter (`cs_secondary_hc.p12`) är förkonfigurerad.
* Standardlösenordet för arkivet med autentiseringsuppgifter har redan angetts i `domain.conf`.
* Det är bara lösenordsaliaset för autentisering som behöver läggas till.

### Konfigurationssteg

#### Steg 1: Verifiera standardarkivet för autentiseringsuppgifter

Bekräfta att standardarkivfilen för autentiseringsuppgifter finns:

* **Windows**

  ```
  <JBOSS_HOME>\domain\configuration\cs_secondary_hc.p12
  ```

* **Linux**

  ```
  <JBOSS_HOME>/domain/configuration/cs_secondary_hc.p12
  ```

Om filen inte finns använder du **Alternativ 2**.

#### Steg 2: Lägg till alias för autentiseringslösenord

Kör följande kommando från `<JBOSS_HOME>/bin`:

* **Windows**

  ```
  elytron-tool.bat credential-store --location "../domain/configuration/cs_secondary_hc.p12" --password "password" --type KeyStoreCredentialStore --properties "keyStoreType=PKCS12" --add "secondary_hc_auth" --secret "ActualSecondaryUserPassword"
  ```

* **Linux**

  ```
  ./elytron-tool.sh credential-store --location "../domain/configuration/cs_secondary_hc.p12" --password "password" --type KeyStoreCredentialStore --properties "keyStoreType=PKCS12" --add "secondary_hc_auth" --secret "ActualSecondaryUserPassword"
  ```

> Det hemliga värdet måste matcha lösenordet som används när användaren `secondary` skapas på den primära noden.

#### Steg 3: Verifiera konfigurationen för domain.conf

Kontrollera att följande post redan finns (inga ändringar krävs):

* **Windows**

  ```
  set "JAVA_OPTS=%JAVA_OPTS% -DSec_Auth_PASS=password"
  ```

* **Linux**

  ```
  JAVA_OPTS="$JAVA_OPTS -DSec_Auth_PASS=password"
  ```

#### Steg 4: Starta noderna

1. Starta den **primära noden** och vänta tills den har initierats helt.
2. Starta den **sekundära noden**.

### Verifiering

Kontrollera loggarna:

* **Primär nod**

  ```
  <JBOSS_HOME>/domain/log/host-controller.log
  ```

  ```
  Registered remote secondary host "secondary"
  ```

* **Sekundär nod**

  ```
  Connected to primary host controller
  ```

## Alternativ 2: Anpassad inställning för arkiv för autentiseringsuppgifter (produktion)

**Passar bäst för:** Produktionsmiljöer som kräver förbättrad säkerhet.

### Konfigurationssteg

#### Steg 1: Ta bort standardarkiv för autentiseringsuppgifter (om det finns någon)

Om standardarkivet för autentiseringsuppgifter finns ändrar du namn på det:

* **Windows**

  ```
  rename cs_secondary_hc.p12 cs_secondary_hc.p12.bak
  ```

* **Linux**

  ```
  mv cs_secondary_hc.p12 cs_secondary_hc.p12.bak
  ```

#### Steg 2: Skapa anpassad autentiseringsuppgiftslagring

Från `<JBOSS_HOME>/bin`:

* **Windows**

  ```
  elytron-tool.bat credential-store --create --location "../domain/configuration/cs_secondary_hc.p12" --password "YourCustomPassword" --type KeyStoreCredentialStore --properties "keyStoreType=PKCS12"
  ```

* **Linux**

  ```
  ./elytron-tool.sh credential-store --create --location "../domain/configuration/cs_secondary_hc.p12" --password "YourCustomPassword" --type KeyStoreCredentialStore --properties "keyStoreType=PKCS12"
  ```

#### Steg 3: Lägg till alias för autentiseringslösenord

* **Windows**

  ```
  elytron-tool.bat credential-store --location "../domain/configuration/cs_secondary_hc.p12" --password "YourCustomPassword" --type KeyStoreCredentialStore --properties "keyStoreType=PKCS12" --add "secondary_hc_auth" --secret "ActualSecondaryUserPassword"
  ```

* **Linux**

  ```
  ./elytron-tool.sh credential-store --location "../domain/configuration/cs_secondary_hc.p12" --password "YourCustomPassword" --type KeyStoreCredentialStore --properties "keyStoreType=PKCS12" --add "secondary_hc_auth" --secret "ActualSecondaryUserPassword"
  ```

#### Steg 4: Uppdatera domain.conf

Uppdatera lösenordsreferensen för arkivet med autentiseringsuppgifter:

* **Windows**

  ```
  set "JAVA_OPTS=%JAVA_OPTS% -DSec_Auth_PASS=YourCustomPassword"
  ```

* **Linux**

  ```
  JAVA_OPTS="$JAVA_OPTS -DSec_Auth_PASS=YourCustomPassword"
  ```

#### Steg 5: Verifiera XML-konfiguration

Kontrollera att `host-secondary.xml` innehåller de konfigurerade autentiseringsuppgiftslagringsplatsen och autentiseringsklientposterna.
Inga ändringar krävs om standardkonfigurationen finns.


#### Steg 6: Starta noderna

1. Starta den **primära noden** och vänta tills den har startats helt.
2. Starta den **sekundära noden**.

### Verifiering

Bekräfta registreringen med värdstyrenhetens loggar på båda noderna.

## Sammanfattning

* Med alternativet **1** kan du snabbt konfigurera med hjälp av ett förkonfigurerat arkiv för autentiseringsuppgifter.
* **Alternativ 2** aktiverar starkare säkerhet med hjälp av ett lösenord för en anpassad autentiseringsuppgift.
* Konfigurationen måste slutföras **endast på sekundära noder**.
* Den primära nodkonfigurationen återanvänds automatiskt i hela domänen.

