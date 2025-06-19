---
title: Ändra organisationslogotyp för varumärken
description: Om du vill märka ut AEM Forms arbetsyta anger du din organisations logotyp genom att anpassa standardlogotypen.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 3aa4aca3-3c94-4936-ba9c-484bbb196256
source-git-commit: b8576049fba41b3bec16046316938274a5046513
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 0%

---

# Ändra organisationslogotyp för varumärken {#changing-the-organization-logo-for-branding}

Organisationslogotypen visas i det övre vänstra hörnet på arbetsytan i AEM Forms. Om du vill uppdatera logotypen följer du de [allmänna stegen för anpassning av arbetsytan i AEM Forms](/help/forms/using/generic-steps-html-workspace-customization.md#generic-steps-for-html-workspace-customization) och följande steg.

1. Skapa en logotyp och ge filen namnet `NewWorkspace.png`. Placera bildfilen i mappen /apps/ws/images med en WebDAV-klient.

   >[!NOTE]
   >
   >Den rekommenderade storleken på logotypbilden är 218 px × 20 px.

   >[!NOTE]
   >
   >Mer information finns i [WebDAV-åtkomst](/help/sites-administering/webdav-access.md).

1. Referera till den nya logotypbilden i formatmallen på /apps/ws/css/newStyle.css genom att lägga till följande format.

   ```css
   #logo {
   
          background: url(../images/NewWorkspace.png) no-repeat 14px 11px;
   
   }
   ```
