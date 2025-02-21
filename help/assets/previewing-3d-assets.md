---
title: Förhandsgranska 3D-resurser
description: Lär dig hur du förhandsgranskar 3D-resurser i Experience Manager.
contentOwner: Rick Brough
docset: aem65
feature: 3D Assets
role: User
hide: true
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 1%

---

# Förhandsgranska 3D-resurser i Adobe Experience Manager {#previewing-3d-assets-aem}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/previewing-3d-assets.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Experience Manager stöder överföring, leverans och interaktiv förhandsgranskning av 3D-resurser som en del av utvecklingsprocessen.

Det interaktiva 3D-visningsprogrammet finns på sidan med resursinformation i Experience Manager. Visningsprogrammet innehåller bland annat en samling interaktiva kamerakontroller som du kan använda för att rotera, zooma och panorera 3D-resursen.

<!-- See also [Working with 3D assets in Dynamic Media](/help/assets/assets-3d.md). -->

## Format som stöds för 3D-förhandsgranskning i Experience Manager {#supported-3d-previewing-assets}

Interaktiv 3D-förhandsgranskning stöder följande filformat:

| 3D-filtillägg | Filformat | MIME-typ | Anteckningar |
|---|---|---|---|
| GLB | Binär GL-överföring | model/gltf-binary | |
| GLTF | GL-överföringsformat | model/gltf+json | Se **Kommentar** nedan. |
| OBJ | WaveFront 3D-objektfil | application/x-tgif | |
| STL | Stereolithografi | application/vnd.ms-pki.stl | |
| DN | Adobe Dimension | model/x-adobe-dn | Stöd endast för förtäring. Förhandsgranskning är inte tillgängligt. |
| USDZ | Zip-arkiv för universell scenbeskrivning | model/vnd.usdz+zip | Stöd endast för förtäring. Förhandsgranskning är inte tillgängligt. |

>[!NOTE]
>
>Om material inte återges i förhandsgranskning av en gLTF-modell kontrollerar du att de har rätt namn och finns i en `textures`-mapp i samma rotmapp som modellen, som i följande:

    Resurs (mapp)
    model.gltf
    model.bin
    textures (mapp)
    material_0_baseColor.jpeg
    material_0_normal.jpeg

## Prestandaöverväganden när du förhandsgranskar 3D-resurser i Experience Manager{#performance-3d-previewing-assets}

Hur lång tid det tar att öppna en 3D-resurs på visningssidan för resursinformation beror på flera faktorer, till exempel bandbredd, bildkomplexitet och fördröjningar för servern.

Dessutom är funktioner i klientdatorn - som en arbetsstation, bärbar dator eller mobil touchenhet - också viktiga att tänka på när du manipulerar kameran interaktivt. Ett relativt kraftfullt system med bra grafikfunktioner kan göra den interaktiva 3D-visningen smidigare och mer gynnsam.

**Så här förhandsgranskar du 3D-resurser i Experience Manager:**

1. Se till att du har överfört 3D-resurser till Experience Manager.
Se [Format som stöds för 3D-förhandsgranskning](#supported-3d-previewing-assets) och [Överför Assets](/help/assets/manage-assets.md#uploading-assets).
1. På sidan **[!UICONTROL Navigation]** i Experience Manager väljer du **[!UICONTROL Assets]** > **[!UICONTROL Files]**.

   ![Navigeringssida](/help/assets/assets-dm/navigation-assets.png)

1. I den nedrullningsbara listan Visa i det övre högra hörnet av sidan väljer du **[!UICONTROL Card View]** och navigerar sedan till en 3D-resurs som du vill förhandsgranska.

   ![Välj 3D-kort](/help/assets/assets-dm/3d-card-select.png)
   _I kortvyn väljer du kortet för den 3D-resurs som du vill förhandsgranska._

1. Välj kortet för 3D-resursen.

   ![Interaktiv 3D-förhandsvisning](/help/assets/assets-dm/3d-preview.png)
   _Interaktiv förhandsgranskning av en 3D-resurs på sidan med resursinfo._
1. Gör något av följande på sidan med resursinformationsvyn för 3D-resursen:

   | Visa | Beskrivning | Musåtgärd | Åtgärd på pekskärmen |
   | --- | --- | --- | --- |
   | **Vrid kameran** | Ordna vyn runt 3D-scenen och objekt. | Vänsterklicka och dra. | Tryck med ett finger och dra. |
   | **Panorera kameran** | Panorera vyn åt vänster, åt höger, uppåt eller nedåt. | Högerklicka och dra. | Tryck med två fingrar och dra. |
   | **Zooma kameran** | Flytta in och ut från områden i 3D-scenen. | Rullningshjul. | Nyp med två fingrar. |
   | **Ange kameran igen** | Centrera kameran igen till en punkt på ett objekt i 3D-scenen. | Dubbelklicka. | Dubbelmarkera. |
   | **Återställ** | I närheten av det nedre högra hörnet av sidan väljer du ikonen Återställ om du vill återställa målpunkten till mitten av 3D-resursen. Återställ flyttar också kameran närmare eller längre bort för att visa resursen i dess helhet och med en rimlig visningsstorlek. |   |   |
   | **Helskärmsläge** | Om du vill aktivera helskärmsläget väljer du Helskärmsikonen längst ned till höger på sidan. |   |   |

1. När du är klar väljer du **[!UICONTROL Close]** i sidans övre högra hörn.
