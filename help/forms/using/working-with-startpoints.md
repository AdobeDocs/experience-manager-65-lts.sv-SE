---
title: Arbeta med startpunkter
description: Steg för att arbeta med en Adobe Experience Manager Forms-process från din mobila enhet som definieras i Workbench.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Arbeta med startpunkter{#working-with-startpoints}

En startpunkt anropar en process som skapats i Workbench. Den är kopplad till ett formulär som anropar processen när formuläret skickas in.

>[!NOTE]
>
>Termerna startpunkter, startprocess och formulär används omväxlande när de hänvisar till det här konceptet.

Om du vill initiera en process från Adobe Experience Manager (AEM) Forms-appen måste du ha en startpunkt av typen **Workspace** i processen. Du måste också välja alternativet **[!UICONTROL Visible in Mobile Workspace]** för startpunkten.

![mws_startpoint_select_option](assets/mws_startpoint_select_option.png)

**Så här startar du en process som definieras i Workbench**

1. Om du vill visa startpunkterna som är tillgängliga i AEM Forms-appen går du till [startskärmen](../../forms/using/home-screen.md).
1. Listan **[!UICONTROL All Forms]** visas som standard på skärmen **[!UICONTROL Home]**.

   Startpunkten är kopplad till ett formulär. Markera startpunkten som är associerad med formuläret i listan för att öppna det.

   Formuläret som är kopplat till startpunkten öppnas.

1. Ange informationen i formuläret **[!UICONTROL Startpoint]**.

   Du kan lägga till anteckningar i den här aktiviteten med knappen [attachment](../../forms/using/add-attachments.md).

1. När du har fyllt i formuläret väljer du knappen **[!UICONTROL Submit]**.

Om programmet är offline sparas formuläret och dess data i mappen Utkorgen.

Om appen är online synkroniseras uppgiften med AEM Forms Server och tilldelas den användare som anges i processen.

Information om hur du arbetar med uppgiften i uppgiftslistan finns i [Öppna en uppgift](/help/forms/using/open-task.md).
