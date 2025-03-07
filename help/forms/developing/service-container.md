---
title: Tjänstbehållare
description: AEM Forms-tjänster i tjänstbehållaren
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: coding, development-tools
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
hide: true
hidefromtoc: true
exl-id: fea20b34-2f66-460e-8b1c-4e55842bc789
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 0%

---

# Tjänstbehållare {#service-container}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

AEM Forms tjänster i tjänstbehållaren (inklusive standardtjänster som krypteringstjänsten, långvariga och kortlivade processer) kan anropas med olika leverantörer, till exempel en EJB-leverantör. Med en EJB-leverantör kan AEM Forms-tjänster anropas via RMI/IIOP. En webbtjänstleverantör visar tjänster som webbtjänster (WSDL Generation) med standarder som SOAP/HTTP och SOAP/JMS.

I följande tabell beskrivs de olika sätt som du kan anropa AEM Forms-tjänster på programmatiskt.

<table>
 <thead>
  <tr>
   <th><p>Anropsmetod</p></th>
   <th><p>Beskrivning</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>Fjärrintegrering</p></td>
   <td><p>Med fjärrintegrering kan Flex-klienter anropa tjänståtgärder. (Se <a href="/help/forms/developing/invoking-aem-forms-using-remoting.md#invoking-aem-forms-using-remoting">Anropa AEM Forms med (borttaget för AEM-formulär) AEM Forms Remoting</a>.)</p></td>
  </tr>
  <tr>
   <td><p>Java API</p></td>
   <td><p>Ett Java API kan anropa en AEM Forms-tjänst. Java API är indelat i klientbibliotek och Java Invocation API. (Se <a href="/help/forms/developing/invoking-aem-forms-using-java.md#invoking-aem-forms-using-the-java-api">Anropa AEM Forms med Java API</a>.)</p></td>
  </tr>
  <tr>
   <td><p>Webbtjänster</p></td>
   <td><p>AEM Forms stöder webbtjänststandarder som SOAP/HTTP. En tjänst kan visas som en webbtjänst där WSDL följer webbtjänststandarder som definieras av W3C.</p><p>En tjänst kan anropas från alla webbtjänststackar, inklusive .NET Framework och Sun™ Web Services SDK. (Se <a href="/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-web-services">Anropa AEM Forms med webbtjänster</a>.)</p></td>
  </tr>
  <tr>
   <td><p>REST-begäranden</p></td>
   <td><p>AEM Forms stöder REST-begäranden. En tjänst kan anropas direkt från en HTML-sida. (Se <a href="/help/forms/developing/invoking-aem-forms-using-rest.md#invoking-aem-forms-using-rest-requests">Anropa AEM Forms med REST-begäranden</a>.)</p></td>
  </tr>
 </tbody>
</table>

Följande bild visar olika sätt som AEM Forms-tjänster kan anropas på med programkod.

>[!NOTE]
>
>Förutom att använda AEM Forms SDK för att skapa klientprogram som kan anropa AEM Forms tjänster, kan du även skapa komponenter som kan distribueras till tjänstbehållaren. Du kan till exempel skapa en bankkomponent som innehåller anpassade datatyper som kan användas i processer. Du kan alltså skapa en datatyp som `com.adobe.idp.BankAccount`. Du kan sedan skapa `com.adobe.idp.BankAccount` instanser i dina klientprogram.

Tjänstbehållaren har följande funktioner:

* Tillåter att AEM Forms tjänster anropas med olika metoder. Du kan konfigurera en tjänst genom att ange slutpunkter så att den kan anropas med alla metoder: Remoting, Java API, web services och REST. (Se [Programmatisk hantering av slutpunkter](/help/forms/developing/programmatically-endpoints.md#programmatically-managing-endpoints).)
* Konverterar ett meddelande till ett normaliserat format som kallas för en anropsbegäran. En anropsbegäran skickas från ett klientprogram (eller en annan tjänst) till en tjänst i tjänstbehållaren. En anropsbegäran innehåller information som namnet på tjänsten som ska anropas och datavärden som krävs för att utföra åtgärden. Många tjänster kräver ett dokument för att utföra en åtgärd. Därför innehåller en anropsbegäran vanligtvis ett dokument, som kan vara PDF-data, XDP-data, XML-data osv.
* Slussar anropsbegäranden till lämpliga tjänster (namnet på tjänsten som ska anropas är en del av anropsbegäran).
* Utför uppgifter som att avgöra om anroparen har behörighet att anropa den angivna tjänståtgärden. Anropsbegäran måste innehålla ett giltigt användarnamn och lösenord för AEM-formulär.

  Det finns olika sätt att skicka en anropsbegäran till en tjänst. Det finns också olika sätt att skicka obligatoriska indatavärden till tjänsten. Anta till exempel att du använder Java API för att anropa en tjänst som kräver ett PDF-dokument. Motsvarande Java-metod innehåller en parameter som godkänner ett PDF-dokument. I det här fallet är parameterns datatyp `com.adobe.idp.Document`. (Se [Skicka data till AEM Forms-tjänster med Java API](/help/forms/developing/invoking-aem-forms-using-java.md#passing-data-to-aem-forms-services-using-the-java-api).)

  Om du anropar en tjänst med bevakade mappar skickas en anropsbegäran när du monterar en fil i en konfigurerad bevakad mapp. Om du anropar en tjänst via e-post skickas en anropsbegäran till en tjänst när ett e-postmeddelande tas emot i en konfigurerad inkorg.

  Tjänstbehållaren skickar tillbaka ett anropssvar när åtgärden har utförts. Ett anropssvar innehåller information om till exempel åtgärdsresultaten. Om åtgärden till exempel ändrar ett PDF-dokument innehåller anropssvaret det ändrade PDF-dokumentet. Om åtgärden misslyckades innehåller anropssvaret ett felmeddelande.

  Ett anropssvar kan hämtas på samma sätt som en anropsbegäran skickas. Det innebär att om anropsbegäran skickas med Java API kan ett anropssvar hämtas med Java API. Anta till exempel att en åtgärd ändrar ett PDF-dokument. Du kan hämta det ändrade PDF-dokumentet genom att hämta returvärdet för Java-metoden som anropade tjänsten.

  När en långvarig process anropas innehåller ett anropssvar ett identifierarvärde som är associerat med anropsbegäran. Med det här identifierarvärdet kan du kontrollera processens status vid ett senare tillfälle. Tänk dig till exempel den långa tjänsten MortgageLoan. Med hjälp av identifierarvärdet kan du kontrollera om processen har slutförts. (Se [Anropar humancentrerade, långlivade processer](/help/forms/developing/invoking-human-centric-long-lived.md#invoking-human-centric-long-lived-processes).)

  I följande diagram visas ett klientprogram (som använder Java API) som anropar en tjänst.

  När ett klientprogram anropar en tjänst inträffar tre händelser:

   1. Ett klientprogram skickar en anropsbegäran till en tjänst.
   1. Tjänsten utför den åtgärd som anges i anropsbegäran.
   1. Tjänstbehållaren returnerar ett anropssvar till klientprogrammet.

**Se även**

[Förstå AEM Forms processer](/help/forms/developing/aem-forms-processes.md#understanding-aem-forms-processes)

[Anropa AEM Forms med (borttaget för AEM-formulär) AEM Forms Remoting](/help/forms/developing/invoking-aem-forms-using-remoting.md#invoking-aem-forms-using-remoting)

[Anropa AEM Forms med Java API](/help/forms/developing/invoking-aem-forms-using-java.md#invoking-aem-forms-using-the-java-api)

[Anropa AEM Forms med Web Services](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-web-services)

[Anropa personalcentrerade, långlivade processer](/help/forms/developing/invoking-human-centric-long-lived.md#invoking-human-centric-long-lived-processes)

[Anropa AEM Forms med REST-begäran](/help/forms/developing/invoking-aem-forms-using-rest.md#invoking-aem-forms-using-rest-requests)
