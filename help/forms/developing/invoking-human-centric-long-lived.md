---
title: Anropa personalcentrerade, långlivade processer
description: Programmatiskt anropa humancentrerade, långlivade processer som skapats i Workbench med ett Java-baserat klientprogram som använder anrops-API, ett ASP.NET som använder webbtjänster och ett klientprogram som skapats med Flex som använder Remoting.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: coding
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms, APIs & Integrations, AEM Forms on JEE
hide: true
hidefromtoc: true
exl-id: 1cc7b91e-c2f1-4831-b8cd-1399e7dd821e
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '3674'
ht-degree: 0%

---

# Anropa personalcentrerade, långlivade processer {#invoking-human-centric-long-lived-processes}

Du kan programmatiskt anropa humancentrerade långvariga processer som skapats i Workbench med följande klientprogram:

* Ett webbaserat Java-klientprogram som använder anrops-API. (Se [Anropa AEM Forms med Java API](/help/forms/developing/invoking-aem-forms-using-java.md)(/help/forms/developing/invoking-aem-forms-using-java.md#invoking-aem-forms-using-the-java-api).)
* Ett ASP.NET som använder webbtjänster. (Se [Anropa AEM Forms med webbtjänster](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-web-services).)
* Ett klientprogram som skapats med Flex och som använder Remoting. (Se [Anropa AEM Forms med (borttaget för AEM-formulär) AEM Forms Remoting](/help/forms/developing/invoking-aem-forms-using-remoting.md#invoking-aem-forms-using-remoting).)

Den långvariga processen som anropas heter *FirstAppSolution/PreLoanProcess*. Du kan skapa den här processen genom att följa den självstudiekurs som anges i [Skapa ditt första AEM Forms-program](https://www.adobe.com/go/learn_aemforms_firstapp_ds_63).

En mänsklig process inbegriper en uppgift som en användare kan svara på med hjälp av Workspace. Med Workbench kan du till exempel skapa en process där en bankansvarig kan godkänna eller avvisa en låneansökan. Följande bild visar processen *FirstAppSolution/PreLoanProcess*.

Processen *FirstAppSolution/PreLoanProcess* accepterar en indataparameter med namnet *formData* vars datatyp är XML. XML-data sammanfogas med en formulärdesign med namnet *PreLoanForm.xdp*. Följande bild visar ett formulär som representerar en uppgift som tilldelats en användare att godkänna eller neka en låneansökan. Användaren godkänner eller nekar programmet med Workspace. Workspace-användaren kan godkänna låneansökan genom att klicka på knappen Godkänn som visas på följande bild. Användaren kan också neka lånebegäran genom att klicka på Neka.

En långvarig process anropas asynkront och kan inte anropas synkront på grund av följande faktorer:

* En process kan omfatta en hel del tid.
* En process kan omfatta flera organisatoriska gränser.
* En process behöver externa indata för att den ska kunna slutföras. Tänk dig till exempel en situation där ett formulär skickas till en chef som inte är på kontoret. I det här fallet är processen inte slutförd förrän hanteraren returnerar och fyller i formuläret.

När en långvarig process anropas skapar AEM Forms ett anrops-ID-värde som en del av skapandet av en post. Posten spårar statusen för den långvariga processen och lagras i AEM Forms-databasen. Med hjälp av anropsidentifierarvärdet kan du spåra den långvariga processens status. Dessutom kan du använda processens identifierarvärde för anrop för att utföra processhanteraråtgärder, som att avsluta en processinstans som körs.

>[!NOTE]
>
>AEM Forms skapar inte ett värde för en anropsidentifierare eller en post när en kort process anropas.

Processen `FirstAppSolution/PreLoanProcess` anropas när en sökande skickar ett program, som representeras av XML-data. Namnet på indataprocessvariabeln är `formData` och dess datatyp är XML. Anta att följande XML-data används som indata till `FirstAppSolution/PreLoanProcess`-processen för den här diskussionen.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <LoanApp>
 <Name>Sam White</Name>
 <LoanAmount>250000</LoanAmount>
 <PhoneOrEmail>(555)555-5555</PhoneOrEmail>
 <ApprovalStatus>PENDING APPROVAL</ApprovalStatus>
 </LoanApp>
```

XML-data som skickas till en process måste matcha fälten i det formulär som används i processen. I annat fall visas inte data i formuläret. Alla program som anropar `FirstAppSolution/PreLoanProcess`-processen måste skicka den här XML-datakällan. De program som skapades i *Anropar personalcentrerade, långlivade processer* skapar dynamiskt XML-datakällan utifrån värden som en användare angett i en webbklient.

Med ett klientprogram kan du skicka *FirstAppSolution/PreLoanProcess* för att bearbeta de XML-data som krävs. En långvarig process returnerar ett anropsidentifierarvärde som returvärde. Följande bild visar klientprogram som anropar den långa processen*FirstAppSolution/PreLoanProcess. Klientprogrammen skickar XML-data och hämtar tillbaka ett strängvärde som representerar anropsidentifierarvärdet.

**Se även**

[Skapa ett Java-webbprogram som anropar en mänsklig, lång process](invoking-human-centric-long-lived.md#creating-a-java-web-application-that-invokes-a-human-centric-long-lived-process)

[Skapa ett ASP.NET webbprogram som anropar en människocentrerad, lång process](invoking-human-centric-long-lived.md#creating-an-asp-net-web-application-that-invokes-a-human-centric-long-lived-process)

[Skapa en klientapplikation som byggts med Flex och som anropar en mänsklig centrerad, långvarig process](invoking-human-centric-long-lived.md#creating-a-client-application-built-with-flex-that-invokes-a-human-centric-long-lived-process)

## Skapa ett Java-webbprogram som anropar en mänsklig, lång process {#creating-a-java-web-application-that-invokes-a-human-centric-long-lived-process}

Du kan skapa ett webbaserat program som använder en Java-servlet för att anropa `FirstAppSolution/PreLoanProcess`-processen. Om du vill anropa den här processen från en Java-server använder du anrops-API:t i Java-serverleten. (Se [Anropa AEM Forms med Java API](/help/forms/developing/invoking-aem-forms-using-java.md#invoking-aem-forms-using-the-java-api).)

Följande bild visar ett webbaserat klientprogram som skickar värden för namn, telefon (eller e-post) och belopp. Dessa värden skickas till Java-servern när användaren klickar på knappen Skicka program.

Java-servern utför följande uppgifter:

* Hämtar de värden som har bokförts från HTML-sidan till Java-servleten.
* Skapar dynamiskt en XML-datakälla som ska skickas till processen *FirstAppSolution/PreLoanProcess*. Värdena för namn, telefon (eller e-post) och belopp anges i XML-datakällan.
* Anropar processen *FirstAppSolution/PreLoanProcess* med AEM Forms Invocation API.
* Returnerar anropsidentifierarvärdet till klientens webbläsare.

### Sammanfattning av steg {#summary-of-steps}

Så här skapar du ett Java-webbaserat program som anropar processen `FirstAppSolution/PreLoanProcess`:

1. [Skapa ett webbprojekt](invoking-human-centric-long-lived.md#create-a-web-project).
1. [Skapa Java-programlogik för servern](invoking-human-centric-long-lived.md#create-java-application-logic-for-the-servlet).
1. [Skapa webbsidan för webbprogrammet](invoking-human-centric-long-lived.md#create-the-web-page-for-the-web-application)
1. [Paketera webbprogrammet i en WAR-fil](invoking-human-centric-long-lived.md#package-the-web-application-to-a-war-file).
1. [Distribuera WAR-filen till J2EE-programservern som är värd för AEM Forms](invoking-human-centric-long-lived.md#deploy-the-war-file-to-the-j2ee-application-server-hosting-aem-forms).
1. [Testa webbprogrammet](invoking-human-centric-long-lived.md#test-your-web-application).

>[!NOTE]
>
>Vissa av dessa steg är beroende av J2EE-programmet som AEM Forms är distribuerat till. Vilken metod du använder för att distribuera en WAR-fil beror till exempel på vilken J2EE-programserver du använder. Man utgår ifrån att AEM Forms används i JBoss®.

### Skapa ett webbprojekt {#create-a-web-project}

Det första steget för att skapa ett webbprogram är att skapa ett webbprojekt. Den Java-IDE som det här dokumentet är baserat på är Eclipse 3.3. Med Eclipse IDE skapar du ett webbprojekt och lägger till de JAR-filer som behövs i projektet. Lägg till en HTML-sida med namnet *index.html* och en Java-servlet i ditt projekt.

I följande lista anges de JAR-filer som ska inkluderas i ditt webbprojekt:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* J2EE.jar

Information om var dessa JAR-filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).

>[!NOTE]
>
>Filen J2EE.jar definierar datatyper som används av en Java-server. Du kan hämta den här JAR-filen från J2EE-programservern där AEM Forms är distribuerat.

**Skapa ett webbprojekt**

1. Starta Eclipse och klicka på **Arkiv** > **Nytt projekt**.
1. I dialogrutan **Nytt projekt** väljer du **Webb** > **Dynamiskt webbprojekt**.
1. Skriv `InvokePreLoanProcess` som namn på ditt projekt och klicka sedan på **Slutför**.

**Lägg till nödvändiga JAR-filer i ditt projekt**

1. Högerklicka på `InvokePreLoanProcess`-projektet i projektutforskarfönstret och välj **Egenskaper**.
1. Klicka på **Java build path** och sedan på fliken **Libraries** .
1. Klicka på knappen **Lägg till externa JAR** och bläddra till de JAR-filer som ska inkluderas.

**Lägg till en Java-server i ditt projekt**

1. Högerklicka på `InvokePreLoanProcess`-projektet i projektutforskarfönstret och välj **Nytt** > **Annat**.
1. Expandera mappen **Webb**, markera **Servlet** och klicka sedan på **Nästa**.
1. Skriv `SubmitXML` som namn på servern i dialogrutan Skapa server och klicka sedan på **Slutför**.

**Lägg till en HTML-sida i ditt projekt**

1. Högerklicka på `InvokePreLoanProcess`-projektet i projektutforskarfönstret och välj **Nytt** > **Annat**.
1. Utöka mappen **Webb**, markera **HTML** och klicka på **Nästa**.
1. Skriv `index.html` som filnamn i dialogrutan Ny HTML och klicka sedan på **Slutför**.

>[!NOTE]
>
>Mer information om hur du skapar HTML-innehåll som anropar Java-servern SubmitXML finns i [Skapa webbsidan för webbprogrammet](invoking-human-centric-long-lived.md#create-the-web-page-for-the-web-application).

### Skapa Java-programlogik för servern {#create-java-application-logic-for-the-servlet}

Skapa Java-programlogik som anropar processen `FirstAppSolution/PreLoanProcess` inifrån Java-serverpaketet. I följande kod visas syntaxen för Java-servern `SubmitXML`:

```java
     public class SubmitXML extends HttpServlet implements Servlet {
         public void doGet(HttpServletRequest req, HttpServletResponse resp
         throws ServletException, IOException {
         doPost(req,resp);
 
         }
         public void doPost(HttpServletRequest req, HttpServletResponse resp
         throws ServletException, IOException {
             //Add code here to invoke the FirstAppSolution/PreLoanProcess process
             }
```

Normalt placerar du inte klientkod i en Java-serverds `doGet`- eller `doPost`-metod. En bättre programmeringspraxis är att placera den här koden i en separat klass. Instansiera sedan klassen inifrån metoden `doPost` (eller metoden `doGet`) och anropa lämpliga metoder. För kodraperation begränsas dock kodexempel till ett minimum och placeras i metoden `doPost`.

Så här anropar du processen `FirstAppSolution/PreLoanProcess` med hjälp av API:t för anrop:

1. Inkludera JAR-klientfiler, t.ex. adobe-livecycle-client.jar, i Java-projektets klassökväg. Mer information om platsen för dessa filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).
1. Hämta namn, telefon och belopp som skickas från HTML-sidan. Använd dessa värden om du dynamiskt vill skapa en XML-datakälla som skickas till `FirstAppSolution/PreLoanProcess`-processen. Du kan använda `org.w3c.dom`-klasser för att skapa XML-datakällan (den här programlogiken visas i följande kodexempel).
1. Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper. (Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).)
1. Skapa ett `ServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet. Med ett `ServiceClient`-objekt kan du anropa en tjänståtgärd. Det hanterar uppgifter som att hitta, skicka och dirigera anropsbegäranden.
1. Skapa ett `java.util.HashMap`-objekt med hjälp av dess konstruktor.
1. Anropa `java.util.HashMap`-objektets `put`-metod för varje indataparameter för att skicka till den långvariga processen. Kontrollera att du anger namnet på processens indataparametrar. Eftersom processen `FirstAppSolution/PreLoanProcess` kräver en indataparameter av typen `XML` (med namnet `formData`) behöver du bara anropa metoden `put` en gång.

   ```java
    //Get the XML to pass to the FirstAppSolution/PreLoanProcess process
    org.w3c.dom.Document inXML = GetDataSource(name,phone,amount);
    
    //Create a Map object to store the parameter value
    Map params = new HashMap();
    params.put("formData", inXML);
   ```

1. Skapa ett `InvocationRequest`-objekt genom att anropa `ServiceClientFactory`-objektets `createInvocationRequest`-metod och skicka följande värden:

   * Ett strängvärde som anger namnet på den långvariga process som ska anropas. Om du vill anropa `FirstAppSolution/PreLoanProcess`-processen anger du `FirstAppSolution/PreLoanProcess`.
   * Ett strängvärde som representerar processåtgärdens namn. Namnet på den långvariga processåtgärden är `invoke`.
   * Objektet `java.util.HashMap` som innehåller de parametervärden som tjänståtgärden kräver.
   * Ett booleskt värde som anger `false`, vilket skapar en asynkron begäran (det här värdet kan användas för att anropa en långvarig process).

   >[!NOTE]
   >
   >*En kort process kan anropas genom att värdet true skickas som den fjärde parametern i metoden createInvocationRequest. Om värdet true skickas skapas en synkron begäran.*

1. Skicka anropsbegäran till AEM Forms genom att anropa `ServiceClient`-objektets `invoke`-metod och skicka `InvocationRequest`-objektet. Metoden `invoke` returnerar ett `InvocationReponse`-objekt.
1. En långvarig process returnerar ett strängvärde som representerar ett anropsidentifieringsvärde. Hämta det här värdet genom att anropa metoden `getInvocationId` för objektet `InvocationReponse`.

   ```java
    //Send the invocation request to the long-lived process and
    //get back an invocation response object
    InvocationResponse lcResponse = myServiceClient.invoke(lcRequest);
    String invocationId = lcResponse.getInvocationId();
   ```

1. Skriv anropsidentifieringsvärdet till klientens webbläsare. Du kan använda en `java.io.PrintWriter`-instans för att skriva det här värdet till klientens webbläsare.

### Snabbstart: Anropa en långvarig process med anrops-API {#quick-start-invoking-a-long-lived-process-using-the-invocation-api}

Följande Java-kodexempel representerar den Java-server som anropar `FirstAppSolution/PreLoanProcess`-processen.

```java
 /*
     * This Java Quick Start uses the following JAR files
     * 1. adobe-livecycle-client.jar
     * 2. adobe-usermanager-client.jar
     *
     * (Because this  quick start is implemented as a Java servlet, it is
     * not necessary to include J2EE specific JAR files - the Java project
     * that contains this quick start is exported as a WAR file which
     * is deployed to the J2EE application server)
     *
     * These JAR files are in the following path:
     * <install directory>/sdk/client-libs/common
     *
     * For complete details about the location of these JAR files,
     * see "Including AEM Forms library files" in Programming with AEM forms
     * */
 import java.io.ByteArrayOutputStream;
 import java.io.File;
 import java.io.IOException;
 import java.io.PrintWriter;
 
 import javax.servlet.ServletException;
 import javax.servlet.http.HttpServletRequest;
 import javax.servlet.http.HttpServletResponse;
 import java.util.*;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory;
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties;
 import javax.xml.parsers.DocumentBuilder;
 import javax.xml.parsers.DocumentBuilderFactory;
 import javax.xml.transform.Transformer;
 import javax.xml.transform.TransformerFactory;
 import javax.xml.transform.dom.DOMSource;
 import javax.xml.transform.stream.StreamResult;
 
 import com.adobe.idp.dsc.InvocationRequest;
 import com.adobe.idp.dsc.InvocationResponse;
 import com.adobe.idp.dsc.clientsdk.ServiceClient;
 import org.w3c.dom.Element;
 
     public class SubmitXML extends javax.servlet.http.HttpServlet implements javax.servlet.Servlet {
       static final long serialVersionUID = 1L;
 
        public SubmitXML() {
         super();
     }
 
 
     protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
         // TODO Auto-generated method stub
         doPost(request,response);
     }
 
     protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
         try{
             //Set connection properties required to invoke AEM Forms
             Properties connectionProps = new Properties();
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_EJB_ENDPOINT, "jnp://localhost:1099");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_EJB_PROTOCOL);
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator");
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password");
 
             //Create a ServiceClientFactory object
             ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps);
 
             //Create a ServiceClient object
             ServiceClient myServiceClient = myFactory.getServiceClient();
 
             //Get the values that are passed from the Loan HTML page
             String name = (String)request.getParameter("name");
             String phone = (String)request.getParameter("phone");
             String amount = (String)request.getParameter("amount");
 
             //Create XML to pass to the FirstAppSolution/PreLoanProcess process
             org.w3c.dom.Document inXML = GetDataSource(name,phone,amount);
 
             //Create a Map object to store the XML input parameter value
             Map params = new HashMap();
             params.put("formData", inXML);
 
             //Create an InvocationRequest object
             InvocationRequest lcRequest =  myFactory.createInvocationRequest(
                 "FirstAppSolution/PreLoanProcess", //Specify the long-lived process name
                     "invoke",           //Specify the operation name
                     params,               //Specify input values
                     false);               //Create an asynchronous request
 
             //Send the invocation request to the long-lived process and
             //get back an invocation response object
             InvocationResponse lcResponse = myServiceClient.invoke(lcRequest);
             String invocationId = lcResponse.getInvocationId();
 
             //Create a PrintWriter instance
             PrintWriter pp = response.getWriter();
 
             //Write the invocation identifier value back to the client web browser
             pp.println("The job status identifier value is: " +invocationId);
 
         }catch (Exception e) {
              System.out.println("The following exception occurred: "+e.getMessage());
       }
     }
 
 
      //Create XML data to pass to the long-lived process
      private static org.w3c.dom.Document GetDataSource(String name, String phone, String amount)
      {
             org.w3c.dom.Document document = null;
 
             try
             {
                 //Create DocumentBuilderFactory and DocumentBuilder objects
                 DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
                 DocumentBuilder builder = factory.newDocumentBuilder();
 
                 //Create a Document object
                 document = builder.newDocument();
 
                 //Create MortgageApp - the root element in the XML
                 Element root = (Element)document.createElement("LoanApp");
                 document.appendChild(root);
 
                 //Create an XML element for Name
                 Element nameElement = (Element)document.createElement("Name");
                 nameElement.appendChild(document.createTextNode(name));
                 root.appendChild(nameElement);
 
                 //Create an XML element for Phone
                 Element phoneElement = (Element)document.createElement("PhoneOrEmail");
                 phoneElement.appendChild(document.createTextNode(phone));
                 root.appendChild(phoneElement);
 
                 //Create an XML element for amount
                 Element loanElement = (Element)document.createElement("LoanAmount");
                 loanElement.appendChild(document.createTextNode(amount));
                 root.appendChild(loanElement);
 
                 //Create an XML element for ApprovalStatus
                 Element approveElement = (Element)document.createElement("ApprovalStatus");
                 approveElement.appendChild(document.createTextNode("PENDING APPROVAL"));
                 root.appendChild(approveElement);
 
               }
          catch (Exception e) {
                   System.out.println("The following exception occurred: "+e.getMessage());
                }
         return document;
          }
         }
```

### Skapa webbsidan för webbprogrammet {#create-the-web-page-for-the-web-application}

Webbsidan *index.html* ger en startpunkt till Java-servern som anropar processen `FirstAppSolution/PreLoanProcess`. Den här webbsidan är ett grundläggande HTML-formulär som innehåller ett HTML-formulär och en skicka-knapp. När användaren klickar på skicka-knappen skickas formulärdata till Java-servern `SubmitXML`.

Java-servleten hämtar data från HTML-sidan med följande Java-kod:

```java
 //Get the values that are passed from the Loan HTML page
 String name = request.getParameter("name");
 String phone = request.getParameter("phone");
 String amount = request.getParameter("amount");
```

Följande HTML-kod representerar filen index.html som skapades under installationen av utvecklingsmiljön. (Se [Skapa ett webbprojekt](invoking-human-centric-long-lived.md#create-a-web-project).)

```xml
 <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
 <title>Insert title here</title>
 </head>
 <body>
 <table>
     <TBODY>
         <TR>
             <td><img src="financeCorpLogo.jpg" width="172" height="62"></TD>
             <td><FONT size="+2"><strong>Java Loan Application Page</strong></FONT></TD>
             <td> </TD>
             <td> </TD>
         </TR>
 
     </TBODY>
 </TABLE>
     <FORM action="https://hiro-xp:8080/PreLoanProcess/SubmitXML" method="post">
        <table>
          <TBODY>
                <TR>
                      <td><LABEL for="name">Name: </LABEL></TD>
                  <td><INPUT type="text" name="name"></TD>
                  <td><input type="submit" value="Submit Application"></TD>
                  </TR>
            <TR>
                  <td> <LABEL for="phone">Phone/Email: </LABEL></TD>
              <td><INPUT type="text" name="phone"></TD>
                  <td></TD>
              </TR>
 
            <TR>
                  <td><LABEL for="amount">Amount: </LABEL></TD>
              <td><INPUT type="text" name="amount"></TD>
                 <td></TD>
             </TR>
          </TBODY>
 </TABLE>
       </FORM>
 </body>
 </html>
```

### Paketera webbprogrammet i en WAR-fil {#package-the-web-application-to-a-war-file}

Om du vill distribuera den Java-server som anropar `FirstAppSolution/PreLoanProcess`-processen paketerar du webbprogrammet i en WAR-fil. Kontrollera att externa JAR-filer som komponentens affärslogik är beroende av, t.ex. adobe-livecycle-client.jar och adobe-usermanager-client.jar, också ingår i WAR-filen.

Följande bild visar Eclipse-projektets innehåll, som paketeras i en WAR-fil.

>[!NOTE]
>
>I föregående bild kan JPG-filen ersättas med alla JPG-filer.

**Paketera ett webbprogram till en WAR-fil:**

1. I fönstret **Projektutforskaren** högerklickar du på projektet `InvokePreLoanProcess` och väljer **Exportera** > **WAR-fil**.
1. Skriv `InvokePreLoanProcess` som namn på Java-projektet i textrutan **Webbmodul**.
1. I textrutan **Mål** skriver du `PreLoanProcess.war`**som** filnamn, anger platsen för WAR-filen och klickar sedan på Slutför.

### Distribuera WAR-filen till J2EE-programservern där AEM Forms finns {#deploy-the-war-file-to-the-j2ee-application-server-hosting-aem-forms}

Distribuera WAR-filen till J2EE-programservern där AEM Forms är distribuerat. Om du vill distribuera WAR-filen till J2EE-programservern kopierar du WAR-filen från exportsökvägen till `[AEM Forms Install]\Adobe\Adobe Experience Manager Forms\jboss\server\lc_turnkey\deploy`.

>[!NOTE]
>
>Om AEM Forms inte distribueras på JBoss måste du distribuera WAR-filen i enlighet med J2EE-programservern som är värd för AEM Forms.

### Testa ditt webbprogram {#test-your-web-application}

När du har distribuerat webbprogrammet kan du testa det med en webbläsare. Om du använder samma dator som är värd för AEM Forms kan du ange följande URL:

* http://localhost:8080/PreLoanProcess/index.html

  Ange värden i HTML formulärfält och klicka på Skicka program. Om det uppstår problem läser du loggfilen för J2EE-programservern.

>[!NOTE]
>
>Bekräfta att Java-programmet har anropat processen genom att starta Workspace och godkänna lånet.

## Skapa ett ASP.NET webbprogram som anropar en människocentrerad, lång process {#creating-an-asp-net-web-application-that-invokes-a-human-centric-long-lived-process}

Du kan skapa ett ASP.NET som anropar `FirstAppSolution/PreLoanProcess`-processen. Använd webbtjänster om du vill anropa den här processen från ett ASP.NET. (Se [Anropa AEM Forms med webbtjänster](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-web-services).)

Följande bild visar ett ASP.NET-klientprogram som hämtar data från en slutanvändare. Data placeras i en XML-datakälla och skickas till `FirstAppSolution/PreLoanProcess`-processen när användaren klickar på knappen Skicka program.

När processen har anropats visas ett värde för anropsidentifierare. Ett anropsidentifierarvärde skapas som en del av en post som spårar statusen för den långvariga processen.

ASP.NET utför följande uppgifter:

* Hämtar värdena som användaren angett på webbsidan.
* Skapar dynamiskt en XML-datakälla som skickas till processen* FirstAppSolution/PreLoanProcess *. De tre värdena anges i XML-datakällan.
* Anropar processen* FirstAppSolution/PreLoanProcess *med webbtjänsterna.
* Returnerar anropsidentifierarvärdet och statusen för den långvariga åtgärden till klientens webbläsare.

### Sammanfattning av steg {#summary_of_steps-1}

Så här skapar du ett ASP.NET som kan anropa processen FirstAppSolution/PreLoanProcess:

1. [Skapa ett ASP.NET](invoking-human-centric-long-lived.md#create-an-asp-net-web-application).
1. [Skapa en ASP-sida som anropar FirstAppSolution/PreLoanProcess](invoking-human-centric-long-lived.md#create-an-asp-page-that-invokes-firstappsolution-preloanprocess).
1. [Kör ASP.NET](invoking-human-centric-long-lived.md#run-the-asp-net-application).

### Skapa ett ASP.NET {#create-an-asp-net-web-application}

Skapa ett Microsoft .NET C# ASP.NET-webbprogram. Följande bild visar innehållet i ASP.NET *InvokePreLoanProcess*.

Meddelande under Tjänstreferenser, det finns två objekt. Det första objektet heter* JobManager*. Den här referensen gör att ASP.NET kan anropa tjänsten Job Manager. Den här tjänsten returnerar information om statusen för en långvarig process. Om processen till exempel körs returnerar den här tjänsten ett numeriskt värde som anger att processen körs. Den andra referensen heter *PreLoanProcess*. Den här tjänstreferensen representerar referensen till processen* FirstAppSolution/PreLoanProcess *. När du har skapat en servicereferens är datatyper som är kopplade till AEM Forms-tjänsten tillgängliga för användning i ditt .NET-projekt.

**Skapa ett ASP.NET:**

1. Starta Microsoft Visual Studio 2008.
1. Välj **Ny**, **Webbplats** på menyn **Arkiv**.
1. Välj **ASP.NET** i listan **Mallar**.
1. Välj en plats för projektet i rutan **Plats**. Ge projektet namnet *InvokePreLoanProcess*.
1. I rutan **Språk** väljer du Visual C#
1. Klicka på OK.

**Lägg till tjänstreferenser:**

1. Välj **Lägg till tjänstreferens** på Projekt-menyn.
1. I dialogrutan **Adress** anger du WSDL till tjänsten Jobbhanteraren.

   ```java
    https://hiro-xp:8080/soap/services/JobManager?WSDL&lc_version=9.0.1
   ```

1. Skriv `JobManager` i fältet Namespace.
1. Klicka på **Gå** och sedan på **OK**.
1. Välj **Lägg till tjänstreferens** på menyn **Projekt**.
1. I dialogrutan **Adress** anger du WSDL till processen FirstAppSolution/PreLoanProcess.

   ```java
    https://hiro-xp:8080/soap/services/FirstAppSolution/PreLoanProcess?WSDL&lc_version=9.0.1
   ```

1. Skriv `PreLoanProcess` i fältet Namespace.
1. Klicka på **Gå** och sedan på **OK**.

>[!NOTE]
>
>Ersätt `hiro-xp` med IP-adressen för J2EE-programservern som är värd för AEM Forms. Alternativet `lc_version` ser till att AEM Forms-funktioner, som MTOM, är tillgängliga. Om du inte anger alternativet `lc_version`kan du inte anropa AEM Forms med MTOM. (Se [Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom).)

### Skapa en ASP-sida som anropar FirstAppSolution/PreLoanProcess {#create-an-asp-page-that-invokes-firstappsolution-preloanprocess}

I ASP.NET lägger du till ett webbformulär (en ASPX-fil) som ansvarar för att visa en HTML-sida för den som ansöker om lånet. Webbformuläret är baserat på en klass som härleds från `System.Web.UI.Page`. C#-programlogiken som anropar `FirstAppSolution/PreLoanProcess` finns i metoden `Button1_Click` (den här knappen representerar knappen Skicka program).

Följande bild visar ASP.NET

I följande tabell visas de kontroller som är en del av det här ASP.NET.

<table>
 <thead>
  <tr>
   <th><p>Kontrollnamn</p></th>
   <th><p>Beskrivning</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>TextBoxName</p></td>
   <td><p>Anger kundens för- och efternamn. </p></td>
  </tr>
  <tr>
   <td><p>TextrutaTelefon</p></td>
   <td><p>Anger kundens telefon- eller e-postadress. </p></td>
  </tr>
  <tr>
   <td><p>TextBoxAmount</p></td>
   <td><p>Anger lånebeloppet.</p></td>
  </tr>
  <tr>
   <td><p>Knapp1</p></td>
   <td><p>Representerar knappen Skicka program.</p></td>
  </tr>
  <tr>
   <td><p>LabelJobID</p></td>
   <td><p>En Label-kontroll som anger värdet för anropsidentifierarvärdet.</p></td>
  </tr>
  <tr>
   <td><p>LabelStatus</p></td>
   <td><p>En Label-kontroll som anger värdet för jobbstatusen. Det här värdet hämtas genom att tjänsten Job Manager anropas. </p></td>
  </tr>
 </tbody>
</table>

Programlogiken som är en del av ASP.NET måste dynamiskt skapa en XML-datakälla för att skicka till `FirstAppSolution/PreLoanProcess`-processen. De värden som sökanden angav på HTML-sidan måste anges i XML-datakällan. Dessa datavärden sammanfogas i formuläret när formuläret visas i Workspace. Klasserna i namnutrymmet `System.Xml` används för att skapa XML-datakällan.

När du anropar en process som kräver XML-data från ett ASP.NET är en XML-datatyp tillgänglig som du kan använda. Du kan alltså inte skicka en `System.Xml.XmlDocument`-instans till processen. Det fullständiga kvalificerade namnet för den här XML-instansen som ska skickas till processen är `InvokePreLoanProcess.PreLoanProcess.XML`. Konvertera instansen `System.Xml.XmlDocument` till `InvokePreLoanProcess.PreLoanProcess.XML`. Du kan utföra den här uppgiften med följande kod.

```java
 //Create the XML to pass to the FirstAppSolution/PreLoanProcess process
 XmlDocument myXML = CreateXML(userName, phone, amount);
 
 //Convert the XML to a InvokePreLoanProcess.PreLoanProcess.XML instance
 StringWriter sw = new StringWriter();
 XmlTextWriter xw = new XmlTextWriter(sw);
 myXML.WriteTo(xw);
 
 InvokePreLoanProcess.PreLoanProcess.XML inXML = new XML();
 inXML.document = sw.ToString();
```

Om du vill skapa en ASP-sida som anropar processen `FirstAppSolution/PreLoanProcess` utför du följande uppgifter i metoden `Button1_Click`:

1. Skapa ett `FirstAppSolution_PreLoanProcessClient`-objekt med hjälp av dess standardkonstruktor.
1. Skapa ett `FirstAppSolution_PreLoanProcessClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten och kodningstypen:

   ```java
    https://hiro-xp:8080/soap/services/FirstAppSolution/PreLoanProcess?blob=mtom
   ```

   Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Du måste dock ange `?blob=mtom`.

   >[!NOTE]
   >
   >Ersätt `hiro-xp`* med IP-adressen för J2EE-programservern som är värd för AEM Forms. *

1. Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för datamedlemmen `FirstAppSolution_PreLoanProcessClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
1. Ange `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-datamedlem till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
1. Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

   * Tilldela användarnamnet för AEM-formulär till datamedlemmen `FirstAppSolution_PreLoanProcessClient.ClientCredentials.UserName.UserName`.
   * Tilldela motsvarande lösenordsvärde till datamedlemmen `FirstAppSolution_PreLoanProcessClient.ClientCredentials.UserName.Password`.
   * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till datamedlemmen `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
   * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till datamedlemmen `BasicHttpBindingSecurity.Security.Mode`.

   I följande kodexempel visas dessa uppgifter.

   ```as3
    //Enable BASIC HTTP authentication
    BasicHttpBinding b = (BasicHttpBinding)mortgageClient.Endpoint.Binding;
    b.MessageEncoding = WSMessageEncoding.Mtom;
    mortgageClient.ClientCredentials.UserName.UserName = "administrator";
    mortgageClient.ClientCredentials.UserName.Password = "password";
    b.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
    b.Security.Mode = BasicHttpSecurityMode.TransportCredentialOnly;
    b.MaxReceivedMessageSize = 2000000;
    b.MaxBufferSize = 2000000;
    b.ReaderQuotas.MaxArrayLength = 2000000;
   ```

1. Hämta värden för namn, telefon och belopp som användaren angett på webbsidan. Använd dessa värden om du dynamiskt vill skapa en XML-datakälla som skickas till `FirstAppSolution/PreLoanProcess`-processen. Skapa en `System.Xml.XmlDocument` som representerar XML-datakällan som ska skickas till processen (den här programlogiken visas i följande kodexempel).
1. Konvertera instansen `System.Xml.XmlDocument` till `InvokePreLoanProcess.PreLoanProcess.XML` (den här programlogiken visas i följande kodexempel).
1. Anropa `FirstAppSolution/PreLoanProcess`-processen genom att anropa `FirstAppSolution_PreLoanProcessClient`-objektets `invoke_Async`-metod. Den här metoden returnerar ett strängvärde som representerar anropsidentifierarvärdet för den långvariga processen.
1. Skapa en `JobManagerClient` med hjälp av konstruktorn is. (Kontrollera att du har angett en tjänstreferens för tjänsten Job Manager.)
1. Upprepa steg 1-5. Ange följande URL för steg 2: `https://hiro-xp:8080/soap/services/JobManager?blob=mtom`.
1. Skapa ett `JobId`-objekt med hjälp av dess konstruktor.
1. Ange `JobId`-objektets `id`-datamedlem med returvärdet för `FirstAppSolution_PreLoanProcessClient`-objektets `invoke_Async`-metod.
1. Tilldela `value` true till `JobId`-objektets `persistent`-datamedlem.
1. Skapa ett `JobStatus`-objekt genom att anropa `JobManagerService`-objektets `getStatus`-metod och skicka `JobId`-objektet.
1. Hämta statusvärdet genom att hämta värdet för `statusCode`-objektets `JobStatus`-datamedlem.
1. Tilldela anropsidentifierarvärdet till fältet `LabelJobID.Text`.
1. Tilldela statusvärdet till fältet `LabelStatus.Text`.

### Snabbstart: Anropa en långvarig process med webbtjänstens API {#quick-start-invoking-a-long-lived-process-using-the-web-service-api}

Följande exempel på C#-kod anropar `FirstAppSolution/PreLoanProcess`processen.

```csharp
 ???/**
     * Ensure that you create a .NET project that uses
     * MS Visual Studio 2008 and version 3.5 of the .NET
     * framework. This is required to invoke a
     * AEM Forms service using MTOM.
 
 
 using System;
 using System.Collections;
 using System.Configuration;
 using System.Data;
 using System.Linq;
 using System.Web;
 using System.ServiceModel;
 using System.Web.Security;
 using System.Web.UI;
 using System.Web.UI.HtmlControls;
 using System.Web.UI.WebControls;
 using System.Web.UI.WebControls.WebParts;
 using System.Xml.Linq;
 using System.Xml;
 using System.IO;
 
 //A reference to FirstAppSolution/PreLoanProcess
 using InvokePreLoanProcess.PreLoanProcess;
 
 //A reference to JobManager service
 using InvokePreLoanProcess.JobManager;
 
 
 namespace InvokePreLoanProcess
 {
        public partial class _Default : System.Web.UI.Page
        {
            //This method is called when the Submit Application button is
            //Clicked
            protected void Button1_Click(object sender, EventArgs e)
            {
                //Create a FirstAppSolution_PreLoanProcessClient object
                FirstAppSolution_PreLoanProcessClient mortgageClient = new FirstAppSolution_PreLoanProcessClient();
                mortgageClient.Endpoint.Address = new System.ServiceModel.EndpointAddress("https://hiro-xp:8080/soap/services/FirstAppSolution/PreLoanProcess?blob=mtom");
 
                //Enable BASIC HTTP authentication
                BasicHttpBinding b = (BasicHttpBinding)mortgageClient.Endpoint.Binding;
                b.MessageEncoding = WSMessageEncoding.Mtom;
                mortgageClient.ClientCredentials.UserName.UserName = "administrator";
                mortgageClient.ClientCredentials.UserName.Password = "password";
                b.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
                b.Security.Mode = BasicHttpSecurityMode.TransportCredentialOnly;
                b.MaxReceivedMessageSize = 2000000;
                b.MaxBufferSize = 2000000;
                b.ReaderQuotas.MaxArrayLength = 2000000;
 
                //Retrieve values that user entered into the web page
                String userName = TextBoxName.Text;
                String phone = TextBoxPhone.Text;
                String amount = TextBoxAmount.Text;
 
                //Create the XML to pass to the FirstAppSolution/PreLoanProcess process
                XmlDocument myXML = CreateXML(userName, phone, amount);
 
                StringWriter sw = new StringWriter();
                XmlTextWriter xw = new XmlTextWriter(sw);
                myXML.WriteTo(xw);
 
                InvokePreLoanProcess.PreLoanProcess.XML inXML = new XML();
                inXML.document = sw.ToString();
 
                //INvoke the FirstAppSolution/PreLoanProcess process
                String invocationID =  mortgageClient.invoke_Async(inXML);
 
                //Create a JobManagerClient object to obtain the status of the long-lived operation
                JobManagerClient jobManager = new JobManagerClient();
                jobManager.Endpoint.Address = new System.ServiceModel.EndpointAddress("https://hiro-xp:8080/soap/services/JobManager?blob=mtom");
 
                //Enable BASIC HTTP authentication
                BasicHttpBinding b1 = (BasicHttpBinding)jobManager.Endpoint.Binding;
                b1.MessageEncoding = WSMessageEncoding.Mtom;
                jobManager.ClientCredentials.UserName.UserName = "administrator";
                jobManager.ClientCredentials.UserName.Password = "password";
                b1.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
                b1.Security.Mode = BasicHttpSecurityMode.TransportCredentialOnly;
                b1.MaxReceivedMessageSize = 2000000;
                b1.MaxBufferSize = 2000000;
                b1.ReaderQuotas.MaxArrayLength = 2000000;
 
 
                //Create a JobID object that represents the status of the
                //long-lived operation
                JobId jobId = new JobId();
                jobId.id = invocationID;
                jobId.persistent = true;
                JobStatus jobStatus = jobManager.getStatus(jobId);
                System.Int16 val2 = jobStatus.statusCode;
                LabelJobID.Text = "The job status identifier value is " + invocationID;
                LabelStatus.Text = "The status of the long-lived operation is " + getJobDescription(val2);
 
            }
 
            private static XmlDocument CreateXML(String name, String phone, String amount)
            {
                //This method dynamically creates a DDX document
                //to pass to the FirstAppSolution/PreLoanProcess process
                XmlDocument xmlDoc = new XmlDocument();
 
                //Create the root element and append it to the XML DOM
                System.Xml.XmlElement root = xmlDoc.CreateElement("LoanApp");
                xmlDoc.AppendChild(root);
 
                //Create the Name element
                XmlElement nameElement = xmlDoc.CreateElement("Name");
                nameElement.AppendChild(xmlDoc.CreateTextNode(name));
                root.AppendChild(nameElement);
 
                //Create the LoanAmount element
                XmlElement LoanAmount = xmlDoc.CreateElement("LoanAmount");
                LoanAmount.AppendChild(xmlDoc.CreateTextNode(amount));
                root.AppendChild(LoanAmount);
 
                //Create the PhoneOrEmail element
                XmlElement PhoneOrEmail = xmlDoc.CreateElement("PhoneOrEmail");
                PhoneOrEmail.AppendChild(xmlDoc.CreateTextNode(phone));
                root.AppendChild(PhoneOrEmail);
 
                //Create the ApprovalStatus element
                XmlElement ApprovalStatus = xmlDoc.CreateElement("ApprovalStatus");
                ApprovalStatus.AppendChild(xmlDoc.CreateTextNode("PENDING APPROVAL"));
                root.AppendChild(ApprovalStatus);
 
                //Return the XmlElement instance
                return xmlDoc;
            }
 
 
            //Returns the String value of the Job Manager status code
            private String getJobDescription(int val)
            {
                switch(val)
                {
                    case 0:
                        return "JOB_STATUS_UNKNOWN";
 
                    case 1:
                        return "JOB_STATUS_QUEUED";
 
                    case 2:
                        return "JOB_STATUS_RUNNING";
 
                    case 3:
                        return "JOB_STATUS_COMPLETED";
 
                    case 4:
                        return "JOB_STATUS_FAILED";
 
                     case 5:
                        return "JOB_STATUS_COMPLETED";
 
                    case 6:
                        return "JOB_STATUS_SUSPENDED";
 
                    case 7:
                        return "JOB_STATUS_COMPLETE_REQUESTED";
 
                    case 8:
                        return "JOB_STATUS_TERMINATE_REQUESTED";
 
                     case 9:
                        return "JOB_STATUS_SUSPEND_REQUESTED";
 
                       case 10:
                        return "JOB_STATUS_RESUME_REQUESTED";
                }
 
                return "";
            }
       }
 }
 
```

>[!NOTE]
>
>Värdena i den användardefinierade metoden getJobDescription motsvarar värden som returneras av tjänsten Job Manager.

### Kör ASP.NET {#run-the-asp-net-application}

När du har kompilerat och distribuerat ASP.NET kan du köra det i en webbläsare. Om namnet på ASP.NET är *InvokePreLoanProcess* anger du följande URL i en webbläsare:

*http://localhost:1629/InvokePreLoanProcess/*Default.aspx

där localhost är namnet på den webbserver som är värd för ASP.NET och 1629 är portnumret. När du kompilerar och bygger ditt ASP.NET-program distribueras det automatiskt av Microsoft Visual Studio.

>[!NOTE]
>
>Bekräfta att ASP.NET aktiverade processen genom att starta Workspace och godkänna lånet.

## Skapa en klientapplikation som byggts med Flex och som anropar en mänsklig centrerad, långvarig process {#creating-a-client-application-built-with-flex-that-invokes-a-human-centric-long-lived-process}

Du kan skapa ett klientprogram som skapats med Flex för att anropa processen *FirstAppSolution/PreLoanProcess*. Det här programmet använder Remoting för att anropa processen *FirstAppSolution/PreLoanProcess*. (Se [Anropa AEM Forms med (borttaget för AEM-formulär) AEM Forms Remoting](/help/forms/developing/invoking-aem-forms-using-remoting.md#invoking-aem-forms-using-remoting).)

Följande bild visar ett klientprogram som byggts med Flex och som samlar in data från en slutanvändare. Data placeras i en XML-datakälla och skickas till processen.

När processen har anropats visas ett värde för anropsidentifierare. Ett anropsidentifierarvärde skapas som en del av en post som spårar statusen för den långvariga processen.

Klientprogrammet som skapats med Flex utför följande uppgifter:

* Hämtar värdena som användaren angett på webbsidan.
* Skapar dynamiskt en XML-datakälla som skickas till processen *FirstAppSolution/PreLoanProcess*. De tre värdena anges i XML-datakällan.
* Anropar processen *FirstAppSolution/PreLoanProcess* med Remoting.
* Returnerar det ID-anropsvärde som används för den långvariga processen.

### Sammanfattning av steg {#summary_of_steps-2}

Så här skapar du ett klientprogram som skapats med Flex och som kan anropa processen FirstAppSolution/PreLoanProcess:

1. Starta ett nytt Flex-projekt.
1. Inkludera filen adobe-remoting-provider.swc i projektets klassökväg. (Se [Inkludera AEM Forms Flex-biblioteksfilen](/help/forms/developing/invoking-aem-forms-using-remoting.md#including-the-aem-forms-flex-library-file).)
1. Skapa en `mx:RemoteObject`-instans via antingen ActionScript eller MXML. (Se [Skapa en mx:RemoteObject-instans](/help/forms/developing/invoking-aem-forms-using-remoting.md))
1. Konfigurera en `ChannelSet`-instans för kommunikation med AEM Forms och koppla den till `mx:RemoteObject`-instansen. (Se [Skapa en kanal till AEM Forms](/help/forms/developing/invoking-aem-forms-using-remoting.md).)
1. Anropa ChannelSet-metoden `login` eller tjänstens `setCredentials`-metod för att ange användaridentifierarvärdet och lösenordet. (Se [Använda enkel inloggning](/help/forms/developing/invoking-aem-forms-using-remoting.md#using-single-sign-on).)
1. Skapa XML-datakällan som ska skickas till `FirstAppSolution/PreLoanProcess`-processen genom att skapa en XML-instans. (Den här programlogiken visas i följande kodexempel.)
1. Skapa ett objekt av typen Object med hjälp av dess konstruktor. Tilldela XML till objektet genom att ange namnet på processens indataparameter, vilket visas i följande kod:

   ```csharp
    //Get the XML data to pass to the AEM Forms process
    var xml:XML = createXML();
    var params:Object = new Object();
    params["formData"]=xml;
   ```

1. Anropa `FirstAppSolution/PreLoanProcess`-processen genom att anropa `mx:RemoteObject`-instansens `invoke_Async`-metod. Skicka `Object` som innehåller indataparametern. (Se [Skicka indatavärden](/help/forms/developing/invoking-aem-forms-using-remoting.md).)
1. Hämta det anropsidentifieringsvärde som returneras från en långvarig process, vilket visas i följande kod:

   ```csharp
    // Handles async call that invokes the long-lived process
    private function resultHandler(event:ResultEvent):void
    {
    ji = event.result as JobId;
    jobStatusDisplay.text = "Job Status ID: " + ji.jobId as String;
    }
   ```

### Anropa en långvarig process med Remoting {#invoking-a-long-lived-process-using-remoting}

Följande Flex-kodexempel anropar processen `FirstAppSolution/PreLoanProcess`.

```java
 <?xml version="1.0" encoding="utf-8"?>
 
 <mx:Application  xmlns="*" backgroundColor="#FFFFFF"
      creationComplete="initializeChannelSet();">
 
 <mx:Script>
          <![CDATA[
 
             import mx.controls.Alert;
             import mx.rpc.events.FaultEvent;
             import mx.rpc.events.ResultEvent;
             import flash.net.navigateToURL;
             import mx.messaging.ChannelSet;
             import mx.messaging.channels.AMFChannel;
             import mx.collections.ArrayCollection;
             import mx.rpc.livecycle.JobId;
             import mx.rpc.livecycle.JobStatus;
             import mx.rpc.livecycle.DocumentReference;
             import mx.formatters.NumberFormatter;
 
             // Holds the job ID returned by LC.JobManager
             private var ji:JobId;
 
             private function initializeChannelSet():void
              {
              var cs:ChannelSet= new ChannelSet();
         cs.addChannel(new AMFChannel("remoting-amf", "https://hiro-xp:8080/remoting/messagebroker/amf"));
         LC_MortgageApp.setCredentials("tblue", "password");
         LC_MortgageApp.channelSet = cs;
              }
 
            private function submitApplication():void
             {
             //Get the XML data to pass to the AEM Forms process
             var xml:XML = createXML();
             var params:Object = new Object();
             params["formData"]=xml;
             LC_MortgageApp.invoke_Async(params);
             }
 
             // Handles async call that invokes the long-lived process
             private function resultHandler(event:ResultEvent):void
             {
                ji = event.result as JobId;
                jobStatusDisplay.text = "Job Status ID: " + ji.jobId as String;
             }
 
             private function createXML():XML
             {
                //Calculate the Mortgage value to place in the XML data
                var propertyPrice:String = txtAmount.text ;
                var name:String = txtName.text ;
                var phone:String = txtPhone.text ;;
 
                var model:XML =
 
                  <LoanApp>
                           <Name>{name}</Name>
                           <LoanAmount>{propertyPrice}</LoanAmount>
                           <PhoneOrEmail>{phone}</PhoneOrEmail>
                           <ApprovalStatus>PENDING APPROVAL</ApprovalStatus>
                  </LoanApp>
 
              return model;
             }
 
 
          ]]>
       </mx:Script>
 
       <!-- Declare the RemoteObject and set its destination to the mortgage-app remoting endpoint defined in AEM Forms. -->
       <mx:RemoteObject id="LC_MortgageApp" destination="FirstAppSolution/PreLoanProcess" result="resultHandler(event);">
          <mx:method name="invoke_Async" result="resultHandler(event)"/>
      </mx:RemoteObject>
 
 
     <mx:Grid x="229" y="186">
         <mx:GridRow width="100%" height="100%">
             <mx:GridItem width="100%" height="100%">
                 <mx:Image>
                     <mx:source>file:///D|/LiveCycle_9/FirstApp/financeCorpLogo.jpg</mx:source>
                 </mx:Image>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
                 <mx:Label text="Flex Loan Application Page" fontSize="20"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
             </mx:GridItem>
         </mx:GridRow>
         <mx:GridRow width="100%" height="100%">
             <mx:GridItem width="100%" height="100%">
                 <mx:Label text="Name:" fontSize="12" fontWeight="bold"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
                 <mx:TextInput id="txtName"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
                 <mx:Button label="Submit Application" click="submitApplication()"/>
             </mx:GridItem>
         </mx:GridRow>
         <mx:GridRow width="100%" height="100%">
             <mx:GridItem width="100%" height="100%">
                 <mx:Label text="Phone/Email:" fontSize="12" fontWeight="bold"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
                 <mx:TextInput id="txtPhone"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
             </mx:GridItem>
         </mx:GridRow>
         <mx:GridRow width="100%" height="100%">
             <mx:GridItem width="100%" height="100%">
                 <mx:Label text="Amount:" fontSize="12" fontWeight="bold"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
                 <mx:TextInput id="txtAmount"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
             </mx:GridItem>
         </mx:GridRow>
         <mx:GridRow width="100%" height="100%">
             <mx:GridItem width="100%" height="100%">
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
                 <mx:Label text="Label" id="jobStatusDisplay" enabled="true" fontSize="12" fontWeight="bold"/>
             </mx:GridItem>
             <mx:GridItem width="100%" height="100%">
             </mx:GridItem>
         </mx:GridRow>
     </mx:Grid>
 
 </mx:Application>
 
```
