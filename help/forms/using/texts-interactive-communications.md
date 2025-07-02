---
title: Texter i interaktiv kommunikation
description: Att skapa och redigera textdokumentfragment som ska användas i interaktiv kommunikation - text är en av de fyra typer av dokumentfragment som används för att skapa interaktiv kommunikation. De andra tre är villkor, listor och layoutfragment.
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: ca18b9f4-9d06-4b15-81dd-68a6821e2e3e
source-git-commit: 6db207b08535c063e41b333054561036481e8db9
workflow-type: tm+mt
source-wordcount: '2410'
ht-degree: 0%

---

# Texter i interaktiv kommunikation{#texts-in-interactive-communications}

## Ökning {#overview}

Ett textdokumentfragment består av ett eller flera textstycken. Ett stycke kan vara statiskt eller dynamiskt. Ett dynamiskt stycke kan innehålla egenskaper och variabler för formulärdatamodellen. Du kan också använda regler och upprepa i ett textdokumentfragment. Kundnamnet i en hälsningsfras kan till exempel vara en FDM-egenskap (Form Data Model) vars värde är tillgängligt vid körning. Genom att ändra dessa värden kan samma interaktiva kommunikation användas för att förbereda interaktiv kommunikation för olika kunder med hjälp av agentens användargränssnitt.

Textdokumentavsnittet i Interaktiv kommunikation har stöd för följande typer av dynamiska data:

* **Datamodellsobjekt**: Dataegenskaperna använder en backend-datakälla.
* **Regelbaserat innehåll**: Delar av innehåll i en text som visas eller döljs baserat på en regel. En regel kan också baseras på egenskaper och variabler för formulärdatamodellen.
* **Variabler**: I textdokumentfragment är variabler inte bundna till en backend-datakälla. Agenten fyller i/väljer värden i variabler eller binder variablerna till datakällor när den förbereder interaktiva kommunikationen för att skicka den till en postprocess.
* **Upprepa**: Du kan ha dynamisk information i din interaktiva kommunikation, t.ex. transaktioner i en kreditkortssats, vars antal kan ändras kontinuerligt för varje genererad interaktiv kommunikation. Med upprepning kan du formatera och strukturera sådan dynamisk information. Mer information finns i [Infogat villkor och upprepa](https://helpx.adobe.com/se/experience-manager/6-3/forms/using/cm-inline-condition.html).

## Skapa text {#createtext}

1. Välj **[!UICONTROL Forms]** > **[!UICONTROL Document Fragments]**.
1. Välj **[!UICONTROL Create]** > **[!UICONTROL Text]**.
1. Ange följande information:

   * **[!UICONTROL Title]**: (Valfritt) Ange titeln för textdokumentfragmentet. Titlar behöver inte vara unika och kan innehålla specialtecken och tecken som inte är engelska. Texterna refereras till av sina titlar (om de är tillgängliga) som miniatyrbilder och egenskaper.
   * **[!UICONTROL Name]**: Det unika namnet för texten, i en mapp. Det får inte finnas två dokumentfragment (text, villkor eller lista) i något läge med samma namn i en mapp. I fältet Namn kan du bara ange engelska tecken, siffror och bindestreck. Fältet Namn fylls i automatiskt baserat på fältet Titel. De specialtecken, blanksteg, siffror och icke-engelska tecken som anges i fältet Titel ersätts med bindestreck i fältet Namn. Även om värdet i fältet Titel automatiskt kopieras till namnet kan du redigera värdet.

   * **[!UICONTROL Description]**: Skriv en beskrivning av texten.
   * **[!UICONTROL Form Data Model]**: Om du vill kan du välja alternativknappen Formulärdatamodell för att skapa texten baserat på en formulärdatamodell. När du väljer alternativknappen Formulärdatamodell visas fältet **[!UICONTROL Form Data Model]**. Bläddra och välj en formulärdatamodell. När du skapar text och villkor för en interaktiv kommunikation måste du se till att du använder samma datamodell som du tänker använda i den interaktiva kommunikationen. Mer information om formulärdatamodell finns i [Dataintegrering](/help/forms/using/data-integration.md).

   * **[!UICONTROL Tags]**: Om du vill skapa en egen tagg anger du ett värde i textfältet och trycker på Retur. När du sparar den här texten skapas de nya taggarna.

1. Välj **[!UICONTROL Next]**.

   Sidan Skapa text visas. Om du har valt att skapa en formulärdatamodellbaserad text visas egenskaperna för formulärdatamodellen i den vänstra rutan.

1. Skriv texten och använd följande alternativ för formatering, villkorsändring och infogning av egenskaper och variabler för formulärdatamodeller i texten:

   * [Formulärdatamodell](#formdatamodel)
   * [Variabel](#variables)
   * [Regelredigerare](#rules)
   * [Formateringsalternativ](#formatting)

      * [Kopiera och klistra in formaterad text från andra program](#paste)

      * [Markera delar av text](#highlight)

   * [Upprepa](/help/forms/using/cm-inline-condition.md)
   * [Specialtecken](#special)
   * [Söka och ersätta text](#searching)
   * [Kortkommandon](/help/forms/using/keyboard-shortcuts.md)

   >[!NOTE]
   >
   >Du kan lägga till element i formulärdatamodellen, element i dataordlistan och variabler med symbolen @ i textredigeraren. När du anger en sträng som föregås av @ i textredigeraren genomsöks alla datamodellselement, datamordlisteelement och variabler, och element eller variabler som innehåller den sökda strängen visas. Du kan navigera bland sökresultaten och välja ett element eller en variabel. Om det inte finns något matchande resultat visas meddelandet *Inga matchande resultat hittades*.

1. Välj **[!UICONTROL Save]**.

   Texten skapas. Nu kan du använda texten som byggsten när du skapar en interaktiv kommunikation.

## Redigera text {#edittext}

Du kan redigera ett befintligt textdokumentfragment genom att följa stegen nedan. Du kan också välja att redigera ett textdokumentfragment i en interaktiv kommunikationsredigerare.

1. Välj **[!UICONTROL Forms]** > **[!UICONTROL Document Fragments]**.
1. Navigera till ett textdokumentfragment och markera det.
1. Välj **[!UICONTROL Edit]**.
1. Gör de ändringar som krävs. Mer information om alternativ i text finns i [Skapa text](#createtext).
1. Välj **[!UICONTROL Save]** och sedan **[!UICONTROL Close]**.

## Anpassa ett textdokumentfragment med hjälp av egenskaper för formulärdatamodell {#formdatamodel}

Du kan anpassa textdokumentfragment genom att infoga egenskaper för formulärdatamodellen. Genom att infoga egenskaper för formulärdatamodellen i text kan du hämta och fylla i mottagarspecifika data från den associerade datakällan samtidigt som du förhandsgranskar en interaktiv kommunikation. Mer information om formulärdatamodell finns i [AEM Forms-dataintegrering](/help/forms/using/data-integration.md).

Om du har angett en formulärdatamodell när du skapar en text visas egenskaperna i formulärdatamodellen i den vänstra rutan i textredigeraren. Den angivna formulärdatamodellen ska vara densamma för textdokumentfragmentet och den interaktiva kommunikation som innehåller det.

![insertfdmelementtext](assets/insertfdmelementtext.png)

* Om du vill infoga en formulärdatamodellegenskap i text placerar du markören där du vill infoga egenskapen, markerar **[A]**-egenskapen i den vänstra rutan genom att trycka på den och väljer **[!UICONTROL [B] Add Selected]**. Du kan också bara dubbelmarkera egenskapen för att infoga den vid markörpositionen **[C]** . Egenskaper för formulärdatamodellen markeras med en bakgrundsfärg i webbläsaren.

Du kan också söka efter och lägga till en formulärdatamodellsegenskap med symbolen @ i textredigeraren. Placera markören där du vill infoga egenskapen. Skriv @ följt av söksträngen. Sökåtgärden utförs på alla formulärdatamodegenskaper och variabler som är tillgängliga i dokumentfragmentet. Egenskaperna eller variablerna som innehåller söksträngen hämtas och visas som en nedrullningsbar lista. Navigera genom sökresultaten och klicka på den egenskap som du vill infoga vid markörens plats. Tryck på Esc för att dölja sökresultaten.

* Om du vill att agenterna ska kunna redigera en formulärdatamodellegenskaps värde i agentgränssnittet när [Förbered och skicka interaktiv kommunikation](/help/forms/using/prepare-send-interactive-communication.md) med agentgränssnittet, markerar du **[D]**-låsikonen för den egenskapen och ser till att den är i olåst läge. Egenskapens standardläge är låst och en agent kan inte redigera egenskapen i agentens användargränssnitt.

Du kan också använda egenskaper för formulärdatamodell för att konstruera regler för att visa eller dölja delar av innehåll. Mer information finns i [Skapa regler i text](#rules).

## Skapa och använda variabler i ett textdokumentfragment {#variables}

Variabler är platshållare som kan bindas när du skapar en interaktiv kommunikation. Variabler kan bindas till en formulärdatamodell eller ett textfragment. Variabler kan också lämnas åt agenten för att fyllas.

Du kan använda variabler i stället för formulärdatamodellsegenskaper när:

* Ett textdokumentfragment ska användas i flera interaktiva kommunikationer där bindningen måste vara annorlunda för olika interaktiva kommunikationer.
* Textdokumentfragmentet har ingen formulärdatamodell när det skapas. Du kan infoga variabler och senare binda dem till egenskaperna för formulärdatamodellen när du skapar den interaktiva kommunikationen.
* Du måste binda och hämta text från ett textdokumentfragment. Endast de textdokumentfragment som kan bindas till variabler ska inte ha några variabler inuti.

När du skapar eller redigerar ett textdokumentfragment kan du skapa och infoga variabler. Variablerna som du skapar visas på fliken Data i agentens användargränssnitt. Agenten anger värdena för variablerna medan [Förbered och skicka interaktiv kommunikation med agentgränssnittet](/help/forms/using/prepare-send-interactive-communication.md).

### Skapa variabler {#createvariables}

1. Välj **[!UICONTROL Variables]** i den vänstra rutan.

   Rutan Variabler visas.

   ![variabelrutan](assets/variablespane.png)

1. Välj **[!UICONTROL Create]**.

   Rutan Skapa variabler visas.

1. Ange följande information och välj **[!UICONTROL Create]**:

   * **[!UICONTROL Name]** : Variabelns namn.
   * **[!UICONTROL Description]** : Om du vill kan du ange en beskrivning av variabeln.
   * **[!UICONTROL Type]** : Välj en typ av variabel: String, Number, Boolean eller Date.
   * **[!UICONTROL Allow Specific Values Only]** : För String- och Number-variabler kan du se till att agenten väljer från en viss uppsättning värden för en platshållare i agentens användargränssnitt. Om du vill ange värdeuppsättningen väljer du det här alternativet och anger sedan kommaavgränsade värden som tillåts i fältet **[!UICONTROL Values]**.

1. Välj **[!UICONTROL Create]**.

   Variabeln skapas och visas i variabelrutan.

1. Om du vill infoga en variabel i texten placerar du markören på lämplig plats, markerar variabeln och väljer **[!UICONTROL Add Selected]**.

   ![variableinsert](assets/variableinserted.png)

   Variabler markeras med ljusblå bakgrundsfärg, medan egenskaper för formulärdatamodell markeras med en bläddringsfärg.

   Du kan också söka efter och lägga till variabler med symbolen @ i textredigeraren. Placera markören där du vill infoga variabeln. Skriv @ följt av söksträngen. Sökåtgärden utförs på alla formulärdatamodegenskaper och variabler som är tillgängliga i dokumentfragmentet. Egenskaperna och variablerna som innehåller söksträngen hämtas och visas som en nedrullningsbar lista. Navigera genom sökresultaten och klicka på variabeln som du vill infoga vid markörens plats. Tryck på Esc för att dölja sökresultaten.

1. Välj **[!UICONTROL Save]**.

## Skapa regler i text {#rules}

Med hjälp av regelredigeraren i en text kan du skapa regler som visar eller döljer textsträngar eller innehållsdelar baserat på **förinställda villkor**. Dessa villkor kan utformas utifrån:

* Strängar
* Nummer
* Matematiskt uttryck
* Datum
* Egenskaper för associerad formulärdatamodell
* Alla variabler som du kan ha skapat i texten

### Skapa regler i text {#create-rules-in-text}

1. När du skapar eller redigerar en text markerar du den textsträng, det stycke eller det innehåll som du vill göra villkorsstyrd med hjälp av regeln.

   ![selectcontentapplyrule](assets/selectcontentapplyrule.png)

1. Välj **[!UICONTROL Create Rule]**.

   Dialogrutan Skapa regel visas. Förutom sträng, tal, matematiskt uttryck och datum finns följande även i regelredigeraren för att skapa satser för reglerna:

   * Egenskaper för associerad formulärdatamodell
   * Alla variabler som du kan ha skapat

   Välj lämpligt alternativ som ska utvärderas.

   ![ruleeditor](assets/ruleeditor.png) ![ruleeditorfdm](assets/ruleeditorfdm.png)

   >[!NOTE]
   >
   >Samlingsegenskapen stöds inte för att skapa regler för villkorlig formatering och visning av text.

1. Välj lämplig operator för att utvärdera regeln, till exempel Är lika med, Innehåller och Börjar med.

   ![ruleeditorfdm-1](assets/ruleeditorfdm-1.png)

1. Infoga det utvärderande uttrycket, värdet, datamodegenskapen eller variabeln.

   ![Regel som visar den markerade texten om mottagarens plats är USA enligt FDM-källdata](assets/ruleeditorfdm-1-1.png)

   Regel som visar den markerade texten om mottagarens plats är USA enligt FDM-källdata

   * När du skapar eller redigerar en regel kan du även välja ![icon_resize](assets/icon_resize.png) (Ändra storlek) för att utöka dialogrutan Skapa regel/Redigera regel. I den utökade dialogrutan med hela fönstret kan du dra och släppa egenskaper och variabler för formulärdatamodellen för att skapa regler. Välj Ändra storlek igen om du vill gå tillbaka till dialogrutan Skapa regel.
   * Du kan också skapa flera villkor i en regel.
   * Du kan också skapa överlappande regler, där en regel tillämpas på en del av ett innehåll som redan har en regel.

1. Välj **[!UICONTROL Done]**.

   Regeln tillämpas. Texten eller innehållet som regeln används på markeras med grönt. När du håller pekaren över markeringens vänstra handtag visas den tillämpade regeln.

   ![applicedruletext](assets/appliedruletext.png)

   När du klickar på den vänstra handtaget för den tillämpade regeln får du möjlighet att redigera eller ta bort regeln.

## Formatera text {#formatting}

När du skapar eller redigerar text ändras verktygsfältet beroende på vilken typ av redigeringar du väljer att göra: Stycke, Justering eller Lista:

![Välj typ av verktygsfält](do-not-localize/toolbarselection.png)

Välj typ av verktygsfält: Stycke, Justering eller Lista

![Verktygsfältet Teckensnittsredigering](do-not-localize/paragraphtoolbar.png)

Verktygsfältet Teckensnittsredigering

![Justeringsverktygsfältet](assets/alignmenttoolbar.png)

Justering (verktygsfält)

![Verktygsfältet Lista](do-not-localize/listingtoolbar.png)

Verktygsfältet Lista

### Markera/framhäv delar av text {#highlight}

Markera texten och välj Markeringsfärg om du vill framhäva delar av text i ett redigerbart dokumentfragment.

![textbakgrundsgrundfärgtillämpad-1](assets/textbackgroundcolorapplied-1.png)

Du kan antingen direkt välja en grundfärg `**[A]**` som finns på paletten Grundfärger eller välja **Markera** efter att du har använt reglaget `**[B]**` för att välja rätt färgton.

Du kan också gå till fliken Avancerat och välja lämplig nyans, ljushet och mättnad `**[C]**` för att skapa den exakta färgen och sedan välja Välj `**[D]**` för att använda färgen för att markera texten.

![textbackgroundColor-2](assets/textbackgroundcolor-2.png)

### Klistra in formaterad text {#paste}

Om du vill återanvända ett av flera textstycken som finns i ett annat program, till exempel från Microsoft® Word- eller HTML-sidor, kopierar och klistrar du in texten i textredigeraren. Formateringen av den kopierade texten behålls i textredigeraren.

Du kan kopiera och klistra in ett eller flera textstycken i ett redigerbart textdokumentfragment. Du kan t.ex. ha ett Microsoft® Word-dokument med en punktlista över godkända intyg för uppehälle, som följande:

![pastetextmsword-2](assets/pastetextmsword-2.png)

Du kan kopiera och klistra in text direkt från Microsoft® Word-dokumentet i ett redigerbart textdokumentfragment. Formateringen som punktlistor, teckensnitt och textfärger behålls i textdokumentfragmentet.

![pastetexteditablemodul-1](assets/pastetexteditablemodule-1.png)

## Infoga specialtecken i text {#special}

Infoga vid behov specialtecken i dokumentfragmentet. Du kan till exempel använda paletten Specialtecken för att infoga:

* Valutasymboler som €,¥ och £
* Matematiska symboler som t.ex.¥, Ð och ^
* Interpunktionssymboler som ‟ och &quot;

![specialtecken-2](assets/specialcharacters-2.png)

Textredigeraren har inbyggt stöd för 210 specialtecken. Administratören kan [lägga till stöd för fler/anpassade specialtecken genom anpassning](/help/forms/using/custom-special-characters.md).

## Söka och ersätta text {#searching}

När du arbetar med textdokumentfragment som innehåller en stor mängd text måste du söka efter en viss textsträng. Du kan också behöva ersätta en viss textsträng med en alternativ sträng.

Med funktionen Sök och ersätt kan du söka efter (och ersätta) valfri textsträng i ett textdokumentfragment. Funktionen innehåller även en kraftfull sökning efter reguljära uttryck.

1. Öppna ett textdokumentfragment för [redigering](#edittext).
1. Välj **[!UICONTROL Find & Replace]**.

1. Ange texten som ska sökas i textrutan **[!UICONTROL Find]** och den nya texten (ersättningstexten) i textrutan **[!UICONTROL Replace]** och markera **[!UICONTROL Replace]**.

1. Om den sökta texten hittas ersätts texten med ersättningstexten.

   * Om en annan förekomst av söktexten hittas markeras den förekomsten i textdokumentfragmentet. Om du väljer **[!UICONTROL Replace]** igen ersätts den markerade instansen och markören flyttas framåt, om en tredje instans hittas.
   * Om ingen annan instans hittas visas ett meddelande i dialogrutan Sök och ersätt: Slutet av modulen har nåtts.

   Du kan också välja Ersätt alla om du vill ersätta alla matchningar på en gång.

   Sök och ersätt innehåller även en kraftfull sökning efter reguljära uttryck. Om du vill använda regex i sökningen väljer du **[!UICONTROL Reg ex]** och sedan **[!UICONTROL Find]** eller **[!UICONTROL Replace]**.
