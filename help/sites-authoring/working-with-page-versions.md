---
title: Arbeta med innehållssidversioner
description: Skapa, jämföra och återställa versioner av en sida i Adobe Experience Manager.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: ba5cc5fb-934f-4144-8e28-7aa5fdd9b92a
source-git-commit: 438d383aa7a08334d9977411fd37395f89133834
workflow-type: tm+mt
source-wordcount: '1567'
ht-degree: 2%

---

# Arbeta med sidversioner{#working-with-page-versions}

Versionshantering skapar en ögonblicksbild av en sida vid en viss tidpunkt. Med versionshantering kan du utföra följande åtgärder:

* Skapa en sidversion.
* Återställa en sida till en tidigare version, till exempel:
   * om du vill ångra en ändring som du har gjort på sidan.
* Jämför den aktuella versionen av en sida med en tidigare version:
   * om du vill framhäva skillnader i text och bilder.

>[!NOTE]
>
>Endast innehåll versionshanteras i AEM-databasen. Dynamiska resurser som kod, CSS och JavaScript är inte versionshanterade.
>
>* När du visar versioner visas innehållet med den aktuella koden, CSS och JavaScript för databasen.
>* När du återställer versioner återställs endast innehållet och den aktuella koden, CSS och JavaScript för databasen tillämpas på det.

## Skapa en ny version {#creating-a-new-version}

Du kan skapa en version av resursen från:

* [Tidslinjerska](#creating-a-new-version-timeline)
* alternativet [Skapa](#creating-a-new-version-create-with-a-selected-resource) (när en resurs har valts)

### Skapa en ny version - Tidslinje {#creating-a-new-version-timeline}

1. Navigera till sidan som du vill skapa en version för.
1. Markera sidan i [markeringsläge](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Öppna kolumnen **Tidslinje**.
1. Klicka på pilen i kommentarfältet för att visa alternativen:

   ![Tidslinje - Spara som version](assets/screen-shot_2019-03-05at112335.png)

1. Välj **Spara som version**.
1. Ange en **etikett** och **kommentar** om det behövs.

   ![Skapa version - lägg till etikett och kommentar](assets/chlimage_1-42.png)

1. Bekräfta den nya versionen med **Create**.

   Informationen i tidslinjen uppdateras för att ange den nya versionen.

### Skapa en ny version - Skapa med en markerad resurs {#creating-a-new-version-create-with-a-selected-resource}

1. Navigera till sidan som du vill skapa en version för.
1. Markera sidan i [markeringsläge](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Välj alternativet **Skapa** i verktygsfältet för att öppna dialogrutan.
1. I dialogrutan kan du ange en **etikett** och en **kommentar** om det behövs:

   ![Ange en etikett och en kommentar](assets/screen_shot_2012-02-15at105050am.png)

1. Bekräfta den nya versionen med **Create**.

   Tidslinjen öppnas med informationen uppdaterad för att ange den nya versionen.

## Återställer versioner {#reinstating-versions}

När du har skapat en version av sidan finns det olika metoder för att återställa en tidigare version:

* alternativet **Återgå till den här versionen** från [tidslinjen](/help/sites-authoring/basic-handling.md#timeline)

  Återskapa en tidigare version av en markerad sida.

* **Återställ**-alternativen i det övre verktygsfältet för [åtgärder](/help/sites-authoring/basic-handling.md#actions-toolbar)

   * **Återställ version**

     Återskapa versioner av angivna sidor i den markerade mappen. Detta kan även omfatta återställning av tidigare borttagna sidor.

   * **Återställ träd**

     Återskapa en version av ett helt träd vid ett angivet datum och en viss tid. Detta kan inkludera sidor som tidigare har tagits bort.

>[!NOTE]
>
>När du återställer en sida blir den skapade versionen en del av en ny gren.
>
>Så här illustrerar du:
>
>1. Skapa versioner av valfri sida.
>1. De inledande etiketterna och versionsnodnamnen blir 1.0, 1.1, 1.2 o.s.v.
>1. Återställ den första versionen, i det här fallet 1.0.
>1. Skapa versioner igen.
>1. De genererade etiketterna och nodnamnen blir nu 1.0.0, 1.0.1, 1.0.2 och så vidare.

### Återgå till en version {#revert-to-a-version}

Så här **återställer du** den markerade sidan till en tidigare version:

1. Navigera till sidan som du vill återställa till en tidigare version.
1. Markera sidan i [markeringsläge](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Öppna kolumnen **Tidslinje** och välj antingen **Visa alla** eller **Versioner**. Sidversionerna för den valda sidan visas.
1. Välj den version som du vill återställa till. Möjliga alternativ visas:

   ![Återgå till den här versionen](assets/screen-shot_2019-03-05at112505.png)

1. Välj **Återgå till den här versionen**. Den valda versionen återställs och informationen på tidslinjen uppdateras.

### Återställ version {#restore-version}

Den här metoden kan användas för att återställa versioner av angivna sidor i den aktuella mappen. Detta kan även omfatta återställning av sidor som tidigare har tagits bort:

1. Navigera till och [markera](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources) den obligatoriska mappen.

1. Välj **Återställ** och sedan **Återställ version** i det övre verktygsfältet [Åtgärder](/help/sites-authoring/basic-handling.md#actions-toolbar).

   >[!NOTE]
   >
   >Om något av följande:
   >
   >* du har valt en sida som aldrig har några underordnade sidor,
   >* eller ingen av sidorna i mappen har versioner,
   >
   >Sedan är visningen tom eftersom det inte finns några tillämpliga versioner.

1. De tillgängliga versionerna visas:

   ![Återställ version - Lista över alla sidor i mappen](/help/sites-authoring/assets/versions-restore-version-01.png)

1. Använd listruteväljaren under **ÅTERSTÄLL TILL VERSION** för att välja önskad version för en viss sida.

   ![Återställ version - Välj version](/help/sites-authoring/assets/versions-restore-version-02.png)

1. Markera den sida som ska återställas i huvudskärmen:

   ![Återställ version - Välj sida](/help/sites-authoring/assets/versions-restore-version-03.png)

1. Välj **Återställ** för den valda versionen av den valda sidan som ska återställas som den aktuella versionen.

>[!NOTE]
>
>Den ordning i vilken du väljer en obligatorisk sida och den relaterade versionen är utbytbara.

### Återställ träd {#restore-tree}

Den här metoden kan användas för att återställa en version av ett träd vid ett angivet datum och en viss tid. Den kan innehålla sidor som tidigare har tagits bort:

1. Navigera till och [markera](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources) den obligatoriska mappen.

1. Välj **Återställ** och sedan **Återställ träd** i det övre verktygsfältet [Åtgärder](/help/sites-authoring/basic-handling.md#actions-toolbar). Trädets senaste version visas:

   ![Återställ träd](/help/sites-authoring/assets/versions-restore-tree-02.png)

1. Använd datum- och tidsväljaren vid **Senaste versioner vid datum** för att välja en annan version av trädet, den som ska återställas.

1. Ange flaggan **Bevarade icke-versionshanterade sidor** efter behov:

   * Om den är aktiv (markerad) bevaras alla sidor som inte är versionshanterade och påverkas inte av återställningen.

   * Om alternativet är inaktivt (omarkerat) tas alla sidor som inte är versionshanterade bort eftersom de inte fanns i versionsträdet.

1. Välj **Återställ** för den valda versionen av trädet som ska återställas som den *aktuella* versionen.

## Förhandsgranska en version {#previewing-a-version}

Du kan förhandsgranska en viss version:

1. Navigera till sidan som du vill jämföra.
1. Markera sidan i [markeringsläge](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Öppna kolumnen **Tidslinje** och välj antingen **Visa alla** eller **Versioner**.
1. Sidversionerna visas. Välj den version som du vill förhandsgranska:

   ![Välj den version som ska förhandsgranskas](assets/screen-shot_2019-03-05at112505-1.png)

1. Välj **Förhandsgranska**. Sidan visas på en ny flik.

   >[!CAUTION]
   >
   >Om en sida har flyttats kan du inte längre förhandsgranska versioner som gjorts före flyttningen.
   >
   >* Om du får problem med en förhandsgranskning kan du kontrollera [tidslinjen](/help/sites-authoring/basic-handling.md#timeline) för sidan för att se om sidan har flyttats.

## Jämföra en version med den aktuella sidan {#comparing-a-version-with-current-page}

Så här jämför du en tidigare version med den aktuella sidan:

1. Navigera till sidan som du vill jämföra.
1. Markera sidan i [markeringsläge](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources).
1. Öppna kolumnen **Tidslinje** och välj antingen **Visa alla** eller **Versioner**.
1. Sidversionerna visas. Välj den version som du vill jämföra:

   ![Sidversioner visas - välj version](assets/screen-shot_2019-03-05at112505-2.png)

1. Välj **Jämför med aktuell**. [Sidskillnaden ](/help/sites-authoring/page-diff.md) öppnas och skillnaderna visas.

## Timewarp {#timewarp}

Timewarp är en funktion som har utformats för att simulera en sidas *publicerade*-läge vid en viss tidpunkt.

>[!TIP]
>
>[Timewarp kan också användas med Launches för att förhandsgranska framtiden](/help/sites-authoring/launches.md) när du kör AEM 6.5.10.0 eller senare.

Att skapa innehåll är en pågående och samarbetsorienterad process. Syftet med Timewarp är att författarna ska kunna spåra den publicerade webbplatsen över tid för att hjälpa dem förstå hur innehållet har ändrats. Den här funktionen använder sidversionerna för att avgöra status för publiceringsmiljön:

* Systemet söker efter den sidversion som var aktiv vid den valda tidpunkten.
   * Den här sidversionen skapades/aktiverades *före* den tidpunkt som valdes i Timewarp.
* När du navigerar till en sida som har tagits bort återges den också, så länge som de gamla versionerna av sidan fortfarande är tillgängliga i databasen.
* Om ingen publicerad version hittas återgår Timewarp till sidans aktuella läge i författarmiljön (för att förhindra ett fel/404-sida, vilket skulle förhindra bläddring).

### Använda Timewarp {#using-timewarp}

Timewarp är ett [läge](/help/sites-authoring/author-environment-tools.md#page-modes) i sidredigeraren. Du startar det genom att helt enkelt växla det på samma sätt som andra lägen.

1. Starta redigeraren för sidan där du vill starta Timewarp och välj sedan **Timewarp** i lägesvalet.

   ![Välj Timewarp i lägesmarkeringen](assets/wwpv-01.png)

1. Ange ett måldatum och en måltid i dialogrutan och klicka på **Ange datum**. Om du inte väljer någon tid används den aktuella tiden som standard.

   ![Ange datum](assets/wwpv-02.png)

1. Sidan visas baserat på det angivna datumet. Timewarp-läget indikeras via det blå statusfältet högst upp i fönstret. Använd länkarna i statusfältet för att välja ett nytt måldatum eller avsluta Timewarp-läget.

   ![Indikator för Timewarp](assets/wwpv-03.png)

### Begränsningar för Timewarp {#timewarp-limitations}

Med Timewarp kan du göra ett bra försök att återskapa en sida vid en viss tidpunkt. På grund av komplexiteten i den kontinuerliga redigeringen av innehåll i AEM är detta dock inte alltid möjligt. Dessa begränsningar bör beaktas när du använder Timewarp.

* **Timewarp fungerar baserat på publicerade sidor** - Timewarp fungerar bara helt om du tidigare har publicerat sidan. I annat fall visas den aktuella sidan i författarmiljön.
* **Timewarp använder sidversioner** - Om du navigerar till en sida som har tagits bort/tagits bort från databasen återges den korrekt om gamla versioner av sidan fortfarande är tillgängliga i databasen.
* **Borttagna versioner påverkar Timewarp** - Om versioner tas bort från databasen kan inte Timewarp visa rätt vy.

* **Timewarp är skrivskyddat** - du kan inte redigera den gamla versionen av sidan. Det är bara tillgängligt för visning. Om du vill återställa den äldre versionen måste du göra det manuellt med [restore](#reverting-to-a-page-version).

* **Timewarp baseras bara på sidinnehåll** - Om element för återgivning av webbplatsen har ändrats skiljer sig vyn från den ursprungliga, eftersom objekten inte versionsindelas i databasen. Sådana element är bland annat kod, css, resurser och bilder.

>[!CAUTION]
>
>Timewarp är ett verktyg som hjälper författare att förstå och skapa sitt innehåll. Den är inte avsedd som en revisionslogg eller för juridiska ändamål.
