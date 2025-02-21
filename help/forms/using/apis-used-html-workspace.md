---
title: API:er som används i AEM Forms arbetsyta
description: Offentlig Java&amp, handel och JavaScript API:er och metoder för arbetsytan i LiveCycle AEM Forms, som kan anpassas och automatiseras.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# API:er som används i AEM Forms arbetsyta {#apis-used-in-aem-forms-workspace}

Följande API:er används på arbetsytan i AEM Forms.

<table>
 <tbody>
  <tr>
   <td><strong>JavaScript</strong></td>
   <td><strong>Tjänstnamn</strong></td>
   <td><strong>API Name</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td>getGroups</td>
   <td>ProcessManagementUserProxyService</td>
   <td>getGroups</td>
   <td>Söker grupper. returnerar en lista över alla grupper om inget anges, annars returneras grupper med det angivna namnet.</td>
  </tr>
  <tr>
   <td>getUsersAndGroups</td>
   <td>ProcessManagementUserProxyService</td>
   <td>getUsersAndGroups</td>
   <td>Söker efter användare och grupper. Den returnerar en lista över alla användare och grupper om inget anges, annars returneras användare och grupper med det angivna namnet.</td>
  </tr>
  <tr>
   <td>prepareForSubmit</td>
   <td>ProcessManagementDocumentHandlingService</td>
   <td>prepareForSubmit</td>
   <td>Det anropas innan ett formulär skickas med DocumentSubmitServlet. Det anger uppgifts-ID i en sessionsvariabel (tillsammans med förfallotid) som hämtas under den faktiska överföringen.</td>
  </tr>
  <tr>
   <td>submitTask</td>
   <td>ProcessManagementDocumentHandlingService</td>
   <td>skicka</td>
   <td>Det skickar dokumentobjektet som är associerat med en uppgift (och skicka i sin tur).</td>
  </tr>
  <tr>
   <td>getRootEndpointCategories</td>
   <td>ProcessManagementStartPointService</td>
   <td>getRootEndpointCategories</td>
   <td>Den hämtar alla rotkategorier som finns på servern.</td>
  </tr>
  <tr>
   <td>getDirectChildCategories</td>
   <td>ProcessManagementStartPointService</td>
   <td>getDirectChildCategories2</td>
   <td>Den hämtar alla direkta underordnade för en kategori.</td>
  </tr>
  <tr>
   <td>getAllStartPoints</td>
   <td>ProcessManagementStartPointService</td>
   <td>getAllStartPoints</td>
   <td>Den hämtar alla startpunkter som finns på servern under alla kategorier.</td>
  </tr>
  <tr>
   <td>invokeStartPoint</td>
   <td>ProcessManagementStartPointService</td>
   <td>invokeStartPoint</td>
   <td>Detta anropar en startpunkt och skapar en uppgift som motsvarar en startpunkt</td>
  </tr>
  <tr>
   <td>getAllTasks</td>
   <td>ProcessManagementTaskService</td>
   <td>getAllActionTasks</td>
   <td>Den hämtar alla uppgifter som har skapats och vidarebefordrats eller konsulterats, sparats, tilldelats, tilldelats och sparats för den inloggade användaren.</td>
  </tr>
  <tr>
   <td>getTask</td>
   <td>ProcessManagementTaskService</td>
   <td>getTask</td>
   <td>Den hämtar en specifik uppgift.</td>
  </tr>
  <tr>
   <td>renderTask</td>
   <td>ProcessManagementTaskService</td>
   <td>återge</td>
   <td>Den återger en uppgift och returnerar information som behövs för att återge formuläret, t.ex. formulär-URL, formulärtyp, data-URL, om det behövs.</td>
  </tr>
  <tr>
   <td>submitWithBeforeData</td>
   <td>ProcessManagementTaskService</td>
   <td>submitWithBeforeData</td>
   <td>Returnerar resultatet av TaskManagers överförings-API med hjälp av resultatnyckeln.</td>
  </tr>
  <tr>
   <td>submitWithData</td>
   <td>ProcessManagementTaskService</td>
   <td>submitWithData</td>
   <td>Den skickar formulärdata (skickas som sträng) som är associerade med uppgiften med TaskManagers API. Det används för Flex-formulär som inte anropar TaskManagers API för att skicka.</td>
  </tr>
  <tr>
   <td>spara</td>
   <td>ProcessManagementTaskService</td>
   <td>spara</td>
   <td>Den sparar en åtgärd på servern.</td>
  </tr>
  <tr>
   <td>complete</td>
   <td>ProcessManagementTaskService</td>
   <td>complete</td>
   <td>Den slutför en uppgift och uppgiften skickas till nästa steg enligt processdesignen.</td>
  </tr>
  <tr>
   <td>getAttachment</td>
   <td>ProcessManagementTaskService</td>
   <td>getAttachment</td>
   <td>Den returnerar en URL till en bifogad fil där den bifogade filen är tillgänglig.</td>
  </tr>
  <tr>
   <td>getAllAttachments</td>
   <td>ProcessManagementTaskService</td>
   <td>getAllActionScriptAttachments</td>
   <td>Den hämtar alla bilagor och anteckningar för en uppgift.</td>
  </tr>
  <tr>
   <td>dela</td>
   <td>ProcessManagementTaskService</td>
   <td>dela</td>
   <td>Den delar en uppgift med en annan användare. En annan användare kan göra anspråk på uppgiften och blir ägare till uppgiften.</td>
  </tr>
  <tr>
   <td>vidarebefordra</td>
   <td>ProcessManagementTaskService</td>
   <td>vidarebefordra</td>
   <td>Den vidarebefordrar en uppgift till en annan användare.</td>
  </tr>
  <tr>
   <td>konsultera</td>
   <td>ProcessManagementTaskService</td>
   <td>konsultera</td>
   <td>Den konsulterar en uppgift med en annan användare.</td>
  </tr>
  <tr>
   <td>krav</td>
   <td>ProcessManagementTaskService</td>
   <td>krav</td>
   <td>Den gör anspråk på en uppgift som är tillgänglig i en delad kö.</td>
  </tr>
  <tr>
   <td>låsa upp</td>
   <td>ProcessManagementTaskService</td>
   <td>låsa upp</td>
   <td>Det låser upp en uppgift.</td>
  </tr>
  <tr>
   <td>lock</td>
   <td>ProcessManagementTaskService</td>
   <td>lock</td>
   <td>Den låser en uppgift och en annan användare kan inte göra anspråk på uppgiften om den delas.</td>
  </tr>
  <tr>
   <td>avvisa</td>
   <td>ProcessManagementTaskService</td>
   <td>avvisa</td>
   <td>Den returnerar en uppgift till uppgiftens tidigare ägare.</td>
  </tr>
  <tr>
   <td>överge</td>
   <td>ProcessManagementTaskService</td>
   <td>överge</td>
   <td>Den tar bort en uppgift.</td>
  </tr>
  <tr>
   <td>setVisibility</td>
   <td>ProcessManagementTaskService</td>
   <td>setVisibility</td>
   <td>Det anger en uppgifts synlighet. Om synligheten är inställd på false är uppgiften inte synlig för användaren efteråt.</td>
  </tr>
  <tr>
   <td>getUsers</td>
   <td>ProcessManagementUserProxyService</td>
   <td>getUsers</td>
   <td>Det används för att söka efter användare. Den returnerar alla användare om inget namn anges, eller returnerar användare med ett angivet namn.</td>
  </tr>
  <tr>
   <td>getUsersInGroup</td>
   <td>ProcessManagementUserProxyService</td>
   <td>getUsersInGroupByName</td>
   <td>Den returnerar alla användare i en grupp.</td>
  </tr>
  <tr>
   <td>grantQueueAccess</td>
   <td>ProcessManagementQueueService</td>
   <td>grantQueueAccess</td>
   <td>Den ger åtkomst till den inloggade användarens kö till en angiven användare. Det är i princip att dela din egen kö med en annan användare.</td>
  </tr>
  <tr>
   <td>requestQueueAccess</td>
   <td>ProcessManagementQueueService</td>
   <td>requestQueueAccess</td>
   <td>Den gör en åtkomstbegäran för en kö med en angiven användare för den inloggade användaren. Om användaren godkänner begäran delas användarens kö med den inloggade användaren.</td>
  </tr>
  <tr>
   <td>getGrantedUsers</td>
   <td>ProcessManagementQueueService</td>
   <td>getGrantedUsers</td>
   <td>Den returnerar alla användare som har åtkomst till kön för den inloggade användaren.</td>
  </tr>
  <tr>
   <td>getUsersForAccessibleQueues</td>
   <td>ProcessManagementQueueService</td>
   <td>getUsersForAccessibleQueues</td>
   <td>Den returnerar alla användare vars kö är tillgänglig för en användare.</td>
  </tr>
  <tr>
   <td>revokeQueueAccess</td>
   <td>ProcessManagementQueueService</td>
   <td>revokeQueueAccess</td>
   <td>Den tar bort en användare från listan över användare som har åtkomst till kön för den inloggade användaren.</td>
  </tr>
  <tr>
   <td>removeQueueAccess</td>
   <td>ProcessManagementQueueService</td>
   <td>removeQueueAccess</td>
   <td>Den tar bort en användare från listan över användare vars kö är tillgänglig för den inloggade användaren.</td>
  </tr>
  <tr>
   <td>getAllQueues<br /> </td>
   <td>ProcessManagementQueueService<br /> </td>
   <td>getAllQueues<br /> </td>
   <td>Den får alla köer (egna, delade och gruppköer) tillgängliga för den inloggade användaren.<br /> </td>
  </tr>
  <tr>
   <td>getOutOfOfficeSettings</td>
   <td>ProcessManagementOutOfOfficeService</td>
   <td>getOutOfOfficeSettings</td>
   <td>Den hämtar användarens inställningar utanför kontoret.</td>
  </tr>
  <tr>
   <td>saveOutOfOfficeSettingsJson</td>
   <td>ProcessManagementOutOfOfficeService</td>
   <td>saveOutOfOfficeSettingsJson</td>
   <td>Det sparar användarens inaktiva inställningar.</td>
  </tr>
  <tr>
   <td>getAllProcesses</td>
   <td>ProcessManagementProcessService</td>
   <td>getAllProcesses</td>
   <td>Den returnerar en lista över alla processer.</td>
  </tr>
  <tr>
   <td>getParticipatedProcesses</td>
   <td>ProcessManagementProcessService</td>
   <td>getParticipatedProcesses</td>
   <td>Den returnerar en lista med alla processnamn som den inloggade användaren har deltagit i.</td>
  </tr>
  <tr>
   <td>getProcessInstance<br /> </td>
   <td>ProcessManagementProcessService<br /> </td>
   <td>getProcessInstance<br /> </td>
   <td>Den hämtar information om en processinstans.<br /> </td>
  </tr>
  <tr>
   <td>getProcessInstances</td>
   <td>ProcessManagementQueryService</td>
   <td>getProcessInstances</td>
   <td>Den hämtar alla processinstanser för en process.</td>
  </tr>
  <tr>
   <td>getPendingTasksForProcessInstance</td>
   <td>ProcessManagementQueryService</td>
   <td>getPendingTasksForProcessInstance</td>
   <td>Den hämtar väntande uppgifter för en processinstans.</td>
  </tr>
  <tr>
   <td>getTasksForProcessInstance</td>
   <td>ProcessManagementQueryService</td>
   <td>getTasksForProcessInstance</td>
   <td>Den hämtar alla uppgifter för en processinstans.</td>
  </tr>
  <tr>
   <td>getAllSearchTemplates</td>
   <td>ProcessManagementQueryService</td>
   <td>getAllSearchTemplates</td>
   <td>Den returnerar en lista med alla sökmallar.</td>
  </tr>
  <tr>
   <td>getTemplate</td>
   <td>ProcessManagementQueryService</td>
   <td>getTemplate</td>
   <td>Det returnerar innehåll för en sökmall.</td>
  </tr>
  <tr>
   <td>findTasksJson<br /> </td>
   <td>ProcessManagementQueryService</td>
   <td>findTasksJson</td>
   <td>Den söker efter och returnerar alla uppgifter som uppfyller alla villkor i en sökmall.</td>
  </tr>
  <tr>
   <td>getAssignmentsForTask</td>
   <td>ProcessManagementTaskService</td>
   <td>getAssignmentsForTask</td>
   <td>Den hämtar alla tilldelningar för en uppgift. Om en användare t.ex. vidarebefordrar eller konsulterar en uppgift till en annan användare, är det en uppgift.</td>
  </tr>
  <tr>
   <td>deleteAttachment </td>
   <td>TaskManagerService</td>
   <td>deleteAttachment</td>
   <td>Den tar bort en bifogad fil.</td>
  </tr>
  <tr>
   <td>initialize</td>
   <td>ProcessManagementClientSessionService</td>
   <td>initialize</td>
   <td>Kontrollen förnyas vid behov. Autentiserar användare. Anger sessionsparametrar för server-/klientinformation. Returnerar användarinformation och avsökningsintervall.</td>
  </tr>
  <tr>
   <td>getTasksForDirectReports</td>
   <td>ProcessManagementTeamTasksService</td>
   <td>getTasksForDirectReports</td>
   <td>Den returnerar alla uppgifter i direkta rapporter för den inloggade hanteraren.</td>
  </tr>
  <tr>
   <td>getTaskOfDirectReport<br /> </td>
   <td>ProcessManagementTeamTasksService</td>
   <td>getDirectReportTask</td>
   <td>Den returnerar en uppgift från en angiven direkt rapport för den inloggade hanteraren.</td>
  </tr>
  <tr>
   <td>forwardTaskOfDirectReport</td>
   <td>ProcessManagementTeamTasksService</td>
   <td>forwardTaskOfDirectReport</td>
   <td>Den vidarebefordrar en uppgift som en direkt rapport till en annan användare.</td>
  </tr>
  <tr>
   <td>rejectTaskOfDirectReport</td>
   <td>ProcessManagementTeamTasksService</td>
   <td>rejectTaskOfDirectReport</td>
   <td>Den returnerar en uppgift som är en direkt rapport till den föregående användaren.</td>
  </tr>
  <tr>
   <td>getProperty</td>
   <td>WorkspacePropertyService</td>
   <td>getProperty</td>
   <td>Den hämtar en Workspace-egenskap för en användare.</td>
  </tr>
  <tr>
   <td>removeProperty</td>
   <td>WorkspacePropertyService</td>
   <td>delete</td>
   <td>Den tar bort en Workspace-egenskap för en användare.</td>
  </tr>
  <tr>
   <td>getProperties</td>
   <td>WorkspacePropertyService</td>
   <td>getPropertiesAsMap</td>
   <td>Den returnerar alla Workspace-egenskaper för en användare.</td>
  </tr>
  <tr>
   <td>setProperty</td>
   <td>WorkspacePropertyService</td>
   <td>setProperty</td>
   <td>Den ställer in en Workspace-egenskap för en användare.</td>
  </tr>
  <tr>
   <td>getCurrentUserImageUrl</td>
   <td>ProcessManagementClientSessionService</td>
   <td>getCurrentUserImageUrl</td>
   <td>Den hämtar användarens bild-URL för den inloggade användaren.</td>
  </tr>
  <tr>
   <td>getUserImageUrl</td>
   <td>ProcessManagementClientSessionService</td>
   <td>getUserImageUrl</td>
   <td>Den hämtar användarens bild-URL för den angivna användaren.</td>
  </tr>
  <tr>
   <td>uploadNote</td>
   <td>ProcessManagementDocumentHandlingService</td>
   <td>uploadNote</td>
   <td>Den överför en anteckning på servern för en uppgift.</td>
  </tr>
  <tr>
   <td>uploadRMAToServer (anropas även direkt från html-mallen)<br /> </td>
   <td>ProcessManagementDocumentHandlingService</td>
   <td>uploadAttachment</td>
   <td>Den överför en bifogad fil på servern för en uppgift.</td>
  </tr>
  <tr>
   <td>getImageURL (anropas även direkt från HTML-mallen)</td>
   <td>ProcessManagementDocumentHandlingService</td>
   <td>getImage</td>
   <td>Den hämtar bilden för en process.</td>
  </tr>
 </tbody>
</table>
