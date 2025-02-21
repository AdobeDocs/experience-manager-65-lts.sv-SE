---
title: Skapa anpassade utseenden i HTML5-formulär
description: Du kan koppla anpassade widgetar till en Mobile Forms. Du kan utöka befintliga jQuery-widgetar eller utveckla egna widgetar.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 0%

---

# Skapa anpassade utseenden i HTML5-formulär{#create-custom-appearances-in-html-forms}

Du kan koppla anpassade widgetar till en Mobile Forms. Du kan utöka befintliga jQuery-widgetar eller utveckla egna widgetar med hjälp av utseenderamverket. XFA-motorn använder olika widgetar. Mer information finns i [Utseenderamverket för adaptiva formulär och HTML5-formulär](/help/forms/using/introduction-widgets.md).

![Ett exempel på standardwidget och anpassad widget](assets/custom-widgets.jpg)

Ett exempel på standardwidget och anpassad widget

## Integrera anpassade widgetar med HTML5-formulär {#integrating-custom-widgets-with-html-forms}

### Skapa en profil  {#create-a-profile-nbsp}

Du kan skapa en profil eller välja en befintlig profil för att lägga till en anpassad widget. Mer information om hur du skapar profiler finns i [Skapa anpassad profil](/help/forms/using/custom-profile.md).

### Skapa en widget {#create-a-widget}

HTML5-formulär innehåller en implementering av widgetramverket som kan utökas för att skapa nya widgetar. Implementeringen är en jQuery-widget *abstractWidget* som kan utökas för att skriva en ny widget. Den nya widgeten kan bara göras funktionell genom att du utökar/åsidosätter funktionerna nedan.

<table>
 <tbody>
  <tr>
   <td>Funktion/klass</td>
   <td>Beskrivning</td>
  </tr>
  <tr>
   <td>återge</td>
   <td>Återgivningsfunktionen returnerar jQuery-objektet för standardelementet för HTML i widgeten. HTML-standardelementet ska vara av fokuserbar typ. Till exempel &lt;a&gt;, &lt;input&gt; och &lt;li&gt;. Det returnerade elementet används som $userControl. Om $userControl anger begränsningen ovan fungerar funktionerna i klassen AbstractWidget som förväntat, i annat fall kräver vissa av de vanliga API:erna (focus, click) ändringar. </td>
  </tr>
  <tr>
   <td>getEventMap</td>
   <td>Returnerar en karta som konverterar HTML-händelser till XFA-händelser. <br /> {<br /> blur: XFA_EXIT_EVENT,<br /> }<br /> Det här exemplet visar att oskärpan är en HTML-händelse och att XFA_EXIT_EVENT motsvarar en XFA-händelse. </td>
  </tr>
  <tr>
   <td>getOptionsMap</td>
   <td>Returnerar en karta som ger en detaljerad beskrivning av vilken åtgärd som ska utföras när ett alternativ ändras. Nycklarna är de alternativ som finns för widgeten och värdena är de funktioner som anropas när en ändring av det alternativet upptäcks. Widgeten innehåller hanterare för alla vanliga alternativ (utom value och displayValue)</td>
  </tr>
  <tr>
   <td>getCommitValue</td>
   <td>Widgetramverket läser in funktionen när värdet för widgeten sparas i XFAModel (till exempel vid en exit-händelse för ett textField). Implementeringen ska returnera värdet som sparas i widgeten. Hanteraren har det nya värdet för alternativet.</td>
  </tr>
  <tr>
   <td>showValue</td>
   <td>Som standard visas fältets rawValue i XFA vid enter-händelse. Den här funktionen anropas för att visa rawValue för användaren. </td>
  </tr>
  <tr>
   <td>showDisplayValue</td>
   <td>Som standard visas fältets formattedValue i XFA vid exit-händelse. Den här funktionen anropas för att visa formattedValue för användaren. </td>
  </tr>
 </tbody>
</table>

Om du vill skapa en egen widget inkluderar du referenser till JavaScript-filen som innehåller åsidosatta funktioner och nyligen tillagda funktioner i den profil som skapas ovan. *sliderNumericFieldWidget* är till exempel en widget för numeriska fält. Om du vill använda widgeten i din profil i rubrikavsnittet inkluderar du följande rad:

```javascript
window.formBridge.registerConfig("widgetConfig" , widgetConfigObject);
```

### Registrera anpassad widget med XFA Scripting Engine  {#register-custom-widget-with-xfa-scripting-engine-nbsp}

När den anpassade widgetkoden är klar registrerar du widgeten med skriptmotorn genom att använda `registerConfig`API för [Form Bridge](/help/forms/using/form-bridge-apis.md). WidgetConfigObject används som indata.

```javascript
window.formBridge.registerConfig("widgetConfig",
        {
        ".<field-identifier>":"<name-of-the-widget>"
        }
    );
```

#### widgetConfigObject {#widgetconfigobject}

Widgetkonfigurationen tillhandahålls som ett JSON-objekt (en samling nyckelvärdepar) där nyckeln identifierar fälten och värdet representerar widgeten som ska användas med dessa fält. En exempelkonfiguration ser ut så här:

```
*{*

*"identifier1" : "customwidgetname",
"identifier2" : "customwidgetname2",
..
}*
```

där &quot;identifier&quot; är en jQuery CSS-väljare som representerar ett visst fält, en uppsättning fält av en viss typ eller alla fält. I följande lista visas identifierarens värde i olika fall:

| Typ av identifierare | Identifierare | Beskrivning |
|---|---|---|
| Särskilt fält med namn fältnamn | Identifierare:&quot;div.fieldName&quot; | Alla fält med namnet&quot;fältnamn&quot; återges med widgeten. |
| Alla fält av typen&quot;type&quot; (där typen är NumericField, DateField och så vidare).: | Identifierare: &quot;div.type&quot; | För Timefield och DateTimeField är typen textfält eftersom dessa fält inte stöds. |
| Alla fält | Identifierare: &quot;div.field&quot; |  |
