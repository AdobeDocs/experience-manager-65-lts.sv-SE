---
title: JavaScript API för klientkontext
description: Läs mer om JavaScript API för klientkontext i Adobe Experience Manager.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
feature: Context Hub,Developing,Personalization
solution: Experience Manager, Experience Manager Sites
role: Developer
exl-id: db4a4a1a-e014-4865-ab8c-d8a5aaefd93a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3106'
ht-degree: 0%

---

# JavaScript API för klientkontext{#client-context-javascript-api}

## CQ_Analytics.ClientContextMgr {#cq-analytics-clientcontextmgr}

Objektet CQ_Analytics.ClientContextMgr är en singleton som innehåller en uppsättning självregistrerade sessionsarkiv och innehåller metoder för att registrera, behålla och hantera sessionsarkiv.

Utökar CQ_Analytics.PersistedSessionStore.

### Metoder {#methods}

#### getRegisteredStore(name) {#getregisteredstore-name}

Returnerar ett sessionsarkiv med ett angivet namn. Se även [Åtkomst till ett sessionsarkiv](/help/sites-developing/client-context.md#accessing-session-stores).

**Parametrar**

* name: String. Namnet på sessionsarkivet.

**Returnerar**

Ett CQ_Analytics.SessionStore-objekt som representerar sessionsarkivet för det angivna namnet. Returnerar `null` när det inte finns något arkiv med det angivna namnet.

#### register(sessionstore) {#register-sessionstore}

Registrerar ett sessionsarkiv med klientkontext. Aktiverar händelserna storeregister och storeupdate när de är klara.

**Parametrar**

* sessionstore: CQ_Analytics.SessionStore. Det sessionsarkivobjekt som ska registreras.

**Returnerar**

Inget returvärde.

## CQ_Analytics.ClientContextUtils {#cq-analytics-clientcontextutils}

Innehåller metoder för avlyssning av aktivering och registrering av sessionsarkiv. Se även [Kontrollera att ett sessionsarkiv är definierat och initierat](/help/sites-developing/client-context.md#checking-that-a-session-store-is-defined-and-initialized).

### Metoder {#methods-1}

#### onStoreInitialized(storeName, callback, delay) {#onstoreinitialized-storename-callback-delay}

Registrerar en callback-funktion som anropas när ett sessionsarkiv initieras. För butiker som initieras flera gånger anger du en återkopplingsfördröjning så att återanropsfunktionen bara anropas en gång:

* När arkivet initieras under fördröjningsperioden för en tidigare initiering avbryts det föregående funktionsanropet och funktionen anropas igen för den aktuella initieringen.
* Om fördröjningsperioden utgår innan en efterföljande initiering inträffar, körs callback-funktionen två gånger.

Ett sessionsarkiv är till exempel baserat på ett JSON-objekt och hämtas via en JSON-begäran. Följande initieringsscenarier är möjliga:

* Begäran har slutförts, data har hämtats och lästs in i arkivet. I det här fallet sker initieringen en gång.
* Begäran misslyckas (timeout). I det här fallet sker ingen initiering och det finns inga data i arkivet.
* Butiken är förifylld med standardvärden (init-egenskaper), men begäran misslyckas (timeout). Det finns bara en initiering med standardvärden.
* Butiken är förifylld.

När fördröjningen är inställd på `true` eller flera millisekunder väntar metoden innan återanropsmetoden anropas. Om en annan initieringshändelse utlöses innan fördröjningen skickas, väntar den tills fördröjningstiden överskrids utan någon initieringshändelse. Detta gör att det går att vänta på att en andra initieringshändelse ska utlösas och anropar callback-funktionen i det mest optimala fallet.

**Parametrar**

* storeName: String. Namnet på det sessionsarkiv som avlyssnaren ska läggas till i.
* callback: Function. Funktionen som ska anropas vid arkivinitiering.
* delay: Boolean eller Number. Den tid i millisekunder som anropet till återanropsfunktionen ska fördröjas. Ett booleskt värde på `true` använder standardfördröjningen `200 ms`. Ett booleskt värde på `false` eller ett negativt tal gör att ingen fördröjning används.

**Returnerar**

Inget returvärde.

#### onStoreRegistered(storeName, callback) {#onstoreregistered-storename-callback}

Registrerar en callback-funktion som anropas när ett sessionsarkiv registreras. Registreringshändelsen inträffar när ett arkiv registreras för [CQ_Analytics.ClientContextMgr](#cq-analytics-clientcontextmgr).

**Parametrar**

* storeName: String. Namnet på det sessionsarkiv som avlyssnaren ska läggas till i.
* callback: Function. Funktionen som ska anropas vid arkivinitiering.

**Returnerar**

Inget returvärde.

## CQ_Analytics.JSONPStore {#cq-analytics-jsonpstore}

Ett icke-beständigt sessionsarkiv som innehåller JSON-data. Data hämtas från en extern JSONP-tjänst. Använd metoden `getInstance` eller `getRegisteredInstance` för att skapa en instans av den här klassen.

Utökar CQ_Analytics.JSONStore.

### Egenskaper {#properties}

Se CQ_Analytics.JSONStore och CQ_Analytics.SessionStore för ärvda egenskaper.

### Metoder {#methods-2}

Se även CQ_Analytics.JSONStore och CQ_Analytics.SessionStore för ärvda metoder.

#### getInstance(storeName, serviceURL, dynamicData, deferLoading, loadingCallback) {#getinstance-storename-serviceurl-dynamicdata-deferloading-loadingcallback}

Skapar ett CQ_Analytics.JSONPStore-objekt.

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler. Om inget storeName anges returnerar metoden null.
* serviceURL: String. URL:en för JSONP-tjänsten
* dynamicData: (valfritt) Object. JSON-data som ska läggas till i butikens initieringsdata innan callback-funktionen anropas.
* deferLoading: (Valfritt) Boolean. Värdet true förhindrar att JSONP-tjänsten anropas när objekt skapas. Värdet false anropar JSONP-tjänsten.
* loadingCallback: (Valfritt) String. Namnet på den funktion som ska anropas för bearbetning av JSONP-objektet som JSONP-tjänsten returnerar. Callback-funktionen måste definiera en enda parameter som är ett CQ_Analytics.JSONPStore-objekt.

**Returnerar**

Det nya CQ_Analytics.JSONPStore-objektet eller null om storeName är null.

#### getServiceURL() {#getserviceurl}

Hämtar URL:en för den JSONP-tjänst som det här objektet använder för att hämta JSON-data.

**Parametrar**

Ingen.

**Returnerar**

En sträng som representerar tjänst-URL:en, eller null om ingen tjänst-URL har konfigurerats.

#### load(serviceURL, dynamicData, callback) {#load-serviceurl-dynamicdata-callback}

Anropar JSONP. JSONP-URL:en är den tjänst-URL som har suffixet ett namn på en återanropsfunktion.

**Parametrar**

* serviceURL: (Valfritt) String. JSONP-tjänsten att ringa. Värdet null gör att den redan konfigurerade tjänst-URL:en används. Ett värde som inte är null anger JSONP-tjänsten som ska användas för det här objektet. (Se setServiceURL.)
* dynamicData: (valfritt) Object. JSON-data som ska läggas till i butikens initieringsdata innan callback-funktionen anropas.
* callback: (Valfritt) String. Namnet på den funktion som ska anropas för bearbetning av JSONP-objektet som JSONP-tjänsten returnerar. Callback-funktionen måste definiera en enda parameter som är ett CQ_Analytics.JSONPStore-objekt.

**Returnerar**

Inget returvärde.

#### registerNewInstance(storeName, serviceURL, dynamicData, callback) {#registernewinstance-storename-serviceurl-dynamicdata-callback}

Skapar ett CQ_Analytics.JSONPStore-objekt och registrerar arkivet med Client Context.

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler. Om inget storeName anges returnerar metoden null.
* serviceURL: (Valfritt) String. URL:en för JSONP-tjänsten.
* dynamicData: (valfritt) Object. JSON-data som ska läggas till i butikens initieringsdata innan callback-funktionen anropas.
* callback: (Valfritt) String. Namnet på den funktion som ska anropas för bearbetning av JSONP-objektet som JSONP-tjänsten returnerar. Callback-funktionen måste definiera en enda parameter som är ett CQ_Analytics.JSONPStore-objekt.

**Returnerar**

Det registrerade CQ_Analytics.JSONPStore-objektet.

#### setServiceURL(serviceURL) {#setserviceurl-serviceurl}

Anger URL:en för JSONP-tjänsten som ska användas för att hämta JSON-data.

**Parametrar**

* serviceURL: String. URL:en för JSONP-tjänsten som tillhandahåller JSON-data

**Returnerar**

Inget returvärde.

## CQ_Analytics.JSONStore {#cq-analytics-jsonstore}

En behållare för ett JSON-objekt. Skapa en instans av den här klassen för att skapa ett icke-beständigt sessionsarkiv som innehåller JSON-data:

`myjsonstore = new CQ_Analytics.JSONStore`

Du kan definiera en uppsättning data som fyller i arkivet vid initieringen.

Utökar CQ_Analytics.SessionStore.

### Egenskaper {#properties-1}

#### STOREKEY {#storekey}

Nyckeln som identifierar butiken. Använd metoden `getInstance` för att hämta det här värdet.

#### STORENAME {#storename}

Butikens namn. Använd metoden `getInstance` för att hämta det här värdet.

### Metoder {#methods-3}

Se även CQ_Analytics.SessionStore för ärvda metoder.

#### clear() {#clear}

Tar bort sessionsarkivdata och tar bort alla initieringsegenskaper.

**Parametrar**

Ingen.

**Returnerar**

Inget returvärde.

#### getInstance(storeName, jsonData) {#getinstance-storename-jsondata}

Skapar ett CQ_Analytics.JSONStore-objekt med ett givet namn och initieras med angivna JSON-data (anropar metoden initJSON).

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler.
* jsonData: Object. Ett objekt som innehåller JSON-data.

**Returnerar**

CQ_Analytics.JSONStore-objektet.

#### getJSON() {#getjson}

Hämtar data för sessionsarkivet i JSON-format.

**Parametrar**

Ingen.

**Returnerar**

Ett objekt som representerar lagringsdata i JSON-format.

#### init() {#init}

Rensar sessionsarkivet och initierar det med initieringsegenskapen. Anger initieringsflaggan som `true` och utlöser sedan händelserna `initialize` och `update`.

**Parametrar**

Ingen.

**Returnerar**

Inga returnerade data.

#### initJSON(jsonData, doNotClear) {#initjson-jsondata-donotclear}

Skapar initieringsegenskaper från data i ett JSON-objekt. Du kan också ta bort alla befintliga initieringsegenskaper.

Egenskapernas namn härleds från datahierarkin i JSON-objektet. Följande exempelkod representerar ett JSON-objekt:

```xml
{
A: "valueA",
B: {
     B1: "valueBB1"
    }
}
```

I det här exemplet skapas följande egenskaper i arkivet:

```xml
A: "valueA"
B/B1: "valueBB1"
```

**Parametrar**

* jsonData: Ett JSON-objekt som innehåller de data som ska lagras.
* doNotClear: Värdet true bevarar de befintliga initieringsegenskaperna och lägger till dem som härletts från JSON-objektet. Värdet false tar bort befintliga initieringsegenskaper innan de som härletts från JSON-objektet läggs till.

**Returnerar**

Inget returvärde.

#### registerNewInstance(storeName, jsonData) {#registernewinstance-storename-jsondata}

Skapar ett CQ_Analytics.JSONStore-objekt med ett givet namn och initieras med angivna JSON-data (anropar metoden initJSON). Det nya objektet registreras automatiskt med Clickstream Cloud Manager.

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler.
* jsonData: Object. Ett objekt som innehåller JSON-data.

**Returnerar**

CQ_Analytics.JSONStore-objektet.

## CQ_Analytics.Observable {#cq-analytics-observable}

Utlöser händelser och tillåter andra objekt att lyssna på dessa händelser och reagera. Klasser som utökar den här klassen kan utlösa händelser som gör att avlyssnare anropas.

### Metoder {#methods-4}

#### addListener(event, fct, scope) {#addlistener-event-fct-scope}

Registrerar en avlyssnare för en händelse. Se även [Skapa en avlyssnare som kan reagera på en sessionsarkivuppdatering](/help/sites-developing/client-context.md#creating-a-listener-to-react-to-a-session-store-update).

**Parametrar**

* event: String. Namnet på händelsen som ska avlyssnas.
* fct: Function. Den funktion som anropas när händelsen inträffar.
* scope: (Valfritt) Objekt. Det omfång som hanterarfunktionen ska köras i. Hanterarfunktionens kontext &quot;this&quot;.

**Returnerar**

Inget returvärde.

#### removeListener(event, fct) {#removelistener-event-fct}

Tar bort den angivna händelsehanteraren för en händelse.

**Parametrar**

* event: String. Namnet på händelsen.
* fct: Function. Händelsehanteraren.

**Returnerar**

Inget returvärde.

## CQ_Analyics.PersistedJSONPStore {#cq-analyics-persistedjsonpstore}

En beständig behållare för ett JSON-objekt som hämtats från en fjärr-JSONP-tjänst.

Utökar CQ_Analytics.PersistedJSONStore.

### Metoder {#methods-5}

Se även CQ_Analytics.PersistedJSONStore för ärvda metoder.

#### getInstance(storeName, serviceURL, dynamicData, deferLoading, loadingCallback) {#getinstance-storename-serviceurl-dynamicdata-deferloading-loadingcallback-1}

Skapar ett CQ_Analytics.PersistedJSONPStore-objekt.

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler. Om inget storeName anges returnerar metoden null.
* serviceURL: String. URL:en för JSONP-tjänsten
* dynamicData: (valfritt) Object. JSON-data som ska läggas till i butikens initieringsdata innan callback-funktionen anropas.
* deferLoading: (Valfritt) Boolean. Värdet true förhindrar att JSONP-tjänsten anropas när objekt skapas. Värdet false anropar JSONP-tjänsten.
* loadingCallback: (Valfritt) String. Namnet på den funktion som ska anropas för bearbetning av JSONP-objektet som JSONP-tjänsten returnerar. Callback-funktionen måste definiera en enda parameter som är ett CQ_Analytics.JSONPStore-objekt.

**Returnerar**

Det nya CQ_Analytics.PersistedJSONPStore-objektet eller null om storeName är null.

#### getServiceURL() {#getserviceurl-1}

Hämtar URL:en för den JSONP-tjänst som det här objektet använder för att hämta JSON-data.

**Parametrar**

Ingen.

**Returnerar**

En sträng som representerar tjänst-URL:en, eller null om ingen tjänst-URL har konfigurerats.

#### load(serviceURL, dynamicData, callback) {#load-serviceurl-dynamicdata-callback-1}

Anropar JSONP. JSONP-URL:en är den tjänst-URL som har suffixet ett namn på en återanropsfunktion.

**Parametrar**

* serviceURL: (Valfritt) String. JSONP-tjänsten att ringa. Värdet null gör att den redan konfigurerade tjänst-URL:en används. Ett värde som inte är null anger JSONP-tjänsten som ska användas för det här objektet. (Se setServiceURL.)
* dynamicData: (valfritt) Object. JSON-data som ska läggas till i butikens initieringsdata innan callback-funktionen anropas.
* callback: (Valfritt) String. Namnet på den funktion som ska anropas för bearbetning av JSONP-objektet som JSONP-tjänsten returnerar. Callback-funktionen måste definiera en enda parameter som är ett CQ_Analytics.JSONPStore-objekt.

**Returnerar**

Inget returvärde.

#### registerNewInstance(storeName, serviceURL, dynamicData, callback) {#registernewinstance-storename-serviceurl-dynamicdata-callback-1}

Skapar ett CQ_Analytics.PersistedJSONPStore-objekt och registrerar arkivet med Client Context.

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler. Om inget storeName anges returnerar metoden null.
* serviceURL: (Valfritt) String. URL:en för JSONP-tjänsten.
* dynamicData: (valfritt) Object. JSON-data som ska läggas till i butikens initieringsdata innan callback-funktionen anropas.
* callback: (Valfritt) String. Namnet på den funktion som ska anropas för bearbetning av JSONP-objektet som JSONP-tjänsten returnerar. Callback-funktionen måste definiera en enda parameter som är ett CQ_Analytics.JSONPStore-objekt.

**Returnerar**

Det registrerade CQ_Analytics.PersistedJSONPStore-objektet.

#### setServiceURL(serviceURL) {#setserviceurl-serviceurl-1}

Anger URL:en för JSONP-tjänsten som ska användas för att hämta JSON-data.

**Parametrar**

* serviceURL: String. URL:en för JSONP-tjänsten som tillhandahåller JSON-data

**Returnerar**

Inget returvärde.

## CQ_Analytics.PersistedJSONStore {#cq-analytics-persistedjsonstore}

En beständig behållare för ett JSON-objekt.

Utökar `CQ_Analytics.PersistedSessionStore`.

### Egenskaper {#properties-2}

#### STOREKEY {#storekey-1}

Nyckeln som identifierar butiken. Använd metoden `getInstance` för att hämta det här värdet.

#### STORENAME {#storename-1}

Butikens namn. Använd metoden `getInstance` för att hämta det här värdet.

### Metoder {#methods-6}

Se även CQ_Analytics.PersistedSessionStore för ärvda metoder.

#### getInstance(storeName, jsonData) {#getinstance-storename-jsondata-1}

Skapar ett CQ_Analytics.PersistedJSONStore-objekt med ett givet namn och initieras med angivna JSON-data (anropar metoden initJSON).

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler.
* jsonData: Object. Ett objekt som innehåller JSON-data.

**Returnerar**

CQ_Analytics.PersistedJSONStore-objektet.

#### getJSON() {#getjson-1}

Hämtar data för sessionsarkivet i JSON-format.

**Parametrar**

Ingen.

**Returnerar**

Ett objekt som representerar lagringsdata i JSON-format.

#### initJSON(jsonData, doNotClear) {#initjson-jsondata-donotclear-1}

Skapar initieringsegenskaper från data i ett JSON-objekt. Du kan också ta bort alla befintliga initieringsegenskaper.

Egenskapernas namn härleds från datahierarkin i JSON-objektet. Följande exempelkod representerar ett JSON-objekt:

```xml
{
A: "valueA",
B: {
     B1: "valueBB1"
    }
}
```

I det här exemplet skapas följande egenskaper i arkivet:

```xml
A: "valueA"
B/B1: "valueBB1"
```

**Parametrar**

* jsonData: Ett JSON-objekt som innehåller de data som ska lagras.
* doNotClear: Värdet true bevarar de befintliga initieringsegenskaperna och lägger till dem som härletts från JSON-objektet. Värdet false tar bort befintliga initieringsegenskaper innan de som härletts från JSON-objektet läggs till.

**Returnerar**

Inget returvärde.

#### registerNewInstance(storeName, jsonData) {#registernewinstance-storename-jsondata-1}

Skapar ett CQ_Analytics.PersistedJSONStore-objekt med ett givet namn och initieras med angivna JSON-data (anropar metoden initJSON). Det nya objektet registreras automatiskt med Client Context Manager.

**Parametrar**

* storeName: String. Namnet som ska användas som STORENAME-egenskap. Värdet för egenskapen STOREKEY anges till storeName med alla versaler.
* jsonData: Object. Ett objekt som innehåller JSON-data.

**Returnerar**

CQ_Analytics.PersistedJSONStore-objektet.

## CQ_Analytics.PersistedSessionStore {#cq-analytics-persistedsessionstore}

En behållare med egenskaper och värden. Data sparas med CQ_Analytics.SessionPersistence. Skapa en instans av den här klassen för att skapa ett beständigt sessionsarkiv:

`mypersistedstore = new CQ_Analytics.PersistedSessionStore`

Utökar CQ_Analytics.SessionStore.

### Egenskaper {#properties-3}

#### STOREKEY {#storekey-2}

Standardvärdet är `key`.

### Metoder {#methods-7}

Se CQ_Analytics.SessionStore för ärvda metoder.

När de ärvda metoderna `clear`, `setProperty`, `setProperties`, `removeProperty` används för att ändra lagringsdata, behålls ändringarna automatiskt, såvida inte de ändrade egenskaperna flaggas som notPersisted.

#### getStoreKey() {#getstorekey}

Hämtar egenskapen `STOREKEY`.

**Parametrar**

Ingen

**Returnerar**

Värdet för egenskapen `STOREKEY`.

#### isPersisted(name) {#ispersisted-name}

Avgör om en dataegenskap är beständig.

**Parametrar**

* name: String. Egenskapens namn.

**Returnerar**

Ett booleskt värde på `true` om egenskapen är beständig och värdet `false` om värdet inte är en beständig egenskap.

#### persist() {#persist}

Innehåller sessionsarkivet. Standardbeständighetsläget använder webbläsaren `localStorage` med `ClientSidePersistence` som namn ( `window.localStorage.set("ClientSidePersistance", store);`)

Om localStorage inte är tillgängligt eller skrivbart sparas arkivet som en egenskap i fönstret.

Utlöser `persist`-händelsen när den har slutförts.

**Parametrar**

Ingen

**Returnerar**

Inget returvärde.

#### reset(deferEvent) {#reset-deferevent}

Tar bort alla dataegenskaper från arkivet och sparar arkivet. `udpate`-händelsen utlöses inte när den har slutförts.

**Parametrar**

* deferEvent: Värdet true förhindrar att `update`-händelsen utlöses. Värdet `false` aktiverar uppdateringshändelsen.

**Returnerar**

Inget returvärde.

#### setNonPersisted(name) {#setnonpersisted-name}

Flaggar en dataegenskap som inte beständig.

**Parametrar**

* name: String. Namnet på den egenskap som inte ska bevaras.

**Returnerar**

Inget returvärde.

## CQ_Analytics.SessionStore {#cq-analytics-sessionstore}

CQ_Analytics.SessionStore representerar ett sessionsarkiv. Skapa en instans av den här klassen för att skapa ett sessionsarkiv:

`mystore = new CQ_Analytics.SessionStore`

Utökar CQ_Analytics.Observable.

### Egenskaper {#properties-4}

#### STORENAME {#storename-2}

Namnet på sessionsarkivet. Använd getName för att hämta egenskapens värde.

### Metoder {#methods-8}

#### addInitProperty(name, value) {#addinitproperty-name-value}

Lägger till en egenskap och ett värde i initieringsdata för sessionsarkivet.

Använd loadInitProperties för att fylla i sessionsarkivdata med initieringsvärden.

**Parametrar**

* name: String. Namnet på den egenskap som ska läggas till.
* value: String. Värdet på egenskapen som ska läggas till.

**Returnerar**

Inget returvärde.

#### clear() {#clear-1}

Tar bort alla dataegenskaper från arkivet.

**Parametrar**

Ingen.

**Returnerar**

Inget returvärde.

#### getData(exclude) {#getdata-excluded}

Returnerar lagringsdata. Namnegenskaperna kan också utelämnas från data. Anropar metoden `init` om egenskapen data för arkivet inte finns.

**Parametrar**

exclude: (Valfritt) En array med egenskapsnamn som ska exkluderas från returnerade data.

**Returnerar**

Ett objekt med egenskaper och deras värden.

#### getInitProperty(name) {#getinitproperty-name}

Hämtar värdet för en data-egenskap.

**Parametrar**

* name: String. Namnet på den dataegenskap som ska hämtas.

**Returnerar**

Värdet för egenskapen data. Returnerar `null` om sessionsarkivet inte innehåller någon egenskap med det angivna namnet.

#### getName() {#getname}

Returnerar namnet på sessionsarkivet.

**Parametrar**

Ingen.

**Returnerar**

Ett String-värde som representerar butiksnamnet.

#### getProperty(name, raw) {#getproperty-name-raw}

Returnerar värdet för en egenskap. Värdet returneras som raw-egenskapen eller det XSS-filtrerade värdet. Anropar metoden `init` om egenskapen data för arkivet inte finns.

**Parametrar**

* name: String. Namnet på den dataegenskap som ska hämtas.
* raw: Boolean. Värdet true gör att raw-egenskapsvärdet returneras. Värdet false gör att det returnerade värdet XSS-filtreras.

**Returnerar**

Värdet för egenskapen data.

#### getPropertyNames(exclude) {#getpropertynames-excluded}

Returnerar namnen på de egenskaper som sessionsarkivet innehåller. Anropar metoden `init` om egenskapen data för arkivet inte finns.

**Parametrar**

exclude: (Valfritt) En array med egenskapsnamn som ska utelämnas från resultatet.

**Returnerar**

En array med strängvärden som representerar sessionsegenskapsnamnen.

#### getSessionStore() {#getsessionstore}

Returnerar det sessionsarkiv som är kopplat till det aktuella objektet.

**Parametrar**

Ingen.

**Returnerar**

this

#### init() {#init-1}

Markerar arkivet som initierat och utlöser händelsen `initialize`.

**Parametrar**

Ingen.

**Returnerar**

Inget returvärde.

#### isInitialized() {#isinitialized}

Anger om sessionslagringen har initierats.

**Parametrar**

Ingen.

**Returnerar**

Värdet `true` om arkivet har initierats och värdet `false` om arkivet inte har initierats.

#### loadInitProperties(obj, setValues) {#loadinitproperties-obj-setvalues}

Lägger till egenskaperna för ett givet objekt i sessionsarkivets initieringsdata. Om du vill kan du även lägga till objektdata i lagringsdata.

**Parametrar**

* obj: Ett objekt som innehåller uppräkningsbara egenskaper.
* setValues: Om värdet är true läggs obj-egenskaperna till i sessionsarkivdata om lagringsdata inte redan innehåller en egenskap med samma namn. Om värdet är false läggs inga data till i sessionsarkivdata.

**Returnerar**

Inget returvärde.

#### removeProperty(name) {#removeproperty-name}

Tar bort en egenskap från sessionsarkivet. Utlöser `update`-händelsen när den har slutförts. Anropar metoden `init` om egenskapen data för arkivet inte finns.

**Parametrar**

* name: String. Namnet på den egenskap som ska tas bort.

**Returnerar**

Inget returvärde.

#### reset() {#reset}

Återställer de ursprungliga värdena för datalagret. Standardimplementeringen tar helt enkelt bort alla data. Utlöser `update`-händelsen när den har slutförts.

**Parametrar**

Ingen.

**Returnerar**

Inget returvärde.

#### setProperties(properties) {#setproperties-properties}

Anger värden för flera egenskaper. Utlöser `update`-händelsen när den har slutförts. Anropar metoden `init` om egenskapen data för arkivet inte finns.

**Parametrar**

* Egenskaper: Objekt. Ett objekt som innehåller uppräkningsbara egenskaper. Varje egenskapsnamn och värde läggs till i arkivet.

**Returnerar**

Inget returvärde.

#### setProperty(name, value) {#setproperty-name-value}

Anger värdet för en egenskap. Utlöser `update`-händelsen när den har slutförts. Anropar metoden `init` om egenskapen data för arkivet inte finns.

**Parametrar**

* name: String. Egenskapens namn.
* value: String. Egenskapsvärde.

**Returnerar**

Inget returvärde.
