---
title: Granskningsloggunderhåll i AEM 6
description: Läs mer om underhåll av granskningsloggar i Adobe Experience Manager (AEM).
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: operations
content-type: reference
feature: Operations
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '605'
ht-degree: 0%

---

# Granskningsloggunderhåll i AEM 6{#audit-log-maintenance-in-aem}

AEM-händelser som berättigar till granskningsloggning genererar mycket arkiverade data. Dessa data kan snabbt växa med tiden på grund av replikeringar, överföringar av resurser och andra systemaktiviteter.

Underhållet i granskningsloggen innehåller flera delar av funktionaliteten som gör det möjligt att automatisera underhåll av granskningsloggar enligt specifika policyer.

Den implementeras som en konfigurerbar underhållsåtgärd varje vecka och är tillgänglig via kontrollkonsolen för Operations Dashboard.

Mer information finns i dokumentationen för [instrumentpanelen för åtgärder](/help/sites-administering/operations-dashboard.md).

Det finns tre typer av rensningsalternativ för granskningslogg:

1. [Rensning av sidgranskningslogg](/help/sites-administering/operations-audit-log.md#configure-page-audit-log-purging)
1. [Rensa granskningslogg för DAM](/help/sites-administering/operations-audit-log.md#configure-dam-audit-log-purging)
1. [Rensning av replikeringsgranskningslogg](/help/sites-administering/operations-audit-log.md#configure-replication-audit-log-purging)

Du kan konfigurera var och en genom att skapa regler i AEM Web Console. När de har konfigurerats kan du utlösa dem genom att gå till **Verktyg - Åtgärder - Underhåll - Veckounderhåll** och köra **underhållsaktiviteten för granskningsloggen**.

## Konfigurera rensning av sidgranskningslogg {#configure-page-audit-log-purging}

Så här konfigurerar du rensning av granskningslogg:

1. Gå till Web Console Admin genom att peka webbläsaren till `http://localhost:4502/system/console/configMgr/`

1. Sök efter ett objekt med namnet **Sidor, regel för rensning av granskningslogg** och klicka på det.

   ![chlimage_1-365](assets/chlimage_1-365.png)

1. Konfigurera sedan schemaläggaren för rensning enligt dina krav. De tillgängliga alternativen är:

   * **Regelnamn:** namnet på granskningspolicyregeln;
   * **Innehållssökväg:** sökvägen för innehållet som regeln gäller för;
   * **Minimiålder:** den tid i dagar som granskningsloggarna måste sparas.
   * **Granskningsloggtyp:** den typ av granskningslogg som ska rensas.

   >[!NOTE]
   >
   >Innehållssökvägen gäller bara för underordnade noder till `/var/audit/com.day.cq.wcm.core.page` i databasen.

1. Spara regeln.
1. Regeln som du skapade måste visas på kontrollpanelen för åtgärder för att den ska kunna köras. Det gör du genom att gå till **Verktyg - Åtgärder - underhåll** på välkomstskärmen i AEM.

1. Tryck på **veckounderhållskortet**.

1. Underhållsuppgiften finns redan under kortet **AuditLog Maintenance Task**.

   ![chlimage_1-366](assets/chlimage_1-366.png)

1. Du kan antingen kontrollera datumet för nästa körning, konfigurera det eller köra det manuellt genom att trycka på uppspelningsknappen.

I AEM 6.3 stoppas aktiviteten automatiskt om det schemalagda underhållsfönstret stängs innan rensningsaktiviteten för granskningslogg kan slutföras. Den återupptas när nästa underhållsfönster öppnas.

**Med AEM 6.5** kan du stoppa en rensningsaktivitet för granskningslogg manuellt genom att klicka på ikonen **Stoppa** . Nästa körning innebär att uppgiften återupptas.

>[!NOTE]
>
>Att stoppa underhållsaktiviteten innebär att avbryta körningen utan att förlora spårning av det pågående jobbet.

## Konfigurera rensning av DAM-granskningslogg {#configure-dam-audit-log-purging}

1. Gå till systemkonsolen på *https://&lt;serveradress>:&lt;serverport>/system/console/configMgr*
1. Sök efter regeln **Rensa DAM-granskningslogg** och klicka på resultatet.
1. Konfigurera regeln i nästa fönster. Alternativen är:

   * **Regelnamn:** namnet på granskningspolicyregeln;
   * **Innehållssökväg:** sökvägen för innehållet som regeln gäller för
   * **Minimiålder:** den tid i dagar som granskningsloggarna måste behållas
   * **Händelsetyper för granskningslogg:** de typer av DAM-granskningshändelser som ska rensas.

1. Klicka på **Spara** för att spara konfigurationen

## Konfigurera rensning av replikeringsgranskningslogg  {#configure-replication-audit-log-purging}

1. Gå till systemkonsolen på *https://&lt;serveradress>:&lt;serverport>/system/console/configMgr*
1. Sök efter **Schemaläggaren för rensning av replikeringsgranskningslogg** och klicka på resultatet
1. Konfigurera regeln i nästa fönster. Alternativen är:

   * **Regelnamn:** namnet på granskningspolicyregeln
   * **Innehållssökväg:** sökvägen för innehållet som regeln gäller för
   * **Minimiålder:** den tid i dagar som granskningsloggarna måste behållas
   * **Händelsetyper för granskningsloggens replikering:** typerna av replikeringsgranskningshändelser som ska rensas

1. Klicka på **Spara** för att spara konfigurationen.
