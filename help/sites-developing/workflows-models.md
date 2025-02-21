---
title: Skapa arbetsflödesmodeller
description: Du skapar en arbetsflödesmodell för att definiera de steg som ska köras när en användare startar arbetsflödet.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2425'
ht-degree: 0%

---

# Skapa arbetsflödesmodeller{#creating-workflow-models}

>[!CAUTION]
>
>Information om hur du använder det klassiska användargränssnittet finns i [AEM 6.3-dokumentationen](https://helpx.adobe.com/experience-manager/6-3/help/sites-developing/workflows-models.html).

Du skapar en [arbetsflödesmodell](/help/sites-developing/workflows.md#model) för att definiera serien med steg som körs när en användare startar arbetsflödet. Du kan också definiera modellegenskaper, t.ex. om arbetsflödet är tillfälligt eller använder flera resurser.

När en användare startar ett arbetsflöde startas en instans. Det här är motsvarande körningsmodell som skapades när du [Synkroniserar](#sync-your-workflow-generate-a-runtime-model) dina ändringar.

## Skapa ett nytt arbetsflöde {#creating-a-new-workflow}

När du först skapar en arbetsflödesmodell innehåller den:

* Stegen **Flödesstart** och **Flödesslut**.
Dessa representerar början och slutet av arbetsflödet. Dessa steg är obligatoriska och kan inte redigeras/tas bort.
* Ett exempel på **Deltagare**-steg med namnet **Steg 1**.
Det här steget är konfigurerat för att tilldela en arbetsuppgift till arbetsflödesinitieraren. Redigera eller ta bort det här steget och lägg till steg efter behov.

Så här skapar du ett arbetsflöde med redigeraren:

1. Öppna konsolen **Arbetsflödesmodeller**, via **Verktyg**, **Arbetsflöde**, **Modeller** eller, till exempel: [https://localhost:4502/aem/workflow](https://localhost:4502/aem/workflow)
1. Välj **Skapa** och sedan **Skapa modell**.
1. Dialogrutan **Lägg till arbetsflödesmodell** visas. Ange **Titel** och **Namn** (valfritt) innan du väljer **Klar**.
1. Den nya modellen visas i konsolen **Arbetsflödesmodeller**.
1. Välj ditt nya arbetsflöde och använd sedan [**Redigera** för att öppna det för konfiguration](#editinganexistingworkflow):
   ![wf-01](assets/wf-01.png)

>[!NOTE]
>
>Om du skapar modeller programmatiskt (med ett crx-paket) kan du även skapa en undermapp i:
>
>`/var/workflow/models`
>
>Exempel: `/var/workflow/models/prototypes`
>
>Den här mappen kan sedan användas för [att hantera åtkomst till modellerna i den mappen](/help/sites-administering/workflows-managing.md#create-a-subfolder-in-var-workflow-models-and-apply-the-acl-to-that).

## Redigera ett arbetsflöde {#editing-a-workflow}

Du kan redigera alla befintliga arbetsflödesmodeller till:

* [definiera steg](#addingasteptoamodel-) och deras [parametrar](#configuring-a-workflow-step)
* konfigurera arbetsflödesegenskaper, inklusive [faser](#configuring-workflow-stages-that-show-workflow-progress), [om arbetsflödet är tillfälligt](#creatingatransientworkflow-) och/eller [använder flera resurser](#configuring-a-workflow-for-multi-resource-support)

Om du redigerar ett [**standardarbetsflöde och/eller äldre arbetsflöde** (ej ikryssat)](#editing-a-default-or-legacy-workflow-for-the-first-time) får du ett ytterligare steg som säkerställer att en [säker kopia](/help/sites-developing/workflows-best-practices.md#locations-workflow-models) utförs innan ändringarna görs.

När uppdateringarna av arbetsflödet är klara måste du använda **Synkronisera** för att **generera en körningsmodell**. Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

### Synkronisera arbetsflödet - Skapa en körningsmodell {#sync-your-workflow-generate-a-runtime-model}

**Synkronisering** (direkt i redigeringsverktygsfältet) genererar en [körningsmodell](/help/sites-developing/workflows.md#runtime-model). Körningsmodellen är den modell som faktiskt används när en användare startar ett arbetsflöde. Om du inte **Synkroniserar** dina ändringar är ändringarna inte tillgängliga vid körning.

När du (eller någon annan användare) gör några ändringar i arbetsflödet måste du använda **Synkronisera** för att generera en körningsmodell, även när enskilda dialogrutor (till exempel för steg) har egna sparalternativ.

När ändringarna synkroniseras med körningsmodellen (sparad) visas **Synkroniserad** i stället.

Vissa steg har obligatoriska fält och/eller inbyggd validering. När dessa villkor inte uppfylls visas ett fel när du försöker **Synkronisera** modellen. Om ingen deltagare har definierats för ett **Deltagare** -steg:

![wf-21](assets/wf-21.png)

### Redigera ett standardarbetsflöde eller äldre arbetsflöde för första gången {#editing-a-default-or-legacy-workflow-for-the-first-time}

När du öppnar en [standardmodell och/eller äldre modell](/help/sites-developing/workflows.md#workflow-types) för redigering:

* Stegwebbläsaren är inte tillgänglig (vänster sida).
* Det finns en **Redigera**-åtgärd tillgänglig i verktygsfältet (höger sida).
* Till att börja med visas modellen och dess egenskaper i skrivskyddat läge som:
   * Standardarbetsflöden finns i `/libs`
   * Äldre arbetsflöden finns i `/etc`
Om du väljer **Redigera** kommer du att:
* ta en kopia av arbetsflödet till `/conf`
* göra stegwebbläsaren tillgänglig
* gör att du kan göra ändringar

>[!NOTE]
>
>Mer information finns i [Platser för arbetsflödesmodeller](/help/sites-developing/workflows-best-practices.md#locations-workflow-models).

![wf-22](assets/wf-22.png)

### Lägga till ett steg i en modell {#adding-a-step-to-a-model}

Lägg till steg i modellen för att representera aktiviteten som ska utföras - varje steg utför en specifik aktivitet. Ett urval stegkomponenter är tillgängliga i en standardinstans av AEM.

När du redigerar en modell visas de tillgängliga stegen i de olika grupperna i **stegwebbläsaren**. Till exempel:

![wf-10](assets/wf-10.png)

>[!NOTE]
>
>Mer information om de primära stegkomponenterna som installeras med AEM finns i [Referens för arbetsflödessteg](/help/sites-developing/workflows-step-ref.md).

Så här lägger du till steg i arbetsflödesmodellen:

1. Öppna en befintlig arbetsflödesmodell för redigering. Välj önskad modell från **arbetsflödesmodellkonsolen** och sedan **Redigera**.
1. Öppna stegwebbläsaren och använd **växlingspanelen** längst till vänster i det övre verktygsfältet. Här kan du:

   * **Filtrera** för specifika steg.
   * Använd listruteväljaren för att begränsa markeringen till en viss grupp steg.
   * Välj ikonen Visa beskrivning ![wf-stepinfo-icon](assets/wf-stepinfo-icon.png) om du vill visa mer information om det aktuella steget.

   ![wf-02](assets/wf-02.png)

1. Dra lämpliga steg till önskad plats i modellen.

   Exempel: ett **deltagarsteg**.

   När du har lagt till i flödet kan du [konfigurera steget](#configuring-a-workflow-step).

   ![wf-03](assets/wf-03.png)

1. Lägg till så många steg eller andra uppdateringar som behövs.

   Vid körning utförs stegen i den ordning som de visas i modellen. När du har lagt till stegkomponenter kan du dra dem till en annan plats i modellen.

   Du kan också kopiera, klippa ut, klistra in, gruppera eller ta bort befintliga steg, som med [sidredigeraren.](/help/sites-authoring/editing-content.md)

   Delade steg kan också komprimeras/expanderas med verktygsfältsalternativet: ![wf-collap-expand-toolbar-icon](assets/wf-collapseexpand-toolbar-icon.png)

1. Bekräfta ändringarna med **Synkronisera** (redigeringsverktygsfältet) för att generera körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

### Konfigurera ett arbetsflödessteg {#configuring-a-workflow-step}

Du kan **konfigurera** och anpassa beteendet för ett arbetsflödessteg med hjälp av dialogrutorna **Stegegenskaper** .

1. Så här öppnar du dialogrutan **Stegegenskaper** för ett steg:

   * Klicka på steget* *i arbetsflödesmodellen och välj **Konfigurera** i komponentverktygsfältet.

   * Dubbelklicka på steget.

   >[!NOTE]
   >
   >Mer information om de primära stegkomponenterna som installeras med AEM finns i [Referens för arbetsflödessteg](/help/sites-developing/workflows-step-ref.md).

1. Konfigurera **stegegenskaperna** efter behov. Vilka egenskaper som är tillgängliga beror på stegtypen. Det kan också finnas flera tillgängliga flikar. Standardsteget **Deltagare**, som finns i ett nytt arbetsflöde, är till exempel `Step 1`:

   ![wf-11](assets/wf-11.png)

1. Bekräfta uppdateringarna.
1. Bekräfta ändringarna med **Synkronisera** (redigeringsverktygsfältet) för att generera körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

### Skapa ett tillfälligt arbetsflöde {#creating-a-transient-workflow}

Du kan skapa en [tillfällig](/help/sites-developing/workflows.md#transient-workflows) arbetsflödesmodell när du skapar en modell, eller genom att redigera en befintlig:

1. Öppna arbetsflödesmodellen för [redigering](#editinganexistingworkflow).
1. Välj **Egenskaper för arbetsflödesmodell** i verktygsfältet.
1. Aktivera **Transient Workflow** (eller inaktivera om det behövs) i dialogrutan:

   ![wf-07](assets/wf-07.png)

1. Bekräfta ändringen med **Spara och stäng**; följt av **Synkronisera** (redigeringsverktygsfältet) för att generera körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

>[!NOTE]
>
>När du kör ett arbetsflöde i läget [transient](/help/sites-developing/workflows.md#transient-workflows) sparas ingen arbetsflödeshistorik i AEM. Därför visar [Tidslinjen](/help/sites-authoring/basic-handling.md#timeline) ingen information som är relaterad till det arbetsflödet.

## Göra arbetsflödesmodeller tillgängliga i Touch UI {#classic2touchui}

Om det finns en arbetsflödesmodell i det klassiska användargränssnittet, men den saknas på snabbmenyn för val i **[!UICONTROL Timeline]**-listen i Touch UI, följer du konfigurationen för att göra den tillgänglig. Följande steg visar hur du använder arbetsflödesmodellen **[!UICONTROL Request for Activation]**.

1. Bekräfta att modellen inte är tillgänglig i det beröringsaktiverade användargränssnittet. Åtkomst till en resurs med sökvägen `/assets.html/content/dam`. Välj en resurs. Öppna **[!UICONTROL Timeline]** i den vänstra listen. Klicka på **[!UICONTROL Start Workflow]** och bekräfta att modellen **[!UICONTROL Request for Activation]** inte finns i popup-listan.

1. Navigera genom **[!UICONTROL Tools > General > Tagging]**. Välj **[!UICONTROL Workflow]**.

1. Välj **[!UICONTROL Create > Create Tag]**. Ange **[!UICONTROL Title]** som `DAM` och **[!UICONTROL Name]** som `dam`. Välj **[!UICONTROL Submit]**.
   ![Skapa tagg i arbetsflödesmodell](assets/workflow_create_tag.png)

1. Navigera till **[!UICONTROL Tools > Workflow > Models]**. Välj **[!UICONTROL Request for Activation]** och sedan **[!UICONTROL Edit]**.

1. Välj **[!UICONTROL Edit]**, öppna menyn **[!UICONTROL Page Information]** och gå sedan till fliken **[!UICONTROL Open Properties]** (om den inte redan är öppen).**[!UICONTROL Basic]**

1. Lägg till `Workflow : DAM` i fältet **[!UICONTROL Tags]**. Bekräfta markeringen med bocken.

1. Bekräfta tillägget av taggen med **[!UICONTROL Save & Close]**.
   ![Redigera sidegenskaper för modellen](assets/workflow_model_edit_activation1.png)

1. Slutför processen med **[!UICONTROL Sync]**. Arbetsflödet är nu tillgängligt i det Touch-aktiverade gränssnittet.

### Konfigurera ett arbetsflöde för stöd för flera resurser {#configuring-a-workflow-for-multi-resource-support}

Du kan konfigurera en arbetsflödesmodell för [Multi Resource Support](/help/sites-developing/workflows.md#multi-resource-support) när du skapar en modell, eller genom att redigera en befintlig:

1. Öppna arbetsflödesmodellen för [redigering](#editinganexistingworkflow).
1. Välj **Egenskaper för arbetsflödesmodell** i verktygsfältet.

1. Aktivera **Multi Resource Support** (eller inaktivera vid behov) i dialogrutan:

   ![wf-08](assets/wf-08.png)

1. Bekräfta ändringen med **Spara och stäng**; följt av **Synkronisera** (redigeringsverktygsfältet) för att generera körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

### Konfigurera arbetsflödessteg (som visar förlopp för arbetsflöde) {#configuring-workflow-stages-that-show-workflow-progress}

[Arbetsflödesfaser](/help/sites-developing/workflows.md#workflow-stages) hjälper dig att visualisera förloppet för ett arbetsflöde när du hanterar uppgifter.

>[!CAUTION]
>
>Om arbetsflödesfaser definieras i **Sidegenskaper**, men inte används för något av arbetsflödesstegen, visas inga förlopp i förloppsindikatorn (oavsett aktuellt arbetsflödessteg).

De steg som ska vara tillgängliga definieras i arbetsflödesmodellerna. Befintliga arbetsflödesmodeller kan uppdateras för att inkludera scendefinitioner. Du kan definiera valfritt antal steg för arbetsflödesmodellen.

Så här definierar du **faser** för ditt arbetsflöde:

1. Öppna arbetsflödesmodellen för redigering.
1. Välj **Egenskaper för arbetsflödesmodell** i verktygsfältet. Öppna sedan fliken **Steg**.
1. Lägg till (och placera) de **steg** som krävs. Du kan definiera valfritt antal steg för arbetsflödesmodellen.

   Till exempel:

   ![wf-08-1](assets/wf-08-1.png)

1. Klicka på **Spara och stäng** för att spara egenskaperna.
1. Tilldela en fas till varje steg i arbetsflödesmodellen. Till exempel:

   ![wf-09](assets/wf-09.png)

   En scen kan tilldelas till mer än ett steg. Till exempel:

   | **Steg** | **Scen** |
   |---|---|
   | Steg 1 | Skapa |
   | Steg 2 | Skapa |
   | Steg 3 | Granska |
   | Steg 4 | Godkänn |
   | Steg 5 | Godkänn |
   | Steg 6 | Complete |

1. Bekräfta ändringarna med **Synkronisera** (redigeringsverktygsfältet) för att generera körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

## Exportera en arbetsflödesmodell i ett paket {#exporting-a-workflow-model-in-a-package}

Så här exporterar du en arbetsflödesmodell i ett paket:

1. Skapa ett paket med hjälp av [pakethanteraren](/help/sites-administering/package-manager.md#package-manager):

   1. Navigera till pakethanteraren via **Verktyg**, **Distribution**, **Paket**.

   1. Klicka på **Skapa paket**.
   1. Ange **paketnamnet** och annan information efter behov.
   1. Klicka på **OK**.

1. Klicka på **Redigera** i verktygsfältet för det nya paketet.

1. Öppna fliken **Filter**.

1. Välj **Lägg till filter** och ange sökvägen till arbetsflödesmodellen *design*:

   `/conf/global/settings/workflow/models/<*your-model-name*>`

   Klicka på **Klar**.

1. Välj **Lägg till filter** och ange sökvägen till arbetsflödesmodellen för *miljön*:

   `/var/workflow/models/<*your-model-name*>`

   Klicka på **Klar**.

1. Lägg till ytterligare filter för anpassade skript som används av modellen.
1. Klicka på **Spara** för att bekräfta filterdefinitionerna.
1. Välj **Build** i verktygsfältet för paketdefinitionen.
1. Välj **Hämta** i paketets verktygsfält.

## Använda arbetsflöden för att bearbeta inskickade formulär {#using-workflows-to-process-form-submissions}

Du kan konfigurera ett formulär som ska bearbetas av det valda arbetsflödet. När användare skickar formuläret skapas en ny arbetsflödesinstans med data från formuläröverföringen som nyttolast.

Så här konfigurerar du arbetsflödet som ska användas med formuläret:

1. Skapa en sida och öppna den för redigering.
1. Lägg till en **Form**-komponent på sidan.
1. **Konfigurera** komponenten **Formulärstart** som visades på sidan.
1. Använd **Starta arbetsflöde** för att välja önskat arbetsflöde bland de tillgängliga:

   ![wf-12](assets/wf-12.png)

1. Bekräfta den nya formulärkonfigurationen med krysset.

## Testa arbetsflöden {#testing-workflows}

Det är en god vana att testa ett arbetsflöde för att använda olika typer av nyttolast, inklusive typer som skiljer sig från den som arbetsflödet har utvecklats för. Om du t.ex. vill att ditt arbetsflöde ska hantera Assets testar du det genom att ange en sida som nyttolast och se till att den inte orsakar fel.

Testa till exempel ditt nya arbetsflöde på följande sätt:

1. [Starta arbetsflödesmodellen](/help/sites-administering/workflows-starting.md) från konsolen.
1. Definiera **nyttolasten** och bekräfta.

1. Utför de åtgärder som behövs så att arbetsflödet fortsätter.
1. Övervaka loggfilerna medan arbetsflödet körs.

Du kan också konfigurera AEM så att **DEBUG** -meddelanden visas i loggfilerna. Mer information finns i [Loggning](/help/sites-deploying/configure-logging.md) och när utvecklingen är klar anger du **loggnivån** till **Info** igen.

## Exempel {#examples}

### Exempel: Skapa ett (enkelt) arbetsflöde för att acceptera eller avvisa en begäran om publicering {#example-creating-a-simple-workflow-to-accept-or-reject-a-request-for-publication}

För att illustrera några av möjligheterna att skapa ett arbetsflöde skapar följande exempel en variant av arbetsflödet `Publish Example`.

1. [Skapa en arbetsflödesmodell](#creating-a-new-workflow).

   Det nya arbetsflödet kommer att innehålla:

   * **Flödesstart**
   * `Step 1`
   * **Flödesslut**

1. Ta bort `Step 1` (eftersom det är fel stegtyp för det här exemplet):

   * Klicka på steget och välj **Ta bort** i komponentens verktygsfält. Bekräfta åtgärden.

1. Dra ett **Deltagarsteg** från markeringen **Arbetsflöde** i stegwebbläsaren till arbetsflödet och placera det mellan **Flödesstart** och **Flödesslut**.
1. Så här öppnar du egenskapsdialogrutan:

   * Klicka på deltagarsteget och välj **Konfigurera** i komponentens verktygsfält.
   * Dubbelklicka på deltagarsteget.

1. På fliken **Allmänt** anger du `Validate Content` för både **Titel** och **Beskrivning**.
1. Öppna fliken **Användare/grupp**:

   * Aktivera **Meddela användare via e-post**.
   * Välj `Administrator` ( `admin`) för fältet **Användare/grupp**.

   >[!NOTE]
   >
   >[E-posttjänsten och användarkontoinformationen måste konfigureras](/help/sites-administering/notification.md) för att e-postmeddelanden ska kunna skickas.

1. Bekräfta uppdateringarna med en bock.

   Du återgår till översikten över arbetsflödesmodellen där deltagarsteget har bytt namn till `Validate Content`.

1. Dra en **eller delad** till arbetsflödet och placera den mellan `Validate Content` och **Flödesslut**.
1. Öppna **eller dela** för konfiguration.
1. Konfigurera:

   * **Allmänt**: ange delningsnamnet.
   * **Förgrening 1**: välj **Standardflöde**.

   * **Förgrening 2**: kontrollera att **Standardflöde** inte är markerat.

1. Bekräfta dina uppdateringar av **ELLER Dela**.
1. Dra ett **deltagarsteg** till den vänstra grenen, öppna egenskaperna, ange följande värden och bekräfta ändringarna:

   * **Titel**: `Reject Publish Request`

   * **Användare/grupp**: till exempel `projects-administrators`

   * **Meddela användaren via e-post**: Aktivera om du vill att användaren ska meddelas via e-post.

1. Dra ett **processsteg** till den högra grenen, öppna egenskaperna, ange följande värden och bekräfta sedan ändringarna:

   * **Titel**: `Publish Page as Requested`

   * **Process**: välj `Activate Page`. Den här processen publicerar den valda sidan till utgivarinstanserna.

1. Klicka på **Synkronisera** (redigeringsverktygsfältet) för att generera körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

   Den nya arbetsflödesmodellen ser ut så här:

   ![wf-13](assets/wf-13.png)

1. Använd det här arbetsflödet på sidan så att användaren kan välja om han/hon vill **publicera sidan som begärd** eller **avvisa publiceringsbegäran** när han/hon går till steget **Fullständig** **Verifiera innehåll**.

   ![chlimage_1-72](assets/chlimage_1-72.png)

### Exempel: Definiera en regel för en OR-delning med ECMA-skript {#defineruleecmascript}

Med stegen **ELLER Dela** kan du infoga villkorsstyrda bearbetningssökvägar i arbetsflödet.

Så här definierar du en OR-regel:

1. Skapa två skript och spara dem i databasen, till exempel under:

   `/apps/myapp/workflow/scripts`

   >[!NOTE]
   >
   >Skripten måste ha en [funktion `check()`](#function-check) som returnerar ett booleskt värde.

1. Redigera arbetsflödet och lägg till **ELLER Dela** i modellen.
1. Redigera egenskaperna för **grenen** i **ELLER Dela**:

   * Definiera detta som **standardflöde** genom att ange **värde** till `true`.

   * Ange sökvägen till skriptet som **Regel**. Till exempel:
     `/apps/myapp/workflow/scripts/myscript1.ecma`

   >[!NOTE]
   >
   >Du kan ändra grenordningen om det behövs.

1. Redigera egenskaperna för **grenen** i **ELLER Dela**.

   * Ange sökvägen till det andra skriptet som **Regel**. Till exempel:
     `/apps/myapp/workflow/scripts/myscript2.ecma`

1. Ange egenskaperna för de enskilda stegen i varje gren. Kontrollera att **Användare/grupp** har angetts.
1. Klicka på **Synkronisera** (redigeringsverktygsfältet) om du vill behålla ändringarna i körningsmodellen.

   Mer information finns i [Synkronisera arbetsflödet](#sync-your-workflow-generate-a-runtime-model).

#### Funktionskontroll() {#function-check}

>[!NOTE]
>
>Se [Använda ECMAScript](/help/sites-developing/workflows-customizing-extending.md#using-ecmascript).

Följande exempelskript returnerar `true` om noden är en `JCR_PATH` som finns under `/content/we-retail/us/en`:

```
function check() {
    if (workflowData.getPayloadType() == "JCR_PATH") {
      var path = workflowData.getPayload().toString();
      var node = jcrSession.getItem(path);

      if (node.getPath().indexOf("/content/we-retail/us/en") >= 0) {
       return true;
      } else {
       return false;
      }
     } else {
      return false;
     }
}
```

### Exempel: Anpassad aktiveringsbegäran {#example-customized-request-for-activation}

Du kan anpassa alla färdiga arbetsflöden. Om du vill ha ett anpassat beteende lägger du över information om rätt arbetsflöde.

Exempel: **Begäran om aktivering**. Det här arbetsflödet används för att publicera sidor i **platser** och aktiveras automatiskt när en innehållsförfattare inte har rätt replikeringsbehörighet. Mer information finns i [Anpassa sidredigering - Anpassa arbetsflödet för begäran om aktivering](/help/sites-developing/customizing-page-authoring-touch.md#customizing-the-request-for-activation-workflow).
