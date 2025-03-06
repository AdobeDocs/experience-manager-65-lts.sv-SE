---
title: Utökar ContextHub
description: Definiera nya typer av ContextHub-butiker och moduler när de angivna lagren inte uppfyller dina lösningskrav
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Personalization
role: Developer
exl-id: 5bece740-d8ec-4cef-89b6-bced158b866d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Utökar ContextHub{#extending-contexthub}

Definiera nya typer av ContextHub-butiker och moduler när de angivna inte uppfyller dina lösningskrav.

## Skapa anpassade butikskandidater {#creating-custom-store-candidates}

ContextHub-butiker skapas från registrerade butikskandidater. Om du vill skapa en anpassad butik skapar och registrerar du en butikskandidater.

JavaScript-filen som innehåller koden som skapar och registrerar arkivkandidaten måste inkluderas i en [klientbiblioteksmapp](/help/sites-developing/clientlibs.md#creating-client-library-folders). Mappens kategori måste matcha följande mönster:

```xml
contexthub.store.[storeType]
```

Delen `[storeType]` i kategorin är den `storeType` som butikskandidaten är registrerad med. (Se [Registrera ett ContextHub Store-förslag](/help/sites-developing/ch-extend.md#registering-a-contexthub-store-candidate)). För till exempel storeType för `contexthub.mystore` måste kategorin för klientbiblioteksmappen vara `contexthub.store.contexthub.mystore`.

### Skapa en ContextHub Store-kandidat {#creating-a-contexthub-store-candidate}

Om du vill skapa en butikskandidat använder du funktionen [`ContextHub.Utils.inheritance.inherit`](/help/sites-developing/contexthub-api.md#inherit-child-parent) för att utöka en av basbutikerna:

* [`ContextHub.Store.PersistedStore`](/help/sites-developing/contexthub-api.md#contexthub-store-persistedstore)
* [`ContextHub.Store.SessionStore`](/help/sites-developing/contexthub-api.md#contexthub-store-sessionstore)
* [`ContextHub.Store.JSONPStore`](/help/sites-developing/contexthub-api.md#contexthub-store-jsonpstore)
* [`ContextHub.Store.PersistedJSONPStore`](/help/sites-developing/contexthub-api.md#contexthub-store-persistedjsonpstore)

Varje basbutik utökar butiken [`ContextHub.Store.Core`](/help/sites-developing/contexthub-api.md#contexthub-store-core).

I följande exempel skapas det enklaste tillägget för arkivkandidaten `ContextHub.Store.PersistedStore`:

```
myStoreCandidate = function(){};
ContextHub.Utils.inheritance.inherit(myStoreCandidate,ContextHub.Store.PersistedStore);
```

I realiteten definierar dina anpassade butikskandidater ytterligare funktioner eller åsidosätter butikens ursprungliga konfiguration. Flera [exempel på arkivkandidater](/help/sites-developing/ch-samplestores.md) har installerats i databasen nedan `/libs/granite/contexthub/components/stores`. Använd CRXDE Lite för att öppna JavaScript-filerna.

### Registrerar en ContextHub Store-kandidat {#registering-a-contexthub-store-candidate}

Registrera en butikskandidat för att integrera den med ContextHub-ramverket så att butiker kan skapas utifrån det. Om du vill registrera en butikskandidater använder du funktionen [`registerStoreCandidate`](/help/sites-developing/contexthub-api.md#registerstorecandidate-store-storetype-priority-applies) i klassen `ContextHub.Utils.storeCandidates`.

När du registrerar en butikskandidat anger du ett namn för butikstypen. När du skapar en butik från kandidaten använder du butikstypen för att identifiera den kandidat som den baseras på.

När du registrerar en butikskandidat anger du dess prioritet. När en butikskandidat registreras med samma butikstyp som en redan registrerad butikskandidat, används den som har den högre prioriteten. Därför kan du åsidosätta befintliga butikskandidater med nya implementeringar.

```
ContextHub.Utils.storeCandidates.registerStoreCandidate(myStoreCandidate,
                                'contexthub.mystorecandidate', 0);
```

Vanligtvis behövs bara en kandidat och prioriteten kan anges till `0`. Men om du är intresserad kan du lära dig mer om [mer avancerade registreringar,](/help/sites-developing/contexthub-api.md#registerstorecandidate-store-storetype-priority-applies) som gör att en av få butiksimplementeringar kan väljas baserat på JavaScript-villkor (`applies`) och kandidatprioritet.

## Skapar gränssnittsmodultyper för ContextHub {#creating-contexthub-ui-module-types}

Skapa anpassade gränssnittsmodultyper när de som är [installerade med ContextHub](/help/sites-developing/ch-samplemodules.md) inte uppfyller dina krav. Om du vill skapa en gränssnittsmodultyp skapar du en gränssnittsmodulrenderare genom att utöka klassen `ContextHub.UI.BaseModuleRenderer` och sedan registrera den med `ContextHub.UI`.

Skapa ett `Class`-objekt som innehåller den logik som återger gränssnittsmodulen om du vill skapa en renderare i användargränssnittsmodulen. Klassen måste minst utföra följande åtgärder:

* Utöka klassen `ContextHub.UI.BaseModuleRenderer`. Den här klassen är den grundläggande implementeringen för alla UI-modulrenderare. Objektet `Class` definierar egenskapen `extend` som du använder för att namnge den här klassen som den som utökas.

* Ange en standardkonfiguration. Skapa en `defaultConfig`-egenskap. Den här egenskapen är ett objekt som innehåller de egenskaper som har definierats för användargränssnittsmodulen [`contexthub.base`](/help/sites-developing/ch-samplemodules.md#contexthub-base-ui-module-type) och alla andra egenskaper som du behöver.

Källan för `ContextHub.UI.BaseModuleRenderer` finns på /libs/granite/contexthub/code/ui/container/js/ContextHub.UI.BaseModuleRenderer.js. Om du vill registrera återgivaren använder du metoden [`registerRenderer`](/help/sites-developing/contexthub-api.md#registerrenderer-moduletype-renderer-dontrender) i klassen `ContextHub.UI`. Ange ett namn för modultypen. När administratörer skapar en gränssnittsmodul som baseras på den här renderaren anger de det här namnet.

Skapa och registrera återgivningsklassen i en anonym funktion som körs automatiskt. Följande exempel baseras på källkoden för gränssnittsmodulen contexthub.browserinfo. Den här gränssnittsmodulen är ett enkelt tillägg till klassen `ContextHub.UI.BaseModuleRenderer`.

```xml
;(function() {

    var SurferinfoRenderer = new Class({
        extend: ContextHub.UI.BaseModuleRenderer,

        defaultConfig: {
            icon: 'coral-Icon--globe',
            title: 'Browser/OS Information',

            storeMapping: {
                surferinfo: 'surferinfo'
            },

            template:
                '<p>{{surferinfo.browser.family}} {{surferinfo.browser.version}}</p>' +
                '<p>{{surferinfo.os.name}} {{surferinfo.os.version}}</p>'
        }
    });

    ContextHub.UI.registerRenderer('contexthub.browserinfo', new SurferinfoRenderer());

}());
```

JavaScript-filen som innehåller koden som skapar och registrerar återgivaren måste inkluderas i en [klientbiblioteksmapp](/help/sites-developing/clientlibs.md#creating-client-library-folders). Mappens kategori måste matcha följande mönster:

```xml
contexthub.module.[moduleType]
```

Delen `[moduleType]` i kategorin är den `moduleType` som modulåtergivaren är registrerad med. För `moduleType` av `contexthub.browserinfo` måste till exempel kategorin för klientbiblioteksmappen vara `contexthub.module.contexthub.browserinfo`.
