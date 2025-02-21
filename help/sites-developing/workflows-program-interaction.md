---
title: Interagera med arbetsflöden programmatiskt
description: Lär dig interagera med arbetsflöden programmatiskt i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1857'
ht-degree: 0%

---

# Interagera med arbetsflöden programmatiskt{#interacting-with-workflows-programmatically}

När du [anpassar och utökar dina arbetsflöden](/help/sites-developing/workflows-customizing-extending.md) får du tillgång till arbetsflödesobjekt:

* [Använda Java API för arbetsflöde](#using-the-workflow-java-api)
* [Hämta arbetsflödesobjekt i ECMA-skript](#obtaining-workflow-objects-in-ecma-scripts)
* [Använda REST API för arbetsflöde](#using-the-workflow-rest-api)

## Använda Java API för arbetsflöde {#using-the-workflow-java-api}

Arbetsflödets Java API består av paketet [`com.adobe.granite.workflow`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/package-summary.html) och flera underpaket. Den viktigaste medlemmen i API är klassen `com.adobe.granite.workflow.WorkflowSession`. Klassen `WorkflowSession` ger åtkomst till arbetsflödesobjekt för både designtid och körning:

* arbetsflödesmodeller
* arbetsposter
* arbetsflödesinstanser
* arbetsflödesdata
* inkorgsobjekt

Klassen innehåller också flera metoder för att hantera arbetsflödets livscykler.

Följande tabell innehåller länkar till referensdokumentationen för flera viktiga Java-objekt som kan användas när du interagerar programmatiskt med arbetsflöden. Exemplen som följer visar hur du hämtar och använder klassobjekten i koden.

| Funktioner | Objekt |
|---|---|
| Åtkomst till ett arbetsflöde | [`WorkflowSession`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/WorkflowSession.html) |
| Köra och fråga en arbetsflödesinstans | [`Workflow`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/Workflow.html)</br>[`WorkItem`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkItem.html)</br>[`WorkflowData`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowData.html) |
| Hantera en arbetsflödesmodell | [`WorkflowModel`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/model/WorkflowModel.html)</br>[`WorkflowNode`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/model/WorkflowNode.html)</br>[`WorkflowTransition`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/model/WorkflowTransition.html) |
| Information för en nod som finns i arbetsflödet (eller inte) | [`WorkflowStatus`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/status/WorkflowStatus.html) |

## Hämta arbetsflödesobjekt i ECMA-skript {#obtaining-workflow-objects-in-ecma-scripts}

Så som beskrivs i [Hitta skriptet](/help/sites-developing/the-basics.md#locating-the-script), tillhandahåller AEM (via Apache Sling) en ECMA-skriptmotor som kör ECMA-skript på serversidan. Klassen [`org.apache.sling.scripting.core.ScriptHelper`](https://sling.apache.org/apidocs/sling5/org/apache/sling/scripting/core/ScriptHelper.html) är omedelbart tillgänglig för dina skript som variabeln `sling`.

Klassen `ScriptHelper` ger åtkomst till `SlingHttpServletRequest` som du kan använda för att till slut få fram objektet `WorkflowSession`, till exempel:

```
var wfsession = sling.getRequest().getResource().getResourceResolver().adaptTo(Packages.com.adobe.granite.workflow.WorkflowSession);
```

## Använda REST API för arbetsflöde {#using-the-workflow-rest-api}

Arbetsflödeskonsolen använder REST API i stor utsträckning, så den här sidan beskriver REST API för arbetsflöden.

>[!NOTE]
>
>Med kommandoradsverktyget curl kan du använda arbetsflödets REST API för att komma åt arbetsflödesobjekt och hantera instanslivscykler. Exemplen på hela den här sidan visar hur REST API används via verktyget för rullande kommandorad.

Följande åtgärder stöds med REST API:

* starta eller stoppa en arbetsflödestjänst
* skapa, uppdatera eller ta bort arbetsflödesmodeller
* [starta, pausa, återuppta eller avsluta arbetsflödesinstanser](/help/sites-administering/workflows.md#workflow-status-and-actions)
* slutföra eller delegera arbetsobjekt

>[!NOTE]
>
>Genom att använda Firebug, ett Firefox-tillägg för webbutveckling, är det möjligt att följa HTTP-trafiken när konsolen används. Du kan till exempel kontrollera parametrarna och värdena som skickas till AEM-servern med en `POST`-begäran.

På den här sidan antas att AEM körs på localhost på port `4502` och att installationskontexten är `/` (rot). Om det inte är fallet med din installation måste de URI:er som HTTP-begäran gäller anpassas därefter.

Den återgivning som stöds för `GET`-begäranden är JSON-återgivningen. URL:erna för `GET` ska ha tillägget `.json`, till exempel:

`http://localhost:4502/etc/workflow.json`

### Hantera arbetsflödesinstanser {#managing-workflow-instances}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/etc/workflow/instances`

<table>
 <tbody>
  <tr>
   <td>HTTP-begärandemetod</td>
   <td>Åtgärder</td>
  </tr>
  <tr>
   <td><code>GET</code></td>
   <td>Visar tillgängliga arbetsflödesinstanser.</td>
  </tr>
  <tr>
   <td><code>POST</code></td>
   <td><p>Skapar en ny arbetsflödesinstans. Parametrarna är: <br /> - <code>model</code>: ID (URI) för respektive arbetsflödesmodell <br /> - <code>payloadType</code>: som innehåller typen av nyttolast (till exempel <code>JCR_PATH</code> eller URL).<br /> Nyttolasten skickas som parameter <code>payload</code>. Ett <code>201</code> (<code>CREATED</code>)-svar skickas tillbaka med ett platshuvud som innehåller URL:en för den nya arbetsflödesinstansresursen.</p> </td>
  </tr>
 </tbody>
</table>

#### Hantera en arbetsflödesinstans utifrån dess tillstånd {#managing-a-workflow-instance-by-its-state}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/etc/workflow/instances.{state}`

| HTTP-begärandemetod | Åtgärder |
|---|---|
| `GET` | Visar tillgängliga arbetsflödesinstanser och deras lägen ( `RUNNING`, `SUSPENDED`, `ABORTED` eller `COMPLETED`) |

#### Hantera en arbetsflödesinstans med dess ID {#managing-a-workflow-instance-by-its-id}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/etc/workflow/instances/{id}`

<table>
 <tbody>
  <tr>
   <td>HTTP-begärandemetod</td>
   <td>Åtgärder</td>
  </tr>
  <tr>
   <td><code>GET</code></td>
   <td>Hämtar instansdata (definition och metadata) inklusive länken till respektive arbetsflödesmodell.</td>
  </tr>
  <tr>
   <td><code>POST</code></td>
   <td>Ändrar instansens tillstånd. Det nya läget skickas som parametern <code>state</code> och måste ha något av följande värden: <code>RUNNING</code>, <code>SUSPENDED</code> eller <code>ABORTED</code>.<br /> Om det inte går att nå det nya läget (t.ex. om du gör uppehåll i en avslutad instans) skickas ett <code>409</code> (<code>CONFLICT</code>)-svar tillbaka till klienten.</td>
  </tr>
 </tbody>
</table>

### Hantera arbetsflödesmodeller {#managing-workflow-models}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/etc/workflow/models`

<table>
 <tbody>
  <tr>
   <td>HTTP-begärandemetod</td>
   <td>Åtgärder</td>
  </tr>
  <tr>
   <td><code>GET</code></td>
   <td>Visar tillgängliga arbetsflödesmodeller.</td>
  </tr>
  <tr>
   <td><code>POST</code></td>
   <td>Skapar en ny arbetsflödesmodell. Om parametern <code>title</code> skickas skapas en ny modell med den angivna titeln. När en JSON-modelldefinition bifogas som parameter <code>model</code> skapas en ny arbetsflödesmodell enligt den angivna definitionen.<br /> Ett <code>201</code>-svar (<code>CREATED</code>) skickas tillbaka med ett platshuvud som innehåller URL:en för den nya arbetsflödesmodellresursen.<br /> Samma sak händer när en modelldefinition bifogas som en filparameter med namnet <code>modelfile</code>.<br /> I båda fallen för parametrarna <code>model</code> och <code>modelfile</code> krävs ytterligare en parameter med namnet <code>type</code> för att definiera serialiseringsformatet. Nya serialiseringsformat kan integreras med OSGI API. En vanlig JSON-serialisering levereras med arbetsflödesmotorn. Dess typ är JSON. Nedan finns ett exempel på formatet.</td>
  </tr>
 </tbody>
</table>

Exempel: i webbläsaren genererar en begäran till `http://localhost:4502/etc/workflow/models.json` ett json-svar som liknar följande:

```
[
    {"uri":"/var/workflow/models/activationmodel"}
    ,{"uri":"/var/workflow/models/dam/adddamsize"}
    ,{"uri":"/var/workflow/models/cloudconfigs/dtm-reactor/library-download"}
    ,{"uri":"/var/workflow/models/ac-newsletter-workflow-simple"}
    ,{"uri":"/var/workflow/models/dam/dam-create-language-copy"}
    ,{"uri":"/var/workflow/models/dam/dam-create-and-translate-language-copy"}
    ,{"uri":"/var/workflow/models/dam-indesign-proxy"}
    ,{"uri":"/var/workflow/models/dam-xmp-writeback"}
    ,{"uri":"/var/workflow/models/dam-parse-word-documents"}
    ,{"uri":"/var/workflow/models/dam/process_subasset"}
    ,{"uri":"/var/workflow/models/dam/dam_set_last_modified"}
    ,{"uri":"/var/workflow/models/dam/dam-autotag-assets"}
    ,{"uri":"/var/workflow/models/dam/update_asset"}
    ,{"uri":"/var/workflow/models/dam/update_asset_offloading"}
    ,{"uri":"/var/workflow/models/dam/dam-update-language-copy"}
    ,{"uri":"/var/workflow/models/dam/update_from_lightbox"}
    ,{"uri":"/var/workflow/models/cloudservices/DTM_bundle_download"}
    ,{"uri":"/var/workflow/models/dam/dam_download_asset"}
    ,{"uri":"/var/workflow/models/dam/dynamic-media-encode-video"}
    ,{"uri":"/var/workflow/models/dam/dynamic-media-video-thumbnail-replacement"}
    ,{"uri":"/var/workflow/models/dam/dynamic-media-video-user-uploaded-thumbnail"}
    ,{"uri":"/var/workflow/models/newsletter_bounce_check"}
    ,{"uri":"/var/workflow/models/projects/photo_shoot_submission"}
    ,{"uri":"/var/workflow/models/projects/product_photo_shoot"}
    ,{"uri":"/var/workflow/models/projects/approval_workflow"}
    ,{"uri":"/var/workflow/models/prototype-01"}
    ,{"uri":"/var/workflow/models/publish_example"}
    ,{"uri":"/var/workflow/models/publish_to_campaign"}
    ,{"uri":"/var/workflow/models/screens/publish_to_author_bin"}
    ,{"uri":"/var/workflow/models/s7dam/request_to_publish_to_youtube"}
    ,{"uri":"/var/workflow/models/projects/request_copy"}
    ,{"uri":"/var/workflow/models/projects/request_email"}
    ,{"uri":"/var/workflow/models/projects/request_landing_page"}
    ,{"uri":"/var/workflow/models/projects/request_launch"}
    ,{"uri":"/var/workflow/models/request_for_activation"}
    ,{"uri":"/var/workflow/models/request_for_deactivation"}
    ,{"uri":"/var/workflow/models/request_for_deletion"}
    ,{"uri":"/var/workflow/models/request_for_deletion_without_deactivation"}
    ,{"uri":"/var/workflow/models/request_to_complete_move_operation"}
    ,{"uri":"/var/workflow/models/reverse_replication"}
    ,{"uri":"/var/workflow/models/salesforce-com-export"}
    ,{"uri":"/var/workflow/models/scene7"}
    ,{"uri":"/var/workflow/models/scheduled_activation"}
    ,{"uri":"/var/workflow/models/scheduled_deactivation"}
    ,{"uri":"/var/workflow/models/screens/screens-update-asset"}
    ,{"uri":"/var/workflow/models/translation"}
    ,{"uri":"/var/workflow/models/s7dam/request_to_remove_from_youtube"}
    ,{"uri":"/var/workflow/models/wcm-translation/create_language_copy"}
    ,{"uri":"/var/workflow/models/wcm-translation/prepare_translation_project"}
    ,{"uri":"/var/workflow/models/wcm-translation/translate-i18n-dictionary"}
    ,{"uri":"/var/workflow/models/wcm-translation/sync_translation_job"}
    ,{"uri":"/var/workflow/models/wcm-translation/translate-language-copy"}
    ,{"uri":"/var/workflow/models/wcm-translation/update_language_copy"}
]
```

#### Hantera en specifik arbetsflödesmodell {#managing-a-specific-workflow-model}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502*{uri}*`

Där `*{uri}*` är sökvägen till modellnoden i databasen.

<table>
 <tbody>
  <tr>
   <td>HTTP-begärandemetod</td>
   <td>Åtgärder</td>
  </tr>
  <tr>
   <td><code>GET</code></td>
   <td>Hämtar modellens <code>HEAD</code>-version (definition och metadata).</td>
  </tr>
  <tr>
   <td><code>PUT</code></td>
   <td>Uppdaterar modellversionen <code>HEAD</code> (skapar en ny version).<br /> Den fullständiga modelldefinitionen för den nya versionen av modellen måste läggas till som en parameter med namnet <code>model</code>. Dessutom krävs en <code>type</code>-parameter som när nya modeller skapas och värdet <code>JSON</code> måste anges.<br /> </td>
  </tr>
  <tr>
   <td><code>POST</code></td>
   <td>Samma beteende som hos PUT. Behövs eftersom AEM-widgetar inte stöder <code>PUT</code>-åtgärder.</td>
  </tr>
  <tr>
   <td><code>DELETE</code></td>
   <td>Tar bort modellen. För att lösa brandväggs-/proxyproblem accepteras även en <code>POST</code> som innehåller en <code>X-HTTP-Method-Override</code> rubrikpost med värdet <code>DELETE</code> som <code>DELETE</code>-begäran.</td>
  </tr>
 </tbody>
</table>

Exempel: i webbläsaren returnerar en begäran till `http://localhost:4502/var/workflow/models/publish_example.json` ett `json`-svar som liknar följande kod:

```shell
{
  "id":"/var/workflow/models/publish_example",
  "title":"Publish Example",
  "version":"1.0",
  "description":"This example shows a simple review and publish process.",
  "metaData":
  {
    "multiResourceSupport":"true",
    "tags":"wcm,publish"
  },
  "nodes":
  [{
    "id":"node0",
    "type":"START",
    "title":"Start",
    "description":"The start node of the workflow.",
    "metaData":
    {
    }
  },
  {
    "id":"node1",
    "type":"PARTICIPANT",
    "title":"Validate Content",
    "description":"Validate the modified content.",
    "metaData":
    {
      "PARTICIPANT":"admin"
    }
  },
  {
    "id":"node2",
    "type":"PROCESS",
    "title":"Publish Content",
    "description":"Publish the modified content.",
    "metaData":
    {
      "PROCESS_AUTO_ADVANCE":"true",
      "PROCESS":"com.day.cq.wcm.workflow.process.ActivatePageProcess"
    }
  },
  {
    "id":"node3",
    "type":"END",
    "title":"End",
    "description":"The end node of the workflow.",
    "metaData":
    {
    }
  }],
  "transitions":
  [{
    "from":"node0",
    "to":"node1",
    "metaData":
    {
    }
  },
  {
    "from":"node1",
    "to":"node2",
    "metaData":
    {
    }
  },
  {
    "from":"node2",
    "to":"node3",
    "metaData":
    {
    }
  }
]}
```

#### Hantera en arbetsflödesmodell med dess version {#managing-a-workflow-model-by-its-version}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/etc/workflow/models/{id}.{version}`

| HTTP-begärandemetod | Åtgärder |
|---|---|
| `GET` | Hämtar data för modellen i den angivna versionen (om den finns). |

### Hantera (användare) inkorgar {#managing-user-inboxes}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/bin/workflow/inbox`

<table>
 <tbody>
  <tr>
   <td>HTTP-begärandemetod</td>
   <td>Åtgärder</td>
  </tr>
  <tr>
   <td><code>GET</code></td>
   <td>Visar de arbetsobjekt som finns i användarens inkorg och som identifieras av rubrikerna för HTTP-autentisering.</td>
  </tr>
  <tr>
   <td><code>POST</code></td>
   <td>Slutför arbetsobjektet vars URI skickas som parametern <code>item</code> och flyttar enligt arbetsflödesinstansen till nästa noder, som definieras av parametern <code>route</code> eller <code>backroute</code> om ett steg går tillbaka.<br /> Om parametern <code>delegatee</code> skickas delegeras arbetsposten som identifieras av parametern <code>item</code> till den angivna deltagaren.</td>
  </tr>
 </tbody>
</table>

#### Hantera en (Användare) inkorg med WorkItem-ID {#managing-a-user-inbox-by-the-workitem-id}

Följande metoder för HTTP-begäran gäller:

`http://localhost:4502/bin/workflow/inbox/{id}`

| HTTP-begärandemetod | Åtgärder |
|---|---|
| `GET` | Hämtar data (definition och metadata) för inkorgen `WorkItem` som identifieras av dess ID. |

## Exempel {#examples}

### Hämta en lista över alla arbetsflöden som körs med deras ID:n {#how-to-get-a-list-of-all-running-workflows-with-their-ids}

Om du vill visa en lista över alla arbetsflöden som körs kan du göra följande med GET:

`http://localhost:4502/etc/workflow/instances.RUNNING.json`

#### Hämta en lista över alla arbetsflöden som körs med deras ID:n - REST med URL {#how-to-get-a-list-of-all-running-workflows-with-their-ids-rest-using-curl}

Exempel med vändning:

```shell
curl -u admin:admin http://localhost:4502/etc/workflow/instances.RUNNING.json
```

`uri` som visas i resultaten kan användas som instans `id` i andra kommandon, till exempel:

```shell
[
    {"uri":"/etc/workflow/instances/server0/2017-03-08/request_for_activation_1"}
]
```

>[!NOTE]
>
>Det här `curl`-kommandot kan användas med alla [arbetsflödesstatus](/help/sites-administering/workflows.md#workflow-status-and-actions) i stället för `RUNNING`.

### Ändra arbetsflödets titel {#how-to-change-the-workflow-title}

Om du vill ändra **arbetsflödets titel** som visas på fliken **Instanser** i arbetsflödeskonsolen skickar du ett `POST`-kommando:

* till: `http://localhost:4502/etc/workflow/instances/{id}`

* med följande parametrar:

   * `action`: dess värde måste vara: `UPDATE`
   * `workflowTitle`: arbetsflödets titel

#### Ändra arbetsflödets titel - REST med vändning {#how-to-change-the-workflow-title-rest-using-curl}

Exempel med vändning:

```shell
curl -u admin:admin -d "action=UPDATE&workflowTitle=myWorkflowTitle" http://localhost:4502/etc/workflow/instances/{id}

# for example
curl -u admin:admin -d "action=UPDATE&workflowTitle=myWorkflowTitle" http://localhost:4502/etc/workflow/instances/server0/2017-03-08/request_for_activation_1
```

### Lista alla arbetsflödesmodeller {#how-to-list-all-workflow-models}

Om du vill visa en lista över alla tillgängliga arbetsflödesmodeller gör du en GET för att:

`http://localhost:4502/etc/workflow/models.json`

#### Så här listar du alla arbetsflödesmodeller - REST med kurva {#how-to-list-all-workflow-models-rest-using-curl}

Exempel med vändning:

```shell
curl -u admin:admin http://localhost:4502/etc/workflow/models.json
```

>[!NOTE]
>
>Se även [Hantera arbetsflödesmodeller](#managing-workflow-models).

### Hämta ett WorkflowSession-objekt {#obtaining-a-workflowsession-object}

Klassen `com.adobe.granite.workflow.WorkflowSession` kan anpassas från ett `javax.jcr.Session`-objekt eller ett `org.apache.sling.api.resource.ResourceResolver`-objekt.

#### Hämta ett WorkflowSession-objekt - Java {#obtaining-a-workflowsession-object-java}

I ett JSP-skript (eller Java-kod för en serletklass) använder du objektet för HTTP-begäran för att hämta ett `SlingHttpServletRequest`-objekt, som ger åtkomst till ett `ResourceResolver`-objekt. Anpassa objektet `ResourceResolver` till `WorkflowSession`.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%><%
%><%@page session="false"
    import="com.adobe.granite.workflow.WorkflowSession,
  org.apache.sling.api.SlingHttpServletRequest"%><%

SlingHttpServletRequest slingReq = (SlingHttpServletRequest)request;
WorkflowSession wfSession = slingReq.getResourceResolver().adaptTo(WorkflowSession.class);
%>
```

#### Hämta ett WorkflowSession-objekt - ECMA-skript {#obtaining-a-workflowsession-object-ecma-script}

Använd variabeln `sling` för att hämta det `SlingHttpServletRequest`-objekt som du använder för att hämta ett `ResourceResolver`-objekt. Anpassa objektet `ResourceResolver` till objektet `WorkflowSession`.

```
var wfsession = sling.getRequest().getResource().getResourceResolver().adaptTo(Packages.com.adobe.granite.workflow.WorkflowSession);
```

### Skapa, läsa eller ta bort arbetsflödesmodeller {#creating-reading-or-deleting-workflow-models}

I följande exempel visas hur du får åtkomst till arbetsflödesmodeller:

* Koden för Java- och ECMA-skript använder metoden `WorkflowSession.createNewModel`.
* Kommandot curl kommer åt modellen direkt med dess URL.

De exempel som används:

1. Skapa en modell (med ID `/var/workflow/models/mymodel/jcr:content/model`).
1. Ta bort modellen.

>[!NOTE]
>
>Om du tar bort modellen anges egenskapen `deleted` för modellens underordnade `metaData`-nod som `true`.
>
>Modellnoden tas inte bort.

När du skapar en modell:

* Arbetsflödesmodellredigeraren kräver att modeller använder en specifik nodstruktur under `/var/workflow/models`. Modellens överordnade nod måste vara av typen `cq:Page` med en `jcr:content`-nod med följande egenskapsvärden:

   * `sling:resourceType`: `cq/workflow/components/pages/model`
   * `cq:template`: `/libs/cq/workflow/templates/model`

  När du skapar en modell måste du först skapa den här `cq:Page`-noden och använda dess `jcr:content`-nod som överordnad modellnod.

* Argumentet `id` som vissa metoder kräver för att identifiera modellen är den absoluta sökvägen för modellnoden i databasen:

  `/var/workflow/models/<*model_name>*/jcr:content/model`

  >[!NOTE]
  >
  >Se [Så här listar du alla arbetsflödesmodeller](#how-to-list-all-workflow-models).

#### Skapa, läsa eller ta bort arbetsflödesmodeller - Java {#creating-reading-or-deleting-workflow-models-java}

```java
<%@include file="/libs/foundation/global.jsp"%><%
%><%@page session="false" import="com.adobe.granite.workflow.WorkflowSession,
                 com.adobe.granite.workflow.model.WorkflowModel,
             org.apache.sling.api.SlingHttpServletRequest"%><%

SlingHttpServletRequest slingReq = (SlingHttpServletRequest)request;
WorkflowSession wfSession = slingReq.getResourceResolver().adaptTo(WorkflowSession.class);
/* Create the parent page */
String modelRepo = new String("/var/workflow/models");
String modelTemplate = new String ("/libs/cq/workflow/templates/model");
String modelName = new String("mymodel");
Page modelParent = pageManager.create(modelRepo, modelName, modelTemplate, "My workflow model");

/* create the model */
String modelId = new String(modelParent.getPath()+"/jcr:content/model")
WorkflowModel model = wfSession.createNewModel("Made using Java",modelId);

/* delete the model */
wfSession.deleteModel(modelId);
%>
```

#### Skapa, läsa eller ta bort arbetsflödesmodeller - ECMA-skript {#creating-reading-or-deleting-workflow-models-ecma-script}

```
var resolver = sling.getRequest().getResource().getResourceResolver();
var wfSession = resolver.adaptTo(Packages.com.adobe.granite.workflow.WorkflowSession);
var pageManager = resolver.adaptTo(Packages.com.day.cq.wcm.api.PageManager);

//create the parent page node
var workflowPage = pageManager.create("/var/workflow/models", "mymodel", "/libs/cq/workflow/templates/model", "Created via ECMA Script");
var modelId = workflowPage.getPath()+ "/jcr:content/model";
//create the model
var model = wfSession.createNewModel("My Model", modelId);
//delete the model
var model = wfSession.deleteModel(modelId);
```

#### Ta bort en arbetsflödesmodell - REST med vändning {#deleting-a-workflow-model-rest-using-curl}

```shell
# deleting the model by its id
curl -u admin:admin -X DELETE http://localhost:4502/etc/workflow/models/{id}
```

>[!NOTE]
>
>På grund av den detaljnivå som krävs anses kurl inte vara praktiskt när du skapar och/eller läser en modell.

### Filtrera ut systemarbetsflöden när arbetsflödesstatus kontrolleras {#filtering-out-system-workflows-when-checking-workflow-status}

Du kan använda [WorkflowStatus API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/status/WorkflowStatus.html) för att hämta information om arbetsflödesstatus för en nod.

Olika metoder har parametern:

`excludeSystemWorkflows`

Den här parametern kan ställas in på `true` för att ange att systemarbetsflöden ska uteslutas från relevanta resultat.

Du [kan uppdatera OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) **Adobe Granite Workflow PayloadMapCache** som anger att arbetsflödet `Models` ska betraktas som systemarbetsflöden. Standardarbetsflödesmodellerna (runtime) är:

* `/var/workflow/models/scheduled_activation/jcr:content/model`
* `/var/workflow/models/scheduled_deactivation/jcr:content/model`

### Gå automatiskt vidare till deltagarsteget efter en timeout {#auto-advance-participant-step-after-a-timeout}

Om du behöver gå vidare till ett **Deltagare**-steg automatiskt som inte har slutförts inom en fördefinierad tid kan du:

1. Implementera en OSGI-händelseavlyssnare för att lyssna på när uppgifter skapas och ändras.
1. Ange en tidsgräns (deadline) och skapa sedan ett schemalagt snedstreck som ska utlösas vid den tidpunkten.
1. Skriv en jobbhanterare som meddelas när tidsgränsen går ut och utlöser jobbet.

   Den här hanteraren utför den nödvändiga åtgärden för aktiviteten om den inte har slutförts ännu

>[!NOTE]
>
>De åtgärder som skall vidtas måste vara klart definierade för att kunna använda detta tillvägagångssätt.

### Interagera med arbetsflödesinstanser {#interacting-with-workflow-instances}

Nedan finns grundläggande exempel på hur du interagerar (programmatiskt) med arbetsflödesinstanser.

#### Interagera med arbetsflödesinstanser - Java {#interacting-with-workflow-instances-java}

```java
// starting a workflow
WorkflowModel model = wfSession.getModel(workflowId);
WorkflowData wfData = wfSession.newWorkflowData("JCR_PATH", repoPath);
wfSession.startWorkflow(model, wfData);

// querying and managing a workflow
Workflow[] workflows workflows = wfSession.getAllWorkflows();
Workflow workflow= wfSession.getWorkflow(id);
wfSession.suspendWorkflow(workflow);
wfSession.resumeWorkflow(workflow);
wfSession.terminateWorkflow(workflow);
```

#### Interagera med arbetsflödesinstanser - ECMA Script {#interacting-with-workflow-instances-ecma-script}

```
// starting a workflow
var model = wfSession.getModel(workflowId);
var wfData = wfSession.newWorkflowData("JCR_PATH", repoPath);
wfSession.startWorkflow(model, wfData);

// querying and managing a workflow
var workflows = wfSession.getWorkflows("RUNNING");
var workflow= wfSession.getWorkflow(id);
wfSession.suspendWorkflow(workflow);
wfSession.resumeWorkflow(workflow);
wfSession.terminateWorkflow(workflow);
```

#### Interagera med arbetsflödesinstanser - REST med vändning {#interacting-with-workflow-instances-rest-using-curl}

* **Starta ett arbetsflöde**

  ```shell
  # starting a workflow
  curl -d "model={id}&payloadType={type}&payload={payload}" http://localhost:4502/etc/workflow/instances
  
  # for example:
  curl -u admin:admin -d "model=/var/workflow/models/request_for_activation&payloadType=JCR_PATH&payload=/content/we-retail/us/en/products" http://localhost:4502/etc/workflow/instances
  ```

* **Visar instanser**

  ```shell
  # listing the instances
  curl -u admin:admin http://localhost:4502/etc/workflow/instances.json
  ```

  Då visas alla förekomster, till exempel:

  ```shell
  [
      {"uri":"/var/workflow/instances/server0/2018-02-26/prototype-01_1"}
      ,{"uri":"/var/workflow/instances/server0/2018-02-26/prototype-01_2"}
  ]
  ```

  >[!NOTE]
  >
  >Se [Så här hämtar du en lista över alla pågående arbetsflöden](#how-to-get-a-list-of-all-running-workflows-with-their-ids) med deras ID:n för att visa instanser med en viss status.

* **Pausa ett arbetsflöde**

  ```shell
  # suspending a workflow
  curl -d "state=SUSPENDED" http://localhost:4502/etc/workflow/instances/{id}
  
  # for example:
  curl -u admin:admin -d "state=SUSPENDED" http://localhost:4502/etc/workflow/instances/server0/2017-03-08/request_for_activation_1
  ```

* **Återuppta ett arbetsflöde**

  ```shell
  # resuming a workflow
  curl -d "state=RUNNING" http://localhost:4502/etc/workflow/instances/{id}
  
  # for example:
  curl -u admin:admin -d "state=RUNNING" http://localhost:4502/etc/workflow/instances/server0/2017-03-08/request_for_activation_1
  ```

* **Avslutar en arbetsflödesinstans**

  ```shell
  # terminating a workflow
  curl -d "state=ABORTED" http://localhost:4502/etc/workflow/instances/{id}
  
  # for example:
  curl -u admin:admin -d "state=ABORTED" http://localhost:4502/etc/workflow/instances/server0/2017-03-08/request_for_activation_1
  ```

### Interagera med arbetsuppgifter {#interacting-with-work-items}

Nedan följer grundläggande exempel på hur du interagerar (programmatiskt) med arbetsobjekt.

#### Interagera med arbetsuppgifter - Java {#interacting-with-work-items-java}

```java
// querying work items
WorkItem[] workItems = wfSession.getActiveWorkItems();
WorkItem workItem = wfSession.getWorkItem(id);

// getting routes
List<Route> routes = wfSession.getRoutes(workItem);

// delegating
Iterator<Participant> delegatees = wfSession.getDelegatees(workItem);
wfSession.delegateWorkItem(workItem, delegatees.get(0));

// completing or advancing to the next step
wfSession.complete(workItem, routes.get(0));
```

#### Interagera med arbetsobjekt - ECMA Script {#interacting-with-work-items-ecma-script}

```
// querying work items
var workItems = wfSession.getActiveWorkItems();
var workItem = wfSession.getWorkItem(id);

// getting routes
var routes = wfSession.getRoutes(workItem);

// delegating
var delegatees = wfSession.getDelegatees(workItem);
wfSession.delegateWorkItem(workItem, delegatees.get(0));

// completing or advancing to the next step
wfSession.complete(workItem, routes.get(0));
```

#### Interagera med arbetsobjekt - REST med vändning {#interacting-with-work-items-rest-using-curl}

* **Visar arbetsobjekt från inkorgen**

  ```shell
  # listing the work items
  curl -u admin:admin http://localhost:4502/bin/workflow/inbox
  ```

  Information om arbetsobjekt som finns i Inkorgen visas, till exempel:

  ```shell
  [{
      "uri_xss": "/var/workflow/instances/server0/2018-02-26/prototype-01_2/workItems/node2_var_workflow_instances_server0_2018-02-26_prototype-01_2",
      "uri": "/var/workflow/instances/server0/2018-02-26/prototype-01_2/workItems/node2_var_workflow_instances_server0_2018-02-26_prototype-01_2",
      "currentAssignee_xss": "workflow-administrators",
      "currentAssignee": "workflow-administrators",
      "startTime": 1519656289274,
      "payloadType_xss": "JCR_PATH",
      "payloadType": "JCR_PATH",
      "payload_xss": "/content/we-retail/es/es",
      "payload": "/content/we-retail/es/es",
      "comment_xss": "Process resource is null",
      "comment": "Process resource is null",
      "type_xss": "WorkItem",
      "type": "WorkItem"
    },{
      "uri_xss": "configuration/configure_analyticstargeting",
      "uri": "configuration/configure_analyticstargeting",
      "currentAssignee_xss": "administrators",
      "currentAssignee": "administrators",
      "type_xss": "Task",
      "type": "Task"
    },{
      "uri_xss": "configuration/securitychecklist",
      "uri": "configuration/securitychecklist",
      "currentAssignee_xss": "administrators",
      "currentAssignee": "administrators",
      "type_xss": "Task",
      "type": "Task"
    },{
      "uri_xss": "configuration/enable_collectionofanonymoususagedata",
      "uri": "configuration/enable_collectionofanonymoususagedata",
      "currentAssignee_xss": "administrators",
      "currentAssignee": "administrators",
      "type_xss": "Task",
      "type": "Task"
    },{
      "uri_xss": "configuration/configuressl",
      "uri": "configuration/configuressl",
      "currentAssignee_xss": "administrators",
      "currentAssignee": "administrators",
      "type_xss": "Task",
      "type": "Task"
    }
  ```

* **Delegerar arbetsobjekt**

  ```xml
  # delegating
  curl -d "item={item}&delegatee={delegatee}" http://localhost:4502/bin/workflow/inbox
  
  # for example:
  curl -u admin:admin -d "item=/etc/workflow/instances/server0/2017-03-08/request_for_activation_1/workItems/node1_etc_workflow_instances_server0_2017-03-08_request_for_act_1&delegatee=administrators" http://localhost:4502/bin/workflow/inbox
  ```

  >[!NOTE]
  >
  >`delegatee` måste vara ett giltigt alternativ för arbetsflödessteget.

* **Slutför eller flyttar arbetsobjekt till nästa steg**

  ```xml
  # retrieve the list of routes; the results will be similar to {"results":1,"routes":[{"rid":"233123169","label":"End","label_xss":"End"}]}
  http://localhost:4502/etc/workflow/instances/<path-to-the-workitem>.routes.json
  
  # completing or advancing to the next step; use the appropriate route ID (rid value) from the above list
  curl -d "item={item}&route={route}" http://localhost:4502/bin/workflow/inbox
  
  # for example:
  curl -u admin:admin -d "item=/etc/workflow/instances/server0/2017-03-08/request_for_activation_1/workItems/node1_etc_workflow_instances_server0_2017-03-08_request_for_activation_1&route=233123169" http://localhost:4502/bin/workflow/inbox
  ```

### Lyssna efter arbetsflödeshändelser {#listening-for-workflow-events}

Använd OSGi-händelseramverket för att lyssna efter händelser som definieras av klassen [`com.adobe.granite.workflow.event.WorkflowEvent`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/event/WorkflowEvent.html). Den här klassen innehåller också flera användbara metoder för att hämta information om föremålet för händelsen. Metoden `getWorkItem` returnerar till exempel objektet `WorkItem` för det arbetsobjekt som är involverat i händelsen.

I följande exempelkod definieras en tjänst som avlyssnar arbetsflödeshändelser och utför åtgärder utifrån händelsetypen.

```java
package com.adobe.example.workflow.listeners;

import org.apache.sling.event.jobs.JobProcessor;
import org.apache.sling.event.jobs.JobUtil;

import org.osgi.service.event.Event;
import org.osgi.service.event.EventHandler;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Property;
import org.apache.felix.scr.annotations.Service;

import com.adobe.granite.workflow.event.WorkflowEvent;
import com.adobe.granite.workflow.exec.WorkItem;

/**
 * The <code>WorkflowEventCatcher</code> class listens to workflow events.
 */
@Component(metatype=false, immediate=true)
@Service(value=org.osgi.service.event.EventHandler.class)
public class WorkflowEventCatcher implements EventHandler, JobProcessor {

 @Property(value=com.adobe.granite.workflow.event.WorkflowEvent.EVENT_TOPIC)
 static final String EVENT_TOPICS = "event.topics";

 private static final Logger logger = LoggerFactory.getLogger(WorkflowEventCatcher.class);

 public void handleEvent(Event event) {
  JobUtil.processJob(event, this);
 }

 public boolean process(Event event) {
  logger.info("Received event of topic: " + event.getTopic());
  String topic = event.getTopic();

  try {
   if (topic.equals(WorkflowEvent.EVENT_TOPIC)) {
    WorkflowEvent wfevent = (WorkflowEvent)event;
    String eventType = wfevent.getEventType();
    String instanceId = wfevent.getWorkflowInstanceId();

    if (instanceId != null) {
     //workflow instance events
     if (eventType.equals(WorkflowEvent.WORKFLOW_STARTED_EVENT) ||
       eventType.equals(WorkflowEvent.WORKFLOW_RESUMED_EVENT) ||
       eventType.equals(WorkflowEvent.WORKFLOW_SUSPENDED_EVENT)) {
      // your code comes here...
     } else if (
       eventType.equals(WorkflowEvent.WORKFLOW_ABORTED_EVENT) ||
       eventType.equals(WorkflowEvent.WORKFLOW_COMPLETED_EVENT)) {
      // your code comes here...
     }
     // workflow node event
     if (eventType.equals(WorkflowEvent.NODE_TRANSITION_EVENT)) {
      WorkItem currentItem = (WorkItem) event.getProperty(WorkflowEvent.WORK_ITEM);
      // your code comes here...
     }
    }
   }
  } catch(Exception e){
   logger.debug(e.getMessage());
   e.printStackTrace();
  }
  return true;
 }
}
```
