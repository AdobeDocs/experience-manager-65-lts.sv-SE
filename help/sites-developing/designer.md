---
title: Designen och Designer
description: Lär dig hur du skapar en design för din webbplats och i AEM med Designer.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 6605deda-99b8-4447-b62d-a1a50c4eed30
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---

# Designen och Designer{#designs-and-the-designer}

>[!CAUTION]
>
>I den här artikeln beskrivs hur du skapar en webbplats baserat på det klassiska användargränssnittet. Adobe rekommenderar att du använder de senaste AEM-teknikerna för dina webbplatser enligt beskrivningen i artikeln [Komma igång med att utveckla AEM Sites](/help/sites-developing/getting-started.md).

Designer används för att skapa en design för din webbplats med det [klassiska användargränssnittet](/help/sites-classic-ui-authoring/classicui.md) i AEM.

>[!NOTE]
>
>Mer information om webbtillgänglighet finns i [AEM och Web Accessibility Guidelines](/help/managing/web-accessibility.md).

## Använda Designer {#using-the-designer}

Din design kan definieras i avsnittet **designs** på fliken **Verktyg** :

![screen_shot_2012-02-01at30237pm](assets/screen_shot_2012-02-01at30237pm.png)

Här kan du skapa den struktur som krävs för att lagra designen och sedan överföra CSS och bilder som behövs.

Designer lagras under `/apps/<your-project>`. Sökvägen till designen som ska användas för en webbplats anges med egenskapen `cq:designPath` för noden `jcr:content`.

![chlimage_1-74](assets/chlimage_1-74a.png)

>[!NOTE]
>
>Alla ändringar som görs på en sida i designläge bevaras under designnoden på webbplatsen och tillämpas automatiskt på alla sidor som har samma design.

## Vad du behöver {#what-you-will-need}

För att förverkliga din design behöver du:

**CSS** - Cascading Style Sheets definierar formaten för specifika områden på sidorna.
**Bilder** - Bilder som du använder för funktioner som bakgrunder, knappar.

### Att tänka på när du utformar din webbplats {#considerations-when-designing-your-website}

När du utvecklar en webbplats bör du lagra bilder och CSS-filer under `/apps/<your-project>` så att du kan referera till dina resurser baserat på den aktuella designen, enligt följande kodutdrag.

```xml
<%= currentDesign.getPath() + "/static/img/icon.gif %>
```

Exemplet ovan ger flera fördelar:

* Komponenter kan ha olika utseende/känsla baserat på varje plats med olika designsökvägar.
* Du kan designa om webbplatsen genom att peka designsökvägen till en annan nod i webbplatsens rot från `design/v1` till `design/v2.`

* `/etc/designs` och `/content` är de enda externa URL:er som webbläsaren ser som skyddar dig mot att en extern användare blir nyfiken på vad som finns under ditt `/apps` -träd. Ovanstående URL-fördelar hjälper också systemadministratören att ställa in bättre säkerhet eftersom du begränsar exponeringen av resurserna till några få distinkta platser.
