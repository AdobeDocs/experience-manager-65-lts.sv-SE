---
title: Aktivera skydd för aktiva länkar i Dynamic Media
description: Information om hur du aktiverar hot link-skydd i Dynamic Media.
contentOwner: Rick Brough
topic-tags: dynamic-media
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
content-type: reference
role: User, Admin
feature: Configuration
solution: Experience Manager, Experience Manager Assets
exl-id: 56eb956e-c6a8-464b-980a-28e0dab0da7c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Aktivera hot link-skydd i Dynamic Media {#activating-hotlink-protection-in-dynamic-media}

Aktiv länkning är när en tredjepartswebbplats använder HTML-kod för att visa en bild från din webbplats. De använder din bandbredd varje gång bilden efterfrågas eftersom besökarens webbläsare öppnar den direkt från servern. Hotlink *protection* är en metod som förhindrar att andra webbplatser direkt länkar till bilder, CSS eller JavaScript på dina webbsidor. Den här typen av skydd minskar onödig bandbreddsanvändning under ditt Dynamic Media-konto.

[Experience Manager kundsupport](https://experienceleague.adobe.com/sv?support-solution=Experience+Manager#support) kan konfigurera ett referensfilter på CDN-nivå (Content Delivery Network) så att Dynamic Media-innehåll endast kan skickas till webbplatser i din lista över tillåtna webbplatser för domänen.

>[!NOTE]
>
>Den här funktionen kräver att du använder det färdiga CDN som medföljer Adobe Experience Manager Dynamic Media. Andra anpassade CDN stöds inte med den här funktionen. För att aktivera aktivering av hot link-skydd måste en administratör skapa en Adobe kundsupportbiljett för att begära konfigurationsändringen av ditt Dynamic Media-konto. Det kostar inget mer att aktivera skydd av aktiva länkar.
