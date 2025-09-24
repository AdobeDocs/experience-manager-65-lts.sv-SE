---
title: Uppgraderingsförfarande
description: Läs om hur du uppgraderar Adobe Experience Manager (AEM).
contentOwner: sarchiz
topic-tags: upgrading
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
docset: aem65
targetaudience: target-audience upgrader
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 255ef365-0da5-4bc9-b099-2e3bc67dd25a
source-git-commit: 57bf39aa914bddca05d526b46b581579965069d6
workflow-type: tm+mt
source-wordcount: '820'
ht-degree: 0%

---

# Uppgraderingsförfarande {#upgrade-procedure}

>[!NOTE]
>
>Uppgraderingen kräver driftstopp för Author-nivån eftersom de flesta uppgraderingar av Adobe Experience Manager (AEM) utförs på plats. Genom att följa dessa rutiner kan du minimera eller eliminera driftstopp i publiceringsskiktet.

När du uppgraderar dina AEM-miljöer måste du ta hänsyn till skillnaderna i strategi mellan att uppgradera författarmiljöer eller publiceringsmiljöer för att minimera driftstoppen för både författare och slutanvändare. På den här sidan beskrivs hur du uppgraderar en AEM-topologi som för närvarande körs på en version av AEM 6.x. Eftersom processen skiljer mellan redigerings- och publiceringsnivåer och Mongo- och tarMK-baserade distributioner har varje nivå och mikrokärna listats i ett separat avsnitt. När du utför en distribution rekommenderar Adobe att du först uppgraderar din utvecklingsmiljö, avgör om du lyckas och sedan fortsätter till publiceringsmiljöerna.

## Stjärtmaskförfattarnivå {#tarmk-author-tier}

### Startopologi {#starting-topology}

Den topologi som antas för det här avsnittet består av en författarserver som körs på tarMK med ett Cold Standby. Replikering sker från författarservern till TjärMK-publiceringsservergruppen. Även om det inte visas här kan den här metoden även användas för distributioner som använder avlastning. Se till att du uppgraderar eller återskapar avlastningsinstansen på den nya versionen efter att du har inaktiverat replikeringsagenter på författarinstansen och innan du återaktiverar dem.

![target_starting_topology](assets/tarmk_starting_topology.jpg)

### Förberedelse av uppgradering {#upgrade-preparation}

![upgrade-prepare-author](assets/upgrade-preparation-author.png)

1. Stoppa framtagning av innehåll.

1. Stoppa vänteläget.

1. Inaktivera replikeringsagenter på författaren.

1. Kör underhållsaktiviteterna [före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md).

### Uppgraderingskörning {#upgrade-execution}

![execute_upgrade](assets/execute_upgrade.jpg)

1. Kör uppgraderingen [på plats](/help/sites-deploying/in-place-upgrade.md).
1. Uppdatera Dispatcher-modulen *vid behov*.

1. QA validerar uppgraderingen.

1. Stäng författarinstansen.

### Om slutförd {#if-successful}

![if_success](assets/if_successful.jpg)

1. Kopiera den uppgraderade instansen för att skapa ett vänteläge i kallt format.

1. Starta Author-instansen.

1. Starta Standby-instansen.

### Om misslyckades (återställning) {#if-unsuccessful-rollback}

![återställning](assets/rollback.jpg)

1. Starta vänteläget som nytt primärt.

1. Bygg om redigeringsmiljön från vänteläget Cold.

## Författarkluster för MongoMK {#mongomk-author-cluster}

### Startopologi {#starting-topology-1}

Den topologi som antas för det här avsnittet består av ett MongoMK Author-kluster med minst två AEM Author-instanser, understött av minst två MongoMK-databaser. Alla författarinstanser delar ett datalager. De här stegen bör gälla för både S3- och File-datalager. Replikering sker från författarservrarna till TjärMK-publiceringsservergruppen.

![mongo-topology](assets/mongo-topology.jpg)

### Förberedelse av uppgradering {#upgrade-preparation-1}

![mongo-upgrade_prep](assets/mongo-upgrade_prep.jpg)

1. Stoppa framtagning av innehåll.
1. Klona datalagret för säkerhetskopiering.
1. Stoppa alla instanser utom en av AEM Author, din primära författare.
1. Ta bort alla MongoDB-noder utom en från replikuppsättningen, den primära Mongo-instansen.
1. Uppdatera filen `DocumentNodeStoreService.cfg` på den primära författaren så att den återspeglar replikuppsättningen för en enskild medlem.
1. Starta om den primära författaren för att säkerställa att den startar om ordentligt.
1. Inaktivera replikeringsagenter på den primära författaren.
1. Kör [underhållsaktiviteter som är före uppgradering](/help/sites-deploying/pre-upgrade-maintenance-tasks.md) på den primära författarinstansen.

### Uppgraderingskörning {#Upgrade-execution-1}

![mongo-execution](assets/mongo-execution.jpg)

1. Kör en [på plats-uppgradering](/help/sites-deploying/in-place-upgrade.md) på den primära författaren.
1. Uppdatera Dispatcher- eller webbmodulen *vid behov*.
1. QA validerar uppgraderingen.

### Om slutförd {#if-successful-1}

![mongo-Secondaries](assets/mongo-secondaries.jpg)

1. Skapa nya instanser av AEM 6.5 LTS Author, anslutna till den uppgraderade Mongo-instansen.

1. Återskapa de MongoDB-noder som tagits bort från klustret.

1. Uppdatera `DocumentNodeStoreService.cfg`-filerna så att de återspeglar hela replikuppsättningen.

1. Starta om Author-instanserna, en åt gången.

1. Ta bort det klonade datalagret.

### Om misslyckades (återställning)  {#if-unsuccessful-rollback-2}

![mongo-rollback](assets/mongo-rollback.jpg)

1. Konfigurera om de sekundära författarinstanserna för att ansluta till det klonade datalagret.

1. Stäng den uppgraderade primära författarinstansen.

1. Stäng den uppgraderade primära instansen av Mongo.

1. Starta de sekundära Mongo-instanserna med en av dem som den nya primära instansen.

1. Konfigurera `DocumentNodeStoreService.cfg`-filerna på de sekundära författarinstanserna så att de pekar på replikuppsättningen med ännu inte uppgraderade Mongo-instanser.

1. Starta de sekundära författarinstanserna.

1. Rensa de uppgraderade författarinstanserna, Mongo-noden och datalagret.

## TjärMK-publiceringsgrupp {#tarmk-publish-farm}

### TjärMK-publiceringsgrupp {#tarmk-publish-farm-1}

Den topologi som antas för det här avsnittet består av två TjärMK-publiceringsinstanser, framtagna av Dispatchers, som i sin tur står framför en belastningsutjämnare. Replikering sker från författarservern till TjärMK-publiceringsservergruppen.

![target-pub-farv5](assets/tarmk-pub-farmv5.png)

### Uppgraderingskörning {#upgrade-execution-2}

![upgrade-publish2](assets/upgrade-publish2.png)

1. Stoppa trafiken till Publish 2-instansen vid belastningsutjämnaren.
1. Kör [föruppgraderingsunderhåll](/help/sites-deploying/pre-upgrade-maintenance-tasks.md) vid publicering 2.
1. Kör en [på plats-uppgradering](/help/sites-deploying/in-place-upgrade.md) vid publicering 2.
1. Uppdatera Dispatcher- eller webbmodulen *vid behov*.
1. Töm cacheminnet för Dispatcher.
1. QA validerar Publish 2 via Dispatcher bakom brandväggen.
1. Stäng publicering 2.
1. Kopiera Publish 2-instansen.
1. Starta Publicera 2.

### Om slutförd {#if-successful-2}

![upgrade-publish1](assets/upgrade-publish1.png)

1. Aktivera trafik till Publish 2.
1. Stoppa trafiken till Publish 1.
1. Stoppa Publish 1-instansen.
1. Ersätt Publish 1-instansen med en kopia av Publish 2.
1. Uppdatera Dispatcher- eller webbmodulen *vid behov*.
1. Töm Dispatcher-cachen för Publicera 1.
1. Starta Publicera 1.
1. QA validerar Publish 1 via Dispatcher bakom brandväggen.

### Om misslyckades (återställning) {#if-unsuccessful-rollback-1}

![pub_rollback](assets/pub_rollback.jpg)

1. Skapa en kopia av Publish 1.
1. Ersätt Publish 2-instansen med en kopia av Publish 1.
1. Töm Dispatcher-cachen för Publish 2.
1. Starta Publicera 2.
1. QA validerar Publish 2 via Dispatcher bakom brandväggen.
1. Aktivera trafik till Publish 2.

## Slutliga uppgraderingssteg {#final-upgrade-steps}

1. Aktivera trafik till Publish 1.
1. QA utför slutlig validering från en offentlig URL.
1. Aktivera replikeringsagenter från redigeringsmiljön.
1. Återuppta redigering av innehåll.
1. Utför [efteruppgraderingskontroller](/help/sites-deploying/post-upgrade-checks-and-troubleshooting.md).

![final](assets/final.jpg)
