---
title: Checka in och checka ut filer i  [!DNL Assets]
description: Lär dig hur du checkar ut resurser för redigering och checkar in dem igen när ändringarna är klara.
contentOwner: AG
role: User
feature: Asset Management
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: bea51406-a033-4db1-ba1d-8596891cd12d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 3%

---

# Checka in och checka ut filer i [!DNL Experience Manager] DAM {#check-in-and-check-out-files-in-assets}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/check-out-and-submit-assets.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

Med [!DNL Adobe Experience Manager Assets] kan du checka ut resurser för redigering och checka in dem igen när du har gjort ändringarna. När du har checkat ut en resurs kan bara du redigera, kommentera, publicera, flytta eller ta bort resursen. När du checkar ut en resurs låses den. Andra användare kan inte utföra någon av dessa åtgärder på resursen förrän du checkar in resursen igen på [!DNL Assets]. De kan dock fortfarande ändra metadata för den låsta resursen.

Om du vill kunna checka ut/in resurser måste du ha skrivbehörighet för dem.

Den här funktionen förhindrar att andra användare åsidosätter ändringar som gjorts av en författare där flera användare samarbetar i redigeringsarbetsflöden mellan team.

## Checka ut resurser {#checking-out-assets}

1. I användargränssnittet [!DNL Assets] väljer du den resurs du vill checka ut. Du kan också välja flera resurser att checka ut.
1. Klicka på **[!UICONTROL Checkout]** i verktygsfältet. Alternativet **[!UICONTROL Checkout]** växlar till **[!UICONTROL Checkin]**.
Logga in som en annan användare om du vill kontrollera om andra användare kan redigera den utcheckade resursen. En låssymbol visas på miniatyrbilden för den resurs som du har checkat ut.

   ![chlimage_1-471](assets/chlimage_1-471.png)

   Markera resursen. Observera att verktygsfältet inte visar några alternativ som gör att du kan redigera, kommentera, publicera eller ta bort resursen.

   ![chlimage_1-472](assets/chlimage_1-472.png)

   Om du vill redigera metadata för den låsta resursen klickar du på **[!UICONTROL View Properties]**.

1. Klicka på **[!UICONTROL Edit]** för att öppna resursen i redigeringsläge.

   ![chlimage_1-473](assets/chlimage_1-473.png)

1. Redigera resursen och spara ändringarna. Beskär till exempel bilden och spara.

   ![chlimage_1-474](assets/chlimage_1-474.png)

   Du kan också välja att anteckna eller publicera resursen.

1. Välj den redigerade resursen i [!DNL Assets]-gränssnittet och klicka på **[!UICONTROL Checkin]** i verktygsfältet. Den ändrade resursen checkas in på [!DNL Assets] och är tillgänglig för andra användare för redigering.

## Tvingad incheckning {#forced-check-in}

Administratörer kan checka in resurser som är utcheckade av andra användare.

1. Logga in på [!DNL Assets] som administratör.
1. I användargränssnittet [!DNL Assets] väljer du en eller flera resurser som har checkats ut av andra användare.

   ![chlimage_1-476](assets/chlimage_1-476.png)

1. Klicka på **[!UICONTROL Release Lock]** i verktygsfältet. Resursen checkas in igen och är tillgänglig för redigering för andra användare.

## Bästa praxis och begränsningar {#tips-limitations}

* Det går att ta bort en *mapp* som innehåller utcheckade resursfiler. Innan du tar bort en mapp kontrollerar du att inga digitala resurser är utcheckade av användarna.

>[!MORELIKETHIS]
>
>* [Förstå incheckning och utcheckning i [!DNL Experience Manager] datorprogrammet](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=sv-SE#how-app-works2)
>* [Videosjälvstudiekurs för att förstå incheckning och utcheckning i [!DNL Assets]](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/collaboration/check-in-and-check-out.html?lang=sv-SE)
