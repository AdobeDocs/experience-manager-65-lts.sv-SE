---
title: Leverera optimerade bilder för en responsiv webbplats
description: Så här använder du funktionen för responsiv kod för att leverera optimerade bilder
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
feature: Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 053efcc4-35dd-49c8-9645-ae29aa492352
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 10%

---

# Leverera optimerade bilder för en responsiv webbplats {#delivering-optimized-images-for-a-responsive-site}

Använd funktionen Responsiv kod när du vill dela koden för responsiv visning med webbutvecklaren. Du kopierar den responsiva (**[!UICONTROL RESS]**) koden till Urklipp så att du kan dela den med webbutvecklaren.

Den här funktionen är användbar om webbplatsen finns på en WCM-fil från tredje part. Om webbplatsen däremot finns på Adobe Experience Manager återger en extern bildserver bilden och skickar den till webbsidan.

Se även [Bädda in visningsprogrammet på en webbsida](embed-code.md).

Se även [Länka URL:er till ditt webbprogram](linking-urls-to-yourwebapplication.md).

**Så här levererar du optimerade bilder för en responsiv webbplats:**

1. Navigera till bilden som du vill ange responsiv kod för och välj **[!UICONTROL Renditions]** i listrutan.

   ![chlimage_1-408](assets/chlimage_1-408.png)

1. Välj en responsiv bildförinställning. Knapparna **[!UICONTROL URL]** och **[!UICONTROL RESS]** visas.

   ![chlimage_1-409](assets/chlimage_1-208.png)

   >[!NOTE]
   >
   >Den valda resursen *och* den valda bildförinställningen eller visningsförinställningen måste publiceras för att knappen **[!UICONTROL URL]** eller **[!UICONTROL RESS]** ska vara tillgänglig.
   >
   >Dynamiska media - Hybrid-läget kräver att du publicerar bildförinställningar. Dynamiska media - Scene7-läget publicerar automatiskt bildförinställningar.

1. Välj **[!UICONTROL RESS]**.

   ![chlimage_1-410](assets/chlimage_1-410.png)

1. I dialogrutan **[!UICONTROL Embed Responsive Image]** markerar och kopierar du den responsiva kodtexten och klistrar in den på din webbplats för att komma åt den responsiva resursen.
1. Redigera standardbrytpunkterna i inbäddningskoden så att de matchar brytpunkterna för den responsiva webbplatsen, direkt i koden. Testa dessutom de olika bildupplösningarna som används vid olika sidbrytpunkter.

## Använd HTTP/2 för att leverera dina dynamiska medieresurser {#using-http-to-delivery-your-dynamic-media-assets}

HTTP/2 är det nya, uppdaterade webbprotokollet som förbättrar kommunikationen mellan webbläsare och servrar. Det ger snabbare överföring av information och minskar mängden processorkraft som behövs. Dynamic Media-material kan levereras med HTTP/2 som ger bättre respons och laddningstider.

Mer information om hur du kommer igång med HTTP/2 med ditt Dynamic Media-konto finns i [HTTP2 Delivery of Content](http2.md).
