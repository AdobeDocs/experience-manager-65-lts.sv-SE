---
title: Utseenderamverk för adaptiva formulär och HTML5-formulär
description: Mobile Forms återger formulärmallar som HTML5-formulär. Dessa formulär använder jQuery-, Backbone.js- och Underscore.js-filer för utseendet och för att aktivera skript.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: customization
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Mobile Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 1%

---

# Utseenderamverk för adaptiva formulär och HTML5-formulär {#appearance-framework-for-adaptive-and-html-forms}

Forms (adaptiva formulär och HTML5-formulär) använder biblioteken [jQuery](https://jquery.com/), [Backbone.js](https://backbonejs.org/) och [Underscore.js](https://underscorejs.org/) för utseende och skript. Formulären använder också [jQuery-gränssnittets](https://jqueryui.com/) **widgetarkitektur** för alla interaktiva element (till exempel fält och knappar) i formuläret. Med den här arkitekturen kan formulärutvecklare använda en mängd tillgängliga jQuery-widgetar och plugin-program i Forms. Du kan också implementera formulärspecifik logik samtidigt som du hämtar in data från användare som leadDigits/trailDigits-begränsningar eller implementerar bildklausuler. Formulärutvecklare kan skapa och använda anpassade utseenden för att förbättra datainhämtningen och göra den mer användarvänlig.

Den här artikeln är avsedd för utvecklare med tillräcklig kunskap om jQuery- och jQuery-widgetar. Den ger insikt i utseenderamverket och gör det möjligt för utvecklare att skapa ett alternativt utseende för ett formulärfält.

Utseenderamverket bygger på olika alternativ, händelser (utlösare) och funktioner för att fånga upp användarinteraktioner med formuläret och svarar på modelländringar för att informera slutanvändaren. Dessutom:

* Ramverket innehåller en uppsättning alternativ för utseendet på ett fält. Dessa alternativ är nyckelvärdepar och indelade i två kategorier: gemensamma alternativ och fälttypsspecifika alternativ.
* Utseendet, som en del av kontraktet, utlöser en uppsättning händelser som enter och exit.
* Utseendet krävs för att implementera en uppsättning funktioner. Vissa funktioner är vanliga medan andra är specifika för fälttypsfunktioner.

## Vanliga alternativ {#common-options}

Här följer de angivna globala alternativen. Dessa alternativ är tillgängliga för alla fält.

<table>
 <tbody>
  <tr>
   <th>Egenskap </th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>name</td>
   <td>En identifierare som används för att ange objektet eller händelsen i skriptuttryck. Den här egenskapen anger till exempel namnet på värdprogrammet.</td>
  </tr>
  <tr>
   <td>value</td>
   <td>Faktiskt värde för fältet. </td>
  </tr>
  <tr>
   <td>displayValue</td>
   <td>Fältets värde visas. </td>
  </tr>
  <tr>
   <td>screenReaderText</td>
   <td>Skärmläsare använder det här värdet för att lägga till en berättarinformation om fältet. Formuläret innehåller värdet och du kan åsidosätta värdet.<br /> </td>
  </tr>
  <tr>
   <td>tabIndex</td>
   <td>Fältets position i formulärets tabbsekvens. Åsidosätt bara tabIndex om du vill ändra standardtabbordningen för formuläret.</td>
  </tr>
  <tr>
   <td>roll</td>
   <td>Elementets roll, till exempel Rubrik eller Tabell.</td>
  </tr>
  <tr>
   <td>height</td>
   <td>Widgetens höjd. Den anges i pixlar. </td>
  </tr>
  <tr>
   <td>width</td>
   <td>Widgetens bredd. Den anges i pixlar.</td>
  </tr>
  <tr>
   <td>åtkomst</td>
   <td>Kontroller som används för att komma åt innehållet i ett behållarobjekt, till exempel ett delformulär.</td>
  </tr>
  <tr>
   <td>paraStyles</td>
   <td>Paraegenskapen för ett XFA-element till widgeten.</td>
  </tr>
  <tr>
   <td>dir</td>
   <td>Textens riktning. Möjliga värden är ltr (vänster till höger) och rtl (höger till vänster).</td>
  </tr>
 </tbody>
</table>

Förutom dessa alternativ innehåller ramverket några andra alternativ som varierar beroende på fälttypen. Information om fältsspecifika alternativ visas nedan.

### Interaktion med formulärramverk {#interaction-with-forms-framework}

För att interagera med formulärramverket utlöser en widget vissa händelser som gör att formulärskriptet kan fungera. Om widgeten inte genererar dessa händelser fungerar inte vissa av skripten som är skrivna i formuläret för det fältet.

#### Händelser som utlöses av widget {#events-triggered-by-widget}

<table>
 <tbody>
  <tr>
   <th>Händelse </th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>XFA_ENTER_EVENT</td>
   <td>Den här händelsen utlöses när fältet är i fokus. Det gör att skriptet "enter" kan köras på fältet. Syntaxen för att utlösa händelsen är <br /> (widget)._trigger(xfalib.ut.XfaUtil.prototype.XFA_ENTER_EVENT)<br /> </td>
  </tr>
  <tr>
   <td>XFA_EXIT_EVENT</td>
   <td>Den här händelsen utlöses när användaren lämnar fältet. Det gör att motorn kan ställa in fältets värde och köra dess "exit"-skript. Syntaxen för att utlösa händelsen är <br /> (widget)._trigger(xfalib.ut.XfaUtil.prototype.XFA_EXIT_EVENT)<br /> </td>
  </tr>
  <tr>
   <td>XFA_CHANGE_EVENT</td>
   <td>Den här händelsen utlöses för att motorn ska kunna köra "change"-skriptet som skrivits i fältet. Syntaxen för att utlösa händelsen är <br /> (widget)._trigger(xfalib.ut.XfaUtil.prototype.XFA_CHANGE_EVENT)<br /> </td>
  </tr>
  <tr>
   <td>XFA_CLICK_EVENT</td>
   <td>Den här händelsen utlöses när användaren klickar på fältet. gör att motorn kan köra klickskriptet som skrivits i fältet. Syntaxen för att utlösa händelsen är <br /> (widget)._trigger(xfalib.ut.XfaUtil.prototype.XFA_CLICK_EVENT)<br /> </td>
  </tr>
 </tbody>
</table>

#### API:er som implementeras av widget {#apis-implemented-by-widget}

Utseenderamverket anropar vissa funktioner i widgeten som implementeras i de anpassade widgetarna. Widgeten måste implementera följande funktioner:

<table>
 <tbody>
  <tr>
   <th>Funktion</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>focus: function()</td>
   <td>Fokuserar på fältet.</td>
  </tr>
  <tr>
   <td>click: function()</td>
   <td>Fokuserar på fältet och anropar XFA_CLICK_EVENT.</td>
  </tr>
  <tr>
   <td><p>markError:function(errorMessage, errorType)<br /> <br /> <em>erorMessage: string </em>representing the error<br /> <em>errorType: string ("warning"/"error")</em></p> <p><strong>Obs!</strong>: Gäller endast för HTML5-formulär.</p> </td>
   <td>Skickar felmeddelande och feltyp till widgeten. Widgeten visar felet.</td>
  </tr>
  <tr>
   <td><p>clearError: function()</p> <p><strong>Obs!</strong>: Gäller endast för HTML5-formulär.</p> </td>
   <td>Anropas om felen i fältet är åtgärdade. Widgeten döljer felet.</td>
  </tr>
 </tbody>
</table>

## Alternativ som är specifika för fälttyp {#options-specific-to-type-of-field}

Alla anpassade widgetar ska följa ovanstående specifikationer. Om du vill använda funktionerna i olika fält måste widgeten följa riktlinjerna för det specifika fältet.

### TextEdit: Textfält {#textedit-text-field}

<table>
 <tbody>
  <tr>
   <th>Alternativ</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>flera</td>
   <td>True if the field supports in a newline character, else false.</td>
  </tr>
  <tr>
   <td>maxChars</td>
   <td>Maximalt antal tecken som kan anges i fältet.</td>
  </tr>
  <tr>
   <td><p>limitLengthToVisibleArea</p> <p><strong>Obs!</strong>: Gäller endast för HTML5-formulär</p> </td>
   <td>Anger textfältets beteende när textbredden överskrider bredden på widgeten.</td>
  </tr>
 </tbody>
</table>

### ChoiceList: DropDownList, ListBox {#choicelist-dropdownlist-listbox}

<table>
 <tbody>
  <tr>
   <th>Alternativ</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>värde<br /> </td>
   <td>Matris med valda värden.<br /> </td>
  </tr>
  <tr>
   <td>objekt<br /> </td>
   <td>Array med objekt som ska visas som alternativ. Varje objekt innehåller två egenskaper - <br /> save: värde att spara, visa: värde att visa.<br /> <br /> </td>
  </tr>
  <tr>
   <td><p>redigerbar</p> <p><strong>Obs!</strong>: Gäller endast för HTML5-formulär.<br /> </p> </td>
   <td>Om värdet är true aktiveras anpassad textinmatning i widgeten.<br /> </td>
  </tr>
  <tr>
   <td>displayValue<br /> </td>
   <td>Matris med värden som ska visas.<br /> </td>
  </tr>
  <tr>
   <td>multiselect<br /> </td>
   <td>True if multiple selections are allowed, else false.<br /> </td>
  </tr>
 </tbody>
</table>

#### API {#api}

<table>
 <tbody>
  <tr>
   <th>Funktion</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td><p>addItem:<em> function(itemValues)<br /> itemValues: objekt som innehåller visnings- och sparningsvärdet <br /> {sDisplayVal: &lt;displayValue&gt;, sSaveVal: &lt;save Value&gt;}</em></p> </td>
   <td>Lägger till ett objekt i listan.</td>
  </tr>
  <tr>
   <td>deleteItem<em>: function(nIndex)<br /> nIndex: index för det objekt som ska tas bort från listan <br /> </em><br /> <br /> </td>
   <td>Tar bort ett alternativ från listan.</td>
  </tr>
  <tr>
   <td>clearItems:<code> function()</code></td>
   <td>Tar bort alla alternativ från listan.</td>
  </tr>
 </tbody>
</table>

### NumericEdit: NumericField, DecimalField {#numericedit-numericfield-decimalfield}

| Alternativ | Beskrivning |
|---|---|
| dataType | Sträng som representerar fältets datatyp (heltal/decimal). |
| leadDigits | Högsta tillåtna radavståndssiffror i decimaltal. |
| fracDigits | Högsta tillåtna decimaltal. |
| noll | Strängbeteckning för noll i fältets nationella inställningar. |
| decimal | Strängbeteckning för decimal i fältets nationella inställningar. |

### CheckButton: RadioButton, CheckBox {#checkbutton-radiobutton-checkbox}

<table>
 <tbody>
  <tr>
   <th>Alternativ</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>values</td>
   <td><p>Array med värden (på/av/neutral).</p> <p>Det är en array med värden för de olika lägena för checkButton. värden[0] är värdet när läget är PÅ, värden[1] är värdet när läget är AV, <br /> värden[2] är värdet när läget är NEUTRAL. Värdearrayens längd är lika med värdet för tillståndsalternativet.<br /> </p> </td>
  </tr>
  <tr>
   <td>lägen</td>
   <td><p>Antal tillstånd som tillåts. </p> <p>Två för adaptiva formulär (På, Av) och tre för HTML5-formulär (På, Av, Neutral).</p> </td>
  </tr>
  <tr>
   <td>läge</td>
   <td><p>Elementets aktuella läge.</p> <p>Två för adaptiva formulär (På, Av) och tre för HTML5-formulär (På, Av, Neutral).</p> </td>
  </tr>
 </tbody>
</table>

### DateTimeEdit: (DateField) {#datetimeedit-datefield}

| Alternativ | Beskrivning |
|---|---|
| dagar | Lokaliserat namn på dagar för det fältet. |
| månader | Lokaliserade månadsnamn för det fältet. |
| noll | Den lokaliserade texten för talet 0. |
| clearText | Den lokaliserade texten för rensningsknappen. |
