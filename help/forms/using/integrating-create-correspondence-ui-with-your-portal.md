---
title: Integrering av Create Correspondence Solution med din anpassade portal
description: Lär dig hur du integrerar ett gränssnitt för korrespondens med din anpassade portal
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
docset: aem65
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 496b125b-b091-4843-ba9f-2479dbeba07b
source-git-commit: 16f57ae1663f035d1dc39005d37426c7a0d8dc16
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---

# Integrering av `Create Correspondence`-lösningen med din anpassade portal{#integrating-create-correspondence-ui-with-your-custom-portal}

## Ökning {#overview}

I den här artikeln beskrivs hur du kan integrera `Create Correspondence`-lösningen med din miljö.

## URL-baserat anrop {#url-based-invocation}

Ett sätt att anropa programmet `Create Correspondence` från en anpassad portal är att förbereda URL:en med följande frågeparametrar:

* identifieraren för bokstavsmallen (med parametern cmLetterId).

* URL:en till XML-data som hämtats från den önskade datakällan (med parametern cmDataUrl).

Den anpassade portalen skulle till exempel förbereda URL:en som\
`https://'[server]:[port]'/[contextPath]/aem/forms/createcorrespondence.html?random=[timestamp]&cmLetterId=[letter identifier]&cmDataUrl=[data URL]`, som kan vara href från en länk på portalen.

>[!NOTE]
>
>Anrop på ett sådant sätt är inte säkert eftersom de nödvändiga parametrarna skickas som en GET-begäran, genom att samma (tydligt synliga) adress visas i URL-adressen.

>[!NOTE]
>
>Innan du anropar programmet `Create Correspondence` sparar och överför du data för att anropa användargränssnittet `Create Correspondence` på den angivna dataURL:en. Den här processen kan utföras antingen från den anpassade portalen eller genom en annan back end-process.

## Inline databaserat anrop {#inline-data-based-invocation}

Ett annat säkrare sätt att anropa programmet `Create Correspondence` är att gå till URL:en på https://&#39;[server]:[port]/[contextPath]/aem/forms/createcorrespondence.html. Kör den här URL:en när du skickar parametrar och data för att anropa programmet `Create Correspondence` som en POST-begäran, och dölja dem för slutanvändaren. Det här arbetsflödet innebär också att du nu kan skicka XML-data för det infogade programmet `Create Correspondence` (som en del av samma begäran, med parametern `cmData`). Det här arbetsflödet var inte möjligt eller idealiskt i det tidigare arbetssättet.

### Parametrar för att ange bokstav {#parameters-for-specifying-letter}

| **Namn** | **Typ** | **Beskrivning** |
| --- | --- | --- |
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
   <td>XML-data från en källfil med hjälp av grundläggande protokoll, t.ex. cq, ftp, http eller file.<br /> </td> 
  </tr>
  <tr>
   <td>cmLetterInstanceId</td> 
   <td>Sträng</td> 
   <td>Använda XML-data som är tillgängliga i bokstavsinstans.</td> 
  </tr>
  <tr>
   <td>cmUseTestData</td> 
   <td>Boolean</td> 
   <td>Om du vill återanvända testdata som är bifogade i en dataordlista.</td> 
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

Om du använder http- eller cq-protokoll för `cmDataURL` måste URL:en för `http/cq` vara anonym.
