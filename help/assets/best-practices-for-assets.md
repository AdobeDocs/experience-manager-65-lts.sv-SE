---
title: Bästa praxis för  [!DNL Assets]
description: Förbättrar systemets stabilitet och prestanda under belastning genom att identifiera och följa bästa praxis som är beroende av din driftsättning och konfiguration.
contentOwner: AG
feature: Asset Management
role: Architect, Admin
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 0%

---

# Bästa praxis för [!DNL Assets] {#best-practices-for-assets}

[!DNL Adobe Experience Manager Assets] är en viktig del av att leverera digitala marknadsföringsupplevelser av hög kvalitet som bidrar till att uppnå affärsmålen genom att öka innehållets hastighet. Om du arbetar med ett stort antal resurser inom [!DNL Experience Manager Assets] eller regelbundet/regelbundet överför flera resurser, inklusive videor och dynamiska media, är det viktigt att optimera din digitala resurshantering för att systemet ska bli effektivt.

Beroende på hur du har placerat [!DNL Assets] för din organisation och vilka funktioner du använder för tillgångsintag, återgivningsgenerering och metadataextrahering, kan det vara bra att identifiera och följa vedertagna metoder inom olika områden och förbättra systemets stabilitet och prestanda vid inläsning.

När du har granskat följande riktlinjer får du de kunskaper och verktyg du behöver för att skapa och hantera ett resurshanteringssystem för företag som passar dina behov:

* [Prestandajusteringsguiden för Assets](/help/assets/performance-tuning-guidelines.md): Den här guiden innehåller en uppsättning metodtips som kan följas när som helst i implementeringen, även när du är klar, för att säkerställa att du får ut så mycket som möjligt av systemet.
* [Assets storleksguide](/help/assets/assets-sizing-guide.md): När du gör uppskattningar för en [!DNL Assets]-implementering är det viktigt att se till att det finns tillräckligt med resurser tillgängliga när det gäller resurslagring, CPU, minne, IO och nätverkets genomströmning. Om du ändrar storlek på många av dessa objekt måste du förstå hur många resurser som läses in i systemet. Den här guiden innehåller bästa praxis som hjälper dig att fastställa effektiva mått för beräkning av den infrastruktur och de resurser som krävs för att distribuera [!DNL Assets] och ett storleksbedömningsverktyg.
* Migreringsguiden för [Assets](/help/assets/assets-migration-guide.md): Om du vill migrera resurser från ditt äldre system till Assets finns det flera steg som ska övervägas för att effektivisera migreringsprocessen. Migreringsguiden innehåller metodtips om de åtgärder du utför för att överföra resurserna till [!DNL Experience Manager] på ett fasvis sätt. Detta inkluderar att lägga till metadata, generera återgivningar och aktivera resurserna för att publicera instanser.
* [Assets dokument om nätverksaspekter](/help/assets/assets-network-considerations.md): När du hanterar [!DNL Experience Manager]-distribution är det viktigt att förstå nätverkstopologin för att förstå nätverksprestanda, identifiera kontrollpunkter och beskriva den förväntade användarupplevelsen. I dokumentet med [!DNL Assets]-nätverksfrågor beskrivs nätverksaspekter när du utformar resursdistributionen.
* [Assets övervakningsguide](/help/assets/assets-monitoring-best-practices.md): När din [!DNL Experience Manager]-distribution har distribuerats bör du övervaka vissa uppgifter och systemet i allmänhet för att säkerställa systemintegriteten och effektiviteten för åtgärderna. Övervakningsguiden innehåller bästa praxis för övervakning av olika aspekter av ditt system.
* [Experience Manager bästa praxis för skrivbordsappar](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/introduction.html): [!DNL Experience Manager] Skrivbordsappen länkar din DAM-lösning (Digital Asset Management) till skrivbordet så att du kan öppna de filer som är tillgängliga i [!DNL Experience Manager] -webbanvändargränssnittet direkt på skrivbordet. Skrivbordsappens lättanvända arbetsflöde aktiveras med hjälp av nätverksdelningsteknik från operativsystemen på datorn. I den här guiden förklaras de viktigaste funktionerna och den rekommenderade användningen av datorprogrammet [!DNL Experience Manager].
* [Bästa praxis för integrering mellan Experience Manager och Creative Cloud](/help/assets/aem-cc-integration-best-practices.md): Du kan integrera din [!DNL Experience Manager]-distribution med [!DNL Creative Cloud] på flera sätt. Om du följer några metodtips för att effektivisera arbetsflödena för integrering och överföring av resurser blir du mycket effektivare. Den här guiden innehåller bästa praxis för integrering av [!DNL Assets] med [!DNL Adobe Creative Cloud].
