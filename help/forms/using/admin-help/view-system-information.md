---
title: Visa systeminformation
description: Lär dig hur du kan visa resursövervakningsdiagram och information om servern där AEM-formulär körs.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/health_monitor
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 6be4ce1d-39fe-4a25-9d4e-f1cbc593d2c7
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---

# Visa systeminformation {#view-system-information}

På fliken System visas resursövervakningsdiagram och information om servern som kör AEM-formulär. Om du vill komma åt den här informationen klickar du på Hälsoövervakning i det övre högra hörnet på sidan i administrationskonsolen. Om du kör AEM-formulär i en klustermiljö visas informationen för den nod som valts i serverlistan.

Om du vill spara den aktuella systeminformationen som en egenskapsfil klickar du på Spara.

I den högra rutan på fliken System visas grafiska representationer av följande information:

* Jobb- och arbetsräkningsobjekt
* Användning av heap och Allmän heap
* Användning utan heap och implementerad utan heap

Du kan dra pekaren längs tidslinjen för att hämta värden för en viss tidpunkt.

>[!NOTE]
>
>Diagramdata, serverinformationsvärden och klockslag uppdateras var 10:e minut. Informationen visas inte i realtid.

I den vänstra rutan på fliken System visas följande information om servern eller noden:

**Virtuell dator:** Java Virtual Machine-version (JVM) på servern.

**Leverantör av virtuell dator:** Tillverkare av JVM.

**Version för virtuell dator:** JVM-versionsnummer

**Datornamn:** Värdnamn för servern där AEM-formulär är installerade.

**Starttid:** Tiden, i timmar och minuter, som servern har körts.

**Just-In-Time-kompilator:** Namnet på den kompilator som används.

**Kompileringstid:** Den tid som har ägnats åt kompileringen.

**Antal Live Threads:** Det totala antalet trådar som finns i AEM formulärsystem.

**Antal Threads-toppar:** Största antal livetrådar som någonsin spelats in i systemet.

**Antal inlästa klasser:** Antal klasser som har lästs in i JVM.

**Antal ej inlästa klasser:** Antal klasser som inte har lästs in från JVM.

**Minimiheap:** Den minsta heap som användes.

**Maximal heap:** Den maximala heap som användes.

**Operativsystemnamn:** Namnet på det operativsystem som körs på AEM Forms-servern.

**Operativsystemversion:** Versionsnummer för det operativsystem som körs på AEM Forms Server.

**Operativsystemsmatris:** Operativsystemsarkitekturen som JVM körs på.

**Antal processorer:** Antalet processorer i systemet.

**Argument för virtuell dator:** Argumentet som används av JVM.

**Klasssökväg:** Klasssökvägen som används av JVM.

**Bibliotekssökväg:** Bibliotekssökvägen som används av JVM.

**Sökväg till startklass:** Sökvägen till startklassen som används av JVM.

**Programservertyp:** Den typ av programserver som används för att köra AEM-formulär.

**Programserverversion:** Versionsnummer för den programserver som används för att köra AEM-formulär.

**Programserverleverantör:** Tillverkare av den programserver som används för att köra AEM-formulär.

**Installationsdatum:** Datum (i formatet åååå-mm-dd) när AEM-formulär installerades.

**AEM-formulär Version:** Version av AEM-formulär som är installerade.

**Patchversion:** AEM-formulärkorrigeringsnummer.

**Databasnamn:** Typ av databas som används av AEM-formulär.

**Databasversion:** Versionsnummer för den databas som används av AEM-formulär.

**Namn på databasenhet:** Namnet på drivrutinen som JVM använder för att ansluta till databasen.

**Databasdrivrutinsversion:** Den version av drivrutinen som JVM använder för att ansluta till databasen.

Med knappen **Spara** kan du spara den här systeminformationen i en egenskapsfil.
