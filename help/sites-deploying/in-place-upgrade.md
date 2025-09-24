---
title: Utföra en uppgradering på plats
description: Lär dig hur du uppgraderar till AEM 6.5 LTS på plats.
topic-tags: upgrading
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: c7351625-b29e-45a7-b966-e7c0f56d4f22
source-git-commit: 57bf39aa914bddca05d526b46b581579965069d6
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---

# Utföra en uppgradering på plats {#performing-an-in-place-upgrade}

>[!NOTE]
>
>På den här sidan beskrivs uppgraderingsproceduren för AEM 6.5 LTS. Om du har en installation som distribueras till en programserver läser du [Uppgradera steg för programserverinstallationer](/help/sites-deploying/app-server-upgrade.md).

## Steg före uppgradering {#pre-upgrade-steps}

Innan du utför uppgraderingen måste du utföra flera steg. Mer information finns i [Uppgradera kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md) och [Underhållsaktiviteter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md). Kontrollera dessutom att datorn uppfyller [kraven för AEM 6.5 LTS](/help/sites-deploying/technical-requirements.md) och se [planeringshänsyn](/help/sites-deploying/upgrade-planning.md) samt hur [Analyzer](/help/sites-deploying/pattern-detector.md) kan hjälpa dig att beräkna komplexiteten.

## Krav för migrering {#migration-prerequisites}

* **Minimikrav på Java-version:** Kontrollera att Oracle Java™ 17/21 är installerat på datorn.

## Förberedelse av filen AEM Quickstart jar {#prep-quickstart-file}

1. Ladda ned nya AEM 6.5 LTS jar-filen

1. [Kontrollera rätt startkommando för uppgradering](#determining-the-correct-upgrade-start-command)

1. Stoppa instansen om den körs

1. Använd nya AEM 6.5 LTS jar för att ersätta den gamla utanför mappen `crx-quickstart`

1. Gör en säkerhetskopia av filen `sling.properties` (finns vanligtvis i `crx-quickstart/conf/`) och ta sedan bort den

1. Packa upp den nya snabbstartsburken genom att köra:

   ```shell
   java -Xmx4096m -jar aem-quickstart.jar -unpack
   ```

1. Om du behöver använda anpassade sling.properties skapar du en ny lokal AEM-instans och hämtar filen sling.properties från katalogen crx-quickstart/conf. Använd de anpassade ändringarna i den här filen och kopiera den sedan till katalogen crx-quickstart/conf för den AEM-instans som uppgraderas. Om det inte finns några anpassade egenskaper kan det här steget hoppas över.

<!-- Alexandru: drafting temporarily

## Content Repository Migration {#content-repository-migration}

This migration is not required if you are upgrading from AEM 6.3. For versions older than 6.3, Adobe provides a tool that can be used to migrate the repository to the new version of the Oak Segment Tar present in AEM 6.3. It is provided as part of the quickstart package and is mandatory for any upgrades that will be using TarMK. Upgrades for environments that are using MongoMK do not require repository migration. For more information on what the benefits of the new Segment Tar format are, see the [Migrating to Oak Segment Tar FAQ](/help/sites-deploying/revision-cleanup.md#online-revision-cleanup-frequently-asked-questions).

The actual migration is performed using the standard AEM quickstart jar file, executed with a new `-x crx2oak` option which executes the crx2oak tool to simplify the upgrade and make it more robust.

>[!NOTE]
>
>If you are performing TarMK repository content migration using the CRX2Oak Quickstart extension, you might remove the **samplecontent** runmode by adding the following to the migration command line:
>
>* `--promote-runmode nosamplecontent`
>

To determine the command that you should run, use the following command:

```shell
java -Xmx4096m -jar aem-quickstart.jar -v -x crx2oak -xargs -- --load-profile <<YOUR_PROFILE>> <<ADDITIONAL_FLAGS>>
```

Where `<<YOUR_PROFILE>>` and `<<ADDITIONAL_FLAGS>>` are replaced with the profile and flags listed in the following table:

<table>
 <tbody>
  <tr>
   <td><strong>Source Repository</strong></td>
   <td><strong>Target Repository</strong></td>
   <td><strong>Profile</strong></td>
   <td><strong>Additional Flags</strong><br /> </td>
  </tr>
  <tr>
   <td>crx2 or TarMK with <code>FileDataStore</code></td>
   <td>TarMK</td>
   <td>segment-fds</td>
   <td>See Troubleshooting section below</td>
  </tr>
  <tr>
   <td>crx2</td>
   <td>MongoMK</td>
   <td>mongo-from-crx2 </td>
   <td><code>-T mongo-uri=mongo://mongo-host:mongo-port -T mongo-db=mongo-database-name</code></td>
  </tr>
  <tr>
   <td>TarMK or crx2 with <code>S3DataStore</code></td>
   <td>TarMK</td>
   <td>segment-custom-ds</td>
   <td>See Troubleshooting section below</td>
  </tr>
  <tr>
   <td>TarMK with no datastore</td>
   <td>TarMK</td>
   <td>segment-no-ds</td>
   <td> </td>
  </tr>
  <tr>
   <td>MongoMK</td>
   <td>MongoMK</td>
   <td>No migration is needed</td>
   <td> </td>
  </tr>
 </tbody>
</table>

**Where:**

* `mongo-host` is the MongoDB server IP (for example, 127.0.0.1)

* `mongo-port` is the MongoDB server port (for example: 27017)

* `mongo-database-name` represents the name of the database (for example: aem-author)

**You may also require additional switches for the following scenarios:**

* If you are performing the upgrade on a Windows system where Java memory mapping is not handled correctly, add the `--disable-mmap` parameter to the command.

For additional instructions on using the crx2oak tool, see Using the [CRX2Oak Migration Tool](/help/sites-deploying/using-crx2oak.md). The crx2oak helper JAR can be manually upgraded if needed, by manually replacing it with newer versions after unpacking the quickstart. Its location in the AEM installation folder is: `<aem-install>/crx-quickstart/opt/extensions/crx2oak.jar`. The newest version of the CRX2Oak migration tool is available for download from the Adobe Repository at: [https://repo1.maven.org/maven2/com/adobe/granite/crx2oak/](https://repo1.maven.org/maven2/com/adobe/granite/crx2oak/)

If the migration has completed successfully, the tool will exit with an exit code of zero. Additionally, check for WARN and ERROR messages in the `upgrade.log` file, located under `crx-quickstart/logs` in the AEM installation directory, as these could indicate non-fatal errors that occurred during the migration.

Check the configuration files beneath `crx-quickstart/install` folder. If a migration was necessary these will be updated to reflect the target repository.

**A note on datastores:**

While `FileDataStore` is the new default for AEM 6.3 installations, using an external datastore is not required. While using an external datastore is recommended as a best practice for production deployments, it is not a prerequisite to upgrade. Due to the complexity already present in upgrading AEM, Adobe recommends performing the upgrade without doing a datastore migration. If desired, a datastore migration can be executed afterwards as a separate effort.

## Troubleshooting Migration Issues {#troubleshooting-migration-issues}

Skip this section if you are upgrading from 6.3. While the provided crx2oak profiles should meet the needs of most customers, there are times when additional parameters will be necessary. If you run into an error during your migration, it is possible that there are aspects of your environment that require additional configuration options to be provided. If so, you will likely encounter the following error:

**Checkpoints are not copied, because no external datastore has been specified. This will result in the full repository reindexing on the first start. Use --skip-checkpoints to force the migration or see https://jackrabbit.apache.org/oak/docs/migration.html#Checkpoints_migration for more info.**

For some reason, the migration process needs access to binaries in the datastore and is unable to find it. To specify your datastore configuration, include the following flags in the `<<ADDITIONAL_FLAGS>>` portion of your migration command:

**For S3 datastores:**

```shell
--src-s3config=/path/to/SharedS3DataStore.config --src-s3datastore=/path/to/datastore
```

Where `/path/to/SharedS3DataStore.config` represents the path to your S3 datastore config file and `/path/to/datastore` represents the path to your S3 datastore.

**For File datastores:**

```shell
--src-datastore=/path/to/datastore
```

Where `/path/to/datastore` represents the path to your File Datastore.

-->

## Utföra uppgraderingen {#performing-the-upgrade}

**Om S3 används:**

1. Ta bort alla tecken under `crx-quickstart/install` som är associerade med en tidigare version av S3-anslutningen.

1. Hämta den senaste versionen av 1.60.2 S3-anslutningen från [https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.oak.s3connector/](https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.oak.s3connector/) <!-- Alexandru: this is a stub link for now -->

1. Extrahera S3-kopplingen (version 1.60.2) och kopiera innehållet i följande mappar under `crx-quickstart/install` enligt följande:

   1. Kopiera `com.adobe.granite.oak.s3connector-1.60.2/jcr_root/libs/system/install/1` under `crx-quickstart/install/1`
   1. Kopiera `com.adobe.granite.oak.s3connector-1.60.2/jcr_root/libs/system/install/15` under `crx-quickstart/install/15`

Starta nu AEM-instansen med det nya kommandot som bestäms med hjälp av informationen under avsnittet [Kontrollera rätt startkommando för uppgradering](#determining-the-correct-upgrade-start-command).

### Kontrollera rätt startkommando för uppgradering {#determining-the-correct-upgrade-start-command}

>[!NOTE]
>
>Stöd för vissa av Java 8/11-argumenten har tagits bort i Java 17/21, se [Oracle Java™ 17-dokument](https://docs.oracle.com/en/java/javase/17/docs/specs/man/java.html), [Oracle Java™ 21-dokument](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html) och [JavaTrade-argument för AEM 6.5 LTS](/help/sites-deploying/custom-standalone-install.md#java-17-considerations-java-considerations).

För att kunna genomföra uppgraderingen är det viktigt att du startar AEM med filen jar för att ta fram instansen.

Observera att uppgraderingen inte startar när AEM startas från startskriptet. De flesta kunder börjar använda AEM med startskriptet och har anpassat det här startskriptet för att inkludera växlar för miljökonfigurationer som minnesinställningar, säkerhetscertifikat osv. Därför rekommenderar Adobe att du följer den här proceduren för att fastställa rätt uppgraderingskommando:

1. Kör följande från kommandoraden på en AEM-instans som körs:

   ```shell
   ps -ef | grep java
   ```

1. Leta efter AEM-processen. Det ser ut ungefär så här:

   ```shell
   /usr/bin/java -server -Xmx1024m -Djava.awt.headless=true -Dsling.run.modes=author,crx3,crx3tar -jar crx-quickstart/app/cq-quickstart-6.5.0-standalone-quickstart.jar start -c crx-quickstart -i launchpad -p 4502 -Dsling.properties=conf/sling.properties
   ```

1. Ändra kommandot genom att ersätta sökvägen till den befintliga burken ( `crx-quickstart/app/aem-quickstart*.jar` i det här fallet) med den nya AEM 6.5 LTS-burken som är jämställd med mappen `crx-quickstart`. Om du använder vårt tidigare kommando som exempel blir vårt kommando:

   ```shell
   /usr/bin/java -server -Xmx4096m -Djava.awt.headless=true -Dsling.run.modes=author,crx3,crx3tar -jar <AEM-6.5-LTS.jar> -c crx-quickstart -p 4502 -Dsling.properties=conf/sling.properties
   ```

   Detta säkerställer att alla korrekta minnesinställningar, anpassade körningslägen och andra miljöparametrar används för uppgraderingen. När uppgraderingen har slutförts kan instansen startas från startskriptet vid framtida starter.

## Distribuera uppgraderad kodbas {#deploy-upgraded-codebase}

När uppgraderingsprocessen på plats har slutförts ska den uppdaterade kodbasen distribueras. Steg för att uppdatera kodbasen så att den fungerar i målversionen av AEM finns på sidan [Uppgraderingskod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md).

## Genomför efteruppgraderingskontroller och felsökning {#perform-post-upgrade-check-troubleshooting}

Se [Efterbelys uppgraderingskontroller och felsökning](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md).
