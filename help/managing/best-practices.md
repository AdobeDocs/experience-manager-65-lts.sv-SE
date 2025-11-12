---
title: Hantera projekt - checklista för bästa praxis
description: För att hantera ett projekt som ska implementera Adobe Experience Manager (AEM) krävs planering och förståelse. Projektchecklistor är avsedda som en uppsättning bästa metoder för projektleverans. De vägleder dig genom alla faser i projektets livscykel och ger dig en högnivåövervakning av din status.
solution: Experience Manager, Experience Manager 6.5 LTS
feature: Compliance
role: Admin,Architect,Data Architect,Developer,Leader
exl-id: 8f8883d8-4e2b-4ba0-bd83-414a96e7d382
source-git-commit: 58c2f666ef6fd89fc42f5d969d1ef1626e45fe45
workflow-type: tm+mt
source-wordcount: '3212'
ht-degree: 0%

---

# Hantera projekt - checklista för bästa praxis{#managing-projects-best-practices-checklist}

Att hantera ett projekt som ska implementera Adobe Experience Manager (AEM) kräver planering och förståelse så att du är medveten om problemen och (de relaterade) besluten som du måste fatta, innan och medan du implementerar projektet.

De bästa sätten är att

* En [interaktiv checklista](/help/managing/best-practices-checklist.md) som gör att du kan spåra och övervaka dina framsteg med de här bästa metoderna.

   * Definierar indata och slutprodukter utifrån fas, milstolpe och persona.
   * Ger automatiska översikter (kvalitet, hälsa och fullständighet) som visar på framsteg och projekthälsa.

* Dokumentation baserad på [checklistan](/help/managing/best-practices-checklist.md) som innehåller information om:

   * [Projektpulsslaganalys](#projectheartbeat).
   * [Status efter roll](#status-by-role) - översikt.
   * [Faser och milstolpar](#phases-and-milestones).
   * [Nyckelpersonal](#persona) och deras medverkan i alla (relevanta) steg.
   * En [ordlista](/help/managing/best-practices-glossary.md) med [dokument och slutprodukter som krävs](#required-documents-and-deliverables).

* [Mer referensmaterial](/help/managing/best-practices-further-reference.md) ger mer information om specifika områden.

## Kontrollpanel för projektpulsslag {#project-heartbeat-dashboard}

Kalkylbladet **Projektpulsslag** innehåller en grafisk översikt över kritiska mätvärden för ditt projekt:

* **Faskvalitet**

   * Anger kvaliteten på [obligatoriska dokument och slutprodukter](#required-documents-and-deliverables) i projektet.

* **Fashälsa**

   * En statusindikator på hög nivå för ditt projekt, användbar för att markera områden som kan vara i fara.

* **Fullständighet för fas**

   * När som helst under projektet visar detta hur mycket som redan har slutförts för varje fas i projektet.

## Status efter roll {#status-by-role}

Kalkylbladet **Status efter roll** visar detaljerad beskrivning av [**Hälsa**, **Kvalitet och &#x200B;** Fullständighet&#x200B;**](#projectheartbeat) av &#x200B;** [Fas](#phases-and-milestones)**&#x200B; och &#x200B;** [Persona](#persona)**.

## Faser och milstolpar {#phases-and-milestones}

Projektplanen är uppdelad i olika faser (på hög nivå).

Varje fas innehåller sina egna milstolpar. För varje [persona](#persona) (eller roll) listas de relevanta milstolparna tillsammans med de dokument som krävs för att skapa de definierade slutprodukterna.

>[!NOTE]
>
>Det finns ingen direkt :1-relation mellan enskilda obligatoriska dokument och slutprodukter.

### Förberedelse {#preparation}

Förberedelser av ditt projekt utgör grunden för hela projektet. Definiera viktiga krav tillsammans med tydliga mål och förväntningar för:

* **Affärslogik**

   * De grundläggande skälen och motiveringen till att projektet genomförs.

* **Omfång och schema**

   * Ett grundläggande omfång och grovschema bör göras tillgängligt för att definiera vad som krävs och inom vilken tidsram. Om det hjälper till att klargöra situationen kan du också definiera vad som ligger utanför omfånget.

Hur du förbereder, planerar och kör ditt projekt och implementerar din lösning påverkas av de begränsningar du har. Exempel: fast budget, fast tidslinje, innehållskvantitet, kvalitet som krävs.

Som alltid påverkar justeringen av någon av faktorerna de andra. Om du till exempel minskar tiden, men behöver samma kvalitetsnivå, kommer priset förmodligen att öka samtidigt som du minskar mängden innehåll som du kan hantera. Budgeten är ofta en nyckelfaktor, så sådana relationer kan inte glömmas bort.

Fyra faktorer:

![projectfaser_fourfaser](assets/projectphases_fourphases.png)

#### Milstolpar {#milestones}

* **Validering**

  I den här fasen måste du validera och bekräfta målen för projektet, till exempel:

   * Vad vill du uppnå/tillhandahålla?
   * Vem tjänar på det?
   * Vad är omfattningen?

      * Om det gör det lättare att klargöra situationen kan du också definiera vad som ligger utanför omfånget.

   * Hur definierar man framgång?
   * Hur mäter man framgång?
   * Vilka är kraven, affärsvillkoren och de tekniska kraven?
   * Finns det äldre system som ska ersättas och, om så är fallet, finns det data som ska migreras?
   * Vilka är inblandade?
   * Hur mäter du framstegen?
   * Hur ofta granskar du projektets förlopp?

* **Budget**

  Innan du påbörjar ett projekt behöver du en tillförlitlig och realistisk uppskattning av kostnaderna:

   * Använd information från valideringsmilstolpen som grund för uppskattningarna.
   * Var realistiska i era uppskattningar.
   * Ta hänsyn till och följ eventuella riktlinjer, processer eller begränsningar som kunden omfattas av.
   * Fundera på beredskaps- och granskningsprocesser om en granskning eller finjustering av budgeten krävs senare.
   * Kom ihåg att kostnader kan tillskrivas många olika former, till exempel inköp, användning av resurser och avgifter.

### Planering {#planning}

När du planerar projektet konsolideras förberedelsen. Här bör ni börja konvertera målen och förväntningarna till en väldefinierad färdplan som består av konkreta uppgifter, som är bundna av tydlig kommunikation, med strikta granskningar för att mäta framstegen.

#### Milstolpar {#milestones-1}

* **Lämna över**

  Ett rent överlämnande säkerställer att rätt person/grupper är medvetna om sitt ansvar inom projektet.

  Fullständiga uppgifter bör tillhandahållas/genereras för att säkerställa att de har fullständig förståelse för alla relevanta aspekter, inklusive färdplanen, omfattningen, målen, kraven och nyckeltalen.

* **Riskbedömning**

  För att undvika obehagliga överraskningar bör man använda riskbedömningar för att identifiera och kvantifiera eventuella risker tillsammans med deras konsekvenser och sannolikhet.

  Detta bör göras tidigt i projektets livscykel för att säkerställa att eventuella sårbarheter identifieras och utvärderas. Baserat på resultaten kan ni rapportera till era intressenter om huruvida de fullständiga kraven kan genomföras och, om det behövs, om det är möjligt att planera för lämpliga åtgärder som ska vidtas och spåras.

* **Kommunikation**

  Kommunikation är alltid avgörande för att ett projekt ska lyckas. Kommunicera tydligt och effektivt för att säkerställa att alla är

   * Att arbeta mot samma grundläggande mål
   * Från samma informationsbas
   * Med samma kanaler

* **Stäng av**

  Mötet med avaktivering används för att öka medvetenheten om att projektet har startat. Det är en bra möjlighet att

   * Bjud in alla berörda parter (eller åtminstone grupprepresentanter).
   * Presentera viktiga fakta om projektet.
   * Svara på frågor.
   * Se till att alla har samma kunskapsbas.
   * Engagemang från alla som kommer att vara inblandade - det här måste du tjäna på.

      * Genom att involvera huvudaktörer (inklusive potentiella författare) i början av projektet ökar ni chanserna att få dem engagerade i projektet.

### Utvecklingsförberedelser {#development-preparation}

Att planera utvecklingsarbetet är avgörande för att säkerställa att projektet byggs på en stabil design av ett team som har den kunskap som krävs.

#### Milstolpar {#milestones-2}

* **Utvecklingsteamet har bemannat sig och utbildat**

  Innan du börjar med ett projekt bör du se till att ditt utvecklingsteam har lämplig personal och att alla teammedlemmar har utbildning för den aktuella uppgiften.

* **Innehållsarkitektur**

  Innehållsarkitekturen definierar och beskriver innehållets framtida arkitektur, inklusive:

   * Innehållsträdet, inklusive resurser
   * Grundläggande strukturer, inklusive kampanjer och så vidare.
   * Flersidiga och flerspråkiga strukturer (MSM, Translation osv.)
   * Innehåll som stöds (inklusive taggar och taggar)
   * Strategier för cachning och återanvändning av innehåll

* **Systemarkitektur**

  Systemarkitekturen definierar den konceptuella vyn för ditt system, inklusive (bland annat information):

   * [Systemstruktur](/help/sites-deploying/recommended-deploys.md#deployment-scenarios) för alla nödvändiga miljöer
   * Delsystem
   * Tredjepartssystem
   * Gränssnitt, maskinvara, programvara och mänsklig interaktion
   * Servrar för varje miljö. Se riktlinjerna för [tekniska krav](/help/sites-deploying/technical-requirements.md) och [Maskinvarans storlek](/help/managing/hardware-sizing-guidelines.md)

   * Processer för varje miljö, t.ex. krav på driftsättning och underhåll
   * Underhållsaktiviteter (Datastore GC, TarPM-optimering och så vidare)
   * [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)-cachelagring
   * [Klustring](/help/sites-deploying/recommended-deploys.md#deployment-scenarios) Publicera/författarresurs
   * Prestanda för klientsidan (JS minify, concat, css sprites, total number of http requests, and others)

* **Programarkitektur**

  Programarkitekturen definierar och beskriver de föreslagna programmens beteende.

  Den fokuserar på:

   * Hur de interagerar med varandra och med användarna.
   * De data som ska användas och produceras av program, i stället för deras interna struktur.

  Definitionerna bör omfatta följande:

   * Grundläggande kodstruktur för projektet
   * Kodartefakter (paket, paket och så vidare)
   * Uppdelning av mallar/komponenter och deras relationer
   * Detaljerad information om nödvändiga anpassningar (specifika överlägg följer senare)
   * Utformning av de arbetsflöden som lösningen kräver (t.ex. skapande av innehåll, godkännande, publicering, omvandlingar, import och export)
   * Särskild hänsyn till komplexa moduler som MSM, Commerce och tredjepartsintegrering

* **Systemintegrering**

  Systemintegrering kräver att du planerar (och sedan implementerar):

   * Hur alla delsystem och [lösningsintegreringar](/help/sites-administering/integration.md) samverkar för att fungera som ett sammanhängande system
   * Hur tredjepartssystem är integrerade, tillsammans med eventuella specialöverväganden som offline/online, klientsidan/webbläsaren eller reservhantering när ett tredjepartssystem är nere

* **Testa konceptet**

  Innan du börjar utveckla bör du skapa ett ingående och omfattande koncept för alla [testningskrav](/help/sites-developing/planning.md) för ditt projekt.

  Detta bör bland annat omfatta följande:

   * Uppgifter om alla tester som ska utföras
   * Förberedelse av allt innehåll som krävs för dessa tester
   * Information om eventuella testverktyg som ska användas
   * Indikation på hög nivå om vem som kommer att delta i testningen, särskilt grupper utanför QA-teamet
   * Detaljer om automatiserad testning, t.ex. med Selenium eller AEM Developer mode

* **Experience Design**

  Experience Design (XD) innebär att utforma användarupplevelsen för er lösning.

  Användarupplevelsen bör analyseras och utvecklas för både författarna och slutanvändarna av webbplatsen.

* **Supportinställningar**

  Innan utveckling bör alla supportprocesser som krävs för att driftsätta, frisläppa, testa och rapportera problem fastställas.

  Se även [Adobe supportportal](https://experienceleague.adobe.com/?support-solution=General&support-tab=home#support).

### Planering och drift {#operations-planning-and-operations}

På liknande sätt måste åtgärderna planeras på rätt sätt för att säkerställa att du har de miljöer du behöver - för alla faser av projektets livscykel. Ni behöver också rätt processer för att underhålla dem.

#### Milstolpar {#milestones-3}

* **Behörigheter**

  Du måste planera och sedan implementera ett rolls- och rättighetskoncept för alla användare/grupper som ska använda lösningen.

  Till exempel:

   * En lista över roller (d.v.s. grupper) med `read`/ `write` åtkomstdefinitioner för varje

   * Definition av användning av behörigheter som påverkar publiceringsmiljön, till exempel `replicate`
   * För användare med minimal behörighet bör arbetsflöden definieras
   * Användare i gruppen `editor` ska inte ha `admin`-rättigheter eller vara en del av gruppen `administrators`

  Mer information finns i [Användaradministration och -säkerhet](/help/sites-administering/security.md).

* **Övervakning och underhåll**

  Övervakning och underhåll är viktiga aspekter av att säkerställa att lösningen fungerar smidigt när den är klar. Därför måste du definiera:

   * Vad behöver övervakas
   * Underhållsuppgifter; både regelbundna och i särskilda fall

  Se även [Övervakning och underhåll](/help/sites-deploying/monitoring-and-maintaining.md) för mer information.

* **Migrering**

  Allt innehåll från det äldre systemet ska granskas och valideras för migrering.

* **Återställningsplan**

  Kontrollera att du har en återställningsplan. I en krissituation måste denna vara tillgänglig för att säkerställa produktionsanvändningen av AEM. Detta bör omfatta situationer som säkerhetskopiering, återställning, återställning och reservlösningar.

### Utveckling {#development}

Utveckling är en avgörande fas som kräver mer än bara kodning.

#### Milstolpar {#milestones-4}

* **Utvecklingsmiljö**

  Planera och dokumentera utvecklingsmiljön, inklusive:

   * Arkitektur
   * [Utvecklingsverktyg](/help/sites-developing/dev-tools.md)

      * En typisk miljö består av:

         * ett system för uppföljning av problem, som Jira
         * en IDE, till exempel Eclipse
         * ett bygghanteringsverktyg, som Maven
         * ett verktyg för kontinuerlig integrering, till exempel Jenkins
         * ett verktyg för versionskontroll, till exempel GIT/SVN
         * en databashanterare för byggarfelaktigheter, som Archiva/Nexus

   * Programintegrering/beroenden från tredje part
   * [Integrering/beroenden av lösningar](/help/sites-administering/integration.md)
   * Distributionscadence

* **Testa systemet**

  Planera och dokumentera testmiljön, inklusive:

   * Arkitektur
   * Beroende på utvecklingsverktyg, inklusive nattbyggen
   * Möjligheterna eller begränsningarna med att testa integrering/beroenden av tredjepartsprogram
   * Testverktyg
   * Automatiserad testningsstrategi

* **Produktionssystem**

  Planera och dokumentera produktionsmiljön:

   * Arkitektur
   * Distributionscadence
   * Programintegrering/beroenden från tredje part
   * Säkerhetsinställningar
   * Baslinjeprestanda verifierat genom att köra [Tough Day-tester](/help/sites-developing/tough-day.md) i produktionsinställningarna
   * Krav för prestandatester. Mer information finns i [Bästa praxis för Assurance med hög kvalitet](/help/sites-deploying/configuring-performance.md#best-practices-for-quality-assurance)

* **Integrering**

  Planera, dokumentera och testa alla aspekter av systemet och [lösningsintegrering](/help/sites-administering/integration.md), inklusive:

   * En automatiserad testningsstrategi
   * Automatiserade processer för att [flytta program från utveckling till test och sedan produktion](/help/managing/enterprise-devops.md#code-movement)
   * Automatiserade processer för att [flytta innehåll från produktion till testning och utveckling](/help/managing/enterprise-devops.md#content-movement)

* **Migrering**

  Planera, dokumentera och testa alla aspekter av innehållsmigreringen, inklusive:

   * Innehållsarkitektur
   * Migreringsstrategi

* **Kommunikation**

  Se till att alla teammedlemmar och projektmedlemmar hålls uppdaterade vid behov.

* **Dokumentation**

  Dokumentera lösningen fullständigt, inklusive:

   * Drifthandbok
   * Anpassningar som kan påverka uppgraderingar
   * Versionsinformation

### Prestanda och testning {#performance-and-testing}

När det nya programmet är tillgängligt måste det genomgå strikta tester, både för funktionalitet och [prestanda](/help/sites-deploying/configuring-performance.md).

>[!NOTE]
>
>Alla testteam bör tillåtas förbli neutrala och leverera testresultaten.
>
>Det är projektledarens ansvar att bedöma eventuella konsekvenser av resultaten och besluta om lämpliga åtgärder.

#### Milstolpar {#milestones-5}

* **Test för godkännande av slutanvändare**

  [Testning av användargodkännande](/help/sites-developing/acceptance-signoff.md) (UAT) är avgörande för att säkerställa att:

   * Lösningen uppfyller användar-/kundkraven
   * Kunden/användarna accepterar lösningen (funktion, design och prestanda)

  Det bör finnas en formaliserad checklista för kundöverlämning; idealiskt automatiserad och kan köras varje natt mot en ögonblicksbild. Resultatet ska skickas till projektledaren och utvecklingsgruppen

* **Prestanda- och inläsningstester**

  Prestanda- och belastningstester används för att säkerställa att lösningen uppfyller de nödvändiga prestandanivåerna vid medelbelastning och toppbelastning.

  Mer information om prestandatestning finns i:

   * [Prestandatestning](/help/sites-deploying/configuring-performance.md)
   * [Planera och köra testning](/help/sites-developing/planning.md)

   * [Riktlinjer för grundläggande prestanda](/help/sites-deploying/configuring-performance.md#basic-performance-guidelines)

  >[!NOTE]
  >
  >Denna process måste fortsätta under normal användning av AEM, men dessa inledande steg är de viktigaste.

### Utrullning {#rollout}

För att lansera ditt nya program krävs noggrann planering för att säkerställa en smidig Go Live. Detta innefattar att bekräfta en hög säkerhetsnivå, utbilda alla potentiella användare och göra flera torrperioder för att bekräfta att alla frågor har behandlats.

#### Milstolpar {#milestones-6}

* **Förberedelse**

  Förberedelser och planering kommer att bidra till en smidig utrullning.

* **Utbildning**

  se till att all berörd personal har utbildats.

  Se [Adobe Experience Manager](https://training.adobe.com/training/courses.html#solution=adobeExperienceManager) i kurskatalogen.

* **Administratörer har utbildats**

  Se till att era lösningsadministratörer har:

   * Har tränats
   * erhållit lämpligt utbildningsmaterial
   * Lämplig dokumentation togs emot

* **Användare utbildade**

  Kontrollera att författarna har:

   * Har tränats
   * erhållit lämpligt utbildningsmaterial
   * Lämplig dokumentation har tagits emot, till exempel användarhandboken

* **Träningstester**

  Penetrationstester simulerar en attack på ett datorsystem för att identifiera potentiella säkerhetsbrister.

* **Testning av penetration/säkerhet**

  För att försäkra dig om att din lösning är säker ska du utföra specifika penetrationstester tillsammans med ett större antal säkerhetstester.

  Mer information finns i [Säkerhetschecklistan](/help/sites-administering/security-checklist.md).

### Go Live {#go-live}

Du vill att ditt Go Live ska vara så smidigt som möjligt. Återigen behöver de sista stegen vara att planera för en ren exekvering.

#### Milstolpar {#milestones-7}

* **Förberedelse**

  Förberedelser och planering säkerställer smidiga Go Live.

* **Säkerhet**

  Bekräfta säkerheten för lösningen för både interna och externa användare och deras innehåll.

* **Återställning**

  Se till att alla system, procedurer och mekanismer som krävs för reservlösningar finns på plats innan du publicerar.

* **Support**

  Se till att supporttjänsterna finns på plats och är klara.

* **Övergång**

  Planera och genomför övergången till produktionsmiljön och användarna.

* **Utrullning**

  Förbered och genomför röktesterna.

## Persona {#persona}

Checklistorna har utformats av en person. Detta är de roller som är mycket involverade i projektets livscykel.

Det finns även [andra profiler](#other-persona) som är involverade i specifika uppgifter.

### Projektsponsorer {#project-sponsor}

Projektsponsorn är:

* Ansvarig för att tillhandahålla/presentera affärsärendet för projektet.
* Nyckel till att forma och definiera projektets omfattning, inklusive:

   * definition av och kriterier för framgång
   * huvudnyckeltal

* Ange huvudmilstolparna baserat på kundens färdplan.

### Projektledare {#project-manager}

Projektledaren är:

* Ansvarig för den övergripande leveransen av projektet utifrån de krav (t.ex. omfattning, nyckeltal, kriterier för framgång och definition) som projektsponsorn ställt.
* Ansvarig för att definiera budgeten och tilldela resurser till projektet baserat på den budgeten.
* Huvudpunkten för kommunikation för alla personer som deltar i projektet.

### Arkitekt {#architect}

Lösningsarkitekten:

* Ansvarar för lösningens och systemets konstruktion på hög nivå.
* Hjälper till att definiera implementeringsstrategin för AEM. Exempel: om en klustrad installation ska implementeras, om ett kallt vänteläge eller när ett leveransnätverk (CDN) krävs.
* Definiera också AEM lösningsarkitektur baserat på kundens behov. Detta kan omfatta konceptet för användarroller (med relaterade behörigheter), relationen mellan mallar och komponenter eller när multisitehantering ska användas.

### Affärsanalytiker {#business-analyst}

Affärsanalytiker:

* Ansvarar främst för att samla in och analysera kraven på hög nivå och sedan omvandla dessa till specifikationer:

   * för projektledaren som ska användas vid planering av utvecklingen
   * så att utvecklingsteamet kan arbeta från design och utveckling.

* Fungerar nära tillsammans med kunden för att analysera kraven. De matchar mot:

   * Definitionen av framgång.
   * Kriterierna för framgång.
   * KPI:er (både affärs- och resultatbaserade).

### Utvecklingsansvarig {#development-lead}

Utvecklingsledd:

* Ansvarar för det tekniska genomförandet av projektet.
* Ansvarar för att välja en utvecklingsmetod som uppfyller kundens krav.
* Utarbeta en utvecklingsstrategi:

   * säkerställa att den är anpassad efter nyckeltal för verksamhet och prestanda
   * med beaktande av kriterier för framgång och definition,

* Fungerar nära ihop med arkitekten (särskilt när du utarbetar utvecklingsstrategin för AEM) för att definiera aspekter som förhållandet mellan mallar och komponenter, integreringsstrategin för tredjepartsprogram och eventuella specialfunktioner.

### Kvalitetslead {#quality-lead}

Kvalitetsledd:

* Ansvarar för kvaliteten på leveransen och ser till att den uppfyller kriterierna för framgång och alla nyckeltal som definieras av klienten.
* Definierar kvalitetsstatistik, anpassar sig till alla intressenter, utarbetar testningsplaner och ser till att de genomförs.
* Skapar och levererar rapporter till projektintressenter.

### Systemtekniker {#system-engineer}

Systemteknikern:

* Ansvarar för att övervaka projektinfrastrukturen.
* Ansvarar för:

   * konfiguration av interna utvecklings- och testmiljöer
   * för att matcha dessa system med klientsystemen

* Tillhandahåller maskinvarurekommendationer, övervakar de olika implementeringarna och tillhandahåller driftstöd både före och efter körning.

### Säkerhetsansvarig {#security-lead}

Säkerhetsledaren:

* Ansvarar för lösningens övergripande säkerhetskoncept och ser till att det är i linje med alla krav och policyer från kunden.
* Ger ett säkerhetskoncept, säkerhetsfunktioner och rekommendationer för alla maskinvarubaserade säkerhetskoncept, som zoner och brandväggar.

### Annan person {#other-persona}

* Intressenter

   * Personer (ofta från företaget) som är intresserade av att projektet lyckas. De bidrar ofta till budgeten.

* Juridisk

   * Juridisk rådgivning krävs vid förhandlingar om kontrakt.

* Utbildare

   * Beroende på projektets omfattning och karaktär kan specialiserade utbildare användas för att utveckla och presentera utbildningstillfällen för de relevanta grupperna.

* Teknikskribenter

   * Beroende på projektets omfattning och karaktär kan teknikskribenter använda särskilda riktlinjer och handböcker för särskilda grupper. Exempel: en underhållshandbok för systemadministratörer eller en användarhandbok för författarna.

* Systemadministratörer

   * Ansvarig för den pågående driften av systemet.

* Författare och slutanvändare

   * De personer som använder systemet för att skapa och underhålla webbplatsinnehåll.

## Begärda dokument och slutprodukter {#required-documents-and-deliverables}

Checklistorna täcker **obligatoriska dokument** och **slutprodukter** för varje milstolpe.

* Det finns ingen :1-relation mellan dessa. En grupp av obligatoriska dokument kan till exempel resultera i en enda slutprodukt.
* En slutprodukt från en person kan vara ett obligatoriskt dokument för en annan person under samma milstolpe.

### Obligatoriska dokument {#required-documents}

**Begärda dokument** behövs av rätt person när deras produkter produceras.

För varje **obligatoriskt dokument** ska personen ange:

* **J/N**: Om den har tagits emot.
* **1-3**: En indikation på kvaliteten på det mottagna dokumentet.

### Leveranser {#deliverables}

För varje milstolpe ansvarar rätt person för att leverera specifika dokument och därmed ta sitt ansvar för en viss milstolpe.

För varje **slutprodukt** måste personen ange:

* **J/N**: Om den har slutförts.

Leveranser används ofta som **Obligatoriska dokument** för den aktuella eller en senare milstolpe.

## Relaterad bästa praxis {#related-best-practices}

De bästa sätten att distribuera, administrera, utveckla eller skapa finns i följande:

* Övriga bästa metoder och riktlinjer för att hantera ett AEM-projekt:
   * [Riktlinjer för maskinvarans storlek](/help/managing/hardware-sizing-guidelines.md)
   * [Enterprise DevOps](/help/managing/enterprise-devops.md)
   * [Bästa praxis för hantering av SEO och URL](/help/managing/seo-and-url-management.md)
   * [AEM och riktlinjerna för webbtillgänglighet](/help/managing/web-accessibility.md)
   * [Allmän dataskyddsförordning](/help/managing/data-protection-and-privacy.md)* [Distribuera och underhålla bästa praxis](/help/sites-deploying/best-practices.md)
* [Administrera metodtips](/help/sites-administering/administer-best-practices.md)
* [Utveckla bästa praxis](/help/sites-developing/best-practices.md)
* [Bästa tillvägagångssätt](/help/sites-authoring/best-practices.md)

## Viktiga dokumentationsområden {#key-documentation-areas}

* AEM Documentation
Dessutom är följande avsnitt av AEM dokumentation av särskilt intresse (listan är dock inte uttömmande):

   * [Dokumentskydd](/help/sites-developing/security.md)
   * [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md)
   * [Enterprise DevOps](/help/managing/enterprise-devops.md)
   * [Maskinvarustorlek](/help/managing/hardware-sizing-guidelines.md)
   * AEM koncept:

      * [Utveckla - grunderna](/help/sites-developing/the-basics.md)
      * [MSM-koncept](/help/sites-administering/msm.md)
      * [HTML-mallspråk (HTL)](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html)

* Relaterad dokumentation

   * Adobe Experience Cloud - [Planering för Adobe Experience Cloud](https://experienceleague.adobe.com/docs/core-services/interface/services/core-services.html)
