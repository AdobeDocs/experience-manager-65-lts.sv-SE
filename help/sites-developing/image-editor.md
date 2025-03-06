---
title: Bildredigeraren
description: Bildredigeraren är en viktig del av AEM och kan användas av komponenter för att underlätta redigering av bilder av innehållsförfattare.
contentOwner: bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: components
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: caaa4902-5f38-45c7-a788-521e05653538
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# Bildredigeraren{#image-editor}

Bildredigeraren är en viktig del av AEM och kan användas av komponenter för att underlätta redigering av bilder av innehållsförfattare.

>[!CAUTION]
>
>Om du vill använda funktionerna i bildredigeraren som beskrivs i den här artikeln måste [funktionspaket 24267](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq640/featurepack/cq-6.4.0-featurepack-24267) vara installerat.

## Relativa enheter för bildschema {#relative-units-for-image-map}

Bildredigeraren behåller bildschemaområden som både absoluta och relativa enheter. Relativa enheter är användbara när de anges som dataattribut för att dynamiskt ändra storlek på ett bildschema (i förhållande till bildstorleken) på klientsidan i en responsiv bildkomponent.

### imageMap, egenskap {#imagemap-property}

Koordinaterna för bildschemat bevaras som en `imageMap`-egenskap i JCR-läsaren. Den har följande format.

Egenskapen lagrar kartområden enligt följande:

`[area1][area2][...]`

Områdesformat:

`[SHAPE(COORDINATES)"HREF"|"TARGET"|"ALT"|(RELATIVE_COORDINATES)]`

Exempel:

`[rect(0,0,10,10)"https://www.adobe.com"|"_self"|"alt"|(0,0,0.8,0.8)]`
`[circle(10,10,10)"https://www.adobe.com"|"_self"|"alt"|(0.8,0.8,0.8)]`

## Stöd för SVG Images {#support-for-svg-images}

Skalbar vektorgrafik (SVG) stöds av bildredigeraren.

* Både dra-och-släpp av en SVG-resurs från DAM och överföring av en SVG-filöverföring från ett lokalt filsystem stöds.

## Aktivera plugin-program efter MIME-typ {#enabling-plugins-by-mime-type}

I vissa situationer måste redigeringsåtgärderna begränsas för vissa MIME-typer på grund av att det inte finns stöd för bearbetning på serversidan. Det är till exempel inte tillåtet att redigera SVG-bilder.

Plugin-program i bildredigeraren kan aktiveras selektivt av MIME-typ genom att en `supportedMimeTypes`-egenskap anges på den enskilda plugin-programmets konfigurationsnod.

### Exempel {#example}

Låt oss till exempel säga att möjligheten att beskära endast ska vara tillåten för GIF-, JPEG-, PNG-, WEBP- och TIFF-bilder.

Egenskapen `supportedMimeTypes` måste sedan anges som en sträng med de tillåtna MIME-typerna på konfigurationsnoden för plugin-programmet på noden `cq:editConfig` i avbildningskomponenten.

`/apps/core/wcm/components/image/v2/image/cq:editConfig`

```xml
 jcr:primaryType="cq:EditConfig">
     <cq:dropTargets jcr:primaryType="nt:unstructured">
         <image ...>
            ...
         </image>
     </cq:dropTargets>
     <cq:inplaceEditing
         jcr:primaryType="cq:InplaceEditingConfig"
         active="{Boolean}true"
         editorType="image">
         <config jcr:primaryType="nt:unstructured">
             <plugins jcr:primaryType="nt:unstructured">
                 <crop
                     jcr:primaryType="nt:unstructured"
                     supportedMimeTypes="[image/gif,image/jpeg,image/png,image/webp,image/tiff]"
                     features="*">
                     <aspectRatios jcr:primaryType="nt:unstructured">
                        ...
                     </aspectRatios>
                 </crop>
                 ...
             </plugins>
             <ui jcr:primaryType="nt:unstructured">
                 ...
             </ui>
         </config>
     </cq:inplaceEditing>
 </jcr:root>
```
