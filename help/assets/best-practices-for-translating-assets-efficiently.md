---
title: Bästa tillvägagångssätt för att översätta resurser
description: Bästa tillvägagångssätt för effektiv hantering av resurser för att synkronisera olika översatta versioner och effektivisera översättningsarbetsflöden.
contentOwner: AG
role: Admin
feature: Asset Management
solution: Experience Manager, Experience Manager Assets
exl-id: 21771c11-ecce-4eff-be5b-f55835a5644e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Bästa tillvägagångssätt för att översätta resurser {#best-practices-for-translating-assets-efficiently}

[!DNL Adobe Experience Manager Assets] har stöd för flerspråkiga arbetsflöden för att översätta binärfiler, metadata och taggar för digitala resurser till flera språkområden och för att hantera översatta resurser. Mer information finns i [Flerspråkig Assets](multilingual-assets.md).

Om du vill hantera resurser på ett effektivt sätt och försäkra dig om att olika översatta versioner förblir synkroniserade skapar du [språkkopior](preparing-assets-for-translation.md) av resurserna innan du kör översättningsarbetsflöden.

En språkkopia av en resurs eller en grupp av resurser är ett språkliknande objekt (eller en version av resurserna i ett modersmål) med en liknande innehållshierarki.

Varje språkkopia är en oberoende resurs. Om du översätter resurser till flera språkområden kan du därför öka storleken på CRX-databasen drastiskt. Om du till exempel översätter resurser med en kombinerad storlek på 10 GB till två språk kan databasstorleken öka med ungefär 20 GB (10 GB för varje språk).

Resurbinärfiler upptar mycket större lagringsutrymme jämfört med metadata och taggar. Om översättning av metadata och taggar bara har ditt syfte utelämnar du därför att översätta binärfilerna. Du kan behålla den ursprungliga kopian av binärfilerna i databasen för association med metadata och taggar översatta till olika språkområden. Genom att behålla en enda kopia av binärfiler, i stället för flera översatta versioner, minimeras påverkan på databasstorleken.

File Data Store och Amazon S3 Data Store tillhandahåller en lagringsinfrastruktur som passar bäst för dessa scenarier. De här lagringsdatabaserna lagrar en enda kopia av resursbinärfiler (inklusive återgivningar) som kan delas av metadata och taggar på flera språkområden. Databasstorleken påverkas därför inte om du skapar kopior av resurser och översätter metadata och taggar.

Du kan också göra några konfigurationsändringar i ett par arbetsflöden och översättningsintegrationsramverket för att ytterligare effektivisera processen.

1. Gör något av följande:

   * [Konfigurera arkiv för fildata](/help/sites-deploying/data-store-config.md)
   * [Konfigurera Amazon S3-datalagret](/help/sites-deploying/data-store-config.md)

<!--
1. Disable the [DAM MetaData Write-back](/help/sites-administering/workflow-offloader.md#disable-offloading) workflow.

   As the name suggests, the [!UICONTROL DAM Metadata Writeback] workflow rewrites the metadata to the binary file. Because the metadata changes after translation, writing it back to the binary file generates a different binary for a language copy.

   >[!NOTE]
   >
   >Disabling the [!UICONTROL DAM MetaData Writeback] workflow turns off XMP metadata write-back on asset binaries. Consequently, future metadata changes are no longer be saved within the assets. Evaluate the consequences before disabling this workflow.
-->

1. Aktivera arbetsflödet [!UICONTROL Set last modified date].

   Arbetsflödet [!UICONTROL DAM MetaData Writeback] konfigurerar det senaste ändringsdatumet för en resurs. Eftersom du inaktiverar det här arbetsflödet i steg 2 kan [!DNL Assets] inte längre hålla det senaste ändrade datumet för resurser uppdaterat. Aktivera därför arbetsflödet *Ange senaste ändringsdatum* för att säkerställa att senaste ändrade datum för resurser är uppdaterade. Assets med inaktuella senast ändrade datum kan orsaka fel.

1. [Konfigurera översättningsintegreringsramverket](/help/sites-administering/tc-tic.md) så att översättningen av resursbinärfiler avbryts. Avmarkera alternativet **[!UICONTROL Translate Assets]** på fliken [!UICONTROL Assets] om du vill stoppa översättningen av resurbinärfiler.
1. Översätt metadata/taggar för resurser med [Arbetsflöden för flerspråkiga resurser](multilingual-assets.md).
