---
title: Metodtips för e-postmallar
description: Lär dig de bästa sätten att skapa e-postmallar i AEM.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration, best-practices
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: 34678cae-3c7f-4c9f-b7b6-c1dd4e0758ad
index: false
source-git-commit: 2edf37c2d6bb04b418618f2780f773ab37559114
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 0%

---


# Metodtips för e-postmallar {#best-practices-for-email-templates}

>[!CAUTION]
>
>Den här artikeln gäller de inaktuella Foundation Components-baserade e-postkomponenterna för AEM.
>
>Användare uppmuntras att använda de moderna e-postkomponenterna för [kärnkomponenter.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/email/introduction.html?lang=sv-SE)

I det här dokumentet beskrivs några av de bästa sätten att arbeta med e-postdesign, vilket resulterar i en välutvecklad mall för e-postkampanjer.

Demokampanjen som finns i AEM följer alla dessa bästa metoder. Hur de bästa metoderna implementeras i demokampanjen beskrivs för varje bästa praxis.

Använd dessa rutiner när du skapar ett eget nyhetsbrev.

>[!NOTE]
>
>Allt kampanjinnehåll ska skapas under en `master`-sida av typen `cq/personalization/components/ambitpage`.
>
>Om er planerade kampanjstruktur till exempel är något liknande
>
>`/content/campaigns/teasers/en/campaign-promotion-global`
>
>Kontrollera att den finns under en `master`-sida
>
>`/content/campaigns/teasers/master/en/campaign-promotion-global`

>[!NOTE]
>
>När du skapar en e-postmall för Adobe Campaign måste du inkludera egenskapen **acMapping** med värdet **mapRecipient** i **jcr:content** -noden i mallen. Om du inte gör det kan du inte välja Adobe Campaign-mallen i **Sidegenskaper** i Experience Manager (fältet är inaktiverat).

## Komponenten Mall/sida {#template-page-component}

***/libs/mcm/campaign/components/campaign_newsletterpage***

<table>
 <tbody>
  <tr>
   <td><strong>Bästa praxis</strong></td>
   <td><strong>Implementering</strong></td>
  </tr>
  <tr>
   <td><p>Ange dokumenttyp så att återgivningen blir konsekvent.</p> <p>Lägg till DOCTYPE i början (HTML eller XHTML)</p> </td>
   <td><p>Kan konfigureras genom att ändra egenskapen <i>cq:doctype</i> i <i>/etc/designs/default/jcr:content/campaign_newsletterpage </i></p> <p>Standardvärdet är "XHTML":</p> <p>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional/EN" "https://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;</p> <p>Kan ändras till "HTML_5":</p> <p>&lt;!DOCTYPE HTML&gt;</p> </td>
  </tr>
  <tr>
   <td><p>Ange en teckendefinition så att du ser till att specialtecken återges korrekt.</p> <p>Lägg till CHARSET-deklaration (till exempel iso-8859-15, UTF-8) till &lt;head&gt;</p> </td>
   <td><p>Är inställd på UTF-8.</p> <p>&lt;meta http-equiv="content-type" content="text/html; charset=UTF-8"&gt;</p> </td>
  </tr>
  <tr>
   <td><p>Koda all struktur med &lt;table&gt;elementet. För mer komplicerade layouter bör du kapsla tabeller för att skapa komplexa strukturer.</p> <p>E-post ska se bra ut även utan CSS.</p> </td>
   <td><p>Tabeller används i hela mallen för att strukturera innehåll. Använder för närvarande maximalt fyra kapslade tabeller (1 bastabell + max). 3 kapslingsnivåer)</p> <p>&lt;div&gt;-taggar används bara i redigeringsläge för att säkerställa korrekt komponentredigering.</p> </td>
  </tr>
  <tr>
   <td>Använd elementattribut (till exempel cellfyllnad, justering och bredd) för att ange tabelldimensioner. Den här metoden tvingar en box-model-struktur.</td>
   <td><p>Alla tabeller innehåller nödvändiga attribut som <i>border</i>, <i>cellpadding</i>, <i>cellspacing</i> och <i>width</i>.</p> <p>För att harmonisera elementplaceringen i tabeller har alla tabellceller attributet <i>valign="top"</i> angetts.</p> </td>
  </tr>
  <tr>
   <td><p>Konto för mobilvänlighet, om möjligt. Använd mediefrågor om du vill öka textstorleken på små skärmar, ange träffområden i ministorlek för länkar.</p> <p>Gör ett e-postmeddelande responsivt om designen tillåter det.</p> </td>
   <td>När det gäller CSS-format som används för att illustrera demodesign används mediefrågor för att erbjuda en mobilvänlig version.</td>
  </tr>
  <tr>
   <td>Inline CSS är bättre än att placera all CSS i början.</td>
   <td><p>För att bättre demonstrera den underliggande HTML-strukturen och förenkla möjligheten att anpassa nyhetsbrevsstrukturen har endast vissa CSS-definitioner infogats.</p> <p>Basformat och mallvarianter har extraherats till ett formatblock på &lt;head&gt; på sidan. När nyhetsbrevet är färdigt infogas dessa CSS-definitioner i HTML. En automatisk infogningsmekanism planeras, men är för närvarande inte tillgänglig.</p> </td>
  </tr>
  <tr>
   <td>Gör din CSS enkel. Undvik sammansatta formatdeklarationer, kodförkortningar, CSS-layoutegenskaper, komplexa väljare och pseudoelement.</td>
   <td>När det gäller CSS-format som används för att illustrera demodesign följs CSS-rekommendationerna.</td>
  </tr>
  <tr>
   <td>E-postmeddelanden ska vara högst 600-800 pixlar breda. Den här storleksändringen gör att de beter sig bättre i den storlek på förhandsgranskningsfönstret som många klienter har.</td>
   <td>Innehållstabellens <i>bredd</i> är begränsad till 600 pixlar i demodesign.</td>
  </tr>
 </tbody>
</table>

### Bilder {#images}

/libs/mcm/campaign/components/image

| **Bästa praxis** | **Implementering** |
|---|---|
| Lägg till *alt*-attribut i bilder | Attributet *alt* har definierats som obligatoriskt för bildkomponenten. |
| Använd formatet *jpg* i stället för formatet *png* för bilder | Bilderna hanteras alltid som JPG av bildkomponenten. |
| Använd elementet `<img>` i stället för bakgrundsbilder i en tabell. | Inga bakgrundsbilddata används i mallarna. |
| Lägg till attributet style=&quot;display block&quot; i bilder. På så sätt kan de visas bra på Gmail. | Alla bilder innehåller attributet *style=&quot;display block&quot;* som standard. |

### Text och länkar {#text-and-links}

/libs/mcm/campaign/components/heading, /libs/mcm/campaign/components/textimage

<table>
 <tbody>
  <tr>
   <td><strong>Bästa praxis</strong></td>
   <td><strong>Implementering</strong></td>
  </tr>
  <tr>
   <td>Använd html &lt;font&gt; i stället för format i CSS (font-family)</td>
   <td>RichTextEditor (till exempel i textimagekomponenten) har nu stöd för att välja och använda teckensnittsfamiljer och teckensnittsstorlekar på markerade texter. De återges som &lt;font&gt;-taggar.</td>
  </tr>
  <tr>
   <td>Använd grundläggande plattformsoberoende teckensnitt som <i>Arial®, Verdana, Georgia</i> och <i>Times New Roman®</i>.</td>
   <td><p>Används för att skapa nyhetsbrev.</p> <p>För demodesignen används teckensnittet"Helvetica®", men det återgår till ett generiskt sans-serif-teckensnitt, om det inte finns något.</p> </td>
  </tr>
 </tbody>
</table>

### Allmän {#generic}

| **Bästa praxis** | **Implementering** |
|---|---|
| Använd W3C-valideraren för att korrigera HTML-koden. Se till att alla öppna taggar stängs ordentligt. | Koden har validerats. För XHTML Transition Doctype saknas det saknade xmlns-attributet för elementet `<html>`. |
| Undvik att använda JavaScript eller Flash - dessa tekniker stöds ofta inte av e-postklienter. | JavaScript eller Flash används inte i nyhetsbrevmallen. |
| Lägg till en oformaterad textversion för att skicka flera delar. | En ny widget skapades i sidegenskaperna för att enkelt extrahera en textversion från sidinnehållet. Du kan använda den som startpunkt för den slutliga versionen av plaintext. |

## Mallar och exempel för kampanjnyhetsbrev {#campaign-newsletter-templates-and-examples}

AEM innehåller flera mallar och komponenter som du kan använda för att skapa nyhetsbrev om kampanjer. Du kan använda dessa mallar och komponenter för att skapa anpassade nyhetsbrev.

### Mallar {#templates}

Det finns tre något olika malltyper som är tillgängliga direkt när du vill erbjuda en heltäckande bas och bredda möjligheterna för innehållsflöde. Du kan enkelt använda dessa tre typer för att skapa anpassade nyhetsbrev.

Alla har ett **sidhuvud**, en **sidfot** och ett **brödavsnitt**. Under brödavsnittet skiljer sig varje mall i **kolumndesign** (en, två eller tre kolumner).

![Varianter på möjliga nyhetsbrev](assets/chlimage_1-69.png)

### Komponenter {#components}

Det finns för närvarande [sju komponenter som kan användas i kampanjmallar](/help/sites-authoring/adobe-campaign-components.md). De här komponenterna är alla baserade på Adobe markeringsspråk **HTML**.

| **Komponentnamn** | **Komponentsökväg** |
|---|---|
| Rubrik | /libs/mcm/campaign/components/heading |
| Bild | /libs/mcm/campaign/components/image |
| Text&amp;Personalization | /libs/mcm/campaign/components/personalization |
| Textimage | /libs/mcm/campaign/components/textimage |
| Länk | /libs/mcm/campaign/components/reference |
| Dynamic Media Classic (tidigare Scene7) Bildmall | /libs/mcm/campaign/s7image |
| Riktad referens | /libs/mcm/campaign/components/reference |

>[!NOTE]
>
>De här komponenterna är optimerade för e-postinnehåll, vilket innebär att de följer den bästa praxis som beskrivs i det här dokumentet. Om du använder andra komponenter som inte finns installerade i systemet bryter det vanligtvis mot dessa regler.

De här komponenterna beskrivs i detalj i [Adobe Campaign-komponenter](/help/sites-authoring/adobe-campaign-components.md).
