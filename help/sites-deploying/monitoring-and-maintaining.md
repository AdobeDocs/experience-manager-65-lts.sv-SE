---
title: Övervaka och underhålla din Adobe Experience Manager-instans
description: Lär dig hur du övervakar och underhåller din Adobe Experience Manager-instans.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
docset: aem65
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '5792'
ht-degree: 0%

---

# Övervaka och underhålla din Adobe Experience Manager-instans{#monitoring-and-maintaining-your-aem-instance}

När AEM-instanserna har distribuerats måste du övervaka och underhålla deras åtgärder, prestanda och integritet.

En viktig faktor här är att du måste känna till hur systemet ser ut och fungerar under normala förhållanden för att kunna identifiera potentiella problem. Detta görs bäst genom att man övervakar systemet och samlar in information över tid.

| Kontrollera | Överväganden | Kommentar/åtgärder |
|---|---|---|
| Planen för säkerhetskopiering. |  | [Säkerhetskopiera din instans](/help/sites-deploying/monitoring-and-maintaining.md#backups). |
| plan för katastrofåterställning. | Företagets riktlinjer för katastrofåterställning. |  |
| Det finns ett felspårningssystem för rapportering av problem. | Exempel: [Bugzilla](https://www.bugzilla.org/), [Jira](https://www.atlassian.com/software/jira) eller någon annan. |  |
| Filsystemen övervakas. | CRX-databasen &quot;fryser&quot; om det inte finns tillräckligt med ledigt diskutrymme. Den återupptas när utrymme blir tillgängligt. | `*ERROR* LowDiskSpaceBlocker`-meddelanden kan visas i loggfilen när det lediga utrymmet börjar ta slut. |
| [Loggfiler](/help/sites-deploying/monitoring-and-maintaining.md#working-with-audit-records-and-log-files) övervakas. |  |  |
| Systemövervakning körs (kontinuerligt) i bakgrunden. | Inklusive CPU, minne, disk- och nätverksanvändning. Med exempelvis iostat / vmstat / permon. | Loggade data visas och kan användas för att spåra prestandaproblem. Rådata är också tillgängliga. |
| [AEM prestanda övervakas](/help/sites-deploying/monitoring-and-maintaining.md#monitoring-performance). | [Begär räknare](/help/sites-deploying/monitoring-and-maintaining.md#request-counters) för övervakning av trafiknivåer. | Om en betydande eller långvarig förlust av resultat konstateras bör en detaljerad undersökning göras. |
| Du övervakar dina [replikeringsagenter](/help/sites-deploying/monitoring-and-maintaining.md#monitoring-your-replication-agents). |  |  |
| Rensa arbetsflödesinstanser regelbundet. | Databasstorlek och arbetsflödets prestanda. | Se [Regelbunden rensning av arbetsflödesinstanser](/help/sites-administering/workflows-administering.md#regular-purging-of-workflow-instances). |

## Säkerhetskopior {#backups}

Det är god praxis att säkerhetskopiera

* Programvaruinstallationen - före/efter viktiga ändringar i konfigurationen
* Innehållet i databasen - regelbundet

Företaget har förmodligen en säkerhetskopieringsprincip som du följer, ytterligare överväganden om vad och när du ska säkerhetskopiera inkluderar följande:

* hur viktigt systemet och data är.
* hur ofta ändringar görs i antingen programvaran eller data.
* datavolym; kapacitet kan ibland vara ett problem, liksom tiden för att utföra säkerhetskopieringen.
* om säkerhetskopieringen kan göras medan användarna är online och, om möjligt, vilken prestandaeffekt har säkerhetskopieringen?
* användarens geografiska utbredning, det vill säga när är den bästa tidpunkten att säkerhetskopiera (för att minimera påverkan)?
* Din återställningspolicy. Finns det riktlinjer för var säkerhetskopierade data ska lagras (t.ex. utanför platsen och ett visst medium).

Ofta utförs en fullständig säkerhetskopiering med regelbundna intervall (t.ex. varje dag, varje vecka eller varje månad), med inkrementella säkerhetskopieringar mellan (t.ex. varje timme, varje dag eller varje vecka).

>[!CAUTION]
>
>När du implementerar säkerhetskopieringar av dina produktionsinstanser måste *test* utföras för att säkerställa att du kan återställa säkerhetskopian.
>
>Utan den här testningen är säkerhetskopian potentiellt oanvändbar (värsta scenariot).

>[!NOTE]
>
>Mer information om prestanda för säkerhetskopiering finns i avsnittet Prestanda [för säkerhetskopiering](/help/sites-deploying/configuring-performance.md#backup-performance) .

### Säkerhetskopiera din programvaruinstallation {#backing-up-your-software-installation}

Efter installationen, eller betydande ändringar i konfigurationen, skapar du en säkerhetskopia av din programvaruinstallation.

För att utföra den här uppgiften, [säkerhetskopiera hela lagringsplatsen](#backing-up-your-repository) och sedan:

1. Stoppa AEM.
1. Säkerhetskopiera hela `<cq-installation-dir>` från filsystemet.

>[!CAUTION]
>
>Om du använder en tredjepartsprogramserver kan ytterligare mappar finnas på en annan plats och måste säkerhetskopieras. Mer information om hur du installerar programservrar finns i [Installera AEM med en programserver](/help/sites-deploying/application-server-install.md).

>[!CAUTION]
>
>Inkrementell säkerhetskopiering av filens datalager stöds. När inkrementell säkerhetskopiering används för andra komponenter (som Lucene-index) måste du se till att borttagna filer även markeras som borttagna i säkerhetskopian.

>[!NOTE]
>
>Diskspegling kan också användas som en säkerhetskopieringsmekanism.

### Säkerhetskopiera din lagringsplats {#backing-up-your-repository}

Avsnittet [Säkerhetskopiering och återställning](/help/sites-administering/backup-and-restore.md) i CRX-dokumentationen täcker alla problem som rör säkerhetskopiering av CRX-databasen.

Mer information om hur du skapar en &quot;hot&quot;-säkerhetskopiering online finns i [Skapa en onlinesäkerhetskopiering](/help/sites-administering/backup-and-restore.md#online-backup).

## Rensning av version {#version-purging}

Verktyget **Rensa versioner** är avsett att rensa versioner av en nod eller en hierarki av noder i din databas. Dess främsta syfte är att hjälpa dig att minska storleken på databasen genom att ta bort tidigare versioner av dina noder.

I det här avsnittet behandlas underhållsåtgärder som rör versionsfunktionen i AEM. Verktyget **Rensa version** är avsett att rensa versioner av en nod eller en hierarki av noder i databasen. Dess primära syfte är att hjälpa dig att minska storleken på ditt arkiv genom att ta bort gamla versioner av dina noder.

### Överblick {#overview}

Verktyget **Rensa versioner** är tillgängligt som en underhållsåtgärd varje vecka. Innan du använder den för första gången måste den läggas till och sedan konfigureras. Därefter kan det köras på begäran eller varje vecka.

### Rensa versioner av en webbplats {#purging-versions-of-a-web-site}

Så här rensar du versioner av en webbplats:

1. Gå till **[Verktyg](/help/sites-administering/tools-consoles.md)** **konsolen**, välj **Åtgärd**, **Underhåll** och sedan **Veckounderhåll**.

1. Välj **+ Lägg till** i det övre verktygsfältet.

   ![Lägg till versionsrensning](assets/version-purge-add.png)

1. Välj **Rensa version** i listrutan i dialogrutan **Lägg till ny aktivitet** . **Spara**.

   ![Lägg till versionsrensning](assets/version-purge-add-new-task.png)

1. Aktiviteten **Rensa**-version har lagts till. Använd kortåtgärder för att:
   * Markera - visar ytterligare åtgärder i det övre verktygsfältet
   * Kör - för att köra den konfigurerade rensningen omedelbart
   * Konfigurera - för att konfigurera veckorensningsaktiviteten

   ![Rensningsåtgärder för version](assets/version-purge-actions.png)

1. Välj åtgärden **Konfigurera** för att öppna webbkonsolen för **Day CQ WCM Version Renge Task**, där du kan konfigurera:

   ![Konfiguration för versionsrensning](assets/version-purge-configuration.png)

   * **Rensa banor**
Ange startsökvägen för innehållet som ska rensas, till exempel `/content/wknd` .

     >[!CAUTION]
     >
     >Adobe rekommenderar att du definierar flera sökvägar för varje webbplats.
     >
     >Om du definierar en bana med för många underordnade objekt kan det ta lång tid att tömma den.

   * **Rensa versioner rekursivt**

      * Avmarkera alternativet om du bara vill rensa den nod som definieras av sökvägen.
      * Välj det här alternativet om du vill rensa noden som definieras av sökvägen och dess underordnade noder.

   * **Maximalt antal versioner**
Ange det maximala antalet versioner (för varje nod) som du vill behålla. Lämna tomt om du inte vill använda den här inställningen.

   * **Minsta antal versioner**
Ange det minsta antal versioner (för varje nod) som du vill behålla. Lämna tomt om du inte vill använda den här inställningen.

   * **Högsta versionsålder**
Ange den maximala versionsåldern i dagar (för varje nod) som du vill behålla. Lämna tomt om du inte vill använda den här inställningen.

   **Spara**.

1. Navigera/gå tillbaka till fönstret **Veckounderhåll** och välj **Kör** för att starta processen direkt.

>[!CAUTION]
>
>Du kan använda dialogrutan Klassiskt användargränssnitt för att utföra en [torr körning](#analyzing-the-console) av din konfiguration:
>
>* http://localhost:4502/etc/versioning/purge.html
>
>Det går inte att återställa rensade noder utan att återställa databasen. Ta hand om konfigurationen genom att alltid göra en torr körning innan du tömmer den.

#### Torr körning - Analyserar konsolen {#analyzing-the-console}

Det klassiska användargränssnittet innehåller ett **torrt körningsalternativ** från:

* http://localhost:4502/etc/versioning/purge.html

Processen visar alla noder som har bearbetats. Under processen kan en nod ha någon av följande statusar:

* `ignore (not versionnable)`: noden stöder inte versionshantering och ignoreras under processen.

* `ignore (no version)`: noden har ingen version och ignoreras under processen.

* `retained`: noden har inte rensats.
* `purged`: noden rensas.

Konsolen ger dessutom användbar information om versionerna:

* `V 1.0`: versionsnumret.
* `V 1.0.1`&#42;: Stjärnan anger att versionen är den aktuella (grundversionen) och inte kan rensas.

* `Thu Mar 15 2012 08:37:32 GMT+0100`: datumet för versionen.

I nästa exempel:

* Versionerna **[!DNL Shirts]** rensas eftersom deras versionsålder är längre än två dagar.
* Versionerna **[!DNL Tonga Fashions!]** rensas eftersom deras antal versioner är större än 5.

![global_version_screenshot](assets/global_version_screenshot.png)

## Arbeta med granskningsposter och loggfiler {#working-with-audit-records-and-log-files}

Granskningsposter och loggfiler för Adobe Experience Manager (AEM) finns på olika platser. Här följer en översikt över vad du kan hitta och var du kan hitta den.

### Arbeta med loggar {#working-with-logs}

AEM WCM registrerar detaljerade loggar. När du har packat upp och startat Quickstart hittar du loggar in:

* `<cq-installation-dir>/crx-quickstart/logs/`

* `<cq-installation-dir>/crx-quickstart/repository/`

#### Rotation av loggfil {#log-file-rotation}

Rotation av loggfiler avser den process som begränsar filens tillväxt genom att skapa en fil regelbundet. I AEM roteras en loggfil med namnet `error.log` en gång om dagen enligt följande regler:

* Filen `error.log` har fått ett nytt namn enligt mönstret `{original_filename}.yyyy-MM-dd`. Exempel: den 11 juli 2010 får den aktuella loggfilen namnet `error.log-2010-07-10` och sedan skapas en ny `error.log`.

* Tidigare loggfiler tas inte bort, så det är ditt ansvar att rensa gamla loggfiler med jämna mellanrum för att begränsa diskanvändningen.

>[!NOTE]
>
>Om du uppgraderar AEM finns alla befintliga loggfiler som inte längre används av AEM kvar på disken. Du kan ta bort dem utan risk. Alla nya loggposter skrivs i de nya loggfilerna.

### Hitta loggfilerna {#finding-the-log-files}

Olika loggfiler finns på den filserver där du installerade AEM:

* `<cq-installation-dir>/crx-quickstart/logs`

   * `access.log`
Alla åtkomstbegäranden till AEM WCM och databasen registreras här.

   * `audit.log`
Modereringsåtgärder registreras här.

   * `error.log`
Felmeddelanden (av varierande allvarlighetsgrad) registreras här.

   * [`ImageServer-<PortId>-yyyy>-<mm>-<dd>.log`](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/config-admin/server-logging/c-image-server-log.html)
Den här loggen används bara om [!DNL Dynamic Media] är aktiverat. Det innehåller statistik och analysinformation som används för att analysera beteendet i den interna ImageServer-processen.

   * `request.log`
Varje åtkomstbegäran registreras här tillsammans med svaret.

   * [`s7access-<yyyy>-<mm>-<dd>.log`](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/config-admin/server-logging/c-access-log.html)
Den här loggen används bara om [!DNL Dynamic Media] är aktiverat. I s7access-loggen registreras varje begäran som gjorts till [!DNL Dynamic Media] till `/is/image` och `/is/content`.

   * `stderr.log`
Innehåller felmeddelanden, återigen av varierande allvarlighetsgrad, som genereras under start. Som standard är loggnivån inställd på `Warning` ( `WARN`)

   * `stdout.log`
Innehåller loggningsmeddelanden som anger händelser under start.

   * `upgrade.log`
Innehåller en logg över alla uppgraderingsåtgärder som körs från `com.day.compat.codeupgrade` - och `com.adobe.cq.upgradesexecutor` -paketen.

* `<cq-installation-dir>/crx-quickstart/repository/segmentstore`

   * `journal.log`
Information om revideringsjournaler.

>[!NOTE]
>
>ImageServer- och s7access-loggarna ingår inte i **Download Full **paketet som genereras från sidan **system/console/status-Bundlelist **. Om du har [!DNL Dynamic Media] utgåvor av supporten lägger du till loggarna ImageServer och s7access när du kontaktar kundsupporten.

### Aktivera loggnivån för FELSÖKNING {#activating-the-debug-log-level}

Standardloggnivån ([Konfiguration för Apache Sling-loggning](/help/sites-deploying/osgi-configuration-settings.md#apacheslingloggingconfiguration)) är Information, så felsökningsmeddelanden loggas inte.

Om du vill aktivera felsökningsloggnivån för en loggare ställer du in egenskapen `org.apache.sling.commons.log.level` på att felsöka i databasen. På `/libs/sling/config/org.apache.sling.commons.log.LogManager` om du till exempel vill konfigurera [global Apache Sling-loggning](/help/sites-deploying/osgi-configuration-settings.md#apacheslingloggingconfiguration).

>[!CAUTION]
>
>Lämna inte loggen på felsökningsloggnivån längre än nödvändigt eftersom den genererar många loggposter och förbrukar resurser.

En rad i felsökningsfilen börjar oftast med DEBUG och anger sedan loggnivån, installationsåtgärden och loggmeddelandet. Till exempel:

```shell
DEBUG 3 WebApp Panel: WebApp successfully deployed
```

Loggnivåerna är följande:

| 0 | Allvarligt fel | Åtgärden misslyckades och installationsprogrammet kan inte fortsätta. |
|---|---|---|
| 1 | Fel | Åtgärden misslyckades. Installationen fortsätter, men en del av AEM WCM installerades inte korrekt och fungerar inte. |
| 2 | Varning | Åtgärden har slutförts men problem uppstod. AEM WCM fungerar eventuellt inte som det ska. |
| 3 | Information | Åtgärden har slutförts. |

### Skapa en anpassad loggfil {#create-a-custom-log-file}

>[!NOTE]
>
>När du arbetar med Adobe Experience Manager finns det flera metoder för att hantera konfigurationsinställningarna för sådana tjänster. Mer information och rekommenderade tillvägagångssätt finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

I vissa fall kanske du vill skapa en anpassad loggfil med en annan loggnivå. Gör följande i databasen:

1. Om den inte finns skapar du en konfigurationsmapp ( `sling:Folder`) för projektet `/apps/<project-name>/config`.
1. Under `/apps/<project-name>/config` skapar du en nod för den nya [loggningskonfigurationen för Apache Sling ](/help/sites-deploying/osgi-configuration-settings.md#apacheslingloggingloggerconfigurationfactoryconfiguration):

   * Namn: `org.apache.sling.commons.log.LogManager.factory.config-<identifier>`

     Där `<identifier>` ersätts av fritext som du (måste) ange för att identifiera instansen (du kan inte utelämna den här informationen).

     Exempel: `org.apache.sling.commons.log.LogManager.factory.config-MINE`

   * Typ: `sling:OsgiConfig`

   >[!NOTE]
   >
   >Även om det inte är ett tekniskt krav, är det lämpligt att göra `<identifier>` unikt.

1. Ange följande egenskaper för den här noden:

   * Namn: `org.apache.sling.commons.log.file`

     Typ: String

     Värde: ange loggfilen, till exempel `logs/myLogFile.log`

   * Namn: `org.apache.sling.commons.log.names`

     Typ: String[] (String + Multi)

     Värde: Ange de OSGi-tjänster som loggaren ska logga meddelanden för, till exempel allt av följande:

      * `org.apache.sling`
      * `org.apache.felix`
      * `com.day`

   * Namn: `org.apache.sling.commons.log.level`

     Typ: String

     Värde: ange den loggnivå som krävs ( `debug`, `info`, `warn` eller `error`), till exempel `debug`

   * Konfigurera de andra parametrarna efter behov:

      * Namn: `org.apache.sling.commons.log.pattern`

        Typ: `String`

        Värde: ange mönstret för loggmeddelandet efter behov. till exempel

        `{0,date,dd.MM.yyyy HH:mm:ss.SSS} *{4}* [{2}] {3} {5}`

   >[!NOTE]
   >
   >`org.apache.sling.commons.log.pattern` stöder upp till sex argument.
   >
   >{0} Tidsstämpeln av typen `java.util.Date`
   >
   >{1} loggmarkören
   >
   >{2} namnet på den aktuella tråden
   >
   >{3} namnet på loggen
   >
   >{4} loggnivån
   >
   >{5} loggmeddelandet
   >
   >Om logganropet innehåller en `Throwable` läggs stackspårningen till i meddelandet.

   >[!CAUTION]
   >
   >org.apache.sling.Commons.log.names måste ha ett värde.

   >[!NOTE]
   >
   >Loggskrivarsökvägarna är relativa till platsen `crx-quickstart`.
   >
   >Därför har en loggfil angetts som:
   >
   >`logs/thelog.log`
   >
   >skriver till:
   >
   >`<cq-installation-dir>/crx-quickstart/logs/thelog.log`.
   >
   >Och en loggfil har angetts som:
   >
   >`../logs/thelog.log`
   >
   >skriver till en katalog:
   >
   >`<cq-installation-dir>/logs/`\
   >(d.v.s. bredvid `<cq-installation-dir>/crx-quickstart/`)

1. Det här steget är bara nödvändigt när ett nytt skrivprogram krävs (d.v.s. med en annan konfiguration än standardskrivaren).

   >[!CAUTION]
   >
   >En ny loggningsskrivarkonfiguration krävs bara när den befintliga standardinställningen inte är lämplig.
   >
   >Om inget explicit skrivprogram är konfigurerat genereras automatiskt ett implicit skrivprogram baserat på standardvärdet.

   Under `/apps/<project-name>/config` skapar du en nod för den nya [konfigurationen för Apache Sling Logging Writer ](/help/sites-deploying/osgi-configuration-settings.md#apacheslingloggingwriterconfigurationfactoryconfiguration):

   * Namn: `org.apache.sling.commons.log.LogManager.factory.writer-<identifier>` (skrivprogram)

     Precis som med loggen ersätts `<identifier>` av fri text som du (måste) anger för att identifiera instansen (du kan inte utelämna den här informationen). Exempel: `org.apache.sling.commons.log.LogManager.factory.writer-MINE`

   * Typ: `sling:OsgiConfig`

   >[!NOTE]
   >
   >Även om det inte är ett tekniskt krav är det tillrådligt att göra `<identifier>` unik.

   Ange följande egenskaper för den här noden:

   * Namn: `org.apache.sling.commons.log.file`

     Typ: `String`

     Värde: ange loggfilen så att den matchar den fil som anges i loggboken.

     i det här exemplet: `../logs/myLogFile.log`.

   * Konfigurera de andra parametrarna efter behov:

      * Namn: `org.apache.sling.commons.log.file.number`

        Typ: `Long`

        Värde: ange antalet loggfiler som du vill behålla, till exempel `5`

      * Namn: `org.apache.sling.commons.log.file.size`

        Typ: `String`

        Värde: Ange som krävs för att styra filrotation efter storlek/datum, till exempel `'.'yyyy-MM-dd`

   >[!NOTE]
   >
   >`org.apache.sling.commons.log.file.size` styr rotationen av loggfilen genom att ange antingen:
   >
   >* maximal filstorlek
   >* ett tids-/datumschema
   >
   >för att ange när en ny fil skapas (och den befintliga filen får ett nytt namn enligt namnmönstret).
   >
   >* En storleksgräns kan anges med ett tal. Om ingen storleksindikator anges används den som antal byte, eller så kan du lägga till en av storleksindikatorerna - `KB`, `MB` eller `GB` (versaler ignoreras).
   >* Ett tids-/datumschema kan anges som ett `java.util.SimpleDateFormat` mönster. Den definierar den tidsperiod efter vilken filen roteras. Dessutom läggs suffixet till i den roterade filen (för identifiering).
   >
   >Standardvärdet är &#39;.&#39;yyyy-MM-dd (för daglig loggrotation).
   >
   >Till exempel vid midnatt den 20 januari 2010 (eller när det första loggmeddelandet efter detta datum inträffar för att vara exakt), .. /logs/error.log byter namn till .. /logs/error.log.2010-01-20. Loggning för 21 januari matas ut till (en ny och tom) .. /logs/error.log tills den rullas över vid nästa dagbyte.
   >
   >| `'.'yyyy-MM` | Rotation i början av varje månad |
   >|---|---|
   >| `'.'yyyy-ww` | Rotation på den första dagen i varje vecka (beror på språkområdet). |
   >| `'.'yyyy-MM-dd` | Rotation vid midnatt varje dag. |
   >| `'.'yyyy-MM-dd-a` | Rotation vid midnatt och middag varje dag. |
   >| `'.'yyyy-MM-dd-HH` | Rotation överst varje timme. |
   >| `'.'yyyy-MM-dd-HH-mm` | Rotation i början av varje minut. |
   >
   >Obs! När du anger tid/datum:
   >
   >1. Du bör&quot;escape&quot;-text inom ett par enkla citattecken (&#39; &#39;);
   >
   >    Undviker att vissa tecken tolkas som mönsterbokstäver.
   >
   >1. Använd bara tecken som är tillåtna för ett giltigt filnamn var som helst i alternativet.

1. Läs den nya loggfilen med det verktyg du valt.

   Loggfilen som skapas i det här exemplet är `../crx-quickstart/logs/myLogFile.log`.

Felix Console innehåller även information om stöd för delningsloggar på `../system/console/slinglog`, till exempel `https://localhost:4502/system/console/slinglog`.

### Söka efter granskningsposter {#finding-the-audit-records}

Granskningsregister förs för att visa vem som gjorde vad och när. Olika granskningsposter genereras för både AEM WCM- och OSGi-händelser.

#### AEM WCM-granskningsposter visas vid sidredigering {#aem-wcm-audit-records-shown-when-page-authoring}

1. Öppna en sida.
1. I sidosparken kan du välja fliken med låsikonen och sedan dubbelklicka på **Granskningslogg..**.
1. Ett nytt fönster öppnas med en lista över granskningsposter för den aktuella sidan.

   ![screen_shot_2012-02-02at43601pm](assets/screen_shot_2012-02-02at43601pm.png)

1. Klicka på **OK** när du vill stänga fönstret.

#### AEM WCM-granskningsposter i databasen {#aem-wcm-auditing-records-within-the-repository}

I mappen `/var/audit` hålls granskningsposter enligt resursen. Du kan gå nedåt tills du ser enskilda poster och den information de innehåller.

Dessa poster innehåller samma information som den som visas när du redigerar en sida.

#### OSGi Granskningsposter från webbkonsolen {#osgi-audit-records-from-the-web-console}

OSGi-händelser genererar också granskningsposter som kan visas på fliken **Konfigurationsstatus** > **Loggfiler** på AEM webbkonsol:

![screen_shot_2012-02-13at50346pm](assets/screen_shot_2012-02-13at50346pm.png)

## Övervaka replikeringsagenter {#monitoring-your-replication-agents}

Du kan övervaka dina [replikeringsköer](/help/sites-deploying/replication.md) för att identifiera när en kö är avstängd eller blockerad, vilket i sin tur kan tyda på ett problem med en publiceringsinstans eller ett externt system:

* Är alla obligatoriska köer aktiverade?
* Krävs det fortfarande inaktiverade köer?
* alla `enabled`-köer ska ha statusen `idle` eller `active`, vilket anger normal åtgärd. Inga köer ska vara `blocked`, vilket ofta är ett tecken på problem på mottagarsidan.

* om storleken på kön ökar över tid kan det indikera en blockerad kö.

Så här övervakar du en replikeringsagent:

1. Gå till fliken **Verktyg** i AEM.
1. Klicka på **Replication**.
1. Dubbelklicka på länken till agenter för lämplig miljö (antingen vänster eller höger ruta), till exempel **Agenter på författaren**.

   I det resulterande fönstret visas en översikt över alla dina replikeringsagenter för redigeringsmiljön, inklusive mål och status.

1. Klicka på lämpligt agentnamn (som är en länk) för att visa detaljerad information om agenten:

   ![chlimage_1](assets/chlimage_1.jpeg)

   Här kan du:

   * Se om agenten är aktiverad.
   * Se målet för alla replikeringar.
   * Kontrollera om replikeringskön är aktiv (aktiverad).
   * Se om det finns några objekt i kön.
   * **Uppdatera** eller **Rensa** om du vill uppdatera visningen av köposter. Om du gör det blir det lättare att se objekt som kommer in i och lämnar kön.
   * **Visa logg** om du vill komma åt loggen för eventuella åtgärder från replikeringsagenten.
   * **Testa anslutning** till målinstansen.
   * **Tvinga nya försök** för alla köobjekt om det behövs.

   >[!CAUTION]
   >
   >Använd inte länken Testa anslutning för den omvända replikeringsutkorgen på en publiceringsinstans.
   >
   >Om ett replikeringstest utförs för en Utkorgskö bearbetas alla objekt som är äldre än testreplikeringen om med varje omvänd replikering.
   >
   >Om sådana objekt finns i en kö kan de hittas med följande XPath JCR-fråga och bör tas bort.
   >
   >`/jcr:root/var/replication/outbox//*[@cq:repActionType='TEST']`

Även här kan du utveckla en lösning för att identifiera alla replikeringsagenter (som finns under `/etc/replication/author` eller `/etc/replication/publish`) och sedan kontrollera status för agenten ( `enabled`, `disabled`) och den underliggande kön ( `active`, `idle`, `blocked`).

## Övervakningsprestanda {#monitoring-performance}

[Prestandaoptimering](/help/sites-deploying/configuring-performance.md) är en interaktiv process som får fokus under utvecklingen. Efter distributionen granskas den efter specifika intervall eller händelser.

Metoder som används för att samla in information för optimering kan också användas för kontinuerlig övervakning.

>[!NOTE]
>
>Specifika [konfigurationer som är tillgängliga för att förbättra prestanda](/help/sites-deploying/configuring-performance.md#configuring-for-performance) kan också kontrolleras.

Nedan visas vanliga prestandaproblem som uppstår, tillsammans med förslag om hur du kan hitta och motverka dem.

| Område | Symptom | Öka kapaciteten.. | Minska volymen... |
|---|---|---|---|
| Klient | Användning av CPU med hög kundnivå. | Installera en klient-CPU med högre prestanda. | Förenkla (HTML) layouten. |
|   | Låg användning av CPU på servern. | Uppgradera till en snabbare webbläsare. | Förbättra cacheminnet på klientsidan. |
|   | Vissa kunder är snabba, vissa långsamma. |  |  |
| Server |  |  |  |
| Nätverk | CPU har låg användning både på servrar och klienter. | Ta bort eventuella flaskhalsar i nätverket. | Förbättra/optimera konfigurationen av klientcachen. |
|   | Det går snabbt att bläddra lokalt på servern (relativt). | Öka nätverkets bandbredd. | Minska&quot;vikten&quot; på dina webbsidor (till exempel färre bilder, optimerad HTML). |
| Webbserver | CPU på webbservern är hög. | Klustra dina webbservrar. | Minska antalet träffar per sida (besök). |
|   |  | Använd en maskinvarubaserad belastningsutjämnare. |  |
| Program | Serverns CPU-användning är hög. | Klustra dina AEM-instanser. | Sök efter, och ta bort, CPU och minnesgropar (använd kodgranskning och tidsutdata). |
|   | Hög minnesförbrukning. |  | Förbättra cachelagringen på alla nivåer. |
|   | Låga svarstider. |  | Optimera mallar och komponenter (till exempel struktur, logik). |
| Databas |  |  |  |
| Cache |  |  |  |

Prestandaproblem kan bero på olika orsaker som inte har något med din webbplats att göra, bland annat tillfälliga avbrottstakten i anslutningshastigheten, CPU-belastningen och många andra.

Det kan även påverka alla besökare eller bara en del av dem.

All denna information måste hämtas, sorteras och analyseras innan du kan optimera den allmänna prestandan eller lösa specifika problem.

* Innan du upplever ett prestandaproblem:

   * Samla in så mycket information som möjligt för att bygga upp goda kunskaper om systemet under normala förhållanden

* När du får ett prestandaproblem:

   * försök att replikera den med en (eller helst med flera) standardwebbläsare, på en annan klient som du vet har bra allmänna prestanda och/eller på själva servern (om möjligt)
   * kontrollera om något (relaterat till systemet) har ändrats inom en lämplig tidsperiod och om någon av dessa ändringar kan ha påverkat prestandan
   * ställa frågor som:

      * inträffar problemet endast vid vissa tidpunkter?
      * uppstår problemet endast på vissa sidor?
      * Påverkas andra förfrågningar?

   * Samla in så mycket information som möjligt för att jämföra med dina kunskaper om systemet under normala förhållanden:

### Verktyg för övervakning och analys av prestanda {#tools-for-monitoring-and-analyzing-performance}

Nedan ges en kort översikt över några av de verktyg som är tillgängliga för övervakning och analys av prestanda.

Vissa av dessa verktyg är beroende av operativsystemet.

<table>
 <tbody>
  <tr>
   <td>Verktyg</td>
   <td>Används för att analysera..</td>
   <td>Användning/Mer information..</td>
  </tr>
  <tr>
   <td>request.log</td>
   <td>Svarstider och samtidighet.</td>
   <td><a href="#interpreting-the-request-log">Tolkar request.log</a>.</td>
  </tr>
  <tr>
   <td>truss/strace</td>
   <td>Sidinläsning</td>
   <td><p>Unix/Linux-kommandon för att spåra systemanrop och signaler. Öka loggnivån till <code>INFO</code>.</p> <p>Analysera antalet sidinläsningar per begäran och vilka sidor.</p> </td>
  </tr>
  <tr>
   <td>Tråddumpar</td>
   <td>Observera JVM-trådar. Identifiera innehåll, lås och långa löptider.</td>
   <td><p>Beroende på operativsystemet:<br /> - Unix/Linux: <code>kill -QUIT &lt;<em>pid</em>&gt;</code><br /> - Windows (konsolläge): Ctrl-Break<br /> </p> <p>Det finns även analysverktyg tillgängliga, till exempel <a href="https://github.com/irockel/tda">TDA</a>.<br /> </p> </td>
  </tr>
  <tr>
   <td>Heap Dumps</td>
   <td>Slut på minne som orsakar långsamma prestanda.</td>
   <td><p>Lägg till alternativet:<br /> <code>-XX:+HeapDumpOnOutOfMemoryError</code><br /> i Java™-anropet som går till AEM.</p> <p>Se <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/prepapp002.html#CEGBHDFH">Alternativ/flaggor för JVM-felsökningssidan</a>.</p> </td>
  </tr>
  <tr>
   <td>Systemanrop</td>
   <td>Identifiera timingproblem.</td>
   <td><p>Anrop till <code>System.currentTimeMillis()</code> eller <code>com.day.util</code>. Tidsinställningen används för att generera tidsstämplar från din kod eller genom <a href="#html-comments">HTML-kommentarer</a>.</p> <p><strong>Obs!</strong> Implementera de här sakerna så att de kan aktiveras/inaktiveras efter behov. När ett system körs smidigt behövs inte den allmänna kostnaden för att samla in statistik.</p> </td>
  </tr>
  <tr>
   <td>Apache Bench</td>
   <td>Identifiera minnesläckor och analysera responstiden selektivt.</td>
   <td><p>grundanvändningen är:</p> <p><code>ab -k -n &lt;<em>requests</em>&gt; -c &lt;<em>concurrency</em>&gt; &lt;<em>url</em>&gt;</code></p> <p>Mer information finns på <a href="#apache-bench">Apache Bench</a> och <a href="https://httpd.apache.org/docs/2.4/programs/ab.html">ab man page</a>.</p> </td>
  </tr>
  <tr>
   <td>Sökanalys</td>
   <td> </td>
   <td>Kör sökfrågor offline, identifiera svarstid för fråga, testa och bekräfta resultatuppsättning.<br /> </td>
  </tr>
  <tr>
   <td>JMeter</td>
   <td>Belastnings- och funktionstester.</td>
   <td><a href="https://jmeter.apache.org/">https://jmeter.apache.org/</a></td>
  </tr>
  <tr>
   <td>JProfiler</td>
   <td>Detaljinfo om CPU och minnesprofilering.</td>
   <td><a href="https://www.ej-technologies.com/">https://www.ej-technologies.com/</a></td>
  </tr>
  <tr>
   <td>Java™ Flight Recorder</td>
   <td>Java™ Flight Recorder (JFR) är ett verktyg för att samla in diagnostik- och profileringsdata om ett Java™-program som körs.</td>
   <td><a href="https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr004.html#BABJJEEE">https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr004.html#BABJJEEE</a></td>
  </tr>
  <tr>
   <td>JConsole</td>
   <td>Observera JVM-statistik och trådar.</td>
   <td><p>Syntax: jconsole</p> <p>Se <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/management/jconsole.html">jconsole</a> och <a href="#monitoring-performance-using-jconsole">Övervaka prestanda med JConsole</a>.</p> <p><strong>Obs!</strong> Med JDK 1.8 kan JConsole utökas med plugin-program, till exempel Top eller TDA (Thread Dump Analyzer).</p> </td>
  </tr>
  <tr>
   <td>Java™ VisualVM</td>
   <td>Observera JVM-statistik, trådar, minne och profilering.</td>
   <td><p>Syntax: visualvm eller visualvm<br /> </p> <p>Se <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/visualvm/">visuelltVm</a> och <a href="#monitoring-performance-using-j-visualvm">Övervaka prestanda med (J)VisualVM</a>.</p> <p><strong>Obs!</strong> Med JDK 1.8 kan VisualVM utökas med plugin-program. VisualVM avbryts efter JDK 9. Använd Java™ Flight Recorder istället.</p> </td>
  </tr>
  <tr>
   <td>truss/strace, lsof</td>
   <td>Ingående kernelanrop och processanalys (UNIX®).</td>
   <td>Unix/Linux-kommandon.</td>
  </tr>
  <tr>
   <td>Timingstatistik</td>
   <td>Se timingstatistik för sidåtergivning.</td>
   <td><p>Om du vill se tidsstatistik för sidåtergivning kan du använda <strong>Ctrl-Skift-U</strong> tillsammans med <code>?debugClientLibs=true</code> som anges i URL:en.</p> </td>
  </tr>
  <tr>
   <td>CPU och minnesprofileringsverktyget <br /> </td>
   <td><a href="#interpreting-the-request-log">Används vid analys av långsamma begäranden under utveckling</a>.</td>
   <td>Exempel: <a href="https://www.yourkit.com/">Ditt paket</a>. eller <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr004.html#BABJJEEE">Java™ Flight Recorder</a>.</td>
  </tr>
  <tr>
   <td><a href="#information-collection">Informationsinsamling</a></td>
   <td>Installationens aktuella tillstånd.</td>
   <td>Om du vet så mycket som möjligt om din installation kan det också hjälpa dig att spåra vad som kan ha orsakat prestandaförändringar och om dessa ändringar är motiverade. Samla in dessa mätvärden med jämna mellanrum så att du enkelt kan se betydande ändringar.</td>
  </tr>
 </tbody>
</table>

### Tolka request.log {#interpreting-the-request-log}

Den här filen registrerar grundläggande information om alla förfrågningar som görs till AEM. Därigenom kan värdefulla slutsatser dras.

`request.log` erbjuder ett inbyggt sätt att se hur lång tid det tar att begära. I utvecklingssyfte är det användbart att `tail -f` `request.log` och hålla utkik efter långsamma svarstider. Om du vill analysera ett större `request.log` rekommenderar Adobe [användning av `rlog.jar`, vilket gör att du kan sortera och filtrera efter svarstider ](#using-rlog-jar-to-find-requests-with-long-duration-times).

Adobe rekommenderar att du isolerar de &quot;långsamma&quot; sidorna från `request.log` och sedan justerar dem individuellt för att få bättre prestanda. Inkludera prestandamått per komponent eller använd ett prestandaprofileringsverktyg som ` [yourkit](https://www.yourkit.com/)`.

#### Övervaka trafik på din webbplats {#monitoring-traffic-on-your-website}

Begärandeloggen registrerar varje begäran som gjorts, tillsammans med svaret:

```xml
09:43:41 [66] -> GET /author/y.html HTTP/1.1
09:43:41 [66] <- 200 text/html 797ms
```

Genom att summera alla GET-poster inom specifika perioder (t.ex. under olika 24-timmarsperioder) kan du göra utdrag om den genomsnittliga trafiken på din webbplats.

#### Övervaka svarstider med request.log {#monitoring-response-times-with-the-request-log}

En bra utgångspunkt för prestandaanalys är begärandeloggen:

`<cq-installation-dir>/crx-quickstart/logs/request.log`

Loggen ser ut så här (raderna förkortas för enkelhetens skull):

```xml
31/Mar/2009:11:32:57 +0200 [379] -> GET /path/x HTTP/1.1
31/Mar/2009:11:32:57 +0200 [379] <- 200 text/html 33ms
31/Mar/2009:11:33:17 +0200 [380] -> GET /path/y HTTP/1.1
31/Mar/2009:11:33:17 +0200 [380] <- 200 application/json 39ms
```

Den här loggen har en rad per begäran eller svar:

* Det datum då varje begäran eller svar gjordes.
* Numret på begäran, inom hakparentes. Numret matchar för begäran och svaret.
* En pil som anger om det är en begäran (pil som pekar till höger) eller ett svar (pilen till vänster).
* För begäranden innehåller raden:

   * metoden (vanligtvis GET, HEAD eller POST)
   * begärd sida
   * protokollet

* För svar innehåller raden:

   * statuskoden (200 betyder &quot;lyckades&quot;, 404 betyder &quot;sidan hittades inte&quot;
   * MIME-typen
   * svarstid

Med små skript kan du extrahera den information som behövs från loggfilen och sammanställa den statistik som du vill ha. Med hjälp av den här statistiken kan du se vilka sidor eller typer av sidor som är långsamma och om de totala prestandan är tillfredsställande.

#### Övervaka söksvarstider med request.log {#monitoring-search-response-times-with-the-request-log}

Sökbegäranden registreras också i loggfilen:

```xml
31/Mar/2009:11:35:34 +0200 [338] -> GET /author/playground/en/tools/search.html?query=dilbert&size=5&dispenc=utf-8 HTTP/1.1
31/Mar/2009:11:35:34 +0200 [338] <- 200 text/html 1562ms
```

Så som ovan kan du använda skript för att extrahera relevant information och bygga upp statistik.

När du har bestämt svarstiden kan du analysera varför begäran tar den tid som behövs och vad som kan göras för att förbättra svaret.

#### Övervaka antalet samtidiga användare och deras inverkan {#monitoring-the-number-and-impact-of-concurrent-users}

Återigen kan `request.log` användas för att övervaka samtidighet och systemets reaktion på den.

Testerna måste göras för att avgöra hur många samtidiga användare som systemet kan hantera innan en negativ påverkan ses. Återigen kan du använda skript för att extrahera resultat från loggfilen:

* övervaka hur många förfrågningar som görs inom en viss tidsperiod, t.ex. en minut.
* testa effekterna av ett visst antal användare som alla gör samma förfrågningar samtidigt (så nära som möjligt). Till exempel klickar 30 användare på **Spara** samtidigt.

```xml
31/Mar/2009:11:45:29 +0200 [333] -> GET /author/libs/Personalize/content/statics.close.gif HTTP/1.1
31/Mar/2009:11:45:29 +0200 [334] -> GET /author/libs/Personalize/content/statics.detach.gif HTTP/1.1
31/Mar/2009:11:45:30 +0200 [335] -> GET /author/libs/CFC/content/imgs/logo.rZMNURccynWcTpCxyuBNiTCoiBMmw000.default.gif HTTP/1.1
31/Mar/2009:11:45:32 +0200 [335] <- 304 text/html 0ms
31/Mar/2009:11:45:33 +0200 [334] <- 200 image/gif 31ms
31/Mar/2009:11:45:38 +0200 [333] <- 200 image/gif 31ms
31/Mar/2009:11:45:42 +0200 [336] -> GET /author/libs/CFC/content/imgs/logo.rZMNURccynWcTZRXunQbbQtvuuCMbRRBuWXz0000.default.gif HTTP/1.1
31/Mar/2009:11:45:43 +0200 [337] -> GET /author/titlebar_bg.gif HTTP/1.1
31/Mar/2009:11:45:43 +0200 [336] <- 304 text/html 0ms
31/Mar/2009:11:45:44 +0200 [337] <- 304 text/html 0ms
```

### Använda rlog.jar för att hitta begäranden med lång varaktighet {#using-rlog-jar-to-find-requests-with-long-duration-times}

AEM innehåller olika hjälpverktyg:
`<cq-installation-dir>/crx-quickstart/opt/helpers`

Ett av dessa verktyg, `rlog.jar`, kan användas för att snabbt sortera `request.log` så att begäranden visas med varaktighet, från längsta till kortaste tid.

Följande kommando visar möjliga argument:

```shell
$java -jar rlog.jar
Request Log Analyzer Version 21584 Copyright 2005 Day Management AG
Usage:
  java -jar rlog.jar [options] <filename>
Options:
  -h               Prints this usage.
  -n <maxResults>  Limits output to <maxResults> lines.
  -m <maxRequests> Limits input to <maxRequest> requests.
  -xdev            Exclude POST request to CRXDE.
```

Du kan till exempel köra den och ange filen `request.log` som en parameter och visa de tio första begäranden som har längst varaktighet:

```shell
$ java -jar ../opt/helpers/rlog.jar -n 10 request.log
*Info * Parsed 464 requests.
*Info * Time for parsing: 22ms
*Info * Time for sorting: 2ms
*Info * Total Memory: 1mb
*Info * Free Memory: 1mb
*Info * Used Memory: 0mb
------------------------------------------------------
     18051ms 31/Mar/2009:11:15:34 +0200 200 GET /content/geometrixx/en/company.html text/ html
      2198ms 31/Mar/2009:11:15:20 +0200 200 GET /libs/cq/widgets.js application/x-javascript
      1981ms 31/Mar/2009:11:15:11 +0200 200 GET /libs/wcm/content/welcome.html text/html
      1973ms 31/Mar/2009:11:15:52 +0200 200 GET /content/campaigns/geometrixx.teasers..html text/html
      1883ms 31/Mar/2009:11:15:20 +0200 200 GET /libs/security/cq-security.js application/x-javascript
      1876ms 31/Mar/2009:11:15:20 +0200 200 GET /libs/tagging/widgets.js application/x-javascript
      1869ms 31/Mar/2009:11:15:20 +0200 200 GET /libs/tagging/widgets/themes/default.js application/x-javascript
      1729ms 30/Mar/2009:16:45:56 +0200 200 GET /libs/wcm/content/welcome.html text/html; charset=utf-8
      1510ms 31/Mar/2009:11:15:34 +0200 200 GET /bin/wcm/contentfinder/asset/view.json/ content/dam?_dc=1238490934657&query=&mimeType=image&_charset_=utf-8 application/json
      1462ms 30/Mar/2009:17:23:08 +0200 200 GET /libs/wcm/content/welcome.html text/html; charset=utf-8
```

Sammanfoga de enskilda `request.log`-filerna om du måste utföra den här åtgärden på ett stort dataexempel.

### Apache Bench {#apache-bench}

För att minimera effekten av specialfall (till exempel skräpinsamling) rekommenderar vi att du använder ett verktyg som `apachebench` (till exempel [ab](https://httpd.apache.org/docs/2.4/programs/ab.html) för ytterligare dokumentation) för att identifiera minnesläckor och selektivt analysera svarstiden.

Apache Bench kan användas på följande sätt:

```shell
$ ab -c 5 -k -n 1000 "https://localhost:4503/content/geometrixx/en/company.html"
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, https://www.zeustech.net/
Licensed to The Apache Software Foundation, https://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests

Server Software: Day-Servlet-Engine/4.1.52
Server Hostname: localhost
Server Port: 4503

Document Path: /content/geometrixx/en/company.html
Document Length: 24127 bytes

Concurrency Level: 5
Time taken for tests: 69.766 seconds
Complete requests: 1000
Failed requests: 998
(Connect: 0, Receive: 0, Length: 998, Exceptions: 0)
Write errors: 0
Keep-Alive requests: 0
Total transferred: 24160923 bytes
HTML transferred: 24010923 bytes
Requests per second: 14.33 /sec (mean)
Time per request: 348.828 [ms] (mean)
Time per request: 69.766 [ms] (mean, across all concurrent requests)
Transfer rate: 338.20 [Kbytes/sec] received

Connection Times (ms)
min mean[+/-sd] median max
Connect: 0 1 3.9 0 58
Processing: 138 347 568.5 282 8106
Waiting: 137 344 568.1 281 8106
Total: 139 348 568.4 283 8106

Percentage of the requests served within a certain time (ms)
50% 283
66% 323
75% 356
80% 374
90% 439
95% 512
98% 1047
99% 1132
100% 8106 (longest request)
```

Siffrorna ovan hämtas från en vanlig bärbar dator från MAcBook Pro (mitten av 2010) som har åtkomst till Geometrixx företagssida, vilket ingår i en AEM standardinstallation. Sidan är enkel, men inte optimerad för prestanda.

`apachebench` visar också tiden per begäran som medelvärde, för alla samtidiga begäranden, se `Time per request: 54.595 [ms]` (medelvärde, för alla samtidiga begäranden). Du kan ändra värdet på parametern för samtidig användning `-c` (antalet flera begäranden som ska utföras samtidigt) för att se eventuella effekter.

### Begäranräknare {#request-counters}

Information om begärandetrafik (antal förfrågningar under en viss tidsperiod) ger dig en indikation på belastningen på din instans. Den här informationen kan extraheras från [request.log](#interpreting-the-request-log), men med räknare automatiseras datainsamlingen så att du kan se:

* betydande skillnader i aktivitet (det vill säga skillnader mellan&quot;många förfrågningar&quot; och&quot;låg aktivitet&quot;)
* när en instans inte används
* alla omstarter (räknarna återställs till 0)

Om du vill automatisera informationsinsamlingen kan du även installera ett RequestFilter för att öka antalet räknare vid varje begäran. Flera räknare kan användas för olika tidsperioder.

De insamlade uppgifterna kan användas för att ange

* betydande förändringar i verksamheten
* en redundant instans
* alla omstarter (räknaren återställs till 0)

### HTML Comments {#html-comments}

Vi rekommenderar att alla projekt innehåller `html comments` för serverprestanda. Många bra exempel finns. Välj en sida, öppna sidkällan för visning och rulla längst ned. Kod som följande kan ses:

```xml
</body>
 </html>
        <!--
        Page took 58 milliseconds to be rendered by server
         -->
```

### Övervaka prestanda med JConsole {#monitoring-performance-using-jconsole}

Verktygskommandot `jconsole` är tillgängligt med JDK.

1. Starta din AEM-instans.
1. Kör `jconsole.`
1. Markera din AEM-instans och **Anslut**.

1. Dubbelklicka på `com.day.crx.quickstart.Main` i programmet `Local`. Översikten visas som standard:

   ![chlimage_1-1](assets/chlimage_1-1.png)

   Nu kan du välja andra alternativ.

### Övervaka prestanda med hjälp av (J)VisualVM {#monitoring-performance-using-j-visualvm}

För JDK 6-8 är verktygskommandot `visualvm` tillgängligt. När du har installerat en JDK kan du göra följande:

1. Starta din AEM-instans.

   >[!NOTE]
   >
   >Om du använder Java™ 5 kan du lägga till argumentet `-Dcom.sun.management.jmxremote` i Java™-kommandoraden som startar din JVM. JMX är aktiverat som standard med Java™ 6.

1. Kör antingen:

   * `jvisualvm`: i mappen JDK 1.6 bin (testversion)
   * `visualvm`: kan hämtas från [VisualVM](https://docs.oracle.com/javase/8/docs/technotes/guides/visualvm/) (version med avblodningskant)

1. Dubbelklicka på `com.day.crx.quickstart.Main` inifrån programmet `Local`. Översikten visas som standard:

   ![chlimage_1-2](assets/chlimage_1-2.png)

   Nu kan du välja andra alternativ, bland annat Bildskärm:

   ![chlimage_1-3](assets/chlimage_1-3.png)

Du kan använda det här verktyget för att generera tråddumpar och minnesdumpar. Den här informationen begärs ofta av den tekniska supportteamet.

### Informationsinsamling {#information-collection}

Om du vet så mycket som möjligt om din installation kan det hjälpa dig att spåra vad som kan ha orsakat prestandaförändringar och om dessa ändringar är motiverade. Samla in dessa mätvärden med jämna mellanrum så att du enkelt kan se betydande ändringar.

Följande information kan vara användbar:

* [Hur många författare arbetar med systemet?](#how-many-authors-are-working-with-the-system)
* [Vilket är det genomsnittliga antalet sidaktiveringar per dag?](#what-is-the-average-number-of-page-activations-per-day)
* [Hur många sidor har du i det här systemet?](#how-many-pages-do-you-currently-maintain-on-this-system)
* [Om du använder MSM, vilket är det genomsnittliga antalet utrullningar per månad?](#if-you-use-msm-what-is-the-average-number-of-rollouts-per-month)
* [Vilket är det genomsnittliga antalet Live-kopior per månad?](#what-is-the-average-number-of-live-copies-per-month)
* [Om du använder AEM Assets, hur många mediefiler har du för närvarande i Assets?](#ifyouusecqdamhowmanyassetsdoyoucurrentlymaintainincqdam)
* [Vilken är den genomsnittliga storleken på resurserna?](#what-is-the-average-size-of-the-assets)
* [Hur många mallar används för närvarande?](#how-many-templates-are-currently-used)
* [Hur många komponenter används för närvarande?](#how-many-components-are-currently-used)
* [Hur många förfrågningar per timme har du på författarsystemet vid maximal tid?](#how-many-requests-per-hour-do-you-have-on-the-author-system-at-peak-time)
* [Hur många begäranden per timme har du i publiceringssystemet vid maximal tid?](#how-many-requests-per-hour-do-you-have-on-the-publish-system-at-peak-time)

#### Hur många författare arbetar med systemet? {#how-many-authors-are-working-with-the-system}

Använd kommandoraden för att se hur många författare som har använt systemet sedan installationen:

```shell
cd <cq-installation-dir>/crx-quickstart/logs
cut -d " " -f 3 access.log | sort -u | wc -l
```

Så här ser du hur många författare som arbetar ett visst datum:

```shell
grep "<date>" access.log | cut -d " " -f 3 | sort -u | wc -l
```

#### Vilket är det genomsnittliga antalet sidaktiveringar per dag? {#what-is-the-average-number-of-page-activations-per-day}

Om du vill se det totala antalet sidaktiveringar sedan serverinstallationen använder du en databasfråga. Med CRXDE - Verktyg - Fråga:

* **Typ** `XPath`

* **Sökväg** `/`

* **Fråga** `//element(*, cq:AuditEvent)[@cq:type='Activate']`

Beräkna sedan medelvärdet genom att beräkna antalet dagar som har gått sedan installationen.

#### Hur många sidor har du i det här systemet? {#how-many-pages-do-you-currently-maintain-on-this-system}

Om du vill se antalet sidor som finns på servern använder du en databasfråga, via CRXDE - Verktyg - Fråga:

* **Typ** `XPath`

* **Sökväg** `/`

* **Fråga** `//element(*, cq:Page)`

#### Om du använder MSM, vilket är det genomsnittliga antalet utrullningar per månad? {#if-you-use-msm-what-is-the-average-number-of-rollouts-per-month}

Använd en databasfråga för att fastställa det totala antalet rollouts sedan installationen. Med CRXDE - Verktyg - Fråga:

* **Typ** `XPath`

* **Sökväg** `/`

* **Fråga** `//element(*, cq:AuditEvent)[@cq:type='PageRolledOut']`

Beräkna medelvärdet genom att beräkna antalet månader som har gått sedan installationen.

#### Vilket är det genomsnittliga antalet Live-kopior per månad? {#what-is-the-average-number-of-live-copies-per-month}

Om du vill fastställa det totala antalet live-kopior som har gjorts sedan installationen använder du en databasfråga, via CRXDE - Verktyg - Fråga:

* **Typ** `XPath`

* **Sökväg** `/`

* **Fråga** `//element(*, cq:LiveSyncConfig)`

Använd återigen antalet månader som har gått sedan installationen för att beräkna genomsnittet.

#### Om du använder AEM Assets, hur många mediefiler har du för närvarande i Assets? {#if-you-use-aem-assets-how-many-assets-do-you-currently-maintain-in-assets}

Använd en databasfråga, via CRXDE - Verktyg - Fråga, om du vill se hur många DAM-resurser du för närvarande har:

* **Typ** `XPath`
* **Sökväg** `/`
* **Fråga** `/jcr:root/content/dam//element(*, dam:Asset)`

#### Vilken är den genomsnittliga storleken på resurserna? {#what-is-the-average-size-of-the-assets}

Så här fastställer du mappens `/var/dam` totala storlek:

1. Använd WebDAV för att mappa databasen till det lokala filsystemet.

1. Använd kommandoraden:

   ```shell
   cd /Volumes/localhost/var
   du -sh dam/
   ```

   Om du vill få fram den genomsnittliga storleken dividerar du den globala storleken med det totala antalet resurser i `/var/dam` (hämtas ovan).

#### Hur många mallar används för närvarande? {#how-many-templates-are-currently-used}

Om du vill visa antalet mallar som finns på servern använder du en databasfråga, via CRXDE - Verktyg - Fråga:

* **Typ** `XPath`
* **Sökväg** `/`
* **Fråga** `//element(*, cq:Template)`

#### Hur många komponenter används för närvarande? {#how-many-components-are-currently-used}

Om du vill se antalet komponenter som finns på servern använder du en databasfråga, via CRXDE - Verktyg - Fråga:

* **Typ** `XPath`
* **Sökväg** `/`
* **Fråga** `//element(*, cq:Component)`

#### Hur många förfrågningar per timme har du på författarsystemet vid maximal tid? {#how-many-requests-per-hour-do-you-have-on-the-author-system-at-peak-time}

Så här avgör du begäranden per timme som du har på författarsystemet vid maximal tid:

1. Använd kommandoraden för att bestämma det totala antalet begäranden sedan installationen:

   ```shell
   cd <cq-installation-dir>/crx-quickstart/logs
   grep -R "\->" request.log | wc -l
   ```

1. Så här bestämmer du start- och slutdatum:

   ```shell
   vim request.log
   G / 1G: for the last/first lines
   ```

   Använd dessa värden för att beräkna antalet timmar som har gått sedan installationen, och sedan det genomsnittliga antalet begäranden per timme.

#### Hur många begäranden per timme har du i publiceringssystemet vid maximal tid? {#how-many-requests-per-hour-do-you-have-on-the-publish-system-at-peak-time}

Upprepa proceduren ovan på din publiceringsinstans.

## Analyserar specifika scenarier {#analyzing-specific-scenarios}

Här följer en lista med förslag på vad du ska kontrollera om du får vissa prestandaproblem. Listan är tyvärr inte helt heltäckande.

>[!NOTE]
>
>Se även följande artiklar för mer information:
>
>* [Tråd dumpar](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17452.html)
>* [Analysera minnesproblem](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17482.html)
>* [Analysera med den inbyggda profileraren](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17499.html)
>* [Analysera långsamma och blockerade processer](https://helpx.adobe.com/experience-manager/kb/AnalyzeSlowAndBlockedProcesses.html)
>

### CPU till 100 % {#cpu-at}

Om CPU i ditt system hela tiden körs med 100 %, se följande:

* Kunskapsbasen:

   * [Analysera långsamma och blockerade processer](https://helpx.adobe.com/experience-manager/kb/AnalyzeSlowAndBlockedProcesses.html)

### Slut på minne {#out-of-memory}

Även om sådana fel bör identifieras under utveckling och testning kan vissa scenarier gå igenom.

Om det inte finns tillräckligt med minne i systemet kan det här problemet visas på olika sätt, bland annat prestandaförsämringar och felmeddelanden, bland annat undertexten:

`java.lang.OutOfMemoryError`

Kontrollera i så fall:

* JVM-inställningarna som används för att [starta AEM](/help/sites-deploying/deploy.md#getting-started)
* Kunskapsbasen:

   * [Analysera minnesproblem](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17482.html)

### Skiva-I/O {#disk-i-o}

Om det inte finns tillräckligt med diskutrymme på datorn eller om det uppstår problem med hårddisken kan du läsa mer i:

* Oavsett om du har inaktiverat en samling felsökningsinformation kan den konfigureras på olika platser, bland annat följande:

   * [Apache Sling JSP Script Handler](/help/sites-deploying/osgi-configuration-settings.md#apacheslingjspscripthandler)
   * [Apache Sling JavaScript-hanterare](/help/sites-deploying/osgi-configuration-settings.md#apacheslingjavascripthandler)
   * [Konfiguration av Apache Sling-loggning](/help/sites-deploying/osgi-configuration-settings.md#apacheslingloggingconfiguration)
   * [Bibliotekshanterare för CQ HTML](/help/sites-deploying/osgi-configuration-settings.md#daycqhtmllibrarymanager)
   * [Felsökningsfilter för CQ WCM](/help/sites-deploying/osgi-configuration-settings.md#daycqwcmdebugfilter)
   * [Loggers](/help/sites-deploying/monitoring-and-maintaining.md#activating-the-debug-log-level)

* Om och hur du har konfigurerat [versionsrensning](/help/sites-deploying/version-purging.md)
* Kunskapsbasen:

   * [För många öppna filer](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17470.html)
   * [Journalen förbrukar för mycket diskutrymme](https://helpx.adobe.com/experience-manager/kb/JournalTooMuchDiskSpace.html)

### Regelbunden prestandaförsämring {#regular-performance-degradation}

Om du ser att prestandan för din instans försämras efter varje omstart (ibland en vecka eller senare) kan du kontrollera följande:

* [Slut på minne](#outofmemory)
* Kunskapsbasen:

   * [Oavslutade sessioner](https://helpx.adobe.com/experience-manager/kb/AnalyzeUnclosedSessions.html)

### JVM-justering {#jvm-tuning}

Java™ Virtual Machine (JVM) har förbättrats när det gäller justering (särskilt sedan Java™ 7). Därför är det ofta lämpligt att ange en rimlig fast JVM-storlek och att använda standardvärdena.

Om standardinställningarna inte är lämpliga är det viktigt att upprätta en metod för att övervaka och bedöma GC-prestanda. Gör det innan du försöker justera JVM. Den här processen kan omfatta övervakningsfaktorer, inklusive heapstorlek, algoritm och andra aspekter.

Några vanliga alternativ är:

* VerboseGC:

  ```
  -verbose:gc \
   -Xloggc:$LOGS/verbosegc.log \
   -XX:+PrintGCDetails \
   -XX:+PrintGCDateStamps
  ```

Den resulterande loggen kan importeras av en GC-visualiserare som:

` [https://www.ibm.com/developerworks/library/j-ibmtools2/](https://www.ibm.com/developerworks/library/j-ibmtools2/)`

Eller JConsole:

* De här inställningarna gäller för en &quot;bred&quot; JMX-anslutning:

  ```
  -Dcom.sun.management.jmxremote \
   -Dcom.sun.management.jmxremote.port=8889 \
   -Dcom.sun.management.jmxremote.authenticate=false \
   -Dcom.sun.management.jmxremote.ssl=false
  ```

* Anslut sedan till JVM med JConsole. Se följande:
  ` [https://docs.oracle.com/javase/8/docs/technotes/guides/management/jconsole.html](https://docs.oracle.com/javase/8/docs/technotes/guides/management/jconsole.html)`

Du kan se hur mycket minne som används, vilka GC-algoritmer som används, hur lång tid det tar att köra dem och vilken effekt den här processen har på programmets prestanda. Utan den är finjustering bara &quot;slumpmässigt virvlande knoppar&quot;.

>[!NOTE]
>
>För Oracle VM finns även information på:
>
>[https://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html)
