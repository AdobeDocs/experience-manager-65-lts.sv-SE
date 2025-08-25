---
title: Uppgradera kod och anpassningar
description: Läs mer om hur du uppgraderar kod och anpassningar i AEM.
contentOwner: sarchiz
topic-tags: upgrading
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
docset: aem65
targetaudience: target-audience upgrader
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 6b94caf1-97b7-4430-92f1-4f4d0415aef3
source-git-commit: f983fc1edc613feaa070c4e82a92aabab9d50cbb
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 0%

---

# Uppgradera kod och anpassningar{#upgrading-code-and-customizations}

När man planerar en uppgradering måste man undersöka och åtgärda följande områden i en implementering.

* [Uppgradera kodbasen](#upgrade-code-base)
* [Testförfarande](#testing-procedure)

## Ökning {#overview}

1. **AEM Analyzer** - Kör AEM Analyzer enligt beskrivningen i uppgraderingsplaneringen och beskrivs i detalj på sidan [Utvärdera uppgraderingskomplexiteten med AEM Analyzer](/help/sites-deploying/aem-analyzer.md). Du får en AEM Analyzer-rapport som innehåller mer information om områden som måste åtgärdas utöver de otillgängliga API:erna/paketen i målversionen av AEM. AEM Analyzer-rapporten ger dig en indikation på eventuella inkompatibiliteter i koden. Om det inte finns någon är din distribution redan 6.5 LTS-kompatibel. Du kan fortfarande välja att skapa nya funktioner för 6.5 LTS, men du behöver dem inte bara för att bibehålla kompatibiliteten.
1. **Utveckla kodbas för 6.5 LTS**- Skapa en dedikerad gren eller databas för kodbasen för målversionen. Använd information från Kompatibilitet före uppgradering för att planera områden med kod att uppdatera.
1. **Kompilera med 6.5 LTS Uber jar** - Uppdatera kodbas-POM så att de pekar på 6,5 LTS uber jar och kompilera kod mot den.
1. **Distribuera till 6.5 LTS-miljö** - en ren instans av AEM 6.5 LTS (författare + publicering) bör ställas upp i en Dev/QA-miljö. Uppdaterad kodbas och ett representativt urval av innehåll (från aktuell produktion) bör distribueras.
1. **QA-validering och felkorrigering** - QA ska validera programmet både i Author- och Publish-instanser av 6.5 LTS. Eventuella buggar som hittas ska vara åtgärdade och implementerade i 6.5 LTS-kodbasen. Upprepa Dev-Cycle tills alla fel är åtgärdade.

Innan du fortsätter med en uppgradering bör du ha en stabil programkodbas som har testats noggrant mot AEM 6.5 LTS.

## Uppgradera kodbasen {#upgrade-code-base}

### Skapa en dedikerad gren för 6.5 LTS-kod i versionskontrollen {#create-a-dedicated-branch-for-6.5-lts-code-in-version-control}

All kod och alla konfigurationer som krävs för din AEM-implementering bör hanteras med någon form av versionskontroll. En dedikerad gren i versionskontrollen bör skapas för att hantera ändringar som behövs för kodbasen i målversionen av AEM. Interaktiv testning av kodbasen mot målversionen av AEM och efterföljande felkorrigeringar hanteras i den här grenen.

### Uppdatera JAR-versionen av AEM Uber {#update-the-aem-uber-jar-version}

AEM Uber jar innehåller alla AEM API:er som ett enda beroende i Maven-projektets `pom.xml`. Det är alltid en god vana att inkludera Uber Jar som ett enda beroende i stället för att inkludera enskilda AEM API-beroenden. När du uppgraderar kodbasen ska du ändra versionen av Uber Jar till att peka på 6.5 LTS-versionen av AEM. Uppdatera alla inaktuella API:er eller metoder så att de är kompatibla med målversionen av AEM. Kompilera om kodbasen mot den nya versionen av Uber Jar.

```
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.6.0</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

>[!NOTE]
>
>Det finns en viss skillnad i hur AEM 6.5 och AEM 6.5 LTS Uber Jars paketeras. Se avsnittet nedan:

**Uber Jars för AEM 6.5**

1. `uber-jar-6.5.x.jar` - Innehåller alla offentliga API:er för AEM 6.5.
1. `uber-jar-6.5.x-apis-with-deprecations.jar` - Innehåller både publika API:er och inaktuella API:er från AEM 6.5.

**Uber Jars för AEM 6.5 LTS**

För AEM 6.5 LTS finns det ytterligare två typer av Uber Jars:

1. `uber-jar-6.6.x-apis.jar` - Innehåller alla publika API:er för AEM 6.5 LTS.
1. `uber-jar-6.6.x-deprecated-apis.jar` - Inkluderar endast inaktuella API:er från AEM 6.5 LTS.

**Viktig skillnad: AEM 6.5 vs. AEM 6.5 LTS Uber Jars**

* Om både publika och inaktuella API:er behövs i AEM 6.5 kan du använda en enda behållare, `uber-jar-6.5.x-apis-with-deprecations.jar`, i `pom.xml`-filen.
* Om du behöver både publika och inaktuella API:er i AEM 6.5 LTS måste du inkludera två separata anrop, `uber-jar-6.6.x-apis.jar` för publika API:er och `uber-jar-6.6.x-deprecated-apis.jar` för inaktuella API:er.

**Maven-koordinater för borttagna API:er Jar**

```
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.6.0</version>
    <classifier>deprecated-apis</classifier>
    <scope>provided</scope>
</dependency>
```

### Utvecklaranteckningar {#developer-notes}

* AEM 6.5 LTS innehåller inte Google guava-bibliotek som är körklart. Den version som krävs kan installeras enligt önskemål.
* Sling XSS-paket använder nu Java HTML Sanitizer-biblioteket, och metoden `XSSAPI#filterHTML()` bör användas för att återge HTML-innehåll på ett säkert sätt och inte för att skicka data till andra API:er.

## Testförfarande {#testing-procedure}

En omfattande testplan bör utarbetas för testning av uppgraderingar. Testning av den uppgraderade kodbasen och programmet måste göras i lägre miljöer först. Alla buggar som hittas bör fixeras på ett iterativt sätt tills kodbasen är stabil, men endast då bör högnivåmiljöer uppgraderas.

### Testa uppgraderingsproceduren {#testing-upgrade-procedure}

Uppgraderingsproceduren som beskrivs här bör testas i Dev- och QA-miljöer så som de beskrivs i din anpassade körbok (se [Planera din uppgradering](/help/sites-deploying/upgrade-planning.md)). Uppgraderingsproceduren bör upprepas tills alla steg har dokumenterats i uppgraderingsboken och uppgraderingsprocessen är smidig.

### Implementeringstestområden  {#implementation-test-areas-}

Nedan visas viktiga delar av alla AEM-implementeringar som ska ingå i testplanen när miljön har uppgraderats och den uppgraderade kodbasen har driftsatts.

<table>
 <tbody>
  <tr>
   <td><strong>Funktionellt testområde</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>Publicerade webbplatser</td>
   <td>Testar AEM-implementeringen och associerad kod på publiceringsnivån <br /> via Dispatcher. Bör innehålla kriterier för siduppdateringar och <br /> cacheogiltigförklaring.</td>
  </tr>
  <tr>
   <td>Redigering</td>
   <td>Testar AEM-implementeringen och tillhörande kod på författarnivån. Bör innehålla sidor, komponentredigering och dialogrutor.</td>
  </tr>
  <tr>
   <td>Integrering med Experience Cloud Solutions</td>
   <td>Validera integreringar med produkter som Analytics.</td>
  </tr>
  <tr>
   <td>Integrering med tredjepartssystem</td>
   <td>Validera integreringar från tredje part på både författarnivå och publiceringsnivå.</td>
  </tr>
  <tr>
   <td>Autentisering, säkerhet och behörigheter</td>
   <td>Alla autentiseringsmekanismer som LDAP/SAML bör valideras.<br /> Behörigheter och grupper ska testas på både författarnivå och publiceringsnivå <br />.</td>
  </tr>
  <tr>
   <td>Frågor</td>
   <td>Anpassade index och frågor bör testas tillsammans med frågeprestanda.</td>
  </tr>
  <tr>
   <td>Anpassningar av användargränssnitt</td>
   <td>Alla tillägg eller anpassningar av AEM-gränssnittet i författarmiljön.</td>
  </tr>
  <tr>
   <td>Arbetsflöden</td>
   <td>Anpassade arbetsflöden och funktioner och/eller inte.</td>
  </tr>
  <tr>
   <td>Prestandatestning</td>
   <td>Belastningstestning bör utföras på både författarnivå och publiceringsnivå som simulerar verkliga scenarier.</td>
  </tr>
 </tbody>
</table>

### Dokumenttestplan och resultat {#document-test-plan-and-results}

En testplan bör skapas som omfattar de ovannämnda testområdena för implementering. Det är ofta klokt att separera testplanen med uppgiftslistorna Skapat av och Publicera. Testplanen bör köras i Dev-, QA- och Stage-miljöer innan du uppgraderar produktionsmiljöer. Testresultat och prestandamått bör samlas in i lägre miljöer för att ge en jämförelse när du uppgraderar scen- och produktionsmiljöer.
