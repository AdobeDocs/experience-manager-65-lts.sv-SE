---
title: Anpassa webbplatskonsolen (Classic UI)
description: Konsolen Administrera webbplatser kan utökas till att visa anpassade kolumner
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 0%

---

# Anpassa webbplatskonsolen (Classic UI){#customizing-the-websites-console-classic-ui}

## Lägga till en anpassad kolumn i webbplatskonsolen (siteadmin) {#adding-a-custom-column-to-the-websites-siteadmin-console}

Konsolen Administrera webbplatser kan utökas till att visa anpassade kolumner. Konsolen byggs baserat på ett JSON-objekt som kan utökas genom att en OSGI-tjänst skapas som implementerar `ListInfoProvider`-gränssnittet. En sådan tjänst ändrar JSON-objektet som skickas till klienten för att bygga konsolen.

I den här steg-för-steg-självstudiekursen beskrivs hur du visar en ny kolumn i administrationskonsolen för webbplatser genom att implementera gränssnittet `ListInfoProvider`. Det består av följande steg:

1. [Skapar OSGI-tjänsten](#creating-the-osgi-service) och distribuerar det paket som innehåller den till AEM-servern.
1. (valfritt) [Testar den nya tjänsten](#testing-the-new-service) genom att utfärda ett JSON-anrop för att begära JSON-objektet som används för att skapa konsolen.
1. [Visar den nya kolumnen](#displaying-the-new-column) genom att utöka nodstrukturen för konsolen i databasen.

>[!NOTE]
>
>Den här självstudien kan även användas för att utöka följande administrationskonsoler:
>
>* Assets Digital Console
>* Community-konsolen
>

### Skapa OSGI-tjänsten {#creating-the-osgi-service}

Gränssnittet `ListInfoProvider` definierar två metoder:

* `updateListGlobalInfo`, om du vill uppdatera globala egenskaper för listan,
* `updateListItemInfo`, om du vill uppdatera ett listobjekt.

Argumenten för båda metoderna är:

* `request`, det associerade Sling HTTP-begäranobjektet,
* `info`, JSON-objektet som ska uppdateras, vilket är den globala listan eller det aktuella listobjektet,
* `resource`, en Sling-resurs.

Exempelimplementeringen är nedan:

* Lägger till en *stjärnröd*-egenskap för varje objekt, vilket är `true` om sidnamnet börjar med *e* och `false` i annat fall.

* Lägger till en *starredCount* -egenskap som är global för listan och innehåller antalet stjärnlistobjekt.

Så här skapar du OSGI-tjänsten:

1. I CRXDE Lite [skapar du ett paket](/help/sites-developing/developing-with-crxde-lite.md#managing-a-bundle).
1. Lägg till exempelkoden nedan.
1. Bygg paketet.

Den nya tjänsten körs.

```java
package com.test;

import com.day.cq.commons.ListInfoProvider;
import com.day.cq.i18n.I18n;
import com.day.cq.wcm.api.Page;
import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.commons.json.JSONException;
import org.apache.sling.commons.json.JSONObject;

@Component(metatype = false)
@Service(value = ListInfoProvider.class)
public class StarredListInfoProvider implements ListInfoProvider {

    private int count = 0;

    public void updateListGlobalInfo(SlingHttpServletRequest request, JSONObject info, Resource resource) throws JSONException {
        info.put("starredCount", count);
        count = 0; // reset for next execution
    }

    public void updateListItemInfo(SlingHttpServletRequest request, JSONObject info, Resource resource) throws JSONException {
        Page page = resource.adaptTo(Page.class);
        if (page != null) {
            // Consider starred if page name starts with 'e'
            boolean starred = page.getName().startsWith("e");
            if (starred) {
                count++;
            }
            I18n i18n = new I18n(request);
            info.put("starred", starred ? i18n.get("Yes") : i18n.get("No"));
        }
    }

}
```

>[!CAUTION]
>
>* Implementeringen bör, baserat på den angivna begäran och/eller resursen, avgöra om den ska lägga till informationen till JSON-objektet eller inte.
>* Om implementeringen av `ListInfoProvider` definierar en egenskap som finns i svarsobjektet skrivs dess värde över av den egenskap du anger.
>
>  Du kan använda [tjänstrankning](https://docs.osgi.org/javadoc/r2/org/osgi/framework/Constants.html#SERVICE_RANKING) för att hantera körningsordningen för flera `ListInfoProvider`-implementeringar.

### Testa den nya tjänsten {#testing-the-new-service}

När du öppnar administrationskonsolen för webbplatser och bläddrar igenom webbplatsen skickar webbläsaren ett Ajax-anrop för att hämta JSON-objektet som används för att skapa konsolen. Om du till exempel bläddrar till mappen `/content/geometrixx` skickas följande begäran till AEM-servern för att skapa konsolen:

[https://localhost:4502/content/geometrixx.pages.json?start=0&amp;limit=30&amp;predicate=siteadmin](https://localhost:4502/content/geometrixx.pages.json?start=0&amp;limit=30&amp;predicate=siteadmin)

Så här kontrollerar du att den nya tjänsten körs efter att du har distribuerat paketet som innehåller den:

1. Peka webbläsaren på följande URL:
   [https://localhost:4502/content/geometrixx.pages.json?start=0&amp;limit=30&amp;predicate=siteadmin](https://localhost:4502/content/geometrixx.pages.json?start=0&amp;limit=30&amp;predicate=siteadmin)

1. Svaret ska visa de nya egenskaperna enligt följande:

![screen_shot_2012-02-13at163046](assets/screen_shot_2012-02-13at163046.png)

### Visa den nya kolumnen {#displaying-the-new-column}

Det sista steget består i att anpassa nodstrukturen i administrationskonsolen för webbplatser så att den nya egenskapen för alla Geometrixx-sidor visas genom att ersätta `/libs/wcm/core/content/siteadmin`. Gör så här:

1. Skapa nodstrukturen `/apps/wcm/core/content` med noder av typen `sling:Folder` i CRXDE Lite så att den återspeglar strukturen `/libs/wcm/core/content`.

1. Kopiera noden `/libs/wcm/core/content/siteadmin` och klistra in den under `/apps/wcm/core/content`.

1. Kopiera noden `/apps/wcm/core/content/siteadmin/grid/assets` till `/apps/wcm/core/content/siteadmin/grid/geometrixx` och ändra dess egenskaper:

   * Ta bort **pageText**

   * Ange **pathRegex** till `/content/geometrixx(/.*)?`
Detta gör att stödrasterkonfigurationen är aktiv för alla Geometrixx webbplatser.

   * Ange **storeProxySuffix** till `.pages.json`

   * Redigera flervärdesegenskapen **storeReaderFields** och lägg till värdet `starred`.

   * Om du vill aktivera MSM-funktioner lägger du till följande MSM-parametrar i multi-String-egenskapen **storeReaderFields**:

      * **msm:isSource**
      * **msm:isInBlueprint**
      * **msm:isLiveCopy**

1. Lägg till en `starred`-nod (av typen **nt:unsigned**) nedanför `/apps/wcm/core/content/siteadmin/grid/geometrixx/columns` med följande egenskaper:

   * **dataIndex**: `starred` av typen String

   * **header**: `Starred` av typen String

   * **xtype**: `gridcolumn` av typen String

1. (valfritt) Släpp de kolumner som du inte vill visa på `/apps/wcm/core/content/siteadmin/grid/geometrixx/columns`

1. `/siteadmin` är en vanlighetssökväg som som standard pekar på `/libs/wcm/core/content/siteadmin`.
Om du vill omdirigera detta till din version av platsadmin på `/apps/wcm/core/content/siteadmin` definierar du egenskapen `sling:vanityOrder` så att den har ett högre värde än det som definierats på `/libs/wcm/core/content/siteadmin`. Standardvärdet är 300, så allt högre är lämpligt.

1. Gå till administrationskonsolen för webbplatser och navigera till Geometrixx webbplats:
   [https://localhost:4502/siteadmin#/content/geometrixx](https://localhost:4502/siteadmin#/content/geometrixx).

1. Den nya kolumnen **Starred** är tillgänglig och visar anpassad information enligt följande:

![screen_shot_2012-02-14at104602](assets/screen_shot_2012-02-14at104602.png)

>[!CAUTION]
>
>Om flera stödrasterkonfigurationer matchar den begärda sökvägen som definieras av egenskapen **pathRegex** används den första, och inte den mest specifika, vilket betyder att konfigurationernas ordning är viktig.

### Exempelpaket {#sample-package}

Resultatet av den här självstudiekursen finns i [Anpassa paketet Administrationskonsol för webbplatser](https://localhost:4502/crx/packageshare/index.html/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/helper/customizing-siteadmin) på paketresursen.
