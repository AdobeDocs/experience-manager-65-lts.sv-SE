---
title: Bästa tillvägagångssätt för att övervaka [!DNL Assets] distributionen
description: Bästa tillvägagångssätt för att övervaka miljön och prestandan för din [!DNL Adobe Experience Manager] distribution efter att den har distribuerats.
contentOwner: AG
role: Admin, Architect
feature: Asset Management
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1639'
ht-degree: 0%

---

# Bästa tillvägagångssätt för att övervaka distributionen av [!DNL Adobe Experience Manager Assets] {#assets-monitoring-best-practices}

Från [!DNL Experience Manager Assets]-synpunkt bör övervakningen omfatta övervakning och rapportering av följande processer och tekniker:

* System CPU
* Systemminnesanvändning
* Väntetid för systemdisk-I/O och IO
* Systemets nätverks-I
* JMX MBeans för heap-användning och asynkrona processer, som arbetsflöden
* OSGi-konsolens hälsokontroller

Vanligtvis kan [!DNL Experience Manager Assets] övervakas på två sätt: live-övervakning och långtidsövervakning.

## Live-övervakning {#live-monitoring}

Du bör utföra direktövervakning under prestandatestningsfasen av din utveckling eller under situationer med hög belastning för att förstå prestandaegenskaperna i din miljö. Vanligtvis bör direktövervakning utföras med en uppsättning verktyg. Här är några rekommendationer:

* [Visual VM](https://visualvm.github.io/): Visual VM gör att du kan visa detaljerad Java VM-information, inklusive CPU-användning och Java-minnesanvändning. Dessutom kan du ta prov på och utvärdera kod som körs i en distribution.
* [Överkant](https://man7.org/linux/man-pages/man1/top.1.html): Överkant är ett Linux-kommando som öppnar en kontrollpanel som visar användningsstatistik, inklusive CPU, minne och IO-användning. Den ger en översikt på hög nivå över vad som händer i en instans.
* [Htop](https://hisham.hm/htop/): Htop är ett interaktivt processvisningsprogram. Den innehåller detaljerad information om CPU och minnesanvändning utöver vad Top kan tillhandahålla. Htop kan installeras på de flesta Linux-system med `yum install htop` eller `apt-get install htop`.

* IoTop: IoTop är en detaljerad kontrollpanel för diskens I/O-användning. Här visas staplar och mätare som avbildar de processer som använder disk-I/O och hur mycket de använder. Jotop kan installeras på de flesta Linux-system som använder `yum install iotop` eller `apt-get install iotop`.

* [InfoTop](https://www.ex-parrot.com/pdw/iftop/): Iftop visar detaljerad information om Ethernet-/nätverksanvändning. Om Iftop visar statistik per kommunikationskanal för de enheter som använder Ethernet och den bandbredd de använder. Iftop kan installeras på de flesta Linux-system med `yum install iftop` eller `apt-get install iftop`.

* Java Flight Recorder (JFR): Ett kommersiellt verktyg från Oracle som du kan använda fritt i icke-produktionsmiljöer. Mer information finns i [Använda Java Flight Recorder för att diagnostisera CQ-körningsproblem](https://cq-ops.tumblr.com/post/73865704329/how-to-use-java-flight-recorder-to-diagnose-cq).
* [!DNL Experience Manager] `error.log`-fil: Du kan undersöka filen [!DNL Experience Manager] `error.log` för att få information om fel som loggats i systemet. Använd kommandot `tail -F quickstart/logs/error.log` för att identifiera fel som ska undersökas.
* [Arbetsflödeskonsol](/help/sites-administering/workflows.md): Använd arbetsflödeskonsolen för att övervaka arbetsflöden som ligger efter eller fastnar.

Vanligtvis använder du de här verktygen tillsammans för att få en heltäckande bild av prestanda för din [!DNL Experience Manager]-distribution.

>[!NOTE]
>
>De här verktygen är standardverktyg och stöds inte direkt av Adobe. De behöver inga ytterligare licenser.

![chlimage_1-33](assets/chlimage_1-143.png)

*Figur: Live-övervakning med verktyget Visual VM.*

![chlimage_1-32](assets/chlimage_1-142.png)

## Långsiktig övervakning {#long-term-monitoring}

Långsiktig övervakning av en [!DNL Experience Manager]-distribution innebär övervakning under en längre tid av samma delar som övervakas live. Det innehåller även definitioner av varningar som är specifika för din miljö.

### Loggaggning och rapportering {#log-aggregation-and-reporting}

Det finns flera verktyg tillgängliga för att samla loggar, till exempel Splunk(TM) och Elastic Search, Logstash och Kabana (ELK). För att utvärdera drifttiden för din [!DNL Experience Manager]-distribution är det viktigt att du förstår logghändelser som är specifika för ditt system och skapar aviseringar baserade på dem. En god kunskap om dina utvecklings- och operationsrutiner kan hjälpa dig att bättre förstå hur du kan trimma loggsammanställningsprocessen för att generera kritiska varningar.

### Miljöövervakning {#environment-monitoring}

Miljöövervakning omfattar övervakning av följande:

* Nätverksgenomströmning
* Skiva-I
* Minne
* CPU-användning
* JMX MBeans
* Externa webbplatser

Du behöver externa verktyg, som NewRelic(TM) och AppDynamics(TM), för att kunna övervaka varje objekt. Med de här verktygen kan du definiera varningar som är specifika för ditt system, till exempel hög systemanvändning, säkerhetskopiering av arbetsflöde, misslyckade hälsokontroller eller oautentiserad åtkomst till din webbplats. Adobe rekommenderar inga särskilda verktyg framför andra. Hitta det verktyg som passar dig och använd det för att övervaka de objekt som diskuteras.

#### Intern programövervakning {#internal-application-monitoring}

Intern programövervakning omfattar övervakning av de programkomponenter som utgör [!DNL Experience Manager]-stacken, inklusive JVM, innehållsdatabasen och övervakning via anpassad programkod som är byggd på plattformen. I allmänhet genomförs det via JMX Mbeans, som kan övervakas direkt av många populära övervakningslösningar, till exempel SolarWinds (TM), HP OpenView(TM), Hyperic(TM), Zabbix(TM) och andra. För system som inte har stöd för en direkt anslutning till JMX kan du skriva gränssnittsskript för att extrahera JMX-data och exponera dem för dessa system i ett format som de själva förstår.

Fjärråtkomst till JMX Mbeans är inte aktiverat som standard. Mer information om övervakning via JMX finns i [Övervakning och hantering med JMX-teknik](https://docs.oracle.com/javase/7/docs/technotes/guides/management/agent.html).

I många fall krävs en baslinje för att effektivt kunna övervaka en statistik. Om du vill skapa en baslinje bör du observera systemet under normala arbetsförhållanden under en förutbestämd period och sedan identifiera det normala måttet.

**JVM-övervakning**

Precis som för alla Java-baserade programstackar är [!DNL Experience Manager] beroende av vilka resurser som tillhandahålls via den underliggande Java Virtual Machine. Du kan övervaka status för många av dessa resurser via plattforms-MXBeans som exponeras av JVM. Mer information om MXBeans finns i [Använda Platform MBean Server och Platform MXBeans](https://docs.oracle.com/javase/7/docs/technotes/guides/management/mxbeans.html).

Här följer några baslinjeparametrar som du kan övervaka för JVM:

Minne

* `MBean: lava.lang:type=Memory`
* URL: `/system/console/jmx/java.lang:type=Memory`
* Instanser: Alla servrar
* Varningströskel: När minnesanvändningen för stackar eller icke-stackar överstiger 75 % av det motsvarande maximala minnet.
* Varningsdefinition: Antingen är systemminnet otillräckligt eller så finns det en minnesläcka i koden. Analysera en tråddump för att komma fram till en definition.

>[!NOTE]
>
>Uppgifterna från denna böna uttrycks i byte.

Threads

* MBean: `java.lang:type=Threading`
* URL: `/system/console/jmx/java.lang:type=Threading`
* Instanser: Alla servrar
* Larm threshold: När antalet trådar är större än 150 % av baslinjen.
* Varningsdefinition: Antingen finns det en aktiv process, eller så använder en ineffektiv åtgärd en stor mängd resurser. Analysera en tråddump för att komma fram till en definition.

**Bildskärm[!DNL Experience Manager]**

[!DNL Experience Manager] visar också en uppsättning statistik och åtgärder via JMX. Dessa kan hjälpa till att utvärdera systemets hälsa och identifiera potentiella problem innan de påverkar användarna. Mer information finns i [dokumentation](/help/sites-administering/jmx-console.md) om [!DNL Experience Manager] JMX MBeans.

Här är några baslinjepametrar som du kan övervaka för [!DNL Experience Manager]:

Replikeringsagenter

* MBean: `com.adobe.granite.replication:type=agent,id="<AGENT_NAME>"`
* URL: `/system/console/jmx/com.adobe.granite.replication:type=agent,id="<AGENT_NAME>"`
* Instanser: En författare och alla publiceringsinstanser (för justeringsagenter)
* Varningströskel: När värdet för `QueueBlocked` är `true` eller värdet för `QueueNumEntries` är större än 150 % av baslinjen.

* Varningsdefinition: Det finns en blockerad kö i systemet som indikerar att replikeringsmålet är nere eller inte kan nås. Nätverks- eller infrastrukturproblem leder ofta till att för många poster köas, vilket kan påverka systemets prestanda negativt.

>[!NOTE]
>
>Ersätt `<AGENT_NAME>` med namnet på den replikeringsagent som du vill övervaka för parametrarna MBean och URL.

Sessionsräknare

* MBean: `org.apache.jackrabbit.oak:id=7,name="OakRepository Statistics",type="RepositoryStats"`
* URL: */system/console/jmx/org.apache.jackrabbit.oak:id=7,name=&quot;OakRepository Statistics&quot;,type*=&quot;RepositoryStats&quot;
* Instanser: Alla servrar
* Larm threshold: När öppna sessioner överskrider baslinjen med mer än 50 %.
* Varningsdefinition: Sessioner kan öppnas genom en del kod och aldrig stängas. Detta kan hända långsamt över tid och så småningom orsaka minnesläckor i systemet. Även om antalet sessioner bör variera i ett system, bör de inte öka kontinuerligt.

Hälsokontroller

Hälsokontroller som är tillgängliga i [åtgärdspanelen](/help/sites-administering/operations-dashboard.md#health-reports) har motsvarande JMX MBeans för övervakning. Du kan dock skriva anpassade hälsokontroller för att visa ytterligare systemstatistik.

Här följer några färdiga hälsokontroller som är bra att övervaka:

* Systemkontroller
   * MBean: `org.apache.sling.healthcheck:name=systemchecks,type=HealthCheck`
   * URL: `/system/console/jmx/org.apache.sling.healthcheck:name=systemchecks,type=HealthCheck`
   * Instanser: En författare, alla publiceringsservrar
   * Larm threshold: When the status is not OK
   * Varningsdefinition: Statusen för ett av mätvärdena är antingen WARN eller CRITICAL. Mer information om orsaken till problemet finns i loggattributet.

* Replikeringskö

   * MBean: `org.apache.sling.healthcheck:name=replicationQueue,type=HealthCheck`
   * URL: `/system/console/jmx/org.apache.sling.healthcheck:name=replicationQueue,type=HealthCheck`
   * Instanser: En författare, alla publiceringsservrar
   * Larm threshold: When the status is not OK
   * Varningsdefinition: Statusen för ett av mätvärdena är antingen WARN eller CRITICAL. Mer information om kön som orsakade problemet finns i loggattributet.

* Svarsprestanda

   * MBean: `org.apache.sling.healthcheck:name=requestsStatus,type=HealthCheck`
   * URL: `/system/console/jmx/org.apache.sling.healthcheck:name=requestsStatus,type=HealthCheck`
   * Instanser: Alla servrar
   * Varaktighet för larm: När statusen inte är OK
   * Varningsdefinition: Statusen för ett av mätvärdena är antingen WARN eller CRITICAL. Mer information om kön som orsakade problemet finns i loggattributet.

* Frågeprestanda

   * MBean: `org.apache.sling.healthcheck:name=queriesStatus,type=HealthCheck`
   * URL: `/system/console/jmx/org.apache.sling.healthcheck:name= queriesStatus,type=HealthCheck`
   * Instanser: En författare, alla publiceringsservrar
   * Larm threshold: When the status is not OK
   * Varningsdefinition: En eller flera frågor körs långsamt i systemet. Mer information om de frågor som orsakade problemet finns i loggattributet.

* Aktiva paket

   * MBean: `org.apache.sling.healthcheck:name=inactiveBundles,type=HealthCheck`
   * URL: `/system/console/jmx/org.apache.sling.healthcheck:name=inactiveBundles,type=HealthCheck`
   * Instanser: Alla servrar
   * Larm threshold: When the status is not OK
   * Varningsdefinition: Förekomst av inaktiva eller olösta OSGi-paket i systemet. Mer information om de paket som orsakade problemet finns i loggattributet.

* Loggfel

   * MBean: `org.apache.sling.healthcheck:name=logErrorHealthCheck,type=HealthCheck`
   * URL: `/system/console/jmx/org.apache.sling.healthcheck:name=logErrorHealthCheck,type=HealthCheck`
   * Instanser: Alla servrar
   * Larm threshold: When the status is not OK
   * Varningsdefinition: Det finns fel i loggfilerna. Mer information om orsaken till problemet finns i loggattributet.

## Vanliga problem och lösningar  {#common-issues-and-resolutions}

Om du råkar ut för problem i samband med övervakningen finns det några felsökningsuppgifter som du kan utföra för att lösa vanliga problem med [!DNL Experience Manager]-distributioner:

* Om du använder tarMK ska du köra Tjärkomprimering ofta. Mer information finns i [Underhåll databasen](/help/sites-deploying/storage-elements-in-aem-6.md#maintaining-the-repository).
* Kontrollera `OutOfMemoryError` loggar. Mer information finns i [Analysera minnesproblem](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17482.html).

* Kontrollera loggarna om det finns referenser till oindexerade frågor, trädgenomgångar eller indexgenomgångar. Dessa indikerar oindexerade frågor eller otillräckligt indexerade frågor. Mer information om hur du optimerar fråga- och indexeringsprestanda finns i [Bästa tillvägagångssätt för frågor och indexering](/help/sites-deploying/best-practices-for-queries-and-indexing.md).
* Använd arbetsflödeskonsolen för att verifiera att arbetsflödena fungerar som förväntat. Om det är möjligt kan du komprimera flera arbetsflöden till ett enda arbetsflöde.
* Läs om live-övervakning och leta efter fler flaskhalsar eller konsumenter av specifika resurser.
* Undersök ingångspunkterna från klientnätverket och ingångspunkterna till [!DNL Experience Manager]-distributionsnätverket, inklusive dispatchern. Det är ofta flaskhalsar. Mer information finns i [Om Assets nätverk](/help/assets/assets-network-considerations.md).
* Storleksändra din [!DNL Experience Manager]-server. Din [!DNL Experience Manager]-distribution kan ha en otillräckligt stor storlek. Adobe kundsupport kan hjälpa dig att identifiera om din server är för liten.
* Undersök filerna `access.log` och `error.log` för att se om det finns poster runt tiden när något gick fel. Leta efter mönster som kan indikera anpassade kodavvikelser. Lägg till dem i listan med händelser som du övervakar.
