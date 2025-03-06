---
title: Skapa och lägga till anpassade funktioner i ett adaptivt formulär
description: AEM Forms stöder anpassade funktioner som gör att användare kan skapa och använda sina egna funktioner i regelredigeraren.
feature: Adaptive Forms, Foundation Components
role: Admin, User, Developer
exl-id: 40329e80-d794-4e43-8ed4-d88ce3c48751
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 0%

---

# Anpassade funktioner i Adaptive Forms

## Introduktion

I AEM Forms 6.5 introducerades möjligheten att definiera JavaScript-funktioner som kan användas för att definiera komplexa affärsregler med regelredigeraren. AEM Forms har ett antal anpassade funktioner som du kan använda, men du måste definiera egna funktioner och använda dem i flera formulär.

Med de anpassade funktionerna kan man bättre hantera blanketterna genom att underlätta hanteringen och bearbetningen av inmatade data. De möjliggör också dynamisk ändring av formulärbeteende baserat på fördefinierade kriterier.
I Adaptiv Forms kan du använda anpassade funktioner i [regelredigeraren för ett adaptivt formulär](/help/forms/using/rule-editor.md) för att skapa specifika valideringsregler för formulärfält.
Låt oss förstå hur den anpassade funktionen används där användare anger e-postadressen och du vill se till att den angivna e-postadressen har ett visst format (den innehåller symbolen&quot;@&quot; och ett domännamn). Skapa en anpassad funktion som&quot;ValidateEmail&quot;, som tar e-postadressen som indata och returnerar true om den är giltig och i annat fall false.

```javascript
function ValidateEmail(inputText)
{
    var email = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
    if(inputText.value.match(email))
        {
            alert("Valid email address!");
            return true;
        }
    else
    {
        alert("Invalid email address!");
        return false;
    }
}
```

I ovanstående exempel anropas den anpassade funktionen &quot;ValidateEmail&quot; för att kontrollera om den angivna e-postadressen är giltig när användaren försöker skicka formuläret.

## Användning av anpassade funktioner {#uses-of-custom-function}

Fördelarna med att använda anpassade funktioner i Adaptive Forms är:

* **Ändring av data**: Anpassade funktioner hanterar och bearbetar data som anges i formulärfälten.
* **Validering av data**: Med anpassade funktioner kan du utföra anpassade kontroller av formulärindata och tillhandahålla angivna felmeddelanden.
* **Dynamiskt beteende**: Med anpassade funktioner kan du styra det dynamiska beteendet i dina formulär baserat på specifika villkor. Du kan till exempel visa/dölja fält, ändra fältvärden eller justera formulärlogiken dynamiskt.
* **Integration**: Du kan använda anpassade funktioner för att integrera med externa API:er eller tjänster. Det hjälper till att hämta data från externa källor, skicka data till externa Rest-slutpunkter eller utföra anpassade åtgärder baserade på externa händelser.

## JS-anteckningar som stöds

Kontrollera att den anpassade funktionen som du skriver åtföljs av `jsdoc` ovan, om du behöver en anpassad konfiguration och beskrivning. Det finns flera sätt att deklarera en funktion i `JavaScript,`, och med kommentarer kan du hålla reda på funktionerna. Mer information finns i [usejsdoc.org](https://jsdoc.app/).

`jsdoc` taggar som stöds:

* **Privat**
Syntax: `@private`
En privat funktion ingår inte som en anpassad funktion.

* **Namn**
Syntax: `@name funcName <Function Name>`
Du kan också `,` använda: `@function funcName <Function Name>` **eller** `@func` `funcName <Function Name>` .
  `funcName` är namnet på funktionen (inga blanksteg tillåts).
  `<Function Name>` är funktionens visningsnamn.

* **Medlem**
Syntax: `@memberof namespace`
Kopplar ett namnutrymme till funktionen.

* **Parameter**
Syntax: `@param {type} name <Parameter Description>`
Du kan också använda: `@argument` `{type} name <Parameter Description>` **eller** `@arg` `{type}` `name <Parameter Description>` .
Visar parametrar som används av funktionen. En funktion kan ha flera parametertaggar, en tagg för varje parameter i ordningen för förekomst.
  `{type}` representerar parametertyp. Tillåtna parametertyper är:

   1. string
   2. tal
   3. boolesk
   4. omfång

  Omfång används för att referera till fält i ett adaptivt formulär. När ett formulär använder lazy loading kan du använda `scope` för att komma åt dess fält. Du kan komma åt fält antingen när fälten har lästs in eller om fälten har markerats som globala.

  Alla andra parametertyper kategoriseras under någon av ovanstående. Ingen stöds inte. Välj en av typerna ovan. Typer är inte skiftlägeskänsliga. Blanksteg tillåts inte i parametern `name`. `<Parameter Descrption>` `<parameter>  can have multiple words. </parameter>`

* **Returtyp**
Syntax: `@return {type}`
Du kan också använda `@returns {type}` .
Lägger till information om funktionen, till exempel dess mål.
{type} representerar funktionens returtyp. Följande returtyper tillåts:

   1. string
   1. tal
   1. boolesk

  Alla andra returtyper kategoriseras under en av ovanstående. Ingen stöds inte. Välj en av typerna ovan. Returtyperna är inte skiftlägeskänsliga.

* **Detta**
Syntax: `@this currentComponent`

  Använd @this för att referera till den adaptiva formulärkomponenten som regeln är skriven för.

  Följande exempel baseras på fältvärdet. I följande exempel döljer regeln ett fält i formuläret. Delen `this` i `this.value` refererar till den underliggande adaptiva formulärkomponenten, som regeln är skriven för.

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
  >Kommentarer före anpassade funktioner används för sammanfattning. Sammanfattningen kan sträcka sig över flera rader tills en tagg påträffas. Begränsa storleken till en enda storlek om du vill ha en kortfattad beskrivning i regelbyggaren.

## Typer som stöds för funktionsdeklaration {#function-declaration-supported-types}

**Funktionssats**

```javascript
function area(len) {
    return len*len;
}
```

Den här funktionen ingår utan `jsdoc` kommentarer.

**Funktionsuttryck**

```javascript
var area;
//Some codes later
/** */
area = function(len) {
    return len*len;
};
```

**Funktionsuttryck och programsats**

```javascript
var b={};
/** */
b.area = function(len) {
    return len*len;
}
```

**Funktionsdeklaration som variabel**

```javascript
/** */
var x1,
    area = function(len) {
        return len*len;
    },
    x2 =5, x3 =true;
```

Begränsning: den anpassade funktionen väljer bara den första funktionsdeklarationen från variabellistan, om den används tillsammans. Du kan använda funktionsuttryck för varje deklarerad funktion.

**Funktionsdeklaration som objekt**

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

## Skapa anpassad funktion {#create-custom-function}

Så här skapar du en anpassad funktion:

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
1. Skapa en JavaScript-fil med namnet `functions.js` under mappen `js`
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

Du kan referera till följande [anpassade funktionsmapp](/help/forms/using/assets/customfunction.zip). Hämta och installera den här mappen i din AEM-instans.

Nu kan du använda den anpassade funktionen i ditt adaptiva formulär genom att lägga till klientbiblioteket.

## Lägga till klientbibliotek i ett adaptivt formulär{#use-custom-function}

När du har distribuerat klientbiblioteket till Forms CS-miljön kan du använda funktionerna i ditt adaptiva formulär. Lägga till klientbiblioteket i ditt adaptiva formulär

1. Öppna formuläret i redigeringsläge. Om du vill öppna ett formulär i redigeringsläge markerar du ett formulär och väljer **[!UICONTROL Edit]**.
1. Öppna innehållsläsaren och markera komponenten **[!UICONTROL Guide Container]** i det adaptiva formuläret.
1. Klicka på egenskapsikonen för Guide Container. Dialogrutan Adaptiv formulärbehållare öppnas.
1. Öppna fliken **[!UICONTROL Basic]** och välj namnet på **[!UICONTROL client library category]** i listrutan (välj i det här fallet `customfunctionscategory`).

   ![Lägger till klientbiblioteket för anpassade funktioner](/help/forms/using//assets/custom-function-category-name-core-component.png)

1. Klicka på **[!UICONTROL Done]** .

Nu kan du skapa en regel som använder anpassade funktioner i regelredigeraren:

![Lägger till klientbiblioteket för anpassade funktioner](/help/forms/using//assets/calculateage-customfunction.png)

Nu ska vi förstå hur du konfigurerar och använder en anpassad funktion med [tjänsten Invoke i regelredigeraren i AEM Forms](/help//forms/using/rule-editor.md).
