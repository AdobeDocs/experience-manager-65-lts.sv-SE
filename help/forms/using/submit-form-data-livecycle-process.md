---
title: Konfigurera AEM Forms för att skicka data till en AEM Forms om JEE-process
description: Integrera adaptiva blanketter med AEM Forms i JEE-processer för bearbetning av blankettdata.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
docset: aem65
role: Admin, User, Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: c888da5d-6a98-4139-9656-a187177efcb0
hide: true
hidefromtoc: true
source-git-commit: 1336ccddcc73459f933e5e4b00a3a22605cdb9a1
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Konfigurera AEM Forms för att skicka formulärdata till ett AEM-formulär i en JEE-process{#configuring-aem-forms-to-submit-form-data-to-an-aem-forms-on-jee-process}

Adaptiva formulär har stöd för att skicka data till AEM Forms om JEE-processen för vidare bearbetning. Med den kan du starta en AEM Forms on JEE-process med data som är tillgängliga från det skickade formuläret. Utför följande steg så att du kan aktivera din AEM Forms-instans för att skicka ett anpassat formulär till AEM Forms i JEE-processen:

## Konfigurera AEM Forms Server {#configure-your-aem-forms-server}

Så här gör du för att AEM Forms Server ska kunna skicka data till en AEM Forms på en JEE-server:

1. Gå till AEM webbkonfigurationskonsol på https://[*host*]:[*port*]/system/console/configMgr.

1. Leta upp och klicka på **Adobe LiveCycle Client SDK Configuration** -komponenten.
1. Klicka för att redigera konfigurationsserverns URL, användarnamn och lösenord för AEM Forms på JEE-servern.
1. Granska inställningarna och klicka på **Spara**.

![Adobe LiveCycle Client SDK-konfiguration](assets/clientsdkconfiguration.jpg)

## Mappa data med processfält {#map-data-with-process-fields}

När du har konfigurerat AEM Forms mappar du XML-data och bilagor från det skickade formuläret till fälten i AEM Forms om JEE-processen. Gör följande:

1. Klicka på i AEM webbkonfigurationskonsol för att redigera konfigurationen **Guide LiveCycle Process Locator och Invoker** .
1. Ange följande parametrar:

   * **Namn på XML-parametern** (obligatoriskt): Ange XML-egenskapsfilen för den AEM Forms on JEE-process som måste bearbeta skickade data. Standardvärdet är **dataxml**.

   * **Namnet på parametern för bifogade filer** (valfritt): Ange listan med dokumentobjekt som AEM Forms on JEE-processen måste behandla. Standardvärdet är **fileAttachmentsList**.

1. Granska inställningarna och klicka på **Spara**.

![Guiden LiveCycle Process Locator och Invoker](assets/test3.jpg)

När åtgärden Skicka till Forms Workflow har konfigurerats listas de AEM Forms på JEE-serverprocesser som innehåller den angivna xml-parametern data.
