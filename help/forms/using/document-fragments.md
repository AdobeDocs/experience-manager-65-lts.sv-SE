---
title: Dokumentfragment
description: Med dokumentfragment som text, listor, villkor och layoutfragment i Correspondence Management kan du skapa statiska, dynamiska och repeterbara komponenter i kundens korrespondens.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 568a1513-1de9-4f68-be09-f47cd5b30847
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Dokumentfragment {#document-fragments}

Dokumentfragment är återanvändbara delar/komponenter i en korrespondens som du kan använda för att skapa interaktiv kommunikation/brev. Dokumentfragmenten är av följande typer:

* **Text**: En textresurs är en del av innehållet som består av ett eller flera textstycken. Ett stycke kan vara statiskt eller dynamiskt.

   * [Texter i interaktiv kommunikation](/help/forms/using/texts-interactive-communications.md)

* **Villkor**: Med villkor kan du definiera vilket innehåll som ska tas med när korrespondensen skapas, baserat på angivna data. Villkoret beskrivs i termer av kontrollvariabler. En kontrollvariabel kan antingen vara ett dataordlisteelement eller en platshållare.

   * [Villkor i interaktiv kommunikation](/help/forms/using/conditions-interactive-communications.md)

* **Lista:** Listan är en grupp dokumentfragment, inklusive text, listor, villkor och bilder. Ordningen på listelementen kan vara fast eller redigerbar. När du skapar en bokstav kan du använda några eller alla listelement för att återanvända ett mönster med element.
* **Layoutfragment**: Ett layoutfragment är en layout som kan användas i en eller flera bokstäver. Ett layoutfragment används för att skapa repeterbara mönster, särskilt dynamiska tabeller. Layouten kan innehålla typiska formulärfält som &quot;Adress&quot; och &quot;Referensnummer&quot;. Den innehåller också tomma delformulär som anger målområden. Layouterna (XDP) skapas i Designer och överförs sedan till AEM Forms.
