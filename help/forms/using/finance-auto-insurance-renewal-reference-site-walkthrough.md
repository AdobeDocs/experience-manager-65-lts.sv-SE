---
title: Genomgång av referenswebbplatsen för förnyelse av autoförsäkring
description: Läs mer om referenswebbplatsen"We.Finance" Auto Insurance Renewal genom en genomgång.
contentOwner: dekalra
products: SG_EXPERIENCEMANAGER/6.5/FORMS
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Foundation Components
role: User, Developer
exl-id: 3f9f1a20-9029-4e30-9c9d-ef452512f7e9
source-git-commit: c0bf6864bb344e582c4f88371c892d401ce2827c
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Genomgång av referenswebbplatsen för förnyelse av automatisk försäkring för `We.Finance`{#we-finance-auto-insurance-renewal-reference-site-walkthrough}

## Scenario för `We.Finance`-referensplats  {#we-finance-reference-site-scenario}

Webbplatsen `We.Finance` är en webbplats för finansiella tjänster som är utformad för att hjälpa dig att lära dig AEM Forms interaktiva kommunikationsfunktioner.

Läs en detaljerad genomgång av ett `We.Finance` Auto Insurance-exempel som visar hur AEM-formulär och dess integrering med Microsoft® Dynamics hjälper till att personalisera kundupplevelsen i ett finansföretag. Den interaktiva genomgången är utformad för att underlätta implementering av komplexa digitala transaktioner och kundkommunikation i ett finansföretag.

**Resan börjar med användningsexemplet:**

Sarah Rose är en befintlig `We.Finance`-kund och har köpt en policy för automatisk försäkring. Det är årets tid för Sarah att förnya sin försäkring. Gloria Rios är sin försäkringsagent. Webbplatsen `We.Finance` skickar en påminnelse med e-post till Sarah om sin förnyelse av principen. Sarah följer instruktionerna i e-postmeddelandet och slutför processen.

## Genomgång av självförsäkringsprogram {#auto-insurance-application-walkthrough}

Scenariot `We.Finance` Auto-Insurance är en visuell berättarröst för användaren och baseras på två profiler:

* Sarah Rose, en `We.Finance`-kund
* Gloria Rios, försäkringsagent, `We.Finance`

### Gloria skickar ett meddelande om förnyelse av försäkringsavtal från `We.Finance` {#gloria-sends-an-insurance-policy-renewal-communication-from-we-finance}

Gloria loggar in på AEM-instansen, klickar på **Förnyelse av automatisk försäkring** och klickar sedan på **Användargränssnittet för Open Agent**. Klicket fyller i försäkringsdokumentet med information om Sarah Rose. Gloria klickar på **Submit** och ett meddelande visas på skärmen&quot;Submission Initiated&quot; (Sändningen har initierats) och sedan om några sekunder&quot;Submit Successful&quot; (Skickades utan fel).

Sarah får ett mejl med texten&quot;Your Auto Insurance Renewal&quot;.

![Agentgränssnitt](assets/agent_ui_email_new.png)

#### Se det själv {#see-it-yourself}

Gå till **Adobe Experience Manager** > **Forms** > **Forms &amp; Documents** > **`We.Finance`** > **Automatisk försäkring**. Välj den automatiska försäkringsförnyelsen **interaktiv kommunikation** och klicka på **Öppna agentgränssnittet**. Den interaktiva kommunikationen öppnas i agentens användargränssnitt. Ange en giltig e-postadress så att de kan ta emot e-postmeddelandet med det bifogade principdokumentet och klicka på Skicka.

Du kan komma åt och granska den interaktiva kommunikationen om förnyelse av autoförsäkring direkt från `https://[authorHost]: authorPort]/aem/formdetails.html/content/dam/formsanddocuments/we-finance/autoinsurance/auto-insurance-renewal.`

### Sarah får ett meddelande om förnyelse av försäkringsavtal från `We.Finance` och bestämmer sig för att förnya {#sarah-receives-an-insurance-policy-renewal-communication-from-we-finance-and-decides-to-renew}

Sarah får ett e-postmeddelande med en bifogad fil från `We.Finance`, som påminner Sarah om att hennes policy för autoförsäkring håller på att upphöra att gälla. Den bifogade filen är en tryckt version av Sarah&#39;s Auto Insurance Brev.

Sarah klickar på alternativet **Förnya nu** och är dirigerad till webbversionen av sitt Autofoningsbrev. Utöver det här brevet hittar Sarah hur lång tid som återstår i policyn innan det går ut. Sidan innehåller en översikt över försäkringsvillkoren. Här finns information om Policynummer, Belopp att betala, Rabatterbjudanden och Förmånsbelöningar. **Förnya nu** klickas längst ned i principen.

![ref1](assets/ref1.png)

#### Så här fungerar det {#how-it-works}

Utdata för webb och utskrift av autofyrningsbrevet skapas med hjälp av flerkanalsfunktionerna i interaktiv kommunikation.

Knappen Förnya nu i e-postmeddelandet är länkad till programmet Förnya automatiskt, som är en interaktiv kommunikation i en publiceringsinstans.

#### Se det själv {#see-it-yourself-1}

Du måste ha fått ett e-postmeddelande med en bifogad PDF. PDF är en tryckt version av ditt Autofyrke-brev. Klicka på **Förnya nu** för att nå webbversionen av profilen. Kontrollera din personliga information och principinformation och klicka på **Förnya nu** som tar dig till en annan interaktiv kommunikation.

Knappen **Förnya nu** i e-postmeddelandet dirigerar Sarah till principen på webben. Du kan gå till följande URL:

`https://[authorServer]:[authorPort]/content/document.html?schema=fdm&documentId=/content/forms/af/we-finance/autoinsurance/auto-insurance-renewal/channels/web.html&customerId=1`

Du kan kontrollera den detaljerade sammanfattningen av din automatiska försäkringsförnyelse och klicka på **Förnya nu** längst ned på sidan.

### Sarah kommer till betalningssidan {#sarah-reaches-the-payment-page}

På webbplatsen `We.Finance` kommer Sarah till betalningssidan. Sarah omkontrollerar sitt principnummer och utgångsdatum med sina register. Till höger på sidan kontrollerar Sarah betalningssammanfattningen för förnyelsen med en premiumrabatt på 10 % på det totala beloppet.

#### Så här fungerar det {#how-it-works-1}

Knappen Förnya nu dirigerar Sarah till betalningssidan. Betalningssidan är ett anpassningsbart formulär.

#### Se det själv {#see-it-yourself-2}

Klicka på **Förnya nu** för att nå betalningssidan. Fyll i kreditkortsinformationen och klicka på **Gör betalning**.

Du kan nå betalningssidan i utvecklingsinstansen på

`https://[authorServer]:[authorPort]/content/document.html?documentId=/content/forms/af/we-finance/credit-card/ccbillpayment.html&schema=fdm&customerId=1`

### Sarah gör betalningen och slutför processen {#sarah-makes-the-payment-and-completes-the-process}

Sarah fyller i sin kreditkortsinformation och klickar på **Gör betalning**.

#### Så här fungerar det {#how-it-works-2}

När Sarah fyller i kreditkortsinformationen och klickar på Submit (Skicka) bearbetas hennes kreditkortsbetalning och ett tackmeddelande som konfigurerats i det adaptiva formuläret visas på skärmen.

#### Se det själv {#see-it-yourself-3}

Du kan visa bekräftelsemeddelandet efter att du klickat på Gör betalning vid

`https://[authorServer]:[authorPort]/content/forms/af/we-finance/credit-card/ccbillpayment/jcr:content/guideContainer.guideThankYouPage.html?owner=admin&status=Submitted`
