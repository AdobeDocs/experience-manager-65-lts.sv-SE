---
title: Aktivera och inaktivera säkert säkerhetskopieringsläge
description: På sidan Säkerhetskopieringsinställningar kan du använda AEM-formulär i säkert säkerhetskopieringsläge så att du kan säkerhetskopiera din databas och GDS-katalogen (Global Document Storage). Lär dig hur du aktiverar och inaktiverar läget för säker säkerhetskopiering.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 34381caa-154e-479c-b475-7b3549909e9a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Aktivera och inaktivera säkert säkerhetskopieringsläge {#enabling-and-disabling-safe-backup-mode}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

På sidan Säkerhetskopieringsinställningar kan du använda AEM-formulär i säkert säkerhetskopieringsläge så att du kan säkerhetskopiera din databas och GDS-katalogen (Global Document Storage).

AEM-formulär är i säkert säkerhetskopieringsläge, men fungerar normalt, förutom att de inte aktivt tar bort filer från GDS-katalogen.

>[!NOTE]
>
>Om du anger det här alternativet säkerhetskopieras inte systemet. Systemet förbereds för säkerhetskopiering.

## Aktivera säkert säkerhetskopieringsläge {#enable-safe-backup-mode}

1. I administrationskonsolen klickar du på Inställningar > Core Systems Settings > Backup Settings.
1. På sidan Inställningar för säkerhetskopiering väljer du Använd i säkert säkerhetskopieringsläge och klickar på OK.

>[!NOTE]
>
>Om systemet redan körs i säkert säkerhetskopieringsläge skapas ingen ny reservation när du klickar på OK.

## Inaktivera säkert säkerhetskopieringsläge {#disable-safe-backup-mode}

1. I administrationskonsolen klickar du på Inställningar > Core Systems Settings > Backup Settings.
1. Avmarkera Använd i säkert säkerhetskopieringsläge på sidan Inställningar för säkerhetskopiering och klicka på OK.
