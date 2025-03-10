---
title: Stöd för RDBMS i AEM 6.4
description: Lär dig mer om relationsdatabasens beständighetsstöd i AEM 6.4 och de tillgängliga konfigurationsalternativen.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
docset: aem65
feature: Administering
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: b6886495-f06e-4e4f-8f38-39206ef43072
source-git-commit: f145e5f0d70662aa2cbe6c8c09795ba112e896ea
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 0%

---

# Stöd för RDBMS i AEM 6.4{#rdbms-support-in-aem}

## Ökning {#overview}

Stöd för relationsdatabasbeständighet i AEM implementeras med Document Microkernel. Dokumentmikrokärnan är grunden som också används för implementering av MongoDB-beständighet.

Det består av ett Java-API som är baserat på Mongo Java API. En implementering av ett BlobStore API ingår också. Bloggar lagras som standard i databasen.

Mer information om implementeringsdetaljer finns i dokumentationen för [RDBDocumentStore](https://jackrabbit.apache.org/oak/docs/apidocs/org/apache/jackrabbit/oak/plugins/document/rdb/RDBDocumentStore.html) och [RDBBlobStore](https://jackrabbit.apache.org/oak/docs/apidocs/org/apache/jackrabbit/oak/plugins/document/rdb/RDBBlobStore.html) .

>[!NOTE]
>
>Stöd för **PostgreSQL 9.4** finns också, men endast för demoändamål. Den kommer inte att vara tillgänglig för produktionsmiljöer.

## Databaser som stöds {#supported-databases}

Mer information om nivån på Relational Database-stödet i AEM finns på sidan [Tekniska krav](/help/sites-deploying/technical-requirements.md).

## Konfigurationssteg {#configuration-steps}

Databasen skapas genom att OSGi-tjänsten `DocumentNodeStoreService` konfigureras. Det har utökats med stöd för relationsdatabasbeständighet utöver MongoDB.

För att en datakälla ska fungera måste den konfigureras med AEM. Detta görs via filen `org.apache.sling.datasource.DataSourceFactory.config`. JDBC-drivrutinerna för respektive databas måste anges separat som OSGi-paket i den lokala konfigurationen.

Anvisningar om hur du skapar OSGi-paket för JDBC-drivrutiner finns i [dokumentationen](https://sling.apache.org/documentation/bundles/datasource-providers.html#convert-driver-jars-to-bundle) på webbplatsen Apache Sling.

När paketen är på plats följer du stegen nedan för att konfigurera AEM med RDB-beständighet:

1. Kontrollera att databasdaemon har startats och att du har en aktiv databas som kan användas med AEM.
1. Kopiera AEM 6.3 jar till installationskatalogen.
1. Skapa en mapp med namnet `crx-quickstart\install` i installationskatalogen.
1. Konfigurera dokumentnodarkivet genom att skapa en konfigurationsfil med följande namn i katalogen `crx-quickstart\install`:

   * `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`

1. Konfigurera datakällan och JDBC-parametrarna genom att skapa en annan konfigurationsfil med följande namn i mappen `crx-quickstart\install`:

   * `org.apache.sling.datasource.DataSourceFactory-oak.config`

   >[!NOTE]
   >
   >Mer information om datakällkonfigurationen för varje databas som stöds finns i [Konfigurationsalternativ för Data Source](/help/sites-deploying/rdbms-support-in-aem.md#data-source-configuration-options).

1. Förbered sedan JDBC OSGi-paketen som ska användas med AEM:

   1. Skapa en mapp med namnet `9` i mappen `crx-quickstart/install`.

   1. Placera JDBC-behållaren i den nya mappen.

1. Starta slutligen AEM med körningslägena `crx3` och `crx3rdb`:

   ```java
   java -jar quickstart.jar -r crx3,crx3rdb
   ```

## Konfigurationsalternativ för Data Source {#data-source-configuration-options}

OSGi-konfigurationen `org.apache.sling.datasource.DataSourceFactory-oak.config` används för att konfigurera de parametrar som behövs för kommunikation mellan AEM och databasens beständighetslager.

Följande konfigurationsalternativ är tillgängliga:

* `datasource.name:` Datakällans namn. Standardvärdet är `oak`.

* `url:` URL-strängen för databasen som ska användas med JDBC. Varje databastyp har ett eget URL-strängformat. Mer information finns i [URL-strängformat](/help/sites-deploying/rdbms-support-in-aem.md#url-string-formats) nedan.

* `driverClassName:` Klassnamnet för JDBC-drivrutinen. Detta varierar beroende på vilken databas du vill använda och därefter vilken drivrutin som behövs för att ansluta till den. Nedan visas klassnamnen för alla databaser som stöds av AEM:

   * `org.postgresql.Driver` för PostgreSQL;
   * `com.ibm.db2.jcc.DB2Driver` för DB2;
   * `oracle.jdbc.OracleDriver` för Oracle;
   * `com.mysql.jdbc.Driver` för MySQL och MariaDB (experimentell);
   * c `om.microsoft.sqlserver.jdbc.SQLServerDriver` för Microsoft SQL Server (experimentell).

* `username:` Användarnamnet som databasen körs under.

* `password:` Databaslösenordet.

### URL-strängformat {#url-string-formats}

Ett annat URL-strängformat används i datakällkonfigurationen beroende på vilken databastyp som ska användas. Nedan finns en lista över format för de databaser som AEM stöder:

* `jdbc:postgresql:databasename` för PostgreSQL;
* `jdbc:db2://localhost:port/databasename` för DB2;
* `jdbc:oracle:thin:localhost:port:SID` för Oracle;
* `jdbc:mysql://localhost:3306/databasename` för MySQL och MariaDB (experimentell);
* `jdbc:sqlserver://localhost:1453;databaseName=name` för Microsoft SQL Server (experimentell).

## Kända begränsningar {#known-limitations}

Samtidigt bruk av flera AEM-instanser med en databas stöds av RDBMS-persistence, men inte samtidiga installationer.

För att undvika detta måste du först köra installationen med en enda medlem och lägga till de andra efter att den första installationen är klar.
