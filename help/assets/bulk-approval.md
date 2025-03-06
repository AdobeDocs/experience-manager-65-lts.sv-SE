---
title: Granska mappresurser och samlingar
description: Ställ in granskningsarbetsflöden för material i en mapp eller en samling och dela dem med granskare eller kreativa partners för att få feedback.
contentOwner: AG
feature: Collaboration, Collections
role: User
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 9d921487-89a7-4271-bdce-67ae539e5d85
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '743'
ht-degree: 4%

---

# Granska mappresurser och samlingar {#review-folder-assets-and-collections}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/bulk-approval.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Ställ in granskningsarbetsflöden för material i en mapp eller en samling och dela dem med granskare eller kreativa partners för att få feedback.

Med [!DNL Adobe Experience Manager Assets] kan du skapa ett ad hoc-granskningsarbetsflöde för resurser i en mapp eller samling och dela det med granskare eller kreativa partner för att få feedback.

Du kan antingen associera granskningsarbetsflödet med ett projekt eller skapa en oberoende granskningsåtgärd.

När du har delat resurserna kan granskarna godkänna eller avvisa dem. Meddelanden skickas i olika faser av arbetsflödet för att meddela avsedda mottagare om att olika uppgifter har slutförts. När du till exempel delar en mapp eller samling får granskaren ett meddelande om att en mapp/samling har delats för granskning.

När granskaren har slutfört granskningen (godkänner eller avvisar resurser) får du ett meddelande om slutförd granskning.

## Skapa en granskningsuppgift för mappar {#creating-a-review-task-for-folders}

1. I användargränssnittet [!DNL Assets] väljer du den mapp som du vill skapa en granskningsaktivitet för.
1. Klicka på **[!UICONTROL Create Review Task]** ![Skapa granskningsåtgärd](assets/do-not-localize/create-review-task.png) i verktygsfältet för att öppna sidan **[!UICONTROL Review Task]**. Om du inte kan se alternativet i verktygsfältet klickar du på **[!UICONTROL More]** och väljer sedan alternativet.

1. (Valfritt) I listan **[!UICONTROL Project]** väljer du det projekt som du vill associera granskningsaktiviteten med. Som standard är alternativet **[!UICONTROL None]** markerat. Om du inte vill associera något projekt med granskningsaktiviteten ska du behålla det här valet.

   >[!NOTE]
   >
   >Endast de projekt som du har redigeringsbehörighet för (eller högre) visas i listan **[!UICONTROL Projects]**.

1. Ange ett namn för granskningsaktiviteten och välj en godkännare i listan **[!UICONTROL Assign To]**.

   >[!NOTE]
   >
   >Medlemmarna/grupperna i det valda projektet är tillgängliga som godkännare i listan **[!UICONTROL Assign To]**.

1. Ange en beskrivning, uppgiftsprioritet och förfallodatum för granskningsaktiviteten.

   ![task_details](assets/task_details.png)

1. Ange en etikett som ska användas för att skapa URI på fliken Avancerat.

   ![review_name](assets/review_name.png)

1. Klicka på **[!UICONTROL Submit]** och sedan på **[!UICONTROL Done]** för att stänga bekräftelsemeddelandet. Ett meddelande om den nya uppgiften skickas till godkännaren.
1. Logga in på [!DNL Assets] som godkännare och navigera till användargränssnittet i [!DNL Assets]. Om du vill godkänna resurser klickar du på **[!UICONTROL Notifications]** och väljer sedan granskningsaktiviteten i listan.

   ![Assets-meddelande](assets/aemAssetsNotification.png)

1. Granska informationen om granskningsaktiviteten på sidan **[!UICONTROL Review Task]** och klicka sedan på **[!UICONTROL Review]**.
1. Markera resurser på sidan **[!UICONTROL Review Task]** och klicka på **[!UICONTROL Approve/Reject]** för att godkänna eller avvisa efter behov.

   ![review_task](assets/review_task.png)

1. Klicka på **[!UICONTROL Complete]** i verktygsfältet. Skriv en kommentar i dialogrutan och klicka på **[!UICONTROL Complete]** för att bekräfta.
1. Navigera till användargränssnittet för [!DNL Assets] och öppna mappen. Ikonerna för godkännandestatus för resurserna visas i kortvyn och listvyn.

   **Kortvy**

   ![Granska status som den visas i kortvyn](assets/chlimage_1-404.png)

   **Listvy**

   ![Granskningsstatus som den visas i listvyn](assets/review_status_listview.png)

## Skapa en granskningsaktivitet för samlingar {#creating-a-review-task-for-collections}

1. På sidan Samlingar väljer du den samling som du vill skapa en granskningsuppgift för.
1. Klicka på **[!UICONTROL Create Review Task]** ![Skapa granskningsåtgärd](assets/do-not-localize/create-review-task.png) i verktygsfältet för att öppna sidan **[!UICONTROL Review Task]**. Om du inte kan se alternativet i verktygsfältet klickar du på **[!UICONTROL More]** och väljer sedan alternativet.

1. (Valfritt) I listan **[!UICONTROL Project]** väljer du det projekt som du vill associera granskningsaktiviteten med. Som standard är alternativet **[!UICONTROL None]** markerat. Om du inte vill associera något projekt med granskningsaktiviteten ska du behålla det här valet.

   >[!NOTE]
   >
   >Endast de projekt som du har redigeringsbehörighet för (eller högre) visas i listan **[!UICONTROL Projects]**.

1. Ange ett namn för granskningsaktiviteten och välj en godkännare i listan **[!UICONTROL Assign To]**.

   >[!NOTE]
   >
   >Medlemmarna/grupperna i det valda projektet är tillgängliga som godkännare i listan **[!UICONTROL Assign To]**.

1. Ange en beskrivning, uppgiftsprioritet och förfallodatum för granskningsaktiviteten.

   ![task_details-collection](assets/task_details-collection.png)

1. Klicka på **[!UICONTROL Submit]** och sedan på **[!UICONTROL Done]** för att stänga bekräftelsemeddelandet. Ett meddelande om den nya uppgiften skickas till godkännaren.
1. Logga in på [!DNL Assets] som godkännare och navigera till konsolen [!DNL Assets]. Om du vill godkänna resurser klickar du på **[!UICONTROL Notifications]** och väljer sedan granskningsaktiviteten i listan.
1. Granska informationen om granskningsaktiviteten på sidan **[!UICONTROL Review Task]** och klicka sedan på **[!UICONTROL Review]**.
1. Alla resurser i samlingen visas på granskningssidan. Markera resurserna och klicka på **[!UICONTROL Approve/Reject]** om du vill godkänna eller avvisa resurserna.

   ![review_task_collection](assets/review_task_collection.png)

1. Klicka på **[!UICONTROL Complete]** i verktygsfältet. Skriv en kommentar i dialogrutan och klicka på **[!UICONTROL Complete]** för att bekräfta.
1. Navigera till samlingskonsolen och öppna samlingen. Ikonerna för godkännandestatus för resurserna visas både i kort- och listvyn.

   ![collection_reviewstatuscardview](assets/collection_reviewstatuscardview.png)

   *Figur: Kortvy.*

   ![collection_reviewstatuslistview](assets/collection_reviewstatuslistview.png)

   *Figur: Listvy.*
