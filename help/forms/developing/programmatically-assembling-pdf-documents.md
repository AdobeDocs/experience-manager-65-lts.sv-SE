---
title: Programmisk sammanställning av PDF-dokument
description: Använd API:t för Assembler-tjänsten för att samla ihop flera PDF-dokument till ett enda PDF-dokument med hjälp av Java API:t och webbtjänstens API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/assembling_pdf_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services
hide: true
hidefromtoc: true
exl-id: a64f0bd4-8d5c-485d-b620-608bfe33a72f
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '2105'
ht-degree: 0%

---

# Programmisk sammanställning av PDF-dokument {#programmatically-assembling-pdf-documents}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Du kan använda Assembler Service API för att samla ihop flera PDF-dokument till ett enda PDF-dokument. Följande bild visar tre PDF-dokument som sammanfogas till ett enda PDF-dokument.

![pa_pa_document_assembly](assets/pa_pa_document_assembly.png)

Om du vill samla ihop två eller flera PDF-dokument till ett enda PDF-dokument behöver du ett DX-dokument. Ett DDX-dokument beskriver det PDF-dokument som Assembler-tjänsten skapar. DX-dokumentet instruerar alltså Assembler-tjänsten vilka åtgärder som ska utföras.

Anta att följande DDX-dokument används för den här diskussionen.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
     <PDF result="out.pdf">
         <PDF source="map.pdf" />
         <PDF source="directions.pdf" />
     </PDF>
 </DDX>
```

Det här DDX-dokumentet sammanfogar två PDF-dokument med namnen *map.pdf* och *directs.pdf* till ett enda PDF-dokument.

>[!NOTE]
>
>Om du vill se ett DX-dokument som demonterar ett PDF-dokument läser du [Dela upp PDF-dokument programmatiskt](/help/forms/developing/programmatically-disassembling-pdf-documents.md#programmatically-disassembling-pdf-documents).

>[!NOTE]
>
>Mer information om Assembler-tjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

>[!NOTE]
>
>Mer information om ett DDX-dokument finns i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

## Att tänka på vid anrop av Assembler-tjänsten med webbtjänster {#considerations-when-invoking-assembler-service-using-web-services}

När du lägger till sidhuvuden och sidfötter under sammanställningen av stora dokument kan ett `OutOfMemory`-fel uppstå, och filerna kommer inte att sättas samman. Om du vill minska risken för att det här problemet uppstår lägger du till ett `DDXProcessorSetting`-element i ditt DDX-dokument, vilket visas i följande exempel.

`<DDXProcessorSetting name="checkpoint" value="2000" />`

Du kan lägga till det här elementet som ett underordnat element till elementet `DDX` eller som ett underordnat element till ett `PDF result` -element. Standardvärdet för den här inställningen är 0 (noll), vilket inaktiverar kontrollpunkten och DDX fungerar som om elementet `DDXProcessorSetting` inte finns. Om du har påträffat ett `OutOfMemory`-fel kan du behöva ange värdet till ett heltal, vanligtvis mellan 500 och 5000. Ett litet kontrollpunktsvärde resulterar i mer frekventa kontrollpunkter.

## Sammanfattning av steg {#summary-of-steps}

Gör så här om du vill samla ihop ett PDF-dokument från flera PDF-dokument:

1. Inkludera projektfiler.
1. Skapa en PDF Assembler-klient.
1. Referera till ett befintligt DDX-dokument.
1. Referera till PDF-dokument.
1. Ange körningsalternativ.
1. Sammanställ PDF-indata.
1. Extrahera resultaten.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-assembler-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

Om AEM Forms körs på en annan J2EE-programserver än JBoss måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern där AEM Forms är distribuerad.

**Skapa en PDF Assembler-klient**

Innan du programmässigt kan utföra en Assembler-åtgärd måste du skapa en Assembler-klient.

**Referera till ett befintligt DDX-dokument**

Det måste finnas referenser till ett DDX-dokument för att du ska kunna sammanställa ett PDF-dokument. Ta till exempel det DX-dokument som introducerades i det här avsnittet. Det här DDX-dokumentet instruerar Assembler-tjänsten att sammanfoga två PDF-dokument till ett enda PDF-dokument.

**Referensindata för PDF-dokument**

Referera till de PDF-dokument som du vill skicka till Assembler-tjänsten. Om du till exempel vill skicka två indatadokument för PDF med namnen Karta och Riktningar måste du skicka motsvarande PDF-filer.

Både filen map.pdf och filen Directions.pdf måste placeras i ett samlingsobjekt. Namnet på nyckeln måste matcha värdet på PDF-källattributet i DDX-dokumentet. Det spelar ingen roll vad namnet på PDF-filen är om nyckeln och källattributet i DDX-dokumentet matchar.

>[!NOTE]
>
>Ett `AssemblerResult`-objekt, som innehåller ett samlingsobjekt, returneras om du anropar åtgärden `invokeDDX`. Den här åtgärden används när du skickar två eller flera PDF-indatadokument till Assembler-tjänsten. Om du bara skickar ett PDF-indata till Assembler-tjänsten och bara förväntar dig ett returdokument, ska du anropa åtgärden `invokeOneDocument`. När den här åtgärden anropas returneras ett enda dokument. Mer information om hur du använder den här åtgärden finns i [Sammanställa krypterade PDF-dokument](/help/forms/developing/assembling-encrypted-pdf-documents.md#assembling-encrypted-pdf-documents).

**Ange körningsalternativ**

Du kan ställa in körningsalternativ som styr beteendet för Assembler-tjänsten när den utför ett jobb. Du kan till exempel ange ett alternativ som instruerar Assembler-tjänsten att fortsätta bearbeta ett jobb om ett fel uppstår. Mer information om de körningsalternativ du kan ange finns i klassreferensen `AssemblerOptionSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

**Sammanställ indata-PDF-dokument**

När du har skapat tjänstklienten, refererat till en DDX-fil, skapat ett samlingsobjekt som lagrar indata-PDF-dokument och angett körningsalternativ, kan du starta DDX-åtgärden. När du använder det DDX-dokument som anges i det här avsnittet sammanfogas filerna map.pdf och direction.pdf till ett PDF-dokument.

**Extrahera resultaten**

Assembler-tjänsten returnerar ett `java.util.Map`-objekt som kan hämtas från `AssemblerResult`-objektet och som innehåller åtgärdsresultat. Det returnerade `java.util.Map`-objektet innehåller de resulterande dokumenten och eventuella undantag.

I följande tabell sammanfattas några av de nyckelvärden och objekttyper som kan finnas i det returnerade `java.util.Map`-objektet.

<table>
 <thead>
  <tr>
   <th><p>Nyckelvärde</p></th>
   <th><p>Objekttyp</p></th>
   <th><p>Beskrivning</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p><code><i>documentName</i></code></p></td>
   <td><p><code>com.adobe.idp.Document</code></p></td>
   <td><p>Innehåller de resulterande dokument som anges i ett DDX-resultatelement</p></td>
  </tr>
  <tr>
   <td><p><code><i>documentName</i></code></p></td>
   <td><p><code>Exception</code></p></td>
   <td><p>Innehåller alla undantag för dokumentet</p></td>
  </tr>
  <tr>
   <td><p><code>OutputMapConstants.LOG_NAME</code></p></td>
   <td><p><code>com.adobe.idp.Documen</code></p></td>
   <td><p>Innehåller jobbloggen</p></td>
  </tr>
 </tbody>
</table>

**Se även**

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Dela upp PDF-dokument programmatiskt](/help/forms/developing/programmatically-disassembling-pdf-documents.md#programmatically-disassembling-pdf-documents)

## Sammanställa PDF-dokument med Java API {#assemble-pdf-documents-using-the-java-api}

Sammanställa ett PDF-dokument med Assembler Service API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-assembler-client.jar, i Java-projektets klassökväg.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar DDX-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för DDX-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Referera till PDF-dokument.

   * Skapa ett `java.util.Map`-objekt som används för att lagra indata-PDF-dokument med hjälp av en `HashMap`-konstruktor.
   * Skapa ett `java.io.FileInputStream`-objekt för varje PDF-indatadokument genom att använda dess konstruktor och skicka platsen för PDF-indatadokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt för varje PDF-indatadokument och skicka det `java.io.FileInputStream`-objekt som innehåller PDF-dokumentet.
   * Lägg till en post till objektet `java.util.Map` för varje indatadokument genom att anropa dess `put`-metod och skicka följande argument:

      * Ett strängvärde som representerar nyckelnamnet. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet.
      * Ett `com.adobe.idp.Document`-objekt (eller `java.util.List`-objekt som anger flera dokument) som innehåller PDF-källdokumentet.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ som uppfyller dina affärskrav genom att anropa en metod som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar, anropar du `AssemblerOptionSpec`-objektets `setFailOnError`-metod och skickar `false`.

1. Sammanställ PDF-indata.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande obligatoriska värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar det DDX-dokument som ska användas
   * Ett `java.util.Map`-objekt som innehåller de PDF-indatafiler som ska monteras
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen, inklusive standardteckensnitt och jobbloggsnivå

   Metoden `invokeDDX` returnerar ett `com.adobe.livecycle.assembler.client.AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som inträffade.

1. Extrahera resultaten.

   Gör så här för att hämta det nya PDF-dokumentet:

   * Anropa metoden `getDocuments` för objektet `AssemblerResult`. Detta returnerar ett `java.util.Map`-objekt.
   * Upprepa genom objektet `java.util.Map` tills du hittar det resulterande `com.adobe.idp.Document`-objektet. (Du kan använda PDF-resultatelementet som anges i DDX-dokumentet för att hämta dokumentet.)
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att extrahera PDF-dokumentet.

   >[!NOTE]
   >
   >Om `LOG_LEVEL` var inställt på att generera en logg kan du extrahera loggen med hjälp av `AssemblerResult`-objektets `getJobLog`-metod.

**Se även**

[Snabbstart (SOAP-läge): Sammanställa ett PDF-dokument med Java API](/help/forms/developing/assembler-service-java-api-quick.md#quick-start-soap-mode-assembling-a-pdf-document-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Sammanställa PDF-dokument med hjälp av API:t för webbtjänster {#assemble-pdf-documents-using-the-web-service-api}

Sammanställa PDF-dokument med Assembler Service API (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/AssemblerService?WSDL&lc_version=9.0.1`.

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

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra DDX-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för DDX-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.

1. Referera till PDF-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor för varje indata-PDF-dokument. Objektet `BLOB` används för att lagra indata-PDF-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod. Skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType`-objekt. Det här samlingsobjektet används för att lagra indata-PDF-dokument.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt för varje PDF-indatadokument. Om du till exempel använder två PDF-indatadokument skapar du två `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt.
   * Tilldela ett strängvärde som representerar nyckelnamnet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `key`-fält. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet. (Utför den här åtgärden för varje PDF-indatadokument.)
   * Tilldela objektet `BLOB` som lagrar PDF-dokumentet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `value`-fält. (Utför den här åtgärden för varje PDF-indatadokument.)
   * Lägg till objektet `MyMapOf_xsd_string_To_xsd_anyType_Item` i objektet `MyMapOf_xsd_string_To_xsd_anyType`. Anropa `MyMapOf_xsd_string_To_xsd_anyType`-objektets `Add`-metod och skicka `MyMapOf_xsd_string_To_xsd_anyType`-objektet. (Utför den här åtgärden för varje PDF-indatadokument.)

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ för att uppfylla dina affärskrav genom att tilldela ett värde till en datamedlem som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar tilldelar du `false` till `AssemblerOptionSpec`-objektets `failOnError`-datamedlem.

1. Sammanställ PDF-indata.

   Anropa `AssemblerServiceClient`-objektets `invoke`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet.
   * Arrayen `mapItem` som innehåller indata-PDF-dokument. Dess nycklar måste matcha namnen på PDF-källfilerna och dess värden måste vara de `BLOB`-objekt som motsvarar dessa filer.
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ.

   Metoden `invoke` returnerar ett `AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som kan ha inträffat.

1. Extrahera resultaten.

   Gör så här för att hämta det nya PDF-dokumentet:

   * Öppna `AssemblerResult`-objektets `documents`-fält, som är ett `Map`-objekt som innehåller PDF-dokumentets resultat.
   * Upprepa genom objektet `Map` tills du hittar nyckeln som matchar namnet på det resulterande dokumentet. Byt sedan `value` för den arraymedlemmen till en `BLOB`.
   * Extrahera de binära data som representerar PDF-dokumentet genom att komma åt objektets `BLOB`-egenskap `MTOM`. Detta returnerar en array med byte som du kan skriva till en PDF-fil.

   >[!NOTE]
   >
   >Om `LOG_LEVEL` var inställt på att skapa en logg kan du extrahera loggen genom att hämta värdet för `AssemblerResult`-objektets `jobLog`-datamedlem.

**Se även**

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
