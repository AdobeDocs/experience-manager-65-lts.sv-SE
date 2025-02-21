---
title: Output Service
description: Beskriver Output Service, som ingår i AEM Document Services
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
docset: aem65
feature: Document Services,Output Service
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Output Service{#output-service}

## Ökning {#overview}

Output service är en OSGi-tjänst som ingår i AEM Document Services. Utdatatjänsten stöder olika utformat och utformningsfunktioner i AEM Forms Designer. Output Service kan konvertera XFA-mallar och XML-data för att generera utskriftsdokument i olika format.

Med Output Service kan du skapa program som gör att du kan:

* Generera slutliga formulärdokument genom att fylla i mallfiler med XML-data.
* Generera utdataformulär i olika format, inklusive icke-interaktiva PDF-, PostScript-, PCL- och ZPL-utskriftsströmmar.
* Generera utskrifts-PDF:er från XFA-formulär-PDF:er.
* Generera flera olika PDF-, PostScript-, PCL- och ZPL-dokument genom att slå samman olika datauppsättningar med medföljande mallar.

>[!NOTE]
>
>Utdatatjänsten är ett 32-bitarsprogram. I Microsoft Windows får 32-bitarsprogram använda maximalt 2 GB minne. Gränsen gäller även för utdatatjänsten.

## Skapa icke-interaktiva formulärdokument {#creating-non-interactive-form-documents}

![använder output_modified](assets/usingoutput_modified.png)

Vanligtvis skapar du mallar med AEM Forms Designer. Med API:erna `generatePDFOutput` och `generatePrintedOutput` för utdatatjänsten kan du direkt konvertera dessa mallar till olika format, bland annat PDF, PostScript, ZPL och PCL.

Åtgärden `generatePDFOutput` genererar PDF-filer, medan åtgärden `generatePrintedOutput` genererar formaten PostScript, ZPL och PCL. Den första parametern i båda åtgärderna godkänner antingen namnet på mallfilen (till exempel `ExpenseClaim.xdp`) eller ett Document-objekt som innehåller mallen. När du anger namnet på mallfilen anger du också innehållsroten som sökväg till mappen som innehåller mallen. Du kan ange innehållsroten antingen med parametern `PDFOutputOptions` eller `PrintedOutputOptions`. Se Javadoc för mer information om andra alternativ som du kan ange med dessa parametrar.

Den andra parametern accepterar ett XML-dokument som sammanfogas med mallen när utdatadokumentet genereras.

Åtgärden `generatePDFOutput` kan också acceptera ett XFA-baserat PDF-formulär som indata och returnera en icke-interaktiv version av PDF-formuläret som utdata.

## Generera icke-interaktiva formulärdokument {#generating-non-interactive-form-documents}

Tänk dig ett scenario där du har en eller flera mallar och flera poster med XML-data för varje mall.

Använd åtgärderna `generatePDFOutputBatch` och `generatePrintedOutputBatch` i utdatatjänsten för att generera ett utskriftsdokument för varje post.

Du kan också kombinera posterna i ett enda dokument. Båda åtgärderna har fyra parametrar.

Den första parametern är en karta som innehåller en godtycklig sträng som nyckel och namnet på mallfilen som värde.

Den andra parametern är en annan karta vars värde är ett Document-objekt som innehåller XML-data. Nyckeln är densamma som den som du anger för den första parametern.

Den tredje parametern för `generatePDFOutputBatch` eller `generatePrintedOutputBatch` är av typen `PDFOutputOptions` respektive `PrintedOutputOptions`.

Parametertyperna är samma som parametertyperna för åtgärderna `generatePDFOutput` och `generatePrintedOutput` och har samma effekt.

Den fjärde parametern är av typen `BatchOptions`, som du använder för att ange om en separat fil kan genereras för varje post. Standardvärdet för den här parametern är false.

Både `generatePrintedOutputBatch` och `generatePDFOutputBatch` returnerar ett värde av typen `BatchResult`. Värdet innehåller en lista med dokument som genererats. Den innehåller också ett metadatadokument i XML-format som innehåller information om varje dokument som genereras.
