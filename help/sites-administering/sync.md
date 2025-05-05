---
title: Användarsynkronisering
description: Lär dig mer om hur användarsynkronisering fungerar i AEM.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
docset: aem65
feature: Security
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: b7b1bce6-9cea-4f13-955f-f9e361f298bf
source-git-commit: a869ffbc6015fd230285838d260434d9c0ffbcb0
workflow-type: tm+mt
source-wordcount: '2224'
ht-degree: 0%

---

# Användarsynkronisering{#user-synchronization}

## Introduktion {#introduction}

När distributionen är en [publiceringsgrupp](/help/sites-deploying/recommended-deploys.md#tarmk-farm) måste medlemmarna kunna logga in och se sina data på valfri publiceringsnod.

Användare och användargrupper (användardata) som har skapats i publiceringsmiljön behövs inte i författarmiljön.

De flesta användardata som skapas i författarmiljön är avsedda att finnas kvar i författarmiljön och inte kopieras till publiceringsinstanser.

Registrering och ändringar som görs på en publiceringsinstans måste synkroniseras med andra publiceringsinstanser för att de ska ha tillgång till samma användardata.

När användarsynkronisering är aktiverat i AEM 6.1 synkroniseras användardata automatiskt mellan publiceringsinstanserna i servergruppen och skapas inte på författaren.

## Sling Distribution {#sling-distribution}

Användardata, tillsammans med deras [ACL](/help/sites-administering/security.md), lagras i [Oak Core](/help/sites-deploying/platform.md), lagret under Oak JCR, och du får åtkomst till dem med [Oak API](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/jackrabbit/oak/api/package-tree.html) . Med ovanliga uppdateringar är det rimligt att användardata synkroniseras med andra publiceringsinstanser med [Sling Content Distribution](https://github.com/apache/sling-old-svn-mirror/blob/trunk/contrib/extensions/distribution/README.md) (Sling-distribution).

Fördelarna med användarsynkronisering med Sling-distribution jämfört med traditionell replikering är:

* *användare*, *användarprofiler* och *användargrupper* som skapats vid publicering skapas inte i författaren

* Sling distribution anger egenskaper i jcr-händelser, vilket gör det möjligt att agera i händelseavlyssnare på publiceringssidan utan att bekymra sig om oändliga replikeringsslingor
* Vid distribution skickas endast användardata till publiceringsinstanser som inte har ursprungsstatus, vilket eliminerar onödig trafik
* [ACL:er](/help/sites-administering/security.md) som angetts i användarnoden ingår i synkroniseringen

>[!NOTE]
>
>Om sessioner krävs rekommenderar vi att du antingen använder en enkel inloggning eller en klisteristisk session, och att kunderna loggar in om de växlar till en annan Publish-instans.

>[!CAUTION]
>
>Synkronisering av gruppen **administratörer** stöds inte, även om användarsynkronisering är aktiverat. I stället loggas ett fel att importera diff-filen i felloggen.
>
>Om en användare läggs till eller tas bort från gruppen **administrators** måste ändringen därför göras manuellt för varje publiceringsinstans när distributionen är en publiceringsgrupp.

## Aktivera användarsynkronisering {#enable-user-sync}

>[!NOTE]
>
>Som standard är användarsynkroniseringen `disabled`.
>
>När du aktiverar användarsynkronisering måste du ändra *befintliga* OSGi-konfigurationer.
>
>Inga nya konfigurationer ska läggas till som ett resultat av aktivering av användarsynkronisering.

Användarsynkronisering förlitar sig på redigeringsmiljön för att hantera distributionen av användardata, även om användardata inte har skapats på författaren. Mycket, men inte allt, av konfigurationen sker i författarmiljön och varje steg identifierar tydligt om den ska utföras på författare eller publicera.

Följande steg krävs för att aktivera användarsynkronisering, följt av avsnittet [Felsökning](#troubleshooting):

### Förutsättningar {#prerequisites}

1. Om användare och användargrupper redan har skapats på en publiceringsinstans rekommenderar vi att du [synkroniserar ](#manually-syncing-users-and-user-groups) användardata manuellt till alla publiceringsinstanser innan du konfigurerar och aktiverar användarsynkronisering.

När användarsynkroniseringen är aktiverad synkroniseras endast nyskapade användare och grupper.

1. Kontrollera att den senaste koden är installerad:

* [AEM plattformsuppdateringar](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=sv-SE)

### 1. Apache Sling Distribution Agent - Sync Agents Factory {#apache-sling-distribution-agent-sync-agents-factory}

**Aktivera användarsynkronisering**

* **på författare**

   * logga in med administratörsbehörighet
   * få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

      * till exempel [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr)

   * leta upp `Apache Sling Distribution Agent - Sync Agents Factory`

      * markera den befintliga konfigurationen så att du kan öppna den för redigering (pennikon)
Verifiera `name`: **`socialpubsync`**

      * markera kryssrutan `Enabled`
      * välj `Save`

![Distributionsagent för Apache Sling](assets/chlimage_1-20.png)

### 2. Skapa behörig användare {#createauthuser}

**Konfigurera behörigheter**

Den auktoriserade användaren används i steg 3 för att konfigurera Sling-distributionen på författaren.

* **för varje publiceringsinstans**

   * logga in med administratörsbehörighet
   * få åtkomst till [säkerhetskonsolen](/help/sites-administering/security.md)

      * till exempel [https://localhost:4503/useradmin](https://localhost:4503/useradmin)

   * skapa en användare

      * till exempel `usersync-admin`

   * lägg till användaren i användargruppen **`administrators`**
   * [lägg till ACL för den här användaren i /home](#howtoaddacl)

      * `Allow jcr:all` med begränsning `rep:glob=*/activities/*`

>[!CAUTION]
>
>En ny användare måste skapas.
>
>* Standardanvändaren som tilldelats är **`admin`**.
>

#### Lägga till ACL {#addacls}

* få tillgång till CRXDE Lite

   * till exempel [https://localhost:4503/crx/de](https://localhost:4503/crx/de)

* välj `/home`-nod
* i den högra rutan väljer du fliken `Access Control`
* om du vill lägga till en ACL-post väljer du knappen `+`

   * **Principal**: *sök efter användare som har skapats för användarsynkronisering*
   * **Typ**: `Allow`
   * **Behörigheter**: `jcr:all`
   * **Begränsningar** `rep:glob`: `*/activities/*`
   * välj **OK**

* välj **Spara alla**

![Lägg till ACL-fönster](assets/chlimage_1-21.png)

Se även

* [Behörighetshantering](/help/sites-administering/user-group-ac-admin.md#access-right-management)
* Felsökningsavsnittet [Ändra åtgärdsundantag under svarsbearbetning](#modify-operation-exception-during-response-processing).

### 3. Adobe Granite Distribution - Krypterad lösenordstransporthemlighetsprovider {#adobegraniteencpasswrd}

**Konfigurera behörigheter**

När en auktoriserad användare som är medlem i användargruppen **`administrators`** har skapats på alla publiceringsinstanser måste den auktoriserade användaren identifieras på författaren som behörig att synkronisera användardata från författare till publicering.

* **på författare**

   * logga in med administratörsbehörighet
   * få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

      * till exempel [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr)

   * leta upp `com.adobe.granite.distribution.core.impl.CryptoDistributionTransportSecretProvider.name`
   * för att öppna för redigering väljer du den befintliga konfigurationen (pennikonen)
Verifiera `property name`: **`socialpubsync-publishUser`**

   * ange användarnamn och lösenord för den [behöriga användaren](#createauthuser) som skapades vid publicering i steg 2

      * till exempel `usersync-admin`

![Krypterad lösenordstransporthemlighetsprovider](assets/chlimage_1-22.png)

### 4. Apache Sling Distribution Agent - Queue Agents Factory {#apache-sling-distribution-agent-queue-agents-factory}

**Aktivera användarsynkronisering**

* **för varje publiceringsinstans**:

   * logga in med administratörsbehörighet
   * få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

      * till exempel [https://localhost:4503/system/console/configMgr](https://localhost:4503/system/console/configMgr)

   * leta upp `Apache Sling Distribution Agent - Queue Agents Factory`

      * för att öppna för redigering väljer du den befintliga konfigurationen (pennikonen)
Verifiera `Name`: `socialpubsync-reverse`

      * markera kryssrutan `Enabled`
      * välj `Save`

   * **upprepa** för varje publiceringsinstans

![Köagenter - fabrik](assets/chlimage_1-23.png)

### 5. Adobe Social Sync - Diff Observer Factory {#diffobserver}

**Aktivera gruppsynkronisering**

* **för varje publiceringsinstans**:

   * logga in med administratörsbehörighet
   * få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

      * till exempel [https://localhost:4503/system/console/configMgr](https://localhost:4503/system/console/configMgr)

   * leta upp **`Adobe Social Sync - Diff Observer Factory`**

      * för att öppna för redigering väljer du den befintliga konfigurationen (pennikonen)

        Verifiera `agent name`: `socialpubsync-reverse`

      * markera kryssrutan `Enabled`
      * välj `Save`

![Diff Observer Factory](assets/screen-shot_2019-05-24at090809.png)

### 6. Apache Sling Distribution Trigger - Factory för schemalagda utlösare {#apache-sling-distribution-trigger-scheduled-triggers-factory}

**(Valfritt) Ändra avsökningsintervallet**

Som standard söker författaren efter ändringar var 30:e sekund. Så här ändrar du intervallet:

* **på författare**

   * logga in med administratörsbehörighet
   * få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

      * till exempel [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr)

   * leta upp `Apache Sling Distribution Trigger - Scheduled Triggers Factory`

      * för att öppna för redigering väljer du den befintliga konfigurationen (pennikonen)

         * Verifiera `Name`: `socialpubsync-scheduled-trigger`

      * ställ in `Interval in Seconds` till önskat intervall
      * välj `Save`

![Schemalagda utlösare - fabrik](assets/chlimage_1-24.png)

## Konfigurera för flera publiceringsinstanser {#configure-for-multiple-publish-instances}

Standardkonfigurationen är för en enda publiceringsinstans. Eftersom orsaken till aktiveringen av användarsynkronisering är att synkronisera flera publiceringsinstanser, t.ex. för en publiceringsgrupp, måste de ytterligare publiceringsinstanserna läggas till i synkroniseringsagentkatalogen.

### 7. Apache Sling Distribution Agent - Sync Agents Factory {#apache-sling-distribution-agent-sync-agents-factory-1}

**Lägg till publiceringsinstanser:**

* **på författare**

   * logga in med administratörsbehörighet
   * få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

      * till exempel [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr)

   * leta upp `Apache Sling Distribution Agent - Sync Agents Factory`

      * för att öppna för redigering väljer du den befintliga konfigurationen (pennikonen)
Verifiera `Name`: `socialpubsync`

![Synkronisera agentfabrik](assets/chlimage_1-25.png)

* **Exportera slutpunkter**
Det ska finnas en exportörslutpunkt för varje Publish-instans. Om det till exempel finns två publiceringsinstanser, localhost:4503 och 4504, ska det finnas två poster:

   * `https://localhost:4503/libs/sling/distribution/services/exporters/socialpubsync-reverse`
   * `https://localhost:4504/libs/sling/distribution/services/exporters/socialpubsync-reverse`

* **Slutpunkter för import**
Det ska finnas en importslutpunkt för varje Publish-instans. Om det till exempel finns två publiceringsinstanser, localhost:4503 och 4504, ska det finnas två poster:

   * `https://localhost:4503/libs/sling/distribution/services/importers/socialpubsync`
   * `https://localhost:4504/libs/sling/distribution/services/importers/socialpubsync`

* välj `Save`

### 8. Unikt ID för försäljning {#unique-sling-id}

>[!CAUTION]
>
>Om Sling ID matchar mellan två eller flera publiceringsinstanser misslyckas synkroniseringen av användargruppen.

Om Sling ID är samma för flera publiceringsinstanser i en publiceringsgrupp synkroniseras inte användargrupperna.

Så här validerar du att alla värden för Sling ID skiljer sig åt för varje Publish-instans:

1. bläddra till `http://<host>:<port>/system/console/status-slingsettings`
1. kontrollera värdet för **Sling ID**

![Kontrollerar värdet för Sling ID](assets/chlimage_1-27.png)

Om Sling ID för en Publish-instans matchar Sling ID för någon annan Publish-instans:

1. stoppa en av publiceringsinstanserna som har ett matchande Sling ID
1. i katalogen crx-quickstart/launchpad/felix

   * sök efter och ta bort filen *sling.id.file*

      * i ett Linux®-system:

        `rm -i $(find . -type f -name sling.id.file)`

      * i ett Windows-system:

        `use windows explorer and search for *sling.id.file*`

1. starta Publish-instansen

   * vid start tilldelas det ett nytt Sling ID

1. validera att **Sling ID** nu är unikt

Upprepa dessa steg tills alla publiceringsinstanser har ett unikt Sling ID.

## Vault Package Builder Factory {#vault-package-builder-factory}

För att uppdateringar ska kunna synkroniseras på rätt sätt måste du ändra valvpaketets byggare för användarsynkronisering:

* på varje AEM Publish-instans
* få åtkomst till [webbkonsolen](/help/sites-deploying/configuring-osgi.md)

   * till exempel [https://localhost:4503/system/console/configMgr](https://localhost:4503/system/console/configMgr)

* hitta `Apache Sling Distribution Packaging - Vault Package Builder Factory`

   * `Builder name: socialpubsync-vlt`

* markera redigeringsikonen
* lägg till två `Package Node Filters`:

   * `/home/users|-.*/.tokens`
   * `/home/users|-.*/rep:cache`

* principhantering:

   * om du vill skriva över befintliga rep:principnoder med nya lägger du till ett tredje paketfilter:

      * `/home/users|+.*/rep:policy`

   * för att förhindra att profiler distribueras, ange

      * `Acl Handling:` `IGNORE`

![Vault Package Builder Factory](assets/vault-package-builder-factory.png)

## Vad händer när ... {#what-happens-when}

### Självregister för användare eller redigeringsprofil vid publicering {#user-self-registers-or-edits-profile-on-publish}

Användare och profiler som skapats i publiceringsmiljön (självregistrering) visas inte i författarmiljön.

När topologin är en [publiceringsgrupp](/help/sites-deploying/recommended-deploys.md#tarmk-farm) och användarsynkroniseringen har konfigurerats korrekt, synkroniseras *användar* och *användarprofilen* över hela publiceringsgruppen med Sling-distribution.

### Användare eller användargrupper skapas med säkerhetskonsolen {#users-or-user-groups-are-created-using-security-console}

Användardata som skapas i publiceringsmiljön visas inte som avsett i redigeringsmiljön och omvänt.

När konsolen [Användaradministration och säkerhet](/help/sites-administering/security.md) används för att lägga till nya användare i publiceringsmiljön synkroniserar användarsynkroniseringen de nya användarna och deras gruppmedlemskap med andra publiceringsinstanser, om det behövs. Användarsynkronisering synkroniserar även användargrupper som skapats via säkerhetskonsolen.

## Felsökning {#troubleshooting}

### Använda användarsynkronisering offline {#how-to-take-user-sync-offline}

Distributionskön måste vara tom och tyst om du vill göra användarsynkroniseringen offline och [ta bort en publiceringsinstans](#how-to-remove-a-publish-instance) eller [manuellt synkronisera data](#manually-syncing-users-and-user-groups).

Så här kontrollerar du status för distributionskön:

* på författare:

   * med [CRXDE Lite](/help/sites-developing/developing-with-crxde-lite.md)

      * sök efter poster i `/var/sling/distribution/packages`

         * mappnoder namngivna med mönstret `distrpackage_*`

   * använder [Package Manager](/help/sites-administering/package-manager.md)

      * söka efter väntande paket (ännu inte installerat)

         * namngiven med mönstret `socialpubsync-vlt*`

Inaktivera användarsynkronisering när distributionskön är tom:

* på författare

   * *uncheck *kryssrutan `Enabled` för [Apache Sling Distribution Agent - Sync Agents Factory](#apache-sling-distribution-agent-sync-agents-factory)

Så här återaktiverar du användarsynkronisering när åtgärder har slutförts:

* på författare

   * markera kryssrutan `Enabled` för [Apache Sling Distribution Agent - Sync Agents Factory](#apache-sling-distribution-agent-sync-agents-factory)

### Diagnostik för användarsynkronisering {#user-sync-diagnostics}

Diagnostik för användarsynkronisering är ett verktyg som kontrollerar konfigurationen och försöker identifiera eventuella problem.

Navigera från huvudkonsolen på författaren via **Verktyg, Åtgärder, Diagnostik, Diagnostik för användarsynkronisering.**

Resultatet visas bara om du anger konsolen för användarsynkronisering.

Detta visas när användarsynkronisering inte har aktiverats:

![Varning! Diagnostik för användarsynkronisering är inte aktiverat](assets/chlimage_1-28.png)

#### Så här kör du diagnostik för publiceringsinstanser {#how-to-run-diagnostics-for-publish-instances}

När diagnostiken körs från redigeringsmiljön innehåller resultatet för godkännande/fel ett [INFO] -avsnitt som visar listan med konfigurerade publiceringsinstanser för bekräftelse.

I listan finns en URL för varje Publish-instans som kör diagnostiken för den instansen. URL-parametern `syncUser` läggs till i diagnostikwebbadressen med värdet inställt på den *auktoriserade synkroniseringsanvändaren* som skapades i [steg 2](#createauthuser).

**Obs!**: Innan du startar URL:en måste den *auktoriserade synkroniseringsanvändaren* redan vara inloggad på den publiceringsinstansen.

![Diagnostik för publiceringsinstanser](assets/chlimage_1-29.png)

### Felaktig konfiguration {#configuration-improperly-added}

När användarsynkroniseringen inte fungerar är det vanligaste problemet att ytterligare konfigurationer *lades till*. I stället borde den befintliga *standardkonfigurationen ha *redigerats*.

Här följer några vyer över hur den redigerade bilden visas standardkonfigurationer i webbkonsolen. Om fler än en instans visas bör den tillagda konfigurationen tas bort.

#### (Författare) En Apache Sling Distribution Agent - Sync Agents Factory {#author-one-apache-sling-distribution-agent-sync-agents-factory}

![Redigerad, standardkonfigurationsvy i webbkonsolen](assets/chlimage_1-30.png)

#### (Författare) En Apache Sling Distribution Transport-autentiseringsuppgifter - Användarautentiseringsuppgifter baserad DistributionTransportSecretProvider {#author-one-apache-sling-distribution-transport-credentials-user-credentials-based-distributiontransportsecretprovider}

![Redigerad, standardkonfigurationsvy i webbkonsolen](assets/chlimage_1-31.png)

#### (Publish) En Apache Sling Distribution Agent - köagentfabrik {#publish-one-apache-sling-distribution-agent-queue-agents-factory}

![Redigerad, standardkonfigurationsvy i webbkonsolen](assets/chlimage_1-32.png)

#### (Publish) One Adobe Social Sync - Diff Observer Factory {#publish-one-adobe-social-sync-diff-observer-factory}

![Redigerad, standardkonfigurationsvy i webbkonsolen](assets/chlimage_1-33.png)

#### (Författare) En utlösare för Apache Sling Distribution - Factory för schemalagda utlösare {#author-one-apache-sling-distribution-trigger-scheduled-triggers-factory}

![Redigerad, standardkonfigurationsvy i webbkonsolen](assets/chlimage_1-34.png)

### Ändra åtgärdsundantag under svarsbearbetning {#modify-operation-exception-during-response-processing}

Om följande syns i loggen:

`org.apache.sling.servlets.post.impl.operations.ModifyOperation Exception during response processing.`

`java.lang.IllegalStateException: This tree does not exist`

Verifiera sedan att avsnittet [2. Skapa auktoriserad användare ](#createauthuser) följdes korrekt.

I det här avsnittet beskrivs hur du skapar en behörig användare, som finns på alla publiceringsinstanser, och identifierar dem i OSGi-konfigurationen för den hemliga providern. Som standard är användaren `admin`.

Den auktoriserade användaren bör göras medlem i användargruppen **`administrators`** och behörigheterna för den gruppen bör inte ändras.

Den auktoriserade användaren bör uttryckligen ha följande behörigheter och begränsningar för alla publiceringsinstanser:

| **sökväg** | **jcr:all** | **rep:glob** |
|---|---|---|
| /home | X | &#42;/aktiviteter/&#42; |
| /home/users | X | &#42;/aktiviteter/&#42; |
| /home/groups | X | &#42;/aktiviteter/&#42; |

Som medlem i gruppen `administrators` bör den auktoriserade användaren ha följande behörigheter för alla publiceringsinstanser:

| **sökväg** | **jcr:all** | **jcr:read** | **rep:write** |
|---|---|---|---|
| /etc/packages/sling/distribution |  |  | X |
| /libs/sling/distribution |  | X |  |
| /var |  |  | X |
| /var/eventing |  | X | X |
| /var/sling/distribution |  | X | X |

### Synkronisering av användargrupp misslyckades {#user-group-sync-failed}

Om Sling ID matchar mellan två eller flera publiceringsinstanser misslyckas synkroniseringen av användargruppen.

Se avsnitt [9. Unikt försäljnings-ID ](#unique-sling-id)

### Synkronisera användare och användargrupper manuellt {#manually-syncing-users-and-user-groups}

* vid publiceringsinstanser där användare och användargrupper finns:

   * [om det är aktiverat, inaktivera användarsynkronisering](#how-to-take-user-sync-offline)
   * [skapa ett paket](/help/sites-administering/package-manager.md#creating-a-new-package) av `/home`

      * när du redigerar paketet

         * Fliken Filter: Lägg till filter: Rotsökväg: `/home`
         * Avancerad flik: AC-hantering: `Overwrite`

   * [exportera paketet](/help/sites-administering/package-manager.md#downloading-packages-to-your-file-system)

* på andra publiceringsinstanser:

   * [importera paketet](/help/sites-administering/package-manager.md#installing-packages)

Om du vill konfigurera eller aktivera användarsynkronisering går du till steg 1: [Apache Sling Distribution Agent - Sync Agents Factory](#apache-sling-distribution-agent-sync-agents-factory)

### När en publiceringsinstans blir otillgänglig {#when-a-publish-instance-becomes-unavailable}

När en publiceringsinstans blir otillgänglig bör den inte tas bort om den kommer tillbaka online i framtiden. Ändringarna köas för Publish-instansen och när den är online igen bearbetas ändringarna.

Om publiceringsinstansen aldrig återgår till onlineläge, om den är permanent offline, måste den tas bort eftersom köbygget resulterar i märkbart diskutrymme i redigeringsmiljön.

När en Publish-instans är nedtryckt har författarloggen följande undantag:

```
28.01.2016 15:57:48.475 ERROR
 [pool-12-thread-34-org_apache_sling_distribution_queue_socialpubsync_endpoint1
 (org/apache/sling/distribution/queue/socialpubsync/endpoint1)]
 org.apache.sling.distribution.agent.impl.SimpleDistributionAgent [agent][socialpubsync] could not deliver package distrpackage_1454014575838_a2b45ec8-0400-42f3-bed8-ae09b66381cb
 org.apache.sling.distribution.packaging.DistributionPackageImportException: failed in importing package ...
```

### Så här tar du bort en publiceringsinstans {#how-to-remove-a-publish-instance}

Om du vill ta bort en publiceringsinstans från [Apache Sling Distribution Agent - Sync Agents Factory](#apache-sling-distribution-agent-sync-agents-factory) måste distributionskön vara tom och tyst.

* på författare:

   * [Ta användarsynkronisering offline](#how-to-take-user-sync-offline)
   * Följ [steg 7](#apache-sling-distribution-agent-sync-agents-factory) för att ta bort Publish-instansen från båda serverlistorna:

      * `Exporter Endpoints`
      * `Importer Endpoints`

   * återaktivera användarsynkronisering

      * markera kryssrutan `Enabled` för [Apache Sling Distribution Agent - Sync Agents Factory](#apache-sling-distribution-agent-sync-agents-factory)
