---
title: Rikta din Adobe Campaign
description: Ni kan skapa riktade upplevelser för Adobe Campaign efter att ha skapat segmentering.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: personalization
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization,Integration
role: User,Admin,Architect,Developer
exl-id: ce6ebfff-3a1d-4c9f-aa50-23d1c3afc852
source-git-commit: 2edf37c2d6bb04b418618f2780f773ab37559114
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# Rikta in er på Adobe Campaign{#targeting-your-adobe-campaign}

Om du vill använda ditt Adobe Campaign-nyhetsbrev måste du först skapa segmentering, som bara är tillgängligt i det klassiska användargränssnittet (för klientkontext). Efter det kan ni skapa riktade upplevelser för Adobe Campaign. Båda beskrivs i det här avsnittet.

## Ställa in segmentering i AEM {#setting-up-segmentation-in-aem}

Om du vill ställa in segmentering måste du använda det klassiska användargränssnittet för att ställa in segmenten. De återstående stegen kan utföras i standardgränssnittet.

Att skapa segmentering innefattar att skapa segment, ett varumärke, en kampanj och upplevelser.

>[!NOTE]
>
>Segment-ID måste mappas till det på Adobe Campaign-sidan.

### Skapa segment {#creating-segments}

Så här skapar du segment:

1. Öppna [segmenteringskonsolen](http://localhost:4502/miscadmin#/etc/segmentation) på **&lt;värd>:&lt;port>/miscadmin#/etc/segmentering**.
1. Skapa en sida och ange en titel - till exempel **AC-segment** - och välj mallen **Segment (Adobe Campaign)**.
1. Markera den skapade sidan i trädvyn till vänster.
1. Skapa till exempel ett segment som riktar sig till manliga användare genom att skapa en sida under segmentet som du skapade som kallas hane och välja mallen **Segment (Adobe Campaign)**.
1. Öppna den skapade segmentsidan och dra och släpp ett **Segment-ID** från sidosparken på sidan.
1. Dubbelklicka på trait, ange det ID som representerar i det här fallet det manliga segment som definieras i Adobe Campaign - till exempel **MALE** - och klicka på **OK**. Följande meddelande ska visas: *`targetData.segmentCode == "MALE"`*
1. Upprepa stegen för ett annat segment, till exempel ett segment som riktar sig till kvinnliga användare.

### Skapa ett varumärke {#creating-a-brand}

Så här skapar du ett varumärke:

1. I **Webbplatser** navigerar du till mappen **Kampanjer** (till exempel i We.Retail).
1. Klicka på **Skapa sida** och ange en rubrik för sidan, till exempel We.Retail Brand och välj mallen **Brand**.

### Skapa en kampanj {#creating-a-campaign}

Skapa en kampanj:

1. Öppna sidan **Varumärke** som du skapade.
1. Klicka på **Skapa sida** och ange en rubrik för sidan, till exempel We.Retail Campaign, och välj mallen **Campaign** och klicka på **Create**.

### Skapa upplevelser {#creating-experiences}

Så här skapar du upplevelser för segment:

1. Öppna sidan **Kampanj** som du skapade.
1. Skapa upplevelser för era segment genom att klicka på **Skapa sida** och ange en titel för sidan, till exempel&quot;Male&quot; när du skapar en upplevelse för&quot;Male&quot;-segmentet, och välj sedan mallen **Experience** .
1. Öppna den skapade Experience-sidan.
1. Klicka på **Redigera** och sedan under Segment klicka på **Lägg till objekt**.
1. Ange sökvägen till det manliga segmentet, till exempel **/etc/segmentation/ac-segment/man**, och klicka på **OK**. Följande meddelande ska visas: *Upplevelsen är riktad till: Man*
1. Upprepa föregående steg för att skapa en upplevelse för alla segment, till exempel kvinnligt mål.
