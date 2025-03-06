---
title: Inkorgen för att hantera uppgifter
description: Hantera dina uppgifter med inkorgen i Adobe Experience Manager 6.5.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 077407ef-1d7f-47ad-b924-0afa19f21119
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 6%

---

# Din inkorg{#your-inbox}

Du kan få meddelanden från olika områden i AEM, inklusive arbetsflöden och projekt, t.ex. om:

* Uppgifter:

   * dessa kan också skapas vid olika tillfällen i AEM-gränssnittet, till exempel under **Projekt**,
   * dessa kan vara produkten av ett **Skapa uppgift**- eller **Skapa projektuppgift**-steg i ett arbetsflöde.

* Arbetsflöden:

   * Arbetsobjekt som representerar åtgärder som du måste utföra på sidinnehåll.

      * detta är produkten av arbetsflödet **Deltagare** steg

   * felobjekt, så att administratörer kan försöka utföra det misslyckade steget igen.

Du får dessa meddelanden i din egen Inkorg där du kan visa dem och vidta åtgärder.

>[!NOTE]
>
>Körklar AEM levereras med administrativa uppgifter som tilldelats administratörsanvändargruppen. Mer information finns i [Administrativa åtgärder som inte är tillgängliga](#out-of-the-box-administrative-tasks).

>[!NOTE]
>
>Mer information om objekttyperna finns även i:
>
>* [Projekt](/help/sites-authoring/touch-ui-managing-projects.md)
>* [Projekt - arbeta med uppgifter](/help/sites-authoring/task-content.md)
>* [Arbetsflöden](/help/sites-authoring/workflows.md)
>* [Forms](/help/forms/using/introduction-aem-forms.md)
>

## Inkorgen i sidhuvudet {#inbox-in-the-header}

Från någon av konsolerna visas det aktuella antalet objekt i din inkorg i sidhuvudet. Indikatorn kan också öppnas för att ge snabb åtkomst till sidor som kräver åtgärder eller åtkomst till inkorgen:

![wf-80](assets/wf-80.png)

>[!NOTE]
>
>Vissa åtgärder visas även i [kortvyn för den aktuella resursen](/help/sites-authoring/basic-handling.md#card-view).

## Administrativa arbetsmoment som inte går att köra  {#out-of-the-box-administrative-tasks}

Enkel AEM levereras med fyra uppgifter som tilldelats administratörsanvändargruppen.

* [Konfigurera analys och målanpassning](/help/sites-administering/opt-in.md)
* [Använd checklistan för AEM-säkerhet](/help/sites-administering/security-checklist.md)
* Aktivera insamling av aggregerad användningsstatistik
* [Konfigurera HTTPS](/help/sites-administering/ssl-by-default.md)

## Öppna Inkorgen {#opening-the-inbox}

Så här öppnar du AEM inkorg för meddelanden:

1. Klicka på indikatorn i verktygsfältet.

1. Välj **Visa alla**. **AEM Inbox** öppnas. I inkorgen visas objekt från arbetsflöden, projekt och uppgifter.
1. Standardvyn är [Listvy](#inbox-list-view), men du kan även växla till [Kalendervy](#inbox-calendar-view). Detta görs med vyväljaren (verktygsfält, överst till höger).

   För båda vyerna kan du även definiera [visningsinställningar](#inbox-view-settings). Vilka alternativ som är tillgängliga beror på den aktuella vyn.

   ![wf-79](assets/inbox-list-view.png)

>[!NOTE]
>
>Inkorgen fungerar som en konsol, och du kan använda [Global navigering](/help/sites-authoring/basic-handling.md#global-navigation) eller [Sök](/help/sites-authoring/search.md) för att navigera till en annan plats när du är klar.

### Inkorg - listvy {#inbox-list-view}

I den här vyn visas alla objekt tillsammans med viktig relevant information:

![wf-82](assets/wf-82.png)

### Inkorg - kalendervy {#inbox-calendar-view}

I den här vyn visas objekt efter deras placering i kalendern och den exakta vyn som du har valt:

![wf-93](assets/wf-93.png)

Du kan:

* välj en specifik vy: **Tidslinje**, **Kolumn**, **Lista**

* Ange vilka uppgifter som ska visas enligt **Schema**; **Alla**, **Planerade**, **Pågår**, **Förfaller snart**, **Förfallodatum**

* detaljgranska för mer detaljerad information om ett objekt
* markera ett datumintervall som vyn ska fokuseras i:

![wf-91](assets/wf-91.png)

### Inkorg - Inställningar {#inbox-view-settings}

För båda vyerna (List och Calendar) kan du definiera inställningar:

* **Kalendervy**

  För **kalendervyn** kan du konfigurera:

   * **Gruppera efter**
   * **Schema** eller **Ingen**
   * **Kortstorlek**

  ![wf-92](assets/wf-92.png)

* **Listvy**

  För **listvyn** kan du konfigurera sorteringsmekanismen:

   * **Sorteringsfält**
   * **Sorteringsordning**

  ![wf-83](assets/inbox-settings.png)

### Inkorg - Administratörskontroll {#inbox-admin-control}

Med alternativet Admin Control kan administratörer:

* Anpassa AEM Inbox-kolumnerna

* Anpassa rubriktext och logotyp

* Styra visningen av navigeringslänkar i sidhuvudet

Alternativet Admin Control är bara synligt för medlemmarna i gruppen `administrators` eller `workflow-administrators`.

* **Kolumnanpassning**: Anpassa en AEM Inbox för att ändra standardrubriken för en kolumn, ändra ordning på en kolumns position och visa ytterligare kolumner baserat på data i ett arbetsflöde.
   * **Lägg till kolumn**: Markera en kolumn som ska läggas till i AEM Inbox.
   * **Redigera kolumn**: Håll musen över kolumnrubriken och välj ikonen ![redigera](assets/edit.svg) för att ange ett kolumnvisningsnamn.
   * **Ta bort kolumn**: Markera ikonen ![ta bort](assets/delete_updated.svg) om du vill ta bort kolumnen från AEM Inbox.
   * **Flytta kolumn**: Dra ikonen ![move](assets/move_updated.svg) för att flytta en kolumn till en ny plats i AEM Inbox.

  ![admin-control](assets/admin-control-column-customize.png)

* **Anpassning av profilering**

   * **Anpassa rubriktext:** Ange den text som ska visas i sidhuvudet för att ersätta standardtexten för **Adobe Experience Manager**.

   * **Anpassa logotyp:** Ange bilden som ska visas i sidhuvudet som logotyp. Överför en bild i DAM (Digital Asset Management) och hänvisa till den bilden i fältet.

* **Användarnavigering**
   * **Dölj navigeringsalternativ:** Välj det här alternativet om du vill dölja navigeringsalternativ som är tillgängliga i sidhuvudet. Navigeringsalternativen inkluderar länkar till andra lösningar, hjälplänkar och de redigeringsalternativ som finns när man trycker på Adobe Experience Manager logotyp eller text.
* **Spara:** Klicka på det här alternativet om du vill spara inställningarna.

## Vidta åtgärder för ett objekt {#taking-action-on-an-item}

>[!NOTE]
>
>Även om det går att markera mer än ett objekt, kan åtgärder bara vidtas för ett objekt i taget.


1. Om du vill utföra en åtgärd för ett objekt markerar du miniatyrbilden för det aktuella objektet. Ikoner för de åtgärder som gäller för det objektet visas i verktygsfältet:

   ![wf-84](assets/wf-84.png)

   Åtgärderna är lämpliga för objektet och omfattar:

   * **Slutför**-åtgärd, till exempel en uppgift eller ett arbetsflödesobjekt.
   * **Tilldela om**/**Delegera** ett objekt.
   * **Öppna** ett objekt. Beroende på objekttypen kan den här åtgärden:

      * visa objektegenskaperna
      * öppna en lämplig kontrollpanel eller guide för ytterligare åtgärder
      * öppna relaterad dokumentation

   * **Gå tillbaka** till ett föregående steg.
   * Visa nyttolasten för ett arbetsflöde.
   * Skapa ett projekt från objektet.

   >[!NOTE]
   >
   >Mer information finns i:
   >
   >* Arbetsflödesobjekt - [Deltar i arbetsflöden](/help/sites-authoring/workflows-participating.md)

1. Beroende på vilket objekt som är markerat startas en åtgärd, till exempel:

   * en dialogruta som är lämplig för åtgärden öppnas.
   * en åtgärdsguide startas.
   * en dokumentationssida öppnas.

   Till exempel öppnar **Tilldela om** en dialogruta:

   ![wf-85](assets/wf-85.png)

   Beroende på om en dialogruta, guide, dokumentationssida har öppnats kan du:

   * Bekräfta lämplig åtgärd, till exempel Tilldela igen.
   * Avbryt åtgärden.
   * Bakåtpilen. Om en åtgärdsguide eller dokumentationssida har öppnats kan du gå tillbaka till Inkorgen.

## Skapa en uppgift {#creating-a-task}

I inkorgen kan du skapa uppgifter:

1. Välj **Skapa** och sedan **Aktivitet**.
1. Fyll i de nödvändiga fälten på flikarna **Grundläggande** och **Avancerat**. Endast **Titel** är obligatoriskt, alla andra är valfria:

   * **Grundläggande**:

      * **Titel**
      * **Projekt**
      * **Tilldelad**
      * **Innehåll**; liknar nyttolast är detta en referens från aktiviteten till en plats i databasen
      * **Beskrivning**
      * **Aktivitetsprioritet**
      * **Startdatum**
      * **Förfallodatum**

   ![wf-86](assets/wf-86.png)

   * **Avancerat**

      * **Namn**: Detta används för att skapa URL:en. Om det är tomt baseras det på **Rubrik**.

   ![wf-87](assets/wf-87.png)

1. Välj **Skicka**.

## Skapa ett projekt {#creating-a-project}

För vissa uppgifter kan du skapa ett [projekt](/help/sites-authoring/projects.md) baserat på den uppgiften:

1. Välj lämplig åtgärd genom att trycka/klicka på miniatyrbilden.

   >[!NOTE]
   >
   >Endast aktiviteter som skapats med alternativet **Skapa** i **Inkorgen** kan användas för att skapa ett projekt.
   >
   >Arbetsobjekt (från ett arbetsflöde) kan inte användas för att skapa ett projekt.

1. Välj **Skapa projekt** i verktygsfältet för att öppna guiden.
1. Välj lämplig mall och sedan **Nästa**.
1. Ange de nödvändiga egenskaperna:

   * **Grundläggande**

      * **Titel**
      * **Beskrivning**
      * **Startdatum**
      * **Förfallodatum**
      * **Användare** och roll

   * **Avancerat**

      * **Namn**

   >[!NOTE]
   >
   >Mer information finns i [Skapa ett projekt](/help/sites-authoring/touch-ui-managing-projects.md#creating-a-project).

1. Välj **Skapa** för att bekräfta åtgärden.

## Filtrera objekt i AEM Inkorg {#filtering-items-in-the-aem-inbox}

Du kan filtrera objekten i listan:

1. Öppna **AEM Inbox**.

1. Öppna filterväljaren:

   ![wf-88](assets/wf-88.png)

1. Du kan filtrera de listade objekten enligt ett intervall av villkor, som många kan förfinas. Exempel:

   ![wf-89](assets/wf-89.png)

   >[!NOTE]
   >
   >Med [Visa inställningar](#inbox-view-settings) kan du även konfigurera sorteringsordningen när du använder [listvyn](#inbox-list-view).
