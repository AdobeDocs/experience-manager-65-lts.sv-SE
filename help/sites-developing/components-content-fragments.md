---
title: Komponenter för innehållsfragment
description: Adobe Experience Manager (AEM) innehållsfragment skapas och hanteras som sidoberoende resurser
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: components
content-type: reference
docset: aem65
pagetitle: Components for Content Fragments
solution: Experience Manager, Experience Manager Sites
feature: Developing,Content Fragments
role: Developer
exl-id: 2196af09-8053-49c3-8a23-caf03bb9a39d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '932'
ht-degree: 0%

---

# Komponenter för innehållsfragment{#components-for-content-fragments}

## Komponenter för fragmentredigering {#components-for-fragment-authoring}

>[!CAUTION]
>
>Vi rekommenderar inte att du utökar eller ändrar de faktiska komponenterna som används i Fragment Editor eftersom de fortfarande kan ändras.

Se [API för hantering av innehållsfragment - klientsidan](/help/sites-developing/customizing-content-fragments.md#the-content-fragment-management-api-client-side).

## Komponenter för sidredigering {#components-for-page-authoring}

>[!CAUTION]
>
>[Kärnkomponenten för innehållsfragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/content-fragment-component.html) rekommenderas nu. Mer information finns i [Utveckla kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/overview.html).
>
>I det här avsnittet beskrivs den ursprungliga komponenten som levererats för användning med innehållsfragment (**Innehållsfragment** i gruppen **Allmänt**).

>[!NOTE]
>
>Mer information finns även i [Innehållsfragment som konfigurerar komponenter för återgivning](/help/sites-developing/content-fragments-config-components-rendering.md).

Adobe Experience Manager (AEM) innehållsfragment [skapas och hanteras som sidoberoende resurser](/help/assets/content-fragments/content-fragments.md). Med dem kan du skapa kanalneutralt innehåll tillsammans med (eventuellt kanalspecifika) variationer. [Du kan sedan använda dessa fragment och deras varianter när du redigerar innehållssidorna](/help/sites-authoring/content-fragments.md). Du kan också använda en befintlig resurs för innehållsfragment genom att [dra den från resursläsaren till sidan](/help/sites-authoring/content-fragments.md#adding-a-content-fragment-to-your-page) (som för andra resursbaserade komponenter, till exempel image-konfigurationen för grundkomponenten). Komponenten för innehållsfragment som inte finns i rutan visar bara ett [element](/help/assets/content-fragments/content-fragments.md#constituent-parts-of-a-content-fragment) av det refererade innehållsfragmentet. Med hjälp av komponentdialogrutan kan du definiera elementet [och variationen och intervallet för fragmentstycken](/help/assets/content-fragments/content-fragments.md#constituent-parts-of-a-content-fragment) som du vill visa på sidan.

>[!NOTE]
>
>Denna Content Fragment-komponent infördes i AEM 6.2 som en förbättrad version av artikelkomponenten, som har tagits bort.

>[!NOTE]
>
>Innehållsfragment stöds inte i det klassiska användargränssnittet.

### Definition {#definition}

Komponenten **Innehållsfragment** används för att hålla en referens till en resurs för innehållsfragment (förbättrade textresurser). Resurstypen för innehållsfragmentet är:

`dam/cfm/components/contentfragment/contentfragment`

Referensen definieras i egenskapen:

`fileReference`

Det är bara redigeraren av det beröringskänsliga användargränssnittet som helt stöder komponenter för innehållsfragment, som inkluderar klientbiblioteket:

`cq.authoring.editor.plugin.cfm`

Det här biblioteket lägger till funktioner som är specifika för innehållsfragment i redigeraren. Det finns till exempel stöd för att lägga till och konfigurera innehållsfragment på sidan, möjlighet att söka efter innehållsfragmentresurser i resursläsaren och efter associerat innehåll på sidopanelen.

### Mellan innehåll {#in-between-content}

Med komponenten **Innehållsram** t kan du släppa ytterligare komponenter mellan de olika styckena i det visade [elementet](/help/assets/content-fragments/content-fragments.md#constituent-parts-of-a-content-fragment). Elementet som visas består i själva verket av olika stycken (varje stycke markeras med en radmatning). Du kan infoga innehåll med andra komponenter mellan styckena.

Ur teknisk synvinkel finns varje stycke i det visade elementet i en egen parsys, och varje komponent som du lägger till mellan styckena infogas (under huven) i parsytan.

Om instansen av innehållsavsnittskomponenten består av tre stycken, har komponenten alltså tre olika parsyser i databasen. Allt mellanliggande innehåll som läggs till i innehållsfragmentet finns i dessa parsys.

I databasen lagras det mellanliggande innehållet i förhållande till dess position inuti den övergripande styckestrukturen, d.v.s. det är inte kopplat till det faktiska styckeinnehållet.

Tänk på följande för att illustrera detta:

* En instans av ett innehållsfragment bestående av tre stycken
* Och att en del innehåll redan har infogats efter det andra stycket

   * Det innebär att innehållet lagras i den andra parsysen.

Om styckestrukturen i den här instansen ändras (genom att ändra variationen, elementet eller styckeintervallet som visas) kan det påverka det mellanliggande innehållet som visas när innehållet i innehållsfragmentet:

* Redigeras och ett annat stycke läggs till före det andra stycket:

   * Det mellanliggande innehållet visas efter det nya stycket (den andra parsysen innehåller nu det nya stycket).

* Redigeras och det andra stycket tas bort:

   * Det mellanliggande innehållet visas efter det stycke som tidigare var det tredje (det andra stycket innehåller nu det föregående tredje stycket).

* Har konfigurerats så att endast det första stycket visas:

   * Det mellanliggande innehållet visas inte (den andra parametern återges inte längre på grund av den nya konfigurationen).

### Anpassa komponenten Innehållsfragment {#customizing-the-content-fragment-component}

Om du vill använda fragmentkomponenten som finns i kartongen som en plan för tillägg måste du följa följande kontrakt:

* Återanvänd HTML-återgivningsskriptet och tillhörande POJO så att du kan se hur funktionen för mellanliggande innehåll implementeras.
* Återanvänd innehållets fragmentnod: `cq:editConfig`

   * Avlyssnarna `afterinsert`/ `afteredit`/ `afterdelete` används för att utlösa JS-händelser. Dessa händelser hanteras i klientbiblioteket `cq.authoring.editor.plugin.cfm` för att visa det associerade innehållet på sidopanelen.
   * `cq:dropTargets` har konfigurerats för att kunna dra innehållsfragmentresurser.
   * `cq:inplaceEditing` har konfigurerats för att stödja redigering av ett innehållsfragment i sidredigeraren. Fragmentets lokala redigerare definieras i klientbiblioteket `cq.authoring.editor.plugin.cfm` och tillåter en snabblänk att öppna det aktuella [elementet/varianten](/help/assets/content-fragments/content-fragments.md#constituent-parts-of-a-content-fragment) i [fragmentredigeraren](/help/assets/content-fragments/content-fragments-variations.md).

### Återgivning av resurser före återgivning {#asset-rewriting-before-rendering}

I Content Fragment Management används en intern återgivningsprocess för att generera det slutliga HTML-resultatet för en sida. Detta används internt av komponenten Content Fragment, men också av bakgrundsprocessen som uppdaterar refererade fragment på refererande sidor.

Internt används Sling Rewriter för den återgivningen. Motsvarande konfiguration finns på `/libs/dam/config/rewriter/cfm` och kan justeras om det behövs. Mer information finns i [Apache Sling Rewriter](https://sling.apache.org/documentation/bundles/output-rewriting-pipelines-org-apache-sling-rewriter.html).

>[!CAUTION]
>
>Om du justerar/täcker över konfigurationen för omskrivaren:
>
>* `/libs/dam/config/rewriter/cfm`
>
>Därefter måste `serializerType` **måste** uppdateras till:
>
>* `serializerType="html5-serializer"`

I konfigurationen som är klar att användas används följande transformatorer:

* `transformer-cfm-payloadfilter` - endast för att hämta `body`-delen ( `<body>...</body>`) av fragmentets HTML

* `transformer-cfm-parfilter` - filtrerar bort oönskade stycken om ett styckeintervall har angetts (som kan göras med komponenten Innehållsfragment)
* `transformer-cfm-assetprocessor` - används internt för att hämta en lista över resurser som är inbäddade i fragmentet

Återgivningsprocessen visas genom [`com.adobe.cq.dam.cfm.content.FragmentRenderService`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/dam/cfm/ContentFragment.html) och kan användas (till exempel) av anpassade komponenter, om det behövs.
