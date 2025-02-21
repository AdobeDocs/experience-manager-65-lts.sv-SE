---
title: Admin Consoles
description: Lär dig hur du använder de Admin Consoles som finns i Adobe Experience Manager.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---


# Admin Consoles{#admin-consoles}

Som standard är möjligheten att växla till det klassiska användargränssnittet via Admin Consoles inaktiverad. Därför visas inte längre de popup-ikoner som visades när användaren placerade musen över vissa konsolikoner, vilket ger åtkomst till det klassiska gränssnittet.

Alla konsoler som har en klassisk användargränssnittsversion i `/libs/cq/core/content/nav` kan återaktiveras individuellt så att alternativet **Klassiskt användargränssnitt** visas igen över konsolikonen när användaren för musen över den.

I det här exemplet återaktiverar du det klassiska gränssnittet för webbplatskonsolen.

1. Använd CRXDE Lite för att hitta noden som motsvarar den Admin Console som du vill aktivera Classic-användargränssnittet för igen. De finns under:

   `/libs/cq/core/content/nav`

   Till exempel

   [`https://localhost:4502/crx/de/index.jsp#/libs/cq/core/content/nav`](https://localhost:4502/crx/de/index.jsp#/libs/cq/core/content/nav)

1. Markera noden som motsvarar konsolen som du vill återaktivera det klassiska användargränssnittet för. I det här exemplet återaktiverar du det klassiska användargränssnittet för webbplatskonsolen.

   `/libs/cq/core/content/nav/sites`

1. Skapa en övertäckning med alternativet **Överläggsnod**, till exempel:

   * **Sökväg**: `/apps/cq/core/content/nav/sites`
   * **Plats för övertäckning**: `/apps/`
   * **Matcha nodtyper**: aktiv (markera kryssrutan)

1. Lägg till följande boolesk egenskap till den överlagrade noden:

   `enableDesktopOnly = {Boolean}true`

1. Alternativet **Klassiskt användargränssnitt** är igen tillgängligt som ett poseralternativ i Admin Console.

   ![Alternativ för klassisk UI-pekare](assets/syui-01-2019-02-27-15-16-55.png)

Upprepa dessa steg för varje konsol som du vill återaktivera åtkomst till den klassiska användargränssnittsversionen för.
