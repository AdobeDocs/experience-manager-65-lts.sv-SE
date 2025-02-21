---
title: API för att anropa formulärdatamodelltjänst från anpassningsbara formulär
description: Beskriver det invokeWebServices-API som du kan använda för att anropa webbtjänster som skrivits i WSDL inifrån ett adaptivt formulärfält.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
discoiquuid: aa3e50f1-8f5a-489d-a42e-a928e437ab79
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---

# API för att anropa formulärdatamodelltjänst från anpassningsbara formulär {#api-to-invoke-form-data-model-service-from-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Ökning {#overview}

Med AEM Forms kan formulärförfattare ytterligare förenkla och förbättra ifyllandet av formulär genom att anropa tjänster som konfigurerats i en formulärdatamodell inifrån ett adaptivt formulärfält. Om du vill anropa en datamodelltjänst kan du antingen skapa en regel i den visuella redigeraren eller ange en JavaScript med `guidelib.dataIntegrationUtils.executeOperation`-API:t i kodredigeraren för [regelredigeraren](/help/forms/using/rule-editor.md).

Det här dokumentet fokuserar på att skriva en JavaScript med API:t `guidelib.dataIntegrationUtils.executeOperation` för att anropa en tjänst.

## Använda API {#using-the-api}

API:t `guidelib.dataIntegrationUtils.executeOperation` anropar en tjänst från ett adaptivt formulärfält. API-syntaxen är följande:

```javascript
guidelib.dataIntegrationUtils.executeOperation(operationInfo, inputs, outputs)
```

Strukturen för `guidelib.dataIntegrationUtils.executeOperation`-API:t anger information om tjänståtgärden. Strukturen har följande syntax.

```javascript
var operationInfo = {
formDataModelId,
operationTitle,
operationName
};
var inputs = {
inputField1,
inputFieldN
};
var outputs = {
outputField1,
outputFieldN
}
```

API-strukturen anger följande information om tjänståtgärden.

<table>
 <tbody>
  <tr>
   <th>Parameter</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td><code>operationInfo</code></td>
   <td>Struktur för att ange identifierare för formulärdatamodell, åtgärdstitel och åtgärdsnamn</td>
  </tr>
  <tr>
   <td><code>formDataModelId</code></td>
   <td>Anger databassökvägen till formulärdatamodellen inklusive dess namn</td>
  </tr>
  <tr>
   <td><code>operationName</code></td>
   <td>Anger namnet på den tjänståtgärd som ska köras</td>
  </tr>
  <tr>
   <td><code>inputs</code></td>
   <td>Kopplar ett eller flera formulärobjekt till indataargumenten för tjänståtgärden</td>
  </tr>
  <tr>
   <td><code>Outputs</code></td>
   <td>Mappar ett eller flera formulärobjekt till utdatavärden från tjänståtgärden för att fylla i formulärfält <br /> </td>
  </tr>
  <tr>
   <td><code>success</code></td>
   <td>Returnerar värden baserat på indataargumenten för serviceåtgärden. Det är en valfri parameter som används som en återanropsfunktion.<br /> </td>
  </tr>
  <tr>
   <td><code>failure</code></td>
   <td>Visar ett felmeddelande om återanropsfunktionen lyckas inte visa utdatavärden baserat på indataargumenten. Det är en valfri parameter som används som en återanropsfunktion.<br /> </td>
  </tr>
 </tbody>
</table>

## Exempelskript för att anropa en tjänst {#sample-script-to-invoke-a-service}

I följande exempelskript används API:t `guidelib.dataIntegrationUtils.executeOperation` för att anropa den `getAccountById`-tjänståtgärd som konfigurerats i formulärdatamodellen `employeeAccount`.

Åtgärden `getAccountById` tar värdet i formulärfältet `employeeID` som indata för argumentet `empId` och returnerar medarbetarens namn, kontonummer och kontosaldo för motsvarande medarbetare. Utdatavärdena fylls i i de angivna formulärfälten. Värdet i argumentet `name` fylls till exempel i i formulärelementet `fullName` och värdet för argumentet `accountNumber` i formulärelementet `account`.

```javascript
var operationInfo = {
"formDataModelId": "/content/dam/formsanddocuments-fdm/employeeAccount",
"operationName": "getAccountDetails"
};
var inputs = {
"empid" : employeeID
};
var outputs = {
"name" : fullName,
"accountNumber" : account,
"balance" : balance
};
guidelib.dataIntegrationUtils.executeOperation(operationInfo, inputs, outputs);
```

## Använda API:t med återanropsfunktionen {#using-the-api-callback}

Du kan också anropa datamodelltjänsten för formulär med API:t `guidelib.dataIntegrationUtils.executeOperation` med en callback-funktion. API-syntaxen är följande:

```javascript
guidelib.dataIntegrationUtils.executeOperation(operationInfo, inputs, outputs, callbackFunction)
```

Återanropsfunktionen kan ha callback-funktionerna `success` och `failure`.

### Exempelskript med återanropsfunktioner för lyckade och misslyckade åtgärder {#callback-function-success-failure}

I följande exempelskript används API:t `guidelib.dataIntegrationUtils.executeOperation` för att anropa den `GETOrder`-tjänståtgärd som konfigurerats i formulärdatamodellen `employeeOrder`.

Åtgärden `GETOrder` tar värdet i formulärfältet `Order ID` som indata för argumentet `orderId` och returnerar orderkvantitetsvärdet i callback-funktionen `success`.  Om återanropsfunktionen `success` inte returnerar ordningsantalet visar återanropsfunktionen `failure` meddelandet `Error occured`.

>[!NOTE]
>
>Om du använder callback-funktionen `success` fylls inte utdatavärdena i de angivna formulärfälten.

```javascript
var operationInfo = {
    "formDataModelId": "/content/dam/formsanddocuments-fdm/employeeOrder",
    "operationTitle": "GETOrder",
    "operationName": "GETOrder"
};
var inputs = {
    "orderId" : Order ID
};
var outputs = {};
var success = function (wsdlOutput, textStatus, jqXHR) {
order_quantity.value = JSON.parse(wsdlOutput).quantity;
 };
var failure = function(){
alert('Error occured');
};
guidelib.dataIntegrationUtils.executeOperation(operationInfo, inputs, outputs, success, failure);
```
