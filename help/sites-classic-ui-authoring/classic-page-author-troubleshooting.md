---
title: Felsöka AEM vid redigering
description: I följande avsnitt beskrivs några problem som du kan stöta på när du använder AEM, tillsammans med förslag på hur du felsöker dem.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: be4397d1-0680-4b44-bdd2-825b521a44d6
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 0%

---

# Felsöka AEM vid redigering{#troubleshooting-aem-when-authoring}

I följande avsnitt beskrivs några problem som du kan stöta på när du använder AEM, tillsammans med förslag på hur du felsöker dem.

>[!NOTE]
>
>När du får problem är det också värt att kontrollera listan över [kända fel](/help/release-notes/release-notes.md) för din instans (release- och servicepaket).

>[!NOTE]
>
>Användare som har administratörsbehörighet och som vill felsöka problem med AEM kan använda de felsökningsmetoder som beskrivs i [Felsökning av AEM (för administratörer)](/help/sites-administering/troubleshoot.md). Om du inte har tillräcklig behörighet kontaktar du systemadministratören för felsökning av AEM.

## Gammal sidversion på publicerad webbplats {#old-page-version-still-on-published-site}

* **Utgåva**:

   * Du har gjort ändringar på en sida och replikerat sidan till publiceringsplatsen, men den *gamla* versionen av sidan visas fortfarande på publiceringsplatsen.

* **Orsak**:

   * Detta kan ha flera orsaker, oftast cacheminnet (antingen din lokala webbläsare eller Dispatcher), men ibland kan det vara ett problem med replikeringskön.

* **Lösningar**:

   * Här finns olika möjligheter:
   * Bekräfta att sidan har replikerats korrekt. Kontrollera sidstatus och, om det behövs, status för replikeringskön.
   * Rensa cacheminnet i den lokala webbläsaren och få åtkomst till sidan igen.
   * Lägg till `?` i slutet av sidans URL. Till exempel:

     `http://localhost:4502/sites.html/content?`

     Detta begär sidan direkt från AEM och kringgår Dispatcher. Om du får den uppdaterade sidan är det en indikation på att du bör rensa Dispatcher-cachen.

   * Kontakta systemadministratören om det finns problem med replikeringsköerna.

## Sidekick är inte synligt {#sidekick-not-visible}

* **Utgåva**:

   * Sidekick syns inte när du redigerar en innehållssida i redigeringsmiljön.

* **Orsak**:

   * I sällsynta fall kan du ha placerat sidhuvudet utanför det aktuella fönstret. Det innebär att du inte kan flytta den igen.

* **Lösning**:

   * Logga ut från den aktuella sessionen och logga in igen. Sidekick återgår till standardpositionen.

## Sök och ersätt - alla förekomster ersätts inte {#find-replace-not-all-instances-are-replaced}

* **Utgåva:**

   * När du använder alternativet **Sök och ersätt** kan det hända att inte alla förekomster av termen `find` ersätts på en sida.

* **Orsak**:

   * Funktionen för **Sök och ersätt** beror på hur innehållet sparas och om det går att söka efter det. En bloggtext sparas till exempel i egenskapen `jcr:text` som inte är konfigurerad för att genomsökas. Standardomfånget för sök- och ersättningsservern omfattar följande egenskaper:

      * `jcr:title`
      * `jcr:description`
      * `jcr:text`
      * `text`

* **Lösning**:

   * Dessa definitioner kan ändras med konfigurationen för **Dag CQ WCM Sök ersätt-server** med **webbkonsolen**, till exempel på

     `http://localhost:4502/system/console/configMgr`
