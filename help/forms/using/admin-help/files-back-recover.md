---
title: Filer som ska säkerhetskopieras och återställas
description: Det här dokumentet beskriver programmet och de datafiler som måste säkerhetskopieras.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 2938a1c6-c8fc-420a-8fad-bb39e5a7936b
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---

# Filer som ska säkerhetskopieras och återställas {#files-to-back-up-and-recover}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

De program- och datafiler som måste säkerhetskopieras beskrivs mer ingående i följande avsnitt.

Tänk på följande när det gäller säkerhetskopiering och återställning:

* Databasen bör säkerhetskopieras före GDS- och AEM-databasen.
* Om du behöver ta ned noderna i en klustrad klustermiljö för säkerhetskopiering måste du se till att de sekundära noderna stängs av före den primära noden. Annars kan det leda till inkonsekvens i klustret eller servern. Dessutom bör den primära noden göras live före en sekundär nod.
* För återställningsåtgärden i ett kluster bör programservern stoppas för varje nod i klustret.

## Katalog för global dokumentlagring {#global-document-storage-directory}

GDS är en katalog som används för att lagra långlivade filer som används i en process. Långvariga filer är avsedda att spänna över en eller flera lanseringar av ett AEM-blankettsystem och kan sträcka sig över flera dagar eller år. Dessa långa filer kan innehålla PDF-filer, profiler och formulärmallar. Långvariga filer är en viktig del av det övergripande tillståndet för många AEM-blanketter. Om vissa eller alla långlivade dokument förloras eller skadas kan Forms Server bli instabil.

Indatadokument för asynkrona jobbanrop lagras också i GDS och måste vara tillgängliga för bearbetning av begäranden. Därför är det viktigt att du ser tillförlitligheten i det filsystem som är värd för GDS och använder en redundant uppsättning av oberoende diskar (RAID) eller annan teknik som passar för dina krav på kvalitet och servicenivå.

Platsen för GDS bestäms under installationen av AEM-formulär eller senare med hjälp av administrationskonsolen. Förutom att ha en plats med hög tillgänglighet för GDS kan du även aktivera databaslagring för dokument. Se [Alternativ för säkerhetskopiering när databasen används för dokumentlagring](files-back-recover.md#backup-options-when-database-is-used-for-document-storage).

### GDS-plats {#gds-location}

Om du lämnar platsinställningen tom under installationen blir platsen som standard en katalog under programserverinstallationen. Säkerhetskopiera följande katalog för programservern:

* (JBoss) `[appserver root]/server/'server'/svcnative/DocumentStorage`
* (WebLogic) `[appserverdomain]/'server'/adobe/AEMformsserver/DocumentStorage`
* (WebSphere) `[appserver root]/installedApps/adobe/'server'/DocumentStorage`

Om du har ändrat GDS-platsen till en annan plats än standardplatsen kan du bestämma den på följande sätt:

* Logga in på administrationskonsolen och klicka på Inställningar > Systeminställningar > Konfigurationer.
* Registrera platsen som anges i rutan Global Document Storage Directory.

I en klustrad miljö pekar GDS vanligtvis på en katalog som delas i nätverket och som kan läsas/skrivas för varje klusternod.

GDS-platserna kan ändras under en återställning om den ursprungliga platsen inte längre är tillgänglig. (Se [Ändra GDS-plats under återställning](/help/forms/using/admin-help/recovering-aem-forms-data.md#changing-the-gds-location-during-recovery).)

### Alternativ för säkerhetskopiering när databasen används för dokumentlagring {#backup-options-when-database-is-used-for-document-storage}

Du kan aktivera dokumentlagring för AEM-formulär i AEM formulärdatabas med administrationskonsolen. Även om det här alternativet behåller alla beständiga dokument i databasen, kräver AEM-formulär fortfarande den filsystembaserade GDS-katalogen eftersom den används för att lagra permanenta och tillfälliga filer och resurser relaterade till sessioner och anrop av AEM-formulär.

När du väljer alternativet &quot;Aktivera dokumentlagring i databasen&quot; i Core System Settings i administrationskonsolen eller med Configuration Manager, tillåter inte AEM-formulär läge för säkerhetskopiering av ögonblicksbilder och rullande säkerhetskopieringsläge. Därför behöver du inte hantera säkerhetskopieringslägen med AEM-formulär. Om du använder det här alternativet bör du endast säkerhetskopiera GDS en gång efter att du har aktiverat alternativet. När du återställer AEM-formulär från en säkerhetskopia behöver du inte byta namn på säkerhetskopieringskatalogen för GDS eller återställa GDS.

## AEM-databas {#aem-repository}

AEM-databasen (crx-database) skapas om crx-databasen konfigureras när AEM-formulär installeras. Platsen för crx-database bestäms under installationen av AEM-formulär. AEM-databasen måste säkerhetskopieras och återställas tillsammans med databas och GDS för att AEM formulärdata ska vara enhetliga i AEM-formulär. AEM-databasen innehåller data för Correspondence Management Solution, Forms Manager och AEM Forms Workspace.

### Correspondence Management Solution {#correspondence-management-solution}

Correspondence Management Solution centraliserar och hanterar framtagning, sammanställning och leverans av säkra, personaliserade och interaktiva korrespondenser. Det gör att ni snabbt kan sammanställa korrespondens från både förgodkänt och skräddarsytt material i en smidig process från det att det skapas till arkivering. Resultatet blir att era kunder får snabb, korrekt, bekväm, säker och relevant kommunikation. Företaget maximerar värdet av kundinteraktioner och minimerar kostnader och risker med en smidig process som är enkel, snabb och effektiv.

En enkel konfiguration av Correspondence Management Solution innehåller en författarinstans och en publiceringsinstans på samma dator eller på olika datorer

### formulärhanterare {#forms-manager}

blanketthanteraren effektiviserar processen att uppdatera, hantera och ta tillbaka blanketter.

### AEM Forms Workspace {#html-workspace}

AEM Forms Workspace matchar funktionerna i (Borttaget för AEM-formulär i JEE) Flex Workspace och har nya funktioner för att utöka och integrera Workspace och göra det mer användarvänligt.

>[!NOTE]
>
>Flex Workspace används inte i AEM formulärversioner.

Den möjliggör uppgiftshantering för klienter utan Flash Player och Adobe Reader. Det underlättar återgivning av HTML Forms, förutom PDF forms- och Flex-formulär.

## AEM blankettdatabas {#aem-forms-database}

AEM formulärdatabas lagrar innehåll som formulärartefakter, tjänstkonfigurationer, processtillstånd och databasreferenser till filer i GDS och rotkatalogen för innehållslagring (för Content Services). Säkerhetskopiering av databaser kan utföras i realtid utan avbrott i tjänsten, och återställning kan ske till en viss tidpunkt eller till en viss ändring. I det här avsnittet beskrivs hur du konfigurerar databasen så att den kan säkerhetskopieras i realtid.

I ett korrekt konfigurerat AEM-formulärsystem kan systemadministratören och databasadministratören enkelt samarbeta för att återställa systemet till ett konsekvent och känt tillstånd.

Om du vill säkerhetskopiera databasen i realtid måste du antingen använda läget för ögonblicksbild eller konfigurera databasen så att den körs i det angivna loggläget. Detta gör att dina databasfiler kan säkerhetskopieras medan databasen är öppen och tillgänglig för användning. Databasen bevarar dessutom sina återställnings- och transaktionsloggar när den körs i dessa lägen.

>[!NOTE]
>
>Adobe® LiveCycle® Content Services ES (utgått) är ett innehållshanteringssystem som installeras med LiveCycle. Det gör det möjligt för användarna att utforma, hantera, övervaka och optimera humancentrerade processer. Supporten för innehållstjänster (borttaget) upphör 2014-12-31. Se [Adobe livscykeldokument](https://www.adobe.com/support/products/enterprise/eol/eol_matrix.html).

### DB2 {#db2}

Konfigurera din DB2-databas så att den körs i arkivloggningsläge.

>[!NOTE]
>
>Om din AEM-formulärmiljö har uppgraderats från en tidigare version av AEM-formulär och använder DB2 stöds inte säkerhetskopiering online. I så fall måste du stänga av AEM-formulär och göra en offlinesäkerhetskopiering. Framtida versioner av AEM-blanketter har stöd för säkerhetskopiering online för uppgraderingskunder.

IBM har en uppsättning verktyg och hjälpsystem som hjälper databasadministratörer att hantera säkerhetskopierings- och återställningsuppgifter:

* IBM DB2 Archive Log Accelerator
* IBM DB2 Data Archive-expert

DB2 har inbyggda funktioner för att säkerhetskopiera en databas till Tivoli Storage Manager. Genom att använda Tivoli Storage Manager kan DB2-säkerhetskopior lagras på andra medier eller på den lokala hårddisken.

### Oracle {#oracle}

Använd säkerhetskopiering av ögonblicksbilder eller konfigurera din Oracle-databas så att den körs i arkivloggläge. (Se [Oracle Säkerhetskopiering: En introduktion](https://www.databasedesign-resource.com/oracle-backup.md).) Mer information om hur du säkerhetskopierar och återställer din Oracle-databas finns på följande webbplatser:

[Oracle Säkerhetskopiering och återställning:](https://www.oracle.com/technetwork/database/features/availability/br-overview-097160.html) Beskriver koncept för säkerhetskopiering och återställning och de vanligaste teknikerna för att använda Recovery Manager (RMAN) för säkerhetskopiering, återställning och rapportering mer i detalj samt ger mer information om hur du planerar en strategi för säkerhetskopiering och återställning.

[Användarhandbok för säkerhetskopiering och återställning av Oracle-databas:](https://download.oracle.com/docs/cd/E11882_01/backup.112/e10642.pdf) innehåller detaljerad information om RMAN-arkitektur, koncept och mekanismer för säkerhetskopiering och återställning, avancerade återställningstekniker som återställning och flashback-funktioner vid tidpunkt samt prestandajustering för säkerhetskopiering och återställning. Det omfattar även användarhanterad säkerhetskopiering och återställning med användarens operativsystem istället för RMAN. Den här volymen är väsentlig för säkerhetskopiering och återställning av mer avancerade databasdistributioner och för avancerade återställningsscenarier.

[Referens för säkerhetskopiering och återställning av Oracle-databas:](https://download.oracle.com/docs/cd/E11882_01/backup.112/e10643.pdf) Tillhandahåller fullständig information om syntax och semantik för alla RMAN-kommandon och beskriver databasvyerna som är tillgängliga för rapportering av säkerhetskopierings- och återställningsaktiviteter.

### SQL Server {#sql-server}

Använd säkerhetskopiering av ögonblicksbilder eller konfigurera SQL Server-databasen så att den körs i transaktionsloggläge.

SQL Server har även två verktyg för säkerhetskopiering och återställning:

* SQL Server Management Studio (GUI)
* T-SQL (kommandorad)

Mer information finns i [Säkerhetskopiera och återställ](https://msdn.microsoft.com/en-us/library/ms187048(v=SQL.90).aspx).

### MySQL {#mysql}

Använd MySQLAdmin eller ändra INI-filerna i Windows för att konfigurera MySQL-databasen så att den körs i binärt loggläge. (Se [Binär MySQL-loggning](https://dev.mysql.com/doc/refman/5.1/en/binary-log.html).) Ett verktyg för säkerhetskopiering under drift för MySQL finns också tillgängligt från programmet InnoBase. (Se [Innobase Hot Backup](https://www.innodb.com/hot-backup/features.md).)

>[!NOTE]
>
>Standardläget för binär loggning för MySQL är &quot;Statement&quot;, vilket är inkompatibelt med tabeller som används av Content Services (utgått). Om du använder binär loggning i det här standardläget misslyckas Content Services (Borttagen). Om ditt system innehåller innehållstjänster (borttaget) använder du loggningsläget Blandat. Om du vill aktivera blandad loggning lägger du till följande argument i filen my.ini: `binlog_format=mixed log-bin=logname`

Du kan använda verktyget mysqldump för att få en fullständig säkerhetskopiering av databasen. Fullständig säkerhetskopiering krävs, men är inte alltid lämplig. De producerar stora säkerhetskopior och tar tid att generera. Om du vill göra en stegvis säkerhetskopiering måste du starta servern med alternativet - `log-bin` enligt beskrivningen i föregående avsnitt. Varje gång MySQL-servern startas om slutar den skriva till den aktuella binära loggen, skapar en ny och från och med då blir den nya den aktuella. Du kan tvinga en växel manuellt med kommandot `FLUSH LOGS SQL`. Efter den första fullständiga säkerhetskopieringen utförs efterföljande stegvisa säkerhetskopieringar med hjälp av verktyget mysqladmin med kommandot `flush-logs`, som skapar nästa loggfil.

Se [Sammanfattning av säkerhetskopieringsstrategi](https://dev.mysql.com/doc/refman/5.5/en/backup-strategy-summary.html).

```text
binlog_format=mixed
log-bin=logname
```

## Rotkatalog för innehållslagring (endast innehållstjänster) {#content-storage-root-directory-content-services-only}

Katalogen Content Storage Root innehåller databasen Content Services (Borttagen) där alla dokument, artefakter och index lagras. Katalogträdet för innehållslagring måste säkerhetskopieras. I det här avsnittet beskrivs hur du fastställer platsen för rotkatalogen för innehållslagring för både fristående och klustrade miljöer.

### Rotplats för innehållslagring (fristående miljö) {#content-storage-root-location-stand-alone-environment}

Rotkatalogen för innehållslagring skapas när Content Services (Borttagen) installeras. Platsen för rotkatalogen för innehållslagring bestäms under installationen av AEM-formulär.

Standardplatsen för rotkatalogen för innehållslagring är `[aem-forms root]/lccs_data`.

Säkerhetskopiera följande kataloger i rotkatalogen för innehållslagring:

/audit.contentstore

/contentstore

/contentstore.deleted

/backup-lucene-indexes

Om katalogen /backup-lucene-indexes inte finns säkerhetskopierar du katalogen /lucene-indexes, även i rotkatalogen för innehållslagring. Om katalogen /backup-lucene-indexes finns ska du inte säkerhetskopiera katalogen /lucene-indexes eftersom det kan orsaka fel.

### Rotplats för innehållslagring (klustrad miljö) {#content-storage-root-location-clustered-environment}

När du installerar innehållstjänster (borttagna) i en klustrad miljö delas rotkatalogen för innehållslagring upp i två separata kataloger:

**Rotkatalog för innehållslagring:** Vanligtvis är en delad nätverkskatalog som är läsbar/skrivskyddad för alla noder i klustret

**Indexrotkatalog:** En katalog som skapas på varje nod i klustret och som alltid har samma sökväg och katalognamn

Standardplatsen för rotkatalogen för innehållslagring är `[GDS root]/lccs_data`, där `[GDS root]` är den plats som beskrivs i [GDS-platsen](files-back-recover.md#gds-location). Säkerhetskopiera följande kataloger i rotkatalogen för innehållslagring:

/audit.contentstore

/contentstore

/contentstore.deleted

/backup-lucene-indexes

Om katalogen /backup-lucene-indexes inte finns säkerhetskopierar du katalogen /lucene-indexes, även i rotkatalogen för innehållslagring. Om katalogen /backup-lucene-indexes finns ska du inte säkerhetskopiera katalogen /lucene-indexes eftersom det kan orsaka fel.

Standardplatsen för indexrotkatalogen är `[aem-forms root]/lucene-indexes` på varje nod.

## Kundinstallerade teckensnitt {#customer-installed-fonts}

Om du har installerat ytterligare teckensnitt i din AEM-formulärmiljö måste du säkerhetskopiera dem separat. Säkerhetskopiera alla Adobe- och kundteckensnittskataloger som anges i administrationskonsolen under Inställningar > Kärnsystem > Konfigurationer. Se till att du säkerhetskopierar hela teckensnittskatalogen.

>[!NOTE]
>
>Som standard finns Adobe-teckensnitten som är installerade med AEM-formulär i katalogen `[aem-forms root]/fonts`.

Om du initierar om operativsystemet på värddatorn och vill använda teckensnitt från det tidigare operativsystemet, bör innehållet i systemteckensnittskatalogen också säkerhetskopieras. (Mer information finns i dokumentationen för ditt operativsystem).
