---
title: Skriptstöd för HTML5-formulär
description: JavaScript, FormCalc-egenskaper och andra metoder som stöds i HTML5 Forms.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 4110e3d5-7242-459b-b249-637b5bf3472d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3916'
ht-degree: 0%

---

# Skriptstöd för HTML5-formulär {#scripting-support-for-html-forms}

JavaScript, FormCalc-egenskaper och -metoder som kan användas i HTML5-formulär anges nedan:

## $event {#event}

<table>
 <tbody>
  <tr>
   <th>Egenskap </th>
   <th>Beskrivning<br /> </th>
   <th>Undantag</th>
  </tr>
  <tr>
   <td><code>prevText</code></td>
   <td>Anger fältets innehåll innan det ändras som svar på en användares åtgärder. Det här värdet kan återkallas, ungefär som en ångra-funktion.</td>
   <td><p>Fungerar inte för listrutor och listrutor. <code>PrevText </code>fungerar inte korrekt i följande fall:</p>
    <ul>
     <li>Om du skriver vissa specialteckennycklar (till exempel $ eller , eller &amp; eller @ med flera) i numeriska fält på iPad, och </li>
     <li>För datumfältet (när datum anges via kalendern).<br /> </li>
    </ul> <p>Det går inte att ange värde via skript.</p> </td>
  </tr>
  <tr>
   <td><code>target</code></td>
   <td>Anger det objekt som händelsen ska agera på.</td>
   <td>Det går inte att ange värde via skript.<br /> </td>
  </tr>
  <tr>
   <td><code>newtext</code></td>
   <td>Anger innehållet i fältet efter att det har ändrats som svar på användaråtgärder.</td>
   <td><p>Egenskapen <code>newText</code> fungerar inte korrekt i följande fall:</p>
    <ul>
     <li>Om att markera och ersätta text</li>
     <li>När du tar bort, kopierar och klistrar in text.</li>
     <li>Om du skriver vissa specialteckennycklar (till exempel $ eller , eller &amp; eller @ med flera) i numeriska fält <br /> </li>
     <li>Vid användning av Skift+alfanumerisk kombination. </li>
     <li>Använda datum/tid-fält.</li>
    </ul>
    <div>
      Det går inte att ange värde via skript.
    </div> </td>
  </tr>
  <tr>
   <td>change</td>
   <td>Anger det värde som en användare skriver eller klistrar in i ett fält omedelbart efter att de utfört åtgärden. </td>
   <td><p>Egenskapen change fungerar inte som den ska i följande fall:</p>
    <ul>
     <li>Om att markera och ersätta text</li>
     <li>När du tar bort, kopierar och klistrar in text.</li>
     <li>Om du skriver vissa specialteckennycklar (till exempel $ eller , eller &amp; eller @ med flera) i numeriska fält <br /> </li>
     <li>Vid användning av Skift+alfanumerisk kombination. </li>
     <li>Använda datum/tid-fält.</li>
    </ul> <p>Det går inte att ange värde via skript.</p> </td>
  </tr>
  <tr>
   <td>keydown</td>
   <td>Avgör om en användare trycker på en piltangent för att göra en markering. Den här egenskapen är bara tillgänglig för listrutor och nedrullningsbara listor.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>modifierare</td>
   <td>Avgör om modifieringstangenten (t.ex. Ctrl i Microsoft® Windows®) ska hållas ned när en viss händelse utförs.</td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

### $host {#host}

<table>
 <tbody>
  <tr>
   <th>Egenskap</th>
   <th>Beskrivning</th>
   <th>Undantag</th>
  </tr>
  <tr>
   <td><code>apptype</code></td>
   <td>Returnerar värddatorns programtyp. Endast tillgängligt för klientprogram.</td>
   <td>Returnerar <code>HTML 5</code>.</td>
  </tr>
  <tr>
   <td><code>name</code></td>
   <td>Returnerar namnet på det aktuella programmet.</td>
   <td>Returnerar webbläsarens namn och version. I Chrome webbläsare returneras till exempel värdet <code>Chrome &lt;version&gt;.</code></td>
  </tr>
  <tr>
   <td><code>numPages</code></td>
   <td>Returnerar antalet sidor i dokumentet.</td>
   <td>Sidindelningsprincipen för HTML5-formulär är inte identisk med PDF forms sidiningspolicy. API:t numPages kan alltså returnera olika värden i båda fallen.</td>
  </tr>
  <tr>
   <td><code>platform</code></td>
   <td>Returnerar en sträng som representerar plattformen på datorn som kör skriptet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>title</code></td>
   <td>Anger dokumentets titel. Den är bara tillgänglig för klientprogram.</td>
   <td>Det returnerar titeln på HTML-dokumentet i form av ett formulär i stället för metadatanamnet för formuläret, som om det finns PDF forms.</td>
  </tr>
  <tr>
   <td><code>version</code></td>
   <td>Returnerar en sträng som representerar versionsnumret för det aktuella programmet.</td>
   <td>Den returnerar versionen av formuläret.</td>
  </tr>
  <tr>
   <td><code>calculationsEnabled</code></td>
   <td>Anger om beräknade skript ska köras.<br /> </td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>validationsEnabled</code></td>
   <td>Anger om valideringsskript ska köras.<br /> </td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>pageUp</code></td>
   <td>Går till föregående sida.</td>
   <td>HTML5-formulär följer inte samma sidnumreringsprincip som PDF-formulär, så föregående sida i ett HTML5-formulär skiljer sig från föregående sida i ett PDF-formulär.</td>
  </tr>
  <tr>
   <td><code>pageDown</code></td>
   <td>Flyttar till nästa sida i ett formulär. Använd metoden pageDown vid körning.</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>setFocus</code></td>
   <td>Ställer in tangentbordsfokus till det angivna fältet. Fältet anges som ett objekt eller som ett SOM-uttryck för fältet. Den är bara tillgänglig för klientprogram.</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>resetdata</code></td>
   <td>Återställer fälten till deras standardvärden i ett dokument.</td>
   <td>Raderar alla data i ett formulär med sammanfogade data, i stället för att återställa dem till standardvärden.</td>
  </tr>
  <tr>
   <td><code>messageBox</code></td>
   <td>Visar en dialogruta på skärmen. Den är bara tillgänglig för klientprogram</td>
   <td>Meddelanderutan av typen Ja/Nej konverteras till OK/Avbryt. Meddelanderuta med tre knappar stöds inte.</td>
  </tr>
  <tr>
   <td>currentPage</td>
   <td><p>Anger den aktiva sidan i ett dokument vid körning.</p> <p>Sidvärdena är nollbaserade, så den första sidan i ett dokument returnerar värdet 0.</p> <p>Egenskapen currentPage är tillgänglig när layout:ready körs på en klient. Den är dock inte tillgänglig när layout:ready körs på servern eftersom egenskapen inte körs förrän formulärlayouten körs.</p> </td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

### fält {#field}

<table>
 <tbody>
  <tr>
   <th><strong><span>Egenskap</span></strong></th>
   <th><strong><span>Beskrivning<br /> </span></strong></th>
   <th><strong><span>Undantag</span></strong></th>
  </tr>
  <tr>
   <td><code>presence</code></td>
   <td>Styr det associerade objektets deltagande i olika bearbetningsfaser. Om objektet är en behållare ärver innehållet i behållaren alla begränsningar som den här kontrollen gäller.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>access</code></td>
   <td>Styr användarnas åtkomst till innehållet.</td>
   <td>Fungerar inte för exkluderingsgruppen. Dessutom ger HTML5-formulär samma behandling för icke-interaktiva och skyddade objekt.<br /> </td>
  </tr>
  <tr>
   <td><code>name</code></td>
   <td>En identifierare som används för att identifiera elementet i skriptuttryck.</td>
   <td>HTML5-formulär tillåter inte att namnegenskap anges för objekt. Det är en skrivskyddad egenskap för HTML5-formulär.</td>
  </tr>
  <tr>
   <td><code>value</code></td>
   <td>A content element that encloses a single unit of data content.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>rawValue</code></td>
   <td>Anger det oformaterade värdet för det här fältet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>formattedValue</code></td>
   <td>Anger det formaterade värdet för det här fältet.</td>
   <td>Det går inte att ange <code>formattedValue</code> via skript.</td>
  </tr>
  <tr>
   <td><code>editValue</code></td>
   <td>Anger redigeringsvärdet för det här fältet.</td>
   <td>Det går inte att ange <code>editValue </code> via skript.</td>
  </tr>
  <tr>
   <td><code>formatMessage</code></td>
   <td>Anger meddelandesträngen för formatvalidering för det här fältet.</td>
   <td>Det går inte att ange <code>formatMessage </code> via skript.</td>
  </tr>
  <tr>
   <td><code>fillcolor</code></td>
   <td>Anger bakgrundsfärgvärdet för det här fältet. Du måste ange egenskapen border.fill.presence som synlig separat.</td>
   <td>Det returnerar inte fältets standardfärg korrekt.</td>
  </tr>
  <tr>
   <td><code>border</code></td>
   <td>Kantobjektet beskriver kanten runt ett objekt.</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>ui</code></td>
   <td>Ui-objektet omsluter användargränssnittsbeskrivningen för ett formulärobjekt.</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>mandatory</code></td>
   <td>Anger värdet nullTest för fältet.</td>
   <td> </td>
  </tr>
  <tr>
   <td><code>borderColor</code></td>
   <td>Anger kantfärgvärdet för det här fältet. Du måste ange egenskapen border.edge.presence som synlig separat.</td>
   <td>Det returnerar inte fältets standardkantfärg korrekt.</td>
  </tr>
  <tr>
   <td><code>length</code></td>
   <td>Antalet objekt i listan.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>addItem</code></td>
   <td>Lägger till nya objekt i det aktuella fältet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>clearItem</code></td>
   <td>Tar bort alla objekt från fältet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>boundItem</code></td>
   <td>Hämtar det bundna värdet för ett visst visningsobjekt i en nedrullningsbar lista eller listruta.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>execCalculate</code></td>
   <td>Kör fältets beräkningsskript.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>execValidate</code></td>
   <td>Kör fältets valideringsskript.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>execEvent</code></td>
   <td>Kör objektets händelseskript.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>getItemState</code></td>
   <td>Returnerar det angivna objektets markeringstillstånd</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>setItemState</code></td>
   <td>Anger det angivna objektets urvalstillstånd.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>getDisplayItem</code></td>
   <td>Hämtar objektets visningstext för det angivna objektindexet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>getSaveItem</code></td>
   <td>Hämtar datavärdet för det angivna objektindexet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>deleteItem</code></td>
   <td>Tar bort objektet vid det angivna indexvärdet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td><code>setItems</code></td>
   <td>Anger angivna objekt i det aktuella fältet. Den ersätter befintliga objekt.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>h</td>
   <td>Ett mått på layoutens höjd.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>w</td>
   <td>A measurement specifying the width for the layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>x</td>
   <td>Anger x-koordinaten för behållarens fästpunkt i förhållande till det övre vänstra hörnet i den överordnade behållaren när den placeras med placerad layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>y</td>
   <td>Anger y-koordinaten för en behållares fästpunkt i förhållande till det övre vänstra hörnet i den överordnade behållaren när den placeras med placerad layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>bildtext</td>
   <td>Bildtextobjektet beskriver en beskrivande etikett som är associerad med ett formulärdesignobjekt.<br /> </td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>validera</td>
   <td>Objektet validate kontrollerar valideringen av användardata i ett formulär. Objektet validate kan aktiveras flera gånger under ett formulärs livstid.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>parentSubform</td>
   <td>Anger det överordnade delformuläret (sidan) för det här fältet.</td>
   <td>Returnerar alltid det överordnade delformuläret i stället för att returnera det första icke-omfångsbaserade överordnade delformuläret.<br /> </td>
  </tr>
  <tr>
   <td>selectedIndex</td>
   <td>Indexvärdet för det första markerade objektet.</td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

## Formulär {#form}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| formNodes | Returnerar en lista med alla formulärmodellobjekt som är bundna till ett angivet dataobjekt. |  |

## InstanceManager {#instancemanager}

| Egenskap | Beskrivning |
|---|---|
| `name` | En identifierare som används för att identifiera elementet i skriptuttryck. |
| `occur` | Beskriver begränsningarna över antalet tillåtna instanser för den omslutande behållaren. |
| `min` | Anger det minsta antalet instanser som kan instansieras. |
| `max` | Anger maximalt antal instanser som kan instansieras. |
| `count` | Anger det aktuella antalet instanser som instansierats. |
| `setInstances` | Lägger till eller tar bort de angivna delformulären eller delformulärsuppsättningarna från den här noden. |
| `addInstance` | Lägger till en ny instans av ett delformulär eller en delformulärsuppsättning till den här noden. |
| `removeInstance` | Tar bort ett delformulär eller en delformulärsuppsättning från den här noden. |
| `moveInstance` | Flyttar ett underordnat objekt för ett formulärmodellobjekt till en annan angiven plats i formulärmodellen. Motsvarande datamodellinformation för objektet flyttas också inom datamodellen. |
| `insertInstance` | Infogar en ny instans av ett delformulär eller en delformulärsuppsättning på den här noden. |

## list {#list}

| Egenskap | Beskrivning |
|---|---|
| `length` | Antalet element i listan. |
| `item` | Ett nollbaserat index i samlingen. |
| `append` | Lägger till en nod i slutet av nodlistan. |
| `remove` | Tar bort en nod från nodlistan. |
| `insert` | Infogar en nod före en viss nod i nodlistan. |

## nod {#node}

| Egenskap | Beskrivning | Undantag |
|---|---|---|
| createNode | Skapar en ny nod baserat på ett giltigt klassnamn. | Ingen |
| `isContainer` | Anger om det här objektet är ett behållarobjekt. | Ingen |
| `isNull` | Anger om det aktuella datavärdet är ett null-värde. | Ingen |
| `resolveNode` | Utvärderar det angivna SOM-uttrycket, med början med det aktuella XML-formulärobjektmodellobjektet, och returnerar värdet för objektet som anges i SOM-uttrycket. | Ingen |
| `resolveNodes` | Utvärderar det angivna SOM-uttrycket, med början med det aktuella XML-formulärobjektmodellobjektet, och returnerar värdet för objektet som anges i SOM-uttrycket. | Ingen |
| oneOfChild | Skapar en ny nod baserat på ett giltigt klassnamn. | Ingen |
| getElement | Returnerar ett angivet underordnat objekt. | Ingen |
| getAttribute | Hämtar ett angivet egenskapsvärde. | Ingen |
| setAttribute | Anger värdet för en angiven egenskap. | Ingen |

## modell {#model}

| Egenskap | Beskrivning | Undantag |
|---|---|---|
| NA | NA | NA |

## Delformulär {#subform}

<table>
 <tbody>
  <tr>
   <th>Egenskap</th>
   <th>Beskrivning</th>
   <th>Undantag</th>
  </tr>
  <tr>
   <td>instanceIndex</td>
   <td>Anger objektets index i förhållande till de andra instansierade instanserna.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>execEvent</td>
   <td>Kör objektets händelseskript.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>getInvalidObjects</td>
   <td>Returnerar en lista med noder i delformuläret (inklusive) som har underkänts vid valideringstestet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>border</td>
   <td>Kantobjektet beskriver kanten runt ett objekt.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>borderColor</td>
   <td>Anger kantfärgvärdet för det här fältet. Du måste ange egenskapen border.edge.presence som synlig separat.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>h</td>
   <td>Ett mått på layoutens höjd.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>w</td>
   <td>A measurement specifying the width for the layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>x</td>
   <td>Anger x-koordinaten för behållarens fästpunkt i förhållande till det övre vänstra hörnet i den överordnade behållaren när den placeras med placerad layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>y</td>
   <td>Anger y-koordinaten för en behållares fästpunkt i förhållande till det övre vänstra hörnet i den överordnade behållaren när den placeras med placerad layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>validera</td>
   <td>Objektet validate kontrollerar valideringen av användardata i ett formulär. Objektet validate kan aktiveras flera gånger under ett formulärs livstid.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>name</td>
   <td>En identifierare som används för att identifiera elementet i skriptuttryck.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>närvaro</td>
   <td>Anger ett objekts synlighet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>åtkomst</td>
   <td>Styr användaråtkomst till innehållet i ett behållarobjekt, t.ex. ett delformulär.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>execValidate</td>
   <td>Beräknar indexvärdet för ett delformulär eller en delformuläruppsättning baserat på var det finns i förhållande till andra instanser av samma formulärobjekt.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>instanceManager</td>
   <td>InstanceManager-objektet hanterar skapande, borttagning och förflyttning av instanser av formulärmodellobjekt.<br /> </td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

### skicka {#submit}

| Egenskap | Beskrivning |
|---|---|
| target | Den URL som data skickas till. Om attributet utelämnas innebär det att XFA-bearbetningsprogrammet hämtar URI:n med hjälp av en produktspecifik teknik, t.ex. åtkomst till produktspecifik information i config-objektet. |

## träd {#tree}

<table>
 <tbody>
  <tr>
   <th>Egenskap</th>
   <th>Beskrivning</th>
   <th>Undantag</th>
  </tr>
  <tr>
   <td>noder</td>
   <td>Returnerar en lista med alla underordnade objekt för det aktuella objektet.</td>
   <td>
    <ul>
     <li>Stöds inte för xfa.nodes, desc</li>
     <li>Antalet noder som rapporteras för PDF och HTML är olika. </li>
    </ul> </td>
  </tr>
  <tr>
   <td>name</td>
   <td>Anger nodens namn.</td>
   <td>Det går inte att ange namnet med skript i HTML.</td>
  </tr>
  <tr>
   <td>parent</td>
   <td>Hämtar den överordnade noden för den här noden.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>index</td>
   <td>Returnerar positionen för den här noden i dess samling med relationsnoder som har samma namn, i samma omfattning och som underordnade.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>somExpression</td>
   <td>Hämtar SOM-uttrycket för den här noden.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>resolveNode</td>
   <td>Utvärderar det angivna SOM-uttrycket, med början med det aktuella XML-formulärobjektmodellobjektet, och returnerar värdet för objektet som anges i SOM-uttrycket.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>resolveNodes</td>
   <td>Utvärderar det angivna SOM-uttrycket, med början med det aktuella XML-formulärobjektmodellobjektet, och returnerar värdet för objektet som anges i SOM-uttrycket.</td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

## delformuläruppsättning {#subformset}

| Egenskap | Beskrivning | Undantag |
|---|---|---|
| instanceManager | InstanceManager-objektet hanterar instansskapande, borttagning och förflyttning av formulärmodellsobjekt. | Ingen |

## innehåll {#content}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| isNull | Anger om det aktuella datavärdet är null-värdet. |  |

## dataValue {#datavalue}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| isNull | Anger om det aktuella datavärdet är null-värdet. |  |

## kant {#edge}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap </strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Egenskapen color beskriver en unik färg för mönsterobjektet.</td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i Modell och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## fyllning {#fill}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Färgegenskaperna definierar en unik fyllningsfärg.</td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i Modell och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## linjär {#linear}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Egenskapen color beskriver en unik färg för en linjär övertoningsfyllning i ett formulär.</td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i Modell och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## line {#line}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>kant</td>
   <td>Kantobjektet beskriver en båge, linje eller en sida av en kant eller rektangel.<br /> </td>
   <td>Attribut som färg, ändpunkt och annat stöds inte.<br /> </td>
  </tr>
 </tbody>
</table>

## mönster {#pattern}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Egenskapen color beskriver en unik färg för mönsterobjektet. </td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i Modell och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## radiell {#radial}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Egenskapen color beskriver en unik färg för det radiella objektet</td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i Modell och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## stöppla {#stipple}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Egenskapen color beskriver en unik färg för stöpplingsobjektet.</td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i modellen och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## rita {#draw}

<table>
 <tbody>
  <tr>
   <td>Egenskap</td>
   <td>Beskrivning</td>
   <td>Undantag</td>
  </tr>
  <tr>
   <td>ui</td>
   <td>UI-objektet omsluter användargränssnittsbeskrivningen för ett formulärobjekt.<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td>bildtext</td>
   <td>Bildtextobjektet beskriver en beskrivande etikett som är kopplad till ett formulärdesignobjekt.</td>
   <td> </td>
  </tr>
  <tr>
   <td>närvaro</td>
   <td>Anger ett objekts synlighet.</td>
   <td> </td>
  </tr>
  <tr>
   <td>name</td>
   <td>Anger en identifierare som kan användas för att ange objektet eller händelsen i skriptuttryck.</td>
   <td>Det går inte att ange värdet vid körning</td>
  </tr>
  <tr>
   <td>value</td>
   <td>Värdeobjektet omsluter en enskild dataenhet.<br /> </td>
   <td> </td>
  </tr>
 </tbody>
</table>

## hörn {#corner}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>färg</td>
   <td>Egenskapen color beskriver en unik färg för hörnobjektet.</td>
   <td>
    <ul>
     <li>Det går inte att hämta standardvärdet. </li>
     <li>Ändringarna återspeglas i modellen och är tillgängliga för skript, men synkroniseras inte med HTML-element. Förändringarna återspeglas därför inte i användargränssnittet.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## checkButton {#checkbutton}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>border</td>
   <td>Kantobjektet beskriver kanten runt checkButton-objektet. </td>
   <td>Ändringarna återspeglas i modellen och är tillgängliga för skript, men synkroniseras inte med HTML-element. Ändringarna återspeglas därför inte i användargränssnittet.<br /> </td>
  </tr>
 </tbody>
</table>

## choiceList {#choicelist}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap<br /> </strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>border</td>
   <td>Kantobjektet beskriver kanten runt choiceList-objektet.</td>
   <td> </td>
  </tr>
 </tbody>
</table>

## dateTimeEdit {#datetimeedit}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| border | Kantobjektet beskriver kanten runt dateTimeEdit-objektet. |  |

## Bild {#image}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>contentType</td>
   <td>Anger innehållstypen i det refererade dokumentet, uttryckt som en MIME-typ.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>namn<br /> </td>
   <td>En identifierare som används för att identifiera elementet i skriptuttryck.</td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

## imageEdit {#imageedit}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| border | Kantobjektet beskriver kanten runt imageEdit-objektet. |  |

## numericEdit {#numericedit}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| border | Kantobjektet beskriver kanten runt ett objekt. | ingen |

## object {#object}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>className</td>
   <td>Bestämmer namnet på objektets klass.<br /> </td>
   <td>ingen</td>
  </tr>
 </tbody>
</table>

## rektangel {#rectangle}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>kant</td>
   <td>Kantobjektet beskriver en båge, linje eller en sida av en kant eller rektangel.<br /> </td>
   <td>Attribut som färg, ände med mera stöds inte.</td>
  </tr>
 </tbody>
</table>

## textEdit {#textedit}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>border</td>
   <td>Kantobjektet beskriver kanten runt ett objekt.<br /> </td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

## exklGroup {#exclgroup}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>layout</td>
   <td>Anger den layoutstrategi som ska användas av det här objektet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>border</td>
   <td>Anger kanten som omger det här fältet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>obligatoriskt</td>
   <td>Anger värdet nullTest för fältet.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>borderColor</td>
   <td>Anger kantfärgvärdet för det här fältet.En kantlinje måste definieras innan du kan ändra färgen med skript.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>borderWidth</td>
   <td>Anger fältets kantlinjebredd.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>h</td>
   <td>Ett mått på layoutens höjd.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>övergående</td>
   <td>Anger om bearbetningsprogrammet måste spara exkluderingsgruppens värde som en del av en formuläröverföring eller en sparåtgärd.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>w</td>
   <td>A measurement specifying the width for the layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>x</td>
   <td>Anger x-koordinaten för behållarens fästpunkt i förhållande till det övre vänstra hörnet i den överordnade behållaren när den placeras med placerad layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>y</td>
   <td>Anger y-koordinaten för en behållares fästpunkt i förhållande till det övre vänstra hörnet i den överordnade behållaren när den placeras med placerad layout.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>bildtext</td>
   <td>Bildtextobjektet beskriver en beskrivande etikett som är associerad med ett formulärdesignobjekt.<br /> </td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>validera</td>
   <td>Objektet validate kontrollerar valideringen av användardata i ett formulär. Objektet validate kan aktiveras flera gånger under ett formulärs livstid.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>dataNode</td>
   <td>Hämtar datanoden som en formulärnod är bunden till efter sammanfogningen.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>närvaro</td>
   <td>Anger ett objekts synlighet.</td>
   <td> </td>
  </tr>
  <tr>
   <td>åtkomst</td>
   <td>Styr användaråtkomst till innehållet i ett behållarobjekt, t.ex. ett delformulär.</td>
   <td>För enskilda objekt i undantaget returneras alltid open. </td>
  </tr>
  <tr>
   <td>name</td>
   <td>Anger en identifierare som kan användas för att ange objektet eller händelsen i skriptuttryck.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>medlemmar</td>
   <td>Ange medlemmarna i exkluderingsgruppen. </td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>selectedMember</td>
   <td>Returnerar den markerade medlemmen i en exkluderingsgrupp.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>execCalculate</td>
   <td>Kör alla skript i händelsen calculate för det angivna objektet och eventuella underordnade objekt.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>beräkna</td>
   <td>Beräkningsobjektet styr beräkningen av ett fälts värde.<br /> </td>
   <td>Ingen</td>
  </tr>
 </tbody>
</table>

## arc {#arc}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap<strong></strong></strong></td>
   <td><strong>Beskrivning<strong></strong></strong></td>
   <td><strong>Undantag<strong></strong></strong></td>
  </tr>
  <tr>
   <td>kant</td>
   <td>Kantobjektet beskriver en båge, linje eller en sida av en kant eller rektangel.<br /> </td>
   <td>Attribut som färg, ände med mera stöds inte. </td>
  </tr>
 </tbody>
</table>

## border {#border}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap<strong></strong></strong></td>
   <td><strong>Beskrivning<strong></strong></strong></td>
   <td><strong>Undantag<strong></strong></strong></td>
  </tr>
  <tr>
   <td>kant</td>
   <td>Kantobjektet beskriver en båge, linje eller en sida av en kant eller rektangel.<br /> </td>
   <td>Attribut som färg, ände med mera stöds inte. </td>
  </tr>
 </tbody>
</table>

## $layout {#layout}

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Undantag</strong></td>
  </tr>
  <tr>
   <td>h</td>
   <td>Bestämmer höjden på ett angivet formulärdesignobjekt.<br /> </td>
   <td>
    <ul>
     <li>Höjdegenskapen (h) stöds inte för sidområdet och innehållsområdet. </li>
     <li>Parametern Offset from first content area the XFA-Form object is on stöds inte.</li>
    </ul> </td>
  </tr>
  <tr>
   <td>w</td>
   <td>Anger bredden på ett angivet formulärdesignobjekt.</td>
   <td>
    <ul>
     <li>Egenskapen Width (w) stöds inte för sidområdet och innehållsområdet. </li>
     <li>Parametern Offset from first content area the XFA-Form object is on stöds inte.</li>
    </ul> </td>
  </tr>
  <tr>
   <td>x</td>
   <td>Bestämmer x-koordinaten för ett givet formulärdesignobjekt i förhållande till dess överordnade objekt.</td>
   <td>
    <ul>
     <li>Egenskapen x-koordinat (x) stöds inte för sidområdet och innehållsområdet. </li>
     <li>Parametern Offset from first content area the XFA-Form object is on stöds inte.</li>
    </ul> </td>
  </tr>
  <tr>
   <td>y</td>
   <td>Bestämmer y-koordinaten för ett givet formulärdesignobjekt i förhållande till dess överordnade objekt.</td>
   <td>
    <ul>
     <li>Egenskapen y-koordinat (y) stöds inte för sidområdet och innehållsområdet. </li>
     <li>Parametern Offset from first content area the XFA-Form object is on stöds inte.</li>
    </ul> </td>
  </tr>
  <tr>
   <td>pagecount</td>
   <td>Anger antalet sidor i det aktuella formuläret.</td>
   <td>
    <ul>
     <li>layout.pageCount()-metoden returnerar olika värden för PDF- och HTML-formulär.</li>
     <li>Om antalet sidor minskas genom att ett objekt döljs returnerar abspagecount-metoden ett felaktigt värde.<br /> </li>
    </ul> </td>
  </tr>
  <tr>
   <td>pagcontent</td>
   <td>Hämtar typer av formulärdesignobjekt från en angiven sida i ett formulär.</td>
   <td>Ingen</td>
  </tr>
  <tr>
   <td>absPageCount</td>
   <td>Anger antalet sidor i det aktuella formuläret.</td>
   <td>
    <ul>
     <li>layout.pageCount()-metoden returnerar olika värden för PDF- och HTML-formulär.</li>
     <li>Om antalet sidor minskar genom att ett objekt döljs returnerar abspagecount-metoden ett felaktigt värde.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## objekt {#items}

| **Egenskap** | **Beskrivning** | **Undantag** |
|---|---|---|
| närvaro | Anger ett objekts synlighet. | Ingen |

## FormCalc {#formcalc}

FormCalc är ett XFA-specifikt språk för att skapa e-formulärbaserade logiska funktioner och beräkningsrötter. FormCalculation har en kraftfull uppsättning byggfunktioner.

### Funktioner som stöds i FormCalc {#formcalc-supported-functions}

### Stöd för FormCalc-uttryck {#formcalc-expression-support}

<table>
 <tbody>
  <tr>
   <td><strong>Kategori </strong></td>
   <td><strong>Beskrivning </strong></td>
   <td><strong>Exempel </strong></td>
  </tr>
  <tr>
   <td>Enkelt uttryck</td>
   <td>Lägg till, subtrahera, multiplicera, dela och parenteser</td>
   <td>(a+b)*3</td>
  </tr>
  <tr>
   <td>Variabeldeklaration</td>
   <td>Definiera en variabel</td>
   <td>var a<br /> var a=3<br /> a=3</td>
  </tr>
  <tr>
   <td>Logiskt uttryck</td>
   <td>
    <ul>
     <li>Logic (och/eller)</li>
     <li>Jämförelse (större/mindre/lika)</li>
    </ul> </td>
   <td>A eller 1<br /> 1 &lt;&gt; 2<br /> A NE B<br /> A eller 1<br /> 1 &lt;&gt; 2<br /> A NE B</td>
  </tr>
  <tr>
   <td>If-uttryck</td>
   <td><br type="_moz" /> </td>
   <td>if (a&gt;b) then 2 endif</td>
  </tr>
  <tr>
   <td>while</td>
   <td><br type="_moz" /> </td>
   <td>while (i lt 5) do i = i + 1 endwhile</td>
  </tr>
  <tr>
   <td>for</td>
   <td><br type="_moz" /> </td>
   <td>för i = 100 ned till 1 <br /> do s = s + i endfor</td>
  </tr>
  <tr>
   <td>för varje</td>
   <td><br type="_moz" /> </td>
   <td>för varje i i in (1, 2, 3) <br /> do s = s + i endfor</td>
  </tr>
  <tr>
   <td>funktionsdeklaration</td>
   <td>Definiera en anpassad funktion i FormCalc</td>
   <td>func foo(n) do var f = n endfunc</td>
  </tr>
 </tbody>
</table>

### Stöd för Acrobat API {#acrobat-api-support}

1. **Aritmetiska funktioner**

   1. Abs()
   1. Avg()
   1. Ceil()
   1. Count()
   1. Floor()
   1. Max()
   1. Min()
   1. Mod()
   1. Round()
   1. Sum()

1. **Vetenskapliga funktioner**

   1. Acos()
   1. assin()
   1. Atan()
   1. Atan2()
   1. Cos()
   1. Sin()
   1. Tan()
   1. Exp()
   1. Log()
   1. Pow()
   1. SQRT()
   1. Deg2Rad()
   1. Rad2Deg()
   1. Pi()

1. **Ekonomiska funktioner**

   1. Apr()
   1. Cterm()
   1. FV()
   1. Ipmt()
   1. Npv()
   1. Pmt()
   1. Ppmt()
   1. Pv()
   1. Rate()
   1. Term()

1. **Logiska funktioner**

   1. Choose()
   1. If()
   1. Oneof()
   1. Within()

1. **Strängfunktioner**

   1. At()
   1. Concat()
   1. Left()
   1. Len()
   1. Lower()
   1. Ltrim()
   1. Replace()
   1. Right()
   1. Rtrim()
   1. Space()
   1. Stuff()
   1. Substr()
   1. Upper()
   1. WordNum()

1. **Datum och tid**

   1. Date()
   1. num2date()
   1. DateFmt()

<table>
 <tbody>
  <tr>
   <td><strong>API</strong></td>
   <td><strong>Beskrivning</strong></td>
   <td><strong>Avvikelser</strong></td>
  </tr>
  <tr>
   <td>console.println()</td>
   <td>Detta acrobat-API dumpar utdata till JavaScript-konsolen.</td>
   <td> </td>
  </tr>
  <tr>
   <td>app.alert()</td>
   <td>Detta acrobat-API skickar ett varningsmeddelande via popup-menyn JavaScript.</td>
   <td> </td>
  </tr>
  <tr>
   <td>app.beep()</td>
   <td>Gör att systemet spelar upp ett ljud.</td>
   <td>Ingen åtgärd utförs.</td>
  </tr>
  <tr>
   <td>app.execDialog()</td>
   <td>Visar en modal dialogruta för användaren. Modala dialogrutor måste stängas av användaren innan värdprogrammet kan användas direkt igen.</td>
   <td>Ingen åtgärd utförs.<br /> </td>
  </tr>
  <tr>
   <td>app.launchURL()</td>
   <td>Startar en URL i ett webbläsarfönster.</td>
   <td> </td>
  </tr>
  <tr>
   <td>app.setInterval()</td>
   <td>Anger ett JavaScript-skript och en tidsperiod. Skriptet körs varje gång perioden förflyter. Returvärdet för den här metoden måste finnas i en JavaScript-variabel. I annat fall skräpsamlas intervallobjektet, vilket gör att klockan stoppas. Om du vill avsluta den periodiska körningen skickar du det returnerade intervallobjektet till clearInterval.</td>
   <td> </td>
  </tr>
  <tr>
   <td>app.setTimeOut()</td>
   <td>Anger ett JavaScript-skript och en tidsperiod. Skriptet körs endast en gång, efter att perioden har gått ut. Returvärdet för den här metoden måste hållas i en JavaScript-variabel. I annat fall kan timeout-objektet vara skräpsamlingar, vilket skulle få klockan att stanna. Om du vill avbryta timeout-händelsen skickar du det returnerade timeout-objektet till clearTimeOut.</td>
   <td> </td>
  </tr>
  <tr>
   <td>app.clearInterval()</td>
   <td>Avbryter ett tidigare registrerat intervall som ursprungligen angetts av metoden setInterval.</td>
   <td>I HTML5-formulär fungerar inte API korrekt.</td>
  </tr>
  <tr>
   <td>app.clearTimeOut()</td>
   <td>Avbryter ett tidigare registrerat timeout-intervall. Ett sådant intervall anges inledningsvis av setTimeOut.</td>
   <td>I HTML5-formulär fungerar inte API korrekt.<br /> </td>
  </tr>
  <tr>
   <td>app.eval()</td>
   <td>Kör ett angivet skript.</td>
   <td> </td>
  </tr>
  <tr>
   <td>app.activeDocs</td>
   <td>En array som innehåller Doc-objektet för varje aktivt dokument. Om inga dokument är aktiva returnerar activeDocs ingenting, d.v.s. det har samma beteende som d = new Array(0) i JavaScript huvuddokument.</td>
   <td>Returnerar en tom array för HTMl5-formulär.</td>
  </tr>
  <tr>
   <td>app.calculate</td>
   <td>Om true (standardvärdet) kan beräkningar utföras. Om false tillåts inte beräkningar.</td>
   <td>Alltid true för HTMl5 Forms.</td>
  </tr>
  <tr>
   <td>app.constants</td>
   <td>Ett wrapper-objekt som innehåller olika konstanta värden. För närvarande returnerar den här egenskapen ett objekt med en enda egenskap, align.</td>
   <td>HTML5-formulär returnerar ett tomt align-objekt.</td>
  </tr>
  <tr>
   <td>app.focusRect</td>
   <td>Aktiverar och inaktiverar fokusramen. Fokusramen är den bleka prickade linjen runt knappar, kryssrutor, alternativknappar och signaturer som anger att formulärfältet har tangentbordsfokus. Värdet true aktiverar fokusramen.</td>
   <td>Alltid true för HTML5-formulär.</td>
  </tr>
  <tr>
   <td>app.formsVersion</td>
   <td>Versionsnumret för visningsprogramvaran. Markera den här egenskapen för att avgöra om objekt, egenskaper eller metoder i senare versioner av programvaran är tillgängliga om du vill behålla bakåtkompatibilitet i skripten.</td>
   <td>11.001 alltid.</td>
  </tr>
  <tr>
   <td>app.language</td>
   <td>Språket för det Acrobat-visningsprogram som körs.</td>
   <td>Alltid"ENU" för HTML5-formulär.</td>
  </tr>
 </tbody>
</table>

## XFA-händelser som stöds {#supported-xfa-events}

Följande klientsidade XFA-händelser stöds:

* Initiera
* Validera
* Beräkna
* Klicka på
* Retur
* Avsluta
* Ändra
* ValidationState

>[!NOTE]
>
>HTML5-formulär återges på klientsidan (webbläsare). Använd skript på klientsidan **validate** och **calculate** i stället för serverbaserade skript.
