---
title: Textbunden formatering av adaptiva formulärkomponenter
description: Du kan använda anpassade format på ett anpassat formulär, men du kan också använda infogade CSS-egenskaper på enskilda komponenter i ett anpassat formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: c074afbb-33d1-4424-bbd2-2acdeeebe9d0
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 0%

---

# Textbunden formatering av adaptiva formulärkomponenter {#inline-styling-of-adaptive-form-components}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/configure-layout-of-an-adaptive-form/inline-style-adaptive-forms.html).

Du kan definiera det övergripande utseendet och formatet för ett anpassat formulär genom att ange format med [temaredigeraren](../../forms/using/themes.md). Du kan också använda infogade CSS-format på enskilda adaptiva formulärkomponenter och förhandsgranska ändringarna direkt. Inline-format åsidosätter format som finns i temat.

## Använda infogade CSS-egenskaper {#apply-inline-css-properties}

Så här lägger du till infogade format i en komponent:

1. Öppna formuläret i formulärredigeraren och ändra läget till formateringsläge. Om du vill ändra läge till formateringsläge väljer du ![listrutan Arbetsyta](assets/canvas-drop-down.png) > **Format** i sidverktygsfältet.
1. Markera en komponent på sidan och markera redigeringsknappen ![edit-button](assets/edit-button.png). Stilegenskaper öppnas i sidofältet.

   Du kan också välja komponenter från formulärhierarkiträdet i sidlisten. Formulärhierarkiträdet är tillgängligt som formulärobjekt i sidlisten.

   Du kan också välja en komponent i sidofältet. I stilläget kan du se komponenter som listas under Formulärobjekt. Formulärobjektslistan i sidofältet visar dock komponenter som fält och paneler. Fält och paneler är generiska komponenter som kan innehålla komponenter som textrutor och alternativknappar.

   När du väljer en komponent i sidofältet visas alla underkomponenter i listan och egenskaperna för den markerade komponenten. Du kan markera en viss underkomponent och formatera den.

1. Klicka på en flik i sidofältet för att ange CSS-egenskaper. Du kan ange egenskaper som:

   * Mått och position (visningsinställning, utfyllnad, höjd, bredd, marginal, position, z-index, flyttal, klar, spill)
   * Text (teckensnittsfamilj, vikt, färg, storlek, radhöjd och justering)
   * Bakgrund (bild och övertoning, bakgrundsfärg)
   * Kant (bredd, stil, färg, radie)
   * Effekter (skugga, opacitet)
   * Avancerat (Gör att du kan skriva anpassad CSS för komponenten)

1. På samma sätt kan du använda format för andra delar av en komponent, till exempel widget, bildtext och Hjälp.
1. Välj **Klar** för att bekräfta ändringarna eller **Avbryt** för att ignorera ändringarna.

## Exempel: textbundna format för en fältkomponent {#example-inline-styles-for-a-field-component}

Följande bilder visar ett textfält före och efter att infogade format har använts på det.

![Textrutekomponent innan intern formatering används](assets/no-style.png)

Textrutekomponent innan infogade formategenskaper används

Lägg märke till ändringen i textrutans format, så som visas i följande bild, när du har använt följande CSS-egenskaper.

<table>
 <tbody>
  <tr>
   <td><p>Väljare</p> </td>
   <td><p>CSS-egenskap</p> </td>
   <td><p>Värde</p> </td>
   <td><p>Effekt</p> </td>
  </tr>
  <tr>
   <td><p>Fält</p> </td>
   <td><p>border</p> </td>
   <td><p>Kantbredd =2px</p> <p>Kantstil=Heldragen</p> <p>Kantfärg=#1111</p> </td>
   <td><p>Skapar en svart 2px bred kant runt fältet</p> </td>
  </tr>
  <tr>
   <td><p>Textruta</p> </td>
   <td><p>background-color</p> </td>
   <td><p>#6495ED</p> </td>
   <td><p>Ändrar bakgrundsfärgen till CornflowerBlue (#6495ED)</p> <p>Obs! Du kan ange ett färgnamn eller dess hexadecimala kod i värdefältet.</p> </td>
  </tr>
  <tr>
   <td><p>Etikett</p> </td>
   <td><p>Mått och position &gt; bredd</p> </td>
   <td><p>100px</p> </td>
   <td><p>Fastställer bredden som 100px för etiketten</p> </td>
  </tr>
  <tr>
   <td>Ikon för fälthjälp</td>
   <td>Text &gt; Teckenfärg</td>
   <td>#2ECC40</td>
   <td>Ändrar färgen på hjälpikonens ansikte.</td>
  </tr>
  <tr>
   <td><p>Lång beskrivning</p> </td>
   <td><p>textjustering</p> </td>
   <td><p>centrera</p> </td>
   <td><p>Justerar den långa beskrivningen för att få hjälp att centrera</p> </td>
  </tr>
 </tbody>
</table>

![Textrutans format efter infogad formatering.](assets/applied-style.png)

Textrutekomponent efter användning av egenskaper för infogat format

Följ stegen ovan för att markera och formatera andra komponenter, till exempel paneler, skicka-knappar och alternativknappar.

>[!NOTE]
>
>Stilegenskaperna varierar beroende på vilken komponent du väljer.
