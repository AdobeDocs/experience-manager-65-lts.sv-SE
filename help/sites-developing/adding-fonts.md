---
title: Lägga till teckensnitt för grafikåtergivning
description: Med AEM kan du skapa grafik som innehåller text dynamiskt från ditt innehåll
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 5ceaa9f0-aba1-40a3-97ef-f5ade0c2a54a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# Lägga till teckensnitt för grafikåtergivning{#adding-fonts-for-graphic-rendering}

Med AEM kan du skapa grafik som innehåller text dynamiskt från ditt innehåll.

Om du vill göra det kan du även läsa in och använda dina egna teckensnitt.

För närvarande stöder alla implementeringar av Java-plattformen [TrueType](https://en.wikipedia.org/wiki/Truetype)-teckensnitt.

1. Öppna CRXDE Lite och navigera till projektprogrammappen:

   `/apps/<your-project>/`

1. Skapa en nod under `/apps/<your-project>/`:

   * **Namn**: `fonts`
   * **Typ**: `sling:Folder`

   Spara alla ändringar.

1. Kopiera teckensnittsfilerna till den här mappen, till exempel med WebDAV.

   >[!NOTE]
   >
   >Teckensnittsfiler i databasen måste ha suffixet `*.ttf` eller `*.TTF`.

1. Uppdatera [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) för [Day Commons GFX Font Helper](/help/sites-deploying/osgi-configuration-settings.md). Lägg till sökvägen till teckensnittsmappen, det vill säga `/apps/<your-project>/fonts`.

1. Kom tillbaka till CRXDE Lite. Nu bör du se en `.fontlist`-nod i mappen som innehåller namnet på de importerade teckensnitten.

   Dessa teckensnitt kan nu användas i Java API.

Mer information om hur du använder teckensnitten med Java API finns i [dokumentationen för klassen Font i Java API](https://download.oracle.com/javase/6/docs/api/java/awt/Font.html).
