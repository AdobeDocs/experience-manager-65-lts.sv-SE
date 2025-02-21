---
title: ConvertPDF Service
description: Använd tjänsten Adobe Experience Manager Forms ConvertPDF för att konvertera PDF-dokument till PostScript eller bildfiler.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
feature: Document Services,Assembler
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# ConvertPDF Service {#convertpdf-service}

## Ökning {#overview}

Med tjänsten Convert PDF kan du konvertera PDF-dokument till PostScript eller bildfiler (JPEG, JPEG 2000, PNG och TIFF). Att konvertera ett PDF-dokument till PostScript är användbart för oövervakad serverbaserad utskrift på alla PostScript-skrivare. Att konvertera ett PDF-dokument till en flersidig TIFF-fil är praktiskt när man arkiverar dokument i content management-system som inte stöder PDF-dokument.

Du kan göra följande med tjänsten Convert PDF:

* Konvertera PDF-dokument till PostScript. När du konverterar till PostScript kan du använda konverteringsåtgärden för att ange källdokumentet och om det ska konverteras till PostScript nivå 2 eller 3. Det PDF-dokument som du konverterar till en PostScript-fil måste vara icke-interaktivt.
* Konvertera PDF-dokument till bildformaten JPEG, JPEG 2000, PNG och TIFF. När du konverterar till något av dessa bildformat kan du använda konverteringsåtgärden för att ange källdokumentet och en bildalternativsspecifikation. Specifikationen innehåller olika inställningar, till exempel bildkonverteringsformat, bildupplösning och färgkonvertering.

## Konfigurera egenskaper för tjänsten   {#properties}

Du kan använda **AEMFD ConvertPDF-tjänsten** i AEM Console för att konfigurera egenskaper för den här tjänsten. AEM-konsolens standardwebbadress är `https://[host]:'port'/system/console/configMgr`.

## Använda tjänsten {#using-the-service}

ConvertPDF-tjänsten tillhandahåller följande två API:er:

* **[toPS](https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/cpdf/api/ConvertPdfService.html#toPS)**: Konverterar ett PDF-dokument till en PostScript-fil.

* **[toImage](https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/cpdf/api/ConvertPdfService.html#toImage)**: Konverterar ett PDF-dokument till en bildfil. Bildformat som stöds är JPEG, JPEG2000, PNG och TIFF.

### Använda toPS API med en JSP eller Servlets {#using-tops-api-with-a-jsp-or-servlets}

```jsp
<%@ page import="java.util.List, java.io.File,

                com.adobe.fd.cpdf.api.ConvertPdfService,
                com.adobe.fd.cpdf.api.ToPSOptionsSpec,
                com.adobe.fd.cpdf.api.enumeration.PSLevel,

                com.adobe.aemfd.docmanager.Document" %><%
%><%@taglib prefix="sling" uri="https://sling.apache.org/taglibs/sling/1.0" %><%
%><sling:defineObjects/><%

 // Get reference to ConvertPdfService OSGi service.
 // Within an OSGi bundle @Reference annotation
 // can be used to retrieve service reference

 ConvertPdfService cpdfService = sling.getService(ConvertPdfService.class);

 // path to input document in AEM repository
 // replace this with path to your document
String documentPath = "/content/dam/formsanddocuments/ExpenseClaimFlat.pdf";

 // Create a Docmanager Document object for
 // the flat PDF file which needs to be converted
 // to PostScript

 Document inputPDF = new Document(documentPath);

 // options object to pass to toPS API
 ToPSOptionsSpec toPSOptions = new ToPSOptionsSpec();

 // mandatory option to pass, sets PostScript language
 toPSOptions.setPsLevel(PSLevel.LEVEL_3);

 // invoke toPS method to convert inputPDF to PostScript
 Document convertedPS = cpdfService.toPS(inputPDF, toPSOptions);

 // save converted PostScript file to disk
 convertedPS.copyToFile(new File("C:/temp/out.ps"));

%>
```

### Använda toImage API med en JSP eller Servlets {#using-toimage-api-with-a-jsp-or-servlets}

```jsp
<%@ page import="java.util.List, java.io.File,

                com.adobe.fd.cpdf.api.ConvertPdfService,
                com.adobe.fd.cpdf.api.ToImageOptionsSpec,
                com.adobe.fd.cpdf.api.enumeration.ImageConvertFormat,

                com.adobe.aemfd.docmanager.Document" %><%
%><%@taglib prefix="sling" uri="https://sling.apache.org/taglibs/sling/1.0" %><%
%><sling:defineObjects/><%

 // Get reference to ConvertPdfService OSGi service.
 // Within an OSGi bundle @Reference annotation
 // can be used to retrieve service reference

 ConvertPdfService cpdfService = sling.getService(ConvertPdfService.class);

 // path to input document in AEM repository
 // replace this with path to your document
 String documentPath = "/content/dam/formsanddocuments/ExpenseClaimFlat.pdf";

 // Create a Docmanager Document object for
 // the flat PDF file which needs to be converted
 // to image

 Document inputPDF = new Document(documentPath);

 // options object to pass to toImage API
 ToImageOptionsSpec toImageOptions = new ToImageOptionsSpec();

 // mandatory option to pass, image format
 toImageOptions.setImageConvertFormat(ImageConvertFormat.JPEG);

 // invoke toImage method to convert inputPDF to Image
 List<Document> convertedImages = cpdfService.toImage(inputPDF, toImageOptions);

 // save converted image files to disk
 for(int i=0;i<convertedImages.size();i++){
     Document pageImage = convertedImages.get(i);
     pageImage.copyToFile(new File("C:/temp/out_"+(i+1)+".jpeg"));
 }

%>
```

### Använda ConvertPDF Service med AEM-arbetsflöden {#using-convertpdf-service-with-aem-workflows}

Att köra ConvertPDF-tjänsten från ett arbetsflöde påminner om att köra från JSP/Servlet.

Den enda skillnaden är när tjänsten körs från JSP/Servlet som dokumentobjektet automatiskt hämtar en instans av ResourceResolver-objektet från objektet ResourceResolverHelper. Denna automatiska mekanism
fungerar inte när koden anropas från ett arbetsflöde. För ett arbetsflöde skickar du en instans av ResourceResolver-objektet explicit till Document-klasskonstruktorn. Dokumentobjektet använder sedan
tillhandahåller ResourceResolver-objekt för att läsa innehåll från databasen.

Följande exempelarbetsflödesprocess konverterar indatadokumentet till ett PostScript-dokument. Koden skrivs i ECMAScript och dokumentet skickas som arbetsflödets nyttolast:

```javascript
/*
 * Imports
 */
var ConvertPdfService = Packages.com.adobe.fd.cpdf.api.ConvertPdfService;
var ToPSOptionsSpec = Packages.com.adobe.fd.cpdf.api.ToPSOptionsSpec;
var PSLevel = Packages.com.adobe.fd.cpdf.api.enumeration.PSLevel;
var Document = Packages.com.adobe.aemfd.docmanager.Document;
var File = Packages.java.io.File;
var ResourceResolverFactory = Packages.org.apache.sling.api.resource.ResourceResolverFactory;

// get reference to ConvertPdfService
var cpdfService = sling.getService(ConvertPdfService);

/*
 * workflow payload and path to it
 */
var payload = graniteWorkItem.getWorkflowData().getPayload();
var payload_path = payload.toString();

/* Create resource resolver using current session
 * this resource resolver needs to be passed to Document
 * object constructor when file is to be read from
 * crx repository.
 */

/* get ResourceResolverFactory service reference , used
 * to construct resource resolver
 */
var resourceResolverFactory = sling.getService(ResourceResolverFactory);

var authInfo = {
    "user.jcr.session":graniteWorkflowSession.getSession()
};

var resourceResolver = resourceResolverFactory.getResourceResolver(authInfo);

// Create Document object from payload_path
var inputDocument = new Document(payload_path, resourceResolver);

// options object to be passed to toPS operation
var toPSOptions = new ToPSOptionsSpec();

// Set PostScript Language Three
toPSOptions.setPsLevel(PSLevel.LEVEL_3);

// invoke toPS operation to convert inputDocument to PS
var convertedPS = cpdfService.toPS(inputDocument, toPSOptions);

// save converted PostScript file to disk
convertedPS.copyToFile(new File("C:/temp/out.ps"));
```
