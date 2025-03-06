---
title: Konfigurera Microsoft Dynamics 365 för inteckningsarbetsflödet i Web.Finance-referenswebbplatsen
description: Lär dig hur du använder Microsoft&reg; Dynamics 365-tjänster med hjälp av adaptiva formulär för heminteckningsarbetsflödet på webbsidan för referens för Web.Finance.
products: SG_EXPERIENCEMANAGER/6.3/FORMS
topic-tags: develop, Configuration
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Foundation Components
role: Admin, User, Developer
exl-id: 1021fbb4-a12a-4758-8f36-dc9ad73681cd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Konfigurera Microsoft Dynamics 365 för inteckningsarbetsflödet i Web.Finance-referenswebbplatsen {#configure-microsoft-dynamics-for-the-home-mortgage-workflow-of-the-we-finance-reference-site}

Lär dig hur du använder Microsoft® Dynamics 365-tjänsterna med hjälp av adaptiva formulär för heminteckningsarbetsflödet på webbplatsen Web.Finance Reference

## Ökning {#overview}

Microsoft® Dynamics 365 är en CRM- och ERP-programvara (Customer Relationship Management) som innehåller företagslösningar för att skapa och hantera kundkonton, kontakter, leads, möjligheter och ärenden.

AEM Forms tillhandahåller en molntjänst för integrering av Dynamics 365 med modulen [Forms-dataintegrering](/help/forms/using/data-integration.md). Innan du kan använda genomgången av Home Mortgage-programmet med Microsoft® Dynamics-scenariot måste du konfigurera Microsoft® Dynamics 365 så att det används med referenswebbplatsen We.Finance.

## Förutsättningar {#prerequisites}

Innan du börjar konfigurera och konfigurera Dynamics 365 måste du kontrollera att du har:

* AEM 6.3 Forms Service Pack 1 och senare
* Microsoft® Dynamics 365-konto
* Registrerat program för tjänsten Dynamics 365 med Microsoft® Azure Active Directory
* Klient-ID och klienthemlighet för det registrerade programmet

## Länka inteckningsberäkningen till webbplatsens startsida {#link-the-home-mortgage-calculator-with-your-site-home-page}

1. Gå till följande sida på författarinstansen:

   `https://[server]:[port]/editor.html/content/we-finance/global/en/loan-landing-page.html`

1. Bläddra nedåt till Hemmedarbetarkalkylatorn.
1. Markera den högra kolumnens panel (räknare) och välj för att visa snabbmenyn. Välj Konfigurera på snabbmenyn. Dialogrutan Redigera AEM Forms-behållare visas.

   ![calculconfigurepanel](assets/calculatorconfigurepanel.png)

1. I dialogrutan Redigera AEM Forms-behållare bläddrar du till resurssökvägen och väljer home-hypotekskalkylator på följande sökväg och väljer **Bekräfta**:

   formsanddocuments/We.Finance/MS Dynamics

   ![selectassetpath](assets/selectassetpath.png)

1. Välj **Klar**.
1. Publicera den redigerade sidan.

   >[!NOTE]
   >
   >Bindningen av beräkningsfälten med FDM är förkonfigurerad via referenspaketet för We.Finance. Om du vill visa bindningen kan du öppna formuläret i redigeringsläget och se de binda referenserna för fältet.

1. Om du vill skapa en anpassad enhet för lagring av ansökningspost för bostadslåneprogram importerar du AEMFormsFSIRefsite_1_0.zip-lösningspaketet till din Microsoft® Dynamics-instans:

   1. Hämta paketet från:

      `https://'[server]:[port]'/content/aemforms-refsite-collaterals/we-finance/home-mortgage/ms-dynamics/AEMFormsFSIRefsite_1_0.zip`

   1. Importera lösningspaketet till instansen av Microsoft® Dynamics. Gå till **Inställningar** > **Lösningar** i din Microsoft® Dynamics-instans och välj sedan **Importera**.

1. Om du vill ställa in kontaktinformation för användare som används i refsite importerar du Sarah Rose Contact.CSV-paketet till din Microsoft® Dynamics-instans:

   1. Hämta paketet från:

      `https://'[server]:[port]'/content/aemforms-refsite-collaterals/we-finance/home-mortgage/ms-dynamics/Sarah%20Rose%20Contact.csv`

   1. Importera paketet till din Microsoft® Dynamics-instans. Gå till **Försäljning** > **Kontakter** i din Microsoft® Dynamics-instans och välj sedan **Importera data**.
