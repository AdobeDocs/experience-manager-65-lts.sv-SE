---
title: Avgöra om dokumenten är PDF/A-kompatibla
description: Använd Assembler-tjänsten för att avgöra om ett PDF-dokument är PDF/A-kompatibelt med Java API och Web Service API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/assembling_pdf_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2065'
ht-degree: 0%

---

# Avgöra om dokumenten är PDF/A-kompatibla {#determining-whether-documents-are-pdf-a-compliant}

Du kan avgöra om ett PDF-dokument är PDF/A-kompatibelt med hjälp av Assembler-tjänsten. Ett PDF/A-dokument finns som arkiveringsformat för långtidsarkivering av dokumentets innehåll. Teckensnitten bäddas in i dokumentet och filen är okomprimerad. Därför är ett PDF/A-dokument vanligtvis större än ett vanligt PDF-dokument. Ett PDF/A-dokument innehåller inte heller ljud- och videoinnehåll.

Specifikationen PDF/A-1 består av två överensstämmelsenivåer, nämligen A och B. Den största skillnaden mellan de två nivåerna är stödet för logisk struktur (hjälpmedel), som inte krävs för överensstämmelsenivå B. Oavsett överensstämmelsenivå anger PDF/A-1 att alla teckensnitt är inbäddade i det genererade PDF/A-dokumentet. För närvarande stöds endast PDF/A-1b vid validering (och konvertering).

Anta att följande DDX-dokument används för den här diskussionen.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
         <DocumentInformation source="Loan.pdf" result="Loan_result.xml">
         <PDFAValidation compliance="PDF/A-1b" resultLevel="Detailed"                       ignoreUnusedResources="true" allowCertificationSignatures="true" />
     </DocumentInformation>
 </DDX>
```

I det här DDX-dokumentet instruerar `DocumentInformation`-elementet Assembler-tjänsten att returnera information om indata-PDF-dokumentet. I `DocumentInformation`-elementet instruerar `PDFAValidation`-elementet Assembler-tjänsten att ange om PDF-indatadokumentet är PDF/A-kompatibelt.

Assembler-tjänsten returnerar information som anger om PDF-indatadokumentet är PDF/A-kompatibelt i ett XML-dokument som innehåller ett `PDFAConformance`-element. Om PDF-indatadokumentet är PDF/A-kompatibelt är värdet för `PDFAConformance`-elementets `isCompliant`-attribut `true`. Om PDF-dokumentet inte är PDF/A-kompatibelt är värdet för `PDFAConformance`-elementets `isCompliant`-attribut `false`.

>[!NOTE]
>
>Eftersom DDX-dokumentet som anges i det här avsnittet innehåller ett `DocumentInformation`-element returnerar Assembler-tjänsten XML-data i stället för ett PDF-dokument. Det innebär att Assembler-tjänsten inte sammanställer eller demonterar ett PDF-dokument, utan returnerar information om indata-PDF-dokumentet i ett XML-dokument.

>[!NOTE]
>
>Mer information om Assembler-tjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

>[!NOTE]
>
>Mer information om ett DDX-dokument finns i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

## Sammanfattning av steg {#summary-of-steps}

Så här avgör du om ett PDF-dokument är PDF/A-kompatibelt:

1. Inkludera projektfiler.
1. Skapa en PDF Assembler-klient.
1. Referera till ett befintligt DDX-dokument.
1. Referera till ett PDF-dokument som används för att fastställa om PDF/A är kompatibelt.
1. Ange körningsalternativ.
1. Hämta information om PDF-dokumentet.
1. Spara det returnerade XML-dokumentet.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-assembler-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

Om AEM Forms körs på en annan J2EE-programserver än JBoss måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på. Mer information om platsen för alla AEM Forms JAR-filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).

**Skapa en PDF Assembler-klient**

Innan du programmässigt kan utföra en Assembler-åtgärd måste du skapa en Assembler-tjänstklient.

**Referera till ett befintligt DDX-dokument**

Det måste finnas referenser till ett DDX-dokument för att en Assembler-tjänståtgärd ska kunna utföras. Om du vill avgöra om ett PDF-indatadokument är PDF/A-kompatibelt kontrollerar du att DDX-dokumentet innehåller elementet `PDFAValidation` i ett `DocumentInformation` -element. Elementet `PDFAValidation` instruerar Assembler-tjänsten att returnera ett XML-dokument som anger om PDF-indatadokumentet är PDF/A-kompatibelt.

**Referera till ett PDF-dokument som används för att fastställa kompatibilitet med PDF/A**

En referens till ett PDF-dokument måste skickas till Assembler-tjänsten för att avgöra om PDF-dokumentet är PDF/A-kompatibelt.

**Ange körningsalternativ**

Du kan ställa in körningsalternativ som styr beteendet för Assembler-tjänsten när den utför ett jobb. Du kan till exempel ange ett alternativ som instruerar Assembler-tjänsten att fortsätta bearbeta ett jobb om ett fel uppstår. Mer information om de körningsalternativ du kan ange finns i klassreferensen `AssemblerOptionSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

**Hämta information om PDF-dokumentet**

När du har skapat Assembler-tjänstklienten, refererat till DDX-dokumentet, refererat till ett interaktivt PDF-dokument och angett körningsalternativ, kan du anropa åtgärden `invokeDDX`. Eftersom DDX-dokumentet innehåller elementet `DocumentInformation` returnerar Assembler-tjänsten XML-data i stället för ett PDF-dokument.

**Spara det returnerade XML-dokumentet**

Det XML-dokument som Assembler-tjänsten returnerar anger om indata-PDF-dokumentet är PDF/A-kompatibelt. Om PDF-indatadokumentet till exempel inte är PDF/A-kompatibelt returnerar Assembler-tjänsten ett XML-dokument som innehåller följande element:

```xml
 <PDFAConformance isCompliant="false" compliance="PDF/A-1b" resultLevel="Detailed" ignoreUnusedResources="true" allowCertificationSignatures="true">
```

Spara XML-dokumentet som en XML-fil så att du kan öppna filen och visa resultatet.

**Se även**

[Kontrollera om ett dokument är PDF/A-kompatibelt med Java API](/help/forms/developing/determining-whether-documents-pdf-a.md#determine-whether-a-document-is-pdf-a-compliant-using-the-java-api)

[Kontrollera om ett dokument är PDF/A-kompatibelt med webbtjänstens API](/help/forms/developing/determining-whether-documents-pdf-a.md#determine-whether-a-document-is-pdf-a-compliant-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Programmisk sammanställning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md)

## Kontrollera om ett dokument är PDF/A-kompatibelt med Java API {#determine-whether-a-document-is-pdf-a-compliant-using-the-java-api}

Kontrollera om ett PDF-dokument är PDF/A-kompatibelt med Java (Assembler Service API):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-assembler-client.jar, i Java-projektets klassökväg.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar DDX-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för DDX-filen. Om du vill avgöra om PDF-dokumentet är PDF/A-kompatibelt kontrollerar du att DDX-dokumentet innehåller elementet `PDFAValidation` som finns i ett `DocumentInformation` -element.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Referera till ett PDF-dokument som används för att fastställa om PDF/A är kompatibelt.

   * Skapa ett `java.io.FileInputStream`-objekt med hjälp av dess konstruktor och skicka platsen för ett PDF-dokument som används för att fastställa PDF/A-kompatibilitet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka det `java.io.FileInputStream`-objekt som innehåller PDF-dokumentet.
   * Skapa ett `java.util.Map`-objekt som används för att lagra PDF-indatadokumentet med hjälp av en `HashMap`-konstruktor.
   * Lägg till en post i objektet `java.util.Map` genom att anropa dess `put`-metod och skicka följande argument:

      * Ett strängvärde som representerar nyckelnamnet. Detta värde måste matcha värdet för källelementet som anges i DDX-dokumentet. Värdet för källelementet i DDX-dokumentet som introducerades i det här avsnittet är till exempel Loan.pdf.
      * Ett `com.adobe.idp.Document`-objekt som innehåller PDF-indatadokumentet.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ som uppfyller dina affärskrav genom att anropa en metod som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar, anropar du `AssemblerOptionSpec`-objektets `setFailOnError`-metod och skickar `false`.

1. Hämta information om PDF-dokumentet.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande obligatoriska värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar det DX-dokument som ska användas
   * Ett `java.util.Map`-objekt som innehåller PDF-indatafilen som används för att fastställa PDF/A-kompatibilitet
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen

   Metoden `invokeDDX` returnerar ett `com.adobe.livecycle.assembler.client.AssemblerResult`-objekt som innehåller XML-data som anger om indata-PDF-dokumentet är PDF/A-kompatibelt.

1. Spara det returnerade XML-dokumentet.

   Så här hämtar du XML-data som anger om indata-PDF-dokumentet är ett PDF/A-dokument:

   * Anropa metoden `getDocuments` för objektet `AssemblerResult`. Detta returnerar ett `java.util.Map`-objekt.
   * Upprepa genom objektet `java.util.Map` tills du hittar det resulterande `com.adobe.idp.Document`-objektet.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att extrahera XML-dokumentet. Se till att du sparar XML-data som en XML-fil.

**Se även**

[Snabbstart (SOAP-läge): Avgör om ett dokument är PDF/A-kompatibelt med Java API ](/help/forms/developing/assembler-service-java-api-quick.md#quick-start-soap-mode-determining-whether-a-document-is-pdf-a-compliant-using-the-java-api) (SOAP-läge)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Kontrollera om ett dokument är PDF/A-kompatibelt med webbtjänstens API {#determine-whether-a-document-is-pdf-a-compliant-using-the-web-service-api}

Kontrollera om ett PDF-dokument är PDF/A-kompatibelt med Assembler Service API (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/AssemblerService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `AssemblerServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/AssemblerService?blob=mtom`). Du behöver inte använda attributet `lc_version`. Detta attribut används när du skapar en tjänstreferens.)
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `AssemblerServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `AssemblerServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `AssemblerServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra DDX-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för DDX-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Referera till ett PDF-dokument som används för att fastställa om PDF/A är kompatibelt.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra indata-PDF-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType`-objekt. Det här samlingsobjektet används för att lagra PDF-dokumentet.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt.
   * Tilldela ett strängvärde som representerar nyckelnamnet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `key`-fält. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet.
   * Tilldela objektet `BLOB` som lagrar PDF-dokumentet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `value`-fält.
   * Lägg till objektet `MyMapOf_xsd_string_To_xsd_anyType_Item` i objektet `MyMapOf_xsd_string_To_xsd_anyType`. Anropa `MyMapOf_xsd_string_To_xsd_anyType`-objektets `Add`-metod och skicka `MyMapOf_xsd_string_To_xsd_anyType`-objektet.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ för att uppfylla dina affärskrav genom att tilldela ett värde till en datamedlem som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar tilldelar du `false` till `AssemblerOptionSpec`-objektets `failOnError`-datamedlem.

1. Hämta information om PDF-dokumentet.

   Anropa `AssemblerServiceService`-objektets `invoke`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet.
   * Objektet `MyMapOf_xsd_string_To_xsd_anyType` som innehåller indata-PDF-dokumentet. Dess nycklar måste matcha namnen på PDF källfiler och dess värden måste vara det `BLOB`-objekt som motsvarar PDF indatafilen.
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ.

   Metoden `invoke` returnerar ett `AssemblerResult`-objekt som innehåller XML-data som anger om indata-PDF-dokumentet är ett PDF/A-dokument.

1. Spara det returnerade XML-dokumentet.

   Så här hämtar du XML-data som anger om indata-PDF-dokumentet är ett PDF/A-dokument:

   * Få åtkomst till `documents`-objektets `AssemblerResult`-fält, som är ett `Map`-objekt som innehåller XML-data som anger om indata-PDF-dokumentet är ett PDF/A-dokument.
   * Iterera genom objektet `Map` för att få fram varje resulterande dokument. Sedan konverterar du matrismedlemmens värde till `BLOB`.
   * Extrahera de binära data som representerar XML-data genom att komma åt objektets `MTOM`-fält `BLOB`. I det här fältet lagras en array med byte som du kan skriva till som en XML-fil.

**Se även**

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
