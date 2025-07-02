---
title: God praxis för HTML5-formulär
description: Finjustera din XFA-baserade HTML5 Forms för bästa prestanda.
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 7f66832f-c9d1-43e1-b3a4-a25ea9a8a224
source-git-commit: 7a52531c6c9c1c81ab563fd85f67ba4b7f81d318
workflow-type: tm+mt
source-wordcount: '1389'
ht-degree: 0%

---

# God praxis för HTML5-formulär{#best-practices-for-html-forms}

## Ökning {#overview}

AEM Forms har en komponent som kallas HTML5-formulär. Det hjälper till att återge befintliga XFA-baserade PDF forms (XDP-filer) i HTML5-format. Det här dokumentet innehåller riktlinjer och rekommendationer för att minska inläsningstiden och förbättra prestanda för HTML5-formulär på mobila enheter.

De flesta mobila enheter har begränsad processorkraft och minneskapacitet. Det hjälper till att förbättra väntetiden för mobila enheter. Webbläsare som körs på en mobil enhet har tillgång till begränsade resurser (begränsat minne och begränsad processorkapacitet). När gränsen har nåtts blir webbläsarbeteendet trögt. Det här dokumentet innehåller rekommendationer för att kontrollera storleken på ett HTML5-formulär. En mindre form bryter inte minnes- och processorenergigränserna för en enhet och ger en smidig upplevelse.

Även om rekommendationerna i den här artikeln är inriktade på HTML5-formulär är de lika tillämpliga på XFA-baserade PDF forms. Dessa bästa metoder bidrar tillsammans till att HTML5-blanketterna fungerar optimalt. Det krävs noggrann planering för att utveckla effektiva och produktiva formulär. Kom igång:

## Noderna är valuta för HTML5-formulär och använd dem klokt {#nodes-are-currency-of-html-forms-spend-them-wisely}

I allmänhet har ett XFA-formulär flera element. Till exempel tabell, textfält och bilder. Alla element har flera egenskaper som styr elementets beteende och utseende. När ett XFA-formulär återges i HTML5-format konverteras alla XFA-element och motsvarande egenskaper till modell- eller HTML DOM-noder. De här noderna ökar storleken och komplexiteten på en DOM. Gör HTML5-formuläret långsamt att återge.

Det är enklare för webbläsarna att återge en mindre DOM. Så du kan utföra följande optimeringar på ett XFA-formulär för att minska antalet noder. Generera därför en tunn DOM-struktur:

* Använd egenskapen caption för att lägga till en etikett i ett fält. Använd inte ett separat textelement för att lägga till en etikett. Det hjälper till att ge extra vikt, vilket leder till prestandavinster. Det hjälper även till att undvika layoutproblem.
* Behåll minsta möjliga antal textelement i ett formulär. Rita-element underlättar läsbarheten och utseendet, men har inga funktioner för informationslagring. Du bör sammanfoga flera Draw-textelement till ett enda Draw-textelement. Låt ingen sten vara omsluten så att formen blir smidigare.

## Lite-formulär fungerar bättre, håll resurserna komprimerade {#lite-forms-perform-better-keep-the-resources-compressed}

Ett HTML5-formulär kan innehålla flera externa resurser, till exempel bild-, JavaScript- och CSS-filer. Varje gång en webbläsare begär ett formulär skickas de externa resurserna via nätverket. Den tid som krävs för att resa över nätverket är direkt proportionerlig till filstorleken.

Att minska storleken på de externa resurserna och endast använda absolut nödvändiga resurser är därför den bästa metoden att förbättra formulärens prestanda. Du kan utföra följande optimeringar på ett XFA-formulär för att minska storleken på externa resurser i ett formulär:

* Använd [komprimerade bilder](/help/assets/best-practices-for-optimizing-the-quality-of-your-images.md). Det minskar nätverksaktiviteten och den mängd minne som krävs för att återge ett formulär. Inläsningstiden för formuläret minskar därför avsevärt.
* Använd alternativet minify i AEM Configuration Manager (Day CQ HTML Library Manager) för att komprimera JavaScript- och CSS-filer. Mer information finns i [OSGi-konfigurationsinställningar](/help/sites-deploying/osgi-configuration-settings.md).
* Aktivera webbkomprimering. Det minskar storleken på begäranden och svar som kommer från ett formulär. Mer information finns i [Prestandajustering av AEM Forms Server](https://helpx.adobe.com/aem-forms/6-3/performance-tuning-aem-forms.html).

## Håll intresset levande, visa endast obligatoriska fält  {#keep-the-interest-alive-show-only-required-fields}

Ett HTML5-formulär kan innehålla hundratals sidor. Ett formulär med ett stort antal fält går långsamt att läsa in i webbläsaren. Du kan göra följande optimeringar i ett XFA-formulär för att optimera formulären med ett stort antal fält och sidor:

* Utvärdera att dela upp stora formulär i flera formulär. Du kan också använda en formuläruppsättning för att gruppera alla mindre formulär och presentera dem som en enda enhet. En formuläruppsättning läser bara in de formulär som krävs. I en formuläruppsättning kan du dessutom konfigurera gemensamma fält i olika formulär för att dela databindningar. Databindningar hjälper användarna att fylla i vanlig information endast en gång. Informationen fylls i automatiskt i efterföljande formulär, vilket leder till avsevärda prestandaförbättringar. Mer information om formuläruppsättningar finns i [Formuläruppsättning i AEM-formulär](https://helpx.adobe.com/aem-forms/6-3/formset-in-aem-forms.html).
* Överväg att dela avsnitt och flytta varje avsnitt till en annan sida. HTML5-formulär läser dynamiskt in varje sida vid sidbläddring. Det är bara rullade sidor (sidan som visas och de sidor som föregår den) som lagras i minnet. Resten av sidorna läses in vid behov. Att dela och flytta ett avsnitt på en egen sida minskar därmed den tid som krävs för att läsa in ett formulär. Du kan också använda formulärets första sida som en landningssida. Den liknar innehållsförteckningen i en bok. En landningssida i formuläret innehåller bara länkar till andra avsnitt i formuläret. Det förbättrar inläsningstiden avsevärt för den första sidan i formuläret och ger en förbättrad användarupplevelse.
* Behåll villkorliga avsnitt dolda som standard. Gör de här avsnitten synliga endast när ett visst villkor är uppfyllt. Det hjälper till att minimera storleken på DOM. Du kan också använda fliknavigering om du bara vill visa ett avsnitt i taget.

## Mindre är mer, minska antalet sidor {#less-is-more-reduce-the-number-of-pages}

HTML5-formulär kan innehålla datadrivna fält (tabeller och delformulär). Dessa fält utökar formulärets storlek vid körning. En datadriven tabell i ett HTML5-formulär kan till exempel omfatta tusentals rader. Sådana tabeller kan orsaka layout- och prestandaförsämringar. De optimeringar som föreslås nedan kan hjälpa dig att minska inläsningstiden för HTML5-formulär med datadrivna fält:

* Använd XFA-skriptning för att skapa sidbaserad navigering för att visa datadrivna fält (tabeller och delformulär). Vid sidindelad navigering visas bara specifika data på en sida. Webbläsarmålningen begränsas till fälten som visas samtidigt och gör det enklare att navigera i ett formulär. Dessutom är användare på mobila enheter endast intresserade av en delmängd av data. Det hjälper er att leverera enastående användarupplevelser och minskar tiden som krävs för att läsa in de data som behövs. Du får två lösningar till priset av en.  Tänk också på att sidindelad navigering inte är tillgängligt direkt. Du kan använda XFA-skript för att utveckla sidnavigering.

* Utvärdera sammanslagning av flera skrivskyddade kolumner till en enda kolumn. Det minskar det minne som krävs för att visa formuläret. Undvik också att visa kolumner som inte kräver några indata från användarna.
* Utvärdera att dela det datadrivna formuläret i en [formuläruppsättning](https://helpx.adobe.com/aem-forms/6-3/formset-in-aem-forms.html) om förslagen ovan inte ger många förbättringar. Om en tabell till exempel har fler än 1 000 rader flyttar du var 100:e rad till ett annat formulär. Det skulle förbättra inläsningstiden och prestanda för formulären.  Observera också att en formuläruppsättning skapar en konsoliderad XML för alla formulär. Använd olika datarötter för att skilja på data i alla typer av formulär. Mer information finns i [Formuläruppsättning i AEM Forms](https://helpx.adobe.com/aem-forms/6-3/formset-in-aem-forms.html).

## Två för dokumentdokument (DOR) {#power-of-two-for-document-of-record-dor}

Ett XFA-formulär kan ha ett stort antal avsnitt som bara är dedikerade för DOR (Document of Record). Om du vill minska antalet noder och förbättra prestanda för ett sådant formulär kan du behålla olika kopior av formuläret - en kopia för att fylla i formuläret och en annan för att generera ett dokument på servern. I kopian för att fylla i XFA-formuläret visas endast fält som krävs för att hämta data. I genereringen av dokument för post-XFA från behåller du bara de fält som krävs i den utskrivna kopian av formuläret. Innan du väljer det föreslagna tillvägagångssättet ska du utvärdera prestandavinster och underhållskostnader.

## Rekommenderade läsningar  {#recommended-reads}

Adobe Experience Manager (AEM)-formulär kan hjälpa er att omvandla komplexa transaktioner till enkla, roliga digitala upplevelser. Det krävs dock samordnade insatser för att utveckla effektiva och produktiva formulär. Förutom HTML5 Forms finns här några rekommenderade artiklar om AEM allmänna bästa praxis:

* [Bästa tillvägagångssätt för driftsättning och underhåll av AEM](/help/sites-deploying/best-practices.md)
* [Bästa tillvägagångssätt för att skapa innehåll](/help/sites-authoring/best-practices.md)
* [Bästa tillvägagångssätt för att administrera AEM](/help/sites-administering/administer-best-practices.md)
* [Bästa tillvägagångssätt för att utveckla lösningar](/help/sites-developing/best-practices.md)
* [Bästa tillvägagångssätt för arbete med anpassningsbara formulär](/help/forms/using/adaptive-forms-best-practices.md)

## Lathund {#quick-reference-card}

Du kan skriva ut följande kort (klicka på kortet för att ladda ned en högupplöst version) och ha det på skrivbordet för en snabb referens:
[![HTML5 Forms Best practices snabbreferenskort](do-not-localize/best-practices_reference_card.png)](assets/html5_forms_best_practices_reference_card.pdf)
