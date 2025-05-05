---
title: Konfigurera videospårning för Adobe Analytics
description: Lär dig hur du konfigurerar videospårning för SiteCatalyst.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: 420dc7d6-0e9e-47be-baef-4c79296eb69a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1758'
ht-degree: 0%

---

# Konfigurera videospårning för Adobe Analytics{#configuring-video-tracking-for-adobe-analytics}

Det finns flera metoder för att spåra videohändelser, varav två är äldre alternativ för äldre versioner av Adobe Analytics. De här äldre alternativen är: Äldre milstolpar och Äldre sekunder.

>[!NOTE]
>
>Kontrollera att du har överfört **uppspelningsbar video** inom AEM innan du fortsätter.
>
>Om du vill vara säker på att dina videofilmer spelas upp på sidan kan du läsa **[den här självstudiekursen](/help/sites-authoring/default-components-foundation.md#video)** och få information om hur du kodar om videofiler i AEM.

Använd följande procedur för att konfigurera ett ramverk för videospårning med hjälp av varje metod.

>[!NOTE]
>
>För nya implementeringar rekommenderar vi att du **inte använder** de äldre alternativen för videospårning. Använd metoden **Milstolpar** i stället.

## Vanliga steg {#common-steps}

1. Konfigurera en webbsida genom att dra en **videokomponent** från sidosparken och lägga till en uppspelningsbar **video som en resurs** för komponenten

1. [Skapa en Adobe Analytics-konfiguration och ett ramverk](/help/sites-administering/adobeanalytics.md).

   * Exemplen i de efterföljande avsnitten använder namnet **my-sc-configuration** för konfigurationen och **videofunktionen** för ramverket.

1. På ramverkssidan väljer du ett RSID och anger användningen till alla. ([https://localhost:4502/cf#/etc/cloudservices/sitecatalyst/videoconf/videofw.html](https://localhost:4502/cf#/etc/cloudservices/sitecatalyst/videoconf/videofw.html))
1. Dra videokomponenten från kategorin Allmänt i Sidekick till ramverket.
1. Välj en spårningsmetod:

   * [Milstolpar](/help/sites-administering/adobeanalytics.md)
   * [Icke-äldre milstolpar](/help/sites-administering/adobeanalytics.md)
   * [Äldre milstolpar](/help/sites-administering/adobeanalytics.md)
   * [Äldre sekunder](/help/sites-administering/adobeanalytics.md)

1. När du väljer en spårningsmetod ändras listan med CQ-variabler därefter. Använd följande avsnitt för information om hur du konfigurerar komponenten ytterligare och mappar CQ-variablerna med Adobe Analytics-egenskaper.

## Milstolpar {#milestones}

Metoden milstolpar håller reda på den mesta informationen om videon, är mycket anpassningsbar och enkel att konfigurera.

Om du vill använda metoden milstolpar anger du tidsbaserade spårförskjutningar för att definiera milstolparna. När en videouppspelning passerar en milstolpe anropar sidan Adobe Analytics för att spåra händelsen. För varje milstolpe som du definierar skapar komponenten en CQ-variabel som du kan mappa till en Adobe Analytics-egenskap. Namnet på CQ-variablerna har följande format:

```shell
eventdata.events.milestoneXX
```

Suffixet XX är den spårförskjutning som definierar milstolpen. Om du till exempel anger spårförskjutningar på 4, 8, 16, 20 och 28 sekunder genereras följande CQ-variabler:

* `eventdata.events.milestone4`
* `eventdata.events.milestone8`
* `eventdata.events.milestone16`
* `eventdata.events.milestone20`
* `eventdata.events.milestone28`

I följande tabell beskrivs CQ-standardvariablerna som anges för metoden Milestone:

<table>
 <tbody>
  <tr>
   <th>CQ-variabler</th>
   <th>Adobe Analytics-egenskaper</th>
  </tr>
  <tr>
   <td>eventdata.videoName </td>
   <td>Variabler som mappas till detta kommer att innehålla det <strong>användarvänliga</strong> namnet (<strong>Title</strong>) för videon om det anges i DAM. Om detta inte anges skickas videons <strong>filnamn</strong> i stället. Skickas endast en gång, i början av uppspelningen av en video.</td>
  </tr>
  <tr>
   <td>eventdata.videoFileName </td>
   <td>Variabler som mappas till detta innehåller filens namn. Endast skickat tillsammans med eventdata.events.a.media.view </td>
  </tr>
  <tr>
   <td>eventdata.videoFilePath </td>
   <td>Variabler som mappas till detta innehåller filens sökväg på servern. Endast skickat tillsammans med eventdata.events.a.media.view </td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.segmentView </td>
   <td>Skickas varje gång en milstolpe för ett segment passeras </td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.timePlayed</td>
   <td>Skickas varje gång en milstolpe aktiveras, skickas även antalet sekunder som användaren har ägnat åt att titta på det aktuella segmentet tillsammans med den här händelsen. eventX=21<br /> </td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.view </td>
   <td>Skickat när videovyn initieras</td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.complete </td>
   <td>Skickat när videon har spelats upp<br /> </td>
  </tr>
  <tr>
   <td>eventdata.events.milestoneX </td>
   <td>Skickas när den angivna milstolpen skickas, X står för den sekund som milstolpen utlöses vid <br /> </td>
  </tr>
  <tr>
   <td>eventdata.a.contentType </td>
   <td>Skickat på varje milstolpe. Visas som pev3 i Adobe Analytics-anropet, skickas vanligtvis som video <br /> </td>
  </tr>
  <tr>
   <td>eventdata.a.media.name </td>
   <td>Matchar exakt eventdata.videoName </td>
  </tr>
  <tr>
   <td>eventdata.a.media.segment </td>
   <td>Innehåller information om segmentet som har visats, till exempel <code>2:O:4-8</code> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Du kan ange ett **användarvänligt**-namn för en video genom att öppna videon för redigering i DAM och ange det önskade namnet i metadatafältet **Titel** .

1. När du har valt milstolpar som spårningsmetod anger du en kommaseparerad lista med spårningsförskjutningar i sekunder i rutan Spåra förskjutning. Följande värde definierar till exempel milstolpar vid 4, 8, 16, 20 och 28 sekunder efter videostarten:

   ```xml
   4,8,16,20,24
   ```

   Förskjutningsvärdena måste vara heltal som är större än 0. Standardvärdet är `10,25,50,75`.

1. Om du vill mappa CQ-variablerna till Adobe Analytics-egenskaper drar du Adobe Analytics-egenskaperna från ContentFinder bredvid CQ-variabeln för komponenten.

   Mer information om hur du optimerar mappningarna finns i guiden [Mäta video i Adobe Analytics](https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=sv-SE).

1. [Lägg till ramverket](/help/sites-administering/adobeanalytics.md) på sidan.
1. Om du vill testa konfigurationen i **förhandsgranskningsläget** spelar du upp videon och får Adobe Analytics-anrop att utlösa.

De följande exemplen på spårningsdata för Adobe Analytics gäller för spårning av milstolpar med hjälp av spårförskjutningar på 4,8,16,20 och 24, och följande mappningar för CQ-variablerna:

<table>
 <tbody>
  <tr>
   <th>CQ-variabel</th>
   <th>Adobe Analytics, egenskap</th>
  </tr>
  <tr>
   <td>eventdata.videoName </td>
   <td>prop2</td>
  </tr>
  <tr>
   <td>eventdata.videoFileName </td>
   <td>prop3 </td>
  </tr>
  <tr>
   <td>eventdata.videoFilePath </td>
   <td>prop4</td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.segmentView </td>
   <td>event1</td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.timePlayed</td>
   <td>event2<br /> </td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.view </td>
   <td>event3</td>
  </tr>
  <tr>
   <td>eventdata.events.a.media.complete </td>
   <td>event4<br /> </td>
  </tr>
  <tr>
   <td>eventdata.events.milestone4</td>
   <td>event10</td>
  </tr>
  <tr>
   <td>eventdata.events.milestone8</td>
   <td>event11</td>
  </tr>
  <tr>
   <td>eventdata.events.milestone16</td>
   <td>event12</td>
  </tr>
  <tr>
   <td>eventdata.events.milestone20</td>
   <td>event13</td>
  </tr>
  <tr>
   <td>eventdata.events.milestone24</td>
   <td>event14</td>
  </tr>
  <tr>
   <td>eventdata.a.contentType </td>
   <td>eVar3</td>
  </tr>
  <tr>
   <td>eventdata.a.media.name </td>
   <td>eVar1, prop1 </td>
  </tr>
  <tr>
   <td>eventdata.a.media.segment </td>
   <td>eVar2</td>
  </tr>
 </tbody>
</table>

I det här exemplet visas videokomponenten på följande sätt på ramverkssidan:

![video1](assets/video1.png)

>[!NOTE]
>
>Om du vill se anropen till Adobe Analytics använder du ett lämpligt verktyg, som DigitalPulse Debugger eller Fiddler.

Anrop till Adobe Analytics som använder exemplet ska se ut så här när de visas med DigitalPulse Debugger:

![chlimage_1-128](assets/chlimage_1-128.png)

*Detta är det **första anropet**&#x200B;som gjorts till Adobe Analytics och som innehåller följande värden:*

* *prop1 och eVar1 för eventdata.a.media.name,*
* *props2-4, tillsammans med eVar2 och eVar3 som innehåller contentType (video) och segment (1:O:1-4)*
* *event3 som har mappats till eventdata.events.a.media.view.*

![chlimage_1-129](assets/chlimage_1-129.png)

*Detta är det **tredje anropet**&#x200B;som gjorts till Adobe Analytics:*

* *prop1 och eVar1 innehåller a.media.name;*
* *event1 eftersom ett segment har visats*
* *event2 skickad med uppspelad tid = 4*
* *event11 har skickats eftersom eventdata.events.milestone8 har nåtts*
* *prop2 till 4 skickas inte (eftersom eventdata.events.a.media.view inte utlöstes)*

## Icke-äldre milstolpar {#non-legacy-milestones}

Metoden Icke-äldre milstolpar liknar metoden milstolpar, förutom att milstolpar definieras med procentvärden av spårlängden. Följande är gemensamma:

* När en videouppspelning passerar en milstolpe anropar sidan Adobe Analytics för att spåra händelsen.
* Den [statiska uppsättningen CQ-variabler](#cqvars) som har definierats för mappning med Adobe Analytics-egenskaper.
* För varje milstolpe som du definierar skapar komponenten en CQ-variabel som du kan mappa till en Adobe Analytics-egenskap.

Namnet på CQ-variablerna har följande format:

Suffixet XX är den procentandel av spårlängden som definierar milstolpen. Om du till exempel anger procenttal på 10, 25, 50 och 75 genereras följande CQ-variabler:

* `eventdata.events.milestone10`
* `eventdata.events.milestone25`
* `eventdata.events.milestone50`
* `eventdata.events.milestone75`

```shell
eventdata.events.milestoneXX
```

1. När du har valt Icke-äldre milstolpar som spårningsmetod anger du en kommaseparerad lista med procentvärden för spårlängd i rutan Spårförskjutning. Följande standardvärde definierar till exempel milstolpar vid 10, 25, 50 och 75 procent av spårlängden:

   ```xml
   10,25,50,75
   ```

   Förskjutningsvärdena måste vara heltal som är större än 0.

1. Om du vill mappa CQ-variablerna till Adobe Analytics-egenskaper drar du Adobe Analytics-egenskaperna från ContentFinder bredvid CQ-variabeln för komponenten.

   Mer information om hur du optimerar mappningarna finns i guiden [Mäta video i Adobe Analytics](https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=sv-SE).

1. [Lägg till ramverket](/help/sites-administering/adobeanalytics.md) på sidan.
1. Om du vill testa konfigurationen i **förhandsgranskningsläget** spelar du upp videon och får Adobe Analytics-anrop att utlösa.

## Äldre milstolpar {#legacy-milestones}

Den här metoden liknar metoden milstolpar med skillnaden att de milstolpar som anges i fältet *Spårningsförskjutning* är procentvärden i stället för att ange punkter i videon.

>[!NOTE]
>
>I fältet Spårningsförskjutning accepteras bara en kommaseparerad lista som innehåller heltal mellan 1 och 100.

1. Ange spårförskjutning.

   * till exempel 10,50,75,100

   Dessutom är informationen som skickas till Adobe Analytics mindre anpassningsbar. Det finns bara tre variabler för mappning:

<table>
 <tbody>
  <tr>
   <td>eventdata.videoName <br /> </td>
   <td>Variabler som mappas till detta kommer att innehålla det <strong>användarvänliga</strong> namnet (<strong>Title</strong>) för videon om det anges i DAM. Om titeln inte anges skickas videons <strong>filnamn</strong> i stället. Skickas endast en gång, i början av uppspelningen av en video.<br /> </td>
  </tr>
  <tr>
   <td>eventdata.videoFileName </td>
   <td>Variabler som mappas till detta innehåller filens namn. Skickas endast en gång, i början av uppspelningen av en video.</td>
  </tr>
  <tr>
   <td>eventdata.videoFilePath </td>
   <td>Variabeln som mappas till detta kommer att innehålla filens sökväg på servern. Skickas endast en gång, i början av uppspelningen av en video.</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Du kan ange ett **användarvänligt**-namn för en video genom att öppna videon för redigering i DAM och ange det önskade namnet i metadatafältet **Titel** . Du måste också spara ändringarna som gjorts när du är klar.

1. Mappa dessa variabler till steg 1 till 3

   **Resten av den relevanta informationen** i anropet skickas sammanfogat till variabeln **one** med namnet **pev3**.

   **Exempelanrop** till Adobe Analytics med det angivna exemplet ska se ut så här när det visas med DigitalPulse Debugger:

   ![milstolpar1](assets/lmilestones1.png)

   *Variabeln **pev3**&#x200B;som skickades i anropet innehåller följande information:*

   * *Namn* - Namnet på videofilen (*film.avi*)

   * *Längd* - längden på videofilen, i sekunder (*100*)

   * *Spelarnamn* - Den videospelare som används för att spela upp videofilen (*HTML5-video*)

   * *Totalt antal sekunder som spelats upp* - Totalt antal sekunder som videon spelades upp (*25*)

   * *Starttidsstämpel* - Tidsstämpel som identifierar när videouppspelningen startade (*1331035567*)

   * *Spela upp session* - Information om uppspelningssessionen. I det här fältet visas hur användaren interagerade med videon. Detta kan omfatta data som var de började spela upp videon, om de använde videoläget för att gå vidare med videon och var de slutade spela upp videon (*L10E24S58L58 - videon stoppades på sek. 25 i avsnitt L10, som sedan hoppas över till sek. 48*)

## Äldre sekunder {#legacy-seconds}

När du använder metoden** för tidigare sekunder* aktiveras Adobe Analytics-anrop var N:e sekund, där N anges i fältet för spårförskjutning.

1. Ställ in spårförskjutningen till valfritt antal sekunder,

   * till exempel 6

   >[!NOTE]
   >
   >Fältet Spårningsförskjutning accepterar endast heltal som är högre än 0

   Informationen som skickas till Adobe Analytics är mindre anpassningsbar. Det finns bara tre variabler för mappning:

<table>
 <tbody>
  <tr>
   <td>eventdata.videoName <br /> </td>
   <td>Variabler som mappas till detta kommer att innehålla det <strong>användarvänliga</strong> namnet (<strong>Title</strong>) för videon om det anges i DAM. Om titeln inte anges skickas videons <strong>filnamn</strong> i stället. Skickas endast en gång, i början av uppspelningen av en video.<br /> </td>
  </tr>
  <tr>
   <td>eventdata.videoFileName </td>
   <td>Variabeln som mappas till detta kommer att innehålla filens namn. Skickas endast en gång, i början av uppspelningen av en video.</td>
  </tr>
  <tr>
   <td>eventdata.videoFilePath </td>
   <td>Variabeln som mappas till detta kommer att innehålla filens sökväg på servern. Skickas endast en gång, i början av uppspelningen av en video.</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Du kan ange ett **användarvänligt**-namn för en video genom att öppna videon för redigering i DAM och ange det önskade namnet i metadatafältet **Titel** . Du måste också spara ändringarna som gjorts när du är klar.

1. Mappa dessa variabler till prop1, prop2 och prop3

   **Resten av den relevanta informationen** i anropet skickas sammanfogat till variabeln **one** med namnet **pev3**.

   Anrop till Adobe Analytics som använder exemplet ska se ut så här när de visas med DigitalPulse Debugger:

   ![lseconds](assets/lseconds.png)

   *Anropet liknar anropet för äldre milstolpar ovan. Se informationen om föregående3 under [Integrera med Adobe Analytics](/help/sites-administering/adobeanalytics.md).*.

**Referenser som används i den här självstudiekursen:**

[0] [https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=sv-SE](https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=sv-SE)
