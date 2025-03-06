---
title: Lägga till ContextHub på Pages och Access Stores
description: Lägg till ContextHub på sidorna för att aktivera ContextHub-funktionerna och länka till ContextHub JavaScript-biblioteken
solution: Experience Manager, Experience Manager Sites
feature: Developing,Personalization
role: Developer
exl-id: a2b32ec3-ff1b-47e0-b346-53a5db5ae27f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 0%

---

# Lägga till ContextHub på Pages och Access Stores {#adding-contexthub-to-pages-and-accessing-stores}

Lägg till ContextHub på sidorna för att aktivera ContextHub-funktionerna och för att länka till ContextHub JavaScript-biblioteken.

ContextHub JavaScript API ger åtkomst till kontextdata som hanteras av ContextHub. Den här sidan beskriver kortfattat huvudfunktionerna i API:t för att komma åt och ändra kontextdata. Följ länkarna till API-referensdokumentationen för att se detaljerad information och kodexempel.

## Lägga till ContextHub i en sidkomponent {#adding-contexthub-to-a-page-component}

Om du vill aktivera ContextHub-funktionerna och länka till ContextHub JavaScript-biblioteken inkluderar du `contexthub`-komponenten i `head`-avsnittet på sidan. HTML-koden för sidkomponenten ska likna följande exempel:

```xml
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

Observera att du även måste konfigurera om ContextHub-verktygsfältet ska visas i förhandsgranskningsläget. Se [Visa och dölja ContextHub-gränssnittet](ch-configuring.md#showing-and-hiding-the-contexthub-ui).

## Om ContextHub Stores {#about-contexthub-stores}

Använd ContextHub-arkiv för att behålla kontextdata. ContextHub innehåller följande typer av butiker som utgör grunden för alla butikstyper:

* [PersistedStore](contexthub-api.md#contexthub-store-persistedstore)
* [SessionStore](contexthub-api.md#contexthub-store-sessionstore)
* [JSONPStore](contexthub-api.md#contexthub-store-persistedjsonpstore)
* [PersistedJSONPStore](contexthub-api.md#contexthub-store-persistedstore)

Alla butikstyper är tillägg till klassen [`ContextHub.Store.Core`](contexthub-api.md#contexthub-store-core). Mer information om hur du skapar en lagringstyp finns i [Skapa anpassade butiker](ch-extend.md#creating-custom-store-candidates). Mer information om exempel på lagringstyper finns i [Exempel på ContextHub Store-förslag](ch-samplestores.md).

### Beständiga lägen {#persistence-modes}

Kontextnavlager använder ett av följande beständiga lägen:

* **Lokal:** Använder HTML5 localStorage för att lagra data. Lokal lagring sparas i webbläsaren i alla sessioner.
* **Session:** Använder HTML5 sessionStorage för att lagra data. Sessionslagringsplatsen sparas under hela webbläsarsessionen och är tillgänglig för alla webbläsarfönster.
* **Cookie:** Använder webbläsarens inbyggda stöd för cookies för datalagring. Cookie-data skickas till och från servern i HTTP-begäranden.
* **Window.name:** Använder egenskapen window.name för att behålla data.
* **Minne:** Använder ett JavaScript-objekt för att behålla data.

Som standard använder Context Hub det lokala beständighetsläget. Om webbläsaren inte har stöd för eller tillåter HTML5 localStorage, används sessionens beständighet. Om webbläsaren inte stöder eller tillåter HTML5 sessionStorage, används Window.name persistence.

### Lagra data {#store-data}

Internt kan du lagra data i en trädstruktur, vilket gör att värden kan läggas till som primära typer eller komplexa objekt. När du lägger till komplexa objekt i butiker skapar objektegenskaperna grenar i dataträdet. Följande komplexa objekt läggs till i ett tomt arkiv med namnet location:

```javascript
Object {
   number: 321,
   data: {
      city: "Basel",
      country: "Switzerland",
      details: {
         population: 173330,
         elevation: 260
      }
   }
}
```

Trädstrukturen för butiksdata kan utformas på följande sätt:

```text
/
|- number
|- data
      |- city
      |- country
      |- details
            |- population
            |- elevation
```

Trädstrukturen definierar dataobjekt i arkivet som nyckel/värde-par. I ovanstående exempel motsvarar tangenten `/number` värdet `321` och tangenten `/data/country` motsvarar värdet `Switzerland`.

### Ändra objekt {#manipulating-objects}

ContextHub innehåller klassen [`ContextHub.Utils.JSON.tree`](contexthub-api.md#contexthub-utils-json-tree) för manipulering av JavaScript-objekt. Använd den här klassens funktioner för att ändra JavaScript-objekt innan du lägger till dem i en butik eller efter att du har fått dem från en butik.

Klassen [`ContextHub.Utils.JSON`](contexthub-api.md#contexthub-utils-json) innehåller dessutom funktioner för serialisering av objekt till strängar och avserialisering av strängar till objekt. Använd den här klassen för att hantera JSON-data för webbläsare som inte innehåller funktionerna `JSON.parse` och `JSON.stringify` internt.

## Interagera med ContextHub Stores {#interacting-with-contexthub-stores}

Använd JavaScript-objektet [`ContextHub`](contexthub-api.md#ui-event-constants) för att få en butik som ett JavaScript-objekt. När du har fått lagringsobjektet kan du ändra de data som det innehåller. Använd funktionen [`getAllStores`](contexthub-api.md#getallstores) eller [`getStore`](contexthub-api.md#getstore-name) för att hämta arkivet.

### Åtkomst till butiksdata {#accessing-store-data}

JavaScript-klassen [`ContexHub.Store.Core`](contexthub-api.md#contexthub-store-core) definierar flera funktioner för interaktion med lagringsdata. Följande funktioner lagrar och hämtar flera dataobjekt som finns i objekt:

* [addAllItems](contexthub-api.md#addallitems-tree-options)
* [getTree](contexthub-api.md#gettree-includeinternals)

Enskilda dataobjekt lagras som en uppsättning nyckel/värde-par. Om du vill lagra och hämta värden anger du motsvarande nyckel:

* [getItem](contexthub-api.md#getitem-key)
* [setItem](contexthub-api.md#setitem-key-value-options)

Observera att anpassade lagringskandidater kan definiera ytterligare funktioner som ger åtkomst till lagringsdata.

>[!NOTE]
>
>ContextHub är som standard inte medveten om den inloggning som för närvarande används på publiceringsservrar, och sådana användare betraktas som&quot;anonyma&quot; av ContextHub.
>
>Du kan göra ContextHub uppmärksam på inloggade användare genom att läsa in profilarkivet. Se [exempelkod i GitHub](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/blob/master/ui.apps/src/main/content/jcr_root/apps/weretail/components/structure/header/clientlib/js/utilities.js).

### ContextHub Eventing {#contexthub-eventing}

ContextHub innehåller ett ramverk för händelser som gör att du automatiskt kan reagera på butikshändelser. Varje lagringsobjekt innehåller ett [`ContextHub.Utils.Eventing`](contexthub-api.md#contexthub-utils-eventing)-objekt som är tillgängligt som butikens [`eventing`](contexthub-api.md#eventing)-egenskap. Använd funktionen [`on`](contexthub-api.md#on-name-handler-selector-triggerforpastevents) eller [`once`](contexthub-api.md#once-name-handler-selector-triggerforpastevents) för att binda en JavaScript-funktion till en butikshändelse.

## Använda kontextnavet för att hantera cookies {#using-context-hub-to-manipulate-cookies}

JavaScript-API:t för Context Hub har stöd för webbläsare för hantering av webbläsarcookies. Namnområdet [`ContextHub.Utils.Cookie`](contexthub-api.md#contexthub-utils-cookie) definierar flera funktioner för att skapa, ändra och ta bort cookies.

## Bestämmer matchade ContextHub-segment {#determining-resolved-contexthub-segments}

Med segmentmotorn för ContextHub kan du avgöra vilket av de registrerade segmenten som matchas i det aktuella sammanhanget. Använd funktionen getResolvedSegments i klassen [`ContextHub.SegmentEngine.SegmentManager`](contexthub-api.md#contexthub-segmentengine-segmentmanager) för att hämta lösta segment. Använd sedan funktionen `getName` eller `getPath` i klassen [`ContextHub.SegmentEngine.Segment`](contexthub-api.md#contexthub-segmentengine-segment) för att testa om det finns ett segment.

### ContextHub-segment {#contexthub-segments}

ContextHub-segment installeras under noden `/conf/<site>/settings/wcm/segments`.

Följande segment installeras med självstudiewebbplatsen [WKND.](getting-started.md)

* sommar
* vintertid

De regler som används för att lösa dessa segment sammanfattas enligt följande:

* Först används arkivet [geolocation](ch-samplestores.md#contexthub-geolocation-sample-store-candidate) för att fastställa användarens latitud.
* Sedan avgör månadsdataobjektet för [surferinfo-arkivet](ch-samplestores.md#contexthub-surferinfo-sample-store-candidate) vilken årstid det är i latituden.

>[!WARNING]
>
>De installerade segmenten tillhandahålls som referenskonfigurationer som hjälper dig att skapa en egen dedikerad konfiguration för ditt projekt. Använd dem inte direkt.

## Debugging ContextHub {#debugging-contexthub}

Det finns flera alternativ för felsökning av ContextHub, bland annat att generera loggar. Mer information finns i [Konfigurera ContextHub.](ch-configuring.md#logging-debug-messages-for-contexthub)

## Se en översikt över ContextHub Framework {#see-an-overview-of-the-contexthub-framework}

ContextHub tillhandahåller en [diagnostiksida](ch-diagnostics.md) där du kan se en översikt över ContextHub-ramverket.
