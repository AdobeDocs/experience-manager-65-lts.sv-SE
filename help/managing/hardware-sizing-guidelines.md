---
title: Riktlinjer för maskinvarans storlek
description: Dessa riktlinjer för storleksändring ger en uppskattning av de maskinvaruresurser som krävs för att driftsätta ett AEM-projekt.
solution: Experience Manager, Experience Manager 6.5
feature: Compliance
role: Developer,Leader
exl-id: dac9b87a-cbd2-49e3-bd4d-ebcccdec1659
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 0%

---

# Riktlinjer för maskinvarans storlek{#hardware-sizing-guidelines}

Dessa riktlinjer för storleksändring ger en uppskattning av de maskinvaruresurser som krävs för att driftsätta ett AEM-projekt. Beräkningar av storleken beror på projektets arkitektur, lösningens komplexitet, förväntad trafik och projektkraven. Den här guiden hjälper dig att fastställa maskinvarubehoven för en viss lösning eller att hitta en övre och nedre uppskattning av maskinvarukraven.

Grundläggande faktorer att beakta är (i denna ordning):

* **Nätverkshastighet**

   * Nätverksfördröjning
   * Tillgänglig bandbredd

* **Datorhastighet**

   * Cacheeffektivitet
   * Förväntad trafik
   * Komplexa mallar, applikationer och komponenter
   * Samtidiga författare
   * Redigeringsåtgärdens komplexitet (enkel innehållsredigering, MSM-utrullning osv.)

* **I/O-prestanda**

   * Prestanda och effektivitet för fil- eller databaslagring

* **Hårddisk**

   * minst två eller tre gånger större än databasstorleken

* **Minne**

   * Webbplatsens storlek (antal objekt, sidor och användare)
   * Antal användare/sessioner som är aktiva samtidigt

## Arkitektur {#architecture}

En typisk AEM-konfiguration består av en författare och en publiceringsmiljö. De här miljöerna har olika krav på den underliggande maskinvarans storlek och systemkonfiguration. Detaljerade överväganden för båda miljöerna beskrivs i avsnitten [författarmiljö](/help/managing/hardware-sizing-guidelines.md#author-environment-specific-calculations) och [publiceringsmiljö](/help/managing/hardware-sizing-guidelines.md#publish-environment-specific-calculations).

I en typisk projektkonfiguration har du flera miljöer där du ska fasa ut projektet:

* **Utvecklingsmiljö**
Om du vill utveckla nya funktioner eller göra betydande ändringar. Bästa praxis är att arbeta i en utvecklingsmiljö per utvecklare (lokala installationer på deras personliga system).

* **Redigeringstestmiljö**
Verifiera ändringar. Antalet testmiljöer kan variera beroende på projektkraven (t.ex. separat för kvalitetskontroll, integrationstestning eller testning av användaracceptans).

* **Publicera testmiljö**
Detta gäller främst för testning av användningsfall för socialt samarbete och/eller interaktionen mellan författare och flera publiceringsinstanser.

* **Författarproduktionsmiljö**
För författare som vill redigera innehåll.

* **Publicera produktionsmiljö**
För publicerat innehåll.

Miljöerna kan dessutom variera, från ett enserversystem som kör AEM och en programserver till en mycket skalad uppsättning multiserverinstanser med flera CPU-instanser. Adobe rekommenderar att du använder en separat dator för varje produktionssystem och att du inte kör andra program på dessa datorer.

## Allmän hänsyn till maskinvarustorlek {#generic-hardware-sizing-considerations}

Avsnitten nedan ger vägledning om hur maskinvarukraven ska beräknas, med beaktande av olika överväganden. För stora system föreslår Adobe att du utför en enkel uppsättning interna prestandatester på en referenskonfiguration.

Prestandaoptimering är en grundläggande uppgift som måste utföras innan det går att utföra riktmärkning för ett visst projekt. Var noga med att följa anvisningarna i [dokumentationen för prestandaoptimering](/help/sites-deploying/configuring-performance.md) innan du utför några prestandatester och använder resultaten för beräkningar av maskinvarustorlek.

Krav på maskinvarustorlek för fall med avancerad användning måste baseras på en detaljerad prestandautvärdering av projektet. Karakteristika för avancerade användningsområden som kräver exceptionella maskinvaruresurser omfattar följande kombinationer:

* nyttolast/dataflöde för högt innehåll
* omfattande användning av anpassad kod, anpassade arbetsflöden eller tredjepartsprogrambibliotek
* integrering med externa system som inte stöds

## Diskutrymme/hårddisk {#disk-space-hard-drive}

Det diskutrymme som krävs beror till stor del på både volymen och typen av webbprogram. Beräkningarna ska ta hänsyn till följande:

* mängden och storleken på sidor, resurser och andra databaslagrade enheter som arbetsflöden, profiler och så vidare.
* den uppskattade frekvensen av innehållsändringar och därmed skapandet av innehållsversioner
* mängden DAM-resursåtergivningar som ska genereras
* den totala innehållstillväxten över tiden

Diskutrymmet övervakas kontinuerligt under rensning online och offline. Om det tillgängliga diskutrymmet skulle sjunka under ett kritiskt värde avbryts processen. Det kritiska värdet är 25 % av databasens aktuella diskutrymme och kan inte konfigureras. Adobe rekommenderar att storleken på disken är minst två eller tre gånger större än databasstorleken, inklusive den beräknade tillväxten.

### Virtualisering {#virtualization}

AEM fungerar bra i virtualiserade miljöer, men det kan finnas faktorer som CPU eller I/O som inte direkt kan jämföras med fysisk maskinvara. En rekommendation är att välja en högre I/O-hastighet (i allmänhet) eftersom detta vanligtvis är en kritisk faktor. Det är nödvändigt att testa miljön för att få en mer detaljerad förståelse för vilka resurser som krävs.

### Parallalisering av AEM-instanser {#parallelization-of-aem-instances}

**Felskydd**

En felsäker webbplats används i minst två separata system. Om ett system kraschar kan ett annat system ta över och därmed kompensera för systemfelet.

**Skalbarhet för systemresurser**

Alla system körs, men det finns bättre datorprestanda. Den extra prestandan är inte nödvändigtvis linjär med antalet klusternoder eftersom relationen är mycket beroende av den tekniska miljön. Mer information finns i [Klusterdokumentation](/help/sites-deploying/recommended-deploys.md).

Beräkningen av hur många klusternoder som behövs baseras på de grundläggande kraven och specifika användningsfall för det aktuella webbprojektet:

* När det gäller felsäkerhet är det nödvändigt att för alla miljöer fastställa hur allvarligt felet är och hur lång tid det tar för en klusternod att återställa felet.
* När det gäller skalbarhet är antalet skrivåtgärder den viktigaste faktorn. Belastningsutjämning kan upprättas för åtgärder som enbart använder systemet för att bearbeta läsåtgärder. Mer information finns i [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html).

### Maskinvarurekommendationer {#hardware-recommendations}

Vanligtvis kan du använda samma maskinvara för din författarmiljö som du rekommenderas för din publiceringsmiljö. Vanligtvis är webbtrafiken lägre i redigeringssystemen, men cacheeffektiviteten är också lägre. Den grundläggande faktorn här är dock antalet författare som arbetar parallellt, tillsammans med den typ av åtgärder som görs i systemet. I allmänhet är AEM-klustring (i författarmiljön) mest effektivt vid skalning av läsåtgärder, med andra ord kan ett AEM-kluster skalas bra tillsammans med författare som utför grundläggande redigeringsåtgärder.

## Ytterligare fallspecifika beräkningar {#additional-use-case-specific-calculations}

Förutom beräkningen för ett standardwebbprogram bör du ta hänsyn till specifika faktorer för följande användningsområden. De beräknade värdena ska läggas till i standardberäkningen.

### Assets-specifika överväganden {#assets-specific-considerations}

Omfattande bearbetning av digitala resurser kräver optimerade maskinvaruresurser, de viktigaste faktorerna är bildstorlek och högsta genomströmning för bearbetade bilder.

Allokera minst 16 GB stackutrymme och konfigurera arbetsflödet [!UICONTROL DAM Update Asset] så att det använder [Camera Raw-paketet](/help/assets/camera-raw.md) för konsumtion av råbilder.

>[!NOTE]
>
>Ett högre bildflöde innebär att datorresurserna måste kunna hålla jämna steg med I/O-systemet och omvänt. Om arbetsflöden till exempel startas vid import av bilder kan överföringen av många bilder via WebDAV orsaka en eftersläpning i arbetsflödena.
>
>Om du använder separata diskar för tarPM, datalager och sökindex kan det hjälpa till att optimera I/O-beteendet för systemet (men vanligtvis är det bra att behålla sökindexet lokalt).

>[!NOTE]
>
>Se även [Assets Performance Guide](/help/sites-deploying/assets-performance-sizing.md).

### Hanterare för flera platser {#multi-site-manager}

Resursanvändningen när du använder AEM MSM i en redigeringsmiljö beror till stor del på de specifika användningsområdena. De grundläggande faktorerna är:

* Antal live-kopior
* Periodicitet för utrullningar
* Innehållsträdets storlek som ska rullas ut
* Anslutna funktioner för utrullningsåtgärderna

Genom att testa det planerade användningsexemplet med ett representativt utdrag kan du få en bättre förståelse för resursanvändningen. Om du extrapolerar resultaten med det planerade dataflödet kan du utvärdera de ytterligare resurser som krävs för AEM MSM.

Ta även hänsyn till parallella författare. De upplever prestandabiverkningar om AEM MSM använder fler resurser än planerat.
