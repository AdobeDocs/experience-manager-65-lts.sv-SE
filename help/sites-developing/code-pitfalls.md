---
title: Kodfallare
description: Vanliga kodfallsfall att undvika vid utveckling för AEM
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '89'
ht-degree: 0%

---

# Kodfallare{#code-pitfalls}

## Undvik att skicka bindningar i Java-kod {#avoid-sling-bindings-in-java-code}

Sling Bindings är ett olämpligt sätt att få tillgång till en tjänst i 90 % av fallen. Du bör i stället använda anteckningarna *@Reference* eller *@Inject*.

## Undvik tråd.avbrott i Java-kod {#avoid-thread-interrupt-in-java-code}

*Thread.Intert* är farligt eftersom det kan stänga filer, inklusive Lucene-filer och beständiga cachefiler, när de anropas vid fel tidpunkt.

## Undvik att blanda Java-synkronisering med ReadWriteLocks {#avoid-mixing-java-synchronization-with-readwritelocks}

Detta kan leda till ett tävlingsvillkor där koden till slut kommer att låsas.
