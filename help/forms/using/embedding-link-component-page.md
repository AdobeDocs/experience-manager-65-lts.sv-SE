---
title: Bädda in länkkomponent på en sida
description: Du kan använda länkkomponenten för att länka ett adaptivt dokument eller ett adaptivt formulär från en sida.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Forms Portal
role: Admin, User, Developer
exl-id: a6ae1633-63a8-4364-b298-bc569459a136
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Bädda in länkkomponent på en sida{#embedding-link-component-in-a-page}

## Förutsättningar {#prerequisites}

Länkkomponenten är medlem i kategorin Dokumenttjänster. Se till att kategorin Document Services visas i AEM komponentwebbläsare. Om kategorin inte finns med i listan följer du stegen i [Aktivera komponenter i formulärportalen](/help/forms/using/enabling-forms-portal-components.md).

## Länkkomponent {#link-component}

Med länkkomponenten kan formulärportalförfattare skapa en länk till ett anpassat formulär var som helst på en sida. Komponenten Link är tillgänglig i avsnittet Document Services i komponentwebbläsaren.

Utför följande steg för att lägga till en länkkomponent på sidan:

1. Dra **Link**-komponenten på sidan. Markera komponenten och välj ![cmpr](assets/cmppr.png). Dialogrutan Redigera länkkomponent öppnas.

   ![edit-link-component](assets/edit-link-component.png)

1. Ange följande på fliken **Visning**:

   * **Länkbeskrivning**: Länka text eller bildtext för länken.
   * **Länkverktygstips**: Länkens verktygstips.
   * **Layoutmall**: Mall för layouten för länkkomponenten.

1. Öppna fliken **Resursinformation** och ange resurstypen. En resurs kan vara ett **formulär**. Beroende på vilken typ av resurs som valts visas alternativen nedan:

   * **Resurssökväg**: Databassökväg där resursen lagras.

   * **Återgivningstyp**: Återgivningsformatet PDF, HTML eller Auto. Återgivningstypen Automatisk identifierar användarmiljön och återger formuläret som HTML eller PDF. Om formuläret till exempel nås från en mobil enhet återges formuläret i HTML av återgivningstypen Auto.
   * **Skicka URL:** till den server där formulärdata skickas.
   * **HTML-profil**: Profil för att återge formuläret som HTML.
   * **PDF-profil**: Profil för att återge formuläret som PDF-dokument.

1. Öppna fliken **Avancerat**. Du kan ange ytterligare parametrar i nyckelvärdepar-formatet. När användaren klickar på länken skickas dessa ytterligare parametrar tillsammans med formuläret.

   Välj **Klar** om du vill spara konfigurationen.

## Bästa tillvägagångssätt för att använda länkkomponenten {#best-practices-for-using-link-component-br}

* Se till att du väljer PDF som återgivningstyp om den sökväg som anges i Formulärsökväg pekar på ett dokument som har PDF som tillåtet återgivningsformat.
* Skicka-URL för ett formulär kan anges på flera ställen och prioritetsordningen är följande:

   1. Skicka-URL som är inbäddad i formuläret (med Skicka-knappen) har högsta prioritet.
   1. Skicka-URL som nämns i Forms Manager har medelhög prioritet.
   1. Överförings-URL som anges i formulärportalen har lägst prioritet.
