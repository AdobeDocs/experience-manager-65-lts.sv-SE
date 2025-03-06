---
title: Arbeta med streckkodade formulär
description: Avkoda data från ett PDF-formulär eller en bild som innehåller en streckkod med Java API och Web Service API.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations,Barcoded Forms
exl-id: 71dc8036-f9a3-4e00-bce1-3f162428053d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1911'
ht-degree: 0%

---

# Arbeta med streckkodade formulär {#working-with-barcoded-forms}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

## Om tjänsten för streckkodade formulär {#about-the-barcoded-forms-service}

Den streckkodade blanketttjänsten automatiserar inhämtning av data från ifyllda och utskrivna blanketter och integrerar inhämtad information i företagets centrala IT-system.

Med hjälp av den streckkodade formulärtjänsten kan du lägga till endimensionella och tvådimensionella streckkoder i interaktiva PDF forms. Du kan sedan publicera streckkodsformulären på en webbplats eller distribuera dem via e-post eller cd. När en användare fyller i ett streckkodsformulär med Adobe Reader, Acrobat Professional eller Acrobat Standard uppdateras streckkoden automatiskt för att koda användarens formulärdata. Användaren kan skicka formuläret elektroniskt eller skriva ut det på papper och skicka det via post, fax eller hand. Du kan senare extrahera användardata som en del av ett automatiserat arbetsflöde och slussa data mellan godkännandeprocesser och affärssystem.

Mer information om den streckkodade formulärtjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Avkoda streckkodade formulärdata {#decoding-barcoded-form-data}

Du kan använda API:t för streckkodsbaserade formulärtjänster för att avkoda data från ett PDF-formulär eller en bild som innehåller en streckkod. Att avkoda formulärdata innebär att extrahera data som finns i streckkoden. Innan data kan avkodas från ett PDF-formulär (eller bild) måste användaren fylla i formuläret med data.

>[!NOTE]
>
>Mer information om den streckkodade formulärtjänsten finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Så här avkodar du data från ett PDF-formulär:

1. Inkludera projektfiler.
1. Skapa ett streckkodat formulärKlient-API-objekt.
1. Hämta ett PDF-formulär som innehåller streckkodsdata.
1. Avkoda data från PDF-formulär.
1. Konvertera data till en XML-datakälla.
1. Bearbeta avkodade data.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

Följande JAR-filer måste läggas till i projektets klassökväg:

* adobe-livecycle-client.jar
* adobe-usermanager-client.jar
* adobe-barcodedforms-client.jar
* adobe-utilities.jar (krävs om AEM Forms används i JBoss)
* jbossall-client.jar (krävs om AEM Forms distribueras på JBoss)
* xercesImpl.jar (i &lt;installationskatalog>/Adobe/Adobe_Experience_Manager_forms/sdk/client-libs\thirdparty)

Om AEM Forms körs på en J2EE-programserver som stöds och som inte är JBOSS, måste du ersätta adobe-utilities.jar och jbossall-client.jar med JAR-filer som är specifika för J2EE-programservern som AEM Forms är distribuerad på. Mer information om platsen för alla AEM Forms JAR-filer finns i [Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files).

**Skapa ett klient-API-objekt för streckkodade formulär**

Innan du programmässigt kan utföra en streckkodsåtgärd måste du skapa en streckkodad Forms-tjänstklient. Om du använder Java API skapar du ett `BarcodedFormsServiceClient`-objekt. Skapa ett `BarcodedFormsServiceService`-objekt om du använder API:t för webbtjänsten för streckkoder.

**Hämta ett PDF-formulär som innehåller streckkodade data**

Hämta ett PDF-formulär som innehåller en streckkod som har fyllts i med användardata.

**Avkoda data från PDF-formuläret**

När du har fått ett PDF-formulär (eller bild) som innehåller en streckkod kan du avkoda data. Tjänsten Barcoded Forms stöder följande typer av streckkoder:

* PDF417-streckkoder.
* Datamatrisstreckkoder.
* QR-kodstreckkoder.
* Kodabarstreckkoder.
* Code 128 barcodes.
* Code 39 barcodes.
* EAN-13-streckkoder.
* EAN-8-streckkoder.

Teckenuppsättningen som hex i avkodnings-API anger att innehållet i streckkoden är kodat som en hexsträng. Om UTF-8 till exempel anges som teckenkodning i formuläret och Hex anges i avkodningsåtgärden, kodas innehållet i streckkoden som en hexsträng i &lt; `xb:content`>-elementet i avkodade utdata. Du kan konvertera det här hexadecimala värdet för att hämta det ursprungliga innehållet genom att skapa programlogik i klientprogrammet.

**Konvertera data till en XML-datakälla**

När du har avkodat formulärdata kan du konvertera dem till XDP- eller XFDF-data. Anta till exempel att du vill importera data till ett annat formulär. Om du vill importera data till ett XFA-formulär måste du konvertera data till XDP-data. Mer information finns i [Importera formulärdata](/help/forms/developing/importing-exporting-data.md#importing-form-data).

**Bearbeta avkodade data**

Du kan bearbeta konverterade data så att de uppfyller dina affärskrav. När du till exempel har avkodat och konverterat data kan du spara dem i en fil, lagra dem i en företagsdatabas, fylla i ett annat formulär och så vidare. I det här avsnittet beskrivs hur du sparar konverterade data som en XML-fil.

>[!NOTE]
>
>Den streckkodade formulärtjänsten kan inte avkoda streckkodsdata när parametrarna för radavgränsare och fältavgränsare har samma värde

**Se även**

[Avkoda streckkodsdata med Java API](barcoded-forms.md#decode-barcoded-form-data-using-the-java-api)

[Avkoda streckkodade formulärdata med webbtjänstens API](barcoded-forms.md#decode-barcoded-form-data-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Avkoda streckkodsdata med Java API {#decode-barcoded-form-data-using-the-java-api}

Avkoda formulärdata med hjälp av API:t för streckkodade formulär (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler i Java-projektets klassökväg.

1. Skapa ett klient-API-objekt för streckkodade formulär

   Skapa ett `BarcodedFormsServiceClient`-objekt med hjälp av dess konstruktor och skicka ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.

1. Hämta ett PDF-formulär som innehåller streckkodsdata

   * Skapa ett `java.io.FileInputStream`-objekt som representerar PDF-formuläret som innehåller streckkodade data med hjälp av dess konstruktor och skicka ett strängvärde som anger platsen för PDF-dokumentet.
   * Skapa ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och skicka `java.io.FileInputStream`-objektet.

1. Avkoda data från PDF-formuläret

   Avkoda formulärdata genom att anropa `BarcodedFormsServiceClient`-objektets `decode`-metod och skicka följande värden:

   * Objektet `com.adobe.idp.Document` som innehåller PDF-formuläret.
   * Ett `java.lang.Boolean`-objekt som anger om en PDF417-streckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en datamatrisstreckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en QR-kodstreckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en codabar-streckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en kod 128-streckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en 39-streckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en EAN-13-streckkod ska avkodas.
   * Ett `java.lang.Boolean`-objekt som anger om en EAN-8-streckkod ska avkodas.
   * Ett `com.adobe.livecycle.barcodedforms.CharSet`-uppräkningsvärde som anger teckenuppsättningens kodningsvärde som används i streckkoden.

   Metoden `decode` returnerar ett `org.w3c.dom.Document`-objekt som innehåller avkodade formulärdata.

1. Konvertera data till en XML-datakälla

   Konvertera avkodade data till antingen XDP- eller XFDF-data genom att anropa `BarcodedFormsServiceClient`-objektets `extractToXML`-metod och skicka följande värden:

   * Objektet `org.w3c.dom.Document` som innehåller avkodade data (kontrollera att du använder `decode`-metodens returvärde).
   * Ett `com.adobe.livecycle.barcodedforms.Delimiter`-uppräkningsvärde som anger radavgränsaren. Du bör ange `Delimiter.Carriage_Return`.
   * Ett `com.adobe.livecycle.barcodedforms.Delimiter`-uppräkningsvärde som anger fältavgränsaren. Ange till exempel `Delimiter.Tab`.
   * Ett `com.adobe.livecycle.barcodedforms.XMLFormat`-uppräkningsvärde som anger om streckkodsdata ska konverteras till XDP- eller XFDF XML-data. Ange till exempel `XMLFormat.XDP` för att konvertera data till XDP-data.

   >[!NOTE]
   >
   >Ange inte samma värden för radavgränsaren och fältavgränsarparametrarna.

   Metoden `extractToXML` returnerar ett `java.util.List`-objekt där varje element är ett `org.w3c.dom.Document`-objekt. Det finns ett separat element för varje streckkod som finns i formuläret. Det vill säga, om det finns fyra streckkoder i formuläret finns det fyra element i det returnerade `java.util.List`-objektet.

1. Bearbeta avkodade data

   * Iterera genom `java.util.List`-objektet för att hämta varje `org.w3c.dom.Document`-objekt som finns i listan.
   * Konvertera `org.w3c.dom.Document`-objektet till ett `com.adobe.idp.Document`-objekt för varje element i listan. (Den programlogik som konverterar ett `org.w3c.dom.Document`-objekt till ett `com.adobe.idp.Document` -objekt visas i streckkodsdata för avkodning med hjälp av Java API-exemplet).
   * Spara XML-data som en XML-fil genom att anropa `copyToFile` för objektet `com.adobe.idp.Document` och skicka ett File-objekt som representerar XML-filen.

**Se även**

[Snabbstart (SOAP-läge): Avkoda streckkodade formulärdata med Java API](/help/forms/developing/barcoded-forms-service-java-api.md#quick-start-soap-mode-decoding-barcoded-form-data-using-the-java-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Avkoda streckkodade formulärdata med webbtjänstens API {#decode-barcoded-form-data-using-the-web-service-api}

Avkoda formulärdata med API:t för streckkodade formulär (webbtjänst):

1. Inkludera projektfiler

   * Skapa en Microsoft .NET-klientsammansättning som använder WSDL för den streckkodade formulärtjänsten. Mer information finns i [Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding).
   * Referera till Microsoft .NET-klientsammansättningen. Mer information finns i&quot;Referera till .NET-klientsammansättningen&quot; i [Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding).

1. Skapa ett klient-API-objekt för streckkodade formulär

   Skapa ett `BarcodedFormsServiceService`-objekt genom att anropa dess standardkonstruktor med hjälp av den Microsoft .NET-klientsammansättning som använder den streckkodade formulärtjänsten WSDL.

1. Hämta ett PDF-formulär som innehåller streckkodsdata

   * Skapa ett `BLOB`-objekt med hjälp av dess konstruktor. Objektet `BLOB` används för att lagra ett PDF-dokument som innehåller en streckkod.
   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för PDF-dokumentet och läget som filen ska öppnas i.
   * Skapa en bytearray som lagrar innehållet i objektet `System.IO.FileStream`. Du kan bestämma storleken på bytearrayen genom att hämta `System.IO.FileStream`-objektets `Length`-egenskap.
   * Fyll i bytearrayen med strömdata genom att anropa `System.IO.FileStream`-objektets `Read`-metod och skicka bytearrayen, startpositionen och strömlängden som ska läsas.
   * Fyll objektet `BLOB` genom att tilldela dess `binaryData`-egenskap med innehållet i bytearrayen.

1. Avkoda data från PDF-formuläret

   Avkoda formulärdata genom att anropa `BarcodedFormsServiceService`-objektets `decode`-metod och skicka följande värden:

   * Objektet `BLOB` som innehåller PDF-formuläret.
   * Ett `Boolean`-objekt som anger om en PDF417-streckkod ska avkodas.
   * Ett `Boolean`-objekt som anger om en datamatrisstreckkod ska avkodas.
   * Ett `Boolean`-objekt som anger om en QR-kodstreckkod ska avkodas.
   * Ett `Boolean`-objekt som anger om en codabar-streckkod ska avkodas.
   * Ett `Boolean`-objekt som anger om en kod 128-streckkod ska avkodas.
   * Ett `Bolean`-objekt som anger om en 39-streckkod ska avkodas.
   * Ett `Boolean`-objekt som anger om en EAN-13-streckkod ska avkodas.
   * Ett `Boolean`-objekt som anger om en EAN-8-streckkod ska avkodas.
   * Ett `CharSet`-uppräkningsvärde som anger teckenuppsättningens kodningsvärde som används i streckkoden.

   Metoden `decode` returnerar ett strängvärde som innehåller avkodade formulärdata.

1. Konvertera data till en XML-datakälla

   Konvertera avkodade data till antingen XDP- eller XFDF-data genom att anropa `BarcodedFormsServiceService`-objektets `extractToXML`-metod och skicka följande värden:

   * Ett strängvärde som innehåller avkodade data (kontrollera att du använder `decode`-metodens returvärde).
   * Ett `Delimiter`-uppräkningsvärde som anger radavgränsaren. Du bör ange `Delimiter.Carriage_Return`.
   * Ett `Delimiter`-uppräkningsvärde som anger fältavgränsaren. Ange till exempel `Delimiter.Tab`.
   * Ett `XMLFormat`-uppräkningsvärde som anger om streckkodsdata ska konverteras till XDP- eller XFDF XML-data. Ange till exempel `XMLFormat.XDP` för att konvertera data till XDP-data.

   >[!NOTE]
   >
   >Ange inte samma värden för radavgränsaren och fältavgränsarparametrarna.

   Metoden `extractToXML` returnerar en `Object`-array där varje element är en `BLOB`-instans. Det finns ett separat element för varje streckkod som finns i formuläret. Det vill säga, om det finns fyra streckkoder i formuläret finns det fyra element i den returnerade `Object`-arrayen.

1. Bearbeta avkodade data

   * Skapa ett `System.IO.FileStream`-objekt genom att anropa dess konstruktor och skicka ett strängvärde som representerar filplatsen för det skyddade PDF-dokumentet.
   * Skapa en bytearray som lagrar datainnehållet för objektet `BLOB` som returnerades av metoden `encryptPDFUsingPassword`. Fyll i bytearrayen genom att hämta värdet för `BLOB`-objektets `binaryData`-datamedlem.
   * Skapa ett `System.IO.BinaryWriter`-objekt genom att anropa dess konstruktor och skicka `System.IO.FileStream`-objektet.
   * Skriv bytearrayens innehåll till en PDF-fil genom att anropa `System.IO.BinaryWriter`-objektets `Write`-metod och skicka bytearrayen.

**Se även**

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
