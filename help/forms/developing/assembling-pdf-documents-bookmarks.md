---
title: Sammanställa PDF-dokument med bokmärken
description: Använd Assembler-tjänsten för att ändra ett PDF-dokument som innehåller bokmärken så att de inkluderar bokmärken med Java API och Web Service API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/assembling_pdf_documents
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services
exl-id: 40fbbef6-3a2e-455d-81a3-23c7e322c0fb
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '2519'
ht-degree: 0%

---

# Sammanställa PDF-dokument med bokmärken {#assembling-pdf-documents-with-bookmarks}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Du kan montera ett PDF-dokument som innehåller bokmärken. Anta till exempel att du har ett PDF-dokument som inte innehåller bokmärken och du vill ändra det genom att ange bokmärken. Med Assembler-tjänsten kan du skicka ett PDF-dokument som inte innehåller bokmärken och sedan gå tillbaka till ett PDF-dokument som innehåller bokmärken.

Bokmärken innehåller följande egenskaper:

* En titel som visas som text på skärmen.
* En åtgärd som anger vad som händer när en användare klickar på bokmärket. En vanlig åtgärd för ett bokmärke är att flytta till en annan plats i det aktuella dokumentet eller öppna ett annat PDF-dokument, men andra åtgärder kan anges.

Anta att följande DDX-dokument används för den här diskussionen.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <DDX xmlns="https://ns.adobe.com/DDX/1.0/">
       <PDF result="FinalDoc.pdf">
          <PDF source="Loan.pdf">
             <Bookmarks source="doc2" />
          </PDF>
       </PDF>
 </DDX>
```

Observera att källattributet är tilldelat värdet `Loan.pdf` i det här DDX-dokumentet. Det här DDX-dokumentet anger att ett enda PDF-dokument skickas till Assembler-tjänsten. När du sammanställer ett PDF-dokument med bokmärken måste du ange ett XML-dokument för bokmärken som beskriver bokmärkena i resultatdokumentet. Om du vill ange ett XML-dokument för bokmärken kontrollerar du att elementet `Bookmarks` har angetts i ditt DDX-dokument.

I det här exemplet på DDX-dokument anger elementet `Bookmarks` `doc2` som värde. Det här värdet anger att indatamappningen som skickades till Assembler-tjänsten innehåller en nyckel med namnet `doc2`. Värdet för nyckeln `doc2` är ett `com.adobe.idp.Document`-värde som representerar XML-bokmärkesdokumentet. (Se&quot;Bokmärkesspråk&quot; i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).)

I det här avsnittet används följande språk för XML-bokmärken för att sammanställa ett PDF-dokument som innehåller bokmärken.

```xml
 <?xml version="1.0" encoding="UTF-8"?>
 <Bookmarks xmlns="https://ns.adobe.com/pdf/bookmarks" version="1.0">
       <Bookmark>
          <Action>
             <Launch NewWindow="true">
                <File Name="C:\Adobe\LoanDetails.pdf" />
             </Launch>
          </Action>
         <Title>Open the Loan document</Title>
       </Bookmark>
 <Bookmark>
          <Action>
             <Launch>
                <Win Name="C:\WINDOWS\notepad.exe" />
             </Launch>
          </Action>
     <Title>Launch NotePad</Title>
       </Bookmark>
 </Bookmarks>
```

Lägg märke till det Action-element som definierar åtgärden som utförs när en användare klickar på bokmärket i det här XML-bokmärkesdokumentet. Under elementet Action (Åtgärd) är det Launch-element som startar program, till exempel NotePad, och öppnar filer, till exempel PDF-filer. Om du vill öppna en PDF-fil måste du använda det File-element som anger vilken fil som ska öppnas. I t.ex. den XML-fil för bokmärke som anges i det här avsnittet heter filen som öppnas LoanDetails.pdf.

>[!NOTE]
>
>Fullständig information om vilka åtgärder som stöds finns i `Action`-elementet i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

Med tanke på det DDX-dokument som anges i det här avsnittet och XML-bokmärkesfilen som indata sammanställer Assembler-tjänsten ett PDF-dokument som innehåller följande bokmärken.

![aw_aw_bmark](assets/aw_aw_bmark.png)

När en användare klickar på bokmärket *Öppna låneinformationen* öppnas filen LoanDetails.pdf. På samma sätt startas NotePad när användaren klickar på bokmärket *Launch NotePad*.

>[!NOTE]
>
>Innan du läser det här avsnittet bör du känna till hur du sammanställer PDF-dokument med hjälp av tjänsten Assembler. I det här avsnittet beskrivs inte koncept, t.ex. att skapa ett samlingsobjekt som innehåller indatadokument eller att lära sig hur du extraherar resultaten från det returnerade samlingsobjektet. (Se [Programmatisk sammansättning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md#programmatically-assembling-pdf-documents).)

>[!NOTE]
>
>Mer information om Assembler-tjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

>[!NOTE]
>
>Mer information om ett DDX-dokument finns i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

## Sammanfattning av steg {#summary-of-steps}

Så här sätter du ihop ett PDF-dokument som innehåller bokmärken:

1. Inkludera projektfiler.
1. Skapa en PDF Assembler-klient.
1. Referera till ett befintligt DDX-dokument.
1. Referera till ett PDF-dokument som bokmärken läggs till i.
1. Referera till XML-dokumentet för bokmärket.
1. Lägg till PDF-dokumentet och bokmärkets XML-dokument i en kartsamling.
1. Ange körningsalternativ.
1. Sammanställ PDF-dokumentet.
1. Spara det PDF-dokument som innehåller bokmärken.

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

Det måste finnas referenser till ett DDX-dokument för att du ska kunna sammanställa ett PDF-dokument. Det här DDX-dokumentet måste innehålla elementet `Bookmarks`, som instruerar Assembler-tjänsten att sätta ihop en PDF som innehåller bokmärken. (Se DDX-dokumentet som visades tidigare i det här avsnittet för ett exempel.)

**Referera till ett PDF-dokument som bokmärken läggs till i**

Referera till ett PDF-dokument som bokmärken läggs till i. Det spelar ingen roll om det refererade PDF-dokumentet redan innehåller bokmärken. Om `Bookmarks`-elementet är underordnat PDF-källelementet ersätter bokmärkena dem som redan finns i PDF-källan. Om du vill behålla de befintliga bokmärkena måste du se till att `Bookmarks` är en jämställd version av PDF-källelementet. Ta till exempel följande exempel:

```xml
 <PDF result="foo">
      <PDF source="inDoc"/>
      <Bookmarks source="doc2"/>
 </PDF>
```

**Referera till XML-dokument för bokmärke**

Om du vill montera ett PDF-dokument som innehåller nya bokmärken måste du referera till ett XML-bokmärkesdokument. Bokmärkets XML-dokument skickas till Assembler-tjänsten i samlingsobjektet för kartor. (Se ett exempel i det XML-dokument för bokmärken som visades tidigare i det här avsnittet.)

>[!NOTE]
>
>Se&quot;Bokmärkesspråk&quot; i [Assembler Service och DDX Reference](https://www.adobe.com/go/learn_aemforms_ddx_63).

**Lägg till PDF-dokumentet och bokmärkets XML-dokument i en kartsamling**

Lägg till både det PDF-dokument som bokmärken läggs till i och det XML-dokument för bokmärken i samlingen Karta. Samlingsobjektet Map innehåller därför två element: ett PDF-dokument och ett XML-dokument för bokmärken.

**Ange körningsalternativ**

Du kan ställa in körningsalternativ som styr beteendet för Assembler-tjänsten när den utför ett jobb. Du kan till exempel ange ett alternativ som instruerar Assembler-tjänsten att fortsätta bearbeta ett jobb om ett fel uppstår. Mer information om de körningsalternativ du kan ange finns i klassreferensen `AssemblerOptionSpec` i [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).

**Sammanställ PDF-dokumentet**

Om du vill montera ett PDF-dokument som innehåller nya bokmärken använder du Assembler-tjänstens `invokeDDX`-åtgärd. Orsaken till att du måste använda åtgärden `invokeDDX` i motsats till andra Assembler-åtgärder som `invokeOneDocument` är att Assembler-tjänsten kräver ett XML-bokmärkesdokument som skickas inom samlingsobjektet för kartan. Det här objektet är en parameter för åtgärden `invokeDDX`.

**Spara det PDF-dokument som innehåller bokmärken**

Extrahera resultaten från det returnerade mappningsobjektet och spara motsvarande PDF-dokument. (Se &quot;Extrahera resultaten&quot; i [Programmatisk sammansättning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md).)

**Se även**

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

[Programmisk sammanställning av PDF-dokument](/help/forms/developing/programmatically-assembling-pdf-documents.md)

## Sammanställa PDF-dokument med bokmärken med Java API {#assemble-pdf-documents-with-bookmarks-using-the-java-api}

Sammanställa ett PDF-dokument med bokmärken med hjälp av Assembler Service API (Java):

1. Inkludera projektfiler.

   Inkludera JAR-klientfiler, som adobe-assembler-client.jar, i Java-projektets klassökväg.

1. Skapa en PDF Assembler-klient.

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper. (Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).)
   * Skapa ett `AssemblerServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Referera till ett befintligt DDX-dokument.

   * Skapa ett `java.io.FileInputStream`-objekt som representerar DDX-dokumentet med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för DDX-filen.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Referera till ett PDF-dokument som bokmärken läggs till i.

   * Skapa ett `java.io.FileInputStream`-objekt med hjälp av konstruktorn och skicka platsen för PDF-dokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka det `java.io.FileInputStream`-objekt som innehåller PDF-dokumentet.

1. Referera till XML-dokumentet för bokmärket.

   * Skapa ett `java.io.FileInputStream`-objekt med hjälp av dess konstruktor och skicka platsen för XML-filen som representerar bokmärkets XML-dokument.
   * Skapa ett `com.adobe.idp.Document`-objekt och skicka `java.io.FileInputStream`-objektet som innehåller PDF-dokumentet.

1. Lägg till PDF-dokumentet och bokmärkets XML-dokument i en kartsamling.

   * Skapa ett `java.util.Map`-objekt som används för att lagra både PDF-indatadokumentet och XML-bokmärkesdokumentet.
   * Lägg till PDF-indatadokumentet genom att anropa `java.util.Map`-objektets `put`-metod och skicka följande argument:

      * Ett strängvärde som representerar nyckelnamnet. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet.
      * Ett `com.adobe.idp.Document`-objekt som innehåller PDF-indatadokumentet.

   * Lägg till XML-bokmärkesdokumentet genom att anropa `java.util.Map`-objektets `put`-metod och skicka följande argument:

      * Ett strängvärde som representerar nyckelnamnet. Värdet måste matcha värdet för källelementet för bokmärken som anges i DDX-dokumentet.
      * Ett `com.adobe.idp.Document`-objekt som innehåller XML-bokmärkesdokumentet.

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ som uppfyller dina affärskrav genom att anropa en metod som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar, anropar du `AssemblerOptionSpec`-objektets `setFailOnError`-metod och skickar `false`.

1. Sammanställ PDF-dokumentet.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande obligatoriska värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar det DDX-dokument som ska användas
   * Ett `java.util.Map`-objekt som innehåller både PDF-indatadokumentet och XML-bokmärkesdokumentet.
   * Ett `com.adobe.livecycle.assembler.client.AssemblerOptionSpec`-objekt som anger körningsalternativen, inklusive standardteckensnitt och jobbloggsnivå

   Metoden `invokeDDX` returnerar ett `com.adobe.livecycle.assembler.client.AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som inträffade.

1. Spara det PDF-dokument som innehåller bokmärken.

   Gör så här för att hämta det nya PDF-dokumentet:

   * Anropa metoden `getDocuments` för objektet `AssemblerResult`. Detta returnerar ett `java.util.Map`-objekt.
   * Upprepa genom objektet `java.util.Map` tills du hittar det resulterande `com.adobe.idp.Document`-objektet. (Du kan använda PDF-resultatelementet som anges i DDX-dokumentet för att hämta dokumentet.)
   * Anropa `com.adobe.idp.Document`-objektets `copyToFile`-metod för att extrahera PDF-dokumentet.

**Se även**

[Snabbstart (SOAP-läge): Sammanställa PDF-dokument med bokmärken med Java API](/help/forms/developing/assembler-service-java-api-quick.md#quick-start-soap-mode-assembling-pdf-documents-with-bookmarks-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Sammanställa PDF-dokument med bokmärken med hjälp av webbtjänstens API {#assemble-pdf-documents-with-bookmarks-using-the-web-service-api}

Sammanställa ett PDF-dokument med bokmärken med Assembler Service API (webbtjänsten):

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
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Referera till ett PDF-dokument som bokmärken läggs till i.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra indata-PDF.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Referera till XML-dokumentet för bokmärket.

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra XML-dokumentet för bokmärket.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för indata-PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll `BLOB`-objektet genom att tilldela dess `MTOM`-fält med innehållet i bytearrayen.

1. Lägg till PDF-dokumentet och bokmärkets XML-dokument i en kartsamling.

   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType`-objekt. Det här samlingsobjektet används för att lagra indata-PDF-dokument och bokmärkets XML-dokument.
   * Skapa ett `MyMapOf_xsd_string_To_xsd_anyType_Item`-objekt för varje PDF-indatadokument och ett XML-bokmärkesdokument.
   * Tilldela ett strängvärde som representerar nyckelnamnet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `key`-fält. Detta värde måste matcha värdet för PDF-källelementet som anges i DDX-dokumentet.
   * Tilldela objektet `BLOB` som lagrar PDF-dokumentet till `MyMapOf_xsd_string_To_xsd_anyType_Item`-objektets `value`-fält.
   * Lägg till objektet `MyMapOf_xsd_string_To_xsd_anyType_Item` i objektet `MyMapOf_xsd_string_To_xsd_anyType`. Anropa `MyMapOf_xsd_string_To_xsd_anyType`-objektets `Add`-metod och skicka `MyMapOf_xsd_string_To_xsd_anyType`-objektet. (Utför den här åtgärden för varje inmatningsdokument i PDF och för bokmärkets XML-dokument.)

1. Ange körningsalternativ.

   * Skapa ett `AssemblerOptionSpec`-objekt som lagrar körningsalternativ med hjälp av dess konstruktor.
   * Ange körningsalternativ för att uppfylla dina affärskrav genom att tilldela ett värde till en datamedlem som tillhör objektet `AssemblerOptionSpec`. Om du till exempel vill instruera Assembler-tjänsten att fortsätta bearbeta ett jobb när ett fel inträffar tilldelar du `false` till `AssemblerOptionSpec`-objektets `failOnError`-datamedlem.

1. Sammanställ PDF-dokumentet.

   Anropa `AssemblerServiceClient`-objektets `invokeDDX`-metod och skicka följande värden:

   * Ett `BLOB`-objekt som representerar DDX-dokumentet
   * Arrayen `MyMapOf_xsd_string_To_xsd_anyType` som innehåller indatadokumenten
   * Ett `AssemblerOptionSpec`-objekt som anger körningsalternativ

   Metoden `invokeDDX` returnerar ett `AssemblerResult`-objekt som innehåller resultatet av jobbet och eventuella undantag som kan ha inträffat.

1. Spara det PDF-dokument som innehåller bokmärken.

   Gör så här för att hämta det nya PDF-dokumentet:

   * Få åtkomst till `documents`-fältet för `AssemblerResult`-objektet, som är ett `Map`-objekt som innehåller PDF-dokument.
   * Upprepa genom objektet `Map` tills du hittar nyckeln som matchar namnet på det resulterande dokumentet. Byt sedan `value` för den arraymedlemmen till en `BLOB`.
   * Extrahera de binära data som representerar PDF-dokumentet genom att komma åt objektets `MTOM`-fält `BLOB`. Detta returnerar en array med byte som du kan skriva till en PDF-fil.

**Se även**

[Anropa AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom)
