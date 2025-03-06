---
title: Lär dig grunderna i redigering
description: Lär dig mer om hur du skapar innehåll för Headless CMS med hjälp av Content Fragments.
solution: Experience Manager, Experience Manager Sites
feature: Headless,Content Fragments
role: Admin, Architect,Data Architect,Developer,User,Leader
exl-id: 20ff7c83-0882-454e-a8f5-9eda1724cfe3
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 0%

---

# Grundläggande om redigering för headless med AEM {#author-headless-basics}

## Story hittills {#story-so-far}

I början av [AEM Headless Content Author Journey](overview.md) innehöll [Introduktion](introduction.md) grundläggande begrepp och terminologi som är relevanta för redigering utan rubrik.

Den här artikeln bygger vidare på detta så att du förstår hur du skapar ditt eget innehåll för AEM headless-projekt.

## Syfte {#objective}

* **Målgrupp**: Nybörjare
* **Mål**: Introducera grunderna för Headless CMS Authoring:
   * Introduktion till utveckling med AEMaaCS
   * Introduktion till innehållsfragment

## Grundläggande hantering {#basic-handling}

Innan du får grepp om innehållsfragment är det här en snabb introduktion till AEM...men ingenting ersätter faktiskt upplevelsen av att logga in och försöka använda systemet.

### Skapa och publicera {#author-preview-publish}

En AEM-installation består vanligtvis av minst två miljöer:

* Författare
* Publicera

Du loggar in på och använder redigeringsmiljön för att generera ditt innehåll. När det är klart publicerar du sedan innehållet så att det blir allmänt tillgängligt. För hemlösa är detta för andra program, för webbsidor är det för läsare på webben.

Mer information finns i Authoring Concepts.

### Loggar in {#signing-in}

Precis som med de flesta system måste du logga in. Som författare får du:

* Användarnamn (konto)
* Lösenord
* Länk till inloggningsskärmen

Ditt konto har konfigurerats med de privilegier du behöver. Om du har problem rekommenderar Adobe att du kontaktar ditt interna projektsupportteam.

### Navigering {#navigation}

Första gången du loggar in i en liten onlinekurs kommer några av huvudfunktionerna i användargränssnittet att markeras.

Du kan sedan använda navigeringspanelen för att komma åt nyckelområden i AEM. För innehållsfragment använder du **Assets Console**.

Du kan öppna navigeringspanelen genom att välja Adobe-ikonen längst upp till vänster, följt av den lilla kompassikonen:

![Navigeringspanelen](/help/journey-headless/author/assets/headless-journey-author-navigation-01.png)

>[!NOTE]
>Även om innehållsfragment är en funktion i AEM **Sites** finns de i **Assets**-konsolen. Detta är en teknisk detalj som inte bör påverka dig, men som kan vara användbar att känna till.

I konsolen kan du välja mappar för att navigera till ditt innehållsfragment, eller de synliga mapparna (i sidhuvudet) för att gå tillbaka till trädet.

![Brödkrumber](/help/journey-headless/author/assets/headless-journey-author-navigation-02.png)

### funktionsmakron, markera, visa {#actions-selecting-viewing}

**Assets**-konsolen har dedikerade **åtgärdsverktygsfält** och **snabbåtgärder** som du kan använda när du har valt en resurs (till exempel en mapp eller ett innehållsavdrag).

Snabbåtgärderna är tillgängliga för en enskild resurs, se **Basel** i exemplet nedan:

![Snabbåtgärder](/help/journey-headless/author/assets/headless-journey-author-navigation-05.png)

Verktygsfältet Åtgärder ger tillgång till alla de åtgärder som gäller för det aktuella scenariot. Vilka åtgärder som är tillgängliga kan ändras, till exempel beroende på var du befinner dig eller om du har valt flera resurser:

![Åtgärdsverktygsfältet](/help/journey-headless/author/assets/headless-journey-author-navigation-06.png)

Du kan välja format för att visa dina resurser med vyväljaren:

![Visa väljare](/help/journey-headless/author/assets/headless-journey-author-navigation-03.png)

Du kan visa ytterligare information om objekt med hjälp av Rail Selector. Detta ger även tillgång till ytterligare åtgärder.

![Vänster linje](/help/journey-headless/author/assets/headless-journey-author-navigation-04.png)

## Skapa innehållsfragment {#authoring-content-fragments}

Det var en mycket snabb introduktion till AEM användargränssnitt, men förhoppningsvis har du haft en chans att prova det. Nu kommer vi till ditt verkliga intresse - innehållsfragment för Headless.

Vi måste gå igenom allt från början till slut, men din instans kanske redan har mappar och/eller fragment skapade, och dessa kan finnas på olika platser. Principerna är desamma.

### Ordna och navigera {#organizing-and-navigating}

Om du inte har ett fåtal innehållsfragment vill du ordna dem så att du (och andra) kan hitta dem igen.

#### Skapa en mapp {#creating-folder}

Du kan göra detta genom att skapa en serie mappar i avsnittet **Filer** i Assets-konsolen. Välj alternativet **Skapa** (överst till höger) följt av **Mapp**:

![Alternativet Skapa mapp](/help/journey-headless/author/assets/headless-journey-author-folder-01.png)

En dialogruta öppnas där du kan ange informationen och sedan bekräfta med **Skapa**:

![Dialogrutan Skapa mapp](/help/journey-headless/author/assets/headless-journey-author-folder-02.png)

#### Använda banor och taggar för att begränsa antalet modeller för innehållsfragment i mappen {#tags-paths-for-models-in-folder}

Det här avsnittet är något mer avancerat. Du behöver det inte riktigt om du bara börjar och försöker saker, men det är mest användbart när du har många fragment. Så det är bra att veta om - även om du inte använder det helt än.

Din innehållsarkitekt har skapat alla innehållsfragmentmodeller som krävs för ditt aktuella projekt, och kanske även några andra projekt. För att göra det enklare för dig själv och andra författare kan du begränsa listan med modeller som är tillgängliga för en viss mapp.

När du har skapat mappen kan du öppna mappen **Egenskaper**. Här finns olika flikar med information och konfigurationsinformation om mappen. Särskilt för innehållsfragment kan du använda fliken **Profiler** för att definiera specifika sökvägar och/eller taggar för den här mappen. Detta begränsar vilka modeller för innehållsfragment som är tillgängliga för användning i mappen, eftersom det innebär att modeller för innehållsfragment måste uppfylla dessa krav innan de kan användas för att generera fragment i den här mappen.

![Skapa mappegenskaper - principer](/help/journey-headless/author/assets/headless-journey-author-folder-04.png)

>[!NOTE]
>
>Du kan läsa mer under Modeller för innehållsfragment - Tillåt modeller för innehållsfragment i din Assets-mapp.

Du navigerar sedan genom de här mapparna för att skapa och redigera dina innehållsfragment.

#### Precis in case - Folder Cloud Services Configuration {#cloud-services-folder}

Bara om..

Du kommer antagligen att få en inledande mapp där du kan skapa dina mappar. Detta beror på att viss konfigurationsinformation måste tillämpas (vanligtvis av en utvecklare eller systemadministratör) på rotmappen. Detta är antagligen inget intresse för dig, men om det behövs kan du kontrollera **konfigurationen** i **molntjänsterna** i mappen **Egenskaper**:

![Skapa mappegenskaper - Konfiguration](/help/journey-headless/author/assets/headless-journey-author-folder-03.png)

>[!NOTE]
>
>Du kan läsa mer under Använd konfigurationen i din Assets-mapp.

### Skapa ett innehållsfragment {#creating-fragment}

Att skapa ett innehållsfragment liknar mycket - du använder bara alternativet **Innehållsfragment** i stället:

![Alternativet Skapa innehållsfragment](/help/journey-headless/author/assets/headless-journey-author-content-fragment-01.png)

Den här gången öppnas en guide. Det första steget är att välja den innehållsfragmentmodell som fragmentet ska baseras på:

![Skapa innehållsfragment - välj modell](/help/journey-headless/author/assets/headless-journey-author-content-fragment-02.png)

När du har gått vidare med **Nästa** kan du ange information (**Grundläggande** och **Avancerat**) för ditt fragment:

![Skapa innehållsfragment - ange namn](/help/journey-headless/author/assets/headless-journey-author-content-fragment-03.png)

Bekräfta med **Skapa** så kan du sedan **Öppna** ditt fragment i redigeraren.

### Redigera ett fragment {#editing-fragment}

Du kan öppna ett fragment omedelbart efter att du har skapat det, eller genom att välja det från Assets-konsolen.

När redigeraren öppnas ser du:

* En lista med ikoner till vänster - det ger dig tillgång till olika funktionsområden. Redigeraren öppnas på fliken **Variationer** och det är här som det mesta av redigeringen görs. Du kan också vara intresserad av flikarna **Anteckningar** och **Metadata** .

* En rubrik med information om fragmentet och åtkomst till olika åtgärder.

* Huvudområdet för redigering - det beror på vilken modell som används för att skapa fragmentet.

Exempel:

* Ett fragment som bara kräver flera informationsdelar, vissa med en viss typ. För headless-innehåll är referenser viktiga och du kommer att lära dig mer om dem senare under din resa.

  ![Innehållsfragmentredigeraren - Mitt fragment](/help/journey-headless/author/assets/headless-journey-author-content-fragment-04.png)

* Ett fragment som gör att du kan skriva ett långt textavsnitt. Här finns ytterligare alternativ för att hantera och formatera texten. Du kan även öppna enskilda textfält i en helskärmsredigerare (med den lilla skärmliknande ikonen till höger)

  ![Content Fragment Editor - Alaska Spirits](/help/journey-headless/author/assets/headless-journey-author-content-fragment-05.png)

>[!NOTE]
>
>Projektspecifik dokumentation kan behövas för att hjälpa författare med detaljer om hur vissa fält kan fyllas i.
>
>Se Modeller för innehållsfragment - Datatyper och egenskaper för allmän information.

Bekräfta uppdateringarna med **Spara** eller **Spara och stäng**.

>[!NOTE]
>
>Mer information finns i Variationer - Skapa innehållsfragment.

#### Det du (antagligen) inte behöver oroa dig för {#what-you-probably-do-not-need-to-worry-about}

OK, det här kan verka lite konstigt, men när du väl har öppnat Content Fragment Editor och börjat utforska ser du olika alternativ som (förmodligen) inte gäller för din resa som innehållsförfattare. Så det här är bara en snabbtitt på vad du bör ignorera i det headlesssammanhanget:

* **Modeller för innehållsfragment**

  Namnet på innehållsfragmentmodellen visas längst upp i redigeraren, direkt under fragmentnamnet. Det här är också en länk som tar dig till modellredigeraren.
Modeller för innehållsfragment är faktiskt viktiga för dina innehållsfragment när de definierar den struktur som du använder. Men att skapa och redigera dem är (vanligtvis) en annan persons ansvar, innehållsarkitekten.

  >[!NOTE]
  >
  >Om du vill veta mer kan du läsa AEM Headless Content Architect Journey.

* **Associerat innehåll**

  Den här är ganska självklar eftersom den är en flik i redigeraren.

  Innehållsfragment har funnits i AEM i många olika versioner. Ursprungligen var de tillgängliga för&quot;traditionell&quot; användning vid redigering av sidor...och de används fortfarande i detta sammanhang. Detta kan innebära att resurser (till exempel bilder) som inte är inbäddade i fragmentet måste vara tillgängliga för författaren när sidan redigeras.

* **Förhandsgranska**

  Det här är en annan flik i redigeraren och ger en teknisk vy som främst är avsedd för utvecklare.

* **Uppdatera sidreferenser**

  Den här åtgärden är tillgänglig i listrutan **..** (ellipser). Det är inte intressant för skribenter utan rubrik när det gäller att skapa sidor.

### Publicering {#publishing}

<!-- needs more details -->

När du har slutfört fragmentet kan du **Publicera** det så att det blir tillgängligt för de Headless-program som använder det.

Publiceringsåtgärderna är tillgängliga i redigeraren (eller från verktygsfältet i **Assets** -konsolen):

![Innehållsfragmentredigeraren - Mitt fragment](/help/journey-headless/author/assets/headless-journey-author-content-fragment-06.png)

## What&#39;s Next {#whats-next}

Nu när du har lärt dig grunderna är nästa steg att [Lär dig mer om referenser](references.md). Här presenteras och diskuteras de olika referenser som finns tillgängliga och hur du skapar strukturnivåer med Fragment References - en viktig del i utvecklingen för headless.

## Ytterligare resurser {#additional-resources}

* [Authoring Concepts](/help/sites-authoring/author.md)

* [Grundläggande hantering](/help/sites-authoring/basic-handling.md) - Den här sidan är primärt baserad på konsolen **Platser**, men många/de flesta funktioner är också relevanta för att skapa **innehållsfragment** under konsolen **Assets**.

   * [Navigeringspanel](/help/sites-authoring/basic-handling.md#navigation-panel)

   * [Sidhuvudet](/help/sites-authoring/basic-handling.md#the-header)

   * [Åtgärdsverktygsfältet](/help/sites-authoring/basic-handling.md#actions-toolbar)

   * [Snabbåtgärder](/help/sites-authoring/basic-handling.md#quick-actions)

   * [Visa och välja resurser](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources)

   * [Järnvägsväljare](/help/sites-authoring/basic-handling.md#rail-selector)

* [Arbeta med innehållsfragment](/help/assets/content-fragments/content-fragments.md)

   * [Hantera innehållsfragment](/help/assets/content-fragments/content-fragments-managing.md)

      * [Använd konfigurationen i din Assets-mapp](/help/assets/content-fragments/content-fragments-configuration-browser.md#apply-the-configuration-to-your-assets-folder)

      * [Skapa ett innehållsfragment](/help/assets/content-fragments/content-fragments-managing.md#creating-a-content-fragment)

   * [Variationer - Skapa innehållsfragment](/help/assets/content-fragments/content-fragments-variations.md)

   * [Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md)

      * [Modeller för innehållsfragment - datatyper](/help/assets/content-fragments/content-fragments-models.md#data-types)

      * [Modeller för innehållsfragment - egenskaper](/help/assets/content-fragments/content-fragments-models.md#properties)

      * [Modeller för innehållsfragment - Tillåt modeller för innehållsfragment i din Assets-mapp](/help/assets/content-fragments/content-fragments-models.md#allowing-content-fragment-models-assets-folder)

* Komma igång-guider
   * [Skapa en startguide för Assets Folder Headless](/help/sites-developing/headless/getting-started/create-assets-folder.md)

* [AEM Headless Content Architect Journey](/help/journey-headless/architect/overview.md)

* [AEM Headless Translation Journey](/help/journey-headless/translation/overview.md)
