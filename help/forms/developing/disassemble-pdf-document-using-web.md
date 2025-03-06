---
title: Dela upp ett PDF-dokument med webbtjänstens API
description: Dela upp ett PDF-dokument med Assembler Service API
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/programmatically_disassembling_pdf_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
exl-id: 35802b50-71ca-4bb2-8f2c-7452b81a6b1f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 0%

---

# Dela upp ett PDF-dokument med hjälp av webbtjänstens API {#disassemble-a-pdf-document-usingthe-web-service-api}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Dela upp ett PDF-dokument med Assembler Service API (webbtjänst):

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

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra DDX-dokumentet.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för DDX-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.

1. Referera till ett PDF-dokument som ska demonteras.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra indata-PDF-dokumentet. Det här `BLOB`-objektet skickas till `invokeOneDocument` som ett argument.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll i `BLOB`-objektet genom att tilldela dess `MTOM`-fält innehållet i bytearrayen.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType`-objekt. Det här samlingsobjektet används för att lagra PDF som ska demonteras.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt.
   * Tilldela ett strängvärde som representerar nyckelnamnet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `key`-fält. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet.
   * Tilldela objektet `BLOB` som lagrar PDF-dokumentet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `value`-fält.
   * Lägg till objektet `MyMapOf_xsd_string_To_xsd_anyType_Item` i objektet `MyMapOf_xsd_string_To_xsd_anyType`. Anropa `MyMapOf_xsd_string_To_xsd_anyType`-objektets `Add`-metod och skicka `MyMapOf_xsd_string_To_xsd_anyType`-objektet.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ för att uppfylla dina affärskrav genom att tilldela ett värde till en datamedlem som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar tilldelar du `false` till `AssemblerOptionSpec`-objektets `failOnError`-fält.

1. Disassemblera PDF-dokumentet.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet som demonterar PDF-dokumentet
   * Objektet `MyMapOf_xsd_string_To_xsd_anyType` som innehåller det PDF-dokument som ska demonteras
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ

   Metoden `invokeDDX` returnerar ett `AssemblerResult`-objekt som innehåller jobbresultaten och eventuella undantag som har inträffat.

1. Spara de omonterade PDF-dokumenten.

   Gör så här för att hämta nya PDF-dokument:

   * Öppna `AssemblerResult`-objektets `documents`-fält, som är ett `Map`-objekt som innehåller de omonterade PDF-dokumenten.
   * Iterera genom objektet `Map` för att få fram varje resulterande dokument. Sedan konverterar du den arraymedlemmens `value` till en `BLOB`.
   * Extrahera de binära data som representerar PDF-dokumentet genom att komma åt objektets `BLOB`-egenskap `MTOM`. Detta returnerar en array med byte som du kan skriva till en PDF-fil.

**Se även**

[Dela upp PDF-dokument programmatiskt](/help/forms/developing/programmatically-disassembling-pdf-documents.md#programmatically-disassembling-pdf-documents)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
