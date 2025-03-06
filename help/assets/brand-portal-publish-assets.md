---
title: Publicera material i Brand Portal
description: Lär dig hur du publicerar och avpublicerar resurser på Brand Portal.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: brand-portal
content-type: reference
docset: aem65
feature: Brand Portal
role: User
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 9eba1e3f-9251-445e-b791-2be0a92aebd1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 39%

---

# Publicera material i Brand Portal {#publish-assets-to-brand-portal}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/brand-portal/publish-to-brand-portal.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Som administratör för Adobe Experience Manager (AEM) Assets kan du publicera resurser och mappar i AEM Assets Brand Portal-instansen (eller schemalägga publiceringsarbetsflödet till ett senare datum/tid) för din organisation. Du måste dock först konfigurera AEM Assets med varumärkesportalen. Mer information finns i [Konfigurera AEM Assets med varumärkesportalen](/help/assets/configure-aem-assets-with-brand-portal.md).

När replikeringen är klar kan du publicera resurser, mappar och samlingar till Brand Portal. Så här publicerar du resurser på Brand Portal:

>[!NOTE]
>
>Adobe rekommenderar stegvis publicering, helst vid tidpunkter med låg belastning, för att AEM-författaren inte ska uppta för mycket resurser.

1. På Assets-konsolen väljer du de resurser/den mapp som du vill publicera och klickar på alternativet **[!UICONTROL Quick Publish]** i verktygsfältet.

   Du kan också välja de mediefiler du vill publicera till Brand Portal.

   ![publish2bp-2](assets/publish2bp.png)

1. Följande två alternativ är tillgängliga när du vill publicera mediefiler till Brand Portal:
   * [Publicera resurser direkt](#publish-to-bp-now)
   * [Publicera resurser senare](#publish-to-bp-now)

## Publicera resurser nu {#publish-to-bp-now}

Gör något av följande för att publicera de markerade resurserna på varumärkesportalen:

* Välj **[!UICONTROL Quick Publish]** i verktygsfältet. Välj sedan **[!UICONTROL Publish to Brand Portal]** på menyn.

* Välj **[!UICONTROL Manage Publication]** i verktygsfältet.

   1. Från **[!UICONTROL Action]** väljer du sedan **[!UICONTROL Publish to Brand Portal]** och från **[!UICONTROL Scheduling]** väljer du **[!UICONTROL Now]**. Klicka på **[!UICONTROL Next]**.

   2. Bekräfta ditt val i **[!UICONTROL Scope]** och klicka på **[!UICONTROL Publish to Brand Portal]**.

Ett meddelande visas som anger att resurserna har placerats i kö för publicering på varumärkesportalen. Logga in på gränssnittet för varumärkesportalen för att visa de publicerade resurserna.

## Publicera resurser senare {#publish-to-bp-later}

Gör så här för att schemalägga publicering av resurser på varumärkesportalen till ett senare datum eller en senare tid:

1. När du har valt resurser/mappar att publicera väljer du **[!UICONTROL Manage Publication]** i verktygsfältet högst upp.

1. På sidan **[!UICONTROL Manage Publication]** väljer du **[!UICONTROL Publish to Brand Portal]** från **[!UICONTROL Action]** och sedan **[!UICONTROL Later]** från **[!UICONTROL Scheduling]**.

   ![publishlaterbp-1](assets/publishlaterbp-1.png)

1. Markera en **[!UICONTROL Activation date]** och ange en tid. Klicka på **[!UICONTROL Next]**.

1. Välj ett **aktiveringsdatum** och ange en tid. Klicka på **Nästa**.

1. Ange en **[!UICONTROL Workflow title]** i **[!UICONTROL Workflows]**. Klicka på **[!UICONTROL Publish Later]**.

   ![publishworkflow](assets/publishworkflow.png)

Logga sedan in på Brand Portal för att se om de publicerade resurserna finns tillgängliga i Brand Portal gränssnitt.

![bp_landingpage](assets/bp_landingpage.png)

## Visa publicerad fil eller mapp i Brand Portal {#view-published-file-folder}

1. Logga in på gränssnittet för varumärkesportalen för att visa de publicerade resurserna (beroende på schemalagt datum eller tid).

   ![bp_landingpage](assets/bp_landingpage.png)

1. Växla till listvyn ![listvyn](assets/list-view.svg) om du vill visa resursens aktuella publiceringsstatus.

<!--2. On the [Asset Reports page](#https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/asset-reports), you can see the current state of the report job, for example, Success, Failed, Queued, or Scheduled.-->

![genererad rapportstatus](assets/report-status.JPG)
