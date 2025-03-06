---
title: Tålig dag
description: Tough Day-testet simulerar den dagliga belastningen för cirka 1 000 författare i ett värsta scenario där alla åtgärder utförs samtidigt.
topic-tags: testing
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 0360a8eb-d4ff-4225-9be3-fa375b5da971
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1825'
ht-degree: 0%

---

# Tålig dag{#tough-day}

## Vad är Tom Dag 2? {#what-is-tough-day}

&quot;Tough Day 2&quot; är ett program där du kan stresstesta gränserna för din AEM-instans. Den kan köras direkt med testsviten eller konfigureras för att passa dina testbehov. Du kan titta på [den här inspelningen](https://experienceleague.adobe.com/docs/events/experience-manager-gems-recordings/gems2017/aem-toughday2-stress-testing-benchmarking-tool.html) om du vill se en presentation av programmet.

>[!CAUTION]
>
>Dag 2 kräver Java™ 8.

## Köra tuff dag 2 {#how-to-run-tough-day}

Hämta den senaste versionen av Tough Day 2 från [Adobe Repository](https://repo1.maven.org/maven2/com/adobe/qe/toughday2/). När du har hämtat programmet kan du köra det direkt genom att ange parametern `host`. I följande exempel körs AEM-instansen lokalt så att värdet `localhost` används:

```xml
java -jar toughday2.jar --host=localhost
```

Standardsviten som körs när parametrarna har lagts till heter `toughday`. Det innehåller följande användningsfall:

* Skapa sidor och live-kopior för dem (inklusive rollouts)
* Hämta hemsida
* Köra frågor i Query Builder
* Skapa resurshierarkier
* Ta bort resurser

Paketet innehåller 15 % skrivåtgärder och 85 % läsåtgärder.

För att köra testerna av sviten installerar Tough Day 2 standardinnehållspaketet. Detta kan undvikas genom att ställa in parametern `installsamplecontent` på `false`, men kom ihåg att du också bör ändra standardsökvägarna för de tester som du tänker köra. Om burken körs utan parametrar visas [hjälpinformationen](/help/sites-developing/tough-day.md#getting-help) för Tough Day 2.

Du kan använda programmet enligt följande mönster:

```xml
java -jar toughday2.jar [--help | --help_full | --help_tests | --help_publish]  [<global arguments> | <actions> | --runmode | --publishmode]
```

>[!NOTE]
>
>Grovdag 2 har inget upprensningssteg. Därför bör du köra Tough Day 2 på en klonad mellanlagringsinstans och inte på huvudproduktionsinstansen. Mellanlagringsinstansen bör tas bort efter testerna.
>

### Få hjälp {#getting-help}

Dag 2 erbjuder en mängd hjälpalternativ som du kan komma åt från kommandoraden. Till exempel:

```xml
java -jar toughday2.jar --help_full
```

I tabellen nedan hittar du relevanta hjälpparametrar.

<table>
 <tbody>
  <tr>
   <td><strong>Parameter</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Exempel</strong></td>
  </tr>
  <tr>
   <td>—help</td>
   <td>Skriver ut global information, t.ex. tillgängliga åtgärder, fördefinierade sviter, körningslägen och globala parametrar.</td>
   <td> </td>
  </tr>
  <tr>
   <td>—help_publish</td>
   <td>Skriver ut alla tillgängliga utgivare.</td>
   <td> </td>
  </tr>
  <tr>
   <td>—help_tests</td>
   <td>Skriver ut testklasserna och deras beskrivning.</td>
   <td> </td>
  </tr>
  <tr>
   <td>—help_full</td>
   <td>Skriver ut allt ovanstående, plus tester, utgivare och Suite-komponenter.</td>
   <td> </td>
  </tr>
  <tr>
   <td> —help —runmode/publishmode type=&lt;Mode&gt;</td>
   <td>Visar information om det angivna körnings- eller publiceringsläget.</td>
   <td><p>Java™ -jar toughday2.jar —help —runmode type=konstantload</p> <p>Java™ -jar toughday2.jar —help —publishmode type=interval</p> </td>
  </tr>
  <tr>
   <td>—help —suite=&lt;SuiteName&gt;</td>
   <td>Listar alla tester av en given svit och deras respektive konfigurerbara egenskaper.</td>
   <td><br /> Java™ -jar toughday2.jar —help —suite=get_tests</td>
  </tr>
  <tr>
   <td> —help —tag=&lt;Tag&gt;</td>
   <td><br /> Visar alla objekt som har den angivna taggen.</td>
   <td>Java™ -jar toughday2.jar —help —tag=publish</td>
  </tr>
  <tr>
   <td>—help &lt;TestClass/PublisherClass&gt;</td>
   <td><br /> Visar alla konfigurerbara egenskaper för det angivna testet eller utgivaren.</td>
   <td><p>Java™ -jar toughday2.jar —help UploadPDFTest</p> <p>Java™ -jar toughday2.jar —help CSVPublisher</p> </td>
  </tr>
 </tbody>
</table>

### Globala parametrar {#global-parameters}

Tough Day 2 erbjuder globala parametrar som ställer in eller ändrar miljön för testerna. Bland dessa finns målvärden, portnummer, protokoll, användare och lösenord för instansen och många andra. Till exempel:

```xml
java -jar toughday2.jar --host=host --protocol=https --port=4502 --duration=30m --dryrun=true
```

Du hittar de relevanta parametrarna i listan nedan:

| **Parameter** | **Beskrivning** | **Standardvärde** | **Möjliga värden** |
|---|---|---|---|
| `--installsamplecontent=<Val>` | Installerar eller hoppar över standardinnehållspaketet för Tough Day 2. | true | true eller false |
| `--protocol=<Val>` | Protokollet som används för värden. | http | http eller https |
| `--host=<Val>` | Värdnamnet eller IP-adressen som ska användas som mål. |  |  |
| `--port=<Val>` | Värdens port. | 4502 |  |
| `--user=<Val>` | Användarnamnet för instansen. | admin |  |
| `--password=<Val>` | Lösenord för den angivna användaren. | admin |  |
| `--duration=<Val>` | Provningens varaktighet. Kan uttryckas i **s** sekunder, **m** minuter, **h** timmar och **d** dagar. | 1d |  |
| `--timeout=<Val>` | Hur länge ett test ska köras innan det avbryts och markeras som misslyckat. Uttryckt i sekunder. | 180 |  |
| `--suite=<Val>` | Värdet kan vara en eller en lista (avgränsad med kommatecken) med fördefinierade testsviter. | tughday |  |
| `--configfile=<Val>` | Den målformade dynamiska konfigurationsfilen. |  |  |
| `--contextpath=<Val>` | Instansens kontextsökväg. |  |  |
| `--loglevel=<Val>` | Loggnivån för motorn Tough Day 2. | INFO | ALLA, FELSÖKNING, INFORMATION, VARNING, FEL, ALLVARLIGT, AV |
| `--dryrun=<Val>` | Om true skrivs den resulterande konfigurationen ut och inga tester körs. | false | true eller false |

## Anpassa {#customizing}

Anpassningen kan göras på två sätt: kommandoradsparametrar eller dynamiska konfigurationsfiler. **Konfigurationsfiler används för stora anpassade programsviter och de åsidosätter standardparametrarna för Tough Day 2. Kommandoradsparametrar åsidosätter både konfigurationsfiler och standardparametrar.**

Det enda sättet att spara en testkonfiguration är att kopiera den i yaml-format.

### Lägga till ett nytt test {#adding-a-new-test}

Om du inte vill använda standardsviten `toughday` kan du lägga till ett test som du väljer med parametern `add`. I exemplen nedan visas hur du lägger till `CreateAssetTreeTest`-testet antingen med kommandoradsparametrar eller en gul konfigurationsfil.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --add CreateAssetTreeTest
```

Genom att använda en dynamisk konfigurationsfil:

```xml
globals:
  host : localhost
tests:
  - add : CreateAssetTreeTest
```

### Lägga till flera instanser av samma test  {#adding-multiple-instances-of-the-same-test}

Du kan också lägga till och köra flera instanser av samma test, men varje instans måste ha ett unikt namn. I exemplen nedan visas hur du lägger till två instanser av samma test med antingen kommandoradsparametrar eller en gul konfigurationsfil.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --add CreateAssetTreeTest name=FirstAssetTree --add CreateAssetTreeTest name=SecondAssetTree
```

Genom att använda en dynamisk konfigurationsfil:

```xml
globals:
  host : localhost
tests:
  - add : CreateAssetTreeTest
    properties:
      name : FirstAssetTree
  - add : CreateAssetTreeTest
    properties:
      name : SecondAssetTree
```

### Ändra testegenskaper {#changing-the-test-properties}

Om du behöver ändra en eller flera av testegenskaperna kan du lägga till den egenskapen på kommandoraden eller i yaml-konfigurationsfilen. Om du vill visa alla tillgängliga testegenskaper lägger du till parametern `--help <TestClass/PublisherClass>` på kommandoraden, till exempel:

```xml
java -jar toughday2.jar --help CreatePageTreeTest
```

Kom ihåg att standardparametrarna Tough Day 2 skrivs över av standardkonfigurationsfilerna och kommandoradsparametrarna åsidosätter både konfigurationsfilerna och standardvärdena.

I exemplen nedan visas hur du ändrar egenskapen `template` för testet `CreatePageTreeTest` antingen med kommandoradsparametrar eller en gul konfigurationsfil.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --add CreatePageTreeTest template=/conf/toughday-templates/settings/wcm/templates/toughday-template
```

Genom att använda en dynamisk konfigurationsfil:

```xml
globals:
  host : localhost
tests:
  - add : CreatePageTreeTest
    properties:
      template : /conf/toughday-templates/settings/wcm/templates/toughday-template
```

### Arbeta med fördefinierade testsviter {#working-with-predefined-test-suites}

I exemplen nedan visas hur du lägger till ett test i en fördefinierad svit och hur du konfigurerar om och exkluderar ett befintligt test från en fördefinierad svit.

Du kan lägga till ett nytt test i en fördefinierad svit med parametern `add` och ange den fördefinierade målsviten.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --suite=toughday --add CreatePageTreeTest
```

Genom att använda en dynamisk konfigurationsfil:

```xml
globals:
  host : localhost
  suite : toughday
tests:
  - add : CreatePageTreeTest
```

Befintliga tester i en given programsvit kan även konfigureras om med parametern `config`* *. Ange också Suite-namnet och testets faktiska namn (inte testklassens namn). Du hittar testnamnet i egenskapen `name` i testklassen. Mer information om hur du söker efter testegenskaper finns i avsnittet [Ändra testegenskaper](/help/sites-developing/tough-day.md#changing-the-test-properties).

I exemplet nedan ändras standardresurstiteln för `CreatePageTreeTest` (med namnet `UploadAsset`) till NewAsset.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --suite=toughday --config UploadAsset title=NewAsset
```

Genom att använda en dynamisk konfigurationsfil:

```xml
globals:
  host : localhost
  suite : toughday
tests:
  - config : UploadAsset
    properties :
      title : NewAsset
```

Du kan också ta bort test från fördefinierade sviter eller utgivare från standardkonfigurationen med hjälp av parametern `exclude`. Ange också namnet på sviten och det faktiska namnet på testet (inte namnet på Test C `lass`). Du kan hitta testnamnet i egenskapen `name` i testklassen. I exemplet nedan tas testet `CreatePageTreeTest` (med namnet `UploadAsset`) bort från toughday-sviten.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --suite=toughday --exclude UploadAsset
```

Genom att använda en dynamisk konfigurationsfil:

```xml
globals:
  host : localhost
  suite : toughday
tests:
  - exclude : UploadAsset
```

### Körningslägen {#run-modes}

Dag 2 kan köras i något av följande lägen: **normal** och **konstant belastning**.

Körningsläget **normal** har två parametrar:

* `concurrency` - samtidighet representerar antalet trådar som Tough Day 2 skapar för testkörning. På dessa trådar kommer tester att utföras tills antingen varaktigheten är slut eller tills det inte finns fler tester att köra.

* `waittime` - väntetiden mellan två på varandra följande testkörningar i samma tråd. Värdet måste anges i millisekunder.

I exemplet nedan visas hur du lägger till parametrar med kommandoraden:

```xml
java -jar toughday2.jar --host=localhost --add CreateAssetTreeTest --runmode=normal concurrency=20
```

eller genom att använda en dynamisk konfigurationsfil:

```xml
runmode:
  type : normal
  waittime : 300
  concurrency : 200
```

Det **konstanta load**-körningsläget skiljer sig från det normala körningsläget genom att generera ett konstant antal startade testkörningar i stället för ett konstant antal trådar. Du kan ställa in inläsningen med parametern för körningsläge med samma namn.

### Testa val {#test-selection}

Testmarkeringsprocessen är densamma för båda körningslägena och den ser ut så här: alla tester har en `weight`-egenskap som avgör sannolikheten för körning i en tråd. Om du till exempel har två tester, ett med en vikt på 5 och det andra med en vikt på 10, är det två gånger mer troligt att det senare utförs än det första.

Dessutom kan tester ha en `count`-egenskap, vilket begränsar antalet körningar till ett visst antal. När detta antal har passerats kommer inga fler testkörningar att utföras. Alla testinstanser som redan körs kommer att slutföra körningen enligt konfigurationen. I följande exempel visas hur du lägger till de här parametrarna antingen på kommandoraden eller med hjälp av en dynamisk konfigurationsfil.

```xml
java -jar toughday2.jar --host=localhost --add CreateAssetTreeTest weight=5 --add CreatePageTreeTest weight=10 count=100 --runmode=normal concurrency=20
```

eller

```xml
- add : CreateAssetTreeTest
    properties :
      name : UploadAsset
      weight : 5
      base : 3
      foldertitle : IAmAFolder
      assettitle : IAmAnAsset
      count : 100
```

>[!NOTE]
>
>På grund av parallella körningar kommer det faktiska antalet testkörningar inte att vara exakt den mängd som konfigurerats i parametern `count`. Förväntade en avvikelse i proportion till antalet pågående trådar (styrs av `concurrency parameter`).

### Torr körning {#dry-run}

En torr körning tolkar alla angivna indata (kommandoradsparametrar eller konfigurationsfiler), sammanfogar dem med standardvärdena och returnerar sedan resultatet. Den utför inte något av testerna.

```xml
java -jar toughday2.jar --host=localhost --suite=toughday --add CreatePageTreeTest --dryrun=true
```

## Utdata {#output}

Tough Day 2 outputs both test metrics and logs. Mer information finns i följande avsnitt.

### Testmått {#test-metrics}

Tuff dag 2 rapporterar för närvarande nio testvärden som du kan utvärdera. Mätvärden med symbolen **&#42;** rapporteras endast efter lyckade körningar:

| **Namn** | **Beskrivning** |
|---|---|
| Tidsstämpel | Tidsstämpel för den senaste slutförda testkörningen. |
| Godkänd | Antal lyckade körningar. |
| Misslyckades | Antal misslyckade körningar. |
| Min&#42; | Testkörningens kortaste varaktighet. |
| Max&#42; | Testkörningens längsta varaktighet. |
| Median&#42; | Beräknad medianduration för alla testkörningar. |
| Genomsnitt &#42; | Beräknad genomsnittlig varaktighet för alla testkörningar. |
| StdDev&#42; | Standardavvikelsen. |
| 90p&#42; | 90-percentil. |
| 99p&#42; | 99-percentil. |
| 99.9p&#42; | 99,9 percentil. |
| Reellt dataflöde &#42; | Antal körningar delat med förfluten körningstid. |

Dessa mått skrivs med hjälp av utgivare som kan läggas till med parametern `add` (på samma sätt som när du lägger till tester). Det finns för närvarande två alternativ:

* **CSVPublisher** - utdata är en CSV-fil.
* **ConsolePublisher** - utdata visas i konsolen.

Som standard är båda utgivare aktiverade.

Det finns också två lägen där mätvärdena rapporteras:

* Publiceringsläget **simple** - rapporterar resultatet från körningens början till publiceringspunkten.
* Publiceringsläget **interval** - rapporterar resultatet i en given tidsram. Du kan ställa in tidsbildrutan med parametern **interval** för publiceringsläge.

I följande exempel visas hur du konfigurerar parametern `intervals` antingen på kommandoraden eller genom att använda en dynamisk konfigurationsfil.

Genom att använda kommandoradsparametrar:

```xml
java -jar toughday2.jar --host=localhost --add CreatePageTreeTest --publishmode type=intervals interval=10s
```

Genom att använda en dynamisk konfigurationsfil:

```xml
publishmode:
     type : intervals
     interval : 10s
     tests:
        -add : CreatePageTreeTest
```

### Loggning {#logging}

Tough Day 2 skapar en loggmapp i samma katalog som du körde Tough Day 2. Den här mappen innehåller två typer av loggar:

* **toughday.log**: innehåller meddelanden som rör programtillståndet, felsökningsinformation och globala meddelanden.
* **toughday_&lt;testname>.log**: meddelanden som är relaterade till det angivna testet.

Loggarna skrivs inte över, efterföljande körningar lägger till meddelanden i de befintliga loggarna. Loggarna har flera nivåer. Mer information finns i parametern [loglevel.](#global-parameters).

<!--
#### Example Usage {#example-usage}

#### Known Issues {#known-issues}

[Get File](assets/toughday-6_1.jar)
-->
