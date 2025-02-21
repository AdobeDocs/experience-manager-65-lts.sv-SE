---
title: Underhållsaktiviteter före uppgraderingen
description: Läs mer om de uppgifter före uppgradering som rekommenderas för AEM.
contentOwner: sarchiz
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: upgrading
docset: aem65
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2014'
ht-degree: 0%

---

# Underhållsaktiviteter före uppgraderingen{#pre-upgrade-maintenance-tasks}

Innan du påbörjar uppgraderingen är det viktigt att du följer dessa underhållsåtgärder för att vara säker på att systemet är klart och kan återställas om det skulle uppstå problem:

* [Se till att det finns tillräckligt med diskutrymme](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#ensure-sufficient-disk-space)
* [Fullständig säkerhetskopiering av AEM](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#fully-back-up-aem)
* [Säkerhetskopiera ändringar i /etc](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#backup-changes-etc)
* [Generera filen quickstart.properties](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#generate-quickstart-properties)
* [Konfigurera rensning av arbetsflöde och granskningslogg](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#configure-wf-audit-purging)
* [Installera, konfigurera och köra uppgifter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#install-configure-run-pre-upgrade-tasks)
* [Inaktivera anpassade inloggningsmoduler](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#disable-custom-login-modules)
* [Ta bort uppdateringar från katalogen /install](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#remove-updates-install-directory)
* [Stoppa alla väntelägesförekomster i kallt läge](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#stop-tarmk-coldstandby-instance)
* [Inaktivera anpassade schemalagda jobb](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#disable-custom-scheduled-jobs)
* [Kör rensning av offlineredigering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#execute-offline-revision-cleanup)
* [Kör skräpinsamling för datastore](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#execute-datastore-garbage-collection)
* [Uppgradera databasschemat om det behövs](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#upgradethedatabaseschemaifneeded)
* [Ta bort användare som kan tyda på en uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#delete-users-that-might-hinder-the-upgrade)

* [Rotera loggfiler](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#rotate-log-files)

## Se till att det finns tillräckligt med diskutrymme {#ensure-sufficient-disk-space}

När du utför uppgraderingen måste du utföra en databasmigrering, utöver content and code upgrade-aktiviteterna. Migreringen skapar en kopia av databasen i det nya segmenttjärformatet. Därför behöver du tillräckligt med diskutrymme för att behålla en andra, eventuellt större, version av databasen.

## Fullständig säkerhetskopiering av AEM {#fully-back-up-aem}

AEM bör säkerhetskopieras fullständigt innan uppgraderingen påbörjas. Säkerhetskopiera databasen, programinstallationen, datalagret och Mongo-instanserna om tillämpligt. Mer information om säkerhetskopiering och återställning av en AEM-instans finns i [Säkerhetskopiering och återställning](/help/sites-administering/backup-and-restore.md).

## Säkerhetskopiera ändringar i /etc {#backup-changes-etc}

Uppgraderingsprocessen gör det bra att underhålla och sammanfoga befintligt innehåll och befintliga konfigurationer från sökvägarna `/apps` och `/libs` i databasen. För ändringar som gjorts i sökvägen `/etc`, inklusive konfigurationer för kontextnav, är det ofta nödvändigt att tillämpa ändringarna igen efter uppgraderingen. När uppgraderingen gör en säkerhetskopia av alla ändringar som inte kan sammanfogas under `/var` rekommenderar Adobe att du säkerhetskopierar dessa ändringar manuellt innan du påbörjar uppgraderingen.

## Generera filen quickstart.properties {#generate-quickstart-properties}

När AEM startas från jar-filen skapas en `quickstart.properties`-fil under `crx-quickstart/conf`. Om AEM endast har startats med det tidigare startskriptet finns inte den här filen och uppgraderingen misslyckas. Kontrollera att filen finns och starta om AEM från filen jar om den inte finns.

## Konfigurera rensning av arbetsflöde och granskningslogg {#configure-wf-audit-purging}

Aktiviteterna `WorkflowPurgeTask` och `com.day.cq.audit.impl.AuditLogMaintenanceTask` kräver separata OSGi-konfigurationer och kan inte fungera utan dem. Om de inte fungerar när en uppgift körs före uppgraderingen är det mest troligt att konfigurationer saknas. Se därför till att du lägger till OSGi-konfigurationer för dessa uppgifter eller tar bort dem helt och hållet från listan över uppgifter som ska optimeras före uppgraderingen om du inte vill köra dem. Dokumentation om hur du konfigurerar rensningsaktiviteter för arbetsflöden finns på [Administrera arbetsflödesinstanser](/help/sites-administering/workflows-administering.md) och konfigurationen av underhållsaktiviteter för granskningsloggar finns på [Underhåll för granskningslogg i AEM 6](/help/sites-administering/operations-audit-log.md).

Mer information om rensning av arbetsflödes- och granskningslogg på CQ 5.6 och rensning av granskningslogg på AEM 6.0 finns i [Rensa arbetsflödes- och granskningsnoder](https://helpx.adobe.com/experience-manager/kb/howtopurgewf.html).

## Installera, konfigurera och köra uppgifter före uppgradering {#install-configure-run-pre-upgrade-tasks}

På grund av den nivå av anpassning som AEM tillåter följer normalt inte miljöer ett enhetligt sätt att utföra uppgraderingar. Därför är det svårt att skapa en standardiserad uppgraderingsprocedur.

I tidigare versioner var det också svårt för AEM uppgraderingar som stoppats eller som inte återställts på ett säkert sätt. Detta ledde till situationer då en omstart av det fullständiga uppgraderingsförfarandet var nödvändig eller där felaktiga uppgraderingar utfördes utan att några varningar utlöstes.

För att åtgärda dessa problem har Adobe lagt till flera förbättringar i uppgraderingsprocessen, vilket gör den mer flexibel och användarvänlig. Underhållsuppgifter före uppgradering som tidigare skulle utföras manuellt optimeras och automatiseras. Rapporter efter uppgraderingen har lagts till så att processen kan granskas i sin helhet i hopp om att problemen blir enklare.

Underhållsuppgifter före uppgradering sprids för närvarande över olika gränssnitt som delvis eller helt utförs manuellt. Tack vare den förbättrade underhållsoptimeringen som introducerades i AEM 6.3 kan dessa uppgifter utlösas på ett enhetligt sätt och resultatet kan kontrolleras vid behov.

Alla uppgifter som ingår i uppgraderingsoptimeringssteget är kompatibla med alla versioner från och med AEM 6.0.

### Så här konfigurerar du {#how-to-set-it-up}

I AEM 6.3 och senare finns underhållsoptimeringen i snabbstartsbehållaren.

<!-- URLs below are all 404s. This content should probably be removed because it is entirely obsolete.

If you are upgrading from an older version of AEM 6, they are made available through separate packages that you can download from the Package Manager.

You can find the packages at these locations:

* [For upgrading from AEM 6.0](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq600/product/pre-upgrade-tasks-content-cq60)

* [For upgrading from AEM 6.1](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq610/product/pre-upgrade-tasks-content-cq61)

* [For upgrading from AEM 6.2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq620/product/pre-upgrade-tasks-content-cq62) -->

### Så här använder du den {#how-to-use-it}

OSGI-komponenten `PreUpgradeTasksMBean` levereras förkonfigurerad med en lista över underhållsuppgifter som kan köras alla på en gång. Du kan konfigurera uppgifterna genom att följa proceduren nedan:

1. Gå till webbkonsolen genom att gå till *https://serveraddress:serverport/system/console/configMgr*

1. Sök efter **föruppgraderingsuppgifter** och klicka sedan på den första matchande komponenten. Komponentens fullständiga namn är `com.adobe.aem.upgrade.prechecks.mbean.impl.PreUpgradeTasksMBeanImpl`

1. Ändra listan över underhållsaktiviteter som måste köras enligt nedan:

   ![1487758925984](assets/1487758925984.png)

Uppgiftslistan varierar beroende på vilket körningsläge som används för att starta instansen. Nedan visas en beskrivning av det körläge som varje underhållsåtgärd är avsedd för.

<table>
 <tbody>
  <tr>
   <td><strong>Uppgift</strong></td>
   <td><strong>Körningsläge</strong></td>
   <td><strong>Anteckningar</strong></td>
  </tr>
  <tr>
   <td><code>TarIndexMergeTask</code></td>
   <td>crx2</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>DataStoreGarbageCollectionTask</code></td>
   <td>crx2</td>
   <td>Kör mark och svepning. För delade datalager tar du bort det här steget och kör <br /> manuellt eller korrekt och förbereder instanser innan du kör.</td>
  </tr>
  <tr>
   <td><code>ConsistencyCheckTask</code></td>
   <td>crx2</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>WorkflowPurgeTask</code></td>
   <td>crx2/crx3</td>
   <td>Adobe Granite Workflow Renge Configuration OSGi måste konfigureras innan programmet körs.</td>
  </tr>
  <tr>
   <td><code>GenerateBundlesListFileTask</code></td>
   <td>crx2/crx3</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>RevisionCleanupTask</code></td>
   <td>crx3</td>
   <td>Om du använder TonaMK-instanser på AEM 6.0 till 6.2 kan du köra Revision Cleanup manuellt i stället.</td>
  </tr>
  <tr>
   <td><code>com.day.cq.audit.impl.AuditLogMaintenanceTask</code></td>
   <td>crx3</td>
   <td>Du måste konfigurera OSGi-konfigurationen för rensning av granskningslogg innan du kör.</td>
  </tr>
 </tbody>
</table>

>[!CAUTION]
>
>`DataStoreGarbageCollectionTask` anropar en skräpinsamlingsåtgärd för datastore med markerings- och svepfasen om den används. För distributioner som använder ett delat datalager måste du antingen konfigurera om det korrekt eller förbereda instansen för att undvika att objekt som refereras av en annan instans tas bort. Den här processen kan kräva att markeringsfasen körs manuellt på alla instanser innan den här föruppgraderingsaktiviteten aktiveras.

### Standardkonfiguration för hälsokontroller före uppgradering {#default-configuration-of-the-pre-upgrade-health-checks}

OSGI-komponenten `PreUpgradeTasksMBeanImpl` levereras förkonfigurerad med en lista över hälsokontrollstaggar som ska köras före uppgraderingen när `runAllPreUpgradeHealthChecks` -metoden anropas:

* **system** - taggen som används av hälsokontrollerna för granitunderhåll

* **föruppgradering** - en anpassad tagg som kan läggas till i alla hälsokontroller som du kan ställa in att köra före en uppgradering

Listan kan redigeras. Du kan använda plus- **(+)** och minusknapparna **(-)** förutom taggarna för att lägga till fler anpassade taggar eller ta bort standardtaggar.

**MBean-metoder**

Du kan komma åt funktionen för hanterade bönor med hjälp av [JMX-konsolen](/help/sites-administering/jmx-console.md).

Du kan komma åt MBeans genom att:

1. Gå till JMX-konsolen på *https://serveraddress:serverport/system/console/jmx*
1. Sök efter **PreUpgradeTasks** och klicka på resultatet

1. Välj en metod i avsnittet **Åtgärder** och välj **Anropa** i följande fönster.

Nedan visas en lista med alla tillgängliga metoder som `PreUpgradeTasksMBeanImpl` visar:

<table>
 <tbody>
  <tr>
   <td><strong>Metodnamn</strong></td>
   <td><strong>Typ</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td><code>getAvailablePreUpgradeTasksNames()</code></td>
   <td>INFO</td>
   <td>Visar en lista med tillgängliga namn på underhållsaktiviteter före uppgradering.</td>
  </tr>
  <tr>
   <td><code>getAvailablePreUpgradeHealthChecksTagNames()</code></td>
   <td>INFO</td>
   <td>Visar en lista med taggar för hälsokontroller som är före uppgraderingen.</td>
  </tr>
  <tr>
   <td><code>runAllPreUpgradeTasks()</code></td>
   <td>ÅTGÄRD</td>
   <td>Kör alla underhållsaktiviteter som är före uppgraderingen i listan.</td>
  </tr>
  <tr>
   <td><code>runPreUpgradeTask(preUpgradeTaskName)</code></td>
   <td>ÅTGÄRD</td>
   <td>Kör underhållsaktiviteten före uppgradering med det namn som anges som parameter.</td>
  </tr>
  <tr>
   <td><code>isRunAllPreUpgradeTaskRunning()</code></td>
   <td>ACTION_INFO</td>
   <td>Kontrollerar om aktiviteten <code>runAllPreUpgradeTasksmaintenance</code> körs.</td>
  </tr>
  <tr>
   <td><code>getAnyPreUpgradeTaskRunning()</code></td>
   <td>ACTION_INFO</td>
   <td>Kontrollerar om någon underhållsuppgift som körs före uppgraderingen körs och <br /> returnerar en matris som innehåller namnen på de aktiviteter som körs för tillfället.</td>
  </tr>
  <tr>
   <td><code>getPreUpgradeTaskLastRunTime(preUpgradeTaskName)</code></td>
   <td>ÅTGÄRD</td>
   <td>Visar den exakta körningstiden för underhållsaktiviteten före uppgradering med det namn som anges som parameter.</td>
  </tr>
  <tr>
   <td><code>getPreUpgradeTaskLastRunState(preUpgradeTaskName)</code></td>
   <td>ÅTGÄRD</td>
   <td>Visar det senaste körningstillståndet för underhållsaktiviteten före uppgradering med det namn som anges som parameter.</td>
  </tr>
  <tr>
   <td><code>runAllPreUpgradeHealthChecks(shutDownOnSuccess)</code></td>
   <td>ÅTGÄRD</td>
   <td><p>Kör alla hälsokontroller som är före uppgraderingen och sparar deras status i en fil med namnet <code>preUpgradeHCStatus.properties</code> som finns i startsökvägen för sling. Om parametern <code>shutDownOnSuccess</code> är inställd på <code>true</code> stängs AEM-instansen av, men bara om alla hälsokontroller före uppgraderingen har statusen OK.</p> <p>Egenskapsfilen används som ett villkor för framtida uppgradering <br /> och uppgraderingsprocessen stoppas om hälsokontrollen <br /> som utförts före uppgraderingen misslyckades. Om du vill ignorera resultatet av föruppgraderingskontrollerna <br /> och starta uppgraderingen ändå, kan du ta bort filen.</p> </td>
  </tr>
  <tr>
   <td><code>detectUsageOfUnavailableAPI(aemVersion)</code></td>
   <td>ÅTGÄRD</td>
   <td>Visar alla importerade paket som inte längre är uppfyllda när <br /> uppgraderar till den angivna AEM-versionen. AEM-målversionen måste vara <br /> angiven som parameter.</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>MBean-metoderna kan anropas via:
>
>* JMX-konsolen
>* Alla externa program som ansluter till JMX
>* cURL
>

## Inaktivera anpassade inloggningsmoduler {#disable-custom-login-modules}

>[!NOTE]
>
>Det här steget krävs bara om du uppgraderar från en version av AEM 5. Den kan helt och hållet hoppas över för uppgraderingar från äldre versioner av AEM 6.

Det sätt som anpassade `LoginModules` konfigureras för autentisering på databasnivå har ändrats i Apache Oak på ett fundamentalt sätt.

I AEM-versioner som använde CRX2-konfiguration placerades den i filen `repository.xml`, medan den från och med AEM 6 görs i tjänsten Apache Felix JAAS Configuration Factory via webbkonsolen.

Alla befintliga konfigurationer måste därför inaktiveras och återskapas för Apache Oak efter uppgraderingen.

Om du vill inaktivera de anpassade moduler som definierats i JAAS-konfigurationen för `repository.xml` måste du redigera konfigurationen så att standardvärdet `LoginModule` används, som i följande exempel:

```xml
<Security >
             ....
          <!--
                 Use LoginModule authenticating against repository itself
                 -->
                 <LoginModule class = "com.day.crx.core.CRXLoginModule" >
                     <param name = "anonymousId" value = "anonymous" />
                     <param name = "adminId" value ="admin" />
                     <param name = "disableNTLMAuth" value = "true" />
                     <param name = "tokenExpiration" value = "43200000" />
                     <!-- param name="trust_credentials_attribute" value="d5b9167e95dad6e7d3b5d6fa8df48af8"/
                -->
                 </LoginModule >
         </ Security>
```

>[!NOTE]
>
>Mer information finns i [Autentisering med modulen för extern inloggning](https://jackrabbit.apache.org/oak/docs/security/authentication/externalloginmodule.html).
>
>Ett exempel på `LoginModule`-konfiguration i AEM 6 finns i [Konfigurera LDAP med AEM 6](/help/sites-administering/ldap-config.md).

## Ta bort uppdateringar från katalogen /install {#remove-updates-install-directory}

>[!NOTE]
>
>Ta endast bort paket från katalogen crx-quickstart/install när du har stängt AEM-instansen. Det här steget är ett av de sista innan du startar uppgraderingsproceduren på plats.

Ta bort alla Service Pack, funktionspaket eller snabbkorrigeringar som har distribuerats via katalogen `crx-quickstart/install` i det lokala filsystemet. Detta förhindrar oavsiktlig installation av gamla snabbkorrigeringar och servicepaket ovanpå den nya AEM-versionen när uppdateringen har slutförts.

## Stoppa alla väntelägesförekomster i kallt läge {#stop-tarmk-coldstandby-instance}

Om du använder kallstart för TjäraMK ska du stoppa alla kalliga standby-instanser. Detta garanterar ett effektivt sätt att återansluta till Internet om det uppstår problem i uppgraderingen. När uppgraderingen har slutförts måste instanserna i kallt vänteläge återskapas från de uppgraderade primära instanserna.

## Inaktivera anpassade schemalagda jobb {#disable-custom-scheduled-jobs}

Inaktivera alla schemalagda OSGi-jobb som ingår i programkoden.

## Kör rensning av offlineredigering {#execute-offline-revision-cleanup}

>[!NOTE]
>
>Detta steg är endast nödvändigt för bensinanläggningar

Om du använder tarMK bör du köra Revision Cleanup offline innan du uppgraderar. Detta gör att databasmigreringssteget och efterföljande uppgraderingsuppgifter körs mycket snabbare och hjälper till att säkerställa att rensning av onlineändringar kan utföras korrekt när uppgraderingen har slutförts. Information om hur du kör rensning av offlineredigering finns i [Utför rensning av offlineredigering](/help/sites-deploying/storage-elements-in-aem-6.md#performing-offline-revision-cleanup).

## Kör skräpinsamling för datastore {#execute-datastore-garbage-collection}

>[!NOTE]
>
>Det här steget är bara nödvändigt för instanser som kör crx3

När du har kört revisionsrensning på CRX3-instanser bör du köra Datastore Garbage Collection för att ta bort alla blobbar som inte refereras i datalagret. Instruktioner finns i dokumentationen om [skräpinsamlingen för datalagret](/help/sites-administering/data-store-garbage-collection.md).

## Uppgradera databasschemat om det behövs {#upgrade-the-database-schema-if-needed}

Vanligtvis tar den underliggande Apache Oak-stacken som AEM använder för beständighet hand om uppgraderingen av databasschemat, om det behövs.

Det kan dock inträffa när schemat inte kan uppgraderas automatiskt. Sådana fall är oftast högsäkerhetsmiljöer där databasen körs under en användare med begränsad behörighet. Om en sådan situation inträffar fortsätter AEM att använda det gamla schemat.

Om du vill förhindra att ett sådant scenario inträffar uppgraderar du schemat genom att göra följande:

1. Stäng den AEM-instans som måste uppgraderas.
1. Uppgradera databasschemat. Läs dokumentationen för din databastyp för att se vilka verktyg som krävs för att uppnå resultatet.

   Mer information om hur Oak hanterar schemauppgraderingar finns på [den här sidan på Apache-webbplatsen](https://jackrabbit.apache.org/oak/docs/nodestore/document/rdb-document-store.html#upgrade).

1. Fortsätt med uppgraderingen av AEM.

## Ta bort användare som kan tyda på en uppgradering {#delete-users-that-might-hinder-the-upgrade}

>[!NOTE]
>
>Underhållsuppgifterna är bara nödvändiga om:
>
>* Du uppgraderar från AEM-versioner äldre än AEM 6.3
>* Du stöter på något av de fel som nämns nedan under uppgraderingen.
>

Det finns exceptionella fall när serviceanvändare kan få en äldre version av AEM som felaktigt taggas som vanliga användare.

Om en sådan situation uppstår misslyckas uppgraderingen med ett meddelande som följande:

```
ERROR [Apache Sling Repository Startup Thread] com.adobe.granite.repository.impl.SlingRepositoryManager Exception in a SlingRepositoryInitializer, SlingRepository service registration aborted
java.lang.RuntimeException: Unable to create service user [communities-utility-reader]:java.lang.RuntimeException: Existing user communities-utility-reader is not a service user.
```

Se till att du gör följande för att undvika problemet:

1. Frigör instansen från produktionstrafiken
1. Skapa en säkerhetskopia av en eller flera användare som orsakar problemet. Du kan utföra den här uppgiften med Pakethanteraren. Mer information finns i [Arbeta med paket.](/help/sites-administering/package-manager.md)
1. Ta bort en eller flera användare som orsakar problemet. Nedan finns en lista över användare som kan ingå i den här kategorin:

   1. `dynamic-media-replication`
   1. `communities-ugc-writer`
   1. `communities-utility-reader`
   1. `communities-user-admin`
   1. `oauthservice`
   1. `sling-scripting`

## Rotera loggfiler {#rotate-log-files}

Adobe rekommenderar att du arkiverar dina aktuella loggfiler innan du påbörjar uppgraderingen. På så sätt blir det enklare att övervaka och skanna loggfilerna under och efter uppgraderingen för att identifiera och lösa eventuella problem som kan uppstå.
