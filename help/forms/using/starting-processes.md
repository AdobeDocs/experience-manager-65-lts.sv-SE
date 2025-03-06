---
title: Startar processer
description: 'Så här använder du arbetsytan i LiveCycle AEM Forms: markera processer, lägga till anteckningar och bilagor, spara utkast och lägg till i favoriter.'
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: 074b0cb2-b060-4180-ab16-ff8f7a4ee625
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1334'
ht-degree: 0%

---

# Startar processer {#starting-processes}

AEM Forms arbetsyta organiserar processerna enligt de kategorier som administratören eller processdesignern ställer in. Du kan också placera processer som du använder ofta i kategorin Favoriter så att du snabbt kan hitta dem.

När du påbörjar en process kan du behöva fylla i ett formulär för att starta en affärsprocess som styrs av AEM Forms arbetsflöde. Om ett formulär använder Förbered dataprocess kan viss information fyllas i i förväg i ett tomt formulär när en ny process initieras.

Du vill till exempel köpa en ny datorskärm och därför starta en process som heter *Inköpsorder*. När du startar processen öppnas ett formulär där du uppmanas att ange information om objektet som ska beställas. Ditt namn, personalnummer och chefens namn kan redan vara ifyllda i förväg i formuläret. När du skickar begäran initieras en affärsprocess. Servern dirigerar automatiskt begäran till din hanterare baserat på processdefinitionen. Uppgiften börjar visas i din chefs Att göra-lista. När din chef har godkänt begäran vidarebefordrar formulärarbetsflödet begäran till inköpsavdelningen och skickar ett e-postmeddelande till dig.

## Välja processer att starta {#selecting-processes-to-start}

Du kan välja en process för att starta den eller för att visa mer information om den.

När du väljer en process att starta kan du behöva fylla i ett formulär som är kopplat till den processen. Processen startas när du skickar formuläret.

Forms i olika filformat stöds, bland annat Adobe PDF-, HTML- och SWF-filer. Ett formulär kan se ut som ett vanligt utskrivbart eller webbaserat formulär eller vägleda dig genom en serie guideliknande paneler för att samla in information.

Om formuläret och processen tillåter det kan du även spara formuläret offline, fylla i det och sedan skicka det för att slutföra uppgiften. När formuläret skickas startas din e-postklient med rätt e-postadress för servern, om e-postslutpunkten har konfigurerats. Du kan sedan skicka det ifyllda formuläret till servern via e-post.

När du väljer en process visas fliken Formulär och fliken Detaljer. Om processen tillåter dig att lägga till anteckningar eller bilagor, visas även fliken Bifogade filer och fliken Anteckningar. Om du även har konfigurerat sammanfattnings-URL:en med processen visas även fliken Sammanfattning. På fliken Forms visas det associerade formuläret och på fliken Detaljer visas information om den aktuella uppgiften och den process som den är en del av.

### Påbörja en affärsprocess {#start-a-business-process}

1. Välj en kategori i listan till vänster på sidan Starta process. Alla processer som du har tillgång till i kategorin visas till höger.

   >[!NOTE]
   >
   >Om kategorirutan är komprimerad klickar du på Öppna kategorier i det övre vänstra området på arbetsytan i AEM Forms för att öppna rutan.

1. Välj en process genom att klicka på en uppgift. Formuläret som är kopplat till processen öppnas på fliken Formulär.

   Alla formulär i en process har en unik URL. Du kan använda den unika URL-adressen för att starta HTML Workspace direkt med den specifika processen och formuläret. URL-adressen har formatet https://&lt;server>:&lt;port>/lc/libs/ws/index.html#/startprocess/&lt;ApplicationName>%2F&lt;ProcessName>. Strängen &lt;ApplicationName>%2F&lt;ProcessName> är alltid URL-kodad. Ett exempel-URL är http://localhost:8080/lc/libs/ws/index.html#/startprocess/MyApplication%2FNewProcess. Strängen ApplicationName%2FPProcessName i exemplet är URL-kodad.

1. Fyll i formuläret enligt instruktionerna som medföljer det. Om det behövs klickar du på **Maximera** för att öka formulärets synliga område.
1. Om fliken Bifogade filer är tillgänglig lägger du till bifogade filer efter behov.
1. Om fliken Anteckningar är tillgänglig kan du göra nödvändiga anteckningar.
1. Gör något av följande:

   * Klicka på knappen Skicka i formuläret, om formuläret har en Skicka-knapp.
   * Klicka på Slutför under formuläret om formuläret inte har någon Skicka-knapp.

   Processhanteringen startar processen och skickar formuläret till Att göra-listorna med lämpliga personer som behöver slutföra nästa uppgift i processen.

   Om du måste stänga ett formulär innan du skickar in det och utan att förlora data som du har angett, sparar du ett utkast och slutför det senare om processen tillåter det. Om formuläret och processen tillåter det kan du även klicka på **Offline** och skicka det senare från Adobe® Reader® eller Adobe® Acrobat® Professional eller Acrobat Standard.

   >[!NOTE]
   >
   >Offlinealternativet är endast tillgängligt för PDF forms.

## Lägga till anteckningar och bilagor {#adding-notes-and-attachments}

Du kan lägga till anteckningar och bifogade filer i en process om processen tillåter det. Du kan ge andra användare behörighet att visa, uppdatera och ta bort anteckningar och bilagor.

### Lägga till en anteckning {#add-a-note}

Du kan lägga till flera anteckningar, redigera de skrivna anteckningarna och ta bort dem. Varje anteckning har en titel, en beskrivning och en åtkomstbehörighet. Du kan ange någon av följande åtkomstbehörigheter för en anteckning:

* Skrivskyddad (standardbehörighet)
* Läs/redigera/ta bort
* Läs/redigera
* Läs/ta bort
* Ingen åtkomst

1. Öppna en uppgift och klicka på fliken **Anteckningar** om processen tillåter det.
1. Skriv en rubrik för anteckningen i rutan **Titel** och skriv anteckningstexten i rutan **Anteckning**.
1. Välj nivån **Behörigheter** för anteckningen för andra användare som deltar i processen.
1. Klicka på **OK**. En textfil som innehåller din anteckning bifogas till formuläret. Du kan uppdatera en anteckning genom att klicka på den och direkt ändra texten. Du kan ta bort en anteckning genom att klicka på knappen **Ta bort** ![Bild på en papperskorgen](assets/icondelete.png) bredvid anteckningen.

### Lägg till en bifogad fil {#add-an-attachment}

Du kan också lägga till dina kommentarer om den bifogade filen. Du kan ange någon av följande åtkomstbehörigheter för en bifogad fil:

* Skrivskyddad (standardbehörighet)
* Läs/redigera/ta bort
* Läs/redigera
* Läs/ta bort
* Ingen åtkomst

1. Klicka på fliken **Bifogade filer** och välj **Bifogad fil**.
1. Klicka på **Bläddra** för att välja filen som ska bifogas.
1. Välj nivån **Behörigheter** för den bifogade filen för andra användare som deltar i processen. Om du väljer **Läs** kan andra användare spara filen lokalt. Om du väljer någon av redigeringsbehörigheterna kan andra användare även överföra en ny fil som ersätter den bifogade filen.
1. Klicka på **OK**. Filen bifogas till formuläret. Du kan ta bort en fil genom att klicka på knappen **Ta bort** ![Bild på ett papperskorgen](assets/icondelete.png) bredvid den bifogade filen.

## Spara utkastkopior av formulär {#saving-draft-copies-of-forms}

Om du måste fylla i och skicka ett formulär senare kan du spara ett utkast av ett formulär så att du inte förlorar ditt befintliga arbete. Utkastkopior läggs till i kategorin Utkast på sidan Att göra.

När du har öppnat och skickat ett utkast på nytt tas utkastet bort från kategorin Utkast.

Du kan också konfigurera arbetsytan så att den information som användaren anger som ett utkast sparas automatiskt. Mer information finns i [Hantera inställningar](/help/forms/using/getting-started-livecycle-html-workspace.md).

>[!NOTE]
>
>Knappen Spara är inte tillgänglig för vissa formulär, beroende på vilken process det är kopplat till.

### Spara ett utkast {#save-a-draft-copy}

1. Klicka på **Spara** i det nedre vänstra hörnet på en flik. Formuläret läggs till i kategorin Utkast på din att göra-sida. Alla ändringar som du har gjort i formuläret sparas.

### Öppna ett utkast igen {#reopen-a-draft-copy}

1. På sidan Att göra markerar du kön **Utkast** och klickar på utkastet av formuläret.

   Om formuläret innehåller en serie paneler kan du behöva gå till den panel där du avslutade din senaste session.

## Lägga till processer i kategorin Favoriter {#adding-processes-to-the-favorites-category}

Du kan lägga till alla processer i kategorin Favoriter. Genom att ange favoriter kan du gruppera alla processer som du ofta börjar i en enda kategori så att du snabbt kan hitta dem.

>[!NOTE]
>
>Om du vanligtvis startar processer när du använder arbetsytan i AEM Forms kan du ange inställningen Startplats som automatiskt visar kategorin Favoriter när du startar arbetsytan i AEM Forms. Mer information finns i Hantera inställningar i [Komma igång med AEM Forms-arbetsytan](/help/forms/using/getting-started-livecycle-html-workspace.md).

Markera en process som favorit genom att markera den i sin kategori och klicka på den ofyllda stjärnan. Stjärnan blir guld. Om du vill avmarkera en process som en favorit klickar du på den gyllene stjärnan igen.
