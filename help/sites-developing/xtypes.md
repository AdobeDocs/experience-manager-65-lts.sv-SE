---
title: Använda xtypes (Classic UI)
description: Läs mer om alla typer som är tillgängliga med Adobe Experience Manager
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: components
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 4a78de53-33bf-4999-ba3c-7d0bc33196a4
source-git-commit: 24bd1f57da3f9ce613ee28276d1ae9465b6dfba6
workflow-type: tm+mt
source-wordcount: '3668'
ht-degree: 0%

---

# Använda xtypes (Classic UI){#using-xtypes-classic-ui}

På den här sidan beskrivs alla typer som är tillgängliga med Adobe Experience Manager (AEM).

I ExtJS-språket är en xtype ett symboliskt namn som ges till en klass. Du kan läsa stycket&quot;Component XTypes&quot; i [Översikt över ExtJS 2](https://docs.sencha.com/) för en detaljerad förklaring om vad en xtype är och hur den kan användas.

Mer information om alla tillgängliga widgetar i AEM finns i [API-dokumentationen för widgeten](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

Om du vill ta reda på i vilka komponenter en viss xtype används i AEM kan du använda följande `Xpath`-fråga i CRXDE. Ersätt bara kryssrutan med den typ av text som du är intresserad av:

`//element(*, cq:Widget)[@xtype='checkbox']`

>[!NOTE]
>
>Den här sidan beskriver användningen av ExtJS-xtyper i det klassiska användargränssnittet.
>
>Adobe rekommenderar att du använder det vanliga, moderna, [pekaktiverade gränssnittet](/help/sites-developing/touch-ui-concepts.md) baserat på [Coral-gränssnittet](/help/sites-developing/touch-ui-concepts.md#coral-ui) och [Granite-gränssnittet](/help/sites-developing/touch-ui-concepts.md#granite-ui-foundation-components).

## xtypes {#xtypes}

Nedan visas de tillgängliga xtyperna i Adobe Experience Manager:

* `annotation`

  [CQ.wcm.Annotation](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Annotation` är ett specialfönster. Den har ett formulär i sin brödtext och en knappgrupp i sidfoten. Det används vanligtvis för att redigera innehåll, men kan även visa enbart information.

* `arraystore`

  [CQ.Ext.data.ArrayStore](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Tidigare känt som `SimpleStore`.

  En liten hjälpklass som gör det enklare att skapa [CQ.Ext.data.Store](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)s från arraydata. En `ArrayStore` konfigureras automatiskt med en [&#x200B; CQ.Ext.data.ArrayReader](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `asseteditor`

  [CQ.dam.AssetEditor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Asset Editor` används i DAM Admin.

* `assetreferencesearchdialog`

  [CQ.wcm.AssetReferenceSearchDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `AssetReferenceSearchDialog` är en dialogruta som visas om en sida refererar till resurser eller taggar.

* `blueprintconfig`

  [CQ.wcm.msm.BlueprintConfig](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  I `BlueprintConfig` finns en panel där du kan visa Live-kopior av en utkast och redigera dessa blå egenskaper (synkroniseringsutlösare och synkroniseringsåtgärder).

* `blueprintstatus`

  [CQ.wcm.msm.BlueprintStatus](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med BlueprintStatus kan du visa och redigera en utkast och dess Live-kopior-relationer. Bläddringen görs via en [CQ.wcm.msm.BlueprintStatus.Tree](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html), en utgåva via en [&#x200B; CQ.wcm.msm.BlueprintConfig](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och en [&#x200B; CQ.wcm.msm.LiveCopyProperties](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `box`

  [CQ.Ext.BoxComponent](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Basklass för alla [komponenter](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som ska storleksändras som en ruta, med bredd och höjd.

  BoxComponent har automatiska rutmodelljusteringar för storlek och placering och fungerar korrekt i komponentåtergivningsmodellen.

* `browsedialog`

  [CQ.BrowseDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med BrowseDialog kan användaren bläddra i databasen och välja en sökväg. Det används vanligtvis via ett [BrowseField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `browsefield`

  [CQ.form.BrowseField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  **Inaktuellt: Använd [&#x200B; CQ.form.PathField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i stället**

* `bulkeditor`

  [CQ.wcm.BulkEditor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `BulkEditor` innehåller en sökmotor och ett rutnät för att redigera sökresultat.

  `BulkEditor` måste infogas i ett HTML-formulär (krävs för importfunktioner). Detta fungerar perfekt med en [CQ.Dialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `bulkeditorform`

  [CQ.wcm.BulkEditorForm](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  BulkEditorForm innehåller [CQ.wcm.BulkEditor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) omgivna av ett HTML-formulär. Den fristående versionen av [CQ.wcm.BulkEditor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). Ett HTML-formulär krävs för att importera-knappen.

* `button`

  [CQ.Ext.Button](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Klassen Simple Button

* `buttongroup`

  [CQ.Ext.ButtonGroup](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Behållare för en grupp knappar.

* `chart`

  [CQ.Ext.chart.Chart](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Paketet CQ.Ext.chart ger möjlighet att visualisera data med flash-based charting. Varje diagram binds direkt till en CQ.Ext.data.Store som möjliggör automatiska uppdateringar av diagrammet. Information om hur du ändrar utseendet på ett diagram finns i konfigurationsalternativen [chartStyle](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [extraStyle](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) .

* `checkbox`

  [CQ.Ext.form.Checkbox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Ett kryssrutefält. Kan användas som ersättning för traditionella kryssrutefält.

* `checkboxgroup`

  [CQ.Ext.form.CheckboxGroup](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En grupperingsbehållare för kontrollerna [CQ.Ext.form.Checkbox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `clearcombo`

  [CQ.form.ClearableComboBox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  ClearableComboBox är en icke-redigerbar kombinationsruta med en utlösare som rensar dess värde.

* `colorfield`

  [CQ.form.ColorField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med ColorField kan användaren ange ett hex-färgvärde antingen direkt eller med [CQ.Ext.ColorMenu](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `colorlist`

  [CQ.form.ColorList](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med ColorList kan användaren välja en färg i en redigerbar lista.

* `colormenu`

  [CQ.Ext.menu.ColorMenu](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En meny som innehåller en [CQ.Ext.ColorPalette](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -komponent.

* `colorpalette`

  [CQ.Ext.ColorPalette](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Enkel färgpalettklass för att välja färger. Paletten kan återges i vilken behållare som helst.

* `combo`

  [CQ.Ext.form.ComboBox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En kombinationsruta med stöd för automatisk komplettering, fjärrinläsning, sidinläsning och många andra funktioner.

  En ComboBox fungerar på ungefär samma sätt som ett vanligt HTML-fält. Skillnaden är att om du vill skicka [valueField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) måste du ange ett [hiddenName](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att skapa dolda indata.

* `component`

  [CQ.Ext.Component](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Basklass för alla `Ext`-komponenter. Alla underklasser till komponenten kan delta i den automatiserade `Ext`-komponentlivscykeln för skapande, återgivning och destruktion, som tillhandahålls av klassen [&#x200B; Container](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). Komponenter kan läggas till i en behållare med konfigurationsalternativet [items](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) när behållaren skapas.

* `componentextractor`

  [CQ.wcm.ComponentExtractor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med ComponentExtractor kan användaren extrahera komponenter från en webbplats/sida.

* `componentselector`

  [CQ.form.ComponentSelector](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En grupperad, ordnad markering med tillgängliga komponenter.

* `componentstyles`

  [CQ.form.ComponentStyles](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

* `compositefield`

  [CQ.form.CompositeField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Basklass för panelbaserade, komplexa formulärfält som innehåller ett formulärfält eller en grupp av formulärfält.

* `container`

  [CQ.Ext.Container](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Basklass för alla [CQ.Ext.BoxComponent](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som kan innehålla andra komponenter. Behållare hanterar grundbeteendet för objekt som innehåller, nämligen att lägga till, infoga och ta bort objekt.

  De vanligaste behållarklasserna är [CQ.Ext.Panel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html), [CQ.Ext.Window](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [CQ.Ext.TabPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `contentfinder`

  [CQ.wcm.ContentFinder](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  ContentFinder är en speciell [visningsruta](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) med två kolumner, som innehåller den faktiska innehållssökaren till vänster och innehållsramen till höger.

* `contentfindertab`

  [CQ.wcm.ContentFinderTab](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  ContentFinderTab är en specialpanel med funktioner som används i flikpanelerna i [CQ.wcm.ContentFinder](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). Vanligtvis innehåller den ett sökformulär - rutan Fråga - och en datavy som visar sökningen.

* `cq.workflow.model.combo`

  [CQ.wcm.WorkflowModelCombo](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  WorkflowModelCombo är en anpassad [CQ.Ext.form.ComboBox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som visar en lista över tillgängliga arbetsflödesmodeller.

* `cq.workflow.model.selector`

  [CQ.wcm.WorkflowModelSelector](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  WorkflowModelSelector kombinerar en WorkflowModelCombo med en miniatyrbild av arbetsflödet och knappar för att skapa och redigera arbetsflödesmodeller.

* `createsitewizard`

  [CQ.wcm.CreateSiteWizard](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  CreateSiteWizard är en steg-för-steg-guide för att skapa (MSM) platser.

* `createversiondialog`

  [CQ.wcm.CreateVersionDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  CreateVersionDialog är en dialogruta där du kan skapa en version av en sida.

* `customcontentpanel`

  [CQ.CustomContentPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  CustomContentPanel är en specialpanel som används i [CQ.Dialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html): innehållet hämtas från och skickas till en annan URL än de andra fälten i dialogrutan.

* `cycle`

  [CQ.Ext.CycleButton](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En specialiserad SplitButton som innehåller en meny med [CQ.Ext.menu.CheckItem](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -element. Knappen bläddrar automatiskt genom varje menyalternativ vid varje klick och höjer knappens [change](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -händelse (eller anropar knappens [changeHandler](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -funktion, om sådan finns) för det aktiva menyalternativet.

* `dataview`

  [CQ.Ext.DataView](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En mekanism för att visa data med anpassade layoutmallar och formatering. DataView använder en [CQ.Ext.XTemplate](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som sin interna mallmetod och är bunden till en [&#x200B; CQ.Ext.data.Store](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) så att vyn uppdateras automatiskt när data i arkivet ändras så att ändringarna återspeglas.

* `datefield`

  [CQ.Ext.form.DateField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Det innehåller ett datuminmatningsfält med en [CQ.Ext.DatePicker](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)-listruta och automatisk datumvalidering.

* `datemenu`

  [CQ.Ext.menu.DateMenu](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En meny som innehåller en [CQ.Ext.DatePicker](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -komponent.

* `datepicker`

  [CQ.Ext.DatePicker](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En datumväljare. Den här klassen används av klassen [DateField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att tillåta bläddring och val av giltiga datum.

* `datetime`

  [CQ.form.DateTime](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med DateTime kan användaren ange ett datum och en tid genom att kombinera [CQ.Ext.form.DateField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [CQ.Ext.form.TimeField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `dialog`

  [CQ.Dialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Dialogrutan är ett specialfönster. Den har ett formulär i sin brödtext och en knappgrupp i sidfoten. Det används vanligtvis för att redigera innehåll, men kan även visa enbart information.

* `dialogfieldset`

  [CQ.form.DialogFieldSet](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  DialogFieldSet är en [FieldSet](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som kan användas i [Dialogs](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `directstore`

  [CQ.Ext.data.DirectStore](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En liten hjälpklass för att skapa en [CQ.Ext.data.Store](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som har konfigurerats med en [&#x200B; CQ.Ext.data.DirectProxy](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [&#x200B; CQ.Ext.data.JsonReader](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) så att det blir enklare att interagera med en [&#x200B; CQ.Ext.Direct](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) Server-side [Provider](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `displayfield`

  [CQ.Ext.form.DisplayField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Ett textfält som bara visas och som inte är validerat och inte har skickats.

* `editbar`

  [CQ.wcm.EditBar](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med EditBar kan användaren redigera innehåll med knappar på ett fält.

  Även om den inte finns med här finns alla medlemmar i [CQ.wcm.EditBase](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i EditBar.

* `editor`

  [CQ.Ext.Editor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Ett basredigeringsfält som hanterar visning/döljning vid behov och har inbyggd logik för storleksändring och händelsehantering.

* `editorgrid`

  [CQ.Ext.grid.EditorGridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Den här klassen utökar [GridPanel-klassen](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) så att cellredigering kan göras för markerade [kolumner](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). De redigerbara kolumnerna anges med en [redigerare](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i [kolumnkonfigurationen](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `editrollover`

  [CQ.wcm.EditRollover](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med EditRollover kan användaren redigera innehåll genom att dubbelklicka och få fler redigeringsåtgärder via en snabbmeny. Det redigerbara området markeras med en ram när muspekaren förs över innehållet.

* `feedimporter`

  [CQ.wcm.FeedImporter](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med FeedImporter kan användaren importera RSS- eller Atom-flöden och skapa sidor för varje feed-post.

* `field`

  [CQ.Ext.form.Field](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Basklass för formulärfält som innehåller standardhändelsehantering, storleksändring, värdehantering och andra funktioner.

* `fieldset`

  [CQ.Ext.form.FieldSet](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Standardbehållare som används för att gruppera objekt i ett [formulär](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `fileuploaddialogbutton`

  [CQ.form.FileUploadDialogButton](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  FileUploadDialogButton skapar en knapp som öppnar en ny dialogruta för att överföra en fil via FileUploadField. Kan användas i redigeringsdialogrutor där överföringen måste ske i ett separat formulär.

* `fileuploadfield`

  [CQ.form.FileUploadField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med FileUploadField kan användaren välja en fil som ska överföras.

* `findreplacedialog`

  [CQ.wcm.FindReplaceDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  FindReplaceDialog är en dialogruta där du kan söka efter och ersätta tokens på en sida och dess underordnade sidor.

* `flash`

  [CQ.Ext.FlashComponent](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

* `grid`

  [CQ.Ext.grid.GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Den här klassen representerar det primära gränssnittet för en komponentbaserad stödrasterkontroll som representerar data i tabellformat för rader och kolumner.

* `groupingstore`

  [CQ.Ext.data.GroupingStore](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En specialiserad butiksimplementering som möjliggör gruppering av poster i ett av de tillgängliga fälten. Används med [CQ.Ext.grid.GroupingView](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att bevisa datamodellen för en grupperad GridPanel.

* `heavymovedialog`

  [CQ.wcm.HeavyMoveDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Dialogrutan HeavyMoveDialog är en dialogruta där du kan flytta en sida och dess underordnade sidor. Dessutom kan du aktivera om tidigare aktiverade sidor (tung flyttning).

* `hidden`

  [CQ.Ext.form.Hidden](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Ett enkelt dolt fält för lagring av dolda värden i formulär som måste skickas i formuläret som skickas.

* `historybutton`

  [CQ.HistoryButton](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  HistoryButton är en liten hjälpklass som enkelt tillhandahåller bakåt- och framåtknappar. Vanligtvis krävs två relaterade instanser: Framåtknappen är en enkel knapp som är länkad till bakåtknappsinstansen som hanterar historiken.

* `htmleditor`

  [CQ.ext.form.htmlEditor](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Den innehåller en kompakt HTML Editor-komponent. Safari stöder inte vissa verktygsfältsfunktioner, så de döljs automatiskt när det behövs. Antecknas i konfigurationsalternativen där det är lämpligt.

  Redigerarens verktygsfältsknappar har verktygstips definierade i egenskapen [buttonTips](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) .

* `iframedialog`

  [CQ.IframeDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En vanlig dialogruta med innehållet i en iframe och som tillåter formulär i iframes.

* `iframepanel`

  [CQ.IframePanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En panel som innehåller en iframe. Det gör det enkelt att skapa iframes, en inläsningshändelse för iframe och att enkelt komma åt innehållet i iframe.

* `inlinetextfield`

  [CQ.form.InlineTextField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  InlineField är ett textfält som visas som en etikett när det inte är i fokus.

* `jsonstore`

  [CQ.Ext.data.JsonStore](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En liten hjälpklass som gör det enklare att skapa [CQ.Ext.data.Store](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)s från JSON-data. En JsonStore konfigureras automatiskt med [CQ.Ext.data.JsonReader](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `label`

  [CQ.Ext.form.Label](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Grundläggande etikettfält.

* `languagecopydialog`

  [CQ.wcm.LanguageCopyDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Dialogrutan LanguageCopy är en dialogruta där du kan kopiera språkträd.

* `linkchecker`

  [CQ.wcm.LinkChecker](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  LinkChecker är ett verktyg för att kontrollera externa länkar på en plats.

* `listview`

  [CQ.Ext.list.ListView](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  CQ.Ext.list.ListView är en snabb och lätt implementering av en [stödrasterliknande](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) vy.

* `livecopyproperties`

  [CQ.wcm.msm.LiveCopyProperties](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  I LiveCopyProperties finns en panel där du kan visa och redigera Live Copy-egenskaper (relationsarv, synkroniseringsutlösare och synkroniseringsåtgärder).

* `lvbooleancolumn`

  [CQ.Ext.list.BooleanColumn](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En kolumndefinitionsklass som återger booleska datafält. Mer information finns i konfigurationsalternativet [xtype](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i [CQ.Ext.list.Column](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `lvcolumn`

  [CQ.Ext.list.Column](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Den här klassen kapslar in kolumnkonfigurationsdata som ska användas vid initieringen av en [ListView](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `lvdatecolumn`

  [CQ.Ext.list.DateColumn](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En kolumndefinitionsklass som återger ett skickat datum enligt standardspråket, eller ett konfigurerat [format](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). Mer information finns i konfigurationsalternativet [xtype](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i [CQ.Ext.list.Column](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `lvnumbercolumn`

  [CQ.Ext.list.NumberColumn](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En kolumndefinitionsklass som återger ett numeriskt datafält enligt strängen [format](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). Mer information finns i konfigurationsalternativet [xtype](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i [CQ.Ext.list.Column](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `mediabrowsedialog`

  [CQ.MediaBrowseDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  **Föråldrat: Använd [Innehållssökning](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att bläddra bland media i stället.**

  MediaBrowseDialog är en dialogruta för att bläddra i mediebiblioteket.

* `menu`

  [CQ.Ext.menu.Menu](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Ett menyobjekt. Behållaren som du kan lägga till menyalternativ i. Menyn kan också fungera som en basklass när du vill ha en speciell meny baserad på en annan komponent (till exempel [CQ.Ext.menu.DateMenu](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)).

  Menyer kan innehålla antingen [menyalternativ](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) eller allmänna [komponenter](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)s.

* `menubaseitem`

  [CQ.Ext.menu.BaseItem](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Basklassen för alla objekt som återges i menyer. BaseItem innehåller standardalternativ för återgivning, aktiverad tillståndshantering och baskonfigurering som delas av alla menykomponenter.

* `menucheckitem`

  [CQ.Ext.menu.CheckItem](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Det lägger till ett menyalternativ som innehåller en kryssruta som standard, men som också kan ingå i en alternativgrupp.

* `menuitem`

  [CQ.Ext.menu.Item](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En basklass för alla menyalternativ som kräver menyrelaterade funktioner (som undermenyer) och inte är statiska visningsobjekt. Objektet utökar basfunktionerna i [CQ.Ext.menu.BaseItem](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) genom att lägga till menyspecifik aktivering och klickningshantering.

* `menuseparator`

  [CQ.Ext.menu.Separator](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En avgränsare läggs till i en meny som används för att dela upp logiska grupper av menyalternativ. Vanligtvis lägger du till en i det genom att använda &quot;-&quot; i anropet för att lägga till() eller i objektkonfigurationen i stället för att skapa en direkt.

* `menutextitem`

  [CQ.Ext.menu.TextItem](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Den lägger till en statisk textsträng på en meny, som används som rubrikavgränsare eller gruppavgränsare.

* `metadata`

  [CQ.dam.form.Metadata](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Metadata` innehåller en uppsättning fält för att avgöra vilken information som krävs för ett metadatafält, som t.ex. används på sidorna i Resursredigeraren.

  Den innehåller följande fält:

* `multifield`

  [CQ.form.MultiField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `MultiField` är en redigerbar lista med formulärfält för redigering av egenskaper med flera värden.

* `mvt`

  [CQ.form.MVT](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Multivariate Testing-komponenten kan användas för att definiera och redigera en uppsättning bilder som visas som omväxlande banderoller. Klicka igenom prisstatistik per banner.

* `notificationinbox`

  [CQ.wcm.NotificationInbox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `NotificationInbox` tillåter användaren att prenumerera på WCM-åtgärder och hantera meddelanden.

* `numberfield`

  [CQ.Ext.form.NumberField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Numeriskt textfält med automatisk filtrering av tangenttryckningar och numerisk validering.

* `offlineimporter`

  [CQ.wcm.OfflineImporter](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `OfflineImporter` är ett verktyg för att importera och konvertera Microsoft® Word-dokument till AEM-sidor. Med den här funktionen kan innehåll redigeras offline med en ordbehandlare.

* `ownerdraw`

  [CQ.form.OwnerDraw](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `OwnerDraw` kan innehålla anpassad HTML-kod (anges direkt eller hämtas från en URL).

* `paging`

  [CQ.Ext.PagingToolbar](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  När antalet poster ökar, ökar tiden som krävs för att webbläsaren ska kunna återge dem. Sidindelning används för att minska mängden data som utbyts med klienten.

* `panel`

  [CQ.Ext.Panel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En `panel` är en behållare som har specifika funktioner och strukturella komponenter som gör den till den perfekta byggstenen för programorienterade användargränssnitt.

  Paneler är, på grund av sitt arv, från [CQ.Ext.Container](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `paragraphreference`

  [CQ.form.ParagraphReference](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  I referensfältet för stycken kan du bläddra bland sidor och välja ett av deras stycken. Det består av ett utlösarfält och en tillhörande dialogruta för styckebläddring.

* `password`

  [CQ.form.Password](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Password` är som ett [&#x200B; CQ.Ext.form.TextField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) men behåller sitt värde privat, vilket gör att användare kan ange känsliga data.

* `pathcompletion`

  [CQ.form.PathCompletion](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  **Inaktuellt: Använd [&#x200B; CQ.form.PathField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i stället**

* `pathfield`

  [CQ.form.PathField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `PathField` är ett inmatningsfält som är utformat för sökvägar med sökvägsinmatning och en knapp för att öppna en [&#x200B; CQ.BrowseDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att bläddra i serverdatabasen. Den kan även bläddra bland sidstycken för avancerad länkgenerering.

* `progress`

  [CQ.Ext.ProgressBar](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En komponent som kan uppdateras. Förloppsindikatorn har stöd för två olika lägen: manuellt och automatiskt.

  I manuellt läge ansvarar du för att visa, uppdatera (via [updateProgress](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)) och rensa förloppsindikatorn efter behov från din egen kod. Den här metoden passar bäst när du vill visa förloppet.

* `propertygrid`

  [CQ.Ext.grid.PropertyGrid](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En specialiserad stödrasterimplementering som är avsedd att efterlikna det traditionella egenskapsstödrastret, som vanligtvis visas i utvecklingsmiljöer. Varje rad i rutnätet representerar en egenskap för ett objekt, och data lagras som en uppsättning namn/värde-par i [CQ.Ext.grid.PropertyRecord](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)s.

* `propgrid`

  [CQ.PropertyGrid](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `PropertyGrid` är ett generiskt stödraster som används för att visa och redigera egenskaper för objekt.

* `quicktip`

  [CQ.Ext.QuickTip](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `@xtype quicktip` - En specialklass för verktygstips som kan anges i kod och hanteras automatiskt av den globala [&#x200B; CQ.Ext.QuickTips](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -instansen. Mer användningsinformation och exempel finns i rubriken för klassen QuickTips.

* `radio`

  [CQ.Ext.form.Radio](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Ett `radio`-fält. Samma som kryssruta, men det är till för att underlätta automatisk inställning av indatatyp. Webbläsaren grupperar automatiskt alternativknappar när varje alternativknapp i gruppen har samma namn.


* `radiogroup`

  [CQ.Ext.form.RadioGroup](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En grupperingsbehållare för kontrollerna [CQ.Ext.form.Radio](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `referencesdialog`

  [CQ.wcm.ReferencesDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `ReferencesDialog` är en dialogruta där du kan visa referenser på en sida.

* `restoretreedialog`

  [CQ.wcm.RestoreTreeDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `RestoreTreeDialog` är en dialogruta där du kan återställa en tidigare version av ett träd.

* `restoreversiondialog`

  [CQ.wcm.RestoreVersionDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  RestoreVersionDialog är en dialogruta där du kan återställa en tidigare version av en sida.

* `richtext`

  [CQ.form.RichText](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `RichText` innehåller ett formulärfält för redigering av formaterad textinformation (RTF).

  Komponenten `RichText` innehåller för närvarande följande funktioner:

* `rolloutplan`

  [CQ.wcm.msm.RolloutPlan](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  I RolloutPlan finns en dialogruta där du kan se hur en sida rullas ut. En [CQ.wcm.msm.RolloutWizard](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) startar RolloutPlan.

* `rolloutwizard`

  [CQ.wcm.msm.RolloutWizard](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `RolloutWizard` innehåller en guide för att rulla ut en sida. RolloutWizard startar en [CQ.wcm.msm.RolloutPlan](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `searchfield`

  [CQ.form.SearchField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `SearchField` innehåller ett sökfält som ger resultaten i en nedrullningsbar lista som kan användas för att söka i databasen.

* `selection`

  [CQ.form.Selection](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med `Selection` kan användaren välja mellan flera alternativ. Alternativen kan ingå i konfigurationen eller läsas in från ett JSON-svar. Markeringen kan antingen återges som en nedrullningsbar (markerad) eller som en kombinationsruta (markerad plus fritextpost).

* `sidekick`

  [CQ.wcm.Sidekick](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Sidekick` är en flytande hjälp som ger användaren tillgång till vanliga verktyg för sidredigering.

* `siteadmin`

  [CQ.wcm.SiteAdmin](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `SiteAdmin` är en konsol som tillhandahåller WCM-administrationsfunktioner.

* `siteimporter`

  [CQ.wcm.SiteImporter](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med `SiteImporter` kan användaren importera hela webbplatser och skapa inledande projekt.

* `sizefield`

  [CQ.form.SizeField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med `SizeField` kan användaren ange bredd och höjd (till exempel för en bild).

* `slider`

  [CQ.Ext.Slider](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Skjutreglage som har stöd för lodrät eller vågrät orientering, tangentbordsjusteringar, konfigurerbar fästning, axelklickning och animering. Den kan läggas till som ett objekt i valfri behållare. Användning: ...

* `slideshow`

  [CQ.form.Slideshow](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med bildspelskomponenten kan du definiera och redigera en uppsättning bilder och bildtitlar. Användarna kan visa uppsättningen som ett bildspel.

  Bildspelskomponenten är baserad på komponenten [CQ.form.SmartImage](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) .

* `smartfile`

  [CQ.form.SmartFile](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  SmartFile är en intelligent filuppladdare.

  Om ett Flash-plugin-program (version >= 9) är installerat körs överföringarna med SWFupload-biblioteket, som är ett bekvämt sätt att hantera överföringar.

* `smartimage`

  [CQ.form.SmartImage](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  SmartImage är en intelligent bildöverförare. Den innehåller verktyg för att bearbeta en överförd bild, till exempel ett verktyg för att definiera bildscheman och en bildpipett.

  Komponenten är utformad för att användas på en separat dialogruteflik.

* `spacer`

  [CQ.Ext.Spacer](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Används för att skapa ett stort utrymme i en layout.

* `spinner`

  [CQ.form.Spinner](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Spinner` är ett utlösarfält för numeriska värden, datum- eller tidsvärden. Värdet kan ökas och minskas med hjälp av upp- och nedutlösarna, rullningshjulet eller tangenterna.

* `splitbutton`

  [CQ.Ext.SplitButton](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En `splitbutton` som innehåller en inbyggd nedrullningsbar pil som kan utlösa en händelse separat från knappens standardklickningshändelse. Vanligtvis används den för att visa en nedrullningsbar meny som innehåller ytterligare alternativ till den primära knappåtgärden, men alla anpassade hanterare kan tillhandahålla implementeringen av `arrowclick`.

* `static`

  [CQ.Static](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Static` kan användas för att visa godtycklig text eller HTML.

* `statistics`

  [CQ.wcm.Statistics](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Statistics` visar sidavbildningarna som ett diagram. Med widgeten kan du välja en period som statistiken ska visas för.

* `store`

  [CQ.Ext.data.Store](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Klassen `Store` kapslar in en klientcache med [&#x200B; Record](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)-objekt som tillhandahåller indata för komponenter som [GridPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html), [ComboBox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) eller [DataView](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `suggestfield`

  [CQ.form.SuframField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `SuggestField` ger användaren förslag baserat på deras inmatning.

* `switcher`

  [CQ.Switcher](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `Switcher` innehåller en knappgrupp för sidhuvudsfältet i en konsol för att växla mellan webbplatser, Digital Assets, Verktyg, Arbetsflöde och Säkerhet.

* `tableedit`

  [CQ.form.TableEdit](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  **Inaktuell: Använd [&#x200B; CQ.form.TableEdit2](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i stället.**

* `tableedit2`

  [CQ.form.TableEdit2](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `TableEdit2` innehåller en widget för att skapa tabeller.

* `tabpanel`

  [CQ.Ext.TabPanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En enkel flikbehållare. TabPanels kan användas exakt som en standard [CQ.Ext.Panel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) i layoutsyfte, men har även särskilt stöd för att innehålla underordnade komponenter ([`items`](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)).

* `tags`

  [CQ.tagging.TagInputField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  ```
  CQ.tagging.TagInputField
  ```

  är en formulärwidget för att ange taggar. Den har en snabbmeny där du kan välja bland befintliga taggar, som innehåller automatisk komplettering och många andra funktioner.

* `textarea`

  [CQ.Ext.form.TextArea](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Flerradigt textfält. Kan användas som direkt ersättning för traditionella `textarea`-fält, plus stöd för automatisk storleksjustering.

* `textbutton`

  [CQ.TextButton](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `TextButton` innehåller en textlänk med funktionerna i en [&#x200B; CQ.Ext.Button](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

* `textfield`

  [CQ.Ext.form.TextField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Grundläggande textfält. Kan användas som direkt ersättning för traditionella textinmatningar eller som basklass för mer avancerade inmatningskontroller (som [CQ.Ext.form.TextArea](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [CQ.Ext.form.ComboBox](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)).

* `thumbnail`

  [CQ.form.Thumbnail](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

* `timefield`

  [CQ.Ext.form.TimeField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Det innehåller ett tidsinmatningsfält med en tidslistruta och automatisk tidsvalidering. Exempelanvändning: ...

* `tip`

  [CQ.Ext.Tip](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  @xtype tip Det här är basklassen för [CQ.Ext.QuickTip](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [CQ.Ext.Tooltip](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som innehåller den grundläggande layout och placering som alla tipsbaserade klasser kräver. Den här klassen kan användas direkt för enkla, statiskt placerade tips.

* `titleseparator`

  [CQ.menu.TitleSeparator](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En avgränsare läggs till i en meny som används för att dela upp logiska grupper av menyalternativ. Avgränsaren kan också innehålla en titel.

* `toolbar`

  [CQ.Ext.Toolbar](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Grundläggande `Toolbar`-klass. Även om [`defaultType`](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för verktygsfältet är [`button`](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) kan verktygsfältselement (underordnade objekt för verktygsfältets behållare) vara praktiskt taget vilken typ av komponent som helst. Verktygsfältselement kan skapas explicit via deras konstruktorer.

* `tooltip`

  [CQ.Ext.ToolTip](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En `tooltip`-standardimplementering som ger ytterligare information när du hovrar över ett målelement. @xtype-verktygstips.

* `treegrid`

  [CQ.tree.TreeGrid](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  @xtype `treegrid`

* `treepanel`

  [CQ.Ext.tree.TreePanel](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  `TreePanel` innehåller trädstrukturerade gränssnittsrepresentationer av trädstrukturerade data.

  [TreeNode](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som lagts till i `TreePanel` kan innehålla metadata som används av ditt program i deras [attributes](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) -egenskap.

* `trigger`

  [CQ.Ext.form.TriggerField](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Det är en praktisk wrapper för `TextFields` som lägger till en klickbar utlösarknapp (ser ut som en kombinationsruta som standard). Utlösaren har ingen standardåtgärd, så du måste tilldela en funktion för att implementera utlösarklickshanteraren genom att åsidosätta [onTriggerClick](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html). Du kan skapa en `TriggerField` direkt, eftersom den återges exakt som en kombinationsruta.

* `uploaddialog`

  [CQ.UploadDialog](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Med `UploadDialog` kan användaren överföra filer till databasen Skapar en ny UploadDialog.

* `userinfo`

  [CQ.UserInfo](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  Verktygsfältsobjekt som visar det aktuella användarnamnet och tillåter användaråtgärder som att redigera användaregenskaper och personifiering.

* `viewport`

  [CQ.Ext.Viewport](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En speciell behållare som representerar det visningsbara programområdet (webbläsarens visningsruta).

  `Viewport` återger sig själv till dokumentets brödtext och anpassar sig automatiskt till storleken på webbläsarens visningsruta och hanterar fönsterstorleksändring. Det kan bara finnas en visningsport som har skapats.

* `window`

  [CQ.Ext.Window](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En specialpanel som är avsedd att användas som programfönster. Windows är flytande, [storleksändras](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) och [dragbara](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) som standard. Windows kan vara [maximerat](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html) för att fylla visningsrutan, återställa den tidigare storleken och kan vara [minimize](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)d.

* `xmlstore`

  [CQ.Ext.data.XmlStore](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)

  En liten hjälpklass som gör det enklare att skapa [CQ.Ext.data.Store](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html)s från XML-data. En `XmlStore` konfigureras automatiskt med [&#x200B; CQ.Ext.data.XmlReader](https://developer.adobe.com/experience-manager/reference-materials/6-5/widgets-api/index.html).

  `cqinclude` - en pseudoxtyp som innehåller widgetdefinitioner från en annan sökväg i databasen. Det används oftast i siddialogrutor. Det finns ingen JavaScript-widgetklass för den här typen av xtype. Klassen `CQ.Util` bearbetar den med funktionen `formatData()`.
