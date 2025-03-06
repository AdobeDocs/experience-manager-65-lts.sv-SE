---
title: Avgränsningskomponent i adaptiva formulär
description: Du kan använda avgränsningskomponenten för att segmentera ett formulär visuellt.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 8b1a9626-6de1-4b19-bb93-ada667f24e83
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Avgränsningskomponent i adaptiva formulär{#separator-component-in-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs ett äldre sätt att skapa adaptiva Forms med baskomponenter. </span>

Du kan använda avgränsarkomponenten för att dela upp paneler i ett formulär visuellt. Du kan definiera det övergripande utseendet och formatet för en avgränsningskomponent genom att ange följande egenskaper för avgränsningskomponenten:

* **Elementnamn:** Anger komponentens namn. SOM-uttrycken adresserar komponenten med ett värde angivet i fältet Elementnamn.
* **Tjocklek:** Anger avgränsarkomponentens tjocklek i pixlar.

* **CSS-klass:** Anger den anpassade CSS-klassen för avgränsarkomponenten

* **Infogade format:** Med AEM Forms kan du nu använda infogade CSS-format på enskilda adaptiva formulärkomponenter och förhandsgranska ändringarna i realtid.

Du kan använda layoutläget för att definiera antalet kolumner som avgränsningskomponenten sträcker sig till. Mer information finns i [Använda layoutläget för att ändra storlek på komponenter](../../forms/using/resize-using-layout-mode.md).

Så här anger du egenskaper för en avgränsningskomponent:

1. Välj en avgränsningskomponent och välj ![cmpr](assets/cmppr.png). Egenskaperna öppnas i sidofältet.
1. Klicka på en flik i avsnittet Infoga CSS-egenskaper så att du kan ange CSS-egenskaper. Till exempel: a. Klicka på **Lägg till objekt** på fliken Fält. En rad med två fält läggs till.
1. I det första fältet från vänster anger du den CSS3-egenskap som du vill använda. Till exempel **border**. Du kan också välja en egenskap genom att klicka på nedpilsknappen. Listrutan är inte fullständig och du kan ange ett CSS3-egenskapsnamn som stöds i det här fältet.
1. Ange ett giltigt värde för den angivna CSS3-egenskapen i det intilliggande fältet. Exempel: **3-px solid black**.
1. Klicka på **Lägg till objekt** om du vill ange en annan egenskap och dess värde.
1. Klicka på **Förhandsgranska** så att du kan förhandsgranska ändringarna i formuläret.
1. Klicka på **OK** om du vill bekräfta ändringarna eller på **Avbryt** om du vill stänga dialogrutan utan några ändringar.
