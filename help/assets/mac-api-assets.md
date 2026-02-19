---
title: '[!DNL Assets] HTTP API.'
description: Skapa, läsa, uppdatera, ta bort, hantera digitala resurser med HTTP API i  [!DNL Adobe Experience Manager Assets].
contentOwner: AG
role: Developer
feature: Assets HTTP API,Developer Tools
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 7bb4aec8-e6c0-416a-a318-d3120f9688c4
source-git-commit: f0385f595035dfd0dce77fc7eb8b2413eaf5431a
workflow-type: tm+mt
source-wordcount: '1763'
ht-degree: 0%

---

# [!DNL Assets] HTTP API {#assets-http-api}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=en) |
| AEM 6.5 | Den här artikeln |

## Ökning {#overview}

HTTP-API:t [!DNL Assets] tillåter åtgärder för att skapa/läsa-uppdatera-ta bort (CRUD) på digitala resurser, inklusive metadata, återgivningar och kommentarer, samt strukturerat innehåll med [!DNL Experience Manager] innehållsfragment. Den exponeras vid `/api/assets` och implementeras som REST API. Den innehåller [stöd för innehållsfragment](/help/assets/assets-api-content-fragments.md).

Så här kommer du åt API:

1. Öppna API-tjänstdokumentet på `https://[hostname]:[port]/api.json`.
1. Följ tjänstlänkens radavstånd för [!DNL Assets] till `https://[hostname]:[server]/api/assets.json`.

API-svaret är en JSON-fil för vissa MIME-typer och en svarskod för alla MIME-typer. JSON-svaret är valfritt och kanske inte är tillgängligt, till exempel för PDF-filer. Använd svarskoden för ytterligare analyser eller åtgärder.

Efter [!UICONTROL Off Time] är en resurs och dess återgivningar inte tillgängliga via webbgränssnittet [!DNL Assets] och via HTTP-API:t. API:t returnerar 404-felmeddelande om [!UICONTROL On Time] ligger i framtiden eller om [!UICONTROL Off Time] redan finns.

>[!CAUTION]
>
>[HTTP API uppdaterar metadataegenskaperna &#x200B;](#update-asset-metadata) i namnområdet `jcr`. Experience Manager användargränssnitt uppdaterar emellertid metadataegenskaperna i namnområdet `dc`.

## Innehållsfragment {#content-fragments}

Ett [innehållsfragment](/help/assets/content-fragments/content-fragments.md) är en särskild typ av resurs. Den kan användas för att komma åt strukturerade data, t.ex. texter, siffror och datum. Eftersom det finns flera skillnader mellan `standard`-resurser (till exempel bilder eller dokument) gäller vissa ytterligare regler för hantering av innehållsfragment.

Mer information finns i [Stöd för innehållsfragment i Experience Manager Assets HTTP API](/help/assets/assets-api-content-fragments.md).

## Datamodell {#data-model}

HTTP-API:t [!DNL Assets] visar två huvudelement, mappar och resurser (för standardresurser).

Dessutom visas mer detaljerade element för anpassade datamodeller som beskriver strukturerat innehåll i innehållsfragment. Mer information finns i [Datamodeller för innehållsfragment](/help/assets/assets-api-content-fragments.md#content-fragments).

### Mappar {#folders}

Mappar är som kataloger i traditionella filsystem. De är behållare för andra mappar eller kontroller. Mappar har följande komponenter:

**Entiteter**: Enheterna i en mapp är dess underordnade element, som kan vara mappar och resurser.

**Egenskaper**:

* `name` är namnet på mappen. Detta är samma som det sista segmentet i URL-sökvägen utan tillägget.
* `title` är en valfri rubrik för mappen som kan visas i stället för dess namn.

>[!NOTE]
>
>Vissa egenskaper för mapp eller resurs är mappade till ett annat prefix. Prefixet `jcr` för `jcr:title`, `jcr:description` och `jcr:language` ersätts med prefixet `dc`. I den returnerade JSON innehåller därför `dc:title` och `dc:description` värdena för `jcr:title` respektive `jcr:description`.

**Länkar**-mappar visar tre länkar:

* `self`: Länka till sig själv.
* `parent`: Länk till den överordnade mappen.
* `thumbnail`: (Valfritt) länk till en mappminiatyrbild.

### Assets {#assets}

I Experience Manager innehåller en resurs följande element:

* Resursens egenskaper och metadata.
* Flera återgivningar, till exempel den ursprungliga återgivningen (som är den ursprungliga överförda resursen), en miniatyrbild och olika andra återgivningar. Ytterligare återgivningar kan vara bilder av olika storlek, olika videokodningar eller extraherade sidor från PDF- eller [!DNL Adobe InDesign]-filer.
* Valfria kommentarer.

Mer information om element i innehållsfragment finns i [Stöd för innehållsfragment i Experience Manager Assets HTTP API](/help/assets/assets-api-content-fragments.md#content-fragments).

I [!DNL Experience Manager] har en mapp följande komponenter:

* Enheter: De underordnade resurserna är dess återgivningar.
* Egenskaper.
* Länkar.

HTTP-API:t [!DNL Assets] innehåller följande funktioner:

* [Hämta en mapplista](#retrieve-a-folder-listing).
* [Skapa en mapp](#create-a-folder).
* [Skapa en resurs](#create-an-asset).
* [Uppdatera resursens binärfil](#update-asset-binary).
* [Uppdatera resursmetadata](#update-asset-metadata).
* [Skapa en resursåtergivning](#create-an-asset-rendition).
* [Uppdatera en resursåtergivning](#update-an-asset-rendition).
* [Skapa en objektkommentar](#create-an-asset-comment).
* [Kopiera en mapp eller resurs](#copy-a-folder-or-asset).
* [Flytta en mapp eller resurs](#move-a-folder-or-asset).
* [Ta bort en mapp, resurs eller återgivning](#delete-a-folder-asset-or-rendition).

>[!NOTE]
>
>För att underlätta läsbarheten utelämnar följande exempel den fullständiga cURL-notationen. Faktum är att noteringen korrelerar med [Resty](https://github.com/micha/resty) som är en skriptwrapper för `cURL`.

**Förutsättningar**

* Åtkomst till `https://[aem_server]:[port]/system/console/configMgr`.
* Navigera till **[!UICONTROL Adobe Granite CSRF Filter]**.
* Kontrollera att egenskapen **[!UICONTROL Filter Methods]** innehåller: `POST`, `PUT`, `DELETE`.

## Hämta en mapplista {#retrieve-a-folder-listing}

Hämtar en siren-representation av en befintlig mapp och av dess underordnade enheter (undermappar eller resurser).

**Begäran**: `GET /api/assets/myFolder.json`

**Svarskoder**: Svarskoderna är:

* 200 - OK - framgång.
* 404 - HITTADES INTE - mappen finns inte eller är inte tillgänglig.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

**Svar**: Klassen för entiteten som returneras är en resurs eller en mapp. Egenskaperna för enheter som ingår är en deluppsättning av alla egenskaper för varje enhet. Om du vill få en fullständig representation av entiteten måste klienterna hämta innehållet i den URL som länken pekar på med `rel` av `self`.

## Skapa en mapp {#create-a-folder}

Skapar en ny `sling`: `OrderedFolder` vid den angivna sökvägen. Om `*` anges i stället för ett nodnamn använder serverleten parameternamnet som nodnamn. Data accepteras antingen som en Siren-representation av den nya mappen eller som en uppsättning namn/värde-par, kodade som `application/www-form-urlencoded` eller `multipart`/ `form`- `data`, vilket är användbart när du skapar en mapp direkt från ett HTML-formulär. Dessutom kan mappens egenskaper anges som URL-frågeparametrar.

Ett API-anrop misslyckas med en `500`-svarskod om den överordnade noden för den angivna sökvägen inte finns. Ett anrop returnerar svarskoden `409` om mappen redan finns.

**Parametrar**: `name` är mappnamnet.

**Begäran**

* `POST /api/assets/myFolder -H"Content-Type: application/json" -d '{"class":"assetFolder","properties":{"jcr:title":"My Folder"}}'`
* `POST /api/assets/* -F"name=myfolder" -F"jcr:title=My Folder"`

**Svarskoder**: Svarskoderna är:

* 201 - SKAPAD - när den skapades.
* 409 - CONFLICT - om mappen redan finns.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Skapa en resurs {#create-an-asset}

Placera den angivna filen på den angivna sökvägen för att skapa en resurs i DAM-databasen. Om `*` anges i stället för ett nodnamn använder serverleten parameternamnet eller filnamnet som nodnamn.

**Parametrar**: Parametrarna är `name` för resursnamnet och `file` för filreferensen.

**Begäran**

* `POST /api/assets/myFolder/myAsset.png -H"Content-Type: image/png" --data-binary "@myPicture.png"`
* `POST /api/assets/myFolder/* -F"name=myAsset.png" -F"file=@myPicture.png"`

**Svarskoder**: Svarskoderna är:

* 201 - SKAPAD - om resursen har skapats.
* 409 - CONFLICT - om tillgången redan finns.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Uppdatera en resurbinär {#update-asset-binary}

Uppdaterar en resurs binärfil (återgivning med namnet original). En uppdatering utlöser standardarbetsflödet för resurshantering som körs, om det är konfigurerat.

**Begäran**: `PUT /api/assets/myfolder/myAsset.png -H"Content-Type: image/png" --data-binary @myPicture.png`

**Svarskoder**: Svarskoderna är:

* 200 - OK - Om resursen har uppdaterats utan fel.
* 404 - HITTADES INTE - Om det inte gick att hitta eller få åtkomst till resursen på angiven URI.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Uppdatera metadata för resurs {#update-asset-metadata}

Uppdaterar metadataegenskaperna för resursen. Om du uppdaterar någon egenskap i namnutrymmet `dc:` uppdaterar API:t samma egenskap i namnutrymmet `jcr`. API:t synkroniserar inte egenskaperna under de två namnutrymmena.

**Begäran**: `PUT /api/assets/myfolder/myAsset.png -H"Content-Type: application/json" -d '{"class":"asset", "properties":{"jcr:title":"My Asset"}}'`

**Svarskoder**: Svarskoderna är:

* 200 - OK - Om resursen har uppdaterats utan fel.
* 404 - HITTADES INTE - Om det inte gick att hitta eller få åtkomst till resursen på angiven URI.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

### Synkronisera metadatauppdatering mellan namnområdet `dc` och `jcr` {#sync-metadata-between-namespaces}

API-metoden uppdaterar metadataegenskaperna i namnområdet `jcr`. Uppdateringarna som görs med användargränssnittet ändrar metadataegenskaperna i namnområdet `dc`. Om du vill synkronisera metadatavärdena mellan `dc` och `jcr`-namnområdet kan du skapa ett arbetsflöde och konfigurera Experience Manager så att arbetsflödet körs när resursen redigeras. Använd ett ECMA-skript för att synkronisera de metadataegenskaper som krävs. Följande exempelskript synkroniserar titelsträngen mellan `dc:title` och `jcr:title`.

```javascript
var workflowData = workItem.getWorkflowData();
if (workflowData.getPayloadType() == "JCR_PATH")
{
 var path = workflowData.getPayload().toString();
 var node = workflowSession.getSession().getItem(path);
 var metadataNode = node.getNode("jcr:content/metadata");
 var jcrcontentNode = node.getNode("jcr:content");
if (jcrcontentNode.hasProperty("jcr:title"))
{
 var jcrTitle = jcrcontentNode.getProperty("jcr:title");
 metadataNode.setProperty("dc:title", jcrTitle.toString());
 metadataNode.save();
}
}
```

## Skapa en resursåtergivning {#create-an-asset-rendition}

Skapa en resursåtergivning för en resurs. Om parameternamnet för begäran inte anges används filnamnet som återgivningsnamn.

**Parametrar**: Parametrarna är `name` för återgivningens namn och `file` som en filreferens.

**Begäran**

* `POST /api/assets/myfolder/myasset.png/renditions/web-rendition -H"Content-Type: image/png" --data-binary "@myRendition.png"`
* `POST /api/assets/myfolder/myasset.png/renditions/* -F"name=web-rendition" -F"file=@myRendition.png"`

**Svarskoder**: Svarskoderna är:

* 201 - SKAPAD - om återgivningen har skapats.
* 404 - HITTADES INTE - Om det inte gick att hitta eller få åtkomst till resursen på angiven URI.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Uppdatera en resursåtergivning {#update-an-asset-rendition}

Uppdateringar ersätter en resursåtergivning med nya binära data.

**Begäran**: `PUT /api/assets/myfolder/myasset.png/renditions/myRendition.png -H"Content-Type: image/png" --data-binary @myRendition.png`

**Svarskoder**: Svarskoderna är:

* 200 - OK - Om återgivningen har uppdaterats.
* 404 - HITTADES INTE - Om det inte gick att hitta eller få åtkomst till resursen på angiven URI.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Lägga till en kommentar till en resurs {#create-an-asset-comment}

Skapar en ny resurskommentar.

**Parametrar**: Parametrarna är `message` för kommentarens meddelandetext och `annotationData` för anteckningsdata i JSON-format.

**Begäran**: `POST /api/assets/myfolder/myasset.png/comments/* -F"message=Hello World." -F"annotationData={}"`

**Svarskoder**: Svarskoderna är:

* 201 - SKAPAD - om kommentaren har skapats.
* 404 - HITTADES INTE - Om det inte gick att hitta eller få åtkomst till resursen på angiven URI.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Kopiera en mapp eller en resurs {#copy-a-folder-or-asset}

Kopierar en mapp eller en resurs som är tillgänglig på den angivna sökvägen till ett nytt mål.

**Begäranrubriker**: Parametrarna är:

* `X-Destination` - en ny mål-URI inom API-lösningsscopet som resursen ska kopieras till.
* `X-Depth` - antingen `infinity` eller `0`. Om du använder `0` kopieras bara resursen och dess egenskaper och inte dess underordnade.
* `X-Overwrite` - Använd `F` för att förhindra att en resurs skrivs över på det befintliga målet.

**Begäran**: `COPY /api/assets/myFolder -H"X-Destination: /api/assets/myFolder-copy"`

**Svarskoder**: Svarskoderna är:

* 201 - SKAPAD - om mappen/resursen har kopierats till ett icke-befintligt mål.
* 204 - INGET INNEHÅLL - om mappen/resursen har kopierats till ett befintligt mål.
* 412 - PRECONDITION MISSLYCKADES - om ett begärandehuvud saknas.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Flytta en mapp eller en resurs {#move-a-folder-or-asset}

Flyttar en mapp eller resurs vid den angivna sökvägen till ett nytt mål.

**Begäranrubriker**: Parametrarna är:

* `X-Destination` - en ny mål-URI inom API-lösningsscopet som resursen ska kopieras till.
* `X-Depth` - antingen `infinity` eller `0`. Om du använder `0` kopieras bara resursen och dess egenskaper och inte dess underordnade.
* `X-Overwrite` - Använd antingen `T` för att framtvinga borttagning av befintliga resurser eller `F` för att förhindra att en befintlig resurs skrivs över.

**Begäran**: `MOVE /api/assets/myFolder -H"X-Destination: /api/assets/myFolder-moved"`

Använd inte `/content/dam` i URL:en. Ett exempelkommando för att flytta resurser och skriva över befintliga är:

```shell
curl -u admin:admin -X MOVE https://[aem_server]:[port]/api/assets/source/file.png -H "X-Destination: https://[aem_server]:[port]/api/assets/destination/file.png" -H "X-Overwrite: T"
```

**Svarskoder**: Svarskoderna är:

* 201 - SKAPAD - om mappen/resursen har kopierats till ett icke-befintligt mål.
* 204 - INGET INNEHÅLL - om mappen/resursen har kopierats till ett befintligt mål.
* 412 - PRECONDITION MISSLYCKADES - om ett begärandehuvud saknas.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Ta bort en mapp, en resurs eller en återgivning {#delete-a-folder-asset-or-rendition}

Tar bort en resurs (-tree) vid den angivna sökvägen.

**Begäran**

* `DELETE /api/assets/myFolder`
* `DELETE /api/assets/myFolder/myAsset.png`
* `DELETE /api/assets/myFolder/myAsset.png/renditions/original`

**Svarskoder**: Svarskoderna är:

* 200 - OK - Om mappen har tagits bort.
* 412 - PRECONDITION MISSLYCKADES - om rotsamlingen inte kan hittas eller nås.
* 500 - INTERNT SERVERFEL - Om något annat går fel.

## Tips och begränsningar {#tips-best-practices-limitations}

* [HTTP API uppdaterar metadataegenskaperna &#x200B;](#update-asset-metadata) i namnområdet `jcr`. Experience Manager användargränssnitt uppdaterar emellertid metadataegenskaperna i namnområdet `dc`.

* Assets HTTP API returnerar inte alla metadata. Namnutrymmena är hårdkodade och endast dessa namnutrymmen returneras. Fullständiga metadata finns i resurssökvägen `/jcr_content/metadata.json`.
