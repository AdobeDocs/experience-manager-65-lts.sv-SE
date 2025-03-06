---
title: Så här uppdaterar du innehåll via AEM Assets API:er
description: I den här delen av AEM Headless Developer Journey kan du lära dig hur du använder REST API för att komma åt och uppdatera innehållet i dina innehållsfragment.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin, Developer
exl-id: 322f08c7-f13a-473f-8c59-1050b2e6c2f5
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---

# Så här uppdaterar du innehåll via AEM Assets API:er {#update-your-content}

I den här delen av [AEM Headless Developer Journey ](overview.md) får du lära dig hur du använder REST API för att komma åt och uppdatera innehållet i dina innehållsfragment.

## Story hittills {#story-so-far}

I det tidigare dokumentet om AEM resa [Så här kommer du åt ditt innehåll via AEM Delivery API:er](access-your-content.md)  och du bör nu:

* Lär dig mer om GraphQL.
* Förstå hur AEM GraphQL API fungerar.
* Förstå några praktiska exempelfrågor.

Den här artikeln bygger på dessa grundläggande funktioner så att du förstår hur du uppdaterar det befintliga headless-innehållet i AEM via REST API.

## Syfte {#objective}

* **Målgrupp**: Avancerat
* **Mål**: Lär dig hur du använder REST API för att komma åt och uppdatera innehållet i dina innehållsfragment:
   * Introducera AEM Assets HTTP API.
   * Presentera och diskutera stöd för innehållsfragment i API:t.
   * Visa information om API:t.

<!--
  * Look at sample code to see how things work in practice.
-->

## Varför behöver du Assets HTTP API för innehållsfragment {#why-http-api}

I det tidigare skedet av Headless Journey lärde du dig att använda AEM GraphQL API för att hämta ditt innehåll med hjälp av frågor.

Varför behövs en annan API?

Med Assets HTTP API kan du **läsa** ditt innehåll, men du kan även använda det för att **skapa**, **uppdatera** och **ta bort** -innehåll - åtgärder som inte är möjliga med GraphQL API.

Assets REST API är tillgängligt för alla färdiga installationer av en nyligen använd Adobe Experience Manager-version.

## ASSETS HTTP API {#assets-http-api}

Assets HTTP API omfattar:

* ASSETS REST API
* inklusive stöd för innehållsfragment

Den aktuella implementeringen av Assets HTTP API baseras på arkitekturstilen **REST** och gör att du kan komma åt innehåll (som lagras i AEM) via **CRUD**-åtgärder (Skapa, Läs, Uppdatera, Ta bort).

Med den här åtgärden kan du använda Adobe Experience Manager som ett headless CMS (Content Management System) genom att tillhandahålla innehållstjänster till ett JavaScript front end-program. Eller något annat program som kan köra HTTP-begäranden och hantera JSON-svar. Exempelvis kräver Single Page Applications (SPA), ramverksbaserade eller anpassade, innehåll som tillhandahålls via ett API, ofta i JSON-format.

<!--
>[!NOTE]
>
>It is not possible to customize JSON output from the Assets REST API. 

The Assets REST API:

* follows the HATEOAS principle
* implements the SIREN format

## Key Concepts {#key-concepts}

The Assets REST API offers REST-style access to assets stored within an AEM instance. 

It uses the `/api/assets` endpoint and requires the path of the asset to access it (without the leading `/content/dam`). 

* This means that to access the asset at:
  * `/content/dam/path/to/asset`
* You need to request:
  * `/api/assets/path/to/asset` 

For example, to access `/content/dam/wknd/en/adventures/cycling-tuscany`, request `/api/assets/wknd/en/adventures/cycling-tuscany.json` 

>[!NOTE]
>Access over:
>
>* `/api/assets` **does not** need the use of the `.model` selector.
>* `/content/path/to/page` **does** require the use of the `.model` selector.

The HTTP method determines the operation to be executed:

* **GET** - to retrieve a JSON representation of an asset or a folder
* **POST** - to create new assets or folders
* **PUT** - to update the properties of an asset or folder
* **DELETE** - to delete an asset or folder

>[!NOTE]
>
>The request body and/or URL parameters can be used to configure some of these operations; for example, define that a folder or an asset should be created by a **POST** request.

The exact format of supported requests is defined in the API Reference documentation. 

### Transactional Behavior {#transactional-behavior}

All requests are atomic.

This means that subsequent (`write`) requests cannot be combined into a single transaction that could succeed or fail as a single entity.

### Security {#security}

If the Assets REST API is used within an environment without specific authentication requirements, AEM's CORS filter needs to be configured correctly.

>[!NOTE]
>
>For further information see:
>
>* CORS/AEM explained
>* Video - Developing for CORS with AEM

In environments with specific authentication requirements, OAuth is recommended.

## Available Features {#available-features}

Content Fragments are a specific type of Asset, see Working with Content Fragments.

For further information about features available through the API see:

* The Assets REST API (Additional Resources) 
* Entity Types, where the features specific to each supported type (as relevant to Content Fragments) are explained 

### Paging {#paging}

The Assets REST API supports paging (for GET requests) via the URL parameters:

* `offset` - the number of the first (child) entity to retrieve
* `limit` - the maximum number of entities returned

The response will contain paging information as part of the `properties` section of the SIREN output. This `srn:paging` property contains the total number of (child) entities ( `total`), the offset and the limit ( `offset`, `limit`) as specified in the request.

>[!NOTE]
>
>Paging is typically applied on container entities (that is, folders or assets with renditions), as it relates to the children of the requested entity.

#### Example: Paging {#example-paging}

`GET /api/assets.json?offset=2&limit=3`

```json
...
"properties": {
    ...
    "srn:paging": {
        "total": 7,
        "offset": 2,
        "limit": 3
    }
    ...
}
...
```

## Entity Types {#entity-types}

### Folders {#folders}

Folders act as containers for assets and other folders. They reflect the structure of the AEM content repository.

The Assets REST API exposes access to the properties of a folder; for example, its name, title, and so on. Assets are exposed as child entities of folders, and sub-folders.

>[!NOTE]
>
>Depending on the asset type of the child assets and folders the list of child entities may already contain the full set of properties that defines the respective child entity. Alternatively, only a reduced set of properties may be exposed for an entity in this list of child entities.

### Assets {#assets}

If an asset is requested, the response will return its metadata; such as title, name and other information as defined by the respective asset schema.

The binary data of an asset is exposed as a SIREN link of type `content`.

Assets can have multiple renditions. These are typically exposed as child entities, one exception being a thumbnail rendition, which is exposed as a link of type `thumbnail` ( `rel="thumbnail"`).
-->

## Assets HTTP API och innehållsfragment {#assets-http-api-content-fragments}

Innehållsfragment används för rubrikfri leverans och ett innehållsfragment är en särskild typ av resurs. De används för att komma åt strukturerade data, t.ex. texter, siffror och datum.

<!--
As there are several differences to *standard* assets (such as images or audio), some additional rules apply to handling them.

### Representation {#representation}

Content fragments:

* Do not expose any binary data.
* Are completely contained in the JSON output (within the `properties` property).

* Are also considered atomic, that is, the elements and variations are exposed as part of the fragment's properties vs. as links or child entities. This allows for efficient access to the payload of a fragment.

### Content Models and Content Fragments {#content-models-and-content-fragments}

Currently the models that define the structure of a content fragment are not exposed through an HTTP API. Therefore the *consumer* needs to know about the model of a fragment (at least a minimum) - although most information can be inferred from the payload; as data types, and so on. are part of the definition.

To create a content fragment, the (internal repository) path of the model has to be provided.

### Associated Content {#associated-content}

Associated content is currently not exposed.
-->

## Använda Assets REST API {#using-aem-assets-rest-api}

### Åtkomst {#access}

Assets REST API använder slutpunkten `/api/assets` och kräver att resursens sökväg har åtkomst till den (utan inledande `/content/dam`).

* Det innebär att du kan få tillgång till resursen på
   * `/content/dam/path/to/asset`
* Du måste begära:
   * `/api/assets/path/to/asset`

Om du till exempel vill komma åt `/content/dam/wknd/en/adventures/cycling-tuscany` begär du `/api/assets/wknd/en/adventures/cycling-tuscany.json`

>[!NOTE]
>Åtkomst över:
>
>* `/api/assets` **behöver inte** använda väljaren `.model`.
>* `/content/path/to/page` **does** kräver att väljaren `.model` används.

### Åtgärd {#operation}

HTTP-metoden avgör vilken åtgärd som ska utföras:

* **GET** - för att hämta en JSON-representation av en resurs eller en mapp
* **POST** - för att skapa nya resurser eller mappar
* **PUT** - för att uppdatera egenskaperna för en resurs eller mapp
* **DELETE** - om du vill ta bort en resurs eller mapp

>[!NOTE]
>
>Parametrarna för begärandeinnehåll och/eller URL kan användas för att konfigurera vissa av dessa åtgärder. Definiera till exempel att en mapp eller en resurs ska skapas av en **POST**-begäran.

Det exakta formatet för begäranden som stöds definieras i API-referensdokumentationen.

Användningen kan variera beroende på om du använder en AEM-författare eller publiceringsmiljö, tillsammans med ditt specifika användningsexempel.

* Vi rekommenderar starkt att skapandet är bundet till en författarinstans (och att det för närvarande inte finns något sätt att replikera ett fragment för publicering med detta API).
* Leverans är möjlig från båda, eftersom AEM endast skickar begärt innehåll i JSON-format.

   * Lagring och leverans från en instans av AEM-författare bör räcka bakom brandväggen, för mediabiblioteksprogram.

   * För direktsänd webbleverans rekommenderas en publiceringsinstans från AEM.

>[!CAUTION]
>
>Dispatcher-konfigurationen på AEM-instanser kan blockera åtkomst till `/api`.

>[!NOTE]
>
>Mer information finns i API-referensen. Särskilt [Adobe Experience Manager Assets API - innehållsfragment](https://www.adobe.io/experience-manager/reference-materials/6-5/assets-api-content-fragments/index.html).

### Läsning/leverans {#read-delivery}

Användning sker via:

`GET /{cfParentPath}/{cfName}.json`

Till exempel:

`http://<host>/api/assets/wknd/en/adventures/cycling-tuscany.json`

Svaret är serialiserat JSON med innehållet strukturerat som i innehållsfragmentet. Referenser levereras som referens-URL:er.

Det finns två typer av läsåtgärder:

* När du läser ett visst innehållsfragment efter sökväg, returneras JSON-representationen av innehållsfragmentet.
* Läser en mapp med innehållsfragment efter sökväg: Detta returnerar JSON-representationerna för alla innehållsfragment i mappen.

### Skapa {#create}

Användning sker via:

`POST /{cfParentPath}/{cfName}`

Brödtexten måste innehålla en JSON-representation av det innehållsfragment som ska skapas, inklusive allt ursprungligt innehåll som ska anges för elementen i innehållsfragmentet. Det är obligatoriskt att ange egenskapen `cq:model` och den måste peka på en giltig innehållsfragmentmodell. Om du inte gör det kommer ett fel att uppstå. Du måste också lägga till rubriken `Content-Type` som är inställd på `application/json`.

### Uppdatera {#update}

Användning sker via

`PUT /{cfParentPath}/{cfName}`

Brödtexten måste innehålla en JSON-representation av vad som ska uppdateras för det angivna innehållsfragmentet.

Detta kan helt enkelt vara titeln eller beskrivningen av ett innehållsfragment, ett enskilt element eller alla elementvärden och/eller metadata.

### Ta bort {#delete}

Användning sker via:

`DELETE /{cfParentPath}/{cfName}`

Mer information om hur du använder AEM Assets REST API finns i:

* Adobe Experience Manager Assets HTTP API (ytterligare resurser)
* Stöd för innehållsfragment i AEM Assets HTTP API (ytterligare resurser)

## What&#39;s Next {#whats-next}

Nu när du är klar med den här delen av AEM Headless Developer Journey bör du:

* Förstå grunderna i AEM Assets HTTP API.
* Förstå hur innehållsfragment stöds i detta API.

<!--
* Have experience with sample code and know how the API works in practice.
-->

<!-- The "How to put it all together" page is not going to be published until the first public release of the Headless SDK. Temporarily commenting out the reference below. -->

<!--You should continue your AEM headless journey by next reviewing the document [How to Put It All Together - Your App and Your Content in AEM Headless](put-it-all-together.md) where you learn how to take your AEM Headless project and prepare it for going live.-->

Du bör fortsätta din resa utan AEM genom att nästa gång granska dokumentet [How to Go Live with Your Headless Application](go-live.md) där du faktiskt gör ditt AEM Headless-projekt tillgängligt!

## Ytterligare resurser {#additional-resources}

* [ASSETS HTTP API](/help/assets/mac-api-assets.md)
* [Innehållsfragment REST API](/help/assets/assets-api-content-fragments.md)
   * [API-referens](/help/assets/assets-api-content-fragments.md#api-reference)
* [Adobe Experience Manager Assets API - innehållsfragment](https://www.adobe.io/experience-manager/reference-materials/6-5/assets-api-content-fragments/index.html)
* [Arbeta med innehållsfragment](/help/assets/content-fragments/content-fragments.md)
* [AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [CORS/AEM förklarar](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/cors-security-article-understand.html)
* [Video - Utveckla för CORS med AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/cors-security-technical-video-develop.html)
* En [introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html)
* [Självstudiekurser för Headless i AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html)
