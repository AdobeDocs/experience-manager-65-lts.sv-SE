---
title: AEM Brackets Extension
description: Lär dig hur du använder Adobe Experience Manager-tillägget för Brackets.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 1%

---

# AEM Brackets Extension{#aem-brackets-extension}

## Ökning {#overview}

Med AEM Brackets Extension får du ett smidigt arbetsflöde när du redigerar AEM-komponenter och klientbibliotek. Du använder kraften i [Brackets](https://brackets.io/) -kodredigeraren som ger åtkomst till Photoshop-filer och -lager inifrån kodredigeraren. Den enkla synkronisering som tillägget ger (ingen Maven eller filvalv krävs) ökar utvecklarens effektivitet och hjälper även gränssnittsutvecklare med begränsade AEM-kunskaper att delta i projekt. Det här tillägget har även stöd för [HTML Template Language (HTL)](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html), vilket gör JSP-komponenten mer komplex och gör komponentutvecklingen enklare och säkrare.

![chlimage_1-53](assets/chlimage_1-53a.png)

### Funktioner {#features}

Huvudfunktionerna i AEM Brackets Extension är:

* Automatisk synkronisering av ändrade filer till AEM-utvecklingsinstansen.
* Manuell dubbelriktad synkronisering av filer och mappar.
* Fullständig innehållspaketsynkronisering av projektet.
* HTML-kodkomplettering för uttryck och `data-sly-*` blocksatser.

Brackets har dessutom många användbara funktioner för AEM teckensnittsutvecklare:

* Stöd för Photoshop-filer för att extrahera information från en PSD-fil, t.ex. lager, mått, färger, teckensnitt, texter och så vidare.
* Kodtips från PSD om du enkelt vill återanvända den extraherade informationen i koden.
* Stöd för CSS-preprocessorer, som LESS och SCSS.
* Och hundratals tillägg som täcker mer specifika behov.

## Installation {#installation}

### Parenteser {#brackets}

AEM Brackets Extension stöder Brackets version 1.0 eller senare.

Hämta den senaste Brackets-versionen från [brackets.io](https://brackets.io/).

### Tillägget {#the-extension}

Så här installerar du tillägget:

1. Öppna Brackets. I menyn **Arkiv** väljer du **Extension Manager...**
1. Ange **AEM** i sökfältet och sök efter **AEM Brackets Extension**.

   ![chlimage_1-54](assets/chlimage_1-54a.png)

1. Klicka på **Installera**.
1. Stäng dialogrutan och Extension Manager när installationen är klar.

## Komma igång {#getting-started}

### Innehållspaketprojektet {#the-content-package-project}

När tillägget har installerats kan du börja utveckla AEM-komponenter genom att öppna en innehållspaketmapp från filsystemet med Brackets.

Projektet måste innehålla minst

1. en `jcr_root`-mapp (till exempel `myproject/jcr_root`)

1. en `filter.xml`-fil (till exempel `myproject/META-INF/vault/filter.xml`). Mer information om strukturen för filen `filter.xml` finns i [Workspace-filterdefinitionen](https://jackrabbit.apache.org/filevault/filter.html).

Välj **Öppna mapp..** på menyn **Arkiv** i hakparenteser och välj antingen mappen `jcr_root` eller den överordnade projektmappen.

>[!NOTE]
>
>Om du inte har ett eget projekt med ett innehållspaket kan du prova [HTL TodoMVC-exemplet](https://github.com/Adobe-Marketing-Cloud/aem-sightly-sample-todomvc). På GitHub klickar du på **Hämta ZIP**, extraherar filerna lokalt och öppnar mappen `jcr_root` i Brackets enligt instruktionerna ovan. Följ sedan stegen nedan för att konfigurera **projektinställningarna** och överföra slutligen hela paketet till din AEM-utvecklingsinstans genom att göra ett **Exportera innehållspaket** enligt anvisningarna längre ned i avsnittet Fullständig synkronisering av innehållspaket.
>
>Efter dessa steg bör du kunna komma åt URL:en `/content/todo.html` på din AEM-utvecklingsinstans och du kan börja göra ändringar i koden i hakparenteser och se hur ändringarna synkroniserades direkt till AEM-servern när du har gjort en uppdatering i webbläsaren.

### Projektinställningar {#project-settings}

Om du vill synkronisera ditt innehåll till och från en AEM-utvecklingsinstans måste du definiera dina projektinställningar. Detta kan du göra genom att gå till menyn **AEM** och välja **Projektinställningar..**

![chlimage_1-55](assets/chlimage_1-55a.png)

Med projektinställningarna kan du definiera följande:

1. Server-URL (till exempel `http://localhost:4502`)
1. Om servrar som inte har ett giltigt HTTPS-certifikat ska tolereras (håll inte markerat om det inte krävs)
1. Användarnamnet som används för att synkronisera innehåll (till exempel `admin`)
1. Användarens lösenord (till exempel `admin`)

## Synkroniserar innehåll {#synchronizing-content}

AEM Brackets Extension tillhandahåller följande typer av innehållssynkronisering för filer och mappar som tillåts av filtreringsreglerna som definieras i `filter.xml`:

### Automatisk synkronisering av ändrade filer {#automated-synchronization-of-changed-files}

Detta synkroniserar endast ändringar från Brackets till AEM-instansen, men inte tvärtom.

### Manuell dubbelriktad synkronisering {#manual-bidirectional-synchronization}

I projektutforskaren öppnar du snabbmenyn genom att högerklicka på en fil eller mapp och alternativen **Exportera till server** eller **Importera från server** är tillgängliga.

![chlimage_1-56](assets/chlimage_1-56a.png)

>[!NOTE]
>
>Om den markerade posten ligger utanför mappen `jcr_root` inaktiveras sammanhangsbaserade menyposter för **Exportera till server** och **Importera från server**.

### Fullständig synkronisering av innehållspaket {#full-content-package-synchronization}

På menyn **AEM** kan du synkronisera hela projektet med servern med alternativen **Exportera innehållspaket** eller **Importera innehållspaket** .

![chlimage_1-57](assets/chlimage_1-57a.png)

### Synkroniseringsstatus {#synchronization-status}

I AEM Brackets Extension finns en meddelandeikon i verktygsfältet till höger om Brackets-fönstret, som anger status för den senaste synkroniseringen:

* grön - alla filer har synkroniserats
* blue - en synkroniseringsåtgärd pågår
* gult - vissa av filerna synkroniserades inte
* röd - ingen av filerna synkroniserades

Om du klickar på meddelandeikonen öppnas dialogrutan Synkroniseringsstatus med en lista över all status för varje synkroniserad fil.

![chlimage_1-58](assets/chlimage_1-58a.png)

>[!NOTE]
>
>Endast innehåll som markerats som inkluderat av filtreringsreglerna från `filter.xml` synkroniseras, oavsett vilken synkroniseringsmetod som används.
>
>Dessutom stöds `.vltignore`-filer för att utesluta innehåll från synkronisering till och från databasen.

## Redigera HTML-kod {#editing-htl-code}

AEM Brackets Extension innehåller även vissa funktioner för automatisk komplettering som underlättar skrivandet av HTML-attribut och -uttryck.

### Automatisk komplettering av attribut {#attribute-auto-completion}

1. I ett HTML-attribut skriver du `sly`. Attributet har fyllts i automatiskt till `data-sly-`.
1. Markera HTL-attributet i listrutan.

### Automatisk komplettering av uttryck {#expression-auto-completion}

Inom ett uttryck `${}` slutförs gemensamma variabelnamn automatiskt.

## Mer information {#more-information}

AEM Brackets Extension är ett öppen källkodsprojekt som hanteras av GitHub av [Adobe Marketing Cloud](https://github.com/Adobe-Marketing-Cloud) -organisationen, under Apache-licensen, version 2.0:

* Koddatabas: [https://github.com/Adobe-Marketing-Cloud/aem-sightly-brackets-extension](https://github.com/Adobe-Marketing-Cloud/aem-sightly-brackets-extension)
* Apache License, version 2.0: [https://www.apache.org/licenses/LICENSE-2.0.html](https://www.apache.org/licenses/LICENSE-2.0.html)

Kodredigeraren Brackets är också ett öppen källkodsprojekt som hanteras av GitHub av [Adobe Systems Incorporated](https://github.com/adobe) -organisationen:

* Koddatabas: [https://github.com/adobe/brackets](https://github.com/adobe/brackets)

Du kan bidra!
