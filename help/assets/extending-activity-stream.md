---
title: Integrera [!DNL Assets] med aktivitetsströmmen
description: Beskriver inspelningsfunktionerna för  [!DNL Experience Manager]  och hur du konfigurerar det för att spela in specifika händelser.
contentOwner: AG
role: Developer
feature: Asset Management
solution: Experience Manager, Experience Manager Assets
exl-id: 44604607-e49d-469c-a6f1-dedbcd657d65
source-git-commit: a869ffbc6015fd230285838d260434d9c0ffbcb0
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Integrera [!DNL Assets] med aktivitetsström {#integrating-assets-with-activity-stream}

[!DNL Adobe Experience Manager Assets] användare utför många åtgärder, till exempel att skapa, överföra och ta bort Assets. Dessa åtgärder kan spelas in så att du kan ange en historik över vad en användare har gjort. I det här avsnittet beskrivs inspelningsfunktionerna för [!DNL Experience Manager] och hur du konfigurerar [!DNL Experience Manager] för att spela in specifika händelser.

## Prestandaöverväganden och standardbeteende {#performance-considerations-and-default-behavior}

Den här integreringen kan vara CPU och diskutrymme som förbrukar vid till exempel massimport. Av dessa anledningar är integreringen [!DNL Assets] med aktivitetsströmmen inaktiverad som standard.

## Åtgärdshändelser som stöds {#supported-action-events}

Du kan konfigurera följande händelser så att de spelas in:

* Licensen har godkänts (ACCEPTED)
* Skapad resurs (ASSET_CREATED)
* Resurs flyttad (ASSET_MOVED)
* Resursen har tagits bort (ASSET_REMOVED)
* Licensen avvisades (avvisades)
* Hämtade resurser (HÄMTADE)
* Resursversion (VERSIONED)
* Resursversionen har återställts (RESTORED)
* Resursmetadata har uppdaterats (METADATA_UPDATED)
* Tillgång publicerad till externt system (PUBLISHED_EXTERNAL)
* Resursens ursprungliga uppdaterade (ORIGINAL_UPDATED)
* Resursåtergivning uppdaterad (RENDITION_UPDATED)
* Resursåtergivning borttagen (RENDITION_REMOVED)
* Underresursen har uppdaterats (SUBASSET_UPDATED)
* Underresurs borttagen (SUBASSET_REMOVED)

## Konfigurera inspelning av [!DNL Assets] händelser {#configuring-aem-assets-events-recording}

[Webbkonsolen](/help/sites-deploying/configuring-osgi.md) ger åtkomst till Assets Event Recorder-inställningen. Så här konfigurerar du Assets Event Recorder:

1. Navigera till **[!UICONTROL Web Console]**

1. Klicka på **[!UICONTROL Configuration]**.

1. Dubbelklicka på **[!UICONTROL Day CQ DAM Event Recorder]**.

1. Kontrollera **[!UICONTROL Enables this service]**.

1. Kontrollera vilka **[!UICONTROL Event Types]** som du vill ska spelas in i användaraktivitetsströmmen.

1. Klicka på **[!UICONTROL Save]**.

## Läs inspelade händelser {#reading-recorded-events}

De registrerade händelserna lagras som aktiviteter. Du kan läsa dem programmatiskt med [ActivityManager API](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/adobe/granite/activitystreams/ActivityManager.html).
