---
title: Använda och utöka widgetar (Classic UI)
description: Adobe Experience Manager webbaserade gränssnitt använder AJAX och andra moderna webbläsartekniker för att WYSIWYG ska kunna redigera och formatera innehåll direkt på webbsidan
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: components
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 20a8e6d7-dab5-476a-9235-0abca3da5ff3
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '4896'
ht-degree: 0%

---

# Använda och utöka widgetar (Classic UI){#using-and-extending-widgets-classic-ui}

>[!NOTE]
>
>Den här sidan beskriver användningen av widgetar i det klassiska användargränssnittet, som har tagits bort i AEM 6.4.
>
>Adobe rekommenderar att du använder det moderna, [pekaktiverade gränssnittet](/help/sites-developing/touch-ui-concepts.md) baserat på [Coral UI](/help/sites-developing/touch-ui-concepts.md#coral-ui) och [Granite-gränssnittet](/help/sites-developing/touch-ui-concepts.md#granite-ui-foundation-components).

Adobe Experience Manager (AEM) webbaserade gränssnitt använder AJAX och andra moderna webbläsartekniker för att möjliggöra redigering och formatering av innehåll från WYSIWYG direkt på webbsidan.

AEM använder widgetbiblioteket [ExtJS](https://www.sencha.com/) som innehåller mycket snygga gränssnittselement som fungerar i alla de viktigaste webbläsarna och gör det möjligt att skapa användargränssnitt på skrivbordsnivå.

Dessa widgetar ingår i AEM och kan, utöver att användas av AEM, användas av alla webbplatser som skapats med AEM.

En fullständig referens om alla tillgängliga widgetar i AEM finns i [widget-API-dokumentationen](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) eller [listan över befintliga xtypes](/help/sites-developing/xtypes.md). Dessutom finns det många exempel som visar hur du använder ExtJS-ramverket på webbplatsen [Sencha](https://examples.sencha.com/extjs/7.6.0/), ramverkets ägare.

På den här sidan finns information om hur du använder och utökar widgetar. Först beskrivs hur du [inkluderar klientsidig kod på en sida](#including-the-client-sided-code-in-a-page). Sedan beskrivs några exempelkomponenter som har skapats för att illustrera grundläggande användning och tillägg. Dessa komponenter är tillgängliga i paketet **Using ExtJS Widgets** på **Package Share**.

Paketet innehåller exempel på:

* [Grundläggande dialogrutor](#basic-dialogs) som skapats med färdiga widgetar.
* [Dynamiska dialogrutor](#dynamic-dialogs) som skapats med färdiga widgetar och anpassad JavaScript-logik.
* Dialogrutor baserade på [anpassade widgetar](#custom-widgets).
* En [trädpanel](#tree-overview) som visar ett JCR-träd under en angiven sökväg.
* En [rutnätspanel](#grid-overview) som visar data i tabellformat.

>[!NOTE]
>
>Det klassiska användargränssnittet i Adobe Experience Manager bygger på [ExtJS 3.4.0](https://extjs.cachefly.net/ext-3.4.0/docs/).

## Inkludera klientsideskoden på en sida {#including-the-client-sided-code-in-a-page}

Klientsidig JavaScript och formatmallskod bör placeras i ett klientbibliotek.

Så här skapar du ett klientbibliotek:

1. Skapa en nod under `/apps/<project>` med följande egenskaper:

   * name=&quot;clientlib&quot;
   * jcr:mixinTypes=&quot;[mix:lockable]&quot;
   * jcr:primärType=&quot;cq:ClientLibraryFolder&quot;
   * sling:resourceType=&quot;widgets/clientlib&quot;
   * categories=&quot;[&lt;category-name>]&quot;
   * beroenden=[cq.widgets]

   `Note: <category-name> is the name of the custom library (for example, "cq.extjstraining") and is used to include the library on the page.`

1. Under `clientlib` skapar du mapparna `css` och `js` (not:folder).

1. Under `clientlib` skapar du filerna `css.txt` och `js.txt` (inte:filer). Dessa .txt-filer listar de filer som ingår i biblioteket.

1. Redigera `js.txt`: den måste börja med `#base=js` följt av listan över de filer som aggregeras av klientbibliotekstjänsten för CQ, till exempel:

   ```
   #base=js
    components.js
    exercises.js
    CustomWidget.js
    CustomBrowseField.js
    InsertTextPlugin.js
   ```

1. Redigera `css.txt`: den måste börja med `#base=css` följt av listan över de filer som aggregeras av klientbibliotekstjänsten för CQ, till exempel:

   ```
   #base=css
    components.css
   ```

1. Under mappen `js` placerar du de JavaScript-filer som tillhör biblioteket.

1. Under mappen `css` placerar du `.css`-filerna och de resurser som används av css-filerna (till exempel `my_icon.png`).

>[!NOTE]
>
>Det är valfritt att hantera formatmallar som beskrivs ovan.

Så här inkluderar du klientbiblioteket i sidkomponentens jsp:

* för att inkludera både JavaScript-kod och formatmallar:
  `<ui:includeClientLib categories="<category-name1>, <category-name2>, ..."/>`
där `<category-nameX>` är namnet på det klientbaserade biblioteket.

* att endast inkludera JavaScript-kod:
  `<ui:includeClientLib js="<category-name>"/>`

Mer information finns i beskrivningen av &lt;ui:includeClientLib>taggen[&#128279;](/help/sites-developing/taglib.md#lt-ui-includeclientlib).&lt;/ui:includeClientLib> 

Ibland bör ett klientbibliotek bara vara tillgängligt i redigeringsläge och bör undantas i publiceringsläge. Det kan uppnås på följande sätt:

```xml
    if (WCMMode.fromRequest(request) != WCMMode.DISABLED) {
        %><ui:includeClientLib categories="cq.collab.blog"/><%
    }
```

### Komma igång med exemplen {#getting-started-with-the-samples}

Om du vill följa självstudiekurserna på den här sidan installerar du paketet **Använda ExtJS-widgetar** i en lokal AEM-instans och skapar en exempelsida där komponenterna ingår. Gör så här:

1. Hämta paketet **Använda ExtJS-widgetar (v01)** från paketresursen i din AEM-instans och installera paketet. Det skapar projektet `extjstraining` under `/apps` i databasen.
1. Inkludera klientbiblioteket som innehåller skript (js) och formatmallen (css) i head-taggen för Geometrixx page jsp. Du kommer att inkludera exempelkomponenterna på en ny sida i **Geometrixx** -grenen:
i **CRXDE Lite** öppnar filen `/apps/geometrixx/components/page/headlibs.jsp` och lägger till kategorin `cq.extjstraining` i den befintliga `<ui:includeClientLib>` -taggen enligt följande:
   `%><ui:includeClientLib categories="apps.geometrixx-main, cq.extjstraining"/><%`
1. Skapa en sida i grenen **Geometrixx** nedan `/content/geometrixx/en/products` och anropa den **med ExtJS-widgetar**.
1. Gå i designläge och lägg till alla komponenter i gruppen **Använda ExtJS-widgetar** i designen av Geometrixx
1. Gå tillbaka i redigeringsläge: komponenterna i gruppen **Using ExtJS Widgets** är tillgängliga i Sidekick.

>[!NOTE]
>
>Exemplen på den här sidan baseras på Geometrixx exempelinnehåll, som inte längre levereras med AEM, som har ersatts av We.Retail. Se [Referensimplementering för webb.butik](/help/sites-developing/we-retail.md#we-retail-geometrixx) för hur du hämtar och installerar Geometrixx.

### Grundläggande dialogrutor {#basic-dialogs}

Dialogrutor används ofta för att redigera innehåll, men kan även visa information. Ett enkelt sätt att visa en komplett dialogruta är att få tillgång till dess representation i json-format. Om du vill göra det pekar du i webbläsaren på:

`https://localhost:4502/<path-to-dialog>.-1.json`

Den första komponenten i gruppen **Using ExtJS Widgets** i Sidekick kallas **1. Dialogrutan Grundläggande** och innehåller fyra grundläggande dialogrutor som har byggts med färdiga widgetar och utan anpassad JavaScript-logik. Dialogrutorna sparas under `/apps/extjstraining/components/dialogbasics`. De grundläggande dialogrutorna är:

* I dialogrutan Fullständig ( `full`-nod): visas ett fönster med tre flikar där varje flik har två textfält.
* dialogrutan för en panel ( `singlepanel`-nod): visar ett fönster med en flik som har två textfält.
* Multipanelsdialogrutan ( `multipanel`-nod): den visas på samma sätt som den fullständiga dialogrutan, men den byggs på ett annat sätt.
* dialogrutan Design ( `design`-nod): den visar ett fönster med två flikar. Den första fliken har ett textfält, en nedrullningsbar meny och ett komprimerbart textområde. Den andra fliken har ett fält med fyra textfält och ett komprimerbart fält med två textfält.

Inkludera **1. Dialogrutekomponenten** på exempelsidan:

1. Lägg till **1. Dialogrutekomponenten** till exempelsidan från fliken **Använda ExtJS-widgetar** i **Sidekick**.
1. Komponenten visar en titel, text och en **EGENSKAPER** -länk. När du väljer länken visas egenskaperna för det stycke som lagras i databasen. Klicka på länken igen för att dölja egenskaperna.

Komponenten visas enligt följande:

![chlimage_1-60](assets/chlimage_1-60.png)

#### Exempel 1: Fullständig dialogruta {#example-full-dialog}

I dialogrutan **Fullständig** visas ett fönster med tre flikar där varje flik har två textfält. Det är standarddialogrutan för komponenten **Dialog Basics**. Dess egenskaper är följande:

* Definieras av en nod: nodtyp = `cq:Dialog`, xtype = ` [dialog](/help/sites-developing/xtypes.md#dialog)`.
* Visar tre flikar (nodtyp = `cq:Panel`).
* Varje flik har två textfält (nodtyp = `cq:Widget`, xtype = ` [textfield](/help/sites-developing/xtypes.md#textfield)`).
* Definieras av noden:
  `/apps/extjstraining/components/dialogbasics/full`
* Renderas i JSON-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dialogbasics/full.-1.json`

Dialogrutan visas enligt följande:

![screen_shot_2012-01-31at45411pm](assets/screen_shot_2012-01-31at45411pm.png)

#### Exempel 2: Dialogrutan En panel {#example-single-panel-dialog}

Dialogrutan **En panel** visar ett fönster med en flik som har två textfält. Dess egenskaper är följande:

* Visar en flik (nodtyp = `cq:Dialog`, xtype = ` [panel](/help/sites-developing/xtypes.md#panel)`)
* Fliken har två textfält (nodtyp = `cq:Widget`, xtype = ` [textfield](/help/sites-developing/xtypes.md#textfield)`)
* Definieras av noden:
  `/apps/extjstraining/components/dialogbasics/singlepanel`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dialogbasics/singlepanel.-1.json`
* En fördel jämfört med den **fullständiga dialogrutan** är att mindre konfiguration behövs.
* Rekommenderad användning: för enkla dialogrutor som visar information eller bara har ett fåtal fält.

Så här använder du dialogrutan En panel:

1. Ersätt dialogrutan för komponenten **Dialog Basics** med dialogrutan **Single Panel** :
   1. Ta bort noden i **CRXDE Lite**: `/apps/extjstraining/components/dialogbasics/dialog`
   1. Klicka på **Spara alla** för att spara ändringarna.
   1. Kopiera noden: `/apps/extjstraining/components/dialogbasics/singlepanel`
   1. Klistra in den kopierade noden nedan: `/apps/extjstraining/components/dialogbasics`
   1. Markera noden `/apps/extjstraining/components/dialogbasics/Copy of singlepanel` och byt namn på den till `dialog`.
1. Redigera komponenten: dialogrutan visas så här:

![screen_shot_2012-01-31at45952pm](assets/screen_shot_2012-01-31at45952pm.png)

#### Exempel 3: Flera paneler {#example-multi-panel-dialog}

Dialogrutan **Flera paneler** visas på samma sätt som dialogrutan **Fullständig**, men den har skapats på ett annat sätt. Dess egenskaper är följande:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtype = ` [tabpanel](/help/sites-developing/xtypes.md#tabpanel)`).
* Visar tre flikar (nodtyp = `cq:Panel`).
* Varje flik har två textfält (nodtyp = `cq:Widget`, xtyp = ` [textfield](/help/sites-developing/xtypes.md#textfield)`).
* Definieras av noden:
  `/apps/extjstraining/components/dialogbasics/multipanel`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dialogbasics/multipanel.-1.json`
* En fördel jämfört med den **fullständiga dialogrutan** är att den har en förenklad struktur.
* Rekommenderas: för dialogrutor med flera flikar.

Använda dialogrutan Flera paneler:

1. Ersätt dialogrutan för komponenten **Dialog Basics** med dialogrutan **Flera paneler**:
följer de steg som beskrivs i [Exempel 2: Dialogrutan för en panel](#example-single-panel-dialog)
1. Redigera komponenten: dialogrutan visas så här:

![screen_shot_2012-01-31at50119pm](assets/screen_shot_2012-01-31at50119pm.png)

#### Exempel 4: Multidialog {#example-rich-dialog}

Dialogrutan **Detaljrik** visar ett fönster med två flikar. Den första fliken har ett textfält, en nedrullningsbar meny och ett komprimerbart textområde. Den andra fliken har ett fält med fyra textfält och ett komprimerbart fält med två textfält. Dess egenskaper är följande:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtype = ` [dialog](/help/sites-developing/xtypes.md#dialog)`).
* Visar två flikar (nodtyp = `cq:Panel`).
* Den första fliken har en ` [dialogfieldset](/help/sites-developing/xtypes.md#dialogfieldset)`-widget med en ` [textfield](/help/sites-developing/xtypes.md#textfield)` och en ` [selection](/help/sites-developing/xtypes.md#selection)`-widget med tre alternativ, och en komprimerbar ` [dialogfieldset](/help/sites-developing/xtypes.md#dialogfieldset)` med en ` [textarea](/help/sites-developing/xtypes.md#textarea)`-widget.
* Den andra fliken har en ` [dialogfieldset](/help/sites-developing/xtypes.md#dialogfieldset)`-widget med fyra ` [textfield](/help/sites-developing/xtypes.md#textfield)`-widgetar och en fällbar `dialogfieldset` med två ` [textfield](/help/sites-developing/xtypes.md#textfield)`-widgetar.
* Definieras av noden:
  `/apps/extjstraining/components/dialogbasics/rich`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dialogbasics/rich.-1.json`

Så här använder du dialogrutan **Multimedia**:

1. Ersätt dialogrutan för komponenten **Dialog Basics** med dialogrutan **Rich**:
följer de steg som beskrivs i [Exempel 2: Dialogrutan för en panel](#example-single-panel-dialog)
1. Redigera komponenten: dialogrutan visas så här:

![screen_shot_2012-01-31at50429pm](assets/screen_shot_2012-01-31at50429pm.png) ![screen_shot_2012-01-31at50519pm](assets/screen_shot_2012-01-31at50519pm.png)

### Dynamiska dialogrutor {#dynamic-dialogs}

Den andra komponenten i gruppen **Using ExtJS Widgets** i Sidekick kallas **2. Dynamiska dialogrutor** och innehåller tre dynamiska dialogrutor som har skapats med färdiga widgetar och **med anpassad JavaScript-logik**. Dialogrutorna sparas under `/apps/extjstraining/components/dynamicdialogs`. De dynamiska dialogrutorna är:

* dialogrutan Byt flikar ( `switchtabs`-nod): visar ett fönster med två flikar. Den första fliken har en alternativmarkering med tre alternativ: när ett alternativ är markerat visas en flik som relaterar till alternativet. Den andra fliken har två textfält.
* dialogrutan Godtycklig ( `arbitrary`-nod): visar ett fönster med en flik. Fliken innehåller ett fält där en resurs och ett fält som visar information om sidan som innehåller objektet och om resursen, om det finns någon referens till det, ska släppas eller överföras.
* dialogrutan Växla fält ( `togglefield`-nod): den visar ett fönster med en flik. Fliken har en kryssruta: när den är markerad visas en fältuppsättning med två textfält.

Inkludera **2. Komponenten för dynamiska dialogrutor** på exempelsidan:

1. Lägg till **2. Dynamiska dialogrutor**-komponenten till exempelsidan på fliken **Använda ExtJS-widgetar** i **Sidekick** .
1. Komponenten visar en titel, text och en **EGENSKAPER** -länk. När du väljer länken visas egenskaperna för det stycke som lagras i databasen. Klicka på länken igen för att dölja egenskaperna.

Komponenten visas enligt följande:

![chlimage_1-61](assets/chlimage_1-61.png)

#### Exempel 1: Dialogrutan Byt flik {#example-switch-tabs-dialog}

Dialogrutan **Byt flikar** visar ett fönster med två flikar. Den första fliken har en alternativmarkering med tre alternativ: när ett alternativ är markerat visas en flik som relaterar till alternativet. Den andra fliken har två textfält.

Dess huvudsakliga egenskaper är:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtype = ` [dialog](/help/sites-developing/xtypes.md#dialog)`).
* Visar två flikar (nodtyp = `cq:Panel`): en markeringsflik, den andra fliken beror på markeringen på den första fliken (tre alternativ).
* Har tre valfria flikar (nodtyp = `cq:Panel`), där var och en har två textfält (nodtyp = `cq:Widget`, xtype = ` [textfield](/help/sites-developing/xtypes.md#textfield)`). Endast en valfri flik i taget visas.
* Definieras av noden `switchtabs` på:
  `/apps/extjstraining/components/dynamicdialogs/switchtabs`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dynamicdialogs/switchtabs.-1.json`

Logiken implementeras med händelseavlyssnare och JavaScript-kod enligt följande:

* Dialognoden har en `beforeshow`-avlyssnare som döljer alla valfria flikar innan dialogrutan visas:
  `beforeshow="function(dialog){Ejst.x2.manageTabs(dialog.items.get(0));}"`
  `dialog.items.get(0)` hämtar `tabpanel` som innehåller markeringspanelen och de tre valfria panelerna.
* Objektet `Ejst.x2` definieras i filen `exercises.js` på:
  `/apps/extjstraining/clientlib/js/exercises.js`
* I metoden `Ejst.x2.manageTabs()` är alla valfria flikar dolda eftersom värdet för `index` är -1 (i går från 1 till 3).
* Markeringsfliken har två avlyssnare: en som visar den valda fliken när dialogrutan läses in (&quot; `loadcontent`&quot;-händelse) och en som visar den valda fliken när markeringen ändras (&quot; `selectionchanged`&quot;-händelse):
  `loadcontent="function(field,rec,path){Ejst.x2.showTab(field);}"`
  `selectionchanged="function(field,value){Ejst.x2.showTab(field);}"`
* För metoden `Ejst.x2.showTab()`:
  `field.findParentByType('tabpanel')` hämtar `tabpanel` som innehåller alla flikar ( `field` representerar markeringswidgeten)
  `field.getValue()` hämtar värdet för markeringen, till exempel tab2
  `Ejst.x2.manageTabs()` visar den valda fliken.
* Varje valfri flik har en avlyssnare som döljer fliken för händelsen `render`:
  `render="function(tab){Ejst.x2.hideTab(tab);}"`
* För metoden `Ejst.x2.hideTab()`:
  `tabPanel` är den `tabpanel` som innehåller alla flikar
  `index` är indexet för den valfria fliken
  `tabPanel.hideTabStripItem(index)` Döljer fliken

Den visas enligt följande:

![screen_shot_2012-02-01at114745am](assets/screen_shot_2012-02-01at114745am.png)

#### Exempel 2: Godtycklig dialogruta {#example-arbitrary-dialog}

I en dialogruta visas ofta innehåll från den underliggande komponenten. Dialogrutan som beskrivs här, med namnet **Godtycklig**, hämtar innehåll från en annan komponent.

Dialogrutan **Godtycklig** visar ett fönster med en flik. Fliken har två fält: ett för att släppa eller överföra en resurs och ett för att visa viss information om behållarsidan och om resursen om det finns referenser till en sådan.

Dess huvudsakliga egenskaper är:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtype = ` [dialog](/help/sites-developing/xtypes.md#dialog)`).
* Visar en `tabpanel`-widget (nodtyp = `cq:Widget`, xtype = ` [tabpanel](/help/sites-developing/xtypes.md#tabpanel)`) med en panel (nodtyp = `cq:Panel`)
* Panelen har en smartfile-widget (nodtyp = `cq:Widget`, xtype = ` [smartfile](/help/sites-developing/xtypes.md#smartfile)`) och en ownerdraw-widget (nodtyp = `cq:Widget`, xtype = ` [ownerdraw](/help/sites-developing/xtypes.md#ownerdraw)`)
* Definieras av noden `arbitrary` på:
  `/apps/extjstraining/components/dynamicdialogs/arbitrary`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dynamicdialogs/arbitrary.-1.json`

Logiken implementeras med händelseavlyssnare och JavaScript-kod enligt följande:

* Widgeten `ownerdraw` har en `loadcontent`-avlyssnare som visar information om sidan som innehåller komponenten. Det vill säga den resurs som smartfile-widgeten refererar till när innehållet läses in:
  `loadcontent="function(field,rec,path){Ejst.x2.showInfo(field,rec,path);}"`
  `field` anges med objektet `ownerdraw`
  `path` anges med komponentens innehållssökväg (till exempel `/content/geometrixx/en/products/triangle/ui-tutorial/jcr:content/par/dynamicdialogs`)
* Objektet `Ejst.x2` definieras i filen `exercises.js` på:
  `/apps/extjstraining/clientlib/js/exercises.js`
* För metoden `Ejst.x2.showInfo()`:
  `pagePath` är sökvägen till sidan som innehåller komponenten;
  `pageInfo` representerar sidegenskaperna i json-format;
  `reference` är sökvägen till den refererade resursen;
  `metadata` representerar metadata för resursen i json-format,
  `ownerdraw.getEl().update(html);` visar den skapade HTML-koden i dialogrutan

Så här använder du dialogrutan **Godtycklig**:

1. Ersätt dialogrutan för komponenten **Dynamisk dialog** med dialogrutan Godtycklig **:** 
Följ stegen som beskrivs för Exempel [2: Dialogrutan En panel](#example-single-panel-dialog)
1. Redigera komponenten: dialogrutan visas så här:

![screen_shot_2012-02-01at115300am](assets/screen_shot_2012-02-01at115300am.png)

#### Exempel 3: Växla fält {#example-toggle-fields-dialog}

Dialogrutan **Växla fält** visar ett fönster med en flik. Fliken har en kryssruta: när den är markerad visas ett fält med två textfält.

Dess viktigaste egenskaper är:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtyp = ` [dialog](/help/sites-developing/xtypes.md#dialog)`).
* Visar en `tabpanel` widget (nodtyp = `cq:Widget`, xtyp = ` [tabpanel](/help/sites-developing/xtypes.md#textpanel)`) med en panel (nodtyp = `cq:Panel`).
* Panelen har en val-/kryssrutewidget (nodtyp = `cq:Widget`, xtype = ` [selection](/help/sites-developing/xtypes.md#selection)`, type = ` [checkbox](/help/sites-developing/xtypes.md#checkbox)`) och en hopfällbar dialogfältuppsättningswidget (nodtyp = `cq:Widget`, xtype = ` [dialogfieldset](/help/sites-developing/xtypes.md#dialogfieldset)`) som är dold som standard, med två textfältswidgets (nodtyp = `cq:Widget`, xtype = ` [textfield](/help/sites-developing/xtypes.md#textfield)`).
* Definieras av noden `togglefields` på:
  `/apps/extjstraining/components/dynamicdialogs/togglefields`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/dynamicdialogs/togglefields.-1.json`

Logiken implementeras med händelseavlyssnare och JavaScript-kod enligt följande:

* På urvalsfliken finns två avlyssnare: en som visar dialogfältuppsättningen när innehållet läses in (&quot; `loadcontent`&quot;-händelse) och en som visar dialogfältuppsättningen när markeringen ändras (&quot; `selectionchanged`&quot;-händelse):
  `loadcontent="function(field,rec,path){Ejst.x2.toggleFieldSet(field);}"`
  `selectionchanged="function(field,value){Ejst.x2.toggleFieldSet(field);}"`
* Objektet `Ejst.x2` definieras i filen `exercises.js` på:
  `/apps/extjstraining/clientlib/js/exercises.js`
* För metoden `Ejst.x2.toggleFieldSet()`:
  `box` är markeringsobjektet;
  `panel` är den panel som innehåller markeringen och dialogfältuppsättningswidgetar;
  `fieldSet` är dialogfältuppsättningsobjektet;
  `show` är värdet för markeringen (true eller false);
baserat på &#39; `show`&#39; visas dialogfältsuppsättningen eller inte

Så här använder du dialogrutan **Växla fält**:

1. Ersätt dialogrutan för komponenten **Dynamisk dialogruta** med dialogrutan **Växla fält**:
följer de steg som beskrivs i [Exempel 2: Dialogrutan för en panel](#example-single-panel-dialog)
1. Redigera komponenten: dialogrutan visas så här:

![screen_shot_2012-02-01at115518am](assets/screen_shot_2012-02-01at115518am.png)

### Anpassade widgetar {#custom-widgets}

De färdiga widgetarna som levereras med AEM bör omfatta de flesta användningsfall. Ibland kan det dock vara nödvändigt att skapa en anpassad widget som täcker ett projektspecifikt krav. Du kan skapa anpassade widgetar genom att utöka befintliga. För att hjälpa dig komma igång med en sådan anpassning innehåller paketet **`Using ExtJS Widgets`** tre dialogrutor som använder tre olika anpassade widgetar:

* I dialogrutan Flerfält ( `multifield`-nod) visas ett fönster med en flik. Fliken har en anpassad widget för flera fält som har två fält: en nedrullningsbar meny med två alternativ och ett textfält. Eftersom den baseras på den användningsklara `multifield`-widgeten (som bara har ett textfält) har den alla funktioner i `multifield`-widgeten.
* I dialogrutan Trädbläddring (noden `treebrowse`) visas ett fönster med en flik som innehåller en sökvägswidget: när du klickar på pilen öppnas ett fönster där du kan bläddra i en hierarki och markera ett objekt. Sökvägen för objektet läggs sedan till i sökvägsfältet och bevaras när dialogrutan stängs.
* en plugin-dialogruta ( `rteplugin`-nod) för Rich Text Editor som lägger till en anpassad knapp i RTF-redigeraren för att infoga viss anpassad text i huvudtexten. Den består av en `richtext`-widget (RTE) och en anpassad funktion som läggs till via plugin-programmet för textredigering.

De anpassade widgetarna och plugin-programmet ingår i komponenten **3. Anpassade widgetar** för **paketet Using ExtJS Widgets** . Så här tar du med den här komponenten på exempelsidan:

1. Lägg till **3. Anpassade widgetar** till exempelsidan från **fliken Använda ExtJS-widgetar** i **Sidekick**.
1. Komponenten visar en titel, text och, när du **klickar på länken PROPERTIES** , egenskaperna för det stycke som lagras i databasen. Om du klickar igen döljs egenskaperna.
Komponenten visas enligt följande:

![chlimage_1-62](assets/chlimage_1-62.png)

#### Exempel 1: Anpassad widget för flera fält {#example-custom-multifield-widget}

Widgetbaserad dialogruta **Eget multifält** visar ett fönster med en flik. Fliken har en anpassad widget för flera fält som, till skillnad från standardwidgeten som har ett fält, har två fält: en listruta med två alternativ och ett textfält.

Widgetbaserad dialogruta för **Anpassat multifält**:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtype = ` [dialog](/help/sites-developing/xtypes.md#dialog)`).
* Visar en `tabpanel`-widget (nodtyp = `cq:Widget`, xtype = ` [tabpanel](/help/sites-developing/xtypes.md#tabpanel)`) som innehåller en panel (nodtyp = `cq:Widget`, xtype = ` [panel](/help/sites-developing/xtypes.md#panel)`).
* Panelen har en `multifield`-widget (nodtyp = `cq:Widget`, xtype = ` [multifield](/help/sites-developing/xtypes.md#multifield)`).
* Widgeten `multifield` har en fieldConfig (nodtyp = `nt:unstructured`, xtype = `ejstcustom`, optionsProvider = `Ejst.x3.provideOptions`) som är baserad på den anpassade xtype &#39; `ejstcustom`&#39;:
   * `fieldconfig` är ett konfigurationsalternativ för objektet ` [CQ.form.MultiField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.form.MultiField)`.
   * `optionsProvider` är en konfiguration av `ejstcustom`-widgeten. Den anges med metoden `Ejst.x3.provideOptions` som definieras i `exercises.js` vid:

     `/apps/extjstraining/clientlib/js/exercises.js`
och returnerar två alternativ.
* Definieras av noden `multifield` på:
  `/apps/extjstraining/components/customwidgets/multifield`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/customwidgets/multifield.-1.json`

Den anpassade `multifield`-widgeten (xtype = `ejstcustom`):

* Är ett JavaScript-objekt med namnet `Ejst.CustomWidget`
* Definieras i JavaScript-filen `CustomWidget.js` på:
  `/apps/extjstraining/clientlib/js/CustomWidget.js`
* Utökar widgeten ` [CQ.form.CompositeField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.form.CompositeField)`.
* Har tre fält: `hiddenField` (Textfield), `allowField` (ComboBox) och `otherField` (Textfield)
* Åsidosätter `CQ.Ext.Component#initComponent` för att lägga till de tre fälten:
   * `allowField` är ett [&#x200B; CQ.form.Selection](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.form.Selection) -objekt av typen select. optionsProvider är en konfiguration av Selection-objektet som initieras med optionsProvider-konfigurationen för CustomWidget som definierats i dialogrutan
   * `otherField` är ett [&#x200B; CQ.Ext.form.TextField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.form.TextField) -objekt
* Åsidosätter metoderna `setValue`, `getValue`, och `getRawValue` för [CQ.form.CompositeField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.form.CompositeField) för att ange och hämta värdet för CustomWidget med formatet:
  `<allowField value>/<otherField value>, for example: 'Bla1/hello'`.
* Registrerar sig som `ejstcustom`-xtype:
  `CQ.Ext.reg('ejstcustom', Ejst.CustomWidget);`

Widgetbaserad dialogruta för **Anpassat multifält** visas enligt följande:

![screen_shot_2012-02-01at115840am](assets/screen_shot_2012-02-01at115840am.png)

#### Exempel 2: Anpassad `Treebrowse` widget {#example-custom-treebrowse-widget}

Den anpassade **`Treebrowse`** widgetbaserade dialogrutan visar ett fönster med en flik som innehåller en anpassad sökvägsbläddringswidget. När du klickar på pilen öppnas ett fönster där du kan bläddra i en hierarki och välja ett objekt. Sökvägen till objektet läggs sedan till i sökvägsfältet och sparas när dialogrutan stängs.

Den anpassade `treebrowse` dialogrutan:

* Definieras av en nod (nodtyp = `cq:Dialog`, xtyp = ` [dialog](/help/sites-developing/xtypes.md#dialog)`).
* Visar en `tabpanel`-widget (nodtyp = `cq:Widget`, xtype = ` [tabpanel](/help/sites-developing/xtypes.md#tabpanel)`) som innehåller en panel (nodtyp = `cq:Widget`, xtype = ` [panel](/help/sites-developing/xtypes.md#panel)`).
* Panelen har en anpassad widget (nodtyp = `cq:Widget`, xtype = `ejstbrowse`)
* Definieras av noden `treebrowse` på:
  `/apps/extjstraining/components/customwidgets/treebrowse`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/customwidgets/treebrowse.-1.json`

Widgeten för anpassad trädbläddring (xtype = `ejstbrowse`):

* Är ett JavaScript-objekt med namnet `Ejst.CustomWidget`
* Definieras i JavaScript-filen `CustomBrowseField.js` på:
  `/apps/extjstraining/clientlib/js/CustomBrowseField.js`
* Utökar ` [CQ.Ext.form.TriggerField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.form.TriggerField)`.
* Definierar ett webbläsarfönster med namnet `browseWindow`.
* Åsidosätter ` [CQ.Ext.form.TriggerField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.form.TriggerField)#onTriggerClick` för att visa bläddringsfönstret när någon klickar på pilen.
* Definierar ett [CQ.Ext.tree.TreePanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.tree.TreePanel) -objekt:
   * Den får sina data genom att anropa den server som är registrerad på `/bin/wcm/siteadmin/tree.json`.
   * Dess rot är `apps/extjstraining`.
* Definierar ett `window`-objekt ( ` [CQ.Ext.Window](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.Window)`):
   * Baserat på den fördefinierade panelen.
   * Har en **OK**-knapp som anger värdet för den markerade banan och döljer panelen.
* Fönstret är förankrat under fältet **Sökväg**.
* Den valda sökvägen skickas från bläddringsfältet till fönstret för händelsen `show`.
* Registrerar sig som `ejstbrowse`-xtype:
  `CQ.Ext.reg('ejstbrowse', Ejst.CustomBrowseField);`

Så här använder du den **anpassade webbläsarwidgetbaserade dialogrutan**:

1. Ersätt dialogrutan för komponenten **Anpassade widgetar** med dialogrutan **Anpassad trädbläddring**:
följer de steg som beskrivs i [Exempel 2: Dialogrutan för en panel](#example-single-panel-dialog)
1. Redigera komponenten: dialogrutan visas så här:

![screen_shot_2012-02-01at120104pm](assets/screen_shot_2012-02-01at120104pm.png)

#### Exempel 3: Plugin-program för RTE (Rich Text Editor) {#example-rich-text-editor-rte-plug-in}

Den **plugin-baserade** dialogrutan för RTE (Rich Text Editor) är en dialogruta baserad på RTF Editor som har en anpassad knapp för att infoga anpassad text inom hakparenteser. Den anpassade texten kan parsas av viss logik på serversidan (implementeras inte i det här exemplet), till exempel för att lägga till text som definieras på den angivna sökvägen:

Dialogrutan baserad **på RTE-insticksprogrammet** :

* Definieras av plugin-programnoden på:
  `/apps/extjstraining/components/customwidgets/rteplugin`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/customwidgets/rteplugin.-1.json`
* Noden `rtePlugins` har en underordnad nod `inserttext` (nodtyp = `nt:unstructured`) som namnges efter plugin-programmet. Den har en egenskap med namnet `features` som definierar vilka av plugin-funktionerna som är tillgängliga för RTE.

RTE-plugin:

* Är ett JavaScript-objekt med namnet `Ejst.InsertTextPlugin`
* Definieras i JavaScript-filen `InsertTextPlugin.js` på:
  `/apps/extjstraining/clientlib/js/InsertTextPlugin.js`
* Utökar objektet ` [CQ.form.rte.plugins.Plugin](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.form.rte.plugins.Plugin)`.
* Följande metoder definierar objektet ` [CQ.form.rte.plugins.Plugin](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.form.rte.plugins.Plugin)` och åsidosätts i det implementerande plugin-programmet:
   * `getFeatures()` returnerar en array med alla funktioner som plugin-programmet gör tillgängliga.
   * `initializeUI()` lägger till den nya knappen i verktygsfältet för textredigering.
   * `notifyPluginConfig()` visar rubrik och text när knappen hovras.
   * `execute()` anropas när någon klickar på knappen och utför plugin-åtgärden: det visar ett fönster som används för att definiera texten som ska inkluderas.
* `insertText()` infogar en text med motsvarande dialogobjekt `Ejst.InsertTextPlugin.Dialog` (se efteråt).
* `executeInsertText()` anropas av metoden `apply()` i dialogrutan, som aktiveras när användaren klickar på knappen **OK**.
* Registrerar sig som plugin-programmet `inserttext`:
  `CQ.form.rte.plugins.PluginRegistry.register("inserttext", Ejst.InsertTextPlugin);`
* `Ejst.InsertTextPlugin.Dialog`-objektet definierar den dialogruta som öppnas när användaren klickar på plugin-knappen. Dialogrutan består av en panel, ett formulär, ett textfält och två knappar (**OK** och **Avbryt**).

Så här använder du den **RTE-baserade dialogrutan** (Rich Text Editor):

1. Ersätt dialogrutan för komponenten **Anpassade widgetar** med den **RTE-baserade dialogrutan** (Rich Text Editor):
följer de steg som beskrivs i [Exempel 2: Dialogrutan för en panel](#example-single-panel-dialog)
1. Redigera komponenten.
1. Klicka på den sista ikonen till höger (den med fyra pilar). Ange en sökväg och klicka på **OK**:
Sökvägen visas inom hakparenteser ([) ]).
1. Klicka på **OK** så att du stänger RTF-redigeraren.

Den **plugin-baserade dialogrutan för RTE-program** (Rich Text Editor) visas så här:

![screen_shot_2012-02-01AT120254PM](assets/screen_shot_2012-02-01at120254pm.png)

>[!NOTE]
>
>I det här exemplet visas bara hur du implementerar klientdelen av logiken: platshållarna (*[text]*) måste sedan tolkas explicit på serversidan (till exempel i komponent-JSP).

### Översikt över träd {#tree-overview}

Det körklara objektet ` [CQ.Ext.tree.TreePanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.tree.TreePanel)` ger en trädstrukturerad gränssnittsrepresentation av trädstrukturerade data. Komponenten Tree Overview som ingår i **paketet Using ExtJS Widgets** visar hur du `TreePanel` använder objektet för att visa ett JCR-träd under en viss sökväg. Själva fönstret kan dockas/avdockas. I det här exemplet är fönsterlogiken inbäddad i komponenten jsp mellan &lt;script> taggar.

Så här inkluderar du komponenten **Trädöversikt** på exempelsidan:

1. **Lägg till 4. Trädöversiktskomponenten** till exempelsidan från **fliken Använda ExtJS-widgetar** i **Sidekick**.
1. Komponenten visar:
   * en titel, med text
   * en **EGENSKAPER**-länk: klicka för att visa egenskaperna för det stycke som lagras i databasen. Klicka igen för att dölja egenskaperna.
   * ett flytande fönster med en trädrepresentation av databasen som kan expanderas.

Komponenten visas enligt följande:

![screen_shot_2012-02-01at120639pm](assets/screen_shot_2012-02-01at120639pm.png)

Komponenten Trädöversikt:

* Definieras vid:
  `/apps/extjstraining/components/treeoverview`

* I dialogrutan kan du ange fönstrets storlek och docka eller avdocka fönstret (se informationen nedan).

Komponent-jsp:

* Hämtar egenskaperna width, height och dockad från databasen.
* Visar text om trädöversiktens dataformat.
* Bäddar in fönsterlogiken i komponent-jsp mellan JavaScript-taggar.
* Definieras vid:
  `apps/extjstraining/components/treeoverview/content.jsp`

Den JavaScript-kod som är inbäddad i komponent-jsp:

* Definierar ett `tree`-objekt genom att försöka hämta ett trädfönster från sidan.
* Om fönstret som visar trädet inte finns skapas `treePanel` ([CQ.Ext.tree.TreePanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.tree.TreePanel)):
   * `treePanel` innehåller de data som används för att skapa fönstret.
   * Data hämtas genom att anropa servern som är registrerad på:

     `/bin/wcm/siteadmin/tree.json`
* Avlyssnaren `beforeload` kontrollerar att den valda noden har lästs in.
* Objektet `root` anger sökvägen `apps/extjstraining` som trädrot.
* `tree` ( ` [CQ.Ext.Window](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.Window)`) anges baserat på den fördefinierade `treePanel` och visas med:
  `tree.show();`
* Om fönstret finns visas det baserat på egenskaperna width, height och dockad som hämtats från databasen.

Komponentdialogrutan:

* Visar en flik med två fält som anger storleken (bredd och höjd) på trädöversiktsfönstret och ett fält som kan dockas/avdockas i fönstret
* Definieras av en nod (nodtyp = `cq:Dialog`, xtype = ` [panel](/help/sites-developing/xtypes.md#panel)`).
* Panelen har en widget för storleksfält (nodtyp = `cq:Widget`, xtype = ` [sizefield](/help/sites-developing/xtypes.md#sizefield)`) och en markeringswidget (nodtyp = `cq:Widget`, xtype = ` [selection](/help/sites-developing/xtypes.md#selection)`, type = `radio`) med två alternativ (true/false)
* Definieras av dialognoden vid:
  `/apps/extjstraining/components/treeoverview/dialog`
* Renderas i json-format genom att begära:
  `https://localhost:4502/apps/extjstraining/components/treeoverview/dialog.-1.json`
* Visar följande:

![screen_shot_2012-02-01at120745pm](assets/screen_shot_2012-02-01at120745pm.png)

### Stödrasteröversikt {#grid-overview}

En rutnätspanel representerar data i tabellformat för rader och kolumner. Den består av följande:

* Store: Den modell som innehåller dataposterna (rader).
* Kolumnmodell: kolumnsammansättning.
* View : kapslar in användargränssnittet.
* Markeringsmodell : markeringsbeteendet.

Komponenten för stödrasteröversikt som ingår i paketet **Using ExtJS Widgets** visar hur du visar data i tabellformat:

* I exempel 1 används statiska data.
* I exempel 2 används data som hämtats från databasen.

Så här tar du med komponenten Stödrasteröversikt till exempelsidan:

1. Lägg till **5. Stödrasteröversikt** till exempelsidan från fliken **Använda ExtJS-widgetar** i **Sidekick**.
1. Komponenten visar:
   * en titel med text
   * en **EGENSKAPER**-länk: klicka för att visa egenskaperna för det stycke som lagras i databasen. Klicka igen för att dölja egenskaperna.
   * ett flytande fönster som innehåller data i tabellformat.

Komponenten visas enligt följande:

![screen_shot_2012-02-01at121109pm](assets/screen_shot_2012-02-01at121109pm.png)

#### Exempel 1: Standardstödraster {#example-default-grid}

I den färdiga versionen **visar komponenten Rutnätsöversikt** ett fönster med statiska data i tabellformat. I det här exemplet är logiken inbäddad i komponenten jsp på två sätt:

* Den generiska logiken definieras mellan &lt;script> taggar
* den specifika logiken finns i en separat .js-fil och är länkad till den i jsp-filen. Med den här inställningen kan du växla mellan de två logiken (statisk/dynamisk) genom att kommentera de önskade &lt;script>-taggarna.

Komponenten Stödrasteröversikt:

* Definieras på:
  `/apps/extjstraining/components/gridoverview`
* I dialogrutan kan du ställa in storleken på fönstret och docka eller avdocka fönstret.

Komponenten jsp:

* Hämtar egenskaperna width, height och docked från databasen.
* Visar text som introduktion till dataformatet för rutnätsöversikten.
* Refererar till JavaScript-kod som definierar GridPanel-objektet:
  `<script type="text/javascript" src="/apps/extjstraining/components/gridoverview/defaultgrid.js"></script>`
  `defaultgrid.js` definierar vissa statiska data som bas för GridPanel-objektet.
* Bäddar in JavaScript-kod mellan JavaScript-taggar som definierar Window-objektet som använder GridPanel-objektet.
* Definieras vid:
  `apps/extjstraining/components/gridoverview/content.jsp`

Den JavaScript-kod som är inbäddad i komponent-jsp:

* Definierar objektet `grid` genom att försöka hämta fönsterkomponenten från sidan:
  `var grid = CQ.Ext.getCmp("<%= node.getName() %>-grid");`
* Om `grid` inte finns definieras ett [&#x200B; CQ.Ext.grid.GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.GridPanel) -objekt ( `gridPanel`) genom att metoden `getGridPanel()` anropas (se nedan). Den här metoden definieras i `defaultgrid.js`.
* `grid` är ett ` [CQ.Ext.Window](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.Window)`-objekt baserat på den fördefinierade GridPanel-panelen och visas: `grid.show();`
* Om det finns `grid` visas den baserat på egenskaperna width, height och dockad som hämtats från databasen.

JavaScript-filen ( `defaultgrid.js`) som refereras i komponent-jsp definierar metoden `getGridPanel()` som anropas av skriptet som är inbäddat i JSP och returnerar ett ` [CQ.Ext.grid.GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.GridPanel)` -objekt baserat på statiska data. Logiken är följande:

* `myData` är en matris med statiska data som formaterats som en tabell med fem kolumner och fyra rader.
* `store` är ett `CQ.Ext.data.Store`-objekt som förbrukar `myData`.
* `store` har lästs in i minnet:
  `store.load();`
* `gridPanel` är ett ` [CQ.Ext.grid.GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.GridPanel)`-objekt som förbrukar `store`:
   * kolumnbredderna ändras alltid:

     `forceFit: true`
   * bara en rad åt gången kan markeras:

     `singleSelect:true`

#### Exempel 2: Referenssökstödraster {#example-reference-search-grid}

När du installerar paketet visar `content.jsp` för komponenten **Stödrasteröversikt** ett stödraster som baseras på statiska data. Det går att ändra komponenten så att ett rutnät visas med följande egenskaper:

* Har tre kolumner.
* Baseras på data som hämtats från databasen genom att anropa en server.
* Cellerna i den sista kolumnen kan redigeras. Värdet finns kvar i en `test`-egenskap under noden som definieras av sökvägen som visas i den första kolumnen.

Så som förklaras i avsnittet ovan hämtar fönsterobjektet sitt ` [CQ.Ext.grid.GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.GridPanel)`-objekt genom att anropa metoden `getGridPanel()` som definieras i filen `defaultgrid.js` vid `/apps/extjstraining/components/gridoverview/defaultgrid.js`. Komponenten **Grid Overview &#x200B;** ger en annan implementering av metoden `getGridPanel()` som definieras i filen `referencesearch.js` på `/apps/extjstraining/components/gridoverview/referencesearch.js`. Genom att byta .js-fil som refereras i komponentens jsp, baseras rutnätet på data som hämtas från databasen.

Byt .js-fil som refereras i komponent-jsp:

1. I **CRXDE Lite** kommenterar du raden som innehåller filen `defaultgrid.js` i filen `content.jsp` för komponenten, så att den ser ut så här:
   `<!-- script type="text/javascript" src="/apps/extjstraining/components/gridoverview/defaultgrid.js"></script-->`
1. Ta bort kommentaren från raden som innehåller filen `referencesearch.js` så att den ser ut så här:
   `<script type="text/javascript" src="/apps/extjstraining/components/gridoverview/referencesearch.js"></script>`
1. Spara ändringarna.
1. Uppdatera exempelsidan.

Komponenten visas enligt följande:

![screen_shot_2012-02-01at121429pm](assets/screen_shot_2012-02-01at121429pm.png)

Den JavaScript-kod som refereras i komponent-jsp ( `referencesearch.js`) definierar den `getGridPanel()` -metod som anropas från komponent-jsp och returnerar ett ` [CQ.Ext.grid.GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.GridPanel)` -objekt baserat på data som hämtas dynamiskt från databasen. Logiken i `referencesearch.js` definierar vissa dynamiska data som bas för GridPanel:

* `reader` är ett ` [CQ.Ext.data.JsonReader](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.data.JsonReader)`objekt som läser serverletssvaret i json-format för tre kolumner.
* `cm` är ett ` [CQ.Ext.grid.ColumnModel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.ColumnModel)`-objekt för tre kolumner.
Kolumncellerna i kolumnen&quot;Testa&quot; kan redigeras så som de definieras med en redigerare:
  `editor: new [CQ.Ext.form.TextField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.form.TextField)({})`
* kolumnerna kan sorteras:
  `cm.defaultSortable = true;`
* `store` är ett ` [CQ.Ext.data.GroupingStore](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.data.GroupingStore)`-objekt:
   * hämtar data genom att anropa servern som är registrerad på `/bin/querybuilder.json` med några parametrar som används för att filtrera frågan
   * den baseras på `reader`, definierad i förväg
   * tabellen sorteras enligt kolumnen **jcr:path** i stigande ordning
* `gridPanel` är ett ` [CQ.Ext.grid.EditorGridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.EditorGridPanel)`-objekt som kan redigeras:
   * den baseras på den fördefinierade `store` och på kolumnmodellen `cm`
   * bara en rad åt gången kan markeras:

     `sm: new [CQ.Ext.grid.RowSelectionModel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html?class=CQ.Ext.grid.RowSelectionModel)({singleSelect:true})`
   * avlyssnaren `afteredit` ser till att när en cell i kolumnen **Test** har redigerats:
      * egenskapen `test` för noden vid sökvägen som definieras av kolumnen **jcr:path** anges i databasen med cellens värde
      * om POST lyckas läggs värdet till i objektet `store`, annars avvisas det
