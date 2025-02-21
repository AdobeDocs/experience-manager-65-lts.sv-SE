---
title: AEM och tredjeparts Commerce Integration med Commerce integration framework
description: Företag kan behöva ytterligare e-handelslösningar från tredje part för att göra sin butik tillgänglig. Commerce integration framework (CIF) kan användas i sådana integreringsscenarier för att ansluta en e-handelslösning från tredje part till Adobe Experience Manager med hjälp av I/O Runtime.
thumbnail: cif-third-party-architecture.jpg
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# AEM och tredjeparts Commerce Integration med Commerce integration framework {#aem-third-party}

Integrationen av lösningar från andra företag än Adobe Commerce är ett vanligt scenario för CIF. Tredjepartslösningar med olika API:er och scheman kopplas samman via ett integreringslager.

## Arkitektur {#architecture}

Den övergripande arkitekturen är följande:

![Översikt över arkitektur för icke-Magento/tredje part för AEM](../assets//AEM_nonMagento_Architecture.png)

Syftet med integreringslagret är att mappa tredjeparts-API:er och scheman mot de Adobe Commerce GraphQL-API:er och -scheman som stöds utanför Experience Manager. Tack vare den här inkapslingen kan integreringslogiken och -systemen uppdateras utan att koden i Experience Manager ändras.

## Lösningskrav för integrering

När Experience Manager hämtar data on-demand krävs realtids-API:er för produktkatalogen.

>[!TIP]
>
>Om det inte finns några API:er i realtid bör en extern produktcache med API:er användas för integreringen. Exempel: [Magento öppen källkod](https://business.adobe.com/products/magento/open-source.html).

Du behöver inte implementera hela GraphQL-schemat, bara schemats objekt för att aktivera de önskade användningsfallen.

## Användningsfall i serverdelen

CIF ger Experience Manager tillgång till produktkataloger i realtid och verktyg för hantering av produktupplevelser. Denna smidiga integrering gör att författare kan komma åt e-handelsdata med inbäddade användargränssnitt när det behövs utan att lämna innehållskontexten.

Integreringen av programmeringsgränssnitt för produktkataloger krävs för att låsa upp dessa användningsfall.

## Vanliga användningsfall

[AEM CIF Core Components](https://github.com/adobe/aem-core-cif-components) hämtar och utbyter data via de Adobe Commerce-API:er som stöds av CIF. Om du vill återanvända komponenter måste respektive API implementeras.

Rekommendationen för prestandakrävande komponenter på klientsidan är att kommunicera direkt med tredjepartslösningen för att undvika latens.

## Utveckla en integrering {#develop-integration}

Adobe rekommenderar att du använder [Adobe I/O Runtime](https://developer.adobe.com/apis/experienceplatform/runtime.html) för integreringslagret. Det ingår i CIF-tillägget för tredje part. Eftersom det fungerar med en mikrotjänstliknande metod är det lämpligt att integrera flera lösningar på ett enkelt sätt.

Referensimplementeringen [av ](https://github.com/adobe/commerce-cif-graphql-integration-reference) är en bra utgångspunkt för att skapa integreringen med din e-handelslösning. Även om det har stöd för GraphQL kan det även integreras med andra typer av API som REST.

Det här integreringslagret behövs inte om ett lager från tredje part är tillgängligt (till exempel Mulesoft) eller om integreringen byggs ovanpå tredjepartslösningen.

## Fördefinierade anslutningar {#connectors}

Kopplingar är en bra början för projekt. De levereras med en e-handelslösningsspecifik anslutning och standard-API-mappning. Dessa kontakter byggs av tredje part och underhålls inte av Adobe. Kontakta respektive partner för mer information.

* [SAP Commerce](https://github.com/diconium/commerce-cif-graphql-integration-hybris), skapat av Diconium
* [Kommersiella verktyg](https://github.com/diconium/commerce-cif-graphql-integration-commercetool), skapade av Diconium

>[!TIP]
>
>Kopplingar hjälper till att snabba upp handelsintegreringen, men de är inte plug-in-play. E-handelslösningar för företag är mycket anpassade och kräver en anpassad integrering. Goda kunskaper om handelsplattformen, Adobe Commerce GraphQL scheman och Adobe I/O Runtime krävs.
