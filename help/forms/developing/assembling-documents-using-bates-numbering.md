---
title: Sammanställa dokument med Bates-numrering
description: Använd Bates-numrering för att sammanställa PDF-dokument med Java- och Web Service API.
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
exl-id: 310d0566-673a-4b78-9f0d-86f56c495105
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1910'
ht-degree: 0%

---

# Sammanställa dokument med Bates-numrering {#assembling-documents-using-bates-numbering}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Du kan sammanställa PDF-dokument som innehåller unika sididentifierare med hjälp av Bates-numrering. *Bates-numrering* är en metod för att tillämpa unika identifierare på en grupp med relaterade dokument. Varje sida i dokumentet (eller dokumentuppsättningen) tilldelas ett Bates-nummer som unikt identifierar sidan. Till exempel kan tillverkningsdokument som innehåller produktstruktur och som är kopplade till produktionen av en sammansättning innehålla en identifierare. Ett Bates-nummer innehåller ett numeriskt värde i följd samt ett valfritt prefix och suffix. Prefixet + det numeriska + suffixet kallas *bates-mönster*.

Följande bild visar ett PDF-dokument som innehåller en unik identifierare i dokumentets sidhuvud.

![au_au_batesnumber](assets/au_au_batesnumber.png)

I det här avsnittet placeras den unika sididentifieraren i ett dokuments sidhuvud. Anta att följande DDX-dokument används.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
        <PDF result="out.pdf">
        <Header>
         <Center>
             <StyledText>
                 <p font-size="20pt"><BatesNumber/></p>
             </StyledText>
         </Center>
     </Header>
           <PDF source="map.pdf" />
          <PDF source="directions.pdf" />
          </PDF>
 </DDX>
```

Det här DDX-dokumentet sammanfogar två PDF-dokument med namnen *map.pdf* och *directs.pdf* till ett enda PDF-dokument. Det resulterande PDF-dokumentet innehåller ett sidhuvud som består av en unik sididentifierare. Dokumentet i ovanstående bild visar till exempel 000016.

>[!NOTE]
>
>Innan du läser det här avsnittet bör du känna till hur du sammanställer PDF-dokument med hjälp av tjänsten Assembler. I det här avsnittet beskrivs inte begreppen, till exempel att skapa ett samlingsobjekt som innehåller indatadokument eller att extrahera resultaten från det returnerade samlingsobjektet. (Se [Programmatisk sammansättning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md).)

>[!NOTE]
>
>Mer information om Assembler-tjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

>[!NOTE]
>
>Mer information om ett DDX-dokument finns i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

## Sammanfattning av steg {#summary-of-steps}

Så här sammanställer du ett PDF-dokument som innehåller en unik sididentifierare (Bates-numrering):

1. Inkludera projektfiler.
1. Skapa en PDF Assembler-klient.
1. Referera till ett befintligt DDX-dokument.
1. Referera till PDF-dokument.
1. Ange det inledande Bates-nummervärdet.
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

Om AEM Forms körs på en annan J2EE-programserver än JBoss måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern där AEM Forms är driftsatt. Mer information om platsen för alla AEM Forms JAR-filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).

**Skapa en PDF Assembler-klient**

Innan du programmässigt kan utföra en Assembler-åtgärd måste du skapa en Assembler-tjänstklient.

**Referera till ett befintligt DDX-dokument**

Det måste finnas referenser till ett DDX-dokument för att du ska kunna sammanställa ett PDF-dokument. Ta till exempel det DX-dokument som introducerades i det här avsnittet. Om du vill montera ett PDF-dokument som innehåller unika sididentifierare måste DDX-dokumentet innehålla elementet `BatesNumber`.

**Referensindata för PDF-dokument**

Indata-PDF-dokument måste refereras till för att du ska kunna sammanställa ett PDF-dokument. Till exempel måste dokumenten map.pdf och directs.pdf refereras för att samla dessa PDF-dokument i ett enda PDF-dokument.

**Ange startvärde för Bates-nummer**

Du kan ställa in det inledande Bates-numret så att det uppfyller dina affärskrav. Anta till exempel att det är ett krav att ställa in startvärdet på 000100. Om du inte anger startvärdet är värdet för den första sidan 00000.

**Sammanställ indata-PDF-dokument**

När du har skapat Assembler-tjänstklienten, refererar till DDX-dokumentet som innehåller `BatesNumber`-elementinformation, refererar till ett PDF-indatadokument och anger körningsalternativ, kan du anropa `invokeDDX` -åtgärden som resulterar i att Assembler-tjänsten samlar ihop ett PDF-dokument som innehåller unika sididentifierare.

**Extrahera resultaten**

Assembler-tjänsten returnerar ett samlingsobjekt som innehåller jobbresultaten. Du kan extrahera det resulterande PDF-dokumentet och eventuella undantag som genereras. I det här fallet finns ett krypterat PDF-dokument i samlingsobjektet.

>[!NOTE]
>
>Ett samlingsobjekt returneras om du anropar åtgärden `invokeDDX`. Den här åtgärden används när två eller flera indatadokument för PDF skickas till Assembler-tjänsten. Om du bara skickar ett PDF-indatadokument till Assembler-tjänsten bör du anropa åtgärden `invokeOneDocument`. Mer information om hur du använder den här åtgärden finns i [Sammanställa krypterade PDF-dokument](/help/forms/developing/assembling-encrypted-pdf-documents.md).

**Se även**

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Programmisk sammanställning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md)

## Sammanställa dokument med Bates-numrering med Java API {#assemble-documents-with-bates-numbering-using-the-java-api}

Sammanställa ett PDF-dokument som använder unika sididentifierare (Bates-numrering) med hjälp av Assembler Service API (Java):

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
   * Skapa ett `java.io.FileInputStream`-objekt för varje PDF-indatadokument genom att använda dess konstruktor och skicka platsen för PDF-indatadokumentet. I så fall skickar du platsen för ett oskyddat PDF-dokument.
   * Skapa ett `com.adobe.idp.Document`-objekt för varje PDF-indatadokument och skicka det `java.io.FileInputStream`-objekt som innehåller PDF-dokumentet.
   * Lägg till en post i objektet `java.util.Map` genom att anropa dess `put`-metod och skicka följande argument:

      * Ett strängvärde som representerar nyckelnamnet. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet. Namnet på PDF-källfilen som anges i DDX-dokumentet som introduceras i det här avsnittet är Loan.pdf.
      * Ett `com.adobe.idp.Document`-objekt som innehåller det oskyddade PDF-dokumentet.

1. Ange det inledande Bates-nummervärdet.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange det inledande Bates-numret genom att anropa `setFirstBatesNumber` för objektet `AssemblerOptionSpec` och skicka ett numeriskt värde som anger det inledande värdet.

1. Sammanställ PDF-indata.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande obligatoriska värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar DDX-dokumentet.
   * Ett `java.util.Map`-objekt som innehåller den oskyddade indatafilen för PDF.
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen, inklusive standardteckensnitt och jobbloggsnivå.

   Metoden `invokeDDX` returnerar ett `com.adobe.livecycle.assembler.client.AssemblerResult`-objekt som innehåller ett lösenordskrypterat PDF-dokument.

1. Extrahera resultaten.

   Gör så här för att hämta det nya PDF-dokumentet:

   * Anropa metoden `getDocuments` för objektet `AssemblerResult`. Den här åtgärden returnerar ett `java.util.Map`-objekt.
   * Upprepa genom objektet `java.util.Map` tills du hittar objektet `com.adobe.idp.Document`.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att extrahera PDF-dokumentet.

**Se även**

[Snabbstart (SOAP-läge): Sammanställa ett PDF-dokument med Bates-numrering med Java API](/help/forms/developing/assembler-service-java-api-quick.md#quick-start-soap-mode-assembling-a-pdf-document-with-bates-numbering-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Sammanställa dokument med Bates-numrering med webbtjänstens API {#assemble-documents-with-bates-numbering-using-the-web-service-api}

Sammanställa ett PDF-dokument som använder unika sididentifierare (Bates-numrering) med Assembler Service API (webbtjänst):

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
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod. Skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Referera till PDF-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor för varje indata-PDF-dokument. Objektet `BLOB` används för att lagra indata-PDF-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod. Skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType`-objekt. Det här samlingsobjektet används för att lagra indata-PDF-dokument.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt för varje PDF-indatadokument. Om du till exempel använder två PDF-indatadokument skapar du två `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt.
   * Tilldela ett strängvärde som representerar nyckelnamnet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `key`-fält. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet. (Utför den här åtgärden för varje PDF-indatadokument.)
   * Tilldela objektet `BLOB` som lagrar PDF-dokumentet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `value`-fält. (Utför den här åtgärden för varje PDF-indatadokument.)
   * Lägg till objektet `MyMapOf_xsd_string_To_xsd_anyType_Item` i objektet `MyMapOf_xsd_string_To_xsd_anyType`. Anropa `MyMapOf_xsd_string_To_xsd_anyType`-objektets `Add`-metod och skicka `MyMapOf_xsd_string_To_xsd_anyType`-objektet. (Utför den här åtgärden för varje PDF-indatadokument.)

1. Ange det inledande Bates-nummervärdet.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange det inledande Bates-numret genom att tilldela datamedlemmen `firstBatesNumber` som tillhör objektet `AssemblerOptionSpec` ett numeriskt värde.

1. Sammanställ PDF-indata.

   Anropa `AssemblerServiceClient`-objektets `invoke`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet.
   * Objektet `MyMapOf_xsd_string_To_xsd_anyType` som innehåller indata-PDF-dokument. Dess nycklar måste matcha namnen på PDF-källfilerna och dess värden måste vara de `BLOB`-objekt som motsvarar dessa filer.
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ.

   Metoden `invoke` returnerar ett `AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som inträffade.

1. Extrahera resultaten.

   Gör så här för att hämta det nya PDF-dokumentet:

   * Få åtkomst till `documents`-fältet för `AssemblerResult`-objektet, som är ett `Map`-objekt som innehåller PDF-dokument.
   * Upprepa genom objektet `Map` tills du hittar nyckeln som matchar namnet på det resulterande dokumentet. Byt sedan `value` för den arraymedlemmen till en `BLOB`.
   * Extrahera de binära data som representerar PDF-dokumentet genom att komma åt objektets `MTOM`-egenskap `BLOB`. Detta returnerar en array med byte som du kan skriva till en PDF-fil.

**Se även**

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
