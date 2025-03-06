---
title: Anpassa mallar för Forms Portal-komponenter
description: Läs om hur man i AEM Forms användargränssnitt lägger in metadata i blanketter. Anpassade metadata förbättrar användarupplevelsen när det gäller att lista och söka i formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: customization
docset: aem65
feature: Forms Portal
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 3eb9c0e3-950e-4dd5-a4c9-2d8f486ea3cf
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1242'
ht-degree: 0%

---

# Anpassa mallar för Forms Portal-komponenter{#customizing-templates-for-forms-portal-components}

## Förutsättningar {#prerequisites}

[Hantera formulärmetadata](../../forms/using/manage-form-metadata.md)

Arbetskunskaper i HTML och CSS

## Ökning {#overview}

Med AEM Forms användargränssnitt kan du lägga till metadata i alla formulär. Anpassade metadata kan förbättra användarupplevelsen samtidigt som du listar och söker efter formulär i organisationen.

Med Forms Portal kan du använda anpassade metadata i formulärlistor. När du skapar anpassade mallar för resurser kan du ändra deras layout och använda anpassade metadata med din CSS-formatuppsättning.

Gör följande så att du kan skapa en anpassad mall för olika Forms Portal-komponenter.

## Skapa en anpassad mall {#creating-a-nbsp-custom-template}

1. Skapa en slinga:Mappnod under /apps

   Lägg till egenskapen fpContentType. Ange lämpliga värden för egenskapen beroende på vilken komponent du definierar den anpassade mallen för.

   * Search &amp; Lister component: &quot;/libs/fd/fp/formTemplate&quot;
   * Komponenten Utkast och inskickat material:

      * Avsnittet Utkast: /libs/fd/fp/draftTemplate
      * Inlämningsavsnitt: /libs/fd/fp/sendingTemplate

   * Länkkomponent: /libs/fd/fp/linkTemplate

   Lägg till en titel som du vill ska visas när du väljer layoutmallar.

   >[!NOTE]
   >
   >Titeln kan skilja sig från nodnamnet för sling:Mapp som du skapade.

   I följande bild visas konfigurationen för komponenten Sök och Lister.
   ![Skapa en sling:Mapp](assets/1.png)

1. Skapa en filmall.html i den här mappen så att den kan fungera som en anpassad mall.
1. Skriv den anpassade mallen och använd anpassade metadata enligt beskrivningen nedan.

## Exempel {#working-example}

Nedan följer ett exempel på implementering av en anpassad mall där Forms Portal förvärvar en anpassad Geometrixx Gov Card-layout för komponenten Sök och Lister.

```html
<div class="__FP_boxes-container __FP_single-color">
    <div class="boxes __FP_boxes __FP_single-color" data-repeatable="true">
 <div class="__FP_boxes-thumbnail">
     <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
        </div>
        <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
        <p>${description}</p>
        <div class="boxes-icon-cont __FP_boxes-icon-cont">
            <div class="op-dow">
                <a href="${formUrl}" target="_blank" class="__FP_button ${htmlStyle}" title="${config-htmlLinkText}">${localize-Apply}</a>
                <a href="${pdfUrl}" class="__FP_button ${pdfStyle}" title="${config-pdfLinkText}">${localize-Download}</a>
            </div>
        </div>
    </div>
</div>
```

## Tekniska specifikationer för anpassade mallar {#technical-specifications-for-custom-templates}

En anpassad mall för valfri Forms Portal-komponent innehåller repeterbara och icke-repeterbara poster. Repeterbara poster är grundläggande enheter som ska tas upp i förteckningen. Exempel på upprepningsbara poster är komponenterna Sök och Lister, Utkast och överföringar samt Länka.

Forms Portal innehåller en syntax för platshållare som kan visa anpassade/färdiga metadata. Platshållarna fylls i när resultatet av formulär, utkast eller inskickade formulär visas.

Om du vill ta med en repeterbar post konfigurerar du värdet för attributet **data-repetable** till **true**.

*I exemplet som behandlas finns två Div-element högst upp i den anpassade mallen. Den första, med CSS-klassen&quot;__FP_boxes-container&quot;, fungerar som ett behållarelement för de formulär som visas. Den andra, med CSS-klassen&quot;__FP_boxes&quot;, är en mall för de grundläggande entiteterna, i det här fallet ett formulär. Attributet **data-repetable**i Div-elementet har värdet **true**.*

Varje platshållare har en exklusiv metadatauppsättning som är färdig att användas. Om du vill visa anpassade metadata på en viss plats i formuläret lägger du till egenskapen **${metadata_prop}** på platsen.

*I exemplet används metadataegenskapen i flera instanser. Den används t.ex. i **description**,**name**,**formUrl**,**htmlStyle**,**pdfUrl**,**pdfStyle**och **path**på föreskrivet sätt.*

## Metadata utanför kartongen {#out-of-the-box-metadata}

Olika Forms Portal-komponenter har exklusiva uppsättningar färdiga metadata som du kan använda för att visa upp en lista.

### Komponenten Search &amp; Lister {#search-amp-lister-component}

* **Formulärets namn:**
* **namn**: Formulärets namn (vanligtvis är det samma som titeln)
* **description**: Beskrivning av formuläret
* **formUrl**: URL för att återge formuläret som HTML
* **pdfUrl**: URL för att återge formuläret som PDF
* **assetType**: Typ av resurs. Giltiga värden är **Form**, **PDF Form**, **Print Form** och **Adaptive Form**

* **htmlStyle**&amp; **pdfStyle**: Visningsformat för HTML- respektive PDF-ikoner som används för återgivning. Giltiga värden är **__FP_display_none** eller tomma.

>[!NOTE]
>
>Kom ihåg att använda klassen __FP_display_none i din anpassade formatmall.

* **downloadUrl**: URL för att hämta en resurs.

Stöd för lokalisering, sortering och användning av konfigurationsegenskaper i användargränssnittet (endast sökning och lister):

1. **Lokaliseringsstöd**: Om du vill lokalisera statisk text använder du attributet `${localize-YOUR_TEXT}` och gör det lokaliserade värdet tillgängligt, om det inte redan finns.
   *I det exempel som behandlas används attributen `${localize-Apply}` och `${localize-Download}` för att lokalisera texten Använd och Hämta.*

1. **Stöd för sortering**: Klicka på HTML-elementet om du vill sortera sökresultaten. Om du vill implementera sortering i en tabelllayout lägger du till attributet data-sortKey i den aktuella tabellrubriken. Lägg dessutom till dess värde som de metadata som du vill sortera efter.
För rubrikrubriken i stödrastervyn är värdet för rubriken&quot;data-sortKey&quot; till exempel&quot;title&quot;. Klicka på rubriken så att du kan sortera värdena i en viss kolumn.

1. **Använda konfigurationsegenskaper**: Komponenten Search &amp; Lister har flera konfigurationer som du kan använda i användargränssnittet. Om du till exempel vill visa HTML ToolTip-text som sparats via redigeringsdialogrutan använder du attributet `${config-htmlLinkText}`. **På samma sätt använder du attributet** `${config-pdfLinkText}` för PDF-verktygstipstext.

### Länkkomponent {#link-component}

* **Formulärets namn:**
* **formUrl**: URL för att återge formuläret som HTML
* **target**: Länks målattribut. Giltiga värden är &quot;_blank&quot; och &quot;_self&quot;.
* **linkText**: Länkbeskrivning

### Komponenten Utkast och inskickat material {#drafts-amp-submissions-component}

* **Sökväg**: Sökväg till metadatanoden för utkast/överföringar. Använd det med tillägget .HTML som en URL-adress så att du kan öppna ett utkast eller skicka det.
* **contextPath**: Kontextsökvägen för AEM-instansen
* **firstLetter**: Första bokstaven (versaler) i titeln för det adaptiva formuläret, som sparades som utkast eller skickades.
* **formName**: Titeln på det adaptiva formuläret, som har sparats som utkast eller skickats.
* **draftID**: ID för det utkast som visas (Använd bara i mallen för avsnittet Utkast).
* **submitID**: ID för överföringen som visas (Använd bara i mallen för avsnittet Skicka).
* **status**: Status för det skickade formuläret. (Använd endast i mallen för avsnittet Skicka).
* **description**: Beskrivning av det adaptiva formuläret som är associerat med utkastet eller sändningen.
* **diffTime**: Skillnad mellan aktuell tid och den senaste sparåtgärden för utkastet. Alternativt kan du ange skillnaden mellan den aktuella tiden och den senast skickade åtgärden för överföringen.
* **iconClass**: CSS-klass som används för att visa den första bokstaven i utkastet/överföringen. Forms Portal innehåller följande klasser med olika färgade bakgrunder.
* **ägare**: Användaren som skapade utkastet/överföringen.
* **I dag**: Skapad av utkast eller inskickad i `DD:MM:YYYY`-format.
* **TimeNow**: Tid då utkast eller inskickning skapades i `HH:MM:SS` 24-timmarsformat

*Obs!*

1. Om du vill ta bort alternativet i avsnittet Utkast under komponenten Utkast och överföringar ska du ge CSS-klassen namnet&quot;__FP_deleteDraft&quot;. Inkludera dessutom attributet &quot;draftID&quot; med värdet **${draftID}**, som är utkast-ID för motsvarande utkast.

1. När du skapar länkar till öppna utkast och inskickade dokument kan du ange **${path}.html** som värde för attributet **href** för ankartaggen.

![Noden Utkast och överföring](assets/raw-image-with-index.png)

**A**. Behållarelement

**B.** Sökväg-metadata med en fast hierarki för att hämta den miniatyrbild som lagras för varje formulär.

**C.** Attribut för upprepning av data som används för mallavsnittet för varje formulär

**D.** Localize &quot;Apply&quot; string

**E.** Använda konfigurationsegenskapen pdfLinkText

**F.** Använda pdfUrl-metadata

## Tips, tricks och kända fel {#tips-tricks-and-known-issues}

1. Använd inte enkla citattecken (&#39;) i någon anpassad mall.
1. Om du vill ha anpassade metadata sparar du den här egenskapen endast på noden **jcr:content/metadata** . Om du lagrar det på något annat ställe kan Forms Portal inte visa metadata.
1. Kontrollera att namnet på anpassade metadata eller befintliga metadata inte innehåller ett kolon ( : ). Om det gör det kan du inte visa det i användargränssnittet.
1. **data-repetable** har ingen betydelse för en **Link**-komponent. Adobe rekommenderar att du undviker att använda den här egenskapen i mallen för en Link-komponent.

## Relaterade artiklar

* [Aktivera Forms Portal-komponenter](/help/forms/using/enabling-forms-portal-components.md)
* [Skapa Forms Portal-sida](/help/forms/using/creating-form-portal-page.md)
* [Visa formulär på en webbsida med API:er](/help/forms/using/listing-forms-webpage-using-apis.md)
* [Använda komponenten Utkast och inskickat material](/help/forms/using/draft-submission-component.md)
* [Anpassa lagring av utkast och inskickade formulär](/help/forms/using/draft-submission-component.md)
* [Exempel på hur man integrerar utkast och inskickningskomponenter med databas](/help/forms/using/integrate-draft-submission-database.md)
* [Anpassa mallar för Forms Portal-komponenter](/help/forms/using/customizing-templates-forms-portal-components.md)
* [Introduktion till att publicera formulär på en portal](/help/forms/using/introduction-publishing-forms.md)
