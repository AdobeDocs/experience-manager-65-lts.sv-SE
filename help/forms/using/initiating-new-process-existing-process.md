---
title: Initiera en ny process med befintliga processdata på arbetsytan i AEM Forms
description: Se hur du kan initiera en ny process med befintliga processdata i AEM Forms arbetsyta.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Initiera en ny process med befintliga processdata på arbetsytan i AEM Forms{#initiating-a-new-process-with-existing-process-data-in-aem-forms-workspace}

Du kan initiera en ny process med data från en befintlig processdata. Behovet av att initiera en ny process från befintliga processdata uppstår när vi måste använda samma formulär ofta med få innehållsändringar som t.ex. för formulär som inte har betalats. Den här funktionen sparar tid och arbete åt användarna, särskilt när processen har långa formulär att fylla i.

Så här startar du en ny process från befintliga processdata:-

1. Gör något av följande:

   * Klicka på den processinstans vars data du vill använda i spårningen. Klicka på den aktivitetsrad som motsvarar startpunkten i vyn Processhistorik i den högra rutan.
   * I Spärra/knip väljer du en sökmall som visar en lista med processinstanser. Markera instansen vars data du vill använda.
   * Markera uppgiften på fliken **[!UICONTROL To-Do]**. Klicka på fliken **[!UICONTROL History]** och välj den åtgärd som initierade processinstansen.

   ![Välj aktiviteten](assets/start3_new.png) ![Välj aktiviteten](assets/start1_new.png)

1. Klicka på **[!UICONTROL Start]** i åtgärdsverktygsfältet. Ett adaptivt formulär för den nya processinstansen visas med förfyllda data.

1. Uppdatera data efter behov och klicka antingen på **[!UICONTROL Complete]** eller en lämplig knapp i formuläret.
