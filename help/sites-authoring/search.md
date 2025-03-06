---
title: Omfattande sökfunktioner
description: Hitta materialet snabbare med omfattande sökfunktioner.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 4c6d1d6a-c000-48cf-9d86-306245a3c10c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 5%

---

# Sök{#searching}

I AEM redigeringsmiljö finns olika metoder för att söka efter innehåll, beroende på resurstypen.

>[!NOTE]
>
>Utanför författarmiljön finns det även andra funktioner som kan användas för sökning, till exempel [Query Builder](/help/sites-developing/querybuilder-api.md) och [CRXDE Lite](/help/sites-developing/developing-with-crxde-lite.md).

## Grunderna i sökning {#search-basics}

Sökfunktionen finns i det övre verktygsfältet:

![Sökning](do-not-localize/chlimage_1-17.png)

Med sökfältet kan du:

* Sök efter ett specifikt nyckelord, en viss sökväg eller en viss tagg.
* Filtrera enligt resursspecifika kriterier som ändrade datum, sidstatus, filstorlek osv.
* Definiera och använd en [sparad sökning](#saved-searches) baserat på ovanstående villkor.

>[!NOTE]
>
>Sökningen kan också anropas med snabbtangenten `/` (snedstreck) när sökfältet är synligt.

## Sök och filtrera {#search-and-filter}

Så här söker och filtrerar du resurser:

1. Öppna **Sök** (med förstoringsglaset i verktygsfältet) och ange söktermen. Förslag kommer att göras och kan väljas:

   ![s-01](assets/s-01.png)

   Som standard begränsas sökresultaten till din aktuella plats (d.v.s. konsol och relaterad resurstyp):

   ![screen_shot_2018-03-23at101445](assets/screen_shot_2018-03-23at101445.png)

1. Om det behövs kan du ta bort platsfiltret (markera **X** på filtret som du vill ta bort) för att söka i alla konsoler/resurstyper.
1. Resultaten visas, grupperade efter konsol och relaterad resurstyp.

   Du kan antingen välja en specifik resurs (för ytterligare åtgärd) eller fördjupa dig genom att välja den resurstyp som krävs, till exempel **Visa alla platser**:

   ![screen-shot_2019-03-05at101900](assets/screen-shot_2019-03-05at101900.png)

1. Om du vill gå vidare väljer du skensymbolen (längst upp till vänster) för att öppna sidopanelen **Filter och alternativ**.

   ![Filter och alternativ](do-not-localize/screen_shot_2018-03-23at101542.png)

   I enlighet med resurstypen Sök visas ett fördefinierat urval av söknings-/filtervillkor.

   På sidopanelen kan du välja:

   * Sparade sökningar
   * Sökkatalog
   * Taggar
   * Sökvillkor, till exempel Ändrade datum, Publiceringsstatus, LiveCopy-status.

   >[!NOTE]
   >
   >Sökvillkoren kan variera:
   >
   >
   >
   >    * Beroende på vilken resurstyp du har valt, t.ex. är Assets- och Communities-kriterierna förståeligt specialiserade.
   >    * Instansen som [Sök i Forms](/help/sites-administering/search-forms.md) kan anpassas (lämplig för platsen i AEM).
   >
   >

   ![screen-shot_2019-03-05at102509](assets/screen-shot_2019-03-05at102509.png)

1. Du kan även lägga till ytterligare söktermer:

   ![screen-shot_2019-03-05at102613](assets/screen-shot_2019-03-05at102613.png)

1. Stäng **sökningen** med **X** (överst till höger).

>[!NOTE]
>
>Sökvillkoren sparas när du väljer ett objekt i sökresultatet.
>
>När du markerar ett objekt på sökresultatsidan och återgår till söksidan efter att du har använt knappen för att återställa webbläsaren, kvarstår sökvillkoren.

## Sparade sökningar {#saved-searches}

Förutom att söka efter en mängd olika aspekter kan du även spara en viss sökkonfiguration för hämtning och användning i ett senare skede:

1. Definiera sökvillkoren och välj **Spara**.

   ![screen-shot_2019-03-05at102613-1](assets/screen-shot_2019-03-05at102613-1.png)

1. Tilldela ett namn och använd sedan **Spara** för att bekräfta:

   ![screen-shot_2019-03-05at102725](assets/screen-shot_2019-03-05at102725.png)

1. Din sparade sökning är tillgänglig från väljaren nästa gång du öppnar sökpanelen:

   ![screen-shot_2019-03-05at102927](assets/screen-shot_2019-03-05at102927.png)

1. När du har sparat kan du:

   * Använd **x** (mot namnet på den sparade sökningen) för att starta en ny fråga (den sparade sökningen tas inte bort).
   * **Redigera sparad sökning**, ändra sökvillkoren och **Spara** igen.

Du kan ändra sparade sökningar genom att markera den sparade sökningen och klicka på **Redigera sparad sökning** längst ned på sökpanelen.

![screen-shot_2019-03-05at103010](assets/screen-shot_2019-03-05at103010.png)
