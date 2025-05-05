---
title: Lägg till en anpassad felhanterare i Adaptive Forms baserat på kärnkomponenter för AEM Adaptive Forms
description: AEM Forms har körklara hanterare och felhanterare för ett formulär som använder REST-slutpunkten som konfigurerats för att anropa en extern tjänst. Du kan lägga till en standardfelhanterare och en anpassad felhanterare i ett anpassat AEM-formulär.
keywords: Lägg till en anpassad felhanterare, lägg till en standardfelhanterare, lägg till en felhanterare i formuläret, använd regelredigerarens anropstjänst för att lägga till en anpassad felhanterare, konfigurera regelredigeraren för att lägga till en anpassad felhanterare, lägg till en anpassad felhanterare med regelredigeraren
contentOwner: Ruchita Srivastav
content-type: reference
feature: Adaptive Forms,Core Components
solution: Experience Manager, Experience Manager Forms
role: Admin, User
exl-id: de6f259f-87d9-4862-a20e-3825be15dd6e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2251'
ht-degree: 0%

---

# Felhanterare i adaptiv Forms (kärnkomponenter) {#error-handlers-in-adaptive-form}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/add-custom-error-handler-adaptive-forms-core-components.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

AEM Forms har färdiga funktioner och felhanterare för att skicka in formulär. Den innehåller även funktioner för att anpassa felhanterarfunktioner. Du kan till exempel anropa ett anpassat arbetsflöde i serverdelen för specifika felkoder eller informera kunden om att tjänsten inte fungerar. Hanterare är funktioner på klientsidan som körs baserat på serversvaret. När en extern tjänst anropas med API:er överförs data till servern för validering, som returnerar ett svar till klienten med information om lyckad eller felhändelse för överföringen. Informationen skickas som parametrar till den relevanta hanteraren för att köra funktionen. En felhanterare hjälper till att hantera och visa fel eller valideringsproblem som påträffats.

![felhanterararbetsflöde för att förstå hur du lägger till en anpassad felhanterare i formulär](/help/forms/using/assets/error-handler-workflow.png)

Det adaptiva formuläret validerar indata som du anger i fält baserat på förinställda valideringskriterier och söker efter olika fel som returneras av REST-slutpunkten som konfigurerats för att anropa en extern tjänst. Du kan ange valideringskriterier baserat på den datakälla som du använder med det adaptiva formuläret. Om du till exempel använder RESTful-webbtjänster som datakälla kan du definiera valideringskriterierna i en Swagger-definitionsfil.

Om indatavärdena uppfyller valideringskriterierna skickas värdena till datakällan i annat fall visas ett felmeddelande i Adaptiv form med hjälp av en felhanterare. På samma sätt som detta arbetssätt integreras Adaptive Forms med anpassade felhanterare för datavalidering. Om indatavärdena inte uppfyller valideringskriterierna visas felmeddelandena på fältnivå i det adaptiva formuläret. Detta inträffar när det valideringsfelmeddelande som returneras av servern har standardmeddelandeformatet.


## Användning av felhanterare {#uses-of-error-handler}

Felhanterare används för olika syften. Några av användningsområdena för felhanterarfunktioner visas nedan:

* **Utför validering**: Felhanteringen börjar med att användarindata valideras mot fördefinierade regler eller villkor. När användarna fyller i ett adaptivt formulär validerar felhanteraren indata så att det uppfyller det format, den längd eller andra begränsningar som krävs.

* **Ge feedback i realtid**: När ett fel upptäcks visar felhanteraren direkt feedback till användaren, till exempel textbundna felmeddelanden under motsvarande formulärfält. Denna feedback hjälper användarna att identifiera och åtgärda fel utan att behöva skicka in formuläret och vänta på ett svar.


* **Visa felmeddelanden**: När en sändning med adaptiva formulär påträffar ett valideringsfel visar felhanteraren ett felmeddelande. Felmeddelandena ska vara tydliga, koncisa och markera de specifika fält som behöver åtgärdas.

* **Markerar det felaktiga fältet**: För att dra användarens uppmärksamhet till specifika felaktiga fält markeras eller visas motsvarande fält i felhanteraren. Den utförs genom att ändra bakgrundsfärgen, lägga till en ikon eller kantlinje eller någon annan visuell indikator som hjälper användarna att snabbt hitta och åtgärda felen.


## Fel-/felsvarsformat {#failure-response-format}

I ett adaptivt formulär visas felen på fältnivå om servervalideringsfelmeddelanden är i följande standardformat.
Koden nedan visar den befintliga strukturen för felsvar:

```javascript
   {
    errorCausedBy : "SERVER_SIDE_VALIDATION/SERVICE_INVOCATION_FAILURE"
    errors : [
        {
             errorMessage / errorMessages : <validationMsg> / [<validationMsg>, <validationMsg>]
        }
    ]
    originCode : <target error Code>
    originMessage : <unstructured error message returned by service>
    }
```


Var:

* `errorCausedBy` beskriver orsaken till felet.
* `errors` anger det kvalificerade fältnamnet för de fält som underkändes i valideringskriterierna tillsammans med valideringsfelmeddelandet.
* Fältet `originCode` har lagts till av AEM och innehåller http-statuskoden som returneras av den externa tjänsten.
* Fältet `originMessage` har lagts till av AEM och innehåller råa feldata som returnerats av den externa tjänsten.

Med förbättringarna i funktioner och efterföljande uppdateringar i AEM Forms-versionerna har den befintliga felsvarsstrukturen ändrats till ett nytt format baserat på RFC7807, som är bakåtkompatibel med den befintliga felsvarsstrukturen:

```javascript
    {
        "type": "SERVER_SIDE_VALIDATION/FORM_SUBMISSION/SERVICE_INVOCATION/FAILURE/VALIDATION_ERROR" (required)
        "title": "Server side validation failed/Third party service invocation failed" (optional)
        "detail": "" (optional)
        "instance": "" (optional)
        "validationErrors" : [ (required)
            {
                "fieldName":"<qualified fieldname of the field whose data sent is invalid>",
                "dataRef":<JSONPath (or XPath) of the data element which is invalid>
                "details": ["Error Message(s) for the field"] (required)
    
            }
        ],
        "originCode": <Origin http status code> (optional - if there is SERVER_SIDE_VALIDATION)
        "originMessage" : "<unstructured error message returned by service>" (optional - if there is SERVER_SIDE_VALIDATION)
    }
```


>[!NOTE]
>
> * Kontrollera att felsvarsstrukturen innehåller antingen **fieldName** eller **dataRef**.
> * Kontrollera att rubriken **ContentType** är **application/problem+json**.

Var:
* `type (required)` anger typen av fel. Det kan vara något av följande värden:
   * `SERVER_SIDE_VALIDATION` indikerar ett fel på grund av validering på serversidan.
   * `FORM_SUBMISSION` indikerar ett fel under formuläröverföringen
   * `SERVICE_INVOCATION` indikerar ett fel under ett anrop av en tredjepartstjänst.
   * `FAILURE` indikerar ett allmänt fel.
   * `VALIDATION_ERROR` indikerar ett fel på grund av ett valideringsfel.

* `title (optional)` innehåller en titel eller en kort beskrivning av felet.
* `detail (optional)` innehåller ytterligare information om felet om det behövs.
* `instance (optional)` representerar en instans eller identifierare som är associerad med felet och hjälper till att spåra eller identifiera den specifika förekomsten av felet.
* `validationErrors (required)` innehåller information om valideringsfel. Den innehåller följande fält:
   * `fieldname` anger det kvalificerade fältnamnet för de fält som inte uppfyller valideringskriterierna.
   * `dataRef` representerar JSON-sökvägen eller XPath för de fält som inte kunde valideras.
   * `details` innehåller valideringsfelmeddelandet med det felaktiga fältet.
* Fältet `originCode (optional)` har lagts till av AEM och innehåller http-statuskoden som returneras av den externa tjänsten
* Fältet `originMessage (optional)` har lagts till av AEM och innehåller råa feldata som returnerats av den externa tjänsten.

### Exempel på felsvarsformat {#sample-error-response-format}

Vissa av alternativen för att visa felsvaren är:

+++  Baserat på egenskapen Adaptive Form fieldName


* **`Header:`** `content-type:application/problem+json`
* **`Response:`**

  ```javascript
          {
              "type": "VALIDATION_ERROR",
              "validationErrors": [
              {
              "fieldName": "$form.PetId",
              "dataRef": "",
              "details": [
              "Invalid ID supplied. Provided value is not correct!"
          ]
          }
          ]}
  ```


+++


+++ Baserat på egenskapen dataRef för adaptiv form

* **`Header:`** `content-type:application/problem+json`
* **`Response:`**

  ```javascript
      {
          "type": "VALIDATION_ERROR",
          "validationErrors": [
          {
              "fieldName": "",
              "dataRef": "$.Pet.id",
              "details": [
              "Invalid ID supplied. Provided value is not correct!"
              ]
              }
      ]}
  ```

+++

## Förutsättningar {#prerequisites}

Innan du använder felhanterare i en adaptiv Forms:

* [Aktivera adaptiva Forms Core-komponenter för din miljö](enable-adaptive-forms-core-components.md).
* Grundläggande kunskap för att [skapa en anpassad funktion](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-functions-aem-forms.html?lang=sv-SE#:~:text=AEM%20Forms%206.5%20introduced%20the,use%20them%20across%20multiple%20forms.).
* Installera den senaste versionen av [Apache Maven](https://maven.apache.org/download.cgi).

## Lägg till felhanterare med Regelredigeraren {#add-error-handler-using-rule-editor}

Med åtgärden [Anropa tjänst](https://experienceleague.adobe.com/docs/experience-manager-65-lts/forms/adaptive-forms-advanced-authoring/rule-editor.html?lang=en#invoke) i regelredigeraren kan du definiera valideringskriterier baserat på datakällan som du använder med det anpassade formuläret. Om du använder RESTful-webbtjänster som datakälla kan du definiera valideringskriterierna i en Swagger-definitionsfil. Genom att använda felhanterarfunktionerna och regelredigeraren i Adaptive Forms kan du effektivt hantera och anpassa felhanteringen. Du definierar villkoren med Regelredigeraren och konfigurerar de åtgärder som ska utföras när regeln aktiveras. Adaptiv form validerar indata som du anger i fält baserat på förinställda valideringskriterier. Om indatavärdena inte uppfyller valideringskriterierna visas felmeddelandena på fältnivån i ett adaptivt formulär.

>[!NOTE]
>
> * Konfigurera Adaptiv Forms med en formulärdatamodell om du vill använda felhanterare med åtgärden Invoke i regelredigeraren.
> * En standardfelhanterare tillhandahålls för att visa felmeddelanden i fält om felsvaret finns i standardschemat. Du kan också anropa standardfelhanteraren från den anpassade felhanterarfunktionen.

Med regelredigeraren kan du:

* [Lägg till standardfelhanterarfunktion](#add-default-errror-handler)
* [Lägg till anpassad felhanterarfunktion](#add-custom-errror-handler)


### Lägg till standardfelhanterarfunktion {#add-default-errror-handler}

En standardfelhanterare stöds för att visa felmeddelanden i fält om felsvaret är i standardschema eller i valideringsfel på serversidan.
För att förstå hur du använder en standardfelhanterare med hjälp av åtgärden [Anropa tjänst för regelredigerare](https://experienceleague.adobe.com/docs/experience-manager-65-lts/forms/adaptive-forms-advanced-authoring/rule-editor.html?lang=en#invoke) ska du ta ett exempel på ett enkelt adaptivt formulär med två fält, **Pet ID** och **Pet Name** , och använda en standardfelhanterare i fältet **Pet ID** för att kontrollera om det finns olika fel som returneras av REST-slutpunkten som konfigurerats för att anropa en extern tjänst. exempel: `200 - OK`,`404 - Not Found`, `400 - Bad Request`. Så här lägger du till en standardfelhanterare med hjälp av åtgärden Anropa tjänst i regelredigeraren:

1. Öppna ett adaptivt formulär i redigeringsläge, markera en formulärkomponent och välj **[!UICONTROL Rule Editor]** för att öppna regelredigeraren.
1. Välj **[!UICONTROL Create]**.
1. Skapa ett villkor i avsnittet **När** i regeln. Till exempel ändras **När[namnet på fältet Pet-ID]** ändras. Markeringen ändras i listrutan **Välj läge**.
1. I avsnittet **Sedan** väljer du **[!UICONTROL Invoke Service]** i listrutan **Välj åtgärd** .
1. Välj en **posttjänst** och dess motsvarande databindningar i avsnittet **Indata**. Om du till exempel vill validera **Pet ID** markerar du en **Post-tjänst** som **GET /pet/{petId}** och väljer **Pet ID** i avsnittet **Indata** .
1. Välj databindningar i avsnittet **Utdata**. Välj **Djurnamn** i avsnittet **Utdata**.
1. Välj **[!UICONTROL Default Error Handler]** i avsnittet **Felhanterare**.
1. Klicka på **[!UICONTROL Done]**.

![lägg till en standardfelhanterare för fältverifieringskontroller i ett formulär](/help/forms/using/assets/default-error-handler.png)

Som ett resultat av den här regeln kontrollerar de värden du anger för **Pet ID** valideringen för **Pet Name** med hjälp av den externa tjänsten som anropas av REST-slutpunkten. Om valideringskriterierna som baseras på datakällan misslyckas, visas felmeddelandena på fältnivå.

![visar standardfelmeddelandet när du lägger till en standardfelhanterare i ett formulär som hanterar felsvar](/help/forms/using/assets/default-error-message.png)

### Lägg till anpassad felhanterarfunktion {#add-custom-errror-handler}

Du kan lägga till en anpassad felhanterarfunktion för att utföra några av åtgärderna:

* hantera felsvar som använder icke-standard- eller standardfelsvar. Observera att dessa icke-standardfelsvar inte är kompatibla med [standardschemat för felsvar](#failure-response-format).
* skicka analyshändelser till alla analysplattformar. Exempel: Adobe Analytics.
* visa modal dialog med felmeddelanden.

Förutom de ovannämnda åtgärderna kan de anpassade felhanterarna användas för att utföra anpassade funktioner som uppfyller specifika användarkrav.

Den anpassade felhanteraren är en funktion (klientbibliotek) som är utformad för att svara på fel som returneras av en extern tjänst och leverera ett anpassat svar till slutanvändarna. Alla klientbibliotek med anteckningen `@errorHandler` betraktas som en anpassad felhanterarfunktion. Den här anteckningen hjälper till att identifiera felhanterarfunktionen som anges i filen `.js`.

För att förstå hur du skapar och använder en anpassad felhanterare med hjälp av åtgärden [Regelredigerarens anropstjänst](https://experienceleague.adobe.com/docs/experience-manager-65-lts/forms/adaptive-forms-advanced-authoring/rule-editor.html?lang=en#invoke) kan vi ta ett exempel på Adaptiv form med två fält, **Pet ID** och **Pet Name**, och använda en anpassad felhanterare i fältet **Pet ID** för att kontrollera om det finns olika fel som returneras av REST-slutpunkten som konfigurerats för att anropa en extern tjänst, till exempel `200 - OK`,`404 - Not Found`, `400 - Bad Request`.

Så här lägger du till och använder en anpassad felhanterare i ett adaptivt formulär:

1. [Skapa en anpassad felhanterare](#create-custom-error-message)
1. [Använd regelredigeraren för att konfigurera en anpassad felhanterare](#use-custom-error-handler)

#### 1. Skapa en anpassad felhanterare {#create-custom-error-message}

Så här skapar du en anpassad felfunktion:

1. Logga in på `http://server:port/crx/de/index.jsp#`.
1. Skapa en mapp i mappen `/apps`. Skapa till exempel en mapp med namnet `experience-league`.
1. Spara ändringarna.
1. Navigera till den skapade mappen och skapa en nod av typen `cq:ClientLibraryFolder` som `clientlibs`.
1. Navigera till den nyligen skapade mappen `clientlibs` och lägg till egenskaperna `allowProxy` och `categories`:

   ![Egenskaper för anpassad biblioteksnod](/help/forms/using/assets/customlibrary-properties.png)

   >[!NOTE]
   >
   > Du kan ange vilket namn som helst istället för `customfunctionsdemo`.

1. Spara ändringarna.

1. Skapa en mapp med namnet `js` under mappen `clientlibs`.
1. Skapa en JavaScript-fil med namnet `functions.js` under mappen `js`
1. Skapa en fil med namnet `js.txt` under mappen `clientlibs`.
1. Spara ändringarna.
Den skapade mappstrukturen ser ut så här:

   ![Mappstrukturen för klientbiblioteket har skapats](/help/forms/using/assets/customclientlibrary_folderstructure.png)
1. Dubbelklicka på filen `functions.js` för att öppna redigeraren. Filen innehåller koden för den anpassade felhanteraren.
Låt oss lägga till följande kod i JavaScript-filen för att visa svar och rubriker som tagits emot från REST-tjänstens slutpunkt i webbläsarkonsolen.

   ```javascript
       /** 
       Custom Error handler
       * @name customErrorHandler Custom Error Handler Function
       * @errorHandler
       */
       function customErrorHandler(response, headers, globals)
       {
           console.log("Custom Error Handler processing start...");
           console.log("response:"+JSON.stringify(response));
           console.log("headers:"+JSON.stringify(headers));
           alert("CustomErrorHandler - Please enter valid PetId.")
           globals.invoke('defaultErrorHandler',response, headers)
           console.log("Custom Error Handler processing end...");
       }
   ```

   Följande rad i exempelkoden används för att anropa standardfelhanteraren från din anpassade felhanterare:
   `globals.invoke('defaultErrorHandler',response, headers) `

1. Spara `function.js`.
1. Navigera till `js.txt` och lägg till följande kod:

   ```javascript
       #base=js
       functions.js
   ```

1. Spara filen `js.txt`.

Nu ska vi förstå hur du konfigurerar och använder en anpassad felhanterare med hjälp av regelredigerarens Invoke-tjänst i AEM Forms.

#### 2. Använd regelredigeraren för att konfigurera en anpassad felhanterare {#use-custom-error-handler}

Innan du implementerar den anpassade felhanteraren i ett adaptivt formulär måste du se till att klientbiblioteksnamnet i **[!UICONTROL Client Library Category]** justeras med det namn som anges i kategorialternativet i filen `.content.xml` .

![Lägger till namnet på klientbiblioteket i konfigurationen för adaptiv formulärbehållare](/help/forms/using/assets/client-library-category-name-core-component.png)

I det här fallet anges klientbiblioteksnamnet som `customfunctionsdemo` i filen `.content.xml`.

Så här använder du en anpassad felhanterare med åtgärden **[!UICONTROL Rule Editor's Invoke Service]**:

1. Öppna ett adaptivt formulär i redigeringsläge, markera en formulärkomponent och välj **[!UICONTROL Rule Editor]** för att öppna regelredigeraren.
1. Välj **[!UICONTROL Create]**.
1. Skapa ett villkor i avsnittet **När** i regeln. Om till exempel **[namnet på fältet för Pet-ID]** ändras, ändras **väljs** i den nedrullningsbara listan **Välj läge**.
1. I avsnittet **Sedan** väljer du **[!UICONTROL Invoke Service]** i listrutan **Välj åtgärd** .
1. Välj en **posttjänst** och dess motsvarande databindningar i avsnittet **Indata**. Om du till exempel vill validera **Pet ID** markerar du en **Post-tjänst** som **GET /pet/{petId}** och väljer **Pet ID** i avsnittet **Indata** .
1. Välj databindningar i avsnittet **Utdata**. Välj till exempel **Djurnamn** i avsnittet **Utdata**.
1. Välj **[!UICONTROL Custom Error Handler]** i avsnittet **[!UICONTROL Error Handler]**.
1. Klicka på **[!UICONTROL Done]**.

![lägg till anpassad felhanterare i ett formulär för att hantera felsvar](/help/forms/using/assets/custom-error-handler.png)

Som ett resultat av den här regeln kontrollerar de värden du anger för **Pet ID** valideringen för **Pet Name** med hjälp av den externa tjänsten som anropas av REST-slutpunkten. Om valideringskriterierna som baseras på datakällan misslyckas, visas felmeddelandena på fältnivå.

![lägg till en anpassad felhanterare i ett formulär för att hantera felsvar](/help/forms/using/assets/custom-error-handler-message-core-component.png)

Öppna webbläsarkonsolen och kontrollera svaret och rubriken som tagits emot från REST-tjänstens slutpunkt för valideringsfelmeddelandet.

Den anpassade felhanterarfunktionen ansvarar för att utföra ytterligare åtgärder, som att visa en modal dialogruta eller skicka en analyshändelse, baserat på felsvaret. En anpassad felhanterarfunktion ger flexibilitet att anpassa felhanteringen efter specifika användarkrav.

## Se även {#see-also}

* [Skapa en fristående grundkomponentbaserad adaptiv form](/help/forms/using/create-an-adaptive-form-core-components.md)
* [Skapa stilar eller teman för formulären](/help/forms/using/create-or-customize-themes-for-adaptive-forms-core-components.md)
* [Skapa eller lägga till ett anpassat formulär på AEM Sites-sidan](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md)
