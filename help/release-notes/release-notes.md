---
title: Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS, SP2
description: Hitta aktuella versionsuppgifter för Adobe Experience Manager 6.5 LTS, tjänstepaket 2.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: b5a8f555-c061-4fe2-a100-cc01335959cb
source-git-commit: f75f02c1e10deb6eef788d6584229c045b88880d
workflow-type: tm+mt
source-wordcount: '6062'
ht-degree: 0%

---

# Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS, SP2 {#release-notes}

## Versionsinformation {#release-information}

| Produkt | [!DNL Adobe Experience Manager] 6.5 LTS |
|---|---|
| Version | Service Pack 2 (SP2) <!-- UPDATE FOR EACH NEW RELEASE --> |
| Typ | Service Pack-version |
| Datum | 19 februari 2026 <!-- UPDATE FOR EACH NEW RELEASE --> |
| Hämta URL | [Programdistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack-lts/cq-quickstart-6.6.2.jar) |


<!-- UPDATE ABOVE FOR EACH NEW RELEASE -->

## Vad ingår i [!DNL Adobe Experience Manager] 6.5 LTS, SP2 {#what-is-new}

<!-- UPDATE EACH RELEASE -->

[!DNL Experience Manager] 6.5 LTS, SP2 innehåller nya funktioner, viktiga förbättringar som kunden efterfrågat samt felkorrigeringar. Det innehåller även förbättringar av prestanda, stabilitet och säkerhet som släppts sedan den första tillgängligheten av 6,5 LTS i mars 2025. [Installera detta Service Pack](#install-update) på 6.5 LTS.

## Nyckelfunktioner och förbättringar

**AEM Sites**

AEM 6.5 LTS SP2 innehåller nu OpenAPI:er för [Content Fragment och Model Management](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/65lts/) och [Launches](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/launches/). Dessa API:er ger åtkomst till innehållsfragment och starter för redigering och schemaläggning. De använder samma moderna OpenAPI:er som AEM as a Cloud Service.


<!-- UPDATE THE EACH RELEASE -->

## Åtgärdade problem i 6.5 LTS, Service Pack 2 {#fixed-issues}

<!-- UPDATE BELOW FOR EACH NEW RELEASE -->

### [!DNL Sites]{#sites-65-LTS-SP2}

#### Tillgänglighet {#sites-accessibility-65-lts-sp2}

* Textkomponenten tappade tangentbordsfokus när författare hovrade objekt i komponentwebbläsaren under redigeringen. Detta störde typningen och utlöste ett tillgänglighetsfel enligt WCAG 3.2.1. Korrigeringen förhindrar att hovringsformatet ändrar fokus och ser till att textkomponenten är i fokus när komponentwebbläsaren interagerar. (SITES-35370)
* Fokushanteringen i RTF-fältet Beskrivning som blockerade framåtnavigering med tabbtangenten har åtgärdats. Användarna fastnade i textredigeraren eftersom komponenten förlitade sig på ett tangentbordskommando som inte var standard för att flytta fokus, vilket gjorde att den förväntade dialognavigeringen bröts. Ändringen använder standardtangentbordsinteraktion och bevarar logisk tabbordningsföljd i hela dialogrutan. (SITES-35228)
* Ett problem i Sites Editor som orsakade ett oväntat beteende vid sidredigering och som ledde till inkonsekvent komponentinteraktion har åtgärdats. Författare upplevde otillförlitliga användargränssnittssvar som påverkade standardredigeringsuppgifter och minskad arbetsflödeseffektivitet. Uppdateringen förfinar den underliggande redigeringslogiken och återställer stabil, förutsägbar interaktion mellan de berörda komponenterna. (SITES-35227)
* en regression som förstörde resursväljaren i sidredigeraren och förhindrade den från att läsas in i specifika sidredigeringsscenarier. Författare kan nu öppna och använda resursväljaren normalt när de väljer eller bläddrar bland resurser när de redigerar en sida. Den här ändringen återställer konsekvent åtkomst till arbetsflöden för val av resurser som inte kunde läsas in. (SITES-35226)
* Eliminerade ett problem i webbplatsredigeraren som orsakade inkonsekvent beteende under sidinteraktion och störde arbetsflödena för standardredigering. Felet ledde till oväntade gränssnittssvar som stör komponentkonfigurationen och innehållsuppdateringarna. Uppdateringen stabiliserar de funktioner som påverkas och återställer tillförlitlig körning av redigeringsåtgärder över flera sidor. (SITES-35225)
* Eliminerade ett fel i webbplatsredigeringsgränssnittet som orsakade inkonsekvent beteende under sidredigering och störde normala arbetsflöden. Författare upptäckte oväntade användargränssnittssvar som stör komponentinteraktion och innehållsuppdateringar. Uppdateringen stabiliserar de funktioner som påverkas och återställer tillförlitligt och förutsägbart beteende i olika redigeringsscenarier. (SITES-35224)
* AEM Sites har nu stöd för `alt` text i bilder för att uppfylla ADA- och WCAG-kraven. Sidutdata utelämnar inte längre `alt` attribut, så skärmläsare får korrekt alternativ text. (SITES-27153)
* Åtgärdade verktygsfältets layout för `Note Add` så att knappen Lägg till inte längre överlappar titeln vid visningsrutans bredd på 320 px. Förbättrad flödesomformning på små skärmar så att kontrollerna förblir läsbara och användbara vid zoomning på 400 %. (SITES-25376)
* Korrigerade meddelanden om saknade skärmläsare för länkvalsdialogrutefel. Gränssnittet publicerar nu feltext via en behållare för statusmeddelanden, så att NVDA läser meddelandet så fort det visas. (SITES-25368)
* ARIA-rutnät och rutnätscellroller har tagits bort från resurslistan på sidospåret. Standardlistsemantik och tangentbordsfokusordning återställdes, vilket förbättrade skärmläsarnavigeringen och minskade antalet extra tabbstopp. (SITES-25361)
* Fokussekvensering på sidospår i Assets har korrigerats. Tangentbordsanvändare når nu alla resursåtgärder, inklusive redigering, via en konsekvent tabbsökväg. (SITES-25360)
* Fast layoutspill i Search Assets modal vid visningsrutans bredd på 320px. Modalt innehåll flödar nu om och förblir läsbart, så kontrollerna överlappar eller överlappar inte längre dialogrutan. (SITES-25330)
* Korrigerade NVDA-utdata för knappen Redigera. NVDA presenterar nu åtgärden Redigera, inte knappen Förhandsgranska nedtryckt. (SITES-25320)
* Korrigerade textinmatningar i verktygsfältet för icke-namngivna demografi som orsakade tysta eller generiska skärmläsarutdata. Varje inmatning visar nu ett tydligt etikettbaserat hjälpmedelsnamn, som förbättrar tangentbords- och hjälpteknisk navigering. (SITES-25316)
* Tangentbordsfokusordningen för det demografiska verktygsfältet har korrigerats vid navigering i förhandsvisning av layout. Tabbnavigering går nu direkt från knappen Demografisk till verktygsfältskontrollerna, utan att hoppa till det sekundära verktygsfältet. (SITES-25305)
* Felaktig meddelanderoll för etiketterna&quot;Mindre Screens&quot; och&quot;Surfplatta&quot; på linjalen för redigeringslayout har korrigerats. Skärmläsare meddelar nu dessa etiketter vid rätt linjalmarkörer, som matchar sidlayouten. (SITES-25291)
* Verktygsfältet Redigera layout med 200 % zoomning har åtgärdats. Innehållet finns nu kvar i visningsrutan och kan nås via rullning. (SITES-25288)
* Felaktig fokusordning i anteckningsövertäckningen har åtgärdats. Tabbordningen går nu igenom överläggskontrollerna och anteckningsobjekten. Den överordnade sidan får inte längre fokus bakom övertäckningen. (SITES-25282)
* Korrigerade färgrutor visar fokushantering. Dialogrutan flyttar nu fokus till en rensad rubrik och startar skärmläsarutdata vid startpunkten. NVDA läser inte längre allt innehåll i dialogrutorna i fel ordning. (SITES-25275)
* Den modala fokushanteringen för Timewarp har åtgärdats efter att datumväljaren stängts. `Escape` returnerar nu fokus till datumväljaren. När du väljer ett datum placeras fokus nu på inmatningsfältet bredvid kontrollen Datumväljare, vilket förhindrar att fokus går förlorat och ger åtkomst till bakgrundssidan. (SITES-25264)
* Hantering av tangentbordsfokus för dialogrutan Ta bort anteckning har åtgärdats. Avbryt återställer nu fokus till kontrollen `Delete` som öppnade dialogrutan, inte till kontrollen Bekräfta hex-värde. Skärmläsare meddelar inte längre innehåll i dialogrutor som inte är relaterade till den efter Avbryt. (SITES-25258)
* Fokushantering för den modala dialogrutan Anteckning har åtgärdats. När du öppnar dialogrutan får du nu fokus på dialogrutans rubrik och förhindrar att NVDA läser arbetsytans innehåll och orelaterad dialogrutetext. Tangentbordsnavigering finns nu kvar i dialogrutan tills den stängs. (SITES-25257)
* Lösta problem med modal sökningslayout vid bredden 320px. Modalt innehåll flödas nu om korrekt och undviker överlappning med trädkatalogen. Användarna kan visa resultaten och navigera i katalogen utan att behöva använda några dolda kontroller. (SITES-25246)
* Söka efter modal text i klipp inte längre efter att textavståndet har ökat. Trädkatalogslayouten behåller nu tydlig separation så att etiketter och poster förblir läsbara. Användarna kan nu söka och navigera utan överlappande eller urklippt text. (SITES-25245)
* När du aktiverar Anteckning flyttas tangentbordsfokus till anteckningsinnehåll, inte till knappen Avsluta anteckning. Tabbordningen följer en logisk sekvens och gör att relaterade kontroller kan nås utan omvänd navigering. (SITES-25241)
* Länkarna Ange datum och Avsluta tidsförvrängning saknade en synlig fokusindikator vid tangentbordsnavigering. Gränssnittet återger nu en distinkt fokusstil med hög kontrast så att användarna snabbt kan identifiera den aktiva länken. (SITES-25232)
* Rubriken Teaser Modal förhindrar inte längre att tangentbordsanvändare flyttar dialogrutan. Med tangentbordskontrollerna kan du nu hämta, flytta och släppa funktionsmakron, vilket förbättrar användbarheten för skärmläsare och den övergripande funktionaliteten. (SITES-25226)
* AEM använder nu en hjälpmedelsetikett för knappen Teaser Modal Info. Skärmläsare meddelar ett tydligt åtgärdsnamn i stället för standardikonens alt-text-sträng. (SITES-25223)
* Skärmläsare meddelar nu korrekt åtgärd när användare aktiverar knappen Redigera. NVDA rapporterar inte längre &quot;Preview button pressed&quot;, som orsakade missvisande feedback och förvirring under tangentbordsnavigering. (SITES-25208)
* När du expanderar den vänstra skenan flyttas nu tangentbordsfokus till den första kontrollen för vänster järnväg. Tabbsekvensen hoppar inte längre till det sekundära verktygsfältet eller går till mittlistan, vilket innebär att tangentbordsanvändare kan nå innehållet på vänster rad utan omvänd navigering. (SITES-24998)
* Innehållet i enhetsemulatorfältet är nu helt synligt vid en visningsportbredd på 320 px. Verktygsfältstext och kontroller radbrytning istället för trunkering, vilket minskar överlappningen och förbättrar läsbarheten. (SITES-24953)
* AEM visar nu den fullständiga iPhone-enhetsetiketten i emulatorns verktygsfält. Text trunkeras inte längre vid standardbredd, vilket förbättrar läsbarheten och tydligheten i enhetsvalet. (SITES-24952)
* Listtabellrubriker visar nu sorteringsstatus via ARIA. Skärmläsare meddelar stigande eller fallande ordning efter en kolumnsorteringsåtgärd. (SITES-24943)
* AEM bevarar nu synligheten för etiketten Fler åtgärder-menyn i kortvyn när textavståndet ändras. Menyalternativen innehåller fullständig text, inklusive Snabbpublicering, och menyn förblir läsbar under inställningarna för WCAG-textavstånd. (SITES-24941)
* Menyraden Kortåtgärder visar nu ett hjälpmedelsnamn i kortvyn. Skärmläsare talar tydligt om syftet med menyraden och röstkontrollen kan rikta kontrollen efter namn. (SITES-24938)
* Kortvyn är inte längre beroende av ARIA-stödrastersemantik som orsakade förvirrande skärmläsarbeteende. Gränssnittet innehåller nu meningsfulla roller och etiketter för kortinnehåll och kortåtgärdsfältet, vilket minskar antalet saknade kontroller vid användning av tangentbordet. (SITES-24933)
* Verktygstipset `Delete Modal` visas nu varje gång användarna hovrar verktygstipsikonen. Fokusåtgärder visar nu samma verktygstipstext, vilket förbättrar upprepad åtkomst för mus- och tangentbordsanvändare. (SITES-24778)
* Navigering till vänster via tåg följer nu den förväntade tangentbordsfokusordningen efter att användaren har konfigurerat rälen. Flikfokus hamnar på det markerade området Vänster linje i stället för Växla visning, vilket förbättrar skärmens läsarnavigering. (SITES-24754)
* Korrigerade felaktig NVDA-återkoppling under färgrutenavigering i modala användarinställningar. NVDA läser nu etiketten för den färgruta som får fokus, vilket tar bort missvisande färgutdata. Färgrutorna har nu stöd för enhetlig tangentbordsnavigering och tydlig markeringsmedvetenhet. (SITES-24739)
* Minskade utförliga NVDA-utdata för kontrollen `Spin`. En överflödig gruppetikett som duplicerade indataetiketten togs bort, så NVDA meddelar kontrollnamnet en gång. Tangentbords- och skärmläsarnavigeringen ger nu ett enda tydligt meddelande. (SITES-24725)
* Dialogrutan Carousel placerar nu fokus på dialogrutans rubrik i stället för på fliken Objekt. Avbryt och Esc återställer fokus till den kontroll som öppnade dialogrutan, vilket minskar NVDA-utdata. (SITES-24716)
* Dialogrutan för länkval justerar nu den programmatiska etiketten med skärmetiketten för trädobjekt på sista nivån. Piltangentsnavigering utlöser ett tillförlitligt skärmläsarmeddelande för varje objekt och tar bort vilseledande etikettutdata. (SITES-24710)
* Dialogrutan Länka öppen markering flödar nu om korrekt under en visningsruta på 320 pixlar. Innehållet överskrider inte längre modala eller trunkerade värden och modala visas inte längre med en vågrät rullningslist. (SITES-24709)
* Dialogrutan Länka öppen markering återställer nu tangentbordsfokus till utlösaren efter Stäng eller Avbryt. Fokus leder inte längre till länkindata, vilket gör att skärmläsarkontexten blir stabil och minskar den extra navigeringen. (SITES-24707)
* Dialogrutan för modal bild följer nu en sekvens med logiskt fokus. Fokus hoppar inte längre över tidigare kontroller eller fall på sidlandmärket efter Avbryt, och användarna återfår fokus på knappen Konfigurera när de har avslutat. (SITES-24693)
* Dialogrutan Referenser - spärra/knip visar nu tangentbordsfokus. Tabb och Skift+Tabb finns kvar i dialogrutekontrollerna och fokus kan inte längre tas bort från sidinnehållet. Skärmläsare meddelar endast innehåll i dialogrutor. (SITES-24683)
* Det spärrade läget för val av hyperlänksbana anger nu fokus på dialogrutans rubrik som är öppen. Avbryt stänger dialogrutan och återställer fokus till knappen i dialogrutan Öppna markering, vilket förhindrar att fokus går förlorat och gör att skärmläsaren inte kan skriva ut något. (SITES-24672)
* I sökfältet används nu en beständig etikett på skärmen i stället för platshållartext. Etiketten förblir synlig under inmatningen, vilket gör att tangentbordet, skärmläsaren och talanvändaren blir tydligare. (SITES-24529)
* Dialogrutan för modal modal anger nu fokus på dialogrutans rubrik när den är öppen. Om du stänger dialogrutan återgår fokus till kontrollen `Configure`, vilket förhindrar att fokus går förlorat och att skärmläsaren genererar för många utdata. (SITES-24522)
* Side Rail Assets-panelen har nu en stängningskontroll. Stäng återgår tangentbordsfokus till växlingsknappen Side Rail och förhindrar att du tvingar tabbning genom panelinnehållet. (SITES-24489)
* Tabbtangenten når nu knappar och länkar i administratörstabeller. Användare förlitar sig inte längre på pilknappsnavigering för att hitta interaktiva kontroller. (SITES-24285)
* Dialogrutan Bildkomponent visar inte längre dekorativ hjälp och helskärmsikoner som bilder. Skärmläsare hoppar nu över dessa ikoner och fokuserar på åtgärdbara kontroller och fältinnehåll. (SITES-2940)
* Platsadministratör tar nu bort bildrollen från mappminiatyrbildikoner. Dessa dekorativa element hoppas över med hjälpfunktioner och fokus ligger på mappnamn och åtgärder. (SITES-2852)
* Innehållsträdet dirigerar nu tangentbordsfokus till det aktiva trädobjektet eller det första trädobjektet. Trädbehållaren fungerar inte längre som ett tomt tabbstopp, vilket förhindrar att fokus flyttas från Skift+Tabb. (SITES-1577)

#### Administratörsgränssnitt{#sites-adminui-65-lts-sp2}

Visningsinställningar för platskonsolen återspeglar inte de kolumner som visas i listvyn. Dialogrutan öppnas med avmarkerade kryssrutor och felaktigt antal markerade kolumner. Dialogrutan för att korrigera synkronisering visas med de aktiva rutnätskolumnerna och räknaren uppdateras så att den matchar den faktiska kolumnsynligheten. (SITES-38576)

#### Klassiskt användargränssnitt{#sites-classicui-65-lts-sp2}

Vid redigering av textkomponenter i det klassiska användargränssnittet visades Raw-taggar i stället för RTF-text efter en uppgradering. I Service Pack 2 korrigeras klassisk gränssnittsåtergivning (Rich Text Editor) så att redigeraren visar formaterat innehåll och bevarar lagrad kod. Korrigeringen stoppar också markeringsexpansion vid upprepade redigeringar och sparar. (SITES-38709)

#### [!DNL Content Fragments]{#sites-contentfragments-65-lts-sp2}

Headless Eventing support missing required OSGi events for Content Fragments and Models in 6.5 LTS. Uppdateringen lägger till händelsepaketet plus nödvändiga beroenden och innehåller en 6,5 LTS-version. Content Fragment- och Model-händelser utlöses nu korrekt och har stöd för Launches API-arbetsflöden. (SITES-35329)

#### [!DNL Content Fragments] - Admin{#sites-admin-65-lts-sp2}

* Justerad komponenthantering i webbplatsens utvecklingsgränssnitt för att stoppa oregelbundna beteenden under siduppdateringar. Felet ledde till oförutsägbara redigeringssvar som stör ändringar av rutininnehåll och minskar arbetsflödets effektivitet. Uppdateringen justerar redigeringslogiken med förväntade interaktionsmönster och ger tillförlitliga resultat under redigeringsaktiviteterna. (SITES-35078) CRITICAL

* En regression bröt listvyn i Assets-konsolen för innehållsfragment och utlöste ett fel under liståtergivningen. Uppdateringen åtgärdar logiken i listvyn efter borttagning av förhandsvisningsinformation och återställer stabila listutdata. Konsolen visar nu Content Fragments utan fel och håller listor användbara. (SITES-38683)
* Taggetiketten lokaliseras nu i Content Fragment Editor. Redigeraren lokaliserar också samlingsetiketten så att gränssnittstexten matchar den valda språkinställningen. (SITES-977)


#### [!DNL Content Fragments] - Fragmentredigeraren{#sites-fragments-editor-65-lts-sp2}

* Variationstaggar för innehållsfragment försvinner när alternativknappen för funktionen fortfarande är inaktiverad efter omfaktorisering. Korrigeringen återställer stödet för varianttaggar även när den inte är aktiverad. Författare kan lägga till och visa varianttaggar igen i redigeraren för innehållsfragment. (SITES-38682) CRITICAL
* Redigerade innehållsfragment försvinner från Assets konsollista när författare har navigerat tillbaka från Content Fragment Editor. Webbläsarcachelagring returnerade en inaktuell lista och dolde det uppdaterade fragmentet tills en manuell uppdatering. Korrigeringen lägger till cachekontrollhantering för redigerarens retursökväg så att listan läses in korrekt och det redigerade fragmentet förblir synligt. (SITES-35374) CRITICAL

* RTE för innehållsfragment visade layoutproblem och visuella problem efter nyligen ändrade gränssnittsformat. Service Pack 2 förfinar RTE-formatet så att verktygsfältet och det redigerbara området återges korrekt och förblir läsbara. Innehållsfragmentsredigeraren justeras nu efter sidredigerarens utseende och beteende. (SITES-38684)
* Om IMS-scope tas bort från polarisresursväljaren bryts integreringen av innehållsfragment med leveransslutpunkten. Författare stöter på fel när de öppnar resursväljaren och väljer resurser. Uppdateringen lägger till de IMS-omfattningar som behövs och återställer stabil åtkomst på leveransnivå. (SITES-35837)
* Panelen Associerat innehåll återger inte längre en hårdkodad&quot;odefinierad&quot; platshållare. Innehållsfragmentsredigeraren löser nu upp texten med hjälp av lokaliseringsresurser, så redigerare ser översatt gränssnittstext. (SITES-33675)
  <!-- REMOVED FROM BUG LIST FEBRUARY 13, 2026 * Preview error messaging now uses localized strings instead of raw `Cannot print fragment's Json` text. The Content Fragment Editor now shows translated output across locales during GraphQL endpoint resolution failures. (SITES-33666)-->
* I Content Fragment Editor visas nu en översatt allmän fliketikett för alla språkområden. Redigeraren ersätter olokaliserad tabbtext och tar bort interna ID:n från tabbtitlar. (SITES-30715)
* I Content Fragment Editor visas nu översatta namn för tillåtna resurstyper. I väljarlistan blandas inte längre interna strängar och endast engelska etiketter när författare konfigurerar begränsningar för innehållsreferenser. (SITES-29699)

#### [!DNL Content Fragments] - GraphQL API {#sites-graphql-api-65-lts-sp2}

* Förfinad frågevalideringshantering i GraphQL för att stoppa misslyckade distributioner orsakade av fel vid filterkörning. Felet genererade undantag när programmet startades och blockerade lyckade utrullningar i drabbade miljöer. Revisionen säkerställer ett konsekvent valideringsbeteende och möjliggör smidig driftsättning utan avbrott i frågevalideringen vid körning. (SITES-34301) CRITICAL

* Dialogrutan Redigera GraphQL-slutpunkt visar nu lokaliserade gränssnittssträngar. I dialogrutan visas inte längre endast engelsk text, t.ex.&quot;GraphQL-schema hämtas från konfigurationen&quot;, och relaterade etiketter återges korrekt på alla språk. (SITES-34018)

#### [!DNL Content Fragments] - GraphQL Query Editor{#sites-graphql-query-editor-65-lts-sp2}

* Förfinad frågevalideringshantering i GraphQL för att stoppa misslyckade distributioner orsakade av fel vid filterkörning. Felet genererade undantag när programmet startades och blockerade lyckade utrullningar i drabbade miljöer. Revisionen säkerställer ett konsekvent valideringsbeteende och möjliggör smidig driftsättning utan avbrott i frågevalideringen vid körning. (SITES-35529)
* GraphQL Explorer fungerar inte längre när namnet på en Configuration Browser innehåller CJK-tecken. Slutpunktsskapande och sparad frågeåtkomst fungerar normalt och GraphQL Query Editor-sidan är felfri. (SITES-31616)

#### [!DNL Content Fragments] - Modellredigerare{#sites-model-editor-65-lts-sp2}

* Kapslade modeller för innehållsfragment slutade fungera när funktionen omfaktoriserades till en inaktiverad växlingsknapp. Korrigeringen återställer stödet för kapslade modeller utan att växla mellan ändringarna. Författare kan skapa och använda kapslade modeller igen i modellredigeraren. (SITES-38681) CRITICAL

* Filterpanelen för innehållsfragmentmodeller visar inte längre olokaliserade strängar. AEM visar nu lokaliserade filteretiketter och lokaliserade statusvärden för alla språk. (SITES-30863)
* Modellredigeraren för innehållsfragment återger nu lokaliserade strängar för dialogrutan för låsvarning. Gränssnittet ersätter olokaliserade engelska meddelanden med språkresurser på språk som stöds. (SITES-28592)

#### [!DNL Content Fragments] - REST API{#sites-restapi-65-lts-sp2}

AEM Headless kräver en dedikerad versionsavdelning för att undvika beroendeförhållanden och versionskonflikter med huvudprogramsbyggen. Uppdateringen lägger till en release/6.5lts headless branch och anpassar beroendeuppsättningar och paketversioner. Jenkins bygger nu den headless-kodbasen utan versionskollisioner. (SITES-36585)

<!-- #### Component console{#sites-component-console-65-lts-sp2} -->

#### Innehålls-API{#sites-content-api-65-lts-sp2}

En funktion som växlar felrapporterad API-status för sidhantering. Uppdateringen lägger till en dedikerad aktiveringsflagga och utvärderar den tillsammans med den befintliga växlingsknappen. API:t för sidhantering visar nu stabil status. API:t för platshantering är fortfarande experimentellt. (SITES-39284)

#### Core backend{#sites-core-backend-65-lts-sp2}

* En förändring i webbplatsutvecklingsmiljön som åtgärdar inkonsekvent beteende som stör arbetsflödena för standardsidredigering. Författare fick oväntade resultat vid komponentinteraktion, vilket påverkade innehållsuppdateringar och minskade tillförlitligheten. Ändringen återställer ett stabilt redigeringsbeteende och säkerställer att redigeringsåtgärderna körs konsekvent i alla berörda scenarier. (SITES-35162) CRITICAL

* Förfinade webbplatser-utvecklingsbeteende löser ett problem som orsakade inkonsekventa resultat vid komponentinteraktion och som orsakade avbrott i sidredigeringen. Författare fick oväntade användargränssnittssvar som påverkade innehållsuppdateringar och minskade arbetsflödets tillförlitlighet. Ändringen återställer stabil redigeringsstatushantering och säkerställer förutsägbar körning av redigeringsåtgärder i olika scenarier. (SITES-34499)

<!--
#### Core Components{#sites-core-components-65-lts-sp2}

#### Campaign integration{#sites-campaign-integration-65-lts-sp2}

#### Experience Fragments{#sites-experiencefragments-65-lts-sp2}

#### Foundation Components (Legacy){#sites-foundation-components-legacy-65-lts-sp2}
-->

#### Launches{#sites-launches-65-lts-sp2}

* Sites Timeline visade hårdkodad engelsk text under Launch-kampanjen:&quot;Created version ... before Promolaunch.&quot; Uppdateringen ersätter den hårdkodade strängen med lokaliserad meddelandehantering. På tidslinjen visas nu lokaliserad text och posten justeras efter AEM standardbeteende för lokalisering. (SITES-39157)
* Startomfånget för erbjudandet uppstod när författare befordrade ett underavsnitt med hjälp av Befordra aktuell sida och underordnade sidor. AEM befordrade även sidor som inte hade med saken att göra och orsakade oväntade ändringar på webbplatsen. Korrigeringen korrigerar beräkningen av startomfånget så att endast de valda underträdets höjningar höjs. (SITES-38315)
* Innehållsfragment i Launches ingick inte i indexet `damAssetLucene` och begränsade sökresultat och frågans effektivitet. Den här ändringen lägger till inledande innehållsfragmentsökvägar i indexdefinitionen. Sök och anpassade frågor hittar nu innehållsfragment under `/content/launches`. (SITES-35634)
* Startar UI och visar startkontroller för innehållsfragment även om produkten inte visar Content Fragment Launches i Touch UI. Den här ändringen rensar kodsökvägarna för Content Fragment Launch från cq-launches-content och justerar filtreringen av Launch-listan. Författarna ser nu enhetliga alternativ för sidstart utan Content Fragment Launch-poster. (SITES-35633)
* AEM 6.5 LTS Quickstart saknade nödvändiga startpaket och nödvändiga förutsättningar, vilket blockerade aktiveringen av OpenAPI. Uppdateringen lägger till startpaket och nödvändiga beroenden, som måttstöd, DAM-cfm-uppdateringar och kökonfiguration. Startar API:er som nu körs på 6.5 LTS Quickstart med de nödvändiga körningskomponenterna. (SITES-35297)
* CF lanserar paketering av nyare beroendeversioner och onödiga GraphQL-bibliotek, som komplicerade integreringen med AEM 6.5 LTS. Den här ändringen anpassar beroendeversionerna till AEM 6.5 LTS-baslinjen och tar bort oanvända GraphQL-beroenden. Paketupplösningen är nu konsekvent och CF Launches-starten är stabil. (SITES-35295)
* AEM Launches kör nu en dedikerad Jenkins-pipeline för 6.5 LTS-grenen. Pipelinen körs på natten och skickar felmeddelanden via e-post. Den här inställningen ökar testtäckningen och fångar regressioner tidigt. (SITES-35293)
* AEM 6.5 LTS har nu ett uppdaterat API-paket för lanseringar med anpassade artefaktversioner. Paketet spårar den primära kodraden samtidigt som rätt version av 6.5 LTS behålls. Den här uppdateringen stabiliserar Launches API-förbrukningen i 6.5 LTS-stacken. (SITES-35292)
* AEM 6.5 LTS innehåller nu ett uppdaterat startpaket med anpassade beroendeversioner. Uppdateringen lägger till startkärnshantering för datatyperna Fragment UUID och Reference UUID. Launch processing now keep consistent behavior across Launches and Content Fragment workflows. (SITES-35290)
* Förbättrade Sites-redigeraren för att åtgärda inkonsekvent beteende som stör de normala arbetsflödena för sidredigering. Författarna råkade ut för oväntad komponentinteraktion som stör innehållsuppdateringar och minskade redigeringstillförlitligheten. Ändringen återställer en konsekvent hantering av gränssnittstillstånd och säkerställer en förutsägbar körning av redigeringsåtgärder i olika scenarier. (SITES-35138)
* Startar Redigera visar nu lokaliserad feltext i stället för den hårdkodade `Provided path is not a launch`-strängen. Gränssnittet återger nu översatta meddelanden mellan språk när Edit (Redigera) får en ogiltig startsökväg. (SITES-33360)
* AEM 6.5 LTS innehåller nu startfunktionen för OpenAPI. Uppdateringen innehåller startpaket för API, innehållspaket och nödvändiga Quickstart-artefakter i paritet och aktiverar Content Fragment Launches OpenAPI scenarios with stable CI validation. (SITES-32050)
* Öppnar gränssnittet lokaliserar nu malletiketten Åsidosatt. Information om åsidosättning av mallar visar nu översatt text i stället för en sträng med enbart engelska. (SITES-29525)
* En lokaliseringsnyckel som saknas i **Platser** > **Startar** > **Redigera** har åtgärdats av AEM. Användarna ser nu ett översatt felmeddelande i stället för strängen&quot;Det går inte att uppdatera startkällistan&quot;. (SITES-21499)
* Startgränssnittet visar nu lokaliserade statusetiketter och åtgärder. I förhandsvisningsområdet visas översatt text för **Borttaget**, **Nytt** och **Visa** i stället för på engelska i Raw-format. (SITES-13540)
* Startskapande visar nu lokaliserade felmeddelanden. Gränssnittet visar inte längre rå engelska strängar, som `Unable to create launch page`, `Source root resource is not a page` eller `Mandatory parameter is missing`. (SITES-13085)


<!-- #### Link Checker{#sites-link-checker-65-lts-sp2} -->


#### MSM - Live-kopior{#sites-msm-live-copies-65-lts-sp2}

* Administratörerna hade begränsad insyn i hanteringen av push-on-modify för MSM under innehållsändringar. Korrigeringen lägger till detaljerad loggning runt mottagning av MSM-händelser och utrullning. Felsökningsutdata visar nu vilka händelser som utlösts, vilka innehållssökvägar som ändrats och vem som utlöste ändringen. (SITES-38029)
* AEM har åtgärdat ett problem med lokaliseringslayout i fältet för utrullning av utkast. Datumprompten passar nu in i kontrollen och kan läsas på alla språk som stöds, inklusive `fr_FR`. (SITES-14961)

<!-- #### Page editor{#sites-pageeditor-65-lts-sp2} -->

#### Replikering{#sites-replication-65-lts-sp2}

Sidredigeringspubliceringen hanterar nu URL:er som innehåller väljare eller suffix. Den publicerade begäran skickar nu JCR-sidsökvägen, inte en URL-sträng för väljare eller suffix, så aktiveringen slutförs och innehållet publiceras. Replikeringen returnerar nu en felstatus vid fel, vilket förhindrar att falska meddelanden om publicering har startats visas. (NPR-43288)

<!-- #### Rich Text Editor{#sites-rte-65-lts-sp2} -->

#### Mallredigerare{#sites-template-editor-65-lts-sp2}

Mallstatustext som visas lodrätt i **Verktyg** > **Allmänt** > **Mallar** för vissa språkområden. Etiketten &quot;outdated&quot; bröt layouten och lästes som en teckenkolumn. Korrigeringen korrigerar mallens statusformat så att etiketten återges på en enda vågrät rad. (SITES-36797)

#### Universell redigerare {#sites-universal-editor-65-lts-sp2}

* En OSGi-standardkonfiguration angavs som `preview=true` och universell redigerare tvingades starta i förhandsgranskningsläge. Den här uppdateringen korrigerar standardvärdet och återställer standardbeteendet för produktionsposten. Universell redigerare öppnas nu i produktionsläge såvida inte en administratör uttryckligen aktiverar förhandsgranskningsläget. (SITES-37193)
* Kommandot Öppna i Universal Editor är nu som standard förgranskningsläget i Dev- och Stage-miljöer. Kommandot lägger till preview=true, vilket gör att författarkontroller justeras efter förhandsvisningssammanhanget och förhindrar att oavsiktlig produktion öppnas. (SITES-33839)

### [!DNL Assets]{#assets-65-lts-sp2}

Assets Relate fungerar nu för filnamn som innehåller blanksteg. Uppdaterad Relate-klientlogik hanterar nu platsinnehållande sökvägar korrekt och undviker `undefined` källfel under relationsval. Dialogrutan Relatera öppnas och du kan spara relationer utan att användargränssnittet hänger sig eller snurrar. DAM-användare kan relatera, härleda och ta bort kopplingar mellan resurser utan att behöva byta namn på filer. (Assets-56418)

#### [!DNL Dynamic Media]{#assets-dm-65-lts-sp2}

* Ny integrering av videospelaren i Dynamic Media (begränsad utrullning) - Nu finns en ny upplevelse av videospelaren i Dynamic Media i AEM 6.6 QuickStart. Den här förbättringen är för närvarande bara aktiverad för initialkunder som en del av en kontrollerad utrullning. (Assets-60165)
* Ett problem där alternativet Välj miniatyrbild i dialogrutan för videoegenskaper inte öppnade resursväljaren har åtgärdats, vilket innebär att användarna kan välja anpassade miniatyrbilder för videoresurser. (Assets-58926)
* I Dynamic Media-videon lades stöd till för att välja arabiska i listrutan Undertexter och ljudspår, där författare kan hantera arabiska undertexter direkt i AEM. (Assets-61771)

<!-- #### [!DNL Dynamic Media] - Hybrid Mode {#assets-dm-hybrid-65-lts-sp2} -->


<!--
### [!DNL Forms]{#forms-65-lts-sp2}

#### Forms Designer

#### Forms

#### Forms JEE 

#### Forms Captcha {#forms-captcha-65-lts-sp2}

#### XMLFM {#forms-xmlfm-65-lts-sp2}

#### [!DNL Adaptive Forms] {#adaptive-forms-65-lts-sp2}

#### [!DNL Forms Designer] {#forms-designer-65-lts-sp2}

#### Forms Designer

#### AdaptIve Forms

#### Forms Captcha

#### Forms Management UI
-->


### Foundation {#foundation-65-lts-sp2}

#### Apache Felix {#foundation-apachefelix-65-lts-sp2}

* SSLING Resource Access Security körs nu i version 1.1.2. ResourceAccessSecurityImpl genererar inte längre ett ClassCastException under initiering när flera ResourceAccessGateHandler-tjänster registreras. Initieringen slutförs nu på ett tillförlitligt sätt och förhindrar startfel i miljöer med flera hanterare. (NPR-42750)
* JMX Console och Web Console skickar nu en `Content-Type: text/css header` för CSS-resurser för konsolen. Strikta MIME-kontroller blockerar inte längre inläsning av formatmallar, så användargränssnittet i `/system/console/jmx` återges med normal formatering. (GRANITE-63677)
* AEM undviker nu dubbletter av ACL-poster för gruppen `contributor` i den genererade gruppen `WEB-INF/resources/provisioning/model.txt`. WAR-utdata innehåller nu ett konsekvent ACL-block, vilket förhindrar förvirrande behörighetsskillnader under granskningen. (GRANITE-63269)
* AEM rensar inte längre inställningarna för brandväggen för deserialisering av blockeringslista och tillåtelselista under paketuppdateringsåtgärder. Uppdaterad logik för filterregistrering gör att den aktiva brandväggsinstansen är justerad mot den sparade konfigurationen, så skyddet förblir aktiverat utan omstart. GRANITE-61382
* Felix Web Console genererar inte längre återkommande `NullPointerException`-fel under `/system/console`-åtkomst. Uppdaterad ServiceTracker-hantering förhindrar att spårningsstatus är null. Konsolinloggning och navigering är stabila vid upprepade förfrågningar och automatisk validering. GRANITE-61042

<!--
#### Campaign{#foundation-campaign-65-lts-sp2}

#### Cloud Services{#foundation-cloudservices-65-lts-sp2}

#### Communities {#foundation-communities-65-lts-sp2}

#### Content distribution{#foundation-content-distribution-65-lts-sp2}
-->

#### CRX {#foundation-crx-65-lts-sp2}

CRXDE Lite visar inte längre en tom flik när du öppnar en JSP-fil efter en Service Pack-uppgradering. AEM levererar nu matchande kod för CodeMirror-kärna och tillägg, vilket förhindrar det allvarliga webbläsarfelet och gör redigeraren användbar. GRANITE-64333

#### Granit{#foundation-granite-65-lts-sp2}

Expression Security Validator hanterar nu tomma eller null OSGi-konfigurationsvärden. Programmet tillämpar säkra standardvärden, ignorerar tomma arrayer och registrerar rensade loggar, vilket förhindrar NullPointerException och oberäkneliga valideringsresultat. (NPR-43163)

<!-- #### HTL{#foundatoin-htl-5-lts-sp2} -->

#### Integreringar{#foundation-integrations-65-lts-sp2}

AEM synkroniserar nu Adobe Target-aktiviteter även när det finns start- och slutdatum. Målnyttolasten formaterar nu aktivitetsdatum som fullständiga tidsstämplar enligt ISO 8601, inklusive sekunder, millisekunder och tidszon. Målet avvisar inte längre begäran med `InvalidJson.Json`. Schemalagda aktiviteter går nu över till ett synkroniserat tillstånd i stället för att vara osynkroniserade. (CQ-4360733)

<!--
#### Jetty{#foundation-jetty-65-lts-sp2}

#### Localization{#foundation-localization-65-lts-sp2} 



#### Omnisearch{#foundation-omnisearch-65-lts-sp2}

#### Platform{#foundation-platform-65-lts-sp2}

#### Projects{#foundation-projects-65-lts-sp2}
-->

#### Oak {#foundation-oak-65-lts-sp2}

AEM 6.5 LTS Service Pack 2 kräver S3 Connector 1.60.10 eller senare. S3-datalagerkonfigurationen innehåller nu `crossRegionAccess` och `mode` så att administratörer kan aktivera åtkomst till bucket för flera regioner och växla lagring till GCP vid behov. `s3EndPoint` förväntar sig nu en region som är justerad till `s3Region`, eller så är den tom så att drivrutinen genererar slutpunkten. (GRANITE-64873)


#### Quickstart{#foundation-quickstart-65-lts-sp2}

* Sling uppdaterar tillåtelselista för administratörsinloggning så att den omfattar även terminologi och nya konfigurations-PID:n. Den här ändringen överensstämmer med Sling JCR Base 3.2.0. GRANITE-63756

  **Effekt**

   * Sling tar bort dessa PID:n och du bör ta bort dem från dina konfigurationer:
      * Fabriks-PID: `org.apache.sling.jcr.base.internal.LoginAdminWhitelist.fragment`
      * Global PID: `org.apache.sling.jcr.base.internal.LoginAdminWhitelist`
Dessa äldre konfigurationer använder egenskaper som `whitelist.name` och `whitelist.bundles` .

   * Sling ger fortfarande delvis bakåtkompatibilitet för de borttagna PID:n, men använd dem inte för nya konfigurationer. Använd de nyare `LoginAdminAllowList.*`-PID:na i stället.
   * Kör inte föråldrade konfigurationer och nya konfigurationer för tillåtelselista samtidigt. Blandade konfigurationer kan skapa tvetydighet och skapa oönskat beteende. När du migrerar till AEM 6.5 LTS SP2 tar du bort de borttagna PID:n helt.

  **Vad du bör göra**

   1. Sök efter konfigurationer med tillåtelselista som använder `LoginAdminWhitelist*` PID:n.
   1. Ersätt dem med rätt nya PID:

      * Fabriks-PID: `org.apache.sling.jcr.base.LoginAdminAllowList.fragment`
      * Global PID: `org.apache.sling.jcr.base.LoginAdminAllowList`

      Mer information finns i [Föråldrat tillvägagångssätt för att tillåtslista paket för administrativ inloggning](https://sling.apache.org/documentation/the-sling-engine/service-authentication.html#deprecated-approach-to-allowlist-bundles-for-administrative-login).

* AEM 6.5 LTS SP2 uppdaterar baslagrets paket för Sling, Oak och Felix. Dessa uppgraderingar stärker stabiliteten i runtime-modulen och anpassar beroendeversionerna över hela plattformen. GRANITE-61874

<!--
#### Security{#foundation-security-65-lts-sp2}

AEM now prevents NullPointerException errors when a logged-in user lacks read access for some groups and opens the Groups tab. The tab now hides groups without access and renders group membership details without a blank or unresponsive UI. (NPR-43311) -->

#### Sling{#foundation-sling-65-lts-sp2}

AEM inkluderar nu Sling Engine 2.16.6. Den här ändringen eliminerar XSS-överträdelser som flaggats av säkerhetsverktyg och förbättrar säkerheten och stabiliteten för grundåtergivningen. (NPR-43105)

<!--
#### Translation{#foundation-translation-65-lts-sp2}

#### User interface{#foundation-ui-65-lts-sp2}
-->

#### WCM{#foundation-wcm-65-lts-sp2}

AEM-översättningar fungerar inte längre i Java 17 eller Java 21 på grund av problem med XLIFF-format. Exportflödet producerar nu standardkompatibel XLIFF som översättningsleverantörer accepterar. Den här ändringen tar bort avbrott i översättningsjobb och återställer förutsägbar överlämning mellan AEM och översättningstjänster. Översättningsarbetsflödena är nu stabila över Java-miljöer som stöds. (CQ-4360217)

#### Arbetsflöde{#foundation-workflow-65-lts-sp2}

EmailNotificationService-Processor utlöser inte längre upprepade fel av typen&quot;Segment not found&quot; under hanteringen av arbetsflödesmeddelanden. Den uppdaterade undantagshanteringen identifierar SegmentNotFoundException och stoppar bearbetningsloopen i stället för att fortsätta med ogiltiga läsningar. Arbetsflödeskörningen är stabil och loggbruset minskar vid åtkomst till inkorgen och arbetsobjekt. (GRANITE-62635)




## Om [!DNL Experience Manager Foundation] {#experience-manager-foundation}

Plattformen för [!DNL Adobe Experience Manager] 6.5 LTS bygger på uppdaterade versioner av det OSGi-baserade ramverket (Apache Sling och Apache Felix) och Java™ Content Repository: Apache Jackrabbit Oak 1.68.x.

Eclipse Jetty 11.0.x används som servermotor för QuickStart.

### Stöd för Java™  {#java-support}

* Stöd för Java™ 17 och Java™ 21.
* För optimala prestanda bör du åsidosätta standardvärdena för GC med andra värden. Mer information finns i avsnittet [Installera och uppdatera](/help/sites-deploying/custom-standalone-install.md).
* Adobe distribuerar Java™ 17- och Java™ 21-underhållsuppdateringar för kundanvändning i AEM-relaterade projekt, när dessa inte är tillgängliga för allmänheten från Oracle.

### Uberjar-förpackning {#uber-jar-packaging}

* Det finns en liten skillnad i förpackningen till AEM 6.5 LTS. Mer information finns i [Uppdatera AEM Uber Jar-versionen](/help/sites-deploying/upgrading-code-and-customizations.md#update-the-aem-uber-jar-version).

### Uppgradera {#upgrade}

* Mer information om uppgraderingsproceduren finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).
* Detaljerade uppgraderingsinstruktioner finns i [uppgraderingshandboken för AEM Forms 6.5 LTS SP1 på JEE](https://experienceleague.adobe.com/sv/docs/experience-manager-65-lts/content/forms/upgrade-aem-forms/upgrade)

#### Bästa tillvägagångssätt för AEM 6.5 LTS Service Pack-uppgraderingar

<!-- THE INFORMATION UNDER THIS HEADING CAME FROM CQDOC-23078 -->

**Miljö**
Gäller för: AEM 6.5 LTS-kunder (On-Premise) som installerar Service Pack 2 (SP2). SP2 levereras som en QuickStart JAR.

**Varför det här betyder något**
SP2 för AEM 6.5 LTS levereras som en JAR för snabbstart i stället för som en ZIP för installation via Package Manager. Lokala kunder uppgraderar genom att ersätta Quickstart JAR, packa upp den och starta om. Den här metoden är förenlig med Adobe förfarande för uppgradering på plats.

**Rekommenderat uppgraderingsflöde (författare eller publicera)**

1. Kontrollera att AEM 6.5 LTS-instansen är felfri och tillgänglig.
1. Hämta QuickStart JAR (till exempel `cq-quickstart-6.6.x.jar`) från programvarudistribution.
1. Stoppa den instans som körs.
1. I AEM installationskatalog (utanför `crx-quickstart/`) ersätter du den tidigare JAR-filen för QuickStart med SP2 JAR.
1. Packa upp JAR:

   ```java
   java -jar cq-quickstart-6.6.x.jar -unpack
   ```

   (Justera stackflaggor efter behov.)

1. Byt namn på den opackade JAR så att den matchar rollen och porten, till exempel `cq-author-4502.jar` eller `cq-publish-4503.jar`.
1. Starta AEM och bekräfta uppgraderingen i användargränssnittet (Hjälp > Om) och loggarna.

**God hygien**

* Kör uppgraderingen i testmiljöer före produktionen.
* Ta en fullständig, återställningsbar säkerhetskopiering (databas plus externa datalager) innan du börjar.
* Läs Adobe riktlinjer för uppgradering på plats och tekniska krav (Java 17/21 rekommenderas för LTS).

>[!NOTE]
>
>Filnamn som visas ovan (till exempel `cq-quickstart-6.6.x.jar`) återspeglar den namngivning av QuickStart-artefakter som observerats för den här LTS-versionen. Använd alltid det exakta filnamn som du hämtar från programvarudistributionen.

## Installera och uppdatera{#install-update}

Installationsanvisningar finns i [Installationsanvisningar](/help/sites-deploying/custom-standalone-install.md).

>[!NOTE]
>
> Om du uppgraderar direkt till LTS SP1 från gamla 6.5 SP följer du instruktionerna för 6.5 till 6.5 LTS GA [upgrade](/help/sites-deploying/upgrade.md).


Detaljerade instruktioner finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

>[!NOTE]
>
> För nya AEM 6.5 LTS-installationer måste indexdefinitioner installeras separat. Mer information finns i [den här artikeln](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#index-definitions).

## Installera och uppdatera AEM Forms-tillägg {#install-update-aem-forms-add-on}

Mer information finns i [Utföra en lokal uppgradering](https://experienceleague.adobe.com/sv/docs/experience-manager-65/content/release-notes/aem-forms-current-service-pack-installation-instructions).


## Plattformar som stöds {#supported-platforms}

Hitta den fullständiga matrisen med plattformar som stöds, inklusive supportnivå på [AEM 6.5 LTS Technical Requirements](/help/sites-deploying/technical-requirements.md).

>[!NOTE]
>
>Java™ 17/Java™ 21 rekommenderas för AEM 6.5 LTS.


## Föråldrade och borttagna funktioner {#deprecated-and-removed-features}

<!-- CARRY OVER EACH RELEASE -->

Adobe granskar och utvecklar kontinuerligt produktfunktioner för att ge större kundvärde genom att modernisera eller ersätta äldre funktioner. Dessa ändringar implementeras med noggrant övervägande av bakåtkompatibilitet.

För att säkerställa transparens och möjliggöra korrekt planering följer Adobe den här borttagningsprocessen för Adobe Experience Manager (AEM):

* Föråldringen tillkännages först. Föråldrade funktioner är fortfarande tillgängliga men har inte förbättrats längre.
* Borttagning sker inte tidigare än nästa större release. Tidslinjen för planerad borttagning kommuniceras separat.
* Kunder får minst en releasecykel för att gå över till alternativ som stöds innan en funktion tas bort.

### Föråldrade funktioner {#deprecated-features}

I det här avsnittet listas funktioner som Adobe har ersatt i AEM 6.5 LTS. Normalt tar Adobe bort funktioner innan de tas bort i en framtida version och erbjuder ett alternativ.

Kunderna rekommenderas att granska om de använder funktionen/funktionen i den aktuella distributionen och planera för att ändra implementeringen så att den använder det alternativ som erbjuds.

| Område | Funktion | Ersättning | Version (SP) |
| --- | --- | --- | --- |
| Quickstart | Mongo-API:er | Mongo-API:er är nu föråldrade och är planerade att tas bort i framtida versioner. | 6.5 TS SP2 |
| Sites | Stöd för innehållsfragment i AEM Assets REST API | AEM 6.5 LTS SP2 innehåller moderna OpenAPI:er för Content Fragment och Model Management, så de äldre slutpunkterna för Content Fragment Support i AEM Assets REST API är nu inaktuella.<br>Adobe har för avsikt att hålla dessa äldre slutpunkter tillgängliga tills ett meddelande om att de har upphört att gälla. Adobe planerar inga ytterligare förbättringar för de borttagna slutpunkterna. | 6.5 LTS SP2 |
| Sites | [SPA-redigerare](/help/sites-developing/spa-overview.md) | De redigerare som rekommenderas för att hantera headless-innehåll i AEM är:<br>- [Universell redigerare](/help/sites-developing/universal-editor/introduction.md) för visuell redigering.<br>- [Innehållsfragmentredigeraren](/help/assets/content-fragments/content-fragments-managing.md) för formulärbaserad redigering. | 6,5 LTS GA |
| [!DNL Foundation] | Stöd för com.adobe.granite.oauth.server | Integrering med Adobe IMS |  |

### Borttagna funktioner {#removed-features}

I det här avsnittet listas funktioner som har tagits bort från AEM 6.5 LTS. Tidigare versioner hade dessa funktioner markerats som föråldrade.

* Stöd för RDBMK för CRX-databasbeständighet har tagits bort.
* I klustrade miljöer är nu MongoMK det enda alternativet som stöds för databasbeständighet.

| Område | Funktion | Ersättning | Version (SP) |
| --- | --- | --- | --- |
| Commerce | AEM CIF Classic stöds inte. | Migrera till [AEM CIF](/help/commerce/cif/migration.md). | 6,5 LTS GA |
| Lösningar | Social/Communities stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Screens | Screens stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Assets | `dam-pim` och `dam-rating` stöds inte eftersom paket är beroende av sociala medier. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Assets | `com.day.cq.dam.scene7.api.model.Scene7ViewerConfig#getSettings()` har tagits bort. | Använd den alternativa API `com.day.cq.dam.scene7.api.model.Scene7ViewerConfig#getSettingsList()` som har lagts till. | 6,5 LTS GA |
| Portal | AEM Portal Director stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | Paketet `com.adobe.granite.socketio` har tagits bort. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | `com.adobe.granite.crx-explorer` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Granit | `crx2oak` stöds inte. | Välj den relevanta versionen av [Oak-upgrade](https://mvnrepository.com/artifact/org.apache.jackrabbit/oak-upgrade) | 6,5 LTS GA |
| Adobe | `com.adobe.cq.cq-searchpromote-integration` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Guava | Alla guava-beroenden har nu tagits bort i AEM och därför ingår inte paketet `com.adobe.granite.osgi.wrapper.guava-15.0.0-0002` i AEM. | Kunderna kan själva lägga till guava om de är beroende av guava eller ersätta guava-kod med java-samlingar eller andra alternativ om det är möjligt. | 6,5 LTS GA |
| `We.Retail` | `We-retail` exempelplats stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | Paketet `oak-solr-osgi` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | `org.apache.servicemix.bundles.abdera-parser`, `org.apache.servicemix.bundles.jdom` och `org.apache.sling.atom.taglib` stöds inte. | Det finns ingen ersättningsprodukt. | 6,5 LTS GA |
| Öppna Source | `org.apache.commons.io` paket exporteras nu från `org.apache.commons.commons-io`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `javax.mail` paket exporteras från paketet `com.sun.javax.mail`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `org.apache.jackrabbit.api` paket exporteras nu från paketet `org.apache.jackrabbit.oak-jackrabbit-api`. | Ingen ändring krävs. | 6,5 LTS GA |
| Öppna Source | `com.github.jknack.handlebars` stöds inte | Välj relevant [version](https://mvnrepository.com/artifact/com.github.jknack/handlebars) | 6,5 LTS GA |


## Kända fel {#known-issues}

### Installera nödvändiga Oak-index för Sites Headless API:er{#site-headless-api}

Vissa API:er som har flyttats till Sites Headless kräver ytterligare Oak-index för full funktionalitet.

Installera paketet `cq-dam-cfm-indices` om du vill använda följande funktioner:

* Visa innehållsfragmentmodeller
* Visa innehållsfragment
* Söknings-API
* Arbetsflöden

Hämta indexpaketet [cq-dam-cfm-indices](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/cq-dam-cfm-indices-1.1.2.zip) från Adobe Software Distribution Portal.

### Dispatcher-anslutningsfel med SSL-funktion (åtgärdat i AEM 6.5 LTS SP1 och senare){#ssl-only-feature}

>[!NOTE]
>
> Problemet förekommer endast i AEM 6.5 LTS GA-utgåvan.

När du aktiverar SSL-funktionen i AEM-distributioner finns det ett känt problem som påverkar anslutningen mellan Dispatcher- och AEM-instanserna. När du har aktiverat den här funktionen kan hälsokontroller misslyckas och kommunikationen mellan Dispatcher- och AEM-instanser kan avbrytas. Det här problemet inträffar specifikt när kunder försöker ansluta via `https + IP` från Dispatcher till AEM-instanser. Det är relaterat till SNI-valideringsproblem (Server Name Indication).

**Effekt**

* Misslyckade hälsokontroller med HTTP 400-svarskoder
* Trafiken mellan Dispatcher och AEM har brutits
* Innehåll kan inte hanteras på rätt sätt via Dispatcher
* Anslutningsfel vid användning av HTTPS med IP-adresser i Dispatcher-konfiguration
* HTTP 400: Felet &quot;Invalid SNI&quot; vid anslutning via HTTPS + IP

**Berörda miljöer**

* AEM installerar med Dispatcher-konfigurationer
* System där SSL-funktionen har aktiverats
* Dispatcher-konfigurationer som använder anslutningsmetoden `https + IP` till AEM-instanser

**Lösning**

Kontakta Adobe kundsupport om du får det här problemet. Det finns en snabbkorrigering [cq-6.5.lts.0-hotfix-CQ-4359803](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-CQ-4359803-1.0.2.zip) som åtgärdar det här problemet. Försök inte aktivera SSL-funktioner förrän du har implementerat den nödvändiga snabbkorrigeringen.

## OSGi-paket och innehållspaket som ingår{#osgi-bundles-and-content-packages-included}

Följande textdokument innehåller en lista över de OSGi-paket och innehållspaket som ingår i den här [!DNL Experience Manager] 6.5 LTS-, Service Pack 1-versionen:

* [Lista över OSGi-paket som ingår i Experience Manager 6.5 LTS, Service Pack 1](/help/release-notes/assets/65lts_sp1_bundles.txt) <!-- UPDATE FOR EACH NEW RELEASE -->
* [Lista över innehållspaket som ingår i Experience Manager 6.5 LTS, Service Pack 1](/help/release-notes/assets/65lts_sp1_packages.txt) <!-- UPDATE FOR EACH NEW RELEASE -->

## Begränsade webbplatser{#restricted-sites}

Dessa webbplatser är bara tillgängliga för kunder. Kontakta din kontoansvarige på Adobe om du är kund och behöver åtkomst.

* [Nedladdning av produkt på licensing.adobe.com](https://licensing.adobe.com/)
* [Kontakta Adobe kundsupport](https://experienceleague.adobe.com/sv/docs/support-resources/adobe-support-tools-guide/adobe-customer-support-experience).

