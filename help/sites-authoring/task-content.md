---
title: Arbeta med uppgifter
description: Uppgifter representerar arbetsuppgifter som ska utföras på innehåll och används i projekt för att fastställa slutförandenivån för aktuella uppgifter
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: projects
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 852aaf6e-acf3-4224-bf4c-c0913110abd4
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# Arbeta med uppgifter {#working-with-tasks}

Uppgifter representerar arbetsuppgifter som ska utföras med avseende på innehåll. När du tilldelas en uppgift visas den i Inkorgen för arbetsflöde. Uppgiftsobjekt kan särskiljas från arbetsflödesobjekt med värdet i kolumnen **Typ**.

Uppgifter används också i projekt för att fastställa projektets fullständighetsnivå.

## Spåra projektförlopp {#tracking-project-progress}

Du kan spåra projektförloppet genom att titta på de aktiva/slutförda aktiviteterna i ett projekt som representeras av **aktivitetspanelen**. Projektets förlopp kan avgöras av:

* **Åtgärdspanel:** Ett övergripande förlopp för projektet visas i aktivitetspanelen, som finns på sidan med projektinformation.

* **Uppgiftslista:** När du klickar på aktivitetspanelen visas en lista med uppgifter. Den här listan innehåller detaljerad information om alla uppgifter som rör projektet.

Båda alternativen listar arbetsflödesuppgifter och uppgifter som du skapar direkt i aktivitetspanelen.

### Åtgärdsfönster {#task-tile}

Om ett projekt innehåller några relaterade uppgifter visas en aktivitetspanel i projektet. Aktivitetsrutan visar projektets aktuella status. Detta baseras på befintliga uppgifter i arbetsflödet och inkluderar inga uppgifter som kommer att genereras i framtiden allt eftersom arbetsflödet fortsätter. Följande information visas i åtgärdsrutan:

* Procent slutförda uppgifter
* Procent av aktiva uppgifter
* Procent av försenade uppgifter

![Aktivitetspanelen](assets/project-tile-tasks.png)

### Visa eller ändra uppgifter i ett projekt {#viewing-or-modifying-the-tasks-in-a-project}

Förutom att följa upp förloppet kanske du också vill visa mer information om projektet eller ändra det.

#### Uppgiftslista {#task-list}

Klicka på ellipsknappen längst ned till höger i aktivitetspanelen för att visa din inkorg filtrerad på aktiviteter relaterade till projektet. Uppgiftsinformationen visas tillsammans med metadata som förfallodatum, tilldelad, prioritet och status.

![Inkorg för projektuppgift](assets/project-tasks.png)

#### Uppgiftsinformation {#task-details}

Om du vill ha mer information om en viss uppgift klickar du i inkorgen på uppgiften för att markera den och sedan på **Öppna** i verktygsfältet.

![Uppgiftsinformation](assets/project-task-detail.png)

Du kan visa, redigera och lägga till information i en uppgift via olika flikar.

* **Aktivitet** - Allmän aktivitetsinformation
* **Projektinformation** - Sammanfattning av projektet som aktiviteten är associerad med
* **Arbetsflöde i** - Sammanfattning av arbetsflödet som aktiviteten är kopplad till (om tillämpligt)
* **Kommentarer** - Allmänna kommentarer för själva aktiviteten

### Lägga till uppgifter {#adding-tasks}

Du kan lägga till nya uppgifter i projekt. Dessa uppgifter visas sedan i aktivitetspanelen och är tillgängliga i inkorgen för meddelanden så att du är medveten om dina utestående uppgifter.

Så här lägger du till en uppgift:

1. Leta reda på **aktivitetspanelen** i projektet
1. Klicka på nedåtpilen längst upp till höger i rutan och välj **Skapa aktivitet**.
1. I fönstret **Lägg till uppgift** anger du aktivitetsinformation som prioritet, tilldelad och förfallodatum.

   ![Lägger till en aktivitet](assets/project-add-task.png)

1. Klicka på **Skicka**.

## Arbeta med uppgifter i Inkorgen {#working-with-tasks-in-the-inbox}

I stället för att komma åt dina projektuppgifter från själva projektet kan du komma åt dem direkt från din inkorg. Inkorgen ger dig en översikt över dina uppgifter i olika projekt så att du kan förstå hela arbetsflödet.

I inkorgen kan du öppna uppgifterna och ange aktivitetsstatus. Uppgifter visas också i inkorgen när de tilldelas till en användargrupp som du tillhör. I det här fallet kan alla medlemmar i gruppen utföra arbetet och slutföra uppgiften.

![Inkorg](assets/project-inbox.png)

Slutför en åtgärd genom att markera den och klicka på **Slutför** i verktygsfältet. Lägg till information för aktiviteten och klicka sedan på **Klar**. Mer information finns i [Inkorgen](/help/sites-authoring/inbox.md).
