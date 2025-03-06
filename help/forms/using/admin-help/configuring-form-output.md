---
title: Konfigurera formulärutdata
description: Lär dig konfigurera formulärutdata. Om du vill konfigurera formulärutdata och aktivera funktionen använder du de anpassade skripten innan formuläret skickas.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 2823d38e-f544-408e-9437-3d0fc622dc34
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Konfigurera formulärutdata{#configuring-form-output}

## Ange vilken typ av HTML-utdata som returneras till webbläsaren {#specify-the-type-of-html-output-returned-to-the-web-browser}

1. Klicka på Tjänster > Formulär i administrationskonsolen.
1. Välj något av följande alternativ i listan Utdatatyp under Formulärutdata:

   **Fullt HTML:** Återge formuläret med fullständiga HTML-taggar (en fullständig HTML-sida). Det här värdet är standardvärdet.

   **Formulärbrödtext:** Återge formuläret inom `<BODY>` -taggar (inte en fullständig HTML-sida).

1. Klicka på Spara.

## Ange var PDF-innehåll ska återges {#specify-the-location-where-pdf-content-is-rendered}

1. Välj något av följande alternativ i listan Återge vid under Formulärutdata:

   **Klient:** Återge PDF forms i Adobe Acrobat eller Adobe Reader. Klientsidesrendering förbättrar prestanda för AEM-formulär och gäller endast för PDFForm-omvandling.

   **Server:** Återge PDF forms på programservern.

   **Auto:** Återge PDF-formuläret på den plats som anges av XDP-filens `dynamicRender`-konfigurationsvärde. Det här värdet är standardvärdet.

1. Klicka på Spara.

## Konfigurera anrop av anpassade skript innan formuläret skickas {#configuring-invocation-of-custom-scripts-before-form-submit}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Gör så här för att aktivera funktionen:

1. Logga in på administrationskonsolen.
1. Gå till **Tjänster** > **formulär**.
1. Ange utdatatypen Form Body.
1. Spara inställningarna.
1. Deklarera en JavaScript-variabel, __CUSTOM_SCRIPTS_VERSION, i huvudsektionen i HTML-koden och ange dess värde till 1.

   >[!NOTE]
   >
   >*Om du vill inaktivera funktionen kan du ta bort variabeln JavaScript eller ange värdet 0.*
