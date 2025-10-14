---
title: HTTP2-leverans av innehåll
description: Läs om hur HTTP/2 förbättrar kommunikationen mellan webbläsare och servrar, vilket ger snabbare överföring av information och minskar behovet av processorkraft.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
role: User, Admin
feature: Publishing,Configuration
solution: Experience Manager, Experience Manager Assets
exl-id: 7576e0e3-b05a-483b-9d38-316ddf0d5816
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 0%

---

# HTTP/2-leverans av innehåll {#http-delivery-of-content}

Adobe är mycket glada över att kunna meddela att HTTP/2-leverans av innehåll är tillgänglig, vilket ger bättre prestanda.

>[!NOTE]
>
>Den här funktionen kräver att du använder det färdiga CDN som medföljer Adobe Experience Manager Dynamic Media. Andra anpassade CDN stöds inte med den här funktionen.

## Vad är HTTP/2? {#what-is-http}

HTTP/2 förbättrar sättet som webbläsare och servrar kommunicerar på, vilket ger snabbare överföring av information samtidigt som man minskar behovet av processorkraft.

På följande webbplats beskrivs HTTP/2 och dess fördelar på ett kort och enkelt sätt:

[Vad du måste veta om HTTP/2](https://www.engadget.com/2015-02-24-what-you-need-to-know-about-http-2.html)

## Vilka är de viktigaste fördelarna med att gå över till HTTP/2 för innehållsleverans? {#what-are-the-key-benefits-of-moving-to-http-for-content-delivery}

Prestandaförbättringen kan variera mycket. Den baseras på många faktorer, till exempel webbplatsens kod, hur du använder Dynamic Media, konsumentens enhet, skärm och plats.

Adobe egen testning gav följande resultat:

* För bilder har svarstiden förbättrats med 7 %-28 % beroende på enhet och webbläsare. De mest betydande prestandavinster gjordes på iOS-enheter.
* För tittarna har lästiden förbättrats med 15 %.

I följande exempel visas skillnaden mellan HTTP/1 och HTTP/2-inläsning:

[https://http2.akamai.com/demo](https://http2.akamai.com/demo)

## Är jag berättigad att växla över till HTTP/2? {#am-i-eligible-to-switch-over-to-http}

Om du vill använda HTTP/2 måste du uppfylla följande krav:

* Använd säker HTTPS för multimedieförfrågningar.
* Använd det Adobe-paketerade CDN (content delivery network) som en del av din Dynamic Media-licens.
* Använd en dedikerad domän (non-company-h.assetsadobe#.com).

  Om du redan har en dedikerad domän kan du anmäla dig via Adobe kundsupport.

  Om du inte har någon dedikerad domän planerar Adobe att schemalägga din övergång till HTTP/2 under 2018.

## Hur aktiverar jag HTTP/2 för mitt Dynamic Media-konto? {#what-is-the-process-for-enabling-http-for-my-dynamic-media-account}

Du initierar begäran om att växla över till HTTP/2. Det görs inte automatiskt åt dig.

1. Om du vill gå över till HTTP/2 initierar du en begäran från Adobe kundsupport. Se [Öppna en supportanmälan](https://experienceleague.adobe.com/sv?support-solution=General&lang=en&support-tab=home#support).

   1. Ange följande information i din supportförfrågan:

      1. Primärt kontaktnamn, e-postadress, telefon.
      1. Alla domäner som ska överföras till HTTP/2.
      1. Kontrollera att du använder säkra HTTPS för multimedieförfrågningar.
      1. Kontrollera att du använder CDN via Adobe och inte hanteras med en direkt relation.
      1. Kontrollera att du använder en dedikerad domän. Om du använder Dynamic Media använder du en dedikerad domän.

   1. Kundsupport lägger till dig i HTTP/2-väntelistan baserat på i vilken ordning förfrågningarna skickades.
   1. När Adobe är redo att hantera din begäran kontaktar kundsupporten dig för att koordinera övergången och ange ett måldatum.
   1. Du meddelas när du är klar och kan verifiera att övergången har slutförts till HTTP2.

      Eftersom webbläsaren inte anger detta är det nödvändigt att hämta ett tillägg.

      För Firefox och Chrome finns det ett tillägg som heter HTTP/2 och SPDY Indicator. Webbläsarna stöder bara http/2 på ett säkert sätt, så det är nödvändigt att anropa en URL med https för att verifiera. Om det finns stöd för http/2 indikeras det av tillägget i form av en blå Flash-symbol och ett sidhuvud &quot;X-Firefox-Spdy&quot; : &quot;h2&quot;.

## När kan jag förvänta mig att gå över till HTTP/2? {#when-can-i-expect-to-be-transitioned-over-to-http}

Förfrågningar behandlas i den ordning som de tas emot av kundsupporten.

>[!NOTE]
>
>Det kan finnas lång ledtid eftersom övergången till HTTP/2 innebär att cacheminnet rensas. Därför kan bara ett fåtal kundövergångar hanteras åt gången.

## Vilka är riskerna med att gå över till HTTP/2? {#what-are-the-risks-with-moving-to-http}

Övergången till HTTP/2 tar bort ditt cacheminne vid CDN eftersom det handlar om att gå över till en ny CDN-konfiguration.

Det icke-cachelagrade innehållet träffar direkt på Adobe ursprungliga servrar tills cachen återskapas. Därför planerar Adobe att hantera ett fåtal kundövergångar i taget så att godtagbara prestanda upprätthålls när man tar begäranden från ursprungsläget.

## Hur kan du verifiera om en URL eller webbplats är aktiverad med HTTP/2? {#how-can-you-verify-whether-a-url-or-website-is-activated-with-http}

Eftersom webbläsaren inte anger detta är det nödvändigt att hämta ett tillägg.

För Firefox och Chrome finns det ett tillägg som heter HTTP/2 och SPDY Indicator. Webbläsarna stöder bara http/2 på ett säkert sätt, så det är nödvändigt att anropa en URL med https för att verifiera. Om http/2 stöds indikeras det av tillägget i form av en blå Flash-symbol och en rubrik `X-Firefox-Spdy` : `h2`.
