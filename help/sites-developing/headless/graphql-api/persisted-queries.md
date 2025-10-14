---
title: Beständiga GraphQL-frågor
description: Lär dig hur du bibehåller GraphQL-frågor i Adobe Experience Manager för att optimera prestandan. Beständiga frågor kan begäras av klientprogram med HTTP GET-metoden och svaret kan cachas i Dispatcher- och CDN-lagren, vilket i slutänden förbättrar klientprogrammens prestanda.
solution: Experience Manager, Experience Manager Sites
feature: Content Fragments,GraphQL API
role: Developer
exl-id: 686d5510-8cdb-49eb-9ed0-f360be9bdc6d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1401'
ht-degree: 0%

---

# Beständiga GraphQL-frågor {#persisted-queries-caching}

Beständiga frågor är GraphQL-frågor som skapas och lagras på Adobe Experience Manager-servern (AEM). De kan begäras med en GET-begäran från klientprogram. Svaret på en GET-förfrågan kan cachas i Dispatcher- och Content Delivery Network-lager (CDN), vilket i slutänden förbättrar prestanda för det begärande klientprogrammet. Detta skiljer sig från vanliga GraphQL-frågor, som körs med POST-begäranden där svaret inte enkelt kan cachas.

<!--
>[!NOTE]
>
>Persisted Queries are recommended. See [GraphQL Query Best Practices (Dispatcher)](/help/headless/graphql-api/content-fragments.md#graphql-query-best-practices) for details, and the related Dispatcher configuration.
-->

[GraphiQL IDE](/help/sites-developing/headless/graphql-api/graphiql-ide.md) är tillgänglig i AEM så att du kan utveckla, testa och behålla dina GraphQL-frågor innan du [överför dem till produktionsmiljön](#transfer-persisted-query-production). Om du behöver anpassa (till exempel när du [anpassar cachen](/help/sites-developing/headless/graphql-api/graphiql-ide.md#caching-persisted-queries)) kan du använda API:t, se exemplet på cURL i [Så här gör du för att behålla en GraphQL-fråga](#how-to-persist-query).

## Beständiga frågor och slutpunkter {#persisted-queries-and-endpoints}

Beständiga frågor måste alltid använda den slutpunkt som är relaterad till [lämplig platskonfiguration](/help/sites-developing/headless/graphql-api/graphql-endpoint.md), så att de kan använda antingen eller båda:

* Den globala konfigurationen och slutpunkten
Frågan har åtkomst till alla modeller för innehållsfragment.
* Specifika platskonfigurationer och slutpunkter
För att skapa en beständig fråga för en specifik platskonfiguration krävs en motsvarande platskonfigurationsspecifik slutpunkt (för att ge åtkomst till relaterade modeller för innehållsfragment).
Om du till exempel vill skapa en beständig fråga specifikt för WKND-platskonfigurationen, måste en motsvarande WKND-specifik platskonfiguration och en WKND-specifik slutpunkt skapas i förväg.

>[!NOTE]
>
>Mer information finns i [Aktivera funktionen för innehållsfragment i konfigurationsläsaren](/help/assets/content-fragments/content-fragments-configuration-browser.md#enable-content-fragment-functionality-in-configuration-browser).
>
>**GraphQL beständiga frågor** måste aktiveras för rätt platskonfiguration.

Om det till exempel finns en viss fråga med namnet `my-query` som använder modellen `my-model` från platskonfigurationen `my-conf`:

* Du kan skapa en fråga med den `my-conf` specifika slutpunkten och sedan sparas frågan så här:
  `/conf/my-conf/settings/graphql/persistentQueries/my-query`
* Du kan skapa samma fråga med `global`-slutpunkten, men sedan sparas frågan så här:
  `/conf/global/settings/graphql/persistentQueries/my-query`

>[!NOTE]
>
>Det här är två olika frågor - sparade under olika sökvägar.
>
>De råkar bara använda samma modell, men via olika slutpunkter.

## Så här behåller du en GraphQL-fråga {#how-to-persist-query}

Vi rekommenderar att du till att börja med behåller frågor i en AEM-redigeringsmiljö och sedan [överför frågan](#transfer-persisted-query-production) till din publiceringsmiljö i AEM där den kan användas av program.

Det finns olika metoder för beständiga frågor, bland annat:

* GraphiQL IDE - se [Spara beständiga frågor](/help/sites-developing/headless/graphql-api/graphiql-ide.md#saving-persisted-queries) (föredragen metod)
* cURL - se följande exempel
* Andra verktyg, inklusive [Postman](https://www.postman.com/)

GraphiQL IDE är den **föredragna** metoden för beständiga frågor. Så här behåller du en given fråga med kommandoradsverktyget **cURL**:

1. Förbered frågan genom att PUTing den till den nya slutpunkts-URL:en `/graphql/persist.json/<config>/<persisted-label>`.

   Skapa till exempel en beständig fråga:

   ```shell
   $ curl -X PUT \
       -H 'authorization: Basic YWRtaW46YWRtaW4=' \
       -H "Content-Type: application/json" \
       "http://localhost:4502/graphql/persist.json/wknd/plain-article-query" \
       -d \
   '{
     articleList {
       items{
           _path
           author
           main {
               json
           }
       }
     }
   }'
   ```

1. Kontrollera svaret nu.

   Kontrollera till exempel om åtgärden lyckades:

   ```json
   {
     "action": "create",
     "configurationName": "wknd",
     "name": "plain-article-query",
     "shortPath": "/wknd/plain-article-query",
     "path": "/conf/wknd/settings/graphql/persistentQueries/plain-article-query"
   }
   ```

1. Du kan sedan begära den beständiga frågan genom att GETing anger URL:en `/graphql/execute.json/<shortPath>`.

   Använd till exempel den beständiga frågan:

   ```shell
   $ curl -X GET \
       http://localhost:4502/graphql/execute.json/wknd/plain-article-query
   ```

1. Uppdatera en beständig fråga genom POSTing till en redan befintlig frågesökväg.

   Använd till exempel den beständiga frågan:

   ```shell
   $ curl -X POST \
       -H 'authorization: Basic YWRtaW46YWRtaW4=' \
       -H "Content-Type: application/json" \
       "http://localhost:4502/graphql/persist.json/wknd/plain-article-query" \
       -d \
   '{
     articleList {
       items{
           _path
           author
           main {
               json
           }
         referencearticle {
           _path
         }
       }
     }
   }'
   ```

1. Skapa en omsluten vanlig fråga.

   Till exempel:

   ```shell
   $ curl -X PUT \
       -H 'authorization: Basic YWRtaW46YWRtaW4=' \
       -H "Content-Type: application/json" \
       "http://localhost:4502/graphql/persist.json/wknd/plain-article-query-wrapped" \
       -d \
   '{ "query": "{articleList { items { _path author main { json } referencearticle { _path } } } }"}'
   ```

1. Skapa en omsluten oformaterad fråga med cachekontroll.

   Till exempel:

   ```shell
   $ curl -X PUT \
       -H 'authorization: Basic YWRtaW46YWRtaW4=' \
       -H "Content-Type: application/json" \
       "http://localhost:4502/graphql/persist.json/wknd/plain-article-query-max-age" \
       -d \
   '{ "query": "{articleList { items { _path author main { json } referencearticle { _path } } } }", "cache-control": { "max-age": 300 }}'
   ```

1. Skapa en beständig fråga med parametrar:

   Till exempel:

   ```shell
   $ curl -X PUT \
       -H 'authorization: Basic YWRtaW46YWRtaW4=' \
       -H "Content-Type: application/json" \
       "http://localhost:4502/graphql/persist.json/wknd/plain-article-query-parameters" \
       -d \
   'query GetAsGraphqlModelTestByPath($apath: String!, $withReference: Boolean = true) {
     articleByPath(_path: $apath) {
       item {
         _path
           author
           main {
           plaintext
           }
           referencearticle @include(if: $withReference) {
           _path
           }
         }
       }
     }'
   ```


## Så här kör du en fråga som är sparad {#execute-persisted-query}

För att köra en Persistent-fråga gör ett klientprogram en GET-begäran med följande syntax:

```
GET <AEM_HOST>/graphql/execute.json/<PERSISTENT_PATH>
```

Där `PERSISTENT_PATH` är en förkortad sökväg till den plats där den beständiga frågan sparas.

1. `wknd` är till exempel konfigurationsnamnet och `plain-article-query` är namnet på den beständiga frågan. Så här kör du frågan:

   ```shell
   $ curl -X GET \
       https://localhost:4502/graphql/execute.json/wknd/plain-article-query
   ```

1. Kör en fråga med parametrar.

   >[!NOTE]
   >
   > Frågevariabler och -värden måste vara korrekt [kodade](#encoding-query-url) när en Persisted-fråga körs.

   Till exempel:

   ```bash
   $ curl -X GET \
       "https://localhost:4502/graphql/execute.json/wknd/plain-article-query-parameters%3Bapath%3D%2Fcontent%2Fdam%2Fwknd%2Fen%2Fmagazine%2Falaska-adventure%2Falaskan-adventures%3BwithReference%3Dfalse
   ```

   Mer information finns i Använda [frågevariabler](#query-variables).

## Använda frågevariabler {#query-variables}

Frågevariabler kan användas med beständiga frågor. Frågevariablerna läggs till i begäran med prefixet semikolon (`;`) med variabelnamnet och variabelvärdet. Flera variabler avgränsas med semikolon.

Mönstret ser ut så här:

```
<AEM_HOST>/graphql/execute.json/<PERSISTENT_QUERY_PATH>;variable1=value1;variable2=value2
```

Följande fråga innehåller till exempel variabeln `activity` för att filtrera en lista baserat på ett aktivitetsvärde:

```graphql
query getAdventuresByActivity($activity: String!) {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: $activity
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
      }
    }
  }
```

Den här frågan kan sparas under sökvägen `wknd/adventures-by-activity`. Så här anropar du den beständiga frågan där `activity=Camping` begäran skulle se ut:

```
<AEM_HOST>/graphql/execute.json/wknd/adventures-by-activity%3Bactivity%3DCamping
```

Observera att `%3B` är UTF-8-kodning för `;` och `%3D` är kodning för `=`. Frågevariablerna och eventuella specialtecken måste vara [kodade korrekt](#encoding-query-url) för att den beständiga frågan ska kunna köras.

## Cachelagra beständiga frågor {#caching-persisted-queries}

Beständiga frågor rekommenderas eftersom de kan cachelagras på [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=sv-SE) - och CDN-lagren (Content Delivery Network), vilket i slutänden förbättrar prestandan för det begärande klientprogrammet.

Som standard gör AEM cachen ogiltig baserat på en TTL-definition (Time To Live). Dessa TTL:er kan definieras med följande parametrar. Dessa parametrar kan nås på olika sätt, med variationer i namnen beroende på vilken mekanism som används:

| Cache-typ | [HTTP-huvud](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)  | cURL  | OSGi-konfiguration  |
|--- |--- |--- |--- |
| Webbläsare | `max-age` | `cache-control : max-age` | `cacheControlMaxAge` |
| CDN | `s-maxage` | `surrogate-control : max-age` | `surrogateControlMaxAge` |
| CDN | `stale-while-revalidate` | `surrogate-control : stale-while-revalidate ` | `surrogateControlStaleWhileRevalidate` |
| CDN | `stale-if-error` | `surrogate-control : stale-if-error` | `surrogateControlStaleIfError` |

{style="table-layout:auto"}

### Författarinstanser {#author-instances}

För författarinstanser är standardvärdena:

* `max-age` : 60
* `s-maxage` : 60
* `stale-while-revalidate` : 86400
* `stale-if-error` : 86400

De var:

* kan inte skrivas över med en OSGi-konfiguration
* kan skrivas över av en begäran som definierar inställningar för HTTP-huvudet med cURL; den bör innehålla lämpliga inställningar för `cache-control` och/eller `surrogate-control`; se till exempel [Hantera cache på den beständiga frågenivån](#cache-persisted-query-level)

<!-- CQDOC-20186 -->
<!-- following entry is only when the GraphiQL IDE is ready; add cross-reference too -->
<!--
* can be overwritten if you specify values in the **Headers** dialog of the [GraphiQL IDE](#http-cache-headers-graphiql-ide)
-->

### Publicera förekomster {#publish-instances}

Standardvärdena för publiceringsinstanser är:

* `max-age` : 60
* `s-maxage` : 7200
* `stale-while-revalidate` : 86400
* `stale-if-error` : 86400

Dessa kan skrivas över:

<!-- CQDOC-20186 -->
<!-- following entry is only when the GraphiQL IDE is ready -->
<!--
* [from the GraphQL IDE](#http-cache-headers-graphiql-ide)
-->

* [&#x200B; på nivån för beständig fråga](#cache-persisted-query-level). Detta innebär att frågan skickas till AEM med cURL i kommandoradsgränssnittet och att den beständiga frågan publiceras.

* [med en OSGi-konfiguration](#cache-osgi-configration)

<!-- CQDOC-20186 -->
<!-- keep for future use; check link -->
<!--
### Managing HTTP Cache Headers in the GraphiQL IDE {#http-cache-headers-graphiql-ide}

The GraphiQL IDE - see [Saving Persisted Queries](/help/sites-developing/headless/graphql-api/graphiql-ide.md#managing-cache)
-->

### Hantera cache på nivån för beständig fråga {#cache-persisted-query-level}

Detta innebär att skicka frågan till AEM med cURL i kommandoradsgränssnittet.

Ett exempel på PUT-metoden (skapa):

```bash
curl -u admin:admin -X PUT \
--url "http://localhost:4502/graphql/persist.json/wknd/plain-article-query-max-age" \
--header "Content-Type: application/json" \
--data '{ "query": "{articleList { items { _path author } } }", "cache-control": { "max-age": 300 }, "surrogate-control": {"max-age":600, "stale-while-revalidate":1000, "stale-if-error":1000} }'
```

Ett exempel på POST-metoden (update):

```bash
curl -u admin:admin -X POST \
--url "http://localhost:4502/graphql/persist.json/wknd/plain-article-query-max-age" \
--header "Content-Type: application/json" \
--data '{ "query": "{articleList { items { _path author } } }", "cache-control": { "max-age": 300 }, "surrogate-control": {"max-age":600, "stale-while-revalidate":1000, "stale-if-error":1000} }'
```

`cache-control` kan anges när den skapas (PUT) eller senare (till exempel via en POST-begäran). Cachekontrollen är valfri när du skapar den beständiga frågan, eftersom AEM kan ange standardvärdet. Se [Så här behåller du en GraphQL-fråga](#how-to-persist-query) om du vill se ett exempel på hur en fråga bevaras med cURL.

### Hantera cache med en OSGi-konfiguration {#cache-osgi-configration}

Om du vill hantera cacheminnet globalt kan du [konfigurera OSGi-inställningarna](/help/sites-deploying/configuring-osgi.md) för **konfigurationen för den beständiga frågetjänsten**. Annars använder den här OSGi-konfigurationen [standardvärdena för publiceringsinstanser](#publish-instances).

>[!NOTE]
>
>OSGi-konfigurationen passar bara för publiceringsinstanser. Konfigurationen finns på författarinstanser, men ignoreras.

## Kodning av fråge-URL för användning av ett program {#encoding-query-url}

Om du vill använda ett program måste alla specialtecken som används för att skapa frågevariabler (t.ex. semikolon (`;`), likhetstecken (`=`), snedstreck `/`) konverteras till motsvarande UTF-8-kodning.

Till exempel:

```bash
curl -X GET \ "https://localhost:4502/graphql/execute.json/wknd/adventure-by-path%3BadventurePath%3D%2Fcontent%2Fdam%2Fwknd%2Fen%2Fadventures%2Fbali-surf-camp%2Fbali-surf-camp"
```

URL:en kan delas upp i följande delar:

| URL-del | Beskrivning |
|----------| -------------|
| `/graphql/execute.json` | Beständig frågeslutpunkt |
| `/wknd/adventure-by-path` | Sökväg för beständig fråga |
| `%3B` | Kodning av `;` |
| `adventurePath` | Frågevariabel |
| `%3D` | Kodning av `=` |
| `%2F` | Kodning av `/` |
| `%2Fcontent%2Fdam...` | Kodad sökväg till innehållsfragmentet |

I vanlig text ser URI:n för begäran ut så här:

```plaintext
/graphql/execute.json/wknd/adventure-by-path;adventurePath=/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp
```

Om du vill använda en beständig fråga i en klientapp bör AEM Headless-klienten SDK användas för [JavaScript](https://github.com/adobe/aem-headless-client-js), [Java](https://github.com/adobe/aem-headless-client-java) eller [NodeJS](https://github.com/adobe/aem-headless-client-nodejs). Den Headless Client SDK kodar automatiskt alla frågevariabler som behövs i begäran.

## Överför en beständig fråga till produktionsmiljön  {#transfer-persisted-query-production}

Beständiga frågor ska alltid skapas i en AEM Author-tjänst och sedan publiceras (replikeras) i en AEM Publish-tjänst. Vanligtvis skapas och testas beständiga frågor i miljöer som lokala miljöer och utvecklingsmiljöer. Det är sedan nödvändigt att befordra beständiga frågor till miljöer på högre nivå och i slutänden göra dem tillgängliga i en produktionsmiljö i AEM Publish för att klientprogram ska kunna använda dem.

### Paketera beständiga frågor

Beständiga frågor kan byggas in i [AEM-paket](/help/sites-administering/package-manager.md). AEM Packages kan sedan laddas ned och installeras i olika miljöer. AEM-paket kan också replikeras från en AEM Author-miljö till AEM Publish-miljöer.

Skapa ett paket:

1. Navigera till **Verktyg** > **Distribution** > **Paket**.
1. Skapa ett paket genom att trycka på **Skapa paket**. Då öppnas en dialogruta där du kan definiera paketet.
1. I dialogrutan Paketdefinition, under **Allmänt**, anger du ett **Namn** som &quot;wknd-persistent-queries&quot;.
1. Ange ett versionsnummer som &quot;1.0&quot;.
1. Lägg till ett nytt **Filter** under **Filter**. Använd Sökväg till Finder för att välja mappen `persistentQueries` under konfigurationen. För konfigurationen `wknd` blir till exempel den fullständiga sökvägen `/conf/wknd/settings/graphql/persistentQueries`.
1. Välj **Spara** om du vill spara den nya paketdefinitionen och stänga dialogrutan.
1. Välj knappen **Skapa** i den nya paketdefinitionen.

När paketet har byggts kan du:

* **Hämta** paketet och överföra det igen till en annan miljö.
* **Replikera** paketet genom att trycka på **Mer** > **Replikera**. Paketet kommer att replikeras till den anslutna AEM Publish-miljön.

<!--
1. Using replication/distribution tool:
   1. Go to the Distribution tool.
   1. Select tree activation for the configuration (for example, `/conf/wknd/settings/graphql/persistentQueries`).

* Using a workflow (via workflow launcher configuration):
  1. Define a workflow launcher rule for executing a workflow model that would replicate the configuration on different events (for example, create, modify, among others).
-->
