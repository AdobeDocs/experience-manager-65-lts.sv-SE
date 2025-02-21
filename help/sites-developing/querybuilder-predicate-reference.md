---
title: Predikatreferens för Query Builder
description: Fullständig predikatreferens för Query Builder API.
contentOwner: sarchiz
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: platform
solution: Experience Manager, Experience Manager Sites
feature: Developing,Search,Query Builder
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2313'
ht-degree: 0%

---

# Predikatreferens för Query Builder{#query-builder-predicate-reference}

>[!CAUTION]
>
>Informationen på denna sida är inte uttömmande.
>
>Mer information finns i listan under **Tillgängliga predikat** på felsökningskonsolen i Query Builder, till exempel på:
>* [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
>
>Se till exempel:
>
>* [http://localhost:4502/system/console/services?filter=%28component.factory%3Dcom.day.cq.search.eval.PredicateEvaluator%2F*%29](http://localhost:4502/system/console/services?filter=%28component.factory%3Dcom.day.cq.search.eval.PredicateEvaluator%2F*%29)

## Allmänt {#general}

* [root](#root)
* [grupp](#group)
* [orderby](#orderby)

## Predikat {#predicates}

* [boolproperty](/help/sites-developing/querybuilder-predicate-reference.md#boolproperty)
* [innehållfragment](/help/sites-developing/querybuilder-predicate-reference.md#contentfragment)
* [dateComparison](/help/sites-developing/querybuilder-predicate-reference.md#datecomparison)
* [daterange](/help/sites-developing/querybuilder-predicate-reference.md#daterange)
* [exkluderingar](/help/sites-developing/querybuilder-predicate-reference.md#excludepaths)
* [fulltext](/help/sites-developing/querybuilder-predicate-reference.md#fulltext)
* [hasPermission](/help/sites-developing/querybuilder-predicate-reference.md#haspermission)
* [språk](/help/sites-developing/querybuilder-predicate-reference.md#language)
* [huvudtillgång](/help/sites-developing/querybuilder-predicate-reference.md#mainasset)
* [medlemOf](/help/sites-developing/querybuilder-predicate-reference.md#memberof)
* [nodename](/help/sites-developing/querybuilder-predicate-reference.md#nodename)
* [inte utgånget](/help/sites-developing/querybuilder-predicate-reference.md#notexpired)
* [bana](/help/sites-developing/querybuilder-predicate-reference.md#path)
* [property](/help/sites-developing/querybuilder-predicate-reference.md#property)
* [rangeProperty](/help/sites-developing/querybuilder-predicate-reference.md#rangeproperty)
* [relativ](/help/sites-developing/querybuilder-predicate-reference.md#relativedaterange)
* [sparad fråga](/help/sites-developing/querybuilder-predicate-reference.md#savedquery)
* [liknande](/help/sites-developing/querybuilder-predicate-reference.md#similar)
* [tag](/help/sites-developing/querybuilder-predicate-reference.md#tag)
* [tagid](/help/sites-developing/querybuilder-predicate-reference.md#tagid)
* [tagsearch](/help/sites-developing/querybuilder-predicate-reference.md#tagsearch)
* [type](/help/sites-developing/querybuilder-predicate-reference.md#type)

### boolproperty {#boolproperty}

Matchar JCR BOOLEAN-egenskaper. Endast värdena `true` och `false` godkänns. Om `false` matchar det om egenskapen har värdet `false` eller om den inte finns alls. Detta kan vara användbart för att kontrollera om det finns booleska flaggor som bara är inställda när de är aktiverade.

Den ärvda parametern `operation` har ingen betydelse.

Stöder extrahering av ansikten. Tillhandahåller bucket för varje `true`- eller `false`-värde, men bara för befintliga egenskaper.

#### Egenskaper {#properties}

* **boolproperty**
Relativ sökväg till egenskap, till exempel `myFeatureEnabled` eller `jcr:content/myFeatureEnabled` .

* **värde**
Värde att kontrollera egenskap för, `true` eller `false`.

### innehållfragment {#contentfragment}

Begränsar resultatet till innehållsfragment.

Filtrering stöds inte.

Stöder inte facetextrahering.

#### Egenskaper {#properties-1}

* **innehållfragment**
Den kan användas med vilket värde som helst för att kontrollera om det finns innehållsfragment.

### dateComparison {#datecomparison}

Jämför två JCR DATE-egenskaper med varandra. Du kan testa om de är lika, olika, större än eller större än eller lika med.

Detta är ett predikat som bara kan filtreras och det går inte att använda ett sökindex.

#### Egenskaper {#properties-2}

* **egenskap1**

  Sökväg till första datumegenskap.

* **property2**

  Sökväg till andra datumegenskap.

* **operation**

  &quot; `equals`&quot; för exakt matchning, &quot; `!=`&quot; för likhetsjämförelse, &quot; `greater`&quot; för egenskap1 större än egenskap 2, &quot; `>=`&quot; för egenskap 1 större än eller lika med egenskap 2. Standardvärdet är `equals`.

### daterange {#daterange}

Matchar JCR DATE-egenskaper mot ett datum/tidsintervall. Detta använder ISO8601
format för datum och tider ( `YYYY-MM-DDTHH:mm:ss.SSSZ`) och tillåter även partiella representationer, som `YYYY-MM-DD`. Alternativt kan tidsstämpeln anges i antal millisekunder sedan 1970 i UTC-tidszonen, UNIX®-tidsformatet.

Du kan söka efter vad som helst mellan två tidsstämplar, vad som helst nyare eller äldre än ett visst datum, och du kan även välja mellan inkluderande och öppna intervall.

Stöder extrahering av ansikten. Innehåller fickorna&quot;idag&quot;,&quot;den här veckan&quot;,&quot;den här månaden&quot;,&quot;de senaste tre månaderna&quot;,&quot;det här året&quot;,&quot;det senaste året&quot; och&quot;tidigare än det senaste året&quot;.

Filtrering stöds inte.

#### Egenskaper {#properties-3}

* **egenskap**

  Relativ sökväg till en `DATE`-egenskap, till exempel `jcr:lastModified`.

* **lowerBound**

  Det lägre datumet är bundet till att kontrollera egenskapen för exempelvis `2014-10-01`.

* **lowerOperation**

  &quot; `>`&quot; (nyare) eller &quot; `>=`&quot; (nyare eller nyare) gäller för `lowerBound`. Standardvärdet är `>`.

* **upperBound**

  Övre gräns för att kontrollera egenskapen för till exempel `2014-10-01T12:15:00`.

* **upperOperation**

  `<` (äldre) eller `<=` (äldre) gäller för `upperBound`. Standardvärdet är `<`.

* **timeZone**

  ID för den tidszon som ska användas när den inte anges som en ISO-8601-datumsträng. Standardvärdet är systemets standardtidszon.

### exkluderingar {#excludepaths}

Utesluter noder från resultatet där deras sökväg matchar ett reguljärt uttryck.

Detta är ett predikat som bara kan filtreras och det går inte att använda ett sökindex.

Stöder inte facetextrahering.

#### Egenskaper {#properties-4}

* **exkluderingsdjup**

  Reguljära uttryck matchade mot resultatsökvägar, utan matchande sökvägar från resultatet.

### fulltext {#fulltext}

Söker efter termer i fulltextindexet.

Filtrering stöds inte.

Stöder inte facetextrahering.

#### Egenskaper {#properties-5}

* **fulltext**

  Fulltextsöktermer.

* **relPath**

  Den relativa sökvägen som ska sökas i egenskapen eller undernoden. Den här egenskapen är valfri.

### grupp {#group}

Tillåter att kapslade villkor skapas. Grupper kan innehålla kapslade grupper. Allt i en fråga i frågebyggaren ingår implicit i en rotgrupp som även kan ha parametrarna `p.or` och `p.not`.

Exempel på matchning av en av två egenskaper mot ett värde:

```
group.p.or=true
group.1_property=jcr:title
group.1_property.value=My Page
group.2_property=navTitle
group.2_property.value=My Page
```

Detta är begreppsmässigt `(1_property` ELLER `2_property)`.

Exempel för kapslade grupper:

```
fulltext=Management
group.p.or=true
group.1_group.path=/content/geometrixx/en
group.1_group.type=cq:Page
group.2_group.path=/content/dam/geometrixx
group.2_group.type=dam:Asset
```

Detta söker efter termen **Hantering** på sidor i `/content/geometrixx/en` eller i resurser i `/content/dam/geometrixx`.

Detta är begreppsmässigt `fulltext AND ( (path AND type) OR (path AND type) )`. Sådana ELLER-kopplingar behöver bra index för att fungera.

#### Egenskaper {#properties-6}

* **p.or**

  Om värdet är `true` måste bara ett predikat i gruppen matcha. Standardvärdet är `false`, vilket innebär att alla måste matcha

* **p.not**

  Om värdet är `true` negeras gruppen (standardvärdet är `false`).

* **&lt;predikat>**

  Lägger till kapslade predikat.

* **N_&lt;predikat>**

  Lägger till flera kapslade predikat samtidigt, som `1_property, 2_property, ...`.

### hasPermission {#haspermission}

Begränsar resultatet till objekt där den aktuella sessionen har angivna [JCR-behörigheter.](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/2.0/16_Access_Control_Management.html#16.2.3%20Standard%20Privileges)

Detta är ett predikat som bara kan filtreras och det går inte att använda ett sökindex. Det stöder inte facetextrahering.

#### Egenskaper {#properties-7}

* **hasPermission**

  Kommaavgränsade JCR-behörigheter som den aktuella användarsessionen måste ALLA ha noden i fråga. Till exempel `jcr:write`, `jcr:modifyAccessControl`.

### språk {#language}

Söker efter CQ-sidor på ett visst språk. Det här tittar både på sidspråksegenskapen och sidsökvägen, som ofta innehåller språket eller språket i en webbplatsstruktur på den översta nivån.

Detta är ett predikat som bara kan filtreras och det går inte att använda ett sökindex.

Stöder extrahering av ansikten. Tillhandahåller bucket för varje unik språkkod.

#### Egenskaper {#properties-8}

* **språk**

  ISO-språkkod, till exempel `de`

### huvudtillgång {#mainasset}

Kontrollerar om en nod är en DAM-huvudresurs och inte en underresurs. Detta är i stort sett alla noder som inte finns i en delresursnod. Detta söker inte efter nodtypen `dam:Asset`. Om du vill använda det här predikatet, ange `mainasset=true` eller `mainasset=false`, finns det inga fler egenskaper.

Detta är ett predikat som bara kan filtreras och det går inte att använda ett sökindex.

Stöder facet-extrahering och ger två möjligheter för huvud- och delresurser.

#### Egenskaper {#properties-9}

* **huvudresurs**

  Boolean, `true` för huvudresurser, `false` för delresurser.

### medlemOf {#memberof}

Söker efter objekt som är medlemmar i en specifik [snedresurssamling](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/resource/collection/ResourceCollection.html).

Detta är ett predikat som bara kan filtreras och det går inte att använda ett sökindex. Stöder inte facetextrahering.

#### Egenskaper {#properties-10}

* **MemberOf**

  Sökväg till Sling-resurssamling.

### nodename {#nodename}

Matchar JCR-nodnamn.

Stöder extrahering av ansikten. Anger bucket för varje unikt nodnamn (filnamn).

#### Egenskaper {#properties-11}

* **nodename**

  Nodnamnsmönster som tillåter jokertecken: `*` = vilket eller inget tecken, `?` = valfritt tecken, `[abc]` = endast tecken inom hakparenteser.

### inte utgånget {#notexpired}

Matchar objekt genom att kontrollera om en JCR DATE-egenskap är större eller lika med den aktuella servertiden. Detta kan användas för att kontrollera en `expiresAt`-liknande datumegenskap och begränsa till endast de som ännu inte har gått ut ( `notexpired=true`) eller som redan har gått ut ( `notexpired=false`).

Filtrering stöds inte.

Stöder facet-extrahering på samma sätt som daterange-predikatet.

#### Egenskaper {#properties-12}

* **har inte gått ut**

  Boolean, `true`, har inte gått ut än (datum i framtiden eller lika med), `false` för förfallit (tidigare datum) (obligatoriskt).

* **egenskap**

  Relativ sökväg till egenskapen `DATE` som ska kontrolleras (obligatoriskt).

### orderby {#orderby}

Låter resultaten sorteras. Om det krävs en ordning efter flera egenskaper måste predikatet läggas till flera gånger med hjälp av talprefixet, till exempel `1_orderby=first`, `2_oderby=second`.

#### Egenskaper {#properties-13}

* **orderby**

  Antingen JCR-egenskapsnamn som anges av ett radavstånd på, till exempel, `@jcr:lastModified` eller `@jcr:content/jcr:title`, eller ett annat predikat i frågan, till exempel `2_property`, som ska sorteras.

* **sortera**

  Sorteringsriktning, antingen `desc` för fallande eller `asc` för stigande (standard).

* **case**

  Om värdet är `ignore` blir sorteringsskiftläget okänsligt, vilket innebär att&quot;a&quot; kommer före&quot;B&quot;. Om det är tomt eller utelämnas är sorteringen skiftlägeskänslig, vilket innebär att&quot;B&quot; kommer före&quot;a&quot;

### bana {#path}

Söker i en viss bana.

Stöder inte facetextrahering.

#### Egenskaper {#properties-14}

* **sökväg**

  Banmönster. Beroende på hur exakt det är, matchar antingen hela underträdet (som att lägga till `//*` i XPath, men observera att detta inte inkluderar bassökvägen) (exact=false, standard) eller bara en exakt sökvägsmatchning, som kan innehålla jokertecken ( `*`). Om du anger sig själv genomsöks hela underträdet inklusive basnoden.

* **exact**

  Om `exact` är true/on måste den exakta sökvägen matcha, men den kan innehålla enkla jokertecken ( `*`) som matchar namn, men inte `/`. Om det är false (standard) inkluderas alla underordnade (valfritt).

* **flat**

  Söker bara efter direkt underordnade objekt (som att lägga till `/*` i XPath) (används bara om `exact` inte är sant, valfritt).

* **self**

  Söker i underträdet men inkluderar basnoden som angetts som sökväg (inga jokertecken).

### property {#property}

Matchar JCR-egenskaper och deras värden.

Stöder extrahering av ansikten. Innehåller grupper för varje unikt egenskapsvärde i resultatet.

#### Egenskaper {#properties-15}

* **egenskap**

  Relativ sökväg till egenskap, till exempel `jcr:title`.

* **värde**

  Värde som egenskapen ska kontrolleras för; följer JCR-egenskapstypen till strängkonverteringar.

* **N_värde**

  Använd `1_value`, `2_value`, ... för att kontrollera om det finns flera värden (kombinerat med `OR` som standard, med `AND` if och=true) (sedan 5.3).

* **och**

  Ange som true för att kombinera flera värden ( `N_value`) med AND (sedan 5.3).

* **operation**

  &quot;`equals`&quot; för exakt matchning (standard), &quot; `unequals`&quot; för jämförelse av olikhet, &quot; `like`&quot; för användning av funktionen `jcr:like` xpath (valfri), &quot; `not`&quot; för ingen matchning (t.ex. &quot;`not(@prop)`&quot; i xpath ignoreras value param) eller &quot; `exists`&quot; för existenskontroll (värdet kan vara true - egenskapen måste finnas, standardvärdet - eller false - samma som &quot; `not`&quot;) .

* **djup**

  Antal jokernivåer under vilka egenskapen/den relativa sökvägen kan finnas (till exempel `property=size depth=2` kontrollerar nod/storlek, nod/&amp;ast;/size och node/&amp;ast;/&amp;ast;/size).

### rangeProperty {#rangeproperty}

Matchar en JCR-egenskap mot ett intervall. Detta gäller för egenskaper med linjära typer som `LONG`, `DOUBLE` och `DECIMAL`. För `DATE`, se daterange-predikatet med optimerade indata för datumformat.

Du kan definiera en nedre gräns och en övre gräns eller endast en av dem. Åtgärden (t.ex. &quot;mindre än&quot; eller &quot;mindre eller lika med&quot;) kan också anges individuellt för den nedre och övre gränsen.

Stöder inte facetextrahering.

#### Egenskaper {#properties-16}

* **egenskap**

  Relativ sökväg till egenskap.

* **lowerBound**

  Lägre gräns för att kontrollera egenskap för.

* **lowerOperation**

  &quot; `>`&quot; (standard) eller &quot; `>=`&quot;, gäller för `lowerValue`

* **upperBound**

  Övre gräns för att kontrollera egenskap för.

* **upperOperation**

  &quot; `<`&quot; (standard) eller &quot; `<=`&quot;, gäller för `lowerValue`

* **decimal**

  &quot; `true`&quot; om egenskapen checked är av typen Decimal

### relativ {#relativedaterange}

Matchar `JCR DATE`-egenskaper mot ett datum/tidsintervall med tidsförskjutningar i förhållande till den aktuella servertiden. Du kan ange `lowerBound` och `upperBound` med antingen ett millisekundvärde eller bugzilla-syntaxen `1s 2m 3h 4d 5w 6M 7y` (en sekund, två minuter, tre timmar, fyra dagar, fem veckor, sex månader, sju år). Prefix med `-` om du vill ange en negativ förskjutning före den aktuella tiden. Om du bara anger `lowerBound` eller `upperBound` blir den andra standardvärdet 0, vilket innebär den aktuella tiden.

Till exempel:

* `upperBound=1h` (och inte `lowerBound`) skulle välja något under nästa timme
* `lowerBound=-1d` (och inte `upperBound`) väljer något under de senaste 24 timmarna
* `lowerBound=-6M` och `upperBound=-3M` väljer mellan sex och tre månader
* `lowerBound=-1500` och `upperBound=5500` skulle välja mellan 1 500 millisekunder tidigare och 5 500 millisekunder i framtiden
* `lowerBound=1d` och `upperBound=2d` väljer vad som helst i övermorgon

Den tar inte hänsyn till skottår och alla månader är 30 dagar.

Filtrering stöds inte.

Stöder facet-extrahering på samma sätt som daterange-predikatet.

#### Egenskaper {#properties-17}

* **upperBound**

  Övre gräns i millisekunder eller `1s 2m 3h 4d 5w 6M 7y` (en sekund, två minuter, tre timmar, fyra dagar, fem veckor, sex månader, sju år) i förhållande till aktuell servertid, använd &quot;-&quot; för negativ offset.

* **lowerBound**

  Nedre datumgräns i millisekunder eller `1s 2m 3h 4d 5w 6M 7y` (en sekund, två minuter, tre timmar, fyra dagar, fem veckor, sex månader, sju år) i förhållande till aktuell servertid, använd &quot;-&quot; för negativ offset.

### root {#root}

Rotpredikatgrupp. Stöder alla funktioner i en grupp och låter dig ange globala frågeparametrar.

Namnet &quot;root&quot; används aldrig i en fråga, det är implicit.

#### Egenskaper {#properties-18}

* **p.offset**

  Numret som anger början på resultatsidan, det vill säga hur många objekt som ska hoppas över.

* **p.limit**

  Numret som anger sidstorleken.

* **p.gissningssumma**

  Rekommenderas: undvik att beräkna den totala resultatsumman som kan vara kostsam. Antingen ett tal som anger den högsta totalsumman som ska räknas upp till (till exempel 1000, ett tal som ger användarna tillräckligt med feedback på grovstorleken och exakta tal för mindre resultat) eller `true` för att räkna endast upp till det minsta nödvändiga `p.offset` + `p.limit`.

* **p.excerpt**

  Om värdet är `true`, inkluderar du det fullständiga textutdraget i resultatet.

* **p.hits**

  (endast för JSON-servern) väljer du hur träffar skrivs som JSON, med dessa standardinställningar (utbyggbara via ResultHitWriter-tjänsten):

   * **enkel**:

     Minimala objekt som `path`, `title`, `lastmodified`, `excerpt` (om de anges).

   * **full**:

     Sling JSON-återgivning av noden, med `jcr:path` som anger träffens sökväg: som standard visas bara nodens direkta egenskaper, inklusive ett djupare träd med `p.nodedepth=N`, med 0 som betyder hela det oändliga underträdet, lägg till `p.acls=true` för att inkludera JCR-behörigheten för den aktuella sessionen på det angivna resultatobjektet (mappningar: `create` = `add_node`, `modify` = `set_property`, `delete` =  `remove`).

   * **selektiv**:

     Endast egenskaper som anges i `p.properties`, som är en blankstegsavgränsad (använd&quot;+&quot; i URL:er) lista med relativa sökvägar. Om den relativa sökvägen har ett djup > 1 representeras de som underordnade objekt. Den speciella jcr:path-egenskapen innehåller sökvägen till träffen

### sparad fråga {#savedquery}

Inkluderar alla predikat för en beständig frågebyggarfråga i den aktuella frågan som ett undergruppsprediat.

Detta kör inte en extra fråga utan utökar den aktuella frågan.

Frågor kan sparas programmatiskt med `QueryBuilder#storeQuery()`. Formatet kan vara en strängegenskap med flera rader eller en `nt:file`-nod som innehåller frågan som en textfil i Java™-egenskapsformat.

Stöder inte facetextrahering för predikaten i den sparade frågan.

#### Egenskaper {#properties-19}

* **sparad fråga**

  Sökväg till den sparade frågan (String-egenskapen eller `nt:file`-noden).

### liknande {#similar}

Likhetssökning med JCR XPath:s `rep:similar()`.

Filtrering stöds inte. Stöder inte facetextrahering.

#### Egenskaper {#properties-20}

* **liknande**
Absolut sökväg till noden som liknande noder ska hittas för.

* **lokal**
En relativ sökväg till en underordnad nod eller `.` för den aktuella noden (valfritt, standardvärdet är `.`).

### tag {#tag}

Söker efter innehåll som har taggats med en eller flera taggar genom att ange sökvägar för taggtiteln.

Stöder extrahering av ansikten. Tillhandahåller bucket för varje unik tagg med hjälp av den aktuella taggtitelsökvägen.

#### Egenskaper {#properties-21}

* **tagg**

  Taggtitelsökväg som du vill söka efter, t.ex. &quot;Resursegenskaper : Orientation / Landscape&quot;.

* **N_värde**

  Använd `1_value`, `2_value`, ... för att kontrollera om det finns flera taggar (i kombination med `OR` som standard, med `AND` if och=true) (sedan 5.6).

* **egenskap**

  Egenskap (eller relativ sökväg till egenskap) som ska kontrolleras (standard `cq:tags`)

### tagid {#tagid}

Söker efter innehåll som taggats med en eller flera taggar genom att ange tagg-ID:n.

Stöder extrahering av ansikten. Tillhandahåller bucket för varje unik tagg med deras aktuella tagg-ID.

#### Egenskaper {#properties-22}

* **tagid**

  Tagg-id så att du kan söka efter t.ex. `properties:orientation/landscape`.

* **N_värde**

  Använd `1_value`, `2_value`, ... för att kontrollera om det finns flera taggar (i kombination med `OR` som standard, med `AND` if och=true) (sedan 5.6).

* **egenskap**

  Egenskap (eller relativ sökväg till egenskap) som ska undersökas (standard `cq:tags`).

### tagsearch {#tagsearch}

Söker efter innehåll som taggats med en eller flera taggar genom att ange nyckelord. Detta söker först efter taggar som innehåller dessa nyckelord i sina titlar och begränsar sedan resultatet till endast objekt som taggats med dessa.

Stöder inte facetextrahering.

#### Egenskaper {#Properties-1}

* **tagsearch**

  Nyckelord att söka efter i taggtitlar.

* **egenskap**

  Egenskap (eller relativ sökväg till egenskap) som ska undersökas (standard `cq:tags`).

* **språk**

  Om du bara vill söka i en viss lokaliserad taggtitel (till exempel `de`).

* **alla**

  (bool) Sök i hela taggens fulltext, det vill säga alla titlar, beskrivning och så vidare. Har prioritet framför&quot;l `ang`&quot;.

### type {#type}

Begränsar resultaten till en viss JCR-nodtyp, både primär nodtyp och blandningstyp. Detta söker även efter undertyper av den nodtypen. Databasens sökindex måste omfatta nodtyperna för effektiv körning.

Stöder extrahering av ansikten. Innehåller grupper för varje unik typ i resultatet.

#### Egenskaper {#Properties-2}

* **typ**

  Nodtyp eller mixnamn att söka efter, till exempel `cq:Page`.
