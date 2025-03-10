---
title: Konfigurera nodarkiv och datalager i AEM 6
description: Lär dig hur du konfigurerar nodarkiv och datalager och hur du utför skräpinsamling i datalager.
content-type: reference
topic-tags: deploying
docset: aem65
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: 69d94737-41d0-47bb-b914-f7606becd038
source-git-commit: f145e5f0d70662aa2cbe6c8c09795ba112e896ea
workflow-type: tm+mt
source-wordcount: '3461'
ht-degree: 0%

---

# Konfigurera nodarkiv och datalager i AEM 6{#configuring-node-stores-and-data-stores-in-aem}

## Introduktion {#introduction}

I Adobe Experience Manager (AEM) kan binära data lagras oberoende av innehållsnoderna. Binära data lagras i ett datalager, medan innehållsnoder lagras i ett nodarkiv.

Både datalager och nodarkiv kan konfigureras med OSGi-konfiguration. Varje OSGi-konfiguration refereras med en beständig identifierare (PID).

## Konfigurationssteg {#configuration-steps}

Så här konfigurerar du både nodarkivet och datalagret:

1. Kopiera AEM snabbstart-JAR-filen till installationskatalogen.
1. Skapa en mapp `crx-quickstart/install` i installationskatalogen.
1. Konfigurera först nodarkivet genom att skapa en konfigurationsfil med namnet på det nodarkivalternativ som du vill använda i katalogen `crx-quickstart/install`.

   Exempelvis används filen `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config` i dokumentnodarkivet (som är grunden för AEM MongoMK-implementering).

1. Redigera filen och ange konfigurationsalternativ.
1. Skapa en konfigurationsfil med PID:t för det datalager som du vill använda. Redigera filen för att ange konfigurationsalternativ.

   >[!NOTE]
   >
   >Mer information om konfigurationsalternativ finns i [Konfigurationer för nodarkivet](#node-store-configurations) och [Konfigurationer för datalagret](#data-store-configurations).

1. Starta AEM.

## Konfigurationer för nodarkivet {#node-store-configurations}

>[!CAUTION]
>
>Nyare versioner av Oak har ett nytt namngivningsschema och format för OSGi-konfigurationsfiler. Det nya namnschemat kräver att konfigurationsfilen får namnet **.config** och det nya formatet kräver att värden skrivs. Mer information finns i [Provisioning Model för Apache Sling och Apache SlingStart - Standardkonfigurationsformat](https://sling.apache.org/documentation/development/slingstart.html#default-configuration-format).
>
>Om du uppgraderar från en äldre version av Oak måste du först säkerhetskopiera mappen `crx-quickstart/install`. Efter uppgraderingen återställer du innehållet i mappen till den uppgraderade installationen och ändrar tillägget för konfigurationsfilerna från **.cfg** till **.config**.

### Segmentnodarkiv {#segment-node-store}

Segmentnodbutiken är grunden för Adobe TjärMK-implementering i AEM6. Det använder `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService`-PID:t för konfiguration.

>[!CAUTION]
>
>PID för segmentnodarkivet har ändrats från `org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService in previous versions` av AEM 6 till `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService` i AEM 6.3. Se till att du gör de nödvändiga konfigurationsjusteringarna för att återspegla den här ändringen.

Du kan konfigurera följande alternativ:

* `repository.home`: Sökväg till databasstartplats där databasrelaterade data lagras. Segmentfiler lagras som standard i katalogen `crx-quickstart/segmentstore`.

* `tarmk.size`: Maximal storlek för ett segment i MB. Standardmaxstorleken är 256 MB.
* `customBlobStore`: Ett booleskt värde som anger att ett anpassat datalager används. Standardvärdet är true för AEM 6.3 och senare versioner. Före AEM 6.3 var standardvärdet false.

Följande är ett exempel på en `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`-fil:

```shell
#Path to repo
repository.home="crx-quickstart/repository"

#Max segment size
tarmk.size=I"256"

#Custom data store
customBlobStore=B"true"
```

#### Dokumentnodarkiv {#document-node-store}

Dokumentnodarkivet är grunden för AEM MongoMK-implementering. Den använder `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService`* *PID. Följande konfigurationsalternativ är tillgängliga:

* `mongouri`: [MongoURI](https://docs.mongodb.org/manual/reference/connection-string/) som krävs för att ansluta till Mongo-databasen. Standardvärdet är `mongodb://localhost:27017`

* `db`: Namnet på Mongo-databasen. Standardvärdet är **Oak** ``. However, new AEM 6 installations use **aem-author** `` som standarddatabasnamn.

* `cache`: Cachestorleken i MB. Detta fördelas mellan olika cacheminnen som används i DocumentNodeStore. Standardvärdet är `256`

* `changesSize`: Storlek i MB på den skyddade samlingen som används i Mongo för att cachelagra diff-utdata. Standardvärdet är `256`

* `customBlobStore`: Ett booleskt värde som anger att ett anpassat datalager används. Standardvärdet är `false`.

Följande är ett exempel på en `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`-fil:

```shell
#Mongo server details
mongouri="mongodb://localhost:27017"

#Name of Mongo database to use
db="aem-author"

#Store binaries in custom BlobStore
customBlobStore=B"false"
```

## Konfigurationer för datalager {#data-store-configurations}

När du hanterar ett stort antal binära filer bör du använda ett externt datalager i stället för standardnodarkiven för att maximera prestandan.

Om ditt projekt till exempel kräver många medieresurser och du lagrar dem i File- eller S3-datalagret blir det snabbare att komma åt dem än att lagra dem direkt i en MongoDB.

Fildatalagret ger bättre prestanda än MongoDB, och säkerhetskopierings- och återställningsåtgärderna i Mongo är också långsammare med ett stort antal resurser.

Information om olika datalager och konfigurationer beskrivs nedan.

>[!NOTE]
>
>Om du vill aktivera anpassade datalager måste du se till att `customBlobStore` är inställt på `true` i respektive konfigurationsfil för nodarkivet ([segmentnodarkiv](/help/sites-deploying/data-store-config.md#segment-node-store) eller [dokumentnodarkiv](/help/sites-deploying/data-store-config.md#document-node-store)).

### Fildatalager {#file-data-store}

Detta är implementeringen av [FileDataStore](https://jackrabbit.apache.org/api/trunk/org/apache/jackrabbit/core/data/FileDataStore.html) som finns i Jackrabbit 2. Det är ett sätt att lagra binära data som normala filer i filsystemet. Den använder `org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore`-PID.

Dessa konfigurationsalternativ är tillgängliga:

* `repository.home`: Sökväg till databasstartplats där olika databasrelaterade data lagras. Som standard lagras binära filer i katalogen `crx-quickstart/repository/datastore`

* `path`: Sökväg till katalogen som filerna ska lagras i. Om den anges har den företräde framför värdet `repository.home`

* `minRecordLength`: Den minsta storleken i byte för en fil som lagras i datalagret. Binärt innehåll som är mindre än det här värdet infogas.

>[!NOTE]
>
>När du använder en NAS för att lagra delade fildatalager bör du kontrollera att du bara använder högpresterande enheter för att undvika prestandaproblem.

## Amazon S3 - datalager {#amazon-s-data-store}

AEM kan konfigureras för att lagra data i Amazon Simple Storage Service (S3). Det använder `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config`-PID:t för konfiguration.

>[!NOTE]
>
>AEM 6.5 har stöd för datalagring i Amazon CS3, men det finns inte stöd för datalagring på andra plattformar, vars leverantörer kan ha egna implementeringar av Amazon S3-API:er.

Om du vill aktivera S3-datalagringsfunktionen måste ett funktionspaket som innehåller S3 Datastore Connector hämtas och installeras. Gå till [Adobe-databasen](https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.oak.s3connector/) och hämta den senaste versionen från 1.10.x-versionerna av funktionspaketet (till exempel com.adobe.granite.oak.s3connector-1.10.0.zip). Du måste även hämta och installera den senaste AEM Service Pack-versionen som finns på sidan [AEM 6.5 Release Notes](/help/release-notes/release-notes.md) .

>[!NOTE]
>
>När du använder AEM med tarMK lagras binärfiler som standard i `FileDataStore`. Om du vill använda tarMK med S3-datastore måste du starta AEM med runmode `crx3tar-nofds`, till exempel:

```shell
java -jar <aem-jar-file>.jar -r crx3tar-nofds
```

När du har laddat ned den kan du installera och konfigurera S3 Connector på följande sätt:

1. Extrahera innehållet i ZIP-filen för funktionspaketet till en tillfällig mapp.

1. Gå till den tillfälliga mappen och navigera till följande plats:

   ```xml
   jcr_root/libs/system/install
   ```

   Kopiera allt innehåll från ovanstående plats till `<aem-install>/crx-quickstart/install.`

1. Om AEM redan har konfigurerats för att fungera med lagringen tar du bort alla befintliga konfigurationsfiler från mappen ***&lt;aem-install>***/*crx-quickstart*/*install* innan du fortsätter. De filer som måste tas bort är:

   * `For MongoMK: org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`
   * `For TarMK: org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`

1. Återgå till den tillfälliga platsen där funktionspaketet har extraherats och kopiera innehållet i följande mapp:

   * `jcr_root/libs/system/config`

   till

   * `<aem-install>/crx-quickstart/install`

   Kontrollera att du bara kopierar de konfigurationsfiler som behövs för den aktuella konfigurationen. Kopiera filen `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` för både ett dedikerat datalager och en delad datalagerkonfiguration.

   >[!NOTE]
   >
   >Utför ovanstående steg på alla noder i klustret en i taget i en klusterkonfiguration. Se även till att använda samma S3-inställningar för alla noder.

1. Redigera filen och lägg till de konfigurationsalternativ som krävs för installationen.
1. Starta AEM.

## Uppgradera till en ny version av 1.10.x S3 Connector {#upgrading-to-a-new-version-of-the-s-connector}

Så här uppgraderar du till en ny version av 1.10.x S3-kontakten (till exempel från 1.10.0 till 1.10.4):

1. Stoppa AEM-instansen.

1. Navigera till `<aem-install>/crx-quickstart/install/15` i installationsmappen för AEM och gör en säkerhetskopia av dess innehåll.
1. Efter säkerhetskopieringen tar du bort den gamla versionen av S3 Connector och dess beroenden genom att ta bort alla jar-filer i mappen `<aem-install>/crx-quickstart/install/15`, till exempel:

   * **oak-blob-cloud-1.6.1.jar**
   * **aws-java-sdk-osgi-1.10.76.jar**

   >[!NOTE]
   >
   >Filnamnen ovan används endast som illustrationer.

1. Hämta den senaste versionen av funktionspaketet 1.10.x från [Adobe Repository](https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.oak.s3connector/).
1. Zippa upp innehållet i en separat mapp och navigera sedan till `jcr_root/libs/system/install/15`.
1. Kopiera jar-filerna till **&lt;aem-install>**/crx-quickstart/install/15 i AEM installationsmapp.
1. Starta AEM och kontrollera anslutningsfunktionen.

Du kan använda konfigurationsfilen med alternativen nedan.

<!--
* accessKey: The AWS access key.
* secretKey: The AWS secret access key. **Note:** When the `accessKey` or `secretKey` is not specified then the [IAM role](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-roles.html) is used for authentication.
* s3Bucket: The bucket name.
* s3Region: The bucket region.
* path: The path of the data store. The default is **&lt;AEM install folder&gt;/repository/datastore**
* minRecordLength: The minimum size of an object that should be stored in the data store. The minimum/default is **16KB.**
* maxCachedBinarySize: Binaries with size less than or equal to this size will be stored in memory cache. The size is in bytes. The default is **17408 **(17 KB).
* cacheSize: The size of the cache. The value is specified in bytes. The default is **64GB**.
* secret: Only to be used if using binaryless replication for shared datastore setup.
* stagingSplitPercentage: The percentage of cache size configured to be used for staging asynchronous uploads. The default value is **10**.
* uploadThreads: The number of uploads threads that are used for asynchronous uploads. The default value is **10**.
* stagingPurgeInterval: The interval in seconds for purging finished uploads from the staging cache. The default value is **300** seconds (5 minutes).
* stagingRetryInterval: The retry interval in seconds for failed uploads. The default value is **600** seconds (10 minutes).
-->

### Alternativ för konfigurationsfil för S3 Connector {#s3-connector-configuration-file-options}

>[!NOTE]
>
>S3-anslutningen stöder både IAM-användarautentisering och IAM-rollautentisering. Om du vill använda IAM-rollautentisering utelämnar du värdena `accessKey` och `secretKey` från konfigurationsfilen. S3-kopplingen kommer sedan att använda den [IAM-roll](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-roles.html) som tilldelats instansen som standard.

| Nyckel | Beskrivning | Standard | Obligatoriskt |
| --- | --- | --- | --- |
| accessKey | Åtkomstnyckel-ID för IAM-användaren med åtkomst till bucket. | | Ja, när IAM-roller inte används. |
| secretsKey | Hemlig åtkomstnyckel för IAM-användaren med åtkomst till bucket. | | Ja, när IAM-roller inte används. |
| cacheSize | Den lokala cachens storlek (i byte). | 64 GB | Nej. |
| connectionTimeout | Ange väntetiden (i millisekunder) före timeout när anslutningen upprättas första gången. | 10000 | Nej. |
| maxCachedBinarySize | Binärfiler som är mindre än eller lika med det här värdet (i byte) lagras i minnescachen. | 17408 (17 kB) | Nej. |
| maxConnections | Ange maximalt antal tillåtna öppna HTTP-anslutningar. | 50 | Nej. |
| maxErrorRetry | Ange det maximala antalet försök för misslyckade (hämtningsbara) begäranden. | 3 | Nej. |
| minRecordLength | Den minsta storleken för ett objekt (i byte) som ska lagras i datalagret. | 16384 | Nej. |
| bana | Den lokala sökvägen för AEM-datalagret. | `crx-quickstart/repository/datastore` | Nej. |
| proxyHost | Ange den valfria proxyvärd som klienten ansluter via. | | Nej. |
| proxyPort | Ange den valfria proxyport som klienten ansluter via. | | Nej. |
| s3Bucket | Namn på S3-bucket. | | Ja |
| s3EndPoint | S3 REST API-slutpunkt. | | Nej. |
| s3Region | Region där bucket finns. Mer information finns på [sidan](https://docs.aws.amazon.com/general/latest/gr/s3.html). | Region där AWS-instansen körs. | Nej. |
| socketTimeout | Ställ in väntetiden (i millisekunder) för data som ska överföras via en etablerad, öppen anslutning innan anslutningens timeout inträffar och stängs. | 50000 | Nej. |
| stagingPurgeInterval | Intervallet (i sekunder) för tömning av slutförda överföringar från mellanlagringscachen. | 300 | Nej. |
| stagingRetryInterval | Intervallet (i sekunder) för återförsök av misslyckade överföringar. | 600 | Nej. |
| stagingSplitPercentage | Procentandelen `cacheSize` som ska användas för att mellanlagra asynkrona överföringar. | 10 | Nej. |
| uploadThreads | Antalet överföringstrådar som används för asynkrona överföringar. | 10 | Nej. |
| writeThreads | Antalet samtidiga trådar som används för att skriva via S3 Transfer Manager. | 10 | Nej. |

<!---
### Bucket region options {#bucket-region-options}

<table>
 <tbody>
  <tr>
   <td>US Standard</td>
   <td><code>us-standard</code></td>
  </tr>
  <tr>
   <td>US West</td>
   <td><code>us-west-2</code></td>
  </tr>
  <tr>
   <td>US West (Northern California)</td>
   <td><code>us-west-1</code></td>
  </tr>
  <tr>
   <td>EU (Ireland)<br /> </td>
   <td><code>EU</code></td>
  </tr>
  <tr>
   <td>Asia Pacific (Singapore)<br /> </td>
   <td><code>ap-southeast-1</code></td>
  </tr>
  <tr>
   <td>Asia Pacific (Sydney)<br /> </td>
   <td><code>ap-southeast-2</code></td>
  </tr>
  <tr>
   <td>Asia Pacific (Tokyo)</td>
   <td><code>ap-northeast-1</code></td>
  </tr>
  <tr>
   <td>South America (Sao Paolo)<br /> </td>
   <td><code>sa-east-1</code></td>
  </tr>
 </tbody>
</table>
-->

### DataStore-cachelagring {#data-store-caching}

>[!NOTE]
>
>DataStore-implementeringarna av `S3DataStore`, `CachingFileDataStore` och `AzureDataStore` stöder cachelagring av lokala filsystem. Implementeringen av `CachingFileDataStore` är användbar när DataStore är på NFS (Network File System).

När du uppgraderar från en äldre cacheimplementering (före Oak 1.6) finns det en skillnad i strukturen för det lokala filsystemets cachekatalog. I den gamla cachestrukturen placerades både de hämtade och de överförda filerna direkt under cachesökvägen. Den nya strukturen delar upp hämtningarna och överföringarna och lagrar dem i två kataloger som heter `upload` och `download` under cachesökvägen. Uppgraderingsprocessen bör vara sömlös och alla väntande överföringar bör schemaläggas för överföring och alla tidigare hämtade filer i cachen läggs i cachen vid initieringen.

Du kan även uppgradera cachen offline med kommandot `datastorecacheupgrade` i ekupen. Mer information om hur du kör kommandot finns i [Viktigt](https://svn.apache.org/repos/asf/jackrabbit/oak/trunk/oak-run/README.md) för ekrun-modulen.

Cachen har en storleksgräns och kan konfigureras med parametern cacheSize.

#### Nedladdningar {#downloads}

Den lokala cachen kontrolleras för posten för den begärda filen/blobben innan den hämtas från DataStore. När cacheminnet överskrider den konfigurerade gränsen (se parametern `cacheSize`) när en fil läggs till i cacheminnet, kommer vissa av filerna att frigöra utrymme.

#### Asynkron överföring {#async-upload}

Cachen stöder asynkrona överföringar till DataStore. Filerna mellanlagras lokalt i cachen (i filsystemet) och ett asynkront jobb börjar överföra filen. Antalet asynkrona överföringar begränsas av mellanlagringscachens storlek. Mellanlagringscachens storlek konfigureras med parametern `stagingSplitPercentage`. Den här parametern definierar den procentandel av cachestorleken som ska användas för mellanlagringscachen. Procentandelen cache som är tillgänglig för nedladdningar beräknas också som **(100 - `stagingSplitPercentage`) &#42;`cacheSize`**.

De asynkrona överföringarna är flertrådiga och antalet trådar konfigureras med parametern `uploadThreads`.

Filerna flyttas till huvudcachen för hämtning när överföringen är klar. När storleken på mellanlagringscachen överskrider gränsen överförs filerna synkront till DataStore tills föregående asynkrona överföringar är slutförda och utrymme är igen tillgängligt i mellanlagringscachen. De överförda filerna tas bort från mellanlagringsområdet av ett periodiskt jobb vars intervall har konfigurerats av parametern `stagingPurgeInterval`.

Misslyckade överföringar (till exempel på grund av nätverksavbrott) placeras i en återförsökskö och försök med jämna mellanrum. Återförsöksintervallet konfigureras med `stagingRetryInterval parameter`.

#### Konfigurera icke-binära replikeringar med Amazon S3 {#configuring-binaryless-replication-with-amazon-s}

Följande steg krävs för att konfigurera en binär replikering med S3:

1. Installera författaren och publicera instanser och se till att de har startats korrekt.
1. Gå till inställningarna för replikeringsagenten genom att öppna en sida till *https://localhost:4502/etc/replication/agents.author/publish.html*.
1. Tryck på knappen **Redigera** i avsnittet **Inställningar** .
1. Ändra typalternativet **Serialisering** till **Binärt mindre**.

1. Lägg till parametern `binaryless`= `true` i transport-URI:n. Efter ändringen bör URI:n se ut ungefär så här:

   *https://localhost:4503/bin/receive?sling:authRequestLogin=1&amp;binaryless=true*

1. Starta om alla författare- och publiceringsinstanser så att ändringarna börjar gälla.

#### Skapa ett kluster med S3 och MongoDB {#creating-a-cluster-using-s-and-mongodb}

1. Packa upp CQ-snabbstart med följande kommando:

   `java -jar cq-quickstart.jar -unpack`

1. När AEM har packats upp skapar du en mapp i installationskatalogen *crx-quickstart*/*install*.

1. Skapa de här två filerna i mappen `crx-quickstart`:

   * *org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService*.*config*

   * *org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore*.*config*

   När filerna har skapats lägger du till konfigurationsalternativen efter behov.

1. Installera de två paket som krävs för S3-datalagret enligt beskrivningen ovan.
1. Kontrollera att MongoDB är installerat och att en instans av `mongod` körs.
1. Starta AEM med följande kommando:

   `java -Xmx1024m -jar cq-quickstart.jar -r crx3,crx3mongo`

1. Upprepa steg 1 till 4 för den andra AEM-instansen.
1. Starta den andra AEM-instansen.

#### Konfigurera ett delat datalager {#configuring-a-shared-data-store}

1. Skapa först konfigurationsfilen för datalagret för varje instans som krävs för att dela datalagret:

   * Om du använder en `FileDataStore` skapar du en fil med namnet `org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config` och placerar den i mappen `<aem-install>/crx-quickstart/install`.

   * Om du använder S3 som datalager skapar du en fil med namnet `rg.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` i mappen `<aem-install>/crx-quickstart/install` enligt ovan.

1. Ändra konfigurationsfilerna för datalagret på varje instans så att de pekar på samma datalager. Mer information finns i [Konfigurationer för datalager](/help/sites-deploying/data-store-config.md#data-store-configurations).
1. Om instansen har klonats från en befintlig server måste du ta bort den nya instansens `clusterId` med hjälp av det senaste körningsverktyget när databasen är offline. Kommandot du måste köra är:

   ```xml
   java -jar oak-run.jar resetclusterid < repository path | Mongo URI >
   ```

   >[!NOTE]
   >
   >Om en segmentnodbutik har konfigurerats måste databassökvägen anges. Som standard är sökvägen `<aem-install-folder>/crx-quickstart/repository/segmentstore.` Om ett dokumentnodarkiv har konfigurerats kan du använda en [monoanslutningssträng-URI](https://docs.mongodb.org/manual/reference/connection-string/).

   >[!NOTE]
   >
   >Du kan hämta Oak-körningsverktyget från följande plats:
   >
   >
   >[https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-run/](https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-run/)
   >
   >
   >Olika versioner av verktyget måste användas beroende på vilken Oak-version du använder med din AEM-installation. Kontrollera listan över versionskrav innan du använder verktyget:
   >
   >
   >
   >    * För Oak-versionerna **1.2.x** använder du Oak-run **1.2.12 eller senare**
   >    * För Oak-versioner **som är nyare än ovanstående** använder du den version av Oak som matchar Oak-kärnan i din AEM-installation.
   >
   >

1. Validera konfigurationen till sist. Du validerar genom att leta efter en unik fil som lagts till i datalagret av varje databas som delar den. Filformatet är `repository-[UUID]`, där UUID är en unik identifierare för varje enskild databas.

   Därför bör en korrekt konfiguration ha så många unika filer som det finns databaser som delar datalagret.

   Filerna lagras på olika sätt beroende på datalagret:

   * För `FileDataStore` skapas filerna under rotsökvägen för datalagringsmappen.
   * För `S3DataStore` skapas filerna i den konfigurerade S3-bucket under mappen `META`.

## Azure Data Store {#azure-data-store}

AEM kan konfigureras för att lagra data i Microsoft® Azure-lagringstjänst. Det använder `org.apache.jackrabbit.oak.plugins.blob.datastore.AzureDataStore.config`-PID:t för konfiguration.

Om du vill aktivera Azure-datalagrets funktioner måste ett funktionspaket som innehåller Azure Connector hämtas och installeras. Gå till [Adobe-databasen](https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.oak.azureblobconnector/) och hämta den senaste versionen från 1.6.x-versionerna av funktionspaketet (till exempel com.adobe.granite.oak.azureblobconnector-1.6.3.zip).

>[!NOTE]
>
>När du använder AEM med tarMK lagras binärfiler som standard i FileDataStore. Om du vill använda tarMK med Azure DataStore måste du starta AEM med runmode `crx3tar-nofds`, till exempel:

```shell
java -jar <aem-jar-file>.jar -r crx3tar-nofds
```

När du har hämtat den kan du installera och konfigurera Azure-anslutningen på följande sätt:

1. Extrahera innehållet i ZIP-filen för funktionspaketet till en tillfällig mapp.

1. Gå till den tillfälliga mappen och kopiera innehållet i `jcr_root/libs/system/install` till mappen `<aem-install>crx-quickstart/install`.
1. Om AEM redan har konfigurerats för att fungera med lagringen tar du bort alla befintliga konfigurationsfiler från mappen `/crx-quickstart/install` innan du fortsätter. De filer som måste tas bort är:

   ForMongoMK:

   `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`

   För tarMK:

   `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`

1. Återgå till den tillfälliga platsen där funktionspaketet har extraherats och kopiera innehållet i `jcr_root/libs/system/config` till mappen `<aem-install>/crx-quickstart/install`.
1. Redigera konfigurationsfilen och lägg till de konfigurationsalternativ som krävs för installationen.
1. Starta AEM.

Du kan använda konfigurationsfilen med följande alternativ:

* azureSas=&quot;&quot;: I version 1.6.3 av anslutningsprogrammet har stöd för Azure Shared Access Signature (SAS) lagts till. **Om det finns både SAS- och lagringsreferenser i konfigurationsfilen har SAS prioritet.** Mer information om SAS finns i den [officiella dokumentationen](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview). Se till att tecknet &#39;=&#39; escape-konverteras som &#39;\=&#39;.

* azureBlobEndpoint=&quot;&quot;: Azure Blob Endpoint. Till exempel https://&lt;storage-account>.blob.core.windows.net.
* accessKey=&quot;&quot;: Lagringskontots namn. Mer information om autentiseringsuppgifter för Microsoft® Azure finns i den [officiella dokumentationen](https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account).

* secretsKey=&quot;&quot;: Lagringsåtkomstnyckeln. Se till att tecknet &#39;=&#39; escape-konverteras som &#39;\=&#39;.
* container=&quot;&quot;: Microsoft® Azure-blobbens lagringsbehållarnamn. Behållaren är en gruppering av en uppsättning blober. Mer information finns i den [officiella dokumentationen](https://learn.microsoft.com/en-us/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata?redirectedfrom=MSDN).
* maxConnections=&quot;&quot;: Det samtidiga antalet samtidiga begäranden per åtgärd. Standardvärdet är 1.
* maxErrorRetry=&quot;&quot;: Antal försök per begäran. Standardvärdet är 3.
* socketTimeout=&quot;&quot;&quot;: Tidsgränsen i millisekunder som används för begäran. Standardvärdet är 5 minuter.

Förutom inställningarna ovan kan följande inställningar också konfigureras:

* sökväg: Sökvägen till datalagret. Standardvärdet är `<aem-install>/repository/datastore.`
* RecordLength: Den minsta storleken för ett objekt som ska lagras i datalagret. Standardvärdet är 16 kB.
* maxCachedBinarySize: Binärfiler som är mindre än eller lika med den här storleken lagras i minnescachen. Storleken anges i byte. Standardvärdet är 17 408 (17 kB).
* cacheSize: Cachens storlek. Värdet anges i byte. Standardvärdet är 64 GB.
* hemlighet: Ska endast användas om binär replikering används för delad datalagringsinställning.
* stagingSplitPercentage: Den procentandel av cachestorleken som har konfigurerats för att användas för att mellanlagra asynkrona överföringar. Standardvärdet är 10.
* uploadThreads: Antalet överförda trådar som används för asynkrona överföringar. Standardvärdet är 10.
* stagingPurgeInterval: Intervallet i sekunder för tömning av slutförda överföringar från mellanlagringscachen. Standardvärdet är 300 sekunder (5 minuter).
* stagingRetryInterval: Återförsöksintervallet i sekunder för misslyckade överföringar. Standardvärdet är 600 sekunder (10 minuter).

>[!NOTE]
>
>Alla inställningar ska anges mellan citattecken, till exempel:

```shell
accessKey="ASDASDERFAERAER"
secretKey="28932hfjlkwdo8fufsdfas\=\="
```

## Skräpinsamling för datalager {#data-store-garbage-collection}

Datalagrets skräpinsamlingsprocess används för att ta bort oanvända filer i datalagret, vilket frigör värdefullt diskutrymme under processen.

Du kan köra skräpinsamling för datalager genom att:

1. Gå till JMX-konsolen på *https://&lt;serveradress:port>/system/console/jmx*
1. Söker efter **RepositoryManagement.** När du har hittat Databashanterarens MBean klickar du på det för att visa tillgängliga alternativ.
1. Bläddra till slutet av sidan och klicka på länken **startDataStoreGC(boolesk markOnly)** .
1. I följande dialogruta anger du `false` för parametern `markOnly` och klickar sedan på **Anropa**:

   ![chlimage_1-9](assets/chlimage_1-9.png)

   >[!NOTE]
   >
   >Parametern `markOnly` anger om svepfasen för skräpinsamlingen körs eller inte.

## Skräpinsamling för datalager för ett delat datalager {#data-store-garbage-collection-for-a-shared-data-store}

>[!NOTE]
>
>När du utför skräpinsamling i ett klustrat eller delat datalager kan loggen konfigureras (med mongo- eller segmentmål) med varningsmeddelanden om att vissa blob-ID inte kan tas bort. Blob-ID:n som tagits bort i en tidigare skräpinsamling refereras felaktigt igen av andra kluster eller delade noder som inte har information om ID-borttagningar. När skräpinsamlingen utförs loggas därför en varning när den försöker ta bort ett ID som redan har tagits bort i den senaste körningen. Det här beteendet påverkar inte prestanda eller funktioner.

>[!NOTE]
>
>Om du använder en delad datalagerinställning och datalagrets skräpinsamling är inaktiverad kan rensningen av Lucene-binärfilen plötsligt öka diskutrymmet som används. Överväg att inaktivera BlobTracker för alla författare- och publiceringsinstanser genom att göra följande:
>
>1. Stoppa AEM Instance.
>2. Lägg till parametern `blobTrackSnapshotIntervalInSecs=L"0"` i filen `crx-quickstart/install/org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`. Den här parametern kräver Oak 1.12.0, 1.10.2 eller senare.
>3. Starta om AEM Instance.

Med nyare versioner av AEM kan skräpinsamlingen i datalagret även köras på datalager som delas av mer än en databas. Så här kan du köra skräpinsamling i datalager på ett delat datalager:

1. Se till att alla underhållsuppgifter som konfigurerats för datalagrets skräpinsamling är inaktiverade för alla databasinstanser som delar datalagret.
1. Kör stegen som anges i [binär skräpinsamling](/help/sites-deploying/data-store-config.md#data-store-garbage-collection) individuellt på **alla** databasinstanser som delar datalagret. Tänk dock på att ange `true` för parametern `markOnly` innan du klickar på knappen Anropa:

   ![chlimage_1-10](assets/chlimage_1-10.png)

1. När du har slutfört ovanstående procedur på alla instanser kör du skräpinsamlingen för datalagret igen från **någon** av instanserna:

   1. Gå till JMX-konsolen och välj Repository Manager Mbean.
   1. Klicka på länken **Click startDataStoreGC(boolean markOnly)** .
   1. I följande dialogruta anger du `false` som `markOnly`-parameter igen.

   Alla filer som hittas sorteras med markeringsfasen som användes före och tar bort resten som inte används från datalagret.
