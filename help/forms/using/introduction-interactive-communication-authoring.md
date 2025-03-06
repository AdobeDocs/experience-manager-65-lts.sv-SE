---
title: Introduktion till gränssnittet för utveckling av interaktiv kommunikation
description: En introduktion till olika element i användargränssnittet som du kan använda för att skapa interaktiv kommunikation
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: interactive-communications
docset: aem65
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 595f8d31-f297-48be-8ead-f171a60891b8
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1318'
ht-degree: 0%

---

# Introduktion till gränssnittet för utveckling av interaktiv kommunikation{#introduction-to-interactive-communication-authoring-ui}

Användargränssnittet för att skapa [interaktiv kommunikation](/help/forms/using/interactive-communications-overview.md) är intuitivt och innehåller följande funktioner för att skapa utskrift och webbkanal för interaktiv kommunikation:

* WYSIWYG dokumentredigerare med dra-och-släpp
* Integrerad databas för resurser - de resurser som överförs till och skapas på servern finns i resursläsaren i redigeringsgränssnittet för interaktiv kommunikation

När du [skapar eller redigerar en befintlig interaktiv kommunikation](../../forms/using/create-interactive-communication.md) använder du följande element i användargränssnittet:

* [Sidebar](#sidebar)
* [Verktygsfältet Sida](#page-toolbar)
* [Komponentverktygsfältet](#component-toolbar)
* Innehållsområde

![användargränssnittet för utveckling av interaktiv kommunikation](assets/form-editor.png)

**A.** Sidpanelen **B.** Sidverktygsfältet **C.** Innehållsområdet

## Sidebar {#sidebar}

![Sidofältet](assets/sidebar-comps-2.png)

**A.** Kanalwebbläsare **B.** Innehållsläsaren **C.** Egenskapsläsaren **D.** Resursläsaren **E.** Komponentwebbläsaren **F.** Datakällor - Datamodell **G.** Datakällwebbläsaren - Huvudinnehåll

<!-- Click to enlarge

![sidebar-comps-3](assets/sidebar-comps-3.png)-->

Sidlisten innehåller följande:

* **Kanalwebbläsare**

Med hjälp av webbläsaren Kanal kan du växla mellan tryck- och webbkanalerna i den interaktiva kommunikationen. Beroende på vilken kanal du har valt i webbläsaren visas alternativen i webbläsarna, till exempel Innehåll och Komponenter.

* **Innehållsläsaren**
I innehållsläsaren kan du se dokumentets objekthierarki för den valda kanalen. Författaren kan navigera till en viss komponent genom att trycka på det elementet i dokumentobjektträdet. Författaren kan söka efter objekt i webbkanalen och ordna om dem från det här trädet.

* **Egenskapsläsaren**

  Gör att du kan redigera egenskaperna för en komponent. Egenskaperna ändras beroende på komponenten. Om du till exempel vill se egenskaper för dokumentbehållaren:
Markera en komponent, välj ![fältnivå](assets/field-level.png) > **Dokumentbehållare** och välj sedan ![cmpr](assets/cmppr.png).

* **Assets webbläsare**
Segmenterar olika typer av innehåll, t.ex. layoutfragment, bilder, dokument, sidor, videor. Författaren kan dra och släppa material i den interaktiva kommunikationen.

* **Komponentwebbläsaren**
Innehåller komponenter som du kan använda för att skapa utskrifts- och webbkanaler för ett dokument. Du kan dra komponenter till den interaktiva kommunikationen för att lägga till element och konfigurera tillagda element enligt kraven. I följande tabell beskrivs komponenterna i komponentwebbläsaren för utskrifts- och webbkanaler:

| **Komponent** | **Utskriftskanal** | **Webbkanal** | **Funktionalitet** |
|---|---|---|---|
| Diagram | ✓ | ✓ | Lägger till ett diagram som du kan använda i en interaktiv kommunikation för visuell representation av tvådimensionella data som hämtats från ett objekt i en datamodellsamling för formulär. |
| Dokumentfragment | ✓ | ✓ | Gör att du kan lägga till en återanvändbar komponent, text, lista eller villkor i en interaktiv kommunikation. Den återanvändbara komponenten som du lägger till i en interaktiv kommunikation kan antingen vara modellbaserad i form av formulärdata eller utan någon formulärdatamodell. |
| Bild | ✓ | ✓ | Infoga en bild. |
| Panel | - | ✓ | Panelkomponenten är en platshållare för att gruppera andra komponenter och styr hur en grupp med komponenter placeras i ett interaktivt meddelande. Med en panelkomponent kan du också göra en grupp komponenter repeterbara för slutanvändaren, t.ex. i flera poster som krävs för att fylla i inloggningsuppgifter. Det är också bra att använda en panel var för en flik i en interaktiv kommunikation med flera flikar. |
| Tabell | &#42; | ✓ | Lägger till en tabell där du kan ordna data i rader och kolumner. |
| Målområde | &#42;&#42; | ✓ | Infogar ett målområde i en webbkanal för att ordna de webbkanalsspecifika komponenterna. |
| Text | - | ✓ | Lägger till text i webbkanalen i en interaktiv kommunikation. Text kan använda formulärdatamodellsobjekt för att göra innehållet dynamiskt. |

&#42; Använd Layoutfragment i utskriftskanalen för att lägga till tabeller.

&#42;&#42; I utskriftskanalen är målområdena fördefinierade i XDP/utskriftsmallen. Du kan inte lägga till nya målområden med hjälp av gränssnittet för redigering av interaktiv kommunikation.

* **Datakälläsaren**
I Datakälläsaren visas de tillgängliga datakällorna i den formulärdatamodell som du valde när du skapade den interaktiva kommunikationen.

### Viktiga punkter för arbete med komponenter {#key-points-for-working-with-components}

De viktigaste punkterna när du arbetar med interaktiva kommunikationskomponenter är följande:

* Varje komponent har tillhörande egenskaper som styr dess utseende och funktion. Om du vill konfigurera egenskaperna för en komponent markerar du komponenten och väljer ![cmpr](assets/cmppr.png) för att öppna komponentegenskaperna i egenskapsläsaren.
* En komponent identifieras med sitt elementnamn. När du väljer ![cmpr](assets/cmppr.png) kan du ändra komponentens namn genom att ändra elementnamnsfältets värde i egenskapsläsaren. Endast bokstäver, siffror, bindestreck (-) och understreck (_) godkänns i fältet Elementnamn. Andra specialtecken tillåts inte och elementnamnet måste börja med en bokstav.
* Du kan ändra egenskapen Title för en interaktiv kommunikationskomponent infogad i redigeraren utan att öppna egenskapsläsaren så länge titeln visas i den interaktiva kommunikationen. Så här gör du:

   1. Markera om du vill markera en komponent som har en Title-egenskap och vars Hide title-egenskap är inaktiverad.
   1. Markera ![aem_6_3_edit](assets/aem_6_3_edit.png) om du vill göra titeln redigerbar.

   1. Ändra titeln och välj returtangenten eller markera var som helst utanför komponenten för att spara ändringarna. Markera Esc om du vill ignorera ändringarna.

## Komponentverktygsfältet {#component-toolbar}

![Etiketter i komponentverktygsfältet](do-not-localize/component_toolbar_labels_new.png)

När du markerar en komponent visas ett verktygsfält där du kan arbeta med den. Du får alternativ för att klippa ut, klistra in, flytta och ange egenskaper för komponenterna. Dina alternativ är:

S.**Konfigurera**: När du väljer **Konfigurera** visas komponentegenskaperna i sidofältet.

B.**Redigera regler**: När du väljer Redigera regler visas Regelredigeraren där du kan redigera och skapa regler för den markerade komponenten. I Regelredigeraren kan du även markera andra formulärobjekt (komponenter) och redigera/skapa regler för dessa formulärobjekt.

C.**Kopiera**: Du kan använda kopieringsalternativet för att kopiera en komponent och klistra in den på andra platser i den interaktiva kommunikationen.

D.**Klipp ut**: Du kan använda alternativet Klipp ut för att flytta en komponent från en plats till en annan i den interaktiva kommunikationen.

E. **Ta bort**: Du kan ta bort komponenten från den interaktiva kommunikationen.

F. **Infoga komponent**: Gör att du kan infoga en komponent ovanför den markerade komponenten.

G. **Klistra in**: Gör att du kan klistra in komponenten som du klippt ut eller kopierat med alternativen som beskrivs ovan.

H. **Grupp**: Gör att du kan markera flera komponenter om du vill klippa ut, kopiera eller klistra in mer än en komponent tillsammans.

I. **Överordnad**: Gör att du kan välja överordnad för en komponent.

J. **Visa SOM-uttryck:** Gör att du kan visa [SOM-uttrycket](../../forms/using/using-som-expressions-adaptive-forms.md) för komponenten.

K: **Gruppera objekt på panelen:** Gör att du kan gruppera komponenterna på en panel för att utföra åtgärder på dessa komponenter samtidigt. Mer information finns i [Gruppera objekt på panelen](create-interactive-communication.md#groupobjectspanel).

L. **Lägg till underordnad panel** (endast för paneler): Gör att du kan lägga till en underordnad panel på panelen.

M: **Verktygsfältet Lägg till panel** (endast för paneler):Gör att du kan lägga till verktygsfältet för panelkomponenten. Sedan kan du utföra ytterligare åtgärder i verktygsfältet.

Med alternativet **Ersätt** i verktygsfältet kan du dessutom ersätta den befintliga komponenten med en alternativ komponent. Alternativet är inte tillgängligt för panelkomponenten.

## Verktygsfältet Sida {#page-toolbar}

Verktygsfältet Sida överst innehåller alternativ som gör att du kan förhandsgranska den interaktiva kommunikationen och ändra dess egenskaper. Du kan förhandsgranska den interaktiva kommunikationen när du redigerar den och göra ändringar i den. I verktygsfältet visas:

* Växla sidopanel ![till sidopanel](assets/toggle-side-panel.png): Visa eller dölj sidopanel.
* Sidinformation ![sidinformation och](assets/pageinformationad.png): Här kan du visa sidegenskaper.
* Emulatorn ![linjal](assets/ruler.png): Gör att du kan emulera utseendet i din interaktiva kommunikation för olika visningsstorlekar, som surfplattor och telefoner.
* Redigera: Här kan du välja andra lägen, t.ex. Redigera, Format, Utvecklare och Design.

   * Redigera: Här kan du redigera egenskaperna för den interaktiva kommunikationen och dess komponenter. Du kan till exempel lägga till en komponent, släppa en bild och ange obligatoriska fält.
   * Stil: Gör att du kan formatera utseendet på komponenterna i din interaktiva kommunikation. I stilläge kan du till exempel markera en panel och ange dess bakgrundsfärg.
   * Utvecklare: Gör att en utvecklare kan:

      * Upptäck vad interaktiv kommunikation består av.
      * Felsök vad som händer var och när, vilket i sin tur hjälper till att lösa problem.

   * Mål: Gör att du kan aktivera eller inaktivera anpassade komponenter eller komponenter som inte finns med i sidofältet.

* Förhandsgranska: Du kan förhandsgranska hur den interaktiva kommunikationen ser ut när du publicerar den.
