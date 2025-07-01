---
title: Prestandaoptimering
description: Lär dig hur du konfigurerar vissa aspekter av AEM för att optimera prestanda.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: configuring
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: c46d9569-23e7-44e2-a072-034450f14ca2
source-git-commit: 2fcdc5df5a4b901c177d8e4158663c6b09793146
workflow-type: tm+mt
source-wordcount: '5054'
ht-degree: 1%

---

# Prestandaoptimering {#performance-optimization}

>[!NOTE]
>
>Mer information om felsökning och korrigering av prestandaproblem finns också i [prestandaträdet](/help/sites-deploying/performance-tree.md).
>
>Du kan även läsa en artikel i kunskapsbasen om [tips för prestandajustering](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-17466).

Ett viktigt problem är den tid det tar för er webbplats att svara på besökarnas förfrågningar. Även om det här värdet varierar för varje begäran kan ett genomsnittligt målvärde definieras. När det här värdet har visat sig vara både genomförbart och underhållbart kan det användas för att övervaka webbplatsens prestanda och indikera utvecklingen av potentiella problem.

De svarstider du vill ha skiljer sig åt mellan olika utvecklings- och publiceringsmiljöer, vilket återspeglar målpublikens olika egenskaper:

## Författarmiljö {#author-environment}

Den här miljön används av författare som anger och uppdaterar innehåll. Den måste tillgodose ett fåtal användare som var och en skapar ett stort antal prestandaintensiva begäranden när de uppdaterar innehållssidor och de enskilda elementen på dessa sidor.

## Publiceringsmiljö {#publish-environment}

Den här miljön innehåller innehåll som du gör tillgängligt för användarna. Här är antalet förfrågningar ännu större och hastigheten är lika viktig. Men eftersom typen av begäran är mindre dynamisk kan ytterligare prestandaförbättringar tillämpas, som cachelagring av innehållet eller belastningsutjämning.

>[!NOTE]
>
>* När du har konfigurerat för prestandaoptimering följer du anvisningarna i [Tough Day](/help/sites-developing/tough-day.md) för att testa miljön under tung belastning.
>* Se även [Prestandajusteringstips.](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-17466)

## Prestandaoptimeringsmetod {#performance-optimization-methodology}

En prestandaoptimeringsmetod för AEM Projects kan sammanfattas i fem enkla regler som kan följas för att undvika prestandaproblem redan från början:

1. [Planering för optimering](#planning-for-optimization)
1. [Simulera verklighet](#simulate-reality)
1. [Fastställ solida mål](#establish-solid-goals)
1. [Var relevant](#stay-relevant)
1. [Flexibla itereringscykler](#agile-iteration-cycles)

Dessa regler gäller för webbprojekt i allmänhet och är relevanta för projektledare och systemadministratörer för att säkerställa att deras projekt inte ställs inför prestandamässiga utmaningar när lanseringen sker.

### Planering för optimering {#planning-for-optimization}

![chlimage_1-3](assets/chlimage_1-3.jpeg)

Planera cirka 10 % av projektinsatsen för prestandaoptimeringsfasen. De faktiska prestandaoptimeringskraven beror på ett projekts komplexitet och utvecklingsteamets erfarenheter. Även om ditt projekt (i slutändan) inte behöver den tilldelade tiden är det bra rutin att alltid planera för prestandaoptimering i den föreslagna regionen.

När så är möjligt bör ett projekt först lanseras på ett mjukt sätt till en begränsad publik för att samla in verkliga erfarenheter och genomföra ytterligare optimeringar, utan det extra tryck som följer efter ett fullständigt meddelande.

Prestandaoptimeringen är inte över när du är&quot;live&quot;. Det är nu när du upplever den&quot;verkliga&quot; belastningen på ditt system. Det är viktigt att planera för ytterligare justeringar efter lanseringen.

Eftersom inläsningen av systemet ändras och prestandaprofilerna för systemet förändras över tid, bör en&quot;trimning&quot; eller&quot;hälsokontroll&quot; för prestandan schemaläggas med 6-12 månaders intervall.

### Simulera verklighet {#simulate-reality}

![chlimage_1-4](assets/chlimage_1-4.jpeg)

Om du bor med en webbplats och sedan lanseringen får du reda på att du stöter på prestandaproblem, det beror troligen på att dina belastnings- och prestandatester inte simulerade verkligheten tillräckligt bra.

Det är svårt att simulera verkligheten och hur mycket arbete du vill lägga på att bli&quot;verklig&quot; beror på projektets karaktär. &quot;Real&quot; betyder inte bara&quot;riktig kod&quot; och&quot;verklig trafik&quot;, utan även&quot;verkligt innehåll&quot;, särskilt när det gäller innehållets storlek och struktur. Mallarna kan bete sig på olika sätt beroende på databasens storlek och struktur.

### Fastställ solida mål {#establish-solid-goals}

![chlimage_1-5](assets/chlimage_1-5.jpeg)

Det är inte underskattat hur viktigt det är att uppnå prestationsmålen på ett korrekt sätt. Efter det att man fokuserat på specifika prestationsmål är det ofta svårt att ändra dessa mål efteråt, även om de bygger på antaganden.

Att uppnå goda, gedigna prestationsmål är verkligen ett av de svåraste områdena. Det är oftast bäst att samla in riktiga livsloggar och referensvärden från en jämförbar webbplats (till exempel den nya webbplatsens föregångare).

### Var relevant {#stay-relevant}

![chlimage_1-6](assets/chlimage_1-6.jpeg)

Det är viktigt att optimera en flaskhals i taget. Om du försöker göra saker parallellt utan att validera effekten av den optimerade optimeringen, kan du tappa reda på vilken optimeringsåtgärd som hjälpte.

### Flexibla itereringscykler {#agile-iteration-cycles}

![chlimage_1-7](assets/chlimage_1-7.jpeg)

Prestandajustering är en iterativ process som innefattar mätning, analys, optimering och validering tills målet nås. Om du vill ta hänsyn till den här aspekten implementerar du en flexibel valideringsprocess i optimeringsfasen i stället för en mer tung testprocess efter varje iteration.

Detta innebär att utvecklaren som implementerar optimeringen snabbt bör kunna se om optimeringen redan har nått målet. Den här informationen är värdefull eftersom optimeringen är över när målet har uppnåtts.

## Riktlinjer för grundläggande prestanda {#basic-performance-guidelines}

I allmänhet bör du spara dina ocachelagrade HTML-begäranden på mindre än 100 millisekunder. Mer specifikt kan följande fungera som riktlinjer:

* 70 % av förfrågningarna om sidor ska besvaras på mindre än 100 millisekunder.
* 25 % av förfrågningarna om sidor bör få ett svar inom 100 millisekunder - 300 millisekunder.
* 4 % av förfrågningarna om sidor bör få ett svar inom 300 millisekunder - 500 millisekunder.
* 1 % av förfrågningarna om sidor ska få ett svar inom 500 millisekunder - 1 000 millisekunder.
* Inga sidor ska svara långsammare än 1 sekund.

Numren ovan förutsätter följande villkor:

* Mätt vid publicering (inga allmänna omkostnader relaterat till en redigeringsmiljö)
* Mätt på servern (ingen nätverksbelastning)
* Inte cachelagrad (ingen AEM-utdatacache, ingen Dispatcher-cache)
* Endast för komplexa objekt med många beroenden (HTML, JS, PDF, ...)
* Ingen annan belastning på systemet

Det finns problem som ofta orsakar prestandaproblem, bland annat följande:

* Ineffektivitet i Dispatcher cachning
* Användning av frågor i vanliga visningsmallar.

JVM- och OS-nivåjustering leder vanligtvis inte till några större prestandaförluster och bör därför utföras i slutet av optimeringscykeln.

Det sätt på vilket en innehållsdatabas är strukturerad kan även påverka prestanda. För bästa prestanda bör antalet underordnade noder som är kopplade till enskilda noder i en innehållsdatabas inte överstiga 1 000 (som regel).

Dina bästa vänner under en vanlig prestandaoptimeringsövning är:

* `request.log`
* Komponentbaserad timing
* En Java™-profilerare.

### Prestanda vid inläsning och redigering av Digital Assets {#performance-when-loading-and-editing-digital-assets}

På grund av den stora datamängden som används vid inläsning och redigering av digitala resurser kan prestanda bli ett problem.

Två saker påverkar prestandan här:

* CPU - flera kärnor ger jämnare arbete vid transkodning
* Hårddisk - parallella RAID-diskar uppnår samma

Om du vill förbättra prestanda bör du tänka på följande:

* Hur många mediefiler överförs per dag? En god uppskattning kan baseras på följande:

![chlimage_1-77](assets/chlimage_1-77.png)

* Tidsram för redigering (vanligtvis arbetsdagens längd, mer för internationella operationer).
* Genomsnittlig storlek på överförda bilder (och storleken på återgivningar som genereras per bild) i MB.
* Bestäm den genomsnittliga datahastigheten:

![chlimage_1-78](assets/chlimage_1-78.png)

* 80 % av alla redigeringar görs på 20 % av tiden, så vid maximal tid har du fyra gånger högre datahastighet än den genomsnittliga. Detta är ditt mål.

## Prestandaövervakning {#performance-monitoring}

Prestanda (eller avsaknaden av) är en av de första saker som användarna märker, så som med andra program med ett användargränssnitt är prestanda av avgörande betydelse. Om du vill optimera prestanda för din AEM-installation ska du övervaka olika attribut för instansen och dess beteende.

Mer information om hur du utför prestandaövervakning finns i [Övervaka prestanda](/help/sites-deploying/monitoring-and-maintaining.md#monitoring-performance).

De problem som orsakar prestandaproblem är ofta svåra att spåra, även när effekterna är enkla att se.

En grundläggande utgångspunkt är goda kunskaper i systemet när det fungerar som vanligt. Om du inte vet hur miljön &quot;ser ut&quot; och &quot;fungerar&quot; när den fungerar som den ska, är det svårt att hitta problemet när prestandan försämras. Lägg tid på att undersöka systemet när det körs smidigt och se till att det är en pågående uppgift att samla in prestandainformation. På så sätt får du en grund att jämföra om prestandan skulle försämras.

I följande diagram visas den väg som en begäran om AEM-innehåll kan ta och därför antalet olika element som kan påverka prestandan.

![chlimage_1-79](assets/chlimage_1-79.png)

Prestanda är också en balans mellan volym och kapacitet:

* **Volym** - Den mängd utdata som bearbetas och levereras av systemet.
* **Kapacitet** - Systemet kan leverera volymen.

Prestanda kan illustreras på olika platser i hela webbkedjan.

![chlimage_1-80](assets/chlimage_1-80.png)

Det finns flera funktionsområden som ofta är ansvariga för att påverka prestandan:

* Cachning
* Programkod (ditt projekt)
* Sökfunktionalitet

### Grundläggande regler för prestanda {#basic-rules-regarding-performance}

Vissa regler bör beaktas vid prestandaoptimering:

* Prestandajustering *måste* vara en del av varje projekt.
* Optimera inte tidigt i utvecklingscykeln.
* Prestanda är bara så bra som den svagaste länken.
* Tänk alltid på kapacitet kontra volym.
* Optimera viktiga saker först.
* Optimera aldrig utan *realistiska* mål.

>[!NOTE]
>
>Tänk på att den mekanism du använder för att mäta prestanda ofta påverkar exakt det du försöker mäta. Försök att ta hänsyn till dessa skillnader och eliminera så mycket av deras effekt som möjligt. I synnerhet bör webbläsarplugin-program inaktiveras när det är möjligt.

## Konfigurera för prestanda {#configuring-for-performance}

Vissa aspekter av AEM (och/eller den underliggande databasen) kan konfigureras för att optimera prestanda. Följande är möjligheter och förslag. Du måste vara säker på om du använder funktionerna innan du gör några ändringar eller inte.

### Sökindexering {#search-indexing}

Från och med AEM 6.0 använder Adobe Experience Manager en Oak-baserad databasarkitektur.

Den uppdaterade indexeringsinformationen finns här:

* [Metodtips för frågor och indexering](/help/sites-deploying/best-practices-for-queries-and-indexing.md)
* [Frågor och indexering](/help/sites-deploying/queries-and-indexing.md)

### Samtidig bearbetning av arbetsflöden {#concurrent-workflow-processing}

Om du vill förbättra prestanda bör du begränsa antalet arbetsflödesprocesser som körs samtidigt. Som standard behandlar arbetsflödesmotorn så många arbetsflöden parallellt som det finns processorer tillgängliga för den virtuella datorn Java™. När arbetsflödessteg kräver stora mängder bearbetningsresurser (RAM eller CPU) kan du ställa höga krav på tillgängliga serverresurser om du kör flera av dessa arbetsflöden parallellt.

När bilder (eller DAM-resurser i allmänhet) överförs, importeras bilderna automatiskt till DAM i arbetsflöden. Bilderna är ofta högupplösta och kan lätt ta upp hundratals MB av stacken för bearbetning. När du hanterar dessa bilder parallellt placeras en hög belastning på undersystemet för minne och skräpinsamlaren.

Arbetsflödesmotorn använder Apache Sling-jobbköer för hantering och schemaläggning av bearbetning av arbetsobjekt. Följande jobbkötjänster har skapats som standard från tjänsten Apache Sling Job Queue Configuration för bearbetning av arbetsflödesjobb:

<!-- TODO: Change the reference to 6.5 LTS javadocs -->
* Begränsa arbetsflödeskö: De flesta arbetsflödessteg, t.ex. de som bearbetar DAM-resurser, använder tjänsten Begränsa arbetsflödeskö.
* Begränsa arbetsflödets externa processjobbkö: Den här tjänsten används för särskilda externa arbetsflödessteg som vanligtvis används för att kontakta ett externt system och avfråga resultat. Exempel: InDesign Media Extraction Process-steget implementeras som en extern process. Arbetsflödesmotorn använder den externa kön för att bearbeta avsökningen. (Se [com.day.cq.workflow.exec.WorkflowExternalProcess](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/workflow/exec/WorkflowExternalProcess.html).)

Konfigurera de här tjänsterna för att begränsa antalet arbetsflödesprocesser som körs samtidigt.

>[!NOTE]
>
>När du konfigurerar dessa jobbköer påverkas alla arbetsflöden såvida du inte har skapat en jobbkö för en viss arbetsflödesmodell (se [Konfigurera kön för en viss arbetsflödesmodell](/help/sites-deploying/configuring-performance.md#configure-the-queue-for-a-specific-workflow) nedan).

#### Konfiguration i databasen {#configuration-in-the-repo}

Om du konfigurerar tjänsterna [med en sling:OsgiConfig-nod](/help/sites-deploying/configuring-osgi.md#adding-a-new-configuration-to-the-repository) måste du hitta PID för de befintliga tjänsterna, till exempel: org.apache.sling.event.job.QueueConfiguration.370aad73-d01b-4a0b-abe4-2019 8d85f 705. Du kan identifiera ditt PID med webbkonsolen.

Konfigurera egenskapen `queue.maxparallel`.

#### Konfiguration i webbkonsolen {#configuration-in-the-web-console}

Om du vill konfigurera de här tjänsterna [med webbkonsolen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-with-the-web-console) ska du leta reda på de befintliga konfigurationsobjekten under konfigurationstjänstfabriken för Apache Sling-jobbkön.

Konfigurera egenskapen Maximum Parallel Jobs.

### Konfigurera kön för ett specifikt arbetsflöde {#configure-the-queue-for-a-specific-workflow}

Skapa en jobbkö för en viss arbetsflödesmodell så att du kan konfigurera jobbhantering för den arbetsflödesmodellen. På så sätt påverkar dina konfigurationer bearbetningen för ett visst arbetsflöde, medan konfigurationen av standardarbetsflödeskön för Granite styr bearbetningen av andra arbetsflöden.

När arbetsflödesmodeller körs skapas Sling-jobb för ett specifikt ämne. Som standard matchar ämnet de ämnen som är konfigurerade för den allmänna arbetsflödeskön för Begränsa arbetsflöde eller den externa jobbkön för Begränsa arbetsflöde:

* `com/adobe/granite/workflow/job*`
* `com/adobe/granite/workflow/external/job*`

Faktiska jobbämnen som arbetsflödesmodeller genererar innehåller modellspecifikt suffix. Arbetsflödesmodellen **DAM Update Asset** genererar till exempel jobb med följande ämne:

`com/adobe/granite/workflow/job/etc/workflow/models/dam/update_asset/jcr_content/model`

Därför kan du skapa en jobbkö för ämnet som matchar jobbavsnitten i arbetsflödesmodellen. Om du konfigurerar de prestandarelaterade egenskaperna för kön påverkas bara arbetsflödesmodellen som genererar jobben som matchar köavsnittet.

Följande procedur skapar en jobbkö för ett arbetsflöde med arbetsflödet **DAM Update Asset** som exempel.

1. Kör arbetsflödesmodellen som du vill skapa jobbkön för så att ämnesstatistik genereras. Lägg till exempel till en bild i Assets för att köra arbetsflödet **DAM Update Asset**.
1. Öppna Sling Jobs-konsolen (`https://<host>:<port>/system/console/slingevent`).
1. Identifiera arbetsflödesrelaterade ämnen i konsolen. Följande avsnitt finns för DAM Update Asset:

   * `com/adobe/granite/workflow/external/job/etc/workflow/models/dam/update_asset/jcr_content/model`
   * `com/adobe/granite/workflow/job/etc/workflow/models/dam/update_asset/jcr_content/model`
   * `com/adobe/granite/workflow/job/etc/workflow/models/dam-xmp-writeback/jcr_content/model`

1. Skapa en jobbkö för vart och ett av dessa ämnen. Om du vill skapa en jobbkö skapar du en fabrikskonfiguration för fabrikstjänsten för Apache Sling Job Queue.

   Fabrikskonfigurationerna liknar den Beviljade arbetsflödeskön som beskrivs i [Samtidig arbetsflödesbearbetning](/help/sites-deploying/configuring-performance.md#concurrent-workflow-processing), förutom att egenskapen Ämnen matchar ämnet för dina arbetsflödesjobb.

### AEM DAM Asset Synchronization Service {#cq-dam-asset-synchronization-service}

`AssetSynchronizationService` används för att synkronisera resurser från monterade databaser (inklusive LiveLink, Documentum®, bland annat). Som standard utförs en regelbunden kontroll var 300:e sekund (5 minuter), så om du inte använder monterade databaser kan du inaktivera den här tjänsten.

Inaktiveringen av tjänsten görs genom att [konfigurera OSGi-tjänsten](/help/sites-deploying/configuring-osgi.md) **CQ DAM Asset Synchronization Service** för att ställa in **Synkroniseringsperioden** ( `scheduler.period`) till (minst) ett år (definieras i sekunder).

### Flera DAM-instanser {#multiple-dam-instances}

Distribuering av flera DAM-instanser kan hjälpa prestandan när till exempel:

* Du har en hög belastning på grund av regelbunden överföring av många resurser för författarmiljön. Här kan en separat DAM-instans användas för att betjäna författaren.
* Du har flera team i hela världen (till exempel USA, Europa, Asien).

Ytterligare överväganden är:

* Separera pågående arbete från författare vid publicering
* Avgränsa interna användare vid författare från externa besökare/användare vid publicering (t.ex. agenter, pressrepresentanter, kunder och studenter).

## Best Practices for Quality Assurance {#best-practices-for-quality-assurance}

Prestanda är av stor betydelse för er publiceringsmiljö. Därför måste du noga planera och analysera prestandatesterna som du gör för publiceringsmiljön när du implementerar projektet.

Det här avsnittet syftar till att ge en standardiserad översikt över problemen med att definiera ett testkoncept specifikt för prestandatester i din *publiceringsmiljö* . Den här informationen är främst av intresse för kvalitetstekniker, projektledare och systemadministratörer.

Nedan beskrivs en standardiserad metod för prestandatester för ett AEM-program i *publiceringsmiljön* . Prestandatestet omfattar följande fem faser:

* [Kunskapsverifiering](#verification-of-knowledge)
* [Definition av tillämpningsområde](#scope-definition)
* [Testmetoder](#test-methodologies)
* [Definition av prestandamål](#defining-the-performance-goals)
* [Optimering](#optimization)

Kontrollen är en extra, heltäckande process - nödvändig men inte begränsad till testning.

### Kunskapsverifiering {#verification-of-knowledge}

Ett första steg är att dokumentera den grundläggande information som du måste känna till innan du kan börja testa:

* Testmiljöns arkitektur
* En programkarta som beskriver de interna element som behöver testas (både separat och kombinerat)

#### Testarkitektur {#test-architecture}

Dokumentera arkitekturen för testmiljön som används för prestandatestning.

Du behöver en återgivning av den planerade produktionsmiljön, tillsammans med Dispatcher och belastningsutjämnaren.

#### Programkarta {#application-map}

Få en tydlig översikt som du kan använda för att skapa en karta över hela programmet (du kanske redan har den här kartan från tester i redigeringsmiljön).

En diagramrepresentation av de interna elementen i programmet kan ge en översikt över testkraven. Med färgkodning kan den också fungera som grund för rapportering.

### Omfattningsdefinition {#scope-definition}

Ett program har vanligtvis ett urval av användningsfall. Vissa användningsområden är viktiga, andra mindre viktiga.

Adobe rekommenderar att du definierar följande för att fokusera på omfattningen av prestandatestningen vid publicering:

* Viktigaste användningsområdena
* Mest kritiska tekniska användningsfall

Antalet användningsfall är upp till dig, men bör begränsas till ett enkelt hanterbart antal (till exempel mellan 5 och 10).

När de viktigaste användningsfallen har valts kan nyckelutförandeindikatorerna (KPI) och de verktyg som används för att mäta dem definieras för varje fall. Exempel på vanliga nyckeltal är:

* Svarstid från slut till slut
* Svarstid för server
* Svarstid för en enskild komponent
* Svarstid för tjänsterna
* Antal inaktiva trådar i trådpoolen
* Antal kostnadsfria anslutningar
* Systemresurser som CPU- och I/O-åtkomst

### Testmetoder {#test-methodologies}

Det här konceptet har fyra scenarier som används för att definiera och testa prestationsmålen:

* Enkomponentstester
* Kombinerade komponenttester
* *Går live*-scenario
* Felscenarier

Baserat på följande principer.

#### Komponentbrytpunkter {#component-breakpoints}

* Varje komponent har en specifik brytpunkt när den är relaterad till prestanda. Det innebär att en komponent kan visa att bra prestanda fungerar tills en viss punkt nås, varefter prestanda försämras snabbt.
* För att få en fullständig översikt över programmet måste du först verifiera dina komponenter för att avgöra när brytpunkten för varje komponent nås.
* Om du vill hitta brytpunkten som du kan utföra ett inläsningstest där du under en tidsperiod ökar antalet användare för att skapa en ökad belastning. Genom att övervaka den här inläsningen och komponenternas respons får du ett specifikt prestandabeteende när komponentens brytpunkt nås. Poängen kan kvalificeras med antalet samtidiga transaktioner per sekund, tillsammans med antalet samtidiga användare (om komponenten är känslig för denna KPI).
* Denna information kan sedan fungera som riktmärke för förbättringar, ange effektiviteten hos de åtgärder som används och hjälpa till att definiera testscenarier.

#### Transaktioner {#transactions}

* Termen transaktion används för att representera begäran från en hel webbsida, inklusive själva sidan och alla efterföljande anrop. Det vill säga, sidbegäran, eventuella AJAX-anrop, bilder och andra objekt **Begär nedladdning**.
* Om du vill analysera varje begäran fullständigt kan du representera varje element i anropsstacken och sedan summera den genomsnittliga bearbetningstiden för varje.

### Definiera prestandamål {#defining-the-performance-goals}

När omfånget och relaterade nyckeltal har definierats, ställs de specifika prestationsmålen in. Den här processen innebär att du utformar testscenarier tillsammans med målvärden.

Provningsprestanda under både genomsnittliga och högsta förhållanden. Dessutom behöver ni Going Live-scenariotester för att försäkra er om att ni kan tillgodose det ökade intresset för er webbplats när den blir tillgänglig för första gången.

Alla erfarenheter och all statistik som du har samlat in från en befintlig webbplats kan också vara användbara när du ska fastställa framtida mål. Till exempel topptrafik från din webbplats.

#### Enstaka komponenttester {#single-component-tests}

Kritiska komponenter måste testas - både under medelvärdes- och toppförhållanden.

I båda fallen kan du definiera det förväntade antalet transaktioner per sekund när ett fördefinierat antal användare använder systemet.

| Komponent | Testtyp | Nej. Användare | Tx/sek (förväntas) | Tx/sek (testad) | Beskrivning |
|---|---|---|---|---|---|
| Startsida - en användare | Genomsnittlig | 1 | 1 |  |  |
|   | Toppvärde | 1 | 3 |  |  |
| Startsida 100 användare | Genomsnittlig | 100 | 3 |  |  |
|   | Toppvärde | 100 | 3 |  |

#### Komponenttester {#combined-component-tests}

Genom att testa komponenterna i kombination får du en närmare bild av hur programmen fungerar. Även här måste man testa genomsnittliga och högsta förhållanden.

| Scenario | Komponent | Nej. Användare | Tx/sek (förväntas) | Tx/sek (testad) | Beskrivning |
|---|---|---|---|---|---|
| Blandat genomsnitt | Hemsida | 10 | 1 |  |  |
|   | Sök | 10 | 1 |  |  |
|   | Nyheter | 10 | 2 |  |  |
|   | Händelser | 10 | 1 |  |  |
|   | Aktiveringar | 10 | 3 |  | Simulering av författarbeteende. |
| Blandad topp | Hemsida | 100 | 5 |  |  |
|   | Sök | 50 | 5 |  |  |
|   | Nyheter | 100 | 10 |  |  |
|   | Händelser | 100 | 10 |  |  |
|   | Aktiveringar | 20 | 20 |  | Simulering av författarbeteende. |

#### Pågående direkttester {#going-live-tests}

Under de första dagarna efter det att webbplatsen har tillgängliggjorts kan du förvänta dig ett ökat intresse. Detta scenario är ännu större än de toppvärden som du testar för. Adobe rekommenderar att du testar Going Live-scenarier för att säkerställa att systemet klarar detta.

| Scenario | Testtyp | Nej. Användare | Tx/sek (förväntas) | Tx/sek (testad) | Beskrivning |
|---|---|---|---|---|---|
| Live-topp på väg | Hemsida | 200 | 20 |  |  |
|   | Sök | 100 | 10 |  |  |
|   | Nyheter | 200 | 20 |  |  |
|   | Händelser | 200 | 20 |  |  |
|   | Aktiveringar | 20 | 20 |  | Simulering av författarbeteende. |

#### Felscenariotest {#error-scenario-tests}

Testa felscenarier för att säkerställa att systemet reagerar korrekt och korrekt. Inte bara hur själva felet hanteras, utan även hur det kan påverka prestandan. Till exempel:

* Vad händer när användaren försöker ange ett ogiltigt sökord i sökrutan
* Vad som händer när söktermen är så allmän att den returnerar ett stort antal resultat

När man utformar dessa tester bör man komma ihåg att inte alla scenarier inträffar regelbundet. Deras inverkan på hela systemet är dock viktig.

| Felscenario | Feltyp | Nej. Användare | Tx/sek (förväntas) | Tx/sek (testad) | Beskrivning |
|---|---|---|---|---|---|
| Överlagring av sökkomponent | Sök på globalt jokertecken (asterisk) | 10 | 1 |  | Endast &ast;&ast;&ast; söks igenom. |
|   | Stoppord | 20 | 2 |  | Söker efter ett stoppord. |
|   | Tom sträng | 10 | 1 |  | Söker efter en tom sträng. |
|   | Specialtecken | 10 | 1 |  | Söker efter specialtecken. |

#### Bevarandetester {#endurance-tests}

Vissa problem uppstår bara efter att systemet har körts kontinuerligt, antingen timmar eller dagar. En uthållighetsprovning används för att testa en konstant genomsnittlig belastning under en viss tidsperiod. Alla prestandaförsämringar kan sedan analyseras.

| Scenario | Testtyp | Nej. Användare | Tx/sek (förväntas) | Tx/sek (testad) | Beskrivning |
|---|---|---|---|---|---|
| Varaktighetsprovning (72 timmar) | Hemsida | 10 | 1 |  |  |
|   | Sök | 10 | 1 |  |  |
|   | Nyheter | 20 | 2 |  |  |
|   | Händelser | 10 | 1 |  |  |
|   | Aktiveringar | 1 | 3 |  | Simulering av författarbeteende. |

### Optimering {#optimization}

I de senare implementeringsfaserna optimerar du programmet så att det uppfyller och maximerar prestandamålen.

Alla optimeringar måste testas för att säkerställa att de har:

* Påverkar inte funktionen
* har verifierats med lastproven innan de släpps

Det finns ett urval verktyg som kan hjälpa dig med lastgenerering, prestandaövervakning och resultatanalys. Några av dessa verktyg är följande:

* [JMeter](https://jmeter.apache.org/)
* [Prestandautveckling för OpenText Professional](https://www.opentext.com/products/professional-performance-engineering).
* [Interaktiv Java™-profil](https://jiprof.sourceforge.net/)

Efter optimeringen testar du igen för att bekräfta påverkan.

### Rapportering {#reporting}

Kontinuerlig rapportering håller alla informerade om statusen. Som tidigare nämnts med färgkodning kan arkitekturschemat användas för den här statusen.

När alla tester är slutförda, rapportera följande:

* Allvarliga fel påträffades
* Icke-kritiska problem som fortfarande kräver mer utredning
* Eventuella antaganden som gjorts under testningen
* Eventuella rekommendationer som ska följa av testningen

## Optimera prestanda när du använder Dispatcher {#optimizing-performance-when-using-the-dispatcher}

[Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) är Adobe verktyg för cachelagring och/eller belastningsutjämning. När du använder Dispatcher bör du överväga att optimera webbplatsen för att få cacheprestanda.

>[!NOTE]
>
>Dispatcher-versionerna är oberoende av AEM, men Dispatcher-dokumentationen är inbäddad i AEM-dokumentationen. Använd alltid den Dispatcher-dokumentation som är inbäddad i dokumentationen för den senaste versionen av AEM.
>
>Du kan ha omdirigerats till den här sidan om du har följt en länk till Dispatcher-dokumentationen som är inbäddad i dokumentationen för en tidigare version av AEM.

Dispatcher har flera inbyggda funktioner som du kan använda för att optimera prestanda om webbplatsen utnyttjar dem. I det här avsnittet beskrivs hur du utformar din webbplats för att maximera fördelarna med cachning.

>[!NOTE]
>
>Det kan hjälpa dig att komma ihåg att Dispatcher lagrar cachen på en standardwebbserver. Om du känner till den här informationen kan du cachelagra allt som du kan lagra som en sida och begära med hjälp av en URL. Och du kan inte lagra andra saker, som cookies, sessionsdata och formulärdata.
>
>I allmänhet innebär många cachelagringsstrategier att du måste välja bra URL:er och inte förlita dig på dessa ytterligare data.
>
>Med Dispatcher version 4.1.11 kan du även cachelagra svarshuvuden, se [Cachelagra HTTP-svarshuvuden](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-the-dispatcher-cache-cache).
>

### Beräkna Dispatcher-cacheförhållandet {#calculating-the-dispatcher-cache-ratio}

Cachekvotsformeln uppskattar procentandelen begäranden som hanteras av cachen av det totala antalet begäranden som kommer in i systemet. För att beräkna cachekvoten behöver du följande:

* Totalt antal begäranden. Den här informationen är tillgänglig i Apache `access.log`. Mer information finns i den [officiella Apache-dokumentationen](https://httpd.apache.org/docs/2.4/logs.html#accesslog).

* Antalet begäranden som den publicerade instansen betjänade. Den här informationen är tillgänglig i instansens `request.log`. Mer information finns i [Tolka request.log](/help/sites-deploying/monitoring-and-maintaining.md#interpreting-the-request-log) och [Söka efter loggfiler](/help/sites-deploying/monitoring-and-maintaining.md#finding-the-log-files).

Formeln för beräkning av cacheförhållandet är:

* (Det totala antalet begäranden **minus** antalet begäranden vid publicering) **delat** med det totala antalet begäranden.

Om det totala antalet begäranden till exempel är 129491 och antalet begäranden som hanteras av Publish-instansen är 58959 är cachekvoten: **(129491 - 58959)/129491= 54,5 %**.

Om du inte har någon personlig koppling mellan utgivaren och utgivaren kan du lägga till förfrågningar från alla avsändare och utgivare tillsammans för att få en korrekt mätning. Se även [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md).

>[!NOTE]
>
>För bästa prestanda rekommenderar Adobe en cachekvot på 90 % till 95 %.

#### Använda konsekvent sidkodning {#using-consistent-page-encoding}

Med Dispatcher version 4.1.11 kan du cachelagra svarshuvuden. Om du inte cachelagrar svarshuvuden på Dispatcher kan det uppstå problem om du lagrar sidkodningsinformation i sidhuvudet. I det här fallet används webbserverns standardkodning för sidan när Dispatcher visar en sida från cachen. Det finns två sätt att undvika det här problemet:

* Om du bara använder en kodning kontrollerar du att kodningen som används på webbservern är densamma som standardkodningen på AEM webbplats.
* Om du vill ställa in kodningen använder du en `<META>`-tagg i HTML `head` -avsnittet, som i följande exempel:

```xml
        <META http-equiv="Content-Type" content="text/html; charset=EUC-JP">
```

#### Undvik URL-parametrar {#avoid-url-parameters}

Undvik om möjligt URL-parametrar för sidor som du vill cachelagra. Om du till exempel har ett bildgalleri cachelagras aldrig följande URL (såvida inte Dispatcher [har konfigurerats därefter](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-the-dispatcher-cache-cache)):

```xml
www.myCompany.com/pictures/gallery.html?event=christmas&amp;page=1
```

Du kan dock placera de här parametrarna i sidans URL-adress enligt följande:

```xml
www.myCompany.com/pictures/gallery.christmas.1.html
```

>[!NOTE]
>
>Denna URL anropar samma sida och samma mall som `gallery.html`. I malldefinitionen kan du ange vilket skript som ska återge sidan eller använda samma skript för alla sidor.

#### Anpassa efter URL {#customize-by-url}

Om du tillåter användare att ändra teckensnittsstorleken (eller annan layoutanpassning) kontrollerar du att de olika anpassningarna återspeglas i webbadressen.

Till exempel cachelagras inte cookies, så om du sparar teckensnittsstorleken i en cookie (eller på liknande sätt) bevaras inte teckensnittsstorleken för den cachelagrade sidan. Därför returnerar Dispatcher dokument med valfri teckensnittsstorlek på måfå.

Om du tar med teckensnittsstorleken i URL:en som väljare undviker du det här problemet:

```xml
www.myCompany.com/news/main.large.html
```

>[!NOTE]
>
>I de flesta layoutaspekter går det även att använda formatmallar, klientskript eller båda. Dessa instrument fungerar bra med cachning.
>
>Den här strategin är också användbar för en utskriftsversion där du kan använda en URL-adress som:
>
>`www.myCompany.com/news/main.print.html`
>
>Med hjälp av skriptordlistan för malldefinitionen kan du ange ett separat skript som återger utskriftssidorna.

#### Invaliderar bildfiler som används som titlar {#invalidating-image-files-used-as-titles}

Om du återger sidrubriker, eller annan text, som bilder bör du lagra filerna så att de tas bort vid en innehållsuppdatering på sidan:

1. Placera bildfilen i samma mapp som sidan.
1. Använd följande namnformat för bildfilen:

   `<page file name>.<image file name>`

Du kan till exempel lagra sidans rubrik `myPage.html` i `file myPage.title.gif`. Den här filen tas automatiskt bort om sidan uppdateras, så alla ändringar av sidans titel återspeglas automatiskt i cachen.

>[!NOTE]
>
>Bildfilen finns inte nödvändigtvis fysiskt på AEM-instansen. Du kan använda ett skript som skapar bildfilen dynamiskt. Dispatcher lagrar sedan filen på webbservern.

#### Ogiltiga bildfiler som används för navigering {#invalidating-image-files-used-for-navigation}

Om du använder bilder för navigeringsposterna är metoden i stort sett densamma som med titlar, men något mer komplex. Lagra alla navigeringsbilder med målsidorna. Om du använder två bilder för normal och aktiv användning kan du använda följande skript:

* Ett skript som visar sidan som vanligt.
* Ett skript som bearbetar &quot;.normal&quot;-begäranden och returnerar den normala bilden.
* Ett skript som bearbetar &quot;.active&quot;-begäranden och returnerar den aktiverade bilden.

Det är viktigt att du skapar dessa bilder med samma namngivningshandtag som sidan, så att du är säker på att en innehållsuppdatering tar bort dessa bilder och sidan.

För sidor som inte ändras finns bilderna kvar i cacheminnet, även om själva sidorna är automatiskt ogiltiga.

#### Personalization {#personalization}

Vi rekommenderar att du begränsar personaliseringen till där det är nödvändigt. Så här visar du varför:

* Om du använder en fritt anpassningsbar startsida måste den sidan sammanställas varje gång en användare begär den.
* Om du däremot erbjuder ett alternativ på tio olika startsidor kan du cachelagra var och en av dem, vilket förbättrar prestandan.

>[!TIP]
>Mer information om hur du konfigurerar cacheminnet för Dispatcher finns i [självstudiekursen för AEM Dispatcher-cachen](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/overview.html) och i avsnittet [Cachelagra skyddat innehåll.](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-1.html#dispatcher-tips-and-tricks)

Om du anpassar varje sida genom att placera användarens namn i namnlisten (till exempel), har det en prestandaeffekt.

>[!TIP]
>Mer information om cachelagring av skyddat innehåll finns i [Cachelagra skyddat innehåll](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html) i Dispatcher-handboken.

När det gäller att blanda begränsat och offentligt innehåll på en sida bör du överväga en strategi där SSI används i Dispatcher, eller också innehåller klientsidan Ajax i webbläsaren.

>[!TIP]
>
>Information om hur du hanterar blandat offentligt och begränsat innehåll finns i [Konfigurera dynamisk SSLING-infogning.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-sling-dynamic-include.html)

#### Fästiga anslutningar {#sticky-connections}

[Antagliga anslutningar](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html#the-benefits-of-load-balancing) säkerställer att dokumenten för en användare är sammansatta på samma server. Om en användare lämnar den här mappen och senare återgår till den, stannar anslutningen fortfarande kvar. Om du vill lagra alla dokument som kräver klisterlappar för webbplatsen, definierar du en mapp. Försök att inte ha med andra dokument i den. Det här scenariot påverkar belastningsutjämningen om du använder personaliserade sidor och sessionsdata.

#### MIME-typer {#mime-types}

Det finns två sätt som en webbläsare kan använda för att avgöra vilken typ av fil det är:

1. Med filnamnstillägget (till exempel `.html`, `.gif` och `.jpg`).
1. Med MIME-typen som servern skickar med filen.

För de flesta filer används MIME-typen i filtillägget. Det vill säga,

1. Med filnamnstillägget (till exempel `.html`, `.gif` och `.jpg`).
1. Med MIME-typen som servern skickar med filen.

Om filnamnet saknar filtillägg visas det som oformaterad text.

Med Dispatcher version 4.1.11 kan du cachelagra svarshuvuden. Om du inte cachelagrar svarshuvuden på Dispatcher är MIME-typen en del av HTTP-huvudet. Om ditt AEM-program returnerar filer som inte har ett känt filslut, och i stället använder MIME-typen, kan dessa filer visas felaktigt.

Följ dessa riktlinjer för att vara säker på att filerna cachelagras korrekt:

* Kontrollera att filerna alltid har rätt filtillägg.
* Undvik generiska skript för filservrar, som har URL-adresser som `download.jsp?file=2214`. Om du vill använda URL:er som innehåller filspecifikationen skriver du om skriptet. I föregående exempel är den här omskrivningen `download.2214.pdf`.
