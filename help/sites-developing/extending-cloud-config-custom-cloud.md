---
title: Skapa en anpassad Cloud Service
description: Standarduppsättningen av molntjänster kan utökas med anpassade Cloud Service-typer
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---

# Skapa en anpassad Cloud Service{#creating-a-custom-cloud-service}

Standarduppsättningen av molntjänster kan utökas med anpassade Cloud Service-typer. På så sätt kan du lägga in egna märkord på sidan på ett strukturerat sätt. Detta är främst till för analytiker från tredje part, till exempel Google Analytics, Chartbeat osv. Molntjänster ärvs från överordnade sidor till underordnade sidor med möjlighet att bryta arvet på alla nivåer.

>[!NOTE]
>
>Den här steg-för-steg-guiden för att skapa en Cloud Service är ett exempel på hur du använder Google Analytics. Allt kanske inte gäller ditt användningsfall.

1. Skapa en nod under `/apps` i CRXDE Lite:

   * **Namn**: `acs`
   * **Typ**: `nt:folder`

1. Skapa en nod under `/apps/acs`:

   * **Namn**: `analytics`
   * **Typ**: `sling:Folder`

1. Skapa två noder under `/apps/acs/analytics`:

   * **Namn**: komponenter
   * **Typ**: `sling:Folder`

   och

   * **Namn**: mallar
   * **Typ**: `sling:Folder`

1. Högerklicka på `/apps/acs/analytics/components`. Välj **Skapa..** följt av **Skapa komponent..** I dialogrutan som öppnas kan du ange:

   * **Etikett**: `googleanalyticspage`
   * **Titel**: `Google Analytics Page`
   * **Supertyp**: `cq/cloudserviceconfigs/components/configpage`
   * **Grupp**: `.hidden`

1. Klicka **Nästa** två gånger och ange:

   * **Tillåtna överordnade:** `acs/analytics/templates/googleanalytics`

   Klicka på **Nästa** två gånger och klicka på **OK**.

1. Lägg till en egenskap i `googleanalyticspage`:

   * **Namn:** `cq:defaultView`
   * **Värde:** `html`

1. Skapa en fil med namnet `content.jsp` under `/apps/acs/analytics/components/googleanalyticspage`, med följande innehåll:

   ```xml
   <%@page contentType="text/html"
               pageEncoding="utf-8"%><%
   %><%@include file="/libs/foundation/global.jsp"%><div>
   
   <div>
       <h3>Google Analytics Settings</h3>
       <ul>
           <li><div class="li-bullet"><strong>accountID: </strong><br><%= xssAPI.encodeForHTML(properties.get("accountID", "")) %></div></li>
       </ul>
   </div>
   ```

1. Skapa en nod under `/apps/acs/analytics/components/googleanalyticspage/`:

   * **Namn**: `dialog`
   * **Typ**: `cq:Dialog`
   * **Egenskaper**:

      * **Namn**: `title`
      * **Typ**: `String`
      * **Värde**: `Google Analytics Config`
      * **Namn**: `xtype`
      * **Typ**: `String`
      * **Värde**: `dialog`

1. Skapa en nod under `/apps/acs/analytics/components/googleanalyticspage/dialog`:

   * **Namn**: `items`
   * **Typ**: `cq:Widget`
   * **Egenskaper**:

      * **Namn**: `xtype`
      * **Typ**: `String`
      * **Värde**: `tabpanel`

1. Skapa en nod under `/apps/acs/analytics/components/googleanalyticspage/dialog/items`:

   * **Namn**: `items`
   * **Typ**: `cq:WidgetCollection`

1. Skapa en nod under `/apps/acs/analytics/components/googleanalyticspage/dialog/items/items`:

   * **Namn**: tab1
   * **Typ**: `cq:Panel`
   * **Egenskaper**:

      * **Namn**: `title`
      * **Typ**: `String`
      * **Värde**: `Config`

1. Skapa en nod under `/apps/acs/analytics/components/googleanalyticspage/dialog/items/items/tab1`:

   * **Namn**: objekt
   * **Typ**: `nt:unstructured`
   * **Egenskaper**:

      * **Namn**: `fieldLabel`
      * **Typ**: Sträng
      * **Värde**: Konto-ID

      * **Namn**: `fieldDescription`
      * **Typ**: `String`
      * **Värde**: `The account ID assigned by Google. Usually in the form UA-NNNNNN-N`

      * **Namn**: `name`
      * **Typ**: `String`
      * **Värde**: `./accountID`
      * **Namn**: `validateOnBlur`
      * **Typ**: `String`
      * **Värde**: `true`
      * **Namn**: `xtype`
      * **Typ**: `String`
      * **Värde**: `textfield`

1. Kopiera `/libs/cq/cloudserviceconfigs/components/configpage/body.jsp` till `/apps/acs/analytics/components/googleanalyticspage/body.jsp` och ändra `libs` till `apps` på rad 34 och gör skriptreferensen på rad 79 till en fullständig kvalificerad sökväg.
1. Skapa en mall under `/apps/acs/analytics/templates/`:

   * med **Resurstyp** = `acs/analytics/components/googleanalyticspage`
   * med **Etikett** = `googleanalytics`
   * med **Title**= `Google Analytics Configuration`
   * med **allowedPath** = `/etc/cloudservices/googleanalytics(/.*)?`
   * med **allowedChildren** = `/apps/acs/analytics/templates/googleanalytics`
   * med **sling:resourceSuperType** = `cq/cloudserviceconfigs/templates/configpage` (på mallnod, inte jcr:content-noden)
   * med **cq:designPath** = `/etc/designs/cloudservices/googleanalytics` (på jcr:content)

1. Skapa en komponent: `/apps/acs/analytics/components/googleanalytics`.

   Lägg till följande innehåll i `googleanalytics.jsp`:

   ```xml
   <%@page import="org.apache.sling.api.resource.Resource,
                   org.apache.sling.api.resource.ValueMap,
                   org.apache.sling.api.resource.ResourceUtil,
                   com.day.cq.wcm.webservicesupport.Configuration,
                   com.day.cq.wcm.webservicesupport.ConfigurationManager" %>
   <%@include file="/libs/foundation/global.jsp" %><%
   
   String[] services = pageProperties.getInherited("cq:cloudserviceconfigs", new String[]{});
   ConfigurationManager cfgMgr = resource.getResourceResolver().adaptTo(ConfigurationManager.class);
   if(cfgMgr != null) {
       String accountID = null;
       Configuration cfg = cfgMgr.getConfiguration("googleanalytics", services);
       if(cfg != null) {
           accountID = cfg.get("accountID", null);
       }
   
       if(accountID != null) {
       %>
   <script type="text/javascript">
   
     var _gaq = _gaq || [];
     _gaq.push(['_setAccount', '<%= accountID %>']);
     _gaq.push(['_trackPageview']);
   
     (function() {
       var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
       ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'https://www') + '.google-analytics.com/ga.js';
       var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
     })();
   
   </script><%
       }
   }
   %>
   ```

   Den anpassade koden ska skapas baserat på konfigurationsegenskaperna.

1. Navigera till `http://localhost:4502/miscadmin#/etc/cloudservices` och skapa en sida:

   * **Titel**: `Google Analytics`
   * **Namn**: `googleanalytics`

   Gå tillbaka i CRXDE Lite och under `/etc/cloudservices/googleanalytics`, lägg till följande egenskap i `jcr:content`:

   * **Namn**: `componentReference`
   * **Typ**: `String`
   * **Värde**: `acs/analytics/components/googleanalytics`

1. Navigera till den nyligen skapade tjänstsidan ( `http://localhost:4502/etc/cloudservices/googleanalytics.html`) och klicka på **+** för att skapa en konfiguration:

   * **Överordnad konfiguration**: `/etc/cloudservices/googleanalytics`
   * **Titel:** `My First GA Config`

   Välj **Google Analytics-konfiguration** och klicka på **Skapa**.

1. Ange ett **konto-ID**, till exempel `AA-11111111-1`. Klicka på **OK**.
1. Navigera till en sida och lägg till den nyligen skapade konfigurationen i sidegenskaperna på fliken **Cloud-tjänster** .
1. Den anpassade koden läggs till på sidan.
