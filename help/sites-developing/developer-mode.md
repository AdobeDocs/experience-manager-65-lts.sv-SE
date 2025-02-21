---
title: Utvecklarläge
description: I utvecklarläget öppnas en sidopanel med flera flikar som ger utvecklaren information om den aktuella sidan.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: components
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '683'
ht-degree: 2%

---

# Utvecklarläge{#developer-mode}

När du redigerar sidor i Adobe Experience Manager (AEM) finns flera [lägen](/help/sites-authoring/author-environment-tools.md#modestouchoptimizedui) tillgängliga, bland annat i utvecklarläget. Då öppnas en sidopanel med flera flikar som ger utvecklaren information om den aktuella sidan. De tre flikarna är:

* **[Komponenter](#components)** för att visa struktur- och prestandainformation.
* **[Testar](#tests)** för att köra tester och analysera resultaten.
* **[Fel](#errors)** om du vill se eventuella problem.

Detta hjälper en utvecklare att:

* Upptäck: vilka sidor som är sammansatta av.
* Felsök: vad som händer var och när, vilket i sin tur hjälper till att lösa problem.
* Test: fungerar programmet som förväntat.

>[!CAUTION]
>
>Utvecklarläge:
>
>* Är bara tillgängligt i det beröringsaktiverade användargränssnittet (vid redigering av sidor).
>* Är inte tillgängligt på mobila enheter eller små fönster på skrivbordet (på grund av utrymmesbegränsningar).
>
>   * Detta inträffar när bredden är mindre än 1024px.
>* Är bara tillgänglig för användare som är medlemmar i gruppen `administrators`.

>[!CAUTION]
>
>Utvecklarläget är bara tillgängligt på en standardförfattarinstans som inte använder körläget nosampling content.
>
>Om det behövs kan det konfigureras för användning:
>
>* på en författarinstans med nosamplsinnehållets körningsläge
>* en publiceringsinstans
>
>Det bör inaktiveras igen efter användning.

>[!NOTE]
>
>Se
>
>* Kunskapsbasartikeln [Felsökning av AEM TouchUI-problem](https://helpx.adobe.com/experience-manager/kb/troubleshooting-aem-touchui-issues.html) innehåller ytterligare tips och verktyg.
>* AEM Gems-session om [AEM 6.0 Developer Mode](https://experienceleague.adobe.com/docs/events/experience-manager-gems-recordings/gems2014/aem-developer-mode.html).
>

## Öppnar utvecklarläge {#opening-developer-mode}

Utvecklarläget implementeras som en sidopanel i sidredigeraren. Om du vill öppna panelen väljer du **Utvecklare** i lägesväljaren i verktygsfältet i sidredigeraren:

![chlimage_1-11](assets/chlimage_1-11.png)

Panelen är uppdelad i två flikar:

* **[Komponenter](/help/sites-developing/developer-mode.md#components)** - Detta visar ett komponentträd, som liknar [innehållsträdet](/help/sites-authoring/author-environment-tools.md#content-tree) för författare

* **[Fel](/help/sites-developing/developer-mode.md#errors)** - När problem uppstår visas information för varje komponent.

### Komponenter {#components}

![chlimage_1-12](assets/chlimage_1-12.png)

Detta visar ett komponentträd som:

* Skapar konturer för kedjan med komponenter och mallar som återges på sidan (SLY, JSP osv.). Trädet kan expanderas för att visa kontext i hierarkin.
* Visar datortiden på serversidan för återgivning av komponenten.
* Gör att du kan expandera trädet och välja specifika komponenter i trädet. Markeringen ger åtkomst till komponentinformation, till exempel:

   * Databassökväg
   * Länkar till skript (används i CRXDE Lite)

* De valda komponenterna (i innehållsflödet, vilket anges med en blå ram) markeras i innehållsträdet (och omvänt).

Detta kan hjälpa till att:

* Fastställ och jämför återgivningstiden per komponent.
* Se och förstå hierarkin.
* Förstå och förbättra sidinläsningstiden genom att hitta långsamma komponenter.

Varje komponentpost kan visa (till exempel:

![chlimage_1-13](assets/chlimage_1-13.png)

* **Visa detaljer**: en länk till en lista som visar:

   * alla komponentskript som används för att återge komponenten.
   * databasens innehållssökväg för den här specifika komponenten.

  ![chlimage_1-14](assets/chlimage_1-14.png)

* **Redigera skript**: en länk som:

   * öppnar komponentskriptet i CRXDE Lite.

* När du expanderar en komponentpost (pilhuvud) kan du även visa:

   * Hierarkin i den markerade komponenten.
   * Återgivningstider för den markerade komponenten separat, alla enskilda kapslade komponenter i den och den kombinerade summan.

  ![chlimage_1-15](assets/chlimage_1-15.png)

>[!CAUTION]
>
>Vissa länkar pekar på skript under `/libs`. De här är bara för referens, men du **får inte** redigera något under `/libs` eftersom ändringar du gör kan gå förlorade. Det beror på att den här grenen kan ändras när du uppgraderar eller installerar en snabbkorrigering eller ett funktionspaket. Gör de ändringar du behöver under `/apps`. Se [Övertäckningar och åsidosättningar](/help/sites-developing/overlays.md).

### Fel {#errors}

![chlimage_1-16](assets/chlimage_1-16.png)

Förhoppningsvis kommer fliken **Fel** alltid att vara tom (som ovan), men när problem uppstår visas följande information för varje komponent:

* En varning om komponenten skriver en post i felloggen, tillsammans med information om felet och direktlänkar till rätt kod i CRXDE Lite.
* En varning om komponenten öppnar en administratörssession.

I en situation där en odefinierad metod anropas visas det resulterande felet på fliken **Fel**:

![chlimage_1-17](assets/chlimage_1-17.png)

Komponentposten i trädet på fliken Komponenter markeras också med en indikator när ett fel inträffar.

### Test {#tests}

>[!CAUTION]
>
>I AEM 6.2 återimplementerades testfunktionerna i utvecklarläget som ett fristående verktygsprogram.
>
>Mer information finns i [Testa användargränssnittet](/help/sites-developing/hobbes.md).
