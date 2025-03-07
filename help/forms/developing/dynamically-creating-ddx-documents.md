---
title: Skapa DDX-dokument dynamiskt
description: Skapa ett DX-dokument dynamiskt med Java API och Web Service API. När du dynamiskt skapar ett DX-dokument kan du använda värden i DDX-dokumentet som hämtas under körning.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/assembling_pdf_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms, Document Services, APIs & Integrations
hide: true
hidefromtoc: true
exl-id: 3508d2d1-e05a-4733-b682-4b022348147a
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '2153'
ht-degree: 0%

---

# Skapa DDX-dokument dynamiskt {#dynamically-creating-ddx-documents}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Du kan dynamiskt skapa ett DX-dokument som kan användas för att utföra en Assembler-åtgärd. När du dynamiskt skapar ett DX-dokument kan du använda värden i DDX-dokumentet som hämtas under körning. Om du vill skapa ett DDX-dokument dynamiskt använder du klasser som tillhör det programmeringsspråk som du använder. Om du till exempel utvecklar ditt klientprogram med Java ska du använda klasser som tillhör paketet `org.w3c.dom.*`. Om du använder Microsoft .NET ska du också använda klasser som tillhör namnutrymmet `System.Xml`.

Innan du kan skicka DDX-dokumentet till Assembler-tjänsten måste du konvertera XML-filen från en `org.w3c.dom.Document`-instans till en `com.adobe.idp.Document`-instans. Om du använder webbtjänster konverterar du XML från den datatyp som används för att skapa XML (till exempel `XmlDocument`) till en `BLOB`-instans.

Anta att följande DDX-dokument skapas dynamiskt för den här diskussionen.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
      <PDFsFromBookmarks prefix="stmt">
     <PDF source="AssemblerResultPDF.pdf"/>
 </PDFsFromBookmarks>
 </DDX>
```

Det här DDX-dokumentet demonterar ett PDF-dokument. Vi rekommenderar att du är bekant med att dela upp PDF-dokument.

>[!NOTE]
>
>Mer information om Assembler-tjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

>[!NOTE]
>
>Mer information om ett DDX-dokument finns i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

## Sammanfattning av steg {#summary-of-steps}

Gör så här om du vill dela upp ett PDF-dokument med ett dynamiskt skapat DDX-dokument:

1. Inkludera projektfiler.
1. Skapa en PDF Assembler-klient.
1. Skapa DDX-dokumentet.
1. Konvertera DDX-dokumentet.
1. Ange körningsalternativ.
1. Disassemblera PDF-dokumentet.
1. Spara de omonterade PDF-dokumenten.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-assembler-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

**Skapa en PDF Assembler-klient**

Skapa en Assembler-tjänstklient innan du programmässigt utför en Assembler-åtgärd.

**Skapa DDX-dokumentet**

Skapa ett DX-dokument med det programmeringsspråk som du använder. Om du vill skapa ett DDX-dokument som demonterar ett PDF-dokument måste du se till att det innehåller elementet `PDFsFromBookmarks`. Konvertera den datatyp som används för att skapa DDX-dokumentet till en `com.adobe.idp.Document`-instans om du använder Java API. Om du använder webbtjänster konverterar du datatypen till en `BLOB`-instans.

**Konvertera DDX-dokumentet**

Ett DDX-dokument som skapas med `org.w3c.dom`-klasser måste konverteras till ett `com.adobe.idp.Document`-objekt. Om du vill utföra den här åtgärden när du använder Java API ska du använda Java XML-omformningsklasser. Om du använder webbtjänster konverterar du DDX-dokumentet till ett `BLOB`-objekt.

**Referera till ett PDF-dokument som ska demonteras**

Om du vill dela upp ett PDF-dokument refererar du till en PDF-fil som representerar det PDF-dokument som ska demonteras. När det skickas till Assembler-tjänsten returneras ett separat PDF-dokument för varje nivå 1-bokmärke i dokumentet.

**Ange körningsalternativ**

Du kan ställa in körningsalternativ som styr beteendet för Assembler-tjänsten när den utför ett jobb. Du kan till exempel ange ett alternativ som instruerar Assembler-tjänsten att fortsätta bearbeta ett jobb om ett fel uppstår. Om du vill ange körningsalternativ använder du ett `AssemblerOptionSpec`-objekt.

**Dela upp PDF-dokumentet**

Disassemblera PDF-dokumentet genom att anropa åtgärden `invokeDDX`. Skicka DDX-dokumentet som skapades dynamiskt. Assembler-tjänsten returnerar omonterade PDF-dokument i ett samlingsobjekt.

**Spara de omonterade PDF-dokumenten**

Alla omonterade PDF-dokument returneras inom ett samlingsobjekt. Upprepa genom samlingsobjektet och spara varje PDF-dokument som en PDF-fil.

**Se även**

[Skapa ett DDX-dokument dynamiskt med Java API](/help/forms/developing/dynamically-creating-ddx-documents.md#dynamically-create-a-ddx-document-using-the-java-api)

[Skapa ett DX-dokument dynamiskt med webbtjänstens API](/help/forms/developing/dynamically-creating-ddx-documents.md#dynamically-create-a-ddx-document-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Dela upp PDF-dokument programmatiskt](/help/forms/developing/programmatically-disassembling-pdf-documents.md#programmatically-disassembling-pdf-documents)

## Skapa ett DDX-dokument dynamiskt med Java API {#dynamically-create-a-ddx-document-using-the-java-api}

Skapa ett DX-dokument dynamiskt och demontera ett PDF-dokument med Assembler Service API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-assembler-client.jar, i Java-projektets klassökväg.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Skapa DDX-dokumentet.

   * Skapa ett Java `DocumentBuilderFactory`-objekt genom att anropa metoden `DocumentBuilderFactory` class&#39; `newInstance` .
   * Skapa ett Java `DocumentBuilder`-objekt genom att anropa `DocumentBuilderFactory`-objektets `newDocumentBuilder`-metod.
   * Anropa `DocumentBuilder`-objektets `newDocument`-metod för att instansiera ett `org.w3c.dom.Document`-objekt.
   * Skapa DX-dokumentets rotelement genom att anropa `org.w3c.dom.Document`-objektets `createElement`-metod. Den här metoden skapar ett `Element`-objekt som representerar rotelementet. Skicka ett strängvärde som representerar elementets namn till metoden `createElement`. Skicka returvärdet till `Element`. Ange sedan ett värde för det underordnade elementet genom att anropa dess `setAttribute`-metod. Lägg slutligen till elementet i rubrikelementet genom att anropa rubrikelementets `appendChild`-metod och skicka det underordnade elementobjektet som ett argument. Följande kodrader visar den här programlogiken:
     ` Element root = (Element)document.createElement("DDX");  root.setAttribute("xmlns","https://ns.adobe.com/DDX/1.0/");  document.appendChild(root);`

   * Skapa elementet `PDFsFromBookmarks` genom att anropa metoden `createElement` för objektet `Document`. Skicka ett strängvärde som representerar elementets namn till metoden `createElement`. Skicka returvärdet till `Element`. Ange ett värde för elementet `PDFsFromBookmarks` genom att anropa dess `setAttribute`-metod. Lägg till elementet `PDFsFromBookmarks` i elementet `DDX` genom att anropa DDX-elementets `appendChild`-metod. Skicka elementobjektet `PDFsFromBookmarks` som ett argument. Följande kodrader visar den här programlogiken:

     ` Element PDFsFromBookmarks = (Element)document.createElement("PDFsFromBookmarks");  PDFsFromBookmarks.setAttribute("prefix","stmt");  root.appendChild(PDFsFromBookmarks);`

   * Skapa ett `PDF`-element genom att anropa `Document`-objektets `createElement`-metod. Skicka ett strängvärde som representerar elementets namn. Skicka returvärdet till `Element`. Ange ett värde för elementet `PDF` genom att anropa dess `setAttribute`-metod. Lägg till elementet `PDF` i elementet `PDFsFromBookmarks` genom att anropa `PDFsFromBookmarks`-elementets `appendChild`-metod. Skicka elementobjektet `PDF` som ett argument. Följande kodrader visar den här programlogiken:

     ` Element PDF = (Element)document.createElement("PDF");  PDF.setAttribute("source","AssemblerResultPDF.pdf");  PDFsFromBookmarks.appendChild(PDF);`

1. Konvertera DDX-dokumentet.

   * Skapa ett `javax.xml.transform.Transformer`-objekt genom att anropa `javax.xml.transform.Transformer`-objektets statiska `newInstance`-metod.
   * Skapa ett `Transformer`-objekt genom att anropa `TransformerFactory`-objektets `newTransformer`-metod.
   * Skapa ett `ByteArrayOutputStream`-objekt med hjälp av dess konstruktor.
   * Skapa ett `javax.xml.transform.dom.DOMSource`-objekt med hjälp av dess konstruktor. Skicka objektet `org.w3c.dom.Document` som representerar DDX-dokumentet.
   * Skapa ett `javax.xml.transform.dom.DOMSource`-objekt med hjälp av dess konstruktor och skicka `ByteArrayOutputStream`-objektet.
   * Fyll i Java `ByteArrayOutputStream`-objektet genom att anropa `javax.xml.transform.Transformer`-objektets `transform`-metod. Skicka objekten `javax.xml.transform.dom.DOMSource` och `javax.xml.transform.stream.StreamResult`.
   * Skapa en bytearray och tilldela storleken på `ByteArrayOutputStream`-objektet till bytearrayen.
   * Fyll i bytearrayen genom att anropa `ByteArrayOutputStream`-objektets `toByteArray`-metod.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka bytearrayen.

1. Referera till ett PDF-dokument som ska demonteras.

   * Skapa ett `java.util.Map`-objekt som används för att lagra indata-PDF-dokument med hjälp av en `HashMap`-konstruktor.
   * Skapa ett `java.io.FileInputStream`-objekt med hjälp av konstruktorn och skicka platsen för det PDF-dokument som ska demonteras.
   * Skapa ett `com.adobe.idp.Document`-objekt. Skicka `java.io.FileInputStream`-objektet som innehåller PDF-dokumentet som ska demonteras.
   * Lägg till en post i objektet `java.util.Map` genom att anropa dess `put`-metod och skicka följande argument:

      * Ett strängvärde som representerar nyckelnamnet. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet. (I DDX-dokumentet som skapas dynamiskt är värdet `AssemblerResultPDF.pdf`.)
      * Ett `com.adobe.idp.Document`-objekt som innehåller det PDF-dokument som ska demonteras.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ som uppfyller dina affärskrav genom att anropa en metod som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar, anropar du `AssemblerOptionSpec`-objektets `setFailOnError`-metod och skickar `false`.

1. Disassemblera PDF-dokumentet.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar det dynamiskt skapade DDX-dokumentet
   * Ett `java.util.Map`-objekt som innehåller det PDF-dokument som ska demonteras
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen, inklusive standardteckensnittet och jobbloggsnivån

   Metoden `invokeDDX` returnerar ett `com.adobe.livecycle.assembler.client.AssemblerResult`-objekt som innehåller de omonterade PDF-dokumenten och eventuella undantag.

1. Spara de omonterade PDF-dokumenten.

   Gör så här för att få fram de uppdelade PDF-dokumenten:

   * Anropa metoden `getDocuments` för objektet `AssemblerResult`. Den här metoden returnerar ett `java.util.Map`-objekt.
   * Upprepa genom objektet `java.util.Map` tills du hittar det resulterande `com.adobe.idp.Document`-objektet.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att extrahera PDF-dokumentet.

**Se även**

[Snabbstart (SOAP-läge): Skapa ett DX-dokument dynamiskt med Java API](/help/forms/developing/assembler-service-java-api-quick.md#quick-start-soap-mode-dynamically-creating-a-ddx-document-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Skapa ett DX-dokument dynamiskt med webbtjänstens API {#dynamically-create-a-ddx-document-using-the-web-service-api}

Skapa ett DX-dokument dynamiskt och demontera ett PDF-dokument med Assembler Service API (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition när du anger en tjänstreferens: `http://localhost:8080/soap/services/AssemblerService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `AssemblerServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/AssemblerService?blob=mtom`). Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `AssemblerServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `AssemblerServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `AssemblerServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Skapa DDX-dokumentet.

   * Skapa ett `System.Xml.XmlElement`-objekt med hjälp av dess konstruktor.
   * Skapa DX-dokumentets rotelement genom att anropa `XmlElement`-objektets `CreateElement`-metod. Den här metoden skapar ett `Element`-objekt som representerar rotelementet. Skicka ett strängvärde som representerar elementets namn till metoden `CreateElement`. Ange ett värde för DDX-elementet genom att anropa dess `SetAttribute`-metod. Lägg slutligen till elementet i DDX-dokumentet genom att anropa `XmlElement`-objektets `AppendChild`-metod. Skicka DDX-objektet som ett argument. Följande kodrader visar den här programlogiken:

     ` System.Xml.XmlElement root = ddx.CreateElement("DDX");  root.SetAttribute("xmlns", "https://ns.adobe.com/DDX/1.0/");  ddx.AppendChild(root);`

   * Skapa DDX-dokumentets `PDFsFromBookmarks`-element genom att anropa `XmlElement`-objektets `CreateElement`-metod. Skicka ett strängvärde som representerar elementets namn till metoden `CreateElement`. Ange sedan ett värde för elementet genom att anropa dess `SetAttribute`-metod. Lägg till elementet `PDFsFromBookmarks` i rotelementet genom att anropa metoden `AppendChild` för elementet `DDX`. Skicka elementobjektet `PDFsFromBookmarks` som ett argument. Följande kodrader visar den här programlogiken:

     ` XmlElement PDFsFromBookmarks = ddx.CreateElement("PDFsFromBookmarks");  PDFsFromBookmarks.SetAttribute("prefix", "stmt");  root.AppendChild(PDFsFromBookmarks);`

   * Skapa DDX-dokumentets `PDF`-element genom att anropa `XmlElement`-objektets `CreateElement`-metod. Skicka ett strängvärde som representerar elementets namn till metoden `CreateElement`. Ange sedan ett värde för det underordnade elementet genom att anropa dess `SetAttribute`-metod. Lägg till elementet `PDF` i elementet `PDFsFromBookmarks` genom att anropa `PDFsFromBookmarks`-elementets `AppendChild`-metod. Skicka elementobjektet `PDF` som ett argument. Följande kodrader visar den här programlogiken:

     ` XmlElement PDF = ddx.CreateElement("PDF");  PDF.SetAttribute("source", "AssemblerResultPDF.pdf");  PDFsFromBookmarks.AppendChild(PDF);`

1. Konvertera DDX-dokumentet.

   * Skapa ett `System.IO.MemoryStream`-objekt med hjälp av dess konstruktor.
   * Fyll i objektet `MemoryStream` med DDX-dokumentet med hjälp av objektet `XmlElement` som representerar DDX-dokumentet. Anropa `XmlElement`-objektets `Save`-metod och skicka `MemoryStream`-objektet.
   * Skapa en bytearray och fylla den med data i objektet `MemoryStream`. I följande kod visas den här programlogiken:

     ` int bufLen = Convert.ToInt32(stream.Length);  byte[] byteArray = new byte[bufLen];  stream.Position = 0;  int count = stream.Read(byteArray, 0, bufLen);`

   * Skapa ett `BLOB`-objekt. Tilldela bytearrayen till `BLOB`-objektets `MTOM`-fält.

1. Referera till ett PDF-dokument som ska demonteras.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra indata-PDF-dokumentet. Det här `BLOB`-objektet skickas till `invokeOneDocument` som ett argument.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll i objektet `BLOB` genom att tilldela dess `MTOM`-egenskap innehållet i bytearrayen.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ för att uppfylla dina affärskrav genom att tilldela ett värde till en datamedlem som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar tilldelar du `false` till `AssemblerOptionSpec`-objektets `failOnError`-datamedlem.

1. Disassemblera PDF-dokumentet.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar det dynamiskt skapade DDX-dokumentet
   * Arrayen `mapItem` som innehåller PDF-indatadokumentet
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ

   Metoden `invokeDDX` returnerar ett `AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som inträffade.

1. Spara de omonterade PDF-dokumenten.

   Gör så här för att hämta nya PDF-dokument:

   * Få åtkomst till `documents`-fältet för `AssemblerResult`-objektet, som är ett `Map`-objekt som innehåller de omonterade PDF-dokumenten.
   * Iterera genom objektet `Map` för att få fram varje resulterande dokument. Sedan konverterar du den arraymedlemmens `value` till en `BLOB`.
   * Extrahera de binära data som representerar PDF-dokumentet genom att komma åt objektets `MTOM`-egenskap `BLOB`. Detta returnerar en array med byte som du kan skriva till en PDF-fil.

**Se även**

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)
