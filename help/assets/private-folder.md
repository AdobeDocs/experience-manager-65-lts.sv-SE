---
title: Privata mappar för att dela resurser
description: Lär dig hur du skapar en privat mapp i  [!DNL Adobe Experience Manager Assets]  och delar den med andra användare och tilldelar olika behörigheter till dem.
contentOwner: AG
role: User
feature: Collaboration
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: adcf1276-f4c3-4c32-a830-cc968185dca3
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 2%

---

# Privat mapp i [!DNL Adobe Experience Manager Assets] {#private-folder}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/private-folder.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Du kan skapa en privat mapp i användargränssnittet [!DNL Adobe Experience Manager Assets] som är exklusivt tillgänglig för dig. Du kan dela den här privata mappen med andra användare och tilldela andra användare olika behörigheter. Beroende på vilken behörighetsnivå du tilldelar kan användare utföra olika åtgärder i mappen, till exempel visa resurser i mappen eller redigera resurserna.

>[!NOTE]
>
>Den privata mappen har minst en medlem med ägarrollen.

## Skapa och dela privata mappar {#create-share-private-folder}

Så här skapar och delar du en privat mapp:

1. Klicka på **[!UICONTROL Create]** i verktygsfältet i [!DNL Assets]-konsolen och välj sedan **[!UICONTROL Folder]** på menyn.

   ![Skapa resursmapp](assets/Create-folder.png)

1. Ange en rubrik och ett namn (valfritt) för mappen i dialogrutan **[!UICONTROL Create Folder]** och välj alternativet **[!UICONTROL Private]**.

1. Klicka på **[!UICONTROL Create]**. En privat mapp skapas.

   ![chlimage_1-413](assets/chlimage_1-413.png)

1. Om du vill dela mappen med andra användare och tilldela behörigheter till dem markerar du mappen och klickar på **[!UICONTROL Properties]** i verktygsfältet.

   ![informationsalternativ](assets/do-not-localize/info-circle-icon.png)

   >[!NOTE]
   >
   >Mappen visas inte för andra användare förrän du delar den.

1. På sidan **[!UICONTROL Folder Properties]** väljer du en användare i listan **[!UICONTROL Add User]**, tilldelar en roll till användaren i din privata mapp och klickar på **[!UICONTROL Add]**.

   ![chlimage_1-415](assets/chlimage_1-415.png)

   >[!NOTE]
   >
   >Du kan tilldela olika roller, till exempel `Editor`, `Owner` eller `Viewer`, till användaren som du delar mappen med. Om du tilldelar användaren en `Owner`-roll har användaren `Editor`-behörighet för mappen. Dessutom kan användaren dela mappen med andra. Om du tilldelar en `Editor`-roll kan användaren redigera resurserna i din privata mapp. Om du tilldelar en visningsprogramroll kan användaren bara visa resurserna i din privata mapp.

   >[!NOTE]
   >
   >Den privata mappen har minst en medlem med rollen `Owner`. Administratören kan därför inte ta bort alla ägarmedlemmar från en privat mapp. Om du vill ta bort befintliga ägare (och själva administratören) från den privata mappen måste administratören lägga till en annan användare som ägare.

1. Klicka på **[!UICONTROL Save]**. Beroende på vilken roll du tilldelar tilldelas användaren en uppsättning behörigheter i din privata mapp när användaren loggar in på [!DNL Assets].
1. Klicka på **[!UICONTROL Ok]** för att stänga bekräftelsemeddelandet.
1. Användaren som du delar mappen med får ett delningsmeddelande. Logga in på [!DNL Assets] med användarens autentiseringsuppgifter för att visa meddelandet.

   ![chlimage_1-416](assets/chlimage_1-416.png)

1. Klicka på [!UICONTROL Notifications] för att öppna en lista med meddelanden.

   ![Lista över meddelanden](assets/Assets-Notification.png)

1. Klicka på posten för den privata mappen som delas av administratören för att öppna mappen.

>[!NOTE]
>
>Om du vill skapa en privat mapp måste du ha [åtkomstkontrollbehörighet](/help/sites-administering/security.md#permissions-in-aem) för Läs och Ändra på den överordnade mapp som du vill skapa en privat mapp i. Om du inte är administratör är de här behörigheterna inte aktiverade för dig som standard på `/content/dam`. I så fall måste du först skaffa dessa behörigheter för ditt användar-ID/din grupp innan du försöker skapa privata mappar.

## Borttagning av privata mappar {#delete-private-folder}

Du kan ta bort en mapp genom att markera mappen och välja alternativet [!UICONTROL Delete] på den översta menyn, eller genom att använda backstegstangenten på tangentbordet.

![borttagningsalternativ på den översta menyn](assets/delete-option.png)

>[!CAUTION]
>
>Om du tar bort en privat mapp från CRXDE Lite lämnas överflödiga användargrupper kvar i databasen.

>[!NOTE]
>
>Om du tar bort en mapp med metoden ovan från användargränssnittet tas även de associerade användargrupperna bort.
>
>Befintliga överflödiga, oanvända och automatiskt genererade användargrupper kan dock tas bort från databasen med metoden `clean` i JMX i författarinstansen (`http://[server]:[port]/system/console/jmx/com.day.cq.dam.core.impl.team%3Atype%3DClean+redundant+groups+for+Assets`).
