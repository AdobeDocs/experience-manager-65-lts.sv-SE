---
title: Använda bibliotek på klientsidan
description: AEM tillhandahåller biblioteksmappar på klientsidan, där du kan lagra klientsidans kod i databasen, ordna den i kategorier och definiera när och hur varje kodkategori ska skickas till klienten
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing,Personalization
role: Developer
exl-id: cafc7120-114e-487a-8b81-9c695318731e
source-git-commit: a061c19dcb883b94ee61be21459c46e21eaf696a
workflow-type: tm+mt
source-wordcount: '2791'
ht-degree: 0%

---

# Använda bibliotek på klientsidan{#using-client-side-libraries}

Moderna webbplatser är starkt beroende av bearbetning på klientsidan som styrs av komplex JavaScript- och CSS-kod. Det kan vara komplicerat att organisera och optimera serveringen av den här koden.

AEM tillhandahåller **Biblioteksmappar på klientsidan** som du kan använda för att lagra din klientkod i databasen, ordna den i kategorier och definiera när och hur varje kodkategori ska skickas till klienten. Klientsidans bibliotekssystem tar sedan hand om att skapa rätt länkar på den slutliga webbsidan för att läsa in rätt kod.

## Hur klientbibliotek fungerar i AEM {#how-client-side-libraries-work-in-aem}

Standardsättet att inkludera ett klientbibliotek (dvs. en JS- eller CSS-fil) i en sidas HTML är helt enkelt att ta med en `<script>` - eller `<link>` -tagg i JSP-filen för den sidan, som innehåller sökvägen till filen i fråga. Exempel:

```xml
...
<head>
   ...
   <script type="text/javascript" src="/etc/clientlibs/granite/jquery/source/1.8.1/jquery-1.8.1.js"></script>
   ...
</head>
...
```

Detta tillvägagångssätt fungerar i AEM, men kan leda till problem när sidor och deras beståndsdelar blir komplexa. I sådana fall finns det en risk för att flera kopior av samma JS-bibliotek kan inkluderas i den slutliga HTML-utskriften. För att undvika detta och för att tillåta logisk organisation av klientbibliotek använder AEM **biblioteksmappar på klientsidan**.

En biblioteksmapp på klientsidan är en databasnod av typen `cq:ClientLibraryFolder`. Dess definition i [CND-notation](https://jackrabbit.apache.org/node-type-notation.html) är

```shell
[cq:ClientLibraryFolder] > sling:Folder
  - dependencies (string) multiple
  - categories (string) multiple
  - embed (string) multiple
  - channels (string) multiple
```

Som standard kan `cq:ClientLibraryFolder`-noder placeras var som helst i underträden `/apps`, `/libs` och `/etc` i databasen (dessa standardinställningar och andra inställningar kan styras via panelen **Adobe Granite HTML Library Manager** i [Systemkonsolen](https://localhost:4502/system/console/configMgr)).

Varje `cq:ClientLibraryFolder` fylls i med en uppsättning JS- och/eller CSS-filer, tillsammans med några stödfiler (se nedan). Egenskaperna för `cq:ClientLibraryFolder` är konfigurerade enligt följande:

* `categories`: Identifierar de kategorier som uppsättningen med JS- och/eller CSS-filer i `cq:ClientLibraryFolder` hamnar i. Egenskapen `categories`, som är flervärd, gör att en biblioteksmapp kan ingå i mer än en kategori (se nedan för hur detta kan vara användbart).

* `dependencies`: Det här är en lista över andra klientbibliotekskategorier som den här biblioteksmappen är beroende av. Om till exempel två `cq:ClientLibraryFolder`-noder `F` och `G` kräver en annan fil i `F` för att den ska fungera på rätt sätt i `G` måste minst en av `categories` i `G` finnas bland `dependencies` i `F` .

* `embed`: Används för att bädda in kod från andra bibliotek. Om nod F bäddar in noderna G och H blir resultatet för HTML en innehållskoncentration från noderna G och H.
* `allowProxy`: Om ett klientbibliotek finns under `/apps` tillåter den här egenskapen åtkomst till det via en proxyserver. Se [Hitta en biblioteksmapp för klient och Använda servern för proxyklientbibliotek](/help/sites-developing/clientlibs.md#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) nedan.

## Referera till bibliotek på klientsidan {#referencing-client-side-libraries}

Eftersom HTML är den rekommenderade tekniken för utveckling av AEM-webbplatser bör HTML användas för att inkludera klientbibliotek i AEM. Det går dock även att göra det med JSP.

### Använda HTML {#using-htl}

I HTML läses klientbibliotek in via en hjälpmall från AEM, som du kommer åt via [`data-sly-use`](https://helpx.adobe.com/se/experience-manager/htl/using/block-statements.html#use). Det finns tre mallar i den här filen som kan anropas via [`data-sly-call`](https://helpx.adobe.com/se/experience-manager/htl/using/block-statements.html#template-call):

* **css** - läser endast in CSS-filer för de refererade klientbiblioteken.
* **js** - Läser bara in JavaScript-filerna för de refererade klientbiblioteken.
* **all** - Läser in alla filer i de refererade klientbiblioteken (både CSS och JavaScript).

Varje hjälpmall förväntar sig ett `categories`-alternativ för att referera till de önskade klientbiblioteken. Det alternativet kan antingen vara en array med strängvärden eller en sträng som innehåller en kommaseparerad värdelista.

Mer information och exempel på användning finns i dokumentet [Komma igång med HTML mallspråk](https://helpx.adobe.com/se/experience-manager/htl/using/getting-started.html#loading-client-libraries).

### Använda JSP {#using-jsp}

Lägg till en `ui:includeClientLib`-tagg i JSP-koden om du vill lägga till en länk till klientbibliotek på den genererade HTML-sidan. Om du vill referera till biblioteken använder du värdet för `categories`-egenskapen för noden `ui:includeClientLib`.

```
<%@taglib prefix="ui" uri="https://www.adobe.com/taglibs/granite/ui/1.0" %>
<ui:includeClientLib categories="<%= categories %>" />
```

Noden `/etc/clientlibs/foundation/jquery` är till exempel av typen `cq:ClientLibraryFolder` med en kategoriegenskap med värdet `cq.jquery`. Följande kod i en JSP-fil refererar till biblioteken:

```xml
<ui:includeClientLib categories="cq.jquery"/>
```

Den genererade HTML-sidan innehåller följande kod:

```xml
<script type="text/javascript" src="/etc/clientlibs/foundation/jquery.js"></script>
```

Fullständig information, inklusive attribut för filtrering av JS-, CSS- eller temabibliotek finns i [ui:includeClientLib](/help/sites-developing/taglib.md#lt-ui-includeclientlib).

>[!CAUTION]
>
>`<cq:includeClientLib>`, som tidigare ofta användes för att inkludera klientbibliotek, har tagits bort sedan AEM 5.6. [`<ui:includeClientLib>`](/help/sites-developing/taglib.md#lt-ui-includeclientlib) bör användas i stället enligt beskrivningen ovan.

## Skapar klientbiblioteksmappar {#creating-client-library-folders}

Skapa en `cq:ClientLibraryFolder`-nod för att definiera JavaScript- och Cascading Style Sheet-bibliotek och göra dem tillgängliga för HTML-sidor. Använd egenskapen `categories` för noden för att identifiera de bibliotekskategorier som den tillhör.

Noden innehåller en eller flera källfiler som vid körning sammanfogas till en enda JS- och/eller CSS-fil. Den genererade filens namn är nodnamnet med filnamnstillägget `.js` eller `.css`. Till exempel resulterar biblioteksnoden `cq.jquery` i den genererade filen `cq.jquery.js` eller `cq.jquery.css`.

Klientbiblioteksmappar innehåller följande objekt:

* JS- och/eller CSS-källfilerna som ska sammanfogas.
* Resurser som stöder CSS-format, t.ex. bildfiler.

  **Obs!** Du kan använda undermappar för att ordna källfiler.
* En `js.txt`-fil och/eller en `css.txt`-fil som identifierar de källfiler som ska sammanfogas i de genererade JS- och/eller CSS-filerna.

![clientlibarch](assets/clientlibarch.png)

Mer information om krav som är specifika för klientbibliotek för widgetar finns i [Använda och utöka widgetar](/help/sites-developing/widgets.md).

Webbklienten måste ha behörighet att komma åt noden `cq:ClientLibraryFolder`. Du kan också visa bibliotek från skyddade områden i databasen (se Inbädda kod från andra bibliotek nedan).

### Åsidosätta bibliotek i /lib {#overriding-libraries-in-lib}

Klientbiblioteksmappar som finns under `/apps` åsidosätter mappar med samma namn som finns i `/libs`. `/apps/cq/ui/widgets` har till exempel företräde framför `/libs/cq/ui/widgets`. När dessa bibliotek tillhör samma kategori används biblioteket under `/apps`.

### Hitta en klientbiblioteksmapp och använda servern för proxyklientbibliotek {#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet}

I tidigare versioner fanns klientbiblioteksmapparna under `/etc/clientlibs` i databasen. Detta stöds fortfarande, men vi rekommenderar att klientbibliotek nu finns under `/apps`. Detta görs för att hitta klientbiblioteken nära de andra skripten, som vanligtvis finns under `/apps` och `/libs`.

>[!NOTE]
>
>Statiska resurser under klientbiblioteksmappen måste finnas i en mapp med namnet *resources*. Om du inte har statiska resurser, till exempel bilder, under mappen *resources* kan det inte refereras till i en publiceringsinstans. Här är ett exempel: https://localhost:4503/etc.clientlibs/geometrixx/components/clientlibs/resources/example.gif

>[!NOTE]
>
>Om du vill isolera kod bättre från innehåll och konfiguration bör du leta upp klientbibliotek under `/apps` och visa dem via `/etc.clientlibs` med egenskapen `allowProxy`.

För att klientbiblioteken under `/apps` ska vara tillgängliga används en proxyserver. Åtkomstkontrollistorna används fortfarande i klientbiblioteksmappen, men med den kan innehållet läsas via `/etc.clientlibs/` om egenskapen `allowProxy` är inställd på `true`.

En statisk resurs kan bara nås via proxyn om den finns under en resurs under klientbiblioteksmappen.

Exempel:

* Du har ett klientlib i `/apps/myprojects/clientlibs/foo`
* Du har en statisk bild i `/apps/myprojects/clientlibs/foo/resources/icon.png`

Sedan ställer du in egenskapen `allowProxy` för `foo` på true.

* Du kan sedan begära `/etc.clientlibs/myprojects/clientlibs/foo.js`
* Du kan sedan referera till bilden via `/etc.clientlibs/myprojects/clientlibs/foo/resources/icon.png`

>[!CAUTION]
>
>När du använder proxyanslutna klientbibliotek kan AEM Dispatcher-konfigurationen kräva en uppdatering för att säkerställa att URI:er med tilläggsklienter tillåts.

>[!CAUTION]
>
>Adobe rekommenderar att du hittar klientbibliotek under `/apps` och gör dem tillgängliga med proxyservern. Tänk dock på att bästa praxis fortfarande kräver att offentliga webbplatser aldrig inkluderar något som opereras direkt över en `/apps`- eller `/libs`-sökväg.

### Skapa en biblioteksmapp för klient {#create-a-client-library-folder}

1. Öppna CRXDE Lite i en webbläsare ([https://localhost:4502/crx/de](https://localhost:4502/crx/de)).
1. Markera den mapp där du vill hitta klientbiblioteksmappen och klicka på **Skapa > Skapa nod**.
1. Ange ett namn för biblioteksfilen och välj `cq:ClientLibraryFolder` i typlistan. Klicka på **OK** och sedan på **Spara alla**.
1. Om du vill ange kategorin eller kategorierna som biblioteket tillhör markerar du noden `cq:ClientLibraryFolder`, lägger till följande egenskap och klickar sedan på **Spara alla**:

   * Namn: kategorier
   * Typ: String
   * Värde: Kategorinamnet
   * Flera: Markera

1. Lägg till källfiler i biblioteksmappen på alla sätt. Använd till exempel en WebDav-klient för att kopiera filer, eller skapa en fil och redigera innehållet manuellt.

   **Obs!** Du kan ordna källfiler i undermappar om du vill.

1. Markera klientbiblioteksmappen och klicka på **Skapa > Skapa fil**.
1. Skriv något av följande filnamn i rutan Filnamn och klicka på OK:

   * **`js.txt`:** Använd det här filnamnet för att skapa en JavaScript-fil.
   * **`css.txt`:** Använd det här filnamnet för att generera en CSS (Cascading Style Sheet).

1. Öppna filen och skriv följande text för att identifiera källfilernas rot:

   `#base=*[root]*`

   Ersätt * `[root]`* med sökvägen till mappen som innehåller källfilerna i förhållande till TXT-filen. Använd till exempel följande text när källfilerna finns i samma mapp som TXT-filen:

   `#base=.`

   Följande kod anger roten som mappen mobile under noden `cq:ClientLibraryFolder`:

   `#base=mobile`

1. På raderna under `#base=[root]` skriver du sökvägarna för källfilerna i förhållande till roten. Placera filnamnen på separata rader.
1. Klicka på **Spara alla**.

### Länka till beroenden {#linking-to-dependencies}

När koden i klientbiblioteksmappen refererar till andra bibliotek identifierar du de andra biblioteken som beroenden. I JSP-filen innehåller taggen `ui:includeClientLib` som refererar till din klientbiblioteksmapp en länk till den biblioteksfil som genererats och beroendena.

Beroenden måste vara en annan `cq:ClientLibraryFolder`. Om du vill identifiera beroenden lägger du till en egenskap i noden `cq:ClientLibraryFolder` med följande attribut:

* **Namn:** beroenden
* **Typ:** String[]
* **Värden:** Värdet på kategoriegenskapen i cq:ClientLibraryFolder-noden som den aktuella biblioteksmappen är beroende av.

/ `etc/clientlibs/myclientlibs/publicmain` är till exempel beroende av biblioteket `cq.jquery`. Den JSP som refererar till huvudklientbiblioteket genererar HTML som innehåller följande kod:

```xml
<script src="/etc/clientlibs/foundation/cq.jquery.js" type="text/javascript">
<script src="/etc/clientlibs/mylibs/publicmain.js" type="text/javascript">
```

### Bädda in kod från andra bibliotek {#embedding-code-from-other-libraries}

Du kan bädda in kod från ett klientbibliotek i ett annat klientbibliotek. Vid körning innehåller de genererade JS- och CSS-filerna för inbäddningsbiblioteket koden för det inbäddade biblioteket.

Inbäddning av kod är användbart för att ge åtkomst till bibliotek som lagras i skyddade områden i databasen.

#### Appspecifika klientbiblioteksmappar {#app-specific-client-library-folders}

Det är en god vana att behålla alla programrelaterade filer i programmappen under `/apps`. Det är också en god vana att neka åtkomst för webbplatsbesökare till mappen `/apps`. Om du vill följa båda de bästa metoderna skapar du en klientbiblioteksmapp under `/apps` och gör den åtkomlig via proxyservern enligt beskrivningen i [Hitta en klientbiblioteksmapp och Använda proxyklientbiblioteksservern](/help/sites-developing/clientlibs.md#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

Använd egenskapen categories för att identifiera klientbiblioteksmappen som ska bäddas in. Om du vill bädda in biblioteket lägger du till en egenskap i noden `cq:ClientLibraryFolder` med följande egenskapsattribut:

* **Namn:** bädda in
* **Typ:** String[]
* **Värde:** Värdet på kategoriegenskapen för noden `cq:ClientLibraryFolder` som ska bäddas in.

#### Använda inbäddning för att minimera begäranden {#using-embedding-to-minimize-requests}

I vissa fall kan du upptäcka att den slutliga HTML som genereras för den typiska sidan av din publiceringsinstans innehåller ett relativt stort antal `<script>`-element, särskilt om din webbplats använder klientkontextinformation för analys eller målanpassning. I ett icke-optimerat projekt kan du till exempel hitta följande serie med `<script>`-element i HTML för en sida:

```xml
<script type="text/javascript" src="/etc/clientlibs/granite/jquery.js"></script>
<script type="text/javascript" src="/etc/clientlibs/granite/utils.js"></script>
<script type="text/javascript" src="/etc/clientlibs/granite/jquery/granite.js"></script>
<script type="text/javascript" src="/etc/clientlibs/foundation/jquery.js"></script>
<script type="text/javascript" src="/etc/clientlibs/foundation/shared.js"></script>
<script type="text/javascript" src="/etc/clientlibs/foundation/personalization/kernel.js"></script>
```

I sådana fall kan det vara användbart att kombinera all nödvändig klientbibliotekskod till en enda fil så att antalet fram- och tillbaka-begäranden vid sidinläsning minskar. För att göra detta kan du `embed` de nödvändiga biblioteken i ditt programspecifika klientbibliotek med hjälp av inbäddningsegenskapen för noden `cq:ClientLibraryFolder` .

Följande klientbibliotekskategorier ingår i AEM. Du bör endast bädda in de som krävs för att din webbplats ska fungera. **Du bör dock behålla ordningen som anges här**:

1. `browsermap.standard`
1. `browsermap`
1. `jquery-ui`
1. `cq.jquery.ui`
1. `personalization`
1. `personalization.core`
1. `personalization.core.kernel`
1. `personalization.clientcontext.kernel`
1. `personalization.stores.kernel`
1. `personalization.kernel`
1. `personalization.clientcontext`
1. `personalization.stores`
1. `cq.collab.comments`
1. `cq.collab.feedlink`
1. `cq.collab.ratings`
1. `cq.collab.toggle`
1. `cq.collab.forum`
1. `cq.cleditor`

#### Sökvägar i CSS-filer {#paths-in-css-files}

När du bäddar in CSS-filer använder den genererade CSS-koden sökvägar till resurser som är relativa till inbäddningsbiblioteket. Det offentligt tillgängliga biblioteket `/etc/client/libraries/myclientlibs/publicmain` bäddar till exempel in klientbiblioteket `/apps/myapp/clientlib`:

![screen_shot_2012-05-29at20122pm](assets/screen_shot_2012-05-29at20122pm.png)

Filen `main.css` innehåller följande format:

```xml
body {
  padding: 0;
  margin: 0;
  background: url(images/bg-full.jpg) no-repeat center top;
  width: 100%;
}
```

CSS-filen som genereras av noden `publicmain` innehåller följande format med den ursprungliga bildens URL:

```xml
body {
  padding: 0;
  margin: 0;
  background: url(../../../apps/myapp/clientlib/styles/images/bg-full.jpg) no-repeat center top;
  width: 100%;
}
```

### Använda ett bibliotek för specifika mobilgrupper {#using-a-library-for-specific-mobile-groups}

Använd egenskapen `channels` i en klientbiblioteksmapp för att identifiera den mobilgrupp som använder biblioteket. Egenskapen `channels` är användbar när bibliotek i samma kategori har utformats för olika enhetsfunktioner.

Om du vill associera en klientbiblioteksmapp med en enhetsgrupp lägger du till en egenskap i noden `cq:ClientLibraryFolder` med följande attribut:

* **Namn:** kanaler
* **Typ:** String[]
* **Värden:** Namnet på mobilgruppen. Om du vill utesluta biblioteksmappen från en grupp anger du ett utropstecken (&quot;!&quot;) för namnet.

I följande tabell visas värdet för egenskapen `channels` för varje klientbiblioteksmapp i kategorin `cq.widgets`:

| Klientbiblioteksmapp | Värde för kanalegenskap |
|---|---|
| `/libs/cq/analytics/widgets` | `!touch` |
| `/libs/cq/analytics/widgets/themes/default` | `!touch` |
| `/libs/cq/cloudserviceconfigs/widgets` | `!touch` |
| `/libs/cq/touch/widgets` | `touch` |
| `/libs/cq/touch/widgets/themes/default` | `touch` |
| `/libs/cq/ui/widgets` | `!touch` |
| `/libs/cq/ui/widgets/themes/default` | `!touch` |

<!-- Search&Promote is end of life as of September 1, 2022 | `/libs/cq/searchpromote/widgets` | `!touch` | -->
<!-- Search&Promote is end of life as of September 1, 2022 | `/libs/cq/searchpromote/widgets/themes/default` |*[no value]* -->

## Använda preprocessorer {#using-preprocessors}

AEM tillåter anslutningsbara preprocessorer och levereras med stöd för [YUI Compressor](https://github.com/yui/yuicompressor#yui-compressor---the-yahoo-javascript-and-css-compressor) för CSS och JavaScript samt [Google Closure Compiler (GCC)](https://developers.google.com/closure/compiler/) för JavaScript med YUI inställt som AEM standardpreprocessor.

De anslutningsbara preprocessorerna möjliggör flexibel användning, inklusive:

* Definiera ScriptProcessors som kan bearbeta skriptkällor
* Processorer kan konfigureras med alternativ
* Processorer kan användas för miniatyrbilder, men även för icke-miniatyrärenden
* clientlib kan definiera vilken processor som ska användas

>[!NOTE]
>
>Som standard använder AEM YUI-kompressor. En lista över kända fel finns i [YUI Compressor GitHub-dokumentationen](https://github.com/yui/yuicompressor/issues). Om du växlar till GCC-komprimerare för vissa klienter kan vissa problem som uppstår när du använder YUI lösas.

>[!CAUTION]
>
>Placera inte ett miniatyrbibliotek i ett klientbibliotek. Ange i stället Raw-biblioteket och om miniatyrbilder krävs använder du alternativen för preprocessorerna.

### Användning {#usage}

Du kan välja att konfigurera preprocessorer-konfigurationen per klientbibliotek eller system i hela systemet.

* Lägg till flervärdesegenskaperna `cssProcessor` och `jsProcessor` på klientbiblioteksnoden

* Eller definiera systemstandardkonfigurationen via OSGi-konfigurationen för **HTML Library Manager**

En preprocessorkonfiguration på klientlib-noden har företräde framför OSGI-konfigurationen.

### Format och exempel {#format-and-examples}

#### Format {#format}

```xml
config:= mode ":" processorName options*;
mode:= "default" | "min";
processorName := "none" | <name>;
options := ";" option;
option := name "=" value;
```

#### YUI-kompressor för CSS Minification och GCC för JS {#yui-compressor-for-css-minification-and-gcc-for-js}

```xml
cssProcessor: ["default:none", "min:yui"]
jsProcessor: ["default:none", "min:gcc;compilationLevel=advanced"]
```

#### Typescript to Preprocess and then GCC to Minify and Obfuscate {#typescript-to-preprocess-and-then-gcc-to-minify-and-obfuscate}

```xml
jsProcessor: [
   "default:typescript",
   "min:typescript",
   "min:gcc;obfuscate=true"
]
```

#### Fler GCC-alternativ {#additional-gcc-options}

```xml
failOnWarning (defaults to "false")
languageIn (defaults to "ECMASCRIPT5")
languageOut (defaults to "ECMASCRIPT5")
compilationLevel (defaults to "simple") (can be "whitespace", "simple", "advanced")
```

Mer information om GCC-alternativ finns i [GCC-dokumentationen](https://developers.google.com/closure/compiler/docs/compilation_levels).

### Ange systemstandardminiatyr {#set-system-default-minifier}

YUI anges som standardminiprogram i AEM. Följ de här stegen för att ändra detta till GCC.

1. Gå till Apache Felix Config Manager på [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr)
1. Hitta och redigera **Adobe Granite HTML Library Manager**.
1. Aktivera alternativet **Minify** (om det inte redan är aktiverat).
1. Ange värdet **JS-processorns standardkonfigurationer** till `min:gcc`.

   Alternativ kan skickas om de avgränsas med ett semikolon, till exempel `min:gcc;obfuscate=true`.

1. Klicka på **Spara** för att spara ändringarna.

## Felsökningsverktyg {#debugging-tools}

AEM innehåller flera verktyg för felsökning och testning av klientbiblioteksmappar.

### Se Inbäddade filer {#see-embedded-files}

Om du vill spåra ursprunget för inbäddad kod eller se till att inbäddade klientbibliotek ger det förväntade resultatet, kan du se namnen på de filer som bäddas in under körning. Om du vill visa filnamnen lägger du till parametern `debugClientLibs=true` i webbsidans URL. Biblioteket som skapas innehåller `@import` programsatser i stället för den inbäddade koden.

I exemplet i det föregående avsnittet [Embedding Code From Other Libraries](/help/sites-developing/clientlibs.md#embedding-code-from-other-libraries) bäddar klientbiblioteksmappen `/etc/client/libraries/myclientlibs/publicmain` in klientbiblioteksmappen `/apps/myapp/clientlib` . Om du lägger till parametern på webbsidan skapas följande länk i webbsidans källkod:

```xml
<link rel="stylesheet" href="/etc/clientlibs/mycientlibs/publicmain.css">
```

Om du öppnar filen `publicmain.css` visas följande kod:

```xml
@import url("/apps/myapp/clientlib/styles/main.css");
```

1. Lägg till följande text i webbadressen för din HTML i webbläsarens adressruta:

   `?debugClientLibs=true`
1. Visa sidans källa när sidan läses in.
1. Klicka på länken som anges som href för länkelementet för att öppna filen och visa källkoden.

### Identifiera klientbibliotek {#discover-client-libraries}

Komponenten `/libs/cq/granite/components/dumplibs/dumplibs` genererar en sida med information om alla klientbiblioteksmappar i systemet. Noden `/libs/granite/ui/content/dumplibs` har komponenten som en resurstyp. Om du vill öppna sidan använder du följande URL (ändra värd och port efter behov):

`https://<host>:<port>/libs/granite/ui/content/dumplibs.test.html`

Informationen omfattar bibliotekets sökväg och typ (CSS eller JS) samt värdena för biblioteksattributen, t.ex. kategorier och beroenden. Efterföljande tabeller på sidan visar biblioteken i varje kategori och kanal.

### Se genererade utdata {#see-generated-output}

Komponenten `dumplibs` innehåller en testväljare som visar den källkod som genereras för `ui:includeClientLib` -taggar. Sidan innehåller kod för olika kombinationer av js-, css- och temaattribut.

1. Använd någon av följande metoder för att öppna sidan Testa utdata:

   * Klicka på länken i texten **Klicka här för utdatatestning** på sidan `dumplibs.html`.

   * Öppna följande URL i webbläsaren (använd en annan värd och port efter behov):

      * `http://<host>:<port>/libs/granite/ui/content/dumplibs.html`

   Standardsidan visar utdata för taggar utan värde för attributet categories.

1. Om du vill visa utdata för en kategori anger du värdet för klientbibliotekets `categories`-egenskap och klickar på **Skicka fråga**.

## Konfigurera bibliotekshantering för utveckling och produktion {#configuring-library-handling-for-development-and-production}

HTML Library Manager-tjänsten bearbetar `cq:ClientLibraryFolder`-taggar och genererar biblioteken vid körning. Typ av miljö, utveckling eller produktion, avgör hur du ska konfigurera tjänsten:

* Öka säkerheten: Inaktivera felsökning
* Förbättra prestanda: Ta bort tomt utrymme och komprimera bibliotek.
* Förbättra läsbarheten: Inkludera tomt utrymme och komprimera inte.

Mer information om hur du konfigurerar tjänsten finns i [AEM HTML Library Manager](/help/sites-deploying/osgi-configuration-settings.md#aemhtmllibrarymanager).
