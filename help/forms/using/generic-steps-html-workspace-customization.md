---
title: Allmänna steg för anpassning av AEM Forms arbetsyta
description: Hur man kommer igång med att anpassa användargränssnittet i Adobe Experience Manager Forms arbetsyta.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: User, Developer
exl-id: 9e467217-ea80-4a08-808f-9e5431b18a5e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 2%

---

# Allmänna steg för anpassning av AEM Forms arbetsyta {#generic-steps-for-aem-forms-workspace-customization}

De allmänna stegen för att utföra en anpassning är:

1. Logga in på CRXDE Lite via `https://'[server]:[port]'/lc/crx/de/index.jsp`.
1. Skapa en `sling:Folder`-mapp med namnet `ws` vid `/apps`, om den inte finns. Om du vill skapa en `sling:Folder`-mapp högerklickar du på mappen `apps` och väljer **[!UICONTROL Create]** > **[!UICONTROL Create Node]**. Ange namnet som `ws`, välj typen som `sling:Folder` och klicka på **[!UICONTROL OK]**. Klicka på **[!UICONTROL Save All]**.
1. Bläddra till `/apps/ws` och navigera till fliken **[!UICONTROL Access Control]**.
1. Välj alternativet **[!UICONTROL Repository]**. Klicka på **[!UICONTROL +]** i listan **[!UICONTROL Access Control]** för att lägga till en post. Klicka på **[!UICONTROL +]** igen.
1. Sök efter och välj **PERM_WORKSPACE_USER** Principal.

   ![Välj PERM_WORKSPACE_USER som en del av de allmänna stegen för att anpassa HTML Workspace](assets/perm_workspace_user.png)

1. Ge `jcr:read` privilegium till huvudmannen.
1. Klicka på **[!UICONTROL Save All]**.
1. Kopiera filerna `GET.jsp`, `index` och `html.jsp` från mappen `/libs/ws` till mappen `/apps/ws`.
1. Kopiera mappen `/libs/ws/locales` i mappen `/apps/ws`. Klicka på **[!UICONTROL Save All]**.
1. Uppdatera referenserna och de relativa sökvägarna i filen `GET.jsp`, så som visas nedan, och klicka på **[!UICONTROL Save all]**.

   ```javascript
   <meta http-equiv="refresh" content="0;URL='/lc/apps/ws/index.html'" />
   ```

1. Gör följande för CSS-anpassningar:

   1. Navigera till mappen `/apps/ws` och skapa en mapp med namnet `css`.

   1. Skapa en fil med namnet `newStyle.css` i mappen `css`.

   1. Öppna `/apps/ws/html`.jsp och ändra från

   ```javascript
   <link lang="en" rel="stylesheet" type="text/css" href="css/style.css" />
   <link lang="en" rel="stylesheet" type="text/css" href="css/jquery-ui.css"/>
   ```

   till

   ```javascript
   <link lang="en" rel="stylesheet" type="text/css" href="../../libs/ws/css/style.css" />
   <link lang="en" rel="stylesheet" type="text/css" href="css/newStyle.css" />
   <link lang="en" rel="stylesheet" type="text/css" href="../../libs/ws/css/jquery-ui.css"/>
   ```

   >[!NOTE]
   >
   >Placera posten för den användardefinierade CSS-filen efter posten för style.css, som visas ovan.

1. I filen /apps/ws/html.jsp kan du ändra från

   ```jsp
   <script data-main="js/main" src="js/libs/require/require.js"></script>
   ```

   till

   ```jsp
   <script data-main="js/main" src="../../libs/ws/js/libs/require/require.js"></script>
   ```

1. Gör följande:

   1. Skapa en mapp med namnet `js` på `/apps/ws`. Klicka på **[!UICONTROL Save All]**.

   1. Skapa en mapp med namnet `libs` på `/apps/ws/js`. Klicka på **[!UICONTROL Save All]**.

   1. Kopiera mappen `/libs/ws/js/libs/jqueryui` till `/apps/ws/js/libs`. Klicka på **[!UICONTROL Save All]**.

1. Gör följande för HTML-anpassningar:

   1. Skapa en mapp med namnet `runtime` under `/apps/ws/js`. Klicka på **[!UICONTROL Save All]**.

   1. Skapa en mapp med namnet `templates` under `/apps/ws/js/runtime`. Klicka på **[!UICONTROL Save All]**.

   1. Kopiera `/libs/ws/js/main.js` till `/apps/ws/js/main.js`.

   1. Kopiera /libs/ws/js/registry.js till `/apps/ws/js/registry.js`.

1. Klicka på **[!UICONTROL Save All]**, rensa cacheminnet och uppdatera arbetsytan i AEM Forms.

   Gå till URL:en `https://'[server]:[port]'/lc/ws` och logga in med autentiseringsuppgifter för administratör/lösenord. Webbläsaren omdirigeras till `https://'[server]:[port]'/lc/apps/ws/index.html`.
