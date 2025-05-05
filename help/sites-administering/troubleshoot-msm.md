---
title: Felsökning av MSM-problem och vanliga frågor
description: Ta reda på hur du felsöker de vanligaste MSM-relaterade problemen och får svar på de vanligaste MSM-relaterade frågorna.
feature: Multi Site Manager
role: Admin
solution: Experience Manager, Experience Manager Sites
exl-id: eca28076-bc91-4a6f-aef8-979ad6f761f7
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Felsökning av MSM-problem och vanliga frågor {#troubleshooting-msm}

## Felsökning av första steg {#first-steps}

Om du upplever något du tycker är felaktigt eller ett fel i MSM ska du se till att du före felsökningen och den detaljerade felsökningen gör så här:

* Kontrollera [MSM FAQ](#faq) eftersom dina problem eller frågor redan kan åtgärdas där.
* Läs artikeln [MSM best practices](msm-best-practices.md) eftersom det finns flera tips där tillsammans med förtydliganden av vissa missuppfattningar.

## Hitta avancerad information om din plan- och Live Copy-status {#advanced-info}

MSM registrerar flera servrar som kan begäras med väljare på resurs-URL:erna. Dessa används av användargränssnittet men kan också begäras direkt för att se ytterligare avancerade beräknade MSM-statusvärden för dina sidor:

1. `http://<host>:<port>/content/path/to/bluprint/page.blueprint.json?&maxSize=500&advancedStatus=true&returnRelationships=true&msm%3Atrigger=ROLLOUT`
   * Använd detta på en ritningssida för att hämta listan över alla Live-kopior som är länkade till den, med ytterligare statusinformation för Live Copy.
   * till exempel:

     `http://localhost:4502/content/wknd/language-masters/en.blueprint.json?&maxSize=500&advancedStatus=true&returnRelationships=true&msm%3Atrigger=ROLLOUT`


1. `http://<host>:<port>/content/path/to/livecopy/page.msm.json`
   * Använd detta på Live Copy-sidor för att hämta avancerad information om deras anslutning till deras ritningssidor. Om sidan inte är en Live-kopia returneras ingenting.
   * till exempel:

     `http://localhost:4502/content/wknd/ca/en.msm.json`

Dessa servrar genererar DEBUG-loggmeddelanden via loggen `com.day.cq.wcm.msm` som också kan vara till hjälp.

## Kontrollera MSM-specifik information i databasen {#checking-repo}

De tidigare servletarna returnerade beräknad information baserat på de MSM-specifika noderna och mixinerna. Informationen lagras i databasen på följande sätt.

* `cq:LiveSync` blandningstyp
   * Detta anges för `jcr:content`-noder och definierar Live Copy-rotsidor.
   * Dessa sidor har en `cq:LiveSyncConfig` underordnad nod av typen `cq:LiveCopy` som innehåller grundläggande och obligatorisk information i Live Copy via följande egenskaper:
      * `cq:master` pekar på Live Copy-sidans utkast.
      * `cq:rolloutConfigs` anger aktiva rollout-konfigurationer som används för Live-kopian.
      * `cq:isDeep` är true om de underordnade sidorna för den här Live Copy-rotsidan inkluderas i Live Copy.
* `cq:LiveRelationship` blandningstyp
   * Alla Live Copy-sidor har en sådan blandningstyp på noden `jcr:content`.
   * Om så inte är fallet har sidan vid något tillfälle kopplats loss eller skapats manuellt via redigeringsgränssnittet utanför en Live Copy-åtgärd (skapa eller rulla ut).
* `cq:LiveSyncCancelled` blandningstyp
   * Lade till `jcr:content` noder med Live Copy-sidor som har pausats.
   * Om uppehållet även gäller för underordnade sidor anges egenskapen `cq:isCancelledForChildren` till true på samma nod.

Informationen i dessa egenskaper bör återspeglas i användargränssnittet, men vid felsökning kan det vara bra att observera MSM-beteenden direkt i databasen när MSM-åtgärder utförs.

Att känna till dessa egenskaper kan också vara användbart för att fråga databasen och få reda på uppsättningar med sidor som är i vissa lägen. Till exempel:

* `select * from cq:LiveSync` returnerar alla Live Copy-rotsidor.

## Vanliga frågor {#faq}

Här är några vanliga frågor om MSM och Live Copy.

### Varför uppdateras inte vissa egenskaper (till exempel titel, anteckningar) under en MSM-utrullning? {#missing-properties}

MSM-synkroniseringsåtgärder är mycket konfigurerbara. Vilka egenskaper eller komponenter som ändras under utrullningar beror direkt på egenskaperna för dessa konfigurationer.

Mer information om det här ämnet finns i [Bästa praxis för MSM](msm-best-practices.md).

### Hur tar jag bort utrullningsbehörigheter för en grupp författare? {#remove-rollout-permissions}

Det finns inget **rollout**-privilegium som kan anges eller tas bort för AEM-objekt (användare eller grupper).

Du kan också:

* Anpassa produktanvändargränssnittet för att dölja rollout-åtgärder för ett givet huvudkonto.
* Ta bort skrivrättigheter från Live Copy-trädet för författare som inte får rulla ut.

### Varför visas Live Copy-sidor med suffixet&quot;_msm_move&quot;? {#moved-pages}

Om en ritningssida introduceras uppdaterar den antingen sin Live Copy-sida eller skapar en ny Live Copy-sida om den inte finns ännu. Till exempel när den rullas ut för första gången eller när sidan Live-kopia togs bort manuellt.

I det senare fallet, om det finns en sida utan egenskapen `cq:LiveRelationship` med samma namn, byter den här sidan namn innan Live Copy-sidan skapas.

Som standard förväntar sig utrullningen en länkad Live Copy-sida, till vilken uppdateringarna av ritningarna rullas ut. Eller så förväntas ingen sida alls när en Live Copy-sida skapas.

Om en fristående sida hittas väljer MSM att byta namn på sidan och skapa en separat länkad Live Copy-sida.

En sådan fristående sida i ett Live Copy-underträd är vanligtvis resultatet av en **Koppla loss** -åtgärd, eller så togs den tidigare Live Copy-sidan bort manuellt av en författare och återskapades med samma namn.

Du undviker detta genom att använda funktionen **Gör uppehåll** i Live-kopian i stället för **Koppla loss**. Mer information om åtgärden **Koppla loss** finns i [den här artikeln.](msm-livecopy.md)
