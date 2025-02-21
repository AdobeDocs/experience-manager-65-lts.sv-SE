---
title: XMP-tillbakaskrivning till återgivningar
description: Lär dig hur XMP återskrivningsfunktion sprider metadataändringar för en resurs till alla eller vissa återgivningar av resursen.
contentOwner: AG
role: User, Admin
feature: Metadata
hide: true
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 4%

---

# XMP-tillbakaskrivning till återgivningar {#xmp-writeback-to-renditions}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/xmp-metadata.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Den här XMP-återskrivningsfunktionen i [!DNL Adobe Experience Manager Assets] replikerar metadataändringarna till återgivningarna av den ursprungliga resursen. När du ändrar metadata för en resurs i Assets eller när du överför resursen, lagras ändringarna först i metadatanoden i resurshierarkin.

Med XMP tillbakaskrivningsfunktion kan du sprida metadataändringarna till alla eller vissa återgivningar av resursen. Funktionen skriver bara tillbaka de metadataegenskaper som använder registrerade namnutrymmen, det vill säga en egenskap med namnet `dc:title` skrivs tillbaka, men egenskapen `mytitle` skrivs inte.

Tänk dig ett scenario där du ändrar egenskapen [!UICONTROL Title] för resursen med namnet `Classic Leather` till `Nylon`.

![metadata](assets/metadata.png)

I det här fallet sparar [!DNL Experience Manager Assets] ändringarna av egenskapen **[!UICONTROL Title]** i parametern `dc:title` för resursens metadata som lagras i resurshierarkin.

![metadata_stored](assets/metadata_stored.png)

[!DNL Experience Manager Assets] sprider dock inte automatiskt metadataändringar till återgivningar av en resurs. Se [hur du aktiverar XMP-tillbakaskrivning](#enable-xmp-writeback).

## Aktivera tillbakaskrivning av XMP {#enable-xmp-writeback}

Om du vill att metadataändringarna ska kunna spridas till återgivningarna av resursen när den överförs ändrar du konfigurationen **[!UICONTROL Adobe CQ DAM Rendition Maker]** i Configuration Manager.

1. Öppna Configuration Manager genom att gå till `https://[aem_server]:[port]/system/console/configMgr`.
1. Öppna konfigurationen för **[!UICONTROL Adobe CQ DAM Rendition Maker]**.
1. Välj alternativet **[!UICONTROL Propagate XMP]** och spara sedan ändringarna.

   ![chlimage_1-135](assets/chlimage_1-346.png)

## Aktivera XMP-tillbakaskrivning för specifika återgivningar {#enabling-xmp-writeback-for-specific-renditions}

Om du vill att XMP Writeback-funktionen ska kunna sprida metadataändringar för att välja återgivningar anger du dessa återgivningar i arbetsflödessteget för XMP Writeback-processen i [!UICONTROL DAM Metadata WriteBack]-arbetsflödet. Som standard konfigureras det här steget med den ursprungliga återgivningen.

Utför dessa steg för XMP Writeback-funktionen för att sprida metadata till återgivningsminiatyrerna 140.100.png och 319.319.png.

1. Navigera till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]** i Experience Manager-gränssnittet.
1. Öppna arbetsflödesmodellen **[!UICONTROL DAM Metadata Writeback]** på sidan Modeller.
1. På egenskapssidan för **[!UICONTROL DAM Metadata Writeback]** öppnar du steget **[!UICONTROL XMP Writeback Process]**.
1. Klicka på fliken **[!UICONTROL Process]** i dialogrutan [!UICONTROL Step Properties].
1. Lägg till `rendition:cq5dam.thumbnail.140.100.png,rendition:cq5dam.thumbnail.319.319.png` i rutan **Argument** och klicka på **[!UICONTROL OK]**.

   ![step_properties](assets/step_properties.png)

1. Spara ändringarna.
1. Om du vill återskapa pyramidbaserade TIFF-återgivningar för [!DNL Dynamic Media]-bilder med de nya attributen lägger du till **[!UICONTROL Dynamic Media Process Image Assets]**-steget i arbetsflödet för [!UICONTROL DAM Metadata Writeback].

   PTIFF-återgivningar skapas och lagras bara lokalt i en Dynamic Media-hybridimplementering.

1. Spara arbetsflödet.

Metadataändringarna sprids till miniatyrbilden för återgivningarna.140.100.png och miniatyrbilden.319.319.png för resursen, inte till de andra.

>[!NOTE]
>
>Information om XMP-tillbakaskrivningsproblem i 64-bitars Linux finns i [Så här aktiverar du XMP återskrivningsproblem i 64-bitars RedHat Linux](https://helpx.adobe.com/experience-manager/kb/enable-xmp-write-back-64-bit-redhat.html).
>
>Information om vilka plattformar som stöds finns i [XMP krav för återskrivning av metadata](/help/sites-deploying/technical-requirements.md#requirements-for-aem-assets-xmp-metadata-write-back).

## Filtrera XMP-metadata {#filtering-xmp-metadata}

[!DNL Experience Manager Assets] har stöd för både blockeringslista och tillåtelselista-filtrering av egenskaper/noder för XMP-metadata som läses från objektbinärfiler och lagras i JCR när resurser hämtas.

Om du filtrerar med blockeringslista kan du importera alla XMP-metadataegenskaper utom de egenskaper som anges för undantag. För resurstyper som INDD-filer som har stora mängder XMP-metadata (till exempel 1 000 noder med 10 000 egenskaper) är namnen på de noder som ska filtreras inte alltid kända i förväg. Om filtrering med hjälp av blockeringslista tillåter att ett stort antal resurser med flera XMP-metadata importeras, kan distributionen av [!DNL Experience Manager] stöta på stabilitetsproblem, till exempel övervakningsköer som har stoppats.

Filtrering av XMP-metadata via tillåtelselista löser problemet genom att du kan definiera XMP-egenskaperna som ska importeras. På så sätt ignoreras alla andra eller okända XMP-egenskaper. För bakåtkompatibilitet kan du lägga till några av dessa egenskaper i filtret som använder blockeringslista.

>[!NOTE]
>
>Filtrering fungerar bara för egenskaper som härletts från XMP-källor i resurbinärfiler. För egenskaper som härleds från andra källor än XMP, t.ex. EXIF- och IPTC-format, fungerar inte filtreringen. Datumet då resursen skapades lagras till exempel i egenskapen `CreateDate` i EXIF TIFF. Experience Manager lagrar det här värdet i ett metadatafält med namnet `exif:DateTimeOriginal`. Eftersom källan inte är en XMP-källa fungerar inte filtrering på den här egenskapen.

1. Öppna Configuration Manager genom att gå till `https://[aem_server]:[port]/system/console/configMgr`.
1. Öppna konfigurationen för **[!UICONTROL Adobe CQ DAM XmpFilter]**.
1. Om du vill använda filtrering via tillåtelselista väljer du **[!UICONTROL Apply Allowlist to XMP Properties]** och anger de egenskaper som ska importeras i rutan **[!UICONTROL Allowed XML Names for XMP filtering]**.

   ![chlimage_1-136](assets/chlimage_1-347.png)

1. Om du vill filtrera bort blockerade XMP-egenskaper efter att ha använt filtrering via tillåtelselista anger du egenskaperna i rutan **[!UICONTROL Blocked XML Names for XMP filtering]**.

   >[!NOTE]
   >
   >Alternativet **[!UICONTROL Apply Blocklist to XMP Properties]** är markerat som standard. Filtrering med blockeringslista är alltså aktiverat som standard. Om du vill inaktivera sådan filtrering avbryter du valet av alternativet **[!UICONTROL Apply Blocklist to XMP Properties]**.

1. Spara ändringarna.
