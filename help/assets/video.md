---
title: Video i Dynamic Media
description: Lär dig hur du arbetar med video i Dynamic Media, t.ex. de bästa sätten att koda videofilmer, lägga till flera ljud- och bildtextspår i videoklipp samt videominiatyrbilder.
feature: Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 5dc734b3-22e3-4839-bc72-b96fa6dd8bd2
source-git-commit: 6ceb03253f939734478cdc25b468737ceb83faa4
workflow-type: tm+mt
source-wordcount: '10339'
ht-degree: 1%

---

# Video i Dynamic Media {#video}

I det här avsnittet beskrivs hur du arbetar med video i Dynamic Media.

## Snabbstart: Videor {#quick-start-videos}

Följande steg-för-steg-beskrivning av arbetsflödet hjälper dig att komma igång snabbt med adaptiva videouppsättningar i Dynamic Media. Efter varje steg finns det korsreferenser till ämnesrubriker där du kan hitta mer information.

>[!IMPORTANT]
>
>Kontrollera att Adobe Experience Manager-administratören har aktiverat och konfigurerat Dynamic Media Cloud-tjänster i antingen Dynamic Media - Scene7-läge eller hybridläge innan du arbetar med video i Dynamic Media.
>
>* Se [Konfigurera Dynamic Media Cloud-tjänster](/help/assets/config-dms7.md#configuring-dynamic-media-cloud-services) i Konfigurera Dynamic Media - Scene7-läge och [Felsök Dynamic Media - Scene7-läge](/help/assets/troubleshoot-dms7.md).
>
>* Se [Konfigurera Dynamic Media Cloud-tjänster](/help/assets/config-dynamic.md#configuring-dynamic-media-cloud-services) i Konfigurera Dynamic Media - hybrid-läge.
>

1. **Överför dina dynamiska medievideor** genom att göra följande:

   * Skapa en egen videokodningsprofil. Eller så kan du helt enkelt använda den fördefinierade _adaptiva videokodningsprofilen_ som medföljer Dynamic Media.

      * [Skapa en videokodningsprofil](/help/assets/video-profiles.md#creating-a-video-encoding-profile-for-adaptive-streaming).
      * Den maximala utdatakodningsupplösningen är 8 192 × 4 320 eller 4 320 × 8 192.md.
      * Läs mer om [Bästa tillvägagångssätt för videokodning](#best-practices-for-encoding-videos).

   * Koppla videobearbetningsprofilen till en eller flera mappar där du ska överföra dina primära källvideor.

      * [Använd en videoprofil för mappar](/help/assets/video-profiles.md#applying-a-video-profile-to-folders).
      * Läs mer om [Bästa tillvägagångssätt för att ordna dina digitala resurser så att du kan använda bearbetningsprofiler](/help/assets/organize-assets.md).
      * Läs mer om [Ordna digitala resurser](/help/assets/organize-assets.md).

   * Överför dina primära källvideor till mapparna. När du lägger till videofilmer i mappen kodas de enligt den videobearbetningsprofil som du tilldelade mappen.

      * Dynamic Media har stöd för i första hand videor i kort form med en maxlängd på 30 minuter och en minimiupplösning som är större än 25 × 25.
      * Den högsta videoupplösningen som stöds är 16 384 × 16 384.
      * Du kan överföra videofiler som är upp till 15 GB vardera.
      * [Överför dina videor](/help/assets/managing-video-assets.md#upload-and-preview-video-assets).
      * Läs mer om [Indatafilformat som stöds](/help/assets/assets-formats.md#supported-multimedia-formats).

   * Övervaka hur [videokodningen fortskrider](#monitoring-video-encoding-and-youtube-publishing-progress) antingen från resursvyn eller arbetsflödesvyn.

1. **Hantera dina dynamiska medievideor** genom att göra något av följande:

   * Ordna, bläddra bland och söka efter videomaterial

      * [Ordna digitala resurser](/help/assets/organize-assets.md)
Läs mer om [Bästa tillvägagångssätt för att ordna digitala resurser så att de kan använda bearbetningsprofiler](organize-assets.md)

      * [Sök efter videomaterial](search-assets.md#custompredicates) eller [Sök efter resurser](/help/assets/search-assets.md)

   * Förhandsgranska och publicera videomaterial

      * Visa källvideon och de kodade återgivningarna av videon tillsammans med tillhörande miniatyrer:

        [Förhandsgranska videoklipp](managing-video-assets.md#upload-and-preview-video-assets) eller [Förhandsgranska resurser](previewing-assets.md)
        [Visa videoåtergivningar](video-renditions.md)
        [Hantera videorenderingar](manage-assets.md#managing-renditions)

      * [Hantera förinställningar för visningsprogram](managing-viewer-presets.md)
      * [Publicera resurser](publishing-dynamicmedia-assets.md)

   * Arbeta med videometadata

      * Visa egenskaperna för en kodad videoåtergivning, t.ex. bildrutefrekvens, ljud- och videobithastighet samt kodek:

        [Visa egenskaper för videoåtergivning](video-renditions.md)

      * Redigera egenskaperna för video, till exempel titel, beskrivning och taggar, anpassade metadatafält:

        [Redigera videoegenskaper](manage-assets.md#editing-properties)

      * [Hantera metadata för digitala resurser](metadata.md)
      * [Metadata-scheman](metadata-schemas.md)

   * Granska, godkänn och kommentera videoklipp och behåll fullständig versionskontroll

      * [Anteckna videofilmer](managing-video-assets.md#annotate-video-assets) eller [Anteckna resurser](manage-assets.md#annotating)

      * [Skapa en version](manage-assets.md#asset-versioning)
      * [Tillämpa arbetsflöden på resurser](assets-workflow.md) eller se [Starta ett arbetsflöde på en resurs](manage-assets.md#starting-a-workflow-on-an-asset)

      * [Granska mappresurser](bulk-approval.md)
      * [Projekt](../sites-authoring/projects.md)

1. **Publicera dina dynamiska medievideor** genom att göra något av följande:

   * Om du använder Adobe Experience Manager som webbinnehållshanteringssystem kan du lägga till videofilmer direkt på dina webbsidor.

      * [Lägg till videofilmer på dina webbsidor](adding-dynamic-media-assets-to-pages.md).

   * Om du använder ett webbinnehållshanteringssystem från en annan leverantör kan du länka eller bädda in videor på dina webbsidor.

      * Integrera video med URL:

        [Länka URL:er till ditt webbprogram](linking-urls-to-yourwebapplication.md).

      * Integrera video med inbäddad kod på en webbsida:

        [Bädda in videovisningsprogrammet på en webbsida](embed-code.md).

   * [Generera videorapporter](#viewing-video-reports).

   * [Lägg till bildtexter i videon](#adding-captions-to-video).

## Arbeta med video i Dynamic Media {#working-with-video-in-dynamic-media}

Video in Dynamic Media är en totallösning som gör det enkelt att publicera högkvalitativ, adaptiv video för direktuppspelning på flera skärmar, inklusive datorer, iOS, Android™, BlackBerry® och Windows-enheter. En adaptiv videouppsättning grupperar versioner av samma video som är kodade med olika bithastigheter och format som 400 kbit/s, 800 kbit/s och 1 000 kbit/s. Datorns eller mobilenhetens tillgängliga bandbredd identifieras.

På en mobilenhet från iOS identifieras t.ex. en bandbredd som 3G, 4G eller Wi-Fi. Sedan väljs automatiskt rätt kodad video bland de olika videobithastigheterna i den adaptiva videouppsättningen. Videon strömmas till datorer, mobila enheter eller surfplattor.

Dessutom ändras videokvaliteten dynamiskt automatiskt om nätverksförhållandena ändras på datorn eller den mobila enheten. Om en kund går över till helskärmsläge på en stationär dator svarar den adaptiva videouppsättningen med en bättre upplösning, vilket förbättrar kundens tittarupplevelse. Adaptiva videouppsättningar ger optimal uppspelning för kunder som visar Dynamic Media-video på flera skärmar och enheter.

Den logik som en videospelare använder för att avgöra vilken kodad video som ska spelas upp eller väljas under uppspelningen baseras på följande algoritm:

1. Videospelaren läser in det inledande videofragmentet baserat på den bithastighet som ligger närmast värdet som är inställt för&quot;inledande bithastighet&quot; i själva spelaren.
1. Videospelaren växlar baserat på ändringar av bandbreddshastigheten med följande kriterier:

   1. Spelaren väljer den högsta bandbreddsströmmen under eller lika med den beräknade bandbredden.
   1. Spelaren hanterar bara 80 % av den tillgängliga bandbredden. Men om den byter upp sig är det mer försiktigt med bara 70 % för att undvika överskattning och omedelbart gå tillbaka.

Detaljerad teknisk information om algoritmen finns på [https://android.googlesource.com/platform/frameworks/av/+/master/media/libstagefright/httplive/LiveSession.cpp](https://android.googlesource.com/platform/frameworks/av/+/master/media/libstagefright/httplive/LiveSession.cpp)

Följande stöds för hantering av enstaka video och adaptiva videouppsättningar:

* Ladda upp videofilmer i olika format som stöds och koda dem till MP4 H.264 för uppspelning på flera skärmar. Du kan använda fördefinierade adaptiva videoförinställningar, enskilda videokodningsförinställningar eller anpassa din egen kodning för att styra videons kvalitet och storlek.

   * När en adaptiv videouppsättning genereras innehåller den MP4-videor.
   * **Obs!**: Primära videoklipp/källvideoklipp läggs inte till i en adaptiv videouppsättning.

* Videoklipp i alla HTML5-videovisningsprogram.
* Ordna, bläddra bland och sök videoklipp med fullt stöd för metadata för effektiv hantering av videomaterial.
* Leverera adaptiva videouppsättningar till webben, datorer och mobila enheter som iPhone, iPad, Android™, BlackBerry® och Windows Phone.

Adaptiv videoströmning stöds på olika iOS-plattformar. Se [Referenshandbok för dynamiska mediavisare](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/video/c-html5-video-reference#video).

Dynamic Media har stöd för videouppspelning i mobiler för MP4 H.264-video. <!-- LINK IS 404 WITH NO SUITABLE REPLACEMENT You can find BlackBerry&reg; devices that support this video format at the following: [Supported video formats on BlackBerry&reg;](https://support.blackberry.com/kb/articleDetail?ArticleNumber=000005482). -->

Du kan hitta Windows-enheter som stöder det här videoformatet på följande plats: [Media codecs som stöds för Windows Phone 8](https://learn.microsoft.com/en-us/windows/uwp/audio-video-camera/supported-codecs)

* Spela upp videon med Dynamic Media Video Viewer Presets, inklusive följande:

   * Enstaka videovisningsprogram.
   * Visningsprogram för blandade media som kombinerar både video- och bildinnehåll.

* Konfigurera videospelare för att tillgodose era varumärkesbehov.
* Integrera video på webbplatsen, mobilsajten eller mobilapplikationen med en enkel URL eller inbäddningskod.

<!-- See [Dynamic video playback](https://s7d9.scene7.com/s7/uvideo.jsp?asset=GeoRetail/Mop_AVS&config=GeoRetail/Universal_Video1&stageSize=640,480) sample. -->

Se även [Visningsprogram för Experience Manager Assets och Dynamic Media Classic](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers#viewers-aem-assets-dmc) och [Visningsprogram endast för Experience Manager-resurser](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/library/viewers-for-aem-assets-only/c-html5-aem-asset-viewers#viewers-for-aem-assets-only).

## Bästa praxis: Använda videovisningsprogrammet för HTML5 {#best-practice-using-the-html-video-viewer}

Förinställningarna för videovisningsprogrammet HTML5 för Dynamic Media är robusta videospelare. Du kan använda dem för att undvika många vanliga problem som är kopplade till HTML5-videouppspelning. Och även problem med mobila enheter som brist på strömmande bithastighet och begränsad webbläsarräckvidd.

På designsidan av spelaren kan du utforma videospelarens funktioner med standardverktyg för webbutveckling. Du kan till exempel utforma knapparna, kontrollerna och den anpassade bakgrunden för förhandsvisningsbilder med HTML5 och CSS så att du kan nå dina kunder med ett anpassat utseende.

På visningsprogrammets uppspelningssida identifieras webbläsarens videokapacitet automatiskt. Sedan skickas videon via HLS (HTTP Live Streaming) eller DASH (Dynamic Adaptive Streaming over HTTP), som också kallas för strömning med adaptiv bithastighet. Eller, om leveransmetoderna inte finns, används progressiv i HTML5 i stället.

Genom att kombinera följande i en enda spelare:

* Möjlighet att utforma uppspelningskomponenterna med HTML5 och CSS
* Har inbäddad uppspelning
* Använd adaptiv och progressiv strömning beroende på webbläsarens kapacitet

Ni kan nå ut med ert multimediematerial till både dator- och mobilanvändare och få en smidig videoupplevelse.

Se även [Om HTML5-visningsprogram](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/library/viewers-for-aem-assets-only/c-html5-aem-asset-viewers#viewers-for-aem-assets-only).

### Uppspelning av video på stationära datorer och mobila enheter med videovisningsprogrammet för HTML5 {#playback-of-video-on-desktop-computers-and-mobile-devices-using-the-html-video-viewer}

För strömning av anpassningsbara video för datorer och mobilenheter baseras de videor som används för växling av bithastighet på alla MP4-videor i den adaptiva videouppsättningen.

Videouppspelning sker med DASH eller HLS, eller progressiv videouppspelning. I tidigare versioner av Experience Manager, till exempel 6.0, 6.1 och 6.2, strömmades videofilmer via HTTP.

I Experience Manager 6.3 och senare direktuppspelas videor via HTTPS (dvs. DASH eller HLS) eftersom DM-gatewaytjänstens URL alltid använder HTTPS. Det här standardbeteendet påverkar inte kunderna. Videoströmning sker alltid via HTTPS, förutom när webbläsaren inte stöder det. (Se följande tabell). Därför bör

* Om du har en HTTPS-webbplats med HTTPS-videoströmning går det bra att strömma.
* Om du har en HTTP-webbplats med HTTPS-videoströmning går det bra att strömma och det finns inga blandade innehållsproblem i webbläsaren.

DASH är den internationella standarden och HLS är en Apple-standard. Båda används för adaptiv videoströmning. Och båda teknikerna justerar automatiskt uppspelningen baserat på bandbreddskapaciteten i nätverket. Man kan också &quot;söka&quot; till valfri punkt i videon utan att behöva vänta på att resten av videon ska laddas ned.

Progressiv video levereras genom att videon hämtas och lagras lokalt på en användares dator eller mobila enhet.

I följande tabell beskrivs enheten, webbläsaren och uppspelningsmetoden för videoklipp på stationära datorer och mobila enheter med videovisningsprogrammet för Dynamic Media.

<table>
 <tbody>
  <tr>
   <td><strong>Enhet</strong></td>
   <td><strong>Webbläsare</strong></td>
   <td><strong>Videouppspelningsläge</strong></td>
  </tr>
  <tr>
   <td>Skrivbord</td>
   <td>Internet Explorer 9 och 10</td>
   <td>Progressiv hämtning.</td>
  </tr>
  <tr>
   <td>Skrivbord</td>
   <td>Internet Explorer 11+</td>
   <td>I Windows 8 och Windows 10 - Tvinga användning av HTTPS när DASH* eller HLS begärs. Känd begränsning: HTTP på DASH* eller HLS fungerar inte i den här kombinationen av webbläsare och operativsystem <br /> <br /> i Windows 7 - progressiv nedladdning. Använder standardlogik för att välja protokollet HTTP jämfört med HTTPS.</td>
  </tr>
  <tr>
   <td>Skrivbord</td>
   <td>Firefox 23-44</td>
   <td>Progressiv hämtning.</td>
  </tr>
  <tr>
   <td>Skrivbord</td>
   <td>Firefox 45 eller senare</td>
   <td>DASH* eller HLS adaptive birate streaming.</td>
  </tr>
  <tr>
   <td>Skrivbord</td>
   <td>Chrome</td>
   <td>DASH* eller HLS adaptive birate streaming.</td>
  </tr>
  <tr>
   <td>Skrivbord</td>
   <td>Safari (Mac)</td>
   <td>HLS adaptiv bithastighetsströmning.</td>
  </tr>
  <tr>
   <td>Mobil</td>
   <td>Chrome (Android™ 6 eller tidigare)</td>
   <td>Progressiv hämtning.</td>
  </tr>
  <tr>
   <td>Mobil</td>
   <td>Chrome (Android™ 7 eller senare)</td>
   <td>DASH* eller HLS adaptive birate streaming.</td>
  </tr>
  <tr>
   <td>Mobil</td>
   <td>Android™ (webbläsare)</td>
   <td>Progressiv hämtning.</td>
  </tr>
  <tr>
   <td>Mobil</td>
   <td>Safari (iOS)</td>
   <td>HLS adaptiv bithastighetsströmning.</td>
  </tr>
  <tr>
   <td>Mobil</td>
   <td>Chrome (iOS)</td>
   <td>HLS adaptiv bithastighetsströmning.</td>
  </tr>
  <tr>
   <td>Mobil</td>
   <td>BlackBerry®</td>
   <td>DASH* eller HLS adaptive birate streaming./td&gt;
  </tr>
 </tbody>
</table>

## Arkitektur för Dynamic Media Video Solution {#architecture-of-dynamic-media-video-solution}

I följande bild visas det övergripande arbetsflödet för redigering av videoklipp som överförs och kodas via DMG-gatewayen (i Dynamic Media Hybrid-läge) och görs tillgängliga för offentlig användning.

![Arkitektur för Dynamic Media-videolösning.](assets/chlimage_1-427.png)

## Hybrid publiceringsarkitektur för videor {#hybrid-publishing-architecture-for-videos}

![Hybrid publiceringsarkitektur för videor.](assets/chlimage_1-428.png)

## Bästa tillvägagångssätt för att koda videofilmer {#best-practices-for-encoding-videos}

Arbetsflödet **Dynamic Media Encode Video** kodar video om du har aktiverat Dynamic Media och konfigurerat videolmolntjänster. Det här arbetsflödet innehåller information om arbetsflödets processhistorik och fel. Om du har aktiverat Dynamic Media och konfigurerat videomolntjänster börjar arbetsflödet **[!UICONTROL Dynamic Media Encode Video]** automatiskt att gälla när du överför en video. (Om du inte använder Dynamic Media börjar arbetsflödet **[!UICONTROL DAM Update Asset]** gälla.)

<!-- DEAD The following are best-practice tips for encoding source video files.

For advice about video encoding, see [Video Encoding Basics](https://www.adobe.com/go/learn_s7_encoding_en).

* [Streaming 101: The Basics — Codecs, Bandwidth, Data Rate, and Resolution](https://www.adobe.com/go/learn_s7_streaming101_en). -->

### Source videofiler {#source-video-files}

När du kodar en videofil ska du använda en källvideofil med högsta möjliga kvalitet. Undvik att använda tidigare kodade videofiler eftersom dessa filer redan är komprimerade, och ytterligare kodning skapar en video med delkvalitet.

* Dynamic Media har stöd för i första hand videor i kort form med en maxlängd på 30 minuter och en minimiupplösning som är större än 25 × 25.
* Du kan överföra primära källvideofiler som är upp till 15 GB vardera.

I följande tabell beskrivs rekommenderad storlek, proportioner och lägsta bithastighet som källvideofilerna måste ha innan du kodar dem:

| Storlek | Proportioner | Minsta bithastighet |
|--- |--- |--- |
| 1024 × 768 | 4:3 | 4 500 kbit/s för de flesta videofilmer. |
| 1280 × 720 | 16:9 | 3 000 - 6 000 kbit/s, beroende på mängden rörelse i videon. |
| 1920 × 1080 | 16:9 | 6000 - 8 000 kbit/s, beroende på mängden rörelse i videon. |

### Hämta metadata för en fil {#obtaining-a-file-s-metadata}

Du kan hämta metadata för en fil genom att visa dess metadata med ett videoredigeringsverktyg eller med ett program som utformats för att hämta metadata. Nedan följer instruktioner om hur du använder MediaInfo, ett tredjepartsprogram, för att hämta videofilens metadata:

1. Gå till [Hämta MediaInfo](https://mediaarea.net/en/MediaInfo/Download).
1. Välj och hämta installationsprogrammet för den grafiska användargränssnittsversionen och följ installationsanvisningarna.
1. Efter installationen högerklickar du på videofilen (endast Windows) och väljer MediaInfo, eller öppnar MediaInfo och drar videofilen till programmet. Alla metadata som är associerade med videofilen, inklusive bredd, höjd och fps, visas.

### Proportioner {#aspect-ratio}

När du väljer eller skapar en förinställning för videokodning för den primära videofilen måste du se till att förinställningens proportioner matchar den primära videofilens. Proportionerna är proportionerna mellan videons bredd och höjd.

Om du vill bestämma proportionerna för en videofil hämtar du filens metadata och noterar filens bredd och höjd. Se Hämta metadata för en fil ovan. Använd sedan den här formeln för att bestämma proportionerna:

width/height = aspect ratio

I följande tabell beskrivs hur formelresultaten översätts till vanliga alternativ för proportioner:

| Formelresultat | Proportioner |
|--- |--- |
| 1,33 | 4:3 |
| 0,75 | 3:4 |
| 1,78 | 16:9 |
| 0,56 | 9:16 |

En video som till exempel är 1440 bredd × 1080 höjd har proportionerna 1440/1080 eller 1,33. I det här fallet väljer du en förinställning för videokodning med 4:3-proportioner för att koda videofilen.

### Bithastighet {#bitrate}

En bithastighet är den mängd data som kodas för att utgöra en sekund av videouppspelningen. Bithastigheten mäts i kilobit per sekund (kbit/s).

>[!NOTE]
>
>Eftersom förlustgivande komprimering används för alla kodekar är bithastigheten den viktigaste faktorn i videokvaliteten. Ju mer du komprimerar en videofil desto sämre blir kvaliteten. Därför är alla andra egenskaper lika (upplösning, bildrutehastighet och kodek), ju lägre bithastighet, desto lägre kvalitet får den komprimerade filen.

När du väljer en bithastighetskodning kan du välja mellan två typer:

* **[!UICONTROL Constant Bitrate Encoding]** (CBR) - Under CBR-kodning är bithastigheten, eller antalet bitar per sekund, densamma under hela kodningsprocessen. CBR-kodning bevarar den angivna datahastigheten enligt inställningen för hela videon. CBR-kodning optimerar inte heller mediefiler för kvalitet utan sparar på lagringsutrymmet.
Använd CBR om videon innehåller en liknande rörelsenivå i hela videon. CBR används oftast för direktuppspelat videoinnehåll. Se även [Använda egna videokodningsparametrar](/help/assets/video-profiles.md#using-custom-added-video-encoding-parameters).

* **[!UICONTROL Variable Bitrate Encoding]** (VBR) - VBR-kodning justerar datahastigheten nedåt och till den övre gräns som du anger, baserat på de data som krävs av kompressorn. Den här funktionen innebär att under en VBR-kodningsprocess ökar eller minskar bithastigheten för mediefilen dynamiskt beroende på mediefilens behov av bithastighet.
Det tar längre tid att koda VBR men ger det bästa resultatet. Kvaliteten på mediefilen är bättre. VBR används oftast för http-progressiv leverans av videoinnehåll.

När använder du VBR jämfört med CRB?
När du väljer VBR jämfört med CBR rekommenderar vi nästan alltid att du använder VBR för dina mediefiler. VBR ger filer av högre kvalitet med konkurrenskraftiga bithastigheter. När du använder VBR måste du vara säker på att du använder kodning i två omgångar och ställa in den maximala bithastigheten till 1,5 gånger målvideobithastigheten.

När du väljer en förinställning för videokodning bör du komma ihåg målanvändarens anslutningshastighet. Välj en förinställning med en datahastighet som är 80 % av den hastigheten. Om målanvändarens anslutningshastighet till exempel är 1000 kbit/s är den bästa förinställningen en med en videodatahastighet på 800 kbit/s.

I den här tabellen beskrivs datahastigheten för typiska anslutningshastigheter.

| Hastighet (kbit/s) | Anslutningstyp |
|--- |--- |
| 256 | Uppringd anslutning. |
| 800 | Vanlig mobilanslutning. För den här anslutningen anger du en datahastighet mellan 400 och maximalt 800 för 3G-upplevelser som mål. |
| 2000 | Vanlig anslutning till stationär bredbandsuppkoppling. För den här anslutningen anger du en datahastighet i intervallet 800-2000 kbit/s med de flesta mål som är i genomsnitt 1200-1500 kbit/s. |
| 5000 | Vanlig bredbandsanslutning. Kodning i det här övre intervallet rekommenderas inte eftersom videoleverans i den här hastigheten inte är tillgänglig för de flesta konsumenter. |

### Upplösning {#resolution}

**Upplösning** beskriver videofilens höjd och bredd i pixlar. Den mesta källvideon lagras med hög upplösning (till exempel 1920 × 1080). Vid direktuppspelning komprimeras källvideo till en lägre upplösning (640 × 480 eller lägre).

Upplösning och datahastighet är två sammankopplade faktorer som avgör videokvaliteten. Om du vill behålla samma videokvalitet måste datahastigheten vara högre ju fler pixlar en videofil har (ju högre upplösning). Ta till exempel antalet pixlar per bildruta i en 320 × 240-upplösning och en 640 × 480-upplösningsvideofil:

| Upplösning | Pixlar per bildruta |
|--- |--- |
| 320 × 240 | 76 800 |
| 640 × 480 | 307 200 |

Filen 640 × 480 har fyra gånger fler pixlar per bildruta. För att uppnå samma datahastighet för dessa två exempelupplösningar använder du fyra gånger så hög komprimering på 640 × 480-filen, vilket kan minska videons kvalitet. En videodatahastighet på 250 kbit/s ger därför en högkvalitativ bild med upplösningen 320 × 240, men inte med upplösningen 640 × 480.

I allmänhet gäller att ju högre datahastighet du använder, desto bättre utseende på videon och ju högre upplösning du använder, desto högre datahastighet måste du behålla visningskvaliteten (jämfört med lägre upplösningar).

Eftersom upplösning och datahastighet är länkade finns det två alternativ när du kodar video:

* Välj en datahastighet och koda sedan med den högsta upplösningen som ser bra ut med den datahastighet du väljer.
* Välj en upplösning och koda sedan med den datahastighet som krävs för att få en video med hög kvalitet med den upplösning du väljer.

När du väljer (eller skapar) en förinställning för videokodning för den primära källvideofilen använder du den här tabellen för att ange rätt upplösning:

| Upplösning | Höjd (pixlar) | Skärmstorlek |
|--- |--- |--- |
| 240p | 240 | Liten skärm |
| 300p | 300 | Liten skärm för mobila enheter |
| 360p | 360 | Liten skärm |
| 480p | 480 | Medium |
| 720p | 720 | Stor skärm |
| 1080p | 1080 | Stor HD-skärm |

Den högsta videoupplösningen som stöds är 16 384 × 16 384. Den maximala utdatakodningsupplösningen för video är 8 192 × 4 320 eller 4 320 × 8 192.

### Fps (bildrutor per sekund) {#fps-frames-per-second}

I USA och Japan spelas de flesta videor in med 29,97 bildrutor per sekund (fps). I Europa är standarden 25 fps. Film spelas dock vanligtvis in med 24 fps.

Välj en förinställning för videokodning som matchar fps-hastigheten för den primära källvideofilen. Om den primära källvideon till exempel är 25 fps väljer du en kodningsförinställning med 25 fps. Som standard används den primära källvideofilens fps för all anpassad kodning. Därför behöver du inte ange fps-inställningen explicit när du skapar en förinställning för videokodning.

### Videokodningsdimensioner {#video-encoding-dimensions}

För bästa resultat väljer du kodningsdimensioner så att källvideon är en hel multipel av alla dina kodade videor.

Om du vill beräkna förhållandet dividerar du källbredden med den kodade bredden för att få breddförhållandet. Sedan dividerar du källhöjden med kodad höjd för att få höjdförhållandet.

Om förhållandet är ett heltal betyder det att videon är optimalt skalad. Om den resulterande kvoten inte är ett heltal påverkas videokvaliteten genom att kvarvarande pixelartefakter lämnas kvar på skärmen. Effekten märks mest när videon innehåller text.

Anta till exempel att källvideon är 1 920 × 1 080. I följande tabell ger de tre kodade videoklippen de optimala kodningsinställningarna som kan användas.

| Videotyp | Bredd × höjd | Breddförhållande | Höjdförhållande |
|--- |--- |--- |--- |
| Source | 1920 × 1080 | 1 | 1 |
| Kodad | 960 × 540 | 2 | 2 |
| Kodad | 640 × 360 | 3 | 3 |
| Kodad | 480 × 270 | 4 | 4 |

### Kodat videofilformat {#encoded-video-file-format}

Dynamic Media rekommenderar att du använder MP4 H.264-videokodningsförinställningar. Eftersom MP4-filer använder H.264-videokodeken ger den video med hög kvalitet men i en komprimerad filstorlek.

## Visa videorapporter {#viewing-video-reports}

>[!NOTE]
>
>Videorapporter är bara tillgängliga när du kör Dynamic Media - hybrid-läge.

Videorapporter visar flera sammanställda värden under en angiven tid för att hjälpa dig att övervaka att *publicerade* enskilda och sammanställda videor fungerar som förväntat. Följande viktigaste mätdata samlas in för alla publicerade videor på hela webbplatsen:

* Video börjar
* Slutförandefrekvens
* Genomsnittlig tid för video
* Total tid för video
* Videor per besök

En tabell över alla *publicerade* videor visas också, så att du kan spåra de mest visade videoklippen på webbplatsen baserat på den totala videostarten.

När du väljer ett videonamn i listan visas videons rapport om målgruppsinnehållande (bortfall) i form av ett linjediagram. Diagrammet visar antalet vyer för en given tidpunkt under videouppspelning. När du spelar upp videon synkroniseras det lodräta strecket med tidsindikatorn i spelaren. Släppningar i linjediagramdata indikerar var publiken slutar intressera sig.

Om videon kodades utanför Adobe Experience Manager Dynamic Media är inte målgruppsinnehållandediagrammet (drop-off) och uppspelningsprocentdata i tabellen tillgängliga.

Se även [Konfigurera Dynamic Media Cloud-tjänster](/help/assets/config-dynamic.md).

>[!NOTE]
>
>Spårnings- och rapportdata baseras uteslutande på användningen av Dynamic Medias egen videospelare och tillhörande videospelarförinställning. Därför kan du inte spåra och rapportera videoklipp som spelas upp via andra videospelare.

Första gången du anger Videorapporter visas som standard videodata från och med den första i den aktuella månaden och till och med den aktuella månadens datum. Du kan dock åsidosätta standarddatumintervallet genom att ange ett eget datumintervall. Nästa gång du anger Videorapporter används det datumintervall du har angett.

För att videorapporter ska fungera på rätt sätt skapas ett Report Suite-ID automatiskt när Dynamic Media Cloud Services konfigureras. Samtidigt skickas Report Suite-ID:t till publiceringsservern så att det är tillgängligt för funktionen Kopiera URL när du förhandsgranskar resurser. Den här funktionen kräver dock att publiceringsservern redan har konfigurerats. Om publiceringsservern inte är konfigurerad kan du fortfarande publicera för att se videorapporten. Du måste dock gå tillbaka till Dynamic Media Cloud Configuration och välja **[!UICONTROL OK]**.

**Så här visar du videorapporter:**

1. I det övre vänstra hörnet av Experience Manager väljer du Experience Manager logotyp och klickar sedan på **[!UICONTROL Tools]** (hammikon) > **[!UICONTROL Assets]** > **[!UICONTROL Video Reports]** till vänster.
1. Gör något av följande på sidan Videorapporter:

   * I närheten av det övre högra hörnet väljer du ikonen **Uppdatera videorapport** .
Använd bara Uppdatera om rapportens slutdatum är den aktuella dagen. Om du gör det ser du till att du ser videospårningen som har utförts sedan du senast körde rapporten.

   * I närheten av det övre högra hörnet väljer du ikonen **Datumväljaren** .
Ange start- och slutdatumintervallet som du vill ha videodata för och välj sedan **[!UICONTROL Run Report]**.

   I grupprutan Top Metrics (Toppvärden) identifieras olika aggregerade mått för alla *publicerade* videor på webbplatsen.

1. I tabellen som listar de publicerade videoklippen väljer du ett videonamn för att spela upp videon och ser även videons återgivningsrapport.

### Visa videorapporter baserade på ett videovisningsprogram som du har skapat med Dynamic Media HTML5 Viewer SDK {#viewing-video-reports-based-on-a-video-viewer-that-you-created-using-the-scene-hmtl-viewer-sdk}

Om du använder ett visningsprogram som inte är installerat från Dynamic Media, eller om du har skapat en anpassad visningsförinställning baserad på ett videoredigeringsprogram som är klart att användas, krävs inga ytterligare steg för att visa videorapporter. Om du har skapat ett eget videovisningsprogram baserat på SDK-API:t för HTML5 Viewer ska du följa de här stegen för att se till att videovisningsprogrammet skickar spårningshändelser till videorapporter för dynamiska media.

Använd [referenshandboken för Adobe Dynamic Media Viewer](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources) och [HTML5 Viewer SDK ](https://s7d1.scene7.com/s7sdk/3.10/docs/jsdoc/index.html) för att skapa egna videovisningsprogram.

**Så här visar du videorapporter baserade på ett videovisningsprogram som du har skapat med Dynamic Media HTML5 Viewer SDK:**

1. Navigera till alla publicerade videoresurser.
1. I listrutan i det övre vänstra hörnet på resursens sida väljer du **[!UICONTROL Viewers]**.
1. Välj en förinställning för visningsprogrammet och kopiera inbäddningskoden.
1. I inbäddningskoden söker du efter raden med följande:

   `videoViewer.setParam("config2", "<value>");`

   Parametern `config2` aktiverar spårning i HTML5-visningsprogram. Det är också en företagsspecifik förinställning som innehåller konfigurationsinformationen för Videorapportering och för kundspecifika Adobe Analytics-konfigurationer.

   Rätt värde för parametern config2 finns både i funktionen **[!UICONTROL Embed Code]** och i funktionen copy **[!UICONTROL URL]**. I URL:en från kommandot copy **[!UICONTROL URL]** är parametern som ska sökas efter `&config2=<value>`. Värdet är nästan alltid `companypreset`, men i vissa fall kan det också vara `companypreset-1`, `companypreset-2` osv.

1. Lägg till AppMeasurementBridge .jsp på visningsprogramsidan i din anpassade videovisningsprogramkod genom att göra följande:

   * Kontrollera först om du behöver parametern `&preset`.

     Om parametern `config2` är `companypreset` behöver du *inte* `&preset=parameter`.

     Om `config2` är något annat anger du parametern preset till samma som parametern `config2`. Om till exempel `config2=companypreset-2` lägger du till `&param2=companypreset-2` i URL:en AppMeasurmentBridge.jsp.

   * Lägg sedan till skriptet AppMeasurementBridge.jsp:

     `<script language="javascript" type="text/javascript" src="https://s7d1.scene7.com/s7viewers/AppMeasurementBridge.jsp?company=robindallas&preset=companypreset-2"></script>`

1. Skapa komponenten TrackingManager genom att göra följande:

   * När du har anropat `s7sdk.Util.init();` skapar du en TrackingManager-instans för att spåra händelser genom att lägga till följande:

     `var trackingManager = new s7sdk.TrackingManager();`

   * Koppla komponenter till TrackingManager genom att göra följande:

     I händelsehanteraren `s7sdk.Event.SDK_READY` kopplar du komponenten som du vill spåra till TrackingManager.

     Om komponenten till exempel är `videoPlayer` lägger du till

     `trackingManager.attach(videoPlayer);`

     för att bifoga komponenten till trackingManager. Om du vill spåra flera visningsprogram på en sida använder du flera komponenter för spårningshanteraren.

   * Skapa AppMeasurementBridge-objektet genom att lägga till följande:

     ```
     var appMeasurementBridge = new AppMeasurementBridge(); appMeasurementBridge.setVideoPlayer(videoPlayer);
     ```

   * Lägg till följande spårningsfunktion:

     ```
     trackingManager.setCallback(appMeasurementBridge.track, 
      appMeasurementBridge);
     ```

   AppMeasurementBridge-objektet har en inbyggd spårfunktion. Du kan dock ge dig ett eget stöd för flera spårningssystem eller andra funktioner.

<!--    For more information, see *Using the TrackingManager Component* in the *Scene7 HTML5 Viewer SDK User Guide* available for download from [Adobe Developer Connection](https://help.adobe.com/en_US/scene7/using/WSef8d5860223939e2-43dedf7012b792fc1d5-8000.html). -->



## Stöd för flera bildtexter och ljudspår för video i Dynamic Media{#about-msma}

Med funktioner för flera bildtexter och ljudspår i Dynamic Media kan du enkelt lägga till flera undertexter och ljudspår i en primär video. Detta innebär att videoklippen är tillgängliga för en global publik. Du kan anpassa en enda publicerad primär video till en global publik på flera språk och följa riktlinjer för tillgänglighet för olika geografiska regioner. Författare kan också hantera undertexter och ljudspår från en enda flik i användargränssnittet.

![Fliken Bildtexter och ljudspår i Dynamic Media tillsammans med en tabell som visar överförda `.vtt` bildtextfiler och överförda MP3-ljudspårfiler för en video.](assets-dm/msma-subtitle-audiotracks-tab2.png)

Några av användningsområdena för att lägga till flera bildtexter och ljudspår i den primära videon är bland annat följande:

| Typ | Använd skiftläge |
|--- |--- |
| **Bildtexter** | Stöd för flera språk |
|  | Beskrivande text för tillgänglighet |
| **Ljudspår** | Stöd för flera språk |
|  | Kommentarspår |
|  | Beskrivande ljud |

Alla [videoformat som stöds i Dynamic Media](/help/assets/assets-formats.md) och alla videovisningsprogram för Dynamic Media - utom i visningsprogrammet för Dynamic Media *Video_360* - stöds för användning med flera beskrivnings- och ljudspår.

Funktionen för flera bildtexter och ljudspår är tillgänglig för ditt Dynamic Media-konto via en funktion som måste aktiveras (aktiveras) av Adobe kundsupport.

### Lägga till flera bildtexter och ljudspår i videon {#add-msma}

Innan du lägger till flera bildtexter och ljudspår i videon måste du kontrollera att du redan har följande på plats:

* Dynamic Media är konfigurerat i en AEM-miljö.
* En [dynamisk medievideoprofil används i mappen där videoklippen har importerats](/help/assets/video-profiles.md#applying-a-video-profile-to-folders).

Tillagda bildtexter och bildtexter stöds med formaten WebVTT och Adobe `.vtt`. Dessutom stöds tillagda ljudspårsfiler med MP3-format.

>[!IMPORTANT]
>
>Alla videofilmer som du överförde *före* och som aktiverar stöd för flera bildtexter och ljudspår på ditt Dynamic Media-konto, [måste bearbetas på nytt](/help/assets/processing-profiles.md#reprocessing-assets). Det här steget för videoombearbetning är nödvändigt för att de ska kunna använda flera bildtexter och ljudspår. Video-URL:erna fortsätter att fungera och spelas upp som vanligt efter ombearbetningen.

**Så här lägger du till flera bildtexter och ljudspår i videon:**

1. [Överför din primära video till en mapp](/help/assets/managing-video-assets.md#upload-and-preview-video-assets) som redan har tilldelats en videoprofil.
1. Navigera till den överförda videoresursen som du vill lägga till flera bildtexter och ljudspår.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen Egenskaper (en cirkel med &quot;i&quot;).
   ![Markerad videoresurs med bockmarkering över videominiatyrbild och Visa egenskaper markerade i verktygsfältet.](assets-dm/msma-selectedasset-propertiesbutton.png)*Markerad videoresurs i kortvyn.*
1. Välj fliken **[!UICONTROL Captions & Audio Tracks]** på videons egenskapssida.

   >[!TIP]
   >Om du inte ser fliken **[!UICONTROL Captions & Audio Tracks]** betyder det något av två:
   >
   >* Mappen där den valda videon finns har ingen tilldelad videoprofil. I så fall ska du läsa [Använda en videoprofil i mappen](/help/assets/video-profiles.md#applying-video-profiles-to-specific-folders).
   >* Eller så måste Dynamic Media bearbeta videon igen. I så fall ska du läsa [Bearbeta resurser igen i en mapp](/help/assets/processing-profiles.md#reprocessing-assets).
   >
   >När du har slutfört någon av ovanstående åtgärder går du tillbaka till dessa steg.

   ![Fliken Bildtexter och ljudspår på sidan Egenskaper.](assets-dm/msma-audiotracks2.png)*Fliken Bildtexter och ljudspår på videons egenskapssida.*

1. (Valfritt) Gör så här om du vill lägga till en eller flera bildtextfiler i en video:
   * Välj **[!UICONTROL Upload Captions]**.
   * Navigera till och markera en eller flera `.vtt`-filer (videotextspår) och öppna dem.
   * För att bildtexter ska kunna visas i mediespelaren *måste* lägga till nödvändig information (metadata) om *varje* bildtextfil som du har överfört. Välj pennikonen till höger om namnet på en bildtextfil. Ange följande obligatoriska information om filen i dialogrutan **Redigera beskrivning** och välj sedan **[!UICONTROL Save]**. Upprepa den här processen för varje bildtextfil som du överförde:

     | Bildtextmetadata | Beskrivning |
     |--- |--- |
     | Filnamn | Standardfilnamnet härleds från det ursprungliga filnamnet. Filnamnet kan bara ändras under överföring och kan inte ändras senare. Teckenkraven för filnamn är desamma som för AEM Assets.<br>Samma filnamn kan inte användas för ytterligare bildtextfiler och ljudspårsfiler. |
     | Språk | Välj språk för bildtexten. |
     | Typ | Välj den typ av bildtext som du använder.<br>**Underrubrik** - Bildtexten som visas med videon som översätter eller transkriberar dialogrutan.<br>**Bildtext** - Bildtexten innehåller bakgrundsljud, talardifferentiering och annan relevant information. Den innehåller även översättning eller transkription av dialogrutan. Alla dessa aspekter gör innehållet mer tillgängligt för personer som är döva eller hörda. |
     | Etikett | Den text som visas för bildtextens namn i popup-listan **[!UICONTROL Select audio or subtitle]** i mediespelaren. Etiketten är det som kunden ser och som motsvarar ett underrubrik- eller bildtextspår. Exempel: `English (CC)`. |

     Om det behövs kan du ändra eller redigera bildtextens metadata senare. När videon publiceras återspeglas dessa uppgifter på offentliga URL:er i publicerade videor.

1. (Valfritt) Gör följande om du vill lägga till ett eller flera ljudspår i en video:
   * Välj **[!UICONTROL Upload Audio Tracks]**.
   * Navigera till och markera en eller flera .mp3-filer och öppna dem.
   * Om du vill att ljudspår ska visas i popup-listan **[!UICONTROL Select audio or caption]** i mediespelaren *måste* ange nödvändig information. Dessa uppgifter behövs för *varje* ljudspårsfil som du har lagt till. Välj pennikonen till höger om namnet på en ljudspårsfil. Ange följande obligatoriska uppgifter i dialogrutan **Redigera ljudspår** och välj sedan **[!UICONTROL Save]**. Upprepa den här processen för varje ljudspårsfil som du överförde.

     | Metadata för ljudspår | Beskrivning |
     |--- |--- |
     | Filnamn | Standardfilnamnet härleds från det ursprungliga filnamnet. Filnamnet kan bara ändras under överföring och kan inte ändras senare. Teckenkraven för filnamn är desamma som för AEM Assets.<br>Samma filnamn kan inte användas för ytterligare ljudspårfiler eller bildtextfiler. |
     | Språk | Välj språk för ljudspåret. |
     | Typ | Välj vilken typ av ljudspår du använder.<br>**Original** - Ljudspåret som ursprungligen var kopplat till videon och representerades som `[Original]` i etiketten med språket `English` markerat som standard. **[!UICONTROL Label]** och **[!UICONTROL Language]** kan ändras i dialogrutan **[!UICONTROL Edit Audio Track]**, men standardvärdet är de ursprungliga värdena om den primära videon bearbetas om.<br>**Standard** - Ett tilläggsljudspår för ett annat språk än det ursprungliga språket.<br>**Ljudbeskrivning** - Ett ljudspår som även innehåller en beskrivande berättarröst för icke-verbala åtgärder och gester i videon, vilket gör innehållet mer tillgängligt för personer med nedsatt syn. |
     | Etikett | Den text som visas som ljudspårets namn i popup-listan **[!UICONTROL Select audio or subtitle]** i mediespelaren. Etiketten är det kunden ser och motsvarar ett ljudspår. Exempel: `English [Original]`. Etiketten för ljud som är kopplat till en video är som standard `[Original]`. |

     Om det behövs kan du ändra eller redigera metadata för ljudspåret senare. När videon publiceras återspeglas dessa uppgifter på offentliga URL:er i publicerade videor.

1. Välj **[!UICONTROL Save]** i den nedrullningsbara listan **[!UICONTROL Save & Close]** i det övre högra hörnet på sidan. Filerna överförs och metadatabearbetningen börjar, vilket visas i kolumnen **Status** i gränssnittet.

   >[!NOTE]
   >
   >Beroende på inställningarna för cachning för instansen kan metadatabearbetningen ta flera minuter innan den visas i förhandsgranskningen och i publicerade URL:er.

1. (Valfritt) Om du valde **[!UICONTROL Save & Close]** i föregående steg kan du fortfarande visa bearbetningsstatusen för de överförda filerna i stället för att välja **[!UICONTROL Save]**. Se [Visa livscykelstatus för överförda beskrivnings- och ljudspårsfiler](#lifecycle-status-video).
1. (Valfritt) Förhandsgranska videon innan du publicerar för att kontrollera att beskrivningarna och ljudet fungerar som förväntat. Se [Förhandsgranska en video som har flera bildtexter och ljudspår](#preview-video-audio-subtitle)
1. Publicera videon. Se [Publicera resurser](publishing-dynamicmedia-assets.md).

#### Lägga till beskrivnings- och ljudspårsfiler i en video som redan är publicerad

Om du överför ytterligare bildtextfiler eller ljudspårsfiler till en redan publicerad video tilldelas filerna statusen `Processed`. Den här statusen används efter att filerna har förberetts efter överföringen. Då kan du förhandsgranska videon i Dynamic Media för att se eller höra de nyligen överförda filerna.

Efter förhandsgranskningen måste du *publicera* videon igen för att de nya bildtextfilerna eller ljudspårsfilerna ska kunna publiceras. Efter publiceringen blir beskrivningarna eller ljudet tillgängliga med den offentliga Dynamic Media-URL:en.

>[!NOTE]
>
>Baserat på cachelagringsinställningarna för din instans kan metadatauppdateringar ta flera minuter innan de visas i förhandsgranskningen och i publicerade URL:er.

Om du har konfigurerat Dynamic Media för omedelbar publicering utlöses en publicering av videon omedelbart när ytterligare bildtext- eller ljudfiler överförs efter att bildtext- eller ljudfiler har överförts.

>[!CAUTION]
>
>När du överför bildtextfiler eller ljudfiler till en video som antingen är publicerad eller opublicerad, tas filerna bort om du [*bearbetar om*](/help/assets/processing-profiles.md#reprocessing-assets) videon. Endast videons ursprungliga ljud förblir intakt. I så fall måste du ladda upp bildtextfilerna och ljudspårsfilerna till videon igen.

#### Lägga till flera bildtexter i en video som har en befintlig URL med bildtextmodifierare

Dynamic Media har stöd för att lägga till en enda bildtext med video via en URL-modifierare. Se [Lägga till bildtexter i videon](#adding-captions-to-video).

Ändringar av flera bildtexter har företräde framför en bildtext som har lagts till med en URL-modifierare för publicerade videor.

**Så här lägger du till flera bildtexter i en video som har en befintlig URL med bildtextmodifierare:**

1. Överför bildtextfilen som redan har lagts till som modifierare till videon, så att du kan hantera filen explicit.
1. Överför eventuella ytterligare bildtextfiler.
1. Publicera videon som vanligt.
Den befintliga URL:en med bildtextmodifieraren kan nu läsa in flera bildtexter.

### Visa livscykelstatus för överförda beskrivnings- och ljudspårsfiler{#lifecycle-status-video}

Du kan följa livscykelstatusen för alla beskrivnings- eller ljudspårsfiler som överförts till den primära videon. Det kan du göra på fliken **Bildtexter och ljudspår** i **Egenskaper**.

**Så här visar du livscykelstatusen för en video:**

1. Navigera till den videoresurs vars livscykelstatus du vill visa.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen Egenskaper (en cirkel med &quot;i&quot;).
1. Välj fliken **[!UICONTROL Captions & Audio Tracks]** på sidan Egenskaper. Observera status för varje bildtext eller ljudfil i kolumnen Status.

| Status för beskrivning eller ljudspår | Beskrivning |
| --- | --- |
| Bearbetar | När en ny beskrivnings- eller ljudspårsfil läggs till och sparas, försätts den i tillståndet&quot;Bearbetar&quot;. Dynamic Media bearbetar filen genom att bifoga strömningsmanifestet till den primära videon. |
| Behandlad | När bearbetningen är klar visas beskrivnings- eller ljudspårsfilen, eller det ursprungliga ljudspåret som är associerat med den primära videon, i läget Behandlad. Du kan förhandsgranska beskrivnings- och ljudspårsfiler som visas som &quot;Behandlad&quot; *innan* du publicerar videon live. |
| Publicerad | Ett publicerat läge representerar ett läge som liknar publicerat för en primär video. Assets publiceras när den primära videon publiceras och är tillgänglig på den offentliga Dynamic Media-URL:en. |
| Misslyckades | Ett &quot;Misslyckat&quot;-läge betyder att bearbetningen av en beskrivnings- eller ljudspårsfil inte slutfördes. Ta bort beskrivnings- eller ljudspårsfilen och överför igen. |
| Opublicerad | När en publicerad primär video avpubliceras explicit avpubliceras även eventuella beskrivnings- eller ljudspårsfiler som du har lagt till i videon. |

![Statuskolumnen är markerad för fälten Bildtexter och Ljudspår.](assets-dm/msma-lifecycle-status2.png)*Livscykelstatus för varje överförd beskrivnings- och ljudspårfil.*

### Ange standardljud för en video som har flera ljudspår

Som standard anges videons ursprungliga ljud som standardljud som ska spelas upp.

Alla överförda ljudspårsfiler kan dock anges som standardljud som spelas upp när en video har lästs in i visningsprogrammet. I egenskapsgränssnittet, under fliken **Bildtexter och ljudspår**, används etiketten `Default` till höger om ljudspårsfilen för videouppspelning.

>[!NOTE]
>
>Uppspelningen av standardljud kan också bero på vad som är inställt i följande webbläsare:
>
>* Chrome - Det standardljud som ställs in i videon spelas upp.
>* Safari - Om standardspråket är inställt i Safari spelas ljudet upp med det angivna standardspråket, om tillgängligt med videons manifest. I annat fall spelas det standardljud som är inställt som en del av en videos egenskaper upp.

**Så här anger du standardljud för en video som har flera ljudspår:**

1. Navigera till den videoresurs vars standardljudspår du vill ställa in.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen Egenskaper (en cirkel med &quot;i&quot;).
1. Välj fliken **[!UICONTROL Captions & Audio Tracks]** på sidan Egenskaper.
1. Under rubriken **Ljudspår** väljer du den ljudspårsfil som du vill ange som videons standard.
1. Välj **[!UICONTROL Set as default]**.
Välj **[!UICONTROL Replace]** i dialogrutan **Ange som standard**.

   ![Rubriken Ljudspår med namnet på den valda ljudspårsfilen och markerad&quot;Ange som standard&quot;-knapp.](assets-dm/msma-defaultaudiotrack2.png)*Anger standardljudspåret för en video.*

1. Välj **[!UICONTROL Save & Close]** i det övre högra hörnet.
1. Publicera videon. Se [Publicera resurser](publishing-dynamicmedia-assets.md).

### Förhandsgranska en video med flera bildtexter och ljudspår{#preview-video-audio-subtitle}

När bildtextfiler och ljudspårfiler har överförts till en video och bearbetats kan du använda videovisningsprogrammet för dynamiska media (eller andra visningsprogramtyper om du vill) för att förhandsgranska alla olika spår. Genom att förhandsgranska kan du se vad videon ser ut och låter som för kunderna och se till att den beter sig som förväntat.

När du är nöjd med videon kan du [publicera den](publishing-dynamicmedia-assets.md) på något av följande sätt.

Se [Bädda in video- eller bildvisningsprogrammet på en webbsida](/help/assets/embed-code.md).
Se [Länka URL:er till ditt webbprogram](/help/assets/linking-urls-to-yourwebapplication.md). Den URL-baserade länkningsmetoden är inte möjlig om det interaktiva innehållet har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.
Se [Lägg till Assets för dynamiska media på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md).

>[!NOTE]
>
>På standardfliken Experience Manager Preview visas inte flera beskrivnings- och ljudspår. Orsaken är att dessa spår är kopplade till Dynamic Media och bara kan ses med förhandsvisning i Dynamic Media Viewer.

**Så här förhandsgranskar du en video som har flera bildtexter och ljudspår:**

1. I **[!UICONTROL Assets]** navigerar du till en befintlig video som du har lagt till flera bildtexter och ljudspår.
1. Klicka på videoresursen så att du kan öppna den i förhandsgranskningsläge.
1. Markera listrutan på förhandsgranskningssidan, nära det övre vänstra hörnet på sidan, och välj sedan **[!UICONTROL Viewers]**.

   ![Listruta med alternativet Visare.](assets-dm/msma-selectviewers.png)

1. Välj ett visningsprogram som du vill använda för videoförhandsvisningen i listan Visare. I följande skärmbild visas det **[!UICONTROL Video]**-visningsprogram som väljs.

   ![Val av videovisningsprogram i listrutan Visare.](assets-dm/msma-dmviewerselected.png)

1. I närheten av det nedre högra hörnet, till vänster om volymikonen, väljer du ikonen för talbubblan och sedan det ljud eller den bildtext som du vill höra eller se eller båda. Om du vill kan du under Bildtexter välja **[!UICONTROL Off]** så att bildtexter inte visas.

   ![Popup-listan Ljud och beskrivningar i videoredigeraren.](assets-dm/msma-selectaudiosubtitle.png)*Simulering av en användare som väljer ljud och bildtext för videouppspelning.*

1. Välj videoklippets **[!UICONTROL Play]**-knapp för att påbörja uppspelningen.
Observera knapparna **[!UICONTROL URL]** och **[!UICONTROL Embed]** i det nedre vänstra hörnet. Använd de här knapparna för att [länka videons URL till ditt webbprogram](/help/assets/linking-urls-to-yourwebapplication.md) eller för att [bädda in videon på en webbsida](/help/assets/embed-code.md).
1. Välj **[!UICONTROL Close]** i det övre högra hörnet på förhandsgranskningssidan.

### Ta bort beskrivnings- eller ljudspårsfiler från en video

Du kan ta bort beskrivnings- eller ljudspårfiler från en video. Borttagning av publicerade bildtexter eller ljudspårsfiler återspeglas automatiskt i videons publicerade URL.

Det går inte att ta bort det ursprungliga ljudspåret som har extraherats från en primär video.

**Så här tar du bort beskrivnings- eller ljudspårsfiler från en video:**

1. Navigera till den videoresurs vars standardljudspår du vill ställa in.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen Egenskaper (en cirkel med &quot;i&quot;).
1. Välj fliken **[!UICONTROL Captions & Audio Tracks]** på sidan Egenskaper.
1. Gör något av följande:

   * Bildtexter - Under rubriken **Bildtexter** markerar du en eller flera bildtextfiler som du vill ta bort från videon och väljer sedan **[!UICONTROL Delete]**.
   * Ljudspår - Under rubriken **Ljudspår** markerar du en eller flera ljudspårsfiler som du vill ta bort från videon och väljer sedan **[!UICONTROL Delete]**.

1. Välj **[!UICONTROL OK]** i dialogrutan Ta bort.
1. Publicera videon.

### Hämta beskrivnings- eller ljudspårsfiler som har överförts till en video

Du kan hämta en eller flera beskrivnings- eller ljudspårsfiler som du har överfört för användning med en video. Du kan antingen hämta alla markerade filer som en ZIP-fil eller skapa en separat hämtningsmapp för varje fil.

Det går inte att hämta det ursprungliga ljudspåret som har extraherats från en primär fil.

**Så här hämtar du beskrivnings- eller ljudspårsfiler från en video:**

1. Navigera till den videoresurs vars standardljudspår du vill ställa in.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen Egenskaper (en cirkel med &quot;i&quot;).
1. Välj fliken **[!UICONTROL Captions & Audio Tracks]** på sidan Egenskaper.
1. Gör något av följande:

   * Bildtexter - Under rubriken **Bildtexter** väljer du en eller flera bildtextfiler som du vill hämta från videon och sedan **[!UICONTROL Download]**.
   * Ljudspår - Under rubriken **Ljudspår** markerar du en eller flera ljudspårsfiler som du vill hämta från videon och väljer sedan **[!UICONTROL Download]**.

1. Ange följande alternativ i dialogrutan Hämta:

   | Alternativ | Beskrivning |
   |--- |--- |
   | Spara som | Använd standardfilnamnet som anges i textfältet Spara som eller ange ett eget namn. |
   | Skapa en separat mapp för varje resurs | Skapa en mapp för varje bildtextfil eller ljudspårsfil som du valde för hämtning. |
   | E-post | Använd ditt standardprogram för e-post för att skicka ZIP-filen till en angiven e-postadress. |
   | Assets | Anger antalet filer som du hämtar och den sammanlagda storleken för alla markerade filer. Om du avmarkerar det här alternativet tonas knappen **[!UICONTROL Download]** ned (inaktiveras), vilket förhindrar att du hämtar någon fil. |

1. Välj **[!UICONTROL Download]**.
1. Publicera videon. Se [Publicera resurser](publishing-dynamicmedia-assets.md).






## Lägga till undertexter i en video {#adding-captions-to-video}

Du kan utöka räckvidden för dina videor till globala marknader genom att lägga till undertexter till enskilda videor eller till adaptiva videouppsättningar. Genom att lägga till undertextning slipper du att duplicera ljudet eller att du behöver använda inbyggda högtalare för att spela in ljudet igen för varje språk. Videon spelas upp på det språk den spelades in på. Undertexter på främmande språk visas så att personer på olika språk fortfarande kan förstå ljuddelen.

Undertexter ger också bättre tillgänglighet för människor som är döva eller hörselskadade.

>[!NOTE]
>
>Den videospelare som du använder måste ha stöd för visning av bildtexter.

Se även [Hjälpmedel i dynamiska media](/help/assets/accessibility-dm.md).

Dynamic Media konverterar bildtextfiler till JSON-format (JavaScript Object Notation). Den här konverteringen innebär att du kan bädda in JSON-texten på en webbsida som en dold men fullständig utskrift av videon. Sökmotorerna kan sedan crawla och indexera innehållet så att videoklippen blir lättare att hitta och ge kunderna ytterligare information om videoinnehållet.

Mer information om hur du använder JSON-funktionen i en URL finns i [Servera statiskt innehåll (inte bildinnehåll)](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/c-serving-static-nonimage-contents#image-serving-api).

**Så här lägger du till undertexter i en video:**

1. Använd ett program eller en tjänst från tredje part för att skapa videobeskrivningsfilen.

   Kontrollera att filen du skapar följer standarden WebVTT (Web Video Text Tracks). Bildtextens filnamnstillägg är `.vtt`. Du kan läsa mer om bildtextstandarden WebVTT.

   Se [WebVTT: Textspår för webbvideo ](https://w3c.github.io/webvtt/).

   Det finns många webbplatser som innehåller både kostnadsfria och premiumverktyg och tjänster som du kan använda för att skapa WebVTT-bildtexter/bildtextfiler utanför Dynamic Media. <!-- THE FOLLOWING LINK IS NO LONGER LIVE. CHECKED DECEMBER 13, 2023 For example, to create a simple video caption file with no styling, you can use the following free online caption authoring and editing tool: -->

   <!--[WebVTT Caption Maker](https://testdrive-archive.azurewebsites.net/Graphics/CaptionMaker/Default.html)

   For best results, use the tool in Internet Explorer 9 or above, Google Chrome, or Safari.

   In the tool, in the **[!UICONTROL Enter URL of video file]** field, paste the copied URL of your video file and then click **[!UICONTROL Load]**. See [Obtain a URL for an Asset](/help/assets/linking-urls-to-yourwebapplication.md#obtaining-a-url-for-an-asset) to get the URL to the video file itself which you can then paste into the **[!UICONTROL Enter URL of video file field]**. Internet Explorer, Chrome, or Safari can then natively play back the video. -->

   Följ instruktionerna på skärmen för att skapa och spara WebVTT-filen. När du är klar kopierar du bildtextfilens innehåll och klistrar in det i en vanlig textredigerare och sparar det med filnamnstillägget `.vtt`.

   >[!NOTE]
   >
   >Om du vill ha globalt stöd för videobeskrivningar på flera språk kräver WebVTT-standarden att du skapar separata `.vtt`-filer och anropar varje språk som du vill ha stöd för.

   Vanligtvis vill du ge bildtexten `.vtt` samma namn som videofilen och lägga till den med språkinställningen -EN, -FR eller -DE. Genom att göra det kan det hjälpa dig att automatisera genereringen av video-URL:er med ditt befintliga system för hantering av webbinnehåll.

1. Överför WebVTT-bildtextfilen till DAM i Experience Manager.
1. Navigera till den *publicerade*-videoresurs som du vill associera med bildtextfilen som du överförde.

   Kom ihåg att URL:er endast går att kopiera *efter* att du har *publicerat* resurserna.

   Se [Publicera resurser](/help/assets/publishing-dynamicmedia-assets.md).

1. Gör något av följande:

   * Klicka på **[!UICONTROL URL]** om du vill visa en popup-video. I dialogrutan URL-adress markerar och kopierar du URL-adressen till Urklipp och sedan förbi URL-adressen till en enkel textredigerare. Lägg till den kopierade URL:en för videon med följande syntax:

     `&caption=<server_path>/is/content/<path_to_caption.vtt_file,1>`

     Observera `,1` i slutet av bildtextssökvägen. Omedelbart efter filnamnstillägget `.vtt` i sökvägen kan du aktivera (aktivera) eller inaktivera (inaktivera) den stängda bildtextsknappen i videospelarfältet genom att ställa in på `,1` eller `,0`.

   * Välj **[!UICONTROL Embed Code]** om du vill ha en inbäddad videovisningsfunktion. I dialogrutan Bädda in kod markerar och kopierar du den inbäddade koden till Urklipp och klistrar sedan in koden i en enkel textredigerare. Lägg till den kopierade inbäddningskoden med följande syntax:

     `videoViewer.setParam("caption","<path_to_caption.vtt_file,1>");`

     Observera `,1` i slutet av bildtextssökvägen. Omedelbart efter filnamnstillägget `.vtt` i sökvägen kan du aktivera (aktivera) eller inaktivera (inaktivera) den stängda bildtextsknappen i videospelarfältet genom att ställa in på `,1` eller `,0`.

## Lägga till kapitelmarkörer i video {#adding-chapter-markers-to-video}

Du kan göra dina videoklipp i långa format enklare att titta på och navigera genom att lägga till kapitelmarkörer i enstaka videor eller i adaptiva videouppsättningar. När en användare spelar upp videon kan han/hon klicka på kapitelmarkörerna på tidslinjen (kallas även videobandskrubbarna) för att enkelt navigera till sin intressanta punkt. Eller så kan de direkt gå över till nytt innehåll, demonstrationer och självstudiekurser.

>[!NOTE]
>
>Den videospelare som används måste ha stöd för kapitelmarkörer. Dynamiska mediespelare har stöd för kapitelmarkörer, men det är inte säkert att tredjepartsvideospelare används.

Om du vill kan du skapa och märka ut ett eget anpassat visningsprogram med kapitel i stället för att använda en förinställning för visningsprogrammet för video. Instruktioner om hur du skapar ett eget HTML5-visningsprogram med kapitelnavigering finns i SDK-API:t för visningsprogrammet för Adobe HTML5, under klasserna `s7sdk.video.VideoPlayer` och `s7sdk.video.VideoScrubber` under rubriken Anpassa beteendet med modifierare. Se dokumentationen för [HTML5 Viewer SDK API](https://s7d1.scene7.com/s7sdk/3.10/docs/jsdoc/index.html).

<!-- If desired, you can create and brand your own custom video viewer with chapters instead of using a video viewer preset. For instructions on creating your own HTML5 viewer with chapter navigation, in the Adobe Scene7 Viewer SDK for HTML5 guide, reference the heading "Customizing Behavior Using Modifiers" under the classes `s7sdk.video.VideoPlayer` and `s7sdk.video.VideoScrubber`. The Adobe Scene7 Viewer SDK is available as a download from [Adobe Developer Connection](https://help.adobe.com/en_US/scene7/using/WSef8d5860223939e2-43dedf7012b792fc1d5-8000.html). -->

Du skapar en kapitellista för videon på ungefär samma sätt som du skapar bildtexter. Det innebär att du skapar en WebVTT-fil. Observera dock att den här filen måste vara separat från alla WebVTT-beskrivningsfiler som du också använder. Du kan inte kombinera bildtexter och kapitel i en WebVTT-fil.

Du kan använda följande exempel som exempel på det format du använder för att skapa en WebVTT-fil med kapitelnavigering:

### WebVTT-fil med videokapitelnavigering {#webvtt-file-with-video-chapter-navigation}

```xml
WEBVTT
Chapter 1
00:00.000 --> 01:04.364
The bicycle store behind it all.
Chapter 2
01:04.364 --> 02:00.944
Creative Cloud.
Chapter 3
02:00.944 --> 03:02.937
Ease of management for a working solution.
Chapter 4
03:02.937 --> 03:35.000
Cost-efficient access to rapidly evolving technology.
```

I exemplet ovan är `Chapter 1` referensidentifieraren och valfri. Referenstiden på `00:00:000 --> 01:04:364` anger kapitlets start- och sluttid i `00:00:000`-format. De sista tre siffrorna är millisekunder och kan lämnas som `000` om det behövs. Kapiteltiteln för `The bicycle store behind it all` är den faktiska beskrivningen av kapitlets innehåll. Referensidentifieraren, startreferenstiden och kapiteltiteln visas alla i en videospelares popup när en användare håller muspekaren över en visuell referenspunkt i videons tidslinje.

Eftersom du använder ett videovisningsprogram för HTML5 bör du kontrollera att den kapitelfil du skapar följer standarden WebVTT (Web Video Text Tracks). Kapitelfiltillägget är `.vtt`. Du kan läsa mer om bildtextstandarden WebVTT.

Se [WebVTT: Textspår för webbvideo ](https://w3c.github.io/webvtt/)

**Så här lägger du till kapitelnavigering:**

1. Spara filen `.vtt` i UTF8-kodning så att du slipper problem med teckenåtergivning i kapiteltiteltexten.

   Vanligtvis vill du ge kapitelfilen `.vtt` samma namn som videofilen och bifoga den med kapitel. Genom att göra det kan det hjälpa dig att automatisera genereringen av video-URL:er med ditt befintliga system för hantering av webbinnehåll.
1. Överför din WebVTT-kapitelfil till Experience Manager.

   Se [Överför Assets](/help/assets/manage-assets.md#uploading-assets).

1. Gör något av följande:

   <table>
     <tbody>
      <tr>
       <td>För en popup-video som visar</td>
       <td>
       <ol>
       <li>Navigera till den <i>publicerade </i>videoresurs som du vill associera med den överförda kapitelfilen. Kom ihåg att URL:er endast går att kopiera <i>efter</i> att du har <i>publicerat</i> resurserna. Se <a href="/help/assets/publishing-dynamicmedia-assets.md">Publicera Assets.</a></li>
       <li>Klicka på <strong>Visare</strong> i listrutan.</li>
       <li>Klicka på förinställningsnamnet för videovisningsprogrammet i den vänstra listen. En förhandsgranskning av videon öppnas på en separat sida.</li>
       <li>Klicka på <strong>URL</strong> längst ned i den vänstra listen.</li>
       <li>I dialogrutan URL-adress markerar och kopierar du URL-adressen till Urklipp och sedan förbi URL-adressen till en enkel textredigerare.</li>
       <li>Lägg till den kopierade URL:en för videon med följande syntax så att du kan associera den med den kopierade URL:en till din kapitelfil:<br /> <br /> <code>&navigation=<<i>full_copied_URL_path_to_chapter_file</i>.vtt></code><br /> </li>
       </ol> </td>
      </tr>
      <tr>
       <td>Om du vill visa en inbäddad video kan du <br /> </td>
       <td>
       <ol>
       <li>Navigera till den <i>publicerade </i>videoresurs som du vill associera med den överförda kapitelfilen. Kom ihåg att URL:er endast går att kopiera <i>efter</i> att du har <i>publicerat</i> resurserna. Se <a href="/help/assets/publishing-dynamicmedia-assets.md">Publicera Assets.</a></li>
       <li>Klicka på <strong>Visare</strong> i listrutan.</li>
       <li>Klicka på förinställningsnamnet för videovisningsprogrammet i den vänstra listen. En förhandsgranskning av videon öppnas på en separat sida.</li>
       <li>Klicka på <strong>Bädda in</strong> längst ned i den vänstra listen.</li>
       <li>I dialogrutan Bädda in kod markerar och kopierar du hela koden till Urklipp och klistrar sedan in den i en enkel textredigerare.</li>
       <li>Lägg till videofilens inbäddningskod med följande syntax så att du kan koppla den till den kopierade URL:en till din kapitelfil:<br /> <br /> <code>videoViewer.setParam("navigation","&lt;<i>full_copied_URL_path_to_chapter_file</i>.vtt&gt;"</code></li>
       </ol> </td>
      </tr>
     </tbody>
   </table>

## Om videominiatyrer i Dynamic Media - Scene7-läge {#about-video-thumbnails-in-dynamic-media-scene-mode}

En videominiatyr är en version med reducerad storlek av en videobildruta eller en bildresurs som representerar videon för kunden. Miniatyrbilden uppmuntrar kunden att välja videon.

Alla videofilmer i Experience Manager måste ha en tillhörande miniatyrbild, och om du vill ta bort en miniatyrbild måste du ersätta den. Som standard används den första bildrutan som miniatyrbild när du överför en video till Experience Manager. Du kan dock anpassa miniatyrbilden för exempelvis varumärke eller visuell sökning. När du anpassar en videominiatyr kan du spela upp videon och pausa den bildruta som du vill använda. Du kan också välja en bildresurs som du redan har överfört och *publicerat* i din Digital Asset Manager.

En anpassad videominiatyrbild som du väljer från en video extraheras inte och sparas i DAM som en separat och distinkt resurs. En anpassad videominiatyr som du väljer från en befintlig bildresurs sparas dock i JCR-filen. Sökvägen för den valda resursen lagras under videoresursens nod som i följande exempelsökväg:

`/content/dam/*<folder_name*>/<*video_name*>/jcr:content/manualThumbnail`

Möjligheten att anpassa en videominiatyr är endast tillgänglig efter att du har tillämpat en videoprofil på den mapp där videon finns.

Se även [Om videominiatyrer i Dynamic Media - hybrid-läge](#about-video-thumbnails-in-dynamic-media-hybrid-mode).

### Lägga till en anpassad videominiatyr {#adding-a-custom-video-thumbnail}

Dessa steg gäller endast för Dynamic Media som körs i läget &quot;DynamicMedia_Scene7&quot;.

**Så här lägger du till en anpassad videominiatyr:**

1. Kontrollera att du redan har gjort följande:

   * Skapade en mapp för dina videoresurser.
   * [Använd en videoprofil för mappen](/help/assets/video-profiles.md#applying-a-video-profile-to-folders).

   * [Dina videofilmer har överförts till mappen](/help/assets/managing-video-assets.md#upload-and-preview-video-assets).

1. Navigera till en överförd videoresurs vars miniatyrbild du vill ändra.
1. I resursurvalsläget, antingen från **[!UICONTROL List View]** eller **[!UICONTROL Card View]**, väljer du videoresursen.
1. I verktygsfältet väljer du ikonen **[!UICONTROL Properties]** (en cirkel med&quot;i&quot;).
1. Välj **[!UICONTROL Change Thumbnail]** på videons egenskapssida.
1. Gör något av följande på sidan Ändra miniatyrbild:

   * Så här använder du en bildruta från videon som ny miniatyrbild:

      * Välj **[!UICONTROL Select Frame from video]** i verktygsfältet.
      * Välj uppspelningsknappen och sedan pausknappen för bildrutan som du vill spela in som videons nya miniatyrbild.

   * Så här använder du en bildresurs som ny miniatyrbild:

      * Välj **[!UICONTROL Select Thumbnail from Assets]** i verktygsfältet.
      * Välj **[!UICONTROL Select Thumbnail]**.
      * Navigera till en tidigare överförd och publicerad bildresurs som du vill använda. Storleken på resursen ändras automatiskt så att den fungerar som en miniatyrbild för videon.
      * Markera bildresursen och välj sedan **[!UICONTROL Select]**.

1. Välj **[!UICONTROL Save Change]** på sidan Ändra miniatyrbild.
1. Välj **[!UICONTROL Save & Close]** i det övre högra hörnet på videons egenskapssida.

## Om videominiatyrer i Dynamic Media - hybrid-läge {#about-video-thumbnails-in-dynamic-media-hybrid-mode}

Du kan välja mellan en av tio miniatyrbilder som har genererats automatiskt av Dynamic Media och lägga till dem i videon. Videospelaren visar den valda miniatyrbilden när en videoresurs används med komponenten Dynamic Media i redigeringsmiljön i Experience Manager Sites, Experience Manager Mobile eller Experience Manager Screens. Miniatyrbilden fungerar som en statisk bild som bäst motsvarar innehållet i hela videon och uppmuntrar dessutom användarna att klicka på knappen Spela upp.

Baserat på den totala tiden för videon hämtar Dynamic Media tio (standard) miniatyrbilder. Systemet hämtar bilder med följande videointervall:

* 1 %
* 11 %
* 21 %
* 31 %
* 41 %
* 51 %
* 61 %
* 71 %
* 81 %
* 91 %

De tio miniatyrbilderna finns kvar, vilket innebär att om du väljer en annan miniatyrbild senare behöver du inte återskapa serien. Du förhandsgranskar de tio miniatyrbilderna och väljer sedan den som du vill använda med videon. Om du vill ändra till standardinställningen kan du använda CRXDE Lite för att konfigurera det tidsintervall som miniatyrbilder genereras. Om du till exempel bara vill generera en serie med fyra miniatyrbilder med jämna mellanrum från videon kan du konfigurera intervalltiden till 24 %, 49 %, 74 % och 99 %.

Helst kan du lägga till en videominiatyr när som helst efter att du har överfört videon, men innan du publicerar videon på webbplatsen.

Om du vill kan du välja att överföra en anpassad miniatyrbild för videon i stället för att använda en miniatyrbild som genererats av Dynamic Media. Du kan till exempel skapa en anpassad miniatyrbild med videons titel, en iögonfallande öppningsbild eller en viss bild som hämtats från videon. Den anpassade videominiatyrbilden som du överför måste ha en maximal upplösning på 1 280 × 720 pixlar (minsta bredd på 640 pixlar) och inte vara större än 2 MB.

Se även [Om videominiatyrer i Dynamic Media - Scene7-läge](/help/assets/video.md#about-video-thumbnails-in-dynamic-media-scene-mode).

### Lägga till en videominiatyr {#adding-a-video-thumbnail}

De här stegen gäller endast för Dynamic Media som körs i hybridläge.

**Så här lägger du till en videominiatyr:**

1. Navigera till en överförd videoresurs som du vill lägga till en videominiatyr.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen **[!UICONTROL View Properties]** (en cirkel med&quot;i&quot;).
1. Välj **[!UICONTROL Change Thumbnail]** på videons egenskapssida.
1. Välj **[!UICONTROL Select Frame]** i verktygsfältet på sidan Ändra miniatyrbild.

   Dynamic Media genererar en serie miniatyrbilder från videon baserat på det standardtidsintervall eller tidsintervall som du har anpassat.

1. Förhandsgranska de genererade miniatyrbilderna och välj sedan den som du vill lägga till i videon.
1. Välj **[!UICONTROL Save Change]**.

   Videons miniatyrbild uppdateras till att använda den miniatyrbild du valde. Om du senare bestämmer dig för att ändra miniatyrbilden kan du gå tillbaka till sidan **[!UICONTROL Change Thumbnail]** och välja en ny.

   Om du anger nya standardtidsintervall eller överför en ny video som ska ersätta den befintliga, måste du se till att Dynamic Media återskapar miniatyrbilderna.

   Se [Konfigurera standardtidsintervallet som videominiatyrbilder genereras](#configuring-the-default-time-interval-that-video-thumbnails-are-generated).

#### Konfigurera det standardtidsintervall som videominiatyrbilder genereras {#configuring-the-default-time-interval-that-video-thumbnails-are-generated}

När du konfigurerar och sparar det nya standardtidsintervallet gäller ändringen automatiskt endast videoklipp som du överför i framtiden. Den nya standardinställningen tillämpas inte automatiskt på videoklipp som du tidigare överfört. För befintliga videofilmer måste du återskapa miniatyrbilderna.

Se [Lägga till en videominiatyr](#adding-a-video-thumbnail).

**Så här konfigurerar du det standardtidsintervall som videominiatyrbilder genereras:**

1. I Experience Manager väljer du **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL CRXDE Lite]**.

1. Gå till `o etc/dam/imageserver/configuration/jcr:content/settings.` på CRXDE Lite-sidan i katalogpanelen till vänster

   Om katalogpanelen inte visas väljer du ikonen >> till vänster om fliken Hem.

1. Dubbelmarkera `thumbnailtime` på den nedre högra panelen på fliken Egenskaper.
1. I dialogrutan **[!UICONTROL Edit thumbnailtime]** använder du textfälten för att ange intervallvärden som procenttal.

   * Markera plustecknet (+) om du vill lägga till ett eller flera intervallvärdesfält. Om det behövs bläddrar du till dialogrutans nedre del för att se ikonen.
   * Markera minustecknet (-) till höger om ett intervallvärdesfält om du vill ta bort det från listan.
   * Välj uppilsikonen och nedpilsikonen om du vill ändra ordningen på intervallvärdena.

1. Välj **[!UICONTROL OK]** och gå tillbaka till fliken Egenskaper.
1. I det övre vänstra hörnet av CRXDE Lite-sidan väljer du **[!UICONTROL Save All]** och sedan ikonen Bakåt i det övre vänstra hörnet för att återgå till Experience Manager.

   Se [Lägga till en videominiatyr](#adding-a-video-thumbnail).

### Lägga till en anpassad videominiatyr {#adding-a-custom-video-thumbnail-1}

De här stegen gäller endast för Dynamic Media som körs i hybridläge.

**Så här lägger du till en anpassad videominiatyr:**

1. Navigera till en överförd videoresurs som du vill lägga till en anpassad videominiatyr.
1. Välj videoresurs i resursurvalsläget, antingen från listvyn eller kortvyn.
1. I verktygsfältet väljer du ikonen **[!UICONTROL View Properties]** (en cirkel med&quot;i&quot;).
1. Välj **[!UICONTROL Change Thumbnail]** på videons egenskapssida.
1. Välj **[!UICONTROL Upload New Thumbnail]** i verktygsfältet på sidan Ändra miniatyrbild.
1. Navigera till en miniatyrbild som du vill använda, markera den och välj sedan **[!UICONTROL Open]** för att börja överföra bilden till Experience Manager. Efter överföringen måste du publicera bilden.
1. När du har överfört och publicerat bilden väljer du **[!UICONTROL Save Changes]** på sidan Ändra miniatyrbild.

   Den anpassade miniatyrbilden läggs till i videon.

## Ändra URL för dynamiska media för dynamiska medieresurser {#manifest-urls}

Videofilmer som bearbetas i Dynamic Media kan användas med körklara visningsprogram. Eller genom att gå till manifest-URL:erna och spela upp dem i anpassade visningsprogram. Nedan följer API:t för hämtning av manifest-URL:er för en video.

### Om API:t getVideoManifestURI

API:t `getVideoManifestURI` exponeras via c`q-scene7-api:com.day.cq.dam.scene7.api` och kan användas för att generera följande manifest-URL:er:

```java
/**   
* Returns the manifest url for videos 
* @param resource video resource 
* @param manifestType type of video streaming manifest being requested 
* @param onlyIfPublished return a manifest only if the video is published 
* @return the manifest url for videos 
* 
* @throws Exception 
*/
@Nullable 
String getVideoManifestURI(Resource resource, ManifestType manifestType, boolean onlyIfPublished) throws Exception;
```

#### getVideoManifestURI API-parametrar

Detta API har följande tre parametrar:

| Parameter | Beskrivning |
| --- | --- |
| `resource` | Resursen som motsvarar videon som Dynamic Media har kapslat. |
| `manifestType` | Kan vara antingen `ManifestType.DASH` eller `ManifestType.HLS` |
| `onlyIfPublished` | Ange som true om manifest-URI bara genereras om den är publicerad och tillgänglig på leveransnivån. |

Om du vill hämta manifest-URL:er för videoklipp med metoden ovan lägger du till en [videokodningsprofil](/help/assets/video-profiles.md#creating-a-video-encoding-profile-for-adaptive-streaming) i en mapp för överföring av videoklipp. Dynamic Media bearbetar dessa videoklipp baserat på kodningarna i den videokodningsfil som tilldelats mappen. Nu kan du anropa ovanstående API för att hämta manifest-URL:er för de överförda videoklippen.

### Felscenarier

API:t returnerar null om det finns fel. Undantag loggas i Experience Manager felloggar. Alla sådana loggade fel börjar med `Could not generate Video Manifest URI`. Följande scenarier kan orsaka sådana fel:

* En `IllegalArgumentException` loggas för något av följande:

   * Parametern `resource` som skickades är null.
   * Den `resource`-parameter som skickades är inte en video.
   * Parametern `manifestType` som skickades är null.
   * Parametern `onlyIfPublished` skickas som true, men videon publiceras inte.
   * Videon har inte importerats med en adaptiv videouppsättning från dynamiska media.

* `IOException` loggas när det uppstår ett problem med att ansluta till Dynamic Media.
* `UnsupportedOperationException` loggas när en `manifestType`-parameter som skickas är `ManifestType.DASH`, medan videon inte har bearbetats i DASH-format.

Följande är ett exempel på ovanstående API som använder serverlets skrivna i specifikationen *HTTPWhiteBoard* . Välj varje flik för kodsyntaxen.

>[!BEGINTABS]

>[!TAB Lägg till beroende i pom.xml]

+++**Lägg till beroende i pom.xml**

```java
dependency> 
     <groupId>com.day.cq.dam</groupId> 
     <artifactId>cq-scene7-api</artifactId> 
     <version>5.12.64</version> 
     <scope>provided</scope> 
</dependency> 
```

+++

>[!TAB Exempelserverlet]

+++**Exempelserverlet**

```java
@Component
        service = Servlet.class 
) 
@HttpWhiteboardServletPattern(value = ManifestServlet.SERVLET_PATTERN) 
@HttpWhiteboardContextSelect(value = Constants.SERVLET_CONTEXT_SELECTOR) 
public class ManifestServlet extends HttpServlet { 

   private static final Logger LOGGER = LoggerFactory.getLogger(ManifestServlet.class); 

   private final ObjectMapper objectMapper; 

    @Reference 
    private Scene7Service scene7Service; 

   public static final String SERVLET_PATTERN = Constants.VIDEO_API_PREFIX + "/manifestUrl"; 

   public ManifestServlet() {
         this.objectMapper = new ObjectMapper(); 
         objectMapper.setSerializationInclusion(JsonInclude.Include.NON_NULL); 
   }

   @Override 

   protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        final ResourceResolver resolver = getResourceResolver(request); 
        String assetPath = request.getParameter("assetPath"); 
        String manifest = request.getParameter("manifestType"); 
        String onlyIfPublished = request.getParameter("onlyIfPublished"); 
        Resource resource = resolver.getResource(assetPath); 
        response.setCharacterEncoding(StandardCharsets.UTF_8.toString()); 
        response.setContentType("application/json"); 
        if(resource == null) { 
            LOGGER.info("could not retrieve the resource from JCR"); 
            error("could not retrieve the resource from JCR", response); 
            return; 
        }

        String manifestUri = null; 

        try{ 
            ManifestType manifestType =  ManifestType.DASH; 
            if(manifest != null) { 
                manifestType = ManifestType.valueOf(manifest); 
            } 
            manifestUri = scene7Service.getVideoManifestURI(resource, manifestType, onlyIfPublished != null); 
            objectMapper.writeValue(response.getWriter(), new ManifestUrl(manifestUri)); 
            response.setContentType("application/json"); 
        } catch (Exception e) { 
            LOGGER.error(e.getMessage(), e); 
            error(String.format("Unable to get the manifest url for %s. %s", assetPath, e.getMessage()), response); 
        } 
    } 

    private ResourceResolver getResourceResolver(HttpServletRequest request) { 
        Object rr = request.getAttribute(AuthenticationSupport.REQUEST_ATTRIBUTE_RESOLVER); 
        if (!(rr instanceof ResourceResolver)) { 
            throw new IllegalStateException( 
                    "The request does not seem to have been created via Apache Sling's authentication mechanism."); 
        } else { 
            return (ResourceResolver) rr; 
        } 
    } 

    private void error(String errorMessage, HttpServletResponse response) throws IOException { 
        ManifestUrl errorManifest = new ManifestUrl(null); 
        errorManifest.setErrorMessage(errorMessage); 
        response.setStatus(HttpServletResponse.SC_INTERNAL_SERVER_ERROR); 
        objectMapper.writeValue(response.getWriter(), errorManifest); 
    } 
} 
```

+++

>[!TAB Svarsklass för server]

+++**Svarsklass för server**

```java
public class ManifestUrl extends VideoResponse { 
     String manifestUrl; 
     public ManifestUrl(String manifestUrl) { 
         this.manifestUrl = manifestUrl; 
     } 
     public String getManifestUrl() { 
         return manifestUrl; 
     } 
} 

public abstract class VideoResponse { 
     String errorString; 

     public String getErrorString() { 
         return errorString; 
     } 

     public void setErrorMessage(String errorString) { 
         this.errorString = errorString; 
     } 
} 
```

+++

>[!TAB Konstantfiler refereras i serverlet]

+++**Konstantfiler refereras i serverlet**

```java
public final class Constants { 

     private Constants() { 
     } 

     public static final String VIDEO_API_PREFIX = "/dynamicmedia/video"; 
     public static final String SERVLET_CONTEXT_SELECTOR = "(" + HttpWhiteboardConstants.HTTP_WHITEBOARD_CONTEXT_NAME + "=" + 
             DMSampleApiHttpContext.CONTEXT_NAME + ")"; 

 } 
```

+++

>[!TAB ServletContext]

+++**ServletContext**

Montera ovanstående servett med en `servletContext`. Följande är ett exempel på `servletContext`.

```java
public class DMSampleApiHttpContext extends ServletContextHelper { 

 public static final String CONTEXT_NAME = "com.adobe.dmSample"; 
 public static final String CONTEXT_PATH = "/dmSample"; 

 private final MimeTypeService mimeTypeService; 

 private final AuthenticationSupport authenticationSupport; 

 /** 
  * Constructs a new context that will use the given dependencies. 
  * 
  * @param mimeTypeService Used when providing mime type of requests. 
  * @param authenticationSupport Used to authenticate requests with sling. 
  */ 
 @Activate 
 public DMSampleApiHttpContext(@Reference final MimeTypeService mimeTypeService, 
                               @Reference final AuthenticationSupport authenticationSupport) { 
     this.mimeTypeService = mimeTypeService; 
     this.authenticationSupport = authenticationSupport; 
 } 

 // ---------- HttpContext interface ---------------------------------------- 
 /** 
  * Returns the MIME type as resolved by the <code>MimeTypeService</code> or 
  * <code>null</code> if the service is not available. 
  */ 
 @Override 
 public String getMimeType(String name) { 
     MimeTypeService mtservice = mimeTypeService; 
     if (mtservice != null) { 
         return mtservice.getMimeType(name); 
     } 
     return null; 
 } 

 /** 
  * Returns the real context path that is used to mount this context. 
  * @param req servlet request 
  * @return the context path 
  */ 
 public static String getRealContextPath(HttpServletRequest req) { 
     final String path = req.getContextPath(); 
     if (path.equals(CONTEXT_PATH)) { 
         return ""; 
     } 
     return path.substring(CONTEXT_PATH.length()); 
 } 

 /** 
  * Returns a request wrapper that transforms the context path back to the original one 
  * @param req request 
  * @return the request wrapper 
  */ 
 public static HttpServletRequest createContextPathAdapterRequest(HttpServletRequest req) { 
     return new HttpServletRequestWrapper(req) { 

         @Override 
         public String getContextPath() { 
             return getRealContextPath((HttpServletRequest) getRequest()); 
         } 

     }; 

 } 

 /** 
  * Always returns <code>null</code> because resources are all provided 
  * through individual endpoint implementations. 
  */ 
 @Override 
 public URL getResource(String name) { 
     return null; 
 } 

 /** 
  * Tries to authenticate the request using the 
  * <code>SlingAuthenticator</code>. If the authenticator or the Repository 
  * is missing this method returns <code>false</code> and sends a 503/SERVICE 
  * UNAVAILABLE status back to the client. 
  */ 
 @Override 
 public boolean handleSecurity(HttpServletRequest request, 
                               HttpServletResponse response) throws IOException { 

     final AuthenticationSupport authenticator = this.authenticationSupport; 
     if (authenticator != null) { 
         return authenticator.handleSecurity(createContextPathAdapterRequest(request), response); 
     } 

     // send 503/SERVICE UNAVAILABLE, flush to ensure delivery 
     response.sendError(HttpServletResponse.SC_SERVICE_UNAVAILABLE, 
             "AuthenticationSupport service missing. Cannot authenticate request."); 
     response.flushBuffer(); 

     // terminate this request now 
     return false; 
 } 
}
```

+++

>[!ENDTABS]

### Använda exempelservleten

Du anropar servern genom att utföra en `GET`-åtgärd på `/dmSample/dynamicmedia/video/manifestUrl`. Följande frågeparametrar skickas:

| Frågeparameter | Beskrivning |
| --- | --- |
| `assetPath` | Obligatoriskt. Sökvägen till videon som `manifestUrl` genereras för. |
| `manifestType` | Valfritt. Parametern kan vara DASH eller HLS. Om det inte skickas blir standardvärdet DASH. |
| `onlyIfPublished` | Valfritt. Om det skickas returneras `manifestUrl` bara om videon publiceras. |

I det här exemplet antar vi följande inställningar:

* Företaget är `samplecompany`.
* Redigeringsinstansen är `http://sample-aem-author.com`.
* En videokodningsprofil används för mappen `/content/dam/video-example`.
* Videon `scenery.mp4` överförs till mappen `/content/dam/video-example`.

Du kan anropa servleten på följande sätt:

| Typ | Beskrivning |
| :--- | --- |
| HLS | `http://sample-aem-author.com/dmSample/dynamicmedia/video/manifestUrl?manifestType=HLS&assetPath=/content/dam/video-example/scenery.mp4`<br><br>Om DASH-leverans är aktiverad:<br>`{"manifestUrl":"https://s7d1.scene7.com/is/content/samplecompany/scenery-AVS.m3u8?packagedStreaming=true"}`<br><br>Om DASH-leverans är inaktiverad:<br>`{"manifestUrl":"https://s7d1.scene7.com/is/content/samplecompany/scenery-AVS.m3u8"}` |
| DASH | `http://sample-aem-author.com/dmSample/dynamicmedia/video/manifestUrl?manifestType=DASH&assetPath=/content/dam/video-example/scenery.mp4`<br><br>Om DASH-leverans är aktiverad:<br>`{"manifestUrl":"https://s7d1.scene7.com/is/content/samplecompany/scenery-AVS.mpd"}`<br><br>Om DASH-leverans är inaktiverad:<br>`{}` |
| Fel: resurssökvägen är fel | `http://sample-aem-author.com/dmSample/dynamicmedia/video/manifestUrl?manifestType=DASH&assetPath=/content/dam/video-example/scennnnnnery.mp4`<br><br>`{"errorString":"could not retrieve the resource from JCR"}` |
