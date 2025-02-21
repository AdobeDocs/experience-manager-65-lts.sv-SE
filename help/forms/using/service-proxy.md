---
title: HTML5 Form Service Proxy
description: HTML5 Forms Service Proxy är en konfiguration som registrerar en proxy för överföringstjänsten. Om du vill konfigurera tjänstproxy anger du URL:en för överföringstjänsten via parametern submitServiceProxy för begäran.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# HTML5 Form Service Proxy{#html-forms-service-proxy}

HTML5 Forms Service Proxy är en konfiguration som registrerar en proxy för överföringstjänsten. Om du vill konfigurera tjänstproxy anger du URL:en för överföringstjänsten via begärandeparametern *submitServiceProxy*.

## Fördelar med tjänstproxy {#benefits-of-service-proxy-br}

Tjänstproxyn eliminerar följande:

* HTML5-formulärarbetsflödet kräver att skicka in tjänsten &quot;/content/xfaforms/submission/default&quot; för HTML5-formuläranvändare. Den exponerar AEM-servrar för en större, oavsiktlig publik.
* Tjänst-URL:en är inbäddad i formulärets körningsmodell. Det går inte att ändra tjänstens URL-sökväg.
* Inlämningen är en tvåstegsprocess. För att skicka in formulärdata krävs minst två resor till servern. Ökar därmed belastningen på servern.
* HTML5-formulär skickar data i POST-begäran i stället för i PDF-begäran. För arbetsflöden som omfattar både PDF- och HTML5-formulär krävs två olika metoder för att behandla inskickade data.

### Topologies {#topologies-br}

HTML5-formulär kan använda följande topologier för att ansluta till AEM-servrarna.

* En topologi där AEM Server eller HTML5-formulär skickar data via POST till servern.
* En topologi där proxyservern skickar POST-data till servern.

![Proxytopologier för HTML5-formulärtjänst](assets/topology.png)

Proxytopologier för HTML5-formulärtjänster

HTML5-formulär ansluter till AEM-servrarna för att köra serverbaserade skript, webbtjänster och överföringar. XFA-miljön för HTML5-formulären använder Ajax-anrop på &quot;/bin/xfaforms/submit&quot;-slutpunkten med olika parametrar för att ansluta till AEM-servrarna. HTML5-formulär ansluter AEM-servrar för att utföra följande åtgärder:

#### Kör serverbaserade skript och webbtjänster {#execute-server-sided-scripts-and-web-services}

De skript som är markerade för att köras på servern kallas för serverbaserade skript. I följande tabell visas alla parametrar som används i serverbaserade skript och webbtjänster.

<table>
 <tbody>
  <tr>
   <td><p><strong>Parameter</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p>aktivitet</p> </td>
   <td><p>Aktiviteten innehåller de händelser som utlöser begäran. Till exempel klicka, avsluta eller ändra</p> </td>
  </tr>
  <tr>
   <td><p>contextAs</p> </td>
   <td><p>contextSOM innehåller SOM-uttryck för objektet där händelser körs.</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>Mallen innehåller mallen som används för att återge formuläret.</p> </td>
  </tr>
  <tr>
   <td><p>contentRoot</p> </td>
   <td><p>contentRoot innehåller mallens rotkatalog som används för att återge formuläret.</p> </td>
  </tr>
  <tr>
   <td><p>Data</p> </td>
   <td><p>Data innehåller byte med data som används för att återge formuläret.</p> </td>
  </tr>
  <tr>
   <td><p>formDom</p> </td>
   <td><p>formDom innehåller DOM för HTML5-formuläret i JSON-format.</p> </td>
  </tr>
  <tr>
   <td><p>paket</p> </td>
   <td><p>Paketet anges som formulär.</p> </td>
  </tr>
  <tr>
   <td><p>debugDir</p> </td>
   <td><p>debugDir innehåller den felsökningskatalog som används för att återge formuläret.</p> </td>
  </tr>
 </tbody>
</table>

#### Skicka data {#submit-data}

När du klickar på skicka-knappen skickar HTML5-formulär data till servern. I följande tabell visas alla parametrar som HTML5-formulär skickar till servern.

<table>
 <tbody>
  <tr>
   <td><p><strong>Parameter</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>Mall som används för att återge formuläret.</p> </td>
  </tr>
  <tr>
   <td><p>contentRoot</p> </td>
   <td><p>mallens rotkatalog som används för att återge formuläret.</p> </td>
  </tr>
  <tr>
   <td><p>Data</p> </td>
   <td><p>byte som används för att återge formuläret.</p> </td>
  </tr>
  <tr>
   <td><p>formDom</p> </td>
   <td><p>DOM för HTML5-formuläret i JSON-format.</p> </td>
  </tr>
  <tr>
   <td><p>underordnad</p> </td>
   <td><p>URL:en där data-XML publiceras.</p> </td>
  </tr>
  <tr>
   <td><p>debugDir</p> </td>
   <td><p>Den felsökningskatalog som används för att återge formuläret.</p> </td>
  </tr>
 </tbody>
</table>

#### Hur skicka-proxyn fungerar? {#how-nbsp-the-nbsp-submit-proxy-works}

Skicka-tjänstproxyn fungerar som ett genomströmningsalternativ om det inte finns någon skicka-URL i request-parametern. Det fungerar som en genomströmning. Begäran skickas till slutpunkten för /bin/xfaforms/submit och svaret skickas till XFA-miljön.

Skicka-tjänstproxyn väljer en topologi om den skicka-URL:en finns i request-parametern.

* Om AEM-servrar skickar data fungerar proxytjänsten som en vidarekoppling. Begäran skickas till slutpunkten för /bin/xfaforms/submit och svaret skickas till XFA-miljön.
* Om proxyn skickar data, skickar proxytjänsten alla parametrar utom submitUrl till slutpunkten */bin/xfaforms/submit* och tar emot xml-byte i svarsströmmen. Sedan skickar proxytjänsten data-xml-byte till submitUrl för bearbetning.

* Innan data skickas (POST-begäran) till en server kontrollerar HTML5-formulär serverns anslutning och tillgänglighet. För att verifiera anslutning och tillgänglighet skickar HTML-formulär en tom huvudbegäran till servern. Om servern är tillgänglig skickar HTML5-formuläret data (POST-begäran) till servern. Om servern inte är tillgänglig visas ett felmeddelande, *Kunde inte ansluta till servern,*. Avancerad identifiering förhindrar att användarna behöver fylla i formuläret på ett enkelt sätt. Proxyservern hanterar huvudbegäran och genererar inget undantag.
