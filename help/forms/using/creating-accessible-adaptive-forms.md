---
title: Skapa hjälpmedelsförberedda adaptiva formulär
description: AEM Forms har verktyg och verktyg för att skapa hjälpmedelsförberedda, anpassningsbara blanketter som följer tillgänglighetsstandarder.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 8a0b276a-6020-4f48-95ab-4e7270e42e44
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2078'
ht-degree: 0%

---

# Skapa hjälpmedelsförberedda adaptiva formulär{#creating-accessible-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/sv/docs/experience-manager-core-components/using/adaptive-forms/introduction) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs ett äldre sätt att skapa adaptiva Forms med baskomponenter. </span>

## Introduktion {#introduction}

Ett hjälpmedelsanpassat formulär är ett formulär som alla kan använda, inklusive användare med särskilda behov. Adaptiva Forms har flera funktioner som gör dem mer användbara för användare med olika funktioner. Att skapa tillgänglighet i anpassningsbara formulär ger inte bara en så bred publik som möjligt, utan det är också ett krav när man levererar dokument i områden där det krävs överensstämmelse med tillgänglighetsstandarder. AEM Forms hjälper formulärutvecklare att följa tillgänglighetsstandarderna.

När du skapar ett anpassat formulär bör du tänka på följande när du skapar hjälpmedelsförberedda formulär:

* Kontrollera formuläret med hjälpmedelstestningsverktyget för namn och beskrivning (ANDI)
* Ange korrekta etiketter för formulärkontroller
* Ange textmotsvarigheter för bilder
* Ange tillräcklig färgkontrast
* Kontrollera att formulärkontrollerna är tangentbordstillgängliga

## Förutsättning

Du behöver ett hjälpmedelsverktyg som **hjälpmedels- och beskrivningsverktyg (ANDI)** och ett **anpassat formulärtema som utvecklats för att åtgärda tillgänglighetsrelaterade problem** för att skapa ett hjälpmedelsanpassat anpassat formulär.

### Hämta och installera hjälpmedelstestningsverktyget

Med verktyget för hjälpmedelsförberedda namn- och beskrivningsgranskare (ANDI) kan du identifiera och åtgärda tillgänglighetsrelaterade problem i webbinnehåll. Det rekommenderas enligt Trusted Tester v5 Guidelines of Department of Homeland Security. Det har utvecklats av &#x200B; för socialförsäkringstjänster i USA för att kontrollera att webbinnehållet uppfyller Section 508. Verktyget:

* Hjälper dig att identifiera tillgänglighetsproblem &#x200B; en webbsida
* Tillhandahåller förslag för att förbättra &#x200B;
* Identifierar problem med tangentbordstillgänglighet och färgkontrast
* Identifierar tydligt skärmläsarinnehållet enligt standarderna

ANDI fungerar med alla större webbläsare. Mer information om hur du konfigurerar och använder verktyget finns i [ANDI:s dokumentation](https://www.ssa.gov/accessibility/andi/help/install.html).

### Hämta och installera temat Ultramarine-Accessible

Temat Ultramarine-Accessible är ett referenstema. Det visar hur du åtgärdar färgkontrast och andra tillgänglighetsrelaterade problem i en adaptiv form. Adobe rekommenderar att du skapar ett anpassat tema för produktionsmiljön baserat på de format som din organisation har godkänt. Så här överför du temat till din AEM-instans:

1. Hämta temapaketet.
1. Navigera till **[!UICONTROL Experience Manager]** > **[!UICONTROL Navigation]** ![Navigering](assets/Smock_Compass_18_N.svg) > **[!UICONTROL Forms]** på din AEM-instans.
1. Välj **[!UICONTROL Create]** > **[!UICONTROL File Upload]**. Markera och överför filen x Ultramarine-Accessible-Theme.zip. Temat överförs till din AEM-instans.

## Göra ett anpassat formulär tillgängligt

Du bör fokusera på fyra nyckelaspekter: tangentbordsnavigering, färgkontrast, meningsfull alternativ text för bilder och lämpliga etiketter för formulärkontroller för att göra ett adaptivt formulär tillgängligt. Gör så här för att göra dina befintliga anpassningsbara formulär tillgängliga:

### 1. Använd ett tillgängligt tema och utför ytterligare korrigeringar

Använd temat Ultramarine-Accessible i din befintliga adaptiva form. Så här använder du temat:

1. Öppna det adaptiva formuläret för redigering.
1. Markera en komponent och välj den överordnade ikonen. Välj **[!UICONTROL Adaptive Form Container]** på snabbmenyn och välj sedan ikonen Konfigurera.
1. Välj det ultramarine-Accessible-temat i egenskapsläsaren och välj ikonen **[!UICONTROL Save]**.
1. Uppdatera webbläsarfönstret. Temat används på det anpassningsbara formuläret.

När du har tillämpat ett tillgängligt tema utför du följande korrigeringar. Förutom tillgänglighetskorrigeringar som ingår i det tillgängliga temat finns det korrigeringar:

1. Lägg till en meningsfull alternativ text för logotypbilden i det adaptiva formuläret.

   Ange en meningsfull alternativ text för bilder i sidhuvud- och sidfotskomponenter i den adaptiva formulärmallen. När du korrigerar mallen och använder den för att skapa ett anpassningsbart formulär ärver de adaptiva formulären alla tillgänglighetsrelaterade korrigeringar som tillämpas på mallens sidhuvud och sidfot.  Gör ändringar på adaptiv formulärnivå för ett befintligt adaptivt formulär. Ändringar som görs i en adaptiv formulärmall flödar inte automatiskt till ett befintligt adaptivt formulär.

1. Lägg till en rubrikkomponent som innehåller formulärnamn i det adaptiva formuläret. Om formulärdesignen anger ett företagsnamn lägger du även till en separat rubrikkomponent för företagsnamnet.

   De flesta tillgänglighetsverktygen informerar användarna om innehållets hierarki så att de lättare kan förstå webbsidans struktur. Ange olika rubriknivåer för organisationsnamn och formulärnamnstext i det adaptiva formuläret för att ge en hierarkisk struktur till dessa texter. Använd dessutom en textkomponent före varje panel och avsnitt med en lämplig rubriknivå för att skapa en hierarki.

   ![Använda ett rubrikformat](assets/apply-style.gif)

1. Ändra bakgrundsfärgen för sidfoten så att rätt kontrast används i enlighet med tillgänglighetsstandarderna för att förbättra textens synlighet och läsbarhet. Du kan använda ANDI för att hitta färgkontrastproblem i formuläret. Använd inte särskilt små teckensnitt. Små teckensnitt är svåra att läsa.

1. Byt ut växel- och bildvalskomponenterna i det befintliga adaptiva formuläret mot en urvalskomponent (radio).

1. Ersätt den numeriska nummerkomponenten i det befintliga adaptiva formuläret med en numerisk rutkomponent.

1. Ersätt datuminmatningsfält med datumväljarfält.

1. Ange visnings-, validerings- och redigeringsmönster för datumväljarkomponenten. Ange även ett eget valideringsfelmeddelande. Du har till exempel angett ett ogiltigt datum. Datumets korrekta format är YYY-MM-DD.

1. Ange anpassad hjälpmedelstext för datumväljarkomponenten. Ange t.ex. ditt födelsedatum. Skärmläsare läser dessa anpassade hjälpmedelstexter.

1. Använd kort beskrivning i stället för lång beskrivning för adaptiva formulärkomponenter. En lång beskrivning lägger till en hjälpknapp. Kontrollera att adaptiven inte har någon hjälpknapp.

1. Lägg till anpassad hjälpmedelstext i alla skrivskyddade celler i tabeller. Du kan även inaktivera alla skrivskyddade celler i tabeller.

1. Ta bort signaturfält för skript, om sådana finns i det adaptiva formuläret. Konfigurera det adaptiva formuläret för att använda Adobe Sign för en sömlös digital signeringsupplevelse.

### 2. Ange korrekta etiketter för formulärkontroller {#provide-proper-labels-for-form-controls}

Etiketten eller titeln för en komponent identifierar vad formulärkomponenten representerar. Texten &quot;Förnamn&quot; anger till exempel att användaren måste ange sitt förnamn i ett textfält. För att skärmläsare ska kunna komma åt etiketten är den programmatiskt kopplad till en formulärkomponent. Formulärkontrollen kan även konfigureras med ytterligare hjälpmedelsinformation.

Etiketten som upplevs av skärmläsare behöver inte vara samma som den visuella bildtexten. I vissa fall kanske du vill ange kontrollens syfte mer specifikt. För varje fältobjekt i ett formulär kan hjälpmedelsalternativen användas för att ange vad skärmläsaren ska meddela för att identifiera det specifika formulärfältet.

Så här använder du alternativet Hjälpmedel:

1. Markera en komponent och välj ![cmpr](assets/cmppr.png).
1. Klicka på **[!UICONTROL Accessibility]** i sidlisten för att välja önskat hjälpmedelsalternativ.

### Tillgänglighetsalternativ i formulärkomponenter {#accessibility-options-in-form-components}

![Hjälpmedelsalternativ i formulärkomponenter](assets/accessibility-options.png)

**Anpassad text** Formulärförfattare tillhandahåller innehållet i hjälpmedelsalternativet Egen text. Den här anpassade texten används i hjälpmedelstekniken, till exempel skärmläsare. Att använda inställningen Titel är det bästa alternativet i de flesta scenarier. Överväg att skapa Reader-text för anpassade skärmar endast när du inte kan använda rubriken eller en kort beskrivning.

**Kort beskrivning** För de flesta komponenter visas den korta beskrivningen vid körning när användaren placerar pekaren över komponenten. Du kan ange det här alternativet i fältet för kort beskrivning under alternativet för hjälpinnehåll.

**Titel** Använd det här alternativet om du vill att den visuella etikett som är associerad med formulärfältet ska kunna användas som skärmläsartext i AEM Forms.

**Namn** Du kan ange ett värde i fältet Namn på fliken Bindning. Namnet får inte innehålla blanksteg.

**Ingen** Om du väljer Ingen får formulärobjektet inget namn i det publicerade formuläret. Ingen rekommenderas inte för formulärkontroller.

>[!NOTE]
>
>* Alternativknapp och kryssruta kan bara ha två alternativ för tillgänglighet, nämligen Anpassad text och Titel.
>* För XFA-baserade adaptiva formulär ärvs hjälpmedelsalternativet från de hjälpmedelsalternativ som angetts i XDP. Verktygstips från XDP mappas till Short Description och Caption mappas till Title. De andra alternativen fungerar som de är.

### 3. Ange textmotsvarigheter för bilder {#provide-text-equivalents-for-images}

Bilder kan förbättra förståelsen för vissa användare. För användare som använder skärmläsare minskar dock bilderna formulärets tillgänglighet. Om du väljer att använda bilder anger du textbeskrivningar för alla bilder.

Kontrollera att texten beskriver objektet och dess syfte i formuläret. En skärmläsare läser upp den här alternativa texten när en bild påträffas. En bild måste alltid ha en alternativ text angiven.

Markera en bildkomponent och välj ![cmpr](assets/cmppr.png). Ange alternativ text för en bild under Egenskaper i sidlisten.

![Alternativ text för en bild](assets/image-properties.png)

### 4. Ange tillräcklig färgkontrast {#provide-sufficient-color-contrast}

Hjälpmedelsdesignen inbegriper att överväga ytterligare riktlinjer för färganvändning. Formulärförfattare kan använda färger för att förbättra formulärens utseende genom att markera olika formulärkomponenter. En felaktig användning av färg kan dock göra ett formulär svårt eller omöjligt att läsa av personer med olika funktioner.

Användare med nedsatt syn förlitar sig på hög kontrast mellan text och bakgrunden för att läsa digitalt innehåll. Utan tillräcklig kontrast kan ett formulär bli svårt, för att inte säga omöjligt, att läsa för vissa användare.

Vi rekommenderar att du använder standardfärgerna för teckensnitt och bakgrund - innehåll i svart färg mot en vit bakgrund. Om du ändrar standardfärgerna väljer du antingen en mörk förgrundsfärg på en ljus bakgrundsfärg, eller omvänt.

Mer information om hur du ändrar färgkontrast och tema för anpassade formulär finns i [Skapa anpassade teman för anpassade formulär](/help/forms/using/creating-custom-adaptive-form-themes.md).

### 5. Kontrollera att formulärkontrollerna är tangentbordstillgängliga {#ensure-that-form-controls-are-keyboard-accessible}

Ett hjälpmedelsanpassat formulär kan fyllas i helt och hållet med bara tangentbordet eller en motsvarande indataenhet. Användare med nedsatt rörelseförmåga eller nedsatt syn har kanske inget annat val än att använda tangentbordet och många användare som kan använda en mus föredrar tangentbordsinmatning. Genom att använda de olika indatametoderna kan du inte bara skapa hjälpmedelsförberedda formulär, du kan också skapa formulär som bättre passar alla användares önskemål.

Följande kortkommandon finns i AEM Forms.

| Åtgärd | Kortkommando |
|---|---|
| Flytta markören framåt genom ett formulär | Tabb |
| Flytta markören bakåt genom ett formulär | Skift+Tabb |
| Flytta till nästa panel | Alt+högerpil |
| Flytta till föregående panel | Alt+vänsterpil |
| Återställ ifyllda data i ett formulär | Alt+R |
| Skicka ett formulär | Alt+S |

Dessutom finns det olika kortkommandon för komponenten **[!UICONTROL Date Picker]** i Adaptive Forms. Om du vill aktivera kortkommandona markerar du komponenten **[!UICONTROL Date Picker]** och väljer ![Konfigurera](assets/configure-icon.svg) för att öppna egenskaperna. I avsnittet **[!UICONTROL Patterns]** väljer du ett visningsmönster med listrutorna **[!UICONTROL Type]** och **[!UICONTROL Pattern]**. Spara egenskaperna för att aktivera användning av kortkommandon för komponenten **[!UICONTROL Date Picker]**.

Följande kortkommandon är tillgängliga för datumväljarkomponenten i Adaptiv Forms:

| Åtgärd | Kortkommando |
|---|---|
| <ul><li>Visa komponentalternativen för datumväljaren när tabbfokus markerar kalenderikonen</li><li>Utför klickhändelsen när tabbfokus markerar ett alternativ</li> | Blanksteg eller Retur |
| Dölj komponentalternativen för datumväljaren | Esc |
| <ul><li>Flytta markören framåt genom de alternativ som är tillgängliga i datumväljarkomponenten.</li><li>Ange flikfokus för kalenderikonen när datuminmatningsfältet är aktivt</li> | Tabb |
| Flytta markören bakåt genom de alternativ som är tillgängliga i datumväljarkomponenten | Skift+Tabb |
| <ul><li>Visa komponentalternativen för datumväljaren när tabbfokus markerar datuminmatningsfältet</li><li>Flytta markören nedåt i kalendern som är tillgänglig i datumväljarkomponenten</li> | Nedåtpil |
| Flytta markören uppåt i kalendern som är tillgänglig i datumväljarkomponenten | Uppåtpil |
| Flytta markören bakåt i kalendern som är tillgänglig i datumväljarkomponenten | Vänsterpil |
| Flytta markören framåt i kalendern som är tillgänglig i datumväljarkomponenten | Högerpil |
| Utför åtgärden för den bildtext som är tillgänglig mellan höger och vänster navigeringspilar i kalendern | Skift + uppåtpil |
| Utför åtgärden för högernavigeringspilikonen ![högerpil](assets/right-navigation-icon.svg) som är tillgänglig i kalendern | Skift + Vänsterpil |
| Utför åtgärden för den vänstra navigeringspilikonen ![vänsterpil](assets/left-navigation-icon.svg) som är tillgänglig i kalendern | Skift + högerpil |

## Använd hjälpmedelsverktyget för att hitta återstående tillgänglighetsproblem

Med hjälp av ANDI (Accessible Name and Description Inspector) kan du identifiera och åtgärda kompatibilitetsrelaterade problem i en adaptiv form. Så här använder du ANDI-verktyget för att hitta hjälpmedelsproblemen i en adaptiv form:

1. Öppna det adaptiva formuläret i förhandsgranskningsläge.
1. Klicka på den bokmärkta ikonen för verktyget ANDI. ANDI-verktyget analyserar det anpassningsbara formuläret och visar tillgänglighetsproblem. Mer information om hur du använder verktyget finns i [ANDI:s dokumentation](https://www.ssa.gov/accessibility/andi/help/howtouse.html).
1. Granska och åtgärda de problem som rapporterats av ANDI.
