---
title: Utöka och konfigurera designimporteraren för landningssidor
description: Lär dig hur du konfigurerar designimporteraren för landningssidor.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: baf8bbbf4d3c27117a620d0ee0c799b8cc37582f
workflow-type: tm+mt
source-wordcount: '3442'
ht-degree: 0%

---

# Utöka och konfigurera designimporteraren för landningssidor{#extending-and-configuring-the-design-importer-for-landing-pages}

I det här avsnittet beskrivs hur du konfigurerar och vid behov utökar designimporteraren för landningssidor. Arbeta med landningssidor efter import beskrivs i [Landing Pages.](/help/sites-classic-ui-authoring/classic-personalization-campaigns-landingpage.md)

**Gör så att designimporteraren extraherar den anpassade komponenten**

Här följer de logiska stegen för att få designimporteraren att känna igen din anpassade komponent

1. Skapa en TagHandler

   * En tagghanterare är en POJO som hanterar HTML-taggar av en viss typ. Den typ av HTML-taggar som TagHandler kan hantera definieras via OSGi-egenskapen &quot;tagpattern.name&quot; i TagHandlerFactory. Den här OSGi-egenskapen är i stort sett en regex som ska matcha den HTML-indatatagg som du vill hantera. Alla kapslade taggar kastas till tagghanteraren för hantering. Om du till exempel registrerar dig för en div som innehåller en kapslad &lt;p>-tagg, genereras &lt;p>-taggen också till din TagHandler och det är upp till dig hur du vill ta hand om den.
   * Tagghanteringsgränssnittet liknar ett SAX-innehållshanterargränssnitt. Den tar emot SAX-händelser för varje html-tagg. Som tagghanterare måste du implementera vissa livscykelmetoder som anropas automatiskt av designimportramverket.

1. Skapa dess motsvarande TagHandlerFactory.

   * Tagghanterarfabriken är en OSGi-komponent (singleton) som ansvarar för att skapa förekomster av tagghanteraren.
   * din tagghanterarfabrik måste visa en OSGi-egenskap med namnet&quot;tagpattern.name&quot;, vars värde matchas mot HTML-taggen input.
   * Om det finns flera tagghanterare som matchar HTML-taggen för indata väljs den med en högre rankning. Själva rankningen visas som en OSGi-egenskap **service.ranking**.
   * TagHandlerFactory är en OSGi-komponent. Alla referenser som du vill ge till din TagHandler måste vara via den här fabriken.

1. Kontrollera att TagHandlerFactory har en bättre rankning om du vill åsidosätta standardinställningen.

>[!CAUTION]
>
>Design Importer, som används för att importera landningssidor, har ersatts med AEM 6.5.

## Förbereda HTML för import {#preparing-the-html-for-import}

När du har skapat en importsida kan du importera den fullständiga HTML-landningssidan. Om du vill importera HTML landningssida måste du först packa innehållet i ett designpaket. Designpaketet innehåller din HTML-landningssida tillsammans med de refererade resurserna (bilder, css, ikoner, skript och så vidare).

I följande exempelfil finns ett exempel på hur du förbereder din HTML för import:

Landing page Cheat Sheet

[Hämta fil](assets/cheatsheet.zip)

### Postfilens layout och krav {#zip-file-layout-and-requirements}

>[!NOTE]
>
>För närvarande kan ZIP-filer bara innehålla en HTML-sida eller en del av en sida.

En exempellayout för zip är följande:

* /index.html > landningssidans HTML-fil
* /css > för att lägga till i CSS-klientlib
* /img > alla bilder och resurser
* /js > för att lägga till i JS-klientlib

Layouten baseras på HTML5 Boilerplate best practices layout. Läs mer på [https://html5boilerplate.com/](https://html5boilerplate.com/)

>[!NOTE]
>
>Designpaketet **måste** innehålla minst en **index.html**-fil på rotnivån. Om landningssidan som ska importeras också har en mobilversion, måste postnumret innehålla en **mobile.index.html** tillsammans med **index.html** på rotnivån.

### Förbereda landningssidan för HTML {#preparing-the-landing-page-html}

För att kunna importera HTML måste du lägga till en arbetsyta-div på landningssidan HTML.

Arbetsytans div är en HTML **div** med `id="cqcanvas"` som måste infogas i HTML `<body>` -taggen och måste kapsla in det innehåll som ska konverteras.

Ett exempelavsnitt av landningssidan HTML efter tillägg av arbetsytans div är följande:

```xml
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title></title>
  <meta name="description" content="">
</head>
<body>
 <div id="cqcanvas">
  <!-- HTML content intended for conversion -->
 </div>
</body>
</html>
```

### Förbereda HTML för redigerbara AEM-komponenter {#preparing-the-html-to-include-editable-aem-components}

När du importerar en landningssida kan du välja att importera sidan i befintligt skick, vilket innebär att när landningssidan har importerats kan du inte redigera något av de importerade objekten i AEM (du kan fortfarande lägga till ytterligare AEM-komponenter på sidan).

Innan du importerar landningssidan kanske du vill konvertera vissa delar av landningssidan så att de är redigerbara i AEM-komponenter. På så sätt kan du snabbt redigera delar av landningssidan även efter det att landningssidans design har importerats.

Det gör du genom att lägga till `data-cq-component` i lämplig komponent i den HTML-fil som du importerar.

I följande avsnitt beskrivs hur du redigerar din HTML-fil så att du konverterar vissa delar av dina landningssidor till olika redigerbara AEM-komponenter. Komponenter beskrivs i detalj i [Landing Pages Components](/help/sites-classic-ui-authoring/classic-personalization-campaigns-landingpage.md).

>[!NOTE]
>
>HTML-kod för att konvertera delar av landningssidan till AEM-komponenter har både lång form och en kortskriftsdeklaration. Båda beskrivs för varje komponent.

### Begränsningar {#limitations}

Observera följande begränsningar innan du importerar:

### Attribut som klass eller id som används i &amp;lt;body>-taggen bevaras inte {#any-attribute-like-class-or-id-applied-on-the-amp-lt-body-tag-is-not-preserved}

Om ett attribut som id eller class tillämpas på body-taggen, till exempel `<body id="container">`, bevaras det inte efter importen. Den design som importeras bör därför inte ha några beroenden av attributen som används för taggen `<body>`.

### Dra och släpp zip {#drag-and-drop-zip}

Zip-överföring med dra och släpp stöds inte för Internet Explorer och Firefox version 3.6 och tidigare. Om du vill överföra en design när du använder dessa webbläsare klickar du på släppzonen för att öppna en dialogruta för filöverföring och överför designen med den dialogrutan.

De webbläsare som har stöd för&quot;dra och släpp&quot; i zip-filen är Chrome, Safari5.x, Firefox 4 och senare.

### Modernizr stöds inte {#modernizr-is-not-supported}

`Modernizr.js` är ett JavaScript-baserat verktyg som identifierar webbläsares inbyggda funktioner och identifierar om de passar för HTML5-element eller inte. Designer som använder Modernizer för att förbättra stödet i äldre versioner av olika webbläsare kan orsaka importproblem i landningssidans lösning. `Modernizr.js` skript stöds inte av designimporteraren.

### Sidegenskaperna bevaras inte vid import av designpaket {#page-properties-are-not-preserved-at-the-time-of-importing-design-package}

Alla sidegenskaper (till exempel Anpassad domän, Tillämpa HTTPS och så vidare) som har angetts för en sida (som använder mallen Tom landningssida) innan designpaketet importeras, försvinner när designen har importerats. Därför rekommenderar vi att du anger sidegenskaperna när du har importerat designpaketet.

### Markering som bara används i HTML {#html-only-markup-assumed}

Vid import saneras koden av säkerhetsskäl och för att undvika import och publicering av ogiltig kod. Detta förutsätter att koden bara är för HTML och att alla andra typer av element, som t.ex. inline SVG eller Web Components, filtreras bort.

### Text {#text}

HTML-kod som infogar en textkomponent ( `foundation/components/text`) i HTML i designpaketet:

```xml
<div data-cq-component="text"> <p>This is some editable text</p> </div>
```

Om du tar med ovanstående kod i HTML gör du följande:

* Skapar en redigerbar AEM-textkomponent ( `sling:resourceType=foundation/components/text`) på landningssidan som skapas när designpaketet har importerats.
* Ställer in egenskapen `text` för den skapade textkomponenten på den HTML som omges av `div`.

**Deklaration för kort komponenttagg**:

```xml
<p data-cq-component="text">Text component shorthand</p>
```

**Text med en lista**

Så här lägger du till en text med en lista:

* 1:a
* 2:a

som kan redigeras i RTE-redigeraren:

```xml
<div data-cq-component="text"><p>This is text with a list:</p><ul><li>1st</li><li>2nd</li></ul><p>It can be edited with the RTE editor</p></div>
```

**Text med färg**

Så här lägger du till en text med färg (rosa) som kan redigeras i textredigeraren:

```xml
<div class="pink" data-cq-component="text"><p>This is pink text.</p><p>It can be edited with the RTE editor</p></div>
```

### Titel {#title}

HTML-kod som infogar en titelkomponent ( `wcm/landingpage/components/title`) i HTML i designpaketet:

```xml
<div data-cq-component="title"> <h1>This is some editable title text</h1> </div>
```

Om du tar med ovanstående kod i HTML gör du följande:

* Skapar en redigerbar AEM-titelkomponent ( `sling:resourceType=wcm/landingpage/components/title`) på landningssidan som skapas när designpaketet har importerats.
* Ställer in egenskapen `jcr:title` för den skapade titelkomponenten på texten inom rubriktaggen som är kapslad i div.
* Anger egenskapen `type` till rubriktaggen, i det här fallet `h1`.

Titelkomponenten stöder sju typer - `h1, h2, h3, h4, h5, h6` och `default`.

**Deklaration för kort komponenttagg**:

```xml
<h1 data-cq-component="title">Title component shorthand</h1>
```

### Bild {#image}

HTML-kod som infogar en bildkomponent (grund/komponenter/bild) i HTML i designpaketet:

```xml
<div data-cq-component="image">
<img src="img/video1.png" alt="Video about Polar Brake Goggles in action" title="Polar Brake Goggles" width="300" height="200" />
</div>
```

Om du tar med ovanstående kod i HTML gör du följande:

* Skapar en redigerbar AEM-bildkomponent ( `sling:resourceType=foundation/components/image`) på landningssidan som skapas när designpaketet har importerats.
* Ställer in egenskapen `fileReference` för den skapade bildkomponenten på den sökväg till vilken bilden som anges i src-attributet importeras.
* Anger egenskapen `alt` till värdet för alt-attributet i img-taggen.
* Anger egenskapen `title` till värdet för title-attributet i img-taggen.
* Anger egenskapen `width` till värdet för width-attributet i img-taggen.
* Anger egenskapen `height` till värdet för height-attributet i img-taggen.

**Deklaration för kort komponenttagg:**

```xml
<img data-cq-component="image" src="test.png" alt="Image component shorthand"/>
```

#### Absolut URL img src stöds inte i Image component Div {#absolute-url-img-src-not-supported-within-image-component-div}

Om en `<img>`-tagg med en absolut url-src används för komponentkonvertering, genereras ett lämpligt **UnsupportedTagContentException** . Följande stöds till exempel inte:

`<div data-cq-component="image">`

`<img src="https://cdn.printfriendly.com/pf-button.gif" alt="Print Friendly and PDF"/>`

`</div>`

I annat fall stöds absoluta URL-bilder för img-taggar som inte ingår i Image Component div.

### Komponenter för uppmaning {#call-to-action-components}

Du kan markera en del av landningssidan för import som en&quot;redigerbar Call to action-komponent&quot; - sådana importerade call-to-action-komponenter kan redigeras efter att landningssidan har importerats. AEM innehåller följande CTA-komponenter:

* Klicka på Via länk - Gör att du kan lägga till en textlänk som när du klickar på den leder besökaren till en mål-URL.
* Grafisk länk - Gör att du kan lägga till en bild som besökaren kommer till en mål-URL när du klickar på den.

#### Klicka genom länken {#click-through-link}

Den här CTA-komponenten kan användas för att lägga till en textlänk på landningssidan.

Egenskaper som stöds

* Etikett, med fet stil, kursiv stil och understrykning
* Mål-URL, stöder tredje part och AEM-URL
* Alternativ för sidåtergivning (samma fönster, nytt fönster, osv.)

HTML-tagg om du vill ta med klickningen genom komponenten i den importerade zippen. Här mappas href till mål-URL, &quot;Visa produktinformation&quot; mappas till etikett och så vidare.

```xml
<div id="cqcanvas">
.
.
                <div data-cq-component="clickThroughLink">
        <a href="/content/we-retail/us/en/products/equipment/snow-sports/flying-snowboard.html">View Product Details  ></a>
  </div>
.
.
</div>
```

Den här komponenten kan användas i alla fristående program eller importeras från zip.

**Deklaration för kort komponenttagg**:

```xml
<a href="/somelink.html" data-cq-component="clickThroughLink">Click Through Link shorthand</a>
```

#### Grafisk länk {#graphical-link}

Den här CTA-komponenten kan användas för att lägga till grafik med en länk på landningssidan. Bilden kan vara en enkel knapp eller en grafisk bild som bakgrund. När användaren klickar på bilden dirigeras användaren till den mål-URL som anges i komponentegenskaperna. Det ingår i gruppen&quot;Call to Action&quot;.

Egenskaper som stöds

* Bildbeskärning, rotering
* Hovringstext, beskrivning, storlek i px
* Mål-URL, stöder tredje part och AEM-URL
* Alternativ för sidåtergivning (samma fönster, nytt fönster, osv.)

HTML-tagg om du vill ta med en grafisk länkkomponent i den importerade zippen. Här mappas href till target url, img src är återgivningsbilden, &quot;title&quot; tolkas som hover text osv.

```xml
<div id="cqcanvas">
  <div data-cq-component="clickThroughGraphicalLink"><a href="https://www.adobe.com/go/wem"><img src="img/call-to-action-button.png" title="Click Here to Learn More" /></a></div>
</div>
```

**Deklaration för kort komponenttagg**:

```xml
<a href="/somelink.html" data-cq-component="clickThroughGraphicalLink"><img src="linkimage.png" alt="Click Through Graphical Link shorthand"/></a>
```

>[!NOTE]
>
>Om du vill skapa en klickbar grafisk länk måste du kapsla in en ankartagg och bildtaggen i en div med attributet `data-cq-component="clickthroughgraphicallink"`.
>
>Exempel: `<div data-cq-component="clickthroughlink"> <a href="https://myURLhere/"><img src="image source here"></a> </div>`
>
>Andra sätt att associera en bild med en ankartagg med CSS stöds inte. Följande kod fungerar till exempel inte:
>
>`<div data-cq-component="clickthroughgraphicallink">`
>
>`<a class="hasBackground" href="https://myURLhere/"></a>`
>
>`</div>`
>
>med en associerad `css .hasbackground { background-image: pathtoimage }`
>

### Leadformulär {#lead-form}

Ett lead-formulär är ett formulär som används för att samla in profilinformation för en besökare/lead. Denna information kan lagras och användas senare för att skapa en effektiv marknadsföring utifrån informationen. Informationen innehåller vanligtvis titel, namn, e-postadress, födelsedatum, adress, ränta osv. Den ingår i gruppen&quot;CTA Lead form&quot;.

**Funktioner som stöds**

* Fördefinierade lead-fält - förnamn, efternamn, adress, dob, kön, about, userId, emailId, submit-knapp är tillgängliga i sidosparken. Dra-och-släpp den komponent du behöver i ditt lead-formulär.
* Med hjälp av dessa komponenter kan författaren utforma ett fristående lead-formulär, motsvarar dessa fält formulärfält lead. I det fristående eller importerade ZIP-programmet kan användaren lägga till extra fält med cq:form eller cta lead-formulärfält, namn och utforma dem enligt kraven.
* Mappa lead-formulärfält med specifika fördefinierade namn för CTA lead-formulär, till exempel: firstName för förnamn i lead-formulär och så vidare.
* Fält som inte är mappade till lead-formulärmappar till cq:formulärkomponenter - text, radio, kryssruta, listruta, dold, lösenord.
* Användaren kan ange rubriken med taggen &quot;label&quot; och formateringen kan fås genom att använda formatattributet &quot;class&quot; (endast tillgängligt för komponenter i CTA lead-formulär).
* Tack! Sidan och prenumerationslistan kan anges som en dold parameter i formuläret (finns i index.htm) eller kan läggas till/redigeras från redigeringsfältet i &quot;Början av lead-formuläret&quot;

  &lt;input type=&quot;hidden&quot; name=&quot;redirectUrl&quot; value=&quot;/content/we-retail/en/user/register/thanks_you&quot;/>

  &lt;input type=&quot;hidden&quot; name=&quot;groupName&quot; value=&quot;leadForm&quot;/>

* Begränsningar som - krävs kan anges i redigeringskonfigurationen för varje komponent.

HTML-tagg om du vill ta med en grafisk länkkomponent i den importerade zippen. Här mappas&quot;firstName&quot; till lead-formulär firstName osv., förutom för kryssrutor - dessa två kryssrutor mappar till cq:form dropdown component.

```xml
<div id="cqcanvas">
   <div id="form_wrapper">
    <h2>NEWSLETTER SIGN UP</h2>
       <div data-cq-component="leadFormGeneration">
       <form method="post" action="#" onsubmit="return popupBox()">
       <label for="firstName" class="checkText">
        FIRST NAME
       </label><br />
       <input name="firstName" class="text pink" type="text" /><br />
       <label for="lastName" class="checkText">
        LAST NAME
       </label><br />
       <input name="lastName" class="text pink" type="text" /><br />
       <label for="emailId" class="checkText">
        EMAIL ADDRESS
       </label><br />
       <input name="emailId" class="text pink" type="text" /><br />

       <div class="checkboxes">
       <input type="checkbox" class="check" name="send_news" /> <label for="send_news" class="checkText">Send me the latest We.Retail news and announcements.</label><br />
       <input type="checkbox" class="check" name="send_offers" /> <label for="send_offers" class="checkText">Send me We.Retail deals and special offers.</label><br />
       </div>
       <input type="submit" name="submit" class="submit pink" value="Sign Up >" />
       </form>
     </div>
   </div>
```

### Parsys {#parsys}

Komponenten AEM Parsys är en behållarkomponent som kan innehålla andra AEM-komponenter. Det går att lägga till en Parsys-komponent i den importerade HTML-filen. Detta gör att användaren kan lägga till/ta bort redigerbara AEM-komponenter på landningssidan även efter att den har importerats.

Styckesystemet ger användarna möjlighet att lägga till komponenter med hjälp av sidbrytaren.

HTML-kod som infogar en Parsys-komponent ( `foundation/components/parsys`) i HTML i designpaketet:

```xml
<div data-cq-component="parsys">
   <div data-cq-component="title"><h2>ULTIMATE PROTECTION</h2></div>
        <div data-cq-component="title"><h3>ON SALE</h3></div>
</div>
```

Om du tar med ovanstående kod i HTML gör du följande:

* Infogar en AEM Parsys-komponent (grund/komponenter/parsys) på landningssidan som skapas när designpaketet har importerats.
* Initierar sidsparken med standardkomponenter. Du kan lägga till nya komponenter på landningssidan genom att dra komponenter från sidosparken till Parsys-komponenten.
* Två titelkomponenter ingår också i Parsys.

### Mål {#target}

Målkomponenten visar innehållet i en upplevelse på sidan. Man kan ha många upplevelser som skapats i en kampanj och målkomponenten kan dynamiskt visa innehåll från olika upplevelser för olika användare som besöker sidan.

html-koden som infogar en målkomponent och skapar också olika upplevelser i en kampanj:

```xml
<div data-cq-component="target">
 <section data-cq-component="experience" data-cq-experience="default">
  <p data-cq-component="text">Default content. Select this campaign in client context to view other experiences</p>
 </section>

 <section data-cq-component="experience" data-cq-segment="over-30">
  <p data-cq-component="text">Content for Over 30</p>
 </section>

 <section data-cq-component="experience" data-cq-segment="under-30">
  <p data-cq-component="text">Content for Under 30</p>
 </section>
</div>
```

## Fler importalternativ {#additional-importing-options}

Förutom att ange om de importerade komponenterna är redigerbara AEM-komponenter, kan du även konfigurera följande innan du importerar designpaketet:

* Ange sidegenskaper genom att extrahera metadata som definierats i den importerade HTML-filen.
* Ange teckenuppsättningens kodning i HTML.
* Ersätta importsidmallen.

### Ange sidegenskaper genom att extrahera metadata som definierats i importerad HTML {#setting-page-properties-by-extracting-metadata-defined-in-imported-html}

Följande metadata som deklarerats i huvudet på den importerade HTML ska extraheras och bevaras av designimportören som egenskapen &quot;jcr:description&quot;:

* &lt;meta name=&quot;description&quot; content=&quot;&quot;>

Lang-attributet som anges i HTML-taggen ska extraheras och bevaras av designimportören som egenskapen &quot;jcr:language&quot;

* &lt;html lang=&quot;en&quot;>

### Ange teckenuppsättningens kodning i html {#specifying-the-charset-encoding-in-the-html}

Designimporteraren läser kodningen som anges i den importerade HTML-filen. Kodningen kan anges enligt följande:

`<meta charset="UTF-8">`

*ELLER*

`<meta http-equiv="content-type" content="text/html;charset=utf-8">`

Om ingen kodning anges i den importerade HTML-filen är standardkodningen som anges av designimportverktyget UTF-8.

### Överläggningsmall {#overlaying-template}

Mallen för tom landningssida kan vara överlagrad genom att skapa en på: `/apps/<appName>/designimporter/templates/<templateName>`

Steg för att skapa en mall i AEM beskrivs under [Mallar](/help/sites-developing/templates.md).

### Referera en komponent från landningssidan {#referring-a-component-from-landing-page}

Anta att du har en komponent som du vill referera till i din HTML med data-cq-component-attribut, så att designimporteraren återger en komponent som finns på den här platsen. Du vill till exempel referera till tabellkomponenten ( `resourceType = /libs/foundation/components/table`). Följande måste läggas till i HTML:

`<div data-cq-component="/libs/foundation/components/table">foundation table</div>`

Sökvägen i data-cq-komponenten ska vara resourceType för komponenten.

### Bästa praxis {#best-practices}

Du bör inte använda CSS-väljare som liknar följande för element som är markerade för komponentkonvertering vid import.

| E > F | ett F-element underordnat ett E-element | [Underordnad kombinator](https://www.w3.org/TR/css3-selectors/#child-combinators) |
|---|---|---|
| E + F | ett F-element som omedelbart föregås av ett E-element | [Intilliggande jämställd kombinator](https://www.w3.org/TR/css3-selectors/#adjacent-sibling-combinators) |
| E ~ F | ett F-element föregås av ett E-element | [Allmän jämställda kombinator](https://www.w3.org/TR/css3-selectors/#general-sibling-combinators) |
| E:root | ett E-element, dokumentets rot | [Strukturella pseudoklasser](https://www.w3.org/TR/css3-selectors/#structural-pseudos) |
| E:nth-child(n) | ett E-element, det n:te underordnade elementet till dess överordnade | [Strukturella pseudoklasser](https://www.w3.org/TR/css3-selectors/#structural-pseudos) |
| E:nth-last-child(n) | ett E-element, det n:te underordnade elementet till det överordnade elementet, räknat från det sista | [Strukturella pseudoklasser](https://www.w3.org/TR/css3-selectors/#structural-pseudos) |
| E:n:n av typen (n) | ett E-element, det n:te jämställda i sin typ | [Strukturella pseudoklasser](https://www.w3.org/TR/css3-selectors/#structural-pseudos) |
| E:nth-last-of-type(n) | ett E-element, det n:te jämställda i sin typ, räknat från det sista | [Strukturella pseudoklasser](https://www.w3.org/TR/css3-selectors/#structural-pseudos) |

Detta beror på att ytterligare HTML-element som &lt;div>-taggen läggs till i den genererade HTML-koden efter importen.

* Skript som använder en struktur som liknar den ovan rekommenderas inte heller för element som är markerade för konvertering till AEM-komponenter.
* Du bör inte använda format i märkordstaggar för komponentkonvertering som &lt;div data-cq-component=&quot;&amp;ast;&quot;>.
* Designlayouten bör följa vedertagna standarder från HTML5 Boilerplate. Läs mer på: [https://html5boilerplate.com/](https://html5boilerplate.com/).

## Konfigurerar OSGI-moduler {#configuring-osgi-modules}

Komponenterna som visar egenskaper som kan konfigureras via OSGI-konsolen är följande:

* Import av landningssiddesign
* Landing Page Builder
* Mobile Landing Page Builder
* Inmatningsförprocessor för landningssida

Tabellen nedan beskriver kortfattat egenskaperna:

<table>
 <tbody>
  <tr>
   <td><strong>Komponent</strong></td>
   <td><strong>Egenskapsnamn</strong></td>
   <td><strong>Egenskapsbeskrivning </strong></td>
  </tr>
  <tr>
   <td>Import av landningssiddesign</td>
   <td>Filtret Extrahera</td>
   <td>Listan med reguljära uttryck som ska användas för att filtrera filer från extraheringen. <br /> ZIP-poster som matchar något av de angivna mönstren exkluderas från extraheringen</td>
  </tr>
  <tr>
   <td>Landing Page Builder</td>
   <td>Filmönster</td>
   <td>Landing Page Builder kan konfigureras för att hantera HTML-filer som matchar ett reguljärt uttryck enligt filmönstret.</td>
  </tr>
  <tr>
   <td>Mobile Landing Page Builder</td>
   <td>Filmönster</td>
   <td>Landing Page Builder kan konfigureras för att hantera HTML-filer som matchar ett reguljärt uttryck enligt filmönstret.</td>
  </tr>
  <tr>
   <td> </td>
   <td>Enhetsgrupper</td>
   <td>Listan över enhetsgrupper som stöds.</td>
  </tr>
  <tr>
   <td>Inmatningsförprocessor för landningssida</td>
   <td>Sökmönster </td>
   <td>Det mönster som ska sökas efter i arkivpostens innehåll. Det reguljära uttrycket matchas med posten content line for line. Vid matchning ersätts den matchande texten med det angivna ersättningsmönstret.<br /> <br /> Se anmärkning nedan angående aktuella begränsningar för preprocessor för inmatning av startsida.</td>
  </tr>
  <tr>
   <td> </td>
   <td>Ersätt mönster</td>
   <td>Mönstret som ersätter de träffar som hittas. Du kan använda regex-gruppreferenser som $1, $2. Det här mönstret stöder även nyckelord som {designPath} som löses med det faktiska värdet under importen.</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>**Aktuell begränsning för preprocessor för landningssidinmatning:**
>Om du behöver göra några ändringar i sökmönstret måste du lägga till omvända snedstreck manuellt när du öppnar egenskapsredigeraren för felix för att undvika regex-metatecken. Om du inte lägger till omvända snedstreck manuellt anses regex vara ogiltigt och ersätter inte det äldre.
>
>Om standardkonfigurationen till exempel är
>
>>`/\* *CQ_DESIGN_PATH *\*/ *(['"])`
>
>Och du måste ersätta `CQ_DESIGN_PATH` med `VIPURL` i sökmönstret, så ska sökmönstret se ut så här:
>
>`/\* *VIPURL *\*/ *(['"])`

## Felsökning {#troubleshooting}

När du importerar designpaketet kan det uppstå flera fel, som beskrivs i det här avsnittet.

### Initiering av sidosparken med komponenter som är relevanta för landningssidan {#initialization-of-sidekick-with-landing-page-relevant-components}

Om designpaketet innehåller en Parsys-komponentkod börjar sidosparken visa relevanta komponenter för landningssidan efter importen. Du kan dra och släppa nya komponenter till Parsys-komponenten på landningssidan. Du kan också gå till designläget och lägga till nya komponenter i sidosparken.

### Felmeddelanden som visas vid import {#error-messages-displayed-during-import}

Om fel uppstår (till exempel om det importerade paketet inte är en giltig zip-fil) importeras inte paketet vid designimporten. I stället visas ett felmeddelande ovanpå sidan precis ovanför dra och släpp-rutan. Här finns exempel på felscenarier. När du har åtgärdat felet kan du importera den uppdaterade zippen på samma tomma landningssida igen. Olika scenarier där fel uppstår är följande:

* Det importerade designpaketet är inte ett giltigt zip-arkiv.
* Det importerade designpaketet innehåller inte index.html på den översta nivån.

### Varningar som visas efter import {#warnings-displayed-after-import}

Om det finns några varningar (t.ex. om HTML refererar till bilder som inte finns i paketet) importerar designimporteraren zip-filen, men visar samtidigt en lista med problem/varningar i resultatrutan. Om du klickar på problemlänken visas en lista med varningar som pekar på eventuella problem i designpaketet. Olika scenarier där varningar fångas upp och visas av designimportören är följande:

* HTML avser bilder som inte finns i paketet.
* HTML avser skript som inte finns i paketet.
* HTML avser format som inte finns i paketet.

### Var lagras filerna i ZIP-filen i AEM? {#where-are-the-files-of-the-zip-file-being-stored-in-aem}

När landningssidan har importerats lagras filerna (bilder, css, js och så vidare) i designpaketet på följande plats i AEM:

`/etc/designs/default/canvas/content/campaigns/<name of brand>/<name of campaign>/<name of landing page>`

Anta att landningssidan skapas under kampanjen `We.Retail` och att namnet på landningssidan är **myBlankLandingPage**. Den plats där ZIP-filer lagras är följande:

`/etc/designs/default/canvas/content/campaigns/geometrixx/myBlankLandingPage`

### Formateringen bevaras inte {#formatting-not-preserved}

Tänk på följande begränsningar när du skapar CSS:

Om en text och (redigerbar) bild ser ut som följande:

```xml
<div class="box">
<p><div data-cq-component="image"><img src="assets/image.jpg" width="115"
height="116" /></div>Some Text </p>
</div>
```

med en CSS tillämpad på klassen `box` enligt följande:

```xml
.box

{ width: 450px; padding:10px; border: 1px #C5DBE7 solid; margin: 0px auto 0 auto; background-image:url(assets/box.gif); background-repeat:repeat-x,y; font-family:Verdana, Arial, Helvetica, sans-serif; font-size:12px; color:#6D6D6D; }
```

Sedan används `box img` i designimporteraren och den resulterande landningssidan verkar inte ha bevarat formateringen. För att undvika detta lägger AEM till div-taggar i CSS och skriver om koden i enlighet med detta. Annars kommer vissa CSS-regler att vara ogiltiga.

```xml
.box img

{ float:right; margin: 0 0 5px 5px; border: 1px #343434 solid; }
```

>[!NOTE]
>
>Designers bör bara koda inuti taggen **id=cqcanvas** som identifieras av importeraren, annars bevaras inte designen.
