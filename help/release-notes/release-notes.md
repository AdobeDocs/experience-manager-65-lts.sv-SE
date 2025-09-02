---
title: Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS, SP1
description: Aktuell versionsinformation om Adobe Experience Manager 6.5 LTS, Service Pack 1.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: b5a8f555-c061-4fe2-a100-cc01335959cb
source-git-commit: e9fc4a6294588b527a3b19d64101c81f0eb7bf55
workflow-type: tm+mt
source-wordcount: '5238'
ht-degree: 0%

---

# Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS, SP1 {#release-notes}

## Versionsinformation {#release-information}

| Produkt | [!DNL Adobe Experience Manager] 6.5 LTS |
|---|---|
| Version | Service Pack 1 (SP1) <!-- UPDATE FOR EACH NEW RELEASE --> |
| Typ | Service Pack-version |
| Datum | 28 augusti 2025 <!-- UPDATE FOR EACH NEW RELEASE --> |
| Hämta URL | [Programdistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack-lts/cq-quickstart-6.6.1.jar) |

<!-- UPDATE ABOVE FOR EACH NEW RELEASE -->

## Vad ingår i [!DNL Adobe Experience Manager] 6.5 LTS, SP1 {#what-is-new}

<!-- UPDATE EACH RELEASE -->

[!DNL Experience Manager] 6.5 LTS, SP1 innehåller nya funktioner, viktiga förbättringar som kunden efterfrågat samt felkorrigeringar. Det innehåller även förbättringar av prestanda, stabilitet och säkerhet som släppts sedan den första tillgängligheten av 6,5 LTS i mars 2025. [Installera detta Service Pack](#install-update) på 6.5 LTS.

<!-- ## Key features and enhancements -->

<!-- 6.5 LTS REVIEWERS: WHAT ARE THE KEY FEATURES AND ENHANCEMENTS THAT YOU WANT TO HIGHLIGHT IN THIS RELEASE? -->

<!-- UPDATE EACH RELEASE -->

## Åtgärdade problem i 6.5 LTS, Service Pack 1 {#fixed-issues}

<!-- UPDATE BELOW FOR EACH NEW RELEASE -->

### [!DNL Sites]{#sites-65-LTS-SP1}

#### Tillgänglighet {#sites-accessibility-65-lts-sp1}

* Ett problem där textredigeringselementet i innehållsfragment kortades av som standard har åtgärdats. Textredigeraren visar nu det fullständiga innehållet utan trunkering. (SITES-33005)
* Ett problem har korrigerats där klickning på URL-sökvägar öppnade Indigos hemsida i stället för rätt mål-URL. (SITES-33004)
* Korrigerade ett tillgänglighetsproblem i en anpassad AEM-komponent som orsakade ADA-kompatibilitetsfel under automatisk testning. (SITES-30660)
* ADA-kompatibilitetsproblem i varningsdialogrutan och arbetsflödesmeddelanden har åtgärdats genom att texten visas i svart på ljusa bakgrunder och uppfyller WCAG 2.0-kontrastkraven. (SITES-30138)
* Korrigerade ett hjälpmedelsproblem där kategoriknappen i AEM Sites-redigeraren saknade en viss etikett, vilket fick JAWS att meddela att den var&quot;Bild-knappmeny&quot; i stället för att ge en beskrivande etikett. (SITES-27497)
* Ett hjälpmedelsproblem har korrigerats där bockmarkeringsikonerna på skärmen Effektiva behörigheter saknade alternativ text, vilket hindrade JAWS från att läsa och förmedla sin betydelse. (SITES-27272)
* Ett tillgänglighetsproblem har korrigerats där sidan Behörigheter inte gav ett tydligt JAWS-meddelande för borttagning av användar- eller gruppbehörigheter, vilket förvirrade skärmläsaranvändare. (SITES-27238)
* Ett hjälpmedelsproblem har korrigerats där felmeddelanden endast visades som ikoner utan beskrivande text, vilket förhindrar JAWS från att meddela felen när de uppstod. (SITES-27155)
* Korrigerade ett hjälpmedelsproblem där JAWS läste felaktiga och otydliga knappbeskrivningar i AEM On-Premise-miljön, inklusive saknade rubriknivå 2-text för modulavsnittet. (SITES-27152)
* Korrigerade ett hjälpmedelsproblem där JAWS inte meddelade antalet resultat efter filtrering av värden på fliken AEM Assets. (SITES-27150)
* Korrigerade ett tillgänglighetsproblem där det inte gick att visa en bekräftelse när en mapp skapades och JAWS inte meddelade det i Assets-gränssnittet. (SITES-27141)
* Korrigerade ett tillgänglighetsproblem i AEM Sites. JAWS meddelade inga formulärfel, fokus flyttades till icke-interaktiva felelement och det gick inte att visa obligatoriska fältfel vid tabbout. (SITES-27138)
* Korrigerade ett hjälpmedelsproblem där knappmenyn Tidslinjer saknade en specifik etikett, vilket gjorde att JAWS läste enbart &quot;knappmeny för aktiviteter&quot; utan att ge en tydlig beskrivning. (SITES-27134)
* Korrigerade ett tillgänglighetsproblem där JAWS inte meddelade åtgärden eller rollen för behållarobjekt, där endast läsningen &quot;Breadcrumb v1&quot; och &quot;button v2&quot; lästes i stället för beskrivande etiketter. (SITES-27131)
* Korrigerade ett tillgänglighetsproblem där popup-fönstret för snabb publicering inte gav något korrekt meddelande, vilket hindrade skärmläsare från att ge feedback. (SITES-26912)
* Korrigerade ett tillgänglighetsproblem i koralkolumnsvyn där element med ARIA-roller som kräver underordnade roller inte innehöll dem, vilket medförde att tillgänglighetsstandarderna inte följdes. (SITES-26898)
* Ett hjälpmedelsproblem har korrigerats där text för mallar och egenskaper i den översta navigeringen på skapandesidan inte var synliga i flödesomformningsläge, vilket förhindrar åtkomst för användare av tangentbord och skärmläsare. (SITES-26895)
* Korrigerade ett hjälpmedelsproblem där verktygstips för ikonerna Sök, Lösning, Hjälp, Inkorg och Användare i den översta navigeringen inte var tillgängliga via tangentbordsnavigering. (SITES-26889)
* Ett tillgänglighetsproblem har korrigerats där formulärfält på fliken Grundläggande inte gav några felförslag, vilket hindrade användarna från att få vägledning när obligatoriska inmatningsfält lämnades tomma. (SITES-26885)
* Korrigerade ett tillgänglighetsproblem där skärmläsare för NVDA och Skärmläsare inte berättade mallfilsinformation på sidan Skapa, vilket hindrade användarna från att ta emot fullständig information programmatiskt. (SITES-26884)
* Ett problem med tillgänglighet som innebar att ett felaktigt namn för textrutan Sidrubrik användes på fliken Grundläggande har korrigerats, vilket hindrade skärmläsare från att tillhandahålla korrekt information till användarna. (SITES-26879)
* Uppdaterade för- och bakgrundsfärger för knappar för att uppfylla WCAG 2.2 A-kraven på minsta kontrastförhållande, vilket förbättrar läsbarheten och tillgängligheten för alla användare. (SITES-26877)
* Löste ett problem som gjorde att texterna &quot;template&quot; och &quot;properties&quot; i den övre navigeringen på skapandesidan försvann efter storleksändringen, vilket säkerställde synlighet och tillgänglighet för synskadade. (SITES-26872)
* Korrigerade ett problem som gjorde att flera vågräta rullningslister visades på huvudsidan efter att en flödesomformning tillämpats, vilket innebar att en rullningslist visades för att ge korrekt tillgänglighet och synlighet för innehållet. (SITES-26800)
* Korrigerade ett tillgänglighetsproblem i AEM Page Editor där tangentbordsfokus oväntat återställs till början av det demografiska verktygsfältet efter att knappar som Persona, Kart eller Övergiven har aktiverats. Fokus ligger nu kvar på den aktiverade knappen för att ge stöd för enhetlig tangentbordsnavigering och arbetsflöden för skärmläsare. (SITES-25306)
* Ett problem med associationen för hjälpmedelsetiketter för sidrubrik- och taggfält har åtgärdats. AEM-gränssnittet kopplar nu hjälpmedelsetiketter korrekt till fälten Rubrik och Sidtitel när du använder skärmläsare som JAWS. Korrigeringen säkerställer korrekt läsning av etiketter och förbättrar ADA-kompatibiliteten för sidgenerering, egenskaper och arbetsflöden. (SITES-27149)
* Korrigerat saknad visuell etikett för kommentarinmatningsfält på tidslinjen. Korrigerade saknade visuella etiketter för inmatningsfält för kommentarer under tidslinjen för att förbättra tillgängligheten. Uppdateringen ser till att skärmläsare kan meddela fältetiketterna korrekt. Den här upplevelsen förbättrar formulärnavigering och överföring för alla användare, särskilt de som använder hjälpmedelstekniker. (SITES-26903)
* Korrigerad tangentbordstillgänglighet för ellipsknappen i tidslinjekommentarer. Aktiverad tangentbordsnavigering för ellipsknappen (tre punkter) bredvid kommentarer under tidslinjeavsnittet. Användarna kan nu komma åt och interagera med knappen med tabbtangenten, vilket förbättrar tillgängligheten för användare som bara använder tangentbordsnavigering. (SITES-26891)
* Förbättrade NVDA-/Skärmläsarmeddelanden för sökresultat i urvalsdialogrutor. Uppdaterade dialogrutan Öppna markering för att meddela om sökresultat hittas eller inte när du använder skärmläsare, t.ex. NVDA eller Skärmläsaren. Denna förbättring hjälper användare som förlitar sig på hjälpmedelstekniker att förstå resultatet av sina sökåtgärder utan att behöva någon visuell bekräftelse. (SITES-26883)
* ARIA-rollen för ellipsikonen har korrigerats bredvid kommentarinmatningsfältet. Ellipsikonen (tre punkter) bredvid kommentarinmatningsfältet har uppdaterats så att rätt ARIA-roll används, vilket gör att skärmläsare kan identifiera elementet korrekt. Den här förbättringen förbättrar tillgängligheten och förbättrar upplevelsen för användare som använder hjälpmedelstekniker. (SITES-26881)
* Ogiltiga ARIA-attribut i Coral UI-komponenter har korrigerats. Uppdaterade Coral UI-komponenter för att säkerställa att alla ARIA-attribut använder giltiga värden, vilket förbättrar tillgängligheten. I synnerhet har fall där ogiltiga värden som `aria-modal="dialog"` har tilldelats felaktigt åtgärdats. Den här förbättringen gör att skärmläsare kan tolka element i dialogrutor korrekt, vilket förbättrar tillgängligheten för användare som använder hjälpmedelstekniker. (SITES-26873)
* Förbättrad synlighet och verktygstips för ikoner i Reflow-scenarier. Förbättrat Reflow-beteende för att verktygstipsen ska visas korrekt för ikonerna **Hämta**, **Bearbeta resurser** och **Checka ut**. Fokuseras på ett hjälpmedelsproblem där ikoner och deras etiketter blev osynliga när visningsrutans storlek ändrades eller webbläsarens zoominställningar ändrades. Den här korrigeringen stöder användare med nedsatt syn genom att bibehålla synligheten och tillhandahålla korrekta ikonbeskrivningar under Reflow. (SITES-26871)


#### Administratörsgränssnitt{#sites-adminui-65-lts-sp1}

* Korrigerade ett tillgänglighetsproblem där JAWS inte meddelade några listroller eller tillhandahöll instruktioner för navigering och aktivering i dialogrutan Skapa plats. (SITES-30661)
* Skärmläsarstöd för statusmeddelanden i webbplatsfiltervyn fungerar som förväntat, vilket garanterar att användarna får tydlig och snabb feedback när de växlar mellan vyer. (SITES-24992)
* Datumväljaren i filterfältet visas helt i behållaren, vilket ger rätt storlek på pekmålet och eliminerar urklippsproblem. (SITES-24988)
* De valda filtertaggarna använder nu semantiska HTML- och ariemärkningar som matchar den visuella presentationen, vilket ger korrekt rollstöd och tydliga åtgärder för hjälpmedelstekniker. (SITES-24980)
* Lagt till ett aria-label-attribut i Reference Rail-regionen för att ge en unik, beskrivande etikett för skärmläsaranvändare och säkerställa en enhetlig landmärkesidentifiering på hela sidan. (SITES-24973)
* Referenser Rail har uppdaterats för att använda relativa enheter för storleksändring och positionering, vilket gör att innehållet kan skalas och förbli fullt funktionellt när det zoomas till 400 % vid en visningsruta på 1 280 x 1 024. (SITES-24972)
* Bekräftade tabellelement i webbplatsens hemsideslistvy innehåller korrekta kolumnrubrikroller, vilket gör att skärmläsare kan meddela rubriker för varje datacell. (SITES-24942)
* NVDA presenterar nu datumet Modified i Tree-katalogen, vilket säkerställer att skärmläsaranvändare får fullständig information om resursen. (SITES-24782)
* Ett problem har korrigerats där skärmläsaren NVDA meddelade ofullständig text för objekt i komponenten Tree Directory i AEM Sites. NVDA läser nu fullständig text för varje objekt, vilket förbättrar tillgängligheten och regelefterlevnaden. (SITES-24780)
* Tangentbordstillgänglighet har lagts till i fönsterdelaren i trädkatalogen, vilket gör att användarna kan ändra storlek på den vänstra raden med bara ett tangentbord. (SITES-24779)
* Uppdaterade sökresultat på hjälpmenyn för att inkludera korrekta ARIA-roller för listobjekt, vilket säkerställer att skärmläsarna meddelar länkar på rätt sätt för att förbättra tillgängligheten. (SITES-24729)
* Korrigerade ett problem där skärmläsare inte meddelade statusmeddelandet &quot;X of Y results&quot;. Du kan också skicka meddelandet&quot;inga resultat hittades&quot; när du har använt filter på panelen Platsfilter, så att användarna får en korrekt bekräftelse på resultatet. (SITES-24720)
* Åtgärdade saknade rolltilldelningar för navigeringslänkar i appnavigeringen. Lämpliga ARIA-roller har lagts till för att skärmläsare ska kunna identifiera och informera om navigeringselement. (SITES-24719)
* Felaktig kod för stödrasterroll för markerade filtertaggar har ersatts med knappelement och hjälpmedelsnamn, vilket säkerställer att skärmläsarna talar rätt om och identifierar taggarna. (SITES-24717)
* Skärmläsarmeddelanden för statusmeddelandet Reference Rail har lagts till när flera markeringar utförs, vilket säkerställer att användarna får bekräftelse på ändringarna. (SITES-24678)
* Korrigerat sökfältbeteende så att det första resultatet inte annonseras automatiskt. Skärmläsare meddelar nu hur många resultat som hittas, så att användarna kan navigera i listan utan felaktiga fokusmeddelanden. (SITES-24658)
* Felaktiga `aria-label`-attribut har tagits bort från icke-interaktiva statiska element i listvyn för att förhindra att skärmläsare meddelar vilseledande eller irrelevant information. (SITES-24515)
* Kryssrutan i den första kolumnen i listvyn har uppdaterats så att kolumntexten Rubrik används som hjälpmedelsnamn, vilket gör att skärmläsare kan förmedla syftet med formulärfältet. (SITES-24514)
* Lagt till korrekta ARIA-attribut och stöd för live-regioner för att meddela inläsningsstatusmeddelanden till skärmläsaranvändare när de navigerar i innehållet. (SITES-24481)
* Uppdaterad responsiv design som eliminerar vågrät rullning när innehållet zoomas till 400 % med en visningsrutebredd på 1 280 × 1 024, vilket ger full synlighet utan sidbläddring. (SITES-24308)
* Fokusnavigeringen i administrationsgränssnittet för webbplatser har korrigerats för att följa en logisk ordning och sedan återgå till fokus till knappen&quot;Markera allt&quot; efter att du tryckt på Esc och flyttat fokus till nästa interaktiva element efter att du tryckt på Tabb. (SITES-24307)
* Fokusordningen har uppdaterats i användargränssnittet för Sites Admin så att knappen för vägbeskrivningar i elementet `<betty-titlebar-title>` får fokus i rätt ordning under tangentbordsnavigering. (SITES-24305)
* Verifierad funktion för att hoppa över länkar för att säkerställa att tangentbordsfokus flyttas till huvudinnehållsområdet, vilket gör att tangentbordsanvändare kan kringgå rubrikelement och komma åt innehåll på ett effektivt sätt. (SITES-24061)


#### Klassiskt användargränssnitt{#sites-classicui-65-lts-sp1}

Korrigerade ett problem i Classic UI där kryssruteetiketter saknades och HTML visades som kodad text i flera gränssnittselement, inklusive Datumsökning och gränssnitt som inte är standard. (SITES-31822)

#### [!DNL Content Fragments]{#sites-contentfragments-65-lts-sp1}

AEM förhindrar nu prestandaförsämring på grund av felaktiga XMP-metadata i bildresurser. Assets som innehåller ogiltiga eller icke-kompatibla egenskapsnamn för XMP, t.ex. de med numeriska segment eller okvalificerade strukturer, utlöser inte längre upprepade varningsloggar under bearbetningen. Systemet filtrerar bort problematiska metadata för att säkerställa att inmatning och validering av resurser är slutförd utan fel. (SITES-30683)

<!--
#### [!DNL Content Fragments] - Admin{#sites-admin-65-lts-sp1} -->

#### [!DNL Content Fragments] - Fragmentredigerare{#sites-fragments-editor-65-lts-sp1}

Andra författare kan fortfarande publicera innehållsfragment även när en annan författare checkar ut dem, vilket strider mot det avsedda beteendet för utcheckningsfunktionen. Den här korrigeringen förhindrar att andra användare ser eller använder publiceringsknapparna i redigeringsgränssnittet när ett innehållsfragment är utcheckat. (SITES-30578)

<!--
#### [!DNL Content Fragments] - GraphQL API {#sites-graphql-api-65-lts-sp1}

#### [!DNL Content Fragments] - GraphQL Query Editor{#sites-graphql-query-editor-65-lts-sp1}

#### [!DNL Content Fragments] - REST API{#sites-restapi-65-lts-sp1} -->

#### Komponentkonsol{#sites-component-console-65-lts-sp1}

Korrigerade ett fel i produktlistkomponenten där kryssrutan &quot;Välj alla&quot; endast lade till de första 20 SKU:erna från den första sidan i stället för alla SKU:er i sökresultaten. (SITES-29191)

#### Core backend{#sites-core-backend-65-lts-sp1}

Felaktigt formaterade XMP-metadata utlöste ett fel vid bearbetning av bildresurser i `ValidationDataServlet`. Korrigeringen säkerställer kompatibel metadatahantering och undviker redundant parsning av ogiltiga egenskaper. (SITE-30683)

<!--
#### Core Components{#sites-core-components-65-lts-sp1}

#### Campaign integration{#sites-campaign-integration-65-lts-sp1}

#### Experience Fragments{#sites-experiencefragments-65-lts-sp1}

#### Foundation Components (Legacy){#sites-foundation-components-legacy-65-lts-sp1}

#### Launches{#sites-launches-65-lts-sp1}

#### Link Checker{#sites-link-checker-65-lts-sp1} -->

#### MSM - Live-kopior{#sites-msm-live-copies-65-lts-sp1}

* Korrigerade ett JavaScript-fel `ns.ui.alert is not a function` som uppstod när spökkomponentarv återaktiverades i AEM 6.5 On-prem. (SITES-31993)
* Ett problem har korrigerats där alternativet &quot;Senare&quot; med utrullning tillät fortsatt arbete utan att ett datum i AEM 6.5 valdes. (SITES-31374)

#### Sidredigerare{#sites-pageeditor-65-lts-sp1}

* Löste ett problem i Teaser Modal där fliken Link &amp; Actions fortsatte visa felformatering, ikoner och attributet aria-invalid efter giltig datainmatning och fellösning. (SITES-25527)
* Korrigerade ett fel i Teaser Modal-textredigeraren där knapparna Listor och Stycke inte visade sitt utökade eller komprimerade läge för skärmläsare, vilket säkerställde korrekta, utökade attributuppdateringar. (SITES-25365)
* Korrigerade ett problem i verktygsfältet Demografi där justeringen av kundvagnsreglaget med tangentbordsinmatning flyttade fokus till kundvagnsknappen i stället för att behålla fokus på skjutreglaget, vilket förbättrade navigeringseffektiviteten för tangentbordsanvändare. (SITES-25324)
* Ett hjälpmedelsnamn har lagts till i Cart Slider i verktygsfältet Demographics (Demografi) genom att ett värde tilldelas till det associerade `<label>`-elementet. Den här korrigeringen förbättrar kompatibiliteten med hjälpmedelstekniker och förbättrar användbarheten för skärmläsaranvändare. (SITES-25322)
* Lagt till ARIA-roller och tillgängliga namn för knappar i listrutan Demographics Toolbar. Den här korrigeringen gjorde att hjälpmedelstekniker kunde identifieras korrekt och att tangentbords- och skärmläsaranvändare kunde navigera på ett bättre sätt. (SITES-25315)
* Justerade layouten för det demografiska verktygsfältet för att förhindra att innehåll flödar över från visningsrutan vid 200 % webbläsarzoom, vilket säkerställer att alla kontroller är tillgängliga utan vågrät rullning. (SITES-25309)
* Fokushanteringen i det demografiska verktygsfältet har korrigerats för att behålla tangentbordsfokus på den aktiverade knappen i stället för att fokus återställs till verktygsfältets startposition. (SITES-25306)
* Knappetiketten överlappar funktioner som de är utformade, och använder ett verktygstips för att visa etiketten när lägen med liknande skärmbredder är aktiva. (SITES-25285)
* Anteckningens modala innehåller en synlig skicka-knapp, som gör att användare kan skicka anteckningar utan att behöva använda Esc-tangenten eller klicka utanför modala. (SITES-25281)
* Den modala anteckningsmetoden innehåller en pennikonknapp som gör att användarna kan skicka anteckningar, vilket ger en tydlig och tillgänglig överföringsmetod. (SITES-25269)
* Skärmläsarmeddelanden för knapparna Anteckningar och Stäng anteckningar har korrigerats, vilket ger korrekt och relevant feedback och tar bort orelaterad eller förvirrande information. (SITES-25268)
* Avsnitten på arbetsytan på AEM Editor har nu stöd för fullständig tangentbordstillgänglighet. Användarna kan bara aktivera avsnittsrubriker och redigera knappar med hjälp av tangentbordet, utan att behöva använda muspekaren. Uppdateringen säkerställer överensstämmelse med WCAG 2.1.1 och förbättrar användbarheten i alla komponenter (t.ex. Teaser, Image, Carousel, Layout, Timewarp och Annotation). (SITES-25256)
* Onödig vågrät rullning i Carousel Modal vid en bredd på 320px har tagits bort för att säkerställa att allt innehåll visas i visningsrutan utan att sidnavigering krävs. (SITES-25254)
* Onödig vågrät rullning i bildmodulen vid en bredd på 320px har tagits bort för att säkerställa att allt innehåll visas i visningsrutan utan att sidnavigering krävs. (SITES-25244)
* Onödig vågrät rullning i Teaser Modal med bredden 320px har tagits bort för att säkerställa att allt innehåll visas i visningsrutan utan att sidnavigering krävs. (SITES-25242)
* Tangentbordsnavigering har aktiverats för popup-menyn `List` och `Paragraph Format`, båda i Teaser Modal. Med den här korrigeringen kan användare komma åt och navigera i dessa menyer med piltangenterna. (SITES-25235)
* Korrigerade skärmläsarmeddelanden för knapparna Anteckningar och Stäng anteckningar för att ge korrekt, relevant feedback anpassad till de associerade åtgärderna. (SITES-25234)
* Förbättrad etikett på hjälpknappen i Teaser Modal för att beskriva dess syfte tydligt och ge alla användare meningsfull kontext, inklusive användare av hjälpmedel. (SITES-25224)
* Förbättrad emulatorlinjal för skärmläsaranvändare genom att koppla linjalens mått till deras respektive enheter. Om du dessutom ersätter verktygstipset med ett arraybeskrivande element. (SITES-24955)
* Ingen korrigering implementerades eftersom knappen Redigera fungerar som tänkt och ger information i stället för att utföra en åtgärd. (SITES-24950)
* Den bekräftade fokusordningen på redigeringssidan följer en logisk sekvens, vilket gör att användarna kan navigera genom alla interaktiva element utan att hoppa över eller hoppa tillbaka oväntat. (SITES-24937)
* Arbetsytan i förhandsgranskningsläget har uppdaterat textmellanrummet korrekt när användare använder anpassade inställningar för textmellanrum, vilket ger en konsekvent formatering i alla innehållsområden. (SITES-24936)
* Den verifierade knappen Förhandsgranska utlöser inte längre kontext- eller lägesändringar i fokus, vilket säkerställer att användarna aktiverar knappen avsiktligt innan sidvyn uppdateras. (SITES-24784)
* Korrekta ARIA-rolltilldelningar har lagts till i appnavigeringslänkar, vilket gör att skärmläsare kan identifiera och meddela navigeringsobjekt för förbättrad tillgänglighet. (SITES-24718)
* Uppdaterade knappen Ändra filter för att meddela expanderade och komprimerade lägen till skärmläsare, borttagna överflödiga ARIA-attribut och justerad etikett för att tillhandahålla tydliga, icke-duplicerade beskrivningar. (SITES-24713)
* Skärmläsarmeddelanden för sökresultatstatusmeddelanden har lagts till i dialogrutan för val av länk, vilket garanterar att användarna får en bekräftelse när resultaten läses in eller inga träffar hittas. (SITES-24700)
* Skärmläsarmeddelanden om inläsningsstatus för Image Modal har lagts till, vilket säkerställer att användarna får feedback när modal läses in och är redo för interaktion. (SITES-24697)
* Löste ett tillgänglighetsproblem där den klibbiga rubriken skymde Teaser Modal-innehållet med zoomning på 200 % och 400 %, vilket gav fullständig synlighet och användbarhet vid sidförstoring. (SITES-24523)
* Ett statusmeddelande med antalet sökresultat har lagts till i fältet Sök/Filter, vilket gör att skärmläsare kan meddela användarna resultatet. (SITES-24506)
* Skärmläsarmeddelanden om antalet sökresultat i fältet Sök/Filter har lagts till så att användarna får omedelbar feedback när resultaten läses in. (SITES-24505)
* Ett hjälpmedelsnamn har lagts till i fliklistan på panelen Side Rail, vilket gör att skärmläsare kan meddela sitt syfte i enlighet med riktlinjerna för WAI-ARIA. (SITES-24492)
* Beskrivande etiketter har lagts till i tvetydiga redigeringsikoner, vilket säkerställer att alla användare förstår varje knapps funktion. (SITES-24480)
* Aktivera fullständig tangentbordstillgänglighet för avsnittsrubriker och åtgärdsknappar i arbetsytevyn, vilket ger en konsekvent användning för mus- och tangentbordsanvändare. (SITES-24479)

<!--
#### Replication{#sites-replication-65-lts-sp1}

#### Rich Text Editor{#sites-rte-65-lts-sp1} -->

#### Universell redigerare {#sites-universal-editor-65-lts-sp1}

* Korrigerade ett konkurrensvillkor i QueryTokenService som orsakade felaktiga inloggningar när flera begäranden med frågeparametrar utlöstes innan tjänsten login-token returnerade ett resultat. (SITES-30659)
* Korrigerade ett fel i UniversalEditorURLService där en matris med mappade sökvägar sparades i Felix ConfigMgr endast för det första objektet. (SITES-30292)

### [!DNL Assets]{#assets-65-lts-sp1}

Ett problem har korrigerats där synkronisering av resurser från fjärr-DAM till platsens lokala AEM tog bort den publicerade statusen och replikeringsrelaterade egenskaper från resurserna. (Assets-48958)

<!--
#### [!DNL Dynamic Media]{#assets-dm-65-lts-sp1}

#### [!DNL Dynamic Media] - Hybrid Mode {#assets-dm-hybrid-65-lts-sp1}



### [!DNL Forms]{#forms-65-lts-sp1}


#### Forms Designer 

#### Forms

#### Forms JEE 
 
#### Forms Captcha {#forms-captcha-65-lts-sp1} 

#### XMLFM {#forms-xmlfm-65-lts-sp1}

#### [!DNL Adaptive Forms] {#adaptive-forms-65-lts-sp1}

#### [!DNL Forms Designer] {#forms-designer-65-lts-sp1} -->



### Foundation {#foundation-65-lts-sp1}

<!--
#### Apache Felix {#foundation-apachefelix-65-lts-sp1}

#### Campaign{#foundation-campaign-65-lts-sp1}

#### Cloud Services{#foundation-cloudservices-65-lts-sp1}



#### Communities {#foundation-communities-65-lts-sp1}

#### Content distribution{#foundation-content-distribution-65-lts-sp1}

#### CRX {#foundation-crx-65-lts-sp1}

#### Granite{#foundation-granite-65-lts-sp1} -->

#### HTL{#foundatoin-htl-5-lts-sp1}

Löste OSGi-beroendecykler som hindrade HTL-skriptmotorfabriken från att fungera, vilket säkerställer en korrekt serviceupplösning och skriptkörning. (Granite-58275)

#### Integreringar{#foundation-integrations-65-lts-sp1}

* Tog bort användningen av Commons-httpclient 3.x från paketet `com.adobe.cq.cq-analytics-integration` och ersatte det med `org.apache.httpcomponents.httpclient` 4.5.13.B0001 för att passa in i den senaste AEM 6.5 LTS-standarden. (CQ-4360586)
* Tog bort det inaktuella Search&amp;Promote-integreringspaketet från AEM för att eliminera oanvända komponenter och minska underhållskostnaderna. (CQ-4358030)
* Nya backend-testfall för SiteCatalyst-integrering har lagts till för att förbättra analysvalideringen och säkerställa en mer omfattande täckning. (CQ-4359991)
* Ett problem har korrigerats i avsnittet Egenskaper för Launch Config där listrutorna Company och Property inte visades. Dessutom utlöste Spara och stäng fel trots att alla obligatoriska fält fylldes i och felaktiga felmeddelanden visades för Company och Property när endast fältet Title var tomt. (CQ-4359853)
* Sökvägsposten `searchpromote` för servertjänsten har tagits bort från version 6.6 för att anpassas till borttagningen av paketet Search&amp;Promote. (CQ-4359523)
* Korrigerade HTTP-testfall för måldatabasen för att säkerställa korrekt validering och förbättrad testtillförlitlighet. (CQ-4359022)
* Borttagen Guava-cachningsanvändning från modulen integration-adobeims-console för att eliminera Guava-biblioteksberoenden. (CQ-4358710)
* Validerade arbetsflöden för DTM-integrering, inkorgsuppgifter och projektfunktioner i AEM 6.6 för att säkerställa att AEM 6.5 fungerar korrekt. (CQ-4358151)
* Validerade Content Insight-funktioner i AEM 6.6 för att säkerställa kompatibilitet och korrekt funktion i AEM 6.5. (CQ-4357774)
* Validerade molntjänster i AEM 6.6 för att säkerställa kompatibilitet och korrekt funktion i AEM 6.5. (CQ-4357773)
* Validerad integrering av Adobe IMS Console i AEM 6.6 för att säkerställa kompatibilitet och korrekt drift i AEM 6.5. (CQ-4357772)
* Jenkins-pipeline har uppdaterats för att Test&amp;Target-integreringen ska kunna köras på Java 17, misslyckade Selenium-tester har åtgärdats, utvalda tester flyttas till Playwright och alla enhetstester har godkänts. (CQ-4357770)
* Justerade DX-integreringar, arbetsflöden, inkorgar och projekt med 6.6.0-grenen genom att uppdatera bygg- och teströrledningarna. Du kan även lösa problem med uppgraderingskompatibilitet och validera alla berörda tjänster för att få stabilitet och funktionalitet. (CQ-4357767)

<!--
#### Jetty{#foundation-jetty-65-lts-sp1} -->

#### Lokalisering{#foundation-localization-65-lts-sp1}

* De lokaliserade strängarna i dialogrutan Ta bort åtkomstkontroll från listan Behörigheter för att visa rätt översättningar. (GRANITE-59427)
* Ett problem har korrigerats i dialogrutan Lägg till regel i modellredigerarens &quot;eller Delade egenskaper&quot; där flera gränssnittssträngar, inklusive operatorer och fältetiketter, verkade vara olokaliserade. Alla strängar visas nu med korrekt lokalisering. (CQ-4354014)
* En översättning som saknas för verktygstipset Visa beskrivning för har lagts till i dialogrutan Redigera arbetsflödesmodeller. (CQ-4347996)

#### Oak {#foundation-oak-65-lts-sp1}

Ett problem där AEM återskapade eller ändrade namn på befintliga konfigurationsfiler under `/apps/system/config` under uppgraderingarna löstes och `.cfg.json`-filer ersattes med `.config`-filer. (GRANITE-58899)

#### Omnisearch{#foundation-omnisearch-65-lts-sp1}

Ett hjälpmedelsproblem har korrigerats där platshållare felaktigt visades som etiketter för inmatningsfält. Det här problemet orsakar saknade fältetiketter i sökningar, AEM Experience Fragments, Content Fragments och Content Fragment Models. (Granite-61791)

<!--
#### Platform{#foundation-platform-65-lts-sp1} -->

#### Projekt{#foundation-projects-65-lts-sp1}

* Ett problem som visade ett felaktigt popup-fönster när ett projekt togs bort i kalendervyn har åtgärdats, trots att projektet tagits bort. (CQ-4358890)
* Ett problem har korrigerats i Firefox där kortets sidfot för&quot;Resurssamling&quot; i projektvyn överlappade kortets kantlinje. Sidfoten justeras nu korrekt utan överlappning. (CQ-4353317)

#### Quickstart{#foundation-quickstart-65-lts-sp1}

* Avinstallationsskriptet har uppdaterats för att justera versionsområdet för Guava-paketet och förhindra att det blocklist när det installeras via pakethanteraren. (GRANITE-59559)
* Ett problem i replikeringsgränssnittet som visade ett fel (`#1660`) vid redigering av replikeringsagenter har korrigerats genom hantering av klassiska kryssrutor i gränssnittet. GRANITE-58302
* Löste flera startfel för S3-datalagret när AEM 6.5 LTS kördes med JDK 21 genom att åtgärda saknade tjänstbehörigheter, uppdatera konfigurationshanteringen och säkerställa att nödvändiga tjänster initieras korrekt. (GRANITE-57082)
* Definierade underhålls- och underhållsstrategin för AEM 6.5. Den här korrigeringen innehåller följande:
   * Service Pack-licens.
   * Hotfix-cadence.
   * Parallellt stöd med AEM 6.6.
   * Uppdaterad supportmatris.
   * Tilläggsansvar för ägarskap. (GRANITE-50459)

<!--
#### Security{#foundation-security-65-lts-sp1} -->

#### Sling{#foundation-sling-65-lts-sp1}

* Sling ResourceAccessSecurity har uppdaterats till version 1.1.2 för att matcha en `ClassCastException` som inträffade när flera `ResourceAccessGate` referenser initierades `ResourceAccessSecurityImpl`. (NPR-42750)
* Korrigerade ett problem i Adobe Stock-integreringen där licensdialogrutan blev nedtonad. Problemet berodde på att inmatningsfälten togs bort av funktionen `sunt:initList`. Funktionen hittades i Coral Foundation-klientbiblioteken. Klientbiblioteken har uppdaterats för att behålla de fält som krävs, vilket aktiverar rätt funktioner för licensdialogrutor. (NPR-42748)
* Korrigeringen för Sling Scripting-problem som orsakade `DataTimeParseException`- och `String.length()` null-pekarundantag under paketinstallationen stöds. Sling Scripting har uppdaterats till version 2.8.3-1.0.10.6 för att minska antalet installationsfel och förbättra stabiliteten. (NPR-42640)

<!--
#### Translation{#foundation-translation-65-lts-sp1} -->

#### Användargränssnitt{#foundation-ui-65-lts-sp1}

* Löste ett problem i användargränssnittet för AEM Author som begränsade visningen av användargrupper till 41. Det här problemet berodde på en standardbatchgräns i gruppväljarkomponenten för Granite-gränssnittet. Komponenten har uppdaterats för att visa alla grupper utan trunkering. (NPR-42749)
* Löste ett problem i guiden Skapa lokal sida där obligatoriska fält i komponenter med flera fält inte validerades när sidegenskaperna redigerades. Med det här problemet kunde författare i sin tur kringgå valideringen och fortsätta med ofullständiga data. GRANITE-58826
* ARIA-attributen för hjälpknappen i AEM har korrigerats för att säkerställa att JAWS-skärmläsare meddelar en tydlig, användarvänlig etikett i stället för att visa oöversatta ikoner och textmetadata. GRANITE-55360

#### WCM{#foundation-wcm-65-lts-sp1}

* Java 17-stödet för AEM Translations har utökats genom att översättningspaketen uppdaterats, kompatibiliteten med Java-paket har verifierats, beroenden har tagits bort och full funktionalitet har säkerställts genom omfattande testning. (CQ-4357525)
* Det tomma Evergreen-testet `com.adobe.cq.platform.it.http.workflow.inbox.InboxOnOffTimeIT.testActivateLater` togs bort för att förhindra falska fel under automatiserad testning. (CQ-4298376)

#### Arbetsflöde{#foundation-workflow-65-lts-sp1}

* Attributet `data-detailsurl` som saknas har lagts till i inkorgsobjekt för att förhindra att odefinierade värden visas i URL:er när AEM 6.5 LTS används med Java 21. GRANITE-60158
* Korrigerade ett NullPointerException i metoden `deactivate` för paketet `WorkflowToPublishEventService` när AEM 6.5 LTS kördes med Java 21, vilket säkerställde att arbetsflödestjänsten stängdes utan fel. (GRANITE-58151)
* Arbetsflödesindexet har uppdaterats med stöd för delning, anpassning utanför kontoret och lösning av frågor på tidslinjen. GRANITE-52640)
* Arbetsflödesindexet har uppdaterats med stöd för delning, användningsklara anpassningsfunktioner och lösning av problem med tidslinjefrågor. (GRANITE-52294)
* Löste ökade fel i felloggarna under loggjämförelsevalideringen för en programuppgradering till AEM version 10912, vilket säkerställer stabil arbetsflödeskörning. (GRANITE-44268)
* URL-saneringsmetoden i arbetsflödesrapporter har uppdaterats för att ersätta `url.searchParams` med `url.search`, vilket förbättrar XSS-skyddet för sårbara URL:er. (CQ-4359585)
* Validerade funktioner för arbetsflöde, inkorg och projekt i AEM 6.6 Forms för att säkerställa korrekt drift och integrering. (CQ-4358777)
* Implementerad automatisering för att lansera artefakter från arbetsflöden via Jenkins, vilket möjliggör effektiva och enhetliga driftsättningsprocesser i AEM 6.5. (CQ-4358472)
* Korrigerade ett problem i arbetsflödet Skapa uppgift i Inkorg där dialogrutan Lägg till uppgift inte stängdes efter att du klickat på Skicka, trots att uppgiften skapades, på grund av ett syntaxfel i JavaScript. (CQ-4355336)
* Ett problem har korrigerats som förhindrade att konfigurationen för inkorgsvyn sparades på grund av att en egenskapsdefinition för `isEndUserConfigurationEnabled` saknades. (CQ-4287757)




## [!DNL Experience Manager Foundation] {#experience-manager-foundation}

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

## Installera och uppdatera {#install-update}

Installationsanvisningar finns i [Installationsanvisningar](/help/sites-deploying/custom-standalone-install.md).

>[!NOTE]
>
> Om du uppgraderar direkt till LTS SP1 från gamla 6.5 SP, följ instruktionerna för 6.5 till 6.5 LTS GA [upgrade](/help/sites-deploying/upgrade.md).


Detaljerade instruktioner finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

>[!NOTE]
>
> För nya AEM 6.5 LTS-installationer måste indexdefinitioner installeras separat. Mer information finns i [den här artikeln](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#index-definitions).

## Plattformar som stöds {#supported-platforms}

Hitta den fullständiga matrisen med plattformar som stöds, inklusive supportnivå på [AEM 6.5 LTS Technical Requirements](/help/sites-deploying/technical-requirements.md).

>[!NOTE]
>
>Java™ 17/Java™ 21 rekommenderas för AEM 6.5 LTS.


## Föråldrade och borttagna funktioner {#deprecated-and-removed-features}

<!-- CARRY OVER EACH RELEASE -->

Adobe granskar kontinuerligt produktfunktionerna för att förbättra kundens värde genom att modernisera eller ersätta äldre funktioner. Dessa ändringar görs med stor noggrannhet för bakåtkompatibilitet.

Följande regler gäller för att informera om den förestående borttagningen eller ersättningen av Adobe Experience Manager-funktioner (AEM):

1. Föråldringsanmälan kommer först. Funktionerna är fortfarande tillgängliga, men har inte förbättrats ytterligare.
1. Borttagning av föråldrade funktioner sker tidigast i följande större version. Det faktiska måldatumet för borttagning planeras att tillkännages senare.

Den här processen ger kunderna minst en releasecykel för att anpassa implementeringen till en ny version eller en efterföljare till en borttagningsfunktion, innan den faktiska borttagningen.

### Föråldrade funktioner {#deprecated-features}

I det här avsnittet listas funktioner som Adobe har ersatt i AEM 6.5 LTS. Normalt tar Adobe bort funktioner innan de tas bort i en framtida version och erbjuder ett alternativ.


Kunderna rekommenderas att granska om de använder funktionen/funktionen i den aktuella distributionen och planera för att ändra implementeringen så att den använder det alternativ som erbjuds.

| Område | Funktion | Ersättning | Version (SP) |
| --- | --- | --- | --- |
| Sites | [SPA-redigerare](/help/sites-developing/spa-overview.md) | De redigerare som rekommenderas för att hantera headless-innehåll i AEM är:<br>- [Universell redigerare](/help/sites-developing/universal-editor/introduction.md) för visuell redigering.<br>- [Innehållsfragmentredigeraren](/help/assets/content-fragments/content-fragments-managing.md) för formulärbaserad redigering. | 6,5 LTS GA |

### Borttagna funktioner {#removed-features}

I det här avsnittet listas funktioner som har tagits bort från AEM 6.5 LTS. Tidigare versioner hade dessa funktioner markerats som föråldrade.

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

<!-- DO THESE KNOWN ISSUES CARRY OVER EACH RELEASE? THE "PRODUCT UPDATES TEAM" IS SUPPOSED TO VERIFY EACH ISSUE AND LET YOU KNOW IF ANYTHING NEEDS TO BE ADDED, DELETED, OR CHANGED IN THIS LIST. -->

### Problem med JSP-skriptpaket i AEM 6.5.21-6.5.23 och AEM 6.5 LTS GA

AEM 6.5.21, 6.5.22, 6.5.23 och AEM 6.5 LTS GA levereras med paketet `org.apache.sling.scripting.jsp:2.6.0`, som innehåller ett känt fel. Problemet är vanligtvis under hög belastning när AEM-instansen hanterar många samtidiga begäranden.

När det här problemet inträffar kan ett av följande undantag visas i felloggarna tillsammans med referenser till `org.apache.sling.scripting.jsp:2.6.0`:

* `java.io.IOException: classFile.delete() failed`
* `java.io.IOException: tmpFile.renameTo(classFile) failed`
* `java.lang.ArrayIndexOutOfBoundsException: Index 0 out of bounds for length 0`
* `java.io.FileNotFoundException`

Det finns en hotfix [cq-6.5.lts.0-hotfix-NPR-42640](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-NPR-42640-1.2.zip) som åtgärdar det här problemet.

### Dispatcher-anslutningsfel med SSL-funktion (åtgärdat i AEM 6.5 LTS SP1 och senare){#ssl-only-feature}

>[!NOTE]
>
> Problemet förekommer endast i AEM 6.5 LTS GA-utgåvan.

När du aktiverar SSL-funktionen i AEM-distributioner finns det ett känt problem som påverkar anslutningen mellan Dispatcher- och AEM-instanserna. När du har aktiverat den här funktionen kan hälsokontroller misslyckas och kommunikationen mellan Dispatcher- och AEM-instanser kan avbrytas. Det här problemet inträffar specifikt när kunder försöker ansluta via `https + IP` från Dispatcher till AEM-instanser. Det är relaterat till SNI-valideringsproblem (Server Name Indication).

**Effekt:**

* Misslyckade hälsokontroller med HTTP 400-svarskoder
* Trafiken mellan Dispatcher och AEM har brutits
* Innehåll kan inte hanteras på rätt sätt via Dispatcher
* Anslutningsfel vid användning av HTTPS med IP-adresser i Dispatcher-konfiguration
* HTTP 400: Felet &quot;Invalid SNI&quot; vid anslutning via HTTPS + IP

**Berörda miljöer:**

* AEM installerar med Dispatcher-konfigurationer
* System där SSL-funktionen har aktiverats
* Dispatcher-konfigurationer som använder anslutningsmetoden `https + IP` till AEM-instanser

**Lösning:**
Kontakta Adobe kundsupport om du får det här problemet. Det finns en snabbkorrigering [cq-6.5.lts.0-hotfix-CQ-4359803](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-CQ-4359803-1.0.2.zip) som åtgärdar det här problemet. Försök inte aktivera SSL-funktioner förrän du har implementerat den nödvändiga snabbkorrigeringen.

## OSGi-paket och innehållspaket som ingår{#osgi-bundles-and-content-packages-included}

Följande textdokument innehåller en lista över de OSGi-paket och innehållspaket som ingår i den här [!DNL Experience Manager] 6.5 LTS-, Service Pack 1-versionen:

* [Lista över OSGi-paket som ingår i Experience Manager 6.5 LTS, Service Pack 1](/help/release-notes/assets/65lts_sp1_bundles.txt) <!-- UPDATE FOR EACH NEW RELEASE -->
* [Lista över innehållspaket som ingår i Experience Manager 6.5 LTS, Service Pack 1](/help/release-notes/assets/65lts_sp1_packages.txt) <!-- UPDATE FOR EACH NEW RELEASE -->

## Begränsade webbplatser{#restricted-sites}

Dessa webbplatser är bara tillgängliga för kunder. Kontakta din kontoansvarige på Adobe om du är kund och behöver åtkomst.

* [Nedladdning av produkt på licensing.adobe.com](https://licensing.adobe.com/)
* [Kontakta Adobe kundsupport](https://experienceleague.adobe.com/sv/docs/customer-one/using/home).

