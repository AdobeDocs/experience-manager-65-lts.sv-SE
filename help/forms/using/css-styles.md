---
title: Skapa CSS-format för HTML5-formulär
description: Lär dig hur du ändrar utseendet på HTML5-formulär genom att ändra CSS-klassen som är kopplad till HTML-formulärelementet.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
discoiquuid: a8d986ab-2a4c-488b-957e-4606f7391bd3
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: f7b8a37a-df4c-4db2-a0fc-dabcdbd74e48
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---

# Skapa CSS-format för HTML5-formulär {#creating-css-styles-for-html-forms}

HTML5-återgivningen av en XFA-baserad formulärmall består av flera HTML-element. Dessa element ordnas i en ordning. Alla element har väldefinierade CSS-klasser. Du kan använda dessa CSS-klasser för att markera och ändra utseendet på ett element.

>[!NOTE]
>
>I CSS-klasserna ska du inte ändra värdet för attributen width, height, border-tjocklek, top, left, right, bottom, padding, margin och other position and size. Alla ändringar i attributen position och storlek medför ändringar i formulärets layout.

## CSS-klasser  för element  {#css-classes-nbsp-for-elements-nbsp}

Alla element innehåller väldefinierade CSS-klasser. Du kan ändra dessa klasser om du vill ändra utseendet på ett element. Alla element, förutom fältet och draw-elementen, har två CSS-klasser - Type-klassen och Name-klassen.

* Klassen **Type** representerar typen av XFA-fält. Du kan åsidosätta klassen `type` om du vill ändra formaten för alla element av en viss typ.

* Klassen **Name** motsvarar namnet på XFA-fältet. Du kan åsidosätta klassen `name` om du vill ändra och använda ett anpassat format på ett element.

>[!NOTE]
>
>Vissa XFA-element har inget namn. Om du vill ändra formaten för sådana komponenter ändrar du alla komponenter av den typen.

För sidor som inte är namngivna i AEM Forms Designer namnges sidorna i ett HTML5-formulär i den ordning de numreras. För ett HTML5-formulär med två sidor heter sidorna t.ex. Page1, Page2.

## Fältelement {#field-element}

Fältelementet innehåller två kapslade element: widget och bildtext.

**Widget-element**

Widgetelementet innehåller användargränssnittselementet för interaktion med användare. Den har tre CSS-klasser:

* **Widget**: Varje widget har den här klassen.
* **namn**: Alla widgetar som levereras med AEM innehåller widgetnamnsklassen. För anpassade widgetar tillhandahåller widgetutvecklaren klassen Widget name.
* **typ**: Varje widget har ett element i användargränssnittet. Den här klassen definierar typen av användargränssnittselement.

```xml
<!--field with caption-->
<div class="field numericfield NumericField3 ">
     <div class="caption">
        caption content
     </div>
     <div class="widget numericfieldwidget numericInput">
       widget content
     </div>
</div>

<!--field without caption-->
<div class="widget numericfieldwidget numericInput">
   widget content
</div>
```

Förutom klassen type och name innehåller fältkomponenten även en ytterligare CSS-klass med namnet **subtype**. En undertyp identifierar vilken typ av fält det är, till exempel NumericField, DateField och TextField. Du kan åsidosätta undertypsklassen om du vill ändra formateringen för alla fält av typen, undertyp.

## CSS-klasser för olika komponenter {#css-classes-for-different-components}

<table>
 <tbody>
  <tr>
   <td><strong>Komponent</strong></td>
   <td><strong>Typ</strong></td>
   <td><strong>Namn</strong></td>
  </tr>
  <tr>
   <td>Sida</td>
   <td>page</td>
   <td>Användardefinierat namn <br /> eller<br /> Sida&lt;pageNumber&gt; (standard)</td>
  </tr>
  <tr>
   <td>Innehållsområde</td>
   <td>innehållsområde</td>
   <td>Användardefinierat namn</td>
  </tr>
  <tr>
   <td>Delformulär</td>
   <td>delformulär</td>
   <td>Användardefinierat namn</td>
  </tr>
  <tr>
   <td>Uteslutningsgrupp</td>
   <td>exclgroup</td>
   <td>Användardefinierat namn</td>
  </tr>
  <tr>
   <td>Rita</td>
   <td>rita</td>
   <td>Användardefinierat namn</td>
  </tr>
  <tr>
   <td>Fält</td>
   <td>fält</td>
   <td>Användardefinierat namn</td>
  </tr>
  <tr>
   <td>Bildtext</td>
   <td>bildtext</td>
   <td>NA</td>
  </tr>
  <tr>
   <td>Widget</td>
   <td>widget</td>
   <td>Widgetutvecklaren definierar den (för användardefinierade widgetar, se tabellen i följande avsnitt)</td>
  </tr>
 </tbody>
</table>

## CSS-klasser för olika fält {#css-classes-for-different-fields}

AEM Forms Designer stöder olika typer av fält i ett formulär som NumericField, DecimalField och Date Field. Alla dessa fält i HTML innehåller de ovannämnda CSS-klasserna. De innehåller också vissa extra klasser beroende på fälttypen.

Varje fält har en tillhörande widget som representerar gränssnittselementet. Klasserna för varje fält och de widgetar som är kopplade till varje fält listas nedan.

<table>
 <tbody>
  <tr>
   <td><strong>Fälttyp</strong></td>
   <td><strong>Undertyp</strong></td>
   <td><strong>Widget-namn</strong></td>
   <td><strong>Widget-typ</strong></td>
   <td><strong>HTML UI-tagg</strong></td>
  </tr>
  <tr>
   <td>Knapp<br type="_moz" /> </td>
   <td>NA</td>
   <td>xfaButton<br type="_moz" /> </td>
   <td>buttonfieldwidget<br type="_moz" /> </td>
   <td>indatatyp=knapp<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>CheckButton<br type="_moz" /> </td>
   <td>kryssrutefält<br /> </td>
   <td>XfaCheckBox<br type="_moz" /> </td>
   <td>checkboxFieldWidget<br type="_moz" /> </td>
   <td>indatatyp=kryssruta<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>DateField<br type="_moz" /> </td>
   <td>datefield<br type="_moz" /> </td>
   <td>dateField<br type="_moz" /> </td>
   <td>datefieldwidget<br type="_moz" /> </td>
   <td>indatatyp=text<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>DateTimeField<br type="_moz" /> </td>
   <td>textfält<br type="_moz" /> </td>
   <td>textField<br type="_moz" /> </td>
   <td>textfältwidget</td>
   <td>indatatyp=text<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>DecimalField<br type="_moz" /> </td>
   <td>numeriskt fält<br type="_moz" /> </td>
   <td>numericInput<br type="_moz" /> </td>
   <td>numeriskFieldWidget<br type="_moz" /> </td>
   <td>indatatyp=text<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>DropDown<br type="_moz" /> </td>
   <td>urvalslista<br type="_moz" /> </td>
   <td>dropDownListWidget<br type="_moz" /> </td>
   <td>choicelistwidget<br type="_moz" /> </td>
   <td>välj</td>
  </tr>
  <tr>
   <td>ListBox<br type="_moz" /> </td>
   <td>urvalslista<br type="_moz" /> </td>
   <td>listBoxWidget<br type="_moz" /> </td>
   <td>choicelistwidget<br type="_moz" /> </td>
   <td>ol</td>
  </tr>
  <tr>
   <td>NumericField<br type="_moz" /> </td>
   <td>numeriskt fält<br type="_moz" /> </td>
   <td>numericInput<br type="_moz" /> </td>
   <td>numeriskFieldWidget<br type="_moz" /> </td>
   <td>indatatyp=text<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>PasswordField<br type="_moz" /> </td>
   <td>lösenordsfält<br type="_moz" /> </td>
   <td>defaultWidget<br type="_moz" /> </td>
   <td>passwordFieldWidget<br type="_moz" /> </td>
   <td>indatatyp=lösenord<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>RadioButton<br type="_moz" /> </td>
   <td>radiofält <br type="_moz" /> </td>
   <td>XfaCheckBox<br type="_moz" /> </td>
   <td>radiofieldwidget<br type="_moz" /> </td>
   <td>indatatyp=radio<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>TextField<br type="_moz" /> </td>
   <td>textfält<br type="_moz" /> </td>
   <td>textField<br type="_moz" /> </td>
   <td>textfieldwidget<br type="_moz" /> </td>
   <td>indatatyp=text<br type="_moz" /> </td>
  </tr>
  <tr>
   <td>TimeField<br type="_moz" /> </td>
   <td>textfält<br type="_moz" /> </td>
   <td>textField<br type="_moz" /> </td>
   <td>textfieldwidget<br type="_moz" /> </td>
   <td>indatatyp=text<br type="_moz" /> </td>
  </tr>
 </tbody>
</table>

## CSS-klasser för olika Draw-element {#css-classes-for-different-draw-elements}

Du kan infoga statiska ritelement som text och bilder med AEM Forms Designer. För varje draw-element kopplas en separat CSS-klass till det elementet. Listan med CSS-klasser för draw-element visas nedan. Alla draw-element har en draw-klass kopplad till sig.

| **Rita typ** | **CSS-klass** |
|---|---|
| Text | text |
| Bild | image |
| Rektangel | rektangel |
| Linje | line |

## Formatera andra delar av formuläret {#styling-other-parts-of-the-form}

Förutom utseendet på gränssnittskomponenter i HTML-formuläret kan du ändra formatet på element som textbundna fel, textbundna varningar och fält med valideringsfel.

`Styling Inline Errors`

När valideringen av ett fält resulterar i ett fel visas ett internt fel när fältet är aktivt. Om du vill ändra formatet för infogade fel åsidosätter du CSS-ID:t **error-msg**.

`Styling Inline Warnings`

När valideringen av ett fält resulterar i en varning visas en intern varning när fältet är aktivt. Om du vill ändra formatet för dessa infogade varningar åsidosätter du CSS-ID:t **warning-msg**.

`Styling Fields with Validation Errors`

När valideringen för ett fält misslyckas ändras formatet på widgeten. Den här formatändringen görs genom att tillämpa en CSS-klass **widgetError** på widgetkomponenten. Om du vill ändra standardformatet åsidosätter du klassen **widgetError**.
