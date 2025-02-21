---
title: Kontrollera och felsök efter uppgradering
description: Lär dig hur du felsöker problem som kan uppstå efter en uppgradering.
contentOwner: sarchiz
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: upgrading
content-type: reference
docset: aem65
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1798'
ht-degree: 0%

---

# Kontrollera och felsök efter uppgradering{#post-upgrade-checks-and-troubleshooting}

## Bokför uppgraderingskontroller {#post-upgrade-checks}

Efter [lokal uppgradering](/help/sites-deploying/in-place-upgrade.md) ska följande aktiviteter utföras för att slutföra uppgraderingen. Vi antar att AEM har startats med 6.5-behållaren och att den uppgraderade kodbasen har distribuerats.

* [Verifiera loggar för uppgraderingen](#main-pars-header-290365562)

* [Verifiera OSGi Bundles](#main-pars-header-1637350649)

* [Verifiera Oak-version](#main-pars-header-1293049773)

* [Granska mappen PreUpgradeBackup](#main-pars-header-988995987)

* [Inledande validering av sidor](#main-pars-header-20827371)
* [Använd AEM Service Pack](#main-pars-header-215142387)

* [Migrera AEM-funktioner](#main-pars-header-1434457709)

* [Verifiera planerade underhållskonfigurationer](#main-pars-header-1552730183)

* [Aktivera replikeringsagenter](#main-pars-header-823243751)

* [Aktivera anpassade schemalagda jobb](#main-pars-header-244535083)

* [Kör testplan](#main-pars-header-1167972233)

### Verifiera loggar för lyckad uppgradering {#verify-logs-for-upgrade-success}

**upgrade.log**

Tidigare krävdes noggrann kontroll av olika loggfiler, delar av databasen och startplattan för att kontrollera status efter uppgraderingen av instansen. Genom att generera en rapport efter uppgraderingen kan du upptäcka defekta uppgraderingar innan du publicerar den.

Huvudsyftet med den här funktionen är att minska behovet av manuell tolkning eller komplex parsningslogik för flera slutpunkter som krävs för att en uppgradering ska lyckas. Lösningen syftar till att tillhandahålla entydig information för externa automatiseringssystem som kan reagera på om en uppdatering lyckas eller inte.

Mer specifikt säkerställer det att

* Uppgraderingsfel som upptäcks av uppgraderingsramverket centraliseras i en enda uppgraderingsrapport.
* Uppgraderingsrapporten innehåller indikatorer om nödvändigt manuellt ingripande.

För att tillgodose detta har ändringar gjorts i hur loggarna genereras i filen `upgrade.log`.

Här följer ett exempel på en rapport som inte visar några fel under uppgraderingen:

![1487887443006](assets/1487887443006.png)

Här följer ett exempel på en rapport som visar ett paket som inte installerades under uppgraderingsprocessen:

![1487887532730](assets/1487887532730.png)

**error.log**

error.log bör granskas noggrant under och efter det att AEM startas med målversionen jar. Alla varningar och fel bör granskas. I allmänhet är det bäst att söka efter problem i början av loggen. Fel som inträffar senare i loggen kan i själva verket vara bieffekter av en rotorsak som anropas tidigt i filen. Om upprepade fel och varningar inträffar, se nedan för [Analysera problem med uppgraderingen](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md#analyzing-issues-with-the-upgrade).

### Verifiera OSGi Bundles {#verify-osgi-bundles}

Navigera till OSGi-konsolen `/system/console/bundles` och kontrollera om några paket inte har startats. Om något paket är installerat kan du ta reda på vad som är rotproblemet i `error.log`.

### Verifiera Oak-version {#verify-oak-version}

Efter uppgraderingen bör du kontrollera att Oak-versionen har uppdaterats till **1.10.2**. Kontrollera Oak-versionen genom att navigera till OSGi-konsolen och titta på den version som är kopplad till Oak-paket: Oak Core, Oak Commons, Oak Segment tar.

### Inspektera mappen PreUpgradeBackup {#inspect-preupgradebackup-folder}

Under uppgraderingen försöker AEM säkerhetskopiera anpassningar och lagra dem under `/var/upgrade/PreUpgradeBackup/<time-stamp-of-upgrade>`. Om du vill visa den här mappen i CRXDE Lite kan du behöva [aktivera CRXDE Lite](/help/sites-administering/enabling-crxde-lite.md) tillfälligt.

Mappen med tidsstämpeln ska ha en egenskap med namnet `mergeStatus` med värdet `COMPLETED`. Mappen **att bearbeta** måste vara tom och noden **overwritten** anger vilka noder som skrevs över under uppgraderingen. Innehåll under den vänstra noden visar innehåll som inte kan sammanfogas på ett säkert sätt under uppgraderingen. Om implementeringen är beroende av någon av de underordnade noderna (och inte redan har installerats av det uppgraderade kodpaketet) måste de sammanfogas manuellt.

Inaktivera CRXDE Lite efter den här övningen om du arbetar på en scen- eller produktionsmiljö.

### Inledande validering av sidor {#initial-validation-of-pages}

Utför en inledande validering mot flera sidor i AEM. Om du uppgraderar en redigeringsmiljö öppnar du Start-sidan och välkomstsidan ( `/aem/start.html`, `/libs/cq/core/content/welcome.html`). I både redigerings- och publiceringsmiljöer öppnar du några programsidor och röktestar som de återges korrekt. Om det uppstår några problem kan du läsa `error.log` för att felsöka.

### Använd AEM Service Pack {#apply-aem-service-packs}

Använd eventuella AEM 6.5 Service Pack om de har släppts.

### Migrera AEM-funktioner {#migrate-aem-features}

Flera funktioner i AEM kräver ytterligare steg efter uppgraderingen. En fullständig lista över de här funktionerna och stegen för att migrera dem i AEM 6.5 finns på sidan [Uppgradera kod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md).

### Verifiera planerade underhållskonfigurationer {#verify-scheduled-maintenance-configurations}

#### Aktivera skräpinsamling för datalager {#enable-data-store-garbage-collection}

Om du använder ett fildatalager måste du se till att aktiviteten Skräpinsamling i datalagret är aktiverad och läggs till i listan Veckounderhåll. Instruktioner beskrivs under [Revision Cleanup](/help/sites-administering/data-store-garbage-collection.md).

>[!NOTE]
>
>Detta rekommenderas inte för anpassade datalagerinstallationer i S3 eller när ett delat datalager används.

#### Aktivera rensning av onlineändringar {#enable-online-revision-cleanup}

Om du använder MongoMK eller det nya StjärmMK-segmentformatet kontrollerar du att aktiviteten Revision Clean Up (Revision Clean Up) är aktiverad och läggs till i listan Daily Maintenance (Dagligt underhåll). Instruktioner beskrivs under [Revision Cleanup](/help/sites-deploying/revision-cleanup.md).

### Kör testplan {#execute-test-plan}

Kör detaljerad testplan mot [uppgraderingskod och anpassningar](/help/sites-deploying/upgrading-code-and-customizations.md) enligt avsnittet **Testprocedur**.

### Aktivera replikeringsagenter {#enable-replication-agents}

När publiceringsmiljön har uppgraderats och validerats aktiverar du replikeringsagenter i redigeringsmiljön. Kontrollera att agenter kan ansluta till respektive publiceringsinstanser. Se [uppgraderingsproceduren](/help/sites-deploying/upgrade-procedure.md) om du vill ha mer information om ordningen för händelser.

### Aktivera anpassade schemalagda jobb {#enable-custom-scheduled-jobs}

Alla schemalagda jobb som en del av kodbasen kan nu aktiveras.

## Analysera problem med uppgraderingen {#analyzing-issues-with-upgrade}

Det här avsnittet innehåller några problemscenarier som man kan ställas inför vid uppgradering till AEM 6.3.

Dessa scenarier bör hjälpa till att hitta orsaken till uppgraderingsrelaterade problem och bör hjälpa till att identifiera projekt- eller produktspecifika problem.

### Databasmigreringen misslyckades  {#repository-migration-failing-}

Datamigreringen från CRX2 till Oak bör vara möjlig för alla scenarier som börjar med Source Instances baserade på CQ 5.4. Se till att du följer uppgraderingsinstruktionerna i det här dokumentet, som innehåller förberedelsen av `repository.xml`, och kontrollera att ingen anpassad autentiserare har startats via JAAS, och att instansen har kontrollerats för inkonsekvenser innan migreringen startar.

Om migreringen fortfarande misslyckas kan du ta reda på vad som är rotorsaken genom att undersöka `upgrade.log`. Om problemet inte är känt ännu rapporterar du det till kundsupport.

### Uppgraderingen kördes inte {#the-upgrade-did-not-run}

Innan du startar förberedelsestegen måste du först köra instansen **source** genom att köra den med kommandot Java™ -jar aem-quickstart.jar. Detta krävs för att säkerställa att filen quickstart.properties genereras korrekt. Om den saknas fungerar inte uppgraderingen. Du kan också kontrollera om filen finns genom att titta under `crx-quickstart/conf` i källinstansens installationsmapp. När AEM startar uppgraderingen måste den också köras med kommandot Java™ -jar aem-quickstart.jar. AEM startas inte i uppgraderingsläge när du startar från ett startskript.

### Paket och paket kunde inte uppdateras  {#packages-and-bundles-fail-to-update-}

Om paketen inte installeras under uppgraderingen kommer de paket de innehåller inte heller att uppdateras. Den här kategorin av problem orsakas av felkonfigurering av datalagret. De visas också som **ERROR**- och **WARN**-meddelanden i error.log. Eftersom standardinloggningen i de flesta fall kan misslyckas kan du använda CRXDE direkt för att undersöka och hitta konfigurationsproblemen.

### Vissa AEM Bundles växlar inte till det aktiva läget {#some-aem-bundles-are-not-switching-to-the-active-state}

Om det inte finns några paket som kan startas kontrollerar du om det finns några beroenden som inte är nöjda.

Om det här problemet uppstår men baseras på en misslyckad paketinstallation som ledde till att paket inte uppgraderas, kommer de att anses vara inkompatibla för den nya versionen. Mer information om hur du felsöker detta finns i **Paket och paket som inte kan uppdateras** ovan.

Vi rekommenderar också att du jämför paketlistan för en ny instans av AEM 6.5 med den uppgraderade instansen för att identifiera de paket som inte uppgraderats. Detta ger en närmare beskrivning av vad du ska söka efter i `error.log`.

### Anpassade paket växlar inte till aktivt läge {#custom-bundles-not-switching-to-the-active-state}

Om dina anpassade paket inte växlar till det aktiva läget är det troligtvis så att det finns kod som inte importerar ändrings-API. Detta leder ofta till missnöjda beroenden.

API som har tagits bort ska markeras som borttaget i en av de tidigare versionerna. Instruktioner om direktmigrering av koden finns i det här meddelandet om borttagning. Adobe har som mål att skapa semantiska versioner där det är möjligt, så att versionerna kan indikera förändringar som går förlorade.

Det är också bäst att kontrollera om den ändring som orsakade problemet var nödvändig och återställa den om så inte är fallet. Kontrollera också om versionsökningen av paketexporten har ökat mer än nödvändigt efter strikt semantisk versionshantering.

### Felaktigt gränssnitt för plattformen {#malfunctioning-platform-ui}

Om det finns vissa gränssnittsfunktioner som inte fungerar som de ska efter uppgraderingen bör du först kontrollera om gränssnittet har anpassats. Vissa strukturer kan ha ändrats och övertäckningen kan behöva uppdateras eller vara föråldrad.

Kontrollera sedan om det finns några JavaScript-fel som kan spåras till anpassade tillägg som är kopplade till klientbibliotek. Samma sak kan gälla för anpassad CSS som kan orsaka problem i AEM-layouten.

Slutligen kontrollerar du om JavaScript inte kan hantera några felkonfigurationer. Detta är vanligtvis fallet med felaktigt inaktiverade tillägg.

### Felfungerande anpassade komponenter, mallar eller gränssnittstillägg {#malfunctioning-custom-components-templates-or-ui-extensions}

Vanligtvis är orsaken till de här problemen densamma som för paket som inte har startats eller paket som inte installeras med den enda skillnaden att problemen börjar när komponenterna används.

Ett sätt att hantera felaktig egen kod är att först utföra röktester för att identifiera orsaken. När du har hittat den kan du titta i rekommendationerna i det här [link]-avsnittet i artikeln för att se hur du kan åtgärda dem.

### Anpassningar saknas under /etc {#missing-customizations-under-etc}

`/apps` och `/libs` hanteras bra av uppgraderingen, men ändringar under `/etc` kan behöva återställas manuellt från `/var/upgrade/PreUpgradeBackup` efter uppgraderingen. Kontrollera den här platsen för allt innehåll som behöver sammanfogas manuellt.

### Analyserar error.log och upgrade.log {#analyzing-the-error.log-and-upgrade.log}

I de flesta fall måste du söka efter fel i loggarna för att hitta orsaken till ett problem. Vid uppgraderingar är det dock också nödvändigt att övervaka beroendeproblem eftersom gamla paket kanske inte uppgraderas korrekt.

Det bästa sättet att göra detta är att ta bort error.log genom att ta bort alla meddelanden som inte har med problemet att göra. Du kan göra detta med ett verktyg som grep genom att använda:

```shell
grep -v UnrelatedErrorString
```

Vissa felmeddelanden kanske inte är omedelbart förklarande. I det här fallet kan det vara lättare att förstå var felet uppstod om man tittar i sammanhanget. Du kan avgränsa felet med:

* `grep -B` för att lägga till rader före felet;

eller

* `grep -A` för att lägga till rader efter.

I ett fåtal fall kan fel också hittas i WARN-meddelanden eftersom det kan finnas giltiga fall som leder till det här läget och programmet inte alltid kan avgöra om det här är ett faktiskt fel. Se till att du också läser dessa meddelanden.

### Kontakta Adobe Support {#contacting-adobe-support}

Om du har gått igenom råden på den här sidan och fortfarande ser problem kontaktar du Adobe Support. Om du vill ge så mycket information som möjligt till den supporttekniker som arbetar med ditt ärende måste du inkludera filen upgrade.log från uppgraderingen.
