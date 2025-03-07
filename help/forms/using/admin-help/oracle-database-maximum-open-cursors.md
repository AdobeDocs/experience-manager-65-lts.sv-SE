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
hide: true
hidefromtoc: true
exl-id: 98663f16-6c05-4485-9bf2-a2de9d1975c8
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '82'
ht-degree: 0%

---

# Tröskelvärde för högsta antal öppna markörer i Oracle-databasen {#oracle-database-maximum-open-cursors-threshold}

Om du vill konfigurera ett maxvärde för öppna markörer i Oracle kan du behöva justera det här värdet till ett värde som passar ditt program. Det är uppenbart att under en måttlig belastning var de genomsnittliga öppna markörerna 2 700. Vi rekommenderar att du börjar med en övre gräns på 3 000. Mer information finns på [https://www.orafaq.com/node/758](https://www.orafaq.com/node/758).
