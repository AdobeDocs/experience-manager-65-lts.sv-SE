---
title: Publicera innehållssidor
description: Lär dig publicera innehållssidor i Adobe Experience Manager 6.5 LTS.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 89bab7e3-f688-4c95-8571-08477e737bc8
source-git-commit: 46948a857ccb9d093f946ec9e48ff585b0647314
workflow-type: tm+mt
source-wordcount: '1702'
ht-degree: 5%

---


# Publicera sidor {#publishing-pages}

När du har skapat och granskat ditt innehåll i författarmiljön [gör du det tillgängligt på din offentliga webbplats](/help/sites-authoring/author.md#concept-of-authoring-and-publishing) (din publiceringsmiljö).

Detta kallas att publicera en sida. När du vill ta bort en sida från publiceringsmiljön kallas det för att avpublicera. När sidan publiceras och avpubliceras är den fortfarande tillgänglig i redigeringsmiljön för ytterligare ändringar tills du tar bort den.

Du kan även publicera/avpublicera en sida direkt eller vid ett fördefinierat datum/tid i framtiden.

>[!NOTE]
>
>Vissa termer om publicering kan vara förvirrade:
>
>* **Publicera/avpublicera**
>  Detta är de primära villkoren för de åtgärder som gör innehållet tillgängligt för allmänheten i publiceringsmiljön (eller inte).
>
>* **Aktivera/inaktivera**
>  Dessa termer är synonyma med publicera/avpublicera.
>
>* **Replikera/replikera**
>  Detta är de tekniska termer som beskriver hur data flyttas (till exempel sidinnehåll, filer, kod, användarkommentarer) från en miljö till en annan, till exempel när användarkommentarer publiceras eller replikeras om.

## Otillräckliga privilegier {#insufficient-privileges}

Om du inte har behörighet att publicera en viss sida:

* Ett arbetsflöde kommer att utlösas för att meddela lämplig person om din begäran om publicering.
* Det här [arbetsflödet kan ha anpassats](/help/sites-developing/workflows-models.md#main-pars-procedure-6fe6) av ditt utvecklingsteam.
* Ett meddelande visas kort för att meddela dig att arbetsflödet har utlösts.

## Publicera sidor {#publishing-pages-1}

Beroende på din plats kan du publicera:

* [Från sidredigeraren](/help/sites-authoring/publishing-pages.md#publishing-from-the-editor)
* [Från webbplatskonsolen](/help/sites-authoring/publishing-pages.md#publishing-from-the-console)

### Publicera från Redigeraren {#publishing-from-the-editor}

Om du redigerar en sida kan den publiceras direkt från redigeraren.

1. Välj ikonen **Sidinformation** för att öppna menyn och sedan alternativet **Publicera sida** .

   ![screen_shot_2018-03-21at152734](assets/screen_shot_2018-03-21at152734.png)

1. Beroende på om sidan har referenser som behöver publiceras:

   * Sidan publiceras direkt om det inte finns några referenser att publicera.
   * Om sidan innehåller referenser som behöver publiceras visas dessa i guiden **Publicera** där du kan antingen:

      * Ange vilka resurser eller taggar du vill publicera tillsammans med sidan och använd sedan **Publicera** för att slutföra processen.

      * Använd **Avbryt** om du vill avbryta åtgärden.

   ![chlimage_1](assets/chlimage_1.png)

1. Om du väljer **Publicera** replikeras sidan till publiceringsmiljön. I sidredigeraren visas en informationsbanderoll som bekräftar publiceringsåtgärden.

   ![screen_shot_2018-03-21at152840](assets/screen_shot_2018-03-21at152840.png)

   När du visar samma sida i konsolen visas den uppdaterade publiceringsstatusen.

   ![pp-01](assets/pp-01.png)

>[!NOTE]
>
>Publicering från redigeraren är en ytlig publicering, det vill säga bara den valda sidan/de markerade sidorna publiceras och inga underordnade sidor publiceras.

>[!NOTE]
>
>Sidor som används av [alias](/help/sites-authoring/editing-page-properties.md#advanced) i redigeraren kan inte publiceras. Publiceringsalternativen i redigeraren är bara tillgängliga för sidor som du kommer åt via de faktiska sökvägarna.

### Publicera från konsolen {#publishing-from-the-console}

I platskonsolen finns det två alternativ för publicering:

* [Snabbpublicering](/help/sites-authoring/publishing-pages.md#quick-publish)
* [Hantera publikation](/help/sites-authoring/publishing-pages.md#manage-publication)

#### Snabbpublicering {#quick-publish}

**Snabbpublicering** är för enkla fall och publicerar de markerade sidorna direkt utan ytterligare interaktion. Därför kommer alla icke-publicerade referenser också att publiceras automatiskt.

Så här publicerar du en sida med Snabbpublicering:

1. Markera sidan eller sidorna i webbplatskonsolen och klicka på knappen **Snabbpublicering** .

   ![pp-02](assets/pp-02.png)

1. I dialogrutan Snabbpublicering bekräftar du publikationen genom att klicka på **Publicera** eller avbryta genom att klicka på **Avbryt**. Kom ihåg att alla opublicerade referenser också publiceras automatiskt.

   ![chlimage_1-1](assets/chlimage_1-1.png)

1. När sidan publiceras visas en varning som bekräftar publiceringen.

>[!NOTE]
>
>Snabbpublicering är en ytlig publicering, d.v.s. endast den valda sidan/de markerade sidorna publiceras och inga underordnade sidor publiceras.

#### Hantera publikation {#manage-publication}

**Hantera publikation** har fler alternativ än Snabbpublicering, vilket gör att underordnade sidor kan inkluderas, referenser anpassas och tillämpliga arbetsflöden startas och möjlighet att publicera vid ett senare datum.

Så här publicerar eller avpublicerar du en sida med Hantera publikation:

1. Markera sidan eller sidorna i webbplatskonsolen och klicka på knappen **Hantera publikation** .

   ![pp-02-1](assets/pp-02-1.png)

1. Guiden **Hantera publikation** startar. I det första steget, **Alternativ**, kan du:

   * Välj om du vill publicera eller avpublicera de markerade sidorna.
   * Välj om du vill utföra åtgärden nu eller vid ett senare datum.

   När du publicerar senare startas ett arbetsflöde för publicering av den eller de valda sidorna vid den angivna tidpunkten. Om du inte publicerar senare startas ett arbetsflöde för att avpublicera den eller de valda sidorna vid en viss tidpunkt.

   Om du vill avbryta en publicering/avpublicering senare går du till [arbetsflödeskonsolen](/help/sites-administering/workflows.md) och avslutar motsvarande arbetsflöde.

   ![chlimage_1-2](assets/chlimage_1-2.png)

   Klicka på **Nästa** för att fortsätta.

1. I nästa steg i guiden Hantera publikation, **Omfång**, kan du definiera omfattningen för publikationen/avpublikationen, till exempel att inkludera underordnade sidor och/eller inkludera referenser.

   ![screen_shot_2018-03-21at153354](assets/screen_shot_2018-03-21at153354.png)

   Du kan använda knappen **Lägg till innehåll** för att lägga till ytterligare sidor i listan över sidor som ska publiceras, om du inte valde någon innan du startade guiden Hantera publikation.

   Om du klickar på knappen Lägg till innehåll startas [sökvägsläsaren](/help/sites-authoring/author-environment-tools.md#path-browser) så att innehållet kan markeras.

   Markera önskade sidor och klicka sedan på **Välj** för att lägga till innehållet i guiden eller **Avbryt **för att avbryta valet och återgå till guiden.

   I guiden kan du markera ett objekt i listan för att konfigurera ytterligare alternativ, till exempel:

   * Inkludera dess underordnade.
   * Ta bort den från markeringen.
   * Hantera dess publicerade referenser.

   ![pp-03](assets/pp-03.png)

   Om du klickar på **Inkludera underordnade** öppnas en dialogruta där du kan:

   * Inkludera endast omedelbara barn.
   * Inkludera endast ändrade sidor.
   * Inkludera endast redan publicerade sidor.

   Klicka på **Lägg till** om du vill lägga till de underordnade sidorna i listan över sidor som ska publiceras eller avpubliceras baserat på de valda alternativen. Klicka på **Avbryt** om du vill avbryta markeringen och återgå till guiden.

   ![chlimage_1-3](assets/chlimage_1-3.png)

   När du återgår till guiden visas de sidor som lagts till baserat på dina val i dialogrutan Inkludera underordnade.

   Du kan visa och ändra referenserna som ska publiceras eller avpubliceras för en sida genom att markera den och sedan klicka på knappen **Publicerade referenser** .

   ![pp-04](assets/pp-04.png)

   I dialogrutan **Publicerade referenser** visas referenser för det markerade innehållet. Som standard är alla markerade och publiceras/avpubliceras, men du kan avmarkera dem så att de inte tas med i funktionsmakrot.

   Klicka på **Klar** om du vill spara ändringarna eller på **Avbryt** om du vill avbryta markeringen och återgå till guiden.

   I guiden uppdateras kolumnen **Referenser** så att den återspeglar ditt val av referenser som ska publiceras eller avpubliceras.

   ![pp-05](assets/pp-05.png)

1. Klicka på **Publicera** för att slutföra.

   I webbplatskonsolen bekräftar ett meddelande publikationen.

1. Om de publicerade sidorna är kopplade till arbetsflöden kan de visas i ett slutligt **arbetsflödessteg** i publikationsguiden.

   >[!NOTE]
   >
   >Steget **Arbetsflöden** visas baserat på vilka rättigheter din användare har eller inte har.
   >
   >Mer information finns i avsnitten [Otillräckliga behörigheter](/help/sites-authoring/publishing-pages.md#insufficient-privileges), [Hantera åtkomst till arbetsflöden](/help/sites-administering/workflows-managing.md) och [Använda arbetsflöden på sidor](/help/sites-authoring/workflows-applying.md#main-pars-text-5-bvhbkh-refd).

   Resurserna grupperas efter de arbetsflöden som utlöses och de olika alternativen:

   * Definiera arbetsflödets rubrik.
   * Behåll arbetsflödespaketet, förutsatt att arbetsflödet har [stöd för flera resurser](/help/sites-developing/workflows-models.md#configuring-a-workflow-for-multi-resource-support).
   * Definiera en titel på arbetsflödespaketet om alternativet att behålla arbetsflödespaketet har valts.

   Klicka på **Publicera** eller **Publicera senare** för att slutföra publiceringen.

   ![chlimage_1-4](assets/chlimage_1-4.png)

>[!TIP]
>
>När du schemalägger innehåll för publicering återges innehållet och publiceringsarbetsflödena respekteras. Om du tillfälligt vill dölja redan publicerat innehåll utan att avpublicera, bör du överväga att [**På tid** och **Av tid** är tillgängliga i sidegenskaperna.](/help/sites-authoring/editing-page-properties.md#on-time)

## Avpublicerar sidor {#unpublishing-pages}

Om du avpublicerar en sida tas den bort från publiceringsmiljön så att den inte längre är tillgänglig för läsarna.

På ett [sätt som liknar publicering](/help/sites-authoring/publishing-pages.md#publishing-pages) kan en eller flera sidor avpubliceras:

* [Från sidredigeraren](/help/sites-authoring/publishing-pages.md#unpublishing-from-the-editor)
* [Från webbplatskonsolen](/help/sites-authoring/publishing-pages.md#unpublishing-from-the-console)

### Avpublicera från redigeraren {#unpublishing-from-the-editor}

Om du vill avpublicera en sida när du redigerar den väljer du **Avpublicera sida** på menyn **Sidinformation**, på samma sätt som du [publicerar sidan](/help/sites-authoring/publishing-pages.md#publishing-from-the-editor).

>[!NOTE]
>
>Sidor som används av [alias](/help/sites-authoring/editing-page-properties.md#advanced) i redigeraren kan inte avpubliceras. Publiceringsalternativen i redigeraren är bara tillgängliga för sidor som du kommer åt via de faktiska sökvägarna.

### Avpublicera från konsolen {#unpublishing-from-the-console}

Precis som du [använder alternativet Hantera publikation för att publicera](/help/sites-authoring/publishing-pages.md#manage-publication) kan du även använda det för att avpublicera.

1. Markera sidan eller sidorna i webbplatskonsolen och klicka på knappen **Hantera publikation** .
1. Guiden **Hantera publikation** startar. I det första steget **Alternativ** väljer du **Avpublicera** i stället för standardalternativet **Publicera**.

   ![chlimage_1-5](assets/chlimage_1-5.png)

   På samma sätt som publiceringen senare startar ett arbetsflöde för att publicera den här versionen av sidan vid den angivna tidpunkten, startar inaktiveringen senare ett arbetsflöde för att avpublicera den valda sidan eller de valda sidorna vid en viss tidpunkt.

   Om du vill avbryta en publicering/avpublicering senare går du till [arbetsflödeskonsolen](/help/sites-administering/workflows.md) och avslutar motsvarande arbetsflöde.

1. Om du vill slutföra borttagningen fortsätter du med guiden på samma sätt som du [publicerar sidan](/help/sites-authoring/publishing-pages.md#manage-publication).

## Publicera och avpublicera ett träd {#publishing-and-unpublishing-a-tree}

När du har angett eller uppdaterat ett stort antal innehållssidor, som alla finns på samma rotsida, kan det vara enklare att publicera hela trädet i en enda åtgärd.

Du kan använda alternativet [Hantera publikation](/help/sites-authoring/publishing-pages.md#manage-publication) på webbplatskonsolen för att göra detta.

1. På webbplatskonsolen markerar du rotsidan för trädet som du vill publicera eller avpublicera och väljer **Hantera publikation**.
1. Guiden **Hantera publikation** startar. Välj om du vill publicera eller avpublicera och när det ska ske och välj **Nästa** för att fortsätta.
1. I steget **Omfång** markerar du rotsidan och väljer **Inkludera underordnade**.

   ![chlimage_1-6](assets/chlimage_1-6.png)

1. Avmarkera alternativen i dialogrutan **Inkludera underordnade**:

   * Inkludera endast omedelbart underordnade
   * Inkludera endast redan publicerade sidor

   De här alternativen är markerade som standard, så du måste komma ihåg att avmarkera dem. Klicka på **Lägg till** för att bekräfta och lägga till innehållet i publikationen/avpublikationen.

   ![chlimage_1-7](assets/chlimage_1-7.png)

1. Guiden **Hantera publikation** visar innehållet i trädet för granskning. Du kan anpassa markeringen ytterligare genom att lägga till eller ta bort de markerade sidorna.

   ![screen_shot_2018-03-21at154237](assets/screen_shot_2018-03-21at154237.png)

   Kom ihåg att du även kan granska referenser som ska publiceras via alternativet **Publicerade referenser**.

1. [Fortsätt med guiden Hantera publikation som vanligt](#manage-publication) för att slutföra publikationen eller avpublikationen av trädet.

## Bestämmer publiceringsstatus {#determining-publication-status}

Du kan ange en sidas publiceringsstatus:

* I [resursöversiktsinformationen på webbplatskonsolen](/help/sites-authoring/basic-handling.md#viewing-and-selecting-resources)

  ![screen-shot_2019-03-05at112019](assets/screen-shot_2019-03-05at112019.png)

  Publikationsstatusen visas i [kort](/help/sites-authoring/basic-handling.md#card-view)-, [kolumn](/help/sites-authoring/basic-handling.md#column-view)- och [list](/help/sites-authoring/basic-handling.md#list-view)vyerna i Sites-konsolen.

* På tidslinjen [](/help/sites-authoring/basic-handling.md#timeline)

  ![screen_shot_2018-03-21at154420](assets/screen_shot_2018-03-21at154420.png)

* På menyn [Sidinformation](/help/sites-authoring/author-environment-tools.md#page-information) när du redigerar en sida

  ![screen_shot_2018-03-21at154456](assets/screen_shot_2018-03-21at154456.png)
