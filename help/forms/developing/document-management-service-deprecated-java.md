---
title: Document Management Service (utgått) Java API Quick Start (SOAP)
description: Document Management Service (utgått) Java API Quick Start (SOAP)
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations,AEM Forms on JEE
exl-id: 3d67ebee-de15-4164-a474-670edd682563
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '639'
ht-degree: 0%

---

# Document Management Service (utgått) Java API Quick Start (SOAP) {#document-management-service-deprecated-java-api-quick-start-soap}

Följande snabbstarter är tillgängliga för dokumenthanteringstjänsten (borttagen).

>[!NOTE]
>
>Från och med den 5 augusti 2011 migrerar Adobe Content Services ES-kunder till Adobe Digital Enterprise Platform Experience Services. Produktens färdplan för kunder som använder Content Services är att gå över till den nya ADEP Experience Services - Core, som innehåller en inbyggd Content Repository som bygger på den moderna, modulära CRX-arkitekturen, som förvärvats i samband med Adobe förvärv av Day Software.

[Snabbstart (SOAP-läge): Skapa innehållstjänstutrymmen med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-create-content-services-spaces-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Ta bort innehåll i Content Services med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-delete-content-services-content-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Lägg till innehåll i Content Services med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-add-content-to-content-services-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Hämta innehåll från Content Services med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-retrieve-content-from-content-services-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Flytta innehåll för innehållstjänster med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-move-content-services-content-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Lista innehåll i Content Services med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-list-content-services-content-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Sök efter innehåll i Content Services med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-search-content-services-content-using-the-java-api-deprecated)

[Snabbstart (SOAP-läge): Ange behörigheter för innehållstjänster med Java API](document-management-service-deprecated-java.md#quick-start-soap-mode-setting-content-services-permissions-using-the-java-api-deprecated)

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

>[!NOTE]
>
>Snabbstart i programmering med AEM-formulär baseras på Forms Server som distribueras på JBoss och operativsystemet Windows. Om du använder ett annat operativsystem, t.ex. UNIX, kan du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

## Snabbstart (SOAP-läge): Skapa innehållstjänstutrymmen med Java API (inaktuellt) {#quick-start-soap-mode-create-content-services-spaces-using-the-java-api-deprecated}

I följande Java-kodexempel skapas ett nytt space med namnet *Test Directory* i företagshem. ID-värdet för det nya utrymmet skrivs till konsolen.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 
 public class CreateNewSpaceSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the name of the store and node
             String storeName ="SpacesStore";
             String nodeName = "/Company Home/Test Directory" ;
 
             //Create a space
             String spaceId = docManager.createSpace(storeName,nodeName);
             System.out.println("The identifier value of the new space is " +spaceId);
         }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Ta bort innehåll i innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-delete-content-services-content-using-the-java-api-deprecated}

I följande Java-kodexempel tas ett blanksteg med namnet /Company Home/Test Directory bort.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 
 public class DeleteContentSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the name of the store and node
             String storeName ="SpacesStore";
             String nodeName = "/Company Home/Test Directory" ;
 
             //Delete the content from /Company Home/Test Directory
             Boolean ans = docManager.deleteContent(storeName, nodeName);
 
             if (ans == true)
                 System.out.println("The content was successfully deleted");
             else
                 System.out.println("The content was not deleted");
             }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Lägg till innehåll i innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-add-content-to-content-services-using-the-java-api-deprecated}

I följande Java-kodexempel läggs en PDF-fil med namnet *MortgageForm.pdf* till i en mapp med namnet /Company Home/Test Directory. Attributen creator och description har angetts. Identifieringsvärdet för det nya innehållet skrivs till konsolen.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 import java.util.*;
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.CRCResult;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 import com.adobe.livecycle.contentservices.client.impl.UpdateVersionType;
 
 public class AddContentSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the store and node name
             String storeName ="SpacesStore";
             String nodeName = "/Company Home/Test Directory" ;
 
             //Retrieve the document to store in /Company Home/Test Directory
             Document content =  new Document(new File("C:\\Adobe\MortgageForm.pdf"), false);
 
             //Create a MAP instance to store attributes
             Map<String,Object> inputs = new HashMap<String,Object>();
 
             //Specify attributes that belong to the new content
             String creator = "{https://www.alfresco.org/model/content/1.0}creator";
             String description = "{https://www.alfresco.org/model/content/1.0}description";
 
             inputs.put(creator,"Tony Blue");
             inputs.put(description,"A mortgage application form");
 
             //Store MortgageForm.pdf in /Company Home/Test Directory
             CRCResult result = docManager.storeContent(storeName,
                      nodeName,
                     "MortgageForm.pdf",
                     "{https://www.alfresco.org/model/content/1.0}content",
                     content,
                     "UTF-8",
                     UpdateVersionType.INCREMENT_MAJOR_VERSION,
                     null,
                     inputs);
 
             //Get the identifier value of the new content
             String id = result.getNodeUuid();
             System.out.println("The identifier value of the new content is "+id);
     }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Hämta innehåll från innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-retrieve-content-from-content-services-using-the-java-api-deprecated}

I följande Java-kodexempel hämtas en PDF-fil med namnet *MortgageForm.pdf* från /Company Home. PDF-filen sparas i det lokala filsystemet och har namnet *UpdatedMortgageForm.pdf*.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 import java.util.*;
 
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.CRCResult;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 
 public class RetrieveContentSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the name of the store and the content to retrieve
                String storeName = "SpacesStore";
                String nodeName  = "/Company Home/MortgageForm.pdf";
 
                //Retrieve /Company Home/MortgageForm.pdf
                CRCResult content = docManager.retrieveContent(
                      storeName,
                      nodeName,
                      "");
 
                //Write the PDF file to the local file system
                File myFile = new File("C:\\Adobe\UpdatedMortgageForm.pdf");
                Document doc =content.getDocument();
                doc.copyToFile(myFile);
          }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
 
```

## Snabbstart (SOAP-läge): Flytta innehåll för innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-move-content-services-content-using-the-java-api-deprecated}

I följande Java-kodexempel flyttas en PDF-fil med namnet *MortgageForm.pdf* från /Company Home/Test Directory till /Company Home. Identifieringsvärdet för det flyttade innehållet skrivs till konsolen.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 
 public class MoveContentSoap {
 
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
 
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the name of the store and the content to move
                String storeName = "SpacesStore";
                String nodeName = "/Company Home/Test Directory/MortgageForm.pdf";
                String newSpace = "/Company Home";
 
                //Move the content from /Company Home/Test Directory
                //to /Company Home and display the identifier value of the
                //moved content
                String contentID = docManager.moveContent(storeName, nodeName, newSpace);
                System.out.println("The identifier value of the moved content is "+contentID);
         }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
 
```

## Snabbstart (SOAP-läge): Lista innehåll i innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-list-content-services-content-using-the-java-api-deprecated}

I följande Java-kodexempel visas innehåll som finns i /Company Home. Varje nodtyp och nodnamn visas.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.CRCResult;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 
 public class ListingContentSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the name of the store and the space
                String storeName = "SpacesStore";
                String nodeName  = "/Company Home";
 
               //List the contents of /Company Home
               List<CRCResult> allImages = docManager.getSpaceContents(
                      storeName,
                      nodeName,
                      false);
 
              //Create an Iterator object and iterate through
              //the List object
              Iterator iter = allImages.iterator();
              int i = 0 ;
              while (iter.hasNext()) {
 
                  //Get the node content type and name
                  CRCResult sinContent = (CRCResult)iter.next();
                  String nodeType =  sinContent.getNodeType();
                  String name = sinContent.getNodeName();
                  System.out.println("The node type is "+nodeType  +". The node name is "+name);
              }
         }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
 
```

## Snabbstart (SOAP-läge): Sök efter innehåll i innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-search-content-services-content-using-the-java-api-deprecated}

Följande Java-kod söker efter ett dokument som innehåller texten MortgageForm /Company Home. Undermapparna söks också igenom.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.ResultSet;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 import com.adobe.livecycle.contentservices.client.impl.QueryImpl;
 import com.adobe.livecycle.contentservices.client.impl.StatementImpl;
 
 public class SearchSpaceSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the name of the store and node
             String path ="/Company Home";
              String storeName = "SpacesStore";
 
             //Create a Query expression
             QueryImpl qImpl = new QueryImpl();
             String myName = "{https://www.alfresco.org/model/content/1.0}name";
             StatementImpl statement = new StatementImpl(myName, StatementImpl.OPERATOR_CONTAINS, "MortgageForm" );
             qImpl.addStatement(statement);
 
             //Perform the search for a document that contains the text MortgageForm
             ResultSet rs = docManager.searchRepository(storeName, path, true, qImpl, 200);
             long resultSize = rs.getResultSize();
 
             //Determine if the document is in Content space
             if (resultSize > 0)
             {
                 System.out.println("MortgageForm is in the Repository");
             }
         }
     catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
 
```

## Snabbstart (SOAP-läge): Ange behörigheter för innehållstjänster med Java API (inaktuellt) {#quick-start-soap-mode-setting-content-services-permissions-using-the-java-api-deprecated}

I följande Java-kodexempel anges en behörighet för en användare med namnet tony blue. Den angivna domänen är standarddomän. Konsumentbehörigheten har angetts och noden är `/Company Home/Test Directory`.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 import com.adobe.livecycle.contentservices.client.impl.ContentAccessPermission;
 
 public class SetPermissionsSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the store and node name
             String storeName ="SpacesStore";
             String nodeName = "/Company Home/Test Directory/";
 
              //Create a permission
             ContentAccessPermission permission = new ContentAccessPermission();
             permission.setAuthority("tblue/DefaultDom");
             permission.setIsAllowed(false);
             permission.setPermission("Consumer");
 
             //Create a collection to hold the values
             List<ContentAccessPermission> permissionList = new ArrayList<ContentAccessPermission>();
             permissionList.add(0,permission);
 
             //Set the permission
             docManager.writePermissions(storeName,
                      nodeName,
                      permissionList,
                     false);
     }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 }
 
```

## Snabbstart (SOAP-läge): Skapa associationer med Java API (inaktuellt) {#quick-start-soap-mode-creating-associations-using-the-java-api-deprecated}

Följande Java-kod skapar en association för en XML-datafil och ett PDF-formulär. Den här associationstypen heter LinkedBy. I PDF-dokumentet måste bildlänkningsfunktionen användas.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-contentservices-client.jar
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
 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import com.adobe.livecycle.contentservices.client.impl.DocumentManagementServiceClientImpl;
 
 public class CreateAssociationsSoap {
 
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
 
             //Create a DocumentManagementServiceClientImpl object
             DocumentManagementServiceClientImpl    docManager = new DocumentManagementServiceClientImpl(myFactory);
 
             //Specify the input values
             String storeName ="SpacesStore";
             String associationType = "{https://www.adobe.com/lc/datacapture/1.0}linkedBy";
             String aspect = "{https://www.adobe.com/lc/datacapture/1.0}linkable";
             String parentPath= "/Company Home/MortgageForm.pdf";
             String childPath= "/Company Home/Loan.xml";
 
             //Set the linkable aspect to MortgageForm.pdf
             List<String> aspectList = new ArrayList();
             aspectList.add(aspect);
 
             //Create an attribute map
             Map<String,Object> inputs = new HashMap<String,Object>();
 
             //Specify attributes that belong to the new content
             String creator = "{https://www.alfresco.org/model/content/1.0}creator";
             String description = "{https://www.alfresco.org/model/content/1.0}description";
 
             inputs.put(creator,"Tony Blue");
             inputs.put(description,"Link the PDF document to loan data");
 
             //Set the aspects
             docManager.setContentAttributes(storeName,parentPath,aspectList,inputs);
 
             //Create an association between MortgageForm.pdf and Loan.xml
             docManager.createAssociation(storeName,
                     associationType,
                     parentPath,
                     childPath);
         }
 
         catch(Exception e)
         {
             e.printStackTrace();
         }
     }
 
```
