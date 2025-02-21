---
title: Visa information i åtgärdssammanfattningsrutan
description: I AEM Forms-arbetsytan kan en åtgärdssammanfattningsruta konfigureras för att sammanfatta uppgiften eller visa en annan webbsida.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Visa information i åtgärdssammanfattningsrutan {#displaying-information-in-the-task-summary-pane}

När du öppnar en uppgift på arbetsytan i AEM Forms kan en åtgärdssammanfattning visa en sammanfattning av uppgiften. Denna ytterligare och relevanta information för en uppgift ger mer värde för slutanvändaren av AEM Forms arbetsyta.

På arbetsytan i AEM Forms kan du visa en webbsida som du väljer i rutan Sammanfattning av uppgifter. En process kan skapas för att visa en åtgärdssammanfattningsruta med Workbench.

1. Skapa en process för tilldelning av uppgift i Workbench. Mer information om åtgärden Tilldela uppgift finns i avsnittet om tjänstreferens i [Workbench-hjälpen](https://help.adobe.com/en_US/AEMForms/6.1/WorkbenchHelp/).

   >[!NOTE]
   >
   >Om det finns en URL för en TaskSummary öppnas vyn Sammanfattning som standard i stället för formulärvyn. I det här fallet öppnas inte formuläret i maximerat läge, även om användaren aktiverar alternativet&quot;Öppna formuläret i maximerat läge&quot; i Tilldela uppgift.

1. Konfigurera URL-fältet för uppgiftssammanfattning. Du kan ange ett literalt värde, en mall, en variabel eller ett XPath-uttryck.
1. Ett exempel på hur informationen på sidan Uppgiftssammanfattning visas nedan.

   * Logga in i CRXDE Lite-miljö på `https://'[server]:[port]'/lc/crx/de`.
   * `Create a node`**SampleSummary** ` under `/content` with type `nt:unStructed`. In the properties of this node, add `sling:resourceType` of type String and value `SampleSummary`. In the Access Control List of this node, add an entry for `PERM_WORKSPACE_USER` allowing `jcr:read` privileges.`
   * `Create a folder`**SampleSummary** under `/apps`. Lägg till en post för `PERM_WORKSPACE_USER` allow `jcr:readprivileges` i åtkomstkontrollistan för `/apps/SampleSummary`.
   * `Create a file `html.esp` at `/apps/SampleSummary`. For example, add the following lines in `html.esp`.`

   ```html
   <html>
       <body>
           <h1>Sample Summary</h1>
           <br/>
           <p>Hello Sir!
               <br/>
               This is sample summary page for this task.
           </p>
       </body>
   </html>
   ```

   * Ange värdet för uppgiftssammanfattnings-URL som `/lc/content/SampleSummary.html` i steget Tilldela uppgift.
   * När uppgiften som är associerad med det här tilldelningssteget öppnas i AEM Forms-arbetsytan återges `html.esp` vid `/apps/SampleSummary` i åtgärdssammanfattningsfönstret.
