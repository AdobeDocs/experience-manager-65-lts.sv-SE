---
title: Bästa tillvägagångssätt för arbete med anpassningsbara formulär
description: Beskriver de bästa sätten att skapa ett AEM Forms-projekt, utveckla adaptiva formulär och optimera prestanda för AEM Forms-system.
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: b87629fa-85a9-4024-963a-4761bc093e62
source-git-commit: d0529c8bce32e192cbbc7686f14825df57762363
workflow-type: tm+mt
source-wordcount: '5535'
ht-degree: 0%

---

# Bästa tillvägagångssätt för arbete med anpassningsbara formulär {#best-practices-for-working-with-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/sv/docs/experience-manager-core-components/using/adaptive-forms/introduction) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Ökning {#overview}

Adobe Experience Manager (AEM)-formulär kan hjälpa er att omvandla komplexa transaktioner till enkla, roliga digitala upplevelser. Det kräver dock samordnade insatser för att implementera, bygga, genomföra och underhålla ett effektivt och produktivt AEM Forms-ekosystem.

Det här dokumentet innehåller riktlinjer och rekommendationer som formuläradministratörer, författare och utvecklare kan dra nytta av när de arbetar med AEM Forms, särskilt adaptiva formulärkomponenter. Här diskuteras de effektivaste strategierna, från att skapa ett formulärutvecklingsprojekt till att konfigurera, anpassa, skapa och optimera AEM Forms. Dessa bästa metoder bidrar tillsammans till AEM Forms ekosystems allmänna prestanda.

Här följer dessutom några rekommenderade artiklar om AEM allmänna bästa praxis:

* [God praxis: distribuera och underhålla AEM](/help/sites-deploying/best-practices.md)
* [God praxis: Skapa innehåll](/help/sites-authoring/best-practices.md)
* [God praxis: Administrera AEM](/help/sites-administering/administer-best-practices.md)
* [God praxis: Utveckla lösningar](/help/sites-developing/best-practices.md)

## Konfigurera och konfigurera AEM Forms {#set-up-and-configure-aem-forms}

### Konfigurera formulärutvecklingsprojekt {#setting-up-forms-development-project}

En förenklad och standardiserad projektstruktur kan avsevärt minska arbetet med utveckling och underhåll. Apache Maven är ett verktyg med öppen källkod som rekommenderas för att skapa AEM-projekt.

* Använd Apache Maven `aem-project-archetype` för att skapa och hantera struktur för AEM-projekt. Här skapas de rekommenderade strukturerna och mallarna för ditt AEM-projekt. Dessutom innehåller det system för automatisering och ändringskontroll som underlättar hanteringen av projektet.

   * Använd kommandot maven `archetype:generate` för att generera den inledande strukturen.
   * Använd kommandot maven `eclipse:eclipse` för att generera de förmörkade projektfilerna och importera projektet till förmörkning.

Mer information finns i [Så här skapar du AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md).

* Med FileVault-verktyget eller VLT kan du mappa innehållet i en CRX- eller AEM-instans till ditt filsystem. Här finns funktioner för ändringshantering, som in- och utcheckning av AEM-projektinnehåll. Se [Använda VLT-verktyget](/help/sites-developing/ht-vlttool.md).

* Om du använder en Eclipse-integrerad utvecklingsmiljö kan du använda AEM Developer-verktygen för smidig integrering av Eclipse IDE med AEM-instanser för att skapa AEM-program. Mer information finns i [AEM utvecklarverktyg för Eclipse](/help/sites-developing/aem-eclipse.md).

* Lagra inte något innehåll och gör inga ändringar i mappen /libs. Skapa övertäckningar i /app-mappar för att utöka eller skriva över standardfunktioner.

* När du skapar paket för att flytta innehåll måste du se till att paketfiltersökvägarna är korrekta och bara obligatoriska sökvägar anges.

* Lagra inte något innehåll och gör inga ändringar i mappen /libs. Skapa övertäckningar i /app-mappar för att utöka eller skriva över standardfunktioner.

* Definiera korrekta beroenden för paketen för att framtvinga en förbestämd installationsordning/sekvens.

* Skapa ingen referensnod i /libs eller /apps.

### Planering för redigeringsmiljö {#planning-for-authoring-environment}

När du väl har skapat ditt AEM-projekt kan du definiera en strategi för att skapa och anpassa adaptiva blankettmallar och komponenter.

* En adaptiv formulärmall är en specialiserad AEM-sida som definierar struktur och sidhuvud/sidfot-information i ett adaptivt formulär. En mall har förkonfigurerade layouter, format och grundstruktur för ett adaptivt formulär. AEM Forms innehåller färdiga mallar och komponenter som du kan använda för att skapa anpassningsbara formulär. Du kan dock skapa egna mallar och komponenter efter behov. Vi rekommenderar att du samlar in krav för ytterligare mallar och komponenter som du behöver i dina adaptiva formulär. Mer information finns i [Anpassa adaptiva formulär och komponenter](/help/forms/using/adaptive-forms-best-practices.md#customize-components).
* Vi rekommenderar att du överför formulärpaketen med användargränssnittet i Form Manager i stället för med användargränssnittet i CRX Package Manager, eftersom överföring av paket via CRX Package Manager ibland kan leda till avvikelser.
* Med AEM Forms kan du skapa anpassningsbara formulär baserade på följande formulärmodeller. Blankettmodellerna fungerar som gränssnitt för datautbyte mellan ett formulär och ett AEM-system och ger en XML-baserad struktur för dataflöden både innanför och utanför ett adaptivt formulär. Formulärmodellerna innehåller dessutom regler och begränsningar för anpassningsbara formulär i form av schema- och XFA-begränsningar.

   * **Inget**: Anpassningsbara formulär som skapas med det här alternativet använder inte någon formulärmodell. Data-XML som genereras från sådana formulär har en platt struktur med fält och motsvarande värden.
   * **XML- eller JSON-schema**: XML- och JSON-scheman representerar strukturen i vilken data produceras eller används av organisationens serversystem. Du kan koppla ett schema till ett anpassat formulär och använda dess element för att lägga till dynamiskt innehåll i det anpassningsbara formuläret. Elementen i schemat är tillgängliga på fliken Datamodellsobjekt i innehållsläsaren för att skapa adaptiva formulär. Du kan dra och släppa schemaelementen för att skapa formuläret.
   * **XFA-formulärmall**: Det är en idealisk formulärmodell om du har investeringar i XFA-baserade HTML5-formulär. Det är ett direkt sätt att konvertera XFA-baserade formulär till anpassningsbara formulär. Alla befintliga XFA-regler behålls i de tillhörande adaptiva formulären. De färdiga adaptiva formulären har stöd för XFA-konstruktioner, till exempel valideringar, händelser, egenskaper och mönster.
   * **Formulärdatamodell**: Det är en föredragen formulärmodell om du vill integrera backend-system som databaser, webbtjänster och AEM användarprofil för att fylla i adaptiva formulär i förväg och skriva inskickade formulärdata tillbaka i backend-systemen. Med en redigerare för formulärdatamodell kan du definiera och konfigurera enheter och tjänster i en formulärdatamodell som du kan använda för att skapa adaptiva formulär. Mer information finns i [AEM Forms-dataintegrering](/help/forms/using/data-integration.md).

Det är viktigt att du noga väljer den datamodell som inte bara passar dina behov utan också utökar dina befintliga investeringar i XFA- och XSD-resurser, om det finns några. Använd XSD-modellen för att skapa formulärmallar eftersom den genererade XML-filen innehåller data enligt XPATH som definieras av schemat. Att använda XSD-modell som standardval för formulärdatamodell är också till hjälp eftersom det frigör formulärdesignen från det bakomliggande system som bearbetar och förbrukar data och förbättrar formulärens prestanda på grund av en-till-en-mappning av formulärfält. Dessutom kan BindRef för fältet göras till XPATH för dess datavärde i XML.

Mer information finns i [Skapa ett anpassat formulär](/help/forms/using/creating-adaptive-form.md).

* Det finns några vanliga avsnitt i adaptiva formulär. Ni kan identifiera dem och definiera en strategi för att främja återanvändning av innehåll. Med adaptiva formulär kan du skapa fristående fragment och återanvända dem i olika formulär. Du kan också spara en panel i ett anpassat formulär som ett fragment. Alla ändringar i ett fragment återspeglas i alla associerade formulär. Det hjälper er att minska utvecklingstiden och säkerställa enhetlighet i alla formulär. Dessutom gör användningen av fragment att adaptiva formulär blir lätta, vilket ger en förbättrad redigeringsupplevelse, särskilt för stora formulär. Mer information finns i [Adaptiva formulärfragment](/help/forms/using/adaptive-form-fragments.md).

### Anpassa adaptiva formulär och komponenter {#customize-components}

* AEM Forms har färdiga anpassningsbara formulärmallar som du kan använda för att skapa anpassningsbara formulär. Du kan också skapa egna mallar. AEM innehåller statiska och redigerbara mallar.

   * Statiska mallar definieras och konfigureras av utvecklare.
   * Redigerbara mallar skapas av författare med hjälp av mallredigeraren. Med mallredigeraren kan du definiera en grundläggande struktur och ursprungligt innehåll i en mall. Alla ändringar i strukturlagret återspeglas i alla formulär som använder den mallen. Det ursprungliga innehållet kan innehålla förkonfigurerat tema, förifyllningstjänst, skicka-åtgärd och så vidare. Dessa inställningar kan dock ändras för ett formulär med formulärredigeraren. Mer information finns i [Adaptiva formulärmallar](/help/forms/using/template-editor.md).

* Om du vill formatera ett visst fält eller en viss panelinstans använder du [intern formatering](/help/forms/using/inline-style-adaptive-forms.md). Du kan också definiera en klass i en CSS-fil och ange klassnamnet i komponentens CSS-klassegenskap.
* Inkludera ett klientbibliotek i en komponent för att konsekvent tillämpa format på adaptiva formulär eller fragment som använder den komponenten. Mer information finns i [Skapa en adaptiv formulärsidkomponent](/help/forms/using/custom-adaptive-forms-templates.md).
* Använd format som har definierats i ett klientbibliotek för att välja adaptiva formulär genom att ange sökvägen till klientbiblioteket i fältet CSS-filsökväg i egenskaperna för den adaptiva formulärbehållaren.
* Om du vill skapa ett klientbibliotek med dina format kan du konfigurera den anpassade CSS-filen i Theme Editor-basklienten eller i egenskaperna för formulärbehållaren.
* Med adaptiva formulär kan du skapa panellayouter, som responsiva, tabbade, dragspel och guide, för att styra hur formulärkomponenter placeras ut på en panel. Du kan skapa anpassade panellayouter och göra dem tillgängliga för formulärförfattare. Mer information finns i [Skapa anpassade layoutkomponenter för adaptiva formulär](/help/forms/using/custom-layout-components-forms.md).
* Du kan också anpassa specifika adaptiva formulärkomponenter som fält och panellayout.

   * Använd funktionen [Övertäckning](/help/sites-developing/overlays.md) i AEM för att ändra en kopia av en komponent. Du bör inte ändra standardkomponenter.
   * Om du vill anpassa layouten för anpassade formulärkomponenter i /libs [skapar du anpassade layoutkomponenter](/help/forms/using/custom-layout-components-forms.md) utöver [standardlayouterna](/help/forms/using/layout-capabilities-adaptive-forms.md).
   * Lägg in anpassade interaktiva funktioner genom att skapa anpassade widgetar eller utseenden. Du bör inte ändra standardkomponenter. Mer information finns i [Utseenderamverket](/help/forms/using/introduction-widgets.md).

* Se [Hantera personligt identifierbar information](/help/forms/using/adaptive-forms-best-practices.md#p-handling-personally-identifiable-information-p) för rekommendationer om hur du hanterar PII-data.

### Skapa formulärmallar

Du kan skapa ett anpassat formulär med formulärmallarna som är aktiverade i **Konfigurationsläsaren**. Information om hur du aktiverar formulärmallarna finns i [Skapa anpassad formulärmall](https://experienceleague.adobe.com/sv/docs/experience-manager-learn/forms/creating-your-first-adaptive-form/create-adaptive-form-template).

Formulärmallarna kan också laddas upp från adaptiva formulärpaket som skapats på en annan författardator. Formulärmallar är tillgängliga genom att installera [aemforms-references-*-paket](https://experienceleague.adobe.com/sv/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases). Några av de bästa metoderna som rekommenderas är:

* Körningsläget **nosample content** rekommenderas endast för författaren och inte för publiceringsnoderna.
* Redigering av resurser som anpassningsbara formulär, teman, mallar eller molnkonfigurationer utförs endast via redigeringsnoder, som kan publiceras på de konfigurerade publiceringsnoderna.
Mer information finns i [Publicera och avpublicera formulär och dokument](/help/forms/using/publishing-unpublishing-forms.md)
* Forms-tilläggspaket krävs för redigering och publicering för att dokumenttjänståtgärderna ska kunna hanteras. Det kan därför anses vara ett beroende.
Om du bara vill ha Forms-relaterade exempelmallar, teman och DOR-paket kan du hämta dem från [aemforms-references-*-paket](/help/forms/using/upgrade-forms-osgi.md).

Mer information finns i [Introduktion till redigering av adaptiva formulär](/help/forms/using/introduction-forms-authoring.md).

## Skapa anpassningsbara formulär {#author-adaptive-forms}

### Använda pekoptimerat användargränssnitt för redigering {#using-touch-optimized-ui-for-authoring}

* Använd objektwebbläsaren i sidlisten för att snabbt komma åt fält som är djupa i formulärhierarkin. Du kan använda sökrutan för att söka efter objekt i formulär- eller objektträdet och navigera mellan olika objekt.
* Om du vill visa och redigera egenskaperna för en komponent i komponentwebbläsaren i sidofältet markerar du komponenten och klickar på ![cmpr-1](assets/cmppr-1.png). Du kan också dubbelklicka på en komponent om du vill visa dess egenskaper i egenskapsläsaren.
* Använd kortkommandon för att vidta snabba åtgärder i formulären. Se [AEM Forms-kortkommandon](/help/forms/using/keyboard-shortcuts.md).

* Adaptiva formulärkomponenter rekommenderas för användning endast i adaptiva formulärsidor. Komponenterna är beroende av sin överordnade hierarki. Använd dem därför inte på en AEM-sida.

Se även komponentbeskrivningar och metodtips i [Introduktion till utveckling av adaptiva formulär](/help/forms/using/introduction-forms-authoring.md).

### Använda regler i anpassningsbara formulär {#using-rules-in-adaptive-forms}

AEM Forms tillhandahåller en [regelredigerare](/help/forms/using/rule-editor.md) som gör att du kan skapa regler för att lägga till dynamiskt beteende i adaptiva formulärkomponenter. Med dessa regler kan du utvärdera villkor och utlösa åtgärder för komponenter, som att visa eller dölja fält, beräkna värden, ändra listrutan dynamiskt och så vidare.

Regelredigeraren innehåller en visuell redigerare och en kodredigerare för att skriva regler. Tänk på följande när du skriver regler i kodredigeringsläget:

* Använd meningsfulla och unika namn för formulärfält och komponenter för att undvika eventuella konflikter när du skriver regler.
* Använd operatorn `this` för en komponent för att referera till sig själv i ett regeluttryck. Det ser till att regeln förblir giltig även om komponentnamnet ändras. Exempel: `field1.valueCommit script: this.value > 10`.

* Använd komponentnamn när du refererar till andra formulärkomponenter. Använd egenskapen `value` för att hämta värdet för ett fält eller en komponent. Exempel: `field1.value`.

* Referera komponenter efter en relativ unik hierarki för att undvika konflikter. Exempel: `parentName.fieldName`.

* När du hanterar komplexa eller ofta använda regler bör du överväga att skriva affärslogik som funktioner i ett separat klientbibliotek som du kan ange och återanvända i adaptiva formulär. Klientbiblioteket ska vara ett självständigt bibliotek och inte ha några externa beroenden, förutom jQuery och Underscore.js. Du kan också använda klientbiblioteket för att framtvinga [omvalidering på serversidan](/help/forms/using/configuring-submit-actions.md#server-side-revalidation-in-adaptive-form) av skickade formulärdata.
* Adaptiva formulär innehåller en uppsättning API:er som du kan använda för att kommunicera med och utföra åtgärder på adaptiva formulär. Några av de viktigaste API:erna är följande. Mer information finns i [API-referens för JavaScript-bibliotek för Adaptiv Forms](https://experienceleague.adobe.com/sv/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions).

   * `guideBridge.reset()`: Återställer ett formulär.
   * `guideBridge.submit()`: Skickar ett formulär.
   * `guideBridge.setFocus(somExp, focusOption, runCompletionExp)`: Anger fokus till ett fält.
   * `guideBridge.validate(errorList, somExpression, focus)`: Validerar ett formulär.
   * `guideBridge.getDataXML(options)`: Hämtar formulärdata som XML.
   * `guideBridge.resolveNode(somExpression)`: Hämtar ett formulärobjekt.
   * `guideBridge.setProperty(somList, propertyName, valueList)`: Anger egenskapen för ett formulärobjekt.
   * Du kan dessutom använda följande fältegenskaper:

      * `field.value` om du vill ändra värdet för ett fält.
      * `field.enabled` om du vill aktivera/inaktivera ett fält.
      * `field.visible` om du vill ändra synlighet för ett fält.

* Den som skapar anpassade blanketter kan behöva skriva JavaScript-kod för att skapa logiska funktioner i blanketter. Även om JavaScript är kraftfullt och effektivt är det troligt att det kan äventyra säkerhetsförväntningarna. Därför måste du se till att formulärförfattaren är en tillförlitlig person och att det finns processer för att granska och godkänna JavaScript-koden innan ett formulär börjar användas. Administratören kan begränsa åtkomsten till regelredigeraren till användargrupper baserat på deras roll eller funktion. Se [Bevilja regelredigeringsåtkomst för valda användargrupper](/help/forms/using/rule-editor-access-user-groups.md).
* Du kan använda uttryck i regler för att göra adaptiva formulär dynamiska. Alla uttryck är giltiga JavaScript-uttryck och använder API:er för adaptiva formulär. Dessa uttryck returnerar värden av vissa typer. Mer information om uttryck och metodtips runt dem finns i [Adaptiva formuläruttryck](/help/forms/using/adaptive-form-expressions.md).

* Adobe rekommenderar att du använder JavaScript synkrona åtgärder över asynkrona när du skapar regler med regelredigeraren. Asynkrona åtgärder bör inte användas. Om du befinner dig i en situation där asynkrona åtgärder inte kan undvikas är det viktigt att du implementerar JavaScript Closure-funktioner. På så sätt kan ni effektivt skydda er mot alla tänkbara konkurrensförhållanden och säkerställa att regelimplementeringarna ger optimala prestanda och upprätthåller stabilitet genom hela processen.

  Låt oss anta att vi behöver hämta data från ett externt API och sedan tillämpa några regler som baseras på dessa data. Vi använder en stängning för att hantera det asynkrona API-anropet och se till att reglerna tillämpas efter att data har hämtats. Här är exempelkoden:

  ```JavaScript
       function fetchDataFromAPI(apiEndpoint, callback) {
        // Simulate asynchronous API call with setTimeout
        setTimeout(() => {
          // Assuming the API call is successful, we receive some data
          const data = {
            someValue: 42,
          };
          // Invoke the callback with the fetched data
          callback(data);
        }, 2000); // Simulate a 2-second delay for the API call
      }
      // Rule implementation using Closure
      function ruleImplementation(apiEndpoint) {
        // Using a closure to handle the asynchronous API call and rule application
        // say you have set this value in street field inside address panel
        var streetField = address.street;
        fetchDataFromAPI(apiEndpoint, (data) => {
          streetField.value = data.someValue;
        });
      }
      // Example usage of the rule implementation
      const apiEndpoint = "https://example-api.com/data";
      ruleImplementation(apiEndpoint);
  ```

  I det här exemplet simulerar `fetchDataFromAPI` ett asynkront API-anrop med `setTimeout`. När data har hämtats anropas den angivna callback-funktionen, som är stängningen för att hantera det efterföljande regelprogrammet. Funktionen `ruleImplementation` innehåller regellogiken.


### Arbeta med teman {#working-with-themes}

Med anpassningsbara teman kan du skapa återanvändbara format som kan användas i olika formulär för enhetlig utformning och formatering. Använd teman för att definiera format för formulärkomponenter och paneler. Här följer några tips om teman:

* Använd resursbiblioteket för snabb användning av textformat, bakgrund och bilder. När ett format läggs till i resursbiblioteket är det tillgängligt för andra teman och i formredigerarens formatläge.
* Använd globala inställningar som teckensnitt och sidbakgrund med sidnivåväljaren.
* Använd klientbibliotek för att importera befintlig eller avancerad formatering till dina teman.
* Du kan åsidosätta formateringen för specifika fält, paneler eller knappar i ett formulärformatslager.
* Om ett tema inte uppfyller dina formatkrav kan du använda fördefinierade klasser som guideFieldNode, guideFieldLabel, guideFieldWidget och guidePanelNode för att använda samma format i alla formulär.

Mer information finns i [Teman](/help/forms/using/themes.md).

### Optimera prestanda för stora och komplexa formulär {#optimizing-performance-of-large-and-complex-forms}

Formulärförfattare och slutanvändare har oftast prestandaproblem när de läser in stora formulär i redigeringsläge eller vid körning. När antalet objekt (fält och paneler) i formuläret ökar försämras upplevelsen av redigering och körning. Det förhindrar också att flera författare samarbetar och skapar ett formulär samtidigt.

Använd följande metodtips för att lösa prestandaproblem med stora formulär:

* Vi rekommenderar att du skapar anpassningsbara formulär med XSD-formulärdatamodell, även när du konverterar en XFA till anpassat formulär, om det är möjligt.
* Inkludera endast de fält och paneler i anpassningsbara formulär som hämtar information från användaren. Tänk på att behålla statiskt innehåll så lite som möjligt eller använd URL:er för att öppna dem i ett separat fönster.
* Alla formulär har utformats för ett specifikt ändamål, men det finns några vanliga segment i de flesta formulär. Exempel: personuppgifter, adress, anställningsinformation osv. Skapa [anpassningsbara formulärfragment](/help/forms/using/adaptive-form-fragments.md) för vanliga formulärelement och avsnitt och använd dem i olika formulär. Du kan också spara en panel i ett befintligt formulär som ett fragment. Alla ändringar i ett fragment återspeglas i alla tillhörande adaptiva former. Det främjar samarbete när flera författare kan arbeta samtidigt i olika fragment som utgör ett formulär.

   * Precis som adaptiva formulär rekommenderar vi att alla fragmentspecifika format och anpassade skript definieras i klientbiblioteket med hjälp av dialogrutan fragmentbehållare. Försök också att skapa egna fragment som inte är beroende av objekt utanför den.
   * Undvik att använda korsfragmentsskript. Om det finns något objekt utanför fragmentet som du måste referera till, kan du försöka göra det objektet till en del av det överordnade formuläret. Om objektet fortfarande måste finnas i ett annat fragment refererar du till det med namnet i skriptet.

* Använd Spara och fortsätt med autosparande för att spara det anpassade formuläret regelbundet och göra det möjligt för användare att gå tillbaka senare för att fylla i formuläret.
* Konfigurera fragment för att läsa in lager. Vid körning återges fragment som markerats för att läsa in lager endast när de behövs. Det minskar inläsningstiden avsevärt för stora formulär. Den stöds även i fragment med repeterbara paneler. Mer information finns i [Konfigurera lazy loading](/help/forms/using/lazy-loading-adaptive-forms.md).

   * Konfigurera inte lazy loading på fragment i en responsiv rutnätslayout eller i den första panelen.
   * Komponenter för bifogade filer och villkor stöds inte i lagerinlästa fragment.
   * Markera ett värde i en lat inläst panel som Använd värdet globalt om det värdet används i någon annan del av formuläret, så att värdet är tillgängligt för användning när behållarpanelen tas bort.
   * Överväg att skriva synlighetsregler för fragment som ska visas eller döljas baserat på ett villkor.
* Ange värdet för **antalet anrop per begäran** i **huvudservern för Apache Sling** till ett ganska stort tal. Det gör att Forms-servern kan tillåta ytterligare samtal. Konfigurationen visar standardvärdet 1500. Värdet, 1 500 anrop, avser andra Experience Manager-komponenter som Sites och Assets. Standardvärdet för adaptiva formulär är 20000. Om du råkar ut för felet `too many calls` i loggar eller om formuläret inte kan återges kan du försöka åtgärda problemet genom att öka värdet till ett stort antal. Om antalet anrop överstiger 20000 innebär det att formuläret är komplext och det kan ta en stund att återge formuläret i webbläsaren. Det här händer bara första gången som formuläret läses in, efter att formuläret har cache-lagrats och när formuläret har cache-lagrats påverkas inte prestanda i någon större utsträckning.

### Förifyllning av anpassningsbara formulär {#prefilling-adaptive-forms}

Du kan förifylla anpassningsbara formulärfält med data som hämtats från backend-sidan för att hjälpa användarna att snabbt fylla i formuläret och undvika skrivfel.

* AEM Forms tillhandahåller en förifyllningstjänst för att läsa data från en fördefinierad XML-datafil och förifylla fälten i ett adaptivt formulär med innehållet i XML-filen för förifyllnad.
* XML-koden för förifyllda data måste vara kompatibel med schemat för den formulärmodell som är associerad med det adaptiva formuläret.
* Inkludera `afBoundedData`- och `afUnBoundedData`-avsnitt i XML-förifyllnad för att förifylla både bundna och obundna fält i ett anpassat formulär.

* För anpassningsbara formulär baserade på formulärdatamodell tillhandahåller AEM Forms en färdig tjänst för förifyllning av formulärdatamodell. förifyllningstjänsten söker efter datakällor för datamodellobjekt i det adaptiva formuläret och fyller i fältvärden i förväg när formuläret återges.
* Du kan också använda de adaptiva formulären för förifyllnad av filer, krökningar, tjänster och http-protokoll.
* AEM Forms har stöd för anpassade förifyllningstjänster som du kan använda som en OSGi-tjänst för att förifylla adaptiva formulär.

Mer information finns i [Förifyll adaptiva formulärfält](/help/forms/using/prepopulate-adaptive-form-fields.md).

### Signera och skicka adaptiva formulär {#signing-and-submitting-adaptive-forms}

För adaptiva formulär krävs Skicka-åtgärder för att bearbeta användarspecificerade data. En Skicka-åtgärd avgör vilken uppgift som utförs på de data som du skickar med ett anpassat formulär.

* Det finns flera inskickningsåtgärder som är tillgängliga i anpassningsbara formulär. Mer information finns i [Konfigurera åtgärden Skicka](/help/forms/using/configuring-submit-actions.md).
* Du kan skriva en anpassad sändningsåtgärd om standardåtgärderna för att skicka inte uppfyller ditt användningssätt. Mer information finns i [Skriva anpassad sändningsåtgärd för anpassningsbara formulär](/help/forms/using/custom-submit-action-form.md).
* Inkludera validering på serversidan för att förhindra att ogiltiga data skickas.

Du kan använda Adobe Sign med flera signaturer i anpassningsbara formulär. Tänk på följande när du konfigurerar Adobe Sign i adaptiva formulär. Mer information finns i [Använda Adobe Sign i ett anpassningsbart formulär](/help/forms/using/working-with-adobe-sign.md).

* Anpassningsbart formulär aktiverat för Adobe Sign skickas bara när alla signerare har signerat formuläret. Forms visas i Väntande signering tills formuläret har signerats av alla signerare.
* Du kan konfigurera signeringsupplevelsen i ett formulär eller dirigera om signerare till en signeringssida när de skickas.
* Konfigurera sekventiell eller parallell signering efter behov.

### Genererar postdokument {#generating-document-of-record}

Ett urkunder är en förenklad PDF-version av ett adaptivt formulär som du kan skriva ut, signera eller arkivera.

* Beroende på vilken formulärdatamodell ett anpassat formulär baseras på kan du konfigurera en mall för DoR enligt följande:

   * **XFA-formulärmall**: Använd den associerade XDP-filen som DoR-mall.
   * **XSD-schema**: Använd den associerade XFA-mallen som använder samma XML-schema som det adaptiva formuläret.
   * **Ingen**: Använd automatiskt genererad DoR.

* Konfigurera sidhuvud, sidfot, bilder, färg, teckensnitt och så vidare, direkt från fliken Dokument i den adaptiva formulärredigeraren.
* Använd `DoRService` för att generera DoR-koden programmatiskt.
* Uteslut dolda fält från DoR.
* Använd parametern `afAcceptLang` request för att visa DoR i en annan språkinställning.

### Felsöka och testa anpassningsbara formulär {#debugging-and-testing-adaptive-forms}

[AEM Chrome Plug-in](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/) är ett webbläsartillägg för Google Chrome som innehåller verktyg för att felsöka adaptiva formulär. Formulärförfattare och utvecklare kan använda dessa verktyg för att:

* Identifiera flaskhalsar och optimera formuläråtergivningens prestanda
* Felsöka nyckelord och bindRef-fel i formuläret
* Aktivera och konfigurera loggar
* Felsöka regler och skript i formuläret
* Utforska och lär dig mer om guideBridge API:er

Mer information finns i [AEM Chrome Plug-in - Adaptiv form](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/adaptive-form/).

### Validerar anpassningsbara formulär på AEM-servern {#validating-adaptive-forms-on-aem-server}

Valideringar på serversidan krävs för att förhindra försök att kringgå valideringar på klienten och eventuella kompromisser med inskickade data och överträdelser av affärsregler. Valideringar på serversidan körs på servern genom att det nödvändiga klientbiblioteket läses in.

* Inkludera funktioner i ett klientbibliotek för att validera uttryck i adaptiva formulär och ange klientbiblioteket i dialogrutan för adaptiva formulärbehållare. Mer information finns i [Omvalidering på serversidan](/help/forms/using/configuring-submit-actions.md#p-server-side-revalidation-in-adaptive-form-p).
* Validering på serversidan validerar formulärmodellen. Vi rekommenderar att du skapar ett separat klientbibliotek för validering och inte blandar det med andra saker som HTML-formatering och DOM-manipulering i samma klientbibliotek.

### Lokalisera anpassningsbara formulär {#localizing-adaptive-forms}

AEM tillhandahåller översättningsarbetsflöden som du kan använda för att lokalisera adaptiva formulär. Mer information finns i [Använda arbetsflöde för AEM-översättning för att lokalisera adaptiva formulär](/help/forms/using/using-aem-translation-workflow-to-localize-adaptive-forms.md).

Nedan beskrivs några tips om hur du översätter anpassningsbara formulär:

* Använd adaptiva formulärfragment för gemensamma element i olika formulär och lokalisera fragment. Det gör att du kan lokalisera ett fragment en gång och det återspeglas i alla former där det lokaliserade fragmentet används.
* Ändringar som att lägga till en ny komponent eller använda ett skript i ett lokaliserat formulär lokaliseras inte automatiskt. Därför måste du slutföra ett formulär innan du lokaliserar det för att undvika flera lokaliseringscykler.
* Använd parametern `afAcceptLang` request för att åsidosätta webbläsarens språkområde och återge formuläret i det angivna språkområdet. Följande URL måste till exempel återge formuläret på japanska, oavsett vilket språk som har angetts i webbläsarinställningen:

  `https://'[server]:[port]'/<contextPath>/<formFolder>/<formName>.html?wcmmode=disabled&afAcceptLang=ja`

* AEM Forms har för närvarande stöd för lokalisering av anpassningsbara formulärinnehåll på engelska (en), spanska (es), franska (fr), italienska (it), tyska (de), japanska (ja), portugisiska-brasilianska (pt-BR), kinesiska (zh-CN), kinesiska-taiwanesiska (zh-TW) och koreanska (ko-KR). Du kan dock lägga till stöd för nya språk för adaptiva formulär vid körning. Mer information finns i [Stöd för nya språk för lokalisering av adaptiva formulär](/help/forms/using/supporting-new-language-localization.md).

## Förbered formulärprojekt för produktion {#prepare-forms-project-for-production}

### Lägger till server för bearbetning av formulär {#adding-forms-processing-server}

Du kan konfigurera ytterligare en instans av AEM Forms-servern som finns bakom brandväggen i en skyddad zon. Du kan använda den här instansen för:

* **Gruppbearbetning**: jobb som är återkommande eller schemalagda i batchar med stor belastning. Du kan till exempel skriva ut programsatser, generera korrespondenser och använda dokumenttjänster som PDF Generator, Output och Assembler.
* **Lagrar PII-data**: Spara PII-data på bearbetningsservern. Det är inte nödvändigt om du redan använder en anpassad lagringsleverantör för att lagra PII-data.

### Flyttar projekt till en annan miljö {#moving-project-to-another-environment}

Du behöver ofta flytta dina AEM-projekt från en miljö till en annan. Några av de viktigaste saker man bör komma ihåg när man rör sig är följande:

* Säkerhetskopiera befintliga klientbibliotek, anpassad kod och konfigurationer.
* Distribuera produktpaket och korrigeringsfiler manuellt och i angiven ordning i den nya miljön.
* Distribuera projektspecifika kodpaket och paket manuellt och som ett separat paket eller paket på den nya AEM-servern.
* (*AEM Forms endast på JEE*) Distribuera LCA och DSC manuellt på Forms Workflow-servern.
* Använd funktionen [Exportera/importera](/help/forms/using/import-export-forms-templates.md) för att flytta resurser till den nya miljön. Du kan också konfigurera replikeringsagenten och publicera resurserna.
* När du uppgraderar ersätter du alla föråldrade API:er och funktioner med nya API:er och funktioner.

### Konfigurera AEM {#configuring-aem}

Nedan beskrivs några tips om hur du konfigurerar AEM för att förbättra prestandan generellt:

* Aktivera HTML-klientbibliotekskomprimering för JavaScript och CSS från Felix Console.
* Cachelagra alla klientbibliotek på `/etc.clientlibs/fd` och eventuella ytterligare anpassade klientbibliotek på AEM Dispatcher för att öka svarstiden och säkerheten för dina publicerade formulär. Mer information finns i [Dispatcher](https://helpx.adobe.com/se/experience-manager/dispatcher/using/dispatcher.html).

* Cachelagra inte `/content/forms/af/`- och `/content/dam/formsanddocuments/*`-sökvägar. Mer information om hur du konfigurerar cachning för adaptiva formulär finns i [Cachelagra adaptiva formulär](/help/forms/using/configure-adaptive-forms-cache.md).

* Aktivera HTML via webbserverkomprimeringsmodulen. Mer information finns i [Prestandajustering av AEM Forms-server](/help/forms/using/performance-tuning-aem-forms.md).
* Öka antalet anrop per begäran för stora formulär. Se [Optimera prestanda för stora och komplexa formulär](#optimizing-performance-of-large-and-complex-forms).
* Skapa [anpassade felsidor som visas av felhanteraren](/help/sites-developing/customizing-errorhandler-pages.md).
* Säker AEM Forms-server.

   * Använd körningsläget `nosamplecontent` för att kontrollera att det inte finns något exempelinnehåll och exempelanvändare distribuerade på produktionsservern. Se [Köra AEM i produktionsklart läge](/help/sites-administering/production-ready.md).

* Behåll stackstorleken till minst 8 GB. Andra inställningar finns i [Prestandajustering av AEM Forms-servern](/help/forms/using/performance-tuning-aem-forms.md).
* Använd tjänstanvändarsessioner i stället för administratörssessioner för att köra uppgifter på tjänstnivå. Mer information finns i [Tjänstautentisering](https://sling.apache.org/documentation/the-sling-engine/service-authentication.html).

>[!VIDEO](https://vimeo.com/)

### Konfigurera extern lagring för utkast och skickade formulärdata {#external-storage}

I en produktionsmiljö bör du inte lagra inlämnade formulärdata i AEM-databasen. Standardimplementeringen av Forms Portal Store, Store Content och Store PDF submit-åtgärderna lagrar formulärdata i AEM-databasen. De här Skicka-åtgärderna är endast avsedda för demonstrationssyften. Dessutom används portallagring som standard för funktionerna Spara, Återuppta och Spara automatiskt. Ta därför följande rekommendationer i beaktande:

* **Lagra utkastdata**: Om du använder funktionen Utkast i adaptiva formulär bör du implementera ett anpassat tjänstleverantörsgränssnitt (SPI) för att lagra utkastdata i en säkrare lagringsplats som en databas. Mer information finns i [Exempel på hur du integrerar komponenter för utkast och överföringar med databas](/help/forms/using/integrate-draft-submission-database.md).

* **Lagra inskickningsdata**: Om du använder Form Portal Submit Store bör du implementera en anpassad SPI för att lagra inskickningsdata i en databas. Se [Exempel på hur du integrerar utkast- och inskickskomponenter med databas](/help/forms/using/integrate-draft-submission-database.md) för en exempelintegrering.

  Du kan också skriva en anpassad skicka-åtgärd som lagrar formulärdata och bifogade filer i säker lagring. Mer information finns i [Skriva anpassad skickaåtgärd för adaptiva formulär](/help/forms/using/custom-submit-action-form.md).

* **Längd på utkast-ID**: När du sparar ett adaptivt formulär som ett utkast genereras ett utkast-ID som unikt identifierar utkastet. Minsta tillåtna värde för längden på utkastets ID-fält är 26 tecken. Adobe rekommenderar att du anger ett utkast-ID med 26 eller fler tecken.

### Hantera personligt identifierbar information {#handling-personally-identifiable-information}

En av de största utmaningarna för organisationer är att hantera personligt identifierbara data. Här följer några tips som kan hjälpa dig att hantera sådana data:

* Använd en säker, extern lagringsplats som databas för att lagra data från utkast och skickade formulär. Se [Konfigurera extern lagring för utkast och skickade formulärdata](/help/forms/using/adaptive-forms-best-practices.md#external-storage).
* Använd formulärkomponenten Villkor om du vill få uttryckligt medgivande från användaren innan du aktiverar automatiskt sparande. I det här fallet aktiverar du bara Spara automatiskt när användaren godkänner villkoren i villkorskomponenten.

## Välj regelredigeraren, kodredigeraren eller anpassade klientlibs för det anpassade formuläret {#RuleEditor-CodeEditor-ClientLibs}

### Regelredigerare {#rule-editor}

<!--The AEM Forms Rule Editor offers predefined functions for defining rules in adaptive forms without extensive programming. It facilitates the implementation of conditional logic, data validation, and integration with external sources. This visual interface is especially valuable for business users and form designers, enabling them to create dynamic and complex rules with ease, here we discusss few use cases where rule editor allows you to:-->

Regelredigeraren i AEM Forms har ett visuellt gränssnitt för att skapa och hantera regler, vilket minskar behovet av omfattande kodning. Det kan vara särskilt användbart för företagsanvändare eller formulärdesigners som inte har avancerade programmeringskunskaper men behöver definiera och underhålla affärsregler i formulären, här diskuterar vi få användningsfall där regelredigeraren tillåter dig:

* &#x200B;<!-- Allows you --> Definiera affärsregler för formulären utan behov av omfattande programmering.
* &#x200B;<!-- Use the Rule Editor when you need --> Använda villkorsstyrd logik i formulären. Detta inkluderar att visa eller dölja formulärelement, ändra fältvärden baserat på vissa villkor eller dynamiskt ändra formulärens beteende.
* &#x200B;<!--When you want --> Regelredigeraren kan användas för att definiera valideringsvillkor om du vill tillämpa datavalideringsregler för formulärinskickade formulär.
* &#x200B;<!-- When you need --> För att integrera formulären med externa datakällor (FDM) eller tjänster kan regelredigeraren hjälpa dig att definiera regler för hämtning, visning och ändring av data under formulärinteraktioner.
* &#x200B;<!-- If you want -->Om du vill skapa dynamiska och interaktiva formulär som svarar på användaråtgärder kan du definiera regler som styr formulärelementens beteende i realtid i regelredigeraren.

Regelredigeraren är tillgänglig för både AEM Forms Foundation-komponenter och Core-komponenter.

### Kodredigeraren {#code-editor}

Kodredigeraren är ett verktyg i Adobe Experience Manager (AEM) Forms som gör att du kan skriva egna skript och kod för mer komplexa och avancerade funktioner i dina formulär, och här diskuterar vi några få användningsområden:

* När du behöver implementera anpassad logik eller beteende på klientsidan som går utöver funktionerna i AEM Forms regelredigerare. Med kodredigeraren kan du skriva JavaScript-kod för att hantera komplexa interaktioner, beräkningar eller valideringar.
* Om formuläret kräver bearbetning på serversidan eller integration med externa system kan du använda kodredigeraren för att skriva egna skript på serversidan. Du kan använda guideBridge API i kodredigeraren för att implementera komplex logik för formulärhändelser och objekt.
* När du behöver skräddarsydda användargränssnitt som går utöver standardfunktionerna i AEM Forms-komponenterna kan du använda kodredigeraren för att implementera anpassade format, beteenden eller till och med skapa anpassade formulärkomponenter.
* Om ditt formulär innehåller asynkrona åtgärder som asynkron datainläsning kan du använda kodredigeraren för att hantera dessa åtgärder med anpassad asynkron JavaScript-kod.

Det är viktigt att komma ihåg att det krävs en god förståelse för JavaScript- och AEM Forms-arkitekturen när du använder kodredigeraren. När du implementerar anpassad kod måste du dessutom följa vedertagna standarder, följa säkerhetsriktlinjer och noga testa koden för att förhindra potentiella problem i produktionsmiljöer. Du kan implementera ett återanrop för FDM med kodredigeraren.

Kodredigeraren är endast tillgänglig för AEM Forms Foundation Component. För komponenter med adaptiv Form Core kan du använda anpassade funktioner för att skapa egna formulärregler, som beskrivs i nästa avsnitt.

### Anpassade funktioner {#custom-client-libs}

Det kan vara bra att använda anpassade klientbibliotek i AEM Forms (Adobe Experience Manager Forms) i olika scenarier om du vill förbättra formulärens funktionalitet, format eller beteende. Här är några situationer där det kan vara lämpligt att använda anpassade klientbibliotek:

* Om du behöver implementera en unik design eller ett varumärke för dina formulär som är mer än vad som finns i standardformateringsalternativen från AEM Forms, kan du välja att skapa anpassade klientbibliotek som styr utseendet och känslan.
* När du behöver anpassad logik på klientsidan kan du återanvända metoder i flera formulär eller beteenden som inte kan uppnås med AEM Forms standardfunktioner. Detta kan innefatta dynamiska formulärinteraktioner, anpassad validering eller integrering med bibliotek från tredje part.
* För att förbättra formulärens prestanda genom att optimera och minimera resurserna på klientsidan. Anpassade klientbibliotek kan användas för att paketera och komprimera JavaScript- och CSS-filer, vilket minskar den totala sidinläsningstiden.
* När du behöver integrera ytterligare JavaScript-bibliotek eller ramverk som inte ingår i AEM Forms standardinställningar. Detta kan vara nödvändigt för funktioner som förbättrade datumväljare, diagram eller andra interaktiva komponenter.

Innan du bestämmer dig för att använda anpassade klientbibliotek är det viktigt att tänka på underhållsinformation, potentiella konflikter med framtida uppdateringar och efterlevnad av bästa praxis. Se till att anpassningarna är väldokumenterade och testade så att du slipper problem under uppgraderingarna eller när du samarbetar med andra utvecklare.

>[!NOTE]
> Anpassad funktion finns för både AEM Forms Foundation-komponenter och Core-komponenter.

**Fördelar med anpassade funktioner:**

**Anpassade funktioner** ger en avsevärd fördel jämfört med **kodredigeraren** eftersom den ger en tydlig separation mellan innehåll och kod som förbättrar samarbetet och effektiviserar arbetsflödena. Du bör använda anpassade funktioner för följande fördelar:

* **Använd versionskontroll som Git:** utan skarvar
   * Isoleringen av kod från innehåll minskar Git-konflikter avsevärt under innehållshantering och främjar en välordnad databas.
   * Anpassade funktioner är användbara för projekt där flera medarbetare arbetar samtidigt.

* **Tekniska fördelar:**
   * Anpassade funktioner erbjuder modularitet och inkapsling.
   * Moduler kan utvecklas, testas och underhållas oberoende av varandra.
   * Förbättrar återanvändbarheten och underhållet av kod.

* **Effektiv utvecklingsprocess:**
   * Tack vare modulariteten kan utvecklare fokusera på specifika funktioner.
   * Minskar bördan för utvecklarna genom att minska komplexiteten i hela kodbasen för en effektivare utvecklingsprocess.
