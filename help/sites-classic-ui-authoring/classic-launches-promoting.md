---
title: Befordra lanseringar
description: Du måste befordra startsidor för att kunna flytta tillbaka innehållet till källan (produktionen) innan du publicerar. När en startsida befordras ersätts motsvarande sida på källsidorna med innehållet på den befordrade sidan.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
legacypath: /content/docs/en/aem/6-0/author/site-page-features/launches
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: 1167735d-a13a-438e-bef8-205e27f59f4e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Befordra lanseringar{#promoting-launches}

Du måste befordra startsidor för att kunna flytta tillbaka innehållet till källan (produktionen) innan du publicerar. När en startsida befordras ersätts motsvarande sida på källsidorna med innehållet på den befordrade sidan. Följande alternativ är tillgängliga när du befordrar en startsida:

* Anger om bara den aktuella sidan eller hela programstarten ska befordras.
* Anger om underordnade sidor för den aktuella sidan ska befordras.
* Anger om en fullständig start ska erbjudas eller endast sidor som har ändrats.

## Marknadsför startsidor {#promoting-launch-pages}

Om du vill befordra sidor utför du följande steg när du redigerar startsidan som du vill befordra:

1. Klicka på **Befordra start** på fliken **Sida** i Sidekick.
1. Ange vilka sidor som ska befordras:

   * (Standard) Om du bara vill befordra den aktuella sidan väljer du **Befordra sidändringar till produktionsversion**.
   * Om du även vill befordra den aktuella sidans underordnade sidor väljer du **Inkludera undersidor**.
   * Om du vill befordra alla sidor i starten väljer du **Befordra fullständig start till produktionsversion**.

1. Om du vill lägga till produktionssidorna i ett arbetsflödespaket väljer du **Lägg till i arbetsflödespaket** och sedan arbetsflödespaketet.
1. Klicka på **Befordra**.

## Bearbeta befordrade sidor med AEM Workflow {#processing-promoted-pages-using-aem-workflow}

Använd arbetsflödesmodeller för att utföra massbearbetning av befordrade startsidor:

1. Skapa ett arbetsflödespaket.
1. När författare befordrar startsidor lagrar de dem i arbetsflödespaketet.
1. Starta en arbetsflödesmodell med paketet som nyttolast.

Om du vill starta ett arbetsflöde automatiskt när sidor befordras, [konfigurerar du en arbetsflödeslungare](/help/sites-administering/workflows-starting.md#workflows-launchers) för paketnoden.

Du kan t.ex. automatiskt generera begäranden om sidaktivering när författare befordrar startsidor. Konfigurera en startfunktion för arbetsflödet för aktivering av begäran när paketnoden ändras.

![chlimage_1-136](assets/chlimage_1-136.png)
