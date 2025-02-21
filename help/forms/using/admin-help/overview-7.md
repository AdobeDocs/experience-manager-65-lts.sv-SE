---
title: Grunderna för att konfigurera formulär
description: Lär dig mer om de olika formulärtjänsterna som hjälper dig att skapa interaktiva program för datainhämtning.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---

# Grunderna för att konfigurera formulär {#basics-of-configuring-forms}

Med tjänsten Forms kan du skapa interaktiva klientapplikationer för datainhämtning som validerar, bearbetar, omvandlar och levererar blanketter som vanligtvis skapas i Designer. Formulärförfattare utvecklar en enda formulärdesign som Forms-tjänsten återger i olika format:

* som PDF i Adobe Reader eller i en webbläsare
* som HTML i olika webbläsarmiljöer, inklusive kompatibel XHTML 1.0-återgivning
* som formulärguider i olika webbläsarmiljöer med stöd för Adobe Flash Player.

Mer information om Forms-tjänsten finns i [Tjänstreferens](https://www.adobe.com/go/learn_aemforms_services_63).

Med hjälp av Forms-sidan i administrationskonsolen kan du konfigurera beteendet för Forms-tjänsten. De här inställningarna gäller för alla anrop av tjänsten. Alla parametrar som skickas via AEM-formulär SDK åsidosätter inställningarna som angetts i administrationskonsolen, men de påverkar bara det specifika anropet.

När du har ändrat Forms-inställningarna i administrationskonsolen klickar du på Spara. Du behöver inte starta om servern för att ändringarna ska börja gälla. Du kan dock behöva stoppa och starta om tjänsten Forms när du konfigurerar inställningar för cacheläge. (Se [Starta och stoppa tjänster](/help/forms/using/admin-help/starting-stopping-services.md#starting-and-stopping-services).)
