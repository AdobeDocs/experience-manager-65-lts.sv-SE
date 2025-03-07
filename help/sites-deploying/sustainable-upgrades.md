---
title: Hållbara uppgraderingar
description: Läs om hållbara uppgraderingar i Adobe Experience Manager 6.4.
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: 5a93918b-3b5f-49e0-9283-86776f9d8fb4
source-git-commit: 180fd02df50f84e0d4f9bc01efe56e28d25555e2
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Hållbara uppgraderingar{#sustainable-upgrades}

## Anpassningsramverk {#customization-framework}

### Arkitektur (funktion/infrastruktur/innehåll/program)  {#architecture-functional-infrastructure-content-application}

Customization Framework-funktionen är utformad för att minska antalet fel i icke-utökningsbara områden i koden (som APIS) eller innehåll (som övertäckningar) som inte är uppgraderingsvänliga.

Det finns två komponenter i anpassningsramverket: **API-gränssnittet** och **innehållsklassificeringen**.

#### API-gränssnitt {#api-surface}

I tidigare versioner av Adobe Experience Manager (AEM) exponerades många API:er via Uber Jar. Vissa av dessa API:er var inte avsedda att användas av kunder, men exponerades för stöd för AEM-funktioner i alla paket. Framöver markeras Java™-API:erna som Public eller Private för att visa kunderna vilka API:er som är säkra att använda i samband med uppgraderingar. Andra detaljer är:

* Java™-API:er som markerats som `Public` kan användas och refereras av anpassade implementeringspaket.

* Offentliga API:er är bakåtkompatibla med installationen av ett kompatibilitetspaket.
* Kompatibilitetspaketet innehåller en Uber JAR-kompatibel komponent för att säkerställa bakåtkompatibilitet
* Java™-API:er som markerats som `Private` är endast avsedda att användas av AEM interna paket, inte av anpassade paket.

>[!NOTE]
>
>Konceptet `Private` och `Public` i det här sammanhanget får inte blandas ihop med Java™-förekomster av offentliga och privata klasser.

![image2018-2-12_23-52-48](assets/image2018-2-12_23-52-48.png)

#### Innehållsklassificeringar {#content-classifications}

AEM har sedan länge använt principen om övertäckningar och Sling Resource Merger för att ge kunderna möjlighet att utöka och anpassa AEM funktioner. Fördefinierade funktioner som driver AEM-konsoler och användargränssnitt lagras i **/libs**. Kunder får aldrig ändra något under **/libs** men kan lägga till ytterligare innehåll under **/apps** för att täcka över och utöka de funktioner som definieras i **/libs** (Mer information finns i Utveckla med övertäckningar). Detta orsakade fortfarande många problem vid uppgradering av AEM eftersom innehållet i **/libs** kan ändras och göra att övertäckningsfunktionen bryts på oväntade sätt. Kunder kan också utöka AEM-komponenter genom arv med hjälp av `sling:resourceSuperType`, eller helt enkelt referera en komponent i **/libs** direkt med hjälp av sling:resourceType. Liknande uppgraderingsproblem kan uppstå med referens- och åsidosättningsanvändningsfall.

För att göra det säkrare och enklare för kunderna att förstå vilka områden i **/libs** som är säkra att använda och täcka över innehållet i **/libs** har klassificerats med följande blandningar:

* **Public (granite:PublicArea)** - Definierar en nod som public så att den kan överlappas, ärvas ( `sling:resourceSuperType`) eller användas direkt ( `sling:resourceType`). Noder under /libs markerade som Public (Offentliga) är säkra att uppgradera med ett kompatibilitetspaket. I allmänhet bör kunderna bara använda noder som är markerade som Public (Offentliga).

* **Abstrakt (granite:AbstractArea)** - Definierar en nod som abstrakt. Noder kan överlappas eller ärvas ( `sling:resourceSupertype`) men inte användas direkt ( `sling:resourceType`).

* **Final (granite:FinalArea)** - Definierar en nod som final. Noder som klassificerats som slutliga bör inte överlappas eller ärvas. Slutliga noder kan användas direkt via `sling:resourceType`. Undernoder under den sista noden betraktas som interna som standard.

* ***Internal (granite:InternalArea)*** *- *Definierar en nod som intern. Noder som klassificeras som interna och idealiska får inte överlappas, ärvas eller användas direkt. De här noderna är avsedda endast för AEM interna funktioner

* **Ingen anteckning** - Noderna ärver klassificering baserat på trädhierarkin. Roten / är som standard Public (Offentlig). **Noder med en överordnad som klassificerats som Internal eller Final ska också behandlas som Internal.**

>[!NOTE]
>
>Dessa profiler används endast mot Sling-sökvägsbaserade mekanismer. Andra områden i **/libs** som ett klientbibliotek kan markeras som `Internal`, men kan ändå användas med standardinkludering av klientlib. Det är viktigt att kunden i dessa fall fortsätter att respektera den interna klassificeringen.

#### Indikatorer för CRXDE Lite-innehållstyp {#crxde-lite-content-type-indicators}

Blandningar som används i CRXDE Lite visar innehållsnoder och träd som är markerade som `INTERNAL` som nedtonade (nedtonade). För `FINAL` är det bara ikonen som är nedtonad. De underordnade noderna visas också nedtonade. Funktionen Overlay Node är inaktiverad i båda dessa fall.

**Offentlig**

![image2018-2-8_23-34-5](assets/image2018-2-8_23-34-5.png)

**Slutlig**

![image2018-2-8_23-34-56](assets/image2018-2-8_23-34-56.png)

**Intern**

![image2018-2-8_23-38-23](assets/image2018-2-8_23-38-23.png)

**Hälsokontroll för innehåll**

>[!NOTE]
>
>Från och med AEM 6.5 rekommenderar Adobe att du använder mönsteravkännaren för att upptäcka åtkomstfel för innehåll. Mönsterdetektorrapporter är mer detaljerade, identifierar fler problem och minskar sannolikheten för falska positiva.
>
>Mer information finns i [Utvärdera uppgraderingskomplexiteten med mönsteravkännaren](/help/sites-deploying/pattern-detector.md).

AEM 6.5 levereras med en hälsokontroll som varnar kunderna om överlagrat eller refererat innehåll används på ett sätt som inte stämmer överens med innehållsklassificeringen.

** Kontroll av åtkomsten till innehåll som delas/beviljas** är en ny hälsokontroll som övervakar databasen för att se om kundkoden inte har åtkomst till skyddade noder i AEM.

Detta skannar **/appar** och tar vanligtvis flera sekunder att slutföra.

Så här kommer du åt den nya hälsokontrollen:

1. Gå till **Verktyg > Åtgärder > Hälsorapporter** på AEM hemskärm.
1. Klicka på **Kontroll av åtkomst till innehåll för att dela/bevilja innehåll**.

   ![screen_shot_2017-12-14at55648pm](assets/screen_shot_2017-12-14at55648pm.png)

När sökningen är klar visas en lista med varningar som meddelar slutanvändaren om den skyddade noden som felaktigt refereras till:

![screenshot-2018-2-5healthreports](assets/screenshot-2018-2-5healthreports.png)

När överträdelserna har åtgärdats återgår den till grönt:

![screenshot-2018-2-5healthreports-violits](assets/screenshot-2018-2-5healthreports-violations.png)

Hälsokontrollen visar information som samlats in av en bakgrundstjänst som kontrollerar asynkront när en övertäckning eller resurstyp används i alla Sling-sökvägar. Om innehållsmixiner används felaktigt rapporteras en överträdelse.
