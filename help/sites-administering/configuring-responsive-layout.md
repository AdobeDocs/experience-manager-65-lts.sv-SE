---
title: Konfigurera layoutbehållare och layoutläge
description: Lär dig hur du konfigurerar layoutbehållaren och layoutläget.
topic-tags: operations
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Operations
role: Admin
exl-id: 413f15c9-5b51-4d8d-8cf0-3e98608b9d9e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1389'
ht-degree: 0%

---

# Konfigurera layoutbehållare och layoutläge{#configuring-layout-container-and-layout-mode}

Lär dig hur du konfigurerar layoutbehållaren och layoutläget.

>[!TIP]
>
>Det här dokumentet innehåller en översikt över responsiv design för webbplatsadministratörer och utvecklare som beskriver hur funktioner implementeras i AEM.
>
>För innehållsförfattare finns information om hur du använder responsiva designfunktioner på en innehållssida i dokumentet [Responsiv layout för dina innehållssidor.](/help/sites-authoring/responsive-layout.md)

## Ökning {#overview}

[Responsiv layout](/help/sites-authoring/responsive-layout.md) är en mekanism för att förverkliga [responsiv webbdesign](https://en.wikipedia.org/wiki/Responsive_web_design). På så sätt kan användaren skapa webbsidor som har en layout och dimensioner som är beroende av vilka enheter som användarna använder.

AEM implementerar en responsiv layout för dina sidor med en kombination av mekanismer:

* [**Layoutbehållare**](/help/sites-authoring/responsive-layout.md#adding-a-layout-container-and-its-content-edit-mode)-komponent

  Den här komponenten har ett rutnätsstyckesystem där du kan lägga till och placera komponenter i ett responsivt rutnät. Den kan användas som standardparsyta för sidan och/eller göras tillgänglig för författare i komponentwebbläsaren.

   * Standardkomponenten för **Layoutbehållare** definieras under:

     /libs/wcm/foundation/components/responsivegrid

   * Du kan definiera layoutbehållare:

      * Som en komponent som användaren kan lägga till på en sida.
      * Som standardparsys för sidan.
      * Båda.

        Du kan ha layoutbehållaren som standard för sidan, samtidigt som användaren kan lägga till fler layoutbehållare i den, till exempel för att uppnå kolumnkontroll.

* **[Layoutläge](/help/sites-authoring/responsive-layout.md#defining-layouts-layout-mode)**
När layoutbehållaren har placerats på sidan kan du använda **layoutläget** för att placera innehåll i det responsiva rutnätet.

* [**Emulator**](/help/sites-authoring/responsive-layout.md#selecting-a-device-to-emulate)
På så sätt kan du skapa och redigera responsiva webbplatser som ändrar layouten beroende på enhetens/fönstrets storlek genom att ändra komponenternas storlek interaktivt. Användaren kan sedan se hur innehållet återges med hjälp av emulatorn.

Med dessa responsiva rutnätsmekanismer kan du:

* Använd brytpunkter (som indikerar enhetsgruppering) för att definiera olika innehållsbeteenden baserat på enhetslayout.
* Dölj komponenter baserat på enhetsgrupp (definiera på vilken brytpunkt en komponent ska döljas).
* Använd vågrät fäst mot rutnät (placera komponenter i rutnätet, ändra storlek efter behov, definiera när de ska komprimeras/omformas så att de ligger sida vid sida eller ovanför/nedanför).
* Uppnå kolumnkontroll.

>[!TIP]
>
>Adobe tillhandahåller [GitHub-dokumentation](https://adobe-marketing-cloud.github.io/aem-responsivegrid/) för den responsiva layouten som en referens som kan ges till gränssnittsutvecklare så att de kan använda AEM-rutnätet utanför AEM, till exempel när de skapar statiska HTML-modeller för en framtida AEM-webbplats.

>[!NOTE]
>
>I en körklar installation har responsiv layout konfigurerats för referenswebbplatsen [We.Retail](/help/sites-developing/we-retail.md). [Aktivera layoutbehållarkomponenten](#enable-the-layout-container-component-for-page) för andra sidor.

>[!CAUTION]
>
>Även om komponenten **Layoutbehållare** är tillgänglig i det klassiska användargränssnittet, är dess fullständiga funktioner bara tillgängliga i det beröringsaktiverade användargränssnittet.

## Aktivera layoutläge för webbplatsen {#activate-layout-mode-for-your-site}

Dessa procedurer används för att aktivera **layoutläget** på din plats.

### Konfigurera brytpunkterna {#configure-the-breakpoints}

[Brytpunkter](/help/sites-authoring/responsive-layout.md#selecting-a-device-to-emulate):

* Används i responsiv design.
* Kan definieras:

   * På sidmallen, från vilken inställningarna kopieras till alla sidor som skapas med den mallen.
   * På sidnoden, varifrån inställningarna ärvs av underordnade sidor.

* Definiera en titel och en bredd:

   * Titeln beskriver den allmänna enhetsgrupperingen, med orientering om det behövs, till exempel telefon, surfplatta, bordslandskap.
   * Bredden definierar den maximala bredden i pixlar för den allmänna enhetsgrupperingen. Om till exempel brytpunktstelefonen har en bredd på 768 är det den maximala bredden för den layout som används för en telefonenhet.

* Är synliga som markörer högst upp i sidredigeraren när du använder emulatorn.
* Ärvs från den överordnade nodhierarkin och kan åsidosättas när som helst.
* Det finns en standardbrytpunkt (som inte finns i lådan) som täcker allt över den senaste *konfigurerade* brytpunkten.

De kan definieras med CRXDE Lite eller XML.

>[!NOTE]
>
>Om du skapar ett nytt projekt:
>
>* lägg till brytpunkter i mallarna.
>
>Om du migrerar ett befintligt projekt (med befintligt innehåll) måste du:
>
>* lägga till brytpunkter i mallar
>* lägga till samma brytpunkter på befintliga sidor
>
>  När arv används kan du begränsa detta till innehållets rotsida.

#### Konfigurera brytpunkter med CRXDE Lite {#configuring-breakpoints-using-crxde-lite}

1. Använd CRXDE Lite (eller motsvarande) för att navigera till:

   * Din malldefinition.
   * Sidans `jcr:content`-nod.

1. Skapa noden under `jcr:content`:

   * Namn: `cq:responsive`
   * Typ: `nt:unstructured`

1. Skapa noden här:

   * Namn: `breakpoints`
   * Typ: `nt:unstructured`

1. Under brytpunktsnoden kan du skapa valfritt antal brytpunkter. Varje definition är en enda nod med följande egenskaper:

   * Namn: `<descriptive name>`
   * Typ: `nt:unstructured`
   * Titel: `String` * `<descriptive title seen in Emulator>`*
   * Bredd: `Decimal` * `<value of breakpoint>`*

#### Konfigurera brytpunkter med XML {#configuring-breakpoints-using-xml}

Brytpunkter finns i avsnittet `<jcr:content>` i `.context.html` under lämplig mallmapp (eller innehållsmapp).

En exempeldefinition:

```xml
<cq:responsive jcr:primaryType="nt:unstructured">
  <breakpoints jcr:primaryType="nt:unstructured">
    <phone jcr:primaryType="nt:unstructured" title="{String}Phone" width="{Decimal}768"/>
    <tablet jcr:primaryType="nt:unstructured" title="{String}Tablet" width="{Decimal}1200"/>
  </breakpoints>
</cq:responsive>
```

### Lägg till en leverantör av responsiv information {#add-a-responsive-information-provider}

>[!NOTE]
>
>Detta behövs bara om sidkomponenten inte är baserad på bassidans komponent.

Kopiera följande `cq:infoProviders`-nodstruktur till den överordnade sidkomponenten:

`/libs/foundation/components/page/cq:infoProviders/responsive`

## Aktivera komponentstorleksändring för sidan {#enable-component-resizing-for-the-page}

Dessa procedurer krävs så att du kan ändra storlek på komponenter i **layoutläget**.

### Ange layoutbehållare som huvudparsys {#set-layout-container-as-main-parsys}

Om du vill ange att huvudparsytan på sidan ska vara en layoutbehållare definierar du parsytorna som:

`wcm/foundation/components/responsivegrid`

I antingen

* Sidkomponent
* Sidmall (för framtida bruk)

I följande två exempel illustreras definitionen:

* **HTML:**

  ```html
  <sly data-sly-resource="${'par' @ resourceType='wcm/foundation/components/responsivegrid'}/>
  ```

* **JSP:**

  ```html
  <cq:include path="par" resourceType="wcm/foundation/components/responsivegrid" />
  ```

### Inkludera responsiv CSS {#include-the-responsive-css}

#### CSS för brytpunkter med LESS {#css-for-breakpoints-using-less}

AEM använder LESS för att generera delar av den CSS som behövs, och dessa måste ingå i dina projekt.

Du måste också skapa ett [klientbibliotek](https://experienceleague.adobe.com/docs/) för att kunna tillhandahålla ytterligare konfigurations- och funktionsanrop. Följande LESS-extrakt är ett exempel på det minsta som du måste lägga till i projektet:

```css
@import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";

/* maximum amount of grid cells to be provided */
@max_col: 12;

/* default breakpoint */
.aem-Grid {
  .generate-grid(default, @max_col);
}

/* phone breakpoint */
@media (max-width: 768px) {
  .aem-Grid {
    .generate-grid(phone, @max_col);
  }
}

/* tablet breakpoint */
@media (min-width: 769px) and (max-width: 1200px) {
  .aem-Grid {
    .generate-grid(tablet, @max_col);
  }
}
```

Basrutnätsdefinitionen finns under:

`/libs/wcm/foundation/clientlibs/grid/grid_base.less`

#### Fundera på formatering {#styling-considerations}

Storleken på komponenter som finns i en responsiv behållare ändras (tillsammans med deras respektive HTML DOM-element) beroende på stödrastrets responsiva storlek. Därför bör du under dessa omständigheter undvika (eller uppdatera) definitioner av DOM-element med fast bredd (som finns).

Till exempel:

* Före:

   * `width=100px`

* Efter:

   * `max-width=100px`

#### Storleksändring och adaptiv bildkompatibilitet {#resizing-and-adaptive-image-compliance}

Om du ändrar storlek på en komponent i rutnätet utlöses följande avlyssnare om det är lämpligt:

* `beforeedit`
* `beforechildedit`
* `afteredit`

* `afterchildedit`

Om du vill ändra storlek på och uppdatera innehållet i en adaptiv bild som ingår i ett responsivt stödraster måste du lägga till en `afterEdit`-uppsättning till `REFRESH_PAGE`-avlyssnare i `EditConfig`-filen för alla ingående komponenter.

Till exempel:

`<cq:listeners jcr:primaryType="cq:EditListenersConfig" afteredit="REFRESH_PAGE" />`

Den adaptiva bildmekanismen görs tillgänglig via ett skript som styr valet av rätt bild för fönstrets aktuella storlek. Den aktiveras när DOM är redo eller när en dedikerad händelse tas emot. För närvarande måste sidan uppdateras för att korrekt återspegla resultatet av användarens åtgärd.

>[!CAUTION]
>
>Klientlibs för anpassade formatmallar måste läsas in som en del av sidhuvudet för att de ska fungera på rätt sätt när de skapas och publiceras.

## Aktivera layoutbehållarkomponenten för sidan {#enable-the-layout-container-component-for-page}

Med de här åtgärderna kan författare dra instanser av komponenten **Layoutbehållare** till sidan.

### Aktivera layoutbehållarkomponenten för sidredigering {#enable-the-layout-container-component-for-page-editing}

Om du vill att författare ska kunna lägga till fler responsiva rutnät på innehållssidorna måste du aktivera layoutbehållarkomponenten för sidan. Du kan göra detta med:

* **Författarmiljö**

  Använd [designläge](/help/sites-authoring/default-components-designmode.md) för att aktivera komponenten **Lagerbehållare** för en sida.

* **Komponentdefinition**

  Använd `allowedComponent` eller en statisk include när du definierar komponenten.

### Konfigurera stödrastret för layoutbehållaren {#configure-the-grid-of-the-layout-container}

Du kan konfigurera antalet kolumner som är tillgängliga för varje särskild instans av layoutbehållaren:

1. **Författarmiljö**

   Du kan konfigurera antalet kolumner som är tillgängliga för varje enskild instans av layoutbehållaren.

   Det gör du genom att använda [designläget](/help/sites-authoring/default-components-designmode.md) och sedan öppna designdialogrutan för önskad behållare. Här kan du ange hur många kolumner som ska vara tillgängliga för placering och storleksändring. Standardvärdet är 12.

1. **XML**

   Definitioner för det responsiva rutnätet anges i:

   `etc/design/<*your-project-name*>/.content.xml`

   Följande parametrar kan definieras:

   * Antal tillgängliga kolumner:

      * `columns="{String}8"`

   * Komponenter som kan läggas till i den aktuella komponenten:

      * `components="[/libs/wcm/foundation/components/responsivegrid, ...`

## Kapslade responsiva stödraster {#nested-responsive-grids}

Det kan finnas tillfällen då du tycker att det är nödvändigt att kapsla in responsiva rutnät för att tillgodose ditt projekts behov. Tänk dock på att Adobe rekommenderade metod är att hålla strukturen så platt som möjligt.

När du inte kan undvika att använda kapslade responsiva stödraster bör du kontrollera följande:

* Alla behållare (behållare, flikar, dragspelspaneler osv.) har egenskapen `layout = responsiveGrid`.
* Blanda inte egenskapen `layout = simple` i behållarhierarkin.

Detta inkluderar alla strukturella behållare från sidmallen.

Kolumnnumret för den inre behållaren får aldrig vara större än det för den yttre behållaren. Följande exempel uppfyller det här villkoret. Medan kolumnnumret för den yttre behållaren är 8 för standardskärmen (skrivbordet) är kolumnnumret för den inre behållaren 4.

>[!BEGINTABS]

>[!TAB Exempel på nodstruktur]

```text
container
  @layout = responsiveGrid
  cq:responsive
    default
      @offset = 0
      @width = 8
  container
  @layout = responsiveGrid
    cq:responsive
      default
        @offset = 0
        @width = 4
    text
      @text =" Text Column 1"
```

>[!TAB Exempelresultat för HTML]

```html
<div class="container responsivegrid aem-GridColumn--default--none aem-GridColumn aem-GridColumn--default--8 aem-GridColumn--offset--default--0">
  <div id="container-c9955c233c" class="cmp-container">
    <div class="aem-Grid aem-Grid--8 aem-Grid--default--8 ">
      <div class="container responsivegrid aem-GridColumn--default--none aem-GridColumn aem-GridColumn--offset--default--0 aem-GridColumn--default--4">
        <div id="container-8414e95866" class="cmp-container">
          <div class="aem-Grid aem-Grid--4 aem-Grid--default--4 ">
            <div class="text aem-GridColumn aem-GridColumn--default--4">
              <div data-cmp-data-layer="..." id="text-1234567890" class="cmp-text">
                <p>Text Column 1</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

>[!ENDTABS]
