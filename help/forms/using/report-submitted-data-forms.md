---
title: API:er för att arbeta med inskickade formulär på formulärportalen
description: AEM Forms tillhandahåller API:er som du kan använda för att fråga efter och vidta åtgärder för skickade formulärdata i formulärportalen.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish, developer-reference
feature: Forms Portal
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 1%

---

# API:er för att arbeta med inskickade formulär på formulärportalen {#apis-to-work-with-submitted-forms-on-forms-portal}

AEM Forms tillhandahåller API:er som du kan använda för att fråga efter formulärdata som skickas via formulärportalen. Dessutom kan du publicera kommentarer eller uppdatera egenskaper för skickade formulär med de API:er som beskrivs i det här dokumentet.

>[!NOTE]
>
>Användare som ska anropa API:erna måste läggas till i granskningsgruppen enligt beskrivningen i [Associera granskare som skickar in till ett formulär](/help/forms/using/adding-reviewers-form.md).

## GET /content/forms/portal/submission.review.json?func=getFormsForSubmissionReview {#get-content-forms-portal-submission-review-json-func-getformsforsubmissionreview-br}

Returnerar en lista med alla giltiga formulär.

### URL-parametrar {#url-parameters}

Detta API kräver inga ytterligare parametrar.

### Svar {#response}

Svarsobjektet innehåller en JSON-array som innehåller formulärnamn och databassökväg. Responsens struktur är följande:

```json
[
 {formName: "<form name>",
 formPath: "<path to the form>" },
 {.....},
 ......]
```

### Exempel {#example}

**Begär URL**

```http
https://[host]:[port]/content/forms/portal/submission.review.json?func=getFormsForSubmissionReview
```

**Svar**

```json
[{"formPath":"/content/dam/formsanddocuments/forms-review/form2","formName":"form2"},{"formPath":"/content/dam/formsanddocuments/forms-review/form1","formName":"form1"}]
```

## GET /content/forms/portal/submission.review.json?func=getAllSubmissions {#get-content-forms-portal-submission-review-json-func-getallsubmissions}

Returnerar information om alla skickade formulär. Du kan dock använda URL-parametrar för att begränsa resultaten.

### URL-parametrar {#url-parameters-1}

Ange följande parametrar i begärande-URL:

<table>
 <tbody>
  <tr>
   <th>Parameter</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td><code>formPath</code></td>
   <td>Anger sökvägen till CRX-databasen där formuläret finns. Om du inte anger formulärsökvägen returneras ett tomt svar.<br /> </td>
  </tr>
  <tr>
   <td><code>offset</code><br /> (valfritt)</td>
   <td>Anger startpunkten i resultatuppsättningens index. Standardvärdet är <strong>0</strong>.</td>
  </tr>
  <tr>
   <td><code>limit</code><br /> (valfritt)</td>
   <td>Begränsar antalet resultat. Standardvärdet är <strong>30</strong>.</td>
  </tr>
  <tr>
   <td><code>orderby</code> <br /> (valfritt)</td>
   <td>Anger egenskapen för sorteringsresultat. Standardvärdet är <strong>jcr:lastModified</strong>, som sorterar resultat baserat på den senaste ändringstiden.</td>
  </tr>
  <tr>
   <td><code>sort</code> <br /> (valfritt)</td>
   <td>Anger sorteringsordningen för resultat. Standardvärdet är <strong>desc</strong>, vilket sorterar resultatet i fallande ordning. Du kan ange <code>asc</code> om du vill sortera resultaten i stigande ordning.</td>
  </tr>
  <tr>
   <td><code>cutPoints</code> <br /> (valfritt)</td>
   <td>Anger en kommaavgränsad lista med formuläregenskaper som ska inkluderas i resultaten. Standardegenskaperna är: <br /> <code>formName</code>, <code>formPath</code>, <code>submitID</code>, <code>formType</code>, <code>jcr:lastModified</code>, <code>owner</code></td>
  </tr>
  <tr>
   <td><code>search</code> <br /> (valfritt)</td>
   <td>Söker efter det angivna värdet i formuläregenskaper och returnerar formulär med matchande värden. Standardvärdet är <strong> </strong>.</td>
  </tr>
 </tbody>
</table>

### Svar {#response-1}

Svarsobjektet innehåller en JSON-array som innehåller information om de angivna formulären. Responsens struktur är följande:

```json
{
 total: "<total number of submissions>",
 items: [{ formName: "<name of the form>", formPath: "<path to the form>", owner: "<owner of the form>"},
 ....]}
```

### Exempel {#example-1}

**Begär URL**

```http
https://[host]:[port]/content/forms/portal/submission.review.json?func=getAllSubmissions&formPath=/content/dam/formsanddocuments/forms-review/form2
```

**Svar**

```json
{"total":1,"items":[{"formName":"form2","formPath":"/content/dam/formsanddocuments/forms-review/form2","submitID":"1403037413508500","formType":"af","jcr:lastModified":"2015-11-05T17:52:32.243+05:30","owner":"admin"}]}
```

## POST /content/forms/portal/submission.review.json?func=addComment {#post-content-forms-portal-submission-review-json-func-addcomment-br}

Lägger till en kommentar i den angivna skicka-instansen.

### URL-parametrar {#url-parameters-2}

Ange följande parametrar i begärande-URL:

| Parameter | Beskrivning |
|---|---|
| `submitID` | Anger det metadata-ID som är associerat med en överföringsinstans. |
| `Comment` | Anger den text som ska läggas till i den angivna överföringsinstansen. |

### Svar {#response-2}

Returnerar ett kommentar-ID när en kommentar har publicerats.

### Exempel {#example-2}

**Begär URL**

```http
https://[host:'port'/content/forms/portal/submission.review.json?func=addComment&submitID=1403037413508500&comment=API+test+comment
```

**Svar**

```java
1403873422601300
```

## GET /content/forms/portal/submission.review.json?func=getComments   {#get-content-forms-portal-submission-review-json-func-getcomments-nbsp}

Returnerar alla kommentarer som har publicerats på den angivna inskickningsinstansen.

### URL-parametrar {#url-parameters-3}

Ange följande parameter i begärande-URL:

| Parameter | Beskrivning |
|---|---|
| `submitID` | Anger metadata-ID för en skickningsinstans. |

### Svar {#response-3}

Svarsobjektet innehåller en JSON-array som innehåller alla kommentarer som är associerade med det angivna överförings-ID:t. Responsens struktur är följande:

```json
[{
 owner: "<name of the commenter>",
 comment: "<comment text>",
 time: "<time when the comment was posted>"},
 { }......]
```

### Exempel {#example-3}

**Begär URL**

```http
https://[host]:'port'/content/forms/portal/submission.review.json?func=getComments&submitID=1403037413508500
```

**Svar**

```java
[{"owner":"fr1","comment":"API test comment","time":1446726988250}]
```

## POST /content/forms/portal/submission.review.json?func=updateSubmission {#post-content-forms-portal-submission-review-json-func-updatesubmission-br}

Uppdaterar värdet för den angivna egenskapen i den angivna formulärinstansen.

### URL-parametrar {#url-parameters-4}

Ange följande parametrar i begärande-URL:

| Parameter | Beskrivning |
|---|---|
| `submitID` | Anger det metadata-ID som är associerat med en överföringsinstans. |
| `property` | Anger den formuläregenskap som ska uppdateras. |
| `value` | Anger värdet på den formuläregenskap som ska uppdateras. |

### Svar {#response-4}

Returnerar ett JSON-objekt med information om den publicerade uppdateringen.

### Exempel {#example-4}

**Begär URL**

```http
https://[host]:'port'/content/forms/portal/submission.review.json?func=updateSubmission&submitID=1403037413508500&value=sample_value&property=some_new_prop
```

**Svar**

```json
{"formName":"form2","owner":"admin","jcr:lastModified":1446727516593,"path":"/content/forms/fp/admin/submit/metadata/1403037413508500.html","submitID":"1403037413508500","status":"submitted"}
```
