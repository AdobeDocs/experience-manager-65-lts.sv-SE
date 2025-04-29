---
title: Så här kör du AEM med TARMK Cold Standby
description: Lär dig hur du skapar, konfigurerar och underhåller en StjärtMK Cold Standby-konfiguration.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
docset: aem65
feature: Administering
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 71e3d2cd-4e22-44a2-88dd-1f165bf2b3d8
source-git-commit: 408f6aaedd2cc0315f6e66b83f045ca2716db61d
workflow-type: tm+mt
source-wordcount: '2672'
ht-degree: 0%

---

# Så här kör du AEM med TARMK Cold Standby{#how-to-run-aem-with-tarmk-cold-standby}

## Introduktion {#introduction}

Tack vare kapaciteten för vänteläge i kallt läge för Tjärmikrokärnan kan en eller flera Adobe Experience Manager-instanser i vänteläge (AEM) ansluta till en primär instans. Synkroniseringsprocessen är bara ett sätt, vilket innebär att den bara utförs från den primära instansen till standby-instansen.

Syftet med standby-instanserna är att garantera en live-datakopia av huvuddatabasen och se till att det går snabbt att växla utan dataförlust om mallsidan inte är tillgänglig av någon anledning.

Innehållet synkroniseras linjärt mellan den primära instansen och standby-instansen utan några integritetskontroller för att filer eller databaser är skadade. På grund av den här designen är standby-instanser exakta kopior av den primära instansen och kan inte bidra till att minska inkonsekvenser i primära instanser.

>[!NOTE]
>
>Funktionen Vänteläge i kallt format är avsedd att skydda scenarier där hög tillgänglighet krävs för **Författare** -instanser. I situationer där hög tillgänglighet krävs för **Publicera**-instanser med hjälp av Tjärmikrokärnan rekommenderar Adobe att du använder en publiceringsgrupp.
>
>Mer information om fler tillgängliga distributioner finns på sidan [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md).

>[!NOTE]
>
>När standby-instansen har konfigurerats, eller härletts från den primära noden, ger den endast åtkomst till följande konsol (för administrationsrelaterade aktiviteter):
>
>* OSGI Web Console
>
>Andra konsoler är inte tillgängliga.

## Så här fungerar det {#how-it-works}

På den primära AEM-instansen öppnas en TCP-port och lyssnar på inkommande meddelanden. För närvarande finns det två typer av meddelanden som slavarna skickar till mallsidan:

* ett meddelande som begär segment-ID för aktuellt huvud
* ett meddelande som begär segmentdata med ett angivet ID

Vänteläge begär regelbundet segment-ID:t för den primära huvuddelen. Om segmentet är lokalt okänt hämtas det. Om den redan finns jämförs segmenten och om det behövs efterfrågas även segment med referenser.

>[!NOTE]
>
>Standby-instanser tar inte emot någon typ av begäranden eftersom de körs i synkroniseringsläge. Det enda avsnittet som är tillgängligt på en standby-instans är webbkonsolen, som underlättar konfigureringen av paket och tjänster.

En typisk STAMK Cold Standby-driftsättning:

![chlimage_1](assets/chlimage_1.png)

## Andra egenskaper {#other-characteristics}

### Robusitet {#robustness}

Dataflödet är utformat för att automatiskt upptäcka och hantera anslutnings- och nätverksrelaterade problem. Alla paket är paketerade med kontrollsummor och när problem uppstår med anslutningen eller skadade paket aktiveras mekanismer för återförsök.

#### Prestanda {#performance}

Om du aktiverar TonaMK Cold Standby på den primära instansen påverkas prestanda nästan inte. Den extra CPU-förbrukningen är låg och den extra hårddisk- och nätverks-I/O-funktionen bör inte orsaka några prestandaproblem.

I vänteläge kan du förvänta dig hög förbrukning av CPU under synkroniseringsprocessen. Eftersom proceduren inte är flertrådig går det inte att öka hastigheten genom att använda flera kärnor. Om inga data ändras eller överförs finns det ingen mätbar aktivitet. Anslutningshastigheten varierar beroende på maskinvara och nätverksmiljö, men beror inte på storleken på databasen eller SSL-användningen. Tänk på detta när du beräknar den tid som krävs för en inledande synkronisering eller när mycket data har ändrats under tiden på den primära noden.

#### Dokumentskydd {#security}

Om man utgår ifrån att alla instanser körs i samma säkerhetszon för intranätet minskar risken för säkerhetsöverträdelse avsevärt. Du kan dock lägga till ett extra säkerhetslager genom att aktivera SSL-anslutningar mellan slavarna och mallsidan. På så sätt minskar risken för att data äventyras av en man-in-the-middle.

Du kan dessutom ange vilka standby-instanser som tillåts ansluta genom att begränsa IP-adressen för inkommande begäranden. Detta bör bidra till att garantera att ingen i intranätet kan kopiera databasen.

>[!NOTE]
>
>Vi rekommenderar att en belastningsutjämnare läggs till mellan Dispatcher och de servrar som ingår i konfigurationen för vänteläge i kylformat. Belastningsutjämnaren ska konfigureras så att den endast dirigerar användartrafik till instansen **primär**. Detta är nödvändigt för att säkerställa enhetlighet och förhindra att innehåll kopieras i standby-instansen på andra sätt än med funktionen för vänteläge i Cold.

## Skapa en väntelägesinställning för AEM tarMK {#creating-an-aem-tarmk-cold-standby-setup}

>[!CAUTION]
>
>PID för segmentnodarkivet och tjänsten Standby Store har ändrats i AEM 6.3 jämfört med tidigare versioner enligt följande:
>
>* från org.apache.jackrabbit.oak.**plugins**.segment.standby.store.StandbyStoreService to org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService
>* från org.apache.jackrabbit.oak.**plugins**.segment.SegmentNodeStoreService to org.apache.jackrabbit.oak.segment.SegmentNodeStoreService
>
>Gör nödvändiga konfigurationsjusteringar så att de återspeglar den här ändringen.

Om du vill skapa ett kalliget TjärMK-vänteläge skapar du först standby-instanserna genom att utföra en kopia av hela installationsmappen för det primära till en ny plats. Du kan sedan starta varje instans med ett körningsläge som anger dess roll ( `primary` eller `standby`).

Nedan visas proceduren som måste följas för att skapa en konfiguration med en master- och en standby-instans:

1. Installera AEM.

1. Stäng instansen och kopiera installationsmappen till den plats där instansen av kallstart körs. Även om du kör från olika datorer måste du ge varje mapp ett beskrivande namn (som *aem-primär* eller *aem-standby*) för att skilja på instanserna.
1. Gå till installationsmappen för den primära instansen och:

   1. Kontrollera och ta bort tidigare OSGi-konfigurationer som du har under `aem-primary/crx-quickstart/install`

   1. Skapa en mapp med namnet `install.primary` under `aem-primary/crx-quickstart/install`

   1. Skapa de konfigurationer som krävs för det önskade nodarkivet och datalagret under `aem-primary/crx-quickstart/install/install.primary`
   1. Skapa en fil med namnet `org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService.config` på samma plats och konfigurera den därefter. Mer information om konfigurationsalternativen finns i [Konfiguration](/help/sites-deploying/tarmk-cold-standby.md#configuration).

   1. Om du använder en AEM tarMK-instans med ett externt datalager skapar du en mapp med namnet `crx3` under `aem-primary/crx-quickstart/install` med namnet `crx3`

   1. Placera konfigurationsfilen för datalagret i mappen `crx3`.

   Om du till exempel kör en AEM tarMK-instans med ett externt File Data Store behöver du följande konfigurationsfiler:

   * `aem-primary/crx-quickstart/install/install.primary/org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`
   * `aem-primary/crx-quickstart/install/install.primary/org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService.config`
   * `aem-primary/crx-quickstart/install/crx3/org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config`

   Här nedan hittar du exempelkonfigurationer för den primära instansen:

   **Exempel på** **org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config**

   ```xml
   org.apache.sling.installer.configuration.persist=B"false"
   customBlobStore=B"true"
   standby=B"false"
   ```

   **Exempel på org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService.config**

   ```xml
   org.apache.sling.installer.configuration.persist=B"false"
   mode="primary"
   port=I"8023"
   ```

   **Exempel på org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config**

   ```xml
   org.apache.sling.installer.configuration.persist=B"false"
   path="./crx-quickstart/repository/datastore"
   minRecordLength=I"16384"
   ```

1. Starta det primära läget och kontrollera att du anger det primära körningsläget:

   ```shell
   java -jar quickstart.jar -r primary,crx3,crx3tar
   ```

1. Skapa en Apache Sling Logging-loggare för paketet **org.apache.jackrabbit.oak.segment**. Ställ in loggnivån på &quot;Felsök&quot; och peka loggutdata på en separat loggfil, som */logs/tarmk-coldstandby.log*. Mer information finns i [Loggning](/help/sites-deploying/configure-logging.md).
1. Gå till platsen för instansen **standby** och starta den genom att köra burken.
1. Skapa samma loggningskonfiguration som för den primära. Stoppa sedan instansen.
1. Förbered sedan standby-instansen. Du kan göra detta genom att utföra samma steg som för den primära instansen:

   1. Ta bort alla filer som du har under `aem-standby/crx-quickstart/install`.
   1. Skapa en mapp med namnet `install.standby` under `aem-standby/crx-quickstart/install`

   1. Skapa två konfigurationsfiler med namnet:

      * `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`
      * `org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService.config`

   1. Skapa en mapp med namnet `crx3` under `aem-standby/crx-quickstart/install`

   1. Skapa datalagerkonfigurationen och placera den under `aem-standby/crx-quickstart/install/crx3`. I det här exemplet är filen som du måste skapa:

      * org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config

   1. Redigera filerna och skapa de konfigurationer som behövs.

   Nedan visas exempel på konfigurationsfiler för en vanlig standby-instans:

   **Exempel på org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config**

   ```xml
   org.apache.sling.installer.configuration.persist=B"false"
   name="Oak-Tar"
   service.ranking=I"100"
   standby=B"true"
   customBlobStore=B"true"
   ```

   **Exempel på org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService.config**

   ```xml
   org.apache.sling.installer.configuration.persist=B"false"
   mode="standby"
   primary.host="127.0.0.1"
   port=I"8023"
   secure=B"false"
   interval=I"5"
   standby.autoclean=B"true"
   ```

   **Exempel på org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config**

   ```xml
   org.apache.sling.installer.configuration.persist=B"false"
   path="./crx-quickstart/repository/datastore"
   minRecordLength=I"16384"
   ```

1. Starta instansen **standby** med vänteläget:

   ```xml
   java -jar quickstart.jar -r standby,crx3,crx3tar
   ```

Tjänsten kan även konfigureras via webbkonsolen genom att:

1. Gå till webbkonsolen på: *https://serveraddress:serverport/system/console/configMgr*
1. Söker efter en tjänst som heter **Apache Jackrabbit Oak Segment Stold Standby Service** och dubbelklickar på den för att redigera inställningarna.
1. Spara inställningarna och starta om instanserna så att de nya inställningarna kan börja gälla.

>[!NOTE]
>
>Du kan när som helst kontrollera rollen för en instans genom att kontrollera om körningslägena **primär** eller **standby** finns i webbkonsolen för delningsinställningar.
>
>Detta kan du göra genom att gå till *https://localhost:4502/system/console/status-slingsettings* och kontrollera raden **&quot;Körningslägen&quot;**.

## Första synkroniseringen {#first-time-synchronization}

När beredningen är klar och standby startas för första gången, uppstår en kraftig nätverkstrafik mellan instanserna när standby-läget fångar upp till primärt. Du kan läsa loggarna för att se synkroniseringens status.

I vänteläge *tarmk-coldstandby.log* kan du se följande poster:

```xml
    *DEBUG* [defaultEventExecutorGroup-2-1] org.apache.jackrabbit.oak.segment.standby.store.StandbyStore trying to read segment ec1f739c-0e3c-41b8-be2e-5417efc05266

    *DEBUG* [nioEventLoopGroup-3-1] org.apache.jackrabbit.oak.segment.standby.codec.SegmentDecoder received type 1 with id ec1f739c-0e3c-41b8-be2e-5417efc05266 and size 262144

    *DEBUG* [defaultEventExecutorGroup-2-1] org.apache.jackrabbit.oak.segment.standby.store.StandbyStore got segment ec1f739c-0e3c-41b8-be2e-5417efc05266 with size 262144

    *DEBUG* [defaultEventExecutorGroup-2-1] org.apache.jackrabbit.oak.segment.file.TarWriter Writing segment ec1f739c-0e3c-41b8-be2e-5417efc05266 to /mnt/crx/author/crx-quickstart/repository/segmentstore/data00016a.tar
```

I väntelägets *error.log* bör du se en post som den här:

```xml
*INFO* [FelixStartLevel] org.apache.jackrabbit.oak.segment.standby.store.StandbyStoreService started standby sync with 10.20.30.40:8023 at 5 sec.
```

I ovanstående loggutdrag är *10.20.30.40* den primära IP-adressen.

I **primär** *target-coldstandby.log* visas poster som:

```xml
    *DEBUG* [nioEventLoopGroup-3-2] org.apache.jackrabbit.oak.segment.standby.store.CommunicationObserver got message 's.d45f53e4-0c33-4d4d-b3d0-7c552c8e3bbd' from client c7a7ce9b-1e16-488a-976e-627100ddd8cd

    *DEBUG* [nioEventLoopGroup-3-2] org.apache.jackrabbit.oak.segment.standby.server.StandbyServerHandler request segment id d45f53e4-0c33-4d4d-b3d0-7c552c8e3bbd

    *DEBUG* [nioEventLoopGroup-3-2] org.apache.jackrabbit.oak.segment.standby.server.StandbyServerHandler sending segment d45f53e4-0c33-4d4d-b3d0-7c552c8e3bbd to /10.20.30.40:34998

    *DEBUG* [nioEventLoopGroup-3-2] org.apache.jackrabbit.oak.segment.standby.store.CommunicationObserver did send segment with 262144 bytes to client c7a7ce9b-1e16-488a-976e-627100ddd8cd
```

I det här fallet är &quot;klienten&quot; som nämns i loggen **standby** -instansen.

När de här posterna inte längre visas i loggen kan du anta att synkroniseringsprocessen är slutförd.

Ovanstående poster visar att avsökningsfunktionen fungerar som den ska, men det är ofta användbart att förstå om det finns data som synkroniseras när avsökningen sker. Om du vill göra det söker du efter poster som följande:

```xml
*DEBUG* [defaultEventExecutorGroup-156-1] org.apache.jackrabbit.oak.segment.file.TarWriter Writing segment 3a03fafc-d1f9-4a8f-a67a-d0849d5a36d5 to /<<CQROOTDIRECTORY>>/crx-quickstart/repository/segmentstore/data00014a.tar
```

När du kör med ett icke-delat `FileDataStore` bekräftar meddelanden som följande att binära filer överförs korrekt:

```xml
*DEBUG* [nioEventLoopGroup-228-1] org.apache.jackrabbit.oak.segment.standby.codec.ReplyDecoder received blob with id eb26faeaca7f6f5b636f0ececc592f1fd97ea1a9#169102 and size 169102
```

### Konfiguration {#configuration}

Följande OSGi-inställningar är tillgängliga för tjänsten Cold Standby:

* **Beständig konfiguration:** Om det här alternativet är aktiverat lagras konfigurationen i databasen i stället för de traditionella OSGi-konfigurationsfilerna. Adobe rekommenderar att du låter den här inställningen vara inaktiverad i produktionssystem så att den primära konfigurationen inte hämtas i vänteläge.

* **Läge (`mode`):** Detta väljer körningsläget för instansen.

* **Port (port):** porten som ska användas för kommunikation. Standardvärdet är `8023`.

* **Primär värd (`primary.host`):** - den primära instansens värd. Den här inställningen gäller endast för vänteläge.
* **Synkroniseringsintervall (`interval`):** - Den här inställningen bestämmer intervallet mellan synkroniseringsbegäran och gäller bara för standby-instansen.

* **Tillåtna IP-intervall (`primary.allowed-client-ip-ranges`):** - IP-intervall som det primära IP-intervallet tillåter anslutningar från.
* **Säker (`secure`):** Aktivera SSL-kryptering. Om du vill använda den här inställningen måste den vara aktiverad för alla instanser.
* **Timeout för vänteläsning (`standby.readtimeout`):** Timeout för begäranden som utfärdas från standby-instansen i millisekunder. Standardvärdet är 60000 (en minut).

* **Standby Automatic Cleanup (`standby.autoclean`):** Anropa rensningsmetoden om storleken på butiken ökar under en synkroniseringscykel.

>[!NOTE]
>
>Adobe rekommenderar att det primära ID:t och vänteläget har olika databas-ID:n för att de ska kunna identifieras separat för tjänster som Offloading.
>
>Det bästa sättet att se till att detta täcks är att ta bort *sling.id* i vänteläge och starta om instansen.

## Redundansprocedurer {#failover-procedures}

Om den primära instansen av någon anledning inte fungerar kan du ange att en av standby-instanserna ska ta rollen som primär genom att ändra startkörningsläget enligt nedan:

>[!NOTE]
>
>Redigera konfigurationsfilerna så att de matchar inställningarna som används för den primära instansen.

1. Gå till den plats där standby-instansen är installerad och stoppa den.

1. Om du har konfigurerat en belastningsutjämnare med konfigurationen kan du ta bort den primära från belastningsutjämnarens konfiguration nu.
1. Säkerhetskopiera mappen `crx-quickstart` från installationsmappen i vänteläge. Den kan användas som utgångspunkt när du skapar ett nytt vänteläge.

1. Starta om instansen med körningsläget `primary`:

   ```shell
   java -jar quickstart.jar -r primary,crx3,crx3tar
   ```

1. Lägg till den nya primära till belastningsutjämnaren.
1. Skapa och starta en ny standby-instans. Mer information finns i proceduren ovan om [Skapa en väntelägesinställning för AEM tarMK ](/help/sites-deploying/tarmk-cold-standby.md#creating-an-aem-tarmk-cold-standby-setup).

## Använda snabbkorrigeringar i en konfiguration för vänteläge i kallt format {#applying-hotfixes-to-a-cold-standby-setup}

Det rekommenderade sättet att tillämpa snabbkorrigeringar i ett kallt vänteläge är att installera dem i den primära instansen och sedan klona dem i en ny kall standby-instans med snabbkorrigeringarna installerade.

Du kan göra detta genom att följa stegen nedan:

1. Stoppa synkroniseringsprocessen på den kalla standby-instansen genom att gå till JMX-konsolen och använda **org.apache.jackrabbit.oak: Status (&quot;Standby&quot;)**bean. Mer information om hur du gör detta finns i avsnittet [Övervakning](#monitoring).
1. Stoppa kallstartsinstansen.
1. Installera snabbkorrigeringen på den primära instansen. Mer information om hur du installerar en snabbkorrigering finns i [Arbeta med paket](/help/sites-administering/package-manager.md).
1. Testa instansen efter problem efter installationen.
1. Ta bort instansen av det kalla vänteläget genom att ta bort installationsmappen.
1. Stoppa den primära instansen och klona den genom att utföra en kopia av hela installationsmappen i filsystemet till platsen för det kalla vänteläget.
1. Konfigurera om den nya klonen så att den fungerar som en instans i kallt vänteläge. Se [Skapa en väntelägesinställning för AEM tarMK Cold.](/help/sites-deploying/tarmk-cold-standby.md#creating-an-aem-tarmk-cold-standby-setup)
1. Starta både den primära instansen och kallstartsinstansen.

## Övervakning {#monitoring}

Funktionen visar information med JMX eller MBeans. Detta gör att du kan inspektera det aktuella läget för standby och master med [JMX-konsolen](/help/sites-administering/jmx-console.md). Informationen finns i MBean på `type org.apache.jackrabbit.oak:type="Standby"`med namnet `Status`.

**Standby**

Om du observerar en standby-instans visar du en nod. ID:t är vanligtvis ett generiskt UUID.

Den här noden har fem skrivskyddade attribut:

* `Running:` booleskt värde som anger om synkroniseringsprocessen körs eller inte.

* `Mode:`-klient: följt av det UUID som används för att identifiera instansen. Detta UUID ändras varje gång konfigurationen uppdateras.

* `Status:` en textbeteckning för det aktuella läget (som `running` eller `stopped`).

* `FailedRequests:`antalet efterföljande fel.
* `SecondsSinceLastSuccess:` antalet sekunder sedan den senaste kommunikationen med servern slutfördes. `-1` visas om ingen kommunikation har utförts.

Det finns också tre anropbara metoder:

* `start():` startar synkroniseringsprocessen.
* `stop():` stoppar synkroniseringsprocessen.
* `cleanup():` kör rensningsåtgärden i vänteläge.

**Primär**

När du observerar den primära informationen visas viss allmän information med hjälp av ett MBean vars ID-värde är det portnummer som standbytjänsten tarMK använder (8023 som standard). De flesta metoder och attribut är desamma som i vänteläge, men vissa skiljer sig åt:

* `Mode:` visar alltid värdet `primary`.

Dessutom kan information för upp till tio klienter (väntelägesinstanser) som är anslutna till mallen hämtas. MBean-ID:t är instansens UUID. Det finns inga anropbara metoder för dessa MBeans, men några användbara skrivskyddade attribut:

* `Name:` klientens ID.
* `LastSeenTimestamp:` tidsstämpeln för den senaste begäran i en textbeteckning.
* `LastRequest:` klientens senaste begäran.
* `RemoteAddress:` klientens IP-adress.
* `RemotePort:` den port klienten använde för den senaste begäran.
* `TransferredSegments:` det totala antalet segment som har överförts till den här klienten.
* `TransferredSegmentBytes:`det totala antalet byte som har överförts till klienten.

## Underhåll av vänteläge, kall {#cold-standby-repository-maintenance}

### Revision Cleanup {#revision-clean}

>[!NOTE]
>
>Om du kör [rensning av onlineändringar](/help/sites-deploying/revision-cleanup.md) på den primära instansen behövs inte den manuella proceduren som beskrivs nedan. Om du använder onlinerevisionsrensning utförs `cleanup ()`-åtgärden i standby-instansen automatiskt.

>[!NOTE]
>
>Kör inte någon rensning av offlineversioner i vänteläge. Den behövs inte och minskar inte storleken på segmentbutiken.

Adobe rekommenderar regelbundet underhåll för att förhindra alltför stor databastillväxt över tid. Följ stegen nedan om du vill utföra underhåll i vänteläge manuellt:

1. Stoppa standbyprocessen i standby-instansen genom att gå till JMX-konsolen och använda **org.apache.jackrabbit.oak: Status (&quot;Standby&quot;)** -böna. Mer information om hur du gör detta finns i avsnittet ovan [Övervakning](/help/sites-deploying/tarmk-cold-standby.md#monitoring).

1. Stoppa AEM primära instans.
1. Kör Oak-komprimeringsverktyget på den primära instansen. Mer information finns i [Underhålla databasen](/help/sites-deploying/storage-elements-in-aem-6.md#maintaining-the-repository).
1. Starta den primära instansen.
1. Starta standby-processen i standby-instansen med samma JMX-böna som i det första steget.
1. Se loggarna och vänta tills synkroniseringen är klar. Det är möjligt att det för närvarande sker en betydande tillväxt i beredskapslagringsplatsen.
1. Kör åtgärden `cleanup()` i standby-instansen med samma JMX-böna som i det första steget.

Det kan ta längre tid än vanligt för standby-instansen att slutföra synkroniseringen med den primära, eftersom offlinekomprimeringen effektivt skriver om databashistoriken, vilket gör att beräkning av ändringarna i databaserna tar längre tid. När den här processen har slutförts är storleken på databasen i vänteläge ungefär lika stor som databasen på den primära databasen.

Som ett alternativ kan den primära databasen kopieras till vänteläge manuellt efter att du har kört komprimering på den primära, vilket innebär att vänteläget återskapas varje gång komprimeringen körs.

### Skräpinsamling för datalager {#data-store-garbage-collection}

Det är viktigt att du kör skräpinsamlingen på fildatalagrets instanser då och då, annars finns det borttagna binärfiler kvar i filsystemet, som till slut fyller i enheten. Så här kör du skräpinsamlingen:

1. Kör underhåll av kallt vänteläge enligt beskrivningen i avsnittet [ovan](/help/sites-deploying/tarmk-cold-standby.md#cold-standby-repository-maintenance).
1. När underhållsprocessen har slutförts och instanserna startats om:

   * Kör skräpinsamlingen i datalagret med den relevanta JMX-böljan enligt beskrivningen i [Skräpinsamlingen i datalagret via JMX-konsolen](/help/sites-administering/data-store-garbage-collection.md#running-data-store-garbage-collection-via-the-jmx-console).
   * I vänteläge är skräpinsamlingen i datalagret bara tillgänglig via **BlobGarbageCollection** MBean - `startBlobGC()`. **RepositoryManagement** MBean är inte tillgängligt i vänteläge.

   >[!NOTE]
   >
   >Om du inte använder ett delat datalager kör du skräpinsamlingen först på primär plats och sedan i vänteläge.
