---
title: Säkerhetskopiera Adobe Experience Manager Forms-data
description: I det här dokumentet beskrivs de steg som krävs för att slutföra en aktiverad, eller online, säkerhetskopiering av Adobe Experience Manager (AEM) formulärdatabas, GDS- och Content Storage Root-kataloger.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: cbcb9301-48c9-4394-b8c0-766eed76101d
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1527'
ht-degree: 0%

---

# Säkerhetskopiera Adobe Experience Manager (AEM) Forms-data {#backing-up-the-aem-forms-data}

<!-- back up is two words when used as a verb; backup is one word when used as an adjective or noun. -->

I det här avsnittet beskrivs de steg som krävs för att slutföra en aktiverad, eller online, säkerhetskopiering av AEM Forms-databasen, GDS- och Content Storage Root-katalogerna.

När AEM Forms har installerats och driftsatts i produktionsområden bör databasadministratören göra en inledande fullständig, eller kall, säkerhetskopiering av databasen. Databasen måste stängas av för den här säkerhetskopieringen. Sedan bör differentiell eller inkrementell (eller het) säkerhetskopiering av databasen göras regelbundet.

För att säkerhetskopieringen och återställningen ska lyckas måste det alltid finnas en säkerhetskopia av systemavbildningen. Om en förlust inträffar kan du återställa hela miljön till ett konsekvent tillstånd.

Genom att säkerhetskopiera databasen samtidigt som säkerhetskopiorna av GDS-, AEM-databasen och Content Storage Root-katalogen synkroniseras dessa system om återställning skulle behövas.

Säkerhetskopieringsproceduren som beskrivs i det här avsnittet kräver att du aktiverar läget för säker säkerhetskopiering innan du säkerhetskopierar katalogerna AEM Forms-databas, AEM-databas, GDS och Content Storage Root. När säkerhetskopieringen är klar måste du avsluta läget för säker säkerhetskopiering. Säkert säkerhetskopieringsläge används för att markera långlivade och beständiga dokument som finns i GDS. I det här läget säkerställs att den automatiska filrensningsfunktionen (filinsamlaren) inte tar bort utgångna filer förrän det säkra säkerhetskopieringsläget släpps. En GDS-säkerhetskopiering måste synkroniseras med en databassäkerhetskopiering.

Hur ofta GDS-platsen måste säkerhetskopieras beror på hur AEM Forms används och vilka säkerhetskopieringsfönster som är tillgängliga. Säkerhetskopieringsfönstret kan påverkas av långvariga processer eftersom de kan köras i flera dagar. Om du kontinuerligt ändrar, lägger till och tar bort filer i den här katalogen bör du säkerhetskopiera GDS-platsen oftare.

Om databasen körs i ett loggningsläge, vilket beskrivs i föregående avsnitt, måste databasloggarna också säkerhetskopieras ofta så att de kan användas för att återställa databasen om ett mediefel uppstår.

>[!NOTE]
>
>Filer som inte refereras kan finnas kvar i GDS-katalogen efter återställningsprocessen. Detta är en känd begränsning för närvarande.

## Säkerhetskopiera katalogerna database, GDS, AEM database och Content Storage Root {#back-up-the-database-gds-aem-repository-and-content-storage-root-directories}

Placera AEM Forms i läget för säker säkerhetskopiering (ögonblicksbild) eller rullande säkerhetskopiering (kontinuerlig täckning). Innan du anger att AEM Forms ska ange något av säkerhetskopieringslägena bör du kontrollera följande:

* Verifiera systemversionen och registrera de korrigeringar eller uppdateringar som tillämpats sedan den senaste fullständiga säkerhetskopieringen av systemavbildningen.
* Om du använder säkerhetskopiering i rullnings- eller ögonblicksbildläge måste databasen vara konfigurerad med rätt logginställningar för att tillåta säkerhetskopiering av databasen. (Se [AEM Forms-databas](/help/forms/using/admin-help/files-back-recover.md#aem-forms-database).)

Förutom detta bör du följa följande riktlinjer för säkerhetskopiering/återställning.

* Säkerhetskopiera GDS-katalogen med hjälp av ett tillgängligt operativsystem eller ett verktyg för säkerhetskopiering från tredje part. (Se [GDS-plats](/help/forms/using/admin-help/files-back-recover.md#gds-location).)
* (Valfritt) Säkerhetskopiera rotkatalogen för innehållslagring med hjälp av ett tillgängligt operativsystem eller ett säkerhetskopierings- och verktyg från tredje part. (Se [Rotplats för innehållslagring (fristående miljö)](/help/forms/using/admin-help/files-back-recover.md#content-storage-root-location-stand-alone-environment) eller [Rotplats för innehållslagring (klustrad miljö)](/help/forms/using/admin-help/files-back-recover.md#content-storage-root-location-clustered-environment).)
* Säkerhetskopiera   skapa och publicera instanser (crx -database backup).

  Om du vill säkerhetskopiera Correspondence Management Solution-miljön utför du stegen på författaren och publicerar instanser enligt beskrivningen i [Säkerhetskopiera och återställ](/help/sites-administering/backup-and-restore.md).

  Tänk på följande när du säkerhetskopierar författare och publiceringsinstanser:

   * Se till att säkerhetskopieringen för författare och publiceringsinstanser är synkroniserad så att den startar samtidigt. Även om du kan fortsätta använda författare- och publiceringsinstanser medan säkerhetskopieringen utförs, bör du inte publicera någon resurs under säkerhetskopieringen för att undvika ohämtade ändringar. Vänta tills säkerhetskopieringen av både författare och publiceringsinstanser har avslutats innan du publicerar nya resurser.
   * Den fullständiga säkerhetskopian av Author-noden inkluderar säkerhetskopiering av data i Forms Manager och AEM Forms Workspace.
   * Workbench-utvecklare kan fortsätta att arbeta med sina processer lokalt. De bör inte driftsätta några nya processer under säkerhetskopieringsfasen.
   * Beslutet om längden på varje säkerhetskopieringssession (för rullande säkerhetskopieringsläge) bör baseras på den totala tid det tar att säkerhetskopiera alla data i AEM Forms (DB, GDS, AEM-databas och andra anpassade data).

Säkerhetskopiera AEM Forms-databasen, inklusive eventuella transaktionsloggar. Se [AEM Forms-databas](/help/forms/using/admin-help/files-back-recover.md#aem-forms-database).

Mer information finns i lämplig kunskapsbasartikel för din databas:
<!-- The four URLs below are all 404s; checked July 19, 2023 -->
* [Oracle Säkerhetskopiering och återställning för AEM Forms](https://www.adobe.com/go/kb403624)
* [MySQL-säkerhetskopiering och återställning för AEM Forms](https://www.adobe.com/go/kb403625)
* [Microsoft® SQL Server Backup and Recovery for AEM Forms](https://www.adobe.com/go/kb403623)
* [DB2® Säkerhetskopiering och återställning för AEM Forms](https://www.adobe.com/go/kb403626)

Dessa artiklar ger vägledning om grundläggande databasfunktioner för säkerhetskopiering och återställning av data. De är inte avsedda som tekniska handledningar för en viss leverantörs säkerhetskopierings- och återställningsfunktion för databaser. De innehåller kommandon som behövs för att skapa en tillförlitlig strategi för säkerhetskopiering av databaser för dina AEM Forms-programdata.

>[!NOTE]
>
>Säkerhetskopieringen av databasen måste vara slutförd innan du börjar säkerhetskopiera GDS. Om säkerhetskopieringen av databasen inte är slutförd är dina data inte synkroniserade.

### Ange lägen för säkerhetskopiering {#entering-the-backup-modes}

Du kan antingen använda administrationskonsolen, kommandot LCBackupMode eller det API som är tillgängligt i AEM Forms-installationen för att gå in i och lämna säkerhetskopieringslägena. För rullande säkerhetskopiering (kontinuerlig täckning) är administrationskonsolalternativet inte tillgängligt. Använd antingen kommandoradsalternativet eller API:t. <!-- Fix broken link For information about using the API to enter and leave backup modes, see AEM Forms API Reference on Help and Tutorials page. -->

>[!NOTE]
>
>Om du har konfigurerat SSL på Forms Server kan du inte placera Forms Server i säkerhetskopieringsläge med skriptet LCBackupMode.CMD.

**Använda administrationskonsolen för att växla till säkert säkerhetskopieringsläge**

1. Logga in på administrationskonsolen.
1. Klicka på Inställningar > Systeminställningar > Verktyg för säkerhetskopiering.
1. Välj Använd i säkert säkerhetskopieringsläge och klicka på OK.

   Med den här metoden försätts AEM Forms i oändligt säkerhetskopieringsläge (ingen timeout), och det försätts i ögonblicksbildsläge i stället för i rullande säkerhetskopieringsläge.

**Använda kommandoradsalternativet för att växla till säkert säkerhetskopieringsläge**

Du kan använda kommandoradsgränssnittet `LCBackupMode` för att ställa in AEM Forms i säkert säkerhetskopieringsläge.

1. Ange ADOBE_LIVECYCLE och starta programservern.
1. Gå till mappen `*[aem-forms root]*/sdk/misc/Foundation/BackupRestoreCommandline`.
1. Beroende på vilket operativsystem du använder kan du redigera skriptet `LCBackupMode.cmd` eller `LCBackupMode.sh` och ange standardvärden som passar ditt system.
1. Kör följande kommando på en rad i kommandotolken:

   * (Windows) `LCBackupMode.cmd enter [-Host=`*värdnamn* `] [-port=`*portnummer* `] [-user=`*användarnamn* `] [-password=`*lösenord* `] [-label=`*etikettnamn* `] [-timeout=`*sekunder* `]`
   * (Linux®, UNIX®) `LCBackupMode.sh enter [-host=`*värdnamn* `] [-port=`*portnummer* `] [-user=`*användarnamn* `] [-password=`*lösenord* `] [-label=`*etikettnamn* `]`

   I de föregående kommandona definieras platshållarna enligt följande:

   `Host` är namnet på värddatorn där AEM Forms körs.

   `port` är WebServices-porten för den programserver där AEM Forms körs.

   `user` är användarnamnet för AEM Forms-administratören.

   `password` är AEM Forms-administratörens lösenord.

   `label` är textetiketten, som kan vara vilken sträng som helst, för den här säkerhetskopian.

   `timeout` är det antal sekunder efter vilket säkerhetskopieringsläget automatiskt lämnas. Den kan vara 0-10 080. Om värdet är 0, vilket är standardvärdet, kommer säkerhetskopieringsläget aldrig att fungera.

   Mer information om kommandoradsgränssnittet till säkerhetskopieringsläget finns i filen Viktigt i katalogen BackupRestoreCommandline.

### Lämnar säkerhetskopieringslägen {#leaving-backup-modes}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan använda administrationskonsolen eller kommandoradsalternativet om du vill lämna säkerhetskopieringslägena.

**Lämna läget för säker säkerhetskopiering (ögonblicksbild)**

Utför följande åtgärder om du vill använda administrationskonsolen för att ta bort AEM Forms från säkert säkerhetskopieringsläge (ögonblicksbildsläge).

1. Logga in på administrationskonsolen.
1. Klicka på Inställningar > Systeminställningar > Verktyg för säkerhetskopiering.
1. Avmarkera Använd i säkert läge och klicka på OK.

**Lämna alla säkerhetskopieringslägen**

Du kan använda kommandoradsgränssnittet för att ta AEM Forms ur säkert säkerhetskopieringsläge (ögonblicksbildsläge) eller för att avsluta den aktuella säkerhetskopieringsläget (rullande läge). Du kan inte använda administrationskonsolen för att lämna det rullande säkerhetskopieringsläget. I läget för rullande säkerhetskopiering är kontrollerna för Backup Utilities på administrationskonsolen inaktiverade. Använd antingen API-anropet eller kommandot LCBackupMode.

1. Gå till mappen `*[aem-forms root]*/sdk/misc/Foundation/BackupRestoreCommandline`.
1. Beroende på vilket operativsystem du använder kan du redigera skriptet `LCBackupMode.cmd` eller `LCBackupMode.sh` och ange standardvärden som passar ditt system.

   >[!NOTE]
   >
   >Ställ in JAVA_HOME-katalogen enligt beskrivningen i rätt kapitel för programservern i [Förbereder installation av AEM Forms](https://www.adobe.com/go/learn_aemforms_prepareInstallsingle_63)*.*

1. Kör följande kommando på en rad:

   * (Windows) `LCBackupMode.cmd leaveContinuousCoverage [-Host=`*värdnamn* `] [-port=`*portnummer* `] [-user=`*användarnamn* `] [-password=`*lösenord* `]`
   * (Linux®, UNIX®) `LCBackupMode.sh leaveContinuousCoverage [-Host=`*värdnamn* `] [-port=`*portnummer* `] [-user=`*användarnamn* `] [-password=`*lösenord* `]`

     I de föregående kommandona definieras platshållarna enligt följande:

     `Host` är namnet på värddatorn där AEM Forms körs.

     `port` är den port där AEM Forms körs på programservern.

     `user` är användarnamnet för AEM Forms-administratören.

     `password` är AEM Forms-administratörens lösenord.

     `leaveContinuousCoverage` Använd det här alternativet om du vill inaktivera läget för rullande säkerhetskopiering helt.

   >[!NOTE]
   >
   >Det går inte att återupprätta kontinuerlig täckning för den tid som säkerhetskopieringsläget är inaktiverat. Ändringar under den tiden är inte skyddade.

   >[!NOTE]
   >
   >Om du har aktiverat dokumentlagring i databasen kan du inte använda läget för säkerhetskopiering av ögonblicksbilder och rullande säkerhetskopiering.

   Mer information om kommandoradsgränssnittet till säkerhetskopieringsläget finns i filen Viktigt i katalogen BackupRestoreCommandline.
