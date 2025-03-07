---
title: Migrera AEM Forms-resurser och -dokument
description: Med migreringsverktyget kan du migrera Adobe Experience Manager (AEM) Forms-resurser och -dokument från AEM 6.5.22.0 Forms till AEM 6.5 Forms LTS.
content-type: reference
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: 636f7b61-549e-45c7-ab21-94bb90db2b22
source-git-commit: 060bb23d64a90f0b2da487ead4c672cbf471c9a8
workflow-type: tm+mt
source-wordcount: '1698'
ht-degree: 0%

---

# Migrera AEM Forms-resurser och -dokument{#migrate-aem-forms-assets-and-documents}

Migreringsverktyget konverterar [adaptiva Forms-resurser](../../forms/using/introduction-forms-authoring.md), [molnkonfigurationer](/help/sites-developing/extending-cloud-config.md) och [Correspondence Management-resurser](/help/forms/using/cm-overview.md) från det format som används i tidigare versioner till det format som används i Adobe Experience Manager (AEM) 6.5 LTS Forms. När du kör migreringsverktyget migreras följande:

* Anpassade komponenter för adaptiva formulär
* Adaptiva formulär och Correspondence Management-mallar
* Molnkonfigurationer
* Korrespondenshantering och adaptiva Forms-resurser

>[!NOTE]
>
>Om det finns en uppgradering som inte är på plats för Correspondence Management-resurser kan du köra migreringen varje gång du importerar resurserna. För migrering av Correspondence Management måste Forms-kompatibilitetspaketet vara installerat.

## Migreringsmetod {#approach-to-migration}

Du kan [uppgradera](../../forms/using/upgrade.md) till [AEM Forms 6.5 LTS från AEM Forms 6.5.22.0](/help/forms/using/upgrade-forms-osgi.md). Beroende på om du har uppgraderat din tidigare installation eller utfört en ny installation måste du göra något av följande:

**Om det finns en uppgradering på plats**

Om du utförde en [på plats-uppgradering](/help/sites-deploying/in-place-upgrade.md) har den uppgraderade instansen redan resurserna och dokumenten. Innan du kan använda resurserna och dokumenten måste du installera [AEMFD-kompatibilitetspaketet](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en) (inkluderar kompatibilitetspaketet för hantering av korrespondenshantering).

Därefter måste du uppdatera resurserna och dokumenten genom att [köra migreringsverktyget](#runningmigrationutility).

**Om det finns en installation på annan plats**

Om det är en oavslutad (ny) installation måste du installera [AEMFD-kompatibilitetspaketet](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en) (inkluderar Correspondence Management Compatibility-paketet) innan du kan använda resurserna och dokumenten.

Därefter måste du importera resurspaketet (zip eller cmp) till den nya konfigurationen och sedan uppdatera resurserna och dokumenten genom att [köra migreringsverktyget](#runningmigrationutility). Adobe rekommenderar att du bara skapar resurser på den nya installationen efter att du har kört migreringsverktyget.

På grund av [bakåtkompatibilitetsrelaterade](/help/sites-deploying/backward-compatibility.md) ändringar har platserna för några mappar i crx-databasen ändrats. Exportera och importera beroenden manuellt (anpassade bibliotek och resurser) från tidigare inställningar till en ny miljö.

## Innan du fortsätter med migreringen {#prerequisites}

För Correspondence Management-resurser:

* För resurserna som importeras från den tidigare plattformen läggs en egenskap till: **fd:version=1.0**.
* Eftersom AEM 6.1 Forms inte går att kommentera direkt i dokumentet. Kommentarerna som lades till tidigare är tillgängliga i resurserna, men visas inte automatiskt i gränssnittet. Anpassa egenskapen extendedProperties i AEM Forms användargränssnitt för att göra kommentarerna synliga.
* I vissa av de tidigare versionerna, till exempel LiveCycle ES4, redigerades text med Flex RichTextEditor, men sedan AEM 6.1 Forms används HTML Editor. På grund av den här återgivningen och utseendet på teckensnitten kan teckenstorlekar och marginaler skilja sig från de tidigare versionerna i användargränssnittet för Författare. Bokstäverna ser dock likadana ut när de återges.
* Listor i textmoduler har förbättrats och återges nu annorlunda. Det kan finnas skillnader i synen. Adobe rekommenderar att du återger och ser bokstäverna där du använder listor i textmoduler.
* Eftersom bildinnehållsmoduler konverteras till DAM-resurser och layouter och fragment läggs till i formulär under migreringen ändras egenskapen Uppdaterat av för dessa moduler till admin.
* Resursernas versionshistorik migreras inte och är inte tillgänglig efter migreringen. Den efterföljande versionshistoriken efter migreringen bevaras.
* Läget Klar att publicera är föråldrat sedan AEM 6.1 Forms, så alla resurser i läget Klar att publicera ändras till läget Ändrat.
* Eftersom användargränssnittet uppdateras i AEM Forms 6.3 skiljer sig även stegen för att utföra anpassningarna. Gör om anpassningen om du migrerar från en version före 6.3.
* Layoutfragment flyttas från `/content/apps/cm/layouts/fragmentlayouts/1001` till `/content/apps/cm/modules/fragmentlayouts`. Referens till datamordlista i resurser visar sökvägen till datamappningen i stället för dess namn.
* Alla tabbavstånd som används för justering i textmoduler måste justeras om. <!--For more information, see [Correspondence Management - Using tab spacing for arranging text](https://helpx.adobe.com/aem-forms/kb/cm-tab-spacing-limitations.html)-->.
* Resurshanterarkonfigurationer ändras till Correspondence Management-konfigurationer.
* Assets flyttas under mappar med namn som Befintlig text och Befintlig lista.

## Använda migreringsverktyget {#using-the-migration-utility}

### Kör migreringsverktyget {#runningmigrationutility}

Kör migreringsverktyget innan du ändrar i resurserna eller skapar resurser. Adobe rekommenderar att du inte kör verktyget efter att du har gjort några ändringar eller skapat resurser. Kontrollera att Correspondence Management eller Adaptive Forms Assets inte är öppet när migreringsprocessen körs.

När du kör migreringsverktyget för första gången skapas en logg med följande sökväg och namn: `\[aem-installation-directory]\cq-quickstart\logs\aem-forms-migration.log`. Loggen uppdateras kontinuerligt med information om Correspondence Management och adaptive Forms-migrering, som att flytta resurser.

>[!NOTE]
>
>Innan du kör migreringsverktyget måste du se till att du har säkerhetskopierat din crx-databas.

1. I en webbläsarsession loggar du in på din AEM Author-instans som administratör.

1. Öppna följande URL i webbläsaren:

   https://[*värdnamn*]:[*port*]/[*context_path*]/libs/fd/foundation/gui/content/migration.html

   Webbläsaren visar fyra alternativ:

   * AEM Forms Assets Migration
   * Adaptiv migrering av anpassade Forms-komponenter
   * Migrering av adaptiva Forms-mallar
   * Migrering av konfigurationer i AEM Forms Cloud

1. Gör följande för att utföra migreringen:

   * Om du vill migrera **resurser** väljer du AEM Forms Assets-migrering och väljer **Starta migrering** på nästa skärm. Följande migreras:

      * Anpassningsbara formulär
      * Dokumentfragment
      * Teman
      * Bokstäver
      * Dataordlistor

   >[!NOTE]
   >
   >Vid resursmigrering kan du hitta varningsmeddelanden som &quot;Konflikt hittad för...&quot;. Sådana meddelanden visar att reglerna för vissa komponenter i anpassade formulär inte kan migreras. Om komponenten till exempel har en händelse som har både regler och skript, kommer inga regler för komponenten att migreras om regler inträffar efter skript. Du kan [migrera sådana regler genom att öppna regelredigeraren](#migrate-rules) i redigeringsprogram för anpassningsbara formulär.

   * Om du vill migrera anpassade formulärkomponenter väljer du **Adaptiv migrering av anpassade Forms-komponenter** och väljer **Starta migrering** på sidan för migrering av anpassade komponenter. Följande migreras:

      * Anpassade komponenter skrivna för Adaptive Forms
      * Komponentövertäckningar, om sådana finns.

   * Om du vill migrera adaptiva formulärmallar väljer du **Adaptiv migrering av Forms-mallar** och väljer **Starta migrering** på sidan migrering av anpassade komponenter. Följande migreras:

      * Anpassningsbara formulärmallar som skapats under `/apps` eller `/conf` med AEM mallredigerare.

   * Migrera konfigurationstjänsterna i AEM Forms Cloud till att använda det nya kontextmedvetna molntjänstparadigmet, som innehåller det beröringsaktiverade användargränssnittet (under `/conf`). När du migrerar konfigurationstjänsterna i AEM Forms Cloud flyttas molntjänsterna i `/etc` till `/conf`. Om du inte har några anpassningar av molntjänster som är beroende av de äldre sökvägarna (`/etc`) rekommenderar Adobe att du kör migreringsverktyget efter uppgradering till 6.5. Använd molnkonfigurationsgränssnittet för ytterligare arbete. Om du har befintliga anpassningar av molntjänster kan du fortsätta använda det klassiska användargränssnittet i den uppgraderade konfigurationen tills anpassningarna har uppdaterats för att passa ihop med de migrerade sökvägarna (`/conf`) och sedan köra migreringsverktyget.

   Om du vill migrera **AEM Forms molntjänster**, som innehåller följande, väljer du AEM Forms Cloud Configuration Migration (migreringen av molnkonfigurationen är oberoende av AEMFD-kompatibilitetspaketet). Välj migrering av AEM Forms Cloud-konfigurationer och välj sedan **Starta migrering** på sidan Konfigurationsmigrering:

   * Molntjänster för formulärdatamodell

      * Source-sökväg: `/etc/cloudservices/fdm`
      * Målsökväg: `/conf/global/settings/cloudconfigs/fdm`

   * Recaptcha

      * Source-sökväg: `/etc/cloudservices/recaptcha`
      * Målsökväg: `/conf/global/settings/cloudconfigs/recaptcha`

   * Adobe Sign

      * Source-sökväg: `/etc/cloudservices/echosign`
      * Målsökväg: `/conf/global/settings/cloudconfigs/echosign`

   * Typekit cloud services

      * Source-sökväg: `/etc/cloudservices/typekit`
      * Målsökväg: `/conf/global/settings/cloudconfigs/typekit`

   I webbläsarfönstret visas följande när migreringsprocessen utförs:

   * När resurserna uppdateras: Assets uppdateras.
   * När migreringen är klar: Slutförd migrering av resurser.

   När migreringsverktyget körs gör det följande:

   * **Lägger till taggarna i resurserna**: Lägger till taggen &quot;Correspondence Management : Migrated Assets&quot; / &quot;Adaptive Forms : Migrated Assets&quot;. till de migrerade resurserna, så att användarna kan identifiera migrerade resurser. När du kör migreringsverktyget markeras alla befintliga resurser i systemet som migrerade.
   * **Genererar taggar**: Kategorier och underkategorier som finns i det tidigare systemet skapas som taggar, och sedan associeras dessa taggar med de relevanta Correspondence Management-resurserna i AEM. Exempelvis genereras en kategori (anspråk) och en underkategori (anspråk) för en bokstavsmall som taggar.

1. När migreringsverktyget har körts klart fortsätter du till [hushållningsaktiviteter](#housekeepingtasks).

#### Migrera regler med regelredigeraren {#migrate-rules}

Dessa komponenter kan migreras genom att de öppnas i regelredigeraren i den adaptiva Forms-redigeraren.

* Om du vill migrera regler och skript (krävs inte om du uppgraderar från 6.3) i anpassade komponenter väljer du Anpassad migrering för Forms och väljer Starta migrering på nästa skärm. Följande migreras:

   * Regler och skript skapade med regelredigeraren (6.1 FP1 och senare)

   * Skript som skapats med fliken Skript i användargränssnittet i 6.1 och tidigare versioner

* Om du vill migrera mallar (krävs inte om du uppgraderar från 6.3 och 6.4) väljer du Adaptiv migrering av Forms-mallar och väljer Starta migrering på nästa skärm. Följande migreras:

   * Gamla mallar - de adaptiva formulärmallarna som skapats under /apps med AEM 6.1 Forms eller tidigare. Detta inkluderar de skript som definierades i mallkomponenterna.

   * Nya mallar - Anpassningsbara formulärmallar som skapats med mallredigeraren under `/conf`. Detta inkluderar migrering av regler och skript som skapats med regelredigeraren.

### Hushållsuppgifter efter att migreringsverktyget har körts {#housekeepingtasks}

När du har kört migreringsverktyget ska du sköta följande uppgifter:

1. Kontrollera att XFA-versionen av layouter och fragmentlayouter är 3.3 eller senare. Om du använder layouter och fragmentlayouter av en äldre version kan det uppstå problem när bokstaven återges. Så här uppdaterar du en version av en äldre XFA till den senaste versionen:

   1. [Hämta XFA som en zip-fil](../../forms/using/import-export-forms-templates.md#p-import-and-export-assets-in-correspondence-management-p) från Forms användargränssnitt.
   1. Hämta filen.
   1. Öppna XFA-filen i den senaste Designer-filen och spara den. Versionen av XFA uppdateras till den senaste.
   1. Överför XFA i Forms användargränssnitt.

1. Publicera alla resurser som publicerades i det tidigare systemet före migreringen. Flyttningsverktyget uppdaterar bara resurserna på Author-instansen och för att uppdatera resurserna på Publish-instanserna måste du publicera resurserna.

1. I AEM Forms 6.4 och 6.5 ändras vissa rättigheter för användargrupperna. Om du vill att någon av dina användare ska kunna överföra XDP-filer och adaptiva Forms-skript eller använda en kodredigerare måste du lägga till dem i gruppen för användare med avancerade formulär. Mallförfattare kan inte heller längre använda kodredigeraren i regelredigeraren. För att användare ska kunna använda en kodredigerare lägger du till dem i gruppen af-template-script-writers.
