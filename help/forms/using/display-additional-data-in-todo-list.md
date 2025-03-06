---
title: Visa ytterligare data i ToDo-listan
description: Hur man anpassar visningen av Att göra-listan för arbetsytan i LiveCycle AEM Forms så att mer information visas utöver standardinställningen.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 988e986f-8e0b-48a2-a529-9c0f931131b9
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Visa ytterligare data i ToDo-listan{#displaying-additional-data-in-todo-list}

Som standard visas uppgiftens visningsnamn och beskrivning i listan Uppgift på arbetsytan i AEM Forms. Du kan dock lägga till annan information, t.ex. skapandedatum och slutdatum. Du kan också lägga till ikoner och ändra visningsformatet.

![En titt på fliken HTML Workspace Att göra som visar standardkonfigurationen](assets/html-todo-list.png)

I den här artikeln beskrivs stegen för hur du lägger till information för varje uppgift i Att göra-listan.

## Vad kan läggas till {#what-can-be-added}

Du kan lägga till den tillgängliga informationen i `task.json` som har skickats av servern. Informationen kan läggas till som oformaterad text eller formateras med hjälp av format.

Mer information om JSON-objektbeskrivningen finns i [den här](/help/forms/using/html-workspace-json-object-description.md)-artikeln.

## Visa information om en uppgift {#displaying-information-on-a-task}

1. Följ de [allmänna stegen för anpassning av arbetsytan i AEM Forms](../../forms/using/generic-steps-html-workspace-customization.md).
1. Om du vill visa ytterligare information för en aktivitet måste motsvarande nyckelvärdepar läggas till i aktivitetsblocket `translation.json`.

   Ändra till exempel `/apps/ws/locales/en-US/translation.json` för engelska:

   ```json
   "task" : {
           "reminder" : {
               "value" : "Reminder",
               "tooltip" : "This is reminder __reminderCount__, for this task."
           },
           "deadlined" : {
               "value" : "Deadlined",
               "tooltip" : "This task has deadlined"
           },
           "save" : {
               "message" : "Task has been saved successfully"
           },
           "status" : {
               "deadlined" : "Deadlined",
               "created" : "Created",
               "assignedsaved" : "Draft from assigned task",
               "terminated" : "Terminated",
               "assigned" : "Assigned",
               "unknown" : "Unknown",
               "createdsaved" : "Draft from created task",
               "completed" : "Completed"
           },
           "draft" : {
               "value" : "Saved",
               "tooltip" : "This task is marked as a draft"
           },
           "escalated" : {
               "value" : "Escalated",
               "tooltip" : "This task has been escalated"
           },
           "forward" : {
               "value" : "Forwarded",
               "tooltip" : "This task was forwarded"
           },
           "priority" : {
               "highest" : "Highest priority",
               "normal" : "Normal priority",
               "high" : "High priority",
               "low" : "Low priority",
               "lowest" : "Lowest priority"
           },
           "claimed" : {
               "value" : "Claimed",
               "tooltip" : "This task has been claimed"
           },
           "locked" : {
               "value" : "Locked",
               "tooltip" : "This task is locked"
           },
           "consulted" : {
               "value" : "Consulted",
               "tooltip" : "This task has been consulted"
           },
           "returned" : {
               "value" : "Returned",
               "tooltip" : "This task was returned back"
           },
           "multiplesubmitbuttons" : {
               "message" : "The form associated with this task has multiple submit buttons so the Workspace Complete button will be disabled. Click the appropriate button on the form to submit it."
           },
           "nosubmitbutton" : {
               "message" : "The form associated with this task does not appear to have submit buttons. You may need to upgrade your Adobe Reader version to 9.1 or greater and enable the Reader Submit option in your process."
           },
           "icon" : {
               "tooltip" : "open the task __taskName__"
           }
       }
   ```

   >[!NOTE]
   >
   >Lägg till motsvarande nyckelvärdepar för alla språk som stöds.

1. Lägg till exempel till information i åtgärdsblocket:

   ```json
   "stepname" : {
               "value" : "Step Name",
               "tooltip" : "This task belongs to __stepName__ step"
   }
   ```

## Definiera CSS för den nya egenskapen {#defining-css-for-the-new-property}

1. Du kan använda format på informationen (egenskapen) som läggs till i en uppgift. För att göra detta måste du lägga till formatinformation för den nya egenskapen som lagts till i `/apps/ws/css/newStyle.css`.

   Lägg till exempel till:

   ```css
   .task .taskProperties .stepname{
       width: 25px;
       background: url(../images/stepname.png) no-repeat; /*-------- Or just reuse background image / image-sprite defined .task .taskProperties span of style.css---------------------*/
       background-position: 0px 0px; /*-------- Dummy values, need to be configured as per user background image / image-sprite ---------------------*/
   }
   ```

## Lägga till post i HTML-mallen {#adding-entry-in-the-html-template}

Slutligen måste du ta med en post i dev-paketet för varje egenskap som du vill lägga till i uppgiften. Om du vill skapa en hänvisar du till Skapa AEM Forms-arbetsytekod.

1. Kopiera `task.html`:

   * från: `/libs/ws/js/runtime/templates/`
   * till: `/apps/ws/js/runtime/templates/`

1. Lägg till den nya informationen i `/apps/ws/js/runtime/templates/task.html`.

   Lägg till under `div class="taskProperties"`:

   ```jsp
   <span class="stepname" alt="<%= $.t('task.stepname.value')%>" title = '<%= $.t("task.stepname.tooltip",{stepName:stepName})%>'/>
   ```
