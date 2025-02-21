---
title: Identifiera innehåll som ska översättas
description: Lär dig hur du identifierar innehåll som behöver översättas i Adobe Experience Manager.
contentOwner: Guillaume Carlino
feature: Language Copy
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 575be4c4fd05ec0a48ca7fdea67e13ce8eca0789
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 1%

---

# Identifiera innehåll som ska översättas{#identifying-content-to-translate}

Översättningsregler identifierar innehållet som ska översättas för sidor, komponenter och resurser som ingår i, eller utesluts från, översättningsprojekt. När en sida eller en resurs översätts extraherar AEM innehållet så att det kan skickas till översättningstjänsten.

Sidor och resurser representeras som noder i JCR-databasen. Innehållet som extraheras är ett eller flera egenskapsvärden för noderna. Översättningsreglerna identifierar de egenskaper som innehåller innehållet som ska extraheras.

Översättningsreglerna uttrycks i XML-format och lagras på följande platser:

* `/libs/settings/translation/rules/translation_rules.xml`
* `/apps/settings/translation/rules/translation_rules.xml`
* `/conf/global/settings/translation/rules/translation_rules.xml`

Filen gäller för alla översättningsprojekt.
Reglerna innehåller följande information:

* Sökvägen till noden som regeln gäller för. Regeln gäller även för nodens underordnade noder.
* Namnen på nodegenskaperna som innehåller innehållet som ska översättas. Egenskapen kan vara specifik för en viss resurstyp eller för alla resurstyper.

Du kan till exempel skapa en regel som översätter det innehåll som författare lägger till i alla AEM Foundation-textkomponenter på dina sidor. Regeln kan identifiera noden `/content` och egenskapen `text` för komponenten `foundation/components/text`.

Det finns en [konsol](#translation-rules-ui) som har lagts till för att konfigurera översättningsregler. Definitionerna i användargränssnittet fyller i filen åt dig.

En översikt över funktionerna för innehållsöversättning i AEM finns i [Översätta innehåll för flerspråkiga platser](/help/sites-administering/translation.md).

>[!NOTE]
>
>AEM har stöd för 1:1-mappning mellan resurstyper och referensattribut för översättning av refererat innehåll på en sida.

## Regelsyntax för sidor, komponenter och Assets {#rule-syntax-for-pages-components-and-assets}

En regel är ett `node`-element med ett eller flera underordnade `property`-element och noll eller flera underordnade `node`-element:

```xml
<node path="content path">
          <property name="property name" [translate="false"]/>
          <node resourceType="component path" >
               <property name="property name" [translate="false"]/>
          </node>
</node>
```

Var och en av dessa `node`-element har följande egenskaper:

* Attributet `path` innehåller sökvägen till rotnoden för grenen som reglerna gäller för.
* Underordnade `property`-element identifierar de nodegenskaper som ska översättas för alla resurstyper:

   * Attributet `name` innehåller egenskapsnamnet.
   * Det valfria attributet `translate` är lika med `false` om egenskapen inte är översatt. Som standard är värdet `true`. Det här attributet är användbart när du åsidosätter tidigare regler.

* Underordnade `node`-element identifierar de nodegenskaper som ska översättas för specifika resurstyper:

   * Attributet `resourceType` innehåller sökvägen som matchar komponenten som implementerar resurstypen.
   * Underordnade `property`-element identifierar nodegenskapen som ska översättas. Använd den här noden på samma sätt som de underordnade `property`-elementen för nodregler.

Följande exempelregel gör att innehållet i alla `text`-egenskaper översätts för alla sidor under noden `/content`. Regeln gäller för alla komponenter som lagrar innehåll i en `text`-egenskap, till exempel komponenten Foundation Text och den grundläggande Image-komponenten.

```xml
<node path="/content">
          <property name="text"/>
</node>
```

Följande exempel översätter innehållet i alla `text`-egenskaper och översätter även andra egenskaper i den grundläggande Image-komponenten. Om andra komponenter har egenskaper med samma namn gäller regeln inte för dem.

```xml
<node path="/content">
      <property name="text"/>
      <node resourceType="foundation/components/textimage">
         <property name="image/alt"/>
         <property name="image/jcr:description"/>
         <property name="image/jcr:title"/>
      </node>
</node>
```

## Regelsyntax för att extrahera Assets från sidor  {#rule-syntax-for-extracting-assets-from-pages}

Använd följande regelsyntax för att inkludera resurser som är inbäddade i eller refererade från komponenter:

```xml
<assetNode resourceType="path to component" assetReferenceAttribute="property that stores asset"/>
```

Varje `assetNode`-element har följande egenskaper:

* Ett `resourceType`-attribut som är lika med sökvägen som matchar komponenten.
* Ett `assetReferenceAttribute`-attribut som är lika med namnet på egenskapen som lagrar resursens binära (för inbäddade resurser) eller sökvägen till den refererade resursen.

I följande exempel extraheras bilder från Image-komponenten som grund:

```xml
<assetNode resourceType="foundation/components/image" assetReferenceAttribute="fileReference"/>
```

## Åsidosätta regler {#overriding-rules}

Filen translation_rules.xml består av ett `nodelist`-element med flera underordnade `node`-element. AEM läser nodlistan uppifrån och ned. När flera regler har samma nod som mål används den regel som är lägre i filen. Följande regler gör till exempel att allt innehåll i `text`-egenskaper översätts förutom för sidgrenen `/content/mysite/en`:

```xml
<nodelist>
     <node path="/content">
           <property name="text" />
     </node>
     <node path="/content/mysite/en">
          <property name="text" translate="false" />
     </node>
<nodelist>
```

## Filteregenskaper {#filtering-properties}

Du kan filtrera noder som har en viss egenskap genom att använda ett `filter`-element.

Följande regler gör till exempel att allt innehåll i `text`-egenskaper översätts, förutom de noder som har egenskapen `draft` inställd på `true`.

```xml
<nodelist>
    <node path="/content">
     <filter>
   <node containsProperty="draft" propertyValue="true" />
     </filter>
        <property name="text" />
    </node>
<nodelist>
```

## Gränssnitt för översättningsregler {#translation-rules-ui}

Det finns även en konsol för att konfigurera översättningsregler.

Så här kommer du åt den:

1. Navigera till **Verktyg** och sedan till **Allmänt**.

   ![chlimage_1-55](assets/chlimage_1-55.jpeg)

1. Välj **Översättningskonfiguration**.

   ![chlimage_1-56](assets/chlimage_1-56.jpeg)

Härifrån kan du **lägga till kontext**. På så sätt kan du lägga till en bana.

![chlimage_1-57](assets/chlimage_1-57.jpeg)

Sedan måste du markera kontexten och sedan klicka på **Redigera**. Då öppnas redigeraren för översättningsregler.

![chlimage_1-58](assets/chlimage_1-58.jpeg)

Det finns fyra attribut som du kan ändra via gränssnittet: `isDeep`, `inherit`, `translate` och `updateDestinationLanguage`.

**isDeep** Det här attributet gäller för nodfilter och är som standard true. Den kontrollerar om noden (eller dess överordnade noder) innehåller den egenskapen med det angivna egenskapsvärdet i filtret. Om värdet är false kontrolleras endast den aktuella noden.

Underordnade noder läggs till i ett översättningsjobb även om den överordnade noden har egenskapen `draftOnly` inställd på true för att flagga utkastinnehåll. Här visas `isDeep` som spelas upp och kontrollerar om de överordnade noderna har egenskapen `draftOnly` som true och exkluderar dessa underordnade noder.

I redigeraren kan du kontrollera/avmarkera **Är djup** på fliken **Filter**.

![chlimage_1-59](assets/chlimage_1-59.jpeg)

Här är ett exempel på den resulterande XML-filen när **Är djup** inte är markerat i gränssnittet:

```xml
 <filter>
    <node containsProperty="draftOnly" isDeep="false" propertyValue="true"/>
</filter>
```

**inherit** Detta gäller för egenskaper. Som standard ärvs alla egenskaper, men om du vill att vissa egenskaper inte ska ärvas på den underordnade, kan du markera den egenskapen som false så att den bara tillämpas på den specifika noden.

I gränssnittet kan du kontrollera/avmarkera **Inherit** på fliken **Egenskaper**.

![chlimage_1-60](assets/chlimage_1-60.jpeg)

**translate** Attributet translate används bara för att ange om en egenskap ska översättas eller inte.

I gränssnittet kan du kontrollera/avmarkera **Översätt** på fliken **Egenskaper**.

**updateDestinationLanguage** Det här attributet används för egenskaper som inte har text men språkkoder, till exempel jcr:language. Användaren översätter inte text utan språkinställningen från källan till målet. Sådana egenskaper skickas inte för översättning.

I gränssnittet kan du kontrollera/avmarkera **Översätt** på fliken **Egenskaper**, men för de specifika egenskaper som har språkkoder som värde.

För att förtydliga skillnaden mellan `updateDestinationLanguage` och `translate` finns det här ett enkelt exempel på en kontext med bara två regler:

![chlimage_1-61](assets/chlimage_1-61.jpeg)

Resultatet i xml kommer att se ut så här:

```xml
<property inherit="true" name="text" translate="true" updateDestinationLanguage="false"/>
<property inherit="true" name="jcr:language" translate="false" updateDestinationLanguage="true"/>
```

## Redigera regelfilen manuellt {#editing-the-rules-file-manually}

Filen translation_rules.xml som installeras med AEM innehåller en standarduppsättning med översättningsregler. Du kan redigera filen så att den uppfyller översättningsprojektens krav. Du kan till exempel lägga till regler så att innehållet i dina anpassade komponenter översätts.

Om du redigerar filen translation_rules.xml sparar du en säkerhetskopia i ett innehållspaket. Om du installerar AEM Service Pack eller installerar om vissa AEM-paket kan den aktuella filen translation_rules.xml ersättas med originalfilen. Om du vill återställa reglerna i den här situationen kan du installera det paket som innehåller säkerhetskopian.

>[!NOTE]
>
>När du har skapat innehållspaketet återskapar du paketet varje gång du redigerar filen.

## Exempel på översättningsregelfil {#example-translation-rules-file}

```xml
<nodelist>
    <!-- translation rules for Geometrixx Demo site (example) -->
    <node path="/content/geometrixx">
        <!-- list all node properties that should be translated -->
        <property name="jcr:title" /> <!-- translation workflows running on content saved in /content/geometrixx, will extract jcr:title values independent of the component. -->
        <property name="jcr:description" />
        <node resourceType ="foundation/components/image"> <!-- translation workflows running on content saved in /content/geometrixx, will extract alternateText values only for Image component. -->
            <property name="alternateText"/>
        </node>
        <node resourceType ="geometrixx/components/title">
            <property name="richText"/>
            <property name="jcr:title" translate="false"/> <!-- translation workflows running on content saved in /content/geometrixx, will not extract jcr:title for Title component, but instead use richText. -->
        </node>
        <node pathContains="/cq:annotations">
            <property name="text" translate="false"/> <!-- translation workflows running on content saved in /content/geometrixx, will not extract text if part of cq:annotations node. -->
        </node>
    </node>
    <!-- translation rules for Geometrixx Outdoors site (example) -->
    <node path="/content/geometrixx-outdoors">
        <node resourceType ="foundation/components/image">
            <property name="alternateText"/>
            <property name="jcr:title" />
        </node>
        <node resourceType ="geometrixx-outdoors/components/title">
            <property name="richText"/>
        </node>
    </node>
    <!-- translation rules for ASSETS (example) -->
    <node path="/content/dam">
        <!-- configure list of metadata properties here -->
        <property name="dc:title" />
        <property name="dc:description" />
    </node>
    <!-- translation rules for extracting ASSETS from SITES content, configure all components that embed or reference assets -->
    <assetNode resourceType="foundation/components/image" assetReferenceAttribute="fileReference"/>
    <assetNode resourceType="foundation/components/video" assetReferenceAttribute="asset"/>
    <assetNode resourceType="foundation/components/download" assetReferenceAttribute="fileReference"/>
    <assetNode resourceType="foundation/components/mobileimage" assetReferenceAttribute="fileReference"/>
    <assetNode resourceType="wcm/foundation/components/image" assetReferenceAttribute="fileReference"/>
</nodelist>
```
