---
title: Ställa in dagens meddelande
description: Med dagens meddelande kan du ange att ett meddelande ska visas på välkomstsidan i Workspace användargränssnitt.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_workspace
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Ställa in dagens meddelande {#setting-the-message-of-the-day}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan ange att ett meddelande ska visas på välkomstsidan i Workspace användargränssnitt.

Om det behövs kan du använda de HTML-taggar som stöds av Adobe Flash® Player för att formatera textens utseende:

* &lt;a> Ankartagg
* &lt;b> Fet tagg
* &lt;br> Bryttagg
* Teckensnittstaggen &lt;font>
* Bild-taggen &lt;img>
* Taggen &lt;i> Kursiv
* &lt;li>-tagg för listobjekt
* Taggen &lt;p> Stycke
* &lt;span> Span-tagg
* Tagg för textformat
* &lt;u> Understrykningstagg

Mer information om de taggar som stöds finns i definitionen av egenskapen `htmlText` för klassen TextField i [Flex språkreferens](https://flex.apache.org/).

## Ange dagens meddelande {#set-the-message-of-the-day}

1. I administrationskonsolen klickar du på Tjänster > Workspace > Dagens meddelande.
1. Ange texten som ska visas på välkomstskärmen i rutan Dagens meddelande.
1. Klicka på Spara.

>[!NOTE]
>
>Flex Workspace används inte i AEM formulärrelease.
