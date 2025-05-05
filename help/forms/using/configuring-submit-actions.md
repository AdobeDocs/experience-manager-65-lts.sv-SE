---
title: Konfigurera åtgärden Skicka
description: Med Forms kan du konfigurera en skicka-åtgärd för att definiera hur ett adaptivt formulär ska bearbetas när det har skickats in. Du kan använda inbyggda skicka-åtgärder eller skriva egna från grunden.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: a5aff5dd-912d-49ee-94e8-38cdbc396e5b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2555'
ht-degree: 0%

---

# Konfigurera åtgärden Skicka {#configuring-the-submit-action}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/configure-submit-actions-and-metadata-submission/configuring-submit-actions.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |


## Introduktion till att skicka in åtgärder {#introduction-to-submit-actions}

En sändningsåtgärd utlöses när en användare klickar på knappen Skicka i ett anpassat formulär. Du kan konfigurera åtgärden skicka i anpassningsbara formulär. Med adaptiva formulär kan du skicka in ett antal åtgärder direkt. Du kan kopiera och utöka standardåtgärderna för att skicka och skapa en egen sändningsåtgärd. Baserat på dina krav kan du dock skriva och registrera din egen skicka-åtgärd för att bearbeta data i det skickade formuläret. Överföringsåtgärden kan använda [synkron eller asynkron sändning](../../forms/using/asynchronous-submissions-adaptive-forms.md).

Du kan konfigurera en skicka-åtgärd i avsnittet **Skicka** i egenskaperna för den adaptiva formulärbehållaren i sidlisten.

![Konfigurera Skicka-åtgärd](assets/thank-you-setting.png)

Konfigurera Skicka-åtgärd

Standardåtgärderna för att skicka in anpassningsbara formulär är:

* Skicka till REST-slutpunkt
* Skicka e-post
* Skicka PDF via e-post
* Anropa en Forms Workflow
* Skicka med formulärdatamodell
* Forms Portal Submit Action
* Starta ett AEM-arbetsflöde
* Skicka till Power Automate

>[!NOTE]
>
>Åtgärden Skicka PDF via e-post gäller endast för adaptiva formulär som använder XFA-mall som formulärmodell.

>[!NOTE]
>
>Kontrollera att mappen [AEM_Installation_Directory]\crx-quickstart\temp\datamanager\ASM
>finns. Katalogen krävs för att temporärt lagra bilagor. Om katalogen inte finns skapar du den.

>[!CAUTION]
>
>Om du [förifyller](../../forms/using/prepopulate-adaptive-form-fields.md) en formulärmall, formulärdatamodell eller schemabaserad adaptiv form med XML- eller JSON-data klagomål till ett schema (XML-schema, JSON-schema, formulärmall eller formulärdatamodell) som är data innehåller inte &lt;afData>-, &lt;afBoundData>- och &lt;/afUnboundData>-taggar, är data för obegränsade fält (obundna fält) adaptiva formulärfält utan [bindref](../../forms/using/prepopulate-adaptive-form-fields.md) -egenskap) i det adaptiva formuläret går förlorade.

Du kan skriva en anpassad skicka-åtgärd för anpassade formulär för att uppfylla ditt användningssätt. Mer information finns i [Skriva anpassad sändningsåtgärd för anpassningsbara formulär](../../forms/using/custom-submit-action-form.md).

## Skicka till REST-slutpunkt {#submit-to-rest-endpoint}

Skicka-alternativet **Skicka till REST-slutpunkten** skickar data som är ifyllda i formuläret till en konfigurerad bekräftelsesida som en del av HTTP GET-begäran. Du kan lägga till namnet på fälten som ska begäras. Begäran har följande format:

`{fieldName}={request parameter name}`

Som visas i bilden nedan skickas `param1` och `param2` som parametrar med värden som kopierats från fälten **textruta** och **numerisk ruta** för nästa åtgärd.

Du kan också **aktivera POST-begäran** och ange en URL för att skicka begäran. Om du vill skicka data till den Experience Manager-server som är värd för formuläret använder du en relativ sökväg som motsvarar rotsökvägen för Experience Manager-servern. Exempel: /content/forms/af/SampleForm.html. Om du vill skicka data till en annan server använder du den absoluta sökvägen.

![Konfigurerar åtgärden Skicka för resterande slutpunkt](assets/action-config.png)

Konfigurerar åtgärden Skicka för resterande slutpunkt

>[!NOTE]
>
>Om du vill skicka fälten som parametrar i en REST-URL måste alla fält ha olika elementnamn, även om fälten placeras på olika paneler.

### Bokför skickade data till en resurs eller extern slutpunkt för vila  {#post-submitted-data-to-a-resource-or-external-rest-end-point-nbsp}

Använd åtgärden **Skicka till REST-slutpunkt** för att skicka skickade data till en rest-URL. URL:en kan vara en intern (servern som formuläret återges på) eller en extern server.

Om du vill skicka data till en intern server anger du sökvägen till resursen. Data bokförs som resurssökväg. Till exempel /content/restEndPoint. För sådana efterfrågningar används autentiseringsinformationen i förfrågan.

Ange en URL om du vill skicka data till en extern server. URL-adressen har formatet https://host:port/path_to_rest_end_point. Se till att du konfigurerar sökvägen så att den hanterar POST-begäran anonymt.

![Mappning för fältvärden skickas som parametrar för Tack-sidan](assets/post-enabled-actionconfig.png)

I exemplet ovan hämtas användarinformationen i `textbox` med parametern `param1`. Syntaxen för att bokföra data som har hämtats med `param1` är:

`String data=request.getParameter("param1");`

På samma sätt är parametrar som du använder för att skicka XML-data och bifogade filer `dataXml` och `attachments`.

Du kan till exempel använda de här två parametrarna i skriptet för att tolka data till en slutpunkt. Du använder följande syntax för att lagra och analysera data:

`String data=request.getParameter("dataXml");`
`String att=request.getParameter("attachments");`

I det här exemplet lagrar `data` XML-data och `att` lagrar data för bifogade filer.

## Skicka e-post {#send-email}

Åtgärden **Skicka e-post** skickar ett e-postmeddelande till en eller flera mottagare när formuläret har skickats. E-postmeddelandet som genereras kan innehålla formulärdata i ett fördefinierat format.

>[!NOTE]
>
>Alla formulärfält måste ha olika elementnamn, även om de finns på olika paneler), för att kunna inkludera formulärdata i ett e-postmeddelande.

## Skicka PDF via e-post {#send-pdf-via-email}

**Skicka PDF via e-post** skickar ett e-postmeddelande med en PDF som innehåller formulärdata till en eller flera mottagare när formuläret har skickats.

>[!NOTE]
>
>Den här överföringsåtgärden är tillgänglig för XFA-baserade adaptiva formulär och XSD-baserade adaptionsformulär som har dokumentmallen.

## Anropa en Forms Workflow {#invoke-a-forms-workflow}

**Skicka till Forms Workflow** skickar en XML-datafil och eventuella bifogade filer till en befintlig Adobe LiveCycle- eller AEM Forms-process i JEE-format.

Mer information om hur du konfigurerar åtgärden Skicka till Forms Workflow finns i [Skicka och bearbeta formulärdata med formulärarbetsflöden](../../forms/using/submit-form-data-livecycle-process.md).

## Skicka med formulärdatamodell {#submit-using-form-data-model}

Åtgärden **Skicka med formulärdatamodell** skriver skickade adaptiva formulärdata för det angivna datamodellsobjektet i en formulärdatamodell till datakällan. När du konfigurerar skicka-åtgärden kan du välja ett datamodellsobjekt vars skickade data du vill skriva tillbaka till dess datakälla.

Dessutom kan du skicka en bifogad fil med hjälp av en formulärdatamodell och en DoR-fil (Document of Record) till datakällan.

Mer information om formulärdatamodell finns i [AEM Forms-dataintegrering](../../forms/using/data-integration.md).

## Forms Portal Submit Action {#forms-portal-submit-action}

Med alternativet **Forms Portal Submit Action** blir formulärdata tillgängliga via en AEM Forms Portal.

Mer information om Forms Portal och skicka-åtgärden finns i [Komponenten för utkast och inskickning](../../forms/using/draft-submission-component.md).

## Starta ett AEM-arbetsflöde {#invoke-an-aem-workflow}

Åtgärden **[!UICONTROL Invoke an AEM Workflow]** Skicka associerar ett anpassat formulär med ett [AEM-arbetsflöde](/help/sites-developing/workflows-models.md). När ett formulär skickas startar det associerade arbetsflödet automatiskt på författarinstansen. Du kan spara datafilen, bifogade filer och postdokument i mappen relativt eller under arbetsflödets eller variabelns nyttolast. Om arbetsflödet är markerat för extern datalagring är variabelalternativet tillgängligt och inte nyttolastalternativet. Du kan välja i listan över variabler som är tillgängliga för arbetsflödesmodellen. Om arbetsflödet markeras för extern datalagring i ett senare skede och inte när arbetsflödet skapas, kontrollerar du att de variabelkonfigurationer som krävs finns på plats.

Innan du använder åtgärden **Anropa ett AEM Workflow** ska du [konfigurera Experience Manager DS-inställningarna](../../forms/using/configuring-the-processing-server-url.md). Mer information om hur du skapar ett AEM-arbetsflöde finns i [Formulärbaserade arbetsflöden i OSGi](../../forms/using/aem-forms-workflow.md).

Åtgärden Skicka placerar följande på arbetsflödets nyttolastplats. Observera dock att endast alternativet Variabel visas om arbetsflödesmodellen är markerad för extern datalagring, och inte nyttolastalternativet.

* **Datafil**: Den innehåller data som har skickats till det adaptiva formuläret. Du kan använda alternativet **[!UICONTROL Data File Path]** för att ange filens namn och sökväg i förhållande till nyttolasten. Sökvägen `/addresschange/data.xml` skapar till exempel en mapp med namnet `addresschange` och placerar den i förhållande till nyttolasten. Du kan också ange att bara `data.xml` ska skicka skickade data utan att skapa en mapphierarki. Använd variabelalternativet och välj variabeln i listan med variabler som är tillgängliga för arbetsflödesmodellen.

>[!NOTE]
>
>Variabler kan användas oavsett om arbetsflödesmodellen är markerad för extern datalagring eller inte.

* **Bifogade filer**: Du kan använda alternativet **[!UICONTROL Attachment Path]** för att ange mappnamnet för att lagra de bifogade filer som överförts till det anpassade formuläret. Mappen skapas i förhållande till nyttolasten. Om arbetsflödet är markerat för extern datalagring använder du variabelalternativet och väljer variabeln i listan med variabler som är tillgängliga för arbetsflödesmodellen.

* **Postdokument**: Det innehåller det postdokument som genererats för det adaptiva formuläret. Du kan använda alternativet **[!UICONTROL Document of Record Path]** för att ange namnet på filen Dokument för post och sökvägen till filen i förhållande till nyttolasten. Sökvägen `/addresschange/DoR.pdf` skapar till exempel en mapp med namnet `addresschange` relativt till nyttolasten och placerar `DoR.pdf` relativt nyttolasten. Du kan även ange att bara `DoR.pdf` ska spara postdokument utan att skapa en mapphierarki. Om arbetsflödet är markerat för extern datalagring använder du variabelalternativet och väljer variabeln i listan med variabler som är tillgängliga för arbetsflödesmodellen.

## Skicka till Power Automate {#microsoft-power-automate}

Du kan konfigurera ett adaptivt formulär så att det kör ett Microsoft® Power Automate Cloud-flöde när du skickar in det. Den konfigurerade adaptiva formen skickar inhämtade data, bilagor och arkivdokument till Power Automate Cloud Flow för bearbetning. Det hjälper er att bygga upp en anpassad datainhämtningsupplevelse och samtidigt utnyttja kraften i Microsoft® Power Automate för att skapa affärslogik kring insamlade data och automatisera kundarbetsflöden. Här är några exempel på vad du kan göra efter att ha integrerat ett adaptivt formulär med Microsoft® Power Automate:

* Använd adaptiva Forms-data i en Power Automate-affärsprocess
* Använd Power Automate för att skicka inhämtade data till fler än 500 datakällor eller till något offentligt tillgängligt API
* Utför komplexa beräkningar på inhämtade data
* Spara adaptiva Forms-data i lagringssystemen enligt ett fördefinierat schema

Den adaptiva Forms-redigeraren tillhandahåller **Anropa ett Microsoft® Power Automate-flöde** för att skicka adaptiva formulärdata, bilagor och arkivdokument till Power Automate Cloud Flow. [Anslut din AEM Forms-instans med Microsoft® Power Automate](/help/forms/using/forms-microsoft-power-automate-integration.md) om du vill använda åtgärden Skicka för att skicka hämtade data till Microsoft® Power Automate.

När konfigurationen är klar kan du använda åtgärden [Anropa ett Microsoft® Power Automate-flöde](/help/forms/using/forms-microsoft-power-automate-integration.md#use-the-invoke-a-microsoft&reg;-power-automate-flow-submit-action-to-send-data-to-a-power-automate-flow-use-the-invoke-microsoft-power-automate-flow-submit-action) för att skicka data till ett Power Automate-flöde.

## Skicka till Microsoft® SharePoint List{#submit-to-sharedrive}

>[!NOTE]
>
>Listfunktionen Skicka till Microsoft® SharePoint introducerades med AEM 6.5 Forms Service Pack 19 (6.5.19.0).

Åtgärden **[!UICONTROL Submit to SharePoint]** kopplar ett adaptivt formulär till ett Microsoft® SharePoint-lagringsutrymme. Du kan skicka formulärdatafilen, bifogade filer eller arkivdokument till den anslutna Microsoft® Sharepoint-lagringsplatsen.

### Ansluta ett anpassat formulär till Microsoft® SharePoint List {#connect-af-sharepoint-list}

Så här ansluter du ett adaptivt formulär till Microsoft® SharePoint List:

1. [Skapa en SharePoint-listkonfiguration](#create-sharepoint-list-configuration): Den ansluter AEM Forms till din Microsoft® Sharepoint-listlagring.
1. [Använd åtgärden **Skicka med formulärdatamodell** i ett adaptivt formulär](#use-submit-using-fdm): Dina anpassade formulärdata skickas till konfigurerade Microsoft® SharePoint.

#### Skapa en listkonfiguration för SharePoint {#create-sharepoint-list-configuration}

Så här ansluter du AEM Forms till din Microsoft® Sharepoint-lista:

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Cloud Services]** > **[!UICONTROL Microsoft® SharePoint]**.
1. Välj en **konfigurationsbehållare**. Konfigurationen lagras i den valda konfigurationsbehållaren.
1. Klicka på **[!UICONTROL Create]** > **[!UICONTROL SharePoint List]** i listrutan. Konfigurationsguiden för SharePoint visas.
1. Ange **[!UICONTROL Title]**, **[!UICONTROL Client ID]**, **[!UICONTROL Client Secret]** och **[!UICONTROL OAuth URL]**. Mer information om hur du hämtar klient-ID, klienthemlighet, klient-ID för OAuth URL finns i [Microsoft®-dokumentation](https://learn.microsoft.com/en-us/graph/auth-register-app-v2).
   * Du kan hämta `Client ID` och `Client Secret` för din app från Microsoft® Azure-portalen.
   * Lägg till omdirigerings-URI som `https://[author-instance]/libs/cq/sharepointlist/content/configurations/wizard.html` i Microsoft® Azure-portalen. Ersätt `[author-instance]` med URL:en för din Author-instans.
   * Lägg till API-behörigheterna `offline_access` och `Sites.Manage.All` på fliken **Microsoft® Graph** för att ge läs-/skrivbehörigheter. Lägg till behörigheten `AllSites.Manage` på fliken **Sharepoint** om du vill fjärrinteragera med SharePoint-data.
   * Använd OAuth-URL: `https://login.microsoftonline.com/tenant-id/oauth2/v2.0/authorize`. Ersätt `<tenant-id>` med `tenant-id` för din app från Microsoft® Azure-portalen.

     >[!NOTE]
     >
     >Fältet **klienthemlighet** är obligatoriskt eller valfritt beroende på din Azure Active Directory-programkonfiguration. Om ditt program är konfigurerat att använda en klienthemlighet är det obligatoriskt att ange klienthemligheten.

1. Klicka på **[!UICONTROL Connect]**. Om anslutningen lyckas visas meddelandet `Connection Successful`.
1. Välj **[!UICONTROL SharePoint Site]** och **[!UICONTROL SharePoint List]** i listrutan.
1. Tryck på **[!UICONTROL Create]** för att skapa molnkonfigurationen för Microsoft® SharePointList.

#### Använda Skicka med formulärdatamodellen i ett anpassat formulär {#use-submit-using-fdm}

Du kan använda den skapade SharePoint List-konfigurationen i ett adaptivt formulär för att spara data eller skapa ett postdokument i en SharePoint List. Utför följande steg om du vill använda en lagringskonfiguration i SharePoint List i en anpassad form:

1. [Skapa en formulärdatamodell med Microsoft](/help/forms/using/create-form-data-model.md)
1. [Konfigurera formulärdatamodellen för att hämta och skicka data](/help/forms/using/work-with-form-data-model.md#configure-services)
1. [Skapa ett anpassat formulär](/help/forms/using/create-adaptive-form.md).
1. [Konfigurera åtgärden Skicka med en formulärdatamodell](/help/forms/using/configuring-submit-actions.md#submit-using-form-data-model-submit)

När du skickar formuläret sparas data i det angivna lagringsutrymmet för Microsoft® Sharepoint-listan.

>[!NOTE]
>
>I Microsoft® SharePoint List stöds inte följande kolumntyper:
>* bildkolumn
>* metadatakolumn
>* personkolumn
>* extern datakolumn


>[!NOTE]
>
>[Generera OSGi-konfigurationer med AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=sv-SE#generating-osgi-configurations-using-the-aem-sdk-quickstart) och [distribuera konfigurationen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/deploy-code.html?lang=sv-SE#deployment-process) till din Cloud Service-instans om du vill ange värden för en konfiguration.

## Förtroende på serversidan i adaptiv form {#server-side-revalidation-in-adaptive-form}

I alla onlinesystem för datainhämtning lägger utvecklare vanligtvis in JavaScript-valideringar på klientsidan för att tillämpa några få affärsregler. Men i moderna webbläsare kan slutanvändarna kringgå valideringarna och skicka in dokument manuellt med hjälp av olika tekniker, till exempel DevTools Console för webbläsare. Sådana tekniker gäller också för adaptiva formulär. En formulärutvecklare kan skapa olika valideringslogik, men tekniskt sett kan slutanvändarna kringgå dessa valideringslogik och skicka ogiltiga data till servern. Ogiltiga data skulle bryta mot de affärsregler som en formulärförfattare har infört.

Med funktionen för omvalidering på serversidan kan du även köra de valideringar som en författare av adaptiva formulär har tillhandahållit när de utformar ett adaptivt formulär på servern. Det förhindrar att inskickade data äventyras och affärsregelöverträdelser som representeras i form av formulärvalidering.

### Vad ska valideras på servern? {#what-to-validate-on-server-br}

Alla körklara fältvalideringar av adaptiva formulär som körs på servern är:

* Obligatoriskt
* Valideringsbildsats
* Valideringsuttryck

### Aktivera validering på serversidan {#enabling-server-side-validation-br}

Använd **Uppdatera på servern** under Adaptiv formulärbehållare i sidlisten för att aktivera eller inaktivera validering på serversidan för det aktuella formuläret.

![Aktivera validering på serversidan](assets/revalidate-on-server.png)

Aktivera validering på serversidan

Om slutanvändaren åsidosätter dessa valideringar och skickar formulären utför servern valideringen igen. Om valideringen misslyckas vid serverslutet stoppas skicka-transaktionen. Slutanvändaren får originalformuläret igen. Insamlade data och skickade data visas för användaren som ett fel.

>[!NOTE]
>
>Validering på serversidan validerar formulärmodellen. Vi rekommenderar att du skapar ett separat klientbibliotek för validering och inte blandar det med andra saker som HTML-formatering och DOM-manipulering i samma klientbibliotek.

### Stöd för anpassade funktioner i valideringsuttryck {#supporting-custom-functions-in-validation-expressions-br}

Ibland finns det komplexa valideringsregler, men det exakta valideringsskriptet finns i anpassade funktioner och författaren anropar dessa anpassade funktioner från fältvalideringsuttryck. Om du vill att det här anpassade funktionsbiblioteket ska vara känt och tillgängligt vid validering på serversidan kan formulärförfattaren konfigurera namnet på AEM klientbibliotek på fliken **Grundläggande** i egenskaper för adaptiv formulärbehållare enligt nedan.

![Stöd för anpassade funktioner i valideringsuttryck](assets/clientlib-cat.png)

Stöd för anpassade funktioner i valideringsuttryck

Författaren kan konfigurera customJavaScript-bibliotek per anpassat formulär. I biblioteket behåller du bara återanvändbara funktioner som är beroende av jquery- och underscore.js-bibliotek från tredje part.

## Felhantering vid sändning {#error-handling-on-submit-action}

Konfigurera anpassade felsidor som 404.jsp och 500.jsp som en del av Experience Manager riktlinjer för säkerhet och skärpning. Dessa hanterare anropas när ett formulär 404- eller 500-fel skickas. Hanterarna anropas också när dessa felkoder aktiveras på noden Publicera.

Mer information finns i [Anpassa sidor som visas av felhanteraren](/help/sites-developing/customizing-errorhandler-pages.md).
