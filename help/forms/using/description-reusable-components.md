---
title: Beskrivning av återanvändbara komponenter
description: En fullständig lista över återanvändbara komponenter med filnamn och beroenden som hjälper dig att integrera arbetsytekomponenten i AEM Forms i dina webbprogram.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: 8ecb0f5a-e11a-4371-8136-5db8c98c6043
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 0%

---

# Beskrivning av återanvändbara komponenter {#description-of-reusable-components}

AEM Forms arbetsyta består av [återanvändbara](/help/forms/using/integrating-html-ws-components-web.md) komponenter som är ordnade i en viss [mappstruktur](/help/forms/using/folder-structure.md) i CRX™. Varje komponent har modell-, vy- och mallfil på den plats som anges i mappstrukturen, JavaScript™-beroenden på andra komponentfiler, händelser som avlyssnas av komponenten och JavaScript-objekt som utlöser dessa händelser på AEM Forms arbetsyta. En fullständig lista över återanvändbara komponenter med komponentens filnamn och beroenden anges här.

## AktivitetLista {#tasklist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>tasklist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>tasklist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>tasklist.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td>
    <ul>
     <li><p>UserSearch</p></li>
     <li><p>Uppgift</p></li>
     <li><p>Teamaktivitet</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td>
    <ul>
     <li><p>aktivitetsmodell</p></li>
     <li><p>gruppaktivitetsmodell</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>filterSelected - aktivitetslistmodell</p></li>
     <li><p>remove - aktivitetslistmodell</p></li>
     <li><p>updateQueue - aktivitetslista, modell</p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Den här komponenten kan användas oberoende av AEM Forms arbetsyta, förutsatt att du utlöser händelsen filterSelected för den här komponenten från ditt anpassade program.

## Uppgift {#task}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>task.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>task.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>task.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td>
    <ul>
     <li><p>aktivitetslistmodell</p></li>
     <li><p>uppgifterfunktionsmakron</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>submitComplete - aktivitetsmodell</p></li>
     <li><p>Avvisa - aktivitetsmodell</p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Workspace anropar funktionen fetchTasks i TaskList-modellen för att skapa aktivitetsmodeller för den här komponenten.

## FilterList {#filterlist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>tasklist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>filterlist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>filterlist.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>hämtad - aktivitetslistmodell </p></li>
     <li><p>remove - aktivitetslistmodell </p></li>
     <li><p>updateQueue - aktivitetslista, modell </p></li>
     <li><p>uppdateradKö - aktivitetslistmodell </p></li>
     <li><p>filterSelected - aktivitetslistmodell</p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

## Filter {#filter}

<table>
 <tbody>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>filter.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>filter.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td>
    <ul>
     <li><p>Fält: kö: { name, qid, isDefault, type}</p> </li>
     <li><p>Fält: fråga: sträng</p> </li>
     <li><p>Fält: parentView: filterlistvy</p> </li>
     <li><p>Fält: parentModel: aktivitetslistmodell</p> </li>
     <li><p>Fält: utility</p> </li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>Händelser som lyssnats</p> </td>
   <td><p>NA</p> </td>
  </tr>
 </tbody>
</table>

## TeamQueues {#teamqueues}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>tasklist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>teamqueues.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>teamqueues.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>hämtad - aktivitetslistmodell </p></li>
     <li><p>remove - aktivitetslistmodell </p></li>
     <li><p>updateQueue - aktivitetslista, modell </p></li>
     <li><p>teamQueuesFetched - tasklist model </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

## TeamFilter {#teamfilter}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>teamfilter.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>teamfilter.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td>
    <ul>
     <li><p>Utökar: filtervy</p> </li>
     <li><p>Fält : queue :{ name, qid, isDefault, type }</p> </li>
     <li><p>Field : query : string</p> </li>
     <li><p>Field : parentView : filterlist view</p> </li>
     <li><p>Fält : parentModel : aktivitetslistmodell</p> </li>
     <li><p>Field : utility</p> </li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>Händelser som lyssnats</p> </td>
   <td><p>NA</p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>TeamFilter hämtar händelsen som anger vilken aktivitet som har valts från TaskList-komponenten. Även om dessa komponenter delar modellklassen finns det inget annat beroende.

## AktivitetInformation {#taskdetails}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>tasklist.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>taskdetails.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>taskdetails.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>De flesta verktygsklasserna</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td>
    <ul>
     <li><p>processinstancehistory.html</p> </li>
     <li><p>formgivningsverktyg</p> </li>
     <li><p>anteckningsverktyg</p> </li>
     <li><p>verktyg för bilagor</p> </li>
     <li><p>uppgifterfunktionsmakron</p> </li>
     <li><p>historikverktyg</p> </li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td>
    <ul>
     <li><p>vidarebefordrad - aktivitetsmodell</p> </li>
     <li><p>delad - aktivitetsmodell</p> </li>
     <li><p>konsulteras - aktivitetsmodell</p> </li>
     <li><p>avvisad - aktivitetsmodell</p> </li>
     <li><p>övergiven - aktivitetsmodell</p> </li>
     <li><p>olåst - aktivitetsmodell</p> </li>
     <li><p>låst - aktivitetsmodell</p> </li>
     <li><p>anspråk - aktivitetsmodell</p> </li>
     <li><p>ändring:aktivitet markerad - aktivitetslistmodell</p> </li>
     <li><p>change:formUrl - aktivitetsmodell</p> </li>
     <li>attachmentURLFetched - task model</li>
    </ul>
    <ul>
     <li>newAttachment - aktivitetsmodell</li>
     <li><p>taskHistoryFetched - aktivitetsmodell</p> </li>
     <li>prepareForSubmitComplete - aktivitetsmodell</li>
     <li><p>submitComplete - aktivitetsmodell</p> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## CategoryList {#categorylist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>categorylist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>categorylist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>startprocess.html (i vägmappen)</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>Kategori</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td>
    <ul>
     <li><p>favoritkategoristandardmodell</p></li>
     <li><p>allccategoryFactory-modell</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>allStartPointsFetched - kategorilistmodell </p></li>
     <li><p>add - kategorilistmodell </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Den här komponenten använder modellklasser för vissa andra komponenter som StartPointList, StartPoint och Task. Förutom detta beroende kan CategoryList användas oberoende.

## Kategori {#category}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>category.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>category.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>category.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td>
    <ul>
     <li><p>kategorilistmodell</p></li>
     <li><p>startpointlistmodell</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>ändrad - kategorimodell </p></li>
     <li><p>childrenFetched - category model </p></li>
     <li><p>kategori:markerad - kategorilistmodell </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

## StartPointList {#startpointlist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>categorylist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>startpointlist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>startprocess.html (i vägmappen)</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td>
    <ul>
     <li><p>kategorimodell</p></li>
     <li><p>favoritkategoristandardmodell</p></li>
     <li><p>allccategoryFactory-modell</p></li>
     <li><p>startpunktsvy</p></li>
     <li><p>startpointlistmodell</p></li>
     <li><p>startpunktsmodell</p></li>
     <li><p>aktivitetsmodell</p></li>
     <li><p>aktivitetsmodell</p></li>
     <li><p>aktivitetslistmodell</p></li>
     <li><p>gruppaktivitetsmodell</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>kategori:markerad - kategorilistmodell </p></li>
     <li><p>allStartPointsFetched - kategorilistmodell </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Komponenterna StartPointList och CategoryList delar modellklassen, och den första är beroende av den senare. CategoryList får åtkomst till informationen om vilken kategori startpunkter som visas. Om du vill använda StartPointList separat simulerar du händelseutlösaren från CategoryList.

## StartPoint {#startpoint}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>startpoint.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>startpoint.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>startpoint.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>aktivitetsmodell</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td><p>change - startpunktsmodell </p></td>
  </tr>
 </tbody>
</table>

## StartProcess {#startprocess}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>categorylist.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>startprocess.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>startprocess.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td>
    <ul>
     <li><p>De flesta verktygsklasserna</p> </li>
     <li><p>UserSearch</p> </li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td>
    <ul>
     <li><p>kategorimodell</p> </li>
     <li><p>favoritkategoristandardmodell</p> </li>
     <li><p>allccategoryFactory-modell</p> </li>
     <li><p>formgivningsverktyg</p> </li>
     <li><p>anteckningsverktyg</p> </li>
     <li><p>verktyg för bilagor</p> </li>
     <li><p>uppgifterfunktionsmakron</p> </li>
    </ul> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td>
    <ul>
     <li><p>kategori:markerad - kategorilistmodell</p> </li>
     <li><p>change:invokedTask - startpunktslistmodell</p> </li>
     <li><p>change:formUrl - aktivitetsmodell</p> </li>
     <li><p>startpunkt:markerad - startpunktslistmodell</p> </li>
     <li><p>vidarebefordrad - aktivitetsmodell</p> </li>
     <li><p>övergiven - aktivitetsmodell</p> </li>
     <li><p>olåst - aktivitetsmodell</p> </li>
     <li><p>låst - aktivitetsmodell</p> </li>
     <li>attachmentURLFetched - task model</li>
     <li>newAttachment - aktivitetsmodell</li>
     <li>prepareForSubmitComplete - aktivitetsmodell </li>
     <li><p>submitComplete - aktivitetsmodell</p> </li>
     <li><p>allStartPointsFetched - kategorilistmodell</p> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Komponenterna StartProcess och StartPointList delar modellklassen. Den här komponenten blir relevant när du väljer en startpunkt från StartPointList.

## ProcessNameList {#processnamelist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>processnamelist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>processnamelist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>tracking.html (i vägmappen)</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>processnamnsmodell</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>add - processnamelist-modell </p></li>
     <li><p>hämtad:processnamn - modell för processnamnlista </p></li>
     <li><p>change - process namelist model </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>ProcessNameList är inte beroende av andra komponenter. Internt är det emellertid beroende av modellklassen ProcessInstanceList som i sin tur är beroende av andra komponenter. ProcessNameList använder därför många modellklasser som ProcessInstanceList, ProcessInstance, TaskList, Teamtask och Task. Förutom dessa beroenden kan ProcessNameList användas oberoende av varandra.

## ProcessName {#processname}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>processname.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>processnamn (i processnamelist.js)</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>processname.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>processinstancelist, modell</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td><p>change - process name model </p></td>
  </tr>
 </tbody>
</table>

## ProcessInstanceList {#processinstancelist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>processnamelist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>processinstancelist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>tracking.html (i vägmappen)</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>processnamnsmodell</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>processnamn:valt - modell för processnamnlista </p></li>
     <li><p>processnamn:instanshämtad - processnamelist-modell </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>ProcessInstanceList förväntar sig en händelse från ProcessNameList som anger processnamnet för hämtning och visning av instanser. Om du vill använda ProcessInstanceList separat simulerar du händelseutlösaren separat.

## ProcessInstance {#processinstance}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>processinstance.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>processnamn inuti processnamelist.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>processinstance.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>aktivitetslistmodell</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td><p>change - processinstansmodell </p></td>
  </tr>
 </tbody>
</table>

## ProcessInstanceHistory {#processinstancehistory}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>processnamelist.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>processinstancehistory.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>processinstancehistory.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td>
    <ul>
     <li><p>processnamnsmodell</p></li>
     <li><p>historikverktyg</p></li>
    </ul></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>processnamn:valt - modell för processnamnlista </p></li>
     <li><p>processinstans:selected - processinstanslistmodell </p></li>
     <li><p>tasksFetched - processinstansmodell </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>ProcessInstanceHistory förväntar sig en händelse från ProcessInstanceList som anger vilken processinstansens historik som ska visas. Förutom detta beroende kan komponenten användas oberoende av varandra.

## OutofOffice {#outofoffice}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>outofoffice.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>outofoffice.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>outofoffice.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>UserSearch</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td><p>användarsökvy</p> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td>
    <ul>
     <li><p>outOfOfficeSettingsFetched - outOffice-modell</p> </li>
     <li><p>outOfOfficeSettingsSaved - kontorsmodell</p> </li>
     <li><p>processesFetched - opacitetsmodell</p> </li>
     <li><p>palSelected - vyn för huvudsökning</p> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>OutofOffice kan användas oberoende av varandra.

## ShareQueue {#sharequeue}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>sharequeue.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>sharequeue.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>sharequeue.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>UserSearch</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td><p>användarsökvy</p> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td>
    <ul>
     <li><p>queueAccessGranted - sharequeue-modell</p> </li>
     <li><p>queueAccessRequested - sharequeue-modell</p> </li>
     <li><p>grantedUsersFetched - sharequeue model</p> </li>
     <li>accessibleUsersFetched - sharequeue model</li>
     <li><p>queueAccessRevoked - sharequeue-modell</p> </li>
     <li><p>queueAccessRemoved - sharequeue-modell</p> </li>
     <li><p>palSelected - vyn för huvudsökning</p> </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>ShareQueue kan användas oberoende av varandra.

## UISettings {#uisettings}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>uisettings.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>uisettings.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>uisettings.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td>
    <ul>
     <li><p>preferencesFetched - Usettings model </p></li>
     <li><p>settingUpdated - uissettings model </p></li>
    </ul></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>UISettings kan användas oberoende av varandra.

## AppNavigation {#appnavigation}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>appnavigation.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>appnavigation.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>appnavigation.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>Händelser som lyssnats</p></td>
   <td><p>NA</p></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>AppNavigation kan användas oberoende av varandra.

## UserInfo {#userinfo}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>userinfo.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>userinfo.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>userinfo.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td>
    <ul>
     <li>userImageUrlFetched - userinfo model</li>
     <li>sessionRenewed - userinfo model <br /> </li>
     <li>sessionExpired - userinfo-modell </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>UserInfo kan användas oberoende av varandra.

## WSError {#wserror}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p></td>
   <td><p>wserror.js</p></td>
  </tr>
  <tr>
   <td><p>Visa</p></td>
   <td><p>wserror.js</p></td>
  </tr>
  <tr>
   <td><p>Mall</p></td>
   <td><p>wserror.html</p></td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p></td>
   <td><p>NA</p></td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p></td>
   <td><p>newWsError - serverfelmodell </p></td>
  </tr>
 </tbody>
</table>

## UserSearch {#usersearch}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>usersearch.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>usersearch.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>usersearch.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td>
    <ul>
     <li>palSearched - principalsearch model</li>
     <li>outOfOfficeInfoFetched - usersearch model</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## SearchTemplate {#searchtemplate}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>searchtemplate.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>searchtemplate (in searchtemplatelist.js) </p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>searchtemplate.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td><p>templateFetched- searchtemplate model</p> </td>
  </tr>
 </tbody>
</table>

## SearchTemplateList {#searchtemplatelist}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>searchtemplatelist.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>searchtemplatelist.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>tracking.html (i vägmappen)</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td><p>sökmallsmodell</p> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td><p>change - searchtemplateList model</p> </td>
  </tr>
 </tbody>
</table>

## SearchTemplateDetails {#searchtemplatedetails}

<table>
 <tbody>
  <tr>
   <td><p>Modell</p> </td>
   <td><p>searchtemplatelist.js</p> </td>
  </tr>
  <tr>
   <td><p>Visa</p> </td>
   <td><p>searchtemplatedetails.js</p> </td>
  </tr>
  <tr>
   <td><p>Mall</p> </td>
   <td><p>searchtemplatedetails.html</p> </td>
  </tr>
  <tr>
   <td><p>Kräver komponenter</p> </td>
   <td><p>NA</p> </td>
  </tr>
  <tr>
   <td><p>JS-beroenden</p> </td>
   <td>NA<br /> </td>
  </tr>
  <tr>
   <td><p>Lyssnade händelser (händelsenamn - utlösare)</p> </td>
   <td><p>sökmall:markerad - sökmallsmodell</p> </td>
  </tr>
 </tbody>
</table>
