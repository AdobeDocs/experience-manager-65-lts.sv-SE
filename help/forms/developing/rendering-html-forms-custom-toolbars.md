---
title: Återge HTML Forms med CustomToolbars
description: Använd Forms för att anpassa ett verktygsfält som återges med ett HTML-formulär. Du kan återge ett HTML-formulär med ett anpassat verktygsfält med hjälp av Java API och ett Web Service API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/rendering_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2328'
ht-degree: 0%

---

# Återge HTML Forms med CustomToolbars {#rendering-html-forms-with-customtoolbars}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

## Återge HTML Forms med anpassade verktygsfält {#rendering-html-forms-with-custom-toolbars}

Med Forms-tjänsten kan du anpassa ett verktygsfält som återges med ett HTML-formulär. Ett verktygsfält kan anpassas för att ändra utseendet genom att åsidosätta CSS-standardformat och lägga till dynamiskt beteende genom att åsidosätta Java-skript. Ett verktygsfält anpassas med hjälp av en XML-fil med namnet fscmenu.xml. Som standard hämtar Forms-tjänsten den här filen från en internt angiven URI-plats.

>[!NOTE]
>
>Denna URI-plats finns i filen adobe-forms-core.jar, som finns i filen adobe-forms-dsc.jar. Filen adobe-forms-dsc.jar finns i mappen C:\Adobe\Adobe_Experience_Manager_forms\ (C:\ är installationskatalogen). Du kan använda ett filextraheringsverktyg, till exempel Win RAR, för att öppna Adobe-filen.

Du kan kopiera fscmenu.xml från den här platsen, ändra den så att den uppfyller dina krav och sedan placera den på en anpassad URI-plats. Använd sedan Forms Service API för att ange körningsalternativ som resulterar i att Forms-tjänsten använder fscmenu.xml-filen från den angivna platsen. Dessa åtgärder resulterar i att Forms-tjänsten återger ett HTML-formulär som har ett anpassat verktygsfält.

Förutom filen fscmenu.xml behöver du även följande filer:

* fscmenu.js
* fscattachments.js
* fscmenu.css
* fscmenu-v.css
* fscmenu-ie.css
* fscdialog.css

fscJS är det Java-skript som associeras med varje nod. Du måste ange en för noden `div#fscmenu` och eventuellt för `ul#fscmenuItem`-noder. JS-filerna implementerar grundläggande verktygsfältsfunktioner och standardfilerna fungerar.

fscCSS är en formatmall som är kopplad till en viss nod. Formaten i CSS-filerna anger verktygsfältets utseende. *fscVCSS* är en formatmall för ett lodrätt verktygsfält, som visas till vänster om det återgivna HTML-formuläret. *fscIECSS* är en formatmall som används för HTML-formulär som återges i Internet Explorer.

Kontrollera att det finns referenser till alla ovanstående filer i filen fscmenu.xml. I filen fscmenu.xml anger du URI-platser som ska peka på dessa filer så att Forms-tjänsten kan hitta dem. Som standard är de här filerna tillgängliga på URI-platser som börjar med interna nyckelord `FSWebRoot` eller `ApplicationWebRoot`.

Om du vill anpassa verktygsfältet ersätter du nyckelorden med det externa nyckelordet `FSToolBarURI`. Detta nyckelord representerar den URI som skickas till Forms-tjänsten vid körning (detta visas senare i detta avsnitt).

Du kan också ange absoluta platser för dessa JS- och CSS-filer, till exempel https://www.mycompany.com/scripts/misc/fscmenu.js. I den här situationen behöver du inte använda nyckelordet `FSToolBarURI`.

>[!NOTE]
>
>Vi rekommenderar inte att du blandar de sätt på vilka dessa filer refereras. Det innebär att alla URI:er ska refereras med nyckelordet `FSToolBarURI` eller en absolut plats.

Du kan hämta JS- och CSS-filerna genom att öppna filen adobe-forms-&lt;appserver>.ear. I den här filen öppnar du adobe-forms-res.war. Alla dessa filer finns i WAR-filen. Filen adobe-forms-&lt;appserver>.ear finns i installationsmappen för AEM-formulär (C:\ är installationskatalogen). Du kan öppna adobe-forms-&lt;appserver>.ear med ett filextraheringsverktyg som WinRAR.

I följande XML-syntax visas ett exempel på filen fscmenu.xml.

```html
 <div id="fscmenu" fscJS="FSToolBarURI/scripts/fscmenu.js" fscCSS="FSToolBarURI/fscmenu.css" fscVCSS="FSToolBarURI/fscmenu-v.css" fscIECSS="FSToolBarURI/fscmenu-ie.css">
         <ul class="fscmenuItem" id="Home">
             <li>
                 <a href="#" fscTarget="_top" tabindex="1">Home</a>
             </li>
         </ul>
         <ul class="fscmenuItem" id="Upload" fscJS="FSToolBarURI/scripts/fscattachments.js" fscCSS="FSToolBarURI/fscdialog.css">
             <li>
                 <a tabindex="2">Upload Attachments</a>
                 <ul class="fscmenuPopup" id="fscUploadAttachments">
                     <li>
                         <a href="javascript:doUploadDialog();" tabindex="3">Add ...</a>
                     </li>
                     <li>
                         <a href="javascript:doDeleteDialog();" tabindex="4">Delete ...</a>
                     </li>
                 </ul>
             </li>
         </ul>
         <ul class="fscmenuItem" id="Download">
             <li>
                 <a tabindex="100">Download Attachments</a>
                 <ul class="fscmenuPopup">
                     <li>
                         <a tabindex="101">None available</a>
                     </li>
                 </ul>
             </li>
         </ul>
     </div>
```

>[!NOTE]
>
>Den fetstilta texten representerar URI:er för CSS- och JS-filer som måste refereras.

Följande objekt beskriver hur du kan anpassa ett verktygsfält:

* Ändra värdena för attributen `fscJS`, `fscCSS`, `fscVCSS`, `fscIECSS` (i filen fscmenu.xml) så att de återspeglar de anpassade platserna för de refererade filerna med någon av de metoder som beskrivs i det här avsnittet (till exempel `fscJS="FSToolBarURI/scripts/fscmenu.js"`).
* Alla CSS- och JS-filer måste anges. Om ingen av filerna ändras anger du standardfilen på den anpassade platsen. Du kan hämta standardfilerna genom att öppna olika filer enligt beskrivningen i det här avsnittet.
* Det är tillåtet att ange en absolut referens (till exempel https://www.example.com/scripts/custom-vertical-fscmenu.css) för alla filer.
* JS- och CSS-filerna som noden `div#fscmenu` kräver är viktiga för verktygsfältets funktioner. Enskilda `ul#fscmenuItem`-noder kan ha stöd för JS- eller CSS-filer.

**Ändrar det lokala värdet**

När du anpassar ett verktygsfält kan du ändra det nationella värdet för verktygsfältet. Det innebär att du kan visa det på ett annat språk. I följande bild visas ett anpassat verktygsfält på franska.

>[!NOTE]
>
>Det går inte att skapa ett anpassat verktygsfält på flera språk. Verktygsfält kan inte använda olika XML-filer baserade på språkinställningarna.

Om du vill ändra språkvärdet för ett verktygsfält kontrollerar du att filen fscmenu.xml innehåller det språk som du vill visa. Följande XML-syntax visar filen fscmenu.xml som används för att visa ett franskt verktygsfält.

```html
 <div id="fscmenu" fscJS="FSToolBarURI/scripts/fscmenu.js" fscCSS="FSToolBarURI/fscmenu.css" fscVCSS="FSToolBarURI/fscmenu-v.css" fscIECSS="FSToolBarURI/fscmenu-ie.css">
         <ul class="fscmenuItem" id="Home">
             <li>
                 <a href="#" fscTarget="_top" tabindex="1">Accueil</a>
             </li>
         </ul>
         <ul class="fscmenuItem" id="Upload" fscJS="FSToolBarURI/scripts/fscattachments.js" fscCSS="FSToolBarURI/fscdialog.css">
             <li>
                 <a tabindex="2">Télécharger les pièces jointes</a>
                 <ul class="fscmenuPopup" id="fscUploadAttachments">
                     <li>
                         <a href="javascript:doUploadDialog();" tabindex="3">Ajouter...</a>
                     </li>
                     <li>
                         <a href="javascript:doDeleteDialog();" tabindex="4">Supprimer...</a>
                     </li>
                 </ul>
             </li>
         </ul>
         <ul class="fscmenuItem" id="Download">
             <li>
                 <a tabindex="100">Télécharger les pièces jointes</a>
                 <ul class="fscmenuPopup">
                     <li>
                         <a tabindex="101">Aucune disponible</a>
                     </li>
                 </ul>
             </li>
         </ul>
     </div>
```

>[!NOTE]
>
>I snabbstarter som är kopplade till det här avsnittet används den här XML-filen för att visa ett anpassat verktygsfält i franska, vilket visades i föregående bild.

Ange också ett giltigt språkvärde genom att anropa `HTMLRenderSpec`-objektets `setLocale`-metod och skicka ett strängvärde som anger språkvärdet. Skicka till exempel `fr_FR` för att ange franska. Forms-tjänsten levereras med lokaliserade verktygsfält.

>[!NOTE]
>
>Innan du återger ett HTML-formulär som använder ett anpassat verktygsfält måste du veta hur HTML-formulär återges. (Se [Återge Forms som HTML](/help/forms/developing/rendering-forms-html.md).)

Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Så här återger du ett HTML-formulär som innehåller ett anpassat verktygsfält:

1. Inkludera projektfiler.
1. Skapa ett Forms Java API-objekt.
1. Referera till en anpassad fscmenu XML-fil.
1. Återge ett HTML-formulär.
1. Skriv formulärdataströmmen till klientens webbläsare.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster inkluderar du proxyfilerna.

**Skapa ett Forms Java API-objekt**

Innan du programmässigt kan utföra en åtgärd som stöds av Forms måste du skapa ett Forms-klientobjekt.

**Referera till en anpassad fscmenu XML-fil**

Om du vill återge ett HTML-formulär som innehåller ett anpassat verktygsfält hänvisar du till en fscmenu XML-fil som beskriver verktygsfältet. (I det här avsnittet finns två exempel på en XML-fil för fscmenu.) Se även till att fscmenu.xml-filen anger platserna för alla refererade filer korrekt. Se till att alla filer refereras till antingen av nyckelordet `FSToolBarURI` eller av deras absoluta platser, som tidigare nämnts i det här avsnittet.

**Återge ett HTML-formulär**

Om du vill återge ett HTML-formulär anger du en formulärdesign som har skapats i Designer och sparats som en XDP-fil. Välj även en HTML-omvandlingstyp. Du kan till exempel ange den HTML-omformningstyp som återger en dynamisk HTML för Internet Explorer 5.0 eller senare.

Återgivning av ett HTML-formulär kräver också värden, t.ex. URI-värden för återgivning av andra formulärtyper.

**Skriv formulärdataströmmen till klientwebbläsaren**

När Forms-tjänsten återger ett HTML-formulär returneras en formulärdataström som du måste skriva till klientens webbläsare för att göra HTML-formuläret synligt för användarna.

**Se även**

[Återge ett HTML-formulär med ett anpassat verktygsfält med Java API](#render-an-html-form-with-a-custom-toolbar-using-the-java-api)

[Återge ett HTML-formulär med ett anpassat verktygsfält med hjälp av webbtjänstens API](#rendering-an-html-form-with-a-custom-toolbar-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Forms Service API](/help/forms/developing/forms-service-api-quick-starts.md#forms-service-api-quick-starts)

[Rendering Interactive PDF forms](/help/forms/developing/rendering-interactive-pdf-forms.md)

[Återger Forms som HTML](/help/forms/developing/rendering-forms-html.md)

[Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md)

### Återge ett HTML-formulär med ett anpassat verktygsfält med Java API {#render-an-html-form-with-a-custom-toolbar-using-the-java-api}

Återge ett HTML-formulär som innehåller ett anpassat verktygsfält med hjälp av Forms Service API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg.

1. Skapa ett Forms Java API-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera en anpassad fscmenu XML-fil

   * Skapa ett `HTMLRenderSpec`-objekt med hjälp av dess konstruktor.
   * Om du vill återge ett HTML-formulär med ett verktygsfält anropar du `HTMLRenderSpec`-objektets `setHTMLToolbar`-metod och skickar ett `HTMLToolbar` enum-värde. Om du till exempel vill visa ett lodrätt HTML-verktygsfält skickar du `HTMLToolbar.Vertical`.
   * Ange platsen för fscmenu XML-filen genom att anropa `HTMLRenderSpec`-objektets `setToolbarURI`-metod och skicka ett strängvärde som anger URI-platsen för XML-filen.
   * Om det är tillämpligt anger du språkvärdet genom att anropa `HTMLRenderSpec`-objektets `setLocale`-metod och skicka ett strängvärde som anger språkvärdet. Standardvärdet är engelska.

   >[!NOTE]
   >
   >Snabbstarter som är associerade med det här avsnittet anger det här värdet till `fr_FR`*.*

1. Återge ett HTML-formulär

   Anropa `FormsServiceClient`-objektets `renderHTMLForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `TransformTo`-uppräkningsvärde som anger HTML-inställningstypen. Om du till exempel vill återge ett HTML-formulär som är kompatibelt med dynamiska HTML för Internet Explorer 5.0 eller senare anger du `TransformTo.MSDHTML`.
   * Ett `com.adobe.idp.Document`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du ett tomt `com.adobe.idp.Document`-objekt.
   * Objektet `HTMLRenderSpec` som lagrar körningsalternativ för HTML.
   * Ett strängvärde som anger rubrikvärdet `HTTP_USER_AGENT`, till exempel `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)`.
   * Ett `URLSpec`-objekt som lagrar URI-värden som krävs för att återge ett HTML-formulär.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter, och du kan ange `null` om du inte vill bifoga filer till formuläret.

   Metoden `renderHTMLForm` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `com.adobe.idp.Document`-objekt genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Hämta innehållstypen för objektet `com.adobe.idp.Document` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `com.adobe.idp.Document`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
   * Skapa en bytearray och fyll i den med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
   * Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Snabbstart (SOAP-läge): Återge ett HTML-formulär med ett anpassat verktygsfält med Java API](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-rendering-an-html-form-with-a-custom-toolbar-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Återge ett HTML-formulär med ett anpassat verktygsfält med hjälp av webbtjänstens API {#rendering-an-html-form-with-a-custom-toolbar-using-the-web-service-api}

Återge ett HTML-formulär som innehåller ett anpassat verktygsfält med hjälp av Forms tjänst-API (webbtjänst):

1. Inkludera projektfiler

   * Skapa Java-proxyklasser som använder Forms tjänst-WSDL.
   * Inkludera Java-proxyklasserna i klassökvägen.

1. Skapa ett Forms Java API-objekt

   Skapa ett `FormsService`-objekt och ange autentiseringsvärden.

1. Referera en anpassad fscmenu XML-fil

   * Skapa ett `HTMLRenderSpec`-objekt med hjälp av dess konstruktor.
   * Om du vill återge ett HTML-formulär med ett verktygsfält anropar du `HTMLRenderSpec`-objektets `setHTMLToolbar`-metod och skickar ett `HTMLToolbar` enum-värde. Om du till exempel vill visa ett lodrätt HTML-verktygsfält skickar du `HTMLToolbar.Vertical`.
   * Ange platsen för fscmenu XML-filen genom att anropa `HTMLRenderSpec`-objektets `setToolbarURI`-metod och skicka ett strängvärde som anger URI-platsen för XML-filen.
   * Om det är tillämpligt anger du språkvärdet genom att anropa `HTMLRenderSpec`-objektets `setLocale`-metod och skicka ett strängvärde som anger språkvärdet. Standardvärdet är engelska.

   >[!NOTE]
   >
   >Snabbstarter som är associerade med det här avsnittet anger det här värdet till `fr_FR`*.*

1. Återge ett HTML-formulär

   Anropa `FormsService`-objektets `renderHTMLForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `TransformTo`-uppräkningsvärde som anger HTML-inställningstypen. Om du till exempel vill återge ett HTML-formulär som är kompatibelt med dynamiska HTML för Internet Explorer 5.0 eller senare anger du `TransformTo.MSDHTML`.
   * Ett `BLOB`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du `null`.
   * Objektet `HTMLRenderSpec` som lagrar körningsalternativ för HTML.
   * Ett strängvärde som anger rubrikvärdet `HTTP_USER_AGENT`, till exempel `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322`). Du kan skicka en tom sträng om du inte vill ange det här värdet.
   * Ett `URLSpec`-objekt som lagrar URI-värden som krävs för att återge ett HTML-formulär.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Den här parametern är valfri och du kan ange `null` om du inte vill bifoga filer till formuläret.
   * Ett tomt `com.adobe.idp.services.holders.BLOBHolder`-objekt som fylls i av metoden `renderHTMLForm`. Det här parametervärdet lagrar det återgivna formuläret.
   * Ett tomt `com.adobe.idp.services.holders.BLOBHolder`-objekt som fylls i av metoden `renderHTMLForm`. Den här parametern lagrar XML-utdata.
   * Ett tomt `javax.xml.rpc.holders.LongHolder`-objekt som fylls i av metoden `renderHTMLForm`. Det här argumentet lagrar antalet sidor i formuläret.
   * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden `renderHTMLForm`. Det här argumentet lagrar språkets värde.
   * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden `renderHTMLForm`. Det här argumentet lagrar det återgivningsvärde för HTML som används.
   * Ett tomt `com.adobe.idp.services.holders.FormsResultHolder`-objekt som innehåller resultatet av den här åtgärden.

   Metoden `renderHTMLForm` fyller i objektet `com.adobe.idp.services.holders.FormsResultHolder` som skickas som det sista argumentvärdet med en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `FormResult`-objekt genom att hämta värdet för `com.adobe.idp.services.holders.FormsResultHolder`-objektets `value`-datamedlem.
   * Skapa ett `BLOB`-objekt som innehåller formulärdata genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Hämta innehållstypen för objektet `BLOB` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `BLOB`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa en bytearray och fyll i den genom att anropa `BLOB`-objektets `getBinaryData`-metod. Den här aktiviteten tilldelar innehållet i objektet `FormsResult` till bytearrayen.
   * Anropa `javax.servlet.http.HttpServletResponse`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
