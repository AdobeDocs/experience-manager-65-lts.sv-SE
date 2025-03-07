---
title: Återger Forms
description: Använd tjänsten Forms för att skapa interaktiva klientapplikationer för datainhämtning som validerar, bearbetar, omvandlar och levererar blanketter som vanligtvis skapas i Designer. Formulärförfattare kan utveckla en enda formulärdesign som Forms-tjänsten återger i PDF, SWF eller HTML i olika webbläsarmiljöer.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/performing_service_operations_using_apis
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
hide: true
hidefromtoc: true
exl-id: 3f1f9ecb-be62-4428-8db8-23c57081b0f7
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# Återger Forms {#rendering-forms}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

**Om Forms-tjänsten**

Med tjänsten Forms kan du skapa interaktiva klientapplikationer för datainhämtning som validerar, bearbetar, omvandlar och levererar blanketter som vanligtvis skapas i Designer. Formulärförfattare kan utveckla en enda formulärdesign som Forms-tjänsten återger i PDF, SWF eller HTML i olika webbläsarmiljöer.

När en slutanvändare begär ett formulär skickar ett klientprogram begäran till Forms-tjänsten, som returnerar formuläret i lämpligt format. Så snart Forms-tjänsten tar emot en begäran sammanfogar den data med en formulärdesign och skickar sedan formuläret i önskat format. Utdata från formulärtjänsten är ett interaktivt formulär, vanligtvis ett PDF-dokument. Med ett interaktivt formulär kan användarna fylla i fält som finns i formuläret.

Beroende på vilken typ av klientprogram du använder kan du skriva formuläret till en webbläsare eller spara formuläret som en PDF-fil. Ett webbaserat program kan skriva formuläret till webbläsaren. Ett skrivbordsprogram kan spara formuläret som en PDF-fil. För att visa hur du skriver ut till en webbläsare och till en PDF-fil ordnas snabbstarterna i avsnittet *Återge Forms* på följande sätt:

* Exemplen med Java API med stark typ (SOAP-läge) är en Java-server.
* Webbtjänstens (Java Base64) exempel är en Java-server.
* Webbtjänstexemplen (MTOM) är ett konsolprogram (alla snabbstarter har inte ett MTOM-exempel).

>[!NOTE]
>
>Mer information om hur du skapar ett webbprogram som använder java-servrar för att anropa Forms-tjänsten finns i [Skapa webbprogram som återger Forms](/help/forms/developing/creating-web-applications-renders-forms.md).

Du kan skicka en formulärdesign (en XDP-fil) eller ett PDF-dokument till Forms-tjänsten på något av följande två sätt:

* Du kan referera till formulärdesignen med ett URL-värde. Det här tillvägagångssättet innebär att du använder ett `URLSpec`-objekt. Innehållsroten skickas till Forms-tjänsten med `URLSpec`-objektets `setContentRootURI`-metod. Formulärdesignnamnet ( `formQuery`) skickas som en separat parameter. De två värdena sammanfogas för att få den absoluta referensen till formulärdesignen. (Det mesta av snabbstarterna i avsnittet *Återgivning av Forms* använder den här metoden.)
* Du kan skicka en `com.adobe.idp.Document` som innehåller formulärdesignen till Forms-tjänsten. Två nya metoder med namnet `renderPDFForm2` och `renderHTMLForm2` accepterar ett `com.adobe.idp.Document`-objekt som innehåller en formulärdesign. (Se [Skicka dokument till Forms-tjänsten](/help/forms/developing/passing-documents-forms-service.md)

Du kan utföra följande uppgifter med Forms-tjänsten:

* Återge interaktiva PDF forms. (Se [Återge interaktiv PDF forms](/help/forms/developing/rendering-interactive-pdf-forms.md).)
* Återge formulär på klienten. (Se [Återge Forms på klienten](/help/forms/developing/rendering-forms-client.md).)
* Rendera formulär baserat på fragment. (Se [Återge Forms baserat på fragment](/help/forms/developing/rendering-forms-based-fragments.md).)
* Återge rättighetsaktiverade formulär. (Se [Renderingsaktiverad Forms](/help/forms/developing/rendering-rights-enabled-forms.md).)
* Återge formulär som HTML. (Se [Återge Forms som HTML](/help/forms/developing/rendering-forms-html.md).)
* Återger HTML Forms med anpassade CSS-filer ([Återger HTML Forms med anpassade CSS-filer](/help/forms/developing/rendering-html-forms-using-custom.md).)
* Hantera skickade formulär. (Se [Hantera skickade Forms](/help/forms/developing/handling-submitted-forms.md).)
* Skapa PDF-dokument med skickade XML-data. (Se [Skapa PDF-dokument med skickade XML-data](/help/forms/developing/creating-pdf-documents-submitted-xml.md).)
* Fyll i formulär i förväg. (Se [Förifyll Forms med flödeslayouter](/help/forms/developing/prepopulating-forms-flowable-layouts.md).)
* Skicka dokument. (Se [Skicka dokument till Forms-tjänsten](/help/forms/developing/passing-documents-forms-service.md)
* Beräkna formulärdata. (Se [Beräkna formulärdata](/help/forms/developing/calculating-form-data.md).)
* Optimera ett program. (Se [Optimera prestanda för Forms-tjänsten](/help/forms/developing/optimizing-performance-forms-service.md).)
