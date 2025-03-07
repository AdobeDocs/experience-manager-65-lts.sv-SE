---
title: Återger Forms som HTML
description: Använd tjänsten Forms för att återge formulär som HTML som svar på en HTTP-begäran från en webbläsare. Du kan använda Java&trade; API och Web Service API för att återge formulär som HTML.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/rendering_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
hide: true
hidefromtoc: true
exl-id: f1e6adca-0591-4974-9c12-66706aa35247
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '4099'
ht-degree: 0%

---

# Återger Forms som HTML {#rendering-forms-as-html}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Forms-tjänsten återger formulär som HTML som svar på en HTTP-begäran från en webbläsare. En fördel med att återge ett formulär som HTML är att den dator där klientwebbläsaren finns inte kräver Adobe Reader, Acrobat eller Flash Player (för formulärguider (borttagen)).

Om du vill återge ett formulär som HTML måste formulärdesignen sparas som en XDP-fil. En formulärdesign som sparas som en PDF-fil kan inte återges som HTML. När du utvecklar en formulärdesign i Designer som ska återges som HTML bör du tänka på följande kriterier:

* Använd inte ett objekts kantegenskaper för att rita linjer, rutor eller rutnät i formuläret. Vissa webbläsare kanske inte justerar kantlinjer exakt som de visas i en förhandsvisning. Objekt kan se ut som skiktade eller skjuta andra objekt bort från sin förväntade position.
* Du kan använda linjer, rektanglar och cirklar för att definiera bakgrunden.
* Rita text något större än vad som krävs för att texten ska få plats. I vissa webbläsare visas inte texten läsbart.

>[!NOTE]
>
>När du återger ett formulär som innehåller TIFF-bilder med `FormServiceClient`-objektets `(Deprecated) renderHTMLForm` - och `renderHTMLForm2` -metoder, visas inte TIFF-bilderna i det återgivna HTML-formuläret som visas i webbläsare Internet Explorer eller Mozilla Firefox. De här webbläsarna har inte inbyggt stöd för TIFF-bilder.

## HTML sidor {#html-pages}

När en formulärdesign återges som ett HTML-formulär återges varje delformulär på andra nivån som en HTML-sida (panel). Du kan visa ett delformulärs hierarki i Designer. Underordnade delformulär som tillhör rotdelformuläret (standardnamnet för ett rotdelformulär är formulär1) är paneldelformulär. I följande exempel visas delformulären för en formulärdesign.

```java
     form1
         Master Pages
         PanelSubform1
             NestedDynamicSubform
                 TextEdit1
         PanelSubform2
             TextEdit1
         PanelSubform3
             TextEdit1
         PanelSubform4
             TextEdit1
```

När formulärdesigner återges som HTML-formulär begränsas inte panelerna till någon viss sidstorlek. Om du har dynamiska delformulär bör de vara kapslade i paneldelformuläret. Dynamiska delformulär kan expandera till ett obegränsat antal HTML-sidor.

När ett formulär återges som ett HTML-formulär har sidstorlekar (som krävs för att sidnumrera formulär som återges som PDF) ingen betydelse. Eftersom ett formulär med flödeslayout kan expandera till ett obegränsat antal HTML-sidor är det viktigt att undvika sidfötter på mallsidan. En sidfot under innehållsområdet på en mallsida kan skriva över HTML-innehåll som flödar förbi en sidgräns.

Du måste gå från panel till panel med metoderna `xfa.host.pageUp` och `xfa.host.pageDown`. Du ändrar sidor genom att skicka ett formulär till Forms och låta Forms-tjänsten återge formuläret till klientenheten, vanligtvis en webbläsare.

>[!NOTE]
>
>Processen med att skicka ett formulär till Forms-tjänsten och sedan låta Forms-tjänsten återge formuläret till klientenheten kallas för att skicka runt data till servern.

>[!NOTE]
>
>Om du vill anpassa utseendet på knappen HTML Digital Signature i ett HTML-formulär måste du ändra följande egenskaper i filen fscdigsig.css (i filen adobe-forms-ds.ear > adobe-forms-ds.war):

**`.fsc-ds-ssb`**: Den här formatmallen kan användas om det finns ett tomt teckenfält.

**`.fsc-ds-ssv`**: Den här formatmallen kan användas om det finns ett giltigt signaturfält.

**`.fsc-ds-ssc`**: Den här formatmallen kan användas om det finns ett giltigt signaturfält men data har ändrats.

**`.fsc-ds-ssi`**: Den här formatmallen kan användas om det finns ett ogiltigt teckenfält.

**`.fsc-ds-popup-bg`**: Den här formatmallsegenskapen används inte.

**.`fsc-ds-popup-btn`**: Den här formatmallsegenskapen används inte.

## Köra skript {#running-scripts}

En formulärförfattare anger om ett skript ska köras på servern eller på klienten. Forms-tjänsten skapar en distribuerad händelsebearbetningsmiljö för att köra formulärintelligens som kan distribueras mellan klienten och servern med attributet `runAt`. Mer information om det här attributet och hur du skapar skript i formulärdesigner finns i [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63)

Forms-tjänsten kan köra skript medan formuläret återges. Det innebär att du kan förifylla ett formulär med data genom att ansluta till en databas eller till webbtjänster som kanske inte är tillgängliga på klienten. Du kan också ange att en knapps `Click`-händelse ska köras på servern så att klienten skickar data till servern. Detta gör att klienten kan köra skript som kan kräva serverresurser, t.ex. en företagsdatabas, medan en användare interagerar med ett formulär. För HTML-formulär kan formulärskript endast köras på servern. Därför måste du markera de här skripten så att de körs vid `server` eller `both`.

Du kan utforma formulär som rör sig mellan sidor (paneler) genom att anropa metoderna `xfa.host.pageUp` och `xfa.host.pageDown`. Det här skriptet placeras i en knapps `Click`-händelse och attributet `runAt` är inställt på `Both`. Anledningen till att du väljer `Both` är att Adobe Reader eller Acrobat (för formulär som återges som PDF) kan ändra sidor utan att gå till servern, och HTML-formulär kan ändra sidor genom att skicka utdata till servern. Det innebär att ett formulär skickas till Forms och att ett formulär återges som HTML med den nya sidan som visas.

Vi rekommenderar att du inte ger skriptvariabler och formulärfält samma namn, till exempel objekt. I vissa webbläsare, t.ex. Internet Explorer, går det inte att initiera en variabel med samma namn som ett formulärfält, vilket resulterar i ett skriptfel. Det är god praxis att ge formulärfält och skriptvariabler olika namn.

När du återger HTML-formulär som innehåller både sidnavigeringsfunktioner och formulärskript (t.ex. antar att ett skript hämtar fältdata från en databas varje gång formuläret återges), kontrollerar du att formulärskriptet är i händelsen form:calculate i stället för i formen:readyevent.

Formulärskript som finns i formen:ready-händelsen körs bara en gång under den inledande återgivningen av formuläret och inte körs för efterföljande sidhämtningar. Händelsen form:calculate körs däremot för varje sidnavigering där formuläret återges.

>[!NOTE]
>
På flersidiga formulär behålls inte ändringar som gjorts av JavaScript till en sida om du går till en annan sida.

Du kan anropa egna skript innan du skickar in ett formulär. Den här funktionen fungerar i alla tillgängliga webbläsare. Den kan dock bara användas när användare återger det HTML-formulär som har egenskapen `Output Type` inställd på `Form Body`. Det fungerar inte när `Output Type` är `Full HTML`. Mer information om hur du konfigurerar den här funktionen finns i Konfigurera formulär i administrationshjälpen.

Definiera först en callback-funktion som anropas innan formuläret skickas, där funktionens namn är `_user_onsubmit`. Det antas att funktionen inte genererar något undantag, eller att undantaget ignoreras om det gör det. Du rekommenderas att placera JavaScript-funktionen i huvudsektionen i html, men du kan deklarera den var som helst före slutet av skripttaggarna som innehåller `xfasubset.js`.

När formserver återger en XDP-fil som innehåller en nedrullningsbar lista skapas även två dolda textfält förutom att listrutan skapas. Dessa textfält lagrar data i den nedrullningsbara listan (ett lagrar alternativens visningsnamn och andra lagrar alternativens värden). Därför skickas alla data i den nedrullningsbara listan varje gång en användare skickar formuläret. Om du inte vill skicka så mycket data varje gång kan du skriva ett eget skript som inaktiverar det. Till exempel: Listrutans namn är `drpOrderedByStateProv` och det omsluts av delformulärsrubriken. Namnet på indataelementet för HTML blir `header[0].drpOrderedByStateProv[0]`. Namnet på de dolda fält som lagrar och skickar data i listrutan har följande namn: `header[0].drpOrderedByStateProv_DISPLAYITEMS_[0] header[0].drpOrderedByStateProv_VALUEITEMS_[0]`

Du kan inaktivera dessa indataelement på följande sätt om du inte vill publicera data. `var __CUSTOM_SCRIPTS_VERSION = 1; //enabling the feature function _user_onsubmit() { var elems = document.getElementsByName("header[0].drpOrderedByStateProv_DISPLAYITEMS_[0]"); elems[0].disabled = true; elems = document.getElementsByName("header[0].drpOrderedByStateProv_VALUEITEMS_[0]"); elems[0].disabled = true; }`

```java
header[0].drpOrderedByStateProv_DISPLAYITEMS_[0] header[0].drpOrderedByStateProv_VALUEITEMS_[0]
```

```java
var __CUSTOM_SCRIPTS_VERSION = 1; //enabling the feature
    function _user_onsubmit() {
    var elems = document.getElementsByName("header[0].drpOrderedByStateProv_DISPLAYITEMS_[0]");
    elems[0].disabled = true;
    elems = document.getElementsByName("header[0].drpOrderedByStateProv_VALUEITEMS_[0]");
    elems[0].disabled = true;
    }
```

## XFA-delmängder {#xfa-subsets}

När du skapar formulärdesigner som ska återges som HTML måste du begränsa skripten till XFA-delmängden för skript i JavaScript-språket.

Skript som körs på klienten eller körs både på klienten och servern måste skrivas i XFA-delmängden. Skript som körs på servern kan använda den fullständiga XFA-skriptmodellen och även FormCalc. Mer information om hur du använder JavaScript finns i [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63).

När du kör skript på klienten kan bara den aktuella panelen som visas använda skript. Du kan till exempel inte skriva skript mot fält som finns på panel A när panel B visas. När du kör skript på servern är alla paneler tillgängliga.

Var försiktig när du använder SOM-uttryck (Scripting Object Model) i skript som körs på klienten. Endast en förenklad delmängd av SOM-uttryck stöds av skript som körs på klienten.

## Tidsinställning för händelser {#event-timing}

XFA-delmängden definierar XFA-händelser som mappas till HTML-händelser. Det finns en liten skillnad i beteendet när det gäller tidpunkten för beräkning och validering av händelser. I en webbläsare körs en fullständig calculate-händelse när du avslutar ett fält. Beräkningshändelser körs inte automatiskt när du ändrar ett fältvärde. Du kan tvinga fram en calculate-händelse genom att anropa metoden `xfa.form.execCalculate`.

I en webbläsare körs valideringshändelser bara när ett fält avslutas eller när ett formulär skickas. Du kan tvinga fram en validate-händelse med metoden `xfa.form.execValidate`.

Forms som visas i en webbläsare (till skillnad från Adobe Reader eller Acrobat) följer XFA null-testet (fel eller varningar) för obligatoriska fält.

* Om null-testet genererar ett fel och du avslutar ett fält utan att ange ett värde, visas en meddelanderuta och du flyttas till fältet efter att du klickat på OK.
* Om ett null-test ger en varning och du avslutar ett fält utan att ange ett värde, uppmanas du att klicka på OK eller Avbryt, så att du kan fortsätta utan att ange ett värde eller gå tillbaka till fältet för att ange ett värde.

Mer information om ett null-test finns i [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63).

## Formulärknappar {#form-buttons}

När du klickar på en skicka-knapp skickas formulärdata till Forms-tjänsten och anger att formulärbearbetningen är slutförd. Händelsen `preSubmit` kan ställas in för att köras på klienten eller servern. Händelsen `preSubmit` körs före formuläröverföringen om den är konfigurerad att köras på klienten. Annars körs händelsen `preSubmit` på servern när formuläret skickas. Mer information om händelsen `preSubmit` finns i [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63).

Om en knapp inte har något klientskript kopplat till sig, skickas data till servern, beräkningar utförs på servern och HTML-formuläret genereras om. Om en knapp innehåller ett klientskript skickas inga data till servern och klientskriptet körs i webbläsaren.

## HTML 4.0 webbläsare {#html-4-0-web-browser}

En webbläsare som bara har stöd för HTML 4.0 kan inte hantera XFA-deluppsättningens klientskriptmodell. När du skapar en formulärdesign som ska fungera i både HTML 4.0 och MSDHTML eller CSS2HTML kommer ett skript som är markerat för att köras på klienten att köras på servern. Anta att en användare klickar på en knapp som finns i ett formulär som visas i en webbläsare i HTML 4.0. I sådana fall skickas formulärdata till servern där skriptet på klientsidan körs.

Vi rekommenderar att du placerar formulärlogiken i calculate-händelser som körs på servern i HTML 4.0 och på klienten för MSDHTML eller CSS2HTML.

## Underhåll presentationsändringar {#maintaining-presentation-changes}

När du växlar mellan HTML-sidor (paneler) bevaras endast datastatus. Inställningar som bakgrundsfärg eller obligatoriska fältinställningar bevaras inte (om de skiljer sig från de ursprungliga inställningarna). Om du vill behålla presentationstillståndet måste du skapa fält (vanligen dolda) som representerar presentationstillståndet för fält. Om du lägger till ett skript i ett fälts `Calculate`-händelse som ändrar presentationen baserat på dolda fältvärden, kan du behålla presentationstillståndet när du går fram och tillbaka mellan HTML-sidor (paneler).

Följande skript bevarar `fillColor` för ett fält baserat på värdet `hiddenField`. Anta att det här skriptet finns i ett fälts `Calculate`-händelse.

```java
     If (hiddenField.rawValue == 1)
         this.fillColor = "255,0,0"
     else
         this.fillColor = "0,255,0"
```

>[!NOTE]
>
Statiska objekt visas inte i ett återgivet HTML-formulär när de är kapslade i en tabellcell. En cirkel och rektangel som är kapslad i en tabellcell visas till exempel inte i ett återgivningsformulär i HTML. Samma statiska objekt visas emellertid korrekt utanför tabellen.

## Digitalt signera HTML-formulär {#digitally-signing-html-forms}

Du kan inte signera ett HTML-formulär som innehåller ett fält för elektronisk underskrift om formuläret återges som en av följande HTML-omformningar:

* AHTML
* HTML4
* StaticHTML
* NoScriptXHTML

Mer information om att signera ett dokument digitalt finns i [Signera och certifiera dokument digitalt](/help/forms/developing/digitally-signing-certifying-documents.md)

## Rendera ett hjälpmedelsanpassat XHTML-formulär {#rendering-an-accessibility-guidelines-compliant-xhtml-form}

Du kan återge ett fullständigt HTML-formulär som är kompatibelt med riktlinjerna för tillgänglighet. Det innebär att formuläret återges med fullständiga HTML-taggar i motsats till HTML-formuläret som återges med body-taggar (inte en fullständig HTML-sida).

## Validerar formulärdata {#validating-form-data}

Vi rekommenderar att du begränsar användningen av valideringsregler för formulärfält när du återger formuläret som ett HTML-formulär. Vissa valideringsregler kanske inte stöds för HTML-formulär. Om du till exempel använder valideringsmönstret MM-DD-YYY för ett `Date/Time`-fält i en formulärdesign som återges som ett HTML-formulär, fungerar det inte korrekt, även om datumet har angetts korrekt. Det här valideringsmönstret fungerar emellertid korrekt för formulär som återges som PDF.

>[!NOTE]
>
Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Sammanfattning av steg {#summary-of-steps}

Så här återger du ett HTML-formulär:

1. Inkludera projektfiler.
1. Skapa ett Forms Client API-objekt.
1. Ange körningsalternativ för HTML.
1. Återge ett HTML-formulär.
1. Skriv formulärdataströmmen till klientens webbläsare.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett Forms Client API-objekt**

Innan du kan importera data programmatiskt till ett PDF formClient-API måste du skapa en tjänstklient för integrering av formulärdata. När du skapar en tjänstklient definierar du de anslutningsinställningar som krävs för att anropa en tjänst.

**Ange körningsalternativ för HTML**

Du anger körningsalternativ för HTML när du återger ett HTML-formulär. Du kan till exempel lägga till ett verktygsfält i ett HTML-formulär så att användare kan välja bifogade filer på klientdatorn eller hämta bifogade filer som återges med HTML-formuläret. Som standard är ett HTML-verktygsfält inaktiverat. Om du vill lägga till ett verktygsfält i ett HTML-formulär måste du programmässigt ange körningsalternativ. Som standard består ett HTML-verktygsfält av följande knappar:

* `Home`: Tillhandahåller en länk till programmets webbrot.
* `Upload`: Tillhandahåller ett användargränssnitt för att välja filer att bifoga till det aktuella formuläret.
* `Download`: Tillhandahåller ett användargränssnitt för att visa de bifogade filerna.

När ett HTML-verktygsfält visas i ett HTML-formulär kan användaren välja högst tio filer att skicka tillsammans med formulärdata. När filerna har skickats kan Forms-tjänsten hämta filerna.

När du återger ett formulär som HTML kan du ange ett användaragentvärde. Ett användaragentvärde ger information om webbläsare och system. Detta är ett valfritt värde och du kan skicka ett tomt strängvärde. När du återger ett HTML-formulär med Java API-snabbstarten visas hur du får ett användaragentvärde och använder det för att återge ett formulär som HTML.

HTTP-URL:er dit formulärdata skickas kan anges genom att ange mål-URL:en med hjälp av Forms Service Client-API:t eller anges i knappen Skicka i XDP-formulärdesignen. Om mål-URL:en anges i formulärdesignen ska du inte ange något värde med Forms Service Client API.

>[!NOTE]
>
Det är valfritt att återge ett HTML-formulär med ett verktygsfält.

>[!NOTE]
>
Om du återger ett AHTML-formulär bör du inte lägga till ett verktygsfält i formuläret.

**Återge ett HTML-formulär**

Om du vill återge ett HTML-formulär anger du en formulärdesign som har skapats i Designer och sparats som en XDP-fil. Välj en HTML-omvandlingstyp. Du kan till exempel ange den HTML-omformningstyp som återger en dynamisk HTML för Internet Explorer 5.0 eller senare.

Återgivning av ett HTML-formulär kräver också värden, t.ex. URI-värden som krävs för att återge andra formulärtyper.

**Skriv formulärdataströmmen till klientwebbläsaren**

När Forms-tjänsten återger ett HTML-formulär returneras en formulärdataström som du måste skriva till klientens webbläsare. När formuläret skrivs till webbläsaren visas det för användaren.

**Se även**

[Återge ett formulär som HTML med Java API](#render-a-form-as-html-using-the-java-api)

[Återge ett formulär som HTML med webbtjänstens API](#render-a-form-as-html-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Forms Service API](/help/forms/developing/forms-service-api-quick-starts.md#forms-service-api-quick-starts)

[Rendering Interactive PDF forms](/help/forms/developing/rendering-interactive-pdf-forms.md)

[Återge HTML Forms med anpassade verktygsfält](/help/forms/developing/rendering-html-forms-custom-toolbars.md)

[Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md)

## Återge ett formulär som HTML med Java API {#render-a-form-as-html-using-the-java-api}

Återge ett HTML-formulär med Forms API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg.

1. Skapa ett Forms Client API-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Ange körningsalternativ för HTML

   * Skapa ett `HTMLRenderSpec`-objekt med hjälp av dess konstruktor.
   * Om du vill återge ett HTML-formulär med ett verktygsfält anropar du `HTMLRenderSpec`-objektets `setHTMLToolbar`-metod och skickar ett `HTMLToolbar` enum-värde. Om du till exempel vill visa ett lodrätt HTML-verktygsfält skickar du `HTMLToolbar.Vertical`.
   * Om du vill ange språkvärdet för HTML-formuläret anropar du `HTMLRenderSpec`-objektets `setLocale`-metod och skickar ett strängvärde som anger språkvärdet. (Det här är en valfri inställning.)
   * Om du vill återge HTML-formuläret med fullständiga HTML-taggar anropar du `HTMLRenderSpec`-objektets `setOutputType`-metod och skickar `OutputType.FullHTMLTags`. (Det här är en valfri inställning.)

   >[!NOTE]
   >
   Forms återges inte korrekt i HTML när alternativet `StandAlone` är `true` och `ApplicationWebRoot` refererar till en annan server än J2EE-programservern som är värd för AEM Forms (värdet `ApplicationWebRoot` anges med objektet `URLSpec` som skickas till metoden `FormsServiceClient` object `(Deprecated) renderHTMLForm` ). När `ApplicationWebRoot` är en annan server från den som är värd för AEM Forms, måste värdet för webrot-URI:n i administrationskonsolen anges som formulärets webbprogram-URI-värde. Detta kan du göra genom att logga in på administrationskonsolen, klicka på Tjänster > Forms och ange webbrots-URI som https://server-name:port/FormServer. Spara sedan inställningarna.

1. Återge ett HTML-formulär

   Anropa `FormsServiceClient`-objektets `(Deprecated) renderHTMLForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `TransformTo`-uppräkningsvärde som anger HTML-inställningstypen. Om du till exempel vill återge ett HTML-formulär som är kompatibelt med dynamiska HTML för Internet Explorer 5.0 eller senare anger du `TransformTo.MSDHTML`.
   * Ett `com.adobe.idp.Document`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du ett tomt `com.adobe.idp.Document`-objekt.
   * Objektet `HTMLRenderSpec` som lagrar körningsalternativ för HTML.
   * Ett strängvärde som anger rubrikvärdet `HTTP_USER_AGENT`, till exempel `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)`.
   * Ett `URLSpec`-objekt som lagrar URI-värden som krävs för att återge ett HTML-formulär.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret.

   Metoden `(Deprecated) renderHTMLForm` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som kan skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `com.adobe.idp.Document`-objekt genom att anropa metoden `getOutputContent` för `FormsResult`-objektet.
   * Hämta innehållstypen för objektet `com.adobe.idp.Document` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `com.adobe.idp.Document`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
   * Skapa en bytearray och fyll i den med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
   * Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Återger Forms som HTML](#rendering-forms-as-html)

[Snabbstart (SOAP-läge): Återge ett HTML-formulär med Java API](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-rendering-an-html-form-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Återge ett formulär som HTML med webbtjänstens API {#render-a-form-as-html-using-the-web-service-api}

Återge ett HTML-formulär med Forms API (webbtjänst):

1. Inkludera projektfiler

   * Skapa Java-proxyklasser som använder Forms tjänst-WSDL.
   * Inkludera Java-proxyklasserna i klassökvägen.

1. Skapa ett Forms Client API-objekt

   Skapa ett `FormsService`-objekt och ange autentiseringsvärden.

1. Ange körningsalternativ för HTML

   * Skapa ett `HTMLRenderSpec`-objekt med hjälp av dess konstruktor.
   * Om du vill återge ett HTML-formulär med ett verktygsfält anropar du `HTMLRenderSpec`-objektets `setHTMLToolbar`-metod och skickar ett `HTMLToolbar` enum-värde. Om du till exempel vill visa ett lodrätt HTML-verktygsfält skickar du `HTMLToolbar.Vertical`.
   * Om du vill ange språkvärdet för HTML-formuläret anropar du `HTMLRenderSpec`-objektets `setLocale`-metod och skickar ett strängvärde som anger språkvärdet. Mer information finns i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).
   * Om du vill återge HTML-formuläret med fullständiga HTML-taggar anropar du `HTMLRenderSpec`-objektets `setOutputType`-metod och skickar `OutputType.FullHTMLTags`.

   >[!NOTE]
   >
   Forms återges inte korrekt i HTML när alternativet `StandAlone` är `true` och `ApplicationWebRoot` refererar till en annan server än J2EE-programservern som är värd för AEM Forms (värdet `ApplicationWebRoot` anges med objektet `URLSpec` som skickas till metoden `FormsServiceClient` object `(Deprecated) renderHTMLForm` ). När `ApplicationWebRoot` är en annan server från den som är värd för AEM Forms, måste värdet för webrot-URI:n i administrationskonsolen anges som formulärets webbprogram-URI-värde. Detta kan du göra genom att logga in på administrationskonsolen, klicka på Tjänster > Forms och ange webbrots-URI som https://server-name:port/FormServer. Spara sedan inställningarna.

1. Återge ett HTML-formulär

   Anropa `FormsService`-objektets `(Deprecated) renderHTMLForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `TransformTo`-uppräkningsvärde som anger HTML-inställningstypen. Om du till exempel vill återge ett HTML-formulär som är kompatibelt med dynamiska HTML för Internet Explorer 5.0 eller senare anger du `TransformTo.MSDHTML`.
   * Ett `BLOB`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du `null`. (Se [Förifyll Forms med flödeslayouter](/help/forms/developing/prepopulating-forms-flowable-layouts.md#prepopulating-forms-with-flowable-layouts).)
   * Objektet `HTMLRenderSpec` som lagrar körningsalternativ för HTML.
   * Ett strängvärde som anger rubrikvärdet `HTTP_USER_AGENT`, till exempel `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)`. Du kan skicka en tom sträng om du inte vill ange det här värdet.
   * Ett `URLSpec`-objekt som lagrar URI-värden som krävs för att återge ett HTML-formulär. (Se [Ange URI-värden](/help/forms/developing/rendering-interactive-pdf-forms.md).)
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret. (Se [Bifoga filer i formuläret](/help/forms/developing/rendering-interactive-pdf-forms.md).)
   * Ett tomt `com.adobe.idp.services.holders.BLOBHolder`-objekt som fylls i av metoden. Det här parametervärdet lagrar det återgivna formuläret.
   * Ett tomt `com.adobe.idp.services.holders.BLOBHolder`-objekt som fylls i av metoden. Den här parametern lagrar XML-utdata.
   * Ett tomt `javax.xml.rpc.holders.LongHolder`-objekt som fylls i av metoden. Det här argumentet lagrar antalet sidor i formuläret.
   * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden. Det här argumentet lagrar språkets värde.
   * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden. Det här argumentet lagrar HTML-återgivningsvärdet som används.
   * Ett tomt `com.adobe.idp.services.holders.FormsResultHolder`-objekt som innehåller resultatet av den här åtgärden.

   Metoden `(Deprecated) renderHTMLForm` fyller i objektet `com.adobe.idp.services.holders.FormsResultHolder` som skickas som det sista argumentvärdet med en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `FormResult`-objekt genom att hämta värdet för `com.adobe.idp.services.holders.FormsResultHolder`-objektets `value`-datamedlem.
   * Skapa ett `BLOB`-objekt som innehåller formulärdata genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Hämta innehållstypen för objektet `BLOB` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `BLOB`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa en bytearray och fyll i den genom att anropa `BLOB`-objektets `getBinaryData`-metod. Den här aktiviteten tilldelar innehållet i objektet `FormsResult` till bytearrayen.
   * Anropa `javax.servlet.http.HttpServletResponse`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Återger Forms som HTML](#rendering-forms-as-html)

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
