---
title: Bygga en säker AEM Forms-app för iOS
description: Lär dig hur du skapar en säker AEM Forms-app för iOS genom att arkivera Xcode-projektet. Detta skapar en installationsfil (en .ipa-fil) och en egenskapslista (en .plist-fil).
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 5cfb956a-454c-4bed-a410-003c716c46ed
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Bygga en säker AEM Forms-app för iOS {#building-a-secure-aem-forms-app-for-ios}

Du måste arkivera Xcode-projektet för AEM Forms-programmet för att kunna skapa installationsprogrammet (en IPA-fil) och en egenskapslista (en PLIST-fil). Egenskapslistfilen innehåller konfigurationsinformation för det värdbaserade interna programmet, till exempel namnet och appens värdplats. Mer information om egenskapslistfilen finns i [Om informationsegenskapslistfiler](https://developer.apple.com/library/ios/#documentation/general/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html).

1. Logga in på följande webbplats:

   [https://developer.apple.com/account/ios/identifier/bundle](https://developer.apple.com/account/ios/identifier/bundle)

1. Skapa ett program-ID. Mer information om hur du skapar ett program-ID finns i [Skapa och konfigurera program-ID](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingProfiles/MaintainingProfiles.html).
1. Klicka på **[!UICONTROL Configure App ID]** om du vill konfigurera källidentifieraren för iOS-programmet för din app.
1. Välj **[!UICONTROL Enable for Data Protection]** längst ned på webbsidan. Ange alternativ för dataskydd.

   Klicka på **[!UICONTROL Done]**.

1. Navigera till Provisioning>Distribution och skapa en ny profil med det program-ID som konfigurerats i steg 3.
1. Hämta och lägg till provisioneringsprofilen i Xcode och iPad.
1. Logga in på en Mac-dator som har Xcode och iOS SDK installerat och konfigurerat.
1. Öppna projektet `AEM Forms.xcodeproj` i Xcode.
1. Klicka på **[!UICONTROL AEM Forms]** under **[!UICONTROL TARGETS]** och välj **[!UICONTROL AEM Forms]**. Välj fliken **[!UICONTROL Build Settings]**, leta upp avsnittet **[!UICONTROL Code Signing Entitlement]** och välj alternativet **[!UICONTROL LC Enterprise]** i listrutan Entitlements.
1. Leta reda på och öppna filen `LC Enterprise.entitlements` i Xcode för redigering. Lägg till samma nyckel/värde-par som finns i provisioneringsprofilen under **XCode-berättigandena**.
1. Klicka på **[!UICONTROL All]** på fliken **[!UICONTROL Build Settings]** och sedan på **[!UICONTROL Combined]**.
1. Expandera **[!UICONTROL Code Signing]** från listan **[!UICONTROL Settings]**.
1. Välj lämplig signatur för **[!UICONTROL Code Signing Identity]**. Se till att samma signatur är markerad för **[!UICONTROL Debug]**, **[!UICONTROL Release]** och **[!UICONTROL Any iOS SDK]**.
1. Under **[!UICONTROL PROJECT]** väljer du **[!UICONTROL AEM Forms]** och kontrollerar att rätt signatur har valts för **[!UICONTROL Code Signing Identity]**, **[!UICONTROL Debug]**, **[!UICONTROL Release]** och **[!UICONTROL Any iOS SDK]**.
1. Bygg och distribuera appen AEM Forms. Detaljerade anvisningar om hur du skapar och distribuerar AEM Forms-appen finns i [Bygg installationsprogrammet för AEM Forms-appen](setup-xcode-project-build-installer.md#build-the-installer-for-the-mobile-workspace-app).
