---
title: Anpassa spårningstabeller
description: Anpassa visningen av information om användarprocesser i uppgiftstabellen som visas på fliken Spårning på arbetsytan i AEM Forms.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 27589617-4020-490c-b01e-1b169ae0801a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Anpassa spårningstabeller{#customize-tracking-tables}

Fliken Spårning på arbetsytan i AEM Forms används för att visa information om processinstanser där den inloggade användaren är involverad. Om du vill visa spårningstabellerna börjar du med att markera ett processnamn i den vänstra rutan för att visa dess lista med instanser i den mittersta rutan. Markera en processinstans om du vill visa en tabell med uppgifter som genereras av den här instansen i den högra rutan. Som standard visar tabellkolumnerna följande uppgiftsattribut (motsvarande attribut i aktivitetsmodellen anges inom parentes):

* ID ( `taskId`)
* Namn ( `stepName`)
* Instruktioner ( `instructions`)
* Markerad åtgärd ( `selectedRoute`)
* Skapad ( `createTime`)
* Slutförandetid ( `completeTime`)
* Ägare ( `currentAssignment.queueOwner`)

De återstående attributen i aktivitetsmodellen som är tillgängliga för visning i aktivitetstabellen är:

<table>
 <tbody>
  <tr>
   <td><p>actionInstanceId</p> </td>
   <td><p>isOpenFullScreen</p> </td>
   <td><p>reminderCount</p> </td>
  </tr>
  <tr>
   <td><p>classOfTask</p> </td>
   <td><p>isOwner</p> </td>
   <td><p>routeList</p> </td>
  </tr>
  <tr>
   <td><p>SeeGroupId</p> </td>
   <td><p>isRouteSelectionRequired</p> </td>
   <td><p>savedFormCount</p> </td>
  </tr>
  <tr>
   <td><p>contentType</p> </td>
   <td><p>isShowAttachments</p> </td>
   <td><p>serializedImageTicket</p> </td>
  </tr>
  <tr>
   <td><p>createTime</p> </td>
   <td><p>isStartTask</p> </td>
   <td><p>serviceName</p> </td>
  </tr>
  <tr>
   <td><p>creationId</p> </td>
   <td><p>isVisible</p> </td>
   <td><p>serviceTitle</p> </td>
  </tr>
  <tr>
   <td><p>currentAssignment</p> </td>
   <td><p>nextReminder</p> </td>
   <td><p>showACLAactions</p> </td>
  </tr>
  <tr>
   <td><p>deadline</p> </td>
   <td><p>numForms</p> </td>
   <td><p>showDirectActions</p> </td>
  </tr>
  <tr>
   <td><p>description</p> </td>
   <td><p>numFormsToBeSaved</p> </td>
   <td><p>status</p> </td>
  </tr>
  <tr>
   <td><p>displayName</p> </td>
   <td><p>outOfOfficeUserId</p> </td>
   <td><p>summaryUrl</p> </td>
  </tr>
  <tr>
   <td><p>forwardGroupId</p> </td>
   <td><p>outOfOfficeUserName</p> </td>
   <td><p>supportsSave</p> </td>
  </tr>
  <tr>
   <td><p>isApprovalUI</p> </td>
   <td><p>prioritet</p> </td>
   <td><p>taskACL</p> </td>
  </tr>
  <tr>
   <td><p>isCustomUI</p> </td>
   <td><p>processInstanceId</p> </td>
   <td><p>taskFormType</p> </td>
  </tr>
  <tr>
   <td><p>isDefaultImage</p> </td>
   <td><p>processInstanceStatus</p> </td>
   <td><p>taskUserInfo</p> </td>
  </tr>
  <tr>
   <td><p>isLocked</p> </td>
   <td><p>processVariables</p> </td>
   <td> </td>
  </tr>
  <tr>
   <td><p>isMustOpenToComplete</p> </td>
   <td><p>readerSubmitOptions</p> </td>
   <td> </td>
  </tr>
 </tbody>
</table>

För följande anpassningar i uppgiftstabellen måste du göra semantiska ändringar i källkoden. Se [Introduktion till anpassning av AEM Forms-arbetsyta](/help/forms/using/introduction-customizing-html-workspace.md) för hur du kan göra semantiska ändringar med arbetsytan SDK och skapa ett minifierat paket från den ändrade källan.

## Ändra tabellkolumner och deras ordning {#changing-table-columns-and-their-order}

1. Om du vill ändra uppgiftsattributen som visas i tabellen och deras ordning konfigurerar du filen /ws/js/runtime/templates/processinstancehistory.html :

   ```html
   <table>
       <thead>
           <tr>
               <!-- put the column headings in order here, for example-->
               <th><%= $.t('history.fixedTaskTableHeader.taskName')%></th>
               <th><%= $.t('history.fixedTaskTableHeader.taskInstructions')%></th>
               <th><%= $.t('history.fixedTaskTableHeader.taskRoute')%></th>
               <th><%= $.t('history.fixedTaskTableHeader.taskCreateTime')%></th>
               <th><%= $.t('history.fixedTaskTableHeader.taskCompleteTime')%></th>
           </tr>
       </thead>
   </table>
   ```

   ```html
   <table>
       <tbody>
           <%_.each(obj, function(task){%>
           <tr>
               <!-- Put the task attributes in the order of headings, for example, -->
               <td><%= task.stepName %></td>
               <td><%= task.instructions %></td>
               <td><%= !task.selectedRoute?'':(task.selectedRoute=='null'?'Default':task.selectedRoute) %></td>
               <td><%= task.createTime?task.formattedCreateTime:'' %></td>
               <td><%= task.completeTime? task.formattedCompleteTime:'' %></td>
           </tr>
           <%});%>
       </tbody>
   </table>
   ```

## Sortera en spårningstabell {#sorting-a-tracking-table}

Så här sorterar du uppgiftslisttabellen när du klickar på kolumnrubriken:

1. Registrera en klickhanterare för `.fixedTaskTableHeader th` i filen `js/runtime/views/processinstancehistory.js`.

   ```javascript
   events: {
       //other handlers
       "click .fixedTaskTableHeader th": "onTaskTableHeaderClick",
       //other handlers
   }
   ```

   Anropa funktionen `onTaskTableHeaderClick` för `js/runtime/util/history.js` i hanteraren.

   ```javascript
   onTaskTableHeaderClick: function (event) {
           history.onTaskTableHeaderClick(event);
   }
   ```

1. Visa metoden `TaskTableHeaderClick` i `js/runtime/util/history.js`.

   Metoden hittar uppgiftsattributet från click-händelsen, sorterar aktivitetslistan för det attributet och återger uppgiftstabellen med den sorterade aktivitetslistan.

   Sorteringen görs med sorteringsfunktionen för ryggrad i aktivitetslistsamlingen genom att en jämförelsefunktion anges.

   ```javascript
       return {
           //other methods
           onTaskTableHeaderClick  : onTaskTableHeaderClick,
           //other methods
       };
   ```

   ```javascript
   onTaskTableHeaderClick = function (event) {
           var target = $(event.target),
            comparator,
               attribute;
           if(target.hasClass('taskName')){
            attribute = 'stepName';
           } else if(target.hasClass('taskInstructions')){
               attribute = 'instructions';
           } else if(target.hasClass('taskRoute')){
               attribute = 'selectedRoute';
           } else if(target.hasClass('taskCreateTime')){
               attribute = 'createTime';
           } else if(target.hasClass('taskCompleteTime')){
               attribute = 'completeTime';
           }
           taskList.comparator = function (task) {
            return task.get(attribute);
           };
           taskList.sort();
           render();
       };
   ```
