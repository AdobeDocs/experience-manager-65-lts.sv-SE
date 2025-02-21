---
title: Ändra standardformat för HTML5-formulär
description: HTML5-formulärformat bygger på CSS. Du kan ändra formulärets standardformat.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Ändra standardformat för HTML5-formulär{#changing-default-styles-of-html-forms}

HTML5-formulär återges med HTML5-funktioner och formateringen av det återgivna formuläret görs med CSS. Standardutseendet för ett HTML5-formulär påminner om dess PDF-återgivning. Utvecklare kan använda anpassad CSS för att ändra standardutseendet på HTML5-formulär.

I den här artikeln finns stegvis information om hur du ändrar format i ett HTML5-formulär. Artikeln [Introduktion till format](/help/forms/using/css-styles.md) innehåller detaljerad information om olika formateringsaspekter i HTML5-formulär. Läs Introduktion till formatartiklar innan du utför de steg som nämns i den här artikeln.

I följande två bilder visas skillnaden mellan standardformat och anpassade format.

![images-002-small](assets/pictures-002-small.png)

## Formatera formulären {#style-your-forms}

1. **Välj en profil för att lägga till egna format**

   Gå till CRX DE-gränssnittet på URL:en: **https://&lt;server>:&lt;port>/crx/de** och skapa en profil eller välj en befintlig profil. Mer information om hur du skapar en profil finns i [Skapa en profil](/help/forms/using/custom-profile.md)

1. **Skapa en CSS-formatmall för formatering av HTML5-formulär**

   Navigera till mappen där du har skapat profilåtergivaren och skapa en CSS-formatmallsfil. De steg du ska följa är

   1. Högerklicka på mappen och välj **skapa** > **skapa fil** på menyn

   1. Ange formatmallens namn i dialogrutan Skapa fil. Kontrollera att du använder filnamnstillägget .css (till exempel stylesheet.css)
   1. Öppna CSS-filen som du har skapat i navigeringsrutan.
   1. Definiera CSS-klasserna för de komponenter som du vill formatera och lägg till format i dessa klasser.

   Om du vill veta vilka CSS-klasser som ska skapas för en viss komponent i dina HTML5-formulär kan du läsa [Introduktion till format](/help/forms/using/css-styles.md).

1. **Inkludera formatmallen i profilåtergivning**

   Öppna sidan Profilåtergivning (jsp-fil) i CRX DE och ta med CSS-filen på sidan precis nedanför XFA-klientbiblioteket. Följ de här stegen för att inkludera CSS-filen i profilen.

   1. Sök på återgivningssidan efter följande rad:

      &lt;cq:includeClientLib categories=&quot;xfaforms.profile&quot; />

   1. Infoga följande nedanför raden ovan för att inkludera formatmallen:

      &lt;link href=&quot;/path/to/stylesheet&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot;/>

   1. Spara filen.
