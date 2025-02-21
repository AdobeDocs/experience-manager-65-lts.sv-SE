---
title: Använda Launches för att utveckla innehåll för en framtida release
description: Med Launches kan ni effektivt utveckla innehåll inför en framtida release. De gör att du kan göra ändringar redo för framtida publicering, samtidigt som du behåller de aktuella sidorna.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: site-features
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Launches
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 4%

---

# Launches{#launches}

Med lanseringar kan du effektivt utveckla innehåll för en framtida release.

En startsida skapas så att du kan göra ändringar redo för framtida publicering (samtidigt som du behåller dina aktuella sidor). När du har redigerat och uppdaterat startsidorna befordrar du dem tillbaka till källan och aktiverar sedan källsidorna (översta nivån). Befordra duplicerar startinnehållet tillbaka till källsidorna och kan göras antingen manuellt eller automatiskt (beroende på fält som anges när du skapar och redigerar startsidan).

Till exempel kommer säsongsproduktsidorna i din onlinebutik att uppdateras kvartalsvis så att de aktuella produkterna passar den aktuella säsongen. Om du vill förbereda dig för nästa kvartalsvisa uppdatering kan du skapa en startsida med lämpliga webbsidor. Under hela kvartalet ackumuleras följande ändringar i startversionen:

* Ändringar av källsidorna som inträffar som ett resultat av normala underhållsåtgärder. Dessa ändringar dupliceras automatiskt på startsidorna.
* Redigeringar som utförs direkt på startsidorna inför nästa kvartal.

När nästa kvartal anländer befordrar du startsidorna så att du kan publicera källsidorna (med det uppdaterade innehållet). Du kan antingen befordra alla sidor eller bara de som du har ändrat.

Startar kan också vara:

* Skapat för flera rotgrenar. Du kan skapa en start för hela webbplatsen (och göra ändringarna där), men det kan vara opraktiskt eftersom hela webbplatsen behöver kopieras. När det gäller hundratals eller till och med tusentals sidor påverkas systemkraven och prestandan av både kopieringsåtgärden och senare jämförelserna som krävs för kampanjuppgifterna.
* Kapslad (en programstart inom en programstart) för att ge dig möjlighet att skapa en programstart från en befintlig programstart så att författare kan utnyttja redan gjorda ändringar i stället för att behöva göra samma ändringar flera gånger för varje programstart.

I det här avsnittet beskrivs hur du skapar, redigerar och befordrar (och vid behov [delete](/help/sites-authoring/launches-creating.md#deleting-a-launch)) startsidor från Sites-konsolen eller [startkonsolen](#the-launches-console):

* [Skapa startprogram](/help/sites-authoring/launches-creating.md)
* [Redigeringsövningar](/help/sites-authoring/launches-editing.md)
* [Befordra lanseringar](/help/sites-authoring/launches-promoting.md)

## Startar - ordningen för händelser {#launches-the-order-of-events}

Med den här funktionen kan du effektivt utveckla innehåll för en framtida release av en eller flera aktiverade webbsidor.

Med Launches:

* Skapa en kopia av källsidorna:

   * Kopian är startsida.
   * Källsidorna på den översta nivån kallas **Produktion**.

      * Källsidorna kan tas från flera (separata) grenar.

  ![Översikt över startåtgärder](assets/chlimage_1-111.png)

* Redigera startkonfigurationen:

   * Lägg till eller ta bort sidor och/eller grenar till/från starten.
   * Redigera startegenskaper, som flaggorna **Titel**, **Startdatum** och **Produktionsklar**.

* Du kan befordra och publicera innehållet antingen manuellt eller automatiskt:

   * Manuellt:

      * Befordra startinnehållet tillbaka till **Target** (källsidor) när det är klart för publicering.
      * Publicera innehållet från källsidorna (efter att ha befordrat dem).
      * Befordra antingen alla sidor eller endast ändrade sidor.

   * Automatiskt - det innebär följande:

      * Fältet **Starta**(**Live**) **date**: detta kan anges när du skapar eller redigerar en start.

      * Flaggan **Produktionsklar**: den kan bara anges när du redigerar en start.
      * Om flaggan **Production Ready** är inställd befordras starten automatiskt till produktionssidorna på den angivna **Launch**(**Live**) **date** . Efter kampanjen publiceras produktionssidorna automatiskt.\
        Om inget datum har angetts har flaggan ingen effekt.

* Uppdatera käll- och startsidor parallellt:

   * Ändringar av källsidorna implementeras automatiskt i startkopian (om den har konfigurerats som arv, d.v.s. som en live-kopia).
   * Du kan göra ändringar i startversionen utan att störa dessa automatiska uppdateringar eller källsidorna.

  ![Översikt över uppdateringar](assets/chlimage_1-112.png)

* [Skapa en kapslad programstart](/help/sites-authoring/launches-creating.md#creating-a-nested-launch) - en programstart inom en programstart:

   * Källan är en befintlig start.
   * Du kan [befordra en kapslad start](/help/sites-authoring/launches-promoting.md#promoting-a-nested-launch) till vilket mål som helst. Det kan vara en överordnad start eller källsidorna på den översta nivån (Produktion).

  ![Översikt över kapslad start](assets/chlimage_1-113.png)

  >[!CAUTION]
  >
  >Om du tar bort en programstart tas själva programstarten och alla underordnade kapslade programstarter bort.

>[!NOTE]
>
>Du måste ha behörighet till `/content/launches` för att kunna skapa och redigera starter, precis som med standardgruppen `content-authors`.
>
>Kontakta systemadministratören om du får problem.

>[!CAUTION]
>
>Det går inte att ändra ordning på komponenter på en startsida.
>
>När sidan befordras återspeglas alla innehållsändringar, men komponentens placering ändras inte.

## Startkonsolen {#the-launches-console}

På startkonsolen får du en översikt över dina starter och kan vidta åtgärder för dem som visas. Konsolen kan nås av:

* **Verktyg**-konsolen: **Verktyg**, **Webbplatser**, **Startar**.

* Eller direkt med [https://localhost:4502/libs/launches/content/launches.html](https://localhost:4502/libs/launches/content/launches.html)

## Startar i referenser (platskonsolen) {#launches-in-references-sites-console}

1. Gå till startkällan i konsolen **Platser**.
1. Öppna listen **Referenser** och välj källsidan.
1. Välj **Startar**. Befintliga starter visas:

   ![Fliken Referens - startar](assets/screen-shot_2019-03-05at121901-1.png)

1. Klicka på lämplig start så visas en lista med möjliga åtgärder:

   ![Välj att starta om du vill visa möjliga åtgärder](assets/screen-shot_2019-03-05at121952-1.png)
