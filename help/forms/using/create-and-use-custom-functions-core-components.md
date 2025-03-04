---
title: Skapa och lägga till anpassade funktioner i ett adaptivt formulär
description: AEM Forms stöder anpassade funktioner som gör att användare kan skapa och använda sina egna funktioner i regelredigeraren.
keywords: Lägg till en anpassad funktion, använd en anpassad funktion, skapa en anpassad funktion, använd anpassad funktion i regelredigeraren.
content-type: reference
feature: Adaptive Forms, Core Components
role: Admin, User, Developer
source-git-commit: 168cb023768ff3139937ab7f437ab7d00185bca0
workflow-type: tm+mt
source-wordcount: '3377'
ht-degree: 0%

---

# Anpassade funktioner i adaptiva Forms Core-komponenter

I den här artikeln beskrivs hur du skapar anpassade funktioner med den senaste adaptiva Form Core-komponenten som har de senaste funktionerna, som:

* Cachelagringsfunktion för anpassade funktioner
* Globala omfångsobjekt och fältobjekt har stöd för anpassade funktioner
* Stöd för moderna JavaScript-funktioner som letfunktioner och pilfunktioner (ES10-stöd)

Se till att du ställer in den [senaste formulärversionen](https://github.com/adobe/aem-core-forms-components/tree/release/650) i AEM Forms Core Component-miljön så att den använder de senaste funktionerna i Anpassade funktioner. </span>


| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM 6.5 | Denna artikel |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/create-and-use-custom-functions) |

## Introduktion

AEM Forms 6.5 innehåller JavaScript-funktioner som gör att du kan definiera komplexa affärsregler med regelredigeraren. AEM Forms har en mängd färdiga anpassade funktioner, men många användningsområden kräver att man definierar egna funktioner för flera formulär. Dessa anpassade funktioner förbättrar formulärens funktioner genom att möjliggöra hantering och bearbetning av inmatade data för att uppfylla specifika krav. Dessutom kan de dynamiskt ändra formulärbeteende baserat på de fördefinierade villkoren.

### Användning av anpassade funktioner {#uses-of-custom-function}

Fördelarna med att använda anpassade funktioner i adaptiva Forms Core-komponenter är:


* **Hantera data**: Anpassade funktioner hanterar och bearbetar data som anges i formulärfälten.
* **Databearbetning**: Anpassade funktioner hjälper dig att bearbeta data som anges i formulärfälten.
* **Validering av data**: Med anpassade funktioner kan du utföra anpassade kontroller av formulärindata och tillhandahålla angivna felmeddelanden.
* **Dynamiskt beteende**: Med anpassade funktioner kan du styra det dynamiska beteendet i dina formulär baserat på specifika villkor. Du kan till exempel visa/dölja fält, ändra fältvärden eller justera formulärlogiken dynamiskt.
* **Integration**: Du kan använda anpassade funktioner för att integrera med externa API:er eller tjänster. Det hjälper till att hämta data från externa källor, skicka data till externa Rest-slutpunkter eller utföra anpassade åtgärder baserade på externa händelser.

Anpassade funktioner är i huvudsak klientbibliotek som läggs till i JavaScript-filen. När du har skapat en anpassad funktion blir den tillgänglig i regelredigeraren så att användaren kan välja den i ett anpassat formulär. De anpassade funktionerna identifieras av JavaScript kommentarer i regelredigeraren.

### JavaScript-anteckningar som stöds för anpassade funktioner {#js-annotations}

**JavaScript-anteckningar innehåller metadata för JavaScript-kod**. Det innehåller kommentarer som börjar med specifika symboler, till exempel `/**` och `@`. Anteckningarna innehåller viktig information om funktioner, variabler och andra element i koden. Adaptiv form stöder följande JavaScript-anteckningar för anpassade funktioner:

#### Namn

**Namn** används för att identifiera den anpassade funktionen i regelredigeraren för ett adaptivt formulär. Följande syntaxer används för att namnge en anpassad funktion:

* `@name [functionName] <Function Name>`
* `@function [functionName] <Function Name>`
* `@func [functionName] <Function Name>`

>[!NOTE]
>`[functionName]` är namnet på funktionen. Blanksteg är inte tillåtna.
>`<Function Name>` är visningsnamnet för funktionen i regelredigeraren för Adaptiv Forms.
>Om funktionsnamnet är identiskt med namnet på själva funktionen kan du utelämna `[functionName]` från syntaxen.

#### Parameter

**Parametern** är en lista med argument som används av anpassade funktioner. En funktion kan ha stöd för flera parametrar. Följande syntaxer används för att definiera en parameter i en anpassad funktion:

* `@param {type} name <Parameter Description>`
* `@argument` `{type} name <Parameter Description>`
* `@arg` `{type}` `name <Parameter Description>`

  `{type}` representerar parametertypen. Tillåtna parametertyper är:

   * string: Representerar ett enda strängvärde.
   * number: Representerar ett numeriskt värde.
   * booleskt: Representerar ett enskilt booleskt värde (true eller false).
   * sträng []: Representerar en array med strängvärden.
   * tal[]: Representerar en matris med numeriska värden.
   * boolesk[]: Representerar en matris med booleska värden.
   * date: Representerar ett enda datumvärde.
   * date[]: Representerar en matris med datumvärden.
   * array: Representerar en generisk array som innehåller värden av olika typer.
   * object: Representerar formulärobjektet som skickas till en anpassad funktion i stället för att skicka dess värde direkt.
   * omfång: Representerar det globala objektet, som innehåller skrivskyddade variabler som formulärinstanser, målfältsinstanser och metoder för att utföra formulärändringar i de anpassade funktionerna. Den deklareras som den sista parametern i JavaScript anteckningar och är inte synlig för regelredigeraren i ett adaptivt formulär. Omfångsparametern har åtkomst till formulärets eller komponentens objekt för att utlösa den regel eller händelse som krävs för formulärbearbetning. Om du vill ha mer information om det globala objektet och hur du använder det [klickar du här](/help/forms/using/create-and-use-custom-functions-core-components.md#field-and-global-scope-objects-in-custom-functions-support-field-and-global-objects)

Parametertypen är **inte skiftlägeskänslig** och blanksteg tillåts inte i parameternamnet.

`<Parameter Description>` innehåller information om parameterns syfte. Det kan innehålla flera ord.

<!--

**Optional Parameters**
By default, all parameters are mandatory. You can define a parameter as optional by either adding `=` after the parameter type or enclosing the parameter name in `[]`. Parameters defined as optional in JavaScript annotations are displayed as optional in the rule editor.
To define a variable as an optional parameter, you can use the any of the following syntaxes:
  
* `@param {type=} Input1`

In the above line of code, `Input1` is an optional parameter without any default value. To declare optional parameter with default value:

`@param {string=<value>} input1`
        
`input1` as an optional parameter with the default value set to `value`. 

* `@param {type} [Input1]`

In the above line of code, `Input1` is an optional parameter without any default value. To declare optional parameter with default value:

`@param {array} [input1=<value>]`

    `input1` is an optional parameter of array type with the default value set to `value`. 
    Ensure that the parameter type is enclosed in curly brackets {} and the parameter name is enclosed in square brackets []. 

Consider the following code snippet, where input2 is defined as an optional parameter:

```javascript

        /**
         * optional parameter function
         * @name OptionalParameterFunction
         * @param {string} input1 
         * @param {string=} input2 
         * @return {string}
        */
        function OptionalParameterFunction(input1, input2) {
        let result = "Result: ";
        result += input1;
        if (input2 !== null) {
            result += " " + input2;
        }
        return result;
        }
```

The following illustration displays using the `OptionalParameterFunction` csutom function in the rule editor:

![Optional or required parameters ](/help/forms/using/assets/optional-default-params.png)

You can save the rule without specifying a value for required parameters, but the rule is not executed and displays a warning message as:

![incomplete rule warning](/help/forms/using/assets/incomplete-rule.png)

When user leaves the optional parameter empty, then the "Undefined" value is passed to the custom function for the optional parameter.

To learn more about how to define optional parameters in JSDocs, [click here](https://jsdoc.app/tags-param).

-->

#### Returtyp

Returtypen anger vilken typ av värde som den anpassade funktionen returnerar efter körningen. Följande syntaxer används för att definiera en returtyp i en anpassad funktion:

* `@return {type}`
* `@returns {type}`
  `{type}` representerar funktionens returtyp. Följande returtyper tillåts:
* string: Representerar ett enda strängvärde.
* number: Representerar ett numeriskt värde.
* booleskt: Representerar ett enskilt booleskt värde (true eller false).
* sträng []: Representerar en array med strängvärden.
* tal[]: Representerar en matris med numeriska värden.
* boolesk[]: Representerar en matris med booleska värden.
* date: Representerar ett enda datumvärde.
* date[]: Representerar en matris med datumvärden.
* array: Representerar en generisk array som innehåller värden av olika typer.
* objekt: Representerar formulärobjektet i stället för dess värde direkt.

Returtypen är inte skiftlägeskänslig.

#### Privat

Den anpassade funktionen, som deklarerats som private, visas inte i listan över anpassade funktioner i regelredigeraren för ett adaptivt formulär. Som standard är anpassade funktioner public. Syntaxen för att deklarera den anpassade funktionen som privat är `@private`.

<!--
#### Member

  Syntax: `@memberof namespace`
  Attaches a namespace to the function.
-->

<!--

#### This

   Syntax: `@this currentComponent`

   Use @this to refer to the Adaptive Form component on which the rule is written. 
  
   The following example is based on the field value. In the following example, the rule hides a field in the form. The `this` portion of `this.value` refers to underlying Adaptive Form component, on which the rule is written.

   ```
      /**
      * @function myTestFunction
      * @this currentComponent
      * @param {scope} scope in which code inside function will be executed.
      */
      myTestFunction = function (scope) {
         if(this.value == "O"){
               scope.age.visible = true;
         } else {
            scope.age.visible = false;
         }
      }

   ```

    >[!NOTE]
    >
    >Comments before custom function are used for summary. Summary can extend to multiple lines until a tag is encountered. Limit the size to a single for a concise description in the rule builder.

-->

<!--

## Function declaration supported types {#function-declaration-supported-types}

**Function Statement**

```javascript
function area(len) {
    return len*len;
}
```

This function is included without `jsdoc` comments.

**Function Expression**

```javascript
var area;
//Some codes later
/** */
area = function(len) {
    return len*len;
};
```

**Function Expression and Statement**

```javascript
var b={};
/** */
b.area = function(len) {
    return len*len;
}
```

**Function Declaration as Variable**

```javascript
/** */
var x1,
    area = function(len) {
        return len*len;
    },
    x2 =5, x3 =true;
```

Limitation: custom function picks only the first function declaration from the variable list, if together. You can use function expression for every function declared.

**Function Declaration as Object**

```javascript
var c = {
    b : {
        /** */
        area : function(len) {
            return len*len;
        }
    }
};
```
-->

## Riktlinjer när du skapar anpassade funktioner {#considerations}

Om du vill visa en lista över anpassade funktioner i regelredigeraren kan du använda något av följande format:

### Funktionssats med eller utan jsdoc-kommentarer

Du kan skapa en anpassad funktion med eller utan jsdoc-kommentarer.

```javascript
    function functionName(parameters) 
        {
            // code to be executed
        }
```

Om användaren inte lägger till några JavaScript-anteckningar i den anpassade funktionen visas den i regelredigeraren med sitt funktionsnamn. Vi rekommenderar dock att du tar med JavaScript-anteckningar för förbättrad läsbarhet av anpassade funktioner.


### Pilfunktion med obligatoriska JavaScript-anteckningar eller kommentarer

Du kan skapa en anpassad funktion med en pilfunktionssyntax:

```javascript
    /**
    * test function
    * @name testFunction 
    * @param {string} a parameter description
    * @param {string=} b parameter description
    * @return {string}
    */
    testFunction = (a, b) => {
    return a + b;
    };
    /** */
    testFunction1=(a) => (return a)
    /** */
    testFunction2 = a => a + 100;
    
```

Om användaren inte lägger till några JavaScript-anteckningar i den anpassade funktionen visas inte den anpassade funktionen i regelredigeraren för ett anpassat formulär.

### Funktionsuttryck med obligatoriska JavaScript-anteckningar eller kommentarer

Om du vill visa anpassade funktioner i regelredigeraren för ett adaptivt formulär skapar du anpassade funktioner i följande format:

```javascript
    /**
    * test function
    * @name testFunction 
    * @param {string} input1 parameter description
    * @param {string=} input2 parameter description
    * @return {string}
    */
    testFunction = function(input1,input2)
        {
            // code to be executed
        }
```

Om användaren inte lägger till några JavaScript-anteckningar i den anpassade funktionen visas inte den anpassade funktionen i regelredigeraren för ett anpassat formulär.

### Krav för att skapa en anpassad funktion

Innan du börjar lägga till en anpassad funktion i din adaptiva Forms måste du se till att följande programvara är installerad på datorn:

* **Vanlig textredigerare (IDE)**: En integrerad utvecklingsmiljö (IDE), som Microsoft Visual Studio Code, har avancerade funktioner för enklare redigering, även om en vanlig textredigerare kan fungera.

* **Git:** Versionskontrollsystemet krävs för att hantera kodändringar. Om du inte har det installerat hämtar du det från https://git-scm.com.


## Skapa en anpassad funktion {#create-custom-function}

Steg för att skapa anpassade funktioner är:
1. [Skapa ett klientbibliotek med AEM Project Archetype och lägg till en anpassad funktion](#create-client-library-archetype)
ELLER
   [Skapa anpassade funktioner med CRXDE](#create-add-custom-function)
1. [Lägga till klientbibliotek i ett adaptivt formulär](#add-client-library)
1. [Använda en anpassad funktion i ett adaptivt formulär](#use-custom-functions)


### Skapa ett klientbibliotek med AEM Project Archetype{#create-client-library-archetype}

Du kan lägga till anpassade funktioner genom att lägga till ett klientbibliotek i projektet som skapats [med AEM Project Archetype](https://experienceleague.adobe.com/en/docs/experience-manager-core-components/using/developing/archetype/using#getting-started).
Om du har ett befintligt projekt <!--and have already the project structure as shown in the image below,--> kan du lägga till [anpassade funktioner](#create-add-custom-function) direkt i ditt lokala projekt.

<!--![custom fuction folder structure](assets/custom-library-folder-structure.png)-->

Skapa ett klientbibliotek när du har skapat ett Arketype-projekt eller använt ett befintligt projekt. Så här skapar du ett klientbibliotek:

**Lägg till en klientbiblioteksmapp**

Följ stegen nedan om du vill lägga till en ny biblioteksmapp i din [AEM-projektkatalog]:

1. Öppna [AEM-projektkatalogen] i en redigerare.

   ![anpassad struktur för funktionsmapp](assets/custom-library-folder-structure.png)

1. Sök efter `ui.apps`.
1. Lägg till ny mapp. Lägg till exempel till en mapp med namnet `experience-league`.
1. Navigera till mappen `/experience-league/` och lägg till en `ClientLibraryFolder`. Skapa till exempel en klientbiblioteksmapp med namnet `customclientlibs`.

   Platsen är: `[AEM project directory]/ui.apps/src/main/content/jcr_root/apps/`

**Lägg till filer och mappar i mappen Klientbibliotek**

Lägg till följande i den tillagda klientbiblioteksmappen:

* `.content.xml` fil
* `js.txt` fil
* Mappen `js`

`Location is: [AEMaaCS project directory]/ui.apps/src/main/content/jcr_root/apps/experience-league/customclientlibs/`

1. Lägg till följande kodrader i `.content.xml`:

   ```javascript
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
   jcr:primaryType="cq:ClientLibraryFolder"
   categories="[customfunctionscategory]"/>
   ```

   >[!NOTE]
   >
   > Du kan välja vilket namn som helst för egenskapen `client library folder` och `categories`.

1. Lägg till följande kodrader i `js.txt`:

   ```javascript
         #base=js
       function.js
   ```

1. Lägg till javascript-filen som `function.js` i mappen `js` som innehåller de anpassade funktionerna:

   ```javascript
   /**
       * Calculates Age
       * @name calculateAge
       * @param {object} field
       * @return {string} 
   */
   
   function calculateAge(field) {
   var dob = new Date(field);
   var now = new Date();
   
   var age = now.getFullYear() - dob.getFullYear();
   var monthDiff = now.getMonth() - dob.getMonth();
   
   if (monthDiff < 0 || (monthDiff === 0 && now.getDate() < dob.getDate())) {
   age--;
   }
   
   return age;
   }
   ```

1. Spara filerna.

![anpassad struktur för funktionsmapp](assets/custom-function-added-files.png)

**Inkludera den nya mappen i filter.xml**:

1. Navigera till filen `/ui.apps/src/main/content/META-INF/vault/filter.xml` i [AEMaaCS-projektkatalogen].

1. Öppna filen och lägg till följande rad i slutet:

   `<filter root="/apps/experience-league" />`
1. Spara filen.

   ![XML för anpassat funktionsfilter](assets/custom-function-filterxml.png)

1. Bygg den nya klientbiblioteksmappen i din AEM-miljö genom att följa stegen i avsnittet [Skapa ](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype#how-to-build).

## Skapa och driftsätt anpassade funktioner via CRXDE{#create-add-custom-function}

Om du använder det senaste AEM Forms- och Forms-tillägget kan du skapa en anpassad funktion via CRXDE för att använda de senaste uppdateringarna av anpassade funktioner. Gör så här:

<!--![custom fuction folder structure](assets/custom-library-folder-structure.png)-->


1. Logga in på `http://server:port/crx/de/index.jsp#`.
1. Skapa en mapp i mappen `/apps`. Skapa till exempel en mapp med namnet `experience-league`.
1. Spara ändringarna.
1. Navigera till den skapade mappen och skapa en nod av typen `cq:ClientLibraryFolder` som `clientlibs`.
1. Navigera till den nyligen skapade mappen `clientlibs` och lägg till egenskaperna `allowProxy` och `categories`:

   ![Egenskaper för anpassad biblioteksnod](/help/forms/using/assets/customlibrary-catproperties.png)

   >[!NOTE]
   >
   > Du kan ange vilket namn som helst istället för `customfunctionsdemo`.

1. Spara ändringarna.

1. Skapa en mapp med namnet `js` under mappen `clientlibs`.
1. Skapa en JavaScript-fil med namnet `functions.js` i mappen `js`.
1. Skapa en fil med namnet `js.txt` under mappen `clientlibs`.
1. Spara ändringarna.
Den skapade mappstrukturen ser ut så här:

   ![Mappstrukturen för klientbiblioteket har skapats](/help/forms/using/assets/clientlibrary_folderstructure.png)
1. Dubbelklicka på filen `functions.js` för att öppna redigeraren. Filen innehåller koden för den anpassade funktionen.
Låt oss lägga till följande kod i JavaScript-filen för att beräkna ålder baserat på födelsedatum (ÅÅÅÅ-MM-DD).

   ```javascript
       /**
            * Calculates Age
            * @name calculateAge 
            * @return {string} 
       */
   
       function calculateAge(dateOfBirthString) {
       var dob = new Date(dateOfBirthString);
       var now = new Date();
   
       var age = now.getFullYear() - dob.getFullYear();
       var monthDiff = now.getMonth() - dob.getMonth();
   
       if (monthDiff < 0 || (monthDiff === 0 && now.getDate() < dob.getDate())) {
       age--;
       }
   
       return age;
       }
   ```

1. Spara `function.js`.
1. Navigera till `js.txt` och lägg till följande kod:

   ```javascript
       #base=js
       functions.js
   ```

1. Spara filen `js.txt`.

Du kan referera till följande [anpassade funktionsmapp](/help/forms/using/assets/customfunction.zip). Hämta och installera den här mappen på din AEM-instans.

Nu kan du använda den anpassade funktionen i ditt adaptiva formulär genom att lägga till klientbiblioteket.

## Lägga till klientbibliotek i ett adaptivt formulär{#add-client-library}

När du har distribuerat klientbiblioteket till din AEM Forms-miljö kan du använda dess funktioner i ditt adaptiva formulär. Lägga till klientbiblioteket i ditt adaptiva formulär

1. Öppna formuläret i redigeringsläge. Om du vill öppna ett formulär i redigeringsläget markerar du ett formulär och väljer **[!UICONTROL Edit]**.
1. Öppna innehållsläsaren och markera komponenten **[!UICONTROL Guide Container]** i det adaptiva formuläret.
1. Klicka på egenskapsikonen för Guide Container. Dialogrutan Adaptiv formulärbehållare öppnas.
1. Öppna fliken **[!UICONTROL Basic]** och välj namnet på **[!UICONTROL client library category]** i listrutan (välj i det här fallet `customfunctionscategory`).

   ![Lägger till klientbiblioteket för anpassade funktioner](/help/forms/using//assets/custom-function-category-name-core-component.png)

1. Klicka på **[!UICONTROL Done]**.

Nu kan du skapa en regel som använder anpassade funktioner i regelredigeraren:

![Lägger till klientbiblioteket för anpassade funktioner](/help/forms/using//assets/calculateage-customfunction.png)

Nu ska vi förstå hur du konfigurerar och använder en anpassad funktion med [tjänsten Invoke i regelredigeraren i AEM Forms 6.5](/help/forms/using/rule-editor-core-components.md#invoke-form-data-model-service-invoke)

## Använda anpassad funktion i ett anpassat formulär {#use-custom-functions}

I ett anpassat formulär kan du använda [anpassade funktioner i regelredigeraren](/help/forms/using/rule-editor-core-components.md).
Låt oss lägga till följande kod i JavaScript-filen (`Function.js`) för att beräkna ålder baserat på födelsedatum (ÅÅÅÅ-MM-DD). Skapa en anpassad funktion som `calculateAge()` som tar födelsedatumet som indata och returnerar ålder:

```javascript
    /**
        * Calculates Age
        * @name calculateAge
        * @param {object} field
        * @return {string} 
    */

    function calculateAge(field) {
    var dob = new Date(field);
    var now = new Date();

    var age = now.getFullYear() - dob.getFullYear();
    var monthDiff = now.getMonth() - dob.getMonth();

    if (monthDiff < 0 || (monthDiff === 0 && now.getDate() < dob.getDate())) {
    age--;
    }

    return age;
    }
```

I ovanstående exempel anropas den anpassade funktionen `calculateAge` när användaren anger födelsedatumet i formatet (ÅÅÅÅ-MM-DD) och sedan returnerar ålder.

![Den anpassade funktionen Beräkna ålder i regelredigeraren](/help/forms/using/assets/custom-function-calculate-age.png)

Låt oss förhandsgranska formuläret för att se hur de anpassade funktionerna implementeras via regelredigeraren:

![Den anpassade funktionen Beräkna ålder i regelredigeraren för förhandsgranskning av formulär](/help/forms/using/assets/custom-function-age-calculate-form.png)

>[!NOTE]
>
> Du kan referera till följande [anpassade funktionsmapp](/help/forms/using/assets/customfunctions.zip). Hämta och installera den här mappen i din AEM-instans med [Package Manager](https://experienceleague.adobe.com/en/docs/experience-manager-65-lts/content/sites/administering/contentmanagement/package-manager).

### Stöd för asynkrona funktioner i anpassade funktioner {#support-of-async-functions}

Asynkrona anpassade funktioner visas inte i regelredigeringslistan. Det går dock att anropa asynkrona funktioner i anpassade funktioner som skapats med synkrona funktionsuttryck.

![Synkronisera och asynkron anpassad funktion](/help/forms/using/assets/workflow-for-sync-async-custom-fumction.png)

>[!NOTE]
>
> Fördelen med att anropa asynkrona funktioner i anpassade funktioner är att asynkrona funktioner tillåter samtidig körning av flera åtgärder, med resultatet av varje funktion som används i de anpassade funktionerna.

Titta på koden nedan för att se hur vi kan anropa asynkrona funktioner med anpassade funktioner:

```javascript
    
    async function asyncFunction() {
    const response = await fetch('https://petstore.swagger.io/v2/store/inventory');
    const data = await response.json();
    return data;
    }

    /**
    * callAsyncFunction
    * @name callAsyncFunction callAsyncFunction
    */
    function callAsyncFunction() {
    asyncFunction()
        .then(responseData => {
        console.log('Response data:', responseData);
        })
        .catch(error => {
         console.error('Error:', error);
    });
}
```

I ovanstående exempel är funktionen asyncFunction en `asynchronous function`. Den utför en asynkron åtgärd genom att göra en `GET`-begäran till `https://petstore.swagger.io/v2/store/inventory`. Det väntar på svar med `await`, tolkar svarstexten som JSON med `response.json()` och returnerar sedan data. Funktionen `callAsyncFunction` är en synkron anpassad funktion som anropar funktionen `asyncFunction` och visar svarsdata i konsolen. Även om funktionen `callAsyncFunction` är synkron anropar den asynkrona funktionen asyncFunction och hanterar resultatet med programsatserna `then` och `catch`.

För att se hur den fungerar kan vi lägga till en knapp och skapa en regel för knappen som anropar den asynkrona funktionen när en knapp klickas.

![skapar regel för asynkron funktion](/help/forms/using/assets/rule-for-async-funct.png)

Titta på bilden för konsolfönstret nedan för att visa att när användaren klickar på knappen `Fetch` anropas den anpassade funktionen `callAsyncFunction` som i sin tur anropar en asynkron funktion `asyncFunction`. Kontrollera konsolfönstret för att visa svaret när knappen klickar:

![Konsolfönstret](/help/forms/using/assets/async-custom-funct-console.png)

Låt oss dyka upp i funktionerna för anpassade funktioner.

## Olika funktioner för anpassade funktioner

Du kan använda anpassade funktioner för att lägga till anpassade funktioner i formulär. Dessa funktioner har stöd för olika funktioner, som att arbeta med specifika fält, använda globala fält eller cachelagring. Tack vare denna flexibilitet kan ni anpassa formulären efter organisationens behov.

### Fält- och globala omfångsobjekt i anpassade funktioner {#support-field-and-global-objects}

Fältobjekt refererar till enskilda komponenter eller element i ett formulär, t.ex. textfält och kryssrutor. Globals-objektet innehåller skrivskyddade variabler som formulärinstans, målfältsinstans och metoder för att göra formulärändringar i anpassade funktioner.

>[!NOTE]
>
> `param {scope} globals` måste vara den sista parametern och visas inte i regelredigeraren för ett anpassat formulär.

<!-- Let us look at the following code snippet:

```JavaScript
   
    /**
    * updateDateTime
    * @name updateDateTime
    * @param {object} field
    * @param {scope} globals
    */
    function updateDateTime(field, globals) {
    // Accessing the Date object from the global scope
    var currentDate = new Date();
    // Formatting the date and time
    var formattedDateTime = currentDate.toLocaleString();
    // Updating the field value with the formatted date and time using setProperty.
    globals.functions.setProperty(field, {value: formattedDateTime});
    }
```

In the above code snippet, a custom function named `updateDateTime` takes parameters such as a field object and a global object. The field represents the textbox object where the formatted date and time value is displayed within the form. -->

Låt oss lära oss hur anpassade funktioner använder fält och globala objekt med hjälp av ett `Contact Us`-formulär med olika användningar.

![Kontakta oss ](/help/forms/using/assets/contact-us-form.png)

#### **Använd skiftläge**: Visa en panel med regeln `SetProperty`

Lägg till följande kod i den anpassade funktionen enligt beskrivningen i avsnittet [create-custom-function](#create-custom-function) för att ange formulärfältet som `Required`.

```javascript
    
    /**
    * enablePanel
    * @name enablePanel
    * @param {object} field1
    * @param {object} field2
    * @param {scope} globals 
    */

    function enablePanel(field1,field2, globals)
    {
       if(globals.functions.validate(field1).length === 0)
       {
       globals.functions.setProperty(field2, {visible: true});
       }
    }
```

>[!NOTE]
>
> * Du kan konfigurera fältegenskaperna med hjälp av de tillgängliga egenskaperna i `[form-path]/jcr:content/guideContainer.model.json`.
> * Ändringar som görs i formuläret med metoden `setProperty` för Global-objektet är asynkrona och återspeglas inte när den anpassade funktionen körs.

I det här exemplet valideras panelen `personaldetails` när du klickar på knappen. Om inga fel upptäcks på panelen visas en annan panel, `feedback`-panelen, när du klickar på knappen.

Låt oss skapa en regel för knappen `Next` som validerar panelen `personaldetails` och gör panelen `feedback` synlig när användaren klickar på knappen `Next`.

![Ange egenskap](/help/forms/using/assets/custom-function-set-property.png)

Se bilden nedan för att visa var panelen `personaldetails` valideras när du klickar på knappen `Next`. Om alla fält i `personaldetails` valideras blir panelen `feedback` synlig.

![Ange förhandsgranskning av egenskapsformulär](/help/forms/using/assets/set-property-form-preview.png)

Om det finns fel i fälten på panelen `personaldetails` visas de på fältnivå när du klickar på knappen `Next` och panelen `feedback` visas inte.

![Ange förhandsgranskning av egenskapsformulär](/help/forms/using/assets/set-property-panel.png)

#### **Använd skiftläge**: Verifiera fältet.

Lägg till följande kod i den anpassade funktionen enligt anvisningarna i avsnittet [create-custom-function](#create-custom-function) för att validera fältet.

```javascript
    /**
    * validateField
    * @name validateField
    * @param {object} field
    * @param {scope} globals
    */
    function validateField(field,globals)
    {
    
        globals.functions.validate(field);
    
    }   
```

>[!NOTE]
>
> Om inget argument skickas i funktionen `validate()` valideras formuläret.

I det här exemplet används ett anpassat valideringsmönster för fältet `contact`. Användare måste ange ett telefonnummer som börjar med `10` följt av `8` siffror. Om användaren anger ett telefonnummer som inte börjar med `10` eller innehåller fler eller färre än `8` siffror visas ett valideringsfelmeddelande när knappen klickar:

![Mönster för e-postadressvalidering](/help/forms/using/assets/custom-function-validation-pattern.png)

Nästa steg är nu att skapa en regel för knappen `Next` som validerar fältet `contact` vid knappklicket.

![Valideringsmönster](/help/forms/using/assets/custom-function-validate.png)

Se bilden nedan för att visa att om användaren anger ett telefonnummer som inte börjar med `10` visas ett felmeddelande på fältnivå:

![Mönster för e-postadressvalidering](/help/forms/using/assets/custom-function-validate-error-message.png)

Om användaren anger ett giltigt telefonnummer och alla fält på panelen `personaldetails` valideras visas panelen `feedback` på skärmen:

![Mönster för e-postadressvalidering](/help/forms/using/assets/validate-form-preview-form.png)

#### **Använd skiftläge**: Återställ en panel

Lägg till följande kod i den anpassade funktionen enligt anvisningarna i avsnittet [create-custom-function](#create-custom-function) för att återställa panelen.

```javascript
    /**
    * resetField
    * @name  resetField
    * @param {string} input1
    * @param {object} field
    * @param {scope} globals 
    */
    function  resetField(field,globals)
    {
    
        globals.functions.reset(field);
    
    }
```

>[!NOTE]
>
> Om inget argument skickas i funktionen `reset()` valideras formuläret.

I det här exemplet återställs panelen `personaldetails` när du klickar på knappen `Clear`. Nästa steg är att skapa en regel för knappen `Clear` som återställer panelen när du klickar på knappen.

![Knappen Rensa](/help/forms/using/assets/custom-function-reset-field.png)

Se bilden nedan för att visa att panelen `personaldetails` återställs om användaren klickar på knappen `clear`:

![Återställ formulär](assets/custom-function-reset-form.png)

#### **Använd skiftläge**: Om du vill visa anpassat meddelande på fältnivå och markera fältet som ogiltigt

Du kan använda funktionen `markFieldAsInvalid()` för att definiera ett fält som ogiltigt och ange ett anpassat felmeddelande på fältnivå. Värdet `fieldIdentifier` kan vara `fieldId`, `field qualifiedName` eller `field dataRef`. Värdet för objektet `option` kan vara `{useId: true}`, `{useQualifiedName: true}` eller `{useDataRef: true}`.
Syntaxerna som används för att markera fält som ogiltiga och ange anpassade meddelanden är:

* `globals.functions.markFieldAsInvalid(field.$id,"[custom message]",{useId: true});`
* `globals.functions.markFieldAsInvalid(field.$qualifiedName, "[custom message]", {useQualifiedName: true});`
* `globals.functions.markFieldAsInvalid(field.$dataRef, "[custom message]", {useDataRef: true});`

Lägg till följande kod i den anpassade funktionen enligt beskrivningen i avsnittet [create-custom-function](#create-custom-function) för att aktivera anpassade meddelanden på fältnivå.

```javascript
    /**
    * customMessage
    * @name customMessage
    * @param {object} field
    * @param {scope} globals 
    */
    function customMessage(field, globals) {
    const minLength = 15;
    const comments = field.$value.trim();
    if (comments.length < minLength) {
        globals.functions.markFieldAsInvalid(field.$id, "Comments must be at least 15 characters long.", { useId: true });
    }
}
```

I det här exemplet visas ett anpassat meddelande på fältnivå om användaren skriver färre än 15 tecken i textrutan för kommentarer.

Nästa steg är att skapa en regel för fältet `comments`:

![Markera fältet som ogiltigt](/help/forms/using/assets/custom-function-invalid-field.png)

Se demonstrationen nedan för att visa att om du anger negativ feedback i fältet `comments` utlöses visningen av ett anpassat meddelande på fältnivå:

![Markera fältet som ogiltigt förhandsgranskningsformulär](/help/forms/using/assets/custom-function-invalidfield-form.png)

Om användaren skriver in mer än 15 tecken i textrutan för kommentarer valideras fältet och formuläret skickas:

![Markera fältet som ett giltigt förhandsgranskningsformulär](/help/forms/using/assets/custom-function-validfield-form.png)


#### **Använd skiftläge**: Skicka ändrade data till servern

Följande kodrad:
`globals.functions.submitForm(globals.functions.exportData(), false);` används för att skicka formulärdata efter manipulering.
* Det första argumentet är de data som ska skickas.
* Det andra argumentet anger om formuläret ska valideras innan det skickas in. Det är `optional` och inställt som `true` som standard.
* Det tredje argumentet är `contentType` i överföringen, som också är valfritt med standardvärdet som `multipart/form-data`. De andra värdena kan vara `application/json` och `application/x-www-form-urlencoded`.

Lägg till följande kod i den anpassade funktionen enligt beskrivningen i avsnittet [create-custom-function](#create-custom-function) för att skicka manipulerade data till servern:

```javascript
    /**
    * submitData
    * @name submitData
    * @param {object} field
    * @param {scope} globals 
    */
    function submitData(globals)
    {
    
    var data = globals.functions.exportData();
    if(!data.comments) {
    data.comments = 'NA';
    }
    console.log('After update:{}',data);
    globals.functions.submitForm(data, false);
    }
```

I det här exemplet skickas `NA` till servern när formuläret skickas om användaren lämnar textrutan `comments` tom.

Skapa nu en regel för knappen `Submit` som skickar data:

![Skicka data](/help/forms/using/assets/custom-function-submit-data.png)

Titta på bilden för `console window` nedan för att visa att om användaren lämnar textrutan `comments` tom så skickas värdet som `NA` på servern:

![Skicka data i konsolfönstret](/help/forms/using/assets/custom-function-submit-data-form.png)

Du kan även kontrollera konsolfönstret för att visa data som skickats till servern:

![Inspektera data i konsolfönstret](/help/forms/using/assets/custom-function-submit-data-console-data.png)

<!--

#### **Use Case**: Display form submission and failure messages for custom submit action 

Add the following line of code as explained in the [create-custom-function ](#create-custom-function) section, to customize the submission or failure message for form submissions and display the form submission messages in a modal box:

```javascript
/**
 * Handles the success response after a form submission.
 *
 * @param {scope} globals - This object contains a read-only form instance, target field instance, triggered event, and methods for performing form modifications within custom functions.
 * @returns {void}
 */
function customSubmitSuccessHandler(globals) {
    var event = globals.event;
    var submitSuccessResponse = event.payload.body;
    var form = globals.form;

    if (submitSuccessResponse) {
        if (submitSuccessResponse.redirectUrl) {
            window.location.href = encodeURI(submitSuccessResponse.redirectUrl);
        } else if (submitSuccessResponse.thankYouMessage) {
            showModal("success", submitSuccessResponse.thankYouMessage);
        }
    }
}

/**
 * Handles the error response after a form submission.
 *
 * @param {string} customSubmitErrorMessage - The custom error message.
 * @param {scope} globals - This object contains a read-only form instance, target field instance, triggered event, and methods for performing form modifications within custom functions.
 * @returns {void}
 */
function customSubmitErrorHandler(customSubmitErrorMessage, globals) {
    showModal("error", customSubmitErrorMessage);
}
function showModal(type, message) {
    // Remove any existing modals
    var existingModal = document.getElementById("modal");
    if (existingModal) {
        existingModal.remove();
    }

    // Create the modal dialog
    var modal = document.createElement("div");
    modal.setAttribute("id", "modal");
    modal.setAttribute("class", "modal");

    // Create the modal content
    var modalContent = document.createElement("div");
    modalContent.setAttribute("class", "modal-content");

    // Create the modal header
    var modalHeader = document.createElement("div");
    modalHeader.setAttribute("class", "modal-header");
    modalHeader.innerHTML = "<h2>" + (type === "success" ? "Thank You" : "Error") + "</h2>";

    // Create the modal body
    var modalBody = document.createElement("div");
    modalBody.setAttribute("class", "modal-body");
    modalBody.innerHTML = "<p class='" + type + "-message'>" + message + "</p>";

    // Create the modal footer
    var modalFooter = document.createElement("div");
    modalFooter.setAttribute("class", "modal-footer");

    // Create the close button
    var closeButton = document.createElement("button");
    closeButton.setAttribute("class", "close-button");
    closeButton.innerHTML = "Close";
    closeButton.onclick = function() {
        modal.remove();
    };

    // Append the elements to the modal content
    modalFooter.appendChild(closeButton);
    modalContent.appendChild(modalHeader);
    modalContent.appendChild(modalBody);
    modalContent.appendChild(modalFooter);

    // Append the modal content to the modal
    modal.appendChild(modalContent);

    // Append the modal to the document body
    document.body.appendChild(modal);
}
```

In this example, when the user uses the `customSubmitSuccessHandler` and `customSubmitErrorHandler` custom functions, the success and failure messages are displayed in a modal. The JavaScript function `showModal(type, message)` is used to dynamically create and display a modal dialog box on a screen.

Now, create a rule for successful form submission:

![Form submission success](/help/forms/using/assets/form-submission-success.png)

Refer to the illustration below to demonstrate that when the form is submitted successfully, the success message is displayed in a modal:

![Form submission success message](/help/forms/using/assets/form-submission-success-message.png )
 
Similarly, let us create a rule for failed form submissions:

![Form submission fail](/help/forms/using/assets/form-submission-fail.png)

Refer to the illustration below to demonstrate that when the form submission fails, the error message is displayed in a modal:

![Form submission fail message](/help/forms/using/assets/form-submission-fail-message.png )

To display form submission success and failure in a default manner, the `Default submit Form Success Handler` and `Default submit Form Error Handler` functions are available out of the box.

In case, the custom submit action fails to perform as expected in existing AEM projects or forms, refer to the [troubleshooting](#troubleshooting) section.

-->

## Cachelagringsstöd för anpassad funktion

Adaptiv Forms implementerar cachning för anpassade funktioner för att förbättra svarstiden samtidigt som den anpassade funktionslistan hämtas i regelredigeraren. Ett meddelande som `Fetched following custom functions list from cache` visas i filen `error.log`.

![anpassad funktion med cachestöd](/help/forms/using/assets/custom-function-cache-error.png)

Om de anpassade funktionerna ändras blir cachningen ogiltig och den tolkas.

## Felsökning {#troubleshooting}

* Användaren måste se till att kärnkomponenten och specifikationsversionen för [är inställd på den senaste versionen ](https://github.com/adobe/aem-core-forms-components/tree/release/650). För befintliga AEM-projekt och -formulär finns det dock ytterligare steg att utföra:

   * För AEM-projektet bör användaren ersätta alla instanser av `submitForm('custom:submitSuccess', 'custom:submitError')` med `submitForm()` och distribuera projektet.

   * Om de anpassade överföringshanterarna inte fungerar som de ska i befintliga formulär måste användaren öppna och spara regeln `submitForm` på knappen **Skicka** med regelredigeraren. Den här åtgärden ersätter den befintliga regeln från `submitForm('custom:submitSuccess', 'custom:submitError')` med `submitForm()` i formuläret.


* Om det uppstår ett fel i JavaScript-filen som innehåller kod för anpassade funktioner visas inte de anpassade funktionerna i regelredigeraren i ett anpassat formulär. Om du vill kontrollera listan med anpassade funktioner kan du navigera till filen `error.log` för felet. Om ett fel uppstår visas listan med anpassade funktioner tom:

  ![felloggfil](/help/forms/using/assets/custom-function-list-error-file.png)

  Om det inte finns något fel hämtas den anpassade funktionen och visas i filen `error.log`. Ett meddelande som `Fetched following custom functions list` visas i filen `error.log`:

  ![felloggfil med rätt anpassad funktion](/help/forms/using/assets/custom-function-list-fetched-in-error.png)

## Överväganden

* `parameter type` och `return type` stöder inte `None`.

* De funktioner som inte stöds i den anpassade funktionslistan är:
   * Generatorfunktioner
   * Async-/Await-funktioner
   * Metoddefinitioner
   * Klassmetoder
   * Standardparametrar
   * Restparametrar
