---
title: Skräpinsamling för datalager
description: Lär dig hur du konfigurerar skräpinsamlingen för datalagring så att du frigör diskutrymme.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Operations
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1897'
ht-degree: 0%

---

# Skräpinsamling för datalager {#data-store-garbage-collection}

När en konventionell WCM-resurs tas bort kan referensen till den underliggande datalagreposten tas bort från nodhierarkin, men själva datalagreposten finns kvar. Den här dataarkivposten utan referenser blir då&quot;skräp&quot; som inte behöver behållas. Om det finns flera skräpinresurser är det bra att ta bort dem för att bevara utrymme och optimera prestanda för säkerhetskopiering och filsystemsunderhåll.

För det mesta tenderar ett WCM-program att samla in information men inte ta bort information nästan lika ofta. Även om nya bilder läggs till, och till och med ersätter gamla versioner, behåller versionskontrollsystemet den gamla och kan återställas om det behövs. Därför lagras merparten av det innehåll som vi anser vara utökat i systemet permanent. Så vad är den typiska källan för&quot;skräp&quot; i databasen som vi kanske vill rensa upp?

AEM använder databasen som lagring för flera interna aktiviteter och hushållsaktiviteter:

* Paket som byggts och laddats ned
* Tillfälliga filer har skapats för publiceringsreplikering
* Arbetsflödesnyttolaster
* Assets som skapats temporärt under DAM-återgivning

När något av dessa temporära objekt är stort nog för att kräva lagring i datalagret, och när objektet inte längre används, förblir själva datalagret som&quot;skräp&quot;. I ett typiskt WCM-program för författare/publicering är den största källan till skräp av den här typen vanligtvis processen för publiceringsaktivering. När data replikeras till Publicera, samlas de först in i samlingar i ett effektivt dataformat som kallas Durbo och lagras i databasen under `/var/replication/data`. Datapaketen är ofta större än den kritiska storlekströskeln för datalagret och därför lagras de som datalagringsposter. När replikeringen är klar tas noden i `/var/replication/data` bort, men datalagringsposten förblir&quot;skräp&quot;.

En annan källa till återvinningsbart skräp är paket. Paketdata lagras, precis som allt annat, i databasen och därmed för paket som är större än 4 kB i datalagret. Under ett utvecklingsprojekt eller under en längre tid med ett system kan paket byggas och byggas om många gånger, och varje bygge resulterar i en ny datalagringspost, vilket gör den föregående byggens arkiv överbliven.

## Hur fungerar skräpinsamlingen i datalagret? {#how-does-data-store-garbage-collection-work}

Om databasen har konfigurerats med ett externt datalager kommer skräpinsamlingen i [datalagret att köras automatiskt](/help/sites-administering/data-store-garbage-collection.md#automating-data-store-garbage-collection) som en del av veckounderhållet. Systemadministratören kan även [köra skräpinsamlingen för datalager manuellt](#running-data-store-garbage-collection) vid behov. I allmänhet rekommenderar vi att skräpinsamlingen för datalager utförs regelbundet, men att följande faktorer beaktas vid planering av skräpinsamlingar för datalager:

* Skräpinsamlingar i datalagret tar tid och kan påverka prestanda, så de bör planeras i enlighet med detta.
* Borttagning av skräpposter i datalager påverkar inte normala prestanda, vilket inte är en prestandaoptimering.
* Om lagringsutnyttjande och relaterade faktorer som säkerhetskopieringstider inte är något problem kan skräpinsamlingen i datalagringen fördröjas.

Skräpinsamlaren för datalagret gör först en anteckning om den aktuella tidsstämpeln när processen börjar. Samlingen utförs sedan med hjälp av en algoritm för flerpassmärke/svepmönster.

I den första fasen utför skräpinsamlaren för datalagring en omfattande genomgång av allt databasinnehåll. För varje innehållsobjekt som har en referens till en datalagringspost, placerades filen i filsystemet och en metadatauppdatering utfördes - det ändrade attributet &quot;last modified&quot; eller MTIME ändrades. I det här läget blir filer som nås av den här fasen nyare än den inledande baslinjetidstämpeln.

I den andra fasen går skräpinsamlaren för datalagret igenom den fysiska katalogstrukturen i datalagret på ungefär samma sätt som en &quot;sök&quot;. Den undersökte filens&quot;senast ändrade&quot; eller MTIME-attribut och fastställer följande:

* Om MTIME är nyare än den ursprungliga baslinjetidstämpeln hittades filen i den första fasen, eller så är det en helt ny fil som lades till i databasen medan insamlingsprocessen pågick. I något av dessa fall ska registreringen anses vara aktiv och akten ska inte raderas.
* Om MTIME är före den ursprungliga baslinjetidstämpeln är filen inte en aktivt refererad fil och den betraktas som ett borttagbart skräp.

Den här metoden fungerar bra för en enskild nod med ett privat datalager. Men datalagret kan delas, och om det är det innebär det att potentiellt aktiva live-referenser till datalagerposter från andra databaser inte kontrolleras och att aktiva refererade filer tas bort av misstag. Det är av största vikt att systemadministratören förstår datalagrets delade karaktär innan någon skräpinsamling planeras, och endast använder den enkla inbyggda skräpinsamlingsprocessen för datalager när det är känt att datalagret inte delas.

>[!NOTE]
>
>När du utför skräpinsamling i ett klustrat eller delat datalager (med mongo- eller segmentmål) kan loggen visa varningar om att vissa blob-ID inte kan tas bort. Detta beror på att blob-ID:n som tagits bort i en tidigare skräpinsamling felaktigt refereras igen av andra kluster eller delade noder som inte har information om ID-borttagningar. När skräpinsamlingen utförs loggas därför en varning när den försöker ta bort ett ID som redan har tagits bort i den senaste körningen. Det här beteendet påverkar inte prestanda eller funktioner.

## Kör skräpinsamling för datalager {#running-data-store-garbage-collection}

Det finns tre sätt att köra skräpinsamling för datalager, beroende på vilken datalagerinställning som AEM körs på:

1. Via [Revision Cleanup](/help/sites-deploying/revision-cleanup.md) - en skräpinsamlingsmekanism som vanligtvis används för rensning av nodarkiv.

1. Via [skräpinsamlingen i datalagret](/help/sites-administering/data-store-garbage-collection.md#running-data-store-garbage-collection-via-the-operations-dashboard) - en skräpinsamlingsmekanism som är specifik för externa datalager, som är tillgänglig på instrumentpanelen för åtgärder.
1. Via [JMX-konsolen](/help/sites-administering/jmx-console.md).

Om tarMK används både som nodarkiv och datalager kan Revision Cleanup användas för skräpinsamling för både nodarkivet och datalagret. Om ett externt datalager har konfigurerats, till exempel ett filsystemsdatalager, måste skräpinsamlingen för datalagret aktiveras separat från Revision Cleanup. Skräpinsamlingen i datalagret kan aktiveras antingen via instrumentpanelen för åtgärder eller JMX-konsolen.

Tabellen nedan visar vilken typ av skräpinsamling för datalager som måste användas för alla datalager-distributioner som stöds i AEM 6:

<table>
 <tbody>
  <tr>
   <td><strong>Nodarkiv</strong><br /> </td>
   <td><strong>Datalager</strong></td>
   <td><strong>Skräpinsamlingsmekanism</strong><br /> </td>
  </tr>
  <tr>
   <td>tarMK</td>
   <td>tarMK</td>
   <td>Revision Cleanup (binärfiler är inbäddade i segmentarkivet)</td>
  </tr>
  <tr>
   <td>tarMK</td>
   <td>Externt filsystem</td>
   <td><p>Skräpinsamlingsaktivitet för datalager via instrumentpanelen för åtgärder</p> <p>JMX Console</p> </td>
  </tr>
  <tr>
   <td>MongoDB</td>
   <td>MongoDB</td>
   <td><p>Skräpinsamlingsaktivitet för datalager via instrumentpanelen för åtgärder</p> <p>JMX Console</p> </td>
  </tr>
  <tr>
   <td>MongoDB</td>
   <td>Externt filsystem</td>
   <td><p>Skräpinsamlingsaktivitet för datalager via instrumentpanelen för åtgärder</p> <p>JMX Console</p> </td>
  </tr>
 </tbody>
</table>

### Kör skräpinsamlingen för datalagret via kontrollpanelen för åtgärder {#running-data-store-garbage-collection-via-the-operations-dashboard}

Det inbyggda veckounderhållet, som är tillgängligt via [Operations Dashboard](/help/sites-administering/operations-dashboard.md), innehåller en inbyggd aktivitet som utlöser Data Store-skräpinsamlingen kl. 1:00 på söndagar.

Om du behöver köra skräpinsamlingen för datalagret utanför den här tiden kan den aktiveras manuellt via kontrollpanelen för åtgärder.

Innan du kör skräpinsamlingen för datalagret bör du kontrollera att inga säkerhetskopieringar körs samtidigt.

1. Öppna instrumentpanelen för åtgärder genom att **Navigera** > **Verktyg** > **Åtgärder** > **Underhåll**.
1. Klicka på **Underhållsfönstret varje vecka**.

   ![chlimage_1-64](assets/chlimage_1-64.png)

1. Markera **skräpinsamlingen för datalagret** och klicka sedan på ikonen **Kör** .

   ![chlimage_1-65](assets/chlimage_1-65.png)

1. Skräpinsamlingen för datalagret körs och dess status visas på instrumentpanelen.

   ![chlimage_1-66](assets/chlimage_1-66.png)

>[!NOTE]
>
>Åtgärden Skräpsamling i datalagret visas bara om du har konfigurerat ett externt fildatalager. Mer information om hur du konfigurerar ett fildatalager finns i [Konfigurera nodarkiv och datalager i AEM 6](/help/sites-deploying/data-store-config.md#file-data-store).

### Kör skräpinsamlingen för datalagret via JMX-konsolen {#running-data-store-garbage-collection-via-the-jmx-console}

Det här avsnittet handlar om att manuellt köra skräpinsamling för datalager via JMX-konsolen. Om installationen har konfigurerats utan ett externt datalager gäller detta inte installationen. Se i stället instruktionerna om hur du kör Revision-rensning under [Underhåll databasen](/help/sites-deploying/storage-elements-in-aem-6.md#maintaining-the-repository).

>[!NOTE]
>
>Om du kör tarMK med ett externt datalager måste du först köra Revision Cleanup för att skräpinsamlingen ska bli effektiv.

Så här kör du skräpinsamlingen:

1. I Apache Felix OSGi Management Console markerar du fliken **Main** och väljer **JMX** på följande meny.
1. Sök efter och klicka sedan på **Databashanteraren** MBean (eller gå till `https://<host>:<port>/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Drepository+manager%2Ctype%3DRepositoryManagement`).
1. Klicka på **startDataStoreGC(boolesk markOnly)**.
1. Ange `true` för parametern `markOnly` om det behövs:

   | **Alternativ** | **Beskrivning** |
   |---|---|
   | boolesk markOnly | Ange som true om du bara vill markera referenser och inte svepa i markeringen och svepningen. Det här läget ska användas när den underliggande BlobStore delas mellan flera olika databaser. För alla andra fall anger du det som false om du vill utföra en fullständig skräpinsamling. |

1. Klicka på **Anropa**. CRX kör skräpinsamlingen och anger när den är klar.

>[!NOTE]
>
>Skräpinsamlingen i datalagret samlar inte in filer som har tagits bort de senaste 24 timmarna.

>[!NOTE]
>
>Datalagrets skräpinsamlingsaktivitet startar bara om du har konfigurerat ett externt fildatalager. Om ett externt fildatalager inte har konfigurerats returnerar aktiviteten meddelandet `Cannot perform operation: no service of type BlobGCMBean found` efter anropet. Mer information om hur du konfigurerar ett fildatalager finns i [Konfigurera nodarkiv och datalager i AEM 6](/help/sites-deploying/data-store-config.md#file-data-store).

## Automatisera skräpinsamling för datalager {#automating-data-store-garbage-collection}

Om det är möjligt bör skräpinsamlingen i datalagret köras när det finns liten belastning på systemet, till exempel på morgonen.

Det inbyggda veckounderhållet, som är tillgängligt via [Operations Dashboard](/help/sites-administering/operations-dashboard.md), innehåller en inbyggd aktivitet som utlöser Data Store-skräpinsamlingen kl. 1:00 på söndagar. Du bör även kontrollera att inga säkerhetskopieringar körs just nu. Underhållsperiodens början kan anpassas via kontrollpanelen efter behov.

>[!NOTE]
>
>Orsaken till att den inte körs samtidigt är att gamla (och oanvända) datalagringsfiler också säkerhetskopieras, så om det krävs att de återställs till en gammal version finns binärfilerna fortfarande kvar i säkerhetskopian.

Om du inte vill köra skräpinsamlingen i datalagret med fönstret för veckounderhåll på kontrollpanelen för åtgärder, kan den även automatiseras med hjälp av widgeten eller surfa HTTP-klienter. Här följer ett exempel på hur du automatiserar skräpinsamlingen med hjälp av url:

>[!CAUTION]
>
>I följande exempel kan `curl`-kommandon behöva konfigureras olika parametrar för din instans, till exempel värdnamnet ( `localhost`), porten ( `4502`), administratörslösenordet ( `xyz`) och olika parametrar för den faktiska datalagringsskräpinsamlingen.

Här är ett exempel på ett curl-kommando som anropar skräpinsamlingen för datalagring via kommandoraden:

```shell
curl -u admin:admin -X POST --data markOnly=true  https://localhost:4503/system/console/jmx/org.apache.jackrabbit.oak"%"3Aname"%"3Drepository+manager"%"2Ctype"%"3DRepositoryManagement/op/startDataStoreGC/boolean
```

Kommandot curl returneras omedelbart.

## Kontrollerar datalagrets konsekvens {#checking-data-store-consistency}

Konsekvenskontrollen av datalagret rapporterar eventuella binära data som saknas men som fortfarande refereras. Så här startar du en konsekvenskontroll:

1. Gå till JMX-konsolen. Mer information om hur du använder JMX-konsolen finns i [Övervaka serverresurser med JMX-konsolen](/help/sites-administering/jmx-console.md#using-the-jmx-console).
1. Sök efter **BlobGarbageCollection** Mbean och klicka på den.
1. Klicka på länken `checkConsistency()`.

När konsekvenskontrollen är klar visas antalet binärfiler som rapporterats som saknade i ett meddelande. Om talet är större än 0 kan du kontrollera `error.log` för mer information om binärfilerna som saknas.

Här nedan hittar du ett exempel på hur de saknade binärfilerna rapporteras i loggarna:

```xml
11:32:39.673 INFO [main] MarkSweepGarbageCollector.java:600 Consistency check found [1] missing blobs
```

```xml
11:32:39.673 WARN [main] MarkSweepGarbageCollector.java:602 Consistency check failure intheblob store : DataStore backed BlobStore [org.apache.jackrabbit.oak.plugins.blob.datastore.OakFileDataStore], check missing candidates in file /tmp/gcworkdir-1467352959243/gccand-1467352959243
```
