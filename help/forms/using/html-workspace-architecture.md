---
title: AEM Forms Workspace Architecture
description: Konceptuell information och översikt över arkitekturen för arbetsytan i LiveCycle AEM Forms.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: User, Developer
exl-id: d317274f-2c9a-4809-b43e-2efebc8fcb3f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# AEM Forms Workspace Architecture {#aem-forms-workspace-architecture}

AEM Forms arbetsyta är ett webbprogram som finns på CRX™. När arbetsytan öppnas i en webbläsare öppnas en CRX-resurs och programmet återges som en HTML-sida i webbläsaren.

Programmet får åtkomst till AEM Forms-servern på REST-slutpunkter för att göra följande:

* Hämta användaruppgifter, processstartpunkter, processhistorik och användarinformation
* Utför åtgärd för uppgifter
* Frågeuppgifter i databasen
* Uppdatera användarinställningar med mera

AEM Forms-servern har åtkomst till AEM Forms-databasen via JDBC. Databasen innehåller uppgifter, processer och instanser, användare och relaterad information.

AEM Forms arbetsyta är utformad i modulära JavaScript™-komponenter som kan anpassas individuellt och återanvändas i andra webbprogram. Komponenterna baseras på BackBone, ett JavaScript-bibliotek som ger struktur åt webbprogram. En detaljerad artikel som beskriver interaktionen mellan komponenter med BackBone är [här](/help/forms/using/backbone-interaction.md). Komponenternas ordning i mappstrukturen för CRX beskrivs i [den här](/help/forms/using/folder-structure.md) artikeln.

Paket som levereras för AEM Forms arbetsyta:

* `adobe-lc-workspace-pkg-<version>.zip`: Det är ett CRX-paket, dvs. det kan distribueras i CRX med hjälp av pakethanteraren.
* `adobe-lc-workspace-<version>-src.zip`: Det är ett arkiv som innehåller fullständig kod för AEM Forms-arbetsytan och skript för att skapa distributionspaketen Ship, Debug och Dev.
