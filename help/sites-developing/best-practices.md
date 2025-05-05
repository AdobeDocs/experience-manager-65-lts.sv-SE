---
title: Best Practices for AEM developers
description: Adobe tekniker och konsultteam har utvecklat en omfattande uppsättning metodtips för AEM-utvecklare.
contentOwner: Justin Edelson
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: fc2aa62a-3fc4-491d-aff5-74896998d7d6
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# Bästa praxis{#best-practices}

## Best Practices for Developers - Getting Started {#best-practices-for-developers-getting-started}

Adobe tekniker och konsultteam har utvecklat en omfattande uppsättning metodtips för AEM-utvecklare. Adobe-utvecklare följer dessa standarder när de utvecklar viktiga AEM-produktuppdateringar och kundkod för kundimplementeringar.

Innan du börjar utveckla AEM bör du först granska följande metodtips:

* [Utvecklingspraxis](/help/sites-developing/development-practices.md)
* [Innehållsarkitektur](/help/sites-developing/content-architecture.md)
* [Programvaruarkitektur](/help/sites-developing/software-architecture.md)
* [Kodningstips](/help/sites-developing/coding-tips.md)
* [Code Pitfalls](/help/sites-developing/code-pitfalls.md)
* [JCR-samverkan](/help/sites-developing/jcr-integration.md)
* [OSGi Bundles](/help/sites-developing/osgi-bundles.md)
* [Bästa praxis för Java API](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/understand-java-api-best-practices.html?lang=sv-SE)

### Ytterligare metodinformation {#additional-best-practices-information}

Följande områden har dokumentation som är specifik för att utveckla metodtips:

* [Sites](#sites)
* [Verktyg/HTML](#tooling-htl)

Specifika dokument beskrivs och länkas till i de tabeller som följer.

De bästa sätten att administrera, distribuera och underhålla, eller att skapa, finns i något av följande:

* [Administrera metodtips](/help/sites-administering/administer-best-practices.md)
* [Bästa tillvägagångssätt](/help/sites-authoring/best-practices.md)
* [Använda vedertagna rutiner](/help/sites-deploying/best-practices.md)

## Sites {#sites}

Hantering och redigering av webbplatsinnehåll har några beprövade metoder:

<table>
 <tbody>
  <tr>
   <td>En del av teorin bakom standardgränssnittet med pekfunktioner.</td>
   <td><p><a href="/help/sites-developing/touch-ui-concepts.md">Pekaktiverat användargränssnitt: Concepts</a></p> <p><a href="/help/sites-developing/touch-ui-structure.md">Pekaktiverat användargränssnitt: Struktur</a></p> </td>
   <td>Dessa dokument ger en översikt över begreppen, och strukturen, för det beröringskänsliga användargränssnittet.</td>
  </tr>
  <tr>
   <td>Pekaktiverat användargränssnitt: Anpassa konsoler </td>
   <td><a href="/help/sites-developing/customizing-consoles-touch.md">Anpassa användargränssnittskonsoler med pekskärm</a></td>
   <td>I det här dokumentet beskrivs det bästa sättet att utöka konsolerna för det beröringsaktiverade användargränssnittet.</td>
  </tr>
  <tr>
   <td>Touchaktiverat användargränssnitt: anpassa sidredigering</td>
   <td><a href="/help/sites-developing/customizing-page-authoring-touch.md">Anpassa redigering av pekaktiverade gränssnittssidor</a></td>
   <td>Beskriver hur du utökar sidredigering för det beröringsaktiverade användargränssnittet.</td>
  </tr>
  <tr>
   <td>Arbetsflöden</td>
   <td><a href="/help/sites-developing/workflows-best-practices.md">Utveckla och utöka arbetsflöden</a></td>
   <td><p>Med arbetsflöden kan du automatisera Adobe Experience Manager (AEM)-aktiviteter och representera en stor del av den bearbetning som sker i en AEM-miljö, så vi rekommenderar att du noggrant planerar implementeringarna av arbetsflöden.</p> </td>
  </tr>
 </tbody>
</table>

## Verktyg/HTML {#tooling-htl}

HTML Template Language (HTL) är ett nytt HTML-mallsystem som introducerades i AEM 6.0. Den ersätter JSP och ESP som standardmallsystem i AEM.

|  |  |  |
|---|---|---|
| HTML - översikt | [Översikt och syntax för HTML](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=sv-SE) | Det här dokumentet beskriver vad HTML är, hur du går till HTML, ett exempelprojekt, syntax, uttryck och programsatser |
| Använda API i java | [HTL Java Use-API](https://helpx.adobe.com/se/experience-manager/htl/using/use-api.html) | Med HTL Java Use-API:t kan en HTML-fil få åtkomst till hjälpmetoder i en anpassad Java-klass. |

>[!NOTE]
>
>Följande självstudiekurser i flera delar kan vara av intresse för den bästa metoden för att skapa ett nytt AEM-projekt, med information om kärnkomponenter, redigerbara mallar, klientbibliotek och komponentutveckling:
>[Komma igång med AEM Sites - WKND-självstudiekurs](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop.html)
