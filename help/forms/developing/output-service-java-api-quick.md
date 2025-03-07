---
title: Snabbstart för Java API för Output Service (SOAP)
description: Använd Java API Quick Start (SOAP) för utdatatjänsten för att skapa ett PDF-dokument, skapa ett PDF-dokument baserat på programmets XDP-fil, skapa ett PDF/A-dokument, skicka dokument till utdatatjänsten, skicka ett dokument i AEM Forms-databasen till utdatatjänsten, skapa ett PDF-dokument baserat på fragment, skriva ut till en fil, skicka en utskriftsström till en nätverksskrivare, skapa flera PDF-filer, skapa sökregler, omvandla ett PDF-dokument.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations,AEM Forms on JEE
hide: true
hidefromtoc: true
exl-id: a85333ac-427f-4730-b6d6-04c4b9419883
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1302'
ht-degree: 0%

---

# Snabbstart för Java API för Output Service (SOAP) {#output-service-java-api-quick-start-soap}

Java API Quick Start (SOAP) är tillgängligt för Output-tjänsten.

[Snabbstart (SOAP-läge): Skapa ett PDF-dokument med Java API](output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa ett PDF-dokument baserat på en XDP-programfil med Java API](output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-based-on-an-application-xdp-file-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa ett PDF/A-dokument med Java API](output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-a-document-using-the-java-api)

[Snabbstart (SOAP-läge): skicka dokument till utdatatjänsten med Java API](output-service-java-api-quick.md#quick-start-soap-mode-passing-documents-to-the-output-service-using-the-java-api)

[Snabbstart (SOAP-läge): skicka ett dokument i AEM Forms-databasen till utdatatjänsten med Java API](output-service-java-api-quick.md#quick-start-soap-mode-passing-a-document-located-in-the-repository-to-the-output-service-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa ett PDF-dokument baserat på fragment med Java API](#quick-start-soap-mode-creating-a-pdf-document-based-on-fragments-using-the-java-api)

[Snabbstart (SOAP-läge): Skriva ut till en fil med Java API](#quick-start-soap-mode-printing-to-a-file-using-the-java-api)

[Snabbstart (SOAP-läge): Skicka en utskriftsström till en nätverksskrivare med Java API](output-service-java-api-quick.md#quick-start-soap-mode-sending-a-print-stream-to-a-network-printer-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa flera PDF-filer med Java API](output-service-java-api-quick.md#quick-start-soap-mode-creating-multiple-pdf-files-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa sökregler med Java API](output-service-java-api-quick.md#quick-start-soap-mode-creating-search-rules-using-the-java-api)

[Snabbstart (SOAP-läge): Omforma ett PDF-dokument med Java API](output-service-java-api-quick.md#quick-start-soap-mode-transforming-a-pdf-document-using-the-java-api)

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

>[!NOTE]
>
>Quick Starts in Programming with AEM forms are based on the Forms Server operating system. Om du använder ett annat operativsystem, till exempel UNIX, ska du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

## Snabbstart (SOAP-läge): Skapa ett PDF-dokument med Java API {#quick-start-soap-mode-creating-a-pdf-document-using-the-java-api}

I följande Java-kodexempel skapas ett PDF-dokument med namnet *Loan.pdf*. Det här PDF-dokumentet är baserat på en formulärdesign med namnet *Loan.xdp* och en XML-datafil med namnet *Loan.xml*. *Loan.pdf* skrivs till mappen C:\Adobe på J2EE-programservern som är värd för AEM Forms, inte till klientdatorn. (Se [Skapa PDF-dokument](/help/forms/developing/creating-document-output-streams.md#creating-pdf-documents).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 
 public class CreatePDFDocument {
 
     public static void main(String[] args) {
 
         try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference form data
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
         Document inXMData = new Document (fileInputStream);
 
         //Set PDF run-time options
         PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
         outputOptions.setFileURI("C:\\Adobe\Loan.pdf");
 
         //Set rendering run-time options
         RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
         pdfOptions.setLinearizedPDF(true);
         pdfOptions.setAcrobatVersion(AcrobatVersion.Acrobat_9);
 
         //Create a PDF document
         OutputResult outputDocument = outClient.generatePDFOutput(
             TransformationFormat.PDF,
             "Loan.xdp",
             "C:\\Adobe",
             outputOptions,
             pdfOptions,
             inXMData
         );
 
         //Retrieve the results of the operation
         Document metaData = outputDocument.getStatusDoc();
         File myFile = new File("C:\\Adobe\Output.xml");
         metaData.copyToFile(myFile);
         }
         catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Skapa ett PDF-dokument baserat på en XDP-programfil med Java API {#quick-start-soap-mode-creating-a-pdf-document-based-on-an-application-xdp-file-using-the-java-api}

I följande Java-kodexempel skapas ett PDF-dokument med namnet *Loan.pdf*. Det här PDF-dokumentet är baserat på en formulärdesign med namnet *Loan.xdp* och en XML-datafil med namnet *Loan.xml*. XDP-filen distribueras som en del av ett AEM Forms-program med namnet `Applications/FormsApplication`. Observera att URI-sökvägen är `repository:///Applications/FormsApplication/1.0/FormsFolder/`. *Loan.pdf* skrivs till mappen C:\Adobe på J2EE-programservern som är värd för AEM Forms, inte till klientdatorn. (Se [Skapa PDF-dokument](/help/forms/developing/creating-document-output-streams.md#creating-pdf-documents).)

>[!NOTE]
>
>Innan du kör den här snabbstarten måste du skapa ett AEM Forms-program med namnet Applications/FormsApplication. Skapa en mapp i programmet som heter FormsFolder och placera XDP-filen i mappen. Mer information finns i [Skapa ett PDF-dokument ](/help/forms/developing/creating-document-output-streams.md)*.*

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * These JAR files are in the following path:
     * <install directory>/Adobe/adobe_experience_manager_forms/SDK/client-libs/common
     *
     * <install directory>/Adobe/adobe_experience_manager_forms/SDK/client-libs/jboss
     *
     * <install directory>/Adobe/adobe_experience_manager_forms/jboss/bin/client
     *
     * If you want to invoke a remote AEM Forms instance and there is a
     * firewall between the client application and AEM Forms, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/Adobe/adobe_experience_manager_forms/SDK/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 
 public class CreatePDFDocumentFromLCApp {
 
     public static void main(String[] args) {
 
         try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference form data
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
         Document inXMData = new Document (fileInputStream);
 
         //Set PDF run-time options
         PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
         outputOptions.setFileURI("C:\\Adobe\Loan.pdf");
 
         //Set rendering run-time options
         RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
         pdfOptions.setLinearizedPDF(true);
         pdfOptions.setAcrobatVersion(AcrobatVersion.Acrobat_9);
 
         //Create a PDF document -- reference an XDP file named Loan.xdp that is deployed as part of
         //a AEM Forms application named Applications/FormsApplication. The XDP file is located
         //in a folder named FormsFolder
         OutputResult outputDocument = outClient.generatePDFOutput(
             TransformationFormat.PDF,
             "Loan.xdp",
             "repository:///Applications/FormsApplication/1.0/FormsFolder/",
             outputOptions,
             pdfOptions,
             inXMData
         );
 
         //Retrieve the results of the operation
         Document metaData = outputDocument.getStatusDoc();
         File myFile = new File("C:\\Adobe\Output.xml");
         metaData.copyToFile(myFile);
         }
         catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 }
 
 
```

## Snabbstart (SOAP-läge): skicka ett dokument i databasen till utdatatjänsten med Java API {#quick-start-soap-mode-passing-a-document-located-in-the-repository-to-the-output-service-using-the-java-api}

Följande Java-kod hämtar en XDP-fil från databasen och skickar den till utdatatjänsten i instansen `com.adobe.idp.Document`. XDP-filen distribueras som en del av ett AEM Forms-program med namnet `Applications/FormsApplication`. Observera att URI-sökvägen är `repository:///Applications/FormsApplication/1.0/FormsFolder/`.

>[!NOTE]
>
>Databas-API:t används för att hämta XDP-filen från den här platsen. (Se [Läser resurser](/help/forms/developing/aem-forms-repository.md#reading-resources).)

Lägg också märke till att innehållets rotvärde `repository:///Applications/FormsApplication/1.0/FormsFolder/` skickas till `OutputClient`-objektets `generatePDFOutput2`-metod (den andra parametern). Det här värdet skickas till utdatatjänsten för att informera utdatatjänsten om att säkerheten, t.ex. bilder, lagras på den här platsen.

>[!NOTE]
>
>Du kan ange innehållets rotvärde på samma sätt när du anropar metoden `generatePrintedOutput2`.

*Loan.pdf* skrivs till mappen C:\Adobe på J2EE-programservern som är värd för AEM Forms. (Se [Skicka dokument i databasen till utdatatjänsten](/help/forms/developing/creating-document-output-streams.md#passing-documents-located-in-the-repository-to-the-output-service).)

>[!NOTE]
>
>Innan du kör den här snabbstarten måste du skapa ett AEM Forms-program med namnet Applications/FormsApplication. Skapa en mapp i programmet som heter FormsFolder och placera XDP-filen i mappen.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-repository-client.jar
     * 3. adobe-livecycle-client.jar
     * 4. adobe-usermanager-client.jar
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import com.adobe.repository.bindings.dsc.client.ResourceRepositoryClient;
 
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 
 public class CreatePDFFFromRepository {
 
     public static void main(String[] args) {
 
         try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference form data
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
         Document inXMData = new Document (fileInputStream);
 
         //Set PDF run-time options
         PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
         outputOptions.setFileURI("C:\\Adobe\Loan.pdf"); // this PDF form is saved on the server
 
         //Get the form design from the AEM Forms Repository
         Document formDesign =  GetFormDesign(myFactory);
 
         //Set rendering run-time options
         RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
         pdfOptions.setLinearizedPDF(true);
         pdfOptions.setAcrobatVersion(AcrobatVersion.Acrobat_9);
 
         //Create a non-interactive PDF document
         OutputResult outputDocument = outClient.generatePDFOutput2(
             TransformationFormat.PDF,
             "repository:///Applications/FormsApplication/1.0/FormsFolder/",
             formDesign,
             outputOptions,
             pdfOptions,
             inXMData
         );
 
         //Save the non-interactive PDF form as a PDF file on the client computer
         Document pdfForm = outputDocument.getGeneratedDoc();
         File myFile = new File("C:\\Adobe\Loan.pdf");
         pdfForm.copyToFile(myFile);
         }
         catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 
     // Retrieve the form design from the following Repository path:
     // /Applications/FormsApplication/1.0/FormsFolder/Loan.xdp
     private static Document GetFormDesign(ServiceClientFactory myFactory)
     {
     try{
 
         // Create a ResourceRepositoryClient object using the service client factory
         ResourceRepositoryClient repositoryClient = new ResourceRepositoryClient(myFactory);
 
         // Specify the path in the Repository to Loan.xdp
         String resourceUri =  "/Applications/FormsApplication/1.0/FormsFolder/Loan.xdp";
 
         // Retrieve the XDP file
         Document doc = repositoryClient.readResourceContent(resourceUri);
 
            //Return the Document instance
            return  doc;
          }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
         return null;
     }
 }
 
 
 
```

## Snabbstart (SOAP-läge): Skapa ett PDF-dokument med Java API {#quick_start_soap_mode_creating_a_pdf_document_using_the_java_api-1}

I följande Java-kodexempel skapas ett PDF-dokument med namnet *Loan.pdf*. Det här PDF-dokumentet är baserat på en formulärdesign med namnet *Loan.xdp* och en XML-datafil med namnet *Loan.xml*. *Loan.pdf* skrivs till mappen C:\Adobe på J2EE-programservern som är värd för AEM Forms, inte till klientdatorn. (Se [Skapa PDF-dokument](/help/forms/developing/creating-document-output-streams.md#creating-pdf-documents).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
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
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 
 public class CreatePDFDocumentSOAP {
 
     public static void main(String[] args) {
 
     try{
         //Set connection properties required to invoke AEM Forms using SOAP mode
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference form data
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
         Document inXMData = new Document (fileInputStream);
 
         //Set PDF run-time options
         PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
         outputOptions.setFileURI("C:\\Adobe\Loan.pdf");
 
         //Set rendering run-time options
         RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
         pdfOptions.setLinearizedPDF(true);
         pdfOptions.setAcrobatVersion(AcrobatVersion.Acrobat_9);
 
         //Create a PDF document
         OutputResult outputDocument = outClient.generatePDFOutput(
             TransformationFormat.PDF,
             "Loan.xdp",
             "C:\\Adobe",
             outputOptions,
             pdfOptions,
             inXMData
         );
 
         //Retrieve the results of the operation
         Document metaData = outputDocument.getStatusDoc();
         File myFile = new File("C:\\Adobe\Output.xml");
         metaData.copyToFile(myFile);
         }
         catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 }
```

## Snabbstart (SOAP-läge): Skapa ett PDF/A-dokument med Java API {#quick-start-soap-mode-creating-a-pdf-a-document-using-the-java-api}

I följande Java-kodexempel skapas ett PDF/A-dokument med namnet *LoanArchive.pdf*. Det här PDF-dokumentet är baserat på en formulärdesign med namnet *Loan.xdp* och en XML-datafil med namnet *Loan.xml*. *LoanArchive.pdf* skrivs till mappen C:\Adobe på J2EE-programservern som är värd för AEM Forms, inte till klientdatorn. (Se [Skapa PDF/A-dokument](/help/forms/developing/creating-document-output-streams.md#creating-pdf-a-documents).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 public class CreatePDFADocument {
 
     public static void main(String[] args) {
 
     try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference an XML data source to merge with the form design
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
         Document inXMData = new Document (fileInputStream);
 
         //Set PDF run-time options
         PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
         outputOptions.setFileURI("C:\\Adobe\LoanArchive.pdf");
 
         //Set rendering run-time options
         RenderOptionsSpec pdfAOptions = new RenderOptionsSpec();
         pdfAOptions.setPDFAConformance(PDFAConformance.A);
         pdfAOptions.setPDFARevisionNumber(PDFARevisionNumber.Revision_1);
 
 
         //Create a PDF/A document
         OutputResult outputDocument = outClient.generatePDFOutput(
             TransformationFormat.PDFA,
             "Loan.xdp",
             "C:\\Adobe",
             outputOptions,
             pdfAOptions,
             inXMData
         );
 
         //Write the results of the operation to OutputLog.xml
         Document resultData = outputDocument.getStatusDoc();
         File myFile = new File("C:\\Adobe\OutputLog.xml");
         resultData.copyToFile(myFile);
 
         }catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): skicka dokument till utdatatjänsten med Java API {#quick-start-soap-mode-passing-documents-to-the-output-service-using-the-java-api}

Följande Java-snabbstart hämtar filen *Loan.xdp* från Content Services. Den här XDP-filen finns i `space /Company Home/Form Designs`. XDP-filen returneras i en `com.adobe.idp.Document`-instans. `com.adobe.idp.Document`-instansen skickas till utdatatjänsten. Det icke-interaktiva formuläret sparas som en PDF-fil med namnet *Loan.pdf *på klientdatorn. Eftersom Arkiv-URI är inställt sparas även PDF-filen *Loan.pdf *på J2EE-programservern som är värd för AEM Forms. (Se [Skicka dokument i Content Services ES2 till utdatatjänsten](/help/forms/developing/creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-contentservices-client.jar
     * 3. adobe-livecycle-client.jar
     * 4. adobe-usermanager-client.jar
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.contentservices.client.CRCResult;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 import com.adobe.livecycle.output.client.*;
 
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 
 public class CreatePDFFFromContentServices {
 
     public static void main(String[] args) {
 
         try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference form data
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
         Document inXMData = new Document (fileInputStream);
 
         //Set PDF run-time options
         PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
         outputOptions.setFileURI("C:\\Adobe\Loan.pdf"); // this PDF form is saved on the server
 
         //Get the form design from Content Services
         Document formDesign =  GetFormDesign(myFactory);
 
         //Set rendering run-time options
         RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
         pdfOptions.setLinearizedPDF(true);
         pdfOptions.setAcrobatVersion(AcrobatVersion.Acrobat_9);
 
         //Create a non-interactive PDF document
         OutputResult outputDocument = outClient.generatePDFOutput2(
             TransformationFormat.PDF,
             "C:\\Adobe",
             formDesign,
             outputOptions,
             pdfOptions,
             inXMData
         );
 
         //Save the non-interactive PDF form as a PDF file on the client computer
         Document pdfForm = outputDocument.getGeneratedDoc();
         File myFile = new File("C:\\Adobe\Loan.pdf");
         pdfForm.copyToFile(myFile);
         }
         catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 
     //Retrieve the form design from Content Services ES2
     private static Document GetFormDesign(ServiceClientFactory myFactory)
     {
         try{
 
         //Create a DocumentManagementServiceClientImpl object
         DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
         //Specify the name of the store and the content to retrieve
            String storeName = "SpacesStore";
            String nodeName  = "/Company Home/Form Designs/Loan.xdp";
 
            //Retrieve /Company Home/Form Designs/Loan.xdp
            CRCResult content = docManager.retrieveContent(
                      storeName,
                      nodeName,
                      "");
 
            //Return the Document instance
             Document doc =content.getDocument();
             return  doc;
          }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
         return null;
     }
 }
 
```

## Snabbstart (SOAP-läge): Skapa ett PDF-dokument baserat på fragment med Java API {#quick-start-soap-mode-creating-a-pdf-document-based-on-fragments-using-the-java-api}

I följande Java-kodexempel skapas ett PDF-dokument som är baserat på en formulärdesign som har sammanställts av Assembler-tjänsten. Med Assembler-tjänsten samlas fragment i flera XDP-filer in i en enda formulärdesign. Programlogik som anropar Assembler-tjänsten finns i en användardefinierad metod med namnet `GetFormDesign`. Det icke-interaktiva formuläret sparas som en PDF-fil med namnet *Loan.pdf *på klientdatorn. (Se [Skapa PDF-dokument med fragment](/help/forms/developing/creating-document-output-streams.md#creating-pdf-documents-using-fragments).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     * 20. adobe-assembler-client.jar
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     *
     * This is the DDX file is used to assemble multiple XDP documents:
     * <?xml version="1.0" encoding="UTF-8"?>
     * <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
     * <XDP result="tuc018result.xdp">
     * <XDP source="tuc018_template_flowed.xdp">
     * <XDPContent insertionPoint="ddx_fragment" source="tuc018_contact.xdp" fragment="subPatientContact" required="false"/>
     * <XDPContent insertionPoint="ddx_fragment" source="tuc018_patient.xdp" fragment="subPatientPhysical" required="false"/>
     * <XDPContent insertionPoint="ddx_fragment" source="tuc018_patient.xdp" fragment="subPatientHealth" required="false"/>
     * </XDP>
     * </XDP>
     * </DDX>
     */
 import com.adobe.livecycle.assembler.client.AssemblerOptionSpec;
 import com.adobe.livecycle.assembler.client.AssemblerResult;
 import com.adobe.livecycle.assembler.client.AssemblerServiceClient;
 import com.adobe.livecycle.output.client.*;
 
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 
 public class CreatePDFFromFragments {
 
     public static void main(String[] args) {
 
         try{
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create an OutputClient object
             OutputClient outClient = new OutputClient(myFactory);
 
             //Reference form data
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
             Document inXMData = new Document (fileInputStream);
 
             //Set PDF run-time options
             PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
             outputOptions.setFileURI("C:\\Adobe\Loan.pdf"); // this PDF form is saved on the server
 
             //Get the form design from Assembler service
             Document formDesign =  GetFormDesign(myFactory);
 
             //Set rendering run-time options
             RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
             pdfOptions.setLinearizedPDF(true);
             pdfOptions.setAcrobatVersion(AcrobatVersion.Acrobat_9);
 
             //Create a non-interactive PDF document
             OutputResult outputDocument = outClient.generatePDFOutput2(
                 TransformationFormat.PDF,
                 "C:\\Adobe",
                 formDesign,
                 outputOptions,
                 pdfOptions,
                 inXMData
             );
 
             //Save the non-interactive PDF form as a PDF file on the client computer
             Document pdfForm = outputDocument.getGeneratedDoc();
             File myFile = new File("C:\\Adobe\Loan.pdf");
             pdfForm.copyToFile(myFile);
             }
             catch (Exception ee)
             {
                 ee.printStackTrace();
             }
         }
 
         //Retrieve the form design from Assembler service
         private static Document GetFormDesign(ServiceClientFactory myFactory)
         {
             try{
 
                 //Create an AssemblerServiceClient object
                 AssemblerServiceClient assemblerClient = new AssemblerServiceClient(myFactory);
 
                 //Create a FileInputStream object based on an existing DDX file
                 FileInputStream myDDXFile = new FileInputStream("C:\\Adobe\fragmentDDX.xml");
 
                 //Create a Document object based on the DDX file
                 Document myDDX = new Document(myDDXFile);
 
                 //Create a Map object to store the input XDP files
                 Map inputs = new HashMap();
                 FileInputStream inSource = new FileInputStream("C:\\Adobe\tuc018_template_flowed.xdp");
                 FileInputStream inFragment1 = new FileInputStream("C:\\Adobe\tuc018_contact.xdp");
                 FileInputStream inFragment2 = new FileInputStream("C:\\Adobe\tuc018_patient.xdp");
 
                 //Create a Document object
                 Document myMapSource = new Document(inSource);
 
                 //Create a Document object
                 Document inFragment1Doc = new Document(inFragment1);
 
                 //Create a Document object
                 Document inFragment2Doc = new Document(inFragment2);
 
                 //Place all the XDP files into the MAP
                 inputs.put("tuc018_template_flowed.xdp",myMapSource);
                 inputs.put("tuc018_contact.xdp",inFragment1Doc);
                 inputs.put("tuc018_patient.xdp",inFragment2Doc);
 
 
                 //Create an AssemblerOptionsSpec object
                 AssemblerOptionSpec assemblerSpec = new AssemblerOptionSpec();
                 assemblerSpec.setFailOnError(false);
 
                 //Submit the job to Assembler service
                 AssemblerResult jobResult = assemblerClient.invokeDDX(myDDX,inputs,assemblerSpec);
                 java.util.Map allDocs = jobResult.getDocuments();
 
                 //Retrieve the result PDF document from the Map object
                 Document outDoc = null;
 
                 //Iterate through the map object to retrieve the result XDP document
                 for (Iterator i = allDocs.entrySet().iterator(); i.hasNext();) {
                     // Retrieve the Map object?s value
                     Map.Entry e = (Map.Entry)i.next();
 
                     //Get the key name as specified in the
                     //DDX document
                     String keyName = (String)e.getKey();
                     if (keyName.equalsIgnoreCase("tuc018result.xdp"))
                     {
                         Object o = e.getValue();
                         outDoc = (Document)o;
 
                     }
                 }
 
             return outDoc;
             }catch (Exception e) {
                 e.printStackTrace();
             }
             return null;
         }
     }
 
 
```

## Snabbstart (SOAP-läge): Skriva ut till en fil med Java API {#quick-start-soap-mode-printing-to-a-file-using-the-java-api}

I följande Java-kodexempel skrivs en utdataström ut till en PostScript-fil med namnet *MortgageForm.ps*. (Se [Skriva ut till filer](/help/forms/developing/creating-document-output-streams.md#printing-to-files).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 public class PrintToFile {
 
     public static void main(String[] args) {
 
         try{
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create an OutputClient object
             OutputClient outClient = new OutputClient(myFactory);
 
             //Reference XML data that represents form data
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
             Document inputXML = new Document(fileInputStream);
 
             //Set print run-time options required to print to a file
             PrintedOutputOptionsSpec printOptions = new PrintedOutputOptionsSpec();
             printOptions.setFileURI("C:\\Adobe\MortgageForm.ps");
 
             //Print the print stream to a PostScript file
             OutputResult outputDocument = outClient.generatePrintedOutput(
                     PrintFormat.PostScript,
                     "Loan.xdp",
                     "C:\\Adobe",
                     null,
                     printOptions,
                     inputXML);
 
             //Write the results of the operation to OutputLog.xml
             Document resultData = outputDocument.getStatusDoc();
             File myFile = new File("C:\\Adobe\OutputLog.xml");
             resultData.copyToFile(myFile);
             System.out.println("AEM Forms printed to MortgageForm.ps");
         }
     catch (Exception ee)
             {
             ee.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Skicka en utskriftsström till en nätverksskrivare med Java API {#quick-start-soap-mode-sending-a-print-stream-to-a-network-printer-using-the-java-api}

I följande Java-kodexempel skickas en PostScript-utskriftsström till en nätverksskrivare med namnet *\\Printer1\Printer*. Två kopior skickas till skrivaren. (Se [Skicka utskriftsströmmar till skrivare](/help/forms/developing/creating-document-output-streams.md#sending-print-streams-to-printers).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import java.util.*;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.output.client.*;
 
 public class SendToPrinter {
 
     public static void main(String[] args) {
 
         try{
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create an OutputClient object
             OutputClient outClient = new OutputClient(myFactory);
 
             //Reference XML data that represents form data
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
             Document inputXML = new Document(fileInputStream);
 
             //Set print run-time options required to print to a file
             PrintedOutputOptionsSpec printOptions = new PrintedOutputOptionsSpec();
 
             //Set the number of copies to print
             printOptions.setCopies(2);
 
             //Turn on the Staple option
             printOptions.setStaple(Staple.on);
 
             //Create a PostScript output stream based on the form design named Loan.xdp and
             //the data in the XML file
             OutputResult outputDocument = outClient.generatePrintedOutput(
                     PrintFormat.PostScript,
                     "Loan.xdp",
                     "C:\\Adobe",
                     "C:\\Adobe",
                     printOptions,
                     inputXML);
 
             //Get a Document object that stores the PostScript print stream
             Document psPrintStream = outputDocument.getGeneratedDoc();
 
             //Specify the print server and the printer name
             String printServer = "\\\ottprint";
             String printerName = "\\\ottprint\Balsom";
 
             //Send the PostScript print stream to the printer
             outClient.sendToPrinter(
                     psPrintStream,
                     PrinterProtocol.SharedPrinter,
                     printServer,
                     printerName);
             }
         catch (Exception ee)
             {
             ee.printStackTrace();
             }
     }
 }
 
```

## Snabbstart (SOAP-läge): Skapa flera PDF-filer med Java API {#quick-start-soap-mode-creating-multiple-pdf-files-using-the-java-api}

Följande Java-kod skapar flera PDF-filer för varje datapost som finns i en XML-datafil med namnet *Loan_data_batch.xml*. Filerna skrivs till katalogen C:\Adobe. PDF-filerna skrivs till mappen C:\Adobe på J2EE-programservern som är värd för AEM Forms, inte till klientdatorn. (Se [Skapa flera utdatafiler](/help/forms/developing/creating-document-output-streams.md#creating-multiple-output-files).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.output.client.*;
 
 public class CreateBatchFiles {
 
     public static void main(String[] args) {
 
         try{
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create an OutputClient object
             OutputClient outClient = new OutputClient(myFactory);
 
             //Reference form data that contains multiple records
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan_data_batch.xml");
             Document inXMData = new Document (fileInputStream);
 
             //Set run-time options to generate many PDF files
             PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
             outputOptions.setFileURI("C:\\Adobe\Loan.pdf");
             outputOptions.setGenerateManyFiles(true);
             outputOptions.setRecordName("LoanRecord");
 
 
             //Set rendering run-time options
             RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
             pdfOptions.setCacheEnabled(new Boolean(true));
 
 
             //Create multiple PDF files
             OutputResult outputDocument = outClient.generatePDFOutput(
                 TransformationFormat.PDF,
                 "Loan.xdp",
                 "C:\\Adobe",
                 outputOptions,
                 pdfOptions,
                 inXMData
                 );
 
             //Retrieve the results of the operation
             Document metaData = outputDocument.getStatusDoc();
             File myFile = new File("C:\\Adobe\Output.xml");
             metaData.copyToFile(myFile);
             }
             catch (Exception ee)
             {
                 ee.printStackTrace();
             }
     }
 }
 
 
```

## Snabbstart (SOAP-läge): Skapa sökregler med Java API {#quick-start-soap-mode-creating-search-rules-using-the-java-api}

I följande Java-kodexempel skapas två textmönster som utdatatjänsten söker efter. Det första textmönstret är Låta. Om det hittas använder utdatatjänsten formulärdesignen *Mortgage.xdp*. Det andra textmönstret är Automomobile. Om det hittas använder Output-tjänsten formulärdesignen *AutomobleLoan.xdp*. Om inget av textmönstren finns, använder Output-tjänsten standardformulärdesignen som heter* Loan.xdp. *(Se [Skapa sökregler](/help/forms/developing/creating-document-output-streams.md#creating-search-rules).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 public class CreateSearchRules {
 
     public static void main(String[] args) {
         try{
 
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create an OutputClient object
             OutputClient outClient = new OutputClient(myFactory);
 
             //Reference form data
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.xml");
             Document inXMData = new Document (fileInputStream);
 
             //Define two text patterns
             Rule mortageRule = new Rule();
             mortageRule.setPattern("Mortgage");
             mortageRule.setForm("Mortgage.xdp");
 
             Rule automobileRule = new Rule();
             automobileRule.setPattern("Automobile");
             automobileRule.setForm("AutomobileLoan.xdp");
 
             //Add the Rules to a List object
             List<Rule> myList = new ArrayList<Rule>();
             myList.add(mortageRule);
             myList.add(automobileRule);
 
             //Define PDF run-time options which includes Search Rules
             PDFOutputOptionsSpec outputOptions = new PDFOutputOptionsSpec();
             outputOptions.setFileURI("C:\\Adobe\Loan.pdf");
             outputOptions.setRules(myList);
             outputOptions.setLookAhead(900);
 
             //Define rendering run-time options
             RenderOptionsSpec pdfOptions = new RenderOptionsSpec();
             pdfOptions.setCacheEnabled(new Boolean(true));
 
             //Create a PDF document based on multiple form designs
             OutputResult outputDocument = outClient.generatePDFOutput(
                 TransformationFormat.PDF,
                 "Loan.xdp",
                 "C:\\Adobe",
                 outputOptions,
                 pdfOptions,
                 inXMData
             );
 
             //Write the results of the operation to OutputLog.xml
             Document resultData = outputDocument.getStatusDoc();
             File myFile = new File("C:\\Adobe\OutputLog.xml");
             resultData.copyToFile(myFile);
             }
         catch (Exception ee)
                 {
                     ee.printStackTrace();
                 }
         }
 }
 
```

## Snabbstart (SOAP-läge): Omforma ett PDF-dokument med Java API {#quick-start-soap-mode-transforming-a-pdf-document-using-the-java-api}

I följande Java-kodexempel omvandlas ett interaktivt PDF-dokument med namnet *Loan.pdf* till ett icke-interaktivt PDF-dokument med namnet *NonInteractiveLoan.pdf*. (Se [Förenkla PDF-dokument](/help/forms/developing/creating-document-output-streams.md#flattening-pdf-documents).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. activation.jar (required for SOAP mode)
     * 5. axis.jar (required for SOAP mode)
     * 6. commons-codec-1.3.jar (required for SOAP mode)
     * 7. commons-collections-3.2.jar  (required for SOAP mode)
     * 8. commons-discovery.jar (required for SOAP mode)
     * 9. commons-logging.jar (required for SOAP mode)
     * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode)
     * 11. jaxen-1.1-beta-9.jar (required for SOAP mode)
     * 12. jaxrpc.jar (required for SOAP mode)
     * 13. log4j.jar (required for SOAP mode)
     * 14. mail.jar (required for SOAP mode)
     * 15. saaj.jar (required for SOAP mode)
     * 16. wsdl4j.jar (required for SOAP mode)
     * 17. xalan.jar (required for SOAP mode)
     * 18. xbean.jar (required for SOAP mode)
     * 19. xercesImpl.jar (required for SOAP mode)
     *
     * The JBoss files must be kept in the jboss\client folder. You can copy the client folder to
     * your local development environment and then include the 3 JBoss JAR files in your class path
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     *
     * <install directory>/jboss/bin/client
     *
     * If you want to invoke a remote Forms Server instance and there is a
     * firewall between the client application and the server, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/sdk/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms Java library files" in Programming
     * with AEM Forms
     */
 import com.adobe.livecycle.output.client.*;
 import java.util.*;
 import java.io.File;
 import java.io.FileInputStream;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 public class TransformPDF {
 
     public static void main(String[] args) {
 
     try{
         //Set connection properties required to invoke AEM Forms
         Properties connectionProps = new Properties();
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
         //Create a ServiceClientFactory object
         ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
         //Create an OutputClient object
         OutputClient outClient = new OutputClient(myFactory);
 
         //Reference an interactive PDF document to transform
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.pdf");
         Document inPDFDoc = new Document (fileInputStream);
 
         //Transform the PDF document to a non-interactive PDF document
         Document transformedDocument = outClient.transformPDF(
                 inPDFDoc,
                 TransformationFormat.PDF,
                 null,
                 null,
                 null);
 
         //Save the non-interactive PDF document
         File myFile = new File("C:\\Adobe\NonInteractiveLoan.pdf");
         transformedDocument.copyToFile(myFile);
 
     }catch (Exception ee)
         {
             ee.printStackTrace();
         }
     }
 }
 
```
