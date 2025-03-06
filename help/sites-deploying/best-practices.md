---
title: Distribuera bästa praxis
description: Lär dig hur du driftsätter och underhåller Adobe Experience Manager (AEM) på det mest effektiva och effektiva sättet.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
exl-id: 4f830ee9-e0e3-48df-b67d-709258cb1991
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Distribuera bästa praxis{#deploying-best-practices}

Bästa praxis beskriver hur man driftsätter eller underhåller Adobe Experience Manager (AEM) på det mest effektiva och effektiva sättet. Den här växande ämneslistan innehåller olika ämnesområden i AEM.

Följande områden har dokumentation om användning och underhåll av bästa metoder och rekommendationer:

* [Oak](#oak)
* [UI](#ui)
* [Prestanda](#performance)

Mer information om hur du administrerar, utvecklar och redigerar finns i följande:

* [Administrera metodtips](/help/sites-administering/administer-best-practices.md)
* [Utveckla bästa praxis](/help/sites-developing/best-practices.md)
* [Bästa tillvägagångssätt](/help/sites-authoring/best-practices.md)

Specifika dokument beskrivs och länkas till i de tabeller som följer.

## Oak {#oak}

[Oak](/help/sites-deploying/platform.md) är en skalbar och högpresterande hierarkisk innehållsdatabas som utgör grunden för AEM.

<table>
 <tbody>
  <tr>
   <td><p>Skalbarhet, prestanda och katastrofåterställning</p> </td>
   <td><a href="/help/sites-deploying/performance.md">Prestanda och skalbarhet</a></td>
   <td>Innehåller en rapport om den tekniska flexibiliteten, höga prestanda och funktioner för återställning efter ljudkatastrof</td>
  </tr>
  <tr>
   <td>Rekommenderade Oak-driftsättningar</td>
   <td><a href="/help/sites-deploying/recommended-deploys.md">Rekommenderade driftsättningar</a></td>
   <td>Beskriver distributionsscenarier</td>
  </tr>
  <tr>
   <td>Mongo topologi</td>
   <td><a href="/help/sites-deploying/recommended-deploys.md">Bästa praxis för Mongo-topologi</a></td>
   <td>Beskriver mongo-topologi - när du ska använda vilken topologi.</td>
  </tr>
  <tr>
   <td>Alternativ för datalagring</td>
   <td><a href="/help/sites-deploying/data-store-config.md">Konfigurera nod- och datalager</a></td>
   <td>I det här dokumentet förklaras de bästa sätten att lagra binära data och innehållsnoder. Innehåller information om hur du använder datalagret i Amazon S3.</td>
  </tr>
  <tr>
   <td>Sök i Oak</td>
   <td><a href="/help/sites-deploying/best-practices-for-queries-and-indexing.md">Bästa tillvägagångssätt för frågor och indexering</a><br /> </td>
   <td>Beskriver de bästa sätten att indexera innehåll.</td>
  </tr>
 </tbody>
</table>

## UI {#ui}

De bästa sätten att använda användargränssnittet beskrivs här:

[Rekommendationer för användargränssnitt för kunder](/help/sites-deploying/ui-recommendations.md)

AEM har för närvarande två användargränssnitt: klassiskt och pekoptimerat användargränssnitt i samma version. Därför måste kunderna fatta ett beslut om vilken användning som ska ske under projektets genomförande. Det här dokumentet är avsett att hjälpa till att hitta rätt alternativ.

## Prestanda {#performance}

De bästa metoderna för prestandaanvändning listas här:

<table>
 <tbody>
  <tr>
   <td>Best Practices for Quality Assurance</td>
   <td><a href="/help/sites-deploying/configuring-performance.md#best-practices-for-quality-assurance">Best Practices for Quality Assurance</a></td>
   <td>En standardiserad översikt över problemen med att definiera ett testkoncept specifikt för prestandatester i din <em>publish</em> -miljö. Detta är främst av intresse för kvalitetstekniker, projektledare och systemadministratörer.</td>
  </tr>
  <tr>
   <td>Använda Dispatcher med ett CDN</td>
   <td><a href="https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html#using-dispatcher-with-a-cdn">Använda Dispatcher med ett CDN</a></td>
   <td>Ett nätverk för innehållsleverans (CDN), som Akamai Edge Delivery eller Amazon Cloud Front, levererar innehåll från en plats nära slutanvändaren.</td>
  </tr>
  <tr>
   <td>Prestandaoptimering</td>
   <td><a href="/help/sites-deploying/configuring-performance.md">Prestandaoptimering</a></td>
   <td>Ett viktigt problem är den tid det tar för er webbplats att svara på besökarnas förfrågningar.</td>
  </tr>
  <tr>
   <td>Prestandatestning</td>
   <td><a href="/help/sites-deploying/best-practices-for-performance-testing.md">Bästa metoder för prestandatestning</a></td>
   <td>Beskriver de bästa sätten att köra prestandatester på en AEM-distribution.<br /> </td>
  </tr>
 </tbody>
</table>
