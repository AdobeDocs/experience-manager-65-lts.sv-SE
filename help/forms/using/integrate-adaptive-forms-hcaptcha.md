---
title: Hur använder man hCaptcha&reg; i en AEM 6.5 Forms?
description: Förbättra formulärsäkerheten med Captcha&reg; service utan problem. Stegvisa anvisningar inifrån!
feature: Adaptive Forms, Foundation Components
role: User, Developer
exl-id: da0f8fc5-732e-41de-b73c-0355ec723d26
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 0%

---

# Koppla samman din AEM Forms-miljö med Captcha® {#connect-your-forms-environment-with-hcaptcha-service}


<span class="preview">Den här funktionen är inte aktiverad som standard. Du kan skriva från din officiella adress till aem-forms-ea@adobe.com för att begära åtkomst till funktionen.</span>

CAPTCHA (Completely Automated Public Turing test to tell Computers and Humans Apart) är ett program som ofta används vid onlinetransaktioner för att skilja mellan människor och automatiserade program eller organ. Det utgör en utmaning och utvärderar användarens svar för att avgöra om det är en människa eller en robot som interagerar med webbplatsen. Det förhindrar användaren att fortsätta om testet misslyckas och gör onlinetransaktionerna säkra genom att förhindra att skräppost eller skadliga syften publiceras.

Förutom hCaptcha® stöder AEM Forms 6.5 följande CAPTCHA-lösningar:

* [Google reCAPTCHA](/help/forms/using/captcha-adaptive-forms.md)
* [Molnformad vändning](/help/forms/using/integrate-adaptive-forms-turnstile.md)

## Integrera AEM Forms-miljö med Captcha®

Med tjänsten Captcha® kan du skydda dina formulär från stötar, skräppost och automatiskt missbruk. Det utgör en utmaning för kryssrutewidgeten och utvärderar användarens svar för att avgöra om det är en människa eller en robot som interagerar med formuläret. Det förhindrar användaren att fortsätta om testet misslyckas och gör onlinetransaktionerna säkra genom att förhindra att skräppost eller skadliga aktiviteter publiceras.

AEM 6.5 Adaptive Forms support Captcha&amp;reg. Du kan använda den för att visa en kryssrutewidget när formulär skickas.

<!-- ![hCaptcha&reg;](assets/hCaptcha&reg;-challenge.png)-->


### Förutsättningar för att integrera AEM Forms-miljön med Captcha® {#prerequisite}

Om du vill konfigurera hCaptcha® med AEM Forms måste du hämta webbplatsnyckeln [hCaptcha® och den hemliga nyckeln](https://docs.hcaptcha.com/switch/#get-your-hcaptcha-sitekey-and-secret-key) från Captcha®-webbplatsen.

### Konfigurera hCaptcha® {#steps-to-configure-hcaptcha}

Så här integrerar du AEM Forms med tjänsten Captcha®:

1. Skapa en konfigurationsbehållare i din AEM Forms-miljö, som innehåller molnkonfigurationer som används för att ansluta AEM till externa tjänster. Så här skapar du en konfigurationsbehållare:
   1. Öppna AEM Forms-miljön.
   1. Gå till **[!UICONTROL Tools > General > Configuration Browser]**.
   1. I Configuration Browser kan du välja en befintlig mapp eller skapa en ny:
      * Så här skapar du en ny mapp och aktiverar molnkonfigurationer:
         1. Klicka på **[!UICONTROL Create]** i konfigurationsläsaren.
         1. Ange ett namn, en titel och markera **[!UICONTROL Cloud Configurations]** i dialogrutan Skapa konfiguration.
         1. Klicka på **[!UICONTROL Create]**.
      * Så här aktiverar du molnkonfiguration för en befintlig mapp:
         1. Markera mappen i Configuration Browser och välj **[!UICONTROL Properties]**.
         1. Aktivera **[!UICONTROL Cloud Configurations]** i dialogrutan Konfiguration.
         1. Klicka på **[!UICONTROL Save & Close]** om du vill spara konfigurationen och stänga dialogrutan.

1. Konfigurera dina molntjänster:
   1. Gå till ![tools-1](assets/tools-1.png) > **[!UICONTROL Cloud Services]** och klicka på **[!UICONTROL hCaptcha®]** i AEM-författarinstansen.
      ![hCaptcha® i ui](assets/hcaptcha-in-ui.png)
   1. Välj en konfigurationsbehållare, skapad eller uppdaterad, enligt beskrivningen i föregående avsnitt. Välj **[!UICONTROL Create]**.
      ![Configuration Captcha®](assets/config-hcaptcha.png)
   1. Ange **[!UICONTROL Title]**, <!--**[!UICONTROL Name]**--> **[!UICONTROL Site Key]** och **[!UICONTROL Secret Key]** för hCaptcha®-tjänsten [ som hämtats i Förutsättning](#prerequisite).
   1. Klicka på **[!UICONTROL Create]**.

      ![Konfigurera Cloud Service för att ansluta din AEM Forms-miljö till Captcha®](assets/create-hcaptcha-config.png)

   >[!NOTE]
   > Användarna behöver inte ändra [Verifierings-URL:en för klientsidan ](https://docs.hcaptcha.com/#add-the-hcaptcha-widget-to-your-webpage) och [Verifierings-URL:en för serversidan](https://docs.hcaptcha.com/#verify-the-user-response-server-side) eftersom de redan är förfyllda för hCaptcha®-validering.

   När hCAPTCHA-tjänsten har konfigurerats kan den användas i ditt adaptiva formulär.

## Använd Captcha® i en anpassad form {#using-hCaptcha-in-aem-6.5}

1. Öppna AEM Forms-miljön.
1. Gå till **[!UICONTROL Forms]** > **[!UICONTROL Forms and Documents]**.
1. Välj ett anpassat formulär och klicka på **[!UICONTROL Properties]**.
1. I **[!UICONTROL Configuration Container]** väljer du den konfigurationsbehållare som innehåller den molnkonfiguration som ansluter AEM Forms till Captcha.
1. Klicka på **[!UICONTROL Save & Close]**.

   Om du inte har någon konfigurationsbehållare för hCaptcha läser du avsnittet [Anslut AEM Forms-miljön med hCaptcha®](#configure-hcaptcha-steps-to-configure-hcaptcha) för att lära dig hur du skapar en konfigurationsbehållare.

   ![Välj konfigurationsbehållare](/help/forms/using/assets/captcha-properties.png)

1. Markera ett anpassat formulär och klicka på **[!UICONTROL Edit]** för att öppna formuläret i redigeraren.
1. Dra komponenten **[!UICONTROL Captcha]** från komponentwebbläsaren till det adaptiva formuläret.
1. Markera komponenten **[!UICONTROL Captcha]** och klicka på egenskapsikonen ![Egenskaper](assets/configure-icon.svg) för att öppna egenskapsdialogrutan. Ange följande egenskaper:

   ![hCaptcha® v1](assets/config-hcaptcha-v1-img.png)

   * **[!UICONTROL Title]:** Ange titeln för Captcha-komponenten.
   * **[!UICONTROL Validation Message]:** Ange ett valideringsmeddelande för din Captcha-validering när formulär skickas eller när en användaråtgärd utförs.
   * **[!UICONTROL Captcha Service]:** Välj CAPTCHA-tjänsten för att skicka formulär, här väljer du hCaptcha®.
   * **[!UICONTROL Configuration Settings]:** Välj din molnkonfiguration som är konfigurerad för hCaptcha®.
     >[!NOTE]
     >Du kan ha flera molnkonfigurationer i din miljö i liknande syfte. Välj tjänsten noggrant. Om ingen tjänst finns med i listan kan du läsa [Koppla din AEM Forms-miljö till hCaptcha®](#connect-your-forms-environment-with-hcaptcha-service) för att lära dig hur du skapar en Cloud Service som ansluter din AEM Forms-miljö till hCaptcha®-tjänsten.

   * **[!UICONTROL Error Message]:** Ange felmeddelandet som ska visas för användaren när Captcha-överföringen misslyckas.
   * **[!UICONTROL Captcha Size]:** Du kan välja visningsstorlek för utmaningsdialogrutan hCaptcha®. Använd alternativet **[!UICONTROL Compact]** för att visa en liten storlek och alternativet **[!UICONTROL Normal]** för att visa en relativt stor hCaptcha®-utmaningsdialogruta eller **[!UICONTROL Invisible]** för att validera hCaptcha® utan att explicit återge kryssrutewidgeten i användargränssnittet.

1. Välj **[!UICONTROL Done]**.


Nu är det bara berättigade formulär, där formuläranvändaren kan ta bort utmaningen från tjänsten hCaptcha®, som kan användas för att skicka in formuläret.

**hCaptcha® är ett registrerat varumärke som tillhör Intuition Machines, Inc.**


## Vanliga frågor

* **F: Kan jag använda mer än en Captcha-komponent i ett adaptivt formulär?**
* **Ans:** Det går inte att använda fler än en Captcha-komponent i ett adaptivt formulär. Du bör inte heller använda en Captcha-komponent i ett fragment eller en panel som är markerad för lazy loading.

## Se även {#see-also}

* [Använda CAPTCHA i anpassningsbara formulär](/help/forms/using/captcha-adaptive-forms.md)
* [Använda Turnstile Captcha i adaptiva former](/help/forms/using/integrate-adaptive-forms-turnstile.md)
