---
title: Publicera samlingar på Brand Portal
description: Lär dig hur du publicerar och avpublicerar samlingar till Brand Portal.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: brand-portal
content-type: reference
docset: aem65
feature: Brand Portal
role: User
solution: Experience Manager, Experience Manager Assets
exl-id: 1456a32e-c98f-4106-8546-799614c51a59
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 22%

---

# Publicera samlingar på Brand Portal {#publish-collections-to-brand-portal}

Som administratör för Adobe Experience Manager (AEM) Assets kan du publicera samlingar i AEM Assets Brand Portal-instansen för din organisation. Du måste dock först integrera AEM Assets med Brand Portal. Mer information finns i [Konfigurera AEM Assets med varumärkesportalen](/help/assets/configure-aem-assets-with-brand-portal.md).

Om du gör senare ändringar i den ursprungliga samlingen i AEM Assets återspeglas inte ändringarna i Brand Portal förrän du publicerar samlingen igen. Den här egenskapen ser till att ändringar i pågående arbete inte är tillgängliga i Brand Portal. Endast godkända ändringar som publiceras av en administratör finns i varumärkesportalen.

>[!NOTE]
>
>Det går inte att publicera innehållsfragment på varumärkesportalen. Om du väljer innehållsfragment i AEM Author är åtgärden **Publicera i Brand Portal** därför inte tillgänglig.
>
>Om samlingar som innehåller innehållsfragment publiceras från AEM Author till Brand Portal replikeras allt innehåll i mappen, förutom innehållsfragment, till Brand Portal gränssnitt.

## Publicera en samling på Brand Portal {#publish-a-collection-to-brand-portal}

1. Klicka på AEM-logotypen i användargränssnittet för AEM Assets.
1. Gå till **Assets > Samlingar** från **navigeringssidan**.
1. På Samlingar-konsolen väljer du den samling du vill publicera till Brand Portal.

   ![select_collection](assets/select_collection.png)

1. Klicka på **Publicera till Brand Portal** i verktygsfältet.
1. Klicka på **Publicera** i bekräftelsedialogrutan.
1. Stäng bekräftelsemeddelandet.
1. Logga in på Brand Portal som administratör. Den publicerade samlingen är tillgänglig i samlingskonsolen.

   ![published collection](assets/published_collection.png)

## Avpublicera samlingar {#unpublish-collections}

Du kan avpublicera samlingar som du publicerar från AEM Assets till Brand Portal. När du har avpublicerat originalsamlingen är kopian inte längre tillgänglig för Brand Portal-användare.

1. Gå till Samlingar-konsolen för din AEM Assets-instans och välj den samling som du vill avpublicera.

   ![select_collection-1](assets/select_collection-1.png)

1. Klicka på ikonen **Ta bort från Brand Portal** i verktygsfältet.
1. Klicka på **Avpublicera** i dialogrutan.
1. Stäng bekräftelsemeddelandet. Samlingen tas bort från varumärkesportalens gränssnitt.
