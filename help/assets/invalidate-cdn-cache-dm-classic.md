---
title: Invalidera Content Delivery Network-cachen med Dynamic Media Classic
description: Om du validerar ditt cachelagrade CDN-innehåll (Content Delivery Network) kan du snabbt uppdatera resurser som levereras av Dynamic Media Classic, i stället för att vänta på att cachen ska upphöra att gälla.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5.5/ASSETS
topic-tags: dynamic-media
content-type: reference
feature: CDN Cache,Dynamic Media Classic
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 7650269e-a9c5-4b74-8bbf-d47856b3a624
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 12%

---

# Invalidera Content Delivery Network-cachen med Dynamic Media Classic {#invalidating-your-cdn-cached-content}

Dynamiska medieresurser cachas av CDN (Content Delivery Network) för snabb leverans. När du uppdaterar en resurs vill du dock att ändringarna ska börja gälla omedelbart. Genom att validera ditt CDN-cachelagrade innehåll kan du snabbt uppdatera resurser som levereras av Dynamic Media, i stället för att vänta på att cachen ska upphöra att gälla.

>[!NOTE]
>
>Den här funktionen kräver att du använder det färdiga CDN som medföljer Adobe Experience Manager Dynamic Media. Andra anpassade CDN stöds inte med den här funktionen.

>[!IMPORTANT]
>
>Följande steg gäller endast för Dynamic Media i Adobe Experience Manager 6.5, Service Pack 5 (Experience Manager 6.5.5) eller tidigare.<br>Om du använder Dynamic Media i Experience Manager 6.5, Service Pack 6 (Experience Manager 6.5.6) eller senare, följer du de steg som beskrivs i [Invalidera CDN-cachen med Dynamic Media](/help/assets/invalidate-cdn-cache-dynamic-media.md).

<!-- REMOVED MARCH 28, 2022 BECAUSE OF 404; NO REDIRECT WAS PUT IN PLACE BY SUPPORT See also [Cache overview in Dynamic Media Classic (Scene7)](https://helpx.adobe.com/experience-manager/scene7/kb/base/caching-questions/scene7-caching-overview.html). -->

**Så här gör du CDN-cachen ogiltig via Dynamic Media Classic:**

1. Öppna [Dynamic Media Classic-datorprogrammet](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/intro/dynamic-media-classic-desktop-app.html#system-requirements-dmc-app) och logga sedan in på ditt konto.

   Dina autentiseringsuppgifter och din inloggning tillhandahölls av Adobe vid tidpunkten för etableringen. Om du inte har den här informationen kan du kontakta Adobe kundsupport.

1. Navigera till **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL General Settings]** i sidans övre högra hörn.
1. Gå till textrutan **[!UICONTROL CDN Invalidation Template]** på sidan Allmänna inställningar under servergrupprubriken.

1. Ange den mall som används för att ogiltigförklara CDN-cachen (Content Delivery Network).

   Anta till exempel att du anger en bild-URL (inklusive bildförinställningar eller modifierare) som refererar till `<ID>`, i stället för ett specifikt bild-ID som i följande exempel:

   `https://server.com/is/image/Company/<ID>?$product$`

   Om mallen bara innehåller `<ID>` fylls Dynamic Media i `https://<server>/is/image`, där `<server>` är det publiceringsservernamn som definieras i Allmänna inställningar och &lt;ID> är de resurser som valts för att ogiltigförklaras.

1. Välj **[!UICONTROL Close]** i sidans nedre högra hörn.
1. I Dynamic Media Classic-användargränssnittet väljer du en eller flera resurser och går sedan till **[!UICONTROL File]** > **[!UICONTROL Invalidate CDN]**. Du ser en lista över en eller flera URL-adresser som genererats från mallen som du skapade och de resurser som du markerade. Den använder den server-URL som anges under &quot;Publicerat servernamn&quot; under Allmänna inställningar för programmet.

   Anta till exempel att du har valt en enda bild med namnet `Backpack_B` när du har angett en mall för CDN-invalidering i föregående steg. När du går till **[!UICONTROL File]** > **[!UICONTROL Invalidate CDN]** resulterar det i följande genererade URL i användargränssnittet för CDN-validering:

   `https://server.com/is/image/Company/Backpack_B?$product$`

1. I listrutan URL väljer du **[!UICONTROL Continue]** för att rensa cachen för varje specifik URL. Du kan redigera en URL eller lägga till en URL genom att skriva eller klistra in den i listrutan URL. Du behöver inte ställa in CDN-valideringsmall i förväg.

   När du har valt **[!UICONTROL Continue]** visas en indikator som ger dig en uppskattning av hur lång tid det tar att rensa cachen.

   Om du valde flera resurser, och sedan gick till **[!UICONTROL File]** > **[!UICONTROL Invalidate CDN]**, refereras varje resurs i den sparade **[!UICONTROL Template URL]**. Därför kan du definiera en **[!UICONTROL CDN Invalidate Template]** som refererar till varje URL-bildförinställning som refereras på webbplatsen (till exempel produktinformation och sökresultat). När du sedan väljer en eller flera bilder som ska ogiltigförklaras från cachen fylls gränssnittet automatiskt i med URL:erna.

   >[!NOTE]
   >
   >När du markerar resurser och sedan går till **[!UICONTROL File]** > **[!UICONTROL Invalidate CDN]** använder Dynamic Media en ogiltig CDN-mall för att automatiskt skapa URL:er som blir ogiltiga i CDN (Content Delivery Network). Om det inte finns något i textrutan **[!UICONTROL CDN Invalidate Template]** visas en tom URL-lista. Cachelagring i leveransnätverket (CDN) är inte resursbaserad utan URL-baserad. Därför är det nödvändigt att känna till de fullständiga URL:erna på webbplatsen. När du har definierat dessa URL:er kan du lägga till dem i textrutan **[!UICONTROL Invalidate CDN Template]** tidigare i stegen. Sedan kan du markera dessa resurser och göra URL:erna ogiltiga i ett enda steg.
   >
   >Ett annat alternativ är att lägga till fullständiga URL:er i listan **[!UICONTROL Invalidate CDN]**. Om du följer den här metoden behöver du inte markera resurser i Dynamic Media Classic innan du går till alternativet **[!UICONTROL File > Invalidate CDN]**.
