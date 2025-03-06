---
title: Uppdaterar länken till dokumentationen
description: Så här uppdaterar du målet för hjälplänken för Workspace i AEM Forms så att den pekar på den anpassade dokumentationslänken.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: e99f1cbd-492e-4cc2-9975-8f17c885dd8c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Uppdaterar länken till dokumentationen {#updating-the-link-to-the-documentation}

Du kommer åt standardhjälpinnehållet för AEM Forms genom att välja **Hjälp > Hjälp om Workspace**. Den hänvisar till onlinedokumentationen på Adobe webbplats. Du kan dock uppdatera den så att den pekar på en annan URL.

Här följer några exempel på hur du kan ändra standardhjälpens URL: n:

* För att få hjälp på valfritt språk.
* Skräddarsy hjälpinnehåll för din anpassade arbetsyta.

Om du vill uppdatera webbadressen för onlinedokumentationen följer du de [allmänna anpassningsstegen](/help/forms/using/generic-steps-html-workspace-customization.md) och följande steg.

1. Kopiera filen `userinfo.html` från `/libs/ws/js/runtime/templates` till `/apps/ws/js/runtime/templates`.
1. Ändra:

   ```html
   <ul class="helpmenu">
     <li>
       <a href="https://www.adobe.com/go/learn_aemforms_documentation_63" title="<%= $.t('index.header.dropdown.WorkspaceHelp')%>" target="_blank"><%= $.t('index.header.dropdown.WorkspaceHelp')%></a>
     </li>
   ```

   till

   ```html
   <ul class="helpmenu">
     <li>
       <a href="<!--place new help url here-->" title="<%= $.t('index.header.dropdown.WorkspaceHelp')%>" target="_blank"><%= $.t('index.header.dropdown.WorkspaceHelp')%></a>
     </li>
   ```

1. Gör följande:

   1. Öppna /apps/ws/js/registry.js för redigering.
   1. Sök och ersätt `text!/lc/libs/ws/js/runtime/templates/userinfo.html` med `text!/lc/apps/ws/js/runtime/templates/userinfo.html`.
