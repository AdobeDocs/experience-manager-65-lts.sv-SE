---
title: Namnkonventioner för tillgångstestning
description: Noderna i databasen omfattas av namnkonventioner i Java Content Repository. Adobe Experience Manager inför dock ytterligare konventioner för resursnodernas namn.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: authoring
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: 37de1a8b-b7db-469e-98a7-20ddb6218510
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 0%

---

# Namnkonventioner för tillgångstestning{#naming-conventions-for-assets-testing}

Noderna i databasen omfattas av namnkonventioner i [Java Content Repository](/help/sites-developing/the-basics.md#java-content-repository). Adobe Experience Manager inför dock ytterligare konventioner för resursnodernas namn.

## Klassiskt användargränssnitt {#classic-ui}

Det klassiska användargränssnittet har tätare begränsningar:

* Validerar resursnamnet när ett explicit nodnamn är:

   * en resurstitel tillhandahålls för konvertering till nodnamnet
   * ett explicit nodnamn har angetts

* Giltiga tecken (endast dessa tecken är giltiga när en resurs skapas i det klassiska användargränssnittet):

   * &#39;a&#39; till &#39;z&#39;
   * &#39;A&#39; till &#39;Z&#39;
   * 0 till 9
   * _ (understreck)
   * `-` (tankstreck/minustecken)
