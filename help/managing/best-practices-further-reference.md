---
title: Checklista - ytterligare referens
description: Läs mer om detaljerad information som går igenom och/eller förstärker de dokument och principer som omfattas av checklistan Hantera projekt - Bästa metoder.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/MANAGING
topic-tags: managing-checklist
content-type: reference
solution: Experience Manager, Experience Manager 6.5
feature: Compliance
role: Admin,Architect,Data Architect,Developer,Leader
exl-id: 15d9c2ac-0b13-4839-a873-bc884ee69132
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3695'
ht-degree: 0%

---

# Checklista - ytterligare referens{#the-checklist-further-reference}

Den här sidan innehåller mer information om hur du kan arbeta vidare med och/eller förstärka de dokument och principer som omfattas av checklistan [Hantera projekt - Bästa metoder](/help/managing/best-practices.md).

## AEM - Vad tänker du använda? {#aem-what-will-you-be-using}

>[!CAUTION]
>
>Förteckningen i detta underavsnitt är inte uttömmande, utan avsedd som en introduktion.

### Funktioner i AEM {#features-within-aem}

När du implementerar AEM (särskilt för första gången) ska du kontrollera vilka områden du vill ha eller behöver i [funktionerna och arbetsflödena i AEM](https://business.adobe.com/products/experience-manager/adobe-experience-manager.html).

Ta en titt på de funktioner i AEM som du använder och hur designen påverkas, till exempel:

* [Assets](/help/assets/assets.md)
* [Taggar](/help/sites-administering/tags.md)
* [Hantering och översättning av flera webbplatser](/help/sites-administering/msm-and-translation.md)
* [Forms](/help/forms/using/introduction-aem-forms.md)

Kontrollera dessutom [Versionsinformation](/help/release-notes/release-notes.md) för de olika versionerna av AEM för att se när nya funktioner lades till.

### Integreringar {#integrations}

AEM kan integreras med andra Adobe-produkter, med tredjepartstjänster eller med båda. Dessa arbetsflöden kan öka kraften och funktionaliteten du har till hands.

Mer information finns i [Lösningsintegrering](/help/sites-administering/integration.md).

## Migrera eller uppgradera? {#migrate-or-upgrade}

Det är viktigt att tänka på om du vill:

* Uppgradera den befintliga installationen.
* Migrera innehållet från det aktuella systemet till en ny installation.

När du går från en tidigare version till den aktuella versionen finns det två alternativ:

* Använd [pakethanteraren](/help/sites-administering/package-manager.md) för att exportera allt innehåll och all programkod från det gamla systemet till det nya.
* [Uppgradera](/help/sites-deploying/upgrade.md) den gamla datorn på plats. Den här metoden rekommenderas vanligtvis.

## Grundregler {#basic-ground-rules}

Precis som med andra projekt är det viktigt att fastställa grundregler så snart som möjligt. Dessa regler omfattar:

>[!NOTE]
>
>Dessa punkter är generiska och checklistan [Best Practices](/help/managing/best-practices.md) behandlar information om AEM.

* **Roller**

  Roller bör vara tydligt definierade och göras kända för alla som är inblandade i projektet. Det är dessutom tillrådligt att understryka:

   * Beslutsfattare
   * Kontaktpunkter

* **Ansvarsområden**

   * För varje roll är det lättare att undvika förvirring om du tydligt definierar ansvarsområdena för ditt projekt.

* **Deltagande**

  Genom att involvera berörda parter så snart som möjligt kan du uppmuntra dem att bli *intressenter* i projektet. Om de gör det ökar de sin vilja till framgång.

   * På kundsidan omfattar den här rollen författare som arbetar med systemet dagligen
   * Inom ditt eget projektteam omfattar detta även de personer som ansvarar för kvalitetssäkring. Ju mer de förstår kundens krav, desto bättre kan de planera testerna.

* **Kommunikationsvägar**

   * Även om kommunikationsvägar inte bör formaliseras alltför mycket bör särskilda definitioner säkerställa att de viktigaste personerna alltid är informerade och därför hålls uppdaterade. Särskild uppmärksamhet bör ägnas kommunikationen med externa parter.

* **Processer**

  De definierade processerna är beroende av ditt enskilda projekt. Försök att göra dessa processer enkla, med tanke på:

   * Definiera processer (och kommunikationsvägar) för interaktion med tredje part, t.ex. designbyråer och tredjepartsleverantörer av programvara.
   * Kunden har ofta egna rutiner och verktyg för projekthantering och rapportering.

* **Spårningsverktyg**

  Det finns många verktyg för att spåra information om buggar, uppgifter och andra aspekter av ditt projekt - se [Översikt över potentiella verktyg](#overview-of-potential-tools) för mer information.

   * Det viktigaste att notera här är att bara behålla en kopia av informationen och dela informationen (och därmed tillgång till det verktyg som används). Det här arbetsflödet underlättar underhållet och hjälper till att förebygga avvikelser.

* **Omfång**

  Definiera tydligt vad som ska täckas av projektet på olika nivåer:

   * de enskilda releaserna (om en iterativ versionsprocess används och oavsett om de levereras till kunder eller till ditt interna testteam).
   * AEM-projektet.
   * hela projektet, inklusive eventuella tredjepartsprogram, deras inverkan på testning, organisatoriska frågor och många andra.
   * För vissa aspekter kan det också vara användbart att ange vad som är *inte* inom projektets omfång. Denna idé kan bidra till att förhindra förvirring och felaktiga antaganden, även om den bör begränsas till väsentliga frågor.

* **Rapportering**

  Definiera tydligt vilken information du vill rapportera, i vilken form, hur ofta och till vem.

* **Terminologi**

   * Definiera de förkortningar och/eller kundspecifika termer som ska användas.

* **Antaganden**

   * Definiera eventuella antaganden.

Den här informationen kan definieras i en projekthandbok. Om du använder en Wiki kan du även se till att pågående ändringar hanteras på ett effektivt sätt. De viktigaste faktorerna är att

* Informationen definieras och underhålls
* All information förmedlas tydligt till alla berörda. Även om det är en vanlig projekthanteringspraxis kan den inte upprepas tillräckligt ofta, så att en tydlig rolldefinition och bra kommunikation kan skapa, eller bryta, ett projekt.
* Det finns bara en version som innehåller all information som spåras, till exempel felspårning och problemspårning.

## Viktiga resultatindikatorer och målvärden {#key-performance-indicators-and-target-metrics}

Organisationer använder nyckeltal (KPI:er) för att utvärdera hur de lyckades uppnå sina mål. Dessa indikatorer är mätbara värden som kan användas för att visa hur effektivt specifika mål uppfylls.

Dessa indikatorer kan vara

* Företag:

   * Används för att mäta viktiga affärsmål.
   * Det är viktigt att välja nyckeltal som är lämpliga för ert företag/scenario med tydliga definitioner av vad de är, hur de mäts, hur de används och av vem.

* Prestanda:

   * Definiera hur systemets prestanda ska mätas.
   * Några exempel är sidinläsningstid, svarstid för servern och prestanda för databasfrågor.

Vissa indikatorer, men inte alla, kan baseras på målmåtten som du identifierar och definierar.

### Måttmål {#target-metrics}

Mätvärden används för att definiera kvantitativa mått för kvaliteten på din webbplats. De är i princip en definition av de prestandamål som du vill uppnå och kan användas för att definiera dina [KPI:er (Key Performance Indicators)](#key-performance-indicators-and-target-metrics).

Många mätvärden kan definieras, men ofta täcker de mål som du har satt upp för prestanda och samtidighet. Detta gäller särskilt faktorer som kan vara svåra att kvantifiera och som ofta är benägna att *göra en känslomässig* bedömning:

* &quot;webbplatsen är *mycket för långsam* idag&quot; - när kvalificerar sig *långsam*?

* &quot;allt *som stannar* när min kollega loggar in&quot; - hur många samtidiga användare kan systemsupporten?
* &quot;när jag söker efter systemet *som stannar* &quot; - vilka sökförfrågningar påverkar systemet?
* &quot;det tar *sidor* att hämta filen&quot; - vilka hämtningstider är tillåtna (under normala nätverksförhållanden)?

Målmått definieras i början av ett projekt till:

* ange de förväntade dimensionerna för webbplatsen som du kan erbjuda
* ange den minimikvalitet som du vill uppnå
* definiera hur dessa faktorer ska mätas
* användas som grund för [nyckelprestandaindikatorerna](#key-performance-indicators-and-target-metrics)

Som alltid måste man vara försiktig när man definierar målmåtten:

* om de är för höga kan de vara omöjliga att nå
* om inställt för låga fluktuationer inte kan markeras
* för att säkerställa att de kan mätas upprepade gånger och på ett konsekvent sätt
* för att skapa en balans mellan de olika faktorer som mäts
* vissa mätvärden relaterar till en testmiljö, men vissa bör återspegla verkliga scenarier eftersom de måste vara mätbara och reproducerbara på produktionens webbplats
* prioritera mätvärdena utifrån deras betydelse för webbplatsen
* begränsa mätvärdena till en uppsättning som kan övervakas

Under projektets utveckling kan de uppdateras och justeras på lämpligt sätt. När projektet har implementerats kan de användas för att hjälpa dig att styra installationen och övervaka/underhålla de servicenivåer som krävs för den pågående åtgärden.

När de används på rätt sätt kan dessa mätvärden vara ett användbart verktyg. När de används på ett oansvarigt sätt kan de vara en tidsödande störning. Som alltid, förstå vad du mäter, hur du mäter det och varför.

>[!NOTE]
>
>I det här avsnittet diskuteras grundläggande principer och frågor som ska övervägas. Alla installationer är olika, så de faktiska värdena som ska mätas tenderar att vara olika.

### Allt hänger på din projektdesign {#everything-rests-on-your-project-design}

Alla mätvärden påverkas av projektets utformning. Omvänt löses många problem bäst genom designändringar.

Definiera därför dina målvärden *innan* bestämmer dig för din design. På så sätt kan du optimera din design baserat på dessa faktorer. När projektet har utvecklats är det en utmaning att följa de grundläggande designprinciperna.

När du skapar webbplatsens struktur följer du den rekommenderade strukturen för AEM webbplatser. Se till att du förstår följande:

* Strukturera webbplatsinnehåll.
* Hur mallar och komponenter fungerar.
* Hur fungerar cachelagring?
* Effekterna av personaliserat innehåll.
* Hur sökfunktionen fungerar.
* Hur du kan använda CSS och relaterade tekniker för att skapa kompakt, icke-redundant HTML-kod.

Om du känner att din design inte följer riktlinjerna, eller om du är osäker på några av konsekvenserna, kan du klargöra dessa problem. Gör det innan du startar antingen programmeringsfasen eller fyller i innehållet.

### Infrastruktur {#infrastructure}

För att definiera eller bedöma infrastrukturen kan det hjälpa till att definiera målvärden som:

* besökare/dag; både medelvärde och toppvärde
* träffar/dag; både medelvärde och toppvärde
* antal webbsidor som görs tillgängliga
* mängd webbinnehåll

Beroende på din situation och webbplatsens strategiska betydelse kan en definition av infrastruktur hjälpa dig att bedöma och välja din infrastruktur:

* antal servrar
* antal AEM-instanser (författare och publicera)

### Prestanda {#performance}

Det finns flera faktorer som kan utvärderas:

* svarstider för enskilda sidor, som representerar:

   * svarstider i en författarmiljö
   * svarstider i publiceringsmiljön

* svarstider för sökbegäranden

Det här avsnittet kan läsas med [Prestandaoptimering](/help/sites-deploying/configuring-performance.md) som utökar den tekniska informationen för att mäta prestanda.

#### Svarstider för enskilda sidor {#response-times-for-individual-pages}

Ett viktigt problem är den tid det tar för er webbplats att svara på besökarnas förfrågningar.

Även om det här värdet varierar för varje begäran kan ett genomsnittligt målvärde definieras. När det här värdet har visat sig vara både genomförbart och underhållbart kan det användas för att övervaka webbplatsens prestanda och ange utvecklingen av potentiella problem

Olika mål för skribent- och publiceringsmiljöer

De svarstider du vill ha skiljer sig åt mellan olika utvecklings- och publiceringsmiljöer, vilket återspeglar målgruppen:

* **Författarmiljö**

  Den här miljön används av författare som anger och uppdaterar innehåll, så den måste:

   * tillgodose ett fåtal användare som skapar ett stort antal förfrågningar när de uppdaterar innehållssidor och de enskilda elementen på dessa sidor
   * vara så snabb som möjligt för att maximera produktiviteten och få ut materialet på webbplatsen

* **Publiceringsmiljö**

  Den här miljön innehåller innehåll som du gör tillgängligt för användarna:

   * hastigheten är fortfarande viktig, men den är ofta långsammare än en författarmiljö
   * fler mekanismer för prestandaförbättring ofta tillämpas:

      * innehållet cachelagras
      * belastningsutjämning används

#### Ange målsvarstider {#setting-target-response-times}

Hur kan du bestämma dig för uppnåbara (genomsnittliga) svarstider? Frågan och svaret är ofta en fråga om erfarenhet:

* upplevelse på er webbplats
* AEM
* känna igen komplexa sidor som har svarstider över genomsnittet (dessa sidor bör optimeras individuellt om det är möjligt)

Under kontrollerade förhållanden kan dock följande riktlinjer tillämpas:

* 70 % av förfrågningarna om sidor ska svara på mindre än 100 ms.
* 25 % av förfrågningarna om sidor bör svara på mindre än 100 ms-300 ms.
* 4 % av förfrågningarna om sidor ska svara på mindre än 300 ms-500 ms.
* 1 % av förfrågningarna om sidor ska svara på mindre än 500 ms-1 000 ms.
* Inga sidor ska svara långsammare än 1 sekund.

Numren ovan förutsätter följande villkor:

* mätt vid publicering (ingen redigeringsmiljö och/eller CFC-overhead)
* mätt på servern (ingen nätverksbelastning)
* inte cachelagrad (ingen AEM-utdatacache, ingen Dispatcher-cache)
* endast för komplexa objekt med många beroenden (HTML, JS, PDF, ...)
* ingen annan belastning på systemet

Det finns flera mekanismer som du kan använda för att övervaka svarstiderna:

* **Övervaka svarstider med AEM request.log**

  En bra utgångspunkt för prestandaanalys är begärandeloggen. Du kan bland annat se svarstiderna för enskilda begäranden. Mer information finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).

* **Övervaka svarstider med HTML-kommentarer**

  HTML-kommentarer kan användas för att inkludera information om svarstid i källan för varje sida:

  `</body> </html>v <-- Page took 58 milliseconds to be rendered by the server --> Response times for search requests`

#### Sökbegäranden {#search-requests}

Sökförfrågningar kan ha stor effekt på din webbplats, både när det gäller:

* Svarstid för den faktiska sökningen

   * En snabb sökfunktion är ett kvalitetsmål för din webbplats

* Effekter på allmänna prestanda

   * Eftersom en sökfunktion måste skanna (potentiellt stora) avsnitt av innehållet, eller ett särskilt extraherat index, kan den här funktionen påverka hela systemets prestanda, om den inte är optimerad

Att ange mål för sökbegäranden är återigen en fråga om erfarenhet beroende på:

* upplevelse av AEM
* en bedömning av hur ofta sökning används i jämförelse med andra mål
* din persistence Manager
* ditt sökindex
* komplexiteten i sökfunktionen. En grundläggande sökfunktion som gör att ett sökord kan anges, är snabbare än en avancerad sökning som gör att användaren kan skapa komplexa söksatser med AND/OR/NOT.

Dessa sökförfrågningar bör planeras och integreras redan från början av projektet. De övervakningsmekanismer som finns är bland annat:

* **Övervaka söksvarstider med AEM request.log**

  Återigen kan request.log användas för att övervaka svarstiderna för sökbegäranden. Mer information finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).

* **Programmerade mekanismer för att mäta svarstider för sökningar**

  Om du vill anpassa den information som du samlar in om sökbegäranden och deras prestanda rekommenderar vi att du inkluderar informationssamling i projektets källkod. Mer information finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).

### Samtidighet {#concurrency}

Gör webbplatsen tillgänglig för vissa användare och besökare, både i författarmiljön och i publiceringsmiljön. Siffrorna är ofta mer än du använde när du testade, men de varierar också och är svåra att förutse. Utforma webbplatsen för ett genomsnittligt antal samtidiga användare och besökare utan att märka någon negativ inverkan på prestandan. Använd `request.log` igen för att göra samtidighetstester. Mer information finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).

Målen för antalet samtidiga användare, beroende på miljötypen:

* **Författarmiljö**

   * Vanligtvis kan antalet samtidiga användare uppskattas korrekt. Du kan veta hur många författare du har totalt, men (troligen) alla är inte aktiva samtidigt.

* **Publiceringsmiljö**

   * Publiceringsmiljön är en större utmaning att förutse, så du måste välja ett målvärde. Återigen bör den baseras på erfarenheterna av den aktuella webbplatsen och realistiska förväntningar på den nya webbplatsen.
   * Specialhändelser (t.ex. när du publicerar nytt populärt innehåll) kan överstiga förväntningarna - eller till och med möjligheterna (som ibland rapporteras i pressen när biljetter till vissa evenemang görs tillgängliga för försäljning).

### Kapacitet och volym {#capacity-and-volume}

Innan du diskuterar relaterade mätvärden ska du snabbt definiera termerna:

* **Volym**

   * Den mängd utdata som bearbetas och levereras av systemet.

* **Kapacitet**

   * Systemets förmåga att leverera volymen.
   * I varje steg mäts kapacitet och volym på olika sätt, vilket framgår av tabellen nedan. För bästa prestanda bör du se till att kapaciteten matchar volymen i varje steg och att både kapacitet och volym delas i alla steg. Du kan till exempel beräkna navigeringen på klientdatorn eller placera den i cachen, i stället för att beräkna den på servern för varje begäran.

* **Kapacitet och volym**

  | Vad / Var | Kapacitet | Volym |
  |---|---|---|
  | Klient | Datorkraft i användarens dator. | Sidlayoutens komplexitet. |
  | Nätverk | Nätverksbandbredd. | Sidans storlek (kod, bilder o.s.v.). |
  | Dispatcher cache | Webbserverminne (huvudminne och hårddisk). | Webbserver (huvudminne och hårddisk). Antal och storlek för cachelagrade sidor. |
  | Utdatacache | Serverminne för AEM-servern (huvudminne och hårddisk). | Antal och storlek för sidorna i utdatacachen, antalet beroenden per sida. Dispatcher-cachen sänker volymen. |
  | Webbserver | Webbserverns datorkraft. | Antal begäranden. Cachelagring sänker volymen. |
  | Mall | Webbserverns datorkraft. | Mallarnas komplexitet. |
  | Databas | Databasens prestanda. | Antal sidor som lästs in från databasen. |

### Andra mått {#other-metrics}

I de föregående avsnitten beskrivs huvudmåtten som ska definieras.

Beroende på dina specifika krav kan det vara användbart att definiera ytterligare mått, antingen separat eller genom att redovisa klassificeringarna ovan.

Men det är bättre att ha en liten uppsättning exakta, viktiga mätvärden som fungerar enkelt och tillförlitligt, i stället för att försöka mäta och definiera alla delar av webbplatsen. Webbplatsen förändras och utvecklas när den skickas vidare till användarna.

## Dokumentskydd {#security}

Säkerhet är avgörande och en ständigt ökande utmaning. Det ***måste*** övervägas och planeras från de tidigaste stadierna i ditt projekt.

[Säkerhetschecklistan](/help/sites-administering/security-checklist.md) innehåller information om de åtgärder du bör vidta för att se till att din AEM-installation är säker när den distribueras. Andra säkerhetsaspekter beskrivs under [Säkerhet (vid utveckling)](/help/sites-developing/security.md) och [Användaradministration och säkerhet](/help/sites-administering/security.md).

## Parallella och interaktiva uppgifter {#parallel-and-iterative-tasks}

>[!NOTE]
>
>Följande:
>
>* Erbjuder en översikt relaterad till den *första*-implementeringen av ett AEM-projekt.
>* Är avsedd som en abstrakt översikt. Se [Projektchecklista](/help/managing/best-practices.md) för specifika faser/milstolpar/uppgifter.
>* Alla tidsskalor är teoretiska.
>

För en ny implementering av ett AEM-standardprojekt bör du överväga följande:

* Överleverans från försäljningsprocessen.
* Implementering av kundprogrammet (**Utveckling**).
* Installation och konfigurering av infrastrukturen (och relaterade processer) på kundplatsen (**Infrastruktur**).
* Skapande (eller migrering) av innehållet (**Innehåll**).
* Lämna över till åtgärder (**Underhåll/support**).
* Följ upp releaser.

![chlimage_1-2](assets/chlimage_1-2.png)

För alla aspekter rekommenderas en iterativ metod:

![chlimage_1-3](assets/chlimage_1-3.png)

>[!NOTE]
>
>Om du vill tillåta justering, optimering och användarutbildning under realistiska förhållanden i produktionsmiljön delar du upp projektstarten i **Soft Launch** (reducerad tillgänglighet, flera iterationer) och **Hard Launch** (full tillgänglighet - Live).

>[!NOTE]
>
>Se [Projektchecklista](/help/managing/best-practices.md) för exempel på uppgifter som du bör utföra (eller utvärdera) under projektets livscykel.

Några poäng för varje kategori är:

* **Utveckling**

   * Definiera basarkitekturen först.
   * Använd flera iterationer (stänk) för utveckling:

      * Den första fjädringen motsvarar den första fullständiga utvecklingscykeln.
      * Första Sprint-resultatet i den första distributionen till testmiljön.
      * Varje fjäder ger ett körbart resultat.
      * Varje Sprint får en kundsignatur (minimum av strukturerat test med feedback).

   * Planera för en eventuell uppdatering av den tillgängliga AEM-versionen under projektet.
   * Planera för tester och optimering under skarvar.
   * Planera för stabiliserings- och optimeringsfaser.
   * Skapa en logg med artiklar som ska planeras för ytterligare versioner.
   * Planera för partnersamarbete och överlämnande.

* **Infrastruktur**

   * Definiera basarkitekturen först:

      * Definiera prestandakrav.
      * Definiera prestationsmål (dvs. tydligt definiera förväntningar).
      * Definiera maskinvaru- och infrastrukturarkitektur, inklusive storlek.
      * Definiera distribution.

   * Använd flera iterationer; för den första sprint-konfigurationen och den inledande konfigurationen förbereds:

      * Utvecklingsmiljö.
      * Utvecklingsprocess.
      * Testmiljö.
      * Distributionsprocess (inklusive konfigurationshantering).

   * Planera för flera lastprovningar.
   * Planera för tester och optimering under skarvar.
   * Planera för en stabiliserings- och optimeringsfas.
   * Driftsätt i produktionsmiljön så tidigt som möjligt (låt driftsteamet installera systemet för att få en upplevelse).
   * Använd namngivna användare och definierade roller så tidigt som möjligt.
   * Planera för utbildning (till exempel administratörsutbildning).
   * Planera för överlämning till verksamheten.

* **Innehåll**

   * Basarkitekturen:
      * Driver innehållshierarkin.
      * Hjälper till att definiera innehållskonceptet.
      * Definierar MSM-användning och -layout.
      * Definierar roller, grupper, arbetsflöden och behörigheter.
   * Ta reda på om det är praktiskt att skapa offlinesidor.
   * Planera för att snabbt skapa första sidor och innehåll (för användning i tester och feedback).
   * Planera för migrering av befintligt innehåll.
   * Planera för&quot;in-sprint-migration&quot; efter omfaktorisering.
   * Planera&quot;content burndown&quot; (platskarta för publicerat innehåll).

## Beräknar tid och insats {#estimating-time-and-effort}

Beroende på vilken uppgiftslista du skapar kan du sedan göra en första uppskattning av tid och arbete för (hög nivå) uppgiftsdefinitioner. Dessa uppskattningar ska innehålla en indikation på vem (kund eller partner) som gör vad och när.

I följande lista visas ungefärliga uppskattningar och inbördes samband mellan ansträngningarna, och därmed kostnader:

>[!CAUTION]
>
>Dessa siffror kan endast användas för ursprungliga uppskattningar. En erfaren AEM-utvecklare måste göra en detaljerad analys.

| Fas | Insats |
|---|---|
| Utveckling | En ungefärlig uppskattning på 2-4 timmar för varje komponentnod som täcker alla utvecklingskrav. |
| Testning av utvecklare | 15 % av utvecklingsarbetet |
| Uppföljning | 10 % av utvecklingsarbetet |
| Dokumentation | 15 % av utvecklingsarbetet |
| JavaDoc-dokumentation | 10 % av utvecklingsarbetet |
| Felkorrigering | 15 % av utvecklingsarbetet |
| Projektledning | 20 % av projektkostnaderna för projektledning |

Detaljerad planering kan sedan relatera tillgängliga eller nödvändiga resurser till deadlines och kostnader.

## Referensarkitektur {#reference-architecture}

Referensarkitekturen är avsedd som en malllösning för AEM-arkitekturen. Referensarkitekturen åtgärdar problem som ofta uppstår i företagssystem, t.ex. skalning, tillförlitlighet och säkerhet.

Följande webbplatsmått ska definieras:

| Klassificering | Definition |
|---|---|
| Antal webbplatser |  |
| Antal intranätsplatser |  |
| Antal kodbaser (t.ex. om Internet och intranätet är olika) |  |
| Antal enskilda sidor |  |
| Antal besök på plats/dag |  |
| Antal sidvisningar/dag |  |
| Volym (i GB) för dataöverföring/dag |  |
| Antal samtidiga användare (stängd användargrupp) |  |
| Antal samtidiga besökare (publicera) |  |
| Antal samtidiga författare |  |
| Antal registrerade författare |  |
| Antal sidaktiveringar/arbetsdag |  |
| Antal sidaktiveringar under distributionen |  |

## Översikt över potentiella verktyg {#overview-of-potential-tools}

Följande lista innehåller information om verktyg som kan användas. Den är avsedd som en introduktion, inte som en omfattande rekommendationslista, och ska inte hindra dig från att använda andra verktyg.

<table>
 <tbody>
  <tr>
   <td><strong>Produkt</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>AEM</td>
   <td><p>AEM har också en rad funktioner som hjälper dig att övervaka, testa, undersöka och felsöka programmet:</p>
    <ul>
     <li><a href="/help/sites-developing/developer-mode.md">Utvecklarläge</a></li>
     <li><a href="/help/sites-developing/hobbes.md">Testkonsolen</a></li>
     <li><a href="/help/sites-administering/operations-dashboard.md">Instrumentpanel för åtgärder</a></li>
     <li><a href="/help/sites-authoring/content-insights.md">Content Insight</a></li>
     <li><a href="/help/sites-authoring/author-environment-tools.md#content-tree">Innehållsträdet</a></li>
    </ul> </td>
  </tr>
  <tr>
   <td> </td>
   <td> </td>
  </tr>
  <tr>
   <td>Selen</td>
   <td><a href="https://www.selenium.dev/">Selenium</a> är ett Open Source-testverktyg. Testerna körs direkt i webbläsaren och emulerar hur användarna arbetar.</td>
  </tr>
  <tr>
   <td>Microsoft® Project</td>
   <td>Ett vanligt projekthanteringsverktyg.</td>
  </tr>
  <tr>
   <td>Jira</td>
   <td><a href="https://www.atlassian.com/software/jira">Jira</a> är ett Open Source-verktyg för att spåra och hantera detaljer om programvarufel. Arbetsflöden kan vid behov läggas på felinformationen.</td>
  </tr>
  <tr>
   <td>Git</td>
   <td><a href="https://git-scm.com/">Git</a> är ett program för versionskontroll.</td>
  </tr>
  <tr>
   <td>Eclipse</td>
   <td><p>Eclipse är en öppen Source-utvecklingsmiljö som består av olika projekt. Det handlar om att bygga en öppen utvecklingsplattform som består av flexibla ramverk, verktyg och runtimes för att bygga, driftsätta och hantera programvara under hela livscykeln.</p> <p>Mer information finns i <a href="/help/sites-developing/howto-projects-eclipse.md">Utveckla AEM-projekt med Eclipse</a>.</p> </td>
  </tr>
  <tr>
   <td>IntelliJ</td>
   <td><p>En professionell utvecklingsmiljö (som därför kan ge upphov till licenskostnader) med ett omfattande utbud av funktioner. </p> <p>Mer information finns i <a href="/help/sites-developing/ht-intellij.md">Utveckla AEM-projekt med IntelliJ IDEA</a> .</p> </td>
  </tr>
  <tr>
   <td>Maven</td>
   <td><a href="https://maven.apache.org/">Maven</a> är ett verktyg för projektledning och förståelse för programvara som kan hantera ett projekts byggprocess (programvara och dokumentation).</td>
  </tr>
 </tbody>
</table>

## Ytterligare läsning {#further-reading}

Dessutom är följande avsnitt av särskilt intresse:

* [Komma igång](/help/sites-deploying/deploy.md#getting-started)
* [Tekniska krav](/help/sites-deploying/technical-requirements.md)
* [Övervaka och underhålla din instans](/help/sites-deploying/monitoring-and-maintaining.md)

### Bästa praxis {#best-practices}

Adobe tillhandahåller ytterligare metodtips för alla faser och målgrupper:

* [Distribuerar](/help/sites-deploying/best-practices.md)
* [Redigering](/help/sites-authoring/best-practices.md)
* [Administratör](/help/sites-administering/administer-best-practices.md)
* [Utvecklar](/help/sites-developing/best-practices.md)
* [Projektledning](/help/managing/best-practices.md)
