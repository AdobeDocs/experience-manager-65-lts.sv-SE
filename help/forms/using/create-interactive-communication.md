---
title: Skapa en interaktiv kommunikation
description: Skapa en interaktiv kommunikation med redigeraren för interaktiv kommunikation. Använd dra-och-släpp-funktionen för att bygga den interaktiva kommunikationen och förhandsgranska både utskrift och webb på olika enhetstyper.
topic-tags: interactive-communications
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '5956'
ht-degree: 0%

---

# Skapa en interaktiv kommunikation{#create-an-interactive-communication}

## Ökning {#overview}

Interactive Communications centraliserar och hanterar framtagning, sammanställning och leverans av personaliserade och interaktiva korrespondenser. Använd utskrift som huvudkanal för webben och minimera arbetet med att duplicera webbutdata i den interaktiva kommunikationen.

### Förutsättningar {#prerequisites}

Nedan följer några förutsättningar för att skapa en interaktiv kommunikation:

* Konfigurera en [formulärdatamodell](/help/forms/using/data-integration.md) som innehåller testdata eller med en faktisk datakälla, till exempel en instans av Microsoft® Dynamics.
* Kontrollera att du har [dokumentfragmenten](/help/forms/using/document-fragments.md).
* Kontrollera att du har [Mallar för utskrift och webbkanal](/help/forms/using/web-channel-print-channel.md).
* Kontrollera att du har det [tema](/help/forms/using/themes.md) som krävs för webbkanalen.

## Skapa interaktiv kommunikation {#createic}

1. Logga in på AEM författarinstans och navigera till **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Forms]** > **[!UICONTROL Forms & Documents]**.
1. Välj **[!UICONTROL Create]** och välj **[!UICONTROL Interactive Communication]**. Sidan Skapa interaktiv kommunikation visas.

   ![create-interactive-communication](assets/create-interactive-communication.png)

1. Ange följande information. :

   * **[!UICONTROL Title]**: Ange titeln för den interaktiva kommunikationen.
   * **[!UICONTROL Name]**: Namnet på den interaktiva kommunikationen hämtas från den titel du anger. Redigera den om det behövs.
   * **[!UICONTROL Description]**: Ange en beskrivning av interaktiv kommunikation.
   * **[!UICONTROL Form Data Model]**: Bläddra och välj formulärdatamodellen. Mer information om formulärdatamodell finns i [AEM Forms-dataintegrering](/help/forms/using/data-integration.md).

   * **[!UICONTROL Prefill Service]**: Välj förifyllningstjänsten för att hämta data och fylla i interaktiv kommunikation i förväg.
   * **[!UICONTROL Post Process Type]**: Du kan välja ett AEM- eller Forms-arbetsflöde som ska aktiveras när den interaktiva kommunikationen skickas. Välj vilken typ av arbetsflöde som ska utlösas.

   * **[!UICONTROL Post Process]**: Välj namnet på arbetsflödet som ska utlösas. När du väljer AEM-arbetsflöde anger du bilagesökväg, Layoutsökväg, PDF-sökväg, Skriv ut datasökväg och Webbdatasökväg.
   * **[!UICONTROL Tags]**: Välj de taggar som ska användas i den interaktiva kommunikationen. Du kan också skriva in ett nytt/anpassat taggnamn och trycka på Retur för att skapa det.
   * **[!UICONTROL Author]**:Författarnamnet hämtas automatiskt från den inloggade användarens användarnamn.
   * **[!UICONTROL Publish Date:]** Ange det datum då den interaktiva kommunikationen ska publiceras.
   * **[!UICONTROL Unpublish Date]**: Ange det datum då den interaktiva kommunikationen ska avpubliceras.

1. Välj **[!UICONTROL Next]**. Skärmen där du anger utskrifts- och webbkanalsinformation visas.
1. Ange följande:

   * **[!UICONTROL Print]**: Välj det här alternativet om du vill generera tryckkanalen för den interaktiva kommunikationen.
   * **[!UICONTROL Print Template]**: Bläddra och välj en XDP-fil som utskriftsmall.
   * **[!UICONTROL Web]**: Välj det här alternativet om du vill generera webbkanalen eller responsiva utdata för interaktiv kommunikation.
   * **[!UICONTROL Interactive Communication Web Template]**: Bläddra och välj webbmallen.
   * **[!UICONTROL Theme]** och **[!UICONTROL Select Theme]**: Bläddra och välj det tema som ska användas för att utforma webbkanalen i den interaktiva kommunikationen. Mer information finns i [Teman i AEM Forms](/help/forms/using/themes.md).

   * **[!UICONTROL Use Print As Master for Web Channel]**: Välj det här alternativet om du vill skapa webbkanalen synkroniserat med utskriftskanalen. Om du använder utskriftskanalen som huvudkanal för webbkanalen kan du kontrollera att innehållet och databindningen i webbkanalen hämtas från utskriftskanalen och att ändringarna som görs i utskriftskanalen återspeglas i webbkanalen när du väljer Synkronisera. Författarna kan dock bryta arvet för specifika komponenter i webbkanalen efter behov. Mer information finns i [Synkronisera webbkanal med skrivarkanal](../../forms/using/create-interactive-communication.md#synchronize).
Om du väljer alternativet **[!UICONTROL Use Print As Master for Web Channel]** kan du välja något av följande lägen för att generera webbkanalen:

      * **[!UICONTROL Auto layout]**: Välj det här läget om du automatiskt vill generera platshållare, innehåll och databindning för webbkanalen från Utskriftskanalen.
      * **[!UICONTROL Manually organize]**: Välj det här läget om du vill markera och lägga till Print channel-element manuellt i webbkanalen med huvudinnehållet som är tillgängligt på fliken **[!UICONTROL Data Sources]**. Mer information finns i [Markera Skriv ut kanalelement för att skapa webbkanalsinnehåll](#selectprintchannelelements).

   Mer information om utskriftskanaler och webbkanaler finns i [Skriva ut kanal och webbkanal](/help/forms/using/web-channel-print-channel.md).

1. Välj **[!UICONTROL Create]**. Interaktiv kommunikation skapas och en varningsruta visas. Välj **[!UICONTROL Edit]** om du vill börja skapa innehållet i den interaktiva kommunikationen enligt beskrivningen i [Lägg till innehåll med hjälp av gränssnittet för redigering av interaktiv kommunikation](#step2). Du kan också markera **[!UICONTROL Done]** och välja att redigera den interaktiva kommunikationen senare.

## Lägga till innehåll i interaktiv kommunikation {#step2}

När du har skapat en interaktiv kommunikation kan du använda redigeringsgränssnittet för interaktiv kommunikation för att skapa dess innehåll.

Mer information om gränssnittet för utveckling av interaktiv kommunikation finns i [Introduktion till redigering av interaktiv kommunikation](/help/forms/using/introduction-interactive-communication-authoring.md).

1. Utvecklingsgränssnittet för interaktiv kommunikation startas när du väljer Redigera enligt [Skapa interaktiv kommunikation](#createic). Du kan också navigera till en befintlig Interactive Communication-resurs på AEM, markera den och välja **[!UICONTROL Edit]** för att starta redigeringsgränssnittet för interaktiv kommunikation.

   Som standard visas den tryckta kanalen i den interaktiva kommunikationen, om inte den interaktiva kommunikationen bara är för webbkanaler. I utskriftskanalen i den interaktiva kommunikationen visas målområdena, som de är tillgängliga i den valda XDP/utskriftskanalmallen. I dessa målområden och fält kan du lägga till komponenter eller resurser.

1. Markera utskriftskanalen och välj fliken **[!UICONTROL Components]**. Följande komponenter är tillgängliga i utskriftskanalen:

   | **Komponent** | **Funktionalitet** |
   |---|---|
   | Diagram | Lägger till ett diagram som du kan använda i interaktiv kommunikation för visuell representation av tvådimensionella data som hämtats från en formulärdatamodellsamling. Mer information finns i [Använda diagram i interaktiv kommunikation](/help/forms/using/chart-component-interactive-communications.md). |
   | Dokumentfragment | Gör att du kan lägga till en återanvändbar komponent, t.ex. text, lista eller villkor, i en interaktiv kommunikation. Komponenten som läggs till kan antingen vara modellbaserad för formulärdata eller utan en formulärdatamodell. |
   | Bild | Infoga en bild. |

   Dra och släpp komponenterna i din interaktiva kommunikation och konfigurera dem efter behov.

   Du kan också använda åtgärderna ångra och gör om när du skapar en interaktiv kommunikation för både utskrifts- och webbkanaler.

   Använd ångra-åtgärden för att ta bort den senast utförda åtgärden och gör om-åtgärden för att ta med den borttagna åtgärden igen. Om du t.ex. har infogat en bild eller skapat en databindning i ett interaktivt meddelande och behöver ta bort den ska du använda åtgärden ångra.

   ![Ångra Gör om åtgärder](assets/undo_redo_actions_new.png)

   Alternativen Ångra och Gör om visas i verktygsfältet för redigeringsgränssnittets sida. Alternativet Ångra visas bara efter att en åtgärd har utförts. Alternativet gör om visas bara i verktygsfältet på sidan när du har utfört en ångra-åtgärd. Dessa åtgärder återställs när sidan uppdateras.

1. När utskriftskanalen är markerad går du till fliken **[!UICONTROL Assets]** och använder filtret för att bara visa de resurser som du vill se.

   Med Assets webbläsare kan du också dra och släppa resurser direkt till målområdena för interaktiv kommunikation.

   ![assets-docfragments](assets/assets-docfragments.png)

1. Dra och släpp dokumentfragmenten i interaktiv kommunikation. Här följer de typer av dokumentfragment som du kan använda i tryckkanalen i den interaktiva kommunikationen.

<table>
 <tbody>
  <tr>
   <td><strong>Dokumentfragmenttyp</strong></td>
   <td><strong>Exempelsyfte</strong></td>
  </tr>
  <tr>
   <td><a href="/help/forms/using/texts-interactive-communications.md" target="_blank">Text</a></td>
   <td>Text för att lägga till adress, mottagarens e-postadress och brödtext i brevet </td>
  </tr>
  <tr>
   <td><a href="/help/forms/using/conditions-interactive-communications.md" target="_blank">Villkor</a></td>
   <td>Villkor för att lägga till lämplig rubrikbild i kommunikationen baserat på typen av princip: Standard eller Premium. <br /> </td>
  </tr>
  <tr>
   <td>Lista</td>
   <td>Grupp med dokumentfragment, inklusive text, villkor, andra listor och bilder. <br /> </td>
  </tr>
 </tbody>
</table>

Du kan också ersätta bindningen mellan ett målområde och ett dokumentfragment genom att släppa det nya fragmentet på målområdet med fliken **[!UICONTROL Assets]**. Målområdets blå färgskuggning när fragmentet dras anger att dokumentfragmentet kan släppas till målområdet.

Mer information om dokumentfragment finns i [Dokumentfragment](/help/forms/using/document-fragments.md).

Med hjälp av redigeringsgränssnittet kan du skilja mellan obundna och bundna fält och variabler i en interaktiv kommunikation. Gränssnittet markerar de obundna fälten och variablerna med en orange ram.

![unbound_fields_variables_highlights_dc](assets/unbound_fields_variables_highlights_dc.jpg)

När du håller muspekaren över dessa element visas dessutom ett verktygstips med meddelandet Fält (obundet) eller Variabel (obundet).

En obunden variabel som används i ett dokumentfragment kanske inte visas i redigeringsgränssnittet. Det kan inträffa på grund av en textbunden regel i ett dokumentfragment eller om det finns ett villkorsfragment. I sådana fall visas ett verktygstips, som är markerat med blått, som en del av dokumentfragmentet. Verktygstipset visar antalet obundna variabler som används i ett dokumentfragment.

![Obunden variabel](assets/df_unbound_variable_new.png)

Markera dokumentfragmentet, välj ![configure_icon](assets/configure_icon.png) (Configure) och välj sedan **[!UICONTROL Properties]** i sidoknappen i den interaktiva kommunikationen. I avsnittet **[!UICONTROL Variables and Data Model Objects]** visas variablerna, inklusive de dolda variablerna och datamodellsobjekten som används i dokumentfragmenten. Använd ikonen ![edit](assets/edit.svg) (Edit) bredvid varje datamodellsobjekt eller variabel för att redigera egenskaperna.

1. Om du vill ställa in bindning för variabler markerar du en variabel och väljer ![configure_icon](assets/configure_icon.png) (Configure). Sedan ställer du in bindningsegenskaperna på egenskapspanelen i sidlisten.

   * **Ingen**: Agenten fyller i värdet för variabeln.
   * **Textfragment**: Om du väljer det här alternativet kan du bläddra och markera ett textdokumentfragment vars innehåll återges i fältet. Endast textdokumentfragment kan bindas till variabler som inte har några variabler inuti.
   * **Datamodellobjekt**: Välj en formulärdatamodellegenskap vars värde fylls i i fältet.
   * **Standardvärde:** Du kan definiera ett standardvärde för variabeln med det här fältet. Värdet visas när du förhandsgranskar den interaktiva kommunikationen eller i agentgränssnittet.
   * **Visningsmönster:** Du kan också definiera ett visningsformat för en variabel. Välj något av de fördefinierade alternativen i listrutan **Typ** om du vill använda ett visningsformat för en variabel. Välj **Egen** om du vill definiera ett visningsmönster som inte är tillgängligt i listan. Mer information finns i [Mönster för datavisning](../../forms/using/create-interactive-communication.md#datadisplaypatterns).

   Navigera till [Variabler och datamodellobjekt](../../forms/using/create-interactive-communication.md#hiddenvariables) för att ställa in bindning för dolda variabler i dokumentfragmentet.

   Du kan också dra och släppa datakällelement eller textdokumentfragment för att ställa in bindning av variabler.  Om du vill skapa en bindning med något av datakällelementen väljer du fliken **Datakällor** och drar och släpper elementet till variabelnamnet. Datakällelementet och variabeln måste vara av samma typ för att bindningen ska kunna konfigureras korrekt. Om du drar och släpper ett datakällelement till en redan bunden variabel, ersätter det nya elementet det föregående och skapar en bindning med variabeln. På samma sätt väljer du fliken **Assets** och drar och släpper textdokumentfragmentet till variabelnamnet för att ange bindningen mellan dem. Textdokumentfragmentet får inte innehålla några variabler.

1. Om du vill lägga till en tabell med utskriftskanalen markerad använder du filtret på fliken **[!UICONTROL Assets]** för att bara visa Layoutfragment. Dra och släpp önskat layoutfragment till Interactive Communication. Ett layoutfragment är baserat på en XDP och kan användas för att skapa grafiska layouter eller statiska och dynamiska tabeller i interaktiv kommunikation som fylls i med dynamiska data.

   Exempel: En layouttabell som visar bruttopremie, förmånsersättning % och tillgänglighet för assistans vid nödsituationer för gamla och nya policyer.

   Mer information om layoutfragment finns i [Dokumentfragment](/help/forms/using/document-fragments.md).

1. När utskriftskanalen är markerad använder du filtret på fliken **[!UICONTROL Assets]** för att visa bilder. Dra-och-släpp de bilder som behövs till Interactive Communication, t.ex. företagslogotyp.

   Hantera dessutom följande i den interaktiva kommunikationen:

   * [Lägga till och konfigurera diagram](/help/forms/using/chart-component-interactive-communications.md)
   * [Synkronisera webbkanalen med utskriftskanalen](../../forms/using/create-interactive-communication.md#synchronize)

      * Automatisk synkronisering
      * Avbryt arv
      * Återaktivera arv
      * Synkronisera

   * [Bifogade filer och biblioteksåtkomst](../../forms/using/create-interactive-communication.md#attachmentslibrary)
   * [Egenskaper för XDP-/layoutfält](../../forms/using/create-interactive-communication.md#xdplayoutfieldproperties)
   * [Lägga till regler i komponenter](../../forms/using/create-interactive-communication.md#rules)

1. Växla till **[!UICONTROL Web Channel]**. Webbkanalen visas i redigeraren för interaktiv kommunikation. När du byter från Print-kanalen till Web channel för första gången sker den automatiska synkroniseringen. Mer information finns i [Synkronisera webbkanal från utskriftskanalen](../../forms/using/create-interactive-communication.md#synchronize).

   Eftersom vi använder Skriv ut som master för webben i det här exemplet synkroniseras platshållarna för utskriftskanalen, innehållet och databindningen till webbkanalen. Du kan dock ändra och anpassa det specifika innehållet i webbkanalen. [Avbryt arv](#cancelinheritance) för målområdena och variablerna som har genererats med utskriftskanalen för att kunna anpassa innehållet.

   ![webchannelAssets](assets/webchannelassets.png)

   Markera dokumentfragmentet, välj ![configure_icon](assets/configure_icon.png) (Configure) och välj sedan **[!UICONTROL Properties]** i sidoknappen i den interaktiva kommunikationen. I avsnittet **[!UICONTROL Variables and Data Model Objects]** visas variablerna, inklusive de dolda variablerna och datamodellsobjekten som används i dokumentfragmenten. Använd ikonen ![edit](assets/edit.svg) (Edit) bredvid varje datamodellsobjekt eller variabel för att redigera egenskaperna. Dessutom, för dokumentfragment som har [genererats automatiskt](#synchronize) i webbkanalen med hjälp av skrivarkanalen, använder du ikonen ![cancellarv](assets/cancelinheritance.png) (avbryt arv) bredvid varje datamodellsobjekt och variabel för att [avbryta arv](#cancelinheritance) och för att kunna redigera dem.

1. Om du vill lägga till ytterligare komponenter i webbkanalen markerar du **[!UICONTROL Components]** med webbkanalen markerad. Dra och släpp komponenter i webbkanalen i din interaktiva kommunikation efter behov och fortsätt att konfigurera dem.

   | Komponenter | Funktionalitet |
   |---|---|
   | Diagram | Lägger till ett diagram som du kan använda i interaktiv kommunikation för visuell representation av tvådimensionella data som hämtats från en formulärdatamodellsamling. Mer information finns i [Använda diagramkomponent](../../forms/using/chart-component-interactive-communications.md). |
   | Dokumentfragment | Gör att du kan lägga till en återanvändbar komponent, text, lista eller villkor i en interaktiv kommunikation. Den återanvändbara komponenten som du lägger till i en interaktiv kommunikation kan antingen vara modellbaserad i form av formulärdata eller utan någon formulärdatamodell. |
   | Bild | Infoga en bild. |
   | Panel | Gör att du kan lägga till en [panel](../../forms/using/create-interactive-communication.md#add-panel-component-to-the-web-channel) i den interaktiva kommunikationen. |
   | Tabell | Lägger till en tabell där du kan ordna data i rader och kolumner. |
   | Målområde | Infogar ett målområde i en webbkanal för att ordna de webbkanalsspecifika komponenterna. Målområdet är en ren behållare som gör att du kan gruppera webbkanalsspecifika komponenter. |
   | Text | Lägger till RTF i webbkanalen i en interaktiv kommunikation. Text kan också använda formulärdatamodellsobjekt för att göra innehållet dynamiskt. |
   | Knapp | Gör att du kan lägga till en [knapp](../../forms/using/create-interactive-communication.md#add-button-component-to-the-web-channel) i den interaktiva kommunikationen. Du kan använda komponenten Button för att navigera till annan interaktiv kommunikation, adaptiva formulär, andra resurser som bilder eller dokumentfragment eller en extern URL. |
   | Avgränsare | Gör att du kan infoga en vågrät linje i en interaktiv kommunikation. Använd den här komponenten för att skilja mellan avsnitt i en korrespondens. Du kan till exempel använda avgränsningskomponenten för att skilja mellan kundinformation och kreditkortsinformation i en kreditkortsutdrag. |

1. Infoga resurser i webbkanalen efter behov.

   Du kan [förhandsgranska din interaktiva kommunikation](#previewic) om du vill se hur utskrifts- och webbutdata för den interaktiva kommunikationen ser ut och fortsätta att göra ändringar efter behov.

## Förhandsgranska interaktiv kommunikation {#previewic}

Du kan använda alternativet **Förhandsgranska** för att utvärdera utseendet på den interaktiva kommunikationen. Webbkanalen i Interactive Communication erbjuder också ett alternativ för att emulera upplevelsen av en interaktiv kommunikation för olika enheter. Exempel: iPhone, iPad och Desktop. Du kan använda både alternativen **Förhandsgranska** och **Emulator** ![linjal](assets/ruler.png) tillsammans för att förhandsgranska webbutdata för enheter med olika skärmstorlekar. Exempeldata i förhandsgranskningen fylls i från den angivna formulärdatamodellen.

1. Markera kanalen (tryck eller webb) för att förhandsgranska och välja förhandsvisning. Interaktiv kommunikation visas.

   >[!NOTE]
   >
   >Förhandsgranskningen fylls i med den angivna formulärdatamodellens exempeldata. Mer information om hur du förhandsgranskar interaktiv kommunikation med vissa andra data eller använder förifyllningstjänsten finns i [Använd formulärdatamodell](/help/forms/using/using-form-data-model.md) och [Arbeta med formulärdatamodell](/help/forms/using/work-with-form-data-model.md).

1. För webbkanalen använder du ![linjal](assets/ruler.png) för att visa hur den interaktiva kommunikationen ser ut på olika enheter.

   ![webchannelPreview](assets/webchannelpreview.png)

Dessutom kan du [Förbered och skicka interaktiv kommunikation med agentgränssnittet](/help/forms/using/prepare-send-interactive-communication.md).

## Konfigurera egenskaper i interaktiv kommunikation  {#configure-properties-in-interactive-communication}

### Bifogade filer och biblioteksåtkomst {#attachmentslibrary}

I utskriftskanalen kan du konfigurera bilagor och biblioteksåtkomst så att agenten kan hantera bilagor i agentgränssnittet för den interaktiva kommunikationen:

1. Markera dokumentbehållaren i utskriftskanalen och välj **Egenskaper**.

   ![documentsContainerProperties](assets/documentcontainerproperties.png)

   Panelen Egenskaper visas i sidofältet.

   ![egenskaperbilagor](assets/propertiesattachments.png)

1. Expandera **Bifogade filer** och ange följande egenskaper:

   * **[!UICONTROL Allow Library Access]**: Välj det här alternativet om du vill aktivera biblioteksåtkomst för agenten i agentgränssnittet. Om det här alternativet är aktiverat kan agenten lägga till filer från biblioteket när den interaktiva kommunikationen förbereds.
   * **[!UICONTROL Allow Re-Ordering Of Attachments]**: Välj det här alternativet om du vill att agenten ska kunna ordna om de bifogade filerna med den interaktiva kommunikationen.
   * **[!UICONTROL Max Number Of Attachments Allowed]**: Ange det maximala antalet bilagor som tillåts med den interaktiva kommunikationen.
   * **[!UICONTROL Files To Be Attached]**: Välj **[!UICONTROL Add]** och bläddra till de filer som ska bifogas och ange följande:

      * **[!UICONTROL Attach This File To Document By Default]**: Du kan ändra det här alternativet om bara bilagan inte är obligatorisk.
      * Agenten **[!UICONTROL Mandatory:]** kan inte ta bort den bifogade filen i agentens användargränssnitt.

   ![bifogade filer](assets/attachfiles.png)

1. Välj **[!UICONTROL Done]**.

### Egenskaper för XDP-/layoutfält {#xdplayoutfieldproperties}

1. Håll muspekaren över ett fält som är inbyggt i kanalmallen för utskrift när du redigerar Print-kanalen i en interaktiv kommunikation och välj ![configure_icon](assets/configure_icon.png) (Konfigurera).

   Dialogrutan Egenskaper visas i sidlisten.

   ![data_display_patterns_fields](assets/data_display_patterns_fields.jpg)

1. Ange följande:

   * **[!UICONTROL Name]**: JCR-nodnamn.
   * **[!UICONTROL Title]**: Ange en titel som ska vara synlig för agenten i agentgränssnittet och i dokumentbehållarträdet.
   * **[!UICONTROL Binding Type]**: Välj en av följande bindningstyper för fältet.

      * Ingen: Agenten fyller i egenskapens värde.
      * Textfragment: Om du väljer det här alternativet kan du bläddra och markera ett textdokumentfragment vars innehåll återges i fältet. Du kan också dra och släppa textdokumentfragmentet till fältnamnet för att ange bindningen mellan dem. Textdokumentfragmentet får inte innehålla några variabler.
      * Datamodellobjekt: Välj en formulärdatamodellegenskap vars värde fylls i i fältet. Du kan också välja fliken **Datakällor** och dra och släppa egenskapen i fältet.

   * **[!UICONTROL Default Values]**: Med standardvärdet säkerställs att fältet inte är tomt när det inte finns något värde från det angivna datamodellsobjektet eller textfragmentet. Om databindningstypen inte är någon fylls standardvärdet i i förväg i fältet.
   * **[!UICONTROL Display Pattern]**: Du kan också definiera ett visningsformat för ett fält. Välj något av de fördefinierade alternativen i listrutan **Typ** om du vill använda ett visningsformat för ett fält. Välj **Egen** om du vill definiera ett visningsmönster som inte är tillgängligt i listan. Mer information finns i [Datavisningsmönster](../../forms/using/create-interactive-communication.md#datadisplaypatterns)

   * **[!UICONTROL Editable By Agent]**: Välj att tillåta agenten att redigera värdet i fältet i agentens användargränssnitt. Den här inställningen gäller inte om bindningstypen är Textfragment.
   * **[!UICONTROL Label]**: Ange en textsträng som visas med fältet till agenten i agentgränssnittet. Den här inställningen gäller inte om bindningstypen är Textfragment.
   * **[!UICONTROL Tooltip]**: Ange en textsträng som ska visas när muspekaren förs över agenten i agentgränssnittet. Den här inställningen gäller inte om bindningstypen är Textfragment.
   * **[!UICONTROL Required]**: Välj att göra fältet obligatoriskt för agenten. Den här inställningen gäller inte om bindningstypen är Textfragment.
   * **[!UICONTROL Allow multiple lines]**: Markera det här fältet om du vill tillåta flera textrader som inmatning i fältet. Den här inställningen gäller inte om bindningstypen är Textfragment.

1. Välj ![made_icon](assets/done_icon.png).

### Visningsmönster {#datadisplaypatterns}

Med hjälp av redigeringsgränssnittet kan du definiera datavisningsmönster för fält, variabler och formulärdatamodellelement som är tillgängliga när du skapar en interaktiv kommunikation för tryck- och webbkanaler.

Om du vill konfigurera datavisningsmönstret markerar du elementet, väljer ![configure_icon](assets/configure_icon.png) (Configure) och anger visningsmönstret på panelen **[!UICONTROL Properties]** i sidlisten. Välj ett fördefinierat alternativ i listrutan **[!UICONTROL Type]** för att visa mönstret som är associerat med den valda typen. Välj **[!UICONTROL Custom]** i listrutan **[!UICONTROL Type]** för att definiera ett mönster som inte är tillgängligt i listan. När du redigerar värden i fältet **[!UICONTROL Pattern]** ändras typen automatiskt till **[!UICONTROL Custom]**.

Om du vill använda visningsmönstret måste antalet tecken eller siffror som definieras i fältet Mönster matcha eller överskrida de tecken eller siffror som definieras i värdet för fält, variabler och formulärdatamodellelement. Mer information finns i [exempel](../../forms/using/create-interactive-communication.md#greaternumberofdigits).

![data_display_pattern_example](assets/data_display_patterns_ssn_new.png)

Du kan omdefiniera visningsmönstret för ett fält, en variabel eller ett element i en formulärdatamodell när du har genererat webbinnehåll från utskriftskanalen. Därför kan ett element ha olika visningsmönster definierade för utskrifts- och webbkanaler. Om du inte definierar ett visningsmönster för ett element i en utskriftskanal och automatiskt genererar webbinnehåll med hjälp av en utskriftskanal, definierar databindningen som är definierad för elementet i utskriftskanalen de visningsmönsteralternativ som finns i listrutan **[!UICONTROL Type]**. Om ingen bindning har definierats för elementet definierar elementets datatyp de tillgängliga alternativen för visningsmönster. Om du till exempel skapar en databindning av typen Number för ett element i en utskriftskanal, har visningsmönsteralternativen i den nedrullningsbara listan **[!UICONTROL Type]** olika format typen Number.

Växla till **förhandsgranskningsläget** eller öppna agentens användargränssnitt för att visa det visningsmönster som används för dessa element.

I följande tabell visas ett exempel på de värden som visas som ett resultat av inställningen av datavisningsmönstret för en variabel:

| Typ | Standardvärde | Visningsmönster | Visningsvärde | Beskrivning |
|---|---|---|---|---|
| SocialSecurityNumber | 123456789 | text{999-99-9999} | 123-45-6789 | Antalet siffror i standardvärdefältet matchar antalet siffror i mönsterfältet. Värdet baserat på mönstret visas. |
| SocialSecurityNumber | 1234567 | text{999-99-9999} | 1-23-4567 | Antalet siffror i standardvärdefältet är mindre än antalet siffror i mönsterfältet. Mönstret används på de 7 tillgängliga siffrorna. |
| SocialSecurityNumber | 1234567890 | text{999-99-9999} | 1234567890 | Antalet siffror i standardvärdefältet är större än antalet siffror i mönsterfältet. Därför ändras inte visningsvärdet. |

Om inget visningsmönster anges för en variabel eller ett formulärdatamodellselement används [det globala dokumentfragmentets konfiguration](https://helpx.adobe.com//experience-manager/6-5/forms/using/interactive-communication-configuration-properties.html) som standard.

Om du inte använder ett visningsmönster för en variabel med taldatatyp, visas mönstret i förhandsvisningen enligt den globala dokumentfragmentkonfigurationen. Om du tillämpar ändringar i standardkonfigurationen för globala dokumentfragment, visar agentgränssnittet fortfarande mönstret enligt standardavgränsarna som är definierade för språkområdet.

Om inget visningsmönster anges för fält används på samma sätt det mönster som definierades när utskriftsmallen (XDP) skapades för fältet. Om det inte finns något mönster när du skapar utskriftsmallen används standardmönstren som baseras på XFA-specifikationerna i fälten.

Om det angivna visningsmönstret är felaktigt eller inte kan användas, används dessutom standardmönstren som baseras på XFA-specifikationerna för fälten, variablerna eller elementen i formulärdatamodellen.

## Tillämpa regler på komponenter för interaktiv kommunikation {#rules}

Om du vill villkoralisera komponenter eller innehåll i den interaktiva kommunikationen markerar du komponenten/delen av innehållet och väljer ![createruleicon](assets/createruleicon.png) (Skapa regel) för att starta regelredigeraren.

Mer information finns i:

* [Regelredigeraren](/help/forms/using/rule-editor.md)
* [Introduktion till utveckling av interaktiv kommunikation](/help/forms/using/introduction-interactive-communication-authoring.md)

## Använda tabeller {#tables}

### Dynamiska tabeller i interaktiv kommunikation {#dynamic-tables-in-interactive-communication}

Du kan lägga till dynamiska tabeller i interaktiv kommunikation med hjälp av layoutfragment. I följande steg används ett exempel på en kreditkortssats för att illustrera hur ett layoutfragment används för att skapa en dynamisk tabell i ett interaktivt meddelande.

1. Kontrollera att det layoutfragment som krävs för att skapa tabellen är tillgängligt i AEM.
1. Dra och släpp ett layoutfragment (med en tabell med flera kolumner) i ett målområde från resursläsaren i utskriftskanalen i din interaktiva kommunikation.

   ![lf_dragdrop](assets/lf_dragdrop.png)

   En tabell visas i layoutområdet Interaktiv kommunikation.

   ![lf_dragdrop_table](assets/lf_dragdrop_table.png)

1. Ange databindning för alla celler i tabellen. Om du vill skapa en repeterbar rad infogar du egenskaper för formulärdatamodell i raden som tillhör en gemensam samlingsegenskap.

   1. Markera en cell i tabellen och välj ![configure_icon](assets/configure_icon.png) (Konfigurera).

      Dialogrutan Egenskaper visas i sidlisten.

      ![lf_cell_properties](assets/lf_cell_properties.png)

   1. Konfigurera egenskaperna:

      * **[!UICONTROL Name]**: JCR-nodnamn.
      * **[!UICONTROL Title]**: Ange en titel som ska visas i redigeraren för interaktiv kommunikation.
      * **[!UICONTROL Binding Type]**: Välj en av följande bindningstyper för fältet.

         * **[!UICONTROL None]**
         * **[!UICONTROL Data model object]**: En formulärdatamodellegenskaps värde fylls i i fältet. Du kan också välja fliken **Datakällor** och dra och släppa egenskapen i fältet.

      * **[!UICONTROL Data Model Object]**: Den formulärdatamodellsegenskap vars värde är ifyllt i fältet.
      * **[!UICONTROL Default Value]**: Med standardvärdet säkerställs att fältet inte är tomt när det inte finns något värde från det angivna datamodellsobjektet. Standardvärdet är förifyllt i fältet.

      * **[!UICONTROL Editable By Agent]**: Välj att tillåta agenten att redigera värdet i fältet i agentens användargränssnitt.

   1. Välj ![made_icon](assets/done_icon.png).

1. Förhandsgranska den interaktiva kommunikationen för att se tabellen renderas med data.

   ![lf_preview](assets/lf_preview.png)

### Enbart tabeller för webbkanaler {#webchanneltables}

Markera rotpanelen i webbmallen och välj **+** för att lägga till en **tabellkomponent** i den interaktiva kommunikationen. En tabell med två rader infogas i interaktiv kommunikation. Tabellens första rad representerar tabellrubriken.

#### Lägga till rader och kolumner i tabellen {#addrowscolumnstable}

**Så här lägger du till eller tar bort kolumner:**

1. Markera standardtextrutan i tabellrubrikraden för att visa komponentens verktygsfält.
1. Välj **Lägg till kolumn** eller **Ta bort kolumn** om du vill lägga till eller ta bort tabellkolumner.

![component_toolbar_table1](assets/component_toolbar_table1.png)

**Så här lägger du till eller tar bort rader:**

1. Markera någon av tabellraderna för att visa komponentens verktygsfält. Du kan också markera en tabellrad med hjälp av innehållsläsaren i sidokickaren i den interaktiva kommunikationen.
1. Välj **Lägg till rad** eller **Ta bort rad** om du vill lägga till eller ta bort tabellrader. Använd alternativen **Flytta uppåt** och **Flytta nedåt** i verktygsfältet för att ordna om rader i tabellen.

![Komponentverktygsfältet](assets/component_toolbar_table_row_new.png)

**A.** Lägg till rad **B.** Ta bort rad **C.** Flytta upp **D.** Flytta ned

#### Lägga till eller redigera text i tabellceller {#addedittexttable}

1. Markera standardtextrutan i tabellcellen och välj ![redigera](assets/edit.png) (redigera).
1. Skriv texten i tabellcellen och välj ![made_icon](assets/done_icon.png) för att spara den.

#### Skapa bindning mellan tabellceller och datamodellobjektselement {#createbindingtablecells}

1. Markera standardtextrutan i tabellraden och välj ![redigera](assets/edit.png) (redigera).
1. Markera listrutan Datamodellsobjekt och välj egenskapen.
1. Välj att spara och skapa en bindning mellan tabellcellen och datamodellens objektegenskap.

![Skapa databindning](assets/create_data_binding_table_new.png)

#### Skapa en hyperlänk för text i tabellcellen {#createhyperlinktable}

1. Markera standardtextrutan i tabellcellen och välj ![redigera](assets/edit.svg) (redigera).
1. Markera texten i tabellcellen och välj ikonen Hyperlänk.
1. Ange URL-adressen i fältet **Sökväg**.
1. Välj ![done_icon](assets/done_icon.png) om du vill spara hyperlänksegenskaperna.

![Skapa hyperlänk](assets/create_hyperlink_table_new.png)

#### Skapa dynamiska tabeller {#createdynamictables}

Du kan skapa en dynamisk tabell bara för webbkanaler i en interaktiv kommunikation med hjälp av en datamodellegenskap av typen samling. En sådan tabell är en representation av en samlingsegenskaps underordnade egenskaper. Du kan bara redigera formateringsegenskaperna för de olika cellerna i tabellen.

1. Växla till webbkanalen och välj sedan att visa webbläsaren Datakällor.
1. Dra en samlingsegenskap till ett delformulär. En tabell skapas i delformuläret.
1. Förhandsgranska tabellen i webbförhandsgranskningen av den interaktiva kommunikationen.

#### Sortera kolumner i en tabell {#sortcolumns}

Du kan sortera data baserat på valfri kolumn i en tabell i den interaktiva kommunikationen. Värdena i kolumnen kan sorteras i stigande eller fallande ordning.

Sortering kan användas för tabellkolumner som innehåller:

* Statisk text
* Egenskaper för datamodellsobjekt
* Kombination av statiska text- och datamodellsobjektsegenskaper

Så här aktiverar du sortering:

1. Markera tabellen och välj ![configure_icon](assets/configure_icon.png) (Konfigurera). Du kan också markera tabellen med hjälp av webbläsaren **Innehåll** i sidoknappen i den interaktiva kommunikationen.
1. Välj **Aktivera sortering.**
1. Välj ![done_icon](assets/done_icon.png) om du vill spara tabellegenskaperna. Sorteringsikonerna, uppåt- och nedåtpilarna, i kolumnrubriker representerar att sorteringen har aktiverats.

   ![Aktivera sortering](assets/enable_sorting_new-1.png)

1. Växla till **förhandsgranskningsläget** om du vill visa utdata. Tabellen sorteras automatiskt baserat på tabellens första kolumn.
1. Klicka på kolumnrubriken om du vill sortera värdena baserat på kolumnen.

   En kolumnrubrik med en uppåtpil representerar:

   * tabellen sorteras utifrån den kolumnen.
   * värden i kolumnen visas i stigande ordning.

   ![Sorterar stigande](assets/sorting_ascending_new-1.png)

   På samma sätt visas en kolumnrubrik med en nedpil som värden i kolumnen i fallande ordning.

## Redigera egenskaper för interaktiv kommunikation {#edit-interactive-communication-properties}

När du har skapat en interaktiv kommunikation kan du redigera dess egenskaper i ett senare skede.

Använd sidan **Egenskaper** för att:

* Redigera värden för de fält som anges när du skapar den interaktiva kommunikationen, till exempel Rubrik och Beskrivning.
* Lägg till eller ta bort webbkanal för en befintlig interaktiv kommunikation.
* Förhandsgranska, ladda ned eller ta bort interaktiv kommunikation
* Öppna [agentgränssnittet](/help/forms/using/prepare-send-interactive-communication.md).

Så här kommer du åt sidan **Egenskaper**:

1. Logga in på AEM författarinstans och gå till **Adobe Experience Manager** > **Forms** > **Forms &amp; Documents**.
1. Välj Interaktiv kommunikation och välj **Egenskaper**.
1. Klicka på fliken **Allmänt** för att redigera fälten **Titel** och **Beskrivning**.

### Lägga till eller ta bort webbkanalen {#add-or-delete-the-web-channel}

Utför följande steg för att lägga till webbkanalen för en befintlig interaktiv kommunikation:

1. Välj fliken **Kanaler** på sidan **Egenskaper**.
1. Markera kryssrutan **Webb** och välj en mall för webbkanalen.
1. Välj **Använd Skriv ut som mallsida för webbkanal** om du vill aktivera synkronisering mellan webbkanalen och skrivarkanalen.
1. Välj **Spara och stäng** om du vill spara ändringarna.

   På samma sätt kan du markera kryssrutan **Webb** på fliken **Kanaler** för att ta bort webbkanalen från den interaktiva kommunikationen.

## Lägg till Button-komponent i webbkanalen {#add-button-component-to-the-web-channel}

Du kan lägga till en knapp som en komponent i webbkanalen i den interaktiva kommunikationen. Definiera regler med [regelredigeraren](../../forms/using/rule-editor.md) för att kunna navigera till annan interaktiv kommunikation, adaptiva formulär, andra resurser som bilder eller dokumentfragment, eller en extern URL när du väljer knappen.

Så här lägger du till en knapp och definierar regler för den:

1. Markera rotpanelen i webbmallen och välj **+** för att lägga till komponenten **Button** i den interaktiva kommunikationen.
1. Markera knappkomponenten och välj ![redigeringsregler](assets/edit-rules.png) för att definiera regler för markeringen av knappen.
1. I avsnittet **När** väljer du **klickat** i läget för den nedrullningsbara listan med knappar.
1. I avsnittet **Sedan**:

   1. Välj en åtgärd i listrutan. Välj till exempel **Navigera till** som åtgärdstyp.

   1. Ange URL:en för den interaktiva kommunikationen, adaptiva formulär, en resurs eller en webbsida. Ange till exempel URL:en i följande format för att navigera till en annan interaktiv kommunikation: https://&lt;server-name>:&lt;port>/editor.html/content/forms/af/&lt;namn på interaktiv kommunikation>/channel/&lt;kanalnamn - skriv ut eller webb>.html
   1. Ange alternativet för att öppna resursen på samma flik, på en ny flik eller i ett nytt fönster.
   1. Välj **Klar** och välj sedan **Stäng** för att spara regeln.

   På samma sätt kan du välja andra tillgängliga alternativ i listrutan för åtgärdstyp, som Anropa tjänst och Skicka formulär. Mer information finns i [Regelredigeraren](../../forms/using/rule-editor.md).

1. Förhandsgranska den interaktiva kommunikationen och välj knappen för att visa interaktiv kommunikation, adaptiv form, en resurs eller en webbsida som anges i steg 4(b).

## Lägg till panelkomponent i webbkanalen {#add-panel-component-to-the-web-channel}

Panelkomponenten är en platshållare för att gruppera andra komponenter och styr hur en grupp komponenter, som dragspelspanel och tabbar, placeras i det interaktiva meddelandet. Med en panelkomponent kan du också göra en grupp komponenter repeterbara för slutanvändaren, t.ex. i flera poster som krävs för att fylla i inloggningsuppgifter.

Utför följande steg för att lägga till en panelkomponent i webbkanalen:

1. Infoga **Panel**-komponenten i webbkanalen med något av följande alternativ:

   * Markera en komponent, markera **+** och välj **Panel** -komponenten.

   * Dra och släpp **Panel**-komponenten i den interaktiva kommunikationen från webbläsarpanelen **Komponent** .

   * Markera **panelen** i webbläsarpanelen **Innehåll** och välj **Lägg till underordnad panel**. Om du väljer alternativet **Lägg till underordnad panel** visas dialogrutan **Lägg till underordnad panel** . Ange panelkomponentens titel och en valfri beskrivning och namn.

1. Välj panelen i **Innehåll** -webbläsaren om du vill utföra ytterligare åtgärder på panelen, till exempel konfigurera, redigera regler, kopiera, ta bort och infoga komponent.

   Du kan också dra och släppa en panel i webbläsaren **Innehåll** för att återspegla ändringen i strukturen för den interaktiva kommunikationen i den högra rutan.

## Synkronisera webbkanal med utskriftskanal {#synchronize}

När du väljer Skriv ut som mallsida för webbkanal när du skapar en interaktiv kommunikation skapas webbkanalen synkroniserat med utskriftskanalen och webbkanalens innehåll och databindning härleds från utskriftskanalen och de ändringar som görs i den kan visas i webbkanalen när du väljer Synkronisera.

Författarna kan dock bryta arvet för komponenter i webbkanalen efter behov.

![Skapa skrivarmallsida](assets/create_ic_print_master_new-1.png) ![Skriv ut mallwebbsida](assets/create_ic_print_master_web_new-1.png)

### Automatisk synkronisering {#autosync}

Om du väljer alternativet **[!UICONTROL Use Print As Master for Web Channel]** kan du välja något av följande lägen för att generera webbkanalen:

* **[!UICONTROL Auto layout]**: Välj det här läget om du automatiskt vill generera platshållare, innehåll och databindning för webbkanalen från Utskriftskanalen.
* **[!UICONTROL Manually organize]**: Välj det här läget om du vill markera och lägga till Print channel-element manuellt i webbkanalen med huvudinnehållet som är tillgängligt på fliken Datakällor. Mer information finns i [Markera Skriv ut kanalelement för att skapa webbkanalsinnehåll](#selectprintchannelelements).

![Skapa IC-alternativ](assets/create_ic_options_updated_new.png)

>[!NOTE]
>
>Synkronisering av kanalerna synkroniserar endast dokumentfragment, bilder, villkor, listor och layoutfragment från tryckkanalen till webbkanalen. De delformulär eller överordnade noder som innehåller sådana element synkroniseras inte.

### Välj Skriv ut kanalelement för att skapa webbkanalsinnehåll {#selectprintchannelelements}

Om du väljer Skriv ut som master när du skapar den interaktiva kommunikationen och inte väljer alternativet för automatisk synkronisering, kan du även dra och släppa Print channel-element till webbkanalens redigeringsgränssnitt.

Navigera till **Datakällor** > **Huvudinnehåll** för att visa elementen i utskriftskanalen. Dra och släpp målområdena, fälten eller tabellerna i webbkanalens redigeringsgränssnitt. En blå cirkel bredvid elementnamnet anger att elementet för utskriftskanalen redan finns i webbkanalen.

![Huvudinnehåll](assets/master_content.png)

### Avbryt arv {#cancelinheritance}

I webbkanalen är komponenterna inbäddade i målområdena.

Hovra över det relevanta målområdet eller variabeln i webbkanalen och välj ![cancelarance](assets/cancelinheritance.png) (Cancel Inheritance) och sedan **[!UICONTROL Yes]** i dialogrutan Cancel Inheritance (Avbryt arv).

Arvet av komponenterna i målområdet avbryts och nu kan du redigera dem efter behov.

### Återaktivera arv {#re-enable-inheritance}

Om du har avbrutit arvet av en komponent i webbkanalen kan du aktivera den igen. Om du vill återaktivera arv håller du pekaren över gränsen för det relevanta målområdet, som innehåller komponenten, och väljer ![återaktivera arv](assets/reenableinheritance.png).

Dialogrutan Återställ arv visas.

![återarv](assets/revertinheritance.png)

Välj **[!UICONTROL Synchronize The Page After Reverting Inheritance]** om det behövs. Välj det här alternativet om du vill synkronisera hela den interaktiva kommunikationen. Om du inte markerar det här alternativet synkroniseras bara det relevanta målområdet när arvet återställs.

Välj **[!UICONTROL Yes]**.

### Synkronisera {#synchronize-1}

Om du använder Skriv ut som mallsida för webbkanal och ändrar utskriftskanalen, kan du synkronisera innehållet för att lägga till de nya ändringarna i webbkanalen.

1. Om du vill synkronisera webbkanalen med skrivarkanalen växlar du till webbkanalen och väljer ikonen Fler alternativ.

   ![Alternativ för automatisk synkronisering](assets/auto_sync_options_new.png)

1. Välj något av följande:

   * **[!UICONTROL Sync with Print]**: Synkroniserar endast innehåll för de målområden där arv inte avbryts.
   * **[!UICONTROL Reset]**: Synkroniserar webbkanalsinnehållet med skrivarkanalen och ignorerar alla ändringar som gjorts i webbkanalen.

### Använda komponentens verktygsfält för att utföra åtgärder på ärvda komponenter {#componenttoolbar}

När du har autogenererat innehåll i webbkanalen med alternativet Synkronisera kan du utföra fler åtgärder på komponenter utan att avbryta arv.

![Komponentverktygsfältet](assets/component_toolbar_inherited_web_new.png)

Markera komponenten för att visa följande alternativ:

* **Kopiera:** Kopiera en komponent och klistra in den på andra platser i den interaktiva kommunikationen.
* **Klipp ut:** Flytta en komponent från en plats till en annan i den interaktiva kommunikationen.
* **Infoga komponent:** Infoga en komponent ovanför den markerade komponenten.
* **Klistra in:** Klistra in komponenten som du klippt ut eller kopierat med alternativen som beskrivs ovan.
* **Grupp:** Markera flera komponenter om du vill klippa ut, kopiera eller klistra in mer än en komponent tillsammans.
* **Överordnad:** Markera den överordnade komponenten för en komponent.
* **Visa SOM-uttryck:** Visa [SOM-uttryck](../../forms/using/using-som-expressions-adaptive-forms.md) för komponenten.

* **Gruppera objekt på panelen:** Gruppera komponenterna på en panel för att kunna utföra åtgärder på dessa komponenter samtidigt. Mer information finns i [Gruppera objekt på panelen](#groupobjectspanel).

* **Avbryt arv:** [Avbryt arvet](#cancelinheritance) av komponenterna i målområdet för att redigera dem.

### Gruppera objekt i panelen {#groupobjectspanel}

Med webbkanalens redigeringsgränssnitt är det lättare att gruppera komponenterna på en panel för att kunna utföra åtgärder på dessa komponenter samtidigt. Fliken **Innehåll** visar de grupperade komponenterna som underordnade element för panelen i innehållsträdet.

1. Markera en komponent och välj gruppåtgärden ( ![grupp](assets/group.jpg)).
1. Markera flera komponenter och välj **Gruppera objekt i panelen**.

   ![Gruppera objekt](assets/component_toolbar_group_objects_new.png)

1. Ange ett namn för panelen i dialogrutan **Gruppera objekt på panelen**.
1. Ange en valfri titel och beskrivning för panelen.
1. Klicka på ![bullet_checkmark](assets/bullet_checkmark.png).

   De grupperade komponenterna visas som underordnade element till panelen i innehållsträdet.

   ![content_tree_grouping](assets/content_tree_grouping.png)

## Utdataformat för utskriftskanal {#output-format-print-channel}

Använd PrintChannel API för att definiera utdataformat för Print-kanalen i en interaktiv kommunikation. Om du inte definierar något utdataformat genererar AEM Forms utdata i PDF-format.

```javascript
//options for rendering print channel of a multi-channel document
PrintChannelRenderOptions renderOptions = new PrintChannelRenderOptions();
PrintDocument printDocument = printChannel.render(renderOptions);
```

Om du vill generera utdata i något annat format anger du typ av utdataformat. I [PrintChannel API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/PrintConfig.html) finns en lista över de utdataformat som stöds.

Du kan till exempel använda följande exempel för att definiera PCL som utdataformat för en interaktiv kommunikation:

```javascript
//options for rendering print channel of a multi-channel document
PrintChannelRenderOptions renderOptions = new PrintChannelRenderOptions();
renderOptions.setRenderFormat(PrintConfig.HP_PCL_5e);
PrintDocument printDocument = printChannel.render(renderOptions);
```
