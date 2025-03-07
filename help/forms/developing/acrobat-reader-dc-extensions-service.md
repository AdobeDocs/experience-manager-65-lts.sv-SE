---
title: Acrobat Reader DC-tillägg ServiceJava API Quick Start (SOAP)
description: Använd tjänsten Acrobat Reader DC Extensions för att lägga in användarrättigheter i ett PDF-dokument, ta bort användarrättigheter från PDF-dokument och hämta information om de autentiseringsuppgifter som används för att lägga in användarrättigheter i ett rättighetsaktiverat PDF-dokument med namnet LoanUsageRights.pdf.
contentOwner: admin
content-type: reference
topic-tags: develop
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Reader Extensions,APIs & Integrations
hide: true
hidefromtoc: true
exl-id: a8ec523c-b304-41ba-9980-8ba84e076c7d
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Acrobat Reader DC-tillägg ServiceJava API - snabbstart (SOAP) {#acrobat-reader-dc-extensions-servicejava-api-quick-start-soap}

Följande snabbstarter är tillgängliga för tjänsten Acrobat Reader DC Extensions.

[Snabbstart (SOAP-läge):Tillämpa användningsrättigheter med Java API](#quick-start-soap-mode-applying-usage-rights-using-the-java-api)

[Ta bort användningsrättigheter från PDF-dokument](#quick-start-soap-mode-removing-usage-rights-from-a-pdf-document-using-the-java-api)

[Snabbstart (SOAP-läge): Hämta inloggningsinformation med Java API](acrobat-reader-dc-extensions-service.md#quick-start-soap-mode-retrieving-credential-information-using-the-java-api)

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

>[!NOTE]
>
>Snabbstart i programmering med AEM Forms baseras på Forms serveroperativsystem. Om du använder ett annat operativsystem, till exempel UNIX, ska du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

## Snabbstart (SOAP-läge):Tillämpa användningsrättigheter med Java API {#quick-start-soap-mode-applying-usage-rights-using-the-java-api}

I följande Java-kodexempel tillämpas användarrättigheter på ett PDF-dokument med namnet *Loan.pdf*. Det rättighetsaktiverade PDF-dokumentet sparas som en PDF-fil med namnet *LoanUsageRights.pdf*. Följande användningsrättigheter gäller för det här PDF-dokumentet: `enabledComments`, `enabledFormFillIn` och `enabledDigitalSignatures`. (Se [Använda användningsrättigheter i PDF-dokument](/help/forms/developing/assigning-usage-rights.md).)


```java
 /* 
     * This Java Quick Start uses the SOAP mode and contains the following JAR files 
     * in the class path: 
     * 1. adobe-reader-extensions-client.jar 
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
 import com.adobe.livecycle.readerextensions.client.*; 
 import java.util.*; 
 import java.io.File; 
 import java.io.FileInputStream; 
 import com.adobe.idp.Document; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
  
 public class ApplyUsageRightsSOAP{ 
  
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
          
           //Create a ReaderExtensionsServiceClient object 
           ReaderExtensionsServiceClient reClient = new ReaderExtensionsServiceClient(myFactory);  
          
           //Retrieve the PDF document to which to apply usage rights 
           FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.pdf");  
           Document inputPDF = new Document(fileInputStream); 
                   
           //Create a UsageRight object and specify specific usage rights 
           UsageRights useRight = new UsageRights();  
           useRight.setEnabledDynamicFormFields(true); 
           useRight.setEnabledComments(true); 
           useRight.setEnabledFormFillIn(true); 
           useRight.setEnabledDigitalSignatures(true); 
          
           //Create a ReaderExtensionsOptions object 
           ReaderExtensionsOptionSpec reOptions = new ReaderExtensionsOptionSpec();  
                   
           //Set the usage rights  
           reOptions.setUsageRights(useRight);  
           reOptions.setMessage("This is a Rights-Enabled PDF Document"); 
          
           //Apply usage rights to a PDF document 
           Document rightsEnabledPDF = reClient.applyUsageRights( 
              inputPDF, 
              "RE2", 
             null, 
             reOptions);  
          
           //Create a PDF file that represents the rights-enabled PDF document 
           File resultFile = new File("C:\\Adobe\LoanUsageRights.pdf");  
           rightsEnabledPDF.copyToFile(resultFile); 
                          
         }catch (Exception e) { 
              e.printStackTrace(); 
         }     
     } 
 } 
  
  
```

## Snabbstart (SOAP-läge): Ta bort användningsrättigheter från ett PDF-dokument med Java API {#quick-start-soap-mode-removing-usage-rights-from-a-pdf-document-using-the-java-api}

I följande Java-kodexempel tas användarrättigheter bort från ett rättighetsaktiverat PDF-dokument med namnet *LoanUsageRights.pdf*. (Se [Ta bort användningsrättigheter från PDF-dokument](/help/forms/developing/assigning-usage-rights.md).)

```java
 /* 
     * This Java Quick Start uses the SOAP mode and contains the following JAR files 
     * in the class path: 
     * 1. adobe-reader-extensions-client.jar 
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
 import com.adobe.livecycle.readerextensions.client.*; 
 import java.util.*; 
 import java.io.File; 
 import java.io.FileInputStream; 
 import com.adobe.idp.Document; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
  
 public class RemoveUsageRights{ 
  
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
                  
           //Create a ReaderExtensionsServiceClient object 
           ReaderExtensionsServiceClient reClient = new ReaderExtensionsServiceClient(myFactory);  
          
                //Retrieve a rights-enabled PDF document from 
                //which to remove usage rights 
           FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\LoanUsageRights.pdf");  
           Document inputPDF = new Document(fileInputStream); 
                   
           //Remove usage rights from the PDF document 
           Document rightsEnabledPDF = reClient.removeUsageRights(inputPDF);  
          
           //Save the PDF document as a PDF file 
           File resultFile = new File("C:\\Adobe\noUsageRightsLoan.pdf");  
           rightsEnabledPDF.copyToFile(resultFile); 
           System.out.println("Usage rights were removed from the document");  
                          
         }catch (Exception e) { 
              e.printStackTrace(); 
         }     
     } 
 } 
 
```

## Snabbstart (SOAP-läge): Hämta inloggningsinformation med Java API {#quick-start-soap-mode-retrieving-credential-information-using-the-java-api}

I följande Java-kodexempel hämtas information om de autentiseringsuppgifter som används för att tillämpa användarrättigheter på ett rättighetsaktiverat PDF-dokument med namnet *LoanUsageRights.pdf*. (Se [Hämtar information om autentiseringsuppgifter](/help/forms/developing/assigning-usage-rights.md).)

```java
 /* 
     * This Java Quick Start uses the SOAP mode and contains the following JAR files 
     * in the class path: 
     * 1. adobe-reader-extensions-client.jar 
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
 import com.adobe.livecycle.readerextensions.client.*; 
 import java.util.*; 
 import java.io.FileInputStream; 
 import com.adobe.idp.Document; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
  
 public class RetrieveCredentialInformation { 
  
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
                  
           //Create a ReaderExtensionsServiceClient object 
           ReaderExtensionsServiceClient reClient = new ReaderExtensionsServiceClient(myFactory);  
          
           //Retrieve a rights-enabled PDF document 
           FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\LoanUsageRights.pdf");  
           Document inputPDF = new Document(fileInputStream); 
                   
           //Retrieve credential information 
           GetUsageRightsResult usageRightsResult = reClient.getDocumentUsageRights(inputPDF);  
          
           //Get the date after which the credential is no longer valid 
           Date endDate = usageRightsResult.getNotAfter(); 
          
           //Get the message displayed in Adobe Reader when the rights-enabled 
           //document is opened 
           String message = usageRightsResult.getMessage(); 
          
           //Get usage rights to see if the enableFormFillIn is enabled 
           UsageRights myRights = usageRightsResult.getRights(); 
           boolean ans = myRights.isEnabledFormFillIn(); 
          
           if (ans==true) 
              System.out.println("The enableFormFillIn usage right is enabled"); 
           else 
             System.out.println("The enableFormFillIn usage right is not enabled"); 
                                                   
         }catch (Exception e) { 
              e.printStackTrace(); 
         }     
                  
     } 
 } 
 
```
