---
title: Tilldela användningsrättigheter
description: Använd Acrobat Reader DC-tilläggen för Java Client API och Web Service API för att tillämpa och ta bort användningsrättigheter från PDF-dokument.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,Reader Extensions
exl-id: d8027b43-10c7-435c-8fb5-059508966d42
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3897'
ht-degree: 0%

---

# Tilldela användningsrättigheter {#assigning-usage-rights}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

## Om Acrobat Reader DC-tilläggstjänsten {#about-the-acrobat-reader-dc-extensions-service}

Med Acrobat Reader DC-tilläggstjänsten kan din organisation enkelt dela interaktiva PDF-dokument genom att utöka funktionerna i Adobe Reader. Tilläggstjänsten Acrobat Reader DC stöder alla PDF-dokument, till och med PDF 1.7. Det fungerar med Adobe Reader 7.0 och senare. Tjänsten lägger in användarrättigheter i ett PDF-dokument och aktiverar funktioner som normalt inte är tillgängliga när ett PDF-dokument öppnas i Adobe Reader. Tredjepartsanvändare behöver inte några extra program eller plugin-program för att kunna arbeta med upphovsrättsaktiverade dokument.

Du kan utföra följande uppgifter med hjälp av tilläggstjänsten i Acrobat Reader DC:

* Lägg in användarrättigheter i PDF-dokument. Mer information finns i [Använda användningsrättigheter i PDF-dokument](assigning-usage-rights.md#applying-usage-rights-to-pdf-documents).
* Ta bort användningsrättigheter från PDF-dokument. Mer information finns i [Ta bort användningsrättigheter från PDF-dokument](assigning-usage-rights.md#removing-usage-rights-from-pdf-documents).
* Hämta information om autentiseringsuppgifter. Mer information finns i [Hämtar information om autentiseringsuppgifter](assigning-usage-rights.md#retrieving-credential-information).

>[!NOTE]
>
>Mer information om Acrobat Reader DC-tilläggstjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Använda användningsbehörighet för PDF-dokument {#applying-usage-rights-to-pdf-documents}

Du kan lägga till användarrättigheter i PDF-dokument med Acrobat Reader DC-tilläggen för Java Client API och webbtjänsten. Användningsrättigheterna gäller funktioner som är tillgängliga som standard i Acrobat men inte i Adobe Reader, t.ex. möjligheten att lägga till kommentarer i ett formulär eller att fylla i formulärfält och spara formuläret. PDF-dokument med användarrättigheter kallas för rättighetsaktiverade dokument. En användare som öppnar ett rättighetsaktiverat dokument i Adobe Reader kan utföra åtgärder som är aktiverade för det specifika dokumentet.

>[!NOTE]
>
>När du tillämpar användningsrättigheter på PDF-dokument med metoden `applyUsageRights`, som är en del av Java API, kan du ange parametern `isModeFinal` för objektet `ReaderExtensionsOptionSpec` till `false`. Detta resulterar i att de bearbetade formulärräknarna inte uppdateras och att prestandan förbättras. Om du inte är orolig för att uppdatera räknaren för bearbetade formulär rekommenderar vi att du ställer in parametern `isModeFinal` på `false`.

>[!NOTE]
>
>Mer information om Acrobat Reader DC-tilläggstjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Gör så här om du vill lägga till användarrättigheter i ett PDF-dokument:

1. Inkludera projektfiler.
1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.
1. Hämta ett PDF-dokument.
1. Ange användningsrättigheter att använda.
1. Lägg in användningsrättigheter i PDF-dokumentet.
1. Spara det aktiverade PDF-dokumentet.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett klientobjekt för Acrobat Reader DC-tillägg**

För att programmässigt kunna utföra en tilläggsåtgärd i Acrobat Reader DC måste du skapa ett klientobjekt för tjänsten Acrobat Reader DC extensions. Om du använder Java API:t för Acrobat Reader DC-tilläggen skapar du ett `ReaderExtensionsServiceClient`-objekt. Om du använder webbtjänstens API för Acrobat Reader DC-tillägg skapar du ett `ReaderExtensionsServiceService`-objekt.

**Hämta ett PDF-dokument**

Hämta ett PDF-dokument om du vill använda användningsrättigheter. Rättighetsaktiverade PDF-dokument innehåller en ordlista med användningsrättigheter. När Adobe Reader öppnar ett dokument som innehåller en sådan ordlista aktiveras de användningsrättigheter som anges i ordlistan endast för det dokumentet. Om dokumentet inte innehåller någon användarrättighetsordlista skapar Acrobat Reader DC-tilläggstjänsten en sådan. Om den redan innehåller en ordlista skriver Acrobat Reader DC-tilläggstjänsten över befintliga användningsrättigheter med de som du anger. Ordlistan anger vilka användarrättigheter som är aktiverade. När en användare öppnar dokumentet i Adobe Reader tillåts bara de användningsrättigheter som anges i ordlistan.

**Ange användningsbehörighet att använda**

Vilka användarrättigheter du kan ange avgörs av en inloggningsinformation som du köper från Adobe Systems Incorporated. Autentiseringsuppgifterna ger vanligtvis behörighet att ange en grupp användarrättigheter, t.ex. de som gäller interaktiva formulär. Varje behörighet ger rätt att skapa ett visst antal rättighetsaktiverade PDF-dokument. Med en utvärderingsautentisering får man rätt att skapa ett obegränsat antal utkast till dokument.

>[!NOTE]
>
>Om du försöker tilldela en användningsbehörighet som inte tillåts av dina autentiseringsuppgifter, kommer du att orsaka ett undantag.

**Använd användningsbehörighet för PDF-dokumentet**

Om du vill lägga till användarrättigheter i ett PDF-dokument refererar du till aliaset för de autentiseringsuppgifter som du använder för att tillämpa användarrättigheter (en autentiseringsuppgift installeras vanligtvis under installationen av AEM Forms). Du måste även ange det PDF-dokument som användarrättigheterna gäller för. Mer information om hur du konfigurerar en autentiseringsuppgift finns i installations- och distributionsguiden för programservern.

**Spara det rättighetsaktiverade PDF-dokumentet**

När Acrobat Reader DC-tilläggstjänsten har tillämpat användarrättigheter på ett PDF-dokument kan du spara det aktiverade PDF-dokumentet som en PDF-fil.

**Se även**

[Använd användningsrättigheter med Java API](assigning-usage-rights.md#apply-usage-rights-using-the-java-api)

[Använd användningsrättigheter med webbtjänstens API](assigning-usage-rights.md#apply-usage-rights-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för tjänsten Acrobat Reader DC Extensions Service API](/help/forms/developing/acrobat-reader-dc-extensions-service.md#acrobat-reader-dc-extensions-service-java-api-quick-start-soap)

### Använd användningsrättigheter med Java API {#apply-usage-rights-using-the-java-api}

Använd användarrättigheter i ett PDF-dokument med Acrobat Reader DC Extensions API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, som adobe-reader-extensions-client.jar, i Java-projektets klassökväg.

1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `ReaderExtensionsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Hämta ett PDF-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar PDF-dokumentet genom att använda dess konstruktor och skicka ett strängvärde som anger platsen för PDF-dokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ange användningsrättigheter att använda.

   * Skapa ett `UsageRights`-objekt som representerar användningsrättigheter med hjälp av konstruktorn.
   * Anropa en motsvarande metod som tillhör objektet `UsageRights` för varje användningsbehörighet som ska användas. Om du till exempel vill lägga till användningsrättigheten `enableFormFillIn` anropar du `UsageRights`-objektets `enableFormFillIn`-metod och skickar `true`. (Upprepa det här steget för varje användningsbehörighet).

1. Lägg in användningsrättigheter i PDF-dokumentet.

   * Skapa ett `ReaderExtensionsOptionSpec`-objekt med hjälp av dess konstruktor. Det här objektet innehåller körningsalternativ som krävs av Acrobat Reader DC-tilläggstjänsten. När du anropar den här konstruktorn måste du ange följande värden:

      * Objektet `UsageRights` som innehåller användningsrättigheterna som ska tillämpas på dokumentet.
      * Ett strängvärde som anger ett meddelande som användaren ser när det aktiverade PDF-dokumentet öppnas i Adobe Reader 7.x. Meddelandet visas inte i Adobe Reader 8.0.

   * Använd användningsbehörighet för PDF-dokumentet genom att anropa `ReaderExtensionsServiceClient`-objektets `applyUsageRights`-metod och skicka följande värden:

      * Objektet `com.adobe.idp.Document` som innehåller det PDF-dokument som användningsrättigheterna tillämpas på.
      * Ett strängvärde som anger det alias för autentiseringsuppgiften som gör att du kan tillämpa användningsbehörighet.
      * Ett strängvärde som anger motsvarande lösenordsvärde. (Den här parametern ignoreras för närvarande. Du kan skicka `null`.)

   * Objektet `ReaderExtensionsOptionSpec` som innehåller körningsalternativ.

   Metoden `applyUsageRights` returnerar ett `com.adobe.idp.Document`-objekt som innehåller det rättighetsaktiverade PDF-dokumentet.

1. Spara det aktiverade PDF-dokumentet.

   * Skapa ett `java.io.File`-objekt och kontrollera att filtillägget är .pdf.
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att kopiera innehållet i `com.adobe.idp.Document`-objektet till filen (se till att du använder det `com.adobe.idp.Document`-objekt som returnerades av metoden `applyUsageRights`).

**Se även**

[Använda användningsbehörighet för PDF-dokument](assigning-usage-rights.md#applying-usage-rights-to-pdf-documents)

[Snabbstart (SOAP-läge):Tillämpa användningsrättigheter med Java API](/help/forms/developing/acrobat-reader-dc-extensions-service.md#quick-start-soap-mode-applying-usage-rights-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Använd användningsrättigheter med webbtjänstens API {#apply-usage-rights-using-the-web-service-api}

Använd användarrättigheter i ett PDF-dokument med Acrobat Reader DC Extensions API (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/ReaderExtensionsService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.

   * Skapa ett `ReaderExtensionsServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `ReaderExtensionsServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/ReaderExtensionsService?blob=mtom`). Se till att du anger `?blob=mtom`.)
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `ReaderExtensionsServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `ReaderExtensionsServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `ReaderExtensionsServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Hämta ett PDF-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra ett PDF-dokument som en användningsbehörighet tillämpas på.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod. Skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.

1. Ange användningsrättigheter att använda.

   * Skapa ett `UsageRights`-objekt som representerar användningsrättigheter med hjälp av konstruktorn.
   * För varje användningsbehörighet som ska användas tilldelar du värdet `true` till motsvarande datamedlem som tillhör objektet `UsageRights`. Om du till exempel vill lägga till användningsrättigheten `enableFormFillIn` tilldelar du `true` till `UsageRights`-objektets `enableFormFillIn`-datamedlem. (Upprepa det här steget för varje användningsbehörighet).

1. Lägg in användningsrättigheter i PDF-dokumentet.

   * Skapa ett `ReaderExtensionsOptionSpec`-objekt med hjälp av dess konstruktor. Det här objektet innehåller körningsalternativ som krävs av Acrobat Reader DC-tilläggstjänsten.
   * Tilldela objektet `UsageRights` till `ReaderExtensionsOptionSpec`-objektets `usageRights`-datamedlem.
   * Tilldela ett strängvärde som anger det meddelande som en användare ser när ett rättighetsaktiverat PDF-dokument öppnas i Adobe Reader till `message`-objektets `ReaderExtensionsOptionSpec`-datamedlem.
   * Använd användningsbehörighet för PDF-dokumentet genom att anropa `ReaderExtensionsServiceClient`-objektets `applyUsageRights`-metod och skicka följande värden:

      * Objektet `BLOB` som innehåller det PDF-dokument som användningsrättigheterna tillämpas på.
      * Ett strängvärde som anger det alias för autentiseringsuppgiften som gör att du kan tillämpa användningsbehörighet.
      * Ett strängvärde som anger motsvarande lösenordsvärde. (Den här parametern ignoreras för närvarande. Du kan skicka `null`.)

   * Objektet `ReaderExtensionsOptionSpec` som innehåller körningsalternativ.

   Metoden `applyUsageRights` returnerar ett `BLOB`-objekt som innehåller det rättighetsaktiverade PDF-dokumentet.

1. Spara det aktiverade PDF-dokumentet.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor. Skicka ett strängvärde som representerar filplatsen för det rättighetsaktiverade PDF-dokumentet.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som returnerades av metoden `applyUsageRights`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till en PDF-fil genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Använda användningsbehörighet för PDF-dokument](assigning-usage-rights.md#applying-usage-rights-to-pdf-documents)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Ta bort användningsrättigheter från PDF-dokument {#removing-usage-rights-from-pdf-documents}

Du kan ta bort användarrättigheter från ett rättighetsaktiverat dokument. Du måste också ta bort användarrättigheter från ett rättighetsaktiverat PDF-dokument för att kunna utföra andra AEM Forms-åtgärder på det. Du måste till exempel signera (eller certifiera) ett PDF-dokument digitalt innan du anger användningsbehörighet. Om du vill utföra åtgärder på ett rättighetsaktiverat dokument måste du därför ta bort användningsbehörighet från PDF-dokumentet, utföra andra åtgärder, som att signera dokumentet digitalt och sedan återanvända användningsbehörighet för dokumentet.

>[!NOTE]
>
>Mer information om Acrobat Reader DC-tilläggstjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-1}

Så här tar du bort användarrättigheter från ett rättighetsaktiverat PDF-dokument:

1. Inkludera projektfiler.
1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.
1. Hämta ett rättighetsaktiverat PDF-dokument.
1. Ta bort användningsrättigheter från PDF-dokumentet.
1. Spara PDF-dokumentet.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett klientobjekt för Acrobat Reader DC-tillägg**

Innan du programmässigt kan utföra en tilläggsåtgärd för Acrobat Reader DC måste du skapa ett klientobjekt för Acrobat Reader DC-tilläggsprogrammet. Om du använder Java API skapar du ett `ReaderExtensionsServiceClient`-objekt. Om du använder webbtjänstens API för Acrobat Reader DC-tillägg skapar du ett `ReaderExtensionsServiceService`-objekt.

**Hämta ett rättighetsaktiverat PDF-dokument**

Hämta ett rättighetsaktiverat PDF-dokument för att ta bort användningsrättigheter.

**Ta bort användningsrättigheter från PDF-dokumentet**

När du har hämtat ett rättighetsaktiverat PDF-dokument kan du ta bort användarrättigheterna. När du har tagit bort användarrättigheterna har PDF-dokumentet inga ytterligare funktioner när det visas i Adobe Reader.

**Spara PDF-dokumentet**

Du kan spara PDF-dokumentet som inte längre innehåller användningsrättigheter som en PDF-fil. När dokumentet har sparats som en PDF-fil kan det öppnas i Adobe Reader eller Acrobat.

**Se även**

[Ta bort användningsrättigheter med Java API](assigning-usage-rights.md#remove-usage-rights-using-the-java-api)

[Ta bort användningsrättigheter med webbtjänstens API](assigning-usage-rights.md#remove-usage-rights-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för tjänsten Acrobat Reader DC Extensions Service API](/help/forms/developing/acrobat-reader-dc-extensions-service.md#acrobat-reader-dc-extensions-service-java-api-quick-start-soap)

[Använda användningsbehörighet för PDF-dokument](assigning-usage-rights.md#applying-usage-rights-to-pdf-documents)

### Ta bort användningsrättigheter med Java API {#remove-usage-rights-using-the-java-api}

Ta bort användarrättigheter från ett rättighetsaktiverat PDF-dokument med Acrobat Reader DC-tilläggs-API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-reader-extensions-client.jar, i Java-projektets klassökväg.

1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.

   Skapa ett `ReaderExtensionsServiceClient`-objekt med hjälp av dess konstruktor och skicka ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.

1. Hämta ett PDF-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar det rättighetsaktiverade PDF-dokumentet genom att använda dess konstruktor och skicka ett strängvärde som anger platsen för PDF-dokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ta bort användningsrättigheter från PDF-dokumentet.

   Ta bort användningsrättigheter från PDF-dokumentet genom att anropa `ReaderExtensionsServiceClient`-objektets `removeUsageRights`-metod och skicka `com.adobe.idp.Document`-objektet som innehåller det rättighetsaktiverade PDF-dokumentet. Den här metoden returnerar ett `com.adobe.idp.Document`-objekt som innehåller ett PDF-dokument som inte har användningsbehörighet.

1. Lägg in användningsrättigheter i PDF-dokumentet.

   * Skapa ett `java.io.File`-objekt och kontrollera att filtillägget är .PDF.
   * Anropa `Document`-objektets `copyToFile`-metod för att kopiera innehållet i `Document`-objektet till filen (se till att du använder det `Document`-objekt som returnerades av metoden `removeUsageRights`).

**Se även**

[Ta bort användningsrättigheter från PDF-dokument](assigning-usage-rights.md#removing-usage-rights-from-pdf-documents)

[Snabbstart (SOAP-läge): Ta bort användningsrättigheter från ett PDF-dokument med Java API](/help/forms/developing/acrobat-reader-dc-extensions-service.md#quick-start-soap-mode-removing-usage-rights-from-a-pdf-document-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Ta bort användningsrättigheter med webbtjänstens API {#remove-usage-rights-using-the-web-service-api}

Ta bort användarrättigheter från ett rättighetsaktiverat PDF-dokument med Acrobat Reader DC-tilläggs-API:t (webbtjänsten):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/ReaderExtensionsService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.

   * Skapa ett `ReaderExtensionsServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `ReaderExtensionsServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/ReaderExtensionsService?blob=mtom`). Se till att du anger `?blob=mtom`.)
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `ReaderExtensionsServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `ReaderExtensionsServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `ReaderExtensionsServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Hämta ett PDF-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra det rättighetsaktiverade PDF-dokumentet från vilket användningsrättigheterna tas bort.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.

1. Ta bort användningsrättigheter från PDF-dokumentet.

   Ta bort användningsrättigheter från PDF-dokumentet genom att anropa `ReaderExtensionsServiceClient`-objektets `removeUsageRights`-metod och skicka `BLOB`-objektet som innehåller det rättighetsaktiverade PDF-dokumentet. Den här metoden returnerar ett `BLOB`-objekt som innehåller ett PDF-dokument som inte har användningsbehörighet.

1. Lägg in användningsrättigheter i PDF-dokumentet.

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för PDF.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som returnerades av metoden `removeUsageRights`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `MTOM`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.

**Se även**

[Ta bort användningsrättigheter från PDF-dokument](assigning-usage-rights.md#removing-usage-rights-from-pdf-documents)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)

## Hämtar information om autentiseringsuppgifter {#retrieving-credential-information}

Du kan hämta information om de autentiseringsuppgifter som användes för att tillämpa användarrättigheter på ett rättighetsaktiverat PDF-dokument. Genom att hämta information om en autentiseringsuppgift, kan du få information som det datum efter vilket certifikatet inte längre är giltigt.

>[!NOTE]
>
>Mer information om Acrobat Reader DC-tilläggstjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-2}

Så här hämtar du information om de autentiseringsuppgifter som användes för att tillämpa användarrättigheter på ett PDF-dokument:

1. Inkludera projektfiler.
1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.
1. Hämta ett rättighetsaktiverat PDF-dokument.
1. Hämta information om autentiseringsuppgifterna.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett klientobjekt för Acrobat Reader DC-tillägg**

Innan du programmässigt kan utföra en tilläggsåtgärd för Acrobat Reader DC måste du skapa ett klientobjekt för Acrobat Reader DC-tilläggsprogrammet. Om du använder Java API skapar du ett `ReaderExtensionsServiceClient`-objekt. Om du använder webbtjänstens API för Acrobat Reader DC-tillägg skapar du ett `ReaderExtensionsServiceService`-objekt.

**Hämta ett rättighetsaktiverat PDF-dokument**

Hämta ett rättighetsaktiverat PDF-dokument för att hämta information om autentiseringsuppgifterna. Du kan även hämta information om en autentiseringsuppgift genom att ange dess alias. Om du vill hämta information om en autentiseringsuppgift som användes för att tillämpa användarrättigheter på ett visst rättighetsaktiverat PDF-dokument måste du dock hämta dokumentet.

**Hämta information om autentiseringsuppgifterna**

När du har hämtat ett rättighetsaktiverat PDF-dokument kan du få information om de autentiseringsuppgifter som användes för att tillämpa användarrättigheterna på det. Du kan få följande information om autentiseringsuppgifterna:

* Meddelandet som visas i Adobe Reader när det aktiverade PDF-dokumentet öppnas.
* Det datum efter vilket autentiseringsuppgifterna inte längre är giltiga.
* Det datum före vilket autentiseringsuppgifterna inte är giltiga.
* De användarrättigheter som har angetts för det här rättighetsaktiverade PDF-dokumentet.
* Antalet gånger som autentiseringsuppgifterna har använts.

**Se även**

[Ta bort användningsrättigheter med Java API](assigning-usage-rights.md#remove-usage-rights-using-the-java-api)

[Ta bort användningsrättigheter med webbtjänstens API](assigning-usage-rights.md#remove-usage-rights-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Snabbstart för tjänsten Acrobat Reader DC Extensions Service API](/help/forms/developing/acrobat-reader-dc-extensions-service.md#acrobat-reader-dc-extensions-service-java-api-quick-start-soap)

### Hämta inloggningsinformation med Java API {#retrieve-credential-information-using-the-java-api}

Hämta inloggningsinformation med Acrobat Reader DC Extensions API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-reader-extensions-client.jar, i Java-projektets klassökväg.

1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.

   Skapa ett `ReaderExtensionsServiceClient`-objekt med hjälp av dess konstruktor och skicka ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.

1. Hämta ett PDF-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar det rättighetsaktiverade PDF-dokumentet genom att använda dess konstruktor och skicka ett strängvärde som anger platsen för det rättighetsaktiverade PDF-dokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Ta bort användningsrättigheter från PDF-dokumentet.

   * Hämta information om de autentiseringsuppgifter som används för att tillämpa användningsrättigheter på PDF-dokumentet genom att anropa `ReaderExtensionsServiceClient`-objektets `getDocumentUsageRights`-metod och skicka `com.adobe.idp.Document`-objektet som innehåller det rättighetsaktiverade PDF-dokumentet. Den här metoden returnerar ett `GetUsageRightsResult`-objekt som innehåller autentiseringsuppgifter.
   * Hämta det datum efter vilket autentiseringsuppgifterna inte längre är giltiga genom att anropa `GetUsageRightsResult`-objektets `getNotAfter`-metod. Den här metoden returnerar ett `java.util.Date`-objekt som representerar det datum efter vilket autentiseringsuppgifterna inte längre är giltiga.
   * Hämta meddelandet som visas i Adobe Reader när det rättighetsaktiverade PDF-dokumentet öppnas genom att anropa `GetUsageRightsResult`-objektets `getMessage`-metod. Den här metoden returnerar ett strängvärde som representerar meddelandet.

**Se även**

[Hämtar information om autentiseringsuppgifter](assigning-usage-rights.md#retrieving-credential-information)

[Snabbstart (SOAP-läge): Hämta inloggningsinformation med Java API](/help/forms/developing/acrobat-reader-dc-extensions-service.md#quick-start-soap-mode-retrieving-credential-information-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Hämta autentiseringsuppgifter med webbtjänstens API {#retrieve-credential-information-using-the-web-service-api}

Hämta inloggningsinformation med Acrobat Reader DC Extensions API (webbtjänst):

1. Inkludera projektfiler.

   Skapa ett Microsoft .NET-projekt som använder MTOM. Kontrollera att du använder följande WSDL-definition: `http://localhost:8080/soap/services/ReaderExtensionsService?WSDL&lc_version=9.0.1`.

   >[!NOTE]
   >
   >Ersätt `localhost` med IP-adressen för servern som är värd för AEM Forms.

1. Skapa ett klientobjekt för Acrobat Reader DC-tillägg.

   * Skapa ett `ReaderExtensionsServiceClient`-objekt med hjälp av dess standardkonstruktor.
   * Skapa ett `ReaderExtensionsServiceClient.Endpoint.Address`-objekt med konstruktorn `System.ServiceModel.EndpointAddress`. Skicka ett strängvärde som anger WSDL till AEM Forms-tjänsten (till exempel `http://localhost:8080/soap/services/ReaderExtensionsService?blob=mtom`). Se till att du anger `?blob=mtom`.)
   * Skapa ett `System.ServiceModel.BasicHttpBinding`-objekt genom att hämta värdet för fältet `ReaderExtensionsServiceClient.Endpoint.Binding`. Skicka returvärdet till `BasicHttpBinding`.
   * Ställ in `System.ServiceModel.BasicHttpBinding`-objektets `MessageEncoding`-fält till `WSMessageEncoding.Mtom`. Detta värde garanterar att MTOM används.
   * Aktivera grundläggande HTTP-autentisering genom att utföra följande åtgärder:

      * Tilldela användarnamnet för AEM-formulär till fältet `ReaderExtensionsServiceClient.ClientCredentials.UserName.UserName`.
      * Tilldela motsvarande lösenordsvärde till fältet `ReaderExtensionsServiceClient.ClientCredentials.UserName.Password`.
      * Tilldela konstantvärdet `HttpClientCredentialType.Basic` till fältet `BasicHttpBindingSecurity.Transport.ClientCredentialType`.
      * Tilldela konstantvärdet `BasicHttpSecurityMode.TransportCredentialOnly` till fältet `BasicHttpBindingSecurity.Security.Mode`.

1. Hämta ett PDF-dokument.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra ett rättighetsaktiverat PDF-dokument.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det rättighetsaktiverade PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `MTOM`-egenskap med innehållet i bytearrayen.

1. Ta bort användningsrättigheter från PDF-dokumentet.

   * Hämta information om de autentiseringsuppgifter som används för att tillämpa användningsrättigheter på PDF-dokumentet genom att anropa `ReaderExtensionsServiceClient`-objektets `getDocumentUsageRights`-metod och skicka `com.adobe.idp.Document`-objektet som innehåller det rättighetsaktiverade PDF-dokumentet. Den här metoden returnerar ett `GetUsageRightsResult`-objekt som innehåller autentiseringsuppgifter.
   * Hämta datumet efter vilket autentiseringsuppgifterna inte längre är giltiga genom att hämta värdet för `GetUsageRightsResult`-objektets `notAfter`-datamedlem. Datatypen för denna datamedlem är `System.DateTime`.
   * Hämta meddelandet som visas när det rättighetsaktiverade PDF-dokumentet öppnas i Adobe Reader genom att hämta värdet för `message`-objektets `GetUsageRightsResult`-datamedlem. Datatypen för den här datamedlemmen är en sträng.
   * Hämta antalet gånger som autentiseringsuppgiften används genom att hämta värdet för `GetUsageRightsResult`-objektets `useCount`-datamedlem. Datatypen för den här datamedlemmen är ett heltal.

**Se även**

[Hämtar information om autentiseringsuppgifter](assigning-usage-rights.md#retrieving-credential-information)

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)

[Anropa AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref)
