---
title: Uppgradera steg för programserverinstallationer (WLP)
description: Lär dig hur du uppgraderar instanser av AEM som distribueras via Webspehere Liberty.
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: cd04a7a493a575cabf416b53869dd3fe4df7ab6b
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 0%

---

# Uppgradera steg för programserverinstallationer (WLP) {#upgrade-steps-for-application-server-installations-wlp}

>[!NOTE]
>
>På den här sidan beskrivs uppgraderingsproceduren för AEM 6.5 LTS on WLP (WebSphere® Liberty).

## Steg före uppgradering {#pre-upgrade-steps}

Innan du utför uppgraderingen måste du utföra flera steg. Mer information finns i [Uppgradera kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md) och [Underhållsaktiviteter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md). Kontrollera dessutom att datorn uppfyller [kraven för AEM 6.5 LTS](/help/sites-deploying/technical-requirements.md).

Kontrollera [Planera uppgraderingen](/help/sites-deploying/upgrade-planning.md) och hur [AEM Analyzer](/help/sites-deploying/pattern-detector.md) kan hjälpa dig att beräkna komplexiteten i uppgraderingen av AEM.

### Krav för migrering {#migration-prerequisites}

* **Minimikrav på Java-version**: Kontrollera att du har installerat IBM® Sumeru JRE 17 på WLP-servern.

### Utföra uppgraderingen {#performing-the-upgrade}

1. Se till att du har slutfört [föruppgraderingen](#pre-upgrade-steps) som att säkerhetskopiera AEM 6.5-servern innan du utför någon uppgraderingsaktivitet
1. Välj något av följande alternativ beroende på dina behov:
   1. **Lokal uppgradering**: Om din nuvarande WLP-server stöder Servlet 6 kan du utföra en uppgradering på plats och fortsätta med steg 3.
   1. **Sidegrade**: Om du föredrar en ny installation eller om din WLP-server inte stöder Servlet 6, skapar du en ny WLP-instans med AEM 6.5 LTS och migrerar innehållet genom att följa guiden [AEM 6.5 till AEM 6.5 LTS Content Migration Using Oak-upgrade](/help/sites-deploying/aem-65-to-aem-65lts-content-migration-using-oak-upgrade.md) och hoppa till avsnittet [Distribuera uppgraderad kodbas](#deploy-upgraded-codebase)

1. Stoppa AEM-instansen. Det kan du vanligtvis göra med det här kommandot:

   ```shell
   <path-to-wlp-directory>/bin/server stop server_name
   ```

1. Ta bort filer och mappar som inte längre behövs. Objekten som du behöver ta bort specifikt är:

   * **cq-quickstart-65.war** från mappen `dropins` och mappen `expanded` som vanligtvis finns på `<path-to-aem-server>/dropins/cq-quickstart-65.war` respektive `<path-to-aem-server>/apps/expanded/cq-quickstart-65.war`
   * Mappen `launchpad/startup`. Du kan ta bort den genom att köra följande kommando i terminalen, förutsatt att du är i servermappen:

     ```shell
     rm -rf crx-quickstart/launchpad/startup
     ```

   * Filen `base.jar`. Du kan göra detta genom att köra följande kommandon:

     ```shell
     find crx-quickstart/launchpad -type f -name "org.apache.sling.launchpad.base.jar*" -exec rm -f {} \;
     ```

   * Filen `BootstrapCommandFile_timestamp.txt`:

     ```shell
     rm -f crx-quickstart/launchpad/felix/bundle0/BootstrapCommandFile_timestamp.txt
     ```

   * Ta bort filen `sling.options` genom att köra:

     ```shell
     find crx-quickstart/launchpad -type f -name "sling.options.file" -exec rm -rf {} \; 
     ```

   * Ta bort filen `sling.bootstrap.txt`:

     ```shell
     rm -rf crx-quickstart/launchpad/sling_bootstrap.txt
     ```

1. Gör en säkerhetskopia av filen `sling.properties` (finns vanligtvis i `crx-quickstart/conf/`) och ta bort den
1. Ändra serverletsversionen till **6.0** i filen `server.xml`
1. Installera Java 17 och kontrollera att det är korrekt installerat genom att köra:

   ```shell
   java -version
   ```

1. Granska startparametrarna för AEM-servern och se till att du uppdaterar parametrarna enligt dina krav. Mer information finns i [Java 17 Considerations](/help/sites-deploying/custom-standalone-install.md#java-17-considerations-java-considerations)
1. Hämta det nya 6.5 LTS-kriget och kopiera det till dropins-mappen på: `/<path-to-aem-server>/dropins/`
1. Starta AEM-instansen: Det går oftast att göra med det här kommandot:

   ```shell
   <path-to-wlp-directory>/bin/server start server_name
   ```

1. Om du har egna ändringar i `sling.properties`, följ instruktionerna nedan:

   1. Stoppa AEM-instansen genom att köra `<path-to-wlp-directory>/bin/server stop server_name`
   1. Använd dina anpassade `sling.properties`-ändringar på den nyligen genererade `sling.properties`-filen (genom att referera till den säkerhetskopiefil som skapades i steg 5)
   1. Starta AEM-instansen. Det kan vanligtvis göras genom att köra: `<path-to-wlp-directory>/bin/server start server_name`

## Distribuera uppgraderad kodbas {#deploy-upgraded-codebase}

När uppgraderingen är klar ska den uppdaterade kodbasen distribueras. Steg för att uppdatera kodbasen så att den fungerar i målversionen av AEM finns på sidan [Uppgraderingskod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md).

## Genomför kontroller och felsökning efter uppgraderingen {#perform-post-upgrade-checks-and-troubleshooting}

Se [Efterbelys uppgraderingskontroller och felsökning](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md).
