---
title: Förstå metadatabegrepp
description: Lär dig mer om behovet av och typer av metadata som gör det enklare att kategorisera och ordna resurser.
contentOwner: AG
role: User, Admin
feature: Metadata
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2653'
ht-degree: 5%

---

# Förstå metadatabegrepp {#why-we-need-metadata}

Metadata är data om data. I det här avseendet avser data era digitala resurser, till exempel en bild. Metadata är avgörande för effektiv resurshantering.

Metadata är en samling med alla data som är tillgängliga för en resurs, men som inte nödvändigtvis finns i den bilden. Några exempel på metadata är:

* Namnet på resursen.
* Tid och datum för senaste ändring.
* Storlek på resursen när den sparades i databasen.
* Namnet på mappen som den finns i.
* Relaterade resurser eller tillämpade taggar.

Ovanstående är de grundläggande metadataegenskaper som [!DNL Experience Manager] kan hantera för resurser, vilket gör att användare kan se alla resurser. Det kan till exempel vara bra att beställa resurser efter det senaste ändringsdatumet när du försöker identifiera nyligen tillagda resurser.

Du kan lägga till mer högnivådata till digitala resurser, till exempel:

* Typ av resurs (en bild, en video, ett ljudklipp eller ett dokument).
* Tillgångens ägare.
* Tillgångens namn.
* Beskrivning av tillgången.
* Taggar som tilldelats en resurs.

Fler metadata hjälper dig att kategorisera resurser ytterligare och är till hjälp när mängden digital information växer. Det går att hantera några hundra filer baserat på bara filnamnen. Den här metoden är dock inte skalbar. Den blir kort när antalet inblandade och antalet förvaltade tillgångar ökar.

Med hjälp av metadata ökar värdet på en digital resurs eftersom resursen blir

* Mer åtkomligt - system och användare hittar det enkelt.
* Enklare att hantera - du kan hitta resurser med samma uppsättning egenskaper enklare och använda ändringarna på dem.
* Fullständigt - materialet innehåller mer information och sammanhang med fler metadata.

Av dessa anledningar ger [!DNL Assets] dig rätt sätt att skapa, hantera och utbyta metadata för dina digitala resurser.

## Typer av metadata {#types-of-metadata}

De två grundläggande metadatatyperna är tekniska metadata och beskrivande metadata.

Tekniska metadata är användbara för program som hanterar digitala resurser och bör inte underhållas manuellt. [!DNL Experience Manager Assets] och andra program bestämmer automatiskt tekniska metadata och metadata kan ändras när resursen ändras. Vilka tekniska metadata som är tillgängliga för en mediefil beror till stor del på filtypen för resursen. Några exempel på tekniska metadata är:

* Storlek på en fil.
* Bildens mått (höjd och bredd).
* Bithastighet för en ljud- eller videofil.
* Bildens upplösning (detaljnivå).

Beskrivande metadata är metadata som rör programdomänen, till exempel det företag som en resurs kommer från. Beskrivande metadata kan inte bestämmas automatiskt. Den skapas manuellt eller halvautomatiskt. En GPS-aktiverad kamera kan till exempel automatiskt spåra latitud och longitud och lägga till geotagga bilden.

Kostnaden för att manuellt skapa beskrivande metadatainformation är hög. Därför har standarder upprättats för att underlätta utbyte av metadata mellan olika programsystem och organisationer. [!DNL Experience Manager Assets] stöder alla relevanta standarder för metadatahantering.

## Kodningsstandarder {#encoding-standards}

Det finns olika sätt att bädda in metadata i filer. Ett urval kodningsstandarder stöds:

* XMP: används av [!DNL Assets] för att lagra extraherade metadata i databasen.
* ID3: för ljud- och videofiler.
* Exif: för bildfiler.
* Annat/äldre: från [!DNL Microsoft Word], [!DNL PowerPoint], [!DNL Excel] och så vidare.

### XMP {#xmp}

[!DNL Extensible Metadata Platform] (XMP) är en öppen standard som används av [!DNL Experience Manager Assets] för all metadatahantering. Standarden erbjuder universell metadatakodning som kan bäddas in i alla filformat. Adobe och andra företag stöder XMP-standarden eftersom den erbjuder en innehållsmodell med mycket innehåll. Användare av XMP-standarden och av [!DNL Experience Manager Assets] har en kraftfull plattform att bygga vidare på. Mer information finns i [XMP](https://www.adobe.com/products/xmp.html).

### ID3 {#id}

Data som lagras i dessa ID3-taggar visas när du spelar upp en digital ljudfil på datorn eller en bärbar MP3-spelare.

ID3-taggar är utformade för filformatet MP3. Mer information om format:

* ID3-taggar fungerar i MP3- och mp3PRO-filer.
* WAV saknar taggar.
* WMA har egna taggar som inte tillåter implementering med öppen källkod.
* Ogg Vorbis använder Xiph-kommentarer som är inbäddade i Ogg-behållaren.
* AAC använder ett märkordsformat.

### Exif {#exif}

Exchangeable image file format (Exif) är det vanligaste metadataformatet som används för digitalfoto. Det är ett sätt att bädda in en fast ordlista med metadataegenskaper i många filformat, till exempel JPEG, TIFF, RIFF och WAV. Exif lagrar metadata som par av ett metadatanamn och ett metadatavärde. Dessa metadatanamn/värde-par kallas också taggar, som inte ska blandas ihop med taggningen i [!DNL Experience Manager]. Moderna digitalkameror skapar Exif-metadata och moderna grafikprogram stöder det. Exif-formatet är den minsta gemensamma nämnaren för metadatahantering, särskilt för bilder.

En stor begränsning för Exif är att ett fåtal populära bildfilsformat som BMP, GIF och PNG inte har stöd för det.

Metadatafält som definieras av Exif är vanligtvis av teknisk natur och har begränsad användning för beskrivande metadatahantering. Av den anledningen erbjuder [!DNL Experience Manager Assets] mappning av Exif-egenskaper till [vanliga metadatamatchningar](metadata-schemas.md) och till [XMP](xmp-writeback.md).

### Andra metadata {#other-metadata}

Andra metadata som kan bäddas in från filer är bland annat [!DNL Microsoft Word], [!DNL PowerPoint], [!DNL Excel].

## Förstå metadatamatchningar {#metadata-schemata}

Metadata-scheman är fördefinierade uppsättningar metadataegenskapsdefinitioner som kan användas i olika program. Egenskaper är alltid kopplade till en resurs, vilket innebär att egenskaperna är&quot;about&quot; för resursen.

Du kan också utforma egna metadatamatchningar om det inte finns några som passar dina behov. Duplicera inte befintlig information. Inom en organisation gör separering av scheman det enklare att dela metadata. [!DNL Experience Manager] ger dig en standardlista över de vanligaste metadatamappningarna. Listan hjälper dig att snabbt komma igång med metadatastrategin och snabbt välja de metadataegenskaper du behöver.

De metadatamappningar som stöds listas nedan.

### Standardmetadata {#standard-metadata}

* DC - [!DNL Dublin Core] är en viktig och allmänt använd uppsättning metadata.
* DICOM - Digital Imaging and Communications in Medicine.
* `Iptc4xmpCore` och `iptc4xmpExt` - International Press Communications Standard innehåller många ämnesspecifika metadata.
* RDF - Resource Description Framework - for generic semantic web metadata.
* XMP - [!DNL Extensible Metadata Platform].
* `xmpBJ` - grundläggande jobbiljett.

### Programspecifika metadata {#application-specific-metadata}

Programspecifika metadata innehåller tekniska och beskrivande metadata. Om du använder sådana metadata kanske andra program inte kan använda dessa metadata. Ett annat bildåtergivningsprogram kanske inte kan komma åt [!DNL Adobe Photoshop]-metadata. Du kan skapa ett arbetsflödessteg som ändrar en programspecifik egenskap till en standardegenskap.

* ACDSee - Metadata hanteras av programmet [!DNL ACDSee]. Se [www.acdsee.com/](https://www.acdsee.com/).
* Album - [!DNL Adobe Photoshop Album].
* CQ - Används av [!DNL Experience Manager Assets].
* DAM - används av [!DNL Experience Manager Assets].
* DEX - [!DNL Optima SC Description explorer] är en samling verktyg för metadata och filhantering för Windows-operativsystem.
* CRS - [Adobe Photoshop Camera Raw](https://helpx.adobe.com/camera-raw/using/introduction-camera-raw.html).
* LR - [!DNL Adobe Lightroom].
* MediaPro - [iView MediaPro](https://en.wikipedia.org/wiki/Phase_One_Media_Pro).
* MicrosoftPhoto och MP - Microsoft Photo.
* PDF och PDF/X.
* Photoshop och psAux - [!DNL Adobe Photoshop].

### Digital Rights Management-metadata (DRM) {#digital-rights-management-metadata}

* CC - [!DNL Creative Commons].
* [!DNL XMPRights].
* PLUS - [Universal System för bildlicensiering](https://www.useplus.com).
* PRISM - [Publiceringskrav för branschstandardmetadata](https://www.w3.org/submissions/2020/SUBM-prism-20200910/Image_Guide.pdf).
* PRL - PRISM Rights Language.
* PUR - PRISM-användningsrättigheter.
* `xmpPlus` - integrering av PLUS med XMP.

### Fotografispecifika metadata {#photography-specific-metadata}

* Exif - Teknisk information från kameran, inklusive GPS-position.
* CRS - [!DNL Camera Raw]-schema.
* `iptc4xmpCore` och `iptc4xmpExt`.
* TIFF - bildmetadata (inte bara för TIFF-bilder).

### Utskriftsspecifika metadata {#print-specific-metadata}

* PDF och PDF/X - Adobe PDF och tredjepartsprogram.
* PRISM - [Publiceringskrav för branschstandardmetadata](https://www.w3.org/submissions/2020/SUBM-prism-20200910/Image_Guide.pdf).
* XMP - [!DNL Extensible Metadata Platform].
* `xmpPG` - XMP-metadata för sidindelad text.

### Multimediaspecifika metadata {#multimedia-specific-metadata}

* `xmpDM` - [!DNL Dynamic Media].
* `xmpMM` - Mediehantering.

## Referens för metadatamappningar {#metadata-schemata-reference}

Följande referens innehåller information om ett visst metadataram (i alfabetisk ordning) och en lista över egenskaper och deras definitioner.

### Dublin Core {#dublin-core}

Dublin Core-metadata innehåller en standardiserad uppsättning konventioner för att beskriva resurser så att de blir lättare att hitta. I [!DNL Assets] beskriver Dublin Core digitala resurser som video, ljud, bilder och dokument.

Den enkla DCMES-uppsättningen (Dublin Core Metadata Element Set) innehåller 15 metadataelement som listas i följande tabell. Varje Dublin Core-element är valfritt och kan upprepas. Du kan lägga till eller ta bort metadata för Dublin Core på samma sätt som du gör för medietypsspecifika metadata.

Förutom DCMES finns det andra metadataelement som skapats av Dublin Core Initiative. Mer information finns i [Dublin Core-initiativet](https://dublincore.org/).

| Egenskap | Beskrivning |
| ----------- | ------------------------------------------------------------------------------------------------------------------------ |
| medverkande | Den person eller det företag som är ansvarigt för att bidra till innehållet. |
| täckning | Den geografiska plats eller tidsperiod som tillgången täcker. |
| skapare | Den person eller det företag som ansvarar för att skapa innehållet. |
| datum | Datum eller tidsperiod som är associerad med tillgången. |
| description | Mer information om resursen. |
| format | Filformat, fysiskt medium eller dimensioner för resursen. [!DNL Experience Manager] använder `dc:format` för att ange resursens MIME-typ. |
| identifierare | En unik referens till tillgången. |
| språk | Resursens språk (till exempel `en` för engelska). |
| utgivare | Den person eller det företag som ansvarar för att göra tillgången tillgänglig. |
| relation | En relaterad tillgång. |
| rättigheter | Information om vem som har behörighet till den här resursen. |
| källa | En relaterad tillgång som tillgången härrör från. |
| ämne | Tillgångens ämne. |
| title | Ett namn för resursen. |
| type | Tillgångens art eller genre. |

### IPTC {#iptc}

International Press Telecommunications Council (IPTC) är ett konsortium av nyhetsbyråer runt om i världen - ett av målen är att utveckla och underhålla tekniska standarder. IPTC har definierat en uppsättning metadatastandarder för bilder som är nästan allmänt accepterade bland fotografer. Dessa metadatastandarder var en del av den bredare standarden IPTC Information Interchange Model (IIM) som skapades på 90-talet.

Även om IPTC rubrikinformation i huvudsak har ersatts av XMP finns det ett IPTC Core-schema och ett tilläggsschema för XMP. I bildprogram synkroniseras både XMP- och IPTC-egenskaper.

## Metadatastyrda arbetsflöden {#metadata-driven-workflows}

Genom att skapa metadatadrivna arbetsflöden kan du automatisera vissa processer, vilket ökar effektiviteten. I ett metadatadrivet arbetsflöde läser arbetsflödet och utför därför en fördefinierad åtgärd. Du kan till exempel använda metadatadrivna arbetsflöden på olika sätt:

* Arbetsflödet kan kontrollera om en bild har en titel eller inte. Om så inte är fallet meddelas systemet om att en titel ska läggas till.
* Arbetsflödet kan kontrollera om ett copyrightmeddelande för en mediefil tillåter distribution eller inte. Systemet skickar alltså resursen till den ena servern eller den andra.
* Ett arbetsflöde kan söka efter resurser utan fördefinierade, obligatoriska metadata eller resurser med *ogiltiga*-metadata.

## XMP metadata {#xmp-metadata}

XMP (Extensible Metadata Platform) är den metadatastandard som används av [!DNL Adobe Experience Manager Assets] för all metadatahantering. XMP har ett standardformat för framtagning, bearbetning och utbyte av metadata i en mängd olika program.

Förutom universell metadatakodning som kan bäddas in i alla filformat, tillhandahåller XMP en omfattande [innehållsmodell](#xmp-core-concepts) och stöds [av Adobe](#advantages-of-xmp) och andra företag, så att användare av XMP i kombination med [!DNL Assets] har en kraftfull plattform att bygga vidare på.

[XMP-specifikationen](https://www.adobe.com/devnet/xmp.html) är tillgänglig från Adobe.

### Vad är XMP? {#what-is-xmp}

Adobe introducerade först XMP som en del av Adobe Acrobat programprodukt. Sedan dess har XMP-standarden blivit allmänt förekommande. [!DNL Assets] har inbyggt stöd för XMP - Extensible Metadata Platform som leds av Adobe. XMP är en standard för bearbetning och lagring av standardiserade och egna metadata i digitala resurser. XMP är en standard som gör att flera program kan arbeta effektivt med metadata.

Produktionsproffs använder t.ex. det inbyggda stödet för XMP i Adobe program för att skicka information till olika filformat. [!DNL Assets]-databasen extraherar XMP-metadata och använder den för att hantera innehållets livscykel och erbjuder möjlighet att skapa automatiserade arbetsflöden.

XMP standardiserar hur metadata definieras, skapas och bearbetas genom att tillhandahålla en datamodell, en lagringsmodell och scheman. Alla dessa begrepp beskrivs i detta avsnitt.

Alla äldre metadata från EXIF, ID3 eller Microsoft Office översätts automatiskt till XMP, som kan utökas för att stödja kundspecifika metadatamatchningar, som produktkataloger.

Metadata i XMP består av en uppsättning egenskaper. Dessa egenskaper är alltid kopplade till en
en viss enhet som kallas en resurs, d.v.s. egenskaperna är&quot;om&quot; resursen. Om det finns XMP är resursen alltid resursen.

### XMP ekosystem {#xmp-ecosystem}

XMP definierar en [metadatamodell](https://sv.wikipedia.org/wiki/Metadata) som kan användas med alla definierade metadataobjekt. XMP definierar också särskilda [scheman](https://en.wikipedia.org/wiki/XML_schema) för grundläggande egenskaper som är användbara för att logga en resurs historik genom olika bearbetningssteg – från fotografering, [skanning](https://sv.wikipedia.org/wiki/Bildl%C3%A4sare) och textredigering via fotoredigeringssteg (som [beskärning](https://sv.wikipedia.org/wiki/Bildbesk%C3%A4rning) eller färgjustering) till den slutliga bilden. Med XMP kan alla program och enheter längs vägen lägga till egen information i en digital resurs, som sedan sparas i den slutliga digitala filen.

XMP serialiseras och lagras oftast med en underuppsättning av [W3C](https://sv.wikipedia.org/wiki/World_Wide_Web_Consortium) [Resource Description Framework](https://sv.wikipedia.org/wiki/Resource_Description_Framework) (RDF), som i sin tur uttrycks i [XML](https://sv.wikipedia.org/wiki/XML).

### Fördelar med XMP {#advantages-of-xmp}

XMP har följande fördelar jämfört med andra kodningsstandarder och -scheman:

* XMP-baserade metadata är mycket kraftfulla och detaljerade.
* Med XMP kan du ha flera värden för en egenskap.
* XMP har standardiserad kodning som gör att du enkelt kan utbyta metadata.
* XMP är utbyggbart. Du kan lägga till ytterligare information i dina resurser.

XMP-standarden är utformad för att vara utbyggbar så att du kan lägga till anpassade typer av metadata i XMP-data. EXIF har däremot inte det, utan en fast lista över egenskaper som inte kan utökas.

>[!NOTE]
>
>XMP tillåter vanligtvis inte att binära datatyper bäddas in. Om du vill ha binära data i XMP, till exempel miniatyrbilder, måste de kodas i ett XML-anpassat format som `Base64`.

### XMP concepts {#xmp-core-concepts}

I följande avsnitt beskrivs grundbegreppen för XMP, inklusive namnutrymmen och scheman, egenskaper och värden samt språkalternativ.

#### Namnutrymmen och scheman {#namespaces-and-schemata}

Ett XMP-schema är en uppsättning egenskapsnamn i ett vanligt XML-namnutrymme som innehåller
datatypen och beskrivande information. Ett XMP-schema identifieras av dess XML-namnområdes-URI. Om du använder namnutrymmen förhindras konflikter mellan egenskaper i olika scheman som har samma namn men en annan betydelse.

Egenskapen `Creator` i två oberoende utformade scheman kan till exempel betyda den person som skapade resursen eller det program som skapade resursen (till exempel Adobe Photoshop).

#### Egenskaper och värden {#properties-and-values}

XMP kan innehålla egenskaper från ett eller flera av scheman. En vanlig delmängd som används av många Adobe-program kan till exempel vara följande:

* Dublin Core-schema: `dc:title`, `dc:creator`, `dc:subject`, `dc:format`, `dc:rights`.
* XMP grundläggande schema: `xmp:CreateDate`, `xmp:CreatorTool`, `xmp:ModifyDate`, `xmp:metadataDate`.
* XMP Rights Management-schema: `xmpRights:WebStatement`, `xmpRights:Marked`.
* XMP mediahanteringsschema: `xmpMM:DocumentID`.

#### Språkalternativ {#language-alternatives}

Med XMP kan du lägga till en `xml:lang`-egenskap i textegenskaperna för att ange textens språk.

## Arbeta med IPTC-metadata {#support-for-iptc-metadata}

Lär dig hur [!DNL Adobe Experience Manager Assets] har stöd för IPTC-metadata, Creative-klassificeringar och nyckelord som läggs till i resurser via [!DNL Adobe Bridge] och andra [!DNL Adobe Creative Cloud]-program.

[!DNL Adobe Experience Manager Assets] stöder metadatastandarden för IPTC som används ofta för att beskriva resurser. På det här sättet förbättrar [!DNL Assets] accepterandet av bilderna mellan olika parter, bland annat fotografer, byråer, bibliotek och museer.

Standardmetadataschemat för mediefiler innehåller nu metadatamatcheman för IPTC Core och IPTC Extension som definierar omfattande metadataegenskaper som gör att användare kan lägga till exakta och tillförlitliga data om personer, platser och produkter som visas i en bild. Det har även stöd för datum, namn och identifierare för att skapa bilden samt ett flexibelt sätt att uttrycka rättighetsinformation.

Sidan Egenskaper för resurser innehåller nu separata flikar för att visa IPTC Core- och IPTC Extension-metadata i redigerbara fält.

1. Välj en bild i användargränssnittet för [!DNL Assets].
1. Klicka på **[!UICONTROL Properties]** i verktygsfältet.
1. Klicka på fliken **[!UICONTROL IPTC]** för att visa metadata för resursen i IPTC.
1. Redigera IPTC-metadataegenskaperna efter behov.

   ![iptc_tab](assets/keywords-in-iptc-tab.png)

1. Klicka på fliken **[!UICONTROL IPTC Extension]** för att visa metadata för IPTC Extension för resursen.
1. Redigera metadataegenskaperna för IPTC Extension efter behov.
1. Klicka på **[!UICONTROL Save & Close]** om du vill spara ändringarna.

### Stöd för kreativa betyg {#creative-rating-support}

Förutom att visa enskilda användarklassificeringar och aggregerade klassificeringar, visar egenskapssidan nu de klassificeringar som tilldelats resurser via Adobe Bridge och andra Creative-program

Dessa klassificeringar finns i avsnittet **[!UICONTROL Creative Rating]** på fliken **[!UICONTROL Advanced]**.

Den här klassificeringen är en skrivskyddad egenskap och varierar mellan 1 och 5. Du kan söka efter resurser baserat på deras Creative Rating från sökpanelen.

Den här egenskapen är dock för närvarande inte indexerad för att undvika konflikter med anpassade ändringar som görs av användare.

### Stöd för nyckelord {#keyword-support}

Fliken **[!UICONTROL IPTC]** på sidan [!UICONTROL Properties] visar även nyckelord som lagts till i resurser via Adobe Bridge och andra Adobe Creative Cloud-appar. Du kan också redigera dessa nyckelord och lägga till fler nyckelord från fliken **[!UICONTROL IPTC]**.

![nyckelord](assets/keywords-in-iptc-tab.png)
