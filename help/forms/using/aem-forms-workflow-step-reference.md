---
title: Forms-centrerat arbetsflöde i OSGi - stegreferens
description: Med ett Forms-baserat arbetsflöde i OSGi-steg kan du snabbt skapa anpassningsbara formulärbaserade arbetsflöden.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Foundation Components
role: User, Developer
exl-id: 13d84b04-dab6-453f-bc0d-62a5f557c4f2
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '7485'
ht-degree: 0%

---

# Forms-centrerat arbetsflöde i OSGi - stegreferens {#forms-centric-workflow-on-osgi-step-reference}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/forms/create-form-centric-workflows/aem-forms-workflow-step-reference).

Du använder arbetsflödesmodeller för att konvertera en affärslogik till en automatiserad repetitiv process. En modell hjälper dig att definiera och köra en serie steg. Du kan också definiera modellegenskaper, t.ex. om arbetsflödet är tillfälligt eller använder flera resurser. Du kan [inkludera olika AEM Workflow-steg i en modell för att uppnå affärslogiken](/help/sites-developing/workflows-models.md#extending-aem).

## Forms Workflow Steps {#forms-workflow-steps}

Forms Workflow steg utför AEM Forms-specifika åtgärder i ett AEM-arbetsflöde. Med dessa steg kan du snabbt skapa anpassningsbara formulärbaserade Forms-baserade arbetsflöden i OSGi. Dessa arbetsflöden kan användas för att utveckla enkla arbetsflöden för granskning och godkännande, interna och övergripande affärsprocesser. Du kan också använda Forms Workflow steg för att starta dokumenttjänster, integrera med signaturarbetsflödet i Adobe Sign och utföra andra AEM Forms-åtgärder.

Forms-baserade arbetsflödessteg utför AEM Forms-specifika åtgärder i ett AEM-arbetsflöde. Med de här stegen kan du snabbt skapa anpassningsbara Forms-baserade Forms-baserade arbetsflöden i OSGi. Dessa arbetsflöden kan användas för att utveckla enkla arbetsflöden för granskning och godkännande, interna och övergripande affärsprocesser.

>[!NOTE]
>
>Om arbetsflödesmodellen är markerad för ett externt lagringsutrymme kan du för alla Forms Workflow-steg bara välja variabelalternativet för att lagra eller hämta datafiler och bilagor.

## Tilldela aktivitetssteg {#assign-task-step}

Tilldela ett uppgiftssteg skapar en uppgift och tilldelar den till en användare eller grupp. Förutom att tilldela uppgiften anger komponenten även det adaptiva formuläret eller icke-interaktiva PDF för uppgiften. Det adaptiva formuläret krävs för att kunna ta emot indata från användare och icke-interaktiva PDF eller ett skrivskyddat anpassat formulär används endast för granskning.

Du kan också använda komponenten för att styra aktivitetens beteende. Du kan till exempel skapa ett automatiskt postdokument, tilldela uppgiften till en viss användare eller grupp, ange sökvägen till skickade data, ange sökvägen till data som ska fyllas i i förväg och ange standardåtgärder. Tilldela uppgift-steget har följande egenskaper:

* **Titel:** Aktivitetens namn. Titeln visas i AEM Inbox.
* **Beskrivning:** Förklaring av de åtgärder som utförs i aktiviteten. Den här informationen är användbar för andra processutvecklare när du arbetar i en delad utvecklingsmiljö.

* **Miniatyrbildssökväg:** Sökväg till aktivitetsminiatyrbilden. Om ingen sökväg anges visas en standardminiatyrbild för ett anpassat formulär och en standardikon visas för Dokument för post.
* **Arbetsflödesfas:** Ett arbetsflöde kan ha flera steg. Dessa steg visas i AEM Inbox. Du kan definiera dessa steg i modellens egenskaper (Sidekick > Sida > Sidegenskaper > Steg).
* **Prioritet:** Den valda prioriteten visas i AEM Inbox. De tillgängliga alternativen är Hög, Medium och Låg. Standardvärdet är Medium.
* **Förfallodatum:** Ange antalet dagar eller timmar efter vilka aktiviteten har markerats som försenad. Om du väljer **Av** markeras aktiviteten aldrig som försenad. Du kan också ange en uttidshanterare för att utföra vissa åtgärder när åtgärden är försenad.

* **Dagar:** Antalet dagar innan aktiviteten ska slutföras. Antalet dagar räknas efter att uppgiften har tilldelats en användare. Om en uppgift inte är slutförd och korsar det antal dagar som anges i fältet Dagar, aktiveras en timeout-hanterare efter förfallodatumet, om detta väljs.
* **Timmar:** Antalet timmar innan aktiviteten ska slutföras. Antalet timmar räknas efter att uppgiften har tilldelats en användare. Om en uppgift inte är slutförd och korsar det antal timmar som anges i fältet Timmar aktiveras en timeout-hanterare efter de timmar som ska förfalla.
* **Tidsgräns efter förfallodatum:** Välj det här alternativet om du vill aktivera markeringsfältet för Timeout-hanterare.
* **Timeout-hanterare:** Välj skriptet som ska köras när tilldelningsaktivitetssteget överskrider förfallodatumet. Skript som placeras i CRX-databasen på [apps]/fd/dashboard/scripts/timeoutHandler är tillgängliga för val. Den angivna sökvägen finns inte i crx-databasen. En administratör skapar sökvägen innan den används.
* **Markera åtgärden och kommentera från den senaste aktiviteten i Uppgiftsinformation:** Välj det här alternativet om du vill visa den senaste åtgärden som utfördes och kommentaren som togs emot i aktivitetsinformationsavsnittet för en aktivitet.
* **Typ:** Välj vilken typ av dokument som ska fyllas i när arbetsflödet startas. Du kan välja ett adaptivt formulär, ett skrivskyddat adaptivt formulär, ett icke-interaktivt PDF-dokument, ett gränssnitt för interaktiv kommunikationsagent eller ett dokument för interaktiv kommunikationskanal.
* **Använd adaptiv form:** Ange metoden för att hitta indataadaptiv form. Det här alternativet är tillgängligt om du väljer Adaptivt formulär eller Skrivskyddat anpassat formulär i listrutan Typ. Du kan använda det adaptiva formuläret som skickas till arbetsflödet, som finns på en absolut sökväg eller som finns på en sökväg i en variabel. Du kan använda en variabel av typen String för att ange sökvägen.\
  Du kan koppla flera adaptiva formulär till ett arbetsflöde. Det innebär att du kan ange ett anpassningsbart formulär i körningsmiljön med hjälp av de tillgängliga indatametoderna.

* **Använd interaktiv kommunikation:** Ange metoden för att hitta den interaktiva kommunikationen för indata. Du kan använda den interaktiva kommunikationen som skickas till arbetsflödet, som finns på en absolut sökväg eller som finns på en sökväg i en variabel. Du kan använda en variabel av typen String för att ange sökvägen. Det här alternativet är tillgängligt om du väljer gränssnittet Interactive Communication Agent eller Interactive Communication Web Channel Document i listrutan Typ.

>[!NOTE]
>
>Du måste ha grupptilldelningar för cm-agent-users och arbetsflödesanvändare för att få tillgång till gränssnittet för Interactive Communications Agent i AEM Inbox.

* **Adaptiv form eller Interaktiv kommunikationssökväg**: Ange sökvägen för det adaptiva formuläret eller den interaktiva kommunikationen. Du kan använda det adaptiva formuläret eller den interaktiva kommunikationen som skickas till arbetsflödet, som finns på en absolut sökväg, eller hämta det adaptiva formuläret från en sökväg som lagras i en variabel av strängdatatyp.
* **Välj indata-PDF med:** Ange sökvägen för ett icke-interaktivt PDF-dokument. Fältet är tillgängligt när du väljer ett icke-interaktivt PDF-dokument i typfältet. Du kan välja indata-PDF med hjälp av sökvägen som är relativ till nyttolasten, sparad med en absolut sökväg eller med hjälp av en variabel av datatypen Document. Exempel: [Payload_Directory]/Workflow/PDF/credit-card.pdf. Sökvägen finns inte i crx-databasen. En administratör skapar sökvägen innan den används. Du måste ha alternativet Dokument för post aktiverat eller formulärmallsbaserade adaptiva formulär för att kunna använda alternativet PDF Path.
* **Återge det adaptiva formuläret som** för slutförd åtgärd: När en åtgärd har markerats som slutförd kan du återge det adaptiva formuläret som ett skrivskyddat adaptivt formulär eller ett PDF-dokument. Du måste ha alternativet Dokument i post aktiverat eller formulärmallsbaserade adaptiva formulär för att kunna återge det adaptiva formuläret som Dokument i post.
* **Förifyllt:** Följande fält i listan nedan fungerar som indata för uppgiften:

   * **[!UICONTROL Select input data file using]**: Sökväg till indatafil (.json, .xml, .doc eller formulärdatamodell). Du kan hämta indatafilen med en sökväg som är relativ till nyttolasten eller hämta filen som lagras i en variabel av datatypen Document, XML eller JSON. Filen innehåller till exempel data som skickats för formuläret via ett AEM Inbox-program. En exempelsökväg är [Payload_Directory]/workflow/data.

   * **Välj indatabilagor med hjälp av:** Bifogade filer på platsen bifogas till formuläret som är associerat med uppgiften. Sökvägen kan vara relativ till nyttolasten eller hämta de bilagor som lagras i en variabel av typen ArrayList of Document. En exempelsökväg är [Payload_Directory]/attachments/. Du kan ange bifogade filer som placeras i förhållande till nyttolasten eller använda en dokumenttypsvariabel (Array list > Document) för att ange en bifogad indatafil för det adaptiva formuläret.

      * **Välj JSON-indata:** Välj en JSON-indatafil med en sökväg som är relativ till nyttolasten eller som lagras i en variabel av datatypen Document, JSON eller Form Data Model. Det här alternativet är tillgängligt om du väljer gränssnittet Interactive Communication Agent eller Interactive Communication Web Channel Document i listrutan Typ.
      * **Välj en anpassad förifyllningstjänst:** Välj förifyllningstjänsten för att hämta data och förifyll dokumentet för den interaktiva kommunikationskanalen eller agentens användargränssnitt.
      * **Använd förifyllningstjänsten för den interaktiva kommunikation som valts ovan:** Använd det här alternativet om du vill använda förifyllningstjänsten för den interaktiva kommunikation som är definierad i listrutan Använd interaktiv kommunikation.
      * **Mappning av attribut för begäran:** Använd Mappning av attribut för begäran för att definiera [namnet och värdet för attributet ](../../forms/using/work-with-form-data-model.md#bindargument) för begäran. Hämta informationen från datakällan baserat på attributnamnet och värdet som anges i begäran. Du kan definiera ett attributvärde för begäran med hjälp av ett literalt värde eller en variabel av datatypen String.\
        Alternativen för förifyllningstjänst och attributmappning för begäran är bara tillgängliga om du väljer Interactive Communication Agent UI eller Interactive Communication Web Channel Document i listrutan Typ.

* **Skickad information:** Följande fält i listan nedan fungerar som utdataplatser för aktiviteten:

   * **Spara utdatafilen med:** Spara datafilen (.json,). xml, .doc eller formulärdatamodell). Datafilen innehåller information som skickas via det associerade formuläret. Du kan spara utdatafilen med en sökväg som är relativ till nyttolasten eller lagra den i en variabel av datatypen Document, XML eller JSON. Exempel: [Payload_Directory]/Workflow/data, där data är en fil.
   * **Spara bifogade filer med:** Spara de bifogade formulärfilerna som finns i en uppgift. Du kan spara de bifogade filerna med en sökväg som är relativ till nyttolasten eller lagra den i en variabel av dokumentdatatypen.
   * **Spara postdokument med:** Sökväg för att spara en postfil. Exempel: [Payload_Directory]/DocumentofRecord/credit-card.pdf. Du kan spara postdokumentet med en sökväg som är relativ till nyttolasten eller lagra det i en variabel av dokumentdatatypen. Om du väljer alternativet **Relativt till nyttolast** genereras inte postdokumentet om sökvägsfältet lämnas tomt. Det här alternativet är bara tillgängligt om du väljer Adaptivt formulär i listrutan Typ.

   * **Spara webbkanaldata med:** Spara webbkanaldatafilen med en sökväg som är relativ till nyttolasten eller lagra den i en variabel av datatypen Document, JSON eller Form Data Model. Det här alternativet är bara tillgängligt om du väljer gränssnittet för interaktiv kommunikationsagent i listrutan Typ.
   * **Spara PDF-dokument med:** Spara PDF-dokumentet med en sökväg som är relativ till nyttolasten eller lagra det i en variabel av dokumentdatatypen. Det här alternativet är bara tillgängligt om du väljer gränssnittet för interaktiv kommunikationsagent i listrutan Typ.
   * **Spara layoutmallen med:** Spara layoutmallen med en sökväg som är relativ till nyttolasten eller lagra den i en variabel av dokumentdatatypen. [layoutmallen](../../forms/using/layout-design-details.md) refererar till en XDP-fil som du skapar med Forms Designer. Det här alternativet är bara tillgängligt om du väljer gränssnittet för interaktiv kommunikationsagent i listrutan Typ.

* **Tilldelad > Tilldelningsalternativ:** Ange den metod som ska användas för att tilldela en användare uppgiften. Du kan dynamiskt tilldela uppgiften till en användare eller en grupp med skriptet för deltagarväljaren eller tilldela uppgiften till en viss AEM-användare eller grupp.
* **Deltagarväljare:** Alternativet är tillgängligt när alternativet **Dynamiskt för en användare eller grupp** är markerat i fältet Tilldela alternativ. Du kan använda ett ECMAScript eller en tjänst för att dynamiskt välja en användare eller grupp.

* **Deltagare:** Fältet är tillgängligt när alternativet **[!UICONTROL com.adobe.granite.workflow.core.process.RandomParticipantChooser]** har valts i fältet **Deltagarväljare**. I fältet kan du välja användare eller grupper för alternativet RandomParticipantChooser.

* **Tilldelad:** Fältet är tillgängligt när **[!UICONTROL com.adobe.fd.workspace.step.service.VariableParticipantChooser]** har valts i fältet **Deltagarväljare**. I fältet kan du välja en variabel av datatypen String för att definiera den som tilldelas.

* **Argument:** Fältet är tillgängligt när ett annat skript än skriptet RandomParticipantChoose har valts i fältet Deltagarväljare. I fältet kan du ange en lista med kommaseparerade argument för det skript som valts i fältet Deltagare.

* **Användare eller grupp:** Uppgiften tilldelas till vald användare eller grupp. Alternativet är tillgängligt när alternativet **Till en viss användare eller grupp** har valts i fältet **Tilldela alternativ**. I fältet visas alla användare och grupper i gruppen för arbetsflödesanvändare.\
  I listrutan **Användare eller Grupp** visas de användare och grupper som den inloggade användaren har åtkomst till. Hur användarnamn visas beror på om du har åtkomstbehörighet till noden **users** i crx-databasen för den aktuella användaren.

* **[!UICONTROL Send Notification Email]**: Välj det här alternativet om du vill skicka e-postmeddelanden till den som tilldelats. Dessa meddelanden skickas när en uppgift tilldelas en användare eller grupp. Du kan använda alternativet **[!UICONTROL Recipient Email Address]** för att ange vilken mekanism som ska användas för att hämta e-postadressen.

* **[!UICONTROL Recipient Email Address]**: Du kan lagra e-postadresser i en variabel, använda en litteral för att ange en permanent e-postadress eller använda e-postadressen som är standard för den tilldelades som anges i den tilldelades profil. Du kan använda literalen eller en variabel för att ange en grupps e-postadress. Variabelalternativet är användbart när du dynamiskt vill hämta och använda en e-postadress. Alternativet **[!UICONTROL Use default email address of the assignee]** är endast för en tilldelad. I det här fallet används den e-postadress som lagras i användarprofilen för tilldelningar.

* **HTML e-postmall**: Välj e-postmall för e-postmeddelandet. Om du vill redigera en mall ändrar du filen på /libs/fd/dashboard/templates/email/htmlEmailTemplate.txt i crx-databasen.
* **Tillåt delegering till:** AEM Inbox ger den inloggade användaren möjlighet att delegera det tilldelade arbetsflödet till en annan användare. Du får delegera inom samma grupp eller till arbetsflödesanvändaren i en annan grupp. Om uppgiften har tilldelats en enskild användare och alternativet **Tillåt delegering till medlemmar i den tilldelade gruppen** har valts går det inte att delegera uppgiften till en annan användare eller grupp.
* **Delningsinställningar:** AEM Inbox innehåller alternativ för att dela en eller alla uppgifter i inkorgen med andra användare:
   * När alternativet **Tillåt att tilldelad delar explicit i inkorg** är markerat kan användaren klicka på uppgiften och dela den med en annan AEM-användare.
   * När alternativet **Tillåt att en tilldelad delar via inkorg** är markerat och en användare delar sina inkorgsobjekt eller tillåter andra användare att få åtkomst till inkorgsobjekt delas endast uppgifter med andra användare som har det här alternativet aktiverat.

* **Åtgärder > Standardåtgärder:** Utanför rutan finns åtgärderna Skicka, Spara och Återställ. Som standard är alla standardåtgärder aktiverade.
* **Vägvariabel:** Vägvariabelns namn. Vägvariabeln hämtar anpassade åtgärder som en användare väljer i AEM Inbox.
* **Vägar:** En aktivitet kan grenas till olika vägar. När du väljer det här alternativet i AEM Inbox returneras ett värde och arbetsflödesgrenarna baserat på det valda flödet. Du kan antingen lagra vägar i en variabel av datatypen String eller välja **Literal** om du vill lägga till vägar manuellt.

* **Rubrik**: Ange ruttens titel. Den visas i AEM Inbox.
* **Korallikon**: Ange HTML-attribut för en korallikon. Adobe CorelUI-biblioteket innehåller en mängd ikoner som sätter fokus först. Du kan välja och använda en ikon för rutten. Den visas tillsammans med titeln i AEM Inbox. Om du lagrar rutterna i en variabel använder rutterna en taggikon.
* **Tillåt att den som tilldelats kan lägga till kommentarer**: Välj det här alternativet om du vill aktivera kommentarer för aktiviteten. En tilldelad kan lägga till kommentarerna inifrån AEM Inbox när uppgiften skickas.
* **Spara kommentar i variabel:** Spara kommentaren i en variabel av datatypen String. Det här alternativet visas bara om du markerar kryssrutan **Tillåt att den som tilldelats kan lägga till kommentarer**.

* **Tillåt att den som tilldelas kan lägga till bifogade filer i aktiviteten**: Välj det här alternativet om du vill aktivera bifogade filer för aktiviteten. En tilldelad kan lägga till de bifogade filerna inifrån AEM Inbox när uppgiften skickas.
* **Spara bifogade utdataaktiviteter med**: Ange platsen för den bifogade mappen. Du kan spara bilagor för utdatauppgifter med en sökväg som är relativ till nyttolasten eller med en variabel av dokumentdatatypen. Det här alternativet visas bara om du markerar kryssrutan **Tillåt att den som tilldelas kan lägga till bifogade filer i aktiviteten** och väljer **Anpassat formulär**, **Skrivskyddat anpassat formulär** eller **Icke-interaktivt PDF-dokument** i listrutan **Typ** på fliken **Formulär/Dokument** .

>[!NOTE]
>
>Använd fliken Bifogade filer i agentanvändargränssnittet under körning för att koppla de bifogade filerna till en interaktiv kommunikation. De associerade bifogade filerna visas som bifogade uppgifter i sidosparken när arbetsobjektet har öppnats i ett fullständigt läge.

* **Använd anpassade metadata:** Välj det här alternativet om du vill aktivera det anpassade metadatafältet. Anpassade metadata används i e-postmallar.
* **Anpassade metadata:** Välj anpassade metadata för e-postmallarna. Anpassade metadata är tillgängliga i crx-databaser på apparna/fd/dashboard/scripts/metadataScripts. Den angivna sökvägen finns inte i crx-databasen. En administratör skapar sökvägen innan den används. Du kan också använda en tjänst för anpassade metadata. Du kan också utöka gränssnittet WorkitemUserMetadataService för att tillhandahålla anpassade metadata.
* **Visa data från föregående steg**: Välj det här alternativet om du vill att tilldelningar ska kunna visa tidigare tilldelningar, åtgärder som redan har vidtagits för uppgiften, kommentarer som har lagts till i uppgiften och dokument med information om den slutförda uppgiften, om tillgängligt.
* **Visa data från efterföljande steg:** Välj det här alternativet om du vill att den aktuella tilldelaren ska kunna visa den åtgärd som har utförts och de kommentarer som har lagts till i uppgiften av efterföljande tilldelningar. Den aktuella personen kan även visa ett dokument med uppgifter om den slutförda uppgiften, om det är tillgängligt.
* **Synlighet för datatyp:** Som standard kan en tilldelad visa ett dokument för registrering, tilldelningar, vidtagna åtgärder och kommentarer som tidigare och efterföljande tilldelningar har lagt till. Använd datatypsalternativet för synlighet för att begränsa vilken typ av data som är synlig för de tilldelade.

>[!NOTE]
>
>Alternativen för att spara steget Tilldela uppgift som utkast och för att hämta historiken för steget Tilldela uppgift inaktiveras när du konfigurerar en [!DNL Adobe Experience Manager]-arbetsflödesmodell för extern datalagring. I Inkorgen är dessutom alternativet att spara inaktiverat.

## Skicka e-poststeg {#send-email-step}

Använd e-poststeget för att skicka ett e-postmeddelande, till exempel ett e-postmeddelande med ett arkivdokument, en länk till ett anpassat formulär, en länk till en interaktiv kommunikation eller med ett bifogat PDF-dokument. Skicka e-post-steget stöder [HTML-e-post](https://en.wikipedia.org/wiki/HTML_email). HTML e-postmeddelanden är responsiva och anpassar sig efter mottagarnas e-postklient och skärmstorlek. Du kan använda en e-postmall från HTML för att definiera utseendet, färgschemat och beteendet för e-postmeddelandet.

I e-poststeget används Day CQ Mail Service för att skicka e-postmeddelanden. Kontrollera att [e-posttjänsten](../../forms/using/aem-forms-workflow.md) har konfigurerats innan du använder e-poststeget. E-poststeget har följande egenskaper:

**Titel:** Steget är en titel som hjälper dig att identifiera steget i arbetsflödesredigeraren.

**Beskrivning:** Förklaring är användbar för andra processutvecklare när du arbetar i en delad utvecklingsmiljö.

**Ämne för e-post:** Ämne kan hämtas från ett arbetsflödes-metadata, anges manuellt eller hämtas från värdet som lagras i en variabel. Välj bland följande alternativ:

* **Litteral -** Ange ett ämne manuellt.
* **Hämta från arbetsflödesmetadata** - Hämta ämnet från en metadataegenskap.
* **Variabel** - Hämta ämnet från värdet som lagras i en variabel av strängdatatyp.

**HTML-e-postmall**: HTML-mall för e-postmeddelandet. Du kan ange variabler i en e-postmall. E-poststeget extraheras och visar alla variabler som ingår i en mall för indata.

**Metadata för e-postmall:** Värdet för e-postmallvariablerna kan vara ett användarspecificerat värde, sökvägen till en resurs på författaren eller på publiceringsservern, bilden eller en metadataegenskap för arbetsflöde.

* **Literal:** Använd alternativet när du vet exakt vilket värde som ska anges. Exempel: [example@example.com](mailto:example@example.com).

* **Metadata för arbetsflöde:** Använd alternativet när värdet som ska användas sparas i en metadataegenskap för arbetsflöde. När du har valt alternativet anger du metadataegenskapens namn i den tomma textrutan under alternativet Metadata för arbetsflöde. Till exempel emailAddress.
* **Resurs-URL:** Använd alternativet för att bädda in en webblänk för en interaktiv kommunikation i e-postmeddelandet. När du har valt alternativet bläddrar du och väljer den interaktiva kommunikation som ska bäddas in. Resursen kan finnas på författaren eller på publiceringsservern.
* **Bild:** Använd alternativet för att bädda in en bild i e-postmeddelandet. När du har valt alternativet bläddrar du och väljer bilden. Bildalternativet är bara tillgängligt för de bildtaggar (&lt;img src=&quot;&#42;&quot;/>) som är tillgängliga i e-postmallen.

**Avsändarens/mottagarens e-postadress:** Välj alternativet **Literal** om du vill ange en e-postadress manuellt eller välj alternativet **Hämta från arbetsflödesmetadata** om du vill hämta e-postadressen från en metadataegenskap. Du kan också ange en lista med egenskapsmatriser för metadata för alternativet **Hämta från arbetsflödesmetadata**. Välj alternativet **Variabel** om du vill hämta e-postadressen från värdet som lagras i en variabel av strängdatatyp.

**Bifogad fil:** Resursen som är tillgänglig på den angivna platsen bifogas till e-postmeddelandet. Resursens sökväg kan vara relativ till nyttolasten eller den absoluta sökvägen. En exempelsökväg är [Payload_Directory]/attachments/.

Välj alternativet **Variabel** om du vill hämta den bifogade filen som lagras i en variabel av datatypen Document, XML eller JSON.

**Filnamn:** E-postbilagefilens namn. E-poststeget ändrar det ursprungliga filnamnet för den bifogade filen till det angivna filnamnet. Namnet kan anges manuellt eller hämtas från en metadataegenskap eller variabel i arbetsflödet. Använd alternativet **Literal** när du vet exakt vilket värde som ska anges. Använd alternativet **Variabel** för att hämta filnamnet från värdet som lagras i en variabel av strängdatatyp. Använd alternativet **Hämta från arbetsflödesmetadata** när värdet som ska användas sparas i en metadataegenskap för arbetsflöde.

## Generera dokumentarkivhandlingssteget {#generate-document-of-record-step}

När ett formulär fylls i eller skickas kan du spara en post med formuläret, i utskrift eller i dokumentformat. Detta kallas DOS (Document of Record). Du kan använda steget Skapa dokument för post för att skapa en skrivskyddad eller interaktiv PDF-version av ett anpassat formulär. PDF-versionen innehåller information som fylls i i formuläret tillsammans med layouten för det adaptiva formuläret.

Dokumentsteget har följande egenskaper:

**Använd adaptiv form**: Ange metoden för att hitta det adaptiva indataformuläret. Du kan använda det adaptiva formuläret som skickas till arbetsflödet, som finns på en absolut sökväg eller som finns på en sökväg i en variabel. Du kan använda en variabel av datatypen String för att ange sökvägen i **Select-variabeln som ska tolkas**.\
Du kan koppla flera adaptiva formulär till ett arbetsflöde. Det innebär att du kan ange ett anpassningsbart formulär i körningsmiljön med hjälp av de tillgängliga indatametoderna.

**Adaptiv formulärsökväg**: Ange sökvägen för det adaptiva formuläret. Fältet är tillgängligt när du väljer alternativet **Tillgängligt på en absolut sökväg** i fältet **Använd anpassat formulär**.

**Välj indata med:** Sökväg för indata för det adaptiva formuläret. Du kan behålla data på en plats i förhållande till nyttolasten, ange en absolut sökväg till data eller hämta data som lagras i en variabel av datatypen Document, JSON eller XML. Indata sammanfogas med det adaptiva formuläret för att skapa ett postdokument.

**Välj sökväg för bifogad indata med:** Sökväg för de bifogade filerna. De här bifogade filerna ingår i dokumentdokumentet. Du kan behålla de bifogade filerna på en plats i förhållande till nyttolasten, ange en absolut sökväg för de bifogade filerna eller hämta bifogade filer som lagras i en variabel av dokumentdatatypen.

Om du anger sökvägen till en mapp, till exempel bilagor, bifogas alla filer som är direkt tillgängliga i mappen till Dokument för post. Om det finns filer i de mappar som är direkt tillgängliga i den angivna sökvägen inkluderas filerna i Postdokument som bilagor. Om det finns mappar i direkt tillgängliga mappar hoppas de över.

**Spara genererat postdokument med följande alternativ:** Ange platsen där ett dokument med en postfil ska sparas. Du kan välja att skriva över nyttolastmappen, placera postdokumentet på en plats i nyttolastkatalogen eller lagra postdokumentet i en variabel av dokumentdatatypen.

**Språk**: Ange språket för postens dokument. Välj **Litteral** om du vill välja språkområde från en nedrullningsbar lista eller välj **Variabel** om du vill hämta språkområdet från det värde som lagras i en variabel av strängdatatyp. Definiera språkkoden medan värdet för språkinställningen lagras i en variabel. Ange till exempel **en_US** för engelska och **fr_FR** för franska.

## Anropa tjänststeg för formulärdatamodell {#invoke-form-data-model-service-step}

Du kan använda [AEM Forms-dataintegrering](../../forms/using/data-integration.md) för att konfigurera och ansluta till olika datakällor. Dessa datakällor kan vara en databas-, webbtjänst-, REST-tjänst-, OData-tjänst- och CRM-lösning. Med AEM Forms dataintegrering kan du skapa en formulärdatamodell som omfattar olika tjänster som utför datahämtnings-, additions- och uppdateringsåtgärder för den konfigurerade databasen. Du kan använda steget **Anropa datamodelltjänst** för att välja en formulärdatamodell (FDM) och använda tjänsterna i FDM för att hämta, uppdatera eller lägga till data till olika datakällor.

Följande databastabell och JSON-filen används som exempel för att förklara indata för stegfält:

**Exempel på CustomerDetails-tabell**

<table>
 <tbody> 
  <tr> 
   <td>Egenskap</td> 
   <td>Värde <br /> </td> 
  </tr> 
  <tr> 
   <td>FirstName<br /> </td> 
   <td>Sarah<br /> </td> 
  </tr> 
  <tr> 
   <td>LastName</td> 
   <td>ros</td> 
  </tr> 
  <tr> 
   <td>Kund-ID</td> 
   <td>1</td> 
  </tr> 
  <tr> 
   <td>E-postadress <br /> </td> 
   <td>srose@we.info</td> 
  </tr> 
 </tbody> 
</table>

**JSON-exempelfil**

```json
  { 
    customer: { 
     firstName: "Sarah", 
     lastName:"Rose", 
     customerId: "1", 
     emailAddress:"srose@we.info" 
   }, 
    insurance: {
     customerId: "1", 
    policyType: "Premium,
    policyNumber: "Premium-521499",
    customerDetails: { 
     firstName: "Sarah",
     lastName: "Rose",
     customerId: "1",
     emailAddress: "srose@we.info" 
    }
   }
  }
```

I steget Anropa formulärdatamodelltjänst visas följande fält för att underlätta formulärdatamodellåtgärder:

* **Titel:** Stegen är titel. Det hjälper till att identifiera steget i arbetsflödesredigeraren.
* **Beskrivning:** Förklaring är användbar för andra processutvecklare när du arbetar i en delad utvecklingsmiljö.

* **Sökväg till formulärdatamodell**: Bläddra och välj en formulärdatamodell som finns på servern.

* **Tjänst**: Lista över tjänster som den valda formulärdatamodellen tillhandahåller.
* **Indata för tjänster > Ange indata med hjälp av litteral-, variabel- eller arbetsflödesmetadata och en JSON-fil**: En tjänst kan ha flera argument. Välj alternativet för att hämta värdet för tjänstargumenten från en metadataegenskap för arbetsflöde, ett JSON-objekt, en variabel eller ange värdet direkt i textrutan:

   * **Literal:** Använd alternativet när du vet exakt vilket värde som ska anges. Exempel: srose@we.info.
   * **Variabel:** Använd alternativet för att hämta värdet som lagras i en variabel.
   * **Hämta från arbetsflödesmetadata:** Använd alternativet när värdet som ska användas sparas i en metadataegenskap för arbetsflöde. Till exempel emailAddress.
   * **[!UICONTROL Relative to Payload]**: Använd alternativet för att hämta den bifogade filen som har sparats på en sökväg i förhållande till nyttolasten. Markera alternativet och ange antingen mappnamnet som innehåller den bifogade filen eller ange namnet på den bifogade filen i textrutan.

     Om till exempel mappen Relativt till nyttolast i CRX-databasen innehåller en bifogad fil på platsen `attachment\attachment-folder` anger du `attachment\attachment-folder` i textrutan efter att du har valt alternativet **[!UICONTROL Relative to Payload]**.
   * **JSON-punktnotation:** Använd alternativet när värdet som ska användas finns i en JSON-fil. Till exempel försäkring.customerDetails.emailAddress. Alternativet JSON-punktnotation är bara tillgängligt om du har valt alternativet Mappa inmatningsfält från JSON-inmatningsalternativet.
   * **Mappa indatafält från JSON-indata:** Ange sökvägen till en JSON-fil för att få indatavärdet för vissa tjänstargument från JSON-filen. Sökvägen till JSON-filen kan vara relativ till nyttolasten, en absolut sökväg eller så kan du välja ett JSON-inmatningsdokument med hjälp av variabeln JSON eller Form Data Model.

* **Indata för tjänster > Ange indata med variabel eller JSON-fil:** Välj alternativet för att hämta värden för alla argument från en JSON-fil som har sparats med en absolut sökväg, en sökväg som är relativ till nyttolast eller i en variabel.
* **Välj Indata-JSON-dokument med**: JSON-filen som innehåller värden för alla tjänstargument. JSON-filens sökväg kan vara **relativ till nyttolasten** eller en absolut **-sökväg.** Du kan även hämta JSON-indatadokumentet med hjälp av en variabel av datatypen JSON eller Form Data Model.

* **JSON-punktnotation:** Lämna fältet tomt om du vill använda alla objekt i den angivna JSON-filen som indata för tjänstargument. Om du vill läsa ett specifikt JSON-objekt från den angivna JSON-filen som indata för serviceargument anger du punktnotation för JSON-objektet, till exempel, om du har en JSON som liknar den som anges i början av avsnittet, anger du försäkring.customerDetails för att ge all information om en kund som indata till tjänsten.
* **Utdata för tjänsten > Mappa och skriv utdatavärden till variabel eller metadata:** Välj alternativet att spara utdatavärdena som egenskaper för arbetsflödesinstansens metadatanod i crx-databasen. Ange namnet på metadataegenskapen och välj det motsvarande tjänstutdataattribut som ska mappas med metadataegenskapen, till exempel mappa det telefonnummer som returneras av utdatatjänsten med egenskapen phone_number för arbetsflödets metadata. På samma sätt kan du lagra utdata i en variabel med datatypen Long. När du väljer en egenskap för alternativet **[!UICONTROL Service output attribute to be mapped]** fylls endast variabler som kan lagra data för den valda egenskapen i för alternativet **[!UICONTROL Save the output to]**.

* **Utdata från tjänst > Spara utdata till variabel eller en JSON-fil:** Välj alternativet att spara utdatavärdena i en JSON-fil med en absolut sökväg, med en sökväg som är relativ till nyttolast eller i en variabel.
* **Spara utdata-JSON-dokument med följande alternativ:** Spara utdata-JSON-filen. Sökvägen till JSON-utdatafilen kan vara relativ till nyttolasten eller en absolut sökväg. Du kan också spara JSON-utdatafilen med en variabel av datatypen JSON eller Form Data Model.

## Underteckna dokumentsteg {#sign-document-step}

I steget Signera dokument kan du använda Adobe Sign för att signera dokument. Stegen Signera dokument har följande egenskaper:

* **Avtalsnamn:** Ange avtalets titel. Avtalsnamnet blir en del av ämnet och brödtexten i det e-postmeddelande som skickas till mottagarna. Du kan antingen lagra namnet i en variabel av datatypen String eller välja **Literal** om du vill lägga till namnet manuellt.

* **Språk:** Ange språk för alternativen för e-post och verifiering. Du kan antingen lagra språkinställningen i en variabel av datatypen String eller välja **Literal** för att välja språkinställningen i listan med tillgängliga alternativ. Definiera språkkoden medan värdet för språkinställningen lagras i en variabel. Ange till exempel **en_US** för engelska och **fr_FR** för franska.

* **Adobe Sign-molnkonfiguration**: Välj en Adobe Sign Cloud-konfiguration. Om du inte har konfigurerat Adobe Sign för AEM Forms kan du läsa [Integrera Adobe Sign med AEM Forms](../../forms/using/adobe-sign-integration-adaptive-forms.md).

* **Välj dokument som ska signeras med:** Du kan välja ett dokument från en plats som är relativ till nyttolasten, använda nyttolasten som dokument, ange en absolut sökväg för dokumentet eller hämta dokumentet som lagras i en variabel av dokumentdatatypen.


* **Välj sökväg för bifogad fil med:** Sökväg för de bifogade filerna. Dessa bilagor inkluderas i signeringsdokumentet. Du kan behålla de bifogade filerna på en plats i förhållande till nyttolasten, ange en absolut sökväg för de bifogade filerna eller hämta bifogade filer som lagras i en variabel av dokumentdatatypen.


  Om du anger sökvägen till en mapp, till exempel bilagor, bifogas alla filer som är direkt tillgängliga i mappen till Signera dokument. Om några filer är tillgängliga i de mappar som är direkt tillgängliga i den angivna sökvägen för bifogade filer, inkluderas filerna i Signera dokument som bifogade filer. Om det finns mappar i direkt tillgängliga mappar hoppas de över.

* **Dagar till deadline:** Ett dokument markeras som förfallet (passerad deadline) efter det att aktiviteten inte har någon aktivitet för det antal dagar som anges i fältet **Dagar till deadline**. Antalet dagar räknas efter att den dokumenterade har tilldelats en användare för signering.
* **E-postfrekvens för påminnelse:** Du kan skicka en påminnelse via e-post varje dag eller vecka. Veckan räknas från den dag dokumentet tilldelas en användare för signering.
* **Signaturprocess:** Du kan välja att signera ett dokument i en sekventiell eller parallell ordning. I sekventiell ordning tar en mottagare emot dokumentet i taget för signering. När den första mottagaren har slutfört signeringen av dokumentet skickas dokumentet till den andra mottagaren och så vidare. Flera mottagare kan signera ett dokument i taget i parallell ordning.
* **URL för omdirigering:** Ange en URL för omdirigering. När dokumentet har signerats kan du dirigera om den som tilldelats till en URL. Oftast innehåller denna URL ett tackmeddelande eller ytterligare instruktioner.
* **Arbetsflödesfas:** Ett arbetsflöde kan ha flera steg. Dessa steg visas i AEM Inbox. Du kan definiera dessa steg i modellens egenskaper (Sidekick > Sida > Sidegenskaper > Steg).
* **Välj mottagare:** Ange metoden för att välja mottagare för dokumentet. Du kan dynamiskt tilldela arbetsflödet till en användare eller en grupp eller manuellt lägga till information om en mottagare. När du väljer Manuellt i listrutan lägger du till mottagarinformation som e-post, roll och autentiseringsmetod.

  >[!NOTE]
  >
  >* I rollavsnittet kan du ange mottagarrollen som signerare, godkännare, godkännare, certifierad mottagare, formulärifyllare och delegerande.
  >* Om du väljer Delegerande i alternativet Roll kan delegeraren tilldela signeringsaktiviteten till en annan mottagare.
  >* Om du har konfigurerat en autentiseringsmetod för [!DNL Adobe Sign], baserat på din konfiguration, väljer du en autentiseringsmetod som telefonbaserad autentisering, social identitet, kunskapsbaserad autentisering, myndighetsidentitetsbaserad autentisering.


* **Skript eller tjänst för att välja mottagare:** Alternativet är bara tillgängligt om du väljer alternativet Dynamiskt i fältet Välj mottagare. Du kan ange ett ECMAScript eller en tjänst för att välja mottagare och verifieringsalternativ för ett dokument.
* **Mottagarinformation:** Alternativet är bara tillgängligt om alternativet Manuellt har valts i fältet Välj mottagare. Ange e-postadress och välj en valfri verifieringsmekanism. Innan du väljer en verifieringsmekanism i två steg ska du kontrollera att motsvarande verifieringsalternativ är aktiverat för det konfigurerade Adobe Sign-kontot. Du kan använda en variabel av datatypen String för att definiera värden för fälten **[!UICONTROL Email]**, **[!UICONTROL Country Code]** och **[!UICONTROL Phone Number]**. Fälten **[!UICONTROL Country Code]** och **[!UICONTROL Phone Number]** visas bara om du väljer **[!UICONTROL Phone Verification]** i listrutan **[!UICONTROL 2-step verification]**.
* **Statusvariabel:** Ett Adobe Sign-aktiverat dokument lagrar dokumentets signeringsstatus i en variabel med datatypen String. Ange namnet på statusvariabeln (adobeSignStatus). En statusvariabel för en instans finns i CRXDE på /etc/workflow/instances/&lt;server>/&lt;datum-tid>/&lt;instans av arbetsflödesmodell>/workItems/&lt;nod>/metaData innehåller status för en variabel.
* **[!UICONTROL Signed Document]**: Du kan spara statusen för det signerade dokumentet till Variabel. Om du vill lägga till granskningsspår för elektroniska signaturer för bättre säkerhet och laglighet i det signerade dokumentet kan du inkludera revideringsrapport. Du kan spara det signerade dokumentet med hjälp av en variabel- eller nyttolastmapp.
  >[!NOTE]
  >
  > Revideringsrapporten läggs till den sista sidan i det signerade dokumentet.
<!--
* **Save signed document using below options:** Specify the location to keep signed documents. You can choose to overwrite the payload file, place the signed document at a location within the payload directory, or store the signed document in a variable of Document type.
-->

## Steg för Document Services {#document-services-steps}

AEM Document Services är en uppsättning tjänster för att skapa, sammanställa och skydda PDF-dokument. AEM Forms tillhandahåller ett separat AEM Workflow-steg för varje dokumenttjänst.

På samma sätt som andra AEM Forms-arbetsflödessteg, som Tilldela uppgift, Skicka e-post och Signera dokument, kan du använda variabler i alla steg i AEM Document Services. Mer information om att skapa och hantera variabler finns i [Variabler i AEM-arbetsflöden](../../forms/using/variable-in-aem-workflows.md).

### Använd tidsstämpelsteg för dokument {#apply-document-time-stamp-step}

Lägg till tidsstämpel i ett dokument. Du kan ange dokumentinformation, t.ex. indatadokumentsökväg, indatadokumentnamn och plats där exporterade data ska lagras. Du kan välja att skriva över en befintlig nyttolastfil, välja ett annat filnamn för att lagra data i en annan fil under nyttolastmappen, ange en absolut sökväg till data eller lagra data i en variabel av dokumentdatatypen.

### Konvertera till bildsteg {#convert-to-image-step}

Konverterar ett PDF-dokument till en lista med bilder. Bildformat som stöds är JPEG, JPEG2000, PNG och TIFF. Följande information gäller för konvertering till TIFF-bilder:

* En flersidig TIFF-fil skapas.
* Vissa anteckningar ingår inte i TIFF-bilder. Anteckningar som kräver att Acrobat genererar sitt utseende inkluderas inte.

### Konvertera till PDF/A {#convert-to-pdf-a-step}

Konverterar ett PDF-dokument till PDF/A-format med de alternativ som anges. PDF/A-versionen av PDF (Portable Document Format) är avsedd för arkivering och långtidsarkivering av dokument.

### Konvertera till PS-steg {#convert-to-ps-step}

Konvertera PDF-dokument till PostScript. När du konverterar till PostScript kan du använda konverteringsåtgärden för att ange källdokumentet och om det ska konverteras till PostScript nivå 2 eller 3. Det PDF-dokument som du konverterar till en PostScript-fil måste vara icke-interaktivt.

### Skapa PDF från angivet textsteg {#create-pdf-from-specified-type-step}

Generera ett PDF-dokument från en indatafil. Indatadokumentet kan vara relativt till nyttolasten, ha en absolut sökväg, vara nyttolasten själv eller lagras i en variabel av datatypen Document.

### Skapa PDF från URL/HTML/ZIP {#create-pdf-from-url-html-zip-step}

Skapar ett PDF-dokument av den angivna URL-, HTML- och ZIP-filen.

### Steget Exportera data {#export-data-step}

Exporterar data från en PDF forms- eller XDP-fil. Du måste ange filsökvägen för Input Document och Export Data Format. Alternativen för Exportera dataformat är Auto, XDP och XmlData.

### Export PDF till angivet textsteg {#export-pdf-to-specified-type-step}

Konverterar ett PDF-dokument till ett valt format.

### Generera icke-interaktivt PDF-steg {#generatenoninteractive}

Skapa en icke-interaktiv PDF. Här finns olika anpassningsalternativ.

>[!NOTE]
>
>Du kan använda variabler för att ange mallfilen för indatadokument. Lagra sökvägen till mallfilen i en variabel av datatypen String.

### Importera datasteg {#import-data-step}

Sammanfogar formulärdata till ett PDF-formulär. Du kan importera formulärdata till ett PDF-formulär.

### Anropa DDX-steg {#invokeddx}

Kör DDX-filen på den angivna kartan över indatadokument och returnerar de manipulerade PDF-dokumenten.

>[!NOTE]
>
>Du kan använda variabler för att ange DDX-filen för indatadokument. Lagra DDX-filen i en variabel av dokumentdatatypen eller XML-datatypen.

### Optimera PDF-steget {#optimize-pdf-step}

Optimerar PDF-filer genom att minska deras storlek. Resultatet av konverteringen är PDF-filer som kan vara mindre än originalversionerna. Den här åtgärden konverterar även PDF-dokument till den PDF-version som anges i optimeringsparametrarna.

Optimeringsinställningarna anger hur filerna optimeras. Här följer några exempelinställningar:

* PDF-målversion
* Ignorera objekt som JavaScript-åtgärder och inbäddade sidminiatyrer
* Ignorera användardata som kommentarer och bifogade filer
* Ignorerar ogiltiga eller oanvända inställningar
* Komprimera okomprimerade data eller använda mer effektiva komprimeringsalgoritmer
* Ta bort inbäddade teckensnitt
* Ange genomskinlighetsvärden

### Återge PDF Form step {#renderpdf}

Återger ett formulär som har skapats i Form Designer (XDP) till ett PDF-formulär.

>[!NOTE]
>
>Du kan använda variabler för att ange mallfilen för indatadokument. Lagra sökvägen till mallfilen i en variabel av datatypen String.

### Säkra dokument, steg {#secure-document-step}

Kryptera, signera och certifiera ett dokument. AEM Forms stöder både lösenordsbaserad kryptering och certifikatbaserad kryptering. Du kan också välja mellan olika algoritmer för signering av dokument. Exempel: SHA-256 och SH-512. Du kan också använda arbetsflödessteget för att läsa utökade PDF-dokument. Arbetsflödessteget innehåller alternativ för att aktivera streckkodsavkodning, digitala signaturer, import och export av PDF-data och andra alternativ.

### Skicka till skrivare, steg {#send-to-printer-step}

Skicka ett dokument direkt till en skrivare. Det har stöd för följande åtkomstmekanismer för utskrift:

* **Direkttillgänglig skrivare**: En skrivare som är installerad på samma dator kallas för en direkt tillgänglig skrivare och datorn kallas för skrivarvärd. Den här typen av skrivare kan vara en lokal skrivare som är ansluten direkt till datorn.
* **Indirekt tillgänglig skrivare**: Den skrivare som är installerad på en utskriftsserver är tillgänglig från andra datorer. Teknik som det gemensamma utskriftssystemet UNIX® (CUPS) och protokollet Line Printer Daemon (LPD) är tillgängliga för anslutning till en nätverksskrivare. Om du vill få åtkomst till en indirekt tillgänglig skrivare anger du utskriftsserverns IP-adress eller värdnamn. Med den här funktionen kan du skicka ett dokument till en LPD-URI när nätverket har ett LPD-program öppet. Med hjälp av den här funktionen kan du dirigera dokumentet till en skrivare som är ansluten till nätverket som har ett LPD-program öppet.

### Generera utskriftssteg {#generatePrintedOutput}

Steget genererar ett PCL-, PostScript-, ZPL-, IPL-, TPCL- eller DPL-utdata som genereras av en formulärdesign och datafil. Datafilen sammanfogas med formulärdesignen och formateras för utskrift. De utdata som genereras i det här steget kan skickas direkt till en skrivare eller sparas som en fil. Vi rekommenderar att du använder det här steget när du vill använda formulärdesigner eller data från ett program. Om dina formulärdesigner eller formulärdesigner finns i nätverket, det lokala filsystemet eller HTTP-platsen använder du åtgärden generatePrintedOutput.

Programmet kräver till exempel att du sammanfogar en formulärdesign med en datafil. Informationen innehåller hundratals poster. Dessutom krävs att utdata skickas till en skrivare som stöder ZPL. Formulärdesignen och dina indata finns i ett program. Använd åtgärden generatePrintedOutput för att sammanfoga varje post med en formulärdesign och skicka utdata till en skrivare som stöder ZPL.

Stegen Generera utskrift har följande egenskaper:

**Indataegenskaper**

* **[!UICONTROL Select template file using]**: Ange sökvägen för mallfilen. Du kan välja mallfilen med hjälp av sökvägen som är relativ till nyttolasten, sparad med en absolut sökväg eller med hjälp av en variabel av datatypen Dokument. Exempel: [Payload_Directory]/Workflow/data.xml. Om sökvägen inte finns i crx-databasen kan en administratör skapa sökvägen innan den används. Dessutom kan du acceptera nyttolast som indatafil.

* **[!UICONTROL Select data document using]**: Ange sökvägen till en indatafil. Du kan markera indatafilen med den sökväg som är relativ till nyttolasten, som har sparats med en absolut sökväg eller med en variabel av dokumentdatatypen. Exempel: [Payload_Directory]/Workflow/data.xml. Om sökvägen inte finns i crx-databasen kan en administratör skapa sökvägen innan den används.

* **[!UICONTROL Printer Format]**: Ett värde för utskriftsformat som anger vilket sidbeskrivningsspråk som ska användas när ingen XDC-fil anges för att generera utdataströmmen. Om du anger ett literalt värde väljer du något av följande värden:

   * **[!UICONTROL Custom PCL]**: Använd alternativet för att ange en anpassad XDC-fil för PCL.
   * **[!UICONTROL Custom PostScript]**: Använd alternativet för att ange en anpassad XDC-fil för PostScript.
   * **[!UICONTROL Custom ZPL]**: Använd alternativet för att ange en anpassad XDC-fil för ZPL.
   * **[!UICONTROL Generic Color PCL (5c)]**: Använd en allmän färg-PCL (5c).
   * **[!UICONTROL Generic PostScript Level3]**: Använd allmän PostScript Level 3.
   * **[!UICONTROL ZPL 300 DPI]**: Använd ZPL 300 DPI. Zpl300.xdc används.
   * **[!UICONTROL ZPL 600 DPI]**: Använd ZPL 600 DPI. Filen zpl600.xdc används.
   * **[!UICONTROL Custom IPL]**: Använd alternativet för att ange en anpassad XDC-fil för IPL.
   * **[!UICONTROL IPL 300 DPI]**: Använd IPL 300 DPI. ipl300.xdc används.
   * **[!UICONTROL IPL 400 DPI]**: Använd IPL 400 DPI. Filen ipl400.xdc används.
   * **[!UICONTROL Custom TPCL]**: Använd alternativet för att ange en anpassad XDC-fil för TPCL.
   * **[!UICONTROL TPCL 305 DPI]**: Använd TPCL 300 DPI. Filen tpcl305.xdc används.
   * **[!UICONTROL PCL 600 DPI]**: Använd TPCL 600 DPI. Filen tpcl600.xdc används.
   * **[!UICONTROL Custom DPL]**: Använd alternativet för att ange en anpassad DPL för XDC-fil.
   * **[!UICONTROL DPL300DPI]**: Använd DPL 300 DPI. Filen dpl300.xdc används.
   * **[!UICONTROL DPL406DPI]**: Använd DPL 400 DPI. dpl406.xdc används.
   * **[!UICONTROL DPL600DPI]**: Använd DPL 600 DPI. dpl600.xdc används.

**Utdataegenskaper**

* **[!UICONTROL Save output document using]**: Ange platsen där utdatafilen ska sparas. Du kan spara utdatafilen på en plats som är relativ till nyttolasten, i en variabel eller ange en absolut plats att spara utdatafilen på. Om sökvägen inte finns i crx-databasen kan en administratör skapa sökvägen innan den används.

**Avancerade egenskaper**

* **[!UICONTROL Select Content Root location using]**: Innehållsroten är ett strängvärde som anger URI, absolut referens eller plats i databasen för att hämta relativa resurser som används i formulärdesignen. Om formulärdesignen till exempel refererar till en bild relativt, som ../myImage.gif, måste myImage.gif finnas på repository://. Standardvärdet är repository://, som pekar på databasens rotnivå.

  När du väljer en resurs från ditt program måste innehållsrots-URI-sökvägen ha rätt struktur. Om ett formulär till exempel hämtas från ett program med namnet SampleApp och placeras på SampleApp/1.0/forms/Test.xdp, måste innehållets rot-URI anges som repository://administrator@password/Applications/SampleApp/1.0/forms/, eller databasen:/Applications/SampleApp/1.0/forms/ (när behörigheten är null). När innehållets rot-URI anges på det här sättet kommer sökvägarna för alla refererade resurser i formuläret att matchas mot denna URI.

* **[!UICONTROL Select XCI file using]**: XCI-filer används för att beskriva teckensnitt och andra egenskaper som används för formulärdesignelement. Du kan behålla en XCI-fil i förhållande till nyttolasten, på en absolut sökväg eller med en variabel av dokumentdatatypen.

* **[!UICONTROL Locale]**: Anger vilket språk som ska användas för att generera PDF-dokumentet. Om du anger ett literalt värde väljer du ett språk i listan eller något av dessa värden:
   * **Så här använder du serverstandard**:
(Standard) Använd språkinställningen som är konfigurerad på AEM Forms-servern. Inställningen Språk konfigureras med administrationskonsolen. (Se [Hjälp om Designer](https://www.adobe.com/go/learn_aemforms_designer_65).)

   * **Så här använder du det anpassade värdet**:
Skriv språkkoden i den litterala rutan eller välj en strängvariabel som innehåller språkkoden. En fullständig lista över språkkoder som stöds finns på https://java.sun.com/j2se/1.5.0/docs/guide/intl/locale.doc.html.

* **[!UICONTROL Copies]**: Ett heltalsvärde som anger antalet kopior som ska genereras för utdata. Standardvärdet är 1.

* **[!UICONTROL Duplex Printing]**: Ett sidnumreringsvärde som anger om dubbelsidig utskrift eller enkelsidig utskrift ska användas. Skrivare som stöder PostScript och PCL använder det här värdet. Om du anger ett literalt värde väljer du något av följande värden:
   * **[!UICONTROL Duplex Long Edge]**: Använd dubbelsidig utskrift och utskrift med hjälp av sidnumrering med lång kant.
   * **[!UICONTROL Duplex Short Edge]**: Använd dubbelsidig utskrift och utskrift med hjälp av kortkantssidnumrering.
   * **[!UICONTROL Simplex]**: Använd enkelsidig utskrift.
