---
title: Multi-tenancy för mallar för samlingar, fragment och fragment
description: Lär dig hur ni med funktionen för flera innehavare kan dela upp innehåll i CRX-databasen baserat på kundorganisationen för att förhindra obehörig åtkomst.
contentOwner: AG
role: Architect, Admin, Leader
feature: Collections
solution: Experience Manager, Experience Manager Assets
exl-id: 39e14f89-8e60-4b5e-8859-d69ebd51864e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Multi-tenancy för mallar för samlingar, fragment och fragment {#multi-tenancy-for-collections-snippets-and-snippet-templates}

Med funktionen för flera innehavare kan du separera innehåll i CRX baserat på organisationens prefix och organisations-ID för att skydda innehållet från obehörig åtkomst för användare i andra organisationer.

[!DNL Adobe Experience Manager Assets] lagrar data för varje organisation på olika sökvägar. Varje organisationsspecifik sökväg identifieras av organisationens prefix och organisations-ID
som ingår i den traditionella platsen där olika typer av mediefiler lagras i CRX.

Om du till exempel skapar en mapp med namnet `Demo`, lagrar [!DNL Experience Manager]-resurser vanligtvis mappen på `../content/dam/Demo`. När multi-tenancy är aktiverat kan du nu lagra data på `../content/dam/<organization prefix>/<organization id>Demo`

Om till exempel [!DNL Adobe Marketing Cloud] användare av [!DNL Assets] (på begäran) som har tilldelats organisationen `aodpremium` kan du använda funktionen för flera innehavare för att konfigurera sökvägen `../content/dam/<mac>/<aodpremium>Demo` för att dela upp innehållet. I det här exemplet är `mac` organisationsprefixet och `aodpremium` är organisations-ID.

Baserat på användarens organisation och ID visas den här kvalificerade sökvägen i [!DNL Assets]-gränssnittet och i olika guider, inklusive guiderna för att skapa Flytta och Fragment för att framtvinga separering.

Med funktionen Multi-tenancy kan du dela upp följande typer av resurser och komponenter:

* Samlingar
* Offentliga samlingar
* Kataloger (inklusive guiden Lägg till/välj sida)
* Mallar
* Fragmentmallar
* Ljuslåda
