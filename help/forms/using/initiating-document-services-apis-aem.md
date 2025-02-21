---
title: Initiera API:er för dokumenttjänster från AEM Workflow
description: Lär dig hur du anropar AEM Document Services på DDX eller angivna indata. Se även hur du konverterar PDF till PDF/A
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
solution: Experience Manager, Experience Manager Forms
feature: Interactive Communication
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1160'
ht-degree: 0%

---

# Initiera API:er för dokumenttjänster från AEM Workflow  {#initiate-document-services-apis-from-aem-workflow}

## Assembler {#assembler}

AEM Forms tillhandahåller anpassade arbetsflöden för att anropa följande API:er för Assembler-tjänster:

* **invoke**: Anropar åtgärder som anges i indata-DDX för angivna indata.
* **toPDFA**: Konverterar PDF-indatadokument till PDF/A-dokument.

### Anropa DDX-arbetsflöde {#invoke-ddx-workflow}

Arbetsflödet **Anropa DDX** anropar API:t för tjänsten `Invoke` Assembler, som du kan använda för att montera eller dela upp dokument, lägga till vattenstämpel i en PDF och så vidare.

1. Dra arbetsflödessteget **[!UICONTROL Invoke DDX]** under fliken Forms Workflow i Sidekick.
1. Dubbelklicka på det tillagda arbetsflödessteget för att redigera komponenten.
1. Konfigurera indatadokument, miljöalternativ och utdatadokument i dialogrutan Redigera komponent och klicka på **[!UICONTROL OK]**.

#### Indatadokument {#input-documents}

Anropa DDX-arbetsflödet kräver följande indatadokument:

* **DDX**: Det är en obligatorisk inmatning för arbetsflödessteget Anropa DDX och kan anges genom att välja ett av följande alternativ i den nedrullningsbara menyn DX-indata.

   * *Relativt till nyttolast*: DDX-indatafilen är relativ till nyttolastmappen för arbetsflödesobjektet.
   * *Använd nyttolast*: Nyttolasten för arbetsflödesobjektet används som indata-DDX-dokument.
   * *Absolut sökväg*: Den absoluta sökvägen till DDX-dokumentet i CRX-databasen.

* **Skapa karta från PayLoad**: När du väljer det här alternativet läggs alla dokument under nyttolastmappen till i indatadokumentets karta för `invoke`-API:t i Assembler. Nodnamnet för varje dokument används som en nyckel på kartan.

* **Karta för indatadokument**: Anger kartan för indatadokument. Du kan lägga till valfritt antal poster, där varje post anger dokumentets nyckel på kartan och dokumentets källa.

#### Miljöalternativ {#environment-options}

På fliken Miljöalternativ kan du ange olika bearbetningsalternativ för anrops-API:t.

* *Jobbloggnivå*: Anger loggnivån för bearbetningsloggarna.
* *Validera endast*: Kontrollerar giltigheten för indata-DDX.

* *Fel vid fel*: Anger om anropet till Assembler-tjänsten ska misslyckas om ett fel uppstår. Standardvärdet är Falskt.

#### Utdatadokument {#output-documents}

Beroende på indata-DDX kan invoke-API:t generera flera utdatadokument. På fliken Utdatadokument kan du välja var utdatadokumentet ska sparas.

1. *Spara utdata i nyttolast*: Sparar utdatadokument under nyttolastmappen, eller skriver över nyttolasten om nyttolasten är en fil.
1. *Utdatadokumentets karta*: Gör att du uttryckligen kan ange var varje utdatadokument ska sparas genom att lägga till en post per utdatadokument. Varje post anger dokumentet och var det ska sparas. Ett utdatadokument kan skriva över nyttolasten eller sparas under nyttolastmappen. Det är användbart när det finns flera utdatadokument.

1. *Jobblogg*: Anger var jobbloggdokumentet ska sparas, vilket är praktiskt vid felsökning.

### Konvertera till PDF/A-arbetsflöde {#convert-to-pdf-a-workflow}

Arbetsflödessteget Konvertera till PDF/A anropar API:t för tjänsten `toPDFA` Assembler. Det används för att konvertera PDF-dokument till dokument som överensstämmer med PDF/A.

1. Dra arbetsflödessteget **[!UICONTROL ConvertToPDFA]** under fliken Forms Workflow i Sidekick.

1. Dubbelklicka på det tillagda arbetsflödessteget för att redigera komponenten.
1. Konfigurera indatadokument, konverteringsalternativ och utdatadokument i dialogrutan Redigera komponent och klicka på **[!UICONTROL OK]**.

#### Indatadokument {#input-documents-1}

Ange källan till dokumentet som ska konverteras till ett PDF/A-kompatibelt dokument på något av följande sätt.

* *Relativt till nyttolast*: Indatadokumentet är relativt till arbetsflödesobjektets nyttolastmapp.
* *Använd nyttolast*: Nyttolasten för arbetsflödesobjektet används som indatadokument.
* *Absolut sökväg*: Den absoluta sökvägen för indatadokumentet i CRX-databasen.

#### Konverteringsalternativ {#conversion-options}

Med konverteringsalternativen kan du ange alternativ som ändrar processen för PDF/A-konvertering.

* *Efterlevnad* : Anger den PDF/A-standard som utdata från PDF/A måste uppfylla.
* *Resultatnivå* : Anger loggnivån som ska användas för PDF/A-konverteringsloggar.
* *Signaturer* : Anger hur signaturer i indatadokument måste bearbetas under konverteringen.
* *Färgrymd* : Anger den fördefinierade färgrymden som ska användas för utdata i PDF/A-dokument.
* *Verifiera* konvertering: Anger om det konverterade PDF/A-dokumentet ska verifieras för PDF/A-kompatibilitet efter konvertering.
* *Jobbloggnivå* : Anger loggnivån som ska användas för bearbetning av loggar.

* *Metadatatilläggsschema* : Anger sökvägen till metadatatilläggsschemat som ska användas för XMP-egenskaper i PDF-dokumentets metadata.

#### Utdatadokument {#output-documents-1}

På fliken Utdatadokument kan du ange mål för utdatadokumenten

* *PDFA-dokument*: Anger platsen där det konverterade PDF/A-dokumentet sparas. Det kan antingen skriva över nyttolastdokumentet eller sparas under nyttolastmappen.
* *Konverteringslogg*: Anger platsen där konverteringsloggarna sparas. Det kan antingen skriva över nyttolastdokumentet eller sparas under nyttolastmappen.

## Forms {#forms}

Arbetsflödet för att återge PDF-formulär är en wrapper runt `renderPDFForm` Forms tjänst-API för att skapa ett PDF-formulär med en XDP-mall och data-xml.

### Återge PDF Form Workflow {#render-pdf-form-workflow}

1. Dra arbetsflödessteget Återge PDF-formulär på fliken Forms Workflow i Sidekick.
1. Dubbelklicka på det tillagda arbetsflödessteget för att redigera komponenten.
1. Konfigurera indatadokument, utdatadokument och ytterligare parametrar i dialogrutan Redigera komponent och klicka sedan på **[!UICONTROL OK]**.

#### Indatadokument {#input-documents-2}

* *Mallfil*: Anger platsen för XDP-mallen. Det är ett obligatoriskt fält.

* *Datadokument*: Anger platsen för data-XML som behöver sammanfogas med mallen.

#### Utdatadokument {#output-documents-2}

* *Utdatadokument*: - Anger namnet på det genererade PDF-formuläret.

#### Ytterligare parametrar {#additional-parameters}

* *Innehållsrot*: Anger sökvägen till mappen i databasen där fragment eller bilder som används i XDP-indatamallen lagras.
* *Skicka-URL*: Anger standardsändnings-URL för genererat PDF-formulär.
* *Språk*: Anger standardspråk för det genererade PDF-formuläret.
* *Acrobat Version*: Anger målversionen för Acrobat för det genererade PDF-formuläret.
* *Taggad PDF*: Anger om den genererade PDF-filen ska vara tillgänglig.
* *XCI-dokument*: Anger sökvägen till XCI-filen.

## Utdata {#output}

Generate Non Interactive PDF Workflow är en wrapper runt `generatePDFOutput`-API:t för utdatatjänster. Den används för att generera icke-interaktiva PDF-dokument från XDP-mallar och data-xml.

### Skapa icke-interaktivt arbetsflöde för PDF Output   {#generate-non-interactive-pdf-output-workflow-nbsp}

1. Dra arbetsflödet Generera icke-interaktiva PDF Output under fliken Forms Workflow i Sidekick.
1. Dubbelklicka på det tillagda arbetsflödessteget för att redigera komponenten.
1. Konfigurera indatadokument, utdatadokument och ytterligare parametrar i dialogrutan Redigera komponent och klicka sedan på **[!UICONTROL OK]**.

#### Indatadokument {#input-documents-3}

* *Mallfil*: Anger platsen för XDP-mallen. Det är ett obligatoriskt fält.

* *Datadokument*: Anger platsen för data-XML som behöver sammanfogas med mallen.

#### Utdatadokument {#output-document}

*Utdatadokument*: Anger namnet på det genererade PDF-formuläret.

#### Ytterligare parametrar {#additional-parameters-1}

* *Innehållsrot*: Anger sökvägen till mappen i databasen där fragment eller bilder som används i XDP-indatamallen lagras.
* *Språk*: Anger standardspråk för genererat PDF-formulär.
* *Acrobat Version*: Anger målversionen för Acrobat för det genererade PDF-formuläret.
* Linjäriserad PDF: Anger om den genererade PDF ska optimeras för webbvisning.
* *Taggad PDF*: Anger om den genererade PDF-filen ska vara tillgänglig.
* *XCI-dokument*: Anger sökvägen till XCI-filen.
