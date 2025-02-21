---
title: Köra AEM-formulär i underhållsläge
description: Underhållsläget är användbart när du utför uppgifter som att korrigera en DSC-fil, uppgradera AEM-formulär eller använda ett Service Pack-paket. Läs mer om hur du kör AEM-formulär i underhållsläge.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_aem_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Köra AEM-formulär i underhållsläge {#running-aem-forms-in-maintenance-mode}

Underhållsläget är användbart när du utför uppgifter som att korrigera en DSC-fil, uppgradera AEM-formulär eller använda ett Service Pack-paket.

Undvik att anropa processer när servern är i underhållsläge. Detta är vad som händer om en process anropas medan servern är i underhållsläge:

* Om processen är långvarig läggs den till i jobbdatabasen, men startas inte. När du avslutar underhållsläget bearbetar AEM de långvariga jobben i sin kö, även om servern startades om i underhållsläge.
* Om processen är kort, behandlas den direkt.

**Placera AEM-formulär i underhållsläge**

1. I en webbläsare anger du:

   `https://[hostname]:[port]/dsc/servlet/DSCStartupServlet?maintenanceMode=pause&user=[administrator username]&password=[password]`

   Ett&quot;nu pausat&quot;-meddelande visas i webbläsarfönstret.

   >[!NOTE]
   >
   >Om du stänger av servern medan den är i underhållsläge är den fortfarande i underhållsläge när den startas om. Stäng av underhållsläget när du är klar med underhållsåtgärderna.

**Kontrollera om AEM-formulär körs i underhållsläge**

1. I en webbläsare anger du:

   `https://[hostname]:[port]/dsc/servlet/DSCStartupServlet?maintenanceMode=isPaused&user=[administrator username]&password=[password]`

   Statusen visas i webbläsarfönstret. Statusen &quot;true&quot; anger att servern körs i underhållsläge och &quot;false&quot; anger att servern inte är i underhållsläge.

**Inaktivera underhållsläge**

1. I en webbläsare anger du:

   `https://[hostname]:[port]/dsc/servlet/DSCStartupServlet?maintenanceMode=resume&user=[administrator username]&password=[password]`

   Ett meddelande som&quot;nu körs&quot; visas i webbläsarfönstret.
