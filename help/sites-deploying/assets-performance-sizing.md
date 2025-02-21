---
title: Assets Performance Guide
description: Lär dig hur du avgör den optimala maskinvarustorleken för en ny konfiguration av Digital Asset Management (DAM) och hur du felsöker prestandaproblem
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: configuring
solution: Experience Manager, Experience Manager Sites
feature: Configuring
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1210'
ht-degree: 0%

---

# Assets Performance Guide{#assets-performance-guide}

DAM (Digital Asset Management) används ofta i fall där prestanda är viktiga. Den vanliga DAM-installationen innehåller dock flera maskinvaru- och programvarukomponenter som kan påverka prestandan. Det här dokumentet innehåller följande:

* Information för systemadministratörer om hur man fastställer optimal maskinvarustorlek för en ny installation av Digital Asset Management
* Information för programutvecklare som vill felsöka DAM-instanser med prestandaproblem

## Prestandaproblem {#performance-issues}

Dåliga prestanda inom digital resurshantering kan påverka användarupplevelsen på tre sätt: interaktiv prestanda, bearbetning av resurser och nedladdningshastighet. För att förbättra prestandan är det viktigt att mäta de observerade prestandan korrekt och att fastställa målvärden.

**1. Interaktiv sökning och bläddring** Användare söker efter resurser eller bläddrar i DAM Finder och klagar över långsamma svarstider eller att sökresultaten inte visas omedelbart. Detta är ett interaktivt prestandaproblem.

Interaktiva prestanda mäts i svarstid för sidor. Detta är den tid det tar från att ta emot HTTP-begäran till att stänga HTTP-svaret, som kan avgöras av loggfilerna för begäran. Vanliga målprestanda är en sidsvarstid på mindre än två sekunder.

**2. Resursbearbetning** Ett problem med tillgångsbearbetning är när användare överför resurser och det tar några minuter tills resurser konverteras och hämtas till Adobe Experience Manager (AEM) DAM.

Prestanda för tillgångsbearbetning mäts i genomsnittlig slutförandetid för arbetsflödet. Det här är den tid det tar från det att arbetsflödet för resursuppdatering anropas till det slutförs, vilket kan avgöras av användargränssnittet för arbetsflödesrapporter. Normala målprestanda beror på storleken och typen av resurser som bearbetas och antalet renderingar. Exempel på målprestanda kan vara följande:

* under tio sekunder för bilder som är mindre än 1 280 x 1 280 pixlar med standardåtergivningar
* under en minut för bilder som är mindre än 100 MB med standardrenderingar
* under fem minuter för HD-videoklipp som är kortare än en minut

**3. Hämtningshastighet** Ett genomströmningsproblem uppstår när hämtningen från AEM DAM tar lång tid och miniatyrerna visas inte direkt när du bläddrar i DAM-administratören eller DAM Finder.

Dataöverföringsprestanda mäts i hämtningshastigheten i kilobit per sekund. Det normala målet är 300 kbit/s för 100 samtidiga nedladdningar.

**4. Faktorer som påverkar bearbetningsprestanda för resurser**

För att kunna beräkna vilken maskinvara du behöver för att bearbeta resurser bör följande aspekter beaktas:

* Bildernas upplösning i antalet pixlar
* Den heap som tilldelats AEM process

Antalet pixlar i bilden avgör bearbetningstiden - fler pixlar innebär att bearbetningen tar längre tid.
Bildtyp, komprimeringshastighet eller den relaterade storleken på filen som bilden lagras i påverkar inte den övergripande prestandan nämnvärt.

Heap har identifierats som den viktigaste begränsningsfaktorn. När resursen överskrider det tillgängliga lediga minnet minskar bearbetningsprestandan snabbt.

DAM-processerna är väl lämpade att utföras parallellt för stora mängder. När du överför resurser i en batch och processorer med flera kärnor går det snabbare att överföra den absoluta tiden per resurs.

**5. Uppskattar maskinvarukrav för bearbetning av resurser**

Omfattande bearbetning av digitala resurser kräver optimerade maskinvaruresurser, de viktigaste faktorerna är bildstorlek och högsta genomströmning för bearbetade bilder.

Allokera minst 16 GB stackutrymme och konfigurera arbetsflödet [!UICONTROL DAM Update Asset] så att det använder [Camera Raw-paketet](/help/assets/camera-raw.md) för konsumtion av råbilder.

## Förstå systemet {#understanding-the-system}

En vanlig DAM-konfiguration består av slutanvändare som använder DAM via en belastningsutjämnare. DAM-instansen kan ingå i en grupperad konfiguration, där varje DAM-instans körs i en Java™ Virtual Machine-process på en fysisk eller virtuell dator. DAM-lagring tillhandahålls antingen av en RAID-disk om det finns konfigurationer för en dator, eller av en hanterad nätverksansluten lagring om det finns grupperade konfigurationer.

I följande förklaring beskrivs de möjliga prestandafallsområdena med vissa lösningar, beroende på vad som är lämpligt.

**Nätverksanslutning till slutanvändare** En långsam nätverksanslutning kan orsaka dataflödesproblem och i vissa sällsynta fall även fördröjningsproblem. Ibland har användaren en långsam anslutning från Internet, särskilt i intranät. Detta är ett tecken på felaktig nätverkstopologi.

**Tillfälligt filsystem** Ett långsamt lokalt filsystem kan orsaka interaktiva prestandaproblem, särskilt vid sökning, eftersom sökindexen lagras på den lokala disken. Det kan också orsaka problem med resursbearbetningen om kommandoradsprocessen används.

**AEM DAM Finder** Interaktiva prestandaproblem som ofta uppstår vid sökningar orsakas av hög användning av CPU på grund av många samtidiga användare eller andra CPU-krävande processer i samma instans. Genom att gå från virtuella datorer till dedikerade datorer och se till att inga andra tjänster körs på datorn kan du förbättra prestandan. Om CPU-belastningen är hög på grund av resursbearbetning och många samtidiga användare rekommenderar dag att du lägger till ytterligare klusternoder.

**AEM DAM-arbetsflöde** Långvariga arbetsflödesprocesser vid tillgångsintag orsakar prestandaproblem vid bearbetning av resurser. Beroende på vilken typ av mediefiler som bearbetas kan detta tyda på överutnyttjande av CPU. Dag rekommenderar att du minskar antalet andra processer som körs i systemet och ökar antalet tillgängliga processorer genom att lägga till klusternoder.

**NAS-anslutning** Dålig nätverksanslutning till NAS orsakar interaktiva prestandaproblem eftersom åtkomst av nya noder under resurshanteringen blir långsammare på grund av nätverksfördröjning. Dessutom påverkar långsam nätverksgenomströmning negativt genomströmning, men även bearbetningsprestanda eftersom inläsning och sparande av återgivningar blir långsammare.

Orsaker till dålig fördröjning och genomströmning i en NAS är nätverkstopologi eller överutnyttjande av NAS av andra tjänster.

**Nätverksansluten lagring** Överanvända nätverksanslutna lagringssystem kan orsaka en mängd problem:

* Det har ofta uppstått problem med diskutrymmet som kan förebyggas genom korrekt storleksändring av ett DAM-projekt.
* Hög diskfördröjning kan spridas till långsamma åtkomsttider för CRX och kan leda till interaktiva prestandaproblem.
* Låg diskgenomströmning kan ge sämre prestanda för CQ5 DAM.

## Prestandatestning {#testing-for-performance}

För varje digitalt resurshanteringsprojekt måste du se till att det finns ett system för prestandatestning som snabbt kan identifiera och åtgärda flaskhalsar. Tänk på följande kontrollpunkter:

1. Prestandatester från början till slut med JMeter - Simulera en exempelsession för sökning och bläddring för att upptäcka interaktiva prestandaproblem.
1. Genomströmnings- och latenstester med JMeter - Att köras på en klientdator säkerställer att det inte finns några topologirelaterade problem.
1. Standardiserade materialbearbetningstester - Infoga några exempelresurser och mät tiden. Detta bör omfatta integration med externa arbetsflöden.
1. Övervaka användningen av CPU, disk och minne för varje klusternod.
1. CRX läs- och skrivprestandadiagnostik för att identifiera problem som inte är relaterade till bearbetning.
1. Övervaka nätverksfördröjning och -genomströmning från DAM-kluster till din NAS.
1. Testa, läs och skriv prestanda och diskfördröjning direkt på NAS, om det är möjligt.

## Justera flaskhalsar {#tweaking-bottlenecks}

Följande prestandaförbättringar har hittills använts i projekt:

* Selektiv renderingsgenerering: Generera bara de renderingar du behöver genom att lägga till villkor i arbetsflödet för resursbearbetning, så att mer kostsamma renderingar bara genereras för vissa resurser.
* Delat datalager mellan instanser: när det börjar ta slut på diskutrymme kan detta avsevärt minska mängden diskutrymme som behövs till priset av högre konfigurationsinsatser och förlorad automatisk rensning av datalagret.

## Ytterligare läsning {#further-reading}

* [Analyserar långsamma och blockerade processer](https://helpx.adobe.com/experience-manager/kb/AnalyzeSlowAndBlockedProcesses.html)
