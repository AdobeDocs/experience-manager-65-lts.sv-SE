---
title: Hur hantering av flera webbplatser för riktat innehåll är strukturerad
description: Ett diagram visar hur stöd för flera webbplatser för riktat innehåll är strukturerat
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: personalization
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 2%

---

# Hur hantering av flera webbplatser för riktat innehåll är strukturerad{#how-multisite-management-for-targeted-content-is-structured}

I följande diagram visas hur stöd för flera webbplatser för riktat innehåll är strukturerat.

Områden visas under **/content/campaign/&lt;brand>** och som standard har varje varumärke ett huvudområde, som skapas automatiskt. Varje område innehåller egna aktiviteter, upplevelser och erbjudanden.

![chlimage_1-268](assets/chlimage_1-268.png)

Om du vill söka efter riktat innehåll kan sidorna eller platserna mappas till ett område. Om inget område är konfigurerat återgår AEM till huvudområdet för det här specifika varumärket.

Följande diagram är ett exempel på hur logiken fungerar för tre platser, som kallas site1, site2 och site3.

![chlimage_1-269](assets/chlimage_1-269.png)

* site1 söker upp myarea1 för brand1 och other area2 för brand2 baserat på områdesmappning.
* site2 söker upp myarea1 för brand1 och master area för brand2 eftersom endast områdesmappningen för brand1 har definierats.
* site3 söker upp huvudområdet för brand1 och brand2 eftersom ingen annan områdesmappning har definierats alls för den här webbplatsen.
