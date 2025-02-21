---
title: Konfigurera och felsöka ett AEM Forms på JEE-serverkluster
description: Lär dig hur du konfigurerar och felsöker ett Adobe Experience Manager (AEM) Forms på JEE-serverkluster.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '3943'
ht-degree: 0%

---

# Konfigurera och felsöka ett AEM Forms i JEE-serverkluster {#configuring-troubleshooting-aem-forms-jee-server-cluster}

## Nödvändig kunskap {#prerequisites}

Bekanta dig med Adobe Experience Manager (AEM) Forms på JEE-, JBoss®-, WebSphere®- och WebLogic-servrar, Red Hat® Linux®, SUSE® Linux®, Microsoft® Windows, IBM® AIX® eller Sun Solaris™, Oracle, IBM® DB2® eller SQL Server-databasservrar samt webbmiljöer.

## Användarnivå {#user-level}

Avancerat

Ett AEM Forms på JEE-kluster är en topologi som är utformad för att AEM Forms på JEE ska kunna motstå ett klusterfel. Det gör det också möjligt för topologin att skala systemkapaciteten utöver funktionerna för en enskild nod. Ett kluster kombinerar flera noder i ett enda logiskt system som delar data och tillåter transaktioner att spänna över flera noder vid körningen. Ett kluster är det mest allmänna sättet att skala AEM Forms på JEE, eftersom alla kombinationer av tjänster som hanterar alla kombinationer av arbetsbelastningar kan stödjas. En AEM Forms i JEE-kluster passar inte nödvändigtvis alla typer av distributioner och en icke-klustrad serverns belastningsbalanserade arkitektur kan vara lämplig.

I det här dokumentet beskrivs specifika konfigurationskrav och möjliga problemområden som du kan stöta på i ett AEM Forms i JEE-kluster.

## Vad finns i ett kluster? {#what-is-in-cluster}

AEM Forms på JEE-klusternoderna kommunicerar sinsemellan och delar information så att klustret som helhet kan ha en enda konsekvent konfiguration och programstatus. Informationsutbytet inom klustret sker på flera olika sätt samtidigt som det används i olika sammanhang. De grundläggande metoderna för informationsutbyte visas i bilden nedan:

![Programserverkluster](assets/application-server-cluster.jpg)

### Programserverkluster {#application-server-cluster}

Ett AEM Forms i JEE-kluster är beroende av den underliggande programserverns klusterfunktioner. Programserverkluster gör att klusterkonfigurationen kan hanteras som en helhet och tillhandahåller klustertjänster på låg nivå, som Java™ Naming and Directory Interface (JNDI), som gör att programvarukomponenterna kan hitta varandra i klustret. Klustertjänsternas utformning och de underliggande tekniska beroenden som programservern har beror på programservern. WebSphere® och WebLogic har sofistikerade hanteringsfunktioner för kluster, medan JBoss® har en grundläggande strategi.

### GemFire-cache {#gemfire-cache}

GemFire-cachen är en distribuerad cachemekanism som implementeras i varje klusternod. Noderna hittar varandra och skapar ett enda logiskt cache-minne som är konsekvent mellan noderna. De noder som hittar varandras hörn för att behålla en enda teoretisk cache som visas som ett moln i bild 1. Till skillnad från GDS och databasen är cachen en rent traditionell enhet. Det faktiska cachelagrade innehållet lagras i minnet och i katalogen `LC_TEMP` på var och en av klusternoderna.

### Databas {#database}

Databasen AEM Forms on JEE, som nås via JDBC-datakällorna IDP_DS, EDC_DS med flera, delas av alla noder i klustret. De mest beständiga data om AEM Forms status för JEE, t.ex. vilka transaktioner som pågår, användardata som är kopplade till pågående transaktioner och data om hur systeminställningar har ställts in i den här databasen.

### Global dokumentlagring {#global-document-storage}

GDS (Global Document Storage) är ett filsystembaserat lagringsutrymme som används av klassen IDPDocument i AEM Forms i JEE. GDS lagrar kortlivade och långlivade filer som måste vara tillgängliga för alla noder i klustret.

### Andra artiklar {#other-items}

Förutom de här delade huvudresurserna finns det andra objekt som har en särskild klusterfunktion, till exempel Quartz. Quartz är ett undersystem för schemaläggning som används av AEM Forms på JEE, och använder databastabeller för att hålla reda på vad som har schemalagts och vilka schemalagda aktiviteter som körs. Quartz måste konfigureras på olika sätt för ennodsinstallationer och kluster, och den hämtar sin referenspunkt från andra AEM Forms i JEE-inställningar.

## Vanliga konfigurationsproblem {#common-configuration}

En av de mest frustrerande sakerna med att underhålla eller felsöka en AEM Forms i ett JEE-kluster är att det inte finns någon enda plats att titta på för att bekräfta att klustret är felfritt. För att bekräfta att alla är bra i klustret utförs en del undersökningar och analyser, och det finns flera fellägen för klusteråtgärden, beroende på vad som är fel med klusterkonfigurationen. Bilden nedan visar ett felaktigt konfigurerat kluster där flera av de delade resurserna är felaktigt delade.

![Felaktigt konfigurerat kluster](assets/bad-configuration-cluster.png)

Förstå hur klustring fungerar och vilka typer av saker du kan söka efter och verifiera i ett kluster, även om du inte tänker köra AEM Forms på JEE i ett kluster. Orsaken är att vissa delar av AEM Forms i JEE kanske inte har någon som helst benägenhet att fungera i ett kluster på rätt sätt och att de får ett klusterbeteende som du inte förväntar dig.

Så vad är det för fel med delningskonfigurationen från bild ovan? I följande avsnitt beskrivs problemen:

### (1) GemFire-klusterkonfiguration {#gemfire-cluster-configuration}

Flera saker kan gå fel med Gemfire-cachen. Två typiska scenarier är:

* Noder som borde kunna hitta varandra kan inte göra det.

* Noder som är klustrade kan hitta varandra och dela ett cacheminne när de inte ska göra det.

Om du har noder som du tänker klustra måste de hitta varandra i nätverket. Som standard gör de detta med multicast-UDP-meddelanden. Varje nod skickar ut utsändningsmeddelanden som annonserar att det finns, och alla noder som tar emot ett sådant meddelande börjar prata med de andra noder som den hittar. Den här typen av metod för automatisk upptäckt är vanligt, och det gör många typer av program och enheter.

Ett vanligt problem med automatisk identifiering är att multicast-meddelanden kan filtreras av nätverket. Detta kan vara en del av en nätverksprincip, eller på grund av brandväggsregler för programvara, eller på grund av att de inte kan dirigeras över det nätverk som finns mellan noder. På grund av de allmänna svårigheterna med att få UDP-autosök att fungera i komplexa nätverk är det vanligt att produktionsdistributioner använder en alternativ identifieringsmetod: TCP-identifierare. En allmän diskussion om TCP-positionerare finns i referenserna.

**Hur vet jag om jag använder lokaliserare eller UDP?**

Följande JVM-egenskaper styr den metod som används i GemFire-cachen för att hitta andra noder.

Multicast-inställningar:

* `adobe.cache.multicast-port`: Multicast-porten som används för att kommunicera med andra medlemmar i det distribuerade systemet. Om värdet är noll inaktiveras multicast för både medlemsidentifiering och distribution.

* `gemfire.mcast-address` (valfritt): Åsidosätter den standard-IP-adress som används av Gemfire.

Inställningar för TCP-positionerare:

* `adobe.cache.cluster-locators`: IP-adressen/värdnamnet för TCP-positioneraren och TCP-positionerarporten för alla positionerare som används av systemmedlemmar för att kommunicera med aktiva positionerare.

Listan måste innehålla alla identifierare som används och måste konfigureras konsekvent för alla medlemmar i klustersystemet.

Om listan TCP-positionerare är tom används inte positionerare och multicast-metoden används i stället.

**Hur kontrollerar jag om min TCP-positionerare körs?**

För det första, om TCP-positionerare används, bör du ha de TCP-positionerare som anges i följande JVM-egenskap på alla klusternoder:

`-Dadobe.cache.cluster-locators=aix01.adobe.com[22345],aix02.adobe.com[22345]`

Du behöver inte köra lokaliserarna på AEM Forms på JEE-klusternoder. De kan köras på andra system som är skilda från klustret om du vill. Fler än ett system kan köra positionerare. Det anses dessutom som god praxis att låta positionerare köras på två platser, mot risken att ett enda fel i positionerarna kan orsaka problem med klusteromstart. På var och en av de datorer som kör positionerare bör du kunna verifiera att de körs med följande kommandon på dessa datorer:

`netstat -an | grep 22345`

Det förväntade svaret bör vara följande:

`tcp 0 0 *.22345 *.* LISTEN`

Ett annat verifieringskommando:

`ps -ef | grep gemfire`

Det förväntade svaret ska se ut ungefär så här:

`livecycl 331984 1 0 10:14:51 pts/0 0:03 java -cp ./gemfire.jar: -Dgemfire.license-type=production -Dlocators=localhost[22345] com.gemstone.gemfire.distributed.Locator 22345`

**Hur ser jag vilka noder GemFire tror finns i klustret?**

GemFire skapar loggningsinformation som kan användas för att diagnostisera vilka klustermedlemmar som har hittats och antagits av GemFire-cachen. Detta kan användas för att verifiera att alla korrekta klustermedlemmar hittas och att ingen extra eller felaktig identifiering av klusternoder sker. Loggfilen för GemFire finns i den konfigurerade AEM Forms-katalogen för JEE temporär:

`.../LC_TEMP/adobeZZ__123456/Caching/Gemfire.log`

Den numeriska strängen efter `adobeZZ_` är unik för servernoden och du måste därför söka i det faktiska innehållet i din tillfälliga katalog. De två tecknen efter `adobe` beror på programservertypen: `wl`, `jb` eller `ws`.

Följande exempelloggar visar vad som händer när ett tvånodskluster hittar sig självt.

På den första noden, AP-HP8:

```xml
[config 2011/08/05 09:28:09.143 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] This member, ap-hp8(4268):18763, is becoming group coordinator.
[info 2011/08/05 09:28:09.151 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] Entered into membership in group GF6.5.1.17 with ID ap-hp8(4268)<v0>:18763/56449.
[info 2011/08/05 09:28:09.152 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] Starting DistributionManager ap-hp8(4268)<v0>:18763/56449.
[info 2011/08/05 09:28:09.153 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] Initial (membershipManager) view =  [ap-hp8(4268)<v0>:18763/56449]
[info 2011/08/05 09:28:09.153 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] Admitting member <ap-hp8(4268)<v0>:18763/56449>. Now there are 1 non-admin member(s).
[info 2011/08/05 09:28:09.154 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] ap-hp8(4268)<v0>:18763/56449 is the elder and the only member.
[info 2011/08/05 09:28:09.163 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] Did not hear back from any other system. I am the first one.
[info 2011/08/05 09:28:09.164 EDT GemfireCacheAdapter <server.startup : 0> tid=0x65] DistributionManager ap-hp8(4268)<v0>:18763/56449 started on 239.192.81.1[33456]. There were 0 other DMs. others: []
[info 2011/08/05 09:28:20.841 EDT GemfireCacheAdapter <Pooled Message Processor 1> tid=0xc4] New administration member detected at ap-hp7(2821)<v1>:19498/59136.
```

På den andra noden, AP-HP7:

```xml
[info 2011/08/05 09:28:09.830 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] Attempting to join distributed system whose membership coordinator is ap-hp8(4268)<v0>:18763 using membership ID ap-hp7(2821):19498
[info 2011/08/05 09:28:10.058 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] Entered into membership in group GF6.5.1.17 with ID ap-hp7(2821)<v1>:19498/59136.
[info 2011/08/05 09:28:10.059 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] Starting DistributionManager ap-hp7(2821)<v1>:19498/59136.
[info 2011/08/05 09:28:10.060 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] Initial (membershipManager) view =  [ap-hp8(4268)<v0>:18763/56449, ap-hp7(2821)<v1>:19498/59136]
[info 2011/08/05 09:28:10.060 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] Admitting member <ap-hp8(4268)<v0>:18763/56449>. Now there are 1 non-admin member(s).
[info 2011/08/05 09:28:10.060 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] Admitting member <ap-hp7(2821)<v1>:19498/59136>. Now there are 2 non-admin member(s).
[info 2011/08/05 09:28:10.128 EDT GemfireCacheAdapter <server.startup : 0> tid=0x64] DistributionManager ap-hp7(2821)<v1>:19498/59136 started on 239.192.81.1[33456]. There were 1 other DMs. others: [ap-hp8(4268)<v0>:18763/56449]
```

**Vad händer om GemFire hittar noder som det inte ska hitta?**

Varje distinkt kluster som delar ett företagsnätverk bör använda en separat uppsättning TCP-positionerare, om TCP-positionerare används, eller ett separat UDP-portnummer om multicast-UDP-konfiguration används. Eftersom automatisk UDP-identifiering är standardkonfigurationen för AEM Forms på JEE, och samma standardport 33456 används av flera kluster, är det möjligt att kluster som inte ska försöka kommunicera kan göra det oväntat. Produktions- och QA-kluster bör till exempel vara separata, men kan ansluta till varandra via UDP-multicast.

Den vanligaste situationen när du kanske upptäcker dubblettportar i ett nätverk som GemFire felaktigt kluderar i är under Bootstrap för ett kluster. Det man kan finna är att Bootstrap-processen misslyckas utan någon tydlig orsak. Vanligtvis visas fel som detta:

```xml
Caused by: com.ibm.ejs.container.UnknownLocalException: nested exception is: com.adobe.pof.schema.ObjectTypeNotFoundException: Object Type: dsc.sc_service_configuration not found.
                at com.adobe.pof.schema.POFDefaultDomain.getObjectType(POFDefaultDomain.java:93)
                at com.adobe.idp.dsc.initializer.DSCInitializerBean.serviceConfigAuditAttributeExists(DSCInitializerBean.java:225)
                at com.adobe.idp.dsc.initializer.DSCInitializerBean.installSchema(DSCInitializerBean.java:186)
                at com.adobe.idp.dsc.initializer.DSCInitializerBean.bootstrap(DSCInitializerBean.java:94)
                at com.adobe.idp.dsc.initializer.EJSLocalStatelessDSCInitializerBeanLocalEJB_7bb34e85.bootstrap(Unknown Source)
                at com.adobe.livecycle.bootstrap.bootstrappers.DSCBootstrapper.bootstrap(DSCBootstrapper.java:68)
```

I det här fallet arbetar Bootstrapper med GemFire för att komma åt de tabeller som krävs. Och det finns en inkonsekvens mellan tabellerna som nås via JDBC och den cachelagrade tabellinformationen som returneras av GemFire, som kommer från ett annat kluster med en annan underliggande databas.

Även om en dubblerad port ofta blir tydlig under Bootstrap, är det möjligt att den här situationen visas senare. Detta kan inträffa när ett kluster startas om efter att ha stängts av när Bootstrap för det andra klustret har inträffat. Eller när nätverkskonfigurationen ändras så att kluster som tidigare isolerats, för multicast-syften, blir synliga för varandra.

Om du vill diagnostisera sådana situationer tittar du på GemFire-loggarna och funderar noggrant på om bara de förväntade noderna hittas. För att åtgärda problemet måste du ändra egenskapen `adobe.cache.multicast-port` till ett annat värde för ett eller båda klustren.

### 2) GDS-delning {#gds-sharing}

GDS-delning är konfigurerad utanför AEM Forms på själva JEE, på O/S-nivå, där du måste se till att samma delade katalogstruktur är tillgänglig för alla klusternoder. I Windows-system uppnås detta genom att en filresurs skapas antingen från en nod till en annan, eller från ett fjärrfilsystem som en NAS-enhet till alla noder. På UNIX®-system sker GDS-delning vanligtvis genom NFS-fildelning, igen, antingen från en nod till en annan eller från en NAS-enhet.

Ett möjligt felläge för klustret är om den här fjärrfilresursen blir otillgänglig eller har små problem. En fjärrmontering kan misslyckas på grund av nätverksproblem, säkerhetsinställningar eller felaktig konfiguration. En omstart av systemet kan göra att konfigurationsändringar som gjorts dagar eller veckor i förväg träder i kraft, vilket kan skapa överraskningar.

**Vad händer om en NFS-resurs inte kan monteras?**

På UNIX® kan det sätt på vilket NFS-monteringar mappas till katalogstrukturen göra det möjligt för en synbart användbar GDS-katalog att vara tillgänglig, även om monteringen misslyckas. Fundera:

* NAS-server: NFS-delad mapp /u01/apply/livecycle_gds
* Nod 1: en monteringspunkt till den delade mappen (som finns på databasservern) som finns här: /u01/apply/livecycle_gds
* Nod 2: en monteringspunkt till den delade mappen (som finns på databasservern) som finns här: /u01/apply/livecycle_gds

* LCES anger sökvägen till GDS: /u01/iapply/livecycle_gds

Om monteringen på nod 1 misslyckas innehåller katalogstrukturen fortfarande sökvägen `/u01/iapply/livecycle_gds` till den tomma monteringspunkten och noden verkar köras korrekt. Men eftersom GDS-innehållet inte delas med den andra noden fungerar inte klustret korrekt. Detta kan och händer, och resultatet blir att klustret misslyckas på mystiska sätt.

Det bästa sättet är att ordna saker så att Linux®-monteringspunkten inte används som roten för GDS, utan i stället används en del katalog i den som GDS-rot:

* Om du har en NFS-server kan den ha en katalog: /some/storage/lc_Cluster_dev/LC_GDS
* På klusternoden har du en monteringspunkt: /u01/iapply/shared
* Montera nfs_server: /some/storage/lc_Cluster_dev/u01/iapply/shared
* Peka GDS på /u01/apply/shared/LC_GDS

Om monteringen av någon anledning inte lyckas innehåller den obelastade monteringspunkten ingen LC_GDS-katalog och klustret misslyckas på ett förutsägbart sätt eftersom det inte går att hitta någon GDS.

**Hur verifierar jag att alla noder ser samma GDS och har behörigheter?**

Verifiering av GDS-åtkomst och -delning görs bäst genom att varje nod används som en interaktiv användare. Du kan göra detta antingen via SSH eller telnet till UNIX®-noder, eller via fjärrskrivbord till Windows-system. Du bör kunna navigera till den konfigurerade GDS-katalogen eller filsystemet på varje nod och skapa testfiler från alla noder som är synliga i alla andra noder.

Var uppmärksam på det användar-ID som AEM Forms på JEE används under. I Windows körklara installationer är detta som lokal administratör. På UNIX® kan det vara som en specifik tjänstanvändare som konfigurerats i startskriptet eller i programserverkonfigurationen. Det är viktigt att detta användar-ID kan skapa och ändra GDS-filer på alla noder.

På UNIX®-system är NFS-konfigurationer ofta standard att förlita sig på rotägande eller rotåtkomsträttigheter till filer och objekt. Om du kör programservern som rotanvändare kanske du måste ange alternativ på NFS-servern, noden som monterar filerna eller båda. På så sätt kan du få bilateral åtkomst till och kontroll över filer som skapats av en nod och som nås av en annan.

### (3) Databasdelning {#database-sharing}

För att ett kluster ska fungera på rätt sätt måste samma databas delas av alla klustermedlemmar. Omfånget att få detta fel är ungefär:

* av misstag ställa in datakällorna IDP_DS, EDC_DS, AdobeDefaultSA_DS eller andra obligatoriska datakällor på olika klusternoder, så att noderna pekar på olika databaser.
* av misstag ange flera separata noder för att dela en databas när de inte ska göra det.

Beroende på programservern kan det vara naturligt att JDBC-anslutningen definieras i ett klusteromfång, så att olika definitioner inte är möjliga på olika noder. I JBoss® är det dock helt möjligt att ställa in saker så att en datakälla, som IDP_DS, pekar på en databas på nod 1, men pekar på något annat på nod 2.

Det motsatta problemet är vanligare. Detta innebär att flera fristående (eller kluster) AEM Forms på JEE-noder oavsiktligt pekar på samma schema när de inte är avsedda att göra det. Detta inträffar oftast när en DBA utan att veta om ger ut en enda AEM Forms på JEE-databasens anslutningsinformation till både DEV- och QA-konfigurationsteamen. Ingendera teamet insåg att DEV- och QA-instanserna kräver separata databaser.

## Programserverkluster {#application-server-cluster-1}

För att AEM Forms ska fungera på JEE-kluster måste programservern vara konfigurerad och fungera som ett kluster. I WebSphere® och WebLogic är detta en okomplicerad och väldokumenterad process. I JBoss® är klusterkonfigurationen lite mer praktisk och det kan vara en utmaning att se till att noderna är konfigurerade att fungera som ett kluster och att de faktiskt hittar och kommunicerar med varandra. JBoss® är internt beroende av JGroups, som använder UDP-multicast för att hitta och koordinera med peer-noder. Vissa av de problem som nämns med GemFire kan uppstå, till exempel om noder inte hittar varandra när de borde, eller om de inte borde hitta varandra.

Referenser:

* [Företagstjänster med hög tillgänglighet via JBoss®-kluster](https://docs.jboss.org/jbossas/jboss4guide/r4/html/cluster.chapt.html)

* [Oracle WebLogic Server-Using-kluster](https://docs.oracle.com/cd/E12840_01/wls/docs103/pdf/cluster.pdf)

### Hur kontrollerar jag att JBoss® klustras korrekt? {#check-jboss-clustering}

När JBoss® startas loggas INFO-nivåmeddelanden om noden som ansluter till klustret till loggfilen/konsolen när klustermedlemmar identifieras.

Om ett klusternamn har angetts med kommandoradsalternativet -g vid körning visas meddelanden som liknar följande:

```xml
GMS: address is 10.36.34.44:55200 (cluster=QE_cluster)
GMS: address is 10.36.34.44:55200 (cluster=QE_cluster-HAPartitionCache)
and ones like:

[org.jboss.ha.framework.interfaces.HAPartition.QE_cluster] (JBoss System Threads(1)-3) Number of cluster members: 1
2011-07-14 11:34:03,072 INFO  [org.jboss.ha.framework.interfaces.HAPartition.QE_cluster] (JBoss System Threads(1)-3) Other members: 0
2011-07-14 11:34:03,138 INFO  [org.jboss.cache.RPCManagerImpl] (main) Received new cluster view: [10.36.34.44:55200|0] [10.36.34.44:55200]
2011-07-14 11:34:03,139 INFO  [org.jboss.cache.RPCManagerImpl] (main) Cache local address is 10.36.34.44:55200
```

### Quartz-schemaläggare {#quartz-scheduler}

I allmänhet är det meningen att AEM Forms på JEE:s användning av den interna Quartz-schemaläggaren i ett kluster automatiskt ska följa AEM Forms globala klusterkonfiguration på JEE i allmänhet. Det finns emellertid ett fel, #2794033, som gör att den automatiska klusterkonfigurationen för Quartz misslyckas om TCP-positionerare används för Gemfire i stället för automatisk multicast-identifiering. I det här fallet körs Quartz felaktigt i ett icke-grupperat läge. Detta skapar dödlägen och korrupta data i Quartz-tabellerna. Biverkningarna är värre i version 8.2.x än 9.0, eftersom Quartz inte används så mycket, utan fortfarande finns där.

Följande korrigeringar är tillgängliga för det här problemet: 8.2.1.2 QF2.143 och 9.0.0.2 QF2.44.

Det finns också en tillfällig lösning som anger båda dessa egenskaper:

* `-Dadobe.cache.cluster.locators=xxx`

* `-Dadobe.cache.cluster-locators=xxx`

I den ena inställningen används en punkt mellan &quot;kluster&quot; och &quot;lokaliserare&quot; och i den andra används ett bindestreck. Det är enkelt att implementera och är mindre riskabelt än att tillämpa en programkorrigering, men det handlar om att på ett konstlat sätt skapa en förvirrande extra, felnamnad konfigurationsinställning.

### Hur kontrollerar jag att Quartz körs som en enda nod eller ett kluster? {#check-quartz}

Om du vill se hur Quartz har konfigurerat sig själv måste du titta på de meddelanden som genereras av AEM Forms i JEE Scheduler-tjänsten under start. Dessa meddelanden genereras med INFO-allvarlighetsgrad och det kan vara nödvändigt att justera loggnivån och starta om för att få meddelanden. I AEM Forms vid JEE-startsekvens börjar Quartz-initieringen med följande rad:

INFO `[com.adobe.idp.scheduler.SchedulerServiceImpl]` IDPSchedulerService onLoad
Det är viktigt att hitta den första raden i loggarna. Orsaken är att vissa programservrar använder Quartz också, och deras Quartz-instanser inte bör blandas ihop med de instanser som används av AEM Forms i JEE Scheduler-tjänsten. Det här är indikationen på att tjänsten Schemaläggaren startas och de rader som följer efter den talar om för dig om den startar i grupperat läge korrekt. Flera meddelanden visas i den här sekvensen, och det är det sista&quot;startade&quot; meddelandet som visar hur Quartz är konfigurerat:

Här anges namnet på Quartz-instansen: `IDPSchedulerService_$_ap-hp8.ottperflab.adobe.com1312883903975`. Namnet på schemaläggarens Quartz-instans börjar alltid med strängen `IDPSchedulerService_$_`. Strängen som läggs till i slutet av detta anger om Quartz körs i grupperat läge. Den långa unika identifieraren som genereras från nodens värdnamn och en lång sträng med siffror, här `ap-hp8.ottperflab.adobe.com1312883903975`, anger att den körs i ett kluster. Om den fungerar som en enda nod är identifieraren ett tvåsiffrigt nummer, &quot;20&quot;:

INFO `[org.quartz.core.QuartzScheduler]` Schemaläggaren `IDPSchedulerService_$_20` har startats.
Den här kontrollen måste göras separat på alla klusternoder eftersom varje nods schemaläggare avgör separat om de ska användas i klusterläge eller inte.

### Vilken typ av problem uppstår om Quartz körs i fel läge? {#quartz-running-in-wrong-mode}

Om Quartz är inställt på att köras som en enda nod, men körs i ett kluster, och Quartz-databastabeller delas med andra noder, resulterar detta i att AEM Forms inte fungerar som det ska på JEE Scheduler-tjänsten. Och den åtföljs ofta av databasdödlägen. Detta är en ganska typisk stackspårning som du kan se i den här situationen:

```xml
[1/20/11 10:40:57:584 EST] 00000035 ErrorLogger   E org.quartz.core.ErrorLogger schedulerError An error occured while marking executed job complete. job= 'Asynchronous.TaskFormDataSaved:12955380518320.5650479324757354'
 org.quartz.JobPersistenceException: Could not remove trigger: ORA-00060: deadlock detected while waiting for resource  [See nested exception: java.sql.SQLException: ORA-00060: deadlock detected while waiting for resource ]
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.removeTrigger(JobStoreSupport.java:1405)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.triggeredJobComplete(JobStoreSupport.java:2888)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport$38.execute(JobStoreSupport.java:2872)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport$40.execute(JobStoreSupport.java:3628)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.executeInNonManagedTXLock(JobStoreSupport.java:3662)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.executeInNonManagedTXLock(JobStoreSupport.java:3624)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.triggeredJobComplete(JobStoreSupport.java:2868)
        at org.quartz.core.QuartzScheduler.notifyJobStoreJobComplete(QuartzScheduler.java:1698)
        at org.quartz.core.JobRunShell.run(JobRunShell.java:273)
        at org.quartz.simpl.SimpleThreadPool$WorkerThread.run(SimpleThreadPool.java:529)
Caused by: java.sql.SQLException: ORA-00060: deadlock detected while waiting for resource
```

### Hur synkroniserar jag systemklockor i ett kluster? {#synchronize-system-clocks-cluster}

För att ett kluster ska fungera smidigt måste klockorna på alla klusternoder vara nära synkroniserade. Detta kan inte göras för hand och måste göras med någon form av tidssynkroniseringstjänst som körs regelbundet. Klockorna på alla noder måste vara inom en sekund från varandra. Enligt bästa praxis ska inte bara klusternoderna, utan även belastningsutjämnaren, databasservern, GDS NAS-servern och alla andra komponenter synkroniseras.

Windows-tidssynkronisering brukar vara till domänkontrollanten. UNIX®-system kan synkroniseras med NTP till en annan tidskälla. Det bästa är om det är möjligt att alla system - både AEM Forms på JEE-noder och andra systemkomponenter - synkroniserar till samma källa.

Inte ens i de flesta temporära testmiljöer räcker det att manuellt ställa in klockorna på noderna. Manuell inställning av klockorna ger inte tillräcklig exakt synkronisering, och klockorna på de två noderna rör sig ofrånkomligen i förhållande till varandra, inte ens under en tid av bara en dag. En aktiv tidssynkroniseringsmekanism är nödvändig för tillförlitlig klusteråtgärd.

### Belastningsutjämning {#load-balancer}

Ett typiskt krav för ett kluster som tillhandahåller användarinteraktiva tjänster är en HTTP-belastningsutjämnare som distribuerar HTTP-begäranden i klustret. Om du vill använda en belastningsutjämnare med ett AEM Forms i JEE-kluster måste du konfigurera följande:

* kliande

* Regler för URL-omskrivning

* nodhälsokontroll

### Vad ska jag göra med funktionen för hälsokontroll av belastningsutjämnare? {#load-balancer-health-check}

Vissa belastningsutjämnare kan konfigureras för att utföra en periodisk hälsokontroll av de noder som belastningsutjämnas. Vanligtvis är detta en URL till en programfunktion som belastningsutjämnaren försöker få åtkomst till. Om belastningen lyckas antas noden vara felfri och behålls i belastningsutjämningsuppsättningen. Om URL:en inte kan läsas in antas noden vara felaktig och tas bort från uppsättningen. URL:en för hälsokontrollen är vanligtvis ansluten till inloggningssidan för AEM Forms på JEE AdminUI. Detta är inte en idealisk hälsokontroll för en klustermedlem, och det skulle vara bättre att implementera en kortlivad process och använda REST API URL som hälsokontrollsfunktion.

## Tillfällig filsökväg och liknande klusterinställningar {#temporary-file-path-cluster-settings}

Vissa filsökvägsinställningar i AEM Forms på JEE är klusteromfattande och har samma effektiva inställning på alla noder, men tolkas separat på varje nod för att hänvisa till lokala filer. De viktigaste är teckensnittssökvägsinställningarna och tillfälliga kataloginställningar. Gå till skärmen med grundläggande AdminUI-konfigurationer (Hem > Inställningar > Kärnsystem > Huvudkonfigurationer)

Följande inställningar bör vara markerade:

1. Plats för tillfällig katalog
1. Sökväg till Adobe Server Fonts-katalogen
1. Plats för kundteckensnittskatalogen
1. Plats för katalogen System Fonts
1. Plats för Data Services-konfigurationsfilen

Klustret har bara en sökvägsinställning för var och en av dessa konfigurationsinställningar. Katalogplatsen Temp kan till exempel vara `/home/project/QA2/LC_TEMP`. I ett kluster är det nödvändigt att varje nod faktiskt har den här sökvägen tillgänglig. Om en nod har den förväntade tillfälliga filsökvägen och en annan nod inte gör det, fungerar den nod som inte gör det felaktigt.

Även om dessa filer och sökvägar kan delas mellan noderna eller lagras separat, eller på fjärranslutna filsystem, är det god praxis att de är lokala kopior på den lokala nodens disklagring.

Den tillfälliga katalogsökvägen bör inte delas mellan noder. En procedur som liknar den som beskrivs för att verifiera att GDS bör användas för att verifiera att den tillfälliga katalogen inte delas. Gå till varje nod, skapa en temporär fil i sökvägen som anges av sökvägsinställningen och kontrollera sedan att de andra noderna inte delar filen. Den tillfälliga katalogsökvägen bör referera till lokal disklagring på varje nod, om det är möjligt, och bör kontrolleras.

För varje sökvägsinställning kontrollerar du att sökvägen finns och att den är åtkomlig från alla noder i klustret med den användaridentitet som AEM Forms i JEE körs under. Innehållet i teckensnittskatalogen måste vara läsbart. Den tillfälliga katalogen måste tillåta läsning, skrivning och kontroll.
