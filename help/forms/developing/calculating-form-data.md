---
title: Beräknar formulärdata
description: Använd tjänsten Forms för att beräkna värden som en användare anger i ett formulär och visa resultaten. Forms-tjänsten beräknar värdena med Java API och Web Service API.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/rendering_forms
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms, APIs & Integrations
hide: true
hidefromtoc: true
exl-id: 071a6ccb-8204-4cbc-a39b-143da52c16f7
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1858'
ht-degree: 0%

---

# Beräknar formulärdata {#calculating-form-data}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Forms-tjänsten kan beräkna de värden som en användare anger i ett formulär och visa resultaten. Om du vill beräkna formulärdata måste du utföra två uppgifter. Först skapar du ett formulärdesignskript som beräknar formulärdata. En formulärdesign har stöd för tre typer av skript. En skripttyp körs på klienten, en annan på servern och den tredje typen körs på både servern och klienten. Skripttypen som beskrivs i det här avsnittet körs på servern. Serverberäkningar stöds för HTML-, PDF- och formulärguideomformningar (borttagna).

Som en del av formulärdesignprocessen kan du använda beräkningar och skript för att ge en bättre användarupplevelse. Beräkningar och skript kan läggas till i de flesta formulärfält och objekt. Skapa ett formulärdesignskript för att utföra beräkningsåtgärder på data som användaren matar in i ett interaktivt formulär.

Användaren anger värden i formuläret och klickar på knappen Beräkna för att visa resultatet. I följande process beskrivs ett exempelprogram som gör att en användare kan beräkna data:

* Användaren kommer åt en HTML-sida med namnet StartLoan.html som fungerar som webbprogrammets startsida. Den här sidan anropar en Java-server med namnet `GetLoanForm`.
* `GetLoanForm`-servleten återger ett låneformulär. Det här formuläret innehåller ett skript, interaktiva fält, en beräkningsknapp och en skicka-knapp.
* Användaren anger värden i formulärets fält och klickar på knappen Beräkna. Formuläret skickas till den `CalculateData` Java-server där skriptet körs. Formuläret skickas tillbaka till användaren med beräkningsresultaten visade i formuläret.
* Användaren fortsätter att ange och beräkna värden tills ett tillfredsställande resultat visas. När användaren är nöjd klickar han/hon på knappen Skicka för att bearbeta formuläret. Formuläret skickas till en annan Java-server med namnet `ProcessForm` som ansvarar för att hämta skickade data. (Se [Hantera skickade Forms](/help/forms/developing/rendering-forms.md#handling-submitted-forms).)


I följande diagram visas programmets logikflöde.

![cf_cf_finsrv_loancalcapp_v1](assets/cf_cf_finsrv_loancalcapp_v1.png)

I följande tabell beskrivs stegen i det här diagrammet.

<table>
 <thead>
  <tr>
   <th><p>Steg</p></th>
   <th><p>Beskrivning</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>1</p></td>
   <td><p>Java-servern <code>GetLoanForm</code> anropas från HTML startsida. </p></td>
  </tr>
  <tr>
   <td><p>2</p></td>
   <td><p>Java-serverns <code>GetLoanForm</code> använder Forms klient-API för att återge låneformuläret till klientens webbläsare. Skillnaden mellan återgivning av ett formulär som innehåller ett skript som är konfigurerat att köras på servern och återgivning av ett formulär som inte innehåller något skript är att du måste ange målplatsen som används för att köra skriptet. Om ingen målplats anges körs inte ett skript som är konfigurerat att köras på servern. Tänk dig till exempel programmet som introducerades i det här avsnittet. Java-servern <code>CalculateData</code> är målplatsen där skriptet körs.</p></td>
  </tr>
  <tr>
   <td><p>3</p></td>
   <td><p>Användaren anger data i interaktiva fält och klickar på knappen Beräkna. Formuläret skickas till Java-servern <code>CalculateData</code>, där skriptet körs. </p></td>
  </tr>
  <tr>
   <td><p>4</p></td>
   <td><p>Formuläret återges i webbläsaren med beräkningsresultaten i formuläret. </p></td>
  </tr>
  <tr>
   <td><p>5</p></td>
   <td><p>Användaren klickar på knappen Skicka när värdena är tillräckliga. Formuläret skickas till en annan Java-server med namnet <code>ProcessForm</code>.</p></td>
  </tr>
 </tbody>
</table>

Vanligtvis innehåller ett formulär som skickas som PDF-innehåll skript som körs på klienten. Serverberäkningar kan dock också utföras. En Skicka-knapp kan inte användas för att beräkna skript. I det här fallet utförs inte beräkningar eftersom Forms-tjänsten anser att interaktionen är fullständig.

För att illustrera användningen av ett formulärdesignskript undersöker det här avsnittet ett enkelt interaktivt formulär som innehåller ett skript som är konfigurerat att köras på servern. I följande diagram visas en formulärdesign som innehåller ett skript som lägger till värden som en användare anger i de första två fälten och som visar resultatet i det tredje fältet.

![cf_cf_caldata](assets/cf_cf_caldata.png)

**A.** Ett fält med namnet NumericField1 **B.** Ett fält med namnet NumericField2 **C.** Ett fält med namnet NumericField3

Skriptets syntax i den här formulärdesignen är följande:

```javascript
     NumericField3 = NumericField2 + NumericField1
```

I den här formulärdesignen är knappen Beräkna en kommandoknapp och skriptet finns i knappens `Click`-händelse. När en användare anger värden i de två första fälten (NumericField1 och NumericField2) och klickar på knappen Beräkna, skickas formuläret till Forms-tjänsten där skriptet körs. Forms-tjänsten återger formuläret till klientenheten med resultatet av beräkningen som visas i fältet NumericField3.

>[!NOTE]
>
>Mer information om hur du skapar ett formulärdesignskript finns i [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63).

>[!NOTE]
>
>Mer information om tjänsten Forms finns i [Tjänstreferens för AEM Forms](https://www.adobe.com/go/learn_aemforms_services_63).

## Sammanfattning av steg {#summary-of-steps}

Utför följande uppgifter för att beräkna formulärdata:

1. Inkludera projektfiler.
1. Skapa ett Forms Client API-objekt.
1. Hämta ett formulär som innehåller ett beräkningsskript.
1. Skriv tillbaka formulärdataströmmen till klientens webbläsare

**Inkludera projektfiler**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java, inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa ett Forms Client API-objekt**

Innan du programmässigt kan utföra en API-åtgärd för Forms-tjänstklienten måste du skapa en Forms-tjänstklient. Om du använder Java API skapar du ett `FormsServiceClient`-objekt. Om du använder Forms webbtjänst-API:t skapar du ett `FormsServiceService`-objekt.

**Hämta ett formulär som innehåller ett beräkningsskript**

Du använder API:t för Forms-tjänstklienten för att skapa programlogik som hanterar ett formulär som innehåller ett skript som är konfigurerat att köras på servern. Processen liknar hantering av ett skickat formulär. (Se [Hantera skickade Forms](/help/forms/developing/handling-submitted-forms.md).)

Kontrollera att bearbetningstillståndet som är associerat med det skickade formuläret är `1` `(Calculate)`, vilket innebär att Forms-tjänsten utför en beräkningsåtgärd på formulärdata och att resultaten måste skrivas tillbaka till användaren. I så fall körs ett skript som är konfigurerat att köras på servern automatiskt.

**Skriv tillbaka formulärdataströmmen till klientwebbläsaren**

När du har verifierat att bearbetningstillståndet som är kopplat till ett skickat formulär är `1` måste du skriva tillbaka resultaten till klientens webbläsare. När formuläret visas visas det beräknade värdet i respektive fält.

**Se även**

[Inkluderar AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)
[Beräkna formulärdata med Java API ](/help/forms/developing/calculating-form-data.md#calculate-form-data-using-the-java-api)
[Beräkna formulärdata med webbtjänstens API ](/help/forms/developing/calculating-form-data.md#calculate-form-data-using-the-web-service-api)
[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)
[Forms Service API - snabbstart](/help/forms/developing/forms-service-api-quick-starts.md#forms-service-api-quick-starts)
[Återge interaktiv PDF forms](/help/forms/developing/rendering-interactive-pdf-forms.md)
[Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md)

## Beräkna formulärdata med Java API {#calculate-form-data-using-the-java-api}

Beräkna formulärdata med Forms API (Java):

1. Inkludera projektfiler

   Inkludera JAR-klientfiler, t.ex. adobe-forms-client.jar, i Java-projektets klassökväg.

1. Skapa ett Forms Client API-objekt

   * Skapa ett `ServiceClientFactory`-objekt som innehåller anslutningsegenskaper.
   * Skapa ett `FormsServiceClient`-objekt med hjälp av dess konstruktor och skicka `ServiceClientFactory`-objektet.

1. Hämta ett formulär som innehåller ett beräkningsskript

   * Om du vill hämta formulärdata som innehåller ett beräkningsskript skapar du ett `com.adobe.idp.Document`-objekt med hjälp av dess konstruktor och anropar `javax.servlet.http.HttpServletResponse`-objektets `getInputStream`-metod inifrån konstruktorn.
   * Anropa `FormsServiceClient`-objektets `processFormSubmission`-metod och skicka följande värden:

      * Objektet `com.adobe.idp.Document` som innehåller formulärdata.
      * Ett strängvärde som anger miljövariabler inklusive alla relevanta HTTP-huvuden. Ange den innehållstyp som ska hanteras genom att ange ett eller flera värden för miljövariabeln `CONTENT_TYPE`. Om du till exempel vill hantera XML- och PDF-data anger du följande strängvärde för parametern: `CONTENT_TYPE=application/xml&CONTENT_TYPE=application/pdf`
      * Ett strängvärde som anger rubrikvärdet `HTTP_USER_AGENT`, till exempel `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)`.
      * Ett `RenderOptionsSpec`-objekt som lagrar körningsalternativ.

     Metoden `processFormSubmission` returnerar ett `FormsResult`-objekt som innehåller resultaten av formuläröverföringen.

   * Kontrollera att bearbetningstillståndet som är associerat med ett skickat formulär är `1` genom att anropa `FormsResult`-objektets `getAction`-metod. Om den här metoden returnerar värdet `1` utfördes beräkningen och data kan skrivas tillbaka till klientens webbläsare.

1. Skriv tillbaka formulärdataströmmen till klientens webbläsare

   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skicka en formulärdataström till klientens webbläsare.
   * Skapa ett `com.adobe.idp.Document`-objekt genom att anropa metoden `getOutputContent` för `FormsResult`-objektet.
   * Skapa ett `java.io.InputStream`-objekt genom att anropa `com.adobe.idp.Document`-objektets `getInputStream`-metod.
   * Skapa en bytearray och fyll i den med formulärdataströmmen genom att anropa `InputStream`-objektets `read`-metod och skicka bytearrayen som ett argument.
   * Anropa `javax.servlet.ServletOutputStream`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**


[Inkluderar AEM Forms Java-biblioteksfiler](/help/forms/developing/invoking-aem-forms-using-java.md#including-aem-forms-java-library-files)
[Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties)

## Beräkna formulärdata med webbtjänstens API {#calculate-form-data-using-the-web-service-api}

Beräkna formulärdata med Forms API (webbtjänst):

1. Inkludera projektfiler

   * Skapa Java-proxyklasser som använder Forms tjänst-WSDL.
   * Inkludera Java-proxyklasserna i klassökvägen.

1. Skapa ett Forms Client API-objekt

   Skapa ett `FormsService`-objekt och ange autentiseringsvärden.

1. Hämta ett formulär som innehåller ett beräkningsskript

   * Om du vill hämta formulärdata som har publicerats på en Java-server skapar du ett `BLOB`-objekt med hjälp av dess konstruktor.
   * Skapa ett `java.io.InputStream`-objekt med `javax.servlet.http.HttpServletResponse`-objektets `getInputStream`-metod.
   * Skapa ett `java.io.ByteArrayOutputStream`-objekt med hjälp av dess konstruktor och skicka längden på `java.io.InputStream`-objektet.
   * Kopiera innehållet i objektet `java.io.InputStream` till objektet `java.io.ByteArrayOutputStream`.
   * Skapa en bytearray genom att anropa metoden `toByteArray` för objektet `java.io.ByteArrayOutputStream`.
   * Fyll i objektet `BLOB` genom att anropa dess `setBinaryData`-metod och skicka bytearrayen som ett argument.
   * Skapa ett `RenderOptionsSpec`-objekt med hjälp av dess konstruktor. Ange språkvärdet genom att anropa `RenderOptionsSpec`-objektets `setLocale`-metod och skicka ett strängvärde som anger språkvärdet.
   * Anropa `FormsServiceClient`-objektets `processFormSubmission`-metod och skicka följande värden:

      * Objektet `BLOB` som innehåller formulärdata.
      * Ett strängvärde som anger miljövariabler inkluderade alla relevanta HTTP-huvuden. Du kan till exempel ange följande strängvärde: `HTTP_REFERER=referrer&HTTP_CONNECTION=keep-alive&CONTENT_TYPE=application/xml`
      * Ett strängvärde som anger rubrikvärdet `HTTP_USER_AGENT`, till exempel `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)`.
      * Ett `RenderOptionsSpec`-objekt som lagrar körningsalternativ. Mer information finns i .
      * Ett tomt `BLOBHolder`-objekt som fylls i av metoden.
      * Ett tomt `javax.xml.rpc.holders.StringHolder`-objekt som fylls i av metoden.
      * Ett tomt `BLOBHolder`-objekt som fylls i av metoden.
      * Ett tomt `BLOBHolder`-objekt som fylls i av metoden.
      * Ett tomt `javax.xml.rpc.holders.ShortHolder`-objekt som fylls i av metoden.
      * Ett tomt `MyArrayOf_xsd_anyTypeHolder`-objekt som fylls i av metoden. Den här parametern används för att lagra bifogade filer som skickas tillsammans med formuläret.
      * Ett tomt `FormsResultHolder`-objekt som fylls i av metoden med formuläret som skickas.

     Metoden `processFormSubmission` fyller i parametern `FormsResultHolder` med resultatet av formuläröverföringen. Metoden `processFormSubmission` returnerar ett `FormsResult`-objekt som innehåller resultaten av formuläröverföringen.

   * Kontrollera att bearbetningstillståndet som är associerat med ett skickat formulär är `1` genom att anropa `FormsResult`-objektets `getAction`-metod. Om den här metoden returnerar värdet `1` utfördes beräkningen och data kan skrivas tillbaka till klientens webbläsare.

1. Skriv tillbaka formulärdataströmmen till klientens webbläsare

   * Skapa ett `javax.servlet.ServletOutputStream`-objekt som används för att skicka en formulärdataström till klientens webbläsare.
   * Skapa ett `BLOB`-objekt som innehåller formulärdata genom att anropa `FormsResult`-objektets `getOutputContent`-metod.
   * Skapa en bytearray och fyll i den genom att anropa `BLOB`-objektets `getBinaryData`-metod. Den här aktiviteten tilldelar innehållet i objektet `FormsResult` till bytearrayen.
   * Anropa `javax.servlet.http.HttpServletResponse`-objektets `write`-metod för att skicka formulärdataströmmen till klientens webbläsare. Skicka bytearrayen till metoden `write`.

**Se även**
[Anropa AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding)
