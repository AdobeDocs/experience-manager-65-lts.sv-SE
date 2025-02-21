---
title: Arbeta med innehållsfragment
description: Läs om hur du med Content Fragments i Adobe Experience Manager (AEM) kan designa, skapa, strukturera och använda sidoberoende innehåll, idealiskt för headless-leverans.
feature: Content Fragments
role: User
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1966'
ht-degree: 3%

---

# Arbeta med innehållsfragment {#working-with-content-fragments}

Med Adobe Experience Manager (AEM) kan du utforma, skapa, strukturera och [publicera sidoberoende innehåll](/help/sites-authoring/content-fragments.md) i innehållsfragment. De gör att du kan förbereda innehåll för användning på flera platser/i flera kanaler, idealiskt för headless-leverans.

Innehållsfragment innehåller strukturerat innehåll:

* De är baserade på en [Content Fragment Model](/help/assets/content-fragments/content-fragments-models.md) som fördefinierar en struktur för det resulterande fragmentet.
* Strukturen kan variera mellan:
   * Grundläggande
      * Ett enda textfält med flera rader.
      * Används för att förbereda okomplicerat innehåll för sidredigering.
   * Komplex
      * En kombination av många fält med olika datatyper, bland annat text, tal, boolesk information och tid.
      * Används antingen för att förbereda mer strukturerat innehåll för sidredigering eller för att skickas till programmet.
   * Kapslad
      * Med de tillgängliga referensdatatyperna kan du kapsla in ditt innehåll.
      * Tender som ska användas för leverans till ditt program.

Innehållsfragment kan också levereras i JSON-format med exportfunktionerna för Sling Model (JSON) i AEM kärnkomponenter. Leveranssätt:

* gör att du kan använda komponenten för att hantera vilka element i ett fragment som ska levereras
* tillåter massleverans genom att lägga till flera kärnkomponenter för innehållsfragment på sidan som används för API-leverans

På den här och följande sidor beskrivs hur du skapar, konfigurerar, underhåller och använder innehållsfragment:

* [Aktivera funktionen för innehållsfragment för din instans](/help/assets/content-fragments/content-fragments-configuration-browser.md)
* [Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md) - aktivera, skapa och definiera dina modeller
* [Hantera innehållsfragment](/help/assets/content-fragments/content-fragments-managing.md) - skapa dina innehållsfragment och redigera, publicera och referera sedan
* [Variationer - Innehåll för redigeringsfragment](/help/assets/content-fragments/content-fragments-variations.md) - författare till fragmentinnehållet och skapa varianter av mallsidan
* [Markering](/help/assets/content-fragments/content-fragments-markdown.md) - använder markeringssyntax för ditt fragment
* [Använder associerat innehåll](/help/assets/content-fragments/content-fragments-assoc-content.md) - lägger till associerat innehåll
* [Metadata - Fragmentegenskaper](/help/assets/content-fragments/content-fragments-metadata.md) - visa och redigera fragmentegenskaperna
* Använd [Content Fragments tillsammans med GraphQL för att leverera innehåll ](/help/assets/content-fragments/content-fragments-graphql.md) som kan användas i dina program. Om du vill ha hjälp med detta kan du förhandsgranska [JSON-utdata](/help/assets/content-fragments/content-fragments-json-preview.md).

>[!NOTE]
>
>Dessa sidor kan läsas med:
>
>* [Sidredigering med innehållsfragment](/help/sites-authoring/content-fragments.md).
>* [Anpassa och utöka Content Fragments](/help/sites-developing/customizing-content-fragments.md)
>* [Content Fragments – konfigurera komponenter för återgivning](/help/sites-developing/content-fragments-config-components-rendering.md)
>* [Stöd för Content Fragments i AEM Assets HTTP API](/help/assets/assets-api-content-fragments.md)
>* [AEM GraphQL API för användning med innehållsfragment](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md)

Antalet kommunikationskanaler ökar årligen. Kanalerna avser vanligen leveransmekanismen, antingen som:

* Fysisk kanal, till exempel dator, mobil.
* Leveranssätt i en fysisk kanal, t.ex.&quot;produktinformationssida&quot;,&quot;produktkategorisida&quot; för dator eller&quot;mobilwebb&quot;,&quot;mobilapp&quot; för mobil.

Men du vill (förmodligen) inte använda samma innehåll för alla kanaler. Du måste optimera innehållet utifrån den specifika kanalen.

Med innehållsfragment kan du:

* Fundera på hur ni kan nå målgrupper effektivt i alla kanaler.
* Skapa och hantera kanalneutralt redaktionellt innehåll.
* Bygg innehållspooler för en rad olika kanaler.
* Utforma innehållsvarianter för specifika kanaler.
* Lägg till bilder i texten genom att infoga resurser (blandade mediefragment).
* Skapa kapslat innehåll så att ni kan återspegla komplexiteten hos era data.

Dessa innehållsfragment kan sedan sammanställas för att ge upplevelser över olika kanaler.

>[!NOTE]
>
>**Innehållsfragment** och **[Upplevelsefragment](/help/sites-authoring/experience-fragments.md)** är olika funktioner i AEM:
>
>* **Innehållsfragment** är redaktionellt innehåll som kan användas för att komma åt strukturerade data, bland annat texter, siffror och datum. De är rent innehåll, med definition och struktur, men utan ytterligare visuell design och/eller layout.
>
>* **Upplevelsefragment** är helt utformat för innehåll, ett fragment av en webbsida.
>
>Upplevelsefragment kan innehålla innehåll i form av innehållsfragment, men inte tvärtom.
>
>Mer information finns i [Om innehållsfragment och upplevelsefragment i AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html#content-fragments).

>[!NOTE]
>
>Före AEM 6.3 skapades innehållsfragment med hjälp av mallar i stället för modeller. Mallar är inte längre tillgängliga för att skapa fragment, men fragment som skapats med en sådan mall stöds fortfarande.

## Innehållsfragment och innehållstjänster {#content-fragments-and-content-services}

AEM Content Services är utformat för att generalisera beskrivningen och leveransen av innehåll i/från AEM utöver fokus på webbsidor.

De levererar innehåll till kanaler som inte är traditionella AEM webbsidor, med standardiserade metoder som kan användas av alla kunder. Dessa kanaler kan omfatta:

* Enkelsidiga program
* Inbyggda mobilprogram
* andra kanaler och kontaktpunkter utanför AEM

Leveransen görs i JSON-format med JSON-exporteraren.

AEM Content Fragments kan användas för att beskriva och hantera strukturerat innehåll. Strukturerat innehåll definieras i modeller som kan innehålla olika innehållstyper, bland annat text, numeriska data, boolesk information, datum och tid.

Tillsammans med JSON-exportfunktionerna i AEM kärnkomponenter kan detta strukturerade innehåll sedan användas för att leverera AEM-innehåll till andra kanaler än AEM-sidor.

<!--
>[!NOTE]
>
>See [Headless and AEM](/help/implementing/developing/headless/introduction.md) for an introduction to Headless Development for AEM Sites.
-->

>[!NOTE]
>
>AEM har också stöd för översättning av fragmentinnehåll.

<!--
>[!NOTE]
>
>AEM also supports the translation of fragment content. See [Translating Assets](/help/assets/translate-assets.md) for further information.
-->

## Innehållstyp {#content-type}

Innehållsfragmenten är:

* Lagrat som **Assets**:

   * Innehållsfragment (och deras varianter) kan skapas och underhållas från **Assets** -konsolen.
   * Skapat och redigerat i Content Fragment Editor.

* Används i [sidredigeraren med komponenten Content Fragment](/help/sites-authoring/content-fragments.md) (refereringskomponent):

   * Komponenten **Innehållsfragment** är tillgänglig för sidförfattare. De kan referera till och leverera det nödvändiga innehållsfragmentet i antingen HTML- eller JSON-format.

* Kan nås med [AEM GraphQL API](/help/sites-developing/headless/graphql-api/graphql-api-content-fragments.md).

Innehållsfragment är en innehållsstruktur som:

* Har ingen layout eller design (viss textformatering är möjlig i RTF-läge).
* Har en eller flera [beståndsdelar](#constituent-parts-of-a-content-fragment).
* Kan [innehålla eller vara ansluten till bilder](#fragments-with-visual-assets).
* Kan använda [mellanliggande innehåll](#in-between-content-when-page-authoring-with-content-fragments) vid referens på en sida.
* Är oberoende av leveransmekanismen (d.v.s. sidan, kanalen).

### Fragment med Visual Assets {#fragments-with-visual-assets}

För att ge författarna bättre kontroll över sitt innehåll kan bilder läggas till och/eller integreras med ett innehållsfragment.

Assets kan användas med ett innehållsavdrag på flera olika sätt, vart och ett med sina egna fördelar:

* **Infoga resurs** i ett fragment (blandade mediefragment)

   * Är en del av fragmentet (se [Ingående delar i ett innehållsfragment](#constituent-parts-of-a-content-fragment)).
   * Definiera tillgångens position.
   * Mer information finns i [Infoga Assets i fragmentet](/help/assets/content-fragments/content-fragments-variations.md#inserting-assets-into-your-fragment) i fragmentredigeraren.

  >[!NOTE]
  >
  >Visuella resurser som infogats i själva innehållsfragmentet kopplas till föregående stycke. När fragmentet läggs till på en sida flyttas dessa resurser i relation till det stycket när mellanliggande innehåll läggs till.

* **Associerat innehåll**

   * Är anslutna till ett fragment, men inte en fast del av fragmentet (se [Ingående delar i ett innehållsfragment](#constituent-parts-of-a-content-fragment)).
   * Möjliggör viss flexibilitet för placering.
   * Är enkelt tillgängliga för användning (som mellanliggande innehåll) när du använder fragmentet på en sida.
   * Mer information finns i [Associerat innehåll](/help/assets/content-fragments/content-fragments-assoc-content.md).

* Resurser som är tillgängliga från **resursläsaren** i sidredigeraren

   * Möjliggör full flexibilitet för val av en resurs.
   * Möjliggör viss flexibilitet för placering.
   * Innebär inte att man kan godkänna ett visst fragment.

<!--
  * See [Assets Browser](/help/sites-authoring/environment-tools.md#assets-browser) for more information.
-->

### Komponentdelar i ett innehållsfragment {#constituent-parts-of-a-content-fragment}

Resurserna för innehållsfragmentet består av följande delar (antingen direkt eller indirekt):

* **Fragmentelement**

   * Elementen korrelerar till de datafält som innehåller innehåll.
   * Du använder en innehållsmodell för att skapa innehållsfragmentet. Elementen (fälten) som anges i modellen definierar fragmentets struktur. Dessa element (fält) kan vara av olika datatyper.

* **Fragmentera stycken**

   * Block med text, ofta flera rader, som avgränsas som enskilda enheter.

   * I lägena [RTF](/help/assets/content-fragments/content-fragments-variations.md#rich-text) och [Markdown-kod](/help/assets/content-fragments/content-fragments-variations.md#markdown) kan ett stycke formateras som en rubrik, och då utgör det och det efterföljande stycket en enhet.

   * Aktivera innehållskontroll vid sidredigering.

* **Assets har infogats i ett fragment (blandade mediefragment)**

   * Assets (bilder) infogade i det faktiska fragmentet och används som det interna innehållet i ett fragment.
   * Är inbäddade i fragmentets styckesystem.
   * Kan formateras när [fragmentet används/refereras på en sida ](/help/sites-authoring/content-fragments.md).
   * Kan endast läggas till, tas bort från eller flyttas inom ett fragment med fragmentredigeraren. Dessa åtgärder kan inte utföras i sidredigeraren.
   * Det går bara att lägga till, ta bort eller flytta inom ett fragment med formatet [RTF i fragmentredigeraren](/help/assets/content-fragments/content-fragments-variations.md#inserting-assets-into-your-fragment).
   * Kan endast läggas till i flerradiga textelement (alla fragmenttyper).
   * Kopplas till föregående text (stycke).

     >[!CAUTION]
     >
     >Assets kan tas bort (oavsiktligt) från ett fragment genom att växla till oformaterad text.

     >[!NOTE]
     >
     >Assets kan också läggas till som [ytterligare (mellanliggande) innehåll](/help/sites-authoring/content-fragments.md#using-associated-content) när du använder ett fragment på en sida, antingen associerat innehåll eller resurser från Assets webbläsare.

* **Associerat innehåll**

   * Detta innehåll är externt, men med redaktionell relevans för, ett fragment. Vanligtvis bilder, videor eller andra fragment.
   * De enskilda resurserna i samlingen är tillgängliga för användning med fragmentet i sidredigeraren när det läggs till på en sida. Det innebär att de är valfria, beroende på den specifika kanalens krav.
   * Resurserna är [kopplade till fragment via samlingar](/help/assets/content-fragments/content-fragments-assoc-content.md). Med associerade samlingar kan författaren bestämma vilka resurser som ska användas när de redigerar sidan.

      * Samlingar kan kopplas till fragment som standardinnehåll, eller av författare vid fragmentredigering.
      * [Assets (DAM)-samlingar](/help/assets/manage-collections.md) är grunden för det associerade fragmentinnehållet.
   * Du kan också lägga till själva fragmentet i en samling för att underlätta spårningen.

* **Fragmentmetadata**

   * Använd [Assets-metadatamatcheman](/help/assets/metadata-schemas.md).
   * Taggar kan skapas när du:

      * Skapa och redigera fragmentet
      * Eller senare:

         * Genom att visa/redigera fragmentet **Egenskaper** från konsolen
         * Genom att redigera **metadata** i fragmentredigeraren

  >[!CAUTION]
  >
  >Metadatabearbetningsprofiler gäller inte för innehållsfragment.

* **Mallen**

   * En del av fragmentet

      * Alla innehållsfragment har en instans av Master.
      * Mallen kan inte tas bort.

   * Mallen är tillgänglig i fragmentredigeraren under **[Variationer](/help/assets/content-fragments/content-fragments-variations.md)**.
   * Huvudet är inte en variation i sig, utan baserar alla variationer.

* **Variationer**

   * Återgivningar av fragmenttext som är specifik för ett redaktionellt syfte. Den kan vara relaterad till kanalen men är inte obligatorisk, men kan även användas för lokala ad hoc-ändringar.
   * Skapas som kopior av **mallsida**, men kan sedan redigeras efter behov. Det finns innehåll som överlappar själva variationerna.
   * Kan definieras vid fragmentredigering.
   * Lagras i fragmentet för att undvika spridning av innehållskopior.
   * Variationer kan [synkroniseras](/help/assets/content-fragments/content-fragments-variations.md#synchronizing-with-master) med mallsidan om huvudinnehållet har uppdaterats.
   * Kan vara [Summerad](/help/assets/content-fragments/content-fragments-variations.md#summarizing-text) för att snabbt korta av texten till en fördefinierad längd.
   * Tillgängligt på fliken [Variationer](/help/assets/content-fragments/content-fragments-variations.md) i fragmentredigeraren.

### Mellan innehåll vid sidredigering med innehållsfragment {#in-between-content-when-page-authoring-with-content-fragments}

Mellanliggande innehåll:

* Kan användas i sidredigeraren när du arbetar med innehållsfragment.
* Är [ytterligare innehåll tillagt i flödet för ett fragment](/help/sites-authoring/content-fragments.md#adding-in-between-content) när det har använts eller refererats på en sida.
* Kan användas i [sidredigeraren när du arbetar med innehållsfragment](/help/sites-authoring/content-fragments.md).
* Mellanliggande innehåll kan läggas till i vilket fragment som helst, där bara ett element är synligt.
* Associerat innehåll kan användas, liksom resurser och/eller komponenter från lämplig webbläsare.

>[!CAUTION]
>
>Det mellanliggande innehållet är sidinnehåll. Den lagras inte i innehållsfragmentet.

### Krävs av fragment {#required-by-fragments}

Om du vill skapa innehållsfragment bör du tänka på följande:

* **Innehållsmodell**

   * Är [aktiverade med hjälp av Konfigurationsläsaren](/help/assets/content-fragments/content-fragments-configuration-browser.md).
   * Skapas [med verktyg](/help/assets/content-fragments/content-fragments-models.md).
   * Krävs för att [skapa ett fragment](/help/assets/content-fragments/content-fragments-managing.md#creating-content-fragments).
   * Definierar strukturen för ett fragment (rubrik, innehållselement, taggdefinitioner).
   * Innehållsmodelldefinitioner kräver en titel och ett dataelement. Allt annat är valfritt.
   * Modellen kan definiera standardinnehåll, om tillämpligt.
   * Författare kan inte ändra den definierade strukturen när de redigerar fragmentinnehåll.
   * Ändringar som görs i en modell efter att beroende innehållsfragment har skapats kan påverka dessa innehållsfragment.

Om du vill använda dina innehållsfragment för att skapa sidor behöver du också:

* **Innehållsfragmentkomponent**

   * Instrumentellt för att leverera fragmentet i HTML- och/eller JSON-format.
   * Krävs för att [referera till fragmentet på en sida](/help/sites-authoring/content-fragments.md).
   * Ansvarig för layout och leverans av ett fragment, det vill säga kanaler.
   * Fragment behöver en eller flera dedikerade komponenter för att definiera layouten och leverera vissa eller alla element/varianter och tillhörande innehåll.
   * Om du drar ett fragment till en sida när du redigerar kopplas den nödvändiga komponenten automatiskt till.

## Exempel på användning {#example-usage}

Ett fragment, med dess element och variationer, kan användas för att skapa sammanhängande innehåll för flera kanaler. När du utformar ditt fragment måste du tänka på vad som används och var det används.
