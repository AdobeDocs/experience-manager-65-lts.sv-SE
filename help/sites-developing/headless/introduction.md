---
title: Headless Development for AEM 6.5 Sites
description: Läs om hur AEM 6.5 har kraftfulla headless-funktioner som Content Models, Content Fragments och GraphQL API tillsammans så att du kan hantera upplevelserna centralt och leverera dem i olika kanaler.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments,GraphQL,Persisted Queries,Developing
role: Admin,Architect,Data Architect,Developer
exl-id: 4eb42d3a-f869-4831-9aaf-58e7272bd1fe
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# Headless Development for AEM 6.5 Sites {#headless-development}

Läs om hur AEM 6.5 har kraftfulla headless-funktioner som Content Models, Content Fragments och GraphQL API tillsammans så att du kan hantera upplevelserna centralt och leverera dem i olika kanaler.

## Ökning {#overview}

Headless-implementering blir allt viktigare för att ni ska kunna leverera upplevelser till er målgrupp, oavsett var de finns och kanal.

Den Headless-implementeringen förskjuter hantering av sidor och komponenter på samma sätt som traditionella lösningar med kompletta stackar och hybridlösningar och fokuserar på att skapa kanalneutrala, återanvändbara fragment av innehåll och deras flerkanalsleverans. Det är ett modernt och dynamiskt utvecklingsmönster för implementering av webbupplevelser.

![AEM implementeringsmodeller](/help/sites-developing/headless/getting-started/assets/aem-implementation-models.png)

## Jämför Headful och Headless {#headful-headless}

Det här dokumentet fokuserar på AEM fullständiga headless-implementeringsmodell. Du behöver dock inte vara binärt i AEM för att vara både headful och headless. Headless-funktioner kan användas för att hantera och leverera innehåll till olika slutpunkter och samtidigt göra det möjligt för innehållsförfattare att redigera single page-applikationer. Allt i AEM.

>[!TIP]
>
>Mer information finns i dokumentet [Headful and Headless in AEM](/help/sites-developing/headful-headless.md).

## AEM 6.5 och Headless {#aem-headless}

AEM 6.5 är ett flexibelt verktyg för den headless-implementeringsmodellen med tre kraftfulla tjänster:

1. Innehållsmodeller
   * Innehållsmodeller är strukturerade representationer av innehåll.
   * Dessa definieras av informationsarkitekter i AEM Content Fragment Model Editor.
   * Innehållsmodeller fungerar som bas för innehållsfragment.
1. Innehållsfragment
   * Innehållsfragment är instansieringar av innehållsmodeller.
   * Dessa skapas av innehållsförfattare med AEM Content Fragment Editor.
   * De lagras i AEM Assets och hanteras i användargränssnittet för Assets Admin.
1. Innehålls-API för leverans
   * AEM GraphQL API har stöd för leverans av innehållsfragment.
   * AEM Assets REST API stöder CRUD-åtgärder för innehållsfragment.
   * Direktinnehållsleverans är också möjligt med [Content Fragment Core Component&#39;s JSON export.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=sv-SE)

## Dina första steg med AEM Headless {#first-steps}

Det finns flera resurser som du kan använda för att komma igång med AEM headless-funktioner. De är avsedda för olika användningsområden, men ger alla en gedigen översikt över AEM headless-funktioner.

| Resurs | Beskrivning | Typ | Målgrupp | Beräkna. Tid |
|---|---|---|---|---|
| [Headless Developer Journey](/help/journey-headless/developer/overview.md) | **För användare som inte har använt AEM tidigare och headless** kan du börja här för att få en omfattande introduktion till AEM och dess headless-funktioner, från teorin om headless till att börja med ditt första headless-projekt. | Guide | Utvecklare **nybörjare i AEM och headless** | 1 timme |
| [Starthandbok för Headless](/help/sites-developing/headless/getting-started/introduction.md) | **För erfarna AEM-användare** som behöver en kort sammanfattning av de viktigaste rubrikfria funktionerna i AEM kan du ta en titt på den här snabbstartsöversikten. | Snabbstart | Utvecklare, administratörer **med AEM** | 20 minuter |
| [Komma igång med självstudiekurs för AEM Headless ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=sv-SE) | **Om du föredrar en praktisk strategi och är bekant med AEM** kan du använda den här självstudiekursen för att skapa ett enkelt headless-projekt. | Självstudiekurs | Utvecklare | 2 timmar |
| [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE) | Den här resurssamlingen tillhandahålls för både **nya**- och **erfarna**-utvecklare. | Resurser | Utvecklare | |
