---
title: Konvertera PDF till PostScript- och bildfiler
description: Använd tjänsten Convert PDF för att konvertera PDF-dokument till PostScript och till flera bildformat (JPEG, JPEG 2000, PNG och TIFF) med Java API och Web Service API.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
exl-id: 509b9e53-9af4-4916-afba-2db358ec0764
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2774'
ht-degree: 0%

---

# Konvertera PDF till PostScript och bildfiler {#converting-pdf-to-postscript-andimage-files}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

**Om Konvertera PDF-tjänst**

Med tjänsten Convert PDF kan du konvertera PDF-dokument till PostScript och flera bildformat (JPEG, JPEG 2000, PNG och TIFF). Att konvertera ett PDF-dokument till PostScript är användbart för oövervakad serverbaserad utskrift på alla PostScript-skrivare. Att konvertera ett PDF-dokument till en flersidig TIFF-fil är praktiskt när man arkiverar dokument i content management-system som inte stöder PDF-dokument.

Du kan utföra följande uppgifter med hjälp av tjänsten Konvertera PDF:

* Konvertera PDF-dokument till PostScript.
* Konvertera PDF-dokument till bildformat.

>[!NOTE]
>
>Mer information om tjänsten Konvertera PDF finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Konvertera PDF-dokument till PostScript {#converting-pdf-documents-to-postscript}

I det här avsnittet beskrivs hur du kan använda Konvertera PDF Service API (Java och webbtjänst) för att programmässigt konvertera PDF-dokument till PostScript-filer. PDF-dokumentet som konverteras till en PostScript-fil måste vara ett icke-interaktivt PDF-dokument. Det innebär att om du försöker konvertera ett interaktivt PDF-dokument till en PostScript-fil genereras ett undantag.

>[!NOTE]
>
>Mer information om tjänsten Konvertera PDF finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Så här konverterar du ett PDF-dokument till en PostScript-fil:

1. Inkludera projektfiler.
1. Skapa en Konvertera PDF-tjänstklient.
1. Referera till PDF-dokumentet som ska konverteras till en PostScript-fil.
1. Ange alternativ för konvertering vid körning.
1. Konvertera PDF-dokumentet till en PostScript-fil.
1. Spara PostScript-filen.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

**Skapa en Konvertera PDF-klient**

Innan du programmässigt kan utföra en Konvertera PDF-tjänståtgärd måste du skapa en Konvertera PDF-tjänstklient. Om du använder Java API skapar du ett `ConvertPdfServiceClient`-objekt. Om du använder webbtjänstens API skapar du ett `ConvertPDFServiceService`-objekt.

I det här avsnittet används webbtjänstfunktioner som introducerades i AEM Forms. För att få tillgång till nya funktioner måste du skapa proxyobjektet med attributet `lc_version`. (Se &quot;Åtkomst till nya funktioner med webbtjänster&quot; i [Anropa AEM Forms med webbtjänster](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-web-services).)

**Referera till PDF-dokumentet som ska konverteras till en PostScript-fil**

Referera till det PDF-dokument som du vill konvertera till en PostScript-fil. Som tidigare nämnts i det här avsnittet måste PDF-dokumentet vara ett icke-interaktivt PDF-dokument. Om du försöker konvertera ett interaktivt PDF-dokument till en PostScript-fil genereras ett undantag.

**Ange alternativ för konvertering vid körning**

När du konverterar ett PDF-dokument till en PostScript-fil kan du definiera körningsalternativ som anger vilken PostScript-typ som skapas. Du kan till exempel definiera en nivå 3-PostScript-fil.

Vanligtvis återspeglar den genererade PostScript-filen storleken på PDF-indatadokumentet. Om du väljer alternativet `ShrinkToFit` (vilket minskar utdata från PostScript-filen så att den passar på sidan) kommer du inte att se någon skillnad mellan PDF-indatadokumentet och den genererade PostScript-filen. Alternativet `ShrinkToFit` aktiveras bara om du väljer att skriva ut med en mindre sidstorlek än PDF-indatadokumentet. Om du vill välja en mindre sidstorlek definierar du alternativet `PageSize`. Vi rekommenderar dessutom att du ställer in alternativet `RotateAndCenter` på `true` för att få rätt PostScript-utdata.

Om du väljer alternativet `ExpandToFit` (som utökar utdata från PostScript-filen så att den passar på sidan), aktiveras det bara om du väljer att skriva ut med en större sidstorlek än PDF-indatadokumentet. Om du vill välja en större sidstorlek definierar du alternativet `PageSize`. Vi rekommenderar dessutom att du ställer in alternativet `RotateAndCenter` på `true` för att få rätt PostScript-utdata.

>[!NOTE]
>
>Mer information om de körningsvärden du kan ange finns i klassreferensen `ToPSOptionsSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

**Konvertera PDF-dokumentet till en PostScript-fil**

När du har skapat tjänstklienten och angett körningsalternativ kan du starta PostScript-konverteringsåtgärden. Den här åtgärden kräver information om dokumentet som ska konverteras, inklusive den önskade PostScript-nivån för måldokumentet.

**Spara PostScript-filen**

När du har konverterat PDF-dokumentet till PostScript kan du spara utdata som en PostScript-fil.

**Se även**

[Konvertera ett PDF-dokument till PS med Java API](converting-pdf-postscript-image-files.md#convert-a-pdf-document-to-ps-using-the-java-api)

[Konvertera ett PDF-dokument till PS med webbtjänstens API](converting-pdf-postscript-image-files.md#convert-a-pdf-document-to-ps-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Konvertera snabbstart för PDF Service API](/help/forms/developing/convert-pdf-service-java-api.md#convert-pdf-service-java-api-quick-start-soap)

### Konvertera ett PDF-dokument till PS med Java API {#convert-a-pdf-document-to-ps-using-the-java-api}

Konvertera ett PDF-dokument till PostScript med hjälp av Konvertera PDF Service API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-convertpdf-client.jar, i Java-projektets klassökväg.

1. Skapa en Convert PDF-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `ConvertPdfServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till PDF-dokumentet som ska konverteras till en PostScript-fil.

   * Skapa ett `java.io.FileInputStream`-objekt med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för det PDF-dokument som ska konverteras.
   * Skapa ett `com.adobe.idp.Document`-objekt som lagrar PDF-dokumentet med konstruktorn `com.adobe.idp.Document`. Skicka `java.io.FileInputStream`-objektet som innehåller PDF-dokumentet.

1. Ange alternativ för konvertering vid körning.

   * Skapa ett `ToPSOptionsSpec`-objekt genom att anropa dess konstruktor.
   * Ange körningsalternativ genom att anropa en lämplig metod som tillhör objektet `ToPSOptionsSpec`. Om du till exempel vill definiera den PostScript-nivå som skapas, anropar du `ToPSOptionsSpec`-objektets `setPsLevel`-metod och skickar ett `PSLevel`-uppräkningsvärde som anger PostScript-nivån. Mer information om alla körningsvärden som du kan ange finns i klassreferensen `ToPSOptionsSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

1. Konvertera PDF-dokumentet till en PostScript-fil.

   Anropa `toPS2`-metoden för objektet `ConvertPdfServiceClient` och skicka följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar PDF-dokumentet som ska konverteras till en PostScript-fil.
   * Ett `ToPSOptionsSpec`-objekt som anger körningsalternativ för PostScript.

   Metoden `toPS2` returnerar ett `Document`-objekt som innehåller det nya PostScript-dokumentet.

1. Spara PostScript-filen.

   * Skapa ett `java.io.File`-objekt och kontrollera att filnamnstillägget är .ps.
   * Anropa `Document`-objektets `copyToFile`-metod för att kopiera innehållet i `Document`-objektet till filen (se till att du använder det `Document`-objekt som returnerades av metoden `toPS2`).

**Se även**

[Sammanfattning av steg](converting-pdf-postscript-image-files.md#summary-of-steps)

[Snabbstart (SOAP-läge): Konvertera ett PDF-dokument till PostScript med Java API](/help/forms/developing/convert-pdf-service-java-api.md#quick-start-soap-mode-converting-a-pdf-document-to-postscript-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Konvertera ett PDF-dokument till PS med webbtjänstens API {#convert-a-pdf-document-to-ps-using-the-web-service-api}

Konvertera ett PDF-dokument till PostScript med hjälp av API:t för konvertering av PDF-tjänster (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/ConvertPDFService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa en Convert PDF-klient.

   * Skapa ett `ConvertPdfServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `ConvertPdfServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/ConvertPDFService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Ange dock `?blob=mtom`.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `ConvertPdfServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `ConvertPdfServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `ConvertPdfServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till PDF-dokumentet som ska konverteras till en PostScript-fil.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra ett PDF-dokument som konverteras till en PostScript-fil.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det PDF-dokument som ska konverteras och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Ange alternativ för konvertering vid körning.

   * Skapa ett `ToPSOptionsSpec`-objekt genom att anropa dess konstruktor.
   * Ange körningsalternativ genom att tilldela ett värde till `ToPSOptionsSpec`-objektets datamedlem. Om du till exempel vill definiera den PostScript-nivå som skapas tilldelar du `PSLevel`-uppräkningsvärdet till `ToPSOptionsSpec`-objektets `psLevel`-datamedlem.

1. Konvertera PDF-dokumentet till en PostScript-fil.

   Anropa `GeneratePDFServiceService`-objektets `toPS2`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar PDF-dokumentet som ska konverteras till en PostScript-fil
   * Ett `ToPSOptionsSpec`-objekt som anger körningsalternativ

   När konverteringen är klar kan du extrahera de binära data som representerar PostScript-dokumentet genom att öppna objektets `MTOM` -egenskap för `BLOB`. Detta returnerar en bytearray som du kan skriva ut till en PostScript-fil.

1. Spara PostScript-filen.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för PS-filen.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som returnerades av metoden `encryptPDFUsingPassword`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-fält.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till PostScript-filen genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](converting-pdf-postscript-image-files.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Konvertera PDF-dokument till bildformat {#converting-pdf-documents-to-image-formats}

Du kan använda tjänsten Convert PDF för att programmässigt konvertera PDF-dokument till bildformat, som JPEG, JPEG 2000, TIFF och PNG. Genom att konvertera ett PDF-dokument till en bildfil kan du använda PDF-dokumentet som en bildfil. Du kan till exempel placera bilden i ett innehållshanteringssystem för företag för lagring.

När du konverterar ett PDF-dokument till en bild skapas en separat bild för varje sida i dokumentet med tjänsten Konvertera PDF. Om dokumentet innehåller 20 sidor skapas alltså 20 bildfiler. När du konverterar ett PDF-dokument till ett bildformat kan du skapa enskilda bilder för varje sida i PDF-dokumentet eller en enda bildfil för hela PDF-dokumentet.

>[!NOTE]
>
>Mer information om tjänsten Konvertera PDF finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-1}

Så här konverterar du ett PDF-dokument till någon av de typer som stöds:

1. Inkludera projektfiler.
1. Skapa en Konvertera PDF-tjänstklient.
1. Hämta det PDF-dokument som ska konverteras.
1. Ange körningsalternativ.
1. Konvertera PDF till en bild.
1. Hämta bildfilerna från en samling.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

**Skapa en Konvertera PDF-klient**

Innan du programmässigt kan utföra en Konvertera PDF-tjänståtgärd måste du skapa en Konvertera PDF-tjänstklient. Om du använder Java API skapar du ett `ConvertPdfServiceClient`-objekt. Om du använder webbtjänstens API skapar du ett `ConvertPDFServiceService`-objekt.

**Hämta PDF-dokumentet för konvertering**

Hämta PDF-dokumentet som ska konverteras till en bild. Du kan inte konvertera ett interaktivt PDF-dokument till en bild. Om du försöker göra det genereras ett undantag. Om du vill konvertera ett interaktivt PDF-dokument till en bildfil måste du förenkla PDF-dokumentet innan du konverterar det. (Se [Förenkla PDF-dokument](/help/forms/developing/creating-document-output-streams.md#flattening-pdf-documents).)

**Ange körningsalternativ**

Ange körningsalternativ som bildformat och upplösningsvärden. Mer information om körningsvärden finns i klassreferensen `ToImageOptionsSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

**Konvertera PDF till en bild**

När du har skapat tjänstklienten och angett körningsalternativ kan du konvertera PDF-dokumentet till en bild. Ett samlingsobjekt som innehåller bilderna returneras.

**Hämta bildfilerna från en samling**

Du kan hämta bildfiler från ett samlingsobjekt som returneras av tjänsten Convert PDF. Varje element i samlingen är en `com.adobe.idp.Document`-instans (eller en `BLOB`-instans om du använder webbtjänster) som du kan spara som en bildfil, till exempel en JPG-fil.

Bildfilens format beror på körningsalternativet `ImageConvertFormat`. Det innebär att om du ställer in körningsalternativet `ImageConvertFormat` på `ImageConvertFormat.JPEG` kan du spara bildfiler som JPG-filer.

**Se även**

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Konvertera snabbstart för PDF Service API](/help/forms/developing/convert-pdf-service-java-api.md#convert-pdf-service-java-api-quick-start-soap)

### Konvertera ett PDF-dokument till bildfiler med Java API {#convert-a-pdf-document-to-image-files-using-the-java-api}

Konvertera ett PDF-dokument till ett bildformat med hjälp av Java (Convert PDF service API):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-convertpdf-client.jar, i Java-projektets klassökväg.

1. Skapa en Convert PDF-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `ConvertPdfServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Hämta det PDF-dokument som ska konverteras.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar det PDF-dokument som ska konverteras med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för PDF-dokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange körningsalternativ.

   * Skapa ett `ToImageOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Anropa metoder som tillhör det här objektet efter behov. Ange till exempel bildtypen genom att anropa metoden `setImageConvertFormat` och skicka ett `ImageConvertFormat`-uppräkningsvärde som anger formattypen.

   >[!NOTE]
   >
   >Uppräkningsvärdet `ImageConvertFormat` måste anges.

1. Konvertera PDF till en bild.

   Anropa `ConvertPdfServiceClient`-objektets `toImage2`-metod och skicka följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar den PDF-fil som ska konverteras.
   * Ett `com.adobe.livecycle.converpdfservice.client.ToImageOptionsSpec`-objekt som innehåller de olika inställningarna för målbildens format.

   Metoden `toImage2` returnerar ett `java.util.List`-objekt som innehåller bilder. Varje element i samlingen är en `com.adobe.idp.Document`-instans.

1. Hämta bildfilerna från en samling.

   Iterera genom objektet `java.util.List` för att avgöra om det finns några bilder. Varje element är en `com.adobe.idp.Document`-instans. Spara bilden genom att anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod och skicka ett `java.io.File`-objekt.

**Se även**

[Snabbstart (SOAP-läge): Konvertera ett PDF-dokument till JPEG-filer med Java API](/help/forms/developing/convert-pdf-service-java-api.md#quick-start-soap-mode-converting-a-pdf-document-to-jpeg-files-using-the-java-api)

### Konvertera ett PDF-dokument till bildfiler med hjälp av webbtjänstens API {#convert-a-pdf-document-to-image-files-using-the-web-service-api}

Konvertera ett PDF-dokument till ett bildformat med hjälp av API:t för konvertering av PDF-tjänster (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/ConvertPDFService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa en konverteringsklient för PDF.

   * Skapa ett `ConvertPdfServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `ConvertPdfServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/ConvertPDFService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Ange dock `?blob=mtom`.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `ConvertPdfServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `ConvertPdfServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `ConvertPdfServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Hämta det PDF-dokument som ska konverteras.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Det här `BLOB`-objektet används för att lagra PDF-formuläret.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som anger var PDF-formuläret finns och i vilket läge filen ska öppnas.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Fastställ bytearrayens storlek genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod. Skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Ange körningsalternativ.

   * Skapa ett `ToImageOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Anropa metoder som tillhör det här objektet efter behov. Ange till exempel bildtypen genom att anropa metoden `setImageConvertFormat` och skicka ett `ImageConvertFormat`-uppräkningsvärde som anger formattypen.

   >[!NOTE]
   >
   >Uppräkningsvärdet `ImageConvertFormat` måste anges.

1. Konvertera PDF till en bild.

   Anropa `ConvertPDFServiceService`-objektets `toImage2`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar filen som ska konverteras
   * Ett `ToImageOptionsSpec`-objekt som innehåller de olika inställningarna för målbildformatet

   Metoden `toImage2` returnerar ett `MyArrayOfBLOB`-objekt som innehåller de nyskapade bildfilerna.

1. Hämta bildfilerna från en samling.

   * Fastställ antalet element i objektet `MyArrayOfBLOB` genom att hämta värdet för dess `Count`-fält. Varje element är ett `BLOB`-objekt som innehåller bilden.
   * Upprepa genom objektet `MyArrayOfBLOB` och spara varje bildfil.

**Se även**

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)
