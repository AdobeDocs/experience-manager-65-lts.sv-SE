---
title: Skapa och ordna sidor
description: I det här avsnittet beskrivs hur du skapar och hanterar sidor med AEM så att du sedan kan skapa innehåll på dessa sidor.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: bff63900-0007-472d-a910-bf20b8013668
source-git-commit: ebef0312d73597e28e5a1635a0e98f833d491d19
workflow-type: tm+mt
source-wordcount: '1898'
ht-degree: 0%

---

# Skapa och ordna sidor{#creating-and-organizing-pages}

I det här avsnittet beskrivs hur du skapar och hanterar sidor med Adobe Experience Manager (AEM) så att du sedan kan [skapa innehåll](/help/sites-classic-ui-authoring/classic-page-author-edit-content.md) på dessa sidor.

>[!NOTE]
>
>Ditt konto behöver [lämpliga åtkomsträttigheter](/help/sites-administering/security.md) och [behörigheter](/help/sites-administering/security.md#permissions) för att kunna utföra åtgärder på sidor, till exempel skapa, kopiera, flytta, redigera, ta bort.
>
>Om du råkar ut för problem rekommenderar vi att du kontaktar systemadministratören.

## Organisera din webbplats {#organizing-your-website}

Som författare måste du organisera din webbplats inom AEM. Detta innebär att du skapar och namnger innehållssidorna så att:

* kan du enkelt hitta dem i redigeringsmiljön
* besökare på er webbplats kan enkelt bläddra bland dem i publiceringsmiljön

Du kan också använda [mappar](#creating-a-new-folder) för att ordna ditt innehåll.

Strukturen på en webbplats kan ses som en *trädstruktur* som innehåller dina innehållssidor. Namnen på dessa innehållssidor används för att skapa URL-adresserna, medan titeln visas när sidinnehållet visas.

Här nedan visas ett utdrag från Geometrixx webbplats, där du till exempel kommer åt sidan `Triangle`:

* Författarmiljö

  `http://localhost:4502/cf#/content/geometrixx/en/products/triangle.html`

* Publiceringsmiljö

  `http://localhost:4503/content/geometrixx/en/products/triangle.html`

  Beroende på konfigurationen för din instans kan det vara valfritt att använda `/content` i publiceringsmiljön.

```xml
  /content
    /geometrixx
      /en
        /toolbar...
        /products
          /triangle
            /overview
            /features
          /square...
          /circle...
          /...
        /...
      /fr...
      /de...
      /es...
      /...
    /...
```

Den här strukturen kan visas från webbplatskonsolen, som du kan använda för att [navigera i trädstrukturen](/help/sites-classic-ui-authoring/author-env-basic-handling.md#main-pars-text-15).

![chlimage_1-151](assets/chlimage_1-151.png)

### Konventioner för sidnamngivning {#page-naming-conventions}

När du skapar en sida finns det två nyckelfält:

* **[Titel](#title)**:

   * Detta visas för användaren i konsolen och visas överst i sidinnehållet när det redigeras.
   * Detta fält är obligatoriskt.

* **[Namn](#name)**:

   * Detta används för att generera URI.
   * Användarindata för det här fältet är valfria. Om inget anges hämtas namnet från titeln.

När du skapar en sida validerar AEM [sidnamnet enligt konventionerna](/help/sites-developing/naming-conventions.md) från AEM och JCR.

Implementeringen och listan över tillåtna tecken skiljer sig något beroende på användargränssnittet (det är mer omfattande för det beröringsaktiverade användargränssnittet), men det minsta tillåtna är:

* &#39;a&#39; till &#39;z&#39;
* A till Z
* 0 till 9
* _ (understreck)
* `-` (bindestreck/minustecken)

Använd bara dessa tecken om du vill vara säker på att de accepteras/används (om du behöver fullständig information om alla tecken som tillåts, se [namnkonventionen](/help/sites-developing/naming-conventions.md)).

#### Titel {#title}

Om du bara anger sidan **Rubrik** när du skapar en sida, hämtar AEM sidan **Namn** från den här strängen och [validerar namnet enligt konventionerna](/help/sites-developing/naming-conventions.md) som AEM och JCR har infört. I båda gränssnitten accepteras ett **Title**-fält som innehåller ogiltiga tecken, men det härledda namnet har de ogiltiga tecknen. Till exempel:

| Titel | Härlett namn |
|---|---|
| Schön | schoen.html |
| SC%&amp;&amp;ast;ç+ | sc---c-.html |

#### Namn {#name}

Om du anger en sida **Namn** när du skapar en sida, validerar AEM [namnet enligt konventionerna](/help/sites-developing/naming-conventions.md) från AEM och JCR.

I det klassiska användargränssnittet kan du **inte ange ogiltiga tecken** i fältet **Namn**.

>[!NOTE]
>I det beröringsaktiverade användargränssnittet kan du **inte skicka ogiltiga tecken** i fältet **Namn**. När AEM identifierar ogiltiga tecken markeras fältet och en förklaring visas som anger vilka tecken som behöver tas bort/ersättas.

>[!NOTE]
>
>Du bör undvika att använda en kod med två bokstäver enligt ISO-639-1, om det inte är en språkrot.
>
>Mer information finns i [Förbereder innehåll för översättning](/help/sites-administering/tc-prep.md).

### Mallar {#templates}

I AEM anger en mall en speciell typ av sida. En mall används som bas för alla nya sidor som skapas.

Mallen definierar strukturen för en sida, inklusive en miniatyrbild och andra egenskaper. Du kan till exempel ha separata mallar för produktsidor, platskartor och kontaktinformation. Mallar består av [komponenter](#components).

AEM levereras med flera färdiga mallar. Vilka mallar som visas beror på den enskilda webbplatsen och vilken information som behöver anges (när du skapar den nya sidan) beroende på vilket gränssnitt som används. Nyckelfälten är:

* **Titel**
Titeln som visas på den slutliga webbsidan.

* **Namn**
Används när sidan namnges.

* **Mall**
En lista med mallar som är tillgängliga för att användas när den nya sidan genereras.

### Komponenter {#components}

Komponenterna är de element som finns i AEM så att du kan lägga till specifika typer av innehåll. AEM har en rad färdiga komponenter som ger omfattande funktionalitet:

* Text
* Bild
* Bildspel
* Video
* många fler

När du har skapat och öppnat en sida kan du [lägga till innehåll med hjälp av komponenterna](/help/sites-classic-ui-authoring/classic-page-author-edit-content.md#insertinganewparagraph), som är tillgängliga från [sidekick](/help/sites-classic-ui-authoring/classic-page-author-env-tools.md#sidekick).

## Hantera sidor {#managing-pages}

### Skapa en ny sida {#creating-a-new-page}

Om du inte har skapat alla sidor åt dig i förväg måste du skapa en sida innan du kan börja skapa innehåll:

1. I konsolen **Webbplatser** väljer du på vilken nivå du vill skapa en sida.

   I följande exempel skapar du en sida under nivån **Produkter**, som visas i den vänstra rutan. I den högra rutan visas sidor som redan finns på nivån under **Produkter**.

   ![screen_shot_2012-02-15at114413am](assets/screen_shot_2012-02-15at114413am.png)

1. Välj **Ny sida..** på menyn **Ny...** (klicka på pilen bredvid **Ny...**). Fönstret **Skapa sida** öppnas.

   Om du klickar på **Ny..** fungerar det också som en genväg till alternativet **Ny sida..**.

1. I dialogrutan **Skapa sida** kan du:

   * Ange en **titel**. Den visas för användaren.
   * Ange ett **namn**. Detta används för att generera URI:n. Om inget anges hämtas namnet från titeln.

      * Om du anger sidan **Namn** när du skapar en sida, validerar AEM [namnet enligt konventionerna](/help/sites-developing/naming-conventions.md) som AEM och JCR har infört.
      * I det klassiska användargränssnittet kan du **inte ange ogiltiga tecken** i fältet **Namn**.

   * Klicka på den mall som du vill använda för att skapa den nya sidan.

     Mallen används som bas för den nya sidan, till exempel för att fastställa den grundläggande layouten för en innehållssida.

   >[!NOTE]
   >
   >Se [Konventioner för sidnamngivning](#page-naming-conventions).

   Den information som krävs för att skapa en sida är **Rubrik** och den mall som krävs.

   ![screen_shot_2012-02-15at114845am](assets/screen_shot_2012-02-15at114845am.png)

   >[!NOTE]
   >
   >Om du vill använda Unicode-tecken i URL-adresserna anger du aliasegenskapen ( `sling:alias`) ([sidegenskaper](/help/sites-classic-ui-authoring/classic-page-author-edit-page-properties.md)).

1. Klicka på **Skapa** för att skapa sidan. Du kommer tillbaka till konsolen **Webbplatser** där du kan se ett inlägg för den nya sidan.

   Konsolen ger information om sidan (till exempel när den senast redigerades och av vem) som uppdateras efter behov.

   >[!NOTE]
   >
   >Du kan också skapa en sida när du redigerar en befintlig sida. Om du använder **Skapa underordnad sida** från fliken **Sida** i sidosparken skapas en sida direkt under sidan som redigeras.

### Öppna en sida för redigering {#opening-a-page-for-editing}

Du kan öppna sidan som ska [redigeras](/help/sites-classic-ui-authoring/classic-page-author-edit-content.md#editing-a-component-content-and-properties) på något av följande sätt:

* Från konsolen **Webbplatser** kan du **dubbelklicka** på sidposten för att öppna den för redigering.

* Från konsolen **Webbplatser** kan du **högerklicka** (snabbmenyn) på sidobjektet och sedan välja **Öppna** på menyn.

* När du har öppnat en sida kan du navigera till andra sidor på webbplatsen (för att redigera dem) genom att klicka på hyperlänkar.

### Kopiera och klistra in en sida {#copying-and-pasting-a-page}

Vid kopiering kan du antingen kopiera:

* en sida
* en sida med alla undersidor

1. I konsolen **Webbplatser** väljer du den sida du vill kopiera.

   >[!NOTE]
   >
   >I det här skedet är det irrelevant om du vill kopiera en enstaka sida eller de underliggande undersidorna.

1. Klicka på **Kopiera**.

1. Navigera till den nya platsen och klicka på:

   * **Klistra in** - om du vill klistra in sidan tillsammans med alla underordnade sidor
   * **Skift + Klistra in** - om du bara vill klistra in den markerade sidan

   Sidorna klistras in på den nya platsen.

   >[!NOTE]
   >
   >Sidnamnet kan justeras automatiskt om en befintlig sida redan har samma namn.

   >[!NOTE]
   >
   >Du kan också använda **Kopiera sida** från fliken **Sida** i sidosparken. Då öppnas en dialogruta där du kan ange mål och så vidare.

### Flytta eller byta namn på sida {#moving-or-renaming-page}

>[!NOTE]
>
>Om du byter namn på en sida gäller även [Sidnamngivningskonventioner](#page-naming-conventions) när du anger det nya sidnamnet.

Du flyttar eller byter namn på en sida på samma sätt. Med samma åtgärd kan man

* flytta en sida till en ny plats
* byta namn på en sida på samma plats
* flytta en sida till en ny plats och byta namn på den samtidigt

AEM erbjuder funktioner för att uppdatera interna länkar till sidan som byter namn eller flyttas. Detta kan göras sida för sida för att ge full flexibilitet.

Så här flyttar eller byter du namn på en sida:

1. Det finns olika metoder för att utlösa en flytt:

   * I konsolen **Webbplatser** klickar du för att markera sidan och väljer sedan **Flytta..**.
   * I konsolen **Webbplatser** kan du även välja sidobjektet, sedan **högerklicka** och välja **Flytta..**.
   * När du redigerar en sida kan du välja **Flytta sida** på fliken **Sida** i sidosparken.

1. Fönstret **Flytta** öppnas. Här kan du antingen ange en ny plats, ett nytt namn för sidan eller båda.

   ![screen_shot_2012-02-15at121336pm](assets/screen_shot_2012-02-15at121336pm.png)

   På sidan visas även alla sidor som direkt eller indirekt refererar till den sida som flyttas. Beroende på status för referenssidan kan du eventuellt justera länkarna på och/eller publicera om sidorna.

1. Fyll i följande fält, beroende på vad som är tillämpligt:

   * **Mål**

     Använd platskartan (som är tillgänglig via den nedrullningsbara väljaren) för att välja platsen dit sidan ska flyttas.

     Ignorera det här fältet om du bara byter namn på sidan.

   * **Flytta**

     Ange vilken sida som ska flyttas - den fylls vanligtvis i som standard, beroende på hur och var du startade flyttåtgärden.

   * **Byt namn till**

     Den aktuella sidetiketten visas som standard. Ange vid behov den nya sidetiketten.

   * **Justera**

     Uppdatera länkarna på den sida som listas där pekar på den flyttade sidan: om sidan A till exempel har länkar till sidan B justerar AEM länkarna på sidan A om du flyttar sidan B.

     Detta kan markeras/avmarkeras för varje enskild referenssida.

   * **Publicera igen**

     Publicera om referenssidan. Återigen kan du välja detta för varje enskild sida.

   >[!NOTE]
   >
   >Om sidan redan är aktiverad inaktiveras den automatiskt när du flyttar den. Som standard återaktiveras den när flytten är klar, men detta kan ändras genom att du avmarkerar fältet **Publicera igen** för sidan i fönstret **Flytta**.

1. Klicka på **Flytta**. Bekräftelse krävs. Bekräfta genom att klicka på **OK**.

   >[!NOTE]
   >
   >Sidans titel kommer inte att uppdateras.

### Ta bort en sida {#deleting-a-page}

1. Du kan ta bort en sida från olika platser:

   * I konsolen **Webbplatser** klickar du för att markera sidan, högerklickar och väljer **Ta bort** på menyn som visas.
   * I konsolen **Webbplatser** klickar du för att markera sidan och väljer sedan **Ta bort** på verktygsmenyn.
   * Använd fliken **Sida** för att välja **Ta bort sida** i sidosparken. Då tas sidan som är öppen bort.

1. När du har valt att ta bort en sida måste du bekräfta begäran eftersom åtgärden inte kan ångras.

   >[!NOTE]
   >
   >Om sidan har publicerats efter borttagningen kan du återställa den senaste versionen (eller en viss version), men detta har kanske inte exakt samma innehåll som den senaste versionen om ytterligare ändringar har gjorts. Mer information finns i [Återställa sidor](/help/sites-classic-ui-authoring/classic-page-author-work-with-versions.md#restoringpages).

>[!NOTE]
>
>Om en sida redan är aktiverad kommer den automatiskt att inaktiveras innan den tas bort.

### Låsa en sida {#locking-a-page}

Du kan [låsa/låsa upp en sida](/help/sites-classic-ui-authoring/classic-page-author-edit-content.md#locking-a-page) från en konsol eller när du redigerar en enskild sida. Information om låsta sidor visas också på båda platserna.

### Skapa en ny mapp {#creating-a-new-folder}

>[!NOTE]
>
>Mappar omfattas också av [Sidnamngivningskonventioner](#page-naming-conventions) när du anger det nya mappnamnet.

1. Öppna konsolen **Webbplatser** och navigera till önskad plats.
1. Välj **Ny mapp..** på menyn **Ny...** (klicka på pilen bredvid **Ny..**).
1. Dialogrutan **Skapa mapp** öppnas. Här kan du ange **Namn** och **Titel**:

   ![chlimage_1-152](assets/chlimage_1-152.png)

1. Välj **Skapa** för att skapa mappen.
