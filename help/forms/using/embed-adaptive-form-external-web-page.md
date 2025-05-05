---
title: Bädda in anpassningsbara formulär på en extern webbsida
description: Lär dig bädda in ett anpassat formulär på en extern webbsida
products: SG_EXPERIENCEMANAGER/6.3/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: ecc90ca2-27a1-4b56-8641-55719240e146
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1043'
ht-degree: 0%

---

# Bädda in anpassningsbara formulär på en extern webbsida{#embed-adaptive-form-in-external-web-page}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/forms/integrate/services/embed-adaptive-form-core-components-external-web-page) |
| AEM 6.5 | Den här artikeln |


<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs ett äldre sätt att skapa adaptiva Forms med baskomponenter. </span>

Du kan [bädda in anpassningsbara formulär på en AEM Sites-sida](/help/forms/using/embed-adaptive-form-aem-sites.md) eller på en webbsida som ligger utanför AEM. Det inbäddade adaptiva formuläret fungerar fullt ut och användarna kan fylla i och skicka formuläret utan att behöva lämna sidan. Det hjälper användaren att stanna kvar i sitt sammanhang för andra element på webbsidan och interagera med formuläret samtidigt.

## Förutsättningar {#prerequisites}

Utför följande steg innan du bäddar in ett anpassat formulär på en extern webbplats

* Publicera det adaptiva formulär som ska bäddas in i Publish-instansen av AEM Forms Server.
* Skapa eller identifiera en webbsida på webbplatsen där du kan lägga upp det adaptiva formuläret. Kontrollera att webbsidan kan [läsa jQuery-filer från ett CDN](https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js) eller ha en lokal kopia av jQuery inbäddad. jQuery krävs för att återge ett anpassat formulär.
* När AEM-servern och webbsidan finns på olika domäner utför du stegen som anges i avsnittet [aktivera AEM Forms för att skicka adaptiva formulär till en korsdomänswebbplats](#cross-site).

## Bädda in anpassat formulär {#embed-adaptive-form}

Du kan bädda in ett anpassat formulär genom att infoga några rader med JavaScript på webbsidan. API:t i koden skickar en HTTP-begäran till AEM-servern för adaptiva formulärresurser och injicerar det adaptiva formuläret i den angivna formulärbehållaren.

Så här bäddar du in det anpassade formuläret:

1. Skapa en webbsida på webbplatsen med följande kod:

   ```html
   <!doctype html>
   <html>
     <head>
       <title>This is the title of the webpage!</title>
       <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
     </head>
     <body>
     <div class="customafsection"/>
       <p>This section is replaced with the adaptive form.</p>
   
    <script>
    var options = {path:"/content/forms/af/locbasic.html", dataRef:"", themepath:"", CSS_Selector:".customafsection"};
    alert(options.path);
    var loadAdaptiveForm = function(options){
    //alert(options.path);
       if(options.path) {
           // options.path refers to the path of the adaptive form
           // For Example: /content/forms/af/ABC, where ABC is the adaptive form
           // Note: If AEM server is running on a context path, the adaptive form URL must contain the context path
           var path = options.path;
           path += "/jcr:content/guideContainer.html";
           $.ajax({
               url  : path ,
               type : "GET",
               data : {
                   // Set the wcmmode to be disabled
                   wcmmode : "disabled"
                   // Set the data reference, if any
                  // "dataRef": options.dataRef
                   // Specify a different theme for the form object
                 //  "themeOverride" : options.themepath
               },
               async: false,
               success: function (data) {
                   // If jquery is loaded, set the inner html of the container
                   // If jquery is not loaded, use APIs provided by document to set the inner HTML but these APIs would not evaluate the script tag in HTML as per the HTML5 spec
                   // For example: document.getElementById().innerHTML
                   if(window.$ && options.CSS_Selector){
                       // HTML API of jquery extracts the tags, updates the DOM, and evaluates the code embedded in the script tag.
                       $(options.CSS_Selector).html(data);
                   }
               },
               error: function (data) {
                   // any error handler
               }
           });
       } else {
           if (typeof(console) !== "undefined") {
               console.log("Path of Adaptive Form not specified to loadAdaptiveForm");
           }
       }
    }(options);
   
    </script>
     </body>
   </html>
   ```

1. I den inbäddade koden:

   * Ändra värdet för variabeln *options.path* med sökvägen för den anpassningsbara formulärets publicerings-URL. Om AEM-servern körs på en kontextsökväg kontrollerar du att URL:en innehåller kontextsökvägen. Ange alltid det fullständiga namnet på det adaptiva formuläret inklusive tillägget. Ovanstående kod och adaptiv från finns till exempel på samma AEM Forms Server så att exemplet använder kontextsökvägen för det adaptiva formuläret `/content/forms/af/locbasic.html`.
   * Ersätt *options.dataRef* med attribut som ska skickas med URL:en. Du kan använda dataref-variabeln för att [förifylla ett anpassat formulär](/help/forms/using/prepopulate-adaptive-form-fields.md).
   * Ersätt *options.themePath* med sökvägen till ett annat tema än det som konfigurerats i det adaptiva formuläret. Du kan också ange temats sökväg med hjälp av attributet request.
   * CSS_Selector är CSS-väljaren för den formulärbehållare där det adaptiva formuläret är inbäddat. Klassen .customafsection css är till exempel CSS-väljaren i exemplet ovan.

Det anpassningsbara formuläret är inbäddat på webbsidan. Observera följande i den inbäddade adaptiva formen:

* Sidhuvudet och sidfoten i det ursprungliga adaptiva formuläret inkluderas inte i det inbäddade formuläret.
* Utkast och inskickade formulär finns på fliken Utkast och inskickningar i Forms Portal.
* Den Skicka-åtgärd som är konfigurerad i det ursprungliga adaptiva formuläret behålls i det inbäddade formuläret.
* Anpassningsbara formulärregler behålls och fungerar fullt ut i det inbäddade formuläret.
* Upplevelsemål- och A/B-tester som konfigurerats i det ursprungliga adaptiva formuläret fungerar inte i det inbäddade formuläret.
* Om Adobe Analytics har konfigurerats på originalformuläret hämtas analysdata av Adobe Analytics-servern. Den finns dock inte i Forms analysrapport.

## Exempel på topologi {#sample-topology}

Den externa webbsidan som bäddar in det adaptiva formuläret skickar förfrågningar till AEM-servern, som vanligtvis ligger bakom brandväggen i ett privat nätverk. För att säkerställa att förfrågningarna dirigeras till AEM-servern på ett säkert sätt bör du konfigurera en omvänd proxyserver.

Vi ska titta på ett exempel på hur du kan konfigurera en omvänd Apache 2.4-proxyserver utan Dispatcher. I det här exemplet är du värd för AEM-servern med kontextsökvägen `/forms` och kartan `/forms` för den omvända proxyn. Den ser till att alla begäranden om `/forms` på Apache-servern dirigeras till AEM-instansen. Den här topologin hjälper till att minska antalet regler i Dispatcher-lagret som alla förfrågningar som har prefixet `/forms` till AEM-servern.

1. Öppna konfigurationsfilen `httpd.conf` och avkommentera följande kodrader. Du kan också lägga till de här kodraderna i filen.

   ```text
   LoadModule proxy_html_module modules/mod_proxy_html.so
   LoadModule proxy_http_module modules/mod_proxy_http.so
   ```

1. Konfigurera proxyregler genom att lägga till följande kodrader i konfigurationsfilen `httpd-proxy.conf`.

   ```text
   ProxyPass /forms https://[AEM_Instance]/forms
   ProxyPassReverse /forms https://[AEM_Instance]/forms
   ```

   Ersätt `[AEM_Instance]` med AEM-serverns publicerings-URL i reglerna.

Om du inte monterar AEM-servern på en kontextbana är proxyreglerna i Apache-lagret som följer:

```text
ProxyPass /content https://<AEM_Instance>/content
ProxyPass /etc https://<AEM_Instance>/etc
ProxyPass /etc.clientlibs https://<AEM_Instance>/etc.clientlibs
# CSRF Filter
ProxyPass /libs/granite/csrf/token.json https://<AEM_Instance>/libs/granite/csrf/token.json

ProxyPassReverse /etc https://<AEM_Instance>/etc
ProxyPassReverse /etc.clientlibs https://<AEM_Instance>/etc.clientlibs
# written for thank you page and other URL present in AF during redirect
ProxyPassReverse /content https://<AEM_Instance>/content
```

>[!NOTE]
>
>Om du konfigurerar någon annan topologi måste du lägga till webbadresserna för Skicka, Förifyll och andra till tillåtelselista i Dispatcher-lagret.

## God praxis {#best-practices}

Tänk på följande när du bäddar in ett anpassat formulär på en webbsida:

* Kontrollera att formateringsreglerna som definieras i webbsidans CSS inte är i konflikt med formulärobjektets CSS. För att undvika konflikterna kan du återanvända webbsidans CSS i det adaptiva formulärtemat med hjälp av AEM klientbibliotek. Mer information om hur du använder klientbiblioteket i adaptiva formulärteman finns i [Teman i AEM Forms](../../forms/using/themes.md).
* Låt formulärbehållaren på webbsidan använda hela fönsterbredden. Det ser till att CSS-reglerna som konfigurerats för mobila enheter fungerar utan ändringar. Om formulärbehållaren inte får hela fönsterbredden måste du skriva anpassad CSS för att formuläret ska kunna anpassas till olika mobila enheter.
* Använd `[getData](https://helpx.adobe.com/se/experience-manager/6-3/forms/javascript-api/GuideBridge.html)` API för att hämta XML- eller JSON-representationen av formulärdata i klienten.
* Använd `[unloadAdaptiveForm](https://helpx.adobe.com/se/experience-manager/6-3/forms/javascript-api/GuideBridge.html)` API för att ta bort det adaptiva formuläret från HTML DOM.
* Ange rubriken för åtkomstkontrollens ursprung när du skickar ett svar från en AEM-server.

## Möjliggör för AEM Forms att skicka adaptiva formulär till en domänövergripande webbplats {#cross-site}

1. På AEM publiceringsinstans går du till AEM Web Console Configuration Manager på `https://'[server]:[port]'/system/console/configMgr`.
1. Leta reda på och öppna konfigurationen för **Refererarfilter för Apache Sling**.
1. I fältet Tillåtna värdar anger du den domän där webbsidan finns. Det gör att värden kan göra POST-begäranden till AEM-servern. Du kan också använda reguljära uttryck för att ange en serie externa programdomäner.
