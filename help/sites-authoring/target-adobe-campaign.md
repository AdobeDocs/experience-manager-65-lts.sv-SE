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
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '801'
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

## Skapa ett nyhetsbrev med riktat innehåll {#creating-a-newsletter-with-targeted-content}

När ni har skapat segment, varumärken, kampanjer och upplevelser kan ni skapa ett nyhetsbrev med riktat innehåll. När ni har skapat upplevelsen länkar ni upplevelser till era segment.

>[!NOTE]
>
>[E-postexempel är bara tillgängliga i Geometrixx](/help/sites-developing/we-retail.md). Ladda ned exempelinnehåll för Geometrixx från Package Share.

Så här skapar du ett nyhetsbrev med riktat innehåll:

1. Skapa ett nyhetsbrev med riktat innehåll: Klicka på **Skapa** > **Sida** nedan för e-postkampanjer i Geometrixx Outdoors och välj en av Adobe Campaign Mail-mallarna.

   ![chlimage_1-188](assets/chlimage_1-188.png)

1. Lägg till en Text- och Personalization-komponent i nyhetsbrevet.
1. Lägg till text i Text- och Personalization-komponenten, till exempel&quot;This is the default&quot;.
1. Klicka på pilen bredvid **Redigera** och välj **Mål**.
1. Välj ert varumärke i den nedrullningsbara menyn Varumärke och välj er kampanj. (Detta är varumärket och kampanjen som du skapade tidigare).
1. Klicka på **Starta målanpassning**. Dina segment visas i området Publiker. Standardupplevelsen används om inget av de definierade segmenten matchar.

   >[!NOTE]
   >
   >Som standard använder de e-postexempel som ingår i AEM Adobe Campaign som målmotor. För anpassade nyhetsbrev kan du behöva välja Adobe Campaign som målmotor. När du väljer mål klickar du på + i verktygsfältet, anger en rubrik för den nya aktiviteten och väljer **Adobe Campaign** som målmotor.

1. Klicka på **Standard** och sedan på komponenten Text och Personalization som du lade till så visas Bullseye med en pil i. Klicka på ikonen om du vill ange komponenten som mål.

   ![chlimage_1-189](assets/chlimage_1-189.png)

1. Navigera till ett annat segment (Man) och klicka på **Lägg till erbjudande** och klicka på plusikonen +. Redigera sedan erbjudandet.
1. Navigera till ett annat segment (Kvinna) och klicka på **Lägg till erbjudande** och plustecknet +. Redigera sedan erbjudandet.
1. Klicka på **Nästa** för att se mappning, klicka sedan på **Nästa** för att visa inställningar som inte gäller för Adobe Campaign och klicka sedan på **Spara**.

   AEM genererar automatiskt rätt kod för Adobe Campaign när innehållet används i en leverans i Adobe Campaign

1. Skapa leveransen i Adobe Campaign - välj **E-postleverans med AEM-innehåll** och välj det lokala AEM-kontot, beroende på vad som är lämpligt, och bekräfta dina ändringar.

   I HTML-vyn omsluts de olika upplevelserna av målkomponenter av Adobe Campaign målkod.

   ![chlimage_1-190](assets/chlimage_1-190.png)

   >[!NOTE]
   >
   >Om du även konfigurerar segmenten i Adobe Campaign kan du visa upplevelserna för varje segment genom att klicka på **Förhandsgranska** .
