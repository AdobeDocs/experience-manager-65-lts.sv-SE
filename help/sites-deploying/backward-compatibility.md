---
title: Bakåtkompatibilitet i AEM 6.5
description: Lär dig hur du kan göra dina program och konfigurationer kompatibla med Adobe Experience Manager (AEM) 6.5
feature: Upgrading
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: 96e44da3-da89-4671-a4fb-19ce1b9a38c4
source-git-commit: 10f0949f6317f060c38791cfe43156a56f8ebe47
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# Bakåtkompatibilitet i AEM 6.5{#backward-compatibility-in-aem}

## Ökning {#overview}

I Adobe Experience Manager (AEM) 6.5 har alla funktioner utvecklats med bakåtkompatibilitet i åtanke.

Normalt behöver inte kunder som kör AEM 6.3 ändra koden eller anpassningarna när de uppgraderar. För kunder som har AEM 6.1 och 6.2 finns det inga fler förändringar än du skulle ha fått vid en uppgradering till 6.3.

För undantag där funktioner inte kan hållas bakåtkompatibla kan bakåtinkompatibilitetsproblem för paket och innehåll minskas. Det gör du genom att installera ett kompatibilitetspaket för 6.4 (se hur du konfigurerar nedan för att få information om var du ska hämta). Det här kompatibilitetspaketet hjälper till att återställa kompatibilitet, vanligtvis för program som är kompatibla med AEM 6.4.

Med Kompatibilitetspaketet kan du köra AEM i kompatibilitetsläge och skjuta upp anpassad utveckling mot nya AEM-funktioner:

>[!NOTE]
>
>Kompatibilitetspaketet är bara en tillfällig lösning för att skjuta upp utvecklingen som krävs för att vara AEM 6.5-kompatibel. Adobe rekommenderar det endast som ett sista alternativ om du inte kan åtgärda kompatibilitetsproblem via utveckling direkt efter uppgraderingen. Adobe rekommenderar dessutom att du växlar till inbyggt läge och avinstallerar kompatibilitetspaketet när du har valt att fortsätta med 6.5-baserad anpassad utveckling och att du har tillgång till alla 6.5-funktioner.

![ase](assets/sase.png)

Kompatibilitetspaketet har två lägen: **Routing Enabled** och **Routing Disabled**.

Detta gör att AEM 6.5 kan köras i tre lägen:

**Inbyggt läge:**

Det inbyggda läget är till för kunder som vill använda alla de nya funktionerna i AEM 6.5 och är redo att göra en del av utvecklingen för att få anpassningarna att fungera med alla nya funktioner.

Det innebär att du måste justera programmet direkt efter uppgraderingen.

**Kompatibilitetsläge: Kompatibilitetspaket installerat med routning aktiverat**

Kompatibilitetsläget är avsett för kunder som har anpassat gränssnitt som inte är bakåtkompatibla. Detta gör att AEM kan köras i kompatibilitetsläge och skjuta upp den anpassade utvecklingen mot nya AEM-funktioner som inte är kompatibla med viss anpassad kod.

**Äldre läge: Kompatibilitetspaket installerat med routning inaktiverat**

Äldre läge är för kunder som har anpassade gränssnitt som baseras på äldre eller inaktuell kod från AEM och som har flyttats ut i kompatibilitetspaketet.

![form](assets/sapte.png)

## Konfigurera {#how-to-set-up}

**Kompatibilitetspaketet för AEM 6.4 för 6.5** kan installeras som ett paket med pakethanteraren. Du kan hämta [AEM 6.4-kompatibilitetspaketet för 6.5 från platsen för programvarudistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=compat*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=20&amp;package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Fcompatpack%2Faem-compat-cq65-to-cq64).

När Kompatibilitetspaketet har installerats kan routningen aktiveras eller inaktiveras med en växel i OSGI-konfigurationen enligt nedan:

![Jämför växlar](assets/compat-switches.png)

När Kompatibilitetspaketet har installerats och konfigurerats används funktionerna baserat på det kompatibilitetsläge som har valts.
