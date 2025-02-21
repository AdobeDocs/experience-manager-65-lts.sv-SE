---
title: AEM Tagging Framework
description: Tagga innehåll och använda AEM Tagging-infrastrukturen
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
docset: aem65
feature: Developing,Tagging
solution: Experience Manager, Experience Manager Sites
role: Developer
source-git-commit: bc00baf14235e6b77fe79ea7a8e149c6d69b10b5
workflow-type: tm+mt
source-wordcount: '1627'
ht-degree: 0%

---


# AEM Tagging Framework {#aem-tagging-framework}

Taggning gör att innehållet kan kategoriseras och struktureras. Taggar kan klassificeras med ett namnutrymme och en taxonomi. Mer information om hur du använder taggar:

* Mer information om hur du taggar innehåll som innehållsförfattare finns i dokumentet [Använda taggar](/help/sites-authoring/tags.md).
* I dokumentet [Administrera taggar](/help/sites-administering/tags.md) finns information om en administratörs perspektiv för att skapa och hantera taggar och för vilka innehållstaggar har tillämpats.

Den här artikeln fokuserar på det underliggande ramverket som stöder taggning i AEM och hur du använder det som utvecklare.

## Introduktion {#introduction}

Så här taggar du innehåll och använder AEM Tagging-infrastrukturen:

* Taggen måste finnas som en nod av typen `[cq:Tag](#tags-cq-tag-node-type)` under rotnoden [taxonomy.](#taxonomy-root-node)

* Den taggade innehållsnodens `NodeType` måste innehålla [`cq:Taggable`](#taggable-content-cq-taggable-mixin)-blandningen.
* [`TagID`](#tagid) läggs till i innehållsnodens [`cq:tags`](#tagged-content-cq-tags-property)-egenskap och löses till en nod av typen ` [cq:Tag](#tags-cq-tag-node-type)`.

## Taggar : cq:Tag Node Type  {#tags-cq-tag-node-type}

Deklarationen för en tagg hämtas i databasen i en nod av typen `cq:Tag`.

En tagg kan vara ett enkelt ord (till exempel `sky`) eller representera en hierarkisk taxonomi (till exempel `fruit/apple`, vilket betyder både den generiska `fruit` och den mer specifika `apple`).

Taggar identifieras av ett unikt TagID.

En tagg har valfri metainformation, t.ex. en titel, lokaliserade titlar och en beskrivning. Titeln ska visas i användargränssnitt i stället för i TagID, om det finns.

Med taggningsramverket kan du även begränsa möjligheten för författare och besökare att endast använda specifika, fördefinierade taggar.

### Märkordsegenskaper {#tag-characteristics}

* Nodtypen är `cq:Tag`n
* Nodnamnet är en komponent i [TagID](#tagid).
* [TagID](#tagid) innehåller alltid ett [namnutrymme.](#tag-namespace)
* Egenskapen `jcr:title` (titeln som ska visas i användargränssnittet) är valfri.
* Egenskapen `jcr:description` är valfri.
* När taggen innehåller underordnade noder kallas den för en [behållartagg.](#container-tags)
* Taggen lagras i databasen under en bassökväg som kallas [taxonomirotnod.](#taxonomy-root-node)

Eftersom taggar bara är JCR-noder måste nodnamnen följa namnkonventionen [JCR.](naming-conventions.md)

### TaggID {#tagid}

Ett TagID identifierar en sökväg som löses till en taggnod i databasen.

TagID är vanligtvis ett kort TagID som börjar med namnutrymmet eller så kan det vara ett absolut TagID med början från [taxonomirotnoden.](#taxonomy-root-node)

Om innehållet är taggat och inte finns ännu, läggs egenskapen `[cq:tags](#tagged-content-cq-tags-property)` till i innehållsnoden och TagID läggs till i egenskapens `String`-arrayvärde.

TagID består av ett [namnområde](#tag-namespace) följt av det lokala TagID:t. [Behållartaggar](#container-tags) har undertaggar som representerar en hierarkisk ordning i taxonomin. Undertaggar kan användas för att referera till taggar som är samma som alla lokala TagID. Du kan till exempel tagga innehåll med `fruit`, även om det är en behållartagg med undertaggar, till exempel `fruit/apple` och `fruit/banana`.

### Taxonomirotnod {#taxonomy-root-node}

Taxonomirotnoden är grundsökvägen för alla taggar i databasen. Taxonomirotnoden får inte vara en nod av typen `cq:Tag`.

I AEM är bassökvägen `/content/cq:tags` och rotnoden är av typen `cq:Folder`.

### Namnutrymme för tagg {#tag-namespace}

Med namnutrymmen kan du gruppera saker. Det vanligaste användningsfallet är ett namnutrymme per webbplats (till exempel public, internal och portal) eller per större program (till exempel WCM, Assets, Communities). Men namnutrymmen kan användas för olika behov. Namnutrymmen används i användargränssnittet för att endast visa deluppsättningen med taggar (d.v.s. taggar för ett visst namnutrymme) som kan användas för det aktuella innehållet.

Taggens namnområde är den första nivån i taxonomiunderträdet, som är noden direkt under [taxonomirotnoden](#taxonomy-root-node). Ett namnområde är en nod av typen `cq:Tag` vars överordnade nod inte är en `cq:Tag`-nodtyp.

Alla taggar har ett namnutrymme. Om inget namnutrymme anges tilldelas taggen standardnamnutrymmet, som är TagID `default` med titeln `Standard Tags`, d.v.s. `/content/cq:tags/default`.

### Behållartaggar {#container-tags}

En behållartagg är en nod av typen `cq:Tag` som innehåller valfritt antal och valfri typ av underordnade noder, vilket gör det möjligt att förbättra taggmodellen med anpassade metadata.

Dessutom fungerar behållartaggar (eller supertaggar) i en taxonomi som undertaggar till alla undertaggar. Innehåll som är taggat med `fruit/apple` anses till exempel även vara taggat med `fruit`. Det innebär att om du söker efter innehåll som är taggat med `fruit` så hittas även innehållet som är taggat med `fruit/apple`.

### Lösa tagg-ID:n {#resolving-tagids}

Om TagID innehåller ett kolon (`:`) separerar kolon namnutrymmet från taggen eller subtaxonomin, som avgränsas ytterligare med snedstreck (`/`). Om TagID inte har ett kolon används standardnamnutrymmet.

Standardplatsen och den enda platsen för taggar är under `/content/cq:tags`.

Tagg som refererar till icke-befintliga sökvägar eller sökvägar som inte pekar på en `cq:Tag`-nod betraktas som ogiltiga och ignoreras.

I följande tabell visas några exempel på tagg-ID:n, deras element och hur tagg-ID:t tolkas till en absolut sökväg i databasen:

| TaggID | Namnutrymme | Lokalt ID | Behållartaggar | Lövtagg | Databassökväg för absolut tagg |
|---|---|---|---|---|---|
| `dam:fruit/apple/braeburn` | `dam` | `fruit/apple/braeburn` | `fruit`, `apple` | `braeburn` | `/content/cq:tags/dam/fruit/apple/braeburn` |
| `color/red` | `default` | `color/red` | `color` | `red` | `/content/cq:tags/default/color/red` |
| `sky` | `default` | `sky` | Ingen | `sky` | `/content/cq:tags/default/sky` |
| `dam:` | `dam` | Ingen | Ingen | Inget, namnutrymmet | `/content/cq:tags/dam` |
| `/content/cq:tags/category/car` | `category` | `car` | `car` | `car` | `/content/cq:tags/category/car` |

### Lokalisering av taggtitel {#localization-of-tag-title}

När taggen innehåller den valfria titelsträngen ( `jcr:title`) går det att lokalisera titeln för visning genom att lägga till egenskapen `jcr:title.<locale>`.

Mer information finns i följande dokument:

* [Taggar på olika språk](/help/sites-developing/building.md#tags-in-different-languages) som beskriver användningen av API:erna
* [Hantera taggar på olika språk](/help/sites-administering/tags.md#managing-tags-in-different-languages), som beskriver hur taggningskonsolen används

### Åtkomstkontroll {#access-control}

Taggar finns som noder i databasen under [taxonomirotnoden](#taxonomy-root-node). Du kan skapa taggar i ett givet namnutrymme genom att ange lämpliga åtkomstkontrollistor i databasen, så att författare och besökare kan neka eller neka åtkomst.

Om du dessutom nekar läsbehörighet för vissa taggar eller namnutrymmen kan du använda taggar för visst innehåll.

Ett typiskt exempel är:

* Tillåter skrivåtkomst för gruppen/rollen `tag-administrators` till alla namnutrymmen (lägg till/ändra under `/content/cq:tags`). Den här gruppen levereras med AEM färdiga.
* Ge användare/författare läsåtkomst till alla namnutrymmen som ska vara läsbara för dem (oftast alla).
* Ger användare/författare skrivåtkomst till de namnutrymmen där taggar ska kunna definieras fritt av användare/författare (lägg till en nod under `/content/cq:tags/some_namespace`)

## Taggbart innehåll : cq:Taggable Mixin {#taggable-content-cq-taggable-mixin}

För att programutvecklare ska kunna bifoga taggning till en innehållstyp måste nodens registrering ([CND](https://jackrabbit.apache.org/jcr/node-type-notation.html)) innehålla `cq:Taggable` mixin eller `cq:OwnerTaggable` mixin.

`cq:OwnerTaggable`-blandningen, som ärver från `cq:Taggable`, är avsedd att indikera att innehållet kan klassificeras av ägaren/författaren. I AEM är det bara ett attribut för noden `cq:PageContent`. `cq:OwnerTaggable`-blandningen krävs inte av taggningsramverket.

>[!NOTE]
>
>Du bör bara aktivera taggar på den översta noden i ett aggregerat innehållsobjekt (eller på dess `jcr:content`-nod). Exempel:
>
>* Sidor (`cq:Page`) där `jcr:content`noden är av typen `cq:PageContent` som innehåller `cq:Taggable` mixin
>* Assets ( `cq:Asset`) där `jcr:content/metadata`-noden alltid har `cq:Taggable`-mixin
>

### Nodtypsnotation (CND) {#node-type-notation-cnd}

Det finns nodtypsdefinitioner i databasen som CND-filer. CND-notationen definieras som en del av [Jackrabbit-dokumentationen](https://jackrabbit.apache.org/jcr/node-type-notation.html).

De viktigaste definitionerna för nodtyperna i AEM är följande:

```xml
[cq:Tag] > mix:title, nt:base
    orderable
    - * (undefined) multiple
    - * (undefined)
    + * (nt:base) = cq:Tag version

[cq:Taggable]
    mixin
    - cq:tags (string) multiple

[cq:OwnerTaggable] > cq:Taggable
    mixin
```

## Taggat innehåll: egenskapen cq:tags {#tagged-content-cq-tags-property}

Egenskapen `cq:tags` är en `String`-matris som används för att lagra ett eller flera TagID:n när de tillämpas på innehåll av författare eller webbplatsbesökare. Egenskapen har bara betydelse när den läggs till i en nod som har definierats med `[cq:Taggable](#taggable-content-cq-taggable-mixin)`-mixinen.

>[!NOTE]
>
>Om du vill använda taggningsfunktionen i AEM ska anpassade utvecklade program inte definiera andra taggegenskaper än `cq:tags`.

## Flytta och sammanfoga taggar {#moving-and-merging-tags}

Nedan följer en beskrivning av effekterna i databasen när du flyttar eller sammanfogar taggar med [taggningskonsolen](/help/sites-administering/tags.md):

* När en tagg A flyttas eller sammanfogas till tagg B under `/content/cq:tags`:

   * Tagg A tas inte bort och hämtar en `cq:movedTo`-egenskap.
   * Tagg B skapas (om det finns en flytt) och hämtar en `cq:backlinks`-egenskap.

* `cq:movedTo` pekar på tagg B.

   * Den här egenskapen innebär att tagg A har flyttats eller sammanfogats till tagg B. Om du flyttar tagg B uppdateras den här egenskapen i enlighet med detta. Tagg A är alltså dold och sparas bara i databasen för att matcha tagg-ID:n i innehållsnoder som pekar på tagg A. Taggskräpinsamlaren tar bort taggar som tagg A en gång och inga fler innehållsnoder pekar på dem.

   * Ett specialvärde för egenskapen `cq:movedTo` är `nirvana`. Den används när taggen tas bort men inte kan tas bort från databasen eftersom det finns undertaggar med en `cq:movedTo` som måste behållas.

  >[!NOTE]
  >
  >Egenskapen `cq:movedTo` läggs bara till i den flyttade eller sammanslagna taggen om något av dessa villkor uppfylls:
  >
  >1. Taggen används i innehåll (vilket innebär att den har en referens) eller
  >1. Taggen har underordnade objekt som redan har flyttats.

* `cq:backlinks` behåller referenserna i den andra riktningen. Det innebär att det finns en lista med alla taggar som har flyttats till eller sammanfogats med tagg B. Detta krävs oftast för att hålla `cq:movedTo`-egenskaperna uppdaterade även när tagg B flyttas/sammanfogas/tas bort eller när tagg B aktiveras, och då måste även alla dess bakåttaggar aktiveras.

  >[!NOTE]
  >
  >Egenskapen `cq:backlinks` läggs bara till i den flyttade eller sammanslagna taggen om något av dessa villkor uppfylls:
  >
  >1. Taggen används i innehåll (vilket innebär att den har en referens) ELLER
  >1. Taggen har underordnade objekt som redan har flyttats.

* Följande upplösning krävs när du läser en `cq:tags`-egenskap för en innehållsnod:

   1. Om det inte finns någon matchning under `/content/cq:tags` returneras ingen tagg.

   1. Om taggen har en `cq:movedTo`-egenskap, följs det tagg-ID som refereras.

      * Det här steget upprepas så länge den efterföljande taggen har en `cq:movedTo`-egenskap.

   1. Om den följande taggen inte har någon `cq:movedTo`-egenskap läses taggen.

* Om du vill publicera ändringen när en tagg har flyttats eller sammanfogats måste noden `cq:Tag` och alla dess bakgrunder replikeras. Detta görs automatiskt när taggen aktiveras i tagghanteringskonsolen.

* Senare uppdateringar av sidans `cq:tags`-egenskap rensar automatiskt de gamla referenserna. Detta utlöses eftersom en flyttad tagg som löses via API returnerar måltaggen och därmed anger måltaggens ID.

>[!NOTE]
>
>Förflyttning av taggar skiljer sig från migrering av taggar.

## Taggmigrering {#tags-migration}

Sedan Adobe Experience Manager 6.4 lagras taggar under `/content/cq:tags` medan tidigare versioner lagrar taggar under `/etc/tags`.

När du uppgraderar ett AEM-system från en version som är tidigare än 6.4 måste taggar migreras till `/content/cq:tags`.
