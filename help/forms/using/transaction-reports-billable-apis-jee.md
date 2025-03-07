---
title: Transaktionsrapporter fakturerbara API:er för AEM Forms på JEE.
description: Lista över alla API:er som räknas som transaktioner för AEM Forms i JEE.
topic-tags: forms-manager
feature: Transaction Reports
role: Admin, User, Developer
solution: Experience Manager, Experience Manager Forms
hide: true
hidefromtoc: true
exl-id: 2bcd650f-c729-43b1-b7a7-9463a47ae25e
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---

# Fakturerbara API:er för transaktionsrapportering för AEM Forms på JEE {#transaction-reports-billable-apis}

AEM Forms on JEE innehåller flera API:er för att skicka, bearbeta och återge dokument. Vissa API:er räknas som transaktioner och andra kan användas. Det här dokumentet innehåller en lista över alla API:er som räknas som transaktioner. Här är några vanliga scenarier där ett fakturerbart API används:

* Konvertera ett dokument från ett format till ett annat
* Förenkla ett dynamiskt PDF-dokument
* Sammanfoga ett interaktivt PDF-dokument med ett annat PDF-dokument

Fakturerings-API:erna tar inte hänsyn till antalet sidor, längden på ett dokument eller formulär eller det återgivna dokumentets slutliga format.
<!--

* **Forms Submitted:** When data is submitted from any type of form created with AEM Forms and the data is submitted to any data storage repository or database is considered form submission. For example, submitting an HTML5 Form, PDF Forms are accounted as forms submitted. Each form in a form set is considered a submission. For example, if a form set has 5 forms, when the form set is submitted, transaction reporting service counts it as 5 submissions.

* **Documents Rendered:** Generating a document by combining a template and data, digitally signing or certifying a document, using a billable document services API for document services, or converting a document from one format to another are accounted as documents rendered.

-->

Nedan finns en lista med fakturerbara JEE-API:er. Hitta listan med [fakturerbara API:er för AEM Forms i OSGi](/help/forms/using/transaction-reports-billable-apis.md).

## Fakturerbara API:er för dokumenttjänster {#billable-document-services-apis}

### Generera PDF-tjänst {#generate-pdf-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
   <tr>
   <td><a>CreatePDF</a></td>
   <td>Skapar Adobe PDF för filtyper som stöds.</td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>CreatePDF3</a></td>
   <td>Skapar Adobe PDF för filtyper som stöds. </td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a> HTMLToPDF</a></td>
   <td>Konverterar HTML-fil till Adobe PDF. </td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>ExportPDF</a></td>
   <td>Exporterar PDF till filtyper som stöds. </td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>ExportPDF2</a></td>
   <td><p>Exporterar PDF till filtyper som stöds.</p> </td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>ExportPDF3</a></td>
   <td>Exporterar PDF till filtyper som stöds.</td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>HTMLFileToPDF</a></td>
   <td>Konverterar HTML-fil till PDF.</td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>HTMLToPDF2</a></td>
   <td>Konverterar HTML-fil till PDF.</td>
   <td>Konvertering <br /> </td>
  </tr>
  <tr>
   <td><a>OptimeraPDF</a></td>
   <td>Optimerar PDF för att minska filstorleken genom att ta bort onödiga metadata utan att påverka kvaliteten.</td>
   <td>Konvertering <br /> </td>
  </tr>
 </tbody>
</table>

### DocAssurance-tjänst {#DocAssurance-Service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
   <td><a>Signera/certifiera</a><br /> </td>
   <td>Med detta API kan du skydda ditt dokument. Du kan använda API:t för att signera och certifiera ett PDF-dokument.</td>
   <td>Konvertering</td>
  </tr>
 </tbody>
</table>


### Distiller Service {#distiller-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
  <tr>
   <td><a>CreatePDF</a></td>
   <td>Skapar Adobe PDF av filtyper som stöds.</td>
   <td>Konvertering</td>
  </tr>
  <tr>
   <td><a>CreatePDF2</a></td>
   <td>Skapar Adobe PDF av filtyper som stöds.</td>
   <td>Konvertering</td>
  </tr>
 </tbody>
</table>

<!--

### Document of Record Service (DoR Service) {#document-of-record-service-dor-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Description</td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/aemds/guide/addon/dor/DoRService.html#render-com.adobe.aemds.guide.addon.dor.DoROptions-" target="_blank">render</a></td>
   <td>Invokes the specified render method to generate a document of record using provided parameters.</td>
   <td>Documents Processed</td>
   <td> </td>
  </tr>
 </tbody>
</table>

-->

### Output Service {#output-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
   <td><a>generateOutput</a></td>
   <td>Sammanfogar data och mallar för att skapa ett PDF-dokument.</td>
   <td>Återgivna dokument</td>
  </tr>
  <tr>
   <td><a>generatePDFOutput</a></td>
   <td>Sammanfogar data och mallar för att skapa ett PDF-dokument.</td>
   <td>Återgivna dokument</td>
  </tr>
  <tr>
   <td><a>generatePDFOutput2</a></td>
   <td>Sammanfogar data och mallar för att skapa ett PDF-dokument.</td>
   <td>Återgivna dokument</td>
  </tr>
  <tr>
   <td><a>generatePrintedOutput</a></td>
   <td>Konverterar XDP- och PDF-dokument till filformaten PostScript (PS), Printer Command Language (PCL) och ZPL. </td>
   <td>Återgivna dokument</td>
  </tr>
  <tr>
   <td><a>generatePrintedOutput2</a></td>
   <td>Konverterar XDP- och PDF-dokument till filformaten PostScript (PS), Printer Command Language (PCL) och ZPL. </td>
   <td>Återgivna dokument</td>
  </tr>
  <tr>
   <td><a>transformPDF</a></td>
   <td>Konverterar en uppsättning XDP- och PDF-dokument till en uppsättning PostScript- (PS), Printer Command Language (PCL) och ZPL-filformat. </td>
   <td>Dokumentkonvertering</td>
  </tr>
 </tbody>
</table>

<!-- ### Forms Service {#forms-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Description</td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html#renderPDFForm-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.fd.forms.api.PDFFormRenderOptions-" target="_blank">renderPDFForm</a></td>
   <td>Renders PDF Form from XDP templates. The XDP templates are created in Forms Designer.</td>
   <td>Documents Processed</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html#exportData-com.adobe.aemfd.docmanager.Document-com.adobe.fd.forms.api.DataFormat-" target="_blank">exportData</a></td>
   <td>Extracts data from a PDF Form or XDP templates</td>
   <td>Documents Processed</td>
   <td> </td>
  </tr>
 </tbody>
</table>

-->

### Konvertera PDF-tjänst {#convert-pdf-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
   <td><a>toImage2</a></td>
   <td>Konverterar ett PDF-dokument till en lista med bilddokument. Bildformat som stöds är JPEG, JPEG2K, PNG och TIFF.</td>
   <td>Dokumentkonvertering</td>
  </tr>
  <tr>
   <td><a>toPS2</a></td>
   <td>Konverterar en platt PDF-fil till PostScript-format med de alternativ som anges i alternativspecifikationen.</td>
   <td>Dokumentkonvertering</td>
  </tr>
  <tr>
   <td><a>toSWF</a></td>
   <td>Konverterar en platt PDF-fil till SWF-format med de alternativ som anges i alternativspecifikationen.</td>
   <td>Dokumentkonvertering</td>
  </tr>
 </tbody>
</table>

### Barcoded Forms Service {#barcoded-forms-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
   <td><a>decode</a></td>
   <td>Avkodar alla streckkoder i ett Document-objekt och returnerar ett org.w3c.dom.Document-objekt som innehåller data som hämtats från streckkoden.</td>
   <td>Dokumentkonvertering</td>
  </tr>
 </tbody>
</table>

### Assembler Service {#assembler-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
   <td><a>invoke</a></td>
   <td>Kör det angivna DDX-dokumentet och returnerar ett AssemblerResult-objekt som innehåller de resulterande dokumenten. </td>
   <td>Dokumentkonvertering</td>
  </tr>
  <tr>
   <td><a>invokeDDX</a></td>
   <td>Kör det angivna DDX-dokumentet och returnerar ett AssemblerResult-objekt som innehåller de resulterande dokumenten. </td>
   <td>Dokumentkonvertering</td>
  </tr>
  <tr>
   <td><a>invokeOneDocument</a></td>
   <td>Konvertera ett angivet dokument till PDF/A med de angivna alternativen.</td>
   <td>Dokumentkonvertering</td>
  </tr>
  <tr>
   <td><a>invokeDDXOneDocument</a></td>
   <td>Konvertera ett angivet dokument till PDF/A med de angivna alternativen.</td>
   <td>Dokumentkonvertering</td>
  </tr>
  <tr>
   <td><a>toPDFA</a></td>
   <td>Konvertera ett angivet dokument till PDF/A med de angivna alternativen.</td>
   <td>Dokumentkonvertering</td>
  </tr>
 </tbody>
</table>

API:ts användning räknas som en transaktion när du utför en eller flera av följande åtgärder:
1. Konvertera från andra format än PDF till PDF. Konverteringen från XDP-format till PDF-format.<!-- catering to both interactive and non-interactive forms of communication, and the conversion from Word to PDF.-->
1. Konvertera från PDF till PDF/A.
1. Konvertera från PDF till andra format än PDF. Exempel omfattar omvandlingen från PDF till bildformat eller konverteringen från PDF till textformat.

>[!NOTE]
>
>* Anrop-API:t för sammansättartjänsten kan internt anropa ett fakturerbart API för en annan tjänst beroende på indata. `invoke API` kan alltså räknas som inga, enskilda eller flera transaktioner. Antalet transaktioner som räknas beror på indata och de interna API:erna som anropas.
>* Ett enda PDF-dokument som skapats med en sammansättningstjänst som `invoke` och `invokeDDX` kan räknas som inga, enstaka eller flera transaktioner. Antalet transaktioner som räknas beror på den angivna <!--DDX-->-koden.

<!--
### PDF Utility Service  {#pdf-utility-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Description</td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td><a>convertPDFtoXDP</a></td>
   <td>Converts a PDF document into an XDP file. For a PDF document to be successfully converted to an XDP file, the PDF document must contain an XFA stream in the AcroForm dictionary.</td>
   <td>Documents Conversion</td>
   <td> </td>
  </tr>
 </tbody>
</table>

-->

## Fakturerbara API:er för datainhämtning {#billable-data-capture-apis}

<!--All the submission events of Forms, HTML5 Forms, and form set are accounted as transactions. By default, submission of a PDF Form is not accounted as a transaction. Use the provided [transaction recorder API](record-transaction-custom-implementation.md) to record a PDF Forms submission as a transaction.-->

<!--
### Adaptive Forms {#adaptive-forms}

<table>
 <tbody>
  <tr>
   <td><p>Use Case</p> </td>
   <td>Description</td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td>Submitting an adaptive form</td>
   <td>Submits an adaptive form to configured submit action. </td>
   <td>Forms Submitted</td>
   <td>
    <ul>
     <li>Successful submissions account for single or two transactions. The number of transactions counted depends upon the type of submit action used for submission. For example, sending PDF through email submit action accounts for two counts of transactions. One transaction for form submission and another for PDF generated using the Document of Record (DOR) service. </li>
     <li>Using the adaptive form within an adaptive form (Adaptive form formset) accounts only single transaction. You can have any number of adaptive forms within an adaptive form.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

-->

<!--### HTML5 Forms {#html-forms}

<table>
 <tbody>
  <tr>
   <td><p>Use Case</p> </td>
   <td>Description </td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td>Submitting an HTML5 Form</td>
   <td>Submits an HTML5 Form to submit URL configured in the form.</td>
   <td>Forms Submitted</td>
   <td> </td>
  </tr>
 </tbody>
</table>

-->

### Forms {#form-set}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
  </tr>
  <tr>
   <td>exportData</td>
   <td>Skickar formulär.</td>
   <td>Forms har skickats</td>
  </tr>
  <tr>
   <td>exportData2</td>
   <td>Skickar formulär.</td>
   <td>Forms har skickats</td>
  </tr>
  <tr>
   <td>renderForm</td>
   <td>Skickar formulär.</td>
   <td>Forms Rendered</td>
  </tr>
  <tr>
   <td>renderHTMLForm</td>
   <td>Skickar formulär.</td>
   <td>Forms Rendered</td>
  </tr>
  <tr>
   <td>renderHTMLForm2</td>
   <td>Skickar formulär.</td>
   <td>Forms Rendered</td>
  </tr>
  <tr>
   <td>renderPDFForm</td>
   <td>Skickar formulär.</td>
   <td>Forms Rendered</td>
  </tr>
  <tr>
   <td>renderPDFForm2</td>
   <td>Skickar formulär.</td>
   <td>Forms Rendered</td>
  </tr>
  <tr>
   <td>renderPDFFormWithUsageRights</td>
   <td>Skickar formulär.</td>
   <td>Forms Rendered</td>
  </tr>
 </tbody>
</table>

<!-- ## Billable Interactive Communication and Form-centric AEM Workflows on OSGi APIs {#billable-interactive-communication-and-form-centric-aem-workflows-on-osgi-apis}

Assign task and document services steps of Form-centric AEM Workflows on OSGi and all the renditions of interactive communication and are accounted as transactions. Previewing an interactive communication on the author instance and previewing on the publish instance using Agent UI are not accounted as transactions. If a workflow step accounts a transaction and the workflow fails to complete, the transaction count is not reversed.

<!--

### Interactive Communication - Web Channel {#interactive-communication-web-channel}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Description</td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td>Rendering a web channel</td>
   <td>Opens the web version of an interactive communication.</td>
   <td>Documents Rendered</td>
   <td>
    <div>
    </div> </td>
  </tr>
 </tbody>
</table>

### Interactive Communication - Print Channel {#interactive-communication-print-channel}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Description</td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/ccm/channels/print/api/model/PrintChannel.html" target="_blank">render</a> (convert to PDF)</td>
   <td>Generates the PDF version of an interactive communication.</td>
   <td>Documents Rendered</td>
   <td>
    <div>
    </div> </td>
  </tr>
 </tbody>
</table>

-->


<!--
### Form-centric AEM Workflows on OSGi  {#form-centric-aem-workflows-on-osgi}

<table>
 <tbody>
  <tr>
   <td><p>Use case</p> </td>
   <td>Transaction report category</td>
   <td>Additional Information</td>
  </tr>
  <tr>
   <td>Submitting an Assign Task step</td>
   <td>Forms Submitted</td>
   <td>
    <div>
    </div> </td>
  </tr>
  <tr>
   <td>Submitting a workflow application startpoint </td>
   <td>Forms Submitted</td>
   <td> </td>
  </tr>
  <tr>
   <td>Submitting an interactive communication (Print Channel) from the Agent UI to a workflow</td>
   <td>Documents Rendered</td>
   <td> </td>
  </tr>
 </tbody>
</table>

-->


<!--

WILL ADD THIS ONE - 

## Recording billable APIs as transactions for custom code {#recording-billable-apis-as-transactions-for-custom-code}

Actions like submitting a PDF Form, using Agent UI to preview an interactive communication, using non-standard form submission, and custom implementations are not accounted as transactions. AEM Forms provides an API to record such actions as transactions. You can call the API from your custom implementations to [record a transaction](/help/forms/using/record-transaction-custom-implementation.md).

## Related Articles {#related-articles}

* [Transaction Reports Overview](../../forms/using/transaction-reports-overview.md)
* [Viewing and Understanding a Transaction Reports](../../forms/using/viewing-and-understanding-transaction-reports.md)
* [Record a transaction for custom implementations](/help/forms/using/record-transaction-custom-implementation.md)

-->

## Relaterade artiklar

* [Aktivera och visa transaktionsrapport för AEM Forms på JEE](/help/forms/using/transaction-report-overview-jee.md)
* [Registrera en transaktion för anpassade komponent-API:er för AEM Forms i JEE](/help/forms/using/record-transaction-custom-component-jee.md)
