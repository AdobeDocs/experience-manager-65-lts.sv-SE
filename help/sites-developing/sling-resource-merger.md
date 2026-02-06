---
title: Använda Sling Resource Merger i AEM
description: Med Sling Resource Merger får du tillgång till och kan sammanfoga resurser
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 6fb6e522-fb81-4ba2-90b2-aad68f8bfa9e
source-git-commit: 9bc1cad84bb14b7513ede1fff2c1a37768dac442
workflow-type: tm+mt
source-wordcount: '1238'
ht-degree: 0%

---

# Använda Sling Resource Merger i AEM{#using-the-sling-resource-merger-in-aem}

## Syfte {#purpose}

Med Sling Resource Merger får du tillgång till och kan sammanfoga resurser. Den innehåller olika mekanismer (differentiering) för båda:

* **[Övertäckningar](/help/sites-developing/overlays.md)** med resurser som använder de [konfigurerade sökvägarna](/help/sites-developing/overlays.md#configuring-the-search-paths).

* **Åsidosätter** komponentdialogrutor för det beröringsaktiverade användargränssnittet (`cq:dialog`) med resurstyphierarkin (med egenskapen `sling:resourceSuperType`).

Sling Resource Merger kombinerar både överläggnings- och åsidosättningsresurser (och deras egenskaper) med de ursprungliga resurserna och egenskaperna:

* Innehållet i den anpassade definitionen har en högre prioritet än det ursprungliga. Det innebär att det är *övertäckningar* eller *åsidosätter*.

* Om det behövs visar [egenskaper](#properties) som definierats i anpassningen hur innehåll som sammanfogats från originalet ska användas.

>[!CAUTION]
>
>Sling Resource Merger och relaterade metoder kan bara användas med [Granite](https://developer.adobe.com/experience-manager/reference-materials/6-5/granite-ui/api/index.html). Detta innebär också att det bara är lämpligt för det pekaktiverade standardgränssnittet. I synnerhet gäller de åsidosättningar som definieras på det här sättet bara för den beröringsaktiverade dialogrutan för en komponent.
>
>Om du vill täcka över eller åsidosätta andra områden (inklusive andra delar av en beröringsaktiverad komponent eller det klassiska användargränssnittet) kopierar du lämplig nod och struktur från originalet. Placera kopian där du definierar anpassningen.

### Mål för AEM {#goals-for-aem}

Målet med Sling Resource Merger i AEM är att

* se till att inga anpassningar görs i `/libs`.
* minska strukturen som replikeras från `/libs`.

  När du använder Samling Resource Merger bör du inte kopiera hela strukturen från `/libs`. Orsaken är att det skulle resultera i att för mycket information sparas i anpassningen (vanligen `/apps`). Om du duplicerar information i onödan ökar risken för problem när systemet uppgraderas.

>[!NOTE]
>
>Åsidosättningar beror inte på sökvägarna. De använder egenskapen `sling:resourceSuperType` för att upprätta anslutningen.
>
>Åsidosättningar definieras dock ofta under `/apps`, som det bästa sättet i AEM är att definiera anpassningar under `/apps`. Orsaken är att du inte får ändra något under `/libs`.

>[!CAUTION]
>
>*Ändra inte* något i sökvägen `/libs`.
>
>Orsaken är att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans. Och det kan mycket väl skrivas över när du lägger till en snabbkorrigering eller ett funktionspaket.
>
>Den rekommenderade metoden för konfiguration och andra ändringar är:
>
>1. Återskapa det obligatoriska objektet (det vill säga som det finns i `/libs`) under `/apps`
>
>1. Gör ändringar i `/apps`
>

### Egenskaper {#properties}

Resurskoncentrationen har följande egenskaper:

* `sling:hideProperties` ( `String` eller `String[]`)

  Anger den egenskap, eller lista med egenskaper, som ska döljas.

  Jokertecknet `*` döljer alla.

* `sling:hideResource` ( `Boolean`)

  Det anger om resurserna är helt dolda, inklusive dess underordnade.

* `sling:hideChildren` ( `String` eller `String[]`)

  Den innehåller den underordnade noden, eller listan med underordnade noder, som ska döljas. Egenskaperna för noden bevaras.

  Jokertecknet `*` döljer alla.

* `sling:orderBefore` ( `String`)

  Den innehåller namnet på noden på samma nivå som den aktuella noden är placerad framför.

De här egenskaperna påverkar hur motsvarande/ursprungliga resurser/egenskaper (från `/libs`) används av övertäckningen/åsidosättningen (ofta i `/apps`).

### Skapa strukturen {#creating-the-structure}

Om du vill skapa en övertäckning eller åsidosättning måste du återskapa den ursprungliga noden, med motsvarande struktur, under målet (vanligtvis `/apps`). Till exempel:

* Övertäckning

   * Definitionen av navigeringsposten för Sites-konsolen, som visas i järnvägen, definieras på:

     `/libs/cq/core/content/nav/sites/jcr:title`

   * Skapa följande nod för att täcka över:

     `/apps/cq/core/content/nav/sites`

     Uppdatera sedan egenskapen `jcr:title` efter behov.

* Åsidosätt

   * Definitionen av den beröringsaktiverade dialogrutan för textkonsolen definieras så här:

     `/libs/foundation/components/text/cq:dialog`

   * Om du vill åsidosätta skapar du följande nod. Till exempel:

     `/apps/the-project/components/text/cq:dialog`

Om du vill skapa någon av dem behöver du bara återskapa skelettstrukturen. För att förenkla återskapandet av strukturen kan alla mellanliggande noder vara av typen `nt:unstructured` (de behöver inte återspegla den ursprungliga nodtypen). Till exempel i `/libs`.

I ovanstående övertäckningsexempel behövs alltså följande noder:

```shell
/apps
  /cq
    /core
      /content
        /nav
          /sites
```

>[!NOTE]
>
>När du använder Sling Resource Merger (d.v.s. när du arbetar med standardgränssnittet med pekfunktioner) bör du inte kopiera hela strukturen från `/libs`. Orsaken är att det skulle resultera i att för mycket information sparas i `/apps`. Detta kan leda till problem när systemet uppgraderas.

### Användningsexempel {#use-cases}

Med standardfunktionalitet kan du göra följande med de här användningsexemplen:

* **Lägg till en egenskap**

  Egenskapen finns inte i definitionen `/libs`, men krävs i övertäckningen `/apps`/åsidosättningen.

   1. Skapa motsvarande nod i `/apps`
   1. Skapa den nya egenskapen på den här noden &quot;

* **Definiera om en egenskap (inte automatiskt skapade egenskaper)**

  Egenskapen definieras i `/libs`, men ett nytt värde krävs i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod i `/apps`
   1. Skapa matchande egenskap på den här noden (under `apps`)

      * Egenskapen har en prioritet som baseras på konfigurationen av Sling Resource Resolver.
      * Det går att ändra egenskapstypen.

        Om du använder en annan egenskapstyp än den som används i `/libs` används den egenskapstyp som du definierar.

  >[!NOTE]
  >
  >Det går att ändra egenskapstypen.

* **Definiera om en egenskap som skapats automatiskt**

  Som standard omfattas inte automatiskt skapade egenskaper (till exempel `jcr:primaryType`) av någon övertäckning eller åsidosättning för att säkerställa att den nodtyp som för närvarande finns under `/libs` respekteras. Om du vill lägga till en övertäckning/åsidosättning måste du återskapa noden i `/apps`, dölja egenskapen explicit och definiera om den:

   1. Skapa motsvarande nod under `/apps` med önskad `jcr:primaryType`
   1. Skapa egenskapen `sling:hideProperties` på den noden med värdet inställt på den automatiskt skapade egenskapen, till exempel `jcr:primaryType`

      Den här egenskapen, som definieras under `/apps`, har nu högre prioritet än den som definieras under `/libs`

* **Definiera om en nod och dess underordnade noder**

  Noden och dess underordnade noder definieras i `/libs`, men en ny konfiguration krävs i `/apps`-övertäckningen/åsidosättningen.

   1. Kombinera åtgärder från:

      1. Dölj underordnade noder för en nod (behåller nodens egenskaper)
      1. Definiera om egenskapen/egenskaperna

* **Dölj en egenskap**

  Egenskapen definieras i `/libs`, men krävs inte i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod i `/apps`
   1. Skapa egenskapen `sling:hideProperties` av typen `String` eller `String[]`. Används för att ange de egenskaper som ska döljas/ignoreras. Du kan också använda jokertecken. Till exempel:

      * `*`
      * `["*"]`
      * `jcr:title`
      * `["jcr:title", "jcr:description"]`

* **Dölj en nod och dess underordnade noder**

  Noden och dess underordnade noder definieras i `/libs`, men krävs inte i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod under `/apps`
   1. Skapa en egenskap `sling:hideResource`

      * typ: `Boolean`
      * värde: `true`

* **Dölj underordnade noder för en nod (samtidigt som egenskaperna för noden behålls)**

  Noden, dess egenskaper och underordnade noder definieras i `/libs`. Noden och dess egenskaper krävs i `/apps`-övertäckningen/åsidosättningen, men vissa eller alla underordnade noder krävs inte i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod under `/apps`
   1. Skapa egenskapen `sling:hideChildren`:

      * typ: `String[]`
      * värde: en lista med underordnade noder (enligt definition i `/libs`) som ska döljas/ignoreras

      Jokertecknet &ast; kan användas för att dölja eller ignorera alla underordnade noder.

* **Ändra ordning på noder**

  Noden och dess jämställda noder definieras i `/libs`. Om du vill ändra ordningen återskapar du noden i `/apps`-övertäckningen eller åsidosättningen. Definiera den nya positionen genom att referera till rätt jämställd nod i `/libs`.


   * Använd egenskapen `sling:orderBefore`:

      1. Skapa motsvarande nod under `/apps`
      1. Skapa egenskapen `sling:orderBefore`:

         Anger noden (som i `/libs`) som den aktuella noden är placerad före:

         * typ: `String`
         * värde: `<before-SiblingName>`

### Anropa Sling Resource Merger från koden {#invoking-the-sling-resource-merger-from-your-code}

Sling Resource Merger innehåller två anpassade resursprovidrar - en för övertäckningar och en annan för åsidosättningar. Var och en kan anropas i koden med en monteringspunkt:

>[!NOTE]
>
>När du använder resursen bör du använda rätt monteringspunkt.
>
>Den här metoden anropar den sammanslagna resursen och returnerar den fullständigt sammanslagna resursen. Det minskar också mängden struktur som du måste kopiera från `/libs`.

* Övertäckning:

   * syfte: sammanfoga resurser baserat på deras sökväg
   * monteringspunkt: `/mnt/overlay`
   * användning: `mount point + relative path`
   * exempel:

      * `getResource('/mnt/overlay' + '<relative-path-to-resource>');`

* Åsidosätt:

   * syfte: sammanfoga resurser baserat på deras supertyp
   * monteringspunkt: `/mnt/overide`
   * användning: `mount point + absolute path`
   * exempel:

      * `getResource('/mnt/override' + '<absolute-path-to-resource>');`

### Exempel på användning {#example-of-usage}

Här följer några exempel:

* Övertäckning:

   * [Anpassa konsolerna](/help/sites-developing/customizing-consoles-touch.md)
   * [Anpassa sidredigering](/help/sites-developing/customizing-page-authoring-touch.md)

* Åsidosätt:

   * [Konfigurera dina sidegenskaper](/help/sites-developing/page-properties-views.md#configuring-your-page-properties)
