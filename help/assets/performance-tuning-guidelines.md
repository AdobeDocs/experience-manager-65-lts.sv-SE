---
title: Prestandajustering  [!DNL Assets].
description: Förslag och vägledning om  [!DNL Experience Manager] konfiguration, ändringar av maskinvara, programvara och nätverkskomponenter för att ta bort flaskhalsar och optimera prestanda för  [!DNL Experience Manager Assets].
contentOwner: AG
mini-toc-levels: 1
role: Architect, Admin
feature: Asset Management
solution: Experience Manager, Experience Manager Assets
exl-id: 43079a69-cd12-4853-9fff-96f9d177987a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2662'
ht-degree: 0%

---

<!-- TBD: Get reviewed by engineering. -->

# Prestandajusteringsguide för [!DNL Adobe Experience Manager Assets] {#assets-performance-tuning-guide}

En [!DNL Experience Manager Assets]-installation innehåller flera maskinvaru-, programvaru- och nätverkskomponenter. Beroende på ditt driftsättningsscenario kan du behöva specifika konfigurationsändringar för maskinvara, programvara och nätverkskomponenter för att ta bort flaskhalsar i prestandan.

Genom att identifiera och följa vissa riktlinjer för optimering av maskinvara och programvara kan du dessutom skapa en bra grund som gör att distributionen av [!DNL Experience Manager Assets] kan uppfylla förväntningarna på prestanda, skalbarhet och tillförlitlighet.

Dåliga prestanda i [!DNL Experience Manager Assets] kan påverka användarupplevelsen kring interaktiva prestanda, bearbetning av resurser, hämtningshastighet och andra områden.

Prestandaoptimering är en grundläggande uppgift som du utför innan du fastställer målvärden för ett projekt.

Här är några viktiga fokusområden som du kan använda för att identifiera och åtgärda prestandaproblem innan de påverkar användarna.

## Plattform {#platform}

Experience Manager stöds på flera plattformar, men Adobe har funnit det bästa stödet för inbyggda verktyg i Linux och Windows, vilket ger optimala prestanda och enklare implementering. Du bör helst distribuera ett 64-bitars operativsystem för att uppfylla de höga minneskraven för en [!DNL Experience Manager Assets]-distribution. Precis som med andra Experience Manager-distributioner bör du implementera tarMK där det är möjligt. Även om StjärmMK inte kan skalas bortom en enda författarinstans, fungerar det bättre än MongoMK. Du kan lägga till instanser för TjärMK-avlastning för att öka arbetsflödets bearbetningskraft för din [!DNL Experience Manager Assets]-distribution.

### Tillfällig mapp {#temp-folder}

Om du vill förbättra överföringstiderna använder du högpresterande lagringsutrymme för den tillfälliga Java-katalogen. I Linux och Windows kan en RAM-enhet eller SSD användas. I molnbaserade miljöer kan en motsvarande typ av höghastighetslagring användas. I Amazon EC2 kan till exempel en [tillfällig enhet](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) användas för den tillfälliga mappen.

Om servern har tillräckligt med minne konfigurerar du en RAM-enhet. Kör följande kommandon i Linux för att skapa en 8 GB RAM-enhet:

```shell
mkfs -q /dev/ram1 800000
 mkdir -p /mnt/aem-tmp
 mount /dev/ram1 /mnt/aem-tmp
 df -H | grep aem-tmp
```

I Windows använder du en drivrutin från tredje part för att skapa en RAM-enhet eller bara använda lagring med höga prestanda, till exempel SSD.

När den tillfälliga volymen med höga prestanda är klar anger du JVM-parametern `-Djava.io.tmpdir`. Du kan till exempel lägga till JVM-parametern nedan till variabeln `CQ_JVM_OPTS` i `bin/start`-skriptet för [!DNL Experience Manager]:

`-Djava.io.tmpdir=/mnt/aem-tmp`

## Java-konfiguration {#java-configuration}

### Java-version {#java-version}

Adobe rekommenderar att [!DNL Experience Manager Assets] distribueras på Java 8 för optimala prestanda.

<!-- TBD: Link to the latest official word around Java.
-->

### JVM-parametrar {#jvm-parameters}

Ange följande JVM-parametrar:

* `-XX:+UseConcMarkSweepGC`
* `-Doak.queryLimitInMemory`=500000
* `-Doak.queryLimitReads`=100000
* `-Dupdate.limit`=250000
* `-Doak.fastQuerySize`=true

## Datalagring och minneskonfiguration {#data-store-and-memory-configuration}

### Konfiguration av fillagring {#file-data-store-configuration}

Du bör separera datalagret från segmentlagret för alla [!DNL Experience Manager Assets]-användare. Dessutom kan konfigurering av parametrarna `maxCachedBinarySize` och `cacheSizeInMB` bidra till att maximera prestanda. Ange `maxCachedBinarySize` till den minsta filstorleken som kan sparas i cachen. Ange storleken på den minnescache som ska användas för datalagret i `cacheSizeInMB`. Adobe rekommenderar att du anger det här värdet mellan 2 och 10 procent av den totala stackstorleken. Inläsnings-/prestandatestning kan dock hjälpa till att fastställa den idealiska inställningen.

### Konfigurera maximal storlek för buffrad bildcache {#configure-the-maximum-size-of-the-buffered-image-cache}

När du överför stora mängder resurser till [!DNL Adobe Experience Manager] bör du minska den konfigurerade maxstorleken för buffrat bildcache för att undvika oväntade ökningar i minnesanvändningen och för att förhindra att JVM misslyckas med OutOfMemoryErrors. Tänk dig ett exempel på att du har ett system med en högsta heap (- `Xmx` param) på 5 GB, en Oak BlobCache som är inställd på 1 GB och dokumentcache som är inställd på 2 GB. I det här fallet tar den buffrade cachen upp till 1,25 GB och minne, vilket innebär att endast 0,75 GB minne återstår för oväntade toppar.

Konfigurera den buffrade cachestorleken i OSGi-webbkonsolen. Vid `https://host:port/system/console/configMgr/com.day.cq.dam.core.impl.cache.CQBufferedImageCache` anger du egenskapen `cq.dam.image.cache.max.memory` i byte. 1073741824 är till exempel 1 GB (1 024 x 1 024 x 1 024 = 1 GB).

Om du använder en `sling:osgiConfig`-nod för att konfigurera den här egenskapen från Experience Manager 6.1 SP1 måste du ange datatypen till Long. Mer information finns i [CQBufferedImageCache förbrukar heap under överföring av tillgångar](https://helpx.adobe.com/experience-manager/kb/cqbufferedimagecache-consumes-heap-during-asset-uploads.html).

### Gemensamma datalager {#shared-data-stores}

Implementering av ett S3- eller delat fildatalager kan bidra till att spara diskutrymme och öka nätverkets genomströmning i storskaliga implementeringar. Mer information om för- och nackdelar med att använda ett delat datalager finns i [Assets storleksguide](/help/assets/assets-sizing-guide.md).

### S3-datalager {#s-data-store}

Följande konfiguration för S3-datalagret ( `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.cfg`) hjälpte Adobe att extrahera 12,8 TB binära stora objekt (BLOB) från ett befintligt arkiv till ett S3-datalager på en kunds plats:

```conf
accessKey=<snip>
 secretKey=<snip>
 s3Bucket=<snip>
 s3Region=us-standard
 s3EndPoint=<a href="https://s3.amazonaws.com/">s3.amazonaws.com</a>
 connectionTimeout=120000
 socketTimeout=120000
 maxConnections=80
 writeThreads=60
 concurrentUploadsThreads=30
 asyncUploadLimit=30
 maxErrorRetry=1000
 path=/opt/author/crx-quickstart/repository/datastore
 s3RenameKeys=false
 s3Encryption=SSE_S3
 proactiveCaching=true
 uploadRetries=1000
 migrateFailuresCount=400
```

## Nätverksoptimering {#network-optimization}

Adobe rekommenderar att du aktiverar HTTPS eftersom många företag har brandväggar som fångar upp HTTP-trafik, vilket påverkar överföringar negativt och skadar filer. För stora filöverföringar måste användarna ha kabelanslutna anslutningar till nätverket eftersom ett WiFi-nätverk snabbt blir mättat. Mer information om hur du identifierar nätverksflaskhalsar finns i [Assets storleksguide](/help/assets/assets-sizing-guide.md). Information om hur du utvärderar nätverksprestanda genom att analysera nätverkstopologi finns i [Assets-nätverkshänsyn](/help/assets/assets-network-considerations.md).

Din nätverksoptimeringsstrategi är i första hand beroende av den tillgängliga bandbredden och belastningen på din [!DNL Experience Manager]-instans. Gemensamma konfigurationsalternativ, inklusive brandväggar och proxies, kan förbättra nätverkets prestanda. Här följer några viktiga saker att tänka på:

* Beroende på vilken instanstyp du har (liten, måttlig, stor) kontrollerar du att du har tillräcklig nätverksbandbredd för din Experience Manager-instans. Lämplig bandbreddsallokering är särskilt viktig om [!DNL Experience Manager] ligger på AWS.
* Om din [!DNL Experience Manager]-instans finns på AWS kan du dra nytta av en flexibel skalförändringsprincip. Överför instansen om användarna förväntar sig hög belastning. Minska storleken för måttlig/låg belastning.
* HTTPS: De flesta användare har brandväggar som krypterar HTTP-trafik, vilket kan påverka överföringen av filer negativt eller till och med skada filer under överföringen.
* Stora filöverföringar: Se till att användarna har kabelanslutna anslutningar till nätverket (WiFi-anslutningar blir snabbt mättade).

## Arbetsflöden {#workflows}

### Övergående arbetsflöden {#transient-workflows}

Ange arbetsflödet [!UICONTROL DAM Update Asset] till Transient om det är möjligt. Inställningen minskar avsevärt de allmänna kostnader som krävs för att bearbeta arbetsflöden, eftersom arbetsflöden i det här fallet inte behöver passera genom de normala spårnings- och arkiveringsprocesserna.

1. Navigera till `/miscadmin` i [!DNL Experience Manager]-distributionen på `https://[aem_server]:[port]/miscadmin`.

1. Expandera **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]** > **[!UICONTROL dam]**.

1. Öppna **[!UICONTROL DAM Update Asset]**. Gå till fliken **[!UICONTROL Page]** på den flytande verktygspanelen och klicka sedan på **[!UICONTROL Page Properties]**.

1. Markera **[!UICONTROL Transient Workflow]** och klicka på **[!UICONTROL OK]**.

   >[!NOTE]
   >
   >Vissa funktioner har inte stöd för tillfälliga arbetsflöden. Om din [!DNL Assets]-distribution kräver dessa funktioner ska du inte konfigurera tillfälliga arbetsflöden.

Om tillfälliga arbetsflöden inte kan användas kör du regelbundet arbetsflödesrensning för att ta bort arkiverade [!UICONTROL DAM Update Asset]-arbetsflöden, så att systemprestanda inte försämras.

Vanligtvis kör du rensningsarbetsflödena varje vecka. I resurskrävande scenarier, till exempel vid omfattande tillgångsinmatning, kan du dock köra det oftare.

Om du vill konfigurera rensning av arbetsflöden lägger du till en ny Adobe Granite Workflow Renge-konfiguration via OSGi-konsolen. Konfigurera och schemalägg arbetsflödet som en del av veckounderhållsperioden.

Om tömningen är för lång så tar det för lång tid. Därför bör du se till att rensningsjobben är fullständiga för att undvika situationer där rensningsarbetsflödena misslyckas på grund av det stora antalet arbetsflöden.

Om du till exempel har kört flera icke-tillfälliga arbetsflöden (som skapar arbetsflödesinstansnoder) kan du köra [ACS AEM Commons Workflow Remover](https://adobe-consulting-services.github.io/acs-aem-commons/features/workflow-remover.html) på ad hoc-basis. Det tar bort överflödiga, slutförda arbetsflödesinstanser direkt i stället för att vänta på att schemaläggaren Adobe Granite Workflow Renge ska köras.

### Maximalt antal parallella jobb {#maximum-parallel-jobs}

Som standard kör [!DNL Experience Manager] ett maximalt antal parallella jobb som är lika med antalet processorer på servern. Problemet med den här inställningen är att under perioder med hög belastning används alla processorer av [!UICONTROL DAM Update Asset] arbetsflöden, vilket gör att gränssnittets svarstid minskar och hindrar [!DNL Experience Manager] från att köra andra processer som skyddar serverns prestanda och stabilitet. Det är en god vana att ange det här värdet till hälften av de processorer som är tillgängliga på servern genom att utföra följande steg:

1. Åtkomst till `https://[aem_server]:[port]/system/console/slingevent` för [!DNL Experience Manager] Author.

1. Klicka på **[!UICONTROL Edit]** i varje arbetsflödeskö som är relevant för implementeringen, till exempel **[!UICONTROL Granite Transient Workflow Queue]**.

1. Uppdatera värdet för **[!UICONTROL Maximum Parallel Jobs]** och klicka på **[!UICONTROL Save]**.

Att ställa in en kö på hälften av de tillgängliga processorerna är en användbar lösning att börja med. Du kan dock behöva öka eller minska det här antalet för att få maximal genomströmning och justera det efter miljö. Det finns separata köer för tillfälliga och icke-tillfälliga arbetsflöden och andra processer, till exempel externa arbetsflöden. Om flera köer är inställda på 50 % av processorerna aktiva samtidigt kan systemet snabbt bli överbelastat. De köer som används ofta varierar mycket mellan olika implementeringar. Därför kan du behöva konfigurera dem noggrant för maximal effektivitet utan att ge avkall på serverstabiliteten.

### DAM-uppdateringskonfiguration {#dam-update-asset-configuration}

Arbetsflödet [!UICONTROL DAM Update Asset] innehåller en komplett serie steg som är konfigurerade för åtgärder, till exempel Dynamic Media PTIFF-generering och [!DNL Adobe InDesign Server]-integrering. De flesta användare behöver dock inte utföra flera av dessa steg. Adobe rekommenderar att du skapar en anpassad kopia av arbetsflödesmodellen [!UICONTROL DAM Update Asset] och tar bort alla onödiga steg. I det här fallet ska du uppdatera startarna för [!UICONTROL DAM Update Asset] så att de pekar på den nya modellen.

Om du kör arbetsflödet [!UICONTROL DAM Update Asset] intensivt kan du öka storleken på fildatalagret avsevärt. Resultaten från ett experiment som utförts av Adobe har visat att datastorleken kan öka med ungefär 400 GB om cirka 500 arbetsflöden utförs inom 8 timmar.

Det är en tillfällig ökning och datalagret återställs till den ursprungliga storleken när du har kört skräpinsamlingsaktiviteten för datalagret.

Vanligtvis körs skräpinsamlingsaktiviteten för datalager varje vecka tillsammans med andra schemalagda underhållsaktiviteter.

Om du har begränsat diskutrymme och kör [!UICONTROL DAM Update Asset] arbetsflöden intensivt bör du överväga att schemalägga skräpinsamlingsaktiviteten oftare.

#### Generering av rendering vid körning {#runtime-rendition-generation}

Kunderna använder bilder av olika storlek och format på sin webbplats eller för att distribuera dem till affärspartners. Eftersom varje återgivning ökar utrymmet i databasen rekommenderar Adobe att du använder den här funktionen med gott omdöme. Om du vill minska mängden resurser som behövs för att bearbeta och lagra bilder kan du generera dessa bilder vid körning i stället för som återgivningar vid importen.

Många webbplatskunder implementerar en bildservett som ändrar storlek på och beskär bilder när de begärs, vilket medför ytterligare belastning på publiceringsinstansen. Så länge dessa bilder kan cachas kan utmaningen dock mildras.

Ett annat sätt är att använda Dynamic Media-tekniken för att helt och hållet överföra bildmanipulering. Dessutom kan du distribuera Brand Portal som inte bara tar över ansvaret för återgivningsgenerering från infrastrukturen [!DNL Experience Manager], utan även hela publiceringsnivån.

#### ImageMagick {#imagemagick}

Om du anpassar arbetsflödet [!UICONTROL DAM Update Asset] för att generera återgivningar med ImageMagick rekommenderar Adobe att du ändrar filen `policy.xml` på `/etc/ImageMagick/`. Som standard använder ImageMagick hela det tillgängliga diskutrymmet på operativsystemsvolymen och det tillgängliga minnet. Gör följande konfigurationsändringar i avsnittet `policymap` i `policy.xml` för att begränsa de här resurserna.

```xml
<policymap>
  <!-- <policy domain="system" name="precision" value="6"/> -->
  <policy domain="resource" name="temporary-path" value="/ephemeral0/imagemagick_tmp"/>
  <policy domain="resource" name="memory" value="1000MiB"/>
  <policy domain="resource" name="map" value="1000MiB"/>
  <!-- <policy domain="resource" name="area" value="1gb"/> -->
  <policy domain="resource" name="disk" value="10000MiB"/>
  <!-- <policy domain="resource" name="file" value="768"/> -->
  <policy domain="resource" name="thread" value="1"/>
  <policy domain="resource" name="throttle" value="50"/>
  <!-- <policy domain="resource" name="time" value="3600"/> -->
</policymap>
```

Dessutom anger du sökvägen till ImageMagick:s tillfälliga mapp i filen `configure.xml` (eller genom att ange miljövariabeln `MAGICK_TEMPORARY_PATH`) till en diskpartition som har tillräckligt med utrymme och IOPS.

>[!CAUTION]
>
>En felaktig konfiguration kan göra servern instabil om ImageMagick använder allt tillgängligt diskutrymme. De principändringar som krävs för att bearbeta stora filer med ImageMagick kan påverka prestanda för [!DNL Experience Manager]. Mer information finns i [Installera och konfigurera ImageMagick](/help/assets/best-practices-for-imagemagick.md).

>[!NOTE]
>
>ImageMagick `policy.xml`- och `configure.xml`-filerna är tillgängliga på `/usr/lib64/ImageMagick-&#42;/config/` i stället för på `/etc/ImageMagick/`.Mer information om var konfigurationsfilerna finns i [ImageMagick-dokumentationen](https://www.imagemagick.org/script/resources.php).

Om du använder [!DNL Experience Manager] på Adobe Managed Services (AMS) kan du kontakta Adobe kundsupport om du tänker bearbeta många stora PSD- eller PSB-filer. Samarbeta med Adobe kundsupportrepresentant för att implementera de bästa metoderna för driftsättningen av AMS och för att välja de bästa möjliga verktygen och modellerna för Adobe egna format. [!DNL Experience Manager] kan inte bearbeta PSB-filer med hög upplösning som är större än 30000 x 23000 pixlar.

### XMP writeback {#xmp-writeback}

XMP tillbakaskrivning uppdaterar den ursprungliga resursen när metadata ändras i [!DNL Experience Manager], vilket ger följande resultat:

* Själva tillgången har ändrats
* En version av resursen skapas
* [!UICONTROL DAM Update Asset] körs mot resursen

De listade resultaten förbrukar avsevärda resurser. Därför rekommenderar Adobe att du inaktiverar XMP-tillbakaskrivning om det inte behövs. Mer information finns i [XMP-tillbakaskrivning](/help/assets/xmp-writeback.md).

Om du importerar en stor mängd metadata kan det leda till resurskrävande XMP-återskrivningsaktivitet om körningsarbetsflödesflaggan är markerad. Planera en sådan import under begränsad serveranvändning så att prestanda för andra användare inte påverkas.

## Replikering {#replication}

När du replikerar resurser till ett stort antal publiceringsinstanser, till exempel i en webbplatsimplementering, rekommenderar Adobe att du använder kedjereplikering. I det här fallet replikeras författarinstansen till en enda publiceringsinstans som i sin tur replikeras till de andra publiceringsinstanserna och frigör författarinstansen.

### Konfigurera kedjereplikering {#configure-chain-replication}

1. Välj vilken publiceringsinstans du vill använda för att länka replikeringarna till
1. På den publiceringsinstansen lägger du till replikeringsagenter som pekar på andra publiceringsinstanser
1. På var och en av dessa replikeringsagenter aktiverar du&quot;Vid mottagning&quot; på fliken&quot;Utlösare&quot;

>[!NOTE]
>
>Adobe rekommenderar inte att resurser aktiveras automatiskt. Om det behövs rekommenderar Adobe att du gör detta som det sista steget i ett arbetsflöde, vanligtvis DAM Update Asset.

## Sökindex {#search-indexes}

Installera [de senaste Service Packs](/help/release-notes/release-notes.md) och prestandarelaterade snabbkorrigeringar eftersom de ofta innehåller uppdateringar av systemindex. Se [tips för prestandajustering](https://experienceleague.adobe.com/docs/experience-manager-65-lts/assets/administer/performance-tuning-guidelines.html?lang=en) för vissa indexoptimeringar.

Skapa anpassade index för frågor som du kör ofta. Mer information finns i [metod för att analysera långsamma frågor](https://aemfaq.blogspot.com/2014/08/oak-query-log-file-analyzer-tool.html) och [skapa anpassade index](/help/sites-deploying/queries-and-indexing.md). Mer information om bästa praxis för frågor och index finns i [Bästa praxis för frågor och indexering](/help/sites-deploying/best-practices-for-queries-and-indexing.md).

### Lucene-indexkonfigurationer {#lucene-index-configurations}

Vissa optimeringar kan göras för Oak indexkonfigurationer som kan förbättra prestandan för [!DNL Experience Manager Assets]. Uppdatera indexkonfigurationerna för att förbättra omindexeringstiden:

1. Öppna CRXDe `/crx/de/index.jsp` och logga in som administrativ användare.
1. Gå till `/oak:index/lucene`.
1. Lägg till en `String[]`-egenskap `excludedPaths` med värdena `/var`, `/etc/workflow/instances` och `/etc/replication`.
1. Gå till `/oak:index/damAssetLucene`. Lägg till egenskapen `String[]` `includedPaths` med värdet `/content/dam`. Spara ändringar.

Om dina användare inte behöver göra fulltextsökning av resurser, till exempel söka igenom text i PDF-dokument, kan du inaktivera det. Du kan förbättra indexets prestanda genom att inaktivera fulltextindexering. Så här inaktiverar du textrahering av [!DNL Apache Lucene]:

1. Öppna [!UICONTROL Package Manager] i gränssnittet [!DNL Experience Manager].
1. Överför och installera det paket som finns på [disable_indexingbinarytextextraction-10.zip](assets/disable_indexingbinarytextextraction-10.zip).

### Gissa totalt {#guess-total}

När du skapar frågor som genererar stora resultatuppsättningar ska du använda parametern `guessTotal` för att undvika att använda mycket minne när du kör dem.

## Kända fel {#known-issues}

### Stora filer {#large-files}

Det finns två stora kända fel som rör stora filer i [!DNL Experience Manager]. När filer når större storlekar än 2 GB kan synkronisering med kalla väntelägen hamna i en situation där minnet är slut. I vissa fall förhindras att standby-synkronisering körs. I andra fall kraschar den primära instansen. Detta scenario gäller för alla filer i [!DNL Experience Manager] som är större än 2 GB, inklusive innehållspaket.

På samma sätt kan det ta lite tid innan filen är helt beständig från cachen till filsystemet om filen är 2 GB stor när ett delat S3-datalager används. Detta innebär att om du använder en binär replikering utan binärfiler kan det hända att binära data inte har befunnits beständiga innan replikeringen slutförs. Denna situation kan leda till problem, särskilt om datatillgängligheten är viktig.

## Prestandatestning {#performance-testing}

För varje [!DNL Experience Manager]-distribution måste du skapa ett prestandatestningssystem som snabbt kan identifiera och lösa flaskhalsar. Här är några nyckelområden att fokusera på.

### Nätverkstestning {#network-testing}

Utför följande uppgifter för alla problem med nätverkets prestanda från kunden:

* Testa nätverksprestanda inifrån kundens nätverk
* Testa nätverksprestanda inifrån Adobe. För AMS-kunder kan du arbeta med din CSE för att testa inifrån Adobe-nätverket.
* Testa nätverksprestanda från en annan åtkomstpunkt
* Genom att använda ett prestandatest för nätverk
* Testa mot dispatchern

### [!DNL Experience Manager] distributionstestning {#aem-deployment-testing}

För att minimera latens och uppnå hög genomströmning genom effektiv användning och lastdelning av CPU bör du regelbundet övervaka prestandan för din [!DNL Experience Manager]-distribution. Särskilt gäller följande:

* Kör lasttester mot distributionen [!DNL Experience Manager].
* Övervaka uppladdningsprestanda och hur användargränssnittet påverkas.

## [!DNL Experience Manager Assets] resultatchecklista och påverkan av resurshanteringsaktiviteter {#checklist}

* Gör det möjligt för HTTPS att kringgå alla HTTP-trafiksniffare på företag.
* Använd en kabelanslutning för överföring av stora resurser.
* Driftsätt med Java 8.
* Ange optimala JVM-parametrar.
* Konfigurera ett datalager i filsystemet eller ett S3-datalager.
* Inaktivera generering av underresurser. Om det är aktiverat skapar AEM arbetsflöde en separat resurs för varje sida i en flersidig resurs. Var och en av dessa sidor är en enskild resurs som förbrukar mer diskutrymme, kräver versionshantering och ytterligare arbetsflödesbearbetning. Om du inte behöver separata sidor inaktiverar du generering av delresurser och sidextrahering.
* Aktivera tillfälliga arbetsflöden.
* Justera Granite-arbetsflödesköerna för att begränsa antalet samtidiga jobb.
* Konfigurera [!DNL ImageMagick] för att begränsa resursförbrukning.
* Ta bort onödiga steg från arbetsflödet [!UICONTROL DAM Update Asset].
* Konfigurera arbetsflöde och versionsrensning.
* Optimera index med de senaste Service Pack-uppdateringarna och snabbkorrigeringarna. Kontakta Adobe kundsupport för eventuella ytterligare indexoptimeringar.
* Använd gissningTotal för att optimera frågeprestanda.
* Om du konfigurerar [!DNL Experience Manager] att identifiera filtyper från filinnehållet (genom att aktivera **[!UICONTROL Day CQ DAM Mime Type Service]** i **[!UICONTROL AEM Web Console]**), överför många filer samtidigt under icke-toppade timmar eftersom det är resurskrävande.
