---
title: Interaktiva videoklipp
description: Lär dig hur du arbetar med interaktiv video och köpbar video i Dynamic Media
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
feature: Interactive Videos
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: caacf3b3-1e12-4ea3-9160-774181aadf41
source-git-commit: d4772c8844861ee82263e16d9c8608662e2e4870
workflow-type: tm+mt
source-wordcount: '5855'
ht-degree: 1%

---

# Interaktiva videor{#interactive-videos}

Du kan enkelt skapa interaktiva videor - även kallade videor som kan köpas - som genererar konverteringar direkt från videon. Kundens engagemang i videon äger rum i en panel bredvid videospelaren där relaterade tjänster, information eller produktminiatyrer rullas in i vyn baserat på vad som visas i videon. Kunderna kan välja miniatyrbilden och länkas direkt till tjänsten, lägga till artikeln i en kundvagn för direktköp eller länkas till en webbsida för mer information.

När videon är slut visas en visuell sammanfattning av alla erbjudanden för att skapa en call to action. Kunderna har en annan möjlighet att välja önskad artikel. Användbara och specifika upplevelser som dessa ökar kundernas engagemang och konverteringar.

Se även [Interaktiva bilder](/help/assets/interactive-images.md).

## Interaktiv video in action {#interactive-video-in-action}

Om du vill se en interaktiv, köpbar video in action väljer du [Livesändningar](https://landing.adobe.com/en/na/dynamic-media/ctir-2755/live-demos.html), bläddrar till rubriken **[!UICONTROL Shoppable Media]** på sidan och väljer videon som kan köpas.

* Under uppspelningen visas den identiska produkten till höger som en miniatyrbild när produkterna används i videon.

* Välj miniatyrbilden om du vill pausa videon och öppna snabbvyn för produkten. Välj till exempel miniatyrbilden Kitchenaid i videon för att få en 360-gradersvy av blandaren, eller zooma in för att se blandarinformationen.

<!-- There was a link here that showed the video frame of an interactive video and when the reader selected the frame the video would play https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/AXIS/index.html. This now needs to call a new interactive video-->

![En bildruta från en interaktiv, köpbar video](assets/chlimage_1-126.png) *En videobildruta från en interaktiv, köpbar video.*

>[!NOTE]
>
>Om du skapar en interaktiv video för att starta en webbsida när en användare väljer en miniatyrbild blockerar vissa enheter popup-webbsidan från att öppnas. I sådana fall måste du ändra inställningen för blockering av popup-fönster på enheten. På en Apple iPhone 6 går du till exempel till **[!UICONTROL Settings]** > **Safari** > **Blockera popup-fönster** och drar kontrollen till **[!UICONTROL Off]**. När du spelar upp en interaktiv video och väljer en miniatyrbild får du nu en fråga om du vill öppna popup-fönstret. Om du accepterar öppnas webbsidan.

### Se hur interaktiva videor skapas {#watch-how-interactive-videos-are-created}

Spela upp en genomgång av [hur interaktiva videor skapas](https://s7d5.scene7.com/s7viewers/html5/VideoViewer.html?videoserverurl=https://s7d5.scene7.com/is/content/&emailurl=https://s7d5.scene7.com/s7/emailFriend&serverUrl=https://s7d5.scene7.com/is/image/&config=Scene7SharedAssets/Universal_HTML5_Video_social&contenturl=https://s7d5.scene7.com/skins/&asset=S7tutorials/InteractiveVideo) (7 minuter och 30 sekunder).
Även om videogenomgången märks med Assets on Demand, gäller fortfarande principerna och stegen för Interactive Video i Adobe Experience Manager Assets.

### Adobe Customer Solutions Webinar {#adobe-customer-success-webinar}

Webbseminariet&quot;Using Interactive Video, Link Sharing, and YouTube sharing in Experience Manager Assets&quot; visar hur du använder interaktiv video och andra funktioner för att knyta konverteringsdrivna händelser till videomarknadsföringsmaterialet.

<!-- NOT FOUND; FIND REPLACEMENT
>[!NOTE]
>
>[Using Interactive Video, Link Sharing, and YouTube sharing in Experience Manager Assets](https://adobecustomersuccess.adobeconnect.com/p1yxzdo4aec/).
-->

## Snabbstart: Interaktiva videoklipp {#quick-start-interactive-videos}

Följande steg-för-steg-beskrivning av arbetsflödet hjälper dig att komma igång snabbt med interaktiva videor i Dynamic Media.

Leta efter rubriken **Exempel** i några av snabbstartsaktiviteterna. Den innehåller en kort självstudiekurs som baseras på den här startdemonstrationswebbsidan som *inte* har lagt till interaktivitet i den än:

[https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-0.html](https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-0.html)

**Exemplen** visar hur du integrerar interaktiva videofilmer på en webbplats.

När du är klar med självstudiekursen i det sista exempelavsnittet ser den slutliga demowebbsidan med den helintegrerade interaktiva videon ut så här:

[https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-3.html](https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-3.html)

Interaktiva videosteg:

1. **(Valfritt) Identifiera QuickView-variabler** - Börja med att identifiera dynamiska variabler som används av den befintliga QuickView-implementeringen. Du använder variablerna för att mappa produktminiatyrbilder till deras motsvarande produkt-snabbvyn när du skapar en interaktiv video. Se [&#x200B; (valfritt) Identifiera QuickView-variabler &#x200B;](#optional-identifying-quickview-variables).
   *Det här steget krävs bara om alla följande är true*:
   * Du vill lägga till interaktivitet i videon genom att aktivera snabbvyn.
   * Din implementering av Experience Manager *använder inte* ett ramverk för e-handelsintegrering för att hämta produktdata till Experience Manager från en e-handelslösning som IBM® WebSphere® Commerce, Elastic Path, Hybris eller Intershop.
1. **(Valfritt) Skapa en förinställning för Interactive Video Viewer** - Anpassa utseendet och beteendet för olika komponenter som utgör spelaren, t.ex. videobandspelaren och de interaktiva miniatyrbilderna.
Det är inte nödvändigt att skapa en egen förinställning för Interactive Video Viewer om du tänker använda förinställningarna `Shoppable_Video_Light` eller `Shoppable_Video_Dark` som är klara att användas i stället.
Se [Skapa en visningsförinställning](/help/assets/managing-viewer-presets.md#creating-a-new-viewer-preset) (valfritt) och [Specialöverväganden för att skapa en förinställning för interaktivt visningsprogram](/help/assets/managing-viewer-presets.md#special-considerations-for-creating-an-interactive-viewer-preset).

1. **Överför en video och dess associerade bildresurser** - Överför en video och associerade bilder som du vill göra interaktiva.
Se [Överför en video och dess associerade miniatyrbildsresurser](#uploading-a-video-and-its-associated-thumbnail-assets).

   >[!NOTE]
   >
   >MXF-videoformatet stöds ännu inte för användning med interaktiva videor i Dynamic Media.

1. **Lägg till interaktivitet i videon** - Lägg till ett eller flera tidssegment i videon. Associera sedan bildminiatyrer inom dessa tidssegment. Tilldela varje miniatyrbild till en åtgärd som en hyperlänk, en snabbvy eller ett Experience Fragment.
(Den URL-baserade länkningsmetoden är inte möjlig om det interaktiva innehållet har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.)
Slutför genom att publicera interaktiva videor. Publicering skapar den inbäddningskod eller URL som du så småningom kopierar och använder på webbplatsens landningssida. Se [Lägg till interaktivitet i videon](#adding-interactivity-to-your-video).
Se [Publicera Assets](/help/assets/publishing-dynamicmedia-assets.md).

1. **Lägg till en interaktiv video på din webbplats eller på din webbplats i Experience Manager** - Om du använder Experience Manager Sites eller eCommerce, eller båda, kan du lägga till den interaktiva videon på en webbsida. Dra Interactive Media-komponenten till sidan i Experience Manager. Se [Lägg till Assets för dynamiska media på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md).
Använd inbäddningskoden eller URL-adressen för att integrera interaktiv video med webbplatsupplevelserna. Se [Integrera en interaktiv video med din webbplats](#integrating-an-interactive-video-with-your-website).
Om du använder en WCM-fil (Web Content Manager) från tredje part måste du integrera den nya interaktiva videon med den befintliga QuickView-implementeringen som används på webbplatsen. Se [Integrera en interaktiv video med en befintlig snabbvy](#integrating-an-interactive-video-with-an-existing-quickview).
   [Lägg till Assets för dynamiska media på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md)

## (Valfritt) Identifiera QuickView-variabler {#optional-identifying-quickview-variables}

>[!NOTE]
>
>Den här aktiviteten krävs bara om följande är sant:
>
>* Du vill lägga till interaktivitet i videon genom att aktivera snabbvyn.
>* Din implementering av Experience Manager *använder inte* ett ramverk för e-handelsintegrering för att hämta produktdata till Experience Manager från en e-handelslösning som IBM® WebSphere® Commerce, Elastic Path, Hybris eller Intershop.
>
>Om din implementering av Experience Manager använder e-handel kan du hoppa över den här uppgiften och fortsätta med nästa uppgift.

Börja med att identifiera dynamiska variabler som används i den befintliga QuickView-implementeringen, så att du kan mappa produktminiatyrbilder till deras motsvarande produkt i QuickView när du skapar interaktiva videofilmer.

När du lägger till tidssegment i en video tilldelar du en SKU (Stock Keeping Unit) och eventuella ytterligare variabler till varje miniatyrbild som du lägger till i ett segment. Sådana variabler används senare för att visa rätt Quickview-produkt.

Det är viktigt att kunna identifiera vilka variabler som krävs för att utlösa en produktsnabbvy på ett unikt sätt.

Ibland räcker det att rådfråga IT-specialister som är ansvariga för din befintliga QuickView-implementering. De känner troligen till den minsta uppsättningen data för att identifiera Quickview i systemet. Det går dock även att analysera det befintliga beteendet för koden.

De flesta QuickView-implementeringar använder följande paradigm:

* Användaren aktiverar ett element i användargränssnittet på webbplatsen. Du kan till exempel välja en snabbvyknapp.
* Webbplatsen skickar en Ajax-begäran till serverdelen för att läsa in QuickView-data eller -innehåll vid behov.
* Quickview-data översätts till innehållet som förberedelse för återgivning på webbsidan.
* Slutligen återges sådant innehåll på skärmen visuellt i koden.

Du bör därför besöka olika delar av webbplatsen där Quickview implementeras, aktivera snabbvyn och hämta den Ajax-URL som webbsidan skickar för att läsa in QuickView-data eller -innehåll.

Normalt behöver du inte använda några specialverktyg för felsökning. Moderna webbläsare har webbinspektörer som klarar ett bra jobb. Nedan följer några exempel på webbläsare som innehåller webbinspektörer:

* Om du vill visa alla utgående HTTP-begäranden i Google Chrome trycker du på **F12** (Windows) eller **Kommando+Alternativ+I** (Mac) för att öppna panelen Utvecklarverktyg och väljer sedan fliken **Nätverk** .

* I Firefox kan du antingen aktivera plugin-programmet för Firebug genom att trycka på **F12** (Windows) eller **Kommando+Alternativ+I** (Mac) och använda fliken **`[Net]`**. Du kan också använda det inbyggda verktyget Granska och fliken Nätverk.

* Aktivera felsökningsverktyget i Internet Explorer genom att trycka på **F12**.

När nätverksövervakning är aktiverat i webbläsaren utlöser du snabbvyn på sidan.

Nu kan du hitta Quickview Ajax-URL:en i nätverksloggen och kopiera den inspelade URL:en för framtida analys. När du utlöser snabbvyn skickas vanligtvis flera begäranden till servern. Vanligtvis är Quickview Ajax-URL en en av de första i listan. Den har antingen en komplex frågesträngsdel eller sökväg och dess MIME-svarstyp är antingen `text/html`, `text/xml` eller `text/javascript`.

Under den här processen är det viktigt att du besöker olika delar av webbplatsen, med olika produktkategorier och typer. Anledningen är att URL-adresser för snabbvyn kan innehålla delar som är gemensamma för en viss webbplatskategori, men bara ändras om du besöker ett annat område på webbplatsen.

I det enklaste fallet är den enda variabeldelen i snabbvyns URL produktens SKU. I det här fallet är produktens SKU-värde den enda datadel som behövs för att lägga till miniatyrbilder i ett tidssegment i den interaktiva videon i Experience Manager.

I komplexa fall har dock QuickView-webbadressen olika element utöver produktversionen, till exempel kategori-ID, färgkod och storlekskod. I sådana fall blir varje sådant element en separat variabel i definitionen av miniatyrbildsdata i Experience Manager.

Titta på följande exempel på QuickView-URL:er och deras resulterande miniatyrbildsvariabler:

<table>
  <tbody>
  <tr>
    <td><p>En SKU, hittades i frågesträngen.</p> </td>
    <td><p>De inspelade URL:erna för snabbvyn är bland annat följande:</p>
    <ul>
      <li><p><code>https://server/json?productId=866558&source=100</code></p> </li>
      <li><p><code>https://server/json?productId=1196184&source=100</code></p> </li>
      <li><p><code>https://server/json?productId=1081492&source=100</code></p> </li>
      <li><p><code>https://server/json?productId=1898294&source=100</code></p> </li>
    </ul> <p>Den enda variabeldelen i URL:en är värdet på frågesträngsparametern <code>productId=</code>, och det är tydligt ett SKU-värde. Därför behöver dina miniatyrbilder bara SKU-fält med värden som <strong><code>866558</code></strong>, <strong><code>1196184</code></strong>, <strong><code>1081492</code></strong>, <strong><code>1898294</code></strong>.</p> </td>
  </tr>
  <tr>
    <td><p>En SKU, finns i URL-sökvägen.</p> </td>
    <td><p>De inspelade URL:erna för snabbvyn är bland annat följande:</p>
    <ul>
      <li><p><code>https://server/product/6422350843</code></p> </li>
      <li><p><code>https://server/product/1607745002</code></p> </li>
      <li><p><code>https://server/product/0086724882</code></p> </li>
    </ul> <p>Variabeldelen finns i den sista delen av sökvägen och blir SKU-värdet för Experience Manager-miniatyrbilder: <strong><code>6422350843</code></strong>, <strong><code>1607745002</code></strong>, <strong><code>0086724882</code></strong>.</p> </td>
  </tr>
  <tr>
    <td><p>SKU och kategori-ID i frågesträngen.</p> </td>
    <td><p>De inspelade URL:erna för snabbvyn är bland annat följande:</p>
    <ul>
      <li><p><code>https://server/quickView/product/?category=1100004&prodId=305466</code></p> </li>
      <li><p><code>https://server/quickView/product/?category=1100004&prodId=310181</code></p> </li>
      <li><p><code>https://server/quickView/product/?category=1740148&prodId=308706</code></p> </li>
    </ul> <p>I det här fallet finns det två olika delar i URL:en. SKU:n lagras i parametern <code>prodId</code> och kategori-ID:t lagras i parametern <code>category=</code>.</p> <p>Miniatyrbildsdefinitionerna är par. Det vill säga ett SKU-värde och en extra variabel som kallas <code>categoryId</code>. De resulterande paren är följande:</p>
    <ul>
      <li>SKU är <code>305466</code> och <code>categoryId</code> är <code>1100004</code></li>
      <li>SKU är <code>310181</code> och <code>categoryId</code> är <code>1100004</code></li>
      <li>SKU är <code>308706</code> och <code>categoryId</code> är <code>1740148</code></li>
    </ul> <p> </p> </td>
  </tr>
  </tbody>
</table>

**Exempel**

När ovanstående metod används på exempelwebbplatsen har du en webbsida med flera produktminiatyrbilder där var och en har knappen &quot;SE MER&quot;:

[https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-0.html](https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-0.html)

När du har aktiverat alla produkter i snabbvyn som är tillgängliga på sidan får du följande lista över snabbvybegäranden som gjorts i bakgrunden:

* datafeed/candles-233396346.json
* datafeed/candles-233978050.json
* datafeed/candles-234024346.json
* datafeed/candles-234024356.json
* datafeed/candles-234024359.json
* datafeed/cushions-233939848.json
* datafeed/cushions-234019477.json
* datafeed/cushions-234019483.json
* datafeed/furniture-231747479.json
* datafeed/furniture-232625621.json
* datafeed/furniture-232625626.json
* datafeed/furniture-233939810.json
* datafeed/furniture-233939825.json
* datafeed/furniture-233939828.json
* datafeed/furniture-233939853.json
* datafeed/furniture-233940334.json
* datafeed/glassware-000064007.json
* datafeed/glassware-230722193.json
* datafeed/glassware-233916550.json
* datafeed/glassware-233916597.json

Om du tittar på serveranropen ser du att produktspecifik information bara finns i sökvägen för begäran. Du observerar också att frågesträngen inte används alls och att det finns två olika typer av datadelar:

* Den första typen är ljus, kuddar, möbler och glasvaror. Du kan kalla den här&quot;produktkategorin&quot;.
* Den andra typen är produktkod, till exempel 233916597. Du kan anta att det är&quot;produkt-SKU&quot;.

Med hjälp av den här informationen har hela snabbvyns URL följande mönster:

`/datafeed/$categoryId$-$SKU$.json`

Utifrån en sådan analys kan du sluta dig till att du kan använda följande två variabler för miniatyrbilderna: `categoryId` och `SKU`.

Du är nu redo att överföra en video och dess associerade miniatyrbildsresurser.

## (Valfritt) Skapa en förinställning för Interactive Video Viewer {#optional-creating-an-interactive-video-viewer-preset}

Du kan hoppa över den här uppgiften och fortsätta till nästa om du tänker använda någon av de förinställda typerna `Shoppable_Video_dark` eller `Shoppable_Video_light` som är färdiga som standard, Interactive Video Viewer.

När en miniatyrbild är markerad i redigeringsmiljön visas en förhandsvisning av dialogrutan Snabbvy.

![chlimage_1-21](assets/chlimage_1-127.png)

Du kan också skapa en egen förinställning för Interactive Video Viewer. Du kan bland annat bestämma hur videospelaren, de interaktiva miniatyrbilderna och miniatyrbildsvyn som visas i slutet av videon ska se ut.

En förinställning för Interactive Video Viewer återger videon och alla tidslinjesegment som du har lagt till. I förhandsvisningsläget används dessutom ett standardexempel för snabbvyn när du väljer en miniatyrbild av en produkt, så att du kan testa dess interaktivitet före publicering.

När du har sparat visningsförinställningen ställs dess läge automatiskt in på **På** på sidan Visningsförinställningar. Det här läget innebär att den är synlig i komponenten Dynamic Media och när du förhandsgranskar en video med den. Se också till att du publicerar den nya visningsförinställningen manuellt.

Se [Skapa en ny visningsförinställning](/help/assets/managing-viewer-presets.md#creating-a-new-viewer-preset) om du vill skapa en egen förinställning för Interactive Video Viewer.

## Överföra en video och dess tillhörande miniatyrbilder {#uploading-a-video-and-its-associated-thumbnail-assets}

Om du redan har överfört dina video- och miniatyrresurser går du vidare till [Lägg till interaktivitet i videon](#adding-interactivity-to-your-video).

>[!NOTE]
>
>MXF-videoformatet stöds ännu inte för användning med interaktiva videoklipp i Dynamic Media.

Om du har överfört fel videoklipp eller bilder, eller om du vill ta bort överförda videoklipp eller bilder som du inte längre behöver, se [Ta bort resurser](/help/assets/manage-assets.md#deleting-assets).

Så här överför du en video och dess associerade miniatyrbildsresurser:

1. Överför videon och tillhörande miniatyrbilder till den eller de mappar du vill använda.

   Se [Överför resurser](/help/assets/manage-assets.md).
Se [Överför resurser med FTP-jobbplanering](/help/assets/manage-assets.md).

   Nu kan du lägga till interaktivitet i videon.

## Lägg till interaktivitet i videon {#adding-interactivity-to-your-video}

Du lägger till tidslinjesegment i en video med den visuella redigeraren på sidan Skapa interaktiv video.

När du har lagt till tidslinjesegment lägger du till miniatyrbilder i varje segment. För varje miniatyrbild som du lägger till tillämpar du en åtgärd på den. Du kan t.ex. använda en snabbvy för miniatyrbilden eller tilldela en hyperlänk till den eller ett Experience Fragment.

Se [Upplevelsefragment](/help/sites-authoring/experience-fragments.md).

>[!NOTE]
>
>Delningsverktygen för sociala medier i interaktiv video stöds inte när du bäddar in visningsprogrammet i ett Experience Fragment. Du kan undvika det här problemet genom att använda eller skapa visningsförinställningar som inte har verktyg för delning av sociala medier. Med sådana visningsförinställningar kan du bädda in dem i Experience Fragments.

>[!NOTE]
>
>Den URL-baserade länkningsmetoden är inte möjlig om det interaktiva innehållet har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.

Alternativen Ångra och Gör om, nära det övre högra hörnet på sidan, stöds under den aktuella skaps-/redigeringssessionen.

När du har sparat den interaktiva videon öppnas videon direkt i förhandsvisningen. Därifrån kan du välja en förinställning för Interactive Video Viewer och spela upp videon för att se en ungefärlig representation av hur den ser ut för kunderna.

**Så här lägger du till interaktivitet i videon:**

1. I vyn Assets navigerar du till videon som du överförde och vill göra interaktiv.
1. Gör något av följande:

   * Hovra över bilden och välj sedan **[!UICONTROL Select]** (bockmarkeringsikon). Välj **[!UICONTROL Edit]** i verktygsfältet.

   * Håll pekaren över bilden och välj sedan **[!UICONTROL More actions]** (ikonen med tre punkter) **[!UICONTROL > Edit]**.

   * Markera bilden så att du kan öppna den på sidan Detaljvy. Välj **[!UICONTROL Edit]** i verktygsfältet.

1. Gör något av följande på sidan Skapa interaktiv video:

   * Välj knappen **[!UICONTROL Play]** för att börja spela upp videon. När en viss produkt, tjänst eller detalj som du vill markera visas väljer du **[!UICONTROL Add Segment]** i verktygsfältet. Upprepa tills du har nått slutet av videon.

     För varje tidssegment som du lägger till tilldelar du en eller flera miniatyrbilder till det och länkar sedan miniatyrbilderna till QuickView-produktsidor som kunderna kan köpa eller till webbsidor för mer information.

   * Välj knappen **[!UICONTROL Play]** för att börja spela upp videon. När en viss produkt, tjänst eller detalj som du vill markera visas väljer du **[!UICONTROL Pause]**. Välj **[!UICONTROL Add Segment]**.

     Fortsätt spela upp och pausa videon vid punkter längs tidslinjen där du vill lägga till ett segment tills du når slutet av videon.

1. (Valfritt) Dra fältet till vänster om du vill zooma in eller till höger om du vill zooma ut, så att du kan styra hur mycket detaljer som visas för de segment du har lagt till.**[!UICONTROL Timeline Scale Slider]**

   ![chlimage_1-22](assets/chlimage_1-128.png)

   Beroende på videons längd är standardvärdet för Segmentvaraktighet följande värden:

   <table>
      <tbody>
        <tr>
        <td><strong>Om videolängden är ...</strong></td>
        <td><strong>Inställningen för segmentvaraktighet är som standard..</strong></td>
        </tr>
        <tr>
        <td>3 minuter eller mer</td>
        <td>60 sekunder</td>
        </tr>
        <tr>
        <td>2-3 minuter</td>
        <td>30 sekunder</td>
        </tr>
        <tr>
        <td>1-2 minuter</td>
        <td>20 sekunder<br /> </td>
        </tr>
        <tr>
        <td>30-60 sekunder</td>
        <td>10 sekunder</td>
        </tr>
        <tr>
        <td>30 sekunder eller mindre</td>
        <td>5 sekunder</td>
        </tr>
      </tbody>
    </table>

   Videons tidslinje använder så mycket skärmutrymme som det som finns tillgängligt för den. Därför behåller de segment som du har lagt till sin korrekta bredd när webbläsarens storlek ändras.

   Följande tre skärmbilder använder samma video. Observera att bredden på varje segment ändras beroende på inställningen för tidslinjens skala.

   ![chlimage_1-23](assets/chlimage_1-129.png)

   Skärmbild A

   Skärmbild A ovan visar standardvyn för en 29-sekunders produktvideo. Tidslinjens skala är inställd på 5 sekunder som standard.

   ![chlimage_1-130](assets/chlimage_1-130.png)

   Skärmbild B

   I skärmbild B ovan har reglaget för tidslinjeskala dragits från standardvärdet 5 sekunder till 3 sekunder. Observera att tidstämplarna för den enskilda tidslinjens skala nu är inställda med 3-sekundersintervall.

   ![chlimage_1-25](assets/chlimage_1-131.png)

   Skärmbild C

   I skärmbild C ovan flyttades inställningen för tidslinjeskala till 8 sekunder. Lägg märke till hur segmenten som innehåller produktminiatyrer har krympt. Det är praktiskt att zooma ut på det här sättet om du har en lång video och vill kunna se en översikt över fler segment som normalt passar in på sidbredden.

1. (Valfritt) Gör något av följande:

   * Justera ett segments starttid och sluttid.

     Markera ett segment och dra sedan den inledande eller avslutande blå ovalen för att justera start- respektive sluttiden. Den videobildruta som visas flyttas till lämplig tid i videon, baserat på dina justeringar. Tidslinjesegmentets rörelse begränsas baserat på intilliggande segment i tidslinjen. Den minsta tillåtna segmenttiden är en sekund.

     Använd följande kortkommandon för att snabbt kontrollera och finjustera videosegmenten:

      * Om du vill söka efter videon direkt till början av det segmentet väljer du den blå inledande ovalen.
      * Om du vill söka efter videon direkt i slutet av det segmentet markerar du den efterföljande blå ovalen.
      * Om du vill återställa videouppspelningen till början av det segmentet markerar du hela segmentet.

   ![chlimage_1-26](assets/chlimage_1-132.png)

   Flytta slutet av ett tidslinjesegment

   * Ta bort ett segment

     Markera det sista segmentet på tidslinjen och välj sedan **[!UICONTROL Delete Segment]** i verktygsfältet. Om två eller flera segment är markerade är **[!UICONTROL Delete Segment]** inaktiverat.

     Du kan bara ta bort det sista segmentet. Om du till exempel vill ta bort alla segment på tidslinjen måste du alltid markera det sista och sedan välja **[!UICONTROL Delete Segment]**.

1. Markera ett tidssegment som du vill associera en eller flera miniatyrbilder till.
1. Välj fliken **[!UICONTROL Content]** till höger om videon.
1. Välj **[!UICONTROL Select Assets]** på fliken Innehåll och bläddra sedan bland och markera alla bildresurser som du vill använda med videon. De markerade resurserna läggs till på panelen Resursväljare på fliken Innehåll.

1. Gör något av följande i resursväljaren under fliken Innehåll:

   <table>
      <tbody>
        <tr>
        <td>Associera en miniatyrbild med det markerade tidslinjesegmentet</td>
        <td><p>Markera bilden på resurssväljarpanelen till höger.</p> <p>Du kan lägga till så många miniatyrbilder du vill i ett tidslinjesegment. För varje bild som du väljer visas en bock över bilden i resursväljaren.</p> </td>
        </tr>
        <tr>
        <td>Ta bort en miniatyrbild från det markerade tidslinjesegmentet</td>
        <td><p>Gör något av följande:</p>
          <ul>
          <li>Avmarkera en bild med en bockmarkering på resurssväljarpanelen. Bildresursen har tagits bort från tidslinjesegmentet.<br /> </li>
          <li>Markera en bild i det markerade tidslinjesegmentet och välj sedan <strong>Ta bort produkt</strong> i verktygsfältet.</li>
          </ul> </td>
        </tr>
      </tbody>
    </table>

   ![Resursväljaren](assets/chlimage_1-133.png)

   Om du väljer en bild på resurssväljarpanelen läggs den till i det valda tidslinjesegmentet.

1. Markera en enda miniatyrbild i ett av tidslinjesegmenten och välj sedan fliken **[!UICONTROL Actions]**.
1. Gör något av följande:
   <table> 
    <tbody> 
      <tr> 
      <td>Associera den markerade miniatyrbilden med en snabbvy</td> 
      <td><p>Välj <strong>Snabbvy</strong> under Åtgärdstyp.</p> <p>Om du är kund inom Experience Manager Sites och e-handel:</p> 
       <ul> 
       <li>Observera att textfältet SKU-värde är förifyllt med den valda produktens SKU (Stock Keeping Unit), som är en unik identifierare för varje separat produkt eller tjänst som du erbjuder. Värdet fylls i automatiskt när bilden är kopplad till en produkt i Experience Manager Commerce.</li> 
       <li>Om den i förväg ifyllda SKU:n är felaktig väljer du produktväljarens ikon (förstoringsglas) för att öppna sidan Välj produkt. Markera den produkt du vill använda och markera sedan bockmarkeringen i det övre högra hörnet av sidan så att du kan gå tillbaka till Interactive Video Editor.</li> 
       </ul> <p> Om du <em>inte</em> är en Experience Manager Sites- eller e-handelskund</p> 
       <ul> 
       <li>Se <a href="/help/assets/carousel-banners.md#identifying-hotspot-and-image-map-variables">Identifiera hotspot-variabler</a>. Variablerna måste definieras.  </li> 
       <li>Som standard använder det här SKU-fältet bildresursens filnamn utan filnamnstillägget. Om du använder en standardnamngivningskonvention för dina filer som baseras på SKU, kräver vanligtvis inte det här filnamnet några ytterligare redigeringar. </li> 
       <li>I annat fall redigerar du standardvärdet och anger rätt SKU-värde. I textfältet SKU-värde skriver du produktens SKU (Stock Keeping Unit), som är en unik identifierare för varje separat produkt eller tjänst som du erbjuder. Det angivna SKU-värdet fyller automatiskt i variabeldelen av QuickView-mallen så att systemet vet att den valda bilden associeras med en viss SKU:s snabbvy.</li> 
       </ul> <p>(Valfritt) Om det finns andra variabler i snabbvyn som du måste använda för att ytterligare identifiera en produkt väljer du <strong>Lägg till allmän variabel</strong>. Ange en extra variabel i textfältet. <code>category=Womens</code> är till exempel en tillagd variabel.</p> <p> </p> </td> 
      </tr> 
      <tr> 
      <td>Associera den markerade miniatyrbilden med en hyperlänk</td> 
      <td><p>Välj <strong>Hyperlänk</strong> under Åtgärdstyp och gör sedan något av följande:</p> 
       <ul> 
       <li>Om du är kund hos Experience Manager Sites väljer du ikonen Platsväljare (mapp) för att navigera till en webbsida. Den URL-baserade länkningsmetoden är inte möjlig om det interaktiva innehållet har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.</li> 
       <li>Om du är en fristående kund av Dynamic Media anger du den fullständiga URL-sökvägen till en länkad webbsida i textfältet HREF.</li> 
       </ul> <p>Var noga med att ange om länken ska öppnas på en ny flik i webbläsaren eller på den aktuella fliken.</p> </td> 
      </tr> 
      <tr> 
      <td>Associera den markerade miniatyrbilden med ett Experience Fragment</td> 
      <td><p>Välj <strong>Experience Fragment</strong> under Åtgärdstyp och gör sedan följande:<p> 
       <ul> 
       <li>Om du är Experience Manager Sites-kund väljer du sökikonen (förstoringsglas) för att öppna sidan Experience Fragment. Markera det Experience Fragment som du vill använda och välj sedan <strong>Markera </strong> i det övre högra hörnet på sidan så att du kan gå tillbaka till åtgärdspanelen på föregående sida.<br /> Se <a href="/help/sites-authoring/experience-fragments.md">Upplevelsefragment</a>.</li> 
      </ul> 
       <ul> 
       <li>Ange bredden och höjden på Experience Fragment så som du vill att det ska visas i videon.</li>
       </ul><strong>Obs!</strong>: Delningsverktygen för sociala medier i interaktiv video stöds inte när du bäddar in visningsprogrammet i ett Experience Fragment. Du kan undvika det här problemet genom att använda eller skapa visningsförinställningar som inte har verktyg för delning av sociala medier. Med sådana visningsförinställningar kan du bädda in dem i Experience Fragments.</p></tr>&lt; 
      <tr> 
      <td>Redigera en åtgärd som redan har tilldelats en miniatyrbild</td> 
      <td>Markera en miniatyrbild som har en kedjelänk till höger om textetiketten i ett tidslinjesegment. Länken kedja anger att en åtgärd har tilldelats den. Välj fliken <strong>Åtgärder</strong> så att du kan göra ändringarna.</td> 
      </tr> 
      <tr> 
      <td>Ändra textetiketten för en miniatyrbild</td> 
      <td><p>Som standard använder textetiketten miniatyrbildens <code>Title</code>-metadatafält. Om <code>Title</code> inte finns används miniatyrbildens filnamn i stället, men utan filnamnstillägget.</p> <p>Om du vill ändra textetiketten för en miniatyrbild anger du önskad text under fliken <strong>Åtgärder </strong>direkt under bildresursen som visas. Se skärmbilden nedan.</p> <p>Den nya textetiketten används bara av själva videospelaren och den miniatyrtext som visas i tidslinjesegmentet. Etikettändringen påverkar inte miniatyrbildens metadatafält Titel eller dess filnamn.</p> </td> 
      </tr> 
      <tr> 
      <td>Så här återställer du en ändring:</td> 
      <td>I närheten av sidans övre högra hörn väljer du <strong>Ångra</strong> eller <strong>Gör om</strong>.</td> 
      </tr> 
    </tbody> 
   </table>

   ![ExperienceDefragment_interactiveVideos](assets/experiencefragment_interactivevideos.png)

   En ny textetikett läggs till i miniatyrbilden.

1. Gör något av följande:

   * Upprepa steg 6-11 om du vill lägga till fler miniatyrbilder i tidslinjesegment i videon.
   * Fortsätt till det valfria steget 13.

1. (Valfritt) Gör något av följande:

   * **[!UICONTROL Merge Segment]** - Du kan kombinera två intilliggande segment (med eller utan produktminiatyrer tilldelade till dem) till ett segment.

     Markera på tidslinjen två eller flera sammanhängande segment som du vill sammanfoga till ett. Det finns inga blå ovala draghandtag på de två valda segmenten i skärmbilden nedan.

     Välj **[!UICONTROL Merge Segment]** i verktygsfältet.

   ![chlimage_1-134](assets/chlimage_1-134.png)

   Sammanfoga två valda femsekunderssegment till ett segment på tio sekunder.

   * **[!UICONTROL Split Segment]** - Du kan dela upp ett enskilt segment i två lika långa segment. Om det redan finns produktminiatyrer för segmentet kombineras miniatyrbilderna till det vänstra segmentet.

     På tidslinjen markerar du ett segment som du vill dela upp till hälften och väljer sedan **[!UICONTROL Split Segment]** i verktygsfältet.

     Om du markerar två eller flera segment inaktiveras funktionen **[!UICONTROL Split Segment]**.

   ![chlimage_1-135](assets/chlimage_1-135.png)

   Dela upp ett markerat segment på tio sekunder i två segment på fem sekunder vardera.

1. I närheten av det övre högra hörnet på sidan **[!UICONTROL Create Interactive Video]** visas namnet på den valda visningsförinställningen som används med videon. Markera namnet om du vill välja en annan visningsförinställning.

   Med visningsförinställningen `Shoppable_Video_light` kan du till exempel spela upp videon med ett vitt visningsområde bredvid videon. Visningsområdet är där de valda miniatyrbilderna visas under uppspelningen. Med visningsförinställningen `Shoppable_Video_dark` kan du spela upp videon med ett svart visningsområde bredvid videon.

   Om du har skapat en egen förinställning för Interactive Video Viewer visas den i listan med förinställningar som du kan välja mellan.

   När du är klar väljer du **[!UICONTROL Save]**.

   >[!NOTE]
   >
   >När du sparar den interaktiva videon sparas en associerad `.vtt`-fil automatiskt med den. Filen `.vtt` sparas i mappen `_VTT` i roten av **[!UICONTROL Assets]**. Filen och mappen är nödvändiga för att den interaktiva videon ska kunna spelas upp korrekt på webbplatsen. Därför ska du inte flytta, redigera eller ta bort mappen `_VTT` eller dess innehåll.

1. Publicera den interaktiva videon. Publicering skapar den inbäddningskod eller URL som du så småningom kopierar och klistrar in på webbplatsupplevelserna.

   Om du har lagt till interaktivitet med snabbvyn ska du bara använda inbäddningskoden. Om du har lagt till interaktivitet med hyperlänkade webbsidor kan du även använda den publicerade URL:en. Observera dock att den URL-baserade länkningsmetoden inte är möjlig om ditt interaktiva innehåll har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.

   Se [Publicera resurser](publishing-dynamicmedia-assets.md).

   >[!NOTE]
   >
   >Om du vill publicera en videoklipp som kan köpas med QuickView måste du även publicera videons relaterade bildresurser separat från din e-handelsplats.

   När du har lagt till tidslinjesegment och publicerat den interaktiva videon kan du lägga till den på din befintliga startsida för webbplatsen. Se [Integrera en interaktiv video med din webbplats](#integrating-an-interactive-video-with-your-website).

## Publicera interaktivt videomaterial {#publishing-interactive-video-assets}

Se [Publicera resurser](/help/assets/publishing-dynamicmedia-assets.md) för mer information om hur du publicerar interaktiva videoresurser.

## Integrera en interaktiv video med webbplatsen {#integrating-an-interactive-video-with-your-website}

När du har överfört en video, lagt till tidslinjesegment i den och publicerat den interaktiva videon är du nu redo att lägga till den på din befintliga webbplats.

Om du är kund hos Experience Manager Sites kan du lägga till den interaktiva videon genom att dra Interactive Media-komponenten till din sida. Se [Lägg till dynamiska medieresurser på sidor](/help/assets/adding-dynamic-media-assets-to-pages.md).

Om du är en fristående Experience Manager Assets-kund kan du lägga till den interaktiva videon manuellt på din webbplats enligt beskrivningen i det här avsnittet.

1. Kopiera den publicerade interaktiva videons inbäddningskod eller URL.
Se [Bädda in video- eller bildvisningsprogrammet på en webbsida](/help/assets/embed-code.md).
Om du har lagt till interaktivitet med snabbvyn ska du bara använda inbäddningskoden. Om du har lagt till interaktivitet med hyperlänkade webbsidor kan du även använda den publicerade URL:en. Observera dock att den URL-baserade länkningsmetoden inte är möjlig om ditt interaktiva innehåll har länkar till relativa URL-adresser, särskilt länkar till Experience Manager Sites-sidor.

1. Identifiera var den statiska videon finns i målets webbsideskod.
1. Ta bort den statiska videon och ersätt koden med den inbäddade koden eller URL-adressen som du kopierade från Experience Manager Assets, som den är.
Den kopierade inbäddningskoden ställs in för en responsiv miljö så att den automatiskt passar det område som den statiska videon tidigare upptog.

>[!NOTE]
>
>Nu är du klar om du har lagt till interaktivitet med endast hyperlänkade webbsidor.
>
>Om du däremot har lagt till någon interaktivitet som utlöser en snabbvy visas endast miniatyrbilderna bredvid den interaktiva videon. De är ännu inte integrerade med din befintliga snabbvy. I sådana fall måste du integrera den interaktiva videon med den befintliga snabbvyn på webbplatsen.

**Exempel**

Använda demowebbplatsen som exempel:

[https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-0.html](https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-0.html)

Observera att inbäddningskoden är standard:

```xml
<style type="text/css">
 #s7video_div.s7videoviewer{
   width:100%;
   height:auto;
 }
</style>

<script type="text/javascript" src="https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/js/VideoViewer.js"></script>
<div id="s7video_div"></div>
<script type="text/javascript">
 var s7videoviewer = new s7viewers.VideoViewer({
  "containerId" : "s7video_div",
  "params" : {
   "serverurl" : "https://adobedemo62-h.assetsadobe.com/is/image",
   "contenturl" : "https://demos-pub.assetsadobe.com/",
   "config" : "/etc/dam/presets/viewer/Video",
   "config2": "/etc/dam/presets/analytics",
   "videoserverurl": "https://gateway-na.assetsadobe.com/DMGateway/public/demoCo",
   "posterimage": "/content/dam/marketing/shoppable-video/john-lewis/shoppable-video-john-lewis-2014.mp4",
   "asset" : "/content/dam/marketing/shoppable-video/john-lewis/shoppable-video-john-lewis-2014.mp4" }
 }).init();
</script>
```

Integrationen är lika enkel som att ta bort inbäddningskoden för video och ersätta den med den interaktiva inbäddningskoden för video från Experience Manager. Resultatet visas på följande URL-adress. Även om det visar en interaktiv video på sidan är den ännu inte integrerad med den befintliga snabbvyn:

[https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-1.html](https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-1.html)

## Integrera en interaktiv video med en befintlig QuickView {#integrating-an-interactive-video-with-an-existing-quickview}

>[!NOTE]
>
>Detta gäller endast om du är en fristående Experience Manager Assets-kund.

Det sista steget i den här processen är att integrera din interaktiva video med en befintlig QuickView-implementering som används på din webbplats. Det finns ingen lösning på integreringen som fungerar i alla fall. Alla QuickView-implementeringar är unika. Därför behövs ett specifikt tillvägagångssätt som innebär att IT-personal på frontend får hjälp.

Den befintliga Quickview-implementeringen representerar normalt en kedja av interrelaterade åtgärder som inträffar på webbsidan i följande ordning:

1. En användare utlöser ett element i användargränssnittet för webbplatsen.
1. FrontEnd-koden hämtar en QuickView-URL som baseras på användargränssnittselementet som utlöstes i steg 1.
1. Slutkoden skickar en AJAX-begäran med den URL som du får i steg 2.
1. Bakåtlogiken returnerar motsvarande QuickView-data eller -innehåll tillbaka till slutkoden.
1. Slutkoden läser in QuickView-data eller -innehåll.
1. Om du vill kan du använda koden i gränssnittet för att konvertera inlästa QuickView-data till en HTML-representation.
1. I slutkoden visas en modal dialogruta eller panel och HTML-innehållet återges på skärmen för slutanvändaren.

Dessa anrop representerar inte oberoende offentliga API-anrop som kan anropas av webbsidans logik från ett godtyckligt steg. I stället är det ett kedjat anrop där varje steg döljs i den sista fasen (återanrop) av föregående steg.

Samtidigt som den interaktiva videon ersätter steg 1, och delvis steg 2, när en användare väljer en miniatyrbild i den interaktiva videon, hanteras denna användarinteraktion av användaren. Visningsprogrammet returnerar en händelse på webbsidan som innehåller alla miniatyrbildsdata som tidigare lagts till i Experience Manager.

I en sådan händelsehanterare gör koden längst fram följande:

* Lyssnar på en händelse som skickas av den interaktiva videon.
* Skapar en QuickView-URL som baseras på miniatyrbildsdata.
* Startar processen att läsa in snabbvyn från serverdelen och återge den på skärmen för visning.

Dessutom har Interactive Video Viewer stöd för helskärmsläge. Slutanvändaren utlöser snabbvyn genom att välja en miniatyrbild utan att lämna helskärmsläget. För att uppnå den här funktionen ändrar du frontdelskoden så att den modala dialogrutan för snabbvyn bifogas till visningsprogrammets behållare. Lägg inte till dokument-BODY eller något annat webbsideselement som inte är tillgängligt när visningsprogrammet är i helskärmsläge. Koden som utför det här jobbet måste lyssna på en eller flera återanrop till visningsprogrammet som skickas efter visningsprogrammet som läses in på sidan.

Den inbäddningskod som returneras av Experience Manager har redan en färdig händelsehanterare på plats. Den kommenteras ut enligt följande markerade kodfragment:

```xml
<style type="text/css">
 #s7interactivevideo_div.s7interactivevideoviewer{
   width:100%;
   height:auto;
 }
</style>
<script type="text/javascript" src="https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/js/InteractiveVideoViewer.js"></script>

<div id="s7interactivevideo_div"></div>
<script type="text/javascript">
 var s7interactivevideoviewer = new s7viewers.InteractiveVideoViewer({
  "containerId" : "s7interactivevideo_div",
  "params" : {
   "serverurl" : "https://adobedemo62-h.assetsadobe.com/is/image",
   "contenturl" : "https://demos-pub.assetsadobe.com/",
   "config" : "/etc/dam/presets/viewer/Shoppable_Video_light",
   "config2": "/etc/dam/presets/analytics",
   "videoserverurl": "https://gateway-na.assetsadobe.com/DMGateway/public/demoCo",
   "interactivedata": "content/dam/_VTT/marketing/shoppable-video/john-lewis/shoppable-video-john-lewis-2014.mp4.svideo.vtt",
   "VideoPlayer.contenturl": "https://adobedemo62-h.assetsadobe.com/is/content",
   "asset" : "/content/dam/marketing/shoppable-video/john-lewis/shoppable-video-john-lewis-2014.mp4" }
 })
 /* // Example of interactive video event for quickview.
   s7interactivevideoviewer.setHandlers({
   "quickViewActivate": function(inData) {
     var sku=inData.sku; //SKU for product ID
    //To pass other parameter from the hotspot, you need to add custom parameter during the hotspot setup as parameterName=value
    loadQuickView(sku); //Replace this call with your quickview plugin
    //See your quickviewer plugin for the quickview call
    },
"initComplete":function() {
    //--- Attach quickview popup to viewer container so popup will work in fullscreen mode ---
    var popup = document.getElementById('quickview_div'); // get custom quickview container
    popup.parentNode.removeChild(popup); // remove it from current DOM
    var sdkContainerId = s7interactivevideoviewer.getComponent("container").getInnerContainerId(); // get viewer container component
    var inner_container = document.getElementById(sdkContainerId);
    inner_container.appendChild(popup); //Attach custom quickview container to viewer
    }
   });
 */
 s7interactivevideoviewer.init();
</script>
```

Därför är det bara nödvändigt att avkommentera det markerade kodfragmentet ovan och ersätta dummy-hanterarbrödtexten med kod som är specifik för den aktuella webbsidan.

Det finns två standardhanterare för återanrop i standardkoden för inbäddning: `quickViewActivate` och `initComplete`. Hanteraren `quickViewActivate` aktiveras när en miniatyrbild väljs i visningsprogrammet. Använd det för att integrera visningsprogrammet med QuickView-aktiveringslogik. Hanteraren `initComplete` utlöser bara en gång när visningsprogrammet läses in på sidan. Den här hanteraren används för att justera platsen för dialogrutan Snabbvy i webbsidans DOM.

Processen med att skapa en URL för snabbvyn är motsatt till processen att identifiera miniatyrbildsvariabler som beskrivs tidigare i det här avsnittet. Med hjälp av de tidigare identifierade exemplen på snabbvyns URL kan du se hur snabbvyns URL är uppbyggd i varje fall:

<table>
  <tbody>
  <tr>
    <td><p>En SKU, som finns i frågesträngen</p> </td>
    <td><code class="code">s7interactivevideoviewer.setHandlers(&lbrace;
      "quickViewActivate": function(inData) &lbrace;
      var quickViewUrl = "https://server/json?productId=" + inData.sku + "&amp;source=100";
      &rbrace;,
      &rbrace;);</code></td>
  </tr>
  <tr>
    <td>En SKU, finns i URL-sökvägen</td>
    <td><code class="code">s7interactivevideoviewer.setHandlers(&lbrace;
      "quickViewActivate": function(inData) &lbrace;
      var quickViewUrl = "https://server/product/" + inData.sku;
      &rbrace;,
      &rbrace;);</code></td>
  </tr>
  <tr>
    <td><p>SKU och kategori-ID i frågesträngen</p> </td>
    <td><code class="code">s7interactivevideoviewer.setHandlers(&lbrace;
      "quickViewActivate": function(inData) &lbrace;
      var quickViewUrl = "https://server/quickView/product/?category=" + inData.categoryId + "&amp;prodId=" + inData.sku;
      &rbrace;,
      &rbrace;);</code></td>
  </tr>
  </tbody>
</table>

Det sista steget för att utlösa snabbgransknings-URL:en och aktivera snabbvypanelen kräver troligen hjälp av en IT-handläggare på IT-avdelningen. De har kunskap om hur de bäst kan utlösa QuickView-implementeringen från rätt steg med en färdig QuickView-URL.

Du kan se hur dessa steg tillämpas på demowebbplatsen för att helt integrera en interaktiv video med QuickView-koden. Tidigare i det här avsnittet identifierades strukturen för snabbvyns URL som följande:

```xml
/datafeed/$CategoryId$-$SKU$.json
```

Det är enkelt att rekonstruera den här URL:en i hanteraren `quickViewActivate` genom att använda fälten `categoryId` och `sku` som är tillgängliga i objektet `inData` som skickas till hanteraren via visningsprogrammets kod enligt följande:

```xml
var sku=inData.sku;
var categoryId=inData.categoryId;
var quickViewUrl = "datafeed/" + categoryId + "-" + sku + ".json";
```

Demonstrationswebbplatsen utlöser dialogrutan Snabbvy med ett enkelt `loadQuickView()`-funktionsanrop. Den här funktionen har bara ett argument, vilket är snabbvydata-URL:en. Så det sista steget för att integrera den interaktiva videon är att lägga till följande kodrad i `quickViewActivate`-hanteraren:

```xml
loadQuickView(quickViewUrl);
```

Kontrollera slutligen att dialogrutan Snabbvy är kopplad till visningsprogrammets behållarelement. Den inbäddade koden innehåller exempelsteg för att uppnå den här funktionen. Om du vill få en referens till visningsprogrammets behållarelement kan du använda följande kodrader:

```xml
var sdkContainerId = s7interactivevideoviewer.getComponent("container").getInnerContainerId(); // get viewer container component
var inner_container = document.getElementById(sdkContainerId);
```

Där `inner_container` är en referens till ett `DIV`-element som hanteras av visningsprogrammet. Du vill att dialogrutan ska vara underordnad `DIV`.

Stegen för att hitta det modala dialogruteelementet och bifoga det till behållaren ovan är skiftlägeskänsliga. Återigen kan du få hjälp av den som är bekant med den QuickView-implementering som behövs.

Om du använder exempelwebbplatsen implementeras den modala dialogrutan för snabbvyn som en `DIV` med det spärra/modala ID som bifogas direkt till dokumentet `BODY`. Koden som flyttar dialogrutan till visningsprogrammets behållare är därför så enkel som följande:

```xml
var sdkContainerId = s7interactivevideoviewer.getComponent("container").getInnerContainerId(); // get viewer container component
var inner_container = document.getElementById(sdkContainerId);
inner_container.appendChild(document.getElementById("quickview-modal"));
```

Den fullständiga källkoden är följande:

```xml
<style type="text/css">
 #s7interactivevideo_div.s7interactivevideoviewer{
   width:100%;
   height:auto;
 }
</style>
<script type="text/javascript" src="https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/js/InteractiveVideoViewer.js"></script>

<div id="s7interactivevideo_div"></div>
<script type="text/javascript">
 var s7interactivevideoviewer = new s7viewers.InteractiveVideoViewer({
  "containerId" : "s7interactivevideo_div",
  "params" : {
   "serverurl" : "https://adobedemo62-h.assetsadobe.com/is/image",
   "contenturl" : "https://demos-pub.assetsadobe.com/",
   "config" : "/etc/dam/presets/viewer/Shoppable_Video_light",
   "videoserverurl": "https://gateway-na.assetsadobe.com/DMGateway/public/demoCo",
   "interactivedata": "content/dam/_VTT/marketing/shoppable-video/john-lewis/shoppable-video-john-lewis-2014.mp4.svideo.vtt",
   "VideoPlayer.contenturl": "https://adobedemo62-h.assetsadobe.com/is/content",
   "asset" : "/content/dam/marketing/shoppable-video/john-lewis/shoppable-video-john-lewis-2014.mp4" }
 })
 // Example of interactive video event for quickview.
   s7interactivevideoviewer.setHandlers({
   "quickViewActivate": function(inData) {
     var sku=inData.sku; //SKU for product ID
     var categoryId=inData.categoryId; //categoryId
    var quickViewUrl = "datafeed/" + categoryId + "-" + sku + ".json";
    loadQuickView(quickViewUrl);
    },
   "initComplete":function() {
    //--- Attach quickview popup to viewer container so popup will work in fullscreen mode ---
    var sdkContainerId = s7interactivevideoviewer.getComponent("container").getInnerContainerId(); // get viewer container component
    var inner_container = document.getElementById(sdkContainerId);
    inner_container.appendChild(document.getElementById("quickview-modal"));
    }
   });
 s7interactivevideoviewer.init();
</script>
```

Den färdiga demowebbplatsen med den helt integrerade interaktiva videon ser ut så här:

[https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-3.html](https://experienceleague.adobe.com/tools/dynamic-media-demo/shoppable-video/john-lewis/landing-3.html)

## Skapa anpassade popup-fönster med snabbvyn {#using-quickviews-to-create-custom-pop-ups}

Se [Skapa anpassade popup-fönster med snabbvyn](/help/assets/custom-pop-ups.md).
