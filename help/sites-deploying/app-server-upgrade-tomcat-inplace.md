---
title: Uppgradera steg för programserverinstallationer (Tomcat)
description: Lär dig hur du uppgraderar instanser av AEM som distribueras via Tomcat.
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: b3c4e946a3f235fa0e3a0945f1ad692ee195e3ef
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Uppgraderingssteg för programserverinstallationer (Tomcat - Inplace upgrade) {#upgrade-steps-for-application-server-installations-tomcat-inplace}

>[!NOTE]
>
>På den här sidan beskrivs uppgraderingsproceduren (uppgradering) från AEM 6.5 LTS till AEM 6.5 LTS Service epack på Tomcat. Om du uppgraderar från AEM 6.5 till 6.5 LTS, [se här](/help/sites-deploying/app-server-upgrade-tomcat.md).

## Steg före uppgradering {#pre-upgrade-steps}

Innan du utför uppgraderingen måste du utföra flera steg. Mer information finns i [Underhållsaktiviteter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md). Kontrollera dessutom att datorn uppfyller [kraven för AEM 6.5 LTS Service Pack](/help/sites-deploying/technical-requirements.md) och se [planeringskontakter för uppgradering](/help/sites-deploying/upgrade-planning.md).


### Krav för migrering {#migration-prerequisites}

* **Minimikrav på Java-version**: Kontrollera att du har installerat Oracle® JRE 17/21 på Tomcat-servern.
* **Tomcat-server**: De versioner av Tomcat-servern som stöds för AEM 6.5 LTS och ServicePacks är **10.0.x** och **10.1.x**.

### Utföra uppgraderingen {#performing-the-upgrade}

I alla exemplen i den här proceduren används Tomcat som Application Server och du antyder att du redan har en fungerande version av AEM 6.5 LTS distribuerad. Proceduren är avsedd att dokumentera uppgraderingar som utförts från AEM version **6.5** LTS till **6.5 LTS** Service Pack.

1. Om AEM 6.5 LTS redan har distribuerats kontrollerar du att paketen fungerar korrekt genom att gå till: *`https://<serveraddress:port>/system/console/bundles`*
1. Stoppa sedan AEM 6.5 LTS. Detta kan du göra från Tomcat App Manager på: *`https://<serveraddress:port>/manager/html`*
1. Kontrollera att du har slutfört [föruppgraderingsaktiviteterna](#pre-upgrade-steps), som säkerhetskopiering av AEM 6.5 LTS-servern, innan du utför någon uppgraderingsaktivitet
1. Stoppa AEM 6.5 LTS Tomcat-servern. I de flesta fall kan du göra detta genom att köra skriptet `./catalina.sh` genom att köra det här kommandot från terminalen:

   ```
   $CATALINA_HOME/bin/catalina.sh stop
   ```

1. Ta bort filer och mappar som inte längre behövs. Objekten som du behöver ta bort specifikt är:

   * Filen **cq-quickstart-65.war** och mappen `cq-quickstart-65` från mappen `webapps` finns vanligtvis på `<path-to-aem-server>/webapps`
   * Mappen `launchpad/startup`. Du kan ta bort den genom att köra följande kommando i terminalen, förutsatt att du är i servermappen:

     ```shell
     rm -rf <path-to-aem-server>/bin/crx-quickstart/launchpad/startup
     ```

   * Filen `base.jar`. Du kan göra detta genom att köra följande kommandon:

     ```shell
     find <path-to-aem-server>/bin/crx-quickstart/launchpad -type f -name "org.apache.sling.launchpad.base.jar*" -exec rm -f {} \;
     ```

   * Filen `BootstrapCommandFile_timestamp.txt`:

     ```shell
     rm -f <path-to-aem-server>/bin/crx-quickstart/launchpad/felix/bundle0/BootstrapCommandFile_timestamp.txt
     ```

   * Ta bort filen `sling.options` genom att köra:

     ```shell
     find <path-to-aem-server>/bin/crx-quickstart/launchpad -type f -name "sling.options.file" -exec rm -rf {} \; 
     ```

   * Ta bort filen `sling.bootstrap.txt`:

     ```shell
     rm -rf <path-to-aem-server>/bin/crx-quickstart/launchpad/sling_bootstrap.txt
     ```

1. Gör en säkerhetskopia av filen `sling.properties` (finns vanligtvis i `<path-to-aem-server>/bin/crx-quickstart/launchpad/`) och ta bort den
1. Kopiera AEM 6.5 LTS Services Pack-filen till mappen `<path-to-aem-server>/webapps`
1. Starta AEM 6.5 LTS Tomcat-servern genom att köra:

   ```
   $CATALINA_HOME/bin/catalina.sh start
   ```

1. Övervaka felloggarna medan AEM startar för att kontrollera att det inte finns några fel och att AEM körs utan problem
1. När AEM 6.5 LTS har startats kontrollerar du att paketen fungerar korrekt genom att gå till: *`https://<serveraddress:port>/cq/system/console/bundles`*

## Genomför kontroller och felsökning efter uppgraderingen {#perform-post-upgrade-checks-and-troubleshooting}

Mer information finns i [Efter uppgraderingskontroller och felsökning](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md).
