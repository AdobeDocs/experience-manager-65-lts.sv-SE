---
title: Anpassa och utöka innehållsfragment
description: Ett innehållsfragment utökar en standardresurs. Lär dig hur du kan anpassa dem.
topic-tags: extending-aem
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Content Fragments
role: Developer
exl-id: 705bffea-ef70-40b5-81d8-b130d3908073
source-git-commit: 79cce324382bada2e9aec107b8e494723bf490e9
workflow-type: tm+mt
source-wordcount: '2687'
ht-degree: 0%

---

# Anpassa och utöka innehållsfragment{#customizing-and-extending-content-fragments}

Ett innehållsfragment utökar en standardresurs, se:

* [Skapa och hantera innehållsfragment](/help/assets/content-fragments/content-fragments.md) och [Skapa sidor med innehållsfragment](/help/sites-authoring/content-fragments.md) om du vill ha mer information om innehållsfragment.

* [Hantera Assets](/help/assets/manage-assets.md) och [Anpassa och utöka Assets](/help/assets/extending-assets.md) om du vill ha mer information om standardresurser.

## Arkitektur {#architecture}

De grundläggande [beståndsdelarna](/help/assets/content-fragments/content-fragments.md#constituent-parts-of-a-content-fragment) i ett innehållsfragment är:

* A *Content Fragment,*
* består av ett eller flera *innehållselement* s,
* och som kan ha en eller flera *innehållsvariationer* s.

Beroende på fragmenttypen används även modeller eller mallar:

>[!CAUTION]
>
>[Modeller för innehållsfragment](/help/assets/content-fragments/content-fragments-models.md) rekommenderas för att skapa alla nya fragment.
>
>Content Fragment Models används för alla exempel i WKND.

>[!NOTE]
>
>Före AEM 6.3 skapades innehållsfragment som bygger på mallar i stället för på modeller.
>
>Mallar för innehållsfragment är nu föråldrade. De kan fortfarande användas för att skapa fragment, men du bör använda Content Fragment Models i stället. Inga nya funktioner kommer att läggas till i fragmentmallar och de kommer att tas bort i en framtida version.

* Modeller för innehållsfragment:

   * Används för att definiera innehållsfragment som innehåller strukturerat innehåll.
   * Modeller för innehållsfragment definierar strukturen för ett innehållsfragment när det skapas.
   * Ett fragment refererar till modellen, så ändringar i modellen kan påverka alla beroende fragment.
   * Modeller är inbyggda i datatyper.
   * Funktioner för att lägga till nya varianter, och så vidare, måste uppdatera fragmentet därefter.

  >[!CAUTION]
  >
  >Alla ändringar i en befintlig innehållsfragmentmodell kan påverka beroende fragment, vilket kan leda till överblivna egenskaper i dessa fragment.

* Mallar för innehållsfragment:

   * Används för att definiera enkla innehållsfragment.
   * Mallar definierar (grundläggande, endast text) strukturen för ett innehållsfragment när det skapas.
   * Mallen kopieras till fragmentet när den skapas, så fler ändringar av mallen återspeglas inte i befintliga fragment.
   * Funktioner för att lägga till nya varianter, och så vidare, måste uppdatera fragmentet därefter.
      * När MIME-typen för innehållet baseras på en mall hanteras det faktiska innehållet. Det innebär att varje element och variant kan ha olika MIME-typer.

### Integrering med Assets {#integration-with-assets}

Content Fragment Management (CFM) ingår i AEM Assets som:

* Innehållsfragment är resurser.
* De använder befintliga Assets-funktioner.
* De är helt integrerade med Assets (administrationskonsoler och så vidare).

#### Mappa strukturerade innehållsfragment till Assets {#mapping-structured-content-fragments-to-assets}

![fragment-till-assets-Strucment](assets/fragment-to-assets-structured.png)

Innehållsfragment med strukturerat innehåll (d.v.s. baserat på en innehållsfragmentmodell) mappas till en enda resurs:

* Allt innehåll lagras under resursens `jcr:content/data`-nod:

   * Elementdata lagras under huvudundernoden:

     `jcr:content/data/master`

   * Variationer lagras under en undernod som har variantens namn:
till exempel `jcr:content/data/myvariation`

   * Data för varje element lagras i respektive undernod som en egenskap med elementnamnet:
Innehållet i elementet `text` lagras till exempel som egenskapen `text` på `jcr:content/data/master`

* Metadata och associerat innehåll lagras under `jcr:content/metadata`
Förutom rubriken och beskrivningen, som inte betraktas som traditionella metadata och lagras på `jcr:content`

#### Mappa enkla innehållsfragment till Assets {#mapping-simple-content-fragments-to-assets}

![chlimage_1-90](assets/chlimage_1-90.png)

Enkla innehållsfragment (som baseras på en mall) mappas till en sammansatt resurs som består av en huvudresurs och (valfritt) underresurser:

* All icke-innehållsinformation i ett fragment (som titel, beskrivning, metadata, struktur) hanteras exklusivt på huvudresursen.
* Innehållet i det första elementet i ett fragment mappas till huvudresursens ursprungliga återgivning.

   * Variationerna (om det finns några) för det första elementet mappas till andra återgivningar av huvudresursen.

* Ytterligare element (om sådana finns) mappas till deltillgångar i huvudtillgången.

   * Huvudinnehållet i dessa ytterligare element mappas till den ursprungliga återgivningen av respektive underresurs.
   * Andra variationer (om tillämpligt) av eventuella ytterligare element överensstämmer med andra återgivningar av respektive deltillgång.

#### Resursplats {#asset-location}

Precis som med standardresurser finns ett innehållsavdrag under:

`/content/dam`

#### Tillgångsbehörigheter {#asset-permissions}

Mer information finns i [Innehållsfragment - Ta bort överväganden](/help/assets/content-fragments/content-fragments-delete.md).

#### Funktionsintegrering {#feature-integration}

* Funktionen Content Fragment Management (CFM) bygger på Assets Core, men bör vara så oberoende som möjligt.
* CFM har sina egna implementeringar för objekt i vyerna kort, kolumn och lista; dessa plugins finns i de befintliga versionerna av Assets Content Rendering.
* Flera Assets-komponenter har utökats för att passa innehållsfragment.

### Använda innehållsfragment på sidor {#using-content-fragments-in-pages}

>[!CAUTION]
>
>[Kärnkomponenten för innehållsfragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=sv-SE) rekommenderas nu. Mer information finns i [Utveckla kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/overview.html?lang=sv-SE).

Innehållsfragment kan refereras från AEM-sidor, precis som andra resurstyper. AEM tillhandahåller kärnkomponenten [**Content Fragment**](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=sv-SE) - en [komponent som gör att du kan ta med innehållsfragment på dina sidor](/help/sites-authoring/content-fragments.md#adding-a-content-fragment-to-your-page). Du kan också utöka den här kärnkomponenten för **innehållsfragment**.

* Komponenten använder egenskapen `fragmentPath` för att referera till det faktiska innehållsfragmentet. Egenskapen `fragmentPath` hanteras på samma sätt som liknande egenskaper för andra resurstyper, till exempel när innehållsfragmentet flyttas till en annan plats.

* Med komponenten kan du välja den variant som ska visas.
* Dessutom kan ett styckeintervall markeras för att begränsa utdata. Det kan till exempel användas för utdata med flera kolumner.
* Komponenten tillåter [mellanliggande innehåll](/help/sites-developing/components-content-fragments.md#in-between-content):

   * Här kan du placera andra resurser (bilder och så vidare) mellan styckena i det refererade fragmentet.
   * För det mellanliggande innehållet behöver du:

      * vara medveten om risken för instabila referenser. Mellanliggande innehåll (som läggs till när en sida redigeras) har ingen fast relation till det stycke som det placeras bredvid, infogning av ett nytt stycke (i innehållsfragmentredigeraren) innan placeringen av det mellanliggande innehållet kan förlora den relativa positionen
      * ta hänsyn till ytterligare parametrar (som variation och styckefilter) för att undvika falska positiva resultat i sökresultaten

>[!NOTE]
>
>**Modell för innehållsfragment:**
>
>När du använder ett innehållsfragment som har baserats på en innehållsfragmentmodell på en sida, refereras modellen. Det innebär att om modellen inte har publicerats när du publicerar sidan, kommer den att flaggas och läggas till i resurserna som ska publiceras med sidan.
>
>**Mallen för innehållsfragment:**
>
>När du använder ett innehållsfragment som har baserats på en mall för innehållsfragment på en sida, finns det ingen referens när mallen kopierades när fragmentet skapades.

#### Konfiguration med OSGi-konsol {#configuration-using-osgi-console}

Serverdelsimplementeringen av innehållsfragment ansvarar till exempel för att göra instanser av ett fragment som används på en sida sökbara eller för hantering av blandat medieinnehåll. Den här implementeringen behöver veta vilka komponenter som används för att återge fragment och hur återgivningen är parametriserad.

Parametrarna för detta kan konfigureras i [webbkonsolen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-with-the-web-console) för OSGi-paketet **Konfiguration för innehållsfragmentkomponent**.

* **Resurstyper**
En lista med `sling:resourceTypes` kan tillhandahållas för att definiera komponenter som används för att återge innehållsfragment och var bakgrundsbearbetningen ska användas.

* **Referensegenskaper**
En lista med egenskaper kan konfigureras för att ange var referensen till fragmentet lagras för respektive komponent.

>[!NOTE]
>
>Det finns ingen direkt mappning mellan egenskap- och komponenttyp.
>
>AEM tar helt enkelt den första egenskapen som finns i ett stycke. Du bör därför välja egenskaperna noggrant.

![screenshot_2019-03-18at100941](assets/screenshot_2019-03-18at100941.png)

Det finns fortfarande några riktlinjer som du måste följa för att se till att komponenten är kompatibel med bakgrundsbearbetningen av innehållsfragment:

* Namnet på egenskapen där elementen som ska återges är antingen `element` eller `elementNames`.

* Namnet på egenskapen där varianten som ska återges definieras måste vara antingen `variation` eller `variationName`.

* Om utdata från flera element stöds (genom att använda `elementNames` för att ange flera element), definieras det faktiska visningsläget av egenskapen `displayMode`:

   * Om värdet är `singleText` (och endast ett element är konfigurerat) återges elementet som en text med mellanliggande innehåll, layoutstöd osv. Det här är standardinställningen för fragment där endast ett element återges.
   * I annat fall används en mycket enklare metod (kan kallas&quot;formulärvy&quot;), där inget mellanliggande innehåll stöds och fragmentinnehållet återges som det är.

* Om fragmentet återges för `displayMode` == `singleText` (implicit eller explicit) spelas följande ytterligare egenskaper upp:

   * `paragraphScope` definierar om alla stycken, eller bara ett intervall med stycken, ska återges (värden: `all` vs. `range`)

   * om `paragraphScope` == `range` definierar egenskapen `paragraphRange` intervallet med stycken som ska återges

### Integrering med andra ramar {#integration-with-other-frameworks}

Innehållsfragment kan integreras med:

* **Översättningar**

  Innehållsfragment är helt integrerade med [AEM översättningsarbetsflöde](/help/sites-administering/tc-manage.md). Arkitekturnivå innebär följande:

   * De enskilda översättningarna av ett innehållsfragment är egentligen separata fragment, till exempel:

      * De finns under olika språkrötter:

        `/content/dam/<path>/en/<to>/<fragment>`

        jämfört med

        `/content/dam/<path>/de/<to>/<fragment>`

      * men de delar exakt samma relativa sökväg under språkroten:

        `/content/dam/<path>/en/<to>/<fragment>`

        jämfört med

        `/content/dam/<path>/de/<to>/<fragment>`

   * Förutom de regelbaserade sökvägarna finns det ingen ytterligare koppling mellan de olika språkversionerna av ett innehållsfragment. De hanteras som två separata fragment, även om gränssnittet ger möjlighet att navigera mellan språkvarianterna.

  >[!NOTE]
  >
  >AEM översättningsarbetsflöde fungerar med `/content`:
  >
  >* Eftersom innehållsfragmentmodellerna finns i `/conf` inkluderas de inte i sådana översättningar. Du kan [internationalisera gränssnittssträngarna](/help/sites-developing/i18n-dev.md).
  >
  >* Mallar kopieras för att skapa fragmentet så detta är implicit.

* **Metadata-scheman**

   * Innehållsfragment (återanvänd) [metadatamatcheman](/help/assets/metadata-schemas.md) som kan definieras med standardresurser.
   * CFM har ett eget specifikt schema:

     `/libs/dam/content/schemaeditors/forms/contentfragment`

     Detta kan vid behov förlängas.

   * respektive schemaformulär är integrerat med fragmentredigeraren.

## API för hantering av innehållsfragment - serversidan {#the-content-fragment-management-api-server-side}

Du kan använda serversidans API för att komma åt dina innehållsfragment. Se:

[com.adobe.cq.dam.cfm](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/dam/cfm/package-summary.html)

>[!CAUTION]
>
>Vi rekommenderar att du använder serversidans API i stället för att komma åt innehållsstrukturen direkt.

### Nyckelgränssnitt {#key-interfaces}

Följande tre gränssnitt kan fungera som startpunkter:

* **Fragmentmall** ([FragmentTemplate](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/dam/cfm/FragmentTemplate.html))

  Använd `FragmentTemplate.createFragment()` för att skapa ett fragment.

  ```
  Resource templateOrModelRsc = resourceResolver.getResource("...");
  FragmentTemplate tpl = templateOrModelRsc.adaptTo(FragmentTemplate.class);
  ContentFragment newFragment = tpl.createFragment(parentRsc, "A fragment name", "A fragment description.");
  ```

  Gränssnittet representerar:

   * antingen en modell för innehållsfragment eller en mall för innehållsfragment från vilken ett innehållsfragment ska skapas,
   * och (efter det att fragmentet har skapats) strukturinformationen

  Denna information kan omfatta:

   * Få tillgång till grundläggande data (titel, beskrivning)
   * Få åtkomst till mallar/modeller för elementen i fragmentet:

      * Mallar för listelement
      * Hämta strukturinformation för ett givet element
      * Åtkomst till elementmallen (se `ElementTemplate`)

   * Åtkomstmallar för variationerna av fragmentet:

      * Lista variantmallar
      * Hämta strukturinformation för en viss variation
      * Åtkomst till variantmallen (se `VariationTemplate`)

   * Hämta initialt associerat innehåll

  Gränssnitt som representerar viktig information:

   * `ElementTemplate`

      * Hämta grundläggande data (namn, titel)
      * Hämta ursprungligt elementinnehåll

   * `VariationTemplate`

      * Hämta grundläggande data (namn, titel, beskrivning)

* **Innehållsfragment** ([ContentFragment](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/dam/cfm/ContentFragment.html))

  Med det här gränssnittet kan du arbeta med ett innehållsfragment på ett abstrakt sätt.

  >[!CAUTION]
  >
  >Vi rekommenderar starkt att du använder ett fragment via det här gränssnittet. Man bör undvika att ändra innehållsstrukturen direkt.

  Gränssnittet ger dig möjlighet att

   * Hantera grundläggande data (till exempel get name; get/set title/description)
   * Åtkomst till metadata
   * Åtkomstelement:

      * Listelement
      * Hämta element efter namn
      * Skapa nya element (se [Caveats](#caveats))

      * Åtkomst till elementdata (se `ContentElement`)

   * Listvarianter definierade för fragmentet
   * Skapa nya varianter globalt
   * Hantera associerat innehåll:

      * Listsamlingar
      * Lägg till samlingar
      * Ta bort samlingar

   * Åtkomst till fragmentets modell eller mall

  Gränssnitt som representerar de primära elementen i ett fragment är:

   * **Innehållselement** ([ContentElement](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/dam/cfm/ContentElement.html))

      * Hämta grundläggande data (namn, titel, beskrivning)
      * Hämta/ange innehåll
      * Få åtkomst till varianter av ett element:

         * Listvarianter
         * Hämta varianter efter namn
         * Skapa nya varianter (se [Caveats](#caveats))
         * Ta bort variationer (se [Caveats](#caveats))
         * Åtkomst till variantdata (se `ContentVariation`)

      * Kortkommando för att matcha variationer (tillämpa ytterligare, implementeringsspecifik reservlogik om den angivna varianten inte är tillgänglig för ett element)

   * **Innehållsvariation** ([ContentVariation](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/dam/cfm/ContentVariation.html))

      * Hämta grundläggande data (namn, titel, beskrivning)
      * Hämta/ange innehåll
      * Enkel synkronisering, baserat på den senast ändrade informationen

  Alla tre gränssnitten ( `ContentFragment`, `ContentElement`, `ContentVariation`) utökar gränssnittet `Versionable`, som lägger till versionshantering som krävs för innehållsfragment:

   * Skapa en ny version av elementet
   * Lista versioner av elementet
   * Hämta innehållet i en specifik version av det versionshanterade elementet

### Adapting - Using customito() {#adapting-using-adaptto}

Följande kan anpassas:

* `ContentFragment` kan anpassas till:

   * `Resource` - den underliggande Sling-resursen. Observera att om du uppdaterar den underliggande `Resource` direkt måste du återskapa `ContentFragment`-objektet.

   * `Asset` - DAM `Asset`-förkortningen som representerar innehållsfragmentet. Observera att om du uppdaterar `Asset` direkt måste `ContentFragment`-objektet återskapas.

* `ContentElement` kan anpassas till:

   * `ElementTemplate` - för åtkomst till elementets strukturinformation.

* `FragmentTemplate` kan anpassas till:

   * `Resource` - den `Resource` som avgör den refererade modellen eller den ursprungliga mallen som kopierades;

      * ändringar som görs via `Resource` återspeglas inte automatiskt i `FragmentTemplate`.

* `Resource` kan anpassas till:

   * `ContentFragment`
   * `FragmentTemplate`

### Caveats {#caveats}

Det bör noteras att

* API:t implementeras för att tillhandahålla funktioner som stöds av användargränssnittet.
* Hela API:t är utformat för att **inte** ska behålla ändringar automatiskt (om inget annat anges i API JavaDoc). Därför måste du alltid implementera resurslösaren för respektive begäran (eller den lösare som du använder).
* Uppgifter som kan kräva ytterligare arbete:

   * När du skapar/tar bort nya element uppdateras inte datastrukturen för enkla fragment (baserat på en fragmentmall).
   * Om du skapar nya varianter från `ContentElement` uppdateras inte datastrukturen (men de skapas globalt från `ContentFragment` will).

   * Om du tar bort befintliga varianter uppdateras inte datastrukturen.

## API:t för hantering av innehållsfragment - klientsidan {#the-content-fragment-management-api-client-side}

>[!CAUTION]
>
>För AEM 6.5 är klientsidans API internt.

### Ytterligare information {#additional-information}

Se följande:

* `filter.xml`

  `filter.xml` för hantering av innehållsfragment har konfigurerats så att den inte överlappar Assets kärninnehållspaket.

## Redigera sessioner {#edit-sessions}

En redigeringssession startas när användaren öppnar ett innehållsfragment på någon av redigeringssidorna. Redigeringssessionen avslutas när användaren lämnar redigeraren genom att välja **Spara** eller **Avbryt**.

### Krav {#requirements}

Krav för att styra en redigeringssession är:

* Att redigera ett innehållsfragment, som kan sträcka sig över flera vyer (= HTML-sidor), bör vara atomiskt.
* Redigeringen ska också vara *transaktionell*. I slutet av redigeringssessionen måste ändringarna antingen verkställas (sparas) eller återställas (avbrytas).
* Edge-ärenden ska hanteras på rätt sätt, t.ex. när användaren lämnar sidan genom att ange en URL manuellt eller med global navigering.
* Det bör finnas en periodisk autosparfunktion (var x:e minut) för att förhindra dataförlust.
* Om ett innehållsfragment redigeras av två användare samtidigt bör de inte skriva över varandras ändringar.

#### Processer {#processes}

Processerna är följande:

* Starta en session

   * En ny version av innehållsfragmentet skapas.
   * Spara automatiskt startas.
   * Cookies är inställda. Dessa definierar det redigerade fragmentet och att en redigeringssession är öppen.

* Avsluta en session

   * Autosparande stoppas.
   * Vid implementering:

      * Den senast ändrade informationen uppdateras.
      * Cookies tas bort.

   * Vid återställning:

      * Den version av innehållsfragmentet som skapades när redigeringssessionen startades återställs.
      * Cookies tas bort.

* Redigering

   * Alla ändringar (autosparande ingår) görs på det aktiva innehållsfragmentet - inte i ett avgränsat, skyddat område.
   * Därför återspeglas dessa ändringar direkt på AEM-sidor som refererar till respektive innehållsfragment

#### Åtgärder {#actions}

Möjliga åtgärder är:

* Ange en sida

   * Kontrollera om det redan finns en redigeringssession genom att markera respektive cookie.

      * Om det finns någon kontrollerar du att redigeringssessionen startades för det innehållsfragment som redigeras just nu

         * Om det aktuella fragmentet används återupprättar du sessionen.
         * Om inte, försök att avbryta redigeringen för det tidigare redigerade innehållsfragmentet och ta bort cookies (ingen redigeringssession finns efteråt).

      * Om det inte finns någon redigeringssession väntar du på den första ändringen som gjorts av användaren (se nedan).

   * Kontrollera om det redan finns referenser till innehållsfragmentet på en sida och visa lämplig information om så är fallet.

* Innehållsändring

   * När användaren ändrar innehåll och det inte finns någon redigeringssession skapas en ny redigeringssession (se [Starta en session](#processes)).

* Lämna en sida

   * Om det finns en redigeringssession och ändringarna inte har sparats, visas en modal bekräftelsedialogruta som meddelar användaren om potentiellt förlorat innehåll och låter dem stanna kvar på sidan.

## Exempel {#examples}

### Exempel: Åtkomst till ett befintligt innehållsfragment {#example-accessing-an-existing-content-fragment}

För att uppnå detta kan du anpassa resursen som representerar API:t till:

`com.adobe.cq.dam.cfm.ContentFragment`

Till exempel:

```java
// first, get the resource
Resource fragmentResource = resourceResolver.getResource("/content/dam/fragments/my-fragment");
// then adapt it
if (fragmentResource != null) {
    ContentFragment fragment = fragmentResource.adaptTo(ContentFragment.class);
    // the resource is now accessible through the API
}
```

### Exempel: Skapa ett innehållsfragment {#example-creating-a-new-content-fragment}

Om du vill skapa ett innehållsfragment programmatiskt måste du använda:

`com.adobe.cq.dam.cfm.ContentFragmentManager#create`

Till exempel:

```java
Resource templateOrModelRsc = resourceResolver.getResource("...");
FragmentTemplate tpl = templateOrModelRsc.adaptTo(FragmentTemplate.class);
ContentFragment newFragment = tpl.createFragment(parentRsc, "A fragment name", "A fragment description.");
```

### Exempel: Ange intervall för autosparande {#example-specifying-the-auto-save-interval}

Intervallet för att spara automatiskt (anges i sekunder) kan definieras med konfigurationshanteraren (ConfMgr):

* Nod: `<*conf-root*>/settings/dam/cfm/jcr:content`
* Egenskapsnamn: `autoSaveInterval`
* Typ: `Long`

* Standard: `600` (10 minuter); detta definieras på `/libs/settings/dam/cfm/jcr:content`

Om du vill ange ett intervall för automatiskt sparande på 5 minuter måste du definiera egenskapen på noden, till exempel:

* Nod: `/conf/global/settings/dam/cfm/jcr:content`
* Egenskapsnamn: `autoSaveInterval`

* Typ: `Long`

* Värde: `300` (5 minuter motsvarar 300 sekunder)

## Komponenter för sidredigering {#components-for-page-authoring}

Mer information finns på

* [Kärnkomponenter - Innehållsfragmentkomponent](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=sv-SE) (rekommenderas)
* [Content Fragment Components - Components for Page Authoring](/help/sites-developing/components-content-fragments.md#components-for-page-authoring)
