---
title: Utveckla SPA för Adobe Experience Manager
description: I den här artikeln finns viktiga frågor att tänka på när en frontutvecklare ska utveckla ett SPA för Adobe Experience Manager (AEM) och en översikt över AEM arkitektur för SPA som ska beaktas när en utvecklad SPA distribueras på AEM.
contentOwner: bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: spa
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing,SPA Editor
role: Developer
exl-id: 8670d700-6ccd-4809-b719-8580d6fb2cf8
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2018'
ht-degree: 0%

---

# Utveckla SPA för AEM{#developing-spas-for-aem}

Single page applications (SPAs) can offer compelling experiences for website users. Utvecklare vill kunna skapa webbplatser med SPA-ramverk och författare vill smidigt redigera innehåll i Adobe Experience Manager (AEM) för en webbplats som skapats med sådana ramverk.

I den här artikeln finns viktiga frågor att tänka på när en frontutvecklare ska utveckla ett SPA för AEM och en översikt över AEM arkitektur när det gäller att driftsätta SPA för AEM.

{{ue-over-spa}}

## SPA Development Principles for AEM {#spa-development-principles-for-aem}

Utveckla single page-applikationer på AEM förutsätter att frontutvecklaren följer vedertagna standarder när han skapar en SPA. Om du som gränssnittsutvecklare följer dessa allmänna bästa metoder och några AEM-specifika principer kommer ditt SPA att fungera med [AEM och dess innehållsredigeringsfunktioner](/help/sites-developing/spa-walkthrough.md#content-editing-experience-with-spa).

* **[Portabilitet](/help/sites-developing/spa-architecture.md#portability) -** Precis som med andra komponenter bör komponenterna byggas så att de är så portabla som möjligt. SPA bör byggas med rörliga och återanvändbara komponenter.
* **[AEM Drives Site Structure](/help/sites-developing/spa-architecture.md#aem-drives-site-structure)** - front-end-utvecklaren skapar komponenter och äger sin interna struktur, men använder AEM för att definiera webbplatsens innehållsstruktur.
* **[Dynamisk återgivning](/help/sites-developing/spa-architecture.md#dynamic-rendering)** - All återgivning ska vara dynamisk.
* **[Dynamisk routning](#dynamic-routing) -** SPA ansvarar för routningen och AEM lyssnar på den och hämtar baserat på den. Alla routningar ska också vara dynamiska.

Om du håller dessa principer i åtanke när du utvecklar din SPA-fil kommer den att vara så flexibel och framtidssäker som möjligt samtidigt som alla funktioner som stöds i AEM aktiveras.

Om du inte behöver stöd för AEM-redigeringsfunktioner kan du behöva överväga en annan [SPA-designmodell](/help/sites-developing/spa-architecture.md#spa-design-models).

### Portabilitet {#portability}

Precis som när du utvecklar en komponent bör dina komponenter utformas på ett sådant sätt att de blir så bärbara som möjligt. Mönster som motverkar komponenternas bärbarhet eller återanvändbarhet bör undvikas för att säkerställa kompatibilitet, flexibilitet och underhållbarhet.

Den resulterande produktresumén bör byggas med mycket portabla och återanvändbara komponenter.

### AEM Drives Site Structure {#aem-drives-site-structure}

Utvecklaren måste tänka på sig själv som ansvarig för att skapa ett bibliotek med SPA-komponenter som används för att bygga appen. Utvecklaren har fullständig kontroll över komponenternas interna struktur. [AEM äger dock alltid webbplatsens struktur.](/help/sites-developing/spa-overview.md)

Det innebär att frontendutvecklaren kan lägga till kundinnehåll före eller efter startpunkten för komponenterna och även göra tredjepartsanrop inuti komponenten. Utvecklaren har dock inte fullständig kontroll över hur komponenterna kapslas, till exempel.

### Dynamisk återgivning {#dynamic-rendering}

SPA bör endast förlita sig på dynamisk återgivning av innehåll. Detta är standardförväntningarna där AEM hämtar och återger alla underordnade element i innehållsstrukturen.

All explicit återgivning som pekar på visst innehåll betraktas som statisk återgivning, även om den stöds, som inte är kompatibel med AEM innehållsredigeringsfunktioner. Detta strider också mot principen för [portability](/help/sites-developing/spa-architecture.md#portability).

### Dynamisk routning {#dynamic-routing}

Precis som vid återgivning ska all routning också vara dynamisk. I AEM ska [SPA alltid äga routningen](/help/sites-developing/spa-routing.md) och AEM lyssnar på den och hämtar innehåll baserat på den.

Statisk routning fungerar mot [principen för bärbarhet](/help/sites-developing/spa-architecture.md#portability) och begränsar författaren genom att inte vara kompatibel med innehållsredigeringsfunktionerna i AEM. Om innehållsförfattaren till exempel vill ändra en väg eller ändra en sida med statisk routning måste han eller hon be frontutvecklaren att göra det.

## AEM Project Archetype {#aem-project-archetype}

Alla AEM-projekt ska använda [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html), som har stöd för SPA-projekt med React eller Angular och som använder SPA SDK.

## SPA-designmodeller {#spa-design-models}

Om du följer [principerna för utveckling av SPA i AEM](/help/sites-developing/spa-architecture.md#spa-development-principles-for-aem) kommer SPA att fungera med alla AEM innehållsredigeringsfunktioner som stöds.

Det kan dock finnas fall där detta inte är helt nödvändigt. Tabellen nedan ger en översikt över de olika designmodellerna, deras fördelar och nackdelar.

<table>
 <tbody>
  <tr>
   <th><strong>Designmodell<br /> </strong></th>
   <th><strong>Fördelar</strong></th>
   <th><strong>Nackdelar</strong></th>
  </tr>
  <tr>
   <td>AEM används som headless CMS utan att <a href="/help/sites-developing/spa-reference-materials.md">SDK-ramverket för SPA-redigeraren används.</a></td>
   <td>Utvecklaren har fullständig kontroll över appen.</td>
   <td><p>Innehållsförfattare kan inte använda AEM redigeringsgränssnitt.</p> <p>Koden är inte portabel eller återanvändbar om den innehåller statiska referenser eller routning.</p> <p>Det går inte att använda mallredigeraren, så frontendutvecklaren måste underhålla redigerbara mallar via JCR.</p> </td>
  </tr>
  <tr>
   <td>Framtidsutvecklaren använder SPA Editor SDK-ramverket, men bara vissa delar öppnas för innehållsförfattaren.</td>
   <td>Utvecklaren behåller kontrollen över appen genom att endast aktivera redigering i begränsade delar av appen.</td>
   <td><p>Innehållsförfattare är begränsade till en begränsad uppsättning av AEM funktioner för innehållsutveckling.</p> <p>Koden kan varken vara flyttbar eller återanvändbar om den innehåller statiska referenser eller routning.</p> <p>Det går inte att använda mallredigeraren, så frontendutvecklaren måste underhålla redigerbara mallar via JCR.</p> </td>
  </tr>
  <tr>
   <td>I projektet används SPA Editor SDK fullt ut och klientkomponenterna utvecklas som ett bibliotek och innehållsstrukturen för programmet delegeras till AEM.</td>
   <td><p>Appen är återanvändbar och portabel.</p> <p>Innehållsförfattaren kan redigera appen med hjälp av AEM innehållsredigeringsgränssnitt.<br /> </p> <p>SPA är kompatibelt med mallredigeraren.</p> </td>
   <td><p>Utvecklaren har inte kontroll över programmets struktur och den del av innehållet som delegerats till AEM.</p> <p>Utvecklaren kan fortfarande reservera delar av programmet för innehåll som inte ska redigeras med AEM.</p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Även om alla modeller stöds i AEM kan innehållsförfattare bara interagera med och redigera innehållet i SPA i AEM när de är vana vid det genom att implementera den tredje (och därför följa de [SPA-utvecklingsprinciper som rekommenderas i AEM](/help/sites-developing/spa-architecture.md#spa-development-principles-for-aem)).

## Migrera befintliga SPA till AEM {#migrating-existing-spas-to-aem}

I allmänhet om SPA följer [SPA-utvecklingsprinciperna för AEM](/help/sites-developing/spa-architecture.md#spa-development-principles-for-aem) fungerar SPA i AEM och kan redigeras med AEM SPA-redigeraren.

Följ de här stegen för att göra din befintliga SPA redo att arbeta med AEM.

1. **Gör dina JS-komponenter modulära.**

   Gör dem kapabla att återges i valfri ordning, position och storlek.
1. **Använd behållarna från Adobe SDK för att placera dina komponenter på skärmen.**

   AEM tillhandahåller en sida- och styckesystemkomponent som du kan använda.
1. **Skapa en AEM-komponent för varje JS-komponent.**

   AEM-komponenterna definierar dialogrutan och JSON-utdata.

## Instruktioner för gränssnittsutvecklare {#instructions-for-front-end-developers}

Den främsta uppgiften att engagera en gränssnittsutvecklare att skapa ett SPA för AEM är att komma överens om komponenterna och deras JSON-modeller.

Här följer en översikt över de steg som en frontendutvecklare måste följa när han eller hon utvecklar ett SPA för AEM.

1. **Godkänn komponenter och deras JSON-modell**

   Framtidsutvecklare och AEM-utvecklare måste komma överens om vilka komponenter som är nödvändiga och en modell så att det går att matcha SPA-komponenterna med bakomliggande komponenter.

   AEM-komponenter behövs fortfarande mest för att kunna tillhandahålla redigeringsdialogrutor och för att exportera komponentmodellen.

1. **I Reagera-komponenter kommer du åt modellen via`this.props.cqModel`**

   När komponenterna är överenskomna och JSON-modellen är på plats kan frontendutvecklaren utveckla SPA och bara få åtkomst till JSON-modellen via `this.props.cqModel`.

1. **Implementera komponentens `render()`-metod**

   Utvecklaren implementerar metoden `render()` så som den passar och kan använda fälten för egenskapen `cqModel`. Detta skickar DOM- och HTML-fragmenten som infogas på sidan. Det här är standardsättet att bygga en app i React.

1. **Mappa komponenten till AEM-resurstypen via`MapTo()`**

   Mappningen lagrar komponentklasser och används internt av den angivna `Container`-komponenten för att hämta och dynamiskt instansiera komponenter baserat på den angivna resurstypen.

   Det här fungerar som en&quot;limma&quot; mellan fram- och bakände så att redigeraren vet vilka komponenter som de olika reaktionskomponenterna motsvarar.

   `Page` och `ResponsiveGrid` är bra exempel på klasser som utökar basen `Container`.

1. **Definiera komponentens `EditConfig` som parameter till`MapTo()`**

   Den här parametern är nödvändig för att tala om för redigeraren hur komponenten ska namnges så länge som den inte har återgetts eller inte har något innehåll att återge.

1. **Utöka den angivna `Container`-klassen för sidor och behållare**

   Sidor och styckesystem bör utöka den här klassen så att delegering till inre komponenter fungerar som förväntat.

1. **Implementera en routningslösning som använder HTML5 `History` API.**

   När `ModelRouter` är aktiverat utlöser ett anrop till funktionerna `pushState` och `replaceState` en begäran till `PageModelManager` om att hämta ett saknat fragment av modellen.

   Den aktuella versionen av `ModelRouter` stöder endast användning av URL:er som pekar mot den faktiska resurssökvägen för Sling Model-startpunkter. Det stöder inte användning av vanity-URL:er eller alias.

   `ModelRouter` kan inaktiveras eller konfigureras för att ignorera en lista med reguljära uttryck.

## AEM-Agnostic {#aem-agnostic}

Dessa kodblock illustrerar hur komponenterna React och Angular inte behöver något som är specifikt för Adobe eller AEM.

* Allt som finns inuti JavaScript-komponenten är AEM-agnostiker.
* Men det som är specifikt för AEM är att JS-komponenten måste mappas till en AEM-komponent med MapTo-hjälpen.

![screen_shot_2018-12-11at144019](assets/screen_shot_2018-12-11at144019.png)

Hjälpprogrammet `MapTo` är den limma som gör att bakände och framände-komponenterna kan matchas tillsammans:

* Den talar om för JS-behållaren (eller JS-styckesystemet) vilken JS-komponent som ansvarar för återgivningen av varje komponent som finns i JSON.
* Det lägger till ett HTML-dataattribut i HTML som JS-komponenten återger, så att SPA-redigeraren vet vilken dialogruta som ska visas för författaren när komponenten redigeras.

Mer information om hur du använder `MapTo` och skapar SPA för AEM i allmänhet finns i guiden Komma igång för det valda ramverket.

* [Getting Started with SPAs in AEM - React](/help/sites-developing/spa-getting-started-react.md)
* [Komma igång med SPA i AEM - Angular](/help/sites-developing/spa-getting-started-angular.md)

## AEM arkitektur och SPA {#aem-architecture-and-spas}

Den allmänna arkitekturen i AEM, inklusive utvecklings-, skribent- och publiceringsmiljöer, förändras inte när SPA används. Det är dock bra att förstå hur SPA-utvecklingen passar in i den här arkitekturen.

![screen_shot_2018-12-11at145348](assets/screen_shot_2018-12-11at145348.png)

* **Byggmiljö**

  Det är här som källan för SPA-programkällan och komponentkällan checkas ut.

   * NPM-generatorn för klientlib skapar ett klientbibliotek från SPA-projektet.
   * Biblioteket tas av Maven och distribueras av Maven Build-pluginen tillsammans med komponenten till AEM Author.

* **AEM Author**

  Innehåll skapas på AEM-författaren, inklusive framtagning av SPA-filer.

  När en SPA redigeras med SPA-redigeraren i redigeringsmiljön:

   1. SPA begär den yttre HTML.
   1. CSS läses in.
   1. SPA-programmets JavaScript läses in.
   1. När SPA-programmet körs begärs JSON, vilket gör att programmet kan skapa sidans DOM inklusive attributen `cq-data`.
   1. Med dessa `cq-data`-attribut kan redigeraren läsa in ytterligare sidinformation så att den vet vilka redigeringskonfigurationer som är tillgängliga för komponenterna.

* **AEM Publish**

  Här publiceras det innehåll och de kompilerade biblioteken, inklusive SPA-programartefakter, klientlibs och komponenter för offentlig konsumtion.

* **Dispatcher/CDN**

  Dispatcher fungerar som cachelagringslager för AEM för besökare på webbplatsen.

   * Förfrågningar behandlas på samma sätt som de behandlas i AEM Author, men det finns ingen begäran om sidinformation eftersom detta bara behövs av redigeraren.
   * JavaScript, CSS, JSON och HTML cachelagras, vilket optimerar sidan för snabb leverans.

>[!NOTE]
>
>I AEM behöver man inte köra JavaScript byggmekanismer eller själva JavaScript. AEM är bara värd för kompilerade artefakter från SPA-programmet.

## Nästa steg {#next-steps}

En översikt över hur en enkel SPA i AEM är strukturerad och hur den fungerar finns i Komma igång-guiden för både [React](/help/sites-developing/spa-getting-started-react.md) och [Angular](/help/sites-developing/spa-getting-started-angular.md).

En stegvis guide till hur du skapar en egen SPA finns i [Komma igång med AEM SPA Editor - WKND Events-självstudiekurs](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html).

Mer information om den dynamiska mappningen av modell till komponent och hur den fungerar i SPA i AEM finns i artikeln [Dynamisk mappning av modell till komponent för SPA](/help/sites-developing/spa-dynamic-model-to-component-mapping.md).

Om du vill implementera SPA i AEM för ett annat ramverk än React eller Angular eller bara vill fördjupa dig i hur SPA SDK för AEM fungerar läser du artikeln [SPA-utkast](/help/sites-developing/spa-blueprint.md).
