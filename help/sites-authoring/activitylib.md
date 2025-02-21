---
title: Hantera aktiviteter
description: Med aktivitetskonsolen kan ni skapa, organisera och hantera marknadsföringsaktiviteter för era varumärken
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: personalization
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1937'
ht-degree: 10%

---

# Hantera aktiviteter{#managing-activities}

Med aktivitetskonsolen kan du skapa, ordna och hantera marknadsföringen av [aktiviteter](/help/sites-authoring/personalization.md#activities) för dina varumärken:

* Lägg till varumärken.
* Lägg till och konfigurera aktiviteter för varje varumärke.
* Administrera aktiviteter.

>[!NOTE]
>
>Om du använder Adobe Target som målmotor kan du även [visa prestandadata för dina aktiviteter](#viewing-performance-and-converting-winning-experiences-a-b-test). Om du använder A/B-tester kan du [konvertera vinnare](#viewing-performance-and-converting-winning-experiences-a-b-test).

På aktivitetskonsolen ordnas aktiviteterna efter varumärke. Du kan använda varumärken och mappar för att strukturera organisationen av dina aktiviteter. Du navigerar till aktivitetskonsolen genom att trycka på/klicka på **Personalization** och trycka/klicka på **Aktiviteter**.

Aktiviteter är tillgängliga i målläge för [redigering av målinnehåll](/help/sites-authoring/content-targeting-touch.md), där du även kan skapa aktiviteter. Aktiviteter som du skapar i målläge visas i aktivitetskonsolen.

Aktiviteter visas med en etikett som beskriver vilken typ av aktivitet som definieras:

* XT - målinriktad Adobe Target-upplevelse
* A/B - Adobe Target A/B-testning
* AEM - målinriktning för Adobe Experience Manager (kontextub- eller klientstyrd)

![chlimage_1-114](assets/chlimage_1-114.png)

>[!NOTE]
>
>Vilka typer av aktiviteter som är tillgängliga bestäms av följande:
>
>* Om alternativet **xt_only** är aktiverat på Adobe Target-klienten (klientkod) som används på AEM-sidan för att ansluta till Adobe Target, kan du skapa **endast** XT-aktiviteter i AEM.
>
>* Om alternativen **xt_only** **inte** är aktiverade på Adobe Target-klienten (klientkod) kan du skapa **både** XT- och A/B-aktiviteter i AEM.
>
>**Ytterligare information:** **xt_only** -alternativen är en inställning som används för en viss målklient (klientkod) och kan bara ändras direkt i Adobe Target. Du kan inte aktivera eller inaktivera det här alternativet i AEM.

>[!CAUTION]
>
>Skydda aktivitetsinställningsnoden **cq:ActivitySettings** på publiceringsinstansen så att den inte är tillgänglig för vanliga användare. Noden för aktivitetsinställningar ska bara vara tillgänglig för tjänsten som hanterar aktivitetssynkroniseringen till Adobe Target.
>
>Mer information finns i [Förutsättningar för integrering med Adobe Target](/help/sites-administering/target-requirements.md#securingtheactivitysettings).

## Skapa ett varumärke med hjälp av aktivitetskonsolen {#creating-a-brand-using-the-activities-console}

Skapa ett varumärke som ni vill hantera marknadsföringsaktiviteter för.

När du skapar ett varumärke med aktivitetskonsolen visas det också i konsolen [Erbjudanden](/help/sites-authoring/offerlib.md) där du kan skapa erbjudanden för upplevelserna av dina aktiviteter.

1. Klicka på **Personalization** i navigeringskonsolen. Klicka på **Aktiviteter**.

   ![screen_shot_2018-03-21at151821](assets/screen_shot_2018-03-21at151821.png)

1. Klicka på **Skapa** och sedan **Skapa varumärke** i aktivitetskonsolen.
1. Markera varumärkesmallen och klicka på **Nästa**.
1. Skriv en rubrik för varumärket som du vill att det ska visas i aktivitets- och offertkonsolerna. Du kan också ange eller markera en eller flera taggar som ska kopplas till varumärket.
1. Klicka på **Skapa**. Ditt varumärke visas i aktivitetskonsolen.

## Lägga till/redigera en aktivitet med aktivitetskonsolen {#adding-editing-an-activity-using-the-activities-console}

Lägg till en aktivitet eller redigera en befintlig aktivitet för att fokusera era marknadsföringssatsningar på specifika målgrupper. När du skapar/redigerar en aktivitet anger du följande information:

* **Namn:** Namnet på aktiviteten.
* **Målinriktningsmotor:** Antingen [AEM](/help/sites-authoring/personalization.md#aem) eller [Adobe Target](/help/sites-authoring/personalization.md#adobe-target) som motor för målinriktat innehåll.

* **Välj en målkonfiguration:** (Endast Adobe Target) Den molnkonfiguration som den här aktiviteten ska använda för att ansluta till Adobe Target. Det här alternativet visas bara när Adobe Target har valts som målinriktningsmotor.
* **Aktivitetstyp: **Aktivitetstyp - A/B-test eller målinriktad upplevelse
* **Mål:** (Valfritt) En beskrivning av aktiviteten.
* **Upplevelser:** Mappningar mellan målgruppsnamn och de marknadssegment som ni riktar in er på.
* **Trafikprocent:** Om A/B-test är valt kan du ändra hur mycket trafik (i procent) som går till varje upplevelse.
* **Varaktighet:** Tidsperioden då aktiviteten används.
* **Prioritet:** Aktivitetens relativa prioritet. När aktiviteter tillhandahåller innehåll för samma användarsegment har aktiviteten med högre prioritet företräde.
* **Målmått:** Om du väljer Adobe Target som målinriktningsmotor kan du lägga till framgångsmått för aktiviteten. Ett framgångsmått krävs.

>[!NOTE]
>
>Nya Adobe Target-aktiviteter måste ***skapas*** i den innehållsredigerare som angetts som mål, inte i **aktivitetskonsolen**, eftersom synkroniseringen med Adobe Target misslyckas annars.
>
>Du kan dock redigera befintliga Adobe Target-aktiviteter i konsolen.

Så här lägger du till en aktivitet:

1. Klicka på det varumärke som du skapar aktiviteten för, klicka på **Skapa** och sedan på **Skapa aktivitet**. Om du redigerar markerar du aktiviteten och klickar sedan på **Redigera**.
1. Ange följande information och klicka sedan på **Nästa**:

   * Ett namn för aktiviteten.
   * Målmotorn som ska användas. ContextHub (AEM) är valt som standard. Om du behöver använda Adobe Target skapar du aktiviteten i den aktiva innehållsredigeraren.
   * Om du valde Adobe Target som målmotor väljer/redigerar du den molnkonfiguration som ska användas för att ansluta till Adobe Target. (Se till att du inte väljer något ramverk som du har skapat för din molnkonfiguration.)
   * (Valfritt) Syftet med eller en beskrivning av aktiviteten.
   * Välj aktivitetstyp.

1. Lägg till en eller flera upplevelser till aktiviteten. Klicka på **Lägg till upplevelse**.
1. Om ni använder AEM för målinriktning eller Adobe Target för upplevelser:

   1. Klicka på **Välj publik **och välj det segment som upplevelsen ska rikta in sig på.
   1. Klicka på **Lägg till upplevelse**, skriv ett namn och klicka på **OK**.

   1. Klicka på **Nästa**.

   Om du använder Adobe Target A/B-testning:

   1. Klicka på pennan i målgruppsrutan för att välja en målgrupp.
   1. Klicka på **Lägg till upplevelse**, skriv ett namn och klicka på **OK**.

   1. Ange den procentandel av trafiken som visar varje upplevelse.
   1. Klicka på **Nästa**.

1. Om du vill ange när aktiviteten startar använder du den nedrullningsbara menyn **Start** och väljer något av följande värden:

   * **Vid aktivering:** Aktiviteten startar när sidan som innehåller målinnehållet aktiveras.
   * **Angivet datum och tid:** En specifik tid. När du väljer det här alternativet klickar du på kalenderikonen, väljer ett datum och anger vilken tid aktiviteten ska starta.

1. Om du vill ange när aktiviteten slutar använder du den nedrullningsbara menyn Slut och väljer något av följande värden:

   * **Vid inaktivering**: Aktiviteten avslutas när sidan som innehåller målinnehållet inaktiveras.
   * **Angivet datum och tid**: En specifik tid. När du väljer det här alternativet klickar du på kalenderikonen, väljer ett datum och anger vilken tid aktiviteten ska avslutas.

1. Om du vill ange en prioritet för aktiviteten använder du skjutreglaget för att välja antingen **Låg**, **Normal** eller **Hög**.
1. Om du använder Adobe Target som målmotor väljer du vad du vill mäta med den här aktiviteten. Mer information om tillgängliga framgångsmått finns i [Konfigurera aktiviteten och ställa in mål](/help/sites-authoring/content-targeting-touch.md). Välj minst ett mål.
1. Klicka på **Spara**.

   >[!NOTE]
   >
   >När du har skapat en aktivitet måste du publicera den så att den blir tillgänglig.

## Förlags- och avpubliceringsverksamhet {#publishing-and-unpublishing-activities}

Du måste publicera aktiviteter för att göra dem tillgängliga. Omvänt kanske du vill göra aktiviteter otillgängliga genom att avpublicera dem.

>[!NOTE]
>
>När du avpublicerar en aktivitet ändras inte aktivitetens status om du inte uppdaterar sidan.

Så här publicerar eller avpublicerar du aktiviteter:

1. Klicka på varumärket och sedan på det område som innehåller aktiviteten som du vill publicera eller avpublicera.
1. Klicka på ikonen bredvid aktiviteten eller aktiviteterna som du vill publicera eller avpublicera.

   ![screen-shot_2019-03-05at123846](assets/screen-shot_2019-03-05at123846.png)

1. Klicka på **Publicera** om du vill publicera. Om du vill avpublicera klickar du på **Avpublicera**. Dina aktiviteter publiceras eller avpubliceras och deras status ändras i aktivitetskonsolen (kan kräva en uppdatering).

## Aktiviteter för författare och publiceringsinstanser {#activities-on-author-and-publish-instances}

När en aktivitet som använder Adobe Target målmotor aktiveras skapas en andra aktivitet i publiceringsinstansen:

* Aktiviteten på författarinstansen spårar aktivitet på författarinstansen och är användbar för att simulera besökarupplevelsen. De analyser som registreras för den här aktiviteten återspeglar bara vad som händer på författarinstansen.
* Aktiviteten i publiceringsinstansen speglar och svarar på aktiviteten på publiceringsservern. Detta är den aktivitet som körs på den offentliga webbplatsen. Det är bara publiceringsaktiviteten som är relevant för att spåra och analysera användningen av den publika webbplatsen.

## Visningsprestanda och konvertering av vinnande upplevelser (A/B-test) {#viewing-performance-and-converting-winning-experiences-a-b-test}

Du kan se prestanda för alla Adobe Target-aktiviteter (XT eller A/B). Om du använder A/B-testning kan du även konvertera den vinnande upplevelsen, som sedan blir standardupplevelsen.

Så här visar du aktivitetsprestanda och konverterar vinnande upplevelser:

1. I **Personalization** klickar du på **Aktiviteter** för att gå till **aktivitetskonsolen**.
1. Klicka på det varumärke som du vill se aktiviteter för.
1. Markera aktiviteten och klicka på **Visa egenskaper** och klicka på fliken **Rapporter** och välj den aktivitet som du vill visa prestanda för/konvertera vinnande upplevelser för. Resultatdata visas.

   ![chlimage_1-115](assets/chlimage_1-115.png)

1. Klicka på länken **Push winner** om du vill använda den upplevelsen som standard.

   Att konvertera vinnaren gör följande:

   * Det inaktiverar den aktuella aktiviteten
   * Ändrar alla sidor och ersätter målinnehållet med det faktiska innehållet i den vinnande upplevelsen. Innehållet i den vinnande upplevelsen blir en del av den normala sidan **utan** som mål.

   ![chlimage_1-116](assets/chlimage_1-116.png)

   En vinnande upplevelse är den upplevelse som genererar mer Lyft i rapporterna, som baseras på konverteringsgraden.

1. Klicka på **Ja** för att bekräfta att du vill konvertera vinnaren, inaktivera den aktuella upplevelsen och ersätta den med innehållet i den vinnande upplevelsen.

## Synkronisera aktiviteter med Adobe Target {#synchronizing-activities-with-adobe-target}

Aktiviteter som använder Adobe Target målinriktningsmotor synkroniseras med Adobe Target kampanjer. En aktivitet synkroniseras automatiskt till Adobe Target när följande villkor uppfylls:

* Aktiviteten innehåller minst en upplevelse.
* Minst en upplevelse innehåller ett mappat segment och ett erbjudande.
* Varje upplevelse i aktiviteten måste ha samma antal erbjudanden.

Dessa villkor gäller för aktiviteter på författare och publiceringsinstanser.

När en aktivitet synkroniseras skapas en motsvarande kampanj i Adobe Target:

* Aktiviteter i publiceringsinstansen har samma namn som motsvarande Adobe Target-kampanj.
* Aktiviteter på författarinstansen motsvarar målkampanjer med samma namn som suffixet `_author`.

![chlimage_1-117](assets/chlimage_1-117.png)

_författaraktiviteter synkroniseras omedelbart när aktiviteten ändras. Omedelbar synkronisering möjliggör simulering av aktiviteter med Client Context eller ContextHub.

Publiceringsaktiviteterna synkroniseras när aktiviteten publiceras till AEM publiceringsinstans.

## Felsökning av aktivitetssynkronisering {#troubleshooting-activity-synchronization}

När AEM synkroniserar en aktivitet med Adobe Target, innehåller AEM en egenskap för aktiviteten med namnet `thirdPartyId`. Värdet för den här egenskapen baseras på sökvägen för aktiviteten i AEM-databasen. Två kampanjer i Adobe Target kan inte ha samma värde för egenskapen `thirdPartyId`. En aktivitet kan därför inte synkroniseras om en befintlig kampanj (av en annan typ AB, XT) i Adobe Target använder samma värde för `thirdPartyId`.

Denna situation kan uppstå under följande omständigheter:

1. En aktivitet skapas och synkroniseras med Adobe Target.
1. I en annan instans av AEM skapas en aktivitet under samma varumärke och med samma namn. Synkronisering av den här aktiviteten misslyckas vid försök.

Denna situation kan även uppstå under följande omständigheter:

1. En aktivitet skapas och synkroniseras med Adobe Target. Aktiviteten tas sedan bort från AEM.
1. En aktivitet skapas under samma varumärke och använder samma namn som den borttagna aktiviteten. Synkronisering av den här aktiviteten misslyckas vid försök.

Använd alltid unika namn för aktiviteter för att undvika synkroniseringsproblem. Om en aktivitet inte kan synkroniseras kan du ta bort kampanjen i Adobe Target som använder samma namn om kampanjen inte används.

>[!NOTE]
>
>När du skapar en kampanj i Adobe Target tilldelas varje kampanj en egenskap med namnet `thirdPartyId t`. När du tar bort kampanjen i Adobe Target tas inte `thirdPartyId` bort. Du kan inte återanvända `thirdPartyId` för kampanjer av olika typer (AB, XT) och den kan inte tas bort manuellt. För att undvika det här problemet kan du namnge varje kampanj med ett unikt namn. Kampanjnamn kan inte återanvändas i olika kampanjtyper.
>
>Om du använder samma namn i samma kampanjtyp skriver du över den befintliga kampanjen.
>
>Om du får felmeddelandet&quot;Begäran misslyckades&quot; under synkroniseringen. `thirdPartyId` finns redan&quot;, ändra namnet på kampanjen och synkronisera igen.
