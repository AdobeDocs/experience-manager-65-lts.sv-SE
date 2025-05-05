---
title: Integrering med Adobe Creative Cloud bästa praxis
description: Bästa tillvägagångssätt för att integrera [!DNL Adobe Experience Manager] med [!DNL Adobe Creative Cloud] för att effektivisera arbetsflöden för resursöverföring och uppnå hög innehållshastighet.
contentOwner: AG
mini-toc-levels: 1
role: User, Admin
feature: Collaboration,Adobe Asset Link,Desktop App
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 51a2f4bb-5fca-48fa-855d-1d610a5eb7c0
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3173'
ht-degree: 9%

---

# [!DNL Adobe Experience Manager] och [!DNL Creative Cloud] bästa praxis för integrering {#aem-and-creative-cloud-integration-best-practices}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/assets/manage/aem-cc-integration-best-practices) |
| AEM 6.5 | Den här artikeln |

[!DNL Adobe Experience Manager Assets] är en DAM-lösning (Digital Asset Management) som kan integreras med [!DNL Adobe Creative Cloud] för att hjälpa DAM-användare att samarbeta med kreativa team och effektivisera samarbetet när innehåll skapas.

[!DNL Adobe Creative Cloud] förser kreativa team med ett ekosystem av lösningar och tjänster som hjälper dem att skapa digitala resurser. Det omfattar dator- och mobilprogram, molntjänster som lagring med skrivbordssynkronisering eller webbupplevelse samt marknadsplatser som [!DNL Adobe Stock].

Läs vidare för att ta reda på vilka integreringar som du ska välja mellan stationär dator och företagsspecifik DAM baserat på ditt användningsfall och vilka metoder som är associerade med de sammankopplade arbetsflödena.

>[!NOTE]
>
>Mappdelning mellan [!DNL Experience Manager] och [!DNL Creative Cloud] är föråldrad och ingår inte längre i den här guiden. Adobe rekommenderar att du använder nyare funktioner som [Adobe Asset Link](https://helpx.adobe.com/se/enterprise/using/adobe-asset-link.html) eller [Experience Manager skrivbordsapp](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/troubleshoot.html?lang=sv-SE) för att ge kreativa användare åtkomst till resurser som hanteras i [!DNL Experience Manager].

## Collaboration behov för kreatörer, marknadsförare och DAM-användare {#collaboration-needs-of-creatives-marketers-and-dam-users}

| Krav | Använd skiftläge | Involverade ytor |
|---|---|---|
| Förenkla för kreatörer på datorn | Effektivisera åtkomsten till resurser från ett DAM-system ([!DNL Experience Manager Assets]) för kreatörer, eller mer allmänt för användare på datorer som arbetar i program för att skapa interna resurser. De behöver ett enkelt och enkelt sätt att identifiera, använda (öppna), redigera och spara ändringar i [!DNL Experience Manager] och överföra nya filer. | Windows eller Mac desktop; [!DNL Creative Cloud] appar |
| Tillhandahåll högkvalitativa, färdiga resurser från [!DNL Adobe Stock] | Marknadsförarna hjälper till att snabba upp processen för att skapa innehåll genom att hjälpa till med materialanskaffning och identifiering. Kreatörer använder det godkända materialet direkt inifrån sina kreativa verktyg. | [!DNL Experience Manager Assets]; [!DNL Adobe Stock] Marketplace; metadatafält |
| Distribuera och dela resurser efter organisationer | Interna avdelningar/lokala kontor och externa partners, distributörer och byråer använder det godkända material som delas av huvudorganisationen. Organisationen vill säkert och smidigt dela de skapade resurserna för vidare återanvändning. | Brand Portal, Resursdelningskommentarer |

## Adobe lösningar för samverkan {#adobe-offerings-to-support-the-collaboration-need}

| Värdeförslag för berörda personer | Adobe | Involverade ytor |
|---|---|---|
| Creative-användare upptäcker resurser från [!DNL Experience Manager], öppnar och använder dem, redigerar och överför ändringar till [!DNL Experience Manager] och överför nya filer till [!DNL Experience Manager], utan att lämna [!DNL Creative Cloud] program. | [Adobe Asset Link](https://helpx.adobe.com/se/enterprise/using/adobe-asset-link.html) | [!DNL Adobe Photoshop], [!DNL Adobe Illustrator] och [!DNL Adobe InDesign]. |
| Affärsanvändare förenklar öppning och användning av resurser, redigering och överföring av ändringar till [!DNL Experience Manager] samt överföring av nya filer till [!DNL Experience Manager] från skrivbordsmiljön. De använder en allmän integrering för att öppna alla resurstyper i det inbyggda datorprogrammet, inklusive sådana som inte är från Adobe. | [Experience Manager-datorprogram](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=sv-SE) | [!DNL Experience Manager] datorprogram på Win och Mac |
| Marknadsförare och affärsanvändare upptäcker, förhandsgranskar, licensierar och sparar samt hanterar [!DNL Adobe Stock]-resurser inifrån [!DNL Experience Manager]. Licensierade och sparade resurser tillhandahåller utvalda [!DNL Adobe Stock]-metadata för bättre styrning. | [Integrering med Experience Manager och Adobe Stock](aem-assets-adobe-stock.md) | Webbgränssnittet [!DNL Experience Manager] |

Den här artikeln fokuserar främst på de två första aspekterna av samarbetsbehovet. Distribution och anskaffning av resurser i stor skala omnämns kortfattat som ett användningsexempel. Överväg Adobes varumärkesportal eller Assets Share Commons för sådana behov. Alternativa lösningar som [Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/home.html?lang=sv-SE), lösningar som kan byggas baserat på [Resursdelningskomponenter](https://adobe-marketing-cloud.github.io/asset-share-commons/), [Länkdelning](/help/assets/link-sharing.md) och [Experience Manager Assets](/help/assets/manage-assets.md) bör granskas utifrån specifika krav.

![Creative Cloud-anslutningar för Experience Manager, avgör vilka funktioner som ska användas](assets/creative-connections-aem.png)

### Mappning av användningsfall och Adobe-lösningar {#mapping-of-use-cases-and-adobe-solutions}

<!-- TBD: Add some info about XD integration and possibly info about DA v2.0.
-->

| Användningsfall | [!DNL Adobe Asset Link] | Datorprogrammet [!DNL Experience Manager] | Anmärkningar/andra lösningar |
|---|---|---|---|
| Upptäck - bläddra bland DAM-mappar | Ja | [!DNL Experience Manager] Webbgränssnitt och skrivbordsåtgärder | |
| Upptäck - få åtkomst till DAM-samlingar | Ja | [!DNL Experience Manager] Webbgränssnitt och skrivbordsåtgärder | |
| Upptäck - sök efter resurser från DAM | Ja | [!DNL Experience Manager] Webbgränssnitt och skrivbordsåtgärder | |
| Använd - öppen resurs | Ja | Ja | [Öppna från webbgränssnittet](manage-assets.md#previewing-assets) eller från Finder |
| Använd - placera resurs från DAM i ett dokument | Ja - inbäddning | Ja - länkning eller inbäddning | Skrivbordsappen [!DNL Experience Manager] ger åtkomst till resurser som filer i det lokala filsystemet. De här länkarna i de ursprungliga programmen representeras av lokala sökvägar. |
| Redigera - öppna för redigering | Ja - utcheckningsåtgärd | Ja - Öppna åtgärd (i nätverksresursen) | [Utcheckning i AAL](https://helpx.adobe.com/se/enterprise/using/manage-assets-using-adobe-asset-link.html) sparar resursen i användarens Creative Cloud-lagringskonto (synkroniserat med Creative Cloud-appen) som standard. |
| Redigera - pågående arbete utanför DAM | Ja - Tillgångar som är tillgängliga i användarens Creative Cloud-lagringskonto synkroniserade med skrivbordet. | Ja | |
| Redigera - ladda upp ändringar | Ja - [Incheckningsåtgärd](https://helpx.adobe.com/se/enterprise/using/manage-assets-using-adobe-asset-link.html) med valfri kommentar | Ja | |
| Överför - en fil | Ja - överför aktuellt aktivt dokument | Ja | [Överför via webbgränssnitt](manage-assets.md#uploading-assets) |
| Överför - flera filer/hierarkiska mappstrukturer | Nej | Ja | [Överför via webbgränssnitt](manage-assets.md#uploading-assets) eller via anpassade skript eller verktyg. |
| Diverse - användare och inloggning | Creative Cloud-användare som är inloggad på Creative Cloud datorprogram blir igenkända (SSO) | [!DNL Experience Manager] användare och autentiseringsuppgifter | Användare av båda lösningarna räknas in i användarkvoten för [!DNL Experience Manager]. |
| Diverse - nätverk och åtkomst | Kräver åtkomst från användarens skrivbord till [!DNL Experience Manager]-distribution via nätverk | Kräver åtkomst från användarens skrivbord till [!DNL Experience Manager]-distribution via nätverk | [!DNL Adobe Asset Link] delar inte nätverksproxymiljö. |
| Diverse - Migrera ett stort antal resurser | Nej | Nej | [Migreringsguide för Assets](assets-migration-guide.md) |

För att stödja användningsexemplen på resursfördelning bör andra lösningar beaktas:

* [Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/home.html?lang=sv-SE) för ett konfigurerbart SaaS-tillägg i [!DNL Experience Manager Assets] om du vill publicera resurser.
* Anpassade lösningar skapas baserat på kodbasen [Resursdelningskommandon](https://adobe-marketing-cloud.github.io/asset-share-commons/).
* [!DNL Experience Manager] [länkresurs](/help/assets/link-sharing.md) om du vill dela resurser på begäran med hjälp av länkar.
* [Experience Manager Assets webbgränssnitt](/help/assets/manage-assets.md) med områden för externa parter som skyddas av [!DNL Experience Manager] åtkomstkontrollinställningar och nödvändiga IT-/nätverkskonfigurationsjusteringar, vilket ger dessa externa användare åtkomst till [!DNL Experience Manager].

## Viktiga begrepp och användningsområden {#key-concepts-and-use-cases}

### Ordlista med vanliga termer {#glossary-of-common-terms}

* **Pågående arbeten eller pågående designarbeten (WIP):** En fas i en resurs livscykel där den genomgår flera ändringar och oftast inte är redo att delas med större team.
* **Kreativa resurser:** [!DNL Assets] som är klara att delas med ett bredare team, eller som har valts ut eller godkänts av det kreativa teamet för delning med marknadsförings- eller LOB-team.
* **Godkännanden av resurser:** Godkännandeprocessen som körs för resurser som redan har överförts till DAM, som vanligtvis omfattar varumärkesgodkännanden, juridiska godkännanden och så vidare.
* **Slutlig resurs:** En resurs som har genomgått alla godkännanden/metadatataggar och är klar att användas av det bredare teamet. En sådan resurs lagras i DAM och är tillgänglig för alla (intresserade) användare. Den kan användas i marknadsföringskanaler eller av designteam för att skapa material.
* **Mindre uppdatering/ändring av resurser:** En snabb och liten ändring av en digital resurs. Ändringarna är ofta retuscheringar, smärre redigeringar, resursgranskningar eller godkännanden (t.ex. omplacering, ändring av textstorlek, justering av mättnad/intensitet eller färg).
* **Större uppdatering/ändring av resurser:** En ändring av en digital resurs som kräver mycket arbete och som ibland måste göras under en längre tidsperiod. Den omfattar vanligtvis flera ändringar. Resursen måste sparas flera gånger medan den uppdateras. Större resursuppdateringar medför oftast att resursen får statusen pågående arbete.
* **DAM:** Digitalt resurshanteringssystem. I det här dokumentet är det synonymt med [!DNL Experience Manager Assets], om inget annat anges.
* **Designanvändare:** En kreatör som skapar digitalt material med Creative Cloud-program och -tjänster. I vissa fall är designanvändaren medlem i ett designteam och kan använda Creative Cloud, men skapar inte digitala resurser (som en designchef eller designteamschef).
* **DAM-användare:** En typisk användare av ett DAM-system. Beroende på organisationen kan en DAM-användare vara en marknadsföringsanvändare eller en icke-marknadsföringsanvändare, till exempel en LOB-användare (Line-of-Business), bibliotekarie, säljare osv.

### Att tänka på när du använder [!DNL Experience Manager] och [!DNL Creative Cloud]-integrering {#considerations-when-using-aem-and-creative-cloud-integration}

* Se [bästa praxis för skrivbordsprogram](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/troubleshoot.html?lang=sv-SE#best-practices-to-prevent-troubles)
* Se [Integrering med Adobe Stock](aem-assets-adobe-stock.md)
* Se [Adobe Asset Link](https://helpx.adobe.com/se/enterprise/using/adobe-asset-link.html)

Detta är en kort sammanfattning av bästa praxis för [!DNL Experience Manager]- och [!DNL Creative Cloud]-integrering. Läs resten av det här dokumentet för att få en mer detaljerad förståelse för dessa.

* **För kreativa användare som arbetar i Photoshop, InDesign eller Illustrator:** Adobe Asset Link ger den bästa användarupplevelsen, inklusive ren hantering av pågående arbeten för resurser som checkats ut från [!DNL Experience Manager].
* **För att förenkla åtkomst till resurser från skrivbordet för alla generiska filformat eller program:** använd [!DNL Experience Manager] skrivbordsappen.
* **Förstå varför och när resurser ska lagras i DAM:** Uppdateringar som ska göras tillgängliga för hela teamet i organisationen.
* **Tänk på mängden resurser som delas:** Om ni använder mediedistribution kan styrning och säkerhet vara de viktigaste aspekterna. Överväg att använda verktyg som är byggda för att göra detta i stor skala, som varumärkesportalen.
* **Förstå resursers livscykel:** Ta reda på hur resurser hanteras i organisationen av olika team
* **Var försiktig med ofta sparade resurser:** Adobe Asset Link tar hand om det med PS, AI och ID. För andra program ska du inte utföra pågående uppgifter i mappad/delad mapp såvida du inte behöver alla ändringar i DAM

### Åtkomst till [!DNL Adobe Stock] resurser från [!DNL Assets] {#access-to-adobe-stock-assets-from-aem-assets}

Integreringen av [Experience Manager och Adobe Stock](/help/assets/aem-assets-adobe-stock.md) ger [!DNL Experience Manager] användare möjlighet att söka, förhandsgranska, licensiera och spara resurser från [!DNL Adobe Stock] till [!DNL Experience Manager]. Licensierade och sparade [!DNL Stock] resurser har valt [!DNL Stock]-metadata, som kan användas för att söka efter dem med extra filter.

Några viktiga punkter om den här integreringen:

* När resurser från Adobe Stock sparas till [!DNL Experience Manager] blir de ett vanligt [!DNL Assets] med binära data sparade i databasen [!DNL Experience Manager]. Vissa metadata som är relaterade till [!DNL Adobe Stock] sparas för resursen i [!DNL Experience Manager], annars ser inmatningsprocessen ut på samma sätt som för andra filer. Om till exempel smarta taggar är aktiva läggs taggarna till i de här resurserna när de sparas.
* Resursen som sparats till [!DNL Experience Manager] är en kopia, inte en länk tillbaka till [!DNL Adobe Stock].

**Arbeta med resurser som har sparats från [!DNL Adobe Stock] till [!DNL Experience Manager] i[!DNL Creative Cloud]**. Den här integreringen är oberoende av [!DNL Adobe Asset Link], men [!DNL Adobe Asset Link] känner igen dessa resurser som sparats från [!DNL Stock] på det sättet och visar ytterligare metadata och en [!DNL Adobe Stock] logotyp för dessa resurser i [!DNL Adobe Asset Link]-tilläggets användargränssnitt i [!DNL Photoshop], [!DNL Illustrator] eller [!DNL InDesign]. Filerna är tillgängliga för att bläddra, öppna och så vidare, eftersom de är vanliga resurser när de sparas i [!DNL Experience Manager].
Creative-användare som arbetar i [!DNL Creative Cloud]-program med tillägget [!DNL Adobe Asset Link] kan, förutom att ha tillgång till redan licensierade resurser från [!DNL Adobe Stock] till [!DNL Experience Manager], även använda panelen [!DNL Creative Cloud] Libraries för att söka efter, förhandsgranska och licensiera [!DNL Adobe Stock]-resurser.
[!DNL Assets] från [!DNL Adobe Stock] licensierade och sparade i [!DNL Experience Manager] blir tillgängliga för de större team som har åtkomst till [!DNL Experience Manager Assets]-distributionen, medan kreativa användare som licensierar resurser från [!DNL Adobe Stock] via [!DNL Creative Cloud]-bibliotekspanelen endast gör dem tillgängliga för sig själva som standard i sitt [!DNL Creative Cloud]-konto.

<!-- 
TBD: A condensed version of the below content is better placed in the Adobe DAM introduction article.
-->

## Lagra resurser i ett resurshanteringssystem {#about-storing-assets-in-a-dam}

För att skapa ett effektivt arbetsflöde mellan kreatörer och marknadsförare/branschspecifika team (LOB) och välja de bästa supportfunktionerna är det viktigt att förstå när och varför resurser lagras i DAM.

### Varför resurser lagras i DAM {#why-assets-are-stored-in-dam}

Genom att lagra resurser i DAM blir de enkelt tillgängliga och sökbara. Det ser till att resurserna kan användas av många användare i organisationen eller ekosystemet, bland annat partners, kunder och så vidare.

De flesta organisationer väljer att endast lagra resurser som är relevanta för marknadsförings-/LOB-processerna längre fram i kedjan (publicera till kanaler som webbkanal via [!DNL Experience Manager Sites] eller andra kanaler som tillhandahålls av Adobe Experience Cloud - Marketing Cloud, Advertising Cloud och mäts av Analytics Cloud, som tillhandahåller till användare/partner och så vidare). Dessutom lagrar organisationer resurser som kan bli föremål för en gransknings-/godkännandeprocess i DAM. På så sätt lagrar DAM mest resurser som har stora chanser att användas och undviker att lagra inaktiva resurser.

Lagring av resurser är också beroende av tekniska aspekter och resursanvändning. DAM tillhandahåller ytterligare tjänster runt lagrade resurser, inklusive extrahering av metadata, versionshantering, generering av förhandsgranskning/omkodning, hantering av referenser och tillägg av åtkomstkontrollsinformation. Dessa tjänster kräver extra tid och infrastrukturresurser.

Det är ofta inte önskvärt att lagra alla resurser och uppdateringar. Om till exempel uppdateringar av specifika resurser har dålig kvalitet och förbrukar för mycket resurser, kanske resurserna inte lagras i DAM.

#### När resurser lagras i DAM {#when-assets-are-stored-in-dam}

Kreativa team (och organisationer) är vanligtvis inte intresserade av att lagra resurser i varje skede av resursens livscykel. De undviker till exempel att lagra resurser i följande fall:

* Assets som ännu inte är klara eller som är föremål för experimenterande.
* Assets som inte klarar granskningsprocessen.
* Jämfört med resursen i fråga har teamet bättre kandidater att representera sitt arbete för externa team.

Vanligtvis lagras följande klassresurser i DAM:

* Assets som nått en viss mognad och anses vara redo att delas.
* Assets som har valts ut i förväg av det kreativa teamet.
* Specifika filformat som kan användas eller begäras av marknadsföring, beroende på ett specifikt kontrakt eller avtal (t.ex. JPG-filer som konverterats från RAW-filer, TIFF-filer/bilder från PSD-original).

#### När resursuppdateringar lagras i DAM {#when-updates-to-assets-are-stored-in-dam}

I regel ska endast uppdateringar av resurser som är relevanta för den bredare uppsättningen DAM-användare lagras i DAM. Det ser till att användare (marknadsföringsfunktioner och liknande funktioner) bara kan se relevanta versioner på tidslinjen för DAM-resurser.

Vanligtvis ändras relaterade till viktiga milstolpar i resursens livscykel. Den initiala marknadsföringsfärdiga resursen eller en officiell uppdatering som baseras på begäran/granskning som tillhandahålls av det kreativa teamet bör till exempel lagras och versionshanteras i DAM.

Det kreativa teamets uppdatering för granskning av marknadsföringsteamet efter en begäran om ändring av befintligt material i DAM är ett exempel på en relevant uppdatering. Den ska lagras och versionshanteras i DAM för vidare referens eller för att återgå till den tidigare versionen.

Nedan följer exempel på uppdateringar som vanligtvis inte är relevanta:

* Tidiga versioner av mediefiler som överförts innan de är klara för marknadsföringsgranskning
* Vanliga kreativa förändringar av resursen i den pågående arbetsfasen innan kreatörer och marknadsförare bestämmer sig för att resursen är klar

### Användaråtkomst till DAM {#user-access-to-dam}

[!DNL Assets] stöder två typer av användare baserat på deras åtkomst till distributionen [!DNL Assets]. Vanligtvis har användare i företagsnätverket (brandväggen) direktåtkomst till DAM. Andra användare utanför företagsnätverket skulle inte ha direkt åtkomst. Användartypen avgör vilka integreringar som kan användas ur teknisk synpunkt.

#### Kreativa användare med direkt åtkomst till DAM {#creative-users-with-direct-access-to-dam}

Vanligtvis har interna kreativa team, byråer/kreativa proffs som är anställda på det interna nätverket tillgång till DAM-distributionen, inklusive [!DNL Experience Manager]-inloggning. [!DNL Experience Manager] och nätverksinfrastruktur kan konfigureras för att ge direktåtkomst till externa parter - vanligen betrodda organisationer som byråer som arbetar för en klient - för att få åtkomst till [!DNL Experience Manager] via nätverket, till exempel via VPN eller IP tillåtelselista.

I sådana fall kan Adobe Asset Link eller datorprogrammet [!DNL Experience Manager] ge enkel åtkomst till slutliga/godkända resurser och göra det möjligt att spara kreativa resurser på DAM.

#### Kreativa användare utan åtkomst till DAM {#creative-users-without-access-to-dam}

Externa byråer och frilansare som inte har direkt åtkomst till DAM-distributionen kan behöva åtkomst till godkända resurser eller lägga till sina nya designer i DAM.

Använd följande strategier för att ge tillgång till slutliga/godkända mediefiler:

* Använd skrivbordsappen om Asset Link inte fungerar.
* Använd [Experience Manager Assets Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/home.html?lang=sv-SE) för säker distribution av resurser till externa partner
* Använd en anpassad implementering av en distributions- och källportal baserad på [Resursdelningskommentarer](https://adobe-marketing-cloud.github.io/asset-share-commons/)
* Använd åtkomstkontrollen som har konfigurerats i [!DNL Experience Manager] och nödvändig nätverksinfrastruktur (till exempel VPN och IP tillåtelselista) för att ge externa parter åtkomst till ett dedikerat innehållsområde i din DAM. De kan använda webbgränssnittet [!DNL Experience Manager] för att hämta resurser och överföra nytt innehåll till din DAM.

#### Arbete pågår för resurser från [!DNL Experience Manager] {#work-in-progress-on-assets-from-aem}

Som beskrivs i det här dokumentet rekommenderas du att utföra större uppdateringar av resurser, ibland kallat pågående arbete, utan att ha alla redigeringar sparade i den lokala filen överförda till [!DNL Experience Manager] som ändringar. Detta snabbar upp arbetet för en stationär användare, begränsar den nätverksbandbredd som används och håller resursernas tidslinje ren och fokuserad på kontrollerade, viktiga uppdateringar.

Adobe Asset Link har ett bra stöd för detta:

* När användare i [!DNL Photoshop], [!DNL InDesign] eller [!DNL Illustrator] har för avsikt att redigera en fil, utför de en utcheckningsåtgärd för den angivna resursen
* Resursen hämtas i bakgrunden, läggs till användare Creative Cloud-kontot synkroniserat med disken av Creative Cloud skrivbordsapp och utcheckningsflaggan aktiveras i [!DNL Experience Manager] på resursen för att minimera redigeringskonflikter
* Därifrån arbetar användaren i en fil som lagras lokalt på den synkroniserade platsen och kan fortsätta att arbeta och spara nödvändiga ändringar med den frekvens som behövs
* Eftersom resursen finns på Creative Cloud-kontot är den även tillgänglig på andra enheter som användaren kan ha (till exempel kan öppnas eller redigeras i en dedikerad Creative Cloud-mobilapp) och kan delas med andra Creative Cloud-användare i samarbetssyfte.
* När den kreativa användaren är klar med ändringarna kan han/hon utföra en incheckningsåtgärd för filen i sitt Creative Cloud-program, med en valfri kommentar. Motsvarande resurs i [!DNL Experience Manager] versionshanteras och uppdateras till med den nya binärfilen. [!DNL Experience Manager]-användare som marknadsförare eller LOB-användare har tillgång till större ändringar av resurser, eller milstolpar, via [!DNL Experience Manager]-resursens tidslinjegränssnitt.

Skrivbordsappen [!DNL Experience Manager] tillhandahåller en nätverksresurs för resurser som har öppnats i det ursprungliga programmet. Som standard överförs alla ändringar som görs lokalt till [!DNL Experience Manager] automatiskt efter en kort stund. Med en sådan konfiguration överförs ofta sparade filer under den pågående arbetsfasen till [!DNL Experience Manager] och versionsindelas, vilket skapar en mängd nätverkstrafik och potentiella skalbarhetsproblem - för att inte tala om onödiga versioner i [!DNL Experience Manager].

Det rekommenderade sättet här är att använda ett alternativ i [!DNL Experience Manager]-skrivbordsappen för att inaktivera automatiska uppdateringar och överföra ändringar av resurser till [!DNL Experience Manager] manuellt med hjälp av åtgärden för överföring av ändringar i appens resursstatusgränssnitt.

#### Massöverföring till DAM {#bulk-upload-to-dam}

Du kan behöva överföra ett större antal filer till DAM samtidigt i vissa scenarier, till exempel:

* Överföra resultat från foton eller större projekt
* Överföra material från reklambyråer
* Överför markerade resurser från en större uppsättning om markeringen görs utanför DAM

Beskrivningen avser att överföra filer operativt (till exempel varje vecka eller med varje fotografering), som en normal del av datoranvändarens arbetsflöde. Stora resursmigreringar beskrivs inte här.

Du kan använda följande överföringsfunktioner:

* Om du vill överföra stora/hierarkiska mappar samtidigt använder du [!DNL Experience Manager]-skrivbordsappen som har [mappöverföringsfunktioner](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=sv-SE#upload-and-add-new-assets-to-aem). Du kan också överföra hierarkiska mappstrukturer. [!DNL Assets] överförs i bakgrunden och är därför inte knuten till en webbläsarsession
* Om du vill överföra några filer från en enda mapp drar du filerna direkt till webbgränssnittet eller använder alternativet Skapa i webbgränssnittet för [!DNL Assets].
* Beroende på vilka affärskrav du har kan du även använda en anpassad överförare.

#### Hantera digitala resurser direkt från skrivbordet {#managing-digital-assets-directly-from-desktop}

Om du använder Nätverksfilresurser för att hantera digitala resurser kan du se att bara den nätverksresurs som är mappad av skrivbordsappen [!DNL Experience Manager] används som ett praktiskt alternativ. När du övergår från nätverksfilresurser innehåller webbgränssnittet [!DNL Experience Manager] en mängd funktioner för hantering av digitala resurser som går mycket längre än vad som är möjligt på en nätverksresurs (sökning, samlingar, metadata, samarbete, förhandsvisningar och så vidare) och skrivbordsappen [!DNL Experience Manager] erbjuder en praktisk länk för att ansluta DAM-databasen på serversidan med skrivbordsarbetet.

Undvik att använda datorprogrammet [!DNL Experience Manager] för att hantera resurser direkt i nätverksresursen för [!DNL Assets]. Undvik till exempel att använda datorprogrammet [!DNL Experience Manager] för att flytta/kopiera flera filer. Använd i stället gränssnittet [!DNL Assets] för att dra mappar från Finder/Explorer till nätverksresursen eller använd funktionen [!DNL Assets] Mappöverföring.

#### Resursmigrering {#asset-migration}

Information om hur du planerar och kör resursmigreringar från ett befintligt system till ett nytt system eller migrering av stora volymer resurser som lagras på servrar finns i [migreringshandboken](/help/assets/assets-migration-guide.md). [!DNL Experience Manager]-datorprogrammet och [!DNL Experience Manager] till [!DNL Creative Cloud]-integreringar stöder inte sådana migreringar. På grund av de stora volymer resurser som ska importeras och ytterligare krav på metadatamappning, omvandling och förtäring bör migreringar hanteras med olika verktyg och metoder.

>[!MORELIKETHIS]
>
>* [Adobe Asset Link](https://helpx.adobe.com/se/enterprise/using/adobe-asset-link.html)
>* [Experience Manager bästa praxis för skrivbordsprogram](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/archive/best-practices-for-v1.html?lang=sv-SE)
>* [Experience Manager Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/introduction/brand-portal.html?lang=sv-SE)
>* [Integrering med Experience Manager och Adobe Stock](aem-assets-adobe-stock.md)
