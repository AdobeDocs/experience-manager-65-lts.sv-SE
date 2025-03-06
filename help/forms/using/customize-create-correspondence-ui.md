---
title: Anpassa gränssnitt för att skapa korrespondens
description: Lär dig hur du anpassar ett gränssnitt för korrespondens, t.ex. logotyp, i AEM Forms-miljön.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
docset: aem65
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 31fad2a6-f6e8-4d9c-a2b2-6749635fcf9b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 0%

---

# Anpassa gränssnitt för att skapa korrespondens{#customize-create-correspondence-ui}

## Ökning {#overview}

Med Correspondence Management kan ni ommärka sin lösningsmall för att få ett bättre varumärkesvärde och följa företagets varumärkesstandarder. När du ändrar användargränssnittet innebär det att du ändrar organisationslogotypen, som visas i det övre vänstra hörnet av användargränssnittet Create Correspondence.

Du kan ändra logotypen i användargränssnittet Create Correspondence med din organisations logotyp.

![Den anpassade ikonen i gränssnittet Skapa korrespondens ](assets/0_1_introscreenshot.png)

Den anpassade ikonen i gränssnittet Skapa korrespondens

### Ändra logotypen i användargränssnittet för Create Correspondence {#changing-the-logo-in-the-create-correspondence-ui}

Så här ställer du in en logotypbild:

1. Skapa lämplig [mappstruktur i CRX](#creatingfolderstructure).
1. [Överför den nya logotypfilen](#uploadlogo) i den mapp du har skapat i CRX.

1. [Konfigurera CSS](#createcss) på CRX för att referera till den nya logotypen.
1. Rensa webbläsarhistoriken och [uppdatera användargränssnittet för Skapa korrespondens](#refreshccrui).

## Skapar den mappstruktur som krävs {#creatingfolderstructure}

Skapa mappstrukturen som beskrivs nedan för värdtjänster för den anpassade logotypbilden och formatmallen. Den nya mappstrukturen med rotmappen /apps liknar strukturen i mappen /libs.

Om du vill anpassa något skapar du en parallell mappstruktur, enligt beskrivningen nedan, i grenen /apps.

`/apps`-grenen (mappstruktur):

* Säkerställer att dina filer är säkra om det finns en uppdatering av systemet. Om det finns en uppgradering, ett funktionspaket eller en snabbkorrigering uppdateras grenen `/libs` och om du är värd för dina ändringar i grenen `/libs` skrivs de över.
* Gör att det inte stör det aktuella systemet/den aktuella grenen, som du kan ta bort av misstag om du använder standardplatserna för lagring av anpassade filer.
* Hjälper dina resurser att få högre prioritet när AEM söker efter resurser. AEM har konfigurerats att söka igenom grenen `/apps` först och sedan grenen `/libs` för att hitta en resurs. Den här mekanismen innebär att systemet använder övertäckningen (och de anpassningar som definieras där).

Följ de här stegen för att skapa den nödvändiga mappstrukturen i grenen `/apps`:

1. Gå till `https://'[server]:[port]'/[ContextPath]/crx/de` och logga in som administratör.
1. I appmappen skapar du en mapp med namnet `css` med en sökväg/struktur som liknar css-mappen (i ccrui-mappen).

   Steg för att skapa css-mappen:

   1. Högerklicka på mappen **css** på följande sökväg och välj **Överläggsnod**: `/libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/css`

      ![Överläggsnod](assets/1_overlaynode_css.png)

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** `/libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/css`

      **Plats för övertäckning:** `/apps/`

      **Matcha nodtyper:** Markerade

      ![Sökväg till överläggsnod](assets/0_1_5ioverlaynodedialog.png)

      >[!NOTE]
      >
      >Ändra inte grenen `/libs`. Alla ändringar du gör kan gå förlorade eftersom den här grenen kan ändras när du:
      >
      >    
      >    
      >    * Uppgradera till din instans
      >    * Använd en snabbkorrigering
      >    * Installera ett funktionspaket
      >    
      >

   1. Klicka på **OK**. CSS-mappen skapas i den angivna sökvägen.

1. I appmappen skapar du en mapp med namnet `imgs` med en sökväg/struktur som liknar mappen `imgs` (i ccrui-mappen).

   1. Högerklicka på mappen **imgs** på följande sökväg och välj **Överläggsnod**: `/libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/imgs`
   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/imgs

      **Plats för övertäckning:** /apps/

      **Matcha nodtyper:** Markerade

   1. Klicka på **OK**.

      >[!NOTE]
      >
      >Du kan också skapa mappstrukturen i mappen /apps manuellt.

1. Klicka på **Spara alla** för att spara ändringarna på servern.

## Ladda upp den nya logotypen till CRX {#uploadlogo}

Ladda upp din anpassade logotypfil till CRX. HTML standardregler styr återgivningen av logotypen. De bildfilsformat som stöds är beroende på vilken webbläsare du använder för att få åtkomst till AEM Forms. Alla webbläsare stöder JPEG, GIF och PNG. Mer information finns i den webbläsarspecifika dokumentationen om de bildformat som stöds.

* Standardmåtten för logotypbilden är 48 px &#42; 48 px. Se till att bilden liknar den här storleken eller är större än 48 px &#42; 48 px.
* Om höjden på logotypbilden är större än 50 px skalas bilden ned i användargränssnittet Create Correspondence till en maximal höjd på 50 px eftersom det är höjden på sidhuvudet. När du skalar ned bilden behåller användargränssnittet Skapa korrespondens bildens proportioner.
* Gränssnittet Skapa korrespondens skalar inte upp bilden om den är liten, så se till att du använder en logotypbild som är minst 48 px hög och tillräckligt bred för att bilden ska bli tydlig.

Så här överför du den anpassade logotypfilen till CRX:

1. Gå till `https://'[server]:[port]'/[contextpath]/crx/de`. Logga in som administratör om det behövs.
1. I CRXDE högerklickar du på mappen **imgs** på följande sökväg och väljer **Skapa > Skapa fil**:

   `/apps/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/imgs/`

   ![Skapa ny nod i imgs-mappen](assets/2_contentexplorernewnode.png)

1. I dialogrutan Skapa fil anger du namnet på filen som CustomLogo.png (eller namnet på logotypfilen).

   ![CustomLogo.png som ny nod](assets/3_contentexplorernewnode_customlogo.png)

1. Klicka på **Spara alla**.

   Under den nya filen som du har skapat (här CustomLogo.png) visas egenskapen jcr:content.

1. Klicka på jcr:innehåll i mappstrukturen.

   jcr:innehållets egenskaper visas.

   ![jcrcontentproperties](assets/jcrcontentproperties.png)

1. Dubbelklicka på egenskapen **jcr:data** .

   Dialogrutan Redigera jcr:data visas.

   Klicka nu på mappen newlogo.png, dubbelklicka på jcr:content (dim option) och ange typen nt:resource. Om den inte finns skapar du en egenskap med namnet jcr:content.

1. I dialogrutan Redigera jcr:data klickar du på **Bläddra** och väljer den bildfil som du vill använda som logotyp (här CustomLogo.png).

   De bildfilsformat som stöds är beroende på vilken webbläsare du använder för att få åtkomst till AEM Forms. Alla webbläsare stöder JPEG, GIF och PNG. Mer information finns i den webbläsarspecifika dokumentationen om de bildformat som stöds.

   ![Exempel på anpassad logotypfil](assets/geometrixx-outdoors.png)

   Exempel: CustomLogo.png som ska användas som anpassad logotyp

1. Klicka på **Spara alla**.

## Skapa CSS för återgivning av logotypen med användargränssnittet {#createcss}

Den anpassade logotypbilden kräver en extra formatmall för att kunna läsas in i innehållskontexten.

Följ de här stegen för att skapa formatmallen för återgivning av logotypen med användargränssnittet:

1. Gå till `https://'[server]:[port]'/[contextpath]/crx/de`. Logga in som administratör om det behövs.
1. Skapa en fil med namnet customcss.css (du kan inte använda ett annat filnamn) på följande plats:

   `/apps/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/css/`

   Steg för att skapa filen customcss.css:

   1. Högerklicka på mappen **css** och välj **Skapa > Skapa fil**.
   1. I dialogrutan Ny fil anger du CSS-filens namn som `customcss.css` (du kan inte använda ett annat filnamn) och klickar på **OK**.
   1. Lägg till följande kod i den nyligen skapade CSS-filen. I content:url i koden anger du bildnamnet som du har överfört till imgs-mappen i CRXDE.

      ```css
      .logo, .logo:after {
      content:url("../imgs/CustomLogo.png");
      }
      ```

   1. Klicka på **Spara alla**.

## Uppdatera användargränssnittet Create Correspondence så att du kan se den anpassade logotypen {#refreshccrui}

Rensa webbläsarcachen och öppna sedan instansen Create Correspondence UI i webbläsaren så att du kan se din anpassade logotyp.

![Skapa korrespondensanvändargränssnitt med anpassad logotyp](assets/0_1_introscreenshot-1.png)

Den anpassade ikonen i gränssnittet Skapa korrespondens
