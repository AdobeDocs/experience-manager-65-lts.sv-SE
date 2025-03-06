---
title: Återställa AEM-formulärdata
description: I det här dokumentet beskrivs de steg som krävs för att återställa AEM formulärdata.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 6345edda-cdc6-4e13-ade6-2dd6de9d9616
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1118'
ht-degree: 0%

---

# Återställa AEM-formulärdata {#recovering-the-aem-forms-data}

I det här avsnittet beskrivs de steg som krävs för att återställa AEM formulärdata. Se även [Specialöverväganden för säkerhetskopiering och återställning](/help/forms/using/admin-help/backup-recovery-strategy-aem-forms.md#special-considerations-for-backup-and-recovery).

>[!NOTE]
>
>Kataloger för databas, GDS, AEM-databas och Content Storage Root måste återställas till en dator med samma DNS-namn som originalet.

AEM blanketter kan återställas på ett tillförlitligt sätt vid följande fel:

**Diskfel:** Det senaste säkerhetskopieringsmediet krävs för att återställa databasinnehållet.

**Datafel:** Filsystem registrerar inte tidigare transaktioner och system kan av misstag skriva över nödvändiga processdata.

**Användarfel:** Återställningen begränsas till data som är tillgängliga i databasen. Om data har lagrats och är tillgängliga förenklas återställningen.

**Strömavbrott, systemkrasch:** Filsystems-API:er är ofta inte utformade eller används på ett robust sätt som skyddar mot oväntade systemfel. Om ett strömavbrott eller en systemkrasch inträffar är det troligare att dokumentinnehåll som lagras i databasen är uppdaterat än innehåll som lagras i ett filsystem.

Om du använder läget för rullande säkerhetskopiering är du fortfarande i säkerhetskopieringsläge efter återställning. Om du använder läget för säkerhetskopiering av ögonblicksbilder är du inte i säkerhetskopieringsläge efter återställning.

När du återställer från säkerhetskopiering till ett nytt system kan följande konfigurationer vara annorlunda. Den här skillnaden bör inte påverka en lyckad återställning av AEM-formulärprogrammet:

* IP-adress
* Fysisk systemkonfiguration (CPU, disk, minne)
* GDS-plats

>[!NOTE]
>
>Säkerhetskopian av rotkatalogen för innehållslagring måste återställas till platsen för den katalogen som den angavs under konfigurationen för innehållstjänster.

Om en enskild nod i ett Multinode-kluster inte fungerar och de återstående noderna i klustret fungerar som de ska, utför du klusteråterställningen med en nod.

## Återställa AEM-formulärdata {#recover-the-aem-forms-data}

1. Stoppa AEM formulärtjänster och programservern om du kör programmet.
1. Om det behövs kan du återskapa det fysiska systemet från en systemavbildning. Det här steget är kanske inte nödvändigt om orsaken till återställningen är en felaktig databasserver.
1. Använd patchar eller uppdateringar i AEM-formulär som har använts sedan bilden skapades. Denna information registrerades under säkerhetskopieringen. AEM-formulär måste korrigeras på samma nivå som när systemet säkerhetskopierades.
1. (WebSphere® Application Server) Om du återställer till en ny instans av WebSphere® Application Server kör du kommandot restoreConfig.bat/sh.
1. Återställ AEM-formulärdatabasen genom att först köra en databasåterställningsåtgärd med hjälp av databasens säkerhetskopieringsfiler och sedan använda transaktionsupprepningsloggarna på den återställda databasen. (Se [AEM-formulärdatabas](/help/forms/using/admin-help/files-back-recover.md#aem-forms-database).) Mer information finns i en av följande artiklar i kunskapsbasen:

   * [DB2](/help/forms/using/admin-help/files-back-recover.md#db2)
   * [Oracle Säkerhetskopiering och återställning för AEM-formulär](/help/forms/using/admin-help/files-back-recover.md#oracle)
   * [Microsoft](/help/forms/using/admin-help/files-back-recover.md#sql-server)
   * [MySQL Backup and Recovery for AEM forms](/help/forms/using/admin-help/files-back-recover.md#mysql)

1. Återställ GDS-katalogen genom att först ta bort innehållet i GDS-katalogen i den befintliga installationen av AEM-formulär och sedan kopiera innehållet i GDS-katalogen från den säkerhetskopierade GDS-katalogen. Om du har ändrat GDS-katalogplatsen läser du [Ändra GDS-platsen under återställning](recovering-aem-forms-data.md#changing-the-gds-location-during-recovery).
1. Byt namn på GDS-säkerhetskopieringskatalogen som ska återställas enligt följande exempel:

   >[!NOTE]
   >
   >Om katalogen /restore redan finns säkerhetskopierar du den och tar sedan bort den innan du byter namn på katalogen /backup som innehåller de senaste data.

   * (JBoss®) Byt namn på `[appserver root]/server/'server'/svcnative/DocumentStorage/backup` till:

     `[appserver root]/server/'server'/svcnative/DocumentStorage/restore`.

   * (WebLogic) Byt namn på `[appserverdomain]/'server'/adobe/AEMformsserver/DocumentStorage/backup` till:

     `[appserverdomain]/'server'/adobe/AEMformsserver/DocumentStorage/restore`.

   * (WebSphere®) Byt namn på `[appserver root]/installedApps/adobe/'server'/DocumentStorage/backup` till:

     `[appserver root]/installedApps/adobe/'server'/DocumentStorage/restore`.

1. Återställ rotkatalogen för innehållslagring genom att först ta bort innehållet i rotkatalogen för innehållslagring i den befintliga installationen av AEM-formulär och sedan återställa innehållet genom att utföra följande åtgärder för antingen fristående eller klustrade miljöer:

   >[!NOTE]
   >
   >Säkerhetskopian av rotkatalogen för innehållslagring måste återställas till platsen för rotkatalogen för innehållslagring så som den angavs under konfigurationen för innehållstjänster (borttagen).

   **Fristående:** Återställ alla säkerhetskopierade kataloger under återställningsprocessen. När katalogerna återställs och katalogen /backup-lucene-indexes finns byter du namn på den till /lucene-indexes. I annat fall ska katalogen lucene-indexes redan finnas och ingen åtgärd krävs.

   **Grupperad:** Återställ alla säkerhetskopierade kataloger under återställningsprocessen. Om du vill återställa indexrotkatalogen utför du följande steg på varje nod i klustret:

   * Ta bort allt innehåll i indexrotkatalogen.
   * Om katalogen /backup-lucene-indexes finns kopierar du innehållet i katalogen *Content Storage Root*/backup-lucene-indexes till katalogen Index Root och tar bort katalogen *Content Storage Root*/backup-lucene-indexes.
   * Om katalogen /lucene-indexes finns kopierar du innehållet i katalogen *Content Storage Root*/lucene-indexes till katalogen Index Root.

1. Återställ CRX-databasen.

   * **Fristående**

     *Återställ författare och publicera instanser*: Om ett haveri inträffar kan du återställa databasen till det senaste säkerhetskopierade läget genom att utföra de steg som beskrivs i [Säkerhetskopiera och återställ.](https://helpx.adobe.com/experience-manager/kb/CRXBackupAndRestoreProcedure.html)

     När författarnoden återställs kontrolleras även återställningen av Forms Manager- och AEM Forms Workspace-data.

   * **Grupperad**

     För återställning i en klustrad miljö, se [Strategi för säkerhetskopiering och återställning i en klustrad miljö](/help/forms/using/admin-help/strategy-backup-restore-clustered-environment.md#strategy-for-backup-and-restore-in-a-clustered-environment).

1. Ta bort eventuella temporära AEM-formulär som har skapats i katalogen java.io.temp eller i Adobe tillfälliga katalog.
1. Starta AEM-formulär (se [Starta och stoppa tjänster](/help/forms/using/admin-help/starting-stopping-services.md#starting-and-stopping-services))<!-- BROKEN LINK and the application server(s) (see [Maintaining the Application Server](/help/forms/using/admin-help/topics/maintaining-the-application-server.md))-->.

## Ändra GDS-platsen under återställning {#changing-the-gds-location-during-recovery}

Om ditt GDS återställs till en annan plats än den ursprungliga kör du LCSetGDS-skriptet och anger GDS till den nya platsen. Skriptet finns i mappen `[aem-forms root]\sdk\misc\Foundation\SetGDSCommandline`. Skriptet har två parametrar, `defaultGDS` och `newGDS`. Se filen `ReadMe.txt` i samma mapp för instruktioner om hur du kör skriptet.

>[!NOTE]
>
>Om du har aktiverat dokumentlagring i databasen behöver du inte ändra GDS-platsen.

>[!NOTE]
>
>Den här begränsningen är den enda under vilken du bör använda det här skriptet för att ändra GDS-platsen. Använd administrationskonsolen om du vill ändra GDS-platsen medan AEM-formulär körs. (Se [Konfigurera allmänna inställningar för AEM-formulär](/help/forms/using/admin-help/configure-general-aem-forms-settings.md#configure-general-aem-forms-settings).)

>[!NOTE]
>
>Komponentdistributionen misslyckas i Windows om GDS-katalogen finns i enhetsroten (till exempel D:\). För GDS måste du se till att katalogen inte finns i enhetens rot utan i en underkatalog. Katalogen ska till exempel vara D:\GDS och inte bara D:\.

## Återställa GDS till en klustrad miljö {#recovering-the-gds-to-a-clustered-environment}

Om du vill ändra GDS-platsen i en klustrad miljö stänger du av hela klustret och kör LCSetGDS-skriptet på en enskild nod i klustret. (Se [Ändra GDS-plats under återställning](recovering-aem-forms-data.md#changing-the-gds-location-during-recovery).) Starta endast den noden. När den noden har startats kan andra noder i klustret startas på ett säkert sätt och pekar korrekt på den nya GDS-servern.

>[!NOTE]
>
>Om du inte kan säkerställa att en nod startas fullständigt innan du startar andra noder måste du köra LCSetGDS-skriptet på alla noder i klustret innan du startar klustret.
