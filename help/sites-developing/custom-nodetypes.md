---
title: Anpassade nodtyper
description: Adobe Experience Manager (AEM) är baserat på Sling och använder en JCR-databas med de nodtyper som erbjuds av båda, men AEM erbjuder även en rad anpassade nodtyper
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 255f52f3-aff4-432c-a541-3ce03e626742
source-git-commit: 9bc1cad84bb14b7513ede1fff2c1a37768dac442
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 0%

---

# Anpassade nodtyper{#custom-node-types}

Eftersom Adobe Experience Manager (AEM) är baserat på Sling och använder en JCR-databas är de nodtyper som erbjuds av båda tillgängliga för användning med följande:

* [JCR-nodtyper](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/2.0/3_Repository_Model.html#3.1.7-Node-Types)
* [Dela nodtyper](https://cwiki.apache.org/confluence/display/SLING/Sling+Node+Types)

Förutom dessa nodtyper finns det ett antal anpassade nodtyper i AEM.

## Granskning {#audit}

### `cq:AuditEvent` {#cq-auditevent}

**Beskrivning**

Definierar nodtypen för en granskningshändelsenod.

* `@prop cq:time`
* `@prop cq:userid`
* `@prop cq:path`
* `@prop cq:type`
* `@prop cq:category`
* `@prop cq:properties`

**Definition**

* `[cq:AuditEvent]`
   * `- * (undefined)`
   * `- * (undefined) multiple`
   * `+ * (nt:base) = nt:base multiple version`
* `- cq:time (date)`
* `- cq:userid (string)`
* `- cq:path (string)`
* `- cq:type (string)`
* `- cq:category (string)`
* `- cq:properties (binary)`

## Kommentar {#comment}

### `cq:Comment` {#cq-comment}

**Beskrivning**

Definierar nodtypen för en kommentarnod.

* `@prop userIdentifier`

**Definition**

* `[cq:Comment] > mix:title, mix:created, mix:language, nt:unstructured, cq:Taggable`
* `- email (string)`
* `- ip (string)`
* `- referer (string)`
* `- url (string)`
* `- userAgent (string)`
* `- userIdentifier (string)`
* `- authorizableId (string)`

### `cq:CommentAttachment` {#cq-commentattachment}

**Beskrivning**

Definierar nodtypen för en `commentattachment`-nod

**Definition**

* `[cq:CommentAttachment] > nt:file`
   * `- * (undefined)`
   * `- * (undefined) multiple`

### `cq:CommentContent` {#cq-commentcontent}

**Beskrivning**

Definierar nodtypen för en kommentarinnehållsnod

**Definition**

* `[cq:Comment] > mix:title, mix:created, mix:language, nt:unstructured, cq:Taggable`
* `- email (string)`
* `- ip (string)`
* `- referer (string)`
* `- url (string)`
* `- userAgent (string)`
* `- userIdentifier (string)`
* `- authorizableId (string)`

### `cq:GeoLocation` {#cq-geolocation}

**Beskrivning**

En blandning som definierar en geografisk plats i decimalgrader (DD)

* `@prop latitude` - latitud kodad som dubbel med decimalgrader
* `@prop longitude` - longitud kodad som dubbel med decimalgrader

**Definition**

* `[cq:GeoLocation] mixin`
* `- latitude (double)`
* `- longitude (double)`

### `cq:Trackback` {#cq-trackback}

**Beskrivning**

Definierar nodtypen för en bakåtspårningsnod.

**Definition**

* `[cq:Trackback] > mix:title, mix:created, mix:language, nt:unstructured`

## Core {#core}

### `cq:Page` {#cq-page}

**Beskrivning**

Definierar CQ-standardsidan.

* `@node jcr:content` - Sidans primära innehåll.

**Definition**

* `[cq:Page] > nt:hierarchyNode orderable`
   * `+ jcr:content (nt:base) = nt:unstructured copy primary`
   * `+ * (nt:base) = nt:base version`

### `cq:PseudoPage` {#cq-pseudopage}

**Beskrivning**

Den här blandningstypen markerar noder som pseudosidor. Med andra ord innebär det att de kan anpassas för redigeringsstöd för Page och WCM.

**Definition**

* `[cq:PseudoPage] mixin`

### `cq:PageContent` {#cq-pagecontent}

**Beskrivning**

Definierar standardnoden för sidinnehåll, med de minimala egenskaper som används av WCM.

* `@prop jcr:title` - Rubrik för sidan.
* `@prop jcr:description` - Beskrivning av den här sidan.
* `@prop cq:template` - Sökväg till mallen som användes för att skapa sidan.
* `@prop cq:allowedTemplates` - Lista med reguljära uttryck som används för att bestämma sökvägarna till den tillåtna mallen.
* `@prop pageTitle` - Rubrik visas i taggen `<title>`.
* `@prop navTitle` - Titel som används i navigering.
* `@prop hideInNav` - Anger om sidan är dold i navigeringen.
* `@prop onTime` - Tid när sidan blir giltig.
* `@prop offTime` - Tid när den här sidan blir ogiltig.
* `@prop cq:lastModified` - Datum då sidan (eller dess stycken) senast ändrades.
* `@prop cq:lastModifiedBy` - Senaste användare som ska ändra sidan (eller dess stycken).
* `@prop jcr:language` - Språket för sidinnehåll.

>[!NOTE]
>
>Det är inte obligatoriskt för sidinnehåll att använda den här typen.

**Definition**

* `[cq:PageContent] > nt:unstructured, mix:title, mix:created, cq:OwnerTaggable, sling:VanityPath, cq:ReplicationStatus, sling:Resource orderable`
   * `- cq:template (string)`
   * `- cq:allowedTemplates (string) multiple`
   * `- pageTitle (string)`
   * `- navTitle (string)`
   * `- hideInNav (boolean)`
   * `- onTime (date)`
   * `- offTime (date)`
   * `- cq:lastModified (date)`
   * `- cq:lastModifiedBy (string)`
   * `- cq:designPath (string)`
   * `- jcr:language (string)`

### `cq:Template` {#cq-template}

**Beskrivning**

Definierar en CQ-mall.

* `@node jcr:content` - Standardinnehåll för nya sidor.
* `@node icon.png` - En fil som innehåller en karakteristisk ikon.
* `@node thumbnail.png` - En fil som innehåller en karakteristisk miniatyrbild.
* `@node workflows` - Tilldela arbetsflödeskonfiguration automatiskt. Konfigurationen följer strukturen nedan:
   * `+ workflows`
      * `+ name1`
         * `- cq:path`
            * `- cq:workflowName`
* `@prop allowedParents` - Mönster för reguljära uttryck som avgör vilka sökvägar till mallar som tillåts som överordnade mallar.
* `@prop allowedChildren` - Mönster för reguljära uttryck som avgör vilka sökvägar till mallar som tillåts som underordnade mallar.
* `@prop ranking` - Placera i listan med mallar i dialogrutan Skapa sida.

**Definition**

* `[cq:Template] > nt:hierarchyNode, mix:title`
   * `- * (undefined)`
   * `- * (undefined) multiple`
   * `+ * (nt:base) = nt:base multiple version`
   * `+ jcr:content (nt:base) copy`
   * `+ icon.png (nt:file) copy`
   * `+ thumbnail.png (nt:file) copy`
   * `+ workflows (nt:base) copy`
   * `- allowedParents (string) multiple`
   * `- allowedChildren (string) multiple`
   * `- ranking (long)`

### `cq:Component` {#cq-component}

**Beskrivning**

Definierar en CQ-komponent.

* `@prop jcr:title` - Komponentens namn.
* `@prop jcr:description` - Beskrivning av komponenten.
* `@node dialog` - primär dialogruta.
* `@prop dialogPath` - sökväg till primär dialogruta (alternativ till dialogruta).
* `@node design_dialog` - Designdialog.
* `@prop cq:cellName` - Designcellens namn.
* `@prop cq:isContainer` - Anger om det är en behållarkomponent. Tvingar cellnamnen för de underordnade komponenterna att användas i stället för sökvägsnamn. `parsys` är till exempel en behållarkomponent. Om det här värdet inte har definierats görs kontrollen utifrån om det finns en `cq:childEditConfig`.
* `@prop cq:noDecoration` - Om värdet är true ritas inga `div`-dekorationstaggar när den här komponenten inkluderas.
* `@node cq:editConfig` - Den konfiguration som definierar parametrarna för redigeringsfältet.
* `@node cq:childEditConfig` - Redigeringskonfigurationen ärvs av underordnade komponenter.
* `@node cq:htmlTag` - Definierar ytterligare taggattribut som läggs till i den omgivande `div` -taggen när komponenten inkluderas.
* `@node icon.png` - En fil som innehåller en karakteristisk ikon.
* `@node thumbnail.png` - En fil som innehåller en karakteristisk miniatyrbild.
* `@prop allowedParents` - Mönster för reguljära uttryck som avgör vilka sökvägar för komponenter som tillåts som överordnade komponenter.
* `@prop allowedChildren` - Mönster för reguljära uttryck som avgör vilka sökvägar för komponenter som tillåts som underordnade komponenter.
* `@node virtual` - Innehåller undernoder som återspeglar virtuella komponenter som används för att dra och släppa komponenter.
* `@prop componentGroup` - Komponentgruppens namn, som används för att dra och släppa komponenten.
* `@node cq:infoProviders` - Innehåller undernoder, som var och en har egenskapen `className` som refererar till en `PageInfoProvider`.

**Definition**

* `[cq:Component] > nt:folder, mix:title, sling:ResourceSuperType`
   * `- * (undefined)`
   * `- * (undefined) multiple`
   * `+ * (nt:base) = nt:base multiple version`
   * `+ dialog (nt:base) = nt:unstructured copy`
   * `- dialogPath (string)`
   * `+ design_dialog (nt:base) = nt:unstructured copy`
   * `- cq:cellName (string)`
   * `- cq:isContainer (boolean)`
   * `- cq:noDecoration (boolean)`
   * `+ cq:editConfig (cq:EditConfig) = cq:EditConfig copy`
   * `+ cq:childEditConfig (cq:EditConfig) = cq:EditConfig copy`
   * `+ cq:htmlTag (nt:base) = nt:unstructured copy`
   * `+ icon.png (nt:file) copy`
   * `+ thumbnail.png (nt:file) copy`
   * `- allowedParents (string) multiple`
   * `- allowedChildren (string) multiple`
   * `+ virtual (nt:base) = sling:Folder copy`
   * `- componentGroup (string)`
   * `+ cq:infoProviders (nt:base) = nt:unstructured copy`

### `cq:ComponentMixin` {#cq-componentmixin}

**Beskrivning**

Definierar en CQ-komponent som en blandningstyp.

**Definition**

`[cq:ComponentMixin] > cq:Component mixin`

### `cq:EditConfig` {#cq-editconfig}

**Beskrivning**

Definierar konfigurationen för `editbar`.

* `@prop cq:dialogMode` - Dialogrutans läge:
   * `floating` - för en normal, flytande dialogruta
   * `inline` - infogad redigering
   * `auto` - automatisk identifiering (beroende på tillgängligt utrymme)
* `@node cq:inplaceEditing` - Redigeringskonfigurationen för den här komponenten är på plats.
* `@prop cq:layout`- Redigeringsfältets layout:
   * `editbar` - redigeringsfält
   * `rollover` - rulla över bildruta
   * `auto` - automatisk identifiering
* `@node cq:formParameters` - Ytterligare parametrar att lägga till i dialogformuläret.
* `@prop cq:actions` - Lista med åtgärder (redigeringsfältsknappar eller menyalternativ).
* `@node cq:actionConfigs` - Widget-konfigurationer för redigeringsfält eller menyalternativ.
* `@prop cq:emptyText` - Text som ska visas om det inte finns något visuellt innehåll.
* `@node cq:dropTargets` - Samling med `{@link cq:DropTargetConfig}` noder.

**Definition**

* `[cq:EditConfig] > nt:unstructured, nt:hierarchyNode orderable`
   * `- cq:dialogMode (string) < 'auto', 'floating', 'inline'`
   * `- cq:layout (string) < 'editbar', 'rollover', 'auto' + cq:formParameters (nt:base) = nt:unstructured`
   * `- cq:actions (string) multiple`
   * `+ cq:actionConfigs (nt:base) = nt:unstructured`
   * `- cq:emptyText (string)`
   * `+ cq:dropTargets (nt:base) = nt:unstructured`
   * `+ cq:listeners (nt:base) = cq:EditListenersConfig`

### `cq:DropTargetConfig` {#cq-droptargetconfig}

**Beskrivning**

Konfigurerar ett släppmål för en komponent. Namnet på den här noden används som ett ID för dra och släpp.

* `@prop accept` - Lista över MIME-typer som accepteras av det här släppmålet, till exempel `["image/*"]`
* `@prop groups` - Lista med dra och släpp-grupper som accepterar en källa.
* `@prop propertyName` - Namnet på egenskapen som används för att lagra referensen.

**Definition**

* `[cq:DropTargetConfig] > nt:unstructured orderable`
   * `- accept (string) multiple`
   * `- groups (string) multiple`
   * `- propertyName (string)`
   * `+ parameters (nt:base) = nt:unstructured`

### `cq:VirtualComponent` {#cq-virtualcomponent}

**Beskrivning**

Definierar en virtuell CQ-komponent. Används för närvarande endast för den nya guiden Dra och släpp för komponenten.

* `@prop jcr:title` - Den här komponentens namn.
* `@prop jcr:description` - Beskrivning av den här komponenten.
* `@node cq:editConfig` - Redigera konfiguration som definierar parametrarna för redigeringsfältet.
* `@node cq:childEditConfig` - Redigera konfiguration som ärvts av underordnade komponenter.
* `@node icon.png` - En fil som innehåller en karakteristisk ikon.
* `@node thumbnail.png` - En fil som innehåller en karakteristisk miniatyrbild.
* `@prop allowedParents` - Mönster för reguljära uttryck för att bestämma sökvägar för komponenter som tillåts som överordnade komponenter.
* `@prop allowedChildren` - Mönster för reguljära uttryck för att bestämma sökvägar för komponenter som tillåts som underordnade komponenter.
* `@prop componentGroup` - Namnet på komponentgruppen för dra och släpp-komponenten.

**Definition**

`[cq:VirtualComponent] > nt:folder, mix:title`
`- * (undefined)`
`- * (undefined) multiple`
`+ * (nt:base) = nt:base multiple version`
`+ cq:editConfig (cq:EditConfig) = cq:EditConfig copy`
`+ icon.png (nt:file) copy`
`+ thumbnail.png (nt:file) copy`
`- allowedParents (string) multiple`
`- allowedChildren (string) multiple`
`- componentGroup (string)`

### `cq:EditListenersConfig` {#cq-editlistenersconfig}

**Beskrivning**

Definierar de (klientsidan) avlyssnare som ska köras i en edit-händelse. Värdena måste antingen referera till en giltig avlyssnarfunktion på klientsidan eller innehålla ett fördefinierat kortkommando:

* `REFRESH_PAGE`
* `REFRESH_SELF`
* `REFRESH_PARENT`

* `@prop aftercreate` - Utlöses när en komponent har skapats.
* `@prop afteredit` - Utlöses när en komponent har redigerats (ändrats).
* `@prop afterdelete` - Utlöses när en komponent har tagits bort.
* `@prop afterinsert` - Utlöses när en komponent har lagts till i den här behållaren.
* `@prop afterremove` - Utlöses när en komponent har tagits bort från den här behållaren.
* `@prop aftermove` - Utlöses när komponenter har flyttats i den här behållaren.

**Definition**

* `[cq:EditListenersConfig]`
   * `- &ast; (undefined)`
   * `- &ast; (undefined) multiple`
   * `+ &ast; (nt:base) = nt:base multiple version`
   * `- aftercreate (string)`
   * `- afteredit (string)`
   * `- afterdelete (string)`
   * `- afterinsert (string)`
   * `- afterremove (string)`
   * `- aftermove (string)`

## DAM {#dam}

### `dam:AssetContent` {#dam-assetcontent}

**Beskrivning**

Innehåll i en DAM-resurs.

**Definition**

* `[dam:AssetContent] > nt:unstructured`
   * `+ metadata (nt:unstructured)`
   * `+ renditions (nt:folder)`

### `dam:Asset` {#dam-asset}

**Beskrivning**

DAM-resurs.

**Definition**

`[dam:Asset] > nt:hierarchyNode`
`+ jcr:content (dam:AssetContent) = dam:AssetContent copy primary`
`+ * (nt:base) = nt:base version`

### `dam:Thumbnail` {#dam-thumbnail}

**Beskrivning**

Miniatyrbild som representerar en DAM-resurs.

**Definition**

* `[dam:Thumbnails]`
   * `mixin`
   * `+ dam:thumbnails (nt:folder)`

## Leveransbehållarlista {#delivery-container-list}

### `cq:containerList` {#cq-containerlist}

**Beskrivning**

Behållarlista.

**Definition**

* `[cq:containerList]`
   * `mixin`

## Leveranssida {#delivery-page}

### `cq:Cq4PageAttributes` {#cq-cq-pageattributes}

**Beskrivning**

Nodtypen `cq:attributes` är för ContentBus-versionstaggar. Den här noden har bara en serie egenskaper av vilka tre är fördefinierade: `created`, `csd` och `timestamp`.

* `@prop created (long) mandatory copy` - Tidsstämpel för när versionsinformationen skapades, vanligtvis tiden för incheckning av den tidigare versionen eller tiden då sidan skapades.
* `@prop csd (string) mandatory copy` - Standardattributet `csd`, kopia av egenskapen `cq:csd` för sidnoden
* `@prop timestamp (long) mandatory copy` - Tidsstämpel för senaste versionsändring, vanligtvis incheckningstid.
* `@prop * (string) copy` - Ytterligare attribut, versionsindelade med den överordnade noden.

**Definition**

* `[cq:Cq4PageAttributes] > nt:base`
   * `- created (long) mandatory copy`
   * `- csd (string) mandatory copy`
   * `- timestamp (long) mandatory copy`
   * `- &ast; (string) copy`

### `cq:Cq4ContentPage` {#cq-cq-contentpage}

**Beskrivning**

Nodtypen `cq:contentPage` innehåller egenskaps- och undernoddefinitioner för ContentBus innehållssidor. Endast när den här blandningstypen läggs till i en nod av typen `cq:page` blir en nod en ContentBus-innehållssida.

Objekten i en `cq:Cq4ContentPage` är:

* `@prop cq:csd` - Sidans ContentBus-värdepapperscentral.
* `@node cq:content` - Sidans innehåll. Den underordnade noden finns inte om sidnoden är i läget &quot;Befintlig utan innehåll&quot; eller &quot;Borttagen&quot;.
* `@node cq:attributes` - Listan med sidattribut, som tidigare kallades versionstaggar. Den här noden är obligatorisk för typen `cq:contentPage`. Attributnoden får en ny version när sidnoden får en ny version.

**Definition**

* `[cq:Cq4ContentPage]`
   * `- cq:csd (string) mandatory copy`
   * `+ cq:attributes (cq:Cq4PageAttributes)`

## Importör {#importer}

### `cq:PollConfig` {#cq-pollconfig}

**Beskrivning**

Avsökningskonfiguration.

* `@prop source (String) mandatory` - URI för datakälla. Obligatoriskt och får inte vara tomt.
* `@prop target (String)` - Målplatsen där data som hämtats från datakällan lagras. Valfritt och standardvärdet är cq:PollConfig-noden.
* `@prop interval (Long)` - Intervallet i sekunder som nya eller uppdaterade data från datakällan ska avsökas. Valfritt och standardvärdet är 30 minuter (1 800 sekunder).
* [Skapar anpassade dataimporteringstjänster för Adobe Experience Manager](https://experienceleague.adobe.com/sv/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview)

**Definition**

* `[cq:PollConfig]`
   * `mixin`
   * `- source (String) mandatory`
   * `- target (String)`
   * `- interval (Long)`

### `cq:PollConfigFolder` {#cq-pollconfigfolder}

**Beskrivning**

Använd den primära nodtypen för att enkelt skapa avfrågningskonfigurationsnoder.

**Definition**

`[cq:PollConfigFolder] > sling:Folder, cq:PollConfig`

## Plats {#location}

### `cq:GeoLocation` {#cq-geolocation-1}

**Beskrivning**

En blandning som definierar en geografisk plats i decimalgrader (DD).

* `@prop latitude` - Latitud kodad som dubbel med decimalgrader.
* `@prop longitude` - Longitud kodad som dubbel med decimalgrader.

**Definition**

* `[cq:GeoLocation]`
   * `mixin`
   * `- latitude (double)`
   * `- longitude (double)`

## Mailer {#mailer}

### `cq:mailerMessage` {#cq-mailermessage}

**Beskrivning**

MailerService-nodtyper. Mejlaren använder noder som har den här mixinen som rotnoder för meddelandedefinitioner.

**Definition**

* `[cq:mailerMessage]`
   * `mixin`
   * `- messageStatus (string)`
   * `= 'new'`
   * `mandatory autocreated`

## MSM {#msm}

### `cq:LiveRelationship` {#cq-liverelationship}

**Beskrivning**

Definierar en LiveRelationship-blandning. En primär källnod (kontrollnod) och en live-kopia (kontrollerad)-nod kan vara praktiskt taget länkad via en LiveRelationship.

**Definition**

* `[cq:LiveRelationship] mixin`
   * `- cq:lastRolledout (date)`
   * `- cq:lastRolledoutBy (string)`
   * `- cq:sourceUUID (string)`

### `cq:LiveSync` {#cq-livesync}

**Beskrivning**

Definierar en LiveSync-blandning. Om en nod ingår i en LiveRelationship med en primär källnod (kontrollnod) och en live-kopia (kontrollerad)-nod, markeras den som LiveSync.

* `@prop cq:master` - Sökväg till den primära källan (kontroll) för LiveRelationship.
* `@prop cq:isDeep` - Definierar om relationen är tillgänglig för underordnade.
* `@prop cq:syncTrigger` - Definierar när synkroniseringen aktiveras.
* `@node * LiveSyncAction` - Åtgärder som ska utföras vid synkronisering

**Definition**

`[cq:LiveSync] > cq:LiveRelationship mixin orderable`
`+ * (cq:LiveSyncAction) = cq:LiveSyncAction`
`+ cq:LiveSyncConfig (nt:base) = cq:LiveSyncConfig`

### `cq:LiveSyncCancelled` {#cq-livesynccancelled}

**Beskrivning**

Definierar en LiveSyncCanceled-blandning. Avbryt LiveSync-beteendet för en live-kopia (kontrollerad)-nod som kan vara inblandad i en LiveRelationship på grund av någon av dess överordnade noder.

* `@prop cq:isCancelledForChildren` - Definierar om en LiveSync är avbruten, även för underordnade.

**Definition**

* `[cq:LiveSyncCancelled] > cq:LiveRelationship mixin`
   * `- cq:isCancelledForChildren (boolean)`

### `cq:LiveSyncAction` {#cq-livesyncaction}

**Beskrivning**

Definierar en LiveSyncAction som är kopplad till en LiveSync.

* `@prop name` - Åtgärdsnamn
* `@prop value` - Åtgärdsvärde

**Definition**

* `[cq:LiveSyncAction] > nt:unstructured`

### `cq:LiveSyncConfig` {#cq-livesyncconfig}

**Beskrivning**

Konfiguration av Live Sync.

**Definition**

* `[cq:LiveSyncConfig]`
   * `- cq:master (string) mandatory`
   * `- cq:isDeep (boolean)`
   * `- cq:trigger (string) /** deprecated **/`

För AEM 5.4 ska följande läggas till i slutet av listan:

* `- cq:rolloutConfigs (string) multiple /** deprecated **/`

### `cq:BlueprintAction` {#cq-blueprintaction}

**Beskrivning**

Funktionsmakro för utkast

**Definition**

* `[cq:BlueprintAction] > nt:unstructured`

## Plattform {#platform}

### `cq:Console` {#cq-console}

**Beskrivning**

Definierar nodtypen för en konsolnod.

**Definition**

* `[cq:Console] > sling:VanityPath, mix:title`
   * `mixin`

## Replikering {#replication}

### `cq:ReplicationStatus` {#cq-replicationstatus}

**Beskrivning**

Definierar blandning av information om replikeringsstatus.

* `@prop cq:lastPublished` - Det datum då sidan senast publicerades (används inte längre).
* `@prop cq:lastPublishedBy` - Användaren som publicerade sidan senast (används inte längre).
* `@prop cq:lastReplicated` - Det datum då sidan senast replikerades.
* `@prop cq:lastReplicatedBy` - Användaren som replikerade sidan senast.
* `@prop cq:lastReplicationAction` - Replikeringsåtgärden: aktivera eller inaktivera.
* `@prop cq:lastReplicationStatus` - Replikeringsstatusen (används inte längre).

**Definition**

* `[cq:ReplicationStatus]`
   * `mixin`
   * `- cq:lastPublished (date) ignore`
   * `- cq:lastPublishedBy (string) ignore`
   * `- cq:lastReplicated (date) ignore`
   * `- cq:lastReplicatedBy (string) ignore`
   * `- cq:lastReplicationAction (string) ignore`
   * `- cq:lastReplicationStatus (string) ignore`

## Dokumentskydd {#security}

### `cq:ApplicationPrivilege` {#cq-applicationprivilege}

**Beskrivning**

Den definierar ett programprivilegium.

**Definition**

* `[cq:ApplicationPrivilege] mixin`

### `cq:PrivilegeAcl` {#cq-privilegeacl}

**Beskrivning**

Den definierar en åtkomstkontrollista för programbehörighet.

* `@prop cq:isPathDependent`
* `@node * ACEs`

**Definition**

* `[cq:PrivilegeAcl] > cq:ApplicationPrivilege mixin orderable`
   * `- cq:isPathDependent (boolean)`
   * `+ * (cq:PrivilegeAce) = cq:PrivilegeAce`

### `cq:PrivilegeAce` {#cq-privilegeace}

**Beskrivning**

Den definierar en ACE för ett programprivilegium.

* `@prop path`
* `@prop deny`

**Definition**

* `[cq:PrivilegeAce]`
   * `- path mandatory`
   * `- deny (boolean)`

### `cq:ApplicationPrivilege` {#cq-applicationprivilege-1}

**Beskrivning**

Den definierar ett programprivilegium.

**Definition**

* `[cq:ApplicationPrivilege] mixin`

### `cq:PrivilegeAcl` {#cq-privilegeacl-1}

**Beskrivning**

Den definierar en åtkomstkontrollista för programbehörighet.

* `@prop cq:isPathDependent`
* `@node * ACEs`

**Definition**

* `[cq:PrivilegeAcl] > cq:ApplicationPrivilege mixin orderable`
   * `- cq:isPathDependent (boolean)`
   * `+ * (cq:PrivilegeAce) = cq:PrivilegeAce`

### `cq:PrivilegeAce` {#cq-privilegeace-1}

**Beskrivning**

Den definierar en ACE för ett programprivilegium.

* `@prop path`
* `@prop deny`

**Definition**

* `[cq:PrivilegeAce]`
   * `- path mandatory`
   * `- deny (boolean)`

## Platsimportör {#site-importer}

### `cq:ComponentExtractorSource` {#cq-componentextractorsource}

**Beskrivning**

Definierar en blandningstyp som markerar filer som kan öppnas med en komponentextraherare.

**Definition**

`[cq:ComponentExtractorSource] mixin`

## Taggar {#tagging}

### `cq:Tag` {#cq-tag}

**Beskrivning**

Definierar en enskild tagg, men kan även innehålla taggar, vilket skapar en taxonomi

**Definition**

* `[cq:Tag] > nt:base, mix:title`
   * `- sling:resourceType (String)`
   * `- * (undefined) multiple`
   * `- * (undefined)`
   * `+ * (nt:base) = cq:Tag version`

### `cq:Taggable` {#cq-taggable}

**Beskrivning**

Abstrakt basblandning för taggningsbart innehåll.

* `@node cq:tags`

**Definition**

* `[cq:Taggable]`
   * `- cq:tags (string) multiple`

### `cq:OwnerTaggable` {#cq-ownertaggable}

**Beskrivning**

Endast författare/ägare får tagga innehållet (modererad/administrerad taggning).

**Definition**

* `[cq:OwnerTaggable] > cq:Taggable`

### `cq:UserTaggable` {#cq-usertaggable}

**Beskrivning**

Alla användare och offentliga webbplatser kan tagga innehållet (Web2.0-format) som används i `cq:userContent`.

**Definition**

* `[cq:UserTaggable] > cq:Taggable`
   * `mixin`

### `cq:AllowsUserContent` {#cq-allowsusercontent}

**Beskrivning**

Lägger till en `cq:userContent`-undernod som användare kan redigera. Varje användare har sin egen `cq:userContent/<userid>`-undernod, som vanligtvis har mixin `cq:UserTaggable`.

**Definition**

* `[cq:AllowsUserContent]`
   * `mixin`
   * `+ cq:userContent (nt:unstructured)`

Utökad variant, definierar mer explicit trädet `cq:userContent`

* `[cq:AllowsUserContent]`
   * `mixin`
   * `+ cq:userContent (cq:UserContent)`

### `cq:UserContent` {#cq-usercontent}

**Beskrivning**

Användare kan redigera det.

**Definition**

* `[cq:UserContent] > nt:unstructured`
   * `// userids`
   * `+ * (cq:UserData)`
   * `// other content`
   * `+ * (nt:base)`

### `cq:UserData` {#cq-userdata}

**Beskrivning**

Användardata

**Definition**

* `[cq:UserData] > nt:unstructured, cq:UserTaggable`

## Widgetar {#widgets}

### `cq:ClientLibraryFolder` {#cq-clientlibraryfolder}

**Beskrivning**

Klientbiblioteksmapp

**Definition**

* `[cq:ClientLibraryFolder] > sling:Folder`
   * `- categories (string) multiple`
   * `- dependencies (string) multiple`

### `cq:Widget` {#cq-widget}

**Beskrivning**

Widget

**Definition**

* `[cq:Widget] > nt:unstructured orderable`
   * `- xtype (string)`
   * `- name (string)`
   * `- title (string)`
   * `+ items (nt:base) = cq:WidgetCollection copy`

### `cq:WidgetCollection` {#cq-widgetcollection}

**Beskrivning**

Widget-samling

**Definition**

* `[cq:WidgetCollection] > nt:unstructured`
   * `orderable`
   * `+ * (cq:Widget) = cq:Widget copy`

### `cq:Dialog` {#cq-dialog}

**Beskrivning**

Dialog

**Definition**

* `[cq:Dialog] > cq:Widget orderable`

### `cq:Panel` {#cq-panel}

**Beskrivning**

Panel

**Definition**

`[cq:Panel] > cq:Widget orderable`

### `cq:TabPanel` {#cq-tabpanel}

**Beskrivning**

Panelen Tabb

**Definition**

* `[cq:TabPanel]` > `cq:Panel orderable`
   * `- activeTab (long)`

### `cq:Field` {#cq-field}

**Beskrivning**

Fält

**Definition**

* `[cq:Field] > cq:Widget orderable`
   * `- fieldLabel (string)`
   * `- value (string)`
   * `- ignoreData (boolean)`

## Wiki {#wiki}

### `wiki:Topic` {#wiki-topic}

**Beskrivning**

Wiki-ämne

**Definition**

* `[wiki:Topic] > nt:unstructured, nt:hierarchyNode, mix:versionable, mix:lockable`
   * `+ * (wiki:Topic) version`
   * `+ wiki:attachments (nt:folder) = nt:folder version`
   * `+ wiki:properties (wiki:Properties) = wiki:Properties copy`
   * `- wiki:text (string) mandatory primary`
   * `- wiki:lastModified (date) mandatory`
   * `- wiki:lastModifiedBy (string) mandatory`
   * `- wiki:topicName`
   * `- wiki:topicTitle`
   * `- wiki:lockedBy`
   * `- wiki:logMessage (string)`
   * `- wiki:quietSave (boolean)`

### `wiki:User` {#wiki-user}

**Beskrivning**

Wiki-användare

**Definition**

* `[wiki:User] mixin`
   * `- wiki:subscriptions (string) multiple`

### `wiki:Properties` {#wiki-properties}

**Beskrivning**

Wiki-egenskaper

**Definition**

* `[wiki:Properties]`
   * `- wiki:isGlobal (boolean)`
   * `- * (undefined)`

## Arbetsflöde {#workflow}

### `cq:Workflow` {#cq-workflow}

**Beskrivning**

Representerar en arbetsflödesinstans.

**Definition**

* `[cq:Workflow] > nt:base, mix:referenceable`
   * `- modelId (String)`
   * `- modelVersion (String)`
   * `- startTime (Date)`
   * `- endTime (Date)`
   * `- initiator (String)`
   * `- &ast; (undefined)`
   * `- &ast; (undefined) multiple`
   * `- sling:resourceType (String) = "cq/workflow/components/instance" mandatory autocreated`
   * `+ workflowStack (nt:unstructured)`
   * `+ wait (nt:unstructured)`
   * `+ orTab (nt:unstructured)`
   * `+ data (cq:WorkflowData)`
   * `+ history (nt:unstructured)`
   * `+ metaData (nt:unstructured)`
   * `+ workItems (nt:unstructured)`

### `cq:WorkItem` {#cq-workitem}

**Beskrivning**

Arbetsobjekt.

**Definition**

* `[cq:WorkItem]`
   * `- assignee (String)`
   * `- workflowId (String)`
   * `- nodeId (String)`
   * `- startTime (Date)`
   * `- endTime (Date)`
   * `- dueTime (Date)`
   * `- sling:resourceType (String) = "cq/workflow/components/workitem" mandatory autocreated`
   * `+ metaData (nt:unstructured)`

### `cq:Payload` {#cq-payload}

**Beskrivning**

Nyttolast

**Definition**

* `[cq:Payload]`
   * `- path (Path)`
   * `- uuid (String)`
   * `- jcr:url (String)`
   * `- binary (Binary)`
   * `- javaObject (String)`
   * `- * (undefined)`
   * `- * (undefined) multiple`

### `cq:WorkflowData` {#cq-workflowdata}

**Beskrivning**

Arbetsflödesdata

**Definition**

* `[cq:WorkflowData]`
   * `- * (undefined)`
   * `- * (undefined) multiple`
   * `+ payload (cq:Payload)`
   * `+ metaData (nt:unstructured) copy`

### `cq:WorkflowModel` {#cq-workflowmodel}

**Beskrivning**

Tilldela arbetsflödeskonfiguration automatiskt. Konfigurationen följer den här strukturen nedan:

* `workflows`
   * `+ name1`
      * `- cq:path`
      * `- cq:workflowName`
   * `+ workflows (nt:base)`

**Definition**

* `[cq:WorkflowModel] > nt:base, mix:versionable`
   * `orderable`
   * `- title (String)`
   * `- description (String)`
   * `- sling:resourceType (String) = "cq/workflow/components/model" mandatory autocreated`
   * `+ nodes (nt:unstructured)`
      * `copy`
   * `+ transitions (nt:unstructured)`
      * `copy`
   * `+ metaData (nt:unstructured)`
      * `copy`

### `cq:WorkflowNode` {#cq-workflownode}

**Beskrivning**

Arbetsflödesnod

**Definition**

* `[cq:WorkflowNode] orderable`
   * `- title (String)`
   * `- description (String)`
   * `- maxIdleTime (long)`
   * `- type (String)`
   * `- * (undefined)`
   * `- * (undefined) multiple`
   * `+ metaData (nt:unstructured)`
      * `copy`
   * `+ timeoutConfiguration (nt:unstructured)`
      * `copy`

### `cq:WorkflowTransition` {#cq-workflowtransition}

**Beskrivning**

Arbetsflödesövergång

**Definition**

* `[cq:WorkflowTransition] orderable`
   * `- from (String)`
   * `- to (String)`
   * `- rule (String)`
   * `+ metaData (nt:unstructured)`
      * `copy`

### `cq:OrTab` {#cq-ortab}

**Beskrivning**

Eller tabb

**Definition**

* `[cq:OrTab]`
   * `- workflowId (String) // not compulsory as this node is already be attached to the workflow node`
   * `- nodeId (String)`

### `cq:Wait` {#cq-wait}

**Beskrivning**

Vänta

**Definition**

* `[cq:Wait]`
   * `- workflowId (String) // not compulsory as this node is already attached to the workflow node`
   * `- destNodeId (String)`
   * `- fromNodeId (String)`

### `cq:WorkflowStack` {#cq-workflowstack}

**Beskrivning**

Arbetsflödesstack

**Definition**

* `[cq:WorkflowStack]`
   * `- containeeInstanceId (String)`
   * `- parentInstanceId (String)`
   * `- nodeId (String)`

### `cq:ProcessStack` {#cq-processstack}

**Beskrivning**

Processstapel

**Definition**

* `[cq:ProcessStack]`
   * `- workflowId (String) // not compulsory as this node is already attached to the workflow node`
   * `- containerWorkflowModelId (String)`
   * `- containerWorkflowNodeId`
   * `- containerWorkflowEndNodeId // still needed (if name already defines that id)`

### `cq:WorkflowLauncher` {#cq-workflowlauncher}

**Beskrivning**

Starta arbetsflöde

**Definition**

* `[cq:WorkflowLauncher]`
   * `- nodetype (String)`
   * `- glob (String)`
   * `- eventType (Long)`
   * `- description (String)`
   * `- condition (String)`
   * `- workflow (String)`
   * `- * (undefined)`
   * `- * (undefined) multiple`
