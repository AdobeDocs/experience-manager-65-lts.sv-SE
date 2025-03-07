---
title: 'IBM DB2-databas: Kör kommandon för regelbundet underhåll'
description: I det här dokumentet visas IBM DB2-kommandon som rekommenderas för regelbundet underhåll av AEM formulärdatabas.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_aem_forms_database
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: cf505217-78c0-4af7-80f6-5ee75f09f46c
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 0%

---

# IBM DB2-databas: Kör kommandon för regelbundet underhåll {#ibm-db-database-running-commands-for-regular-maintenance}

Följande IBM DB2-kommandon rekommenderas för regelbundet underhåll av AEM formulärdatabas. Detaljerad information om underhåll och prestandajustering för din DB2-databas finns i *IBM DB2 Administration Guide*.

* **körningsstatistik:** Det här kommandot uppdaterar statistik som beskriver de fysiska egenskaperna för en databastabell, tillsammans med tillhörande index. Dynamiska SQL-satser som genererats av AEM-formulär använder automatiskt den uppdaterade statistiken, men statiska SQL-satser som skapats i en databas kräver att kommandot `db2rbind` också körs.
* **db2rbind:** Det här kommandot binder om alla paket i databasen. Använd det här kommandot när du har kört verktyget `runstats` för att validera alla paket i databasen igen.
* **rapporttabell eller index:** Det här kommandot kontrollerar om en omorganisering av vissa tabeller och index krävs.

  I takt med att databaserna växer och förändras är det viktigt att tabellstatistiken beräknas om för att förbättra databasens prestanda och bör göras regelbundet. Dessa kommandon kan köras antingen manuellt med skript eller med ett cron-jobb.

>[!NOTE]
>
>Innan du kör kommandot `runstats` måste databasen innehålla data och minst en katalogsynkronisering måste ha utförts.

För en liten databas, t.ex. för 10 000 användare eller 2 500 grupper, räcker det att anropa kommandot `runstats` för att reducera synkroniseringstiderna.

För större databaser, till exempel för 100 000 användare eller 10 000 grupper, kör du kommandot `reorg` innan du kör kommandot `runstats`.

## Använda kommandot runstats i din AEM-formulärdatabas {#use-the-runstats-command-on-your-aem-forms-database}

Kör kommandot `runstats` på följande databastabeller och index för AEM-formulär.

>[!NOTE]
>
>Kommandot `runstats` behöver bara köras under den första databassynkroniseringen. Den måste dock köras två gånger under den processen: en gång under synkroniseringen av användare och grupper och sedan under synkroniseringen av gruppmedlemmar. Kontrollera att skriptet körs fullständigt varje gång du kör det.

Korrekt syntax och användning finns i dokumentationen från databastillverkaren. Nedan används `<schema>` för att ange schemat som är associerat med ditt DB2-användarnamn. Om du har en enkel standardinstallation av DB2 är detta databasens schemanamn.

```sql
     TABLE <schema>.EDCPRINCIPALGROUPENTITY
 
     TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY
 
     TABLE <schema>.EDCPRINCIPALENTITY
 
     TABLE <schema>.EDCPRINCIPALUSERENTITY
 
     TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY
 
     TABLE <schema>.EDCPRINCIPALENTITY FOR INDEXES ALL
 
     TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY FOR INDEXES ALL
 
     TABLE <schema>.EDCPRINCIPALUSERENTITY FOR INDEXES ALL
 
     TABLE <schema>.EDCPRINCIPALGROUPENTITY FOR INDEXES ALL
 
     TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY FOR INDEXES ALL
```

## Kör organisationskommandot i din AEM-formulärdatabas {#run-the-reorg-command-on-your-aem-forms-database}

Kör kommandot `reorg` på följande databastabeller och index för AEM-formulär. Korrekt syntax och användning finns i dokumentationen från databastillverkaren.

```sql
     TABLE <schema>.EDCPRINCIPALGROUPENTITY
 
     TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY
 
     TABLE <schema>.EDCPRINCIPALENTITY
 
     TABLE <schema>.EDCPRINCIPALUSERENTITY
 
     TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY
 
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALENTITY
 
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY
 
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALUSERENTITY
 
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALGROUPENTITY
 
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY
```
