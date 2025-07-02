---
title: Strategi för säkerhetskopiering och återställning i en klustrad miljö
description: Om er AEM-formulärimplementering lagrar ytterligare anpassade data i en annan databas måste ni implementera en strategi för att säkerhetskopiera dessa data så att de är synkroniserade med AEM formulärdata.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 0fe9b02a-96b4-462f-a940-a2d6084ed0a4
source-git-commit: 1b7e0c532ab46346059de01cee4a1adecf3a0a13
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# Strategi för säkerhetskopiering och återställning i en klustrad miljö {#strategy-for-backup-and-restore-in-a-clustered-environment}

>[!NOTE]
>
>Om er AEM-formulärimplementering lagrar ytterligare anpassade data i en annan databas måste ni implementera en strategi för att säkerhetskopiera dessa data så att de är synkroniserade med AEM formulärdata. Dessutom måste programmet vara utformat så att det är tillräckligt robust för att hantera ett scenario där ytterligare databaser inte är synkroniserade. Vi rekommenderar starkt att alla databasåtgärder som utförs utförs i samband med en transaktion för att upprätthålla ett konsekvent tillstånd.

Du behöver säkerhetskopiera följande delar av AEM formulärsystem för att kunna återställa efter eventuella fel:

* Databas som används i AEM-formulär
* GDS som har långlivade data och andra beständiga dokument
* AEM-databas (crx-database)

>[!NOTE]
>
>Du måste säkerhetskopiera alla andra data som används i AEM-formulärinställningarna, som kundteckensnitt, anslutningsdata osv.

## Säkerhetskopiera en klustrad miljö {#back-up-a-clustered-environment}

I det här avsnittet diskuteras följande strategier för att säkerhetskopiera AEM-formulär i en klustrad miljö:

* Offlinesäkerhetskopiering med driftstopp
* Offlinesäkerhetskopiering utan driftstopp (säkerhetskopiering av en sekundär nod som är avstängd)
* Onlinesäkerhetskopiering utan driftstopp och fördröjning
* Säkerhetskopiera Bootstrap egenskapsfil

### Offlinesäkerhetskopiering med driftstopp {#offline-backup-with-downtime}

1. Stäng av hela klustret och tillhörande tjänster. (se [Starta och stoppa tjänster](/help/forms/using/admin-help/starting-stopping-services.md#starting-and-stopping-services))
1. Säkerhetskopiera databasen, GDS och Connectors på alla noder. (se [Filer som ska säkerhetskopieras och återställas](/help/forms/using/admin-help/files-back-recover.md#files-to-back-up-and-recover))
1. Så här säkerhetskopierar du AEM-databasen offline:

   1. För varje klusternod säkerhetskopierar du filen som innehåller klusternods-ID:t.
   1. Säkerhetskopiera alla filer i en sekundär klusternod, inklusive underkataloger.
   1. Säkerhetskopiera databas-/system-ID för varje klusternod separat.

   Mer information finns i [Säkerhetskopiera och återställ](/help/sites-administering/backup-and-restore.md).

1. Säkerhetskopiera alla andra data, till exempel kundens typsnitt.
1. Starta klustret igen.

### Offlinesäkerhetskopiering utan driftstopp {#offline-backup-with-no-downtime}

1. Ange läget för rullande säkerhetskopiering. (se [Ange säkerhetskopieringslägen](/help/forms/using/admin-help/backing-aem-forms-data.md#entering-the-backup-modes))

   Lämna läget för rullande säkerhetskopiering efter en återställning.

1. Stäng någon av de sekundära noderna i klustret för AEM. (se [Starta och stoppa tjänster](/help/forms/using/admin-help/starting-stopping-services.md#starting-and-stopping-services))
1. Säkerhetskopiera databasen, GDS och Connectors på alla noder. (se [Filer som ska säkerhetskopieras och återställas](/help/forms/using/admin-help/files-back-recover.md#files-to-back-up-and-recover))
1. Så här säkerhetskopierar du AEM-databasen offline:

   1. För varje klusternod säkerhetskopierar du filen som innehåller klusternods-ID:t.
   1. Säkerhetskopiera alla filer i en sekundär klusternod, inklusive underkataloger.
   1. Säkerhetskopiera repository/system.id av varje klusternod separat.

   Mer information finns i [Säkerhetskopiera och återställ](/help/sites-administering/backup-and-restore.md).

1. Säkerhetskopiera alla andra data, till exempel kundens typsnitt.
1. Starta klustret igen.

### Onlinesäkerhetskopiering utan driftstopp och fördröjning {#online-backup-with-no-downtime-but-delay-in-response}

1. Ange läget för rullande säkerhetskopiering. (se [Ange säkerhetskopieringslägen](/help/forms/using/admin-help/backing-aem-forms-data.md#entering-the-backup-modes))

   Lämna läget för rullande säkerhetskopiering efter en återställning.

1. Stäng någon av de sekundära noderna i klustret för AEM. (se [Starta och stoppa tjänster](/help/forms/using/admin-help/starting-stopping-services.md#starting-and-stopping-services))
1. Säkerhetskopiera databasen, GDS och Connectors på alla noder. (se [Filer som ska säkerhetskopieras och återställas](/help/forms/using/admin-help/files-back-recover.md#files-to-back-up-and-recover))
1. Så här säkerhetskopierar du AEM-databasen online:

   1. För varje klusternod säkerhetskopierar du filen som innehåller Cluster_node.id.
   1. Säkerhetskopiera repository/system.id av varje klusternod separat.
   1. På valfri sekundär nod gör du en onlinesäkerhetskopiering av databasen. Mer information finns i Onlinesäkerhetskopiering.

1. Säkerhetskopiera alla andra data, till exempel kundens typsnitt.
1. Starta klustret igen.

### Säkerhetskopiera Bootstrap egenskapsfil {#back-up-the-bootstrap-properties-file}

När vi skapar ett AEM-kluster skapas en egenskapsfil i programservern för alla sekundära noder. Vi rekommenderar att du säkerhetskopierar Bootstrap egenskapsfil. Du kan hitta filen på följande plats på programservern:

* JBoss®: i katalogen BIN
* WebLogic: i domänkatalogen
* WebSphere®: i profilkatalogen

Säkerhetskopiera filen för ett återställningsscenario för den sekundära AEM-noden och ersätt den på den angivna platsen på programservern, om den har återställts.

## Återställning i en klustrad miljö {#recovery-in-a-clustered-environment}

Om fel uppstår i hela klustret eller en enda nod återställer du det med hjälp av säkerhetskopian.

Om du vill återställa en enskild nod stänger du den enskilda noden och kör återställningsproceduren för en enda nod.

Om hela klustret inte fungerar på grund av fel som t.ex. databaskrascher utför du följande steg. Återställning beror på vilken säkerhetskopieringsmetod som används.

### Återställa en enskild nod {#restoring-a-single-node}

1. Stoppa den skadade noden.

   >[!NOTE]
   >
   >Om den skadade noden är en primär AEM-nod stänger du av hela klusternoden.

1. Återskapa det fysiska systemet från en systemavbildning.
1. Använd patchar eller uppdateringar i AEM-formulär som har använts sedan bilden skapades. Denna information registrerades under säkerhetskopieringen. AEM-formulär måste återställas till samma nivå som när systemet säkerhetskopierades.
1. (*Valfritt*) Om alla andra noder fungerar som de ska kan även AEM-databasen vara skadad. I så fall visas ett osynkroniserat databasmeddelande i filen error.log i AEM-databasen.

   Så här återställer du databasen:

   >[!NOTE]
   >
   >Om en zippad crx-database backup togs online, packa upp den på valfri plats och följ processen för offlineåterställning.

   1. Ta bort katalogerna för databasen, delade arbetsytor, versioner och arbetsytor i katalogen ClusterNode i noden.
   1. Återställ säkerhetskopian av klusternoden (inklusive underkataloger) till noden.
   1. Ta bort filen clusterNode/revision.log på noden.
   1. Ta bort .lock-koden på noden, om sådan finns.
   1. Ta bort repository/system.id på noden, om det finns någon.
   1. Ta bort filerna &ast;&ast;/listener.properties på noden, om sådan finns.
   1. Återställ repository/cluster_node.id för enskilda klusternoder.

>[!NOTE]
>
>Tänk på följande:

* Om den misslyckade noden var en primär AEM-nod kopierar du allt innehåll från den sekundära databasmappen (crx-database\crx.0000 där 000 kan vara valfri siffra) till databasmappen crx-database och tar bort den sekundära databasmappen.
* Innan du startar om en klusternod måste du ta bort databasen /clustered.txt från den primära noden.
* Se till att den primära noden startas först och starta sedan andra noder när den har startats.

### Återställer hela klustret {#restoring-the-entire-cluster}

1. Stoppa alla klusternoder.
1. Återskapa det fysiska systemet från en systemavbildning.
1. Använd patchar eller uppdateringar i AEM-formulärAEM-formulär som har använts sedan bilden skapades. Denna information registrerades i steg 1 av säkerhetskopieringsproceduren. AEM-formulär måste återställas till samma nivå som när systemet säkerhetskopierades.
1. Återställ databasen, GDS och Connectors.
1. Så här återställer du AEM-databasen offline:

   >[!NOTE]
   >
   >Om en zippad crx-database backup togs online, packa upp den på valfri plats och följ processen för offlineåterställning.

   1. På alla klusternoder tar du bort katalogerna för databasen, delade arbetsytor, versioner och arbetsytor i mappen ClusterNode.
   1. Ta bort alla filer och kataloger i den delade katalogen.
   1. Återställ säkerhetskopian av klusternoden (inklusive underkataloger) till en klusternod.
   1. Kopiera alla filer i den återställda klusternoden till alla andra klusternoder. När du är klar innehåller varje klusternod samma data.
   1. Ta bort filen clusterNode/revision.log på alla klusternoder.
   1. Ta bort .lock på alla klusternoder, om det finns.
   1. Ta bort repository/system.id alla klusternoder, om sådana finns.
   1. Ta bort filerna &ast;&ast;/listener.properties på alla klusternoder, om sådana finns.
   1. Återställ repository/cluster_node.id för enskilda klusternoder.

>[!NOTE]
>
>Tänk på följande:

* Om den misslyckade noden var en primär AEM-nod kopierar du allt innehåll från den sekundära databasmappen (den ser ut som crx-database\crx.0000, där 000 kan vara vilken siffra som helst) till databasmappen crx-database.
* Innan du startar om en klusternod måste du ta bort databasen /clustered.txt från den primära noden.
* Se till att den primära noden startas först och starta sedan andra noder när den har startats.

## Säkerhetskopiera och återställa publiceringsnoden för Correspondence Management Solution {#back-up-and-restore-correspondence-management-solution-publish-node}

Utgivarnoden har ingen primär-sekundär relation i en klustrad miljö. Du kan säkerhetskopiera alla Publisher-noder genom att följa [Säkerhetskopiera och återställ](/help/sites-administering/backup-and-restore.md).

### Återställa en enskild utgivarnod {#recover-a-single-publisher-node}

1. Stäng noden som måste återställas och gör ingen publiceringsaktivitet förrän noden är uppe igen.
1. Återställ publiceringsnoden med [Återställer säkerhetskopian](/help/sites-administering/backup-and-restore.md).

### Återställa ett kluster {#recover-a-cluster}

1. Stäng av klustret.
1. Återställ publiceringsnoden med [Återställer säkerhetskopian](/help/sites-administering/backup-and-restore.md).
1. Starta den primära noden följt av den sekundära noden i utvecklarklustret.
