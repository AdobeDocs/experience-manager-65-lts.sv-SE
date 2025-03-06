---
title: Authoring - the Environment and Tools in AEM
description: I redigeringsmiljön i AEM finns olika sätt att ordna och redigera ditt innehåll.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 11ab6be0-ed61-4a4b-af82-d26eec982edd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2211'
ht-degree: 3%

---

# Redigering - miljö och verktyg{#authoring-the-environment-and-tools}

I redigeringsmiljön i AEM finns olika sätt att ordna och redigera ditt innehåll. Verktygen som tillhandahålls är tillgängliga från olika konsoler och sidredigerare.

## Hantera din webbplats {#managing-your-site}

Med konsolen **Platser** kan du navigera och hantera din webbplats med hjälp av sidhuvudsfältet, verktygsfältet, åtgärdsikonerna (som gäller för den valda resursen), vägbeskrivningar och, när du väljer det, sekundära spår (till exempel tidslinje och referenser).

Till exempel kolumnvy:

![ateat-01](assets/ateat-01.png)

## Redigera sidinnehåll {#editing-page-content}

Du kan redigera en sida med sidredigeraren. Till exempel:

`https://localhost:4502/editor.html/content/we-retail/us/en/equipment.html`

![ateat-02](assets/ateat-02.png)

>[!NOTE]
>
>Första gången du öppnar en sida för redigering visas en serie bilder med en genomgång av funktionerna.
>
>Du kan när som helst hoppa över genomgången och upprepa den genom att välja **Sidinformation** på menyn Sidinformation.

## Få hjälp {#accessing-help}

När du redigerar en sida kan du komma åt **hjälpen** från:

* väljaren [**Sidinformation**](/help/sites-authoring/editing-page-properties.md#page-properties). Här visas introduktionsbilderna (som visas första gången du öppnar redigeraren).
* dialogrutan [configuration](/help/sites-authoring/editing-content.md#edit-configure-copy-cut-delete-paste) för specifika komponenter (med frågetecknet (?) ikonen i dialogrutans verktygsfält); här visas sammanhangsberoende hjälp.

Ytterligare [hjälprelaterade resurser är tillgängliga från konsoler](/help/sites-authoring/basic-handling.md#accessing-help).

## Komponentbläddraren {#components-browser}

Komponentwebbläsaren visar alla komponenter som är tillgängliga för användning på den aktuella sidan. Dessa kan dras till rätt plats och sedan redigeras för att lägga till ditt innehåll.

Komponentläsaren är en flik i sidopanelen (tillsammans med [resursläsaren](/help/sites-authoring/author-environment-tools.md#assets-browser) och [innehållsträdet](/help/sites-authoring/author-environment-tools.md#content-tree)). Om du vill öppna (eller stänga) sidopanelen använder du ikonen längst upp till vänster i verktygsfältet:

![ateat-03](assets/ateat-03.png)

När du öppnar sidopanelen, bildrutorna öppnas från den vänstra sidan (välj fliken **Komponenter** om det behövs). När du är öppen kan du bläddra igenom alla komponenter som är tillgängliga för sidan.

Det faktiska utseendet och hanteringen beror på vilken enhetstyp du använder:

>[!NOTE]
>
>En mobil enhet identifieras när bredden är mindre än 1 024 pixlar. Detta kan också vara fallet för ett litet skrivbordsfönster.

* **Mobil enhet (till exempel iPad)**

  Komponentwebbläsaren täcker hela sidan som redigeras.

  Om du vill lägga till en komponent på sidan trycker du på och håller ned den nödvändiga komponenten och flyttar den åt höger - komponentwebbläsaren stängs och sidan visas igen - där du kan placera komponenten.

  ![ateat-04](assets/ateat-04.png)

* **Skrivbordsenhet**

  Komponentwebbläsaren öppnas till vänster i fönstret.

  Om du vill lägga till en komponent på sidan klickar du på den önskade komponenten och drar den till önskad plats.

  ![ateat-05](assets/ateat-05.png)

  Komponenterna representeras av

   * Komponentnamn
   * Komponentgrupp (i grått)
   * Ikon eller förkortning

      * Standardkomponenternas ikoner är monokroma.
      * Förkortningar är alltid de två första tecknen i komponentnamnet.

  I det övre verktygsfältet i webbläsaren **Komponenter** kan du göra följande:

   * Filtrera komponenter efter namn.
   * Begränsa visningen till en viss grupp med listrutan.

  Om du vill ha en mer detaljerad beskrivning av komponenten kan du klicka på informationsikonen bredvid komponenten i webbläsaren **Komponenter** (om den är tillgänglig). För **layoutbehållaren**:

  ![ateat-06](assets/ateat-06.png)

  Mer information om de komponenter som är tillgängliga för dig finns i [Komponentkonsolen](/help/sites-authoring/default-components-console.md).

## Assets Browser {#assets-browser}

Resursläsaren visar alla [resurser](/help/assets/assets.md) som är tillgängliga för direkt användning på den aktuella sidan.

Resursläsaren är en flik i sidopanelen tillsammans med [komponenterna browse](/help/sites-authoring/author-environment-tools.md#components-browser)r och [innehållsträdet](/help/sites-authoring/author-environment-tools.md#content-tree). Om du vill öppna eller stänga sidopanelen använder du ikonen längst upp till vänster i verktygsfältet:

![ateat-03-1](assets/ateat-03-1.png)

När du öppnar sidopanelen, glider den upp från vänster sida. Välj fliken **Assets** om det behövs.

![ateat-07](assets/ateat-07.png)

När resursläsaren är öppen kan du bläddra bland alla resurser som är tillgängliga för sidan. Oändlig rullning används för att expandera listan vid behov.

![ateat-08](assets/ateat-08.png)

Om du vill lägga till en resurs på sidan markerar och drar du den till önskad plats. Detta kan vara:

* En befintlig komponent av lämplig typ.

   * Du kan till exempel dra en resurs av typen bild till en bildkomponent.

* En [platshållare](/help/sites-authoring/editing-content.md#component-placeholder) i styckesystemet för att skapa en komponent av lämplig typ.

   * Du kan till exempel dra en resurs av typen bild till styckesystemet för att skapa en bildkomponent.

>[!NOTE]
>
>Detta är tillgängligt för specifika resurser och komponenttyper. Mer information finns i [Infoga en komponent med Assets Browser](/help/sites-authoring/editing-content.md#inserting-a-component-using-the-assets-browser).

I det övre verktygsfältet i resursläsaren kan du filtrera resurserna efter:

* Namn
* Bana
* Typ av resurs som bilder, manuskript, dokument, videor, sidor, stycken och produkter
* Resursegenskaper som Orientation (stående, liggande, fyrkantig) och Style (färg, monokrom, gråskala)

   * Endast tillgängligt för vissa tillgångstyper

Det faktiska utseendet och hanteringen beror på vilken enhetstyp du använder:

>[!NOTE]
>
>En mobil enhet identifieras när bredden är mindre än 1 024 pixlar, d.v.s. även i ett litet skrivbordsfönster.

* **Mobil enhet som iPad**

  Resursläsaren täcker hela sidan som redigeras.

  Om du vill lägga till en resurs på sidan trycker du på och håller kvar den önskade resursen och flyttar den åt höger. Resursläsaren stängs och sidan visas igen, där du kan lägga till resursen i den önskade komponenten.

  ![ateat-09](assets/ateat-09.png)

* **Skrivbordsenhet**

  Resursläsaren öppnas till vänster i fönstret.

  Om du vill lägga till en resurs på sidan klickar du på resursen och drar den till önskad komponent eller plats.

  ![ateat-10](assets/ateat-10.png)

Om du snabbt måste ändra en resurs kan du starta [Resursredigeraren](/help/assets/manage-assets.md) direkt från resursläsaren genom att klicka på redigeringsikonen som visas bredvid resursens namn.

![Assets Browser Desktop device](do-not-localize/screen_shot_2018-03-22at142448.png)

## Innehållsträd {#content-tree}

**Innehållsträdet** ger en översikt över alla komponenter på sidan i en hierarki så att du snabbt kan se hur sidan är uppbyggd.

Innehållsträdet är en flik i sidopanelen (tillsammans med komponenterna och resursläsaren). Om du vill öppna eller stänga sidopanelen använder du ikonen längst upp till vänster i verktygsfältet:

![Innehållsträdet](do-not-localize/screen_shot_2018-03-22at142042.png)

När du öppnar sidopanelen, glider den upp (från vänster sida). Välj fliken **Innehållsträd** om det behövs. När den är öppen kan du se en trädvyrepresentation av sidan eller mallen, så att det blir lättare att förstå hur innehållet är hierarkiskt strukturerat. På en komplex sida är det också enklare att hoppa mellan sidans komponenter.

![ateat-11](assets/ateat-11.png)

En sida kan enkelt bestå av många av samma typ av komponenter, så innehållsträdet (komponentträdet) visar beskrivande text (i grått) efter komponenttypens namn (i svart). Den beskrivande texten kommer från vanliga egenskaper för komponenten, till exempel rubrik eller text.

Komponenttyper visas på användarspråket, medan komponentbeskrivningstexten kommer från sidspråket.

Om du klickar på markeringen bredvid en komponent komprimeras eller expanderas den nivån.

![screen_shot_2018-03-22at142559](assets/screen_shot_2018-03-22at142559.png)

>[!NOTE]
>
>Innehållsträdet är inte tillgängligt om du redigerar en sida på en mobil enhet (om webbläsarbredden är mindre än 1024px).

Om du klickar på komponenten markeras komponenten i sidredigeraren. Vilka åtgärder som är tillgängliga beror på sidstatus:

* En grundsida, till exempel:

  `https://localhost:4502/editor.html/content/we-retail/language-masters/en/equipment.html`

  ![ateat-12](assets/ateat-12.png)

  Om komponenten som du klickar på i trädet är redigerbar visas en skiftnyckelsikon till höger om namnet. Om du klickar på den här ikonen öppnas redigeringsdialogrutan för komponenten.

  ![Ikon för skiftnyckel - Redigera](do-not-localize/screen_shot_2018-03-22at142725.png)

* Eller en sida som är en del av en [livecopy](/help/sites-administering/msm.md), där komponenter ärvs från en annan sida, till exempel:

  `https://localhost:4502/editor.html/content/we-retail/us/en/equipment.html`

  ![ateat-13](assets/ateat-13.png)

## Fragment - Associerad innehållsläsare {#fragments-associated-content-browser}

Om sidan innehåller innehållsfragment har du åtkomst till [webbläsaren för associerat innehåll](/help/sites-authoring/content-fragments.md#using-associated-content).

## Referenser {#references}

**Referenser** visar anslutningar till den valda sidan:

* Ritningar
* Launches
* Live-kopior
* Språkversioner
* Inkommande länkar
* Användning av referenskomponenten: lånat och lånat innehåll

Öppna den nödvändiga konsolen, gå till den nödvändiga resursen och öppna **Referenser** med:

![screen_shot_2018-03-22at153653](assets/screen_shot_2018-03-22at153653.png)

[Välj önskad resurs](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources) och visa en lista med referenstyper som är relevanta för den resursen:

![ateat-22](assets/ateat-22.png)

Välj lämplig referenstyp för mer information. I vissa situationer är ytterligare åtgärder tillgängliga när du väljer en specifik referens, bland annat:

* **Inkommande länkar** innehåller en lista med sidor som refererar till sidan, tillsammans med direktåtkomst till **Redigera** en av dessa sidor när du väljer en specifik länk.

   * Detta kan bara visa statiska länkar, inte dynamiskt genererade länkar, till exempel från List-komponenten.

* Instanser av lånat och lånat innehåll med komponenten **Reference**, härifrån kan du navigera till sidan med referenser/referenser

* [Starta](/help/sites-authoring/launches.md) ger åtkomst till relaterade starter.
* [Live-kopior](/help/sites-administering/msm.md) visar sökvägarna för alla live-kopior som baseras på den valda resursen.
* [Utskrift](/help/sites-administering/msm-best-practices.md) innehåller information och olika åtgärder.
* [Språkkopior](/help/sites-administering/tc-manage.md#creating-translation-projects-using-the-references-panel) innehåller information och olika åtgärder.

Du kan till exempel korrigera en bruten referens i en Reference-komponent:

![ateat-14](assets/ateat-14.png)

## Händelser - Tidslinje {#events-timeline}

För lämpliga resurser (till exempel sidor från konsolen **Platser** eller resurser från konsolen **Assets**) kan tidslinjen i [användas för att visa den senaste aktiviteten för valda objekt](/help/sites-authoring/basic-handling.md#timeline).

Öppna den nödvändiga konsolen, gå till den nödvändiga resursen och öppna **tidslinjen** med:

![ateat-15](assets/ateat-15.png)

[Välj önskad resurs](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources) och sedan **Visa alla** eller **Aktiviteter** för att visa alla senaste åtgärder för de valda resurserna:

![ateat-16](assets/ateat-16.png)

## Sidinformation {#page-information}

Knappen Sidinformation (utjämningsikon) öppnar en meny som även innehåller information om den senaste redigeringen och det senaste dokumentet. Beroende på sidans egenskaper, dess plats och din instans kan det finnas fler eller färre alternativ:

![ateat-17](assets/ateat-17.png)

* [Öppna egenskaper](/help/sites-authoring/editing-page-properties.md)
* [Utrullningssida](/help/sites-administering/msm.md#msm-from-the-ui)
* [Starta arbetsflöde](/help/sites-authoring/workflows-applying.md#starting-a-workflow-from-the-page-editor)
* [Lås sida](/help/sites-authoring/editing-content.md#locking-a-page)
* [Publicera sida](/help/sites-authoring/publishing-pages.md#main-pars-title-10)
* [Avpublicera sida](/help/sites-authoring/publishing-pages.md#main-pars-title-5)
* [Redigera mall](/help/sites-authoring/templates.md); när sidan är baserad på en [redigerbar mall](/help/sites-authoring/templates.md#editable-and-static-templates)

* [Visa som publicerad](/help/sites-authoring/editing-content.md#view-as-published)
* Visa i Admin; öppnar sidan i [webbplatskonsolen](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources)
* [Hjälp](/help/sites-authoring/basic-handling.md#accessing-help)

Exempel: **Sidinformation** har även följande alternativ:

* [Befordra start](/help/sites-authoring/launches-promoting.md) om sidan är en start
* [Öppna i Classic UI](/help/sites-authoring/select-ui.md#switching-to-classic-ui-when-editing-a-page) om det här alternativet har [aktiverats av en administratör](/help/sites-administering/enable-classic-ui-editor.md)

Dessutom kan **Sidinformation** ge åtkomst till analyser och rekommendationer, när det är lämpligt.

## Sidlägen {#page-modes}

Det finns olika lägen när du redigerar en sida som tillåter olika åtgärder:

* [Redigera](/help/sites-authoring/editing-content.md) - använd det här läget när du redigerar sidinnehållet.
* [Layout](/help/sites-authoring/responsive-layout.md) - gör att du kan skapa och redigera din responsiva layout beroende på enheten (om sidan baseras på en layoutbehållare)

* [Utvecklare](/help/sites-developing/developer-mode.md) - låter dig utföra olika åtgärder (kräver behörighet). Dessa omfattar granskning av den tekniska informationen på en sida och dess komponenter.

* [Design](/help/sites-authoring/default-components-designmode.md) - gör att du kan aktivera/inaktivera komponenter för användning på en sida och konfigurera komponentens design (om sidan är baserad på en [statisk mall](/help/sites-authoring/templates.md#editable-and-static-templates)).

* [Målgruppsanpassning](/help/sites-authoring/content-targeting-touch.md) - öka innehållets relevans genom målinriktning och mätning i alla kanaler.
* [Activity Map](/help/sites-authoring/page-analytics-using.md#analyticsvisiblefromthepageeditor) - visar analysdata för sidan.

* [Timewarp](/help/sites-authoring/working-with-page-versions.md#timewarp) - gör att du kan visa ett sidläge vid en viss tidpunkt.
* [Live Copy-status](/help/sites-authoring/editing-content.md#live-copy-status) - ger dig en snabb översikt över live-kopians status och vilka komponenter som ärvs/inte ärvs.
* [Förhandsgranska](/help/sites-authoring/editing-content.md#previewing-pages) - används för att visa sidan så som den visas i publiceringsmiljön, eller för att navigera med hjälp av länkar i innehållet.

* [Anteckning](/help/sites-authoring/annotations.md) - används för att lägga till eller visa anteckningar på sidan.

Du kommer åt dem med hjälp av ikonerna i det övre högra hörnet. Den faktiska ikonen ändras för att återspegla det läge som du använder för närvarande:

![ateat-18](assets/ateat-18.png)

>[!NOTE]
>
>* Beroende på sidans egenskaper kanske vissa lägen inte är tillgängliga.
>* Åtkomst till vissa lägen kräver lämplig behörighet/behörighet.
>* Utvecklarläget är inte tillgängligt på mobila enheter på grund av utrymmesbegränsningar.
>* Det finns ett [kortkommando](/help/sites-authoring/page-authoring-keyboard-shortcuts.md) ( `Ctrl-Shift-M`) som växlar mellan **Förhandsgranska** och det läge som är markerat (till exempel **Redigera** och **Layout**).
>

## Banmarkering {#path-selection}

När du redigerar är det ofta nödvändigt att välja en annan resurs, till exempel när du definierar en länk till en annan sida eller resurs eller markerar en bild. För att enkelt kunna markera en sökväg erbjuder [sökvägsfält](/help/sites-authoring/author-environment-tools.md#path-fields) automatisk komplettering och [sökvägsläsaren](/help/sites-authoring/author-environment-tools.md#path-browser) ger en mer robust markering.

### Sökvägsfält {#path-fields}

Det exempel som används här för att illustrera är bildkomponenten. Mer information om hur du använder och redigerar komponenter finns i [Komponenter för sidredigering](/help/sites-authoring/default-components.md).

Sökvägsfält har automatisk komplettering och framåtblickande funktioner som gör det enklare att hitta en resurs.

Om du klickar på knappen **Öppna dialogruta för markering** i sökvägsfältet öppnas dialogrutan [sökvägsläsare](/help/sites-authoring/author-environment-tools.md#path-browser) där du kan ange mer detaljerade markeringsalternativ.

![Öppna dialogrutan Markering](do-not-localize/screen_shot_2018-03-22at154427.png)

Du kan också börja skriva i sökvägsfältet så att AEM erbjuder matchande sökvägar när du skriver.

![ateat-19](assets/ateat-19.png)

### Sökvägsläsaren {#path-browser}

Sökvägsläsaren är organiserad som [kolumnvyn](/help/sites-authoring/basic-handling.md#column-view) i webbplatskonsolen, vilket ger ett mer detaljerat urval av resurser.

![screen_shot_2018-03-22at154521](assets/screen_shot_2018-03-22at154521.png)

* När en resurs har valts aktiveras knappen **Markera** i dialogrutans övre högra hörn. Klicka för att bekräfta markeringen eller **Avbryt** för att avbryta.
* Om det går att välja flera resurser aktiveras även knappen **Välj** när du väljer en resurs och antalet valda resurser läggs till i det övre högra hörnet fönstret. Klicka på **X** bredvid talet för att avmarkera alla.
* När du navigerar genom trädet visas platsen i de synliga kolumnerna högst upp i dialogrutan. Dessa vägbeskrivningar kan också användas för att snabbt hoppa in i resurshierarkin.
* Du kan när som helst använda sökfältet högst upp i dialogrutan. Klicka på **X** i sökfältet för att rensa sökningen.
* Om du vill begränsa sökningen kan du visa filteralternativen och filtrera resultaten baserat på en viss bana.

  ![ateat-21](assets/ateat-21.png)

## Kortkommandon {#keyboard-shortcuts}

Det finns olika [kortkommandon](/help/sites-authoring/page-authoring-keyboard-shortcuts.md).
