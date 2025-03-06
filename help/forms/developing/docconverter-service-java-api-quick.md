---
title: DocConverter Service Java&trade; API QuickStart(SOAP)
description: Lär dig hur du konverterar ett dokument till ett PDF/A-dokument och hanterar kompatibilitet med Java&trade; API Quick Start (SOAP).
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations,AEM Forms on JEE
exl-id: b42303c4-e328-4c57-8ff5-9f214b6c3acd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# DocConverter Service Java™ API Quick Start (SOAP) {#docconverter-service-java-api-quickstart-soap}

Java™ API Quick Start (SOAP) finns för tjänsten DocConverter.

[Snabbstart (SOAP-läge): Kontrollera PDF/A-kompatibilitet med Java](docconverter-service-java-api-quick.md#quick-start-soap-mode-determining-pdf-a-compliancy-using-the-java-api)

[Snabbstart (SOAP-läge): Konvertera ett dokument till ett PDF/A-dokument med Java](docconverter-service-java-api-quick.md#quick-start-soap-mode-converting-a-document-to-a-pdf-a-document-using-the-java-api)

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

>[!NOTE]
>
>Snabbstart i programmering med AEM-formulär baseras på Forms Server som distribueras på JBoss® Application Server och operativsystemet Microsoft® Windows. Om du använder ett annat operativsystem, till exempel UNIX®, ska du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

## Snabbstart (SOAP-läge): Konvertera ett dokument till ett PDF/A-dokument med Java™ API {#quick-start-soap-mode-converting-a-document-to-a-pdf-a-document-using-the-java-api}

I följande Java™-kodexempel konverteras ett PDF-dokument med namnet *Loan.pdf* till ett PDF/A-dokument som sparas som en PDF-fil med namnet *LoanArchive.pdf*. (Se [Konverterar dokument till PDF/A-dokument](/help/forms/developing/pdf-a-documents.md#converting-documents-to-pdf-a-documents).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-docconverter-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. adobe-utilities.jar
     * 5. jboss-client.jar (use a different JAR file if the Forms Server is not deployed
     * on JBoss)
     * 6. activation.jar (required for SOAP mode)
     * 7. axis.jar (required for SOAP mode)
     * 8. commons-codec-1.3.jar (required for SOAP mode)
     * 9.  commons-collections-3.1.jar  (required for SOAP mode)
     * 10. commons-discovery.jar (required for SOAP mode)
     * 11. commons-logging.jar (required for SOAP mode)
     * 12. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 13. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 14. jaxrpc.jar (required for SOAP mode)
     * 15. log4j.jar (required for SOAP mode)
     * 16. mail.jar (required for SOAP mode)
     * 17. saaj.jar (required for SOAP mode)
     * 18. wsdl4j.jar (required for SOAP mode)
     * 19. xalan.jar (required for SOAP mode)
     * 20. xbean.jar (required for SOAP mode)
     * 21. xercesImpl.jar (required for SOAP mode)
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     * The adobe-utilities.jar file is in the following path:
     * <install directory>/sdk/client-libs/jboss
     *
     * The jboss-client.jar file is in the following path:
     * <install directory>/jboss/bin/client
     *
     * SOAP required JAR files are in the following path:
     * <install directory>/sdk/client-libs/thirdparty
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include these additional JAR files
     *
     * For information about the SOAP
     * mode, see "Setting connection properties" in Programming
     * with AEM Forms
     */
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.docconverter.client.DocConverterServiceClient;
 import com.adobe.livecycle.docconverter.client.PDFAConversionOptionSpec;
 import com.adobe.livecycle.docconverter.client.PDFAConversionResult;
 
 public class CreatePDFADocumentSOAP {
 
     public static void main(String[] args) {
     try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory instance
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create a DocConverterServiceClient object
         DocConverterServiceClient docConverter = new DocConverterServiceClient(myFactory);
 
         //Reference a PDF document to convert to a PDF/A document
         FileInputStream myPDF = new FileInputStream("C:\\Adobe\Loan.pdf");
         Document inDoc = new Document(myPDF);
 
         //Create a PDFAConversionOptionSpec object and set
         //tracking information
         PDFAConversionOptionSpec spec = new PDFAConversionOptionSpec();
         spec.setLogLevel("FINE");
 
         //Convert the PDF document to a PDF/A document
         PDFAConversionResult result =  docConverter.toPDFA(inDoc,spec);
 
         //Save the PDF/A file
         Document pdfADoc= result.getPDFADocument();
         File pdfAFile = new File("C:\\Adobe\LoanArchive.pdf");
         pdfADoc.copyToFile(pdfAFile);
       }catch (Exception e) {
         e.printStackTrace();
     }
      }
 }
```

## Snabbstart (SOAP-läge): Kontrollera PDF/A-kompatibilitet med Java™ API {#quick-start-soap-mode-determining-pdf-a-compliancy-using-the-java-api}

I följande Java™-kodexempel avgörs om PDF-indatadokumentet är PDF/A-kompatibelt. Indata-PDF-dokumentet som skickas till tjänsten DocConverter heter *LoanArchive.pdf*. Valideringsresultat skrivs till en XML-fil med namnet *ValidationResults.xml*. (Se [Programmatiskt fastställa PDF/A-kompatibilitet](/help/forms/developing/pdf-a-documents.md#programmatically-determining-pdf-a-compliancy).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-docconverter-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. adobe-utilities.jar
     * 5. jboss-client.jar (use a different JAR file if the Forms Server is not deployed
     * on JBoss)
     * 6. activation.jar (required for SOAP mode)
     * 7. axis.jar (required for SOAP mode)
     * 8. commons-codec-1.3.jar (required for SOAP mode)
     * 9.  commons-collections-3.1.jar  (required for SOAP mode)
     * 10. commons-discovery.jar (required for SOAP mode)
     * 11. commons-logging.jar (required for SOAP mode)
     * 12. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 13. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 14. jaxrpc.jar (required for SOAP mode)
     * 15. log4j.jar (required for SOAP mode)
     * 16. mail.jar (required for SOAP mode)
     * 17. saaj.jar (required for SOAP mode)
     * 18. wsdl4j.jar (required for SOAP mode)
     * 19. xalan.jar (required for SOAP mode)
     * 20. xbean.jar (required for SOAP mode)
     * 21. xercesImpl.jar (required for SOAP mode)
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     * The adobe-utilities.jar file is in the following path:
     * <install directory>/sdk/client-libs/jboss
     *
     * The jboss-client.jar file is in the following path:
     * <install directory>/jboss/bin/client
     *
     * SOAP required JAR files are in the following path:
     * <install directory>/sdk/client-libs/thirdparty
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include these additional JAR files
     *
     * For information about the SOAP
     * mode, see "Setting connection properties" in Programming
     * with AEM Forms
     */
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.docconverter.client.DocConverterServiceClient;
 import com.adobe.livecycle.docconverter.client.PDFAValidationOptionSpec;
 import com.adobe.livecycle.docconverter.client.PDFAValidationResult;
 
 public class IsDocumentPDFASOAP {
 
     public static void main(String[] args) {
     try{
         //Set connection properties required to invoke AEM Forms using SOAP mode
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory instance
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create a DocConverterServiceClient object
         DocConverterServiceClient docConverter = new DocConverterServiceClient(myFactory);
 
         //Reference a PDF document used to determine PDF/A compliancy
         FileInputStream myPDF = new FileInputStream("C:\\Adobe\LoanArchive.pdf");
         Document inDoc = new Document(myPDF);
 
         //Create a PDFAValidationOptionSpec object and set
         //run-time values
         PDFAValidationOptionSpec spec = new PDFAValidationOptionSpec();
         spec.setCompliance(PDFAValidationOptionSpec.Compliance.PDFA_1B);
         spec.setResultLevel(PDFAValidationOptionSpec.ResultLevel.DETAILED);
         spec.setLogLevel("FINE");
         spec.setIgnoreUnusedResource(true);
 
         //Determine if the PDF document is PDF/A compliant
         PDFAValidationResult result =  docConverter.isPDFA(inDoc,spec)    ;
 
         //Get the results of the operation
         Boolean isPDFA = result.getIsPDFA();
 
         //Get XML data that contains validaction results
         Document validationResults =  result.getValidationLog();
         File file= new File("C:\\Adobe\ValidationResults.xml");
         validationResults .copyToFile(file);
 
     }catch (Exception e) {
         e.printStackTrace();
     }
      }
 }
```
