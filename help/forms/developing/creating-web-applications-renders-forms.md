---
title: Skapa webbprogram som återger Forms
description: Skapa ett webbaserat program som använder Java-servrar för att anropa Forms-tjänsten och återge formulär. Java-servern fungerar som länk mellan Forms-tjänsten som returnerar ett formulär och en webbläsare på klienten.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/rendering_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Workbench,APIs & Integrations
exl-id: 071781e8-990d-4d01-b46e-be1c57bdbe3a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Skapa webbprogram som återger Forms {#creating-web-applications-thatrenders-forms}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

## Skapa webbprogram som återger Forms {#creating-web-applications-that-renders-forms}

Du kan skapa ett webbaserat program som använder Java-servrar för att anropa Forms-tjänsten och återge formulär. En fördel med att använda en Java™-server är att du kan skriva processens returvärde till en webbläsare på klienten. Det innebär att en Java-server kan användas som länk mellan den Forms-tjänst som returnerar ett formulär och en webbläsare på klienten.

>[!NOTE]
>
>I det här avsnittet beskrivs hur du skapar ett webbaserat program som använder en Java-server som anropar Forms-tjänsten och återger formulär som är baserade på fragment. (Se [Återge Forms baserat på fragment](/help/forms/developing/rendering-forms-based-fragments.md).)

Med hjälp av en Java-server kan du skriva ett formulär till en webbläsare så att kunden kan visa och ange data i formuläret. När formuläret har fyllts i med data klickar webbanvändaren på en skicka-knapp som finns i formuläret för att skicka tillbaka information till Java-servern, där informationen kan hämtas och bearbetas. Data kan till exempel skickas till en annan process.

I det här avsnittet beskrivs hur du skapar ett webbaserat program där användaren kan välja antingen amerikansk eller kanadensisk formulärdata, vilket visas i bilden nedan.

![cw_cw_fragmentwebclient](assets/cw_cw_fragmentwebclient.png)

Formuläret som återges är ett formulär som är baserat på fragment. Det vill säga, om användaren väljer amerikanska data, använder det returnerade formuläret fragment som baseras på amerikanska data. Sidfoten i formuläret innehåller till exempel en amerikansk adress, vilket visas på följande bild.

![cw_cw_fragementformfooter](assets/cw_cw_fragementformfooter.png)

På samma sätt, om användaren väljer kanadensiska data, innehåller det returnerade formuläret en kanadensisk adress, vilket visas på följande bild.

![cw_cw_fragementformfootercnd](assets/cw_cw_fragementformfootercnd.png)

>[!NOTE]
>
>Mer information om hur du skapar formulärdesigner baserade på fragment finns i [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63).

**Exempelfiler**

I det här avsnittet används exempelfiler som kan finnas på följande plats:

&lt;*Forms Designer installationskatalog*>/Samples/Forms/Purchase Order/Form Fragments

där &lt;*installationskatalogen*> är installationssökvägen. För klientprogrammets syften kopierades filen Purchase Order Dynamic.xdp från den här installationsplatsen och distribuerades till ett Forms-program med namnet *Applications/FormsApplication*. Filen Purchase Order Dynamic.xdp placeras i en mapp som heter FormsFolder. På samma sätt placeras fragmenten i mappen Fragments, som på följande bild.

![cw_cw_fragmentsdatabas](assets/cw_cw_fragmentsrepository.png)

Om du vill få åtkomst till formulärdesignen Purchase Order Dynamic.xdp anger du `Applications/FormsApplication/1.0/FormsFolder/Purchase Order Dynamic.xdp` som formulärnamn (den första parametern som skickas till metoden `renderPDFForm`) och `repository:///` som URI-värde för innehållsroten.

De XML-datafiler som används av webbprogrammet har flyttats från datamappen till `C:\Adobe` (filsystemet som tillhör J2EE-programservern som är värd för AEM Forms). Filnamnen är Purchase Order *Canada.xml* och Purchase Order *US.xml*.

>[!NOTE]
>
>Mer information om hur du skapar ett Forms-program med Workbench finns i [Workbench-hjälpen](https://www.adobe.com/go/learn_aemforms_workbench_63).

### Sammanfattning av steg {#summary-of-steps}

Så här skapar du ett webbaserat program som återger formulär baserat på fragment:

1. Skapa ett webbprojekt.
1. Skapa Java-programlogik som representerar Java-servleten.
1. Skapa webbsidan för webbprogrammet.
1. Paketera webbprogrammet i en WAR-fil.
1. Distribuera WAR-filen till J2EE-programservern.
1. Testa webbprogrammet.

>[!NOTE]
>
>Vissa av dessa steg är beroende av J2EE-programmet som AEM Forms är distribuerat till. Vilken metod du använder för att distribuera en WAR-fil beror till exempel på vilken J2EE-programserver du använder. I det här avsnittet antas att AEM Forms distribueras på JBoss®.

### Skapa ett webbprojekt {#creating-a-web-project}

Det första steget för att skapa ett webbprogram som innehåller en Java-server som kan anropa Forms-tjänsten är att skapa ett webbprojekt. Den Java-IDE som det här dokumentet är baserat på är Eclipse 3.3. Med Eclipse IDE skapar du ett webbprojekt och lägger till de JAR-filer som behövs i projektet. Till sist lägger du till en HTML-sida med namnet *index.html* och en Java-server i ditt projekt.

I följande lista anges de JAR-filer som du måste lägga till i ditt webbprojekt:

* adobe-forms-client.jar
* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-utilities.jar

Information om var dessa JAR-filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).

**Så här skapar du ett webbprojekt:**

1. Starta Eclipse och klicka på **Arkiv** > **Nytt projekt**.
1. I dialogrutan **Nytt projekt** väljer du **Webb** > **Dynamiskt webbprojekt**.
1. Skriv `FragmentsWebApplication` som namn på ditt projekt och klicka sedan på **Slutför**.

**Så här lägger du till nödvändiga JAR-filer i ditt projekt:**

1. Högerklicka på `FragmentsWebApplication`-projektet i projektutforskarfönstret och välj **Egenskaper**.
1. Klicka på **Java build path** och sedan på fliken **Libraries** .
1. Klicka på knappen **Lägg till externa JAR** och bläddra till de JAR-filer som ska inkluderas.

**Så här lägger du till en Java-servlet i ditt projekt:**

1. Högerklicka på `FragmentsWebApplication`-projektet i projektutforskarfönstret och välj **Nytt** > **Annat**.
1. Expandera mappen **Webb**, markera **Servlet** och klicka sedan på **Nästa**.
1. Skriv `RenderFormFragment` som namn på servern i dialogrutan Skapa server och klicka sedan på **Slutför**.

**Så här lägger du till en HTML-sida i ditt projekt:**

1. Högerklicka på `FragmentsWebApplication`-projektet i projektutforskarfönstret och välj **Nytt** > **Annat**.
1. Utöka mappen **Webb**, markera **HTML** och klicka på **Nästa**.
1. Skriv `index.html` som filnamn i dialogrutan Ny HTML och klicka sedan på **Slutför**.

>[!NOTE]
>
>Mer information om hur du skapar den HTML-sida som anropar Java-servern `RenderFormFragment` finns i [Skapa webbsidan](/help/forms/developing/rendering-forms.md#creating-the-web-page).

### Skapa Java-programlogik för serverleten {#creating-java-application-logic-for-the-servlet}

Du skapar Java-programlogik som anropar Forms-tjänsten inifrån Java-servern. I följande kod visas syntaxen för Java-servern `RenderFormFragment`:

```java
     public class RenderFormFragment extends HttpServlet implements Servlet {
         public void doGet(HttpServletRequest req, HttpServletResponse resp
         throws ServletException, IOException {
         doPost(req,resp);
 
         }
         public void doPost(HttpServletRequest req, HttpServletResponse resp
         throws ServletException, IOException {
             //Add code here to invoke the Forms service
             }
```

Normalt placerar du inte klientkod i en Java-serverds `doGet`- eller `doPost`-metod. En bättre programmeringspraxis är att placera den här koden i en separat klass, instansiera klassen inifrån metoden `doPost` (eller metoden `doGet`) och anropa lämpliga metoder. För kodrasteri begränsas dock kodexemplen i det här avsnittet till ett minimum och kodexempel placeras i metoden `doPost`.

Så här återger du ett formulär baserat på fragment med hjälp av Forms tjänst-API:

1. Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg. Mer information om platsen för dessa filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).
1. Hämta värdet för den alternativknapp som skickas från HTML-formuläret och ange om amerikansk eller kanadensisk information ska användas. Om USA skickas skapar du en `com.adobe.idp.Document` som lagrar data i *Purchase Order US.xml*. Om du har en kanadensisk version skapar du en `com.adobe.idp.Document` som lagrar data i filen *Purchase Order Canada.xml* .
1. Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper. (Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).)
1. Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.
1. Skapa ett `URLSpec`-objekt som lagrar URI-värden med hjälp av dess konstruktor.
1. Anropa `URLSpec`-objektets `setApplicationWebRoot`-metod och skicka ett strängvärde som representerar programmets webbrot.
1. Anropa `URLSpec`-objektets `setContentRootURI`-metod och skicka ett strängvärde som anger innehållets rot-URI-värde. Kontrollera att formulärdesignen och fragmenten finns i innehållets rot-URI. Annars genereras ett undantag. Om du vill referera till AEM Forms-databasen anger du `repository://`.
1. Anropa `URLSpec`-objektets `setTargetURL`-metod och skicka ett strängvärde som anger mål-URL-värdet som formulärdata skickas till. Om du definierar mål-URL:en i formulärdesignen kan du skicka en tom sträng. Du kan också ange den URL dit ett formulär ska skickas för att utföra beräkningar.
1. Anropa `FormsServiceClient`-objektets `renderPDFForm`-metod och skicka följande värden:

   * Ett strängvärde som anger formulärdesignens namn, inklusive filnamnstillägget.
   * Ett `com.adobe.idp.Document`-objekt som innehåller data som ska sammanfogas med formuläret (skapat i steg 2).
   * Ett `PDFFormRenderSpec`-objekt som lagrar körningsalternativ. Mer information finns i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).
   * Ett `URLSpec`-objekt som innehåller URI-värden som krävs av Forms-tjänsten för att återge ett formulär baserat på fragment.
   * Ett `java.util.HashMap`-objekt som lagrar bifogade filer. Det här är en valfri parameter och du kan ange `null` om du inte vill bifoga filer till formuläret.

   Metoden `renderPDFForm` returnerar ett `FormsResult`-objekt som innehåller en formulärdataström som måste skrivas till klientens webbläsare.

1. Skapa ett `com.adobe.idp.Document`-objekt genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
1. Hämta innehållstypen för objektet `com.adobe.idp.Document` genom att anropa dess `getContentType`-metod.
1. Ange innehållstypen för objektet `javax.servlet.http.HttpServletResponse` genom att anropa dess `setContentType`-metod och skicka innehållstypen för objektet `com.adobe.idp.Document`.
1. Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skriva formulärdataströmmen till klientwebbläsaren genom att anropa `javax.servlet.http.HttpServletResponse`-objektets `getOutputStream`-metod.
1. Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
1. Skapa en bytearray som fyller i den med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
1. Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

Följande kodexempel representerar den Java-server som anropar Forms-tjänsten och återger ett formulär baserat på fragment.

```java
 /*
     * This Java Quick Start uses the following JAR files
     * 1. adobe-forms-client.jar
     * 2. adobe-livecycle-client.jar
     * 3. adobe-usermanager-client.jar
     *
     * (Because Forms quick starts are implemented as Java servlets, it is
     * not necessary to include J2EE specific JAR files - the Java project
     * that contains this quick start is exported as a WAR file which
     * is deployed to the J2EE application server)
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs
     *
     * For complete details about the location of these JAR files,
     * see "Including AEM Forms library files" in Programming with AEM forms
     */
 import java.io.File;
 import java.io.FileInputStream;
 import java.io.IOException;
 import java.io.PrintWriter;
 
 import javax.servlet.Servlet;
 import javax.servlet.ServletException;
 import javax.servlet.ServletOutputStream;
 import javax.servlet.http.HttpServlet;
 import javax.servlet.http.HttpServletRequest;
 import javax.servlet.http.HttpServletResponse;
 import com.adobe.livecycle.formsservice.client.*;
 import java.util.*;
 import java.io.InputStream;
 import java.net.URL;
 
 import com.adobe.idp.Document;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 
 public class RenderFormFragment extends HttpServlet implements Servlet {
 
     public void doGet(HttpServletRequest req, HttpServletResponse resp)
         throws ServletException, IOException {
             doPost(req,resp);
 
     }
     public void doPost(HttpServletRequest req, HttpServletResponse resp)
     throws ServletException, IOException {
 
 
 
         try{
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "https://'[server]:[port]'");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Get the value of selected radio button
             String radioValue = req.getParameter("radio");
 
             //Create an Document object to store form data
             Document oInputData = null;
 
             //The value of the radio button determines the form data to use
             //which determines which fragments used in the form
             if (radioValue.compareTo("AMERICAN") == 0)            {
                 FileInputStream myData = new FileInputStream("C:\\Adobe\Purchase Order US.xml");
                 oInputData = new Document(myData);
             }
             else if (radioValue.compareTo("CANADIAN") == 0)            {
                 FileInputStream myData = new FileInputStream("C:\\Adobe\Purchase Order Canada.xml");
                 oInputData = new Document(myData);
             }
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create a FormsServiceClient object
             FormsServiceClient formsClient = new FormsServiceClient(myFactory);
 
             //Set the parameter values for the renderPDFForm method
             String formName = "Applications/FormsApplication/1.0/FormsFolder/Purchase Order Dynamic.xdp";
 
             //Cache the PDF form
             PDFFormRenderSpec pdfFormRenderSpec = new PDFFormRenderSpec();
             pdfFormRenderSpec.setCacheEnabled(new Boolean(true));
 
             //Specify URI values that are required to render a form
             //design based on fragments
             URLSpec uriValues = new URLSpec();
             uriValues.setApplicationWebRoot("https://'[server]:[port]'/RenderFormFragment");
             uriValues.setContentRootURI("repository:///");
             uriValues.setTargetURL("https://'[server]:[port]'/FormsServiceClientApp/HandleData");
 
             //Invoke the renderPDFForm method and write the
             //results to a client web browser
             FormsResult formOut = formsClient.renderPDFForm(
                         formName,               //formQuery
                         oInputData,             //inDataDoc
                         pdfFormRenderSpec,      //PDFFormRenderSpec
                         uriValues,                //urlSpec
                         null                    //attachments
                         );
 
             //Create a Document object that stores form data
             Document myData = formOut.getOutputContent();
 
             //Get the content type of the response and
             //set the HttpServletResponse object’s content type
             String contentType = myData.getContentType();
             resp.setContentType(contentType);
 
             //Create a ServletOutputStream object
             ServletOutputStream oOutput = resp.getOutputStream();
 
             //Create an InputStream object
             InputStream inputStream = myData.getInputStream();
 
             //Write the data stream to the web browser
             byte[] data = new byte[4096];
             int bytesRead = 0;
             while ((bytesRead = inputStream.read(data)) > 0)
             {
                 oOutput.write(data, 0, bytesRead);
             }
 
         }catch (Exception e) {
              System.out.println("The following exception occurred: "+e.getMessage());
       }
     }
 }
```

### Skapa webbsidan {#creating-the-web-page}

På webbsidan index.html finns en startpunkt för Java-servleten och Forms-tjänsten anropas. Den här webbsidan är ett grundläggande HTML-formulär som innehåller två alternativknappar och en skicka-knapp. Alternativknapparnas namn är radio. När användaren klickar på skicka-knappen skickas formulärdata till Java-servern `RenderFormFragment`.

Java-servleten hämtar data från HTML-sidan med följande Java-kod:

```java
             Document oInputData = null;
 
             //Get the value of selected radio button
             String radioValue = req.getParameter("radio");
 
             //The value of the radio button determines the form data to use
             //which determines which fragments used in the form
             if (radioValue.compareTo("AMERICAN") == 0)            {
                 FileInputStream myData = new FileInputStream("C:\\Adobe\Purchase Order US.xml");
                 oInputData = new Document(myData);
             }
             else if (radioValue.compareTo("CANADIAN") == 0)            {
                 FileInputStream myData = new FileInputStream("C:\\Adobe\Purchase Order Canada.xml");
                 oInputData = new Document(myData);
             }
```

Följande HTML-kod finns i filen index.html som skapades under installationen av utvecklingsmiljön. (Se [Skapa ett webbprojekt](/help/forms/developing/rendering-forms.md#creating-a-web-project).)

```xml
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "https://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
 <html xmlns="https://www.w3.org/1999/xhtml">
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
 <title>Untitled Document</title>
 </head>
 
 <body>
 <form name="myform" action="https://'[server]:[port]'/FragmentsWebApplication/RenderFormFragment" method="post">
      <table>
      <tr>
        <th>Forms Fragment Web Client</th>
      </tr>
      <tr>
        <td>
          <label>
          <input type="radio" name="radio" id="radio_Data" value="CANADIAN" />
          Canadian data<br />
          </label>
          <p>
            <label>
            <input type="radio" name="radio" id="radio_Data" value="AMERICAN" checked/>
            American data</label>
          </p>
        </td>
      </tr>
      <tr>
      <td>
        <label>
          <input type="submit" name="button_Submit" id="button_Submit" value="Submit" />
            </label>
            </td>
         </tr>
        </table>
      </form>
 </body>
 </html>
```

### Paketera webbprogrammet {#packaging-the-web-application}

Om du vill distribuera den Java-server som anropar Forms-tjänsten paketerar du webbprogrammet i en WAR-fil. Se till att externa JAR-filer som komponentens affärslogik är beroende av, t.ex. adobe-livecycle-client.jar och adobe-forms-client.jar, också inkluderas i WAR-filen.

**Så här paketerar du ett webbprogram till en WAR-fil:**

1. I fönstret **Projektutforskaren** högerklickar du på projektet `FragmentsWebApplication` och väljer **Exportera** > **WAR-fil**.
1. Skriv `FragmentsWebApplication` som namn på Java-projektet i textrutan **Webbmodul**.
1. I textrutan **Mål** skriver du `FragmentsWebApplication.war`**som** filnamn, anger platsen för WAR-filen och klickar sedan på Slutför.

### Distribuera WAR-filen till J2EE-programservern {#deploying-the-war-file-to-the-j2ee-application-server}

Du kan distribuera WAR-filen till J2EE-programservern som AEM Forms distribueras på. När WAR-filen har distribuerats kan du komma åt HTML webbsida via en webbläsare.

**Så här distribuerar du WAR-filen till J2EE-programservern:**

* Kopiera WAR-filen från exportsökvägen till `[Forms Install]\Adobe\Adobe Experience Manager Forms\jboss\server\all\deploy`.

### Testa ditt webbprogram {#testing-your-web-application}

När du har distribuerat webbprogrammet kan du testa det med en webbläsare. Om du använder samma dator som är värd för AEM Forms kan du ange följande URL:

* http://localhost:8080/FragmentsWebApplication/index.html

  Markera en alternativknapp och klicka på knappen Skicka. Ett formulär som är baserat på fragment visas i webbläsaren. Om det uppstår problem läser du loggfilen för J2EE-programservern.
