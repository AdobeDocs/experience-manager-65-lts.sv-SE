---
title: Variabler i AEM Forms arbetsflöden
description: Skapa en variabel, ange ett värde för variabeln och använd den i AEM Forms Workflow steps.
contentOwner: khsingh
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 1a0d00f9-45f7-45af-ab34-d1c164980abb
source-git-commit: a869ffbc6015fd230285838d260434d9c0ffbcb0
workflow-type: tm+mt
source-wordcount: '2043'
ht-degree: 0%

---

# Variabler i AEM Forms arbetsflöden{#variables-in-aem-forms-workflows}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/create-form-centric-workflows/variable-in-aem-workflows.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

En variabel i en arbetsflödesmodell är ett sätt att lagra ett värde baserat på dess datatyp. Du kan sedan använda namnet på variabeln i vilket arbetsflödessteg som helst för att hämta värdet som lagras i variabeln. Du kan också använda variabelnamn för att definiera uttryck för att fatta beslut om routning.

I AEM arbetsflödesmodeller kan du

* [Skapa en variabel](../../forms/using/variable-in-aem-workflows.md#create-a-variable) för en datatyp baserat på den informationstyp som du vill lagra i den.
* [Ange ett värde för variabeln ](../../forms/using/variable-in-aem-workflows.md#set-a-variable) med hjälp av arbetsflödessteget Ange variabel.
* [Använd variabeln ](../../forms/using/variable-in-aem-workflows.md#use-a-variable) i alla AEM Forms-arbetsflödessteg för att hämta det lagrade värdet och i stegen ELLER Dela och Gå till för att definiera ett routningsuttryck.

Variabler är ett tillägg till det befintliga [MetaDataMap](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html) -gränssnittet. Du kan använda [MetaDataMap](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html) i ECMAScript för att komma åt metadata som sparats med variabler.

## Skapa en variabel {#create-a-variable}

Du skapar variabler med hjälp av avsnittet Variabler som är tillgängliga i arbetsflödesmodellens sidospak. AEM arbetsflödesvariabler har stöd för följande datatyper:

* **Primitiva datatyper**: Long, Double, Boolean, Date och String
* **Komplexa datatyper**: [Dokument](https://helpx.adobe.com/se/experience-manager/6-5/forms/javadocs/com/adobe/aemfd/docmanager/Document.html), [XML](https://docs.oracle.com/javase/8/docs/api/org/w3c/dom/Document.html), [JSON](https://static.javadoc.io/com.google.code.gson/gson/2.3/com/google/gson/JsonObject.html) och Form Data Model-instans.

>[!NOTE]
>
>Arbetsflöden har endast stöd för ISO8601-format för datatypsvariabler.

Du behöver [AEM Forms-tilläggspaket](https://helpx.adobe.com/se/aem-forms/kb/aem-forms-releases.html) för datatyperna Document och Form Data Model.  Använd datatypen ArrayList för att skapa variabelsamlingar. Du kan skapa en ArrayList-variabel för alla primitiva och komplexa datatyper. Skapa till exempel en ArrayList-variabel och välj String som undertyp för att lagra flera strängvärden med variabeln.

Så här skapar du en variabel:

1. I en AEM-instans går du till Verktyg ![Verktyg](/help/forms/using/assets/hammer.png) > Arbetsflöde > Modeller.
1. Välj **[!UICONTROL Create]** och ange titeln och ett valfritt namn för arbetsflödesmodellen. Markera modellen och välj **[!UICONTROL Edit]**.
1. Välj variabelikonen som är tillgänglig i sidosparken för arbetsflödesmodellen och välj **[!UICONTROL Add Variable]**.

   ![Lägg till variabel](assets/variables_add_variable_new.png)

1. I dialogrutan Lägg till variabel anger du namnet och väljer variabeltyp.
1. Välj datatypen i listrutan **[!UICONTROL Type]** och ange följande värden:

   * Primitiv datatyp - Ange ett valfritt standardvärde för variabeln.
   * JSON eller XML - Ange en JSON- eller XML-schemasökväg (tillval). Systemet validerar schemasökvägen när egenskaper som är tillgängliga i schemat mappas och lagras till en annan variabel.
   * Formulärdatamodell - Ange en formulärdatamodell.
   * ArrayList - Ange en undertyp för samlingen.

1. Ange en valfri beskrivning av variabeln och välj ![made_icon](assets/done_icon.png) för att spara ändringarna. Variabeln visas i listan som är tillgänglig i den vänstra rutan.

Tänk på följande när du skapar variabler:

* Skapa så många variabler som ett arbetsflöde kräver. Om du vill spara databasresurser bör du dock använda det minsta antalet variabler som krävs och återanvända variabler när det är möjligt.
* Variabler är skiftlägeskänsliga. Se till att du refererar till variabler med samma skiftläge i arbetsflödet.
* Undvik att använda specialtecken i namnet på variabeln

## Ange en variabel {#set-a-variable}

Du kan använda steget Ange variabel för att ange värdet för en variabel och definiera i vilken ordning värdena ska anges. Variabeln ställs in i den ordning som variabelmappningarna visas i steget för att ange variabel.

Ändringar av variabelvärden påverkar bara instansen av den process i vilken ändringen sker. När ett arbetsflöde initieras och variabeldata ändras påverkar ändringarna bara den instansen av arbetsflödet. Ändringarna påverkar inte andra instanser av arbetsflödet som har initierats tidigare eller som har initierats senare.

Beroende på variabelns datatyp kan du använda följande alternativ för att ange värdet för en variabel:

* **Literal:** Använd alternativet när du vet exakt vilket värde som ska anges.

* **Uttryck:** Använd alternativet när värdet som ska användas beräknas baserat på ett uttryck. Uttrycket skapas i angiven uttrycksredigerare.

* **JSON-punktnotation:** Använd alternativet för att hämta ett värde från en JSON- eller FDM-typvariabel.
* **XPATH:** Använd alternativet för att hämta ett värde från en XML-typvariabel.

* **Relativt till nyttolast:** Använd alternativet när värdet som ska sparas i variabeln är tillgängligt på en sökväg som är relativ till nyttolasten.

* **Absolut sökväg:** Använd alternativet när värdet som ska sparas i variabeln är tillgängligt på en absolut sökväg.

Du kan också uppdatera specifika element i en JSON- eller XML-typvariabel med JSON DOT Notation eller XPATH.

### Lägg till mappning mellan variabler {#add-mapping-between-variables}

Utför följande steg för att lägga till mappning mellan variabler:

1. På arbetsflödets redigeringssida väljer du ikonen Steg som är tillgänglig i arbetsflödesmodellens sidospark.
1. Dra och släpp steget **Ange variabel** till arbetsflödesredigeraren, markera steget och välj ![configure_icon](assets/configure_icon.png) (Konfigurera).
1. I dialogrutan Ange variabel väljer du **[!UICONTROL Mapping]** > **[!UICONTROL Add Mapping]**.
1. I avsnittet **Mappningsvariabel** markerar du variabeln som data ska lagras i, väljer mappningsläget och anger ett värde som ska lagras i variabeln. Mappningslägena varierar beroende på variabeltypen.
1. Mappa fler variabler för att skapa ett meningsfullt uttryck. Välj ![made_icon](assets/done_icon.png) om du vill spara ändringarna.

### Exempel 1: Fråga en XML-variabel för att ange ett värde för en strängvariabel {#example-query-an-xml-variable-to-set-value-for-a-string-variable}

Välj en variabel av XML-typ för att lagra en XML-fil. Fråga XML-variabeln för att ange värdet för en strängvariabel för egenskapen som är tillgänglig i XML-filen. Använd **Ange XPATH för fältet XML-variabel** för att definiera den egenskap som ska lagras i strängvariabeln.

I det här exemplet väljer du en **formdata** XML-variabel som ska lagra filen **cc-app.xml**. Fråga variabeln **formdata** om du vill ange värdet för strängvariabeln **emailaddress** för att lagra värdet för egenskapen **emailAddress** som finns i filen **cc-app.xml** .

### Exempel 2: Använd ett uttryck för att lagra värden baserat på andra variabler {#example2}

Använd ett uttryck för att beräkna summan av variablerna och lagra resultatet i en variabel.

I det här exemplet använder du uttrycksredigeraren för att definiera ett uttryck för att beräkna summan av variablerna **assetsCost** och **balanceamount** och lagra resultatet i variabeln **totalValue** .

## Använda uttrycksredigeraren {#use-expression-editor}

Du använder också uttryck för att beräkna värdet för en variabel i körningsmiljön. Variabler tillhandahåller en uttrycksredigerare för att definiera uttryck.

Använd uttrycksredigeraren för att:

* Ange värdet för variabler med andra arbetsflödesvariabler, siffror eller matematiska uttryck.
* Använd arbetsflödesvariabler, sträng, tal eller ett uttryck i ett matematiskt uttryck
* Lägg till villkor för att ange värden för variabler.
* Lägg till operatorer mellan villkor.

![Uttrycksredigeraren](assets/variables_expression_editor_new.png)

Det baseras på redigering av adaptiva formulärregler med följande ändringar. Regelredigerare i variabler:

* Har inte stöd för funktioner.
* Inget användargränssnitt för att visa en sammanfattning av regler
* Har ingen kodredigerare.
* Stöder inte aktivering och inaktivering av ett objekts värde.
* Stöder inte inställning av ett objekts egenskap.
* Stöder inte anrop till en webbtjänst.

Mer information finns i [Regelredigeraren för anpassade formulär](../../forms/using/rule-editor.md).

## Använda en variabel {#use-a-variable}

Du kan använda variabler för att hämta indata och utdata eller spara resultatet av ett steg. Arbetsflödesredigeraren innehåller två typer av arbetsflödessteg:

* Arbetsflödessteg med stöd för variabler
* Arbetsflödessteg utan stöd för variabler

### Arbetsflödessteg med stöd för variabler {#workflow-steps-with-support-for-variables}

Steget Gå till, eller Dela, och alla AEM Forms Workflow-steg har stöd för variabler.

#### ELLER Dela upp steg {#or-split-step}

Med ELLER-delning skapas en delning i arbetsflödet, varefter endast en gren är aktiv. I det här steget kan du lägga in sökvägar för villkorlig bearbetning i arbetsflödet. Du kan lägga till arbetsflödessteg i varje gren efter behov.

Du kan definiera routningsuttryck för en gren med en regeldefinition, ett ECMA-skript eller ett externt skript.

Du kan använda variabler för att definiera routningsuttrycket med hjälp av uttrycksredigeraren. Mer information om hur du använder routningsuttryck för steget ELLER Dela finns i [ELLER Dela steg](/help/sites-developing/workflows-step-ref.md#or-split).

I det här exemplet använder du [example 2](../../forms/using/variable-in-aem-workflows.md#example2) för att ange värdet för variabeln **totalvalue** innan du definierar routningsuttrycket. Gren 1 är aktiv om värdet för variabeln **totalvalue** är större än 50000. På samma sätt kan du definiera en regel som gör grenen 2 aktiv om värdet för variabeln **totalvalue** är mindre än 50000.

Välj på liknande sätt en extern skriptsökväg eller ange ECMA-skriptet för routningsuttryck för att utvärdera den aktiva grenen. Välj **[!UICONTROL Rename Branch]** om du vill ange ett alternativt namn för grenen.

Mer exempel finns i [Skapa en arbetsflödesmodell](../../forms/using/aem-forms-workflow.md#create-a-workflow-model).

#### Gå till steg {#go-to-step}

Med **Gå till steg** kan du ange nästa steg i arbetsflödesmodellen som ska köras, beroende på resultatet av ett routningsuttryck.

Ungefär som i steget ELLER Dela kan du definiera routningsuttryck för Gå till med hjälp av en regeldefinition, ett ECMA-skript eller ett externt skript.

Du kan använda variabler för att definiera routningsuttrycket med hjälp av uttrycksredigeraren. Mer information om hur du använder routningsuttryck för steget Gå till finns i [Gå till steg](/help/sites-developing/workflows-step-ref.md#goto-step).

![Gå till regel](assets/variables_goto_rule1_new.png)

I det här exemplet anger Gå till-steget Granska kreditkortsansökan som nästa steg om värdet för variabeln **actiontaken** är lika med **Behöver mer information**.

Mer exempel på hur du använder regeldefinitionen i steget Gå till finns i [Simulera en For-slinga](/help/sites-developing/workflows-step-ref.md#simulateforloop).

#### Centrala arbetsflödessteg för Forms {#forms-workflow-centric-workflow-steps}

Alla AEM Forms Workflow-steg har stöd för variabler. Mer information finns i [Forms-centrerat arbetsflöde i OSGi](../../forms/using/aem-forms-workflow-step-reference.md).

### Arbetsflödessteg utan stöd för variabler {#workflow-steps-without-support-for-variables}

Du kan använda gränssnittet [MetaDataMap](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html) för att komma åt variabler i arbetsflödessteg som inte stöder variabler.

#### Hämta variabelvärdet {#retrieve-the-variable-value}

Använd följande API:er i ECMA-skriptet för att hämta värden för befintliga variabler baserat på datatypen:

| Variabeldatatyp | API |
|---|---|
| Primitiv (long, Double, Boolean, Date och String) | workItem.getWorkflowData().getMetaDataMap().get(variableName, type) |
| Dokument | Packages.com.adobe.aemfd.docmanager.Document doc = workItem.getWorkflowData().getMetaDataMap(&quot;docVar&quot;, Packages.com.adobe.aemfd.docmanager.Document.class); |
| XML | Packages.org.w3c.dom.Document xmlObject = workItem.getWorkflowData().getMetaDataMap().get(variableName, Packages.org.w3c.dom.Document.class); |
| Formulärdatamodell | Packages.com.adobe.aem.dermis.api.FormDataModelInstance fdmObject = workItem.getWorkflowData().getMetaDataMap().get(variableName, Packages.com.adobe.aem.dermis.api.FormDataModelInstance.class); |
| JSON | Packages.com.google.gson.JsonObject jsonObject = workItem.getWorkflowData().getMetaDataMap().get(variableName, Packages.com.google.gson.JsonObject.class); |

Du behöver [AEM Forms-tilläggspaket](https://helpx.adobe.com/se/aem-forms/kb/aem-forms-releases.html) för datatyperna Document och Form Data Model.

**Exempel**

Hämta värdet för strängdatatypen med följande API:

```javascript
workItem.getWorkflowData().getMetaDataMap().get(accname, Packages.java.lang.String)
```

#### Uppdatera variabelvärdet {#update-the-variable-value}

Använd följande API i ECMA-skriptet för att uppdatera värdet för en variabel:

```javascript
workItem.getWorkflowData().getMetaDataMap().put(variableName, value)
```

**Exempel**

```javascript
workItem.getWorkflowData().getMetaDataMap().put(salary, 50000)
```

uppdaterar värdet för variabeln **Använd** på 50000.

### Ange variabler för att starta arbetsflöden {#apiinvokeworkflow}

Du kan använda ett API för att ange variabler och skicka dem för att anropa arbetsflödesinstanser.

[workflowSession.startWorkflow](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/adobe/granite/workflow/WorkflowSession.html#startWorkflow-com.adobe.granite.workflow.model.WorkflowModel-com.adobe.granite.workflow.exec.WorkflowData-java.util.Map-) använder modell, wfData och metaData som argument. Använd MetaDataMap för att ange värdet för variabeln.

I detta API är variabeln **variableName** inställd på **value** med metaData.put(variableName, value);

```javascript
import com.adobe.granite.workflow.model.WorkflowModel;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.aemfd.docmanager.Document;

/*Assume that you already have a workflowSession and modelId along with the payloadType and payload*/
WorkflowData wfData = workflowSession.newWorkflowData(payloadType, payload);
MetaDataMap metaData = wfData.getMetaDataMap();
metaData.put(variableName, value); //Create a variable "variableName" in your workflow model
WorkflowModel model = workflowSession.getModel(modelId);
workflowSession.startWorkflow(model, wfData, metaData);
```

**Exempel**

Initiera dokumentobjektet **doc** till en sökväg (&quot;a/b/c&quot;) och ange värdet för variabeln **docVar** till sökvägen som lagras i dokumentobjektet.

```javascript
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.aemfd.docmanager.Document;

/*This example assumes that you already have a workflowSession and modelId along with the payloadType and payload */
WorkflowData wfData = workflowSession.newWorkflowData(payloadType, payload);
MetaDataMap metaData = wfData.getMetaDataMap();
Document doc = new Document("/a/b/c");// initialize a document object
metaData.put("docVar",doc); //Assuming that you have created a variable "docVar" of type Document in your workflow model
WorkflowModel model = workflowSession.getModel(modelId);
workflowSession.startWorkflow(model, wfData, metaData);
```

### Lagra känsliga användardata utanför JCR med arbetsflödesvariabler {#jcr-independent-persistance}

Data som behandlas med Forms Workflow kan innehålla känsliga användardata, t.ex. personligt identifierbar information och känslig personlig information. Företag kan välja att lagra data, som bearbetas i olika arbetsflödessteg (och skickas med arbetsflödesvariabler), utanför JCR-lagring i ett externt datalager som ägs och hanteras av dem. Mer information om beständiga arbetsflödesdata i ett externt lagringsutrymme finns i [Använda arbetsflödesvariabler för kundägda datalager](/help/sites-administering/workflows-administering.md#using-workflow-variables-customer-datastore).
[!DNL Adobe Experience Manager] innehåller arbetsflödes-API:t [UserMetaDataPersistenceProvider](https://github.com/adobe/workflow-variable-externalizer) för lagring av arbetsflödesvariabler i externa Azure-blobblagringsplatser. Mer information om hur du använder API finns i [Använda arbetsflödesvariabler för att parametrisera känsliga data och lagra i externa datalager](/help/forms/using/aem-forms-workflow.md#externalize-wf-variables).

## Redigera en variabel {#edit-a-variable}

1. På sidan Redigera arbetsflöde väljer du ikonen Variabler som är tillgänglig i arbetsflödesmodellens sidokikon. I avsnittet Variabler i den vänstra rutan visas alla befintliga variabler.
1. Markera ikonen ![redigera](assets/edit.png) (redigera) bredvid variabelnamnet som du vill redigera.
1. Redigera variabelinformationen och välj ![made_icon](assets/done_icon.png) för att spara ändringarna. Du kan inte redigera fälten **[!UICONTROL Name]** och **[!UICONTROL Type]** för en variabel.

## Ta bort en variabel {#delete-a-variable}

Ta bort alla referenser till variabeln från arbetsflödet innan du tar bort variabeln. Kontrollera att variabeln inte används i arbetsflödet.

Så här tar du bort en variabel:

1. På sidan Redigera arbetsflöde väljer du ikonen Variabler som är tillgänglig i arbetsflödesmodellens sidokikon. I avsnittet Variabler i den vänstra rutan visas alla befintliga variabler.
1. Välj ikonen Ta bort bredvid variabelnamnet som du vill ta bort.
1. Välj ![made_icon](assets/done_icon.png) för att bekräfta och ta bort variabeln.

## Referenser {#references}

Mer exempel på hur du använder variabler i AEM Forms Workflow steps finns i [Variabler i AEM-arbetsflöden](https://helpx.adobe.com/experience-manager/kt/forms/using/authoring_variables_in_aem_forms-workflow1.html).
