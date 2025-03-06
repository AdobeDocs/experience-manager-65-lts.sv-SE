---
title: Formulär-API:er för HTML5-formulär
description: Externa program använder FormBridge API för att ansluta till XFA-mobilformuläret. API:t skickar en FormBridgeInitialized-händelse i det överordnade fönstret.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: developer-reference
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 46a0ca88-0014-400f-b56f-30afb847e30f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%

---

# Formulär-API:er för HTML5-formulär {#form-bridge-apis-for-html-forms}

Du kan använda API:erna i Form Bridge för att öppna en kommunikationskanal mellan ett XFA-baserat HTML5-formulär och dina program. API:erna för Form Bridge tillhandahåller ett **connect**-API för att skapa anslutningen.

API:t **connect** accepterar en hanterare som ett argument. När en lyckad anslutning har skapats mellan XFA-baserade HTML5-formulär och Form Bridge anropas handtaget.

Du kan använda följande exempelkod för att skapa anslutningen.

```javascript
// Example showing how to connect to FormBridge
window.addEventListener("FormBridgeInitialized",
                                function(event) {
                                    var fb = event.detail.formBridge;
                                    fb.connect(function() {
                                           //use form bridge functions
                         })
                            })
```

>[!NOTE]
>
>Se till att du skapar en anslutning innan du inkluderar filen formRuntime.jsp.

## Tillgängligt Bridge API  {#available-form-bridge-api-nbsp}

**getBridgeVersion()**

Returnerar versionsnumret för skriptbiblioteket

* **Indata**: Ingen
* **Utdata**: Versionsnummer för skriptbiblioteket
* **Fel**: Inga

**isConnected()** Kontrollerar om formulärtillståndet har initierats

* **Indata**: Ingen
* **Utdata**: **True** om XFA-formulärtillståndet har initierats

* **Fel**: Inga

**connect(handler, context)** Skapar en anslutning till FormBridge och kör funktionen när anslutningen har upprättats och formulärtillståndet har initierats

* **Indata**:

   * **handler**: Funktion som ska köras när Form Bridge är anslutet
   * **context**: Det objekt som kontexten (this) för funktionen *handler* anges till.

* **Utdata**: Ingen
* **Fel**: Ingen

**getDataXML(options)** Returnerar aktuella formulärdata i XML-format

* **Indata:**

   * **alternativ:** JavaScript Object innehåller följande egenskaper:

      * **Fel**: Felhanterarfunktion
      * **success**: Hanterarfunktionen Slutförd. Den här funktionen skickas till ett objekt som innehåller XML i egenskapen *data*.
      * **context**: Det objekt som kontexten (this) för funktionen *success* anges till
      * **validationChecker**: Funktion att anropa för att kontrollera valideringsfel som tagits emot från servern. Valideringsfunktionen skickar en array med felsträngar.
      * **formState**: JSON-tillståndet för XFA-formuläret som data-XML ska returneras för. Om inget anges returneras data-XML för det återgivna formuläret.

* **Utdata:** Ingen
* **Fel:** Ingen

**registerConfig(configName, config)** Registrerar användare/portalspecifika konfigurationer med FormBridge. Dessa konfigurationer åsidosätter standardkonfigurationerna. De konfigurationer som stöds anges i avsnittet config.

* **Indata:**

   * **configName:** Namnet på konfigurationen som ska åsidosättas

      * **widgetConfig:** Tillåter användaren att åsidosätta standardwidgetar i formuläret med anpassade widgetar. Konfigurationen åsidosätts enligt följande:

        *formBridge.registerConfig(&quot;widgetConfig&quot;:{/&amp;ast;configuration&amp;ast;/})*

      * **pagingConfig:** Tillåter användaren att åsidosätta standardbeteendet för återgivning av endast den första sidan. Konfigurationen åsidosätts enligt följande:

        *window.formBridge.registerConfig(&quot;pagingConfig&quot;:{pagingDisabled: &lt;true | false>, crinkPageDisabled: &lt;true | false> }).*

      * **LoggingConfig:** Används för att åsidosätta loggningsnivån, inaktivera loggning för en kategori eller för att visa loggkonsolen eller skicka till servern. Konfigurationen kan åsidosättas på följande sätt:

     ```javascript
     formBridge.registerConfig{
       "LoggerConfig" : {
     {
     "on":`<true *| *false>`,
     "category":`<array of categories>`,
     "level":`<level of categories>`, "
     type":`<"console"/"server"/"both">`
     }
       }
     ```

      * **SubmitServiceProxyConfig:** Tillåt användarna att registrera proxytjänster för sändning och inloggning.

        ```javascript
        window.formBridge.registerConfig("submitServiceProxyConfig",
        {
        "submitServiceProxy" : "`<submitServiceProxy>`",
        "logServiceProxy": "`<logServiceProxy>`",
        "submitUrl" : "`<submitUrl>`"
        });
        ```

   * **config:** Konfigurationens värde

* **Utdata:** Objektet innehåller det ursprungliga värdet för konfigurationen i egenskapen *data*.

* **Fel:** Ingen

**hideFields(fieldArray)** Döljer de fält vars uttryck anges i fieldArray. Anger egenskapen presence för de angivna fälten till osynlig

* **Indata:**

   * **fieldArray:** Array med vissa uttryck för de fält som ska döljas

* **Utdata:** Ingen
* **Fel:** Ingen

**showFields(fieldArray)** Visar de fält vars uttryck anges i fieldArray. Anger egenskapen presence för de angivna fälten till visible

* **Indata:**

   * **fieldArray:** Array med vissa uttryck för fälten som ska visas

* **Utdata:** Ingen
* **Fel:** Ingen

**hideSubmitButtons()** Döljer alla Skicka-knappar i formuläret

* **Indata**: Ingen
* **Utdata**: Ingen
* **Fel**: Utlöser ett undantag om formulärtillståndet inte har initierats

**getFormState()** Returnerar den JSON som representerar formulärstatusen

* **Indata:** Ingen
* **Utdata:** Objekt som innehåller JSON som representerar det aktuella formulärtillståndet i egenskapen *data*.

* **Fel:** Ingen

**restoreFormState(options)** Återställer formulärtillståndet från det angivna JSON-läget i options-objektet. Läget används och hanterare för lyckade eller fel anropas när åtgärden har slutförts

* **Indata:**

   * **Alternativ:** JavaScript Object innehåller följande egenskaper:

      * **Fel**: Felhanterarfunktion
      * **success**: Hanterarfunktionen Slutförd
      * **context**: Det objekt som kontexten (this) för funktionen *success* anges till
      * **formState**: formulärets JSON-tillstånd. Formuläret återställs till JSON-läget.

* **Utdata:** Ingen
* **Fel:** Ingen

**setFocus (som)** Anger fokus på fältet som anges i som-uttrycket

* **Indata:** Som uttryck för fältet som fokus ska ställas in på
* **Utdata:** Ingen
* **Fel:** Utlöser ett undantag om det finns ett felaktigt SSA-uttryck

**setFieldValue (som, value)** Anger värdet för fälten för angivna som-uttryck

* **Indata:**

   * **som:** Array som innehåller några uttryck för fältet. Det uttryck som anger fältets värde.
   * **värde:** Array som innehåller värden som motsvarar de som anges i en **som** array. Om värdeets datatyp inte är densamma som fieldType ändras inte värdet.

* **Utdata:** Ingen
* **Fel:** Utlöser ett undantag om det finns ett felaktigt AS-uttryck

**getFieldValue (som)** Returnerar värdet för fälten för angivna som-uttryck

* **Indata:** Matris som innehåller uttryck för fält vars värde måste hämtas
* **Utdata:** Objektet innehåller resultatet som en array i egenskapen **data**.

* **Fel:** Ingen

### Exempel på getFieldValue() API {#example-of-nbsp-getfieldvalue-api}

```JavaScript
var a =  formBridge.getFieldValue("xfa.form.form1.Subform1.TextField");
if(a.errors) {
    var err;
     while((err = a.getNextMessage()) != null)
               alert(a.message)
} else {
   alert(a.data[0])
}
```

**getFieldProperties(som, property)** Hämta listan med värden för den angivna egenskapen i fälten som anges i SOM-uttryck

* **Indata:**

   * **som:** Array som innehåller vissa uttryck för fälten
   * **egenskap**: Namnet på den egenskap vars värde krävs

* **Utdata:** Objektet innehåller resultatet som en array i egenskapen *data*

* **Fel:** Ingen

**setFieldProperties(som, property, values)** Anger värdet för den angivna egenskapen för alla fält som anges i som-uttryck

* **Indata:**

   * **som:** Array som innehåller uttryck för fält vars värde måste anges
   * **egenskap**: Egenskapen vars värde måste anges
   * **värde:** Arrayen innehåller värden för den angivna egenskapen för fält som anges i vissa uttryck

* **Utdata:** Ingen
* **Fel:** Ingen

## Exempel på användning av Bridge API för formulär {#sample-usage-of-form-bridge-api}

```JavaScript
// Example 1: FormBridge.restoreFormState
  function loadFormState() {
    var suc = function(obj) {
             //success
            }
    var err = function(obj) {
           while(var t = obj.getNextMessage()) {
         $("#errorDiv").append("<div>"+t.message+"</div>");
           }
           }
        var _formState = // load form state from storage
    formBridge.restoreFormState({success:suc,error:err,formState:_formState}); // not passing a context means that this will be formBridge itself. Validation errors will be checked.
  }

//--------------------------------------------------------------------------------------------------

//Example 2: FormBridge.submitForm
  function SubmitForm() {
    var suc = function(obj) {
             var data = obj.data;
         // submit the data to a url;
            }
    var err = function(obj) {
           while(var t = obj.getNextMessage()) {
         $("#errorDiv").append("<div>"+t.message+"</div>");
           }
           }
    formBridge.submitForm({success:suc,error:err}); // not passing a context means that this will be formBridge itself. Validation errors will be checked.
  }
```
