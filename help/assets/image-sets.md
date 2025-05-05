---
title: Bilduppsättningar
description: Lär dig hur du arbetar med bilduppsättningar i Dynamic Media
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
feature: Image Sets,Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 654cf0fc-1a80-4764-8ce7-22d060e1f61a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2207'
ht-degree: 5%

---

# Bilduppsättningar {#image-sets}

Bilduppsättningar ger användarna en integrerad visningsupplevelse, där användarna kan se olika vyer av ett objekt genom att välja en miniatyrbild. Med bilduppsättningar kan du visa alternativa vyer av ett objekt och visningsprogrammet har zoomverktyg som gör att du kan granska bilder noggrant.

Bilduppsättningar anges av en banderoll med ordet `IMAGESET`. Om bilduppsättningen publiceras visas dessutom det publiceringsdatum som anges av ikonen **[!UICONTROL World]** på banderollen tillsammans med det senaste ändringsdatumet, som anges av ikonen **[!UICONTROL Pencil]**.

![Bilduppsättning](assets/chlimage_1-339.png)

I bilduppsättningen kan du även skapa färgrutor genom att skapa en bilduppsättning och lägga till miniatyrbilder.

Det här programmet är användbart när du vill visa ett objekt i en annan färg, ett annat mönster eller en annan avslutning. Om du vill skapa en bilduppsättning med färgrutor behöver du en bild för varje färg, mönster eller slut som du vill presentera för användarna. Du behöver också en färg-, mönster- eller slutfärgruta för varje färg, mönster eller slut.

Anta till exempel att du vill visa bilder av ändpunkter med olika färgskalor. Bilderna är röda, gröna och blå. I så fall behöver du tre bilder med samma lock. Du behöver en bild med rött, en med grönt och en med blå räkning. Du behöver också en röd, grön och blå färgruta. Färgrutorna fungerar som miniatyrbilder som användarna väljer i Visningsprogrammet för färgrutor för att se den röda, gröna eller blå hatten.

>[!NOTE]
>
>Mer information om Assets användargränssnitt finns i [Hantera resurser](/help/assets/manage-assets.md).

När du skapar en bilduppsättning rekommenderar Adobe följande metodtips och tillämpar följande begränsningar:

| Begränsningstyp | Bästa praxis | Begränsning har införts |
| --- | --- | --- |
| Antal dubblettresurser per uppsättning | Inga dubbletter | 20‡ |
| Maximalt antal bilder per uppsättning | 5-10 bilder per uppsättning | 1000 |

‡ Bästa praxis är att inte ha duplicerade resurser i en uppsättning. Gränsen är 20 kopior för en enskild resurs. Om du lägger till ytterligare en dubblett för den resursen, inom den uppsättningen, returnerar begäran ett fel eller ignorerar dubbletten.

Se även [Dynamiska mediebegränsningar](/help/assets/limitations.md).

## Snabbstart: Bilduppsättningar {#quick-start-image-sets}

**Så här kommer du igång snabbt:**

1. [Överför dina primära källbilder för flera vyer](#uploading-assets-in-image-sets).

   Börja med att ladda upp bilderna för dina bilduppsättningar. När du väljer bilder bör du komma ihåg att dina kunder kan zooma in på bilder i bilduppsättningsvisningsprogrammet. Se till att bilderna har minst 2 000 pixlar i den största dimensionen för optimal zoomdetaljrikedom. Dynamic Media kan återge bilder på upp till 25 MP (megapixlar) vardera. Du kan till exempel använda en 5 000 x 5 000 MP-bild eller någon annan storlekskombination på upp till 25 MP.

   Se [Dynamiska media - Rasterbildformat som stöds](/help/assets/assets-formats.md#supported-raster-image-formats-dynamic-media) för en lista över format som stöds av bilduppsättningar.

<!--    Adobe Experience Manager Assets supports many image file formats, but lossless TIFF, PNG, and EPS images are recommended. -->

1. [Skapa en bilduppsättning](#creating-image-sets).

   I Bilduppsättningar väljer användare miniatyrbilder i Bilduppsättningsvisningsprogrammet.

   Om du vill skapa en bilduppsättning i Assets går du till **[!UICONTROL Create]** > **[!UICONTROL Image Sets]**. Lägg sedan till bilder och välj **[!UICONTROL Save]**.

   Du kan också skapa bilduppsättningar automatiskt med [gruppuppsättningsförinställningar](/help/assets/config-dms7.md).
   >[!IMPORTANT]
   >
   >Batchuppsättningar skapas av IPS (Image Production System) som en del av tillgångsintag och är endast tillgängliga i läget Dynamic Media - Scene7.

   Se [Förbered bilduppsättningsresurser för överföring och överföring av filer](#uploading-assets-in-image-sets).

   Se [Arbeta med väljare](/help/assets/working-with-selectors.md).

1. Lägg till [visningsförinställningar för bilduppsättning](/help/assets/managing-viewer-presets.md) efter behov.

   Administratörer kan skapa eller ändra förinställningar för bildspelsvisningsprogrammet. Om du vill visa din bilduppsättning med en visningsförinställning väljer du bilduppsättningen och väljer **[!UICONTROL Viewers]** i den vänstra listrutan.

   Navigera till **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Viewer Presets]** om du vill skapa eller redigera visningsförinställningar.

1. (Valfritt) [Visa en bilduppsättning](/help/assets/image-sets.md#viewing-image-sets) som har skapats med gruppuppsättningsförinställningar.
1. [Förhandsvisa bilduppsättningar](/help/assets/previewing-assets.md).

   Markera bilduppsättningen och du kan förhandsgranska den. Markera miniatyrbildikonerna så att du kan undersöka bilduppsättningen i det valda visningsprogrammet. Du kan välja olika visningsprogram på menyn **[!UICONTROL Viewers]**, som är tillgänglig från den vänstra menyn för spår.

1. [Publicera en bilduppsättning](/help/assets/publishing-dynamicmedia-assets.md).

   När du publicerar en bilduppsättning aktiveras URL-adressen och inbäddningskoden. Dessutom måste du [publicera alla anpassade visningsprogramförinställningar](/help/assets/managing-viewer-presets.md) som du har skapat. Visningsförinställningarna som är färdiga för leverans har redan publicerats.

1. [Länka URL:er till webbprogrammet](/help/assets/linking-urls-to-yourwebapplication.md) eller [Bädda in video- eller bildvisningsprogrammet](/help/assets/embed-code.md).

   Experience Manager Assets skapar URL-anrop för bilduppsättningar och aktiverar dem när du har publicerat bilduppsättningarna. Du kan kopiera dessa URL:er när du förhandsgranskar resurser. Du kan även bädda in dem på din webbplats.

   Markera bilduppsättningen och välj sedan **[!UICONTROL Viewers]** i listrutan till vänster.

   Se [Länka en bilduppsättning till en webbsida](/help/assets/linking-urls-to-yourwebapplication.md) och [Bädda in video- eller bildvisningsprogrammet](/help/assets/embed-code.md).

Mer information om hur du redigerar bilduppsättningar finns i [Redigera bilduppsättningar](#editing-image-sets). Dessutom kan du visa och redigera [bilduppsättningsegenskaper](/help/assets/manage-assets.md#editing-properties).

Om du har problem med att skapa uppsättningar kan du läsa Bilder och uppsättningar i [Felsöka dynamiska media - Scen7-läge](/help/assets/troubleshoot-dms7.md#images-and-sets).

## Överför resurser i bilduppsättningar {#uploading-assets-in-image-sets}

Börja med att ladda upp bilderna för dina bilduppsättningar. När du väljer bilder bör du komma ihåg att dina kunder kan zooma in på bilder i bilduppsättningsvisningsprogrammet. Se till att bilderna har minst 2 000 pixlar i den största dimensionen. Bilduppsättningar har stöd för många bildfilsformat, men förlustfria TIFF-, PNG- och EPS-bilder rekommenderas.

Du kan överföra bilder för bilduppsättningar på samma sätt som du [överför andra resurser i Assets](/help/assets/manage-assets.md#uploading-assets).

Se [Dynamiska media - Rasterbildformat som stöds](/help/assets/assets-formats.md#supported-raster-image-formats-dynamic-media) för en lista över format som stöds av bilduppsättningar.

### Förbered bilduppsättningsresurser för överföring {#preparing-image-set-assets-for-upload}

Innan du skapar bilduppsättningar bör du kontrollera att bilderna har rätt storlek och format.

Om du vill skapa en bilduppsättning med flera vyer behöver du bilder som visar ett objekt från olika vypunkter eller visar olika aspekter av samma objekt. Målet är att framhäva de viktiga funktionerna i ett objekt så att läsarna får en fullständig bild av hur det ser ut eller gör.

Eftersom användare kan zooma bilder i bilduppsättningar bör du se till att bilderna har minst 2 000 pixlar i den största dimensionen. <!-- Assets support many image file formats, but lossless TIFF, PNG, and EPS images are recommended. -->

>[!NOTE]
>
>Om du använder miniatyrbilder för att ange färgrutor för en produkt måste du dessutom göra följande:
>
>Du behöver vinjetter eller olika bilder av samma bild som visar den i olika färger, mönster eller färger. Du behöver också miniatyrbilder som motsvarar de olika färgerna, mönstren eller ytorna. Om du till exempel vill visa miniatyrbilder med en bilduppsättning med samma jacka i svart, brunt och grönt behöver du:
>
>* En svart, brun och grön tagning av samma jacka.
>* En svart, brun och grön färgminiatyrbild.

## Skapa en bilduppsättning {#creating-image-sets}

Du kan skapa bilduppsättningar via användargränssnittet eller API:t. I det här avsnittet beskrivs hur du skapar bilduppsättningar i användargränssnittet.

>[!NOTE]
>
>Du kan också skapa bilduppsättningar automatiskt med [gruppuppsättningsförinställningar](/help/assets/config-dms7.md#creating-batch-set-presets-to-auto-generate-image-sets-and-spin-sets).
>**Viktigt!** Gruppuppsättningar skapas av IPS (Image Production System) som en del av tillgångsintag och är endast tillgängliga i läget Dynamic Media - Scene7.

När du lägger till resurser i uppsättningen läggs de automatiskt till i alfanumerisk ordning. Du kan ändra ordning på eller sortera resurser manuellt när de har lagts till.

>[!NOTE]
>
>Bilduppsättningar stöds inte för resurser med &quot;,&quot; (komma) i filnamnet.

När du skapar en bilduppsättning rekommenderar Adobe följande metodtips och tillämpar följande begränsningar:

| Begränsningstyp | Bästa praxis | Begränsning har införts |
| --- | --- | --- |
| Antal dubblettresurser per uppsättning | Inga dubbletter | 20‡ |
| Maximalt antal bilder per uppsättning | 5-10 bilder per uppsättning | 1000 |

‡ Bästa praxis är att inte ha duplicerade resurser i en uppsättning. Gränsen är 20 kopior för en enskild resurs. Om du lägger till ytterligare en dubblett för den resursen, inom den uppsättningen, returnerar begäran ett fel eller ignorerar dubbletten.

Se även [Dynamiska mediebegränsningar](/help/assets/limitations.md).

**Så här skapar du en bilduppsättning:**

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och går sedan till **[!UICONTROL Navigation]** > **[!UICONTROL Assets]**. Navigera till den plats där du vill skapa en bilduppsättning och gå sedan till **[!UICONTROL Create]** > **[!UICONTROL Image Set]** för att öppna sidan Bilduppsättningsredigerare.

   Du kan också skapa uppsättningen inifrån en mapp som innehåller dina resurser.

   ![6_5_imagesets-createpulldown](assets/6_5_imagesets-createpulldown.png)

1. Ange ett namn för bilduppsättningen i fältet **[!UICONTROL Title]** på sidan Bilduppsättningsredigerare. Namnet visas i banderollen över bilduppsättningen. Du kan också ange en beskrivning.

   ![6_5_imageset-creating-newset](assets/6_5_imageset-creatingnewset.png)

1. Gör något av följande:

   * Välj **[!UICONTROL Add Asset]** i det övre vänstra hörnet av bilduppsättningsredigerarsidan.

   * Välj **[!UICONTROL Tap to open Asset Selector]** i mitten av bilduppsättningsredigerarsidan.

   Välj de resurser som du vill inkludera i din bilduppsättning. De markerade resurserna visas med en bock. När du är klar väljer du **[!UICONTROL Select]** i sidans övre högra hörn.

   Med resursväljaren kan du söka efter resurser genom att skriva ett nyckelord och trycka eller klicka på **[!UICONTROL Return]**. Du kan också använda filter för att förfina sökresultatet. Du kan filtrera efter sökväg, samling, filtyp och tagg. Markera filtret och välj sedan ikonen **[!UICONTROL Filter]** i verktygsfältet. Ändra vyn genom att trycka på ikonen Visa och sedan välja **[!UICONTROL Column View]**, **[!UICONTROL Card View]** eller **[!UICONTROL List View]**.

   Se [Arbeta med väljare](/help/assets/working-with-selectors.md).

   ![6_5_imageset-adding-assets](assets/6_5_imageset-addingassets.png)

1. När du lägger till resurser i uppsättningen läggs de automatiskt till i alfanumerisk ordning. Du kan ändra ordning på eller sortera resurser manuellt när du har lagt till dem.

   Om det behövs kan du dra en resurs sorteringsikon till höger om resursens filnamn för att ordna om bilderna uppåt eller nedåt i uppsättningslistan.

   ![6_5_imageset-reorderassets](assets/6_5_imageset-reorderassets.png)

   Om du vill ändra en miniatyrbild eller färgruta markerar du ikonen **+** **miniatyrbild** bredvid bilden och går till den miniatyrbild eller färgruta du vill använda. När du är klar väljer du **[!UICONTROL Save]**.

1. (Valfritt) Gör något av följande:

   * Om du vill ta bort en bild markerar du bilden och väljer **[!UICONTROL Delete Asset]**.

   * Om du vill använda en förinställning väljer du **[!UICONTROL Preset]** längst upp till höger på sidan och väljer sedan en förinställning som ska användas på alla resurser samtidigt.

   >[!NOTE]
   >
   >När du skapar bilduppsättningen kan du ändra miniatyrbilden för bilduppsättningen eller låta Experience Manager välja miniatyrbilden automatiskt baserat på resurserna i bilduppsättningen. Om du vill välja en miniatyrbild väljer du **[!UICONTROL Change thumbnail]** ovanför fältet Titel på sidan Redigeraren för bilduppsättning och väljer sedan en bild (du kan även navigera till andra mappar för att hitta bilder). Om du har valt en miniatyrbild och sedan vill att Experience Manager ska generera en från bilduppsättningen väljer du **[!UICONTROL Switch to]** > **[!UICONTROL Automatic thumbnail]**.

1. Välj **[!UICONTROL Save]**. Den nya bilduppsättningen visas i den mapp du skapade den i.

## Visa en bilduppsättning {#viewing-image-sets}

Du kan skapa bilduppsättningar antingen i användargränssnittet eller automatiskt med [gruppuppsättningsförinställningar](/help/assets/config-dms7.md#creating-batch-set-presets-to-auto-generate-image-sets-and-spin-sets).

>[!IMPORTANT]
>
>Batchuppsättningar skapas av IPS [Image Production System] som en del av tillgångsintag och är endast tillgängliga i Dynamic Media - Scene7-läge.)

Uppsättningar som skapats med gruppuppsättningsförinställningar visas *inte* i användargränssnittet. Du kan visa uppsättningarna på tre olika sätt. (Dessa metoder är tillgängliga även om du har skapat bilduppsättningarna i användargränssnittet).

* Öppna egenskaperna för en enskild resurs. Egenskaper anger vad som ställer in den valda resursen eller en medlem i. Markera uppsättningens namn om du vill se hela uppsättningen.

  ![6_5_imageset-assetproperties](assets/6_5_imageset-assetproperties2.png)

* Från en medlemsbild i en uppsättning. Välj menyn **[!UICONTROL Sets]** om du vill visa de uppsättningar som resursen är medlem i.

  ![6_5_imageset-setspulldownmenu](assets/6_5_imageset-setspulldownmenu.png)

* Från sökningen kan du välja **[!UICONTROL Filter]**, sedan expandera **[!UICONTROL Dynamic Media]** och välja **[!UICONTROL Sets]**.

  Sökningen returnerar matchande uppsättningar som skapats manuellt i användargränssnittet eller automatiskt skapats med gruppuppsättningsförinställningar. För automatiska uppsättningar utförs sökfrågan med sökvillkoren &quot;Börjar med&quot;, som skiljer sig från Experience Manager-sökningen, som baseras på sökvillkoren &quot;Innehåller&quot;. Det enda sättet att söka efter automatiska uppsättningar är att ställa in filtret på **[!UICONTROL Sets]**.

  ![chlimage_1-134](assets/chlimage_1-134.png)

>[!NOTE]
>
>Du kan visa uppsättningar via användargränssnittet enligt beskrivningen i [Redigera bilduppsättningar](#editing-image-sets).

## Redigera en bilduppsättning {#editing-image-sets}

Du kan utföra olika redigeringsåtgärder på bilduppsättningar, till exempel följande:

* Lägg till bilder i bilduppsättningen.
* Ändra ordning på bilderna i bilduppsättningen.
* Ta bort resurser i bilduppsättningen.
* Använd förinställningar för visningsprogram.
* Ta bort bilduppsättningen.

**Så här redigerar du en bilduppsättning:**

1. Gör något av följande:

   * Håll pekaren över en bilduppsättningsresurs och välj sedan **[!UICONTROL Edit]** (pennikon).
   * Håll pekaren över en bilduppsättningsresurs, välj **[!UICONTROL Select]** (bockmarkeringsikon) och välj sedan **[!UICONTROL Edit]** i verktygsfältet.
   * Välj en bilduppsättningsresurs och välj sedan **[!UICONTROL Edit]** (pennikon) i verktygsfältet.

1. Gör något av följande om du vill redigera bilderna i bilduppsättningen:

   * Om du vill ändra ordning på resurser drar du en bild till en ny plats (markera sorteringsikonen för att flytta objekt).
   * Om du vill sortera objekt i stigande eller fallande ordning markerar du kolumnrubriken.
   * Om du vill lägga till en resurs eller uppdatera en befintlig resurs väljer du **[!UICONTROL Add Asset]**. Navigera till en resurs, markera den och välj sedan **[!UICONTROL Select]** i sidans övre högra hörn.

     >[!NOTE]
     >
     >Om du tar bort den bild som Experience Manager använder som miniatyrbild genom att ersätta den med en annan bild, visas fortfarande originalresursen.
   * Om du vill ta bort en resurs markerar du den och väljer **[!UICONTROL Delete Asset]**.
   * Om du vill använda en förinställning väljer du **[!UICONTROL Preset]** längst upp till höger på sidan och väljer sedan en visningsförinställning.
   * Om du vill lägga till eller ändra en miniatyrbild markerar du miniatyrbildikonen bredvid resursens högra sida. Navigera till den nya miniatyrbilden eller färgruteresursen, markera den och välj sedan **[!UICONTROL Select]**.
   * Om du vill ta bort en hel bilduppsättning går du till bilduppsättningen, markerar den och väljer **[!UICONTROL Delete]**.

   >[!NOTE]
   >
   >Du kan redigera bilderna i en bilduppsättning genom att navigera till uppsättningen, välja **[!UICONTROL Set Members]** i den vänstra listen och sedan välja pennikonen på en enskild resurs för att öppna redigeringsfönstret.

1. Välj **[!UICONTROL Save]** när du är klar med redigeringen.

## Förhandsvisa en bilduppsättning {#previewing-image-sets}

Se [Förhandsgranska resurser](/help/assets/previewing-assets.md).

## Publicera en bilduppsättning {#publishing-image-sets}

Se [Publicera Assets](/help/assets/publishing-dynamicmedia-assets.md).
