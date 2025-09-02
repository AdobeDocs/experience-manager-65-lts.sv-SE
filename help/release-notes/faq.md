---
title: Vanliga frågor och svar
description: Frågor och svar om AEM 6.5 LTS.
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
exl-id: d18c9dc3-fdcc-4558-b9b6-ecf1ce61048a
source-git-commit: 9f9da819550b93d7a06b151962bf41751ecbc8b3
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# AEM 6.5 LTS Frågor och svar (FAQ) {#faq}

Den här sidan är avsedd att besvara några vanliga frågor om AEM 6.5 LTS.

## Varför har Adobe släppt 6.5 LTS för AEM?

Adobe är fortfarande djupt engagerat i säkerheten och stabiliteten i de program som tillhandahålls. AEM 6.5 Långsiktig support lägger grunden för framtida uppdateringar av AEM 6.5. AEM 6.5 LTS har stöd för Oracle Java 17 och Java 21, och kommer att vara den AEM-gren som tar emot nya funktioner och innovationer från AEM.

## Jag är en lokal kund, vad händer om jag inte uppgraderar till AEM 6.5 LTS?

AEM 6.5 LTS innehåller viktiga säkerhets- och stabilitetsuppdateringar, inklusive stöd för Oracle Java 17 och Java 21. Adobe fortsätter att ge support för AEM 6.5 i minst två år, men vi rekommenderar att man börjar planera för en uppgradering till 6.5 LTS.

## Kommer mina befintliga anpassningar och integreringar att påverkas om jag uppgraderar till AEM 6.5 LTS?

AEM 6.5 LTS har som mål att bibehålla bakåtkompatibilitet, men vissa äldre funktioner och artefakter har tagits bort.
Det är viktigt att du granskar [versionsinformationen](/help/release-notes/release-notes.md#deprecated-and-removed-features) och använder [AEM Analyzer-verktyget](/help/sites-deploying/aem-analyzer.md) för att utvärdera effekten på anpassningar och integreringar.

## Hur kan jag säkerställa en smidig övergång till AEM 6.5 LTS?

För att övergången ska bli så smidig som möjligt bör du:

* Granska [versionsinformationen](/help/release-notes/release-notes.md) och dokumentationen noggrant.
* Använd [AEM Analyzer-verktyget](/help/sites-deploying/aem-analyzer.md) för att utvärdera uppgraderingens komplexitet.
* Planera och tilldela tillräckligt med tid och resurser för uppgraderingsprocessen.
* Använd Adobe support- och hjälpseminarier för vägledning och hjälp.

## Vad är AEM 6.5 LTS Service Pack?

AEM 6.5 LTS Service Packs är en kumulativ uppdatering som innehåller alla korrigeringar och förbättringar som gjorts i AEM 6.5 LTS sedan den första versionen. Vi rekommenderar att du installerar det senaste Service Pack-versionen för att försäkra dig om att din AEM-instans är uppdaterad med de senaste funktionerna och säkerhetsuppdateringarna.

## Jag är för närvarande på AEM 6.5, kan jag uppgradera direkt till AEM 6.5 LTS Service Pack utan att uppgradera till AEM 6.5 LTS GA-utgåvan?

Ja, du kan uppgradera direkt från AEM 6.5 till valfritt AEM 6.5 LTS Service Pack. Vi rekommenderar att du läser [versionsinformationen](/help/release-notes/release-notes.md) och [uppgraderar till AEM 6.5 LTS](/help/sites-deploying/upgrade.md).

## Jag har för närvarande AEM 6.5 LTS GA, behöver jag göra några kodändringar för att uppgradera till AEM 6.5 LTS Service Packs?

Nej, du behöver inte göra några kodändringar för att uppgradera från AEM 6.5 LTS till AEM 6.5 LTS Service Pack. Vi rekommenderar dock alltid att du granskar [versionsinformationen](/help/release-notes/release-notes.md) och testar dina anpassningar och integreringar i en staging-miljö innan du använder Service Pack på produktionsinstansen.

## Jag vill börja om från början med en ny AEM 6.5 LTS-installation. Kan jag börja direkt med AEM 6.5 LTS Service Pack?

Ja, du kan direkt konfigurera ett nytt AEM 6.5 LTS Service Pack utan att konfigurera AEM 6.5 LTS GA-bygget. Vi rekommenderar att du tittar i avsnittet [versionsinformation](/help/release-notes/release-notes.md) och [Anpassad fristående installation](/help/sites-deploying/custom-standalone-install.md) för mer information.
