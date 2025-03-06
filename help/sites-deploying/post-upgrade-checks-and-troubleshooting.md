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
exl-id: 8b3d8d0f-10f7-4736-881d-8f1f21c69182
source-git-commit: a037dc7cbb13abfeb8a7289baded50d3d788cbf6
workflow-type: tm+mt
source-wordcount: '1200'
ht-degree: 0%

---

# Kontrollera och felsök efter uppgradering{#post-upgrade-checks-and-troubleshooting}

## Bokför uppgraderingskontroller {#post-upgrade-checks}

Efter [lokal uppgradering](/help/sites-deploying/in-place-upgrade.md) ska följande aktiviteter utföras för att slutföra uppgraderingen. Vi antar att AEM har startats med AEM 6.5 LTS jar och att den uppgraderade kodbasen har distribuerats.

* [Verifiera loggar för uppgraderingen](#verify-logs-for-upgrade-success)

* [Verifiera OSGi Bundles](#verify-osgi-bundles)

* [Verifiera Oak-version](#verify-oak-version)

* [Inledande validering av sidor](#initial-validation-of-pages)

* [Verifiera planerade underhållskonfigurationer](#verify-scheduled-maintenance-configurations)

* [Aktivera replikeringsagenter](#enable-replication-agents)

* [Aktivera anpassade schemalagda jobb](#enable-custom-scheduled-jobs)

* [Kör testplan](#execute-test-plan)


### Verifiera loggar för lyckad uppgradering {#verify-logs-for-upgrade-success}

**upgrade.log**

Tidigare krävdes noggrann kontroll av olika loggfiler, delar av databasen och startplattan för att kontrollera status efter uppgraderingen av instansen. Genom att generera en rapport efter uppgraderingen kan du upptäcka defekta uppgraderingar innan du publicerar den.

Huvudsyftet med den här funktionen är att minska behovet av manuell tolkning eller komplex parsningslogik för flera slutpunkter som krävs för att en uppgradering ska lyckas. Lösningen syftar till att tillhandahålla entydig information för externa automatiseringssystem som kan reagera på om en uppdatering lyckas eller inte.

Mer specifikt säkerställer det att

* Uppgraderingsfel som upptäcks av uppgraderingsramverket centraliseras i en enda uppgraderingsrapport.
* Uppgraderingsrapporten innehåller indikatorer om nödvändigt manuellt ingripande.

För att tillgodose detta har ändringar gjorts i hur loggarna genereras i filen `upgrade.log`.

**error.log**

error.log bör granskas noggrant under och efter det att AEM startas med målversionen jar. Alla varningar och fel bör granskas. I allmänhet är det bäst att söka efter problem i början av loggen. Fel som inträffar senare i loggen kan i själva verket vara bieffekter av en rotorsak som anropas tidigt i filen. Om upprepade fel och varningar inträffar, se nedan för [Analysera problem med uppgraderingen](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md#analyzing-issues-with-the-upgrade).

### Verifiera OSGi Bundles {#verify-osgi-bundles}

Navigera till OSGi-konsolen `/system/console/bundles` och kontrollera om några paket inte har startats. Om något paket är installerat kan du ta reda på vad som är rotproblemet i `error.log`.

### Verifiera Oak-version {#verify-oak-version}

Efter uppgraderingen bör du se att Oak-versionen har uppdaterats till **1.68.x**. Kontrollera Oak-versionen genom att navigera till OSGi-konsolen och titta på den version som är kopplad till Oak-paket: Oak Core, Oak Commons, Oak Segment tar.

### Inledande validering av sidor {#initial-validation-of-pages}

Utför en inledande validering mot flera sidor i AEM. Om du uppgraderar en redigeringsmiljö öppnar du Start-sidan och välkomstsidan ( `/aem/start.html`, `/libs/cq/core/content/welcome.html`). I både redigerings- och publiceringsmiljöer öppnar du några programsidor och röktestar som de återges korrekt. Om det uppstår några problem kan du läsa `error.log` för att felsöka.

### Verifiera planerade underhållskonfigurationer {#verify-scheduled-maintenance-configurations}

#### Aktivera skräpinsamling för datalager {#enable-data-store-garbage-collection}

Om du använder ett fildatalager måste du se till att aktiviteten Skräpinsamling i datalagret är aktiverad och läggs till i listan Veckounderhåll. Instruktioner beskrivs under [Revision Cleanup](/help/sites-administering/data-store-garbage-collection.md).

>[!NOTE]
>
>Detta rekommenderas inte för anpassade datalagerinstallationer i S3 eller när ett delat datalager används.

#### Aktivera rensning av onlineändringar {#enable-online-revision-cleanup}

Om du använder MongoMK eller det nya StjärmMK-segmentformatet kontrollerar du att aktiviteten Revision Clean Up (Revision Clean Up) är aktiverad och läggs till i listan Daily Maintenance (Dagligt underhåll). Instruktioner beskrivs under [Revision Cleanup](/help/sites-deploying/revision-cleanup.md).

### Aktivera replikeringsagenter {#enable-replication-agents}

När publiceringsmiljön har uppgraderats och validerats aktiverar du replikeringsagenter i redigeringsmiljön. Kontrollera att agenter kan ansluta till respektive publiceringsinstanser. Mer information om ordningen för händelser finns i [Uppgraderingsprocedur](/help/sites-deploying/upgrade-procedure.md).

### Aktivera anpassade schemalagda jobb {#enable-custom-scheduled-jobs}

Alla schemalagda jobb som en del av kodbasen kan nu aktiveras.

### Kör testplan {#execute-test-plan}

Kör detaljerad testplan enligt definitionen i [Uppgradera kod och anpassningar under avsnittet **Testprocedur**](/help/sites-deploying/upgrading-code-and-customizations.md#testing-procedure-testing-procedure).

## Analysera problem med uppgraderingen {#analyzing-issues-with-the-upgrade}

Det här avsnittet innehåller några problemscenarier som man kan ställas inför vid uppgradering till AEM 6.5 LTS.

### Paket och paket kunde inte uppdateras  {#packages-and-bundles-fail-to-update}

Om paketen inte installeras under uppgraderingen kommer de paket de innehåller inte heller att uppdateras. Den här kategorin av problem orsakas av felkonfigurering av datalagret. De visas också som **ERROR**- och **WARN**-meddelanden i error.log. Eftersom standardinloggningen i de flesta fall kan misslyckas kan du använda CRXDE direkt för att undersöka och hitta konfigurationsproblemen.

### Uppgraderingen kördes inte {#the-upgrade-did-not-run}

Innan du startar förberedelsestegen måste du först köra **source**-instansen genom att köra den med kommandot `java -jar aem-quickstart.jar`. Detta krävs för att säkerställa att filen quickstart.properties genereras korrekt. Om den saknas fungerar inte uppgraderingen. Du kan också kontrollera om filen finns genom att titta under `crx-quickstart/conf` i källinstansens installationsmapp. När du startar AEM för att starta uppgraderingen måste den dessutom köras med kommandot `java -jar <aem-quickstart-6.5-LTS.jar>`. AEM startas inte i uppgraderingsläge när du startar från ett startskript.

### Vissa AEM Bundles växlar inte till det aktiva läget {#some-aem-bundles-are-not-switching-to-the-active-state}

Om det inte finns några paket som kan startas kontrollerar du om det finns några beroenden som inte är nöjda.

Om det här problemet uppstår men baseras på en misslyckad paketinstallation som ledde till att paket inte uppgraderas, kommer de att anses vara inkompatibla för den nya versionen. Mer information om hur du felsöker detta finns i **Paket och paket som inte kan uppdateras** ovan.

Vi rekommenderar också att du jämför paketlistan för en ny AEM 6.5 LTS-instans med den uppgraderade instansen för att identifiera de paket som inte uppgraderats. Detta ger en närmare beskrivning av vad du ska söka efter i `error.log`.

### Anpassade paket växlar inte till aktivt läge {#custom-bundles-not-switching-to-the-active-state}

Om dina anpassade paket inte växlar till det aktiva läget är det troligtvis så att det finns kod som inte importerar ändrat API. Detta leder ofta till missnöjda beroenden.

Det är också bäst att kontrollera om den ändring som orsakade problemet var nödvändig och återställa om så inte är fallet. Kontrollera också om versionsökningen av paketexporten har ökat mer än nödvändigt efter strikt semantisk versionshantering.

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

Om du har gått igenom råden på den här sidan och fortfarande ser problem kontaktar du Adobe Support. Om du vill ge så mycket information som möjligt till den supporttekniker som arbetar med ditt ärende måste du inkludera filerna `error.log` och `upgrade.log` från din uppgradering.
