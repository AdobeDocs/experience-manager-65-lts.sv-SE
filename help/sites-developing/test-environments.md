---
title: Vilka testmiljöer behövs?
description: Flera miljöer bör beaktas som en del av testningen
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: testing
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: f74fbf2b-62bb-4fac-9ecb-5ace90ba0275
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# Vilka testmiljöer behövs?{#which-test-environments-will-be-needed}

För att definiera vilka konfigurationer som ska testas bör du tänka på följande:

**Utveckling** - För enhets- och vissa integreringstester.

**Testar** - För de flesta testerna.

**Live** - För slutliga prestanda- och stresstester. Även för godkännandetester med kunden.

Bestäm vilka instanser du behöver och var (vanligtvis minst en av varje för alla testnivåer):

**Författare** - Den här instansen tillåter författare att ange och publicera innehåll.

**Publicera** - Den här instansen presenterar webbplatsen i dess publicerade form så att besökarna kan komma åt den.

Testad med Dispatcher.

Slutligen måste den faktiska maskinvaran beaktas - alla prestandatester bör göras på ett system så nära som möjligt i konfigurationen till den slutliga livemiljön. Därför rekommenderar vi även att du delar upp projektstarten i en

**Mjuk start** - Minskad tillgänglighet, vilket ger tid för prestandatester, justering och optimering under realistiska förhållanden i produktionsmiljön.

**Hård start** - fullständig tillgänglighet.
