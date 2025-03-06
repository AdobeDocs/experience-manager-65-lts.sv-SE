---
title: Forms Service
description: Artikeln beskriver Forms-tjänsten och de formulärrelaterade uppgifter du kan utföra med tjänsten Forms.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
feature: Document Services,Forms Service,PDF Generator
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 1e7aa0fa-4440-42fb-8e0b-3c757568b78f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# Forms Service {#forms-service}

## Ökning {#overview}

Med tjänsten Forms kan du skapa interaktiva klientapplikationer för datainhämtning som validerar, bearbetar, omvandlar och levererar blanketter som vanligtvis skapas i Designer. Forms-tjänsten återger alla formulärdesigner som du utvecklar som PDF-dokument.

Med Forms kan man också utöka sina intelligenta datainhämtningsprocesser genom att använda elektroniska blanketter som Adobe PDF:er. Du kan också använda tjänsten för att importera och exportera data till och från befintliga PDF forms.

Använd tjänsten Forms för att göra följande:

* Rendera PDF forms baserat på mallar och XML-data.
* Möjliggör integrering av formulärdata för import av data till och extrahering av data från PDF forms.
* Rendera formulär baserat på fragment.

## Skapa PDF forms  {#creating-pdf-forms-nbsp}

Använd Form Service för att skapa PDF forms för datainhämtning. Vanligtvis börjar du med en AEM Forms Designer-mall. Använd åtgärden `renderPDFForm` (länk till Javadoc) för Forms-tjänsten för att konvertera den här mallen till ett PDF-formulär.

Den första parametern för åtgärden `renderPDFForm` är namnet på mallfilen (till exempel `ExpenseClaim.xdp`). Du kan lagra mallfilen i ett lokalt filsystem, i en CRX-databas eller på en HTTP- eller FTP-plats. Du kan ange platsen för mallfilen genom att ange innehållsroten i parametern `PDFFormRenderOptions` i åtgärden `renderPDFForm`. Mer information om andra alternativ som du kan ange för parametern `PDFFormRenderOptions` finns i Javadoc.

Åtgärden `renderPDFForm` kan också acceptera XML-data. XML-data sammanfogas med mallen när du skapar ett PDF-formulär så att det genererade PDF-formuläret innehåller angivna data. Den andra parametern för åtgärden `renderPDFForm` kan acceptera ett Document-objekt (Javadoc) som innehåller XML-data.

## Extrahera data från PDF forms  {#extracting-data-from-pdf-forms-nbsp}

Använd åtgärden `exportData` (Javadoc) för Forms-tjänsten för att extrahera data-XML från ett PDF-formulär. Den här åtgärden accepterar ett dokument som sin första parameter. Du kan exportera data antingen som ett XDP-dokument eller som en XML-fil. Om du exporterar data som en XML-fil tar exporterade data bort XDP-kuvertet och returnerar en vanlig XML-fil. Du kan ange den här ordningen med den andra parametern.

## Importera data till PDF forms {#importing-data-into-pdf-forms}

Med Forms-tjänsten kan du även sammanfoga ett PDF-formulär som skapats med antingen AEM Forms Designer eller `renderPDFForm`-åtgärden med XML-data. Åtgärden `importData` (Javadoc) i Forms-tjänsten accepterar PDF-formulär och XML-data och returnerar ett PDF-formulär med data-XML.

## Återge formulär som bygger på fragment {#rendering-forms-based-on-fragments}

Forms kan återge formulär baserat på fragment som du skapar med AEM Forms Designer. Ett fragment är en återanvändbar del av ett formulär. Den sparas som en separat XDP-fil som kan infogas i flera formulärdesigner. Ett fragment kan t.ex. innehålla ett adressblock eller juridisk text.

Med fragment blir det enklare och snabbare att skapa och underhålla stora mängder formulär. När du skapar ett formulär infogar du en referens till det fragment som krävs för att fragmentet ska visas i formuläret. Fragmentreferensen innehåller ett delformulär som pekar på den fysiska XDP-filen.

Nedan följer några fördelar med att använda fragment:

* **Återanvändning av innehåll**: Du kan återanvända innehåll i flera formulärdesigner. Om du snabbt vill återanvända delar av samma innehåll i flera formulär skapar du ett fragment. Det tar längre tid att kopiera eller återskapa innehållet. Genom att använda fragment säkerställs också att de delar av en formulärdesign som används ofta har ett konsekvent innehåll och utseende i alla referensformulär.
* **Globala uppdateringar**: Du kan bara göra globala ändringar i flera formulär en gång i en fil. Du kan ändra innehåll, skriptobjekt, databindningar, layout eller format i ett fragment. Alla XDP-formulär som refererar till fragmentet återspeglar ändringarna.
* **Skapa delade formulär**: Du kan dela skapandet av formulär mellan flera resurser. Blankettutvecklare med expertkunskaper inom skript eller andra avancerade funktioner i AEM Forms Designer kan utveckla och dela fragment som använder skript och dynamiska egenskaper. Formulärdesigners kan använda fragmenten för att utforma formulär. Dessutom kan de använda fragmenten för att säkerställa att alla delar av ett formulär har ett konsekvent utseende och funktion i flera formulär.
