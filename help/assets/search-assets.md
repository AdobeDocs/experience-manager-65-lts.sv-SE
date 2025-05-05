---
title: Sök efter digitala resurser och bilder i  [!DNL Adobe Experience Manager]
description: Lär dig hur du söker efter de nödvändiga resurserna i [!DNL Adobe Experience Manager] genom att använda panelen Filter och hur du använder de resurser som visas i sökningen.
contentOwner: AG
mini-toc-levels: 1
feature: Search, Metadata
role: User
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 3511e07b-f6d0-435a-aa80-55357d3dccf5
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '5593'
ht-degree: 3%

---

# Sök efter digitala resurser i [!DNL Adobe Experience Manager] {#search-assets-in-aem}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/search-assets.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

[!DNL Adobe Experience Manager Assets] innehåller robusta metoder för resursidentifiering som hjälper dig att uppnå högre innehållshastighet. Teamen kan korta time-to-market med smidiga, intelligenta sökfunktioner som är körklara och anpassade metoder. Att söka resurser är centralt för användningen av ett digitalt resurshanteringssystem - oavsett om det är avsett för kreativa användare, för robust hantering av resurser av företagsanvändare och marknadsförare eller för administration av DAM-administratörer. Enkla, avancerade och anpassade sökningar som du kan utföra via [!DNL Assets]-användargränssnittet eller andra appar och ytor hjälper dig att uppfylla dessa användningsfall.

[!DNL Experience Manager Assets] har stöd för följande användningsfall och den här artikeln beskriver användning, begrepp, konfigurationer, begränsningar och felsökning för dessa användningsfall.

| Sök efter resurser | Konfigurera och administrera sökfunktioner | Arbeta med sökresultat |
|---|---|---|
| [Grundläggande sökningar](#searchbasics) | [Sökindex](#searchindex) | [Sortera resultat](#sort) |
| [Förstå sökgränssnittet](#searchui) | [Visuell sökning eller likhetssökning](#configvisualsearch) | [Kontrollera egenskaper och metadata för en resurs](#checkinfo) |
| [Sökförslag](#searchsuggestions) | [Obligatoriska metadata](#mandatorymetadata) | [Hämta](#download) |
| [Förstå sökresultat och beteenden](#searchbehavior) | [Ändra sökfaktorer](#searchfacets) | [Massmetadatauppdateringar](#metadataupdates) |
| [Sökrankning och förstärkning](#searchrank) | [Textextrahering](#extracttextupload) | [Smarta samlingar](#collections) |
| [Avancerad sökning: filtrering och sökomfattning](#scope) | [Anpassade predikat](#custompredicates) | [Förstå och felsöka oväntade resultat](#unexpected-results) |
| [Sök från andra lösningar och appar](#search-assets-other-surfaces):<ul><li>[Adobe Asset Link](#aal)</li><li>[Brand Portal](#brand-portal)</li><li>[Experience Manager-datorprogram](#desktop-app)</li><li>[Adobe Stock-bilder](#adobe-stock)</li><li>[Dynamiska medieresurser](#dynamic-media)</li></ul> | | |
| [Resursväljare](#asset-picker) | | |
| [Begränsningar](#limitations) och [Tips](#tips) | | |
| [Illustrerade exempel](#samples) | | |

Sök efter digitala resurser med Omnissearch-fältet högst upp i webbgränssnittet [!DNL Experience Manager]. Gå till **[!UICONTROL Assets]** > **[!UICONTROL Files]** i [!DNL Experience Manager], klicka på ![search_icon](assets/do-not-localize/search_icon.png) i det övre fältet, ange söknyckelord och välj `Return`. Du kan också använda nyckelordskortet `/` (snedstreck) för att öppna Omnissearch-fältet. `Location:Assets` är förvalt för att begränsa sökningarna till DAM-resurser. [!DNL Experience Manager] ger förslag när du börjar skriva ett söknyckelord.

Använd panelen **[!UICONTROL Filters]** för att söka efter resurser, mappar, taggar och metadata. Du kan filtrera sökresultaten baserat på de olika alternativen (predikaten), t.ex. filtyp, filstorlek, datum då filen senast ändrades, status för resursen, information om insikter och Adobe Stock-licensiering. Du kan anpassa filterpanelen och lägga till eller ta bort sökpredikatorer med [sökfaktorer](/help/assets/search-facets.md). Filtret [!UICONTROL File Type] på panelen [!UICONTROL Filters] har kryssrutor för blandat läge. Om du inte markerar alla kapslade predikat (eller format) markeras därför kryssrutorna på första nivån delvis.

[!DNL Experience Manager]-sökfunktionen stöder sökning efter samlingar och sökning efter resurser i en samling. Se [söksamlingar](/help/assets/manage-collections.md).

## Förstå sökgränssnittet {#searchui}

Bekanta dig med sökgränssnittet och de tillgängliga åtgärderna.

![Förstå Experience Manager Assets gränssnitt för sökresultat](assets/aem_search_results.png)

*Figur: Förstå [!DNL Experience Manager Assets] gränssnittet för sökresultat.*

**A.** Spara sökningen som en smart samling. **B.** Filtrerar eller förutsäger för att begränsa sökresultaten. **C.** Visa filer, mappar eller båda. **D.** Klicka på Filter för att öppna eller stänga den vänstra rutan. **E.** Sökplatsen är DAM. **F.** Omundersökningsfält med användardefinierat söknyckelord. **G.** Välj inlästa sökresultat. **H.** Antal visade sökresultat av totalt antal sökresultat. **I.** Stäng sökning. **J.** Växla mellan kortvyn och listvyn.

### Dynamiska sökfaktorer {#dynamicfacets}

Du kan identifiera önskade resurser snabbare från sökresultatsidan med det dynamiskt uppdaterade antalet förväntade sökresultat i sökmetoderna. Det förväntade antalet resurser uppdateras även innan sökfiltret används. Genom att se det förväntade antalet mot filtret kan du snabbt och effektivt navigera bland sökresultaten.

![Se det ungefärliga antalet resurser utan att filtrera sökresultaten i sökfaktorer.](assets/asset_search_results_in_facets_filters.png)

*Bild: Se det ungefärliga antalet resurser utan att filtrera sökresultaten i sökfaktorer.*

## Förstå sökresultat och beteenden {#searchbehavior}

### Grundläggande söktermer och sökresultat {#searchbasics}

Du kan köra nyckelordssökningar från OmniSearch-fältet. Nyckelordssökningen är inte skiftlägeskänslig och är en fulltextsökning (i alla vanliga metadatafält). Om mer än ett nyckelord används är `AND` standardoperatorn mellan nyckelorden.

Resultatet sorteras efter relevans, med början med närmast matchande. För flera nyckelord är mer relevanta resultat de resurser som innehåller båda termerna i sina metadata. I metadata rangordnas nyckelord som visas som smarta taggar högre än nyckelord som visas i andra metadatafält. [!DNL Experience Manager] tillåter att en viss sökterm får högre vikt. Det går också att [höja rankningen](#searchrank) för ett fåtal målresurser för specifika söktermer.

För att snabbt hitta relevanta resurser innehåller det avancerade gränssnittet funktioner för filtrering, sortering och markering. Du kan filtrera resultat baserat på flera villkor och se antalet sökningar efter olika filter. Du kan också köra sökningen igen genom att ändra frågan i fältet Omnissearch. När du ändrar söktermer eller filter används de andra filtren för att bevara sökkontexten.

När resultatet är många resurser visar [!DNL Experience Manager] de första 100 i kortvyn och 200 i listvyn. När användare rullar läses fler resurser in. Detta för att förbättra prestandan. Titta på en videodemonstration av [antalet resurser som visas](https://www.youtube.com/watch?v=LcrGPDLDf4o).

Ibland kan du se oväntade resurser i sökresultaten. Mer information finns i [oväntade resultat](#unexpected-results).

[!DNL Experience Manager] kan söka i många filformat och sökfiltren kan anpassas efter dina affärsbehov. Kontakta administratören för att få veta vilka sökalternativ som är tillgängliga för DAM-databasen och vilka begränsningar ditt konto har.

### Resultat med och utan förbättrade smarta taggar {#withsmarttags}

Som standard kombineras söktermerna i [!DNL Experience Manager] med en AND-sats. Du kan till exempel söka efter nyckelordskvinna som springer. Som standard visas endast resurser med både kvinna och nyckelord som körs i metadata i sökresultatet. Samma beteende behålls när specialtecken (punkter, understreck eller streck) används med nyckelorden. Följande sökfrågor returnerar samma resultat:

* `woman running`
* `woman.running`
* `woman-running`

Frågan `woman -running` returnerar dock resurser utan `running` i sina metadata.
Om du använder smarta taggar läggs en extra `OR`-sats till för att hitta någon av söktermerna som de använda smarta taggarna. En resurs som taggats med antingen `woman` eller `running` med smarta taggar visas också i en sådan sökfråga. Sökresultaten är en kombination av

* Assets med nyckelorden `woman` och `running` i metadata (standardbeteende).

* Assets smart taggad med något av nyckelorden (beteendet Smarta taggar).

### Sök efter förslag medan du skriver {#searchsuggestions}

När du börjar skriva nyckelord föreslår [!DNL Experience Manager] möjliga söknyckelord eller fraser. Förslagen baseras på metadata för de befintliga resurserna. [!DNL Experience Manager] indexerar alla metadatafält som ska vara till hjälp vid sökning. Systemet använder värdena från följande metadatafält för att ge sökförslag. Om du vill ge sökförslag bör du överväga att fylla i följande fält med lämpliga nyckelord:

* Resurstaggar. (mappar till `jcr:content/metadata/cq:tags`)
* Resursrubrik. (mappar till `jcr:content/metadata/dc:title`)
* Resursbeskrivning. (mappar till `jcr:content/metadata/dc:description`)
* Titel i JCR-databasen. Värdet kan mappas till Resursrubrik. (mappar till `jcr:content/jcr:title`)
* Beskrivning i JCR-databasen. Värdet kan mappas till tillgångsbeskrivningen. (mappar till `jcr:content/jcr:description`)

Om du vill få förslag på fler än ett söknyckelord fortsätter du att skriva alla nyckelord utan att markera något förslag för ett nyckelord.

![Skriv flera nyckelord för att visa förslag som passar alla](assets/search_suggestionsmanykeywords.gif)

*Bild: Skriv flera nyckelord för att visa förslag som passar alla.*

### Sök efter rankning och förstärkning {#searchrank}

Sökresultaten som matchar alla söktermer i metadatafält visas först, följt av sökresultaten som matchar någon av söktermerna i de smarta taggarna. I ovanstående exempel är den ungefärliga visningsordningen för sökresultat:

1. Matchar `woman running` i de olika metadatafälten.
1. Matchar `woman running` i smarta taggar.
1. Matchar `woman` eller `running` i smarta taggar.

Du kan förbättra nyckelordens relevans för vissa resurser för att öka sökningen baserat på nyckelorden. Det innebär att de bilder som du befordrar särskilda nyckelord för visas högst upp i sökresultatet när du söker baserat på dessa nyckelord.

1. Öppna egenskapssidan för resursen från användargränssnittet [!DNL Assets]. Klicka på **[!UICONTROL Advanced]** och klicka på **[!UICONTROL Add]** under **[!UICONTROL Elevate for search keywords]**.
1. I rutan **[!UICONTROL Search Promote]** anger du ett nyckelord som du vill göra sökningen efter bilden snabbare och klickar sedan på **[!UICONTROL Add]**. Du kan ange flera nyckelord på samma sätt.
1. Klicka på **[!UICONTROL Save & Close]**. Den resurs som du befordrade för det här nyckelordet visas bland de översta sökresultaten.

Du kan använda detta till din fördel genom att öka rankningen för vissa resurser i sökresultaten för nyckelordet target. Se exempelvideon nedan. Mer information finns i [söka i [!DNL Experience Manager]](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search-boost.html?lang=sv-SE).

>[!VIDEO](https://video.tv.adobe.com/v/16766/?quality=6)

*Video: Förstå hur sökresultaten rangordnas och hur rangordningen kan påverkas.*

## Avancerad sökning {#scope}

[!DNL Experience Manager] innehåller olika metoder, till exempel filter, som tillämpas på de sökda resurserna, så att du snabbare kan hitta de önskade resurserna. Nedan beskrivs några vanliga metoder. Några [illustrerade exempel](#samples) delas nedan.

**Sök efter filer eller mappar**: I sökresultaten kan du se antingen filer, mappar eller båda. På panelen **[!UICONTROL Filters]** kan du välja lämpligt alternativ. Se [sökgränssnitt](#searchui).

**Sök efter resurser i en mapp**: Du kan begränsa sökningen till en viss mapp. Lägg till en mappsökväg på panelen **[!UICONTROL Filters]**. Du kan bara markera en mapp i taget.

![Begränsa sökresultat till en mapp genom att lägga till en mappsökväg i panelen Filter](assets/search_folder_select.gif)

*Figur: Begränsa sökresultaten till en mapp genom att lägga till en mappsökväg på panelen Filter.*

### Söka efter liknande bilder {#visualsearch}

Om du vill söka efter bilder som visuellt liknar en användarvald bild klickar du på alternativet **[!UICONTROL Find Similar]** i kortvyn för en bild eller i verktygsfältet. [!DNL Experience Manager] visar de smarta taggade bilderna från DAM-databasen som liknar den bild som användaren har valt. Läs [Så här konfigurerar du likhetssökning](#configvisualsearch).

![Sök efter liknande bilder med alternativet i kortvyn](assets/search_find_similar.png)

*Bild: Sök efter liknande bilder med alternativet i kortvyn.*

### Adobe Stock-bilder {#adobe-stock}

I användargränssnittet [!DNL Experience Manager] kan användare söka efter [Adobe Stock-resurser](/help/assets/aem-assets-adobe-stock.md) och licensiera de nödvändiga resurserna. Lägg till `Location: Adobe Stock` i sökfältet. Du kan också använda panelen Filter för att hitta alla licensierade eller olicensierade mediefiler eller söka efter en viss mediefil med hjälp av Adobe Stock filnummer.

### Dynamiska medieresurser {#dmassets}

Du kan filtrera efter dynamiska mediebilder genom att välja **[!UICONTROL Dynamic Media]** > **[!UICONTROL Sets]** på panelen **[!UICONTROL Filters]**. Den filtrerar och visar resurser som bilduppsättningar, karuseller, blandade medieuppsättningar och snurruppsättningar.

### GQL-sökning med specifika värden i metadatafält {#gql-search}

Du kan söka efter digitala resurser baserat på exakta värden för metadatafält, som titel, beskrivning och skapare. Funktionen för fulltextsökning i GQL hämtar endast resurser vars metadatavärde exakt matchar din sökfråga. Namnen på egenskaperna (Skapare, Titel och så vidare) och värdena är skiftlägeskänsliga.

| Metadatafält | Fasettvärde och -användning |
|---|---|
| Titel | title:John |
| Skapare | skapare:John |
| Plats | plats:NA |
| Beskrivning | description:&quot;Sample Image&quot; |
| Skapare | creatortool:&quot;Adobe Photoshop&quot; |
| Copyright-ägare | copyrightowner:&quot;Adobe Systems&quot; |
| Medarbetare | medarbetare:John |
| Användningsvillkor | usageterms:&quot;CopyRights Reserved&quot; |
| Skapad | skapad:YYY-MM-DDTHH |
| Utgångsdatum | förfaller:ÅÅÅ-MM-DDTHH |
| I tid | ontime:YYY-MM-DDTHH |
| Fråntid | offtime:YYY-MM-DDTHH |
| Tidsintervall (förfaller dateontime, offtime) | facet field : lowerbound..upperbound |
| Bana | /content/dam/&lt;mappnamn> |
| PDF Title | pdftitle:&quot;Adobe Document&quot; |
| Ämne | ämne: Utbildning |
| Taggar | taggar:&quot;Plats och resa&quot; |
| Typ | type:&quot;image\png&quot; |
| Bildens bredd | width:lowerbound..upperbound |
| Bildens höjd | height:lowerbound..upperbound |
| Person | person:John |

Egenskaperna `path`, `limit`, `size` och `orderby` kan inte kombineras med operatorn `OR` med någon annan egenskap.

<!-- TBD: Where are the limit, size, orderby properties defined?
-->

Nyckelordet för en användargenererad egenskap är dess fältetikett i egenskapsredigeraren i gemener, med borttagna blanksteg.

Här är några exempel på sökformat för komplexa frågor:

* Så här visar du alla resurser med flera facets-fält (till exempel: title=John Doe och creator-verktyget = Adobe Photoshop): `title:"John Doe" creatortool:Adobe*`
* Så här visar du alla resurser när ansiktsvärdet inte är ett enda ord utan en mening (till exempel: title=Scott Reynolds): `title:"Scott Reynolds"`
* Så här visar du resurser med flera värden för en enda egenskap (till exempel: title=Scott Reynolds eller John Doe): `title:"Scott Reynolds" OR "John Doe"`
* Så här visar du resurser med egenskapsvärden som börjar med en viss sträng (till exempel: titeln är Scott Reynolds): `title:Scott*`
* Så här visar du resurser med egenskapsvärden som slutar med en specifik sträng (till exempel: titeln är Scott Reynolds): `title:*Reynolds`
* Så här visar du resurser med ett egenskapsvärde som innehåller en specifik sträng (till exempel: title = Basel Meeting Room): `title:*Meeting*`
* Så här visar du resurser som innehåller en viss sträng och som har ett specifikt egenskapsvärde (till exempel: sök efter Adobe i resurser som har title=John Doe): `*Adobe* title:"John Doe"`

## Sök efter digitala resurser från andra [!DNL Experience Manager]-erbjudanden eller gränssnitt {#search-assets-other-surfaces}

[!DNL Adobe Experience Manager] ansluter DAM-databasen till olika andra [!DNL Experience Manager]-lösningar för att ge snabbare åtkomst till digitala resurser och effektivisera de kreativa arbetsflödena. Alla resursidentifieringar börjar med bläddring eller sökning. Sökfunktionen är i stort sett densamma på alla olika ytor och lösningar. Vissa sökmetoder ändras när målgruppen, användningsexemplen och användargränssnittet varierar mellan [!DNL Experience Manager]-lösningarna. De specifika metoderna beskrivs för de enskilda lösningarna på länkarna nedan. De universellt tillämpliga tipsen och beteendena beskrivs i den här artikeln.

### Söka efter digitala resurser från panelen Adobe Asset Link {#aal}

Med Adobe Asset Link kan formgivarna nu komma åt innehåll som lagras i [!DNL Experience Manager Assets], utan att lämna de Adobe Creative Cloud-program som stöds. Med hjälp av panelen i appen [!DNL Adobe Creative Cloud]-apparna kan du enkelt bläddra bland, söka efter, checka ut och checka in resurser: [!DNL Adobe Photoshop], [!DNL Adobe Illustrator] och [!DNL Adobe InDesign]. Med Asset Link kan du också söka visuellt liknande resultat. Visuella sökresultat bygger på Adobe Sensei maskininlärningsalgoritmer och hjälper användarna att hitta estetiskt liknande bilder. Se [söka efter och bläddra bland resurser](https://helpx.adobe.com/se/enterprise/using/manage-assets-using-adobe-asset-link.html#UseAdobeAssetLink) med Adobe Asset Link.

### Sök efter digitala resurser i [!DNL Experience Manager]-datorprogrammet {#desktop-app}

Kreatörer använder skrivbordsappen för att göra [!DNL Experience Manager Assets] enkelt sökbar och tillgänglig på sin lokala dator (Win eller Mac). Creative Cloud-användare kan enkelt visa de önskade resurserna i Mac Finder eller Utforskaren i Windows, som öppnats i skrivbordsprogram och ändrats lokalt. Ändringarna sparas sedan i [!DNL Experience Manager] med en ny version som skapats i databasen. Programmet stöder enkla sökningar med ett eller flera nyckelord, `*`, `?` jokertecken och operatorn `AND`. Se [bläddra bland, söka efter och förhandsgranska resurser](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=sv-SE#browse-search-preview-assets) i skrivbordsappen.

### Sök efter digitala resurser i [!DNL Brand Portal] {#brand-portal}

Affärsanvändare och marknadsförare använder Brand Portal för att effektivt och säkert dela godkända digitala resurser med interna team, partners och återförsäljare. Se [söka efter resurser på Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/search-capabilities/brand-portal-searching.html?lang=sv-SE).

### Sök efter [!DNL Adobe Stock] bilder {#adobe-stock1}

I användargränssnittet [!DNL Experience Manager] kan användare söka efter Adobe Stock-resurser och licensiera de nödvändiga resurserna. Lägg till `Location: Adobe Stock` i sökfältet. Du kan också använda panelen **[!UICONTROL Filters]** för att hitta alla licensierade eller olicensierade mediefiler eller söka efter en viss mediefil med hjälp av Adobe Stock filnummer. Se [hantera [!DNL Adobe Stock] bilder i [!DNL Experience Manager]](/help/assets/aem-assets-adobe-stock.md#usemanage).

### Sök efter [!DNL Dynamic Media] resurser {#dynamic-media}

Du kan filtrera efter dynamiska mediebilder genom att välja **[!UICONTROL Dynamic Media]** > **[!UICONTROL Sets]** på panelen **[!UICONTROL Filters]**. Den filtrerar och visar resurser som bilduppsättningar, karuseller, blandade medieuppsättningar och rotationsuppsättningar. När författarna redigerar webbsidor kan de söka efter uppsättningar inifrån Content Finder. Det finns ett filter för uppsättningar på en snabbmeny.

### Söka efter digitala resurser i Content Finder vid redigering av webbsidor {#content-finder}

Författare kan använda Content Finder för att söka i DAM-databasen efter relevanta resurser och använda resurserna på de webbsidor de skapar. Upphovsmannen kan också använda funktionen för anslutna Assets för att söka efter resurser som är tillgängliga på en fjärrdistribution av [!DNL Experience Manager]. Författare kan sedan använda dessa resurser på webbsidor på en lokal [!DNL Experience Manager]-distribution. Se [använda fjärrresurser](/help/assets/use-assets-across-connected-assets-instances.md#use-remote-assets).

### Sök i samlingar {#collections}

[!DNL Experience Manager]-sökfunktionen stöder sökning efter samlingar och sökning efter resurser i en samling. Se [söksamlingar](/help/assets/manage-collections.md).

## Resursväljare {#asset-picker}

>[!NOTE]
>
>Resursväljaren anropades som [resursväljare](https://helpx.adobe.com/se/experience-manager/6-2/assets/using/asset-picker.html) i tidigare versioner av [!DNL Adobe Experience Manager].

Med resursväljaren kan du söka efter, filtrera och bläddra bland DAM-resurser på ett speciellt sätt. Resursväljaren är tillgänglig på `https://[aem_server]:[port]/aem/assetpicker.html`. Du kan hämta metadata för resurser som du väljer med resursväljaren. Du kan starta det med begärandeparametrar som stöds, till exempel resurstyp (bild, video, text) och markeringsläge (enstaka eller flera markeringar). De här parametrarna anger kontexten för resursväljaren för en viss sökinstans och förblir intakta genom hela markeringen.

Resursväljaren använder HTML5 `Window.postMessage`-meddelandet för att skicka data för den valda resursen till mottagaren. Det fungerar bara i bläddringsläget och endast med omsökningsresultatsidan.

Skicka följande frågeparametrar i en URL för att starta resursväljaren i en viss kontext:

| Namn | Värden | Exempel | Syfte |
|---|---|---|---|
| resurssuffix (B) | Mappsökväg som resurssuffix i URL:en: [https://localhost:4502/aem/assetpicker.html/](https://localhost:4502/aem/assetpicker.html) | Om du vill starta resursväljaren med en viss mapp markerad, till exempel med mappen `/content/dam/we-retail/en/activities` markerad, ska URL:en ha formatet: `https://localhost:4502/aem/assetpicker.html/content/dam/we-retail/en/activities?assettype=images` | Om du vill att en viss mapp ska väljas när resursväljaren startas, skickar du den som ett resurssuffix. |
| `mode` | en, flera | <ul><li>`https://localhost:4502/aem/assetpicker.html?mode=single`</li><li>`https://localhost:4502/aem/assetpicker.html?mode=multiple`</li></ul> | I flera lägen kan du markera flera resurser samtidigt med resursväljaren. |
| `dialog` | true, false | [https://localhost:4502/aem/assetpicker.html?dialog=true](https://localhost:4502/aem/assetpicker.html?dialog=true) | Använd de här parametrarna för att öppna resursväljaren som Granite-dialogrutan. Det här alternativet kan bara användas när du startar resursväljaren via fältet Bevilja sökväg och konfigurerar den som URL för pickerSrc. |
| `root` | &lt;mappsökväg> | `https://localhost:4502/aem/assetpicker.html?assettype=images&root=/content/dam/we-retail/en/activities` | Använd det här alternativet om du vill ange rotmappen för resursväljaren. I det här fallet kan du bara välja underordnade resurser (direkt/indirekt) under rotmappen med resursväljaren. |
| `viewmode` | sök | | Om du vill starta resursväljaren i sökningsläge, med parametrarna `assettype` och `mimetype`. |
| `assettype` | Bilder, dokument, multimedia, arkiv. | <ul><li>`https://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=images`</li><li> `https://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=documents` </li><li> `https://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=multimedia` </li><li> `https://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=archives` </li></ul> | Använd alternativet för att filtrera resurstyper baserat på angivet värde. |
| `mimetype` | MIME-typ (`/jcr:content/metadata/dc:format`) för en resurs (jokertecken stöds också). | <ul><li>`https://localhost:4502/aem/assetpicker.html?mimetype=image/png`</li><li>`https://localhost:4502/aem/assetpicker.html?mimetype=*png`</li><li>`https://localhost:4502/aem/assetpicker.html?mimetype=*presentation`</li><li>`https://localhost:4502/aem/assetpicker.html?mimetype=*presentation&mimetype=*png`</li></ul> | Använd det för att filtrera resurser baserat på MIME-typ. |

Gå till `https://[aem_server]:[port]/aem/assetpicker` om du vill komma åt resursväljargränssnittet. Navigera till önskad mapp och markera en eller flera resurser. Du kan också söka efter den önskade resursen i rutan Sök, tillämpa det filter som behövs och sedan markera den.

![Bläddra och markera resurs i resursväljaren](assets/assetpicker.png)

*Figur: Bläddra och markera resurs i resursväljaren.*

## Begränsningar {#limitations}

Sökfunktionen i [!DNL Experience Manager Assets] har följande begränsningar:

* Ange inget radavståndsutrymme i sökfrågan, annars fungerar inte sökningen.
* [!DNL Experience Manager] kan fortsätta att visa söktermen efter att du har valt egenskaper för en resurs bland sökresultaten och sedan avbryta sökningen. <!-- (CQ-4273540) -->
* När du söker efter mappar, filer och mappar kan sökresultaten inte sorteras efter någon parameter.
* Om du väljer `Return` utan att skriva in Omnissearch bar returnerar [!DNL Experience Manager] bara en lista över filer och inte mappar. Om du söker efter mappar specifikt utan att använda ett nyckelord returnerar [!DNL Experience Manager] inga resultat.
* Du kan utföra fulltextsökning på mappar. Ange ett sökord som sökningen ska fungera med.

Visuell sökning eller likhetssökning har följande begränsningar:

* Visuell sökning fungerar bäst med en stor databas. Även om det inte finns något minsta antal bilder som krävs för bra resultat är kvaliteten på matchningar med ett fåtal bilder inte lika bra som matchningar från en stor databas.
* Du kan inte ändra modellen eller tåget [!DNL Experience Manager] för att hitta liknande bilder. Modellen ändras inte om du till exempel lägger till eller tar bort smarta taggar för ett fåtal resurser. Resurserna tas inte med i de visuellt liknande sökresultaten.

Sökfunktionen kan ha prestandabegränsningar i följande scenarier:

* Kortvyn har en snabbare inläsningstid jämfört med listvyn för att visa sökresultaten.

## Söktips {#tips}

* När du övervakar granskningsstatusen för resurser ska du använda lämpligt alternativ för att hitta vilka resurser som är godkända eller vilka resurser som väntar på godkännande.
* Använd Insights-predikatet för att söka efter resurser som stöds baserat på användningsstatistik från olika Creative-program. Användningsdata grupperas under Användningspoäng, Impressions, Clicks och Media-kanaler där resurserna visas i kategorier.
* Använd kryssrutan **[!UICONTROL Select All]** för att välja de sökda resurserna. [!DNL Experience Manager] visar först 100 resurser i kortvyn och 200 resurser i listvyn. Fler resurser läses in när du bläddrar i sökresultaten. Du kan välja fler resurser än de inlästa resurserna. Antalet markerade resurser visas i det övre högra hörnet på sökresultatsidan. Du kan arbeta med markeringen, till exempel hämta de markerade resurserna, uppdatera metadataegenskaperna i grupp för de markerade resurserna eller lägga till de markerade resurserna i en samling. När fler resurser är markerade än vad som visas tillämpas en åtgärd antingen på alla markerade resurser eller så visas antalet resurser som åtgärden används på i en dialogruta. Om du vill använda en åtgärd på de resurser som inte lästes in kontrollerar du att alla resurser är uttryckligen markerade.
* Om du vill söka efter resurser som inte innehåller obligatoriska metadata läser du [obligatoriska metadata](#mandatorymetadata).
* Alla metadatafält används för sökningen. En allmän sökning, som att söka efter 12, ger vanligtvis många resultat. För att få bättre resultat bör du använda dubbla (inte enkla) citattecken eller se till att talet ligger intill ett ord utan specialtecken (till exempel `shoe12`).
* Fulltextsökning stöder operatorer som `-` och `^`. Om du vill söka efter de här bokstäverna som stränglitteraler omger du sökuttrycket med citattecken. Använd till exempel `"Notebook - Beauty"` i stället för `Notebook - Beauty`.
* Om sökresultaten är för många begränsar du [omfattningen av sökningen](#scope) till noll för de önskade resurserna. Det fungerar bäst om du har en aning om hur du ska söka efter de önskade resurserna, till exempel en viss filtyp, en viss plats, specifika metadata och så vidare.

* **Taggning**: Med taggar kan du kategorisera resurser som du kan bläddra bland och söka efter mer effektivt. Taggning hjälper till att sprida rätt taxonomi till andra användare och arbetsflöden. [!DNL Experience Manager] erbjuder metoder för att automatiskt tagga resurser med hjälp av Adobe Sensei artificiellt intelligenta tjänster som hela tiden blir bättre på att tagga dina resurser med användning och utbildning. När du söker efter resurser tas smarta taggar med i beräkningen om funktionen är aktiverad på ditt konto. Det fungerar tillsammans med de inbyggda sökfunktionerna. Se [sökbeteende](#searchbehavior). Om du vill optimera den ordning i vilken sökresultaten visas kan du [öka sökrankningen](#searchrank) för ett fåtal utvalda resurser.

* **Indexering**: Endast indexerade metadata och resurser returneras i sökresultaten. För bättre täckning och prestanda bör du se till att indexeringen är korrekt och följa bästa praxis. Se [indexering](#searchindex).

* Om du vill exkludera specifika resurser från sökresultaten använder du egenskapen `excludedPath` i Lucene-indexet.

## Några exempel som illustrerar sökning {#samples}

Använd citattecken runt nyckelord för att hitta resurser som innehåller den exakta frasen i exakt den ordning som anges av användaren.

![Sökbeteende med och utan citattecken](assets/search_with_quotes.gif)

*Bild: Sökbeteende med och utan citattecken.*

**Sök med asterisk-jokertecken**: Om du vill bredda sökningen använder du en asterisk före eller efter sökordet för att matcha ett valfritt antal tecken. Om du till exempel söker efter en körning utan asterisk returneras inga resurser som innehåller någon variant av ordet (inklusive i metadata). En asterisk ersätter ett valfritt antal tecken. Exempel:

* `run` returnerar resurser med exakt nyckelord
* `run*` returnerar resurser med `running`, `run`, `runaway` och så vidare.
* `*run` returnerar resurser med `outrun`, `rerun` och så vidare.
* `*run*` returnerar alla möjliga kombinationer.

![Illustration use of asterisk wildcard in asset search using an example](assets/search_with_asterisk_run.gif)

*Bild: Illustrerar användningen av jokertecken för asterisk i resurssökning med hjälp av ett exempel.*

**Sökning med frågetecknet jokertecken**: Om du vill bredda sökningen använder du en eller flera ? tecken som matchar det exakta antalet tecken. I följande bild:

* `run???`-frågan matchar inte någon resurs.

* `run????`-frågan matchar ordet `running` med fyra tecken efter `run`.

* `??run`-frågan matchar ordet `rerun` med två tecken före `run`.

![Illustrerar användningen av frågetecken med jokertecken i resurssökning med hjälp av ett exempel](assets/search_with_questionmark_run.gif)

*Bild: Illustrerar användningen av frågetecknet jokertecken i resurssökning med hjälp av ett exempel.*

**Uteslut ett nyckelord**: Använd bindestreck för att söka efter resurser som inte innehåller något nyckelord. `running -shoe`-frågan returnerar till exempel resurser som innehåller `running`, men inte `shoe`. På samma sätt returnerar `camp -night`-frågan resurser som innehåller `camp` men inte `night`. Frågan `camp-night` returnerar resurser som innehåller både `camp` och `night`.

![Användning av bindestreck för att söka efter resurser som inte innehåller ett exkluderat nyckelord](assets/search_dash_exclude_keyword.gif)

*Bild: Användning av bindestreck för att söka efter resurser som inte innehåller ett exkluderat nyckelord.*

## Konfigurations- och administrationsuppgifter som rör sökfunktioner {#configadmin}

### Sök i indexkonfigurationer {#searchindex}

Resursidentifiering bygger på indexering av DAM-innehåll, inklusive metadata. Snabbare och exaktare tillgångsidentifiering bygger på optimerad indexering och lämpliga konfigurationer. Se [sökindex](/help/assets/performance-tuning-guidelines.md#search-indexes), [sök frågor och indexera](/help/sites-deploying/queries-and-indexing.md) och [bästa praxis](/help/sites-deploying/best-practices-for-queries-and-indexing.md).

Om du vill exkludera specifika resurser från sökresultaten använder du egenskapen `excludedPath` i Lucene-indexet.

### Visuell sökning eller likhetssökning {#configvisualsearch}

Visuell sökning använder smarta taggar. Följ de här stegen när du har konfigurerat funktionen för smart taggning.

1. I [!DNL Experience Manager] CRXDE, i noden `/oak:index/lucene`, lägger du till följande egenskaper och värden och sparar ändringarna.

   * Egenskapen `costPerEntry` av typen `Double` med värdet `10`.
   * Egenskapen `costPerExecution` av typen `Double` med värdet `2`.
   * Egenskapen `refresh` av typen `Boolean` med värdet `true`.

   Den här konfigurationen tillåter sökningar från lämpligt index.

1. Skapa en nod med namnet `imageFeatures` av typen `nt-unstructured` i CRXDE vid `/oak:index/damAssetLucene/indexRules/dam:Asset/properties` om du vill skapa Lucene-index. I noden `imageFeatures`,

   * Lägg till egenskapen `name` av typen `String` med värdet `jcr:content/metadata/imageFeatures/haystack0`.
   * Lägg till egenskapen `nodeScopeIndex` av typen `Boolean` med värdet `true`.
   * Lägg till egenskapen `propertyIndex` av typen `Boolean` med värdet `true`.
   * Lägg till egenskapen `useInSimilarity` av typen `Boolean` med värdet `true`.

   Spara ändringarna.

1. Få åtkomst till `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predictedTags` och lägg till egenskapen `similarityTags` av typen `Boolean` med värdet `true`.
1. Använd smarta taggar för resurserna i din [!DNL Experience Manager]-databas. Se [Konfigurera smarta taggar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/tagging.html?lang=sv-SE#configuring).
1. I CRXDE, i noden `/oak-index/damAssetLucene`, ställer du in egenskapen `reindex` på `true`. Spara ändringarna.
1. (Valfritt) Om du har anpassat sökformulär kopierar du noden `/libs/settings/dam/search/facets/assets/jcr%3Acontent/items/similaritysearch` till `/conf/global/settings/dam/search/facets/assets/jcr:content/items`. Spara ändringarna.

Mer information finns i [Mer information om smarta taggar i Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html?lang=sv-SE) och [hur du hanterar smarta taggar](/help/assets/enhanced-smart-tags.md).

>[!CAUTION]
>
>Om Lucene-indexeringen görs av [!DNL Adobe Experience Manager] fungerar inte sökningen baserat på smarta taggar som förväntat.

### Obligatoriska metadata {#mandatorymetadata}

Affärsanvändare, administratörer och DAM-bibliotek kan definiera vissa metadata som obligatoriska metadata som är ett måste för att affärsprocesserna ska fungera. Av olika anledningar kan vissa resurser sakna dessa metadata, t.ex. äldre resurser eller resurser som migrerats i grupp. Assets med saknade eller ogiltiga metadata identifieras och rapporteras baserat på den indexerade metadataegenskapen. Mer information om hur du konfigurerar den finns i [obligatoriska metadata](/help/assets/metadata-schemas.md#define-mandatory-metadata).

### Ändra sökfaktorer {#searchfacets}

[!DNL Experience Manager Assets] erbjuder sökfaktorer som du kan använda för att filtrera sökresultaten, vilket förbättrar upptäcktens hastighet. Panelen Filter innehåller som standard några standardaspekter. Administratörer kan anpassa filterpanelen för att ändra standardmetoderna med hjälp av inbyggda predikat. [!DNL Experience Manager] innehåller en bra samling inbyggda predikat och en redigerare för att anpassa ansiktena. Se [sökfaktorer](/help/assets/search-facets.md).

### Extrahera text när du överför resurser {#extracttextupload}

Du kan konfigurera [!DNL Experience Manager] så att texten extraheras från resurserna när användare överför resurser, till exempel PSD- eller PDF-filer. [!DNL Experience Manager] indexerar den extraherade texten och hjälper användarna att söka efter dessa resurser baserat på den extraherade texten. Se [Överför resurser](/help/assets/manage-assets.md#uploading-assets).

Om textraheringen blir för resurskrävande för din distribution bör du överväga att [inaktivera textrahering](https://helpx.adobe.com/experience-manager/kb/Disable-binary-text-extraction-to-optimize-Lucene-indexing-AEM.html).

### Anpassade predikat för att filtrera sökresultat {#custompredicates}

Predikat används för att skapa ansikten. Administratörer kan anpassa sökmetoderna på panelen Filter med förkonfigurerade predikat. Dessa predikat kan anpassas med hjälp av övertäckningar. Se [skapa anpassade predikat](/help/assets/searchx.md).

Du kan söka efter digitala resurser baserat på en eller flera av följande egenskaper. Filter som gäller för vissa av dessa egenskaper är som standard tillgängliga och vissa andra filter kan skapas för att användas på andra egenskaper.

| Sökfält | Sök egenskapsvärden |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|
| MIME-typer | Bilder, Dokument, Multimedia, Arkiv eller Annat. |
| Senast ändrad | Timme, dag, vecka, månad eller år. |
| Filstorlek | Liten, Medium eller Stor. |
| Publiceringsstatus | Publicerad eller opublicerad. |
| Godkänd status | Godkänd eller Avvisad. |
| Orientering | Vågrät, Lodrät eller Fyrkant. |
| Stil | Färg eller Svartvitt. |
| Videohöjd | Anges som lägsta och högsta värde. Värdet lagras endast i metadata för videoåtergivningar. |
| Videobredd | Anges som lägsta och högsta värde. Värdet lagras endast i metadata för videoåtergivningar. |
| Videoformat | DVI, Flash, MPEG4, MPEG, OGG Theora, QuickTime, Windows Media. Värdet lagras i källvideons metadata och eventuella återgivningar. |
| Videokodek | x264. Värdet lagras endast i metadata för videoåtergivningar. |
| Videobithastighet | Anges som lägsta och högsta värde. Värdet lagras endast i metadata för videoåtergivningar. |
| Ljudkodek | Libvorbis, Lame MP3, AAC-kodning. Värdet lagras endast i metadata för videoåtergivningar. |
| Bithastighet för ljud | Anges som lägsta och högsta värde. Värdet lagras endast i metadata för videoåtergivningar. |

## Arbeta med resurssökningsresultat {#aftersearch}

Du kan göra följande med de resurser du har sökt i [!DNL Experience Manager]:

* Visa metadataegenskaper och annan information.
* Hämta en eller flera resurser.
* Använd Skrivbordsåtgärder för att öppna resurserna i skrivbordsappen.
* Skapa smarta samlingar.
* Skapa en version
* Starta ett arbetsflöde
* Relatera eller inte relatera tillgångar
* Använd filter på panelen Filter som visas automatiskt när du har gjort sökningen för att begränsa sökresultaten.

### Sortera sökresultat {#sort}

Sortera sökresultaten för att hitta de resurser som behövs snabbare. Du kan sortera sökresultaten i listvyn och endast när du väljer **[[!UICONTROL Files]](#searchui)** på panelen **[!UICONTROL Filters]**. [!DNL Assets] använder sortering på serversidan för att snabbt sortera alla resurser (oavsett hur många) i en mapp eller resultaten av en sökfråga. Sortering på serversidan ger snabbare och exaktare resultat än sortering på klientsidan.

I listvyn kan du sortera sökresultaten på samma sätt som du kan sortera resurser i valfri mapp. Sortering fungerar för de här kolumnerna - Namn, Titel, Status, Dimensioner, Storlek, Klassificering, Användning, Skapad, Ändrad, Publicerad, Arbetsflöde och Utcheckad.

Information om begränsningar för sorteringsfunktioner finns i [begränsningar](#limitations).

### Kontrollera detaljerad information om en resurs {#checkinfo}

Du kan kontrollera detaljerad information om en sökresurs från sökresultatsidan.

Om du vill visa alla metadata för en resurs markerar du resursen och klickar på **[!UICONTROL properties]** i verktygsfältet.

Om du vill kontrollera kommentarerna för en resurs eller versionshistoriken för en resurs klickar du på resursen för att öppna en stor förhandsvisning. Öppna tidslinjen i den vänstra rutan och välj **[!UICONTROL Comments]** eller **[!UICONTROL Versions]**. Du kan också sortera tidslinjeaktiviteter, som kommentarer eller versioner, i kronologisk ordning.

![Sortera tidslinjeposter för en sökresurs](assets/sort_timeline_search_results.gif)

*Figur: Sortera tidslinjeposter för en sökresurs.*

### Hämta sökbara resurser {#download}

Du kan hämta de sökda resurserna och deras återgivningar på samma sätt som du hämtar vanliga resurser från mappar. Välj en eller flera resurser från sökresultaten och klicka på **[!UICONTROL Download]** i verktygsfältet.

### Uppdatera metadataegenskaper gruppvis {#metadataupdates}

Det går att göra satsvisa uppdateringar av de gemensamma metadatafälten för flera resurser. Välj en eller flera resurser från sökresultaten. Klicka på **[!UICONTROL Properties]** i verktygsfältet och uppdatera metadata efter behov. Klicka på **[!UICONTROL Save and Close]** när du är klar. De befintliga metadata i de uppdaterade fälten skrivs över.

För resurser som är tillgängliga i en enda mapp eller en samling är det enklare att [uppdatera metadata i grupp](/help/assets/metadata.md) utan att använda sökfunktionen. För resurser som är tillgängliga i olika mappar eller matchar ett gemensamt villkor är det snabbare att uppdatera metadata i grupp via sökning.

### Smarta samlingar {#smart-collections}

En samling är en ordnad uppsättning resurser som kan innehålla resurser från olika platser, eftersom samlingar bara innehåller referenser till dessa resurser. Samlingar är av följande två typer:

* En statisk referenslista med resurser, mappar och andra samlingar.
* En dynamisk lista (smart samling) som fyller i resurser i samlingen baserat på sökvillkor.

Du kan skapa smarta samlingar baserat på sökvillkoren. På panelen **[!UICONTROL Filters]** väljer du **[!UICONTROL Files]** och klickar på **[!UICONTROL Save Smart Collection]**. Läs mer i [Hantera samlingar](/help/assets/manage-collections.md).

### Skapa en version {#create-version}

Skapa en version för resurserna som visas i sökresultaten. Markera resursen och klicka på **[!UICONTROL Create]** > **[!UICONTROL Version]**. Lägg till en valfri etikett eller kommentar och klicka på **[!UICONTROL Create]**. Du kan också välja flera resurser och skapa versioner för dem samtidigt.

### Skapa ett arbetsflöde {#create-workflow}

På samma sätt som när du skapar en version kan du också skapa ett arbetsflöde för resurserna som visas i sökresultaten. Markera resurserna och klicka på **[!UICONTROL Create]** > **[!UICONTROL Workflow]**. Välj arbetsflödesmodellen, ange en rubrik för arbetsflödet och klicka på **[!UICONTROL Start]**.

### Relatera och inte relatera tillgångar {#relate-unrelate-assets}

Relatera och dela upp resurser som visas i sökresultaten. Markera resurserna och klicka på **[!UICONTROL Relate]** eller **[!UICONTROL Unrelate]**.

## Oväntade sökresultat och problem {#unexpected-results}

| Fel, problem, symtom | Möjlig orsak | Möjlig korrigering eller förståelse för problemet |
|---|---|---|
| Felaktiga resultat vid sökning efter resurser som saknar metadata. | När du söker efter resurser som saknar obligatoriska metadata kan [!DNL Experience Manager] visa vissa resurser som har giltiga metadata. Resultatet baseras på indexerad metadataegenskap. | När metadata har uppdaterats krävs omindexering för att resursens metadata ska visas korrekt. Se [obligatoriska metadata](metadata-schemas.md#define-mandatory-metadata). |
| För många sökresultat. | En stor sökparameter. | Överväg att begränsa [omfattningen av sökningen](#scope). Smarta taggar kan ge fler sökresultat än du förväntade dig. Se [sökbeteende med smarta taggar](#withsmarttags). |
| Orelaterade eller delvis relaterade sökresultat. | Sökbeteendet ändras med smart taggning. | [Förstå hur sökningen ändras efter smart taggning](#withsmarttags). |
| Inga förslag för resurser som fylls i automatiskt. | Nyligen överförda resurser har ännu inte indexerats. Metadata är inte omedelbart tillgängliga som förslag när du börjar skriva ett söknyckelord i omsökningsfältet. | [!DNL Experience Manager] väntar tills en timeout-period har gått ut (en timme som standard) innan ett bakgrundsjobb körs för att indexera metadata för alla nyligen överförda eller uppdaterade resurser och lägger sedan till metadata i listan med förslag. |
| Inga sökresultat. | <ul><li>Assets som matchar din fråga finns inte. </li><li> Blanksteg har lagts till före sökfrågan. </li><li> Det metadatafält som inte stöds innehåller nyckelordet som du sökte efter.</li><li> Sökningar som görs när en resurs är ledig. </li></ul> | <ul><li>Sök med ett annat nyckelord. Du kan också använda smart taggning eller likhetssökning för att förbättra sökresultaten. </li><li>[Känd begränsning](#limitations).</li><li>Alla metadatafält används inte för sökningar. Se [scope](#scope).</li><li>Sök senare eller ändra i tid och offline för att hitta de resurser som behövs.</li></ul> |
| Sökfilter eller predikat är inte tillgängligt. | <ul><li>Sökfiltret är inte konfigurerat.</li><li>Den är inte tillgänglig för din inloggning.</li><li>(Sannolikheten är mindre) Sökalternativen är inte anpassade efter den distribution du använder.</li></ul> | <ul><li>Kontakta administratören för att kontrollera om sökanpassningarna är tillgängliga eller inte.</li><li>Kontakta administratören för att kontrollera om ditt konto har behörighet att använda anpassningen.</li><li>Kontakta administratören och kontrollera tillgängliga anpassningar för den [!DNL Assets]-distribution som du använder.</li></ul> |
| När du söker efter visuellt liknande bilder saknas en förväntad bild. | <ul><li>Bilden är inte tillgänglig i [!DNL Experience Manager].</li><li>Bilden är inte indexerad. Vanligtvis när den nyligen har överförts.</li><li>Bilden är inte smart taggad.</li></ul> | <ul><li>Lägg till bilden i [!DNL Assets].</li><li>Kontakta administratören om du vill indexera om databasen. Se även till att du använder rätt index.</li><li>Kontakta administratören om du vill tagga de relevanta resurserna på ett smart sätt.</li></ul> |
| När du söker efter visuellt liknande bilder visas en irrelevant bild. | Visuell sökfunktion. | [!DNL Experience Manager] visar så många potentiellt relevanta resurser som möjligt. Mindre relevanta bilder, om sådana finns, läggs till i resultatet men med en lägre sökrankning. Kvaliteten på matchningarna och relevansen hos de sökda resurserna minskar när du bläddrar nedåt i sökresultaten. |
| När du väljer och arbetar med sökresultat utförs inte alla sökbara resurser. | Alternativet [!UICONTROL Select All] väljer bara de första 100 sökresultaten i kortvyn och de första 200 sökresultaten i listvyn. | |

>[!MORELIKETHIS]
>
>* [[!DNL Experience Manager] sökimplementeringsguide](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/search-tutorial-develop.html?lang=sv-SE)
>* [Avancerad konfiguration som förbättrar sökresultaten](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search-boost.html?lang=sv-SE)
>* [Konfigurera smart översättningssökning](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/translation/smart-translation-search-technical-video-setup.html)
