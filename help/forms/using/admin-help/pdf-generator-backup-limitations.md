---
title: PDF Generator begränsningar för säkerhetskopiering
description: Läs mer om PDF Generator begränsningar för säkerhetskopiering. Den temporära katalog som PDF Generator använder kan inte säkerhetskopieras eftersom den raderar innehållet i angivna intervall.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
noindex: true
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '73'
ht-degree: 0%

---

# PDF Generator begränsningar för säkerhetskopiering {#pdf-generator-backup-limitations}

Den tillfälliga katalog som PDF Generator använder för att konvertera filer kan inte säkerhetskopieras. Även om tjänsten återställs på rätt sätt kan data gå förlorade eftersom PDF Generator granskar och raderar innehållet i den tillfälliga katalogen med angivna intervall.
