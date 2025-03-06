---
title: Återger Forms efter värde
description: Använd Forms API (Java) för att återge ett formulär utifrån värde med Java API och Web Service API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/rendering_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
exl-id: 8ad8cf67-3e90-4790-a063-099134b377a3
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1822'
ht-degree: 0%

---

# Återger Forms efter värde {#rendering-forms-by-value}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Vanligtvis skickas en formulärdesign som har skapats i Designer med referens till tjänsten Forms. Formulärdesigner kan vara stora och därför är det mer effektivt att skicka dem med referens för att undvika att behöva konvertera byte för formulärdesign efter värde. Forms-tjänsten kan även cachelagra formulärdesignen så att den inte behöver läsa formulärdesignen kontinuerligt när den cache-lagras.

Om en formulärdesign innehåller ett UUID-attribut cache-lagras den. UUID-värdet är unikt för alla formulärdesigner och används för att unikt identifiera ett formulär. När du återger ett formulär efter värde bör formuläret bara cachelagras när det används upprepade gånger. Om formuläret inte används upprepade gånger och måste vara unikt, kan du undvika att cache-lagra formuläret med cachelagringsalternativ som har angetts med AEM Forms API.

Forms-tjänsten kan också lösa platsen för det länkade innehållet i formulärdesignen. Länkade bilder som refereras inifrån formulärdesignen är till exempel relativa URL-adresser. Länkat innehåll antas alltid vara relativt till formulärdesignens plats. Att lösa länkat innehåll är därför en fråga om att fastställa dess plats genom att använda den relativa sökvägen på den absoluta formulärdesignplatsen.

I stället för att skicka en formulärdesign med referens kan du skicka en formulärdesign med värde. Att skicka en formulärdesign med värde är effektivt när en formulärdesign skapas dynamiskt, det vill säga när ett klientprogram genererar XML-koden som skapar en formulärdesign under körning. I det här fallet lagras inte en formulärdesign i en fysisk databas eftersom den lagras i minnet. När du dynamiskt skapar en formulärdesign vid körning och skickar den med värde, kan du cachelagra formuläret och förbättra prestanda för Forms-tjänsten.

**Begränsningar för att skicka ett formulär med värde**

Följande begränsningar gäller när en formulärdesign skickas med värde:

* Inget relativt länkat innehåll kan finnas i formulärdesignen. Alla bilder och fragment måste vara inbäddade i formulärdesignen eller refereras till absolut.
* Det går inte att utföra beräkningar på serversidan efter att formuläret har återgetts. Om formuläret skickas tillbaka till Forms-tjänsten extraheras data och returneras utan serverberäkningar.
* Eftersom HTML bara kan använda länkade bilder vid körning går det inte att generera HTML med inbäddade bilder. Detta beror på att Forms-tjänsten stöder inbäddade bilder med HTML genom att hämta bilderna från en refererad formulärdesign. Eftersom en formulärdesign som skickas med värde inte har någon referensplats, kan inbäddade bilder inte extraheras när HTML-sidan visas. Därför måste bildreferenser vara absoluta sökvägar för att kunna återges i HTML.

>[!NOTE]
>
>Även om du kan återge olika typer av formulär efter värde (t.ex. HTML-formulär eller formulär som innehåller användarrättigheter) behandlas återgivningen av interaktiv PDF forms i det här avsnittet.

>[!NOTE]
>
>Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Sammanfattning av steg {#summary-of-steps}

Så här återger du ett formulär efter värde:

1. Inkludera projektfiler.
1. Skapa ett Forms Client API-objekt.
1. Referera formulärdesignen.
1. Återge ett formulär efter värde.
1. Skriv formulärdataströmmen till klientens webbläsare.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett Forms Client API-objekt**

Innan du kan importera data programmatiskt till ett PDF-formulär-klient-API måste du skapa en dataintegreringstjänstklient. När du skapar en tjänstklient definierar du de anslutningsinställningar som krävs för att anropa en tjänst.

**Referera formulärdesignen**

När du återger ett formulär efter värde måste du skapa ett `com.adobe.idp.Document`-objekt som innehåller den formulärdesign som ska återges. Du kan referera till en befintlig XDP-fil eller så kan du dynamiskt skapa en formulärdesign vid körning och fylla i en `com.adobe.idp.Document` med dessa data.

>[!NOTE]
>
>Det här avsnittet och motsvarande snabbstart refererar till en befintlig XDP-fil.

**Återge ett formulär efter värde**

Om du vill återge ett formulär med hjälp av ett värde skickar du en `com.adobe.idp.Document`-instans som innehåller formulärdesignen till återgivningsmetodens `inDataDoc`-parameter (kan vara någon av `FormsServiceClient`-objektets återgivningsmetoder som `renderPDFForm`, `(Deprecated) renderHTMLForm` och så vidare). Det här parametervärdet är vanligtvis reserverat för data som sammanfogas med formuläret. På samma sätt skickar du ett tomt strängvärde till parametern `formQuery`. I vanliga fall kräver den här parametern ett strängvärde som anger namnet på formulärdesignen.

>[!NOTE]
>
>Om du vill visa data i formuläret måste data anges i elementet `xfa:datasets`. Mer information om XFA-arkitekturen finns på [https://www.pdfa.org/norm-refs/XFA-3_3.pdf](https://www.pdfa.org/norm-refs/XFA-3_3.pdf).

**Skriv formulärdataströmmen till klientwebbläsaren**

När Forms-tjänsten återger ett formulär efter värde returneras en formulärdataström som du måste skriva till klientens webbläsare. När formuläret skrivs till webbläsaren visas det för användaren.

**Se även**

[Återge ett formulär med hjälp av Java API](#render-a-form-by-value-using-the-java-api)

[Återge ett formulär utifrån värde med hjälp av webbtjänstens API](#render-a-form-by-value-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Forms Service API](/help/forms/developing/forms-service-api-quick-starts.md#forms-service-api-quick-starts)

[Skicka dokument till Forms](/help/forms/developing/passing-documents-forms-service.md)

[Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md)

## Återge ett formulär med hjälp av Java API {#render-a-form-by-value-using-the-java-api}

Återge ett formulär med hjälp av Forms API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg.

1. Skapa ett Forms Client API-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till formulärdesignen

   * Skapa ett `java.io.FileInputStream`-objekt som representerar formulärdesignen som ska återges med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för XDP-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Återge ett formulär efter värde

   Anropa `FormsServiceClient`-objektets `renderPDFForm`-metod och skicka följande värden:

   * Ett tomt strängvärde. (Den här parametern kräver vanligtvis ett strängvärde som anger formulärdesignens namn.)
   * Ett `com.adobe.idp.Document`-objekt som innehåller formulärdesignen. Normalt är det här parametervärdet reserverat för data som sammanfogas med formuläret.
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ. Det här är en valfri parameter och du kan ange `null` om du inte vill ange körningsalternativ.
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret.

   Metoden `renderPDFForm` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som kan skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `com.adobe.idp.Document`-objekt genom att anropa metoden `getOutputContent` för `FormsResult`-objektet.
   * Hämta innehållstypen för objektet `com.adobe.idp.Document` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `com.adobe.idp.Document`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
   * Skapa en bytearray och tilldela storleken på `InputStream`-objektet. Anropa `InputStream`-objektets `available`-metod för att hämta storleken på `InputStream`-objektet.
   * Fyll i bytearrayen med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
   * Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Återger Forms efter värde](/help/forms/developing/rendering-forms.md)

[Snabbstart (SOAP-läge): Återge efter värde med Java API](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-rendering-by-value-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Återge ett formulär utifrån värde med hjälp av webbtjänstens API {#render-a-form-by-value-using-the-web-service-api}

Återge ett formulär med hjälp av Forms API (webbtjänsten):

1. Inkludera projektfiler

   * Skapa Java-proxyklasser som använder Forms tjänst-WSDL.
   * Inkludera Java-proxyklasserna i klassökvägen.

1. Skapa ett Forms Client API-objekt

   Skapa ett `FormsService`-objekt och ange autentiseringsvärden.

1. Referera till formulärdesignen

   * Skapa ett `java.io.FileInputStream`-objekt med hjälp av dess konstruktor. Skicka ett strängvärde som anger platsen för XDP-filen.
   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra ett PDF-dokument som är krypterat med ett lösenord.
   * Skapa en bytearray som lagrar innehållet i objektet `java.io.FileInputStream`. Du kan bestämma storleken på bytearrayen genom att hämta `java.io.FileInputStream`-objektets storlek med hjälp av dess `available`-metod.
   * Fyll i bytearrayen med strömdata genom att anropa `java.io.FileInputStream`-objektets `read`-metod och skicka bytearrayen.
   * Fyll i objektet `BLOB` genom att anropa dess `setBinaryData`-metod och skicka bytearrayen.

1. Återge ett formulär efter värde

   Anropa `FormsService`-objektets `renderPDFForm`-metod och skicka följande värden:

   * Ett tomt strängvärde. (Den här parametern kräver vanligtvis ett strängvärde som anger formulärdesignens namn.)
   * Ett `BLOB`-objekt som innehåller formulärdesignen. Normalt är det här parametervärdet reserverat för data som sammanfogas med formuläret.
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ. Det här är en valfri parameter och du kan ange `null` om du inte vill ange körningsalternativ.
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret.
   * Ett tomt `com.adobe.idp.services.holders.BLOBHolder`-objekt som fylls i av metoden. Detta används för att lagra det återgivna PDF-formuläret.
   * Ett tomt `javax.xml.rpc.holders.LongHolder`-objekt som fylls i av metoden. (Detta argument lagrar antalet sidor i formuläret.)
   * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden. (Det här argumentet lagrar språkets värde.)
   * Ett tomt `com.adobe.idp.services.holders.FormsResultHolder`-objekt som innehåller resultatet av den här åtgärden.

   Metoden `renderPDFForm` fyller i objektet `com.adobe.idp.services.holders.FormsResultHolder` som skickas som det sista argumentvärdet med en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `FormResult`-objekt genom att hämta värdet för `com.adobe.idp.services.holders.FormsResultHolder`-objektets `value`-datamedlem.
   * Skapa ett `BLOB`-objekt som innehåller formulärdata genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Hämta innehållstypen för objektet `BLOB` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `BLOB`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa en bytearray och fyll i den genom att anropa `BLOB`-objektets `getBinaryData`-metod. Den här aktiviteten tilldelar innehållet i objektet `FormsResult` till bytearrayen.
   * Anropa `javax.servlet.http.HttpServletResponse`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Återger Forms efter värde](#rendering-forms-by-value)

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
