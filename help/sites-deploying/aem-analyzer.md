---
title: Utvärdera uppgraderingskomplexiteten med AEM Analyzer
description: Lär dig hur du använder AEM Analyzer för att bedöma hur komplex din uppgradering är.
topic-tags: upgrading
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 87c30912-c89a-42f1-b37b-ec439e7318c7
source-git-commit: 6b846e456466492f4be2c1e5a1f6b3913ae4dab4
workflow-type: tm+mt
source-wordcount: '2069'
ht-degree: 15%

---

# Utvärdera uppgraderingskomplexiteten med AEM Analyzer {#assessing-the-upgrade-complexity-with-the-aem-analyzer}

## Ökning {#overview}

AEM 6.5 LTS Analyzer ger en bedömning av din nuvarande AEM-implementering genom att ange områden som behöver uppdateras för att ge en sömlös uppgradering till 6,5 LTS.

Verktyget genererar en rapport som identifierar områden med potentiell omfaktorisering.

## AEM 6.5 LTS Analyzer Report {#aem-65lts-analyzer-report}

AEM 6.5 LTS Analyzer Report används för att få en god förståelse för den allmänna uppgraderingsberedskapen. Rapporten innehåller uppgifter inom olika kategorier som måste åtgärdas för att man ska kunna säkerställa en lyckad uppgradering till AEM 6.5 LTS.

AEM 6.5 LTS Analyzer-rapporten innehåller följande kategorier:

* Programfunktioner som måste refaktoriseras
* Databasobjekt som måste flyttas till en plats som stöds
* Konfigurationsproblem
* AEM 6.5-funktioner som har tagits bort av nya funktioner eller som för närvarande inte stöds i AEM 6.5 LTS
* Ta bort Java- och Guava API-användning

Mer information om kategorier och möjliga konsekvenser och lösningar som är kopplade till dessa kategorier finns via länkar i AEM 6.5 LTS Analyzer Report.

## Tillgänglighet {#analyzer-availability}

AEM Analyzer kan laddas ned som en zip-fil från [programdistributionsportalen](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html). Du kan installera paketet via [Package Manager](/help/sites-administering/package-manager.md) på din AEM-källinstans.

## Viktigt att tänka på när du använder AEM Analyzer {#important-considerations-for-using-aem-analyzer}

Följ avsnittet nedan för att få en förståelse för viktiga aspekter av att köra AEM Analyzer:

* Analysrapporten byggs med utdata från AEM Mönsteravkännare. Den version av Mönsteravkännare som används av Analyzer ingår i AEM Analyzer-installationspaketet
* AEM Analyzer kan bara köras av **admin**-användaren eller en användare i **administratörsgruppen**
* Analyzer stöds på AEM-instanser med version 6.5 och senare.

>[!NOTE]
>
>För att undvika att affärskritiska instanser påverkas rekommenderar vi att du kör AEM Analyzer på en scenmiljö som är så nära produktionsmiljön som möjligt när det gäller anpassningar, konfigurationer, innehåll och användarprogram. Alternativt kan det köras på en klon av produktionsredigeringsmiljön.

* Det kan ta lång tid att generera rapportinnehåll i AEM Analyzer, från flera minuter till några timmar. Hur lång tid som krävs beror i hög grad på storleken och typen av AEM-databasinnehåll, AEM-versionen och andra faktorer
* På grund av den stora tid som kan behövas för att generera rapportinnehållet genereras det av en bakgrundsprocess och lagras i ett cacheminne. Det bör gå relativt snabbt att visa och hämta rapporten eftersom den använder innehållscachen tills den upphör att gälla eller tills rapporten uttryckligen uppdateras. Under genereringen av rapportinnehåll kan du stänga webbläsarfliken och sedan gå tillbaka för att visa rapporten när dess innehåll är tillgängligt i cachen.

## Visa AEM Analyzer-rapporten {#viewing-the-aem-analyzer-report}

Följ stegen nedan för att visa AEM Analyzer-rapporten:

1. Välj Adobe Experience Manager och gå till **Verktyg - Åtgärder - 6.5 LTS Modernizer**

   ![Visa analysrapport 1](/help/sites-deploying/assets/view-analyzer-report-1.png)

1. Klicka på **AEM 6.5 LTS Analyzer** för att öppna den

   ![Visa analysrapport 2](/help/sites-deploying/assets/view-analyzer-report-2.png)

1. Klicka på **Generera rapport** för att köra AEM Analyzer

   ![Visa analysrapport 3](/help/sites-deploying/assets/view-analyzer-report-3.png)

1. Medan AEM Analyzer genererar rapporten kan du se hur verktyget utvecklas på skärmen. Den visar förloppet i procent slutfört. Här visas även antalet analyserade objekt och antalet resultat

   ![Visa analysrapport 4](/help/sites-deploying/assets/view-analyzer-report-4.png)

1. När 6.5 LTS Analyzer-rapporten har genererats visas en sammanfattning och antalet resultat i ett tabellformat, ordnade efter typ av sökning och prioritetsnivå. Om du vill ha mer information om en viss sökning kan du klicka på talet som motsvarar sökningstypen i tabellen

   ![Visa analysrapport 5](/help/sites-deploying/assets/view-analyzer-report-5.png)

1. Du kan hämta rapporten i ett kommaavgränsat värdeformat (CSV) genom att klicka på **Exportera till CSV**. Du kan tvinga Analyzer att rensa sin cache och återskapa rapporten genom att klicka på **Uppdatera rapport**. Om cachen förfaller måste du återskapa rapporten.

## Tolka AEM Analyzer-rapporten {#interpreting-the-aem-analyzer-report}

När verktyget 6.5 LTS Analyzer körs i AEM-instansen visas rapporten som resultat i verktygsfönstret.

Rapportens format är:

* **Report Overview**: Information om själva rapporten med följande information:

   * **Rapporttid**: När rapportinnehållet genererades och först gjordes tillgängligt
   * **Förfallotid**: När cachen för rapportinnehåll förfaller
   * **Tidsperiod för generering**: Den tid som rapporten genererades
   * **Antal sökningar**: Det totala antalet upptäckter som ingår i rapporten

* **Systemöversikt**: Information om det AEM-system som Analyzer kördes på
* **Finding Categories**: Flera avsnitt som vart och ett handlar om ett eller flera resultat i samma kategori. Varje avsnitt innehåller följande: kategorinamn, undertyper, antal resultat och viktighetsgrad, sammanfattning, länk till kategoridokumentation och information om enskilda resultat.

  ![Sammanfattning av analysrapport](/help/sites-deploying/assets/analyzer-report-summary.png)

  Viktighetsgrad tilldelas varje resultat och anger ungefärlig prioritet för åtgärder.

>[!NOTE]
>
>Mer information om de olika sökkategorierna finns i [Mönsterdetektorkategorierna](https://experienceleague.adobe.com/sv/docs/experience-manager-pattern-detection/table-of-contents/aso).

För att förstå prioritetsnivåerna följer du tabellen nedan:

| Viktighetsgrad | Beskrivning |
|---|---|
| INFO | Resultatet tillhandahålls i informationssyfte. |
| ADVISORY | Resultatet kan innebära ett uppgraderingsproblem. Ytterligare undersökningar rekommenderas. |
| CRITICAL | Detta resultat innebär sannolikt ett uppgraderingsproblem som måste åtgärdas för att förhindra funktions- eller prestandaproblem. |

## Tolka CSV-rapporten för AEM 6.5 LTS Analyzer {#interpreting-the-aem-65lts-analyzer-report}

När du klickar på alternativet **CSV** från din AEM-instans skapas CSV-formatet för Analyzer-rapporten från innehållscachen och returneras till webbläsaren. Beroende på inställningarna i webbläsaren hämtas den här rapporten automatiskt som en fil med standardnamnet `report.csv`.

Om cacheminnet har upphört att gälla genereras rapporten om innan CSV-filen skapas och hämtas.

Rapporten i CSV-format innehåller information som genereras med utdata från Pattern Detector och som sorteras och organiseras efter kategorityp, undertyp och viktighetsgrad. Formatet är lämpligt för visning och redigering i program som Microsoft Excel. Avsikten är att ge all sökinformation i ett upprepningsbart format, som kan vara användbar vid jämförelse av rapporter över tiden för att mäta förloppet.

Kolumnerna i rapporten i CSV-format är:

* **code**: kategorikoden
* **type**: kategorinamnet
* **subtype**: undertypen av kategori
* **importance**: viktighetsgrad
* **identifier**: den primära identifieraren för resultatet
* **message**: det meddelande som tillhandahålls för resultatet
* **context**: en JSON-sträng med resultatdata

Värdet `\N` i en kolumn för ett enskilt resultat anger att inga data har angetts.

## HTTP-gränssnitt {#http-interface}

6.5 LTS Analyzer tillhandahåller ett HTTP-gränssnitt som kan användas som ett alternativ till användargränssnittet i AEM. Gränssnittet stöder både `HEAD`- och `GET`-kommandon. Den kan användas för att generera Analyzer-rapporten och returnera den i ett av tre format: JSON, CSV och tabbseparerade värden (TSV).

Följande URL:er är tillgängliga för HTTP-åtkomst, där `<host>` är värdnamnet, vid behov tillsammans med porten, för den server där Analyzer är installerat:

* `http://<host>/apps/aem66-analyzer/analysis/report.json` för JSON-format
* `http://<host>/apps/aem66-analyzer/analysis/report.csv` för CSV-format
* `http://<host>/apps/aem66-analyzer/analysis/report.tsv` för TSV-format

### Köra en HTTP-begäran {#executing-an-http-request}

Ett enkelt sätt att köra en HTTP-begäran är att öppna en flik i samma webbläsare som du redan har loggat in på AEM som administratör. Du kan ange URL-adressen på webbläsarfliken och låta webbläsaren visa eller hämta resultat.

Du kan också använda ett kommandoradsverktyg som `curl` eller `wget` och alla HTTP-klientprogram. Om du inte använder en webbläsarflik med en autentiserad session måste du ange ett administratörsanvändarnamn och lösenord som en del av kommentaren.

Följande är ett exempel på hur detta kan göras:

```shell
curl -u admin:admin 'http://localhost:4502/apps/aem66-analyzer/analysis/report.csv' > report.csv.
```

## Justera cachelivstid {#adjusting-the-cache-lifetime}

Standardvärdet för AEM 6.5 LTS Analyzer-cache är 24 timmar. Med alternativet att uppdatera en rapport och återskapa cachen, både i AEM-instansen och i HTTP-gränssnittet, är det här standardvärdet troligtvis lämpligt för de flesta användningsområdena för AEM 6.5 LTS Analyzer. Om rapportgenereringstiden är särskilt lång för din AEM-instans kanske du vill justera cachelivstiden för att minimera omgenereringen av rapporten.

Livslängdsvärdet för cache lagras som egenskapen `maxCacheAge` på följande databasnod:

```
/apps/aem66-analyzer/content/modernizer/analyzer/jcr:content
```

Värdet för den här egenskapen är cachelivslängden i sekunder. Administratören kan justera cachelivslängden med CRX/DE Lite.

## Använda innehållstransformeraren {#using-content-transformer}

### Tillgänglighet {#content-transformer-availability}

Content Transformer medföljer AEM 6.5 LTS Analyzer som kan laddas ned som en zip-fil från Software Distribution Portal.

### Viktigt att tänka på när du använder innehållstransformeraren {#important-considerations-for-using-content-transformer}

Se avsnittet nedan om du vill veta mer om viktiga aspekter av att använda innehållstransformeraren (CT):

* Om du vill använda Content Transformer måste du först köra AEM Analyzer i din AEM-miljö
* Även om du kan köra Content Transformer i produktionsmiljön rekommenderar vi att du kör Content Transformer på en klon av produktionsmiljön. Viktigare är att du måste se till att AEM Analyzer och CT körs i samma miljö
* Du måste vara administratör i miljön där du vill köra innehållstransformeraren
* En borttagningsåtgärd som kan ändra källinnehållet skapar som standard ett säkerhetskopieringspaket med källsökvägarna under `/etc/packages/modernizer-content-transformation` före omvandlingen. Även om dialogrutan för borttagningsåtgärd har ett alternativ för att inaktivera/aktivera skapandet av säkerhetskopieringspaketet rekommenderar vi att du alltid har aktiverat skapandet av paketet
* Varje sida i innehållstransformeraren är konfigurerad att visa upp maximalt 50 träffar. Därför kan högst 50 resultat omvandlas i taget. Detta görs för att ge ett svar i rätt tid i användargränssnittet.

### Öppna innehållstransformeraren {#opening-the-content-transformer}

1. Logga in på AEM-källinstansen som administratör och gå till startsidan på *https://host:port/aem/start.htm*
1. Navigera till **Verktyg - Åtgärder - 6.5 LTS Modernizer**

   ![Öppnar innehållstransformeraren 1](/help/sites-deploying/assets/opening-content-transformer-1.png)

1. Klicka på **Content Transformer för 6.5 LTS Analyzer-rapportkortet**

   ![Öppnar innehållstransformeraren 2](/help/sites-deploying/assets/opening-content-transformer-2.png)

1. Om analysrapporten inte genereras kommer sidan **Omforma innehåll** att visa **Ingen rapport**. Samma **Inget rapportmeddelande** visas också om alla innehållsrelaterade upptäckter har tagits bort

   ![Öppnar innehållstransformeraren 3](/help/sites-deploying/assets/opening-content-transformer-3.png)

1. Nedan visas ett exempel på hur sidan Översikt över innehållstransformering ser ut om AEM Analyzer-rapporten har skapats och om innehållsrelaterade problem påträffas.

Den återstående förfallotiden för AEM Analyzer-rapporten visas på sidospåret. Vi rekommenderar att du kör Content Transformer tillsammans med den senaste AEM Analyzer-rapporten för att undvika att innehållsrelaterade brister saknas

![Öppnar innehållstransformeraren 4](/help/sites-deploying/assets/opening-content-transformer-4.png)

1. Du kan filtrera problemen baserat på mönsterkod, undertyp, prioritet och Source

   ![Öppnar innehållstransformeraren 5](/help/sites-deploying/assets/opening-content-transformer-5.png)

### Ta bort banor {#removing-paths}

1. Du kan välja alla eller specifika problem och välja **Ta bort** för att lösa dem

   ![Tar bort banor ](/help/sites-deploying/assets/removing-paths-1.png)

   >[!NOTE]
   >En borttagningsåtgärd skapar ett säkerhetskopiepaket med källsökvägarna under `/etc/packages/modernizer-content-transformation` som standard, före omvandlingen. Även om dialogrutan för borttagningsåtgärd har ett alternativ för att inaktivera/aktivera skapandet av säkerhetskopieringspaketet, rekommenderar vi att du alltid har aktiverat skapandet av paketet.

   ![Tar bort banor ](/help/sites-deploying/assets/removing-paths-2.png)

1. Ett exempel på ett säkerhetskopieringspaket som skapats för borttagningsåtgärden för sökvägarna visas nedan. Du kan klicka på **Installera** för att hämta källsökvägarna

   ![Tar bort banor 3](/help/sites-deploying/assets/removing-paths-3.png)

   >[!CAUTION]
   >
   >Ta inte bort `/etc/packages/modernizer-content-transformation` eftersom det är platsen där säkerhetskopieringspaketen finns. Du kan ta bort den här platsen för att minska databasstorleken endast om du är säker på att du inte behöver dessa paket längre.

1. Om du vill kan du paketera det valda innehållet för framtida bruk. Det gör du genom att markera de resultat du vill inkludera och sedan klicka på **Packa** uppifrån till vänster. Ange ett paketnamn, välj en paketsökväg och klicka på knappen **Packa** för att slutföra processen.

   ![Tar bort banor 3](/help/sites-deploying/assets/removing-paths-4.png)

### Kända fel {#known-issues}

* Ibland kan meddelandet visas i åtgärden Ta bort: *Vissa sökvägar har inte tagits bort. Kontrollera loggarna och försök igen.*&quot;. Om sökvägarna verkligen tagits bort kan du ignorera det här meddelandet
* På samma sätt kan paketåtgärden misslyckas med felet: *. Fel när du utför den önskade åtgärden. Kontrollera loggarna och försök igen.*&quot;. Detta beror troligtvis på att sessionen har upphört. I sådana fall bör du försöka åtgärda problemet genom att försöka igen.
