---
title: Kompilera testplanen
description: De enskilda testfallen samlas i din testplan
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: testing
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: b2dfc8fb-7bc4-4b5e-8c8f-1463fdc18e50
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---

# Kompilera testplanen{#compiling-your-test-plan}

De enskilda testfallen sammanförs sedan i din testplan, som också definierar:

**Prioriteringar**

Vissa tester kommer att ha större betydelse än andra, så det är tillrådligt att ange deras prioritet.

Vissa tester kan t.ex. påverka ett Go-/No-Go-beslut och måste därför bekräftas vid varje testad interimsrelease.

**Iterationer**

Om ditt projekt använder någon form av utvecklingsiteration (med flera releaser tillgängliga) kan du behöva eller vill ha en indikation på resultatet för varje iteration. Detta kan användas för att ange:

* vilka tester som ska omfattas av upprepningen.
* resultaten av tester som upprepas i olika iterationer.
* att prioritetstester och tester av grundläggande funktioner upprepas med regelbundna mellanrum.

**Tester**

Vid något tillfälle kan du antingen utse rätt testgrupp eller en specifik testperson (eventuellt beroende på tillgänglighet och/eller erfarenhet).

**Sammanfattning eller översikt**

För rapportering vill du ge en översikt över testresultaten:

* Procentandel av tester som redan omfattas.
* Procent lyckade/misslyckade.
* Specifika siffror för de prioriterade testerna.
