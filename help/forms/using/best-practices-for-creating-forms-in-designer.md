---
title: Bästa tillvägagångssätt för att skapa formulär i formulärdesigner
description: Lär dig mer om de bästa hjälpmedelsrutinerna för att skapa formulär i formulärdesigner
feature: Adaptive Forms, Forms Designer
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
source-git-commit: 1a4df932f0933e9644b40eb5d02900231b6a48c6
workflow-type: tm+mt
source-wordcount: '11687'
ht-degree: 0%

---

# Bästa tillvägagångssätt för att skapa formulär i formulärdesigner

Med LiveCycle Designer kan du skapa avancerat formulärinnehåll och följa riktlinjerna i Section 508. Den här guiden innehåller en översikt över de effektivaste strategierna för att skapa hjälpmedelsförberedda formulär och riktlinjer för hur du implementerar dessa metodtips med LiveCycle Designer. Följande bästa metoder beskrivs:

1. [Gör formulären enkla att använda](#keep-simple)
1. [Konfigurera formuläregenskaper för att generera hjälpmedelsinformation](#configure-form-properties)
1. [Välj rätt kontroller](#choose-right-controls)
1. [Ange textmotsvarigheter för bilder](#provide-text-equivalents)
1. [Ange korrekta etiketter för formulärkontroller](#provide-proper-labels)
1. [Kontrollera att läs- och tabbordningen är korrekt](#ensure-reading-tab-order)
1. [Se till att formulärkontrollerna är tangentbordstillgängliga](#ensure-keyboard-accessible)
1. [Använda färger på ett ansvarsfullt sätt](#use-color-responsibly)
1. [Ange rubrikceller för tabeller](#provide-heading-cells)
1. [Ange en navigeringsbar formulärstruktur](#provide-navigable-form)
1. [Undvik störande skript](#avoid-disruptive-scripting)
1. [Se till att allt ljud- och videoinnehåll är tillgängligt](#ensure-audio-video-accessible)
1. [Identifiera naturligt språk och eventuella språkändringar](#identify-natural-language)

## Gör formulären enkla att använda {#keep-simple}

Ett formulär är inte tillgängligt om det inte är lätt att använda. Du bör försöka utforma formulär som är enkla och användbara. En enkel layout med kontroller och fält med tydliga, meningsfulla beskrivningar och verktygstips gör formuläret mycket enklare för alla användare.
Att utforma formulär som är tydliga och logiskt ordnade och som innehåller tydliga och enkla instruktioner hjälper alla användare att fylla i formulär så enkelt som möjligt. Navigeringsfunktionerna, t.ex. tabbordningen och kortkommandona, bör ha stöd för den logiska ordningen för objekten i formuläret.

### Undvik att flytta, blinka och blinka

Vissa individer med fotokänslig epilepsi kan ha en anfall som utlöses av rörelser i frekvenser som är större än 2 Hz (1 Hz, eller Hertz, är lika med en per sekund) och lägre än 55 Hz (55 per sekund).

Rörelse vid mindre än 2 Hz anses vara tillräckligt långsam för att vara säker för individer med fotokänslig epilepsi. Rörelse vid mer än 55 Hz anses vara oförutsägbar.

Utvecklarna bör vara medvetna om de här parametrarna när de använder rörelser i webbinnehåll.

Andra användare kan ha kognitiva funktionshinder som gör det svårt att koncentrera sig när det finns animerat eller blinkande innehåll i formuläret.

I allmänhet bör du undvika att använda optiska effekter som infogats av skript, till exempel blinkande text eller animering, i interaktiva formulär. Sådana effekter minskar formulärens användbarhet för vissa användare.

Relaterade kontrollpunkter
* Section 508 §11934.21

   * h) När animering visas skall informationen visas i minst ett presentationsläge utan animering, om användaren så önskar.
   * k) Programvaran får inte använda blinkande eller blinkande text, objekt eller andra element som har en blixt- eller blinkfrekvens som är högre än 2 Hz och lägre än 55 Hz.
* Section 508 §11934.22
   * (j) Sidorna ska vara utformade så att de inte orsakar att skärmen flimrar med en frekvens som är högre än 2 Hz och lägre än 55 Hz.
* WCAG 1.0
   * 7.1 Undvik att skärmen flimrar tills användaragenterna tillåter användaren att styra flimmer. (P1)
   * 7.2 Innan användaragenterna tillåter användare att kontrollera blinkning bör du undvika att innehållet blinkar (dvs. ändra presentationen med en jämn hastighet, som att slå på och av) (P2).
   * 7.3 Undvik att flytta sidor tills användaragenten tillåter användarna att frysa rörligt innehåll.
   * 14.1 Använd det tydligaste och enklaste språk som passar för en webbplats innehåll.
* WCAG 2.0
   * 2.2.2 Pausa, stoppa, dölj: För att flytta, blinka, rulla eller uppdatera information automatiskt gäller följande: (nivå A)
   * 2.3.1 Tre blinkningar eller under tröskelvärde: Webbsidor innehåller inte något som blinkar mer än tre gånger under en sekund eller blixten är under de allmänna tröskelvärdena för blixt och rött. (nivå A)
   * 2.3.2 Tre blinkningar: Webbsidor innehåller inte något som blinkar mer än tre gånger under en sekund. (nivå AAA)


## Konfigurera formuläregenskaper för att generera hjälpmedelsinformation {#configure-form-properties}

För att ett formulär ska vara tillgängligt måste det vara [perceptiellt](https://www.w3.org/TR/WCAG20/#perceivable) av hjälpmedelsteknik. De flesta skärmläsare tar till exempel inte hänsyn till formulärets visuella layout, utan till den underliggande strukturen.

Om du vill implementera den underliggande strukturen med LiveCycle Designer måste du skapa ett PDF-formulär med hjälpmedelsinformation (kallas ibland taggar) så att skärmläsaren eller annan hjälpmedelsteknik kan läsa formulärets text och komponenter. I ett formulär med tillgänglighetsinformation innehåller varje element information om sin egen struktur, plus information om hur den är relaterad till eller beroende av andra element. Det är bara i PDF-filer som innehåller hjälpmedelsinformation som skärmläsare kan identifiera och beskriva innehållet i ett dokument korrekt.

Om du vill skapa ett hjälpmedelsanpassat formulär måste du konfigurera formuläregenskaperna så att LiveCycle Designer genererar hjälpmedelsinformation när du sparar formulärdesignen som en PDF-fil:
1. Välj Arkiv > Formuläregenskaper
1. Klicka på fliken Alternativ för spara och kontrollera att Generera tillgänglighetsinformation (taggar) för Acrobat är markerat i området PDF.
1. Klicka på OK.

I LiveCycle Designer är det här alternativet markerat som standard.

>[!NOTE]
> Dessa alternativ gäller endast när du sparar formulärdesignen som en PDF-fil. De gäller inte för PDF-filer som skapats med LiveCycle Forms som har konfigurationsalternativ som är oberoende av det här alternativet i LiveCycle Designer.

**Relaterade kontrollpunkter**

* Section 508 §1194.21
   * d) Tillräcklig information om ett element i användargränssnittet, inklusive elementets identitet, funktion och tillstånd, skall finnas tillgänglig för hjälpmedelsteknik. När en bild representerar ett programelement måste den information som bilden förmedlar också vara tillgänglig i text.
   * l) När elektroniska formulär används skall formuläret ge personer som använder hjälpmedel möjlighet att få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in formuläret, inklusive alla anvisningar och tips.
* Section 508 §1194.22
   * n) När elektroniska blanketter är avsedda att fyllas i online skall de personer som använder hjälpmedel få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in blanketten, inklusive alla anvisningar och anvisningar.


## Välj rätt kontroller {#choose-right-controls}

När du utformar formulär ska du använda utvecklingsobjekt från flikarna i LiveCycle Designer objektbibliotek. Du kan visa den här panelen genom att välja Fönster > Objektbibliotek eller genom att trycka på Skift+F12 (se figur 1).

![Objektbibliotekspanelen](/help/forms/using/assets/image-1.png)

Bild 1: **Objektbibliotekspanelen**

Om du använder andra objekt kan de ignoreras av hjälpfunktioner. Om du bara använder standardobjekten slipper du definiera hjälpmedelsegenskaper för objekt som du själv har skapat. Om du skapar och använder egna anpassade objekt måste du använda hjälpmedelspaletten för att ange hjälpmedelsegenskaper som Roll, Verktygstips, Skärmprioritet och Reader-text för anpassade skärmar. Om du vill visa hjälpmedelspaletten väljer du Fönster > Hjälpmedel.

**Relaterade kontrollpunkter**
* Section 508 §1194.21
   * c) En väldefinierad indikation på skärmen av det aktuella fokus som rör sig mellan interaktiva gränssnittselement när indatafokus ändras. Fokus ska visas programmatiskt så att hjälpmedelstekniken kan spåra fokus- och fokusförändringar.
   * d) Tillräcklig information om ett element i användargränssnittet, inklusive elementets identitet, funktion och tillstånd, skall finnas tillgänglig för hjälpmedelsteknik. När en bild representerar ett programelement måste den information som bilden förmedlar också vara tillgänglig i text.
   * l) När elektroniska formulär används skall formuläret ge personer som använder hjälpmedel möjlighet att få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in formuläret, inklusive alla anvisningar och tips.
* Section 508 §1194.22
   * n) När elektroniska blanketter är avsedda att fyllas i online skall de personer som använder hjälpmedel få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in blanketten, inklusive alla anvisningar och anvisningar.

* WCAG 2.0
   * 3.2.4 Konsekvent identifiering: Komponenter som har samma funktioner på en uppsättning webbsidor identifieras på ett enhetligt sätt. (nivå AA).
   * 4.1.2 Namn, Roll, Värde: För alla komponenter i användargränssnittet (inklusive men inte begränsat till: formulärelement, länkar och komponenter som genereras av skript) kan namnet och rollen bestämmas programmatiskt, tillstånd, egenskaper och värden som kan anges av användaren kan anges programmatiskt och meddelanden om ändringar av dessa objekt är tillgängliga för användaragenter, inklusive hjälpmedelstekniker. (nivå A)


## Ange textmotsvarigheter för bilder {#provide-text-equivalents}

Bilder kan hjälpa till att förbättra förståelsen för användare med vissa typer av funktionshinder. För användare av skärmläsare minskar dock bilderna formulärets tillgänglighet om du inte anger något textalternativ.

Om du väljer att använda bilder anger du textbeskrivningar för alla bild- och bildfältsobjekt. Kontrollera att texten beskriver objektet och dess syfte i formuläret. När du definierar ett textalternativ läser skärmläsaren upp det här alternativet när bilden påträffas. Därför måste en bild som innehåller information alltid ha ett textalternativ angivet.

Du kan ange textbeskrivningar med hjälp av egenskaperna Verktygstips eller Egen skärmbild, Reader-text på paletten Tillgänglighet eller via textfält, bildtexter och objektnamn, som du har angett i alternativet Namn på fliken Bindning. I bild 2 visas till exempel ett exempel på en bild som innehåller texten&quot;Get Adobe Reader&quot;. Eftersom en skärmläsare inte kan läsa text som är en del av en bild bör du ta med ett textalternativ i fältet Reader-text för anpassad skärm på hjälpmedelspaletten för det här objektet. I de flesta fall bör den alternativa texten vara densamma som texten som visas i bilden (se figur 2).

![Ange alternativ text för en bild med hjälpmedelspaletten](/help/forms/using/assets/image-2.png)

Bild 2: **Ange alternativ text för en bild med hjälpmedelspaletten**

Tänk på följande när du anger alternativ text:
* Om bildobjektet eller den skannade bilden innehåller viktig information för formuläret skapar du text för bilden på paletten Tillgänglighet som beskriver objektet och dess syfte. Texten för en företagslogotyp kan t.ex. bestå av orden&quot;företagslogotyp&quot; och företagets namn.
* Om bildobjektet innehåller semantisk färginformation tar du även med den i beskrivningen. En beskrivning av ett grönt trafikljus kan till exempel vara &quot;Överföringen lyckades&quot; och beskrivningen av ett rött ljus kan vara &quot;Överföringen misslyckades&quot;.
* Om du använder komplex grafik, till exempel stapeldiagram, anger du informationen i en alternativ tillgänglig version, till exempel en tabell eller längre textbeskrivning.
* Skapa inte textbeskrivningar för statiska bilder som bara används för dekoration.
* Använd inte skannade data som bakgrundsinformation. Det här kan hända när en designer skannar ett utskriftsformulär och använder Adobe LiveCycle Designer för att lägga till nya fält i formuläret. Skärmläsare kan inte identifiera inlästa data i det här läget.

När du lägger in rent dekorativt grafiskt innehåll i formulären bör du se till att skärmläsarna inte meddelar om bilden. För de flesta skärmläsare kan du uppnå detta genom att ange Reader Skärmtext till Ingen på paletten Tillgänglighet. Om du inte gör det kan vissa skärmläsare meddela att det finns en bild, utan att ange vad bilden representerar. För dynamiska bilder, t.ex. bildfältsobjekt, måste du se till att textalternativen uppdateras korrekt när bilden ändras. Skapa inte textbeskrivningar för bildfältsobjekt som bara används för dekoration. Du kan använda skriptspråket FormCalc för att tilldela textbeskrivningar till ett bildfältsobjekt dynamiskt. FormCalc är standardskriptspråket i Adobe LiveCycle Designer. Ta till exempel ett formulär med bildfältet ImageField1 och tillhörande text i bildtextnoden för körningsdata. Du kan använda skript för att skicka den här texten i en lämplig händelse (till exempel `form:ready`) enligt följande:

`ImageField1.assist.toolTip = $record.imagetext.value`

Relaterade kontrollpunkter
* Section 508 §1194.22
   * a) En textmotsvarighet för varje icke-textelement skall anges (t.ex. via &quot;alt&quot;, &quot;long&quot; eller i elementinnehållet).
* WCAG 1.0
   * 1.1 Ange en textmotsvarighet för alla icke-textelement (t.ex. via &quot;alt&quot;, &quot;long&quot; eller i elementinnehållet). Detta inkluderar: bilder, grafiska representationer av text (inklusive symboler), bildschemaområden, animeringar (t.ex. animerade GIF-filer), miniprogram och programmatiska objekt, ascii-bilder, bildrutor, skript, bilder som används som listpunkter, mellanrum, grafiska knappar, ljud (spelas upp med eller utan användarinteraktion), fristående ljudfiler, ljudspår för video och video (P1).
* WCAG 2.0
   * 1.1.1 Innehåll som inte är text: Allt innehåll som inte är text och som presenteras för användaren har ett textalternativ som har samma syfte, förutom de situationer som anges nedan. (nivå A)


## Ange korrekta etiketter för formulärkontroller{#provide-proper-labels}

Etiketten eller bildtexten för en formulärkontroll identifierar vad formulärkontrollen ska representera. Texten &quot;Förnamn&quot; anger till exempel att användaren måste ange sitt förnamn i ett textfält. För att skärmläsare ska kunna komma åt etiketten måste den vara programmatiskt kopplad till formulärkontrollen, annars måste formulärkontrollen konfigureras med ytterligare hjälpmedelsinformation med hjälpmedelspaletten. Det räcker inte att bara placera ett textobjekt bredvid kontrollen. För användare med synnedsättning eller nedsatt syn är det viktigt att etiketten är rätt placerad intill kontrollen. Båda teknikerna beskrivs i följande avsnitt.

### Ange hjälpmedelsanpassad etikettext med paletten Tillgänglighet

Etiketten som upplevs av skärmläsaranvändare behöver inte nödvändigtvis vara samma som den visuella bildtexten. I vissa fall kanske du vill vara mer specifik när det gäller kontrollens syfte.
För varje fältobjekt i ett formulär kan hjälpmedelspaletten (se figur 3) användas för att ange vad skärmläsaren ska meddela för att identifiera det specifika formulärfältet.

Så här använder du paletten Tillgänglighet:

1. Visa paletten Tillgänglighet genom att välja Fönster > Tillgänglighet eller genom att trycka på Skift+F6.
1. Markera ett objekt i formuläret. På paletten visas objektets hjälpmedelsegenskaper.

![Paletten Tillgänglighet](/help/forms/using/assets/image-3.png)

Bild 3: **Paletten Tillgänglighet**

När formuläret sparas som PDF söker LiveCycle Designer igenom formuläret efter egenskaperna Egen text, Verktygstips, Bildtext och Namn, i den ordningen, för att hitta text som ska läsas av skärmläsare. Du kan åsidosätta den här standardordningen genom att använda alternativet Företräde för skärm-Reader på paletten Tillgänglighet:

1. Markera objektet i formulärdesignen.
1. Klicka på paletten Tillgänglighet.
1. Markera ett annat alternativ för Skärmprioritet för Reader än Ingen.

Följande alternativ är tillgängliga:

* **Egen text** som du anger i fältet Reader-text på hjälpmedelspaletten. Med det här alternativet kan du ange vilken text som helst som du vill att hjälpmedelstekniken, t.ex. skärmläsare, ska använda. Det är bäst att använda bildtextinställningen i de flesta situationer. Att skapa Reader-text för anpassade skärmar bör endast vara ett alternativ när det inte går att använda bildtexten eller en funktionsbeskrivning.
* **Verktygstips** som du anger i fältet Verktygstips på paletten Tillgänglighet. För de flesta objekt visas verktygstips vid körning när användaren håller pekaren över objektet. Verktygstips visas endast för vissa skrivskyddade objekt, till exempel ett streckkodsobjekt för ett pappersformulär, när en skärmläsare används.
* **Bildtext**, vilket gör att LiveCycle Designer använder formulärfältets associerade (visuella) etikett som skärmläsartext.
* **Namn** som du anger i fältet Namn på fliken Bindning. Observera att namnet inte får innehålla blanksteg.
* **Inget**, vilket gör att objektet inte har något namn. Detta rekommenderas aldrig för formulärkontroller.

Tänk på följande när du använder paletten Tillgänglighet för formulärkontrollsetiketter:

* Om bildtexten för formulärkontrollen beskriver kontrollen på rätt sätt kan du använda den för skärmläsare. I det här fallet låter du både fälten Egen text och Funktionsbeskrivning vara tomma på paletten Tillgänglighet, eller ändrar inställningen för Reader Skärmprioritet till Bildtext.
* När skärmläsare används som mål är det ingen mening med att ange olika textbeskrivningar för samma formulärkontroll, eftersom endast ett av dem används: Det första icke-tomma fältet i prioritetsordningen för skärmbild Reader. Det finns till exempel ingen anledning att ange både anpassad text och verktygstips för en skärmläsare.
* Som standard läser skärmläsaren bildtexten om inget anges i rutan Verktygstips eller i rutan Reader-text för anpassad skärm.
* Använd inte paletten Tillgänglighet för att skapa beskrivningar för osynliga fält eller områden.
* Om du måste skapa en beskrivning med alternativen Verktygstips eller Egen skärmtext i Reader, ska du alltid inkludera den bildtext som är synlig i formuläret, förutom när den synliga bildtexten inte är meningsfull, till exempel när själva bildtexten är förkortad. Detta gör att skärmläsaranvändare kan kommunicera effektivt med andra användare om gränssnittselement. Dessa olika användargrupper har svårt att identifiera samma gränssnittselement om bildtexten skiljer sig från Reader-texten för verktygstips eller anpassade skärmar.
* För kryssrutor och listrutekontroller i tabellceller kommer skärmläsaren att meddela vilken bildtext, funktionsbeskrivning eller anpassad skärmläsartext du har angett för objektet. Om du vill använda kolumnrubriken för den alternativa texten för dessa objekt när de placeras i en tabell ska du inte ange någon beskrivning, funktionsbeskrivning eller anpassad uppläsningstext.
* Om kontrollen kräver ytterligare instruktioner, se till att de också finns med i textalternativet. Inkludera tillräckligt med talad information så att användarna vet vilka indata som förväntas och hur fältet ska fyllas i korrekt, men överbelasta inte användare med redundant information.
* Lämna inte information som beskriver hur du använder kontroller i onödan - låt användarens hjälpfunktioner hantera detta åt användaren. Användarna kan konfigurera den utförliga färgen så att den passar deras komfort.

Bild 4 visar ett exempel på ett textfält med en visuell bildtext som kan vara oklar för vissa skärmläsaranvändare. I det här exemplet är Reader-text för anpassad skärm inställd på&quot;Antal sidor&quot; och Reader-prioritet för skärm är inställd på Anpassad text. Därför kommer den faktiska (visuella) bildtexten (&quot;# av sidor&quot;) inte att användas av skärmläsaren. Ett verktygstips kan också ha angetts.

![Ange Reader-text för anpassad skärm när den synliga etiketten inte räcker till](/help/forms/using/assets/image-4.png)

Bild 4: **Ange Reader-text för anpassad skärm när den synliga etiketten är otillräcklig**

### Ange etiketter för alternativknappar

När en användare med synnedsättning tabbar in i en alternativknapp måste skärmläsaren läsa två saker:
* En indikation på syftet med gruppen med alternativknappar
* En meningsfull etikett för varje alternativknapp
Så här gör du alternativknappar tillgängliga med knappbeskrivningar:
   1. Markera exkluderingsgruppen på paletten Hierarki.
   1. Klicka på paletten Tillgänglighet och skriv den text som ska läsas för gruppen i rutan Reader-text för anpassad skärm. Om du t.ex. har en exkluderingsgrupp som anger alternativ för betalning med olika kreditkort skriver du Välj en betalningsmetod.
   1. Om bildtexterna för varje alternativknapp innehåller text som är meningsfull när den talas av en skärmläsare, markerar du fliken Bindning på paletten Objekt och avmarkerar kryssrutan Ange objektvärde.

  Så här gör du alternativknappar tillgängliga med ett visst objektvärde:
   1. Markera exkluderingsgruppen på paletten Hierarki.
   1. Klicka på paletten Tillgänglighet och skriv den text som ska läsas för gruppen i rutan Reader-text för anpassad skärm. Om du t.ex. har en exkluderingsgrupp som anger alternativ för betalning med olika kreditkort skriver du Välj en betalningsmetod.
   1. Markera den första alternativknappen i gruppen på paletten Hierarki.
   1. Klicka på fliken Fält på paletten Objekt. Dubbelklicka på objektet i objektområdet och skriv ett meningsfullt värde för den markerade alternativknappen. Du kan till exempel skriva Kontant för den första knappen i en grupp betalningsmetoder.
   1. Upprepa steg 3 och 4 för varje alternativknapp i exkluderingsgruppen.

### Anpassade etikettkontroller

Vi rekommenderar att du använder standardkomponenter i stället för anpassade komponenter, eftersom hjälpmedlet som standard innehåller rätt ledtrådar och information. Om anpassade kontroller används bör du dock tänka på följande:
* Vi presenterar status för kryssrutor och alternativknappar.
* I listrutor och nedrullningsbara listor kan du meddela det standardobjekt som är markerat i listan. Se till att användaren vet hur man använder upp- och nedpilarna för att flytta mellan listobjekten. Observera att om du trycker på tabbtangenten eller Retur-tangenten så markeras objektet i listan. Med hjälp av skript kan du ställa in objektets Change-händelse för att meddela vilket objekt som är markerat i listan.
* Meddela användarna alla specialtangenttryckningar de behöver för att utföra en funktion, till exempel genom att trycka på blankstegstangenten för att välja en knapp eller på nedpilstangenten för att välja ett objekt i en listruta.

### Rätt placering av en kontrolls bildtext

Placeringen av en bildtext är viktig eftersom användarna förväntar att de ska finnas intill kontrollen. För användare med skärmförstoring är detta ännu viktigare eftersom de kanske inte kan visa både kontrollen och bildtexten samtidigt om de är för långt ifrån varandra.

När du skapar ett objekt placerar LiveCycle Designer automatiskt bildtexten enligt objektstypen. Bildtexterna för alternativknappar placeras till exempel till höger. Den här standardplaceringen är alltid den bästa platsen för en tillgänglig bildtext. Om du måste ändra bildtextens position gör du så här:
1. Markera objektet genom att flytta fokus till det.
1. På paletten Layout väljer du positionen för objektets bildtext under Placering i bildtextavsnittet, längst ned på paletten.

I exemplet i bild 5 visas en textruta med en bildtext ovanför. Placeringen på paletten Layout är inställd på Överkant. Bildtextens standardplats är till vänster om textrutan.

![Ändra bildtextens placering med paletten Layout](/help/forms/using/assets/image-5.png)

Bild 5: **Ändra bildtextens placering med paletten Layout**

I följande tabell visas en översikt över placeringsregler för etiketter för vanliga kontroller.

| Kontrolltyp | Placeringsregler |
|--------------|-----------------|
| Textindata (inklusive datum-, tid- och lösenordsfält) | Placera bildtexten till vänster om kontrollen (standard). Om det inte är möjligt kan du placera den omedelbart ovanför eller under den. Etiketter bör placeras nära kontrollen för användare med ökad förstoring så att etiketten och kontrollen med större sannolikhet visas tillsammans i den förstorade vyn. |
| Kryssruta | Placera bildtexten till höger om kryssrutan (standard). För kryssrutekontroller i tabellceller kommer skärmläsaren att meddela vilken bildtext, funktionsbeskrivning eller anpassad skärmläsartext du har angett för objektet. Om du vill använda kolumnrubriken som alternativ text för en kryssruta i en tabell ska du inte ange någon beskrivning, funktionsbeskrivning eller anpassad uppläsningstext. |
| Grupp med alternativknappar | Skapa en synlig rubrik för alternativknappsgruppen genom att skapa ett statiskt textelement och placera det till vänster om eller ovanför gruppen. För varje enskild alternativknapp placerar du etiketten till höger (standard). |
| Listruta | Placera bildtexten till vänster om objektet (standard). Om det inte är möjligt placerar du den omedelbart ovanför den. För nedrullningsbara listkontroller i tabellceller kommer skärmläsaren att meddela vilken bildtext, funktionsbeskrivning eller anpassad skärmläsartext du har angett för objektet. Om du vill använda kolumnrubriken som alternativ text för de här objekten i en tabell ska du inte ange någon beskrivning, funktionsbeskrivning eller anpassad uppläsningstext. |
| Listruta | Bildtexten placeras som standard ovanför listrutan när du skapar den. |
| Knapp | Bildtexten placeras automatiskt på knappen och behöver inte placeras manuellt. Kontrollera att knappens syfte beskrivs korrekt av bildtexten. |


### Fylla i en funktionsbeskrivning eller Reader-text för anpassad skärm dynamiskt

Du kan också dynamiskt fylla i en formulärkontrolls textalternativ, till exempel verktygstipset, med ett värde från en datakälla. Du kan till exempel visa en anpassad verktygstips för ett objekt på franska.
Schemat som du ansluter till kan ha följande definierat för en verktygstips:


```html
<form>
<tooltip dp_tt="tooltip1"/>
</form>
```


Datafilen som du pekar på kan ha följande definierat för ett verktygstips:

```html
<form>
<tooltip dp_tt="Quantité - Entrez un nombre inférieur ou égal à 100."/>
</form>
```

1. Klicka på kategorin Standard på paletten Objektbibliotek och dra ett objekt till formulärdesignen. Infoga till exempel ett textfältobjekt.
1. (Valfritt) Klicka på fliken Fält på paletten Objekt och skriv en bildtext för objektet i rutan Bildtext. Skriv t.ex. Kvantité.
1. Klicka på den aktiva etiketten Verktygstips på paletten Tillgänglighet.
1. Markera dataanslutningen.
1. Klicka på triangeln bredvid rutan Bindning och välj en bindning. Välj till exempel verktygstips > @dp_tt.

Följande sträng visas i rutan Bindning: $record.tooltip.dp_tt Tip: Du kan skriva strängen i rutan Objekt i stället för att markera den.
1. Klicka på OK.
1. Visa formuläret på fliken Förhandsgranska PDF.

### Ange länktext

Användare av hjälpmedelsteknik kan ha olika metoder för att läsa länkad text. Användare av skärmläsare använder till exempel ofta en länklista som den som visas i bild 6 för att snabbt skanna de tillgängliga länkarna på en sida.

![Dialogrutan JAWS-länklista](/help/forms/using/assets/image-6.png)

Bild 6: **Dialogrutan JAWS-länklista**

Därför måste länkar vara självbeskrivande. Det är deras betydelse inte beroende på sammanhanget (den omgivande texten). Orden&quot;klicka här&quot; kan till exempel utgöra det faktiska länkelementet i frasen&quot;klicka här för att ladda ned ansökningsformuläret&quot;. En sådan länk skulle vara svår att förstå när den läses igenom en länklista, särskilt när det finns flera länkar som innehåller samma text.

När du använder länkar i formuläret måste du se till att varje länk beskriver dess syfte, utan att vara beroende av den omgivande texten eller placeringen på sidan. I stället för att använda en fras som&quot;Klicka här&quot; som länktext använder du&quot;Hämta ansökningsformulär&quot; som länktext.

**Relaterade kontrollpunkter**

* Section 508 §1194.21
   * d) Tillräcklig information om ett element i användargränssnittet, inklusive elementets identitet, funktion och tillstånd, skall finnas tillgänglig för hjälpmedelsteknik. När en bild representerar ett programelement måste den information som bilden förmedlar också vara tillgänglig i text.
   * l) När elektroniska formulär används skall formuläret ge personer som använder hjälpmedel möjlighet att få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in formuläret, inklusive alla anvisningar och tips.
* Section 508 §1194.22
   * n) När elektroniska blanketter är avsedda att fyllas i online skall de personer som använder hjälpmedel få tillgång till den information, de fältelement och den funktionalitet som krävs för att fylla i och skicka in blanketten, inklusive alla anvisningar och anvisningar.
* WCAG 1.0
   * 12.4 Associera etiketter explicit med deras kontroller (P2).
   * 13.1 Identifiera tydligt målet för varje länk (P2).
* WCAG 2.0
   * 1.1.1 Innehåll som inte är text: Allt innehåll som inte är text och som presenteras för användaren har ett textalternativ som har samma syfte, förutom de situationer som anges nedan. (nivå A)
   * 2.4.6 Rubriker och etiketter: Rubriker och etiketter beskriver ämne eller syfte. (nivå AA)
   * 3.2.4 Konsekvent identifiering: Komponenter som har samma funktioner på en uppsättning webbsidor identifieras på ett enhetligt sätt. (nivå AA)
   * 3.3.2 Etiketter eller instruktioner: Etiketter eller instruktioner tillhandahålls när innehållet kräver användarindata. (nivå A)
   * 4.1.2 Namn, Roll, Värde: För alla komponenter i användargränssnittet (inklusive men inte begränsat till: formulärelement, länkar och komponenter som genereras av skript) kan namnet och rollen bestämmas programmatiskt, tillstånd, egenskaper och värden som kan anges av användaren kan anges programmatiskt och meddelanden om ändringar av dessa objekt är tillgängliga för användaragenter, inklusive hjälpmedelstekniker. (nivå A)


## Kontrollera att läs- och tabbordningen är korrekt {#ensure-reading-tab-order}

Att säkerställa en meningsfull läsordning är mycket viktigt när man utformar formulär som är tillgängliga för användare med synnedsättning eller andra funktionshinder. Dessa användare använder vanligtvis inte musen för att navigera i ett formulär, så de är beroende av tangentbordet. Läsordningen bestämmer den sekvens som används av skärmläsaranvändare när de läser igenom formuläret. Dessutom kan man med tabbordningen snabbt gå från en interaktiv formulärkontroll till nästa med hjälp av tabb- eller skift+tabbtangenterna. En logisk tabbordning säkerställer att de har tillgång till alla fält i formuläret och att de kan navigera i formuläret på ett vettigt och effektivt sätt.

Formulärets läsordning innehåller alla statiska objekt (till exempel text och bilder) och fältobjekt, men bara de interaktiva formulärkontrollerna är en del av tabbordningen.

>[!NOTE]
> I många fall är tabbordningen nära relaterad till läsordningen. För enkelhetens skull används termen&quot;tabbordning&quot; i stället för&quot;tabbordning&quot; i den här handboken.

### Standardtabbordningen i LiveCycle Designer-formulär

Standardtabbordningen skapas automatiskt när du sparar formuläret som en taggad PDF. Till att börja med bestäms tabbordningen i ett formulär utifrån objektens lokala position med hjälp av följande regler:

* Alla objekt ordnas från vänster till höger och uppifrån och ned (lokal ordning), med början i formulärets övre vänstra hörn.
* Alla delformulär som du skapar behandlas som självständiga enheter och navigeras också från vänster till höger och uppifrån och ned. Om två delformulär placeras bredvid varandra, som båda innehåller objekt, navigerar läsordningen mellan alla objekt i det första delformuläret innan du går till nästa delformulär.

För enkla formulär (det vill säga formulär med vänster-till-höger-layout uppifrån och ned) är standardtabbordningen vanligtvis korrekt. Du bör kontrollera standardtabbordningen innan du publicerar formuläret. Du kan göra tabbordningen synlig med någon av följande metoder:

* Välj Visa > Visa tabbordning.
* Klicka på Visa ordning på paletten Tabbordning.

Alla objekt visas med en siffra i det övre högra hörnet som anger objektets plats i standardtabbordningen. De interaktiva objekten i den här sekvensen utgör tabbordningen. Bild 7 visar visualiseringen av läsordningen i ett grundläggande formulär.

![Visualisering av standardläsordningen för ett typiskt orderformulär](/help/forms/using/assets/image-7.png)

Bild 7: **Visualisering av standardläsordningen för ett typiskt orderformulär**

Varje tabbordningsnummer visas i en färgad form. Formerna har följande betydelse:
* Grå cirklar (#1 och #4) används för objekt i innehållsområdet.
* Gröna cirklar (#6 och #7) används för mallsidesobjekt.
* Lavendelfyrkanter (#2 och #3) används för objekt inuti ett fragment.

Du kan välja att bara visa interaktiva formulärkontroller (som utgör tabbordningen) eller alla objekt i läsordningen (som även innehåller statiska objekt som text och bilder). Om du vill ändra den här inställningen väljer du Verktyg > Alternativ > Tabbordning och markerar Visa endast tabbordning för fält.

I komplexa formulär kan det vara svårt att se hur tabbordningen flödar från ett objekt till nästa. Du kan använda visuella hjälpmedel för att se tabbflödet i formuläret. När du håller pekaren över objektet när du har aktiverat visuella hjälpmedel visar blå pilar tabbflödet för de två föregående och två efterföljande objekten i tabbordningen (se figur 8).

![Visuella hjälpmedel markerar tabbordningen](/help/forms/using/assets/image-8.png)

Bild 8: **Visuella hjälpmedel markerar tabbordningen**

Använd följande metoder för att aktivera visuella hjälpmedel:
* Välj Verktyg > Alternativ > Tabbordning och välj Visa ytterligare visuella hjälpmedel för tabbordning på panelen Tabbordning.
* Välj Visa visuella hjälpmedel på palettmenyn Tabbordning.

### Använda position för att påverka standardtabbordningen

Om du vill påverka standardtabbordningen kan du ändra ett objekts koordinater genom att flytta det till en annan plats. I bild 9 visas till exempel fältet Produktnamn i tabbordningen före fältet Kvantitet. Om du vill ändra den här ordningen kan du flytta fältet Produktnamn så att det placeras under eller till höger om fältet Kvantitet.

![Standardtabbordningen är vänster till höger](/help/forms/using/assets/image-9.png)

Bild 9: **Standardtabbordningen är vänster till höger**

Du kan ändra ett objekts position genom att göra något av följande:
* Dra den med musen
* Markera den och flytta den med piltangenterna på tangentbordet.

>[!NOTE]
> Det kan vara praktiskt att behålla objektjusteringen genom att välja Visa > Fäst mot rutnät.

Du kan ändra ett objekts koordinater mer exakt med paletten Layout (visas i bild 10). På den här paletten kan du ange X- och Y-koordinater samt objektets bredd och höjd.

![Använda koordinater för att exakt positionera ett objekt med layoutpaletten](/help/forms/using/assets/image-10.png)

Bild 10: **Använda koordinater för att exakt positionera ett objekt med layoutpaletten**

>[!NOTE]
> När bildtexten och kontrollen inte sammanfogas är positionen för bildtexten för en formulärkontroll oberoende av den ordning i vilken skärmläsare läser objektet och dess element. Mer information om bildtexter finns i avsnitt 2.5 Ange korrekta etiketter för formulärkontroller i den här handboken.

### Använda delformulär för att påverka standardtabbordningen

Som nämnts ovan kan du i delformulär infoga grupper med objekt som har sin egen tabbordning. Du kan skapa ett delformulär på något av följande sätt:
* Välj Infoga > Standard > Delformulär.
* Markera objekten på paletten Hierarki och gruppera dem i ett delformulär genom att välja Infoga > Lägg i delformulär.
* Markera objekten i det faktiska formuläret, högerklicka på markeringen och välj Lägg i delformulär

När två delformulär som innehåller fältobjekt placeras sida vid sida, går tabbordningen igenom fälten i det första delformuläret innan du går vidare till nästa. Detta illustreras i bild 11, där två delformulär används för att skapa en kolumnbaserad standardtabbordning.

![Standardtabbordning med delformulär](/help/forms/using/assets/image-11.png)

Figur 11: **Standardtabbordning med delformulär**

Delformulär, alternativknappar och innehållsområden, tillsammans med den lodräta placeringen av objekt på en sida och dess mallsida, påverkar alla tabbordningen.

### Skapa en anpassad tabbordning med paletten Tabbordning

Du kan ändra standardtabbordningen när du behöver en annan sekvens i formuläret och ändringen inte kan göras med placering eller gruppering i delformulär. Om du vill ändra standardtabbordningen kan du skapa en anpassad tabbordning med paletten Tabbordning.
Med paletten Tabbordning (se figur 12) kan du inspektera och ändra den ordning i vilken objekt i formuläret läses av hjälpfunktioner och navigeras med användarens tabbtangent.

![Paletten Tabbordning](/help/forms/using/assets/image-12.png)

Bild 12: **Paletten Tabbordning**

På paletten Tabbordning finns en alternativ vy över formulärets tabbordning. Den visar alla objekt i formuläret som numrerade listor, där varje nummer representerar objektets position i tabbflödet.
Öppna paletten Tabbordning genom att välja Fönster > Tabbordning.


På paletten Tabbordning finns följande synliga markörer:
* Varje sida i formuläret markeras med ett grått streck. Tabbordningen på varje sida börjar med 1.
* Bokstaven M i en grön cirkel anger mallsidesobjekt (visas bara när formuläret visas på fliken Designvy).
* Ett nummerintervall indikerar objekt inom ett fragment.
* En gul bakgrund indikerar det markerade objektet.
* En låsikon bredvid det första objektet på sidan anger att objektet inte kan flyttas inom tabbordningen (visas bara när formuläret visas på fliken Mallsidor).

I listan visas samma tabbordningsnummer som de nummer som visas i själva formuläret när du väljer Visa > Visa tabbordning. Du ändrar ett objekts position i tabbordningen genom att flytta objektet uppåt eller nedåt i tabbordningslistan. Du kan flytta ett enskilt objekt eller en grupp med objekt. Detta kan uppnås på något av följande sätt:

* Dra det markerade objektet uppåt eller nedåt i listan och placera det på önskad plats. Ett svart handtag markerar den aktuella positionen i listan innan du monterar objektet.
* Klicka på upp- eller nedpilarna på paletten Tabbordning tills det markerade objektet har placerats på rätt plats. Du kan också trycka på Ctrl+uppil eller Ctrl+nedpil.
* Välj Flytta upp eller Flytta ned på palettmenyn Tabbordning.
* Klicka på det markerade objektet (eller markera det och tryck på F2) i listan på paletten Tabbordning för att göra numret som visas bredvid objektnamnet redigerbart. Skriv sedan numret som anger objektets nya position i tabbordningen och tryck på Retur.
* Välj Kopiera på palettmenyn Tabbordning och markera det objekt ovanför vilket du vill placera det objekt som du flyttar. Välj sedan Klistra in på menyn.

När du flyttar objektet till en ny plats i ordningen, omtilldelas tabbordningsnumren i LiveCycle Designer. Även om tabbordningen för de objekt som finns på en mallsida visas på fliken Designvy, kan du bara ändra ordningen för dessa objekt på fliken Mallsidor. Om du använder fragmentreferenser i formuläret visas tabbordningen i ett fragment när du visar formulärets ordning. Om du vill ändra tabbordningen i ett fragment måste du öppna fragmentkällfilen för redigering, göra ändringen och spara filen. Alla formulär som använder det här fragmentet påverkas av den här ändringen.

Om du bestämmer dig för att du inte vill ha den anpassade tabbordningen i formuläret kan du snabbt återgå till den automatiska (standardtabbordningen) genom att följa de här stegen (du förlorar alla ändringar som gjorts i tabbordningen):
1. Välj Automatisk på paletten Tabbordning.
1. I meddelanderutan som visas klickar du på Ja för att bekräfta borttagningen av den anpassade tabbordningen.

**Relaterade kontrollpunkter**
* Section 508 §1194.21
   * (a) När programvara är utformad för att köras på ett system som har ett tangentbord, ska produktfunktioner kunna köras från ett tangentbord där själva funktionen eller resultatet av att utföra en funktion kan urskiljas i text.
* WCAG 1.0
   * 9.2 Se till att alla element som har ett eget gränssnitt kan användas på ett enhetsoberoende sätt.
* WCAG 2.0
   * 1.3.2 Meningsfull sekvens: När den sekvens i vilken innehållet presenteras påverkar dess betydelse, kan en korrekt lässekvens bestämmas programmatiskt. (nivå A)
   * 2.1.1 Tangentbord: All funktionalitet i innehållet kan opereras via ett tangentbordsgränssnitt utan att särskilda tidsinställningar krävs för enskilda tangenttryckningar, utom när den underliggande funktionen kräver indata som är beroende av sökvägen för användarens rörelse och inte bara slutpunkterna. (nivå A)
   * 2.1.3 Tangentbord (inget undantag): All funktionalitet i innehållet kan utföras via ett tangentbordsgränssnitt utan att särskilda tidsinställningar krävs för enskilda tangenttryckningar. (nivå AAA)
   * 2.4.3 Fokusordning: Om en webbsida kan navigeras sekventiellt och navigeringssekvenserna påverkar innebörden eller funktionen får fokuserbara komponenter fokus i en ordning som bevarar innebörden och användbarheten. (nivå A)


## Se till att formulärkontrollerna är tangentbordstillgängliga{#ensure-keyboard-accessible}

Användarna måste kunna fylla i formuläret helt med bara tangentbordet eller en motsvarande alternativ indataenhet. Användare med nedsatt rörelseförmåga eller synnedsättning har kanske inget annat val än att använda tangentbordet, och många användare som kan använda en mus föredrar helt enkelt tangentbordsinmatning. Genom att tillåta olika inmatningsmetoder kan du inte bara skapa hjälpmedelsförberedda formulär, du kan också skapa formulär som är bättre anpassade efter alla användares önskemål.

I LiveCycle Designer är det enklaste sättet att se till att kontrollerna är tangentbordstillgängliga genom att använda kontrollerna som listas under fliken Allmänt på paletten Objektbibliotek. Dessa kontroller svarar på indata från både mus och tangentbord som standard. Mer information finns i avsnitt 2.3 Välja rätt kontroller i den här handboken.

En annan viktig aspekt av tangentbordstillgänglighet är att se till att alla interaktiva element är en del av formulärets tabbordning. Detta gör att användaren kan flytta markören framåt och bakåt i formuläret med tabb- och skift+tabbtangenterna. Var noga med att ange en logisk tabbordning som innehåller alla fält och knappar. Mer information finns i avsnitt 2.6 Kontrollera att läs- och tabbordningen är korrekt i den här handboken.

Slutligen är det viktigt att se till att skriptbeteendet även är tillgängligt via tangentbordet och inte är beroende av enhetsspecifika händelser. Mushändelsen MouseEnter kan till exempel inte köras med tangentbordet. Sådana händelsehanterare får inte heller påverka tangentbordstillgängligheten. Kontrollera till exempel att ändringshändelser som används i nedrullningsbara listor eller listrutor inte utlöser oväntade åtgärder.

**Relaterade kontrollpunkter**
* Section 508 §1194.21
   * (a) När programvara är utformad för att köras på ett system som har ett tangentbord, ska produktfunktioner kunna köras från ett tangentbord där själva funktionen eller resultatet av att utföra en funktion kan urskiljas i text.
* WCAG 1.0
   * 6.4 Kontrollera att händelsehanterare är enhetsoberoende (P2) för skript och applets.
   * 9.2 Se till att alla element som har ett eget gränssnitt kan användas på ett enhetsoberoende sätt (P2).
   * 9.3 För skript anger du logiska händelsehanterare i stället för enhetsberoende händelsehanterare (P2).
* WCAG 2.0
   * 2.1.1 Tangentbord: All funktionalitet i innehållet kan opereras via ett tangentbordsgränssnitt utan att särskilda tidsinställningar krävs för enskilda tangenttryckningar, utom när den underliggande funktionen kräver indata som är beroende av sökvägen för användarens rörelse och inte bara slutpunkterna. (nivå A)
   * 2.1.2 Ingen tangentbordssvällning: Om tangentbordsfokus kan flyttas till en komponent på sidan med ett tangentbordsgränssnitt, kan fokus flyttas bort från komponenten med bara ett tangentbordsgränssnitt. Om det kräver mer än oförändrade pil- eller tabbtangenter eller andra standardmetoder för avslutning informeras användaren om metoden för att flytta fokus bort. (nivå A)
   * 2.1.3 Tangentbord (inget undantag): All funktionalitet i innehållet kan utföras via ett tangentbordsgränssnitt utan att särskilda tidsinställningar krävs för enskilda tangenttryckningar. (nivå AAA)


## Använda färger på ett ansvarsfullt sätt{#use-color-responsibly}

När du utformar formulär för tillgänglighet måste du ta hänsyn till några andra riktlinjer för hur du använder färg. Designers använder färger för att förbättra formulärens utseende genom att markera olika komponenter i formuläret. Felaktig användning av färger kan dock göra informationen i din form svår eller omöjlig att läsa av personer med funktionshinder.

### Förmedla inte information med enbart färg

Färger kan framhäva och förbättra vissa delar av formuläret, men du bör inte förmedla information enbart i färg.

Information som endast förmedlas i färg (färger med semantisk innebörd) är inte tillgänglig för blinda användare. Detsamma gäller användare med nedsatt färgseende eller användare som använder olika färgscheman, till exempel en färgskärm med hög kontrast och vit text eller förgrund mot en svart bakgrund. Du måste också tänka på att skärmläsare inte kan identifiera färginformation automatiskt.

I bild 13 visas till exempel ett formulärfält som har en röd bildtext (som anges med paletten Teckensnitt) som anger att formulärfältet är obligatoriskt. I det här exemplet är färgen den enda beteckningen på skillnaden mellan obligatoriska och valfria inmatningsfält, vilket gör det omöjligt för blinda användare eller användare med vissa typer av färgblindhet att skilja på dem.

![Använda enbart färg för att förmedla information](/help/forms/using/assets/image-13.png)

Bild 13: **Använder enbart färg för att förmedla information**

För att lösa det här problemet anger du även formulärets obligatoriska status i formulärkontrollens alternativa text (som beskrivs i avsnitt 2.5 Ange korrekta etiketter för formulärkontroller). Du kan till exempel ställa in uppläsningstexten på&quot;Postnummer (obligatoriskt)&quot;. För användare som har svårt att se färg i vissa kombinationer rekommenderar vi att du ställer in textfältstypen till Anges av användaren - krävs på paletten Objekt, förutom alternativ text som anger att fältet är obligatoriskt. Du kan också använda andra indikationer än färg, till exempel visuell text, textformat och kantlinjeformat. För skärmläsaranvändare måste du dock ändå förmedla den information som krävs via hjälpmedelspaletten.

När du ger formuläranvändaren beskrivningar eller instruktioner bör du tänka på att programsatser som enbart baseras på färg inte räcker för användare med nedsatt syn. I stället för en programsats som&quot;Klicka på den gröna knappen för att fortsätta&quot; använder du en textbeskrivning för åtgärder, som&quot;Klicka på knappen Nästa för att fortsätta&quot;.

>[!NOTE]
> Detta är en bra metod som inte förbjuder användning av färg. Det är förbjudet att använda färg som enda sätt att förmedla viktig information. Om en visuell indikation fortfarande önskas för den här typen av information kan designern använda en asterisk eller liknande visuell indikator för att markera obligatoriska fält.

### Ange tillräcklig färgkontrast

Många användare med synnedsättning förlitar sig på hög kontrast mellan text och bakgrunden för att läsa formulär. När kontrasten mellan bakgrunds- och förgrundsfärger inte är tillräcklig kan ett formulär bli svårt eller till och med omöjligt att läsa för vissa användare. Bild 14 visar ett exempel på ett formulär med otillräcklig kontrast.

![Ett formulär med otillräcklig färgkontrast](/help/forms/using/assets/image-14.png)

Bild 14: **Ett formulär med otillräcklig färgkontrast**

Vi rekommenderar starkt att du använder standardteckensnitt och bakgrundsfärger: svart på vit bakgrund. Om du måste ändra dessa standardfärger måste du välja en lämplig kombination av färger med hög kontrast. Använd antingen en mörk förgrundsfärg på en ljus bakgrundsfärg eller vice versa. Du kan vara säker på att du använder ett verktyg (till exempel WAT-C Color Contrast Analyzer) för att kontrollera att kontrasten är tillräcklig.

Med Adobe Reader och Adobe Acrobat kan man ange om färgerna ska ersättas för att passa de visuella behoven. Användarna kan ange ett eget kontrastschema eller välja att använda ett schema som tillhandahålls av operativsystemet. Dessutom har Adobe Reader och Adobe Acrobat ett eget kontrastschema som kan vara aktiverat. För att de här alternativen ska lyckas bör du alltid använda standardfärger.

När du utformar formuläret bör du testa det ofta med en färgschemainställning som liknar den som många användare med synnedsättning använder för att fylla i formuläret. Med den här metoden kan du upptäcka och korrigera problem tidigt i designprocessen.

Rekommendationer för färganvändning:
* Se till att ingen information går förlorad om den semantiska färgen inte syns.
* Om du inte kan använda standardfärger bör du kontrollera att färgerna har hög kontrast, till exempel svart på en ljus (vit) bakgrund. Delvis betraktade användare kräver i allmänhet en hög kontrast mellan texten och dess bakgrund för att kunna läsa den.
* Testa hur läsbara formulären är genom att växla skärmen till en högkontrastskärm, både i Windows och Adobe Reader eller Adobe Acrobat. Mac OSX har bara ett enkelt gråskalefilter för hög kontrast vilket innebär att detta inte räcker för testning.
* Förmedla inte information enbart baserat på färg. Använd till exempel inte bara färg för att markera viktiga textdelar. Använd även andra markeringsmetoder och textbeskrivningar.
* Använd inte för många färger eftersom det kan göra den faktiska informationen i innehållet svår att läsa. Ha alltid informationens läsbarhet som högsta prioritet när du bestämmer vilka färger som ska användas.

**Relaterade kontrollpunkter**
* Section 508 §1194.21
   * i) Färgkodning får inte användas som det enda sättet att förmedla information, ange en åtgärd, föreslå ett svar eller särskilja ett visuellt element.
* WCAG 1.0
   * 2.1 Kontrollera att all information som förmedlas med färg också är tillgänglig utan färg, till exempel från sammanhang eller markering.
   * 2.2 Se till att kombinationer av förgrunds- och bakgrundsfärger ger tillräcklig kontrast när de visas av någon som har färgbrist eller när de visas på en svart och vit skärm. [Prioritet 2 för bilder, Prioritet 3 för text ] (P2).
* WCAG 2.0
   * 1.4.1 Användning av Färg: Färg används inte som det enda visuella sättet att förmedla information, indikera en åtgärd, fråga ett svar eller särskilja ett visuellt element. (nivå A)
   * 1.4.3 Kontrast (minimal): Den visuella presentationen av text och bilder av text har ett kontrastförhållande på minst 4,5:1, utom följande: (Nivå AA)
   * 1.4.6 Kontrast (Förbättrat): Den visuella presentationen av text och bilder av text har ett kontrastförhållande på minst 7:1, utom följande: (Nivå AAA)


## Ange rubrikceller för tabeller{#provide-heading-cells}

Tabeller är ett effektivt sätt att ordna och presentera innehåll i hjälpmedelsförberedda formulär. När den används på rätt sätt ger tabellens rader och kolumner en förutsägbar och konsekvent struktur för formulärinnehållet. När en skärmläsaranvändare till exempel navigerar till en innehållsradcell anger skärmläsaren cellens placering och läser sedan cellinnehållet. Skärmläsaren anger cellens placering med en kombination av rad- och kolumnrubriker eller rad- och kolumnnummer. Eftersom skärmläsare anger information som orienterar användaren till platsen för innehållet i tabellen, påverkar layouten direkt tabellens hjälpmedel.

Du kan ange följande roller för tabellelement när du skapar tabeller. Med de här rollerna kan skärmläsare navigera i tabellstrukturen med särskilda kortkommandon och förmedla relationen mellan tabellceller och motsvarande rubrikceller till användaren.
* Tabell
Tilldelar rollen för en tabell till det markerade delformuläret. När användaren navigerar till det här delformuläret identifierar de flesta skärmläsare det som en tabell och anger antalet rader och kolumner.
* Rubrikrad
Ger det markerade delformuläret eller tabellraden rollen av en rubrikrad. När innehållet i en innehållsradcell skrivs identifierar de flesta skärmläsare först innehållet i motsvarande cell i rubrikraden.
* Brödtextrad
Ger det markerade delformuläret eller tabellraden rollen av en innehållsrad. Om en cell innehåller ett delformulär läser skärmläsare vanligtvis upp innehållet i motsvarande cell i rubrikraden, följt av fälten i delformuläret.
* Sidfotsrad
Ger det markerade delformuläret eller tabellraden rollen som sidfotsrad.
* (Ingen)
Anger en rad som förmedlar information om tabellen eller dess innehåll. Raden anses inte vara en del av tabellen, men skärmläsaren läser upp innehållet.

När tabeller används på rätt sätt är de ett effektivt sätt att ordna och presentera tabellinformation. Undvik alltför komplicerade tabeller, t.ex. tabeller med kapslade tabeller och avsnitt.

### Göra enkla tabeller tillgängliga

Tabeller med enkel layout rekommenderas. Enkla tabeller börjar med en rubrikrad följt av innehållsraderna.

Tänk på följande när du utformar enkla tabeller för tillgänglighet:

* Tabbordningen för en tabell är geografisk ordning, vilket är samma som för själva formuläret. Se till att tabellinnehållet är organiserat så att det blir bra när det läses från vänster till höger och uppifrån och ned.
* De flesta skärmläsare tolkar den första raden i en tabell som rubrikrad. När du läser innehållet i en innehållsradcell läser dessa skärmläsare först innehållet i den tillhörande rubrikradcellen. Se till att innehållet i varje rubrikradcell på ett meningsfullt sätt beskriver kolumninnehållet.
* Undvik celler som spänner över två eller flera kolumner, kapslade tabeller eller tabellavsnitt. Vissa skärmläsare har svårt att tolka dessa funktioner korrekt eller kanske inte använder dem. Om en cell i en innehållsrad till exempel sträcker sig över två kolumner, kanske skärmläsare inte refererar till rätt cellinnehåll i rubrikraden när nästa cell läses in i raden.

### Göra komplexa tabeller tillgängliga

När du utformar tabeller för hjälpmedel bör du sträva efter att göra tabellayouten enkel, med en rubrikrad följt av innehållsrader. Visst kan en del innehåll kräva en mer komplex tabellayout. Du kan till exempel behöva använda cellspridning eller mer än en rubrik för att förmedla innehållet på ett effektivt sätt.

Du kan skapa komplexa tabeller genom att använda tabellobjektet eller genom att kombinera delformulärsobjekt. Med tabellobjektet kan du använda funktioner som är avsedda att underlätta designprocessen, till exempel alternativ för att infoga och ändra storlek på kolumner och rader.

På paletten Tillgänglighet kan du ange tabellrelaterade roller till delformulär för att skapa en hjälpmedelsanpassad, komplex tabell. Beroende på din designerfarenhet och dina inställningar kan du välja att skapa komplexa tabeller genom att kombinera delformulärsobjekt. Du kan t.ex. skapa ett delformulär som innehåller två rader och ange det här delformuläret som tabellrubrik och ange ett annat delformulär för tabellinnehållsraderna.

När du använder delformulärsobjekt i stället för tabellobjekt för att skapa tabeller krävs följande steg:
* På fliken Delformulär ställer du in typen för varje delformulär till Placerad.
* Ange lämplig delformulärsroll för varje delformulär som tabellen består av på paletten Tillgänglighet. Du kan till exempel tilldela rollen Rubrikrad till det delformulär som används som tabellrubrik.
* Tilldela delformulärsrollen Ingen för rader som förmedlar information om tabellen eller dess innehåll men som inte anses ingå i tabellen. Skärmläsaren läser radinnehållet.

De funktioner som stöds av skärmläsaren avgör vilken information som läses in för en komplex tabell. Ta till exempel en tabell som innehåller en rubrikrad och ett avsnitt med en rubrikrad. När användaren navigerar till en innehållsradcell i tabellavsnittet, läser skärmläsare vanligtvis följande innehåll i ordning:
* Innehåll från lämplig cell i tabellens rubrikrad
* Innehåll från lämplig cell i avsnittets rubrikrad
* Innehåll från den markerade cellen
Vissa skärmläsare kanske inte läser innehåll från båda sidhuvudsraderna.

Skapa meningsfulla synliga namn eller titlar för tabellerna. Du kan skapa ett tabellnamn som statisk text i Adobe LiveCycle Designer och placera det framför tabellen. Du kan gruppera en tabell och dess namn i ett delformulär. Delformulär är särskilt användbara när du vill kombinera kopplade objekt i en layout.

För kontroller i tabellceller kommer skärmläsaren att meddela vilken bildtext, funktionsbeskrivning eller anpassad uppläsningstext du har angett för objektet. Om du vill använda kolumnrubriken som alternativ text för en kontroll i en tabell ska du inte ange någon beskrivning, funktionsbeskrivning eller anpassad uppläsningstext. Tänk dock på att den här strategin inte alltid är lika tydlig för skärmläsaranvändare eftersom skärmläsare bara kan associera kolumnrubriken med kontrollen när användaren inte är i skärmläsarens interaktionsläge.

**Relaterade kontrollpunkter**
* Section 508 §1194.22
   * g) Rad- och kolumnrubriker skall identifieras för datatabeller.
   * h) Markering skall användas för att koppla dataceller och rubrikceller till datatabeller som har två eller flera logiska nivåer av rad- eller kolumnrubriker.
* WCAG 1.0
   * 5.1 Identifiera rad- och kolumnrubriker (P1) för datatabeller.
   * 5.2 För datatabeller som har två eller flera logiska nivåer med rad- eller kolumnrubriker använder du kod för att koppla dataceller och rubrikceller (P1)
* WCAG 2.0
   * 1.3.1 Information och relationer: Information, struktur och relationer som förmedlas genom presentation kan bestämmas programmatiskt eller vara tillgängliga i text. (nivå A)


## Ange en navigeringsbar formulärstruktur{#provide-navigable-form}

När ett formulär blir långt och komplicerat kommer det att bli mycket lättare att använda det på ett sätt som är strukturerat. På samma sätt som en bok blir lättare att förstå när den är uppdelad i kapitel och avsnitt, blir ett formulär lättare att använda när det är uppdelat i rubriker och underrubriker. Den här partitioneringen är särskilt användbar för skärmläsaranvändare av följande skäl:
* Varje rubrik anger för skärmläsaranvändaren vad som kan förväntas i avsnittet efter rubriken.
* Skärmläsare har kortkommandon som du kan använda för att snabbt gå fram och tillbaka mellan de olika rubrikerna i formuläret. Dessutom kan användaren komma åt en lista med rubriker, som ger en översikt över dokumentstrukturen och möjliggör snabb navigering.

Formuläret blir enklare om användaren kan hoppa till andra delar av formuläret. Du kan lägga till en rubrikstruktur i formuläret med hjälpmedelspaletten i LiveCycle Designer.

### Tillhandahåll mekanismer för att hoppa över

Kända användare kan skanna en sida i vilken ordning som helst. De kan börja med att titta i det nedre högra hörnet på sidan och skanna bakåt genom innehållet. Skärmläsaranvändaren har inte det här alternativet eftersom skärmläsaren kommer att börja läsa sidan i det övre vänstra hörnet (vilket anges i källkoden) och gå igenom i linjär ordning. Dessutom kan den synkade användaren skanna sidan och leta efter intressanta länkar och aktivera dem med musen. En skärmläsaranvändare måste gå igenom sidan sekventiellt.

Det enklaste och effektivaste sättet att skapa en navigeringsbar formulärstruktur är att använda strukturrubriker och korrekt definierade listor i formuläret.
Du kan också ange mekanismer som gör att användaren kan hoppa till andra delar av formuläret, till exempel genom att lägga till navigeringsknappar högst upp och längst ned i formuläret. Överst i ett formulär kan du inkludera knappar som Öppna datafil, Föregående sida och Nästa sida. Längst ned i formuläret kan du inkludera knappar som Spara data, E-postdata, Gå överst på sidan och Skriv ut.

Smarta fält kan vara ett effektivt sätt att göra vissa formulär enklare att fylla i. Ett resebegärandeformulär kan t.ex. innehålla flera rader och kolumner med fält. Om en viss rad är tom kan du hoppa till nästa avsnitt i formuläret genom att trycka på tabbtangenten på det sista objektet i raden, i stället för att gå igenom ett antal fält som ska vara tomma.

### Lägga till rubriker med paletten Tillgänglighet

Du kan använda paletten Tillgänglighet för att tilldela roller till objekt baserat på vad objektet används för. Dessa roller kan användas för att skapa rubriker på olika nivåer.

![Ange en rubrikroll på paletten Tillgänglighet](/help/forms/using/assets/image-15.png)
Bild 15: **Ange en rubrikroll på paletten Tillgänglighet**

Så här skapar du en rubrik i formuläret:

1. Identifiera början av varje logiskt segment i formuläret med statiska textetiketter,
1. För varje etikett och välj ett av rubrikalternativen som roll på paletten Tillgänglighet. Med de olika rubriknivåerna (1 till 6) kan du skapa en rubrikstruktur i formuläret. Börja med nivå 1 och använd sedan nivå 2 och så vidare för kapslade underavsnitt.

De flesta skärmläsare tillåter användare att snabbt navigera mellan rubrikelement baserat på deras nivå. I bild 16 visas ett formulär som är uppdelat i mindre segment med hjälp av rubriker. I det här exemplet används följande rubrikstruktur:

* Rubriknivå 1: Produktförfrågan
   * Rubriknivå 2: Orderinformation
      * Rubriknivå 3: Leveransalternativ
* Rubriknivå 2: Ytterligare information
   * Rubriknivå 3: Personuppgifter
   * Rubriknivå 3: Adress

![Strukturera ett formulär med rubriker](/help/forms/using/assets/image-16.png)

Figur 16: **Strukturera ett formulär med rubriker**

Dessa rubriker är bara statiska textelement som har fått en viss teckenstorlek och en rubrikroll med lämplig nivå.

>[!NOTE]
> Om du ändrar en textetiketts visuella utseende så att den ser ut som en rubrik kommer skärmläsare inte att känna igen den som en rubrik. Du måste använda en rubrikroll.

Kontrollera alltid att rubriknivåernas ordning är logisk. Ett underavsnitt av en rubrik på nivå 2 måste till exempel alltid vara en rubrik på nivå 3. Du ska aldrig hoppa över nivåer när du markerar underavsnitt. Användare av skärmläsare använder de olika nivåerna för att få en bättre förståelse för formulärets struktur. När du har påträffat en rubrik på nivå 2 kan användaren använda ett kortkommando för att leta efter rubriker på nivå 3 och avgöra om det finns några underavsnitt. Om du hoppar över nivåer kommer användaren att ha svårt att identifiera dessa underavsnitt.

### Markera listor

Ibland kan det också vara användbart att lägga till listinnehåll i formuläret. Listor är användbara för att gruppera relaterade objekt, och de gör det möjligt för skärmläsaranvändare att veta hur många objekt som finns i en lista och snabbt navigera förbi den. Om du markerar listor på rätt sätt blir formulärets struktur tydligare för skärmläsaranvändare.

I LiveCycle Designer skapar du listor med delformulär enligt följande:

1. Välj ett delformulär som innehåller innehållet som ska markeras som listobjekt.
1. Välj Lista som roll på paletten Tillgänglighet.
1. Markera varje kapslat delformulär i delformuläret Lista och ange dess roll som Listobjekt.

>[!NOTE]
> En listobjektroll kan bara tilldelas till ett delformulär som finns i ett delformulär som har en angiven listroll. Du kan inte definiera en tabell eller tabellrad som en list- eller listpost, men ett listobjekt kan innehålla en tabell.

**Relaterade kontrollpunkter**
* Section 508 §11934.22
   * o) En metod skall finnas som gör det möjligt för användarna att hoppa över repetitiva navigeringslänkar.
* WCAG 1.0
   * 3.5 Använd rubrikelement för att förmedla dokumentstrukturen och använda dem enligt specifikationen (P2).
   * 3.6 Markera listor och listobjekt korrekt. (P2).
   * 12.3 Dela upp stora informationsblock i mer hanterbara grupper där det är naturligt och lämpligt. (P2).
   * 13.3 Ange information om webbplatsens allmänna layout (t.ex. en webbplatskarta eller innehållsförteckning).
   * 13.4 Använda navigeringsmekanismer på ett konsekvent sätt (P2).
* WCAG 2.0
   * 1.3.2 Meningsfull sekvens: När den sekvens i vilken innehållet presenteras påverkar dess betydelse, kan en korrekt lässekvens bestämmas programmatiskt. (nivå A)
   * 2.4.1 Kringgå block: Det finns en mekanism för att kringgå innehållsblock som upprepas på flera webbsidor. (nivå A)
   * 2.4.5 Flera sätt: Det finns mer än ett sätt att hitta en webbsida i en uppsättning webbsidor, förutom där webbsidan är ett resultat av eller ett steg i en process. (nivå AA)
   * 2.4.6 Rubriker och etiketter: Rubriker och etiketter beskriver ämne eller syfte. (nivå AA)
   * 2.4.10 Avsnittsrubriker: Avsnittsrubriker används för att ordna innehållet. (nivå AAA)
   * 3.2.3 Konsekvent navigering: Navigeringsfunktioner som upprepas på flera webbsidor i en uppsättning webbsidor sker i samma relativa ordning varje gång de upprepas, såvida inte en ändring initieras av användaren. (nivå AA)


## Undvik störande skript{#avoid-disruptive-scripting}

Som en del av formulärdesignprocessen kan formulärutvecklare använda skript för att ge en bättre användarupplevelse. Du kan lägga till skript i de flesta formulärfält och objekt. Du kan till exempel skapa enkla skript som dynamiskt uppdaterar värden i ett interaktivt formulär som svar på användarens indata.

Tänk på följande allmänna riktlinjer när du utformar hjälpmedelsskript:

* Se till att formulärinnehållet inte innehåller några visuella störningar. Undvik till exempel funktioner som gör att innehållet flimrar, blinkar eller rör sig.
* Kontrollera att popup-fönster bara visas som ett resultat av åtgärder som användaren har initierat. Tillåt inte heller formulärets aktuella fokus (användarens aktuella vy) att ändras eller att innehållet visas igen om inte användaren har startat det. Om användaren t.ex. fyller i fält i formulärets nedre halva ska fokus inte ändras till formulärets övre vänstra hörn om inte användaren väljer att navigera till den här platsen.
* Användare med funktionshinder kan behöva mer tid för att ange indata i fält. Ange inte tidsbaserade svar för inmatningsfält.
* Tänk på att skript på klienten kan störa skärmläsare och tangentbord om skriptet ändrar klientprogrammets fokus. Händelserna change och mouseEnter, när de används med nedrullningsbara listor eller listrutor, kan till exempel orsaka oväntade åtgärder. Kontrollera att dina klientskript inte orsakar några problem för skärmläsaranvändare och användare med enbart tangentbord.
* Användare av hjälpmedel behöver ibland ytterligare tid för att utföra uppgifter. Om en tidsbestämd rutin håller på att gå ut visar du ett hjälpmedelsanpassat meddelande som tillåter ett tillägg. Varningsrutor som skapas via JavaScript kan användas med hjälpmedel. Ett nytt fönster med ett meddelande som varnar användaren om att tidsgränsen närmar sig kan också distribueras.

**Relaterade kontrollpunkter**:
* Section 508 §1194.22
   * (l) När sidor använder skriptspråk för att visa innehåll, eller för att skapa gränssnittselement, ska den information som ges av skriptet identifieras med funktionstext som kan läsas med hjälpmedelsteknik.
   * p) När ett tidsbegränsat svar krävs, skall användaren underrättas och ges tillräckligt med tid för att ange att mer tid krävs.
* WCAG 1.0
   * 1.4 För alla tidsbaserade multimediepresentationer (t.ex. en film eller animering) synkroniserar du motsvarande alternativ (t.ex. bildtexter eller ljudbeskrivningar av det visuella spåret) med presentationen (P1).
   * 6.2 Se till att motsvarigheten för dynamiskt innehåll uppdateras när det dynamiska innehållet ändras.
   * 6.3 Kontrollera att sidorna kan användas när skript, miniprogram eller andra programmatiska objekt är avstängda eller inte stöds. Om detta inte är möjligt kan du ange motsvarande information på en alternativ tillgänglig sida.
   * 6.5 Kontrollera att dynamiskt innehåll är tillgängligt eller tillhandahåller en alternativ presentation eller sida (P2).
   * 8.1 Gör programmatiska element som skript och miniprogram direkt tillgängliga eller kompatibla med hjälpmedelstekniker [Prioritet 1 om funktionaliteten är viktig och inte presenteras någon annanstans], annars (P2).
   * 9.3 För skript anger du logiska händelsehanterare i stället för enhetsberoende händelsehanterare (P2).
   * 10.1 Innan användaragenter tillåter användare att stänga av fönster som skapats i förväg, får inte popup-fönster eller andra fönster visas och det aktuella fönstret ändras inte utan att användaren informeras.
* WCAG 2.0
   * 3.2.1 Vid fokus: När en komponent får fokus initierar den inte någon förändring av sammanhanget. (nivå A)
   * 3.2.2 Vid inmatning: Om du ändrar inställningen för en användargränssnittskomponent ändras inte sammanhanget automatiskt, såvida inte användaren har informerats om beteendet innan komponenten används. (nivå A)
   * 3.2.5 Ändring på begäran: Ändringar i sammanhanget initieras endast av användaren eller så finns det en mekanism som kan inaktivera sådana ändringar. (nivå AAA)

## Se till att allt ljud- och videoinnehåll är tillgängligt{#ensure-audio-video-accessible}

Om formulären innehåller ljud- eller videoinnehåll, inklusive ljud- och videoklipp, måste du se till att innehållet är tillgängligt. Se särskilt till att videoklipp som är inbyggda i formulär innehåller bildtexter (ibland kallade undertexter) för döva och hörselskadade användare samt videobeskrivningar för blinda användare. För ljudfiler som inte är synkroniserade med videoinnehåll räcker det med en enkel utskrift.
För Flash-baserade media finns information om hur du tillhandahåller bildtexter på [link](/help/forms/using/best-practices-for-creating-forms-in-designer.md).

**Relaterade kontrollpunkter**:
* Section 508 §1194.22
   * b) Motsvarande alternativ för alla multimediepresentationer skall synkroniseras med presentationen.
* WCAG 1.0
   * 1.1 Ange en textmotsvarighet för alla icke-textelement (t.ex. via &quot;alt&quot;, &quot;long&quot; eller i elementinnehållet). Detta inkluderar: bilder, grafiska representationer av text (inklusive symboler), bildschemaområden, animeringar (t.ex. animerade GIF-filer), miniprogram och programmatiska objekt, ascii-bilder, bildrutor, skript, bilder som används som listpunkter, mellanrum, grafiska knappar, ljud (spelas upp med eller utan användarinteraktion), fristående ljudfiler, ljudspår för video och video (P1).
   * 1.3 Innan användaragenter automatiskt kan läsa upp textmotsvarigheten i ett visuellt spår, kan de ge en ljudbeskrivning av den viktiga informationen i det visuella spåret i en multimediepresentation (P1).
   * 1.4 För alla tidsbaserade multimediepresentationer (t.ex. en film eller animering) synkroniserar du motsvarande alternativ (t.ex. bildtexter eller ljudbeskrivningar av det visuella spåret) med presentationen (P1).
* WCAG 2.0
   * 1.2.1 Endast ljud och endast video (inspelat i förväg): För förinspelat ljud och förinspelat video gäller följande, utom när ljud eller video är ett meditalternativ för text och är tydligt märkt som sådant: (nivå A)
   * 1.2.2 Bildtexter (inspelat i förväg): Bildtexter tillhandahålls för allt inspelat ljudinnehåll i synkroniserade medier, utom när mediet är ett mediaalternativ för text och är tydligt märkt som sådant. (nivå A)
   * 1.2.3 Ljudbeskrivning eller mediaalternativ (inspelat i förväg): Ett alternativ till tidsbaserad media- eller ljudbeskrivning av det inspelade videoinnehållet tillhandahålls för synkroniserade media, utom när mediet är ett mediaalternativ för text och är tydligt märkt som ett sådant. (nivå A)
   * 1.2.4 Bildtexter (Live): Bildtexter finns för allt direktsänt ljudinnehåll i synkroniserade media. (nivå AA)
   * 1.2.5 Ljudbeskrivning (inspelad i förväg): Ljudbeskrivning ges för allt inspelat videoinnehåll i synkroniserade media. (nivå AA)
   * 1.2.6 Signeringsspråk (inspelat i förväg): Signeringsspråk tolkas för allt inspelat ljudinnehåll i synkroniserade media. (nivå AAA)
   * 1.2.7 Utökad ljudbeskrivning (inspelad i förväg): Om pauser i förgrundsljud inte räcker till för att ljudbeskrivningar ska kunna förmedla innebörden i videon, ges en utökad ljudbeskrivning för allt inspelat videoinnehåll i synkroniserade media. (nivå AAA)
   * 1.2.8 Mediealternativ (inspelat i förväg): Ett alternativ för tidsbaserade medier tillhandahålls för alla inspelade synkroniserade media i förväg och för alla inspelade media med endast video. (nivå AAA)
   * 1.2.9 Endast ljud (live): Ett alternativ för tidsbaserade media som visar motsvarande information för direktsänt innehåll tillhandahålls. (nivå AAA)

## Identifiera naturligt språk och eventuella språkändringar{#identify-natural-language}

Formulärets innehåll läses av hjälpmedelstekniker som använder språkspecifika talsyntetiserare, så det är viktigt att korrekt identifiera formulärets primära språk för att säkerställa att formulären läses på det avsedda språket.

Om texten (eller den alternativa texten) i formulären finns på fler än ett språk måste du identifiera i vilka områden i formuläret som en växling görs mellan olika språk.

I LiveCycle Designer kan du ställa in det primära språket genom att ställa in egenskapen Locale för formuläret och egenskapen Locale för det översta delformuläret. Om du vill identifiera ändringar i det primära språket ändrar du egenskapen Språk för alla objekt som använder ett annat språk än formulärets språk.

Så här anger du egenskapen Locale för ett formulär:
1. Välj Arkiv > Formuläregenskaper och välj fliken Standard
2. Välj lämpligt språk för formulärets språkområde (se figur 17)
3. Klicka på OK

![Ändra formulärets nationella inställningar i dialogrutan Formuläregenskaper](/help/forms/using/assets/image-17.png)

Figur 17: **Ändra formulärets nationella inställningar i dialogrutan Formuläregenskaper**

Så här anger du egenskapen Local för det översta delformuläret eller ett objekt som kräver ett annat språk:
1. Markera det översta delformuläret eller objektet i designvyn
1. Visa paletten Objekt genom att välja Fönster > Objekt
1. Välj fliken Fält på paletten Objekt och välj sedan det språk som ska användas för objektet i listan Språk (se figur 18). När du tillämpar olika språkinställningar på enskilda objekt bör du tänka på att de objekt som finns i tabeller och delformulär automatiskt får samma språkinställning som tabell- och delformulärsobjektet.

![Ändra språkinställningen för ett objekt](/help/forms/using/assets/image-18.png)

Figur 18: **Ändra språkområde för ett objekt**

**Relaterade kontrollpunkter**:
* WCAG 1.0
   * 4.1 Identifiera ändringar i ett dokuments naturliga språk och eventuella motsvarande textändringar (t.ex. bildtexter).
* WCAG 2.0
   * 3.1.1 Sidans språk: Det mänskliga standardspråket för varje webbsida kan bestämmas programmatiskt. (nivå A)
   * 3.1.2 Språk i delar: Det mänskliga språket i varje stycke eller fras i innehållet kan fastställas programmatiskt med undantag för egennamn, tekniska termer, obestämda språkord och ord eller fraser som har blivit en del av språket i den omedelbart omgivande texten. (nivå AA)
