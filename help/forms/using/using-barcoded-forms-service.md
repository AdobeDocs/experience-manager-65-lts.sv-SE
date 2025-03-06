---
title: Barcoded Forms Service
description: Använd tjänsten AEM Forms Barcoded Forms för att extrahera data från elektroniska bilder av streckkoder.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
docset: aem65
feature: Document Services,Assembler,Barcoded Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: f823d240-88c7-4357-9dbb-6cc069451721
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# Barcoded Forms Service{#barcoded-forms-service}

## Ökning {#overview}

Tjänsten Barcoded Forms extraherar data från elektroniska bilder av streckkoder. Tjänsten accepterar TIFF- och PDF-filer som innehåller en eller flera streckkoder som indata och extraherar streckkodsdata. Streckkodsdata kan formateras på olika sätt, t.ex. XML, avgränsade strängar eller andra anpassade format som skapats med JavaScript.

Tjänsten Barcoded Forms stöder följande **tvådimensionella (2D)**-symboler som tillhandahålls som skannade TIFF- eller PDF-dokument:

* PDF417
* Datamatris
* QR-kod

Tjänsten stöder även följande **endimensionella**-symboler som tillhandahålls som skannade TIFF- eller PDF-dokument:

* Codabar
* Code128
* Kod 3 av 9
* EAN13
* EAN8

Du kan använda tjänsten Barcoded Forms för att utföra följande uppgifter:

* Extrahera streckkodsdata från streckkodsbilder (TIFF eller PDF). Data lagras som avgränsad text.
* Konvertera avgränsade textdata till XML (XDP eller XFDF). XML-data är enklare att tolka än avgränsad text. Data i XDP- eller XFDF-format kan också användas som indata för andra tjänster i AEM Forms.

För varje streckkod i en bild hittar tjänsten Barcoded Forms streckkoden, avkodar den och extraherar informationen. Tjänsten returnerar streckkodsdata (vid behov med enhetskodning) i ett innehållselement i ett XML-dokument. Följande skannade TIFF-bild av ett formulär innehåller till exempel två streckkoder:

![exempel](assets/example.png)

Tjänsten Barcoded Forms returnerar följande XML-dokument när streckkoderna har avkodats:

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<xb:scanned_image xmlns:xb="https://decoder.barcodedforms.adobe.com/xmlbeans"     path="tiff" version="1.0"> 
    <xb:decode> 
        <xb:date>2007-05-11T15:07:49.965-04:00</xb:date>  
        <xb:host_name>myhost.adobe.com</xb:host_name>  
        <xb:status type="success"> 
            <xb:message />  
        </xb:status> 
    </xb:decode> 
    <xb:barcode id="1"> 
        <xb:header symbology="pdf417"> 
            <xb:location page_no="1"> 
                <xb:coordinates> 
                    <xb:point x="0.119526625" y="0.60945123" />  
                    <xb:point x="0.44457594" y="0.60945123" />  
                    <xb:point x="0.44457594" y="0.78445125" />  
                    <xb:point x="0.119526625" y="0.78445125" />  
                </xb:coordinates> 
            </xb:location> 
        </xb:header> 
        <xb:body> 
            <xb:content encoding="utf-8">t_SID t_FirstName t_MiddleName t_LastName t_nFirstName t_nMiddleName t_nLastName 90210 Patti Y Penne Patti P Prosciutto</xb:content>  
        </xb:body> 
    </xb:barcode> 
    <xb:barcode id="2"> 
        <xb:header symbology="pdf417"> 
            <xb:location page_no="1"> 
                <xb:coordinates> 
                    <xb:point x="0.119526625" y="0.825" />  
                    <xb:point x="0.44457594" y="0.825" />  
                    <xb:point x="0.44457594" y="0.9167683" />  
                    <xb:point x="0.119526625" y="0.9167683" />  
                </xb:coordinates> 
            </xb:location> 
         </xb:header> 
        <xb:body> 
            <xb:content encoding="utf-8">t_FormType t_FormVersion ChangeName 20061128</xb:content>  
         </xb:body> 
    </xb:barcode> 
</xb:scanned_image>
```

## Överväganden för tjänsten {#considerations}

### Arbetsflöden som använder streckkodade formulär {#workflows-that-use-barcoded-forms}

Formulärförfattare skapar interaktiva streckkodsformulär med Designer. (Se [Designer-hjälpen](https://www.adobe.com/go/learn_aemforms_designer_63).) När en användare fyller i ett streckkodsformulär med Adobe Reader eller Acrobat uppdateras streckkoden automatiskt för att koda formulärdata.

Tjänsten Barcoded Forms är användbar vid konvertering av data som finns på papper till elektroniskt format. När ett streckkodsformulär fylls i och skrivs ut kan den utskrivna kopian skannas och användas som indata till tjänsten Barcoded Forms.

Bevakade mappslutpunkter används vanligtvis för att starta program som använder tjänsten Barcoded Forms. Dokumentskannrar kan till exempel spara TIFF- eller PDF-bilder av streckkodade formulär i en bevakad mapp. Den bevakade mappens slutpunkt skickar bilderna till tjänsten för avkodning.

### Rekommenderade kodnings- och avkodningsformat {#recommended-encoding-and-decoding-formats}

Man rekommenderar att man använder ett enkelt, avgränsat format (t.ex. tabbavgränsat) när man kodar data i streckkoder. Undvik också att använda vagnretur som fältavgränsare. Designer har ett urval av avgränsade kodningar som automatiskt genererar JavaScript-skript för att koda streckkoder. Avkodade data har fältnamnen på den första raden och deras värden på den andra raden, med tabbar mellan varje fält.

När du avkodar streckkoder anger du det tecken som används för att avgränsa fält. Det tecken som har angetts för avkodning måste vara samma tecken som användes för att koda streckkoden. Om du till exempel använder det rekommenderade tabbavgränsade formatet måste standardvärdet Tab användas för fältavgränsaren i åtgärden Extrahera till XML.

### Användarspecificerade teckenuppsättningar {#user-specified-character-sets}

När formulärförfattare lägger till streckkodsobjekt i sina formulär med Designer kan de ange en teckenkodning. De identifierade kodningarna är UTF-8, ISO-8859-1, ISO-8859-2, ISO-8859-7, Shift-JIS, KSC-5601, Big-Five, GB-2312, UTF-16. Som standard kodas alla data i streckkoder som UTF-8.

När du avkodar streckkoder kan du ange vilken teckenuppsättningskodning som ska användas. Om du vill vara säker på att alla data avkodas korrekt anger du samma teckenuppsättning som formulärförfattaren angav när formuläret utformades.

### API-begränsningar {#api-limitations}

Tänk på följande begränsningar när du använder BCF API:er:

* Dynamiska formulär stöds inte.
* Interaktiva formulär avkodas inte korrekt om de inte förenklas.
* 1-D-streckkoder får bara innehålla alfanumeriska värden (om sådana stöds). 1-D-streckkoder som innehåller specialsymboler avkodas inte.

### Andra begränsningar {#other-limitations}

Tänk också på följande begränsningar när du använder tjänsten Barcoded Forms:

* Tjänsten stöder AcroForms fullt ut och statiska formulär som innehåller 2D-streckkoder som har sparats med Adobe Reader eller Acrobat. För 1D-streckkoder kan du dock antingen förenkla formuläret eller skicka det som skannat PDF- eller TIFF-dokument.
* Dynamiska XFA-formulär stöds inte helt. Om du vill avkoda 1D- och 2D-streckkoder på rätt sätt i ett dynamiskt formulär kan du antingen förenkla formuläret eller skicka det som skannat PDF- eller TIFF-dokument.

Tjänsten kan dessutom avkoda alla streckkoder som har en symbolik som stöds om ovanstående begränsningar iakttas. Mer information om hur du skapar interaktiva streckkodade formulär finns i [hjälpen för Designer](https://www.adobe.com/go/learn_aemforms_designer_63).

## Konfigurera egenskaper för tjänsten   {#configureproperties}

Du kan använda **AEMFD Barcoded Forms Service** i AEM Console för att konfigurera egenskaper för den här tjänsten. AEM-konsolens standardwebbadress är `https://[host]:'port'/system/console/configMgr`.

## Använda tjänsten {#using}

Barcoded Forms Service innehåller följande två API:er:

* **[decode](https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/bcf/api/BarcodedFormsService.html#decode)**: Avkodar alla streckkoder som är tillgängliga i ett PDF-indatadokument eller en TIFF-bild. Det returnerar ett annat XML-dokument som innehåller data som hämtats från alla streckkoder som finns i indatadokumentet eller bilden.

* **[extractToXML](https://helpx.adobe.com/experience-manager/6-3/forms/javadocs/com/adobe/fd/bcf/api/BarcodedFormsService.html#decode)**: Konvertera data som avkodats med avkodnings-API till XML-data. Dessa XML-data kan sammanfogas med ett XFA-formulär. Den returnerar en lista med XML-dokument, en för varje streckkod.

### Använda BCF Service med JSP eller Servlets {#using-bcf-service-with-a-jsp-or-servlets}

Följande exempelkod avkodar en streckkod i ett dokument och sparar XML-utdata på disken.

```jsp
<%@ page import="java.util.List,
                com.adobe.fd.bcf.api.BarcodedFormsService,
                com.adobe.fd.bcf.api.CharSet,
                com.adobe.fd.bcf.api.Delimiter,
                com.adobe.fd.bcf.api.XMLFormat,
                com.adobe.aemfd.docmanager.Document,
                javax.xml.transform.Transformer,
                javax.xml.transform.TransformerFactory,
                java.io.StringWriter,
                javax.xml.transform.stream.StreamResult,
                javax.xml.transform.dom.DOMSource,
                javax.servlet.jsp.JspWriter,
                org.apache.commons.lang.StringEscapeUtils" %><%
%><%@taglib prefix="sling" uri="https://sling.apache.org/taglibs/sling/1.0" %><%
%><sling:defineObjects/><%

 // Get reference to BarcodedForms OSGi service.
 // Within an OSGi bundle @Reference annotation 
 // can be used to retrieve service reference

 BarcodedFormsService bcfService = sling.getService(BarcodedFormsService.class);

 // Path to image containing barcodes in AEM repository. 
 // Replace this with path to image in your repository
 String documentPath = "/content/dam/TestImage-010.tif";

 // Create a Docmanager Document object for 
 // the tiff file containing barcode
 // See Docmanager Document javadoc for
 // more details
 Document inputDoc = new Document(documentPath);

 // Invoke decode operation of barcoded forms service 
 // Second parameter is set to true to decode PDF417 barcode symbology
 // See javadoc for details of parameters

 org.w3c.dom.Document result = bcfService.decode(inputDoc, // Input Document Object
                                                    true, 
                                                    false,  
                                                    false,
                                                    false,
                                                    false,
                                                    false,
                                                    false,
                                                    false,
                                                    CharSet.UTF_8);// use UTF-8 character encoding 

 out.println("<h2> Output Returned from decode operation </h2>");
 printDoc(result,out);

 // Invoke extractToXML to convert Carriage 
 // return / Tab delimited data to XML 

 List<org.w3c.dom.Document> listResult = bcfService.extractToXML(result, // w3c document from the output of decode operation
                                                                    Delimiter.Carriage_Return, // Lines are separated using "\r"
                                                                    Delimiter.Tab, // Fields are separated using "\t" 
                                                                    XMLFormat.XDP); // wraps generated XML in <xdp:xdp> packet

 out.println("<h2> Output returned from extractToXML </h2>");
 printDoc(listResult.get(0),out);

%><%!

 // helper function to convert w3c document to string

 String convertToString(org.w3c.dom.Document inputDoc) throws Exception {
   TransformerFactory tf = TransformerFactory.newInstance();
   Transformer tr = tf.newTransformer();
   StringWriter sw = new StringWriter();
   StreamResult sr = new StreamResult(sw);
   tr.transform(new DOMSource(inputDoc), sr);
   return sw.toString();
 }

 // helper function to render XML to response

 void printDoc(org.w3c.dom.Document inputDoc,JspWriter out) throws Exception {
   String escapedString = StringEscapeUtils.escapeHtml(convertToString(inputDoc));
   out.println(escapedString);
 }

%>
```

### Använda BCF-tjänsten med AEM Workflows {#using-the-bcf-service-with-aem-workflows}

Att köra Barcoded Forms-tjänsten från ett arbetsflöde påminner om att köra tjänsten från JSP/Servlet. Den enda skillnaden är när tjänsten körs från JSP/Servlet som dokumentobjektet automatiskt hämtar en instans av ResourceResolver-objektet från objektet ResourceResolverHelper. Den här automatiska mekanismen fungerar inte när koden anropas från ett arbetsflöde.

För ett arbetsflöde skickar du en instans av ResourceResolver-objektet explicit till Document-klasskonstruktorn. Dokumentobjektet använder sedan det angivna ResourceResolver-objektet för att läsa innehåll från databasen.

Följande exempelarbetsflödesprocess avkodar en streckkod i ett dokument och sparar resultatet på disken. Koden skrivs i ECMAScript och dokumentet skickas som arbetsflödets nyttolast:

```javascript
/*
 * Imports 
 */
var BarcodedFormsService = Packages.com.adobe.fd.bcf.api.BarcodedFormsService;
var CharSet = Packages.com.adobe.fd.bcf.api.CharSet;

var Document = Packages.com.adobe.aemfd.docmanager.Document;
var File = Packages.java.io.File;
var FileOutputStream = Packages.java.io.FileOutputStream;
var TransformerFactory = Packages.javax.xml.transform.TransformerFactory;
var Transformer = Packages.javax.xml.transform.Transformer;
var StreamResult = Packages.javax.xml.transform.stream.StreamResult;
var DOMSource = Packages.javax.xml.transform.dom.DOMSource;

var ResourceResolverFactory = Packages.org.apache.sling.api.resource.ResourceResolverFactory;

// get reference to BarcodedFormsService
var bcfService = sling.getService(BarcodedFormsService);

/*
 * workflow payload and path to it
 */
var payload = graniteWorkItem.getWorkflowData().getPayload();
var payload_path = payload.toString();

/* Create resource resolver using current session 
 * this resource resolver needs to be passed to Document
 * object constructor when file is to be read from 
 * crx repository. 
 */

/* get ResourceResolverFactory service reference , used 
 * to construct resource resolver
 */
var resourceResolverFactory = sling.getService(ResourceResolverFactory);

var authInfo = {
    "user.jcr.session":graniteWorkflowSession.getSession()
};

var resourceResolver = resourceResolverFactory.getResourceResolver(authInfo);

// Create Document object from payload_path 
var inputDocument = new Document(payload_path, resourceResolver);

// invoke decode operation to decode barcodes in inputDocument
var decodedBarcode = bcfService.decode(inputDocument, true, false, false, false, false, false, false, false, CharSet.UTF_8);

// save decoded barcode data to disk
saveW3CDocument(decodedBarcode, "C:/temp/decoded.xml");

/*
 * Helper function to save W3C Document
 * to a file on disk
 */
function saveW3CDocument(inputDoc, filePath) {
   var tf = TransformerFactory.newInstance();
   var tr = tf.newTransformer();
   var os = new FileOutputStream(filePath);
   var sr = new StreamResult(os);
   tr.transform(new DOMSource(inputDoc), sr);
   os.close();
}
```
