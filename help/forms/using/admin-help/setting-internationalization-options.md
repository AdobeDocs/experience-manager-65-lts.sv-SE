---
title: Ange alternativ för internationalisering
description: Lär dig hur du anger språkområdet som används för att återge formulär och hur du anger teckenuppsättningen som används för att koda utdataströmmen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 47a49147-2921-4d28-8d04-2281c0b9a190
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# Ange alternativ för internationalisering{#setting-internationalization-options}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

## Ange det språk som används för att återge formulär {#specify-the-locale-used-to-render-forms}

Du kan ange vilken språkinställning som ska användas vid återgivning av ett PDF-formulär. Fälten i ett PDF-formulär använder den angivna språkinställningen för att visa data. Om språkområdet till exempel är inställt på tyska, används decimalavgränsare för numeriska värden i formuläret. Språkinställningen används också för att skicka valideringsmeddelanden till klientenheter, som webbläsare, som en del av HTML-omvandlingar.

1. Klicka på Tjänster > Forms i administrationskonsolen.
1. Under Internationalisering i listan Språk väljer du det språk som används för att återge ett formulär. Standardvärdet är engelska (USA).
1. Klicka på Spara.

## Ange den teckenuppsättning som används för att koda utdataströmmen {#specify-the-character-set-used-to-encode-the-output-stream}

1. Välj en teckenuppsättning i listan Teckenuppsättning under Internationalisering. Den här inställningen beror på vilken API som används, antingen renderHTMLForm eller renderPDFForm. Om du vill ange en annan teckenuppsättning än de som visas väljer du Anpassad och anger ett kodningsvärde i rutan som visas.

   För HTML-omformningar har AEM-formulär stöd för teckenkodningsvärden som definieras av paketet `java.nio.charset`. Om sFormPreference är PDFForm stöds endast specifika teckenuppsättningar. Teckenuppsättningen måste vara ett giltigt kanoniskt namn. Standardvärdet är ISO-8859-1.

1. Klicka på Spara.
