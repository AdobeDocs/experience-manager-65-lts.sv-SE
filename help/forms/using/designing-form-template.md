---
title: Utforma formulärmallar för HTML5-formulär
description: AEM Forms kan återge XFA-formulärmallen till HTML5-format. Formulärdesigners kan utforma blankettmallar med Designer och använda HTML5-renderingsfunktionen.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: hTML5_forms
docset: aem65
feature: HTML5 Forms,Mobile Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---

# Utforma formulärmallar för HTML5-formulär{#designing-form-templates-for-html-forms}

HTML5-formulärkomponenten i AEM kan återge XFA-formulärmallen till HTML5-format. Formulärdesigners kan utforma formulärmallar med [Forms Designer](https://www.adobe.com/go/learn_aemforms_designer_63) och använda återgivningsfunktionen HTML5. Dessa formulärmallar, tillsammans med deras resurser, kan finnas i AEM-databasen, filsystemet eller visas via http. Men om du tänker hantera dina formulär med Forms Manager bör mallarna och resurserna finnas i AEM-databasen.

Även om HTML5-formulär i stor utsträckning matchar PDF forms beteende finns det vissa funktioner i båda formaten som inte kan användas i det andra formatet. Hur streckkoder tillämpas på ett PDF-formulär i Adobe Reader varierar till exempel från ett mobilformulär eller hur ett formulär signeras digitalt varierar också mellan formaten. Mer information om sådana variationer finns i [Funktionsdifferentiering mellan HTML5-formulär och PDF forms](../../forms/using/feature-differentiation-html5-forms-pdf-forms.md).

Vanliga XFA-funktioner finns i följande metodtips och riktlinjer för att utforma ett formulär som fungerar i båda formaten.

## God praxis {#best-practices}

De flesta stegen för att utforma en formulärmall, som schemabindningar eller skrivning av formulärlogik, är desamma. På grund av skillnader mellan återgivnings- och skriptmotorn i en tjock klient som Adobe Reader och webbläsarbaserade formulär finns det dock vissa rekommendationer som beskrivs i artikeln [best practices](/help/forms/using/design-accessible-html5-forms.md) . De bästa sätten att utforma formulärmallar så att de fungerar som förväntat i båda formaten.

### Funktioner i AEM Forms Designer för HTML5 Forms {#capabilities-in-aem-forms-designer-for-html-forms}

#### Förhandsgranska HTML {#preview-html}

Fliken Förhandsgranska HTML har lagts till i designläget så att formulärdesigners kan förhandsgranska formulär i HTML5-format under designprocessen. Mer information om hur du aktiverar och konfigurerar den här funktionen i AEM Forms Designer finns i [Förhandsgranska HTML](../../forms/using/preview-xdp-forms-html.md).

#### Klottra signaturer {#scribble-signature}

Huvudmålet för HTML5-formulär är pekenheter. Därför har en ny kontroll för klottra signaturer lagts till i AEM Forms Designer. Du kan klicka på eller dra och släppa kontrollen för signering av skript på formulärmallen och konfigurera den. Det återges som ett klottrigt fält i HTML5-renderingen och kan användas för att klottra signaturer på enheter med pekskärm. På stationära datorer kan den användas som ett klotterfält med hjälp av muskontroll. Mer information om hur du använder den här funktionen finns i [XFA-skriptfält](../../forms/using/scribble-signature.md).

![4](assets/4.png)

#### RTF-format {#rich-text-format}

Du kan konvertera ett textfält till ett RTF-fält. En lista med formateringsalternativ läggs till i textfältet. Om du vill konvertera öppnar du Forms Designer och markerar textfältet i **[!UICONTROL Design View]**. Välj **[!UICONTROL Rich Text]** i listrutan **[!UICONTROL Field Format]** på fliken **[!UICONTROL Field]**. När XFA-formuläret återges som ett HTML5-formulär återges nu fältet som ett RTF-fält. Välj ![Maximera](assets/maximize_icon.svg) om du vill visa fler formateringsalternativ.
