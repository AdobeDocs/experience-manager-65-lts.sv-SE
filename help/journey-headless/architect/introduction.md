---
title: Adobe Experience Manager Headless Content Architect Journey
description: En introduktion till de kraftfulla och flexibla headless-funktionerna i Adobe Experience Manager och hur du modellerar innehåll för ditt projekt.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments
role: Admin, Architect,Data Architect
exl-id: cb64e012-7001-47a3-b038-8f8f6891c6a0
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 0%

---

# Innehållsmodellering för Headless med AEM - en introduktion {#architect-headless-introduction}

I den här delen av [AEM Headless Content Architect Journey](overview.md) kan du lära dig de (grundläggande) begrepp och termer som behövs för att förstå innehållsmodellering för headless Content Delivery med Adobe Experience Manager (AEM).

Det här dokumentet hjälper er att förstå hur headless-innehåll levereras, hur AEM stöder headless och hur innehåll utformas för headless. När du har läst bör du:

* Förstå de grundläggande begreppen för leverans av headless-innehåll.
* Läs om hur AEM stöder headless och innehållsmodellering.

## Syfte {#objective}

* **Målgrupp**: Nybörjare
* **Mål**: Presentera de koncept och den terminologi som är relevanta för Headless Content Modeling.

## Leverans av högklassigt innehåll {#full-stack}

Ända sedan de lättanvända, storskaliga CMS:erna (Content Management System) började användas har organisationer använt dem som en central plats för att hantera meddelanden, varumärken och kommunikation. Genom att använda CMS som en central punkt för att administrera upplevelser blir ni effektivare eftersom ni slipper utföra samma uppgifter i olika system.

![Den klassiska högklassiga CMS](/help/journey-headless/developer/assets/full-stack.png)

I en CMS i full hög finns all funktionalitet för att hantera innehåll i CMS. Systemfunktionerna består av olika komponenter i CMS-stacken. Lösningen i full hög har många fördelar.

* Det finns ett system att underhålla.
* Innehållet hanteras centralt.
* Alla tjänster i systemet är integrerade.
* Det går smidigt att skapa innehåll.

Om en ny kanal behöver läggas till eller stöd för nya typer av upplevelser krävs, kan en (eller flera) ny komponent infogas i högen och det finns bara en plats att göra ändringar på.

![Lägger till en ny kanal i högen](/help/journey-headless/developer/assets/adding-channel.png)

Men komplexiteten i beroendena i högen blir snabbt tydlig eftersom andra objekt i högen måste justeras för att kunna anpassa ändringarna.

## Huvudet utan huvud {#the-head}

Huvudet för alla system är vanligtvis systemets utdatarenderare, vanligtvis i form av ett grafiskt gränssnitt eller andra grafiska utdata.

När vi talar om ett headless CMS hanterar CMS innehållet och fortsätter att leverera det till konsumenterna. Men om **content** endast levereras på ett standardiserat sätt utesluter ett headless CMS den slutliga utdatarenderingen, vilket innebär att **presentationen** av innehållet lämnas till den förbrukande tjänsten.

![Headless CMS](/help/journey-headless/developer/assets/headless-cms.png)

De konsumerande tjänsterna, oavsett om de är AR-upplevelser, en webshop, mobilupplevelser, progressiva webbappar (PWA) och så vidare, tar in innehåll från det headless CMS och erbjuder sin egen rendering. De ser till att kunna erbjuda sina egna huvuden för ert innehåll.

Om du utelämnar huvudet förenklas CMS genom att komplexiteten försvinner. När du gör det flyttas även ansvaret för att återge innehållet till de tjänster som faktiskt behöver innehållet och som ofta är bättre lämpade för sådan återgivning.

## Innehållsmodellering {#content-modeling}

Innehållsmodellering (även kallat datamodellering) är din specialitet, så vad behöver du tänka på när du modellerar för headless?

För att headless-programmen ska kunna komma åt ditt innehåll och göra något med det måste innehållet verkligen ha en fördefinierad struktur. Det skulle vara möjligt att ha ditt innehåll som en ledig form, men det skulle göra livet *mycket* komplicerat för programmen.

För AEM kommer du, som innehållsarkitekt, att göra innehållsmodelleringen för att utforma ett intervall av **modeller för innehållsfragment**. Dessa definierar den struktur som används när innehållsförfattarna skapar de **innehållsfragment** som innehåller innehållet.

### Åtkomst till innehållet {#access-content}

Det här är mer av en detaljfråga - men det kan intressera dig, bara för att slutföra artikeln.

När du har skapat modellerna för innehållsfragment, och dina författare har använt dem för att generera innehållet, måste de headless-program ha tillgång till det här innehållet.

Adobe Experience Manager (AEM) kan selektivt komma åt dina innehållsfragment med AEM GraphQL API, och returnera endast det innehåll som behövs. Med API:t kan en utvecklare formulera frågor som markerar specifikt innehåll. Den här urvalsprocessen baseras på *dina* modeller för innehållsfragment.

Detta innebär att projektet kan leverera strukturerat material utan extra kostnad för användning i dina program.

## What&#39;s Next {#whats-next}

Nu när du har lärt dig koncept och terminologi är nästa steg att [Lär dig grunderna i modellering med Content Fragment Models](basics.md).

## Ytterligare resurser {#additional-resources}

* AEM Headless Developer Journey
   * [Läs om CMS Headless Development](/help/journey-headless/developer/learn-about.md)
   * [Lär dig modellera ditt innehåll](/help/journey-headless/developer/model-your-content.md)
* [Introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)
* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)
* [Självstudiekurser för Headless i AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE)
