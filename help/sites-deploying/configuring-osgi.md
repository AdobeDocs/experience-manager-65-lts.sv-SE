---
title: Konfigurerar OSGi
description: OSGi är en grundläggande del i Adobe Experience Manager (AEM). Det används för att styra de sammansatta paketen av AEM och deras konfiguration. I den här artikeln beskrivs hur du kan hantera konfigurationsinställningarna för sådana paket.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 3bf3ba2e-f5f2-428a-a1fc-36f885350f6b
source-git-commit: 408f6aaedd2cc0315f6e66b83f045ca2716db61d
workflow-type: tm+mt
source-wordcount: '1900'
ht-degree: 0%

---

# Konfigurerar OSGi{#configuring-osgi}

[OSGi](https://www.osgi.org/) är ett grundläggande element i Adobe Experience Manager (AEM) teknikstack. Det används för att styra de sammansatta paketen av AEM och deras konfiguration.

OSGi *innehåller standardmallar som gör att program kan skapas från små, återanvändbara och samarbetskomponenter. Dessa komponenter kan sättas samman till ett program och distribueras*.

På så sätt blir det enkelt att hantera paket när de kan stoppas, installeras och startas individuellt. De inbördes beroendena hanteras automatiskt. Varje OSGi-komponent (se [OSGi-specifikationen](https://docs.osgi.org/specification/)) finns i ett av de olika paketen.

Du kan hantera konfigurationsinställningarna för sådana paket genom att antingen:

* med [Adobe CQ-webbkonsolen](#osgi-configuration-with-the-web-console)
* använder [konfigurationsfiler](#osgi-configuration-with-configuration-files)
* konfigurerar [content-nodes ( `sling:OsgiConfig`) i databasen](#osgi-configuration-in-the-repository)

Båda metoderna kan användas trots att det finns små skillnader, främst i förhållande till [körningslägena](/help/sites-deploying/configure-runmodes.md):

* [Adobe CQ Web Console](#osgi-configuration-with-the-web-console)

   * Webbkonsolen är standardgränssnittet för OSGi-konfiguration. Det innehåller ett användargränssnitt för redigering av de olika egenskaperna, där möjliga värden kan väljas från fördefinierade listor.

     Det är därför den enklaste metoden.

   * Alla konfigurationer som görs med webbkonsolen tillämpas omedelbart och tillämpas på den aktuella instansen, oavsett aktuellt körningsläge eller eventuella efterföljande ändringar av körningsläget.

* [konfigurationsfiler](#osgi-configuration-with-configuration-files)

   * Innehåller inställningar som är definierade i webbkonsolen.
   * Kan inkluderas i innehållspaket för användning i andra instanser.

* [content-nodes (sling:osgiConfig) i databasen](#osgi-configuration-in-the-repository)

   * Kräver manuell konfiguration med CRXDE Lite.
   * På grund av namngivningskonventionerna för noderna `sling:OsgiConfig` kan du koppla konfigurationen till ett specifikt [körläge](/help/sites-deploying/configure-runmodes.md). Du kan även spara konfigurationer för mer än ett körningsläge i samma databas.
   * Alla lämpliga konfigurationer används omedelbart (beroende på körningsläget).

Alla dessa konfigurationsmetoder, oavsett vilken metod du använder:

* Kontrollera att identiska konfigurationer återskapas när du kopierar eller replikerar databasinnehållet.
* Gör att du kan checka ut konfigurationer till FileVault eller Subversion, antingen av säkerhetsskäl eller för ytterligare uppdateringar.
* Kan sparas i paket som kan användas när du konfigurerar andra instanser.
* Gör att du kan utföra konfigurationsimplementeringar med skript för att sprida konfigurationsinformationen.

>[!NOTE]
>
>Information om vissa viktiga inställningar visas under [OSGi-konfigurationsinställningar.](/help/sites-deploying/osgi-configuration-settings.md)

## OSGi-konfiguration med webbkonsolen {#osgi-configuration-with-the-web-console}

[Webbkonsolen](/help/sites-deploying/web-console.md) i AEM har ett standardiserat gränssnitt för att konfigurera paketen. Fliken **Konfiguration** används för att konfigurera OSGi-paket och är därför den underliggande mekanismen för att konfigurera AEM systemparametrar.

Alla ändringar som görs tillämpas omedelbart på den aktuella OSGi-konfigurationen, ingen omstart krävs.

>[!NOTE]
>
>Ändringar som görs i webbkonsolen sparas i databasen som [konfigurationsfiler](#osgi-configuration-with-configuration-files). Dessa filer kan inkluderas i innehållspaket för återanvändning i andra installationer.

>[!NOTE]
>
>På webbkonsolen gäller alla beskrivningar som anger standardinställningar för Sling.
>
>Adobe Experience Manager har sina egna standardinställningar, så de standardvärden som är angivna kan skilja sig från standardvärdena som är dokumenterade på konsolen.

Så här uppdaterar du en konfiguration med webbkonsolen:

1. Gå till fliken **Konfiguration** i webbkonsolen genom att antingen:

   * Öppnar webbkonsolen från länken på menyn **Verktyg > Åtgärder** . När du har loggat in på konsolen kan du använda den nedrullningsbara menyn i:

     **OSGi >**

   * Den direkta URL:en, till exempel:

     `http://localhost:4502/system/console/configMgr`

   En lista visas.

1. Välj det paket som du vill konfigurera av:

   * klicka på ikonen **Redigera** för det paketet
   * klicka på paketets **namn**

1. En dialogruta öppnas. Här kan du redigera efter behov. Ange till exempel **Loggnivå** till `INFO`:

   ![chlimage_1-140](assets/chlimage_1-140.png)

   >[!NOTE]
   >
   >Uppdateringar sparas i databasen som [konfigurationsfiler](#osgi-configuration-with-configuration-files). Om du vill hitta de här filerna efteråt och inkludera dem i ett innehållspaket för användning på en annan instans, till exempel, bör du anteckna den beständiga identiteten ( `PID`).

1. Klicka på **Spara**.

   Ändringarna tillämpas omedelbart på den aktuella OSGi-konfigurationen för det system som körs, ingen omstart krävs.

   >[!NOTE]
   >
   >Du kan nu hitta de relaterade [konfigurationsfilerna](#osgi-configuration-with-configuration-files). Om du till exempel vill inkludera innehåll i ett innehållspaket som ska användas på en annan instans.

## OSGi-konfiguration med konfigurationsfiler {#osgi-configuration-with-configuration-files}

Konfigurationsändringar som görs med webbkonsolen sparas i databasen som konfigurationsfiler ( `.config`) under:

`/apps`

Dessa filer kan inkluderas i innehållspaket och återanvändas i andra instanser.

>[!NOTE]
>
>Konfigurationsfilernas format är specifikt - se Sling Apache-dokumentationen för:
>* fullständig information om [Apache Sling Provisioning Model och Apache SlingStart](https://sling.apache.org/documentation/development/slingstart.html#default-configuration-format).
>* självstudier och exempel på [Hämta resurser och egenskaper i Sling](https://sling.apache.org/documentation/tutorials-how-tos/getting-resources-and-properties-in-sling.html).
>
>Därför rekommenderar vi att du skapar och underhåller konfigurationsfilen genom att göra faktiska ändringar i webbkonsolen.

Webbkonsolen visar ingen indikation på var i databasen dina ändringar har sparats, men de kan enkelt hittas:

1. Skapa konfigurationsfilen genom att [göra en första ändring i webbkonsolen](#osgi-configuration-with-the-web-console).
1. Öppna CRXDE Lite.
1. Välj **Fråga ...** på menyn **Verktyg** .
1. Om du vill söka efter PID för konfigurationen som du har uppdaterat skickar du en fråga av typen **Type** `SQL`.

   **Apache Felix OSGi Management Console** har till exempel den beständiga identiteten (PID):

   `org.apache.felix.webconsole.internal.servlet.OsgiManager`

   Så SQL-frågan kan vara:

   ```shell
   select * from nt:base where jcr:path like '/apps/%' and contains(*, 'org.apache.felix.webconsole.internal.servlet.OsgiManager')
   ```

1. Konfigurationsfilsnoden visas.

   I exemplet ovan:

   `/apps/system/config/org.apache.felix.webconsole.internal.servlet.OsgiManager.config`

   >[!CAUTION]
   >
   >Du kan öppna den här filen om du vill visa ändringarna, men för att undvika att skriva fel rekommenderar vi att du gör faktiska ändringar i konsolen.

1. Du kan nu skapa ett innehållspaket som innehåller den här noden och använda det som behövs på dina andra instanser.

## OSGi-konfiguration i databasen {#osgi-configuration-in-the-repository}

Förutom att använda webbkonsolen kan du även definiera konfigurationsinformation i databasen. På så sätt kan du enkelt konfigurera olika körningslägen.

Dessa konfigurationer görs genom att `sling:OsgiConfig` noder skapas i databasen som systemet refererar till. De här noderna speglar OSGi-konfigurationerna och ett användargränssnitt har skapats för dem. Om du vill uppdatera konfigurationsdata uppdaterar du nodegenskaperna.

Om du ändrar konfigurationsdata i databasen tillämpas ändringarna omedelbart på den aktuella OSGi-konfigurationen. Det är som om ändringarna gjorts med webbkonsolen, med lämplig validering och konsekvenskontroll. Det här arbetsflödet gäller även kopiering av en konfiguration från `/libs/` till `/apps/`.

Eftersom samma konfigurationsparameter finns på flera ställen:

* söker efter alla noder av typen `sling:OsgiConfig`
* filter efter tjänstnamn
* filter enligt körningsläge


### Lägga till en ny konfiguration i databasen {#adding-a-new-configuration-to-the-repository}

#### Vad du behöver veta {#what-you-need-to-know}

Om du vill lägga till en konfiguration i databasen måste du känna till följande:

1. Tjänstens **PID (Persistent Identity**).

   Referera till **konfigurationerna** i webbkonsolen. Namnet visas i **konfigurationsinformationen** längst ned på sidan.

   Till exempel `com.day.cq.wcm.core.impl.VersionManagerImpl.` för att konfigurera **AEM WCM Version Manager**.

   ![chlimage_1-141](assets/chlimage_1-141.png)

1. Krävs ett specifikt [körningsläge](/help/sites-deploying/configure-runmodes.md)? Skapa mappen:

   * `config` - för alla körningslägen
   * `config.author` - för författarmiljön
   * `config.publish` - för publiceringsmiljön
   * `config.<run-mode>` - efter behov

1. Krävs en **konfiguration** eller **fabrikskonfiguration**?
1. De enskilda parametrar som ska konfigureras, inklusive befintliga parameterdefinitioner som måste återskapas.

   Referera till det enskilda parameterfältet i webbkonsolen. Namnet visas inom hakparenteser för varje parameter.

   Skapa till exempel en egenskap
   `versionmanager.createVersionOnActivation` om du vill konfigurera **Skapa version vid aktivering**.

   ![chlimage_1-142](assets/chlimage_1-142.png)

1. Finns det en konfiguration i `/libs`? Om du vill visa alla konfigurationer i din instans använder du **frågeverktyget** i CRXDE Lite för att skicka följande SQL-fråga:

   `select * from sling:OsgiConfig`

   I så fall kan konfigurationen kopieras till ` /apps/<yourProject>/` och sedan anpassas på den nya platsen.

#### Skapa konfigurationen i databasen {#creating-the-configuration-in-the-repository}

Så här lägger du till den nya konfigurationen i databasen:

1. Använd CRXDE Lite för att navigera till:

   ` /apps/<yourProject>`

1. Om den inte finns skapar du mappen `config` ( `sling:Folder`):

   * `config` - gäller för alla körningslägen
   * `config.<run-mode>` - specifikt för ett visst körläge

1. Skapa en nod under den här mappen:

   * Typ: `sling:OsgiConfig`
   * Namn: den beständiga identiteten (PID).

     för AEM WCM Version Manager använder du `com.day.cq.wcm.core.impl.VersionManagerImpl`

   >[!NOTE]
   >
   >När du gör en fabrikskonfiguration lägger du till `~<identifier>` till namnet.
   >
   >Som i: `org.apache.sling.commons.log.LogManager.factory.config~<identifier>`
   >
   >Där `<identifier>` ersätts med fritext som du (måste) anger för att identifiera instansen (du kan inte utelämna den här informationen), till exempel:
   >
   >`org.apache.sling.commons.log.LogManager.factory.config~MINE`

1. Skapa en egenskap på den här noden för varje parameter som du vill konfigurera:

   * Namn: parameternamnet som visas i webbkonsolen. Namnet visas inom hakparenteser i slutet av fältbeskrivningen. Använd till exempel `Create Version on Activation` för `versionmanager.createVersionOnActivation`
   * Typ: efter behov.
   * Värde: efter behov.

   Du får bara skapa egenskaper för de parametrar som du vill konfigurera, medan andra fortfarande använder de standardvärden som angetts av AEM.

1. Spara alla ändringar.

   Ändringarna tillämpas när noden uppdateras genom att tjänsten startas om (som med ändringarna i webbkonsolen).

>[!CAUTION]
>
>Ändra ingenting i sökvägen `/libs`.

>[!CAUTION]
>
>Den fullständiga sökvägen till en konfiguration måste vara korrekt för att den ska kunna läsas vid start.

## Konfigurationsinformation {#configuration-details}

### Upplösningsordning vid start {#resolution-order-at-startup}

Följande prioritetsordning används:

1. Databasnoder under `/apps/*/config...`.antingen med typen `sling:OsgiConfig` eller egenskapsfiler.

1. Databasnoder med typen `sling:OsgiConfig` under `/libs/*/config...`. (färdiga definitioner).

1. Alla `.config`-filer från `<*cq-installation-dir*>/crx-quickstart/launchpad/config/...`. i det lokala filsystemet.

En allmän konfiguration i `/libs` kan maskeras av en projektspecifik konfiguration i `/apps`.

### Upplösningsordning vid körning {#resolution-order-at-runtime}

Konfigurationsändringar som görs medan systemet körs utlöser en omladdning med den ändrade konfigurationen.

Sedan gäller följande prioritetsordning:

1. När du ändrar en konfiguration i webbkonsolen börjar det gälla direkt när det gäller körning.
1. Ändring av en konfiguration i `/apps` börjar gälla omedelbart.
1. Ändring av en konfiguration i `/libs` börjar gälla omedelbart, såvida den inte maskeras av en konfiguration i `/apps`.

### Upplösning för flera körningslägen {#resolution-of-multiple-run-modes}

För körningslägesspecifika konfigurationer kan flera körningslägen kombineras. Du kan till exempel skapa konfigurationsmappar med följande format:

`/apps/*/config.<runmode1>.<runmode2>/`

Konfigurationer i sådana mappar används om alla körningslägen matchar ett körningsläge som har definierats vid start.

Om en instans till exempel startades med körningslägena `author,dev,emea` tillämpas konfigurationsnoderna i `/apps/*/config.emea`, `/apps/*/config.author.dev/` och `/apps/*/config.author.emea.dev/`, medan konfigurationsnoderna i `/apps/*/config.author.asean/` och `/config/author.dev.emea.noldap/` inte tillämpas.

Om flera konfigurationer för samma PID kan användas, används konfigurationen med det högsta antalet matchande körningslägen.

Om en instans till exempel startades med körningslägena `author,dev,emea`, definierar både `/apps/*/config.author/` och `/apps/*/config.emea.author/` en konfiguration för
`com.day.cq.wcm.core.impl.VersionManagerImpl` används konfigurationen i `/apps/*/config.emea.author/` .

Regelns granularitet är på PID-nivå.
Du kan inte definiera vissa egenskaper för samma PID i `/apps/*/config.author/` och mer specifika i `/apps/*/config.emea.author/` för samma PID.
Konfigurationen med det högsta antalet matchande körningslägen gäller för hela PID.

### Standardkonfigurationer {#standard-configurations}

Om det finns någon standardkonfiguration i `/libs` får den inte redigeras direkt, utan kopieras till programområdet ( `/apps`) före anpassning.

Om du vill visa alla konfigurationsnoder i din instans använder du funktionen **Fråga** i CRXDE Lite för att skicka följande SQL-fråga:

`select * from sling:OsgiConfig`

### Konfigurationsbeständighet {#configuration-persistence}

* Om du ändrar en konfiguration via webbkonsolen skrivs den (vanligtvis) in i databasen på:

  `/apps/{somewhere}`

   * Som standard är `{somewhere}` `system/config` så konfigurationen skrivs till

     `/apps/system/config`

   * Om du redigerar en konfiguration som ursprungligen kom från en annan plats i databasen: till exempel:

     /libs/foo/config/someconfig

     Den uppdaterade konfigurationen skrivs sedan under den ursprungliga platsen, till exempel:

     `/apps/foo/config/someconfig`

* Inställningar som ändras av `admin` sparas i `*.config` filer under:

  ```
     /crx-quickstart/launchpad/config
  ```

   * Det här området är de privata data som tillhör OSGi-konfigurationsadministratören och innehåller alla konfigurationsdetaljer som anges av `admin`, oavsett hur de angavs i systemet.
   * Det här området är en implementeringsdetalj och du får aldrig redigera den här katalogen direkt.
   * Det är emellertid användbart att veta var dessa konfigurationsfiler finns så att kopior kan tas för säkerhetskopiering, eller för flera installationer, eller både och:

      * Apache Felix OSGi Management Console

        `../crx/org/apache/felix/webconsole/internal/servlet/OsgiManager.config`

      * CRX Sling Client Repository

        `../com/day/crx/sling/client/impl/CRXSlingClientRepository/<pid-nr>.config`

>[!CAUTION]
>
>Redigera aldrig mappar eller filer under:
>
>`/crx-quickstart/launchpad/config`
