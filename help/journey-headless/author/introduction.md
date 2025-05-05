---
title: Skapa för Headless med Adobe Experience Manager
description: En introduktion till de kraftfulla och flexibla headlessfunktionerna i Adobe Experience Manager och hur du skapar innehåll för ditt projekt.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments
role: Admin, Architect,Data Architect,Developer,User,Leader
exl-id: 4864d5e7-65e3-4309-9512-cde4a138e04c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 0%

---

# Om du skriver för Headless med AEM - en introduktion {#author-headless-introduction}

I den här delen av [AEM Headless Content Author Journey](overview.md) kan du lära dig de (grundläggande) begrepp och termer som behövs för att förstå hur du skapar innehåll för headless Content Delivery med Adobe Experience Manager (AEM).

## Syfte {#objective}

* **Målgrupp**: Nybörjare
* **Mål**: Introducera de koncept och termer som är relevanta för Headless Authoring.

## Content Management System (CMS) {#content-management-system}

Vad är ett innehållshanteringssystem?

Ett innehållshanteringssystem (CMS) är precis vad det sägs vara - ett datorsystem som används för att hantera innehåll. Det är lite allmänt, så för att vara mer exakt används det (vanligtvis) för att hantera innehåll som du vill göra tillgängligt på dina webbplatser.

## Headless CMS {#headless-cms}

Headless är en term som används för att beskriva system som effektivt skiljer innehållet från hur det visas på webben.

Traditionellt sett hanterar du materialet i CMS och samma CMS ansvarar för återgivningen av materialet på webbsidorna.

Nu innebär headless att ditt innehåll kan hanteras i CMS och sedan nås av ett eller flera (oberoende) program.

Det innebär att innehållet kan levereras till alla enheter i en mängd olika format. Detta gör hela processen mycket mer flexibel och innebär också att du inte behöver bekymra dig om layout och formatering.

>[!NOTE]
>
>Om du vill veta mer om den tekniska informationen om Headless CMS kan du läsa mer på Learn About CMS Headless Development.

## Adobe Experience Manager {#aem-cms}

Så vad är AEM?

Först och främst är AEM ett innehållshanteringssystem med många olika funktioner som också kan anpassas efter dina behov.

Detta innebär att det kan användas som

* Headless CMS
   * För headless kan ditt innehåll redigeras som **Innehållsfragment**.
Dessa är självständiga innehållsobjekt som kan nås direkt av ett antal program, eftersom de har en fördefinierad struktur som baseras på **modeller för innehållsfragment**.
Det innebär att innehållet kan nå ut till en mängd olika enheter, i en mängd olika format och med ett stort urval av funktioner.
(Och som en dubbelpanorering kan dessa fragment också användas när du skapar AEM webbsidor - om du vill.)

* &quot;Traditionell&quot; CMS
   * Innehåll skapas för webbsidor med hjälp av en rad komponenter som anger hur innehållet ska återges på webbplatsen. Även här är AEM extremt flexibelt eftersom projektteamet kan utveckla anpassade komponenter.

## Innehållsmodellering {#content-modeling}

Innehållsmodellering (även kallat datamodellering) är alltså en annan teknisk term - varför ska det intressera dig som författare?

För att de headless-appar ska kunna komma åt ditt innehåll och göra något med det måste innehållet verkligen ha en fördefinierad struktur. Det skulle vara möjligt att ha ditt innehåll som en ledig form, men det skulle göra livet *mycket* komplicerat för programmen.

Processen att definiera strukturen för ditt innehåll som ska följas inbegriper att utforma en modell - och detta kallas datamodellering.

För AEM utför rollen Innehållsarkitektur (ofta en annan person) datamodelleringen för att utforma ett intervall av **modeller för innehållsfragment** som du sedan använder som grund för ditt innehåll med **Innehållsfragment**.

>[!NOTE]
>
>Om du vill veta mer om datamodellering kan du läsa mer under AEM Headless Content Architect Journey.

## What&#39;s Next {#whats-next}

Nu när du har lärt dig koncept och terminologi är nästa steg att [Lär dig grunderna i att skapa innehållsfragment](basics.md). Då introduceras den grundläggande hanteringen av AEM tillsammans med hur du skapar innehållsfragment.

## Ytterligare resurser {#additional-resources}

* AEM Headless Developer Journey
   * [Läs om CMS Headless Development](/help/journey-headless/developer/learn-about.md)

* [AEM Headless Content Architect Journey](/help/journey-headless/architect/overview.md)

* [AEM Headless Content Translation Journey](/help/journey-headless/translation/overview.md)

* [Introduktion till AEM som Headless CMS](/help/sites-developing/headless/introduction.md)

* [AEM Developer Portal](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=sv-SE)

* [Självstudiekurser för Headless i AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=sv-SE)
