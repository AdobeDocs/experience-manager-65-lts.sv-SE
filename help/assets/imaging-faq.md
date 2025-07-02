---
title: Smart bildbehandling
description: Smart Imaging tillämpar varje användares unika visningsegenskaper för att leverera rätt bilder som automatiskt optimerats för sin upplevelse, vilket ger bättre prestanda och engagemang.
contentOwner: Rick Brough
topic-tags: dynamic-media
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
feature: Asset Management,Renditions
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 9f95a54d-6c5e-44c1-965e-631ec7487308
source-git-commit: dc405bec510b0f72e916df343790572b3cd51526
workflow-type: tm+mt
source-wordcount: '3270'
ht-degree: 0%

---

# Smart bildbehandling {#smart-imaging}

Smart Imaging tillämpar varje användares unika visningsegenskaper för att leverera rätt bilder som automatiskt optimerats för sin upplevelse, vilket ger bättre prestanda och engagemang.

## Om Smart Imaging {#what-is-smart-imaging}

Smart Imaging-tekniken tillämpar Adobe Sensei AI-funktioner och fungerar med befintliga&quot;bildförinställningar&quot;. Det förbättrar bildleveransen genom att automatiskt optimera bildformat, storlek och kvalitet baserat på webbläsarens funktioner.

Och nu får du en bättre Google Core Web Vital-poäng för LCP (Störst Contentful Paint) med förbättrad Smart Imaging, som nu har stöd för både AVIF och WebP.

>[!IMPORTANT]
>
>Smart Imaging kräver att du använder det färdiga CDN (Content Delivery Network) som medföljer Adobe Experience Manager - Dynamic Media. Andra anpassade CDN stöds inte med den här funktionen.

>[!TIP]
>
>Testa och upptäck fördelarna med dynamiska mediefärgmodifieringar och smart bildbehandling med Dynamic Media [_Snapshot_](https://snapshot.scene7.com/).
>
>Ögonblicksbild är ett visuellt demonstrationsverktyg som är utformat för att illustrera kraften i Dynamic Media för optimerad och dynamisk bildleverans. Experimentera med testbilder eller dynamiska medie-URL:er för att se utdata från olika dynamiska mediabildmodifierare visuellt och optimera smart bildhantering för följande:
>
>* Filstorlek (med WebP- och AVIF-leverans)
>* Nätverksbandbredd
>* DPR (Device Pixel Ratio)
>
>Om du vill lära dig hur enkelt det är att använda ögonblicksbild kan du spela upp utbildningsvideon [för ögonblicksbild](https://experienceleague.adobe.com/sv/docs/experience-manager-learn/assets/dynamic-media/images/dynamic-media-snapshot) (3 minuter och 17 sekunder).

Smart Imaging drar nytta av den ökade prestandaförbättringen genom att vara helt integrerad med Adobe förstklassiga CDN-tjänst (Content Delivery Network). Den här tjänsten hittar den optimala Internet-vägen mellan servrar, nätverk och peering-punkter. Här hittas en väg som har lägst latens och lägst paketförlustfrekvens i stället för att använda standardvägen på Internet.

I följande exempel på bildobjekt visas den nya optimeringen av smarta bilder:

| Bild (URL) | Miniatyrbild | Storlek (JPEG) | Storlek (WebP) med smart bildbehandling | Storlek (AVIF) med smart bildbehandling | % minskning med WebP | % minskning med AVIF |
|---|---|---|---|---|---|---|
| [Bild 1](https://techsupport.scene7.com/is/image/TechSupport/SmartImaging_6?hei=500&fmt=jpg&qlt=85&resmode=bisharp&op_usm=5,0.125,5,0) | ![bild1](/help/assets/assets-dm/picture1.png) | 145 kB | 106 kB | 90,2 kB | 26,89 % | 37,79 % |
| [Bild 2](https://techsupport.scene7.com/is/image/TechSupport/SmartImaging_3?hei=500&fmt=jpg&qlt=85&resmode=bisharp&op_usm=5,0.125,5,0) | ![bild2](/help/assets/assets-dm/picture2.png) | 412 kB | 346 kB | 113 kB | 16,01 % | 72,57 % |
| [Bild 3](https://techsupport.scene7.com/is/image/TechSupport/SmartImaging_2?hei=500&fmt=jpg&qlt=85&resmode=bisharp&op_usm=5,0.125,5,0) | ![bild3](/help/assets/assets-dm/picture3.png) | 221 kB | 189 kB | 87,1 kB | 14,47 % | 60,58 % |
| [Bild 4](https://techsupport.scene7.com/is/image/TechSupport/SmartImaging_1?hei=500&qlt=85&resmode=bisharp&op_usm=5,0.125,5,0) | ![bild4](/help/assets/assets-dm/picture4.png) | 594 kB | 545 kB | 286 kB | 8,25 % | 51,85 % |

På samma sätt som ovanstående har Adobe även kört ett test med en större exempeluppsättning. Formatet AVIF gav 20 % extra storleksminskning jämfört med WebP, vilket gav en 27-procentig minskning jämfört med JPEG. Allt med samma visuella kvalitet. Totalt ger AVIF upp till 41 % genomsnittlig storleksminskning jämfört med JPEG.

Jämför WebP och AVIF med PNG, du kan se en storleksminskning på 84 % med WebP och 87 % med AVIF. Och eftersom både WebP- och AVIF-formaten har stöd för genomskinlighet och flera bildanimeringar är det en bra ersättning för genomskinliga PNG- och GIF-filer.

Se även [Bildoptimering med Next-gen Image Formats (WebP och AVIF)](https://blog.developer.adobe.com/image-optimisation-with-next-gen-image-formats-webp-and-avif-248c75afacc4)

<!-- HIDDEN ON MAY 19, 2022 BASED ON CQDOC-19280 On the mobile web, the challenges are compounded by two factors:

* Large variety of devices with different form factors and high-resolution displays.
* Constrained network bandwidth.

In terms of images, the goal is to serve the best quality images as efficiently as possible. -->

## Fördelar med Smart Imaging {#what-are-the-key-benefits-of-smart-imaging}

Smart Imaging förbättrar bildhanteringen genom att automatiskt optimera filstorleken baserat på användarens webbläsare, enhetsvisning och nätverksförhållanden. Den här metoden ger snabbare inläsning och en bättre visningsupplevelse i olika miljöer. Eftersom bilder utgör större delen av en sidas laddningstid kan prestandaförbättringar få en genomgripande effekt på nyckeltal som följande:

* Högre konverteringsgrader.
* Tid som tillbringats på en webbplats.
* Lägre avhoppsfrekvens för webbplatser.

De nyaste fördelarna med den senaste Smart Imaging är följande:

* Stöd för nästa generations AVIF-format.
* PNG till WebP och AVIF har nu stöd för förlustkonvertering. Eftersom PNG är ett icke-förstörande format levererades tidigare WebP och AVIF utan dataförlust.
* Konvertering av webbläsarformat (`bfc`)
* Enhetens pixelproportioner (`dpr`)
* Nätverksbandbredd (`network`)

### Om konvertering av webbläsarformat (bfc) {#bfc}

Om du aktiverar konvertering av webbläsarformat genom att lägga till `bfc=on` till bild-URL:en konverteras automatiskt JPEG och PNG till AVIF, förstörande WebP, förstörande JPEGXR, förstörande JPEG2000 för olika webbläsare. För webbläsare som inte stöder dessa format fortsätter Smart Imaging att fungera som JPEG eller PNG. Smart bildbehandling beräknar om kvaliteten på det nya formatet tillsammans med formatändringen.

Du kan inaktivera Smart Imaging genom att lägga till `bfc=off` till bildens URL.

Se även [bfc](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/command-reference/r-bfc) i API:t för dynamisk mediabildsserver och återgivning.

### Om optimering av Device Pixel Ratio (dpr) {#dpr}

Enhetens pixelproportioner (DPR), som även kallas CSS-pixelproportioner, representerar relationen mellan en enhets fysiska pixlar och logiska pixlar. I och med uppkomsten av retina-skärmar har pixelupplösningen i moderna mobila enheter ökat snabbt.

Om du aktiverar optimering av enhetspixelproportioner återges bilden med skärmens ursprungliga upplösning, vilket gör den skarp.

För närvarande kommer bildskärmens pixeldensitet från Akamai CDN-rubrikvärden.

| Tillåtna värden i en bilds URL | Beskrivning |
|---|---|
| `dpr=off` | Inaktivera DPR-optimering på URL-nivå för en enskild bild. |
| `dpr=on,dprValue` | Åsidosätt det DPR-värde som identifieras av Smart Imaging, med ett anpassat värde (som identifieras av någon klientlogik eller annan metod). Tillåtet värde för `dprValue` är ett tal som är större än 0. |

>[!NOTE]
>
>* Du kan använda `dpr=on,dprValue` även om DPR-inställningen på företagsnivå är inaktiverad.
>* På grund av DPR-optimering identifieras alltid MaxPix-bredden när den resulterande bilden är större än inställningen för dynamiska medier i MaxPix genom att bildens proportioner behålls.

| Begärd bildstorlek | Enhetspixelproportioner (dpr) | Levererad bildstorlek |
|---|---|---|
| 816 x 500 | 1 | 816 x 500 |
| 816 x 500 | 2 | 1632 x 1000 |

Se även [När du arbetar med bilder](/help/assets/adding-dynamic-media-assets-to-pages.md#when-working-with-images) och [När du arbetar med smart beskärning](/help/assets/adding-dynamic-media-assets-to-pages.md#when-working-with-smart-crop).

### Om optimering av nätverksbandbredd {#network}

När du aktiverar nätverksbandbredden justeras automatiskt bildkvaliteten baserat på den faktiska nätverksbandbredden. För dålig nätverksbandbredd inaktiveras DPR-optimering (Device Pixel Ratio) automatiskt, även om den redan är aktiverad.

Ditt företag kan inaktivera optimering av nätverksbandbredd för enskilda bilder genom att lägga till `network=off` till bild-URL:en.

| Tillåtet värde i URL:en för en bild | Beskrivning |
|---|---|
| `network=off` | Stänger av nätverksoptimering på URL-nivå för en enskild bild. |

DPR- och nätverksbandbreddsvärdena baseras på de värden som identifierats på klientsidan för det paketerade CDN. Dessa värden är ibland felaktiga. IPhone5 med DPR=2 och iPhone12 med `dpr=3` visar till exempel `dpr=2`. För högupplösta enheter är det ändå bättre att skicka `dpr=2` än att skicka `dpr=1`. Det bästa sättet att överbrygga denna brist är dock att använda DPR på klientsidan för att ge er 100 % korrekta värden. Och det fungerar för alla enheter, oavsett om det är Apple eller någon annan enhet som startades. Se [Använd smart bildbehandling med enhetspixelproportioner på klientsidan](/help/assets/client-side-dpr.md).

### Ytterligare viktiga fördelar med smart bildbehandling

* Förbättrad Google SEO-rankning för webbsidor som använder den senaste Smart Imaging-funktionen.
* Serverar optimerat innehåll direkt (vid körning).
* Använder Adobe Sensei-teknik för konvertering enligt den kvalitet (`qlt`) som anges i bildbegäran.
* TTL-oberoende (Time To Live). Tidigare var en minsta TTL på 12 timmar obligatorisk för att Smart Imaging skulle fungera.
* Tidigare cachelagrades både original- och variantbilderna, och det var en tvåstegsprocess att ogiltigförklara cachen. I den senaste versionen av Smart Imaging cachelagras bara derivat, vilket möjliggör en cacheogiltigförklaring i ett enda steg.
* Kunder som använder anpassade rubriker i sina regeluppsättningar kan dra nytta av den senaste smarta bildhanteringen eftersom dessa rubriker inte blockeras, till skillnad från den tidigare versionen av Smart Imaging. Exempel:&quot;Timing Allow Origin&quot; och&quot;X-Robot&quot;.

## Frågor och svar

+++Är det några licenskostnader kopplade till Smart Imaging?

Nej. Smart Imaging ingår i din befintliga licens. Den här regeln gäller antingen Dynamic Media Classic eller Experience Manager - Dynamic Media (On-prem, AMS och Experience Manager as a Cloud Service).

>[!NOTE]
>
>Smart Imaging är inte tillgängligt för kunder med Dynamic Media - Hybrid.

+++

+++Hur fungerar Smart Imaging?

När en konsument begär en bild analyserar Smart Imaging användarens egenskaper och konverterar den till lämpligt format baserat på webbläsaren. Dessa formatkonverteringar görs på ett sätt som inte försämrar den visuella återgivningen. Smart bildbehandling konverterar automatiskt bilder till olika format baserat på webbläsarkapacitet på följande sätt.

* Konvertera automatiskt till AVIF om webbläsaren stöder formatet
* Konvertera automatiskt till WebP om AVIF-konvertering inte är fördelaktig eller om webbläsaren inte stöder AVIF
* Konvertera automatiskt till JPEG2000 om Safari inte stöder WebP
* Konvertera automatiskt till JPEGXR för IE 9+ eller om Edge inte stöder WebP

  | Bildformat | Webbläsare som stöds |
  |---|---|
  | AVIF | [https://caniuse.com/avif](https://caniuse.com/avif) |
  | WebP | [https://caniuse.com/webp](https://caniuse.com/webp) |
  | JPEG 2000 | [https://caniuse.com/jpeg2000](https://caniuse.com/jpeg2000) |
  | JPEGXR | [https://caniuse.com/jpegxr](https://caniuse.com/jpegxr) |

* För webbläsare som inte stöder dessa format används det bildformat som ursprungligen begärdes.

Om den ursprungliga bildstorleken är mindre än vad Smart Imaging skapar, behålls originalbilden.

+++

+++Vilka bildformat stöds?

Följande bildformat stöds för Smart Imaging:

* JPEG
* PNG

Med Smart Imaging beräknas kvaliteten på JPEG bildfilformat om vid konvertering till ett nytt format.

För bildfilsformat som stöder genomskinlighet som PNG kan du konfigurera Smart Imaging så att AVIF och WebP blir förstörande. För konvertering av förlustgivande format använder Smart Imaging den kvalitet som anges i bildens URL, eller i annat fall den kvalitet som konfigurerats i Dynamic Media-företagskontot.

+++

+++Hur fungerar Smart Imaging med befintliga bildförinställningar som redan används?

Smart Imaging integreras smidigt med dina befintliga bildförinställningar och alla bildinställningar respekteras.

De enda justeringarna är bildformatet, kvaliteten eller båda. Vid formatkonvertering bevarar Smart Imaging den fullständiga visuella återgivningen enligt dina förinställda inställningar, men ger en mindre filstorlek. Aktivera det bara genom att lägga till `bfc=on`, `dpr=on,dprValue`, `network=on` eller alla tre parameterinställningarna till dina befintliga URL:er eller förinställningar.

Låt oss säga att en bildförinställning anger ett JPEG-format som är 500 × 500 pixlar, med `quality=85` och `unsharp mask=0.1,1,5`. Smart Imaging identifierar om användaren använder en webbläsare i Chrome. Sedan konverteras bilden till WebP med samma dimensioner (500 × 500) och en oskarp mask som matchar JPEG-inställningarna. Systemet jämför sedan filstorlekarna för WebP- och JPEG-versionerna och skickar den mindre till användaren.

+++

<!--

### Do I have to change any URLs, image presets, or deploy any new code on my site for Smart Imaging? 

No. Smart Imaging works seamlessly with your existing image URLs and image presets. In addition, Smart Imaging does not require you to add code to your website to detect a user's browser. All of this functionality is handled automatically.

<!-- Smart Imaging works seamlessly with your existing image URLs and image presets if you configure Smart Imaging on your existing custom domain. In addition, Smart Imaging does not require you to add any code on your website to detect a user's browser. It is all handled automatically.

In case you must configure a new custom domain to use Smart Imaging, the URLs must be updated to reflect this custom domain.

To understand pre-requisites for Smart Imaging, see [Am I eligible to use Smart Imaging?](#am-i-eligible-to-use-smart-imaging) -->

<!-- OLD As mentioned earlier, Smart Imaging supports only JPEG and PNG image formats. For other formats, you need to append the `bfc=off` modifier to the URL as described earlier. -->

+++Fungerar Smart Imaging med HTTPS? Vad sägs om HTTP/2?

Smart Imaging fungerar med bilder som levereras via HTTP eller HTTPS. Dessutom fungerar det även över HTTP/2.

+++

+++Är jag berättigad att använda Smart Imaging?

Smart Imaging är tillgängligt direkt för alla kunder. Om du vill börja dra nytta av fördelarna lägger du bara till `bfc=on`, `dpr=on,dprValue`, `network=on` eller alla tre parameterinställningarna i dina befintliga URL:er eller förinställningar.

Om du vill använda Smart Imaging måste ditt företags Dynamic Media Classic- eller Dynamic Media-konto på Experience Manager inkludera Adobe paketerade CDN (Content Delivery Network) som en del av licensen.

+++

+++Hur aktiverar jag Smart Imaging för ett konto?

Om du vill börja använda Smart Imaging lägger du till `bfc=on`, `dpr=on,dprValue`, `network=on` eller alla tre parameterinställningarna till dina befintliga URL:er eller förinställningar. Om du inte vill göra dessa ändringar manuellt kan du aktivera Smart Imaging som standard genom att skapa ett supportärende.

När du skapar ett supportärende anger du vilka smarta bildredigeringsfunktioner du vill aktivera på ditt konto:

* Konvertering av webbläsarformat (WebP eller AVIF)
* Optimering av nätverksbandbredd

>[!NOTE]
>
>DPR kräver klientjusteringar för att fastställa rätt `dprValue`. Adobe rekommenderar därför att DPR aktiveras via URL:er genom att lägga till `dpr=on,dprValue`.

**Så här skapar du ett supportärende för att aktivera Smart Imaging för ditt konto:**

1. [Använd Admin Console för att börja skapa ett nytt supportärende](https://helpx.adobe.com/se/enterprise/using/support-for-experience-cloud.html).
1. Ange följande information i ditt supportärende:

   * **Information om primär kontakt:**

      * Ange namn, e-postadress och telefonnummer.

   * **Smarta bildredigeringsfunktioner som ska aktiveras:**

      * Visa en lista över de funktioner du vill använda för ditt konto:

         * Konvertering av webbläsarformat: WebP eller AVIF
         * Optimering av nätverksbandbredd
         * DPR: DPR kräver klientjusteringar för att fastställa rätt `dprValue`. Adobe rekommenderar därför att DPR aktiveras via URL:er genom att lägga till `dpr=on,dprValue`.

   * **Domän för smart bildbehandling:**

      * Visa alla relevanta domäner, till exempel *`company.com`* eller *`mycompany.scene7.com`*
      * Smart Imaging stöder både generiska och anpassade domäner.
      * Identifiera dina domäner genom att öppna [Dynamic Media Classic-datorprogrammet](https://experienceleague.adobe.com/sv/docs/dynamic-media-classic/using/getting-started/signing-out#getting-started) och logga in på ditt företagskonto.

         1. Navigera till **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL General Settings]**.
         1. Kontrollera domänen genom att leta efter fältet **[!UICONTROL Published Server Name]**.
         1. Kontrollera att du använder Adobe CDN i stället för en som hanteras av en annan leverantör.

   * **Ange HTTP/2-stöd:**

      * Ange om du behöver Smart Imaging för att kunna arbeta över HTTP/2.

1. Adobe kundsupport aktiverar de begärda funktionerna för smart bildbehandling som standard, vilket eliminerar behovet av att lägga till parametrar manuellt till URL-adresser.
1. Adobe rekommenderar att TTL (Time To Live) ställs in på minst 24 timmar för att maximera prestanda genom cachning.
Justera TTL:

   1. **För Dynamic Media Classic:**
      1. Navigera till **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL Publish Setup]** > **[!UICONTROL Image Server]**.
      1. Ange värdet **[!UICONTROL Default Client Cache Time To Live]** till 24 timmar eller mer.
   1. **För dynamiska media på Adobe Experience Manager:**
      1. Följ [dessa instruktioner](/help/assets/dm-publish-settings.md#common-thumbnail-attributes-tab).
      1. Ange värdet **[!UICONTROL Expiration]** för 24 timmar eller mer.

+++

+++När kan jag förvänta mig att ett konto ska aktiveras med Smart Imaging?

Kundsupport bearbetar förfrågningar i den ordning de får dem, efter väntelistan.

>[!NOTE]
>
>Det kan ta lång tid att skapa eftersom Adobe rensar cachen när du aktiverar Smart Imaging. Därför kan bara ett fåtal kundövergångar hanteras vid en viss tidpunkt.

+++

+++Vilka är riskerna med att byta till Smart Imaging?

Det finns ingen risk för kundens webbsida. Övergången till Smart Imaging tar dock bort CDN-cachen. Den här åtgärden innebär att du måste gå till en ny konfiguration av Dynamic Media Classic eller Dynamic Media på Experience Manager.

Under den inledande övergången kommer de icke-cachelagrade bilderna att direkt nå Adobe ursprungsservrar tills cachen återskapas. Därför planerar Adobe att hantera ett fåtal kundövergångar i taget så att godtagbara prestanda upprätthålls när man tar begäranden från ursprungsläget. För de flesta kunder är cacheminnet helt uppbyggt igen på CDN inom cirka 1-2 dagar.

+++

+++Hur verifierar jag om Smart Imaging fungerar som väntat?

1. När ditt konto har konfigurerats med Smart Imaging läser du in en Dynamic Media Classic- eller Adobe Experience Manager - Dynamic Media-bild-URL i webbläsaren.
1. Öppna Chrome-utvecklarfönstret genom att gå till **[!UICONTROL View]** > **[!UICONTROL Developer]** > **[!UICONTROL Developer Tools]** i webbläsaren. Eller välj ett valfritt verktyg för webbläsare.

1. Kontrollera att cachen är inaktiverad när utvecklingsverktygen är öppna.

   * I Windows® går du till inställningarna i rutan för utvecklarverktyget och markerar kryssrutan **[!UICONTROL Disable cache (while devtools is open)]**.
   * I macOS väljer du **[!UICONTROL Network]** på fliken **[!UICONTROL disable cache]** i rutan Utvecklare.

1. Observera att innehållstypen har omvandlats till lämpligt format. På följande skärmbild visas en PNG-bild som konverteras dynamiskt till WebP på Chrome. Om din domän har AVIF aktiverat kan du även förvänta dig att se AVIF i innehållstypen.
1. Upprepa testet i olika webbläsare och under olika användarförhållanden.

>[!NOTE]
>
>Alla bilder konverteras inte. Smart Imaging avgör om konverteringen kan förbättra prestandan. Ibland konverteras bilden inte om det inte finns någon förväntad prestandaökning eller om formatet inte är JPEG eller PNG.

![image2017-11-14_15398](/help/assets/assets/image2017-11-14_15398.png)

+++

+++Hur vet jag om prestandavinsten? Finns det något sätt att lära sig fördelarna med Smart bildbehandling?

Huvudet Smart Imaging avgör fördelarna med Smart Imaging. När Smart Imaging är aktiverat kan du se **[!UICONTROL Response Headers]** efter att du har begärt en bild, under rubriken `-X-Adobe-Smart-Imaging`, enligt följande markerade exempel:

![Rubrik för smart bildåtergivning](/help/assets/assets-dm/smart-imaging-header2.png)

Den här rubriken innehåller följande information:

* Smart Imaging fungerar för företaget.
* Ett positivt värde innebär att konverteringen lyckas. I det här fallet returneras en ny WebP-bild.
* Ett negativt värde innebär att konverteringen inte lyckas. I så fall returneras den ursprungliga begärda bilden (JPEG som standard, om inget anges).
* Ett positivt värde visar skillnaden i byte mellan den begärda bilden och den nya bilden. I exemplet ovan är de sparade byten `75048` eller ungefär 75 kB för en bild.
* Ett negativt värde innebär att den begärda bilden är mindre än den nya bilden. Den negativa storleksskillnaden visas, men bilden som visas är endast den ursprungliga begärda bilden.

>[!NOTE]
>
>**X-Adobe-Smart-Imaging = -1 när WebP levereras**
>
>Om värdet för `X-Adobe-Smart-Imaging` är -1 och WebP fortfarande levereras är Smart Imaging aktivt. Storleksfördelarna beräknades dock inte på grund av inaktuell cache. Du kan använda `cache=update` (endast en gång) i bildens URL för att åtgärda problemet.
>&#x200B;>Ett exempel på hur du använder modifieraren:
>&#x200B;>`https://smartimaging.scene7.com/is/image/SmartImaging/sample1?cache=update`
>&#x200B;>Om du vill göra hela cachen ogiltig måste du skapa ett supportärende.

+++

+++Hur inaktiverar jag AVIF-optimering i Smart Imaging?

Om du vill växla tillbaka till att använda WebP som standard skapar du ett supportärende för det. Som vanligt kan du inaktivera Smart Imaging genom att lägga till parametern `bfc=off` i bildens URL. Du kan dock inte välja WebP eller AVIF i URL-modifieraren för Smart Imaging. Den här möjligheten bibehålls på kontonivån.

+++

+++Kan Smart Imaging stängas av på begäran?

Ja. Du kan inaktivera Smart bildbehandling genom att lägga till någon av följande modifierare:

* `bfc=off` om du vill inaktivera konvertering av webbläsarformat. Se även [Konvertering av webbläsarformat](#bfc).
* `dpr=off` om du vill inaktivera Device Pixel Ratio. Se även [Enhetens pixelproportioner](#dpr).
* `network=off` om du vill inaktivera nätverksbandbredd. Se även [Nätverksbandbredd](#network).

+++

+++Vilken &quot;justering&quot; är tillgänglig? Finns det några inställningar eller beteenden som kan definieras?

Smart bildbehandling har tre alternativ som du kan aktivera eller inaktivera.

* [Konvertering av webbläsarformat](#bfc)
* [Enhetens pixelproportioner](#dpr)
* [Nätverksbandbredd](#network)

+++

+++I har en URL med fmt=tif i Chrome webbläsare. Men min begäran misslyckas med ett ImageServer-fel. Varför?

Det här felet inträffar inte om Smart Imaging inte är aktiverat på ditt konto. Smart Imaging fungerar endast med JPEG- eller PNG-format.

För att undvika det här felet kan du antingen:

* Ange JPEG eller PNG, eller
* inte använder modifieraren `fmt` alls, eller
* Använd ett webbläsarformat som definieras av Smart Imaging. Du kan till exempel använda WebP för Chrome webbläsare.

+++

+++Jag vill hämta en TIFF-bild från en bilds URL. Hur ska jag göra det?

Lägg till `fmt=tif` och `bfc=off` i bildens URL-sökväg.

+++

+++Hanterar Smart Imaging bara bildformatet eller hanterar det även bildkvalitetsinställningarna för bästa resultat?

Smart bildbehandling använder både format och kvalitet. Resten av parametrarna förblir desamma, om det krävs i bildens URL.

+++

+++Om Smart Imaging hanterar kvalitetsinställningarna, finns det några minimi- och maximumvärden jag kan ange? Med andra ord, en kvalitet som inte är lägre än 60 och inte större än 80?

Det finns för närvarande ingen sådan etablering.

+++

+++Justerar Smart Imaging automatiskt procentkvalitetsinställningen eller är det en inställning som justeras manuellt och gäller för alla bilder? Inom vilket intervall?

Smart Imaging justerar automatiskt kvalitetsprocenten. Kvaliteten bestäms med en maskininlärningsalgoritm som utvecklats av Adobe. Den här procentandelen är inte intervallspecifik.

+++

+++Med Smart Imaging, vilka bildserverkommandon stöds eller ignoreras?

De enda kommandon som ignoreras är `fmt` och `qlt`. Alla återstående kommandon stöds.

+++

+++Är bara JPEG-bilder ersatta med Smart Imaging? Vad händer om jag begär en WebP, PNG eller något annat?

Den här funktionen fungerar endast för JPEG och PNG.

+++

+++Varför returneras en JPEG-bild till Chrome istället för till WebP?

Smart bildbehandling avgör om konverteringen är bra eller inte. Den returnerar bara den nya bilden för konverteringen.

+++

+++Varför fungerar inte funktionen Device Pixel Ratio (dpr) som förväntat för sammansatta bilder?

Om en sammansatt bild innehåller för många lager kan dpr-funktionen påverkas när du använder en positionsmodifierare. Problemet är känt och bör åtgärdas i framtida versioner av Smart Imaging. Om andra Smart Imaging-funktioner inte fungerar som förväntat kan du skapa ett supportärende för att rapportera problemet.

+++

+++Varför konverteras Smart Imaging PNG fortfarande till förlustfri WebP/AVIF?

Eftersom PNG är ett förlustfritt format var tidigare WebP och AVIF levererade utan dataförlust, vilket ger större storlek än förväntat. Smart Imaging har nu stöd för förlustkonvertering. Du kan använda modifieraren `cache=update` (endast en gång) i en bildbegäran för att åtgärda problemet. Ett exempel på hur den här modifieraren används:

`https://smartimaging.scene7.com/is/image/SmartImaging/sample1?cache=update`

Om du vill göra hela cacheminnet ogiltigt måste du skapa ett supportärende och begära en sådan åtgärd.

+++

+++Hur kan jag fortsätta använda PNG för förlustfri konvertering i Smart Imaging?

Smart Imaging har nu stöd för förlustkonvertering baserat på kvalitetsnivån. Du kan fortsätta använda förlustfri konvertering genom att ange kvaliteten till 100, antingen genom ditt företags inställningar eller genom att lägga till `qlt=100` i bildens URL-sökväg.

+++
