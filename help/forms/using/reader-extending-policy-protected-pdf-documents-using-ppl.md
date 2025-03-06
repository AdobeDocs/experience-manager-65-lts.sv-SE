---
title: Reader för att utöka profilskyddade PDF-dokument med hjälp av Portable Protection Library
description: Reader-tillägg möjliggör interaktiva funktioner i Adobe PDF-dokument via Acrobat Reader. Du kan använda PPL (Portable Protection Library) för att utöka de DRM-skyddade PDF-dokumenten.
contentOwner: khsingh
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
feature: Document Security,Reader Extensions
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: b1430a30-313f-4efc-85c5-ccb914923031
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 0%

---

# Reader för att utöka profilskyddade PDF-dokument med hjälp av Portable Protection Library {#reader-extending-policy-protected-pdf-documents-using-portable-protection-library}

Lär dig mer om dokumentsäkerhet, Reader-tillägg och Java Programming Language för att utöka dokumentskyddet i PDF-dokument.

Du kan använda dokumentskydd för att begränsa åtkomsten till vissa PDF-dokument till endast behöriga användare. Du kan också bestämma hur en mottagare ska kunna använda ett skyddat dokument. Du kan till exempel ange om mottagarna ska kunna skriva ut, kopiera eller redigera text i ett dokument som skyddas av dokumentsäkerhetsregler. Mer information om dokumentsäkerhet finns i [om dokumentsäkerhet](/help/forms/using/admin-help/document-security.md).

Du kan använda läsartillägg för att aktivera interaktiva funktioner i Adobe PDF-dokument via Acrobat Reader. Dessa interaktiva funktioner som normalt bara är tillgängliga via Adobe Acrobat Professional och Standard. Mer information om de interaktiva funktioner som läsartillägg kan aktivera finns i [Adobe Experience Manager Forms DocAssurance-tjänsten ](/help/forms/using/overview-aem-document-services.md)**.**

Du kan använda det portabla skyddsbiblioteket för att tillämpa skyddsprofiler på dokumentet utan att behöva skicka dokumentet via nätverket. Det är bara säkerhetsreferenser och skyddsprofiler som rör sig över nätverket. Det faktiska dokumentet lämnar aldrig klienten och skyddsprofiler tillämpas lokalt på klienten.

## Reader utökning av dokumentsäkerhet skyddat PDF-dokument {#reader-extending-document-security-policy-protected-pdf-documents}

Skyddade dokument är krypterade. Du kan inte använda standardAPI:er för läsartillägg för att tillämpa, ta bort och hämta användningsrättigheter för principskyddade PDF-dokument. Det är bara Reader Extensions-tjänsten i Portable Protection Library som tillhandahåller API:er för att lägga till, ta bort och hämta användarrättigheter för PDF-dokument som skyddas av dokumentsäkerhetsprinciper.

### Tjänsten Reader Extensions {#reader-extensions-service}

Tilläggstjänsten för läsare lägger till användarrättigheter i ett profilskyddat PDF-dokument och aktiverar funktioner som normalt inte är tillgängliga när ett PDF-dokument öppnas med Adobe Acrobat Reader. Den har även API:er för att ta bort och hämta användarrättigheter för ett policyskyddat dokument.

Reader Extensions-tjänsten stöder fullt ut PDF-dokument som är baserade på PDF-standard 1.6 och senare. Förutom Acrobat Reader behöver användare från tredje part inga ytterligare program eller plugin-program för att kunna använda profilskyddade PDF-dokument.

Du kan utföra följande uppgifter med tjänsten Reader Extensions:

* Lägg in användarrättigheter i policyskyddade PDF-dokument.
* Ta bort användningsrättigheterna för ett profilskyddat PDF-dokument.
* Hämta användningsbehörighet för profilskyddade PDF-dokument.

### Tillämpa användarrättigheter på ett PDF-dokument som skyddas av dokumentsäkerhetsregler {#apply-usage-rights-to-a-document-security-policy-protected-pdf-document}

Du kan använda Java-API:t `applyUsageRights`för att tillämpa användningsrättigheter på principskyddade PDF-dokument. Användningsrättigheterna gäller funktioner som är tillgängliga som standard i Acrobat men inte i Adobe Reader, t.ex. möjligheten att lägga till kommentarer i ett formulär eller att fylla i formulärfält och spara formuläret. PDF-dokument med användarrättigheter kallas för rättighetsaktiverade dokument. En användare som öppnar ett rättighetsaktiverat dokument i Adobe Reader kan utföra åtgärder som är aktiverade för det specifika dokumentet.

**Syntax:** `InputStream applyUsageRights(InputStream inputFile, File certFile, String credentialPassword, UsageRights usageRights)`

<table>
 <tbody>
  <tr>
   <td><p><strong>Parameter</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p>inputFile</p> </td>
   <td><p>Ange InputStream som representerar det PDF-dokument som användningsrättigheterna ska tillämpas på. Du kan använda LiveCycle Rights Management eller AEM Forms dokumentsäkerhetsskyddade dokument.</p> </td>
  </tr>
  <tr>
   <td><p>certFile</p> </td>
   <td><p>Ange ett File-objekt som representerar en .jks-fil. .jks-filen är en nyckelfil. Det pekar på ett certifikat som ger användarrättigheter.</p> </td>
  </tr>
  <tr>
   <td><p>credentialPassword</p> </td>
   <td><p>Ange lösenordet för nyckelbehållaren. </p> </td>
  </tr>
  <tr>
   <td><p>usageRights</p> </td>
   <td><p>Anger ett objekt av typen <a href="https://help.adobe.com/en_US/livecycle/11.0/ProgramLC/javadoc/com/adobe/livecycle/readerextensions/client/UsageRights.html" target="_blank">UsageRights</a>. Objektet usageRights representerar individuella rättigheter som kan tillämpas på ett policyskyddat PDF-dokument.</p> </td>
  </tr>
 </tbody>
</table>

### Hämta användningsbehörighet för profilskyddade PDF-dokument.   {#retrieve-usage-rights-applied-to-a-policy-protected-pdf-document-nbsp}

Du kan använda Java-API:t `getDocumentUsageRights`för att hämta läsartilläggets användningsrättigheter som tillämpas på ett principskyddat PDF-dokument. Genom att hämta information om användningsrättigheter kan du lära dig mer om de funktioner som läsartillägget har aktiverat för det principskyddade PDF-dokumentet.

**Syntax:** `public GetUsageRightsResult getDocumentUsageRights(InputStream inDoc)`

<table>
 <tbody>
  <tr>
   <td><p><strong>Parameter</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p>inDoc</p> </td>
   <td><p>Ange InputStream som representerar det PDF-dokument som användningsrättigheterna ska hämtas från. Du kan använda LiveCycle Rights Management eller AEM Forms dokumentsäkerhetsskyddade dokument.</p> </td>
  </tr>
 </tbody>
</table>

#### Kodexempel {#code-sample}

```java
//Create a ServiceClientFactory instance
ServiceClientFactory factory = ServiceClientFactory.createInstance(connectionProps);
//Create a RightsManagementClient object
RightsManagementClient2 rmClient2= new RightsManagementClient2(factory);

String inputFileName = "C:\\Sample\\protected.pdf"; //Input file can be RM protected or unprotected pdf file
File certFile = new File("C:\\Sample\\cert.jks"); //RE certificate file
String password = "password"; //password for RE certificate
UsageRights usageRights = getUsageRights(true,true,false,false,true,true,false,false,false,false,true);

//RE rights to be applied on the file : FormFillIn, FormDataImportExport, SubmitStandalone, OnlineForms, DynamicFormField, DynamicFormPages, BarcodeDecoding, DigitalSignatures, Comments, CommentsOnline, EmbeddedFiles

InputStream inputFileStream = new FileInputStream(inputFileName);
InputStream output = rmClient2.getRightsManagementReaderExtensionService().applyUsageRights(inputFileStream, certFile, credentialPassword, rights);

String outputFileName = "C:\\Sample\\ReAdded.pdf";
//Save the PDF document
File myFile = new File(outputFileName);
FileOutputStream outputStream = new FileOutputStream(myFile);

int read = 0;
byte[] bytes = new byte[1024];

while ((read = output.read(bytes)) != -1) {

    outputStream.write(bytes, 0, read);
}

System.out.println("UsageRights applied successfully to the document. ");
 outputStream.close();
inputFileStream.close();

//Get Usage Rights for the output pdf document
InputStream fileWithRe = new FileInputStream(myFile);

GetUsageRightsResult usageRights = rmClient2.getRightsManagementReaderExtensionService().getDocumentUsageRights(fileWithRe);

UsageRights rights = usageRights.getRights();
String right1 = rights1.toString();
System.out.println("RE rights for the file are :\n"+right1);
 fileWithRe.close();
```

### Ta bort användningsbehörighet för ett profilskyddat PDF-dokument {#remove-usage-rights-of-a-policy-protected-pdf-document}

Du kan använda Java-API:t `removeUsageRights` för att ta bort användningsrättigheter från ett principskyddat dokument. Du måste ta bort användningsrättigheter från ett profilskyddat PDF-dokument för att kunna utföra andra AEM Forms-åtgärder i dokumentet. Du måste till exempel signera (eller certifiera) ett PDF-dokument digitalt innan du anger användningsbehörighet. Om du vill utföra åtgärder på ett policyskyddat dokument måste du därför ta bort användningsbehörighet från PDF-dokumentet, utföra andra åtgärder, t.ex. signera dokumentet digitalt och sedan återanvända användningsbehörighet för dokumentet.

**Syntax:** `InputStream removeUsageRights(InputStream inputFile)`

<table>
 <tbody>
  <tr>
   <td><p><strong>Parameter</strong></p> </td>
   <td><p><strong>Beskrivning</strong></p> </td>
  </tr>
  <tr>
   <td><p> </p> <p>inputFile</p> </td>
   <td>Ange InputStream som representerar det PDF-dokument från vilket användningsrättigheterna <br /> ska tas bort. Du kan använda LiveCycle Rights Management eller AEM Forms dokumentsäkerhetsskyddade dokument.</td>
  </tr>
 </tbody>
</table>

#### Kodexempel {#code-sample-1}

```java
//Create a ServiceClientFactory instance
ServiceClientFactory factory = ServiceClientFactory.createInstance(connectionProps);
//Create a RightsManagementClient object
RightsManagementClient2 rmClient2= new RightsManagementClient2(factory);

String inputFileName = "C:\\Sample\\fileWithRe.pdf"; //Input file can be RM protected or unprotected pdf file
InputStream inputFileStream = new FileInputStream(inputFileName);

InputStream fileStream = rmClient2.getRightsManagementReaderExtensionService().removeUsageRights(inputFileStream);

String outputFileName = "C:\\Sample\\ReRemoveded.pdf";
//Save the PDF document
File myFile = new File(outputFileName);
FileOutputStream outputStream = new FileOutputStream(myFile);

int read = 0;
byte[] bytes = new byte[1024];

while ((read = fileStream.read(bytes)) != -1) {

    outputStream.write(bytes, 0, read);
}
System.out.println("RE rights removed successfully from the document.");
outputStream.close();
inputFileStream.close();
```
