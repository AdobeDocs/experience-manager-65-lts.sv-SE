---
title: Developing the Bulk Editor
description: Taggar gör att innehållet kan kategoriseras och struktureras
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 3ce50030-86c7-4291-98fa-0cc9cb63f45c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# Developing the Bulk Editor{#developing-the-bulk-editor}

I det här avsnittet beskrivs hur du utvecklar verktyget Gruppredigerare och hur du utökar komponenten Produktlista, som baseras på gruppredigeraren.

## Frågeparametrar för gruppredigerare {#bulk-editor-query-parameters}

När du arbetar med gruppredigeraren finns det flera frågeparametrar som du kan lägga till i URL:en för att anropa gruppredigeraren med en viss konfiguration. Om du vill att gruppredigeraren alltid ska användas med en viss konfiguration, till exempel som i produktlistekomponenten, måste du redigera `bulkeditor.jsp` (i /libs/wcm/core/components/bulkeditor) eller skapa en komponent med den specifika konfigurationen. Ändringar som görs med frågeparametrar är inte permanenta.

Om du t.ex. skriver följande i webbläsarens URL:

`https://<servername><port_number>/etc/importers/bulkeditor.html?rootPath=/content/geometrixx/en&queryParams=geometrixx&initialSearch=true&hrp=true`

Massredigeraren visas utan fältet **Rotsökväg** eftersom hrp=true döljer fältet. Med parametern hrp=false visas fältet (standardvärdet).

Här följer en lista med frågeparametrar för gruppredigeraren:

>[!NOTE]
>
>Varje parameter kan ha ett långt och ett kort namn. Det långa namnet för sökrotsökvägen är till exempel `rootPath`, det korta är `rp`. Om det långa namnet inte är definierat läses det korta av begäran.

<table>
 <tbody>
  <tr>
   <td> </td>
   <td> </td>
   <td> </td>
  </tr>
  <tr>
   <td><p> Parameter</p> <p>(långt namn/kort namn)<br /> </p> </td>
   <td> Typ <br /> </td>
   <td> Beskrivning <br /> </td>
  </tr>
  <tr>
   <td> rootPath / rp<br /> </td>
   <td> Sträng </td>
   <td> sökrotsökväg</td>
  </tr>
  <tr>
   <td> queryParams / qp<br /> </td>
   <td> Sträng</td>
   <td> sökfråga</td>
  </tr>
  <tr>
   <td> contentMode / cm<br /> </td>
   <td> Boolean</td>
   <td> när true är innehållsläget aktiverat<br /> </td>
  </tr>
  <tr>
   <td> ProtokollValue / cv<br /> </td>
   <td> Sträng[]</td>
   <td> sökta egenskaper (markerade värden från colSelection visas som kryssrutor)</td>
  </tr>
  <tr>
   <td> extraCols / ec<br /> </td>
   <td> Sträng[]</td>
   <td> extra sökta egenskaper (visas i ett kommaseparerat textfält)</td>
  </tr>
  <tr>
   <td> initialSearch / är <br /> </td>
   <td> Boolean</td>
   <td> när true utförs frågan vid sidinläsning <br /> </td>
  </tr>
  <tr>
   <td> ProtokollSelection / cs<br /> </td>
   <td> Sträng[]</td>
   <td> sökbar egenskapsmarkering (visas som kryssrutor)</td>
  </tr>
  <tr>
   <td> showGridOnly / go<br /> </td>
   <td> Boolean</td>
   <td> om true visas endast stödrastret och inte sökpanelen <br /> </td>
  </tr>
  <tr>
   <td> searchPanelCollapsed/spc</td>
   <td> Boolean</td>
   <td> när true är sökpanelen komprimerad vid inläsning</td>
  </tr>
  <tr>
   <td> hideRootPath / hrp</td>
   <td> Boolean</td>
   <td> om true döljs rotsökvägsfältet</td>
  </tr>
  <tr>
   <td> hideQueryParams / hqp</td>
   <td> Boolean</td>
   <td> om true döljs frågefältet</td>
  </tr>
  <tr>
   <td> hideContentMode / hcm</td>
   <td> Boolean</td>
   <td> när true döljs fältet för innehållsläge</td>
  </tr>
  <tr>
   <td> hideColsSelection / hcs</td>
   <td> Boolean</td>
   <td> om true döljs kolumnmarkeringsfältet</td>
  </tr>
  <tr>
   <td> hideExtraCols / hec</td>
   <td> Boolean</td>
   <td> om true döljs fältet för extra kolumner</td>
  </tr>
  <tr>
   <td> hideSearchButton</td>
   <td> Boolean</td>
   <td> om true döljs sökknappen</td>
  </tr>
  <tr>
   <td> hideSaveButton / hsavep</td>
   <td> Boolean</td>
   <td> om true döljs knappen Spara</td>
  </tr>
  <tr>
   <td> hideExportButton / hexpb</td>
   <td> Boolean</td>
   <td> när true döljs exportknappen</td>
  </tr>
  <tr>
   <td> hideImportButton / hib</td>
   <td> Boolean</td>
   <td> om true döljs importknappen</td>
  </tr>
  <tr>
   <td> hideResultNumber / hrn</td>
   <td> Boolean</td>
   <td> om true döljs texten för stödrastrets sökresultatnummer</td>
  </tr>
  <tr>
   <td> hideInsertButton / hinsertb</td>
   <td> Boolean</td>
   <td> om true döljs infogningsknappen för stödrastret</td>
  </tr>
  <tr>
   <td> hideDeleteButton / hdelb</td>
   <td> Boolean</td>
   <td> om true döljs knappen för att ta bort stödraster</td>
  </tr>
  <tr>
   <td> hidePathCol / hpc</td>
   <td> Boolean</td>
   <td> om true döljs stödrastrets "sökvägskolumn"</td>
  </tr>
 </tbody>
</table>

### Utveckla en gruppredigeringsbaserad komponent: produktlistkomponenten {#developing-a-bulk-editor-based-component-the-product-list-component}

I det här avsnittet finns en översikt över hur du använder gruppredigeraren och en beskrivning av den befintliga Geometrixx-komponenten baserat på gruppredigeraren: produktlistkomponenten.

Med komponenten Produktlista kan användare visa och redigera en datatabell. Du kan till exempel använda komponenten Produktlista för att representera produkter i en katalog. Informationen visas i en HTML-standardtabell och redigering utförs i dialogrutan **Redigera** som innehåller en BulkEditor-widget. (Den här gruppredigeraren är samma som den som finns på /etc/importers/bulkeditor.html eller via Verktyg-menyn). Komponenten Produktlista har konfigurerats för specifika, begränsade funktioner i gruppredigeraren. Alla delar av gruppredigeraren (eller komponenter som härletts från gruppredigeraren) kan konfigureras.

Med gruppredigeraren kan du lägga till, ändra, ta bort, filtrera och exportera raderna, spara ändringar och importera en uppsättning rader. Varje rad lagras som en nod under själva produktlistkomponentinstansen. Varje cell är en egenskap för varje nod. Det här är ett designalternativ och kan enkelt ändras. Du kan till exempel lagra noder någon annanstans i databasen. Frågeserverns roll är att returnera listan med noder som ska visas. Sökvägen definieras som en produktlistinstans.

Källkoden för produktlistkomponenten finns i databasen i /apps/geometrixx/components/productlist och består av flera delar som alla Adobe Experience Manager-komponenter (AEM):

* HTML-återgivning: återgivningen görs i en JSP-fil (/apps/geometrixx/components/productlist/productlist.jsp). JSP läser undernoderna till den aktuella produktlistkomponenten och visar var och en av dem som en rad i en HTML-tabell.
* Dialogrutan Redigera, där du definierar konfigurationen för gruppredigeraren. Konfigurera dialogrutan så att den matchar komponentens behov: tillgängliga kolumner och möjliga åtgärder som utförs i rutnätet eller i sökningen. Mer information om alla konfigurationsegenskaper finns i [Konfigurationsegenskaper för gruppredigerare](#bulk-editor-configuration-properties).

Här är en XML-representation av delnoderna i dialogrutan:

```xml
        <editor
            jcr:primaryType="cq:Widget"
            colsSelection="[ProductId,ProductName,Color,CatalogCode,SellingSku]"
            colsValue="[ProductId,ProductName,Color,CatalogCode,SellingSku]"
            contentMode="false"
            exportURL="/etc/importers/bulkeditor/export.tsv"
            extraCols="Selection"
            hideColsSelection="false"
            hideContentMode="true"
            hideDeleteButton="false"
            hideExportButton="false"
            hideExtraCols="true"
            hideImportButton="false"
            hideInsertButton="false"
            hideMoveButtons="false"
            hidePathCol="true"
            hideRootPath="true"
            hideSaveButton="false"
            hideSearchButton="false"
            importURL="/etc/importers/bulkeditor/import"
            initialSearch="true"
            insertedResourceType="geometrixx/components/productlist/sku"
            queryParams=""
            queryURL="/etc/importers/bulkeditor/query.json"
            saveURL="/etc/importers/bulkeditor/save"
            xtype="bulkeditor">
            <saveButton
                jcr:primaryType="nt:unstructured"
                text="Save modifications"/>
            <searchButton
                jcr:primaryType="nt:unstructured"
                text="Apply filter"/>
            <queryParamsInput
                jcr:primaryType="nt:unstructured"
                fieldDescription="Enter here your filters"
                fieldLabel="Filters"/>
            <searchPanel
                jcr:primaryType="nt:unstructured"
                height="200">
                <defaults
                    jcr:primaryType="nt:unstructured"
                    labelWidth="150"/>
            </searchPanel>
            <grid
                jcr:primaryType="nt:unstructured"
                height="275"/>
            <store jcr:primaryType="nt:unstructured">
                <sortInfo
                    jcr:primaryType="nt:unstructured"
                    direction="ASC"
                    field="CatalogCode"/>
            </store>
            <colModel
                jcr:primaryType="nt:unstructured"
                width="150"/>
            <colsMetadata jcr:primaryType="nt:unstructured">
                <Selection
                    jcr:primaryType="nt:unstructured"
                    checkbox="true"
                    forcedPosition="0"
                    headerText=""/>
                <ProductId
                    jcr:primaryType="nt:unstructured"
                    cellCls="productlist-cell-productid"
                    headerText="Product Id"/>
                <ProductName
                    jcr:primaryType="nt:unstructured"
                    cellStyle="background-color: #FFCC99;"
                    headerText="Product Name"/>
                <CatalogCode
                    jcr:primaryType="nt:unstructured"
                    cellStyle="background-color: #EDEDED;"
                    headerText="Catalog Code"/>
                <Color jcr:primaryType="nt:unstructured">
                    <editor
                        jcr:primaryType="nt:unstructured"
                        store="[Blue,Red,Yellow]"
                        triggerAction="all"
                        typeAhead="true"
                        xtype="combo"/>
                </Color>
                <SellingSku
                    jcr:primaryType="nt:unstructured"
                    headerText="Sku Id"/>
            </colsMetadata>
        </editor>
```

### Konfigurationsegenskaper för gruppredigeraren {#bulk-editor-configuration-properties}

Alla delar av gruppredigeraren kan konfigureras. I följande tabell visas alla konfigurationsegenskaper för gruppredigeraren.

<table>
 <tbody>
  <tr>
   <td>Egenskapsnamn</td>
   <td>Definition</td>
  </tr>
  <tr>
   <td>rootPath</td>
   <td>Sökrotsökväg</td>
  </tr>
  <tr>
   <td>queryParams</td>
   <td>Sökfråga</td>
  </tr>
  <tr>
   <td>contentMode</td>
   <td>True to enable content mode: properties are read on jcr:content node and not on search result node</td>
  </tr>
  <tr>
   <td>colValue</td>
   <td>Sökda egenskaper (markerade värden från colSelection visas som kryssrutor)</td>
  </tr>
  <tr>
   <td>extraCols</td>
   <td>Extra sökta egenskaper (visas i ett textfält med kommaseparerad)</td>
  </tr>
  <tr>
   <td>initialSearch</td>
   <td>True to perform query on page load</td>
  </tr>
  <tr>
   <td>colSelection</td>
   <td>Markering av sökta egenskaper (visas som kryssrutor)</td>
  </tr>
  <tr>
   <td>showGridOnly</td>
   <td>True to show only the grid and not the search panel (glöm inte att ställa in initialSearch på true)</td>
  </tr>
  <tr>
   <td>searchPanelCollapsed</td>
   <td>Sant att komprimera sökpanelen som standard</td>
  </tr>
  <tr>
   <td>hideRootPath</td>
   <td>Dölj rotsökvägsfält</td>
  </tr>
  <tr>
   <td>hideQueryParams</td>
   <td>Dölj frågefält</td>
  </tr>
  <tr>
   <td>hideContentMode</td>
   <td>Dölj fältet för innehållsläge</td>
  </tr>
  <tr>
   <td>hideColsSelection</td>
   <td>Dölj fältet för val av protokoll</td>
  </tr>
  <tr>
   <td>hideExtraCols</td>
   <td>Dölj extra protokollfält</td>
  </tr>
  <tr>
   <td>hideSearchButton</td>
   <td>Dölj sökknapp</td>
  </tr>
  <tr>
   <td>hideSaveButton</td>
   <td>Dölj knappen Spara</td>
  </tr>
  <tr>
   <td>hideExportButton</td>
   <td>Dölj exportknapp</td>
  </tr>
  <tr>
   <td>hideImportButton</td>
   <td>Dölj importknapp</td>
  </tr>
  <tr>
   <td>hideResultNumber</td>
   <td>Dölj text för sökresultat för stödraster</td>
  </tr>
  <tr>
   <td>hideInsertButton</td>
   <td>Dölj infogningsknapp för stödraster</td>
  </tr>
  <tr>
   <td>hideDeleteButton</td>
   <td>Knappen Ta bort stödraster</td>
  </tr>
  <tr>
   <td>hidePathCol</td>
   <td>Dölj kolumnen "bana" för stödraster</td>
  </tr>
  <tr>
   <td>queryURL</td>
   <td>Sökväg till frågeservice</td>
  </tr>
  <tr>
   <td>exportURL</td>
   <td>Sökväg till exportserver</td>
  </tr>
  <tr>
   <td>importURL</td>
   <td>Sökväg till importservlet</td>
  </tr>
  <tr>
   <td>insertResourceType</td>
   <td>Resurstypen läggs till i noden när en rad infogas</td>
  </tr>
  <tr>
   <td>saveButton</td>
   <td>Spara konfiguration för knappwidget</td>
  </tr>
  <tr>
   <td>searchButton</td>
   <td>Widget-konfiguration för sökknapp</td>
  </tr>
  <tr>
   <td>exportButton</td>
   <td>Konfigurera knappwidget</td>
  </tr>
  <tr>
   <td>importButton</td>
   <td>Konfigurera knappwidget</td>
  </tr>
  <tr>
   <td>searchPanel</td>
   <td>Widgetkonfiguration för sökpanel</td>
  </tr>
  <tr>
   <td>rutnät</td>
   <td>Konfiguration för stödrasterwidget</td>
  </tr>
  <tr>
   <td>store</td>
   <td>Butikskonfiguration</td>
  </tr>
  <tr>
   <td>colModel</td>
   <td>Modellkonfiguration för stödrasterkolumn</td>
  </tr>
  <tr>
   <td>rootPathInput</td>
   <td>rootPath widget config</td>
  </tr>
  <tr>
   <td>queryParamsInput</td>
   <td>queryParams widget config</td>
  </tr>
  <tr>
   <td>contentModeInput</td>
   <td>contentMode widget config</td>
  </tr>
  <tr>
   <td>colSelectionInput</td>
   <td>formsSelection widget config</td>
  </tr>
  <tr>
   <td>extraColsInput</td>
   <td>extraCols widget, konfiguration</td>
  </tr>
  <tr>
   <td>ProtokollMetadata</td>
   <td>Kolumnens metadatakonfiguration. Möjliga egenskaper har angetts (används för alla celler i kolumnen): <br />
    <ul>
     <li>cellStyle: html-format </li>
     <li>cellCls: css-klass </li>
     <li>readOnly: true to not able change value </li>
     <li>kryssruta: true om du vill definiera alla celler i kolumnen som kryssrutor (true/false-värden) </li>
     <li>forcerad position: heltalsvärde som anger var kolumnen måste placeras i rutnätet (mellan 0 och antal kolumner-1)<p><br /> </p> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

### Metadatakonfiguration för kolumner {#columns-metadata-configuration}

Du kan konfigurera för varje kolumn:

* visningsegenskaper: html-format, CSS-klass och skrivskyddad

* en kryssruta
* en fast position

CSS och skrivskyddade kolumner

Gruppredigeraren har tre kolumnkonfigurationer:

* Cell-CSS-klassnamn (cellCls): ett CSS-klassnamn som läggs till i varje cell i den konfigurerade kolumnen.
* Cellformat (cellStyle): ett HTML-format som läggs till i varje cell i den konfigurerade kolumnen.
* Skrivskyddad (readOnly): skrivskyddad anges för varje cell i den konfigurerade kolumnen.

Konfigurationen måste definieras som följande:

```
"colsMetadata": {
"Column name": {
     "cellStyle": "html style",
     "cellCls": "CSS class",
     "readOnly": true/false
}
}
```

Följande exempel finns i produktlistekomponenten (/apps/geometrixx/components/productlist/dialog/items/editor/colMetadata):

```xml
            <colsMetadata jcr:primaryType="nt:unstructured">
                <Selection
                    jcr:primaryType="nt:unstructured"
                    checkbox="true"
                    forcedPosition="0"
                    headerText=""/>
                <ProductId
                    jcr:primaryType="nt:unstructured"
                    cellCls="productlist-cell-productid"
                    headerText="Product Id"/>
                <ProductName
                    jcr:primaryType="nt:unstructured"
                    cellStyle="background-color: #FFCC99;"
                    headerText="Product Name"/>
                <CatalogCode
                    jcr:primaryType="nt:unstructured"
                    cellStyle="background-color: #EDEDED;"
                    headerText="Catalog Code"/>
                <Color jcr:primaryType="nt:unstructured">
                    <editor
                        jcr:primaryType="nt:unstructured"
                        store="[Blue,Red,Yellow]"
                        triggerAction="all"
                        typeAhead="true"
                        xtype="combo"/>
                </Color>
                <SellingSku
                    jcr:primaryType="nt:unstructured"
                    headerText="Sku Id"/>
            </colsMetadata>
```

**Kryssruta**

Om konfigurationsegenskapen för kryssrutan är true återges alla celler i kolumnen som kryssrutor. En kryssruta skickar **true** till serverns Spara-server, annars **false**. På rubrikmenyn kan du även **markera alla** eller **inte**. De här alternativen aktiveras om den valda rubriken är rubriken för en kryssrutekolumn.

I det tidigare exemplet innehåller markeringskolumnen bara kryssrutor som checkbox=&quot;true&quot;.

**Tvingad position**

Med metadata för framtvingad position med fast position kan du ange var kolumnen placeras i rutnätet: 0 är den första platsen och &lt;antal kolumner>-1 är den sista positionen. Alla andra värden ignoreras.

I det tidigare exemplet är markeringskolumnen den första kolumnen som forceradPosition=&quot;0&quot;.

### Frågeservlet {#query-servlet}

Som standard finns frågeservern på `/libs/wcm/core/components/bulkeditor/json.java`. Du kan konfigurera en annan sökväg för att hämta data.

Frågeservern fungerar så här: den tar emot en GQL-fråga och de kolumner som ska returneras, beräknar resultaten och skickar tillbaka resultaten till gruppredigeraren som en JSON-ström.

I produktlistekomponenterna är de två parametrar som skickas till frågeservern följande:

* fråga: &quot;path:/content/geometrixx/en/customers/jcr:content/par/productlist Cube&quot;
* Protokoll: &quot;Selection,ProductId,ProductName,Color,CatalogCode,SellingSku&quot;

Och JSON-strömmen returneras enligt följande:

```
{
  "hits": [{
      "jcr:path": "/content/geometrixx/en/products/jcr:content/par/productlist/1258674828905",
      "ProductId": "21",
      "ProductName": "Cube",
      "Color": "Blue",
      "CatalogCode": "43244",
      "SellingSku": "32131"
    }
  ],
  "results": 1
}
```

Varje träff motsvarar en nod och dess egenskaper, och visas som en rad i rutnätet.

Du kan utöka frågeservern för att returnera en komplex arvsmodell eller returnera noder som lagras på en viss logisk plats. Frågeservern kan användas för alla typer av komplex beräkning. Rutnätet kan sedan visa rader som är en sammanställning av flera noder i databasen. Ändringen och sparandet av dessa rader måste i så fall hanteras av Spara server.

### Spara server {#save-servlet}

I standardkonfigurationen för gruppredigeraren är varje rad en nod och sökvägen för den här noden lagras i radposten. Med gruppredigeraren behålls länken mellan raden och noden genom jcr-sökvägen. När en användare redigerar stödrastret skapas en lista över alla ändringar. När en användare klickar på **Spara** skickas en POST-fråga till varje sökväg med de uppdaterade egenskapsvärdena. Detta är grunden för Sling-konceptet och fungerar bra om varje cell är en nodegenskap. Men om frågeservern implementeras för arvsberäkning kan modellen inte fungera som en egenskap som returneras av frågeservern kan ärvas från en annan nod.

Konceptet Spara serverlet är att ändringarna inte publiceras direkt till varje nod, utan att de bokförs på en server som utför sparandet. Detta ger den här servern möjlighet att analysera ändringarna och spara egenskaperna på rätt nod.

Varje uppdaterad egenskap skickas till servern i följande format:

* Parameternamn: &lt;jcr path>/&lt;egenskapsnamn>

  Exempel: /content/geometrixx/en/products/jcr:content/par/productlist/1258674859000/SellingSku

* Värde: &lt;value>

  Exempel: 12123

Servern behöver veta var egenskapen catalogCode lagras.

En standardserverimplementering för Spara finns på /libs/wcm/bulkeditor/save/POST.jsp och används i produktlistkomponenten. Den tar alla parametrar från begäran (med formatet &lt;jcr path>/&lt;egenskapsnamn>) och skriver egenskaper på noder med JCR API. Noden skapas också om den inte finns (rutnätsinfogade rader).

Använd inte standardkoden som den är eftersom den återimplementerar det som servern gör (en POST på &lt;jcr path>/&lt;egenskapsnamn>) och därför bara är en bra startpunkt för att skapa en Spara-server som kan hantera en egenskapsarvsmodell.
