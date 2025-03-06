---
title: Konfigurera din sida för gruppredigering av sidegenskaper
description: Med gruppredigering av sidegenskaper kan du redigera egenskaperna för flera sidor samtidigt
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Developer
exl-id: 7ba0a378-0b52-40bd-8dab-32490594558e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---

# Konfigurera din sida för gruppredigering av sidegenskaper {#configuring-your-page-for-bulk-editing-of-page-properties}

[Med gruppredigering av sidegenskaper](/help/sites-authoring/editing-page-properties.md#from-the-sites-console-multiple-pages) kan du redigera egenskaperna för flera sidor samtidigt.

På grund av möjligheten att det finns olika värden är sidegenskaperna inte aktiverade för massredigering som standard. De måste vara uttryckligen tillåtna (aktiverade). När du definierar vilka sidegenskaper som ska vara tillgängliga för massredigering måste du ta hänsyn till vissa konsekvenser, till exempel:

* Vissa fält är vanligtvis unika, till exempel en sidtitel. Bestäm om det är meningsfullt att aktivera sådana fält för massredigering, när ett värde ska användas.
* Vissa fält kan ha flera värden - detta kräver en meningsfull representation vid återgivningen.

  Till exempel en kryssruta som anger&quot;Klart för publikation&quot;. Detta kan ha flera värden före gruppredigering (t.ex. ready, in-review, in-progress).

>[!CAUTION]
>
>Massredigering av sidegenskaper är:
>
>* Inte tillgängligt i det klassiska användargränssnittet.
>* Inte tillgängligt för sidor i en live-kopia.
>* Endast tillgängligt för sidor med samma resurstyp.
>

>[!NOTE]
>
>Massredigering finns också för Assets. Den är mycket lik, men skiljer sig på några punkter. Mer information finns i [Redigera egenskaper för flera Assets](/help/assets/metadata.md). Du kan anpassa fälten i redigeraren för massmetadata för Assets med [schemaredigeraren](/help/assets/metadata-schemas.md).

## Aktivera ett fält {#enabling-a-field}

>[!NOTE]
>
>Vissa fält kan ha flera värden - detta kräver en meningsfull representation vid återgivningen. Därför bör du bara aktivera följande fälttyper:
>
>* `/libs/granite/ui/components/foundation/form/textfield`
>* `/libs/granite/ui/components/foundation/form/textarea`
>* `/libs/granite/ui/components/foundation/form/tagspicker`
>* `/libs/granite/ui/components/foundation/form/datepicker`
>* `/libs/granite/ui/components/foundation/form/pathbrowser`
>* `/libs/granite/ui/components/foundation/form/checkbox`
>

Fält är aktiverade i sidkomponenten (*inte* i mallen):

1. Öppna sidkomponenten med CRXDE Lite (eller en motsvarande metod).

   Till exempel: `/apps/core/wcm/components/page/v1/page`

   >[!NOTE]
   >
   >I det här exemplet antas att kärnkomponenterna har installerats på instansen, vilket är fallet om instansen körs med exempelinnehållet We.Retail. Mer information finns i [dokumentationen för kärnkomponenter](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html).

1. Navigera till det obligatoriska fältet i definitionen `cq:dialog`.
1. Definiera följande egenskap på fältnoden:

   * **Namn**: `allowBulkEdit`
   * **Typ**: `Boolean`
   * **Värde**: `true`

   Exempel: för standardsidan [Foundation component](/help/sites-authoring/default-components-foundation.md):

   `/libs/foundation/components/page`

   Egenskapen skulle definieras för:

   `cq:dialog/content/items/tabs/items/basic/items/column/items/onofftime/items/ondate`

   >[!CAUTION]
   >
   >Du ***får*** inte ändra något i sökvägen `/libs`.
   >
   >Detta beror på att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan mycket väl skrivas över när du använder en snabbkorrigering eller ett funktionspaket).
   >
   >Den rekommenderade metoden för konfiguration och andra ändringar är:
   >
   >    1. Återskapa det obligatoriska objektet (det vill säga som det finns i `/libs`) under `/apps`
   >    1. Gör ändringar i `/apps`

1. Välj **Spara alla** om du vill behålla uppdateringarna.
