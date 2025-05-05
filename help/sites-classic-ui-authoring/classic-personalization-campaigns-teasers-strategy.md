---
title: Lärare och strategier
description: Kampanjerna använder ofta teasers som en mekanism för att locka ett visst segment av besökspopulationen till innehåll som fokuserar på deras intressen. En eller flera lärare definieras för en viss kampanj.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization
role: User
exl-id: 3232ccb0-dd4c-4457-9467-cdad788f977c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 1%

---

# Lärare och strategier{#teasers-and-strategies}

Kampanjerna använder ofta teasers som en mekanism för att locka ett visst segment av besökspopulationen till innehåll som fokuserar på deras intressen. En eller flera lärare definieras för en viss kampanj.

>[!NOTE]
>
>Teaser-komponenten är nu borttagen i AEM 6.2. Använd i stället [målkomponenten](/help/sites-authoring/content-targeting-touch.md).

* **Varumärkessidor** lagras i kampanjavsnittet på webbplatsen. Ett varumärke innehåller de enskilda kampanjerna.
* **Kampanjsidor** lagras i avsnittet Kampanjer på webbplatsen. Varje kampanj har en egen sida, där de mer detaljerade definitionerna finns. Behållaren, eller översikten, innehåller också viss information och statistik om de enskilda sidorna för teaser.

Teaser inom AEM består av flera delar:

* **Teaser-sidor** lagras under rätt kampanjsida och innehåller definitioner för de Teaser-stycken som är tillgängliga för varje specifik kampanj. De här definitionerna används när de teaserskiktade styckena visas, inklusive innehållsvariationer, det segment som ska användas för att markera en variant och förstärkningsfaktor.
* **Teaser-komponenten** är tillgänglig direkt och gör att du kan skapa en instans av ditt specifika teaser-stycke på en innehållssida. Du kan dra laserkomponenten från sidosparken och sedan ange din laserdefinition för att skapa ett eget teaser-stycke. **Obs!** Teaser-komponenten är nu inaktuell i AEM 6.2. Använd i stället komponenten [ Target ](/help/sites-authoring/content-targeting-touch.md) .
* **Teaser paragraphs** är faktiska instanser av teaser på en innehållssida. Dessa locka fram ett segment av besökare till innehåll som fokuserar på deras intressen.
* Sidor där kampanjinnehållet är inriktat på ett specifikt besökarsegment. Vanligtvis leder de smalare styckena besökaren till sådana sidor.

## Strategier {#strategies}

När du lägger till ett steg på en sida måste du definiera **strategin**.

Detta gäller om flera scener är tillgängliga för markering när deras tilldelade segment kan matchas. **Strategin** anger sedan ett extra villkor som används för att välja det teaser som visas:

* **Klickströmsbakgrundsmusik** baseras på de taggar och relaterade taggträffar som finns i besökarens klientkontext (visa hur ofta en besökare har klickat på sidor som innehåller respektive tagg). Träffarna för de taggar som definieras på scensidan jämförs.
* **Slumpmässig**, för slumpmässig markering. Den slumpmässiga faktorn som genereras för en sida används. Detta kan ses med [klientkontexten](/help/sites-administering/client-context.md).
* **Första** i listan över lösta segment. Ordningen är densamma som för teasers på kampanjbehållarsidan.

Segmentets [Förstärkningsfaktor](/help/sites-administering/campaign-segmentation.md#boost-factor) påverkar även markeringen. Detta är en viktningsfaktor som läggs till i en segmentdefinition för att öka eller minska den relativa sannolikheten för att den väljs.

Processen och de inbördes förhållandena mellan de olika urvalskriterierna illustreras bäst med ett exempel (en metod som också kan användas för att säkerställa att dina lärare når rätt målgrupp).

Om följande segment redan har skapats och tilldelats respektive startfaktor:

| Segment | Förstärkningsfaktor |
|---|---|
| S1 | 0 |
| S2 | 0 |
| S3 | 10 |
| S4 | 30 |
| S5 | 0 |
| S6 | 100 |

Och vi använder följande definitioner av teaser:

<table>
 <tbody>
  <tr>
   <td>Campaign</td>
   <td>Teaser</td>
   <td>Tilldelade segment</td>
   <td>Tilldelade taggar </td>
  </tr>
  <tr>
   <td>C1</td>
   <td>T1</td>
   <td>S1, S2</td>
   <td>Näringsliv, marknadsföring</td>
  </tr>
  <tr>
   <td>C1</td>
   <td>T2 </td>
   <td>S1</td>
   <td><br /> </td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T3</td>
   <td>S3, S4</td>
   <td><br /> </td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T4</td>
   <td>S2, S5</td>
   <td><br /> </td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T5</td>
   <td>S1, S2, S6</td>
   <td>Marknadsföring</td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T6</td>
   <td>S6</td>
   <td>Företag <br /> </td>
  </tr>
 </tbody>
</table>

Om vi sedan tillämpar detta på en besökare där:

* **S1**, **S2 och &#x200B;** S6** har matchats

* taggen **marketing** har tre träffar
* taggen **business** har sex träffar

Vi kan se resultatet:

* matchar framgång - kan något av de segment som tilldelats till teaser matchas för den aktuella besökaren?
* förstärkningsfaktor - den högsta förstärkningsfaktorn för alla tillämpliga segment
* clickstream score - den ackumulerade summan för alla tillämpliga taggträffar

som beräknas innan lämplig strategi tillämpas:

<table>
 <tbody>
  <tr>
   <td>Campaign</td>
   <td>Teaser</td>
   <td>Tilldelade segment</td>
   <td>Taggar </td>
   <td>Lyckad matchning?</td>
   <td>Resultatförstärkningsfaktor</td>
   <td>Resultat av Clickstream-bakgrundsmusik </td>
  </tr>
  <tr>
   <td>C1</td>
   <td>T1</td>
   <td>S1, S2</td>
   <td>Näringsliv, marknadsföring</td>
   <td>Ja</td>
   <td>0</td>
   <td>9</td>
  </tr>
  <tr>
   <td>C1</td>
   <td>T2 </td>
   <td>S1</td>
   <td><br /> </td>
   <td>Ja</td>
   <td>0</td>
   <td><br /> </td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T3</td>
   <td>S3, S4</td>
   <td><br /> </td>
   <td>Nej</td>
   <td><br /> </td>
   <td><br /> </td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T4</td>
   <td>S2, S5</td>
   <td><br /> </td>
   <td>Ja<br /> </td>
   <td>0<br /> </td>
   <td><br /> </td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T5</td>
   <td>S1, S2, S6</td>
   <td>Marknadsföring</td>
   <td>Ja</td>
   <td>100</td>
   <td>3</td>
  </tr>
  <tr>
   <td>C1 </td>
   <td>T6</td>
   <td>S6</td>
   <td>Företag</td>
   <td>Ja</td>
   <td>100</td>
   <td>6 </td>
  </tr>
 </tbody>
</table>

De här värdena används för att avgöra vilka steg besökaren ska se, beroende på vilken **strategi** som tillämpas på det underordnade stycket:

<table>
 <tbody>
  <tr>
   <td>Strategi</td>
   <td>Teaser</td>
   <td>Kommentar</td>
  </tr>
  <tr>
   <td>Första</td>
   <td>T5</td>
   <td>Endast T5 och T6 betraktas som deras segment, som alla löser <i> och</i> som har den högsta förstärkningsfaktorn. Den returnerade listan är i ordningen T5, T6, så T5 markeras och visas.</td>
  </tr>
  <tr>
   <td>Slumpmässig</td>
   <td>T5 eller T6</td>
   <td>Båda teasers har segment som alla löser sig och samma förstärkningsfaktor. De två teasrarna visas därför i samma proportion.</td>
  </tr>
  <tr>
   <td>ClickStream-bakgrundsmusik</td>
   <td>T6</td>
   <td><p>Segmenten för T1, T4, T5 och T6 kan alla lösas för besökaren. De högre förstärkningsfaktorerna för T5 och T6 utesluter sedan T1 och T4. Det högre klickströms-poängen i T6 resulterar slutligen i att detta väljs.</p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Om det efter upplösningsteknikerna ovan finns flera mätare tillgängliga för markering kommer ett internt urval (slumpmässigt) att markera ett enskilt teaser för visning.
>
>Om strategin till exempel var Clickstream-bakgrundsmusik och T5 hade samma Clickstream-bakgrundsmusik som T6 (d.v.s. sex i stället för tre) skulle det interna urvalet (random) användas för att välja en av dessa båda.

Teaser Pages/Paragraphs används för att styra specifika besökarsegment till innehåll som är inriktat på deras intressen. De kan presentera en rad alternativ som besökaren kan välja mellan, eller visa endast ett steg baserat på det specifika besökarsegmentet. Det streckade stycket kan t.ex. vara beroende av besökarens ålder.

Vanligtvis är en teaser-sida en tillfällig åtgärd som varar en viss tid tills den ersätts av nästa steg.

När ni har skapat ert varumärke och er kampanj kan ni skapa och skapa en läroupplevelse.

### Skapa en kontaktyta för din Teaser {#creating-a-touchpoint-for-your-teaser}

>[!NOTE]
>
>Teaser-komponenten är nu borttagen i AEM 6.2. Använd i stället [målkomponenten](/help/sites-authoring/content-targeting-touch.md).

1. Navigera till innehållssidan där du vill placera det steg som ska leda till kampanjsidan.
1. Lägg till en **Teaser**-komponent (tillgänglig i sektionen **Personalization** i sidosparken) i önskad position. När kampanjen skapas visas att kampanjsökvägen inte har konfigurerats ännu:

   ![chlimage_1](assets/chlimage_1.png)

1. Redigera teaserkomponenten för att lägga till:

   * **Kampanjsökväg**
Vägen till kampanjsidan som innehåller den enskilda steg-sidan; segmenten avgör exakt vilken teaser som visas.

   * **[Strategi](/help/sites-classic-ui-authoring/classic-personalization-campaigns.md#strategies)**
Metod som används för markering när flera segment har matchats.

   ![chlimage_1-1](assets/chlimage_1-1.png)

1. Klicka på **OK** för att spara. Beroende på vilka segment du har angett för teaser och profilen för den användare du är inloggad som, visas rätt innehåll:

   ![chlimage_1-2](assets/chlimage_1-2.png)

1. För musen över det trasiga stycket för att visa frågetecknet (komponentens nedre högra hörn). Klicka här om du vill visa de segment som används och om de för närvarande löses.

   ![chlimage_1-3](assets/chlimage_1-3.png)

### Teaser Overview {#teaser-overview}

Förutom kampanjvyn i MCM ger kampanjsidan även information om de lärare som är kopplade till den:

1. Öppna kampanjsidan från konsolen **Webbplatser**, till exempel:

   `https://localhost:4502/content/campaigns/geometrixx-outdoors/storefront/summer.html`

   Här visas en översikt över definitioner och visningsstatistik för teaser:

   ![chlimage_1-4](assets/chlimage_1-4.png)
