---
title: Uppgradera steg för programserverinstallationer
description: Lär dig hur du uppgraderar instanser av AEM som distribueras via programservrar.
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 28701105452c347c5470fdb582d783e7aef1adb0
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Uppgradera steg för programserverinstallationer {#upgrade-steps-for-application-server-installations}

>[!NOTE]
>
>På den här sidan beskrivs uppgraderingsproceduren för AEM 6.5 LTS war on WLP (WebSphere Liberty).

## Steg före uppgradering {#pre-upgrade-steps}

Innan du utför uppgraderingen måste du utföra flera steg. Mer information finns i [Uppgradera kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md) och [Underhållsaktiviteter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md). Kontrollera dessutom att datorn uppfyller kraven för AEM 6.5 LTS. Se hur Analyzer kan hjälpa dig att uppskatta hur komplicerad din uppgradering är och se även en plan för uppgraderingen (mer information finns i [Planera din uppgradering](/help/sites-deploying/upgrade-planning.md)).

### Krav för migrering {#migration-prerequisites}

* **Minimikrav för Java-version**: Kontrollera att du har installerat IBM Sumeru JRE 17 på WLP-servern.

### Utföra uppgraderingen {#performing-the-upgrade}

1. Säkerhetskopiera instansen innan du påbörjar en uppgradering.
1. Identifiera om du behöver en uppgradering eller uppgradering på plats beroende på vilken version av WLP-servern du använder. Om din nuvarande WLP-server har stöd för Servlet 6 kan du utföra en uppgradering på plats och fortsätta med den här dokumentationen. Annars måste du utföra en separat uppgradering. Om du vill uppgradera följer du länken för innehållsmigrering med Oak-uppgraderingsdokumentation - [TBD som ska läggas till]
1. Stoppa AEM-instansen. Det kan du vanligtvis göra med det här kommandot:

   ```shell
   <path-to-wlp-directory>/bin/server stop server_name
   ```

1. Ta bort filer och mappar som inte längre behövs. Objekten som du behöver ta bort specifikt är:

   * `cq-quickstart-65.war` från mappen `dropins` och den expanderade mappen som vanligtvis finns på `<path-to-aem-server>/dropins/cq-quickstart-65.war` respektive `<path-to-aem-server>/apps/expanded/cq-quickstart-65.war`
   * Mappen `launchpad/startup`. Du kan ta bort den genom att köra följande kommando i terminalen, förutsatt att du är i servermappen:

     ```shell
     rm -rf crx-quickstart/launchpad/startup
     ```

   * Filen `base.jar`. Du kan göra detta genom att köra följande kommandon:

     ```shell
     find crx-quickstart/launchpad -type f -name 
     "org.apache.sling.launchpad.base.jar*" -exec rm -f {} \;
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
1. Granska startparametrarna för AEM-servern och se till att du uppdaterar parametrarna enligt systemkraven. Mer information finns i [Anpassad fristående installation](/help/sites-deploying/custom-standalone-install.md)
1. Installera Java 17 och kontrollera att det är korrekt installerat genom att köra:

   ```shell
   java -version
   ```

1. Hämta det nya AR 6.5 från Software Distribution och kopiera det till mappen dropins som finns på: `/<path-to-aem-server>/dropins/`
1. Starta AEM-instansen: Det går oftast att göra med det här kommandot:

   ```shell
   <path-to-wlp-directory>/bin/server start server_name
   ```

1. Om du har egna ändringar i `sling.properties`, följ instruktionerna nedan:

   1. Stoppa AEM-instansen genom att köra `<path-to-wlp-directory>/bin/server stop server_name`
   1. Använd dina anpassade `sling.properties`-ändringar på den nyligen genererade `sling.properties`-filen (genom att referera till den säkerhetskopiefil som skapades i steg 6)
   1. Starta AEM-instansen. Det kan vanligtvis göras genom att köra: `<path-to-wlp-directory>/bin/server start server_name`

## Distribuera uppgraderad kodbas {#deploy-upgraded-codebase}

När uppgraderingsprocessen på plats har slutförts ska den uppdaterade kodbasen distribueras. Steg för att uppdatera kodbasen så att den fungerar i målversionen av AEM finns på sidan [Uppgraderingskod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md).

## Genomför kontroller och felsökning efter uppgraderingen {#perform-post-upgrade-checks-and-troubleshooting}

Mer information finns i [Efter uppgraderingskontroller och felsökning](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md).