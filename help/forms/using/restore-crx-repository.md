---
title: Det går inte att återställa skadad CRX-databas som kan användas på JEE-klusterservern
description: Lär dig hur du kan återställa en skadad CRX-databas.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 716d8eb2-2010-4d55-b8fe-bd4f6f256a4d
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Det går inte att återställa skadad CRX-databas {#unable-to-restore-corrupt-crx-repository}

## Problem {#issue}

För AEM Forms på JEE som använder en relationsdatabas ska tiden på den dator som är värd för AEM Forms och relationsdatabasen alltid vara i absolut synk. Om tiden på dessa datorer blir osynkroniserad kan AEM Forms CRX-databas på JEE-servern bli otillgänglig. Den kan vara skadad och oåtkomlig via URL. Felet `AuthenticationsupportService missing` har loggats.

## Förutsättningar {#prerequisites}

Säkerhetskopiera CRX-databasen innan du utför stegen nedan.

## Lösning {#solution}

1. Gå till `https://[AEM Forms Server]:[port]/system/console/bundles`.

1. Leta reda på paketet `oak-core` och kontrollera om det körs.

1. Starta om paketet `oak-core` om det inte körs. Om ikonen ![Pause button](/help/forms/using/assets/stop.png) finns framför paketet `oak-core` anger den att paketet körs.

1. Om problemet fortfarande inte är löst kan du återställa från CRX-databasen från säkerhetskopian eller återskapa CRX-databasen om det inte finns någon säkerhetskopia.


## Gäller för {#applies-to}

Den här lösningen gäller för AEM Forms i JEE Cluster.
