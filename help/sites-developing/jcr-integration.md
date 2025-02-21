---
title: JCR-integrering
description: Lär dig några tips när du behöver integrera med Adobe Experience Manager på JCR-nivå.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# JCR-integrering{#jcr-integration}

## Föredra Sling Resource API till JCR API {#prefer-the-sling-resource-api-to-jcr-api}

Sling API fungerar på en högre och mer abstrakt nivå än JCR API. Detta gör att koden kan återanvändas mer och vara oberoende av den underliggande lagringen. Detta gör det enklare att inkludera externa virtuella data via ResourceProvider-mekanismen om det behövs.

## Undvik frågor där det är möjligt {#avoid-queries-wherever-possible}

Det är alltid snabbare att navigera i databasen för att hämta data än att köra en fråga. Det finns tillfällen då frågor är nödvändiga, t.ex. en slutanvändarfråga eller behöver hitta strukturerat innehåll från hela databasen, men i alla andra fall är det bäst att navigera till de noder som behövs. Frågor bör alltid undvikas i återgivningslogik som navigeringskomponenter,&quot;lista över senaste objekt&quot;, antal objekt och så vidare. I dessa fall är det bättre att gå igenom hierarkin eller cachelagra resultatet i förväg så att det kan användas direkt när det återges.

## Begränsa omfattningen av JCR-observation {#restrict-the-scope-of-jcr-observation}

När du lyssnar efter händelser i databasen är det viktigt att begränsa omfattningen så mycket som möjligt. Det är till exempel mycket bättre att lyssna efter en händelse på `/etc/mycompany` än att lyssna på `/etc`. Lyssna aldrig efter händelser i databasroten. Kontrollera dessutom att återkallningsmetoderna körs så snabbt som möjligt när det inte finns något att göra.

## Eliminera användningen av JCR-administratörsåtkomst {#eliminate-use-of-jcr-admin-access}

Från och med AEM 6 används inte inloggningsadministration eftersom en administrativ session har hämtats från ResourceResolverFactory. Tjänstkonton ska i stället skapas för back office-åtgärder som skulle kräva den här typen av åtkomst och ResourceResolverFactory kan användas för att hämta en ResourceResolver för det här kontot.
