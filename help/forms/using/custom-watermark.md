---
title: Egen vattenstämpel i PDF-förhandsvisning
description: Lär dig hur du skapar en anpassad vattenstämpel i PDF Preview.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
docset: aem65
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: eb089355-51d9-4c41-bd82-5ca873438410
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Egen vattenstämpel i PDF-förhandsvisning{#custom-watermark-in-letter-pdf-preview}

## Ökning {#overview}

I användargränssnittet Skapa korrespondens förhandsgranskar agentanvändare korrespondensen i den slutliga form som den skickas till efterbearbetning, till exempel för e-post eller utskrift.

För att förhindra obehörig användning av dessa data kan man lägga in en vattenstämpel i förhandsgranskningen av PDF. Standardvattenstämpeln är&quot;PREVIEW&quot;, som visas i hela PDF.

Om du vill aktivera vattenstämpeln i förhandsgranska PDF väljer du alternativet **[!UICONTROL Apply Watermark]** under förhandsgranskning i **[!UICONTROL Correspondence Management Configurations]** på https://&#39;[server]:[port]/system/console/configMgr.

![standardvattenstämpel](assets/default-watermark.png)

Följ de här stegen för att anpassa texten och utseendet på vattenstämpeln:

## Anpassa vattenstämpeln i PDF-förhandsvisning i användargränssnittet Skapa korrespondens {#customizewatermark-}

1. Gå till `https://'[server]:[port]'/[ContextPath]/crx/de` och logga in som administratör.
1. I appmappen skapar du en mapp med namnet **[!UICONTROL previewwatermark]** med en sökväg/struktur som påminner om mappen med förhandsvattenstämpeln i mappen libs:

   1. Högerklicka på mappen **previewwatermark** på följande sökväg och välj **Overlay Node**:

      `/libs/fd/cm/configFiles/previewwatermark`

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/configFiles/previewwatermark

      **Plats för övertäckning:** /apps/

      **Matcha nodtyper:** Markerade

      >[!NOTE]
      >
      >Gör inga ändringar i grenen /libs. Alla ändringar du gör kan gå förlorade eftersom den här grenen kan ändras när du:
      >
      >    
      >    
      >    * Uppgradera till din instans
      >    * Använd en snabbkorrigering
      >    * Installera ett funktionspaket
      >    
      >

   1. Klicka på **OK** och sedan på **Spara alla**. Mappen **[!UICONTROL previewwatermark]** skapas i den angivna sökvägen.

1. Kopiera och klistra in ddx-filen från mappen &quot;/libs/fd/cm/configFiles/previewwatermark&quot; till mappen &quot;/apps/fd/cm/configFiles/previewwatermark&quot; och klicka på **[!UICONTROL Save All]**.
1. Gör önskade ändringar i ddx-filen under /apps/fd/cm/configFiles/previewwatermark/.

   ```xml
   <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
    <PDF result="output.pdf">
     <PDF source="input.pdf"/>
           <Watermark opacity="15%" rotation="45">
            <StyledText>
                     <p font-family="Georgia" font-size="70pt" color="black" font-weight="bold">
                         PREVIEW
                    </p>
            </StyledText>
           </Watermark>
    </PDF>
   </DDX>
   ```

   Information om hur du anpassar vattenstämpelns utseende, text och justering finns i Lägga till och ta bort vattenstämplar och bakgrunder i dokumentet [Assembler Service och DDX Reference](https://help.adobe.com/en_US/livecycle/11.0/ddxRef.pdf) .

   >[!NOTE]
   >
   >I ddx-filen ska referenserna till result och source inte ändras till output.pdf och input.pdf. Namnet på filen ddx ska inte heller ändras.

1. Klicka på **Spara alla**.
