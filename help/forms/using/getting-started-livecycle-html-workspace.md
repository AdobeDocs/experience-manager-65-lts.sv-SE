---
title: Komma igång med arbetsytan i AEM Forms
description: Hur man kommer igång med att använda arbetsytan i LiveCycle AEM Forms för att hantera automatiserade affärsprocesser.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: b38cd444-80f2-4747-9a99-68f69bd87e34
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '993'
ht-degree: 0%

---

# Komma igång med arbetsytan i AEM Forms {#getting-started-with-aem-forms-workspace}

Du kan använda AEM Forms arbetsyta för att utföra följande uppgifter:

* Påbörja en affärsprocess
* Visa och agera på uppgifter som är tilldelade dig eller på andra Att göra-listor som du har tillgång till
* Spåra uppgifter som ingår i de processer du påbörjade eller deltog i

## Navigera på arbetsytan i AEM Forms {#navigating-html-workspace}

Olika objekt i AEM Forms arbetsytans användargränssnitt visas beroende på vilken process och åtgärd du arbetar med. Du kan se flikarna Sammanfattning, Forms, Detaljer, Historik, Bilagor eller Anteckningar eller alla knappar som alltid beskrivs i den här hjälpen.

Du kan navigera i AEM Forms huvudgränssnitt på arbetsytan på något av följande sätt:

* Klicka på objekten i det övre navigeringsfältet för att komma åt alternativet Starta process, Att göra-lista, Inställningar, Spårning, Hjälp och Logga ut.
* Klicka på fliken Starta process, Att göra eller Spärra/knip för att komma åt de tre huvudarbetsytorna.
* På flikarna Starta process, Att göra och Spärra/knip klickar du på objekten i listan i den vänstra panelen för att få tillgång till favoriter, processkategorier, sökmallar, utkast eller tilldelade uppgifter. Använd rullningslisten för att se fler objekt i listan.
* Alla åtgärdsknappar - Godkänn, Avvisa, Framåt, Konsult, Lås och Dela - visas i både dokumentet och ägarskapet.
* Klicka på ikonen Alla alternativ i navigeringsfältet, längst ned på sidan, om du vill vidarebefordra uppgiften till en annan användare, dela uppgiften med en annan användare, om du vill fråga om uppgiften med en annan användare eller om du vill låsa uppgiften.
* På fliken Historik väljer du en uppgift som visar flikarna Bifogade filer och Uppdrag för den uppgiften.
* Använd tabbtangenten, piltangenterna och blankstegstangenten för att navigera på arbetsytan i AEM Forms utan att använda en mus.

## Använda AEM Forms arbetsyta med skärmläsare {#using-html-workspace-with-screen-readers}

AEM Forms arbetsyta är ett webbaserat HTML-program som är kompatibelt med skärmläsare. Du kan navigera i AEM Forms arbetsyta med hjälp av tangentbordet.

Om du vill använda AEM Forms-arbetsytan med en skärmläsare bör du tänka på följande:

* AEM Forms arbetsyta är ett HTML-standardprogram som uppfyller alla vanliga skärmläsarverktyg. Du behöver inga specifika skript för att köra ett skärmläsarverktyg.
* All navigering på arbetsytan i AEM Forms sker via ankartaggar, som du enkelt kommer åt via flikar.
* Forms kan ta några sekunder att ladda. Skärmläsaren talar inte hörbart om att formuläret läses in och att du måste vänta.

## Navigera på arbetsytan i AEM Forms med ett tangentbord {#navigating-html-workspace-using-a-keyboard}

När du navigerar på AEM Forms arbetsyta med hjälp av ett tangentbord följer navigeringen HTML tillgänglighetskonventioner. I vissa situationer följer tabbordningen inte den vanliga vanliga ordningen. Följande tips hjälper dig att navigera i gränssnittet:

* Om du har problem med att tabba ut från verktygsfälten längst upp i webbläsaren trycker du på Ctrl+Tabb för att tabba in innehållet i webbläsarfönstret.
* Hjälp om arbetsytan i AEM Forms öppnas i ett separat webbläsarfönster. När du har visat hjälpen återgår fokus till webbläsarfönstret som innehåller AEM Forms-arbetsytan. Hjälpmenyn förblir fokuserad när fokus återgår.
* När du öppnar ett formulär för att starta en process eller slutföra en åtgärd behåller fokus det befintliga elementet och ändras inte till formuläret. Använd fliken för att flytta fokus till formuläret och bläddra igenom det. Tabbordningen i formuläret beror på formulärets typ och design.

  När du går till formulärets slut med tabbtangenten eller skickar formuläret i PDF forms flyttas markörens fokus till webbläsarens adressfält. Gå igenom menyerna igen (men inte hela formuläret) för att gå till formuläråtgärdsknapparna, till exempel Spara som utkast och Fullständigt. Om formuläret fortfarande är öppet kan du även tabba förbi knapparna och tillbaka till formuläret.

## Hantera inställningar {#managing-preferences}

Du kan ange olika inställningar för AEM Forms-arbetsytan i följande kategorier:

**Frånvarande:** Ange inställningar för hur uppgifter tilldelas andra personer när du inte är på kontoret. Se [Inställningar för frånvaro](todo-lists.md#setting-out-of-office-preferences).

**Köer:** Ange inställningar för att dela din Att göra-lista med andra användare eller för att begära åtkomst till en annan användares lista. Se [Arbeta med uppgifter från grupper och delade köer](todo-lists.md#working-with-tasks-from-group-and-shared-queues).

**Användargränssnittsinställningar:** Ange inställningar för hur du interagerar med AEM Forms arbetsyta. Se [Ange användargränssnittsinställningar](#set-user-interface-preferences).

### Ange inställningar för användargränssnitt {#set-user-interface-preferences}

Ange gränssnittsinställningarna på fliken Inställningar > Användargränssnittsinställningar. Följande inställningar är tillgängliga.

* **Startplats:** Anger den sida som visas när du loggar in på arbetsytan i AEM Forms. De fyra tillgängliga alternativen är Starta process, Att göra, Spårning och Favoriter.
* **Utloggningsfråga:** Anger om du uppmanas att bekräfta att du vill logga ut när du klickar på Logga ut.
* **Datumformat:** Anger vilket datumvisningsformat som ska användas på AEM Forms-arbetsytan.
* **Tidsformat**: Anger vilket tidsvisningsformat som ska användas på AEM Forms-arbetsytan.
* **Meddela aktivitetshändelser via e-post:** Anger om du får e-postmeddelanden för aktivitetshändelser, inklusive aktivitetstilldelningar, påminnelser och deadlines för uppgifter i din Att göra-lista och i gruppAtt göra-listor som du tillhör.
* **Bifoga Forms i e-post:** Anger om en kopia av formuläret är bifogad till e-postmeddelanden. Bifogade filer stöds bara för PDF- och XDP-formulär.
* **Spara utkast regelbundet:** Anger om dina formulärutkast sparas automatiskt regelbundet eller inte. Om du vill spara dina utkast regelbundet aktiverar du det här alternativet och anger hur länge du vill spara automatiskt från 1 till 30 minuter. När Spara automatiskt är aktiverat och en användare arbetar med ett utkast, sparas utkastet regelbundet efter det angivna antalet minuter. Utkastet sparas bara automatiskt när det har ändrats sedan du senast sparade eller sparade det automatiskt. När utkastet sparas visas ett varningsmeddelande på skärmen.
