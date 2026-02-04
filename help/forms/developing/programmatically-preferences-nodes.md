---
title: Hantera PreferencesNodes programmatiskt
description: Använd Preferences Manager Service API (Java) för att hantera Preferences Nodes programmatiskt.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: operations
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,APIs & Integrations
hide: true
hidefromtoc: true
exl-id: 95a83858-c0b7-4c68-b4a9-d525bfc663c0
source-git-commit: 51342861dd01e659999c19fbe0274e8d3cbcf8c4
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Hantera inställningsnoderna programmatiskt {#programmatically-managing-the-preferencesnodes}

**Exempel och exempel i det här dokumentet är bara för AEM Forms i JEE-miljön.**

I det här avsnittet beskrivs hur du kan använda Preferences Manager Service API (Java) för att hantera Preferences Nodes programmatiskt.

Du kan ändra konfigurationsinställningarna manuellt från administratörsgränssnittet. Navigera till `Home>Settings>User Management> Configuration>Manual Configuration` om du vill ändra alternativen. Importera `config.xml` när du har gjort ändringarna kommer du att märka att alla ändringar förutom de som har gjorts på noden `/Adobe/Adobe Experience Manager Forms/Config/UM persist` går förlorade. Förhandsgranskningen av Import och export av användarhantering stöder inte ändring av konfigurationsinställningar för andra komponenter. Dessa ändringar kan nu göras med `PreferencesManagerServiceClient` API:er.

**Sammanfattning av steg**
Så här hanterar du inställningarna för noder med programkod:

1. Inkludera projektfilerna.
1. Skapa en`PreferencesManagerService`-klient.
1. Anropa lämpliga roll- eller behörighetsåtgärder.

**Inkludera projektfilerna**

Inkludera nödvändiga filer i utvecklingsprojektet. Om du skapar ett klientprogram med Java inkluderar du de JAR-filer som behövs. Om du använder webbtjänster måste du inkludera proxyfilerna.

**Skapa en `PreferencesManagerService`-klient**

Innan du programmässigt kan utföra en användarhanteringsåtgärd `PreferencesManagerService` måste du skapa en `PreferencesManagerService`-klient. Skapa ett `PreferencesManagerServiceClient`-objekt med Java API.

**Anropa lämplig roll eller lämpliga behörighetsåtgärder**

När du har skapat tjänstklienten kan du sedan anropa Preferences Manager-åtgärderna. Med tjänstklienten kan du läsa och ange behörigheter.
