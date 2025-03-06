---
title: Konfigurera bevakade mappslutpunkter
description: Lär dig konfigurera bevakade mappslutpunkter. Om ett dokument placeras i bevakad mapp anropas en konfigurerad tjänståtgärd och filen ändras.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_endpoints
products: SG_EXPERIENCEMANAGER/6.4/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: ae001541-ae7f-42ce-8236-5fbb6ddb4c1f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '7192'
ht-degree: 0%

---

# Konfigurera bevakade mappslutpunkter {#configuring-watched-folder-endpoints}

En administratör kan konfigurera en nätverksmapp, en så kallad *bevakad mapp*, så att en konfigurerad tjänståtgärd anropas och filen ändras när en användare placerar en fil (till exempel en PDF-fil) i den bevakade mappen. När tjänsten har utfört den angivna åtgärden sparas den ändrade filen i en angiven utdatamapp.

## Konfigurera tjänsten Bevakade mappar {#configuring-the-watched-folder-service}

Konfigurera tjänsten Bevakade mappar innan du konfigurerar en bevakad mappslutpunkt. Konfigurationsparametrarna för tjänsten Bevakade mappar har två syften:

* Konfigurera attribut som är gemensamma för alla bevakade mappslutpunkter
* Ange standardvärden för alla bevakade mappslutpunkter

När du har konfigurerat tjänsten Bevakade mappar lägger du till en bevakad mappslutpunkt för måltjänsten. När du lägger till slutpunkten anger du värden, till exempel tjänstnamnet och åtgärdsnamnet som ska anropas när filer eller mappar placeras i indatamappen för den konfigurerade tjänsten Bevakade mappar. Mer information om hur du konfigurerar tjänsten Bevakade mappar finns i [Inställningar för tjänsten Bevakade mappar](/help/forms/using/admin-help/configure-service-settings.md#watched-folder-service-settings).

## Skapa en bevakad mapp {#creating-a-watched-folder}

Du kan skapa en bevakad mapp på följande två sätt:

* När du konfigurerar inställningarna för en bevakad mappslutpunkt skriver du den fullständiga sökvägen till den överordnade katalogen i rutan Sökväg och lägger till namnet på den bevakade mapp som ska skapas, enligt följande exempel:
  `  C:\MyPDFs\MyWatchedFolder`Eftersom mappen MyWatchedFolder inte redan finns försöker AEM att skapa den på den platsen.

* Skapa en mapp i filsystemet innan du konfigurerar en bevakad mappslutpunkt och skriv sedan den fullständiga sökvägen i rutan Sökväg.

I en klustermiljö måste mappen som används som en bevakad mapp vara tillgänglig, skrivbar och delad i filsystemet eller nätverket. I det här fallet måste varje programserverinstans i klustret ha åtkomst till samma delade mapp.

Om programservern körs som en tjänst i Windows måste den startas med lämplig åtkomst till den delade mappen på något av följande sätt:

* Konfigurera programservertjänstens inloggning som **parameter** så att den startar som en specifik användare med lämplig åtkomst till den delade bevakade mappen.
* Konfigurera programservertjänsten Start som lokalt system så att tjänsten kan interagera med skrivbordet. Det här alternativet kräver att den delade bevakade mappen är tillgänglig och skrivbar för alla.

## Sammanfoga bevakade mappar {#chaining-together-watched-folders}

Bevakade mappar kan sammanfogas så att ett resultatdokument för en bevakad mapp är indatadokumentet för nästa bevakade mapp. Varje bevakad mapp kan anropa en annan tjänst. Genom att konfigurera bevakade mappar på det här sättet kan flera tjänster anropas. En bevakad mapp kan till exempel konvertera PDF-filer till Adobe PostScript® och en andra bevakad mapp kan konvertera PostScript-filerna till PDF/A-format. Det gör du genom att ange att mappen *result* i den bevakade mappen som definieras av din första slutpunkt ska peka på mappen *input* i den bevakade mappen som definieras av din andra slutpunkt.

Utdata från den första konverteringen går till \path\result. Indata för den andra konverteringen är \path\result, och utdata från den andra konverteringen går till \path\result\result (eller den katalog som du definierar i rutan Resultatmapp för den andra konverteringen).

## Hur användare interagerar med bevakade mappar {#how-users-interact-with-watched-folders}

För en bevakad mappslutpunkt kan användare anropa genom att kopiera eller dra indatafiler eller -mappar från sina datorer till en bevakad mapp. Filerna kommer att bearbetas i den ordning som de kommer fram.

Om jobbet endast kräver en indatafil kan användaren kopiera den filen till roten i den bevakade mappen för bevakade mappslutpunkter.

Om jobbet innehåller mer än en indatafil måste användaren skapa en mapp utanför den bevakade mapphierarkin som innehåller alla nödvändiga filer. Den nya mappen bör innehålla indatafilerna (och eventuellt en DDX-fil om det behövs av processen). När jobbmappen har skapats kopierar användaren den till den bevakade mappens indatamapp.

>[!NOTE]
>
>Kontrollera att programservern har tagit bort åtkomsten till filerna i den bevakade mappen. Om AEM-formulär inte kan ta bort filerna från indatamappen efter att de har skannats in, kommer den tillhörande processen att anropas i oändlighet.

## Bevakade mapputdata {#watched-folder-output}

När indata är en mapp och utdata består av flera filer skapar AEM-formulär en utdatamapp med samma namn som indatamappen och kopierar utdatafilerna till den mappen. När utdata består av en dokumentöversikt som innehåller ett nyckelvärdepar, till exempel utdata från en utdataprocess, används nyckeln som utdatafilens namn.

Namn på utdatafiler som är ett resultat av en slutpunktsprocess får inte innehålla andra tecken än bokstäver, siffror och en punkt (.) före filtillägget. AEM-formulär konverterar andra tecken till sina hexadecimala värden.

Klientprogrammen hämtar resultatdokumenten från den bevakade mappens resultatmapp. Processfel loggas i den bevakade mappen för mappfel.

## Så här fungerar Bevakade mappar {#how-watched-folder-works}

Modulen Bevakade mappar innehåller följande tjänster:

* Tjänsten Bevakade mappar
* provider.file_scan_service
* provider.file_write_results_service

Förutom de tjänster som anges ovan är Bevakade mappar också beroende av andra tjänster, inklusive schemaläggningstjänsten för schemaläggning av jobb och jobbhanterartjänsten för asynkront anrop av måltjänster.

### Hur bevakad mapp bearbetar en anropsbegäran {#how-watched-folder-processes-an-invocation-request}

Tjänsten Bevakade mappar hanterar generering, uppdatering och borttagning av slutpunkterna. När administratören har skapat slutpunkterna schemaläggs de att utlösas av tjänsten Schemaläggaren baserat på det angivna upprepningsintervallet eller cron-uttrycket.

Bilden visar hur Bevakade mappar bearbetar en anropsbegäran.

![en_bevakad_mapp](assets/en_watchedfolder.png)

Så här anropar du en tjänst med bevakade mappar:

1. Ett klientprogram placerar filer eller mappar i den bevakade mappens indatamapp.
1. När jobbsökningsintervallet inträffar anropar tjänsten Scheduler provider.file_scan_service för att bearbeta filerna eller mapparna i indatamappen.
1. provider.file_scan_service utför följande uppgifter:


   * Söker igenom indatamappen efter filer eller mappar som matchar inkluderingsfilmönstret och utesluter filer eller mappar för det angivna uteslutningsfilmönstret. De äldsta filerna eller mapparna hämtas först. Filer och mappar som är äldre än väntetiden hämtas också. I en sökning baseras antalet filer eller mappar som bearbetas på gruppstorleken. Mer information om filmönster finns i [Om filmönster](configuring-watched-folder-endpoints.md#about-file-patterns). Mer information om hur du anger gruppstorlek finns i [Inställningar för tjänsten Bevakade mappar](/help/forms/using/admin-help/configure-service-settings.md#watched-folder-service-settings).
   * Hämtar filer eller mappar för bearbetning. Om filerna eller mapparna inte hämtas helt hämtas de vid nästa sökning. För att vara säker på att mapparna laddas ned fullständigt bör administratörer skapa en mapp med ett namn genom att använda filmönstret exclude. När mappen har alla filer måste namnet ändras till det mönster som anges i inkluderingsfilmönstret. Detta steg säkerställer att mappen har alla filer som behövs för att anropa tjänsten. Mer information om hur du ser till att mapparna hämtas fullständigt finns i [Tips och tricks för bevakade mappar](configuring-watched-folder-endpoints.md#tips-and-tricks-for-watched-folders).
   * Flyttar filerna eller mapparna till scenmappen efter att de har markerats för bearbetning.
   * Konverterar filerna eller mapparna i scenmappen till lämplig indata baserat på mappningar av indataparametrar för slutpunkter. Exempel på mappningar av indataparametrar finns i [Tips och tricks för bevakade mappar](configuring-watched-folder-endpoints.md#tips-and-tricks-for-watched-folders).


1. Den måltjänst som konfigurerats för slutpunkten anropas synkront eller asynkront. Måltjänsten anropas med det användarnamn och lösenord som har konfigurerats för slutpunkten.

   * Synkrona anrop anropar måltjänsten direkt och hanterar omedelbart svaret.
   * För asynkrona anrop anropas måltjänsten via tjänsten Jobbhanteraren, som placerar begäran i en kö. Jobbhanteraren anropar i sin tur provider.file_write_results_service för att hantera resultaten.

1. provider.file_write_results_service hanterar svaret eller felet för måltjänstens anrop. När det är klart sparas utdata i resultatmappen baserat på slutpunktskonfigurationen. provider.file_write_results_service bevarar också källan om slutpunkten är konfigurerad för att bevara resultaten när de har slutförts.

   När anropet av måltjänsten resulterar i ett fel loggar provider.file_write_results_service orsaken till felet i en error.log-fil och placerar filen i felmappen. Felmappen skapas baserat på de konfigurationsparametrar som angetts för slutpunkten. När administratören anger alternativet Bevara vid fel för slutpunktskonfigurationen kopierar även provider.file_write_results_service källfilerna till felmappen. Mer information om hur du återställer filer från felmappen finns i [Felpunkter och återställning](configuring-watched-folder-endpoints.md#failure-points-and-recovery).


## Slutpunktsinställningar för bevakad mapp {#watched-folder-endpoint-settings}

Använd följande inställningar för att konfigurera en bevakad mappslutpunkt.

**Namn:** (obligatoriskt) Identifierar slutpunkten. Ta inte med ett &lt;-tecken eftersom det kortar av namnet som visas i Workspace. Om du anger en URL som namn på slutpunkten kontrollerar du att den överensstämmer med de syntaxregler som anges i RFC1738.

**Beskrivning:** En beskrivning av slutpunkten. Ta inte med ett &lt;-tecken eftersom det kortar av beskrivningen som visas i Workspace.

**Sökväg:** (obligatoriskt) Anger platsen för bevakad mapp. I en klustrad miljö måste den här inställningen peka på en delad nätverksmapp som är tillgänglig från alla datorer i klustret.

**Asynkron:** Identifierar anropstypen som asynkron eller synkron. Standardvärdet är asynkront. Asynkron rekommenderas för långvariga processer, medan synkron rekommenderas för tillfälliga eller kortvariga processer.

**Cron-uttryck:** Ange ett cron-uttryck om den bevakade mappen måste schemaläggas med ett cron-uttryck. När den här inställningen är konfigurerad ignoreras upprepningsintervall.

**Intervall för upprepning:** Intervallet i sekunder för skanning av den bevakade mappen för indata. Om inte inställningen Gräns är aktiverad ska intervallet för upprepning vara längre än tiden för att bearbeta ett genomsnittligt jobb. Annars kan systemet bli överbelastat. Standardvärdet är 5. Mer information finns i beskrivningen för Batchstorlek.

**Antal upprepningar:** Antal gånger den bevakade mappen genomsöker mappen eller katalogen. Värdet -1 anger oändlig skanning. Standardvärdet är -1.

**Begränsning:** När det här alternativet är markerat begränsas antalet bevakade mappjobb som bearbetas i AEM-formulär vid en given tidpunkt. Det maximala antalet jobb bestäms av värdet för Batchstorlek. (Se Om begränsning.)

**Användarnamn:** (obligatoriskt) Det användarnamn som används när en måltjänst anropas från den bevakade mappen. Standardvärdet är SuperAdmin.

**Domännamn:** (obligatoriskt) Användarens domän. Standardvärdet är DefaultDom.

**Batchstorlek:** Antalet filer eller mappar som ska hämtas per skanning. Används för att förhindra överbelastning i systemet. Om du skannar för många filer samtidigt kan det orsaka en krasch. Standardvärdet är 2.

Inställningarna Intervall för upprepning och Gruppstorlek avgör hur många filer i Bevakade mappar som tas upp vid varje sökning. Bevakad mapp använder en Quartz-trådpool för att skanna indatamappen. Trådpoolen delas med andra tjänster. Om skanningsintervallet är litet genomsöks indatamappen ofta av trådarna. Om filer ofta placeras i den bevakade mappen bör du hålla sökintervallet litet. Om filerna tas bort sällan bör du använda ett större inläsningsintervall så att de andra tjänsterna kan använda trådarna.

Om det finns en stor mängd filer som tas bort gör du gruppstorleken stor. Om till exempel tjänsten som anropas av den bevakade mappens slutpunkt kan bearbeta 700 filer per minut, och användarna släpper filer i indatamappen med samma hastighet, och du sedan anger värdet 350 för Gruppstorlek och intervallet för upprepning till 30 sekunder, kommer prestandan för bevakad mapp att bli bättre utan att kostnaden för att skanna den bevakade mappen för ofta påverkas.

När filer släpps i den bevakade mappen visas filerna i indata, vilket kan försämra prestanda om skanningen sker varje sekund. Om du ökar skanningsintervallet kan prestandan förbättras. Om volymen för de filer som tas bort är liten justerar du batchstorleken och upprepningsintervallet därefter. Om till exempel 10 filer tas bort varje sekund, kan du försöka med att ange intervallet för upprepning till 1 sekund och gruppstorleken till 10.

**Väntetid:** Den tid i millisekunder som du vill vänta innan du skannar en mapp eller fil efter att den har skapats. Om väntetiden till exempel är 3 600 000 millisekunder (en timme) och filen skapades för en minut sedan, kommer filen att hämtas efter 59 eller fler minuter. Standardvärdet är 0.

Den här inställningen är användbar för att säkerställa att en fil eller mapp kopieras helt till indatamappen. Om du t.ex. har en stor fil att bearbeta och det tar tio minuter att hämta filen anger du väntetiden till 10&amp;ast;60 &amp;ast;1000 millisekunder. Detta förhindrar att den bevakade mappen skannar filen om den inte är tio minuter gammal.

**Uteslut filmönster:** Ett semikolon **;** avgränsad lista över mönster som används i en bevakad mapp för att avgöra vilka filer och mappar som ska sökas igenom och plockas upp. Filer och mappar med det här mönstret skannas inte för bearbetning.

Den här inställningen är användbar när indata är en mapp med flera filer. Innehållet i mappen kan kopieras till en mapp med ett namn som hämtas av den bevakade mappen. Detta förhindrar att den bevakade mappen hämtar en mapp för bearbetning innan mappen kopieras till indatamappen.

Du kan använda filmönster för att exkludera:

* Filer med specifika filnamnstillägg, till exempel &amp;ast;.dat, &amp;ast;.xml, &amp;ast;.pdf.
* Filer med specifika namn, till exempel data.&amp;ast; skulle exkludera filer och mappar med namnen *data1*, *data2* och så vidare.
* Filer med sammansatta uttryck i namnet och tillägget, som i följande exempel:

   * Data[0-9][0-9][0-9].[d][aA]&#39;port&#39;
   * &amp;ast;..[d][Aa]&#39;port&#39;
   * &amp;ast;..[xx][mm][Ll]

Mer information om filmönster finns i [Om filmönster](configuring-watched-folder-endpoints.md#about-file-patterns).

**Inkludera filmönster:** (obligatoriskt) Ett semikolon **;** avgränsad lista över mönster som den bevakade mappen använder för att avgöra vilka mappar och filer som ska sökas igenom och plockas upp. Om till exempel Inkludera filmönster är indata&amp;ast; hämtas alla filer och mappar som matchar indata&amp;ast;. Detta inkluderar filer och mappar med namnen input1, input2 och så vidare.

Standardvärdet är &amp;ast; och anger alla filer och mappar.

Du kan använda filmönster för att inkludera:

* Filer med specifika filnamnstillägg, till exempel &amp;ast;.dat, &amp;ast;.xml, &amp;ast;.pdf.
* Filer med specifika namn, till exempel data.&amp;ast; skulle innehålla filer och mappar med namnen *data1*, *data2* och så vidare.
* Filer med sammansatta uttryck i namnet och tillägget, som i följande exempel:

   * Data[0-9][0-9][0-9].[d][aA]&#39;port&#39;
   * &amp;ast;..[d][Aa]&#39;port&#39;
   * &amp;ast;..[xx][mm][Ll]

Mer information om filmönster finns i [Om filmönster](configuring-watched-folder-endpoints.md#about-file-patterns).


**Resultatmapp:** Mappen där de sparade resultaten lagras. Om resultaten inte visas i den här mappen kontrollerar du felmappen. Skrivskyddade filer bearbetas inte och sparas i felmappen. Värdet kan vara en absolut eller relativ sökväg med följande filmönster:

* %F = filnamnsprefix
* %E = filnamnstillägg
* %Y = år (full)
* %y = år (de två sista siffrorna)
* %M = månad
* %D = dag i månaden
* %d = dag på året
* %H = timme (24-timmars klocka)
* %h = timme (12-timmars klocka)
* %m = minut
* %s = sekund
* %l = millisekund
* %R = slumptal (mellan 0 och 9)
* %P = process- eller jobb-ID

Om det till exempel är 2009-08-17 och du anger `C:/Test/WF0/failure/%Y/%M/%D/%H/` blir resultatmappen `C:/Test/WF0/failure/2009/07/17/20`.

Om sökvägen inte är absolut men relativ skapas mappen i den bevakade mappen. Standardvärdet är result/%Y/%M/%D/, som är resultatmappen i den bevakade mappen. Mer information om filmönster finns i [Om filmönster](configuring-watched-folder-endpoints.md#about-file-patterns).

>[!NOTE]
>
>Ju mindre resultatmapparna är, desto bättre prestanda blir Bevakade mappar. Om den beräknade inläsningen för den bevakade mappen till exempel är 1 000 filer varje timme kan du prova ett mönster som `result/%Y%M%D%H` så att en ny undermapp skapas varje timme. Om inläsningen är mindre (till exempel 1 000 filer per dag) kan du använda ett mönster som `result/%Y%M%D`.

**Bevara mapp:** Platsen där filerna lagras efter att sökningen och hämtningen har slutförts. Sökvägen kan vara en absolut, relativ eller null-katalogsökväg. Du kan använda filmönster enligt beskrivningen för resultatmappen. Standardvärdet är preserve/%Y/%M/%D/.

**Felmapp:** Mappen där felfiler sparas. Den här platsen är alltid relativ till den bevakade mappen. Du kan använda filmönster enligt beskrivningen för resultatmappen.

Skrivskyddade filer bearbetas inte och sparas i felmappen.

Standardvärdet är fel/%Y/%M/%D/.

**Bevara vid fel:** Bevara indatafiler om det inte går att köra åtgärden på en tjänst. Standardvärdet är true.

**Skriv över duplicerade filnamn:** Om värdet är True skrivs filerna i resultatmappen och bevara mappen över. Om värdet är Falskt används filer och mappar med ett numeriskt indexsuffix för namnet. Standardvärdet är Falskt.

**Töm varaktighet:** (obligatoriskt) Filer och mappar i resultatmappen tas bort när de är äldre än det här värdet. Detta värde mäts i dagar. Den här inställningen är användbar för att säkerställa att resultatmappen inte blir full.

Värdet -1 dagar anger att resultatmappen aldrig ska tas bort. Standardvärdet är -1.

**Åtgärdsnamn:** (obligatoriskt) En lista över åtgärder som kan tilldelas till den bevakade mappens slutpunkt.

**Mappningar av indataparameter:** Används för att konfigurera de indata som krävs för att bearbeta tjänsten och åtgärden. Vilka inställningar som är tillgängliga beror på vilken tjänst som använder den bevakade mappens slutpunkt. Här är två typer av indata:

**Litteral:** Den bevakade mappen använder det värde som anges i fältet när det visas. Alla grundläggande Java-typer stöds. Om ett API till exempel använder indata som String, long, int och Boolean, konverteras strängen till rätt typ och tjänsten anropas.

**Variabel:** Det angivna värdet är ett filmönster som den bevakade mappen använder för att välja indata. Om det till exempel finns en krypteringslösenordstjänst där indatadokumentet måste vara en PDF-fil, kan användaren använda &amp;ast;.pdf som filmönster. Den bevakade mappen hämtar alla filer i den bevakade mappen som matchar mönstret och anropar tjänsten för varje fil. När en variabel används konverteras alla indatafiler till dokument. Endast API:er som använder Document som indatatyp stöds.

**Mappningar av utdataparameter:** Används för att konfigurera utdata för tjänsten och åtgärden. Vilka inställningar som är tillgängliga beror på vilken tjänst som använder den bevakade mappens slutpunkt.

Bevakade mapputdata kan vara ett enstaka dokument, en lista med dokument eller en karta med dokument. Dessa utdatadokument sparas sedan i resultatmappen med det mönster som anges i Mappning av utdataparameter.

>[!NOTE]
>
>Om du anger namn som ger unika utdatafilnamn förbättras prestandan. Tänk dig till exempel det fall där tjänsten returnerar ett utdatadokument och Output Parameter Mapping mappar det till `%F.%E` (indatafilens filnamn och filtillägg). Om användare i det här fallet släpper filer med samma namn varje minut och resultatmappen är konfigurerad till `result/%Y/%M/%D` och inställningen Skriv över duplicerat filnamn är inaktiverad, kommer Bevakad mapp att försöka matcha de duplicerade filnamnen. Prestandan kan påverkas av att duplicerade filnamn tolkas. I det här fallet kan prestandan förbättras om du ändrar utdataparametermappningen till `%F_%h_%m_%s_%l` för att lägga till timmar, minuter, sekunder och millisekunder i namnet, eller om du ser till att borttagna filer har unika namn.

## Om filmönster {#about-file-patterns}

Administratörer kan ange vilken typ av fil som kan anropa en tjänst. Du kan skapa flera filmönster för varje bevakad mapp. Ett filmönster kan vara någon av följande filegenskaper:

* Filer med specifika filnamnstillägg. Till exempel &amp;ast;.dat, &amp;ast;.xml, &amp;ast;.pdf
* Filer med specifika namn. Till exempel data.&amp;ast;
* Filer med sammansatta uttryck i namnet och tillägget, som i följande exempel:

   * Data[0-9][0-9][0-9].[d][aA]&#39;port&#39;
   * &amp;ast;..[d][Aa]&#39;port&#39;
   * &amp;ast;..[xx][mm][Ll]

Administratören kan definiera filmönstret för utdatamappen där resultaten ska lagras. För utdatamappar (resultat, bevarande och fel) kan administratören ange något av följande filmönster:

* %Y = år (full)
* %y = år (de två sista siffrorna)
* %M = månad,
* %D = dag i månaden,
* %d = dag på året,
* %h = timme,
* %m = minut,
* %s = sekund,
* %R = slumptal mellan 0 och 9
* %J = Jobbnamn

Sökvägen till resultatmappen kan till exempel vara `C:\Adobe\Adobe_Experience_Manager_forms\BarcodedForms\%y\%m\%d`.

Mappningar av utdataparametrar kan även ange ytterligare mönster, som:

* %F = Source filnamn
* %E = Source Filename Extension

Om mappningsmönstret för utdataparametrar avslutas med &quot;File.separator&quot; (som är sökvägsavgränsaren) skapas en mapp och innehållet kopieras till den mappen. Om mönstret inte avslutas med &quot;File.separator&quot; skapas innehållet (resultatfilen eller mappen) med det namnet. Mer information om mappningar av utdataparametrar finns i [Tips och tricks för bevakade mappar](configuring-watched-folder-endpoints.md#tips-and-tricks-for-watched-folders).

## Om begränsning {#about-throttling}

När du har aktiverat begränsning för en bevakad mappslutpunkt begränsas antalet bevakade mappjobb som kan bearbetas vid en given tidpunkt. Det maximala antalet jobb bestäms av värdet för Gruppstorlek, som också kan konfigureras i slutpunkten för bevakad mapp. Inkommande dokument i indatakatalogen för den bevakade mappen avsöks inte när begränsningsgränsen har uppnåtts. Dokumenten kommer också att finnas kvar i indatakatalogen tills andra bevakade mappjobb har slutförts och ytterligare ett avsökningsförsök görs. Om det finns synkron bearbetning räknas alla jobb som bearbetas i en enda omröstning mot begränsningsgränsen, även om jobben bearbetas i följd i en enda tråd.

>[!NOTE]
>
>Begränsningen skalas inte med ett kluster. När du har aktiverat begränsning kommer klustret som helhet inte att bearbeta mer än det antal jobb som har angetts i gruppstorleken vid en given tidpunkt. Den här gränsen är klusterbred och gäller inte för alla noder i klustret. Om du till exempel har en gruppstorlek på 2 kan begränsningen nås med en enda nod som bearbetar två jobb, och inga andra noder kommer att anropa indatakatalogen tills ett av jobben är klart.

### Så här fungerar strypning {#how-throttling-works}

Bevakad mapp skannar indatamappen vid varje upprepningsintervall, hämtar det antal filer som anges i gruppstorleken och anropar måltjänsten för var och en av dessa filer. Om till exempel gruppstorleken är fyra hämtar Bevakad mapp fyra filer vid varje skanning, skapar fyra anropsbegäranden och anropar måltjänsten. Om Bevakade mappar anropas startas fyra jobb igen innan dessa begäranden har slutförts, oavsett om de föregående fyra jobben har slutförts eller inte.

Begränsning förhindrar att bevakad mapp anropar nya jobb när tidigare jobb inte har slutförts. Bevakad mapp identifierar pågående jobb och bearbetar nya jobb baserat på batchstorleken minus pågående jobb. I det andra anropet anropas bara tre jobb till om antalet slutförda jobb är tre och ett jobb fortfarande pågår.

* Bevakad mapp är beroende av antalet filer som finns i scenmappen för att ta reda på hur många jobb som pågår. Om filerna inte bearbetas i scenmappen kommer Bevakad mapp inte att anropa fler jobb. Om batchstorleken till exempel är fyra och tre jobb stoppas, kommer Bevakad mapp endast att anropa ett jobb i efterföljande anrop. Det finns flera scenarier som kan göra att filer förblir obearbetade i scenmappen. När jobben är klara kan administratören avsluta processen på administrationssidan för formulärarbetsflödet så att Bevakad mapp flyttar filerna från scenmappen.
* Om Forms Server kraschar innan den bevakade mappen kan anropa jobben, kan administratören flytta filerna från scenmappen. Mer information finns i [Felpunkter och återställning](configuring-watched-folder-endpoints.md#failure-points-and-recovery).
* Om Forms Server körs men Bevakade mappar inte körs när tjänsten Job Manager anropas tillbaka, vilket inträffar när tjänster inte startar i den ordnade sekvensen, kan administratören flytta filerna från scenmappen. Mer information finns i [Felpunkter och återställning](configuring-watched-folder-endpoints.md#failure-points-and-recovery).


## Prestanda och skalbarhet {#performance-and-scalability}

Bevakad mapp kan hantera totalt 100 mappar på en enda nod. Hur övervakad mapp fungerar beror på Forms-serverns prestanda. För asynkrona anrop är prestanda mer beroende av systeminläsningen och jobben i jobbhanterarkön.

Du kan förbättra prestanda för bevakade mappar genom att lägga till noder i klustret. Bevakade mappjobb distribueras mellan klusternoderna med hjälp av Quartz-schemaläggaren och, om det finns asynkrona begäranden, med tjänsten Job Manager. Alla jobb sparas i databasen.

Bevakad mapp är beroende av schemaläggningstjänsten för schemaläggning, avschemaläggning och omplanering av jobben. Andra tjänster, som händelsehanteringstjänsten, tjänsten User Manager och e-postprovidertjänsten, är tillgängliga som delar trådpoolen för tjänsten Scheduler. Detta kan påverka prestandan för bevakade mappar. Inställningen av trådpoolen för schemaläggningstjänsten behövs när alla tjänster börjar använda den.

## Bevakade mappar i ett kluster {#watched-folders-in-a-cluster}

I ett kluster beror Bevakade mappar på Quartz-schemaläggaren och jobbhanterartjänsten för belastningsutjämning och failover. Mer information om klusterbeteendet i Quartz finns i [Quartz-dokumentation](https://www.quartz-scheduler.org/documentation).

Bevakad mapp utför följande tre huvuduppgifter vid varje omröstning:

* Skanna mappen
* Anropa måltjänsten
* Hantera resultatet

Beteendet för belastningsutjämning och failover ändras beroende på om den bevakade mappen är konfigurerad för synkront eller asynkront anrop.

### Synkron bevakad mapp i ett kluster {#synchronous-watched-folder-in-a-cluster}

För synkrona anrop avgör Quartz-belastningsutjämnaren vilken nod som får avsökningshändelsen. Noden som hämtar avsökningshändelsen utför alla åtgärder: skanna mappen, anropa måltjänsten och hantera resultaten.

![en_synchwatchedfolderCluster](assets/en_synchwatchedfoldercluster.png)

För synkrona anrop skickas nya avsökningshändelser till andra noder när en nod misslyckas. Anrop som startades på den misslyckade noden går förlorade. Mer information om hur du återställer filer som är associerade med det misslyckade jobbet finns i [Felpunkter och återställning](configuring-watched-folder-endpoints.md#failure-points-and-recovery).


### Asynkron bevakad mapp i ett kluster {#asynchronous-watched-folder-in-a-cluster}

För asynkrona anrop avgör belastningsutjämnaren i Quartz vilken nod som får avsökningshändelsen. Noden som hämtar avsökningshändelsen genomsöker indatamappen och anropar måltjänsten genom att placera begäran i jobbhanterarens tjänstkö. Belastningsutjämnaren för tjänsten Job Manager ansvarar i sin tur för att avgöra vilken nod som ska bearbeta anropsbegäran. Det är möjligt att även om nod A skapade anropsbegäran så kommer nod B att bearbeta begäran. Eller så kan noden som startade anropsbegäran också avsluta bearbetningen av begäran.

![en_asynchwatchedfolderCluster](assets/en_asynchwatchedfoldercluster.png)

För asynkrona anrop skickas nya avsökningshändelser till andra noder när en nod misslyckas. Anropsbegäranden som har skapats på den misslyckade noden finns i jobbhanterarens tjänstkö och skickas till andra noder för bearbetning. Filer som inte har någon startbegäran kommer att finnas kvar i scenmappen. Mer information om hur du återställer filer som är associerade med det misslyckade jobbet finns i [Felpunkter och återställning](configuring-watched-folder-endpoints.md#failure-points-and-recovery).


## Felpunkter och återställning {#failure-points-and-recovery}

Vid varje omröstning låses indatamappen av Bevakad mapp, filerna som matchar inkluderingsfilmönstret flyttas till scenmappen och indatamappen låses upp. Låsning krävs så att två trådar inte kan hämta samma uppsättning filer och bearbeta dem två gånger. Risken för detta ökar med ett litet upprepningsintervall och en stor batchstorlek. När filer har flyttats till scenmappen låses indatamappen upp så att andra trådar kan skanna in mappen. Det här steget ger hög genomströmning eftersom andra trådar kan skanna medan en tråd bearbetar filerna.

När filer har flyttats till scenmappen skapas en anropsbegäran för varje fil och måltjänsten anropas. Det kan finnas tillfällen då bevakad mapp inte kan återskapa filerna i scenmappen:

* Om servern kraschar innan Bevakad mapp kan skapa en anropsbegäran finns filerna i scenmappen kvar i scenmappen och återställs inte.
* Om Bevakad mapp har skapat anropsbegäran för var och en av filerna i scenmappen och servern kraschar, finns det två beteenden baserade på anropstypen:

**Synkron:** Om bevakad mapp är konfigurerad att anropa tjänsten synkront förblir alla filer i scenmappen obearbetade i scenmappen.

**Asynkron:** I det här fallet är den bevakade mappen beroende av tjänsten Jobbhanteraren. Om tjänsten Jobbhanteraren anropar den bevakade mappen, flyttas filerna i scenmappen till mappen för bevarande eller fel baserat på resultatet av anropet. Om tjänsten Jobbhanteraren inte anropar den bevakade mappen kommer filerna inte att bearbetas i scenmappen. Detta inträffar när den bevakade mappen inte körs när jobbhanteraren anropar tillbaka.

### Återställa obearbetade källfiler i scenmappen {#recovering-unprocessed-source-files-in-the-stage-folder}

Om den bevakade mappen inte kan bearbeta källfilerna i scenmappen kan du återställa de obearbetade filerna.

1. Starta om programservern eller noden.
1. (Valfritt) Stoppa bevakad mapp från att bearbeta nya indatafiler. Om du hoppar över det här steget blir det mycket svårare att avgöra vilka filer som inte bearbetas i scenmappen. Om du vill förhindra att Bevakade mappar bearbetar nya indatafiler gör du något av följande:

   * I Program och tjänster ändrar du parametern Inkludera filmönster för den bevakade mappens slutpunkt till något som inte matchar någon av de nya indatafilerna (ange till exempel `NOMATCH`).
   * Skjut upp processen som skapar nya indatafiler.

   Vänta tills AEM återställer och bearbetar alla filer. De flesta filerna bör återställas och alla nya indatafiler bearbetas korrekt. Hur lång tid du väntar på att Bevakade mappar ska återställas och bearbetas beror på hur lång åtgärden ska vara och hur många filer som ska återställas.

1. Avgör vilka filer som inte kan bearbetas. Om du väntade en viss tid och slutförde det föregående steget och det fortfarande finns obearbetade filer kvar i scenmappen går du till nästa steg.

   >[!NOTE]
   >
   >Du kan titta på datum- och tidsstämpeln för filerna i scenkatalogen. Beroende på antalet filer och den normala bearbetningstiden kan du avgöra vilka filer som är tillräckligt gamla för att anses ha fastnat.

1. Kopiera de obearbetade filerna från scenkatalogen till indatakatalogen.
1. Om du förhindrade att Bevakad mapp bearbetar nya indatafiler i steg 2, ändrar du Inkludera filmönster till dess tidigare värde eller återaktiverar den process som du inaktiverade.

## Säkerhetsaspekter för bevakade mappar {#security-considerations-for-watched-folders}

Varje bevakad mapp konfigureras med ett användarnamn och lösenord. Dessa autentiseringsuppgifter används när tjänsterna anropas. Bevakad mapp är beroende av att den delade mappen skyddas med det underliggande säkerhetsfilsystemet, så att bara ägaren till den bevakade mappen kan komma åt den delade mappen.

## Tips och tricks för bevakade mappar {#tips-and-tricks-for-watched-folders}

Här följer några tips och råd när du konfigurerar slutpunkten för bevakad mapp:

* Om du har en bevakad mapp i Windows som bearbetar bildfiler anger du värden för alternativet Inkludera filmönster eller Uteslut filmönster för att förhindra att den automatiskt genererade Windows-filen Thumbs.db avsöks av den bevakade mappen.
* Om ett cron-uttryck anges ignoreras det upprepade intervallet. Användningen av cron-uttryck baseras på Quartz-systemet för jobbplanering med öppen källkod, version 1.4.0.
* Batchstorleken är antalet filer eller mappar som hämtas vid varje sökning i den bevakade mappen. Om gruppstorleken är inställd på två och tio filer eller mappar släpps i den bevakade mappens indatamapp, hämtas endast två vid varje sökning. I nästa sökning, som sker efter den tidpunkt som anges i upprepningsintervallet, hämtas de två följande filerna.
* För filmönster kan administratörer ange reguljära uttryck med stöd för jokerteckenmönster för att ange filmönster. Bevakad mapp ändrar det reguljära uttrycket så att det stöder mönster för jokertecken som &amp;ast;.&amp;ast; or&amp;ast;.pdf. Dessa mönster med jokertecken stöds inte av reguljära uttryck.
* Bevakad mapp söker igenom indatamappen efter indatamappen och vet inte om källfilen eller -mappen kopieras fullständigt till indatamappen innan den börjar bearbeta filen eller mappen. Så här ser du till att källfilen eller källmappen kopieras till indatamappen i den bevakade mappen innan filen eller mappen hämtas:

   * Använd väntetid, vilket är den tid i millisekunder som den bevakade mappen väntar från den senaste ändringstiden. Använd den här funktionen om du har stora filer att bearbeta. Om det t.ex. tar 10 minuter att hämta en fil anger du väntetiden som 10&amp;ast;60 &amp;ast;1 000 millisekunder. Detta förhindrar att övervakad mapp kan hämta filen om den inte är lika gammal som tio minuter.
   * Använd exkludera filmönster och inkludera filmönster. Om det uteslutna filmönstret till exempel är `ex*` och inkluderingsfilmönstret är `in*`, kommer Bevakad mapp att hämta de filer som börjar med &quot;in&quot; och inte hämta de filer som börjar med &quot;ex&quot;. Om du vill kopiera stora filer eller mappar måste du först byta namn på filen eller mappen så att namnet börjar med &quot;ex&quot;. När filen eller mappen med namnet &quot;ex&quot; har kopierats helt till den bevakade mappen byter du namn på den till &quot;in&amp;ast;&quot;.

* Använd Töm varaktighet om du vill att resultatmappen ska vara ren. Bevakad mapp rensar alla filer som är äldre än den varaktighet som anges i rensningstiden. Längden är i dagar.
* När du lägger till en bevakad mappslutpunkt fylls indataparametermappningen i när du har valt åtgärdsnamnet. För varje indata i åtgärden genereras ett mappningsfält för indataparametrar. Här är exempel på indataparametermappningar:

   * För `com.adobe.idp.Document`-indata: Om tjänståtgärden har indata av typen `Document` kan administratören ange mappningstypen som `Variable`. Bevakad mapp hämtar indata från den bevakade mappens indatamapp baserat på det filmönster som angetts för indataparametern. Om administratören anger `*.pdf` som parameter hämtas varje fil som har filnamnstillägget .pdf, konverteras till `com.adobe.idp.Document` och tjänsten anropas.
   * För `java.util.Map`-indata: Om tjänståtgärden har indata av typen `Map` kan administratören ange mappningstypen som `Variable` och ange ett mappningsvärde med ett mönster som `*.pdf`. En tjänst behöver till exempel en karta över två `com.adobe.idp.Document`-objekt som representerar två filer i indatamappen, till exempel 1.pdf och 2.pdf. Bevakad mapp skapar en karta med nyckeln som filnamn och värde som `com.adobe.idp.Document`.
   * För `java.util.List`-indata: Om tjänståtgärden har indata av typen List kan administratören ange mappningstypen som `Variable` och ange ett mappningsvärde med ett mönster som `*.pdf`. När PDF-filer släpps i indatamappen skapas en lista med de `com.adobe.idp.Document`-objekt som representerar de här filerna och anropar måltjänsten.
   * För `java.lang.String`: Administratören har två alternativ. Först kan administratören ange mappningstypen som `Literal` och ange ett mappningsvärde som en sträng, t.ex. `hello.` Bevakade mappar anropar tjänsten med strängen `hello`. Därefter kan administratören ange mappningstypen som `Variable` och ange ett mappningsvärde med ett mönster som `*.txt`. I det senare fallet läses filer med tillägget .txt som ett dokument som tvingas som en sträng för att anropa tjänsten.
   * Primitiv Java-typ: Administratören kan ange mappningstypen som `Literal` och ange värdet. Bevakad mapp anropar tjänsten med det angivna värdet.

* Bevakad mapp fungerar med dokument. Utdata som stöds är `com.adobe.idp.Document`, `org.w3c.Document`, `org.w3c.Node` och en lista och karta över dessa typer. Alla andra typer resulterar i felutdata i felmappen.
* Om resultaten inte finns i resultatmappen kontrollerar du om ett fel har inträffat i felmappen.
* Bevakade mappar fungerar bäst om de används i asynkront läge. I det här läget placerar Bevakad mapp anropsbegäran i kön och anropar igen. Kön bearbetas sedan asynkront. När alternativet Asynkron inte har angetts anropas måltjänsten synkront av Bevakade mappar och processmotorn väntar tills tjänsten har slutförts med begäran och resultaten har skapats. Om måltjänsten tar lång tid att behandla begäran kan timeoutfel uppstå i den bevakade mappen.
* Om du skapar bevakade mappar för import- och exportåtgärder tillåts inte abstraktion av filnamnstillägg. När du anropar tjänsten för integrering av formulärdata med bevakade mappar kanske inte filnamnstilläggstypen för utdatafilen matchar det avsedda utdataformatet för dokumentobjekttypen. Om indatafilen till en bevakad mapp som anropar exportåtgärden till exempel är ett XFA-formulär som innehåller data, bör utdata vara en XDP-datafil. Om du vill få en utdatafil med rätt filnamnstillägg kan du ange den i mappningen av utdataparametrar. I det här exemplet kan du använda %F.xdp för att mappa utdataparametrar.
* Bevakade mappar kan bearbeta indatafiler innan de kopieras helt till mappen. Fillåsning är inte obligatoriskt på UNIX som i Windows. Det innebär att när en fil kopieras till en bevakad mapp kan övervakad mapp flytta filen till scenen utan att vänta på att filkopian ska slutföras. Detta beteende gör att endast en del av indatafilen bearbetas. Det finns för närvarande två tillfälliga lösningar:

   * Tillfällig lösning 1

      1. Ange ett mönster för Uteslut filmönster, t.ex. tillfällig&amp;ämpel;ast;.ps.
      1. Kopiera filer som börjar med temporärt (till exempel temp1.ps) till den bevakade mappen.
      1. När filen har kopierats till den bevakade mappen byter du namn på filen så att den matchar mönstret som har angetts för Inkludera filmönster. Bevakad mapp flyttar sedan den färdiga filen till scenen.

   * Tillfällig lösning 2

     Om du vet hur lång tid det tar att kopiera filerna till en bevakad mapp anger du väntetiden i sekunder. Bevakad mapp väntar sedan på den angivna tidslängden innan filen flyttas till scenen.

     Detta är inte något problem för filer i Windows eftersom en fil låses när en tråd skrivs. Detta är dock ett problem för mappar i Windows. För mappar måste du följa stegen i Tillfällig lösning 1.

* Om attributet Bevara mappnamnets slutpunkt för Bevakad mapp är inställt på en null-katalogsökväg, rensas inte mellanlagringskatalogen som den ska vara. Katalogen innehåller fortfarande den bearbetade filen och den tillfälliga mappen.

## Servicespecifika rekommendationer för bevakade mappar {#service-specific-recommendations-for-watched-folders}

För alla tjänster bör du justera gruppstorleken och upprepningsintervallet för den bevakade mappen så att hastigheten med vilken bevakade mappar hämtar nya filer och mappar för bearbetning inte överstiger hastigheten för de jobb som kan bearbetas av AEM Forms Server. De faktiska parametrarna som ska användas kan variera beroende på hur många bevakade mappar som har konfigurerats, vilka tjänster som använder bevakade mappar och hur intensiva jobben är för processorn.

### Generera PDF servicerekommendationer {#generate-pdf-service-recommendations}

* Tjänsten Generate PDF kan bara konvertera en fil i taget för följande filtyper: Microsoft Word, Microsoft Excel, Microsoft PowerPoint, Microsoft Project, AutoCAD, Adobe Photoshop®, Adobe FrameMaker® och Adobe PageMaker®. Det här är långa jobb som körs. Se därför till att batchstorleken är låg. Öka också upprepningsintervallet om det finns fler noder i klustret.
* För filtyperna PostScript (PS), Encapsulated PostScript (EPS) och image kan tjänsten Generate PDF bearbeta flera filer parallellt. Du bör noggrant justera storleken på sessionsböna (som styr antalet konverteringar som ska göras parallellt) beroende på serverns kapacitet och antalet noder i klustret. Öka sedan gruppstorleken till ett tal som är lika med poolstorleken för den sessionsböna för de filtyper som du försöker konvertera. Avsökningsfrekvensen bör bero på antalet noder i klustret. Men eftersom tjänsten Generate PDF utför den här typen av jobb ganska snabbt, kan du konfigurera upprepningsintervallet till ett lågt värde som 5 eller 10.
* Även om Generate PDF-tjänsten bara kan konvertera en OpenOffice-fil i taget går konverteringen mycket snabbt. Ovanstående logik för PS-, EPS- och bildkonverteringar gäller även för OpenOffice-konverteringar.
* Om du vill aktivera enhetlig belastningsfördelning i klustret ska du hålla batchstorleken nere och öka repeteringsintervallet.

### rekommendationer för streckkodsblanketttjänst {#barcoded-forms-service-recommendations}

* För bästa prestanda vid bearbetning av streckkodsformulär (små filer) anger du `10` som Gruppstorlek och `2` som Intervall för upprepning.
* När många filer placeras i indatamappen kan fel med dolda filer som kallas *thumbs.db* uppstå. Vi rekommenderar därför att du anger Inkludera filmönster för inkluderingsfilerna till samma värde som anges för indatavariabeln (till exempel `*.tiff`). Detta förhindrar att Bevakade mappar bearbetar DB-filerna.
* Ett gruppstorleksvärde på `5` och upprepningsintervall på `2` är vanligtvis tillräckligt eftersom tjänsten Barcoded Forms vanligtvis tar cirka 1,5 sekunder att bearbeta en streckkod.
* Bevakad mapp väntar inte på att processmotorn ska slutföra jobbet innan nya filer eller mappar hämtas. Det fortsätter att skanna den bevakade mappen och anropa måltjänsten. Det här beteendet kan överbelasta motorn och ge upphov till resursproblem och tidsgränser. Kontrollera att du använder upprepningsintervall och gruppstorlek för att begränsa indata för bevakad mapp. Du kan öka upprepningsintervallet och minska gruppstorleken om det finns fler bevakade mappar eller aktivera strypning på slutpunkten. Mer information om begränsning finns i [Om begränsning](configuring-watched-folder-endpoints.md#about-throttling).
* Bevakad mapp personifierar användaren som anges i användarnamnet och domännamnet. Bevakad mapp anropar tjänsten som den här användaren om den anropas direkt eller om processen är kort. För en långvarig process anropas processen med systemkontexten. Administratörer kan ange operativsystemprinciper för bevakad mapp för att avgöra vilken användare som ska tillåta eller neka åtkomst till.
* Använd filmönster för att ordna resultat, fel och bevara mappar. (Se [Om filmönster](configuring-watched-folder-endpoints.md#about-file-patterns).)

* Bevakad mapp använder Quartz-schemaläggaren för att skanna de bevakade mapparna. Quartz-schemaläggaren har en trådpool för att skanna dem. Om upprepningsintervallet för den bevakade mappen är mycket lågt (&lt; 5 sekunder) och gruppstorleken är hög (> 2) kan ett konkurrenstillstånd uppstå. När detta inträffar hämtas en fil av två Quartz-trådar:

   * En av trådarna hittar filen och anropar måltjänsten med filen.
   * Den andra tråden ser filen men misslyckas när den försöker ta reda på om filen är giltig (läs- eller skrivfil), vilket orsakar falska fel som anger att filen inte kan bearbetas eftersom den är skrivskyddad. Detta inträffar endast med ett lågt upprepningsintervall och en hög batchstorlek.
