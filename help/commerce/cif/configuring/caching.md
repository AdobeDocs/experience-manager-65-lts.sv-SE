---
title: Cachelagring och prestanda
description: Lär dig mer om de olika konfigurationer som är tillgängliga för att aktivera GraphQL- och innehållscachning för att optimera prestanda för implementeringen av din e-handel.
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
exl-id: 7da2c607-b407-4e4b-bfba-bfaa78aff475
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---

# Cachelagring och prestanda {#caching}

## Cachelagring av komponenter och GraphQL-svar {#graphql}

AEM CIF Core Components har redan inbyggt stöd för cachelagring av GraphQL-svar för enskilda komponenter. Den här funktionen kan användas för att minska antalet GraphQL backend-anrop med stor faktor. Effektiv cachelagring kan utföras särskilt för upprepade frågor, som att hämta kategoriträdet för en navigeringskomponent eller hämta alla tillgängliga aggregations-/facets-värden som visas på produktsöknings- och kategorisidorna.

För AEM CIF Core Components konfigureras cachelagring på komponentbasis, så det är möjligt att styra om (och hur länge) GraphQL-begäranden/svar cachelagras för varje komponent. Det går också att definiera cachelagring för OSGi-tjänster med GraphQL-klienten.

### Konfiguration

När cachen har konfigurerats för en viss komponent börjar den lagra GraphQL-frågor och svar enligt varje cachekonfigurationspost. Storleken på cacheminnet och cachelagringstiden för varje post ska definieras på projektbasis, t.ex. beroende på hur ofta katalogdata kan ändras, hur viktigt det är att en komponent alltid visar de senaste tillgängliga data, och så vidare. Observera att det inte finns någon cacheogiltigförklaring, så var försiktig när du anger varaktigheten för cachen.

När du konfigurerar cachelagring för komponenter måste cachenamnet vara namnet på de **proxy**-komponenter som du definierar i ditt projekt.

Innan klienten skickar en GraphQL-begäran kontrollerar den om **exakt** samma GraphQL-begäran redan har cache-lagrats och returnerar eventuellt det cachelagrade svaret. För att matcha måste GraphQL-begäran matcha exakt, dvs. frågan, åtgärdsnamnet (om det finns något), variablerna (om sådana finns) MÅSTE alla vara lika med den cachelagrade begäran och alla anpassade HTTP-rubriker som kan anges MÅSTE också vara desamma. Adobe Commerce `Store`-huvudet MÅSTE till exempel matcha.

### Exempel

Vi rekommenderar att du konfigurerar viss cachelagring för söktjänsten som hämtar alla tillgängliga aggregations-/facets-värden som visas på produktsöknings- och kategorisidorna. Dessa värden ändras vanligtvis bara när ett nytt attribut till exempel läggs till i produkter, så längden för den här cacheposten kan vara &quot;stor&quot; om uppsättningen produktattribut inte ändras så ofta. Detta är projektspecifikt, men Adobe rekommenderar värden på några minuter i projektutvecklingsfaserna och några timmar i stabila produktionssystem.

Detta konfigureras vanligtvis med följande cachepost:

```
com.adobe.cq.commerce.core.search.services.SearchFilterService:true:10:3600
```

Ett annat exempel där cachningsfunktionen GraphQl bör användas är navigeringskomponenten eftersom den skickar samma GraphQL-fråga på alla sidor. I det här fallet är cacheposten vanligtvis inställd på:

```
venia/components/structure/navigation:true:10:600
```

När du överväger att använda referensarkivet [Venia](https://github.com/adobe/aem-cif-guides-venia). Observera användningen av komponentproxynamnet `venia/components/structure/navigation` och **inte** namnet på CIF-navigeringskomponenten (`core/cif/components/structure/navigation/v1/navigation`).

Cachelagring för andra komponenter bör definieras på projektbasis, vanligen i samordning med cachning som konfigurerats på Dispatcher-nivå. Kom ihåg att det inte finns någon aktiv ogiltigförklaring av dessa cacheminnen, så cachelagringstiden bör vara noggrann. Det finns inga värden för&quot;en storlek passar alla&quot; som matchar alla möjliga projekt och användningsfall. Se till att du definierar en cachelagringsstrategi på projektnivå som bäst motsvarar projektets krav.

## Dispatcher Caching {#dispatcher}

Cachelagring av AEM-sidor eller -fragment i [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) är bästa sättet för alla AEM-projekt. Oftast bygger det på invalideringstekniker som säkerställer att allt innehåll som ändras i AEM uppdateras korrekt i Dispatcher. Detta är en viktig egenskap i AEM Dispatcher cachningsstrategi.

Förutom rent AEM-hanterat innehåll kan en sida vanligtvis visa e-handelsdata som hämtas dynamiskt från Adobe Commerce via GraphQL. Även om sidstrukturen i sig kanske aldrig ändras kan e-handelsinnehållet ändras, till exempel om vissa produktdata (till exempel namn eller pris) ändras i Adobe Commerce.

För att CIF-sidor ska kunna cachas under en begränsad tid i AEM Dispatcher rekommenderar vi därför att du använder [tidsbaserad cacheinvalidering](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl) (kallas även TTL-baserad cachelagring) när du cachelagrar CIF-sidor i AEM Dispatcher. Den här funktionen kan konfigureras i AEM med det extra [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) -paketet.

Med TTL-baserad cachning definierar en utvecklare vanligtvis en eller flera cachningstider för utvalda AEM-sidor. På så sätt kan du vara säker på att CIF-sidor bara cachelagras i AEM Dispatcher upp till den konfigurerade längden och att innehållet uppdateras regelbundet.

>[!NOTE]
>
>Data på serversidan kan cachas av AEM Dispatcher, men vissa CIF-komponenter som `product`, `productlist` och `searchresults` hämtar vanligtvis alltid produktpriser på nytt i en webbläsarbegäran på klientsidan när sidan läses in. Detta säkerställer att viktigt dynamiskt innehåll alltid hämtas vid sidinläsning.

## Ytterligare resurser

- [Referensarkiv för Venedig](https://github.com/adobe/aem-cif-guides-venia)
- [GraphQL cachelagringskonfiguration](https://github.com/adobe/commerce-cif-graphql-client#caching)
- [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
