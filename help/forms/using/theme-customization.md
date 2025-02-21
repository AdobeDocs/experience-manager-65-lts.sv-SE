---
title: Temaanpassning
description: Lär dig hur du anpassar temat för AEM Forms-program. Du kan anpassa HTML-koden och CSS-filen för att ge organisationen ett specifikt utseende och känsla.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# Temaanpassning {#theme-customization}

Du kan anpassa HTML-koden och CSS-filen för att ge AEM Forms-appen ett distinkt utseende och känsla. Du kan till exempel ändra bakgrundsfärgen och höjden på uppgifter eller startpunkter. I följande exempel finns instruktioner för hur du ändrar:

* visningsanvisningar i stället för beskrivningen
* antal visningsrutter
* bakgrundsövertoningsfärg

## Steg {#steps}

1. Öppna projektet.

   * Öppna `Capture.xcodeproj` i Xcode för iOS
   * För Android öppnar du Android-projektet i Eclipse.
   * För Windows öppnar du `MWSWindows.sln` i Visual Studio.

1. Navigera till mappen Mallar.

   * Navigera till mappen **Capture > www > wsmoble > js > runtime > templates** i Xcode.
   * I Eclipse navigerar du till mappen **assets > www > wsmoble > js > runtime > templates** .
   * I Visual Studio går du till mappen **MWSWindows > www > wsmoble > js > runtime > templates** .

1. Öppna filen `template.html` för redigering.
1. Leta reda på följande sträng:

   ```jsp
   <%if ( (task.description !== "") && (task.description !== null) && (typeof task.description !== null) && (typeof task.description !== 'undefined') ) {%>
                  <div class="description_details">
                    <%= task.description %>
                  </div>
                 <%} else
   ```

   Ersätt den med `<%`.

1. Leta reda på följande kod i filen `template.html`:

   ```jsp
   <ul id="task_menu_list">
                                   <li class="approve" title="<%= task.availableCommands.directCommands[0]%>" data-routename="<%= task.availableCommands.directCommands[0]%>">
                                       <%= task.availableCommands.directCommands[0]%>
                                   </li>
                                   <li class="reject last" title="<%= task.availableCommands.directCommands[1]%>" data-routename="<%= task.availableCommands.directCommands[1]%>">
                                       <%= task.availableCommands.directCommands[1]%>
                                   </li>
   ```

1. Kommentera följande rad och spara filen.

   ```jsp
   task.availableCommands.directCommands[1]%>">
   <%= task.availableCommands.directCommands[1]%>
   </li>
   ```

1. Navigera till css-mappen.

   * I Xcode går du till **Capture > www > wsmoble > css**.
   * I Eclipse går du till **assets > www > wsmoble > css**.
   * I Visual Studio går du till **MWSWindows > www > wsmoble > css**.

1. Öppna filen `_style.css` för redigering.
1. Ändra `#323232` till `#fff` för bakgrundsbilden.
1. Spara ändringarna och stäng filen `_style.css`.
1. Öppna appen AEM Forms.

   Nu visas instruktioner i stället för beskrivningar i AEM Forms-appen.
