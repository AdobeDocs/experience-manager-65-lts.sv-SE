---
title: Arbeta med XMP Utilities
description: Använd XMP Utilities Java och Web Service API:er för att programmässigt importera XMP-metadata till ett PDF-dokument och hämta och spara XMP-metadata från ett PDF-dokument.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Services,APIs & Integrations
exl-id: 6deed56a-2e87-4444-8fb5-1d06b0792a5e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 0%

---

# Arbeta med XMP Utilities {#working-with-xmp-utilities}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

**Om XMP Utilities Service**

PDF-dokument innehåller metadata, vilket är information om dokumentet som skiljer sig från dokumentets innehåll, t.ex. text och bilder. Adobe Extensible Metadata Platform (XMP) är en standard för hantering av dokumentmetadata.

XMP-verktygen kan hämta och spara XMP-metadata från PDF-dokument och importera XMP-metadata till PDF-dokument.

Du kan utföra dessa uppgifter med tjänsten XMP Utilities:

* Importera metadata till PDF-dokument. (Se [Importera metadata till PDF-dokument](xmp-utilities.md#importing-metadata-into-pdf-documents).)
* Exportera metadata från PDF-dokument. (Se [Exportera metadata från PDF-dokument](xmp-utilities.md#exporting-metadata-from-pdf-documents).)

>[!NOTE]
>
>Mer information om tjänsten XMP Utilities finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Importera metadata till PDF-dokument {#importing-metadata-into-pdf-documents}

Du kan använda XMP Utilities Java och webbtjänstens API:er för att programmässigt importera XMP-metadata till ett PDF-dokument. Metadata innehåller information om ett PDF-dokument, t.ex. dokumentets författare och nyckelord som hör till dokumentet. Metadata kan finnas i dokumentets dialogruta Dokumentegenskaper, som på följande bild.

![ww_ww_metadata_dialog](assets/ww_ww_metadatadialog.png)

Om du vill importera metadata programmatiskt till ett PDF-dokument kan du använda ett befintligt XML-dokument som anger metadatavärdena eller så kan du använda ett objekt av typen `XMPUtilityMetadata`. (Se [AEM Forms API Reference](https://www.adobe.com/go/learn_aemforms_javadocs_63_en).)

>[!NOTE]
>
>I det här avsnittet beskrivs hur du använder ett XML-dokument för att importera metadata till ett PDF-dokument.

Följande XML-kod innehåller metadatavärden som motsvarar föregående bild. Observera till exempel feta objekt, som anger nyckelord.

```xml
 <?xpacket begin="?" id="W5M0MpCehiHzreSzNTczkc9d"?>
 <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="Adobe XMP Core 4.2-jc015 52.349034, 2008 Jun 20 00:30:39-PDT (debug)">
       <rdf:RDF xmlns:rdf="https://www.w3.org/1999/02/22-rdf-syntax-ns#">
          <rdf:Description rdf:about=""
                xmlns:xmp="https://ns.adobe.com/xap/1.0/">
             <xmp:MetadataDate>2008-10-22T10:52:21-04:00</xmp:MetadataDate>
             <xmp:CreatorTool>AEM Forms</xmp:CreatorTool>
             <xmp:ModifyDate>2008-10-22T10:52:21-04:00</xmp:ModifyDate>
             <xmp:CreateDate>2008-02-13T11:00:18-05:00</xmp:CreateDate>
          </rdf:Description>
          <rdf:Description rdf:about=""
                xmlns:pdf="https://ns.adobe.com/pdf/1.3/">
             <pdf:Producer>AEM Forms</pdf:Producer>
             <pdf:Keywords>keyword1, keyword2, keyword3,keyword4</pdf:Keywords>
          </rdf:Description>
          <rdf:Description rdf:about=""
                xmlns:xmpMM="https://ns.adobe.com/xap/1.0/mm/">
             <xmpMM:DocumentID>uuid:1cce1f84-331e-4d8d-8538-15441c271dd7</xmpMM:DocumentID>
             <xmpMM:InstanceID>uuid:cdda0ca6-7c91-4771-9dc9-796c8fe59350</xmpMM:InstanceID>
          </rdf:Description>
          <rdf:Description rdf:about=""
                >
             <dc:format>application/pdf</dc:format>
             <dc:description>
                <rdf:Alt>
                   <rdf:li xml:lang="x-default">Adobe Designer Sample</rdf:li>
                </rdf:Alt>
             </dc:description>
             <dc:title>
                <rdf:Alt>
                   <rdf:li xml:lang="x-default">Grant Application</rdf:li>
                </rdf:Alt>
             </dc:title>
             <dc:creator>
                <rdf:Seq>
                   <rdf:li>Tony Blue</rdf:li>
                </rdf:Seq>
             </dc:creator>
             <dc:subject>
                <rdf:Bag>
                   <rdf:li>keyword1</rdf:li>
                   <rdf:li>keyword2</rdf:li>
                   <rdf:li>keyword3</rdf:li>
                   <rdf:li>keyword4</rdf:li>
                </rdf:Bag>
             </dc:subject>
          </rdf:Description>
          <rdf:Description rdf:about=""
                xmlns:desc="https://ns.adobe.com/xfa/promoted-desc/">
             <desc:version rdf:parseType="Resource">
                <rdf:value>1.0</rdf:value>
                <desc:ref>/template/subform[1]</desc:ref>
             </desc:version>
             <desc:contact rdf:parseType="Resource">
                <rdf:value>Adobe Systems Incorporated</rdf:value>
                <desc:ref>/template/subform[1]</desc:ref>
             </desc:contact>
          </rdf:Description>
       </rdf:RDF>
 </x:xmpmeta>
```

>[!NOTE]
>
>Mer information om tjänsten XMP Utilities finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary-of-steps}

Så här importerar du XMP-metadata till ett PDF-dokument:

1. Inkludera projektfiler.
1. Skapa en XMPUtilityService-klient.
1. Anropa importen av XMP-metadata.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa en XMPUtilityService-klient**

Innan du programmässigt kan utföra en XMP Utilities-åtgärd måste du skapa en XMPUtilityService-klient. Med Java-API:t uppnås detta genom att ett `XMPUtilityServiceClient`-objekt skapas. Med webbtjänstens API:er uppnås detta med hjälp av ett `XMPUtilityServiceService`-objekt.

**Anropa importen av XMP-metadata**

När du har skapat tjänstklienten kan du starta någon av XMP metadataimportåtgärder för att importera XMP-metadata till det angivna PDF-dokumentet.

**Se även**

[Importera XMP-metadata med Java API](xmp-utilities.md#import-xmp-metadata-using-the-java-api)

[Importera XMP-metadata med hjälp av webbtjänstens API](xmp-utilities.md#importing-xmp-metadata-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Importera XMP-metadata med Java API {#import-xmp-metadata-using-the-java-api}

Importera XMP-metadata med hjälp av XMP Utilities API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-pdfutility-client.jar, i Java-projektets klassökväg.

   >[!NOTE]
   >
   >Filen adobe-pdfutility-client.jar innehåller klasser som gör att du kan anropa tjänsten XMP Utilities programmatiskt.

1. Skapa en XMPUtilityService-klient

   Skapa ett `XMPUtilityServiceClient`-objekt med hjälp av dess konstruktor och skicka ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.

1. Anropa import av XMP-metadata

   Om du vill ändra XMP-metadata anropar du antingen `XMPUtilityServiceClient`-objektets `importMetadata`-metod eller dess `importXMP`-metod.

   Om du använder metoden `importMetadata` skickar du följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar PDF-filen.
   * Ett `XMPUtilityMetadata`-objekt som innehåller de metadata som ska importeras.

   Om du använder metoden `importXMP` skickar du följande värden:

   * Ett `com.adobe.idp.Document`-objekt som representerar PDF-filen.
   * Ett `com.adobe.idp.Document`-objekt som representerar en XML-fil som innehåller de metadata som ska importeras.

   I båda fallen är det returnerade värdet ett `com.adobe.idp.Document`-objekt som representerar PDF-filen med de importerade metadata. Du kan sedan spara objektet på hårddisken.

**Se även**

[Importera metadata till PDF-dokument](xmp-utilities.md#importing-metadata-into-pdf-documents)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Importera XMP-metadata med hjälp av webbtjänstens API {#importing-xmp-metadata-using-the-web-service-api}

Så här importerar du XMP-metadata med webbtjänstens API för XMP Utilities:

1. Inkludera projektfiler

   * Skapa en Microsoft .NET-klientsammansättning som använder XMP Utilities-tjänstens WSDL-fil. (Se [Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding).)
   * Referera till Microsoft .NET-klientsammansättningen. (Se [Skapa en .NET-klientsammansättning som använder Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#creating-a-net-client-assembly-that-uses-base64-encoding).)

1. Skapa en XMPUtilityService-klient

   Skapa ett `XMPUtilityServiceService`-objekt med hjälp av din proxyklasskonstruktor.

1. Anropa import av XMP-metadata

   Om du vill ändra XMP-metadata anropar du antingen `XMPUtilityServiceService`-objektets `importMetadata`-metod eller dess `importXMP`-metod.

   Om du använder metoden `importMetadata` skickar du följande värden:

   * Ett `BLOB`-objekt som representerar PDF-filen.
   * Ett `XMPUtilityMetadata`-objekt som innehåller de metadata som ska importeras.

   Om du använder metoden `importXMP` skickar du följande värden:

   * Ett `BLOB`-objekt som representerar PDF-filen.
   * Ett `BLOB`-objekt som representerar en XML-fil som innehåller de metadata som ska importeras.

   I båda fallen är det returnerade värdet ett `BLOB`-objekt som representerar PDF-filen med de importerade metadata. Du kan sedan spara objektet på hårddisken.

**Se även**

[Importera metadata till PDF-dokument](xmp-utilities.md#importing-metadata-into-pdf-documents)

<!--REVIEW: [Quick Start (Base64): Importing XMP metadata using the web service API](unresolvedlink-lc-qs-xmp-utilities-xu.xml#ws624e3cba99b79e12e69a9941333732bac8-7be8.2)-->

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)

[Skapa en .NET-klientsammansättning som använder Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#creating-a-net-client-assembly-that-uses-base64-encoding)

## Exportera metadata från PDF-dokument {#exporting-metadata-from-pdf-documents}

Du kan använda XMP Utilities Java och webbtjänstens API:er för att hämta och spara XMP-metadata från ett PDF-dokument.

>[!NOTE]
>
>Mer information om tjänsten XMP Utilities finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

### Sammanfattning av steg {#summary_of_steps-1}

Så här exporterar du XMP-metadata från ett PDF-dokument:

1. Inkludera projektfiler.
1. Skapa en XMPUtilityService-klient.
1. Anropa export av XMP-metadata.

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa en XMPUtilityService-klient**

Innan du programmässigt kan utföra en XMP Utilities-åtgärd måste du skapa en XMPUtilityService-klient. Med Java-AP uppnås detta genom att ett `XMPUtilityServiceClient`-objekt skapas. Med webbtjänstens API:er uppnås detta med ett `XMPUtilityServiceService`-objekt.

**Starta export av XMP-metadata**

När du har skapat tjänstklienten kan du starta någon av XMP metadataexportåtgärder som kan användas för att undersöka XMP metadata eller spara dem på hårddisken.

**Se även**

[Importera XMP-metadata med Java API](xmp-utilities.md#import-xmp-metadata-using-the-java-api)

[Importera XMP-metadata med hjälp av webbtjänstens API](xmp-utilities.md#importing-xmp-metadata-using-the-web-service-api)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Exportera XMP-metadata med Java API {#export-xmp-metadata-using-the-java-api}

Exportera XMP-metadata med hjälp av XMP Utilities API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-pdfutility-client.jar, i Java-projektets klassökväg.

   >[!NOTE]
   >
   >Filen adobe-pdfutility-client.jar innehåller klasser som gör att du kan anropa tjänsten XMP Utility programmatiskt.

1. Skapa en XMPUtilityService-klient

   Skapa ett `XMPUtilityServiceClient`-objekt med hjälp av dess konstruktor och skicka ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.

1. Anropa import av XMP-metadata

   Om du vill inspektera XMP-metadata anropar du `XMPUtilityServiceClient`-objektets `exportMetadata`-metod och skickar ett `com.adobe.idp.Document`-objekt som representerar PDF-filen. Metoden returnerar ett `XMPUtilityMetadata`-objekt som innehåller de hämtade metadata.

   Om du vill hämta och spara XMP-metadata anropar du `XMPUtilityServiceClient`-objektets `exportXMP`-metod och skickar ett `com.adobe.idp.Document`-objekt som representerar PDF-filen. Metoden returnerar ett `com.adobe.idp.Document`-objekt som innehåller hämtade metadata, som du sedan kan spara på disken som en XML-fil.

**Se även**

[Exportera metadata från PDF-dokument](xmp-utilities.md#exporting-metadata-from-pdf-documents)

[Inkludera AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)

[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

### Exportera XMP-metadata med webbtjänstens API {#export-xmp-metadata-using-the-web-service-api}

Exportera XMP-metadata med XMP Utilities API (webbtjänst):

1. Inkludera projektfiler

   * Skapa en Microsoft .NET-klientsammansättning som använder XMP Utilities-tjänstens WSDL-fil.
   * Referera till Microsoft .NET-klientsammansättningen.

1. Skapa en XMPUtilityService-klient

   Skapa ett `XMPUtilityServiceService`-objekt med hjälp av din proxyklasskonstruktor.

1. Anropa import av XMP-metadata

   Om du vill inspektera XMP-metadata anropar du `XMPUtilityServiceClient`-objektets `exportMetadata`-metod och skickar ett `BLOB`-objekt som representerar PDF-filen. Metoden returnerar ett `XMPUtilityMetadata`-objekt som innehåller de hämtade metadata.

   Om du vill hämta och spara XMP-metadata anropar du `XMPUtilityServiceClient`-objektets `exportXMP`-metod och skickar ett `BLOB`-objekt som representerar PDF-filen. Metoden returnerar ett `BLOB`-objekt som innehåller hämtade metadata, som du sedan kan spara på disken som en XML-fil.

**Se även**

[Exportera metadata från PDF-dokument](xmp-utilities.md#exporting-metadata-from-pdf-documents)

[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)

[Skapa en .NET-klientsammansättning som använder Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#creating-a-net-client-assembly-that-uses-base64-encoding)
