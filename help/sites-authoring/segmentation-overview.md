---
title: Om segmentering när du skapar en kampanj
description: Segmentering är en viktig faktor när man skapar en kampanj.
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization,Integration
role: User,Admin,Architect,Developer
exl-id: 7167c672-8d24-4493-aff6-b5b453074bff
source-git-commit: 36022b939d598a43ee7f5339ed04122e476205b7
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# Förstå segmentering{#understanding-segmentation}

Segmentering är en viktig faktor när man skapar en kampanj. Oftast måste ni ha definierade segment innan ni kan påbörja kampanjen.

Besökare har olika intressen och mål när de besöker en webbplats. Att förstå dessa mål och uppfylla förväntningarna är en viktig framgångsrik faktor för onlinemarknadsföring.

Segmentering hjälper till att uppnå detta genom att analysera och karakterisera besökarens:

* aktivitet på webbplatsen
* profil
* aktivitet på andra webbplatser

Innehållet kan sedan anpassas efter besökarens behov och intressen, beroende på vilka segment de matchar.

## Använda segmentering {#using-segmentation}

Segment definieras i [Konfigurera segmentering](/help/sites-administering/campaign-segmentation.md). De används för att styra det faktiska innehåll som ses av en viss målgrupp.

## Segmenteringstermin {#segmentation-terminology}

Vid diskussion av segmentering används följande terminologi:

**Besökare** - En besökare är en person som besöker en webbplats. Personens besök börjar oftast från en hänvisande sida och går sedan vidare till en eller flera sidvisningar på din egen webbplats. En beteendeprofil kan skapas utifrån detaljerna från den personens besök.

**Användare** - En användare är en besökare som registrerar sig hos webbplatsen för att ta emot en kontoprofil. För att kunna generera en profil måste de tillhandahålla ytterligare identifiering, till exempel en e-postadress och ett kön. Ytterligare information kan också samlas in, bland annat om communityaktiviteter och köpmönster. Baserat på informationen i profilen kan en demografisk profil skapas.

**Trait** - En trait är en egenskap eller egenskap hos en besökare som kan användas för att bestämma medlemskap i ett visst segment.

**Segment** - Ett segment är en samling besökare som delar vissa egenskaper. Segmenten bör vara distinkta, med ett minimum av överlappning med andra segment.

**Beteenden** - Beteenden är sådana som relaterar till en besökares beteende på webbplatsen. Bland dessa finns:

* Intresset på er webbplats, inklusive besökta sidor och köpta produkter.
* Intresse för den refererande webbplatsen, inklusive söktermer som används eller annonser som klickas.
* Intressen av andra sajter; bestäms med verktyg som Spyjax.
* Besökarnas lojalitet; besökets varaktighet, besöksfrekvens.

**Demografiska egenskaper** - Dessa är valda populationsegenskaper, bland annat:

* Ålder
* Intäkter
* Familjestorlek
* Civilstånd
* Kön
* Plats

**Härledda egenskaper** - Vissa demografiska egenskaper är svåra att fastställa utan registrering, men kan härledas genom en kombination av beteenden och demografiska egenskaper.

Om du t.ex. kombinerar den refererande URL:en (som en beteendeegenskap) med demografiska data (som hämtas från verktyg som [Google Ad Planner](https://www.google.com/adplanner/)) kan webbplatsägarna skapa demografiska egenskaper för sina besökare.

**Delsegment** - Ett segment kan delas upp i flera delsegment. Detta görs genom att definiera ytterligare egenskaper.

**Teaser Page** - en teaser-sida är riktad mot en viss målgrupp. Den innehåller återanvändbart innehåll som kan användas i det underordnade stycket.

**Kampanj** - En kampanj är en samling med interaktiva sidor och e-postmarknadsföringssidor, som nyhetsbrev och inbjudningar. En kampanj körs vanligtvis under en begränsad period och ersätts av en annan kampanj.

**Teaser Paragraph** - Det här är ett stycke som hämtar innehåll från en annan sida beroende på en markeringsstrategi. Denna urvalsstrategi kan ta segment och kampanjer i beaktande.

**Lista** - En lista extraheras från ett segment med registrerade användare. Den plats som t.ex. används för att styra innehållet i det traderande stycket.

>[!NOTE]
>
>Mer information om segment i Adobe Experience Manager finns i [Segmentering](/help/sites-administering/campaign-segmentation.md).
