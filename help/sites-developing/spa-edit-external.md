---
title: Redigera en extern SPA i Adobe Experience Manager
description: I det här dokumentet beskrivs de rekommenderade stegen för att överföra en fristående SPA till en Adobe Experience Manager-instans, lägga till redigerbara innehållsavsnitt och aktivera redigering.
solution: Experience Manager, Experience Manager Sites
feature: Developing,SPA Editor
role: Developer
exl-id: cb5495f9-bc54-4515-ae15-55a5397500aa
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 0%

---

# Redigera en extern SPA i Adobe Experience Manager {#editing-external-spa-within-aem}

När du bestämmer vilken nivå av integration du vill ha mellan din externa SPA och Adobe Experience Manager (AEM) behöver du ofta kunna redigera och visa SPA inom AEM.

## Ökning {#overview}

I det här dokumentet beskrivs de rekommenderade stegen för att överföra en fristående SPA till en AEM-instans, lägga till redigerbara innehållsavsnitt och aktivera redigering.

{{ue-over-spa}}

## Förutsättningar {#prerequisites}

Förutsättningarna är enkla.

* Kontrollera att en instans av AEM körs lokalt.
* Skapa ett basprojekt för AEM SPA med [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?#available-properties).
   * Detta utgör grunden för AEM-projektet, som kommer att uppdateras med det externa SPA-projektet.
   * Exemplen i det här dokumentet använder startpunkten för [WKND SPA-projektet](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/spa-editor/spa-editor-framework-feature-video-use.html#spa-editor).
* Ha den fungerande, externa React SPA som ni vill integrera till hands.

## Överför SPA till AEM Project {#upload-spa-to-aem-project}

Först måste du överföra den externa SPA-filen till ditt AEM-projekt.

1. Ersätt `src` i projektmappen `/ui.frontend` med `src`-mappen för ditt React-program.
1. Inkludera eventuella ytterligare beroenden i appens `package.json` i filen `/ui.frontend/package.json`.
   * Kontrollera att SPA SDK-beroendena är av [rekommenderade versioner](spa-getting-started-react.md#dependencies).
1. Inkludera eventuella anpassningar i mappen `/public`.
1. Inkludera eventuella infogade skript eller format som lagts till i filen `/public/index.html`.

## Konfigurera fjärr-SPA {#configure-remote-spa}

Nu när den externa produktresumén ingår i ditt AEM-projekt måste den konfigureras i AEM.

### Inkludera Adobe SPA SDK Packages {#include-spa-sdk-packages}

För att dra nytta av AEM SPA-funktioner finns det beroenden av följande tre paket.

* [`@adobe/aem-react-editable-components`](https://github.com/adobe/aem-react-editable-components)
* [`@adobe/aem-spa-component-mapping`](https://www.npmjs.com/package/@adobe/aem-spa-component-mapping)
* [`@adobe/aem-spa-page-model-manager`](https://www.npmjs.com/package/@adobe/aem-spa-model-manager)

`@adobe/aem-spa-page-model-manager` innehåller API:t för att initiera en modellhanterare och hämta modellen från AEM-instansen. Den här modellen kan sedan användas för att återge AEM-komponenter med API:er från `@adobe/aem-react-editable-components` och `@adobe/aem-spa-component-mapping`.

#### Installation {#installation}

Kör följande npm-kommando för att installera de nödvändiga paketen.

```shell
npm install --save @adobe/aem-spa-component-mapping @adobe/aem-spa-page-model-manager @adobe/aem-react-editable-components
```

### ModelManager-initiering {#model-manager-initialization}

Innan appen återges måste [`ModelManager`](spa-blueprint.md#pagemodelmanager) initieras för att hantera skapandet av AEM `ModelStore`.

Detta måste göras i filen `src/index.js` i programmet eller där programmets rot återges.

Använd därför `initializationAsync`-API:t från `ModelManager`.

Följande skärmbild visar hur du aktiverar initiering av `ModelManager` i ett enkelt React-program. Den enda begränsningen är att `initializationAsync` måste anropas före `ReactDOM.render()`.

![Initiera ModelManager](assets/external-spa-initialize-modelmanager.png)

I det här exemplet initieras `ModelManager` och en tom `ModelStore` skapas.

`initializationAsync` kan acceptera ett `options`-objekt som en parameter:

* `path` - Vid initiering hämtas modellen vid den definierade sökvägen och lagras i `ModelStore`. Detta kan användas för att hämta `rootModel` vid initiering om det behövs.
* `modelClient` - Tillåter att du anger en anpassad klient som ansvarar för att hämta modellen.
* `model` - Ett `model`-objekt som skickas som en parameter som vanligtvis fylls i när SSR används.

### AEM Authorable Leaf Components {#authorable-leaf-components}

1. Skapa/identifiera en AEM-komponent som en redigerbar React-komponent ska skapas för. I det här exemplet använder WKND-projektet textkomponenten.

   ![WKND-textkomponent](assets/external-spa-text-component.png)

1. Skapa en enkel React-textkomponent i SPA. I det här exemplet har en ny fil, `Text.js`, skapats med följande innehåll.

   ![Text.js](assets/external-spa-textjs.png)

1. Skapa ett konfigurationsobjekt för att ange de attribut som krävs för att aktivera redigering i AEM.

   ![Skapa config-objekt](assets/external-spa-config-object.png)

   * `resourceType` är obligatoriskt för att mappa React-komponenten till AEM-komponenten och aktivera redigering när den öppnas i AEM Editor.

1. Använd wrapper-funktionen `withMappable`.

   ![Använd medMappable](assets/external-spa-withmappable.png)

   Den här wrapper-funktionen mappar React-komponenten till AEM `resourceType` som anges i konfigurationen och aktiverar redigeringsfunktioner när den öppnas i AEM Editor. För fristående komponenter hämtas även modellinnehållet för den specifika noden.

   >[!NOTE]
   >
   >I det här exemplet finns det separata versioner av komponenten: AEM inkapslade och oinkapslade React-komponenter. Den omslutna versionen måste användas när komponenten används explicit. När komponenten är en del av en sida kan du fortsätta använda standardkomponenten på samma sätt som i SPA-redigeraren.

1. Återge innehåll i komponenten.

   JCR-egenskaperna för textkomponenten visas på följande sätt i AEM.

   ![Egenskaper för textkomponent](assets/external-spa-text-properties.png)

   Dessa värden skickas som egenskaper till den nyligen skapade `AEMText`-reaktionskomponenten och kan användas för att återge innehållet.

   ```javascript
   import React from 'react';
   import { withMappable } from '@adobe/aem-react-editable-components';
   
   export const TextEditConfig = {
       // Empty component placeholder label
       emptyLabel:'Text', 
       isEmpty:function(props) {
          return !props || !props.text || props.text.trim().length < 1;
       },
       // resourcetype of the AEM counterpart component
       resourceType:'wknd-spa-react/components/text'
   };
   
   const Text = ({ text }) => (<div>{text}</div>);
   
   export default Text;
   
   export const AEMText = withMappable(Text, TextEditConfig);
   ```

   Så här visas komponenten när AEM-konfigurationerna är klara.

   ```javascript
   const Text = ({ cqPath, richText, text }) => {
      const richTextContent = () => (
         <div className="aem_text" id={cqPath.substr(cqPath.lastIndexOf('/') + 1)} data-rte-editelement dangerouslySetInnerHTML={{__html: text}}/>
      );
      return richText ? richTextContent() : (<div className="aem_text">{text}</div>);
   };
   ```

   >[!NOTE]
   >
   >I det här exemplet har ytterligare anpassningar gjorts för den återgivna komponenten för att matcha den befintliga textkomponenten. Detta gäller dock inte redigering i AEM.

#### Lägg till redigerbara komponenter på sidan {#add-authorable-component-to-page}

När de redigerbara React-komponenterna har skapats kan de användas i hela programmet.

Låt oss ta en exempelsida där text från WKND SPA-projektet måste läggas till. I det här exemplet vill du visa texten&quot;Hello World!&quot; på `/content/wknd-spa-react/us/en/home.html`.

1. Ange sökvägen till noden som ska visas.

   * `pagePath`: Den sida som innehåller noden, i exemplet `/content/wknd-spa-react/us/en/home`
   * `itemPath`: Sökväg till noden på sidan, i exemplet `root/responsivegrid/text`
      * Detta består av namnen på de objekt som finns på sidan.

   ![Sökväg till noden](assets/external-spa-path.png)

1. Lägg till en komponent på önskad plats på sidan.

   ![Lägg till komponent på sidan](assets/external-spa-add-component.png)

   Komponenten `AEMText` kan läggas till på den önskade positionen på sidan med `pagePath` - och `itemPath`-värden angivna som egenskaper. `pagePath` är en obligatorisk egenskap.

#### Verifiera redigering av textinnehåll i AEM {#verify-text-edit}

Testa nu komponenten på den AEM-instans som körs.

1. Kör följande Maven-kommando från katalogen `aem-guides-wknd-spa` för att skapa och distribuera projektet till AEM.

```shell
mvn clean install -PautoInstallSinglePackage
```

1. Navigera till `http://<host>:<port>/editor.html/content/wknd-spa-react/us/en/home.html` på din AEM-instans.

![Redigera SPA i AEM](assets/external-spa-edit-aem.png)

Komponenten `AEMText` är nu redigerbar på AEM.

### AEM Authorable Pages {#aem-authorable-pages}

1. Identifiera en sida som ska läggas till för redigering i SPA. I det här exemplet används `/content/wknd-spa-react/us/en/home.html`.
1. Skapa en fil (till exempel `Page.js`) för den redigerbara sidkomponenten. Här kan sidkomponenten återanvändas som anges i `@adobe/cq-react-editable-components`.
1. Upprepa steg fyra i avsnittet [AEM skrivbara bladkomponenter](#authorable-leaf-components). Använd adapterfunktionen `withMappable` för komponenten.
1. Som tidigare har gjorts använder du `MapTo` på AEM-resurstyperna för alla underordnade komponenter på sidan.

   ```javascript
   import { Page, MapTo, withMappable } from '@adobe/aem-react-editable-components';
   import Text, { TextEditConfig } from './Text';
   
   export default withMappable(Page);
   
   MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   >[!NOTE]
   >
   >I det här exemplet används den oomslutna React-textkomponenten i stället för den omslutna `AEMText` som skapades tidigare. Detta beror på att när komponenten är en del av en sida/behållare och inte fristående, kommer behållaren att hantera rekursiv mappning av komponenten och aktivera redigeringsfunktioner och den extra adaptern behövs inte för varje underordnad.

1. Om du vill lägga till en redigerbar sida i SPA följer du samma steg i avsnittet [Lägg till redigerbara komponenter på sidan](#add-authorable-component-to-page). Här kan vi emellertid hoppa över egenskapen `itemPath`.

#### Verifiera sidinnehåll i AEM {#verify-page-content}

Verifiera att sidan kan redigeras genom att följa samma steg i avsnittet [Verifiera redigering av textinnehåll i AEM](#verify-text-edit).

![Redigera en sida i AEM](assets/external-spa-edit-page.png)

Sidan kan nu redigeras i AEM med en layoutbehållare och en underordnad textkomponent.

### Virtuella lövkomponenter {#virtual-leaf-components}

I de föregående exemplen har vi lagt till komponenter i SPA med befintligt AEM-innehåll. Det finns dock fall där innehåll ännu inte har skapats i AEM, men måste läggas till senare av innehållsförfattaren. För att tillgodose detta kan front-end-utvecklaren lägga till komponenter på lämpliga platser i SPA. Komponenterna visar platshållare när de öppnas i redigeraren i AEM. När innehållet har lagts till i platshållarna av innehållsförfattaren skapas noderna i JCR-strukturen och innehållet bevaras. Den skapade komponenten tillåter samma uppsättning åtgärder som de fristående bladkomponenterna.

I det här exemplet återanvänder vi komponenten `AEMText` som skapades tidigare. Vi vill att ny text ska läggas till under den befintliga textkomponenten på WKND-startsidan. Tillägget av komponenter är detsamma som för normala bladkomponenter. `itemPath` kan dock uppdateras till den sökväg där den nya komponenten måste läggas till.

Eftersom den nya komponenten måste läggas till under den befintliga texten på `root/responsivegrid/text` blir den nya sökvägen `root/responsivegrid/{itemName}`.

```html
<AEMText
 pagePath='/content/wknd-spa-react/us/en/home'
 itemPath='root/responsivegrid/text_20' />
```

Komponenten `TestPage` ser ut så här när den virtuella komponenten har lagts till.

![Testar den virtuella komponenten](assets/external-spa-virtual-component.png)

>[!NOTE]
>
>Kontrollera att `resourceType` har angetts för komponenten `AEMText` i konfigurationen för att aktivera den här funktionen.

Du kan nu distribuera ändringarna till AEM enligt stegen i avsnittet [Verifiera redigering av textinnehåll på AEM](#verify-text-edit). En platshållare visas för den `text_20`-nod som för närvarande inte finns.

![Noden text_20 i aem](assets/external-spa-text20-aem.png)

När innehållsförfattaren uppdaterar den här komponenten skapas en ny `text_20`-nod `root/responsivegrid/text_20` i `/content/wknd-spa-react/us/en/home`.

![Noden text20](assets/external-spa-text20-node.png)

#### Krav och begränsningar {#limitations}

Det finns flera krav för att lägga till virtuella bladkomponenter och vissa begränsningar.

* Egenskapen `pagePath` är obligatorisk för att skapa en virtuell komponent.
* Sidnoden som anges i sökvägen i `pagePath` måste finnas i AEM-projektet.
* Namnet på noden som ska skapas måste anges i `itemPath`.
* Komponenten kan skapas på alla nivåer.
   * Om vi anger en `itemPath='text_20'` i det föregående exemplet skapas den nya noden direkt under sidan som är `/content/wknd-spa-react/us/en/home/jcr:content/text_20`
* Sökvägen till noden där en ny nod skapas måste vara giltig när den anges via `itemPath`.
   * I det här exemplet måste `root/responsivegrid` finnas så att den nya noden `text_20` kan skapas där.
* Det går bara att skapa lövkomponenter. Virtuell behållare och sida stöds i framtida versioner.

### Virtuella behållare {#virtual-containers}

Möjligheten att lägga till behållare stöds, även om motsvarande behållare ännu inte har skapats i AEM. Konceptet och tillvägagångssätt liknar [virtuella lövkomponenter.](#virtual-leaf-components)

Utvecklaren kan lägga till behållarkomponenterna på lämpliga platser i SPA och dessa komponenter visar platshållare när de öppnas i redigeraren i AEM. Författaren kan sedan lägga till komponenter och deras innehåll i behållaren, vilket skapar de nödvändiga noderna i JCR-strukturen.

Om det till exempel redan finns en behållare på `/root/responsivegrid` och utvecklaren vill lägga till en ny underordnad behållare:

![Behållarplats](assets/container-location.png)

`newContainer` finns inte i AEM ännu.

När du redigerar sidan som innehåller den här komponenten i AEM visas en tom platshållare för en behållare där författaren kan lägga till innehåll.

![Platshållare för behållare](assets/container-placeholder.png)

![Behållarplats i JCR](assets/container-jcr-structure.png)

När författaren lägger till en underordnad komponent i behållaren skapas den nya behållarnoden med motsvarande namn i JCR-strukturen.

![Behållare med innehåll](assets/container-with-content.png)

![Behållare med innehåll i JCR](assets/container-with-content-jcr.png)

Fler komponenter och innehåll kan läggas till i behållaren nu när författaren kräver det och ändringarna bevaras.

#### Krav och begränsningar {#container-limitations}

Det finns flera krav för att lägga till virtuella behållare och vissa begränsningar.

* Principen för att bestämma vilka komponenter som kan läggas till ärvs från den överordnade behållaren.
* Den omedelbara överordnade behållaren för den behållare som ska skapas måste redan finnas i AEM.
   * Om behållaren `root/responsivegrid` redan finns i AEM-behållaren kan en ny behållare skapas genom att sökvägen `root/responsivegrid/newContainer` anges.
   * `root/responsivegrid/newContainer/secondNewContainer` är dock inte möjligt.
* Det går endast att skapa en ny komponentnivå åt gången.

## Ytterligare anpassningar {#additional-customizations}

Om du följde de föregående exemplen kan din externa SPA nu redigeras i AEM. Det finns dock andra aspekter av din externa SPA som du kan anpassa ytterligare.

### Rotnods-ID {#root-node-id}

Som standard antar vi att React-programmet återges i en `div` av element-ID:t `spa-root`. Om det behövs kan det anpassas.

Anta till exempel att vi har en SPA där programmet återges inuti `div` för element-ID `root`. Detta måste återspeglas i tre filer.

1. I `index.js` av React-programmet (eller där `ReactDOM.render()` anropas)

   ![ReactDOM.render() i filen index.js](assets/external-spa-root-index.png)

1. I `index.html` av React-programmet

   ![Programmets index.html](assets/external-spa-index.png)

1. I sidkomponentbrödtexten för AEM-appen i två steg:

   1. Skapa en `body.html` för sidkomponenten.

   ![Skapa filen body.html](assets/external-spa-update-body.gif)

   1. Lägg till det nya rotelementet i den nya `body.html`-filen.

   ![Lägg till rotelementet i body.html](assets/external-spa-add-root.png)

### Redigera en React SPA med Routning {#editing-react-spa-with-routing}

Om det externa SPA-programmet för React har flera sidor kan [det använda routning för att avgöra vilken sida/komponent som ska återges](spa-routing.md). Det grundläggande användningsexemplet är att matcha den aktiva URL:en mot sökvägen som anges för ett flöde. Om du vill aktivera redigering för sådana routningsaktiverade program måste sökvägen som ska matchas mot omformas för att rymma AEM-specifik information.

I följande exempel har vi ett enkelt React-program med två sidor. Den sida som ska återges bestäms genom att matcha sökvägen som har angetts till routern mot den aktiva URL:en. Om vi till exempel är på `mydomain.com/test` återges `TestPage`.

![Routning i en extern SPA](assets/external-spa-routing.png)

Följande steg krävs för att aktivera redigering i AEM för detta exempel på SPA.

1. Identifiera nivån som skulle fungera som rot för AEM.

   * För vårt exempel ser vi `wknd-spa-react/us/en` som roten till SPA. Det innebär att allt som ligger före den sökvägen bara är AEM sidor/innehåll.

1. Skapa en sida på önskad nivå.

   * I det här exemplet är sidan som ska redigeras `mydomain.com/test`. `test` finns i programmets rotsökväg. Detta måste också bevaras när du skapar sidan i AEM. Därför kan du skapa en sida på den rotnivå som definierades i föregående steg.
   * Den nya sidan måste ha samma namn som sidan som ska redigeras. I det här exemplet för `mydomain.com/test` måste den nya sidan som skapas vara `/path/to/aem/root/test`.

1. Lägg till hjälpredor i SPA-routning.

   * Den nyskapade sidan återger inte det förväntade innehållet i AEM ännu. Detta beror på att routern förväntar sig sökvägen `/test` medan den aktiva AEM-sökvägen är `/wknd-spa-react/us/en/test`. För att få plats med den AEM-specifika delen av URL:en måste vi lägga till några hjälpredor på SPA-sidan.

   ![Hjälpprogram för routning](assets/external-spa-router-helper.png)

   * `toAEMPath`-hjälpen som tillhandahålls av `@adobe/cq-spa-page-model-manager` kan användas för detta. Den omformar sökvägen för routning så att den omfattar delar som är specifika för AEM när programmet är öppet i en AEM-instans. Den godkänner tre parametrar:
      * Sökvägen som krävs för routning
      * Den ursprungliga URL:en för den AEM-instans där SPA redigeras
      * Projektroten på AEM enligt det första steget

   * Dessa värden kan anges som miljövariabler för större flexibilitet.

1. Verifiera redigering av sidan i AEM.

   * Distribuera projektet till AEM och gå till den nyligen skapade `test`-sidan. Sidinnehållet återges nu och AEM-komponenterna kan redigeras.

## Rambegränsningar {#framework-limitations}

RemotePage-komponenten förväntar sig att implementeringen tillhandahåller ett tillgångsmanifest som [webpack-manifest-plugin i GitHub](https://github.com/shellscape/webpack-manifest-plugin). RemotePage-komponenten har bara testats för att fungera med React Framework (och Next.js via komponenten remote-page-next) och stöder därför inte fjärrinläsning av program från andra ramverk, som Angular.

## Ytterligare resurser {#additional-resources}

Följande referensmaterial kan vara användbart för att förstå SPA i samband med AEM.

* [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [WKND SPA-projektet](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/spa-editor/spa-editor-framework-feature-video-use.html)
* [Komma igång med SPA i AEM med React](spa-getting-started-react.md)
* [SPA-referensmaterial (API-referenser)](spa-reference-materials.md)
* [SPA Blueprint och PageModelManager](spa-blueprint.md#pagemodelmanager)
* [SPA-modellroutning](spa-routing.md)
