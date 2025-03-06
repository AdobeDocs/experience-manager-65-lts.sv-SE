---
title: Migrera från Dynamic Media - hybrid-läge till Dynamic Media - S7-läge
description: Lär dig hur du migrerar en instans av Dynamic Media - hybrid-läge till Dynamic Media - S7-läge
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
docset: aem65
role: User, Admin
feature: Scene7 Mode,Hybrid Mode
solution: Experience Manager, Experience Manager Assets
exl-id: 7d21d993-f7a3-4c12-aa4d-03057c8f29fc
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 2%

---

# Om att gå från Dynamic Media-Hybrid till Dynamic Media-Scene7 {#about-migrating}

Dynamic Media-Hybrid är en äldre version av Dynamic Media-integrering med Adobe Experience Manager. Hybridversionen introducerades först i Adobe Experience Manager 6.1. Adobe har fortfarande stöd för hybridläge, men det är inte det rekommenderade läget. Dynamic Media-Scene7 är det rekommenderade läget. Hybridläget stöder inte heller nya funktioner som Smart beskärning och panoramabilder, medan Dynamic Media-Scene7 stöder dem.

Ytterligare viktiga skillnader mellan Dynamic Media-Hybrid och Dynamic Media-Scene7 är följande:

* URL-adressernas struktur.
* Inmatning av videor.
* Skapa och lagra bildåtergivningar.
* Molnkonfiguration och autentiseringsuppgifter (etablering).

Det finns två alternativ när du går från Dynamic Media-Hybrid till Dynamic Media-Scene7. Det första alternativet är att tillhandahålla en ny instans av Dynamic Media-Scene7 på Experience Manager. Det andra alternativet är att migrera din befintliga instans av Dynamic Media-Hybrid till Dynamic Media-Scene7. Det här alternativet visar tabellformuläret nedanför de steg och överväganden som du kan göra under flyttningen.

>[!IMPORTANT]
>
>Adobe rekommenderar att du inte migrerar en Dynamic Media-Hybrid-implementering till Dynamic Media-Scene7 för direktproduktion.

## Alternativ 1 - Skapa en ny instans av Dynamic Media-Scene7 på Experience Manager {#provision-new-dms7}

Överväg att börja om från början med en ny, etablerad instans av Dynamic Media-Scene7 på Adobe Experience Manager. Förutom att man får in och bearbeta mediefiler via Dynamic Media Cloud Service rekommenderar vi en granskning av användningen av mediefiler, arbetsflöden och komponenter från Adobe. Du kan ofta ersätta anpassade komponenter och arbetsflöden med nya färdiga funktioner.

## Alternativ 2 - Migrera din befintliga instans av Dynamic Media-Hybrid till Dynamic Media-Scene7 {#process-for-migrating}

| Steg | Uppgift | Överväganden |
|---|---|---|
| 1 | Clone Dynamic Media-Hybrid Author instance. | Behåll din befintliga instans av Dynamic Media-Hybrid Author i reserv tills de återstående stegen i migreringsprocessen har slutförts. |
| 2 | Starta klonad författarinstans i läget Dynamic Media-Scene7. |  |
| 3 | Konfigurera Dynamic Media med autentiseringsuppgifter för Dynamic Media-Scene7 i Adobe Experience Manager Cloud Services. | Adobe måste godkänna etableringen av Dynamic Media-Scene7. Det innebär att ni har samtidiga miljöer för Dynamic MediaM-Hybrid och Dynamic Media-Scene7 som stöds av Adobe, men endast under en begränsad tid. |
| 4 | Skapa migreringspaket så att du kan importera resurser efter behov.<br>Ta bort de lokala PTIFF-filer som skapades vid första intaget till Dynamic Media-Hybrid. | Om alla resurser finns tillgängliga i din Dynamic Media-Hybrid-instans finns det redan en klon som innehåller alla resurser. Därför behövs inget paket. |
| 5 | Kör resursuppdateringsarbetsflödet så att du kan synkronisera resurser till Dynamic Media Cloud Service. | Adobe rekommenderar att du utför uppdateringsarbetsflödet i grupper för att tillåta komprimering. |
| 6 | Migrera inställningar för visningsprogram, bilder och video. |  |
| 7 | Gå igenom varje resurs som refereras till av Web Content Management och uppdatera deras associerade URL:er. |  |
| 8 | Migrera anpassade arbetsflöden som du vill ha stöd för det nya läget Dynamic Media-Scene7 (manuella uppdateringar). |  |
| 9 | Verifiera överföring och konfiguration av Web Content Management. |  |
| 10 | Efter verifieringen får du ett godkännande för att inaktivera Dynamic Media-Hybrid Author (behåll som reserv). |  |
| 11 | Ta bort Dynamic Media-Hybrid Author-instansen efter ungefär en månads lyckad användning av Dynamic Media-Scene7. |  |
