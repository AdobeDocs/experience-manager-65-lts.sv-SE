---
title: Åtkomst och ifyllande av publicerade formulär
description: Forms Portal förser webbutvecklare med komponenter för att skapa och anpassa en Forms Portal på webbplatser som skapats med Adobe Experience Manager (AEM).
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Forms Portal
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 0%

---

# Åtkomst och ifyllande av publicerade formulär{#accessing-and-filling-published-forms}

I en formulärcentrerad portaldistribution är formulärutveckling och portalutveckling två skilda aktiviteter. När formulärdesigners designar och lagrar formulär i en databas skapar webbutvecklare ett webbprogram som följer den listan och hanterar inskickade formulär. Forms kopieras sedan till webbnivån eftersom det inte finns någon kommunikation mellan formulärdatabasen och webbprogrammet.

Detta leder ofta till problem med att hantera installations- och produktionsförseningar. Om det till exempel finns en nyare version av ett formulär i databasen ersätter formuläret Designer formuläret på webbnivån, ändrar webbprogrammet och distribuerar om formuläret på den offentliga webbplatsen. Omdistribuering av webbprogrammet kan orsaka serverdriftavbrott. Eftersom serverns driftstopp är en planerad aktivitet kan ändringarna inte skickas direkt till den offentliga webbplatsen.

Forms Portal minskar omkostnaderna och förseningarna i produktionen. Den förser webbutvecklare med komponenter för att skapa och anpassa en Forms Portal på webbplatser som skapats med Adobe Experience Manager (AEM).

Mer information om Forms Portal och dess funktioner finns i [Introduktion till att publicera formulär på en portal](/help/forms/using/introduction-publishing-forms.md).

## Komma igång med Forms Portal {#getting-started-with-forms-portal}

Navigera till den publicerade Forms Portal-sidan. Mer information om hur du skapar en Forms Portal-sida finns i [Skapa en Forms Portal-sida](../../forms/using/creating-form-portal-page.md).

Komponenten Search and Lister i Forms Portal visar de formulär som är tillgängliga i publiceringsinstansen på AEM-servern. Den här listan innehåller alla formulär eller de formulär som definieras i filtret vid redigeringen av Forms Portal-sidan. En Forms Portal-sida ser ut ungefär som i bilden nedan:

![En exempelformulärportalsida ](assets/forms-portal-page.png)

Exempel på en Forms Portal-sida

### Sök och visa {#search-and-lister}

Med komponenten Sök och Lister kan du lägga till följande funktioner i din Forms Portal:

* Visa en lista över formulär som är tillgängliga direkt i panelen, kortet eller stödrastervyn. Det har även stöd för anpassade mallarLista formulär från specifika mappar i Forms Manager.
* Ange hur formulär ska återges - HTML5, PDF eller båda.
* Ange hur PDF- och XFA-formulär återges - HTML5, PDF eller båda. Icke-XFA-formulär som HTML5.
* Möjliggör sökning av formulär baserat på kriterier som formuläregenskaper, metadata och taggar.
* Skicka formulärdata till en server.
* Använd anpassade formatmallar (CSS) för att anpassa utseendet och känslan i portalen.
* Skapa länkar till formulär.

Du kan söka efter formulär på Forms Portal-sidan med följande alternativ:

* Fullständig textsökning
* Avancerad sökning

Med fulltextsökning kan du söka efter och lista formulär baserat på angivna nyckelord.

![En avancerad sökdialogruta](assets/search-panel.png)

En avancerad sökdialogruta

Med avancerad sökning kan du söka i formulär baserat på angivna formuläregenskaper. Detta ger ett mer specifikt resultat än fulltextsökning. I avancerad sökning ingår sökning baserat på taggar, egenskaper (till exempel Författare, Beskrivning och Titel), ändringsdatum och fullständig text.

Listan visar formulär som baseras på sökparametrarna. Varje formulär i sökresultatet visas med en ikon, som är hyperlänkad till det associerade formuläret. Du kan klicka på ikonen för att öppna och arbeta med det associerade formuläret.

### Fylla i ett formulär {#filling-a-form}

![Ett adaptivt exempelformulär](assets/filling_a_form.png)

Ett adaptivt exempelformulär

Formulären kan nås via länken som medföljer formuläret i sid- och listkomponenten.

Varje formulär innehåller hjälpinformation som gör att användaren kan fylla i formuläret.

#### Utkast och inlämning {#drafts-and-submission}

En användare kan välja att spara ett utkast av ett formulär genom att klicka på **Spara**. Detta gör att användaren kan arbeta med ett formulär under en tidsperiod innan formuläret skickas.

De data som fylls i formuläret (inklusive bilagor) sparas som ett utkast på servern. Utkastet kan sparas hur många gånger som helst. Det sparade formuläret visas på fliken Utkast i delen Utkast &amp; överföring på sidan.

När formuläret har fyllts i skickar användaren formulären genom att klicka på knappen Skicka i formuläret. De skickade formulären visas på fliken Skicka in i delen Utkast &amp; överföring på sidan.

>[!NOTE]
>
>Skickade formulär visas bara på fliken Skickat Forms om skicka-åtgärden för det adaptiva formuläret har konfigurerats som Forms Portal Submit Action (Skicka-åtgärd). Mer information om Skicka-åtgärder finns i [Konfigurera Skicka-åtgärden](../../forms/using/configuring-submit-actions.md).

![Komponenten för utkast och inskickade dokument](assets/draft-submission.png)

Komponenten Utkast och inskickat material

## Starta ett nytt formulär med skickade formulärdata {#start-a-new-form-using-submitted-form-data}

Det finns vissa formulär som du måste fylla i och skicka ofta. Formuläret för att skicka in en individuell skattedeklaration skickas till exempel varje år. I sådana fall ändras en del av informationen varje gång du fyller i formuläret, men de flesta av dem, som personuppgifter och familjeuppgifter, ändras inte. Du måste dock ändå fylla i hela formuläret från grunden igen.

AEM Forms kan hjälpa till att optimera formulärifyllningen och avsevärt minska tiden för att fylla i och skicka in ett formulär igen. Slutanvändare kan starta ett nytt formulär med data från ett skickat formulär. Den här funktionen är inbyggd i komponenten [Utkast och överföringar](../../forms/using/draft-submission-component.md). När du lägger till komponenterna Utkast och Skicka på din Forms Portal-sida och publicerar den ser slutanvändarna ett alternativ på flikarna Skickat Forms och Utkast till Forms. Med det här alternativet kan du starta ett nytt formulär med data från ett skickat formulär. Följande bild markerar det alternativet.

![start-a-new-form](assets/start-a-new-form.png)

När du klickar på knappen för att initiera ett nytt formulär öppnas ett nytt formulär med data från motsvarande skickade formulär. Nu kan du granska och uppdatera informationen efter behov och skicka in formuläret.
