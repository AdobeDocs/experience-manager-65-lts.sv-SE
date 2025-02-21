---
title: Programvaruarkitektur
description: Lär dig några tips om hur du kan skapa program för Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 0%

---

# Programvaruarkitektur{#software-architecture}

## Designa för uppgraderingar {#design-for-upgrades}

När du utökar användningsklara beteenden är det viktigt att tänka på uppgraderingar. Använd alltid anpassningar i katalogen /apps och lägg antingen över motsvarande noder i katalogen /libs eller använd sling:resourceSuperType för att utöka beteendet utanför rutan. Vissa ändringar kan behövas för att stödja en ny version av AEM, men den nya versionen bör inte skriva över dina anpassningar om den här metoden följs.

### Återanvänd mallar och komponenter när det är möjligt {#reuse-template-and-components-when-possible}

På så sätt kan webbplatsen få ett mer konsekvent utseende och känsla och förenkla kodunderhållet. När en ny mall behövs måste du se till att utöka från en delad basmall så att globala krav som klientlib-inkludering kan kodas på ett ställe. När en ny komponent behövs letar du efter möjligheter att utöka från en befintlig komponent.

### Designa mallar {#design-template-designs}

Genom att definiera vilka komponenter som kan inkluderas i varje parsys på sidan kan du styra enhetligheten i webbplatsens utseende och känsla. Genom att begränsa åtkomsten till designen på sidor kan superförfattare ändra de tillåtna komponenterna per sida utan att behöva göra något från utvecklaren samtidigt som de säkerställer att de andra författarna följer företagets standarder.

### Utveckla en SOLID-arkitektur {#develop-a-solid-architecture}

SOLID är en förkortning som beskriver fem arkitektoniska principer som bör följas:

* **S** En enda ansvarsprincip - varje modul, klass, metod och så vidare, ska bara ha ett ansvar.
* **O** pen/Closed Principle - modulerna ska vara öppna för tillägg och stängda för ändring.
* **L** iskov-ersättningsprincip - typer ska kunna ersättas av sina undertyper.
* **I** Interface Segregation Principle - ingen klient ska tvingas att vara beroende av metoder som den inte använder.
* **D** Princip för beroendekonvertering - moduler på hög nivå bör inte vara beroende av moduler på låg nivå. Båda bör vara beroende av abstraktioner. Abstraktioner bör inte vara beroende av detaljer. Detaljer bör vara beroende av abstraktioner.

Att sträva efter att följa dessa fem principer bör leda till ett system som är strikt åtskilt från oron.

>[!TIP]
>
>SOLID är ett vanligt koncept för objektorienterad programmering och varje element diskuteras ofta i branschlitteratur.
>
>Den här informationen är bara en kort sammanfattning som presenteras för att vara medveten om detta och du uppmuntras att bekanta dig med dessa koncept på ett mer djupgående sätt.

### Följ tillförlitlighetsprincipen {#follow-the-robustness-principle}

I Robustness Principle står det att du bör vara försiktig i det du skickar, men vara liberal i det du accepterar. Med andra ord, när du skickar meddelanden till en tredje part bör du helt och hållet följa specifikationerna. När du får meddelanden från en tredje part bör du dock acceptera meddelanden som inte uppfyller kraven så länge som meddelandets innebörd är tydlig.

### Implementera toppar i sina egna moduler {#implement-spikes-in-their-own-modules}

Spikes och testkod ingår i alla Agile-programimplementeringar. Du bör dock se till att de inte tar sig in i produktionskodbasen utan lämplig tillsynsnivå. Därför rekommenderar vi att taggar skapas i en egen modul.

### Implementera skript för datamigrering i sin egen modul {#implement-data-migration-scripts-in-their-own-module}

Skript för datamigrering körs bara en gång när en webbplats startas första gången. När webbplatsen publiceras blir därför skripten inaktiva. För att vara säker på att du inte skapar implementeringskod som är beroende av migreringsskripten, bör de implementeras i en egen modul. På så sätt kan vi ta bort och kassera den här koden direkt efter start, vilket eliminerar den döda koden från systemet.

### Följ publicerade Maven-konventioner i POM-filer {#follow-published-maven-conventions-in-pom-files}

Apache har publicerat formatkonventioner på [https://maven.apache.org/developers/conventions/code.html](https://maven.apache.org/developers/conventions/code.html). Det är bäst att följa dessa konventioner eftersom det gör det enklare för nya resurser att komma igång snabbt.
