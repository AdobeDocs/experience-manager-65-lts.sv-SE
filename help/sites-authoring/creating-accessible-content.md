---
title: Creating Accessible Content for Adobe Experience Manager (WCAG 2.1-överensstämmelse)
description: Använd AEM för att göra webbmaterial tillgängligt och användbart för personer med funktionshinder
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 0c3e3b6c-3c41-455e-823a-7cce50f174d4
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '13636'
ht-degree: 2%

---

# Skapa hjälpmedelsanpassat innehåll (WCAG 2.1-överensstämmelse) {#creating-accessible-content-wcag-conformance}

[Riktlinjerna för tillgängligt webbinnehåll (WCAG) 2.1](https://www.w3.org/TR/WCAG/), som har utarbetats av [en arbetsgrupp inom World Wide Web Consortium](https://www.w3.org/groups/#Accessibility_Guidelines_Working_Group), består av en uppsättning teknikoberoende riktlinjer och framgångskriterier som gör webbinnehåll tillgängligt för och användbart för personer med funktionshinder.

Som en introduktion tillhandahåller konsortiet en serie sektioner och styrkande dokument:

* [Nya funktioner i WCAG 2.1](https://www.w3.org/TR/WCAG/#new-features-in-wcag-2-1)
* [Så här uppfyller du WCAG 2.1](https://www.w3.org/WAI/WCAG21/quickref/)
* [Förstå WCAG 2.1](https://www.w3.org/WAI/WCAG21/Understanding/)
* [Tekniker för WCAG 2.1](https://www.w3.org/WAI/WCAG21/Techniques/)
* [WCAG-dokumenten](https://www.w3.org/WAI/standards-guidelines/wcag/docs/)

Se även:
* Snabbguiden [till WCAG 2.1](/help/managing/qg-wcag.md).
* [Rapporter om överensstämmelse för tillgänglighet för Adobe-lösningar](https://www.adobe.com/accessibility/compliance.html).
* [Konfigurera RTF-redigeraren för att skapa tillgängligt innehåll](/help/sites-administering/rte-accessible-content.md)

Riktlinjerna är indelade i tre överensstämmelsenivåer: Nivå A (lägsta), Nivå AA och Nivå AAA (högsta). Nivåerna definieras kortfattat enligt följande:

* **Nivå A:** Webbplatsen har en grundläggande, lägsta tillgänglighetsnivå. För att den här nivån ska uppnås måste alla kriterier på nivå A uppfyllas.
* **Nivå AA:** Detta är en idealisk nivå av hjälpmedel att eftersträva, där din webbplats når en grundläggande nivå av hjälpmedel, så att den är tillgänglig för de flesta personer i de flesta situationer som använder de flesta tekniker. För att den här nivån ska uppnås måste alla kriterier nivå A och nivå AA uppfyllas.
* **Nivå AAA:** Webbplatsen har hög tillgänglighet. För att uppnå den här nivån uppfylls alla kriterier för lyckade resultat på nivå A, nivå AA och nivå AAA.

När du skapar din webbplats bör du bestämma den övergripande nivån som du vill att din plats ska anpassas efter.

I följande avsnitt visas [lager i WCAG 2.1-riktlinjerna](https://www.w3.org/TR/WCAG/#wcag-2-layers-of-guidance) med relaterade kriterier för lyckade resultat för överensstämmelsenivåerna Nivå A och Nivå AA [&#128279;](https://www.w3.org/TR/WCAG/#conformance-to-wcag-2-1).

>[!NOTE]
>
>I det här dokumentet används följande:
>
>* [korta namn för WCAG 2.1-riktlinjerna](https://www.w3.org/TR/WCAG/#wcag-2-layers-of-guidance).
>* Den [numrering som används i WCAG 2.1-riktlinjerna](https://www.w3.org/TR/WCAG/#numbering-in-wcag-2-1) för att underlätta korsreferering med WCAG-webbplatsen.

## Princip 1: Förutsägbar {#principle-perceivable}

[Princip 1: Perfekt - Information och användargränssnittskomponenter måste vara presenterbara för användarna på ett sätt som de kan uppfatta.](https://www.w3.org/TR/WCAG/#perceivable)

### Textalternativ (1.1) {#text-alternatives}

[Riktlinje 1.1 Textalternativ: Ange textalternativ för allt icke-textinnehåll så att det kan ändras till andra formulär som användare behöver, till exempel stor utskrift, blindskrift, tal, symboler eller enklare språk.](https://www.w3.org/TR/WCAG/#text-alternatives)

### Innehåll som inte är text (1.1.1) {#non-text-content}

* Kriteriet 1.1.1 lyckades
* Nivå A
* Innehåll som inte är text: Allt innehåll som inte är text och som visas för användaren har ett textalternativ som har samma syfte, förutom de situationer som anges nedan.

#### Syfte - Innehåll som inte är text (1.1.1) {#purpose-non-text-content}

Information på en webbsida kan finnas i många olika format som inte är text, till exempel bilder, videor, animeringar, diagram och diagram. Personer som är blinda eller har allvarliga synskador kan inte se icke-textbaserat innehåll. De kan emellertid få åtkomst till textinnehåll genom att låta det läsas av en skärmläsare eller presenteras i taktil form av en blindskriftsvisningsenhet. Genom att tillhandahålla textalternativ för innehåll i grafiskt format kan alltså de som inte kan se det grafiska innehållet få tillgång till en motsvarande version av den information som innehållet ger.

En annan fördel är att textalternativ gör det möjligt att indexera icke-textinnehåll med sökmotorteknik.

#### Så här möts innehåll som inte är text (1.1.1) {#how-to-meet-non-text-content}

För statisk grafik är det grundläggande kravet att tillhandahålla ett motsvarande textalternativ för grafiken. Detta kan du göra i fältet **Alternativ text**. Se t.ex. kärnkomponenten **[Bild](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=sv-SE)**.

>[!NOTE]
>
>Vissa av de körklara kärnkomponenterna, som **[Carousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/carousel.html?lang=sv-SE)**, innehåller inte något **Alternativtext**-fält för att lägga till alternativa textbeskrivningar till enskilda bilder, även om det finns fältet **Etikett** (**[Hjälpmedel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/carousel.html?lang=sv-SE#accessibility-tab)** ) för hela komponenten.
>
>När du implementerar versioner av dessa för din AEM-instans måste ditt utvecklingsteam konfigurera sådana komponenter så att de stöder attributet `alt` så att författare kan lägga till det i innehållet (se [Lägga till stöd för ytterligare HTML Elements och attribut](/help/sites-administering/rte-accessible-content.md#add-support-for-more-html-elements-and-attributes)).

AEM kräver att fältet **Alternativ text** fylls i som standard. Om bilden är enbart dekorativ och alternativ text inte behövs kan alternativet **Bilden är dekorativ** kontrolleras.

#### Skapa bra textalternativ {#creating-good-text-alternatives}

Det finns olika former av innehåll som inte är text, så textalternativets värde beror på vilken roll bilden spelar på webbsidan. Några allmänna regler för tummen som ska följas är:

* Textalternativen bör vara kortfattade men ändå tydligt återge den viktiga information som icke-textinnehållet ger.
* För långa beskrivningar (över 100 tecken) bör undvikas. Om ett textalternativ kräver mer information:
   * ge en kort beskrivning i den alternativa texten
   * och har en längre beskrivning i text på en annan plats på samma sida eller på en separat webbsida. Länka till den här separata beskrivningen genom att göra bilden till en länk eller genom att placera en textlänk bredvid bilden.
* Alternativ text ska inte återge innehåll som finns i textformulär i närheten på samma sida. Kom ihåg att många bilder är illustrationer av punkter som redan finns på en sida, så det kan finnas ett detaljerat textalternativ.
* Om innehållet som inte är text är en länk till en annan sida eller ett annat dokument och det inte finns någon annan text som ingår i samma länk, måste den alternativa texten för bilden ange länkens mål. Den får inte beskriva bilden.
* Om innehållet som inte är text finns i ett knappelement och det inte finns någon text som tillhör samma knapp, måste den alternativa texten i bilden ange knappens funktion. Den får inte beskriva bilden.
* Det går bra att ge en bild en tom (null) alternativ text, men bara om bilden inte behöver någon alternativ text. Det är till exempel en helt dekorativ bild eller om motsvarande text finns i sidtexten.

<!--
The [W3C draft: HTML5 Techniques for providing useful text alternatives](https://dev.w3.org/html5/alt-techniques/) has more details and examples of appropriate alternative text provision for images of different types.
-->

Specifika typer av icke-textinnehåll som kräver textalternativ kan vara:

* Illustrativa foton: Det här är bilder av människor, objekt eller platser. Det är viktigt att tänka på fotots roll på sidan och att beskriva bildinnehållet, eftersom hjälpmedelstekniken meddelar elementtypen (till exempel `graphic` eller `image`). Det kan göra det tydligare att använda `screenshot` eller `illustration` i de alternativa textbeskrivningarna, men det beror på sammanhanget. Enhetlighet är en viktig faktor, ett beslut bör fattas för ett helt redigeringsteam och det ska tillämpas i hela användarupplevelsen.
* Ikoner: Det här är små bilder (grafik) som förmedlar specifik information. De måste användas konsekvent på en sida och en webbplats. Alla förekomster av ikonen på en sida eller på en webbplats bör ha samma korta och koncisa textalternativ, såvida inte detta leder till onödig duplicering av intilliggande text.
* Diagram och diagram: Dessa representerar vanligtvis numeriska data. Ett alternativ för att tillhandahålla ett textalternativ kan vara att ta med en kort sammanfattning av huvudtrenderna som visas i diagrammet eller grafiken. Om det behövs kan du även ge en mer detaljerad beskrivning i texten med hjälp av fältet **Beskrivning** på fliken **Avancerade** bildegenskaper. Du kan även tillhandahålla källdata i tabellformat någon annanstans på sidan eller webbplatsen.
* Kartor, diagram, flödesscheman: För grafik som innehåller rumsliga data (till exempel för att ge stöd för att beskriva relationer mellan objekt eller en process) måste du se till att huvudmeddelandet finns i textformat och att den här textinformationen är placerad nära varje associerad datapunkt. För kartor är det troligtvis opraktiskt att ange en fullständig textmotsvarighet, men om kartan tillhandahålls som ett sätt att hjälpa människor att hitta rätt till en viss plats, kan den alternativa texten i mappningsbilden kortfattat visa *Karta över X* och sedan ge anvisningar till den platsen i text någon annanstans på sidan eller genom fältet **Beskrivning** på fliken **Avancerat** i komponenten **Bild** .
* CAPTCHA: En CAPTCHA är ett *Helt automatiserat offentligt kurstest för att skilja datorer och människor åt*. Det är en säkerhetskontroll som används på webbsidor för att skilja människor från skadliga program, men som kan orsaka tillgänglighetshinder. Det är bilder som kräver att användarna beskriver vad de ser för att klara ett säkerhetstest. Det går inte att ange ett textalternativ för bilden, så du måste istället överväga alternativa icke-grafiska lösningar. W3C ger några förslag, till exempel:
   * Logikpussel
   * Användning av ljudutdata i stället för bilder
   * Begränsade användningskonton och skräppostfilter.
* Bakgrundsbilder: Dessa skapas med CSS (Cascading Style Sheets) i stället för i HTML. Det innebär att det inte går att ange ett alternativt textvärde. Därför bör bakgrundsbilder inte innehålla viktig textinformation. Om de gör det måste den här informationen också anges i sidans text. Det är dock viktigt att en alternativ bakgrund visas när bilden inte kan visas.

>[!NOTE]
>
>Det bör finnas en lämplig kontrastnivå mellan bakgrunden och förgrundstexten. Detta beskrivs mer ingående i [Kontrast (minimal) (1.4.3)](#contrast-minimum).

#### Mer information - Innehåll som inte är text (1.1.1) {#more-information-non-text-content}

* [Förstå villkor 1.1.1](https://www.w3.org/WAI/WCAG21/Understanding/non-text-content.html)
* [Så här uppfyller du kriterierna 1.1.1](https://www.w3.org/WAI/WCAG21/quickref/#non-text-content)
* [W3C-förklaring och alternativ till CAPTCHA](https://www.w3.org/TR/turingtest/)

<!--
* [W3C: HTML5 Techniques for providing useful text alternatives (draft)](https://dev.w3.org/html5/alt-techniques/)
-->

### Tidsbaserade media (1.2) {#time-based-media}

[Riktlinje 1.2 Tidsbaserat media: Ange alternativ för tidsbaserade media.](https://www.w3.org/TR/WCAG/#time-based-media)

Detta gäller webbinnehåll som är *tidsbaserat*. Detta omfattar innehåll som användaren kan spela upp (t.ex. video, ljud och animerat innehåll) och som kan spelas in i förväg eller i en liveström.

### Endast ljud och endast video (inspelat i förväg) (1.2.1) {#audio-only-and-video-only-prerecorded}

* Kriteriet 1.2.1 lyckades
* Nivå A
* Endast ljud och endast video (inspelat i förväg): För förinspelat ljud och förinspelat video gäller följande, utom när ljud eller video är ett meditalternativ för text och är tydligt märkt som sådant:
   * Inspelat endast ljud i förväg: Ett alternativ för tidsbaserade media anges som ger motsvarande information för förinspelat innehåll med endast ljud.
   * Inspelad video endast i förväg: Antingen är ett alternativ för tidsbaserade media, eller också tillhandahålls ett ljudspår som ger motsvarande information för förinspelat videomaterial.

#### Syfte - Endast ljud och endast video (inspelat i förväg) (1.2.1) {#purpose-audio-only-and-video-only-prerecorded}

Hjälpmedelsproblem för video och ljud kan uppstå om:

* Personer med nedsatt syn när det inte finns något ljudspår eller ljudspåret inte är tillräckligt för att informera dem om vad som händer i videon eller animeringen.
* Personer med nedsatt hörsel eller som är döva och som inte kan höra ljudspåret.
* Personer som kan höra ljudspåret, men som inte förstår vad som talas (till exempel för att det är på ett språk som de inte förstår).

Video eller ljud kan också vara otillgängligt för personer som använder webbläsare eller enheter som inte har stöd för uppspelning av innehåll i särskilda medieformat, t.ex. Adobe Flash.

Om du anger den här informationen i ett annat format, till exempel text (eller ljud för video utan ljud), kan det göra den tillgänglig för personer som inte kan komma åt det ursprungliga innehållet.

#### Så här möts du - endast ljud och endast video (inspelat i förväg) (1.2.1) {#how-to-meet-audio-only-and-video-only-prerecorded}

* Om innehållet spelas in i förväg utan video (till exempel en poddsändning):
   * Ange en länk omedelbart före eller efter innehållet till en textavskrift av ljudinnehållet. Avskriften ska vara en HTML-sida med en textmotsvarighet till allt tal och viktigt icke-talat innehåll, plus en indikation på vem som talar, en beskrivning av inställningen, röstuttryck och en beskrivning av allt annat viktigt ljud.
* Om innehållet är en animering eller förinspelad video utan ljud:
   * Tillhandahåll en länk omedelbart före eller efter innehållet till en motsvarande textbeskrivning av den information som videon ger
   * Eller en motsvarande ljudbeskrivning i ett vanligt ljudformat som MP3.

>[!NOTE]
>
>Om ljud- eller videoinnehållet tillhandahålls som ett alternativ till innehåll som finns i ett annat format på samma webbsida kanske inget extra alternativ krävs.
>
>Riktlinjerna [Om WCAG 1.2.1](https://www.w3.org/WAI/WCAG21/Understanding/audio-only-and-video-only-prerecorded.html) finns mer information.

Att infoga multimedia på dina AEM-webbsidor påminner om att infoga en bild. Men eftersom multimediainnehållet är mycket mer än en stillbild finns det olika inställningar och alternativ för att styra hur multimediainnehållet spelas upp.

>[!NOTE]
>
>När du använder multimedia med informativt innehåll måste du också skapa länkar till alternativ. Om du till exempel vill ta med en textutskrift skapar du en HTML-sida som visar utskriften och lägger sedan till en länk bredvid eller under ljudinnehållet.

#### Mer information - endast ljud och endast video (inspelat i förväg) (1.2.1) {#more-information-audio-only-and-video-only-prerecorded}

* [Förstå villkor 1.2.1](https://www.w3.org/WAI/WCAG21/Understanding/audio-only-and-video-only-prerecorded.html)
* [Så här uppfyller du kriterierna 1.2.1](https://www.w3.org/WAI/WCAG21/quickref/#audio-only-and-video-only-prerecorded)

### Bildtexter (inspelade i förväg) (1.2.2) {#captions-prerecorded}

* Kriteriet 1.2.2 lyckades
* Nivå A
* Bildtexter (inspelade i förväg): Bildtexter tillhandahålls för allt inspelat ljudinnehåll i synkroniserade medier, utom när mediet är ett mediaalternativ för text och är tydligt märkt som sådant.

#### Syfte - Textning (inspelad i förväg) (1.2.2) {#purpose-captions-prerecorded}

Personer som är döva eller hörselskadade kan inte eller har stora svårigheter att komma åt ljudinnehållet. Bildtexter är textmotsvarigheter för tal och icke-tal ljud som visas på skärmen vid lämplig tidpunkt under videon. De gör det möjligt för personer som inte kan höra ljudet att förstå vad som händer.

#### Så här uppfyller du kraven - bildtexter (inspelade i förväg) (1.2.2) {#how-to-meet-captions-prerecorded}

Bildtexter kan antingen vara:

* Öppna: alltid synligt när videon spelas upp
* Stängd: bildtexterna kan aktiveras och inaktiveras av användaren

Använd undertexter där det är möjligt, eftersom det ger användarna möjlighet att välja om de vill visa undertexter eller inte.

För undertexter skapar och tillhandahåller du en synkroniserad bildtextfil i ett lämpligt format (till exempel [SMIL](https://www.w3.org/AudioVideo/)) tillsammans med videofilen (information om hur du gör detta ligger utanför den här handbokens räckvidd, men vi har tillhandahållit länkar till vissa självstudiekurser under [Mer information - Bildtexter (inspelade i förväg) (1.2.2)](#more-information-captions-prerecorded)). Se till att du anger en anteckning, eller aktivera bildtextfunktionen i videospelaren, så att användarna vet att bildtexter är tillgängliga för videon.

Om du måste använda öppna bildtexter bäddar du in texten i videospåret. Detta kan du göra med videoredigeringsprogram som tillåter att titlar läggs över i videon.

#### Mer information - bildtexter (inspelade i förväg) (1.2.2) {#more-information-captions-prerecorded}

* [Förstå villkor 1.2.2](https://www.w3.org/WAI/WCAG21/Understanding/captions-prerecorded.html)
* [Så här uppfyller du kriterierna 1.2.2](https://www.w3.org/WAI/WCAG21/quickref/#captions-prerecorded)

<!--
* [W3C: Synchronized Multimedia](https://www.w3.org/AudioVideo/)
* [Captions, Transcripts, and Audio Descriptions - by WebAIM](https://webaim.org/techniques/captions/)
-->

### Ljudbeskrivning eller mediaalternativ (inspelat i förväg) (1.2.3) {#audio-description-or-media-alternative-prerecorded}

* Kriteriet 1.2.3 lyckades
* Nivå A
* Ljudbeskrivning eller mediaalternativ (inspelat i förväg): Ett alternativ för tidsbaserad media- eller ljudbeskrivning av det inspelade videoinnehållet tillhandahålls för synkroniserade media, utom när mediet är ett mediaalternativ för text och är tydligt märkt som ett sådant.

#### Syfte - Ljudbeskrivning eller mediealternativ (inspelat i förväg) (1.2.3) {#purpose-audio-description-or-media-alternative-prerecorded}

Personer med nedsatt syn eller nedsatt syn kommer att uppleva tillgänglighetshinder om informationen i en video eller animering endast tillhandahålls visuellt, eller om ljudspåret inte ger tillräcklig information för att förstå vad som händer visuellt.

#### Så här möts - ljudbeskrivning eller mediaalternativ (inspelat i förväg) (1.2.3) {#how-to-meet-audio-description-or-media-alternative-prerecorded}

Det finns två strategier som kan användas för att uppfylla detta kriterium. Båda är godtagbara:

1. Inkludera ytterligare ljudbeskrivning för videoinnehållet. Detta kan uppnås på ett av tre sätt:
   * Under pauser i den befintliga dialogen, lämna information om förändringar i scenen som inte presenteras som en del av det befintliga ljudspåret.
   * Skapa ett nytt, extra och valfritt ljudspår som innehåller det ursprungliga ljudspåret, men även extra ljudinformation om ändringar i scenen.
      * Användare kan växla mellan det befintliga ljudspåret (som *inte* innehåller en ljudbeskrivning) och det nya ljudspåret (som *inte* innehåller en ljudbeskrivning).
      * Detta förhindrar avbrott för användare som inte behöver den ytterligare beskrivningen.
   * Skapa en andra version av videoinnehållet som tillåter utökade ljudbeskrivningar. Detta minskar de svårigheter som är förknippade med att tillhandahålla detaljerade ljudbeskrivningar i mellanrummen mellan de befintliga dialogrutorna genom att tillfälligt pausa ljudet och videon vid lämpliga tidpunkter. Därför kan en mycket längre ljudbeskrivning ges innan åtgärden startar om. Precis som i föregående exempel är detta det bästa som finns som ett extra ljudspår för att förhindra avbrott för användare som inte behöver den extra beskrivningen.
1. Ange en textavskrift som är en lämplig textmotsvarighet till ljud- och visuella element i videon eller animeringen. Detta bör i tillämpliga fall omfatta en indikation om vem som talar, en beskrivning av inställningen, eventuella händelser eller information som presenteras visuellt samt röstuttryck. Beroende på hur lång den är kan du placera utskriften på samma sida som videon eller animeringen, eller på en separat sida. Om du väljer det senare alternativet anger du en länk till utskriften bredvid videon eller animeringen.

Exakta detaljer om hur du skapar ljudbeskrivad video ligger utanför den här handbokens räckvidd. Det kan ta lång tid att skapa videoklipp och ljudbeskrivningar, men med andra Adobe-produkter kan du göra detta.

#### Mer information - Ljudbeskrivning eller mediealternativ (inspelat i förväg) (1.2.3) {#more-information-audio-description-or-media-alternative-prerecorded}

* [Förstå villkor 1.2.3](https://www.w3.org/WAI/WCAG21/Understanding/audio-description-or-media-alternative-prerecorded.html)
* [Så här uppfyller du kriterierna 1.2.3](https://www.w3.org/WAI/WCAG21/quickref/#audio-description-or-media-alternative-prerecorded)

<!--
* [Adobe Encore](https://www.adobe.com/products/encore.html) - a DVD authoring software tool
-->

### Bildtexter (Live) (1.2.4)  {#captions-live}

* Kriteriet 1.2.4 lyckades
* Nivå AA
* Bildtexter (Live): Bildtexter finns för allt live-ljudinnehåll i synkroniserade media.

#### Syfte - Textning (live) (1.2.4) {#purpose-captions-live}

Detta kriterium är identiskt med [Bildtexter (inspelade i förväg)](#captions-prerecorded) eftersom det åtgärdar tillgänglighetshinder som upplevs av personer som är döva eller hörselskadade, förutom att detta kriterium gäller live-presentationer som webbsändningar.

#### Så här fungerar det - bildtexter (Live) (1.2.4) {#how-to-meet-captions-live}

Följ anvisningarna för [Bildtexter (inspelade i förväg)](#captions-prerecorded) ovan. På grund av mediernas aktiva natur måste dock bildtexter skapas så snabbt som möjligt och som svar på vad som händer. Därför bör du överväga att använda bildtexter i realtid eller tal-till-text-verktyg.

Detaljerade instruktioner ligger utanför det här dokumentets räckvidd, men med följande resurser får du användbar information:

* [WebAIM: Bildtext i realtid](https://webaim.org/techniques/captions/realtime)

* [AccessComputing-projekt (University of Washington): Kan bildtexter genereras automatiskt med taligenkänning?](https://www.washington.edu/accesscomputing/can-captions-be-generated-automatically-using-speech-recognition)

#### Mer information - Bildtexter (Live) (1.2.4) {#more-information-captions-live}

* [Förstå villkor 1.2.4](https://www.w3.org/WAI/WCAG21/Understanding/captions-live.html)
* [Så här uppfyller du kriterierna 1.2.4](https://www.w3.org/WAI/WCAG21/quickref/#captions-live)

### Ljudbeskrivning (inspelad i förväg) (1.2.5)  {#audio-description-prerecorded}

* Kriteriet 1.2.5 lyckades
* Nivå AA
* Ljudbeskrivning (inspelad i förväg): Ljudbeskrivning ges för allt inspelat videoinnehåll i synkroniserade media.

#### Syfte - ljudbeskrivning (inspelad i förväg) (1.2.5) {#purpose-audio-description-prerecorded}

Detta kriterium är identiskt med [Ljudbeskrivning eller Mediealternativ (inspelat i förväg)](#audio-description-or-media-alternative-prerecorded), förutom att författare måste ange en mycket mer detaljerad ljudbeskrivning för att uppfylla nivå AA.

#### Så här uppfyller du kraven - ljudbeskrivning (inspelad i förväg) (1.2.5) {#how-to-meet-audio-description-prerecorded}

Följ anvisningarna för [Ljudbeskrivning eller mediealternativ (inspelat i förväg)](#audio-description-or-media-alternative-prerecorded).

#### Mer information - ljudbeskrivning (inspelad i förväg) (1.2.5) {#more-information-audio-description-prerecorded}

* [Förstå villkor 1.2.5](https://www.w3.org/WAI/WCAG21/Understanding/audio-description-prerecorded.html)
* [Så här uppfyller du kriterierna 1.2.5](https://www.w3.org/WAI/WCAG21/quickref/#audio-description-prerecorded)

### Anpassningsbar (1.3) {#adaptable}

[Riktlinje 1.3 Anpassningsbar: Skapa innehåll som kan presenteras på olika sätt (till exempel enklare layout) utan att förlora information eller struktur.](https://www.w3.org/TR/WCAG/#adaptable)

Denna riktlinje omfattar de krav som är nödvändiga för att stödja personer som

* kanske inte kan komma åt information som presenterats av en författare i standardpresentationen av det innehållet (t.ex. en layout med flera kolumner eller en sida där färg och/eller bilder används mycket).

* kan använda enbart ljud eller alternativ visuell visning som stor text eller hög kontrast.

### Information och relationer (1.3.1)  {#info-and-relationships}

* Kriteriet 1.3.1 lyckades
* Nivå A
* Information och relationer: Information, struktur och relationer som förmedlas genom presentationen kan fastställas programmatiskt eller vara tillgängliga i text.

#### Syfte - Information och relationer (1.3.1) {#purpose-info-and-relationships}

Många hjälpmedelstekniker som används av personer med funktionshinder använder strukturinformation för att effektivt visa eller *förstå* -innehåll. Den här strukturinformationen kan ha formen av sidrubriker, tabellrader, kolumnrubriker och listtyper. En skärmläsare kan till exempel tillåta användaren att navigera på en sida från rubrik till rubrik. Men när sidinnehåll bara verkar ha en struktur genom visuell formatering, i stället för den underliggande HTML, finns det ingen strukturinformation tillgänglig för hjälpmedelstekniker, vilket begränsar deras möjligheter att hantera enklare surfning.

Detta kriterium gäller för att se till att sådan strukturinformation tillhandahålls via programkod via HTML eller andra kodningstekniker, så att webbläsare och hjälpfunktioner kan komma åt informationen och dra nytta av den.

#### Hur man möter - Information och relationer (1.3.1) {#how-to-meet-info-and-relationships}

AEM gör det enkelt att skapa semantiskt meningsfullt webbinnehåll med hjälp av lämpliga HTML-element. Öppna sidinnehållet i textredigeraren (en textkomponent) och använd menyn **Paraformat** (styckesymbol) för att ange lämpligt strukturelement (till exempel stycke och rubrik).

Du kan se till att dina webbsidor får rätt struktur genom att använda följande element där det är tillämpligt:

* **Rubriker:** Så länge du har tillgänglighetsfunktionerna i textredigeraren aktiverade erbjuder AEM tre nivåer för sidrubriken. Du kan använda dessa för att identifiera avsnitt och underavsnitt för innehåll. Rubrik 1 är den högsta rubriknivån, rubrik 3 den lägsta. Systemadministratören kan konfigurera systemet så att fler rubriknivåer tillåts.

* **Listor**: Du kan använda HTML för att ange tre olika typer av listor:
   * Elementet `<ul>` används för *oordnade* punktlistor. Enskilda listobjekt identifieras med elementet `<li>`. Använd ikonen **Punktlista** i textredigeraren.
   * Elementet `<ol>` används för *numrerade* listor. Enskilda listobjekt identifieras med elementet `<li>`. Använd ikonen **Numrerad lista** i textredigeraren.

  Om du vill ändra befintligt innehåll till en viss listtyp markerar du lämplig text och väljer lämplig listtyp. Precis som i det tidigare exemplet som visar hur stycketext skrivs in, läggs de rätta listelementen automatiskt till i din HTML.

  I helskärmsläge visas ikonerna **Punktlista** och **Numrerad lista**. Om du inte arbetar i helskärmsläge finns de två alternativen bakom den enda **Listor**-ikonen.

* **Tabeller**: Datatabeller måste identifieras med HTML tabellelement:
   * ett `<table>`-element
   * ett `<tr>`-element för varje rad i tabellen
   * ett `<th>`-element för varje rad och kolumnrubrik
   * ett `<td>`-element för varje datacell

  Tillgängliga tabeller använder dessutom följande element och attribut:

   * Elementet `<caption>` används för att tillhandahålla en synlig bildtext för tabellen. Bildtexter visas som standard centrerade ovanför tabellen, men kan placeras korrekt med CSS. Bildtexten är programmatiskt kopplad till tabellen och är därför en användbar metod för att ge en introduktion till innehållet.
   * Elementet `<summary>` hjälper icke-synkade användare att enklare förstå informationen som presenteras i en tabell genom att ge en sammanfattning av vad en synkad användare kan se. Det här är användbart när komplexa eller okonventionella tabellayouter används (det här attributet visas inte i webbläsaren, det läses bara ut för hjälpfunktioner).
   * `scope`-attributet för elementet `<th>` används för att ange om en cell representerar en rubrik för en viss rad eller för en viss kolumn. Ett liknande sätt är att använda attributen header och id i komplexa tabeller, där dataceller kan kopplas till en eller flera rubriker.

  >[!NOTE]
  >
  >Som standard är dessa element och attribut inte direkt tillgängliga, men det är möjligt för systemadministratören att lägga till stöd för dessa värden i dialogrutan **Tabellegenskaper** (se [Lägga till stöd för ytterligare HTML Elements och attribut](/help/sites-administering/rte-accessible-content.md#add-support-for-more-html-elements-and-attributes)).

  Så här öppnar du dialogrutan **Tabell** där du kan välja fliken **Tabellegenskaper**:

   * Definiera en lämplig **bildtext**.
   * Ta helst bort alla standardvärden för **Bredd**, **Höjd**, **Kant**, **Cellfyllnad** och **Cellmellanrum** eftersom dessa egenskaper kan anges i en global formatmall.

  Du kan sedan använda **cellegenskaperna** för att välja om cellen är en data- eller rubrikcell:

* **Betoning**: Använd elementet `<strong>` eller `<em>` för att ange betoning. Använd inte rubriker för att markera text i stycken.
   * Markera den text som du vill framhäva;
   * Klicka på ikonen **B** (för `<strong>`) eller **I** (för `<em>`) som visas på panelen **Egenskaper** (kontrollera att HTML är valt).

     >[!NOTE]
     >
     >RTE i en standardinstallation av AEM är konfigurerad att använda:
     >
     >* `<b>` för `<strong>`
     >* `<i>` för `<em>`
     >
     >De är i själva verket samma, men `<strong>` och `<em>` är att föredra eftersom de semantiskt korrekt HTML. Utvecklingsteamet kan konfigurera RTE så att `<strong>` och `<em>` (i stället för `<b>` och `<i>`) används när projektinstansen utvecklas.

* **Komplexa datatabeller**: Ibland, där det finns komplexa tabeller med två eller flera rubriknivåer, kanske de grundläggande tabellegenskaperna inte räcker till för att ge all nödvändig strukturinformation. För den här typen av komplexa tabeller måste direkta relationer skapas mellan rubrikerna och deras relaterade celler med attributen **header** och **id**.

  >[!NOTE]
  >
  >Attributet id är inte tillgängligt i en körklar installation. Den kan aktiveras genom att konfigurera HTML-regler och serialiseraren i textredigeraren.

  I tabellen nedan matchas till exempel rubriker och ID:n för att skapa en programmatisk association för hjälpmedelsanvändare.

  ```xml
    <table>
      <tr>
        <th rowspan="2" id="h">Homework</th>
        <th colspan="3" id="e">Exams</th>
        <th colspan="3" id="p">Projects</th>
      </tr>
      <tr>
        <th id="e1" headers="e">1</th>
        <th id="e2" headers="e">2</th>
        <th id="ef" headers="e">Final</th>
        <th id="p1" headers="p">1</th>
        <th id="p2" headers="p">2</th>
        <th id="pf" headers="p">Final</th>
      </tr>
      <tr>
        <td headers="h">15%</td>
        <td headers="e e1">15%</td>
        <td headers="e e2">15%</td>
        <td headers="e ef">20%</td>
        <td headers="p p1">10%</td>
        <td headers="p p2">10%</td>
        <td headers="p pf">15%</td>
      </tr>
    </table>
  ```

  Om du vill göra det i AEM lägger du till koden direkt i källredigeringsläget.

  >[!NOTE]
  >
  >Den här funktionen är inte omedelbart tillgänglig i en standardinstallation. Det kräver konfiguration av RTE, HTML-regler och serialisering.

#### Mer information - Info och relationer (1.3.1) {#more-information-info-and-relationships}

* [Förstå villkor 1.3.1](https://www.w3.org/WAI/WCAG21/Understanding/info-and-relationships.html)
* [Så här uppfyller du kriterierna 1.3.1](https://www.w3.org/WAI/WCAG21/quickref/#info-and-relationships)

### Betydelsefull sekvens (1.3.2)  {#meaningful-sequence}

* Kriteriet 1.3.2 lyckades
* Nivå A
* Betydelsefull sekvens: När den sekvens i vilken innehållet presenteras påverkar dess betydelse, kan en korrekt lässekvens bestämmas programmatiskt.

#### Syfte - meningsfull sekvens (1.3.2) {#purpose-meaningful-sequence}

Syftet med detta villkor är att en användaragent ska kunna tillhandahålla en alternativ presentation av innehållet samtidigt som läsordningen som behövs för att förstå innebörden bevaras. Det är viktigt att det är möjligt att programmässigt avgöra minst en sekvens av innehållet som är lämplig. Innehåll som inte uppfyller detta villkor kan förvirra eller skada användare när hjälpmedelstekniken läser innehållet i fel ordning eller när alternativa formatmallar eller andra formateringsändringar tillämpas.

#### Så här möts du - meningsfull sekvens (1.3.2) {#how-to-meet-meaningful-sequence}

Följ riktlinjerna under [Så här uppfyller du kriterierna 1.3.2](https://www.w3.org/WAI/WCAG21/quickref/#meaningful-sequence).

#### Mer information - meningsfull sekvens (1.3.2) {#more-information-meaningful-sequence}

* [Förstå villkor 1.3.2](https://www.w3.org/WAI/WCAG21/Understanding/meaningful-sequence.html)
* [Så här uppfyller du kriterierna 1.3.2](https://www.w3.org/WAI/WCAG21/quickref/#meaningful-sequence)

### Sensoriska egenskaper (1.3.3)  {#sensory-characteristics}

* Kriteriet 1.3.3 lyckades
* Nivå A
* Sensoriska egenskaper: Instruktioner för att förstå och hantera innehåll är inte enbart beroende av sensoriska egenskaper hos komponenter som form, storlek, visuell placering, orientering eller ljud.

#### Syfte - Sensoriska egenskaper (1.3.3) {#purpose-sensory-characteristics}

Designers fokuserar ofta på visuella designfunktioner som färg, form, textstil eller innehållets absoluta eller relativa position när de presenterar information. Dessa kan vara kraftfulla designtekniker för att förmedla information (och kan förbättra den övergripande tillgängligheten för synskadade med behov av kognitiv tillgänglighet), men personer med nedsatt syn eller blindhet kanske inte kan komma åt information som kräver visuell identifiering av attribut som position, färg eller form.

På samma sätt innebär information som kräver att man skiljer mellan olika ljud (till exempel manligt eller kvinnligt talt innehåll) tillgänglighetshinder för personer med nedsatt hörsel, om den inte återspeglas i något textalternativ för ljudinnehållet.

>[!NOTE]
>
>Mer information om krav för alternativ till färg finns i [Användning av färg](#use-of-color).

#### Hur man uppfyller kraven - sensoriska egenskaper (1.3.3) {#how-to-meet-sensory-characteristics}

Se till att all information som bygger på visuella egenskaper för sidinnehåll också presenteras i ett alternativt format.

* Förlita dig inte på visuell position för att ge information. Om du till exempel vill referera användare till en meny till höger på sidan för att få tillgång till mer information, ska du inte referera till *menyn till höger*. I stället ska du namnge menyn (t.ex. som en rubrik) och hänvisa till det namnet i texten.
* Förlita dig inte på att textformatering (till exempel fet eller kursiv text) är det enda sättet att förmedla information.

>[!NOTE]
>
>Beskrivande termer får användas om de anses ha betydelse i en icke-visuell kontext. Om du till exempel använder *ovan* och *nedan* skulle det i allmänhet vara acceptabelt, eftersom de antyder innehåll före och efter ett visst innehållsobjekt. Detta skulle fortfarande vara rimligt när innehållet talas högt.

#### Mer information - Sensoriska egenskaper (1.3.3) {#more-information-sensory-characteristics}

* [Förstå villkor 1.3.3](https://www.w3.org/WAI/WCAG21/Understanding/sensory-characteristics.html)
* [Så här uppfyller du kriterierna 1.3.3](https://www.w3.org/WAI/WCAG21/quickref/#sensory-characteristics)

### Skiljbar (1.4) {#distinguishable}

[Riktlinje 1.4 Skiljbar: Gör det enklare för användare att se och höra innehåll, inklusive att separera förgrunden från bakgrunden.](https://www.w3.org/TR/WCAG/#distinguishable)

### Användning av färg (1.4.1)  {#use-of-color}

* Kriteriet 1.4.1 lyckades
* Nivå A
* Användning av Färg: Färg används inte som det enda visuella sättet att förmedla information, indikera en åtgärd, fråga ett svar eller särskilja ett visuellt element.

>[!NOTE]
>
>Detta kriterium gäller specifikt färguppfattningen. Andra former av uppfattningar beskrivs i [Anpassningsbar (1.3)](#adaptable), inklusive programmatisk åtkomst till färg och annan visuell presentationskodning.

#### Syfte - Användning av färg (1.4.1) {#purpose-use-of-color}

Färg är ett effektivt sätt att förbättra webbsidornas estetiska utseende och kan även användas för att förmedla information. Det finns dock en rad synstörningar, från blindhet till färgssynsbrist, vilket innebär att vissa personer inte kan skilja mellan olika färger. Detta gör färgkodning till ett otillförlitligt sätt att tillhandahålla information.

En person med synsbrist i rött-grönt kommer till exempel inte att kunna skilja på nyanser i grönt och röda toner. De kan se båda färgerna som en tredje färg (till exempel brunt), och då kan de inte skilja mellan rött, grönt och brunt.

Färgen kan inte heller uppfattas av personer som använder webbläsare som bara innehåller text, enheter för monokrom visning eller som visar en svartvit utskrift av sidan.

Ett annat övervägande är det *valda*-läget för ett gränssnittselement (t.ex. tabbar, växlingsknappar), som måste förmedlas på något annat sätt än bara med färg och utanför bara en visuell presentation. För sådana element är den extra användningen av mönster, former och programmatisk information användbar när du skapar en helomfattande användarupplevelse som inte är beroende av en viss innebörd.

#### Hur man klarar - Färganvändning (1.4.1) {#how-to-meet-use-of-color}

Kontrollera att det finns information om färgen, oavsett var den används för att förmedla information, utan att du behöver se färgen.

Kontrollera till exempel att information som anges av färg också finns explicit i texten.

Om färg används som en referenspunkt för att ge information bör du ange en extra visuell referenspunkt, som att ändra formatet (till exempel fet, kursiv) eller teckensnitt. Detta hjälper personer med nedsatt syn eller som har nedsatt färgseende att identifiera informationen. Den kan dock inte användas helt eftersom den inte hjälper personer som inte kan se sidan alls. Därför är det (ibland) användbart att tillhandahålla dold text eller att använda programmatiska lösningar, som t.ex. [ARIA (Accessible Rich Internet Applications) ](https://www.w3.org/WAI/standards-guidelines/aria/), för att förmedla informationen till icke-synkade användare.

#### Mer information - Färganvändning (1.4.1) {#more-information-use-of-color}

* [Förstå villkor 1.4.1](https://www.w3.org/WAI/WCAG21/Understanding/use-of-color.html)
* [Så här uppfyller du kriterierna 1.4.1](https://www.w3.org/WAI/WCAG21/quickref/#use-of-color)

### Ljudkontroll (1.4.2)  {#audio-control}

* Kriteriet 1.4.2 lyckades
* Nivå A
* Ljudkontroll: Om något ljud på en webbsida spelas upp automatiskt i mer än 3 sekunder finns det antingen en mekanism som gör att ljudet pausas eller stoppas, eller så finns en mekanism som styr ljudvolymen oberoende av den totala systemvolymnivån.

#### Syfte - Ljudkontroll (1.4.2) {#purpose-audio-control}

Personer som använder skärmläsarprogram kan uppleva att det är svårt att höra talresultatet om det finns annat ljud som spelas upp samtidigt. Svårigheten förvärras när skärmläsarens tal-utdata är programbaserade (som de flesta är idag) och styrs via samma volymkontroll som ljudet. Dessutom kan vissa personer med kognitiva funktionshinder och personer som är neuroavvikande ha ljuskänslighet. De här personerna upptäcker att de inte kan ändra volymnivån på ljudinnehållet störande.

Därför är det viktigt att användaren kan stänga av bakgrundsljudet.

>[!NOTE]
>
>Att ha kontroll över volymen innebär bland annat att kunna minska volymen till noll.

#### Hur man klarar - ljudkontroll (1.4.2) {#how-to-meet-audio-control}

Följ riktlinjerna under [Så här uppfyller du kriterierna 1.4.2](https://www.w3.org/WAI/WCAG21/quickref/#audio-control).

#### Mer information - Ljudkontroll (1.4.2) {#more-information-audio-control}

* [Förstå villkor 1.4.2](https://www.w3.org/WAI/WCAG21/Understanding/audio-control.html)
* [Så här uppfyller du kriterierna 1.4.2](https://www.w3.org/WAI/WCAG21/quickref/#audio-control)

### Kontrast (minimal) (1.4.3) {#contrast-minimum}

* Kriteriet 1.4.3 lyckades
* Nivå AA
* Kontrast (minimal): Den visuella presentationen av text och bilder av text har ett kontrastförhållande på minst 4,5:1, utom följande:
   * Stor text: Storskalig text och bilder av storskalig text har ett kontrastförhållande på minst 3:1.
   * Incident: Text eller bilder av text som är en del av en inaktiv användargränssnittskomponent, [ren dekoration](https://www.w3.org/TR/WCAG/#dfn-pure-decoration), inte synliga för någon, eller som är en del av en bild som innehåller annat visuellt innehåll, har inget kontrastkrav.
   * Logotyper: Text som ingår i en logotyp eller ett varumärkesnamn har inget minimikrav på kontrast.

  >[!NOTE]
  >
  >Mer information finns i [Förstå icke-textkontrast](https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html) för att se till att innehållsförfattare förstår ytterligare krav runt icke-textelement (inklusive ikoner, gränssnittselement med flera).

#### Syfte - Kontrast (minimum) (1.4.3) {#purpose-contrast-minimum}

Personer med vissa nedsatt syn kanske inte kan skilja mellan vissa färgpar med låg kontrast. Tillgänglighetsproblem kan uppstå för dessa personer om något av följande:

* Texten har dålig kontrast mot bakgrundsfärgen.
* Färgkodningen för text (t.ex. länktext och icke-länktext) är viktig för att kunna skilja information åt.

>[!NOTE]
>
>Text som endast används för dekorationsändamål ingår inte i detta kriterium.

#### Hur man klarar - Kontrast (minimum) (1.4.3) {#how-to-meet-contrast-minimum}

Se till att texten kontrasterar tillräckligt med bakgrunden. Kontrastförhållanden beror på textens storlek och stil:

* För text som är mindre än 18 punkter (eller 14 punkter fet) bör kontrastförhållandet mellan text/bilder i texten och bakgrunden vara minst 4,5:1.
* För text som är minst 18 punkter (eller 14 punkter fet) bör kontrastförhållandet vara minst 3:1.
* Om en bakgrund är mönstrad ska bakgrunden runt all text skuggas så att proportionerna 4.5:1 eller 3:1 behålls.

>[!NOTE]
>
>Kom ihåg att teckensnitt kan skilja sig åt när det gäller hur de återger motsvarande PT/PX/EM-storlek.
>
>Använd god vana vid och felen när det gäller läsbarhet och användbarhet när du väljer rätt teckensnitt och storlek för webbinnehåll.

>[!NOTE]
>
>Följande sajter kan vara till hjälp vid konvertering till andra enheter:
>
>* [Px to Em Calculator - Omni](https://www.omnicalculator.com/conversion/px-to-em)
>* Se &quot;Konvertering av teckenstorlek: pixel-point-em-rem-percent&quot; vid `https://websemantics.uk/tools/font-size-conversion-pixel-point-em-rem-percent/`
>* Se PMtoEM.com: PX to EM conversion made simple at `http://pxtoem.com/`

Om du vill kontrollera kontrastförhållanden använder du ett färgkontrastverktyg, till exempel [Pacific Group Color Contrast Analyzer](https://www.paciellogroup.com/resources/contrast-analyser.html) eller [WebAIM-färgkontrastkontrollen](https://webaim.org/resources/contrastchecker/). Med dessa verktyg kan du kontrollera färgpar och rapportera om eventuella kontrastproblem.

Om du inte är lika orolig för hur sidan ska se ut kan du välja att inte ange färg för bakgrunds- och förgrundstext. Ingen kontrastkontroll krävs eftersom användarens webbläsare bestämmer färgerna för texten och bakgrunden.

Om det inte går att följa de rekommenderade kontrastnivåerna skapar du en länk till en alternativ, motsvarande version av sidan (som inte har några problem med färgkontrasten). Eller låt användaren justera kontrasten i sidfärgschemat efter sina egna krav.

#### Mer information - Kontrast (minimum) (1.4.3) {#more-information-contrast-minimum}

* [Förstå villkor 1.4.3](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html)
* [Så här uppfyller du kriterierna 1.4.3](https://www.w3.org/WAI/WCAG21/quickref/#contrast-minimum)

### Ändra storlek på text (1.4.4)  {#resize-text}

* Kriteriet 1.4.4
* Nivå A
* Ändra storlek på text: Förutom för bildtexter och bilder av text kan du ändra storlek på texten utan hjälpmedel upp till 200 procent utan att förlora innehåll eller funktioner.

#### Syfte - Ändra textstorlek (1.4.4) {#purpose-resize-text}

Syftet med detta villkor är att säkerställa att visuellt återgiven text, inklusive textbaserade kontroller (texttecken som har visats så att de kan ses som [kontra texttecken som fortfarande är i dataformat som t.ex. ASCII]), kan skalas så att de kan läsas direkt av personer med lindriga visuella funktionshinder, utan att hjälpteknik som skärmförstorare behöver användas. Det kan vara bra för användaren att skalförändra allt innehåll på webbsidan, men texten är viktigast.

#### Så här uppfyller du kraven - ändra storlek på text (1.4.4) {#how-to-meet-resize-text}

Förutom att följa riktlinjerna under [Uppfyll kriterier för framgång 1.4.4](https://www.w3.org/WAI/WCAG21/quickref/#resize-text) kan du uppmuntra skribenter att använda flytande, flexibel bredd och höjd i sin siddesign och sina teckenstorlekar (till exempel responsiv webbdesign) för att ge läsarna möjlighet att ändra storlek på texten.

#### Mer information - Ändra textstorlek (1.4.4) {#more-information-resize-text}

* [Förstå villkor 1.4.4](https://www.w3.org/WAI/WCAG21/Understanding/resize-text.html)
* [Så här uppfyller du kriterierna 1.4.4](https://www.w3.org/WAI/WCAG21/quickref/#resize-text)

### Bilder av text (1.4.5) {#images-of-text}

* Kriteriet 1.4.5 lyckades
* Nivå AA
* Bilder av text: Om den teknik som används kan åstadkomma den visuella presentationen används texten för att förmedla information i stället för bilder av text, med undantag för följande:
   * Anpassningsbar: Bilden av texten kan anpassas visuellt efter användarens behov.
   * Grundläggande: En viss presentation av texten är väsentlig för den information som förmedlas.

>[!NOTE]
>
>Logotyper (text som är en del av en logotyp eller ett varumärkesnamn) anses vara viktiga.

#### Syfte - Textbilder (1.4.5) {#purpose-images-of-text}

Bilder av text används ofta när ett visst textformat är att föredra, t.ex. en logotyp eller om text har genererats från en annan källa (t.ex. en skanning av ett pappersdokument). Jämfört med text som presenteras i HTML och formateras med CSS saknar dock bilder flexibiliteten att ändra storlek och utseende som kan behövas för personer med nedsatt syn eller nedsatt läsförmåga.

#### Så här möts - bilder av text (1.4.5) {#how-to-meet-images-of-text}

Om du måste använda bilder av text använder du CSS för att ersätta bilder av text med motsvarande text i HTML så att texten blir tillgänglig på ett anpassningsbart sätt. Se till exempel [C30: Använda CSS för att ersätta text med bilder av text och tillhandahålla gränssnittskontroller för att växla](https://www.w3.org/TR/2008/NOTE-WCAG20-TECHS-20081211/C30).

#### Mer information - Textbilder (1.4.5) {#more-information-images-of-text}

* [Förstå villkor 1.4.5](https://www.w3.org/WAI/WCAG21/Understanding/images-of-text.html)
* [Så här uppfyller du kriterierna 1.4.5](https://www.w3.org/WAI/WCAG21/quickref/#images-of-text)

## Princip 2: Användbar {#principle-operable}

[Princip 2: Operable - Användargränssnittskomponenter och navigering måste vara operabla.](https://www.w3.org/TR/WCAG/#operable)

### Tangentbord tillgängligt (2.1) {#keyboard-accessible}

[Riktlinje 2.1-tangentbord tillgänglig: Gör alla funktioner tillgängliga från ett tangentbord.](https://www.w3.org/TR/WCAG/#keyboard-accessible)

Det handlar om att se till att användarna har tillgång till alla funktioner via ett tangentbord.

### Tangentbord (2.1.1)  {#keyboard}

* Kriteriet 2.1.1 lyckades
* Nivå A
* Tangentbord: Alla funktioner i innehållet kan opereras via ett tangentbordsgränssnitt utan att särskilda tidsinställningar krävs för enskilda tangenttryckningar, förutom där den underliggande funktionen kräver indata som är beroende av sökvägen för användarens rörelse och inte bara för slutpunkterna.

#### Syfte - Tangentbord (2.1.1) {#purpose-keyboard}

Syftet med detta villkor är att se till att innehåll, när det är möjligt, kan köras via ett tangentbord eller tangentbordsgränssnitt (så att ett alternativt tangentbord kan användas). När innehåll kan användas via ett tangentbord eller alternativt tangentbord kan det hanteras av personer som inte har någon syn (som inte kan använda enheter som möss som kräver ögonkoordinering) och av personer som måste använda alternativa tangentbord eller inmatningsenheter som fungerar som tangentbordsemulatorer. Emulatorer för tangentbord omfattar talinmatningsprogram, klipp-och-släpp-program, tangentbord på skärmen, skanningsprogram samt olika hjälpmedelstekniker och alternativa tangentbord. Individer med nedsatt syn kan också ha problem med att spåra en pekare och upptäcka att programvaran är mycket enklare (eller endast möjligt) om de kan styra den via tangentbordet.

#### Så här möts du - tangentbord (2.1.1) {#how-to-meet-keyboard}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 2.1.1](https://www.w3.org/WAI/WCAG21/quickref/#keyboard).

#### Mer information - Tangentbord (2.1.1) {#more-information-keyboard}

* [Förstå villkor för slutförande 2.1.1](https://www.w3.org/WAI/WCAG21/Understanding/no-keyboard-trap.html)
* [Så här uppfyller du kriterierna för lyckade försök 2.1.1](https://www.w3.org/WAI/WCAG21/quickref/#keyboard)

### Ingen tangentbordssvällning (2.1.2)  {#no-keyboard-trap}

* Kriteriet 2.1.2 lyckades
* Nivå A
* Ingen tangentbordssvällning: Om tangentbordsfokus kan flyttas till en komponent på sidan med hjälp av ett tangentbordsgränssnitt, kan fokus flyttas bort från komponenten med bara ett tangentbordsgränssnitt. Om det kräver mer än oförändrad pil- eller tabbtangent eller andra standardmetoder för att avsluta, informeras användaren om metoden för att flytta fokus.

#### Syfte - Ingen tangentbordssvällning (2.1.2) {#purpose-no-keyboard-trap}

Syftet med det här villkoret är att se till att innehållet inte *fångar upp* tangentbordsfokus inom underavsnitt av innehållet på en webbsida. Detta är ett vanligt problem när flera format kombineras på en sida och återges med plugin-program eller inbäddade program.

Det kan finnas tillfällen då webbsidans funktioner begränsar fokus till ett underavsnitt av innehållet (till exempel en modal dialogruta). I sådana fall bör du ange en metod som gör att en användare kan lämna det underavsnittet av innehållet (ESC-tangenten stänger den modala dialogrutan eller en stängningsknapp stänger den modala dialogrutan).

#### Så här möts - ingen tangentbordssvällning (2.1.2) {#how-to-meet-no-keyboard-trap}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 2.1.2](https://www.w3.org/WAI/WCAG21/quickref/#no-keyboard-trap).

#### Mer information - Ingen tangentbordssvällning (2.1.2) {#more-information-no-keyboard-trap}

* [Förstå villkor för slutförande 2.1.2](https://www.w3.org/WAI/WCAG21/Understanding/no-keyboard-trap.html)
* [Så här uppfyller du kriterierna för lyckade försök 2.1.2](https://www.w3.org/WAI/WCAG21/quickref/#no-keyboard-trap)

### Tillräcklig tid (2.2) {#enough-time}

[Riktlinje 2.2 Tillräcklig tid: Ge användarna tillräckligt med tid för att läsa och använda innehåll.](https://www.w3.org/TR/WCAG/#enough-time)

Det handlar om att se till att användarna har tillräckligt med tid för att läsa och vidta åtgärder.

### Tidsjustering (2.2.1)  {#timing-adjustable}

* Kriteriet 2.2.1 lyckades
* Nivå A
* Tangentbord: Ge användarna tillräckligt med tid för att läsa och använda innehåll.

#### Syfte - Tidsjustering (2.2.1) {#purpose-timing-adjustable}

Syftet med detta kriterium är att se till att användare med funktionshinder får tillräckligt med tid för att interagera med webbinnehållet när det är möjligt. Personer med funktionshinder som blindhet, nedsatt syn, försämrad rörlighet och kognitiva begränsningar kan behöva mer tid för att läsa innehåll eller utföra funktioner som att fylla i onlineformulär. Om webbfunktionerna är tidsberoende är det svårt för vissa användare att utföra den nödvändiga åtgärden innan en tidsgräns inträffar. Detta kan göra tjänsten oåtkomlig för dem. Att utforma funktioner som inte är tidsberoende hjälper personer med funktionshinder att slutföra dessa funktioner. Genom att tillhandahålla alternativ för att inaktivera tidsgränser, anpassa tidsgränslängden eller begära mer tid innan en tidsgräns inträffar, kan de användare som behöver mer tid än förväntat sig för att kunna utföra sina uppgifter. De här alternativen visas i den ordning som passar användaren bäst. Det är bättre att inaktivera tidsgränser än att anpassa tidsgränslängden, vilket är bättre än att begära mer tid innan en tidsgräns inträffar.

#### Så här möts - Tidsjustering (2.2.1) {#how-to-meet-timing-adjustable}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 2.2.1](https://www.w3.org/WAI/WCAG21/quickref/#timing-adjustable).

#### Mer information - Tidsjustering (2.2.1) {#more-information-timing-adjustable}

* [Förstå villkor för slutförande 2.2.1](https://www.w3.org/WAI/WCAG21/Understanding/timing-adjustable.html)
* [Så här uppfyller du kriterierna för lyckade resultat &lbrace;2.2.1](https://www.w3.org/WAI/WCAG21/quickref/#timing-adjustable)

### Pausa, Stoppa, Dölj (2.2.2)  {#pause-stop-hide}

* Kriteriet 2.2.2 lyckades
* Nivå A
* Pausa, Stoppa, Dölj: Följande gäller för flyttning, blinkning, rullning eller automatisk uppdatering:
   * Flytta, blinka, rulla: För rörlig, blinkande eller rullningsinformation som
      * a) startar automatiskt,
      * b) varar mer än fem sekunder, och
      * C) presenteras parallellt med annat innehåll,
Det finns en mekanism för användaren att pausa, stoppa eller dölja den, såvida inte rörelsen, blinkningen eller rullningen är en del av en verksamhet där det är nödvändigt.
   * Automatisk uppdatering: För all information som uppdateras automatiskt
      * a) startar automatiskt, och
      * b) presenteras parallellt med annat innehåll
det finns en funktion som användaren kan använda för att pausa, stoppa eller dölja uppdateringen eller för att styra uppdateringens frekvens, såvida inte den automatiska uppdateringen är en del av en aktivitet där den är nödvändig.

Poängen är:

1. Krav som rör flimmer eller blinkande innehåll finns i Designa inte innehåll på ett sätt som är känt för att orsaka kramper (2.3).
1. Eftersom innehåll som inte uppfyller detta kriterium kan påverka användarens möjlighet att använda hela sidan, måste allt innehåll på webbsidan (vare sig det används för att uppfylla andra kriterier för framgång eller inte) uppfylla detta kriterium. Se [Krav på överensstämmelse 5: Störning](https://www.w3.org/TR/WCAG20/#cc5).
1. Innehåll som uppdateras regelbundet av programvara eller som direktuppspelas till användaragenten behöver inte bevara eller presentera information som genereras eller tas emot mellan inledandet av paus och återupptagandet, eftersom detta kanske inte är tekniskt möjligt, och i många situationer kan det vara vilseledande.
1. En animering som är en del av en förinläsningsfas eller liknande situation kan anses vara nödvändig om interaktion inte kan ske under den fasen för alla användare och om inte förloppet visar sig kan det förvirra användarna eller få dem att tro att innehållet frystes eller förstörs.

#### Syfte - Pausa, stoppa, dölj (2.2.2) {#purpose-pause-stop-hide}

Vissa användare kan finna att flyttningar är störande, eller till och med fysiskt besvärliga, vilket gör det svårt att koncentrera sig på andra delar av sidan. Dessutom kan sådant innehåll vara svårt att läsa för personer som har svårt att hänga med i rörlig text.

#### Så här möts du - Pausa, stoppa, dölj (2.2.2) {#how-to-meet-pause-stop-hide}

Beroende på innehållets natur kan du använda ett eller flera av följande förslag när du skapar webbsidor som innehåller rörligt, blinkande eller blinkande innehåll:

* Tillhandahåll ett sätt att pausa rullning av innehåll så att användarna får tillräckligt med tid för att läsa det. Till exempel nyhetsmarkörer, autouppdaterad text och bildkaruseller som går framåt automatiskt.
* Se till att innehåll som blinkar slutar blinka efter fem sekunder.
* Använd lämplig teknik för att visa rörligt eller blinkande innehåll som kan inaktiveras av webbläsaren. Exempel: GIF- (Graphics Interchange Format) eller APNG-filer (Animated Portable Network Graphics).
* Gör det möjligt för användaren att inaktivera allt rörligt eller blinkande innehåll på sidan genom att tillhandahålla en formulärkontroll på webbsidan.
* Om något av ovanstående inte är möjligt kan du skapa en länk till en sida som innehåller allt innehåll, men utan att flytta eller blinka.

#### Mer information - Pausa, Stoppa, Dölj (2.2.2) {#more-information-pause-stop-hide}

* [Förstå villkor 2.2.2](https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html)
* [Så här uppfyller du kriterium 2.2.2](https://www.w3.org/WAI/WCAG21/quickref/#pause-stop-hide)

### Kramper och fysiska reaktioner (2.3) {#seizures-and-physcial-reactions}

[Riktlinje 2.3 Kramper: Utforma inte innehåll på ett sätt som är känt för att orsaka kramper eller fysiska reaktioner.](https://www.w3.org/TR/WCAG/#seizures-and-physical-reactions)

### Tre blinkningar eller under tröskelvärde (2.3.1) {#three-flashes-or-below-threshold}

* Kriteriet 2.3.1 lyckades
* Nivå A
* Tre blinkningar eller under tröskelvärde: Webbsidor innehåller inte något som blinkar mer än tre gånger under en ensekundersperiod, eller blixten är under de allmänna tröskelvärdena för blixt och rött.

>[!NOTE]
>
>Eftersom innehåll som inte uppfyller detta kriterium kan påverka användarens förmåga att använda hela sidan, måste allt innehåll på webbsidan (vare sig det används för att uppfylla andra kriterier för framgång eller inte) uppfylla detta kriterium. Se [Krav på överensstämmelse 5: Störning](https://www.w3.org/TR/WCAG/#cc5).

#### Syfte - Tre blinkningar eller under tröskelvärde (2.3.1) {#purpose-three-flashes-or-below-threshold}

I vissa fall kan blinkande innehåll orsaka fotokänsliga anfall. Detta kriterium ger användarna möjlighet att få tillgång till och uppleva allt innehåll utan att behöva oroa sig för att innehållet blinkar.

#### Så här möts - tre blinkningar eller under tröskelvärdet (2.3.1) {#how-to-meet-three-flashes-or-below-threshold}

Se till att följande tekniker används:

* Se till att komponenterna inte blinkar mer än tre gånger under en 1-sekundersperiod.
* Om ovanstående villkor inte kan uppfyllas visas blinkande innehåll i pixlar i ett *litet säkert område* på skärmen. Det här området beräknas med en komplex formel, som beskrivs i [G176: Behåll blinkningsområdet tillräckligt litet](https://www.w3.org/TR/2008/NOTE-WCAG20-TECHS-20081211/G176), så den här tekniken bör endast följas om blinkande innehåll är *nödvändigt.

#### Mer information - tre blinkningar eller under tröskelvärde (2.3.1) {#more-information-three-flashes-or-below-threshold}

* [Förstå villkor för slutförande 2.3.1](https://www.w3.org/WAI/WCAG21/Understanding/three-flashes-or-below-threshold.html)
* [Så här uppfyller du villkor för att lyckas &lbrace;2.3.1](https://www.w3.org/WAI/WCAG21/quickref/#three-flashes-or-below-threshold)

### Navigeringsbar (2.4) {#navigable}

[Riktlinje 2.4 Navigeringsbar: Tillhandahåller sätt att hjälpa användare navigera, hitta innehåll och avgöra var de är.](https://www.w3.org/TR/WCAG/#navigable)

Det handlar om att säkerställa att innehållet är enkelt och enkelt att navigera i.

### Kringgå block (2.4.1)  {#bypass-blocks}

* Kriteriet 2.4.1 lyckades
* Nivå A
* Kringgå block: Det finns en funktion för att kringgå innehållsblock som upprepas på flera webbsidor.

#### Syfte - Kringgå block (2.4.1) {#purpose-bypass-blocks}

Syftet med detta villkor är att personer som navigerar sekventiellt genom innehållet ska kunna få mer direkt åtkomst till det primära innehållet på webbsidan. Webbsidor och program har ofta innehåll som visas på andra sidor eller skärmar. Exempel på upprepade innehållsblock är bland annat navigeringslänkar, rubrikgrafik, menyer och reklamramar, men är inte begränsade till dem. Små upprepade avsnitt, t.ex. enskilda ord, fraser eller enstaka länkar, anses inte utgöra block i denna bestämmelse.

#### Så här möts du - kringgå block (2.4.1) {#how-to-meet-bypass-blocks}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas &lbrace;2.4.1](https://www.w3.org/WAI/WCAG21/quickref/#bypass-blocks).

#### Mer information - Kringgå block (2.4.1) {#more-information-bypass-blocks}

* [Förstå villkor för slutförande 2.4.1](https://www.w3.org/WAI/WCAG21/Understanding/bypass-blocks.html)
* [Så här uppfyller du kriterierna för lyckade försök 2.4.1](https://www.w3.org/WAI/WCAG21/quickref/#bypass-blocks)

### Sida med rubriker (2.4.2)  {#page-titled}

* Kriteriet 2.4.2
* Nivå A
* Sidans namn: Webbsidor har rubriker som beskriver ämnet eller syftet.

#### Syfte - Sidtitlar (2.4.2) {#purpose-page-titled}

Detta kriterium hjälper alla att snabbt identifiera innehållet på en webbsida utan att behöva läsa hela sidan, oavsett eventuella försämringar. Det här är användbart när flera webbsidor öppnas på webbläsarflikar, eftersom sidrubriken visas på fliken och därför kan hittas snabbt.

#### Så här möts du - sida titel (2.4.2) {#how-to-meet-page-titled}

När en ny HTML-sida skapas i AEM kan du ange sidrubriken. Se till att titeln på rätt sätt beskriver sidans innehåll och syfte, särskilt unika aspekter, så att besökarna snabbt kan identifiera om innehållet är relevant för deras behov.

Du kan också redigera sidans titel när du redigerar en sida, tillgänglig via **Sidinformation** – **Egenskaper**.

#### Mer information - sida titel (2.4.2) {#more-information-page-titled}

* [Förstå villkor 2.4.2](https://www.w3.org/WAI/WCAG21/Understanding/page-titled.html)
* [Så här uppfyller du kriterium 2.4.2](https://www.w3.org/WAI/WCAG21/quickref/#page-titled)

### Fokusordning (2.4.3)  {#focus-order}

* Kriteriet 2.4.3 lyckades
* Nivå A
* Fokusordning: Om en webbsida kan navigeras sekventiellt och navigeringssekvenserna påverkar innebörden eller funktionen, får fokuserbara komponenter fokus i en ordning som bevarar betydelse och användbarhet.

#### Syfte - Fokusordning (2.4.3) {#purpose-focus-order}

Syftet med detta villkor är att se till att användare som navigerar sekventiellt genom innehållet stöter på information i en ordning som överensstämmer med innehållets innebörd och kan användas från tangentbordet. Detta minskar förvirringen genom att användarna kan skapa en konsekvent mentalmodell av innehållet. Det kan finnas olika sorteringsordning som återspeglar logiska relationer i innehållet. Om du till exempel förflyttar dig mellan komponenter i ett onlineformulär som består av flera fält och/eller steg, återspeglas de logiska relationerna i innehållet.

#### Så här möts du - fokusordning (2.4.3) {#how-to-meet-focus-order}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas &lbrace;2.4.3](https://www.w3.org/WAI/WCAG21/quickref/#focus-order).

#### Mer information - Fokusordning (2.4.3) {#more-information-focus-order}

* [Förstå villkor för slutförande 2.4.3](https://www.w3.org/WAI/WCAG21/Understanding/focus-order.html)
* [Så här uppfyller du kriterierna för lyckade försök 2.4.3](https://www.w3.org/WAI/WCAG21/quickref/#focus-order)

### Länksyfte (i sitt sammanhang) (2.4.4)  {#link-purpose-in-context}

* Kriteriet 2.4.4
* Nivå A
* Länksyfte (i kontext): Syftet med varje länk kan avgöras av länktexten fristående eller av länktexten tillsammans med dess programmatiskt fastställda länkkontext, utom när länkens syfte skulle vara tvetydigt för användarna i allmänhet.

#### Syfte - Länksyfte (i sammanhang) (2.4.4) {#purpose-link-purpose-in-context}

För alla användare är det viktigt att tydligt ange riktningen på en länk genom lämplig länktext, oavsett om det finns någon försämring. Detta hjälper användarna att avgöra om de faktiskt vill följa en länk. För synkade användare är meningsfull länktext användbar när det finns flera länkar på en sida (särskilt om sidan är texttung), eftersom meningsfull länktext ger en tydligare indikation på målsidans funktion. Användare av vissa hjälpmedelstekniker, som kan generera en lista över alla länkar på en sida, kan enklare förstå länktexten ur sitt sammanhang om länktexten är både unik och informativ. Synkroniserade individer med kognitiva funktionshinder kan dock bli förvirrade om en länk inte ger tillräckligt med information för att korrekt beskriva var länken tar dem.

#### Så här möts - länksyfte (i sammanhang) (2.4.4) {#how-to-meet-link-purpose-in-context}

Se framför allt till att länkens syfte tydligt beskrivs i länktexten.

* Felaktigt exempel:
   * Text: Klicka här om du vill ha information om våra kvällskurser för hösten 2010.
   * Orsak: Den anger inte tydligt och otvetydigt sin destination.
* Exempel:
   * Text: Kvällsklasser för hösten 2010 - detaljer.
   * Orsak: Genom att justera texten och placeringen av länkelementet något kan länktexten förbättras:

Länkarna ska vara enhetliga på olika sidor, särskilt för navigeringsfält. Om till exempel en länk till en viss sida heter **Publikationer** på en sida kan du använda den texten på andra sidor för att säkerställa konsekvens.

Vid skrivtillfället finns det vissa problem som omger användningen av rubrikattribut för att säkerställa att liknande länkar som visas på en sida ger unik information om destinationen (t.ex.&quot;läs mer&quot; avser ofta ett intervall av olika destinationer):

* Texten i rubrikattributet är bara tillgänglig för musanvändare som popup-fönster med verktygstips och kan inte öppnas konsekvent med tangentbordet eller av mobilanvändare.
* Skärmläsare kan läsa upp rubrikattribut, men den här funktionen kanske inte är aktiverad som standard. Därför kanske användarna inte känner till att det finns ett rubrikattribut.
* Det är svårt att ändra utseendet på titeltexten, vilket innebär att det kan vara svårt eller omöjligt att läsa av vissa personer.

Titelattributet kan användas för att ge en länk extra kontext, men tänk på dess begränsningar och använd det inte som ett alternativ till lämplig länktext.

Där länken består av en bild kontrollerar du att den alternativa texten för bilden beskriver länkens mål. Om till exempel en bild av en bokhylla är inställd som en länk till en persons publikationer, ska den alternativa texten läsa **John Smiths publikationer** och inte **Bookshelf**.

Om länkankarpunkten innehåller text som beskriver länkens syfte förutom bildelementet (och därmed texten visas bredvid bilden) använder du ett tomt alt-attribut för bilden:

```xml
<a href="publications.html">
<img src = "bookshelf.jpg" alt = "" />
John Smith's publications
</a>
```

>[!NOTE]
>
>Ovanstående kodutdrag är en illustration. Du bör använda komponenten **Bild**.

Även om det är tillrådligt att ge länktext som identifierar länkens syfte utan att behöva ytterligare kontext, är det inte alltid möjligt. Kontextfria länkar kan användas i följande fall, varav HTML-exempel finns i [Så här uppfyller du kriterium 2.4.4](https://www.w3.org/WAI/WCAG21/quickref/#link-purpose-in-context).

* Länktexten är en del av en lista med närbesläktade länkar och när listobjektet som länken omges av innehåller tillräckligt med kontext.
* Syftet med en länk kan tydligt identifieras från stycketexten *före* (inte följande).
* Länken ingår i en datatabell och syftet kan därför tydligt identifieras från tillhörande rubriker.
* En lista med länkar finns i en uppsättning rubriker och själva rubriken ger rätt sammanhang.
* En lista med länkar finns i en kapslad länk och det överordnade listobjektet ovanför den kapslade länken ger rätt kontext.

Ibland kan det vara lämpligt att tillhandahålla en alternativ version av webbsidan som visar exakt samma innehåll men där länktexten inte är så detaljerad, om det finns flera länkar på en sida (där var och en innehåller länkens riktning i komplex men nödvändig detalj).

Du kan också använda skript så att bara en liten del av texten finns i själva länken. När du aktiverar en lämplig kontroll som är placerad överst på sidan, *utökas* till mer information. Ett liknande sätt är att använda CSS för att *dölja* den fullständiga länken för identifierade användare, men ändå visa den i sin helhet för skärmläsaranvändare. Detta ligger utanför det här dokumentets omfång, men mer information om hur detta kan uppnås finns i avsnittet [Mer information - länksyfte (i kontext) (2.4.4)](#more-information-link-purpose-in-context).

#### Mer information - Länksyfte (i sammanhang) (2.4.4) {#more-information-link-purpose-in-context}

* [Förstå villkor för slutförande 2.4.4](https://www.w3.org/WAI/WCAG21/Understanding/link-purpose-in-context.html)
* [Så här uppfyller du kriterium 2.4.4](https://www.w3.org/WAI/WCAG21/quickref/#link-purpose-in-context)

<!--
* [C7: Using CSS to hide a portion of the link text](https://www.w3.org/TR/2008/NOTE-WCAG20-TECHS-20081211/C7)
-->

### Flera sätt (2.4.5)  {#multiple-ways}

* Kriteriet 2.4.5 lyckades
* Nivå AA
* Flera sätt: Det finns mer än ett sätt att hitta en webbsida i en uppsättning webbsidor, förutom där webbsidan är resultatet av, eller ett steg i, en process.

#### Syfte - Flera sätt (2.4.5) {#purpose-multiple-ways}

Syftet med detta kriterium är att göra det möjligt för användare att hitta innehåll på det sätt som passar deras behov bäst. En teknik kan vara lättare att använda eller lättare att förstå än en annan.

Även små webbplatser bör ge användarna en viss orienteringsmetod. För en webbplats på tre eller fyra sidor, med alla sidor länkade från hemsidan, räcker det kanske bara att tillhandahålla länkar från och till hemsidan, där länkarna på hemsidan också kan fungera som en webbplatskarta.

#### Så här möts du - flera sätt (2.4.5) {#how-to-meet-multiple-ways}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas &lbrace;2.4.5](https://www.w3.org/WAI/WCAG21/quickref/#multiple-ways).

#### Mer information - Flera sätt (2.4.5) {#more-information-multiple-ways}

* [Förstå villkor för slutförande 2.4.5](https://www.w3.org/WAI/WCAG21/Understanding/multiple-ways.html)
* [Så här uppfyller du kriterierna för att lyckas &lbrace;2.4.5](https://www.w3.org/WAI/WCAG21/quickref/#multiple-ways)

### Rubriker och etiketter (2.4.6)  {#headings-and-labels}

* Kriteriet 2.4.6 lyckades
* Nivå AA
* Rubriker och etiketter: Rubriker och etiketter beskriver ämnet eller syftet.

#### Syfte - Rubriker och etiketter (2.4.6) {#purpose-headings-and-labels}

Syftet med detta kriterium är att hjälpa användarna att förstå vilken information som finns på webbsidorna och hur informationen är organiserad. När rubrikerna är tydliga och beskrivande kan användarna enklare hitta den information de söker, och de kan enklare förstå relationen mellan olika delar av innehållet. Beskrivande etiketter hjälper användarna att identifiera specifika komponenter i innehållet.

#### Hur man uppfyller kraven - rubriker och etiketter (2.4.6) {#how-to-meet-headings-and-labels}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas &lbrace;2.4.6](https://www.w3.org/WAI/WCAG21/quickref/#headings-and-labels).

#### Mer information - Rubriker och etiketter (2.4.6) {#more-information-headings-and-labels}

* [Förstå villkor för slutförande 2.4.6](https://www.w3.org/WAI/WCAG21/Understanding/headings-and-labels.html)
* [Så här uppfyller du kriterierna för lyckade försök 2.4.6](https://www.w3.org/WAI/WCAG21/quickref/#headings-and-labels)

### Synligt fokus (2.4.7)  {#focus-visible}

* Kriteriet 2.4.7 lyckades
* Nivå AA
* Fokussynlig: Alla tangentbordsoperabla användargränssnitt har ett driftläge där tangentbordsfokusindikatorn visas.

#### Syfte - Synligt fokus (2.4.7) {#purpose-focus-visible}

Syftet med detta kriterium är att hjälpa en person att veta vilket element som har tangentbordsfokus.

Det måste vara möjligt för en person att veta vilket element bland flera element som har tangentbordsfokus. Om det bara finns en tangentbordskontroll på skärmen är det kriterium som uppfylls eftersom den visuella designen endast innehåller ett objekt som kan användas av tangentbordet.

Om resultatvillkoret är&quot;driftssätt&quot;, ska detta beaktas för plattformar som kanske inte alltid visar en fokusindikator. Normalt finns det bara ett driftsätt, så det här kriteriet gäller.

#### Hur man möter - Synligt fokus (2.4.7) {#how-to-meet-focus-visible}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas &lbrace;2.4.7](https://www.w3.org/WAI/WCAG21/quickref/#focus-visible).

#### Mer information - Synligt fokus (2.4.7) {#more-information-focus-visible}

* [Förstå villkor för slutförande 2.4.7](https://www.w3.org/WAI/WCAG21/Understanding/focus-visible.html)
* [Så här uppfyller du kriterierna för lyckade försök 2.4.7](https://www.w3.org/WAI/WCAG21/quickref/#focus-visible)

## Princip 3: Förstå {#principle-understandable}

[Princip 3: Förstå - Information och hur användargränssnittet fungerar måste vara begriplig.](https://www.w3.org/TR/WCAG/#understandable)

### Gör textinnehåll läsbart och begripligt (3.1) {#make-text-content-readable-and-understandable}

[Riktlinje 3.1 läsbar: Gör textinnehållet läsbart och begripligt.](https://www.w3.org/TR/WCAG/#readable)

### Sidans språk (3.1.1) {#language-of-page}

* Villkor för lyckat resultat 3.1.1
* Nivå A
* Sidans språk: Det mänskliga standardspråket för varje webbsida kan bestämmas programmatiskt.

#### Syfte - Sidans språk (3.1.1) {#purpose-language-of-page}

Syftet med detta kriterium är att säkerställa att text och annat språkligt innehåll återges korrekt. För skärmläsaranvändare säkerställer detta att innehållet uttalas korrekt, medan visuella webbläsare troligtvis visar vissa teckenuppsättningar korrekt.

#### Hur man uppfyller kraven - sidans språk (3.1.1) {#how-to-meet-language-of-page}

För att uppfylla det här kriteriet kan standardspråket på en webbsida identifieras med attributet `lang` i elementet `<html>` överst på sidan. Till exempel:

* Om en sida är skriven på engelska ska `<html>`-elementet ha följande namn:
  `<html lang = "en">`

* En sida som skall återges på spanska bör anta följande standard:
  `<html lang = "es">`

I AEM anges sidans standardspråk när du skapar sidan, men det kan också ändras när du redigerar [Sidegenskaper](/help/sites-authoring/editing-page-properties.md).

>[!NOTE]
>
>AEM erbjuder ytterligare finjustering för varianter av ett rotspråk, till exempel amerikansk engelska - en-us, brittisk engelska - en-gb och kanadensisk engelska - en-ca. Denna detaljnivå är ofta överflödig för hjälpmedelstekniker, men kan användas för regionala variationer av sidinnehåll.

#### Mer information - Sidans språk (3.1.1) {#more-information-language-of-page}

* [Förstå villkor 3.1.1](https://www.w3.org/WAI/WCAG21/Understanding/language-of-page.html)
* [Så här uppfyller du villkor 3.1.1](https://www.w3.org/WAI/WCAG21/quickref/#language-of-page)
* Koderna baseras på ISO 639-1. En mer omfattande lista med koder för varje språk finns på [W3 Schools-webbplatsen](https://www.w3schools.com/tags/ref_language_codes.asp).

### Delarnas språk (3.1.2)  {#language-of-parts}

* Villkor för lyckat resultat 3.1.2
* Nivå AA
* Språk för delar: Det mänskliga språket för varje stycke eller fras i innehållet kan bestämmas programmatiskt. Undantagen är egennamn, tekniska termer, ord av obestämt språk och ord eller fraser som har blivit en del av språket i den omedelbart omgivande texten.

#### Syfte - Språk för delar (3.1.2) {#purpose-language-of-parts}

Syftet med det här kriteriet är att det fungerar på samma sätt som kriteriet [Sidans språk](#language-of-page), förutom att det gäller webbsidor som innehåller innehåll på flera språk på en sida (t.ex. på grund av offerter eller ovanliga låneord).

Sidor som använder det här framgångsvillkoret tillåter:

* Programmet för blindskriftsövergång för att infoga tecken med accent.
* Skärmläsare använder för att uttala ord som har specialtecken eller som inte finns i standardspråket som identifierades på sidnivå.
* Översättningsverktyg som Google Translate för korrekt översättning av innehåll från ett språk till ett annat.

#### Hur man uppfyller kraven - Språk för delar (3.1.2) {#how-to-meet-language-of-parts}

Attributet `lang` kan användas för att identifiera ändringar i innehållsspråket. En offert på tyska (ISO 639-1-kod &quot;de&quot;) kan till exempel visas på följande sätt:

```xml
<blockquote cite = "John F. Kennedy" lang = "de">
     <p>Ich bin ein Berliner</p>
 </blockquote>
```

>[!NOTE]
>
>Blockcitattecken stöds inte i en körklar instans. En anpassad komponent kan utvecklas som stöd för funktionen.

På samma sätt kan webbläsaren återge ett ovanligt låneord eller en ovanlig fras korrekt om elementet `span` används enligt följande:

```xml
<p>The only French phrase I know is <span lang = "fr">je ne sais quoi</code>.</p>
```

>[!NOTE]
>
>Det är inte nödvändigt att följa detta kriterium när namn eller städer på olika språk inkluderas. Eller när du använder ord eller fraser som har blivit vanliga i standardspråket, som *schadenfreude* på engelska.

Om du vill lägga till intervallelementet med ett lämpligt språk kan du redigera din HTML-kod manuellt i källredigeringsläget för textredigeraren så att den läses som ovan. Alternativt kan attributet `lang` inkluderas i textredigeringsfilen av en systemadministratör (se [Lägga till stöd för ytterligare HTML Elements och attribut](/help/sites-administering/rte-accessible-content.md#add-support-for-more-html-elements-and-attributes)).

#### Mer information - Språk för delar (3.1.2) {#more-information-language-of-parts}

* [Förstå villkor 3.1.2](https://www.w3.org/WAI/WCAG21/Understanding/language-of-parts.html)
* [Så här uppfyller du villkor 3.1.2](https://www.w3.org/WAI/WCAG21/quickref/#language-of-parts)

### Förutsägbar (3.2) {#predictable}

[Riktlinje 3.2 Förutsägbar: Gör så att webbsidor visas och fungerar på förutsägbara sätt.](https://www.w3.org/TR/WCAG/#predictable)

Det handlar om att säkerställa att webbsidorna ser likadana ut och fungerar som de ska.

### Vid fokus (3.2.1)  {#on-focus}

* Villkor för lyckat resultat 3.2.1
* Nivå A
* I fokus: När en användargränssnittskomponent får fokus initieras ingen kontextändring.

#### Syfte - Vid fokus (3.2.1) {#purpose-on-focus}

Syftet med detta kriterium är att säkerställa att funktionaliteten är förutsägbar när besökarna navigerar genom ett dokument. Komponenter som kan utlösa en händelse när de får fokus får inte ändra sammanhanget. Exempel på föränderliga sammanhang när en komponent får fokus är bland annat följande, men är inte begränsade till:

* formulär som skickas automatiskt när en komponent får fokus,
* nya fönster öppnas när en komponent får fokus,
* fokus ändras till en annan komponent när den komponenten får fokus,

Fokus kan flyttas till en kontroll antingen via tangentbordet (t.ex. genom att tabba till en kontroll) eller med musen (t.ex. genom att klicka på ett textfält). Om du flyttar musen över en kontroll flyttas inte fokus om inte skriptet implementerar det här beteendet. För vissa typer av kontroller kan det även vara bra att klicka på en kontroll (till exempel knapp), som i sin tur kan aktivera en ändring i sammanhanget.

#### Så här möts du - i fokus (3.2.1) {#how-to-meet-on-focus}

Följ riktlinjerna under [Så här uppfyller du kriterierna för lyckat resultat 3.2.1](https://www.w3.org/WAI/WCAG21/quickref/#on-focus).

#### Mer information - I fokus (3.2.1) {#more-information-on-focus}

* [Förstå villkor 3.2.1](https://www.w3.org/WAI/WCAG21/Understanding/on-focus.html)
* [Så här uppfyller du villkoren 3.2.1](https://www.w3.org/WAI/WCAG21/quickref/#on-focus)

### Indata (3.2.2)  {#on-input}

* Villkor för lyckat resultat 3.2.2
* Nivå A
* Vid inmatning: Om du ändrar inställningen för en användargränssnittskomponent ändras inte sammanhanget automatiskt, såvida inte användaren har informerats om beteendet innan komponenten används.

#### Syfte - Vid inmatning (3.2.2) {#purpose-on-input}

Syftet med det här kriteriet är att säkerställa att det går att ange data eller välja en formulärkontroll. Om du ändrar inställningen för en användargränssnittskomponent ändras en del av kontrollen som kvarstår när användaren inte längre interagerar med den. Om du markerar en kryssruta, skriver text i ett textfält eller ändrar det markerade alternativet i en lista ändras inställningen, men om du aktiverar en länk eller en knapp ändras inte inställningen. Ändringar i sammanhanget kan förvirra användare som inte lätt uppfattar ändringen eller som lätt distraheras av ändringar. Kontextändringar är bara lämpliga när det är tydligt att en sådan ändring sker som svar på användarens åtgärd.

#### Så här möts - Vid inmatning (3.2.2) {#how-to-meet-on-input}

Följ riktlinjerna under [Så här uppfyller du kriterierna för lyckat resultat 3.2.2](https://www.w3.org/WAI/WCAG21/quickref/#on-input).

#### Mer information - Vid inmatning (3.2.2) {#more-information-on-input}

* [Förstå villkor 3.2.2](https://www.w3.org/WAI/WCAG21/Understanding/on-input.html)
* [Så här uppfyller du villkoren 3.2.2](https://www.w3.org/WAI/WCAG21/quickref/#on-input)

### Enhetlig navigering (3.2.3)  {#consistent-navigation}

* Villkor för lyckat resultat 3.2.3
* Nivå AA
* Enhetlig navigering: Navigeringsfunktioner som upprepas på flera webbsidor i en uppsättning webbsidor sker i samma relativa ordning varje gång de upprepas, såvida inte användaren påbörjar en ändring.

#### Syfte - Enhetlig navigering (3.2.3) {#purpose-consistent-navigation}

Syftet med detta kriterium är att uppmuntra till enhetlig presentation och layout för användare som interagerar med upprepat innehåll på en uppsättning webbsidor och måste hitta specifik information eller funktionalitet mer än en gång. Personer med nedsatt syn som använder skärmförstoring för att visa en liten del av skärmen i taget använder ofta visuella ledtrådar och sidgränser för att snabbt hitta upprepat innehåll. Att presentera upprepat innehåll i samma ordning är också viktigt för visuella användare som använder rumsligt minne eller visuella ledtrådar i designen för att hitta upprepat innehåll.

Användningen av frasen &quot;samma ordning&quot; i detta avsnitt innebär inte att undernavigeringsmenyer inte kan användas eller att block av sekundär navigering eller sidstruktur inte kan användas. Istället är resultatvillkoret avsett att hjälpa användare som interagerar med upprepat innehåll på webbsidor att kunna förutse var innehållet finns. Och hitta det snabbare när de kommer på det igen.

Användare kan initiera en ändring av ordningen med hjälp av adaptiva användaragenter eller genom att ange inställningar så att informationen presenteras på ett sätt som är mest användbart för dem.

#### Hur man möter - Enhetlig navigering (3.2.3) {#how-to-meet-consistent-navigation}

Följ riktlinjerna under [Så här uppfyller du kriterierna för lyckat resultat &lbrace;3.2.3](https://www.w3.org/WAI/WCAG21/quickref/#consistent-navigation).

#### Mer information - Enhetlig navigering (3.2.3) {#more-information-consistent-navigation}

* [Förstå villkor för slutförande 3.2.3](https://www.w3.org/WAI/WCAG21/Understanding/consistent-navigation.html)
* [Så här uppfyller du villkoren 3.2.3](https://www.w3.org/WAI/WCAG21/quickref/#consistent-navigation)

### Konsekvent identifiering (3.2.4)  {#consistent-identification}

* Villkor för lyckat resultat 3.2.4
* Nivå A
* Konsekvent identifiering: Komponenter som har samma funktioner i en uppsättning webbsidor identifieras konsekvent.

#### Syfte - Konsekvent identifiering (3.2.4) {#purpose-consistent-identification}

Syftet med detta kriterium är att säkerställa en konsekvent identifiering av funktionskomponenter som visas upprepade gånger i en uppsättning webbsidor. En strategi som användare av skärmläsare använder när de använder en webbplats är att förlita sig mycket på att de känner till funktioner som kan visas på olika webbsidor. Om identiska funktioner har olika etiketter (eller mer allmänt ett annat namn) på olika webbsidor är webbplatsen mycket svårare att använda. Den kan också vara förvirrande och öka den kognitiva belastningen för personer med kognitiva begränsningar. Därför är det lättare att använda konsekventa etiketter.

Den här konsekvensen omfattar även textalternativen. Om ikoner eller andra icke-textobjekt har samma funktioner bör deras textalternativ också vara konsekventa.

Om det finns två komponenter på en webbsida som båda har samma funktioner som en komponent på en annan sida i en uppsättning webbsidor, måste alla tre vara konsekventa. Därför kommer de två sidorna på samma sida att vara konsekventa.

Även om det är önskvärt och bästa praxis att alltid vara konsekvent på en enda webbsida, behandlar 3.2.4 endast enhetlighet inom en uppsättning webbsidor där något upprepas på mer än en sida i uppsättningen.

#### Hur man uppfyller kraven - konsekvent identifiering (3.2.4) {#how-to-meet-consistent-identification}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas ](https://www.w3.org/WAI/WCAG21/quickref/#consistent-identification).

#### Mer information - Konsekvent identifiering (3.2.4) {#more-information-consistent-identification}

* [Förstå villkor för slutförande 3.2.4](https://www.w3.org/WAI/WCAG21/Understanding/consistent-identification.html)
* [Så här uppfyller du villkoren 3.2.4](https://www.w3.org/WAI/WCAG21/quickref/#consistent-identification)

### Ingångsstöd (3.3) {#input-assistance}

[Riktlinje 3.3 Hjälp med inmatning: Hjälp användarna att undvika och rätta till misstag.](https://www.w3.org/TR/WCAG/#input-assistance)

### Felidentifiering (3.3.1)  {#error-identification}

* Villkor för lyckat resultat 3.3.1
* Nivå A
* Felidentifiering: Om ett indatafel upptäcks automatiskt identifieras det felande objektet och felet beskrivs till användaren i texten.

#### Syfte - Felidentifiering (3.3.1) {#purpose-error-identification}

Syftet med detta villkor är att se till att användarna är medvetna om att ett fel har inträffat och kan avgöra vad som är fel. Felmeddelandet ska vara så specifikt som möjligt. Om det inte går att skicka in ett formulär, räcker det inte att återvisa formuläret och ange felfälten för att vissa användare ska inse att ett fel har inträffat. Användare av skärmläsare vet till exempel inte om ett fel uppstod förrän de stöter på en av indikatorerna. De kan hoppa över hela formuläret innan felindikatorn påträffas, eftersom de tror att sidan helt enkelt inte fungerar. Enligt definitionen i WCAG är ett [indatafel](https://www.w3.org/TR/WCAG/#dfn-input-error) information som användaren anger och som inte accepteras. Detta inkluderar följande:

Information som krävs av webbsidan men utelämnas av användaren, eller information som tillhandahålls av användaren men som ligger utanför det obligatoriska dataformatet eller tillåtna värden.
Till exempel:

* Användaren underlåter att ange rätt förkortning i fältet för delstat, provins eller region.
* användaren skriver en lägesförkortning som inte är ett giltigt tillstånd,
* användaren anger ett postnummer eller postnummer som inte finns,
* Användaren skriver in ett födelsedatum två år i framtiden.
* användaren skriver in alfabetiska tecken eller parenteser i sitt telefonnummerfält som endast accepterar siffror,
* användaren lägger ett bud som är lägre än föregående bud eller den lägsta anbudsökningen.

#### Så här möts du - Felidentifiering (3.3.1) {#how-to-meet-error-identification}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 3.3.1](https://www.w3.org/WAI/WCAG21/quickref/#error-identification).

#### Mer information - Felidentifiering (3.3.1) {#more-information-error-identification}

* [Förstå villkor för slutförande 3.3.1](https://www.w3.org/WAI/WCAG21/Understanding/error-identification.html)
* [Så här uppfyller du villkoren 3.3.1](https://www.w3.org/WAI/WCAG21/quickref/#error-identification)

### Etiketter eller instruktioner (3.3.2) {#labels-or-instructions}

* Villkor för lyckat resultat 3.3.2
* Nivå A
* Etiketter eller instruktioner: Etiketter eller instruktioner tillhandahålls när innehållet kräver indata från användaren.

#### Syfte - Etiketter eller instruktioner (3.3.2) {#purpose-labels-or-instructions}

Att ge instruktioner som hjälper människor att fylla i formulär är en grundläggande del av god praxis när det gäller gränssnittsanvändning. Detta är användbart för personer med nedsatt syn eller kognitiva funktionshinder som annars kan ha svårt att förstå layouten i ett formulär och den typ av data som ska anges i ett visst formulärfält.

##### Forms

I AEM WKND-demoprojektet läggs en standardetikett till när du lägger till en formulärkomponent, till exempel ett **textfält**, på sidan. Den här standardtiteln beror på komponenttypen. Du kan lägga till din egen titel på fliken **Titel och Text** i redigeringsdialogrutan för det fältet. Det är viktigt att se till att etiketter hjälper användarna att förstå informationen som är kopplad till varje formulärkomponent.

Det här fältet **Rubrik** måste användas för fältelement eftersom det innehåller en etikett som är tillgänglig för hjälpmedelsteknik. Det räcker inte att bara skriva en etikett bredvid fältet.

För vissa formulärkomponenter går det även att dölja etiketter visuellt med kryssrutan **Dölj rubrik** . Etiketter som döljs på det här sättet är fortfarande tillgängliga för hjälpfunktioner, men de visas inte på skärmen. Detta kan vara en bra metod i vissa situationer, men det är bäst att ta med en visuell etikett där det är möjligt, eftersom vissa användare kanske tittar på ett litet avsnitt på skärmen (ett fält i taget) och behöver etiketterna för att kunna identifiera fältet på rätt sätt.

###### Bildknappar {#image-buttons}

Där bildknappar används (till exempel komponenten **Bildknapp** i WKND-projektet), innehåller fältet **Titel** på fliken **Titel och text** i redigeringsdialogrutan den alternativa texten för bilden, i stället för etiketten. I exemplet nedan har bilden med texten `Submit` Alt-texten `Submit`, som lagts till med fältet **Titel** i redigeringsdialogrutan.

###### Grupper med formulärfält {#groups-of-form-fields}

I WKND-projektet, där det finns en grupp med relaterade kontroller, till exempel **Alternativgrupp**, kan en rubrik behövas för gruppen och enskilda kontroller. När du lägger till en uppsättning med alternativknappar i AEM visas den här grupptiteln i fältet **Titel**, medan enskilda titlar anges när alternativknapparna (**Objekt**) skapas.

Det finns dock ingen programmatisk koppling mellan grupptiteln och alternativknapparna själva. Mallredigerare måste kapsla in titeln i de `fieldset` - och `legend` -taggar som krävs för att skapa den här kopplingen, och detta kan bara göras genom att redigera sidans källkod. En systemadministratör kan också lägga till stöd för dessa element så att de visas i dialogrutan **Fältegenskaper** (se [Lägga till stöd för ytterligare HTML Elements och attribut](/help/sites-administering/rte-accessible-content.md#add-support-for-more-html-elements-and-attributes)).

###### Ytterligare överväganden för Forms {#additional-considerations-for-forms}

Om data ska matas in i ett visst format bör du göra detta tydligt i etikettexten. Om till exempel ett datum måste anges i formatet `DD-MM-YYYY` anger du det här som en del av etiketten. Det innebär att när skärmläsaranvändare stöter på fältet visas etiketten automatiskt tillsammans med ytterligare information om formatet.

Om indata för ett formulärfält är obligatoriska klargör du detta genom att använda ordet ”required” som en del av etiketten. AEM lägger till en asterisk när ett fält är obligatoriskt, men det är bra att inkludera ordet `required` i själva etiketten (i fältet **Titel** i redigeringsdialogrutan).

Placeringen av etiketter är också viktig eftersom den hjälper dem att hitta rätt fält. Detta är särskilt viktigt när användaren har ett komplext formulär. Följ konventionen nedan:

* Kryssrutor eller alternativknappar:
Etiketterna placeras direkt till höger om fältet.
* Alla andra formulärkomponenter (till exempel textrutor, kombinationsrutor):
Etiketterna placeras antingen direkt ovanför eller direkt till vänster om fältet.

I enkla formulär med begränsad funktionalitet kan etikettering av en `Submit`-knapp fungera som etikett för det intilliggande fältet (till exempel `Search`). Detta är användbart när det kan vara svårt att hitta plats för etikettexten.

#### Mer information - etiketter eller instruktioner (3.3.2) {#more-information-labels-or-instructions}

* [Förstå villkor 3.3.2](https://www.w3.org/WAI/WCAG21/Understanding/labels-or-instructions.html)
* [Så här uppfyller du villkor 3.3.2](https://www.w3.org/WAI/WCAG21/quickref/#labels-or-instructions)

### Felförslag (3.3.3)  {#error-suggestion}

* Villkor för lyckat resultat 3.3.3
* Nivå AA
* Tangentbord: Om ett indatafel upptäcks automatiskt och förslag på korrigering är kända, får användaren förslag, såvida det inte skulle äventyra innehållets säkerhet eller syfte.

#### Syfte - Felförslag (3.3.3) {#purpose-error-suggestion}

Syftet med detta villkor är att se till att användarna får lämpliga förslag på hur ett indatafel kan korrigeras om det är möjligt. WCAG-definitionen för [indatafel](https://www.w3.org/TR/WCAG/#dfn-input-error) anger att det är &quot;information som användaren anger som inte accepteras&quot; av systemet. Några exempel på information som inte accepteras är information som är obligatorisk men utelämnad av användaren och information som tillhandahålls av användaren men som ligger utanför det obligatoriska dataformatet eller tillåtna värden.

Success Criterion 3.3.1 innehåller meddelanden om fel. Personer med kognitiva begränsningar kan dock finna det svårt att förstå hur felen ska korrigeras. Personer med visuella funktionshinder kanske inte kan komma på exakt hur felet ska korrigeras. Om det inte går att skicka in formuläret kan användaren överge det eftersom han/hon kanske inte vet hur felet ska åtgärdas trots att han/hon vet att det har inträffat.

Innehållsförfattaren kan ge en beskrivning av felet eller så kan användaragenten ge en beskrivning av felet baserat på teknikspecifik, programmässigt bestämd information.

#### Så här möts du - Felförslag (3.3.3) {#how-to-meet-error-suggestion}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 3.3.3](https://www.w3.org/WAI/WCAG21/quickref/#error-suggestion).

#### Mer information - Felförslag (3.3.3) {#more-information-error-suggestion}

* [Förstå villkor för slutförande 3.3.3](https://www.w3.org/WAI/WCAG21/Understanding/error-suggestion.html)
* [Så här uppfyller du villkoren 3.3.3](https://www.w3.org/WAI/WCAG21/quickref/#error-suggestion)

### Förebygga fel (juridisk, finansiell, data) (3.3.4)  {#error-prevention-legal-financial-data}

* Villkor för lyckat resultat 3.3.4
* Nivå AA
* Felförebyggande (juridisk, ekonomisk, datarelaterad): För webbsidor som gör att juridiska åtaganden eller ekonomiska transaktioner för användaren inträffar, som ändrar eller tar bort användarstyrda data i datalagringssystem eller som skickar in testsvar från användaren är minst ett av följande sant:

   * Reversible
Inlämningarna kan återställas.
   * Markerad
Data som anges av användaren kontrolleras för indatafel och användaren ges möjlighet att korrigera dem.
   * Bekräftat
Det finns en mekanism för att granska, bekräfta och korrigera information innan inlämningen är klar.

#### Syfte - Förebyggande av fel (rättsliga, finansiella, uppgifter) (3.3.4) {#purpose-error-prevention-legal-financial-data}

Syftet med detta kriterium är att hjälpa användare med funktionshinder att undvika allvarliga konsekvenser till följd av ett misstag när de utför en åtgärd som inte kan ångras. Exempel: köp av icke-återbetalningsbara flygbiljetter eller inlämning av en order om att köpa aktier på ett mäklarkonto är finansiella transaktioner med allvarliga följder. Om en användare har gjort ett misstag på flygresedagen kan det resultera i en biljett för fel dag som inte kan bytas ut. Om användaren begick ett misstag i fråga om antalet aktier som skulle köpas kan det resultera i att användaren köper mer aktier än vad han/hon hade tänkt sig. Båda dessa typer av kostsamma misstag inbegriper transaktioner som äger rum omedelbart och inte kan ändras i efterhand. På samma sätt kan det vara ett oåterkalleligt fel om användare oavsiktligt ändrar eller tar bort data som lagras i en databas som de senare måste komma åt, till exempel hela reseprofilen på en webbplats för resetjänster. När det gäller ändring eller borttagning av användarkontrollerbara data är avsikten att förhindra massförlust av data som att ta bort en fil eller post. Det är inte avsikten att kräva en bekräftelse för varje Spara-kommando eller att enkelt skapa eller redigera dokument, poster eller andra data.

Användare med funktionshinder kan vara mer benägna att göra misstag. Personer med lässvårigheter kan transponera siffror och bokstäver, och personer med motoriska funktionshinder kan råka ut för nycklar av misstag. Om användaren kan ångra åtgärder kan användaren rätta till ett misstag som kan få allvarliga följder. Genom att tillhandahålla möjligheten att granska och korrigera information kan användaren upptäcka ett misstag innan han eller hon vidtar en åtgärd som får allvarliga följder.

Användarstyrda data är användaranpassade data som användaren kan ändra och/eller ta bort genom en avsiktlig åtgärd. Exempel på användare som kontrollerar sådana data är att uppdatera telefonnumret och adressen för användarens konto eller att ta bort en post med tidigare fakturor från en webbplats. Det refererar inte till sådant som Internet-loggar och data för sökmotorövervakning som användaren inte kan visa eller interagera med direkt.

#### Hur man ska uppfylla kraven - Förebyggande av fel (rättsliga, finansiella, uppgifter) (3.3.4) {#how-to-meet-error-prevention-legal-financial-data}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas &lbrace;3.3.4](https://www.w3.org/WAI/WCAG21/quickref/#error-prevention-legal-financial-data).

#### Mer information - Felförebyggande (Juridik, Finans, Data) (3.3.4) {#more-information-error-prevention-legal-financial-data}

* [Förstå villkor för slutförande 3.3.4](https://www.w3.org/WAI/WCAG21/Understanding/error-prevention-legal-financial-data.html)
* [Så här uppfyller du kriterierna 3.3.4](https://www.w3.org/WAI/WCAG21/quickref/#error-prevention-legal-financial-data)

## Princip 4: Robust {#principle-robust}

[Princip 4: Robust - Innehållet måste vara tillräckligt robust så att det kan tolkas av ett stort antal användaragenter, inklusive hjälpmedelstekniker.](https://www.w3.org/TR/WCAG/#robust)

### Kompatibel (4.1) {#compatible}

[Riktlinje 4.1 Kompatibel: Maximera kompatibiliteten med nuvarande och framtida användaragenter, inklusive hjälpmedelstekniker.](https://www.w3.org/TR/WCAG/#compatible)

Maximera kompatibiliteten med nuvarande och framtida användaragenter, inklusive hjälpmedelstekniker.

### Analys (4.1.1)  {#parsing}

* Kriteriet 4.1.1 lyckades
* Nivå A
* Analys: I innehåll som implementeras med hjälp av markeringsspråk har elementen fullständiga start- och sluttaggar, elementen är kapslade enligt deras specifikationer, elementen innehåller inte dubblettattribut och alla ID:n är unika, utom när specifikationerna tillåter dessa funktioner.

#### Syfte - Analys (4.1.1) {#purpose-parsing}

Syftet med detta kriterium är att se till att användaragenter, inklusive hjälpmedelstekniker, kan tolka och tolka innehåll korrekt. Om innehållet inte kan tolkas i en datastruktur kan olika användaragenter presentera det annorlunda eller inte kunna tolka det. Vissa användaragenter använder&quot;reparationstekniker&quot; för att återge dåligt kodat innehåll.

Eftersom reparationstekniken varierar mellan olika användaragenter kan man inte anta att innehållet tolkas korrekt i en datastruktur eller att det återges korrekt av specialiserade användaragenter, inklusive hjälpmedelstekniker, såvida inte innehållet skapas enligt reglerna som definieras i den formella grammatiken för den tekniken. I kodspråk leder fel i elementsyntax och attributsyntax samt misslyckande med att tillhandahålla korrekt kapslade start-/sluttaggar till fel som förhindrar att användaragenter tolkar innehållet på ett tillförlitligt sätt. Därför kräver resultatvillkoret att innehållet kan tolkas med enbart reglerna för den formella grammatiken.

#### Hur man möter - parsing (4.1.1) {#how-to-meet-parsing}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 4.1.1](https://www.w3.org/WAI/WCAG21/quickref/#parsing).

#### Mer information - Analys (4.1.1) {#more-information-parsing}

* [Förstå villkor 4.1.1](https://www.w3.org/WAI/WCAG21/Understanding/parsing.html)
* [Så här uppfyller du kriterierna för lyckade försök 4.1.1](https://www.w3.org/WAI/WCAG21/quickref/#parsing)

### Namn, roll, värde (4.1.2)  {#name-role-value}

* Kriteriet 4.1.2
* Nivå A
* Namn, Roll, Värde: För alla användargränssnittskomponenter (inklusive men inte begränsat till: formulärelement, länkar och komponenter som genereras av skript) kan namnet och rollen bestämmas programmatiskt, tillstånd, egenskaper och värden som kan anges av användaren kan anges programmatiskt och meddelanden om ändringar av dessa objekt är tillgängliga för användaragenter, inklusive hjälpmedelstekniker.

#### Syfte - Namn, roll, värde (4.1.2) {#purpose-ame-role-value}

Syftet med detta villkor är att se till att hjälpfunktioner kan samla in information om, aktivera (eller ställa in) och hålla sig uppdaterade om statusen för användargränssnittskontrollerna i innehållet.

När standardkontroller från hjälpmedelstekniker används är den här processen enkel. Om användargränssnittets element används enligt specifikationen är villkoren i denna bestämmelse uppfyllda. (Se exempel på villkor för att lyckas 4.1.2 nedan)

Om anpassade kontroller skapas, eller gränssnittselement programmeras (i kod eller skript) för att ha en annan roll och/eller funktion än vanligt, måste ytterligare åtgärder vidtas för att säkerställa att kontrollerna tillhandahåller viktig information för hjälpmedelstekniker och gör att de kan styras av hjälpmedelstekniker.

Ett viktigt läge för en användargränssnittskontroll är om den har fokus eller inte. Fokusläget för en kontroll kan fastställas programmatiskt och meddelanden om fokusändring skickas till användaragenter och hjälpmedelsteknik. Andra exempel på kontrollstatus för användargränssnittet är om en kryssruta eller alternativknapp har markerats. Eller om ett komprimeringsbart träd eller en listnod är expanderad eller komprimerad.

#### Så här möts du - namn, roll, värde (4.1.2) {#how-to-meet-ame-role-value}

Följ riktlinjerna under [Så här uppfyller du kriterierna för att lyckas 4.1.2](https://www.w3.org/WAI/WCAG21/quickref/#name-role-value).

#### Mer information - namn, roll, värde (4.1.2 {#more-information-ame-role-value}

* [Förstå villkor 4.1.2](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value.html)
* [Så här uppfyller du kriterierna 4.1.2](https://www.w3.org/WAI/WCAG21/quickref/#name-role-value)
