---
title: Felsökning vid redigering i AEM
description: Vissa problem kan uppstå när du använder AEM.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '294'
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

      * `http://localhost:4502/sites.html/content?`
      * Detta begär sidan direkt från AEM och kringgår Dispatcher. Om du får den uppdaterade sidan är det en indikation på att du bör rensa Dispatcher-cachen.

   * Kontakta systemadministratören om det finns problem med replikeringsköerna.

## Komponentåtgärder visas inte i verktygsfältet {#component-actions-not-visible-on-toolbar}

* **Utgåva**:

   * Hela omfånget av tillämpliga komponentåtgärder visas inte när du redigerar en innehållssida i redigeringsmiljön.

* **Orsak**:

   * I sällsynta fall kan en tidigare åtgärd påverka verktygsfältet.

* **Lösning**:

   * Uppdatera sidan.
