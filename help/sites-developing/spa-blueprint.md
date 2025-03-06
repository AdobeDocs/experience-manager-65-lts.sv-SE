---
title: SPA Blueprint
description: I det här dokumentet beskrivs det allmänna, ramverksoberoende kontraktet som alla SPA-ramverk ska uppfylla för att implementera redigerbara SPA-komponenter inom AEM.
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: spa
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing,SPA Editor
role: Developer
exl-id: 798b9ad8-47fa-432d-8887-9de63c20cfca
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2023'
ht-degree: 0%

---

# SPA Blueprint{#spa-blueprint}

Om du vill att författaren ska kunna använda AEM SPA Editor för att redigera innehållet i ett SPA, finns det krav som SPA måste uppfylla, vilket beskrivs i det här dokumentet.

{{ue-over-spa}}

## Introduktion {#introduction}

I det här dokumentet beskrivs det allmänna kontraktet att alla SPA-ramverk ska uppfylla (dvs. den typ av AEM-supportskikt) för att implementera redigerbara SPA-komponenter i AEM.

>[!NOTE]
>
>Följande krav är ramverksoberoende. Om dessa krav är uppfyllda kan ett ramverksspecifikt lager bestående av moduler, komponenter och tjänster tillhandahållas.
>
>**Dessa krav är redan uppfyllda för ramverken React och Angular i AEM.** Kraven i den här planen är bara relevanta om du vill implementera ett annat ramverk för användning med AEM.

>[!CAUTION]
>
>Även om SPA-funktionerna i AEM är ramverksoberoende stöds för närvarande bara ramverken React och Angular.

Om du vill att författaren ska kunna använda AEM Page Editor för att redigera data som exponeras av ett ramverk för ett enkelsidigt program, måste ett projekt kunna tolka modellstrukturen som representerar semantiken för data som lagras för ett program i AEM-databasen. För att uppnå det här målet finns två ramverksbaserade bibliotek: `PageModelManager` och `ComponentMapping`.

### PageModelManager {#pagemodelmanager}

Biblioteket `PageModelManager` tillhandahålls som ett NPM-paket som ska användas av ett SPA-projekt. Det medföljer SPA och fungerar som en datamodellshanterare.

För SPA:s räkning tar det bort och hanterar JSON-strukturen som representerar den faktiska innehållsstrukturen. Den ansvarar också för synkroniseringen med SPA för att meddela när komponenterna ska återges på nytt.

Se NPM-paketet [@adobe/aem-spa-page-model-manager](https://www.npmjs.com/package/@adobe/aem-spa-page-model-manager)

När `PageModelManager` initieras läser biblioteket först in den angivna rotmodellen för appen (via parameter, metaegenskap eller aktuell URL). Om biblioteket identifierar att den aktuella sidans modell inte är en del av den rotmodell som hämtas, och tar med den som modell för en underordnad sida.

![page_model_conversion](assets/page_model_consolidation.png)

### ComponentMapping {#componentmapping}

Modulen `ComponentMapping` tillhandahålls som ett NPM-paket till frontendprojektet. Det lagrar komponenter i gränssnittet och erbjuder ett sätt för SPA att mappa komponenter i gränssnittet till AEM-resurstyper. Detta aktiverar en dynamisk upplösning för komponenter när JSON-modellen för programmet analyseras.

Varje objekt i modellen innehåller ett `:type`-fält som visar en AEM-resurstyp. När den är monterad kan den främre komponenten återge sig själv med det fragment av modellen som den har fått från de underliggande biblioteken.

#### Dynamisk mappning av modell till komponent {#dynamic-model-to-component-mapping}

Mer information om hur den dynamiska mappningen av modell till komponent sker i JavaScript SPA SDK för AEM finns i artikeln [Dynamisk mappning av modell till komponent för SPA](/help/sites-developing/spa-dynamic-model-to-component-mapping.md).

### Ramverksspecifikt lager {#framework-specific-layer}

Ett tredje lager måste implementeras för varje frontramverk. Detta tredje bibliotek ansvarar för interaktionen med de underliggande biblioteken och erbjuder en serie välintegrerade och lättanvända startpunkter för interaktion med datamodellen.

I resten av detta dokument beskrivs kraven för det mellanliggande ramverksspecifika lagret och strävar efter att vara ramverksoberoende. Genom att uppfylla följande krav kan ett ramverksspecifikt lager tillhandahållas för projektkomponenterna för interaktion med de underliggande biblioteken som ansvarar för att hantera datamodellen.

## Allmänna begrepp {#general-concepts}

### Sidmodell {#page-model}

Sidans innehållsstruktur lagras i AEM. Sidans modell används för att mappa och instansiera SPA-komponenter. SPA-utvecklarna skapar SPA-komponenter som de mappar till AEM-komponenter. För att göra detta använder de resurstypen (eller sökvägen till AEM-komponenten) som en unik nyckel.

SPA-komponenterna måste vara synkroniserade med sidmodellen och uppdateras om innehållet ändras i enlighet med detta. Ett mönster som använder dynamiska komponenter måste användas för att instansiera komponenter i farten efter den angivna sidmodellstrukturen.

### Metafält {#meta-fields}

Sidmodellen använder JSON-modellens exporterare, som i sin tur är baserad på API:t [Sling Model](https://sling.apache.org/documentation/bundles/models.html) . De exporterbara snedsättningsmodellerna visar följande fältlista för att de underliggande biblioteken ska kunna tolka datamodellen:

* `:type`: Typ av AEM-resurs (standard = resurstyp)
* `:children`: Hierarkiska underordnade för den aktuella resursen. Underordnade är inte en del av den aktuella resursens inre innehåll (kan hittas på objekt som representerar en sida)
* `:hierarchyType`: Hierarkisk typ för en resurs. `PageModelManager` har för närvarande stöd för sidtypen

* `:items`: Underordnade innehållsresurser för den aktuella resursen (kapslad struktur, endast i behållare)
* `:itemsOrder`: Ordnad lista över underordnade. JSON-mappningsobjektet garanterar inte fältordningen. Genom att ha både kartan och den aktuella arrayen får API-konsumenten fördelarna med båda strukturerna
* `:path`: Innehållssökväg för ett objekt (finns i objekt som representerar en sida)

Se även [Komma igång med AEM Content Services.](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

### Ramverksspecifik modul {#framework-specific-module}

Separata hänsyn underlättar projektgenomförandet. Därför bör ett npm-specifikt paket tillhandahållas. Det här paketet innehåller information om hur du samlar och exponerar basmoduler, tjänster och komponenter. Dessa komponenter måste kapsla in hanteringslogiken för datamodellen och ge åtkomst till data som projektkomponenten väntar sig. Modulen är också ansvarig för att tillfälligt exponera användbara startpunkter i underliggande bibliotek.

För att underlätta bibliotekens driftskompatibilitet rekommenderar Adobe den ramverksspecifika modulen att paketera följande bibliotek. Om det behövs kan lagret kapsla in och anpassa de underliggande API:erna innan de exponeras för projektet.

* [@adobe/aem-spa-page-model-manager](https://www.npmjs.com/package/@adobe/aem-spa-page-model-manager)
* [@adobe/aem-spa-component-mapping](https://www.npmjs.com/package/@adobe/aem-spa-component-mapping)

#### Implementeringar {#implementations}

#### Reagera {#react}

npm-modul: [@adobe/aem-rea-editable-components](https://www.npmjs.com/package/@adobe/aem-react-editable-components)

#### Angular {#angular}

npm-modul: [@adobe/aem-angular-editable-components](https://www.npmjs.com/package/@adobe/aem-angular-editable-components)

## Huvudsakliga tjänster och komponenter {#main-services-and-components}

Följande enheter bör genomföras i enlighet med de riktlinjer som är specifika för varje ramverk. Implementeringen kan variera mycket beroende på ramverkets arkitektur, men de beskrivna funktionerna måste tillhandahållas.

### Modellprovidern {#the-model-provider}

Projektkomponenter måste delegera åtkomst till en modells fragment till en modellprovider. Modellprovidern ansvarar sedan för att lyssna efter ändringar som gjorts i det angivna fragmentet i modellen och returnerar den uppdaterade modellen till den delegerande komponenten.

För att göra detta måste modellprovidern registrera ` [PageModelManager](/help/sites-developing/spa-blueprint.md#pagemodelmanager)`. När en ändring inträffar tar den emot och skickar de uppdaterade data till den delegerande komponenten. Egenskapen som gjorts tillgänglig för den delegerande komponenten som kommer att bära fragmentet i modellen heter `cqModel`. Implementeringen kan fritt tillhandahålla den här egenskapen till komponenten men bör beakta aspekter som integrering med ramverksarkitekturen, upptäckbarhet och användarvänlighet.

### Komponenten HTML Decorator {#the-component-html-decorator}

Komponentdekoratorn ansvarar för att dekorera det yttre HTML-elementet för varje komponentinstans med en serie dataattribut och klassnamn som förväntas av sidredigeraren.

#### Komponentdeklaration {#component-declaration}

Följande metadata måste läggas till i det yttre HTML-elementet som skapas av projektkomponenten. De gör att sidredigeraren kan hämta motsvarande redigeringskonfiguration.

* `data-cq-data-path`: Sökväg till resursen i förhållande till `jcr:content`

#### Funktionsdeklaration och platshållare för redigering {#editing-capability-declaration-and-placeholder}

Följande metadata och klassnamn måste läggas till i det yttre HTML-elementet som skapas av projektkomponenten. De gör att sidredigeraren kan erbjuda relaterade funktioner.

* `cq-placeholder`: Klassnamn som identifierar platshållaren för en tom komponent
* `data-emptytext`: Etikett som ska visas av övertäckningen när en komponentinstans är tom

**Platshållare för tomma komponenter**

Varje komponent måste utökas med en funktion som dekorerar det yttre HTML-elementet med dataattribut och klassnamn som är specifika för platshållare och relaterade övertäckningar när komponenten identifieras som tom.

**Om en komponents tomhet**

* Är komponenten logiskt tom?
* Vilken etikett ska visas av övertäckningen när komponenten är tom?

### Behållare {#container}

En behållare är en komponent som ska innehålla och återge underordnade komponenter. För att göra det itererar behållaren över modellens `:itemsOrder`-, `:items`- och `:children`-egenskaper.

Behållaren hämtar de underordnade komponenterna dynamiskt från arkivet för biblioteket [`ComponentMapping`](/help/sites-developing/spa-blueprint.md#componentmapping). Behållaren utökar sedan den underordnade komponenten med modellproviderfunktionerna och instansierar den till slut.

### Sida {#page}

Komponenten `Page` utökar komponenten `Container`. En behållare är en komponent som är avsedd att innehålla och återge underordnade komponenter, inklusive underordnade sidor. För att göra det itererar behållaren över modellens `:itemsOrder`-, `:items`- och `:children`-egenskaper. Komponenten `Page` hämtar de underordnade komponenterna dynamiskt från arkivet för biblioteket [ ComponentMapping](/help/sites-developing/spa-blueprint.md#componentmapping). `Page` ansvarar för att instansiera underordnade komponenter.

### Responsivt rutnät {#responsive-grid}

Komponenten för responsivt stödraster är en behållare. Den innehåller en specifik variant av modellprovidern som representerar dess kolumner. Det responsiva stödrastret och dess kolumner ansvarar för att dekorera det yttre HTML-elementet i projektkomponenten med de specifika klassnamnen som finns i modellen.

Komponenten för responsivt stödraster bör förmappas till sin AEM-motsvarighet eftersom den här komponenten är komplex och sällan anpassad.

#### Specifika modellfält {#specific-model-fields}

* `gridClassNames:` har angett klassnamn för det responsiva stödrastret
* `columnClassNames:` angivna klassnamn för den responsiva kolumnen

#### Platshållare för det motstående stödrastret {#placeholder-of-the-reponsive-grid}

SPA-komponenten mappas till en grafisk behållare som t.ex. det responsiva stödrastret och måste lägga till en virtuell underordnad platshållare när innehållet skapas. När innehållet i SPA-filen redigeras av sidredigeraren bäddas innehållet in i redigeraren med en iframe och attributet `data-cq-editor` läggs till i dokumentnoden för det innehållet. När attributet `data-cq-editor` finns med måste behållaren innehålla ett HTMLElement som representerar det område som författaren interagerar med när en ny komponent infogas på sidan.

Till exempel:

```
<div data-cq-data-path={"path/to/the/responsivegrid/*"} className="new section aem-Grid-newComponent"/>
```

>[!NOTE]
>
>Klassnamnen som används i exemplet krävs för närvarande av sidredigeraren.
>
>* `"new section"`: Anger att aktuellt element är behållarens platshållare
>* `"aem-Grid-newComponent"`: Normaliserar komponenten för layoututveckling
>

#### Komponentmappning {#component-mapping}

Det underliggande [`Component Mapping`](/help/sites-developing/spa-blueprint.md#componentmapping)-biblioteket och dess `MapTo`-funktion kan kapslas in och utökas för att tillhandahålla funktioner i förhållande till redigeringskonfigurationen som tillhandahålls tillsammans med den aktuella komponentklassen.

```
const EditConfig = {

    emptyLabel: 'My Component',

    isEmpty: function() {
        return !this.props || !this.props.cqModel || this.props.cqModel.isEmpty;
    }
};

class MyComponent extends Component {

    render() {
        return <div className={'my-component'}></div>;
    }
}

MapTo('component/resource/path')(MyComponent, EditConfig);
```

I implementeringen ovan utökas projektkomponenten med tomrumsfunktionen innan den registreras i [komponentmappningsarkivet](/help/sites-developing/spa-blueprint.md#componentmapping). Detta görs genom att kapsla in och utöka biblioteket [`ComponentMapping`](/help/sites-developing/spa-blueprint.md#componentmapping) för att ge stöd för konfigurationsobjektet `EditConfig`:

```
/**
 * Configuration object in charge of providing the necessary data expected by the page editor to initiate the authoring. The provided data will be decorating the associated component
 *
 * @typedef {{}} EditConfig
 * @property {String} [dragDropName]       If defined, adds a specific class name enabling the drag and drop functionality
 * @property {String} emptyLabel           Label to be displayed by the placeholder when the component is empty. Optionally returns an empty text value
 * @property {function} isEmpty            Should the component be considered empty. The function is called using the context of the wrapper component giving you access to the component model
 */

/**
 * Map a React component with the given resource types. If an {@link EditConfig} is provided the <i>clazz</i> is wrapped to provide edition capabilities on the AEM Page Editor
 *
 * @param {string[]} resourceTypes                      - List of resource types for which to use the given <i>clazz</i>
 * @param {class} clazz                                 - Class to be instantiated for the given resource types
 * @param {EditConfig} [editConfig]                     - Configuration object for enabling the edition capabilities
 * @returns {class}                                     - The resulting decorated Class
 */
ComponentMapping.map = function map (resourceTypes, clazz, editConfig) {};
```

## Dra samman med sidredigeraren {#contract-with-the-page-editor}

Projektkomponenterna måste generera minst följande dataattribut så att redigeraren kan interagera med dem.

* `data-cq-data-path`: Komponentens relativa sökväg som anges av `PageModel` (till exempel `"root/responsivegrid/image"`). Det här attributet ska inte läggas till på sidor.

Sammanfattningsvis, för att sidredigeraren ska kunna tolka som redigerbar, måste en projektkomponent respektera följande kontrakt:

* Ange de förväntade attributen för att associera en klientkomponentinstans till en AEM-resurs.
* Ange den förväntade serie attribut och klassnamn som gör att tomma platshållare kan skapas.
* Ange de förväntade klassnamnen för att aktivera dra och släpp-funktionen för resurser.

### Vanlig HTML Element-struktur {#typical-html-element-structure}

Följande fragment illustrerar den typiska HTML-representationen av en sidinnehållsstruktur. Här är några viktiga punkter:

* Det responsiva rutnätselementet innehåller klassnamn med prefixet `aem-Grid--`
* Det responsiva kolumnelementet innehåller klassnamn som har prefixet `aem-GridColumn--`
* Ett responsivt stödraster som också är kolumnen i ett överordnat stödraster kapslas, t.ex. de två föregående prefixen visas inte i samma element
* Element som motsvarar redigerbara resurser har en `data-cq-data-path`-egenskap. Se avsnittet [Kontrakt med sidredigeraren](#contract-wtih-the-page-editor) i det här dokumentet.

```
<div data-cq-data-path="/content/page">
    <div class="aem-Grid aem-Grid--12 aem-Grid--default--12">
        <div class="aem-container aem-GridColumn aem-GridColumn--default--12" data-cq-data-path="/content/page/jcr:content/root/responsivegrid">
            <div class="aem-Grid aem-Grid--12 aem-Grid--default--12">
                <div class="cmp-image cq-dd-image aem-GridColumn aem-GridColumn--default--12" data-cq-data-path="/root/responsivegrid/image">
                    <img src="/content/we-retail-spa-sample/react/jcr%3acontent/root/responsivegrid/image.img.jpeg/1512113734019.jpeg">
                </div>
            </div>
        </div>
    </div>
</div>
```

## Navigering och routning {#navigation-and-routing}

Appen äger routningen. Utvecklaren måste först implementera en navigeringskomponent (mappas till en AEM-navigeringskomponent). Den här komponenten återger URL-länkar som ska användas tillsammans med en serie vägar som visar eller döljer innehållsfragment.

Det underliggande [`PageModelManager`](/help/sites-developing/spa-blueprint.md#pagemodelmanager)-biblioteket och dess ` [ModelRouter](/help/sites-developing/spa-routing.md)`-modul (aktiverad som standard) ansvarar för förhämtning och ger åtkomst till modellen som är associerad med en given resurssökväg.

De två entiteterna är relaterade till begreppet routning, men ` [ModelRouter](/help/sites-developing/spa-routing.md)` ansvarar bara för att ` [PageModelManager](/help/sites-developing/spa-blueprint.md#pagemodelmanager)` har lästs in med en datamodell som är synkroniserad med det aktuella programtillståndet.

Mer information finns i artikeln [SPA-modellroutning](/help/sites-developing/spa-routing.md).

## SPA in action {#spa-in-action}

Se hur en enkel SPA fungerar och experimentera med en SPA själv genom att fortsätta med dokumentet [Komma igång med SPA i AEM](/help/sites-developing/spa-getting-started-react.md).

## Ytterligare läsning {#further-reading}

Mer information om SPA i AEM finns i följande dokument:

* [SPA-redigeringsöversikt](/help/sites-developing/spa-overview.md) för en översikt över SPA i AEM och kommunikationsmodellen
* [Komma igång med SPA i AEM](/help/sites-developing/spa-getting-started-react.md) om du vill ha en guide till ett enkelt SPA-program och hur det fungerar
