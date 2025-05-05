---
title: Introduktion till utveckling av anpassningsbara formulär
description: AEM Forms har ett lättanvänt men ändå kraftfullt gränssnitt för framtagning av adaptiva blanketter. Den innehåller en mängd komponenter och verktyg som du kan använda för att skapa formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: introduction, author
docset: aem65
feature: Adaptive Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: a1791374-d05c-4f60-b178-152a7bc06c45
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3086'
ht-degree: 0%

---

# Introduktion till utveckling av anpassningsbara formulär {#introduction-to-authoring-adaptive-forms}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/create-an-adaptive-form-on-forms-cs/introduction-forms-authoring.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |


## Ökning {#overview}

Med adaptiva formulär kan du skapa formulär som är engagerande, responsiva, dynamiska och anpassningsbara. AEM Forms har ett intuitivt användargränssnitt och färdiga komponenter för att skapa och arbeta med anpassningsbara formulär. Du kan välja att skapa ett anpassat formulär baserat på en formulärmodell eller ett schema eller utan en formulärmodell. Det är viktigt att du noga väljer den formulärmodell som inte bara passar dina behov, utan som utökar dina befintliga infrastrukturinvesteringar och resurser. Du kan välja mellan följande alternativ för att skapa ett anpassat formulär:

* **Använda en formulärdatamodell**
  Med [dataintegrering](../../forms/using/data-integration.md) kan du integrera entiteter och tjänster från olika datakällor i en formulärdatamodell som du kan använda för att skapa anpassade formulär. Välj formulärdatamodell om det adaptiva formulär du skapar inbegriper hämtning och skrivning av data från och till flera datakällor.

* **Använda en XDP-formulärmall**
Det är en idealisk formulärmodell om du har investeringar i XFA- eller XDP-formulär. Det är ett direkt sätt att konvertera XFA-baserade formulär till anpassningsbara formulär. Alla befintliga XFA-regler behålls i de tillhörande adaptiva formulären. De färdiga adaptiva formulären har stöd för XFA-konstruktioner, till exempel valideringar, händelser, egenskaper och mönster.

* **Använda en XSD (XML Schema Definition) eller ett JSON-schema**
XML- och JSON-scheman representerar den struktur i vilken data produceras eller förbrukas av organisationens serversystem. Du kan koppla schemat till ett anpassat formulär och använda dess element för att lägga till dynamiskt innehåll i det anpassningsbara formuläret. Elementen i schemat kommer att vara tillgängliga för användning på fliken Datamodellobjekt i innehållsläsaren när du redigerar adaptiva formulär.

* **Använda ingen eller utan formulärmodell**
Anpassningsbara formulär som skapas med det här alternativet använder inte någon formulärmodell. Data-XML som genereras från sådana formulär har en platt struktur med fält och motsvarande värden.

Mer information om hur du skapar ett anpassat formulär finns i [Skapa ett anpassat formulär](../../forms/using/creating-adaptive-form.md).

## Gränssnitt för redigering av anpassningsbara formulär {#adaptive-form-authoring-ui}

Det pekoptimerade användargränssnittet för att skapa anpassningsbara formulär är intuitivt och ger:

* Dra-och-släpp-funktioner
* Standardformulärkomponenter
* Integrerad databas för resurser

När du skapar eller redigerar ett befintligt anpassat formulär använder du följande gränssnittselement:

* [Sidebar](#sidebar)
* [Verktygsfältet Sida](#page-toolbar)
* [Komponentverktygsfältet](#component-toolbar)
* [Adaptiv formulärsida](#af-page)

![Gränssnitt för anpassad formulärutveckling](assets/formeditor.png)

**A.** Sidpanelen **B.** Sidverktygsfältet **C.** Sidan Adaptivt formulär

### Sidebar {#sidebar}

Med sidofältet

* Se formulärinnehåll som paneler, komponenter, fält och layout.
* Redigera komponentegenskaper.
* Sök, visa och använd resurser i AEM Digital Asset Management (DAM).
* Lägg till komponenter i formuläret.

![Sidofältet](assets/sidebar-comps.png)

**A.** Innehållsläsaren **B.** Egenskapsläsaren **C.** Assets-webbläsaren **D.** Komponentwebbläsaren

<!--Click to enlarge

](assets/sidebar-comps-1.png) -->

Sidlisten innehåller följande webbläsare:

* **Innehållsläsaren**
I innehållsläsaren ser du

   * **Formulärobjekt**
Visar objekthierarkin för formuläret. Författaren kan navigera till en viss formulärkomponent genom att trycka på det elementet i formulärobjektträdet. Författaren kan söka efter objekt och ordna om dem från det här trädet.

   * **Datamodellsobjekt**
Här kan du se formulärmodellens hierarki.
Du kan dra och släppa formulärmodellelement i det anpassade formuläret. De tillagda elementen konverteras automatiskt till formulärkomponenter samtidigt som deras ursprungliga egenskaper behålls. Du kan se datamodellsobjekt när formuläret använder XML-schema, JSON-schema eller XDP-mall.

* **Egenskapsläsaren**

  Gör att du kan redigera egenskaperna för en komponent. Egenskaperna ändras enligt en komponent. Så här visar du egenskaper för den adaptiva formulärbehållaren:

  Markera en komponent, välj ![fältnivå](assets/field-level.png) > **[!UICONTROL Adaptive Form Container]** och välj sedan ![cmpr](assets/cmppr.png).

* **Assets webbläsare**

  Segmenterar olika typer av innehåll som bilder, dokument, sidor, filmer och så vidare.

* **Komponentwebbläsaren**

  Innehåller komponenter som du kan använda för att skapa ett anpassat formulär. Du kan dra komponenter från till det adaptiva formuläret för att lägga till formulärelement och konfigurera tillagda element enligt kraven. I följande tabell beskrivs komponenterna i komponentwebbläsaren.

<table>
 <tbody>
  <tr>
   <th><strong>Komponent</strong></th>
   <th><strong>Funktionalitet</strong></th>
  </tr>
  <tr>
   <td>Adobe Sign-block</td>
   <td>Lägger till ett textblock med platshållare för fält som ska fyllas i vid signering med Adobe Sign.</td>
  </tr>
  <tr>
   <td>Knapp</td>
   <td>Lägger till en knapp som du kan konfigurera för att utföra åtgärder som att spara, återställa, gå vidare, gå till föregående och så vidare.</td>
  </tr>
  <tr>
   <td>Captcha</td>
   <td>Lägger till CAPTCHA-validering med tjänsten Google reCAPTCHA. Mer information finns i <a href="../../forms/using/captcha-adaptive-forms.md" target="_blank">Använda CAPTCHA i adaptiva formulär</a>.</td>
  </tr>
  <tr>
   <td>Diagram</td>
   <td>Lägger till ett diagram som du kan använda i adaptiva formulär och dokument för visuell representation av tvådimensionella data i repeterbara paneler och tabellrader.</td>
  </tr>
  <tr>
   <td>Kryssruta</td>
   <td>Lägger till en kryssruta.</td>
  </tr>
  <tr>
   <td>Datumindatafält</td>
   <td>Använd datumindatafält i formuläret för att låta kunderna fylla i dag, månad och år separat i tre rutor. Du kan anpassa komponentens utseende och känsla och ändra datumformatet. Du kan till exempel låta kunderna ange datum i formaten MM/DD/ÅÅÅÅ eller DD/MM/ÅÅÅÅ.</td>
  </tr>
  <tr>
   <td>Datumväljaren</td>
   <td>Lägger till ett kalenderfält för att välja ett datum.</td>
  </tr>
  <tr>
   <td>Dokumentfragment</td>
   <td>Gör att du kan lägga till återanvändbara komponenter i en korrespondens.</td>
  </tr>
  <tr>
   <td>Dokumentfragmentgrupp</td>
   <td>Gör att du kan lägga till en grupp med relaterade dokumentfragment som du kan använda i en brevmall som en enskild enhet.</td>
  </tr>
  <tr>
   <td>Nedrullningsbar lista</td>
   <td>Lägger till en nedrullningsbar lista - en eller flera markeringar</td>
  </tr>
  <tr>
   <td>E-post</td>
   <td><p>Lägger till ett fält för att hämta e-postadressen. E-postkomponenten validerar som standard e-postadresser med följande reguljära uttryck.</p> <p><code>^[a-zA-Z0-9.!#$%&amp;'*+/=?^_&grave;{|}~-]+@[a-zA-Z0-9-]+(?:.[a-zA-Z0-9-]+)*$</code></p> </td>
  </tr>
  <tr>
   <td>Bifogad fil</td>
   <td><p>Lägger till en knapp som gör att användare kan bläddra bland och bifoga stöddokument till ett formulär. Du kan bifoga flera filer till en bifogad filkomponent. Du kan också ange **[!UICONTROL Maximum File Size]** och **[!UICONTROL Supported File Types]** för de bifogade filerna i egenskapswebbläsaren för komponenten. </p> <p><strong> Obs! </strong><ul> <li> Komponenten stöder inte bifogade filer med filnamn som börjar med tecken (.), som innehåller tecknen \ / : * ? " &lt; &gt; | ; % $, eller innehåller speciella filnamn som är reserverade för Windows-operativsystem som null, prn, con, lpt eller com. </li> <li> Om du vill bifoga flera filer till en bifogad fil som öppnas i webbläsaren Apple Safari markerar och bifogar du filer en i taget. Du kan inte markera och bifoga flera filer samtidigt.</li> <li>Komponenten Bifogad fil har stöd för en fördefinierad uppsättning filformat i adaptiva formulär som är aktiverade för Adobe Sign. Mer information finns i <a href="https://helpx.adobe.com/se/document-cloud/help/supported-file-formats-fill-sign.html#main-pars_text">Filformat som stöds</a>. </li></ul></p> </td>
  </tr>
  <tr>
   <td>Lista över bifogade filer</td>
   <td>Lägger till ett fält som visar alla bifogade filer som har överförts med komponenten Bifogad fil.</td>
  </tr>
  <tr>
   <td>Header<br /> </td>
   <td>Lägger till sidhuvudet som vanligtvis innehåller en företagslogotyp, formulärets rubrik och sammanfattning.<br /> </td>
  </tr>
  <tr>
   <td>Sidfot</td>
   <td>Lägger till sidfoten som vanligtvis innehåller copyrightinformation och länkar till andra sidor. </td>
  </tr>
  <tr>
   <td>Bild</td>
   <td>Infoga en bild.</td>
  </tr>
  <tr>
   <td>Bildval</td>
   <td>Gör att kunderna kan välja en bild som ger information. Ni kan använda informationen för att tillhandahålla personaliserade tjänster till era kunder.</td>
  </tr>
  <tr>
   <td>Knappen Nästa</td>
   <td>Lägger till en knapp för att navigera till nästa panel i ett formulär.</td>
  </tr>
  <tr>
   <td>Numerisk ruta</td>
   <td>Lägger till ett fält för att hämta numeriska värden</td>
  </tr>
  <tr>
   <td>Numerisk stege</td>
   <td>Använd Numeric Stepper i formuläret för att låta kunderna ange ett numeriskt värde som de kan öka eller minska baserat på ett fördefinierat steg.</td>
  </tr>
  <tr>
   <td>Panel</td>
   <td><p>Lägger till en panel eller underpanel.</p> <p>Du kan också lägga till en panelkomponent från den överordnade panelens verktygsfält med <span class="uicontrol">Lägg till underordnad panel</code> -knappen. På samma sätt kan du lägga till ett panelspecifikt verktygsfält med verktygsfältet <span class="uicontrol">Lägg till panel</code> -knappen. Du kan konfigurera placeringen av panelens verktygsfält med hjälp av dialogrutan Redigera panel.</p> </td>
  </tr>
  <tr>
   <td>Lösenordsruta</td>
   <td>Lägger till ett fält för att hämta ett lösenord.</td>
  </tr>
  <tr>
   <td>Knappen Föregående</td>
   <td>Lägger till en knapp som användare behöver för att gå tillbaka till föregående sida eller panel.</td>
  </tr>
  <tr>
   <td>Alternativknapp</td>
   <td>Lägger till alternativknappar.</td>
  </tr>
  <tr>
   <td>Knappen Återställ</td>
   <td>Lägger till en knapp för att återställa formulärfält.</td>
  </tr>
  <tr>
   <td>Spara-knapp</td>
   <td>Lägger till en knapp för att spara formulärdata.</td>
  </tr>
  <tr>
   <td>Klottra signaturer</td>
   <td>Lägger till ett fält för att hämta skriptsignaturer.</td>
  </tr>
  <tr>
   <td>Avgränsare</td>
   <td>Gör att panelerna i formuläret kan delas upp visuellt.</td>
  </tr>
  <tr>
   <td>Signatursteg</td>
   <td>Visar informationen i formuläret och signaturfälten som användaren kan verifiera och signera.</td>
  </tr>
  <tr>
   <td>Text</td>
   <td>Här kan du ange statisk text.</td>
  </tr>
  <tr>
   <td>Skicka-knapp</td>
   <td>Lägger till en skicka-knapp för att skicka formuläret till den konfigurerade skicka-åtgärden.</td>
  </tr>
  <tr>
   <td>Sammanfattningssteg</td>
   <td>Skickar formuläret och visar en sammanfattning som författarna anger när formuläret har skickats. </td>
  </tr>
  <tr>
   <td>Byt</td>
   <td>Lägger till en växel som utför en åtgärd för att växla eller aktivera/inaktivera. Du kan inte lägga till fler än två alternativ i komponenten Switch. Eftersom en switch bara kan ha två värden: På eller Av, är obligatoriskt inte tillämpligt. Minst ett värde sparas oavsett användarens indata. <br /> </td>
  </tr>
  <tr>
   <td>Tabell</td>
   <td>Lägger till en tabell där du kan ordna data i rader och kolumner. </td>
  </tr>
  <tr>
   <td>Telefonnummer</td>
   <td><p>Lägger till ett fält för att hämta telefonnummer. Med telefonkomponenten kan författare konfigurera någon av följande telefonnummertyper. Varje typ är associerad med ett reguljärt standarduttryck för validering.</p>
    <ul>
     <li>Typen International har validerats av <code>^[+][0-9]{0,14}$</code>.</li>
     <li>Typen USPhoneNumber har validerats av <code>{'+1 ('999') '999-9999}</code>.</li>
     <li>Typen UKPhoneNumber har validerats av <code>text{'+'99 999 999 9999}</code>.</li>
     <li>Typen Anpassad innehåller inget standardvalideringsmönster. Den får värdet för den senast valda telefonnummertypen. Du kan också ange ett eget valideringsmönster.</li>
    </ul> </td>
  </tr>
  <tr>
   <td>Villkor <br /> </td>
   <td>Lägger till ett fält som författare kan använda för att ange villkoren som användare ska granska innan formuläret fylls i.</td>
  </tr>
  <tr>
   <td>Textruta </td>
   <td><p>Lägger till en textruta där en användare kan ange nödvändig information. </p> <p>Komponenten Textruta accepterar som standard bara oformaterad text. Du kan aktivera en textrutekomponent för att acceptera RTF. En RTF-aktiverad textkomponent innehåller alternativ för att lägga till rubriker, ändra teckenformat (fet, kursiv, stryka under tecknen), skapa sorterade och osorterade listor, ändra textbakgrund och textfärg samt lägga till hyperlänkar. Aktivera alternativet <strong> Tillåt RTF </strong> i komponentegenskaperna om du vill aktivera RTF-text för en textruta.</p> </td>
  </tr>
  <tr>
   <td>Titel</td>
   <td>Anger en rubrik för det adaptiva formuläret.</td>
  </tr>
  <tr>
   <td>Verifiera steg</td>
   <td><p>Lägger till en platshållare som visar det ifyllda formuläret för verifiering av användaren.</p> <p><strong>Obs!</strong>: Adaptiv form som innehåller Verifiera-komponenten stöder inte anonyma användare. Du bör inte heller använda komponenten Verify i ett adaptivt formulärfragment.</p> </td>
  </tr>
 </tbody>
</table>

#### Bästa tillvägagångssätt vid arbete med komponenter {#best-practices}

Här följer några tips och viktiga saker du bör komma ihåg när du arbetar med adaptiva formulärkomponenter:

* Varje komponent har tillhörande egenskaper som styr dess utseende och funktion. Om du vill konfigurera egenskaperna för en komponent markerar du komponenten och väljer ![cmpr](assets/cmppr.png) för att öppna komponentegenskaperna i egenskapsläsaren.
* En komponent identifieras med sitt elementnamn. När du väljer ![cmpr](assets/cmppr.png) kan du ändra komponentens namn genom att ändra fältvärdet **[!UICONTROL Element Name]** i egenskapsläsaren. Endast bokstäver, siffror, bindestreck (-) och understreck (_) godkänns i fältet Elementnamn. Andra specialtecken tillåts inte och elementnamnet måste börja med en bokstav.

* Du kan ändra egenskapen Title för en adaptiv formulärkomponent infogad i formulärredigeraren utan att öppna egenskapsgranskaren så länge titeln visas i formuläret. Så här gör du:

   1. Välj det här alternativet om du vill markera en komponent som har en **[!UICONTROL Title]**-egenskap och vars **[!UICONTROL Hide title]**-egenskap är inaktiverad.

   1. Markera ![aem_6_3_edit](assets/aem_6_3_edit.png) om du vill göra titeln redigerbar.

   1. Ändra titeln och välj returtangenten eller markera var som helst utanför komponenten för att spara ändringarna. Markera Esc om du vill ignorera ändringarna.

* Vissa adaptiva formulärkomponenter som e-post och telefon innehåller färdiga valideringsmönster. Du kan dock ange anpassad validering genom att uppdatera fältet **[!UICONTROL Validation Pattern]** under dragspelsfliken Mönster i komponentegenskaperna. Mer information om standardvalideringar finns i komponentbeskrivningarna i tabellen ovan.

* Anpassningsbara formulärfält, t.ex. Numerisk ruta och E-post, kan konfigureras att innehålla speciella indatatyper för HTML5. När de här fälten är i fokus på mobila enheter och surfplattor visas särskilda alfabet, siffror och tecken som används ofta för att mata in information i fälten. Det gör det lättare för användarna att ange information snabbt utan att behöva växla mellan teckenuppsättningar på knappsatsen. Om du vill tillåta specialiserade indata för en komponent aktiverar du kryssrutan **[!UICONTROL Use HTML Type Number]** i komponentegenskaperna.

* Du kan aktivera en textrutekomponent för att acceptera RTF. Om du vill aktivera RTF-text för en textruta aktiverar du kryssrutan **[!UICONTROL Allow Rich Text]** i komponentegenskaperna.

* Du kan aktivera komponenterna Textruta, E-post och Telefon för att autofylla värden för fält som namn, adress, kreditkort, telefon och e-post från informationen som lagras i webbläsarens autofyllningsinställningar. Om du vill aktivera den här funktionen väljer du **[!UICONTROL Enable Autofill]** i komponentegenskaperna och väljer en **[!UICONTROL Autofill Attribute]**. När en användare fyller i ett anpassat formulär föreslås värdena från profilen för automatisk ifyllning i webbläsaren eller baserat på de värden som användaren tidigare fyllt i. Observera att Autofyll fungerar om autofyllningsinställningarna i användarens webbläsare är aktiverade.

* Ange värden för alternativknappar och kryssruteobjekt i formatet `{value}={text}` i komponentegenskaperna.
* Komponenten för bifogad fil tillåter som standard endast användare att bifoga en fil. Du kan dock konfigurera komponentegenskaperna så att de stöder flera bifogade filer. Om en användare dessutom bifogar flera filer med samma filnamn kan de bifogade filerna orsaka problem. Därför rekommenderar vi att du kopplar en unik identifierare till varje bifogad fil när formuläret skickas. Så här gör du:

   1. På din AEM Forms-server går du till **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.
   1. Sök efter och välj **[!UICONTROL Adaptive Forms Configuration Service]**.
   1. Aktivera **[!UICONTROL Make File Names Unique]** i dialogrutan Adaptiv Forms Configuration Service. Som standard är den inaktiverad.

* Om du vill att användare ska kunna koppla en PDF-fil med Safari-webbläsaren kontrollerar du att **application/pdf** har lagts till i egenskapen Filtyper som stöds i den bifogade filkomponenten. Anpassningsbara formulär som har skapats med en tidigare version av AEM Forms kan innehålla **.pdf** i stället för **application/pdf** i egenskapen Filtyper som stöds.

Mer information om adaptiva formulär finns i [Bästa tillvägagångssätt för att arbeta med adaptiva formulär](/help/forms/using/adaptive-forms-best-practices.md).

>[!NOTE]
>
>Anpassade formulärkomponenter har inte stöd för RTL-språk (right to left). Till exempel hebreiska.

### Verktygsfältet Sida {#page-toolbar}

Verktygsfältet längst upp på sidan innehåller alternativ som gör att du kan förhandsgranska formuläret, ändra formuläregenskaper och redigera formulärlayouten. Du kan förhandsgranska formuläret när du redigerar det och göra ändringar i det. I verktygsfältet visas:

* **Växla sidopanel** ![växlingspanel](assets/toggle-side-panel.png): Här kan du visa eller dölja sidopanel.

* **Sidinformation** ![temaalternativ](assets/theme-options.png): Här kan du visa sidegenskaper, publicera/avpublicera ett formulär, starta ett formulärarbetsflöde och öppna formuläret i klassiskt gränssnitt.

* **Emulator** ![linjal](assets/ruler.png): Gör att du kan emulera formulärutseendet för olika visningsstorlekar som surfplattor och telefoner.

* **Redigera**: Gör att du kan välja andra lägen, till exempel: **[!UICONTROL Edit]**, **[!UICONTROL Style]**, **[!UICONTROL Developer]** och **[!UICONTROL Design]**.

   * **Redigera**: Gör att du kan redigera egenskaperna för formuläret och dess komponenter. Du kan till exempel lägga till en komponent, släppa en bild och ange obligatoriska fält.
   * **Stil**: Gör att du kan formatera utseendet på komponenter i formuläret. I stilläge kan du till exempel markera en panel och ange dess bakgrundsfärg.

   * **Utvecklare**: Låter en utvecklare:

      * Upptäck vad formulären består av.
      * Felsök vad som händer var och när, vilket i sin tur hjälper till att lösa problem.

   * **Design**. Gör att du kan aktivera eller inaktivera anpassade komponenter eller komponenter som inte finns med i sidofältet.

* **Förhandsgranska**: Du kan förhandsgranska hur formuläret ser ut när du publicerar det.

### Komponentverktygsfältet {#component-toolbar}

![Komponentverktygsfältet i pekgränssnittet](assets/component-toolbar.png)

När du markerar en komponent visas ett verktygsfält där du kan arbeta med den. Du får alternativ för att klippa ut, klistra in, flytta och ange egenskaper för komponenterna. Dina alternativ är:

S.**Konfigurera**: När du väljer **[!UICONTROL Configure]** visas komponentegenskaperna i sidofältet. Om du konfigurerar dessa egenskaper kan du anpassa datainhämtningen. Du kan ändra komponentens elementnamn och ange etikettexten i komponentens rubrikfält. Med elementnamnet kan du hämta värden som användarna anger med komponenten. I komponentegenskaperna anger du komponentens beteende och hanterar användarindata. Konfigurera egenskaperna i sidofältet för att hämta användardata och använda dem för vidare bearbetning. Med egenskaper för adaptiv formulärbehållare kan du ange klientbibliotek, layouter, teman, inställningar för dokumentdokument, inställningar för att spara, inställningar för överföring och metadatainställningar.

B.**Kopiera**: Du kan använda kopieringsalternativet för att kopiera en komponent och klistra in den på andra platser i formuläret. När du klistrar in en komponent får den inklistrade komponenten ett nytt elementnamn men behåller den kopierade komponentens egenskaper.

C.**Klipp ut**: Du kan använda alternativet Klipp ut för att flytta en komponent från en plats till en annan i det adaptiva formuläret.

D. **Ta bort**: Du kan ta bort komponenten från formuläret.

E. **Infoga**: Gör att du kan infoga en komponent ovanför den markerade komponenten.

F. **Klistra in**: Gör att du kan klistra in komponenten som du klippt ut eller kopierat med alternativen som beskrivs ovan.

G. **Redigera regler**: Du kan öppna regelredigeraren. Mer information finns i [Regelredigeraren](../../forms/using/rule-editor.md).

H. **Grupp**: Gör att du kan markera flera komponenter om du vill klippa ut, kopiera eller klistra in mer än en komponent tillsammans.

I. **Överordnad**: Gör att du kan välja överordnad för en komponent. Ett textfält ligger till exempel i ett underavsnitt som finns i ett avsnitt. Avsnittet finns i stödlinjens rotpanel och den adaptiva formulärbehållaren är överordnad en stödlinjens rotpanel. För en komponent kan du se alla alternativ med hierarkin sorterad längst ned.

Om du till exempel väljer **[!UICONTROL Parent]** för en textruta kan du se:

* Underavsnitt
* Avsnitt
* guideRootPanel
* Adaptiv formulärbehållare

J. **Övriga**: Tillhandahåller fler alternativ för att arbeta med den markerade komponenten.

* Visa SOM-uttryck
* Spara en panel som fragment (endast för paneler)
* Lägg till underordnad panel (endast för paneler)
* Verktygsfältet Lägg till panel (endast för paneler)
* Ersätt (inte för paneler)

### Adaptiv formulärsida {#af-page}

Den anpassningsbara formulärsidan är det faktiska formuläret. Det är som alla andra WCM-sidor som modelleras som WCM `cq:Page`-komponenten. Följande bild visar innehållsstrukturen i ett typiskt anpassat formulär.

![Innehållsstruktur för en WCM-sida med anpassat formulär](assets/afstructure.png)

Innehållsstrukturen innehåller vanligtvis följande primära komponenter:

* **guideContainer**: Roten för ett adaptivt formulär, som markeras som **[!UICONTROL Start of adaptive form]** i gränssnittet för det adaptiva formuläret. I den här komponenten kan du ange:

   * *Mobil layout för det adaptiva formuläret*: Definierar formulärets utseende på mobila enheter.
   * *Tack!*: Definierar sidan där användaren omdirigeras efter att formuläret har skickats.
   * *Skicka åtgärd*: Definierar hur formuläret bearbetas på servern när användaren skickar formuläret.
   * *Format*: Anger sökvägen till CSS-filen som används för att anpassa formulärets utseende.

* **rootPanel:** Rotpanelen i ett adaptivt formulär. Den kan innehålla underpaneler under objektnoden. Varje panel, inklusive rotpanelen, kan ha en tillhörande layout. Panelens layout bestämmer hur formuläret placeras. I dragspelslayouten placeras till exempel objekten som dragspelssteg.

* **verktygsfält:** En adaptiv formulärbehållare har ett associerat globalt verktygsfält som är globalt för formuläret. Det här verktygsfältet kan läggas till med åtgärden **[!UICONTROL Add Toolbar]** i redigeringsfältet, som gör att författare kan lägga till åtgärder som Skicka, Spara, Återställ och så vidare.

* **resurser:** Den här noden innehåller ytterligare information som används för formulärredigering. Exempel: formulärmodellinformation, lokaliseringsinformation osv.).
