---
title: Validerar DDX-dokument
description: Validera ett DDX-dokument programmatiskt med Java API och Web Service API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/assembling_pdf_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
hide: true
hidefromtoc: true
exl-id: 4efc6e82-f577-4881-9c9d-30e1fe2cdb9d
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1507'
ht-degree: 0%

---

# Validerar DDX-dokument {#validating-ddx-documents}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Du kan programmässigt validera ett DDX-dokument som används av Assembler-tjänsten. Med andra ord kan du med hjälp av API:t för Assembler-tjänsten avgöra om ett DDX-dokument är giltigt eller inte. Om du till exempel uppgraderade från en tidigare version av AEM Forms och vill vara säker på att ditt DDX-dokument är giltigt, kan du validera det med hjälp av Assembler-tjänst-API:t.

>[!NOTE]
>
>Mer information om Assembler-tjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

>[!NOTE]
>
>Mer information om ett DDX-dokument finns i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

## Sammanfattning av steg {#summary-of-steps}

Så här validerar du ett DX-dokument:

1. Inkludera projektfiler.
1. Skapa en Assembler-klient.
1. Referera till ett befintligt DDX-dokument.
1. Ange körningsalternativ för att validera DDX-dokumentet.
1. Utför valideringen.
1. Spara valideringsresultaten i en loggfil.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

Följande JAR-filer måste läggas till i projektets klasssökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-assembler-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms används i JBoss)

Om AEM Forms körs på en annan J2EE-programserver än JBoss måste du ersätta filerna adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på.

**Skapa en PDF Assembler-klient**

Innan du programmässigt kan utföra en Assembler-åtgärd måste du skapa en Assembler-tjänstklient.

**Referera till ett befintligt DDX-dokument**

Om du vill validera ett DDX-dokument måste du referera till ett befintligt DDX-dokument.

**Ange körningsalternativ för att validera DDX-dokumentet**

När du validerar ett DX-dokument måste du ange specifika körningsalternativ som instruerar Assembler-tjänsten att validera DX-dokumentet i stället för att köra det. Du kan också öka mängden information som Assembler-tjänsten skriver till loggfilen.

**Utför valideringen**

När du har skapat Assembler-tjänstklienten, refererat till DDX-dokumentet och angett körningsalternativ, kan du anropa åtgärden `invokeDDX` för att validera DDX-dokumentet. När du validerar DDX-dokumentet kan du skicka `null` som mappningsparameter (den här parametern lagrar vanligtvis PDF-dokument som Assembler kräver för att utföra de åtgärder som anges i DDX-dokumentet).

Om valideringen misslyckas genereras ett undantag och loggfilen innehåller information som förklarar varför DDX-dokumentet är ogiltigt kan hämtas från `OperationException`-instansen. Efter den grundläggande XML-tolkningen och schemakontrollen utförs valideringen mot DDX-specifikationen. Alla fel som finns i DDX-dokumentet anges i loggen.

**Spara verifieringsresultaten i en loggfil**

Assembler-tjänsten returnerar valideringsresultaten som du kan skriva till en XML-loggfil. Hur detaljerad Assembler-tjänsten skriver till loggfilen beror på vilket körningsalternativ du anger.

**Se även**

[Validera ett DX-dokument med Java API](#validate-a-ddx-document-using-the-java-api)

[Validera ett DX-dokument med webbtjänstens API](#validate-a-ddx-document-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Programmisk sammanställning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md)

## Validera ett DX-dokument med Java API {#validate-a-ddx-document-using-the-java-api}

Validera ett DX-dokument med Assembler Service API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-assembler-client.jar, i Java-projektets klassökväg.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar DDX-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för DDX-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange körningsalternativ för att validera DDX-dokumentet.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange det körningsalternativ som instruerar Assembler-tjänsten att validera DDX-dokumentet genom att anropa `AssemblerOptionSpec`-objektets setValidateOnly-metod och skicka `true`.
   * Ange den mängd information som Assembler-tjänsten skriver till loggfilen genom att anropa `AssemblerOptionSpec`-objektets `getLogLevel`-metod och skicka ett strängvärde som uppfyller dina krav. När du validerar ett DX-dokument vill du att mer information ska skrivas till loggfilen som ska vara till hjälp vid valideringsprocessen. Du kan därför skicka värdet `FINE` eller `FINER`.

1. Utför valideringen.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar DDX-dokumentet.
   * Värdet `null` för java.io.Map-objektet som vanligtvis lagrar PDF-dokument.
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen.

   Metoden `invokeDDX` returnerar ett `AssemblerResult`-objekt som innehåller information som anger om DDX-dokumentet är giltigt.

1. Spara valideringsresultaten i en loggfil.

   * Skapa ett `java.io.File`-objekt och kontrollera att filnamnstillägget är .xml.
   * Anropa metoden `getJobLog` för objektet `AssemblerResult`. Den här metoden returnerar en `com.adobe.idp.Document`-instans som innehåller verifieringsinformation.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen.

   >[!NOTE]
   >
   >Om DDX-dokumentet är ogiltigt genereras en `OperationException`. I catch-satsen kan du anropa `OperationException`-objektets `getJobLog`-metod.

**Se även**

[Validerar DDX-dokument](#validating-ddx-documents)

[Snabbstart (SOAP-läge): Validerar DDX-dokument med Java API &#x200B;](/help/forms/developing/assembler-service-java-api-quick.md#quick-start-soap-mode-validating-ddx-documents-using-the-java-api) (SOAP-läge)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Validera ett DX-dokument med webbtjänstens API {#validate-a-ddx-document-using-the-web-service-api}

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

[Validerar DDX-dokument](#validating-ddx-documents)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
