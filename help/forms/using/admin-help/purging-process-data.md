---
title: Rensningsprocessdata
description: Processdata som genereras när en långvarig process anropas kan bli för stora, vilket ger lägre prestanda för AEM-formulär och kräver onödigt diskutrymme. Se hur du kan tömma processdata.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_aem_forms_database
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 53ce63a3-704a-4da6-b652-362a436f05a7
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Rensningsprocessdata {#purging-process-data}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Processdata som genereras när en långvarig process anropas kan bli för stora, vilket ger lägre prestanda för AEM-formulär och kräver onödigt diskutrymme. Det är god praxis att rensa processdata när det inte längre behövs några poster. I AEM-formulär finns flera sätt att rensa processdata:

* Du kan använda administrationskonsolen för att utföra en engångsåtervinning av föråldrade poster som rör långvariga processer eller för att schemalägga regelbundna automatiska rensningar. (Se [Rensa poster från jobbhanterardatabasen](/help/forms/using/admin-help/purge-records-job-manager-database.md#purge-records-from-the-job-manager-database).)
* Du kan använda AEM formulär-API:t för Java och webbtjänster för att programmässigt rensa processdata som rör långvariga processer. (Se&quot;Rensa processdata&quot; i [Programmering med AEM-formulär](https://www.adobe.com/go/learn_aemforms_programming_63).)
* Använd processrensningsverktyget för att rensa processer baserat på processnamn och andra parametrar. Mer information finns i Viktigt-filen för processrensningsverktyget i *[aem_forms root]*\sdk\misc\Foundation\ProcessPurgeTool\ReadMe.txt.
