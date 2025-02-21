---
title: Skapa anpassade tillägg
description: Du kan ringa din egen kod i Adobe Campaign från AEM eller från AEM till Adobe Campaign.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 1%

---

# Skapa anpassade tillägg{#creating-custom-extensions}

När du implementerar ett projekt har du vanligtvis anpassad kod i både AEM och Adobe Campaign. Med det befintliga API:t kan du anropa din anpassade kod i Adobe Campaign från AEM eller från AEM till Adobe Campaign. I det här dokumentet beskrivs hur du gör det.

## Förutsättningar {#prerequisites}

Du måste ha följande installerat:

* Adobe Experience Manager
* Adobe Campaign 6.1

Mer information finns i [Integrera AEM med Adobe Campaign 6.1](/help/sites-administering/campaignonpremise.md).

## Exempel 1: AEM till Adobe Campaign {#example-aem-to-adobe-campaign}

Standardintegrationen mellan AEM och Campaign baseras på JSON och JSSP (JavaScript Server Page). Dessa JSSP-filer finns i Campaign-konsolen och alla börjar med **aec** (Adobe Experience Cloud).

![chlimage_1-15](assets/chlimage_1-15a.png)

>[!NOTE]
>
>[Det här exemplet finns i Geometrixx](/help/sites-developing/we-retail.md), som är tillgängligt från Paketdelning.

I det här exemplet skapades en ny anpassad JSSP-fil som anropar den från AEM-sidan för att hämta resultatet. Den kan till exempel användas för att hämta data från Adobe Campaign eller spara data i Adobe Campaign.

1. Om du vill skapa en JSSP-fil i Adobe Campaign klickar du på ikonen **Nytt** .

   ![Ikonen Ny visas på en sida med en stjärna i det övre vänstra hörnet.](do-not-localize/chlimage_1-4a.png)

1. Ange namnet på JSSP-filen. I det här exemplet används **cus:custom.jssp** (vilket betyder att det finns i namnutrymmet **cus**).

   ![chlimage_1-16](assets/chlimage_1-16a.png)

1. Lägg in följande kod i jssp-filen:

   ```
   <%
   var origin = request.getParameter("origin");
   document.write("Hello from Adobe Campaign, origin : " + origin);
   %>
   ```

1. Spara ditt arbete. Resten av jobbet görs i AEM.
1. Skapa en enkel servlet på AEM-sidan så att du kan anropa denna JSSP. I det här exemplet kan du anta följande:

   * Anslutningen mellan AEM och Campaign fungerar
   * Kampanjmolntjänsten är konfigurerad för **/content/geometrixx-outdoor**

   Det viktigaste objektet i det här exemplet är **GenericCampaignConnector** som gör att du kan anropa (hämta och posta) jssp-filer på Adobe Campaign-sidan.

   Här är ett litet kodfragment:

   ```
   @Reference
   private GenericCampaignConnector campaignConnector;
   ...
   Map<String, String> params = new HashMap<String, String>();
   params.put("origin", "AEM");
   CallResults results = campaignConnector.callGeneric("/jssp/cus/custom.jssp", params, credentials);
   return results.bodyAsString();
   ```

1. I det här exemplet måste du skicka inloggningsuppgifterna till anropet. Du kan hämta dem med metoden getCredentials() där du skickar en sida som har molntjänsten Campaign konfigurerad.

   ```xml
   // page containing the cloudservice for Adobe Campaign
   Configuration config = campaignConnector.getWebserviceConfig(page.getContentResource().getParent());
   CampaignCredentials credentials = campaignConnector.retrieveCredentials(config);
   ```

Den fullständiga koden är följande:

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.ServletException;

import org.apache.felix.scr.annotations.Reference;
import org.apache.felix.scr.annotations.sling.SlingServlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.day.cq.mcm.campaign.CallResults;
import com.day.cq.mcm.campaign.CampaignCredentials;
import com.day.cq.mcm.campaign.GenericCampaignConnector;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.PageManager;
import com.day.cq.wcm.api.PageManagerFactory;
import com.day.cq.wcm.webservicesupport.Configuration;

@SlingServlet(paths="/bin/campaign", methods="GET")
public class CustomServlet extends SlingSafeMethodsServlet {

 private final Logger log = LoggerFactory.getLogger(this.getClass());

 @Reference
 private GenericCampaignConnector campaignConnector;

 @Reference
 private PageManagerFactory pageManagerFactory;

 @Override
 protected void doGet(SlingHttpServletRequest request,
   SlingHttpServletResponse response) throws ServletException,
   IOException {

  PageManager pm = pageManagerFactory.getPageManager(request.getResourceResolver());

  Page page = pm.getPage("/content/geometrixx-outdoors");

  String result = null;
  if ( page != null) {
   result = callCustomFunction(page);
  }
  if ( result != null ) {
   PrintWriter pw = response.getWriter();
   pw.print(result);
  }
 }

 private String callCustomFunction(Page page ) {
  try {
   Configuration config = campaignConnector.getWebserviceConfig(page.getContentResource().getParent());
   CampaignCredentials credentials = campaignConnector.retrieveCredentials(config);

   Map<String, String> params = new HashMap<String, String>();
   params.put("origin", "AEM");
   CallResults results = campaignConnector.callGeneric("/jssp/cus/custom.jssp", params, credentials);
   return results.bodyAsString();
  } catch (Exception e ) {
   log.error("Something went wrong during the connection", e);
  }
  return null;

 }

}
```

## Exempel 2: Adobe Campaign till AEM {#example-adobe-campaign-to-aem}

AEM erbjuder API:er som kan hämta objekt var som helst i platadminutforskarvyn.

![chlimage_1-17](assets/chlimage_1-17a.png)

>[!NOTE]
>
>[Det här exemplet finns i Geometrixx](/help/sites-developing/we-retail.md), som är tillgängligt från Paketdelning.

För varje nod i Utforskaren finns det ett API som är länkat till det. För noden:

* [http://localhost:4502/siteadmin#/content/campaigns/geometrixx/scott-recommends](http://localhost:4502/siteadmin#/content/campaigns/geometrixx/scott-recommends)

API:t är:

* [http://localhost:4502/content/campaigns/geometrixx/scott-recommends.1.json](http://localhost:4502/content/campaigns/geometrixx/scott-recommends.2.json)

Slutet av URL:en **.1.json** kan ersättas med **.2.json**, **.3.json**, enligt antalet undernivåer som du är intresserad av att få. Om du vill få alla nyckelord kan du använda **infinity**:

* [http://localhost:4502/content/campaigns/geometrixx/scott-recommends.infinity.json](http://localhost:4502/content/campaigns/geometrixx/scott-recommends.2.json)

AEM använder som standard grundläggande autentisering för att förbruka API.

Ett JS-bibliotek med namnet **amcIntegration.js** är tillgängligt i 6.1.1 (build 8624 och senare) som implementerar den logiken bland flera andra.

### AEM API-anrop {#aem-api-call}

```java
loadLibrary("nms:amcIntegration.js");

var cmsAccountId = sqlGetInt("select iExtAccountId from NmsExtAccount where sName=$(sz)","aemInstance")
var cmsAccount = nms.extAccount.load(String(cmsAccountId));
var cmsServer = cmsAccount.server;

var request = new HttpClientRequest(cmsServer+"/content/campaigns/geometrixx.infinity.json")
aemAddBasicAuthentication(cmsAccount, request);
request.method = "GET"
request.header["Content-Type"] = "application/json; charset=UTF-8";
request.execute();
var response = request.response;
```
