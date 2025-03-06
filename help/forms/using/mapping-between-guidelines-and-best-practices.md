---
title: Riktlinjer och bästa hjälpmedelspraxis för att skapa formulär i formulärdesigner
description: Bästa praxis och riktlinjer för tillgänglighet när man utvecklar formulär i formulärdesigner.
feature: Adaptive Forms, Forms Designer
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 7d082f5d-9c10-47a7-95d3-f2219c4b139c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3366'
ht-degree: 0%

---

# Koppling mellan riktlinjer och bästa metoder

I följande avsnitt mappas riktlinjerna för avsnitt 508 och WCAG till de bästa metoderna som beskrivs i den här handboken.

## § 1194.21 Riktlinjer, beskrivning och bästa praxis

### Section 508 §1194.21: Software applications and operating systems

| § 1194.21 Riktlinje | Riktlinjebeskrivning | LiveCycle Designer Best Practices krävs för regelefterlevnad | Anteckningar |
|-----------|-----------------------|-----------------------------------------------------------|-------|
| (a) | När programvara är utformad för att köras på ett system som har ett tangentbord, ska produktfunktioner kunna köras från ett tangentbord där själva funktionen eller resultatet av att utföra en funktion kan urskiljas i text. | <li> 2.7 Kontrollera att det går att använda tangentbordskontroller </li><li> 2.6 Kontrollera att läs- och tabbordningen är korrekt</li> | |
| b) | Applikationerna får inte störa eller inaktivera aktiverade funktioner i andra produkter som identifieras som hjälpmedelsfunktioner, om dessa funktioner utvecklas och dokumenteras i enlighet med branschstandarder. Applikationerna får inte heller störa eller inaktivera aktiverade funktioner i operativsystem som identifieras som hjälpmedelsfunktioner, där programprogrammeringsgränssnittet för dessa hjälpmedelsfunktioner har dokumenterats av operativsystemets tillverkare och är tillgängligt för produktutvecklaren. | Inga specifika tekniker för LiveCycle Designer - denna riktlinje hanteras av Adobe Reader för PDF forms. | |
| (c) | En väldefinierad indikering på skärmen av det aktuella fokus ska tillhandahållas som rör sig mellan interaktiva gränssnittselement när indatafokus ändras. Fokus ska visas programmatiskt så att hjälpmedelstekniken kan spåra fokus- och fokusförändringar. | 2.3 Välj rätt reglage För att se till att fokus visas både programmatiskt och visuellt bör du alltid använda standardkontrollerna. | |
| (d) | Tillräcklig information om ett element i användargränssnittet, inklusive elementets identitet, funktion och tillstånd, ska finnas tillgänglig för hjälpmedelsteknik. När en bild representerar ett programelement måste den information som bilden förmedlar också vara tillgänglig i text. | <ul><li>2.1 Förenkla formulären</li> <li>2.1.1 Undvik att flytta, blinka eller blinka</li> <li>2.2 Konfigurera formuläregenskaper för att generera hjälpmedelsinformation</li> <li>2.5 Ange korrekta etiketter för formulärkontroller</li></ul> | |
| (e) | När bitmappsbilder används för att identifiera kontroller, statusindikatorer eller andra programmatiska element ska den betydelse som tilldelas dessa bilder vara konsekvent genom hela programmets prestanda. | <ul><li>2.4 Ange textmotsvarigheter för bilder</li><li> 2.5 Ange korrekta etiketter för formulärkontroller Den här standarden gäller bara om du använder samma bild på flera ställen i ett formulär. Du bör inte använda bildbaserade anpassade kontroller. Använd i stället endast standardkontroller från LiveCycle Designer. Om du använder bilder i kontrollerna ska du alltid se till att de används på ett konsekvent sätt.</li> | |
| (f) | Textuell information ska tillhandahållas via operativsystemets funktioner för att visa text. Den information som minst ska göras tillgänglig är textinnehåll, textinmatningskarta och textattribut. | 2.3 Välj rätt kontroller Undvik att använda bilder för att förmedla textinformation. Använd alltid standardkontrollerna i stället för att använda anpassade indatakomponenter som kanske inte visar textegenskaperna på rätt sätt för operativsystemet. | |
| (g) | Tillämpningarna får inte åsidosätta användarvalda kontrast- och färgmarkeringar eller andra individuella visningsattribut. | Inga LiveCycle Designer-specifika tekniker | Om det är möjligt bör du använda de grundläggande standardsystemfärgerna. |
| (h) | När animering visas ska informationen kunna visas i minst ett presentationsläge utan animering om användaren så önskar. | 2.1 Gör formulären enkla att använda Undvik att använda animeringar i formulären, eller ge separata versioner där animeringar ersätts med statiska bilder. | |
| (i) | Färgkodning får inte användas som det enda sättet att förmedla information, ange en åtgärd, föreslå ett svar eller särskilja ett visuellt element. | 2.8 Använda färger på ett ansvarsfullt sätt | |
| (j) | När en produkt tillåter en användare att justera färg- och kontrastinställningar, ska det finnas en mängd olika färgval som kan ge ett intervall av kontrastnivåer. | Ej tillämpligt | Den här funktionen tillhandahålls vanligtvis av Adobe Reader, inte av formulärutvecklaren. |
| (k) | Programvaran får inte använda blinkande eller blinkande text, objekt eller andra element som har en blixt- eller blinkfrekvens som är större än 2 Hz och lägre än 55 Hz. | 2.1.1 Undvik att flytta, blinka eller blinka | |
| (l) | När elektroniska formulär används ska formuläret göra det möjligt för personer som använder hjälpmedel att få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in formuläret, inklusive alla anvisningar och sätt. | 2.5 Ange korrekta etiketter för formulärkontroller | |

### Section 508 §11942: Web-based intranet and internet information and applications

| §11942 Guide | Riktlinjebeskrivning | LiveCycle Designer Best Practices krävs för regelefterlevnad | Anteckningar |
|-----------|-----------------------|-----------------------------------------------------------|-------|
| (a) | En textmotsvarighet för varje icke-textelement ska anges (t.ex. via &quot;alt&quot;, &quot;long&quot; eller i elementinnehållet). | 2.4 Ange textmotsvarigheter för bilder | |
| b) | Motsvarande alternativ för alla multimediepresentationer ska synkroniseras med presentationen. | 2.12 Se till att allt multimediematerial är tillgängligt | |
| (c) | Webbsidor ska utformas så att all information som förmedlas med färg även är tillgänglig utan färg, t.ex. från sammanhang eller markeringar. | 2.8 Använda färger på ett ansvarsfullt sätt | |
| (d) | Dokumenten ska vara ordnade så att de är läsbara utan tillhörande formatmall. | Ej tillämpligt | |
| (e) | Det ska finnas överflödiga textlänkar för varje aktivt område i en serverbildschema. | Ej tillämpligt | |
| (f) | Bildscheman på klientsidan ska tillhandahållas i stället för serversidans bildscheman, utom när områdena inte kan definieras med en tillgänglig geometrisk form. | Ej tillämpligt | |
| (g) | Rad- och kolumnrubriker ska identifieras för datatabeller. | 2.9 Ange rubrikceller för tabeller | |
| (h) | Markering ska användas för att koppla dataceller och rubrikceller för datatabeller som har två eller flera logiska nivåer av rad- eller kolumnrubriker. | 2.9 Ange rubrikceller för tabeller | |
| (i) | Ramar ska ha text som underlättar ramidentifiering och navigering. | Ej tillämpligt | |
| (j) | Sidorna ska vara konstruerade så att de inte orsakar att skärmen flimrar med en frekvens som är högre än 2 Hz och lägre än 55 Hz. | 2.1 Förenkla formulären | |
| (k) | En sida som bara innehåller text, med motsvarande information eller funktionalitet, ska tillhandahållas för att göra så att en webbplats uppfyller bestämmelserna i denna del, när överensstämmelse inte kan uppnås på något annat sätt. Innehållet på den textbaserade sidan ska uppdateras när den primära sidan ändras. | Ej tillämpligt | |
| (l) | När sidor använder skriptspråk för att visa innehåll, eller för att skapa gränssnittselement, ska den information som ges av skriptet identifieras med funktionstext som kan läsas med hjälpmedelsteknik. | 2.11 Undvik störande skriptning | |
| (m) | När en webbsida kräver att det finns ett tillägg, ett plugin-program eller något annat program på klientdatorn för att sidinnehållet ska kunna tolkas, måste sidan innehålla en länk till ett plugin-program eller ett miniprogram som överensstämmer med §1194.21(a) till (l). | Ej tillämpligt | Webbsidor som länkar till PDF forms bör innehålla en länk till Adobe Reader. |
| (n) | När elektroniska blanketter är avsedda att fyllas i online ska de personer som använder hjälpmedel kunna få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in blanketten, inklusive alla anvisningar och anvisningar. | 2.5 Ange korrekta etiketter för formulärkontroller | |
| (o) | En metod ska finnas som gör det möjligt för användarna att hoppa över repetitiva navigeringslänkar. | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| (p) | När ett tidsbegränsat svar krävs ska användaren informeras och få tillräckligt med tid för att ange att mer tid krävs. | 2.11 Undvik störande skriptning | |

### Kontrollpunkter för WCAG 1.0 Prioritet 1

| Kontrollpunkt | Kontrollpunktsbeskrivning | LiveCycle Designer Best Practices krävs för regelefterlevnad | Anteckningar |
|------------|------------------------|-----------------------------------------------------------|-------|
| [1.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-text-equivalent) | Ange en textmotsvarighet för alla icke-textelement (t.ex. via &quot;alt&quot;, &quot;long&quot; eller i elementinnehållet). Detta inkluderar: bilder, grafiska representationer av text (inklusive symboler), bildschemaområden, animeringar (t.ex. animerade GIF-filer), miniprogram och programmatiska objekt, ASCII-bilder, ramar, skript, bilder som används som listpunkter, mellanrum, grafiska knappar, ljud (spelas upp med eller utan användarinteraktion), fristående ljudfiler, ljudspår för video och video. | <ul><li>2.4 Ange textmotsvarigheter för bilder</li> <li>2.12 Se till att allt multimediematerial är tillgängligt</li> | |
| [1.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-redundant-server-links) | Tillhandahåll överflödiga textlänkar för varje aktivt område i ett serverbildschema. | Ej tillämpligt | |
| [1.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-auditory-descriptions) | Tills användaragenterna automatiskt kan läsa upp textmotsvarigheten i ett visuellt spår, ska de ge en ljudbeskrivning av den viktiga informationen i det visuella spåret i en multimediepresentation. | 2.12 Se till att allt multimediematerial är tillgängligt | |
| [1.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-synchronize-equivalents) | För alla tidsbaserade multimediepresentationer (t.ex. en film eller animering) synkroniserar du motsvarande alternativ (t.ex. bildtexter eller ljudbeskrivningar av det visuella spåret) med presentationen. | 2.12 Se till att allt multimediematerial är tillgängligt | |
| [2.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-color-convey) | Se till att all information som förmedlas med färg också är tillgänglig utan färg, till exempel från kontext eller markering. | 2.8 Använda färger på ett ansvarsfullt sätt | |
| [4.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-identify-changes) | Identifiera tydligt ändringar i ett dokuments naturliga språk och eventuella textmotsvarigheter (t.ex. bildtexter). | 2.13 Identifiera språkändringar | |
| [5.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-table-headers) | Identifiera rad- och kolumnrubriker för datatabeller. | 2.9 Ange rubrikceller för tabeller | |
| [5.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-table-structure) | För datatabeller som har två eller flera logiska nivåer med rad- eller kolumnrubriker använder du kod för att koppla dataceller och rubrikceller. | 2.9 Ange rubrikceller för tabeller | |
| [6.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-order-style-sheets) | Ordna dokumenten så att de kan läsas utan formatmallar. Om ett HTML-dokument återges utan tillhörande formatmallar måste det till exempel fortfarande vara möjligt att läsa dokumentet. | Ej tillämpligt | |
| [6.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-dynamic-source) | Se till att motsvarigheter för dynamiskt innehåll uppdateras när det dynamiska innehållet ändras. | 2.11 Undvik störande skriptning | |
| [6.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-scripts) | Kontrollera att sidorna kan användas när skript, miniprogram eller andra programmatiska objekt är avstängda eller inte stöds. Om detta inte är möjligt kan du ange motsvarande information på en alternativ tillgänglig sida. | 2.11 Undvik störande skriptning | |
| [7.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-avoid-flicker) | Undvik att skärmen flimrar tills användaragenterna tillåter användaren att kontrollera flimmer. | 2.1 Förenkla formulären | |
| [9.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-client-side-maps) | Tillhandahåll klientbildscheman i stället för serverbildscheman, förutom där områdena inte kan definieras med en tillgänglig geometrisk form. | Ej tillämpligt | |
| [11.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-alt-pages) | Om du efter bästa förmåga inte kan skapa en tillgänglig sida kan du skapa en länk till en alternativ sida där W3C-teknik används, som är tillgänglig, har motsvarande information (eller funktioner) och som uppdateras så ofta som den otillgängliga (ursprungliga) sidan. | Ej tillämpligt | |
| [12.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-frame-titles) | Ge varje ram en rubrik som underlättar identifiering och navigering. | Ej tillämpligt | |
| [14.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-simple-and-straightforward) | Använd det tydligaste och enklaste språk som passar för en webbplats innehåll. | 2.1 Förenkla formulären | |

### Kontrollpunkter för WCAG 1.0-prioritet 2

| Prioritet 2 - kontrollpunkt | Kontrollpunktsbeskrivning | Bästa praxis för LiveCycle som krävs för efterlevnad | Anteckningar |
|------------|------------------------|-------------------------------------------------|-------|
| [2.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-color-contrast) | Se till att kombinationer av för- och bakgrundsfärger ger tillräcklig kontrast när de visas av någon som har färgunderskott eller när de visas på en svart och vit skärm. [Prioritet 2 för bilder, prioritet 3 för text]. | 2.8 Använda färger på ett ansvarsfullt sätt | |
| [3.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-use-markup) | När det finns ett lämpligt markeringsspråk kan du använda markeringar i stället för bilder för att förmedla information. | <ul><li>2.1 Förenkla formulären</li><li> 2.1.1 Undvik att flytta, blinka eller blinka</li> <li>2.2 Konfigurera formuläregenskaper för att generera hjälpmedelsinformation Använd alltid faktisk text i stället för bilder av text.</li> | |
| [3.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-identify-grammar) | Skapa dokument som validerar till publicerad formell grammatik. | | PDF forms måste överensstämma med den publicerade PDF-specifikationen för att kunna återges i Adobe Reader. |
| [3.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-style-sheets) | Använd formatmallar för att styra layout och presentation. | Ej tillämpligt | |
| [3.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-relative-units) | Använd relativ i stället för absoluta enheter i attributvärden för markeringsspråk och värden för formatmallsegenskaper. | Ej tillämpligt | |
| [3.5](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-logical-headings) | Använd rubrikelement för att förmedla dokumentstrukturen och använda dem i enlighet med specifikationen. | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| [3.6](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-list-structure) | Märk upp listor och listobjekt korrekt. | 2.10.3 Markering listar Markera listbaserat innehåll som listor med rollerna Lista och Listobjekt. | |
| [3.7](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-quotes) | Märk upp offerter. Använd inte citattecken för formateringseffekter som indrag. | Ej tillämpligt | |
| [5.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-avoid-table-for-layout) | Använd inte tabeller för layout om inte tabellen är korrekt när den är linjär. I annat fall, om tabellen inte stämmer, kan du ange en alternativ motsvarighet (som kan vara en linjär version). | Inga specifika LiveCycle-tekniker | Det finns ingen anledning att använda tabeller för layout i LiveCycle-formulär. Använd i stället paletten Layout för att placera formulärfälten i ett rutnätsmönster. Använd bara en tabell när du använder tabellspecifika funktioner som tabellrubriker. |
| [5.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-table-layout) | Om en tabell används för layout ska du inte använda någon strukturkod för visuell formatering. | Inga specifika LiveCycle-tekniker | |
| [6.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-keyboard-operable-scripts) | För skript och miniprogram måste händelsehanterare vara enhetsoberoende. | 2.7 Kontrollera att det går att använda tangentbordskontroller | |
| [6.5](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-fallback-page) | Kontrollera att dynamiskt innehåll är tillgängligt eller tillhandahåller en alternativ presentation eller sida. | 2.11 Undvik störande skriptning | |
| [7.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-avoid-blinking) | Tills användaragenterna tillåter användare att kontrollera blinkning bör du undvika att innehållet blinkar (d.v.s. ändra presentationen med en vanlig hastighet, t.ex. aktivera och inaktivera). | 2.1 Förenkla formulären | |
| [7.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-avoid-movement) | Undvik rörelse på sidor tills användaragenterna tillåter användarna att frysa rörligt innehåll. | 2.1 Förenkla formulären | |
| [7.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-no-periodic-refresh) | Tills användaragenter ger möjlighet att stoppa uppdateringen ska du inte skapa sidor som uppdateras automatiskt regelbundet. | Ej tillämpligt | |
| [7.5](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-no-auto-forward) | Använd inte kod för att omdirigera sidor automatiskt förrän användaragenterna ger möjlighet att stoppa automatisk omdirigering. Konfigurera i stället servern att utföra omdirigeringar. | Ej tillämpligt | |
| [8.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-directly-accessible) | Gör programmatiska element som skript och miniprogram direkt tillgängliga eller kompatibla med hjälpmedelstekniker [Prioritet 1 om funktionaliteten är viktig och inte presenteras någon annanstans, annars Prioritet 2.] | 2.11 Undvik störande skriptning | |
| [9.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-keyboard-operable) | Se till att alla element som har ett eget gränssnitt kan hanteras på ett enhetsoberoende sätt. | 2.7 Kontrollera att det går att använda tangentbordskontroller | |
| [9.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-device-independent-events) | För skript anger du logiska händelsehanterare i stället för enhetsberoende händelsehanterare. | 2.7 Kontrollera att det går att använda tangentbordskontroller | |
| [10.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-avoid-pop-ups) | Tills användaragenter tillåter användare att inaktivera fönster som skapats för visning ska du inte göra så att popup-fönster eller andra fönster visas och inte ändra det aktuella fönstret utan att informera användaren. | 2.11 Undvik störande skriptning | |
| [10.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-unassociated-labels) | Se till att etiketten är rätt placerad tills användaragenten har stöd för explicita associationer mellan etiketter och formulärkontroller för alla formulärkontroller med implicit associerade etiketter. | 2.5 Ange korrekta etiketter för formulärkontroller | |
| [11.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-latest-w3c-specs) | Använd W3C-tekniker när de är tillgängliga och lämpliga för en viss uppgift och använd de senaste versionerna när de stöds. | Ej tillämpligt | |
| [11.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-avoid-deprecated) | Undvik föråldrade funktioner i W3C-tekniken. | Ej tillämpligt | |
| [12.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-frame-longdesc) | Beskriv syftet med ramar och hur bildrutorna relaterar till varandra om det inte framgår av bildrutetitel separat. | Ej tillämpligt | |
| [12.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-group-information) | Dela upp stora informationsblock i mer hanterbara grupper där det är naturligt och lämpligt. | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| [12.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-associate-labels) | Associera etiketter explicit med deras kontroller. | 2.5 Ange korrekta etiketter för formulärkontroller | |
| [13.1](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-meaningful-links) | Identifiera tydligt målet för varje länk. | 2.5 Ange lämpliga etiketter för formulärkontroller 2.5.6 Ange länktext | |
| [13.2](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-use-metadata) | Ange metadata för att lägga till semantisk information till sidor och webbplatser. | Ej tillämpligt | |
| [13.3](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-site-description) | Ange information om den allmänna layouten för en webbplats (t.ex. en webbplatskarta eller innehållsförteckning). | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| [13.4](https://www.w3.org/TR/WCAG10/wai-pageauth.html#tech-clear-nav-mechanism) | Använd navigeringsmekanismer på ett konsekvent sätt. | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | Använd mallsidor för att skapa enhetligt navigeringsinnehåll. |

### Villkor för WCAG 2.0-lyckade åtgärder

| Prioritet 1 G 2 Kontrollpunkter | Bästa praxis för LiveCycle som krävs för efterlevnad | Anteckningar |
| --- | --- | --- |
| 1.1 [Textalternativ](https://www.w3.org/TR/UNDERSTANDING-WCAG20/text-equiv.html) | | |
| 1.1.1 [Innehåll som inte är text](https://www.w3.org/TR/UNDERSTANDING-WCAG20/text-equiv-all.html) | 2.4 Ange textmotsvarigheter för bilder | |
| | 2.5 Ange korrekta etiketter för formulärkontroller | |
| 1.2 [Tidsbaserade media](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv.html) | | |
| 1.2.1 [Endast ljud och endast video (inspelat i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-av-only-alt.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.2 [Bildtexter (inspelade i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-captions.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.3 [Ljudbeskrivning eller mediealternativ (inspelat i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-audio-desc.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.4 [Bildtexter (Live)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-real-time-captions.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.5 [Ljudbeskrivning (inspelad i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-audio-desc-only.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.6 [Signeringsspråk (inspelat i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-sign.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.7 [Beskrivning av utökat ljud (inspelat i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-extended-ad.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.8 [Mediealternativ (inspelat i förväg)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-text-doc.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.2.9 [Endast ljud (live)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-live-audio-only.html) | 2.12 Se till att allt ljud- och videoinnehåll är tillgängligt | |
| 1.3 [Adaptiv](https://www.w3.org/TR/UNDERSTANDING-WCAG20/content-structure-separation.html) | | |
| 1.3.1 [Information och relationer](https://www.w3.org/TR/UNDERSTANDING-WCAG20/content-structure-separation-programmatic.html) | 2.9 Ange rubrikceller för tabeller | |
| 1.3.2 [Betydelsefull sekvens](https://www.w3.org/TR/UNDERSTANDING-WCAG20/content-structure-separation-sequence.html) | 2.6 Kontrollera att läs- och tabbordningen är korrekt | |
| | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| 1.3.3 [Sensoriska egenskaper](https://www.w3.org/TR/UNDERSTANDING-WCAG20/content-structure-separation-understanding.html) | 2.8 Använda färger på ett ansvarsfullt sätt | |
| 1.4 [Distinguishable](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast.html) | | |
| 1.4.1 [Användning av färg](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-without-color.html) | 2.8 Använda färger på ett ansvarsfullt sätt | |
| 1.4.2 [Ljudkontroll](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-dis-audio.html) | Inga specifika LiveCycle-tekniker | |
| 1.4.3 [Kontrast (minimal)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html) | 2.8 Använda färger på ett ansvarsfullt sätt | |
| 1.4.4 [Ändra storlek på text](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-scale.html) | Inga specifika LiveCycle-tekniker | |
| 1.4.5 [Bilder av text](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-text-presentation.html) | Inga specifika LiveCycle-tekniker | |
| 1.4.6 [Kontrast (Förbättrat)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast7.html) | 2.8 Använda färger på ett ansvarsfullt sätt | |
| 1.4.7 [Lågt eller inget bakgrundsljud](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-noaudio.html) | Inga specifika LiveCycle-tekniker | |
| 1.4.9 [Bilder av text (inga undantag)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-text-images.html) | Inga specifika LiveCycle-tekniker | |
| 2.1 [Tangentbord tillgängligt](https://www.w3.org/TR/UNDERSTANDING-WCAG20/keyboard-operation.html) | | |
| 2.1.1 [Tangentbord](https://www.w3.org/TR/UNDERSTANDING-WCAG20/keyboard-operation-keyboard-operable.html) | 2.6 Kontrollera att läs- och tabbordningen är korrekt | |
| | 2.7 Kontrollera att det går att använda tangentbordskontroller | |
| 2.1.2 [Ingen tangentbordssvällning](https://www.w3.org/TR/UNDERSTANDING-WCAG20/keyboard-operation-trapping.html) | 2.7 Kontrollera att det går att använda tangentbordskontroller | |
| 2.1.3 [Tangentbord (inget undantag)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/keyboard-operation-all-funcs.html) | 2.6 Kontrollera att läs- och tabbordningen är korrekt | |
| | 2.7 Kontrollera att det går att använda tangentbordskontroller | |
| 2.2 [Tillräcklig tid](https://www.w3.org/TR/UNDERSTANDING-WCAG20/time-limits.html) | | |
| 2.2.1 [Tidsjustering](https://www.w3.org/TR/UNDERSTANDING-WCAG20/time-limits-required-behaviors.html) | Inga specifika LiveCycle-tekniker | |
| 2.2.2 [Pausa, stoppa, dölj](https://www.w3.org/TR/UNDERSTANDING-WCAG20/time-limits-pause.html) | 2.1 Förenkla formulären | |
| 2.2.3 [Ingen timing](https://www.w3.org/TR/UNDERSTANDING-WCAG20/time-limits-no-exceptions.html) | Inga specifika LiveCycle-tekniker | |
| 2.2.4 [Avbrott](https://www.w3.org/TR/UNDERSTANDING-WCAG20/time-limits-postponed.html) | Inga specifika LiveCycle-tekniker | |
| 2.2.5 [Återautentiserar](https://www.w3.org/TR/UNDERSTANDING-WCAG20/time-limits-server-timeout.html) | Inga specifika LiveCycle-tekniker | |
| 2.3 [Kramper] | | |
| 2.3.1 [Tre blinkningar eller under tröskelvärde](https://www.w3.org/TR/UNDERSTANDING-WCAG20/seizure-does-not-violate.html) | 2.1 Förenkla formulären | |
| 2.3.2 [Tre blinkningar](https://www.w3.org/TR/UNDERSTANDING-WCAG20/seizure-three-times.html) | 2.1 Förenkla formulären | |
| 2.4 [Navigeringsbar](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms.html) | | |
| 2.4.1 [Kringgå block](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-skip.html) | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| 2.4.2 [Sida med rubriker](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-title.html) | Inga specifika LiveCycle-tekniker | |
| 2.4.3 [Fokusordning](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-focus-order.html) | 2.6 Kontrollera att läs- och tabbordningen är korrekt | |
| 2.4.4 [Länksyfte (i kontext)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-refs.html) | Inga specifika LiveCycle-tekniker | Länksyftet är beroende av att författaren väljer meningsfull text för länkade element. |
| 2.4.5 [Flera sätt](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-mult-loc.html) | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| 2.4.6 [Rubriker och etiketter](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-descriptive.html) | <ul><li>2.5 Ange korrekta etiketter för formulärkontroller</li><li>2.10 Tillhandahålla en navigeringsbar formulärstruktur</li> | |
| 2.4.7 [Synligt fokus](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-focus-visible.html) | Inga specifika LiveCycle-tekniker | Standardfokus i LiveCycle-formulär är synligt. |
| 2.4.8 [Plats](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-location.html) | Inga specifika LiveCycle-tekniker | Ej tillämpligt: LiveCycle-formulär kräver inga navigeringssystem. |
| 2.4.9 [Länksyfte (endast länk)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-link.html) | Inga specifika LiveCycle-tekniker | Länksyftet är beroende av att författaren väljer meningsfull text för länkade element. |
| 2.4.10 [Avsnittsrubriker](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-headings.html) | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| 3.1 [Kan läsas](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning.html) | | |
| 3.1.1 [Sidans språk](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning-doc-lang-id.html) | 2.13 Identifiera naturligt språk och eventuella språkförändringar | |
| 3.1.2 [Språk för delar](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning-other-lang-id.html) | 2.13 Identifiera naturligt språk och eventuella språkförändringar | |
| 3.1.3 [Ovanliga ord](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning-idioms.html) | Inga specifika LiveCycle-tekniker | |
| 3.1.4 [Förkortningar](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning-located.html) | Inga specifika LiveCycle-tekniker | |
| 3.1.5 [Läsnivå](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning-supplements.html) | Inga specifika LiveCycle-tekniker | |
| 3.1.6 [Uttal](https://www.w3.org/TR/UNDERSTANDING-WCAG20/meaning-pronunciation.html) | Inga specifika LiveCycle-tekniker | |
| 3.2 [Förutsägbar](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior.html) | | |
| 3.2.1 [Vid fokus](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-receive-focus.html) | 2.11 Undvik störande skriptning | |
| 3.2.2 [Vid indata](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html) | 2.11 Undvik störande skriptning | |
| 3.2.3 [Konsekvent navigering](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-consistent-locations.html) | 2.10 Tillhandahålla en navigeringsbar formulärstruktur | |
| 3.2.4 [Konsekvent identifiering](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-consistent-functionality.html) | <ul><li>2.3 Välj rätt kontroller</li><li>2.5 Ange korrekta etiketter för formulärkontroller</li> | |
| 3.2.5 [Ändra på begäran](https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-no-extreme-changes-context.html) | 2.11 Undvik störande skriptning | |
| 3.3 [Hjälp med indata](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error.html) | | |
| 3.3.1 [Felidentifiering](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-identified.html) |  | LiveCycle Designer har verktyg för att markera formulärfält efter behov och för att utföra validering av formulärindata. |
| 3.3.2 [Etiketter eller instruktioner](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-cues.html) | 2.5 Ange korrekta etiketter för formulärkontroller | |
| 3.3.3 [Felförslag](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-suggestions.html) |  | LiveCycle Designer har verktyg för att markera formulärfält efter behov och för att utföra validering av formulärindata. |
| 3.3.4 [Felförebyggande (Juridik, Ekonomi, Data)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-reversible.html) | Inga specifika LiveCycle-tekniker | |
| 3.3.5 [Hjälp](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-context-help.html) | Inga specifika LiveCycle-tekniker | |
| 3.3.6 [Felförebyggande (alla)](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-reversible-all.html) | Inga specifika LiveCycle-tekniker | |
| 4.1 [Kompatibel](https://www.w3.org/TR/UNDERSTANDING-WCAG20/ensure-compat.html) | | |
| 4.1.1 [Analys](https://www.w3.org/TR/UNDERSTANDING-WCAG20/ensure-compat-parses.html) | Inga specifika LiveCycle-tekniker | |
| 4.1.2 [Namn, roll, värde](https://www.w3.org/TR/UNDERSTANDING-WCAG20/ensure-compat-rsv.html) | <ul><li>2.3 Välj rätt kontroller</li> <li>2.5 Ange korrekta etiketter för formulärkontroller</li> | |
