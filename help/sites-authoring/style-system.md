---
title: Formatsystem
description: Med Style System kan mallskapare definiera formatklasser i en komponents innehållsprincip så att en innehållsförfattare kan markera dem när komponenten på en sida redigeras. Dessa format kan vara alternativa visuella varianter av en komponent, vilket gör den mer flexibel.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: a3d42a73-e1b2-4f76-b98a-89cd98eea2c9
source-git-commit: 1b01d338c8400f38e4f062cb3ee592062f560a67
workflow-type: tm+mt
source-wordcount: '1292'
ht-degree: 1%

---

# Formatsystem{#style-system}

Med Style System kan mallskapare definiera formatklasser i en komponents innehållsprincip så att en innehållsförfattare kan markera dem när komponenten på en sida redigeras. Dessa format kan vara alternativa visuella varianter av en komponent, vilket gör komponenten mer flexibel.

På så sätt elimineras behovet av att utveckla en anpassad komponent för varje format eller att anpassa komponentdialogrutan för att aktivera sådana formatfunktioner. Det leder till mer återanvändbara komponenter som snabbt och enkelt kan anpassas efter innehållsförfattarnas behov utan någon AEM-backend-utveckling.

## Användningsfall {#use-case}

Mallförfattare behöver inte bara kunna konfigurera hur komponenterna fungerar för innehållsförfattarna, utan även konfigurera flera alternativa visuella varianter av en komponent.

På samma sätt behöver innehållsförfattare inte bara kunna strukturera och ordna sitt innehåll, utan också kunna välja hur det ska presenteras visuellt.

Style System ger en enhetlig lösning för både mallskaparens och innehållsförfattarens krav:

* Mallförfattare kan definiera formatklasser i komponenternas innehållsprincip.
* Innehållsförfattare kan sedan välja dessa klasser i en listruta när de redigerar komponenten på en sida för att tillämpa motsvarande format.

Klassen style infogas sedan i elementet dekoration wrapper i komponenten så att komponentutvecklaren inte behöver bekymra sig om att hantera formaten utöver att tillhandahålla sina CSS-regler.

## Ökning {#overview}

Vanligtvis har du följande format när du använder Style System.

1. Webbdesignern skapar olika visuella variationer av en komponent.

1. HTML-utvecklaren får HTML-utdata av komponenterna och de visuella variationer som ska implementeras.

1. HTML-utvecklaren definierar de CSS-klasser som motsvarar varje visuell variation och som ska infogas i elementet som omsluter komponenterna.

1. HTML-utvecklaren implementerar motsvarande CSS-kod (och eventuellt JS-kod) för alla visuella variationer så att de ser ut som de definierats.

1. AEM-utvecklaren placerar angiven CSS (och valfri JS) i ett [klientbibliotek](/help/sites-developing/clientlibs.md) och distribuerar det.

1. AEM utvecklare eller mallskapare konfigurerar sidmallarna och redigerar profilen för varje formaterad komponent, lägger till definierade CSS-klasser, ger användarvänliga namn för varje format och anger vilka format som kan kombineras.

1. Sidförfattaren av AEM kan sedan välja de formgivna formaten i sidredigeraren via formatmenyn i komponentens verktygsfält.

Observera att endast de tre sista stegen faktiskt utförs i AEM. Detta innebär att all utveckling av nödvändig CSS och JavaScript kan göras utan AEM.

För att kunna implementera formaten behöver du bara distribuera dem på AEM och välja dem bland komponenterna i de önskade mallarna.

Följande diagram visar arkitekturen i Style System.

![aem-style-system](assets/aem-style-system.png)

## Använd {#use}

För att demonstrera funktionen använder vi [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)-implementeringen av kärnkomponentens [title-komponent](https://www.adobe.com/go/aem_cmp_title_v2) som exempel.

I följande avsnitt [Som innehållsförfattare](#as-a-content-author) och [ som mallskapare](#as-a-template-author) beskrivs hur du testar funktionaliteten i Style System med Style System of WKND.

Om du vill använda Style System för dina egna komponenter gör du följande:

1. Installera CSS som klientbibliotek enligt beskrivningen i avsnittet [Översikt](#overview).
1. Konfigurera de CSS-klasser som du vill göra tillgängliga för innehållsförfattarna enligt beskrivningen i avsnittet [Som mallförfattare](#as-a-template-author).
1. Innehållsförfattare kan sedan använda de format som beskrivs i avsnittet [Som innehållsförfattare](#as-a-content-author).

### Som innehållsförfattare {#as-a-content-author}

1. När du har installerat WKND-projektet går du till WKND:s engelska huvudsida på `http://<host>:<port>/sites.html/content/wknd/language-masters/en` och redigerar sidan.
1. Markera en **Title**-komponent längre ned på sidan

   ![Formatsystem för författaren](assets/style-system-author.png)

1. Klicka på knappen **Stilar** i verktygsfältet för komponenten **Lista** för att öppna stilmenyn och ändra komponentens utseende.

   ![Markera format](assets/style-system-author2.png)

   >[!NOTE]
   >
   >I det här exemplet utesluter formaten **Färger** (**Svart**, **Vit** och **Grå**) varandra, medan alternativen **Format** (**Understruken**, **Högerjustera** och **Mini Spacing**}) kan kombineras. Detta kan [konfigureras i mallen som mallskapare](#as-a-template-author).

### Som mallskapare {#as-a-template-author}

1. När du redigerar WKND:s engelska huvudsida på `http://<host>:<port>/sites.html/content/wknd/language-masters/en` redigerar du sidans mall via **Sidinformation > Redigera mall**.

   ![Redigera mall](assets/style-system-edit-template.png)

1. Redigera principen för komponenten **Title** genom att trycka på eller klicka på knappen **Policy** för komponenten.

   ![Redigera princip](assets/style-system-edit-policy.png)

1. På fliken Format i egenskaperna kan du se hur formaten har konfigurerats.

   ![Redigera egenskaper](assets/style-system-properties.png)

   * **Gruppnamn:** Format kan grupperas tillsammans på den formatmeny som innehållsförfattaren kommer att se när komponentens format konfigureras.
   * **Format kan kombineras:** Tillåter att flera format i den gruppen markeras samtidigt.
   * **Formatnamn:** Beskrivning av det format som ska visas för innehållsförfattaren när komponentens format konfigureras.
   * **CSS-klasser:** Det faktiska namnet på CSS-klassen som är associerad med formatet.

   Använd draghandtagen för att ordna gruppernas och gruppernas inbördes ordning. Använd ikonerna för att lägga till eller ta bort för att lägga till eller ta bort grupper eller format i grupper.

>[!CAUTION]
>
>CSS-klasserna (och eventuella nödvändiga JavaScript) som konfigurerats som formategenskaper för en komponents policy måste distribueras som [klientbibliotek](/help/sites-developing/clientlibs.md) för att fungera.

## Inställningar {#setup}

Core Components version 2 och senare är helt aktiverade för att utnyttja Style System och kräver ingen ytterligare konfiguration.

Följande steg är bara nödvändiga för att aktivera Style System för dina egna anpassade komponenter eller för att [aktivera fliken Format i dialogrutan Redigera.](#enable-styles-tab-edit)

### Aktivera fliken Format i designdialogrutan {#enable-styles-tab-design}

För att en komponent ska kunna fungera med AEM Style System och visa formatfliken i sin designdialogruta måste komponentutvecklaren inkludera formatfliken med följande inställningar för komponenten:

* `path = "/mnt/overlay/cq/gui/components/authoring/dialog/style/tab_design/styletab"`
* `sling:resourceType = "granite/ui/components/coral/foundation/include"`

När komponenten är konfigurerad infogas de format som har konfigurerats av sidförfattarna automatiskt av AEM i dekorationselementet som AEM automatiskt omsluter runt varje redigerbar komponent. Själva komponenten behöver inte göra något annat för att detta ska hända.

### Aktivera fliken Format i dialogrutan Redigera {#enable-styles-tab-edit}

Nu finns det en flik för valfria format i dialogrutan Redigera. Till skillnad från fliken Design Dialog är fliken i dialogrutan Redigera inte nödvändig för att formatsystemet ska fungera, men den är ett valfritt gränssnitt där en innehållsförfattare kan ange format.

Fliken för redigeringsdialogrutan kan läggas in på ungefär samma sätt som fliken för designdialogrutan:

* `path = "/mnt/overlay/cq/gui/components/authoring/dialog/style/tab_edit/styletab"`
* `sling:resourceType = "granite/ui/components/coral/foundation/include"`

>[!NOTE]
>
>Fliken Format i dialogrutan Redigera är inte aktiverad som standard.

### Format med elementnamn {#styles-with-element-names}

En utvecklare kan också konfigurera en lista med tillåtna elementnamn för format i komponenten med strängmatrisegenskapen `cq:styleElements`. På fliken Format i profilen i designdialogrutan kan mallskaparen också välja ett elementnamn som ska anges för varje format. Då anges elementnamnet för elementet wrapper.

Den här egenskapen har angetts för noden `cq:Component`. Till exempel:

* `/apps/<yoursite>/components/content/list@cq:styleElements=[div,section,span]`

>[!CAUTION]
>
>Undvik att definiera elementnamn för format som kan kombineras. När flera elementnamn definieras är prioritetsordningen:
>
>1. HTML har företräde framför allt: `data-sly-resource="${'path/to/resource' @ decorationTagName='span'}`
>1. När det sedan finns flera aktiva format används det första formatet i listan med format som är konfigurerade i komponentens profil.
>1. Slutligen betraktas komponentens `cq:htmlTag`/ `cq:tagName` som ett reservvärde.
>

Den här möjligheten att definiera formatnamn är användbar för mycket generiska komponenter, som Layoutbehållaren eller komponenten Innehållsfragment, för att ge dem ytterligare innebörd.

Den tillåter till exempel att en Layout Container får semantik som `<main>`, `<aside>`, `<nav>` och så vidare.
