---
title: Dynamisk mappning av modell till komponent för SPA
description: Lär dig hur den dynamiska mappningen av modell till komponent sker i JavaScript SPA SDK för Adobe Experience Manager.
contentOwner: bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: spa
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,SPA Editor
role: Developer
exl-id: 051be106-bb15-46b2-8158-53817f68f57c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Dynamisk mappning av modell till komponent för SPA{#dynamic-model-to-component-mapping-for-spas}

I det här dokumentet beskrivs hur den dynamiska mappningen av modell till komponent sker i JavaScript SPA SDK för Adobe Experience Manager (AEM).

{{ue-over-spa}}

## ComponentMapping-modul {#componentmapping-module}

Modulen `ComponentMapping` tillhandahålls som ett NPM-paket till frontendprojektet. Det lagrar komponenter för användargränssnitt och tillhandahåller ett sätt för Single Page-programmet att mappa komponenter för användargränssnitt till AEM-resurstyper. Detta aktiverar en dynamisk upplösning för komponenter när JSON-modellen för programmet analyseras.

Varje objekt i modellen innehåller ett `:type`-fält som visar en AEM-resurstyp. När den är monterad kan den främre komponenten återge sig själv med det fragment av modellen som den har fått från de underliggande biblioteken.

Se [SPA-utkast](/help/sites-developing/spa-blueprint.md) om du vill ha mer information om modellparsning och åtkomst till modellen för den främre komponenten.

Se även npm-paketet: [https://www.npmjs.com/package/@adobe/aem-spa-component-mapping](https://www.npmjs.com/package/@adobe/aem-spa-component-mapping)

## Modellstyrt Single Page-program {#model-driven-single-page-application}

Single-page Applications using the JavaScript SPA SDK for AEM are model-driven:

1. Front-end-komponenter registrerar sig själva för [Component Mapping Store](/help/sites-developing/spa-dynamic-model-to-component-mapping.md#componentmapping-module).
1. Sedan itererar [behållaren](/help/sites-developing/spa-blueprint.md#container), när den har tillhandahållits med en modell av [modellprovidern](/help/sites-developing/spa-blueprint.md#the-model-provider), över sitt modellinnehåll ( `:items`).

1. Om det finns en sida hämtar dess underordnade sidor ( `:children`) först en komponentklass från [komponentmappningen](/help/sites-developing/spa-blueprint.md#componentmapping) och instansierar den sedan.

## Programinitiering {#app-initialization}

Varje komponent utökas med funktionerna i [`ModelProvider`](/help/sites-developing/spa-blueprint.md#the-model-provider). Initieringen har därför följande allmänna form:

1. Varje modellprovider initierar sig själv och lyssnar efter ändringar som gjorts i den del av modellen som motsvarar dess inre komponent.
1. [`PageModelManager`](/help/sites-developing/spa-blueprint.md#pagemodelmanager) måste initieras så som den representeras av [initieringsflödet](/help/sites-developing/spa-blueprint.md).

1. När sidmodellhanteraren har lagrats returnerar den hela appmodellen.
1. Den här modellen skickas sedan till rotkomponenten [Container](/help/sites-developing/spa-blueprint.md#container) i början av programmet.
1. Delar av modellen sprids slutligen till varje enskild underordnad komponent.

![app_model_initialization](assets/app_model_initialization.png)
