---
title: Anpassa CIF Core-komponenter
description: Lär dig hur du anpassar Adobe Experience Manager CIF Core Components. Självstudiekursen handlar om hur du på ett säkert sätt kan utöka en CIF Core Component för att uppfylla företagsspecifika krav. Lär dig hur du utökar en GraphQL-fråga för att returnera ett anpassat attribut och visa det nya attributet i en CIF Core-komponent.
feature: Commerce Integration Framework
solution: Experience Manager,Commerce
role: Admin, Developer
exl-id: e8f2a771-b2e3-4f3e-85a0-480f783fc313
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2305'
ht-degree: 0%

---

# Anpassa Adobe Experience Manager CIF Core Components {#customize-cif-components}

[CIF Venia Project](https://github.com/adobe/aem-cif-guides-venia) är en referenskodbas för [CIF Core Components](https://github.com/adobe/aem-core-cif-components). I den här självstudiekursen utökar du [Product Teaser](https://github.com/adobe/aem-core-cif-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/cif/components/commerce/productteaser/v1/productteaser)-komponenten ytterligare för att visa ett anpassat attribut från Adobe Commerce. Du kan även läsa mer om GraphQL-integrationen mellan Adobe Experience Manager (AEM) och Adobe Commerce och de tilläggskopplingar som finns i CIF Core Components.

>[!TIP]
>
>Använd [AEM Project-arkivtypen](https://github.com/adobe/aem-project-archetype) när du startar en egen handelsimplementering.

## Vad du ska bygga

Varumärket Venia började nyligen tillverka vissa produkter med hjälp av hållbara material och företaget skulle vilja visa ett **miljövänligt**-märke som en del av Product Teaser. Ett nytt anpassat attribut skapas i Adobe Commerce för att ange om en produkt använder det **miljövänliga**-materialet. Det här anpassade attributet läggs till som en del av GraphQL-frågan och visas i Product Teaser för angivna produkter.

![Slutlig implementering av miljöanpassade märken](../assets/customize-cif-components/final-product-teaser-eco-badge.png)

## Förutsättningar {#prerequisites}

Det krävs en lokal utvecklingsmiljö för att slutföra den här självstudiekursen. Detta inkluderar en instans av AEM som körs och som är konfigurerad och ansluten till en Adobe Commerce-instans. Granska kraven och stegen för att [konfigurera en lokal utveckling med AEM](../develop.md). Om du vill följa självstudiekursen fullständigt måste du ha behörighet att lägga till [attribut till en produkt](https://docs.magento.com/user-guide/catalog/product-attributes-add.html) i Adobe Commerce.

Du behöver också GraphQL IDE, till exempel [GraphiQL](https://github.com/graphql/graphiql) eller ett webbläsartillägg, för att köra kodexempel och självstudiekurser. Om du installerar ett webbläsartillägg måste du se till att det går att ange begäranrubriker. I Google Chrome är _Altair GraphQL Client_ ett tillägg som kan utföra jobbet.

## Klona Veniaprojektet {#clone-venia-project}

Du klonar [Venedig-projektet](https://github.com/adobe/aem-cif-guides-venia) och åsidosätter sedan standardformaten.

>[!NOTE]
>
>**Du kan använda ett befintligt projekt** (baserat på AEM Project Archetype med CIF inkluderat) och hoppa över det här avsnittet.

1. Kör följande Git-kommando så att du kan klona projektet:

   ```shell
   $ git clone git@github.com:adobe/aem-cif-guides-venia.git
   ```

1. Bygg och distribuera projektet till en lokal instans av AEM:

   ```shell
   $ cd aem-cif-guides-venia/
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Lägg till nödvändiga OSGi-konfigurationer så att du kan ansluta din AEM-instans till en Adobe Commerce-instans eller lägga till konfigurationerna till det nyskapade projektet.

1. Nu bör du ha en fungerande version av en storefront som är ansluten till en Adobe Commerce-instans. Gå till sidan `US` > `Home` på: [http://localhost:4502/editor.html/content/venia/us/en.html](http://localhost:4502/editor.html/content/venia/us/en.html).

   Du ser att butiken för närvarande använder temat Venia. När du expanderar huvudmenyn för butiken bör du se olika kategorier som anger att anslutningen till Adobe Commerce fungerar.

   ![Storefront har konfigurerats med Veniatema](../assets/customize-cif-components/venia-store-configured.png)

## Skapa Product Teaser {#author-product-teaser}

Product Teaser Component utökas genom hela kursen. Som ett första steg lägger du till en instans av Product Teaser på startsidan för att förstå baslinjefunktionerna.

1. Navigera till webbplatsens **hemsida**: [http://localhost:4502/editor.html/content/acme/us/en.html](http://localhost:4502/editor.html/content/acme/us/en.html)

2. Infoga en ny **Product Teaser**-komponent i huvudlayoutbehållaren på sidan.

   ![Infoga Product Teaser](../assets/customize-cif-components/product-teaser-add-component.png)

3. Expandera sidopanelen (om den inte redan är aktiverad) och växla till listrutan **Produkter** för att hitta resurser. Här visas en lista över tillgängliga produkter från en ansluten Adobe Commerce-instans. Välj en produkt och **dra+släpp** den på komponenten **Product Teaser** på sidan.

   ![Dra och släpp Product Teaser](../assets/customize-cif-components/drag-drop-product-teaser.png)

   >[!NOTE]
   >
   >Observera att du även kan konfigurera den visade produkten genom att konfigurera komponenten med hjälp av dialogrutan (klicka på ikonen _skiftnyckel_ ).

4. Du bör nu se en produkt som visas av Product Teaser. Produktens namn och produktens pris är standardattribut som visas.

   ![Product Teaser - standardformat](../assets/customize-cif-components/product-teaser-default-style.png)

## Lägg till ett anpassat attribut i Adobe Commerce {#add-custom-attribute}

De produkter och produktdata som visas i AEM lagras i Adobe Commerce. Lägg sedan till ett attribut för **Eco-Friendly** som en del av produktattributuppsättningen med hjälp av användargränssnittet i Adobe Commerce.

>[!TIP]
>
>Har du redan ett anpassat **Yes/No**-attribut som en del av produktattributuppsättningen? Du kan använda den och hoppa över det här avsnittet.

1. Logga in på din Adobe Commerce-instans.
1. Navigera till **Katalog** > **Produkter**.
1. Uppdatera sökfiltret så att du kan hitta den **konfigurerbara produkt** som användes när den lades till i Teaser-komponenten i föregående övning. Öppna produkten i redigeringsläge.

   ![Sök efter Valeriaprodukt](../assets/customize-cif-components/search-valeria-product.png)

1. Klicka på **Lägg till attribut** > **Skapa nytt attribut** i produktvyn.
1. Fyll i formuläret **Nytt attribut** med följande värden (lämna standardinställningarna för andra värden)

   | Fältuppsättning | Fältetikett | Värde |
   | ----------------------------- | ------------------ | ---------------- |
   | Attributegenskaper | Attributetikett | **Eko-vänlig** |
   | Attributegenskaper | Indatatyp för katalog | **Ja/Nej** |
   | Avancerade attributegenskaper | Attributkod | **eco_friendly** |

   ![Nytt attributformulär](../assets/customize-cif-components/attribute-new-form.png)

   Klicka på **Spara attribut** när du är klar.

1. Bläddra till produktens nedre del och expandera rubriken **Attribut**. Det nya fältet **Eko-Friendly** ska visas. Växla till **Ja**.

   ![Växla till ja](../assets/customize-cif-components/eco-friendly-toggle-yes.png)

   **Spara** ändringarna i produkten.

   >[!TIP]
   >
   >Mer information om hur du hanterar [produktattribut finns i användarhandboken för Adobe Commerce](https://docs.magento.com/user-guide/catalog/attribute-best-practices.html).

1. Navigera till **System** > **Verktyg** > **Cachehantering**. Eftersom en uppdatering gjordes av dataschemat måste du göra vissa cachetyper i Adobe Commerce ogiltiga.
1. Markera rutan bredvid **Konfiguration** och skicka cachetypen för **Uppdatera**

   ![Uppdatera cachetyp för konfiguration](../assets/customize-cif-components/refresh-configuration-cache-type.png)

   >[!TIP]
   >
   >Mer information om [Cachehantering finns i användarhandboken för Adobe Commerce](https://docs.magento.com/user-guide/system/cache-management.html).

## Använd en GraphQL-utvecklingsmiljö för att verifiera attribut {#use-graphql-ide}

Innan du hoppar in i AEM-kod är det användbart att utforska [Adobe Commerce GraphQL](https://devdocs.magento.com/guides/v2.4/graphql/) med en GraphQL-utvecklingsmiljö. Adobe Commerce integrering med AEM sker huvudsakligen via en serie GraphQL-frågor. Att förstå och ändra GraphQL-frågor är ett av de viktigaste sätten att utöka CIF Core Components.

Använd sedan en GraphQL IDE för att verifiera att attributet `eco_friendly` har lagts till i produktattributuppsättningen. Skärmbilder i den här självstudien använder Google Chrome-tillägget _Altair GraphQL Client_.

1. Öppna GraphQL IDE och ange URL:en `http://<server>/graphql` i URL-fältet för IDE eller tillägget.
2. Lägg till följande [produktfråga](https://devdocs.magento.com/guides/v2.4/graphql/queries/products.html) där `YOUR_SKU` är **SKU** för den produkt som användes i föregående övning:

   ```json
     {
       products(
       filter: { sku: { eq: "YOUR_SKU" } }
       ) {
           items {
           name
           sku
           eco_friendly
           }
       }
   }
   ```

3. Kör frågan så får du ett svar som följande:

   ```json
   {
     "data": {
       "products": {
         "items": [
           {
             "name": "Valeria Two-Layer Tank",
             "sku": "VT11",
             "eco_friendly": 1
           }
         ]
       }
     }
   }
   ```

   ![Exempel på GraphQL-svar](../assets/customize-cif-components/sample-graphql-query.png)

Värdet **Yes** är heltalet **1**. Detta är användbart när du skriver GraphQL-frågan i Java™.

>[!TIP]
>
>Mer detaljerad dokumentation om Adobe Commerce GraphQL finns i följande [GraphQL-översikt](https://devdocs.magento.com/guides/v2.4/graphql/index.html).

## Uppdatera produktundervisningsmodellen {#updating-sling-model-product-teaser}

Därefter utökar du affärslogiken i Product Teaser genom att implementera en Sling Model. [Sling Models](https://sling.apache.org/documentation/bundles/models.html) är anteckningsdrivna &quot;POJOs&quot; (Plain Old Java™ Objects) som implementerar någon av de affärslogik som behövs för komponenten. Sling-modeller används med HTML-skript som en del av komponenten. Följ [delegeringsmönstret för Sling Models](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) så att du kan utöka delar av den befintliga Product Teaser-modellen.

Sling Models implementeras som Java™ och finns i **core** -modulen i det genererade projektet.

Använd [den utvecklingsmiljö du väljer](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html?lang=sv-SE#set-up-the-development-ide) för att importera Venia-projektet. De skärmbilder som används är från [Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html?lang=sv-SE&#microsoft-visual-studio-code).

1. I din IDE navigerar du under **core** -modulen till: `core/src/main/java/com/venia/core/models/commerce/MyProductTeaser.java`.

   ![Core location IDE](../assets/customize-cif-components/core-location-ide.png)

   `MyProductTeaser.java` är ett Java™-gränssnitt som utökar CIF [ProductTeaser](https://github.com/adobe/aem-core-cif-components/blob/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/models/productteaser/ProductTeaser.java) -gränssnitt.

   En ny metod har redan lagts till med namnet `isShowBadge()` för att visa ett emblem om produkten betraktas som&quot;ny&quot;.

1. Lägg till metoden `isEcoFriendly()` i gränssnittet:

   ```java
   @ProviderType
   public interface MyProductTeaser extends ProductTeaser {
       // Extend the existing interface with the additional properties which you
       // want to expose to the HTL template.
       public Boolean isShowBadge();
   
       public Boolean isEcoFriendly();
   }
   ```

Det här är en ny metod för att kapsla in logiken som anger om produkten har attributet `eco_friendly` inställt på **Yes** eller **No**.

1. Kontrollera sedan `MyProductTeaserImpl.java` på `core/src/main/java/com/venia/core/models/commerce/MyProductTeaserImpl.java`.

   [Delegeringsmönstret för delningsmodeller](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) gör att `MyProductTeaserImpl` kan referera till `ProductTeaser`-modellen via egenskapen `sling:resourceSuperType`:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private ProductTeaser productTeaser;
   ```

   För alla metoder som inte åsidosätts eller ändras kan du returnera värdet som returneras av `ProductTeaser`. Till exempel:

   ```java
   @Override
   public String getImage() {
       return productTeaser.getImage();
   }
   ```

   Detta minimerar mängden Java™-kod som en implementering måste skriva.

1. En av de extra tilläggspunkterna som tillhandahålls av AEM CIF Core Components är `AbstractProductRetriever` som ger åtkomst till specifika produktattribut. Inspektera metoden `initModel()`:

   ```java
   import javax.annotation.PostConstruct;
   ...
   @Model(adaptables = SlingHttpServletRequest.class, adapters = MyProductTeaser.class, resourceType = MyProductTeaserImpl.RESOURCE_TYPE)
   public class MyProductTeaserImpl implements MyProductTeaser {
       ...
       private AbstractProductRetriever productRetriever;
   
       /* add this method to intialize the proudctRetriever */
       @PostConstruct
       public void initModel() {
           productRetriever = productTeaser.getProductRetriever();
   
           if (productRetriever != null) {
               productRetriever.extendProductQueryWith(p -> p.createdAt());
           }
   
       }
   ...
   ```

   Anteckningen `@PostConstruct` ser till att den här metoden anropas när Sling-modellen initieras.

   Observera att GraphQL-produktfrågan redan har utökats med metoden `extendProductQueryWith` för att hämta det ytterligare `created_at`-attributet. Attributet används senare som en del av metoden `isShowBadge()`.

1. Uppdatera GraphQL-frågan så att attributet `eco_friendly` inkluderas i den partiella frågan:

   ```java
   //MyProductTeaserImpl.java
   
   private static final String ECO_FRIENDLY_ATTRIBUTE = "eco_friendly";
   
   @PostConstruct
   public void initModel() {
       productRetriever = productTeaser.getProductRetriever();
   
       if (productRetriever != null) {
           productRetriever.extendProductQueryWith(p -> p
               .createdAt()
               .addCustomSimpleField(ECO_FRIENDLY_ATTRIBUTE)
           );
       }
   }
   ```

   Att lägga till i metoden `extendProductQueryWith` är ett kraftfullt sätt att se till att ytterligare produktattribut är tillgängliga för resten av modellen. Det minimerar även antalet frågor som körs.

   I ovanstående kod används `addCustomSimpleField` för att hämta attributet `eco_friendly`. Detta visar hur du kan söka efter anpassade attribut som ingår i Adobe Commerce-schemat.

   >[!NOTE]
   >
   >Metoden `createdAt()` har implementerats som en del av [produktgränssnittet](https://github.com/adobe/commerce-cif-magento-graphql/blob/master/src/main/java/com/adobe/cq/commerce/magento/graphql/ProductInterface.java). De flesta vanliga schemaattribut har implementerats, så använd bara `addCustomSimpleField` för anpassade attribut.

1. Lägg till en längre behållare så att den kan hjälpa dig felsöka Java™-koden:

   ```java
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   ...
   @Model(adaptables = SlingHttpServletRequest.class, adapters = MyProductTeaser.class, resourceType = MyProductTeaserImpl.RESOURCE_TYPE)
   public class MyProductTeaserImpl implements MyProductTeaser {
   
   private static final Logger LOGGER = LoggerFactory.getLogger(MyProductTeaserImpl.class);
   ```

1. Implementera sedan metoden `isEcoFriendly()`:

   ```java
   @Override
   public Boolean isEcoFriendly() {
   
       Integer ecoFriendlyValue;
       try {
           ecoFriendlyValue = productRetriever.fetchProduct().getAsInteger(ECO_FRIENDLY_ATTRIBUTE);
           if(ecoFriendlyValue != null && ecoFriendlyValue.equals(Integer.valueOf(1))) {
               LOGGER.info("*** Product is Eco Friendly**");
               return true;
           }
       } catch (SchemaViolationError e) {
           LOGGER.error("Error retrieving eco friendly attribute");
       }
       LOGGER.info("*** Product is not Eco Friendly**");
       return false;
   }
   ```

   I ovanstående metod används `productRetriever` för att hämta produkten och metoden `getAsInteger()` används för att hämta värdet för attributet `eco_friendly`. Baserat på de GraphQL-frågor du körde tidigare vet du att det förväntade värdet när attributet `eco_friendly` är inställt på **Yes** är i själva verket ett heltal på **1**.

   Nu när du har uppdaterat delningsmodellen uppdaterar du komponentkoden så att den visar indikatorn **Eco Friendly** baserat på delningsmodellen.

## Anpassa koden för Product Teaser {#customize-markup-product-teaser}

Ett vanligt tillägg för AEM-komponenter är att ändra den kod som genereras av komponenten. Detta görs genom att åsidosätta det [HTML-skript](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=sv-SE) som komponenten använder för att återge sin kod. HTML Template Language (HTL) är ett lättviktsmallspråk som används av AEM-komponenter för att dynamiskt återge kod baserat på det innehåll som skapats, vilket gör att komponenterna kan återanvändas. Product Teaser kan till exempel återanvändas om och om igen för att visa olika produkter.

I det här fallet vill du återge en banderoll ovanpå teaser för att ange att produkten är&quot;miljövänlig&quot; baserat på ett anpassat attribut. Designmönstret för [att anpassa markeringen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/customizing.html?lang=sv-SE#customizing-the-markup) för en komponent är standard för alla AEM-komponenter, inte bara för AEM CIF Core-komponenter.

>[!NOTE]
>
>Om du anpassar en komponent med CIF produkt- och kategoriväljare som detta Product Teaser eller sidkomponenten CIF måste du inkludera det nödvändiga `cif.shell.picker`-klientlibet för komponentdialogrutorna. Mer information finns i [Användning av CIF produkt- och kategoriväljare](use-cif-pickers.md).

1. I IDE navigerar och expanderar du modulen `ui.apps` och expanderar mapphierarkin till: `ui.apps/src/main/content/jcr_root/apps/venia/components/commerce/productteaser` och inspekterar filen `.content.xml`.

   ![Product Teaser ui.apps](../assets/customize-cif-components/product-teaser-ui-apps-ide.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:cq="https://www.day.com/jcr/cq/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:description="Product Teaser Component"
       jcr:primaryType="cq:Component"
       jcr:title="Product Teaser"
       sling:resourceSuperType="core/cif/components/commerce/productteaser/v1/productteaser"
       componentGroup="Venia - Commerce"/>
   ```

   Komponentdefinitionen för Product Teaser Component i det här projektet är ovan. Observera egenskapen `sling:resourceSuperType="core/cif/components/commerce/productteaser/v1/productteaser"`. Det här är ett exempel på hur du skapar en [proxykomponent](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/get-started/using.html?lang=sv-SE#create-proxy-components). I stället för att kopiera och klistra in alla HTML-skript för Product Teaser från AEM CIF Core Components kan du använda `sling:resourceSuperType` för att ärva alla funktioner.

1. Öppna filen `productteaser.html`. Det här är en kopia av filen `productteaser.html` från [CIF Product Teaser](https://github.com/adobe/aem-core-cif-components/blob/master/ui.apps/src/main/content/jcr_root/apps/core/cif/components/commerce/productteaser/v1/productteaser/productteaser.html)

   ```html
   <!--/* productteaser.html */-->
   <sly
     data-sly-use.product="com.venia.core.models.commerce.MyProductTeaser"
     data-sly-use.templates="core/wcm/components/commons/v1/templates.html"
     data-sly-use.actionsTpl="actions.html"
     data-sly-test.isConfigured="${properties.selection}"
     data-sly-test.hasProduct="${product.url}"
   ></sly>
   ```

   Observera att Sling Model för `MyProductTeaser` används och tilldelas variabeln `product`.

1. Redigera `productteaser.html` så att den anropar den `isEcoFriendly`-metod som implementerades i föregående övning:

   ```html
   ...
   <div
     data-sly-test="${isConfigured && hasProduct}"
     class="item__root"
     data-cmp-is="productteaser"
     data-virtual="${product.virtualProduct}"
   >
     <div data-sly-test="${product.showBadge}" class="item__badge">
       <span>${properties.text || 'New'}</span>
     </div>
     <!--/* Insert call to Eco Friendly here */-->
     <div data-sly-test="${product.ecoFriendly}" class="item__eco">
       <span>Eco Friendly</span>
     </div>
     ...
   </div>
   ```

   När du anropar en Sling Model-metod i HTML tas delen `get` och `is` av metoden bort och den första bokstaven sänks. `isShowBadge()` blir alltså `.showBadge` och `isEcoFriendly` blir `.ecoFriendly`. Baserat på det booleska värdet som returneras från `.isEcoFriendly()` avgör om `<span>Eco Friendly</span>` visas.

   Mer information om `data-sly-test` och andra HTML-blocksatser finns i [HTML-specifikationen](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=sv-SE).

1. Spara ändringarna och distribuera uppdateringarna till AEM med dina Maven-kunskaper från en kommandoradsterminal:

   ```shell
   $ cd aem-cif-guides-venia/
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Öppna ett nytt webbläsarfönster och gå till AEM och **OSGi-konsolen** > **Status** > **Sling Models**: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

1. Sök efter `MyProductTeaserImpl` och du bör se en rad som följande:

   ```plain
   com.venia.core.models.commerce.MyProductTeaserImpl - venia/components/commerce/productteaser
   ```

   Detta indikerar att Sling-modellen har distribuerats korrekt och mappats till rätt komponent.

1. Uppdatera till **Venias hemsida** på [http://localhost:4502/editor.html/content/venia/us/en.html](http://localhost:4502/editor.html/content/venia/us/en.html) där Product Teaser har lagts till.

   ![Eko-vänligt meddelande visas](../assets/customize-cif-components/eco-friendly-text-displayed.png)

   Om produkten har attributet `eco_friendly` inställt på **Yes** bör du se texten&quot;Eco Friendly&quot; på sidan. Försök med att byta till olika produkter för att se hur beteendet förändras.

1. Öppna sedan AEM `error.log` för att se de loggsatser som lagts till. `error.log` är på `<AEM SDK Install Location>/crx-quickstart/logs/error.log`.

   Sök i AEM loggar för att se de loggsatser som lagts till i Sling-modellen:

   ```plain
   2020-08-28 12:57:03.114 INFO [com.venia.core.models.commerce.MyProductTeaserImpl] *** Product is Eco Friendly**
   ...
   2020-08-28 13:01:00.271 INFO [com.venia.core.models.commerce.MyProductTeaserImpl] *** Product is not Eco Friendly**
   ...
   ```

   >[!CAUTION]
   >
   >Du kan också se vissa stackspår om den produkt som används i teaser inte har attributet `eco_friendly` som en del av dess attributuppsättning.

## Lägg till stilar för etiketten Eco Friendly {#add-styles}

I det här läget fungerar logiken för när märket **Eko-Friendly** ska visas, men den oformaterade texten kan använda vissa format. Lägg sedan till en ikon och format i modulen `ui.frontend` för att slutföra implementeringen.

1. Hämta filen [eco_friendly.svg](../assets/customize-cif-components/eco_friendly.svg). Det här används som märket **Eko-Friendly**.
1. Återgå till IDE och navigera till mappen `ui.frontend`.
1. Lägg till filen `eco_friendly.svg` i mappen `ui.frontend/src/main/resources/images`:

   ![Eko-vänlig SVG har lagts till](../assets/customize-cif-components/eco-friendly-svg-added.png)

1. Öppna filen `productteaser.scss` vid `ui.frontend/src/main/styles/commerce/_productteaser.scss`.
1. Lägg till följande Sass-regler i klassen `.productteaser`:

   ```scss
   .productteaser {
       ...
       .item__eco {
           width: 60px;
           height: 60px;
           left: 0px;
           overflow: hidden;
           position: absolute;
           padding: 5px;
   
       span {
           display: block;
           position: absolute;
           width: 45px;
           height: 45px;
           text-indent: -9999px;
           background: no-repeat center center url('../resources/images/eco_friendly.svg');
           }
       }
   ...
   }
   ```

   >[!NOTE]
   >
   >Gå till [Utforma CIF Core Components](./style-cif-component.md) om du vill ha mer information om arbetsflöden i gränssnittet.

1. Spara ändringarna och distribuera uppdateringarna till AEM med dina Maven-kunskaper från en kommandoradsterminal:

   ```shell
   $ cd aem-cif-guides-venia/
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Uppdatera till **Venias hemsida** på [http://localhost:4502/editor.html/content/venia/us/en.html](http://localhost:4502/editor.html/content/venia/us/en.html) där Product Teaser har lagts till.

   ![Slutlig implementering av miljöanpassade märken](../assets/customize-cif-components/final-product-teaser-eco-badge.png)

## Grattis {#congratulations}

Du har skräddarsytt din första AEM CIF-komponent! Hämta de [färdiga lösningsfilerna här](../assets/customize-cif-components/customize-cif-component-SOLUTION_FILES.zip).

## Bonus Challenge {#bonus-challenge}

Granska funktionaliteten för märket **New** som redan har implementerats i Product Teaser. Försök att lägga till en extra kryssruta så att författare kan kontrollera när märket **Eco Friendly** ska visas. Uppdatera komponentdialogrutan på `ui.apps/src/main/content/jcr_root/apps/venia/components/commerce/productteaser/_cq_dialog/.content.xml`.

![Nytt badge-implementeringsproblem](../assets/customize-cif-components/new-badge-implementation-challenge.png)

## Ytterligare resurser {#additional-resources}

- [AEM Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=sv-SE)
- [AEM CIF Core Components](https://github.com/adobe/aem-core-cif-components)
- [Anpassa AEM CIF Core-komponenter](https://github.com/adobe/aem-core-cif-components)
- [Anpassa kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/customizing.html?lang=sv-SE)
- [Komma igång med AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=sv-SE)
- [Användning av CIF produkt- och kategoriväljare](use-cif-pickers.md)
