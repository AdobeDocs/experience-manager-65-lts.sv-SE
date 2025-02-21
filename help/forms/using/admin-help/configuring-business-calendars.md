---
title: Konfigurera affärskalendrar
description: Affärskalendrar definierar affärsdagar och icke-affärsdagar för din organisation. Lär dig hur du konfigurerar affärskalendrar.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '1901'
ht-degree: 0%

---

# Konfigurera affärskalendrar {#configuring-business-calendars}

*Affärskalendrar* definierar affärsdagar och icke-affärsdagar (t.ex. lagstadgade helger, helger och företagsstängningsdagar) för din organisation. När du använder affärskalendrar hoppar AEM-formulär över icke-arbetsdagar vid vissa datumberäkningar. I Workbench kan du ange om du vill använda affärskalendrar för användarrelaterade händelser som påminnelser om uppgifter, deadlines och eskaleringar eller för åtgärder som inte är kopplade till användare, som Timer Events och Wait Service.

En påminnelse om en aktivitet är till exempel konfigurerad att inträffa tre arbetsdagar efter att uppgiften har tilldelats en användare. Uppgiften tilldelas på torsdag. De följande tre dagarna är dock inte arbetsdagar eftersom fredagen är en nationell helgdag och de följande två dagarna är helgdagar. Påminnelsen skickas därför på onsdagen nästa vecka.

>[!NOTE]
>
>När du beräknar datum och tid med hjälp av affärskalendrar använder AEM-formulär datum och tid för den server där den körs och justerar inte skillnaden mellan tidszoner. Om en påminnelse om en uppgift till exempel är schemalagd till klockan 10.00 på en server som körs i London, men användaren som tar emot påminnelsen är i New York, får användaren påminnelsen kl. 17.00 lokal tid.

## Använda standardaffärskalendern {#using-the-default-business-calendar}

AEM-formulär innehåller en standardaffärskalender (som kallas *Inbyggd kalender*) som anger lördagar och söndagar som lediga dagar. Om alla användare i organisationen har samma icke-arbetsdagar kan du uppdatera standardarbetskalendern så att den passar din organisation. Om du bara använder standardarbetskalendern behöver du inte aktivera affärskalendrar i Användarhantering eller tillhandahålla några mappningar. När inga andra affärskalendrar är definierade, använder AEM-formulär standardaffärskalendern.

## Konfigurera flera affärskalendrar {#setting-up-multiple-business-calendars}

Om några användare i organisationen har olika icke-arbetsdagar kan du definiera flera affärskalendrar och konfigurera mappningar som tillåter en körtidsupplösning av en affärskalender för en användare.

### Definiera flera affärskalendrar {#define-multiple-business-calendars}

1. Bestäm hur du ska associera rätt affärskalender med en användare. Det finns två sätt att associera en affärskalender med en användare:

   **Gruppmedlemskap:** Du kan tilldela en affärskalender till en användare baserat på användarens gruppmedlemskap. I det här fallet använder varje användare i gruppen samma affärskalender.

   Om en användare är medlem i två olika grupper, och dessa grupper är mappade till två olika affärskalendrar, använder AEM-formulär den första kalendern som hittas i sökresultaten. I det här fallet bör du överväga att använda affärskalendernycklar för att associera användare med affärskalendrar.

   **Affärskalenummernycklar:** Du kan tilldela en användare en affärskalender baserat på en affärskalendernyckel, vilket är en inställning som anges i Användarhantering. Du kan sedan mappa affärskalendernyckeln till en affärskalender i formulärarbetsflödet.

   Hur du tilldelar användare affärskalendernycklar beror på om du använder en företagsdomän, lokal domän eller hybriddomän. Mer information om hur du konfigurerar domäner finns i [Lägga till domäner](/help/forms/using/admin-help/adding-domains.md#adding-domains).

   Om du använder en lokal domän eller hybriddomän lagras information om användare endast i databasen för användarhantering. Om du vill ställa in affärskalendernyckeln för de här användarna anger du en sträng i fältet Affärskalendernyckel när du lägger till eller redigerar en användare i Användarhantering. (Se [Lägga till och konfigurera användare](/help/forms/using/admin-help/adding-configuring-users.md#adding-and-configuring-users).) Du kan sedan mappa affärskalendernycklarna (strängarna) till affärskalendrar i formulärarbetsflödet. (Se [Mappa användare och grupper till en affärskalender](configuring-business-calendars.md#mapping-users-and-groups-to-a-business-calendar).)

   Om du använder en företagsdomän finns information om användare i ett tredjepartslagringssystem, till exempel en LDAP-katalog, som användarhanteringen synkroniserar med användarhanteringsdatabasen. På så sätt kan du mappa en affärskalendernyckel till ett fält i LDAP-katalogen. Om till exempel varje användarpost i din katalog innehåller fältet&quot;land&quot;, och du vill tilldela affärskalendrar baserat på det land där användaren finns, anger du fältnamnet&quot;land&quot; i fältet Affärskalendernyckel när du anger användarinställningarna för katalogen. (Se [Konfigurera kataloger](/help/forms/using/admin-help/configuring-directories.md#configuring-directories).) Du kan sedan mappa affärskalendernycklarna (de värden som definieras för fältet&quot;land&quot; i LDAP-katalogen) till affärskalendrar i formulärarbetsflödet. (Se [Mappa användare och grupper till en affärskalender](configuring-business-calendars.md#mapping-users-and-groups-to-a-business-calendar).)

1. I formulärarbetsflödet definierar du en kalender för varje uppsättning användare som delar samma icke-arbetsdagar. (Se [Skapa eller uppdatera en affärskalender](configuring-business-calendars.md#create-or-update-a-business-calendar).)
1. I formulärarbetsflödet kan du mappa affärskalendernycklarna eller gruppmedlemskapen för varje kalender. (Se [Mappa användare och grupper till en affärskalender](configuring-business-calendars.md#mapping-users-and-groups-to-a-business-calendar).)
1. I Workbench väljer processutvecklaren om han eller hon ska använda affärskalendrar för påminnelser, deadlines och eskalering. (Se [Workbench-hjälp](https://www.adobe.com/go/learn_aemforms_workbench_63).)

   Om processutvecklaren väljer att använda affärskalendrar väljer AEM-formulär dynamiskt lämplig affärskalender baserat på inställningen för användarhantering och de företagskalendermappningar som definierats i administrationskonsolen. Om det inte finns några mappningar används standardkalendern.

   Om processutvecklaren inte använder affärskalendrar behandlas varje dag som en arbetsdag i datumberäkningen för händelsen. En deadline för en uppgift är till exempel konfigurerad att inträffa tre dagar efter att uppgiften har tilldelats en användare. Uppgiften tilldelas på torsdag. Tidsgränsen för aktiviteten infaller på söndag, trots att det är en helg.

## Skapa eller uppdatera en affärskalender {#create-or-update-a-business-calendar}

Om din organisation innehåller olika uppsättningar användare som har olika icke-arbetsdagar kan du definiera flera affärskalendrar. Du kan också ändra befintliga kalendrar, inklusive den inbyggda standardkalender som finns i AEM-formulär.

>[!NOTE]
>
>* Om du inte skapar någon affärskalender används standardkalendern.
>* Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Tjänster > Forms-arbetsflöde > Affärskalendrar.
1. Om du vill lägga till en ny affärskalender klickar du på ![bus_cal_plus](assets/bus_cal_plus.png). Texten *Ny kalender* visas i listrutan. Markera texten och ange ett annat namn för kalendern.

   Om du vill redigera en befintlig affärskalender väljer du den i listrutan.

1. Välj veckodagar som inte är arbetsdagar som standard, t.ex. helger.
1. [Valfritt] Välj Använd arbetstider och ange start- och sluttider för affärsdagarna.

   Om du väljer det här alternativet flyttas en händelse som inträffar före det angivna tidsintervallet till början av tidsintervallet och en händelse som inträffar efter att tidsintervallet har flyttats till starttiden för nästa arbetsdag.

   Tänk dig till exempel en situation där en användare tilldelas en uppgift kl. 2:00 på en tisdag, och påminnelsen för den uppgiften är inställd på två arbetsdagar. Utan arbetstider skulle påminnelsen hållas kl. 2.00 på torsdag. Om arbetstiderna är inställda på 08.00 till 17.00, flyttas påminnelsen till 08.00 på torsdag. Om en påminnelsehändelse skapades kl. 18.00 på tisdagen skulle påminnelsen inträffa efter kontorstid på torsdag. Med öppettider inställda på mellan 08.00 och 17.00, skulle påminnelsen ske kl. 08.00 på fredag.

1. Dubbelklicka i kalendern till vänster på andra icke-vardagar, till exempel helger. Du kan inte välja tidigare dagar. De icke-arbetsdagar som du väljer visas i en lista till höger med datumet två gånger på en rad. Välj datumet till vänster för att ange namn eller beskrivning för icke-affärsdagen.

   Om du vill ta bort en icke-arbetsdag från listan klickar du på ![bus_cal_trash](assets/bus_cal_trash.png) bredvid dagen.

1. [Valfritt] Om den här kalendern ska vara standardkalender väljer du Standardkalender. Standardkalendern används när ingen annan kalendermappning finns för användarassocierade händelser eller när ingen affärskalender har angetts för Timer-händelsen eller Wait-tjänsten. Du kan inte ta bort standardkalendern.
1. När du är klar med att definiera icke-arbetsdagar väljer du Aktiverad kalender och klickar sedan på Spara.

   Om du uppdaterar en befintlig kalender träder den nya versionen i kraft omedelbart och används för alla beräkningar i affärskalendern, även för uppgifter som redan körs.

   >[!NOTE]
   >
   >Om du inte aktiverar kalendern används standardkalendern.

## Mappa användare och grupper till en affärskalender {#mapping-users-and-groups-to-a-business-calendar}

Det finns två metoder som du kan använda för att associera en affärskalender med en användare. Du kan tilldela användare affärskalendrar baserat på en företagskalendernyckel eller utifrån den kataloggrupp som användaren tillhör. På fliken Mappning kan du ange vilken metod som ska användas i AEM-formulär och även mappa affärskalendernycklarna och -grupperna till affärskalendrar. Mer information om hur du associerar affärskalendernycklar med användare finns i [Konfigurera flera affärskalendrar](configuring-business-calendars.md#setting-up-multiple-business-calendars).

### Associera affärskalendrar med användare baserat på affärskalendernycklar {#associate-business-calendars-with-users-based-on-business-calendar-keys}

1. I administrationskonsolen klickar du på Tjänster > Formulärarbetsflöde > Affärskalendrar och sedan på fliken Mappning.
1. I listan Används väljer du Nyckelupplösning för användarhanterarens affärskalender.
1. Välj Visa affärskalendernyckel för användarhanteraren. En lista visas med en unik uppsättning affärskalendernycklar som har definierats i Användarhantering.

   För lokala domäner och hybriddomäner visar listan de värden som anges i fältet Nyckel för affärskalender i Användarhantering. För Enterprise-domäner (LDAP) visar listan den unika uppsättning som returneras från LDAP-fältet (till exempel&quot;country&quot;) som konfigurerats i LDAP-domäninställningarna.

   Om administratören för användarhantering inte har definierat några affärskalendernycklar är listan tom.

1. Välj en kalender för varje objekt i listan Nyckel för UM-affärskalender.
1. Klicka på Spara.

### Associera affärskalendrar med användare och grupper baserat på katalogtjänstgrupper {#associate-business-calendars-with-users-and-groups-based-on-directory-service-groups}

1. I administrationskonsolen klickar du på Tjänster > Formulärarbetsflöde > Affärskalendrar och sedan på fliken Mappning.
1. I listan System kommer att använda väljer du Grupper som definierats av katalogservern.
1. Välj Visa katalogtjänstgrupper på fliken Mappning. En lista visas med de grupper som har definierats i Användarhantering. (Se [Kataloginställningar](/help/forms/using/admin-help/configuring-directories.md#directory-settings).)

   >[!NOTE]
   >
   >Om du i Workbench har konfigurerat en användartjänst att använda affärskalendrar och tjänsten är tilldelad en grupp, använder AEM-formulär gruppmappningarna som anges här för att matcha kalendern för gruppen. AEM-formulär använder alltid gruppmappningar för att matcha kalendern för grupper, även när du använder affärskalendernycklar för att matcha kalendern för användare. Om ingen gruppmappning hittas används standardaffärskalendern.

1. Välj en kalender för varje objekt i listan Katalogtjänstgrupp.
1. Klicka på Spara.

## Exportera och importera affärskalendrar {#exporting-and-importing-business-calendars}

Med AEM-formulär kan du exportera och importera dina affärskalendrar som XML-filer. Du kan använda den här funktionen för att flytta kalendrar från ett mellanlagringssystem till ett produktionssystem.

>[!NOTE]
>
>Den här funktionen exporterar och importerar alla definierade affärskalendrar, inklusive standardaffärskalendern som tillhandahålls av AEM-formulär. En importerad affärskalender med samma namn som en befintlig kalender skriver över den befintliga kalendern.

### Exportera affärskalendrar {#export-business-calendars}

1. Klicka på Tjänster > Formulärarbetsflöde > Affärskalendrar i administrationskonsolen.
1. Klicka på Exportera och spara XML-filen.

### Importera affärskalendrar {#import-business-calendars}

1. Klicka på Tjänster > Formulärarbetsflöde > Affärskalendrar i administrationskonsolen.
1. Klicka på Importera.
1. Markera XML-filen som innehåller de exporterade affärskalendrarna och klicka på Öppna.

## Ta bort en affärskalender {#delete-a-business-calendar}

Du kan ta bort alla affärskalendrar som din organisation inte längre behöver. Om du tar bort en affärskalender som fortfarande är mappad till användare och grupper används standardkalendern.

1. I administrationskonsolen klickar du på Tjänster > Forms-arbetsflöde > Affärskalendrar.
1. Markera kalendern.
1. Klicka på Ta bort.
