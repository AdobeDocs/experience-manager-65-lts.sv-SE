---
title: Skydda ett dokument för en annan användares räkning
description: Läs om hur AEM Forms Document Security Java&trade; SDK erbjuder API:er för ett användarkonto som skyddar ett dokument åt en annan användare.
geptopics: SG_AEMFORMS/categories/working_with_document_security
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 56280931-c88b-46ea-bee6-f5a5494d715a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 0%

---

# Skydda ett dokument för en annan användares räkning {#protect-a-document-on-behalf-of-another-user}

AEM Forms Document Security Java™ SDK har API:er som gör att ett användarkonto kan skydda ett dokument åt en annan användare utan att behöva ha behörighet att redigera dokumentet. Du kan använda API:erna i en arbetsflödesprocess eller programmatiskt som en dokumenttjänst. De nya API:erna är:

* **protectDocumentUse** Skydda dokument-API:t så att du kan tillämpa en profil på ett dokument för

  ett annat användarkonto. Behörigheterna för det användarkonto som används för att tillämpa profilen förblir begränsade till att skydda dokumentet. Det ger inte rätt att öppna och visa dokumentet. RMSecureDocumentResult protectDocument(Document inDoc, String documentName, String policySetName, String policyName, RMLocale locale, boolean bExactMatchForNames)

* **createLicenseUse** API:t CreateLicense så att du kan skapa en licens för en profil för ett annat användarkonto. PublishLicenseDTO createLicense(String policyId, String documentName, booleskt logSecureDocEvent)
* **protectDocumentWithCoverPageUse** API:t ProtectDocumentWithCoverPage så att du kan tillämpa en profil och lägga till en försättssida i ett dokument för en annan användares räkning. Behörigheterna för det användarkonto som används för att tillämpa profilen förblir begränsade till att skydda dokumentet. Det ger inte rätt att öppna och visa dokumentet. RMSecureDocumentResult protectDocumentWithCoverPage(Document inDoc, String documentName, String policySetName, String policyName, Document coverDoc, boolean bExactMatchForNames)

## Skydda ett dokument för en annan användares räkning med API:er {#using-the-apis-to-protect-a-document-on-behalf-of-another-user}

Gör följande så att du kan skydda ett dokument för en annan användares räkning och utan att ha behörighet att redigera dokumentet:

1. Skapa en principuppsättning. PolicySet1.
1. Skapa en profil i den nya principuppsättningen. Policy1 i PolicySet1.
1. Skapa en användare med rollen Rights Management-slutanvändare. Exempel: Användare1. Ge den nyskapade användaren behörighet att visa dokument som skyddas med Policy1.
1. Skapa en roll. Till exempel Role1. Ange behörigheten Tjänstanrop till den nyligen skapade rollen. Skapa en användare med en nyskapad roll. Exempel: Användare2. Du kan använda User2 eller en administratör för att skapa en SDK-anslutning och anropa tjänsten protectDocument.

   Nu kan du köra följande exempelkod för att skydda ett dokument utan att ge behörighet att redigera dokumentet till den användare som skyddar dokumentet:

   ```java
   import java.io.File;
   import java.io.FileInputStream;
   import java.io.FileNotFoundException;
   import java.io.FileOutputStream;
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.Properties;
   
   import com.adobe.edc.common.dto.PublishLicenseDTO;
   import com.adobe.edc.sdk.SDKException;
   import com.adobe.idp.Document;
   import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
   import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
   import com.adobe.livecycle.rightsmanagement.RMSecureDocumentResult;
   import com.adobe.livecycle.rightsmanagement.client.DocumentManager;
   import com.adobe.livecycle.rightsmanagement.client.RightsManagementClient;
   import com.adobe.livecycle.rightsmanagement.client.RightsManagementClient2;
   
   public class PublishAsProtectAPI {
   
   private static final String unprotectedFileName = "C:\\unprotected.pdf";
   private static final String protectedFileName = "C:\\protect.pdf";
   private static final String coverFileName = "C:\\CoverPage.pdf";
   private static final String POLICY_ID = "2EF66008-5E2D-1034-9B06-00000A292C18"; 
   
   public static void main(String[] args) {
   
   try {
   
   Properties connectionProps = new Properties();
   connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT,"http://localhost:8080");
   connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
   connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
   connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME,"administrator");
   connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD,"password");
   
   // Create a ServiceClientFactory instance
   ServiceClientFactory factory = ServiceClientFactory.createInstance(connectionProps);
   testProtectDocument(factory);
   testProtectDocumentWithCoverPage(factory);
   testProtectDocumentJavaPPL(factory);
   
   } 
   catch (Exception ex) {
   ex.printStackTrace(); }
   }
   
   private static void testProtectDocument(ServiceClientFactory factory) throws FileNotFoundException, SDKException {
   // Create a RightsManagementClient object
   RightsManagementClient rmClient = new RightsManagementClient(factory);
   // Create a Document Manager object
   DocumentManager documentManager = rmClient.getDocumentManager();
   //Reference a policy-protected PDF document from which to remove a policy
   FileInputStream is = new FileInputStream(unprotectedFileName);
   Document inPDF = new Document(is);
   long startTime = System.currentTimeMillis();
   //Remove a policy from the policy-protected PDF document
   RMSecureDocumentResult securePDF = documentManager.protectDocument(inPDF, "test", "newPolicySet", "latest", "DefaultDom", "administrator", null, true);
   System.out.println("Total Time taken for protectDocument = " + (System.currentTimeMillis() - startTime));
   //Save the unsecured PDF document
   File myFile = new File(protectedFileName);
   securePDF.getProtectedDoc().copyToFile(myFile);
   }
   
   private static void testProtectDocumentWithCoverPage(ServiceClientFactory factory) throws FileNotFoundException, SDKException {
   // Create a RightsManagementClient object
   RightsManagementClient rmClient = new RightsManagementClient(factory);
   // Create a Document Manager object
   DocumentManager documentManager = rmClient.getDocumentManager();
   //Reference a policy-protected PDF document from which to remove a policy
   FileInputStream is = new FileInputStream(unprotectedFileName);
   Document inPDF = new Document(is);
   FileInputStream coverIS = new FileInputStream(coverFileName);
   Document inCoverPDF = new Document(coverIS);
   long startTime = System.currentTimeMillis();
   //Remove a policy from the policy-protected PDF document
   RMSecureDocumentResult securePDF = documentManager.protectDocumentWithCoverPage(inPDF, "test", "newPolicySet", "latestPolicy", inCoverPDF, true);
   System.out.println("Total Time taken for Page0ProtectDocument = " + (System.currentTimeMillis() - startTime));
   //Save the unsecured PDF document
   File myFile = new File(protectedFileName);
   securePDF.getProtectedDoc().copyToFile(myFile);
   }
   
   private static PublishLicenseDTO testProtectDocumentJavaPPL (ServiceClientFactory factory) throws SDKException, FileNotFoundException, IOException {
   // Create a RightsManagementClient object
   RightsManagementClient2 rmClient2 = new RightsManagementClient2(factory);
   // Create a Document Manager object
   DocumentManager documentManager = rmClient2.getDocumentManager();
   long startTime = System.currentTimeMillis();
   PublishLicenseDTO license = documentManager.createLicense(POLICY_ID, "Out.pdf", true);
   System.out.println("Create License totalTime = " + (System.currentTimeMillis() - startTime));
   startTime = System.currentTimeMillis();
   // Reference a PDF document to which a policy is applied
   InputStream inputFileStream = new FileInputStream(unprotectedFileName);
   // Apply a policy to the PDF document
   InputStream protectPDF = rmClient2.getRightsManagementEncryptionService().protectDocument(inputFileStream, license);
   // Save the policy-protected PDF document
   File myFile = new File(protectedFileName);
   // write the inputStream to a FileOutputStream
   FileOutputStream outputStream = new FileOutputStream(myFile);
   int read = 0;
   byte[] bytes = new byte[1024];
   while ((read = protectPDF.read(bytes)) != -1) {
   outputStream.write(bytes, 0, read);
   }
   System.out.println("protectPDFDocument totalTime = " + (System.currentTimeMillis() - startTime));
   outputStream.close();
   inputFileStream.close();
   System.out.println("Document Protected Successfully");
   return license;
   }
   }
   ```
