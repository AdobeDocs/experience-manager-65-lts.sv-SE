---
title: Introduktion till anpassning av AEM-formulärarbetsyta
description: En snabb introduktion med konceptuell och teknisk information för att anpassa LiveCycle AEM Forms-arbetsytan för processhantering.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: Admin, User, Developer
exl-id: 39be83b0-c003-4e6c-baca-95166f654bc7
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1752'
ht-degree: 0%

---

# Introduktion till anpassning av AEM-formulärarbetsyta{#introduction-to-customizing-aem-form-workspace}

AEM blankettarbetsyta har funktioner för att ändra presentationssemantik och funktionalitet i gränssnittet. Typerna av anpassningar för att ändra format, layout, formatering, varumärke och grundfunktioner beskrivs nedan.

![cu_customized_workspace_example](assets/cu_customized_workspace_example.png)

Ett exempel på en anpassad arbetsyta

## Typer av anpassningar {#types-of-customizations}

AEM Forms arbetsyta har stöd för en mängd anpassningar för att uppdatera användargränssnittets layout, utseende, funktioner och mycket annat. Anpassningarna innefattar att uppdatera ett eller flera av följande:

* Användargränssnittets utseende
* Funktioner som använder semantiska anpassningar
* Återanvända HTML-komponenter i andra program

### Ändringar i användargränssnittet {#user-interface-changes}

Du kan ändra utseende, layout och andra presentationssemantik för AEM Forms-arbetsytan. Ändra arbetsyta genom att anpassa CSS-, HTML- och JavaScript™-filer. Alla standardfiler finns i standardinstallationen.

De vanligaste stegen beskrivs i [Allmänna steg för anpassning av AEM Forms-arbetsytan](../../forms/using/generic-steps-html-workspace-customization.md). Specifika exempel på sådana anpassningar, inklusive detaljerade steg, finns i de relaterade artiklarna i slutet av den här artikeln.

#### Förstå formatmallen {#understanding-the-style-sheet}

Innan du anpassar arbetsytan bör du bekanta dig med den standardformatmall som finns i AEM Forms på /libs/ws/css/style.css.

Om du vill anpassa arbetsytan bör du bekanta dig med den befintliga formatmallen, style.css, i mappen /libs/ws/css. Nedan beskrivs några viktiga komponenter.

<table>
 <tbody>
  <tr>
   <th><p>CSS-element</p> </th>
   <th><p>Användargränssnittskomponenten har ändrats</p> </th>
  </tr>
  <tr>
   <td><p>#header</p> </td>
   <td><p>Sidhuvud på arbetsytan i AEM Forms</p> </td>
  </tr>
  <tr>
   <td><p>.categoryList</p> </td>
   <td><p>Kategorilista</p> </td>
  </tr>
  <tr>
   <td><p>.categoryList.header</p> </td>
   <td><p>Rubrik för kategorilista</p> </td>
  </tr>
  <tr>
   <td><p>.categories, .filters</p> </td>
   <td><p>Avstånd nedanför kategorilistan</p> </td>
  </tr>
  <tr>
   <td><p>.category, .filter</p> </td>
   <td><p>Kategori</p> </td>
  </tr>
  <tr>
   <td><p>.category:hover, .category.selected, .filter:hover, .filter.selected</p> </td>
   <td><p>Markerad kategori och typ av hovring över-kategori</p> </td>
  </tr>
  <tr>
   <td><p>categoryListBar.tool, categoryListBar.content</p> </td>
   <td><p>Startprocesssida (stängd kategorilista)</p> </td>
  </tr>
  <tr>
   <td><p>filterListBar.tool, filterListBar.content</p> </td>
   <td><p>Att göra-sida (sluten filterlista)</p> </td>
  </tr>
  <tr>
   <td><p>processNameListBar.tool, processNameListBar.content</p> </td>
   <td><p>Spårningssida (namnlista för stängd process)</p> </td>
  </tr>
  <tr>
   <td><p>.startPointList, .tasklist</p> </td>
   <td><p>Startpunktslistan eller uppgiftslistan</p> </td>
  </tr>
  <tr>
   <td><p>.startPointList.header, .tasklist.header</p> </td>
   <td><p>Huvudet för en startpunktslista eller en uppgiftslista</p> </td>
  </tr>
  <tr>
   <td><p>.startpoint.selected, .task.selected</p> </td>
   <td><p>Den valda startpunkten eller uppgiften</p> </td>
  </tr>
  <tr>
   <td><p>.startpoint.selected.description, .task.selected.description</p> </td>
   <td><p>Beskrivning av den valda startpunkten eller uppgiften</p> </td>
  </tr>
  <tr>
   <td><p>#taskarea</p> </td>
   <td><p>Åtgärdsområdet</p> </td>
  </tr>
  <tr>
   <td><p>#header.dropdown</p> </td>
   <td><p>Listrutan Användare i rubriken</p> </td>
  </tr>
  <tr>
   <td><p>.sortDrop dd ul</p> </td>
   <td><p>Sorteringsaktivitetslistruta</p> </td>
  </tr>
 </tbody>
</table>

#### CSS {#css}

Utseendet på arbetsytan i AEM Forms lånar utseendet från en CSS. Genom att anpassa CSS kan du ändra presentationssemantik för arbetsytan, som teckensnitt, färger, varumärke och layout.

De översta stegen för CSS-anpassning är:

* Skapa en CSS-fil.
* Lägg till formatobjekt i denna CSS. Mer information finns i Förstå CSS-format.
* Uppdatera referenser i `html.jsp`.

Exakt hur du gör dessa anpassningar finns i [Allmänna steg för anpassning av AEM Forms-arbetsytan](../../forms/using/generic-steps-html-workspace-customization.md). CSS-filen som levereras med arbetsytan i AEM Forms finns på /libs/ws/css/. Använd [Leveranspaket](../../forms/using/introduction-customizing-html-workspace.md#p-crx-package-p) för CSS-relaterade anpassningar. Specifika exempel på CSS-relaterade anpassningar finns i de relaterade hjälpavsnitten i slutet av den här artikeln.

#### Bild {#image}

Du kan anpassa AEM Forms-arbetsytan för att lägga till olika användare eller lägga till din organisations logotyp. Använd [Leveranspaket](../../forms/using/introduction-customizing-html-workspace.md#p-crx-package-p) för dessa anpassningar.

De översta stegen för anpassning av bilderna är:

* Installera och konfigurera WebDAV.
* Lägg till nya bilder.
* Lägg till nya format som motsvarar de nya bilderna.
* Länka till den nya CSS-filen i filen `html.jsp`.

Följ de [allmänna stegen för anpassning av arbetsytan i AEM Forms](../../forms/using/generic-steps-html-workspace-customization.md) för att komma igång med att anpassa bilderna i AEM Forms. Specifika exempel på bildrelaterade anpassningar finns i de relaterade hjälpavsnitten i slutet av artikeln.

#### HTML-mall {#html-template}

Med HTML-mallar kan du definiera utseendet och layouten för arbetsytans användargränssnitt. Genom att uppdatera HTML standardmallar kan du anpassa standardanvändargränssnittet för layouten.

De viktigaste stegen för anpassning av HTML-mallen är:

* Skapa kopior av de standardfiler som behövs i en mapp som skapats av användaren.
* Lägg till nya mallar i en användardefinierad mapp.
* Gör relevanta uppdateringar av de kopierade filerna, som sökvägen till den nya mallen.

Specifika exempel på sådana anpassningar finns i hjälpavsnitten i slutet av artikeln. Välj mellan [Leveranspaket](../../forms/using/introduction-customizing-html-workspace.md#p-crx-package-p) eller [Dev-paket](../../forms/using/introduction-customizing-html-workspace.md#p-crx-package-p), beroende på vilken mall som ska anpassas.

### Semantiska ändringar {#semantic-changes}

Ändra JavaScript källkod om du vill ändra funktionaliteten för arbetsytan i AEM Forms. Ändringarna i kärnfunktionen markeras som Semantiska ändringar. Du kan ändra modeller, vyer och mallar som ingår i källkoden för AEM Forms-arbetsytan.

De översta stegen för att göra semantiska ändringar för att ändra funktionaliteten i AEM Forms-arbetsytan är:

* Skapa kopior av standardfilerna i en mapp som skapas av användaren.
* Lägg till nya modeller och vyer i den användardefinierade mappen.
* Gör relevanta uppdateringar som att uppdatera sökvägar för nya modeller och vyer i JavaScript standardfiler.
* Minimera paketet för att optimera prestandan.

Mer konceptuell information om komponenterna som är en del av källkoden finns i [Beskrivning av återanvändbara komponenter](/help/forms/using/description-reusable-components.md). Använd Dev Package för dessa anpassningar.

### Återanvändbara komponenter {#reusable-components}

Eftersom AEM Forms arbetsyta är ett komponentbaserat program kan det enkelt anpassas och återanvändas. Du kan enkelt integrera arbetsytekomponenterna med dina webbprogram.

Mer konceptuell information finns i [Beskrivning av återanvändbara komponenter](/help/forms/using/description-reusable-components.md) och instruktioner om hur du använder komponenterna finns i [Integrera AEM Forms-arbetsytekomponenter i webbprogram](/help/forms/using/description-reusable-components.md).

## Skapar AEM Forms arbetsytekod {#building-html-workspace-code}

### SDK package {#sdk-package}

Paketet innehåller källkoden för AEM Forms-arbetsytan. Paketet är tillgängligt på `[LC root]\sdk\html-workspace\adobe-lc-workspace-src.zip`.

Det är främst avsett för anpassningar, eftersom det ger möjlighet att generera:

* CRX-paket för Ship-, Debug- och Dev-profiler (anges nedan i [CRX-paket](../../forms/using/introduction-customizing-html-workspace.md#p-crx-package-p)).
* Minimerad version av anpassad kod (för semantiska ändringar).

#### WS-innehåll {#ws-content}

* client-pkg:

   * src - Innehåller artefakter som behövs för att skapa CRX-noder.
   * pom.xml - Skript för att skapa distributionspaket för olika profiler WS-Deploy Package

* client-html:

   * assembly - Innehåller zip.xml som används av skript för att skapa AEM Forms-arbetsytan SDK.
   * src/main/webapp -

      * css - Innehåller formatmallar för AEM Forms-arbetsytan.
      * bilder - Innehåller bilder som används på arbetsytan i AEM Forms.
      * js:

         * libs - Innehåller alla tredjepartsbibliotek som används i AEM Forms arbetsyta.
         * licenser - Innehåller licenser för HTML- och JS-filer och kod som prefix för dessa licenser till respektive källfil.
         * minifier - Används för kombination, minifiering och uppgradering av anpassad JavaScript-kod.
         * resourcejs_optimizer - Används för kombination, minifiering och uglifiering av JavaScript-källan.
         * resource_generator - Används för att generera register.js och modelcontroller.path.js.
         * runtime:

            * initierare - Innehåller initializer.js som används för att initiera vyer och modeller för ryggrad som används på arbetsytan i AEM Forms.
            * modeller - Innehåller stammodeller av alla komponenter som finns på arbetsytan i AEM Forms.
            * vägar - Innehåller JavaScript-filer och HTML-filer som läser in startprocess, att göra-och-spåra samt inställningar på arbetsytan i AEM Forms.
            * services - Innehåller service.js som används i AEM Forms arbetsyta. Alla serveranrop görs via service.js.
            * mallar - Innehåller alla mallar, det vill säga HTML-filer av alla vyer på arbetsytan i AEM Forms.
            * util - Innehåller alla verktygsfiler (javascript) som används på arbetsytan i AEM Forms.
            * vyer - Innehåller ryggradsvyer av alla komponenter på arbetsytan i AEM Forms.

         * main.js
         * router.js

      * libs/ws: pdf.html och pluginPing.pdf används för att läsa in PDF forms på arbetsytan i AEM Forms och WSNextAdapter.swf används för att läsa in SWF-formulär och stödlinjer på arbetsytan i AEM Forms.
      * nationella inställningar:

         * de-DE - Innehåller translation.json för tyska.
         * en-US - Innehåller translation.json för engelska.
         * fr-FR - Innehåller translation.json för franska.
         * ja-JP - Innehåller translation.json för japanska.
         * html.jsp - Innehåller kod för att ta reda på webbläsarens aktuella språkområde.

      * html.jsp
      * GET.jsp

### CRX Package {#crx-package}

CRX-paketet kan användas i CRX™-databasen. Den är tillgänglig på `[LC root]\crx-repository\install\adobe-lc-workspace-pkg.zip`.

Paketet kan byggas med de tre profiler som beskrivs nedan.

| **Profil** | **Beskrivning** | **Användning** |
|---|---|---|
| Leveransprofil | Den här profilen skapar ett CRX-paket med minsta möjliga storlek med hjälp av miniatyrbilder. Det här paketet är mest effektivt. Alla JavaScript™-filer kombineras och mineras i en enda JS-fil. | Använd den här profilen när inga ytterligare semantiska ändringar krävs i JS-filer. |
| Felsökningsprofil | Den här profilen skapar ett måttligt effektivt CRX-paket. Paketets storlek är något större än det paket som skapats med Ship-profilen. Det här paketet innehåller de flesta JavaScript-filer kombinerade till en enda JS-fil. | Använd den här profilen för felsökning. |
| Dev-profil | Den här profilen skapar ett CRX-paket med största möjliga storlek. Alla JavaScript-filer är tillgängliga separat, som de är i SDK-paket. | Använd den här profilen när du inkluderar semantiska ändringar. |

#### Leveransprofil {#ship-profile}

#### Kommando {#command}

* mvn clean -P Ship install on client-pkg folder of Source package shipping to client.
* Kommandokörning av Ship-profil fungerar bara på en 64-bitars JVM.

#### WS-innehåll {#ws-content-1}

* css - Innehåller style.css, ie.css och jquery-ui.css.
* bilder - Innehåller alla bilder.
* js:

   * libs:

      * require - Contains require.js.
      * jqueryui - Innehåller jquery.ui.datepicker.ja.js.

   * runtime:

      * mallar - Innehåller alla mallar, det vill säga HTML-filer för alla komponenter på AEM Forms arbetsyta.

   * main.js (kombinerat, minifierat och uppgraderat).
   * registry.js

* libs:

   * ws - Innehåller pluginPing.pdf, pdf.html och WSNextAdapter.swf.

* Språk - innehåller .content.xml.
* nationella inställningar:

   * de-DE - Innehåller translation.json för tyska.
   * en-US - Innehåller translation.json för engelska.
   * fr-FR - Innehåller translation.json för franska.
   * ja-JP - Innehåller translation.json för japanska.
   * html.jsp - Innehåller kod för att ta reda på webbläsarens aktuella språkområde.

* Index - innehåller .content.xml
* profile - Innehåller offline.jsp.
* GET.jsp
* html.jsp
* content.xml
* _rep_policy.xml

#### Felsökningsprofil {#debug-profile}

#### Kommando {#command-1}

* mvn ren -P Felsökningsinstallation på klient-pkg
* Kommandokörning av felsökningsprofiler fungerar bara på 64-bitars JVM.

#### WS-innehåll {#ws-content-2}

* css - Innehåller style.css, ie.css och jqueri-ui.css.
* bilder - Innehåller alla bilder.
* js:

   * libs:

      * require - Contains require.js.
      * jqueryui - Innehåller jquery.ui.datepicker.ja.js.

   * runtime:

      * mallar - Innehåller alla mallar, det vill säga HTML-filer för alla komponenter på AEM Forms arbetsyta.

   * main.js (kombinerat).
   * registry.js

* libs:

   * ws - Innehåller pluginPing.pdf, pdf.html och WSNextAdapter.swf.

* Språk - innehåller .content.xml.
* nationella inställningar:

   * de-DE - Innehåller translation.json för tyska.
   * en-US - Innehåller translation.json för engelska.
   * fr-FR - Innehåller translation.json för franska.
   * ja-JP - Innehåller translation.json för japanska.
   * html.jsp - Innehåller kod för att ta reda på webbläsarens aktuella språkområde.

* Index - innehåller .content.xml
* profile - Innehåller offline.jsp.
* GET.jsp
* html.jsp
* content.xml
* _rep_policy.xml

#### Dev-profil {#dev-profile}

#### Kommando {#command-2}

mvn clean -P Dev install on client-pkg

#### WS-innehåll {#ws-content-3}

* css - Innehåller style.css, ie.css och jqueri-ui.css.
* bilder - Innehåller alla bilder.
* js:

   * libs - Innehåller alla bibliotek som används i AEM Forms arbetsyta.
   * require - Contains require.js
   * jqueryui - innehåller jquery.ui.datepicker.ja.js
   * runtime:

      * initializer - Innehåller initializer.js och modelcontroller.path.js.
      * modeller - Innehåller modeller av alla komponenter på arbetsytan i AEM Forms.
      * vägar - Innehåller JavaScript-filer och HTML-filer som läser in startprocess, att göra-och-spåra samt inställningar på arbetsytan i AEM Forms.
      * services - Innehåller service.js som används i AEM Forms arbetsyta.
      * mallar - Innehåller alla mallar, det vill säga HTML-filer för alla komponenter på AEM Forms arbetsyta.
      * util - Innehåller alla verktygsfiler (JavaScript) som används på arbetsytan i AEM Forms.
      * vyer - Innehåller vyer över alla komponenter på arbetsytan i AEM Forms.

   * main.js
   * registry.js
   * router.js

* libs:

   * ws - Innehåller pluginPing.pdf, pdf.html och WSNextAdapter.swf.

* Språk - innehåller .content.xml.
* nationella inställningar:

   * de-DE - Innehåller translation.json för tyska.
   * en-US - Innehåller translation.json för engelska.
   * fr-FR - Innehåller translation.json för franska.
   * ja-JP - Innehåller translation.json för japanska.
   * html.jsp - Innehåller kod för att ta reda på webbläsarens aktuella språkområde.

* Index - innehåller .content.xml
* profile - Innehåller offline.jsp.
* GET.jsp
* html.jsp
* content.xml
* _rep_policy.xml
