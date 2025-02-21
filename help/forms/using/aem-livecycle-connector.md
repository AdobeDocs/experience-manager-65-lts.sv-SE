---
title: Ansluta AEM Forms till Adobe LiveCycle
description: Med Adobe Experience Manager (AEM) LiveCycle Connector kan du starta LiveCycle ES4 Acrobat Services inifrån AEM program och arbetsflöden.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Interactive Communication
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1026'
ht-degree: 0%

---

# Ansluta AEM Forms till Adobe LiveCycle {#connecting-aem-forms-with-adobe-livecycle}

Adobe Experience Manager (AEM) LiveCycle Connector möjliggör smidigt anrop av Adobe LiveCycle ES4 Acrobat Services inifrån AEM webbprogram och arbetsflöden. LiveCycle har en omfattande klient-SDK med vilken klientapplikationer kan starta LiveCycle-tjänster med Java™ API:er. AEM LiveCycle Connector förenklar användningen av dessa API:er i OSGi-miljön.

## Ansluta AEM-server till Adobe LiveCycle {#connecting-aem-server-to-adobe-livecycle}

AEM LiveCycle Connector ingår i [AEM Forms-tilläggspaketet](/help/forms/using/installing-configuring-aem-forms-osgi.md). När du har installerat AEM Forms-tilläggspaketet gör du följande så att du kan lägga till information om LiveCycle-servern i AEM Web Console.

1. Leta reda på konfigurationskomponenten Adobe LiveCycle Client SDK i AEM webbkonsol.
1. Klicka på komponenten så att du kan redigera konfigurationsserverns URL-adress, användarnamn och lösenord.
1. Granska inställningarna och klicka på **Spara**.

Även om egenskaperna är självförklarande är de viktiga följande:

* **Server-URL** - Anger URL till LiveCycle-servern. Om du vill att LiveCycle och AEM ska kommunicera via https, kan du starta AEM med följande JVM

  ```java
  argument
   -Djavax.net.ssl.trustStore=<<em>path to LC keystore</em>>
  ```

  alternativ.

* **Användarnamn** - Anger användarnamnet för kontot som används för att upprätta kommunikation mellan AEM och LiveCycle. Kontot är ett LiveCycle-användarkonto som har behörighet att starta Acrobat Services.
* **Lösenord** - Anger lösenordet.
* **Tjänstnamn** - Anger vilka tjänster som startas med de användarautentiseringsuppgifter som anges i fälten Användarnamn och Lösenord. Som standard skickas inga autentiseringsuppgifter när LiveCycle-tjänster startas.

## Startar dokumenttjänster {#starting-document-services}

Klientapplikationer kan starta LiveCycle-tjänster programmatiskt med Java™ API, Web Services, Remoting och REST. För Java™-klienter kan programmet använda LiveCycle SDK. LiveCycle SDK har ett Java™-API för fjärrstart av dessa tjänster. Om du till exempel vill konvertera ett Microsoft® Word-dokument till PDF startar klienten GeneratePDFService. Anropsflödet består av följande steg:

1. Skapa en ServiceClientFactory-instans.
1. Varje tjänst tillhandahåller en klientklass. Om du vill starta en tjänst skapar du en klientinstans av tjänsten.
1. Starta tjänsten och bearbeta resultatet.

AEM LiveCycle Connector förenklar flödet genom att visa de här klientinstanserna som OSGi-tjänster som kan nås med vanliga OSGi-metoder. LiveCycle-anslutningen har följande funktioner:

* Klientinstanser som OSGi-tjänst: Klienterna som paketeras som OSGI-paket listas i avsnittet [Acrobat Services-lista](/help/forms/using/aem-livecycle-connector.md#p-document-services-list-p). Varje klientjar registrerar klientinstansen som en OSGi-tjänst med OSGi-tjänstregistret.
* Spridning av autentiseringsuppgifter för användare: Den anslutningsinformation som krävs för att ansluta till LiveCycle-servern hanteras på en central plats.
* ServiceClientFactory-tjänst: För att starta processerna kan klientprogrammet få åtkomst till ServiceClientFactory-instansen.

### Starta via tjänstreferenser från OSGi-tjänstregistret {#starting-via-service-references-from-osgi-service-registry}

Så här startar du en exponerad tjänst från AEM:

1. Fastställ maven-beroenden. Lägg till beroendet till den klientjar som behövs i filen maven pom.xml. Lägg till minst beroende i jars för adobe-livecycle-client och adobe-usermanager-client.

   ```xml
   <dependency>
     <groupId>com.adobe.livecycle</groupId>
     <artifactId>adobe-livecycle-client</artifactId>
     <version>11.0.0</version>
   </dependency>
   <dependency>
     <groupId>com.adobe.livecycle</groupId>
     <artifactId>adobe-usermanager-client</artifactId>
     <version>11.0.0</version>
   </dependency>
   <dependency>
     <groupId>com.adobe.livecycle</groupId>
     <artifactId>adobe-cq-integration-api</artifactId>
     <version>11.0.0</version>
   </dependency>
   ```

   Om du vill starta en tjänst lägger du till ett motsvarande Maven-beroende för tjänsten. En lista över beroenden finns i [Lista över Acrobat-tjänster](/help/forms/using/aem-livecycle-connector.md#p-document-services-list-p). För tjänsten Generate PDF lägger du till följande beroende:

   ```xml
   <dependency>
     <groupId>com.adobe.livecycle</groupId>
     <artifactId>adobe-generatepdf-client</artifactId>
     <version>11.0.0</version>
   </dependency>
   ```

1. Hämta tjänstreferensen. Hämta en referens till tjänstinstansen. Om du skriver en Java™-klass kan du använda kommentarerna för deklarativa tjänster.

   ```java
   import com.adobe.livecycle.generatepdf.client.GeneratePdfServiceClient;
   import com.adobe.livecycle.generatepdf.client.CreatePDFResult;
   import com.adobe.idp.Document;
   
   @Reference
   GeneratePdfServiceClient generatePDF;
   ...
   
   Resource r = resourceResolver.getResource("/path/tp/docx");
   Document sourceDoc = new Document(r.adaptTo(InputStream.class));
   CreatePDFResult result = generatePDF.createPDF2(
                       sourceDoc,
                       extension, //inputFileExtension
                       null, //fileTypeSettings
                       null, //pdfSettings
                       null, //securitySettings
                       settingsDoc, //settingsDoc
                       null //xmpDoc
               );
   ```

   Ovanstående kodfragment startar createPDF API för GeneratePdfServiceClient för konvertering av ett dokument till PDF. Du kan utföra ett liknande anrop i en JSP med följande kod. Den största skillnaden är att följande kod använder Sling ScriptHelper för att komma åt GeneratePdfServiceClient.

   ```jsp
   <%@ page import="com.adobe.livecycle.generatepdf.client.GeneratePdfServiceClient" %>
   <%@ page import="com.adobe.livecycle.generatepdf.client.CreatePDFResult" %>
   <%@ page import="com.adobe.idp.Document" %>
   
   GeneratePdfServiceClient generatePDF = sling.getService(GeneratePdfServiceClient.class);
   Document sourceDoc = ...
   CreatePDFResult result = generatePDF.createPDF2(
                       sourceDoc,
                       extension, //inputFileExtension
                       null, //fileTypeSettings
                       null, //pdfSettings
                       null, //securitySettings
                       settingsDoc, //settingsDoc
                       null //xmpDoc
               );
   ```

### Startar via ServiceClientFactory {#starting-via-serviceclientfactory}

Klassen ServiceClientFactory krävs ibland. Du behöver till exempel ServiceClientFactory för att anropa processer.

```java
import com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider;
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;

@Reference
ServiceClientFactoryProvider scfProvider;

...
ServiceClientFactory scf = scfProvider.getDefaultServiceClientFactory();
...
```

## Stöd för RunAs {#runas-support}

Nästan alla Acrobat-tjänster i LiveCycle kräver autentisering. Du kan använda något av följande alternativ för att starta de här tjänsterna utan att ange specifika autentiseringsuppgifter i koden:

### Tillåtelselista-konfiguration {#allowlist-configuration}

LiveCycle Client SDK-konfigurationen innehåller en inställning för tjänstnamn. Den här konfigurationen är en lista över tjänster som anropslogiken använder administratörsbehörighet för. Om du till exempel lägger till DirectoryManager-tjänster (som ingår i API:t för användarhantering) i den här listan kan vilken klientkod som helst använda tjänsten direkt. Dessutom skickar startlagret automatiskt de konfigurerade inloggningsuppgifterna som en del av den begäran som skickas till LiveCycle-servern.

### RunAsManager {#runasmanager}

Som en del av integreringen tillhandahålls en ny tjänst, RunAsManager. Med den kan du programmässigt styra vilka autentiseringsuppgifter som ska användas vid anrop till LiveCycle-servern.

```java
import com.adobe.livecycle.dsc.clientsdk.security.PasswordCredential;
import com.adobe.livecycle.dsc.clientsdk.security.PrivilegedAction;
import com.adobe.livecycle.dsc.clientsdk.security.RunAsManager;
import com.adobe.idp.dsc.registry.component.ComponentRegistry;

@Reference
private RunAsManager runAsManager;

List<Component> components = runAsManager.doPrivileged(new PrivilegedAction<List<Component>>() {
            public List<Component> run() {
                return componentRegistry.getComponents();
            }
        });
assertNotNull(components);
```

Om du vill skicka en annan referens kan du använda den överlagrade metoden som tar en PasswordCredential-instans.

```java
PasswordCredential credential = new PasswordCredential("administrator","password");
List<Component> components = runAsManager.doPrivileged(new PrivilegedAction<List<Component>>() {
    public List<Component> run() {
        return componentRegistry.getComponents();
    }
},credential);
```

### InvocationRequest, egenskap {#invocationrequest-property}

Om du anropar en process eller använder klassen ServiceClientFactory direkt och skapar en InvocationRequest, kan du ange en egenskap som anger att anropslagret ska använda konfigurerade autentiseringsuppgifter.

```java
import com.adobe.idp.dsc.InvocationResponse
import com.adobe.idp.dsc.InvocationRequest
import com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory
import com.adobe.livecycle.dsc.clientsdk.InvocationProperties

ServiceClientFactoryProvider scfp = sling.getService(ServiceClientFactoryProvider.class)
ServiceClientFactory serviceClientFactory = scfp.getDefaultServiceClientFactory()
InvocationRequest ir = serviceClientFactory.createInvocationRequest("sample/LetterSubmissionProcess", "invoke", new HashMap(), true);

//Here we are invoking the request with system user
ir.setProperty(InvocationProperties.INVOKER_TYPE,InvocationProperties.INVOKER_TYPE_SYSTEM)

InvocationResponse response = serviceClientFactory.getServiceClient().invoke(ir);
```

## Acrobat Services-lista {#document-services-list}

### Adobe LiveCycle Client SDK API bundle {#adobe-livecycle-client-sdk-api-bundle}

Följande tjänster är tillgängliga:

* com.adobe.idp.um.api.AuthenticationManager
* com.adobe.idp.um.api.DirectoryManager
* com.adobe.idp.um.api.AuthorizationManager
* com.adobe.idp.dsc.registry.service.ServiceRegistry
* com.adobe.idp.dsc.registry.component.ComponentRegistry

#### Maven-beroenden {#maven-dependencies}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-livecycle-client</artifactId>
  <version>11.0.0</version>
</dependency>
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-usermanager-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Client SDK Bundle {#adobe-livecycle-client-sdk-bundle}

Följande tjänster är tillgängliga:

* com.adobe.livecycle.dsc.clientsdk.security.RunAsManager
* com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider

#### Maven-beroenden {#maven-dependencies-1}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-livecycle-cq-integration-api</artifactId>
  <version>1.1.10</version>
</dependency>
```

### Adobe LiveCycle TaskManager Client bundle {#adobe-livecycle-taskmanager-client-bundle}

Följande tjänster är tillgängliga:

* com.adobe.idp.taskmanager.dsc.client.task.TaskManager
* com.adobe.idp.taskmanager.dsc.client.TaskManagerQueryService
* com.adobe.idp.taskmanager.dsc.client.queuemanager.QueueManager
* com.adobe.idp.taskmanager.dsc.client.emailsettings.EmailSettingService
* com.adobe.idp.taskmanager.dsc.client.endpoint.TaskManagerEndpointClient
* com.adobe.idp.taskmanager.dsc.client.userlist.UserlistService

#### Maven-beroenden {#maven-dependencies-2}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-taskmanager-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Workflow Client Bundle {#adobe-livecycle-workflow-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.idp.workflow.client.WorkflowServiceClient

#### Maven-beroenden {#maven-dependencies-3}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-workflow-client-sdk</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle PDF Generator Client bundle {#adobe-livecycle-pdf-generator-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.generatepdf.client.GeneratePdfServiceClient

#### Maven-beroenden {#maven-dependencies-4}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-generatepdf-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Application Manager Client bundle {#adobe-livecycle-application-manager-client-bundle}

Följande tjänster är tillgängliga:

* com.adobe.idp.applicationmanager.service.ApplicationManager
* com.adobe.livecycle.applicationmanager.client.ApplicationManager
* com.adobe.livecycle.design.service.DesigntimeService

#### Maven-beroenden {#maven-dependencies-5}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-applicationmanager-client-sdk</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Assembler Client bundle {#adobe-livecycle-assembler-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.assembler.client.AssemblerServiceClient

#### Maven-beroenden {#maven-dependencies-6}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-assembler-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Form Data Integration Client bundle {#adobe-livecycle-form-data-integration-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.formdataintegration.client.FormDataIntegrationClient

#### Maven-beroenden {#maven-dependencies-7}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-formdataintegration-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Forms Client bundle {#adobe-livecycle-forms-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.formsservice.client.FormsServiceClient

#### Maven-beroenden {#maven-dependencies-8}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-forms-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Output Client bundle {#adobe-livecycle-output-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.output.client.OutputClient

#### Maven-beroenden {#maven-dependencies-9}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-output-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Reader Extensions Client bundle {#adobe-livecycle-reader-extensions-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.readerextensions.client.ReaderExtensionsServiceClient

#### Maven-beroenden {#maven-dependencies-10}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-reader-extensions-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Rights Manager Client bundle {#adobe-livecycle-rights-manager-client-bundle}

Följande tjänster är tillgängliga:

* com.adobe.livecycle.rightsmanagement.client.DocumentManager
* com.adobe.livecycle.rightsmanagement.client.EventManager
* com.adobe.livecycle.rightsmanagement.client.ExternalUserManager
* com.adobe.livecycle.rightsmanagement.client.LicenseManager
* com.adobe.livecycle.rightsmanagement.client.WatermarkManager
* com.adobe.livecycle.rightsmanagement.client.PolicyManager
* com.adobe.livecycle.rightsmanagement.client.AbstractPolicyManager

#### Maven-beroenden {#maven-dependencies-11}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-rightsmanagement-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Signatures Client bundle {#adobe-livecycle-signatures-client-bundle}

Följande tjänst är tillgänglig:

* com.adobe.livecycle.signatures.client.SignatureServiceClientInterface

#### Maven-beroenden {#maven-dependencies-12}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-signatures-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Truststore Client bundle {#adobe-livecycle-truststore-client-bundle}

Följande tjänster är tillgängliga:

* com.adobe.truststore.dsc.TrustConfigurationService
* com.adobe.truststore.dsc.CRLService
* com.adobe.truststore.dsc.CredentialService
* com.adobe.truststore.dsc.CertificateService

#### Maven-beroenden {#maven-dependencies-13}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-truststore-client</artifactId>
  <version>11.0.0</version>
</dependency>
```

### Adobe LiveCycle Repository Client bundle {#adobe-livecycle-repository-client-bundle}

Följande tjänster är tillgängliga:

* com.adobe.repository.bindings.ResourceRepository
* com.adobe.repository.bindings.ResourceSynchronizer

#### Maven-beroenden {#maven-dependencies-14}

```xml
<dependency>
  <groupId>com.adobe.livecycle</groupId>
  <artifactId>adobe-repository-client</artifactId>
  <version>11.0.0</version>
</dependency>
```
