---
title: Introduktion till  [!DNL Adobe Experience Manager Assets]
description: Skapa, hantera, bearbeta och distribuera digitala resurser i Experience Manager. I dessa handledningar beskrivs de effektivaste strategierna, tillgänglighetsfunktionerna och hur du använder AEM 6.5 LTS-resurser.
hide: true
feature: Asset Management
role: Leader, Architect, User
solution: Experience Manager, Experience Manager Assets
exl-id: 2f2eb576-4924-4314-b348-c4b290a57fe3
source-git-commit: aafd340d9ef03f9bf12ed23aad5eb03dc055af38
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 1%

---

# Om [!DNL Adobe Experience Manager Assets] som DAM-lösning {#administering-assets}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/overview) |
| AEM 6.5 | Den här artikeln |

AEM [!DNL Assets] är ett DAM-verktyg (Digital Asset Management) som är en del av [!DNL Experience Manager] -plattformen och gör att ditt företag kan hantera och distribuera digitala resurser. Användare i en hel organisation kan hantera, lagra och få tillgång till många typer av digitalt material som bilder, videor, dokument, ljudklipp, 3D-filer och multimedia för användning på webben, i tryck och för digital distribution.

## Vad är Digital Asset Management? {#what-is-digital-asset-management}

[!DNL Assets] ger företagsövergripande delning och distribution av en organisations viktigaste digitala resurser. Användare i en hel organisation kan lagra, hantera och komma åt digitala resurser som bilder, grafik, ljud, video och dokument via ett webbgränssnitt (eller en CIFS- eller WebDAV-mapp).

[!DNL Assets]-funktionen i [!DNL Experience Manager] gör följande:

* Lägg till och dela bilder, dokument, ljudfiler och videofiler i olika filformat.
* Hantera resurser genom att gruppera dem efter taggar, ljusbord eller stjärnor (dina favoriter). Lägg till anteckningar i resurser.
* Sök efter resurser genom att söka efter filnamn, den fullständiga texten i dokument och genom att söka efter datum, dokumenttyp och taggar.
* Lägg till eller redigera metadatainformation för resurser. Metadata versionshanteras automatiskt tillsammans med motsvarande resurs. Du kan importera eller exportera metadata för resurser.
* Utför bildredigeringsfunktioner som skalning och lägga till bildfilter. Importera och exportera flera digitala resurser samtidigt med en WebDAV- eller CIFS-mapp.
* Använd arbetsflöden och meddelanden för att möjliggöra gemensam bearbetning och hämtning av alla typer av resurser och hantera åtkomsträttigheter till resurser.

### [!DNL Experience Manager Assets] är integrerat med [!DNL Experience Manager Sites] {#aem-assets-fully-integrated-in-cq-wcm}

[!DNL Assets] är helt integrerat med [!DNL Sites] och fungerar sömlöst för alla användningsfall. När författarna till [!DNL Sites] skapar webbsidor kan de till exempel hitta och använda de digitala resurserna via Innehållssökning. Användargränssnittet för [!DNL Assets] är detsamma som för [!DNL Sites]. Mer information finns i [översikten över platser](/help/sites-authoring/page-authoring.md).

Det grundläggande användargränssnittet är samma som för [!DNL Sites]. Mer information finns i [Översikt över webbplatserna](/help/sites-authoring/page-authoring.md).

### Digital Asset Management jämfört med bildkomponent {#digital-asset-management-versus-image-component}

När du avgör om en bild ska placeras i DAM-databasen eller om en bildkomponent ska användas, bör du tänka på bildens livscykel:

* Om bilden har samma livscykel som sidan använder du bildkomponenten.
* Om bilden har en separat livscykel, till exempel om du använder bilden två gånger eller utanför WCM, använder du [!DNL Assets].

## Vad är digitala resurser? {#what-are-digital-assets}

En resurs är ett digitalt dokument, en bild, en video eller ett ljud (eller en del av ett) som kan ha flera återgivningar. Den kan också innehålla underresurser, till exempel lager i en Photoshop-fil, bilder i en PowerPoint-fil, sidor i en PDF-fil, filer i en ZIP-fil.

En resurs är i stort sett en binär resurs plus metadata plus återgivningar plus underresurser. Mer information finns i [DAM-prestandaguiden](/help/sites-deploying/assets-performance-sizing.md).

>[!CAUTION]
>
>Överföring och/eller redigering av en stor mängd resurser (särskilt bilder) kan påverka prestanda för din [!DNL Experience Manager]-distribution.

### [!DNL Experience Manager Assets]-terminologi {#aem-assets-terminology}

När du arbetar med digitala resurser i [!DNL Experience Manager] är det bra om du förstår följande terminologi:

* **Samling**: En samling resurser, antingen baserat på fysisk plats (mapp), gemensamma egenskaper (sparad sökmapp) eller användarval (lightbox-mappar).

* **Metadata** [!DNL Assets] har metadata, till exempel författare, förfallodatum och DRM-information (Digital Rights Management). Metadata är under åtkomstkontroll. [!DNL Assets] har stöd för följande vanliga metadatascheman:

   * Dublin Core: inklusive författare, beskrivning, datum, ämne och så vidare.
   * IPTC: inkludera händelse, modell, plats och så vidare.
   * WCM: inkluderar sidegenskaper, [!UICONTROL On Time] och [!UICONTROL Off Time] osv.

* **Taggning**: [!DNL Assets] kan taggas och klassificeras. Se [ordna resurser](/help/assets/organize-assets.md).

* **Återgivningar**: En återgivning är den binära återgivningen av en resurs. [!DNL Assets] har alltid en primär representation - den för den överförda filen. De kan ha valfritt antal ytterligare representationer som skapas, till exempel genom anpassade arbetsflödessteg eller när en resurs överförs. Återgivningar kan ha en annan storlek, med en annan upplösning, med en vattenstämpel eller någon annan förändrad egenskap.

* **Versioner**: Versionshantering skapar en ögonblicksbild av digitala resurser vid en viss tidpunkt. Du kan återställa resurser till tidigare versioner. Se [versionshantering i [!DNL Assets]](manage-assets.md#asset-versioning).

* **Delresurser**: Delresurser är resurser som utgör en resurs, till exempel lager i en [!DNL Adobe Photoshop] -fil eller sidor i en PDF-fil. I [!DNL Assets] kan du hantera underresurser på samma sätt som du hanterar resurser.

### Så här arbetar du med digitala resurser {#how-to-work-with-assets}

Du utför en åtgärd på en resurs eller samling. Funktionsmakron kan skapa och ändra resurser, samlingar och återgivningar. Många av de grundläggande åtgärder du utför på resurser - överföra, ta bort, uppdatera, spara underresurser - utlöser förkonfigurerade arbetsflöden. Dessa aktiveras automatiskt i [!DNL Assets] och beskrivs mer ingående i [!DNL Assets] mediehanterare.

De uppgifter du kan utföra med dessa förkonfigurerade arbetsflöden:

* Spara resursen i eller ta bort resursen från databasen.
* Extrahera och spara metadata för resursen. De enskilda metadataobjekten sparas som XMP.
* Generera återgivningar och miniatyrbilder för resursen, inklusive automatisk storleksändring och beskärning vid behov.
* Omkoda tillgången där det behövs. Video för mobil- och webbanvändning omkodas till exempel med 24 bildrutor per sekund, och video hämtas med 30 bildrutor per sekund. Ljud för mobil- och webbanvändning omkodas med 128 kbit/s, ljud för nedladdning med 192 kbit/s.

Du kan även använda arbetsflöden manuellt. En lista med standardarbetsflöden finns i [Assets Media Handlers](media-handlers.md).

## [!DNL Experience Manager Assets] och [!DNL Media Library] {#cq-dam-vs-cq-medialibrary}

Mer information om skillnaderna finns i [Assets och mediebibliotek](medialibrary.md).

>[!MORELIKETHIS]
>
>* [Videointroduktion - Experience Manager Assets som modern DAM](https://www.youtube.com/watch?v=PBwQqZgC-yo)
>* [Förstå metadatabegrepp](/help/assets/metadata-concepts.md)
