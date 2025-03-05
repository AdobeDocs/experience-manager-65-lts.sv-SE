---
title: Underhållsaktiviteter före uppgraderingen
description: Läs mer om de uppgifter före uppgradering som rekommenderas för AEM.
contentOwner: sarchiz
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: upgrading
docset: aem65
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: b7304709729915dbcc27533caf88b61cd5657a2c
workflow-type: tm+mt
source-wordcount: '1117'
ht-degree: 0%

---

# Underhållsaktiviteter före uppgraderingen{#pre-upgrade-maintenance-tasks}

Innan du påbörjar uppgraderingen är det viktigt att du följer dessa underhållsåtgärder för att vara säker på att systemet är klart och kan återställas om det skulle uppstå problem:

* [Indexdefinitioner](#index-definitions)
* [Se till att det finns tillräckligt med diskutrymme](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#ensure-sufficient-disk-space)
* [Fullständig säkerhetskopiering av AEM](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#fully-back-up-aem)
* [Generera filen quickstart.properties](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#generate-quickstart-properties)
* [Konfigurera rensning av arbetsflöde och granskningslogg](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#configure-wf-audit-purging)
* [Installera, konfigurera och köra uppgifter före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#install-configure-run-pre-upgrade-tasks)
* [Ta bort uppdateringar från katalogen /install](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#remove-updates-install-directory)
* [Stoppa alla väntelägesförekomster i kallt läge](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#stop-tarmk-coldstandby-instance)
* [Inaktivera anpassade schemalagda jobb](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#disable-custom-scheduled-jobs)
* [Kör rensning av offlineredigering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#execute-offline-revision-cleanup)
* [Kör skräpinsamling för datastore](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#execute-datastore-garbage-collection)
* [Uppgradera databasschemat om det behövs](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#upgradethedatabaseschemaifneeded)
* [Rotera loggfiler](/help/sites-deploying/pre-upgrade-maintenance-tasks.md#rotate-log-files)

## Indexdefinitioner {#index-definitions}

Se till att du har installerat de indexdefinitioner som krävs och som lanserats med AEM 6.5 Service Pack fram till AEM Service Pack 22 som minimum. (Mer information finns i [Versionsinformation för AEM 6.5 Service Pack](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/release-notes/release-notes)).

## Se till att det finns tillräckligt med diskutrymme {#ensure-sufficient-disk-space}

Kontrollera att det finns tillräckligt med diskutrymme när du kör uppgraderingen.

## Fullständig säkerhetskopiering av AEM {#fully-back-up-aem}

AEM bör säkerhetskopieras fullständigt innan uppgraderingen påbörjas. Säkerhetskopiera databasen, programinstallationen, datalagret och Mongo-instanserna om tillämpligt. Mer information om säkerhetskopiering och återställning av en AEM-instans finns i [Säkerhetskopiering och återställning](/help/sites-administering/backup-and-restore.md).

## Generera filen quickstart.properties {#generate-quickstart-properties}

När AEM startas från jar-filen skapas en `quickstart.properties`-fil under `crx-quickstart/conf`. Om AEM endast har startats med det tidigare startskriptet finns inte den här filen och uppgraderingen misslyckas. Kontrollera att filen finns och starta om AEM från filen jar om den inte finns.

## Konfigurera rensning av arbetsflöde och granskningslogg {#configure-wf-audit-purging}

Aktiviteterna `WorkflowPurgeTask` och `com.day.cq.audit.impl.AuditLogMaintenanceTask` kräver separata OSGi-konfigurationer och kan inte fungera utan dem. Om de inte fungerar när en uppgift körs före uppgraderingen är det mest troligt att konfigurationer saknas. Se därför till att du lägger till OSGi-konfigurationer för dessa uppgifter eller tar bort dem helt och hållet från listan över uppgifter som ska optimeras före uppgraderingen om du inte vill köra dem. Dokumentation om hur du konfigurerar rensningsaktiviteter för arbetsflöden finns på [Administrera arbetsflödesinstanser](/help/sites-administering/workflows-administering.md) och konfigurationen av underhållsaktiviteter för granskningsloggar finns på [Underhåll för granskningslogg i AEM 6](/help/sites-administering/operations-audit-log.md).


## Installera, konfigurera och köra uppgifter före uppgradering {#install-configure-run-pre-upgrade-tasks}

Underhållsuppgifter före uppgradering som tidigare skulle utföras manuellt optimeras och automatiseras. Tack vare underhållsoptimeringen före uppgraderingen kan dessa uppgifter utlösas på ett enhetligt sätt och resultaten kan kontrolleras vid behov.

### Så här använder du den {#how-to-use-it}

OSGI-komponenten `PreUpgradeTasksMBean` levereras förkonfigurerad med en lista över underhållsuppgifter som kan köras alla på en gång. Du kan konfigurera uppgifterna genom att följa proceduren nedan:

1. Gå till webbkonsolen genom att gå till *https://serveraddress:serverport/system/console/configMgr*

1. Sök efter **föruppgraderingsuppgifter** och klicka sedan på den första matchande komponenten. Komponentens fullständiga namn är `com.adobe.aem.upgrade.prechecks.mbean.impl.PreUpgradeTasksMBeanImpl`

1. Ändra listan över underhållsaktiviteter som måste köras enligt nedan:

   ![1487758925984](assets/1487758925984.png)

Nedan visas en beskrivning av det körläge som varje underhållsåtgärd är avsedd för.

| Uppgift | Anteckningar |
|---|---|
| ArbetsflödeRensaAktivitet | Adobe Granite Workflow Renge Configuration OSGi måste konfigureras innan programmet körs. |
| GenerateBundlesListFileTask |   |
| RevisionRensaAktivitet |   |
| com.day.cq.audit.impl.AuditLogMaintenanceTask | Du måste konfigurera OSGi-konfigurationen för rensning av granskningslogg innan du kör. |

### Standardkonfiguration för hälsokontroller före uppgradering {#default-configuration-of-the-pre-upgrade-health-checks}

OSGI-komponenten `PreUpgradeTasksMBeanImpl` levereras förkonfigurerad med en lista över hälsokontrollstaggar som ska köras före uppgraderingen när `runAllPreUpgradeHealthChecks` -metoden anropas:

* **system** - taggen som används av hälsokontrollerna för granitunderhåll

* **föruppgradering** - en anpassad tagg som kan läggas till i alla hälsokontroller som du kan ställa in att köra före en uppgradering

**MBean-metoder**

Du kan komma åt funktionen för hanterade bönor med hjälp av [JMX-konsolen](/help/sites-administering/jmx-console.md).

Du kan komma åt MBeans genom att:

1. Gå till JMX-konsolen på *https://serveraddress:serverport/system/console/jmx*
1. Sök efter **PreUpgradeTasks** och klicka på resultatet

1. Välj en metod i avsnittet **Åtgärder** och välj **Anropa** i följande fönster.

Nedan visas en lista med alla tillgängliga metoder som `PreUpgradeTasksMBeanImpl` visar:

| Metodnamn | Typ | Beskrivning |
|---|---|---|
| runAllPreUpgradeTasks() | ÅTGÄRD | Kör alla underhållsaktiviteter som är före uppgraderingen i listan. |
| runPreUpgradeTask(preUpgradeTaskName) | ÅTGÄRD | Kör underhållsaktiviteten före uppgradering med det namn som anges som parameter. |
| getPreUpgradeTaskLastRunTime(preUpgradeTaskName) | ÅTGÄRD | Visar den exakta körningstiden för underhållsaktiviteten före uppgradering med det namn som anges som parameter. |
| getPreUpgradeTaskLastRunState(preUpgradeTaskName) | ÅTGÄRD | Visar det senaste körningstillståndet för underhållsaktiviteten före uppgradering med det namn som anges som parameter. |
| runAllPreUpgradeHealthChecks(closedDownOnSuccess) | ÅTGÄRD | Kör alla hälsokontroller som är före uppgraderingen och sparar statusen i en fil som heter preUpgradeHCStatus.properties i den sling-hemsökvägen. Om closeDownOnSuccess är inställt på true stängs AEM-instansen av, men bara om alla hälsokontroller före uppgraderingen har statusen OK. Egenskapsfilen används som ett villkor för framtida uppgraderingar och uppgraderingsprocessen stoppas om hälsokontrollen före uppgraderingen misslyckades. Om du vill ignorera resultatet av föruppgraderingskontrollerna och starta uppgraderingen ändå, kan du ta bort filen. |
| discoverUsageOfUnavailableAPI(aemVersion) | ÅTGÄRD | Visar alla importerade paket som inte längre är uppfyllda vid uppgradering till den angivna AEM-versionen. AEM-målversionen måste anges som en parameter. |

>[!NOTE]
>
>MBean-metoderna kan anropas via:
>
>* JMX-konsolen
>* Alla externa program som ansluter till JMX
>* cURL
>

## Ta bort uppdateringar från katalogen /install {#remove-updates-install-directory}

>[!NOTE]
>
>Ta endast bort paket från katalogen crx-quickstart/install när du har stängt AEM-instansen. Det här steget är ett av de sista innan du startar uppgraderingsproceduren på plats.

Ta bort alla Service Pack, funktionspaket eller snabbkorrigeringar som har distribuerats via katalogen `crx-quickstart/install` i det lokala filsystemet. Detta förhindrar oavsiktlig installation av gamla snabbkorrigeringar och servicepaket ovanpå den nya AEM-versionen när uppdateringen har slutförts.

## Stoppa alla väntelägesförekomster i kallt läge {#stop-tarmk-coldstandby-instance}

Om du använder kallstart för TjäraMK ska du stoppa alla kalliga standby-instanser. Detta garanterar ett effektivt sätt att återansluta till Internet om det uppstår problem i uppgraderingen. När uppgraderingen har slutförts måste instanserna i kallt vänteläge återskapas från de uppgraderade primära instanserna.

## Inaktivera anpassade schemalagda jobb {#disable-custom-scheduled-jobs}

Inaktivera alla schemalagda OSGi-jobb som ingår i programkoden.

## Kör rensning av offlineredigering {#execute-offline-revision-cleanup}

>[!NOTE]
>
>Detta steg är endast nödvändigt för bensinanläggningar

Om du använder tarMK bör du köra Revision Cleanup offline innan du uppgraderar. Detta gör att databasmigreringssteget och efterföljande uppgraderingsuppgifter körs mycket snabbare och hjälper till att säkerställa att rensning av onlineändringar kan utföras korrekt när uppgraderingen har slutförts. Information om hur du kör rensning av offlineredigering finns i [Utför rensning av offlineredigering](/help/sites-deploying/revision-cleanup.md#revision-cleanuprevision-cleanup).

## Kör skräpinsamling för datastore {#execute-datastore-garbage-collection}

När du har kört revisionsrensning på CRX3-instanser bör du köra Datastore Garbage Collection för att ta bort alla blobbar som inte refereras i datalagret. Instruktioner finns i dokumentationen om [skräpinsamlingen för datalagret](/help/sites-administering/data-store-garbage-collection.md).

## Rotera loggfiler {#rotate-log-files}

Adobe rekommenderar att du arkiverar dina aktuella loggfiler innan du påbörjar uppgraderingen. På så sätt blir det enklare att övervaka och skanna loggfilerna under och efter uppgraderingen för att identifiera och lösa eventuella problem som kan uppstå.
