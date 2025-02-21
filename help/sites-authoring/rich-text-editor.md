---
title: Använda RTF-redigeraren för att skapa innehåll
description: Använda RTF-redigeraren för att skapa innehåll i Adobe Experience Manager 6.5.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Använda RTF-redigeraren för att skapa innehåll {#use-rich-text-editor-to-author-content}

RTE (Rich Text Editor) är en grundläggande byggsten för att infoga textinnehåll i AEM. Den utgör grunden för olika komponenter, bland annat följande:

* [Text](https://experienceleague.adobe.com/en/docs/experience-manager-core-components/using/wcm-components/text)
* [Tabell](https://experienceleague.adobe.com/en/docs/experience-manager-core-components/using/wcm-components/text#table)

## Direktredigering {#in-place-editing}

Om du markerar en textbaserad komponent med ett enda klick visas verktygsfältet [för komponenten](/help/sites-authoring/editing-content.md#edit-configure-copy-cut-delete-paste) som för alla komponenter.

![screen_shot_2018-03-21at163054](assets/screen_shot_2018-03-21at163054.png)

Om du trycker/klickar igen eller först markerar komponenten med ett långsamt dubbelklick öppnas redigering på plats, som har ett eget verktygsfält. Här kan du redigera innehållet och göra grundläggande formateringsändringar.

![screen_shot_2018-03-21at163214](assets/screen_shot_2018-03-21at163214.png)

Det här verktygsfältet innehåller följande alternativ:

* **Format**: Det gör att du kan ange Fet, Kursiv och Understruken.
* **Listor**: Med detta kan du skapa punktlistor eller numrerade listor eller ange indrag.
* **Hyperlänk**
* **Bryt länk**
* **Helskärm**
* **Stäng**
* **Spara**

## Helskärmsredigering {#full-screen-editing}

Om du trycker på helskärmsläget i verktygsfältet ![Fullskärmsläge](do-not-localize/screen_shot_2018-03-21at163236.png) för textbaserade komponenter öppnas RTF-redigeraren och resten av sidinnehållet döljs.

I helskärmsläge visas alla konfigurerade alternativ som du kan använda för att skapa. Tillgängligheten är alternativ [beroende på konfigurationen](/help/sites-administering/rich-text-editor.md).

![screen_shot_2018-03-21at163248](assets/screen_shot_2018-03-21at163248.png)

Fler alternativ för textredigering:

* **Ankarpunkt**: Skapa en ankarpunkt i texten som du senare kan länka till/referera till.
* **Vänsterjustera text**
* **Centrera text**
* **Högerjustera text**

Stäng helskärmsläget genom att klicka på minimeringsikonen.

![screen_shot_2018-03-21at163323](assets/screen_shot_2018-03-21at163323.png)

>[!NOTE]
>
>Om du kopierar kapslade listor från Microsoft Word till textredigeraren kan det ge inkonsekventa resultat och du kan behöva justera texten manuellt efter att du har klistrat in texten i textredigeraren.
