---
title: Använda taggar
description: Taggar är ett snabbt och enkelt sätt att klassificera innehåll på en webbplats. Taggar kan ses som nyckelord eller etiketter som kan bifogas till en sida, en resurs eller annat innehåll för att göra det möjligt att söka efter innehållet och relaterat innehåll.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: 6489c2cf-d4a5-452e-a554-5e814aa196b7
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# Använda taggar{#using-tags}

Taggar är ett snabbt och enkelt sätt att klassificera innehåll på en webbplats. Taggar kan ses som nyckelord eller etiketter som kan bifogas till en sida, en resurs eller annat innehåll för att göra det möjligt att söka efter innehållet och relaterat innehåll.

* Mer information om hur du skapar och hanterar taggar och till vilka innehållstaggar har tillämpats finns i [Administrera taggar](/help/sites-administering/tags.md) .
* Mer information om taggningsramverket och hur du inkluderar och utökar taggar i anpassade program finns i [Tagga för utvecklare](/help/sites-developing/tags.md).

## Tio skäl att använda märkord {#ten-reasons-to-use-tagging}

1. Ordna innehåll: taggning gör livet enklare för författare eftersom de snabbt kan ordna innehåll utan ansträngning.
1. Ordna taggar: när taggar organiserar innehåll organiserar hierarkiska taxonomier/namnutrymmen taggar.
1. Djupt organiserade taggar: med möjlighet att skapa taggar och undertaggar blir det möjligt att uttrycka hela taxonomiska system som omfattar termer, undertermer och deras relationer. Detta gör att en andra (eller tredje) innehållshierarki kan skapas parallellt med den officiella.
1. Kontrollerad taggning: taggning kan styras genom att behörigheter tillämpas på taggar och/eller namnutrymmen för att styra hur taggar skapas och används.
1. Flexibel taggning: Taggar har många namn och ansikten: taggar, taxonomitermer, kategorier, etiketter och mycket annat. De är flexibla i sin innehållsmodell och i hur de kan användas, t.ex. när måldemografi kontureras, kategoriseras och klassificeras innehåll eller när en sekundär innehållshierarki skapas.
1. Förbättrad sökning: Standardsökkomponenten i AEM innehåller i stort sett skapade taggar och tillämpade taggar som du kan använda filter på för att begränsa resultaten till dem som är relevanta.
1. SEO-aktivering: taggar som används som sidegenskaper visas automatiskt i de metataggar på sidan som gör den synlig för sökmotorer.
1. Enkel sofistikering: taggar kan skapas från ett ord och med en knapptryckning. Därefter kan en titel, beskrivning och ett obegränsat antal etiketter läggas till för att ge taggen mer semantik.
1. Core Consistency : taggningssystemet är en kärnkomponent i AEM och används av alla AEM-funktioner för att kategorisera innehåll. Programmeringsgränssnittet för taggning är även tillgängligt för utvecklare som skapar taggningsaktiverade program med tillgång till samma taxonomier.
1. Kombinerar struktur och flexibilitet: AEM är idealiskt för att arbeta med strukturerad information på grund av inkapsling av sidor och banor. Det är lika kraftfullt när du arbetar med ostrukturerad information på grund av den inbyggda textsökningen. Taggning kombinerar styrkan hos både strukturen och flexibiliteten.

När du utformar innehållsstrukturen för en plats och metadatarammet för resurser bör du tänka på att taggningen är enkel och tillgänglig.

## Tillämpar taggar {#applying-tags}

I redigeringsmiljön kan författare lägga till taggar genom att gå till sidegenskaperna och ange en eller flera taggar i fältet **Taggar/nyckelord**.

Om du vill använda [fördefinierade taggar](/help/sites-administering/tags.md) använder du `Tags/Keywords`-fältets listruta i fönstret **Sidegenskaper** och väljer en tagg som är tillåten för sidan. Fliken **Standardtaggar** är standardnamnutrymmet, vilket innebär att det inte finns något prefix `namespace-string:` för taxonomin.

![chlimage_1-2](assets/chlimage_1-2a.png)

### Publiceringstaggar {#publishing-tags}

Precis som med sidor kan du göra följande med taggar och namnutrymmen:

**Aktivera**

* Aktivera enskilda taggar.

  Precis som för sidor måste nyligen skapade taggar aktiveras innan de blir tillgängliga i publiceringsmiljön.

>[!NOTE]
>
>När du aktiverar en sida öppnas en dialogruta automatiskt där du kan aktivera oaktiverade taggar som hör till sidan.

**Inaktivera**

* Inaktivera de markerade taggarna.

## Tagga moln {#tag-clouds}

Taggmoln visar ett moln med taggar, antingen för den aktuella sidan, för hela webbplatsen eller för de mest använda. Tagga moln är ett sätt att markera problem som är (har varit) av intresse för användaren. Storleken på texten som används för att visa taggen varierar beroende på hur den används.

Komponenten [Taggmoln](/help/sites-classic-ui-authoring/classic-page-author-edit-mode.md#tag-cloud) (allmän komponentgrupp) används för att lägga till ett taggmoln på en sida.

## Söka på taggar {#searching-on-tags}

Du kan söka efter taggar både i författarmiljön och i publiceringsmiljön.

### Använda sökkomponenten {#using-search-component}

Om du lägger till en [sökkomponent](/help/sites-classic-ui-authoring/classic-page-author-edit-mode.md#search) på en sida får du en sökfunktion som innehåller taggar och kan användas i både författarmiljön och publiceringsmiljön.

![chlimage_1-3](assets/chlimage_1-3a.png)
