---
title: AEM 6.5 till AEM 6.5 LTS Content Migration Using Oak-upgrade
description: Lär dig hur du migrerar innehåll från AEM 6.5 till AEM 6.5 LTS med hjälp av ekupningsverktyget
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 8c4ffb0e-b4dc-4a81-ac43-723754cbc0de
source-git-commit: 69033442fda82d9efdd1ba2f55a45173c8ffc6ec
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# AEM 6.5 till AEM 6.5 LTS Content Migration Using Oak-upgrade {#aem-65-to-aem-65lts-content-migration-using-oak-upgrade}

Det här dokumentet innehåller en omfattande guide för uppgradering av Adobe Experience Manager från version **6.5** till **6.5 LTS** som fokuserar på migrering av innehållsdatabas med hjälp av ekupningsverktyget, ett kraftfullt verktyg för att överföra innehåll mellan olika databaser med precision och kontroll.

## Förutsättningar {#prerequisites}

Kontrollera att följande krav är uppfyllda innan du startar migreringen:

1. Java-kompatibilitet: AEM 6.5 LTS måste installeras och konfigureras för att köras med Java™ 17. Starta AEM-instansen när du har konfigurerat den och kontrollera att alla paket är aktiva och körs utan problem
1. Systemresurser: Se till att det finns tillräckligt med diskutrymme och minne för att hantera båda databaserna under migreringsprocessen
1. Oak-uppgraderingsverktyg: Hämta `oak-upgrade` jar från [Maven-databasen](https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-upgrade). Kontrollera att versionen överensstämmer med den ekkärnversion som används i AEM 6.5 LTS. Oak-uppgraderingsverktyget kan köras i Oracle® Java™ 11 eller senare

## Steg för steg-migreringsprocess {#step-by-step-migration-process}

### Stoppa AEM 6.5 och AEM 6.5 LTS {#stopping-aem65-and-aem65lts}

Stoppa AEM 6.5- och AEM 6.5 LTS-instanserna innan du startar migreringen. Detta garanterar att databasen är i ett stabilt läge och att inga ytterligare skrivningar sker under migreringen.

### Säkerhetskopiera AEM 6.5 Instance {#backing-up-the-aem65-instance}

Gör en fullständig säkerhetskopiering av din AEM 6.5-instans om du inte redan har gjort det.

### Använda Oak-uppgraderingsverktyget för innehållsmigrering {#using-the-oak-upgrade-tool-for-content-migration}

Oak-uppgraderingsverktyget körs via kommandoraden, vilket visas här:

```
java -jar oak-upgrade-*.jar [options] /path/to/source/repository /path/to/destination/repository 
```

Nedan finns de viktigaste kommandona och alternativen:

**Nyckelalternativ**

* `--include-paths`: Ange underträd som ska inkluderas i migreringen. I det här exemplet finns information om kommandoanvändning:

  ```
  java -jar oak-upgrade-*.jar --include-paths=/content/site /old/repository /new/repository
  ```

* `--exclude-paths`: Exkludera specifika sökvägar från migreringen. Var försiktig när du använder det här alternativet - om sökvägen finns i målsystemet tas den bort. I det här exemplet finns information om kommandoanvändning:

  ```
  java -jar oak-upgrade-*.jar --exclude-paths=/content/old_site /old/repository /new/repository 
  ```

* `--copy-binaries`: Som standard migreras endast referenser till binära filer, och de faktiska filerna lämnas kvar i den ursprungliga blob-/datalagret. Därför är den nya databasen fortfarande beroende av källagringsplatsen för binärfiler. Om du vill migrera binärfiler tillsammans med databasinnehållet använder du parametern `--copy-binaries` för att kopiera alla binära data till den nya lagringsplatsen, vilket visas nedan:

  ```
  java -jar oak-upgrade-*.jar \
  --copy-binaries \
  --src-datastore=/old/repository/datastore \
  --datastore=/new/repository/datastore \
  /old/repository \
  /new/repository 
  ```

### Migrera kontrollpunkter {#migratiing-checkpoints}

När du migrerar en gammal SegmentMK-databas (före Oak 1.6) till en ny SegmentMK (Oak version 1.6 eller senare) migreras även kontrollpunkterna. Detta gör att omindexering kan undvikas när Oak körs för första gången på den nya databasen. Kontrollpunkterna migreras dock inte i följande fall:

1. Anpassade inkluderings-, uteslutnings- eller kopplingssökvägar har angetts eller
1. Binärfilerna kopieras av referenser, inget källdatalager anges och två olika kontrollpunkter innehåller en annan binär fil under samma sökväg.

I det andra fallet ger ekuppgradering följande varningar och avbrott:

```
Checkpoints won't be copied, because no external datastore has been specified. This will result in the full repository reindexing on the first start. Use --skip-checkpoints to force the migration or see https://jackrabbit.apache.org/oak/docs/migration.html#Checkpoints_migration for more info. 
```

Det enklaste sättet att åtgärda problemet är att ange källdatalagret i kommandoradsalternativen (till exempel `--src-datastore` eller `--src-s3datastore`).

Varningen kan också ignoreras, men i det här fallet kommer databasen att indexeras om helt vid första starten. Det kan vara en lång process, särskilt för stora instanser. Databasen kan inte användas förrän omindexeringsprocessen är klar. Använd alternativet `--skip-checkpoints` om du vill inaktivera varningen.

Du kan även indexera om databasen offline innan du startar AEM med [omindexering offline](/help/sites-deploying/offline-reindexing.md) för att undvika fullständig omindexering vid första starten.

Mer information om ekuponteringsverktyget och avancerad användning finns i [officiell dokumentation](https://jackrabbit.apache.org/oak/docs/migration.html).
