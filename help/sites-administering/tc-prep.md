---
title: Förbereder innehåll för översättning
description: Lär dig förbereda innehåll för översättning i Adobe Experience Manager.
contentOwner: Guillaume Carlino
feature: Language Copy
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 1%

---

# Förbereder innehåll för översättning{#preparing-content-for-translation}

Flerspråkiga webbplatser har i allmänhet en viss mängd innehåll på flera språk. Webbplatsen är skriven på ett språk och sedan översatt till andra språk. Vanligtvis består flerspråkiga webbplatser av sidgrenar, där varje gren innehåller webbplatsens sidor på ett annat språk.

Geometrixx Demo Site innehåller flera språkgrenar och följande struktur används:

```xml
/content
    |- geometrixx
             |- en
             |- fr
             |- de
             |- es
             |- it
             |- ja
             |- zh
```

Varje språkgren på en webbplats kallas för en språkkopia. Rotsidan för en språkkopia, som kallas språkroten, identifierar språket för innehållet i språkkopian. `/content/geometrixx/fr` är till exempel språkroten för den franska språkkopian. Språkkopior måste använda en [korrekt konfigurerad språkrot](/help/sites-administering/tc-prep.md#creating-a-language-root) så att rätt språk används när översättningar av en källplats utförs.

Den språkkopia som du ursprungligen skapade webbplatsinnehållet för är språkinställningen. Språkmallsidan är källan som översätts till andra språk.

Gör så här för att förbereda webbplatsen för översättning:

1. Skapa språkroten för din språkinställning. Språkroten för Geometrixx demowebbplats är till exempel /content/geometrixx/en. Kontrollera att språkroten är korrekt konfigurerad enligt informationen i [Skapa en språkrot](/help/sites-administering/tc-prep.md#creating-a-language-root).
1. Skriv innehållet i din språkmaster.
1. Skapa språkroten för varje språkkopia för webbplatsen. Den franska språkkopian av Geometrixx exempelwebbplats är till exempel /content/geometrixx/fr.

När du har förberett innehållet för översättning kan du automatiskt skapa saknade sidor i dina språkkopior och tillhörande översättningsprojekt. (Se [Skapa ett översättningsprojekt](/help/sites-administering/tc-manage.md).) En översikt över innehållsöversättningsprocessen i AEM finns i [Översätta innehåll för flerspråkiga webbplatser](/help/sites-administering/translation.md).

## Skapa en språkrot {#creating-a-language-root}

Skapa en språkrot som rotsida för en språkkopia som identifierar språket i innehållet. När du har skapat språkroten kan du skapa översättningsprojekt som innehåller språkkopian.

Om du vill skapa språkroten skapar du en sida och använder en ISO-språkkod som värde för egenskapen Namn. Språkkoden måste ha något av följande format:

* `<language-code>`Den språkkod som stöds är en kod med två bokstäver som definieras av ISO-639-1, till exempel `en`.

* `<language-code>_<country-code>` eller `<language-code>-<country-code>`Den landskod som stöds är en kod med två bokstäver (gemener) eller versaler (versaler) enligt ISO 3166, till exempel `en_US`, `en_us`, `en_GB`, `en-gb`.

Du kan använda båda formaten enligt den struktur som du har valt för den globala platsen.  Rotsidan för den franska språkkopian av Geometrixx-webbplatsen har till exempel `fr` som Name-egenskap. Egenskapen Namn används som namn på sidnoden i databasen och avgör därför sidans sökväg. (http://localhost:4502/content/geometrixx/fr.html)

I följande procedur används det pekoptimerade användargränssnittet för att skapa en språkkopia av en webbplats. Instruktioner om hur du använder det klassiska användargränssnittet finns i [Skapa en språkrot med det klassiska användargränssnittet](/help/sites-administering/tc-lroot-classic.md).

1. Navigera till Webbplatser.
1. Klicka på den webbplats som du vill skapa en språkkopia för.

   Om du till exempel vill skapa en språkkopia av Geometrixx Outdoors webbplats klickar du på Geometrixx Outdoors webbplats.

1. Klicka på Skapa och sedan på Skapa sida.

   ![chlimage_1-21](assets/chlimage_1-21a.png)

1. Markera sidmallen och klicka sedan på Nästa.
1. I fältet Namn skriver du landskoden i formatet `<language-code>` eller `<language-code>_<country-code>`, till exempel `en`, `en_US`, `en_us`, `en_GB`, `en_gb`. Skriv en rubrik för sidan.

   ![chlimage_1-22](assets/chlimage_1-22a.png)

1. Klicka på Skapa. Klicka på **Klar** i bekräftelsedialogrutan om du vill gå tillbaka till webbplatskonsolen, eller **Öppna** om du vill öppna språkkopian.

## Se status för språkrötter {#seeing-the-status-of-language-roots}

Det pekoptimerade användargränssnittet innehåller en referenspanel som visar en lista med språkrötter som har skapats.

![chlimage_1-23](assets/chlimage_1-23a.png)

I följande procedur används det pekoptimerade användargränssnittet för att öppna referenspanelen för en sida.

1. På webbplatskonsolen markerar du en sida på webbplatsen och klickar sedan på **Referenser**.

   ![chlimage_1-24](assets/chlimage_1-24a.png)

1. Klicka på **Språkkopior** på referenspanelen. På panelen Språkkopior visas språkkopiorna för webbplatsen.
