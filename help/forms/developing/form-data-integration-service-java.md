---
title: JavaAPI snabbstart (SOAP) för integreringstjänsten för formulärdata
description: Använd tjänsten för integrering av formulärdata för att importera data till ett PDF-formulär och exportera data från ett PDF-formulär med Java API.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations,AEM Forms on JEE
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%

---

# Java API snabbstart (SOAP) för integreringstjänsten för formulärdata {#form-data-integration-service-javaapi-quick-start-soap}

Följande snabbstarter är tillgängliga för integreringstjänsten för formulärdata.

[Snabbstart (SOAP-läge): Importera formulärdata med Java API](form-data-integration-service-java.md#quick-start-soap-mode-importing-form-data-using-the-java-api)

[Snabbstart (SOAP-läge): Exportera formulärdata med Java API](form-data-integration-service-java.md#quick-start-soap-mode-exporting-form-data-using-the-java-api)

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

>[!NOTE]
>
>Snabbstart vid programmering med AEM-formulär baseras på Forms Server som distribueras på JBoss Application Server och Microsoft Windows. Om du använder ett annat operativsystem, till exempel UNIX, ska du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

## Snabbstart (SOAP-läge): Importera formulärdata med Java API {#quick-start-soap-mode-importing-form-data-using-the-java-api}

I följande Java-kodexempel importeras data till ett PDF-formulär. Data finns i en XML-fil med namnet *Loan_data.xml* och PDF-formuläret sparas som en PDF-fil med namnet *ResultLoanForm.pdf*. (Se [Importera formulärdata](/help/forms/developing/importing-exporting-data.md#importing-form-data).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-formdataintegration-client.jar
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
 import com.adobe.livecycle.formdataintegration.client.*;
 
 public class ImportDataSOAP {
 
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
 
             //Create a FormDataIntegrationClient object
             FormDataIntegrationClient dataClient = new FormDataIntegrationClient(myFactory);
 
             //Import XDP XML data into an XFA PDF document
             //Reference an XFA PDF form
             FileInputStream inputStream = new FileInputStream("C:\\Adobe\Loan.pdf");
             Document inputPDF = new Document(inputStream);
 
             FileInputStream dataInput = new FileInputStream("C:\\Adobe\Loan_data.xml");
             Document inputDataFile = new Document(dataInput);
 
             //Import data into the form
             Document resultPDF = dataClient.importData(inputPDF,inputDataFile);
 
             //Save the PDF file
             File resultFile = new File("C:\\Adobe\ResultLoanForm.pdf");
             resultPDF.copyToFile(resultFile);
 
         }catch (Exception e) {
              e.printStackTrace();
         }
       }
     }
 
```

## Snabbstart (SOAP-läge): Exportera formulärdata med Java API {#quick-start-soap-mode-exporting-form-data-using-the-java-api}

I följande Java-kodexempel exporteras data från ett PDF-formulär. Formulärdata sparas som en XML-fil med namnet *Loan_data.xml*. (Se [Exportera formulärdata](/help/forms/developing/importing-exporting-data.md#exporting-form-data).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-formdataintegration-client.jar
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
 import com.adobe.livecycle.formdataintegration.client.*;
 
 public class ExportDataSOAP {
 
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
 
          //Create a FormDataIntegrationClient object
          FormDataIntegrationClient dataClient = new FormDataIntegrationClient(myFactory);
 
          //Reference a PDF form from which to export data
          FileInputStream fileInputStream2 = new FileInputStream("C:\\Adobe\LoanForm.pdf");
          Document inputPDF = new Document(fileInputStream2);
 
          //Export data from the form
          Document resultPDF = dataClient.exportData(inputPDF);
 
          //Save the exported form data as an XML file
          File resultFile = new File("C:\\Adobe\Loan_data.xml");
          resultPDF.copyToFile(resultFile);
 
     }catch (Exception e) {
              e.printStackTrace();
         }
     }
 }
```
