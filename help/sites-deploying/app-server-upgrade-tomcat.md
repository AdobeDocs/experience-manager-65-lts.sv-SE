---
title: Uppgradera steg för programserverinstallationer (Tomcat)
description: Lär dig hur du uppgraderar instanser av AEM som distribueras via Tomcat.
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 7f8de16f-9e9a-4d37-9978-d26c496b911c
source-git-commit: 2a33cb4b8aa1dcfd989cf61465492d563f9cd99a
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 0%

---

# Uppgradera steg för programserverinstallationer (Tomcat - Sidegrade) {#upgrade-steps-for-application-server-installations-tomcat}

>[!NOTE]
>
>På den här sidan beskrivs uppgraderingsproceduren från AEM 6.5 till AEM 6.5 LTS på Tomcat. För uppgradering från AEM 6.5 LTS till AEM 6.5 LTS Service Pack [se denna](/help/sites-deploying/app-server-upgrade-tomcat-inplace.md)

## Steg före uppgradering {#pre-upgrade-steps}

Innan du utför uppgraderingen måste du utföra flera steg. Mer information finns i [Uppgradera kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md) och [Underhållsaktiviteter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md). Kontrollera dessutom att datorn uppfyller [kraven för AEM 6.5 LTS](/help/sites-deploying/technical-requirements.md) och se [planeringshänsyn](/help/sites-deploying/upgrade-planning.md) samt hur [Analyzer](/help/sites-deploying/aem-analyzer.md) kan hjälpa dig att beräkna komplexiteten.


### Krav för migrering {#migration-prerequisites}

* **Minimikrav på Java-version**: Kontrollera att du har installerat Oracle® JRE 17/21 på Tomcat-servern.
* **Tomcat-server**: De versioner av Tomcat-servern som stöds för AEM 6.5 LTS är **10.0.x** och **10.1.x**.

### Utföra uppgraderingen {#performing-the-upgrade}

I alla exemplen i den här proceduren används Tomcat som Application Server och du antyder att du har en fungerande version av AEM som redan är distribuerad. Proceduren är avsedd att dokumentera uppgraderingar som utförts från AEM version **6.5** till **6.5 LTS**.

1. Om AEM 6.5 redan är distribuerat kontrollerar du att paketen fungerar korrekt genom att gå till: *`https://<serveraddress:port>/system/console/bundles`*
1. Stoppa sedan AEM 6.5. Detta kan du göra från Tomcat App Manager på: *`https://<serveraddress:port>/manager/html`*
1. Kontrollera att du har slutfört [föruppgraderingsaktiviteterna](#pre-upgrade-steps), som säkerhetskopiering av AEM 6.5-servern, innan du utför någon uppgraderingsaktivitet
1. Installera Java 17/Java 21 och kontrollera att det är korrekt installerat genom att köra kommandot:

   ```
   java –version
   ```

1. Konfigurera en AEM 6.5 LTS-kompatibel Tomcat-server
1. Granska startparametrarna för AEM-servern och se till att uppdatera parametrarna enligt systemkraven. Mer information finns i [Java 17/Java 21 Considerations](/help/sites-deploying/custom-standalone-install.md#java-considerations)
1. Installera det nya 6.5 LTS-kriget på Tomcat-servern med Java 17/Java 21 och starta AEM 6.5 LTS Tomcat-servern genom att köra:

   ```
   $CATALINA_HOME/bin/catalina.sh start
   ```

1. När AEM är igång kontrollerar du att alla paket är i körläge genom att gå till: *`https://<serveraddress:port>/cq/system/console/bundles`*
1. Stoppa AEM 6.5 LTS Tomcat-servern. I de flesta fall kan du göra detta genom att köra skriptet `./catalina.sh` genom att köra det här kommandot från terminalen:

   ```
   $CATALINA_HOME/bin/catalina.sh stop
   ```

1. Migrera nu ditt innehåll från AEM 6.5 till AEM 6.5 LTS genom att följa stegen här: [AEM 6.5 till AEM 6.5 LTS Content Migration Using Oak-upgrade](/help/sites-deploying/aem-65-to-aem-65lts-content-migration-using-oak-upgrade.md)
1. När innehållet har migrerats använder du eventuella anpassade ändringar som krävs i filen `sling.properties`
1. Starta AEM 6.5 LTS Tomcat-servern genom att köra:

   ```
   $CATALINA_HOME/bin/catalina.sh start
   ```

1. Övervaka felloggarna medan AEM startar för att kontrollera att det inte finns några fel och att AEM körs utan problem
1. När AEM 6.5 LTS har startats kontrollerar du att paketen fungerar korrekt genom att gå till: *`https://<serveraddress:port>/cq/system/console/bundles`*

## Distribuera uppgraderad kodbas {#deploy-upgraded-codebase}

När uppgraderingen är klar ska den uppdaterade kodbasen distribueras. Steg för att uppdatera kodbasen så att den fungerar i målversionen av AEM finns på sidan [Uppgraderingskod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md).

## Genomför kontroller och felsökning efter uppgraderingen {#perform-post-upgrade-checks-and-troubleshooting}

Mer information finns i [Efter uppgraderingskontroller och felsökning](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md).
