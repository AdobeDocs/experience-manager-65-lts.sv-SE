---
title: Kontrollpaneler
description: Lär dig hur du skapar, konfigurerar och utvecklar nya AEM-kontrollpaneler.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '832'
ht-degree: 1%

---

# Kontrollpaneler{#dashboards}

När du använder AEM kan du hantera många olika typer av innehåll (till exempel sidor, resurser). AEM Dashboards är ett enkelt och anpassningsbart sätt att definiera sidor som visar konsoliderade data.

>[!NOTE]
>
>AEM Dashboards skapas per användare så att en användare bara kan komma åt sin egen kontrollpanel.
>
>[Instrumentpanelsmallar](#creating-a-dashboard-template) kan dock användas för att dela gemensam konfiguration och instrumentpanelslayout.

![chlimage_1-22](assets/chlimage_1-22.jpeg)

## Administrera kontrollpaneler {#administering-dashboards}

### Skapa en kontrollpanel {#creating-a-dashboard}

1. Klicka på **Konfigurationskonsol** i avsnittet **Verktyg**.
1. Dubbelklicka på **instrumentpanelen** i trädet.
1. Klicka på **Ny instrumentpanel**.
1. Skriv **Titel** (till exempel Min instrumentpanel) och **Namn**.
1. Klicka på **Skapa**.

### Klona en instrumentpanel {#cloning-a-dashboard}

Du kanske vill ha flera kontrollpaneler för att snabbt se information om ditt innehåll från olika vyer. AEM har en klonfunktion som du kan använda för att duplicera en befintlig Dashboard så att du enklare kan skapa en ny Dashboard. Så här klonar du en kontrollpanel:

1. Klicka på **Konfigurationskonsol** i avsnittet **Verktyg**.

1. Klicka på **Instrumentpanel** i trädet.
1. Klicka på den kontrollpanel som du vill klona.

1. Klicka på **Klona**.

1. Ange **namnet** för den nya instrumentpanelen.

### Ta bort en instrumentpanel {#removing-a-dashboard}

1. Klicka på **Konfigurationskonsol** i avsnittet **Verktyg**.

1. Klicka på **Instrumentpanel** i trädet.
1. Klicka på den kontrollpanel som du vill ta bort.

1. Klicka på **Ta bort**.

1. Bekräfta genom att klicka på **Ja**.

## Kontrollpanelskomponenter {#dashboard-components}

### Ökning {#overview}

Instrumentpanelskomponenter är bara vanliga [AEM-komponenter](/help/sites-developing/developing-components-samples.md). I det här avsnittet beskrivs rapportkomponenter som levereras med AEM.

### Rapporteringskomponenter för Web Analytics {#web-analytics-reporting-components}

AEM levereras med en uppsättning komponenter som återger flera mätvärden för dina [SiteCatalyst](/help/sites-administering/adobeanalytics.md) -data. Dessa komponenter listas i Sidekick under **Kontrollpanelen** .

Varje rapportkomponent har minst tre flikar:

* **Grundläggande**: innehåller huvudkonfigurationen.

* **Rapport:** innehåller konfigurationen som är specifik för varje rapport.
* **Format**: innehåller formatkonfiguration som diagramstorlek och marginal.

Rapporteringskomponenterna initieras med en standardkonfiguration som hjälper dig att snabbt konfigurera instrumentpanelen.

#### Grundkonfiguration {#basic-configuration}

Fliken **Grundläggande** ger åtkomst till följande konfigurationsposter:

**Titel** Titeln som visas på instrumentpanelen.

**Typ av begäran** Det sätt på vilket data begärs.

**SiteCatalyst-konfiguration (valfritt)** Den konfiguration du vill använda för att ansluta till SiteCatalyst. Om den inte anges antas konfigurationen vara konfigurerad på instrumentpanelssidan (via sidegenskaper).

**Report Suite ID (valfritt)** SiteCatalyst-rapportsviten som du vill använda för att generera diagrammet.

#### Rapportkonfiguration {#report-configuration}

Om du vill visa webbstatistik måste du definiera datumintervallet för de data som du vill hämta. Fliken **Rapport** innehåller två fält som definierar det intervallet.

>[!NOTE]
>
>Om du anger ett stort datumintervall kan kontrollpanelens svarstider försämras.

**Datum från** Absolut eller relativt datum som data hämtas från.

**Datum till** Absolut eller relativt datum som data hämtas till.

Varje komponent definierar också specifika inställningar.

#### Övertidsrapport {#overtime-report}

![chlimage_1-26](assets/chlimage_1-26a.png)

**Datumgranularitet** Tidsenhet för X-axeln (till exempel dag, timme).

**Mätvärden** Listan med händelser som du vill visa.

**Element** Listan med element som bryter ned måttdata i diagrammet.

#### Rankad lista - rapport {#ranked-list-report}

![chlimage_1-27](assets/chlimage_1-27a.png)

**Element** Elementet som bryter ned måttdata i diagrammet.

**Metrisk** Händelsen som du vill visa.

**Nej. av de översta objekten** Antal objekt som visas av rapporten.

#### Rankad rapport {#ranked-report}

![chlimage_1-28](assets/chlimage_1-28a.png)

**Metrisk** Händelsen som du vill visa.

**Element** Elementet som bryter ned måttdata i diagrammet.

#### Avsnittsrapport för översta webbplatsen {#top-site-section-report}

Den här komponenten visar ett diagram över det mer besökta avsnittet på en webbplats enligt följande konfiguration.

![chlimage_1-29](assets/chlimage_1-29a.png)

**Nej. av de översta objekten** Antal avsnitt som visas i rapporten.

#### Trendrapport {#trended-report}

![chlimage_1-30](assets/chlimage_1-30a.png)

**Datumgranularitet** Tidsenhet för X-axeln (till exempel dag, timme).

**Metrisk** Händelsen som du vill visa.

**Element** Elementet som bryter ned måttdata i diagrammet.

## Utöka instrumentpanel {#extending-dashboard}

### Ökning {#overview-1}

Instrumentpaneler är normala sidor ( `cq:Page`), och därför kan alla komponenter användas för att montera instrumentpaneler.

Det finns en standardkomponentgrupp `Dashboard` som innehåller analysrapportkomponenter som är aktiverade i mallen som standard.

### Skapa en instrumentpanelsmall {#creating-a-dashboard-template}

En mall definierar standardinnehållet för en ny kontrollpanel. Du kan använda flera mallar för att skapa olika typer av kontrollpaneler.

Instrumentpanelsmallar skapas som andra sidmallar, förutom att de lagras under `/libs/cq/dashboards/templates/`. Se avsnittet [Skapa innehållsidesmall](/help/sites-developing/website.md#creating-the-contentpage-template).

>[!NOTE]
>
>Instrumentpanelsmallar delas mellan användare.

### Utveckla en kontrollpanelskomponent {#developing-a-dashboard-component}

Du utvecklar en Dashboard-komponent genom att skapa en vanlig AEM-komponent. I det här avsnittet beskrivs ett exempel på en komponent som visar de 10 viktigaste deltagarna.

![chlimage_1-31](assets/chlimage_1-31a.png)

De översta författarkomponenterna lagras i databasen på `/apps/geometrixx-outdoors/components/reporting` och består av:

1. en `jsp`-fil som läser jcr-data och definierar platshållaren `html`.

1. ett klientbibliotek som innehåller en `js`-fil som hämtar och beställer data och fyller sedan i platshållaren `html`.

![chlimage_1-32](assets/chlimage_1-32a.png)

Följande JavaScript-fil definieras i `geout.reporting.topauthors` [klientbiblioteket](/help/sites-developing/clientlibs.md) som underordnad till själva komponenten.

[QueryBuilder](/help/sites-developing/querybuilder-api.md) används för att fråga databasen om att läsa `cq:AuditEvent`-noder. Frågeresultatet är ett JSON-objekt från vilket författarbidrag extraheras.

#### top_authors.js {#top-authors-js}

```
$.ajax({
  url: "/bin/querybuilder.json",
  cache: false,
  data: {
       "orderby": "cq:time",
       "orderby.sort": "desc",
       "p.hits": "full",
       "p.limit": 100,
       "path": "/var/audit/com.day.cq.wcm.core.page/",
       "type": "cq:AuditEvent"
   },
  dataType: "json"
}).done(function( res ) {
    var authors = {};
    // from JSON to Object
    for(var r in res.hits) {
        var userId = res.hits[r].userId;
        if(userId == undefined) {
            continue;
        }
        var auth = authors[userId] || {userId : userId};
        auth.contrib = (auth.contrib || 0) +1;

        authors[userId] = auth;
    }

    // order by contribution
    var orderedByContrib = [];
    for(var a in authors) {
        orderedByContrib.push(authors[a]);
    }
    orderedByContrib.sort(function(a,b){return b.contrib - a.contrib});

    // produce the list
    for (var i=0, tot=orderedByContrib.length; i < tot; i++) {
        var current = orderedByContrib[i];
        $("<div> #" + (i + 1) +" "+ current.userId + " (" + current.contrib +" contrib.)</div>").appendTo("#authors-list");

    }
});
```

`JSP` innehåller både `global.jsp` och `clientlib`.

#### top_authors.jsp {#top-authors-jsp}

```java
<%@page session="false" contentType="text/html; charset=utf-8" %><%
%><%
%><%@include file="/libs/foundation/global.jsp" %><%
%>
<ui:includeClientLib categories="geout.reporting.topauthors" />
<%
String reportletTitle = properties.get("title", "Top Authors");
%>
<html>
     <h3><%=xssAPI.encodeForHTML(reportletTitle) %></h3>
     <div id="authors-list"></div>
</html>
```
