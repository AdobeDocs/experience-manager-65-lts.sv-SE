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
hide: true
hidefromtoc: true
exl-id: f76ce3be-6d50-4531-a982-2e902f866208
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '73'
ht-degree: 0%

---

# PDF Generator begränsningar för säkerhetskopiering {#pdf-generator-backup-limitations}

Den tillfälliga katalog som PDF Generator använder för att konvertera filer kan inte säkerhetskopieras. Även om tjänsten återställs på rätt sätt kan data gå förlorade eftersom PDF Generator granskar och raderar innehållet i den tillfälliga katalogen med angivna intervall.
