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
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1247'
ht-degree: 0%

---

# Använda Sling Resource Merger i AEM{#using-the-sling-resource-merger-in-aem}

## Syfte {#purpose}

Med Sling Resource Merger får du tillgång till och kan sammanfoga resurser. Den innehåller olika mekanismer (differentiering) för båda:

* **[Övertäckningar](/help/sites-developing/overlays.md)** med resurser som använder de [konfigurerade sökvägarna](/help/sites-developing/overlays.md#configuring-the-search-paths).

* **Åsidosätter** komponentdialogrutor för det beröringsaktiverade användargränssnittet (`cq:dialog`) med resurstyphierarkin (med egenskapen `sling:resourceSuperType`).

Med Sling Resource Merger sammanfogas överläggnings-/åsidosättningsresurserna och/eller egenskaperna med de ursprungliga resurserna/egenskaperna:

* Innehållet i den anpassade definitionen har en högre prioritet än det ursprungliga (det vill säga *övertäckningar* eller *åsidosätter*).

* Om det behövs visar [egenskaper](#properties) som definierats i anpassningen hur innehåll som sammanfogats från originalet ska användas.

>[!CAUTION]
>
>Sling Resource Merger och relaterade metoder kan bara användas med [Granite](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/granite-ui/api/index.html). Det innebär också att det bara är lämpligt för det vanliga användargränssnittet med pekfunktion. I synnerhet gäller de åsidosättningar som definieras på det här sättet bara för den beröringsaktiverade dialogrutan för en komponent.
>
>Övertäckningar/åsidosättningar för andra områden (inklusive andra aspekter av en beröringsaktiverad komponent eller det klassiska användargränssnittet) innefattar att kopiera lämplig nod och struktur från originalet till den plats där anpassningen ska definieras.

### Mål för AEM {#goals-for-aem}

Målet med Sling Resource Merger i AEM är att

* se till att inga anpassningar görs i `/libs`.
* minska strukturen som replikeras från `/libs`.

  När du använder Sling Resource Merger bör du inte kopiera hela strukturen från `/libs` eftersom det skulle resultera i att för mycket information hålls kvar i anpassningen (vanligen `/apps`). Om du duplicerar information i onödan ökar risken för problem när systemet uppgraderas på något sätt.

>[!NOTE]
>
>Åsidosättningar är inte beroende av sökvägarna, de använder egenskapen `sling:resourceSuperType` för att upprätta anslutningen.
>
>Åsidosättningar definieras dock ofta under `/apps`, vilket är den bästa metoden i AEM att definiera anpassningar under `/apps`. Detta beror på att du inte får ändra något under `/libs`.

>[!CAUTION]
>
>Du ***får*** inte ändra något i sökvägen `/libs`.
>
>Detta beror på att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan mycket väl skrivas över när du använder en snabbkorrigering eller ett funktionspaket).
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

  Anger om resurserna ska vara helt dolda, inklusive dess underordnade.

* `sling:hideChildren` ( `String` eller `String[]`)

  Innehåller den underordnade noden, eller listan med underordnade noder, som ska döljas. Egenskaperna för noden bevaras.

  Jokertecknet `*` döljer alla.

* `sling:orderBefore` ( `String`)

  Innehåller namnet på noden på samma nivå som den aktuella noden ska placeras framför.

De här egenskaperna påverkar hur motsvarande/ursprungliga resurser/egenskaper (från `/libs`) används av övertäckningen/åsidosättningen (ofta i `/apps`).

### Skapa strukturen {#creating-the-structure}

Om du vill skapa en övertäckning eller åsidosättning måste du återskapa den ursprungliga noden, med motsvarande struktur, under målet (vanligtvis `/apps`). Till exempel:

* Övertäckning

   * Definitionen av navigeringsposten för Sites-konsolen, som visas i järnvägen, definieras på:

     `/libs/cq/core/content/nav/sites/jcr:title`

   * Om du vill täcka över det här skapar du följande nod:

     `/apps/cq/core/content/nav/sites`

     Uppdatera sedan egenskapen `jcr:title` efter behov.

* Åsidosätt

   * Definitionen av den beröringsaktiverade dialogrutan för textkonsolen definieras på:

     `/libs/foundation/components/text/cq:dialog`

   * Om du vill åsidosätta detta skapar du följande nod, till exempel:

     `/apps/the-project/components/text/cq:dialog`

Om du vill skapa någon av dessa behöver du bara återskapa skelettstrukturen. För att förenkla återskapandet av strukturen kan alla mellanliggande noder vara av typen `nt:unstructured` (de behöver inte återspegla den ursprungliga nodtypen, till exempel i `/libs`).

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
>När du använder Sling Resource Merger (d.v.s. när du arbetar med standardgränssnittet med pekfunktioner) bör du inte kopiera hela strukturen från `/libs` eftersom det skulle resultera i att för mycket information hålls i `/apps`. Detta kan orsaka problem när systemet uppgraderas på något sätt.

### Användningsexempel {#use-cases}

Tillsammans med standardfunktionerna kan du:

* **Lägg till en egenskap**

  Egenskapen finns inte i definitionen `/libs`, men krävs i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod i `/apps`
   1. Skapa den nya egenskapen på den här noden &quot;

* **Definiera om en egenskap (inte automatiskt skapade egenskaper)**

  Egenskapen definieras i `/libs`, men ett nytt värde krävs i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod i `/apps`
   1. Skapa matchande egenskap på den här noden (under / `apps`)

      * Egenskapen får en prioritet baserat på konfigurationen för Sling Resource Resolver.
      * Det går att ändra egenskapstypen.

        Om du använder en annan egenskapstyp än den som används i `/libs` används den egenskapstyp som du definierar.

  >[!NOTE]
  >
  >Det går att ändra egenskapstypen.

* **Definiera om en egenskap som skapats automatiskt**

  Som standard omfattas inte automatiskt skapade egenskaper (till exempel `jcr:primaryType`) av någon övertäckning/åsidosättning för att säkerställa att den nodtyp som för närvarande finns under `/libs` respekteras. Om du vill införa en övertäckning/åsidosättning måste du återskapa noden i `/apps`, dölja egenskapen explicit och definiera om den:

   1. Skapa motsvarande nod under `/apps` med önskad `jcr:primaryType`
   1. Skapa egenskapen `sling:hideProperties` på den noden med värdet inställt på den automatiskt skapade egenskapen, till exempel `jcr:primaryType`

      Den här egenskapen, som definieras under `/apps`, får nu högre prioritet än den som definieras under `/libs`

* **Definiera om en nod och dess underordnade noder**

  Noden och dess underordnade noder definieras i `/libs`, men en ny konfiguration krävs i `/apps`-övertäckningen/åsidosättningen.

   1. Kombinera åtgärder från:

      1. Dölj underordnade noder för en nod (behåller nodens egenskaper)
      1. Definiera om egenskapen/egenskaperna

* **Dölj en egenskap**

  Egenskapen definieras i `/libs`, men krävs inte i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod i `/apps`
   1. Skapa egenskapen `sling:hideProperties` av typen `String` eller `String[]`. Använd den här inställningen för att ange vilka egenskaper som ska döljas/ignoreras. Du kan också använda jokertecken. Till exempel:

      * `*`
      * `["*"]`
      * `jcr:title`
      * `["jcr:title", "jcr:description"]`

* **Dölj en nod och dess underordnade noder**

  Noden och dess underordnade noder definieras i `/libs`, men krävs inte i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod under /apps
   1. Skapa en egenskap `sling:hideResource`

      * typ: `Boolean`
      * värde: `true`

* **Dölj underordnade noder för en nod (samtidigt som egenskaperna för noden behålls)**

  Noden, dess egenskaper och underordnade noder definieras i `/libs`. Noden och dess egenskaper krävs i `/apps`-övertäckningen/åsidosättningen, men vissa eller alla underordnade noder krävs inte i `/apps`-övertäckningen/åsidosättningen.

   1. Skapa motsvarande nod under `/apps`
   1. Skapa egenskapen `sling:hideChildren`:

      * typ: `String[]`
      * värde: en lista med underordnade noder (enligt definition i `/libs`) som ska döljas/ignoreras

      Jokertecknet &amp;ast; kan användas för att dölja/ignorera alla underordnade noder.

* **Ändra ordning på noder**

  Noden och dess jämställda noder definieras i `/libs`. En ny position krävs så att noden återskapas i `/apps`-övertäckningen/åsidosättningen, där den nya positionen definieras med referens till lämplig nod på samma nivå i `/libs`.

   * Använd egenskapen `sling:orderBefore`:

      1. Skapa motsvarande nod under `/apps`
      1. Skapa egenskapen `sling:orderBefore`:

         Detta anger noden (som i `/libs`) som den aktuella noden ska placeras före:

         * typ: `String`
         * värde: `<before-SiblingName>`

### Anropa Sling Resource Merger från koden {#invoking-the-sling-resource-merger-from-your-code}

Sling Resource Merger innehåller två anpassade resursprovidrar - en för övertäckningar och en annan för åsidosättningar. Var och en av dessa kan anropas i koden med hjälp av en monteringspunkt:

>[!NOTE]
>
>När du använder resursen bör du använda rätt monteringspunkt.
>
>Detta garanterar att Sling Resource Merger anropas och att den fullständigt sammanfogade resursen returneras (och reducerar strukturen som måste replikeras från `/libs`).

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
