---
title: ContextHub
description: ContextHub är ett ramverk för att lagra, ändra och presentera kontextdata
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Personalization
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---

# ContextHub{#contexthub}

ContextHub är ett ramverk för att lagra, ändra och presentera kontextdata. Med JavaScript-API:t på klientsidan kan du komma åt data för att anpassa innehåll.

>[!NOTE]
>
>Referensimplementeringen [We.Retail](/help/sites-developing/we-retail.md) implementerar ContextHub och kan fungera som referens när du integrerar ContextHub i ditt eget projekt.

>[!CAUTION]
>
>Sökvägen som innehåller exempelkonfigurationen för ContextHub som används av referensimplementeringen [We.Retail](/help/sites-developing/we-retail.md) ( `/libs/settings/cloudsettings/legacy`) bör endast användas som referens för att skapa din egen konfiguration.
>
>Använd inte i ett projekt som din egen ContextHub-konfiguration.

## Persistence {#persistence}

ContextHub lagrar kontextdata på klienten. Med ContextHub JavaScript API kan du komma åt butiker för att skapa, uppdatera och ta bort data efter behov. Därför representerar ContextHub ett datalager på dina sidor.

Varje ContextHub-butik är en instans av en fördefinierad lagringstyp:

* ContextHub innehåller flera [typer av exempelarkiv](/help/sites-developing/ch-samplestores.md).
* Använd AEM-konsoler för att [skapa butiker](ch-configuring.md#creating-a-contexthub-store).
* Utvecklare kan [skapa anpassade butikstyper](/help/sites-developing/ch-extend.md#creating-custom-store-candidates).
* Utvecklare kan [komma åt lagringsdata](/help/sites-developing/ch-adding.md#interacting-with-contexthub-stores) via JavaScript.

## Segmentering {#segmentation}

ContextHub innehåller en segmenteringsmotor som hanterar segment och fastställer vilka segment som matchas för den aktuella kontexten. Flera segment är definierade. Du kan använda JavaScript API för att [identifiera lösta segment](/help/sites-developing/ch-adding.md#determining-resolved-contexthub-segments).

## Presentation {#presentation}

Verktygsfältet [ContextHub](/help/sites-authoring/ch-previewing.md) gör att marknadsförare och författare kan se och ändra lagringsdata för att simulera användarupplevelsen när de skapar sidor. Verktygsfältet består av grupper med UI-moduler som ger åtkomst till ContextHub-butiker.

Varje ContextHub-gränssnittsmodul är en instans av en fördefinierad modultyp:

* ContextHub innehåller flera [exempelmodultyper](/help/sites-developing/ch-samplemodules.md).
* Använd AEM-konsoler för att [lägga till gränssnittsmoduler](ch-configuring.md#adding-a-ui-module) och för att [gruppera dem i gränssnittslägen](ch-configuring.md#adding-a-ui-mode).

* Utvecklare kan [skapa anpassade modultyper](/help/sites-developing/ch-extend.md#creating-contexthub-ui-module-types).

Utvecklare måste [lägga till ContextHub-komponenten på sidan](/help/sites-developing/ch-adding.md).
