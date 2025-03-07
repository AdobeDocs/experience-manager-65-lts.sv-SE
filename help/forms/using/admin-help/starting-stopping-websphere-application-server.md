---
title: Startar och stoppar WebSphere Application Server
description: Flera procedurer kräver att du stoppar eller startar instansen av WebSphere där du vill distribuera AEM formulärprodukter. I det här dokumentet beskrivs hur du startar och stoppar WebSphere Application Server.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_application_server
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 20cd6efb-edcf-4c87-b0f5-bdec5a0f6280
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Startar och stoppar WebSphere Application Server {#starting-and-stopping-websphere-application-server}

Flera procedurer kräver att du stoppar eller startar instansen av WebSphere där du vill distribuera AEM formulärprodukter. Om du är osäker på om programservern har startats kan du först visa statusen för WebSphere Application Server.

## Visa status för WebSphere Application Server {#view-the-status-of-websphere-application-server}

1. Gå till katalogen `[appserver root]/bin` från en kommandotolk.
1. Ange följande kommando och ersätt *server_name* med namnet på WebSphere-programservern:

   * (Windows) `serverStatus.bat`*server_name*
   * (Linux, UNIX) ./ `serverStatus.sh`*server_name*

## Starta WebSphere Application Server {#start-websphere-application-server}

1. Gå till katalogen `[appserver root]/bin` från en kommandotolk.
1. Ange följande kommando och ersätt *server_name* med namnet på WebSphere-programservern:

   * (Windows) `startServer.bat`*server_name*
   * (Linux, UNIX) ./ `startServer.sh`*server_name*

## Stoppa WebSphere-programserver {#stop-websphere-application-server}

1. Gå till katalogen `[appserver root]/bin` från en kommandotolk.
1. Ange följande kommando och ersätt *server_name* med namnet på WebSphere-programservern:

   * (Windows) `stopServer.bat`*server_name*
   * (Linux, UNIX) ./ `stopServer.sh`*server_name*
