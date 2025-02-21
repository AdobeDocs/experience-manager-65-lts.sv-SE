---
title: Integrera Create Correspondence UI med din anpassade portal
description: Lär dig hur du integrerar ett gränssnitt för korrespondens med din anpassade portal
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
docset: aem65
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Integrera Create Correspondence UI med din anpassade portal{#integrating-create-correspondence-ui-with-your-custom-portal}

## Ökning {#overview}

I den här artikeln beskrivs hur du kan integrera Create Correspondence Solution med din miljö.

## URL-baserat anrop {#url-based-invocation}

Ett sätt att anropa programmet Create Correspondence från en anpassad portal är att förbereda URL:en med följande frågeparametrar:

* identifieraren för bokstavsmallen (med parametern cmLetterId).

* URL:en till XML-data som hämtats från den önskade datakällan (med parametern cmDataUrl).

Den anpassade portalen skulle till exempel förbereda URL:en som\
`https://'[server]:[port]'/[contextPath]/aem/forms/createcorrespondence.html?random=[timestamp]&cmLetterId=[letter identifier]&cmDataUrl=[data URL]`, som kan vara href från en länk på portalen.

>[!NOTE]
>
>Anrop på ett sådant sätt är inte säkert eftersom de nödvändiga parametrarna skickas som en GET-begäran, genom att samma (tydligt synliga) adress visas i URL-adressen.

>[!NOTE]
>
>Innan du anropar programmet Create Correspondence sparar och överför du data för att anropa användargränssnittet Create Correspondence på angiven dataURL. Detta kan antingen göras från den anpassade portalen eller genom en annan back end-process.

## Inline databaserat anrop {#inline-data-based-invocation}

Ett annat (och säkrare) sätt att anropa programmet Create Correspondence kan vara att bara trycka på URL:en på https://&#39;[server]:[port]/[contextPath]/aem/forms/createcorrespondence.html medan du skickar parametrar och data för att anropa programmet Create Correspondence som en POST-begäran (dölja dem för slutanvändaren). Det innebär också att du nu kan skicka XML-data för Create Correspondence-programmet (som en del av samma begäran, med parametern cmData), vilket inte var möjligt/idealiskt i den tidigare metoden.

### Parametrar för att ange bokstav {#parameters-for-specifying-letter}

| **Namn** | **Typ** | **Beskrivning** |
|---|---|---|
| cmLetterInstanceId | Sträng | Bokstavsinstansens identifierare. |
| cmLetterId | Sträng | Namnet på brevmallen. |

Parametrarnas ordning i tabellen anger inställningarna för parametrar som används för att läsa in bokstaven.

### Parametrar för att ange XML-datakällan {#parameters-for-specifying-the-xml-data-source}

<table>
 <tbody>
  <tr>
   <td><strong>Namn</strong></td> 
   <td><strong>Typ</strong></td> 
   <td><strong>Beskrivning</strong></td> 
  </tr>
  <tr>
   <td>cmDataUrl<br /> </td> 
   <td>URL</td> 
   <td>XML-data från en källfil med hjälp av grundläggande protokoll som cq, ftp, http eller file.<br /> </td> 
  </tr>
  <tr>
   <td>cmLetterInstanceId</td> 
   <td>Sträng</td> 
   <td>Använda XML-data som är tillgängliga i Letter Instance.</td> 
  </tr>
  <tr>
   <td>cmUseTestData</td> 
   <td>Boolean</td> 
   <td>Om du vill återanvända testdata som bifogats i dataordlistan.</td> 
  </tr>
 </tbody>
</table>

Parametrarnas ordning i tabellen anger inställningarna för de parametrar som används för att läsa in XML-data.

### Andra parametrar {#other-parameters}

<table>
 <tbody>
  <tr>
   <td><strong>Namn</strong></td> 
   <td><strong>Typ</strong></td> 
   <td><strong>Beskrivning</strong></td> 
  </tr>
  <tr>
   <td>cmPreview<br /> </td> 
   <td>Boolean</td> 
   <td>True to open the letter in preview mode<br /> </td> 
  </tr>
  <tr>
   <td>Slumpmässig</td> 
   <td>Tidsstämpel</td> 
   <td>Lös problem med webbläsarcachning.</td> 
  </tr>
 </tbody>
</table>

Om du använder http- eller cq-protokoll för cmDataURL bör URL:en för http/cq vara anonym.
