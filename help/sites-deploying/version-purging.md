---
title: Rensning av version
description: I den här artikeln beskrivs de tillgängliga alternativen för att rensa versioner.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
docset: aem65
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: e3ef1435-d405-482f-9eb5-f9a64ff03322
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# Rensning av version{#version-purging}

I en standardinstallation skapar Adobe Experience Manager (AEM) en version av en sida eller nod när du aktiverar en sida efter att innehållet har uppdaterats.

>[!NOTE]
>
>Om inga innehållsändringar görs visas ett meddelande om att sidan har aktiverats, men ingen ny version skapas.

Du kan skapa ytterligare versioner på begäran på fliken **Versioning** i sidosparken. Dessa versioner lagras i databasen och kan återställas om det behövs.

Dessa versioner rensas aldrig, så databasstorleken ökar med tiden och måste därför hanteras.

AEM levereras med olika mekanismer som hjälper dig att hantera din databas:

* [Versionshanteraren](#version-manager)
Detta kan konfigureras för att rensa gamla versioner när nya versioner skapas.

* verktyget [Rensa versioner](/help/sites-deploying/monitoring-and-maintaining.md#purgeversionstool)
Detta används som en del av övervakningen och underhållet av din databas.
Du kan ingripa för att ta bort gamla versioner av en nod, eller en hierarki av noder, enligt följande parametrar:

   * Det maximala antalet versioner som ska behållas i databasen.
När den här siffran överskrids tas den äldsta versionen bort.

   * Högsta ålder för alla versioner som lagras i databasen.
När en versions ålder överskrider det här värdet rensas den från databasen.

* underhållsaktiviteten [Version Rensa ](/help/sites-administering/operations-dashboard.md#automated-maintenance-tasks). Du kan schemalägga underhållsaktiviteten Rensa version så att tidigare versioner tas bort automatiskt. Därför minimeras behovet av att manuellt använda verktygen för versionsrensning.

>[!CAUTION]
>
>Optimera databasstorleken genom att köra versionsrensningen ofta. Uppgiften bör schemaläggas utanför kontorstid när trafiken är begränsad.

## Versionshanteraren {#version-manager}

Förutom explicit rensning med rensningsverktyget kan Version Manager konfigureras för att rensa gamla versioner när nya versioner skapas.

[Skapa en konfiguration](/help/sites-deploying/configuring-osgi.md) för:

`PID com.day.cq.wcm.core.impl.VersionManagerImpl`

Följande alternativ är tillgängliga:

* `versionmanager.createVersionOnActivation` (Boolean, standard: true)
Anger om en version ska skapas när sidor aktiveras.
En version skapas såvida inte replikeringsagenten har konfigurerats för att förhindra att versioner skapas, vilket stöds av versionshanteraren.
En version skapas bara om aktiveringen sker på en sökväg som finns i `versionmanager.ivPaths` (se nedan).

* `versionmanager.ivPaths`(String[], default: `{"/"}`)
Anger sökvägarna som versionerna implicit skapas på vid aktivering om `versionmanager.createVersionOnActivation` är true.

* `versionmanager.purgingEnabled` (Boolean, standard: false)
Definierar om rensning ska aktiveras när nya versioner skapas.

* `versionmanager.purgePaths` (String[], standard: {&quot;/content&quot;})
Anger på vilka sökvägar versioner ska rensas när nya versioner skapas.

* `versionmanager.maxAgeDays` (int, standard: 30)
Vid versionsrensning tas alla versioner som är äldre än det konfigurerade värdet bort. Om värdet är mindre än 1 utförs inte rensning baserat på versionens ålder.

* `versionmanager.maxNumberVersions` (int, standard 5)
Vid versionsrensning tas alla versioner som är äldre än den n:te nyaste versionen bort. Om värdet är mindre än 1 utförs inte rensning baserat på antalet versioner.

* `versionmanager.minNumberVersions` (int, standard 0)
Det minsta antalet versioner som behålls oavsett ålder. Om värdet är mindre än 1 behålls inget minsta antal versioner.

>[!NOTE]
>
>Vi rekommenderar inte att du behåller många versioner i databasen. När du konfigurerar versionsrensningsåtgärden bör du därför tänka på att inte utesluta för många versioner från rensningen, eftersom databasstorleken då inte är korrekt optimerad. Om du har ett stort antal versioner på grund av verksamhetskrav kontaktar du Adobe support för att hitta andra sätt att optimera databasstorleken.

### Kombinera kvarhållningsalternativ {#combining-retention-options}

Alternativen som definierar hur versioner ska behållas ( `maxAgeDays`, `maxNumberVersions`, `minNumberVersions`) kan kombineras beroende på dina behov.

Om du till exempel definierar det maximala antalet versioner som ska behållas OCH den äldsta versionen som ska behållas:

* Inställning:

   * `maxNumberVersions` = 7

   * `maxAgeDays` = 30

* Med:

   * Tio versioner gjordes de senaste 60 dagarna
   * Tre av dessa versioner skapades de senaste 30 dagarna

* Det innebär att

   * De tre senaste versionerna bevaras

När du definierar t.ex. det högsta OCH lägsta antalet versioner som ska behållas OCH den äldsta versionen som ska behållas:

* Inställning:

   * `maxNumberVersions` = 3
   * `maxAgeDays` = 30
   * `minNumberVersions` = 3

* Med:

   * Fem versioner gjordes för 60 dagar sedan

* Det innebär att

   * Tre versioner behålls

## Rensa versioner {#purge-versions-tool}

Verktyget [Rensa versioner](/help/sites-deploying/monitoring-and-maintaining.md#purgeversionstool) är avsett att rensa versioner av en nod eller en hierarki av noder i din databas. Dess främsta syfte är att hjälpa dig att minska storleken på databasen genom att ta bort tidigare versioner av dina noder.
