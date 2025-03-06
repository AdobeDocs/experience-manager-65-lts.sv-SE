---
title: Arbeta med uppgifter
description: Använd sidan Uppgiftssökning om du vill söka efter uppgifter efter användarnamn eller uppgifts-ID. Läs mer om hur du arbetar med uppgifter.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: d6a0caed-99fa-4121-ac2e-bc21626ff9e0
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# Arbeta med uppgifter {#working-with-tasks}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Använd sidan Uppgiftssökning om du vill söka efter uppgifter efter användarnamn eller uppgifts-ID. Sökresultaten visas på sidan Uppgiftslista där du kan komma åt en uppgifts historik. Du kan också tilldela om en uppgift som en användare har för många uppgifter eller om en användare har fått en aktivitetstilldelning av fel.

>[!NOTE]
>
>Vid uppgiftssökningar returneras inga resultat för användarnamn som börjar med ett nummertecken (#). Undvik om möjligt att skapa användarnamn som börjar med ett nummertecken.

## Sök efter uppgifter som är kopplade till en användare {#search-for-tasks-associated-with-a-user}

1. Klicka på Tjänster > Formulärarbetsflöde > Uppgiftssökning i administrationskonsolen.
1. Välj Användarnamn i Sök efter. Om du känner till en del av användarnamnet som du söker efter skriver du det i rutan. Klicka på Sök efter användare.
1. Sidan Sök efter användare visas. Du kan förfina sökningen ytterligare genom att söka efter användarnamn eller e-postadress. När du hittar användaren du söker efter markerar du alternativknappen bredvid namnet och klickar på OK.
1. Som standard söker uppgiftssökningen efter uppgifter som för närvarande är tilldelade användaren. Om du även vill söka efter uppgifter som tidigare har tilldelats användaren väljer du Visa ej tilldelad uppgift. Om du även vill söka efter uppgifter som användaren har slutfört väljer du Visa slutförd uppgift.
1. Klicka på Sök. Sidan Uppgiftslista visas med sökresultatet.

## Sök efter en uppgift när du känner till dess uppgifts-ID {#search-for-a-task-when-you-know-its-task-id}

1. Klicka på Tjänster > Formulärarbetsflöde > Uppgiftssökning i administrationskonsolen.
1. I Sök efter väljer du Aktivitets-ID och skriver uppgifts-ID i rutan.
1. Klicka på Sök. Sidan Uppgiftslista visas med sökresultatet.

## Arbeta med uppgiftslistan {#working-with-the-task-list}

Resultatet av en uppgiftssökning visas på sidan Uppgiftslista. Du kan välja en uppgift för att öppna sidan Uppgiftshistorik. Därifrån kan du tilldela uppgiften till en annan användare.

Uppgifterna visas med följande information:

**Aktivitets-ID:** Det positiva heltal som formulärarbetsflödet tilldelar när aktiviteten initieras (initieras av en användare). Du kan använda den här identifieraren för att spåra uppgiften genom dess livscykel. Klicka på ett aktivitets-ID om du vill visa information om aktivitetshistoriken eller om du vill tilldela om uppgiften till en annan användare.

**Status:** Tilldelad innebär att uppgiften är tilldelad användaren. Ej tilldelad innebär att uppgiften tidigare har tilldelats användaren. Status kan också vara Slutförd.

**Aktivitet:** Visar formuläret och namnet på en inledande åtgärd eller processåtgärden som genererade aktiviteten.

**Process-ID:** Detta positiva heltal som tilldelas av formulärarbetsflödet när processen initieras (det vill säga när en användare eller ett automatiskt steg initierar en process). Du kan använda den här identifieraren för att spåra processinstansen genom dess livscykel.

**Processnamn - Version:** Processens namn, enligt definition i Workbench.

**Program:** Namnet på programmet som processen tillhör, enligt definition i Workbench.

**Skapad:** Datum och tid då aktiviteten skapades.

## Visa aktivitetshistorik och tilldela om uppgifter {#viewing-task-history-and-reassigning-tasks}

På sidan Uppgiftshistorik visas en lista över de användare och grupper som har tilldelats en viss uppgift.

För varje uppgiftstilldelning visas följande information i listan:

**Namn:** Användarens namn.

**Status:** Tilldelad innebär att uppgiften för närvarande är tilldelad användaren. Ej tilldelad innebär att uppgiften tidigare har tilldelats användaren.

**Arbets-ID:** Den numeriska identifieraren för den användarkö som uppgiften tillhör. En process kan delas av flera användare.

**Typ:** Anger hur uppgiften tilldelades:

**Inledande:** Användaren tilldelades ursprungligen aktiviteten.

**Vidarebefordra:** Den ursprungliga aktivitetsägaren tilldelade uppgiften till en annan användare.

**Avvisa:** En vidarebefordrad aktivitet avvisades eller så returnerades en aktivitet till en arbetslista utan att ha slutförts.

**Anspråk:** Användaren gjorde anspråk på uppgiften i en delad arbetslista.

**Eskalering:** En förbestämd tid förflutit (som anges i användaråtgärden i Workbench) utan användarinteraktion och en annan användare tilldelades uppgiften.

**Konsult:** Aktivitetsägaren har vidarebefordrat den här aktiviteten till en annan användare för konsultation som kan öppna formuläret, spara data, ändra bilagor och anteckningar, men inte slutföra steget. Användaren måste returnera uppgiften till aktivitetsägaren som har rådfrågat användaren.

**Administratörsomtilldelning:** Uppgiften har omtilldelats av en administratör.

**Tilldelningsdatum:** Datum och tid då uppgiften tilldelades användaren.

### Tilldela en ny användare till en uppgift {#assigning-a-new-user-to-a-task}

På sidan Tilldela användare visas de användare som kan tilldelas till en uppgift. Du kommer åt sidan Tilldela användare genom att klicka på Tilldela ny användare på sidan Uppgiftshistorik.

1. I rutan Sök efter på sidan Tilldela användare skriver du en del av eller hela det användarnamn eller den e-postadress som krävs.
1. Under Använda väljer du Namn eller E-postadress och klickar sedan på Sök. De användare som matchar sökningen visas.
1. Markera användaren i listan och klicka på OK. Sidan Uppgiftshistorik visas med det nya användaruppdraget.
