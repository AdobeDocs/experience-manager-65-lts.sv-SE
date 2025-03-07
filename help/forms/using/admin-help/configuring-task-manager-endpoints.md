---
title: Konfigurera slutpunkter för Aktivitetshanteraren
description: Lär dig hur du konfigurerar Task Manager-slutpunkter så att tjänsten anropas. Olika inställningar krävs för att konfigurera slutpunkter för Task Manager.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_endpoints
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 57fd8b5d-6347-4b83-9489-e8ee59ee39a5
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Konfigurera slutpunkter för Aktivitetshanteraren {#configuring-task-manager-endpoints}

Med Task Manager-slutpunkter kan Workspace-användare anropa tjänsten.

**Slutpunktsinställningar för Aktivitetshanteraren**

Använd följande inställningar för att konfigurera en Task Manager-slutpunkt.

**Namn:** (obligatoriskt) Identifierar slutpunkten. Namnet visas i kortvyn i Workspace. Ta inte med ett &lt;-tecken eftersom det kortar av namnet som visas i Workspace. Om du anger en URL som namn på slutpunkten kontrollerar du att den överensstämmer med de syntaxregler som anges i RFC1738.

**Beskrivning:** En beskrivning av slutpunkten. Ta inte med ett &lt;-tecken eftersom det kortar av beskrivningen som visas i Workspace.

**Aktivitetsinstruktioner:** Instruktioner för den användare som startar det här arbetsflödet.

**Processägare:** Namnet på den person som är ansvarig för processen.

**Användaren kan vidarebefordra aktiviteten:** Tillåter användaren att vidarebefordra den första aktiviteten.

**Visa fönster för bifogad fil:** Används för att visa fönstret för bifogad fil.

**Tillåt tillägg av bifogade filer:** Tillåter användaren att lägga till bifogade filer och anteckningar.

**Inledande låst aktivitet:** Låser den inledande aktiviteten.

**Lägg till åtkomstkontrollistor för delade köer:** Den första uppgiften skapas med åtkomstkontrollistor för användare i delade köer.

**Kategorisering:** (obligatoriskt) Den kategori som användaren ska se formuläret i Workspace i. Välj en kategori i listan eller välj Ny kategori om du vill lägga till en kategori.

**Åtgärdsnamn:** (obligatoriskt) En lista över åtgärder som kan tilldelas slutpunkten.
