---
title: Rendering Rights-aktiverad Forms
description: Använd tjänsten Forms för att återge formulär som har användarrättigheter. Du kan återge behörighetsaktiverade formulär med Java API och Web Service API.
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
exl-id: 70b2d1aa-6fcd-461d-b628-e82ddf266f48
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# Rendering Rights-aktiverad Forms {#rendering-rights-enabled-forms}

Forms-tjänsten kan återge formulär som har användarrättigheter. Användningsrättigheterna gäller funktioner som är tillgängliga som standard i Acrobat men inte i Adobe Reader, t.ex. möjligheten att lägga till kommentarer i ett formulär eller att fylla i formulärfält och spara formuläret. Forms med användarrättigheter kallas för rättighetsaktiverade formulär. En användare som öppnar ett rättighetsaktiverat formulär i Adobe Reader kan utföra åtgärder som är aktiverade för det formuläret.

Om du vill lägga till användarrättigheter i ett formulär måste Acrobat Reader DC-tilläggstjänsten ingå i installationen av AEM-formulär. Dessutom måste du ha en giltig autentiseringsuppgift som gör att du kan lägga in användarrättigheter i PDF-dokument. Det innebär att du måste konfigurera Acrobat Reader DC-tilläggstjänsten innan du kan återge ett aktiverat formulär. (Se [Om Acrobat Reader DC-tilläggstjänsten](/help/forms/developing/assigning-usage-rights.md#about-the-acrobat-reader-dc-extensions-service).)

>[!NOTE]
>
>Om du vill återge ett formulär som innehåller användningsrättigheter måste du använda en XDP-fil som indata, inte en PDF-fil. Om du använder en PDF-fil som indata återges formuläret fortfarande, men det kommer inte att vara ett aktiverat formulär.

>[!NOTE]
>
>Du kan inte fylla i ett formulär i förväg med XML-data när du anger följande användningsbehörighet: `enableComments`, `enableCommentsOnline`, `enableEmbeddedFiles` eller `enableDigitalSignatures`. (Se [Förifyll Forms med flödeslayouter](/help/forms/developing/prepopulating-forms-flowable-layouts.md).)

>[!NOTE]
>
>Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Sammanfattning av steg {#summary-of-steps}

Så här återger du ett rättighetsaktiverat formulär:

1. Inkludera projektfiler.
1. Skapa ett Forms Client API-objekt.
1. Ange körningsalternativ för användningsrättigheter.
1. Återge ett rättighetsaktiverat formulär.
1. Skriv det aktiverade formuläret i webbläsaren.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett Forms Client API-objekt**

Innan du programmässigt kan utföra en API-åtgärd för Forms-tjänstklienten måste du skapa en Forms-tjänstklient.

**Ange körningsalternativ för användningsrättigheter**

Ange körningsalternativ för användningsrättigheter för att återge ett rättighetsaktiverat formulär. Ange aliaset för de autentiseringsuppgifter som används för att tillämpa användningsrättigheter på ett formulär. När du har angett aliasvärdet anger du vilken användningsbehörighet som ska gälla för formuläret.

**Återge ett rättighetsaktiverat formulär**

Om du vill återge ett rättighetsaktiverat formulär använder du samma programlogik som att återge ett formulär utan användarrättigheter. Den enda skillnaden är att du måste se till att körningsalternativen för användningsrättigheter inkluderas i programlogiken.

>[!NOTE]
>
>När du återger ett rättighetsaktiverat formulär med Forms webbtjänst-API:t kan du inte bifoga filer till formuläret.

**Skriv formulärdataströmmen till klientwebbläsaren**

När Forms-tjänsten återger ett rättighetsaktiverat formulär returneras en formulärdataström som du måste skriva till klientens webbläsare. När formuläret har skrivits till webbläsaren visas det för användaren. En användare som visar det aktiverade formuläret i Adobe Reader kan utföra åtgärder som är aktiverade för det formuläret.

**Se även**

[Återge rättighetsaktiverade formulär med Java API](#render-rights-enabled-forms-using-the-java-api)

[Återge rättighetsaktiverade formulär med webbtjänstens API](#render-rights-enabled-forms-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Forms Service API](/help/forms/developing/forms-service-api-quick-starts.md#forms-service-api-quick-starts)

[Rendering Interactive PDF forms](/help/forms/developing/rendering-interactive-pdf-forms.md)

[Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md)

### Återge rättighetsaktiverade formulär med Java API {#render-rights-enabled-forms-using-the-java-api}

Återge ett rättighetsaktiverat formulär med Forms API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg.

1. Skapa ett Forms Client API-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Ange körningsalternativ för användningsrättigheter

   * Skapa ett `ReaderExtensionSpec`-objekt med hjälp av dess konstruktor.
   * Ange alias för autentiseringsuppgiften genom att anropa `ReaderExtensionSpec`-objektets `setReCredentialAlias`-metod och ange ett strängvärde som representerar aliasvärdet.
   * Ställ in varje användningsbehörighet genom att anropa motsvarande metod som tillhör objektet `ReaderExtensionSpec`. Du kan dock bara ange användarbehörighet om du kan göra det med de referenser du anger. Det innebär att du inte kan ange en användningsbehörighet om du inte kan ange den i inloggningsuppgifterna. Till exempel. Om du vill ange användarbehörighet som gör att en användare kan fylla i formulärfält och spara formuläret, anropar du `setReFillIn`-metoden för `ReaderExtensionSpec`-objektet och skickar `true`.

   >[!NOTE]
   >
   >Du behöver inte anropa `ReaderExtensionSpec`-objektets `setReCredentialPassword`-metod. Den här metoden används inte av Forms-tjänsten.

1. Återge ett rättighetsaktiverat formulär

   Anropa `FormsServiceClient`-objektets `renderPDFFormWithUsageRights`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `com.adobe.idp.Document`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du ett tomt `com.adobe.idp.Document`-objekt.
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ.
   * Ett `ReaderExtensionSpec`-objekt som lagrar körningsalternativ för användningsrättigheter.
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten.

   Metoden `renderPDFFormWithUsageRights` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `com.adobe.idp.Document`-objekt genom att anropa metoden `getOutputContent` för `FormsResult`-objektet.
   * Hämta innehållstypen för objektet `com.adobe.idp.Document` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `com.adobe.idp.Document`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
   * Skapa en bytearray som fyller i den med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
   * Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Snabbstart (SOAP-läge): Återge ett rättighetsaktiverat formulär med Java API](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-rendering-a-rights-enabled-form-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Återge rättighetsaktiverade formulär med webbtjänstens API {#render-rights-enabled-forms-using-the-web-service-api}

Återge ett rättighetsaktiverat formulär med Forms API (webbtjänst):

1. Inkludera projektfiler

   * Skapa Java-proxyklasser som använder Forms tjänst-WSDL.
   * Inkludera Java-proxyklasserna i klassökvägen.

1. Skapa ett Forms Client API-objekt

   Skapa ett `FormsService`-objekt och ange autentiseringsvärden.

1. Ange körningsalternativ för användningsrättigheter

   * Skapa ett `ReaderExtensionSpec`-objekt med hjälp av dess konstruktor.
   * Ange alias för autentiseringsuppgiften genom att anropa `ReaderExtensionSpec`-objektets `setReCredentialAlias`-metod och ange ett strängvärde som representerar aliasvärdet.
   * Ställ in varje användningsbehörighet genom att anropa motsvarande metod som tillhör objektet `ReaderExtensionSpec`. Du kan dock bara ange användarbehörighet om du kan göra det med de referenser du anger. Det innebär att du inte kan ange en användningsbehörighet om du inte kan ange den i inloggningsuppgifterna. Om du vill ange användarbehörighet som gör att en användare kan fylla i formulärfält och spara formuläret, anropar du `setReFillIn`-metoden för `ReaderExtensionSpec`-objektet och skickar `true`.

1. Återge ett rättighetsaktiverat formulär

   Anropa `FormsService`-objektets `renderPDFFormWithUsageRights`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `BLOB`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data med formuläret måste du skicka ett `BLOB`-objekt som är baserat på en tom XML-datakälla. Du kan inte skicka ett `BLOB`-objekt som är null, annars genereras ett undantag.
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ.
   * Ett `ReaderExtensionSpec`-objekt som lagrar körningsalternativ för användningsrättigheter.
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten.

   Metoden `renderPDFFormWithUsageRights` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `BLOB`-objekt som innehåller formulärdata genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Hämta innehållstypen för objektet `BLOB` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `BLOB`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa en bytearray och fyll i den genom att anropa `BLOB`-objektets `getBinaryData`-metod. Den här aktiviteten tilldelar innehållet i objektet `FormsResult` till bytearrayen.
   * Anropa `javax.servlet.http.HttpServletResponse`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Rendering Rights-aktiverad Forms](#rendering-rights-enabled-forms)

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
