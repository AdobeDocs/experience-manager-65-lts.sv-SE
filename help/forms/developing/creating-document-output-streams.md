---
title: Skapa dokumentutdataströmmar
description: Använd utdatatjänsten för att konvertera dokument till PDF (inklusive PDF/A-dokument), PostScript, Printer Control Language (PCL) och Zebra - ZPL, Intermec - IPL, Datamax - DPL och TecToshiba - TPCL-etikettformat.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
hide: true
hidefromtoc: true
exl-id: a90ccd28-00ae-4317-bfda-c39acbdb835b
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '18860'
ht-degree: 0%

---

# Skapa dokumentutdataströmmar  {#creating-document-output-streams}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

**Om utdatatjänsten**

Med Output Service kan du skriva ut dokument som PDF (inklusive PDF/A-dokument), PostScript, Printer Control Language (PCL) och följande etikettformat:

* Zebra - ZPL
* Intermec - IPL
* Datamax - DPL
* TecToshiba - TPCL

Med hjälp av utdatatjänsten kan du sammanfoga XML-formulärdata med en formulärdesign och skicka dokumentet till en nätverksskrivare eller fil.

Det finns två sätt att skicka en formulärdesign (en XDP-fil) till utdatatjänsten. Du kan skicka en `com.adobe.idp.Document`-instans som innehåller en formulärdesign till utdatatjänsten. Du kan också skicka ett URI-värde som anger platsen för formulärdesignen. Båda dessa sätt beskrivs i *Programmering med AEM-formulär*.

>[!NOTE]
>
>Output-tjänsten stöder inte Acrobat PDF-dokument som innehåller programobjektsspecifika skript. Acrobat PDF-dokument som innehåller programobjektspecifika skript återges inte.

I följande avsnitt visas hur du skickar en formulärdesign till utdatatjänsten med ett URI-värde:

* [Skapa PDF-dokument](creating-document-output-streams.md#creating-pdf-documents)
* [Skapa PDF/A-dokument](creating-document-output-streams.md#creating-pdf-a-documents)

I följande avsnitt visas hur du skickar en formulärdesign inom en `com.adobe.idp.Document`-instans:

* [Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten](creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service)
* [Skapa PDF-dokument med fragment](creating-document-output-streams.md#creating-pdf-documents-using-fragments)

När du bestämmer vilken teknik du ska använda är det viktigt att du hämtar formulärdesignen från en annan AEM Forms-tjänst och skickar den inom en `com.adobe.idp.Document`-instans. I avsnitten *Skicka dokument till utdatatjänsten* och *Skapa PDF-dokument med fragment* visas hur du hämtar en formulärdesign från en annan AEM Forms-tjänst. Det första avsnittet hämtar formulärdesignen från innehållstjänster (borttagen). Det andra avsnittet hämtar formulärdesignen från Assembler-tjänsten.

Om du hämtar formulärdesignen från en fast plats, t.ex. i filsystemet, kan du använda vilken teknik som helst. Det innebär att du kan ange URI-värdet till en XDP-fil eller använda en `com.adobe.idp.Document`-instans.

Om du vill skicka ett URI-värde som anger platsen för formulärdesignen när du skapar ett PDF-dokument använder du metoden `generatePDFOutput`. Om du vill skicka en `com.adobe.idp.Document`-instans till utdatatjänsten när du skapar ett PDF-dokument använder du metoden `generatePDFOutput2`.

När du skickar en utdataström till en nätverksskrivare kan du också använda båda teknikerna. Om du vill skicka en utdataström till en skrivare genom att skicka en `com.adobe.idp.Document`-instans som innehåller en formulärdesign använder du metoden `sendToPrinter2`. Om du vill skicka en utdataström till en skrivare genom att skicka ett URI-värde använder du metoden `sendToPrinter`. Avsnittet *Skicka utskriftsströmmar till skrivare* använder metoden `sendToPrinter`.

Du kan utföra följande uppgifter med hjälp av utdatatjänsten:

* [Skapa PDF-dokument](creating-document-output-streams.md#creating-pdf-documents)
* [Skapa PDF/A-dokument](creating-document-output-streams.md#creating-pdf-a-documents)
* [Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten](creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service)
* [Skapa PDF-dokument med fragment](creating-document-output-streams.md#creating-pdf-documents-using-fragments)
* [Skriva ut till filer](creating-document-output-streams.md#printing-to-files)
* [Skicka utskriftsströmmar till skrivare](creating-document-output-streams.md#sending-print-streams-to-printers)
* [Skapa flera utdatafiler](creating-document-output-streams.md#creating-multiple-output-files)
* [Skapa sökregler](creating-document-output-streams.md#creating-search-rules)
* [Förenkla PDF-dokument](creating-document-output-streams.md#flattening-pdf-documents)

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Skapa PDF-dokument {#creating-pdf-documents}

Du kan använda utdatatjänsten för att skapa ett PDF-dokument som är baserat på en formulärdesign och XML-formulärdata som du anger. PDF-dokumentet som skapas av Output-tjänsten är inte ett interaktivt PDF-dokument. Användaren kan inte ange eller ändra formulärdata.

Om du vill skapa ett PDF-dokument som är avsett för långsiktig lagring rekommenderar vi att du skapar ett PDF/A-dokument. (Se [Skapa PDF/A-dokument](creating-document-output-streams.md#creating-pdf-a-documents).)

Använd tjänsten Forms för att skapa ett interaktivt PDF-formulär där användaren kan ange data. (Se [Återge interaktiv PDF forms](/help/forms/developing/rendering-forms.md#rendering-interactive-pdf-forms).)

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Så här skapar du ett PDF-dokument:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Referera till en XML-datakälla.
1. Ange körningsalternativ för PDF.
1. Ange alternativ för återgivning vid körning.
1. Skapa ett PDF-dokument.
1. Hämta resultatet av åtgärden.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

Följande JAR-filer måste läggas till i projektets klassökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms distribueras på JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på.

**Skapa ett Output Client-objekt**

Innan du programmässigt kan utföra en utdatatjänståtgärd måste du skapa ett klientobjekt för utdatatjänsten. Om du använder Java API skapar du ett `OutputClient`-objekt. Skapa ett `OutputServiceService`-objekt om du använder API:t för webbtjänsten för utdata.

**Referera till en XML-datakälla**

Om du vill sammanfoga data med formulärdesignen måste du referera till en XML-datakälla som innehåller data. Det måste finnas ett XML-element för varje formulärfält som du vill fylla i med data. XML-elementnamnet måste matcha fältnamnet. Ett XML-element ignoreras om det inte motsvarar ett formulärfält eller om XML-elementnamnet inte matchar fältnamnet. Det är inte nödvändigt att matcha den ordning i vilken XML-elementen visas om alla XML-element har angetts.

Titta på följande exempelformulär för låneansökan.

![cp_cp_loanformdata](assets/cp_cp_loanformdata.png)

Om du vill sammanfoga data i den här formulärdesignen måste du skapa en XML-datakälla som motsvarar formuläret. Följande XML representerar en XDP XML-datakälla som motsvarar exempelformuläret för låneansökan.

```xml
 <?xml version="1.0" encoding="UTF-8" ?>
 - <xfa:datasets xmlns:xfa="https://www.xfa.org/schema/xfa-data/1.0/">
 - <xfa:data>
 - <data>
     - <Layer>
         <closeDate>1/26/2007</closeDate>
         <lastName>Johnson</lastName>
         <firstName>Jerry</firstName>
         <mailingAddress>JJohnson@NoMailServer.com</mailingAddress>
         <city>New York</city>
         <zipCode>00501</zipCode>
         <state>NY</state>
         <dateBirth>26/08/1973</dateBirth>
         <middleInitials>D</middleInitials>
         <socialSecurityNumber>(555) 555-5555</socialSecurityNumber>
         <phoneNumber>5555550000</phoneNumber>
     </Layer>
     - <Mortgage>
         <mortgageAmount>295000.00</mortgageAmount>
         <monthlyMortgagePayment>1724.54</monthlyMortgagePayment>
         <purchasePrice>300000</purchasePrice>
         <downPayment>5000</downPayment>
         <term>25</term>
         <interestRate>5.00</interestRate>
     </Mortgage>
 </data>
 </xfa:data>
 </xfa:datasets>
```

**Ange körningsalternativ för PDF**

Ange filens URI-alternativ när du skapar ett PDF-dokument. Det här alternativet anger namn och plats för den PDF-fil som utdatatjänsten genererar.

>[!NOTE]
>
>I stället för att ställa in körningsalternativet för fil-URI kan du hämta PDF-dokumentet programmatiskt från den komplexa datatyp som returneras av Output-tjänsten. Genom att ställa in körningsalternativet fil-URI behöver du emellertid inte skapa programlogik som hämtar PDF-dokumentet programmatiskt.

**Ange alternativ för återgivning vid körning**

Du kan ange alternativ för återgivning vid körning när du skapar ett PDF-dokument. Även om dessa alternativ inte är nödvändiga (till skillnad från PDF körningsalternativ som krävs) kan du utföra åtgärder som att förbättra prestanda för utdatatjänsten. Du kan till exempel cachelagra den formulärdesign som Output-tjänsten använder för att förbättra dess prestanda.

Om du använder ett taggat Acrobat-formulär som indata kan du inte använda Java- eller webbtjänstens API för utdatatjänsten för att inaktivera den taggade inställningen. Om du försöker ange det här alternativet till `false` programmatiskt är resultatet i PDF-dokumentet fortfarande taggat.

>[!NOTE]
>
>Om du inte anger alternativ för återgivning vid körning används standardvärden. Mer information om alternativ för återgivning vid körning finns i klassreferensen `RenderOptionsSpec`. (Se [API-referens för AEM Forms](https://www.adobe.com/go/learn_aemforms_javadocs_63_en)).

**Skapa ett PDF-dokument**

När du har refererat till en giltig XML-datakälla som innehåller formulärdata och angett körningsalternativ, kan du anropa utdatatjänsten, vilket resulterar i att ett PDF-dokument skapas.

När du genererar ett PDF-dokument anger du de URI-värden som krävs av utdatatjänsten för att skapa ett PDF-dokument. En formulärdesign kan lagras på platser som serverfilsystemet eller som en del av ett AEM Forms-program. En formulärdesign (eller andra resurser som en bildfil) som finns som en del av ett Forms-program kan refereras med hjälp av URI-värdet `repository:///` för innehållsroten. Ta till exempel följande formulärdesign *Loan.xdp* som finns i ett Forms-program med namnet *Applications/FormsApplication*:

![cp_cp_formdatabase](assets/cp_cp_formrepository.png)

Om du vill komma åt filen Loan.xdp som visas på föregående bild anger du `repository:///Applications/FormsApplication/1.0/FormsFolder/` som den tredje parametern som skickas till `OutputClient`-objektets `generatePDFOutput` -metod. Ange formulärnamnet (*Loan.xdp*) som den andra parametern som skickas till `OutputClient`-objektets `generatePDFOutput`-metod.

Om XDP-filen innehåller bilder (eller andra resurser som fragment) placerar du resurserna i samma programmapp som XDP-filen. AEM Forms använder innehållets rot-URI som grundsökväg för att lösa referenser till bilder. Om filen Loan.xdp till exempel innehåller en bild måste du montera bilden i `Applications/FormsApplication/1.0/FormsFolder/`.

>[!NOTE]
>
>Du kan referera till en Forms-program-URI när du anropar `OutputClient`-objektets `generatePDFOutput`- eller `generatePrintedOutput`-metoder.

>[!NOTE]
>
>En fullständig snabbstart som skapar ett PDF-dokument genom att referera till en XDP-fil i ett Forms-program finns i [Snabbstart (EJB-läge): Skapa ett PDF-dokument baserat på en program-XDP-fil med Java API ](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-based-on-an-application-xdp-file-using-the-java-api) .

**Hämta resultatet av åtgärden**

När utdatatjänsten har utfört en åtgärd returneras olika dataobjekt, t.ex. status-XML-data, som anger om åtgärden lyckades.

**Se även**

[Skapa ett PDF-dokument med Java API](creating-document-output-streams.md#create-a-pdf-document-using-the-java-api)

[Skapa ett PDF-dokument med hjälp av webbtjänstens API](creating-document-output-streams.md#create-a-pdf-document-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

### Skapa ett PDF-dokument med Java API {#create-a-pdf-document-using-the-java-api}

Skapa ett PDF-dokument med hjälp av utdata-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till en XML-datakälla.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar XML-datakällan som används för att fylla i PDF-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor. Skicka objektet `java.io.FileInputStream`.

1. Ange körningsalternativ för PDF.

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange alternativet Fil-URI genom att anropa `PDFOutputOptionsSpec`-objektets `setFileURI`-metod. Skicka ett strängvärde som anger platsen för den PDF-fil som utdatatjänsten genererar. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.

1. Ange alternativ för återgivning vid körning.

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Cachelagra formulärdesignen för att förbättra utdatatjänstens prestanda genom att anropa `setCacheEnabled` för `RenderOptionsSpec`-objektet och skicka `true`.

   >[!NOTE]
   >
   >Du kan inte ange version av PDF-dokumentet med `RenderOptionsSpec`-objektets `setPdfVersion`-metod om indatadokumentet är ett Acrobat-formulär (ett formulär som har skapats i Acrobat) eller ett XFA-dokument som är signerat eller certifierat. Utdata-PDF-dokumentet behåller den ursprungliga PDF-versionen. På samma sätt kan du inte ange alternativet taggad Adobe PDF genom att anropa `RenderOptionsSpec`-objektets `setTaggedPDF`-metod om indatadokumentet är ett Acrobat-formulär eller ett signerat eller certifierat XFA-dokument.

   >[!NOTE]
   >
   >Du kan inte ange linjärt PDF-alternativ med `RenderOptionsSpec`-objektets `setLinearizedPDF`-metod om PDF-indatadokumentet är certifierat eller digitalt signerat. (Se [Signera PDF-dokument digitalt ](/help/forms/developing/digitally-signing-certifying-documents.md#digitally-signing-pdf-documents)*.)*

1. Skapa ett PDF-dokument.

   Skapa ett PDF-dokument genom att anropa `OutputClient`-objektets `generatePDFOutput`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.

   Metoden `generatePDFOutput` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

   >[!NOTE]
   >
   >När du genererar ett PDF-dokument genom att anropa metoden `generatePDFOutput` kan du inte sammanfoga data med ett XFA PDF-formulär som är signerat eller certifierat. (Se [Signera och certifiera dokument digitalt ](/help/forms/developing/digitally-signing-certifying-documents.md#digitally-signing-and-certifying-documents)*.)*

   >[!NOTE]
   >
   >`OutputResult`-objektets `getRecordLevelMetaDataList`-metod returnerar `null`*.*

   >[!NOTE]
   >
   >Du kan också skapa ett PDF-dokument genom att anropa `OutputClient`-objektets `generatePDFOutput2`-metod. (Se [Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten ](creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service)*.)*

1. Hämta resultatet av åtgärden.

   * Hämta ett `com.adobe.idp.Document`-objekt som representerar statusen för `generatePDFOutput`-åtgärden genom att anropa `OutputResult`-objektets `getStatusDoc`-metod. Den här metoden returnerar status-XML-data som anger om åtgärden lyckades.
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filnamnstillägget är .xml.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `getStatusDoc`).

   Även om utdatatjänsten skriver PDF-dokumentet till den plats som anges av argumentet som skickas till `PDFOutputOptionsSpec`-objektets `setFileURI` -metod, kan du hämta PDF/A-dokumentet genom att anropa `OutputResult`-objektets `getGeneratedDoc` -metod.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): Skapa ett PDF-dokument med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa ett PDF-dokument med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Skapa ett PDF-dokument med hjälp av webbtjänstens API {#create-a-pdf-document-using-the-web-service-api}

Skapa ett PDF-dokument med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till en XML-datakälla.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra XML-data som ska sammanfogas med PDF-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för XML-filen som innehåller formulärdata.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Ange körningsalternativ för PDF

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange alternativet Fil-URI genom att tilldela ett strängvärde som anger platsen för den PDF-fil som utdatatjänsten genererar till `fileURI`-objektets `PDFOutputOptionsSpec`-datamedlem. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.

1. Ange alternativ för återgivning vid körning.

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Cachelagra formulärdesignen för att förbättra utdatatjänstens prestanda genom att tilldela värdet `true` till `RenderOptionsSpec`-objektets `cacheEnabled`-datamedlem.

   >[!NOTE]
   >
   >Du kan inte ange version av PDF-dokumentet med `RenderOptionsSpec`-objektets `setPdfVersion`-metod om indatadokumentet är ett Acrobat-formulär (ett formulär som har skapats i Acrobat) eller ett XFA-dokument som är signerat eller certifierat. Utdata-PDF-dokumentet behåller den ursprungliga PDF-versionen. Du kan inte heller ange alternativet taggad Adobe PDF genom att anropa `RenderOptionsSpec`-objektets `setTaggedPDF`*-metod om indatadokumentet är ett Acrobat-formulär eller ett signerat eller certifierat XFA-dokument.*

   >[!NOTE]
   >
   >Du kan inte ange linjärt PDF-alternativ med `RenderOptionsSpec`-objektets `linearizedPDF`-medlem om PDF-indatadokumentet är certifierat eller digitalt signerat. (Se [Signera PDF-dokument digitalt ](/help/forms/developing/digitally-signing-certifying-documents.md#digitally-signing-pdf-documents)*.)*

1. Skapa ett PDF-dokument.

   Skapa ett PDF-dokument genom att anropa `OutputServiceService`-objektets `generatePDFOutput`metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs bara för webbtjänstanrop).
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med resultatdata. (Det här parametervärdet krävs bara för webbtjänstanrop).
   * Ett `OutputResult`-objekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs bara för webbtjänstanrop).

   >[!NOTE]
   >
   >När du genererar ett PDF-dokument genom att anropa metoden `generatePDFOutput` kan du inte sammanfoga data med ett XFA PDF-formulär som är signerat eller certifierat. (Se [Signera och certifiera dokument digitalt ](/help/forms/developing/digitally-signing-certifying-documents.md#digitally-signing-and-certifying-documents)*.)*

   >[!NOTE]
   >
   >Du kan också skapa ett PDF-dokument genom att anropa `OutputClient`-objektets `generatePDFOutput2`-metod. (Se [Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten ](creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service)*.)*

1. Hämta resultatet av åtgärden.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar en plats för en XML-fil som innehåller resultatdata. Kontrollera att filnamnstillägget är .xml.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som fylldes med resultatdata av metoden `generatePDFOutput` för objektet `OutputServiceService` (den åttonde parametern). Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM` `field`.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till XML-filen genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

   Se även

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

   >[!NOTE]
   >
   >`OutputServiceService`-objektets `generateOutput`-metod är inaktuell.

## Skapa PDF/A-dokument {#creating-pdf-a-documents}

Du kan använda utdatatjänsten för att skapa ett PDF/A-dokument. Eftersom PDF/A är ett arkiveringsformat för långvarig lagring av dokumentets innehåll, bäddas alla teckensnitt in och filen är okomprimerad. Därför är ett PDF/A-dokument vanligtvis större än ett vanligt PDF-dokument. Dessutom innehåller ett PDF/A-dokument inte ljud- och videoinnehåll. Precis som med andra Output Service-uppgifter tillhandahåller du både en formulärdesign och data som ska sammanfogas med en formulärdesign för att skapa ett PDF/A-dokument.

Specifikationen PDF/A-1 består av två överensstämmelsenivåer, nämligen a och b. Den största skillnaden mellan de två är stöd för logisk struktur (hjälpmedel), som inte krävs för överensstämmelsenivå b. Oavsett överensstämmelsenivå anger PDF/A-1 att alla teckensnitt är inbäddade i det genererade PDF/A-dokumentet.

Även om PDF/A är standarden för arkivering av PDF-dokument är det inte obligatoriskt att använda PDF/A för arkivering om ett PDF-standarddokument uppfyller företagets behov. Syftet med PDF/A-standarden är att upprätta en PDF-fil som kan lagras under lång tid och som uppfyller kraven på dokumentarkivering. En URL kan till exempel inte bäddas in i en PDF/A eftersom URL:en kan bli ogiltig över tiden.

Organisationen måste bedöma sina egna behov, hur lång tid du tänker behålla dokumentet, ta hänsyn till filstorlek och fastställa en egen arkiveringsstrategi. Med tjänsten DocConverter kan du programmässigt avgöra om ett PDF-dokument är PDF/A-kompatibelt. (Se [Programmatiskt fastställa PDF/A-kompatibilitet](/help/forms/developing/pdf-a-documents.md#programmatically-determining-pdf-a-compliancy).)

Ett PDF/A-dokument måste ha det teckensnitt som är angivet i formulärdesignen och teckensnitt kan inte ersättas. Om ett teckensnitt som finns i ett PDF-dokument inte är tillgängligt på operativsystemet (OS) inträffar därför ett undantag.

När ett PDF/A-dokument öppnas i Acrobat visas ett meddelande som bekräftar att dokumentet är ett PDF/A-dokument, vilket visas på följande bild.

![cp_cp_pdframe](assets/cp_cp_pdfamessage.png)

>[!NOTE]
>
>AIIM-webbplatsen har ett avsnitt med vanliga frågor om PDF/A som du kan få åtkomst till på [https://www.loc.gov/preservation/digital/formats/fdd/fdd000125.shtml](https://www.loc.gov/preservation/digital/formats/fdd/fdd000125.shtml).

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_65).

### Sammanfattning av steg {#summary_of_steps-1}

Så här skapar du ett PDF/A-dokument:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Referera till en XML-datakälla.
1. Ange körningsalternativ för PDF/A.
1. Ange alternativ för återgivning vid körning.
1. Skapa ett PDF/A-dokument.
1. Hämta resultatet av åtgärden.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett anpassat program med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms distribueras på JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på.

**Skapa ett Output Client-objekt**

Innan du programmässigt kan utföra en utdatatjänståtgärd måste du skapa ett klientobjekt för utdatatjänsten. Om du använder Java API skapar du ett `OutputClient`-objekt. Skapa ett `OutputServiceService`-objekt om du använder API:t för webbtjänsten för utdata.

**Referera till en XML-datakälla**

Om du vill sammanfoga data med formulärdesignen måste du referera till en XML-datakälla som innehåller data. Det måste finnas ett XML-element för varje formulärfält som du vill fylla i med data. XML-elementnamnet måste matcha fältnamnet. Ett XML-element ignoreras om det inte motsvarar ett formulärfält eller om XML-elementnamnet inte matchar fältnamnet. Det är inte nödvändigt att matcha den ordning i vilken XML-elementen visas om alla XML-element har angetts.

**Ange körningsalternativ för PDF/A**

Du kan ange alternativet Fil-URI när du skapar ett PDF/A-dokument. URI:n är relativ till J2EE-programservern där AEM Forms finns. Det innebär att om du anger C:\Adobe skrivs filen till mappen på servern, inte till klientdatorn. URI:n anger namn och plats för den PDF/A-fil som utdatatjänsten genererar.

**Ange alternativ för återgivning vid körning**

Du kan ange alternativ för återgivning vid körning när du skapar PDF/A-dokument. Två PDF/A-relaterade alternativ som du kan ange är värdena `PDFAConformance` och `PDFARevisionNumber`. Värdet `PDFAConformance` refererar till hur ett PDF-dokument uppfyller krav som anger hur långfristiga elektroniska dokument bevaras. Giltiga värden för det här alternativet är `A` och `B`. Mer information om överensstämmelse för nivå a och b finns i ISO-specifikationen PDF/A-1 som heter *ISO 19005-1 Document Management*.

Värdet `PDFARevisionNumber` refererar till revisionsnumret för ett PDF/A-dokument. Information om revisionsnumret för ett PDF/A-dokument finns i ISO-specifikationen PDF/A-1 som heter *ISO 19005-1 Document Management*.

>[!NOTE]
>
>Du kan inte ange alternativet för taggad Adobe PDF till `false` när du skapar ett PDF/A 1A-dokument. PDF/A 1A är alltid ett taggat PDF-dokument. Du kan inte heller ställa in alternativet för taggad Adobe PDF på `true` när du skapar ett PDF/A 1B-dokument. PDF/A 1B är alltid ett otaggat PDF-dokument.

**Skapa ett PDF/A-dokument**

När du har refererat till en giltig XML-datakälla som innehåller formulärdata och angett körningsalternativ, kan du anropa utdatatjänsten och få den att generera ett PDF/A-dokument.

**Hämta resultatet av åtgärden**

När utdatatjänsten har utfört en åtgärd returneras olika dataobjekt, till exempel XML-data, som anger om åtgärden lyckades.

**Se även**

[Skapa ett PDF/A-dokument med Java API](creating-document-output-streams.md#create-a-pdf-a-document-using-the-java-api)

[Skapa ett PDF/A-dokument med webbtjänstens API](creating-document-output-streams.md#create-a-pdf-a-document-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

### Skapa ett PDF/A-dokument med Java API {#create-a-pdf-a-document-using-the-java-api}

Skapa ett PDF/A-dokument med hjälp av utdata-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till en XML-datakälla.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar XML-datakällan som används för att fylla i PDF/A-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange körningsalternativ för PDF/A.

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange alternativet Fil-URI genom att anropa `PDFOutputOptionsSpec`-objektets `setFileURI`-metod. Skicka ett strängvärde som anger platsen för den PDF-fil som utdatatjänsten genererar. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.

1. Ange alternativ för återgivning vid körning.

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange värdet `PDFAConformance` genom att anropa `RenderOptionsSpec`-objektets `setPDFAConformance`-metod och skicka ett `PDFAConformance` enum-värde som anger överensstämmelsenivån. Om du till exempel vill ange överensstämmelsenivå A skickar du `PDFAConformance.A`.
   * Ange värdet `PDFARevisionNumber` genom att anropa `RenderOptionsSpec`-objektets `setPDFARevisionNumber`-metod och skicka `PDFARevisionNumber.Revision_1`.

   >[!NOTE]
   >
   >PDF-versionen av ett PDF/A-dokument är 1.4 oavsett vilket värde du anger för `RenderOptionsSpec`-objektets `setPdfVersion`*metod.*

1. Skapa ett PDF/A-dokument.

   Skapa ett PDF/A-dokument genom att anropa `OutputClient`-objektets `generatePDFOutput`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF/A-dokument anger du `TransformationFormat.PDFA`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.

   Metoden `generatePDFOutput` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

   >[!NOTE]
   >
   >`OutputResult`-objektets `getRecordLevelMetaDataList`-metod returnerar `null`.

   >[!NOTE]
   >
   >Du kan också skapa ett PDF/A-dokument genom att anropa `OutputClient`-objektets `generatePDFOutput` 2-metod. (Se [Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten](creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service).)

1. Hämta resultatet av åtgärden.

   * Skapa ett `com.adobe.idp.Document`-objekt som representerar `generatePDFOutput`-metodens status genom att anropa `OutputResult`-objektets `getStatusDoc`-metod.
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filnamnstillägget är .xml.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `getStatusDoc`).

   >[!NOTE]
   >
   >Även om utdatatjänsten skriver PDF/A-dokumentet till den plats som anges av argumentet som skickas till `PDFOutputOptionsSpec`-objektets `setFileURI` -metod, kan du hämta PDF/A-dokumentet genom att anropa `OutputResult`-objektets `getGeneratedDoc` -metod.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (SOAP-läge): Skapa ett PDF/A-dokument med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-a-document-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Anger anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

### Skapa ett PDF/A-dokument med webbtjänstens API {#create-a-pdf-a-document-using-the-web-service-api}

Skapa ett PDF/A-dokument med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till en XML-datakälla.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra data som ska sammanfogas med PDF/A-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det PDF-dokument som ska krypteras och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med bytearrayinnehållet.

1. Ange körningsalternativ för PDF/A.

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange alternativet Fil-URI genom att tilldela ett strängvärde som anger platsen för den PDF-fil som utdatatjänsten genererar till `fileURI`-objektets `PDFOutputOptionsSpec`-datamedlem. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn

1. Ange alternativ för återgivning vid körning.

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange värdet `PDFAConformance` genom att tilldela ett `PDFAConformance` enum-värde till `RenderOptionsSpec`-objektets `PDFAConformance`-datamedlem. Om du till exempel vill ange överensstämmelsenivå A tilldelar du `PDFAConformance.A` till den här datamedlemmen.
   * Ange värdet `PDFARevisionNumber` genom att tilldela ett `PDFARevisionNumber` enum-värde till `RenderOptionsSpec`-objektets `PDFARevisionNumber`-datamedlem. Tilldela `PDFARevisionNumber.Revision_1` till den här datamedlemmen.

   >[!NOTE]
   >
   >PDF-versionen av ett PDF/A-dokument är 1.4 oavsett vilket värde du anger.

1. Skapa ett PDF/A-dokument.

   Skapa ett PDF-dokument genom att anropa `OutputServiceService`-objektets `generatePDFOutput`metod och skicka följande värden:

   * Ett TransformationFormat-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDFA`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs endast för webbtjänstanrop.)
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med resultatdata. (Det här parametervärdet krävs endast för webbtjänstanrop.)
   * Ett `OutputResult`-objekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs endast för webbtjänstanrop.)

   >[!NOTE]
   >
   >Du kan också skapa ett PDF/A-dokument genom att anropa `OutputClient`-objektets `generatePDFOutput` 2-metod. (Se [Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten](creating-document-output-streams.md#passing-documents-located-in-content-services-deprecated-to-the-output-service).)

1. Hämta resultatet av åtgärden.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar en plats för en XML-fil som innehåller resultatdata. Kontrollera att filnamnstillägget är .xml.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som fylldes med resultatdata av metoden `generatePDFOutput` för objektet `OutputServiceService` (den åttonde parametern). Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-fält.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till XML-filen genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Skicka dokument i innehållstjänster (borttaget) till utdatatjänsten {#passing-documents-located-in-content-services-deprecated-to-the-output-service}

Output-tjänsten återger ett icke-interaktivt PDF-formulär som är baserat på en formulärdesign som vanligtvis sparas som en XDP-fil och skapas i Designer. Du kan skicka ett `com.adobe.idp.Document`-objekt som innehåller formulärdesignen till utdatatjänsten. Utdatatjänsten återger sedan formulärdesignen i objektet `com.adobe.idp.Document`.

En fördel med att skicka ett `com.adobe.idp.Document`-objekt till utdatatjänsten är att andra AEM Forms-tjänståtgärder returnerar en `com.adobe.idp.Document`-instans. Det innebär att du kan hämta en `com.adobe.idp.Document`-instans från en annan tjänståtgärd och återge den. Anta till exempel att en XDP-fil lagras i en Content Services-nod (utgått) med namnet `/Company Home/Form Designs`, vilket visas på följande bild.

Du kan hämta Loan.xdp via programkod från Content Services (utgått) och skicka XDP-filen till Output-tjänsten i ett `com.adobe.idp.Document`-objekt.

>[!NOTE]
>
>Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-2}

Så här skickar du ett dokument som hämtats från innehållstjänster (borttaget) till utdatatjänsten:

1. Inkludera projektfiler.
1. Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten.
1. Hämta formulärdesignen från Content Services (utgått).
1. Återge det icke-interaktiva PDF-formuläret.
1. Utför en åtgärd med dataströmmen.

**Inkludera projektfiler**

Inkludera de filer som behövs i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster inkluderar du proxyfilerna.

**Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten**

Innan du programmässigt kan utföra en API-åtgärd för en utdatatjänst skapar du ett API-objekt för utdataklienten. Eftersom det här arbetsflödet hämtar en XDP-fil från Content Services (utgått) skapar du också ett API-objekt för dokumenthantering.

**Hämta formulärdesignen från Content Services (utgått)**

Hämta XDP-filen från Content Services (utgått) med Java- eller webbtjänstens API. XDP-filen returneras i en `com.adobe.idp.Document`-instans (eller en `BLOB`-instans om du använder webbtjänster). Du kan sedan skicka instansen `com.adobe.idp.Document` till utdatatjänsten.

**Återge det icke-interaktiva PDF-formuläret**

Om du vill återge ett icke-interaktivt formulär skickar du `com.adobe.idp.Document`-instansen som returnerades från Content Services (utgått) till utdatatjänsten.

>[!NOTE]
>
>Två nya metoder med namnen `generatePDFOutput2` och g `eneratePrintedOutput2`accepterar ett `com.adobe.idp.Document`-objekt som innehåller en formulärdesign. Du kan också skicka en `com.adobe.idp.Document` som innehåller formulärdesignen till utdatatjänsten när du skickar en utskriftsström till en nätverksskrivare.

**Utför en åtgärd med formulärdataströmmen**

Du kan spara det icke-interaktiva formuläret som en PDF-fil. Formuläret kan visas i Adobe Reader eller Acrobat.

**Se även**

[Skicka dokument till utdatatjänsten med Java API](creating-document-output-streams.md#pass-documents-to-the-output-service-using-the-java-api)

[Skicka dokument till utdatatjänsten med hjälp av webbtjänstens API](creating-document-output-streams.md#pass-documents-to-the-output-service-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

[Skapa PDF-dokument med fragment](creating-document-output-streams.md#creating-pdf-documents-using-fragments)

### Skicka dokument till utdatatjänsten med Java API {#pass-documents-to-the-output-service-using-the-java-api}

Skicka ett dokument som hämtats från Content Services (utgått) med hjälp av Output Service och Content Services (utgått) API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar och adobe-contentservices-client.jar, i Java-projektets klassökväg.

1. Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper. (Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).)
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.
   * Skapa ett `DocumentManagementServiceClientImpl`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Hämta formulärdesignen från Content Services (utgått).

   Anropa `DocumentManagementServiceClientImpl`-objektets `retrieveContent`-metod och skicka följande värden:

   * Ett strängvärde som anger den lagringsplats där innehållet läggs till. Standardarkivet är `SpacesStore`. Detta värde är en obligatorisk parameter.
   * Ett strängvärde som anger den fullständigt kvalificerade sökvägen för innehållet som ska hämtas (till exempel `/Company Home/Form Designs/Loan.xdp`). Detta värde är en obligatorisk parameter.
   * Ett strängvärde som anger versionen. Det här värdet är en valfri parameter och du kan skicka en tom sträng. I det här fallet hämtas den senaste versionen.

   Metoden `retrieveContent` returnerar ett `CRCResult`-objekt som innehåller XDP-filen. Hämta en `com.adobe.idp.Document`-instans genom att anropa `CRCResult`-objektets `getDocument`-metod.

1. Återge det icke-interaktiva PDF-formuläret.

   Anropa `OutputClient`-objektets `generatePDFOutput2`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger innehållsroten där de ytterligare resurserna, t.ex. bilderna, finns.
   * Ett `com.adobe.idp.Document`-objekt som representerar formulärdesignen (använd instansen som returneras av `CRCResult`-objektets `getDocument` -metod).
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.

   Metoden `generatePDFOutput2` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

1. Utför en åtgärd med formulärdataströmmen.

   * Hämta ett `com.adobe.idp.Document`-objekt som representerar det icke-interaktiva formuläret genom att anropa `OutputResult`-objektets `getGeneratedDoc`-metod.
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filnamnstillägget är .pdf.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `getGeneratedDoc`).

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): skicka dokument till utdatatjänsten med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-passing-documents-to-the-output-service-using-the-java-api)

[Snabbstart (SOAP-läge): skicka dokument till utdatatjänsten med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-passing-documents-to-the-output-service-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Skicka dokument till utdatatjänsten med hjälp av webbtjänstens API {#pass-documents-to-the-output-service-using-the-web-service-api}

Skicka ett dokument som hämtats från innehållstjänster (borttaget) med hjälp av utdatatjänsten och innehållstjänstens (utgått) API (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Eftersom klientprogrammet anropar två AEM Forms-tjänster skapar du två tjänstreferenser. Använd följande WSDL-definition för den tjänstreferens som är associerad med utdatatjänsten: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   Använd följande WSDL-definition för den tjänstreferens som är associerad med dokumenthanteringstjänsten: `http://localhost:8080/soap/services/DocumentManagementService?WSDL&lc_version=9.0.1`.

   Eftersom datatypen `BLOB` är gemensam för båda tjänstreferenserna kan du kvalificera datatypen `BLOB` fullständigt när du använder den. I motsvarande snabbstart för webbtjänsten är alla `BLOB` instanser kvalificerade.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`). Du behöver inte använda attributet `lc_version`. Detta attribut används när du skapar en tjänstreferens.)
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.

   * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

   >[!NOTE]
   >
   >Upprepa dessa steg för tjänstklienten `DocumentManagementServiceClient`.

1. Hämta formulärdesignen från Content Services (utgått).

   Hämta innehåll genom att anropa `DocumentManagementServiceClient`-objektets `retrieveContent`-metod och skicka följande värden:

   * Ett strängvärde som anger den lagringsplats där innehållet läggs till. Standardarkivet är `SpacesStore`. Detta värde är en obligatorisk parameter.
   * Ett strängvärde som anger den fullständigt kvalificerade sökvägen för innehållet som ska hämtas (till exempel `/Company Home/Form Designs/Loan.xdp`). Detta värde är en obligatorisk parameter.
   * Ett strängvärde som anger versionen. Det här värdet är en valfri parameter och du kan skicka en tom sträng. I det här fallet hämtas den senaste versionen.
   * En strängutdataparameter som lagrar värdet för bläddringslänken.
   * En `BLOB`-utdataparameter som lagrar innehållet. Du kan använda den här utdataparametern för att hämta innehållet.
   * En `ServiceReference1.MyMapOf_xsd_string_To_xsd_anyType`-utdataparameter som lagrar innehållsattribut.
   * En `CRCResult`-utdataparameter. I stället för att använda det här objektet kan du använda utdataparametern `BLOB` för att hämta innehållet.

1. Återge det icke-interaktiva PDF-formuläret.

   Anropa `OutputServiceClient`-objektets `generatePDFOutput2`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger innehållsroten där de ytterligare resurserna, t.ex. bilderna, finns.
   * Ett `BLOB`-objekt som representerar formulärdesignen (använd den `BLOB`-instans som returneras av Content Services (utgått)).
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.
   * Ett utdataobjekt, `BLOB`, som fylls i av metoden `generatePDFOutput2`. Metoden `generatePDFOutput2` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs bara för webbtjänstanrop).
   * Ett `OutputResult`-utdataobjekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs bara för webbtjänstanrop).

   Metoden `generatePDFOutput2` returnerar ett `BLOB`-objekt som innehåller det icke-interaktiva PDF-formuläret.

1. Utför en åtgärd med formulärdataströmmen.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för det interaktiva PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `BLOB` som hämtats från metoden `generatePDFOutput2`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till en PDF-fil genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

## Skicka dokument i databasen till utdatatjänsten {#passing-documents-located-in-the-repository-to-the-output-service}

Output-tjänsten återger ett icke-interaktivt PDF-formulär som är baserat på en formulärdesign som vanligtvis sparas som en XDP-fil och skapas i Designer. Du kan skicka ett `com.adobe.idp.Document`-objekt som innehåller formulärdesignen till utdatatjänsten. Utdatatjänsten återger sedan formulärdesignen i objektet `com.adobe.idp.Document`.

En fördel med att skicka ett `com.adobe.idp.Document`-objekt till utdatatjänsten är att andra AEM Forms-tjänståtgärder returnerar en `com.adobe.idp.Document`-instans. Det innebär att du kan hämta en `com.adobe.idp.Document`-instans från en annan tjänståtgärd och återge den. Anta till exempel att en XDP-fil lagras i AEM Forms-databasen, vilket visas i följande bild.

![pd_pd_formdatabas](assets/pd_pd_formrepository.png)

Mappen *FormsFolder* är en användardefinierad plats i AEM Forms-databasen (den här platsen är ett exempel och finns inte som standard). I det här exemplet finns en formulärdesign med namnet Loan.xdp i den här mappen. Förutom formulärdesignen kan andra formulärdata, t.ex. bilder, lagras på den här platsen. Sökvägen till en resurs i AEM Forms-databasen:

`Applications/Application-name/Application-version/Folder.../Filename`

Du kan hämta Loan.xdp programmatiskt från AEM Forms-databasen och skicka det till utdatatjänsten inom ett `com.adobe.idp.Document`-objekt.

Du kan skapa en PDF baserad på en XDP-fil i databasen på något av två sätt. Du kan skicka XDP-platsen med referens eller så kan du hämta XDP-filen från databasen programmässigt och skicka den till utdatatjänsten i en XDP-fil.

[Snabbstart (EJB-läge): Skapa ett PDF-dokument baserat på en program-XDP-fil med Java-API:t ](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-based-on-an-application-xdp-file-using-the-java-api) (visar hur du skickar platsen för XDP-filen med referens).

[Snabbstart (EJB-läge): Skickar ett dokument i AEM Forms-databasen till utdatatjänsten med Java-API:t ](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-passing-a-document-located-in-the-repository-to-the-output-service-using-the-java-api) (visar hur du hämtar XDP-filen programmatiskt från AEM Forms-databasen och skickar den till utdatatjänsten i en `com.adobe.idp.Document`-instans). (I det här avsnittet beskrivs hur du utför den här uppgiften)

>[!NOTE]
>
>Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-3}

Så här skickar du ett dokument som hämtats från AEM Forms-databasen till Output-tjänsten:

1. Inkludera projektfiler.
1. Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten.
1. Hämta formulärdesignen från AEM Forms-databasen.
1. Återge det icke-interaktiva PDF-formuläret.
1. Utför en åtgärd med dataströmmen.

**Inkludera projektfiler**

Inkludera de filer som behövs i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster inkluderar du proxyfilerna.

**Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten**

Innan du programmässigt kan utföra en API-åtgärd för en utdatatjänst skapar du ett API-objekt för utdataklienten. Eftersom det här arbetsflödet hämtar en XDP-fil från Content Services (utgått) skapar du också ett API-objekt för dokumenthantering.

**Hämta formulärdesignen från AEM Forms-databasen**

Hämta XDP-filen från AEM Forms-databasen med API:t för databas. (Se [Läser resurser](/help/forms/developing/aem-forms-repository.md#reading-resources).)

XDP-filen returneras i en `com.adobe.idp.Document`-instans (eller en `BLOB`-instans om du använder webbtjänster). Du kan sedan skicka `com.adobe.idp.Document`-instansen till utdatatjänsten.

**Återge det icke-interaktiva PDF-formuläret**

Om du vill återge ett icke-interaktivt formulär skickar du `com.adobe.idp.Document`-instansen som returnerades med AEM Forms Repository API.

>[!NOTE]
>
>Två nya metoder med namnen `generatePDFOutput2` och `generatePrintedOutput2`godkänner ett `com.adobe.idp.Document`-objekt som innehåller en formulärdesign. Du kan också skicka en `com.adobe.idp.Document` som innehåller formulärdesignen till utdatatjänsten när du skickar en utskriftsström till en nätverksskrivare.

**Utför en åtgärd med formulärdataströmmen**

Du kan spara det icke-interaktiva formuläret som en PDF-fil. Formuläret kan visas i Adobe Reader eller Acrobat.

**Se även**

[Skicka dokument i databasen till utdatatjänsten med Java API](creating-document-output-streams.md#pass-documents-located-in-the-repository-to-the-output-service-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

ResursDatabasKlient

### Skicka dokument i databasen till utdatatjänsten med Java API {#pass-documents-located-in-the-repository-to-the-output-service-using-the-java-api}

Skicka ett dokument som hämtats från databasen med hjälp av utdatatjänsten och databas-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar och adobe-database-client.jar, i Java-projektets klassökväg.

1. Skapa ett utdata och ett API-objekt för dokumenthanteringsklienten.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper. (Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).)
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.
   * Skapa ett `DocumentManagementServiceClientImpl`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Hämta formulärdesignen från AEM Forms Repository.

   Anropa `ResourceRepositoryClient`-objektets `readResourceContent`-metod och skicka ett strängvärde som anger URI-platsen till XDP-filen. Exempel: `/Applications/FormsApplication/1.0/FormsFolder/Loan.xdp`. Detta värde är obligatoriskt. Den här metoden returnerar en `com.adobe.idp.Document`-instans som representerar XDP-filen.

1. Återge det icke-interaktiva PDF-formuläret.

   Anropa `OutputClient`-objektets `generatePDFOutput2`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger innehållsroten där de ytterligare resurserna, t.ex. bilderna, finns. Exempel: `repository:///Applications/FormsApplication/1.0/FormsFolder/`.
   * Ett `com.adobe.idp.Document`-objekt som representerar formulärdesignen (använd instansen som returneras av `ResourceRepositoryClient`-objektets `readResourceContent` -metod).
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.

   Metoden `generatePDFOutput2` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

1. Utför en åtgärd med formulärdataströmmen.

   * Hämta ett `com.adobe.idp.Document`-objekt som representerar det icke-interaktiva formuläret genom att anropa `OutputResult`-objektets `getGeneratedDoc`-metod.
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filnamnstillägget är .pdf.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `getGeneratedDoc`).

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): skicka ett dokument i AEM Forms-databasen till utdatatjänsten med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-passing-a-document-located-in-the-repository-to-the-output-service-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Skapa PDF-dokument med fragment {#creating-pdf-documents-using-fragments}

Du kan använda tjänsterna Output och Assembler för att skapa en utdataström, t.ex. ett PDF-dokument, som är baserad på fragment. Assembler-tjänsten sätter ihop ett XDP-dokument som är baserat på fragment i flera XDP-filer. Det monterade XDP-dokumentet skickas till Output-tjänsten som skapar ett PDF-dokument. Även om det här arbetsflödet visar att ett PDF-dokument genereras, kan Output-tjänsten generera andra utdatatyper, som ZPL, för det här arbetsflödet. Ett PDF-dokument används endast i diskussionssyfte.

Följande bild visar det här arbetsflödet.

![cp_cp_outputassemblefragments](assets/cp_cp_outputassemblefragments.png)

Innan du läser *Skapa PDF-dokument med fragment* bör du känna till hur du använder Assembler-tjänsten för att samla ihop flera XDP-dokument. (Se [Sammanställa flera XDP-fragment](/help/forms/developing/assembling-pdf-documents.md#assembling-multiple-xdp-fragments).)

>[!NOTE]
>
>Du kan också skicka en formulärdesign som har sammanställts av Assembler-tjänsten till Forms istället för till Output-tjänsten. Den största skillnaden mellan Output-tjänsten och Forms-tjänsten är att Forms-tjänsten genererar interaktiva PDF-dokument och Output-tjänsten producerar icke-interaktiva PDF-dokument. Forms-tjänsten kan inte heller generera skrivarbaserade utdataströmmar som ZPL.

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-4}

Så här skapar du ett PDF-dokument baserat på fragment:

1. Inkludera projektfiler.
1. Skapa ett Output and Assembler Client-objekt.
1. Använd Assembler-tjänsten för att generera formulärdesignen.
1. Använd Output-tjänsten för att generera PDF-dokumentet.
1. Spara PDF-dokumentet som en PDF-fil.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

**Skapa ett Output and Assembler-klientobjekt**

Innan du programmässigt kan utföra en API-åtgärd för en utdatatjänst skapar du ett API-objekt för utdataklienten. Eftersom det här arbetsflödet anropar Assembler-tjänsten för att skapa formulärdesignen skapar du ett Assembler Client API-objekt.

**Använd Assembler-tjänsten för att generera formulärdesignen**

Använd Assembler-tjänsten för att generera formulärdesignen med fragment. Assembler-tjänsten returnerar en `com.adobe.idp.Document`-instans som innehåller formulärdesignen.

**Använd utdatatjänsten för att generera PDF-dokumentet**

Du kan använda utdatatjänsten för att generera ett PDF-dokument med hjälp av den formulärdesign som Assembler-tjänsten skapade. Skicka `com.adobe.idp.Document`-instansen som Assembler-tjänsten returnerade till utdatatjänsten.

**Spara PDF-dokumentet som en PDF-fil**

När utdatatjänsten har genererat ett PDF-dokument kan du spara det som en PDF-fil.

**Se även**

[Skapa ett PDF-dokument baserat på fragment med Java API](creating-document-output-streams.md#create-a-pdf-document-based-on-fragments-using-the-java-api)

[Skapa ett PDF-dokument baserat på fragment med webbtjänstens API](creating-document-output-streams.md#create-a-pdf-document-based-on-fragments-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

[Sammanställa flera XDP-fragment](/help/forms/developing/assembling-pdf-documents.md#assembling-multiple-xdp-fragments)

[Skapa PDF-dokument](creating-document-output-streams.md#creating-pdf-documents)

### Skapa ett PDF-dokument baserat på fragment med Java API {#create-a-pdf-document-based-on-fragments-using-the-java-api}

Skapa ett PDF-dokument baserat på fragment med hjälp av API:t för utdatatjänsten och API:t för Assembler-tjänsten (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output and Assembler Client-objekt.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.
   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Använd Assembler-tjänsten för att generera formulärdesignen.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande obligatoriska värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar det DDX-dokument som ska användas.
   * Ett `java.util.Map`-objekt som innehåller XDP-indatafilerna.
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen, inklusive standardteckensnittet och jobbloggsnivån.

   Metoden `invokeDDX` returnerar ett `com.adobe.livecycle.assembler.client.AssemblerResult`-objekt som innehåller det sammansatta XDP-dokumentet. Så här hämtar du det monterade XDP-dokumentet:

   * Anropa metoden `getDocuments` för objektet `AssemblerResult`. Den här metoden returnerar ett `java.util.Map`-objekt.
   * Upprepa genom objektet `java.util.Map` tills du hittar det resulterande `com.adobe.idp.Document`-objektet.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att extrahera det monterade XDP-dokumentet.

1. Använd Output-tjänsten för att generera PDF-dokumentet.

   Anropa `OutputClient`-objektets `generatePDFOutput2`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`
   * Ett strängvärde som anger innehållsroten där de ytterligare resurserna, till exempel bilder, finns
   * Ett `com.adobe.idp.Document`-objekt som representerar formulärdesignen (använd instansen som returneras av Assembler-tjänsten)
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen

   Metoden `generatePDFOutput2` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden

1. Spara PDF-dokumentet som en PDF-fil.

   * Hämta ett `com.adobe.idp.Document`-objekt som representerar PDF-dokumentet genom att anropa `OutputResult`-objektets `getGeneratedDoc`-metod.
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filnamnstillägget är .pdf.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen. (Kontrollera att du använder objektet `com.adobe.idp.Document` som metoden `getGeneratedDoc` returnerade.)

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): Skapa ett PDF-dokument baserat på fragment med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-based-on-fragments-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa ett PDF-dokument baserat på fragment med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-based-on-fragments-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Anger anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

### Skapa ett PDF-dokument baserat på fragment med webbtjänstens API {#create-a-pdf-document-based-on-fragments-using-the-web-service-api}

Skapa ett PDF-dokument baserat på fragment med hjälp av API:t för Output Service och Assembler Service (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Använd följande WSDL-definition för den tjänstreferens som är kopplad till utdatatjänsten:

   ```java
    http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1.
   ```

   Använd följande WSDL-definition för den tjänstreferens som är associerad med Assembler-tjänsten:

   ```java
    http://localhost:8080/soap/services/AssemblerService?WSDL&lc_version=9.0.1.
   ```

   Eftersom datatypen `BLOB` är gemensam för båda tjänstreferenserna kan du kvalificera datatypen `BLOB` fullständigt när du använder den. I motsvarande snabbstart för webbtjänsten är alla `BLOB` instanser kvalificerade.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output and Assembler Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.

   * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

   >[!NOTE]
   >
   >Upprepa dessa steg för objektet `AssemblerServiceClient`.

1. Använd Assembler-tjänsten för att generera formulärdesignen.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet
   * Objektet `MyMapOf_xsd_string_To_xsd_anyType` som innehåller de nödvändiga filerna
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ

   Metoden `invokeDDX` returnerar ett `AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som inträffade. Utför följande åtgärder för att hämta det nya XDP-dokumentet:

   * Öppna `AssemblerResult`-objektets `documents`-fält, som är ett `Map`-objekt som innehåller de resulterande PDF-dokumenten.
   * Iterera genom objektet `Map` för att hämta den sammansatta formulärdesignen. Kasta den arraymedlemmens `value` till en `BLOB`. Skicka den här `BLOB`-instansen till utdatatjänsten.

1. Använd Output-tjänsten för att generera PDF-dokumentet.

   Anropa `OutputServiceClient`-objektets `generatePDFOutput2`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger innehållsroten där de ytterligare resurserna, t.ex. bilder, finns.
   * Ett `BLOB`-objekt som representerar formulärdesignen (använd instansen `BLOB` som returneras av Assembler-tjänsten).
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.
   * Ett `BLOB`-utdataobjekt som fylls i av metoden `generatePDFOutput2`. Metoden `generatePDFOutput2` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs bara för webbtjänstanrop).
   * Ett `OutputResult`-utdataobjekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs bara för webbtjänstanrop).

   Metoden `generatePDFOutput2` returnerar ett `BLOB`-objekt som innehåller det icke-interaktiva PDF-formuläret.

1. Spara PDF-dokumentet som en PDF-fil.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för det interaktiva PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `BLOB` som hämtats från metoden `generatePDFOutput2`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till en PDF-fil genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

## Skriva ut till filer {#printing-to-files}

Du kan använda utdatatjänsten för att skriva ut strömmar som PostScript, Printer Control Language (PCL) eller följande etikettformat till en fil:

* Zebra - ZPL
* Intermec - IPL
* Datamax - DPL
* TecToshiba - TPCL

Med hjälp av tjänsten Output kan du sammanfoga XML-data med en formulärdesign och skriva ut formuläret till en fil. Följande bild visar hur Output-tjänsten skapar laserfiler och etikettfiler.

>[!NOTE]
>
>Mer information om hur du skickar utskriftsströmmar till skrivare finns i [Skicka utskriftsströmmar till skrivare](creating-document-output-streams.md#sending-print-streams-to-printers).

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-5}

Så här skriver du ut till en fil:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Referera till en XML-datakälla.
1. Ange alternativ för utskriftskörning som krävs för att skriva ut till en fil.
1. Skriv ut utskriftsströmmen till en fil.
1. Hämta resultatet av åtgärden.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på. (Se [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).)

**Skapa ett Output Client-objekt**

Innan du programmässigt kan utföra en utdatatjänståtgärd måste du skapa ett klientobjekt för utdatatjänsten. Om du använder Java API skapar du ett `OutputClient`-objekt. Skapa ett `OutputServiceService`-objekt om du använder API:t för webbtjänsten för utdata.

**Referera till en XML-datakälla**

Om du vill skriva ut ett dokument som innehåller data måste du referera till en XML-datakälla som innehåller XML-element för varje formulärfält som du vill fylla i med data. XML-elementnamnet måste matcha fältnamnet. Ett XML-element ignoreras om det inte motsvarar ett formulärfält eller om XML-elementnamnet inte matchar fältnamnet. Det är inte nödvändigt att matcha den ordning i vilken XML-elementen visas om alla XML-element har angetts.

**Ange nödvändiga alternativ för utskriftskörning för att skriva ut till en fil**

Om du vill skriva ut till en fil måste du ange körningsalternativet Fil-URI genom att ange plats och namn för filen som utdatatjänsten skriver ut på. Om du till exempel vill instruera Output-tjänsten att skriva ut en PostScript-fil med namnet *MortgageForm.ps* till C:\Adobe anger du C:\Adobe\MortgageForm.ps.

>[!NOTE]
>
>Det finns valfria körningsalternativ som du kan definiera. Mer information om alla alternativ som du kan ange finns i klassreferensen `PrintedOutputOptionsSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

**Skriv ut utskriftsströmmen till en fil**

När du har refererat till en giltig XML-datakälla som innehåller formulärdata och angett alternativ för utskriftskörning, kan du anropa utdatatjänsten, vilket gör att den skriver ut en fil.

**Hämta resultatet av åtgärden**

När utdatatjänsten har utfört en åtgärd returneras olika dataobjekt, till exempel XML-data, som anger om åtgärden lyckades.

**Se även**

[Skriva ut till filer med Java API](creating-document-output-streams.md#print-to-files-using-the-java-api)

[Skriva ut till filer med hjälp av webbtjänstens API](creating-document-output-streams.md#print-to-files-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

### Skriva ut till filer med Java API {#print-to-files-using-the-java-api}

Skriva ut till en fil med hjälp av utdata-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till en XML-datakälla.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar XML-datakällan som används för att fylla i dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange alternativ för utskriftskörning som krävs för att skriva ut till en fil.

   * Skapa ett `PrintedOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange filen genom att anropa PrintOutputOptionsSpec-objektets `setFileURI`-metod och skicka ett strängvärde som representerar filens namn och plats. Om du till exempel vill att utdatatjänsten ska skriva ut till en PostScript-fil med namnet MortgageForm.ps i C:\Adobe anger du C:\\Adobe\MortgageForm.ps.
   * Ange antalet kopior som ska skrivas ut genom att anropa `PrintedOutputOptionsSpec`-objektets `setCopies`-metod och skicka ett heltalsvärde som representerar antalet kopior.

1. Skriv ut utskriftsströmmen till en fil.

   Skriv ut till en fil genom att anropa `OutputClient`-objektets `generatePrintedOutput`-metod och skicka följande värden:

   * Ett `PrintFormat`-uppräkningsvärde som anger vilket utskriftsdataströmformat som ska skapas. Om du till exempel vill skapa en PostScript-utskriftsström skickar du `PrintFormat.PostScript`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger platsen för relaterade säkerhetsfiler, t.ex. bildfiler.
   * Ett strängvärde som anger platsen för XDC-filen som ska användas (du kan skicka `null` om du har angett XDC-filen som ska användas med objektet `PrintedOutputOptionsSpec`).
   * Objektet `PrintedOutputOptionsSpec` som innehåller körningsalternativ som krävs för att skriva ut till en fil.
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller formulärdata.

   Metoden `generatePrintedOutput` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

   >[!NOTE]
   >
   >`OutputResult`-objektets `getRecordLevelMetaDataList`-metod returnerar `null`.

1. Hämta resultatet av åtgärden.

   * Skapa ett `com.adobe.idp.Document`-objekt som representerar `generatePrintedOutput`-metodens status genom att anropa `OutputResult`-objektets `getStatusDoc`-metod (objektet `OutputResult` returnerades av metoden `generatePrintedOutput`).
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filtillägget är XML.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `getStatusDoc`).

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (SOAP-läge): Skriva ut till en fil med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-printing-to-a-file-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Anger anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).

### Skriva ut till filer med hjälp av webbtjänstens API {#print-to-files-using-the-web-service-api}

Skriva ut till en fil med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till en XML-datakälla.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra formulärdata.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen som innehåller formulärdata.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `binaryData`-egenskap med innehållet i bytearrayen.

1. Ange alternativ för utskriftskörning som krävs för att skriva ut till en fil.

   * Skapa ett `PrintedOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange filen genom att tilldela ett strängvärde som representerar platsen och namnet på filen till `PrintedOutputOptionsSpec`-objektets `fileURI`-datamedlem. Om du till exempel vill att utdatatjänsten ska skriva ut till en PostScript-fil med namnet *MortgageForm.ps* i C:\Adobe anger du C:\\Adobe\MortgageForm.ps.
   * Ange antalet kopior som ska skrivas ut genom att tilldela ett heltalsvärde som representerar antalet kopior till `copies`-objektets `PrintedOutputOptionsSpec`-datamedlemmar.

1. Skriv ut utskriftsströmmen till en fil.

   Skriv ut till en fil genom att anropa `OutputServiceService`-objektets `generatePrintedOutput`-metod och skicka följande värden:

   * Ett `PrintFormat`-uppräkningsvärde som anger vilket utskriftsdataströmformat som ska skapas. Om du till exempel vill skapa en PostScript-utskriftsström skickar du `PrintFormat.PostScript`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger platsen för relaterade säkerhetsfiler, t.ex. bildfiler.
   * Ett strängvärde som anger platsen för XDC-filen som ska användas (du kan skicka `null` om du har angett XDC-filen som ska användas med objektet `PrintedOutputOptionsSpec`).
   * Objektet `PrintedOutputOptionsSpec` som innehåller alternativ för utskriftskörning som krävs för att skriva ut till en fil.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller formulärdata.
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs endast för webbtjänstanrop.)
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med resultatdata. (Det här parametervärdet krävs endast för webbtjänstanrop.)
   * Ett `OutputResult`-objekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs endast för webbtjänstanrop.)

1. Hämta resultatet av åtgärden.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar en plats för en XML-fil som innehåller resultatdata. Kontrollera att filtillägget är XML.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som fylldes med resultatdata av metoden `generatePDFOutput` för objektet `OutputServiceService` (den åttonde parametern). Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till XML-filen genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Skicka utskriftsströmmar till skrivare {#sending-print-streams-to-printers}

Du kan använda Utdatatjänsten för att skicka utskriftsströmmar som PostScript, Printer Control Language (PCL) eller följande etikettformat till nätverksskrivare:

* Zebra - ZPL
* Intermec - IPL
* Datamax - DPL
* TecToshiba - TPCL

Med hjälp av tjänsten Output kan du sammanfoga XML-data med en formulärdesign och skriva ut formuläret som en utskriftsström. Du kan till exempel skapa en PostScript-utskriftsström och skicka den till en nätverksskrivare. Följande bild visar hur Output-tjänsten skickar utskriftsströmmar till nätverksskrivare.

>[!NOTE]
>
>För att visa hur du skickar en utskriftsström till en nätverksskrivare skickar det här avsnittet en PostScript-utskriftsström till en nätverksskrivare med hjälp av skrivarprotokollet SharedPrinter.

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-6}

Så här skickar du en utskriftsström till en nätverksskrivare:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Referera till en XML-datakälla.
1. Ange alternativ för utskriftskörning
1. Hämta ett dokument som ska skrivas ut.
1. Skicka dokumentet till en nätverksskrivare.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms distribueras på JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på.

**Skapa ett Output Client-objekt**

Skapa ett klientobjekt för utdatatjänsten innan du programmässigt utför en åtgärd. Om du använder Java API skapar du ett `OutputClient`-objekt. Skapa ett `OutputServiceClient`-objekt om du använder API:t för webbtjänsten för utdata.

**Referera till en XML-datakälla**

Om du vill skriva ut ett dokument som innehåller data måste du referera till en XML-datakälla som innehåller XML-element för varje formulärfält som du vill fylla i med data. XML-elementnamnet måste matcha fältnamnet. Ett XML-element ignoreras om det inte motsvarar ett formulärfält eller om XML-elementnamnet inte matchar fältnamnet. Det är inte nödvändigt att matcha den ordning i vilken XML-elementen visas om alla XML-element har angetts.

**Ange alternativ för utskriftskörning**

Du kan ange körningsalternativ när du skickar en utskriftsström till en skrivare, inklusive följande alternativ:

* **Kopior**: Anger antalet kopior som ska skickas till skrivaren. Standardvärdet är 1.
* **Häftning**: Ett XCI-alternativ anges när en häftare används. Det här alternativet kan anges i konfigurationsmodellen av elementet staple och används endast för PS- och PCL-skrivare.
* **OutputJog**: Ett XCI-alternativ anges när utdatasidor ska joggas (fysiskt flyttas i utmatningsfacket). Det här alternativet gäller endast för PS- och PCL-skrivare.
* **OutputBin**: XCI-värde som används för att aktivera skrivardrivrutinen för att välja lämplig utdatabing.

>[!NOTE]
>
>Mer information om alla körningsalternativ som du kan ange finns i klassreferensen `PrintedOutputOptionsSpec`.

**Hämta ett dokument som ska skrivas ut**

Hämta en utskriftsström som ska skickas till en skrivare. Du kan till exempel hämta en PostScript-fil och skicka den till en skrivare.

Du kan välja att skicka en PDF-fil om din skrivare stöder PDF. Ett problem med att skicka ett PDF-dokument till en skrivare är dock att varje skrivartillverkare har olika implementeringar av PDF tolk. Det innebär att vissa tillverkare använder Adobe PDF tolkning, men det beror på skrivaren. Andra skrivare har sin egen PDF-tolk. Resultatet av utskriften kan därför variera.

En annan begränsning för att skicka ett PDF-dokument till en skrivare är att det bara skrivs ut. Det går inte att komma åt dubbelsidig utskrift, val av pappersfack och häftning, förutom via skrivarinställningarna.

Om du vill hämta ett dokument som ska skrivas ut använder du metoden `generatePrintedOutput`. Följande tabell anger innehållstyper som ställs in för en given utskriftsström när metoden `generatePrintedOutput` används.

<table>
 <thead>
  <tr>
   <th><p>Utskriftsformat </p></th>
   <th><p>Beskrivning</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>DPL </p></td>
   <td><p>Skapar en dpl203.xdc som standard eller en anpassad xdc-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>DPL300DPI </p></td>
   <td><p>Skapar en DPL 300 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>DPL406DPI </p></td>
   <td><p>Skapar en DPL 400 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>DPL600DPI </p></td>
   <td><p>Skapar en DPL 600 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>GenericColorPCL </p></td>
   <td><p>Skapar en generisk PCL-utdataström (5c).</p></td>
  </tr>
  <tr>
   <td><p>GenericPSLevel3 </p></td>
   <td><p>Skapar en generisk utdataström för PostScript Level 3.</p></td>
  </tr>
  <tr>
   <td><p>IPL </p></td>
   <td><p>Skapar en anpassad IPL-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>IPL300DPI </p></td>
   <td><p>Skapar en IPL 300 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>IPL400DPI </p></td>
   <td><p>Skapar en IPL 400 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>PCL </p></td>
   <td><p>Skapar en allmän monokrom PCL-utdataström (5e).</p></td>
  </tr>
  <tr>
   <td><p>PostScript </p></td>
   <td><p>Skapar en generisk PostScript Level 2-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>TPCL </p></td>
   <td><p>Skapar en anpassad TPCL-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>TPCL305DPI </p></td>
   <td><p>Skapar en TPCL 305 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>TPCL600DPI </p></td>
   <td><p>Skapar en TPCL 600 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>ZPL </p></td>
   <td><p>Skapar en ZPL 203 DPI-utdataström.</p></td>
  </tr>
  <tr>
   <td><p>ZPL300DPI </p></td>
   <td><p>Skapar en ZPL 300 DPI-utdataström.</p></td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Du kan också skicka en utskriftsström till en skrivare med metoden `generatePrintedOutput2`. Snabbstarten som är associerad med avsnittet Skicka utskriftsströmmar till skrivare använder metoden `generatePrintedOutput`.

**Skicka utskriftsströmmen till en nätverksskrivare**

När du har hämtat ett dokument som ska skrivas ut kan du anropa utdatatjänsten, vilket gör att den skickar en utskriftsström till en nätverksskrivare. För att utdatatjänsten ska kunna hitta skrivaren måste du ange både utskriftsservern och skrivarnamnet. Dessutom måste du ange utskriftsprotokoll.

>[!NOTE]
>
>Om PDFG är installerat på Forms Server och servern körs på Windows Server 2008 kan du inte använda egenskapen SharedPrinter. I det här fallet använder du ett annat skrivarprotokoll.

>[!NOTE]
>
>Om du använder en nätverksskrivare och åtkomstmekanismen är SharedPrinter måste du ange den fullständiga nätverkssökvägen för skrivaren.Skicka en utskriftsström till en nätverksskrivare med Java API

Skicka en utskriftsström till en nätverksskrivare med hjälp av utdata-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera en XML-datakälla

   * Skapa ett `java.io.FileInputStream`-objekt som representerar XML-datakällan som används för att fylla i dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange alternativ för utskriftskörning

   Skapa ett `PrintedOutputOptionsSpec`-objekt som representerar alternativ för utskriftskörning. Du kan till exempel ange antalet kopior som ska skrivas ut genom att anropa metoden `setCopies` för objektet `PrintedOutputOptionsSpec`.

   >[!NOTE]
   >
   >Du kan inte ange sidnumreringsvärdet med `PrintedOutputOptionsSpec`-objektets `setPagination`-metod om du genererar en ZPL-utskriftsström. Du kan inte heller ange följande alternativ för en ZPL-utskriftsström: OutputJog, PageOffset och Staple. Metoden `setPagination` är inte giltig för PostScript-generering. Den gäller endast för PCL-generering.

1. Hämta ett dokument som ska skrivas ut

   * Hämta ett dokument som ska skrivas ut genom att anropa `OutputClient`-objektets `generatePrintedOutput`-metod och skicka följande värden:

      * Ett `PrintFormat`-uppräkningsvärde som anger utskriftsströmmen. Om du till exempel vill skapa en PostScript-utskriftsström skickar du `PrintFormat.PostScript`.
      * Ett strängvärde som anger formulärdesignens namn.
      * Ett strängvärde som anger platsen för relaterade säkerhetsfiler, t.ex. bildfiler.
      * Ett strängvärde som anger platsen för XDC-filen som ska användas.
      * Objektet `PrintedOutputOptionsSpec` som innehåller körningsalternativ som krävs för att skriva ut till en fil.
      * Objektet `com.adobe.idp.Document` som representerar XML-datakällan som innehåller formulärdata som ska sammanfogas med formulärdesignen.

     Den här metoden returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

   * Skapa ett `com.adobe.idp.Document`-objekt som ska skickas till skrivaren genom att anropa metoden `OutputResult` object&#39;s `getGeneratedDoc` . Den här metoden returnerar ett `com.adobe.idp.Document`-objekt.

1. Skicka utskriftsströmmen till en nätverksskrivare

   Skicka utskriftsströmmen till en nätverksskrivare genom att anropa `OutputClient`-objektets `sendToPrinter`-metod och skicka följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar utskriftsströmmen som ska skickas till skrivaren.
   * Ett `PrinterProtocol`-uppräkningsvärde som anger vilket skrivarprotokoll som ska användas. Om du till exempel vill ange SharedPrinter-protokollet skickar du `PrinterProtocol.SharedPrinter`.
   * Ett strängvärde som anger utskriftsserverns namn. Om till exempel namnet på utskriftsservern är PrintServer1, skickar du `\\\PrintSever1`.
   * Ett strängvärde som anger skrivarens namn. Om skrivarens namn till exempel är Skrivare1, skickar du `\\\PrintSever1\Printer1`.

   >[!NOTE]
   >
   >Metoden `sendToPrinter` lades till i AEM Forms API i version 8.2.1.

### Skicka en utskriftsström till en skrivare med hjälp av webbtjänstens API {#send-a-print-stream-to-a-printer-using-the-web-service-api}

Skicka en utskriftsström till en nätverksskrivare med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till en XML-datakälla.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra formulärdata.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som anger platsen för XML-filen som innehåller formulärdata.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Ta reda på bytearraylängden genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Ange alternativ för utskriftskörning.

   Skapa ett `PrintedOutputOptionsSpec`-objekt med hjälp av dess konstruktor. Du kan till exempel ange antalet kopior som ska skrivas ut genom att tilldela ett heltalsvärde som representerar antalet kopior till `copies`-objektets `PrintedOutputOptionsSpec`-datamedlem.

   >[!NOTE]
   >
   >Du kan inte ange sidnumreringsvärdet med `pagination`-objektets `PrintedOutputOptionsSpec`-datamedlem om du genererar en ZPL-utskriftsström. Du kan inte heller ange följande alternativ för en ZPL-utskriftsström: OutputJog, PageOffset och Staple. Datamedlemmen `pagination` är inte giltig för PostScript-generering. Den gäller endast för PCL-generering.

1. Hämta ett dokument som ska skrivas ut.

   * Hämta ett dokument som ska skrivas ut genom att anropa `OutputServiceService`-objektets `generatePrintedOutput`-metod och skicka följande värden:

      * Ett `PrintFormat`-uppräkningsvärde som anger utskriftsströmmen. Om du till exempel vill skapa en PostScript-utskriftsström skickar du `PrintFormat.PostScript`.
      * Ett strängvärde som anger formulärdesignens namn.
      * Ett strängvärde som anger platsen för relaterade säkerhetsfiler, t.ex. bildfiler.
      * Ett strängvärde som anger platsen för XDC-filen som ska användas.
      * Objektet `PrintedOutputOptionsSpec` som innehåller alternativ för utskriftskörning som används när en utskriftsström skickas till en nätverksskrivare.
      * Objektet `BLOB` som innehåller XML-datakällan som innehåller formulärdata.
      * Ett `BLOB`-objekt som fylls i av metoden `generatePrintedOutput`. Metoden `generatePrintedOutput` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs endast för webbtjänstanrop.)
      * Ett `BLOB`-objekt som fylls i av metoden `generatePrintedOutput`. Metoden `generatePrintedOutput` fyller i det här objektet med resultatdata. (Det här parametervärdet krävs endast för webbtjänstanrop.)
      * Ett `OutputResult`-objekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs endast för webbtjänstanrop.)

   * Skapa ett `BLOB`-objekt som ska skickas till skrivaren genom att hämta värdet för metoden `generatedDoc` för `OutputResult`-objektet. Den här metoden returnerar ett `BLOB`-objekt som innehåller PostScript-data som returneras av metoden `generatePrintedOutput`.

1. Skicka utskriftsströmmen till en nätverksskrivare.

   Skicka utskriftsströmmen till en nätverksskrivare genom att anropa `OutputClient`-objektets `sendToPrinter`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar utskriftsströmmen som ska skickas till skrivaren.
   * Ett `PrinterProtocol`-uppräkningsvärde som anger vilket skrivarprotokoll som ska användas. Om du till exempel vill ange SharedPrinter-protokollet skickar du `PrinterProtocol.SharedPrinter`.
   * Ett `bool`-värde som anger om föregående parametervärde ska användas. Skicka värdet `true`. (Det här parametervärdet krävs endast för webbtjänstanrop.)
   * Ett strängvärde som anger utskriftsserverns namn. Om du till exempel antar att namnet på utskriftsservern är PrintServer1, skickar du `\\\PrintSever1`.
   * Ett strängvärde som anger skrivarens namn. Om du till exempel antar att skrivarens namn är Skrivare1, skickar du `\\\PrintSever1\Printer1`.

   >[!NOTE]
   >
   >Metoden `sendToPrinter` lades till i AEM Forms API i version 8.2.1.

## Skapa flera utdatafiler {#creating-multiple-output-files}

Utdatatjänsten kan skapa separata dokument för varje post i en XML-datakälla eller en enda fil som innehåller alla poster (den här funktionen är standard). Anta till exempel att tio poster finns i en XML-datakälla och att du instruerar Output-tjänsten att skapa separata PDF-dokument (eller andra typer av utdata) för varje post med hjälp av API:t för utdatatjänsten. Resultatet blir att Output-tjänsten genererar tio PDF-dokument. (I stället för att skapa dokument kan du skicka flera utskriftsströmmar till en skrivare.)

I följande bild visas också hur Output-tjänsten bearbetar en XML-datafil som innehåller flera poster. Anta dock att du instruerar Output-tjänsten att skapa ett enda PDF-dokument som innehåller alla dataposter. I det här fallet genererar utdatatjänsten ett dokument som innehåller alla poster.

Följande bild visar hur Output-tjänsten bearbetar en XML-datafil som innehåller flera poster. Anta att du instruerar Output-tjänsten att skapa ett separat PDF-dokument för varje datapost. I sådana fall genererar Output-tjänsten ett separat PDF-dokument för varje datapost.

![cm_outputbatchmany](assets/cm_outputbatchmany.png)

Följande XML-data visar ett exempel på en datafil som innehåller tre dataposter.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <batch>
 <LoanRecord>
     <mortgageAmount>500000</mortgageAmount>
     <lastName>Blue</lastName>
     <firstName>Tony</firstName>
     <SSN>555666777</SSN>
     <PositionTitle>Product Manager</PositionTitle>
     <Address>555 No Where Dr</Address>
     <City>New York</City>
     <StateProv>New York</StateProv>
     <ZipCode>51256</ZipCode>
     <Email>TBlue@NoMailServer.com</Email>
     <PhoneNum>555-7418</PhoneNum>
     <FaxNum>555-9981</FaxNum>
     <Description>Buy a home</Description>
 </LoanRecord>
 <LoanRecord>
     <mortgageAmount>300000</mortgageAmount>
     <lastName>White</lastName>
     <firstName>Sam</firstName>
     <SSN>555666222</SSN>
     <PositionTitle>Program Manager</PositionTitle>
     <Address>557 No Where Dr</Address>
     <City>New York</City>
     <StateProv>New York</StateProv>
     <ZipCode>51256</ZipCode>
     <Email>SWhite@NoMailServer.com</Email>
     <PhoneNum>555-7445</PhoneNum>
     <FaxNum>555-9986</FaxNum>
     <Description>Buy a home</Description>
 </LoanRecord>
 <LoanRecord>
     <mortgageAmount>700000</mortgageAmount>
     <lastName>Green</lastName>
     <firstName>Steve</firstName>
     <SSN>55566688</SSN>
     <PositionTitle>Project Manager</PositionTitle>
     <Address>445 No Where Dr</Address>
     <City>New York</City>
     <StateProv>New York</StateProv>
     <ZipCode>51256</ZipCode>
     <Email>SGreeb@NoMailServer.com</Email>
     <PhoneNum>555-2211</PhoneNum>
     <FaxNum>555-2221</FaxNum>
     <Description>Buy a home</Description>
 </LoanRecord>
 </batch>
```

Observera att XML-elementet som startar och avslutar varje datapost är `LoanRecord`. Det här XML-elementet refereras av programlogiken som genererar flera filer.

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-7}

Så här skapar du flera PDF-filer baserade på en XML-datakälla:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Referera till en XML-datakälla.
1. Ange körningsalternativ för PDF.
1. Ange alternativ för återgivning vid körning.
1. Generera flera PDF-filer.
1. Hämta resultatet av åtgärden.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på.

**Skapa ett Output Client-objekt**

Innan du programmässigt kan utföra en utdatatjänståtgärd måste du skapa ett klientobjekt för utdatatjänsten. Om du använder Java API skapar du ett `OutputClient`-objekt. Skapa ett `OutputServiceService`-objekt om du använder API:t för webbtjänsten för utdata.

**Referera till en XML-datakälla**

Referera till en XML-datakälla som innehåller flera poster. Ett XML-element måste användas för att separera dataposterna. I XML-datakällan som visades tidigare i det här avsnittet får till exempel XML-elementet som avgränsar dataposter namnet `LoanRecord`.

Det måste finnas ett XML-element för varje formulärfält som du vill fylla i med data. XML-elementnamnet måste matcha fältnamnet. Ett XML-element ignoreras om det inte motsvarar ett formulärfält eller om XML-elementnamnet inte matchar fältnamnet. Det är inte nödvändigt att matcha den ordning i vilken XML-elementen visas om alla XML-element har angetts.

**Ange körningsalternativ för PDF**

Ange följande körningsalternativ för Output-tjänsten för att skapa flera filer baserade på en XML-datakälla:

* **Många filer**: Anger om utdatatjänsten skapar ett eller flera dokument. Du kan ange true eller false. Om du vill skapa ett separat dokument för varje datapost i XML-datakällan anger du true.
* **Fil-URI**: Anger platsen för de filer som genereras av utdatatjänsten. Anta till exempel att du anger C:\\Adobe\forms\Loan.pdf. I sådana fall skapar Output-tjänsten en fil med namnet Loan.pdf och placerar filen i mappen C:\\Adobe\forms. När det finns flera filer är filnamnen Loan0001.pdf, Loan0002.pdf, Loan003.pdf och så vidare. Om du anger en filplats placeras filerna på servern, inte på klientdatorn.
* **Postnamn**: Anger XML-elementnamnet i datakällan som avgränsar dataposterna. I XML-datakällan som visades tidigare i det här avsnittet kallas till exempel XML-elementet som avgränsar dataposter `LoanRecord`. (I stället för att ange alternativet Postnamn vid körning kan du ange postnivån genom att tilldela den ett numeriskt värde som anger elementnivån som innehåller dataposter. Du kan dock bara ange postnamn eller postnivå. Du kan inte ange båda värdena.)

**Ange alternativ för återgivning vid körning**

Du kan ange alternativ för återgivning vid körning när du skapar flera filer. Även om dessa alternativ inte är nödvändiga (till skillnad från alternativ för körning av utdata som krävs) kan du utföra åtgärder som att förbättra prestanda för utdatatjänsten. Du kan till exempel cachelagra formulärdesignen som Output-tjänsten använder för att förbättra prestandan.

När utdatatjänsten bearbetar batchposter, läses data som innehåller flera poster stegvis. Det innebär att Output-tjänsten läser data till minnet och frigör data när en grupp med poster bearbetas. Utdatatjänsten läser in data stegvis när något av två körningsalternativ är inställt. Om du anger körningsalternativet Postnamn läser utdatatjänsten in data stegvis. Om du anger körningsalternativet Postnivå till 2 eller högre läser utdatatjänsten in data stegvis.

Du kan kontrollera om utdatatjänsten utför inkrementell inläsning med metoden `PDFOutputOptionsSpec` eller `PrintedOutputOptionSpec`-objektets `setLazyLoading`. Du kan skicka värdet `false` till den här metoden som inaktiverar inkrementell inläsning.

**Skapa flera PDF-filer**

När du har refererat till en giltig XML-datakälla som innehåller flera dataposter och angett körningsalternativ, kan du anropa utdatatjänsten, vilket gör att den genererar flera filer. När du genererar flera poster returnerar `OutputResult`-objektets `getGeneratedDoc`-metod `null`.

**Hämta resultatet av åtgärden**

När utdatatjänsten har utfört en åtgärd returneras XML-data som anger om åtgärden lyckades. Följande XML returneras av Output-tjänsten. I den här situationen genererade Output Service 42 dokument.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <printResult>
 <status>0</status>
 <requestId>4ad85f9e2</requestId>
 <context/>
 <messages>
 <message>Printed all 42 records successfully.</message>
 </messages>
 <printSpec>
 <input>
 <validated>true</validated>
 <dataFile recordIdField="" recordLevel="0" recordName="LoanRecord"/>
 <sniffRules lookAhead="300"/>
 <formDesign>Loan.xdp</formDesign>
 <contentRoot>C:\Adobe</contentRoot>
 <metadata-spec record="false"/>
 </input>
 <output>
 <format>PDF</format>
 <fileURI>C:\Adobe\forms\Loan.pdf</fileURI>
 <optionString>cacheenabled=true&padebug=false&linearpdf=false&pdfarevisionnumber=1&pdfaconformance=A&taggedpdf=false&TransactionTimeOut=180</optionString>
 <waitForResponse>true</waitForResponse>
 <outputStream>multiple</outputStream>
 </output>
 </printSpec>
 </printResult>
```

**Se även**

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

### Skapa flera PDF-filer med Java API {#create-multiple-pdf-files-using-the-java-api}

Skapa flera PDF-filer med hjälp av utdata-API (Java):

1. Inkludera projektfiler&quot;

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera en XML-datakälla

   * Skapa ett `java.io.FileInputStream`-objekt som representerar XML-datakällan som innehåller flera poster genom att använda dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange körningsalternativ för PDF

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange alternativet Många filer genom att anropa `PDFOutputOptionsSpec`-objektets `setGenerateManyFiles`-metod. Skicka till exempel värdet `true` för att instruera Output-tjänsten att skapa en separat PDF-fil för varje post i XML-datakällan. (Om du skickar `false` genererar utdatatjänsten ett enda PDF-dokument som innehåller alla poster).
   * Ange alternativet Fil-URI genom att anropa `PDFOutputOptionsSpec`-objektets `setFileUri`-metod och skicka ett strängvärde som anger platsen för de filer som genereras av utdatatjänsten. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.
   * Ange alternativet Postnamn genom att anropa `OutputOptionsSpec`-objektets `setRecordName`-metod och skicka ett strängvärde som anger XML-elementnamnet i datakällan som avgränsar dataposterna. (Ta till exempel en titt på XML-datakällan som visades tidigare i det här avsnittet. Namnet på XML-elementet som avgränsar dataposter är LoanRecord).

1. Ange alternativ för återgivning vid körning

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Cachelagra formulärdesignen för att förbättra utdatatjänstens prestanda genom att anropa `RenderOptionsSpec`-objektets `setCacheEnabled` och skicka `Boolean`-värdet `true`.

1. Generera flera PDF-filer

   Generera flera PDF-filer genom att anropa `OutputClient`-objektets `generatePDFOutput`-metod och skicka följande värden:

   * Ett enum-värde på `TransformationFormat`. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `com.adobe.idp.Document` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.

   Metoden `generatePDFOutput` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

1. Hämta resultatet av åtgärden

   * Skapa ett `java.io.File`-objekt som representerar en XML-fil som ska innehålla resultatet av metoden `generatePDFOutput`. Kontrollera att filnamnstillägget är .xml.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `applyUsageRights`).

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): Skapa flera PDF-filer med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-multiple-pdf-files-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Skapa flera PDF-filer med hjälp av webbtjänstens API {#create-multiple-pdf-files-using-the-web-service-api}

Skapa flera PDF-filer med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till en XML-datakälla.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra formulärdata som innehåller flera poster.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för XML-filen som innehåller flera poster.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Ange körningsalternativ för PDF.

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange alternativet Många filer genom att tilldela ett booleskt värde till `generateManyFiles`-objektets `OutputOptionsSpec`-datamedlem. Tilldela till exempel värdet `true` till den här datamedlemmen för att instruera utdatatjänsten att skapa en separat PDF-fil för varje post i XML-datakällan. (Om du tilldelar `false` till den här datamedlemmen genererar utdatatjänsten en enda PDF som innehåller alla poster).
   * Ange fil-URI-alternativet genom att tilldela ett strängvärde som anger platsen för filen/filerna som genereras av utdatatjänsten till `OutputOptionsSpec`-objektets `fileURI`-datamedlem. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.
   * Ange alternativet för postnamn genom att tilldela ett strängvärde som anger XML-elementnamnet i datakällan som avgränsar dataposterna till `recordName`-objektets `OutputOptionsSpec`-datamedlem.
   * Ange alternativet för kopior genom att tilldela ett heltalsvärde som anger antalet kopior som utdatatjänsten genererar till `copies`-objektets `OutputOptionsSpec`-datamedlem.

1. Ange alternativ för återgivning vid körning.

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Cachelagra formulärdesignen för att förbättra utdatatjänstens prestanda genom att tilldela värdet `true` till `RenderOptionsSpec`-objektets `cacheEnabled`-datamedlem.

1. Generera flera PDF-filer.

   Skapa flera PDF-filer genom att anropa `OutputServiceService`-objektets `generatePDFOutput`metod och skicka följande värden:

   * Ett TransformationFormat-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med genererade metadata som beskriver dokumentet.
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med resultatdata.
   * Ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

1. Hämta resultatet av åtgärden

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar en plats för en XML-fil som innehåller resultatdata. Kontrollera att filnamnstillägget är .xml.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som fylldes med resultatdata av metoden `generatePDFOutput` för objektet `OutputServiceService` (den åttonde parametern). Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `binaryData`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till XML-filen genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Skapa sökregler {#creating-search-rules}

Du kan skapa sökregler som resulterar i att Output-tjänsten undersöker indata och använder olika formulärdesigner baserade på datainnehållet för att generera utdata. Om texten *inteckning* till exempel finns i indata kan utdatatjänsten använda formulärdesignen Mortgage.xdp. Om texten *car* finns i indata kan Output-tjänsten använda en formulärdesign som har sparats som AutomobleLoan.xdp. Även om utdatatjänsten kan generera olika utdatatyper förutsätter det här avsnittet att utdatatjänsten genererar en PDF-fil. I följande diagram visas hur Output-tjänsten genererar en PDF-fil genom att bearbeta en XML-datafil och använda en av många formulärdesigner.

Dessutom kan utdatatjänsten generera dokumentpaket där flera poster finns i datauppsättningen och varje post matchas mot en formulärdesign och ett dokument skapas som består av flera formulärdesigner.

![cs_outputbatchmanyformdesigns2](assets/cs_outputbatchmanyformdesigns2.png)

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-8}

Så här instruerar du utdatatjänsten att använda sökregler när ett dokument skapas:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Referera till en XML-datakälla.
1. Definiera sökregler.
1. Ange körningsalternativ för PDF.
1. Ange alternativ för återgivning vid körning.
1. Skapa ett PDF-dokument.
1. Hämta resultatet av åtgärden.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

Följande JAR-filer måste läggas till i projektets klassökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms distribueras på JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på.

**Skapa ett Output Client-objekt**

Innan du programmässigt kan utföra en utdatatjänståtgärd måste du skapa ett klientobjekt för utdatatjänsten.

**Referera till en XML-datakälla**

Det måste finnas ett XML-element för varje formulärfält som du vill fylla i med data. XML-elementnamnet måste matcha fältnamnet. Ett XML-element ignoreras om det inte motsvarar ett formulärfält eller om XML-elementnamnet inte matchar fältnamnet. Det är inte nödvändigt att matcha den ordning i vilken XML-elementen visas, förutsatt att alla XML-element har angetts.

**Definiera sökregler**

Om du vill definiera sökregler definierar du ett eller flera textmönster som Output Services söker efter i indata. För varje textmönster som du definierar anger du en motsvarande formulärdesign som används om textmönstret finns. Om det finns ett textmönster använder Output-tjänsten motsvarande formulärdesign för att generera utdata. Ett exempel på ett textmönster är *inteckning*.

>[!NOTE]
>
>Om textmönster inte hittas används standardformuläret. Kontrollera att alla formulärdesigner som du använder finns i innehållsroten.

**Ange körningsalternativ för PDF**

Ange följande körningsalternativ för PDF för att Output-tjänsten ska kunna skapa ett PDF-dokument baserat på flera formulärdesigner:

* **Fil-URI**: Anger namn och plats för den PDF-fil som utdatatjänsten genererar.
* **Regler**: Anger regler som du har definierat.
* **LookAHead**: Anger antalet byte som ska användas från början av indatafilen för att söka efter definierade textmönster. Standardvärdet är 500 byte.

**Ange alternativ för återgivning vid körning**

Du kan ange alternativ för återgivning vid körning när du skapar PDF-filer. Även om dessa alternativ inte är nödvändiga (till skillnad från PDF körningsalternativ) kan du utföra åtgärder som att förbättra prestanda för utdatatjänsten. Du kan till exempel cachelagra formulärdesignen som Output-tjänsten använder för att förbättra prestandan.

**Skapa ett PDF-dokument**

När du har refererat till en giltig XML-datakälla och angett körningsalternativ kan du anropa utdatatjänsten, vilket resulterar i att ett PDF-dokument genereras. Om utdatatjänsten hittar ett angivet textmönster i indata används motsvarande formulärdesign. Om ett textmönster inte används används standardformulärdesignen.

**Hämta resultatet av åtgärden**

När utdatatjänsten har utfört en åtgärd returneras XML-data som anger om åtgärden lyckades.

**Se även**

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

### Skapa sökregler med Java API {#create-search-rules-using-the-java-api}

Skapa sökregler med hjälp av utdata-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till en XML-datakälla.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar XML-datakällan som används för att fylla i PDF-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för XML-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Definiera sökregler.

   * Skapa ett `Rule`-objekt med hjälp av dess konstruktor.
   * Definiera ett textmönster genom att anropa metoden `setPattern` för objektet `Rule` och skicka ett strängvärde som anger ett textmönster.
   * Definiera motsvarande formulärdesign genom att anropa `Rule`-objektets `setForm`-metod. Skicka ett strängvärde som anger formulärdesignens namn.

   >[!NOTE]
   >
   >För varje textmönster som du vill definiera upprepar du de tre föregående delstegen.

   * Skapa ett `java.util.List`-objekt med hjälp av en `java.util.ArrayList`-konstruktor.
   * För varje `Rule`-objekt som du har skapat anropar du `java.util.List`-objektets `add`-metod och skickar `Rule`-objektet.

1. Ange körningsalternativ för PDF.

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange namn och plats för den PDF-fil som utdatatjänsten genererar genom att anropa `PDFOutputOptionsSpec`-objektets `setFileURI`-metod. Skicka ett strängvärde som anger platsen för PDF-filen. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.
   * Ange reglerna som du definierade genom att anropa `PDFOutputOptionsSpec`-objektets `setRules`-metod. Skicka `java.util.List`-objektet som innehåller `Rule`-objekten.
   * Ange antalet byte som ska genomsökas efter definierade textmönster genom att anropa `PDFOutputOptionsSpec`-objektets `setLookAhead`-metod. Skicka ett heltalsvärde som representerar antalet byte.

1. Ange alternativ för återgivning vid körning.

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Cachelagra formulärdesignen för att förbättra utdatatjänstens prestanda genom att anropa `setCacheEnabled` för `RenderOptionsSpec`-objektet och skicka `true`.

1. Skapa ett PDF-dokument.

   Generera ett PDF-dokument som baseras på flera formulärdesigner genom att anropa `OutputClient`-objektets `generatePDFOutput`-metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger namnet på standardformulärdesignen. Det vill säga den formulärdesign som används om det inte finns något textmönster.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `com.adobe.idp.Document` som innehåller formulärdata som söks igenom av utdatatjänsten efter definierade textmönster.

   Metoden `generatePDFOutput` returnerar ett `OutputResult`-objekt som innehåller resultatet av åtgärden.

1. Hämta resultatet av åtgärden.

   * Skapa ett `com.adobe.idp.Document`-objekt som representerar `generatePDFOutput`-metodens status genom att anropa `OutputResult`-objektets `getStatusDoc`-metod.
   * Skapa ett `java.io.File`-objekt som innehåller resultatet av åtgärden. Kontrollera att filtillägget är .xml.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `getStatusDoc`).

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): Skapa sökregler med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-search-rules-using-the-java-api)

[Snabbstart (SOAP-läge): Skapa sökregler med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-search-rules-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Skapa sökregler med webbtjänstens API {#create-search-rules-using-the-web-service-api}

Skapa sökregler med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Referera till en XML-datakälla.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra data som ska sammanfogas med PDF-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det PDF-dokument som ska krypteras och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Definiera sökregler.

   * Skapa ett `Rule`-objekt med hjälp av dess konstruktor.
   * Definiera ett textmönster genom att tilldela ett strängvärde som anger ett textmönster till `pattern`-objektets `Rule`-datamedlem.
   * Definiera motsvarande formulärdesign genom att tilldela ett strängvärde som anger formulärdesignen till `form`-objektets `Rule`-datamedlem.

   >[!NOTE]
   >
   >För varje textmönster som du vill definiera upprepar du de tre föregående delstegen.

   * Skapa ett `MyArrayOf_xsd_anyType`-objekt som lagrar reglerna.
   * Tilldela varje `Rule`-objekt till ett element i arrayen `MyArrayOf_xsd_anyType`. Anropa `MyArrayOf_xsd_anyType`-objektets `Add`-metod för varje `Rule`-objekt.

1. Ange körningsalternativ för PDF

   * Skapa ett `PDFOutputOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Ange fil-URI-alternativet genom att tilldela ett strängvärde som anger platsen för den PDF-fil som utdatatjänsten genererar till `fileURI`-objektets `PDFOutputOptionsSpec`-datamedlem. Alternativet Fil-URI är relativt till J2EE-programservern som är värd för AEM Forms, inte klientdatorn.
   * Ange alternativet för kopior genom att tilldela ett heltalsvärde som anger antalet kopior som utdatatjänsten genererar till `copies`-objektets `PDFOutputOptionsSpec`-datamedlem.
   * Ange reglerna som du definierade genom att tilldela `MyArrayOf_xsd_anyType`-objektet som lagrar reglerna till `PDFOutputOptionsSpec`-objektets `rules`-datamedlem.
   * Ange antalet byte som ska genomsökas efter definierade textmönster genom att tilldela ett heltalsvärde som representerar antalet byte som ska genomsökas till `lookAhead`-objektets `PDFOutputOptionsSpec`-datametod.

1. Ange alternativ för återgivning vid körning

   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor.
   * Cachelagra formulärdesignen för att förbättra utdatatjänstens prestanda genom att tilldela värdet `true` till `RenderOptionsSpec`-objektets `cacheEnabled`-datamedlem.

   >[!NOTE]
   >
   >Du kan inte ange version av PDF-dokumentet med `RenderOptionsSpec`-objektets `pdfVersion`-medlem om indatadokumentet är ett Acrobat-formulär. Utdata-PDF-dokumentet behåller PDF-versionen av Acrobat-formuläret. På samma sätt kan du inte ange alternativet taggad PDF med `RenderOptionsSpec`-objektets `taggedPDF`-metod om indatadokumentet är ett Acrobat-formulär.

   >[!NOTE]
   >
   >Du kan inte ange linjärt PDF-alternativ med `RenderOptionsSpec`-objektets `linearizedPDF`-medlem om PDF-indatadokumentet är certifierat eller digitalt signerat. Mer information finns i [Signera PDF-dokument digitalt](/help/forms/developing/digitally-signing-certifying-documents.md#digitally-signing-pdf-documents).

1. Skapa ett PDF-dokument

   Skapa ett PDF-dokument genom att anropa `OutputServiceService`-objektets `generatePDFOutput`metod och skicka följande värden:

   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett strängvärde som anger formulärdesignens namn.
   * Ett strängvärde som anger innehållsroten där formulärdesignen finns.
   * Ett `PDFOutputOptionsSpec`-objekt som innehåller körningsalternativ för PDF.
   * Ett `RenderOptionsSpec`-objekt som innehåller alternativ för återgivning vid körning.
   * Objektet `BLOB` som innehåller XML-datakällan som innehåller data som ska sammanfogas med formulärdesignen.
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med genererade metadata som beskriver dokumentet. (Det här parametervärdet krävs bara för webbtjänstanrop).
   * Ett `BLOB`-objekt som fylls i av metoden `generatePDFOutput`. Metoden `generatePDFOutput` fyller i det här objektet med resultatdata. (Det här parametervärdet krävs bara för webbtjänstanrop).
   * Ett `OutputResult`-objekt som innehåller resultatet av åtgärden. (Det här parametervärdet krävs bara för webbtjänstanrop).

   >[!NOTE]
   >
   >När du genererar ett PDF-dokument genom att anropa metoden `generatePDFOutput` kan du inte sammanfoga data med ett XFA PDF-formulär som är signerat, certifierat eller innehåller användningsrättigheter. Mer information om användningsrättigheter finns i [Använda användningsrättigheter i PDF-dokument](/help/forms/developing/assigning-usage-rights.md#applying-usage-rights-to-pdf-documents).

1. Hämta resultatet av åtgärden

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar en plats för en XML-fil som innehåller resultatdata. Kontrollera att filtillägget är XML.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som fylldes med resultatdata av metoden `generatePDFOutput` för objektet `OutputServiceService` (den åttonde parametern). Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till XML-filen genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Förenkla PDF-dokument {#flattening-pdf-documents}

Du kan använda Output-tjänsten för att omvandla ett interaktivt PDF-dokument till en icke-interaktiv PDF. Med ett interaktivt PDF-dokument kan användare ange eller ändra data som finns i PDF dokumentfält. Processen att omforma ett interaktivt PDF-dokument till ett icke-interaktivt PDF-dokument kallas *förenkling*. När ett PDF-dokument förenklas kan användaren inte ändra data i dokumentfälten. Ett skäl till att förenkla ett PDF-dokument är att se till att data inte kan ändras.

Du kan förenkla följande typer av PDF-dokument:

* Interaktiva XFA PDF-dokument
* Acrobat Forms

Om du försöker förenkla ett PDF som är ett icke-interaktivt PDF-dokument genereras ett undantag.

>[!NOTE]
>
>Mer information om utdatatjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-9}

Så här förenklar du ett interaktivt PDF-dokument till ett icke-interaktivt PDF-dokument:

1. Inkludera projektfiler.
1. Skapa ett Output Client-objekt.
1. Hämta ett interaktivt PDF-dokument.
1. Omforma PDF-dokumentet.
1. Spara det icke-interaktiva PDF-dokumentet som en PDF-fil.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du ta med proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-output-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBoss, måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på. Mer information om platsen för alla AEM Forms JAR-filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).

**Skapa ett Output Client-objekt**

Innan du programmässigt kan utföra en utdatatjänståtgärd måste du skapa ett klientobjekt för utdatatjänsten. Om du använder Java API skapar du ett `OutputClient`-objekt. Skapa ett `OutputServiceService`-objekt om du använder API:t för webbtjänsten för utdata.

**Hämta ett interaktivt PDF-dokument**

Hämta ett interaktivt PDF-dokument som du vill omvandla till ett icke-interaktivt PDF-dokument. Om du försöker omforma ett icke-interaktivt PDF-dokument genereras ett undantagsfel.

**Omforma PDF-dokumentet**

När du har hämtat ett interaktivt PDF-dokument kan du omvandla det till ett icke-interaktivt PDF-dokument. Output-tjänsten returnerar ett icke-interaktivt PDF-dokument.

**Spara det icke-interaktiva PDF-dokumentet som en PDF-fil**

Du kan spara det icke-interaktiva PDF-dokumentet som en PDF-fil.

**Se även**

[Förenkla ett PDF-dokument med Java API](creating-document-output-streams.md#flatten-a-pdf-document-using-the-java-api)

[Förenkla ett PDF-dokument med hjälp av webbtjänstens API](creating-document-output-streams.md#flatten-a-pdf-document-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för Output Service API](/help/forms/developing/output-service-java-api-quick.md#output-service-java-api-quick-start-soap)

### Förenkla ett PDF-dokument med Java API {#flatten-a-pdf-document-using-the-java-api}

Förenkla ett interaktivt PDF-dokument till ett icke-interaktivt PDF-dokument med hjälp av utdata-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, t.ex. adobe-output-client.jar, i Java-projektets klassökväg.

1. Skapa ett Output Client-objekt.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `OutputClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Hämta ett interaktivt PDF-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar det interaktiva PDF-dokumentet som ska omformas med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för den interaktiva PDF-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Omforma PDF-dokumentet.

   Omvandla det interaktiva PDF-dokumentet till ett icke-interaktivt PDF-dokument genom att anropa `OutputServiceService`-objektets `transformPDF`-metod och skicka följande värden:

   * Objektet `com.adobe.idp.Document` som innehåller det interaktiva PDF-dokumentet.
   * Ett enum-värde på `TransformationFormat`. Om du vill generera ett icke-interaktivt PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett `PDFARevisionNumber`-uppräkningsvärde som anger revisionsnumret. Eftersom den här parametern är avsedd för ett PDF/A-dokument kan du ange `null`.
   * Ett strängvärde som representerar ändringsnumret och året, avgränsat med ett kolon. Eftersom den här parametern är avsedd för ett PDF/A-dokument kan du ange `null`.
   * Ett enum-värde på `PDFAConformance` som representerar PDF/A-överensstämmelsenivån. Eftersom den här parametern är avsedd för ett PDF/A-dokument kan du ange `null`.

   Metoden `transformPDF` returnerar ett `com.adobe.idp.Document`-objekt som innehåller ett icke-interaktivt PDF-dokument.

1. Spara det icke-interaktiva PDF-dokumentet som en PDF-fil.

   * Skapa ett `java.io.File`-objekt och kontrollera att filnamnstillägget är .pdf.
   * Anropa `Document`-objektets `copyToFile`-metod för att kopiera innehållet i `Document`-objektet till filen (se till att du använder det `Document`-objekt som returnerades av metoden `transformPDF`).

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Snabbstart (EJB-läge): Omforma ett PDF-dokument med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-transforming-a-pdf-document-using-the-java-api)

[Snabbstart (SOAP-läge): Omforma ett PDF-dokument med Java API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-transforming-a-pdf-document-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Förenkla ett PDF-dokument med hjälp av webbtjänstens API {#flatten-a-pdf-document-using-the-web-service-api}

Förenkla ett interaktivt PDF-dokument till ett icke-interaktivt PDF-dokument med hjälp av Output API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/OutputService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett Output Client-objekt.

   * Skapa ett `OutputServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `OutputServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/OutputService?blob=mtom`.) Du behöver inte använda attributet `lc_version`. Det här attributet används när du skapar en tjänstreferens. Ange `?blob=mtom` om du vill använda MTOM.
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `OutputServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `OutputServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `OutputServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Hämta ett interaktivt PDF-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra det interaktiva PDF-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det interaktiva PDF-dokumentet.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.

1. Omforma PDF-dokumentet.

   Omvandla det interaktiva PDF-dokumentet till ett icke-interaktivt PDF-dokument genom att anropa `OutputClient`-objektets `transformPDF`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som innehåller det interaktiva PDF-dokumentet.
   * Ett `TransformationFormat`-uppräkningsvärde. Om du vill generera ett icke-interaktivt PDF-dokument anger du `TransformationFormat.PDF`.
   * Ett `PDFARevisionNumber`-uppräkningsvärde som anger revisionsnumret.
   * Ett booleskt värde som anger om enum-värdet `PDFARevisionNumber` används. Eftersom den här parametern är avsedd för ett PDF/A-dokument kan du ange `false`.
   * Ett strängvärde som representerar ändringsnumret och året, avgränsat med ett kolon. Eftersom den här parametern är avsedd för ett PDF/A-dokument kan du ange `null`.
   * Ett enum-värde på `PDFAConformance` som representerar PDF/A-överensstämmelsenivån.
   * Booleskt värde som anger om enum-värdet `PDFAConformance` används. Eftersom den här parametern är avsedd för ett PDF/A-dokument kan du ange `false`.

   Metoden `transformPDF` returnerar ett `BLOB`-objekt som innehåller ett icke-interaktivt PDF-dokument.

1. Spara det icke-interaktiva PDF-dokumentet som en PDF-fil.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det icke-interaktiva PDF-dokumentet.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som returnerades av metoden `transformPDF`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till en PDF-fil genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Sammanfattning av steg](creating-document-output-streams.md#summary-of-steps)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)
