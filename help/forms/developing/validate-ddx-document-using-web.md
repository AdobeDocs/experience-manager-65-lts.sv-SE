---
title: Validera ett DX-dokument med webbtjänstens API
description: Använd Assembler Service API för att validera ett DX-dokument.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/validating_ddx_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
exl-id: f934b721-1ca3-4626-9039-a38f0dfecea2
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 0%

---

# Validera ett DX-dokument med webbtjänstens API {#validate-a-ddx-document-using-theweb-service-api}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Validera ett DX-dokument med Assembler Service API (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/AssemblerService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt localhost med IP-adressen för Forms Server.

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

1. Ange körningsalternativ för att validera DDX-dokumentet.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange det körningsalternativ som instruerar Assembler-tjänsten att validera DDX-dokumentet genom att tilldela värdet true till `AssemblerOptionSpec`-objektets `validateOnly`-datamedlem.
   * Ange den mängd information som Assembler-tjänsten skriver till loggfilen genom att tilldela ett strängvärde till `AssemblerOptionSpec`-objektets `logLevel`-datamedlem. -metod När du validerar ett DX-dokument vill du att mer information ska skrivas till loggfilen som ska vara till hjälp vid valideringsprocessen. Du kan därför ange värdet `FINE` eller `FINER`. Mer information om de körningsalternativ du kan ange finns i klassreferensen `AssemblerOptionSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

1. Utför valideringen.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet.
   * Värdet `null` för objektet `Map` som vanligtvis lagrar PDF-dokument.
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ.

   Metoden `invokeDDX` returnerar ett `AssemblerResult`-objekt som innehåller information som anger om DDX-dokumentet är giltigt.

1. Spara valideringsresultaten i en loggfil.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar platsen för loggfilen och läget som filen ska öppnas i. Kontrollera att filnamnstillägget är .xml.
   * Skapa ett `BLOB`-objekt som lagrar logginformation genom att hämta värdet för `AssemblerResult`-objektets `jobLog`-datamedlem.
   * Skapa en bytearray som lagrar innehållet i objektet `BLOB`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-fält.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till en PDF-fil genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

   >[!NOTE]
   >
   >Om DDX-dokumentet är ogiltigt genereras en `OperationException`. I catch-satsen kan du hämta värdet för `OperationException`-objektets `jobLog`-medlem.

**Se även**

[Validerar DDX-dokument](/help/forms/developing/validating-ddx-documents.md#validating-ddx-documents)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
