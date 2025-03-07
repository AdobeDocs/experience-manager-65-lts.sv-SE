---
title: Att tänka på när du kör administrationskonsolen
description: I det här dokumentet visas några punkter som du bör tänka på när du kör administrationskonsolen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_application_server
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: bdd884c4-ae12-4827-8251-01033cbc0185
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# Att tänka på när du kör administrationskonsolen {#considerations-when-running-administrationconsole}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Detta är några saker att tänka på när du kör administrationskonsolen:

* Om du använder administrationskonsolen med URL:en `https://[hostname]:'port'/adminui` får det angivna värdnamnet inte innehålla understreck. I annat fall kanske länkar till vissa delar av administrationskonsolen inte fungerar som de ska.
* Om du kör en administrationskonsol i Utforskaren i Windows på ett japanskt operativsystem kan du få följande problem:

   * När du klickar på en länk återgår du till inloggningssidan i stället för till den förväntade länken.
   * Om du klickar på en länk visas ett behörighetsfel.

  Det bästa sättet är att köra administrationskonsolen från en annan webbläsare, till exempel Mozilla Firefox, för att se till att inga länkar fungerar.

* Använd inte omvända snedstreck () när du gör sökningar i administrationskonsolen.
