---
title: Konfigurera synkroniseringsplaneraren
description: Lär dig hur du migrerar och synkroniserar resurser, konfigurerar schemaläggaren för synkronisering och använder mappar för att ordna resurser.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
docset: aem65
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Workbench,Adaptive Forms
exl-id: b41e5e15-eb7f-4404-82a0-2ba034694577
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 0%

---

# Konfigurera synkroniseringsplaneraren {#configuring-the-synchronization-scheduler}

Som standard körs synkroniseringsschemaläggaren efter var tredje minut för att synkronisera alla resurser som ändrats och uppdaterats i databasen via LiveCycle Workbench 11. Program som innehåller formulär och resurser visas i AEM Forms användargränssnitt när synkroniseringsprocessen är klar.

## Ändra intervall för synkroniseringsschemaläggaren {#change-interval-of-the-synchronization-scheduler}

Utför följande steg för att ändra intervallet för synkroniseringsplaneraren:

1. Logga in på AEM Configuration Manager. URL för Configuration Manager är `https://'[server]:[port]'/lc/system/console/configMgr`

1. Leta upp och öppna paketet **FormsManagerConfiguration**.

1. Ange ett nytt värde för alternativet **Schemaläggarens frekvens för synkronisering**.

   Frekvensenheten är minuter. Om du till exempel vill konfigurera schemaläggaren att köras efter var 60:e minut anger du 60.

## Synkronisera resurser {#synchronizing-assets}

Du kan använda alternativet **Synkronisera Assets från databas** om du vill synkronisera resurserna manuellt. Så här synkroniserar du resurserna manuellt:

1. Logga in på AEM Forms. Standardwebbadressen är `https://'[server]:[port]'/lc/aem/forms/`.

   ![AEM Forms användargränssnitt](assets/aem_forms_ui.png)

   **Figur:** *AEM Forms användargränssnitt*

1. Klicka på ikonen ![aem6forms_sync](assets/aem6forms_sync.png) i verktygsfältet. Om du inte har några resurser på den senast konfigurerade sökvägen visas dialogrutan enligt nedan. Klicka på **Start** för att starta synkroniseringen.

   ![Dialogrutan Synkronisering](assets/migrate-and-syncronize.png)

   **Figur:** *Dialogrutan Synkronisering*

## Felsökning av synkroniseringsfel {#troubleshooting-synchronization-error}

Du kan skapa nya program i arbetsflödesdesignern (LiveCycle Workbench).

Om det nyligen skapade programmet och en mapp på /content/dam/formSanddocuments har samma namn, finns det redan ett fel på rotnivån: *En resurs med samma namn som det här programmet.* är loggad.

Lös konflikten genom att byta namn på programmet och manuellt synkronisera resurserna.

![Konflikter i dialogrutan för resurssynkronisering](assets/sync-conflict.png)

**Figur:** *Konflikter i dialogrutan för resurssynkronisering*
