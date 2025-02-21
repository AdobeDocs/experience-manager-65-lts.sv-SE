---
title: Anpassa flikar för en uppgift
description: Anpassa flikarnas namn i LiveCycle AEM Forms.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 0%

---

# Anpassa flikar för en uppgift {#customizing-tabs-for-a-task}

Du kan anpassa fliknamn för komponenten `Start Process` i `Start Process` nummervyn och för komponenten `Task Details` i `ToDo` användarvyn.

1. Följ de [allmänna stegen för anpassning av arbetsytan i AEM Forms](/help/forms/using/generic-steps-html-workspace-customization.md).
1. Ändra värdet för `tabname` i filen `translation.json`.

   Ändra till exempel `/apps/ws/locales/en-US/translation.json` för engelska till följande.

   * Använd följande utdrag från blocket `"startprocess" : {}` för uppgifter som initieras i startprocessen.

   ```json
   "tabname" : {
               "form" : "Application",
               "details" : "Overview",
               "attachments" : "Attachments",
               "notes" : "Helper Notes"
           }
   ```

   * Använd följande utdrag från blocket `"todo" : {}` för uppgifter i Att göra.

   ```json
   "tabname" : {
               "summary" : "Bird's-eye view",
               "history" : "Past",
               "form" : "Form",
               "details" : "Overview",
               "attachments" : "Attachments",
               "notes" : "Notes"
   }
   ```

   >[!NOTE]
   >
   >Lägg till motsvarande nyckelvärdepar för alla språk som stöds.
