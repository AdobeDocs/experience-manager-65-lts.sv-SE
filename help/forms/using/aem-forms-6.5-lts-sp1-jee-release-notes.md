---
title: Versionsinformation för  [!DNL Adobe Experience Manager] 6.5 LTS SP1
description: Hitta versionsinformation, nyheter, installationsanvisningar och en detaljerad ändringslista för  [!DNL Adobe Experience Manager] 6.5 LTS SP1
solution: Experience Manager
feature: Release Information
role: User,Admin,Developer
source-git-commit: 27ec3c516b0746fd7e0d82f86750fbb4ef410711
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# Versionsinformation om Adobe Experience Manager (AEM) Forms 6.5 LTS SP1 på JEE

## Versionsinformation

| **Produkt** | Adobe Experience Manager Forms |
| -------------------- | ------------------------------------ |
| **Version** | 6.5 LTS SP1 |
| **Typ** | JEE (Long-Term Support Service Pack) |
| **Versionskategori** | Uppgraderingsversion |
| **Hämta URL** | Programvarudistribution |

## Vad ingår i [!DNL Experience Manager] 6.5 LTS SP1 {#what-is-included-in-aem-65ltssp1}

Adobe Experience Manager (AEM) Forms **6.5 LTS SP1 på JEE** innehåller nya funktioner, viktiga plattformsuppdateringar som kunden efterfrågat och allmänna felkorrigeringar, med starkt fokus på produktstabilitet och långsiktig support.

## Vad ingår i AEM Forms 6.5 LTS SP1?

### &#x200B;1. Java Support-uppdateringar

Stöd för nyare Java-versioner har införts:

* **Java™ 17**
* **Java™ 21**

### &#x200B;2. Programserversupport - uppdateringar

#### Stöd för JBoss EAP 8

* Stöd för **JBoss EAP 8** har lagts till.
* Det äldre säkerhetsramverket **PicketBox** har tagits bort.
* **Elytron-baserade autentiseringsuppgiftslager** stöds nu för säker uppgiftshantering.

##### Konfiguration: Autentiseringsarkiv (Elytron-baserad)

AEM Forms på JBoss EAP 8 använder Elytron för att hantera säkra inloggningsuppgifter. Kunderna måste konfigurera ett Elytron-baserat autentiseringsarkiv för autentiseringsuppgifter för att säkerställa att servern startas korrekt och att databasautentiseringen är säker.

Konfigurationsinformation finns i installations- och konfigurationshandboken.

### &#x200B;3. Plattforms- och kompatibilitetsändringar

#### Stöd för serverspecifikationer

* Stöd för **serverspecifikation, 5+**
* Baserat på **Jakarta EE 9** -kompatibilitet

#### Krav för migrering av namnområde

* Jakarta EE 9 introducerar en namnområdesändring från `javax.*` till `jakarta.*`
* Alla **anpassade DSC:er** måste migreras till namnutrymmet `jakarta.*`
* AEM Forms 6.5 LTS SP1 stöder **endast Jakarta EE 9+-baserade programservrar**

Mer information finns i **Migrering från javax till jakarta Namespace**.

## Uppgradera

Detaljerade uppgraderingsinstruktioner finns i [uppgraderingshandboken för AEM Forms 6.5 LTS SP1 på JEE](https://experienceleague.adobe.com/sv/docs/experience-manager-65-lts/content/forms/upgrade-aem-forms/upgrade)

## Installation

Installationsanvisningar och krav finns i **Installationshandboken för AEM Forms 6.5 LTS SP1 (JEE)**.

## Plattformar som stöds

En fullständig lista över plattformar, operativsystem, databaser och programservrar som stöds finns i:

**Plattformsmatris som stöds - AEM Forms 6.5 LTS SP1 (JEE)**

## Föråldrade och borttagna funktioner

* Stöd för **RDBMK** för CRX-databasbeständighet har tagits bort.
* För klustrade miljöer stöds **endast MongoMK** för databasbeständighet.

## Migrering från javax till jakarta Namespace

### Migrering från `javax` till `jakarta`-namnområde

Från och med **AEM Forms 6.5 LTS SP1** stöds endast programservrar som implementerar **Jakarta Servlet API 5/6**. Med **Jakarta EE 9 och senare** gick alla API:er från namnområdet `javax.{}` till `jakarta.`.

Därför måste **alla anpassade DSC:er använda `jakarta` namespace**. Anpassade komponenter som skapats med `javax.{}` API:er är **inte kompatibla** med de programservrar som stöds.

### Migreringsalternativ för anpassade DSC

Du kan migrera befintliga anpassade DSC:er på något av följande sätt:

#### Alternativ 1: Source Code Migration (Recommended)

* Uppdatera alla importsatser från `javax.{}` till `jakarta.`
* Återskapa och kompilera om de anpassade DSC-projekten
* Distribuera om de uppdaterade komponenterna till programservern

**Fördelar:**

* Garanterar långsiktig kompatibilitet med Jakarta EE 9+
* Passar bäst för projekt som underhålls aktivt

#### Alternativ 2: Binär migrering med Eclipse Transformer

* Använd verktyget **Eclipse Transformer** för att konvertera kompilerade binärfiler (`.jar`, `.war`) från `javax` till `jakarta`
* Inga källkodsändringar eller omkompilering krävs
* Distribuera om de omformade binärfilerna till programservern

>[!NOTE]
>
> Binär omformning utförs på **bytekodnivå**.

Exempelimportmappningar

Nedan visas vanliga exempel på namnutrymmesändringar som krävs under migreringen:

Före (javax)    Efter (jakarta)
javax.servlet.*    jakarta.servlet.*
javax.servlet.http.*    jakarta.servlet.http.*

### Exempelimportmappningar

I följande tabell visas vanliga namnområdesändringar som krävs vid migrering från `javax` till `jakarta`:

| Före (`javax`) | Efter (`jakarta`) |
| ---------------------- | ------------------------ |
| `javax.servlet.*` | `jakarta.servlet.*` |
| `javax.servlet.http.*` | `jakarta.servlet.http.*` |

Använd dessa mappningar som referens när du uppdaterar anpassad DSC-källkod eller validerar transformerade binärfiler.

