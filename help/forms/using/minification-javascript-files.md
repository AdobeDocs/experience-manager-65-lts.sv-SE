---
title: Miniatyrbilder av JavaScript-filer
description: Instruktioner för att generera minierad kod efter AEM Forms-arbetsyteanpassningar för att optimera JS-filerna för webben.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: d2d17d2c-b82f-4a7b-8ff1-0c226626412a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Miniatyrbilder av JavaScript-filer {#minification-of-the-javascript-files}

Med miniatyr tas de redundanta tecknen bort från källkoden, till exempel blanktecken, nya rader och kommentarer. Detta förbättrar prestandan genom att minska storleken på koden. Miniatyrfunktionen påverkar inte funktionen, men den minskar kodens läsbarhet.

Följ de här stegen för att generera miniatyrkod för semantiska ändringar.

1. Kopiera `client-html/src/main/webapp/js` från src-paket i filsystemet.

   >[!NOTE]
   >
   >Mer information om paketen finns i [Introduktion till anpassning av AEM Forms-arbetsytan](/help/forms/using/introduction-customizing-html-workspace.md).

1. Uppdatera sökvägar i `main.js` som finns under client-html/src/main/webapp/js, för tillagda/uppdaterade modeller/vyer.

   Om du till exempel lägger till en ny Sharequeue-modell, till exempel mySharequeue, ändras följande:

   ```javascript
   sharequeuemodel : pathprefix + 'runtime/models/sharequeue',
   ```

   Till

   ```javascript
   sharequeuemodel : pathprefix + 'runtime/myModels/mySharequeue',
   ```

1. Uppdatera `registry-config.xml, located at client-html/src/main/webapp/js/resource_generator,` om det finns en ändring/tillägg av alias i `main.js`.

   Om du till exempel lägger till en ny Sharequeue-modell, till exempel mySharequeue, ändras följande:

   ```xml
   <sharequeue
               name="sharequeue"
               path="runtime/models/sharequeue.js"
               service="service"/>
   ```

   Till

   ```xml
   <sharequeue
               name="sharequeue"
               path="runtime/myModels/mySharequeue.js"
               service="service"/>
   ```

1. Kör kommando på client-html/src/main/webapp/js/minifier:

   ```shell
   mvn clean install
   ```

   Den genererar en mapp med minifierade filer, under client-html/src/main/webapp/js med minifierade main.js och register.js.

>[!NOTE]
>
>Miniatyrbilder fungerar bara på en 64-bitars JVM.

>[!NOTE]
>
>Om du gör det så påverkas uppgraderingen.
