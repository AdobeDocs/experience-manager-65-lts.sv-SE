---
title: Konfigurera RTE för flera redigerare på plats.
description: Skapa flera redigerare på plats i Adobe Experience Manager genom att konfigurera RTF-redigeraren.
contentOwner: AG
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 68e6ddea-1a50-4238-a13d-883a1b0b798d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Konfigurera flera redigerare på plats {#configure-multiple-in-place-editors}

Du kan konfigurera RTF-redigeraren i Adobe Experience Manager så att den har flera redigerare på plats. När du har konfigurerat det kan du välja rätt innehåll och öppna rätt redigerare.

![En specifik redigerare på plats](assets/rte-inplace-editor.png)

## Konfigurera flera redigerare {#configure-multiple-editors}

Om du vill aktivera flera redigerare på plats har strukturen för en `cq:InplaceEditingConfig`-nodtyp förbättrats med definitionen för nodtypen `cq:ChildEditorConfig`.

Till exempel:

```js
   /**
       * Configures in-place editing of a component.
       *
       * @prop active true to activate in-place editing for the component.
       * @prop editorType ID of in-place editor to use.
       * @prop cq:childEditors collection of {@link cq:ChildEditorConfig} nodes.
       * @prop configPath path to editor's config (optional).
       * @node config editor's config (used if no configPath is specified; optional).
     */
    [cq:InplaceEditingConfig] > nt:unstructured
      - active (boolean)
      - editorType (string)
      + cq:childEditors (nt:base) = nt:unstructured
      - configPath (string)
      + config (nt:unstructured) = nt:unstructured

    /**
      * Configures one child editor for a sub-component. The name of the this node is
      * used as DD ID.
      *
      * @prop type type of the inline editor. For example, ["image"].
      * @prop title Title of the inline editor.
      * @prop icon Icon to represent the inline editor.
    */
    [cq:ChildEditorConfig] > nt:unstructured
      orderable
      - type (string)
      - title (string)
```

Så här konfigurerar du flera redigerare:

1. Definiera följande egenskaper på noden `cq:inplaceEditing` (av typen `cq:InplaceEditingConfig`):

   * Namn:`editorType`
   * Typ: `String`
   * Värde: `hybrid`

1. Skapa en nod under den här noden:

   * Namn: `cq:ChildEditors`
   * Typ: `nt:unstructured`

1. Under noden `cq:childEditors` skapar du en nod för varje lokal redigerare:

   * Namn: Namnet på varje nod är namnet på den egenskap som den representerar, vilket är fallet med släppmål. Till exempel `image` och `text`.
   * Typ: `cq:ChildEditorConfig`

   >[!NOTE]
   >
   >Det finns en korrelation mellan de definierade släppmålen och de underordnade redigerarna. Namnet på noden `cq:ChildEditorConfig` betraktas som mål-ID för släppning, som används som parameter till den markerade underordnade redigeraren. Om det redigerbara delområdet inte har något släppmål, till exempel i en textkomponent, betraktas namnet på den underordnade redigeraren fortfarande som ett ID som identifierar motsvarande redigerbara område.

1. Definiera egenskaperna för var och en av dessa noder (`cq:ChildEditorConfig`):

   * Namn: `type`.
   * Värde: Namnet på den registrerade redigeraren på plats, till exempel `image` och `text`.

   * Namn: `title`.
   * Värde: Den titel som visas i komponentens urvalslista med tillgängliga redigerare. Till exempel `Image` och `Text`.

### Ytterligare konfiguration för RTF-redigerare {#additional-configuration-for-rich-text-editors}

Konfigurationen för flera textredigerare är något annorlunda eftersom du kan konfigurera varje enskild RTE-instans separat. Mer information finns i [Konfigurera RTF-redigeraren](/help/sites-administering/rich-text-editor.md). Om du vill ha flera textredigerare skapar du en konfiguration för varje fast textredigerare. Adobe rekommenderar att du skapar den nya konfigurationsnoden under `cq:InplaceEditingConfig` eftersom varje enskild RTE kan ha olika konfigurationer. Under den nya noden skapar du varje enskild RTE-konfiguration.

```xml
    texttext
        cq:dialog
        cq:editConfig
            cq:inplaceEditing
                cq:childEditors
                    someconfig
                        text1
                            rtePlugins
                        text2
                            rtePlugins
```

>[!NOTE]
>
>För RTE stöds egenskapen `configPath` om det bara finns en instans av textredigeraren (redigerbart delområde) i komponenten. Den här användningen av `configPath` har stöd för bakåtkompatibilitet med äldre användargränssnittsdialogrutor för komponenten.

>[!CAUTION]
>
>Ge inte RTE-konfigurationsnoden namnet `config`. Annars är RTE-konfigurationerna bara tillgängliga för administratörer och inte för användarna i gruppen `content-author`.

## Kodexempel {#code-samples}

Du hittar koden för den här sidan i projektet [aem-authoring-hybrideditors på GitHub](https://github.com/Adobe-Marketing-Cloud/aem-authoring-hybrideditors). Du kan hämta hela projektet som [ett ZIP-arkiv](https://github.com/Adobe-Marketing-Cloud/aem-authoring-hybrideditors/archive/master.zip).

## Lägga till en lokal redigerare {#add-an-in-place-editor}

Allmän information om hur du lägger till en lokal redigerare finns i dokumentet [Anpassa sidredigering](/help/sites-developing/customizing-page-authoring-touch.md#add-new-in-place-editor).

>[!MORELIKETHIS]
>
>* [Konfigurera RTF-redigerare i Experience Manager](/help/sites-administering/rich-text-editor.md).
