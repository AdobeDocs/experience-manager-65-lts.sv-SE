---
title: PDF-generering skriver inte ut ett stort antal PDF-filer med WorkBench
description: När en kund genererar ett stort antal PDF-filer via tjänster som implementerats via WorkBench misslyckas utskriftstjänsten.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 80a4e5c0-d68f-4591-a43d-ab75b5f0764c
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 0%

---

# Det går inte att skriva ut ett stort antal PDF-filer via WorkBench {#PDF-generation-fails-to-print-a-large-number-of-PDFs-via-WorkBench}

## Problem {#issue}

När en kund genererar ett stort antal PDF-filer via tjänster som implementerats via WorkBench. Tjänsten misslyckas eftersom minnet är slut. Felet visas som:

`ALC-OUT-002-013: XMLFormFactory, PAexecute failure: "0: Out of Memory"`

<!-- Attached is a simplified template (BollatoRiservatiLandscape_table_simple.xdp) that simulates the problem.
Using the Designer, if we associate the template "BollatoRiservatiLandscape_table_semplice.xdp" with the XML file "BollatoRiservati.xml" during the generation of the pdf, the process comes to occupy 1.6 Gb of RAM. On the server side, with the complete template, the pdf generation process breaks down, occupying 2 GB of RAM.-->

Detta beror på att det maximala antalet sidor i en utskriftsbegäran är begränsat till ungefär 1 000 sidor i Windows. När en utskrift genereras måste mallen och data läsas in i minnet och den resulterande layouten byggs upp i minnet. Det innebär att det finns gränser för storleken på den slutliga utdatafilen. Processen som genererar utskriften är en 32-bitarsåtgärd, vilket innebär att den är begränsad till 2 GB RAM i Windows <!--and 4 GB on UNIX-->.

## Gäller för {#applies-to}

Lösningen gäller för AEM Forms <!--JEE Server and AEM Forms on OSGi Server--> för x86_win32 XMLFM.

## Lösning {#solution}

Den största faktorn som påverkar minnesanvändningen är mängden data i ett formulär. Det finns dock andra faktorer i formulärdesignen som påverkar minnesanvändningen i mindre utsträckning. När du är medveten om dessa faktorer kan du utforma ett formulär för större utskrift. I följande avsnitt anges, i prioriterad ordning, faktorer som påverkar minnesutrymmet:

### Effektfaktor {#impact-factor}

**Hög**

1. **Alternativa delformulär** - En urvalsuppsättning med delformulär är en variant av delformuläruppsättningsobjektet som gör att du kan anpassa visningen av specifika delformulär inifrån uppsättningen med hjälp av villkorssatser.
1. **Använd statisk text i stället för bildtexter** - Nästan alla fält innehåller en bildtext inuti, och du bör använda den istället för att ha ytterligare statisk text som bildtext.
1. Använd **RTF** när det är möjligt.

**Medel**

Ytterligare faktorer som du bör tänka på när du utformar formulärmallen för att förbättra minnesanvändningen:

1. Använd inte statisk text för att etikettera ett fält. Använd i stället bildtexter i textfältet.
2. Använd inte för många rektanglar, linjer, objekt och tabeller.
3. Undvik om möjligt att använda delformulär för RichText och Choice.
4. Undvik överdriven användning av delformulär och kapslade delformulär.

### Begränsning av datastorlek {#data-size-limitations}

Eftersom vi är begränsade av det maximala processminnet och det minne som används av processen beror inte bara på datafilens storlek. Den är mycket nära kopplad till formulärdesignen och i viss utsträckning till den faktiska mängden data som sammanfogas i formuläret.

Om formuläret har många små noder med små data tar processen mer minne i anspråk (och därför går det snabbare att få slut på minne) än ett formulär som har färre antal noder (även) med stora data.

Läs [Bilaga nedan](#appendix) om du vill ha mer information, där testresultaten baseras på utskriftsformuläret (ej taggat PDF). Behovet av taggad PDF-processminne ökar. Det beror också på antalet fält i formuläret. Processminneskravet är ungefär 1,5 gånger större än för otaggade PDF.

### Interaktiv Forms {#interactive-forms}

Interaktiva formulär förbrukar mer minne än Skriv ut Forms eftersom interaktiva fält återges igen. I de tester som utfördes ökade minnesförbrukningen med en faktor på cirka 1,5 jämfört med tryckta formulär och dessa var statiska interaktiva former.

### Bildformat {#image-formats}

Adobe rekommenderar inga specifika bildformat. Men det skulle vara bra att ha en mindre bildstorlek, till exempel PNG (Portable Network Graphics). Det är inte heller tillrådligt att använda bilder med hög upplösning, vars storlek varierar mellan hundratals MegaBytes. Det är inte heller tillrådligt att använda komprimerade bilder vars storlek vid dekomprimering sträcker sig till flera hundra megabyte med data.

### Bilaga {#appendix}

**Tabellexempel**

Olika varianter för tabeller visas nedan som visar återgivningsantal för sidor jämfört med datastorlek för enkla och komplexa tabeller.

1. En tabell med en enda kolumn där 5 000 sidor PDF-filer genereras, datafilens storlek är 24 MB och 30 kB.

   ![table_single_column](/help/forms/using/assets/table_single_column.png)

1. En tabell med många små kolumner där 800 sidor PDF-filer genereras, är datafilens storlek 4,6 MB och 20-K poster.
   ![table_many_small_columns](/help/forms/using/assets/table_many_small_columns.png)

1. En tabell med många små kolumner, men större datafil på grund av användningen av större xmlTag-namn.
Här är allt detsamma som föregående, men XML-taggnamn har gjorts stora (så att datafilens storlek ökar utan att de faktiska effektiva data ökar), slutresultatet (den övre gränsen) är nästan detsamma. Datafilens storlek ökade från 4,6 MB till 44,6 MB. Här genereras 800 PDF-sidor, datafilens storlek är 44,6 MB och 20 kB.

   ![table_larger_xml_tagname](/help/forms/using/assets/table_bigger_xml_tagname.png)

Det är därför svårt att sätta en allmän övre gräns för datafilens storlek. Varje formulär är unikt, och därför skiljer sig minnesanvändningen från formulär till formulär.
