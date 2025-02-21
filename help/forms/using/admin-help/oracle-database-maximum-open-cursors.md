---
title: Tröskelvärde för högsta antal öppna markörer i Oracle-databasen
description: Lär dig hur du konfigurerar ett maximalt värde för öppna markörer i Oracle.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_aem_forms_database
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '82'
ht-degree: 0%

---

# Tröskelvärde för högsta antal öppna markörer i Oracle-databasen {#oracle-database-maximum-open-cursors-threshold}

Om du vill konfigurera ett maxvärde för öppna markörer i Oracle kan du behöva justera det här värdet till ett värde som passar ditt program. Det är uppenbart att under en måttlig belastning var de genomsnittliga öppna markörerna 2 700. Vi rekommenderar att du börjar med en övre gräns på 3 000. Mer information finns på [https://www.orafaq.com/node/758](https://www.orafaq.com/node/758).
