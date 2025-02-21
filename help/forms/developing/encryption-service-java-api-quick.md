---
title: Krypteringstjänstens Java&handel; API QuickStart (SOAP)
description: Lär dig hur du krypterar, tar bort lösenord-/certifikatbaserad kryptering, låser upp och fastställer krypteringstyp för PDF-dokument med Java&trade; API i SOAP-läge.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations,AEM Forms on JEE
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Snabbstart för Java™ API för krypteringstjänsten (SOAP) {#encryption-service-java-api-quickstart-soap}

[Snabbstart (SOAP-läge): Kryptera ett PDF-dokument med Java](encryption-service-java-api-quick.md#quick-start-soap-mode-encrypting-a-pdf-document-using-the-java-api)

[Snabbstart (SOAP-läge): Ta bort lösenordsbaserad kryptering med Java](encryption-service-java-api-quick.md#quick-start-soap-mode-removing-password-based-encryption-using-the-java-api)

[Snabbstart (SOAP-läge): Kryptera ett PDF-dokument med ett certifikat med hjälp av Java](encryption-service-java-api-quick.md#quick-start-soap-mode-encrypting-a-pdf-document-with-a-certificate-using-the-java-api)

[Snabbstart (SOAP-läge): Tar bort certifikatbaserad kryptering med Java](encryption-service-java-api-quick.md#quick-start-soap-mode-removing-certificate-based-encryption-using-the-java-api)

[Snabbstart (SOAP-läge): Låsa upp ett krypterat PDF-dokument med Java](encryption-service-java-api-quick.md#quick-start-soap-mode-unlocking-an-encrypted-pdf-document-using-the-java-api)

[Snabbstart (SOAP-läge): Bestämma krypteringstyp med Java](encryption-service-java-api-quick.md#quick-start-soap-mode-determining-encryption-type-using-the-java-api)

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

>[!NOTE]
>
>Snabbstart i programmering med AEM-formulär baseras på Forms Server som distribueras på JBoss® Application Server och operativsystemet Microsoft® Windows. Om du använder ett annat operativsystem, till exempel UNIX®, ska du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

## Snabbstart (SOAP-läge): Kryptera ett PDF-dokument med Java™ API {#quick-start-soap-mode-encrypting-a-pdf-document-using-the-java-api}

I följande Java™-kodexempel krypteras ett PDF-dokument med namnet *Loan.pdf* med lösenordsvärdet `OpenPassword`. Det primära lösenordet är `PermissionPassword`. Det skyddade PDF-dokumentet sparas som en PDF-fil med namnet *EncryptLoan.pdf*. (Se [Kryptera PDF-dokument med ett lösenord](/help/forms/developing/encrypting-decrypting-pdf-documents.md#encrypting-pdf-documents-with-a-password).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-encryption-client.jar
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
 import java.io.File;
 import java.io.FileInputStream;
 import java.util.ArrayList;
 import java.util.List;
 import java.util.Properties;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.encryption.client.*;
 
 public class PasswordEncryptPDFSoap{
 
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
 
         //Create an EncryptionServiceClient object
         EncryptionServiceClient encryptClient = new EncryptionServiceClient(myFactory);
 
         //Specify the PDF document to encrypt with a password
         FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.pdf");
         Document inDoc = new Document (fileInputStream);
 
         //Create a PasswordEncryptionOptionSpec object that stores encryption run-time values
         PasswordEncryptionOptionSpec passSpec = new PasswordEncryptionOptionSpec();
 
         //Specify the PDF document resource to encrypt
         passSpec.setEncryptOption(PasswordEncryptionOption.ALL);
 
         //Specify the permission associated with the password
         //These permissions enable data to be extracted from a password
         //protected PDF form
         List<PasswordEncryptionPermission> encrypPermissions = new ArrayList<PasswordEncryptionPermission>();
         encrypPermissions.add(PasswordEncryptionPermission.PASSWORD_EDIT_ADD);
         encrypPermissions.add(PasswordEncryptionPermission.PASSWORD_EDIT_MODIFY);
         passSpec.setPermissionsRequested(encrypPermissions);
 
         //Specify the Acrobat version
         passSpec.setCompatability(PasswordEncryptionCompatability.ACRO_7);
 
         //Specify the password values
         passSpec.setDocumentOpenPassword("OpenPassword");
         passSpec.setPermissionPassword("PermissionPassword");
 
         //Encrypt the PDF document
         Document encryptDoc = encryptClient.encryptPDFUsingPassword(inDoc,passSpec);
 
         //Save the password-encrypted PDF document
         File outFile = new File("C:\\Adobe\EncryptLoan.pdf");
         encryptDoc.copyToFile (outFile);
 
         }catch (Exception e) {
             e.printStackTrace();
         }
     }
 }
```

## Snabbstart (SOAP-läge): Lösenordsbaserad kryptering tas bort med Java™ API {#quick-start-soap-mode-removing-password-based-encryption-using-the-java-api}

Följande Java™-kodexempel tar bort lösenordsbaserad kryptering från ett PDF-dokument med namnet *EncryptLoan.pdf*. Det primära lösenordsvärdet som används för att ta bort lösenordsbaserad kryptering är *PermissionPassword*. Det oskyddade PDF-dokumentet sparas som en PDF-fil med namnet *noEncryptionLoan.pdf*. (Se [Ta bort lösenordskryptering](/help/forms/developing/encrypting-decrypting-pdf-documents.md#removing-password-encryption).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-encryption-client.jar
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
 import java.io.File;
 import java.io.FileInputStream;
 import java.util.Properties;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.encryption.client.*;
 
 public class RemovePasswordFromPDFSOAP {
 
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
 
             //Create an EncryptionServiceClient object
             EncryptionServiceClient encryptClient = new EncryptionServiceClient(myFactory);
 
             //Get the encrypted PDF from which to remove password-based encryption
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\EncryptLoan.pdf");
             Document inDoc = new Document (fileInputStream);
 
             //Remove password-based encryption from the PDF document
             Document encryptDoc = encryptClient.removePDFPasswordSecurity(inDoc,"PermissionPassword");
 
             //Save the unsecured PDF document
             File outFile = new File("C:\\Adobe\noEncryptionLoan.pdf");
             encryptDoc.copyToFile (outFile);
 
         }catch (Exception e) {
             e.printStackTrace();
         }
     }
 }
```

## Snabbstart (SOAP-läge): Kryptera ett PDF-dokument med ett certifikat med Java™ API {#quick-start-soap-mode-encrypting-a-pdf-document-with-a-certificate-using-the-java-api}

I följande Java™-kodexempel krypteras ett PDF-dokument med namnet *Loan.pdf* med ett certifikat med namnet *Encryption.cer*. Det krypterade PDF-dokumentet sparas som en PDF-fil med namnet *EncryptLoanCert.pdf*. (Se [Kryptera PDF-dokument med certifikat](/help/forms/developing/encrypting-decrypting-pdf-documents.md#encrypting-pdf-documents-with-certificates).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-encryption-client.jar
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
 import java.io.File;
 import java.io.FileInputStream;
 import java.util.ArrayList;
 import java.util.List;
 import java.util.Properties;
 
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.encryption.client.*;
 
 public class PKIEncryptPDFSoap {
 
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
 
             //Create an EncryptionServiceClient object
             EncryptionServiceClient encryptClient = new EncryptionServiceClient(myFactory);
 
             //Specify the PDF document to encrypt with a certificate
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\Loan.pdf");
             Document inDoc = new Document (fileInputStream);
 
             //Set the List that stores PKI information
             List pkiIdentities = new ArrayList();
 
             //Set the Permission List
             List permList = new ArrayList();
             permList.add(CertificateEncryptionPermissions.PKI_ALL_PERM) ;
 
             //Create a Recipient object to store certificate information
             Recipient recipient = new Recipient();
 
             //Specify the private key that is used to encrypt the document
             FileInputStream fileInputStreamCert = new FileInputStream("C:\\Adobe\Encryption.cer");
             Document privateKey = new Document (fileInputStreamCert);
             recipient.setX509Cert(privateKey);
 
             //Create an EncryptionIdentity object
             CertificateEncryptionIdentity encryptionId = new CertificateEncryptionIdentity();
             encryptionId.setPerms(permList);
             encryptionId.setRecipient(recipient);
 
             //Add the EncryptionIdentity to the list
             pkiIdentities.add(encryptionId);
 
             //Set encryption run-time options
             CertificateEncryptionOptionSpec certOptionsSpec = new CertificateEncryptionOptionSpec();
             certOptionsSpec.setOption(CertificateEncryptionOption.ALL);
             certOptionsSpec.setCompat(CertificateEncryptionCompatibility.ACRO_7);
 
             //Encrypt the PDF document with a certificate
             Document encryptDoc = encryptClient.encryptPDFUsingCertificates(inDoc,pkiIdentities, certOptionsSpec);
 
             //Save the encrypted PDF document
             File outFile = new File("C:\\Adobe\EncryptLoanCert.pdf");
             encryptDoc.copyToFile (outFile);
 
             }catch (Exception e) {
                 e.printStackTrace();
             }
     }
 }
 
```

## Snabbstart (SOAP-läge): Tar bort certifikatbaserad kryptering med Java™ API {#quick-start-soap-mode-removing-certificate-based-encryption-using-the-java-api}

Följande Java™-kodexempel tar bort certifikatbaserad kryptering från ett PDF-dokument med namnet *EncryptLoanCert.pdf*. Aliaset för den offentliga nyckeln som används för att ta bort kryptering är `Encryption`. Det oskyddade PDF-dokumentet sparas som en PDF-fil med namnet *noEncryptionLoan.pdf*. (Se [Tar bort certifikatbaserad kryptering](/help/forms/developing/encrypting-decrypting-pdf-documents.md#removing-certificate-based-encryption).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-encryption-client.jar
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
     * <install directory>/jboss/bin/client/jboss/bin/client
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
 import java.io.File;
 import java.io.FileInputStream;
 import java.util.Properties;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.encryption.client.*;
 
 public class RemovePKIFromPDFSOAP {
 
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
 
             //Create an EncryptionServiceClient object
             EncryptionServiceClient encryptClient = new EncryptionServiceClient(myFactory);
 
             //Get the encrypted PDF document
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\EncryptLoanCert.pdf");
             Document inDoc = new Document (fileInputStream);
 
             //Remove certificate-based encryption from the PDF document
             Document encryptDoc = encryptClient.removePDFCertificateSecurity(inDoc, "Encryption");
 
             //Save the unsecured PDF document
             File outFile = new File("C:\\Adobe\noEncryptionLoan.pdf");
             encryptDoc.copyToFile (outFile);
 
         }catch (Exception e) {
                 e.printStackTrace();
             }
     }
 }
```

## Snabbstart (SOAP-läge): Låsa upp ett krypterat PDF-dokument med Java™ API {#quick-start-soap-mode-unlocking-an-encrypted-pdf-document-using-the-java-api}

Följande Java™-kodexempel låser upp ett lösenordskrypterat PDF-dokument med namnet *EncryptLoan.pdf*. (Se [Låsa upp krypterade PDF-dokument](/help/forms/developing/encrypting-decrypting-pdf-documents.md#unlocking-encrypted-pdf-documents).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-encryption-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     * 4. adobe-utilities.jar
     * 5. jboss-client.jar (use a different JAR file if Forms Server is not deployed
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
 import java.io.FileInputStream;
 import java.util.Properties;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.encryption.client.*;
 
 public class UnlockPDFSOAP {
 
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
 
             //Create an EncryptionServiceClient object
             EncryptionServiceClient encryptClient = new EncryptionServiceClient(myFactory);
 
             //Get the password-encrypted PDF document to unlock
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\EncryptLoan.pdf");
             Document inDoc = new Document (fileInputStream);
 
             //Specify the password to open the password-encrypted PDF document
             String openPassword = "OpenPassword" ;
 
             //Unlock the password-encrypted PDF document
             Document unlockedDoc = encryptClient.unlockPDFUsingPassword(inDoc,openPassword);
 
         }catch (Exception e) {
                 System.out.println("The following error occurred during this operation " +e.getMessage());
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Bestämma krypteringstyp med Java™ API {#quick-start-soap-mode-determining-encryption-type-using-the-java-api}

Följande Java™-kodexempel avgör vilken typ av kryptering som skyddar ett PDF-dokument med namnet *EncryptLoan.pdf*. (Se [Bestämmer krypteringstyp](/help/forms/developing/encrypting-decrypting-pdf-documents.md#determining-encryption-type).)

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-encryption-client.jar
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
 import java.io.FileInputStream;
 import java.util.Properties;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.encryption.client.*;
 
 public class GetEncryptionTypeSOAP {
 
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
 
             //Create a EncryptionServiceClient object
             EncryptionServiceClient encryptClient = new EncryptionServiceClient(myFactory);
 
             //Get the PDF document
             FileInputStream fileInputStream = new FileInputStream("C:\\Adobe\EncryptLoan.pdf");
             Document inDoc = new Document (fileInputStream);
 
             //Determine the type of encryption of the PDF document
             EncryptionTypeResult encryptTypeResult = encryptClient.getPDFEncryption(inDoc);
 
             if (encryptTypeResult.getEncryptionType() == EncryptionType.PASSWORD)
                 System.out.println("The PDF document is protected with password-based encryption");
             else if (encryptTypeResult.getEncryptionType() == EncryptionType.POLICY_SERVER)
                 System.out.println("The PDF document is protected with policy");
             else if (encryptTypeResult.getEncryptionType() == EncryptionType.CERTIFICATE)
                 System.out.println("The PDF document is protected with certificate-based encryption");
             else if (encryptTypeResult.getEncryptionType() == EncryptionType.OTHER)
                 System.out.println("The PDF document is protected with another type of encryption");
             else if (encryptTypeResult.getEncryptionType() == EncryptionType.NONE)
                 System.out.println("The PDF document is not protected.");
         }catch (Exception e) {
                 e.printStackTrace();
         }
     }
 }
 
 
 
```
