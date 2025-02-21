---
title: Implementera sidnamngivning på serversidan för analys
description: Adobe Analytics använder egenskapen s.pageName för att unikt identifiera sidor och för att associera data som samlas in för sidorna
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '847'
ht-degree: 0%

---

# Implementera sidnamngivning på serversidan för analys{#implementing-server-side-page-naming-for-analytics}

Adobe Analytics använder egenskapen `s.pageName` för att unikt identifiera sidor och för att associera data som samlas in för sidorna. Vanligtvis utför du följande uppgifter i AEM för att tilldela den här egenskapen ett värde som AEM skickar till Analytics:

* Använd molntjänstramverket för Analytics för att mappa en CQ-variabel till egenskapen `s.pageName` för Analytics. (Se [Mappa komponentdata med Adobe Analytics-egenskaper](/help/sites-administering/adobeanalytics-mapping.md).)

* Designa sidkomponenten så att den innehåller CQ-variabeln som du mappar till egenskapen `s.pageName`. (Se [Implementera Adobe Analytics-spårning för anpassade komponenter](/help/sites-developing/extending-analytics-components.md).)

Om du vill visa analysrapportdata i webbplatskonsolen och i Content Insight, kräver AEM värdet för egenskapen `s.pageName` för varje sida. Java-API:t för AEM Analytics definierar det `AnalyticsPageNameProvider`-gränssnitt som du implementerar för att ge webbplatskonsolen och innehållsinsikter värdet för egenskapen `s.pageName`. Tjänsten `AnaltyicsPageNameProvider` löser egenskapen pageName på servern för rapportering, eftersom den kan ställas in dynamiskt med JavaScript på klienten för spårning.

## Sidnamnsprovidertjänsten för standardanalys {#the-default-analytics-page-name-provider-service}

Tjänsten `DefaultPageNameProvider` är standardtjänst som avgör värdet på egenskapen `s.pageName` som ska användas för att hämta Analytics-data för en sida. Tjänsten fungerar tillsammans med AEM Foundation page-komponent ( `/libs/foundation/components/page`). Den här sidkomponenten definierar följande CQ-variabler som ska mappas till egenskapen `s.pageName`:

* `pagedata.path`: Värdet är inställt på sidsökvägen.
* `pagedata.title`: Värdet ställs in på sidrubriken.
* `pagedata.navTitle`: Värdet ställs in på sidnavigeringsrubriken.

Tjänsten `DefaultPageNameProvider` avgör vilken av dessa CQ-variabler som mappas till egenskapen `s.pageName` i molntjänstramverket för Analytics. Tjänsten avgör sedan vilken sidegenskap som ska användas för att hämta analysrapportdata:

* `pagedata.path`: Tjänsten använder `page.getPath()`

* `pagedata.title`: Tjänsten använder `page.getTitle()`

* `pagedata.navTitle`: Tjänsten använder `page.getNavigationTitle()`

Objektet `page` är Java-objektet [`com.day.cq.wcm.api.Page`](https://helpx.adobe.com/experience-manager/6-3/sites-developing/reference-materials/javadoc/com/day/cq/wcm/api/Page.html) för sidan.

Om du inte mappar en CQ-variabel till egenskapen `s.pageName` i ramverket genereras värdet för `s.pageName` från sidsökvägen. Sidan med sökvägen `/content/geometrixx/en` använder till exempel värdet `content:geometrixx:en` för `s.pageName`.

>[!NOTE]
>
>Tjänsten DefaultPageNameProvider använder en rankning på 100.

## Bevara kontinuitet i analysrapporter {#maintaining-continuity-in-analytics-reporting}

För att upprätthålla en komplett historik med analysdata för en sida måste värdet för egenskapen s.pageName som används för en sida aldrig ändras. Det är dock enkelt att ändra de analysegenskaper som definieras av bassidans komponent. Om du till exempel flyttar en sida ändras värdet för `pagedata.path` och kontinuiteten i rapporthistoriken bryts:

* Data som samlades in för föregående sökväg är inte längre kopplade till sidan.
* Om en annan sida använder sökvägen som en annan sida använde, ärver den andra sidan informationen för den sökvägen.

För att säkerställa kontinuitet i rapporteringen bör värdet för `s.pageName` ha följande egenskaper:

* Unik.
* Stabil.
* Kan läsas av människor.

En anpassad sidkomponent kan till exempel innehålla en sidegenskap som författare använder för att ange ett unikt ID för sidan som används som värde för egenskapen `s.pageProperties`:

* Sidan innehåller en analysvariabel som är inställd på värdet för det unika ID som lagras i sidegenskapen.
* Analysvariabeln mappas till egenskapen `s.pageProperties` i Analytics-ramverket.
* Din implementering av gränssnittet AnalyticsPageNameProvider hämtar värdet på sidegenskapen som ska användas för att fråga efter sidanalysdata.

>[!NOTE]
>
>Be din Analytics-konsult om hjälp med att utveckla en effektiv strategi för ditt `s.pageName`-värde.

### Implementera en tjänst för leverantör av sidnamn för analyser {#implementing-an-analytics-page-name-provider-service}

Implementera gränssnittet `com.day.cq.analytics.sitecatalyst.AnalyticsPageNameProvider` som en OSGi-tjänst för att anpassa logiken som hämtar egenskapsvärdet `s.pageName`. Webbplatssidans analys och Content Insight använder tjänsten för att hämta rapportdata från Analytics.

Gränssnittet AnalyticsPageNameProvider definierar två metoder som du måste implementera:

* `getPageName`: Returnerar ett `String`-värde som representerar värdet som ska användas som `s.pageName`-egenskap.

* `getResource`: Returnerar ett `org.apache.sling.api.resource.Resource`-objekt som representerar sidan som är associerad med egenskapen `s.pageName`.

Båda metoderna använder ett `com.day.cq.analytics.sitecatalyst.AnalyticsPageNameContext`-objekt som parameter. Klassen `AnalyticsPageNameContext` innehåller information om kontexten för analysanropen:

* Sidresursens grundsökväg.
* Objektet `Framework` för molntjänstkonfigurationen för Analytics.
* `Resource`-objektet för sidan.
* `ResourceResolver`-objektet för sidan.

Klassen innehåller också en set-metod för sidnamnet.

### Exempel på implementering av AnalyticsPageNameProvider {#example-analyticspagenameprovider-implementation}

I följande exempel `AnalyticsPageNameProvider`-implementering stöds en anpassad sidkomponent:

* Komponenten utökar bassidans komponent.
* Dialogrutan innehåller ett fält som författare använder för att ange värdet för egenskapen `s.pageName`.
* Egenskapsvärdet lagras i egenskapen pageName för noden `jcr:content` i sidinstanserna.
* Analysegenskapen som lagrar egenskapen `s.pageName` kallas `pagedata.pagename`. Den här egenskapen är mappad till egenskapen `s.pageName` i Analytics-ramverket.

Följande implementering av metoden `getPageName` returnerar värdet för nodegenskapen pageName om ramverksmappningen är korrekt konfigurerad:

```java
public String getPageName(AnalyticsPageNameContext context) {
        String pageName = null;

        Framework framework = context.getFramework();
        Resource resource = context.getResource();

        if (resource != null && framework != null && framework.mapsSCVariable(S_PAGE_NAME)) {
            String cqVar = framework.getMapping(S_PAGE_NAME);
            Page page = resource.adaptTo(Page.class);
            if (cqVar.equals("pagedata.pagename")) {
                pageName = page.getProperties().get("pageName",null);
            }
        }
        return pageName;
    }
```

Följande implementering av metoden getResource returnerar resursobjektet för sidan:

```java
     public Resource getResource(AnalyticsPageNameContext context) {
        Resource res = null;

        Framework framework = context.getFramework();
        ResourceResolver resolver = context.getResourceResolver();
        String pageName = context.getPageName();
        String basePath = context.getBasePath();

        if (pageName != null && basePath != null && resolver != null
                && framework != null && framework.mapsSCVariable(S_PAGE_NAME)) {
            String cqVar = framework.getMapping(S_PAGE_NAME);
            if (cqVar.equals("pagedata.pagename")) {
             Iterator<Resource>
             hits = resolver.findResources(createQuery(pageName, basePath, "pagename"), Query.JCR_SQL2);
             if (hits.hasNext()) {
              res = hits.next();
              res = res.getParent();
             }
            }
        }
        return res;
    }

    private String createQuery(String pageName, String basePath, String propName) {
        return "SELECT * FROM [cq:PageContent] WHERE ISDESCENDANTNODE(["
                + basePath + "]) and [" + propName + "] = \"" + pageName + "\"";
    }
```

Följande kod representerar hela klassen, inklusive SCR-anteckningar som konfigurerar tjänsten. Tjänstrankningen är 200 vilket åsidosätter standardtjänsten.

```java
/*************************************************************************
 *
 * ADOBE CONFIDENTIAL
 * __________________
 *
 * Copyright 2019 Adobe Systems Incorporated
 * All Rights Reserved.
 *
 * NOTICE:  All information contained herein is, and remains
 * the property of Adobe Systems Incorporated and its suppliers,
 * if any.  The intellectual and technical concepts contained
 * herein are proprietary to Adobe Systems Incorporated and its
 * suppliers and may be covered by U.S. and Foreign Patents,
 * patents in process, and are protected by trade secret or copyright law.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe Systems Incorporated.
 **************************************************************************/
package com.day.cq.analytics.sitecatalyst;

import java.util.Iterator;

import javax.jcr.query.Query;

import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;

import com.day.cq.analytics.sitecatalyst.AnalyticsPageNameContext;
import com.day.cq.analytics.sitecatalyst.AnalyticsPageNameProvider;
import com.day.cq.analytics.sitecatalyst.Framework;
import com.day.cq.wcm.api.Page;

import static com.day.cq.analytics.sitecatalyst.AnalyticsPageNameContext.S_PAGE_NAME;

/**
 * Default implementation of {@link AnalyticsPageNameProvider} that resolves
 * page title, path or navTitle if mapped in {@link Framework}.
 */
@Component(
    service = { AnalyticsPageNameProvider.class },
    property = {
        Constants.SERVICE_DESCRIPTION + "=Example Page Name Resolver implementation",
        Constants.SERVICE_RANKING + ":Integer=200"
    }
)
public class ExamplePageNameProvider implements AnalyticsPageNameProvider {
    public String getPageName(AnalyticsPageNameContext context) {
        String pageName = null;

        Framework framework = context.getFramework();
        Resource resource = context.getResource();

        if (resource != null && framework != null && framework.mapsSCVariable(S_PAGE_NAME)) {
            String cqVar = framework.getMapping(S_PAGE_NAME);
            Page page = resource.adaptTo(Page.class);
            if (cqVar.equals("pagedata.path")) {
                pageName = page.getProperties().get("pageName",null);
            }
        }
        return pageName;
    }

    public Resource getResource(AnalyticsPageNameContext context) {
        Resource res = null;

        Framework framework = context.getFramework();
        ResourceResolver resolver = context.getResourceResolver();
        String pageName = context.getPageName();
        String basePath = context.getBasePath();

        if (pageName != null && basePath != null && resolver != null
                && framework != null && framework.mapsSCVariable(S_PAGE_NAME)) {
            String cqVar = framework.getMapping(S_PAGE_NAME);
            if (cqVar.equals("pagedata.pagename")) {
                Iterator<Resource>
                hits = resolver.findResources(createQuery(pageName, basePath, "pagename"), Query.JCR_SQL2);
                if (hits.hasNext()) {
                    res = hits.next();
                    res = res.getParent();
                }
            }
        }
        return res;
    }

    private String createQuery(String pageName, String basePath, String propName) {
        return "SELECT * FROM [cq:PageContent] WHERE ISDESCENDANTNODE(["
                + basePath + "]) and [" + propName + "] = \"" + pageName + "\"";
    }
}
```
