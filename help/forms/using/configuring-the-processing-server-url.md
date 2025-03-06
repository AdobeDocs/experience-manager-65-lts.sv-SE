---
title: Konfigurera AEM DS-inställningar
description: Lär dig hur du anger bearbetningsserverns URL innan du skickar ett formulär.
contentOwner: amgoyal
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
docset: aem65
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: 8ad3afd6-e1c6-4f21-bb0f-4d97ef50710e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Konfigurera AEM DS-inställningar{#configuring-aem-ds-settings}

I den här artikeln beskrivs hur du konfigurerar **AEM DS Settings Service**. Den här inställningen kan användas i flera scenarier, till exempel:

* I korrespondenshantering

   * För konfigurering av AEM Forms Workflow
   * När du använder Forms Portal för att spara utkast/inskickning på fjärrbasis

* I adaptiva formulär, till exempel när ett adaptivt formulär skickas från en publiceringsinstans

Så här konfigurerar du **[!UICONTROL AEM DS Settings]**:

1. Öppna Configuration Manager på publiceringsinstansen med URL:en:\
   *https://localhost:port/system/console/configMgr*.

   ![Konfiguration av AEM Web Console](assets/web_configuration_console_new.png)

1. Leta reda på och klicka på alternativet **[!UICONTROL AEM DS Settings]** i fönstret **[!UICONTROL Adobe Experience Manager Web Console Configuration]**.

   ![DS-inställningar](assets/ds_settings_new.png)

1. Fönstret **[!UICONTROL AEM DS Settings Service]** visar de vanliga konfigurationsinställningarna för AEM DS-komponenter.

   ![DS-inställningstjänst](assets/ds_settings_service_new.png)

1. Lägg till följande information i respektive fält:

   **[!UICONTROL Processing Server URL]**: Bearbetningsservern är den server där Forms- eller AEM-arbetsflödet måste aktiveras. Detta kan vara samma URL som till AEM-författarinstansen eller den andra server-URL:en (d.v.s. https://localhost:port/).

   **[!UICONTROL Processing Server User Name]**: Arbetsflödesanvändarens användarnamn [baserat på den server-URL som används]

   **[!UICONTROL Processing Server Password]**: Lösenord för arbetsflödesanvändare

   >[!NOTE]
   >
   >
   >    
   >    
   >    * När du använder arbetsflöden för Forms eller AEM måste du konfigurera tjänsten DS-inställningar innan du skickar något från publiceringsservern. I annat fall ska inlämningen av blanketten misslyckas.
   >    
   >
