---
title: Komma igång med SPA i AEM - Angular
description: I den här artikeln beskrivs ett exempel på ett SPA-program, hur det sätts samman och hur du snabbt kommer igång med ditt eget SPA-program med hjälp av Angular-ramverket.
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: spa
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing,SPA Editor
role: Developer
source-git-commit: cd23626858b9061228718a0455c8dd0812de92b3
workflow-type: tm+mt
source-wordcount: '978'
ht-degree: 0%

---

# Komma igång med SPA i AEM - Angular{#getting-started-with-spas-in-aem-angular}

Single page applications (SPAs) can offer compelling experiences for website users. Utvecklare vill kunna skapa webbplatser med SPA-ramverk och författare vill smidigt redigera innehåll i AEM för en webbplats som byggts med SPA-ramverk.

SPA-funktionen är en omfattande lösning för SPA-program i AEM. I den här artikeln presenteras ett förenklat SPA-program i Angular-ramverket, som förklarar hur det är sammanställt, så att du snabbt kan komma igång med ditt eget SPA.

>[!NOTE]
>
>Den här artikeln är baserad på Angular ramverk. Motsvarande dokument för React framework finns i [Komma igång med SPA i AEM - React](/help/sites-developing/spa-getting-started-react.md).

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

Det här dokumentet kommer att gå igenom strukturen i en förenklad SPA och illustrera hur det fungerar så att du kan tillämpa den här förståelsen på din egen SPA.

## Beroenden, konfiguration och byggteknik {#dependencies-configuration-and-building}

Förutom det förväntade Angular-beroendet kan SPA-exemplet använda ytterligare bibliotek för att göra skapandet av SPA mer effektivt.

### Beroenden {#dependencies}

Filen `package.json` definierar kraven för det övergripande SPA-paketet. Här listas de minsta AEM-beroenden som krävs.

```
"dependencies": {
  "@adobe/aem-angular-editable-components": "~1.0.3",
  "@adobe/aem-spa-component-mapping": "~1.0.5",
  "@adobe/aem-spa-page-model-manager": "~1.0.3"
}
```

`aem-clientlib-generator` används för att skapa klientbibliotek automatiskt som en del av byggprocessen.

`"aem-clientlib-generator": "^1.4.1",`

Mer information om [aem-clientlib-generator finns på GitHub](https://github.com/wcm-io-frontend/aem-clientlib-generator).

>[!CAUTION]
>
>Den lägsta versionen av `aem-clientlib-generator` som krävs är 1.4.1.

`aem-clientlib-generator` har konfigurerats i filen `clientlib.config.js` enligt följande.

```
module.exports = {
    // default working directory (can be changed per 'cwd' in every asset option)
    context: __dirname,

    // path to the clientlib root folder (output)
    clientLibRoot: "./../content/jcr_root/apps/my-angular-app/clientlibs",

    libs: {
        name: "my-angular-app",
        allowProxy: true,
        categories: ["my-angular-app"],
        embed: ["my-angular-app.responsivegrid"],
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

`"build": "ng build --build-optimizer=false && clientlib",`

När paketet har skapats kan det överföras till en AEM-instans.

### AEM Project Archetype {#aem-project-archetype}

Alla AEM-projekt ska använda [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html), som har stöd för SPA-projekt med React eller Angular och som använder SPA SDK.

## Programstruktur {#application-structure}

Om du tar med beroenden och bygger din app enligt beskrivningen ovan får du ett fungerande SPA-paket som du kan överföra till din AEM-instans.

I nästa avsnitt av detta dokument beskrivs hur en SPA i AEM är strukturerad, vilka filer som är viktiga för programmet och hur de fungerar tillsammans.

En förenklad bildkomponent används som exempel, men alla komponenter i programmet är baserade på samma koncept.

### app.module.ts {#app-module-ts}

Startpunkten i SPA är filen `app.module.ts` som visas här förenklad för att fokusera på det viktiga innehållet.

```
// app.module.ts
import { BrowserModule, BrowserTransferStateModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { SpaAngularEditableComponentsModule } from '@adobe/aem-angular-editable-components';
import { AppRoutingModule } from './app-routing.module';

@NgModule({
  imports: [ BrowserModule.withServerTransition({ appId: 'my-angular-app' }),
    SpaAngularEditableComponentsModule,
    AppRoutingModule,
    BrowserTransferStateModule ],
  providers: ...,
  declarations: [ ... ],
  entryComponents: [ ... ],
  bootstrap: [ AppComponent ]
})
export class AppModule {}
```

Filen `app.module.ts` är startpunkten för programmet och innehåller den inledande projektkonfigurationen och använder `AppComponent` för att starta programmet.

#### Statisk instansiering {#static-instantiation}

När komponenten instansieras statiskt med komponentmallen måste värdet skickas från modellen till komponentens egenskaper. Värdena från modellen skickas som attribut som senare är tillgängliga som komponentegenskaper.

### app.component.ts {#app-component-ts}

När `app.module.ts` har startat `AppComponent` kan den initiera appen, som visas här i en förenklad version för att fokusera på det viktiga innehållet.

```
// app.component.ts
import { Component } from '@angular/core';
import { ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Constants } from "@adobe/aem-angular-editable-components";

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>
  `
})

export class AppComponent {
  items;
  itemsOrder;
  path;

  constructor() {
    ModelManager.initialize().then(this.updateData.bind(this));
  }

  private updateData(model) {
    this.path = model[Constants.PATH_PROP];
    this.items = model[Constants.ITEMS_PROP];
    this.itemsOrder = model[Constants.ITEMS_ORDER_PROP];
  }
}
```

### main-content.component.ts {#main-content-component-ts}

Genom att bearbeta sidan anropar `app.component.ts` `main-content.component.ts` som listas här i en förenklad version.

```
import { Component } from '@angular/core';
import { ModelManagerService }     from '../model-manager.service';
import { ActivatedRoute } from '@angular/router';
import { Constants } from "@adobe/aem-angular-editable-components";

@Component({
  selector: 'app-main',
  template: `
    <aem-page class="structure-page" [attr.data-cq-page-path]="path" [cqPath]="path" [cqItems]="items" [cqItemsOrder]="itemsOrder" ></aem-page>
  `
})

export class MainContentComponent {
  items;
  itemsOrder;
  path;

  constructor( private route: ActivatedRoute,
    private modelManagerService: ModelManagerService) {
    this.modelManagerService.getData({ path: this.route.snapshot.data.path }).then((data) => {
      this.path = data[Constants.PATH_PROP];
      this.items = data[Constants.ITEMS_PROP];
      this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
    });
  }
}
```

`MainComponent` importerar JSON-representationen av sidmodellen och bearbetar innehållet för att kapsla in/dekorera varje element på sidan. Mer information om `Page` finns i dokumentet [SPA-utkast](/help/sites-developing/spa-blueprint.md#main-pars-header-1694932501).

### image.component.ts {#image-component-ts}

`Page` består av komponenter. När JSON har importerats kan `Page` bearbeta komponenter som `image.component.ts`, vilket visas här.

```
/// image.component.ts
import { Component, Input } from '@angular/core';

const ImageEditConfig = {

    emptyLabel: 'Image',

    isEmpty: function(cqModel) {
        return !cqModel || !cqModel.src || cqModel.src.trim().length < 1;
    }
};

@Component({
  selector: 'app-image',
  templateUrl: './image.component.html',
})

export class ImageComponent {
  @Input() src: string;
  @Input() alt: string;
  @Input() title: string;
}

MapTo('my-angular-app/components/image')(ImageComponent, ImageEditConfig);
```

Den centrala idén med SPA i AEM är att mappa SPA-komponenter till AEM-komponenter och uppdatera komponenten när innehållet ändras (och omvänt). En sammanfattning av kommunikationsmodellen finns i dokumentet [SPA Editor Overview](/help/sites-developing/spa-overview.md).

`MapTo('my-angular-app/components/image')(Image, ImageEditConfig);`

Metoden `MapTo` mappar SPA-komponenten till AEM-komponenten. Det stöder användningen av en enda sträng eller en array med strängar.

`ImageEditConfig` är ett konfigurationsobjekt som bidrar till att aktivera en komponents redigeringsfunktioner genom att tillhandahålla de metadata som behövs för att redigeraren ska kunna generera platshållare

Om det inte finns något innehåll visas etiketter som platshållare för det tomma innehållet.

#### Egenskaper som skickats dynamiskt {#dynamically-passed-properties}

Data som kommer från modellen skickas dynamiskt som egenskaper för komponenten.

### image.component.html {#image-component-html}

Slutligen kan bilden återges i `image.component.html`.

```
// image.component.html
<img [src]="src" [alt]="alt" [title]="title"/>
```

## Dela information mellan SPA-komponenter {#sharing-information-between-spa-components}

Det är regelbundet nödvändigt att komponenter i ett ensidigt program delar information. Det finns flera rekommenderade sätt att göra detta, som anges nedan i ökande komplexitetsordning.

* **Alternativ 1:** Centralisera logiken och sända till nödvändiga komponenter, till exempel genom att använda en util-klass som en ren objektorienterad lösning.
* **Alternativ 2:** Dela komponentlägen med ett tillståndsbibliotek som NgRx.
* **Alternativ 3:** Utnyttja objekthierarkin genom att anpassa och utöka behållarkomponenten.

## Nästa steg {#next-steps}

En stegvis guide till hur du skapar en egen SPA finns i [Komma igång med AEM SPA Editor - WKND Events-självstudiekurs](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html).

Mer information om hur du organiserar dig för att utveckla SPA för AEM finns i artikeln [Utveckla SPA för AEM](/help/sites-developing/spa-architecture.md).

Mer information om den dynamiska mappningen av modell till komponent och hur den fungerar i SPA i AEM finns i artikeln [Dynamisk mappning av modell till komponent för SPA](/help/sites-developing/spa-dynamic-model-to-component-mapping.md).

Om du vill implementera SPA i AEM för ett annat ramverk än React eller Angular eller bara vill fördjupa dig i hur SPA SDK för AEM fungerar läser du artikeln [SPA-utkast](/help/sites-developing/spa-blueprint.md).
