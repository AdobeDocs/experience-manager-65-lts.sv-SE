---
title: Vi.Gov:s referenswebbplats FOIA genomgång
description: Se genomgången av webbsajten We.Gov så att du kan förstå hur AEM Forms hjälper myndigheter att ta emot och skicka ut information som efterfrågats av privatpersoner enligt lagen om informationsfrihet.
topic-tags: introduction
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Foundation Components
role: Admin, User, Developer
exl-id: a2f79634-6eca-479a-89d7-e1ef2e4a6e6d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 0%

---

# Vi.Gov:s referenswebbplats FOIA genomgång {#we-gov-reference-site-foia-walkthrough}

## Scenario för lagen om informationsfrihet för webbplatser {#reference-site-freedom-of-information-act-scenario}

We.Gov är en statlig organisation som låter adoptivföräldrar registrera sig för barnsupport om de använder ett barn. We.Gov tillåter också föräldrar att begära information från följande myndigheter inom ramen för lagen om informationsfrihet:

* Försvarets logistikbyrå
* Department of Defense Office of Inspector General
* Department of Justice - Office of Information Policy
* Avdelningen för flottan
* Naturvårdsverket

Mer information om lagen om informationsfrihet finns på [https://www.foia.gov/](https://www.foia.gov).

Scenariot omfattar följande personligheter:

* Sarah Rose, den person som begär information under
* John Jacobs, den person som hanterar begäran vidarebefordrar den till rätt avdelning
* Gloria Rios, en statstjänsteman som lämnar uppgifter i enlighet med begäran

## Sarah initierar en begäran om information under FOIA {#sarah-initiates-request-for-information-under-foia}

Enligt lagen om informationsfrihet begär Sarah en kopia av Handboken Administration for Children and Families (FY) 2013 till 2016. Sarah lämnar in denna begäran till justitiedepartementet - Office of Information Policy och menar också att hon kan betala upp till 100 USD för utskrifts- och portokostnader.

### Så här fungerar det {#how-it-works}

### Se det själv {#see-it-yourself}

Öppna `https://<hostname>:<PublishPort>/wegov` i webbläsaren. Välj Program > Alla program på webbplatsen Web.Gov. På sidan Alla program väljer du Använd under Program för FOIA-begäran.

## Sarah börjar sin ansökan om information under FOIA {#sarah-starts-her-application-for-information-under-foia}

Sarah klickar på **Använd** och på sidan om att begära information anger Sarah bland annat följande:

* **Byrå:** Sarah anger vilken myndighet som förfrågan adresserades till som justitiedepartementet - informationspolicyn.

* **Betala upp till**: Sarah anger att hon är beredd att betala upp till 100 USD för utskrifts- och portokostnader.
* **Beskriv begäran i detalj**: Sarah anger&quot;Requesting copy of the Administration for Children and Families case logs for Fiscal years 2013 through 2016&quot;.

![Begär en kopia av ärendeloggarna Administration för barn och familjer för räkenskapsåren 2013 till 2016](assets/sarahfiosform.png)

Begär kopia av ärendeloggen Administration for Children and Families för räkenskapsåren 2013 till 2016

Sarah kan när som helst välja **Spara** för att spara ett utkast av formuläret och sedan gå tillbaka till det för att fylla i formuläret och skicka det. Sarah skickar formuläret.

>[!NOTE]
>
>Arbetsflödet från e-post fungerar endast med inloggade användare. I scenariot för referensplatsen ser du till att användaren Sarah Rose läggs till. Sarah inloggningsuppgifter är `srose/password`.

## John Jacobs tar emot och godkänner ansökan {#john-jacobs-receives-and-approves-the-application}

John Jacobs tar emot begäran och skickar den till rätt person. Med AEM Inbox kan John se alla inskickade program på ett och samma ställe.

### Så här fungerar det {#how-it-works-1}

När Sarah fyller i och skickar in FOIA-programmet skickas en anmälan till John Jacobs inkorg. John Jacobs kan visa den inskickade ansökan och godkänna eller avvisa den.

### Se det själv {#see-it-yourself-1}

Du kommer åt AEM Inbox på https://&lt;***värdnamn***>:&lt;***PublishPort***>/content/we-finance/global/en/login.html?resource=/aem/inbox.html. Logga in på AEM Inbox med jjacobs/password som användarnamn/lösenord för John Jacobs och se FOIA-programmet. Information om hur du använder AEM Inbox för formulärbaserade arbetsflödesuppgifter finns i [Hantera Forms-program och -uppgifter i AEM Inbox](/help/forms/using/manage-applications-inbox.md).

![johnjacobs](assets/johnjacobs.png)

John Jacobs kan se, godkänna eller avvisa programmet från programkontrollpanelen. John Jacobs väljer och öppnar informationen och godkänner den efter att ha granskat begäran.

![johnjacsticker-detail-1](assets/johnjacobstaskdetail-1.png)

### <strong>Sarah får ett bekräftelsemeddelande via e-post</strong> {#strong-sarah-receives-an-acknowledgement-email-strong}

När John Jacobs har godkänt ansökan får Sarah ett bekräftelsemeddelande från webbsidan We.Gov. Sarah informeras om de avgifter och den tid som krävs för att behandla hennes ansökan. E-postmeddelandet innehåller även e-post- och telefoninformation som Sarah kan kontakta för att få uppdateringar om sin app.

![sarahroseemail](assets/sarahroseemail.png)

## Gloria får FOIA-begäran om godkännande på andra nivån {#gloria-receives-the-foia-request-for-second-level-approval}

När John Jacobs fyllt i den information som krävs och godkänt Sarah&#39;s begäran går det till Gloria Rios för slutligt godkännande. Gloria granskar det bifogade dokumentet och godkänner begäran.

![gloriariosinbox](assets/gloriariosinbox.png)

### Så här fungerar det {#how-it-works-2}

När John Jacobs godkänner FOIA-begäran skapas en PDF eller Document of Record-fil av programmet och skickas till Gloria Rios inkorg. Gloria kan visa den inskickade förfrågan och godkänna eller avvisa den.

### Se själv {#see-for-yourself}

Du kommer åt AEM Inbox på https://&lt;***värdnamn***>:&lt;***PublishPort***>/content/we-finance/global/en/login.html?resource=/aem/inbox.html. Logga in på AEM Inbox med grios/password som användarnamn/lösenord för Gloria Rios och se FOIS-begäran.

Gloria öppnar förfrågan och undersöker informationen i FOIA-begäran. Efter att ha granskat detaljerna i begäran och kontrollerat om det går att skicka in de begärda dokumenten, godkänner Gloria begäran.

![gloriarioapproves](assets/gloriariosapproves.png)

## Sarah får ett meddelande om att hennes begäran har godkänts {#sarah-receives-notification-that-her-request-is-approved}

När Gloria har godkänt FOIA-begäran får Sarah ett e-postmeddelande som meddelar henne att hennes ansökan har godkänts. E-postmeddelandet innehåller även information om den preliminära tidslinjen för att skicka dokumentet och kontaktuppgifter för uppföljning av begäran.

![sarahrosee-postgodkännande](assets/sarahroseemailapproval.png)
