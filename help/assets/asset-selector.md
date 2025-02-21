---
title: Resursväljare
description: Lär dig hur du använder resursväljaren för att söka efter, filtrera, bläddra bland och hämta metadata för resurser i Adobe Experience Manager Assets. Lär dig även hur du anpassar gränssnittet för resursväljaren.
contentOwner: Adobe
feature: Asset Management,Metadata,Search
role: User
hide: true
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 2%

---

# Resursväljare {#asset-selector}

>[!NOTE]
>
>[Resursväljaren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/asset-selector.html?lang=en) kallades [Resursväljaren](https://helpx.adobe.com/experience-manager/6-2/assets/using/asset-picker.html) i tidigare versioner av [!DNL Experience Manager].

Med resursväljaren kan du bläddra bland, söka efter och filtrera resurser i [!DNL Adobe Experience Manager] Assets. Du kan också hämta metadata för resurser som du väljer med resursväljaren. Om du vill anpassa resursväljargränssnittet kan du starta det med parametrar för begäran som stöds. Dessa parametrar anger kontexten för resursväljaren för ett visst scenario.

För närvarande kan du skicka parametrarna `assettype` (*Bild/Video/Text*) och markering `mode` (*En/Flera*) som sammanhangsberoende information för resursväljaren, som förblir intakt genom hela markeringen.

Resursväljaren använder HTML5-meddelandet **Window.postMessage** för att skicka data för den valda resursen till mottagaren.

Resursväljaren baseras på Granites vokabulär för att välja bas. Som standard fungerar resursväljaren i bläddringsläge. Du kan dock använda filter med hjälp av omsökningsfunktionen för att begränsa sökningen efter vissa resurser.

Du kan integrera alla webbsidor (oavsett om de ingår i CQ-behållaren) med resursväljaren (`https://[AEM_server]:[port]/aem/assetpicker.html`).

## Sammanhangsberoende parametrar {#contextual-parameters}

Du kan skicka följande frågeparametrar i en URL för att starta resursväljaren i ett visst sammanhang:

| Namn | Värden | Exempel | Syfte |
|---|---|---|---|
| resurssuffix (B) | Mappsökväg som resurssuffix i URL:`http://localhost:4502/aem/`<br>`assetpicker.html/<folder_path>` | Om du vill starta resursväljaren med en viss mapp markerad, till exempel med mappen `/content/dam/we-retail/en/activities` markerad, ska URL:en ha formatet: `http://localhost:4502/aem/assetpicker.html`<br>`/content/dam/we-retail/en/activities?assettype=images` | Om du vill att en viss mapp ska väljas när resursväljaren startas, skickar du den som ett resurssuffix. |
| läge | en, flera | `http://localhost:4502/aem/assetpicker.html`<br>`?mode=multiple` <br> `http://localhost:4502/aem/assetpicker.html`<br>`?mode=single` | I flera lägen kan du markera flera resurser samtidigt med resursväljaren. |
| dialog | true, false | `http://localhost:4502/aem/assetpicker.html`<br>`?dialog=true` | Använd de här parametrarna för att öppna resursväljaren som Granite-dialogrutan. Det här alternativet kan bara användas när du startar resursväljaren via fältet Bevilja sökväg och konfigurerar den som URL för pickerSrc. |
| root | `<folder_path>` | `http://localhost:4502/aem/`<br>`assetpicker.html?assettype=images`<br>`&root=/content/dam/we-retail/en/activities` | Använd det här alternativet om du vill ange rotmappen för resursväljaren. I det här fallet kan du bara välja underordnade resurser (direkt/indirekt) under rotmappen med resursväljaren. |
| visningsläge | sök |  | Om du vill starta resursväljaren i sökningsläge med parametrarna för resurstyp och mimeType. |
| assettype (S) | bilder, dokument, multimedia, arkiv | <ul><li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=images`</li> <li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=documents`</li> <li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=multimedia`</li> <li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&assettype=archives`</li> | Använd det här alternativet om du vill filtrera resurstyper baserat på det värde som skickats. |
| mimeType | mimeType(er) (`/jcr:content/metadata/dc:format`) för en resurs (jokertecken stöds också) | <ul><li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&mimetype=image/png`</li>  <li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&?mimetype=*png`</li>  <li>`http://localhost:4502/aem/assetpicker.html?viewmode=search&mimetype=*presentation`</li>  <li>`http://localhost:4502/aem/assetpicker?viewmode=search&mimetype=*presentation&mimetype=*png`</li></ul> | Använd det för att filtrera resurser baserat på MIME-typ(er) |

## Använda resursväljaren {#using-the-asset-selector}

1. Gå till `https://[AEM_server]:[port]/aem/assetpicker` om du vill komma åt resursväljargränssnittet.
1. Navigera till önskad mapp och markera en eller flera resurser.

   ![chlimage_1-441](assets/chlimage_1-441.png)

   Du kan också söka efter den önskade resursen i rutan OmniSearch och sedan markera den.

   ![chlimage_1-442](assets/chlimage_1-442.png)

   Om du söker efter resurser med rutan OmniSearch kan du förfina sökningen genom att välja olika filter i rutan **[!UICONTROL Filters]**.

   ![chlimage_1-443](assets/chlimage_1-443.png)

1. Klicka på **[!UICONTROL Select]** i verktygsfältet.

>[!MORELIKETHIS]
>
>* [Mikrofrontsväljare för mediefiler i AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/asset-selector.html?lang=en)
