---
title: Formuläruppsättning i AEM Forms
description: I den här artikeln beskrivs hur du skapar formuläruppsättningar genom att sammanfoga HTML5-formulär. I den här artikeln beskrivs också hur du kan förifylla xml-data i en formuläruppsättning och hur du kan använda formuläruppsättningar i processhanteringen.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 29339930-2c0c-46d8-84ad-92b1e8384422
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2804'
ht-degree: 0%

---

# Formuläruppsättning i AEM Forms{#form-set-in-aem-forms}

## Ökning {#overview}

Kunderna måste ofta skicka in flera formulär för att ansöka om en tjänst eller förmån. Det handlar om att hitta alla relevanta formulär och fylla i, skicka och spåra dem separat. De måste också fylla i gemensamma uppgifter flera gånger i formulär. Hela processen blir krånglig och felbenägen om den innehåller ett stort antal formulär. Funktionen för formuläruppsättning i AEM Forms kan förenkla användarupplevelsen i sådana scenarier.

En formuläruppsättning är en samling HTML5-formulär som grupperats tillsammans och presenteras som en enda formuläruppsättning för slutanvändarna. När användarna börjar fylla i en formuläruppsättning, överförs de smidigt från ett formulär till ett annat. Till sist kan de skicka in alla blanketter med bara ett klick.

AEM Forms har ett intuitivt användargränssnitt för att skapa, konfigurera och hantera formuläruppsättningar. Som författare kan du beställa formulär i en viss sekvens som du vill att slutanvändarna ska följa. Du kan också använda villkor eller uttryck för behörighet i enskilda formulär för att kontrollera synligheten baserat på användarens indata. Du kan t.ex. konfigurera att informationsformuläret för make/maka endast ska visas när civilstånd anges som gifta.

Dessutom kan du konfigurera gemensamma fält i olika formulär för att dela gemensamma databindningar. Med rätt databindningar på plats behöver slutanvändarna bara fylla i vanlig information när de har fyllts i automatiskt i efterföljande formulär.

Formuläruppsättningar stöds också i AEM Forms-appen, vilket gör att fältarbetarna kan göra en formuläruppsättning offline, besöka kunder, lägga in data och synkronisera senare med AEM Forms-servern för att skicka formulärdata till affärsprocesserna.

## Skapa och hantera formuläruppsättning {#creating-and-managing-form-set}

Du kan koppla flera XDP-filer eller formulärmallar, som har skapats med Designer, till en formuläruppsättning. Formuläruppsättningar kan sedan användas för att selektivt återge XDP:er baserat på värden som användarna angett i de ursprungliga formulären och deras profiler.

Använd [AEM Forms användargränssnitt](../../forms/using/introduction-managing-forms.md) för att hantera alla formulär, formuläruppsättningar och relaterade resurser.

### Skapa en formuläruppsättning {#create-a-form-set}

Så här skapar du en formuläruppsättning:

1. Välj Forms > Forms och Dokument.
1. Välj Skapa > Formuläruppsättning.

1. Lägg till följande information på sidan Lägg till egenskaper och klicka på Nästa.

   * Titel: Anger dokumentets titel. Titeln hjälper dig att identifiera formuläruppsättningen i AEM Forms användargränssnitt.
   * Beskrivning: Anger detaljerad information om dokumentet.
   * Taggar: Anger taggar som unikt identifierar formuläruppsättningen. Taggar hjälper dig att söka i formuläruppsättningen. Om du vill skapa taggar skriver du nya taggnamn i rutan Taggar.
   * Skicka-URL: Anger den URL där skickade data publiceras för fristående återgivning av formuläruppsättning (icke-AEM Forms-programanvändning). Data skickas till den här slutpunkten som multipart/formdata med följande request-parameter:
   * dataXML: Den här parametern innehåller en XML-representation av skickade formuläruppsättningsdata. Om alla formulär i formuläruppsättningen använder ett gemensamt schema, genereras XML enligt det schemat. Annars innehåller XML-rottaggen en underordnad tagg för varje ifyllt formulär i formuläruppsättningen som innehåller data för de bifogade formulärfilerna.
   * formsetPath: Sökvägen till den formuläruppsättning i CRXDE som har skickats.
   * HTML återgivningsprofil: Du kan konfigurera vissa alternativ, till exempel flytande fält, bilagor och utkaststöd (för fristående formuläruppsättningsåtergivning) för att anpassa formuläruppsättningens utseende, beteende och interaktioner. Du kan anpassa eller utöka den befintliga profilen om du vill ändra inställningarna för en HTML-formulärprofil.

   ![Formuläruppsättning: lägg till egenskaper](assets/createformset1.png)

1. Skärmen Välj formulär visar tillgängliga XDP-formulär eller XDP-filer. Sök efter och markera formulären som ska ingå i formuläruppsättningen och klicka sedan på Lägg till i formuläruppsättningen. Om det behövs kan du söka efter formulär som ska läggas till igen. När du har lagt till alla formulär i formuläruppsättningen klickar du på Nästa.

   >[!NOTE]
   >
   >Se till att fältnamnen i XDP-formulär inte innehåller punkttecknet. I annat fall kan skript som försöker lösa fälten, som har punkttecken, inte matcha dem.

1. På sidan Konfigurera formulär kan du göra följande:

   * Formulärordning: Dra och släpp formulären för att ordna om dem. Formulärordningen definierar den ordning i vilken formulären visas för slutanvändaren i AEM Forms-appen och i en fristående återgivning.
   * Formuläridentifierare: Anger en unik identitet för de formulär som ska användas i berättigandeuttryck.
   * Datarot: För varje formulär i formuläruppsättningen kan författaren konfigurera XPATH där data i det aktuella formuläret placeras i skickad XML. Som standard är värdet /. Om alla formulär i formuläruppsättningen är schemabundna och har samma XML-schema kan du ändra det här värdet. Vi rekommenderar att alla fält i formuläret har rätt databindning angiven i XDP. Om två fält i två olika formulär har samma gemensamma databindning, visar fältet i det andra formuläret förfyllda värden från det första formuläret. Bind inte två delformulär med samma interna innehåll till samma XML-nod. Mer information om XML-strukturen i formuläruppsättningen finns i [Förifyll XML för formuläruppsättningen](../../forms/using/formset-in-aem-forms.md#p-prefill-xml-for-form-set-p).
   * Kvalifikationsuttryck: Anger ett JavaScript-uttryck som utvärderar ett booleskt värde och anger om ett formulär i formuläruppsättningen kan fyllas i. Om värdet är false tillfrågas inte användaren och visas inte heller formuläret för att fyllas i. Uttrycket baseras vanligtvis på värdena i de fält som har hämtats före det här formuläret. Uttrycken innehåller även anrop till formuläruppsättningens API fs.valueOf för att extrahera de värden som användaren fyller i i ett fält i ett formulär i formuläruppsättningen:

   *fs.valueOf(&lt;Form Identifier>, &lt;fieldAs expression>) > &lt;value>*

   Om du t.ex. har två formulär i formuläruppsättningen: utgift för företag och resekostnader, kan du lägga till ett JavaScript-utdrag i fältet Berättigandeuttryck för båda dessa formulär för att kontrollera vilka typer av utgifter användaren anger i formuläret. Om användaren väljer Affärskostnad återges formuläret Affärskostnad för slutanvändaren. Eller om användaren väljer en resekostnad, återges ett annat formulär för slutanvändaren. Mer information finns i Kvalificeringsuttryck.

   Dessutom kan författaren välja att ta bort ett formulär från formuläruppsättningen med ikonen Ta bort i det högra hörnet av varje rad eller lägga till en annan uppsättning formulär med ikonen **+** i verktygsfältet. Den här ikonen **+** dirigerar användaren tillbaka till föregående steg i guiden som användes för att välja formulär. De befintliga markeringarna behålls och eventuella ytterligare markeringar som görs måste läggas till i formuläruppsättningen med hjälp av ikonen Lägg till i formuläruppsättning på den sidan.

   ![Formuläruppsättning: Konfigurera formulär ](assets/createformset2.png)

   >[!NOTE]
   >
   >Alla formulär som används i formuläruppsättningen hanteras av AEM Forms användargränssnitt.

### Hantera en formuläruppsättning {#managing-a-form-set}

När en formuläruppsättning har skapats kan du utföra följande åtgärder för den formuläruppsättningen:

* Klicka en gång: När en formuläruppsättning skapas och visas på huvudresurssidan kan du visa den genom att klicka en gång på den. En formuläruppsättning öppnas och visar alla formulärmallar (XDP) i den formuläruppsättningen.
* Redigera: När du klickar på Redigera efter att du har valt en formuläruppsättning öppnas skärmen Konfigurera formulär som visas ovan i steg för att skapa en formuläruppsättning. Du kan utföra alla funktioner som beskrivs där.
* Kopiera och klistra in: Detta gör att du kan kopiera hela formuläruppsättningen från en plats och klistra in den på samma eller någon annan plats eller mapp.
* Hämta: Du kan hämta formuläruppsättningen med alla dess beroenden.
* Starta/hantera granskning: När formuläruppsättningen har skapats kan du konfigurera granskningen genom att klicka på Starta granskning. När granskningen har startats för en formuläruppsättning visas alternativet Hantera granskning för användaren. På granskningsskärmen kan du uppdatera/avsluta granskningen. För de granskningar du har lagt till kan du kontrollera granskningen och lägga till kommentarer om det behövs.
* Ta bort: Tar bort hela formuläruppsättningen. Formulären i den borttagna formuläruppsättningen finns kvar i databasen.
* Publicera/avpublicera: Detta publicerar/återpublicerar formuläruppsättningen tillsammans med alla formulär som den innehåller och de relaterade resurserna i dessa formulär.
* Förhandsgranska: Förhandsgranska innehåller två alternativ: Förhandsgranska som HTML (utan data) och anpassad förhandsvisning med exempeldata.
* Visa/redigera egenskaper: Du kan visa/redigera metadataegenskaperna för en markerad formuläruppsättning.

![createForset3](assets/createformset3.png)

### Redigera en formuläruppsättning {#edit-a-form-set}

Så här redigerar du en formuläruppsättning:

1. Välj Forms > Forms och Dokument.
1. Leta reda på den formuläruppsättning som du vill redigera. Håll pekaren över den och välj Redigera ( ![redigering](assets/editicon.png)).
1. På sidan Konfigurera formulär kan du redigera följande:

   * Formulärordning
   * Formuläridentifierare
   * Datarot
   * Kvalifikationsuttryck

   Du kan också klicka på motsvarande Ta bort-ikon för att ta bort formuläret från formuläruppsättningen.

## Formuläret har angetts i processhantering {#form-set-in-process-management}

När du har skapat en formuläruppsättning med användargränssnittet i AEM Forms Management kan du använda formuläruppsättningen i en startpunkt- eller tilldelningsaktivitet med Workbench.

### Använda formuläruppsättning i Aktivitet eller Startpunkt {#using-form-set-in-task-or-start-point}

1. När du utformar en process väljer du **Använd en CRX-resurs** under Presentation &amp; Data i Tilldela uppgift/startpunkt. Webbläsaren CRX Asset visas.

   ![Designa en process: använd en CRX-resurs](assets/formsetinprocessmgmt1.png)

1. Välj formuläruppsättning för att filtrera formuläruppsättningen i AEM-databasen (CRX).

   ![Utforma en process: Välj formulärresurs, dialogruta](assets/formsetinprocessmgmt2.png)

1. Markerar en formuläruppsättning och klickar på OK.

## Kvalifikationsuttryck {#eligibility-expressions}

Kvalifikationsuttryck i en formuläruppsättning används för att definiera och dynamiskt kontrollera formulär som visas för en användare. Om du till exempel bara vill visa ett visst formulär om användaren tillhör en viss åldersgrupp. Ange och redigera ett berättigandeuttryck med formulärhanteraren.

Ett kvalificeringsuttryck kan vara vilken giltig JavaScript-sats som helst som returnerar ett booleskt värde. Den sista programsatsen i JavaScript-kodfragmentet behandlas som ett booleskt värde som avgör om formuläret är kvalificerat baserat på bearbetningen i resten (tidigare rader) av JavaScript-kodfragmentet. Om värdet för uttrycket är true kan formuläret visas för användaren. Sådana formulär kallas för bidragsberättigande formulär.

>[!NOTE]
>
>Kvalifikationsuttrycket för det första formuläret i formuläruppsättningen körs inte. Det första formuläret visas alltid, oavsett vilket uttryck det gäller.

Förutom JavaScript standardfunktioner visar formuläruppsättningen även API:t fs.valueOf som ger åtkomst till värdet i ett formulärfält i en formuläruppsättning. Använd detta API för att komma åt värdet i ett formulärfält i en formuläruppsättning. API-syntaxen är fs.valueOf (formUid, fieldSOM), där:

* formUid (sträng): Ett unikt ID för ett formulär i formuläruppsättningen. Du kan ange det när du skapar formuläruppsättningen i användargränssnittet för formulärhanteraren. Som standard är det formulärets namn.
* fieldSOM (string): A SOM expression of the field in the form specified by the formUid. SOM-uttryck eller skriptobjektmodelluttryck används för att referera till värden, egenskaper och metoder i en viss dokumentobjektmodell (DOM). Du kan visa den i Form Designer under fliken Skript när fältet är markerat.

>[!NOTE]
>
>Både formUid- och fieldSOM-parametrar måste vara stränglitteraler.

### Exempel {#examples}

Giltig användning av API:

`fs.valueOf("form1", "xfa.form.form1.subform1.field1")`

Ogiltig användning av API:

```javascript
var formUid = "form1";
 var fieldSOM = "xfa.form.form1.subform1.field1"; fs.valueOf(formUid, fieldSOM);
```

## Förifyll XML för formuläruppsättning {#prefill-xml-for-form-set}

Formuläruppsättningen är en samling med flera HTML5-formulär som har gemensamma eller olika scheman. Formuläruppsättningen har stöd för förifyllning av formulärfält med hjälp av en XML-fil. Du kan associera en XML-fil med en formuläruppsättning så att vissa fält i formuläret förpoleras när du öppnar ett formulär i formuläruppsättningen.

XML-filen för förifyllnad anges med parametern dataRef för formulärets URL. Parametern dataRef anger den absoluta sökvägen till XML-datafilen som sammanfogas med formuläruppsättningen.

Du har till exempel tre formulär (formulär1, formulär2 och formulär3) i formuläruppsättningen med följande struktur:

formulär1

fält
form1field

formulär2

fält
form2field

form3

fält
form3field

Varje formulär har ett gemensamt namngivet fält med namnet&quot;field&quot; och ett unikt namngivet fält med namnet&quot;form&lt;i>field&quot;.

Du kan förifylla den här formuläruppsättningen med en XML-kod med följande struktur:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<formSetRootTag>
 <field>common field value</field>
 <form1field>value1</form1field>
 <form2field>value2</form2field>
 <form3field>value3</form3field>
</formSetRootTag>
```

>[!NOTE]
>
>XML-rottaggen kan ha vilket namn som helst, men elementtaggarna som motsvarar fälten måste ha samma namn som fältet. XML-hierarkin måste likna formulärets hierarki, vilket innebär att XML måste ha motsvarande taggar för att kapsla delformulär.

Ovanstående XML-kodutdrag visar att XML-förifyllnad för formuläruppsättningen är en förening av de individuella formulärens förifyllda XML-fragment. Om vissa fält i de olika formulären har en likartad datahierarki eller dataschema fylls fälten i förväg med samma värden. I det här exemplet är alla tre formulär förifyllda med samma värde för det gemensamma fältet, &quot;field&quot;. Detta är ett enkelt sätt att föra data vidare från ett formulär till nästa. Detta kan också uppnås genom att binda fälten till samma schema eller datareferens. Om du vill dela upp formuläruppsättningsdata baserat på formulärets schema. Detta kan du uppnå genom att ange formulärets&quot;datarotattribut&quot; när en formuläruppsättning skapas (standardvärdet är&quot;/&quot;, som mappar till formulärets rottagg).

Om du i det föregående exemplet anger datarötter: &quot;/form1&quot;, &quot;/form2&quot; och &quot;/form3&quot; för de tre formulären måste du använda en förifylld XML med följande struktur:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<formSetRootTag>
 <form1>
  <field>field value1</field>
  <form1field>value1</form1field>
 </form1>
 <form2>
  <field>field value2</field>
  <form2field>value2</form2field>
 </form2>
 <form3>
  <field>field value3</field>
  <form3field>value3</form3field>
 </form3>
</formSetRootTag>
```

I en formuläruppsättning definierade XML-schemat med följande syntax:

```xml
<formset>
 <fs_data>
  <xdp:xdp xmlns:xdp="https://ns.adobe.com/xdp/">
  <xfa:datasets xmlns:xfa="https://www.xfa.org/schema/xfa-data/1.0/">
   <xfa:data>
   <rootElement>
    ... data ....
   </rootElement>
   </xfa:data>
  </xfa:datasets>
  </xdp:xdp>
 </fs_data>
 <fs_draft>
  ... private data...
 </fs_draft>
</formset>
```

>[!NOTE]
>
>Om det finns två formulär med överlappande datarötter, eller om elementhierarkin i ett formulär överlappar datarothierarkin i ett annat formulär, sammanfogas de överlappande elementens värden i XML-förinställningen. Skicka-XML har en struktur som liknar förifyll-XML, men skicka-XML har fler omslutningstaggar och några kontexttaggar för formulärets uppsättning tillagda i slutet.

### Beskrivning av förifyllda XML-element {#prefill-xml-elements-description}

Syntaxregler för att skapa en XML-fil för förifyllning:

* överordnade element: element som kan vara överordnade element, där null anger att elementet kan vara roten för XML.
* kardinalitet: visar hur många gånger elementet kan användas inuti det överordnade elementet.
* submitXML: anger om elementet alltid finns (P) eller valfritt(O) i XML-sändning.
* prefillXML: anger om elementet är obligatoriskt(R) eller valfritt(O) i XML för förifyllning.
* underordnade: anger vilka element som kan vara underordnade.

### FORMSET {#formset}

`parent elements:`

`null`

`cardinality: [0,1]`

`submitXML: P`

`prefillXML: O`

`children: fs_data`

Rotelementet i XML för formuläruppsättningen. Du bör inte använda det här ordet som namn på rootSubform för något formulär i formuläruppsättningen.

### FS_DATA {#fs-data}

`parent elements:`

`formset`

kardinalitet: [1]

submitXML: P

prefillXML: O

`children: xdp:xdp/rootElement`

Underträdet visar data i formulären i formuläruppsättningen. Elementet är valfritt i förifylld XML bara om det inte finns något formulärelementet

### XDP:XDP {#xdp-xdp}

`parent elements: fs_data/null`

`cardinality: [0,1]`

`submitXML: O`

`prefillXML: O`

`children: xfa:datasets`

Det här märkordet anger början på HTML5 Form XML. Detta läggs till i XML-sändningsfilen om den finns i XML-förifyllningen eller om det inte finns någon XML-förifyllning. Det här märkordet kan tas bort från XML-koden för förifyllning.

### XFA:DATASETS {#xfa-datasets}

`parent elements: xdp:xdp`

`cardinality: [1]`

`submitXML: O`

`prefillXML: O`

`children: xfa:data`

### XFA:DATA {#xfa-data}

`parent elements: xfa:datasets`

`cardinality: [1]`

`submitXML: O`

`prefillXML: O`

`children: rootElement`

### ROOTELEMENT {#rootelement}

`parent elements: xfa:datasets/fs_data/null`

`cardinality: [0,1]`

`submitXML: P`

`prefillXML: O`

`children: controlled by the Forms in Form set`

Namnet rootElement är bara en platshållare. Det faktiska namnet hämtas från formulären som används i formuläruppsättningen. Det underträd som börjar med rootElement innehåller data från fälten och delformulären i Forms i formuläruppsättningen. Det finns flera faktorer som bestämmer strukturen för rootElement och dess underordnade element.

I förifylld XML är den här taggen valfri, men om den saknas ignoreras hela XML-koden.

NAMN PÅ ROTELEMENTTAGGEN

Om det finns ett rotelement i XML-förifyllningen används elementets namn även i XML-sändningsfilen. I de fall där det inte finns någon prefill xml är namnet på rootElement namnet på rotdelformuläret för det första formuläret i formuläruppsättningen som har egenskapen dataRoot inställd på /. Om det inte finns något sådant formulär är rootElement-namnet **fs_dummy_root**, som är ett reserverat nyckelord.

## Formuläruppsättning i AEM Forms-app {#formset-in-workspace-app}

Med AEM Forms-appen kan fältarbetare synkronisera sina mobila enheter med en AEM Forms-server och arbeta med sina uppgifter. Programmet fungerar även när enheten är offline genom att spara data lokalt på enheten. Med annoteringsfunktioner som fotografier kan fältarbetare ge korrekt information som kan integreras i affärsprocesserna.

<!-- Update link as it is a 404 - For more information on AEM Forms app, see [AEM Forms app overview](/help/forms/using/mobile-workspace-overview.md).-->

## Kända begränsningar - mönster som inte stöds fullt ut i formuläruppsättningen {#known-limitations-patterns-not-fully-supported-in-form-set}

Följande datamönster stöds inte fullt ut i formuläruppsättningen:

<table>
 <tbody>
  <tr>
   <td><strong>Mönstret stöds inte fullständigt i formuläruppsättningen</strong></td>
   <td><strong>Exempel</strong></td>
  </tr>
  <tr>
   <td>Indatastorlek och mönsterstorlek matchar inte</td>
   <td><p>When pattern= num{z,zzz}</p> <p>And input=</p> <p>12 345 eller</p> <p>1,23</p> </td>
  </tr>
  <tr>
   <td>Bildpunktsmönster med hakparenteser "(" ")"</td>
   <td>num{(zz,zzz)}</td>
  </tr>
  <tr>
   <td>Flera datamönster</td>
   <td>num{zz,zzz} | num{z,zzz,zzz}</td>
  </tr>
  <tr>
   <td>Shorthand-mönster </td>
   <td><p>num.integer{},</p> <p>num.decimal{},</p> <p>num.percent{}, eller</p> <p>num.currency{}</p> </td>
  </tr>
 </tbody>
</table>
