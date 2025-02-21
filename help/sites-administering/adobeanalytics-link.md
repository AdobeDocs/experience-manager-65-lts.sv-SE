---
title: Konfigurera länkspårning för Adobe Analytics
description: Lär dig hur du konfigurerar länkspårning för SiteCatalyst.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1615'
ht-degree: 0%

---


# Konfigurera länkspårning för Adobe Analytics{#configuring-link-tracking-for-adobe-analytics}

När användare klickar på länkar på webbplatsens sidor kan du hämta relaterad information i Adobe Analytics. Använd till exempel länkspårning för att lära dig hur användare interagerar med webbplatsen, spåra filhämtningar och spåra avslutslänkar.

## Konfigurera länkspårning för ett Adobe Analytics Framework {#configuring-link-tracking-for-an-adobe-analytics-framework}

1. Använd **Navigering**, gå via **Distribution**, **molntjänster** till avsnittet **Adobe Analytics**.

1. Använd **Visa konfigurationer** och öppna det Adobe Analytics-ramverk som krävs.
1. Utöka avsnittet **Konfiguration av länkspårning** och konfigurera efter behov (den här sidan innehåller mer information):

   ![Analysramverk](assets/aa-08.png)

## Spåra filhämtningar {#tracking-file-downloads}

Konfigurera Adobe Analytics-ramverket så att filer som hämtas från associerade sidor automatiskt spåras som hämtningar i Adobe Analytics. När du aktiverar spårning av hämtningar spåras bara de filtyper som du anger.

Nedladdningar av följande filtyper spåras som standard:

* exe
* zip
* wav
* mp3
* mov
* mpg
* avi
* wmv
* doc
* PDF
* xls

Om du t.ex. har aktiverat hämtningsspårning för PDF-filer spåras hämtningen av PDF när användare klickar på länkar till PDF-filer.

Ramverkets egenskaper för hämtningsspårning implementeras som kod i filen `analytics.sitecatalyst.js` som genereras för en sida. Följande kodexempel representerar standardkonfigurationen för hämtningsspårning:

```
s.trackDownloadLinks= true;
s.linkDownloadFileTypes= 'exe,zip,wav,mp3,mov,mpg,avi,wmv,doc,pdf,xls';
```

Så här aktiverar du hämtningsspårning för ditt Adobe Analytics-ramverk:

1. [Öppna Adobe Analytics-ramverket och expandera konfigurationsavsnittet Länkspårning](#configuring-link-tracking-for-an-adobe-analytics-framework).
1. Aktivera **Spåra hämtningar**.
1. I rutan **Hämta filtyper** skriver du filnamnstilläggen för de filtyper som du vill spåra.

## Spåra externa länkar {#tracking-external-links}

Du kan spåra klickningen på externa länkar (avsluta länkar) på sidorna.

Så här spårar du externa länkar för ditt Adobe Analytics-ramverk:

1. [Öppna Adobe Analytics-ramverket och utöka **Konfiguration för länkspårning** ](#configuring-link-tracking-for-an-adobe-analytics-framework).
1. Konfigurera följande egenskaper enligt dina krav.

Egenskaper för spårning när du klickar på externa länkar:

* **Spåra externa**
Aktivera extern länkspårning.

* **Externa filter**
(Valfritt) Definierar filter för att matcha de externa URL:erna för länkmålen. När länkmålen matchar filtret spåras länken. Externa filter är användbara när du bara vill spåra vissa av de externa länkarna på sidorna.

  Om du vill ange de externa länkar som ska spåras skriver du hela eller delar av länkmålets URL. Separera flera filter med kommatecken. Omsluter stränglitteraler inom enkla citattecken. Inget värde (standardvärdet `''`, två enkla citattecken) gör att alla externa länkar spåras.

* **Interna filter**
Definierar filter för att matcha URL:er för interna länkar. När länken pekar mot URL:er som matchar det här filtret spåras inte länken. Standardvärdet är ett javascript-kommando som returnerar värdnamnet för URL:en för den aktuella fönsteradressen.

  Om du vill ange interna länkar som inte spåras skriver du hela eller en del av länkmålets interna URL. Separera flera filter med kommatecken. Omsluter stränglitteraler inom enkla citattecken.

  Standardvärdet är `'javascript:,'+window.location.hostname`

* **Lämna frågesträng**
Inkluderar URL-parametrar vid utvärdering av matchningar med interna och externa filter.

  Aktivera om du vill inkludera URL-parametrar när du utvärderar URL:er för länkmål mot externa och interna filter.

Egenskaperna för extern länkspårning implementeras som kod i filen `analytics.sitecatalyst.js` som genereras för en sida. Följande exempelkod genereras för en sida som är associerad med ett ramverk som har aktiverat extern länkspårning med följande konfiguration:

* Det externa filtret är `'google.com'`
* Internt filter är standardvärdet för `'javascript:,'+window.location.hostname`
* Frågesträngar inkluderas inte när länkmålet utvärderas mot filter.

```
s.trackExternalLinks= false;
s.linkExternalFilters= 'google.com';
s.linkInternalFilters= 'javascript:,'+window.location.hostname;
s.linkLeaveQueryString= false;
```

## Skicka variabeldata med länkklick {#sending-variable-data-with-link-clicks}

Du kan konfigurera AEM att skicka händelse- och variabeldata till Adobe Analytics när en användare klickar på en länk. Med egenskaperna **Konfiguration för länkspårning** kan du ange vilka Adobe Analytics-händelser och -variabler som ska spåras när länkklick inträffar.

Ramverksmappningarna bestämmer händelse- och variabelvärdena. Du kan mappa Adobe Analytics-variabler till de variabler i innehållskomponenterna som lagrar data som du vill spåra när någon klickar på länkarna.

Så här skickar du variabeldata med länkklick:

1. [Öppna Adobe Analytics-ramverket och expandera konfigurationsavsnittet Länkspårning](#configuring-link-tracking-for-an-adobe-analytics-framework).
1. Konfigurera följande egenskaper enligt dina krav.

Egenskaper för att skicka variabeldata med länkklick:

* **Länkspårningshändelser**
Ange de Adobe Analytics-händelsevariabler som du vill använda för att räkna länkklick.

  Avgränsa flera variabelnamn med komma.

  Standardvärdet `None` orsakar ingen händelsespårning.

* **Länkspårsvariabler**
Ange de Adobe Analytics-variabler som du vill skicka till Adobe Analytics när någon klickar på länkarna. Avgränsa flera variabelnamn med komma.

  Standardvärdet `None` gör att inga variabeldata skickas.

När du anger händelser och variabler som ska skickas implementeras konfigurationen som kod i filen `analytics.sitecatalyst.js` som genereras för en sida. Följande exempelkod genereras för en sida när ramverket spårar händelsen `event10` och egenskapen `prop4`:

```
s.linkTrackEvents= 'event10';
s.linkTrackVars= 'prop4';
```

## Exempel på konfiguration för länkspårning {#example-link-tracking-configuration}

Utför följande procedurer för att utforska länkspårningsbeteendet i Adobe Analytics-integreringen. Processerna visar resultat från [Adobe Marketing Cloud Debugger](https://experienceleague.adobe.com/docs/debugger/using/experience-cloud-debugger.html).

### Allmän konfiguration {#general-configuration}

I det här exemplet visas hur mappningen fungerar för spårning och felsökning:

1. Öppna det ramverk som har kopplats till en webbsida.
1. Dra **Page**-komponenten till ramverkets mappningsområde. Komponenten **Sida** tillhör komponentgruppen **Allmänt** i Sidekick.

   >[!NOTE]
   >
   >Vilken komponent du ska använda i ett verkligt scenario beror på vilken komponent som ärvs från (om det är någon).
   >
   >Om inte bör du ha en egen komponent exponerad där (genom att definiera en analytisk undernod i dess sidkomponent).

   Konfigurera mappningen enligt följande tabell genom att dra variabeln Analytics (SiteCatalyst) från den vänstra panelen:

<table>
 <tbody>
  <tr>
   <th>CQ-variabel <br /> </th>
   <th>Post i variabelbläddraren <br /> </th>
   <th>Adobe Analytics Variable</th>
  </tr>
  <tr>
   <td>pagedata.title</td>
   <td>Anpassad eVar 1 (eVar1)</td>
   <td>eVar1</td>
  </tr>
  <tr>
   <td>eventdata.events.pageView</td>
   <td>Anpassad 1 (händelse1)</td>
   <td>event1</td>
  </tr>
 </tbody>
</table>

1. Dra sökkomponenten till ramverkets mappningsområde. Sökkomponenten tillhör komponentgruppen Allmänt i Sidekick. Konfigurera mappningen enligt följande tabell genom att dra variabeln Analytics (SiteCatalyst) från den vänstra panelen:

<table>
 <tbody>
  <tr>
   <th>CQ-variabel <br /> </th>
   <th>Post i Variables Browser</th>
   <th>Adobe Analytics Variable</th>
  </tr>
  <tr>
   <td>eventdata.keyword</td>
   <td>Anpassad eVar 2 (eVar2)</td>
   <td>eVar2</td>
  </tr>
  <tr>
   <td>eventdata.results</td>
   <td>Anpassad eVar 3 (eVar3)</td>
   <td>eVar3</td>
  </tr>
  <tr>
   <td>eventdata.events.search</td>
   <td>Anpassad 2 (händelse2)</td>
   <td>event2</td>
  </tr>
 </tbody>
</table>

### Konfigurera extern länkspårning {#configure-external-link-tracking}

1. Utöka området **Konfiguration av länkspårning** i ditt ramverk.
1. Avmarkera **Spåra hämtningar**.

1. Välj **Spåra externt**.
1. Avmarkera **Lämna frågesträng**.
1. Använd följande värde för listan **Externa filter** för att identifiera den som en extern URL:

   `'yahoo.com'`

1. Lägg till följande värde i fältet **Länkspårningshändelser**:

   ```
       event1,event2
   ```

1. Lägg till följande värde i fältet **Länkspår**:

   ```
       eVar1,eVar2
   ```

1. Lägg till en **Text** -komponent på sidan som är associerad med ramverket. I komponenten **Text** lägger du till en hyperlänk som pekar på följande adress:

   `https://search.yahoo.com/?p=this`

1. Växla till **förhandsgranskningsläget** och klicka på länken.

Anropet ser ut så här när det visas med Adobe Marketing Cloud Debugger:

![Adobe Marketing Cloud Debugger](assets/aa-leavequerysearch-blank.png)

>[!NOTE]
>
>URL:en innehåller inte frågesträngen: `?p=this`

### Inkludera URL-parametern {#include-the-url-parameter}

1. Utöka området **Konfiguration för länkspårning** i ramverket.
1. Aktivera **Lämna frågesträng**.
1. Läs in förhandsgranskningen på nytt och klicka på länken.

Anropsinformationen som visas i Adobe Marketing Cloud Debugger liknar följande exempel:

![Adobe Marketing Cloud Debugger igen](assets/aa-leavequerysearch-active.png)

>[!NOTE]
>
>Den här gången innehåller URL:en frågesträngen: `?p=this`

## Ad hoc-länkspårning {#ad-hoc-link-tracking}

Med ad hoc-länkspårning kan författare konfigurera länkspårning för en komponent. Komponentens konfiguration åsidosätter ramverkets **länkspårningskonfiguration**, så på sidor som är kopplade till ramverket kan **Text** -komponenter konfigureras för länkspårning av URL:er.

Med ad hoc-länkspårning kan du spåra hämtningslänkar, externa länkar samt händelse- och variabeldata.

Om du vill aktivera ad hoc-länkspårning måste du:

* [Koppla sidan som innehåller komponenten **Text** till ramverket](/help/sites-administering/adobeanalytics-connect.md#associating-a-page-with-a-adobe-analytics-framework).
* [Konfigurera Adobe Analytics-ramverket för att aktivera ad hoc-länkspårning](#enabling-ad-hoc-link-tracking).
* [Konfigurera länkspårning för en textkomponent](#configuring-link-tracking-for-a-text-component).

### Aktivera ad hoc-länkspårning {#enabling-ad-hoc-link-tracking}

Konfigurera Adobe Analytics-ramverket för att aktivera ad hoc-länkspårning.

1. Öppna Adobe Analytics-ramverket och expandera avsnittet **Konfiguration för länkspårning**.

1. Aktivera **Ad-hoc-länkspårning**.

   >[!NOTE]
   >
   >Alla användartyper har inte åtkomst till den här kryssrutan. Kontakta webbplatsadministratören om du behöver åtkomst.

>[!NOTE]
>
>Konfigurationen av XSS-antisamyn finns nu i SLING under sökvägen **/libs/sling/xss.config.xml** och följande regler måste läggas till i ad hoc så att länkningen fungerar:

#### Regeltillägg för ankartagg {#anchor-tag-rule-extension}

```xml
<attribute name="onclick">
    <literal-list>
        <literal value="CQ_Analytics.Sitecatalyst.customTrack(this)"/>
    </literal-list>
</attribute>
<attribute name="adhocenable">
    <literal-list>
        <literal value="true"/>
        <literal value="false"/>
    </literal-list>
</attribute>
<attribute name="adhocevents">
    <regexp-list>
        <regexp name="anything"/>
    </regexp-list>
</attribute>
<attribute name="adhocevars">
    <regexp-list>
        <regexp name="anything"/>
    </regexp-list>
</attribute>
```

### Konfigurera länkspårning för en textkomponent {#configuring-link-tracking-for-a-text-component}

Innan du kan konfigurera ad hoc-länkspårning för själva **Text**-komponenterna måste följande konfigurationer ha implementerats:

* [Adobe Analytics-ramverket har konfigurerats för att aktivera ad hoc-länkspårning](#enabling-ad-hoc-link-tracking).
* Sidan [som innehåller komponenten **Text** är kopplad till ramverket ](/help/sites-administering/adobeanalytics-connect.md#associating-a-page-with-a-adobe-analytics-framework).

Använd följande procedur för att konfigurera länkspårning för en **Text** -komponent:

1. Öppna sidan i redigeringsläge och redigera komponenten **Text**.

1. Markera texten som du vill använda som hypertext och klicka på knappen Hyperlänk.

   ![Länkikon](do-not-localize/chlimage_1.png)

1. Lägg till mål-URL:en i rutan Länka till och utöka sedan området Länkspårning.

   >[!NOTE]
   >
   >Anpassad länkspårning visas som en separat åtgärd bredvid åtgärden Länka/Bryt länk (analysikonen).
   >
   >Den aktiveras bara när du har valt en giltig länk i textredigeraren.

   ![Aktivera länkspårning](assets/aa-17.png)

1. Aktivera **Anpassad länkspårning** om du vill åsidosätta länkspårningskonfigurationen för Adobe Analytics-ramverket och aktivera länkspårning för den aktuella länken.

1. (Valfritt) Om du vill spåra händelser med länkklickningen lägger du till Adobe Analytics-händelsenamn i fältet **Inkludera Adobe Analytics-variabler**. Separera flera händelsenamn med kommatecken, till exempel

   `event1, event22`.

1. (Valfritt) Om du vill spåra variabeldata med länkklickningen lägger du till Adobe Analytics-variabler i fältet **Inkludera Adobe Analytics-variabler**. Använd något av följande format:

   * *`<Variable-name>`*: *`<Dynamic Value>`*
   * *`<Variable-name>`*: *`'CONSTANT'`*

   I följande exempel visas varje format:

   * `eVar10:pagedata.title`
   * `prop1: 'Aubergine'`

   Avgränsa flera värden med komma.

1. Välj **OK**.
