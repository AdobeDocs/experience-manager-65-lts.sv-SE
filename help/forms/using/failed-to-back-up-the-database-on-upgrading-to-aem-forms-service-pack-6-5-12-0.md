---
title: Det gick inte att säkerhetskopiera databasen under uppgradering till 6.5.12.0 för MySQL.
description: När en användare uppgraderar till Experience Manager 6.5.12.0 och klickar på Uppgradera MySQL kan konfigurationshanteraren inte säkerhetskopiera den tidigare Experience Manager Forms-databasen.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,AEM Forms on JEE
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 0%

---

# Det gick inte att säkerhetskopiera databasen under uppgradering till 6.5.12.0 för MySQL {#issue}

När en användare uppgraderar till Experience Manager 6.5.12.0 och klickar på Uppgradera MySQL, misslyckas konfigurationshanteraren med att säkerhetskopiera den tidigare Experience Manager Forms-databasen. Felet visas:

`Failed to backup the previous Adobe Experience Manager Forms Database`


## Gäller för {#applies-to}

* Experience Manager 6.5 Forms

## Lösning {#solution}

Lös problemet genom att öka databasens max_packet_size till 100 M eller så som krävs i my.ini-filen som finns på {AEM_HOME}/mysql.
