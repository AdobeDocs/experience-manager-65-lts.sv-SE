---
title: Återger Forms på klienten
description: Optimera leveransen av PDF-material och förbättra Forms-tjänstens förmåga att hantera nätverksbelastningen med klientåtergivningsfunktionen i Acrobat eller Adobe Reader
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/rendering_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
exl-id: cd0a9205-5ccc-420b-9245-98f8bd7d6c9f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1690'
ht-degree: 0%

---

# Återger Forms på klienten {#rendering-forms-at-the-client}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

## Återger Forms på klienten {#rendering-forms-at-the-client-inner}

Du kan optimera leveransen av PDF-material och förbättra Forms-tjänstens förmåga att hantera nätverksbelastningen genom att använda klientåtergivningsfunktionen i Acrobat eller Adobe Reader. Den här processen kallas att återge ett formulär på klienten. Om du vill återge ett formulär på klienten måste klientenheten (vanligtvis en webbläsare) använda Acrobat 7.0 eller Adobe Reader 7.0 eller senare.

Ändringar i ett formulär som är ett resultat av skriptkörning på serversidan återspeglas inte i ett formulär som återges på klienten såvida inte rotdelformuläret innehåller attributet `restoreState` som är inställt på `auto`. Mer information om attributet finns i [Forms Designer.](https://www.adobe.com/go/learn_aemforms_designer_63)

>[!NOTE]
>
>Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Så här återger du ett formulär på klienten:

1. Inkludera projektfiler.
1. Skapa ett Forms Client API-objekt.
1. Ange körningsalternativ för klientåtergivning.
1. Återge ett formulär på klienten.
1. Skriv formuläret till webbläsaren.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett Forms Client API-objekt**

Innan du programmässigt kan utföra en API-åtgärd för Forms-tjänstklienten måste du skapa en Forms-tjänstklient. Om du använder Java API skapar du ett `FormsServiceClient`-objekt. Om du använder Forms webbtjänst-API:t skapar du ett `FormsService`-objekt.

**Ange körningsalternativ för klientåtergivning**

Ange körtidsalternativet för klientåtergivning för att återge ett formulär på klienten genom att ställa in körningsalternativet `RenderAtClient` på `true`. Detta resulterar i att formuläret levereras till klientenheten där det återges. Om `RenderAtClient` är `auto` (standardvärdet) avgör formulärdesignen om formuläret återges hos klienten. Formulärdesignen måste vara en formulärdesign med flödeslayout.

Ett valfritt körningsalternativ som du kan ange är alternativet `SeedPDF`. Alternativet `SeedPDF` kombinerar PDF-behållaren (dirigerat PDF-dokument) med formulärdesignen och XML-data. Både formulärdesignen och XML-data levereras till Acrobat eller Adobe Reader, där formuläret återges. Alternativet `SeedPDF` kan användas när klientdatorn inte har teckensnitt som används i formuläret, till exempel när en slutanvändare inte har licens att använda ett teckensnitt som formulärägaren har licens att använda.

Du kan använda Designer för att skapa en enkel dynamisk PDF-fil som kan användas som en PDF-startfil. Följande steg krävs för att utföra den här uppgiften:

1. Ange om du behöver bädda in teckensnitt i startfilen för PDF. Startfilen för PDF måste innehålla ytterligare teckensnitt som krävs för att formuläret ska kunna återges. När du bäddar in teckensnitt i startfilen för PDF måste du se till att du inte bryter mot några teckensnittslicensavtal. I Designer kan du bestämma om du ska kunna bädda in teckensnitt som är juridiskt bindande. Om det finns teckensnitt som du inte kan bädda in i formuläret visas ett meddelande i Designer med en lista över de teckensnitt som du inte kan bädda in. Det här meddelandet visas inte i Designer för statiska PDF-dokument.
1. Om du skapar dirigerad PDF-fil i Designer bör du åtminstone lägga till ett textfält som innehåller ett meddelande. Meddelandet ska riktas till användare av tidigare versioner av Adobe Reader som uppger att de behöver Acrobat 7.0 eller senare eller Adobe Reader 7.0 eller senare för att kunna läsa dokumentet.
1. Spara PDF-startfilen som en dynamisk PDF-fil med filnamnstillägget PDF.

>[!NOTE]
>
>Du behöver inte definiera startalternativet för PDF vid körning för att återge ett formulär på klienten. Om du inte anger någon dirigerad PDF skapar Forms-tjänsten en skalpdf som inte innehåller COS-objekt, men som innehåller en PDF-wrapper med det faktiska XDP-innehållet inbäddat. Stegen i det här avsnittet anger inte startalternativet för PDF. Mer information om COS-objekt finns i referenshandboken för Adobe PDF.

**Återge ett formulär på klienten**

Om du vill återge ett formulär på klienten måste du se till att alternativen för klientåtergivning vid körning inkluderas i programlogiken för att återge ett formulär.

**Skriv formulärdataströmmen till klientwebbläsaren**

Forms skapar en formulärdataström som du måste skriva till klientens webbläsare. När formuläret skrivs till webbläsaren återges det av Acrobat 7.0 eller Adobe Reader 7.0 eller senare och är synligt för användaren.

**Se även**

[Återge ett formulär på klienten med Java API](#render-a-form-at-the-client-using-the-java-api)

[Återge ett formulär på klienten med hjälp av webbtjänstens API](#render-a-form-at-the-client-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Forms Service API](/help/forms/developing/forms-service-api-quick-starts.md#forms-service-api-quick-starts)

[Skicka dokument till Forms](/help/forms/developing/passing-documents-forms-service.md)

[Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md)

### Återge ett formulär på klienten med Java API {#render-a-form-at-the-client-using-the-java-api}

Återge ett formulär på klienten med Forms API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg.

1. Skapa ett Forms Client API-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Ange körningsalternativ för klientåtergivning

   * Skapa ett `PDFFormRenderSpec`-objekt med hjälp av dess konstruktor.
   * Ange körningsalternativet `RenderAtClient` genom att anropa `PDFFormRenderSpec`-objektets `setRenderAtClient`-metod och skicka uppräkningsvärdet `RenderAtClient.Yes`.

1. Återge ett formulär på klienten

   Anropa `FormsServiceClient`-objektets `renderPDFForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett AEM Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `com.adobe.idp.Document`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du ett tomt `com.adobe.idp.Document`-objekt.
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ som krävs för att återge ett formulär på klienten.
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten för att återge ett formulär.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret.

   Metoden `renderPDFForm` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som måste skrivas till klientens webbläsare.

1. Skriv formulärdataströmmen till klientens webbläsare

   * Skapa ett `com.adobe.idp.Document`-objekt genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Hämta innehållstypen för objektet `com.adobe.idp.Document` genom att anropa dess `getContentType`-metod.
   * Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `com.adobe.idp.Document`.
   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
   * Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
   * Skapa en bytearray och fyll i den med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
   * Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**

[Snabbstart (SOAP-läge): Återge ett formulär på klienten med Java API](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-rendering-a-form-at-the-client-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Återge ett formulär på klienten med hjälp av webbtjänstens API {#render-a-form-at-the-client-using-the-web-service-api}

Återge ett formulär på klienten med Forms API (webbtjänst):

1. Inkludera projektfiler

   * Skapa Java-proxyklasser som använder Forms tjänst-WSDL.
   * Inkludera Java-proxyklasserna i klassökvägen.

1. Skapa ett Forms Client API-objekt

   Skapa ett `FormsService`-objekt och ange autentiseringsvärden.

1. Ange körningsalternativ för klientåtergivning

   * Skapa ett `PDFFormRenderSpec`-objekt med hjälp av dess konstruktor.
   * Ange körningsalternativet `RenderAtClient` genom att anropa `PDFFormRenderSpec`-objektets `setRenderAtClient`-metod och skicka strängvärdet `RenderAtClient.Yes`.

1. Återge ett formulär på klienten

   Anropa `FormsService`-objektets `renderPDFForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget. Om du refererar till en formulärdesign som ingår i ett Forms-program måste du ange den fullständiga sökvägen, till exempel `Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`.
   * Ett `BLOB`-objekt som innehåller data som ska sammanfogas med formuläret. Om du inte vill sammanfoga data skickar du `null`. (Se [Förifyll Forms med flödeslayouter](/help/forms/developing/prepopulating-forms-flowable-layouts.md).)
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ som krävs för att återge ett formulär på klienten.
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret.
   * Ett tomt `com.adobe.idp.services.holders.BLOBHolder`-objekt som fylls i av metoden. Den här parametern används för att lagra det återgivna PDF-formuläret.
   * Ett tomt `javax.xml.rpc.holders.LongHolder`-objekt som fylls i av metoden. (Det här argumentet lagrar antalet sidor i formuläret).
   * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden. (Det här argumentet lagrar språkets värde).
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

[Återger Forms på klienten](#rendering-forms-at-the-client)

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
