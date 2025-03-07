---
title: Konfigurerar frånvaroinställningar
description: Med funktionen Frånvarande kan du ange när en användare ska vara frånvarande och inte kunna slutföra uppgifter som tilldelats av AEM-formulär.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: bf4fa6e4-25c7-46a8-9bae-4af7bfc14426
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 0%

---

# Konfigurerar frånvaroinställningar {#configuring-out-of-office-settings}

Med funktionen Frånvarande kan användare och administratörer ange när en användare ska vara utanför kontoret och inte kunna slutföra uppgifter som tilldelats av AEM-formulär. När en användare är inställd på Frånvarande tilldelas användaren sina uppgifter till en eller flera angivna användare. Användare kan ändra sina frånvaroinställningar i Workspace eller så kan administratörer ändra inställningarna för en användares räkning i formulärarbetsflödet.

När du skapar en process kan Workbench-användaren ange om en uppgift kan omdirigeras på grund av inställningar för frånvaro.

## Visa information om frånvaro för en användare {#view-a-user-s-out-of-office-information}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. Klicka på Tjänster > Formulärarbetsflöde > Frånvarande i administrationskonsolen.
1. I rutan uppe på sidan Frånvarande kan du göra något av följande:

   **Sök efter namn**

   Välj alternativet Sök efter namn. Skriv hela eller en del av användarnamnet och klicka på Sök. Om du lämnar fältet tomt returneras en lista över alla användare i Forms-arbetsflödet.

   **Sök efter datumintervall**

   Välj alternativet Sök efter datumintervall. Ange datumen från och till tillsammans med önskade tidsstämplar för att begränsa sökresultatet. Klicka på Sök.

1. Klicka på ett användarnamn för att visa användarens frånvaroinformation under listan med användare.

## Ändra en användares frånvarostatus {#change-a-user-s-out-of-office-status}

1. Hitta användaren enligt beskrivningen i [Visa användarens frånvaroinformation](configuring-out-office-settings.md#view-a-user-s-out-of-office-information).
1. Klicka på namnet på den användare som du vill ändra.
1. I listan *Användarnamn* är för närvarande väljer du antingen In the Office eller Out of the Office.
1. Klicka på Spara.

## Lägga till ett datumintervall utanför kontoret för en användare {#add-an-out-of-office-date-range-for-a-user}

1. Hitta användaren enligt beskrivningen i [Visa användarens frånvaroinformation](configuring-out-office-settings.md#view-a-user-s-out-of-office-information).
1. Klicka på namnet på den användare som du vill ändra.
1. Klicka på Lägg till datumintervall.
1. Ange en starttid och en sluttid. Du kan klicka på kalenderikonen för att välja ett datum. Om du inte anger någon sluttid anges användaren som ledig på obestämd tid.
1. Klicka på Spara.

## Tilldela en användare en frånvarouppgift {#assign-a-user-for-out-of-office-tasks}

När en användare inte är på kontoret kan du tilldela en eller flera användare för att utföra nya uppgifter för användaren. Du kan konfigurera följande konfigurationer:

* Tilldela alla nya uppgifter till en angiven standardanvändare.
* Tilldela inte om några uppgifter. Nya uppgifter förblir tilldelade den användare som inte är på kontoret.
* Tilldela en standardanvändare som tar emot de flesta av användarens uppgifter, men ange att uppgifter från vissa processer ska tilldelas andra användare eller förbli tilldelade till den användare som inte är på kontoret.
* Tilldela inte en standardanvändare, men tilldela vissa uppgifter från vissa processer till specifika användare.

   1. Hitta användaren enligt beskrivningen i [Visa användarens frånvaroinformation](configuring-out-office-settings.md#view-a-user-s-out-of-office-information).
   1. Klicka på namnet på den användare som du vill ändra.
   1. Välj en användare i listan Standardanvändare för aktiviteter utanför kontoret. Om du inte vill utse en standardanvändare som ska ta emot omtilldelade objekt väljer du Tilldela inte.

      Om rätt användarnamn inte visas i listan klickar du på Sök efter användare och använder dialogrutan Sök efter användare för att söka efter användaren. Välj lämplig användare i listan och klicka på Välj användare. Du kan också klicka på Visa användarens schema i dialogrutan Sök efter användare för att visa den valda användarens frånvaroschema.

   1. Om det finns processer som inte ska skickas till standardanvändaren klickar du på Lägg till ett undantag, markerar processen och väljer en annan användare i listan. Du kan också välja Tilldela inte om du vill att uppgiften ska fortsätta att vara tilldelad den användare som inte är på kontoret.
   1. Klicka på Spara.
