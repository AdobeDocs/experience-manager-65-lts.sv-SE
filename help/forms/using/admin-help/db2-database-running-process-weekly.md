---
title: 'DB2&reg; databas: Kör en process varje vecka'
description: Lär dig hur du kan förbättra prestandan i din AEM Forms DB2&reg;-databas.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_aem_forms_database
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: e8cf9e73-345c-4dea-8361-b678c1a3cd1b
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# DB2®-databas: Köra en process varje vecka{#db-database-running-a-process-weekly}

Om AEM Forms DB2®-databasen börjar köras långsamt kan prestanda förbättras om du kör följande process varje vecka:

1. Start DB2® Control Center:

   (Windows) Välj Start > Program > IBM® DB2® > General Administration Tools > Control Center.

   (Linux® och UNIX®) Skriv kommandot `db2jcc` från en kommandotolk.

1. Klicka på Alla databaser i objektträdet i DB2® Control Center.
1. Klicka på databasen som du skapade för AEM Forms och klicka på mappen Tabeller.
1. Markera alla databastabeller i innehållsrutan, högerklicka på dem och välj sedan Kör statistik.
1. Gå till Statistik > Indexstatistik.
1. Välj Samla statistik för alla index, välj Samla statistik för index med utökad detaljerad statistik och klicka sedan på OK.

Ett meddelande visas när processen har slutförts. Stäng meddelandet.
