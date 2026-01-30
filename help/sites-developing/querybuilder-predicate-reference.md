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
exl-id: c044d541-24d6-4975-9b38-6a4317a16358
source-git-commit: a85b54d5a7c3b00f95f439941a390dcfee883187
workflow-type: tm+mt
source-wordcount: '2291'
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

### `boolproperty` {#boolproperty}

Matchar JCR BOOLEAN-egenskaper. Endast värdena `true` och `false` godkänns. Om värdet är `false` matchar det om egenskapen har värdet `false` eller om den inte finns alls. Användbar för att kontrollera om det finns booleska flaggor som bara är inställda när de är aktiverade.

Den ärvda parametern `operation` har ingen betydelse.

Den stöder extrahering av ansikten. Tillhandahåller bucket för varje `true`- eller `false`-värde, men bara för befintliga egenskaper.

#### Egenskaper {#properties}

* **boolproperty**
Relativ sökväg till egenskap, till exempel `myFeatureEnabled` eller `jcr:content/myFeatureEnabled` .

* **värde**
Värde att kontrollera egenskap för, `true` eller `false`.

### `contentfragment` {#contentfragment}

Begränsar resultatet till innehållsfragment.

Det stöder inte filtrering.

Det stöder inte facetextrahering.

#### Egenskaper {#properties-1}

* **innehållfragment**
Den kan användas med vilket värde som helst för att kontrollera om det finns innehållsfragment.

### `dateComparison` {#datecomparison}

Jämför två JCR DATE-egenskaper med varandra. Du kan testa om de är lika, olika, större än eller större än eller lika med.

Ett predikat som bara kan filtreras och som inte kan använda ett sökindex.

#### Egenskaper {#properties-2}

* **egenskap1**

  Sökväg till första datumegenskap.

* **property2**

  Sökväg till andra datumegenskap.

* **operation**

  &quot; `equals`&quot; för exakt matchning, &quot; `!=`&quot; för likhetsjämförelse, &quot; `greater`&quot; för egenskap1 större än egenskap 2, &quot; `>=`&quot; för egenskap 1 större än eller lika med egenskap 2. Standardvärdet är `equals`.

### `daterange` {#daterange}

Matchar JCR DATE-egenskaper mot ett datum- och tidsintervall. Använder ISO8601
format för datum och tider ( `YYYY-MM-DDTHH:mm:ss.SSSZ`) och tillåter även partiella representationer, som `YYYY-MM-DD`. Alternativt kan tidsstämpeln anges i antal millisekunder sedan 1970 i UTC-tidszonen, UNIX®-tidsformatet.

Du kan söka efter vad som helst mellan två tidsstämplar, vad som helst nyare eller äldre än ett visst datum, och du kan även välja mellan inkluderande och öppna intervall.

Den stöder extrahering av ansikten. Tillhandahåller&quot;idag&quot;,&quot;den här veckan&quot;,&quot;den här månaden&quot;,&quot;de senaste tre månaderna&quot;,&quot;det här året&quot;,&quot;det senaste året&quot; och&quot;tidigare än förra året&quot;.

Det stöder inte filtrering.

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

### `excludepaths` {#excludepaths}

Utesluter noder från resultatet där deras sökväg matchar ett reguljärt uttryck.

Ett predikat som bara kan filtreras och som inte kan använda ett sökindex.

Det stöder inte facetextrahering.

#### Egenskaper {#properties-4}

* **exkluderingsdjup**

  Reguljära uttryck matchade mot resultatsökvägar, utan matchande sökvägar från resultatet.

### `fulltext` {#fulltext}

Söker efter termer i fulltextindexet.

Det stöder inte filtrering.

Det stöder inte facetextrahering.

#### Egenskaper {#properties-5}

* **fulltext**

  Fulltextsöktermer.

* **relPath**

  Den relativa sökvägen som ska sökas i egenskapen eller undernoden. Den här egenskapen är valfri.

### `group` {#group}

Gör att kapslade villkor kan skapas. Grupper kan innehålla kapslade grupper. Allt i en fråga i frågebyggaren ingår implicit i en rotgrupp som även kan ha parametrarna `p.or` och `p.not`.

Exempel på matchning av en av två egenskaper mot ett värde:

```
group.p.or=true
group.1_property=jcr:title
group.1_property.value=My Page
group.2_property=navTitle
group.2_property.value=My Page
```

`(1_property` ELLER `2_property)`.

Exempel för kapslade grupper:

```
fulltext=Management
group.p.or=true
group.1_group.path=/content/geometrixx/en
group.1_group.type=cq:Page
group.2_group.path=/content/dam/geometrixx
group.2_group.type=dam:Asset
```

Söker efter termen **Hantering** på sidor i `/content/geometrixx/en` eller i resurser i `/content/dam/geometrixx`.

Konceptuellt `fulltext AND ( (path AND type) OR (path AND type) )`. Sådana ELLER-kopplingar behöver bra index för att fungera.

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

Ett predikat som bara kan filtreras och som inte kan använda ett sökindex. Det stöder inte facetextrahering.

#### Egenskaper {#properties-7}

* **hasPermission**

  Kommaavgränsade JCR-behörigheter som den aktuella användarsessionen måste ALLA ha noden i fråga. Till exempel `jcr:write`, `jcr:modifyAccessControl`.

### `language` {#language}

Söker efter CQ-sidor på ett visst språk. Det här tittar både på sidspråksegenskapen och sidsökvägen, som ofta innehåller språket eller språket i en webbplatsstruktur på den översta nivån.

Ett predikat som bara kan filtreras och som inte kan använda ett sökindex.

Den stöder extrahering av ansikten. Tillhandahåller bucket för varje unik språkkod.

#### Egenskaper {#properties-8}

* **språk**

  ISO-språkkod, till exempel `de`.

### `mainasset` {#mainasset}

Kontrollerar om en nod är en DAM-huvudresurs och inte en underresurs. I princip finns alla noder inte i en delresursnod. Söker inte efter nodtypen `dam:Asset`. Om du vill använda det här predikatet anger du `mainasset=true` eller `mainasset=false`, det finns inga fler egenskaper.

Ett predikat som bara kan filtreras och som inte kan använda ett sökindex.

Det har stöd för facetteextrahering och har två klot för huvud- och delresurser.

#### Egenskaper {#properties-9}

* **huvudresurs**

  Boolean, `true` för huvudresurser, `false` för delresurser.

### medlemOf {#memberof}

Söker efter objekt som är medlemmar i en specifik [snedresurssamling](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/resource/collection/ResourceCollection.html).

Ett predikat som bara kan filtreras och som inte kan använda ett sökindex. Det stöder inte facetextrahering.

#### Egenskaper {#properties-10}

* **MemberOf**

  Sökväg till Sling-resurssamling.

### `nodename` {#nodename}

Matchar JCR-nodnamn.

Den stöder extrahering av ansikten. Anger bucket för varje unikt nodnamn (filnamn).

#### Egenskaper {#properties-11}

* **nodename**

  Nodnamnsmönster som tillåter jokertecken: `*` = vilket eller inget tecken, `?` = valfritt tecken, `[abc]` = endast tecken inom hakparenteser.

### `notexpired` {#notexpired}

Matchar objekt genom att kontrollera om en JCR DATE-egenskap är större eller lika med den aktuella servertiden. Kan användas för att kontrollera en `expiresAt`-liknande datumegenskap och begränsa till endast de som ännu inte har gått ut ( `notexpired=true`) eller som redan har gått ut ( `notexpired=false`).

Det stöder inte filtrering.

Det stöder facetextrahering på samma sätt som daterange-predikatet.

#### Egenskaper {#properties-12}

* **har inte gått ut**

  Boolean, `true`, har inte gått ut än (datum i framtiden eller lika med), `false` för förfallit (tidigare datum) (obligatoriskt).

* **egenskap**

  Relativ sökväg till egenskapen `DATE` som ska kontrolleras (obligatoriskt).

### `orderby` {#orderby}

Gör att resultaten kan sorteras. Om det krävs en ordning efter flera egenskaper måste predikatet läggas till flera gånger med hjälp av talprefixet, till exempel `1_orderby=first`, `2_oderby=second`.

#### Egenskaper {#properties-13}

* **orderby**

  Antingen JCR-egenskapsnamn som anges av ett radavstånd på, till exempel, `@jcr:lastModified` eller `@jcr:content/jcr:title`, eller ett annat predikat i frågan, till exempel `2_property`, som ska sorteras.

* **sortera**

  Sorteringsriktning, antingen `desc` för fallande eller `asc` för stigande (standard).

* **case**

  Om värdet är `ignore` blir sorteringsskiftläget okänsligt, vilket innebär att&quot;a&quot; kommer före&quot;B&quot;. Om det är tomt eller utelämnas är sorteringen skiftlägeskänslig, vilket innebär att&quot;B&quot; kommer före&quot;a&quot;

### `path` {#path}

Söker i en viss bana.

Det stöder inte facetextrahering.

#### Egenskaper {#properties-14}

* **sökväg**

  Banmönster. När det är `exact=false` (standard) matchar sökningen hela underträdet under den angivna sökvägen, ungefär som när `//*` läggs till i XPath, men den innehåller inte själva bassökvägen. När `exact=true` matchar sökningen endast den exakta sökvägen, som kan innehålla `*` jokertecken. Om `self` anges inkluderas basnoden och hela underträdet i sökningen.


* **exact**

  Om `exact` är true (på) måste den exakta sökvägen matcha, men den kan innehålla enkla jokertecken ( `*`) som matchar namn, men inte `/`. Om det är false (standard) inkluderas alla underordnade (valfritt).

* **flat**

  Söker bara efter direkt underordnade (som att lägga till `/*` i `xpath`) (används bara om `exact` inte är sant, valfritt).

* **self**

  Söker i underträdet men inkluderar basnoden som angetts som sökväg (inga jokertecken).

### `property` {#property}

Matchar JCR-egenskaper och deras värden.

Den stöder extrahering av ansikten. Innehåller grupper för varje unikt egenskapsvärde i resultatet.

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

  Använd `equals` för en exakt matchning (standard) och `unequals` för en jämförelse av olikhet. Använd `like` för att använda den valfria `jcr:like` XPath-funktionen. Använd `not` om du inte har någon matchning (till exempel `not(@prop)` i XPath). I det här fallet ignoreras parametern `value`. Använd `exists` för att kontrollera om en egenskap finns: `true` (standard) kräver egenskapen och `false` är ekvivalent med `not`.

* **djup**

  Ett antal jokernivåer under vilka egenskapen och den relativa sökvägen kan finnas. `property=size depth=2` kontrollerar till exempel nod och storlek, nod/&amp;ast;/size och node/&amp;ast;/&amp;ast;/size.

### `rangeproperty` {#rangeproperty}

Matchar en JCR-egenskap mot ett intervall. Gäller för egenskaper med linjära typer, som `LONG`, `DOUBLE` och `DECIMAL`. För `DATE`, se daterange-predikatet med optimerade indata för datumformat.

Du kan definiera en nedre gräns och en övre gräns eller endast en av dem. Åtgärden (t.ex. &quot;mindre än&quot; eller &quot;mindre eller lika med&quot;) kan också anges individuellt för den nedre och övre gränsen.

Det stöder inte facetextrahering.

#### Egenskaper {#properties-16}

* **egenskap**

  Relativ sökväg till egenskap.

* **lowerBound**

  Lägre gräns för att kontrollera egenskap för.

* **lowerOperation**

  &quot; `>`&quot; (standard) eller &quot; `>=`&quot; gäller för `lowerValue`

* **upperBound**

  Övre gräns för att kontrollera egenskap för.

* **upperOperation**

  &quot; `<`&quot; (standard) eller &quot; `<=`&quot; gäller för `lowerValue`

* **decimal**

  &quot; `true`&quot; om egenskapen checked är av typen Decimal

### `relativedaterange` {#relativedaterange}

Matchar `JCR DATE`-egenskaper mot ett datum och ett tidsintervall med tidsförskjutningar i förhållande till den aktuella servertiden. Du kan ange `lowerBound` och `upperBound` med antingen ett millisekundsvärde eller bugzilla-syntaxen `1s 2m 3h 4d 5w 6M 7y`. Prefix med `-` om du vill ange en negativ förskjutning före den aktuella tiden. Om du bara anger `lowerBound` eller `upperBound` blir den andra standardvärdet 0, vilket innebär den aktuella tiden.

Till exempel:

* `upperBound=1h` (och inte `lowerBound`) skulle välja något under nästa timme
* `lowerBound=-1d` (och inte `upperBound`) väljer något under de senaste 24 timmarna
* `lowerBound=-6M` och `upperBound=-3M` väljer mellan sex och tre månader
* `lowerBound=-1500` och `upperBound=5500` skulle välja mellan 1 500 millisekunder tidigare och 5 500 millisekunder i framtiden
* `lowerBound=1d` och `upperBound=2d` väljer vad som helst i övermorgon

Den tar inte hänsyn till skottår och alla månader är 30 dagar.

Det stöder inte filtrering.

Det stöder facetextrahering på samma sätt som daterange-predikatet.

#### Egenskaper {#properties-17}

* **upperBound**

  Övre gräns i millisekunder eller `1s 2m 3h 4d 5w 6M 7y` (en sekund, två minuter, tre timmar, fyra dagar, fem veckor, sex månader, sju år) i förhållande till aktuell servertid, använd &quot;-&quot; för negativ offset.

* **lowerBound**

  Nedre datumgräns i millisekunder eller `1s 2m 3h 4d 5w 6M 7y` (en sekund, två minuter, tre timmar, fyra dagar, fem veckor, sex månader, sju år) i förhållande till aktuell servertid, använd &quot;-&quot; för negativ offset.

### `root` {#root}

Rotpredikatgrupp. Den har stöd för alla funktioner i en grupp och gör att du kan ange globala frågeparametrar.

Namnet &quot;root&quot; används aldrig i en fråga, det är implicit.

#### Egenskaper {#properties-18}

* **p.offset**

  Numret som anger början på resultatsidan, det vill säga hur många objekt som ska hoppas över.

* **p.limit**

  Numret som anger sidstorleken.

* **p.gissningssumma**

  Om du vill undvika kostnaden för att beräkna en fullständig resultatsumma ska du inte räkna alla träffar. Ställ i stället in en maximal summa att räkna upp till (till exempel `1000`) för att ge användarna en grov storlek och exakta summor för mindre resultat. Eller ange det till `true` så att det bara räknas upp till det minsta antal som krävs: `p.offset + p.limit`.


* **p.excerpt**

  Om värdet är `true` inkluderar du utdrag av fullständig text i resultatet.

* **p.hits**

  (endast för JSON-servern) väljer du hur träffar skrivs som JSON, med dessa standardinställningar (utbyggbara via ResultHitWriter-tjänsten):

   * **enkel**:

     Minimala objekt som `path`, `title`, `lastmodified`, `excerpt` (om de anges).

   * **full**:

     Resultaten återges som Sling JSON för varje nod, där `jcr:path` visar träffsökvägen. Som standard innehåller svaret bara nodens direkta egenskaper. Använd `p.nodedepth=N` för att inkludera djupare innehåll, där `0` returnerar hela underträdet. Ange `p.acls=true` så att den aktuella sessionens JCR-behörigheter inkluderas för varje objekt (`create` = `add_node`, `modify` = `set_property`, `delete` = `remove`).


   * **selektiv**:

     Svaret innehåller bara de egenskaper som anges i `p.properties`, som är en blankstegsavgränsad lista med relativa sökvägar (använd `+` i URL-adresser). Om en relativ bana har ett djup som är större än 1, kapslas den in som underordnade objekt. Den speciella `jcr:path`-egenskapen innehåller alltid träffsökvägen.


### `savedquery` {#savedquery}

Den innehåller alla predikat för en beständig fråga i den aktuella frågan som ett undergruppspredikat.

Den kör inte en extra fråga utan utökar den aktuella frågan.

Frågor kan sparas programmatiskt med `QueryBuilder#storeQuery()`. Formatet kan vara en strängegenskap med flera rader eller en `nt:file`-nod som innehåller frågan som en textfil i Java™-egenskapsformat.

Det stöder inte facetextrahering för predikaten i den sparade frågan.

#### Egenskaper {#properties-19}

* **sparad fråga**

  Sökväg till den sparade frågan (String-egenskapen eller `nt:file`-noden).

### `similar` {#similar}

Likhetssökning med JCR XPath:s `rep:similar()`.

Det stöder inte filtrering. Det stöder inte facetextrahering.

#### Egenskaper {#properties-20}

* **liknande**
Absolut sökväg till noden som liknande noder ska hittas för.

* **lokal**
En relativ sökväg till en underordnad nod eller `.` för den aktuella noden (valfritt, standardvärdet är `.`).

### `tag` {#tag}

Söker efter innehåll som har taggats med en eller flera taggar genom att ange sökvägar för taggtiteln.

Den stöder extrahering av ansikten. Tillhandahåller bucket för varje unik tagg med hjälp av den aktuella taggtitelsökvägen.

#### Egenskaper {#properties-21}

* **tagg**

  Taggtitelsökväg som du vill söka efter, t.ex. &quot;Resursegenskaper: Orientering / Liggande&quot;.

* **N_värde**

  Använd `1_value`, `2_value`, ... för att kontrollera om det finns flera taggar (i kombination med `OR` som standard, med `AND` if och=true) (sedan 5.6).

* **egenskap**

  Egenskap (eller relativ sökväg till egenskap) som ska kontrolleras (standard `cq:tags`)

### `tagid` {#tagid}

Söker efter innehåll som taggats med en eller flera taggar genom att ange tagg-ID:n.

Den stöder extrahering av ansikten. Tillhandahåller bucket för varje unik tagg med deras aktuella tagg-ID.

#### Egenskaper {#properties-22}

* **tagid**

  Tagg-id så att du kan söka efter t.ex. `properties:orientation/landscape`.

* **N_värde**

  Använd `1_value`, `2_value`, ... för att kontrollera om det finns flera `tagids` (i kombination med `OR` som standard, med `AND` if och=true) (sedan 5.6).

* **egenskap**

  Egenskap (eller relativ sökväg till egenskap) som ska undersökas (standard `cq:tags`).

### `tagsearch` {#tagsearch}

Söker efter innehåll som taggats med en eller flera taggar genom att ange nyckelord. Söker först efter taggar som innehåller dessa nyckelord i sina titlar och begränsar sedan resultatet till endast taggade objekt.

Det stöder inte facetextrahering.

#### Egenskaper {#Properties-1}

* **tagsearch**

  Nyckelord att söka efter i taggtitlar.

* **egenskap**

  Egenskap (eller relativ sökväg till egenskap) som ska undersökas (standard `cq:tags`).

* **språk**

  Om du bara vill söka i en viss lokaliserad taggtitel (till exempel `de`).

* **alla**

  (bool) Sök i hela taggens fulltext, det vill säga alla titlar, beskrivning och så vidare. Har prioritet framför&quot;l `ang`&quot;.

### `type` {#type}

Begränsar resultaten till en viss JCR-nodtyp, både den primära nodtypen eller mixtypen och söker efter undertyper av den nodtypen. Databasens sökindex måste omfatta nodtyperna för effektiv körning.

Den stöder extrahering av ansikten. Innehåller grupper för varje unik typ i resultatet.

#### Egenskaper {#Properties-2}

* **typ**

  Nodtyp eller mixnamn att söka efter, till exempel `cq:Page`.
