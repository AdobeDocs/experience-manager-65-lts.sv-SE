---
title: ContextHub JavaScript API Reference
description: ContextHub JavaScript API är tillgängligt för skript när ContextHub-komponenten har lagts till på sidan
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
feature: Context Hub,Developing,Personalization
solution: Experience Manager, Experience Manager Sites
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '4913'
ht-degree: 0%

---

# ContextHub JavaScript API Reference{#contexthub-javascript-api-reference}

ContextHub JavaScript API är tillgängligt för dina skript när [ContextHub-komponenten har lagts till på sidan ](/help/sites-developing/ch-adding.md#adding-contexthub-to-a-page-component).

## ContextHub-konstanter {#contexthub-constants}

Konstantvärden som definieras av ContextHub JavaScript-API:t.

### Händelsekonstanter {#event-constants}

I följande tabell visas namnen på händelser som inträffar för ContextHub Stores. Se även [ContextHub.Utils.Eventing](/help/sites-developing/contexthub-api.md#contexthub-utils-eventing).

| Konstant | Beskrivning | Värde |
|---|---|---|
| ContextHub.Constants.EVENT_NAMESPACE | ContextHub&#39;s event namespace | ch |
| ContextHub.Constants.EVENT_ALL_STORES_READY | Anger att alla nödvändiga butiker är registrerade, initierade och klara att användas | all-stores-ready |
| ContextHub.Constants.EVENT_STORES_PARTIALLY_READY | Anger att inte alla butiker initierades inom en angiven tidsgräns | stores-delvis-ready |
| ContextHub.Constants.EVENT_STORE_REGISTERED | Utlöses när en butik registreras | butiksregistrerad |
| ContextHub.Constants.EVENT_STORE_READY | Anger att butikerna är redo att arbeta. Den aktiveras omedelbart efter registreringen, förutom JSONP-arkiv, där den aktiveras när data hämtas). | butiksklar |
| ContextHub.Constants.EVENT_STORE_UPDATED | Utlöses när en butik uppdaterar sin beständighet | butiksuppdaterad |
| ContextHub.Constants.PERSISTENCE_CONTAINER_NAME | Namn på beständiga behållare | ContextHubPersistence |
| ContextHub.Constants.SERVICE_RAW_RESPONSE_KEY | Lagrar ett särskilt namn på en beständig nyckel där JSON-resultatet i Raw-format lagras | /_/raw-response |
| ContextHub.Constants.SERVICE_RESPONSE_TIME_KEY | Lagrar en specifik tidsstämpel som anger när JSON-data hämtades | /_/response-time |
| ContextHub.Constants.SERVICE_LAST_URL_KEY | Lagrar en specifik URL för JSON-tjänsten som användes under det senaste anropet | /_/url |
| ContextHub.Constants.IS_CONTAINER_EXPANDED | Anger om användargränssnittet för ContextHub är expanderat | /_/container-expanded |

### Konstanter för gränssnittshändelser {#ui-event-constants}

I följande tabell visas namnen på händelser som inträffar för användargränssnittet för ContextHub.

| **Konstant** | **Beskrivning** | **Värde** |
|---|---|---|
| ContextHub.Constants.EVENT_UI_MODE_REGISTERED | Utlöses när ett läge registreras | ui-mode-registered |
| ContextHub.Constants.EVENT_UI_MODE_UNREGISTERED | Utlöses när ett läge avregistreras | ui-mode-unregistered |
| ContextHub.Constants.EVENT_UI_MODE_RENDERER_REGISTERED | Utlöses när en lägesåtergivare registreras | ui-mode-renderer-registered |
| ContextHub.Constants.EVENT_UI_MODE_RENDERER_UNREGISTERED | Utlöses när en lägesåtergivare avregistreras | ui-mode-renderer-unregistered |
| ContextHub.Constants.EVENT_UI_MODE_ADDED | Utlöses när ett nytt läge läggs till | Ui-mode-added |
| ContextHub.Constants.EVENT_UI_MODE_REMOVED | Utlöses när ett läge tas bort | ui-mode-removed |
| ContextHub.Constants.EVENT_UI_MODE_SELECTED | Utlöses när ett läge väljs av användaren | ui-mode-selected |
| ContextHub.Constants.EVENT_UI_MODULE_REGISTERED | Utlöses när en ny modul registreras | ui-module-registered |
| ContextHub.Constants.EVENT_UI_MODULE_UNREGISTERED | Utlöses när en modul avregistreras | ui-module-unregistered |
| ContextHub.Constants.EVENT_UI_MODULE_RENDERER_REGISTERED | Utlöses när en modulåtergivare registreras | ui-module-renderer-registered |
| ContextHub.Constants.EVENT_UI_MODULE_RENDERER_UNREGISTERED | Utlöses när en modulåtergivare avregistreras | ui-module-renderer-unregistered |
| ContextHub.Constants.EVENT_UI_MODULE_ADDED | Utlöses när en ny modul läggs till | ui-module-added |
| ContextHub.Constants.EVENT_UI_MODULE_REMOVED | Utlöses när en modul tas bort | ui-module-removed |
| ContextHub.Constants.EVENT_UI_CONTAINER_ADDED | Utlöses när gränssnittsbehållaren läggs till på sidan | ui-container-added |
| ContextHub.Constants.EVENT_UI_CONTAINER_OPENED | Utlöses när ContextHub-gränssnittet öppnas | ui-container-opened |
| ContextHub.Constants.EVENT_UI_CONTAINER_CLOSED | Utlöses när gränssnittet för ContextHub är komprimerat | ui-container-closed |
| ContextHub.Constants.EVENT_UI_PROPERTY_MODIFIED | Utlöses när en egenskap ändras | ui-property-modified |
| ContextHub.Constants.EVENT_UI_RENDERED | Utlöses varje gång ContextHub-gränssnittet återges (till exempel efter en egenskapsändring) | ui-renderad |
| ContextHub.Constants.EVENT_UI_INITIALIZED | Utlöses när gränssnittsbehållaren initieras | ui-initierad |
| ContextHub.Constants.ACTIVE_UI_MODE | Anger aktivt användargränssnittsläge | /_/active-ui-mode |

## ContextHub JavaScript API Reference {#contexthub-javascript-api-reference-2}

ContextHub-objektet ger åtkomst till alla arkiv.

### Funktioner (ContextHub) {#functions-contexthub}

#### getAllStores() {#getallstores}

Returnerar alla registrerade ContextHub-butiker.

Den här funktionen har inga parametrar.

**Returnerar**

Ett objekt som innehåller alla ContextHub-arkiv. Varje butik är ett objekt som använder samma namn som butiken.

**Exempel**

I följande exempel hämtas alla butiker och sedan hämtas geopositioneringsarkivet:

```
var allStores = ContextHub.getAllStores();
var geoloc = allStores.geolocation
```

#### getStore(name) {#getstore-name}

Hämtar en butik som ett JavaScript-objekt.

**Parametrar**

* **namn:** Det namn som butiken registrerats med.

**Returnerar**

Ett objekt som representerar butiken.

**Exempel**

I följande exempel hämtas geopositioneringsarkivet:

```
var geoloc = ContextHub.getStore("geolocation");
```

## ContextHub.SegmentEngine.Segment {#contexthub-segmentengine-segment}

Representerar ett ContextHub-segment. Använd ContextHub.SegmentEngine.SegmentManager för att hämta segment.

### Funktioner (ContextHub.ContextEngine.Segment) {#functions-contexthub-contextengine-segment}

#### getName() {#getname}

Returnerar segmentets namn som ett strängvärde.

#### getPath() {#getpath}

Returnerar databassökvägen för segmentdefinitionen som ett strängvärde.

## ContextHub.SegmentEngine.SegmentManager {#contexthub-segmentengine-segmentmanager}

Ger åtkomst till ContextHub-segment.

### Funktioner (ContextHub.SegmentEngine.SegmentManager) {#functions-contexthub-segmentengine-segmentmanager}

#### getResolvedSegments() {#getresolvedsegments}

Returnerar de segment som matchas i den aktuella kontexten. Den här funktionen har inga parametrar.

**Returnerar**

En array med ContextHub.SegmentEngine.Segment-objekt.

## ContextHub.Store.Core {#contexthub-store-core}

Basklassen för ContextHub-butiker.

### Egenskaper (ContextHub.Store.Core) {#properties-contexthub-store-core}

#### eventera {#eventing}

Ett [ContextHub.Utils.Eventing](/help/sites-developing/contexthub-api.md#contexthub-utils-eventing)-objekt. Använd det här objektet för bindningsfunktioner för att lagra händelser. Mer information om standardvärde och initiering finns i [init(name,config)](/help/sites-developing/contexthub-api.md#init-name-config).

#### name {#name}

Butikens namn.

#### beständighet {#persistence}

Ett ContextHub.Utils.Persistence-objekt. Mer information om standardvärde och initiering finns i `[init(name,config)](/help/sites-developing/contexthub-api.md#init-name-config).`

### Funktioner (ContextHub.Store.Core) {#functions-contexthub-store-core}

#### addAllItems(träd, alternativ) {#addallitems-tree-options}

Sammanfogar ett dataobjekt eller en array med lagringsdata. Varje nyckel/värde-par i objektet eller arrayen läggs till i arkivet (via funktionen `setItem`):

* **Objekt:** Nycklar är egenskapsnamnen.
* **Matris:** Nycklar är matrisindex.

Observera att värden kan vara objekt.

**Parametrar**

* **träd:** (objekt eller matris) De data som ska läggas till i arkivet.
* **options:** (Object) Ett valfritt objekt med alternativ som skickas till funktionen setItem. Mer information finns i parametern `options` för [setItem(key,value,options)](/help/sites-developing/contexthub-api.md#setitem-key-value-options).

**Returnerar**

Värdet `boolean`:

* Värdet `true` anger att dataobjektet har lagrats.
* Värdet `false` anger att datalagret inte ändras.

#### addReference(key, anotherKey) {#addreference-key-anotherkey}

Skapar en referens från en tangent till en annan. En nyckel kan inte referera till sig själv.

**Parametrar**

* **nyckel:** Nyckeln som refererar till `anotherKey`.

* **en annan nyckel:** Nyckeln som `key` refererar till.

**Returnerar**

Värdet `boolean`:

* Värdet `true` anger att referensen har lagts till.
* Värdet `false` anger att ingen referens har lagts till.

#### announReadiness() {#announcereadiness}

Startar `ready`-händelsen för den här butiken. Den här funktionen har inga parametrar och returnerar inget värde.

#### clear() {#clean}

Tar bort alla data från arkivet. Funktionen har inga parametrar och inget returvärde.

#### getItem(key) {#getitem-key}

Returnerar värdet som är associerat med en nyckel.

**Parametrar**

* **key:** (String) Nyckeln som värdet ska returneras för.

**Returnerar**

Ett objekt som representerar värdet för nyckeln.

#### getKeys(includeInternals) {#getkeys-includeinternals}

Hämtar nycklarna från butiken. Du kan också hämta nycklar som används internt av ContextHub-ramverket.

**Parametrar**

* **includeInternals:** Värdet `true` innehåller internt använda nycklar i resultatet. Dessa tangenter börjar med understrecket (&quot;_&quot;). Standardvärdet är `false`.

**Returnerar**

En array med nyckelnamn ( `string` värden).

#### getReferences() {#getreferences}

Hämtar referenserna från butiken.

**Returnerar**

En array som använder refererande nycklar som index för refererade nycklar:

* Referensnycklar motsvarar parametern `key` i funktionen `addReference`.

* Refererade nycklar motsvarar parametern `anotherKey` i funktionen `addReference`.

#### getTree(includeInternals) {#gettree-includeinternals}

Hämtar dataträdet från butiken. Du kan också inkludera nyckel/värde-par som används internt av ContextHub-ramverket.

**Parametrar**

* `includeInternals:` Värdet `true` innehåller nyckelvärdepar som används internt i resultatet. Nycklarna för dessa data börjar med understrecket (&quot;_&quot;). Standardvärdet är `false`.

**Returnerar**

Ett objekt som representerar dataträdet. Nycklarna är objektets egenskapsnamn.

#### init(name, config) {#init-name-config}

Initierar butiken.

* Ställer in lagringsdata till ett tomt objekt.
* Ställer in butiksreferenserna till ett tomt objekt.
* eventChannel är data:*name*, där *name* är butiksnamnet.

* storeDataKey är /store/*name*, där *name* är butiksnamnet.

**Parametrar**

* **namn:** Butikens namn.
* **config:** Ett objekt som innehåller konfigurationsegenskaper:

   * eventDeferring: Standardvärdet är 32.
   * Händelse: Objektet [ContextHub.Utils.Eventing](/help/sites-developing/contexthub-api.md#contexthub-utils-eventing) för det här arkivet. Standardvärdet är det ContextHub.eventing-objekt som används.
   * persistence: ContextHub.Utils.Persistence-objektet för det här arkivet. Standardvärdet är ContextHub.persistence-objektet.

#### isEventingPaused() {#iseventingpaused}

Avgör om händelser pausas för det här arkivet.

**Returnerar**

Boolesk:

* `true`: Händelser pausas så att inga händelser aktiveras för det här arkivet.
* `false`: Händelser pausas inte så att händelser utlöses för det här arkivet.

#### pauseEventing() {#pauseeventing}

Pausar händelser för arkivet så att inga händelser utlöses. Den här funktionen kräver inga parametrar och returnerar inget värde.

#### removeItem(key, options) {#removeitem-key-options}

Tar bort ett nyckel/värde-par från arkivet.

När en tangent tas bort utlöser funktionen händelsen `data`. Händelsedata innehåller arkivnamnet, namnet på den nyckel som togs bort, det värde som togs bort, det nya värdet för nyckeln (null) och åtgärdstypen &quot;remove&quot;.

Du kan också förhindra att händelsen `data` aktiveras.

**Parametrar**

* **key:** (String) Namnet på nyckeln som ska tas bort.
* **alternativ:** (Objekt) Ett objekt med alternativ. Följande objektegenskaper är giltiga:

   * silent: Värdet `true` förhindrar att händelsen `data` aktiveras. Standardvärdet är `false`.

**Returnerar**

Värdet `boolean`:

* Värdet `true` anger att nyckel/värde-paret har tagits bort.
* Värdet `false` anger att datalagret är oförändrat eftersom nyckeln inte hittades i arkivet.

#### removeReference(key) {#removereference-key}

Tar bort en referens från arkivet.

**Parametrar**

* **nyckel:** Nyckelreferensen som ska tas bort. Den här parametern motsvarar parametern `key` i funktionen `addReference`.

**Returnerar**

Värdet `boolean`:

* Värdet `true` anger att referensen har tagits bort.
* Värdet `false` anger att nyckeln inte var giltig och att arkivet inte är förändrat.

#### reset(keepRemainingData) {#reset-keepremainingdata}

Återställer de ursprungliga värdena för butikens beständiga data. Du kan också ta bort alla andra data från arkivet. Händelser pausas för det här arkivet när arkivet återställs. Den här funktionen returnerar inget värde.

Initialvärden anges i egenskapen initialValues för det config-objekt som används för att instansiera lagringsobjektet.

**Parametrar**

* **keepRemainingData:** (Boolean) Ett värde på true gör att icke-initiala data bevaras. Värdet false medför att alla data tas bort utom de ursprungliga värdena.

Återställer de ursprungliga värdena för butikens beständiga data. Du kan också ta bort alla andra data från arkivet. Händelser pausas för det här arkivet när arkivet återställs. Den här funktionen returnerar inget värde.

Initialvärden anges i egenskapen initialValues för det config-objekt som används för att instansiera lagringsobjektet.

**Parametrar**

* keepRemainingData: (Boolean) Värdet true gör att icke-initiala data bevaras. Värdet false medför att alla data tas bort utom de ursprungliga värdena.

#### resolveReference(key, retry) {#resolvereference-key-retry}

Hämtar en refererad nyckel. Du kan också ange antalet iterationer som ska användas för att matcha den bästa matchningen.

**Parametrar**

* **key:** (String) Nyckeln som referensen ska matchas för. Parametern `key` motsvarar parametern `key` i funktionen `addReference`.

* **försök igen:** (Number) Antalet iterationer som ska användas.

**Returnerar**

Ett `string`-värde som representerar den refererade nyckeln. Om ingen referens matchas returneras värdet för parametern `key`.

#### resumeEventing() {#resumeeventing}

Återupptar händelser för den här butiken så att händelser utlöses. Den här funktionen definierar inga parametrar och returnerar inget värde.

#### setItem(key, value, options) {#setitem-key-value-options}

Lägger till ett nyckel/värde-par i butiken.

Startar bara `data`-händelsen om värdet för nyckeln skiljer sig från värdet som för närvarande lagras för nyckeln. Du kan också förhindra utlösaren av `data`-händelsen.

Händelsedata innehåller butiksnamnet, nyckeln, det tidigare värdet, det nya värdet och åtgärdstypen för `set`.

**Parametrar**

* **key:** (String) Namnet på nyckeln.
* **alternativ:** (Objekt) Ett objekt med alternativ. Följande objektegenskaper är giltiga:

   * silent: Värdet `true` förhindrar att händelsen `data` aktiveras. Standardvärdet är `false`.

* **värde:** (Objekt) Värdet som ska associeras med nyckeln.

**Returnerar**

Värdet `boolean`:

* Värdet `true` anger att dataobjektet har lagrats.
* Värdet `false` anger att datalagret inte ändras.

## ContextHub.Store.JSONPStore {#contexthub-store-jsonpstore}

Ett arkiv som innehåller JSON-data. Data hämtas från en extern JSONP-tjänst, eller eventuellt från en tjänst som returnerar JSON-data. Ange tjänstinformationen med funktionen [`init`](/help/sites-developing/contexthub-api.md#init-name-config) när du skapar en instans av den här klassen.

Butiken använder In-Memory Persiststance (JavaScript variable). Lagringsdata är bara tillgängliga under sidans livstid.

ContextHub.Store.JSONPStore utökar [ContextHub.Store.Core](/help/sites-developing/contexthub-api.md#contexthub-store-core) och ärver funktionerna för den klassen.

### Funktioner (ContextHub.Store.JSONPStore) {#functions-contexthub-store-jsonpstore}

#### configureService(serviceConfig, override) {#configureservice-serviceconfig-override}

Konfigurerar informationen för anslutning till den JSONP-tjänst som det här objektet använder. Du kan antingen uppdatera eller ersätta den befintliga konfigurationen. Funktionen returnerar inget värde.

**Parametrar**

* **serviceConfig:** Ett objekt som innehåller följande egenskaper:

   * host: (String) Servernamnet eller IP-adressen.
   * jsonp: (Boolean) Värdet true anger att tjänsten är en JSONP-tjänst, i annat fall false. När värdet är true är {callback: &quot;ContextHub.Callbacks.Objektet *Object.name* läggs till i objektet service.params.
   * params: (Object) URL-parametrar representeras som objektegenskaper. Parameternamn är egenskapsnamn och parametervärden är egenskapsvärden.
   * path: (String) Sökvägen till tjänsten.
   * port: (Number) Tjänstens portnummer.
   * secure: (String eller Boolean) Anger vilket protokoll som ska användas för tjänstens URL:

      * auto: //
      * true: https://
      * false: https://

* **åsidosätt:** (Boolean). Värdet `true` gör att den befintliga tjänstkonfigurationen ersätts av egenskaperna för `serviceConfig`. Värdet `false` gör att de befintliga tjänstkonfigurationsegenskaperna sammanfogas med egenskaperna för `serviceConfig`.

#### getRawResponse() {#getrawresponse}

Returnerar det obearbetade svar som har cachelagrats sedan det senaste anropet till JSONP-tjänsten. Funktionen kräver inga parametrar.

**Returnerar**

Ett objekt som representerar råsvaret.

#### getServiceDetails() {#getservicedetails}

Hämtar tjänstobjektet för det här ContextHub.Store.JSONPStore-objektet. Tjänsteobjektet innehåller all information som krävs för att skapa tjänst-URL:en.

**Returnerar**

Ett objekt med följande egenskaper:

* **host:** (String) Servernamnet eller IP-adressen.
* **jsonp:** (Boolean) Värdet true anger att tjänsten är en JSONP-tjänst, annars false. När värdet är true är {callback: &quot;ContextHub.Callbacks.Objektet *Object.name* läggs till i objektet service.params.

* **parametrar:** (Object) URL-parametrar representeras som objektegenskaper. Parameternamn är egenskapsnamn och parametervärden är egenskapsvärden.
* **sökväg:** (sträng) Sökvägen till tjänsten.
* **port:** (nummer) Tjänstens portnummer.
* **secure:** (String eller Boolean) Anger vilket protokoll som ska användas för tjänstens URL:

   * auto: //
   * true: https://
   * false: https://

#### getServiceURL(resolve) {#getserviceurl-resolve}

Hämtar URL:en för JSONP-tjänsten.

**Parametrar**

* **resolve:** (Boolean) Avgör om matchade parametrar ska inkluderas i URL:en. Värdet `true` löser parametrarna och `false` gör det inte.

**Returnerar**

Ett `string`-värde som representerar tjänst-URL:en.

#### init(name, config) {#init-name-config-1}

initierar ContextHub.Store.JSONPStore-objektet.

**Parametrar**

* **name:** (String) Butikens namn.
* **config:** (Object) Ett objekt som innehåller egenskapen service. JSONPStore-objektet använder egenskaperna för objektet `service` för att skapa URL:en för JSONP-tjänsten:

   * eventDeferring: 32.
   * Händelse: ContextHub.Utils.Eventing-objektet för det här arkivet. Standardvärdet är objektet `ContextHub.eventing`.
   * persistence: ContextHub.Utils.Persistence-objektet för det här arkivet. Som standard används minnesbeständighet (JavaScript-objekt).
   * service: (Object)

      * host: (String) Servernamnet eller IP-adressen.
      * jsonp: (Boolean) Värdet true anger att tjänsten är en JSONP-tjänst, i annat fall false. När värdet är true läggs objektet `{callback: "ContextHub.Callbacks.*Object.name*}` till i `service.params`.
      * params: (Object) URL-parametrar representeras som objektegenskaper. Parameternamn och värden är objektegenskapsnamnen och -värdena.
      * path: (String) Sökvägen till tjänsten.
      * port: (Number) Tjänstens portnummer.
      * secure: (String eller Boolean) Anger vilket protokoll som ska användas för tjänstens URL:

         * auto: //
         * true: https://
         * false: https://

      * timeout: (Number) Den väntetid i millisekunder som JSONP-tjänsten ska svara före timeout.
      * ttl: Den kortaste tiden i millisekunder som går mellan anrop till JSONP-tjänsten. (Se funktionen [queryService](/help/sites-developing/contexthub-api.md#queryservice-reload)).

#### queryService(reload) {#queryservice-reload}

Frågar fjärrtjänsten JSONP och cachelagrar svaret. Om tiden sedan det föregående anropet till den här funktionen är mindre än värdet `config.service.ttl` anropas inte tjänsten och det cachelagrade svaret ändras inte. Du kan också tvinga tjänsten att anropas. Egenskapen `config.service.ttl` anges när funktionen [init](/help/sites-developing/contexthub-api.md#init-name-config) anropas för att initiera arkivet.

Startar ready-händelsen när frågan är klar. Om JSONP-tjänstens URL inte är inställd händer ingenting.

**Parametrar**

* **reload:** (Boolean) Värdet true tar bort det cachelagrade svaret och tvingar JSONP-tjänsten att anropas.

#### återställ {#reset}

Återställer de ursprungliga värdena för butikens beständiga data och anropar sedan JSONP-tjänsten. Du kan också ta bort alla andra data från arkivet. Händelser pausas för det här arkivet medan de ursprungliga värdena återställs. Den här funktionen returnerar inget värde.

Initialvärden anges i egenskapen initialValues för det config-objekt som används för att instansiera lagringsobjektet.

**Parametrar**

* **keepRemainingData:** (Boolean) Ett värde på true gör att icke-initiala data bevaras. Värdet false medför att alla data tas bort utom de ursprungliga värdena.

#### resolveParameter(f) {#resolveparameter-f}

Matchar den angivna parametern.

## ContextHub.Store.PersistedJSONPStore {#contexthub-store-persistedjsonpstore}

ContextHub.Store.PersistedJSONPStore utökar [ContextHub.Store.JSONPStore](/help/sites-developing/contexthub-api.md#contexthub-store-jsonpstore) så att den ärver alla funktioner i den klassen. Data som hämtas från JSONP-tjänsten sparas dock enligt konfigurationen för ContextHub-beständighet. (Se [Persistenslägen](/help/sites-developing/ch-adding.md#persistence-modes).)

## ContextHub.Store.PersistedStore {#contexthub-store-persistedstore}

ContextHub.Store.PersistedStore utökar [ContextHub.Store.Core](/help/sites-developing/contexthub-api.md#contexthub-store-core) så att den ärver alla funktioner i den klassen. Data i det här arkivet bevaras enligt konfigurationen för ContextHub-beständighet.

## ContextHub.Store.SessionStore {#contexthub-store-sessionstore}

ContextHub.Store.SessionStore utökar [ContextHub.Store.Core](/help/sites-developing/contexthub-api.md#contexthub-store-core) så att den ärver alla funktioner i den klassen. Data i det här arkivet sparas med InMemory Perstance (JavaScript-objekt).

## ContextHub.UI {#contexthub-ui}

Hanterar gränssnittsmoduler och gränssnittsmodulrenderare.

### Funktioner (ContextHub.UI) {#functions-contexthub-ui}

#### registerRenderer(moduleType, renderer, dontRender) {#registerrenderer-moduletype-renderer-dontrender}

Registrerar en gränssnittsmodulrenderare med ContextHub. När återgivaren har registrerats kan den användas för att [skapa gränssnittsmoduler](ch-configuring.md#adding-a-ui-module). Använd den här funktionen när du [utökar ContextHub.UI.BaseModuleRenderer](/help/sites-developing/ch-extend.md#creating-contexthub-ui-module-types) för att skapa en anpassad renderare för användargränssnittsmodul.

**Parametrar**

* **moduleType:** (String) Identifieraren för gränssnittsmodulens renderare. Om en renderare redan är registrerad med det angivna värdet avregistreras den befintliga renderaren innan den registreras.
* **renderare:** (String) Namnet på den klass som renderar gränssnittsmodulen.
* **dontRender:** (Boolean) anges till `true` för att förhindra att ContextHub-gränssnittet återges efter att renderaren har registrerats. Standardvärdet är `false`.

**Exempel**

I följande exempel registreras en renderare som modultypen contexthub.browserinfo.

```
ContextHub.UI.registerRenderer('contexthub.browserinfo', new SurferinfoRenderer());
```

## ContextHub.Utils.Cookie {#contexthub-utils-cookie}

En verktygsklass för interaktion med cookies.

### Funktioner (ContextHub.Utils.Cookie) {#functions-contexthub-utils-cookie}

#### exists(key) {#exists-key}

Avgör om det finns en cookie.

**Parametrar**

* **nyckel:** A `String` som innehåller nyckeln för den cookie som du testar för.

**Returnerar**

Värdet `boolean` är true och anger att cookien finns.

**Exempel**

```
if (ContextHub.Utils.Cookie.exists("name")) {
   // conditionally-executed code
}
```

#### getAllItems(filter) {#getallitems-filter}

Returnerar alla cookies som har nycklar som matchar ett filter.

**Parametrar**

* (Valfritt) **filter:** Villkor för matchning av cookie-nycklar. Om du vill returnera alla cookies anger du inget värde. Följande typer stöds:

   * Sträng: Strängen jämförs med cookie-nyckeln.
   * Array: Varje objekt i arrayen är ett filter.
   * Ett RegExp-objekt: Objektets testfunktion används för att matcha cookie-nycklar.
   * En funktion: En funktion som testar en cookie-nyckel för en matchning. Funktionen måste ta cookie-nyckeln som parameter och returnera true om testet bekräftar en matchning.

**Returnerar**

Ett objekt med cookies. Objektegenskaper är cookie-nycklar och nyckelvärden är cookie-värden.

**Exempel**

```
ContextHub.Utils.Cookie.getAllItems([/^cq-authoring/, /^cq-editor/])
```

#### getItem(key) {#getitem-key-1}

Returnerar ett cookie-värde.

**Parametrar**

* **nyckel:** Nyckeln till den cookie som du vill ha värdet för.

**Returnerar**

Cookie-värdet eller `null` om ingen cookie hittades för nyckeln.

**Exempel**

```
ContextHub.Utils.Cookie.getItem("name");
```

#### getKeys(filter) {#getkeys-filter}

Returnerar en array med nycklarna för befintliga cookies som matchar ett filter.

**Parametrar**

* **filter:** Villkor för matchning av cookie-nycklar. Följande typer stöds:

   * Sträng: Strängen jämförs med cookie-nyckeln.
   * Array: Varje objekt i arrayen är ett filter.
   * Ett RegExp-objekt: Objektets testfunktion används för att matcha cookie-nycklar.
   * En funktion: En funktion som testar en cookie-nyckel för en matchning. Funktionen måste använda cookie-nyckeln som parameter och returnera `true` om testet bekräftar en matchning.

**Returnerar**

En array med strängar där varje sträng är nyckeln till en cookie som matchar filtret.

**Exempel**

```
ContextHub.Utils.Cookie.getKeys([/^cq-authoring/, /^cq-editor/])
```

#### removeItem(key, options) {#removeitem-key-options-1}

Tar bort en cookie. Om du vill ta bort cookien anges värdet till en tom sträng och förfallodatumet anges till dagen före det aktuella datumet.

**Parametrar**

* **nyckel:** Ett `String`-värde som representerar nyckeln till den cookie som ska tas bort.

* **alternativ:** Ett objekt som innehåller egenskapsvärden för konfiguration av cookie-attributen. Mer information finns i funktionen ` [setItem](/help/sites-developing/contexthub-api.md#setitem-key-value-options)`. Egenskapen `expires` har ingen effekt.

**Returnerar**

Den här funktionen returnerar inget värde.

**Exempel**

```
ContextHub.Utils.Cookie.vanish([/^cq-authoring/, 'cq-scrollpos']);
```

#### setItem(key, value, options) {#setitem-key-value-options-1}

Skapar en cookie med den angivna nyckeln och det angivna värdet och lägger till cookien i det aktuella dokumentet. Du kan också ange alternativ som konfigurerar attributen för cookien.

**Parametrar**

* **nyckel:** En sträng som innehåller nyckeln för cookien.
* **värde:** En sträng som innehåller cookie-värdet.
* **alternativ:** (Valfritt) Ett objekt som innehåller någon av följande egenskaper som konfigurerar cookie-attributen:

   * förfaller: Ett `date`- eller `number`-värde som anger när cookien förfaller. Ett datumvärde anger den absoluta förfallotiden. Ett tal (i dagar) anger förfallotiden till den aktuella tiden plus talet. Standardvärdet är `undefined`.
   * secure: Ett `boolean`-värde som anger cookie-attributet `Secure`. Standardvärdet är `false`.
   * sökväg: Ett `String`-värde som ska användas som `Path`-attribut för cookien. Standardvärdet är `undefined`.

**Returnerar**

Cookien med det angivna värdet.

**Exempel**

```
ContextHub.Utils.Cookie.setItem("name", "mycookie", {
    expires: 3,
    domain: 'localhost',
    path: '/some/directory',
    secure: true
});
```

#### vDanish(filter, options) {#vanish-filter-options}

Tar bort alla cookies som matchar ett visst filter. Cookies matchas med funktionen getKeys och tas bort med funktionen removeItem.

**Parametrar**

* **filter:** Argumentet `filter` som ska användas i anropet till funktionen `[getKeys](/help/sites-developing/contexthub-api.md#getkeys-filter)`.

* **options:** Argumentet `options` som ska användas i anropet till funktionen `[removeItem](/help/sites-developing/contexthub-api.md#removeitem-key-options)`.

**Returnerar**

Den här funktionen returnerar inget värde.

## ContextHub.Utils.Eventing {#contexthub-utils-eventing}

Gör att du kan binda och koppla upp funktioner till ContextHub-butikshändelser. Få åtkomst till ContextHub.Utils.Eventing-objekt för en butik med egenskapen [eventing](/help/sites-developing/contexthub-api.md#eventing) för arkivet.

### Funktioner (ContextHub.Utils.Eventing) {#functions-contexthub-utils-eventing}

#### off(name, selector) {#off-name-selector}

Avbinder en funktion från en händelse.

**Parametrar**

* **namn:** Namnet [på den händelse](/help/sites-developing/contexthub-api.md#contexthub-utils-eventing) som du avbinder funktionen för.

* **väljare:** Väljaren som identifierar bindningen. (Se parametern `selector` för funktionerna [ on](/help/sites-developing/contexthub-api.md#on-name-handler-selector-triggerforpastevents) och [ once](/help/sites-developing/contexthub-api.md#once-name-handler-selector-triggerforpastevents)).

**Returnerar**

Den här funktionen returnerar inget värde.

#### on(name, handler, selector, triggerForPastEvents) {#on-name-handler-selector-triggerforpastevents}

Bindar en funktion till en händelse. Funktionen anropas varje gång händelsen inträffar. Funktionen kan också anropas för händelser som har inträffat tidigare, innan bindningen har upprättats.

**Parametrar**

* **name:** (String) Namnet [på händelsen ](/help/sites-developing/contexthub-api.md#contexthub-utils-eventing) som du binder funktionen till.

* **hanterare:** (Funktion) Funktionen som ska bindas till händelsen.
* **selector:** (String) En unik identifierare för bindningen. Du behöver väljaren för att identifiera bindningen om du vill använda funktionen `off` för att ta bort bindningen.

* **triggerForPastEvents:** (Boolean) Anger om hanteraren ska köras för händelser som har inträffat tidigare. Värdet `true` anropar hanteraren för tidigare händelser. Värdet `false` anropar hanteraren för framtida händelser. Standardvärdet är `true`.

**Returnerar**

När argumentet `triggerForPastEvents` är `true` returnerar den här funktionen ett `boolean`-värde som anger om händelsen har inträffat tidigare:

* `true`: Händelsen inträffade tidigare och hanteraren anropas.
* `false`: Händelsen har inte inträffat tidigare.

Om `triggerForPastEvents` är `false` returnerar den här funktionen inget värde.

**Exempel**

I följande exempel binds en funktion till händelsen data i geolocation-arkivet. Funktionen fyller i ett element på sidan med värdet för latituddataobjektet från butiken.

```
<div class="location">
    <p>latitude: <span id="lat"></span></p>
</div>

<script>
    var geostore = ContextHub.getStore("geolocation");
    geostore.eventing.on(ContextHub.Constants.EVENT_DATA_UPDATE,getlat,"getlat");

    function getlat(){
       latitude = geostore.getItem("latitude");
       $("#lat").html(latitude);
    }
</script>
```

#### once(name, handler, selector, triggerForPastEvents) {#once-name-handler-selector-triggerforpastevents}

Bindar en funktion till en händelse. Funktionen anropas bara en gång för den första förekomsten av händelsen. Funktionen kan också anropas för den händelse som har inträffat tidigare, innan bindningen har upprättats.

**Parametrar**

* **name:** (String) Namnet [på händelsen ](/help/sites-developing/contexthub-api.md#contexthub-utils-eventing) som du binder funktionen till.

* **hanterare:** (Funktion) Funktionen som ska bindas till händelsen.
* **selector:** (String) En unik identifierare för bindningen. Du behöver väljaren för att identifiera bindningen om du vill använda funktionen `off` för att ta bort bindningen.

* **triggerForPastEvents:** (Boolean) Anger om hanteraren ska köras för händelser som har inträffat tidigare. Värdet `true` anropar hanteraren för tidigare händelser. Värdet `false` anropar hanteraren för framtida händelser. Standardvärdet är `true`.

**Returnerar**

När argumentet `triggerForPastEvents` är `true` returnerar den här funktionen ett `boolean`-värde som anger om händelsen har inträffat tidigare:

* `true`: Händelsen inträffade tidigare och hanteraren anropas.
* `false`: Händelsen har inte inträffat tidigare.

Om `triggerForPastEvents` är `false` returnerar den här funktionen inget värde.

## ContextHub.Utils.inheritance {#contexthub-utils-inheritance}

En verktygsklass som gör att ett objekt kan ärva egenskaper och metoder för ett annat objekt.

### Funktioner (ContextHub.Utils.arv) {#functions-contexthub-utils-inheritance}

#### inherit(child, parent) {#inherit-child-parent}

Gör att ett objekt ärver egenskaper och metoder för ett annat objekt.

**Parametrar**

* **child:** (Object) Det objekt som ärver.
* **parent:** (Object) Det objekt som definierar de egenskaper och metoder som ärvs.

## ContextHub.Utils.JSON {#contexthub-utils-json}

Tillhandahåller funktioner för att serialisera objekt till JSON-format och deserialisera JSON-strängar till objekt.

### Funktioner (ContextHub.Utils.JSON) {#functions-contexthub-utils-json}

#### parse(data) {#parse-data}

Tolkar ett strängvärde som JSON och konverterar det till ett javascript-objekt.

**Parametrar**

* **data:** Ett strängvärde i JSON-format.

**Returnerar**

Ett JavaScript-objekt.

**Exempel**

Koden `ContextHub.Utils.JSON.parse("{'city':'Basel','country':'Switzerland','population':'173330'}");` returnerar följande objekt:

```
Object {
   city: "Basel",
   country: "Switzerland",
   population: 173330
}
```

#### stringify(data) {#stringify-data}

Serialiserar JavaScript-värden och -objekt till strängvärden i JSON-format.

**Parametrar**

* **data:** Värdet eller objektet som ska serialiseras. Den här funktionen stöder värden för booleskt värde, matris, tal, sträng och datum.

**Returnerar**

Det serialiserade strängvärdet. När `data` är ett R `egExp`-värde returnerar den här funktionen ett tomt objekt. Returnerar `undefined` när `data` är en funktion.

**Exempel**

Följande kod returnerar `"{'city':'Basel','country':'Switzerland','population':'173330'}":`

```
ContextHub.Utils.JSON.stringify({
   city: "Basel",
   country: "Switzerland",
   population: 173330
});
```

## ContextHub.Utils.JSON.tree {#contexthub-utils-json-tree}

Den här klassen underlättar manipuleringen av dataobjekt som ska lagras eller hämtas från ContextHub-arkiv.

### Funktioner (ContextHub.Utils.JSON.tree) {#functions-contexthub-utils-json-tree}

#### addAllItems() {#addallitems}

Skapar en kopia av ett dataobjekt och lägger till dataträdet från ett andra objekt. Funktionen returnerar kopian och ändrar inte något av de ursprungliga objekten. När dataträden för de två objekten innehåller identiska nycklar skriver värdet för det andra objektet över värdet för det första objektet.

**Parametrar**

* **träd:** Det objekt som kopieras.
* **secondTree:** Objektet som sammanfogas med kopian av `tree`-objektet.

**Returnerar**

Ett objekt som innehåller sammanfogade data.

#### cleanup() {#cleanup}

Skapar en kopia av ett objekt, söker efter och tar bort objekt i dataträdet som inte innehåller några värden, null-värden eller odefinierade värden och returnerar kopian.

**Parametrar**

* **träd:** Det objekt som ska rensas.

**Returnerar**

En kopia av trädet som är städat.

#### getItem() {#getitem}

Hämtar värdet från ett objekt för nyckeln.

**Parametrar**

* **träd:** Dataobjektet.
* **nyckel:** Nyckeln för värdet som du vill hämta.

**Returnerar**

Värdet som motsvarar nyckeln. När nyckeln har underordnade nycklar returnerar den här funktionen ett komplext objekt. När värdetypen för nyckeln är `undefined` returneras `null`.

**Exempel**

Tänk på följande JavaScript-objekt:

```
myObject {
  user: {
    location: {
      city: "Basel",
        details: {
          population: 173330,
          elevation: 260
        }
      }
    }
  }
```

Följande exempelkod returnerar värdet `260`:

```
ContextHub.Utils.JSON.tree.getItem(myObject, "/user/location/details/elevation");
```

Följande exempelkod hämtar värdet för en nyckel som har underordnade nycklar:

```
ContextHub.Utils.JSON.tree.getItem(myObject, "/user");
```

Funktionen returnerar följande objekt:

```
Object {
  location: {
    city: "Basel",
    details: {
      population: 173330,
      elevation: 260
    }
  }
}
```

#### getKeys() {#getkeys}

Hämtar alla nycklar från ett objekts dataträd. Om du vill kan du bara hämta nycklarna för de underordnade nycklarna för en viss nyckel. Du kan också ange en sorteringsordning för de hämtade nycklarna.

**Parametrar**

* **träd:** Det objekt som nycklarna i dataträdet ska hämtas från.
* **överordnad:** (Valfritt) Nyckeln till ett objekt i dataträdet som du vill hämta nycklarna för de underordnade objekten för.
* **order:** (valfritt) En funktion som fastställer sorteringsordningen för de returnerade tangenterna. (Se [Array.prototype.sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) i Mozilla Developer Network.)

**Returnerar**

En array med nycklar.

**Exempel**

Tänk på följande objekt:

```
myObject {
  location: {
    weather: {
      temperature: "28C",
      humidity: "77%",
      precipitation: "10%",
      wind: "8km/h"
    },
    city: "Basel",
    country: "Switzerland",
    longitude: 7.5925727,
    latitude: 47.557421
  }
}
```

Skriptet `ContextHub.Utils.JSON.tree.getKeys(myObject);` returnerar följande array:

```
["/location", "/location/city", "/location/country", "/location/latitude", "/location/longitude", "/location/weather", "/location/weather/humidity", "/location/weather/precipitation", "/location/weather/temperature", "/location/weather/wind"]
```

#### removeItem() {#removeitem}

Skapar en kopia av ett givet objekt, tar bort den angivna grenen från dataträdet och returnerar den ändrade kopian.

**Parametrar**

* träd: Ett dataobjekt.
* key: The key to remove.

**Returnerar**

En kopia av det ursprungliga dataobjektet där tangenten har tagits bort.

**Exempel**

Tänk på följande objekt:

```
myObject {
  one: {
    foo: "bar",
    two: {
      three: {
        four: {
          five: 5,
          six: 6
        }
      }
    }
  }
}
```

Följande exempelskript tar bort grenen /en/två/tre/fyra från dataträdet:

```
myObject = ContextHub.Utils.JSON.tree.removeItem(myObject, "/one/two/three/four");
```

Funktionen returnerar följande objekt:

```
myObject {
  one: {
    foo: "bar"
  }
}
```

#### sanitizeKey(key) {#sanitizekey-key}

Ändrar strängvärden så att de kan användas som nycklar. Om du vill sanera en sträng utför den här funktionen följande åtgärder:

* Minskar flera efterföljande snedstreck till ett enda snedstreck.
* Tar bort mellanrum från början och slutet av strängen.
* Delar upp resultatet i en array med strängar som avgränsas av snedstreck.

Använd den resulterande arrayen för att skapa en användbar nyckel.  **Parametrar**

* **nyckel:** Den `string` som ska saneras.

**Returnerar**

En array med `string` värden där varje sträng är den del av `key` som avgränsades av snedstreck. representerar den sanerade nyckeln. Om den sanerade arrayen har längden noll returnerar funktionen `null`.

**Exempel**

Följande kod sanerar en sträng för att skapa arrayen `["this", "is", "a", "path"]` och genererar sedan nyckeln `"/this/is/a/path"` från arrayen:

```
var key = " / this////is/a/path ";
ContextHub.Utils.JSON.tree.sanitizeKey(key)
"/" + ContextHub.Utils.JSON.tree.sanitizeKey(key).join("/");
```

#### setItem(tree, key, value) {#setitem-tree-key-value}

Lägger till ett nyckel/värde-par i dataträdet för en kopia av ett objekt. Mer information om dataträd finns i [Persistence](/help/sites-developing/contexthub.md#persistence).

**Parametrar**

* träd: Ett dataobjekt.
* nyckel: Den tangent som ska associeras med värdet som du lägger till. Nyckeln är sökvägen till objektet i dataträdet. Den här funktionen anropar `ContextHub.Utils.JSON.tree.sanitize` för att rensa nyckeln innan den läggs till.
* värde: Det värde som ska läggas till i dataträdet.

**Returnerar**

En kopia av objektet `tree` som innehåller paret `key`/ `value`.

**Exempel**

Tänk på följande JavaScript-kod:

```
var myObject = {
     user: {
        location: {
           city: "Basel"
           }
        }
     };

var myKey = "/user/location/details";

var myValue = {
      population: 173330,
      elevation: 260
     };

myObject = ContextHub.Utils.JSON.tree.setItem(myObject, myKey, myValue);
```

Objektet myObject har följande värde:

## ContextHub.Utils.storeCandidates {#contexthub-utils-storecandidates}

Här kan du registrera butikskandidater och få registrerade butikskandidater.

### Funktioner (ContextHub.Utils.storeCandidates) {#functions-contexthub-utils-storecandidates}

#### getRegisteredCandidates(storeType) {#getregisteredcandidates-storetype}

Returnerar de butikstyper som är registrerade som butikskandidater. Hämta antingen registrerade annulleringar av en viss butikstyp eller alla butikstyper.

**Parametrar**

* **storeType:** (String) Namnet på butikstypen. Se parametern `storeType` för funktionen [`ContextHub.Utils.storeCandidates.registerStoreCandidate`](/help/sites-developing/contexthub-api.md#contexthub-utils-storecandidates).

**Returnerar**

Ett objekt av lagringstyper. Objektegenskaperna är lagringstypsnamnen och egenskapsvärdena är en array med registrerade lagringskandidater.

#### getStoreFromCandidates(storeType) {#getstorefromcandidates-storetype}

Returnerar en butikstyp från de registrerade anbudssökande. Om mer än en lagringstyp med samma namn återställs returnerar funktionen den lagringstyp som har högst prioritet.

**Parametrar**

* storeType: (String) Namnet på lagringskandidaten. Se parametern `storeType` för funktionen [`ContextHub.Utils.storeCandidates.registerStoreCandidate`](/help/sites-developing/contexthub-api.md#registerstorecandidate-store-storetype-priority-applies).

**Returnerar**

Ett objekt som representerar den registrerade butikskandidaten. Om den begärda lagringstypen inte har registrerats genereras ett fel.

#### getSupportedStoreTypes() {#getsupportedstoretypes}

Returnerar namnen på de butikstyper som är registrerade som butikskandidater. Den här funktionen kräver inga parametrar.

**Returnerar**

En array med strängvärden, där varje sträng är den storetype som en lagringskandidat registrerades med. Se parametern `storeType` för funktionen [`ContextHub.Utils.storeCandidates.registerStoreCandidate`](/help/sites-developing/contexthub-api.md#contexthub-utils-storecandidates).

#### registerStoreCandidate(store, storeType, priority, apply) {#registerstorecandidate-store-storetype-priority-applies}

Registrerar ett lagringsobjekt som ett arkivkandidat med ett namn och en prioritet.

Prioriteten är ett tal som anger vikten av butiker med samma namn. När en butikskandidat registreras med samma namn som en redan registrerad butikskandidat, används den som har den högre prioriteten. När du registrerar en butikskandidater registreras butiken endast om prioriteten är högre än de registrerade butikskandidaterna med samma namn.

**Parametrar**

* **store:** (Object) Det lagringsobjekt som ska registreras som en butikskandidater.
* **storeType:** (String) Namnet på lagringskandidaten. Detta värde krävs när en instans av lagringskandidaten skapas.
* **prioritet:** (tal) Prioriteten för lagringskandidaten.
* **gäller:** (Funktion) Den funktion som ska anropas som utvärderar butikens tillämplighet i den aktuella miljön. Funktionen måste returnera `true` om arkivet är tillämpligt, i annat fall `false`. Standardvärdet är en funktion som returnerar true: `function() {return true;}`

**Exempel**

```
ContextHub.Utils.storeCandidates.registerStoreCandidate(myStoreCandidate,
                                'contexthub.mystorecandiate', 0);
```
