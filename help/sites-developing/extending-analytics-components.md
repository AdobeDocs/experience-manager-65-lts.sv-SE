---
title: Lägga till Adobe Analytics Tracking i komponenter
description: Lär dig hur du lägger till Adobe Analytics Tracking i komponenter i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Developer
exl-id: 94bdf379-d10f-4dd3-b250-f2d1a3e4c251
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1244'
ht-degree: 0%

---

# Lägga till Adobe Analytics Tracking i komponenter{#adding-adobe-analytics-tracking-to-components}

## Inkludera Adobe Analytics-modulen i en sidkomponent {#including-the-adobe-analytics-module-in-a-page-component}

Sidmallskomponenter (till exempel `head.jsp, body.jsp`) behöver JSP-inkluderingar för att läsa in ContextHub och Adobe Analytics-integreringen (som är en del av molntjänster). Alla innehåller läsa in JavaScript-filer.

ContextHub-posten ska inkluderas direkt under taggen `<head>`, medan molntjänster ska inkluderas i avsnittet `<head>` och före `</body>` , till exempel:

```xml
<head>
   <sling:include path="contexthub" resourceType="granite/contexthub/components/contexthub" />
...
   <cq:include script="/libs/cq/cloudserviceconfigs/components/servicelibs/servicelibs.jsp"/>
...
</head>
<body>
...
    <cq:include path="cloudservices" resourceType="cq/cloudserviceconfigs/components/servicecomponents"/>
</body>
```

Skriptet `contexthub` som du infogar efter elementet `<head>` lägger till ContextHub-funktionerna på sidan.

De `cloudservices`-skript som du lägger till i avsnitten `<head>` och `<body>` gäller för de molntjänstkonfigurationer som läggs till på sidan. (Om sidan använder mer än en konfiguration för molntjänster måste du bara inkludera ContextHub-jsp och Cloud Services-jsp en gång.)

När ett Adobe Analytics-ramverk läggs till på sidan genererar `cloudservices`-skripten Adobe Analytics-relaterade JavaScript och referenser till klientbibliotek, som i följande exempel:

```xml
<div class="sitecatalyst cloudservice">
<script type="text/javascript" src="/etc/clientlibs/foundation/sitecatalyst/sitecatalyst.js"></script>
<script type="text/javascript" src="/etc/clientlibs/foundation/sitecatalyst/util.js"></script>
<script type="text/javascript" src="/content/geometrixx-outdoors/_jcr_content/analytics.sitecatalyst.js"></script>
<script type="text/javascript" src="/etc/clientlibs/mac/mac-sc.js"></script>
<script type="text/javascript" src="/etc/clientlibs/foundation/sitecatalyst/plugins.js"></script>
<script type="text/javascript">
<!--
CQ_Analytics.Sitecatalyst.frameworkComponents = ['foundation/components/page'];
/**
 * Sets Adobe Analytics variables accordingly to mapped components. If <code>options</code>
 * object is provided only variables matching the options.componentPath are set.
 *
 * @param {Object} options Parameter object from CQ_Analytics.record() call. Optional.
 */
CQ_Analytics.Sitecatalyst.updateEvars = function(options) {
    this.frameworkMappings = [];
 this.frameworkMappings.push({scVar:"pageName",cqVar:"pagedata.title",resourceType:"foundation/components/page"});
    for (var i=0; i<this.frameworkMappings.length; i++){
  var m = this.frameworkMappings[i];
  if (!options || options.compatibility || (options.componentPath == m.resourceType)) {
   CQ_Analytics.Sitecatalyst.setEvar(m);
  }
    }
}

CQ_Analytics.CCM.addListener("storesinitialize", function(e) {
 var collect = true;
    var lte = s.linkTrackEvents;
    s.pageName="content:geometrixx-outdoors:en";
    CQ_Analytics.Sitecatalyst.collect(collect);
    if (collect) {
  CQ_Analytics.Sitecatalyst.updateEvars();
     /************* DO NOT ALTER ANYTHING BELOW THIS LINE ! **************/
     var s_code=s.t();if(s_code)document.write(s_code);
     s.linkTrackEvents = lte;
     if(s.linkTrackVars.indexOf('events')==-1){delete s.events};
     $CQ(document).trigger("sitecatalystAfterCollect");
    }
});
//-->
</script>
<script type="text/javascript">
<!--
if(navigator.appVersion.indexOf('MSIE')>=0)document.write(unescape('%3C')+'\!-'+'-')
//-->
</script>
<noscript><img src="https://daydocumentation.112.2o7.net/b/ss/daydocumentation/1/H.25--NS/1380120772954?cdp=3&gn=content%3Ageometrixx-outdoors%3Aen" height="1" width="1" border="0" alt=""/></noscript>
<span data-tracking="{event:'pageView', values:{}, componentPath:'foundation/components/page'}"></span>
<div id="cq-analytics-texthint" style="background:white; padding:0 10px; display:none;">
 <h3 class="cq-texthint-placeholder">Component clientcontext is missing or misplaced.</h3>
</div>
<script type="text/javascript">
$CQ(function(){
 if( CQ_Analytics &&
  CQ_Analytics.ClientContextMgr &&
  !CQ_Analytics.ClientContextMgr.isConfigLoaded )
  {
   $CQ("#cq-analytics-texthint").show();
  }
});
</script>
</div>
```

Koden inkluderas på alla exempelwebbplatser i AEM, t.ex. Geometrixx Outdoors.

### Händelsen sitecatalystAfterCollect {#the-sitecatalystaftercollect-event}

Skriptet `cloudservices` utlöser händelsen `sitecatalystAfterCollect`:

```
$CQ(document).trigger("sitecatalystAfterCollect");
```

Den här händelsen aktiveras för att ange att sidspårning har slutförts. Om du utför ytterligare spårningsåtgärder på den här sidan bör du lyssna på den här händelsen i stället för dokumentets load- eller dokumentready-händelse. Om du använder händelsen `sitecatalystAfterCollect` undviker du kollisioner eller andra oförutsägbara beteenden.

>[!NOTE]
>
>Biblioteket `/libs/cq/analytics/clientlibs/sitecatalyst/sitecatalyst.js` innehåller koden från Adobe Analytics `s_code.js`-filen.

## Implementera Adobe Analytics Tracking för anpassade komponenter {#implementing-adobe-analytics-tracking-for-custom-components}

Låt AEM-komponenterna interagera med Adobe Analytics ramverk. Konfigurera sedan ramverket så att Adobe Analytics spårar komponentdata.

Komponenter som interagerar med Adobe Analytics-ramverket visas i Sidekick när du redigerar ett ramverk. När du har dragit komponenten till ramverket visas komponentegenskaperna och du kan sedan mappa dem med Adobe Analytics-egenskaper. (Se [Konfigurera ett ramverk för grundläggande spårning](/help/sites-administering/adobeanalytics-connect.md#creating-a-adobe-analytics-framework).)

Komponenter kan interagera med Adobe Analytics-ramverket när komponenten har en underordnad nod med namnet `analytics`. Noden `analytics` har följande egenskaper:

* `cq:trackevents`: Identifierar CQ-händelserna som komponenten visar. (Se Anpassade händelser.)
* `cq:trackvars`: Namnger CQ-variablerna som mappas med Adobe Analytics-egenskaper.
* `cq:componentName`: Namnet på komponenten som visas i Sidekick.
* `cq:componentGroup`: Den grupp i Sidekick som innehåller komponenten.

Koden i komponent-JSP lägger till JavaScript på sidan som utlöser spårningen och definierar data som spåras. Händelsenamnet och datanamnen som används i JavaScript måste matcha motsvarande värden för nodegenskaperna `analytics`.

* Använd dataspårningsattributet för att spåra händelsedata när en sida läses in. (Se [Spåra anpassade händelser vid sidinläsning](/help/sites-developing/extending-analytics.md#tracking-custom-events-on-page-load).)
* Använd funktionen CQ_Analytics.record för att spåra händelsedata när användarna interagerar med sidfunktioner. (Se [Spåra anpassade händelser efter sidinläsning](/help/sites-developing/extending-analytics.md#tracking-custom-events-after-page-load).)

När du använder dessa dataspårningsmetoder utför Adobe Analytics integreringsmodul automatiskt anropen till Adobe Analytics för att registrera händelser och data.

### Exempel: Spåra tabbtangenter {#example-tracking-topnav-clicks}

Förläng grundkomponenten så att Adobe Analytics spårar klickningar på navigeringslänkar högst upp på sidan. När du klickar på en navigeringslänk registreras länken som användaren klickade på och den sida där den klickades.

Följande procedurer kräver att du redan har utfört följande uppgifter:

* Skapade ett CQ-program.
* Skapade en Adobe Analytics-konfiguration och ett Adobe Analytics Framework.

#### Kopiera den övre komponenten {#copy-the-topnav-component}

Kopiera den övre komponenten till CQ-programmet. Proceduren kräver att ditt program är konfigurerat i CRXDE Lite.

1. Högerklicka på noden `/libs/foundation/components/topnav` och klicka på Kopiera.
1. Högerklicka på mappen Komponenter under programmappen och klicka på Klistra in.
1. Klicka på Spara alla.

#### Integrera topnav med Adobe Analytics Framework {#integrating-topnav-with-the-adobe-analytics-framework}

Konfigurera den övre komponenten och redigera JSP-filen för att definiera spårningshändelser och data.

1. Högerklicka på den övre noden och klicka på Skapa > Skapa nod. Ange följande egenskapsvärden och klicka sedan på OK:

   * Namn: `analytics`
   * Typ: `nt:unstructured`

1. Lägg till följande egenskap i analysnoden så att du kan namnge spårningshändelsen:

   * Namn: cq:trackevents
   * Typ: String
   * Värde: topnavClick

1. Lägg till följande egenskap i analysnoden så att du kan namnge datavariablerna:

   * Namn: cq:trackvars
   * Typ: String
   * Värde: topnavTarget,topnavLocation

1. Lägg till följande egenskap i analysnoden för att namnge komponenten för Sidekick:

   * Namn: cq:componentName
   * Typ: String
   * Värde: topnav (spärra/knip)

1. Lägg till följande egenskap i analysnoden för att namnge komponentgruppen för Sidekick:

   * Namn: cq:componentGroup
   * Typ: String
   * Värde: Allmänt

1. Klicka på Spara alla.
1. Öppna filen `topnav.jsp`.
1. Lägg till följande attribut i elementet:

   ```xml
   onclick = "tracknav('<%= child.getPath() %>.html')"
   ```

1. Längst ned på sidan lägger du till följande JavaScript-kod:

   ```xml
   <script type="text/javascript">
       function tracknav(target) {
               if (CQ_Analytics.Sitecatalyst) {
                   CQ_Analytics.record({
                       event: 'topnavClick',
                       values: {
                           topnavTarget: target,
                           topnavLocation:'<%=currentPage.getPath() %>.html'
                       },
                       componentPath: '<%=resource.getResourceType()%>'
                   });
               }
       }
   </script>
   ```

1. Klicka på Spara alla.

Innehållet i filen `topnav.jsp` ska se ut så här:

```xml
<%@page session="false"%><%--
  Copyright 1997-2008 Day Management AG
  Barfuesserplatz 6, 4001 Basel, Switzerland
  All Rights Reserved.

  This software is the confidential and proprietary information of
  Day Management AG ("Confidential Information"). You shall not
  disclose such Confidential Information and shall use it only in
  accordance with the terms of the license agreement you entered into
  with Day.

  ==============================================================================

  Top Navigation component

  Draws the top navigation

--%><%@include file="/libs/foundation/global.jsp"%><%
%><%@ page import="java.util.Iterator,
        com.day.text.Text,
        com.day.cq.wcm.api.PageFilter,
        com.day.cq.wcm.api.Page,
        com.day.cq.commons.Doctype,
        org.apache.commons.lang3.StringEscapeUtils" %><%

    // get starting point of navigation
    long absParent = currentStyle.get("absParent", 2L);
    String navstart = Text.getAbsoluteParent(currentPage.getPath(), (int) absParent);

    //if not deep enough take current node
    if (navstart.equals("")) navstart=currentPage.getPath();

    Resource rootRes = slingRequest.getResourceResolver().getResource(navstart);
    Page rootPage = rootRes == null ? null : rootRes.adaptTo(Page.class);
    String xs = Doctype.isXHTML(request) ? "/" : "";
    if (rootPage != null) {
        Iterator<Page> children = rootPage.listChildren(new PageFilter(request));
        while (children.hasNext()) {
            Page child = children.next();
            %><a onclick = "tracknav('<%= child.getPath() %>.html')"  href="<%= child.getPath() %>.html"><%
            %><img alt="<%= StringEscapeUtils.escapeXml(child.getTitle()) %>" src="<%= child.getPath() %>.navimage.png"<%= xs %>></a><%
        }
    }
%><script type="text/javascript">
    function tracknav(target) {
            if (CQ_Analytics.Sitecatalyst) {
                CQ_Analytics.record({
                    event: 'topnavClick',
                    values: {
                        topnavTarget:target,
                        topnavLocation:'<%=currentPage.getPath() %>.html'
                    },
                    componentPath: '<%=resource.getResourceType()%>'
                });
            }
    }
</script>
```

>[!NOTE]
>
>Det är ofta önskvärt att spåra data från ContextHub. Mer information om hur du använder JavaScript för att hämta den här informationen finns i [Åtkomst till värden i ContextHub](/help/sites-developing/extending-analytics.md#accessing-values-in-the-contexthub).

#### Lägga till spårningskomponenten i Sidekick {#adding-the-tracking-component-to-sidekick}

Lägg till komponenter som är aktiverade för spårning med Adobe Analytics i Sidekick så att du kan lägga till dem i ramverket.

1. Öppna Adobe Analytics-ramverket från din Adobe Analytics-konfiguration. ([http://localhost:4502/etc/cloudservices/sitecatalyst.html](http://localhost:4502/etc/cloudservices/sitecatalyst.html))
1. I Sidekick klickar du på Design.

   ![Knappen Design med en kvadrat med höger vinkel.](assets/chlimage_1a.png)

1. Klicka på Konfigurera arv i området Konfiguration av länkspårning.

   ![chlimage_1](assets/chlimage_1aa.png)

1. I listan Tillåtna komponenter väljer du topnav (spårning) i avsnittet Allmänt och klickar sedan på OK.
1. Expandera Sidekick till redigeringsläget. Komponenten är nu tillgänglig i gruppen Allmänt.

#### Lägga till komponenten topnav i ramverket {#adding-the-topnav-component-to-your-framework}

Dra den övre komponenten till Adobe Analytics-ramverket och mappa komponentvariablerna och händelserna till Adobe Analytics-variabler och -händelser. (Se [Konfigurera ett ramverk för grundläggande spårning](/help/sites-administering/adobeanalytics-connect.md).)

![chlimage_1-1](assets/chlimage_1-1a.png)

Den topnav-komponenten är nu integrerad med Adobe Analytics-ramverket. När du lägger till komponenten på en sida skickas spårningsdata till Adobe Analytics om du klickar på objekten i det övre navigeringsfältet.

### Skicka s.products Data till Adobe Analytics {#sending-s-products-data-to-adobe-analytics}

Komponenter kan generera data för variabeln s.products som skickas till Adobe Analytics. Utforma komponenterna för att bidra till variabeln s.products:

* Registrera ett värde med namnet `product` för en specifik struktur.
* Visa datamedlemmarna för värdet `product` så att de kan mappas med Adobe Analytics-variabler i Adobe Analytics-ramverket.

Variabeln Adobe Analytics s.products har följande syntax:

```
s.products="category;product;quantity;price;eventY={value}|eventZ={value};evarA={value}|evarB={value}"
```

Adobe Analytics-integreringsmodulen konstruerar variabeln `s.products` med de `product`-värden som AEM-komponenter genererar. Värdet `product` i JavaScript som AEM-komponenter genererar är en array med värden som har följande struktur:

```
"product": [{
    "category": "",
    "sku"     : "path to product node",
    "quantity": quantity,
    "price"   : price,
    "events   : {
      "eventName1": "eventValue1",
      "eventName_n": "eventValue_n"
    }
    "evars"   : {
      "eVarName1": "eVarValue1",
      "eVarName_n": "eVarValue_n"
    }
}]
```

När ett dataobjekt utelämnas från värdet `product` skickas det som en tom sträng i s.products.

>[!NOTE]
>
>När ingen händelse är associerad med ett produktvärde använder Adobe Analytics `prodView`-händelsen som standard.

Komponentens `analytics`-nod måste visa variabelnamnen med egenskapen `cq:trackvars`:

* product.category
* product.sku
* product.quantity
* product.price
* product.events.eventName1
* product.events.eventName_n
* product.evars.eVarName1
* product.evars.eVarName_n

eCommerce-modulen innehåller flera komponenter som genererar variabeldata för s.products. Komponenten `submitorder` ([http://localhost:4502/crx/de/index.jsp#/libs/commerce/components/submitorder/submitorder.jsp](http://localhost:4502/crx/de/index.jsp#/libs/commerce/components/submitorder/submitorder.jsp)) genererar till exempel JavaScript som liknar följande exempel:

```
<script type="text/javascript">
    function trackCartPurchase() {
        if (CQ_Analytics.Sitecatalyst) {
            CQ_Analytics.record({
                "event": ["productsCartPurchase"],
                "values": {
                    "product": [
                        {
                            "category": "",
                            "sku"     : "/path/to/prod/1",
                            "quantity": 3,
                            "price"   : 179.7,
                            "evars"   : {
                                "childSku": "/path/to/prod/1/green/xs",
                                "size"    : "XS"
                            }
                        },
                        {
                            "category": "",
                            "sku"     : "/path/to/prod/2",
                            "quantity": 10,
                            "price"   : 150,
                            "evars"   : {
                                "childSku": "/path/to/prod/2",
                                "size"    : ""
                            }
                        },
                        {
                            "category": "",
                            "sku"     : "/path/to/prod/3",
                            "quantity": 2,
                            "price"   : 102,
                            "evars"   : {
                                "childSku": "/path/to/prod/3/m",
                                "size"    : "M"
                            }
                        }
                    ]
                },
                "componentPath": "commerce/components/submitorder"
            });
            CQ_Analytics.record({
                "event": ["discountRedemption"],
                "values": {
                    "discount": "/path/to/discount/1 - /path/to/discount/2",
                    "product" : [{
                        "category": "",
                        "sku"     : "Promotional Discount",
                        "events"  : {"discountRedemption": 20.00}
                    }]
                },
                "componentPath": "commerce/components/submitorder"
            });
            CQ_Analytics.record({
                "event": ["cartPurchase"],
                "values": {
                    "orderId"       : "00e40e2d-13a2-4a00-a8ee-01a9ebb0bf68",
                    "shippingMethod": "overnight",
                    "paymentMethod" : "Amex",
                    "billingState"  : "NY",
                    "billingZip"    : "10458",
                    "product"       : [{"category": "", "sku": "", "quantity": "", "price": ""}]
                },
                "componentPath": "commerce/components/submitorder"
            });
        }
        return true;
    }
</script>
```

#### Begränsa storleken på spårningsanrop {#limiting-the-size-of-tracking-calls}

Webbläsare begränsar i allmänhet storleken på GET-begäranden. Eftersom CQ-produkter och SKU-värden är databassökvägar, kan produktarrayer som innehåller flera värden överskrida storleksgränsen för begäran. Därför bör dina komponenter begränsa antalet objekt i arrayen `product` för varje `CQ_Analytics.record function`. Skapa flera funktioner om antalet objekt som du måste spåra kan överskrida gränsen.

ECommerce `submitorder`-komponenten begränsar till exempel antalet `product`-objekt i ett anrop till fyra. När vagnen innehåller fler än fyra produkter genereras flera `CQ_Analytics.record`-funktioner.
