---
title: Konfigurera segmentering
description: Lär dig konfigurera segmentering för AEM Campaign.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Administering,Personalization
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1128'
ht-degree: 0%

---


# Konfigurera segmentering {#configuring-segmentation}

>[!NOTE]
>
>I det här dokumentet beskrivs konfigurationen av segmentering så som den används med klientkontexten. Information om hur du konfigurerar segment med ContextHub med hjälp av pekgränssnittet finns i [Konfigurera segmentering med ContextHub](/help/sites-administering/segmentation.md).

Segmentering är en viktig faktor när man skapar en kampanj. Mer information om hur segmentering fungerar och nyckeltermer finns i [Segmenteringsordlista](/help/sites-authoring/segmentation-overview.md).

Beroende på den information du redan har samlat in om webbplatsbesökarna och vilka mål du vill uppnå, måste du definiera de segment och strategier som behövs för målinnehållet.

Dessa segment används sedan för att förse en besökare med specifikt riktat innehåll. Det här innehållet bevaras i avsnittet [Kampanjer](/help/sites-classic-ui-authoring/classic-personalization-campaigns.md) på webbplatsen. Teaser pages defined here can be included as teaser paragraphs on any page and define which visitor segment the specialized content is applicable for.

Med AEM kan ni enkelt skapa och uppdatera segment, teasers och kampanjer. Du kan även kontrollera resultatet av dina definitioner.

Med **segmentredigeraren** kan du enkelt definiera ett segment:

![Segmentredigeringsfönstret](assets/segmenteditor.png)

Du kan **redigera** för varje segment om du vill ange en **titel**-, **beskrivning**- och **förstärkningsfaktor**. Med hjälp av sidosparken kan du lägga till **AND**- och **OR**-behållare för att definiera **segmentlogiken** och sedan lägga till de **segmentegenskaper** som krävs för att definiera urvalskriterierna.

## Förstärkningsfaktor {#boost-factor}

Varje segment har en **Förstärkningsparameter** som används som viktningsfaktor. Ett högre värde anger att segmentet väljs före ett segment med ett lägre värde.

* Minsta värde: `0`
* Högsta värde: `1000000`

## Segmentlogik {#segment-logic}

Följande logikbehållare är tillgängliga när du vill och du kan skapa logiken för ditt segmentval. De kan dras från sidosparken till redigeraren:

<table>
 <tbody>
  <tr>
   <td> OCH-behållare<br /> </td>
   <td> Den booleska AND-operatorn.<br /> </td>
  </tr>
  <tr>
   <td> ELLER-behållare<br /> </td>
   <td> Operatorn boolesk OR.</td>
  </tr>
 </tbody>
</table>

## Segmentegenskaper {#segment-traits}

Följande segmentegenskaper är tillgängliga när som helst. De kan dras från sidosparken till redigeraren:

<table>
 <tbody>
  <tr>
   <td> IP-intervall <br /> </td>
   <td>Definierar ett intervall med IP-adresser som besökaren kan ha.<br /> </td>
  </tr>
  <tr>
   <td> Sidträffar <br /> </td>
   <td>Hur ofta sidan har begärts. <br /> </td>
  </tr>
  <tr>
   <td> Sideegenskap <br /> </td>
   <td>Alla egenskaper för den besökta sidan.<br /> </td>
  </tr>
  <tr>
   <td> Referensnyckelord <br /> </td>
   <td>Nyckelord som matchar information från den refererande webbplatsen. <br /> </td>
  </tr>
  <tr>
   <td> Skript</td>
   <td>JavaScript-uttryck som ska utvärderas.<br /> </td>
  </tr>
  <tr>
   <td> Segmentreferens <br /> </td>
   <td>Referens till en annan segmentdefinition.<br /> </td>
  </tr>
  <tr>
   <td> Tagg Cloud<br /> </td>
   <td>Taggar som ska matchas med taggar från besökta sidor.<br /> </td>
  </tr>
  <tr>
   <td> Användarålder<br /> </td>
   <td>Som hämtat från användarprofilen.<br /> </td>
  </tr>
  <tr>
   <td> Användaregenskap <br /> </td>
   <td>Annan information som är tillgänglig i användarprofilen. </td>
  </tr>
 </tbody>
</table>

Du kan kombinera dessa egenskaper med hjälp av de booleska operatorerna OR och AND (se [Skapa ett nytt segment](#creating-a-new-segment)) för att definiera det exakta scenariot för markering av det här segmentet.

När hela programsatsen utvärderas till true är det här segmentet löst. Om det finns flera tillämpliga segment används även **[Förstärkningsfaktorn](/help/sites-administering/campaign-segmentation.md#boost-factor)**.

>[!CAUTION]
>
>Segmentredigeraren söker inte efter några cirkelreferenser. Segmentet A refererar till exempel till ett annat segment B, som i sin tur refererar till segment A. Se till att dina segment inte innehåller några cirkelreferenser.

>[!NOTE]
>
>Egenskaper med suffixet **_i18n** anges av ett skript som är en del av personaliseringens användargränssnittsklient. Alla användargränssnittsrelaterade klienter läses bara in på författaren eftersom användargränssnittet inte behövs vid publicering.
>
>När du skapar ett segment med sådana egenskaper är det därför normalt nödvändigt att förlita sig på **browserFamily** i stället för **browserFamily_i18n**.

### Skapa ett nytt segment {#creating-a-new-segment}

Så här definierar du det nya segmentet:

1. Välj **Verktyg > Åtgärder > Konfiguration** i fältet.
1. Klicka på sidan **Segmentering** i den vänstra rutan och navigera till önskad plats.
1. Skapa en [ny sida](/help/sites-authoring/editing-content.md#creatinganewpage) med mallen **Segment**.
1. Öppna den nya sidan och se segmentredigeraren:

   ![Det första steget i att skapa ett segment i segmentredigeraren](assets/screen_shot_2012-02-02at101726am.png)

1. Använd sidosparken eller snabbmenyn (oftast högerklickning med musknappen och välj sedan **Nytt...** för att öppna fönstret Infoga ny komponent) för att hitta den segmentegenskap du behöver. Dra den sedan till **segmentredigeraren** som den kommer att visas i standardbehållaren **AND**.
1. Dubbelklicka på den nya egenskapen för att redigera de specifika parametrarna, till exempel musens position:

   ![Redigera en komponent i segmentredigeraren](assets/screen_shot_2012-02-02at103135am.png)

1. Klicka på **OK** för att spara definitionen:
1. Du kan **redigera** segmentdefinitionen och ge den en **rubrik**, **beskrivning** och **[förstärkning](#boost-factor)**:

   ![Redigera segmentinställningarna i segmentredigeraren](assets/screen_shot_2012-02-02at103547am.png)

1. Lägg till fler egenskaper om det behövs. Du kan formulera booleska uttryck med komponenterna **AND Container** och **OR Container** som finns under **Segmentlogik**. Med segmentredigeraren kan du ta bort egenskaper eller behållare som inte längre behövs, eller dra dem till nya positioner i programsatsen.

### Använda OCH- och ELLER-behållare {#using-and-and-or-containers}

Du kan skapa komplexa segment i AEM. Man bör vara medveten om några grundläggande punkter:

* Definitionens översta nivå är alltid den AND-behållare som skapas från början. Detta kan inte ändras, men påverkar inte resten av segmentdefinitionen.
* Se till att det är rimligt att kapsla behållaren. Behållarna kan ses som parenteser i ditt booleska uttryck.

Följande exempel används för att välja besökare som antingen är:

Man och mellan 16 och 65 år

ELLER

Kvinnor och mellan 16 och 62 år

Som huvudoperator är OR måste du börja med en **OR-behållare**. I det här exemplet har du två AND-programsatser, för vart och ett av dem behöver du en **AND-behållare**, där du kan lägga till de enskilda egenskaperna.

![Ett exempel på AND- och OR-operatorer i segmentredigeraren](assets/screen_shot_2012-02-02at105145am.png)

## Testa tillämpningen av ett segment {#testing-the-application-of-a-segment}

När segmentet har definierats kan potentiella resultat testas med hjälp av **[klientkontexten](/help/sites-administering/client-context.md)**:

1. Välj det segment som ska testas.
1. Tryck på **[Ctrl-Alt-C](/help/sites-authoring/page-authoring.md#keyboardshortcuts)** för att öppna **[klientkontexten](/help/sites-administering/client-context.md)** som visar de data som har samlats in. I testsyfte kan du **redigera** vissa värden eller **läsa in** en annan profil för att se effekten där.

1. Beroende på vilka egenskaper som har definierats, kanske data som är tillgängliga för den aktuella sidan inte matchar segmentdefinitionen. Status för matchningen visas under definitionen.

En enkel segmentdefinition kan till exempel baseras på användarens ålder och kön. När du läser in en viss profil visas att segmentet har lösts:

![Använda fönstret Klientkontext för att testa en AND-segmenteringsåtgärd](assets/screen_shot_2012-02-02at105926am.png)

Eller inte:

![Använda fönstret Klientkontext för att testa en NOT-segmenteringsåtgärd](assets/screen_shot_2012-02-02at110019am.png)

>[!NOTE]
>
>Alla egenskaper åtgärdas omedelbart, men de flesta ändras bara vid sidinläsning. Ändringar av musens position visas omedelbart, vilket är praktiskt vid testning.

Sådana tester kan även utföras på innehållssidor och i kombination med **Teaser** -komponenter.

Om du för musen över ett teaser-stycke visas de segment som används, oavsett om de för närvarande löses och varför den aktuella teaser-instansen har valts:

![Ett exempel på muspekaren över ett segment](assets/chlimage_1-47.png)

### Använda ditt segment {#using-your-segment}

Segment används för närvarande inom [kampanjer](/help/sites-classic-ui-authoring/classic-personalization-campaigns.md). De används för att styra det faktiska innehåll som ses av specifika målgrupper. Mer information finns i [Om segment](/help/sites-authoring/segmentation-overview.md).
