---
title: Adobe Experience Manager med MongoDB
description: Lär dig mer om de uppgifter och överväganden som krävs för att distribuera Adobe Experience Manager med MongoDB.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
hide: true
hidefromtoc: true
exl-id: af957cd7-ad3d-46f2-9ca5-e175538104f1
source-git-commit: 0e60c406a9cf1e5fd13ddc09fd85d2a2f8a410f6
workflow-type: tm+mt
source-wordcount: '5965'
ht-degree: 0%

---

# Adobe Experience Manager med MongoDB{#aem-with-mongodb}

>[!NOTE]
>
>Den lägsta version av Mongo som stöds är Mongo 6.

Den här artikeln syftar till att förbättra kunskapen om de uppgifter och överväganden som krävs för att distribuera AEM (Adobe Experience Manager) med MongoDB.

Mer distributionsrelaterad information finns i avsnittet [Distribuera och underhålla](/help/sites-deploying/deploy.md) i dokumentationen.

## När MongoDB ska användas med AEM {#when-to-use-mongodb-with-aem}

MongoDB används vanligtvis för att stödja distributioner av AEM-författare när något av följande villkor uppfylls:

* Mer än 1000 unika användare per dag.
* Mer än 100 samtidiga användare.
* stora volymer sidredigeringar,
* Stora utrullningar eller aktiveringar.

Kriterierna ovan gäller bara för författarinstanserna och inte för publiceringsinstanser som ska vara TjärMK-baserade. Antalet användare refererar till autentiserade användare, eftersom författarinstanser inte tillåter oautentiserad åtkomst.

Om villkoren inte uppfylls rekommenderar vi en aktiverings-/standby-distribution för att åtgärda tillgängligheten. Vanligtvis bör MongoDB övervägas i situationer där skalningskraven är mer än vad som kan uppnås med en enda maskinvaruartikel.

>[!NOTE]
>
>Mer information om storleken på författarinstanser och definitionen av samtidiga användare finns i [Riktlinjerna för maskinvarustorlek](/help/managing/hardware-sizing-guidelines.md#authors-working-in-parallel).

### Minimal MongoDB-distribution för AEM {#minimal-mongodb-deployment-for-aem}

Nedan visas en minimal driftsättning för AEM på MongoDB. För enkelhetens skull har SSL-terminering och HTTP-proxykomponenter generaliserats. Den består av en enda MongoDB-replikuppsättning, med en primär och två sekundära.

![chlimage_1-4](assets/chlimage_1-4.png)

En minimal distribution kräver tre `mongod`-instanser konfigurerade som en replikuppsättning. En instans har valts som primär och de andra instanserna är sekundära, med markeringen hanterad av `mongod`. Kopplade till varje instans är en lokal disk. Klustret kan alltså hantera belastningen, och en minsta genomströmning på 12 MB per sekund med mer än 3 000 I/O-åtgärder per sekund (IOPS) rekommenderas.

AEM-författarna är anslutna till `mongod`-instanserna, där varje AEM-författare ansluter till alla tre `mongod`-instanserna. Skrivningar skickas till det primära och läsningar kan läsas från någon av instanserna. Trafiken distribueras baserat på inläsning från en Dispatcher till någon av de aktiva AEM-författarinstanserna. Oak datalager är en `FileDataStore` och MongoDB-övervakning tillhandahålls av MMS eller MongoDB Ops Manager beroende på platsen för distributionen. Operativsystemnivå och loggövervakning tillhandahålls av tredjepartslösningar som Splunk eller Ganglia.

I den här distributionen krävs alla komponenter för en lyckad implementering. Om en komponent saknas blir implementeringen icke-funktionell.

### Operativsystem {#operating-systems}

En lista över vilka operativsystem som stöds för AEM 6.5 LTS finns på sidan [Tekniska krav](/help/sites-deploying/technical-requirements.md).

### Miljö {#environments}

Virtualiserade miljöer stöds förutsatt att det finns god kommunikation mellan de olika tekniska team som kör projektet. Det här stödet omfattar teamet som kör AEM, teamet som äger operativsystemet och teamet som hanterar den virtualiserade infrastrukturen.

Det finns specifika krav som omfattar I/O-kapaciteten för MongoDB-instanserna som måste hanteras av det team som hanterar den virtualiserade miljön. Om projektet använder en molndistribution, som Amazon Web Services, måste instanser etableras med tillräcklig I/O-kapacitet och konsekvens för att stödja MongoDB-instanserna. Annars fungerar MongoDB-processerna och Oak-databasen otillförlitligt och felaktigt.

I virtualiserade miljöer kräver MongoDB specifika I/O- och VM-konfigurationer för att säkerställa att lagringsmotorn i MongoDB inte har någon VMWare-resursallokeringsprinciper. En framgångsrik implementering säkerställer att det inte finns några hinder mellan de olika teamen och att alla är registrerade för att leverera de prestanda som krävs.

## Maskinvarufrågor {#hardware-considerations}

### Lagring {#storage}

För att uppnå läs- och skrivgenomströmning för bästa prestanda utan behov av för tidig horisontell skalning, kräver MongoDB vanligtvis SSD-lagring eller lagring med prestanda som motsvarar SSD.

### RAM {#ram}

Otillräckligt RAM-minne ger en avsevärd prestandaförsämring. Storleken på arbetsuppsättningen och databasen är mycket programberoende. Även om det går att göra vissa uppskattningar är det mest tillförlitliga sättet att fastställa mängden RAM-minne som krävs att bygga AEM-programmet och testa det.

För att underlätta inläsningstestningsprocessen kan följande förhållande mellan arbetsuppsättningens och databasens totala storlek antas:

* 1:10 för SSD-lagring
* 1:3 för hårddisklagring

Dessa proportioner innebär att för SSD-distributioner krävs 200 GB RAM för en databas på 2 TB.

>[!NOTE]
>
>Adobe rekommenderar att du använder lagringsmotorn WiredTiger för AEM 6.5 LTS-distributioner som använder MongoDB 6 eller senare.

### Datalager {#data-store}

På grund av begränsningarna i MongoDB-arbetsuppsättningen rekommenderas att datalagret upprätthålls oberoende av MongoDB. I de flesta miljöer bör en `FileDataStore` som använder en NAS som är tillgänglig för alla AEM-instanser användas. För situationer där Amazon Web Services används finns det också en `S3 DataStore`. Om datalagret av någon anledning bevaras i MongoDB, bör datalagrets storlek läggas till i den totala databasstorleken och arbetsmängdsberäkningarna justeras på rätt sätt. Den här storleksändringen kan innebära att mer RAM-minne etableras för att upprätthålla prestanda utan sidfel.

## Övervakning {#monitoring}

Övervakning är nödvändigt för ett framgångsrikt genomförande av projektet. Med tillräcklig kunskap är det möjligt att köra AEM på MongoDB utan övervakning. Denna kunskap finns dock normalt hos tekniker som är specialiserade för varje del av driftsättningen.

Denna specialkunskap innefattar normalt en FoU-tekniker som arbetar på Apache Oak Core och en MongoDB-specialist.

Utan övervakning på alla nivåer krävs detaljerade kunskaper om kodbasen för att kunna diagnostisera problem. Med övervakning på plats och lämplig vägledning om huvudstatistiken kan genomförandegrupper reagera på avvikelser på lämpligt sätt.

Det går att använda kommandoradsverktyg för att få en snabb ögonblicksbild av driften av ett kluster, men det är nästan omöjligt att göra det i realtid över många värdar. Kommandoradsverktyg ger sällan historisk information längre än några minuter och tillåter aldrig korrelation mellan olika typer av mätvärden. En kort period av långsam bakgrundssynkronisering `mongod` kräver en betydande manuell insats för att korrelera mot I/O Wait eller överdrivet skrivande till en delad lagringsresurs från en till synes oansluten virtuell dator.

### MongoDB Cloud Manager {#mongodb-cloud-manager}

MongoDB Cloud Manager är en kostnadsfri tjänst som erbjuds av MongoDB som tillåter övervakning och hantering av MongoDB-instanser. Den ger en översikt över prestanda och hälsa för MongoDB-klustret i realtid. Den hanterar både molninstanser och privata värdinstanser förutsatt att instansen kan nå Cloud Manager övervakningsserver.

Det kräver en agent som är installerad på MongoDB-instansen som ansluter till övervakningsservern. Det finns tre nivåer av agenset:

* En automatiseringsagent som helt kan automatisera allt på MongoDB-servern,
* En övervakningsagent som kan övervaka instansen `mongod`,
* En säkerhetskopieringsagent som kan utföra schemalagda säkerhetskopieringar av data.

Även om det är enklare att använda Cloud Manager för underhållsautomatisering av ett MongoDB-kluster är många av rutinuppgifterna inte nödvändiga, och inte heller används de för säkerhetskopiering. Övervakning krävs dock när du väljer en Cloud Manager som ska övervakas.

Mer information om MongoDB Cloud Manager finns i [MongoDB-dokumentationen](https://docs.cloud.mongodb.com/).

### Ops-hanteraren för MongoDB {#mongodb-ops-manager}

MongoDB Ops Manager är samma programvara som MongoDB Cloud Manager. När Ops Manager har registrerats kan det laddas ned och installeras lokalt i ett privat datacenter eller på en annan bärbar eller stationär dator. Den använder en lokal MongoDB-databas för att lagra data och kommunicera på samma sätt som Cloud Manager med de hanterade servrarna. Om du har säkerhetsprofiler som förbjuder en övervakningsagent bör MongoDB Ops Manager användas.

### Övervakning av operativsystem {#operating-system-monitoring}

Övervakning på operativsystemnivå krävs för att köra ett AEM MongoDB-kluster.

Ganglia är ett bra exempel på ett sådant system och ger en bild av hur omfattande och detaljerad information som krävs utöver grundläggande hälsomått som CPU, genomsnittlig belastning och ledigt diskutrymme. För att kunna diagnostisera problem krävs information på lägre nivå, t.ex. entropisolnivåer, CPU I/O Wait, socketar i FIN_WAIT2-läge.

### Loggaggregering {#log-aggregation}

Med ett kluster av flera servrar är central loggaggregering ett krav för ett produktionssystem. Programvara som Splunk har stöd för loggaggregering och gör det möjligt för team att analysera beteendemönster i programmet utan att behöva samla in loggarna manuellt.

## Checklistor {#checklists}

I det här avsnittet beskrivs olika åtgärder som du bör vidta för att se till att dina AEM- och MongoDB-distributioner är korrekt konfigurerade innan du implementerar ditt projekt.

### Nätverk {#network}

1. Kontrollera först att alla värdar har en DNS-post
1. Alla värdar ska kunna matchas genom sin DNS-post från alla andra routningsbara värdar
1. Alla MongoDB-värdar kan dirigeras från alla andra MongoDB-värdar i samma kluster
1. MongoDB-värdar kan dirigera paket till MongoDB Cloud Manager och andra övervakningsservrar
1. AEM-servrar kan dirigera paket till alla MongoDB-servrar
1. Fördröjning för paket mellan en AEM-server och en MongoDB-server är mindre än två millisekunder utan paketförlust och standarddistribution på en millisekund eller mindre.
1. Kontrollera att det inte finns mer än två hopp mellan en AEM- och en MongoDB-server
1. Det finns bara två hopp mellan två MongoDB-servrar
1. Det finns inga routrar som är högre än OSI Level 3 mellan några huvudservrar (MongoDB eller AEM eller någon kombination).
1. Om VLAN-trunkering eller någon form av nätverkstunnling används måste den uppfylla paketets latenskontroller.

### AEM Configuration {#aem-configuration}

#### Konfiguration av nodarkiv {#node-store-configuration}

AEM-instanserna måste konfigureras att använda AEM med MongoMK. Basen på mongoMK-implementeringen i AEM är Document Node Store.

Mer information om hur du konfigurerar nodarkiv finns i [Konfigurera nodarkiv och datalager i AEM](/help/sites-deploying/data-store-config.md).

Nedan visas ett exempel på Document Node Store-konfiguration för en minimal MongoDB-distribution:

```xml
# org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config
#MongoDB server details
mongodburi=mongodb://aem:aempassword@mongodbserver1.customer.com:27000,mongodbserver2.customer.com:27000

#Name of MongoDB database to use
db=aem

#Store binaries in custom BlobStore for example, FileDataStore
customBlobStore=true

cache=2048
blobCacheSize=1024
```

Var:

* `mongodburi`
MongoDB-servern som AEM måste ansluta till. Anslutningar görs till alla kända medlemmar i standardreplikuppsättningen. Om MongoDB Cloud Manager används aktiveras serversäkerhet. Därför måste anslutningssträngen innehålla ett lämpligt användarnamn och lösenord. Icke-företagsversioner av MongoDB stöder endast autentisering av användarnamn och lösenord. Mer information om syntaxen för anslutningssträngar finns i [dokumentationen](https://docs.mongodb.org/manual/reference/connection-string/).

* `db`
Namnet på databasen. Standardvärdet för AEM är `aem-author`.

* `customBlobStore`
Om distributionen lagrar binärfiler i databasen utgör de en del av arbetsuppsättningen. Av den anledningen bör du inte lagra binärfiler i MongoDB. Du bör därför välja ett alternativt datalager, som ett `FileSystem`-datalager på en NAS.

* `cache`
Cachestorleken i MB. Det här utrymmet fördelas mellan olika cacheminnen som används i `DocumentNodeStore`. Standardvärdet är 256 MB. Men Oak har bättre läsprestanda tack vare ett större cacheminne.

* `blobCacheSize`
Ofta använda bloggar kan cachas av AEM för att undvika att de hämtas från datalagret. Detta har större inverkan på prestandan, särskilt när du lagrar blobbar i MongoDB-databasen. Alla filsystembaserade datalager drar nytta av diskcachen på operativsystemnivå.

#### Konfiguration av datalager {#data-store-configuration}

Datalagret används för att lagra filer som är större än ett tröskelvärde. Under det tröskelvärdet lagras filer som egenskaper i Document Node Store. Om `MongoBlobStore` används skapas en dedikerad samling i MongoDB för att lagra blobbarna. Den här samlingen bidrar till arbetsuppsättningen för instansen `mongod` och kräver att `mongod` har mer RAM-minne för att undvika prestandaproblem. Av den anledningen rekommenderas att konfigurationen undviker `MongoBlobStore` för produktionsdistributioner och använder `FileDataStore` som stöds av en NAS som delas av alla AEM-instanser. Eftersom cacheminnet på operativsystemnivå är effektivt vid filhantering bör den minsta storleken för en fil på disken ställas in på nära diskens blockstorlek. Om du gör det kan du vara säker på att filsystemet används effektivt, och många små dokument bidrar inte så mycket till `mongod`-instansens arbetsuppsättning.

Här är en typisk datalagerkonfiguration för en minimal AEM-driftsättning med MongoDB:

```xml
# org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config
# The minimum size of an object that should be stored in this data store.
minRecordLength=4096
path=/datastore
maxCachedBinarySize=4096
cacheSizeInMB=128
```

Var:

* `minRecordLength`
Storlek i byte. Binärfiler som är mindre än eller lika med den här storleken lagras med Document Node Store. I stället för att lagra blobbens ID lagras innehållet i binärfilen. Med binärfiler som är större än den här storleken lagras binärfilens ID som en dokumentegenskap i nodsamlingen. Och binärfilens brödtext lagras i `FileDataStore` på disken. 4 096 byte är en typisk blockstorlek i filsystemet.

* `path`
Sökvägen till datalagrets rot. För en MongoMK-distribution måste sökvägen vara ett delat filsystem som är tillgängligt för alla AEM-instanser. Vanligtvis används en NAS-server (Network Attached Storage). För molndistributioner som Amazon Web Services är även `S3DataFileStore` tillgänglig.

* `cacheSizeInMB`
Den totala storleken på binärt cacheminne i megabyte. Den används för att cachelagra binärfiler som är mindre än inställningen `maxCacheBinarySize`.

* `maxCachedBinarySize`
Maximal storlek i byte för en binär cache-lagring i den binära cachen. Om ett filsystembaserat datalager används bör du inte använda höga värden för datalagrets cache eftersom binärfilerna redan cachas av operativsystemet.

#### Inaktivera frågetipset {#disabling-the-query-hint}

Du bör inaktivera frågetipset som skickas med alla frågor genom att lägga till egenskapen `-Doak.mongo.disableIndexHint=true` när du startar AEM. På så sätt ser du till att MongoDB beräknar det lämpligaste indexvärdet baserat på intern statistik.

Om frågetipset inte är inaktiverat påverkar prestandajusteringen av index inte AEM prestanda.

#### Aktivera beständig cache för MongoMK {#enable-persistent-cache-for-mongomk}

Vi rekommenderar att en beständig cachekonfiguration aktiveras för MongoDB-distributioner för att maximera hastigheten i miljöer med höga I/O-läsprestanda. Mer information finns i [Jackrabbit Oak-dokumentationen](https://jackrabbit.apache.org/oak/docs/nodestore/persistent-cache.html).

## Optimering av operativsystemet MongoDB {#mongodb-operating-system-optimizations}

### Stöd för operativsystem {#operating-system-support}

För snabb åtgärd bör MongoDB-databasen bara komma åt data som redan finns i RAM-minnet. De data som de måste komma åt består av index och data. Den här samlingen med index och data kallas för arbetsuppsättningen. Om arbetsuppsättningen är större än den tillgängliga RAM MongoDB måste skicka in data från disken till en I/O-kostnad, och andra data som redan finns i minnet raderas. Om avlägsnandet medför att data läses in på nytt från disken, dominerar sidfelen och prestandan försämras. Om arbetsuppsättningen är dynamisk och variabel uppstår fler sidfel som stöd för åtgärder.

MongoDB kan köras i flera operativsystem, bland annat en mängd olika Linux®-versioner, Windows och macOS. Mer information finns på [https://docs.mongodb.com/manual/installation/#supported-platforms](https://docs.mongodb.com/manual/installation/#supported-platforms). Beroende på vilket operativsystem du väljer har MongoDB olika rekommendationer på operativsystemnivå. Dokumentationen finns på [https://docs.mongodb.com/manual/administration/production-checklist-operations/#operating-system-configuration](https://docs.mongodb.com/manual/administration/production-checklist-operations/#operating-system-configuration) och sammanfattas här.

#### Linux® {#linux}

* Stäng av de genomskinliga övertoningarna och överstrålningarna. Mer information finns i [Inställningar för genomskinliga stora sidor](https://docs.mongodb.com/manual/tutorial/transparent-huge-pages/).
* [Justera readahead-inställningarna](https://docs.mongodb.com/manual/administration/production-notes/#readahead) på enheterna som lagrar databasfilerna så att du passar ditt användningssätt.

   * För lagringsmotorn WiredTiger ska du ställa in readahead på 0 oavsett lagringsmedietyp (snurrning, SSD osv.). I allmänhet bör du använda den rekommenderade inställningen för framåtriktad avläsning, såvida inte testningen visar en mätbar, upprepningsbar och tillförlitlig fördel i ett högre avläsningsvärde. [Stöd för MongoDB Professional](https://docs.mongodb.com/manual/administration/production-notes/#readahead) kan ge råd och vägledning om icke-nollbaserade readahead-konfigurationer.

* Inaktivera det justerade verktyget om du kör RHEL 7/CentOS 7 i en virtuell miljö.
* När RHEL 7/CentOS 7 körs i en virtuell miljö anropar det justerade verktyget automatiskt en prestandaprofil som härleds från prestandagenomströmning, vilket automatiskt ställer in readahead-inställningarna till 4 MB. Den här inställningen kan påverka prestandan negativt.
* Använd diskschemaläggaren för noop eller deadline för SSD-enheter.
* Använd diskschemaläggaren på toppen för virtualiserade enheter på virtuella gästdatorer.
* Inaktivera NUMA eller ange `vm.zone_reclaim_mode` till 0 och kör [mongod](https://docs.mongodb.com/manual/administration/production-notes/#readahead)-instanser med nodinterfoliering. Mer information finns i [MongoDB och NUMA Hardware](https://docs.mongodb.com/manual/administration/production-notes/#readahead).

* Justera de högsta tillåtna värdena för maskinvaran så att de passar ditt användningssätt. Om flera [mongod](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) - eller [mongos](https://docs.mongodb.com/manual/reference/program/mongos/#bin.mongos)-instanser körs under samma användare, skalar du gränsvärdena i enlighet med detta. Mer information finns i [UNIX® Ulimit Settings](https://docs.mongodb.com/manual/reference/ulimit/).

* Använd noatime för monteringspunkten [dbPath](https://docs.mongodb.com/manual/reference/configuration-options/#storage.dbPath).
* Konfigurera tillräckliga filreferenser (fs.file-max), kernelns pidgräns (kernel.pid_max) och maximala trådar per process (kernel.threads-max) för distributionen. För stora system ger följande värden en bra utgångspunkt:

   * fs.file-max värde 98000,
   * kernel.pid_max värde på 64000,
   * andkernel.threads-max value of 64000

* Kontrollera att utbytesutrymmet är konfigurerat på datorn. Mer information om lämplig storlek finns i dokumentationen för ditt operativsystem.
* Kontrollera att systemets standardinställning för TCP Keepalive är korrekt. Värdet 300 ger ofta bättre prestanda för replikuppsättningar och delade kluster. Se: [Påverkar TCP-livetid MongoDB-distributioner?](https://docs.mongodb.com/manual/faq/diagnostics/#faq-keepalive) i Vanliga frågor om du vill ha mer information.

#### Windows {#windows}

* Överväg att inaktivera uppdateringarna för senaste åtkomsttid för NTFS. Den här inställningen motsvarar inaktivering av tid på Unix-liknande system.

### WiredTiger {#wiredtiger}

Från och med MongoDB 3.2 är standardlagringsmotorn för MongoDB lagringsmotorn WiredTiger. Den här motorn har en del robusta och skalbara funktioner som gör den mycket bättre lämpad för allmänna databasarbetsbelastningar. I följande avsnitt beskrivs dessa funktioner.

#### Samtidighet på dokumentnivå {#document-level-concurrency}

WiredTiger använder samtidighetskontroll på dokumentnivå för skrivåtgärder. Det innebär att flera klienter kan ändra olika dokument i en samling samtidigt.

För de flesta läs- och skrivåtgärder använder WiredTiger en optimistisk samtidighetskontroll. WiredTiger använder endast intent-lås på global nivå, databas- och samlingsnivå. När lagringsmotorn upptäcker konflikter mellan två åtgärder uppstår en skrivkonflikt som gör att MongoDB försöker utföra åtgärden igen. För vissa globala åtgärder, vanligtvis kortvariga åtgärder som omfattar flera databaser, krävs fortfarande ett globalt &quot;instansövergripande&quot; lås.

Vissa andra åtgärder, till exempel att släppa en samling, kräver fortfarande ett exklusivt databaslås.

#### Fixeringar och kontrollpunkter {#snapshots-and-checkpoints}

WiredTiger använder MultiVersion Concurrency Control (MVCC). I början av en åtgärd ger WiredTiger en ögonblicksbild av data som skickas till transaktionen. En ögonblicksbild ger en enhetlig vy över data i minnet.

När WiredTiger skriver till disk, skrivs alla data i en ögonblicksbild på ett konsekvent sätt över alla datafiler. Nu- [varaktiga](https://docs.mongodb.com/manual/reference/glossary/#term-durable)-data fungerar som en kontrollpunkt i datafilerna. Kontrollpunkten ser till att datafilerna är konsekventa till och med den sista kontrollpunkten. Det innebär att kontrollpunkter kan fungera som återställningspunkter.

MongoDB konfigurerar WiredTiger för att skapa kontrollpunkter (d.v.s. skriva ögonblicksbildsdata till disk) med 60 sekunders intervall eller 2 GB journaldata.

När en ny kontrollpunkt skrivs är den föregående kontrollpunkten fortfarande giltig. Även om MongoDB avbryter eller stöter på ett fel när en ny kontrollpunkt skrivs kan MongoDB återställas från den senaste giltiga kontrollpunkten när den startas om.

Den nya kontrollpunkten blir tillgänglig och permanent när metadatatabellen i WiredTiger uppdateras automatiskt för att referera till den nya kontrollpunkten. När den nya kontrollpunkten är tillgänglig frigör WiredTiger sidor från de gamla kontrollpunkterna.

Med WiredTiger kan MongoDB återställas från den senaste kontrollpunkten, även utan [journalföring](https://docs.mongodb.com/manual/reference/glossary/#term-durable). Om du vill återställa ändringar som gjorts efter den senaste kontrollpunkten kör du med [journaling](https://docs.mongodb.com/manual/core/wiredtiger/#storage-wiredtiger-journal).

#### Journal {#journal}

WiredTiger använder en transaktionsinloggningskombination för write-ahead med [kontrollpunkter](https://docs.mongodb.com/manual/core/wiredtiger/#storage-wiredtiger-checkpoints) för att säkerställa datalagring.

WiredTiger-journalen består av alla dataändringar mellan kontrollpunkterna. Om MongoDB avslutas mellan kontrollpunkter används journalen för att spela upp alla data som ändrats sedan den senaste kontrollpunkten. Information om hur ofta MongoDB skriver journaldata till disk finns i [Journalföringsprocess](https://docs.mongodb.com/manual/core/journaling/#journal-process).

WiredTiger-journalen komprimeras med komprimeringsbiblioteket [snappy](https://docs.mongodb.com/manual/core/journaling/#journal-process) . Om du vill ange en alternativ komprimeringsalgoritm eller ingen komprimering använder du inställningen [storage.wiredTiger.engineConfig.journalCompressor](https://docs.mongodb.com/manual/reference/configuration-options/#storage.wiredTiger.engineConfig.journalCompressor) .

Se [Journalföring med WiredTiger](https://docs.mongodb.com/manual/core/journaling/#journaling-wiredtiger).

>[!NOTE]
>
>Minsta loggpoststorlek för WiredTiger är 128 byte. Om en loggpost är 128 byte eller mindre komprimeras inte posten av WiredTiger.
>
>Du kan inaktivera journalföring genom att ställa in [storage.journal.enabled](https://docs.mongodb.com/manual/reference/configuration-options/#storage.journal.enabled) på false, vilket kan minska overheadkostnaden för att underhålla journalen.
>
>Om du inte använder journalen för [fristående](https://docs.mongodb.com/manual/reference/glossary/#term-standalone) instanser innebär det att du förlorar vissa dataändringar när MongoDB avslutas oväntat mellan kontrollpunkter. För medlemmar i [replikuppsättningar](https://docs.mongodb.com/manual/reference/glossary/#term-replica-set) kan replikeringsprocessen ge tillräckliga varaktighetsgarantier.

#### Komprimering {#compression}

Med WiredTiger stöder MongoDB komprimering för alla samlingar och index. Komprimering minimerar användningen av lagringsutrymme på bekostnad av ytterligare CPU.

Som standard använder WiredTiger blockkomprimering med komprimeringsbiblioteket [snappy](https://docs.mongodb.com/manual/reference/glossary/#term-snappy) för alla samlingar och [prefix-komprimering](https://docs.mongodb.com/manual/reference/glossary/#term-prefix-compression) för alla index.

För samlingar är blockkomprimering med [zlib](https://docs.mongodb.com/manual/reference/glossary/#term-zlib) också tillgängligt. Om du vill ange en alternativ komprimeringsalgoritm eller ingen komprimering använder du inställningen [storage.wiredTiger.collectionConfig.blockCompressor](https://docs.mongodb.com/manual/reference/glossary/#term-zlib) .

Om du vill inaktivera [prefixkomprimering](https://docs.mongodb.com/manual/reference/glossary/#term-prefix-compression) för index använder du inställningen [storage.wiredTiger.indexConfig.prefixCompression](https://docs.mongodb.com/manual/reference/configuration-options/#storage.wiredTiger.indexConfig.prefixCompression) .

Komprimeringsinställningarna kan även konfigureras per samling och per index när du skapar samlingar och index. Se [Ange lagringsmotoralternativ](https://docs.mongodb.com/manual/reference/method/db.createCollection/#create-collection-storage-engine-options) och alternativet [db.collection.createIndex() storageEngine](https://docs.mongodb.com/manual/reference/method/db.collection.createIndex/#createindex-options) .

För de flesta arbetsbelastningar balanserar standardkomprimeringsinställningarna lagringseffektivitet och bearbetningskrav.

Journalen WiredTiger komprimeras också som standard. Mer information om journalkomprimering finns i [Journal](https://docs.mongodb.com/manual/core/wiredtiger/#storage-wiredtiger-journal).

#### Minnesanvändning {#memory-use}

Med WiredTiger använder MongoDB både det interna WiredTiger-cacheminnet och filsystemets cache.

Från och med 3.4 används som standard det interna cache-minnet för WiredTiger det större av antingen:

* 50 % RAM minus 1 GB, eller
* 256 MB

Som standard använder WiredTiger blockkomprimering för Snappy för alla samlingar och prefixkomprimering för alla index. Komprimeringsstandardinställningarna kan konfigureras på global nivå och kan även ställas in per samling och per index när du skapar samlingar och index.

Olika representationer används för data i det interna WiredTiger-cacheminnet jämfört med i formatet på disken:

* Data i filsystemets cache är samma som på disken-formatet, inklusive fördelarna med komprimering för datafiler. Filsystemets cache används av operativsystemet för att minska I/O-diskens storlek.

Index som läses in i det interna WiredTiger-cacheminnet har en annan datarepresentation än formatet på disken, men kan ändå utnyttja indexprefixkomprimering för att minska RAM-användningen.

Vid komprimering av indexprefix dupliceras vanliga prefix från indexerade fält.

Samlingsdata i det interna WiredTiger-cacheminnet är okomprimerade och använder en annan representation än i formatet på disken. Blockkomprimering kan ge avsevärda besparingar på disken, men data måste vara okomprimerade för att kunna hanteras av servern.

Via filsystemets cache använder MongoDB automatiskt allt ledigt minne som inte används av WiredTiger-cachen eller av andra processer.

Information om hur du justerar storleken på det interna WiredTiger-cacheminnet finns i [storage.wiredTiger.engineConfig.cacheSizeGB](https://docs.mongodb.com/manual/reference/configuration-options/#storage.wiredTiger.engineConfig.cacheSizeGB) och [—wiredTigerCacheSizeGB](https://docs.mongodb.com/manual/reference/program/mongod/#cmdoption-wiredtigercachesizegb). Undvik att öka den interna cachestorleken för WiredTiger över standardvärdet.

### NUMA {#numa}

NUMA (Non-Uniform Memory Access) gör att en kärna kan hantera hur minne mappas till processorkärnorna.

I ett NUMA-arkitekturminne är alltså anslutet till CPU:er och CPU:er är anslutna till en buss. I en SMP- eller UMA-arkitektur är minnet anslutet till bussen och delas av CPU:er. När en tråd allokerar minne på ett NUMA-CPU allokeras det enligt en princip. Standardinställningen är att minne som är kopplat till trådens lokala CPU ska allokeras, såvida det inte finns något ledigt utrymme. I så fall används minne från en kostnadsfri CPU till högre kostnad. När minnet har tilldelats flyttas det inte mellan CPU:er. Allokeringen utförs av en princip som ärvs från den överordnade tråden, vilket i slutändan är den tråd som startade processen.

Om du kör MongoDB på ett system med icke-Uniform Memory Access (NUMA) kan det orsaka ett antal driftproblem, bland annat långsamma prestanda under tidsperioder, oförmåga att använda allt tillgängligt RAM-minne och hög användning av systemprocesser.

Lösningen är att ändra NUMA-principen för huvudtråden som används för att starta `mongod`-processen genom att köra följande kommando:

```shell
numactl --interleaved=all <mongod> -f config
```

Denna policy tilldelar minne i en runda robat över alla CPU-noder, vilket ger en jämn fördelning över alla noder. Den ger inte tillgång till minne med högsta prestanda, som i system med flera CPU-maskinvara. Ungefär hälften av minnesåtgärderna är långsammare och ligger över bussen, men `mongod` har inte skrivits för att nå NUMA på ett optimalt sätt, så det är en rimlig kompromiss.

### NUMA-problem {#numa-issues}

Om processen `mongod` startas från en annan plats än mappen `/etc/init.d` är det troligt att den inte har startats med rätt NUMA-princip. Beroende på vilken standardprincip som används kan problem uppstå. Orsaken är att de olika installationsprogrammen för Linux® Package Manager för MongoDB även installerar en tjänst med konfigurationsfiler i `/etc/init.d` som utför det steg som beskrivs ovan. Om du installerar och kör MongoDB direkt från ett arkiv ( `.tar.gz`) måste du manuellt köra pengar under `numactl`-processen.

>[!NOTE]
>
>Mer information om tillgängliga NUMA-principer finns i den [numeriska dokumentationen](https://linux.die.net/man/8/numactl).

MongoDB-processen fungerar på olika sätt med olika allokeringsprinciper:

```

```

* `-membind=<nodes>`
Allokera bara på noderna i listan. Mongod allokerar inte minne på noder som visas och använder kanske inte allt tillgängligt minne.

* `-cpunodebind=<nodes>`
Kör bara på noderna. Mongod körs bara på de angivna noderna och använder bara minne som är tillgängligt på dessa noder.

* `-physcpubind=<nodes>`
Kör bara på de angivna processorerna (kärnor). Mongod körs bara på de angivna CPU:erna och använder bara minne som är tillgängligt på dessa CPU:er.

* `--localalloc`
Alltid allokera minne på den aktuella noden, men använd alla noder som tråden körs på. Om en tråd utför en allokering används endast det minne som är tillgängligt för den aktuella CPU.

* `--preferred=<node>`
Prioriterar allokering till en nod, men återgår till andra om den önskade noden är full. Relativ notation för att definiera en nod kan användas. Dessutom körs trådarna på alla noder.

Vissa principer kan resultera i att mindre än allt tillgängligt RAM-minne ges till processen `mongod`. Till skillnad från MySQL undviker MongoDB aktivt sidindelning på operativsystemnivå, och därför kan `mongod`-processen få mindre minne som verkar tillgängligt.

#### Byter {#swapping}

På grund av databasernas minneskrävande natur måste byte på operativsystemnivå inaktiveras. Med MongoDB-processen undviker du att byta design.

#### Fjärrfilsystem {#remote-filesystems}

Fjärrfilsystem som NFS rekommenderas inte för MongoDB:s interna datafiler (enkelprocessdatabasfiler) eftersom de orsakar för mycket latens. Blanda inte ihop med det delade filsystem som krävs för lagring av Oak Blob&#39;s (FileDataStore), där NFS rekommenderas.

#### Läs framåt {#read-ahead}

Trimma lästipset så att onödiga block inte läses från disken när en sida växlas in med en slumpmässig läsning. Sådana resultat innebär onödig användning av I/O-bandbredd.

### Linux®-krav {#linux-requirements}

#### Lägsta kernel-versioner {#minimum-kernel-versions}

* **2.6.23** för `ext4` filsystem

* **2.6.25** för `xfs` filsystem

#### Rekommenderade inställningar för databasdiskar {#recommended-settings-for-database-disks}

**Inaktivera tid**

Vi rekommenderar att `atime` inaktiveras för diskarna som innehåller databaserna.

**Ange NOOP-diskschemaläggaren**

Gör följande:

Kontrollera först i/O-schemaläggaren som är inställd genom att köra följande kommando:

```shell
cat /sys/block/sdg/queue/scheduler
```

Om svaret är `noop` finns det inget mer du måste göra.

Om NOOP inte är den inställda I/O-schemaläggaren kan du ändra den genom att köra:

```shell
echo noop > /sys/block/sdg/queue/scheduler
```

**Justera värdet för read ahead**

Vi rekommenderar att du använder värdet 32 för de diskar där MongoDB-databaser körs. Värdet är 16 kB. Du kan ställa in den genom att köra följande:

```shell
sudo blockdev --setra <value> <device>
```

#### Aktivera NTP {#enable-ntp}

Kontrollera att NTP är installerat och igång på datorn som är värd för MongoDB-databaserna. Du kan till exempel installera det med hjälp av Yum Package Manager på en CentOS-dator:

```shell
sudo yum install ntp
```

När NTP-daemon har installerats och startats kan du kontrollera om körningsfilen innehåller serverns tidsförskjutning.

#### Inaktivera genomskinliga stora sidor {#disable-transparent-huge-pages}

Red Hat® Linux® använder en minneshanteringsalgoritm som kallas för Transparent Huge Pages (THP). Vi rekommenderar att du inaktiverar det om du använder operativsystemet för databasarbetsbelastningar.

Du kan inaktivera det genom att följa nedanstående procedur:

1. Öppna filen `/etc/grub.conf` i valfri textredigerare.
1. Lägg till följande rad i filen grob.conf:

   ```xml
   transparent_hugepage=never
   ```

1. Kontrollera slutligen om inställningen har börjat gälla genom att köra:

   ```shell
   cat /sys/kernel/mm/redhat_transparent_hugepage/enabled
   ```

   Om THP är inaktiverat bör utdata för ovanstående kommando vara:

   ```xml
   always madvise [never]
   ```

>[!NOTE]
>
>Mer information om genomskinliga stora sidor finns i följande artikel på Red Hat®-kundportalen: [Så här använder, övervakar och inaktiverar du genomskinliga sidor i Red Hat Enterprise Linux 6,7 och 8?](https://access.redhat.com/solutions/46111).

#### Inaktivera NUMA {#disable-numa}

I de flesta installationer där NUMA är aktiverat inaktiveras det automatiskt av MongoDB daemon om det körs som en tjänst från mappen `/etc/init.d`.

Om så inte är fallet kan du inaktivera NUMA per processnivå. Om du vill inaktivera det kör du följande kommandon:

```shell
numactl --interleave=all <path_to_process>
```

Där `<path_to_process>` är sökvägen till monteringsprocessen.

Inaktivera sedan zonåteranvändning genom att köra:

```shell
echo 0 > /proc/sys/vm/zone_reclaim_mode
```

#### Justera de högsta tillåtna inställningarna för monteringsprocessen {#tweak-the-ulimit-settings-for-the-mongod-process}

Linux® ger konfigurerbar kontroll över resursallokeringen med hjälp av kommandot `ulimit`. Den här konfigurationen kan göras per användare eller process.

Vi rekommenderar att du konfigurerar en övre gräns för montageprocessen enligt [MongoDB Recommended Ulimit Settings](https://docs.mongodb.org/manual/reference/ulimit/#recommended-ulimit-settings).

#### Testa MongoDB I/O-prestanda {#test-mongodb-i-o-performance}

MongoDB har ett verktyg med namnet `mongoperf` som är utformat för att testa I/O-prestanda. Vi rekommenderar att du använder den för att testa prestanda för alla dina MongoDB-instanser som utgör din infrastruktur.

Mer information om hur du använder `mongoperf` finns i [MongoDB-dokumentationen](https://docs.mongodb.org/manual/reference/program/mongoperf/).

>[!NOTE]
>
>`mongoperf` är en indikator på MongoDB-prestanda på plattformen som den körs på. Resultatet bör därför inte betraktas som slutgiltigt när det gäller ett produktionssystems prestanda.
>
>Om du vill få mer exakta prestandasultat kan du köra kompletterande tester med `fio` Linux®-verktyget.

**Testa läsprestanda på de virtuella datorer som ingår i distributionen**

När du har installerat verktyget växlar du till databaskatalogen MongoDB för att köra testerna. Starta sedan det första testet genom att köra `mongoperf`med den här konfigurationen:

```shell
echo "{nThreads:32,fileSizeMB:1000,r:true}" | mongoperf
```

De önskade utdata bör vara upp till två gigabyte per sekund (2 GB/s) och 500 000 IOPS med 32 trådar för alla MongoDB-instanser.

Kör ett andra test, den här gången med minnesmappade filer, genom att ange parametern `mmf:true`:

```shell
echo "{nThreads:32,fileSizeMB:1000,r:true,mmf:true}" | mongoperf
```

Utdata från det andra testet bör vara betydligt högre än det första, vilket indikerar minnesöverföringsprestanda.

>[!NOTE]
>
>När du utför testerna ska du kontrollera I/O-användningsstatistik för de virtuella datorerna i operativsystemets övervakningssystem. Om de anger värden som är lägre än 100 procent för I/O-läsningar kan det vara problem med din virtuella dator.

**Testa skrivprestanda för den primära MongoDB-instansen**

Kontrollera sedan I/O-skrivprestanda för den primära MongoDB-instansen genom att köra `mongoperf` från MongoDB-databaskatalogen med samma inställningar:

```shell
echo "{nThreads:32,fileSizeMB:1000,w:true}" | mongoperf
```

Önskat utvärde bör vara 12 megabyte per sekund och nå cirka 3 000 IOPS, med liten variation mellan antalet trådar.

## Steg för virtualiserade miljöer {#steps-for-virtualised-environments}

### VMWare {#vmware}

Om du använder WMWare ESX för att hantera och driftsätta dina virtualiserade miljöer måste du utföra följande inställningar från ESX-konsolen för att kunna utföra MongoDB-åtgärden:

1. Stäng av minnesballongfunktion
1. Förallokera och reservera minne för de virtuella datorer som är värdar för MongoDB-databaser
1. Använd I/O-kontroll för lagring för att tilldela tillräcklig I/O till `mongod`-processen.
1. Garantera CPU-resurserna på datorerna som är värdar för MongoDB genom att ställa in [CPU Reservation](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.hostclient.doc/GUID-6C9023B2-3A8F-48EB-8A36-44E3D14958F6.html?hWord=N4IghgNiBc4RB7AxmALgUwAQGEAKBVTAJ3QGcEBXIpMkAXyA)

1. Överväg att använda ParaVirtual I/O-drivrutiner. <!-- URL is a 404 See [knowledgebase article](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1010398).-->

### Amazon Web Services {#amazon-web-services}

Mer information om hur du konfigurerar MongoDB med Amazon Web Services finns i artikeln [Konfigurera AWS-integrering](https://www.mongodb.com/docs/cloud-manager/tutorial/configure-aws-integration/) på MongoDB-webbplatsen.

## Säkra MongoDB före distribution {#securing-mongodb-before-deployment}

Se det här inlägget på [distribuerar MongoDB](https://blogs.adobe.com/security/2015/07/securely-deploying-mongodb-3-0.html) säkert för råd om hur du skyddar konfigurationen av dina databaser före distributionen.

## Dispatcher {#dispatcher}

### Välja operativsystem för Dispatcher {#choosing-the-operating-system-for-the-dispatcher}

För att din MongoDB-distribution ska fungera på rätt sätt måste operativsystemet som är värd för Dispatcher köra **Apache httpd** **version 2.4 eller senare.**

Se även till att alla bibliotek som används i ditt bygge är uppdaterade för att minimera säkerhetsriskerna.

### Dispatcher Configuration {#dispatcher-configuration}

En typisk Dispatcher-konfiguration fungerar mellan tio och 20 gånger så mycket som genomströmningen för en enda AEM-instans.

Eftersom Dispatcher är tillståndslöst kan det enkelt skalas vågrätt. I vissa distributioner måste författare hindras från att komma åt vissa resurser. Vi rekommenderar att du använder en Dispatcher med författarinstanserna.

Om du kör AEM utan Dispatcher måste SSL-avslutning och belastningsutjämning utföras av ett annat program. Det är obligatoriskt eftersom sessioner måste ha tillhörighet till den AEM-instans som de skapas i, ett koncept som kallas klibbiga anslutningar. Orsaken är att uppdateringarna av innehållet har minimal fördröjning.

Läs [Dispatcher-dokumentationen](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher) om du vill ha mer information om hur du konfigurerar den.

### Ytterligare konfiguration {#additional-configuration}

#### Fästiga anslutningar {#sticky-connections}

Med häftiga anslutningar säkerställs att personaliserade sidor och sessionsdata för en användare sammanställs på samma instans av AEM. Dessa data lagras på instansen, så efterföljande begäranden från samma användare återgår till samma instans.

Vi rekommenderar att klisterlappande anslutningar aktiveras för alla inre lagerroutningsbegäranden till AEM-instanser, vilket uppmuntrar efterföljande begäranden att nå samma AEM-instans. Om du gör det minimeras fördröjningen som annars syns när innehållet uppdateras mellan olika instanser.

#### Långt förfallodatum {#long-expires}

Som standard har innehåll som skickas ut från en AEM Dispatcher rubriker Senaste ändring och Etag utan någon indikation på att innehållet har upphört att gälla. Det här flödet ser till att användargränssnittet alltid får den senaste versionen av resursen. Det innebär också att webbläsaren utför en GET-åtgärd för att se om resursen har ändrats. Det kan därför resultera i flera begäranden som HTTP-svaret är 304 (inte ändrat), beroende på sidinläsningen. Om du anger ett förfallodatum för resurser och tar bort rubrikerna Last-Modified och ETag, kommer innehållet att cachelagras. Inga fler uppdateringsbegäranden görs förrän datumet i huvudet Förfaller är uppfyllt.

Men om du använder den här metoden finns det inget rimligt sätt att låta resursen förfalla i webbläsaren innan rubriken Förfaller förfaller. För att minimera arbetsflödet kan HtmlClientLibraryManager konfigureras att använda oföränderliga URL:er för klientbibliotek.

Dessa URL:er ändras garanterat inte. När innehållet i resursen som finns i URL-adressen ändras, återspeglas ändringarna i URL-adressen så att webbläsaren begär rätt version av resursen.

Standardkonfigurationen lägger till en väljare i HtmlClientLibraryManager. Resursen är en väljare och cachas i Dispatcher med väljaren intakt. Den här väljaren kan också användas för att säkerställa korrekt förfallobeteende. Standardväljaren följer mönstret `lc-.*?-lc`. Följande httpd-konfigurationsdirektiv för Apache säkerställer att alla begäranden som matchar mönstret hanteras med lämplig förfallotid.

```xml
Header set Expires "Tue, 20 Jan 2037 04:20:42 GMT" "expr=(%{REQUEST_STATUS} -eq 200) && (%{REQUEST_URI} =~ /.*lc-.*?-lc.*/)"
Header set Cache-Control "public, no-transform, max-age=267840000" "expr=(%{REQUEST_STATUS} -eq 200) && (%{REQUEST_URI} =~ /.*lc-.*?-lc.*/)"
Header unset ETag "expr=(%{REQUEST_STATUS} -eq 200) && (%{REQUEST_URI} =~ /.*lc-.*?-lc.*/)"
Header unset Last-Modified "expr=(%{REQUEST_STATUS} -eq 200) && (%{REQUEST_URI} =~ /.*lc-.*?-lc.*/)"
Header unset Pragma "expr=(%{REQUEST_STATUS} -eq 200) && (%{REQUEST_URI} =~ /.*lc-.*?-lc.*/)"
```

#### Ingen sniff {#no-sniff}

Där innehåll skickas ut utan innehållstyp försöker många webbläsare att gissa vilken typ av innehåll det är genom att läsa de första byten i innehållet. Den här metoden kallas för &quot;sniffing&quot;. Sniffing öppnar en säkerhetsrisk eftersom användare som kan skriva till databasen kan överföra skadligt innehåll utan innehållstyp.

Därför är det tillrådligt att lägga till en `no-sniff`-rubrik till resurser som hanteras av Dispatcher. Dispatcher cache-lagrar emellertid inte rubriker. Det innebär alltså att innehåll som hanteras från det lokala filsystemet har innehållstypen som bestäms av dess tillägg, i stället för att den ursprungliga innehållstyprubriken från den ursprungliga AEM-servern används.

Inga kodavsnitt kan aktiveras på ett säkert sätt om webbprogrammet är känt för att aldrig hantera cachelagrade resurser utan filtyp.

Du kan aktivera Ingen signatur:

```xml
Header set X-Content-Type-Options "nosniff"
```

Den kan också aktiveras selektivt:

```xml
RewriteCond %{REQUEST_URI} \.(?:js|jsonp)$ [OR]
RewriteCond %{QUERY_STRING} (callback|jsonp|cb)=\w+
RewriteRule .* - [E=jsonp_request:1]
Header set X-Content-Type-Options "nosniff"  env=jsonp_request
Header setifempty Content-Type application/javascript env=jsonp_request
```

#### Skyddsprincip för innehåll {#content-security-policy}

Dispatcher standardinställningar tillåter en öppen säkerhetsprincip för innehåll, som även kallas CSP. Med de här inställningarna kan en sida läsa in resurser från alla domäner som omfattas av standardreglerna i webbläsarsandlådan.

Det är önskvärt att begränsa varifrån resurser kan läsas in för att undvika att läsa in kod till JavaScript-motorn från otillförlitliga eller overifierade externa servrar.

Med CSP kan du finjustera principer. I ett komplext program måste emellertid CSP-huvuden utvecklas med försiktighet eftersom för restriktiva profiler kan bryta delar av användargränssnittet.

>[!NOTE]
>
>Mer information om hur det här fungerar finns på [OWASP-sidan om skyddsprofiler för innehåll](https://owasp.deteact.com/cheat/cheatsheets/Content_Security_Policy_Cheat_Sheet.html).

### Storleksändring {#sizing}

Mer information om storleksändring finns i [Riktlinjerna för maskinvarustorlek](/help/managing/hardware-sizing-guidelines.md).

### Prestandaoptimering för MongoDB {#mongodb-performance-optimization}

Mer allmän information om MongoDB-prestanda finns i [Analyserar MongoDB-prestanda](https://docs.mongodb.org/manual/administration/analyzing-mongodb-performance/).

## Kända begränsningar {#known-limitations}

### Samtidiga installationer {#concurrent-installations}

MongoMK stöder samtidig användning av flera AEM-instanser med en databas, men inte samtidiga installationer.

För att lösa problemet måste du först köra installationen med en enda medlem och lägga till de andra efter att den första installationen är klar.

>[!NOTE]
>
>Se [MongoDB-dokumentationen](https://docs.mongodb.com/manual/reference/limits/) så att du kan bekanta dig med de kända begränsningarna och tröskelvärdena i MongoDB.
