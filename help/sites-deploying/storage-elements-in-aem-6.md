---
title: Lagringselement i AEM 6.5 LTS
description: Lär dig mer om nodlagringsimplementationer i AEM 6.5 LTS och hur du underhåller databasen.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
legacypath: /content/docs/en/aem/6-0/deploy/upgrade/microkernels-in-aem-6-0
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
exl-id: e51842b5-fa91-42d2-a490-5a7e867dada7
source-git-commit: 0e60c406a9cf1e5fd13ddc09fd85d2a2f8a410f6
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 0%

---

# Lagringselement i AEM 6.5 LTS{#storage-elements-in-aem}

Denna artikel omfattar följande:

* [Översikt över lagring i AEM 6.5 LTS](/help/sites-deploying/storage-elements-in-aem-6.md#overview-of-storage-in-aem)
* [Underhålla databasen](/help/sites-deploying/storage-elements-in-aem-6.md#maintaining-the-repository)

## Översikt över lagring i AEM 6.5 LTS {#overview-of-storage-in-aem}

En av de viktigaste förändringarna i AEM 6.5 LTS är innovationerna på databasnivå.

För närvarande finns det två nodlagringsimplementationer i AEM 6.5 LTS: Tjärlagring och MongoDB-lagring.

### Tjärlagring {#tar-storage}

#### Köra en nyinstallerad AEM-instans med TAR Storage {#running-a-freshly-installed-aem-instance-with-tar-storage}

Som standard använder AEM 6.5 LTS-lagring för att lagra noder och binära filer med hjälp av standardkonfigurationsalternativen. Du kan konfigurera lagringsinställningarna manuellt genom att göra följande:

1. Ladda ned AEM 6.5 LTS quickstart jar och placera den i en ny mapp.
1. Packa upp AEM genom att köra:

   `java -jar <aem-65-lts>.jar -unpack`

1. Skapa en mapp med namnet `crx-quickstart\install` i installationskatalogen.

1. Skapa en fil med namnet `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config` i den nyligen skapade mappen.

1. Redigera filen och ange konfigurationsalternativ. Följande alternativ är tillgängliga för Segment Node Store, som är grunden för AEM Tjära-lagringsimplementering:

   * `repository.home`: Sökväg till databasstartplats där olika databasrelaterade data lagras. Som standard lagras segmentfiler under katalogen crx-quickstart/segmentstore.
   * `tarmk.size`: Maximal storlek för ett segment i MB. Standardvärdet är 256 MB.

1. Starta AEM.

### Mongo-lagring {#mongo-storage}

>[!NOTE]
>
>Den lägsta version av Mongo som stöds är Mongo 6.

#### Köra en nyinstallerad AEM-instans med Mongo Storage {#running-a-freshly-installed-aem-instance-with-mongo-storage}

AEM 6.5 LTS kan konfigureras att köras med MongoDB-lagring enligt följande procedur:

1. Ladda ned AEM 6.5 LTS quickstart jar och placera den i en ny mapp.
1. Packa upp AEM genom att köra följande kommando:

   `java -jar <aem-65-lts>.jar -unpack`

1. Kontrollera att MongoDB är installerat och att en instans av `mongod` körs. Mer information finns i [Installera MongoDB](https://docs.mongodb.org/manual/installation/).
1. Skapa en mapp med namnet `crx-quickstart\install` i installationskatalogen.
1. Konfigurera nodarkivet genom att skapa en konfigurationsfil med namnet på konfigurationen som du vill använda i katalogen `crx-quickstart\install`.

   I Document Node Store (som är grunden för AEM mongoDB-lagringsimplementering) används en fil med namnet `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`

1. Redigera filen och ange konfigurationsalternativ. Följande alternativ är tillgängliga:

   * `mongouri`: [MongoURI](https://docs.mongodb.org/manual/reference/connection-string/) som krävs för att ansluta till Mongo-databasen. Standardvärdet är `mongodb://localhost:27017`
   * `db`: Namnet på Mongo-databasen. Som standard använder nya AEM 6.5 LTS-installationer **aem-author** som databasnamn.
   * `cache`: Cachestorleken i MB. Cachestorleken fördelas mellan olika cacheminnen som används i DocumentNodeStore. Standardvärdet är 256.
   * `changesSize`: Storlek i MB på den skyddade samlingen som används i Mongo för att cachelagra diff-utdata. Standardvärdet är 256.
   * `customBlobStore`: Ett booleskt värde som anger att ett anpassat datalager används. Standardvärdet är false.

1. Skapa en konfigurationsfil med PID för det datalager som du vill använda och redigera filen för att ange konfigurationsalternativen. Mer information finns i [Konfigurera nodarkiv och datalager](/help/sites-deploying/data-store-config.md).

1. Starta AEM 6.5 LTS jar med en MongoDB-lagringsserver genom att köra:

   ```shell
   java -jar <aem-65-lts>.jar -r crx3,crx3mongo
   ```

   När serverdelens körningsläge är **`-r`** börjar exemplet med stöd för MongoDB.

#### Inaktivera genomskinliga stora sidor {#disabling-transparent-huge-pages}

Red Hat® Linux® använder en minneshanteringsalgoritm som kallas för Transparent Huge Pages (THP). AEM utför finkorniga läsningar och skrivningar, men THP är optimerat för stora operationer. Därför rekommenderar vi att du inaktiverar THP för både Tjärs- och Mongo-lagring. Så här inaktiverar du algoritmen:

1. Öppna filen `/etc/grub.conf` i valfri textredigerare.
1. Lägg till följande rad i filen **grub.conf**:

   ```
   transparent_hugepage=never
   ```

1. Kontrollera slutligen om inställningen har börjat gälla genom att köra:

   ```
   cat /sys/kernel/mm/redhat_transparent_hugepage/enabled
   ```

   Om THP är inaktiverat bör utdata för ovanstående kommando vara:

   ```
   always madvise [never]
   ```

>[!NOTE]
>
>Se följande resurser:
>
>* Mer information om genomskinliga stora sidor på Red Hat® Linux® finns i följande artikel på Red Hat®-kundportalen: [Så här använder, övervakar och inaktiverar du genomskinliga sidor i Red Hat Enterprise Linux 6,7 och 8?](https://access.redhat.com/solutions/46111)
>* Inställningstips för Linux® finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).
>

## Underhålla databasen {#maintaining-the-repository}

Varje uppdatering av databasen skapar en innehållsändring. Det innebär att databasstorleken ökar för varje uppdatering. För att undvika okontrollerad databastillväxt måste äldre versioner rensas för att frigöra diskutrymme. Den här underhållsfunktionen kallas Revision Cleanup. Revision Cleanup-funktionen frigör diskutrymme genom att ta bort föråldrade data från databasen. Mer information om Revision Cleanup finns på sidan [Revision Cleanup](/help/sites-deploying/revision-cleanup.md).
