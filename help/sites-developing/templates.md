---
title: Mallar
description: Mallar används när du skapar en sida som används som bas för den nya sidan.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
legacypath: /content/docs/en/aem/6-1/develop/the-basics/templates
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 3b3cff43-4edc-4250-8e6d-08eb5906ffcd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '882'
ht-degree: 0%

---

# Mallar{#templates}

Mallar används vid olika tillfällen i AEM:

* [När du skapar en sida väljer du en mall](#templates-pages). Den här mallen används som bas för den nya sidan. Mallen definierar sidans struktur, allt initialt innehåll och de [komponenter](/help/sites-authoring/default-components.md) som kan användas (designegenskaper).

Följande mallar beskrivs i detalj:

* [Sidmallar - redigerbara](/help/sites-developing/page-templates-editable.md)

## Mallar - Sidor {#templates-pages}

AEM har nu två grundläggande typer av mallar för att skapa sidor:

>[!NOTE]
>
>När du använder en mall för att [skapa en sida](/help/sites-authoring/managing-pages.md#creating-a-new-page) finns det ingen synlig skillnad (för sidförfattaren) och ingen indikation på vilken typ av mall som används.

### Redigerbara mallar {#editable-templates}

Redigerbara mallar är nu de bästa sätten att utveckla med AEM.

Fördelarna med redigerbara mallar:

* Kan [skapas](/help/sites-authoring/templates.md#creating-a-new-template-template-author) och [redigeras](/help/sites-authoring/templates.md#editing-a-template-structure-template-author) av dina författare.

* Har introducerats för att du ska kunna definiera följande för alla sidor som skapas med mallen:

   * strukturen
   * det ursprungliga innehållet
   * innehållsprinciper

* När den nya sidan har skapats upprätthålls en dynamisk anslutning mellan sidan och mallen. Den här anslutningen innebär att ändringar i mallstrukturen återspeglas på alla sidor som skapas med den mallen. Ändringar av det ursprungliga innehållet återspeglas inte.
* Innehållsprinciper (redigerade från mallredigeraren) används för att bevara designegenskaperna (designläget används inte i sidredigeraren).
* lagras under `/conf`
* Mer information finns i [Redigerbara mallar](/help/sites-developing/page-templates-editable.md).

>[!NOTE]
>
>Se [Använda redigerbara sidmallar för att utveckla en Experience Manager-webbplats](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=sv-SE).

### Statiska mallar {#static-templates}

Statiska mallar:

* Måste definieras och konfigureras av utvecklarna.
* Det ursprungliga mallsystemet i AEM som har funnits i många versioner.
* En statisk mall är en hierarki med noder som har samma struktur som den sida som ska skapas, men utan något faktiskt innehåll.
* Kopieras för att skapa sidan och det finns ingen dynamisk anslutning därefter.
* Använder [designläge](/help/sites-authoring/default-components-designmode.md) för att behålla designegenskaper.
* lagras under `/apps`

>[!NOTE]
>
>Från och med AEM 6.5 anses användningen av statiska mallar inte vara en god praxis. Använd redigerbara mallar i stället.
>
>Verktygen för [AEM-modernisering](modernization-tools.md) kan hjälpa dig att migrera från statiska till redigerbara mallar.

### Malltillgänglighet {#template-availability}

>[!CAUTION]
>
>AEM erbjuder flera egenskaper för att styra mallarna som tillåts under **Webbplatser**. Om du kombinerar dem kan det dock leda till komplexa regler som är svåra att spåra och hantera.
>
>Därför rekommenderar Adobe att du börjar enkelt genom att definiera:
>
>* endast egenskapen `cq:allowedTemplates`
>
>* endast i platsroten
>
>Se till exempel We.Retail: `/content/we-retail/jcr:content`
>
>Egenskaperna `allowedPaths`, `allowedParents` och `allowedChildren` kan också placeras i mallarna för att definiera mer avancerade regler. Om det är möjligt är det *mycket* enklare att definiera ytterligare `cq:allowedTemplates` egenskaper för underavsnitt på platsen om det finns behov av att begränsa de tillåtna mallarna ytterligare.
>
>En extra fördel är att `cq:allowedTemplates`-egenskaperna kan uppdateras av en författare på fliken **Avancerat** i **Sidegenskaper**. De andra mallegenskaperna kan inte uppdateras med (standard) användargränssnittet, så behöver en utvecklare för att behålla reglerna och en koddistribution för varje ändring.

När du skapar en sida i webbplatsens administratörsgränssnitt beror listan med tillgängliga mallar på platsen för den nya sidan och de placeringsbegränsningar som anges i varje mall.

Följande egenskaper avgör om mallen `T` används för en ny sida som ska placeras som underordnad till sidan `P`. Var och en av dessa egenskaper är en sträng med flera värden som innehåller noll eller flera reguljära uttryck som används för matchning med sökvägar:

* Egenskapen `cq:allowedTemplates` för `jcr:content`-undernoden till `P` eller en överordnad till `P`.

* Egenskapen `allowedPaths` för `T`.

* Egenskapen `allowedParents` för `T`.

* Egenskapen `allowedChildren` för mallen för `P`.

Utvärderingen fungerar enligt följande:

* Den första icke-tomma `cq:allowedTemplates`-egenskapen som påträffades när sidhierarkin som börjar med `P` överskreds matchas mot sökvägen för `T`. Om inget av värdena matchar avvisas `T`.

* Om `T` har en `allowedPaths`-egenskap som inte är tom, men inget av värdena matchar sökvägen för `P`, avvisas `T`.

* Om båda ovanstående egenskaper är tomma eller inte finns, avvisas `T` om den inte tillhör samma program som `P`. `T` tillhör samma program som `P` om och bara om namnet på den andra nivån i sökvägen `T` är detsamma som namnet på den andra nivån i sökvägen `P`. Mallen `/apps/geometrixx/templates/foo` tillhör till exempel samma program som sidan `/content/geometrixx`.

* Om `T` har en `allowedParents`-egenskap som inte är tom, men inget av värdena matchar sökvägen för `P`, avvisas `T`.

* Om mallen för `P` har en `allowedChildren`-egenskap som inte är tom, men inget av värdena matchar sökvägen för `T`, avvisas `T`.

* I alla andra fall tillåts `T`.

I följande diagram visas mallutvärderingsprocessen:

![chlimage_1-176](assets/chlimage_1-176.png)

#### Begränsa mallar som används på underordnade sidor {#limiting-templates-used-in-child-pages}

Om du vill begränsa vilka mallar som kan användas för att skapa underordnade sidor under en viss sida använder du egenskapen `cq:allowedTemplates` för noden `jcr:content` för att ange listan med mallar som ska tillåtas som underordnade sidor. Varje värde i listan måste vara en absolut sökväg till en mall för en tillåten underordnad sida, till exempel `/apps/geometrixx/templates/contentpage`.

Du kan använda egenskapen `cq:allowedTemplates` på mallens `jcr:content`-nod om du vill att den här konfigurationen ska tillämpas på alla nyskapade sidor som använder den här mallen.

Om du vill lägga till fler begränsningar, till exempel för mallhierarkin, kan du använda egenskaperna `allowedParents/allowedChildren` i mallen. Du kan sedan uttryckligen ange att sidor som skapats från en mall T måste vara överordnade/underordnade sidor till sidor som skapats från en mall T.
