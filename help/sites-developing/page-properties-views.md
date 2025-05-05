---
title: Anpassa vyer av Sidegenskaper
description: Varje sida har en uppsättning egenskaper som du kan redigera efter behov
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 027e086f-0883-45de-9531-b8119c99b118
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---

# Anpassa vyer av Sidegenskaper{#customizing-views-of-page-properties}

Varje sida har en uppsättning [egenskaper](/help/sites-authoring/editing-page-properties.md) som användare kan visa och redigera. En del krävs när sidan skapas (skapa vy), andra kan visas och redigeras (redigeringsvy) i ett senare skede. Dessa sidegenskaper definieras och görs tillgängliga genom dialogrutan ( `cq:dialog`) för den aktuella sidkomponenten.

>[!CAUTION]
>
>Det klassiska användargränssnittet gör det inte möjligt att anpassa vyn för sidegenskaper.

Standardläget för varje sidegenskap är:

* dolda i skapandevyn (t.ex. guiden **Skapa sida**)

* som finns i redigeringsvyn (till exempel **Vyegenskaper**)

Fälten måste vara specifikt konfigurerade om någon ändring krävs. Detta görs med lämpliga nodegenskaper:

* Sidegenskap som ska vara tillgänglig i skapandevyn (t.ex. guiden **Skapa sida**):

   * Namn: `cq:showOnCreate`
   * Typ: `Boolean`

* Sidegenskapen ska vara tillgänglig i redigeringsvyn (till exempel alternativet **Visa**/**Redigera**) **Egenskaper**):

   * Namn: `cq:hideOnEdit`
   * Typ: `Boolean`

Se till exempel inställningarna för fält som är grupperade under **Fler rubriker och beskrivning** på fliken **Grundläggande** för bassidkomponenten. Dessa är synliga i guiden **Skapa sida** eftersom `cq:showOnCreate` har angetts till `true`:

```xml
/libs/foundation/components/page/cq:dialog/content/items/tabs/items/basic/items/column/items/moretitles
```

>[!TIP]
>
>I självstudiekursen [Utöka sidegenskaper](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/page-properties-technical-video-develop.html) finns en guide om hur du anpassar sidegenskaper.

## Konfigurera dina sidegenskaper {#configuring-your-page-properties}

Du kan också konfigurera fälten som är tillgängliga genom att konfigurera dialogrutan för sidkomponenten och använda lämpliga nodegenskaper.

Som standard visar guiden **[&#128279;](/help/sites-authoring/managing-pages.md#creating-a-new-page)** Skapa sida de fält som är grupperade under **Fler rubriker och beskrivning**. Så här döljer du dessa konfigurationer:

1. Skapa sidkomponenten under `/apps`.
1. Skapa en åsidosättning (med *dialog diff* från [Sling Resource Merger](/help/sites-developing/sling-resource-merger.md)) för `basic`-delen av sidkomponenten, till exempel:

   ```xml
   <your-page-component>/cq:dialog/content/items/tabs/items/basic
   ```

   >[!NOTE]
   >
   >Se följande som referens:
   >
   >    `/libs/wcm/foundation/components/basicpage/v1/basicpage/cq:dialog`
   >
   >Du ***får*** inte ändra något i sökvägen `/libs`.
   >
   >Detta beror på att innehållet i `/libs` skrivs över nästa gång du uppgraderar din instans (och kan mycket väl skrivas över när du använder en snabbkorrigering eller ett funktionspaket).
   >
   >Den rekommenderade metoden för konfiguration och andra ändringar är:
   >
   >1. Återskapa det obligatoriska objektet (det vill säga som det finns i `/libs`) under `/apps`
   >1. Gör ändringar i `/apps`

1. Ställ in egenskapen `path` för `basic` så att den pekar på åsidosättningen av grundfliken (se även nästa steg). Till exempel:

   ```xml
   /apps/demos/components/page/tabs/basic
   ```

1. Skapa en åsidosättning av avsnittet `basic` - `moretitles` vid motsvarande sökväg, till exempel:

   ```xml
   /apps/demos/components/page/tabs/basic/items/column/items/moretitles
   ```

1. Använd lämplig nodegenskap:

   * **Namn**: `cq:showOnCreate`
   * **Typ**: `Boolean`
   * **Värde**: `false`

   Avsnittet **Fler rubriker och beskrivning** visas inte längre i guiden **Skapa sida**.

>[!NOTE]
>
>Mer information finns i [Konfigurera MSM-lås på Sidegenskaper](/help/sites-developing/extending-msm.md#configuring-msm-locks-on-page-properties-touch-enabled-ui) när du konfigurerar sidegenskaper för användning med live-kopior.

## Exempelkonfiguration av sidegenskaper {#sample-configuration-of-page-properties}

Det här exemplet visar dialogtekniken för [Sling Resource Merger](/help/sites-developing/sling-resource-merger.md), inklusive användningen av [`sling:orderBefore`](/help/sites-developing/sling-resource-merger.md#properties). Det visar också hur både `cq:showOnCreate` och `cq:hideOnEdit` används.

KOD PÅ GITHUB

Koden för den här sidan finns på GitHub

* [Öppna aem-authoring-extension-page-dialog-projekt på GitHub](https://github.com/Adobe-Marketing-Cloud/aem-authoring-extension-page-dialog)
