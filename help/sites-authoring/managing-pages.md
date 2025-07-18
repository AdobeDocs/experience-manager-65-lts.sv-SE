---
title: Skapa och ordna sidor med AEM
description: Lär dig hur du skapar och hanterar sidor med Adobe Experience Manager.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 66bbd6d8-d07c-48ad-b58e-819bf032851a
source-git-commit: ebef0312d73597e28e5a1635a0e98f833d491d19
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 1%

---

# Skapa och ordna sidor {#creating-and-organizing-pages}

I det här avsnittet beskrivs hur du skapar och hanterar sidor med Adobe Experience Manager (AEM) så att du sedan kan [skapa innehåll](/help/sites-authoring/editing-content.md) på dessa sidor.

>[!NOTE]
>
>Ditt konto behöver [lämpliga åtkomsträttigheter](/help/sites-administering/security.md) och [behörigheter](/help/sites-administering/security.md#permissions) för att kunna utföra åtgärder på sidor som att skapa, kopiera, flytta, redigera och ta bort.
>
>Om du råkar ut för problem rekommenderar vi att du kontaktar systemadministratören.

>[!NOTE]
>
>Det finns flera [kortkommandon](/help/sites-authoring/keyboard-shortcuts.md) som du kan använda från webbplatskonsolen för att ordna dina sidor mer effektivt.

## Organisera din webbplats {#organizing-your-website}

Som författare kan du ordna din webbplats i AEM. Detta innebär att du skapar och namnger innehållssidorna så att:

* Du kan enkelt hitta dem i redigeringsmiljön
* Besökare på webbplatsen kan enkelt hitta dem i publiceringsmiljön

Du kan också använda [mappar](#creating-a-new-folder) för att ordna ditt innehåll.

Strukturen på en webbplats kan ses som en trädstruktur som innehåller dina innehållssidor. Namnen på dessa innehållssidor används för att skapa URL-adresserna, medan titeln visas när sidinnehållet visas.

I följande exempel visas ett exempel från webbplatsen We.Retail, där du kan komma åt en vandrande kortsida ( `desert-sky-shorts`):

* Författarmiljö
  `https://localhost:4502/editor.html/content/we-retail/us/en/products/equipment/hiking/desert-sky-shorts.html`

* Publiceringsmiljö
  `https://localhost:4503/content/we-retail/us/en/products/equipment/hiking/desert-sky-shorts.html`

Beroende på konfigurationen för din instans kan det vara valfritt att använda `/content` i publiceringsmiljön.

```xml
 /content
 /we-retail
  /us
   /en
    /products
     /equipment
      /hiking
       /desert-sky-shorts
       /hiking-poles
       /...
      /running...
      /surfing...
      /...
     /seasonal...
     /...
    /about-us
    /experience
    /...
   /es...
  /de...
  /fr...
  /...
 /...
```

Den här strukturen kan visas i konsolen **Platser**, där du kan [navigera bland sidorna på webbplatsen](/help/sites-authoring/basic-handling.md#navigating) och utföra åtgärder på sidorna. Du kan också skapa nya webbplatser och [nya sidor](#creating-a-new-page).

Du kan se grenen uppåt från vägbeskrivningar i sidhuvudsfältet:

![caop-01](assets/caop-01.png)

### Konventioner för sidnamngivning {#page-naming-conventions}

När du skapar en sida finns det två nyckelfält:

* **[Titel](#title)**:

   * Detta visas för användaren i konsolen och visas överst i sidinnehållet när det redigeras.
   * Detta fält är obligatoriskt.

* **[Namn](#name)**:

   * Detta används för att generera URI.
   * Användarindata för det här fältet är valfria. Om inget anges hämtas namnet från titeln. Mer information finns i följande avsnitt: [Begränsningar för sidnamn och Bästa metoder](/help/sites-authoring/managing-pages.md#page-name-restrictions-and-best-practices).

#### Begränsningar för sidnamn och bästa praxis {#page-name-restrictions-and-best-practices}

Sidans **titel** och **namn** kan skapas separat men hänger ihop:

* När du skapar en sida krävs bara fältet **Titel**. Om inget **namn** anges när sidan skapas, genererar AEM ett namn från de 64 första tecknen i titeln (med den validering som anges nedan). Endast de första 64 tecknen används för att ge stöd åt de bästa sätten med namn på korta sidor.

* Om ett sidnamn anges manuellt av författaren gäller inte gränsen på 64 tecken, men andra tekniska begränsningar på sidnamnets längd kan förekomma.

>[!NOTE]
>
>När du definierar ett sidnamn är en bra tumregel att hålla sidnamnet så kort, men så uttrycksfullt och minnesvärt som möjligt så att det blir lätt att förstå för läsaren. Mer information finns i [W3C-formatguiden](https://www.w3.org/Provider/Style/TITLE.html) för elementet `title` .
>
>Tänk också på att vissa webbläsare (till exempel äldre versioner av IE) bara kan acceptera URL:er med en viss längd, så det finns också tekniska skäl att hålla sidnamnen korta.

När du skapar en sida validerar AEM [sidnamnet enligt konventionerna](/help/sites-developing/naming-conventions.md) från AEM och JCR.

Minsta tillåtna tecken är:

* &#39;a&#39; till &#39;z&#39;
* A till Z
* 0 till 9
* `_` (understreck)
* `-` (bindestreck/minustecken)

Fullständig information om alla tillåtna tecken finns i [namnkonventionen](/help/sites-developing/naming-conventions.md).

>[!NOTE]
>
>Om AEM körs på en [MongoMK persistence Manager-distribution](/help/sites-deploying/recommended-deploys.md) är sidnamnen begränsade till 150 tecken.

#### Titel {#title}

Om du bara anger sidan **Rubrik** när du skapar en sida, hämtar AEM sidan **Namn** från den här strängen och [validerar namnet enligt konventionerna](/help/sites-developing/naming-conventions.md) som AEM och JCR har infört. Ett **titelfält** som innehåller ogiltiga tecken accepteras, men namnet som härleds kommer att ha ogiltiga tecken. Till exempel:

| Titel | Härlett namn |
|---|---|
| Schön | schoen.html |
| SC%&amp;&#42;ç+ | sc---c-.html |

#### Namn {#name}

När du anger en sida **Namn** när du skapar en sida, validerar AEM [namnet enligt konventionerna](/help/sites-developing/naming-conventions.md) från AEM och JCR. Du kan inte skicka ogiltiga tecken i fältet **Namn**. När AEM identifierar ogiltiga tecken markeras fältet med en förklaring.

![caop-02](assets/caop-02.png)

>[!NOTE]
>
>Du bör undvika att använda en kod med två bokstäver enligt ISO-639-1 som sidnamn, såvida det inte är en språkrot.
>
>Mer information finns i [Förbereder innehåll för översättning](/help/sites-administering/tc-prep.md).

### Mallar {#templates}

I AEM anger en mall en speciell typ av sida. En mall används som bas för alla nya sidor som skapas.

Mallen definierar strukturen för en sida, inklusive en miniatyrbild och andra egenskaper. Du kan till exempel ha separata mallar för produktsidor, platskartor och kontaktinformation. Mallar består av [komponenter](#components).

AEM levereras med flera färdiga mallar. Vilka mallar som är tillgängliga beror på den enskilda webbplatsen. Nyckelfälten är:

* **Titel**
Titeln som visas på den slutliga webbsidan.

* **Namn**
Används när sidan namnges.

* **Mall**
En lista med mallar som är tillgängliga för att användas när den nya sidan genereras.

>[!NOTE]
>
>Om den är konfigurerad på din instans kan [mallskapare skapa mallar med mallredigeraren](/help/sites-authoring/templates.md).

### Komponenter {#components}

Komponenterna är de element som finns i AEM så att du kan lägga till specifika typer av innehåll. AEM innehåller ett urval av [komponenter som inte finns installerade](/help/sites-authoring/default-components-console.md) och som har omfattande funktionalitet. Bland dessa finns:

* Text
* Bild
* Bildspel
* Video
* Och många fler

När du har skapat och öppnat en sida kan du [lägga till innehåll med komponenterna](/help/sites-authoring/editing-content.md#insertinganewparagraph), som är tillgängliga från [komponentwebbläsaren](/help/sites-authoring/author-environment-tools.md#componentbrowser).

>[!NOTE]
>
>[Komponentkonsolen](/help/sites-authoring/default-components-console.md) ger en översikt över komponenterna i din instans.

## Hantera sidor {#managing-pages}

### Skapa en ny sida {#creating-a-new-page}

Om du inte har skapat alla sidor åt dig i förväg måste du skapa en sida innan du kan börja skapa innehåll:

1. Öppna webbplatskonsolen (till exempel [https://localhost:4502/sites.html/content](https://localhost:4502/sites.html/content)).
1. Navigera till den plats där du vill skapa den nya sidan.
1. Öppna den nedrullningsbara väljaren med **Skapa** i verktygsfältet och välj sedan **Sida** i listan:

   ![caop-03](assets/caop-03.png)

1. Från det första steget i guiden kan du antingen:

   * Välj den mall som du vill använda för att skapa den nya sidan och klicka sedan på **Nästa** för att fortsätta.

   * **Avbryt** om du vill avbryta processen.

   ![caop-04](assets/caop-04.png)

1. Från det sista steget i guiden kan du antingen:

   * Använd de tre flikarna för att ange de [sidegenskaper](/help/sites-authoring/editing-page-properties.md) som du vill tilldela den nya sidan och klicka sedan på **Skapa** för att skapa sidan.

   * Använd **Bakåt** för att återgå till mallval.

   Nyckelfält är:

   * **Titel**:

      * Detta visas för användaren och är obligatoriskt.

   * **Namn**:

      * Detta används för att generera URI. Om inget anges hämtas namnet från titeln.
      * Om du anger en sida **Namn** när du skapar en sida, validerar AEM [namnet enligt konventionerna](/help/sites-developing/naming-conventions.md) från AEM och JCR.

      * Du **kan inte skicka ogiltiga tecken** i fältet **Namn**. När AEM identifierar ogiltiga tecken markeras fältet och en förklaring visas som anger vilka tecken som behöver tas bort/ersättas.

   >[!NOTE]
   >
   >Se [Konventioner för sidnamngivning](#page-naming-conventions).

   Den information som krävs för att skapa en sida är **Title**.

   ![caop-05](assets/caop-05.png)

1. Använd **Skapa** för att slutföra processen och skapa en ny sida. Bekräftelsedialogrutan frågar om du vill **öppna** sidan direkt eller återgå till konsolen (**Klar**):

   ![chlimage_1-118](assets/chlimage_1-118.png)

   >[!NOTE]
   >
   >Om du skapar en sida med ett namn som redan finns på den platsen, genereras automatiskt en variant av namnet genom att en siffra läggs till. Om `winter` till exempel redan finns blir en ny sida `winter0`.

1. Om du återgår till konsolen ser du den nya sidan:

   ![caop-06](assets/caop-06.png)

>[!CAUTION]
>
>När en sida har skapats går det inte att ändra dess mall, såvida du inte [skapar en start med en ny mall](/help/sites-authoring/launches-creating.md#create-launch-with-new-template), men innehåll som redan finns går förlorat.

### Öppna en sida för redigering {#opening-a-page-for-editing}

När du har skapat en sida eller navigerat till en befintlig sida (i konsolen) kan du öppna den för redigering:

1. Öppna konsolen **Platser**.
1. Navigera tills du hittar sidan som du vill redigera.
1. Välj sida genom att använda något av följande:

   * [Snabbåtgärder](/help/sites-authoring/basic-handling.md#quick-actions)
   * [Markeringsläge](/help/sites-authoring/basic-handling.md#navigatingandselectionmode) och verktygsfältet

   Välj sedan ikonen **Redigera**:

   ![screen_shot_2018-03-22at105355](assets/screen_shot_2018-03-22at105355.png)

1. Sidan öppnas och du kan [redigera sidan](/help/sites-authoring/editing-content.md#touchoptimizedui) efter behov.

>[!NOTE]
>
>Du kan bara navigera till andra sidor från sidredigeraren i förhandsgranskningsläget eftersom länkarna inte är aktiva i redigeringsläget.

### Kopiera och klistra in en sida {#copying-and-pasting-a-page}

Du kan kopiera en sida och alla dess underordnade sidor till en ny plats:

1. Navigera i konsolen **Platser** tills du hittar sidan som du vill kopiera.
1. Välj sida med hjälp av:

   * [Snabbåtgärder](/help/sites-authoring/basic-handling.md#quick-actions)
   * [Markeringsläge](/help/sites-authoring/basic-handling.md#navigatingandselectionmode) och verktygsfältet

   Sedan sidikonen **Kopiera**:

   ![screen_shot_2018-03-22at105425](assets/screen_shot_2018-03-22at105425.png)

   >[!NOTE]
   >
   >Om du är i markeringsläge avslutas detta automatiskt så snart sidan kopieras.

1. Navigera till den nya kopians plats.
1. Ikonen **Klistra in** är tillgänglig med en nedrullningsbar pil direkt till höger:

   ![Klistra in](assets/paste-without-children.png)

   Du kan antingen:
   * Markera själva sidikonen **Klistra in**: En kopia av originalsidan och eventuella underordnade sidor skapas på den här platsen.
   * Markera listrutepilen för att visa alternativet **Klistra in utan underordnade**. En kopia av originalsidan skapas på den här platsen. Underordnade sidor kopieras inte.

   >[!NOTE]
   >
   >Om du kopierar sidan till en plats där det redan finns en sida med samma namn som originalet, kommer systemet automatiskt att generera en variant av namnet genom att lägga till en siffra. Om `winter` redan finns blir till exempel `winter` `winter1`.

### Flytta eller byta namn på en sida {#moving-or-renaming-a-page}

>[!NOTE]
>
>Om du byter namn på en sida gäller även [Sidnamngivningskonventioner](#page-naming-conventions) när du anger det nya sidnamnet.

>[!NOTE]
>
>En sida kan bara flyttas till en plats där mallen som sidan baseras på tillåts. Mer information finns i [Malltillgänglighet](/help/sites-developing/templates.md#template-availability).

Proceduren för att flytta eller byta namn på en sida är i princip densamma och hanteras av samma guide. Med den här guiden kan du

* Byt namn på en sida utan att flytta den.
* Flytta sidan utan att byta namn på den.
* Flytta och byt namn samtidigt.

I AEM finns funktioner för att uppdatera interna länkar som refererar till sidan som byter namn/flyttas. Detta kan göras sida för sida för att ge full flexibilitet.

1. Navigera tills du hittar sidan som du vill flytta.
1. Välj sida med hjälp av:

   * [Snabbåtgärder](/help/sites-authoring/basic-handling.md#quick-actions)
   * [Markeringsläge](/help/sites-authoring/basic-handling.md#navigatingandselectionmode) och verktygsfältet

   Välj sedan sidikonen **Flytta**:

   ![screen_shot_2018-03-22at105534](assets/screen_shot_2018-03-22at105534.png)

   Guiden Flytta sida öppnas.

1. I steget **Byt namn** i guiden får du **information** om sidan, inklusive skapandedatum, sökväg och antal direkta referenser. Här kan du antingen:

   * Ange det namn du vill att sidan ska ha efter att den har flyttats och klicka sedan på **Nästa** för att fortsätta.

   * **Avbryt** om du vill avbryta processen.

   ![caop-07](assets/caop-07.png)

   Sidnamnet kan vara detsamma om du bara flyttar sidan.

   >[!NOTE]
   >
   >Om du flyttar en sida till en plats där det redan finns en sida med samma namn, kommer systemet automatiskt att generera en variant av namnet genom att lägga till en siffra. Om `winter` redan finns blir till exempel `winter` `winter1`.

1. Från **Välj mål**-steget i guiden kan du antingen:

   * Använd [kolumnvyn](/help/sites-authoring/basic-handling.md#column-view) för att navigera till sidans nya plats:

      * Markera målet genom att klicka på målets miniatyrbild.
      * Klicka på **Nästa** för att fortsätta.

   * Använd **Tillbaka** för att återgå till sidnamnsspecifikationen.

   >[!NOTE]
   >
   >Som standard väljs den överordnade sidan för sidan som du flyttar/byter namn på som mål.

   ![caop-08](assets/caop-08.png)

   >[!NOTE]
   >
   >Om du flyttar en sida till en plats där det redan finns en sida med samma namn, kommer systemet automatiskt att generera en variant av namnet genom att lägga till en siffra. Om `winter` redan finns blir till exempel `winter` `winter1`.

1. Om sidan är länkad till eller refererad, eller har publicerats, visas informationen i steget **Justera/Publicera**.

   Du kan ange vilka som ska justeras och/eller publiceras på nytt efter behov.

   >[!NOTE]
   >
   >* Om sidan varken är länkad till eller refererad är det här steget inte tillgängligt.
   >* I det här steget visas både direkta och indirekta referenser. Detta kan skilja sig från mängden som rapporteras i steget **Byt namn** i guiden samt referenserna som rapporteras av referenslinjen, där båda endast rapporterar direkta referenser av prestandaskäl.

   ![caop-09](assets/caop-09.png)

1. Om du väljer **Flytta** slutförs processen och du kan flytta/byta namn på sidan efter behov.

>[!NOTE]
>
>Om sidan redan har publicerats avpubliceras den automatiskt när du flyttar den. Som standard publiceras den om när flytten är klar, men detta kan ändras genom att avmarkera fältet **Publicera igen** i steget **Justera/Publicera igen**.

>[!NOTE]
>
>Om det inte finns någon referens till sidan på något sätt hoppas steget **Justera/Publicera** över.

#### Asynkrona åtgärder {#asynchronous-actions}

Åtgärder för att flytta sidor bearbetas alltid asynkront, vilket gör att användaren kan fortsätta att redigera i gränssnittet utan hinder.

* Användaren måste definiera när den asynkrona åtgärden ska utföras
   * **Nu** startar körningen av det asynkrona jobbet omedelbart.
   * **Senare** låter användaren definiera när det asynkrona jobbet ska starta.

  ![Asynkron sidflyttning](assets/asynchronous-page-move.png)

Status för asynkrona jobb kan kontrolleras på kontrollpanelen [**Async Jobs Status**](/help/sites-administering/asynchronous-jobs.md#monitor-the-status-of-asynchronous-operations) på **Global Navigation** > **Tools** > **Operations** > **Jobs**

>[!NOTE]
>
>Mer information om asynkron jobbbearbetning och hur du konfigurerar gränsen för åtgärder för att flytta/byta namn på sidor finns i dokumentet [Asynkrona jobb](/help/sites-administering/asynchronous-jobs.md) i användarhandboken för administration.

>[!NOTE]
>
>Asynkron bearbetning av sidflyttning kräver AEM 6.5.3.0 eller senare.

### Ta bort en sida {#deleting-a-page}

1. Navigera tills du ser sidan som du vill ta bort.
1. Använd [markeringsläget](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources) för att markera önskad sida och använd sedan **Ta bort** i verktygsfältet:

   ![screen_shot_2018-03-22at105622](assets/screen_shot_2018-03-22at105622.png)

   >[!NOTE]
   >
   >Som en säkerhetsåtgärd är ikonen **Ta bort** inte tillgänglig som en snabbåtgärd.

1. En dialogruta öppnas där du uppmanas att bekräfta. Använd:

   * **Avbryt** om du vill avbryta åtgärden
   * **Ta bort** för att bekräfta åtgärden:

      * Om sidan inte har några referenser tas sidan bort.
      * Om sidan har referenser visas en meddelanderuta om att det finns referenser till **en eller flera sidor.** Du kan välja **Tvinga borttagning** eller **Avbryt**.

>[!NOTE]
>
>Om en sida redan har publicerats avpubliceras den automatiskt innan den tas bort.

### Låsa en sida {#locking-a-page}

Du kan [låsa/låsa upp en sida](/help/sites-authoring/editing-content.md#locking-a-page) från en konsol eller när du redigerar en enskild sida. Information om låsta sidor visas också på båda platserna.

![screen_shot_2018-03-22at105713](assets/screen_shot_2018-03-22at105713.png) ![screen_shot_2018-03-22at105720](assets/screen_shot_2018-03-22at105720.png)

### Skapa en ny mapp {#creating-a-new-folder}

Du kan skapa mappar som hjälper dig att ordna dina filer och sidor.

>[!NOTE]
>
>Mappar omfattas också av [Sidnamngivningskonventioner](#page-naming-conventions) när du anger det nya mappnamnet.

>[!CAUTION]
>
>* Mappar kan bara skapas direkt under **Platser** eller under andra mappar. De kan inte skapas under en sida.
>* Standardåtgärderna för att flytta, kopiera, klistra in, ta bort, publicera, avpublicera och visa/redigera egenskaper kan utföras på en mapp.
>* Det går inte att välja mappar i en live-kopia.
>

1. Öppna konsolen **Platser** och navigera till önskad plats.
1. Välj **Skapa** i verktygsfältet för att öppna alternativlistan
1. Välj **Mapp** för att öppna dialogrutan. Här kan du ange **Namn** och **Titel**:

   ![chlimage_1-119](assets/chlimage_1-119.png)

1. Välj **Skapa** för att skapa mappen.
