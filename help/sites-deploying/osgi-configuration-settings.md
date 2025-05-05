---
title: Konfigurationsinställningar för OSGi
description: Den här artikeln innehåller information om de OSGi-konfigurationsinställningar (listade enligt paket) som är relevanta för projektimplementeringen. Förteckningen fungerar som riktlinjer och är inte uttömmande.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
docset: aem65
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: d3356f5f-f80f-4ce0-b4e2-3ee927208ab1
source-git-commit: 408f6aaedd2cc0315f6e66b83f045ca2716db61d
workflow-type: tm+mt
source-wordcount: '3247'
ht-degree: 0%

---

# Konfigurationsinställningar för OSGi{#osgi-configuration-settings}

[OSGi](https://www.osgi.org/) är ett grundläggande element i AEM teknikstack. Det används för att styra de sammansatta paketen av AEM och deras konfiguration.

OSGi *innehåller standardmallar som gör att program kan skapas från små, återanvändbara och samarbetskomponenter. Dessa komponenter kan sättas samman till ett program och distribueras*.

Den här funktionen gör det enkelt att hantera paket när de kan stoppas, installeras och startas individuellt. De inbördes beroendena hanteras automatiskt. Varje OSGi-komponent (se [OSGi-specifikationen](https://docs.osgi.org/specification/)) finns i ett av de olika paketen. När du arbetar med AEM finns det flera metoder för att hantera konfigurationsinställningarna för sådana paket. Mer information och rekommenderade tillvägagångssätt finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

Följande OSGi-konfigurationsinställningar (listade efter paket) är relevanta för projektimplementeringen. Alla inställningar som visas behöver inte justeras, vissa anges för att du ska förstå hur AEM fungerar.

>[!CAUTION]
>
>Förteckningen är avsedd att fungera som riktlinjer och är inte uttömmande. Alla paket visas inte och inte heller alla parametrar för vissa av de paket som ingår.
>
>Den konfiguration som krävs varierar från projekt till projekt.
>
>På webbkonsolen finns värden och detaljerad information om parametrar.

>[!NOTE]
>
>Ytterligare programpaket kan behövas för specifika funktionsområden inom AEM. I dessa fall finns konfigurationsinformation på sidan som rör lämplig funktion.

**Händelseavlyssnaren för AEM-replikering** Konfigurera:

* **Körningslägen**, i vilka replikeringshändelser distribueras till avlyssnare. Om det till exempel definieras som författare är det systemet som&quot;initierar&quot; replikeringen.

* Lägg till körningsläget **publish** om projektkoden bearbetar replikeringshändelser (omvänd replikering) i en publiceringsmiljö. Exempel: när Dispatcher används för att tömma från publiceringsmiljön eller när standardsynkronisering med andra publiceringsinstanser sker.

**AEM Repository Change listener** Konfigurera:

* **Sökvägar**, platser att avlyssna för databashändelser som är klara för distribution.

**CRX Sling-klientdatabas** Konfigurera åtkomst till den underliggande innehållsdatabasen.

* **Administratörslösenordet** bör ändras efter installationen för att säkerställa instansens [säkerhet](/help/sites-administering/security-checklist.md).
* Andra ändringar bör inte vara nödvändiga och försiktighet måste vidtas eftersom de kan påverka åtkomsten till databasen.

**Apache Felix OSGi Management Console** Konfigurera:

* **Plugins**, de viktigaste navigeringsobjekten (konsolplugin-program) som ska vara tillgängliga i **Apache Felix Web Management Console** som menyalternativ på den översta nivån. Inaktivera allt du inte behöver eftersom utrymme och resurser krävs för varje åtgärd.

>[!CAUTION]
>
>Var noga med att konfigurera följande:
>
>**Användarnamn** och **lösenord**, autentiseringsuppgifter för åtkomst till själva webbhanteringskonsolen för Apache Felix.
>Lösenordet måste ändras efter den första installationen för att säkerställa instansens [säkerhet](/help/sites-administering/security-checklist.md).

>[!NOTE]
>
>Den här konfigurationen bör göras med Felix Console så som den behövs vid start - innan databasen är tillgänglig.

**Loggaren för anpassningsbara begärandedata för Apache Sling** Konfigurera:

* **Loggningsnamn** och **Loggformat** för att konfigurera plats och format för begäran och åtkomstloggning (standard: `request.log`). Den här loggfilen är viktig när du analyserar prestanda eller felsökningsfunktioner som är relaterade till webbkedjan. Den har parats med [Apache Sling Request Logger](#apacheslingrequestlogger).

Se [AEM-loggning](/help/sites-deploying/configure-logging.md) och [Sling-loggning](https://sling.apache.org/documentation/development/logging.html).

**Konfiguration av trådpoolen Apache Sling**:

* **Minsta poolstorlek** och **Maximal poolstorlek**, storleken på poolen som används för att lagra händelsetrådar.

* **Köstorlek**, den maximala storleken för trådkön om poolen är slut.
Det rekommenderade värdet är `-1` eftersom kön anges till obegränsad. Om en gräns anges kan förluster uppstå när den överskrids.

* Om du ändrar de här inställningarna kan du få bättre prestanda i scenarier med ett stort antal händelser. Exempel: stor användning av AEM DAM eller arbetsflöde.
* Värden som är specifika för ditt scenario bör fastställas med hjälp av tester.
* De här inställningarna kan påverka instansens prestanda, så ändra dem inte utan orsak och vederbörlig hänsyn.

**Apache Sling GET Servlet** Konfigurera några aspekter av återgivning:

* **Automatiskt index** om du vill aktivera/inaktivera katalogåtergivning för bläddring.
* **Aktivera** (eller inaktivera) standardåtergivningar, till exempel **HTML**, **Oformaterad text**, **JSON** eller **XML**.
Inaktivera inte JSON.

>[!NOTE]
>
>Den här inställningen konfigureras automatiskt för produktionsinstanser om du kör AEM i [Produktionsklart läge](/help/sites-administering/production-ready.md).

**Apache Sling Java Script Handler** Konfigurera inställningar för kompilering av Java-filer som skript (serverlets).

Vissa inställningar kan påverka prestanda. Inaktivera de här inställningarna där det är möjligt, särskilt för en produktionsinstans.

* **Source VM** och **Mål-VM** definierar den JDK-version som används som JVM vid körning

* för produktionsenheter:

   * inaktivera **Generera felsökningsinformation**

**Installationsprogram för Apache Sling JCR** De här parametrarna behöver förmodligen inte konfigureras, men de kan vara användbara att känna till när du utvecklar eller felsöker. Installationsmapparna kan till exempel vara användbara när du vill checka in eller ut eller skapa ett paket.

* **Installationsmapparnas namn regexp** och **Maximalt hierarkidjup för installationsmappar** - ange var och i vilket djup databasmapparna ska sökas efter resurser som ska installeras. När ett jokertecken används (som i .&#42;/install) alla lämpliga matchningar söks igenom, till exempel `/libs/sling/install` och `/libs/cq/core/install`.

* **Sökväg**, en lista med sökvägar som installeras söker efter resurser som ska installeras, tillsammans med en siffra som anger viktningsfaktorn för sökvägen.

**Konfiguration av Apache Sling-kö** Konfigurera parametrar som hanterar jobbplanering:

* **Återförsöksintervall**, **Maximalt antal försök**, **maximalt antal parallella jobb** bland andra.

* Om du ändrar de här inställningarna kan prestandan förbättras i scenarier med ett stort antal jobb, till exempel stor användning av AEM DAM och arbetsflöden.
* Värden som är specifika för ditt scenario bör fastställas med hjälp av tester.
* Ändra inte de här inställningarna utan orsak. Ändra bara efter vederbörlig hänsyn.

**Apache Sling JSP Script Handler** Konfigurera prestandarelevanta inställningar för JSP-skripthanteraren. Om du vill förbättra prestandan bör du inaktivera så mycket som möjligt.

Särskilt för produktionsinstanser:

* inaktivera **Generera felsökningsinformation**
* inaktivera **Behåll genererad Java™**
* inaktivera **Mappat innehåll**
* inaktivera **Visa Source-fragment**

>[!NOTE]
>
>Den här inställningen konfigureras automatiskt för produktionsinstanser om du kör AEM i [Produktionsklart läge](/help/sites-administering/production-ready.md).

**Konfiguration för Apache Sling-loggning** Konfigurera:

* **Loggnivå** och **Loggfil** för att definiera platsen och loggnivån för konfigurationen för central loggning (error.log). Nivån kan ställas in på något av `DEBUG`, `INFO`, `WARN`, `ERROR` och `FATAL`.

* **Antal loggfiler** och **Tröskelvärde för loggfil** som definierar loggfilens storlek och versionsrotation.

* **Meddelandemönster** definierar loggmeddelandenas format.

Se [AEM-loggning](/help/sites-deploying/configure-logging.md#global-logging) och [Sling-loggning](https://sling.apache.org/documentation/development/logging.html).

**Loggningskonfiguration för Apache Sling (fabrikskonfiguration)** Konfigurera:

* **Loggnivå**, **Loggfil** och **Meddelandeformat** för att definiera information om loggfilen och meddelanden.

* **Logger** för att definiera kategorin, till exempel bara log för com.day.cq.

* Genom att använda **Fabrikskonfigurationer** kan valfritt antal ytterligare konfigurationer läggas till för att tillgodose de olika loggnivåer och kategorier som behövs.
* Sådana konfigurationer är användbara under utvecklingen, till exempel för att logga TRACE-meddelanden för en viss tjänst i en specifik loggfil.
* Sådana konfigurationer är användbara i en produktionsmiljö, t.ex. för att få meddelanden om en viss tjänst loggade till en enskild loggfil för enklare övervakning.

Se [AEM-loggning](/help/sites-deploying/configure-logging.md) och [Sling-loggning](https://sling.apache.org/documentation/development/logging.html).

**Konfiguration av skrivprogram för Apache Sling Logging (fabrikskonfiguration)** Konfigurera:

* **Loggfil** för att definiera om det finns en loggfil.
* **Antal loggfiler** som definierar versionsrotationen.

* Skrivaren kan användas av en **konfiguration för Apache Sling-loggningsloggning**.

* Sådana konfigurationer är användbara under utvecklingen, till exempel för att logga TRACE-meddelanden för en viss tjänst i en specifik loggfil.
* Sådana konfigurationer är användbara i en produktionsmiljö, t.ex. för att få meddelanden om en viss tjänst loggade till en enskild loggfil för enklare övervakning.

Se [AEM-loggning](/help/sites-deploying/configure-logging.md) och [Sling-loggning](https://sling.apache.org/documentation/development/logging.html).

**Huvudserverkonfiguration för Apache Sling**:

* **Antal anrop per begäran** och **rekursionsdjup** som skyddar datorn mot oändlig rekursion och för många skriptanrop.

**MIME-typtjänst för Apache Sling Commons** Konfigurera:

* **MIME-typer** om du vill lägga till typer som krävs för ditt projekt i systemet. Om du gör det kan en `GET`-begäran för en fil ange rätt innehållstyprubrik för länkning av filtypen och programmet.

**Refererarfilter för Apache Sling** För att åtgärda kända säkerhetsproblem med CSRF (Cross-Site Request Forgery) i CRX WebDAV och Apache Sling måste du konfigurera referensfiltret.

Refererarfiltertjänsten är en OSGi-tjänst som du kan konfigurera:

* vilka http-metoder som ska filtreras
* om en tom referensrubrik tillåts
* och en lista över servrar som ska tillåtas utöver servervärden.

Mer information finns i [Säkerhetschecklistan - Problem med Förfalskning av begäran mellan webbplatser](/help/sites-administering/security-checklist.md#protect-against-cross-site-request-forgery).

>[!NOTE]
>
>Refererarfiltret för Apache Sling beror på installationen av ett snabbkorrigeringspaket.

**Loggaren för Apache Sling-begäran** Konfigurera:

* olika parametrar för att definiera hur förfrågningar loggas.
* **Aktivera begärandeloggen** om du vill aktivera eller inaktivera.

* **Aktivera åtkomstlogg** om du vill aktivera eller inaktivera.

Parad med [Dataloggningsprogrammet för anpassningsbara begäranden för Apache Sling](#apacheslingcustomizablerequestdatalogger).

Se [AEM-loggning](/help/sites-deploying/configure-logging.md) och [Sling-loggning](https://sling.apache.org/documentation/development/logging.html).

**Apache Sling Resource Resolver Factory** Konfigurera centrala aspekter av Sling-resursupplösning:

* **Sökvägar för resurser**, lägg till projektspecifika sökvägar (men ta inte bort `/libs` eller `/apps`).

* **Virtuella URL:er** för att definiera dina egna URL-mappningar.

* **URL-mappningar** för att definiera alias. Exempel: från `/content` till `/`.

* **Mappningsplats**, mappningskonfigurationen har externaliserats i `/etc/map`.

* Använd din lokala installation (använd till exempel `https://localhost:4502/system/console/jcrresolver`) för att avgöra vilken resurslösare som är aktiv.

Se: [https://cwiki.apache.org/confluence/display/SLING/Flexible+Resource+Resolution](https://cwiki.apache.org/confluence/display/SLING/Flexible+Resource+Resolution).

>[!CAUTION]
>
>Konfigurera dessa alternativ i databasen.
>
>I annat fall kan ändringar som görs i **URL-mappningar** med Felix-konsolen skrivas över av AEM vid nästa start.

**Hanterare för Apache Sling-server/skript** Hanteraren för Sling-servern och skriptlösaren har flera uppgifter:

1. Den används som `ServletResolver` för att välja den server eller det skript som ska anropas för att hantera begäran.

1. Det fungerar som `SlingScriptResolver`.

1. Den hanterar felhantering genom att implementera `ErrorHandler`-gränssnittet med samma algoritm för att välja felhanteringsservrar och skript som används för att matcha servrar och skript för begärandebearbetning.

Du kan ange olika parametrar, bland annat:

* **Körningssökvägar** - Visar sökvägarna för körbara skript. Genom att konfigurera specifika sökvägar kan du begränsa vilka skript som kan köras. Om ingen sökväg är konfigurerad används standardvärdet ( `/` = root), vilket tillåter körning av alla skript.
Om ett konfigurerat sökvägsvärde avslutas med ett snedstreck genomsöks hela underträdet. Utan ett sådant avslutande snedstreck körs skriptet bara om det är en exakt matchning.

* **Standardtillägg** - Listan med tillägg som standardbeteendet används för. Det sista sökvägssegmentet i resurstypen kan användas som skriptnamn.

**Proxykonfiguration för Apache HTTP Components** - Proxykonfigurationen för all kod som använder Apache HTTP-klienten, används när en HTTP görs. Till exempel vid replikering.

När du skapar en konfiguration ska du inte ändra fabrikskonfigurationen. Skapa i stället en fabrikskonfiguration för den här komponenten med konfigurationshanteraren som finns här: **https://localhost:4502/system/console/configMgr/**. Proxykonfigurationen är tillgänglig i **org.apache.http.proxyconfigurator.**

**Adobe Granite HTML Library Manager** Konfigurera för att styra hanteringen av klientbibliotek (css eller js), inklusive, t.ex., hur den underliggande strukturen visas.

* För produktionsinstanser:

   * aktivera **Minify** (för att ta bort CRLF- och whitespace-tecken).
   * aktivera **Gzip** (om du vill tillåta att filer grupperas och nås med en begäran).
   * inaktivera **Felsök**
   * inaktivera **Timing**

* För JS-utveckling (särskilt vid felsökning/felsökning):

   * inaktivera **Minify**
   * aktivera **Debug** för att separera filerna för felsökning och använda med brandfel.
   * aktivera **Timing** om du är intresserad av timing.
   * aktivera **Debug**-konsolen för att visa JS-konsolens loggmeddelanden.

>[!CAUTION]
>
>När du ändrar inställningen för antingen **Minify** eller **Gzip** tar du bort innehållet i cachen för klienter. Mer information finns i [kunskapsbasartikeln](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-16543.html?lang=sv-SE).

>[!NOTE]
>
>Den här inställningen konfigureras automatiskt för produktionsinstanser om du kör AEM i [Produktionsklart läge](/help/sites-administering/production-ready.md).

**Adobe Granite HTTP Header Authentication Handler** Systemomfattande inställningar för den grundläggande autentiseringsmetoden i HTTP-begäran.

När du använder [stängda användargrupper](/help/sites-administering/cug.md) kan du bland annat konfigurera följande:

* **HTTP-sfär**
* **Standardinloggningssidan**

**Day CQ Link Checker Service** Kontrollera och konfigurera vid behov:

* **Schemaläggarperiod** för att definiera intervallet där externa länkar ska kontrolleras automatiskt.

* Kontrollera **Felaktigt intervall för länktolerans** för den period efter vilken en felaktig extern länk anses vara skadad.
* **Åsidosätt mönster för länkkontroll** om du vill definiera sökvägar som ska uteslutas från länkkontroll.

**Dagens CQ-länkkontrollsaktivitet** Konfigurera inställningar för en enskild länkkontrollsaktivitet (en uppgift som kontrollerar en extern länk):

* Kontrollera intervallen som definierats i **Testintervall för bra länkar** och **Testintervall för felaktig länk**

* De olika parametrar som rör proxy för Internetåtkomst och NTLM som behövs för extern åtkomst när en länk kontrolleras.

**Day CQ Mail Service** Konfigurera värdnamn och åtkomstinformation för e-postservern. Mer information finns i avsnittet Konfigurera e-posttjänsten.

**Day CQ MCM Newsletter** Konfigurera de olika inställningar som används i nyhetsbrevet.

**Dag CQ-rotmappning** Konfigurera:

* **Målsökväg** som definierar var en begäran till `/` omdirigeras till.

Det finns två gränssnitt i AEM:

* det pekaktiverade användargränssnittet är standardgränssnittet
* och det inaktuella klassiska användargränssnittet fungerar fortfarande

Med AEM Root Mapping kan du konfigurera det användargränssnitt som du vill använda som standard för din instans:

* **Målsökvägen** bör peka på följande om du vill att det pekaktiverade användargränssnittet ska vara standardgränssnittet:

  ```shell
     /projects.html
  ```

* **Målsökvägen** ska peka på följande om du vill använda det klassiska användargränssnittet som standardgränssnitt:

  ```shell
     /welcome.html
  ```

>[!NOTE]
>
>I en standardinstallation är det pekoptimerade användargränssnittet standardgränssnittet.

**Adobe Bevilja SSO-autentiseringshanterare** - Konfigurera SSO-information (enkel inloggning). Dessa uppgifter behövs ofta i Enterprise Author Setup, ofta med LDAP.

Det finns olika konfigurationsegenskaper:

* **Sökväg**
Sökvägen som den här autentiseringshanteraren är aktiv för. Om den här parametern lämnas tom inaktiveras autentiseringshanteraren. Sökvägen / gör att autentiseringshanteraren används för hela databasen.

* **Servicerankning**
OSGi Framework Service Ranking-värdet används för att ange den order som används för att anropa den här tjänsten. Det här värdet är ett `int`-värde där högre värden anger högre prioritet.
Standardvärdet är `0`.

* **Rubriknamn**
Namnen på rubrikerna som kan innehålla ett användar-ID.

* **Cookie-namn**
Namnen på cookies som kan innehålla ett användar-ID.

* **Parameternamn**
Namnen på parametrarna som kan ge användar-ID.

* **Användarkarta**
För valda användare kan användarnamnet som extraheras från HTTP-begäran ersättas med ett annat i autentiseringsobjektet. Mappningen definieras här. Om användarnamnet `admin` visas på båda sidor om kartan ignoreras mappningen. Tecknet &quot;=&quot; måste föregås av ett inledande &quot;\&quot;.

* **Format**
Anger i vilket format användar-ID:t anges. Använd:

   * `Basic` om användar-ID är kodat i HTTP Basic Authentication-format
   * `AsIs` om användar-ID anges i oformaterad text eller om ett reguljärt uttryck använder ett värde som det är eller något reguljärt uttryck

**Dag CQ WCM-felsökningsfilter** Detta är användbart vid utveckling eftersom det tillåter användning av suffix som ?debug=layout vid åtkomst till en sida. https://localhost:4502/cf#/content/geometrixx/en/support.html?debug=layout innehåller till exempel layoutinformation som kan vara av intresse för utvecklaren.

* För att säkerställa prestanda och säkerhet ska du inaktivera på produktionsenheter.

**Dag CQ WCM-filter** Konfigurera:

* **WCM-läge** för att definiera standardläge.
* På en författarinstans kan det här läget vara `edit`, `disable,preview` eller `analytics`.
De andra lägena kan nås från sidosparken eller så kan suffixet `?wcmmode=disabled` användas för att emulera en produktionsmiljö.

* På en publiceringsinstans måste det här läget anges till `disabled` för att säkerställa att inget annat läge är tillgängligt.

>[!NOTE]
>
>Den här inställningen konfigureras automatiskt för produktionsinstanser om du kör AEM i [Produktionsklart läge](/help/sites-administering/production-ready.md).

**Dagens CQ WCM-länkkontroll** Konfigurera:

* **Lista över omskrivningskonfigurationer** för att ange en lista över platser för innehållsbaserade länkkontrollerarkonfigurationer. Konfigurationerna kan baseras på körningsläge. Detta är viktigt för att skilja mellan skribent- och publiceringsmiljöer, eftersom inställningarna för länkkontroll kan variera.

**Day CQ WCM Page Manager Factory** Konfigurera:

* **Aktiveringskontroll för sidunderträd** för en användare (utan replikeringsbehörighet) att ta bort eller flytta sidor (även om sidorna inte är aktiverade).

**Dagens CQ WCM-sidprocessor** Konfigurera:

* **Banor**, en lista över platser där systemet lyssnar efter sidändringar innan en `jcr:Event` aktiveras.

**Spårare för Adobe-sidkomprimering** För en författarinstans ska du konfigurera så här:

* **sling.auth.requirements**: ange värdet för den här egenskapen till `-/libs/wcm/stats/tracker`

>[!CAUTION]
>
>Den här konfigurationen tillåter anonyma begäranden till spårningstjänsten.

>[!NOTE]
>
>Mer information finns i [Sidavbildningar](/help/sites-deploying/configuring.md#enabling-page-impressions).

**Dagens CQ WCM-sidstatistik** Konfigurera för en publiceringsinstans:

* **URL för att skicka data** för att konfigurera URL:en som används för att spåra sidstatistik (är viktig om en spårningsbegäran går igenom Dispatcher). Standardvärdet är till exempel `https://localhost:4502/libs/wcm/stats/tracker`.

* **Spårningsskript aktiverat** för att aktivera ( `true`) eller inaktivera ( `false`) inkludering av spårningsskriptet på sidorna. Standardvärdet är `false`.

>[!NOTE]
>
>Mer information finns i [Sidavbildningar](/help/sites-deploying/configuring.md#enabling-page-impressions).

**Day CQ WCM Version Manager** Kontrollera om och hur versioner hanteras i systemet:

* **Skapa version vid aktivering**, aktiverat i en standardinstallation
* **Aktivera rensning**

* **Rensa banor**, sökvägarna som sökningen igenom.
* **Implicit versionssökvägar**, sökvägarna där implicit versionshantering är aktiv.

* **Högsta versionsålder**, högsta ålder (i dagar) för en version

* **Max antal versioner**, maximalt antal versioner att behålla

Mer information finns i [Rensning av version](/help/sites-deploying/version-purging.md).

**Day CQ Workflow Email Notification Service** Konfigurera e-postinställningarna för meddelanden som skickas av ett arbetsflöde.

**Adobe AEM Rewriter HTML Parser Factory**

Controls the HTML Parser for the CQ rewriter.

* **Ytterligare taggar att bearbeta** - Du kan lägga till eller ta bort HTML-taggar som ska bearbetas av parsern. Som standard bearbetas följande taggar: A, IMG, AREA, FORM, BASE, LINK, SCRIPT, BODY, HEAD.
* **Bevara kameraskiftläge** - Som standard konverterar HTML-tolken attribut i kameraläge (till exempel `eBay`) till gemener (till exempel `ebay`). Du kan inaktivera den här inställningen om du vill bevara attributen för kamerans skiftläge. Den här inställningen är användbar när du använder ramverk som Angular 2.

**Day Commons JDBC Connections Pool** Konfigurera åtkomst till en extern databas som används som innehållskälla.

En fabrikskonfiguration, så att flera instanser kan konfigureras.

**CDN Rewriter** Kommunikation mellan AEM och ett CDN måste säkerställas så att resurser och binära filer levereras till en slutanvändare på ett säkert sätt. Den här processen omfattar följande två uppgifter:

* Åtkomst till resursen från AEM via CDN första gången (eller efter att den har gått ut i cache).
* Åtkomst till resursen som cachelagrats i CDN sker på ett säkert sätt. När resursen har cachelagrats i CDN går begäran inte till AEM, och alla användare som har åtkomst till resursen på ska hanteras från CDN.

AEM tillhandahåller en omskrivare för att skriva om interna URL:er för resurser till externa CDN-URL:er. Den skriver om länkar som ska skickas vidare till CDN, inklusive en JWS-signatur, och anger att resursen ska kunna nås på ett säkert sätt. Den här funktionen ska användas på författarinstanser.

Det övergripande flödet är följande:

1. Användaren autentiserar med AEM och begär en sida med resurser.
1. Den begärda sidan innehåller en resurs som liknar `/content/dam/geometrixx-media/articles/paladin_trailer.jpg/jcr:content/renditions/cq5dam.thumbnail.319.319.png`
1. Omskrivare omvandlar länken till en CDN-URL som innehåller en JWS-signatur:
   `CDN_domain/content/dam/geometrixx-media/articles/paladin_trailer.jpg/_jcr_content/renditions/cq5dam.thumbnail.319.319.png?cdn_sign=JWS_SIGNATURE`

1. Användarens webbläsare vidarebefordrar sedan resursbegäran till CDN-servern
1. CDN bör konfigureras för att vidarebefordra begäran till AEM tillsammans med parametern `cdn_sign`.
1. En autentiseringshanterare validerar parametern `cdn_sign` och returnerar resursen till CDN som sedan levereras till användaren

Flödet mellan användarens webbläsare, CDN och AEM kan visas på följande sätt.

![chlimage_1-8](assets/chlimage_1-8.png)

>[!NOTE]
>
>Den här funktionen är bara aktiverad för AEM författarinstanser.

**CDNConfigServiceImpl** Tillhandahåller CDN-konfigurationer

CDN-omskrivningsfunktionen kan aktiveras genom att **CDN-distributionsdomännamnet** anges i konfigurationen för com.adobe.cq.cdn.rewriter.impl.CDNConfigServiceImpl.

Tjänsten innehåller även andra konfigurationsalternativ som att aktivera/inaktivera CDN-omskrivning, sökvägsprefix som CDN-omskrivning utförs för, TTL-värden och protokoll (HTTP eller HTTPS).

**CDNRewriter** En omskrivare för att skriva om interna bild-URL:er till CDN-URL:er

Värdet **Taggattribut** i com.adobe.cq.cdn.rewriter.impl.CDNRewriter kan definieras så att endast selektiva bildlänkar skrivs om.
