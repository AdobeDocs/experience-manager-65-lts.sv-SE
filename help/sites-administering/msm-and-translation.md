---
title: Hanterare och översättning av flera webbplatser
description: Lär dig hur du återanvänder ditt innehåll i ditt projekt och hanterar flerspråkiga webbplatser i Adobe Experience Manager.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: site-features
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Multi Site Manager, Language Copy
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Hanterare och översättning av flera webbplatser {#msm-and-translation}

Följande administrationsverktyg finns för att hantera webbplatser och sidor:

* Med Multi Site Manager (MSM) kan du använda samma webbplatsinnehåll på flera platser samtidigt som du kan använda olika varianter:

   * [Återanvända innehåll: Multi Site Manager och Live Copy](/help/sites-administering/msm.md)

* Med översättningen kan ni automatisera översättningen av sidinnehåll, resurser och användargenererat innehåll för att skapa och underhålla flerspråkiga webbplatser:

   * [Översätta innehåll för flerspråkiga webbplatser](/help/sites-administering/translation.md)

* Dessa två funktioner kan kombineras för att passa för webbplatser som är både [Flerspråkiga och flerspråkiga](#multinational-and-multilingual-sites).

## Flerspråkiga och flerspråkiga webbplatser {#multinational-and-multilingual-sites}

Ni kan effektivt skapa innehåll för multinationella och flerspråkiga webbplatser genom att kombinera Multi Site Manager och arbetsflödet för översättning. Skapa en huvudwebbplats på ett språk, för ett visst land, och använd sedan innehållet som grund för de andra webbplatserna, med översättning där det behövs:

* [Översätt](/help/sites-administering/translation.md) huvudwebbplatsen till olika språk.

* Använd [Multi Site Manager](/help/sites-administering/msm.md) för att:

   * Återanvänd innehåll från huvudwebbplatsen och översättningarna för att skapa webbplatser för andra länder och kulturer.
   * Se till att begränsa användningen av Multi Site Manager till innehåll på ett språk, till exempel engelsk master > engelska språkgrenar på landsplatser, fransk master > franska språkgrenar på landsplatser.
   * Om det behövs frigör du element från live-kopiorna för att lägga till lokaliseringsinformation.

I följande diagram visas hur huvudbegreppen överlappar (men inte alla nivåer/element som berörs):

![Diagram som visar huvudbegrepp för MSM och översättning](assets/chlimage_1-71a.png)

>[!NOTE]
>
>I detta fall, och på liknande sätt, hanterar inte MSM de olika språkversionerna som sådana.
>
>* [MSM](/help/sites-administering/msm.md) hanterar distributionen av översatt innehåll från en plan (till exempel en global master) till live-kopiorna (till exempel de lokala platserna) inom ett språks gränser.
>* Integreringsfunktionerna i [translation](/help/sites-administering/translation.md) i AEM, tillsammans med tredjepartstjänster för översättningshantering, hanterar språken och översätter innehåll till dessa olika språk.
>
>För mer avancerade användningsområden kan MSM användas även av flerspråkiga mallsidor.

>[!NOTE]
>
>För alla användningsfall rekommenderas följande metodtips:
>
>* [Bästa praxis för MSM](/help/sites-administering/msm-best-practices.md), särskilt:
>
>   * [Skapa plats](/help/sites-administering/msm-best-practices.md#create-site)
>   * [MSM och flerspråkiga webbplatser](/help/sites-administering/msm-best-practices.md#msm-and-multilingual-websites)
>
>* [Bästa metoder för översättning](/help/sites-administering/tc-bp.md)
