---
title: Bästa tillvägagångssätt för att optimera kvaliteten på dina bilder i Dynamic Media
description: Lär dig de bästa sätten att optimera bildkvaliteten i Dynamic Media
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
feature: Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 30038003-e307-46d1-b5f9-624d98a672a7
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 3%

---

# Bästa tillvägagångssätt för att optimera kvaliteten på dina bilder i Dynamic Media {#best-practices-for-optimizing-the-quality-of-your-images}

Att optimera bildkvaliteten kan vara en tidskrävande process eftersom många faktorer bidrar till att återge godtagbara resultat. Resultatet är delvis subjektivt eftersom individer upplever olika bildkvalitet. Strukturerade experiment är avgörande.

Adobe Experience Manager innehåller över 100 kommandon för leverans av dynamiska bilder för justering och optimering av bilder och återgivning. Följande riktlinjer kan hjälpa dig att effektivisera processen och uppnå goda resultat snabbt med några viktiga kommandon och bästa metoder.

## Bästa tillvägagångssätt för bildformat (`&fmt=`) {#best-practices-for-image-format-fmt}

* JPG eller PNG är de bästa sätten att leverera bilder med god kvalitet och hanterbar storlek och vikt.
* Om inget formatkommando anges i URL:en används JPG som standard för leverans av dynamiska bilder.
* JPG komprimerar 10:1 och ger vanligtvis mindre bildfilsstorlekar. PNG komprimeras med ett förhållande på cirka 2:1, förutom ibland när bilder innehåller en vit bakgrund. Vanligtvis är PNG-filer större än JPG-filer.
* JPG använder förstörande komprimering, vilket innebär att bildelement (pixlar) tas bort under komprimeringen. PNG använder däremot förlustfri komprimering.
* JPG komprimerar ofta fotografiska bilder med bättre återgivning än syntetiska bilder med skarpa kanter och kontrast.
* Om dina bilder innehåller genomskinlighet bör du använda PNG eftersom JPG inte stöder genomskinlighet.

Ett tips för bildformat är att börja med den vanligaste inställningen `&fmt=JPG`.

## Bästa tillvägagångssätt för bildstorlek {#best-practices-for-image-size}

Att minska bildstorleken dynamiskt är en av de vanligaste uppgifterna. Det handlar om att ange storleken och, om så önskas, vilket nedsamplingsläge som används för att nedskala bilden.

* För storleksändring av bilder är det bästa och enklaste sättet att använda `&wid=<value>` och `&hei=<value>,` eller bara `&hei=<value>`. Dessa parametrar ställer automatiskt in bildbredden i enlighet med proportionerna.
* `&resMode=<value>`styr den algoritm som används för nedsampling. Börja med `&resMode=sharp2`. Det här värdet ger den bästa bildkvaliteten. Nedsamplingen `value =bilin` är snabbare, men leder ofta till aliasing av artefakter.

Använd `&wid=<value>&hei=<value>&resMode=sharp2` eller `&hei=<value>&resMode=sharp2` om du vill ändra bildstorlek

## Bästa tillvägagångssätt för bildskärpa {#best-practices-for-image-sharpening}

Bildskärpa är den mest komplicerade aspekten när det gäller att styra bilder på webbplatsen och var många misstag görs. Ta dig tid att lära dig mer om hur skärpa och oskarp maskning fungerar i Experience Manager med hjälp av följande resurser:

Best practices white paper [Öka skärpan i bilder i Adobe Dynamic Media Classic](/help/assets/assets/sharpening_images.pdf) som även gäller för Experience Manager.

<!-- To be reviewed and updated: Broken link.
See also [Sharpening an image with unsharp mask](https://helpx.adobe.com/photoshop/atv/cs6-tutorials/sharpening-an-image-with-unsharp-mask.html). -->

Med Experience Manager kan du öka skärpan i bilder vid intag, vid leverans eller både och. Vanligtvis kan du skärpa upp bilder med bara en metod eller med en annan, men inte med båda metoderna. Att skärpa bilderna vid leverans, på en URL-adress, ger oftast bäst resultat.

Det finns två metoder för bildskärpa:

* Enkel skärpa ( `&op_sharpen`) - Precis som skärpefiltret som används i Photoshop, tillämpar enkel skärpa grundläggande skärpa på den slutliga vyn av bilden efter den dynamiska storleksändringen. Den här metoden kan dock inte konfigureras av användaren. Det bästa sättet är att inte använda &amp;op_sharpen om det inte behövs.
* Oskarp maskering ( `&op_USM`) - Oskarp maskering är ett skärpefilter som är branschstandard. Det bästa sättet är att göra bilder skarpare med oskarp maskering enligt riktlinjerna nedan. Med Oskarp maskning kan du styra följande tre parametrar:

   * `&op_sharpen=amount,radius,threshold`

      * **[!UICONTROL *mängd *]**(0-5, effektens styrka.)
      * **[!UICONTROL *radius *]**(0-250, bredden på de&quot;skärpelinjer&quot; som ritas runt det skarpa objektet, mätt i pixlar.)

     Tänk på att parametrarnas radie och mängd fungerar mot varandra. Reducerad radie kan kompenseras genom ett ökat belopp. Med radie får du bättre kontroll eftersom ett lägre värde ökar skärpan endast för kantpixlarna, medan ett högre värde ökar skärpan för ett större antal pixlar.

      * **[!UICONTROL *threshold *]**(0-255, effektkänslighet.)

            Den här parametern avgör hur annorlunda de pixlar som ska göras skarpare måste vara jämfört med det omgivande området innan de betraktas som kantpixlar och filtret gör dem skarpare. Parametern **[!UICONTROL threshold]** hjälper dig att undvika att göra områden med liknande färger för mycket skarpare, till exempel hudtoner. Ett tröskelvärde på 12 ignorerar till exempel små variationer i hudtonens ljusstyrka för att undvika att lägga till ”brus”, men lägger ändå till kantkontrast i områden med hög kontrast, till exempel där ögonfransarna möter huden.
        
        Mer information om hur du ställer in de här tre parametrarna, inklusive de bästa sätten att använda med filtret, finns i följande resurser:

        Experience Manager hjälpavsnitt om skärpa i en bild.

        Best practices white paper [Öka skärpan i bilder i Adobe Dynamic Media Classic](/help/assets/assets/sharpening_images.pdf).

      * I Experience Manager kan du även styra en fjärde parameter: monokrom (0,1). Den här parametern avgör om oskarp maskning används separat på varje färgkomponent med värdet 0 eller på bildens intensitet/intensitet med värdet 1.

Ett tips är att börja med parametern unsharp mask radius. Radie-inställningar som du kan börja med är följande:

* **[!UICONTROL Website]** - 0,2-0,3 pixlar
* **[!UICONTROL Photographic printing (250-300 ppi)]** - 0,3-0,5 pixlar
* **[!UICONTROL Offset printing (266-300 ppi)]** - 0,7-1,0 pixlar
* **[!UICONTROL Canvas printing (150 ppi)]** - 1,5-2,0 pixlar

Öka mängden gradvis från 1,75 till 4. Om skärpan fortfarande inte är som du vill ha den ökar du radien med ett decimalkomma och kör mängden igen från 1,75 till 4. Upprepa vid behov.

Lämna den monokroma parameterinställningen på 0.

### Metodtips för JPEG-komprimering (`&qlt=`) {#best-practices-for-jpeg-compression-qlt}

* Den här parametern styr kodningskvaliteten i JPG. Ett högre värde innebär en bild med högre kvalitet men en stor filstorlek. Ett lägre värde innebär en bild med lägre kvalitet men mindre filstorlek. Intervallet för parametern är 0-100.
* Om du vill optimera kvaliteten ska du inte ange parametervärdet 100. Skillnaden mellan en inställning på 90 eller 95 och 100 är nästan otydlig, men 100 ökar storleken på bildfilen i onödan. Om du vill optimera för kvaliteten men undvika att bildfilerna blir för stora anger du därför `qlt= value` till 90 eller 95.
* Om du vill optimera för en liten bildfilsstorlek men behålla bildkvaliteten på en acceptabel nivå anger du `qlt= value` till 80. Värden under 70 till 75 ger en signifikant försämring av bildkvaliteten.
* För att vara i mitten bör du ange `qlt= value` till 85 för att förbli i mitten.
* Använda kroma-flaggan i `qlt=`

   * Parametern `qlt=` har en andra inställning som gör att du kan aktivera nedsampling av färgvärden i RGB med värdet `,1` eller inaktiverat med värdet `,0`.
   * Börja med att RGB nedsampling av kromaticitet är inaktiverat (`,0`) för att göra det enkelt. Den här inställningen ger vanligtvis bättre bildkvalitet, särskilt för syntetiska bilder med många skarpa kanter och kontrast.

`&qlt=85,0` bör användas som en bra metod för JPG-komprimering.

## Metodtips för JPEG-storleksändring (`&jpegSize=`) {#best-practices-for-jpeg-sizing-jpegsize}

jpegSize är en användbar parameter om du vill försäkra dig om att en bild inte överskrider en viss storlek för leverans till enheter som har begränsat minne.

* Den här parametern anges i kilobyte (`jpegSize=&lt;size_in_kilobytes&gt;`). Det definierar den största tillåtna storleken för bildleverans.
* `&jpegSize=` interagerar med JPG-komprimeringsparametern `&qlt=`. Om JPG-svaret med den angivna JPG-komprimeringsparametern (`&qlt=`) inte överskrider jpegSize-värdet returneras bilden med `&qlt=` enligt definitionen. Annars minskas `&qlt=` gradvis tills bilden får plats i den tillåtna maxstorleken, eller tills systemet fastställer att den inte får plats och returnerar ett fel.

Det bästa sättet är att ange `&jpegSize=` och lägga till parametern `&qlt=` om du levererar JPG-bilder till enheter med begränsat minne.

## Sammanfattning av bästa praxis {#best-practices-summary}

Det bästa sättet att uppnå en hög bildkvalitet och liten filstorlek är att börja med följande kombination av parametrar:

`fmt=jpg&qlt=85,0&resMode=sharp2&op_usm=1.75,0.3,2,0`

Den här kombinationen av inställningar ger utmärkta resultat under de flesta omständigheter.

Om bilden behöver optimeras ytterligare finjusterar du stegvis skärpeparametrarna (oskarp maskning) genom att börja med radien 0,2 eller 0,3. Därefter ökar du mängden gradvis från 1,75 till maximalt 4 (vilket motsvarar 400 % i Photoshop). Kontrollera att resultatet är det önskade.

Om skärpeeffekten fortfarande inte är tillräcklig ökar du radien i decimalsteg. För varje decimalsteg startar du om beloppet vid 1,75 och ökar det gradvis till 4. Upprepa den här processen tills du uppnår önskat resultat. Värdena ovan är en metod som de kreativa studierna har validerat, men kom ihåg att du kan börja med andra värden och följa andra strategier. Oavsett om resultaten är tillfredsställande för dig eller inte är en subjektiv fråga, så är strukturerade experiment avgörande.

När du experimenterar kan följande allmänna förslag vara användbara för att ytterligare optimera ditt arbetsflöde:

* Testa olika parametrar i realtid direkt på en URL.
* Det är en god vana att gruppera kommandona Dynamic Media Image Serving i en bildförinställning. En bildförinställning är i princip URL-kommandomakron med anpassade förinställningsnamn som `$thumb_low$` och `&product_high$`. Det anpassade förinställningsnamnet i en URL-sökväg anropar de här förinställningarna. Den här funktionen hjälper dig att hantera kommandon och kvalitetsinställningar för olika användningsmönster för bilder på webbplatsen och förkortar den totala längden på URL-adresser.
* Experience Manager har också mer avancerade sätt att finjustera bildkvaliteten, t.ex. genom att lägga på skärpa vid inhämtning. I avancerade fall där det finns alternativ för att justera och optimera återgivningsresultat kan [Adobe Professional Services](https://business.adobe.com/customers/consulting-services/main.html) hjälpa dig med anpassade insikter och metodtips.
