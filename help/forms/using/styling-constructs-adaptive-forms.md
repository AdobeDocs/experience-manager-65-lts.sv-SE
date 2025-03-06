---
title: Formateringskonstruktioner för adaptiva formulär
description: Använd LESS-ramverket för att anpassa utseendet på adaptiva formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 0ecfd918-2c68-4085-8c6e-440499b84fc1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2352'
ht-degree: 0%

---

# Formateringskonstruktioner för adaptiva formulär{#styling-constructs-for-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Förutsättningar {#prerequisites}

Kunskap om CSS och LESS Framework.

## Vad kan anpassas {#what-can-be-customized}

I artikeln listas allmänt tillgängliga CSS-klasser för adaptiva formulär. Du kan använda dessa klasser för att formatera olika komponenter i ett anpassat formulär. Formateringen av redigeringskomponenter, t.ex. dialogrutor och statusfält som visar varningar, ligger utanför artikelns omfång. Använd dessa formateringskonstruktioner om du bara vill skapa format (med CSS eller Less) när du inte kan formatera komponenter med [temaredigeraren](https://helpx.adobe.com/experience-manager/6-3/forms/using/themes.html).

## Anpassa format i anpassningsbara formulär {#customizing-styles-in-adaptive-forms}

Med LESS-ramverket blir det enklare att anpassa format i anpassningsbara formulär. I ramverket kan du definiera format med hjälp av en uppsättning variabler och funktioner (mixins). LESS-ramverket hjälper till att minska storleken på den paketerade koden och ökar dess återanvändbarhet.

Du kan anpassa anpassade formulärformat på följande sätt:

* Ändra temat
* Ändra komponentens stil

## Ändra tema {#changing-theme}

Du kan ändra temat för ett anpassat formulär för att se till att det ser ut som på de webbsidor där det anpassade formuläret är inbäddat.

Ändringar av det övergripande utseendet på det adaptiva formuläret med hjälp av CSS-egenskaper är vanligtvis en del av temaförändringar. Stora förändringar av logotypen&quot;ok och känsla i den adaptiva formen, t.ex. ändringar av komponenternas layout och placering, betraktas inte som temaförändringar.

Baserat på Bootstrap definierar följande uppsättning CSS-egenskaper temat för en webbsida:

* Bakgrundsfärg
* Kant (text, färg, tjocklek)
* Teckenfärg
* Utfyllnad
* Marginal
* Teckenstorlek
* LineHeight

För närvarande definieras LESS-variabler bara för dessa egenskaper för de olika elementen i en adaptiv form.

## Ändra komponentstil {#changing-component-style}

Du kan ändra utseende, layout, placering och synlighet för element. För att utföra den här uppgiften skapar eller uppdaterar du dina anpassade css-filer så att de innehåller de formateringskonstruktioner som listas i den här artikeln.

Om du vill använda ett format på ett anpassat formulär öppnar du det adaptiva formuläret i för redigering, öppnar egenskaper för adaptiv formulärbehållare och anger sökvägen till en anpassad CSS-fil på fliken Grundläggande. Standardformateringskonstruktioner för det adaptiva formuläret och åsidosätts av de konstruktioner som anges i den anpassade css-filen.

## Komponenter {#components}

Komponenterna som beskrivs i den här artikeln har sina fördefinierade CSS-klasser. Du kan redigera variablerna om du vill ändra formaten i CSS-klasserna. Du kan också skriva om hela klassen. I det här avsnittet beskrivs klasserna i komponenter och format som du kan ändra med variabler.

## Behållarformat {#container-styling}

En behållare är komponenten på den översta nivån. Andra paneler och fält ligger under behållarkomponenten.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guideContainerNode</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabelbeskrivning</strong></p> </td>
   <td><p><strong>Variabelbeskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>container-bgColor</code></p> </td>
   <td><p>Behållarens bakgrundsfärg</p> </td>
  </tr>
  <tr>
   <td><p><code>container-padding</code></p> </td>
   <td><p>Utfyllnad för behållaren</p> </td>
  </tr>
  <tr>
   <td><p><code>container-margin</code></p> </td>
   <td><p>Behållarens marginal</p> </td>
  </tr>
  <tr>
   <td><p><code>container-fontColor</code></p> </td>
   <td><p>Teckenfärg för behållaren</p> </td>
  </tr>
 </tbody>
</table>

## Fältformat {#field-styling}

Anpassningsbara formulär innehåller olika typer av fält. Varje fält har ett unikt klassnamn, som är fältets namn. Fältet har också ett vanligt klassnamn `guideFieldNode`.

Fälten innehåller etiketter, widgetar, hjälpbeskrivning (både lång och kort beskrivning) och hjälpikoner (frågetecken).

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guideFieldNode</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>field-padding</code><strong></strong></p> </td>
   <td><p>Utfyllnad för fältet</p> </td>
  </tr>
  <tr>
   <td><p><code>field-error-font-color</code></p> </td>
   <td><p>Teckenfärg för felmeddelandet i fältet</p> </td>
  </tr>
  <tr>
   <td><p><code>field-error-font-size</code></p> </td>
   <td><p>Teckensnittsstorlek för felmeddelandet i fältet</p> </td>
  </tr>
 </tbody>
</table>

## Etikettformat {#label-styling}

HTML-elementet **label** som används för fältet innehåller klasserna **left** eller **top** beroende på om etiketten finns högst upp eller till vänster.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guideFieldLabel</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>label-font-color</code></p> </td>
   <td><p>Teckenfärg för fältetiketten</p> </td>
  </tr>
  <tr>
   <td><p><code>label-font-size</code></p> </td>
   <td><p>Teckensnittsstorlek för fältetiketten</p> </td>
  </tr>
  <tr>
   <td><p><code>label-line-height</code></p> </td>
   <td>Egenskapen CSS-radhöjd för fältetiketten </td>
  </tr>
  <tr>
   <td><p><code>label-font-weight</code></p> </td>
   <td>Egenskapen CSS-teckensnittsvikt för fältetiketten </td>
  </tr>
  <tr>
   <td><p><code>label-margin</code></p> </td>
   <td><p>Marginal för fältetiketten</p> </td>
  </tr>
 </tbody>
</table>

CSS-reglerna för etiketten tillämpas med etiketten **guideFieldLabel** . Om du är författare åsidosätter du den här regeln för att göra dina anpassade ändringar synliga.

## Widgets-format {#widgets-styling}

Beroende på typ innehåller widgetar även klasser. Vanligtvis innehåller widgetar klassen `guideFieldWidget`. De widgetar som levereras med HTML använder normalt HTML standardelementindata och -markeringar. Formateringen görs därefter. Du kan inte formatera en anpassad widget genom att ändra variablerna.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guideFieldWidget</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel <code></code></strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-bg-color</code></p> </td>
   <td>Bakgrundsfärg för widgetar (fungerar inte för kryssrutor och alternativknappar)</td>
  </tr>
  <tr>
   <td><p><code>widgets-border-color</code></p> </td>
   <td><p>Kantfärg för widgetar</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-border-thickness</code></p> </td>
   <td><p>Kantstorlek för widgetar</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-border-radius</code></p> </td>
   <td><p>Kantradie för widgetar</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-border-type</code></p> </td>
   <td><p>Kanttyp för widgetar</p> </td>
  </tr>
  <tr>
   <td><p><code>widget-border-focus-type</code></p> </td>
   <td><p>Fokusera typ för widgetkanter</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-border</code></p> </td>
   <td><p>Konsoliderad kantlinjestil för widgetar</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-font-color</code></p> </td>
   <td><p>Textens färg i widgeten</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-font-size</code></p> </td>
   <td><p>Storlek på texten i widgeten</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-line-height</code></p> </td>
   <td>CSS-egenskapen lineheight för widgeten </td>
  </tr>
  <tr>
   <td><p><code>widgets-padding</code></p> </td>
   <td><p>CSS-utfyllnadsegenskap för widgeten</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-focus-border-color</code></p> </td>
   <td><p>Kantfärg när widgeten är i fokus</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-mandatory-border-color</code></p> </td>
   <td><p>Kantfärg för widgeten för de obligatoriska fälten</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-mandatory-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för widgeten för de obligatoriska fälten</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-disabled-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för widgeten när fältet är inaktiverat</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-disabled-font-color</code></p> </td>
   <td><p>Teckenfärg för widgeten när fältet är inaktiverat</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-disabled-border-color</code></p> </td>
   <td><p>Kantfärg för widgeten när fältet är inaktiverat</p> </td>
  </tr>
  <tr>
   <td><p><code>widget-height</code></p> </td>
   <td>Widgetens höjd (fungerar inte för kryssrutor och alternativknappar)</td>
  </tr>
  <tr>
   <td><p><code>checkbutton-height</code></p> </td>
   <td><p>Höjd för kryssruta och alternativknapp.</p> </td>
  </tr>
  <tr>
   <td><p><code>listboxwidget-height</code></p> </td>
   <td><p>Maximal höjd för en flervalslistruta</p> </td>
  </tr>
 </tbody>
</table>

### Begränsningar i widgetformat {#limitations-in-widget-styling}

Formateringen för fokuserade, obligatoriska och inaktiverade fält är begränsad med hjälp av variabler. Du kan dock ändra den genom att åsidosätta formaten. Begränsningar med hjälp av variabler anges huvudsakligen för att hålla antalet variabler under kontroll. Begränsningen kan förenklas om utseendet på ett fält ändras drastiskt eftersom det är i något av de lägen som diskuterades tidigare.

## Hjälpbeskrivning {#help-description}

En författare kan ange hjälpinnehåll i fälten med hjälp av komponenterna för kort och lång beskrivning. Båda komponenterna har en gemensam klass, `.guideHelpDescription`, och en annan klass, `.long`/ `.short`, beroende på typen av beskrivning. Hjälpinnehållet omsluts av ett styckeelement för att åsidosätta formateringen av beskrivningen. Hjälpbeskrivningen (både lång och kort) ändras med variabler som börjar med widgetshelp, vilket anges i följande tabell:

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-long-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för widgetens långa hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-long-border-color</code></p> </td>
   <td><p>Kantfärg för widgetens långa hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-long-border-indicator-color</code></p> </td>
   <td><p>Den vänstra indikatorns kantlinjefärg för widgetens långa hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-short-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för widgetens korta hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-short-color</code></p> </td>
   <td><p>Teckenfärg för widgetens korta hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-short-tooltip-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för widgetens korta verktygstips Hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>widgets-help-short-tooltip-color</code></p> </td>
   <td><p>Teckenfärg för widgetens korta verktygstips Hjälp</p> </td>
  </tr>
 </tbody>
</table>

## Villkor {#terms-and-conditions}

Med widgeten Villkor (TnC `` ``) kan du ange villkor. Du kan anpassa widgeten med hjälp av de variabler som beskrivs i följande tabell.

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><code>tnc-unvisited</code></td>
   <td>Teckensnittsfärg för obesökt kontaktlänk.</td>
  </tr>
  <tr>
   <td><code>tnc-visited</code></td>
   <td>Teckensnittsfärg för den besökta kontaktlänken.</td>
  </tr>
 </tbody>
</table>

## Knapp {#button}

Knappar är också widgetar. Men deras format skiljer sig något från widgetarna. I anpassningsbara former utgör något av följande en knapp:

* indata[type = text]
* knapp
* element med klass .button

HTML-kod för knapp:

`<button type="button" >`

`<span class="iconButtonicon"></`

`span>`

`<span class="iconButtonlabel"></`

`span>`

`</button>`

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS-klass</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>iconButton-icon</code></p> </td>
   <td><p>Visar ikoner för knappen</p> </td>
  </tr>
  <tr>
   <td><p><code>iconButton-label</code></p> </td>
   <td><p>Etikett/bildtext för knappknappen Stilar</p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel <code></code></strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>button-border-size</code></p> </td>
   <td><p>Kantstorlek för knapparna</p> </td>
  </tr>
  <tr>
   <td><p><code>button-border-type</code></p> </td>
   <td><p>Kanttyp</p> </td>
  </tr>
  <tr>
   <td><p><code>button-padding</code></p> </td>
   <td><p>CSS-utfyllnadsegenskap för knappen</p> </td>
  </tr>
  <tr>
   <td><p><code>button-font-size</code></p> </td>
   <td><p>Knappens teckensnittsstorlek</p> </td>
  </tr>
  <tr>
   <td><p><code>button-background-color</code></p> </td>
   <td><p>Bakgrundsfärg för knappen</p> </td>
  </tr>
  <tr>
   <td><p><code>button-font-color</code></p> </td>
   <td><p>Knappens teckensnittsfärg</p> </td>
  </tr>
  <tr>
   <td><p><code>button-border-color</code></p> </td>
   <td><p>Kantfärg för knappen</p> </td>
  </tr>
  <tr>
   <td><p><code>button-large-padding</code></p> </td>
   <td><p>Utfyllnad för stora knappar (knappar med klassen .buttonLarge)</p> </td>
  </tr>
  <tr>
   <td><p><code>button-large-font-size</code></p> </td>
   <td><p>Teckenstorlek för stora knappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-small-padding</code></p> </td>
   <td><p>Utfyllnad för små knappar (knappar med klassen .buttonSmall)</p> </td>
  </tr>
  <tr>
   <td><p><code>button-small-font-size</code></p> </td>
   <td><p>Teckenstorlek för små knappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-info-background-color</code></p> </td>
   <td><p>Bakgrundsfärg för informativa knappar (knappar med klassen .buttoninformative)</p> </td>
  </tr>
  <tr>
   <td><p><code>button-info-font-color</code></p> </td>
   <td><p>Teckenfärg för informativa knappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-info-border-color</code></p> </td>
   <td><p>Kantfärg för informativa knappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-warning-background-color</code></p> </td>
   <td><p>Bakgrundsfärg för varningsknappar (knappar med klassen .buttonWarning)</p> </td>
  </tr>
  <tr>
   <td><p><code>button-warning-font-color</code></p> </td>
   <td><p>Teckenfärg för varningsknappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-warning-border-color</code></p> </td>
   <td><p>Kantfärg för varningsknappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-alert-background-color</code></p> </td>
   <td><p>Bakgrundsfärg för varningsknappar (knappar med klassen .buttonalert)</p> </td>
  </tr>
  <tr>
   <td><p><code>button-alert-font-color</code></p> </td>
   <td><p>Teckenfärg för varningsknappar</p> </td>
  </tr>
  <tr>
   <td><p><code>button-alert-border-color</code></p> </td>
   <td><p>Kantfärg för varningsknappar</p> </td>
  </tr>
 </tbody>
</table>

## Frågetecken {#question-mark}

För widgetarna visas ett questionMark när en författare lägger till en lång beskrivning i hjälpinnehållet. Standardikonen som finns i Bootstrap används. Om du vill använda en anpassad ikon kan du anpassa bootstrap-ikonerna.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guideHelpQuestionMark</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>questionmark-font-color</code></p> </td>
   <td><p>Ikonens färg</p> </td>
  </tr>
  <tr>
   <td><p><code>questionmark-hover-font-color</code></p> </td>
   <td><p>Färg på ikonen när muspekaren förs över den</p> </td>
  </tr>
 </tbody>
</table>

## Tabell {#table}

Du kan ändra färgtemat för huvud- och innehållsrader i en tabell med hjälp av följande variabler.

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>table-header-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för rubrikraden. Standardvärdet är <code>#333</code>.<br /> </p> </td>
  </tr>
  <tr>
   <td><p><code>table-odd-row-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för den udda innehållsraden. Standardvärdet är <code>rgb(255, 255, 255)</code>.</p> </td>
  </tr>
  <tr>
   <td><p><code>table-even-row-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för den jämna innehållsraden. Standardvärdet är <code>#eee</code>.</p> </td>
  </tr>
 </tbody>
</table>

## Bifogad fil {#file-attachment}

Med widgeten Bifogad fil i adaptiva formulär kan du överföra filer. Du kan också anpassa widgeten med hjälp av variablerna.

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>fileItemPadding</code></p> </td>
   <td><p>Utfyllnad för de filer som visas i widgeten</p> </td>
  </tr>
  <tr>
   <td><p><code>fileItemBackground</code></p> </td>
   <td><p>Bakgrundsfärg för filobjektet</p> </td>
  </tr>
  <tr>
   <td><p><code>fileItemBorderColor</code></p> </td>
   <td><p>Kantfärg för den övre kanten</p> </td>
  </tr>
  <tr>
   <td><p><code>fileItemColor</code></p> </td>
   <td><p>Teckenfärg för filobjektet</p> </td>
  </tr>
  <tr>
   <td><p><code>filePreviewIconColor</code></p> </td>
   <td><p>Färg för ikonen Förhandsgranska (Bootstrap-ikon) i widgeten</p> </td>
  </tr>
  <tr>
   <td><p><code>fileItemCommentHeight</code></p> </td>
   <td><p>Höjd på kommentar för filobjektet</p> </td>
  </tr>
 </tbody>
</table>

## Navigeringsformat {#navigator-styles}

Det finns fyra typer av navigeringsflikar. Det finns flikar till vänster, högst upp i guiden och dragspelet. Varje navigator har en egen klass.

<table>
 <tbody>
  <tr>
   <td><p><strong>Navigator</strong></p> </td>
   <td><p><strong>CSS, klass</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>Accordion</code></p> </td>
   <td><p>.accordion-navigators</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs on the left</code></p> </td>
   <td><p>.tab-navigators-vertical</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs on the top</code></p> </td>
   <td><p>.tab-navigators</p> </td>
  </tr>
  <tr>
   <td><p><code>Wizard</code></p> </td>
   <td><p>.wizard-navigators</p> </td>
  </tr>
 </tbody>
</table>

Nedan följer HTML-koden för tabbnavigerarelementet (liknar bootstrap-flikarna):

`<li>`

`<a>tab title</a>`

`</li>`

`Accordion navigator is an exception, it has following barebone`

`structure:`

`<div class="accordion.navigators">`

`<div>`

`<div class = "guideHeader">`

`<a>`

`<span class = "guideSummary" ></code>`

`........................... repeatable buttons, if the repeatable configuration is set ................................`

`<div class = "repeatableButtons">`

`<button name="Add" class="Add"/>`

`<button name="Remove" class="Remove"/>`

`</div>`

`</a>`

`</div>`

`................................ panel content ..................................`

`</div>`

`</div>`

Du kan ändra navigatorns format med CSS-regler som markerar elementen med **underordnade** väljare. Så här lägger du till ett textdekorationsformat till ankartaggen:

Fliknavigator överst:

`.tab-navigators`

`li a {`

`text-decoration:`

`underline;`

`}`

`Tab navigator on left:`

`.tab-navigators-vertical`

`li a {`

`text-decoration:`

`underline;`

`}`

`Accordion navigator:`

`.accordion-navigators .guideHeader a .guideSummary {`

`text-decoration:`

`underline;`

`}`

`Wizard navigator:`

`.wizard-navigators`

`li a {`

`text-decoration:`

`underline;`

`}`

Det finns dessutom klasser för att formatera tabbnavigatorer (både vänster och överst) baserat på om de har kapslade/underordnade/underordnade navigatorer.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS-klass</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>nested_true</code></p> </td>
   <td><p>Tabbnavigatörer (vänster och överst) med kapslade/underordnade/underordnade navigatorer</p> </td>
  </tr>
  <tr>
   <td><p><code>nested_false</code></p> </td>
   <td><p>Tabbnavigatörer (vänster och överst) som inte har kapslade/underordnade/underordnade navigatorer</p> </td>
  </tr>
 </tbody>
</table>

Klassen guideNavIcon innehåller en standardikon för tabbnavigering (både vänster och överst) och guidenavigatorer.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guideNavIcon</code></p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Du kan ändra ikonen för en viss navigator genom att ange en CSS-klass på panelen vid redigeringen, till exempel &lt;CLASS_NAME>. Du lägger till **&lt;CLASS_NAME>_nav** som ikon för navigatorn.

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><strong>Fliknavigatörer</strong></p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p><code>navigator-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för hela fliknavigatorn</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för fliken</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-font-color</code></p> </td>
   <td><p>Teckenfärg för fliken</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-hover-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för fliken vid hovring</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-hover-font-color</code></p> </td>
   <td><p>Teckenfärg för fliken vid hovring</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-active-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg när panelen är i fokus (aktiv)</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-active-font-color</code></p> </td>
   <td><p>Teckenfärg när panelen är i fokus</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-completed-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg när panelens slutförandeuttryck returnerar true</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-completed-font-color</code></p> </td>
   <td><p>Teckenfärg när panelens slutförandeuttryck returnerar true</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-stepped-bg-color</code></p> </td>
   <td>Bakgrundsfärg när panelen har fokus en gång men slutförandeuttrycket returnerar false </td>
  </tr>
  <tr>
   <td><p><code>tabs-stepped-font-color</code></p> </td>
   <td>Teckenfärg när panelen har fokus en gång men slutförandeuttrycket returnerar false </td>
  </tr>
  <tr>
   <td><p><code>tabs-border-color</code></p> </td>
   <td><p>Kantfärg för fliken</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-font-size</code></p> </td>
   <td><p>Teckenstorlek för fliken</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-padding</code></p> </td>
   <td><p>Utfyllnad för fliken</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-margin</code></p> </td>
   <td><p>Marginal för fliken</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-vertical-margin</code></p> </td>
   <td><p>Marginal för de lodräta flikarna</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-border-thickness</code></p> </td>
   <td><p>Kantstorlek för flikarna</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-min-height</code></p> </td>
   <td><p>Flikarnas minsta höjd</p> </td>
  </tr>
  <tr>
   <td><p><code>heirarichal-indent</code></p> </td>
   <td><p>Indrag för kapslade flikar</p> </td>
  </tr>
  <tr>
   <td><p><strong>Guidenavigatörer</strong></p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-navigator-bg-color</code></p> </td>
   <td>Bakgrundsfärg för hela guidenavigatorn</td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för guiden</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-font-color</code></p> </td>
   <td><p>Teckenfärg för guiden</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-active-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg när panelen är i fokus (aktiv)</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-active-font-color</code></p> </td>
   <td><p>Teckenfärg när panelen är i fokus (fokuserad)</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-completed-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg när panelens slutförandeuttryck returnerar true</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-completed-font-color</code></p> </td>
   <td><p>Teckenfärg när panelens slutförandeuttryck returnerar true</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-stepped-bg-color</code></p> </td>
   <td>Bakgrundsfärg när panelen har fokus en gång men slutförandeuttrycket returnerar false</td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-stepped-font-color</code></p> </td>
   <td><p>Teckenfärg när panelen har fokuserats en gång men slutförandeuttrycket returnerar false</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-border-color</code></p> </td>
   <td><p>Färg för guiden</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-font-size</code></p> </td>
   <td><p>Teckensnittsstorlek för guiden</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-padding</code></p> </td>
   <td><p>Utfyllnad för guiden</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-tabs-border-thickness</code></p> </td>
   <td><p>Kantstorlek för guiden</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-nav-bullet-border</code></p> </td>
   <td><p>Kantfärg för guideens navigerarpunkt (prefixera bildtexten/etiketten)</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-progress-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg i guidenavigerarens förloppsindikator</p> </td>
  </tr>
  <tr>
   <td><p><code>wizard-progress-color</code></p> </td>
   <td><p>Fyllningsfärg för förloppsindikatorn</p> </td>
  </tr>
  <tr>
   <td><p><strong>Dragspelsnavigatörer</strong></p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p><code>accordion-tabs-padding</code></p> </td>
   <td><p>Utfyllnad för dragspel</p> </td>
  </tr>
 </tbody>
</table>

## Panelformat {#panel-styling}

En panel innehåller ett valfritt verktygsfält och dess innehåll.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guidePanelNode</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>panel-background-color</code></p> </td>
   <td><p>Bakgrundsfärg för panelen</p> </td>
  </tr>
  <tr>
   <td><p><code>panel-font-size</code></p> </td>
   <td><p>Teckensnittsstorlek för paneltexten</p> </td>
  </tr>
  <tr>
   <td><p><code>panel-font-color</code></p> </td>
   <td><p>Teckenfärg för paneltexten <br /> </p> </td>
  </tr>
  <tr>
   <td><p><code>panel-padding</code></p> </td>
   <td><p>Utfyllnad på panelen</p> </td>
  </tr>
  <tr>
   <td><p><code>panel-description-font-size</code></p> </td>
   <td><p>Teckensnittsstorlek för panelbeskrivningen</p> </td>
  </tr>
  <tr>
   <td><p><code>panel-description-padding</code></p> </td>
   <td><p>Utfyllnad av panelens beskrivning</p> </td>
  </tr>
  <tr>
   <td><p><code>panel-help-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för panelens hjälp</p> </td>
  </tr>
  <tr>
   <td><p><code>panel-help-border-indicator-color</code></p> </td>
   <td><p>Indikatorkantsfärg för panelhjälpen</p> </td>
  </tr>
 </tbody>
</table>

Panelnoden är uppdelad i navigatorer och innehåll. Det finns ingen separat formatkomponent för innehållet i `` ``. Variablerna som beskrivs tillämpas på navigatorn och innehållet.

Den översta panelen (RootPanel) har inte den här klassen.

## Mobilformat {#mobile-styling}

## Sidhuvudsfält {#header-bar}

Dessa variabler påverkar den rubrikrad som är synlig på en mobil enhet eller små skärmar som innehåller panelrubriker och navigeringsknappar för nästa och bakre.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>guide-header-bar</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>headerbar-background-color</code></p> </td>
   <td><p>Bakgrundsfärg för rubrikfältet</p> </td>
  </tr>
  <tr>
   <td><p><code>headerbar-font-color</code></p> </td>
   <td><p>Teckensnittsfärg för texten i rubrikfältet</p> </td>
  </tr>
  <tr>
   <td><p><code>headerbar-padding</code></p> </td>
   <td><p>Utfyllnad för rubrikfält</p> </td>
  </tr>
 </tbody>
</table>

## Rullningsindikator {#scroll-indicator}

Dessa variabler påverkar rullningsindikatorn, som är en orange pil som visas på en mobil enhet eller små skärmar. En rullningsindikator anger att det finns innehåll utanför den synliga delen av skärmen. Du kan rulla nedåt för att se den. När du klickar på slutet av innehållet försvinner pilen.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>mobileScrollIndicator</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>scrollIndicatorBottom</code></p> </td>
   <td><p>Rullningsindikatorns fasta position nedifrån</p> </td>
  </tr>
  <tr>
   <td><p><code>scrollIndicatorRight</code></p> </td>
   <td><p>Rullningsindikatorns fasta position från höger</p> </td>
  </tr>
  <tr>
   <td><p><code>scrollIndicatorWidth</code></p> </td>
   <td><p>Bredd på rullningsvisare</p> </td>
  </tr>
  <tr>
   <td><p><code>scrollIndicatorHeight</code></p> </td>
   <td><p>Höjd på rullningsindikator</p> </td>
  </tr>
 </tbody>
</table>

## Mobile fixed toolbar layout-specific variables {#mobile-fixed-toolbar-layout-specific-variables}

Variablerna i följande tabell påverkar den fasta verktygsfältslayouten för mobila enheter.

<table>
 <tbody>
  <tr>
   <td><p><strong>CSS, klass </strong></p> </td>
   <td><p><code>mobileToolbar</code></p> </td>
  </tr>
 </tbody>
</table>

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>mobileToolbarBottom</code></p> </td>
   <td><p>Verktygsfältets position, på den mobila enheten, från nederkanten är fast</p> </td>
  </tr>
  <tr>
   <td><p><code>mobileToolbarTop</code></p> </td>
   <td><p>Verktygsfältets position, på den mobila enheten, från överkanten är fast</p> </td>
  </tr>
  <tr>
   <td><p><code>mobileToolbarLeft</code></p> </td>
   <td><p>Verktygsfältets fasta position på den mobila enheten från vänster</p> </td>
  </tr>
  <tr>
   <td><p><code>mobileToolbarRight</code></p> </td>
   <td><p>Verktygsfältets fasta position på den mobila enheten från höger</p> </td>
  </tr>
  <tr>
   <td><p><code>mobileButtonIconTopMargin</code></p> </td>
   <td><p>Placeringen för verktygsfältets knappikon, uppifrån, har åtgärdats</p> </td>
  </tr>
  <tr>
   <td><p><code>mobileButtonIconWidth</code></p> </td>
   <td><p>Bredd på verktygsfältets knappikon på den mobila enheten</p> </td>
  </tr>
  <tr>
   <td><p><code>mobileButtonIconHeight</code></p> </td>
   <td><p>Höjd på verktygsfältets knappikon på den mobila enheten</p> </td>
  </tr>
  <tr>
   <td><p><code>mobilefixedtoolbarbgcolor</code></p> </td>
   <td><p>Bakgrundsfärg i verktygsfältet på den mobila enheten</p> </td>
  </tr>
 </tbody>
</table>

## Temaspecifik variabel {#theme-specific-variable}

Temat **Enkel registrering** på /etc/clientlibs/fd/af/guithema/simpleEnrollment och kategorin `guide.theme.simpleEnrollment` innehåller också några variabler. Om du vill skapa ett tema som förbättrar enkel registrering kan du använda följande&quot;extra variabler&quot;:

<table>
 <tbody>
  <tr>
   <td><p><strong>Variabel </strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p><code>button-focus-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för knapp i fokus</p> </td>
  </tr>
  <tr>
   <td><p><code>button-hover-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för knapp vid hovring</p> </td>
  </tr>
  <tr>
   <td><p><code>button-radius</code></p> </td>
   <td><p>Knappradie</p> </td>
  </tr>
  <tr>
   <td><p><code>navigation-button-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för navigeringsknappar (bakåt/nästa)</p> </td>
  </tr>
  <tr>
   <td><p><code>navigation-button-bg-hover-color</code></p> </td>
   <td><p>Bakgrundsfärg för navigeringsknappar (bakåt/nästa) vid hovring</p> </td>
  </tr>
  <tr>
   <td><p><code>initial-nav-color</code></p> </td>
   <td><p>Bakgrundsfärg för guidenavigatörer och motsvarande förloppsindikator, när den först återges.</p> </td>
  </tr>
  <tr>
   <td><p><code>active-nav-color</code></p> </td>
   <td>Bakgrundsfärg för aktuell/aktiv guidenavigator och motsvarande förloppsindikator </td>
  </tr>
  <tr>
   <td><p><code>visited-nav-color</code></p> </td>
   <td><p>Bakgrundsfärg för guidenavigatörer och motsvarande förloppsindikator, som har besökts.</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-bifercating-border-color</code></p> </td>
   <td><p>Kantfärgsbifurcera behållare till navigatorer och paneler</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-navigator-separator-color</code></p> </td>
   <td><p>Nedre kantfärg som separerar tabbar för tabbar till vänster (tabbnavigering).</p> </td>
  </tr>
  <tr>
   <td><p><code>tabs-child-nav-bg-color</code></p> </td>
   <td><p>Bakgrundsfärg för navigatorns kapslade/underordnade/underordnade navigatorer</p> </td>
  </tr>
 </tbody>
</table>
