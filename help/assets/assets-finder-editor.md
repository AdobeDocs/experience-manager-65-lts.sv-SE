---
title: Skapa och konfigurera sidor för Resursredigeraren
description: Lär dig hur du skapar anpassade sidor i Resursredigeraren och redigerar flera resurser samtidigt.
contentOwner: AG
role: User, Admin
feature: Developer Tools,Asset Management
solution: Experience Manager, Experience Manager Assets
exl-id: 39e1b20b-27d5-4869-b7e1-0dd2d087df7b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2068'
ht-degree: 0%

---

# Skapa och konfigurera sidor för Resursredigeraren {#creating-and-configuring-asset-editor-pages}

I det här dokumentet beskrivs följande:

* Därför ska du skapa anpassade resursredigeringssidor.
* Skapa och anpassa resursredigeringssidor, dvs. WCM-sidor, där du kan visa och redigera metadata och utföra åtgärder på resursen.
* Så här redigerar du flera resurser samtidigt.

<!-- TBD: Add UICONTROL tags. Need PM review. Flatten the structure a bit. Re-write to remove Geometrixx mentions and to adhere to 6.5 default samples. -->

>[!NOTE]
>
>Resursresurs är tillgänglig som en referensimplementering med öppen källkod. Se [Kommandon för resursdelning](https://adobe-marketing-cloud.github.io/asset-share-commons/). Det stöds inte officiellt.

## Varför skapa och konfigurera sidor i Resursredigeraren? {#why-create-and-configure-asset-editor-pages}

Digital Asset Management används i fler scenarier. När du går från en småskalig lösning för en liten användargrupp med professionellt utbildade användare - till exempel fotografer eller taxonomier - till större och mer mångsidiga användargrupper, till exempel företagsanvändare, WCM-författare och journalister, kan det kraftfulla användargränssnittet i [!DNL Adobe Experience Manager Assets] tillhandahålla för mycket information. Intressenter börjar begära specifika användargränssnitt eller -tillämpningar för att få tillgång till digitala resurser som är relevanta för dem.

Dessa resurscentrerade program kan vara enkla fotogallerier i ett intranät där medarbetarna kan ladda upp bilder från mässor eller ett presscenter på en offentlig webbplats. Tillgångscentrerade tillämpningar kan även omfatta kompletta lösningar som kundvagnar, kassor och verifieringsprocesser.

Att skapa ett resurscentrerat program blir en konfigurationsprocess som inte kräver kodning, bara kunskap om användargrupper och deras behov och kunskap om de metadata som används. Resurscentrerade program som skapats med [!DNL Assets] är utökningsbara: med måttlig kodningsansträngning kan återanvändbara komponenter för sökning, visning och ändring av resurser skapas.

Ett resurscentrerat program i [!DNL Experience Manager] består av en resursredigeringssida, som kan användas för att få en detaljerad vy över en viss resurs. På en resursredigeringssida kan du även redigera metadata, förutsatt att användaren som använder resursen har de behörigheter som krävs.

<!--
## Create and configure an Asset Share page {#creating-and-configuring-an-asset-share-page}

You customize the DAM Finder functionality and create pages that have all the functionality you require, which are called Asset Share pages. To create an Asset Share page, you add the page using the Geometrixx Asset Share template and then you customize the actions users can perform on that page, determine how viewers see the assets, and decide how users can build their queries.

Here are some use cases for creating a customized Asset Share page:

* Press Center for Journalists.
* Image Search Engine for internal business users.
* Image Database for website users.
* Media Tagging Interface for metadata editors.

### Create an Asset Share page {#creating-an-asset-share-page}

To create an Asset Share page, you can either create it when you are working on web sites or from the digital asset manager.

>[!NOTE]
>
>By default, when you create an Asset Share page from **New** in the digital asset manager, an Asset viewer and Asset editor are automatically created for you.

To create an new Asset Share page in the **Websites** console:

1. In the **Websites** tab, navigate to the place where you want to create an asset share page and click **New**.

1. Select the **Asset Share** page and click **Create**. The new page is created and the asset share page is listed in the **Websites** tab.

![dam8](assets/dam8.png)

The basic page created using the Geometrixx DAM Asset Share template looks as follows:

![screen_shot_2012-04-18at115456am](assets/screen_shot_2012-04-18at115456am.png)

To customize your Asset Share page, you use elements from the sidekick and you also edit query builder properties. The page **Geometrixx Press Center** is a customized version of a page based on this template:

![screen_shot_2012-04-19at123048pm](assets/screen_shot_2012-04-19at123048pm.png)

To create an asset share page by way of the digital asset manager:

1. In the digital asset manager, in **New**, select **New Asset Share**.
1. In the **Title**, enter the name of the asset share page. If desired, enter a name for the URL.

   ![screen_shot_2012-04-19at23626pm](assets/screen_shot_2012-04-19at23626pm.png)

1. Double-click the asset share page to open it and configure the page.

   ![screen_shot_2012-04-19at24114pm](assets/screen_shot_2012-04-19at24114pm.png)

   By default, when you create an Asset Share page from **New**, an Asset viewer and Asset editor are automatically created for you.

#### Customize actions {#customizing-actions}

You can determine what actions users can perform on selected digital assets from a selection of predefined actions.

To add actions to the Asset Share page:

1. In the Asset Share page that you want to customize, click **Actions** in the sidekick.

The following actions are available:

 | Action | Description |
 |---|---|
 | [!UICONTROL Delete Action] | Users can delete the selected assets. |
 | [!UICONTROL Download Action] | Lets users download selected assets to their computers. |
 | [!UICONTROL Lightbox Action] | Saves assets to a "lightbox"   where you can perform other actions on them. This comes in handy when working   with assets across multiple pages. The lightbox can also be used as a   shopping cart for assets. |
 | [!UICONTROL Move Action] | Users can move the asset to another   location |
 | [!UICONTROL Tags Action] | Lets users add tags to selected assets |
 | [!UICONTROL View Asset Action] | Opens the asset in the Asset editor for   user manipulation. |

1. Drag the appropriate action to the **Actions** area on the page. Doing so creates a button that is used to execute that action.

![chlimage_1-159](assets/chlimage_1-387.png)

#### Determine how search results are presented {#determining-how-search-results-are-presented}

You determine how results are displayed from a predefined list of lenses.

To change how search results are viewed:

1. In the Asset Share page that you want to customize, click Search.

![chlimage_1](assets/assetshare3.png)

1. Drag the appropriate lens to the top center of the page. In the Press Center, the lenses are already available. Users press the appropriate lens icon to display search results as desired.

The following lenses are available:

| Lens | Description |
|---|---|
| **[!UICONTROL List Lens]** |Presents the assets in a list fashion with details. |
| **[!UICONTROL Mosaic Lens]** |Presents assets in a mosaic fashion. |

#### Mosaic Lens {#mosaic-lens}

![chlimage_1-160](assets/chlimage_1-388.png)

#### List Lens {#list-lens}

![chlimage_1-161](assets/chlimage_1-389.png)

#### Customize the Query Builder {#customizing-the-query-builder}

The query builder lets you enter search terms and create content for the Asset Share page. When you edit the query builder, you also get to determine how many search results are displayed per page, which asset editor opens when you double-click an asset, the path the query searches, and customizes nodetypes.

To customize the query builder:

1. In the Asset Share page that you want to customize, click **Edit** in the Query Builder. By default, the **General** tab opens.
1. Select the number of results per page, the path of the asset editor (if you have a customized asset editor) and the Actions title.

![screen_shot_2012-04-23at15055pm](assets/screen_shot_2012-04-23at15055pm.png)

1. Click the **Paths** tab. Enter a path or multiple paths that the search will run. These paths are overwritten if the user uses the Paths predicate.

![screen_shot_2012-04-23at15150pm](assets/screen_shot_2012-04-23at15150pm.png)

1. Enter another node type, if desired.

1. In the **Query Builder URL** field, you can override or wrap the query builder and enter the new servlet URLs with the existing query builder component. In the **Feed URL** field, you can override the Feed URL as well.

![screen_shot_2012-04-23at15313pm](assets/screen_shot_2012-04-23at15313pm.png)

1. In the **Text** field, enter the text you want to appear for results and page numbers of results. Click **OK** when finished making changes.

![screen_shot_2012-04-23at15300pm](assets/screen_shot_2012-04-23at15300pm.png)

#### Add predicates {#adding-predicates}

Experience Manager Assets includes several predicates that you can add to the Asset Share page. These let your users further narrow searches. In some cases, they may override a query builder parameter (for example, the Path parameter).

To add predicates:

1. In the Asset Share page that you want to customize, click **Search**.

![assetshare3](assets/assetshare3.png)

1. Drag the appropriate predicates to the Asset Share page underneath the query builder. Doing so creates the appropriate fields.

![assetshare4](assets/assetshare4.png)

The following predicates are available:

| Predicate | Description |
|---|---|
| **[!UICONTROL Date Predicate]** |Lets users search for assets that were modified before and after certain dates. |
| **[!UICONTROL Options Predicate]** |The site owner can specify a property to search for (as in the property predicate, for example, cq:tags) and a content tree to populate the options from (for example, the tag tree). Doing so generates a list of options where the users can select the values (tags) that the selected property (tag property) should have. This predicate lets you build list controls like the list of tags, file types, image orientations, and so on. It is great for a fixed set of options. |
| **[!UICONTROL Path Predicate]** |Lets users define the path and subfolders, if desired. |
| **[!UICONTROL Property Predicate]** |The site owner specifies a property to search for, for example, tiff:ImageLength and the user can then enter a value, for example, 800. This returns all images that are 800 pixels high. Useful predicate if your property can have arbitrary values. |

For more information, see the [predicate Javadocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/search/eval/package-summary.html).

1. To configure the predicate further, double-click it. For example, when you open the Path Predicate, you need to assign the root path.

![screen_shot_2012-04-23at15640pm](assets/screen_shot_2012-04-23at15640pm.png)
-->

## Skapa och konfigurera en resursredigeringssida {#creating-and-configuring-an-asset-editor-page}

Du kan anpassa resursredigeraren för att bestämma hur användare kan visa och redigera digitala resurser. För att göra detta skapar du en resursredigeringssida och anpassar sedan vyerna och de åtgärder som användarna kan utföra på den sidan.

>[!NOTE]
>
>Om du vill lägga till anpassade fält i DAM-resursredigeraren lägger du till nya `cq:Widget`-noder i `/apps/dam/content/asseteditors.`

### Skapa en resursredigeringssida {#creating-the-asset-editor-page}

När du skapar sidan Resursredigerare kan det vara bra att skapa sidan direkt under sidan Resursdelning.

Så här skapar du en resursredigeringssida:

1. Gå till den plats där du vill skapa en resursredigeringssida på fliken **[!UICONTROL Websites]** och klicka på **Ny**.
1. Välj **Geometrixx Resursredigerare** och klicka på **Skapa**. Den nya sidan skapas och sidan visas på fliken **Webbplatser**.

![screen_shot_2012-04-23at15858pm](assets/screen_shot_2012-04-23at15858pm.png)

Grundsidan som skapas med Geometrixx Resursredigerarmall ser ut så här:

![assetshare5](assets/assetshare5.png)

Om du vill anpassa resursredigeringssidan använder du element från sidosparken. Sidan Resursredigeraren som du kommer åt från **Geometrixx Press Center** är en anpassad version av en sida som är baserad på den här mallen:

![assetshare6](assets/assetshare6.png)

#### Ange att en resursredigerare ska öppnas från en resursdelningssida {#setting-which-asset-editor-opens-from-an-asset-share-page}

När du har skapat den anpassade resursredigeringssidan ser du till att när du dubbelklickar på resurser som den anpassade resursresurs du skapade öppnar resurserna på den anpassade redigeringssidan.

Så här anger du sidan Resursredigeraren:

1. Klicka på **Redigera** bredvid frågeverktyget på sidan Resursresurs.

![screen_shot_2012-04-23at20123pm](assets/screen_shot_2012-04-23at20123pm.png)

1. Klicka på fliken **Allmänt** om den inte redan är markerad.

1. I fältet **Sökväg till resursredigeraren** anger du sökvägen till den resursredigerare som du vill att sidan Resursdelning ska öppna resurser i och klickar på **OK**.

![screen_shot_2012-04-23at21653pm](assets/screen_shot_2012-04-23at21653pm.png)

#### Lägg till komponenterna i Resursredigeraren {#adding-asset-editor-components}

Du avgör vilka funktioner en resursredigerare har genom att lägga till komponenter på sidan.

Så här lägger du till komponenterna i Resursredigeraren:

1. På sidan Resursredigerare som du vill anpassa väljer du **Resursredigeraren** i sidosparken. Alla tillgängliga komponenterna i Resursredigeraren visas.

>[!NOTE]
>
>Vad du kan anpassa beror på vilka komponenter som är tillgängliga. Om du vill aktivera komponenter går du till designläge och markerar de komponenter som du behöver ha aktiverade.

1. Dra komponenterna från sidosparken till resursredigeraren och gör eventuella redigeringar i komponentdialogrutorna. Komponenterna beskrivs i följande tabell och beskrivs i de detaljerade instruktionerna som följer.

>[!NOTE]
>
>När du utformar sidan Resursredigering skapar du komponenter som är skrivskyddade eller redigerbara. Användarna vet att ett fält kan redigeras om en bild av en penna visas i den komponenten. Som standard är de flesta komponenter skrivskyddade.

| Komponent | Beskrivning |
|---|---|
| **[!UICONTROL Metadata Form]och[!UICONTROL Metadata Text Field]** | Gör att du kan lägga till ytterligare metadata för en resurs och utföra en åtgärd, som att skicka, för den resursen. |
| **[!UICONTROL Sub Assets]** | Gör att du kan anpassa underresurser. |
| **Taggar** | Tillåter användare att markera och lägga till taggar i en resurs. |
| **[!UICONTROL Thumbnail]** | Visar en miniatyrbild av resursen, dess filnamn och låter dig lägga till en alternativ text. Du kan även lägga till resursredigeringsåtgärder här. |
| **[!UICONTROL Title]** | Visar resursens titel, som kan anpassas. |

![screen_shot_2012-04-23at22743pm](assets/screen_shot_2012-04-23at22743pm.png)

#### Metadataformulär och textfält - Konfigurera komponenten Visa metadata {#metadata-form-and-text-field-configuring-the-view-metadata-component}

Metadataformuläret är ett formulär som innehåller en start- och slutåtgärd. däremellan anger du **textfält**. Mer information om hur du arbetar med formulär finns i [Forms](/help/sites-authoring/default-components-foundation.md#form-component).

1. Skapa en startåtgärd genom att klicka på **Redigera** i formulärets startområde. Om du vill kan du ange en Box-titel. Som standard är Box title **Metadata**. Markera kryssrutan Klientvalidering om du vill att JavaScript-klientkoden ska genereras för validering.

![screen_shot_2012-04-23at22911pm](assets/screen_shot_2012-04-23at22911pm.png)

1. Skapa en slutåtgärd genom att klicka på **Redigera** i formulärets slutområde. Du kan till exempel skapa ett **[!UICONTROL Submit]**-alternativ som tillåter användare att skicka sina metadataändringar. Du kan också lägga till ett **Återställ**-alternativ som återställer metadata till det ursprungliga läget.

![screen_shot_2012-04-23at23138pm](assets/screen_shot_2012-04-23at23138pm.png)

1. Dra metadatatextfält mellan **Formulärstart** och **formulärslut** till formuläret. Användare fyller i metadata i dessa textfält som de kan skicka eller slutföra en annan åtgärd på.

1. Dubbelklicka på fältnamnet, till exempel **Titel**, för att öppna metadatafältet och göra ändringar. På fliken **Allmänt** i fönstret **Redigera komponent** definierar du namnutrymmet, fältetiketten och typen, till exempel `dc:title`.

![screen_shot_2012-04-23at23305pm](assets/screen_shot_2012-04-23at23305pm.png)

Mer information om hur du ändrar de namnutrymmen som finns i metadataformuläret finns i [Anpassa och utöka Assets](/help/assets/extending-assets.md).

1. Klicka på fliken **Begränsningar**. Här kan du välja om ett fält är obligatoriskt och vid behov lägga till begränsningar.

![screen_shot_2012-04-23at23435pm](assets/screen_shot_2012-04-23at23435pm.png)

1. Klicka på fliken **Visning**. Här kan du ange en ny bredd och ett nytt antal rader för metadatafältet. Markera kryssrutan **Fältet är skrivskyddat** om du vill att användarna ska kunna redigera metadata.

![screen_shot_2012-04-23at23446pm](assets/screen_shot_2012-04-23at23446pm.png)

Följande är ett exempel på ett metadataformulär med olika fält:

![metadata](assets/chlimage_1-390.png)

På sidan Resursredigeraren kan användare sedan ange värden i metadatafälten (om de är redigerbara) och utföra slutåtgärden (till exempel skicka ändringarna).

#### Deltillgångar {#sub-assets}

I den underordnade Assets-komponenten kan du visa och välja underresurser. Du kan avgöra vilka namn som visas under [huvudresursen](/help/assets/assets.md#what-are-digital-assets) och underresurserna.

Dubbelklicka på den underordnade Assets-komponenten så att du kan öppna dialogrutan med underresurser där du kan ändra rubrikerna för huvudresursen och eventuella underresurser. Standardvärdena visas under motsvarande fält.

![screen_shot_2012-04-23at23907pm](assets/screen_shot_2012-04-23at23907pm.png)

Följande är ett exempel på en ifylld underordnad Assets-komponent:

![screen_shot_2012-04-23at24442pm](assets/screen_shot_2012-04-23at24442pm.png)

Om du till exempel markerar en underresurs bör du tänka på hur komponenten visar rätt sida och Box-titeln ändras från Sub Assets till Siblings.

![screen_shot_2012-04-23at24552pm](assets/screen_shot_2012-04-23at24552pm.png)

#### Taggar {#tags}

Komponenten Tags är en komponent där användare kan tilldela befintliga taggar till en resurs, vilket gör det enklare att ordna och hämta den senare. Du kan göra den här komponenten skrivskyddad, så användarna kan inte lägga till taggar, utan bara visa dem.

![screen_shot_2012-04-23at25031pm](assets/screen_shot_2012-04-23at25031pm.png)

Dubbelklicka på taggkomponenten så att du kan öppna dialogrutan Taggar där du kan ändra titeln från Taggar, om du vill, och där du kan välja de tilldelade namnutrymmena. Om du vill göra det här fältet redigerbart avmarkerar du kryssrutan **[!UICONTROL Hide Edit]**. Som standard är taggar redigerbara.

![screen_shot_2012-04-23at24731pm](assets/screen_shot_2012-04-23at24731pm.png)

Om användare kan redigera taggar kan de klicka på pennan för att lägga till taggar genom att välja dem i listrutan Taggar.

![screen_shot_2012-04-23at25150pm](assets/screen_shot_2012-04-23at25150pm.png)

Följande är en ifylld tagg-komponent:

![screen_shot_2012-04-23at25244pm](assets/screen_shot_2012-04-23at25244pm.png)

#### Miniatyrbild {#thumbnail}

Miniatyrkomponenten är den plats där den valda miniatyrbilden visas (för många av formaten extraheras miniatyrbilden automatiskt). Komponenten visar dessutom filnamnet och [åtgärder som du kan ändra](/help/assets/assets-finder-editor.md#adding-asset-editor-actions).

![screen_shot_2012-04-23at25452pm](assets/screen_shot_2012-04-23at25452pm.png)

Dubbelklicka på miniatyrkomponenten så att du kan öppna dialogrutan med miniatyrbilder där du kan ändra alternativ text. Som standard är miniatyrens alt-text **Klicka för att hämta**-resurs.

![screen_shot_2012-04-23at25604pm](assets/screen_shot_2012-04-23at25604pm.png)

Följande är ett exempel på en fylld miniatyrkomponent:

![screen_shot_2012-04-23at34815pm](assets/screen_shot_2012-04-23at34815pm.png)

#### Titel {#title}

Rubrikkomponenten visar resursens namn och en beskrivning.

Som standard är den i skrivskyddat läge, så användare kan inte redigera den. Dubbelklicka på komponenten och avmarkera kryssrutan **Dölj redigeringsknapp** för att göra den redigerbar. Ange dessutom en titel för flera resurser.

![screen_shot_2012-04-23at35100pm](assets/screen_shot_2012-04-23at35100pm.png)

Om du kan redigera titeln kan du lägga till en titel och en beskrivning genom att klicka på pennan för att öppna fönstret **Resursegenskaper** . Dessutom kan du aktivera och inaktivera resursen genom att välja datum och tid.

När du redigerar [!UICONTROL Title] kan användare ändra **Title**, **Description** och ange **On** och **Off Times** för att aktivera och inaktivera resursen.

![screen_shot_2012-04-23at35241pm](assets/screen_shot_2012-04-23at35241pm.png)

Följande är ett exempel på en ifylld Title-komponent:

![chlimage_1-164](assets/chlimage_1-392.png)

#### Lägg till resursredigeringsåtgärder {#adding-asset-editor-actions}

Du kan avgöra vilka åtgärder användare kan utföra på valda digitala resurser från ett urval av fördefinierade åtgärder.

Så här lägger du till åtgärder på sidan Resursredigeraren:

1. Klicka på **Resursredigeraren** på sidan Resursredigerare som du vill anpassa.

![screen_shot_2012-04-23at35515pm](assets/screen_shot_2012-04-23at35515pm.png)

Följande åtgärder är tillgängliga:

| Åtgärd | Beskrivning |
|---|---|
| [!UICONTROL Download] | Tillåter användare att hämta valda resurser till sina datorer. |
| [!UICONTROL Editors] | Låter användarna redigera en bild (interaktiv redigering) |
| [!UICONTROL Lightbox] | Sparar resurser i en&quot;ljuslåda&quot; där du kan utföra andra åtgärder på dem. Detta är praktiskt när du arbetar med resurser på flera sidor. |
| [!UICONTROL Locking] | Tillåter användare att låsa en resurs. Den här funktionen är inte aktiverad som standard och måste aktiveras i komponentlistan. |
| [!UICONTROL References] | Klicka här för att visa på vilka sidor resursen används. |
| [!UICONTROL Versioning] | Gör att du kan skapa och återställa versioner av en resurs. |

1. Dra lämplig åtgärd till området **Åtgärder** på sidan. Det skapar ett alternativ som används för att köra åtgärden som dras på sidan.

![chlimage_1-165](assets/chlimage_1-393.png)

## Flera redigeringsresurser med sidan Resursredigeraren {#multi-editing-assets-with-the-asset-editor-page}

Med [!DNL Experience Manager Assets] kan du ändra flera resurser samtidigt. När du har valt resurserna kan du ändra taggar och metadata samtidigt.

Så här gör du om du vill redigera flera resurser på sidan Resursredigeraren:

1. Öppna Geometrixx **Press Center** -sida:
   `https://localhost:4502/content/geometrixx/en/company/press.html`

1. Välj resurser:

   * i Windows: `Ctrl + click` varje resurs.
   * på Mac: `Cmd + click` varje resurs.

   Om du vill välja ett intervall av resurser klickar du på den första resursen och `Shift + click` den sista resursen.

1. Klicka på **Redigera metadata** i fältet **Åtgärder** (vänster del av sidan).
1. Geometrixx **Press Center Resursredigerare** öppnas på en ny flik. Resursernas metadata visas enligt följande:

   * En tagg som inte gäller för alla resurser utan bara för ett fåtal visas i kursiv stil.
   * En tagg som gäller för alla resurser visas med ett vanligt teckensnitt.
   * Andra metadata än taggar: värdet för fältet visas bara om det är samma för alla markerade resurser.

1. Klicka på **Hämta** om du vill hämta en ZIP-fil som innehåller de ursprungliga resursåtergivningarna.
1. Klicka på Redigera taggalternativet som finns bredvid fältet **Taggar**.

   * En tagg som inte gäller för alla resurser, men bara för ett fåtal har en grå bakgrund.
   * En tagg som gäller för alla resurser har en vit bakgrund.

   Du kan:

   * Klicka på `x` för att ta bort taggen för alla resurser.
   * Klicka på `+` för att lägga till taggen i alla resurser.
   * Klicka på **pilen** och markera en tagg för att lägga till en ny tagg till alla resurser.

   Klicka på **OK** om du vill skriva ändringarna i formuläret. Rutan bredvid fältet **Taggar** markeras automatiskt.

1. Redigera beskrivningsfältet. Ange det till exempel till:

   `This is a common description`

   När ett fält redigeras skrivs de befintliga värdena för de valda resurserna över när formuläret skickas.

   Obs! Rutan bredvid fältet markeras automatiskt när fältet redigeras.

1. Klicka på **Uppdatera metadata** för att skicka formuläret och spara ändringarna för alla resurser.

   Obs! Endast markerade metadata ändras.
