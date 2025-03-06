---
title: Utöka arbetsflödesfunktioner
description: Lär dig hur du utökar Adobe Experience Manager arbetsflödesfunktioner.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 9036e26c-74cd-4013-a63d-70ece0f80904
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3499'
ht-degree: 0%

---

# Utöka arbetsflödesfunktioner{#extending-workflow-functionality}

I det här avsnittet beskrivs hur du utvecklar anpassade stegkomponenter för dina arbetsflöden och hur du interagerar programmatiskt med arbetsflöden.

När du skapar ett anpassat arbetsflödessteg ingår följande aktiviteter:

* Utveckla stegkomponenten för arbetsflödet.
* Implementera stegfunktionerna som en OSGi-tjänst eller ett ECMA-skript.

Du kan även [interagera med dina arbetsflöden från program och skript](/help/sites-developing/workflows-program-interaction.md).

## Steg-komponenter för arbetsflöde - Grunderna {#workflow-step-components-the-basics}

En arbetsflödesstegkomponent definierar utseendet och beteendet för steget när du skapar arbetsflödesmodeller:

* Kategori- och stegnamn i arbetsflödets sidospark.
* Stegen i arbetsflödesmodeller.
* Redigeringsdialogrutan för konfiguration av komponentegenskaper.
* Tjänsten eller skriptet som körs vid körning.

Precis som med [alla komponenter](/help/sites-developing/components.md) ärver arbetsflödesstegkomponenter från den komponent som har angetts för egenskapen `sling:resourceSuperType`. I följande diagram visas hierarkin för `cq:component` noder som utgör grunden för alla komponenter för arbetsflödessteg. Diagrammet innehåller också komponenterna **Processsteg**, **Deltagarsteg** och **Dynamiskt deltagarsteg** eftersom dessa är de vanligaste (och vanligaste) startpunkterna för utveckling av anpassade stegkomponenter.

![aem_wf_component_inherit](assets/aem_wf_componentinherit.png)

>[!CAUTION]
>
>Du ***får*** inte ändra något i sökvägen `/libs`.
>
>Detta beror på att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan mycket väl skrivas över när du använder en snabbkorrigering eller ett funktionspaket).
>
>Den rekommenderade metoden för konfiguration och andra ändringar är:
>
>1. Återskapa det obligatoriska objektet (det vill säga som det finns i `/libs` under `/apps`)
>2. Gör ändringar i `/apps`

Komponenten `/libs/cq/workflow/components/model/step` är den närmaste gemensamma överordnade för **Processsteg**, **Deltagarsteg** och **Dynamiskt deltagarsteg** som alla ärver följande objekt:

* `step.jsp`

  Skriptet `step.jsp` återger titeln på stegkomponenten när den läggs till i en modell.

  ![wf-22-1](assets/wf-22-1.png)

* [cq:dialog](/help/sites-developing/developing-components.md#creating-and-configuring-a-dialog)

  En dialogruta med följande flikar:

   * **Allmänt**: för redigering av titeln och beskrivningen.
   * **Avancerat**: för redigering av egenskaper för e-postmeddelanden.

  ![wf-44](assets/wf-44.png) ![wf-45](assets/wf-45.png)

  >[!NOTE]
  >
  >När flikarna i redigeringsdialogrutan för en stegkomponent inte matchar det här standardutseendet har stegkomponenten definierat skript, nodegenskaper eller dialogruteflikar som åsidosätter de ärvda flikarna.

### ECMA-skript {#ecma-scripts}

Följande objekt är tillgängliga (beroende på stegtyp) i ECMA-skript:

* [WorkItem](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/workflow/exec/WorkItem.html) workItem
* [WorkflowSession](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/workflow/WorkflowSession.html) workflowSession
* [WorkflowData](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/workflow/exec/WorkflowData.html) workflowData
* `args`: matris med processargument.

* `sling`: om du vill komma åt andra SGB-tjänster.
* `jcrSession`

### MetaDataMaps {#metadatamaps}

Du kan använda arbetsflödets metadata för att behålla information som krävs under arbetsflödets livstid. Ett vanligt krav för arbetsflödessteg är att behålla data för framtida användning i arbetsflödet eller att hämta beständiga data.

Det finns tre typer av MetaDataMap-objekt - för `Workflow`-, `WorkflowData`- och `WorkItem`-objekt. De har alla samma syfte - att lagra metadata.

En WorkItem har en egen MetaDataMap som bara kan användas medan arbetsobjektet (till exempel steg) körs.

Både `Workflow` och `WorkflowData` metadatamappningar delas över hela arbetsflödet. I dessa fall bör du bara använda metadatamappningen `WorkflowData`.

## Skapa anpassade komponenter för arbetsflödessteg {#creating-custom-workflow-step-components}

Arbetsflödesstegkomponenter kan [skapas på samma sätt som andra komponenter](/help/sites-developing/components.md).

Om du vill ärva från en av de (befintliga) basstegskomponenterna lägger du till följande egenskap i noden `cq:Component`:

* Namn: `sling:resourceSuperType`
* Typ: `String`
* Värde: En av följande sökvägar som matchar en baskomponent:

   * `cq/workflow/components/model/process`
   * `cq/workflow/components/model/participant`
   * `cq/workflow/components/model/dynamic_participant`

### Ange standardtitel och beskrivning för stegförekomster {#specifying-the-default-title-and-description-for-step-instances}

Använd följande procedur för att ange standardvärden för fälten **Rubrik** och **Beskrivning** på fliken **Allmänt**.

>[!NOTE]
>
>Fältvärdena visas i steginstansen när båda följande krav är uppfyllda:
>
>* I redigeringsdialogrutan för steget lagras titeln och beskrivningen på följande platser: >
>* `./jcr:title`
>* `./jcr:description` platser
>
>  Detta krav uppfylls när redigeringsdialogrutan använder fliken Allmänt som komponenten `/libs/cq/flow/components/step/step` implementerar.
>
>* Stegkomponenten eller ett överordnat element för komponenten åsidosätter inte skriptet `step.jsp` som implementeras av komponenten `/libs/cq/flow/components/step/step`.

1. Lägg till följande nod under noden `cq:Component`:

   * Namn: `cq:editConfig`
   * Typ: `cq:EditConfig`

   >[!NOTE]
   >
   >Mer information om cq:editConfig-noden finns i [Konfigurera redigeringsbeteendet för en komponent](/help/sites-developing/developing-components.md#configuring-the-edit-behavior).

1. Lägg till följande nod under noden `cq:EditConfig`:

   * Namn: `cq:formParameters`
   * Typ: `nt:unstructured`

1. Lägg till `String`-egenskaper för följande namn i noden `cq:formParameters`:

   * `jcr:title`: Värdet fyller i fältet **Titel** på fliken **Allmänt**.
   * `jcr:description`: Värdet fyller i fältet **Beskrivning** på fliken **Allmänt**.

### Spara egenskapsvärden i arbetsflödesmetadata {#saving-property-values-in-workflow-metadata}

>[!NOTE]
>
>Se [Bevara och komma åt data](#persisting-and-accessing-data). Mer information om hur du får åtkomst till egenskapsvärdet vid körning finns i [Åtkomst till egenskapsvärden för dialogrutor vid körning](#accessing-dialog-property-values-at-runtime).

Egenskapen name för `cq:Widget` objekt anger den JCR-nod som lagrar widgetens värde. När widgetar i dialogrutan för arbetsflödesstegkomponenter lagrar värden under noden `./metaData` läggs värdet till i arbetsflödet `MetaDataMap`.

Ett textfält i en dialogruta är till exempel en `cq:Widget`-nod som har följande egenskaper:

| Namn | Typ | Värde |
|---|---|---|
| `xtype` | `String` | `textarea` |
| `name` | `String` | `./metaData/subject` |
| `fieldLabel` | `String` | `Email Subject` |

Värdet som anges i det här textfältet läggs till i arbetsflödesinstansens ` [MetaDataMap](#metadatamaps)`-objekt och är associerat med `subject`-tangenten.

>[!NOTE]
>
>När nyckeln är `PROCESS_ARGS` är värdet lätt tillgängligt i ECMA-skriptimplementeringar via variabeln `args`. I det här fallet är värdet för name-egenskapen `./metaData/PROCESS_ARGS.`

### Åsidosätta stegimplementeringen {#overriding-the-step-implementation}

Med varje basstegskomponent kan utvecklare av arbetsflödesmodeller konfigurera följande nyckelfunktioner i designläge:

* Processsteg: Tjänsten eller ECMA-skriptet som ska köras under körning.
* Deltagarsteg: ID:t för den användare som har tilldelats det genererade arbetsobjektet.
* Dynamiskt deltagarsteg: Tjänsten eller ECMA-skriptet som väljer ID för användaren som är tilldelad arbetsposten.

Om du vill fokusera komponenten för användning i ett specifikt arbetsflödesscenario konfigurerar du nyckelfunktionen i designen och tar bort möjligheten för modellutvecklare att ändra den.

1. Lägg till följande nod under cq:component-noden:

   * Namn: `cq:editConfig`
   * Typ: `cq:EditConfig`

   Mer information om cq:editConfig-noden finns i [Konfigurera redigeringsbeteendet för en komponent](/help/sites-developing/developing-components.md#configuring-the-edit-behavior).

1. Under cq:EditConfig-noden lägger du till följande nod:

   * Namn: `cq:formParameters`
   * Typ: `nt:unstructured`

1. Lägg till en `String`-egenskap i noden `cq:formParameters`. Komponentens överordnade typ avgör egenskapens namn:

   * Processsteg: `PROCESS`
   * Deltagarsteg: `PARTICIPANT`
   * Steg för dynamisk deltagare: `DYNAMIC_PARTICIPANT`

1. Ange egenskapens värde:

   * `PROCESS`: Sökvägen till ECMA-skriptet eller PID för tjänsten som implementerar stegbeteendet.
   * `PARTICIPANT`: ID:t för användaren som har tilldelats arbetsobjektet.
   * `DYNAMIC_PARTICIPANT`: Sökvägen till ECMA-skriptet eller PID för tjänsten som väljer användaren att tilldela arbetsposten.

1. Om du vill ta bort möjligheten för modellutvecklare att ändra egenskapsvärden åsidosätter du dialogrutan för komponentens supertyp.

### Lägga till Forms och dialogrutor i deltagarsteg {#adding-forms-and-dialogs-to-participant-steps}

Anpassa stegkomponenten för deltagare för att tillhandahålla funktioner som finns i komponenterna [Formulärdeltagare, steg](/help/sites-developing/workflows-step-ref.md#form-participant-step) och [Dialogrutedeltagare, steg](/help/sites-developing/workflows-step-ref.md#dialog-participant-step):

* Visa ett formulär för användaren när han/hon öppnar det genererade arbetsobjektet.
* Visa en anpassad dialogruta för användaren när han/hon slutför det genererade arbetsobjektet.

Utför följande procedur på den nya komponenten (se [Skapa anpassade stegkomponenter för arbetsflöde](#creating-custom-workflow-step-components)):

1. Lägg till följande nod under noden `cq:Component`:

   * Namn: `cq:editConfig`
   * Typ: `cq:EditConfig`

   Mer information om cq:editConfig-noden finns i [Konfigurera redigeringsbeteendet för en komponent](/help/sites-developing/components-basics.md#edit-behavior).

1. Under cq:EditConfig-noden lägger du till följande nod:

   * Namn: `cq:formParameters`
   * Typ: `nt:unstructured`

1. Om du vill visa ett formulär när användaren öppnar arbetsobjektet lägger du till följande egenskap i noden `cq:formParameters`:

   * Namn: `FORM_PATH`
   * Typ: `String`
   * Värde: Sökvägen som matchar formuläret

1. Om du vill visa en anpassad dialogruta när användaren slutför arbetsobjektet lägger du till följande egenskap i noden `cq:formParameters`

   * Namn: `DIALOG_PATH`
   * Typ: `String`
   * Värde: Sökvägen som leder till dialogrutan

### Konfigurera beteende för arbetsflödesstegets körningsmiljö {#configuring-the-workflow-step-runtime-behavior}

Lägg till en `cq:EditConfig`-nod nedanför noden `cq:Component`. Under den här noden lägger du till en `nt:unstructured`-nod (måste heta `cq:formParameters`) och lägger till följande egenskaper:

* Namn: `PROCESS_AUTO_ADVANCE`

   * Typ: `Boolean`
   * Värde:

      * om det anges till `true` kommer arbetsflödet att köra det steget och fortsätta - detta är standard och rekommenderas också
      * när `false` kommer arbetsflödet att köras och stoppas. Detta kräver extra hantering, så `true` rekommenderas

* Namn: `DO_NOTIFY`

   * Typ: `Boolean`
   * Värde: anger om e-postmeddelanden ska skickas för användardeltagande (och förutsätter att e-postservern är korrekt konfigurerad)

## Bevara och få åtkomst till data {#persisting-and-accessing-data}

### Beständiga data för efterföljande arbetsflödessteg {#persisting-data-for-subsequent-workflow-steps}

Du kan använda arbetsflödets metadata för att behålla information som krävs under arbetsflödets livstid - och mellan steg. Ett vanligt krav för arbetsflödessteg är att bevara data för framtida bruk eller att hämta beständiga data från tidigare steg.

Arbetsflödets metadata lagras i ett [`MetaDataMap`](#metadatamaps)-objekt. Java-API:t tillhandahåller metoden [`Workflow.getWorkflowData`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/Workflow.html) för att returnera ett [`WorkflowData`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowData.html) -objekt som innehåller rätt `MetaDataMap`-objekt. Det här `WorkflowData` `MetaDataMap`-objektet är tillgängligt för OSGi-tjänsten eller ECMA-skriptet för en stegkomponent.

#### Java {#java}

Körningsmetoden för implementeringen av `WorkflowProcess` skickas till objektet `WorkItem`. Använd det här objektet för att hämta `WorkflowData`-objektet för den aktuella arbetsflödesinstansen. I följande exempel läggs ett objekt till i arbetsflödesobjektet `MetaDataMap` och sedan loggas varje objekt. Objektet (&quot;minkey&quot;,&quot;My Step Value&quot;) är tillgängligt för efterföljande steg i arbetsflödet.

```java
public void execute(WorkItem item, WorkflowSession session, MetaDataMap args) throws WorkflowException {

    MetaDataMap wfd = item.getWorkflow().getWorkflowData().getMetaDataMap();

    wfd.put("mykey", "My Step Value");

    Set<String> keyset = wfd.keySet();
    Iterator<String> i = keyset.iterator();
    while (i.hasNext()){
     Object key = i.next();
     log.info("The workflow medata includes key {} and value {}",key.toString(),wfd.get(key).toString());
    }
}
```

#### ECMA-skript {#ecma-script}

Variabeln `graniteWorkItem` är ECMA-skriptrepresentationen av det aktuella `WorkItem` Java-objektet. Du kan därför använda variabeln `graniteWorkItem` för att hämta arbetsflödets metadata. Följande ECMA-skript kan användas för att implementera ett **Processsteg** för att lägga till ett objekt i arbetsflödesobjektet `MetaDataMap` och sedan logga varje objekt. Dessa objekt är sedan tillgängliga för efterföljande steg i arbetsflödet.

>[!NOTE]
>
>Variabeln `metaData` som är omedelbart tillgänglig för stegskriptet är metadata för steget. Metadata för steget skiljer sig från metadata för arbetsflödet.

```
var currentDateInMillis = new Date().getTime();

graniteWorkItem.getWorkflowData().getMetaDataMap().put("hardcodedKey","theKey");

graniteWorkItem.getWorkflowData().getMetaDataMap().put("currentDateInMillisKey",currentDateInMillis);

var iterator = graniteWorkItem.getWorkflowData().getMetaDataMap().keySet().iterator();
while (iterator.hasNext()){
    var key = iterator.next();
    log.info("Workflow metadata key, value = " + key.toString() + ", " + graniteWorkItem.getWorkflowData().getMetaDataMap().get(key));
}
```

### Åtkomst till egenskapsvärden för dialogrutor vid körning {#accessing-dialog-property-values-at-runtime}

Objektet `MetaDataMap` i arbetsflödesinstanser är användbart för att lagra och hämta data under arbetsflödets hela livstid. För implementeringar av komponenter för arbetsflödessteg är `MetaDataMap` särskilt användbart för att hämta egenskapsvärden för komponenter vid körning.

>[!NOTE]
>
>Mer information om hur du konfigurerar komponentdialogrutan för att lagra egenskaper som arbetsflödesmetadata finns i [Spara egenskapsvärden i arbetsflödesmetadata](#saving-property-values-in-workflow-metadata).

Arbetsflödet `MetaDataMap` är tillgängligt för implementeringar av Java- och ECMA-skriptprocessen:

* I Java-implementeringar av WorkflowProcess-gränssnittet är parametern `args` arbetsflödets `MetaDataMap`-objekt.

* I ECMA-skriptimplementeringar är värdet tillgängligt med hjälp av variablerna `args` och `metadata`.

### Exempel: Hämta argument för komponenten Processsteg {#example-retrieving-the-arguments-of-the-process-step-component}

Redigeringsdialogrutan för komponenten **Processsteg** innehåller egenskapen **Arguments** . Värdet för egenskapen **Arguments** lagras i arbetsflödets metadata och associeras med nyckeln `PROCESS_ARGS`.

I följande diagram är värdet för egenskapen **Arguments** `argument1, argument2`:

![wf-24](assets/wf-24.png)

#### Java {#java-1}

Följande Java-kod är metoden `execute` för en `WorkflowProcess`-implementering. Metoden loggar värdet i `args` `MetaDataMap` som är associerad med nyckeln `PROCESS_ARGS`.

```java
public void execute(WorkItem item, WorkflowSession session, MetaDataMap args) throws WorkflowException {
     if (args.containsKey("PROCESS_ARGS")){
      log.info("workflow metadata for key PROCESS_ARGS and value {}",args.get("PROCESS_ARGS","string").toString());
     }
    }
```

När ett processsteg som använder den här Java-implementeringen körs innehåller loggen följande post:

```xml
16.02.2018 12:07:39.566 *INFO* [JobHandler: /var/workflow/instances/server0/2018-02-16/model_855140139900189:/content/we-retail/de] com.adobe.example.workflow.impl.process.LogArguments workflow metadata for key PROCESS_ARGS and value argument1, argument2
```

#### ECMA-skript {#ecma-script-1}

Följande ECMA-skript används som process för **Processsteg**. Här loggas antalet argument och argumentvärdena:

```
var iterator = graniteWorkItem.getWorkflowData().getMetaDataMap().keySet().iterator();
while (iterator.hasNext()){
    var key = iterator.next();
    log.info("Workflow metadata key, value = " + key.toString() + ", " + graniteWorkItem.getWorkflowData().getMetaDataMap().get(key));
}
log.info("hardcodedKey "+ graniteWorkItem.getWorkflowData().getMetaDataMap().get("hardcodedKey"));
log.info("currentDateInMillisKey "+ graniteWorkItem.getWorkflowData().getMetaDataMap().get("currentDateInMillisKey"));
```

>[!NOTE]
>
>I det här avsnittet beskrivs hur du arbetar med argument för processsteg. Informationen gäller även för dynamiska deltagarval.

>[!NOTE]
>Ett annat exempel på hur du lagrar komponentegenskaper i arbetsflödets metadata finns i Exempel: Skapa ett steg för loggningsarbetsflöde. Det här exemplet innehåller en dialogruta som associerar metadatavärde med en annan nyckel än PROCESS_ARGS.

### Skript och processargument {#scripts-and-process-arguments}

I ett skript för en **Process Step**-komponent är argument tillgängliga via objektet `args`.

När du skapar en anpassad stegkomponent är objektet `metaData` tillgängligt i ett skript. Det här objektet är begränsat till ett strängargument.

## Utveckla stegimplementeringar för processer {#developing-process-step-implementations}

När processteg startas under en arbetsflödesprocess skickar stegen en begäran till en OSGi-tjänst eller kör ett ECMA-skript. Utveckla den tjänst eller det ECMA-skript som utför de åtgärder som arbetsflödet kräver.

>[!NOTE]
>
>Mer information om hur du associerar processstegskomponenten med tjänsten eller skriptet finns i [Processsteg](/help/sites-developing/workflows-step-ref.md#process-step) eller [Åsidosätta stegimplementeringen](#overriding-the-step-implementation).

### Implementera ett processsteg med en Java-klass {#implementing-a-process-step-with-a-java-class}

Så här definierar du ett processteg som en OSGI-tjänstkomponent (Java bundle):

1. Skapa paketet och distribuera det i OSGI-behållaren. Mer information finns i dokumentationen om hur du skapar ett paket med [CRXDE Lite](/help/sites-developing/developing-with-crxde-lite.md) eller [Eclipse](/help/sites-developing/howto-projects-eclipse.md).

   >[!NOTE]
   >
   >OSGI-komponenten måste implementera gränssnittet `WorkflowProcess` med dess `execute()`-metod. Se exempelkoden nedan.

   >[!NOTE]
   >
   >Paketnamnet måste läggas till i avsnittet `<*Private-Package*>` i `maven-bundle-plugin`-konfigurationen.

1. Lägg till SCR-egenskapen `process.label` och ange värdet efter behov. Detta är det namn som processsteget listas som när du använder den generiska **processstegskomponenten**. Se exemplet nedan.
1. I redigeraren för **modeller** lägger du till processsteget i arbetsflödet med den generiska komponenten **Processsteg**.
1. Gå till fliken **Process** i dialogrutan Redigera (i **Processsteg**) och välj processimplementering.
1. Om du använder argument i koden anger du **Processargument**. Till exempel: false.
1. Spara ändringarna för både steget och arbetsflödesmodellen (modellredigerarens övre vänstra hörn).

Java-metoderna, respektive klasserna som implementerar den körbara Java-metoden, registreras som OSGI-tjänster, vilket gör att du kan lägga till metoder när som helst under körningen.

Följande OSGI-komponent lägger till egenskapen `approved` i sidinnehållsnoden när nyttolasten är en sida:

```java
package com.adobe.example.workflow.impl.process;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Property;
import org.apache.felix.scr.annotations.Service;

import org.osgi.framework.Constants;

import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;

/**
 * Sample workflow process that sets an <code>approve</code> property to the payload based on the process argument value.
 */
@Component
@Service
public class MyProcess implements WorkflowProcess {

 @Property(value = "An example workflow process implementation.")
 static final String DESCRIPTION = Constants.SERVICE_DESCRIPTION;
 @Property(value = "Adobe")
 static final String VENDOR = Constants.SERVICE_VENDOR;
 @Property(value = "My Sample Workflow Process")
 static final String LABEL="process.label";

 private static final String TYPE_JCR_PATH = "JCR_PATH";

 public void execute(WorkItem item, WorkflowSession session, MetaDataMap args) throws WorkflowException {
  WorkflowData workflowData = item.getWorkflowData();
  if (workflowData.getPayloadType().equals(TYPE_JCR_PATH)) {
   String path = workflowData.getPayload().toString() + "/jcr:content";
   try {
    Session jcrSession = session.adaptTo(Session.class);
    Node node = (Node) jcrSession.getItem(path);
    if (node != null) {
     node.setProperty("approved", readArgument(args));
     jcrSession.save();
    }
   } catch (RepositoryException e) {
    throw new WorkflowException(e.getMessage(), e);
   }
  }
 }

 private boolean readArgument(MetaDataMap args) {
  String argument = args.get("PROCESS_ARGS", "false");
  return argument.equalsIgnoreCase("true");
 }
}
```

>[!NOTE]
>
>Om processen misslyckas tre gånger i rad placeras ett objekt i Inkorgen för arbetsflödesadministratören.

### Använda ECMAScript {#using-ecmascript}

Med ECMA-skript kan skriptutvecklare implementera processsteg. Skripten finns i JCR-databasen och körs därifrån.

I följande tabell visas de variabler som är omedelbart tillgängliga för att bearbeta skript, vilket ger tillgång till objekt i arbetsflödets Java API.

| Java-klass | Skriptvariabelnamn | Beskrivning |
|---|---|---|
| `com.adobe.granite.workflow.exec.WorkItem` | `graniteWorkItem` | Den aktuella steginstansen. |
| `com.adobe.granite.workflow.WorkflowSession` | `graniteWorkflowSession` | Arbetsflödessessionen för den aktuella steginstansen. |
| `String[]` (innehåller processargument) | `args` | Stegargumenten. |
| `com.adobe.granite.workflow.metadata.MetaDataMap` | `metaData` | Metadata för aktuell steginstans. |
| `org.apache.sling.scripting.core.impl.InternalScriptHelper` | `sling` | Ger åtkomst till Sling-miljön. |

Följande exempelskript visar hur du får åtkomst till JCR-noden som representerar arbetsflödets nyttolast. Variabeln `graniteWorkflowSession` är anpassad till en JCR-sessionsvariabel, som används för att hämta noden från nyttolastsökvägen.

```
var workflowData = graniteWorkItem.getWorkflowData();
if (workflowData.getPayloadType() == "JCR_PATH") {
    var path = workflowData.getPayload().toString();
    var jcrsession = graniteWorkflowSession.adaptTo(Packages.javax.jcr.Session);
    var node = jcrsession.getNode(path);
    if (node.hasProperty("approved")){
     node.setProperty("approved", args[0] == "true" ? true : false);
     node.save();
 }
}
```

Följande skript kontrollerar om nyttolasten är en bild ( `.png` fil), skapar en svartvit bild från den och sparar den som en nod på samma nivå.

```
var workflowData = graniteWorkItem.getWorkflowData();
if (workflowData.getPayloadType() == "JCR_PATH") {
    var path = workflowData.getPayload().toString();
    var jcrsession = graniteWorkflowSession.adaptTo(Packages.javax.jcr.Session);
    var node = jcrsession.getRootNode().getNode(path.substring(1));
     if (node.isNodeType("nt:file") && node.getProperty("jcr:content/jcr:mimeType").getString().indexOf("image/") == 0) {
        var is = node.getProperty("jcr:content/jcr:data").getStream();
        var layer = new Packages.com.day.image.Layer(is);
        layer.grayscale();
                var parent = node.getParent();
                var gn = parent.addNode("grey" + node.getName(), "nt:file");
        var content = gn.addNode("jcr:content", "nt:resource");
                content.setProperty("jcr:mimeType","image/png");
                var cal = Packages.java.util.Calendar.getInstance();
                content.setProperty("jcr:lastModified",cal);
                var f = Packages.java.io.File.createTempFile("test",".png");
        var tout = new Packages.java.io.FileOutputStream(f);
        layer.write("image/png", 1.0, tout);
        var fis = new Packages.java.io.FileInputStream(f);
                content.setProperty("jcr:data", fis);
                parent.save();
        tout.close();
        fis.close();
        is.close();
        f.deleteOnExit();
    }
}
```

Så här använder du skriptet:

1. Skapa skriptet (till exempel med CRXDE Lite) och spara det i databasen nedan `//apps/workflow/scripts/`
1. Om du vill ange en titel som identifierar skriptet i redigeringsdialogrutan **Processsteg** lägger du till följande egenskaper i skriptets `jcr:content`-nod:

   | Namn | Typ | Värde |
   |---|---|---|
   | `jcr:mixinTypes` | `Name[]` | `mix:title` |
   | `jcr:title` | `String` | Namnet som ska visas i redigeringsdialogrutan. |

1. Redigera instansen **Processsteg** och ange det skript som ska användas.

## Utveckla deltagarväljare {#developing-participant-choosers}

Du kan utveckla deltagarval för **komponenter för dynamiskt deltagarsteg**.

När en **komponent för dynamisk deltagare** startas under ett arbetsflöde, måste steget definiera deltagaren som det genererade arbetsobjektet kan tilldelas till. Så här gör du i det här steget:

* skickar en begäran till en OSGi-tjänst
* kör ett ECMA-skript för att välja deltagaren

Du kan utveckla en tjänst eller ett ECMA-skript som väljer deltagare enligt kraven i ditt arbetsflöde.

>[!NOTE]
>
>Mer information om hur du associerar din **Dynamic Participant Step**-komponent med tjänsten eller skriptet finns i [Dynamiskt deltagarsteg](/help/sites-developing/workflows-step-ref.md#dynamic-participant-step) eller [Åsidosätta stegimplementeringen](#persisting-and-accessing-data).

### Utveckla en deltagarväljare Använda en Java-klass {#developing-a-participant-chooser-using-a-java-class}

Så här definierar du ett deltagarsteg som en OSGI-tjänstkomponent (Java-klass):

1. OSGI-komponenten måste implementera gränssnittet `ParticipantStepChooser` med dess `getParticipant()`-metod. Se exempelkoden nedan.

   Skapa paketet och distribuera det i OSGI-behållaren.

1. Lägg till SCR-egenskapen `chooser.label` och ange värdet efter behov. Det här är namnet som deltagarväljaren visas med hjälp av komponenten **Dynamiskt deltagarsteg**. Se exemplet:

   ```java
   package com.adobe.example.workflow.impl.process;
   
   import com.adobe.granite.workflow.WorkflowException;
   import com.adobe.granite.workflow.WorkflowSession;
   import com.adobe.granite.workflow.exec.ParticipantStepChooser;
   import com.adobe.granite.workflow.exec.WorkItem;
   import com.adobe.granite.workflow.exec.WorkflowData;
   import com.adobe.granite.workflow.metadata.MetaDataMap;
   
   import org.apache.felix.scr.annotations.Component;
   import org.apache.felix.scr.annotations.Property;
   import org.apache.felix.scr.annotations.Service;
   
   import org.osgi.framework.Constants;
   
   /**
    * Sample dynamic participant step that determines the participant based on a path given as argument.
    */
   @Component
   @Service
   
   public class MyDynamicParticipant implements ParticipantStepChooser {
   
    @Property(value = "An example implementation of a dynamic participant chooser.")
    static final String DESCRIPTION = Constants.SERVICE_DESCRIPTION;
       @Property(value = "Adobe")
       static final String VENDOR = Constants.SERVICE_VENDOR;
       @Property(value = "Dynamic Participant Chooser Process")
       static final String LABEL=ParticipantStepChooser.SERVICE_PROPERTY_LABEL;
   
       private static final String TYPE_JCR_PATH = "JCR_PATH";
   
       public String getParticipant(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
           WorkflowData workflowData = workItem.getWorkflowData();
           if (workflowData.getPayloadType().equals(TYPE_JCR_PATH)) {
               String path = workflowData.getPayload().toString();
               String pathFromArgument = args.get("PROCESS_ARGS", String.class);
               if (pathFromArgument != null && path.startsWith(pathFromArgument)) {
                   return "admin";
               }
           }
           return "administrators";
       }
   }
   ```

1. Lägg till det dynamiska deltagarsteget i arbetsflödet med den generiska komponenten **Dynamiskt deltagarsteg** i redigeraren för **modeller** .
1. I redigeringsdialogrutan väljer du fliken **Deltagarväljaren** och väljer en implementering av väljaren.
1. Om du använder argument i koden anger du **Processargument**. I det här exemplet: `/content/we-retail/de`.
1. Spara ändringarna för både steget och arbetsflödesmodellen.

### Utveckla en deltagarväljare med ett ECMA-skript {#developing-a-participant-chooser-using-an-ecma-script}

Du kan skapa ett ECMA-skript som väljer användaren som är tilldelad arbetsposten som genereras av **Deltagarsteget**. Skriptet måste innehålla en funktion med namnet `getParticipant` som inte kräver några argument, och returnerar en `String` som innehåller ID:t för en användare eller grupp.

Skript finns i JCR-databasen och körs därifrån.

I följande tabell visas de variabler som ger omedelbar åtkomst till Java-objekt i arbetsflöden i dina skript.

| Java-klass | Skriptvariabelnamn |
|---|---|
| `com.adobe.granite.workflow.exec.WorkItem` | `graniteWorkItem` |
| `com.adobe.granite.workflow.WorkflowSession` | `graniteWorkflowSession` |
| `String[]` (innehåller processargument) | `args` |
| `com.adobe.granite.workflow.metadata.MetaDataMap` | `metaData` |
| `org.apache.sling.scripting.core.impl.InternalScriptHelper` | `sling` |

```
function getParticipant() {
    var workflowData = graniteWorkItem.getWorkflowData();
    if (workflowData.getPayloadType() == "JCR_PATH") {
        var path = workflowData.getPayload().toString();
        if (path.indexOf("/content/we-retail/de") == 0) {
            return "admin";
        } else {
            return "administrators";
        }
    }
}
```

1. Skapa skriptet (till exempel med CRXDE Lite) och spara det i databasen nedan `//apps/workflow/scripts`
1. Om du vill ange en titel som identifierar skriptet i redigeringsdialogrutan **Processsteg** lägger du till följande egenskaper i skriptets `jcr:content`-nod:

   | Namn | Typ | Värde |
   |---|---|---|
   | `jcr:mixinTypes` | `Name[]` | `mix:title` |
   | `jcr:title` | `String` | Namnet som ska visas i redigeringsdialogrutan. |

1. Redigera instansen [Dynamic Participant Step](/help/sites-developing/workflows-step-ref.md#dynamic-participant-step) och ange det skript som ska användas.

## Hantera arbetsflödespaket {#handling-workflow-packages}

[Arbetsflödespaket](/help/sites-authoring/workflows-applying.md#specifying-workflow-details-in-the-create-workflow-wizard) kan skickas till ett arbetsflöde för bearbetning. Arbetsflödespaket innehåller referenser till resurser som sidor och resurser.

>[!NOTE]
>
>Följande arbetsflödessteg accepterar arbetsflödespaket för aktivering av gruppsidor:
>
>* [`com.day.cq.wcm.workflow.process.ActivatePageProcess`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/workflow/process/ActivatePageProcess.html)
>* [`com.day.cq.wcm.workflow.process.DeactivatePageProcess`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/workflow/process/DeactivatePageProcess.html)
>

Du kan utveckla arbetsflödessteg som hämtar paketresurserna och bearbetar dem. Följande medlemmar i paketet `com.day.cq.workflow.collection` ger åtkomst till arbetsflödespaket:

* `ResourceCollection`: Paketklass för arbetsflöde.
* `ResourceCollectionUtil`: Används för att hämta ResourceCollection-objekt.
* `ResourceCollectionManager`: Skapar och hämtar samlingar. En implementering distribueras som en OSGi-tjänst.

I följande exempel visas hur Java-klassen hämtar paketresurser:

```java
package com.adobe.example;

import java.util.ArrayList;
import java.util.List;

import com.day.cq.workflow.WorkflowException;
import com.day.cq.workflow.WorkflowSession;
import com.day.cq.workflow.collection.ResourceCollection;
import com.day.cq.workflow.collection.ResourceCollectionManager;
import com.day.cq.workflow.collection.ResourceCollectionUtil;
import com.day.cq.workflow.exec.WorkItem;
import com.day.cq.workflow.exec.WorkflowData;
import com.day.cq.workflow.exec.WorkflowProcess;
import com.day.cq.workflow.metadata.MetaDataMap;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Property;
import org.apache.felix.scr.annotations.Service;
import org.apache.felix.scr.annotations.Reference;
import org.osgi.framework.Constants;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Node;
import javax.jcr.PathNotFoundException;
import javax.jcr.RepositoryException;
import javax.jcr.Session;

@Component
@Service
public class LaunchBulkActivate implements WorkflowProcess {

 private static final Logger log = LoggerFactory.getLogger(LaunchBulkActivate.class);

 @Property(value="Bulk Activate for Launches")
  static final String PROCESS_NAME ="process.label";
 @Property(value="A sample workflow process step to support Launches bulk activation of pages")
 static final String SERVICE_DESCRIPTION = Constants.SERVICE_DESCRIPTION;

 @Reference
 private ResourceCollectionManager rcManager;
public void execute(WorkItem workItem, WorkflowSession workflowSession) throws Exception {
    Session session = workflowSession.getSession();
    WorkflowData data = workItem.getWorkflowData();
    String path = null;
    String type = data.getPayloadType();
    if (type.equals(TYPE_JCR_PATH) && data.getPayload() != null) {
        String payloadData = (String) data.getPayload();
        if (session.itemExists(payloadData)) {
            path = payloadData;
        }
    } else if (data.getPayload() != null && type.equals(TYPE_JCR_UUID)) {
        Node node = session.getNodeByUUID((String) data.getPayload());
        path = node.getPath();
    }

    // CUSTOMIZED CODE IF REQUIRED....

    if (path != null) {
        // check for resource collection
        ResourceCollection rcCollection = ResourceCollectionUtil.getResourceCollection((Node)session.getItem(path), rcManager);
        // get list of paths to replicate (no resource collection: size == 1
        // otherwise size >= 1
        List<String> paths = getPaths(path, rcCollection);
        for (String aPath: paths) {

            // CUSTOMIZED CODE....

        }
    } else {
        log.warn("Cannot process because path is null for this " + "workitem: " + workItem.toString());
    }
}

/**
 * helper
 */
private List<String> getPaths(String path, ResourceCollection rcCollection) {
    List<String> paths = new ArrayList<String>();
    if (rcCollection == null) {
        paths.add(path);
    } else {
        log.debug("ResourceCollection detected " + rcCollection.getPath());
        // this is a resource collection. the collection itself is not
        // replicated. only its members
        try {
            List<Node> members = rcCollection.list(new String[]{"cq:Page", "dam:Asset"});
            for (Node member: members) {
                String mPath = member.getPath();
                paths.add(mPath);
            }
        } catch(RepositoryException re) {
            log.error("Cannot build path list out of the resource collection " + rcCollection.getPath());
        }
    }
    return paths;
}
}
```

## Exempel: Skapa ett anpassat steg {#example-creating-a-custom-step}

Ett enkelt sätt att börja skapa egna anpassade steg är att kopiera ett befintligt steg från:

`/libs/cq/workflow/components/model`

### Skapa det grundläggande steget {#creating-the-basic-step}

1. Återskapa sökvägen under /apps, till exempel:

   `/apps/cq/workflow/components/model`

   De nya mapparna är av typen `nt:folder`:

   ```xml
   - apps
     - cq
       - workflow (nt:folder)
         - components (nt:folder)
           - model (nt:folder)
   ```

   >[!NOTE]
   >
   >Det här steget gäller inte för den klassiska UI-modellredigeraren.

1. Placera sedan det kopierade steget i mappen /apps, till exempel:

   `/apps/cq/workflow/components/model/myCustomStep`

   Här är resultatet av vårt skräddarsydda steg:

   ![wf-34](assets/wf-34.png)

   >[!CAUTION]
   >
   >Eftersom bara rubriken och inte informationen inte visas på kortet i standardgränssnittet behövs inte `details.jsp` som den var för den klassiska gränssnittsredigeraren.

1. Använd följande egenskaper på noden:

   `/apps/cq/workflow/components/model/myCustomStep`

   **Intressegenskaper:**

   * `sling:resourceSuperType`

     Måste ärva från ett befintligt steg.

     I det här exemplet ärver vi från bassteget på `cq/workflow/components/model/step`, men du kan använda andra supertyper som `participant`, `process` och så vidare.

   * `jcr:title`

     Visar titeln när komponenten visas i stegwebbläsaren (den vänstra panelen i arbetsflödesmodellredigeraren).

   * `cq:icon`

     Används för att ange en [korallikon](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html) för steget.

   * `componentGroup`

     Måste vara något av följande:

      * Collaboration Workflow
      * DAM-arbetsflöde
      * Forms Workflow
      * Projekt
      * WCM-arbetsflöde
      * Arbetsflöde

   ![wf-35](assets/wf-35.png)

1. Nu kan du öppna en arbetsflödesmodell för redigering. I stegwebbläsaren kan du filtrera så att **Mina anpassade steg** visas:

   ![wf-36](assets/wf-36.png)

   Om du drar **Mitt anpassade steg** till modellen visas kortet:

   ![wf-37](assets/wf-37.png)

   Om ingen `cq:icon` har definierats för steget återges en standardikon med de två första bokstäverna i titeln. Till exempel:

   ![wf-38](assets/wf-38.png)

#### Definiera dialogrutan Konfigurera steg {#defining-the-step-configure-dialog}

När du har [skapat det grundläggande steget](#creating-the-basic-step) definierar du dialogrutan **Konfigurera** enligt följande:

1. Konfigurera egenskaperna på noden `cq:editConfig` enligt följande:

   **Intressegenskaper:**

   * `cq:inherit`

     Om du anger `true` ärver stegkomponenten egenskaperna från det steg du angav i `sling:resourceSuperType`.

   * `cq:disableTargeting`

     Ange efter behov.

   ![wf-39](assets/wf-39.png)

1. Konfigurera egenskaperna på noden `cq:formsParameter` enligt följande:

   **Intressegenskaper:**

   * `jcr:title`

     Anger standardtiteln på stegkortet i modellkartan och i fältet **Titel** i konfigurationsdialogrutan **Min egen - stegegenskaper** .

   * Du kan också definiera egna anpassade egenskaper.

   ![wf-40](assets/wf-40.png)

1. Konfigurera egenskaperna på noden `cq:listeners`.

   Med noden `cq:listener` och dess egenskaper kan du ange händelsehanterare som reagerar på händelser i den pekaktiverade UI-modellredigeraren, till exempel dra ett steg till en modellsida eller redigera en stegegenskap.

   **Intressegenskaper:**

   * `afterMove: REFRESH_PAGE`
   * `afterdelete: CQ.workflow.flow.Step.afterDelete`
   * `afteredit: CQ.workflow.flow.Step.afterEdit`
   * `afterinsert: CQ.workflow.flow.Step.afterInsert`

   Den här konfigurationen är viktig för att redigeraren ska fungera korrekt. I de flesta fall får den här konfigurationen inte ändras.

   Om du ställer in `cq:inherit` på true (på noden `cq:editConfig`, se ovan) kan du ärva den här konfigurationen, utan att behöva inkludera den explicit i stegdefinitionen. Om inget arv finns på plats måste du lägga till den här noden med följande egenskaper och värden.

   I det här exemplet har arv aktiverats så att vi kan ta bort noden `cq:listeners` och steget kommer fortfarande att fungera korrekt.

   ![wf-41](assets/wf-41.png)

1. Nu kan du lägga till en instans av steget i en arbetsflödesmodell. När du **konfigurerar** visas dialogrutan:

   ![wf-42](assets/wf-42.png) ![wf-43](assets/wf-43.png)

#### Exempelkod som används i det här exemplet {#sample-markup-used-in-this-example}

Markering för ett anpassat steg representeras i `.content.xml` för komponentens rotnod. Exemplet `.content.xml` som används i det här exemplet:

`/apps/cq/workflow/components/model/myCustomStep/.content.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:cq="https://www.day.com/jcr/cq/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
    cq:icon="bell"
    jcr:primaryType="cq:Component"
    jcr:title="My Custom Step"
    sling:resourceSuperType="cq/workflow/components/model/process"
    allowedParents="[*/parsys]"
    componentGroup="Workflow"/>
```

`_cq_editConfig.xml`-exemplet som används i det här exemplet:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="https://www.day.com/jcr/cq/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0" xmlns:nt="https://www.jcp.org/jcr/nt/1.0"
    cq:disableTargeting="{Boolean}true"
    cq:inherit="{Boolean}true"
    jcr:primaryType="cq:EditConfig">
    <cq:formParameters
        jcr:primaryType="nt:unstructured"
        jcr:title="My Custom Step Card"
        SAMPLE_PROPERY="sample value"/>
    <cq:listeners
        jcr:primaryType="cq:EditListenersConfig"
        afterdelete="CQ.workflow.flow.Step.afterDelete"
        afteredit="CQ.workflow.flow.Step.afterEdit"
        afterinsert="CQ.workflow.flow.Step.afterInsert"
        afterMove="REFRESH_PAGE"/>
</jcr:root>
```

`_cq_dialog/.content.xml`-exemplet som används i det här exemplet:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:cq="https://www.day.com/jcr/cq/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0" xmlns:nt="https://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"
    jcr:title="My Custom - Step Properties"
    sling:resourceType="cq/gui/components/authoring/dialog">
    <content
        jcr:primaryType="nt:unstructured"
        sling:resourceType="granite/ui/components/coral/foundation/tabs">
        <items jcr:primaryType="nt:unstructured">
            <common
                cq:hideOnEdit="true"
                jcr:primaryType="nt:unstructured"
                jcr:title="Common"
                sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"/>
            <process
                cq:hideOnEdit="true"
                jcr:primaryType="nt:unstructured"
                jcr:title="Process"
                sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"/>
            <mycommon
                jcr:primaryType="nt:unstructured"
                jcr:title="Common"
                sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
                <items jcr:primaryType="nt:unstructured">
                    <columns
                        jcr:primaryType="nt:unstructured"
                        sling:resourceType="granite/ui/components/coral/foundation/container">
                        <items jcr:primaryType="nt:unstructured">
                            <title
                                jcr:primaryType="nt:unstructured"
                                sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                fieldLabel="Title"
                                name="./jcr:title"/>
                            <description
                                jcr:primaryType="nt:unstructured"
                                sling:resourceType="granite/ui/components/coral/foundation/form/textarea"
                                fieldLabel="Description"
                                name="./jcr:description"/>
                        </items>
                    </columns>
                </items>
            </mycommon>
            <advanced
                jcr:primaryType="nt:unstructured"
                jcr:title="Advanced"
                sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
                <items jcr:primaryType="nt:unstructured">
                    <columns
                        jcr:primaryType="nt:unstructured"
                        sling:resourceType="granite/ui/components/coral/foundation/container">
                        <items jcr:primaryType="nt:unstructured">
                            <email
                                jcr:primaryType="nt:unstructured"
                                sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                fieldDescription="Notify user via email."
                                fieldLabel="Email"
                                name="./metaData/PROCESS_AUTO_ADVANCE"
                                text="Notify user via email."
                                value="true"/>
                        </items>
                    </columns>
                </items>
            </advanced>
        </items>
    </content>
</jcr:root>
```

>[!NOTE]
>
>Lägg märke till de gemensamma noderna och processnoderna i dialogdefinitionen. Dessa ärvs från det processteg som vi använde som supertyp för vårt anpassade steg:
>
>`sling:resourceSuperType : cq/workflow/components/model/process`

>[!NOTE]
>
>Klassiska dialogrutor för modellredigering i användargränssnittet fungerar fortfarande med den pekaktiverade standardredigeraren.
>
>AEM har [moderniseringsverktyg](/help/sites-developing/modernization-tools.md) om du vill uppgradera dina klassiska dialogrutor i användargränssnittet till standarddialogrutor i användargränssnittet. Efter konverteringen finns det fortfarande några manuella förbättringar som kan göras i dialogrutan för vissa fall.
>
>* Om en uppgraderad dialogruta är tom kan du titta på dialogrutor i `/libs` som har liknande funktionalitet som exempel på hur du tillhandahåller en lösning. Till exempel:
>
>* `/libs/cq/workflow/components/model`
>* `/libs/cq/workflow/components/workflow`
>* `/libs/dam/components`
>* `/libs/wcm/workflow/components/autoassign`
>* `/libs/cq/projects`
>
>  Redigera ingenting i `/libs`, använd dem bara som exempel. Om du vill använda något av de befintliga stegen kopierar du dem till `/apps` och redigerar dem där.
