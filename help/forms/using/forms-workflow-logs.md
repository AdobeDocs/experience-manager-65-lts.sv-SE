---
title: Logga in i AEM Forms arbetsflöden
description: Lär dig hur du felsöker problem i AEM Forms Workflow och aktiverar felsökningsloggning för AEM Forms-arbetsflöden för att visa loggarna.
contentOwner: anujkapo
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 90a44cab-3ecf-4a71-95d4-e8ce2d996980
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 5%

---

# Logga in i AEM Forms arbetsflöden{#logging-in-aem-forms-workflows}

I Forms Workflow steg finns detaljerade loggar för att enkelt felsöka arbetsflödesrelaterade problem. Aktivera felsökningsloggning för AEM Forms-arbetsflöden för att visa loggarna.

Som standard är all loggningsinformation tillgänglig i filen **error.log** i katalogen */crx-database/logs/* .

Felsökningsloggarna för formulärarbetsflöden innehåller:

* Inmatning av varje arbetsflödessteg. Till exempel:\
  `[DEBUG] "Executing Invoke DDX Process step"`

* Avsluta varje arbetsflödessteg. Till exempel:\
  `[DEBUG] "Successfully finished Invoke DDX Process step"`

* Meddelanden om serviceanrop. Till exempel:\
  `[DEBUG] Invoking Adobe Sign Service for creating agreement`

* Meddelanden om tjänstavslutning. Till exempel:\
  `[DEBUG] Agreement created successfully with agreement id <agreement id>`

* Variabler som läses från metadatamappningen. Till exempel:\
  `[DEBUG] Successfully retrieved variable <variable name> from workflow meta data map`

* Variabler skrivna i JCR-databasen. Till exempel:

  ```verilog
     [DEBUG] Successfully written variable <variable name> into meta data node at <JCR path where meta data is being written>
  ```

* Undantag med fullständig stackspårning. Till exempel:\
  `[DEBUG] Exception in Adobe Sign Service <complete stack trace>`

* Parametrar för metadata för dynamiskt steg. Till exempel:

  ```verilog
  [DEBUG] Document of Record to be generated for adaptive form <path of adaptive form>
   [DEBUG] Locale to be used for Document of Record is <locale>
  ```

I följande exempel visas loggarna för steget Signera dokument:

```verilog
[DEBUG] Executing sign document step.
[DEBUG] Using adobe sign configuration: <path of adobe sign configuration>
[DEBUG] Invoking Adobe Sign Service for creating agreement
[DEBUG] Agreement created successfully with agreement id <agreement id>
[DEBUG] Exception in Adobe Sign Service <complete stack trace>
[ERROR] Exception in Adobe Sign Service
[DEBUG] Successfully finished sign document step
```

Använd loggarna för att utvärdera att:

* Du använder en korrekt Adobe Sign-konfiguration.
* Adobe Sign-tjänsten avslutas när ett avtal har skapats.
* Signera dokument-steget avslutas med ett meddelande om att åtgärden lyckades.

Om det finns ett undantag kan du visa hela stackspårningen för att utvärdera orsaken till felet.

## Aktivera felsökningsloggning för AEM Forms-arbetsflöden {#enable-debug-logging-for-aem-forms-workflows}

Gör så här för att aktivera felsökningsloggning för AEM Forms-arbetsflöden:

1. Gå till konfigurationshanteraren för AEM webbkonsol på:

   https://&#39;[server]:[port]/system/console/configMgr

1. Välj **[!UICONTROL Sling]** > **[!UICONTROL Log Support]**.
1. Välj **[!UICONTROL Add new Logger.]**
1. Välj **[!UICONTROL Debug]** som **[!UICONTROL Log Level]**.
1. Ange platsen för loggfilen. Standardplatsen för loggfilen är: *logs\error.log*
1. Ange paketets namn som **com.adobe.granite.workflow.core** i kolumnen **[!UICONTROL Logger]**.

   Om du utför dessa steg kan du lagra felsökningsloggarna för paketet **com.adobe.granite.workflow.core**. Välj **[!UICONTROL +]** och lägg till följande paketnamn i listan:

   * com.adobe.fd.workflow
   * com.adobe.fd.workspace
