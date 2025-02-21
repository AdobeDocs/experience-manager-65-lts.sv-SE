---
title: AEM - Commerce Integration med Commerce integration framework - frågor och svar
description: AEM - Commerce Integration med Commerce integration framework - frågor och svar
solution: Experience Manager,Commerce
feature: Commerce Integration Framework
role: Admin, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---

# AEM - Commerce Integration med Commerce integration framework - frågor och svar

## 1. Används CIF GraphQL endast för e-handel eller är det tillgängligt för att fråga innehåll som skapats i AEMs JCR?

Adobe har antagit Adobe Commerce GraphQL API:er som sitt officiella e-handels-API för alla e-handelsrelaterade data. Därför använder AEM GraphQL för att utbyta affärsdata med Adobe Commerce och med alla e-handelsmotorer via I/O Runtime. Det här GraphQL-API:t är oberoende av AEM GraphQL-API för att få åtkomst till innehållsfragment.

## 2. Kan produktresurser (bilder) lagras och refereras från AEM via Adobe Commerce Admin? Hur kan resurser från Dynamic Media förbrukas?

Det finns ingen officiell integrering med AEM Assets - Adobe Commerce. Det finns en partnerkoppling tillgänglig på [Marketplace](https://marketplace.magento.com/partner/bounteous_ecomm).

Du kan också som tillfällig lösning lagra produktresurser (bilder) i AEM Assets, men du måste lagra resursens URL-adresser manuellt i Adobe Commerce. Dynamic Media är en del av AEM Assets och fungerar på samma sätt.

## 3. Spelar det någon roll var e-handelslösningen används? (Lokalt eller i molnet)

Nej, det spelar ingen roll var er handelslösning är installerad. CIF och AEM Store arbetar oavsett distributionsmodell. Om lösningen distribueras med den rekommenderade E2E-referensarkitekturen kan E2E-tester köras mot nyckeltal för prestanda som representerar en typisk företagsprofil. Denna process ger ytterligare information som kan användas som riktmärke.

## 4. Hur skapas katalogsidor eller produktsidor i AEM? Hur kvarstår de i AEM?

Katalogsidor och produktsidor skapas och cachelagras dynamiskt i AEM baserat på generiska katalog- och produktsidmallar. Inga produkt- eller katalogdata importeras och lagras i AEM.

## 5. När ni uppdaterar produktdata i er e-handelslösning, är det en realtidspress till AEM? Eller är det en gruppbearbetning?

CIF-tillägget som används med AEM gör att data kan flöda från e-handelslösningen till AEM on-demand. Det här arbetsflödet är alltså inte en push-process i realtid eller en batchprocess när det finns en uppdatering i e-handelslösningen.

## 6. Vilken katalogstorlek stöder AEM med CIF?

Stöd för katalogstorlek beror på några ytterligare aspekter som du måste tänka på. Hur stor är cachekvoten för katalogdata och sidor? Hur många samtidiga förfrågningar förväntar du dig under högtider? Hur skalbar är API:erna för era e-handelslösningar?

## 7. Hur fungerar PIM i detta ramverk?

PIM-data exponeras för AEM och kunder på begäran av GraphQL. Adobe rekommendation är att integrera PIM med e-handelsmotorn (Adobe Commerce eller andra) så att PIM-data kan hämtas från e-handelsmotorn.

## 8. Cachelagra även priser och andra data via Dispatcher. Blir det ofta en cachedomål?

Dynamiska data som pris eller lager cachelagras inte på Dispatcher. Dynamiska data hämtas på klientsidan med webbkomponenter direkt via GraphQL API:er. Endast statiska data (som produkt- eller kategoridata) cachelagras på Dispatcher. Om produktdata ändras behövs cacheogiltigförklaring.

## 9. Hur fungerar cacheminnet för AEM Dispatcher tillsammans med AEM och e-handel?

Adobe rekommenderar att du ställer in TTL-baserad cacheogiltigförklaring för sidor som cachelagrats på Dispatcher. För dynamisk information som pris eller aktie rekommenderar Adobe att du återger datumet på klientsidan. Mer information om ogiltigförklaring av TTL-baserad cache finns i [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html)

## 10. Finns det någon rekommendation om enhetlig sökning i AEM-innehåll med Commerce?

En referensimplementering av produktsökningar tillhandahålls, men ingen enhetlig sökning med innehåll. Den här funktionen är kundspecifik och bättre på projektspecifik nivå.

## 11. Hur fungerar Search med AEM och e-handel med CIF?

CIF innehåller komponenterna Sökfält och Sökresultat. Sökfältskomponenten skickar en GraphQL-begäran med söktermen till e-handelslösningen som sedan returnerar en produktlista som innehåller produktnamn, pris, SLUG och så vidare. Sökresultatkomponenten visar sedan sökresultaten i en gallerivy på en sökresultatsida som skapats i AEM. Sökfunktionen har stöd för grundläggande textsökning. Använd SLUG/url-tangenten för att skapa en referens till PDP.

## 12. Hur kan produktdata användas i MSM eller översättningar?

Produktdata är redan översatta i PIM eller Adobe Commerce. AEM - Adobe Commerce Integration har stöd för anslutning till flera Adobe Commerce butiker och butiksvyer. I en MSM-konfiguration är vanligtvis en AEM-webbplats länkad till en Adobe Commerce Store-vy.

## 13. Finns det något sätt att förbättra produktinformationen med kommersiell text? I så fall, var görs detta? I AEM eller i e-handelslösningen?

Adobe rekommenderar att man hanterar marknadsföringsrelaterade data och marknadsföringsrelaterat innehåll i AEM. Dekorera produktdata från er e-handelslösning med ytterligare attribut med hjälp av innehållsfragment eller skapa och länka Experience Fragments för ostrukturerat innehåll till era produkter.

## 14. Hur ser ett företag till att PCI-kompatibiliteten följs när det använder AEM för hela presentationslagret?

Adobe rekommenderar att du använder abstrakta betalningsmetoder. Om du gör det kommer webbläsarklienten att kommunicera direkt med betalgatewayleverantören så att Adobe inte håller eller godkänner kortinnehavarens datum eller handelslösningarna. Den här metoden kräver endast en nivå 3 PCI-kompatibilitet. Det finns dock ytterligare saker att tänka på som helt PCI-kompatibla, till exempel hur medarbetarna interagerar med systemet och data. Mer information om Adobe Commerce PCI-kompatibilitet finns i [PCI-kompatibilitet](https://business.adobe.com/products/magento/pci-compliance.html)

## 15. Om jag använder molnversionerna av AEM och Adobe Commerce, är denna gemensamma lösning PCI-kompatibel?

Ja, självutvärderingsformulär D och försäkran om överensstämmelse finns tillgängliga på begäran.

## 16. Hur begär jag en I/O Runtime-licens?

Mer information om hur du begär en utvärderingslicens för att använda I/O-miljön finns i [Få åtkomst](https://developer.adobe.com/runtime/docs/guides/overview/getting_access/).
