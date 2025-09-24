---
title: Fakturerbara API:er för transaktionsrapporter
description: Lista över alla API:er som räknas som transaktioner
topic-tags: forms-manager
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
feature: Transaction Reports
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: d9dc7630-a157-4202-8caf-7c55e348c06e
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '1763'
ht-degree: 0%

---

# Transaction Reports Billable APIs for AEM Forms on OSGi {#transaction-reports-billable-apis}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/forms/using-communications/transaction-reports-billable-apis).

AEM Forms har flera API:er för att skicka formulär, bearbeta dokument och återge dokument. Vissa API:er räknas som transaktioner och andra kan användas. Det här dokumentet innehåller en lista över alla API:er som har redovisats som transaktioner i en transaktionsrapport. Här är några vanliga scenarier där ett fakturerbart API används:

* Skicka ett anpassat formulär, HTML5-formulär och formuläruppsättning
* Återge en tryckt eller webbversion av en interaktiv kommunikation
* Konvertera ett dokument från ett format till ett annat
* Förenkla ett dynamiskt PDF-dokument
* Generera ett postdokument
* Sammanfoga ett interaktivt PDF-dokument med ett annat PDF-dokument
* Tilldela uppgiftssteg och dokumenttjänststeg i AEM Workflows
* Använda anpassningsbara formulär i ett adaptivt formulär

Fakturerings-API:erna tar inte hänsyn till antalet sidor, längden på ett dokument eller formulär eller det återgivna dokumentets slutliga format. En transaktionsrapport delar upp transaktionerna i två kategorier: Dokument som återges och Forms som skickas.

* **Forms har skickats:** När data skickas från en typ av formulär som skapats med AEM Forms och data skickas till en datalagringsplats eller databas betraktas som formuläröverföring. Om du t.ex. skickar in ett anpassningsbart formulär, HTML5-formulär, PDF forms och formuläruppsättning, räknas de som inskickade formulär. Varje formulär i en formuläruppsättning betraktas som en inlämning. Om en formuläruppsättning till exempel har fem formulär, räknas den som 5 inskickade när formuläruppsättningen skickas.

* **Dokument som återges:** Generera ett dokument genom att kombinera en mall och data, signera digitalt eller certifiera ett dokument, använda ett fakturerbart dokument-API för dokumenttjänster eller genom att konvertera ett dokument från ett format till ett annat, räknas som dokument som återges.

>[!NOTE]
>
>Gränssnittet för transaktionsrapporter visar tre kategorier: Forms skickat, Dokument återgivet och Bearbetade dokument. Både återgivna dokument och behandlade dokument räknas som återgivna dokument.

## Fakturerbara API:er för dokumenttjänster {#billable-document-services-apis}

### Generera PDF-tjänst {#generate-pdf-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#createPDF-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-" target="_blank">createPDF</a></td>
   <td>Skapar Adobe PDF av filtyper som stöds.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#createPDF2-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-" target="_blank">createPDF2</a></td>
   <td>Skapar Adobe PDF av filtyper som stöds.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#exportPDF-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-" target="_blank">exportPDF</a></td>
   <td>Konverterar Adobe PDF till filtyper som stöds. </td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#exportPDF2-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-" target="_blank">exportPDF2</a></td>
   <td>Konverterar Adobe PDF till filtyper som stöds. </td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#exportPDF2-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-" target="_blank">exportPDF3</a></td>
   <td>Konverterar Adobe PDF till filtyper som stöds. </td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#htmlFileToPdf-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-">htmlFileToPdf</a></td>
   <td><p>Skapar PDF från HTML sidor.</p> </td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#htmlToPdf-java.lang.String-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-" target="_blank">htmlToPdf</a></td>
   <td>Skapar PDF från URL:er som pekar mot en HTML-sida.</td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#htmlToPdf2-java.lang.String-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-" target="_blank">htmlToPdf2</a></td>
   <td>Skapar PDF från URL:er som pekar mot en HTML-sida.</td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/GeneratePDFService.html#optimizePDF-com.adobe.aemfd.docmanager.Document-java.lang.String-com.adobe.aemfd.docmanager.Document-" target="_blank">optimeraPDF</a></td>
   <td>Optimerar PDF för att minska filstorleken genom att ta bort onödiga metadata utan att påverka kvaliteten.</td>
   <td>Bearbetade dokument:<br /> </td>
   <td> </td>
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
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-4/forms/javadocs/com/adobe/fd/docassurance/client/api/DocAssuranceService.html#secureDocument-com.adobe.aemfd.docmanager.Document-com.adobe.fd.docassurance.client.api.EncryptionOptions-com.adobe.fd.docassurance.client.api.SignatureOptions-com.adobe.fd.docassurance.client.api.ReaderExtensionOptions-com.adobe.fd.signatures.pdf.inputs.UnlockOptions-" target="_blank">secureDocument</a><br /> </td>
   <td>Med detta API kan du skydda ditt dokument. Du kan använda API:t för att signera, certifiera, utöka eller kryptera ett PDF-dokument.</td>
   <td>Bearbetade dokument</td>
   <td>Endast signerings- och certifieringsåtgärden för secureDocument faktureras.</td>
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
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/DistillerService.html#createPDF-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-" target="_blank">createPDF</a><br /> </td>
   <td>Skapar Adobe PDF av filtyper som stöds.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/pdfg/service/api/DistillerService.html#createPDF2-com.adobe.aemfd.docmanager.Document-java.lang.String-java.lang.String-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-" target="_blank">createPDF2</a></td>
   <td>Skapar Adobe PDF av filtyper som stöds.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
 </tbody>
</table>

### DoR-tjänst (Document of Record Service) {#document-of-record-service-dor-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/aemds/guide/addon/dor/DoRService.html#render-com.adobe.aemds.guide.addon.dor.DoROptions-" target="_blank">återge</a></td>
   <td>Anropar den angivna återgivningsmetoden för att generera ett postdokument med angivna parametrar.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Output Service {#output-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-" target="_blank">generatePDFOutput</a></td>
   <td>Sammanfogar data och mallar för att skapa ett PDF-dokument.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-" target="_blank">generatePDFOutput</a></td>
   <td>Sammanfogar data och mallar för att skapa ett PDF-dokument.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutputBatch-java.util.Map-java.util.Map-com.adobe.fd.output.api.PDFOutputOptions-com.adobe.fd.output.api.BatchOptions-" target="_blank">generatePDFOutputBatch</a></td>
   <td>Sammanfogar data och mallar för att skapa en uppsättning PDF-dokument.</td>
   <td>Bearbetade dokument</td>
   <td> API:t generatePDFOutputBatch kombinerar en formulärmall med en post och genererar en PDF. När du bearbetar en grupp poster räknas varje post som en separat PDF-återgivning av transaktionsrapporteringstjänsten. <br> Du kan använda flaggan <a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/BatchOptions.html#getGenerateManyFiles--"> getGenerateManyFiles</a> för att kombinera flera återgivningar till en enda PDF-fil. Oavsett flaggstatus räknas varje post som en separat PDF-återgivning. </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePrintedOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PrintedOutputOptions-" target="_blank">generatePrintedOutput</a></td>
   <td>Konverterar XDP- och PDF-dokument till filformaten PostScript (PS), Printer Command Language (PCL) och ZPL. </td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePrintedOutput-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PrintedOutputOptions-" target="_blank">generatePrintedOutput</a></td>
   <td>Konverterar XDP- och PDF-dokument till filformaten PostScript (PS), Printer Command Language (PCL) och ZPL. </td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePrintedOutputBatch-java.util.Map-java.util.Map-com.adobe.fd.output.api.PrintedOutputOptions-com.adobe.fd.output.api.BatchOptions-" target="_blank">generatePrintedOutputBatch</a></td>
   <td>Konverterar en uppsättning XDP- och PDF-dokument till en uppsättning PostScript- (PS), Printer Command Language (PCL) och ZPL-filformat. </td>
   <td>Bearbetade dokument</td>
   <td> API:t generatePDFOutputBatch kombinerar en formulärmall med en post och genererar en PDF. När du bearbetar en grupp poster räknas varje post som en separat PDF-återgivning av transaktionsrapporteringstjänsten. <br> Du kan använda flaggan <a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/output/api/BatchOptions.html#getGenerateManyFiles--"> getGenerateManyFiles</a> för att kombinera flera återgivningar till en enda PDF-fil. Oavsett flaggstatus räknas varje post som en separat PDF-återgivning. </td>
  </tr>
 </tbody>
</table>

### Forms Service {#forms-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html#renderPDFForm-java.lang.String-com.adobe.aemfd.docmanager.Document-com.adobe.fd.forms.api.PDFFormRenderOptions-" target="_blank">renderPDFForm</a></td>
   <td>Återger PDF-formulär från XDP-mallar. XP-mallarna skapas i Forms Designer.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html#exportData-com.adobe.aemfd.docmanager.Document-com.adobe.fd.forms.api.DataFormat-" target="_blank">exportData</a></td>
   <td>Extraherar data från ett PDF-formulär eller XDP-mallar</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Konvertera PDF-tjänst {#convert-pdf-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/cpdf/api/ConvertPdfService.html#toImage-com.adobe.aemfd.docmanager.Document-com.adobe.fd.cpdf.api.ToImageOptionsSpec-" target="_blank">toImage</a></td>
   <td>Konverterar ett PDF-dokument till en lista med bilddokument. Bildformat som stöds är JPEG, JPEG2K, PNG och TIFF.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/cpdf/api/ConvertPdfService.html#toImage-com.adobe.aemfd.docmanager.Document-com.adobe.fd.cpdf.api.ToImageOptionsSpec-" target="_blank">toPS</a></td>
   <td>Konverterar en platt PDF-fil till PostScript-format med de alternativ som anges i alternativspecifikationen.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
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
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/bcf/api/BarcodedFormsService.html#decode-com.adobe.aemfd.docmanager.Document-java.lang.Boolean-java.lang.Boolean-java.lang.Boolean-java.lang.Boolean-java.lang.Boolean-java.lang.Boolean-java.lang.Boolean-java.lang.Boolean-com.adobe.fd.bcf.api.CharSet-" target="_blank">decode</a></td>
   <td>Avkodar alla streckkoder i ett Document-objekt och returnerar ett org.w3c.dom.Document-objekt som innehåller data som hämtats från streckkoden.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
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
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html#invoke-com.adobe.aemfd.docmanager.Document-java.util.Map-com.adobe.fd.assembler.client.AssemblerOptionSpec-">invoke</a></td>
   <td>Kör det angivna DDX-dokumentet och returnerar ett <a href="https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/assembler/client/AssemblerResult.html">AssemblerResult</a> -objekt som innehåller de resulterande dokumenten. </td>
   <td>Bearbetade dokument</td>
   <td>Följande operationer redovisas inte som transaktioner:
    <ul>
     <li>Skapa paket eller portfölj</li>
     <li>Välja ut flera XDP-filer </li>
    </ul> </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html#invoke-com.adobe.aemfd.docmanager.Document-java.util.Map-com.adobe.fd.assembler.client.AssemblerOptionSpec-" target="_blank">invoke</a></td>
   <td>Kör det angivna DDX-dokumentet och returnerar ett <a href="https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/assembler/client/AssemblerResult.html">AssemblerResult</a> -objekt som innehåller de resulterande dokumenten. </td>
   <td>Bearbetade dokument</td>
   <td>Alla indatafilformat som stöds av PDF Generator, Forms och Output Services har stöd för alla dessa format som utdatafilformat. </td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html#toPDFA-com.adobe.aemfd.docmanager.Document-com.adobe.fd.assembler.client.PDFAConversionOptionSpec-">toPDFA</a></td>
   <td>Konvertera ett angivet dokument till PDF/A med de angivna alternativen.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
 </tbody>
</table>

API:ts användning räknas som en transaktion när du utför en eller flera av följande åtgärder:
1. Konvertera från andra format än PDF till PDF. Konverteringen från XDP-format till PDF-format, till exempel både interaktiva och icke-interaktiva kommunikationsformer och konverteringen från Word till PDF.
1. Konvertera från PDF till PDF/A.
1. Konvertera från PDF till andra format än PDF. Exempel omfattar omvandlingen från PDF till bildformat eller konverteringen från PDF till textformat.

>[!NOTE]
>
>* Anrop-API:t för sammansättartjänsten kan internt anropa ett fakturerbart API för en annan tjänst beroende på indata. Detta innebär att invoke-API:t kan redovisas som inga, enskilda eller flera transaktioner. Antalet transaktioner som räknas beror på indata och de interna API:erna som anropas.
>* Ett enda PDF-dokument som skapats med en monteringstjänst kan räknas som inga, enstaka eller flera transaktioner. Antalet transaktioner som räknas beror på den angivna DDX-koden.

### PDF Utility Service  {#pdf-utility-service}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/pdfutility/services/PDFUtilityService.html#convertPDFtoXDP-com.adobe.aemfd.docmanager.Document-" target="_blank">convertPDFtoXDP</a></td>
   <td>Konverterar PDF-dokument till XDP-filer. För att ett PDF-dokument ska kunna konverteras till en XDP-fil måste PDF-dokumentet innehålla en XFA-ström i AcroForm-ordlistan.</td>
   <td>Bearbetade dokument</td>
   <td> </td>
  </tr>
 </tbody>
</table>

## Fakturerbara API:er för datainhämtning {#billable-data-capture-apis}

Alla inlämningshändelser för adaptiva formulär, HTML5 Forms och formuläruppsättningar redovisas som transaktioner. Som standard räknas inte inlämning av ett PDF-formulär som en transaktion. Använd det tillhandahållna [API:t för transaktionsregistrering](record-transaction-custom-implementation.md) om du vill spela in en inskickning från PDF forms som en transaktion.

### Adaptiv Forms {#adaptive-forms}

<table>
 <tbody>
  <tr>
   <td><p>Användningsfall</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td>Skicka ett anpassat formulär</td>
   <td>Skickar ett anpassat formulär till den konfigurerade skicka-åtgärden. </td>
   <td>Forms har skickats</td>
   <td>
    <ul>
     <li>Konto för slutförda överföringar för en eller två transaktioner. Antalet transaktioner som räknas beror på vilken typ av överföringsåtgärd som används för att skicka in. Du kan till exempel skicka PDF via e-poståtgärdskonton för två antal transaktioner. En transaktion för att skicka formulär och en annan för PDF som genererats med tjänsten Document of Record (DOR). </li>
     <li>När du använder det adaptiva formuläret i ett adaptivt formulär (adaptiv formuläruppsättning) utförs endast en transaktion. Du kan ha ett obegränsat antal anpassningsbara formulär i ett anpassat formulär.</li>
    </ul> </td>
  </tr>
 </tbody>
</table>

### HTML5 Forms {#html-forms}

<table>
 <tbody>
  <tr>
   <td><p>Användningsfall</p> </td>
   <td>Beskrivning </td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td>Skicka ett HTML5-formulär</td>
   <td>Skickar ett HTML5-formulär för att skicka en URL som konfigurerats i formuläret.</td>
   <td>Forms har skickats</td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Formuläruppsättning {#form-set}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td>Skicka en formuläruppsättning</td>
   <td>Skickar formuläruppsättningen till den skicka-URL som är konfigurerad i formuläruppsättningen.</td>
   <td>Forms har skickats</td>
   <td>
    <ul>
     <li>När du använder det adaptiva formuläret i ett adaptivt formulär (adaptiv formuläruppsättning) utförs endast en transaktion. Du kan ha ett obegränsat antal anpassningsbara formulär i ett anpassat formulär.</li>
     <li>Alla formulär i en HTML5 Forms-formuläruppsättning har ett separat konto. </li>
    </ul> </td>
  </tr>
 </tbody>
</table>

## Fakturerbar interaktiv kommunikation och formulärbaserade AEM-arbetsflöden i OSGi API:er {#billable-interactive-communication-and-form-centric-aem-workflows-on-osgi-apis}

Tilldela uppgifter och dokumenttjänster i blankettbaserade AEM-arbetsflöden i OSGi och alla återgivningar av interaktiv kommunikation. De redovisas som transaktioner. Förhandsgranskning av en interaktiv kommunikation på författarinstansen och förhandsgranskning på publiceringsinstansen med agentens användargränssnitt räknas inte som transaktioner. Om ett arbetsflödessteg redovisar en transaktion och arbetsflödet inte kan slutföras, återförs inte antalet transaktioner.

### Interaktiv kommunikation - webbkanal {#interactive-communication-web-channel}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td>Återge en webbkanal</td>
   <td>Öppnar webbversionen av en interaktiv kommunikation.</td>
   <td>Återgivna dokument</td>
   <td>
    <div>
    </div> </td>
  </tr>
 </tbody>
</table>

### Interaktiv kommunikation - tryckkanal {#interactive-communication-print-channel}

<table>
 <tbody>
  <tr>
   <td><p>API</p> </td>
   <td>Beskrivning</td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td><a href="https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/ccm/channels/print/api/model/PrintChannel.html" target="_blank">render</a> (konvertera till PDF)</td>
   <td>Skapar PDF-versionen av en interaktiv kommunikation.</td>
   <td>Återgivna dokument</td>
   <td>
    <div>
    </div> </td>
  </tr>
 </tbody>
</table>

### Formulärbaserade AEM-arbetsflöden i OSGi  {#form-centric-aem-workflows-on-osgi}

<table>
 <tbody>
  <tr>
   <td><p>Använd skiftläge</p> </td>
   <td>Kategori för transaktionsrapport</td>
   <td>Ytterligare information</td>
  </tr>
  <tr>
   <td>Skicka ett steg för Tilldela uppgift</td>
   <td>Forms har skickats</td>
   <td>
    <div>
    </div> </td>
  </tr>
  <tr>
   <td>Skicka en startpunkt för ett arbetsflödesprogram </td>
   <td>Forms har skickats</td>
   <td> </td>
  </tr>
  <tr>
   <td>Skicka en interaktiv kommunikation (tryckkanal) från agentgränssnittet till ett arbetsflöde</td>
   <td>Återgivna dokument</td>
   <td> </td>
  </tr>
 </tbody>
</table>

## Registrera fakturerbara API:er som transaktioner för anpassad kod {#recording-billable-apis-as-transactions-for-custom-code}

Åtgärder som att skicka ett PDF-formulär, använda agentanvändargränssnittet för att förhandsgranska interaktiv kommunikation, skicka formulär som inte är standard och anpassade implementeringar räknas inte som transaktioner. AEM Forms tillhandahåller ett API för att spela in sådana åtgärder som transaktioner. Du kan anropa API:t från dina anpassade implementeringar för att [spela in en transaktion](/help/forms/using/record-transaction-custom-implementation.md).

## Relaterade artiklar {#related-articles}

* [Översikt över transaktionsrapporter för AEM Forms på OSGi](../../forms/using/transaction-reports-overview.md)
* [Visa och förstå transaktionsrapporter för AEM Forms i OSGi](../../forms/using/viewing-and-understanding-transaction-reports.md)
* [Registrera en transaktion för anpassade implementeringar för AEM Forms på OSGi](/help/forms/using/record-transaction-custom-implementation.md)
