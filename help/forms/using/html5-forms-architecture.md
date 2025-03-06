---
title: Arkitektur för HTML5-formulär
description: HTML5-formulär distribueras som ett paket i den inbäddade AEM-instansen och visar funktionaliteten som REST-slutpunkt över HTTP/S med hjälp av RESTful Apache Sling-arkitekturen.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: e57d51de-9d98-4b20-8180-22fa81fad4fd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1976'
ht-degree: 0%

---

# Arkitektur för HTML5-formulär{#architecture-of-html-forms}

## Arkitektur {#architecture}

HTML5-formulärfunktionen distribueras som ett paket i den inbäddade AEM-instansen och visas som en REST-slutpunkt över HTTP/S med RESTful [Apache Sling Architecture](https://sling.apache.org/).

![02-aem-forms-architecture_large](assets/02-aem-forms-architecture_large.jpg)

### Använda Sling Framework {#using-sling-framework}

[Apache Sling](https://sling.apache.org/) är resurscentrerad. Den använder en begärande URL för att först matcha resursen. Varje resurs har en **sling:resourceType**- (eller **sling:resourceSuperType**)-egenskap. Baserat på den här egenskapen, förfrågningsmetoden och egenskaperna för begärande-URL:en, väljs sedan ett sling-skript för att hantera begäran. Det här snedstrecksskriptet kan vara en JSP eller en servlet. För HTML5-formulär fungerar **Profile**-noder som snedställningsresurser och **Profile Renderer** fungerar som snedskriftsskript som hanterar begäran om att återge mobilformuläret med en viss profil. En **profilrenderare** är en JSP som läser parametrar från en begäran och anropar Forms OSGi-tjänsten.

Mer information om REST-slutpunkten och parametrar för begäran som stöds finns i [Återge formulärmall](/help/forms/using/rendering-form-template.md).

När en användare gör en begäran från en klientenhet som webbläsaren iOS eller Android™, löser Sling först profilnoden baserat på den begärda URL:en. Från den här profilnoden läser den **sling:resourceSuperType** och **sling:resourceType** för att identifiera alla tillgängliga skript som kan hantera den här formuläråtergivningsbegäran. Sedan används väljare för Sling-begäran tillsammans med begärandemetoden för att identifiera det skript som lämpar sig bäst för att hantera denna begäran. När begäran når en profilåtergivnings-JSP anropar JSP:n Forms OSGi-tjänsten.

Mer information om Sling-skriptupplösningen finns i [AEM Sling Cheat Sheet](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=en) eller [Apache Sling Url-nedbrytning](https://sling.apache.org/documentation/the-sling-engine/url-decomposition.html).

#### Vanligt anropsflöde för formulärbearbetning {#typical-form-processing-call-flow}

HTML5-formulär cachelagrar alla mellanliggande objekt som krävs för att bearbeta (återge eller skicka) ett formulär vid den första begäran. Objekten som är beroende av data cachelagras inte eftersom sådana objekt troligtvis ändras.

Mobilformulär upprätthåller två olika cachenivåer, PreRender-cache och Render Cache. Cacheminnet för preRender innehåller alla fragment och bilder i en löst mall och Render-cachen innehåller återgivet innehåll som HTML.

![HTML5-formulärarbetsflöde](assets/cacheworkflow.png)

HTML5 - arbetsflöde för blanketter

HTML5-formulär cache-lagrar inte mallar som saknar referenser till fragment och bilder. Om HTML5-formulär tar längre tid än normalt kontrollerar du om det finns referenser och varningar som saknas i serverloggarna. Se även till att objektets maximala storlek inte nås.

Forms OSGi-tjänsten bearbetar en begäran i två steg:

* **Layout och ursprunglig formulärtillståndsgenerering**: Återgivningstjänsten för Forms OSGi anropar Forms Cache-komponenten för att avgöra om formuläret redan har cache-lagrats och inte har ogiltigförklarats. Om formuläret är cache-lagrat och giltigt, skickas den genererade HTML från cachen. Om formuläret blir ogiltigt genererar Forms OSGi-renderingstjänsten den första formulärlayouten och formulärläget i XML-format. Denna XML konverteras till HTML layout och JSON-formulärtillstånd (Initial JSON Form State) av Forms OSGi-tjänsten och cachelagras sedan för efterföljande förfrågningar.
* **Förifylld Forms**: Om en användare begär formulär med förifyllda data vid återgivningen anropar Forms OSGi-renderingstjänsten Forms tjänstbehållare och genererar ett nytt formulärtillstånd med sammanfogade data. Eftersom layouten redan genereras i ovanstående steg är anropet snabbare än det första anropet. Detta anrop utför bara datasammanfogningen och kör skripten på data.

Om det finns någon uppdatering i formuläret eller något av resurserna som används i formuläret, upptäcks den av formulärets cachekomponent och cachen för det aktuella formuläret ogiltigförklaras. När Forms OSGi-tjänsten har slutfört bearbetningen lägger Profile Renderer jsp till JavaScript biblioteksreferenser och formatering i det här formuläret och returnerar svaret till klienten. En vanlig webbserver som [Apache](https://httpd.apache.org/) kan användas här med HTML-komprimering aktiverat. En webbserver skulle minska svarsstorleken, nätverkstrafiken och den tid som krävs för att strömma data mellan servern och klientdatorn avsevärt.

När en användare skickar formuläret, skickar webbläsaren formulärets status i JSON-format till [tjänstproxyn](../../forms/using/service-proxy.md). Sedan genererar tjänstproxyn en data-XML med JSON-data och skickar data-XML till slutpunkten.

## Komponenter {#components}

Du behöver AEM Forms tilläggspaket för att kunna aktivera HTML 5-formulär. Mer information om hur du installerar AEM Forms tilläggspaket finns i [Installera och konfigurera AEM Forms](../../forms/using/installing-configuring-aem-forms-osgi.md).

### OSGi Components (adobe-lc-forms-core.jar) {#osgi-components-adobe-lc-forms-core-jar}

**Adobe XFA Forms Renderer (com.adobe.livecycle.adobe-lc-forms-core)** är visningsnamnet för HTML5-formulären OSGi-paketet när det visas i Bundle-vyn i Felix Admin Console (https://[host]:[port]/system/console/bundles).

Den här komponenten innehåller OSGi-komponenter för rendering, cachehantering och konfigurationsinställningar.

#### Forms OSGi Service {#forms-osgi-service}

Den här OSGi-tjänsten innehåller logiken för att återge en XDP som HTML och hanterar överföringen av ett formulär för att generera data-XML. Den här tjänsten använder Forms tjänstbehållare. Forms tjänstbehållare anropar den interna komponenten `XMLFormService.exe` som utför bearbetningen.

Om en återgivningsbegäran tas emot anropar den här komponenten Forms tjänstbehållare för att generera layout- och tillståndsinformation som bearbetas ytterligare för att generera DOM-tillstånd för HTML och JSON.

Den här komponenten ansvarar också för att generera data-XML från det inskickade formulärtillståndet JSON.

#### Cachekomponent {#cache-component}

HTML5-formulär använder cachelagring för att optimera dataflödet och svarstiden. Du kan konfigurera cachetjänstens nivå för att finjustera avvägningen mellan prestanda och utrymmesanvändning.

<table>
 <tbody>
  <tr>
   <th>Cachestrategi</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>Ingen</td>
   <td>Cachelagra inte artefakter <br /> </td>
  </tr>
  <tr>
   <td>Konservativ</td>
   <td>Cachelagra endast mellanliggande artefakter som genereras före återgivningen av formuläret, som en mall som innehåller textbundna fragment och bilder</td>
  </tr>
  <tr>
   <td>Aggressiv</td>
   <td>Cachelagra återgivet HTML-innehåll <br /> Cachelagra alla artefakter som cachelagrats på den konservativa nivån.<br /> <strong>Obs!</strong> Den här strategin ger bästa prestanda men kräver mer minne för att lagra cachelagrade artefakter.</td>
  </tr>
 </tbody>
</table>

HTML5-formulär utför cachelagring i minnet med LRU-strategi. Om cachestrategin är inställd på Ingen skapas ingen cache och befintliga cachedata rensas, om det finns några. Förutom cachelagringsstrategin kan du även konfigurera den totala cachestorleken i minnet, vilket kan bidra till att få den maximala gränsen för cachestorleken och om den går längre kommer LRU-läget att frigöra cacheresurser.

>[!NOTE]
>
>Cacheminnet i minnet delas inte mellan klusternoder.

#### Konfigurationstjänst {#configuration-service}

Med konfigurationstjänsten kan du justera konfigurationsparametrar och cacheinställningar för HTML5-formulär.

Om du vill uppdatera de här inställningarna går du till CQ Felix Admin Console (finns på https://&lt;&#39;[server]:[port]&#39;/system/console/configMgr), söker efter och väljer Mobile Forms Configuration.

Du kan konfigurera cachestorleken eller inaktivera cacheminnet med hjälp av konfigurationstjänsten. Du kan även aktivera felsökning med parametern Felsökningsalternativ. Mer information om felsökning av formulär finns i [Felsöka HTML5-formulär](/help/forms/using/debug.md).

### Körningskomponenter (adobe-lc-forms-runtime-pkg.zip) {#runtime-components-adobe-lc-forms-runtime-pkg-zip}

Runtime Package innehåller de klientbibliotek som används för att återge HTML-formulär.

**Viktiga komponenter som är tillgängliga som en del av körtidspaketet:**

#### Skriptmotor {#scripting-engine}

Implementeringen av Adobe XFA stöder två typer av skriptspråk för att möjliggöra användardefinierad logikkörning i formulär: JavaScript och FormCalc.

Skriptmotorn i HTML Forms är skriven i JavaScript för att stödja XFA-skriptnings-API:t på båda dessa språk.

Vid återgivning översätts (och cachelagras) FormCalc-skriptet till JavaScript på servern, som är genomskinligt för användaren eller designern.

Den här skriptmotorn använder en del av funktionen i ECMAScript5 som Object.defineProperty. Motorn/biblioteket levereras som CQ-klientbibliotek med kategorinamnet **xfaforms.profile**. Den innehåller även **FormBridge API** som gör att externa portaler eller appar kan interagera med formuläret. Med FormBridge kan ett externt program programmässigt dölja vissa element, hämta eller ange deras värden eller ändra deras attribut.

Mer information finns i artikeln [Form Bridge](/help/forms/using/form-bridge-apis.md).

#### Layoutmotor {#layout-engine}

Layouten och den visuella aspekten av HTML5-formulären baseras på funktionerna SVG 1.1, jQuery, BackBone och CSS3. Det ursprungliga utseendet för ett formulär genereras och cachelagras på servern. Den inledande layouten och eventuella ytterligare ändringar av formulärlayouten hanteras på klienten. För att uppnå detta innehåller körtidspaketet en layoutmotor som är skriven i JavaScript och baserad på jQuery/Backbone. Den här motorn hanterar allt dynamiskt beteende, som Lägg till/ta bort repeterbara instanser och Utbyggbar objektlayout. Den här layoutmotorn återger ett formulär en sida i taget. Inledningsvis visar en användare bara en sida och den vågräta rullningslisten bara första sidan. När en användare rullar nedåt börjar dock nästa sida återgivningen. Den här återgivningen sida för sida minskar den tid som krävs för att återge den första sidan i en webbläsare och förbättrar formulärets upplevda prestanda. Motorn/biblioteket är en del av CQ-klientbiblioteket med kategorinamnet **xfaforms.profile**.

Layoutmotorn innehåller också en uppsättning widgetar som används för att hämta värdet för formulärfält från en användare. De här widgetarna är modellerade som [jQuery-gränssnittswidgetar](https://api.jqueryui.com/jQuery.widget/) som implementerar vissa ytterligare kontrakt som ska fungera sömlöst med layoutmotorn.

Mer information om widgetar och motsvarande kontrakt finns i [Anpassade widgetar för HTML5-formulär](/help/forms/using/introduction-widgets.md).

#### Stilar {#styling}

Det format som är associerat med HTML-elementen läggs till antingen textbundet eller baserat på inbäddat CSS-block. Vissa vanliga format som inte är beroende av formulär är en del av CQ Client Lib med kategorinamnet xfaforms.profile.

Förutom standardegenskaper för format innehåller varje formulärelement även vissa CSS-klasser baserade på elementtyp, namn och andra egenskaper. Med hjälp av dessa klasser kan du omformatera element genom att ange deras egen CSS.

Mer information om standardformat och klasser finns i [Introduktion till format](/help/forms/using/css-styles.md).

#### Serverskript och webbtjänster {#server-side-script-and-web-services}

Skript som har markerats för att köras på servern eller som har markerats för att anropa en webbtjänst (oavsett var den har markerats för att köras) körs alltid på servern.

Klientskriptmotorn:

1. Gör ett synkront anrop till servern som skickar det aktuella formulärtillståndet i form av JSON
1. Kör skriptet eller webbtjänsten på servern
1. Skapar ett nytt JSON-läge
1. Sammanfogar det nya JSON-läget på klienten när svaret returneras.

#### Resurspaket för lokalisering {#localization-resource-bundles}

HTML5-formulär har stöd för italienska (it), spanska (es), brasiliansk portugisiska (pt_BR), förenklad kinesiska (zh_CN), traditionell kinesiska (endast begränsat stöd) (zh_TW), koreanska (ko_KR), engelska (en_US), franska (fr_FR), tyska (de_DE) och japanska (ja). Beroende på vilket språkområde som tas emot i begärandehuvudet skickas motsvarande resurspaket till klienten. Det här resurspaketet läggs till i profil-JSP som ett CQ-klientbibliotek med kategorinamnet **xfaforms.I18N**. Du kan åsidosätta logiken för att hämta språkpaketet i profilen.

### Sling Components (adobe-lc-forms-content-pkg.zip) {#sling-components-adobe-lc-forms-content-pkg-zip}

Sling-paketet innehåller innehåll som är relaterat till profiler och profilåtergivning.

#### Profiler {#profiles}

Profiler är resursnoder som representerar ett formulär eller Forms-familjen. På CQ-nivå är dessa profiler JCR-noder. Noderna finns i mappen **/content** i JCR-databasen och kan finnas i alla undermappar i mappen **/content**.

#### Profilåtergivare {#profile-renderers}

Profilnoden har egenskapen **sling:resourceSuperType** med värdet **xfaforms/profile**. Den här egenskapen skickar begäranden internt till snedställningsskriptet för profilnoder i mappen **/libs/xfaforms/profile**. Dessa skript är JSP-sidor, som är behållare för att sätta ihop HTML-formulären och obligatoriska JS/CSS-artefakter. Sidorna innehåller referenser till:

* **xfaforms.I18N.&lt;locale>**: Det här biblioteket innehåller lokaliserade data.
* **xfaforms.profile**: Det här biblioteket innehåller implementering för XFA-skriptnings- och layoutmotorn.

Dessa bibliotek är modellerade som CQ Client Libraries, som utnyttjar automatisk sammanfogning, miniatyrbildnings- och komprimeringsfunktioner i CQ framework JavaScript Libraries.
Mer information om CQ-klientbibliotek finns i [CQ Clientlib-dokumentation](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=en).

Så som beskrivs ovan anropar profilåtergivaren JSP Forms Service via en sling include. Denna JSP anger också olika felsökningsalternativ baserat på administratörskonfigurationen eller frågeparametrarna.

Med HTML5-formulär kan utvecklare skapa profil- och profilåtergivning för att anpassa formulärens utseende. Med HTML-formulär kan utvecklare till exempel integrera formulär på en panel eller &lt;div> i en befintlig HTML-portal.
Mer information om hur du skapar anpassade profiler finns i [Skapa en anpassad profil](/help/forms/using/custom-profile.md).
