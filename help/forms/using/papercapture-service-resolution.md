---
title: Felsöka artikel för att lösa problemet när tjänsten PaperCapture inte kan utföra OCR-åtgärder (Optical Character Recognition) på PDF-filer.
description: Lär dig hur du löser problemet där tjänsten PaperCapture inte kan utföra OCR-åtgärder (Optical Character Recognition) på PDF-filer.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---


# Tjänsten PaperCature kan inte utföra OCR-åtgärder på PDF-filer

## Problem

När du har uppgraderat till AEM Forms Service Pack 6.5.21.0 misslyckas tjänsten `PaperCapture` med att utföra OCR-åtgärder (Optical Character Recognition) på PDF-filer. Tjänsten genererar inte utdata i form av en PDF- eller loggfil.

## Gäller för

Denna lösning gäller
* AEM Forms på alla (JBoss, Weblogic, Websphere) JEE-servrar
* AEM Forms på OSGi-servrar

## Lösning

1. Hämta [snabbkorrigeringen](https://nam04.safelinks.protection.outlook.com/?url=https%3A%2F%2Fexperience.adobe.com%2F%23%2Fdownloads%2Fcontent%2Fsoftware-distribution%2Fen%2Faem.html%3Fpackage%3D%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Fhotfix%2FPaperCaptureSvc.zip&amp;data=05%7C02%7Cruchitas%40adobe.com%7Cf50f80aab6994875271a08dc91f2f137%7Cfa7b1b5a7b34438794aed2c178decee1%7C0%7C0%7C638545719814675925%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&amp;sdata=9pTrMfiMD%2B5kQezxsZwTdOmaaktxURR99d7f6wHr%2FWQ%3D&amp;reserved=0) från portalen för programvarudistribution.
1. Extrahera och kopiera innehållet i den hämtade mappen.
1. Navigera till sökvägarna nedan för motsvarande programservrar:
   * **jboss**:
     `..\Adobe\Adobe_Experience_Manager_Forms\jboss\standalone\svcnative\PaperCaptureSvc`
   * **weblogic**:
     `..\Adobe\Adobe_Experience_Manager_Forms\crx-repository\bedrock\svcnative\PaperCaptureSvc`
   * **websphere**:\
     `..\Adobe\Adobe_Experience_Manager_Forms\crx-repository\bedrock\svcnative\PaperCaptureSvc`
   * **OSGi-konfiguration**:\
     `..\quickstart\crx-quickstart\bedrock\svcnative\PaperCaptureSvc`
1. Stoppa AEM programserver.
1. Ersätt det befintliga innehållet i mappen `PaperCaptureSvc` med det kopierade innehållet.
1. Starta om AEM programserver.

   >[!NOTE]
   >
   > Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.
