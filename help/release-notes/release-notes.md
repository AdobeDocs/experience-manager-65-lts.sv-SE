---
title: Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS, SP1
description: Hitta aktuella versionsuppgifter för Adobe Experience Manager 6.5 LTS, tjänstepaket 1.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: b5a8f555-c061-4fe2-a100-cc01335959cb
source-git-commit: 7c3f5d203be1ee2daa3274f76eade9af2ab9c821
workflow-type: tm+mt
source-wordcount: '7751'
ht-degree: 0%

---

# Aktuell versionsinformation för Adobe Experience Manager 6.5 LTS, SP1 {#release-notes}

## Versionsinformation {#release-information}

| Produkt | [!DNL Adobe Experience Manager] 6.5 LTS |
|---|---|
| Version | Service Pack 1 (SP1), programfix för GRANITE-61551 <!-- UPDATE FOR EACH NEW RELEASE --> |
| Typ | Service Pack-version |
| Datum | 9 september 2025 <!-- UPDATE FOR EACH NEW RELEASE --> |
| Hämta URL | [Programdistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq660%2Fhotfixes%2Fcq-6.5.lts.1-hotfix-GRANITE-61551-1.2.zip) |

<!-- OLD URL TO JAR
(https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/servicepack-lts/cq-quickstart-6.6.1.jar) | -->


<!-- UPDATE ABOVE FOR EACH NEW RELEASE -->

## Vad ingår i [!DNL Adobe Experience Manager] 6.5 LTS, SP1 {#what-is-new}

<!-- UPDATE EACH RELEASE -->

[!DNL Experience Manager] 6.5 LTS, SP1 innehåller nya funktioner, viktiga förbättringar som kunden efterfrågat samt felkorrigeringar. Det innehåller även förbättringar av prestanda, stabilitet och säkerhet som släppts sedan den första tillgängligheten av 6,5 LTS i mars 2025. [Installera detta Service Pack](#install-update) på 6.5 LTS.

## Viktiga funktioner och förbättringar

### Forms

AEM 6.5 Forms LTS on JEE finns nu att köpa. Mer information om miljöer som stöds finns i dokumentet [Kombinationer av plattformar som stöds](/help/forms/using/aem-forms-jee-supported-platforms.md). Installationslänkar finns på sidan [AEM Forms-versioner](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases).

#### Vad ingår i AEM Forms 6.5 LTS SP1?

**Java Support-uppdateringar**

Stöd för nyare Java-versioner har införts:

* Java™ 17
* Java™ 21

**Supportuppdateringar för programservrar**

* Stöd för JBoss EAP 8 har lagts till.
* Det äldre PicketBox-säkerhetsramverket har tagits bort.
* Elytron-baserade arkiv med autentiseringsuppgifter stöds nu för säker hantering av autentiseringsuppgifter.

**Konfiguration: Autentiseringsarkiv (Elytron-baserad)**

AEM Forms på JBoss EAP 8 använder Elytron för att hantera säkra inloggningsuppgifter. Kunderna måste konfigurera ett Elytron-baserat autentiseringsarkiv för autentiseringsuppgifter för att säkerställa att servern startas korrekt och att databasautentiseringen är säker.

Konfigurationsinformation finns i installations- och konfigurationshandboken.

**Plattforms- och kompatibilitetsändringar**

* Stöd för serverspecifikation 5+
* Baserat på efterlevnad av Jakarta EE 9

**Krav för namnområdesmigrering**

* Jakarta EE 9 introducerar en namnområdesändring från `javax.*` till `jakarta.*`
* Alla **anpassade DSC:er** måste migreras till namnutrymmet `jakarta.*`
* AEM Forms 6.5 LTS SP1 stöder **endast Jakarta EE 9+-baserade programservrar**

Mer information finns i **Migrering från javax till jakarta Namespace**.

#### Migrering från `javax` till `jakarta`-namnområde

Från och med **AEM Forms 6.5 LTS SP1** stöds endast programservrar som implementerar **Jakarta Servlet API 5/6**. Med **Jakarta EE 9 och senare** gick alla API:er från namnområdet `javax.{}` till `jakarta.`.

Därför måste **alla anpassade DSC:er använda `jakarta` namespace**. Anpassade komponenter som skapats med `javax.{}` API:er är **inte kompatibla** med de programservrar som stöds.

**Migreringsalternativ för anpassade DSC:er**

Du kan migrera befintliga anpassade DSC:er på något av följande sätt:

**Alternativ 1: Source-kodmigrering (rekommenderas)**

* Uppdatera alla importsatser från `javax.{}` till `jakarta.`
* Återskapa och kompilera om de anpassade DSC-projekten
* Distribuera om de uppdaterade komponenterna till programservern

**Fördelar:**

* Garanterar långsiktig kompatibilitet med Jakarta EE 9+
* Passar bäst för projekt som underhålls aktivt

**Alternativ 2: Binär migrering med Eclipse-transformator**

* Använd verktyget **Eclipse Transformer** för att konvertera kompilerade binärfiler (`.jar`, `.war`) från `javax` till `jakarta`
* Inga källkodsändringar eller omkompilering krävs
* Distribuera om de omformade binärfilerna till programservern

>[!NOTE]
>
> Binär omformning utförs på **bytekodnivå**.

Nedan visas vanliga exempel på namnutrymmesändringar som krävs under migreringen:

* Före (javax)    Efter (jakarta)
* javax.servlet. **jakarta.servlet**
* javax.servlet.http. **jakarta.servlet.http.**

**Exempelimportmappningar**

I följande tabell visas vanliga namnområdesändringar som krävs vid migrering från `javax` till `jakarta`:

| Före (`javax`) | Efter (`jakarta`) |
| ---------------------- | ------------------------ |
| `javax.servlet.*` | `jakarta.servlet.*` |
| `javax.servlet.http.*` | `jakarta.servlet.http.*` |

Använd dessa mappningar som referens när du uppdaterar anpassad DSC-källkod eller validerar transformerade binärfiler.

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
* Ett oväntat JSP-kompileringsfel med `org.apache.sling.scripting.jsp 2.6.0` har korrigerats. (NPR-42640)

<!--
* Backported the fix for Sling Scripting issue that caused `DataTimeParseException` and `String.length()` null pointer exceptions during package installation. Updated Sling Scripting to version 2.8.3-1.0.10.6 to reduce installation errors and improve stability. (NPR-42640) -->

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

## Forms

### Forms Designer

* När en användare exporterar data för en XFA-baserad PDF med exportDataAPI visar XML-resultatet avvikelser när de jämförs med XML-data som exporteras manuellt med Acrobat Reader. Värden för vissa fält saknades i utdata jämfört med utdata som genererats från Acrobat Reader. (LC-3922791)
* När du skapar en taggad PDF med utdatatjänsten i Workbench läggs en oväntad etiketttagg till under referenstaggen i ett innehållsobjekt. (LC-3922756)
* När du förenklar dynamiska, ifyllbara PDF-filer till PDF/A-format med hjälp av utdatatjänsten bevaras inte det dynamiska läget. Detta problem leder till dataförlust och potentiella kompatibilitetsproblem, särskilt när taggning är aktiverat. (LC-3922708)
* När en användare placerar fältbeskrivningar med justering längst ned eller till höger i AEM Forms Designer, innehåller taggträdet endast bildtexten utan motsvarande värde, vilket leder till ofullständig taggning av hjälpmedel. (LC-3922619)
* QR-koderna i genererade PDF-filer blir oläsbara. Den alternativa texten för QR-koderna misslyckas också i hjälpmedelstestningen, vilket påverkar skärmläsarkompatibiliteten. (LC-3922551)
* När en användare återger ett brev i agentens användargränssnitt visas inte innehållet korrekt på grund av FormService-återgivnings()-API:t. (LC-3922461)
* När en användare försöker skapa PDF/A-filer från XDP-filer med formatet Nedsänkt fyrkant i AEM Forms, uppstår problem med kantåtergivning. (LC-3922180)
* Förenkling av dynamiska formulär som är bundna till ett XSD-schema orsakar partiell dataförlust eftersom vissa bundna formulärdata inte bevaras i den slutliga PDF. (LC-3922008)
* När en användare försöker exportera data från interaktiva PDF-filer med hjälp av API:t extractData i AEM Forms 6.5.13 och senare, leder det till att data saknas jämfört med manuell export. (LC-3921983)
* Om du konverterar XDP-formulär till statiska PDF-filer med AEM Forms Designer eller Output-tjänsten skapas flera `Link-OBJR`-taggar. Problemet orsakar ett kompatibilitetsproblem eftersom en enda enhetlig länktagg förväntas. (LC-3921977)

### Adaptiv Forms

* Om du aktiverar&quot;Tillåt RTF-text för rubrik&quot; i rotpanelen i AEM Forms döljs rotpanelens rubrik felaktigt av&quot;Uteslut namn från postdokument&quot; på en kapslad panel. Det gör det i det genererade postdokumentet. (FORMS-19696)
* Systemet ignorerar den anpassade `sling:resourceType` som tilldelats via `aem:afProperties` i ett JSON-schema. Den anpassade resurstypen ignoreras under återgivningen. (FORMS-19691)
* När en användare skickar ett adaptivt formulär med förifyllda bilagor med URI:er, misslyckas formuläröverföringen med ett NullPointerException på grund av att binära data saknas. (FORMS-19371) (FORMS-19486)
* När en användare överför en PDF under avsnittet Forms och dokument slutar tidslinjefunktionen att fungera. (FORMS-19407)(FORMS-19234)
* När en användare överför filer med OTB-komponenten (OOTB) för bifogade filer i AEM Forms identifieras säkerhetsproblem. Detta kan leda till att obehöriga kan komma att avbryta inlämningsprocessen. (FORMS-19271)
* När en användare konfigurerar ett anpassat formulär i AEM Forms så att det automatiskt genererar ett dokument för inspelning (DoR), visas inte den hämtade DoR-titeln i fältet Titel i Acrobat Reader dokumentegenskaper. Som standard visas inte formulärtiteln i stället för filnamnet. (FORMS-19263)
* När en användare öppnar en interaktiv kommunikation i agentens användargränssnitt kan de förfyllda data inte raderas helt. När de tas bort fylls de automatiskt i med samma data. (FORMS-19151)
* När en användare förhandsgranskar ett datumfält i agentgränssnittet ändras datumet oväntat. Problemet inträffar på grund av tidszonsavvikelser mellan den virtuella datorns UTC-inställning och systemets tolkning av datumet. (FORMS-19115)
* När en användare skickar ett formulär kan bifogade filer dupliceras, vilket leder till flera överföringar av samma fil. (FORMS-19045)(FORMS-19051)
* Det går inte att lägga till koordinatorer i uppsättningar av profiler i dokumentskydd i både produktions- och undermiljöer. (FORMS-18603, FORMS-18212, FORMS-19697)
* När en användare klickar på ikonen&quot;datepicker-calendar-icon&quot; i skrivbordsläge med ett tomt fält inträffar ett fel på grund av den odefinierade variabeln _$focusedDate, som stör associerade anpassade skript. (FORMS-18483)(FORMS-18268)
* När en kund förhandsgranskar en bokstav visas inte nummervärdena felaktigt i fältet&quot;Belopp i ord&quot;, vilket leder till felpassning och att blanksteg saknas i innehållet. (FORMS-18437, FORMS-17330, FORMS-18209, FORMS-18557, CTG-4150848, FORMS-19614, LC-3922004)
* När en kund förhandsgranskar en sparad bokstav på RHEL saknas, blanksteg och oväntade tecken som x visas. (FORMS-18422)(FORMS-17641)
* När en användare navigerar mellan flikar i AEM Forms svarar inte komponenterna på den första fliken. (FORMS-18345)
* När en användare konverterar en HTML-fil till PDF med alternativet WebToPDF, saknar utdata-PDF rubrikavsnittet, inklusive metadata- och titeltaggar. (FORMS-18223, FORMS-17835, FORMS-19642, FORMS-18224)
* När en användare anropar metoden retryAction(long actionOid) i AEM JEE Process Manager SDK försöker systemet felaktigt den första åtgärden som hittas i tabellen tb_action_instance. Det här arbetsflödet inträffar även när ett visst åtgärds-ID anges eller när ID:t är null, vilket resulterar i ett oväntat beteende. (FORMS-18187)
* En användare stöter på problem där de sparade utkasts- och överföringsfunktionerna misslyckas utan att visa något felmeddelande. (FORMS-18069)
* Övergången från XSD-baserade Foundation Components till Core Components förhindrar implementering av korsfilsreferenser i JSON-scheman, vilket påverkar den adaptiva Forms-migreringen. (FORMS-18065)
* När en användare förhandsgranskar en bokstav i agentanvändargränssnittet visas ett felaktigt värde i datumfältet på grund av problem med konc-tidskonvertering. Skillnaderna beror på skillnader i tidszon mellan den virtuella datormiljön och systemets tolkning av tid (UTC kontra lokal tid). (FORMS-17988) (FORMS-17248)
* När en användare förhandsgranskar brev med hjälp av mallar för e-postmeddelanden i AEM Forms varierar PDF-genereringstiderna avsevärt, från 1,5 sekunder till mer än 10 sekunder, även på samma server. Inkonsekvensen påverkar verksamhetskritiska arbetsflöden. (FORMS-17951)
* När en användare binder ett klottersigneringsobjekt i ett adaptivt formulär till en XDP-fil med alternativet Datakällor, kan ändringarna inte sparas. Orsaken beror på beständiga valideringsfel för proportioner, även när giltiga värden används. (FORMS-17587)
* När en användare använder en viss XDP-fil med många dolda fält för dokumentfragment, skapar AEM CRX-noder med egenskapen `cm:optional` inställd på false, vilket gör att överföringen av interaktiv kommunikation (IC) misslyckas. (FORMS-17538)
* När en kund förhandsgranskar en bokstav kan de numeriska rutorna inte hantera negativa värden korrekt när siffergränserna för lead och Frac definieras. Problemet inträffar på grund av användningen av parseFloat, som hanterar minustecknet som en del av talet. (FORMS-17451)
* När en bokstav förhandsgranskas uppmärksammas användningen av jokertecknet &quot;*&quot; i filen Adobe.json, vilket ger anledning till oro och kan komma att ändras. (FORMS-17317)
* När en användare använder en skärmläsare på kopplingskontot Ansök om en fast avgift för sparare annonseras rubrikerna felaktigt som klickbara, vilket kan orsaka tillgänglighetsproblem. (FORMS-17038)
* När ett formulär är inbäddat saknar den genererade iframe-funktionen ett rubrikattribut, vilket leder till ett kompatibilitetsproblem. (FORMS-17010)
* När du hämtar ett formulär med användargränssnittet i Forms Manager ingår alltid associerade beroenden, som teman och fragment. (FORMS-1581)
* När en användare öppnar formuläret på mobila enheter (iOS och Android™) inaktiveras knapparna &quot;next&quot; och &quot;previous&quot; på den första sidan. Skärmläsaren identifierar dem dock inte som inaktiverade. (FORMS-15773)
* När en användare sparar ett stort formulär med fragment och lazy loading aktiverat, kan han/hon inte hämta utkast, vilket stör arbetsflödet. (FORMS-19890, FORMS-19808)
* Användarna fick problem med att spara formuläregenskaper för adaptiva formulär baserat på kärnkomponenter. Det här felet uppstod eftersom redundanta skript från redigeraren för adaptiva formulär baserade på Foundation Components ingår, vilket orsakar konflikter i det adaptiva formuläret baserat på Core-komponenter. redigerare. (FORMS-17474)
* Användare fick problem med Adobe Sign GovCloud Signature-sidan som inte återges i en iframe. (FORMS-16803)
* Fel uppstod när användare valde referenser för AF-fragment (Core Component Adaptive Forms). Felmeddelandet&quot;Det gick inte att återge referensen: Inte en absolut sökväg&quot; visas, vilket förhindrar korrekt referensåtergivning. (FORMS-1968)
* Stöd för flertrådskonvertering med Acrobat DC gör att man kan konvertera dokument från Word, Excel och PowerPoint till PDF effektivare. (FORMS-21310)
* `com.adobe.granite.toggle.impl.dev`-paketet har lagts till i AEM Service Pack 24, vilket ger effektivare utvecklingsprocesser genom att det tas bort från Forms-tillägget. (FORMS-20139)
* FeatureToggleRenderConditionServlet har tagits bort från forms-foundation och paketet com.adobe.granite.toggle.impl.dev från formulärtillägget. Denna uppdatering säkerställer att återgivningsvillkoret åtgärdas korrekt efter att ett formulär har installerats, vilket förbättrar komponentfunktionaliteten för kunderna. (FORMS-20138)
* Användarna fick långsamma prestanda på grund av långa frågor i Adaptive Forms. Den här uppdateringsbackports-frågan ändras för att öka effektiviteten. Kunder kan nu skapa ett index med taggnamnet aemformsAFReferences. (FORMS-21411)
* Användarna fick feljusterade rubrikpositioner när de konverterade HTML till PDF (Portable Document Format) med WebtoPDF. Det här problemet påverkade dokumentlayouten och läsbarheten för utdata. (FORMS-21502, FORMS-21540)
* PDF/A-1b-valideringen misslyckades trots lyckad PreFlight-verifiering. Problemet påverkade dokumentkompatibilitetskontrollerna för företagskunder med PDF valideringsverktyg. (FORMS-20196)
* Användare upplevde oöversatta strängar i användargränssnittet, vilket orsakade förvirring och svårigheter att förstå gränssnittet. (FORMS-6542)
* Användarna råkade ut för problem med e-postmeddelanden. Arbetsflödessteget Skicka e-post kunde inte skicka e-postmeddelanden, vilket påverkade automatiserade kommunikationsprocesser. (FORMS-17961)
* Användarna har råkat ut för misslyckade tester av formulärarbetsflöden, vilket har påverkat deras förmåga att slutföra arbetsflöden effektivt. (FORMS-16231)
* Användare kunde inte använda tidslinjefunktionen i PDF-filer i AEM-formulär. Det här problemet påverkade användarnas möjlighet att spåra dokumentändringar och revisioner effektivt. När du överför någon PDF under avsnittet Forms och dokument i AEM formulärområde fungerar inte tidslinjevyn. (FORMS-1908)
* Användarna får ett null-pekarundantag när de interagerar med OData. Detta orsakar avbrott i datahämtningsprocesserna. (FORMS-20348)
* Tog bort biblioteket google.common.collect efter borttagningen av Guava, ett Java-bibliotek med öppen källkod. Denna uppdatering ger bättre kompatibilitet och prestanda för företagskunder som använder Adaptive Forms. (FORMS-17031)

### Forms Captcha

* `Hcaptcha` och `Turnstile` stöds för adaptiv Forms baserat på Foundation-komponenter. (FORMS-16562)
* Ikonen överlappar problem i dialogrutan `Create hCaptcha Configuration`. När du fyller i obligatoriska fält överlappade informationsikonen felikonen och orsakade förvirring under konfigurationsinställningarna. (FORMS-1616)
* Användare fick felaktig konfiguration när de plockades upp för reCAPTCHA i Adaptive Forms baserat på Foundation Components. När konfigurationsbehållaren inte har valts för ett formulär orsakade flera konfigurationer i mappen `conf/global` problemet. (FORMS-19237)
* Användare råkade ut för problem med att reCAPTCHA inte skulle återges. Detta påverkade inskickade formulär och säkerhetsvalidering för företagskunder. (FORMS-17136, FORMS-19596)
* Användarna får problem där storleken på reCAPTCHA Enterprise inte återspeglas i användargränssnittet. (FORMS-16574)
* Användare fick problem med ReCaptcha-funktionen på grund av en oavslutad ResourceResolver i `ReCaptchaConfigurationServiceImpl`, vilket orsakade intermittenta valideringsfel vid formulärskickning. (FORMS-19241)
* Användare fick problem med reCAPTCHA-validering när formulär redigerades på webbplatser. AEM-formulär kände inte igen formulärnamnet korrekt, vilket orsakade valideringsfel. (FORMS-20486)
* Användarna fick in blanketter även när reCAPTCHA-poängen var 1.0, vilket ledde till potentiella säkerhetsrisker. (FORMS-16766){{$include }}
* Förbättrade reCAPTCHA-varningar i Adaptive Forms genom att uppdatera skicka-felkoder till 400. Du kan även förfina loggvarningar för att skilja mellan tidsgränser, förfallodatum och fel vid robotidentifiering, vilket förbättrar felsökningsnoggrannheten och systemobserverbarheten. (FORMS-19240)
* Stängde en oavslutad ResourceResolver-instans i ReCaptchaConfigurationServiceImpl för att förhindra potentiella resursläckor och förbättra systemstabiliteten när du använder reCAPTCHA-integreringar i AEM Forms. (FORMS-1924)
* Förbättrad CAPTCHA-konfigurationshantering för AEM Forms genom att säkerställa att rätt konfigurationskoder binds till varje formulär när det finns flera poster i mappen /conf/global. Förhindrar oavsiktlig användning av felaktiga CAPTCHA-inställningar när konfigurationsbehållaren inte uttryckligen har valts. (FORMS-19239)

### Forms Management UI

* Användare upplevde olokaliserade strängar i skapandeprocessen `Forms` > `Create Watchfolder` >` Watchfolder`. När en bevakad mapp skapades hittades inte strängar som `Watchfolder creation` och `Watchfolder created successfully`, vilket påverkar användargränssnittet. (FORMS-15234)

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
* Detaljerade uppgraderingsinstruktioner finns i [uppgraderingshandboken för AEM Forms 6.5 LTS SP1 på JEE](https://experienceleague.adobe.com/en/docs/experience-manager-65-lts/content/forms/upgrade-aem-forms/upgrade)

#### Bästa tillvägagångssätt för AEM 6.5 LTS Service Pack-uppgraderingar

<!-- THE INFORMATION UNDER THIS HEADING CAME FROM CQDOC-23078 -->

**Miljö**
Gäller för: AEM 6.5 LTS-kunder (On-Premise) som installerar Service Pack 1 (SP1). SP1 levereras som en JAR för snabbstart.

**Varför det här betyder något**
SP1 för AEM 6.5 LTS levereras som en JAR för snabbstart i stället för som en ZIP för installation via Package Manager. Lokala kunder uppgraderar genom att ersätta Quickstart JAR, packa upp den och starta om. Den här metoden är förenlig med Adobe förfarande för uppgradering på plats.

**Rekommenderat uppgraderingsflöde (författare eller publicera)**

1. Kontrollera att AEM 6.5 LTS-instansen är felfri och tillgänglig.
1. Hämta SP1 Quickstart JAR (till exempel `cq-quickstart-6.6.x.jar`) från programvarudistribution.
1. Stoppa den instans som körs.
1. I AEM installationskatalog (utanför `crx-quickstart/`) ersätter du den tidigare JAR-filen för QuickStart med SP1 JAR.
1. Packa upp JAR:

   ```java
   java -jar cq-quickstart-6.6.x.jar -unpack
   ```

   (Justera stackflaggor efter behov.)

1. Byt namn på den opackade JAR så att den matchar rollen och porten, till exempel `cq-author-4502.jar` eller `cq-publish-4503.jar`.
1. Starta AEM och bekräfta uppgraderingen i användargränssnittet (Hjälp > Om) och loggarna.

**God hygien**

* Kör uppgraderingen i testmiljöer före produktionen.
* Gör fullständiga, återställningsbara säkerhetskopieringar (databasen plus eventuella externa datalager) innan du börjar.
* Läs Adobe riktlinjer för uppgradering på plats och tekniska krav (Java 17/21 rekommenderas för LTS).

>[!NOTE]
>
>Filnamn som visas ovan (till exempel `cq-quickstart-6.6.x.jar`) återspeglar SP1 Quickstart-artefaktnamnet som observerats för den här LTS-versionen. Använd alltid det exakta filnamn som du hämtar från programvarudistributionen.

## Installera och uppdatera {#install-update}

Installationsanvisningar finns i [Installationsanvisningar](/help/sites-deploying/custom-standalone-install.md).

>[!NOTE]
>
> Om du uppgraderar direkt till LTS SP1 från gamla 6.5 SP, följ instruktionerna för 6.5 till 6.5 LTS GA [upgrade](/help/sites-deploying/upgrade.md).


Detaljerade instruktioner finns i [uppgraderingsdokumentationen](/help/sites-deploying/upgrade.md).

>[!NOTE]
>
> För nya AEM 6.5 LTS-installationer måste indexdefinitioner installeras separat. Mer information finns i [den här artikeln](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#index-definitions).

## Installera och uppdatera AEM Forms-tillägg {#install-update-aem-forms-add-on}

Mer information finns i [Utföra en lokal uppgradering](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/release-notes/aem-forms-current-service-pack-installation-instructions).



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

<!-- DO THESE KNOWN ISSUES CARRY OVER EACH RELEASE? THE "PRODUCT UPDATES TEAM" IS SUPPOSED TO VERIFY EACH ISSUE AND LET YOU KNOW IF ANYTHING NEEDS TO BE ADDED, DELETED, OR CHANGED IN THIS LIST. -->

<!-- REMOVED THIS SECTION AS PER CQDOC-23046
### Issue with JSP scripting bundle in AEM 6.5.21-6.5.23 and AEM 6.5 LTS GA

AEM 6.5.21, 6.5.22, 6.5.23, and AEM 6.5 LTS GA ship with the `org.apache.sling.scripting.jsp:2.6.0` bundle, which contains a known issue. The issue typically occurs under high load when the AEM instance handles many concurrent requests.

When this issue occurs, one of the following exceptions may appear in the error logs alongside references to `org.apache.sling.scripting.jsp:2.6.0`:

* `java.io.IOException: classFile.delete() failed`
* `java.io.IOException: tmpFile.renameTo(classFile) failed`
* `java.lang.ArrayIndexOutOfBoundsException: Index 0 out of bounds for length 0`
* `java.io.FileNotFoundException`

A hotfix [cq-6.5.lts.0-hotfix-NPR-42640](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq660/hotfixes/cq-6.5.lts.0-hotfix-NPR-42640-1.2.zip) is available to resolve this problem. -->

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
* [Kontakta Adobe kundsupport](https://experienceleague.adobe.com/en/docs/customer-one/using/home).

