---
title: Sidmallar - redigerbara
description: Redigerbara mallar har lagts till som gör att icke-utvecklare kan skapa och redigera mallar, mallar som behåller en dynamisk anslutning till sidor som skapats av dem och som gör sidkomponenten mer generisk
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 7831c056-86f8-41c1-bc45-5e9829bc54bc
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2981'
ht-degree: 0%

---

# Sidmallar - redigerbara {#page-templates-editable}

Redigerbara mallar har lagts till:

* Tillåt specialiserade författare att [skapa och redigera mallar](/help/sites-authoring/templates.md).

   * Sådana specialiserade författare kallas **mallskapare**
   * Mallförfattare måste vara medlemmar i gruppen `template-authors`.

* Tillhandahåll mallar som behåller en dynamisk anslutning till alla sidor som skapas från dem. Om du gör det ser du till att alla ändringar i mallen återspeglas på själva sidorna.
* Gör sidkomponenten mer generisk så att kärnsideskomponenten kan användas utan anpassning.

Med redigerbara mallar isoleras de delar som utgör en sida inuti komponenterna. Du kan konfigurera nödvändiga kombinationer av komponenter i ett användargränssnitt så att du slipper skapa en ny sidkomponent för varje sidvariant.

Det här dokumentet:

* Ger en översikt över hur du skapar redigerbara mallar

   * Mer information finns i [Skapa sidmallar](/help/sites-authoring/templates.md)

* Beskriver de admin-/utvecklaråtgärder som krävs för att skapa redigerbara mallar
* Beskriver de tekniska grunderna för redigerbara mallar

Det här dokumentet förutsätter att du redan är bekant med att skapa och redigera mallar. Se redigeringsdokumentet [Skapa sidmallar](/help/sites-authoring/templates.md), som beskriver funktionerna i redigerbara mallar så som de visas för mallskaparen.

>[!NOTE]
>
>Följande självstudiekurs kan också vara intressant för att konfigurera en redigerbar sidmall i ett nytt projekt:
>[Komma igång med AEM Sites del 2 - Skapa en bassida och mall ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/pages-templates.html?lang=sv-SE)

## Skapa en ny mall {#creating-a-new-template}

Skapandet av redigerbara mallar görs huvudsakligen med [mallkonsolen och mallredigeraren](/help/sites-authoring/templates.md) av en mallskapare. I det här avsnittet ges en översikt över processen och en beskrivning av vad som händer på teknisk nivå.

Mer information om hur du använder redigerbara mallar i ett AEM-projekt finns i [Skapa ett AEM-projekt med Lazybone](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/create-aem-project-structure-using-lazybones/m-p/186478).

När du skapar en redigerbar mall:

1. Skapa en [mapp för mallarna](#template-folders). Den här mappen är inte obligatorisk, men rekommenderas.
1. Välj en [malltyp](#template-type). Den här typen kopieras för att skapa [malldefinitionen](#template-definitions).

   >[!NOTE]
   >
   >Du kan välja mellan olika malltyper direkt. Du kan även [skapa egna webbplatsspecifika malltyper](/help/sites-developing/page-templates-editable.md#creating-template-types), om det behövs.

1. Konfigurera den nya mallens struktur, innehållsprinciper, ursprungliga innehåll och layout.

   **Struktur**

   * Strukturen gör att du kan definiera komponenter och innehåll för mallen.
   * Komponenter som definieras i mallstrukturen kan inte flyttas till en resultatsida eller tas bort från eventuella resultatsidor.

      * Om du skapar en mall i en anpassad mapp utanför exempelinnehållet `We.Retail` kan du välja Foundation Components (Foundation-komponenter) eller använda [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/overview.html?lang=sv-SE).

   * Om du vill att sidförfattare ska kunna lägga till och ta bort komponenter lägger du till ett styckesystem i mallen.
   * Komponenter kan låsas upp och låsas igen så att du kan definiera ursprungligt innehåll.

   Mer information om hur en mallskapare definierar strukturen finns i [Skapa sidmallar](/help/sites-authoring/templates.md#editing-a-template-structure-template-author).

   Mer teknisk information om strukturen finns i [Struktur](/help/sites-developing/page-templates-editable.md#structure) i det här dokumentet.

   **Profiler**

   * Innehållsprinciperna definierar designegenskaperna för en komponent.

      * Till exempel de tillgängliga komponenterna eller minimi-/maximidimensionerna.

   * Dessa profiler gäller för mallen (och sidor som skapas med mallen).

   Mer information om hur en mallskapare definierar principer finns i [Skapa sidmallar](/help/sites-authoring/templates.md#editing-a-template-structure-template-author).

   Mer teknisk information om profiler finns i [Innehållsprinciper](/help/sites-developing/page-templates-editable.md#content-policies) i det här dokumentet.

   **Inledande innehåll**

   * Ursprungligt innehåll definierar innehåll som visas när en sida skapas baserat på mallen.
   * Det initiala innehållet kan sedan redigeras av sidförfattare.

   Mer information om hur en mallskapare definierar strukturen finns i [Skapa sidmallar](/help/sites-authoring/templates.md#editing-a-template-initial-content-author).

   Teknisk information om ursprungligt innehåll finns i [Inledande innehåll](/help/sites-developing/page-templates-editable.md#initial-content) i det här dokumentet.

   **Layout**

   * Du kan definiera mallayouten för ett antal olika enheter.
   * Responsiv layout för mallar fungerar på samma sätt som för sidredigering.

   Mer information om hur en mallskapare definierar mallayouten finns i [Skapa sidmallar](/help/sites-authoring/templates.md#editing-a-template-layout-template-author).

   Mer teknisk information om mallayout finns i [Layout](/help/sites-developing/page-templates-editable.md#layout) i det här dokumentet.

1. Aktivera mallen och tillåt den sedan för specifika innehållsträd.

   * En mall kan aktiveras eller inaktiveras för att göra den tillgänglig eller inte tillgänglig för sidförfattare.
   * En mall kan göras tillgänglig eller otillgänglig för vissa sidgrenar.

   Mer information om hur mallskaparen aktiverar en mall finns i [Skapa sidmallar](/help/sites-authoring/templates.md#enabling-and-allowing-a-template-template-author).

   Mer teknisk information om hur du aktiverar en mall finns i [Aktivera och tillåta en mall för användare](/help/sites-developing/page-templates-editable.md#enabling-and-allowing-a-template-for-use)e i det här dokumentet

1. Använd det för att skapa innehållssidor.

   * När du använder en mall för att skapa en sida finns det ingen synlig skillnad och ingen indikation mellan statiska och redigerbara mallar.
   * För sidförfattaren är processen genomskinlig.

   Mer information om hur en sidförfattare använder mallar för att skapa en sida finns i [Skapa och ordna sidor](/help/sites-authoring/managing-pages.md#templates).

   Mer teknisk information om hur du skapar sidor med redigerbara mallar finns i [Gällande innehållssidor](/help/sites-developing/page-templates-editable.md#resultant-content-pages) i det här dokumentet.

>[!TIP]
>
>Ange aldrig någon information som måste internationaliseras i en mall. För internalisering rekommenderas [lokaliseringsfunktionen för kärnkomponenterna](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/get-started/localization.html?lang=sv-SE).

>[!NOTE]
>
>Mallar är kraftfulla verktyg som effektiviserar arbetsflödet för att skapa sidor. Alltför många mallar kan överbelasta författarna och göra det förvirrande att skapa sidor. En bra tumregel är att hålla antalet mallar under 100.
>
>Adobe rekommenderar inte att man har fler än 1 000 mallar på grund av potentiella prestandaeffekter.

>[!NOTE]
>
>Redigerarens klientbibliotek förutsätter att namnområdet `cq.shared` finns på innehållssidorna. Om den inte finns resulterar det i JavaScript-felet `Uncaught TypeError: Cannot read property 'shared' of undefined`.
>
>Alla exempelinnehållssidor innehåller `cq.shared`, så allt innehåll som baseras på dem inkluderar automatiskt `cq.shared`. Om du vill skapa egna innehållssidor från grunden utan att basera dem på exempelinnehåll måste du se till att inkludera namnutrymmet `cq.shared`.
>
>Mer information finns i [Använda bibliotek på klientsidan](/help/sites-developing/clientlibs.md).

## Mallmappar {#template-folders}

Du kan använda följande mappar för att ordna dina mallar:

* **global**
* Webbplatsspecifik
De platsspecifika mappar som du skapar för att ordna dina mallar skapas med kontoadministratörsbehörighet.

>[!NOTE]
>
>Även om du kan kapsla dina mappar visas de som en platt struktur när användaren visar dem i konsolen **Mallar**.

I en standardinstans av AEM finns mappen **global** i mallkonsolen. Den här mappen innehåller standardmallar och fungerar som reserv om inga principer och/eller malltyper hittas i den aktuella mappen. Du kan lägga till dina standardmallar i den här mappen eller skapa en mapp (rekommenderas).

>[!NOTE]
>
>Det är bäst att skapa en mapp som innehåller dina anpassade mallar och inte använda den globala mappen.

>[!CAUTION]
>
>Mappar måste skapas av en användare med `admin` rättigheter.

Malltyper och profiler ärvs i alla mappar enligt följande prioritetsordning:

1. Den aktuella mappen.
1. Överordnad eller överordnad till den aktuella mappen.
1. `/conf/global`
1. `/apps`
1. `/libs`

En lista över alla tillåtna poster skapas. Om några konfigurationer överlappar ( `path`/ `label`) visas endast den instans som ligger närmast den aktuella mappen för användaren.

Så här skapar du en mapp:

* Programmerat eller med CRXDE Lite
* Använda Konfigurationsläsaren

## Använda CRXDE Lite {#using-crxde-lite}

1. En ny mapp (under /conf) kan skapas för din instans antingen programmatiskt eller med CRXDE Lite.

   Följande struktur måste användas:

   ```xml
   /conf
       <your-folder-name> [sling:Folder]
           settings [sling:Folder]
               wcm [cq:Page]
                   templates [cq:Page]
                   policies [cq:Page]
   ```

1. Du kan sedan definiera följande egenskaper på mappens rotnod:

   `<your-folder-name> [sling:Folder]`

   Namn: `jcr:title`

   * Typ: `String`

   * Värde: Den rubrik (för mappen) som du vill ska visas i konsolen **Mallar**.

1. I *tillägg* till de vanliga redigeringsbehörigheterna och -behörigheterna (till exempel `content-authors`) tilldelar du grupper och definierar de åtkomstbehörigheter (ACL) som krävs för att författarna ska kunna skapa mallar i den nya mappen.

   Gruppen `template-authors` är standardgruppen som måste tilldelas. Mer information finns i följande avsnitt, [ACL:er och grupper](/help/sites-developing/page-templates-editable.md#acls-and-groups).

   Mer information om hur du hanterar och tilldelar åtkomsträttigheter finns i [Hantering av åtkomsträttigheter](/help/sites-administering/user-group-ac-admin.md#access-right-management).

### Använda Konfigurationsläsaren {#using-the-configuration-browser}

1. Gå till **Global navigering** > **Verktyg** > **Konfigurationsläsaren**.

   De befintliga mapparna visas till vänster inklusive mappen **global**.

1. Klicka på **Skapa**.
1. I dialogrutan **Skapa konfiguration** måste följande fält konfigureras:

   * **Titel**: Ange en rubrik för konfigurationsmappen
   * **Redigerbara mallar**: Markera för att tillåta redigerbara mallar i den här mappen

1. Klicka på **Skapa**

>[!NOTE]
>
>I konfigurationsläsaren kan du redigera den globala mappen och aktivera alternativet **Redigerbara mallar** om du vill skapa mallar i den här mappen. Denna metod rekommenderas dock inte.
>
>Mer information finns i dokumentationen för [Configuration Browser](/help/sites-administering/configurations.md).

### Behörighetslistor och grupper {#acls-and-groups}

När mallmapparna har skapats (antingen med CRXDE eller med Configuration Browser) måste åtkomstkontrollistor definieras för rätt grupper för mallmapparna för att säkerställa att de är skyddade.

Mallmapparna för referensimplementeringen [`We.Retail` ](/help/sites-developing/we-retail.md) kan användas som exempel.

#### Mallförfattargruppen {#the-template-authors-group}

Gruppen `template-authors` är den grupp som används för att hantera åtkomst till mallar och levereras som standard med AEM, men är tom. Användare måste läggas till i gruppen för projektet/webbplatsen.

>[!CAUTION]
>
>Gruppen `template-authors` är *endast* för användare som måste kunna skapa mallar.
>
>Att redigera mallar är kraftfullt och om det inte görs på rätt sätt kan befintliga mallar brytas. Därför bör denna roll vara inriktad och endast omfatta kvalificerade användare.

Följande tabell visar vilka behörigheter som krävs för mallredigering.

<table>
 <tbody>
  <tr>
   <th>Bana</th>
   <th>Roll/grupp</th>
   <th>Behörigheter<br /> </th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td rowspan="3"><code>/conf/&lt;<i>your-folder</i>&gt;/settings/wcm/templates</code></td>
   <td>Mallförfattare<br /> </td>
   <td>läsa, skriva, replikera</td>
   <td>Mallförfattare som skapar, läser, uppdaterar, tar bort och replikerar mallar i det platsspecifika <code>/conf</code>-området</td>
  </tr>
  <tr>
   <td>Anonym webbanvändare</td>
   <td>read</td>
   <td>En anonym webbanvändare måste läsa mallar när en sida återges</td>
  </tr>
  <tr>
   <td>Innehållsförfattare</td>
   <td>replikera</td>
   <td>replicateInnehållsförfattare måste aktivera sidans mallar när en sida aktiveras</td>
  </tr>
  <tr>
   <td rowspan="3"><code>/conf/&lt;<i>your-folder</i>&gt;/settings/wcm/policies</code></td>
   <td><code>Template Author</code></td>
   <td>läsa, skriva, replikera</td>
   <td>Mallförfattare som skapar, läser, uppdaterar, tar bort och replikerar mallar i det platsspecifika <code>/conf</code>-området</td>
  </tr>
  <tr>
   <td>Anonym webbanvändare</td>
   <td>read</td>
   <td>En anonym webbanvändare måste läsa principer när en sida återges</td>
  </tr>
  <tr>
   <td>Innehållsförfattare</td>
   <td>replikera</td>
   <td>Innehållsförfattare måste aktivera profilerna för en sidmall när en sida aktiveras</td>
  </tr>
  <tr>
   <td rowspan="2"><code>/conf/&lt;site&gt;/settings/template-types</code></td>
   <td>Mallförfattare</td>
   <td>read</td>
   <td>Mallförfattare skapar en mall baserad på en av de fördefinierade malltyperna.</td>
  </tr>
  <tr>
   <td>Anonym webbanvändare</td>
   <td>ingen</td>
   <td>En anonym webbanvändare får inte komma åt malltyperna</td>
  </tr>
 </tbody>
</table>

Den här standardgruppen `template-authors` täcker bara projektinställningarna, där alla `template-authors`-medlemmar har åtkomst till och kan redigera alla mallar. För mer komplexa konfigurationer, där det finns ett behov av flera mallskapargrupper för att åtskilja mallarna, måste fler anpassade mallskapargrupper skapas. Behörigheterna för mallförfattargrupperna är dock fortfarande desamma.

#### Äldre mallar under /conf/global {#legacy-templates-under-conf-global}

Lagra inte mallar i `/conf/global`. För vissa äldre installationer kan det dock fortfarande finnas mallar på den här platsen. *Endast* i sådana äldre situationer bör följande `/conf/global` sökvägar konfigureras explicit.

<table>
 <tbody>
  <tr>
   <th>Bana</th>
   <th>Roll/grupp</th>
   <th>Behörigheter<br /> </th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td rowspan="3"><code>/conf/global/settings/wcm/templates</code></td>
   <td>Mallförfattare</td>
   <td>läsa, skriva, replikera</td>
   <td>Mallförfattare som skapar, läser, uppdaterar, tar bort och replikerar mallar i <code>/conf/global</code></td>
  </tr>
  <tr>
   <td>Anonym webbanvändare</td>
   <td>read</td>
   <td>En anonym webbanvändare måste läsa mallar när en sida återges</td>
  </tr>
  <tr>
   <td>Innehållsförfattare</td>
   <td>replikera</td>
   <td>Innehållsförfattare måste aktivera sidmallarna när en sida aktiveras</td>
  </tr>
  <tr>
   <td rowspan="3"><code>/conf/global/settings/wcm/policies</code></td>
   <td><code>Template Author</code></td>
   <td>läsa, skriva, replikera</td>
   <td>Mallförfattare som skapar, läser, uppdaterar, tar bort och replikerar mallar i <code>/conf/global</code></td>
  </tr>
  <tr>
   <td>Anonym webbanvändare</td>
   <td>read</td>
   <td>En anonym webbanvändare måste läsa principer när en sida återges</td>
  </tr>
  <tr>
   <td>Innehållsförfattare</td>
   <td>replikera</td>
   <td>Innehållsförfattare måste aktivera profilerna för en sidmall när en sida aktiveras</td>
  </tr>
  <tr>
   <td rowspan="2"><code>/conf/global/settings/wcm/template-types</code></td>
   <td>Mallförfattare</td>
   <td>read</td>
   <td>Mallförfattare skapar en mall baserad på en av de fördefinierade malltyperna</td>
  </tr>
  <tr>
   <td>Anonym webbanvändare</td>
   <td>ingen</td>
   <td>En anonym webbanvändare får inte komma åt malltyperna</td>
  </tr>
 </tbody>
</table>

## Malltyp {#template-type}

När du skapar en mall anger du en malltyp:

* Malltyper tillhandahåller effektivt mallar för en mall. När du skapar en mall används strukturen och det ursprungliga innehållet för den valda malltypen för att skapa mallen.

   * Malltypen kopieras för att skapa mallen.
   * När kopian är klar är den enda kopplingen mellan mallen och malltypen en statisk referens i informationssyfte.

* Med malltyper kan du definiera:

   * Sidkomponentens resurstyp.
   * Rotnodens princip, som definierar vilka komponenter som tillåts i mallredigeraren.

* AEM har ett litet urval färdiga malltyper som HTML5 Page och Adaptive Form Page.

   * Ytterligare exempel finns som en del av exempelinnehållet [`We.Retail`](/help/sites-developing/we-retail.md).

* Malltyper definieras vanligtvis av utvecklare.

Malltyperna som inte finns lagrade under:

* `/libs/settings/wcm/template-types`

>[!CAUTION]
>
>Ändra ingenting i sökvägen `/libs`. Orsaken är att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan skrivas över när du använder en snabbkorrigering eller ett funktionspaket).

Platsspecifika malltyper bör lagras på samma plats som:

* `/apps/settings/wcm/template-types`

Definitioner för dina anpassade malltyper ska lagras i användardefinierade mappar (rekommenderas) eller i `global`. Till exempel:

* `/conf/<my-folder-01>/<my-folder-02>/settings/wcm/template-types`
* `/conf/<my-folder>/settings/wcm/template-types`
* `/conf/global/settings/wcm/template-types`

>[!CAUTION]
>
>Malltyperna måste respektera rätt mappstruktur (d.v.s. `/settings/wcm/...`), annars går det inte att hitta malltyperna.

### Skapa malltyper {#creating-template-types}

Om du har skapat en mall som kan användas som bas för andra mallar kan du kopiera den här mallen som en malltyp.

1. Skapa en mall på samma sätt som du skapar redigerbara mallar. Se [Skapa sidmallar](/help/sites-authoring/templates.md#creating-a-new-template-template-author). Detta kan fungera som grund för malltypen.
1. Med CRXDE Lite kopierar du den nya mallen från noden `templates` till noden `template-types` under [mallmappen](/help/sites-developing/page-templates-editable.md#template-folders).
1. Ta bort mallen från noden `templates` under [mallmappen](/help/sites-developing/page-templates-editable.md#template-folders).
1. I kopian av mallen som finns under noden `template-types` tar du bort alla `cq:template` - och `cq:templateType` -egenskaper från alla `jcr:content` -noder.

Du kan också utveckla en egen malltyp med en exempelredigerbar mall som bas, som finns på GitHub.

KOD PÅ GITHUB

Koden för den här sidan finns på GitHub

* [Öppna aem-sites-example-custom-template-type-projekt på GitHub](https://github.com/Adobe-Marketing-Cloud/aem-sites-example-custom-template-type)
* Hämta projektet som [en ZIP-fil](https://codeload.github.com/Adobe-Marketing-Cloud/aem-sites-example-custom-template-type/zip/refs/heads/master)

## Malldefinitioner {#template-definitions}

Definitioner för redigerbara mallar lagras i [användardefinierade mappar](/help/sites-developing/page-templates-editable.md#template-folders) (rekommenderas) eller i `global`. Till exempel:

* `/conf/<my-folder>/settings/wcm/templates`
* `/conf/<my-folder-01>/<my-folder-02>/settings/wcm/templates`
* `/conf/global/settings/wcm/templates`

Rotnoden för mallen är av typen `cq:Template` med en skelettstruktur på:

```xml
<template-name>
  initial
    jcr:content
      root
        <component>
        ...
        <component>
  jcr:content
    @property status
  policies
    jcr:content
      root
        @property cq:policy
        <component>
          @property cq:policy
        ...
        <component>
          @property cq:policy
  structure
    jcr:content
      root
        <component>
        ...
        <component>
      cq:responsive
        breakpoints
  thumbnail.png
```

Huvudelementen är:

* `<template-name>`

   * ` [initial](#initial-content)`
   * `jcr:content`
   * ` [structure](#structure)`
   * ` [policies](#policies)`
   * `thumbnail.png`

### jcr:innehåll {#jcr-content}

Den här noden innehåller egenskaper för mallen:

* **Namn**: `jcr:title`

* **Namn**: `status`

   * **Typ**: `String`

   * **Värde**: `draft`, `enabled` eller `disabled`

### Struktur {#structure}

Definierar strukturen för den resulterande sidan:

* Sammanfogas med det ursprungliga innehållet ( `/initial`) när en sida skapas.
* Ändringar som görs i strukturen återspeglas i alla sidor som skapas med mallen.
* Noden `root` ( `structure/jcr:content/root`) definierar listan över komponenter som är tillgängliga på den resulterande sidan.

   * Komponenter som definieras i mallstrukturen kan inte flyttas eller tas bort från resultatsidor.
   * När en komponent är olåst ställs egenskapen `editable` in på `true`.

   * När en komponent som redan innehåller innehåll har låsts upp, flyttas det här innehållet till grenen `initial`.

* Noden `cq:responsive` innehåller definitioner för den responsiva layouten.

### Ursprungligt innehåll {#initial-content}

Definierar det ursprungliga innehåll som en ny sida har när den skapas:

* Innehåller en `jcr:content`-nod som kopieras till nya sidor.
* Sammanfogas med strukturen ( `/structure`) när du skapar en sida.
* Befintliga sidor uppdateras om det ursprungliga innehållet ändras efter att de har skapats.
* Noden `root` innehåller en lista med komponenter för att definiera vad som är tillgängligt på den resulterande sidan.
* Om innehåll läggs till i en komponent i strukturläge och komponenten senare är olåst (eller omvänt), används det här innehållet som ursprungligt innehåll.

### Layout {#layout}

När du [redigerar en mall kan du definiera layouten](/help/sites-authoring/templates.md), använder den här metoden [responsiv standardlayout](/help/sites-authoring/responsive-layout.md) som också kan [konfigureras](/help/sites-administering/configuring-responsive-layout.md).

### Innehållsprinciper {#content-policies}

Innehållets (eller designens) profiler definierar designegenskaperna för en komponent, till exempel komponentens tillgänglighet eller min-/maxmått. Dessa profiler gäller för mallen (och sidor som skapas med mallen). Du kan skapa och välja innehållsprinciper i mallredigeraren.

* Egenskapen `cq:policy` på noden `root`
  `/conf/<your-folder>/settings/wcm/templates/<your-template>/policies/jcr:content/root`
Ger en relativ referens till innehållsprincipen för sidans styckesystem.

* Egenskapen `cq:policy`, på de komponentspecifika noderna under `root`, innehåller länkar till principerna för de enskilda komponenterna.

* De faktiska principdefinitionerna lagras under:
  `/conf/<your-folder>/settings/wcm/policies/wcm/foundation/components`

>[!NOTE]
>
>Sökvägarna för principdefinitioner beror på komponentens sökväg. `cq:policy` innehåller en relativ referens till själva konfigurationen.

>[!NOTE]
>
>Sidor som skapas från redigerbara mallar har inte något designläge i sidredigeraren.
>
>Trädet `policies` i en redigerbar mall har samma hierarki som designlägeskonfigurationen för en statisk mall under:
>
>`/etc/designs/<my-site>/jcr:content/<component-name>`
>
>Konfiguration av designläge för en statisk mall har definierats per sidkomponent.

### Sidprofiler {#page-policies}

Med sidprofiler kan du definiera [innehållsprincipen](#content-policies) för sidan (huvudparsys), antingen i mallen eller på de resulterande sidorna.

### Aktivera och tillåta en mall för användning {#enabling-and-allowing-a-template-for-use}

1. **Aktivera mallen**

   Innan en mall kan användas måste den aktiveras av något av följande:

   * [Aktivera mallen](/help/sites-authoring/templates.md#enablingatemplateauthor) från konsolen **Mallar**.

   * Anger egenskapen status för noden `jcr:content`.

      * På:

        `/conf/<your-folder>/settings/wcm/templates/<your-template>/jcr:content`

      * Definiera egenskapen:

         * Namn: status
         * Typ: String
         * Värde: `enabled`

1. **Tillåtna mallar**

   * [Definiera sökvägarna för tillåtna mallar på **Sidegenskaper**](/help/sites-authoring/templates.md#allowing-a-template-author) för rätt sida eller rotsida i en undergren.
   * Ange egenskapen:

     `cq:allowedTemplates`
På noden `jcr:content` för den begärda grenen.

   Med värdet:

   `/conf/<your-folder>/settings/wcm/templates/.*`

## Gällande innehållssidor {#resultant-content-pages}

Sidor skapade från redigerbara mallar:

* Skapas med ett underträd som sammanfogas från `structure` och `initial` i mallen

* Har referenser till information som finns i mallen och malltypen. Du kan uppnå den här funktionen med en `jcr:content`-nod med egenskaperna:

   * `cq:template`
Innehåller den dynamiska referensen till den faktiska mallen; gör att malländringarna kan återspeglas på de faktiska sidorna.

   * `cq:templateType`
Anger en referens till malltypen.

![chlimage_1-71](assets/chlimage_1-71.png)

Diagrammet ovan visar hur mallar, innehåll och komponenter samverkar:

* Styrenhet - `/content/<my-site>/<my-page>`
Den resulterande sidan som refererar till mallen. Innehållet styr hela processen. Enligt definitionerna får den åtkomst till rätt mall och komponenter.

* Konfiguration - `/conf/<my-folder>/settings/wcm/templates/<my-template>`
[ Mallen och relaterade innehållsprinciper ](#template-definitions) definierar sidkonfigurationen.

* Modell - OSGi-paket
[OSGI-paketen](/help/sites-deploying/osgi-configuration-settings.md) implementerar funktionen.

* Visa - `/apps/<my-site>/components`
I både författar- och publiceringsmiljöer återges innehållet av [ components ](/help/sites-developing/components.md) .

När en sida återges:

* **Mallar**:

   * Egenskapen `cq:template` för noden `jcr:content` refereras till för att komma åt mallen som motsvarar den sidan.

* **Komponenter**:

   * Sidkomponenten sammanfogar mallens `structure/jcr:content`-träd med sidans `jcr:content`-träd.

   * Sidkomponenten tillåter bara författaren att redigera noderna i mallstrukturen som har flaggats som redigerbara (och eventuella underordnade noder).
   * När du återger en komponent på en sida hämtas komponentens relativa sökväg från noden `jcr:content`. Samma sökväg under noden `policies/jcr:content` i mallen söks sedan igenom.

      * Egenskapen `cq:policy` för den här noden pekar på den faktiska innehållsprincipen (d.v.s. den innehåller designkonfigurationen för den komponenten).

      * Med den här funktionen kan du ha flera mallar som återanvänder samma innehållsprincipkonfigurationer.
