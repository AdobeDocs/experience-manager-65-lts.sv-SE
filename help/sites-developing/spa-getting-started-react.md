---
title: Getting Started with SPAs in AEM - React
description: I den här artikeln beskrivs ett exempel på ett SPA-program, hur det sätts samman och hur du snabbt kommer igång med ditt eget SPA med hjälp av React Framework.
contentOwner: bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: spa
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing,SPA Editor
role: Developer
exl-id: 9889b0fe-f39c-42dc-bc7b-8743d68e7503
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 0%

---

# Getting Started with SPAs in AEM - React{#getting-started-with-spas-in-aem-react}

Single page applications (SPAs) can offer compelling experiences for website users. Utvecklare vill kunna skapa webbplatser med SPA-ramverk och författare vill smidigt redigera innehåll i AEM för en webbplats som byggts med SPA-ramverk.

SPA-funktionen är en omfattande lösning för SPA-program i AEM. I den här artikeln presenteras en förenklad SPA-applikation i React-ramverket, som förklarar hur det är uppbyggt, så att du snabbt kan komma igång med din egen SPA.

>[!NOTE]
>
>Artikeln bygger på React Framework. Motsvarande dokument för Angular-ramverket finns i [Komma igång med SPA i AEM - Angular](/help/sites-developing/spa-getting-started-angular.md).

{{ue-over-spa}}

## Introduktion {#introduction}

I den här artikeln sammanfattas grundläggande funktioner i ett enkelt SPA-avtal och det minimum du behöver känna till för att få igång det.

Mer information om hur SPA fungerar i AEM finns i följande dokument:

* [Introduktion och genomgång av SPA](/help/sites-developing/spa-walkthrough.md)
* [SPA-introduktion](/help/sites-developing/spa-overview.md)
* [SPA Blueprint](/help/sites-developing/spa-blueprint.md)

>[!NOTE]
>
>För att innehållet ska kunna redigeras i ett SPA måste det lagras i AEM och kunna visas av innehållsmodellen.
>
>En SPA som utvecklats utanför AEM kommer inte att vara tilltalande om den inte följer innehållsmodellkontraktet.

Det här dokumentet kommer att gå igenom strukturen i en förenklad SPA som skapats med React-ramverket och illustrera hur det fungerar så att du kan tillämpa den här förståelsen på din egen SPA.

## Beroenden, konfiguration och byggteknik {#dependencies-configuration-and-building}

Förutom det förväntade React-beroendet kan SPA-exemplet använda ytterligare bibliotek för att göra skapandet av SPA mer effektivt.

### Beroenden {#dependencies}

Filen `package.json` definierar kraven för det övergripande SPA-paketet. Här listas de minsta AEM-beroendena för en fungerande SPA.

```
  "dependencies": {
    "@adobe/aem-react-editable-components": "~1.0.4",
    "@adobe/aem-spa-component-mapping": "~1.0.5",
    "@adobe/aem-spa-page-model-manager": "~1.0.3"
  }
```

Eftersom det här exemplet baseras på React-ramverket finns det två React-specifika beroenden som är obligatoriska i filen `package.json`:

```
react
 react-dom
```

`aem-clientlib-generator` används för att skapa klientbibliotek automatiskt som en del av byggprocessen.

`"aem-clientlib-generator": "^1.4.1",`

Mer information om [aem-clientlib-generatorn finns på GitHub](https://github.com/wcm-io-frontend/aem-clientlib-generator).

>[!CAUTION]
>
>Den lägsta versionen av `aem-clientlib-generator` som krävs är 1.4.1.

`aem-clientlib-generator` har konfigurerats i filen `clientlib.config.js` enligt följande.

```
module.exports = {
    // default working directory (can be changed per 'cwd' in every asset option)
    context: __dirname,

    // path to the clientlib root folder (output)
    clientLibRoot: "./../content/jcr_root/apps/my-react-app/clientlibs",

    libs: {
        name: "my-react-app",
        allowProxy: true,
        categories: ["my-react-app"],
        embed: ["my-react-app.responsivegrid"],
        jsProcessor: ["min:gcc"],
        serializationFormat: "xml",
        assets: {
            js: [
                "dist/**/*.js"
            ],
            css: [
                "dist/**/*.css"
            ]
        }
    }
};
```

### Byggnad {#building}

När du skapar appen används [Webpack](https://webpack.js.org/) för implementering, förutom aem-clientlib-generator, för att skapa klientbibliotek automatiskt. Därför påminner kommandot build om:

`"build": "webpack && clientlib --verbose"`

När paketet har skapats kan det överföras till en AEM-instans.

### AEM Project Archetype {#aem-project-archetype}

Alla AEM-projekt ska använda [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html), som har stöd för SPA-projekt med React eller Angular och som använder SPA SDK.

## Programstruktur {#application-structure}

Om du tar med beroenden och bygger din app enligt beskrivningen ovan får du ett fungerande SPA-paket som du kan överföra till din AEM-instans.

I nästa avsnitt av detta dokument beskrivs hur en SPA i AEM är strukturerad, vilka filer som är viktiga för programmet och hur de fungerar tillsammans.

En förenklad bildkomponent används som exempel, men alla komponenter i programmet är baserade på samma koncept.

### index.js {#index-js}

Startpunkten i SPA är filen `index.js` som visas här förenklad för att fokusera på det viktiga innehållet.

```
import ReactDOM from 'react-dom';
import App from './App';
import { ModelManager, Constants } from "@adobe/aem-spa-page-model-manager";

...

ModelManager.initialize().then((pageModel) => {
ReactDOM.render(
    <App cqChildren={pageModel[Constants.CHILDREN_PROP]} cqItems={pageModel[Constants.ITEMS_PROP]} cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]} cqPath={ModelManager.rootPath} locationPathname={ window.location.pathname }/>
, document.getElementById('page'));

});
```

Huvudfunktionen i `index.js` är att använda funktionen `ReactDOM.render` för att avgöra var i DOM som programmet ska injiceras.

Det här är en standardanvändning av den här funktionen, som inte är unik för det här exempelprogrammet.

#### Statisk instansiering {#static-instantiation}

När komponenten instansieras statiskt med komponentmallen (till exempel JSX), måste värdet skickas från modellen till komponentens egenskaper.

### App.js {#app-js}

Genom att återge appen anropar `index.js` `App.js`, som visas här i en förenklad version för att fokusera på det viktiga innehållet.

```
import {Page, withModel } from '@adobe/aem-react-editable-components';

...

class App extends Page {
...
}

export default withModel(App);
```

`App.js` fyller i första hand i att kapsla in rotkomponenterna som appen består av. Startpunkten för alla program är sidan.

### Page.js {#page-js}

Genom att återge sidan anropar `App.js` `Page.js` som listas här i en förenklad version.

```
import {Page, MapTo, withComponentMappingContext } from "@adobe/aem-react-editable-components";

...

class AppPage extends Page {
...
}

MapTo('my-react-app/components/structure/page')(withComponentMappingContext(AppPage));
```

I det här exemplet utökar klassen `AppPage` `Page`, som innehåller de interna innehållsmetoderna som sedan kan användas.

`Page` importerar JSON-representationen av sidmodellen och bearbetar innehållet för att kapsla in/dekorera varje element på sidan. Mer information om `Page` finns i dokumentet [SPA-utkast](/help/sites-developing/spa-blueprint.md#main-pars-header-1694932501).

### Image.js {#image-js}

När sidan återges kan komponenter som `Image.js` som visas här återges.

```
import React, {Component} from 'react';
import {MapTo} from '@adobe/aem-react-editable-components';

require('./Image.css');

const ImageEditConfig = {

    emptyLabel: 'Image',

    isEmpty: function() {
        return !this.props || !this.props.src || this.props.src.trim().length < 1;
    }
};

class Image extends Component {

    render() {
        return (<img src={this.props.src}>);
    }
}

MapTo('my-react-app/components/content/image')(Image, ImageEditConfig);
```

Den centrala idén med SPA i AEM är att mappa SPA-komponenter till AEM-komponenter och uppdatera komponenten när innehållet ändras (och omvänt). En sammanfattning av kommunikationsmodellen finns i dokumentet [SPA Editor Overview](/help/sites-developing/spa-overview.md).

`MapTo('my-react-app/components/content/image')(Image, ImageEditConfig);`

Metoden `MapTo` mappar SPA-komponenten till AEM-komponenten. Det stöder användningen av en enda sträng eller en array med strängar.

`ImageEditConfig` är ett konfigurationsobjekt som bidrar till att aktivera en komponents redigeringsfunktioner genom att tillhandahålla de metadata som behövs för att redigeraren ska kunna generera platshållare

Om det inte finns något innehåll visas etiketter som platshållare för det tomma innehållet.

#### Egenskaper som skickats dynamiskt {#dynamically-passed-properties}

Data som kommer från modellen skickas dynamiskt som egenskaper för komponenten.

## Exportera redigerbart innehåll {#exporting-editable-content}

Du kan exportera en komponent och göra den redigerbar.

```
import React, { Component } from 'react';
import { MapTo } from '@adobe/aem-react-editable-components';

...

const EditConfig = {...}

class PageClass extends Component {...};

...

export default MapTo('my-react-app/react/components/structure/page')(PageClass, EditConfig);
```

Funktionen `MapTo` returnerar en `Component` som är resultatet av en disposition som utökar den tillhandahållna `PageClass` med klassnamnen och attributen som aktiverar redigeringen. Den här komponenten kan exporteras för att senare instansieras i koden för programmet.

När komponenten `Page` exporteras med funktionerna `MapTo` eller `withModel` kapslas den in med en `ModelProvider` -komponent som ger standardkomponenter tillgång till den senaste versionen av sidmodellen eller en exakt plats i den sidmodellen.

Mer information finns i dokumentet [SPA-utkast](/help/sites-developing/spa-blueprint.md#main-pars-header-329251743).

>[!NOTE]
>
>Som standard tar du emot hela komponentmodellen när du använder funktionen `withModel`.

## Dela information mellan SPA-komponenter {#sharing-information-between-spa-components}

Det är regelbundet nödvändigt att komponenter i ett ensidigt program delar information. Det finns flera rekommenderade sätt att göra detta, som anges nedan i ökande komplexitetsordning.

* **Alternativ 1:** Centralisera logiken och sända till nödvändiga komponenter, till exempel genom att använda React Context.
* **Alternativ 2:** Dela komponentlägen med hjälp av ett tillståndsbibliotek som Redux.
* **Alternativ 3:** Utnyttja objekthierarkin genom att anpassa och utöka behållarkomponenten.

## Nästa steg {#next-steps}

En stegvis guide till hur du skapar en egen SPA finns i [Komma igång med AEM SPA Editor - WKND Events-självstudiekurs](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html).

Mer information om hur du organiserar dig för att utveckla SPA för AEM finns i artikeln [Utveckla SPA för AEM](/help/sites-developing/spa-architecture.md).

Mer information om den dynamiska mappningen av modell till komponent och hur den fungerar i SPA i AEM finns i artikeln [Dynamisk mappning av modell till komponent för SPA](/help/sites-developing/spa-dynamic-model-to-component-mapping.md).

Om du vill implementera SPA i AEM för ett annat ramverk än React eller Angular eller bara vill fördjupa dig i hur SPA SDK för AEM fungerar läser du artikeln [SPA-utkast](/help/sites-developing/spa-blueprint.md).
