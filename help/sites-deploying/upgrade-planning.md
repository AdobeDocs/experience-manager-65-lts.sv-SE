---
title: Planera din uppgradering
description: I den här artikeln beskrivs tydliga mål, faser och slutprodukter när du planerar en AEM-uppgradering.
contentOwner: sarchiz
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: upgrading
docset: aem65
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 4e27e726711b7469e1d3fa1f7662ba54aaeba54a
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 0%

---

# Planera din uppgradering{#planning-your-upgrade}

## AEM Project Overview {#aem-project-overview}

AEM används ofta i storslagna installationer som kan hjälpa miljontals användare. Vanligtvis finns det anpassade program som distribueras på instanserna, vilket ökar komplexiteten. Alla försök att uppgradera en sådan distribution måste hanteras metodiskt.

Den här guiden hjälper dig att fastställa tydliga mål, faser och slutprodukter när du planerar en uppgradering. Det fokuserar på det övergripande projektgenomförandet och på riktlinjerna. Den ger en översikt över de faktiska uppgraderingsstegen, men hänvisar till tillgängliga tekniska resurser där det är lämpligt. Den bör användas tillsammans med de tillgängliga tekniska resurser som anges i dokumentet.

AEM uppgraderingsprocess måste noggrant hanteras i planerings-, analys- och körningsfaserna med nyckelresultat definierade för varje fas.

Du kan uppgradera direkt från AEM version 6.0 och upp till 6.5. Kunder som kör 5.6.x och tidigare måste uppgradera först till version 6.0 eller senare, och 6.0(SP3) rekommenderas. Det nya Oak Segment tar-formatet används nu även för segmentnodarkivet sedan 6.3, och databasmigrering till det nya formatet är obligatoriskt även för 6.0, 6.1 och 6.2.

>[!CAUTION]
>
>Om du uppgraderar från AEM 6.2 till 6.3 bör du antingen uppgradera från versioner (**6.2-SP1-CFP1 - -6.2SP1-CFP12.1**) eller **6.2SP1-CFP15** eller senare. Om du uppgraderar från **6.2SP1-CFP13/6.2SP1CFP14** till AEM 6.3 måste du annars även uppgradera till minst version **6.3.2.2**. I annat fall misslyckas AEM Sites efter uppgraderingen.

## Uppgraderingsomfång och krav {#upgrade-scope-requirements}

Nedan finns en lista över områden som påverkas i ett typiskt AEM Upgrade Project:

<table>
 <tbody>
  <tr>
   <td><strong>Komponent</strong></td>
   <td><strong>Effekt</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>Operativsystem</td>
   <td>Osäker, men subtila effekter</td>
   <td>När uppgraderingen av AEM görs kan det vara dags att uppgradera operativsystemet, vilket kan få lite effekt.</td>
  </tr>
  <tr>
   <td>Java™ Runtime</td>
   <td>Måttlig effekt</td>
   <td>AEM 6.3 kräver JRE 1.7.x (64 bitar) eller senare. JRE 1.8 är den enda version som för närvarande stöds av Oracle.</td>
  </tr>
  <tr>
   <td>Maskinvara</td>
   <td>Måttlig effekt</td>
   <td>Onlineversionsrensning kräver <br /> ledigt diskutrymme som är 25 % av databasens storlek och 15 % ledigt stackutrymme <br /> för att slutföras. Du kan behöva uppgradera din maskinvara för att <br /> ska se till att det finns tillräckligt med resurser för att kunna köra onlineversionen av rensning <br /> helt. Om du uppgraderar från en version som är tidigare än AEM 6 kan det dessutom finnas ytterligare lagringsbehov för <br />.</td>
  </tr>
  <tr>
   <td>Innehållsdatabas (CRX eller Oak)</td>
   <td>Hög effekt</td>
   <td>Från och med version 6.1 stöder inte AEM CRX2, så en migrering till <br /> Oak (CRX3) krävs om du uppgraderar från en äldre version. AEM 6.3 har<br /> implementerat ett nytt segmentnodarkiv som även kräver en migrering. Verktyget <br /> crx2oak används för detta.</td>
  </tr>
  <tr>
   <td>AEM Components/Content</td>
   <td>Måttlig effekt</td>
   <td><code>/libs</code> och <code>/apps</code> hanteras enkelt genom uppgraderingen, men <code>/etc</code> kräver vanligtvis manuell återanvändning av anpassningar.</td>
  </tr>
  <tr>
   <td>AEM Services</td>
   <td>Låg effekt</td>
   <td>De flesta av AEM bastjänster har testats för uppgradering. Detta är ett område med låg effekt.</td>
  </tr>
  <tr>
   <td>Anpassade programtjänster</td>
   <td>Låg till hög effekt</td>
   <td>Beroende på programmet och anpassningen kan det finnas <br /> beroenden av JVM, operativsystemversioner och vissa indexeringsrelaterade <br /> ändringar, eftersom index inte genereras automatiskt i Oak.</td>
  </tr>
  <tr>
   <td>Anpassat programinnehåll</td>
   <td>Låg till hög effekt</td>
   <td>Innehåll som inte kommer att hanteras genom uppgraderingen kan säkerhetskopieras <br /> innan uppgraderingen äger rum och sedan flyttas tillbaka till databasen.<br /> Det mesta innehållet kan hanteras med migreringsverktyget.</td>
  </tr>
 </tbody>
</table>

Det är viktigt att du kör ett operativsystem som stöds, Java™-miljön, httpd och Dispatcher-versionen. Mer information finns på sidan [Tekniska krav för AEM 6.5](/help/sites-deploying/technical-requirements.md). Du måste ta hänsyn till att du uppgraderar dessa komponenter i din projektplan och de bör göras innan du uppgraderar AEM.

## Projektfaser {#project-phases}

Mycket arbete går åt till att planera och köra en AEM-uppgradering. För att förtydliga de olika arbetsinsatser som ingår i denna process har Adobe delat upp planerings- och exekveringsövningarna i separata faser. I avsnitten nedan resulterar varje fas i en slutprodukt som ofta används i en framtida fas av projektet.

### Planering för författarutbildning {#planning-for-author-training}

I alla nya versioner finns det risk för förändringar i användargränssnittet och användararbetsflöden. Nya releaser innehåller också nya funktioner som kan vara till nytta för företaget. Adobe rekommenderar att du granskar de funktionsändringar som har gjorts och organiserar en plan för att utbilda dina användare i att använda dem effektivt.

![unu_cropped](assets/unu_cropped.png)

Nya funktioner i AEM 6.5 finns i [AEM-delen av adobe.com](/help/release-notes/release-notes.md). Observera alla ändringar av användargränssnitt och produktfunktioner som används ofta i din organisation. När du tittar igenom de nya funktionerna bör du också tänka på alla funktioner som kan vara av värde för din organisation. När du har gått igenom vad som har ändrats i AEM 6.5 kan du ta fram en utbildningsplan för dina författare. Det kan handla om att använda kostnadsfria resurser som hjälpfunktionsvideor eller formell utbildning som erbjuds via [Adobe Digital Learning Services](https://learning.adobe.com/).

### Skapa en testplan {#creating-a-test-plan}

Varje kunds implementering av AEM är unik och har anpassats för att uppfylla deras affärskrav. Därför är det viktigt att fastställa alla anpassningar som har gjorts i systemet så att de kan inkluderas i en testplan. Testplanen kommer att driva den QA-process som Adobe utför på den uppgraderade instansen.

![test-plan](assets/test-plan.png)

Den exakta produktionsmiljön måste dupliceras och testning bör utföras på den efter uppgraderingen för att säkerställa att alla program och anpassad kod fortfarande fungerar som de ska. Regress all anpassning och kör prestanda, inläsning och säkerhetstestning. När du organiserar din testplan måste du ta med alla anpassningar som har gjorts i systemet, förutom de användargränssnitt och arbetsflöden som används i de dagliga åtgärderna. Dessa kan omfatta anpassade OSGI-tjänster och -servrar, integrering med Adobe Experience Cloud, integrering med tredje part via AEM-anslutningar, anpassade tredjepartsintegreringar, anpassade komponenter och mallar, anpassade gränssnittsövertäckningar i AEM samt anpassade arbetsflöden. För kunder som migrerar från en tidigare version än AEM 6 bör alla anpassade frågor analyseras eftersom dessa kan behöva indexeras. För kunder som redan använder en version av AEM 6.x bör dessa frågor fortfarande testas för att säkerställa att deras index fortsätter att fungera effektivt efter uppgraderingen.

### Fastställa nödvändiga arkitektoniska förändringar och infrastrukturförändringar {#determining-architectural-and-infrastructure-changes-needed}

När du uppgraderar kan du behöva uppgradera andra komponenter i din tekniska stack, till exempel operativsystemet eller JVM. På grund av ändringar i databaskonfigurationen kan det dessutom behövas ytterligare maskinvara. Detta gäller endast kunder som migrerar från tidigare versioner än 6.x, men det är viktigt att tänka på. Slutligen kan det finnas ändringar som är nödvändiga i era rutiner för övervakning, underhåll och säkerhetskopiering och katastrofåterställning.

![doi_cropped](assets/doi_cropped.png)

Granska de tekniska kraven för AEM 6.5 och se till att din nuvarande maskin- och programvara är tillräcklig. Följande dokument innehåller information om eventuella ändringar av de operativa processerna:

**Övervakning och underhåll:**

[Instrumentpanel för åtgärder](/help/sites-administering/operations-dashboard.md)

[Assets Monitoring Best Practices](/help/assets/assets-monitoring-best-practices.md)

[Övervakningsserverresurser med JMX-konsolen](/help/sites-administering/jmx-console.md)

[Revision Cleanup](/help/sites-deploying/revision-cleanup.md)

**Säkerhetskopiering/återställning och Disaster Recovery:**

[Säkerhetskopiering och återställning](/help/sites-administering/backup-and-restore.md)

[Prestanda och skalbarhet](/help/sites-deploying/performance.md)

[Så här kör du AEM med TARMK Cold Standby](/help/sites-deploying/tarmk-cold-standby.md)

### Utvärderar uppgraderingskomplexitet {#assessing-upgrade-complexity}

På grund av det stora utbudet av anpassningar som Adobe-kunder använder i sina AEM-miljöer är det viktigt att lägga lite tid på att avgöra hur stor arbetsinsatsen ska vara i uppgraderingen.

Det finns två sätt att bedöma uppgraderingens komplexitet. En inledande fas kan använda den nya mönsteravkännaren, som är tillgänglig för körning i dina AEM 6.1-, 6.2- och 6.3-instanser. Mönsterdetektorn är det enklaste sättet att bedöma uppgraderingens totala komplexitet som kan förväntas med hjälp av rapporterade mönster. Mönsterdetektorrapporten innehåller mönster för att identifiera otillgängliga API:er som används av den anpassade kodbasen (detta gjordes med kompatibilitetskontrollerna före uppgradering i 6.3).

Efter den första bedömningen kan ett mer omfattande nästa steg vara att utföra en uppgradering av en testinstans och utföra några grundläggande röktester. Adobe har också vissa funktioner. Listan över [borttagna och borttagna funktioner](/help/release-notes/release-notes.md#deprecated-and-removed-features) bör granskas inte bara för den version du uppgraderar till, utan även för alla versioner mellan käll- och målversionerna.

![trei_cropped](assets/trei_cropped.png)

Den mönsteravkännare som introducerades nyligen bör ge en korrekt uppskattning av vad du kan förvänta dig under en uppgradering i de flesta fall. För mer komplexa anpassningar och distributioner där du har inkompatibla ändringar kan du uppgradera en utvecklingsinstans till AEM 6.5 enligt instruktionerna i [Utföra en lokal uppgradering](/help/sites-deploying/in-place-upgrade.md). När det är klart utför du några högnivåröktester på den här miljön. Målet med denna övning är inte att göra en omfattande inventering av testfall och göra en formell inventering av defekter, utan att ge oss en ungefärlig uppskattning av mängden arbete som krävs för att uppgradera koden för kompatibilitet med 6.5. I kombination med [Mönsteridentifiering](/help/sites-deploying/pattern-detector.md) och de arkitektoniska ändringar som bestämdes i föregående avsnitt, kan en grov uppskattning ges till projekthanteringsteamet för planering av uppgraderingen.

### Bygga Runbook för uppgradering och återställning {#building-the-upgrade-and-rollback-runbook}

Adobe har dokumenterat processen för uppgradering av en AEM-instans, men varje kunds nätverkslayout, driftsättningsarkitektur och anpassningar kräver att man finjusterar och skräddarsyr den här metoden. Därför rekommenderar Adobe att du granskar all bifogad dokumentation och använder den för att informera en projektspecifik Runbook som beskriver de uppgraderings- och återställningsprocedurer som du kommer att följa i din miljö. Om du uppgraderar från CRX2 måste du se till att utvärdera hur lång tid det tar att migrera innehåll när du går från CRX2 till Oak. För stora databaser kan det vara mycket viktigt.

![runbook-chart](assets/runbook-diagram.png)

Adobe har tillhandahållit uppgraderings- och återställningsprocedurer i [uppgraderingsproceduren](/help/sites-deploying/upgrade-procedure.md) och stegvisa instruktioner för hur uppgraderingen ska användas i Utföra en [lokal uppgradering](/help/sites-deploying/in-place-upgrade.md). Dessa instruktioner bör granskas och övervägas med din systemarkitektur, anpassningar och driftsavvikelse för att avgöra vilka procedurer för växling och återställning som du ska utföra under uppgraderingen. Alla ändringar av arkitektur eller serverstorlekar bör inkluderas när du skapar din anpassade runbook. Det är viktigt att notera att detta bör behandlas som ett första utkast. När teamet slutför sina QA- och utvecklingscykler och distribuerar uppgraderingen till testmiljön är det troligt att det krävs ytterligare åtgärder. Helst bör det här dokumentet innehålla tillräckligt med information så att om det skulle överlämnas till en medlem av personalen kan de slutföra uppgraderingen helt utifrån informationen i det.

### Utveckla en projektplan {#developing-a-project-plan}

Utdata från tidigare övningar kan användas för att bygga en projektplan som täcker de förväntade tidslinjerna för test- eller utvecklingsarbete, utbildning och faktiskt utförande av uppgraderingen.

![develop-project-plan](assets/develop-project-plan.png)

En omfattande projektplan bör omfatta följande:

* Slutförande av utvecklings- och testplaner
* Uppgraderar utvecklings- och QA-miljöer
* Uppdatera den anpassade kodbasen för AEM 6.5
* En QA-provning och korrigeringscykel
* Uppgraderar mellanlagringsmiljön
* Integrering, prestanda och belastningstestning
* Miljöcertifiering
* Go live

### Utveckling och kvalitetskontroll {#performing-development-and-qa}

Adobe har tillhandahållit procedurer för [Uppgradering av kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md) som är kompatibla med AEM 6.5. När den här iterativa processen körs bör ändringar göras i Runbook efter behov. Se även [Bakåtkompatibilitet i AEM 6.5](/help/sites-deploying/backward-compatibility.md) om hur dina anpassningar kan vara bakåtkompatibla normalt utan att behöva utvecklas omedelbart efter uppgraderingen.

![patru_cropped](assets/patru_cropped.png)

Utvecklings- och testprocessen är vanligtvis iterativ. På grund av anpassningar kan ändringar som görs under uppgraderingen göra att en hel del av produkten blir oanvändbar. När utvecklarna har åtgärdat den grundläggande orsaken till problemet och testteamet har tillgång till dessa funktioner, finns det risk för att ytterligare problem upptäcks. När problem upptäcks som kräver justeringar i uppgraderingsprocessen måste du lägga till dem i din anpassade uppgraderingsrunbook. Efter flera iterationer av testning och korrigering bör kodbasen vara helt validerad och klar för distribution till testmiljön.

### Slutlig testning {#final-testing}

Adobe rekommenderar en sista testomgång efter att kodbasen har certifierats av din organisations QA-team. Denna testomgång innebär att du validerar din runbook i en staging-miljö, följt av rundor där användaren accepteras, prestanda och säkerhetstestning.

![cinci_cropped](assets/cinci_cropped.png)

Det här steget är viktigt eftersom det är enda gången som du kan validera stegen i Runbook mot en produktionsliknande miljö. När miljön har uppgraderats är det viktigt att användarna ges tid att logga in och gå igenom de aktiviteter de utför när de använder systemet i sina dagliga aktiviteter. Det är inte ovanligt att användare använder en del av systemet som inte tidigare övervägts. Att hitta och åtgärda problem i dessa områden innan du publicerar produkten kan bidra till att förhindra kostsamma produktionsavbrott. Som en ny version av AEM innehåller betydande förändringar av den underliggande plattformen är det också viktigt att utföra prestanda-, laddnings- och säkerhetstester på systemet som om det startades för första gången.

### Utföra uppgraderingen {#performing-the-upgrade}

När den slutliga signeringen har tagits emot från alla intressenter är det dags att utföra de definierade Runbook-procedurerna. Adobe har tillhandahållit steg för uppgradering och återställning i [uppgraderingsproceduren](/help/sites-deploying/upgrade-procedure.md) och installationssteg i Utföra en [lokal uppgradering](/help/sites-deploying/in-place-upgrade.md) som referenspunkt.

![perform-upgrade](assets/perform-upgrade.png)

Adobe har tagit del av uppgraderingsinstruktionerna för miljövalidering. Dessa omfattar grundläggande kontroller som att skanna uppgraderingsloggarna och verifiera att alla OSGi-paket har startats korrekt, men Adobe rekommenderar även att du validerar med dina egna testfall baserat på dina affärsprocesser. Adobe rekommenderar också att du kontrollerar schemat för AEM Online Revision Cleanup och tillhörande rutiner för att vara säker på att de inträffar under en lugn tid för ditt företag. Dessa rutiner är viktiga för AEM långsiktiga prestanda.
