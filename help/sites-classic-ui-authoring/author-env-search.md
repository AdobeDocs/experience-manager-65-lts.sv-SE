---
title: Sök
description: I AEM redigeringsmiljö finns olika metoder för att söka efter innehåll, beroende på resurstypen.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: 2e4f4444-5005-4b46-8bbc-eb935b3a19a5
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 0%

---

# Sök{#searching}

I AEM redigeringsmiljö finns olika metoder för att söka efter innehåll, beroende på resurstypen.

>[!NOTE]
>
>Utanför författarmiljön finns det även andra funktioner som kan användas för sökning, till exempel [Query Builder](/help/sites-developing/querybuilder-api.md) och [CRXDE Lite](/help/sites-developing/developing-with-crxde-lite.md).

## Grunderna i sökning {#search-basics}

Om du vill komma åt sökpanelen klickar du på fliken **Sök** längst upp i den vänstra rutan i rätt konsol.

![chlimage_1-101](assets/chlimage_1-101.png)

Med sökpanelen kan du söka på alla webbplatssidor. Den innehåller fält och widgetar för följande:

* **Fulltext**: Sök efter den angivna texten
* **Ändrad efter/före**: Sök bara på de sidor som har ändrats mellan de angivna datumen
* **Mall**: Sök bara på de sidor som är baserade på den angivna mallen
* **Taggar**: Sök endast på sidor med de angivna taggarna

>[!NOTE]
>
>När din instans har konfigurerats för [Lucene-sökning](/help/sites-deploying/queries-and-indexing.md) kan du använda följande i **Fulltext**:
>
>* [Jokertecken](https://lucene.apache.org/core/5_3_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Wildcard_Searches)
>* [Booleska operatorer](https://lucene.apache.org/core/5_3_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Boolean_operators)
>
>* [Reguljära uttryck](https://lucene.apache.org/core/5_3_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Regexp_Searches)
>* [Fältgruppering](https://lucene.apache.org/core/5_3_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Field_Grouping)
>* [Boosting](https://lucene.apache.org/core/5_3_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Boosting_a_Term)
>

Utför sökningen genom att klicka på **Sök** längst ned i rutan. Klicka på **Återställ** för att rensa sökvillkoren.

## Filter {#filter}

På olika platser kan ett filter ställas in (och rensas) för att detaljgranska och förfina vyn:

![chlimage_1-102](assets/chlimage_1-102.png)

## Sök och ersätt {#find-and-replace}

I konsolen **Webbplatser** kan du med menyalternativet **Sök och ersätt** söka efter och ersätta flera förekomster av en sträng i ett avsnitt på webbplatsen.

1. Markera rotsidan, eller mappen, där du vill att åtgärden Sök och ersätt ska utföras.
1. Välj **Verktyg** och sedan **Sök och ersätt**:

   ![screen_shot_2012-02-15at120346pm](assets/screen_shot_2012-02-15at120346pm.png)

1. Dialogrutan **Sök och ersätt** gör följande:

   * bekräftar rotsökvägen där sökåtgärden ska starta
   * definierar den term som ska hittas
   * definierar termen som ska ersätta den
   * anger om sökningen ska vara skiftlägeskänslig
   * anger om endast hela ord ska hittas (i annat fall hittas även delsträngar)

   Klicka på **Förhandsgranska** för att visa var termen har hittats. Du kan markera/rensa specifika förekomster som ska ersättas:

   ![screen_shot_2012-02-15at120719pm](assets/screen_shot_2012-02-15at120719pm.png)

1. Klicka på **Ersätt** om du vill ersätta alla instanser. Du ombeds bekräfta åtgärden.

Standardomfånget för sök- och ersättningsservern omfattar följande egenskaper:

* `jcr:title`
* `jcr:description`
* `jcr:text`
* `text`

Omfånget kan ändras med Apache Felix Web Management Console (till exempel på `https://localhost:4502/system/console/configMgr`). Välj `CQ WCM Find Replace Servlet (com.day.cq.wcm.core.impl.servlets.FindReplaceServlet)` och konfigurera omfånget efter behov.

>[!NOTE]
>
>I en standardinstallation av AEM använder Sök och ersätt Lucene för sökfunktionen.
>
>Lucene indexerar strängegenskaper på upp till 16 kB. Strängar som överskrider detta söks inte igenom.
