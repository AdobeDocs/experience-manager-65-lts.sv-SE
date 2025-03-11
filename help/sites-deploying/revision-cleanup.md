---
title: Revision Cleanup
description: Lär dig hur du använder Revision Cleanup-funktionen i Adobe Experience Manager 6.5.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
feature: Administering
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 114a77bc-0b7e-49ce-bca1-e5195b4884dc
source-git-commit: 3cbc2ddd4ff448278e678d1a73c4ee7ba3af56f4
workflow-type: tm+mt
source-wordcount: '5139'
ht-degree: 0%

---

# Revision Cleanup{#revision-cleanup}

## Introduktion {#introduction}

Varje uppdatering av databasen skapar en innehållsändring. Det innebär att databasstorleken ökar för varje uppdatering. Gamla revideringar måste rensas för att frigöra diskutrymme - det är viktigt för att undvika okontrollerad databastillväxt. Den här underhållsfunktionen kallas Revision Cleanup. Det har funnits som en offlinerutrutin sedan Adobe Experience Manager (AEM) 6.0.

Med AEM 6.3 och senare introducerades en onlineversion av den här funktionen som kallas Online Revision Cleanup. Jämfört med offlineredigering där AEM-instansen måste stängas av, kan onlineredigering köras när AEM-instansen är online. Rensa onlineändringar är aktiverat som standard och det är det rekommenderade sättet att rensa en revision.

**Obs!**: [I videon ](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/use-online-revision-clean-up.html) finns en introduktion och information om hur du använder rensning av onlineversioner.

Revideringsrensningsprocessen består av tre faser: **uppskattning**, **komprimering** och **rensning**. Beräkningen avgör om nästa fas (komprimering) ska köras eller inte baserat på hur mycket skräp som kan samlas in. Under komprimeringsfasen skrivs segment och tjärfiler om, utan att något innehåll som inte används tas bort. Rensa upp-fasen tar sedan bort de gamla segmenten, inklusive eventuellt skräp som de innehåller. Offlineläget kan vanligtvis frigöra mer utrymme eftersom onlineläget måste ta hänsyn till AEM arbetsgrupper som inte samlar in fler segment.

Mer information om Revision Cleanup finns på följande länkar:

* [Så här kör du rensning av onlineändringar](/help/sites-deploying/revision-cleanup.md#how-to-run-online-revision-cleanup)
* [Vanliga frågor och svar om rensning av onlineversioner](/help/sites-deploying/revision-cleanup.md#online-revision-cleanup-frequently-asked-questions)
* [Så här kör du borttagning av offlinerevision](/help/sites-deploying/revision-cleanup.md#how-to-run-offline-revision-cleanup)

Du kan även läsa den [officiella Oak-dokumentationen](https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html).

### När ska onlinerevision rensas i stället för Offline Revision Cleanup? {#when-to-use-online-revision-cleanup-as-opposed-to-offline-revision-cleanup}

**Rensning av onlineändringar rekommenderas för att utföra rensning av revisioner.Rensning av** offlineredigering bör endast användas i undantagsfall, till exempel innan du migrerar till det nya lagringsformatet eller om du har ombetts att göra det av Adobe kundtjänst.

## Så här kör du rensning av onlineändringar {#how-to-run-online-revision-cleanup}

Rensa onlineändringar är konfigurerat som standard så att det automatiskt körs en gång om dagen på både AEM Author- och Publish-instanser. Allt du behöver göra är att definiera underhållsfönstret under en period med minsta användaraktivitet. Du kan konfigurera rensningsaktiviteten för onlineändringar på följande sätt:

1. I huvudfönstret i AEM går du till **Verktyg - Åtgärder - Kontrollpanel - Underhåll** eller pekar din webbläsare på: `https://serveraddress:serverport/libs/granite/operations/content/maintenance.html`

   ![chlimage_1-90](assets/chlimage_1-90.png)

1. Håll muspekaren över **fönstret Dagligt underhåll** och klicka på ikonen **Inställningar** .

   ![chlimage_1-91](assets/chlimage_1-91.png)

1. Ange önskade värden (upprepning, starttid, sluttid) och klicka på **Spara**.

   ![chlimage_1-92](assets/chlimage_1-92.png)

Om du vill köra revideringsrensningen manuellt kan du:

1. Gå till **Verktyg - Åtgärder - Kontrollpanel - Underhåll** eller bläddra direkt till `https://serveraddress:serverport/libs/granite/operations/content/maintenance.html`
1. Klicka på **Daglig underhållsplan**.
1. Håll muspekaren över ikonen **Revision Cleanup** .
1. Klicka på **Kör**.

   ![chlimage_1-93](assets/chlimage_1-93.png)

### Online Revision Cleanup After Offline Revision Cleanup {#running-online-revision-cleanup-after-offline-revision-cleanup}

Rensningsprocessen för revision återtar gamla versioner av generationer. Det innebär att varje gång du kör en revideringsrensning skapas en ny generation som sparas på disken. Det finns dock en skillnad mellan de två sorteringstyperna av revisioner: när du rensar en version offline behålls en generation medan rensning av onlineversioner behåller två generationer. Så när du kör rensning av onlinerevision **efter** rensas offlinerevisionen händer följande:

1. När den första rensningen av onlineversionen har gjorts dubbleras databasstorleken. Det beror på att det nu finns två generationer som finns på disken.
1. Under efterföljande körningar kommer databasen att växa tillfälligt medan den nya generationen skapas och sedan stabiliseras tillbaka till den storlek som den hade efter den första körningen, när rensningsprocessen för onlineändringar återtar den tidigare generationen.

Tänk också på att beroende på typ och antal implementeringar kan varje generation variera i storlek jämfört med den föregående, vilket innebär att den slutliga storleken kan variera mellan olika körningar.

På grund av detta bör du göra skivans storlek minst två eller tre gånger större än den ursprungligen beräknade databasstorleken.

## Komprimeringslägen i full- och slutläge  {#full-and-tail-compaction-modes}

**AEM 6.5 LTS** har **två lägen** för **compaction** -fasen i rensningsprocessen för onlineändringar:

* Läget **Fullständig komprimering** skriver om alla segment och målfiler i hela databasen. Den efterföljande rensningsfasen kan på så sätt ta bort den maximala mängden skräp i databasen. Eftersom fullständig komprimering påverkar hela databasen krävs det mycket systemresurser och tid för att slutföra.
* Läget **slutkomprimering** skriver bara om de senaste segmenten och tjärfilerna i databasen. De senaste segmenten och tjärfilerna är de som har lagts till sedan den senaste gången komprimeringen kördes, antingen helt eller slut. Den efterföljande rensningsfasen kan därför bara ta bort det skräp som finns i den senaste delen av databasen. Eftersom slutkomprimering bara påverkar en del av databasen krävs betydligt mindre systemresurser och tid för att slutföra kompaktionen än fullständig komprimering.

Dessa komprimeringslägen utgör en kompromiss mellan effektivitet och resursförbrukning: även om slutkomprimeringen är mindre effektiv har den mindre inverkan på den normala systemdriften. Fullständig komprimering är däremot mer effektivt men har större inverkan på den normala systemdriften.

AEM 6.5 LTS har en effektiv funktion för borttagning av innehållsdubbletter vid komprimering, vilket ytterligare minskar databasens diskutrymme.


### Så här konfigurerar du kompaktion med hel- och slutdata {#how-to-configure-full-and-tail-compaction}

Standardkonfigurationen kör slutkomprimering på veckodagar och fullständig komprimering på söndagar. Standardkonfigurationen kan ändras med det nya konfigurationsvärdet `full.gc.days` för `RevisionCleanupTask` [underhållsaktiviteten](/help/sites-deploying/revision-cleanup.md#how-to-run-online-revision-cleanup).

När du konfigurerar värdet `full.gc.days` körs fullständig komprimering under de dagar som definieras i värdet och slutkomprimeringen körs under de dagar som inte har definierats i värdet. Om du till exempel konfigurerar fullständig komprimering för att köras på söndag körs svansen på måndag till lördag. Om du till exempel konfigurerar fullständig komprimering för att köras varje veckodag körs inte slutkomprimeringen alls.

Tänk också på följande:

* **Slutlig komprimering** är mindre effektivt och har mindre inverkan på normala systemåtgärder. Den är således avsedd att köras under vardagar.
* **Fullständig komprimering** är mer effektivt men har också större effekt på normala systemåtgärder. Den är således avsedd att användas utanför kontorstid.
* Både svanskomprimering och fullständig komprimering bör schemaläggas att köras under lågbelastningstimmar.

### Felsökning {#troubleshooting}

Tänk på följande när du använder de nya komprimeringslägena:

* Du kan övervaka in-/utdataaktiviteten (I/O), t.ex. I/O-åtgärder, CPU väntar på IO, spara köstorlek. Detta hjälper till att avgöra om systemet håller på att bli I/O-bundet och kräver en uppgradering.
* `RevisionCleanupTaskHealthCheck` anger den övergripande hälsostatusen för onlinerevideringsrensningen.
* Loggmeddelandena innehåller relevant information om komprimeringslägena. När t.ex. onlineredigering av revision startas, visar motsvarande loggmeddelanden komprimeringsläget. I vissa hörnfall återställs systemet till fullständig komprimering när det var schemalagt att köra en slutkomprimering och loggmeddelandena indikerar den här ändringen. Loggexemplen nedan visar komprimeringsläget och ändringen från svans till full komprimering:

```
TarMK GC: running tail compaction
TarMK GC: no base state available, running full compaction instead
```

### Kända begränsningar {#known-limitations}

Ibland kan rensningsprocessen fördröjas om du växlar mellan svansen och det fullständiga komprimeringsläget. Mer exakt kommer databasen att växa efter en fullständig komprimering (den dubbleras i storlek). Det extra utrymmet återtas i den efterföljande svansen när databasen hamnar under den förfyllda komprimeringsstorleken. Parallella körningar av underhållsuppgifter bör också undvikas.

**Du bör ändra storlek på disken minst två eller tre gånger så stor som den ursprungligen uppskattade databasstorleken.**

## Vanliga frågor och svar om rensning av onlineversioner {#online-revision-cleanup-frequently-asked-questions}

### Online Revision Cleanup körs {#running-online-revision-cleanup}

<table style="table-layout:auto">
 <tbody>
  <tr>
   <td><strong>Frågor</strong></td>
   <td><strong>Svar</strong></td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur ofta ska onlinerevision rensas?</strong></td>
   <td>En gång om dagen. Det här är standardkonfigurationen på kontrollpanelen för åtgärder.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur konfigurerar jag starttiden för underhållsaktiviteten för onlinerensning av revision?</strong></td>
   <td>Se avsnittet <a href="/help/sites-deploying/revision-cleanup.md#how-to-run-online-revision-cleanup">Så här kör du onlineändringsrensning</a>. </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Finns det en maxfrekvens som inte får överskridas för onlinerensning av revision?</strong></td>
   <td>Vi rekommenderar att du kör rensning av onlineändringar en gång om dagen, enligt konfigurationen.<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vilka är nyckelindikatorerna för hur ofta onlinerevision ska rensas?</strong></td>
   <td>Det finns ingen anledning att avgöra hur ofta onlinerättning av revision har konfigurerats som en underhållsuppgift och den körs automatiskt varje dag.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Varför tar onlineredigering inte bort utrymme när programmet körs för första gången?</strong></td>
   <td>Online Revision Cleanup återkallar gamla versioner efter generationer. En ny generation genereras varje gång som en revision rensas. Endast det innehåll som är minst två generationer gammalt kommer att återvinnas, vilket betyder att det inte finns något att återkräva i en första omgång.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Varför tar inte den första rensningen av onlinerevision bort något utrymme när den körs efter rensningen av offlinerevision?</strong></td>
   <td><p>Offline Revision Cleanup återtar allt utom den senaste generationen jämfört med de senaste två generationerna för Online Revision Cleanup. Om det finns en ny databas kommer rensning av onlineändringar inte att frigöra utrymme första gången efter rensning av offlineredigering, eftersom det inte finns någon generation som räcker för att återvinnas.</p> <p>Läs även avsnittet"Running Online Revision Cleanup after Offline Revision Cleanup" i <a href="/help/sites-deploying/revision-cleanup.md#how-to-run-online-revision-cleanup">det här kapitlet</a>.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Skulle författare och publicering vanligtvis ha olika fönster för rensning av onlineversioner?</strong></td>
   <td>Detta beror på kontorstid och kundens trafikmönster online. Underhållsfönstren bör konfigureras utanför de huvudsakliga produktionstiderna för att ge bästa möjliga rensningseffekt. För flera AEM Publish-instanser (tarMK-servergrupp) bör underhållsfönstren för onlinerevision Cleanup mellanlagras.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vilka faktorer avgör hur länge onlineversionen ska rensas?</strong></td>
   <td>Faktorer är:<br />
    <ul>
     <li>Databasstorlek</li>
     <li>Läs in på systemet (begäranden per minut, specifikt skrivåtgärder)</li>
     <li>Aktivitetsmönster (läses eller skrivs)</li>
     <li>Maskinvaruspecifikationer (CPU prestanda, minne, IOPS)</li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Kan skribenter fortfarande arbeta medan onlineversionen rensas?</strong></td>
   <td>Ja, onlineredigering kan hantera samtidiga skrivningar. Online Revision Cleanup fungerar dock snabbare och effektivare utan samtidiga write-transaktioner. Adobe rekommenderar att underhållsaktiviteten för Online Revision Cleanup schemaläggs till en relativt lugn tid utan någon större trafik.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vilka är minimikraven för diskutrymme och heap-minne när du kör Online Revision Cleanup?</strong></td>
   <td><p>Diskutrymmet övervakas kontinuerligt under rensning av onlineändringar. Om det tillgängliga diskutrymmet skulle sjunka under ett kritiskt värde avbryts processen. Det kritiska värdet är 25 % av databasens aktuella diskutrymme och kan inte konfigureras.</p> <p><strong>Adobe rekommenderar att du ändrar storlek på disken minst två eller tre gånger så stor som den ursprungligen uppskattade databasstorleken.</strong></p> <p>Ledigt stackutrymme övervakas kontinuerligt under rensningsprocessen. Om det lediga stackutrymmet skulle sjunka under ett kritiskt värde avbryts processen. Det kritiska värdet konfigureras via org.apache.jackrabbit.oak.segment.SegmentNodeStoreService#MEMORY_THRESHOLD. Standardvärdet är 15 %.</p> <p>Rekommendationer för minsta storlek för komprimeringsstackar separeras inte från AEM rekommendationer för minnesstorlek. Vanligtvis: <strong>Om en AEM-instans är tillräckligt stor för att kunna hantera användningsfallen och den förväntade nyttolasten på den, får rensningsprocessen tillräckligt med minne.</strong></p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vilken är den förväntade prestandapåverkan när onlinerevision rensas?</strong></td>
   <td>Rensa onlineändringar är en bakgrundsprocess som läser från och skriver till databasen samtidigt som vanliga systemåtgärder. Det kan i synnerhet behöva få exklusiv åtkomst till databasen under en kort tidsperiod, vilket förhindrar att andra trådar skriver till databasen.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur länge förväntas onlinerevision Cleanup köras?</strong></td>
   <td>Det bör inte ta längre tid än två timmar att köra enligt de senaste prestandatesterna som Adobe utfört internt.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad ska du göra om rensning av onlineversioner tar längre tid?</strong></td>
   <td>
    <ul>
     <li>Kontrollera att den körs dagligen.<br /> </li>
     <li>Se till att den körs under minimala databasaktiviteter genom att konfigurera underhållsfönstren i Operations Dashboard därefter.</li>
     <li>Skala upp systemresurser (CPU, Memory, I/O).</li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad händer om onlineredigering av versioner överskrider konfigurerade underhålls-Windows?</strong></td>
   <td>Se till att andra underhållsåtgärder inte försenar körningen. Detta kan vara fallet om fler underhållsåtgärder än när onlinerevision rensas utförs inom samma underhållsfönster. Underhållsåtgärder körs sekventiellt utan någon konfigurerbar order.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Varför ignoreras skräpinsamlingen för revideringar?</strong></td>
   <td><p>Revision Cleanup förlitar sig på en uppskattningsfas för att avgöra om det finns tillräckligt med skräp att rengöra. Uppskattaren jämför den aktuella storleken med storleken på databasen efter den senaste komprimeringen. Om storleken överskrider den konfigurerade deltavärdet körs rensning. Storleksdeltavärdet är inställt på 1 GB. Detta innebär att om databasstorleken inte har ökat med 1 GB sedan den senaste rensningen, hoppas den nya versionen över. </p> <p>Nedan anges de relevanta loggposterna för uppskattningsfasen:</p>
    <ul>
     <li>Granska GC körs: <em>Storleksskillnad är N% eller N/N (N/N byte), så komprimering körs</em></li>
     <li>GC för revision kör <strong>inte</strong>: <em>Storleksförändringen är N% eller N/N (N/N byte), så hoppa över komprimering för tillfället</em></li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Går det att avbryta den automatiska komprimeringen på ett säkert sätt om prestandan är för hög?</strong></td>
   <td>Ja. Från och med AEM 6.3 kan det stoppas på ett säkert sätt via underhållsuppgiftsfönstret på kontrollpanelen för åtgärder eller via JMX.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Om AEM-instansen stängs av under en schemalagd rensningsåtgärd avbryts processen på ett säkert sätt eller blockeras avstängningen tills komprimeringen är klar?</strong></td>
   <td>Revision Cleanup avbryts och databasen stängs av på ett säkert sätt.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad händer när systemet kraschar vid rensning av onlinerevision?</strong></td>
   <td>Det finns ingen risk för att data skadas i sådana fall. Skräprester rensas bort i en efterföljande körning.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad blir det för effekt om onlineversionen inte rensas?</strong></td>
   <td>Prestandaförsämring över tid.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vilka revisioner samlas in?</strong></td>
   <td>Som standard samlar rensningen av onlineändringar bara in revideringar som är minst 24 timmar gamla.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad händer om det finns för mycket störningar från samtidiga skrivningar till databasen?</strong></td>
   <td><p>Om det finns samtidiga skrivningar i systemet kan rensning av onlineversioner kräva exklusiv skrivåtkomst för att ändringarna ska kunna verkställas i slutet av en kompaktionscykel. Systemet försätts i <strong>forceCompact-läge</strong>, vilket beskrivs mer ingående i <a href="https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html" target="_blank">Oak-dokumentationen</a>. Under force compact hämtas ett exklusivt skrivlås för att slutligen genomföra ändringarna utan att några samtidiga skrivningar stör. För att begränsa effekten på svarstiderna kan ett timeout-värde definieras. Detta värde är inställt på en minut som standard, vilket innebär att om force compact inte slutförs inom en minut avbryts komprimeringsprocessen till förmån för samtidiga implementeringar.</p> <p>Kraftkomprimeringens varaktighet beror på följande faktorer:</p>
    <ul>
     <li>maskinvara: IOPS. Varaktigheten minskar med fler IOPS.</li>
     <li>segmentbutikens storlek: längden ökar med segmentbutikens storlek.</li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><p><strong>Hur körs onlinerevision Cleanup på en standby-instans?</strong></p> </td>
   <td><p>I ett kallt vänteläge måste endast den primära instansen konfigureras för att köra onlinerevisionsrensning. I väntelägesinstansen behöver rensning av onlineändringar inte schemaläggas specifikt.</p> <p>Motsvarande åtgärd i en standby-instans är den automatiska rensningen - det motsvarar rensningsfasen i onlineändringsrensningen. Den automatiska rensningen körs i väntelägesinstansen efter att Online Revision Cleanup på den primära instansen har körts.</p> <p>Uppskattnings- och komprimeringsfaserna körs inte på en standby-instans.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Kan Revision Cleanup frigöra mer diskutrymme än Online Revision Cleanup?</strong></td>
   <td><p>Revision Cleanup offline kan omedelbart ta bort gamla versioner medan rensning av onlineändringar måste ta hänsyn till gamla versioner som fortfarande refereras av programstacken. Den första kan på så sätt ta bort skräp mer aggressivt än den senare, där effekten skrivs av under ett par sopinsamlingscykler.</p> <p>Läs även avsnittet"Running Online Revision Cleanup after Offline Revision Cleanup" i <a href="/help/sites-deploying/revision-cleanup.md#how-to-run-online-revision-cleanup">det här kapitlet</a>.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td>Har du något att tänka på när det gäller minnesmappade filåtgärder?</td>
   <td>
    <ul>
     <li><strong>I Windows-miljöer</strong> tvingas regelbunden filåtkomst alltid till, så minnesmappad åtkomst används inte. Som en allmän rekommendation bör allt tillgängligt RAM-minne allokeras till heap-objektet och segmentets cachestorlek ökas. Du ökar segmentCache genom att lägga till alternativet segmentCache.size i org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config (till exempel segmentCache.size=20480). Kom ihåg att utelämna lite RAM-minne för operativsystemet och andra processer.</li>
     <li><strong>I icke-Windows-miljöer</strong> kan du öka storleken på det fysiska minnet för att förbättra minnesmappningen för databasen.</li>
    </ul> </td>
   <td>
    <ul>
     <li> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

### Övervaka rensning av onlineändringar {#monitoring-online-revision-cleanup}

<table style="table-layout:auto">
 <tbody>
  <tr>
   <td><strong>Vad måste övervakas under rensning av onlineversioner?</strong></td>
   <td>
    <ul>
     <li>Diskutrymmet bör övervakas när rensning av onlinerevision är aktiverat. Rensningen körs inte eller avslutas i förväg när det inte finns tillräckligt med diskutrymme.</li>
     <li>Kontrollera loggarna för att se när onlineversionen har rensats. Det får inte ta längre tid än 2 timmar.</li>
     <li>Antal kontrollpunkter. Om det finns fler än tre kontrollpunkter när komprimeringen körs bör du rensa upp kontrollpunkterna.</li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur kontrollerar jag om rensningen av onlineversionen har slutförts utan problem?</strong></td>
   <td><p>Du kan kontrollera om rensningen av onlineändringar har slutförts genom att kontrollera loggarna.</p> <p><code>TarMK GC #{}: compaction completed in {} ({} ms), after {} cycles</code> innebär till exempel att komprimeringssteget har slutförts utan fel om det inte föregås av meddelandet <code>TarMK GC #{}: compaction gave up compacting concurrent commits after {} cycles</code>, vilket innebär att det fanns för mycket samtidig inläsning.</p> <p>Motsvarande meddelande <code>TarMK GC #{}: cleanup completed in {} ({} ms</code> om att rensningssteget har slutförts.</p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><strong>Var hittar vi statistik över de senaste rensningarna av onlineversioner?</strong></td>
   <td><p>Status, förlopp och statistik visas via JMX (<code>SegmentRevisionGarbageCollection</code> MBean). Mer information om <code>SegmentRevisionGarbageCollection</code> MBean finns i <a href="https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html#monitoring-via-jmx" target="_blank">följande stycke</a>.</p> <p>Förloppet kan spåras via attributet <code>EstimatedRevisionGCCompletion</code> i <code>SegmentRevisionGarbageCollection MBean.</code></p> <p>Du kan hämta en referens för MBean med <code>ObjectName org.apache.jackrabbit.oak:name="Segment node store revision garbage collection",type="SegmentRevisionGarbageCollection"</code>.</p> <p>Statistiken är endast tillgänglig sedan den senaste systemstarten. <a href="/help/sites-administering/operations-dashboard.md#monitoring-with-external-services" target="_blank">Externa övervakningsverktyg kan användas för att hålla data utanför AEM drifttid</a>.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad är relevanta loggposter?</strong></td>
   <td>
    <ul>
     <li>Rensa onlineändringar har startats/stoppats
      <ul>
       <li>Rensa onlineändringar består av tre faser: uppskattning, komprimering och rensning. Uppskattningen kan tvinga komprimering och rensning att hoppa över om databasen inte innehåller tillräckligt mycket skräp. I den senaste versionen av AEM markerar meddelandet <code>TarMK GC #{}: estimation started</code> början av uppskattningen, <code>TarMK GC #{}: compaction started, strategy={}</code> markerar början på komprimeringen och T<code>arMK GC #{}: cleanup started. Current repository size is {} ({} bytes</code> markerar början på rensningen.</li>
      </ul> </li>
     <li>Det diskutrymme som frigjorts vid rensning av revision
      <ul>
       <li>Utrymmet återvinns endast när rensningsfasen är slutförd. Slutförandet av rensningsfasen markeras av loggmeddelandet "T<code>arMK GC #{}: cleanup completed in {} ({} ms</code>". Storleken efter rensning är {} ({} byte) och utrymmet återanvänds {} ({} byte). Vikt/djup för komprimeringsmappning är {}/{} ({} byte/{})."</li>
      </ul> </li>
     <li>Ett fel uppstod under rensningen av revisionen
      <ul>
       <li>Det finns många feltillstånd, som alla markeras med WARN- eller ERROR-loggmeddelanden som börjar med TjärMK GC.</li>
      </ul> </li>
    </ul> <p>Se även avsnittet <a href="/help/sites-deploying/revision-cleanup.md#troubleshooting-based-on-error-messages">Felsökning baserad på felmeddelanden</a> nedan.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur mycket utrymme har tagits i anspråk efter att onlinerevision har rensats?</strong></td>
   <td>Det finns ett meddelande i loggen i slutet av rensningscykeln: <code>TarMK GC #3: cleanup completed</code> som innehåller storleken på databasen och mängden återvunnet skräp.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur kontrollerar jag databasens integritet efter att onlineversionen har rensats?</strong></td>
   <td><p>Det behövs ingen integritetskontroll för databasen efter rensning av onlinerevision. </p> <p>Du kan dock utföra följande åtgärder för att kontrollera databasens status efter rensning:</p>
    <ul>
     <li>En <a href="/help/sites-deploying/consistency-check.md" target="_blank">traversal-kontroll för databasen</a></li>
     <li>Använd ekkörningsverktyget när rensningsprocessen har slutförts för att kontrollera om det finns några inkonsekvenser. Mer information om hur du gör detta finns i <a href="https://github.com/apache/jackrabbit-oak/blob/trunk/oak-doc/src/site/markdown/nodestore/segment/overview.md#check" target="_blank">Apache-dokumentationen.</a> Du behöver inte stänga av AEM för att köra verktyget.</li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Hur du identifierar om rensning av onlinerevision har misslyckats och vilka steg ska återställas?</strong></td>
   <td>Feltillstånd markeras med WARN- eller FELloggmeddelanden som börjar med TjärMK GC. Se även avsnittet <a href="/help/sites-deploying/revision-cleanup.md#troubleshooting-based-on-error-messages">Felsökning baserad på felmeddelanden</a> nedan.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vilken information visas i hälsokontrollen för Revision Cleanup? Hur och när bidrar de till den färgkodade statusnivån? </strong></td>
   <td><p>Hälsokontrollen för rensning av revision ingår i <a href="/help/sites-administering/operations-dashboard.md#health-reports" target="_blank">instrumentpanelen för åtgärder</a>.<br /> </p> <p>Statusen är <strong>GREEN</strong> om den senaste körningen av underhållsaktiviteten för rensning av onlinerevision har slutförts.</p> <p>Det är <strong>YELLOW</strong> om underhållsaktiviteten för rensning av onlinerevision avbröts en gång.<br /> </p> <p>Det är <strong>RED</strong> om underhållsaktiviteten Rensa online-revision avbröts tre gånger i rad. <strong>I det här fallet krävs manuell interaktion</strong> eller så misslyckas rensningen av onlineändringar troligtvis igen. Mer information finns i avsnittet <a href="/help/sites-deploying/revision-cleanup.md#troubleshooting-online-revision-cleanup">Felsökning</a> nedan.<br /> </p> <p>Dessutom återställs hälsokontrollsstatusen efter en omstart av systemet. En instans som har startats om nyligen visar en grön status i hälsokontrollen för Revision Cleanup.  <a href="/help/sites-administering/operations-dashboard.md#monitoring-with-external-services" target="_blank">Externa övervakningsverktyg kan användas för att hålla data utanför AEM drifttid</a>.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><p><strong>Hur övervakar jag automatisk rensning på en standby-instans?</strong></p> </td>
   <td><p>Status, förlopp och statistik visas via JMX med <code>SegmentRevisionGarbageCollection</code> MBean. Se även följande <a href="https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html#monitoring-via-jmx" target="_blank">Oak-dokumentation</a>. </p> <p>Du kan hämta en referens för MBean genom att använda <code>ObjectName org.apache.jackrabbit.oak:name="Segment node store revision garbage collection",type="SegmentRevisionGarbageCollection"</code>.</p> <p>Statistiken är endast tillgänglig sedan den senaste systemstarten.  <a href="/help/sites-administering/operations-dashboard.md#monitoring-with-external-services" target="_blank">Externa övervakningsverktyg kan användas för att hålla data utanför AEM drifttid</a>.</p> <p>Loggfilerna kan också användas för att kontrollera status, förlopp och statistik för den automatiska rensningen.</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><p><strong>Vad måste övervakas under automatisk rensning på en standby-instans?</strong></p> </td>
   <td>
    <ul>
     <li>Diskutrymmet bör övervakas när den automatiska rensningen körs.</li>
     <li>Slutförandetid (via loggarna) för att säkerställa att 2 timmar inte överskrids.</li>
     <li>Storlek på segmentlager efter att automatisk rensning har körts. Storleken på segmentbutiken i standby-instansen bör vara ungefär densamma som storleken på den primära instansen.</li>
    </ul> </td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Felsökning av rensning av onlineversioner {#troubleshooting-online-revision-cleanup}

<table style="table-layout:auto">
 <tbody>
  <tr>
   <td><strong>Vad är det värsta som kan hända om du inte kör Online Revision Cleanup?</strong></td>
   <td>AEM-instansen har slut på diskutrymme vilket orsakar produktionsavbrott.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Är det problem med hög användartrafik att köra rensning av onlinerevision på en publiceringsinstans?</strong></td>
   <td>Hög användartrafik påverkar om komprimeringsfasen kan slutföras eller inte.<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Enligt hälsokontrollen och loggposterna har rensning av onlineändringar inte slutförts tre gånger i rad. Vad krävs för att onlineversionen ska kunna rensas korrekt?</strong></td>
   <td>Du kan utföra flera steg för att hitta och åtgärda problemet:<br />
    <ul>
     <li>Kontrollera först loggposterna <br /> </li>
     <li>Beroende på informationen i loggarna ska du vidta lämpliga åtgärder:
      <ul>
       <li>Om loggarna visar fem missade kompakta cykler och en timeout i <code>forceCompact</code>-cykeln, schemalägger du underhållsperioden till en tyst tidpunkt när mängden databasskrivningar är låg. Du kan kontrollera databasskrivningar i databasmätningsverktyget på <em>https://serveraddress:serverport/libs/granite/operations/content/monitoring/page.html</em></li>
       <li>Om rensningen stoppas i slutet av underhållsperioden kontrollerar du att konfigurationen för underhållsperioden i underhållsuppgifter-användargränssnittet är tillräckligt stor</li>
       <li>Om det inte finns tillräckligt med stackminne kontrollerar du att instansen har tillräckligt med minne.</li>
       <li>Om det blir en sen reaktion kan segmentlagret växa för mycket för att onlineversionen ska kunna rensas även i ett längre underhållsfönster. Om t.ex. ingen lyckad rensning av onlinerevision slutfördes under den senaste veckan rekommenderar vi att du planerar ett offlineserv och kör en rensning av offlinerevision för att få segmentlagret att bli hanterbart igen.</li>
      </ul> </li>
    </ul> </td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad ska göras när hälsokontrollen är aktiverad?</strong></td>
   <td>Se föregående punkt.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad händer om rensning av onlineversioner tar slut under det schemalagda underhållet?</strong></td>
   <td>Rensa onlineändringar har avbrutits och överbliven tas bort. Den startar igen nästa gång underhållsfönstret är schemalagt.</td>
   <td> </td>
  </tr>
  <tr>
   <td><strong>Vad gör att <code>SegmentNotFoundException</code> instanser loggas i <code>error.log</code> och hur kan jag återställa?</strong></td>
   <td><p>En <code>SegmentNotFoundException</code> loggas av tarMK när den försöker komma åt en lagringsenhet (ett segment) som den inte kan hitta. Det finns tre scenarier som kan orsaka problemet:</p>
    <ol>
     <li>Ett program som kringgår de rekommenderade åtkomstmekanismerna (som Sling och JCR API) och använder ett API/SPI på lägre nivå för att komma åt databasen och sedan överskrider kvarhållningstiden för ett segment. Det innebär att den behåller en referens till en enhet som är längre än den kvarhållningstid som tillåts av onlinerevideringsrensningen (24 timmar som standard). Det här fallet är övergående och leder inte till att data skadas. För att återställa systemet bör ekkörningsverktyget användas för att bekräfta undantagets transienta karaktär (ekskörningskontrollen bör inte rapportera några fel). För att göra detta måste instansen tas offline och sedan startas om.</li>
     <li>En extern händelse orsakade att data på disken skadades. Det kan vara ett diskfel, slut på diskutrymme eller en oavsiktlig ändring av de datafiler som krävs. I det här fallet måste instansen tas offline och repareras med körkontrollen. Mer information om hur du utför körningskontrollen finns i följande <a href="https://github.com/apache/jackrabbit-oak/blob/trunk/oak-doc/src/site/markdown/nodestore/segment/overview.md#check" target="_blank">Apache-dokumentation</a>.</li>
     <li>Åtgärda alla andra händelser via <a href="https://experienceleague.adobe.com/?support-solution=General&amp;support-tab=home#support" target="_blank">Adobe kundtjänst</a>.</li>
    </ol> </td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Felsökning baserad på felmeddelanden {#troubleshooting-based-on-error-messages}

error.log är utförlig om det finns incidenter under rensningen av onlineversioner. Följande matris syftar till att förklara de vanligaste budskapen och ge möjliga lösningar:

<!---| **Phase** |**Log Messages** |**Explanation** |**Next Steps** |
|---|---|---|---|
|   |  |  |  |
| Estimation |TarMK GC #2: estimation skipped because compaction is paused |The estimation phase is skipped when compaction is disabled on the system by configuration. |Enable Online Revision Cleanup. |
|   |TarMK GC #2: estimation interrupted: ${REASON}. Skipping compaction. |The estimation phase terminated prematurely. Some examples of events that could interrupt the estimation phase: not enough memory or disk space on the host system. |Depends on the given reason. |
| Compaction |TarMK GC #2: compaction paused |As long as the compaction phase is paused by configuration, neither the estimation phase nor the compaction phase will be executed. |Enable online revision cleanup. |
|   |TarMK GC #2: compaction cancelled: ${REASON}. |The compaction phase terminated prematurely. Some examples of events that could interrupt the compaction phase: not enough memory or disk space on the host system. Moreover, compaction can also be cancelled by shutting down the system or by explicitly cancelling it via administrative interfaces such as the Maintenance Window within the Operations Dashobard. |Depends on the given reason. |
|   |TarMK GC #2: compaction failed in 32.902 min (1974140 ms), after 5 cycles |This message does not mean that there was an unrecoverable error, but only that compaction was terminated after a certain amount of attempts. Also, read the [following paragraph](https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html#how-does-compaction-works-with-concurrent-writes). |Read the following [Oak documentation](https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html#how-does-compaction-works-with-concurrent-writes), and the last question of the [Running Online Revision Cleanup](/help/sites-deploying/revision-cleanup.md#running-online-revision-cleanup) section. |
| Cleanup |TarMK GC #2: cleanup interrupted |Cleanup has been cancelled by shutting down the repository. No impact on consistency is expected. Also, disk space is most likely not reclaimed to full extent. It will be reclaimed during next revision cleanup cycle. |Investigate why repository has been shut down and going forward try to avoid shutting down the repository during maintenance windows. |-->

<table style="table-layout:auto">
 <tbody>
  <tr>
    <th>Fas</th>
    <th>Loggmeddelanden</th>
    <th>Förklaring</th>
    <th>Nästa steg</th>
  </tr>  
  <tr>
    <td>Uppskattning</td>
    <td>TjäraMK GC #2: Uppskattningen hoppades över eftersom komprimeringen pausas.</td>
    <td>Uppskattningsfasen hoppas över när komprimering är inaktiverat i systemet efter konfiguration.</td>
    <td>Aktivera rensning av onlineversioner.</td>
  </td>
  </tr>
  <tr>
    <td>Ej tillämpligt</td>
    <td>TjärMK GC #2: Uppskattningen avbröts: ${REASON}. Hoppar över komprimering.</td>
    <td>Uppskattningsfasen avslutades i förtid. Några exempel på händelser som kan avbryta beräkningsfasen: det finns inte tillräckligt med minne eller diskutrymme på värdsystemet.</td>
    <td>Beroende på den angivna orsaken.</td>
  </td>
  </tr>
  <tr>
    <td>Komprimering</td>
    <td>TjäraMK GC #2: komprimering pausad.</td>
    <td>Så länge komprimeringsfasen pausas av konfigurationen körs varken beräkningsfasen eller komprimeringsfasen.</td>
    <td>Aktivera rensning av onlineversioner.</td>
  </td>
  </tr>
   <tr>
    <td>Ej tillämpligt</td>
    <td>TjäraMK GC #2: komprimeringen avbröts: ${REASON}.</td>
    <td>Komprimeringsfasen avslutades för tidigt. Några exempel på händelser som kan avbryta komprimeringsfasen: det finns inte tillräckligt med minne eller diskutrymme på värdsystemet. Komprimeringen kan också avbrytas genom att systemet stängs av eller genom att det uttryckligen avbryts via administrativa gränssnitt som underhållspanelen i Operations Dashboard.</td>
    <td>Beroende på den angivna orsaken.</td>
  </td>
  </tr>
  <tr>
    <td>Ej tillämpligt</td>
    <td>TjärMK GC #2: komprimering misslyckades i 32,902 min (1974140 ms), efter 5 cykler.</td>
    <td>Det här meddelandet betyder inte att det fanns ett oåterkalleligt fel, men bara den komprimeringen avslutades efter några försök. Läs även det <a href="https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html#how-does-compaction-works-with-concurrent-writes">följande stycket.</a></td>
    <td>Läs följande <a href="https://jackrabbit.apache.org/oak/docs/nodestore/segment/overview.html#how-does-compaction-works-with-concurrent-writes">Oak-dokumentation</a> och den senaste frågan i rensningsavsnittet för onlineändringar.</a></td>
  </td>
  </tr>
  <tr>
    <td>Rensa</td>
    <td>TjärMK GC #2: rensningen avbröts.</td>
    <td>Rensningen avbröts genom att databasen stängdes av. Ingen påverkan på konsekvensen förväntas. Dessutom kommer diskutrymmet troligtvis inte att återvinnas i full utsträckning. Den kommer att återvinnas under nästa revisionsrensningscykel.</td>
    <td>Undersök varför databasen har stängts av och gå framåt för att undvika att stänga av databasen under underhållsfönstren.</td>
  </td>
  </tr>
  </tbody>
</table>

## Så här kör du borttagning av offlinerevision {#how-to-run-offline-revision-cleanup}

>[!CAUTION]
>
>Använd en Oak-rundeverktygsrelease med ett versionsnummer (både större och mindre) som överensstämmer med Oak grundversion av din AEM-installation. Om din AEM-instans till exempel har Oak Core version 1.2.x bör du använda den senaste versionen av Oak-verktyget 1.2.x.

Adobe har ett verktyg som heter **Oak-run** som utför revisionsrensning. Den kan laddas ned här:

[https://repo1.maven.org/maven2/org/apache/jackrabbit/oak-run/](https://repo1.maven.org/maven2/org/apache/jackrabbit/oak-run/)

Verktyget är en körbar burk som kan köras manuellt för att komprimera databasen. Processen kallas för rensning av offlineändringar eftersom databasen måste stängas av för att verktyget ska kunna köras korrekt. Se till att planera rensningen i enlighet med ditt underhållsfönster.

Tips om hur du kan förbättra rensningsprocessens prestanda finns i [Förbättra prestanda för rensning av offlineredigering](/help/sites-deploying/revision-cleanup.md#increasing-the-performance-of-offline-revision-cleanup).

>[!NOTE]
>
>Du kan även rensa gamla kontrollpunkter innan underhållet utförs (steg 2 och 3 i proceduren nedan). Detta rekommenderas endast för instanser som har fler än 100 kontrollpunkter.

1. Kontrollera alltid att du har en säkerhetskopia av AEM-instansen.

   Stäng av AEM.

1. (Valfritt) Använd verktyget för att hitta gamla kontrollpunkter:

   ```xml
   java -jar oak-run.jar checkpoints install-folder/crx-quickstart/repository/segmentstore
   ```

1. (Valfritt) Ta sedan bort de orefererade kontrollpunkterna:

   ```xml
   java -jar oak-run.jar checkpoints install-folder/crx-quickstart/repository/segmentstore rm-unreferenced
   ```

1. Kör komprimeringen och vänta tills den är klar:

   ```xml
   java -jar -Dsun.arch.data.model=32 oak-run.jar compact install-folder/crx-quickstart/repository/segmentstore
   ```

### Förbättra prestanda för rensning av offlinerevision {#increasing-the-performance-of-offline-revision-cleanup}

Verktyget för ekakning innehåller flera funktioner som syftar till att öka prestandan för revisionsrensningen och minimera underhållsfönstret så mycket som möjligt.

Listan innehåller flera kommandoradsparametrar, enligt beskrivningen nedan:

* **-map.** Du kan ange det här som sant eller falskt. Om värdet är true används minnesmappad åtkomst. Om värdet är false används filåtkomst. Om inget anges används minnesmappad åtkomst på 64-bitarssystem och filåtkomst används på 32-bitarssystem. I Windows används alltid vanlig filåtkomst och det här alternativet ignoreras. **Den här parametern har ersatt parametern -Dtar.memoryMMapped.**

* **-Dupdate.limit**. Definierar tröskelvärdet för tömning av en temporär transaktion till disk. Standardvärdet är 10000.

* **-Dcompress-interval**. Antal komprimeringsmappningsposter som ska behållas tills den aktuella kartan komprimeras. Standardvärdet är 100000. Om det finns tillräckligt med stackminne bör du öka det här värdet till ett ännu högre värde för snabbare dataflöde. **Den här parametern har tagits bort i Oak version 1.6 och har ingen effekt.**

* **-Dcompaction-progress-log**. Antalet komprimerade noder som loggas. Standardvärdet är 150000, vilket innebär att de första 150000 komprimerade noderna loggas under åtgärden. Använd den här med nästa parameter som beskrivs nedan.

* **-Dtar.PersistCompactionMap.** Ställ in den här parametern på true om du vill använda diskutrymme i stället för heap-minne för att bevara komprimeringskartan. Kräver ekkörningsverktyget **version 1.4** och senare. Mer information finns i fråga 3 i avsnittet [Vanliga frågor och svar ](/help/sites-deploying/revision-cleanup.md#offline-revision-cleanup-frequently-asked-questions) om offlineredigering. **Den här parametern har tagits bort i Oak version 1.6 och har ingen effekt.**

* **—force.** Tvinga komprimering och ignorera en version som inte matchar segmentlagret.

>[!CAUTION]
>
>Om du använder parametern `--force` uppgraderas segmentbutiken till den senaste versionen, vilket inte är kompatibelt med äldre Oak-versioner. Tänk också på att ingen nedgradering är möjlig. I allmänhet bör du använda dessa parametrar med försiktighet och endast om du har kunskap om hur du använder dem.

Ett exempel på de parametrar som används:

```xml
java -Dupdate.limit=10000 -Dcompaction-progress-log=150000 -Dlogback.configurationFile=logback.xml -Xmx8g -jar oak-run-*.jar checkpoints <repository>
```

### Ytterligare metoder för att utlösa rensning av revision {#additional-methods-of-triggering-revision-cleanup}

Förutom metoderna ovan kan du även aktivera funktionen för revisionsrensning med hjälp av JMX-konsolen på följande sätt:

1. Öppna JMX-konsolen genom att gå till [http://localhost:4502/system/console/jmx](http://localhost:4502/system/console/jmx)
1. Klicka på **RevisionGarbageCollection** MBean.
1. I nästa fönster klickar du på **startRevisionGC()** och sedan på **Invoke** för att starta jobbet Revision Skräpsamling.

### Vanliga frågor och svar om rensning av offlinerevision {#offline-revision-cleanup-frequently-asked-questions}

<table style="table-layout:auto">
 <tbody>
  <tr>
   <td><strong>Vilka faktorer avgör varaktigheten för rensningen av offlineredigering?</strong></td>
   <td><p>Databasstorleken och antalet revisioner som måste rensas avgör hur lång rensningen ska vara.</p> </td>
  </tr>
  <tr>
   <td><strong>Vad är skillnaden mellan en revision och en sidversion?</strong></td>
   <td>
    <ul>
     <li><strong>Oak-revision:</strong> Oak organiserar allt innehåll i en stor trädhierarki som består av noder och egenskaper. Varje ögonblicksbild eller revidering av det här innehållsträdet kan inte ändras och ändringar av trädet uttrycks som en sekvens av nya revideringar. Vanligtvis utlöser varje innehållsändring en ny revision. Se även <a href="https://jackrabbit.apache.org/dev/ngp.html" target="_blank"> Följ länk </a>.</li>
     <li><strong>Sidversion:</strong> En ögonblicksbild av en sida skapas vid en viss tidpunkt. Vanligtvis skapas en ny version när en sida aktiveras. Mer information finns i <a href="/help/sites-authoring/working-with-page-versions.md" target="_blank">Arbeta med sidversioner</a>.</li>
    </ul> </td>
  </tr>
  <tr>
   <td><strong>Hur kan vi snabba upp rensningen av offlinerevision om den inte slutförs inom 8 timmar?</strong></td>
   <td>Om revideringsaktiviteten inte slutförs inom 8 timmar och <a href="/help/sites-administering/operations-dashboard.md#diagnosis-tools" target="_blank">tråddumparna</a> visar att den huvudsakliga hotspot-positionen är <code>InMemoryCompactionMap.findEntry</code> använder du följande parameter med körningsverktyget <strong>version 1.4 </strong> eller senare: <code>-Dtar.PersistCompactionMap=true</code>. Parametern <code>-Dtar.PersistCompactionMap</code> har tagits bort i Oak version 1.6.</td>
  </tr>
 </tbody>
</table>
