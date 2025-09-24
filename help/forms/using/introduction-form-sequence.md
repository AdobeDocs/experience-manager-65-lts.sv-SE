---
title: Introduktion till formulärsekvenser i flera steg
description: Med AEM Forms kan du definiera en sekvens med formulärpaneler där du vill att användarna ska kunna navigera och fylla i ett anpassat formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 5455facf-ed09-4266-a43a-61eef3ecc33e
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---

# Introduktion till formulärsekvenser i flera steg{#introduction-to-multi-step-form-sequence}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs ett äldre sätt att skapa adaptiva Forms med baskomponenter. </span>

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/configure-layout-of-an-adaptive-form/introduction-form-sequence.html).


Med adaptiva blanketter kan man enkelt skapa datainhämtning i flera steg. Den har inbyggt stöd för att skapa flera paneler och koppla ihop varje panel med olika navigeringsmönster. Formulärförfattare kan gruppera formulärfält i logiska avsnitt och representera en grupp som en panel. Den övergripande navigeringen mellan paneler styrs med hjälp av panellayouten. Författare kan välja att ordna paneler i olika layouter, t.ex. placera sekventiellt med guidelayouten eller på ett ad hoc-sätt med hjälp av fliklayouten. Mer information om panellayouter finns i [Layoutfunktioner i adaptiva formulär](../../forms/using/layout-capabilities-adaptive-forms.md).

I en typisk miljö för ifyllnad av formulär finns det fler steg att utföra än att bara hämta in data. En fullständig inlämning av formulär kan omfatta andra steg, t.ex. signera formuläret digitalt, verifiera den information som fylls i formuläret och betala för behandlingen. Det skiljer sig från fall till fall.

Om ditt användningsfall föreskriver en uppsättning steg för datainhämtning, eller om det finns bestämmelser som kräver att vissa steg ska följas, kan AEM Forms tillhandahålla ett sätt att tillämpa den gemensamma strukturen i alla formulär. Den färdiga implementeringen av en formulärstruktur definierar stegsekvensen för ett formulär. ![Exempel på en formulärsekvens i flera steg](assets/formpipeline.png)

Exempel på en formulärsekvens i flera steg

Låt oss ta ett exempel där du behöver skapa en sekvens för att fylla i, verifiera, signera och bekräfta ett formulär. Så här skapar du en sådan sekvens:

1. Definiera en formulärmall och lägg till den panel som behövs i den. Det ska finnas en panel för varje steg i sekvensen. Du kan dock inkludera underpaneler inuti en panel.

   I det här exemplet kan följande paneler läggas till:

   * **Fyllning**: Den innehåller formulärfält för datainhämtning. Här kan du ta med kapslade underpaneler för att skapa avsnitt för olika typer av information, till exempel personlig information, familjeinformation och ekonomisk information.

   * **Verifiera**: Den innehåller komponenten **Verifiera** som kan användas i ett XFA-baserat adaptivt formulär. Den information som hämtas på panelen Fyllning visas i skrivskyddat läge för verifiering.

   * **E-signatur**: Den innehåller komponenten **Sign** som kan användas i ett XFA-baserat adaptivt formulär. den tillhandahåller följande signeringstjänster:

      * Adobe Document Cloud eSign-tjänster
      * Klottra signaturer

   * **Bekräftelse**: Den innehåller komponenten **Sammanfattning** som visar ett meddelande som bekräftar att formuläret har skickats när en användare har signerat och når steget Bekräfta (Sammanfattning) i sekvensen. Författare kan konfigurera texten i sammanfattningskomponenten, visa ett tackmeddelande, visa en länk till den genererade PDF och så vidare.

1. Välj layout för rotpanelen som **[!UICONTROL Wizard]**.
1. Slutför de återstående stegen så att du kan skapa formulärmallen. Se [Skapa en anpassad anpassad formulärmall](../../forms/using/custom-adaptive-forms-templates.md).

När du har definierat formulärsekvensen i formulärmallen kan du använda den för att skapa formulär som har den grundläggande strukturen definierad som sekvensen på plats. Du kan alltid anpassa formuläret efter dina behov.
