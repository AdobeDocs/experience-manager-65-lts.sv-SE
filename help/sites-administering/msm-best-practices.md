---
title: MSM Best Practices
description: Här hittar du bästa praxis som sammanställts av Adobe tekniker och konsultteam så att de kan komma igång med AEM Multi Site Manager.
topic-tags: site-features, best-practices
feature: Multi Site Manager
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 907ed679-5a91-4581-b0ab-ed550586da71
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1599'
ht-degree: 0%

---

# MSM Best Practices{#msm-best-practices}

## Allmänt {#general}

MSM är ett konfigurerbart ramverk för automatisering av innehållsdistribution. Implementeringar omfattar ofta större delar av en webbplats och omfattar organisationer och geografiska områden. Vi rekommenderar därför att du planerar MSM-implementeringar lika noggrant som du planerar din webbplats:

* **Planera struktur och innehållsflöden** noggrant innan implementeringen startas.
* **Behåll så många live-kopior som möjligt.** Bearbetning av live-kopior är en resurskrävande uppgift. Ju fler live-kopior som finns i systemet, desto bättre prestanda kan påverkas: från bearbetning av interna live-kopieringsindex, över live-kopieringsåtgärder som rollouts, till gränssnittsåtgärder som att visa live-kopieringsrelationer i referenslisten för Sites Admin. Bästa sättet är att skapa live-kopior av webbplatser eller grenar av en webbplats, där live-kopierelationerna ärvs till sidorna på webbplatsen eller grenen. Undvik att skapa individuella live-kopior för sidor på en webbplats eller en gren när hela strukturen kan göras till en live-kopia.
* **Anpassa så mycket som behövs, men så lite som möjligt.** MSM har stöd för en hög grad av anpassning (till exempel utrullningskonfigurationer), men det bästa sättet att använda prestanda, tillförlitlighet och uppgraderingsmöjligheter för din webbplats är att minimera anpassningen.
* Upprätta en **styrningsmodell** tidigt och utbilda användare därefter för att säkerställa framgång. En god praxis från en styrningssynpunkt är att **minimera den auktoritet som lokala innehållsproducenter har** för att allokera/ansluta innehåll till andra lokala användare och deras respektive livekopior. Detta beror på att icke-styrda, kedjade arv kan öka komplexiteten i en MSM-struktur avsevärt och påverka dess prestanda och tillförlitlighet.

* När det finns en plan för din struktur, innehållsflöden, automatisering och styrning - **prototyp och grundligt testa ditt system** innan du påbörjar en liveimplementering.
* Tänk på att **Adobe Consulting och ledande systemintegratörer** har djupgående upplevelseplanering och implementering av innehållsautomatisering med MSM, så att de kan hjälpa er att komma igång med ert MSM-projekt och under hela dess implementering.

>[!NOTE]
>
>Mer information om hur du arbetar med MSM finns i kunskapsbasartiklarna:
>
>* [Felsökning av MSM-problem och vanliga frågor ](troubleshoot-msm.md)
>

>[!NOTE]
>
>Du kan också använda [referenskomponenten](/help/sites-authoring/default-components-foundation.md#reference) för att återanvända en sida eller ett stycke. Kom dock ihåg:
>
>* MSM är flexiblare och ger detaljerad kontroll över vilket innehåll som synkroniseras och när.
>* [Kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) rekommenderas nu framför grundkomponenterna.
>

## Live Copy-källor och layoutkonfigurationer {#live-copy-sources-and-blueprint-configurations}

Kom ihåg att en live-kopia kan skapas med [vanliga sidor](/help/sites-administering/msm-livecopy.md#creating-a-live-copy-of-a-page) eller en [plantryckskonfiguration](/help/sites-administering/msm-livecopy.md#creating-a-live-copy-of-a-site-from-a-blueprint-configuration). Båda är giltiga användningsfall.

Ytterligare fördelar med att använda en ritkonfiguration är att de:

* Tillåt författaren att använda alternativet **Rollout** för en plan - att (explicit) överföra ändringar till live-kopior som ärver från den här planen.
* Tillåt författaren att använda **Skapa plats**. På så sätt kan användaren enkelt välja språk och konfigurera strukturen för live-kopian.
* Definiera en standardkonfiguration för utrullning för live-kopior som har en relation till ritningen.

Om det inte finns någon referens till en ritningskonfiguration kan rollouts bara initieras från själva live-kopiorna, vilket i själva verket leder till att innehållet hämtas från källan.

När du skapar en webbplats med en live-kopia är det fördelaktigt att skapa designkonfigurationer för att säkerställa att hela MSM-funktionsuppsättningen är tillgänglig.

>[!NOTE]
>
>Observera att CUG-filer på fliken Behörigheter inte kan rullas ut till Live-kopior från utkast. Planera runt detta när du konfigurerar Live Copy.

## Komponenter- och behållarsynkronisering {#components-and-container-synchronization}

I allmänhet gäller följande som utrullningsregel i MSM för synkronisering av komponenter:

* Komponenter introduceras med synkronisering av alla resurser som ingår i planen.
* Behållare synkroniserar bara den aktuella resursen.

Detta innebär att komponenterna behandlas som en sammanställning och att själva komponenten och alla dess underordnade komponenter i en utrullning ersätts med de i ritningarna. Det innebär att om en resurs läggs till lokalt i en sådan komponent, kommer den att gå förlorad i innehållet i ritningen vid utrullning.

För att ge stöd åt kapsling av komponenter så att lokalt tillagda komponenter bibehålls i en utrullning måste komponenten deklareras som en behållare. Som ett exempel deklareras standardparsysen som en behållare så att den kan hantera lokalt tillagt innehåll.

>[!NOTE]
>
>Lägg till egenskapen `cq:isContainer` i komponenten för att ange den som en behållare.

## Skapa webbplats {#create-site}

Observera att AEM har två huvudmetoder för att skapa live-kopior:

* När [skapar en Live-kopia](/help/sites-administering/msm-livecopy.md#creating-a-live-copy-of-a-page)

  Detta kan betraktas som ett mer allmänt tillvägagångssätt, vilket gör att du kan skapa live-kopior från vilken sida som helst. Innehållsstrukturen för en live-kopia matchar källan exakt.

* När [skapar en plats](/help/sites-administering/msm-livecopy.md#creating-a-live-copy-of-a-site-from-a-blueprint-configuration)

  Detta är ett mer specialiserat tillvägagångssätt, främst för att skapa webbplatser med en flerspråkig struktur.

Tänk på följande när du skapar en plats:

* Om du vill skapa en plats behöver du en [plantryckskonfiguration](/help/sites-administering/msm-livecopy.md#managing-blueprint-configurations).
* Om du vill att språksökvägar ska kunna väljas på en ny plats måste motsvarande språkrörelser finnas i planen (källa).
* När en [ny webbplats har skapats som en live-kopia](/help/sites-administering/msm-livecopy.md#creating-a-live-copy-of-a-site-from-a-blueprint-configuration) (med **Create** och sedan **Site**) är de två första nivåerna i den här live-kopian *shallow*. Sidans underordnade hör inte till live-relationen, men en utrullning kommer ändå att gå ned om en live-relation som matchar utlösaren hittas.

  Det hjälper till att undvika:

   * manuellt lägga till språk i planen (under den första nivån)
   * manuellt lägga till innehåll direkt under språkroten,
   * leder inte till att det nya innehållet automatiskt överförs till den aktiva kopian vid utrullning.

## MSM och flerspråkiga webbplatser {#msm-and-multilingual-websites}

MSM kan hjälpa till att skapa flerspråkiga webbplatser på två sätt:

* När du skapar språkmallar.

   * Även om själva MSM **inte tillhandahåller innehållsöversättning** kan den integreras med översättningsanslutningar från tredje part som gör det. Observera att:

      * Med MSM kan du avbryta arv på sid- och/eller komponentnivå. Detta förhindrar att översatt innehåll skrivs över (från en live-kopia, med ännu inte översatt innehåll från en ritning) vid nästa utrullning.
      * Vissa översättningsanslutningar från tredje part automatiserar hanteringen av MSM-arv.

        Kontakta översättningstjänsten för mer information.

      * Ett annat sätt att skapa och översätta språkmallsidor är att använda språkkopior i kombination med AEM färdiga integrationsramverk för översättning.

* När du distribuerar innehåll från språkmallsidor.

   * Till exempel, från den franska huvudpersonen till landsspecifika webbplatser, som Frankrike/Frankrike, Kanada/franska, Schweiz/franska.

Mer information finns i [Översätta innehåll för flerspråkiga platser](/help/sites-administering/translation.md) och [Bästa praxis för översättning](/help/sites-administering/tc-bp.md).

## Strukturändringar och utrullningar {#structure-changes-and-rollouts}

Ändringar av innehållsstrukturen i ett utkast/källträd återspeglas på olika sätt i en live-kopia. Detta beror på ändringstypen:

* **Om du skapar** nya sidor i en plan skapas motsvarande sidor i live-kopior efter utrullning med standardkonfigurationen.

* **Om du tar bort** sidor i en plan kommer motsvarande sidor att tas bort från live-kopior efter utrullning med standardkonfiguration.

* **Om du flyttar** sidor i en plan kommer **inte** att resultera i att motsvarande sidor flyttas i live-kopior efter utrullning med standardkonfiguration:

   * Orsaken till detta är att en sidflyttning implicit inkluderar en sidborttagning. Detta kan potentiellt leda till oväntat beteende vid publicering, eftersom borttagning av sidor på författaren automatiskt inaktiverar motsvarande innehåll vid publicering. Detta kan också få en &quot;spara på&quot;-effekt på relaterade objekt som länkar, bokmärken och andra.
   * Innehållsarv i respektive live-kopia uppdateras för att återspegla den nya platsen för deras källor i planen.
   * Om du vill förverkliga en sidövergång från en plan till dynamiska kopior bör du tänka på följande bästa tillvägagångssätt:

>[!NOTE]
>
>Detta fungerar bara med utlösaren [Vid utrullning](/help/sites-administering/msm-sync.md#rollout-triggers).

* Skapa en anpassad utrullningskonfiguration:

   * Den nya konfigurationen måste innehålla åtgärden:

     `PageMoveAction`

     Lägg inte till andra åtgärder i den här konfigurationen.

* Placera den nya konfigurationen:

   * Om du vill flytta sidan helt och hållet och ta bort respektive sidor på den gamla platsen i den aktiva kopian:

      * Placera den nyskapade konfigurationen före standardkonfigurationen för utrullning.

        Standardkonfigurationen för utrullning tar bort sidorna på den gamla platsen.

   * Så här flyttar du ut sidan samtidigt som du behåller respektive sidor på den gamla platsen i live-kopiorna (duplicerar i stort sett innehållet):

      * Placera den nyskapade konfigurationen efter standardkonfigurationen för utrullning.

        Detta säkerställer att inget innehåll tas bort i den aktiva kopian eller inaktiveras från publiceringen.

## Anpassa utrullningar {#customizing-rollouts}

MSM-utrullningskonfigurationer är mycket anpassningsbara. Automatisering av utrullningar kan få stora konsekvenser. Som en god vana bör du planera *mycket* noggrant innan, till exempel:

* automatiserar rollouter, till exempel med [onModify-utlösare](#onmodify),
* anpassar [nodtyper/egenskaper](#node-types-properties),
* starta efterföljande arbetsflöden,
* och/eller aktivering av innehåll som en del av utrullningar.

### onModify {#onmodify}

När du använder [rollout-utlösaren](/help/sites-administering/msm-sync.md#rollout-triggers) `onModify` bör du tänka på följande:

* Automatisering av rollouter med `onModify` utlösare kan ha en negativ inverkan på redigeringsprestanda eftersom de utlöser rollouts efter *varje* sidändring.

* Resultatet av utrullningen kan skilja sig från det förväntade:

   * Du kan inte ange ordningen för ändringshändelserna som skapas.
   * Den händelsebaserade arkitekturen kan inte garantera sekvensen för de händelser som skickas till utrullningshanteraren.

* Om en sådan utrullningskonfiguration används kan konflikter uppstå om samma resurs uppdateras samtidigt.

Därför rekommenderar vi att du *endast* använder `onModify` utlösare om fördelarna med automatisk utrullning är större än eventuella prestandaproblem.

### Nodtyper/egenskaper {#node-types-properties}

Kom ihåg följande:

* Förutom att anpassa utrullningsåtgärder kan MSM även anpassa nodegenskaper som introduceras. Med konfigurationen [MSM OSGi kan du undanta nodtyperna ](/help/sites-administering/msm-sync.md#excluding-properties-and-node-types-from-synchronization) från att kopieras från källan till live-kopian.

## Ytterligare information {#further-information}

Här och på följande sidor finns de relaterade frågorna:

* [Skapa och synkronisera Live-kopior](/help/sites-administering/msm-livecopy.md)
* [Översiktskonsol för Live Copy](/help/sites-administering/msm-livecopy-overview.md)
* [Konfigurera Live Copy-synkronisering](/help/sites-administering/msm-sync.md)
* [MSM-utrullningskonflikter](/help/sites-administering/msm-rollout-conflicts.md)
