---
title: Utöka sökfunktionen
description: Utöka sökfunktionerna i  [!DNL Adobe Experience Manager Assets]  utöver standardvärdena.
contentOwner: AG
role: Developer
feature: Search
solution: Experience Manager, Experience Manager Assets
exl-id: 92efe52b-8fa5-4006-bd68-2472b4ba04f6
source-git-commit: a869ffbc6015fd230285838d260434d9c0ffbcb0
workflow-type: tm+mt
source-wordcount: '814'
ht-degree: 12%

---

# Utöka resurssökning {#extending-assets-search}

Du kan utöka sökfunktionerna för [!DNL Adobe Experience Manager Assets]. [!DNL Experience Manager Assets] söker efter resurser efter strängar.

Sökningen görs via gränssnittet i QueryBuilder så att sökningen kan anpassas med flera predikat. Du kan täcka över standarduppsättningen med predikat i följande katalog: `/apps/dam/content/search/searchpanel/facets`.

Du kan också lägga till fler flikar på administratörspanelen för [!DNL Assets].

>[!CAUTION]
>
>Från och med [!DNL Experience Manager] 6.4 är det klassiska användargränssnittet föråldrat. Adobe rekommenderar att du använder ett användargränssnitt med pekskärm. Mer information om anpassning finns i [sökfaktorer](/help/assets/search-facets.md).

## Övertäckning {#overlaying}

Om du vill täcka över de förkonfigurerade predikaten kopierar du noden `facets` från `/libs/dam/content/search/searchpanel` till `/apps/dam/content/search/searchpanel/` eller anger en annan `facetURL`-egenskap i `searchpanel`-konfigurationen (standardvärdet är `/libs/dam/content/search/searchpanel/facets.overlay.infinity.json`).

![screen_shot_2012-06-05at113619am](assets/screen_shot_2012-06-05at113619am.png)

>[!NOTE]
>
>Som standard finns inte katalogstrukturen under `/apps`, så skapa den. Kontrollera att nodtyperna matchar dem under `/libs`.

## Lägg till tabbar {#adding-tabs}

Du kan lägga till fler sökflikar genom att konfigurera dem i administratörsgränssnittet för [!DNL Assets]. Så här skapar du ytterligare flikar:

1. Skapa mappstrukturen `/apps/wcm/core/content/damadmin/tabs,` om den inte redan finns, och kopiera `tabs`-noden från `/libs/wcm/core/content/damadmin` och klistra in den.
1. Skapa och konfigurera den andra fliken efter behov.

   >[!NOTE]
   >
   >När du skapar ytterligare en `siteadminsearchpanel` måste du ange en `id`-egenskap för att förhindra formulärkonflikter.

## Skapa anpassade predikat {#creating-custom-predicates}

[!DNL Assets] innehåller en uppsättning fördefinierade predikat som kan användas för att anpassa en resursdelssida. Anpassa en resursresurs på det här sättet beskrivs i [Skapa och konfigurera en resursdelningssida](/help/assets/assets-finder-editor.md#creating-and-configuring-an-asset-share-page).

Förutom att använda befintliga predikat kan [!DNL Experience Manager]-utvecklare även skapa egna predikat med [Query Builder API](/help/sites-developing/querybuilder-api.md) .

Det krävs grundläggande kunskaper om [widgetramverket](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att kunna skapa anpassade predikat.

Det bästa sättet är att kopiera ett befintligt predikat och justera det. Exempelpredikaten finns i **/libs/cq/search/components/predikates**.

### Exempel: Skapa ett enkelt egenskapspredikat {#example-build-a-simple-property-predicate}

Så här skapar du ett egenskapspredikat:

1. Skapa en komponentmapp i din projektkatalog, till exempel **/apps/werdetail/components/titlepreate**.
1. Lägg till **content.xml**:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:cq="https://www.day.com/jcr/cq/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Title Predicate"
       sling:resourceSuperType="foundation/components/parbase"
       allowedParents="[*/parsys]"
       componentGroup="Search"/>
   ```

1. Lägg till `titlepredicate.jsp`.

   ```java
   <%--
   
     Sample title predicate component
   
   --%><%@ page import="java.util.Calendar" %><%
   %><%@include file="/libs/foundation/global.jsp"%><%
   
       // A unique id is necessary in case this predicate is inserted multiple times on the same page
       String elemId = "cq-predicate-" +  Long.toString(Calendar.getInstance().getTimeInMillis());
   
   %><div class="predicatebox">
   
       <div class="title">Title</div>
   
       <%-- The wrapper for the form elements. All items are appended to this wrapper. --%>
       <div id="<%= elemId %>" class="content"></div>
   
   </div><script type="text/javascript">
   
       CQ.Ext.onLoad(function() {
   
           var predicateName = "property";
           var propertyName = "jcr:content/metadata/dc:title";
           var elemId = "<%= elemId %>";
   
           // Get the page wide available QueryBuilder.
           var qb = CQ.search.Util.getQueryBuilder();
   
           // createId adds a counter to the predicate name - useful in case this predicate
           // is inserted multiple times on the same page.
           var id = qb.createId(predicateName);
   
           // Hidden field that defines the property to search for; in our case this
           // is the "dc:title" metadata. The name "property" (or "1_property", "2_property", and so on.)
           // indicates the server to use the property predicate
           // (com.day.cq.search.eval.JcrPropertyPredicateEvaluator).
           qb.addField({
               "xtype": "hidden",
               "renderTo": elemId,
               "name": id,
               "value": propertyName
           });
   
           // The visible text field. The name has to be like the one of the hidden field above
           // plus the ".value" suffix.
           qb.addField({
               "xtype": "textfield",
               "renderTo": elemId,
               "name": id + ".value"
           });
   
           // Depending on the predicate, additional parameters let you configure the
           // predicate. Here we add an operation parameter to create a "like" query.
           // Again note the name set to the id and a suffix.
           qb.addField({
               "xtype": "hidden",
               "renderTo": elemId,
               "name": id + ".operation",
               "value": "like"
           });
       });
   </script>
   ```

1. Om du vill göra komponenten tillgänglig måste du kunna redigera den. Om du vill göra en komponent redigerbar lägger du i CRXDE till en nod **cq:editConfig** av den primära typen **cq:EditConfig**. Du kan ta bort stycken genom att lägga till en egenskap med flera värden **cq:actions** med ett enda värde på **DELETE**.
1. Navigera till webbläsaren och på exempelsidan (till exempel **press.html**) växla till designläge och aktivera den nya komponenten för det prediktiva styckesystemet (till exempel **left**).

1. I **redigeringsläget** är den nya komponenten nu tillgänglig i sidosparken (finns i gruppen **Sök**). Infoga komponenten i kolumnen **Predicates** och skriv ett sökord, till exempel **Diamant**, och klicka på förstoringsglaset för att starta sökningen.

   >[!NOTE]
   >
   >När du söker måste du skriva in ordet exakt, inklusive rätt skiftläge.

### Exempel: Skapa ett enkelt grupppredikat {#example-build-a-simple-group-predicate}

Så här skapar du ett grupppredikat:

1. Skapa en komponentmapp i din projektkatalog, till exempel **/apps/werdetail/components/picspredicate**.
1. Lägg till **content.xml**:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:cq="https://www.day.com/jcr/cq/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image Formats"
       sling:resourceSuperType="foundation/components/parbase"
       allowedParents="[*/parsys]"
       componentGroup="Search"/>
   ```

1. Lägg till **titlepreate.jsp**:

   ```java
   <%--
   
     Sample group predicate component
   
   --%><%@ page import="java.util.Calendar" %><%
   %><%@include file="/libs/foundation/global.jsp"%><%
   
       // A unique id is necessary in case this predicate is inserted multiple times on the same page.
       String elemId = "cq-predicate-" +  Long.toString(Calendar.getInstance().getTimeInMillis());
   
   %><div class="predicatebox">
   
       <div class="title">Image Formats</div>
   
       <%-- The wrapper for the form elements. All items are append to this wrapper. --%>
       <div id="<%= elemId %>" class="content"></div>
   
   </div><script type="text/javascript">
   
       CQ.Ext.onLoad(function() {
   
           var predicateName = "property";
           var propertyName = "jcr:content/metadata/dc:format";
           var elemId = "<%= elemId %>";
   
           // Get the page wide available QueryBuilder.
           var qb = CQ.search.Util.getQueryBuilder();
   
           // Create a unique group ID; will return for example, "1_group".
           var groupId = qb.createGroupId();
   
           // Hidden field that defines the property to search for  - in our case "dc:format" -
           // and declares the group of predicates. "property" in the name ("1_group.property")
           // indicates to the server to use the "property predicate"
           // (com.day.cq.search.eval.JcrPropertyPredicateEvaluator).
           qb.addField({
               "xtype": "hidden",
               "renderTo": "<%= elemId %>",
               "name": groupId + "." + predicateName, // 1_group.property
               "value": propertyName
           });
   
           // Declare to combine the multiple values using OR.
           qb.add(new CQ.Ext.form.Hidden({
               "name": groupId + ".p.or",  // 1_group.p.or
               "value": "true"
           }));
   
           // The options
           var options = [
               { "label":"JPEG", "value":"image/jpeg"},
               { "label":"PNG",  "value":"image/png" },
               { "label":"GIF",  "value":"image/gif" }
           ];
   
           // Build a checkbox for each option.
           for (var i = 0; i < options.length; i++) {
               qb.addField({
                   "xtype": "checkbox",
                   "renderTo": "<%= elemId %>",
                   // 1_group.property.0_value, 1_group.property.1_value, and so on.
                   "name": groupId + "." +  predicateName + "." + i + "_value",
                   "inputValue": options[i].value,
                   "boxLabel": options[i].label,
                   "listeners": {
                       "check": function() {
                           // Submit the search form when checking/unchecking a checkbox.
                           qb.submit();
                       }
                   }
               });
           }
       });
   ```

1. Om du vill göra komponenten tillgänglig måste du kunna redigera den. Om du vill göra en komponent redigerbar lägger du i CRXDE till en nod **cq:editConfig** av den primära typen **cq:EditConfig**. Du kan ta bort stycken genom att lägga till en egenskap med flera värden **cq:actions** med ett enda värde på **DELETE**.
1. Navigera till webbläsaren och på exempelsidan (till exempel **press.html**) växla till designläge och aktivera den nya komponenten för det prediktiva styckesystemet (till exempel **left**).
1. I **redigeringsläget** är den nya komponenten nu tillgänglig i sidosparken (finns i gruppen **Sök**). Infoga komponenten i kolumnen **Predicates**.

## Installerade prediktiva widgetar {#installed-predicate-widgets}

Följande predikat är tillgängliga som förkonfigurerade ExtJS-widgetar.

### FulltextPredicate {#fulltextpredicate}

| Egenskap | Typ | Beskrivning |
|---|---|---|
| predikateName | Sträng | Predikatets namn. Standardvärdet är `fulltext` |
| searchCallback | Funktion | Återanrop för att utlösa sökning för händelse `keyup`. Standardvärdet är `CQ.wcm.SiteAdmin.doSearch` |

### PropertyPredicate {#propertypredicate}

| Egenskap | Typ | Beskrivning |
|---|---|---|
| predikateName | Sträng | Predikatets namn. Standardvärdet är `property` |
| propertyName | Sträng | Namn på JCR-egenskapen. Standardvärdet är `jcr:title` |
| defaultValue | Sträng | Förfyllt standardvärde. |

### PathPredicate {#pathpredicate}

| Egenskap | Typ | Beskrivning |
|---|---|---|
| predikateName | Sträng | Predikatets namn. Standardvärdet är `path` |
| rootPath | Sträng | Predikatets rotsökväg. Standardvärdet är `/content/dam` |
| pathFieldPredicateName | Sträng | Standardvärdet är `folder` |
| showFlatOption | Boolean | Flagga som visar kryssrutan `search in subfolders`. Standardvärdet är true. |

### DatePredicate {#datepredicate}

| Egenskap | Typ | Beskrivning |
|---|---|---|
| predikateName | Sträng | Predikatets namn. Standardvärdet är `daterange` |
| egenskapsnamn | Sträng | Namn på JCR-egenskapen. Standardvärdet är `jcr:content/jcr:lastModified` |
| defaultValue | Sträng | Förfyllt standardvärde |

### OptionsPredicate {#optionspredicate}

| Egenskap | Typ | Beskrivning |
|---|---|---|
| title | Sträng | Lägger till ytterligare en rubrik |
| predikateName | Sträng | Predikatets namn. Standardvärdet är `daterange` |
| egenskapsnamn | Sträng | Namn på JCR-egenskapen. Standardvärdet är `jcr:content/metadata/cq:tags` |
| komprimera | Sträng | Komprimera nivå. Standardvärdet är `level1` |
| triggerSearch | Boolean | Flagga för att utlösa sökning vid kontroll. Standardvärdet är false |
| searchCallback | Funktion | Återanrop för att utlösa sökning. Standardvärdet är `CQ.wcm.SiteAdmin.doSearch` |
| searchTimeoutTime | Nummer | Timeout innan searchCallback aktiveras. Standardvärdet är 800 ms |

## Anpassa sökresultat {#customizing-search-results}

Presentationen av sökresultaten på en resursdelningssida styrs av det valda objektivet. [!DNL Experience Manager Assets] innehåller en uppsättning fördefinierade objektiv som kan användas för att anpassa en resursdelssida. Att anpassa en resursresurs på det här sättet beskrivs i [Skapa och konfigurera en resursdelningssida](/help/assets/assets-finder-editor.md#creating-and-configuring-an-asset-share-page).

Förutom att använda befintliga linser kan [!DNL Experience Manager]-utvecklare även skapa egna linser.
