---
title: Integrera landningssidor med Adobe Analytics
description: Lär dig integrera landningssidor med Adobe Analytics.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: 24ab494d-4a11-408e-8dc0-de16508edfac
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 1%

---

# Integrera landningssidor med Adobe Analytics{#integrating-landing-pages-with-adobe-analytics}

AEM har integrerat landningssidornas lösning med [Adobe Analytics](https://www.omniture.com/en/products/analytics/sitecatalyst) genom att använda följande CTA-komponenter:

1. Klicka genom komponent
1. Komponent för grafisk länk

Dessa komponenter visar vissa attribut som kan mappas via Adobe Analytics-variabler (trafik, konverteringsvariabler) och success-händelser för att skicka information till Adobe Analytics.

## Förutsättningar {#prerequisites}

Adobe rekommenderar att du går igenom den [befintliga AEM-Adobe Analytics-integreringen](/help/sites-administering/adobeanalytics.md) för att förstå hur den här integreringen fungerar.

## Komponenter tillgängliga för mappning {#components-available-for-mapping}

I AEM kan **Call to Action**-komponenterna - **ClickThroughLink** och **GraphicalLink** - som visas här i sidosparken mappas till Adobe Analytics-variabler.

![chlimage_1-21](assets/chlimage_1-21a.jpeg)

### Mappa landningssidkomponenter till Adobe Analytics {#mapping-landing-page-components-to-adobe-analytics}

Så här mappar du landningssidans komponenter till Adobe Analytics:

1. När du har skapat Adobe Analytics-konfigurationen och skapat ett ramverk väljer du lämplig rapporteringssvit i listrutan. Detta leder till att Adobe Analytics-variablerna hämtas och visas i innehållssökaren.
1. Dra och släpp CTA-komponenter (Call to Action) från sidosparken till mappningsområdet mitt på sidan.

<table>
 <tbody>
  <tr>
   <td><strong>Komponentnamn</strong></td>
   <td><strong>Exponerade attribut</strong></td>
   <td><strong>Betydelse av attribut</strong></td>
  </tr>
  <tr>
   <td><strong>CTA Click Through Link</strong></td>
   <td><i>eventdata.clickthroughLinkLabel</i> <br /> </td>
   <td>Etiketten på länken eller texten i länken </td>
  </tr>
  <tr>
   <td><br type="_moz" /> </td>
   <td><i>eventdata.clickthroughLinkTarget</i> <br /> </td>
   <td>Destinationen du tar när du klickar på länken </td>
  </tr>
  <tr>
   <td><br type="_moz" /> </td>
   <td><i>eventData.events.clickthroughLinkClick</i> <br /> </td>
   <td>Klickhändelsen </td>
  </tr>
  <tr>
   <td><strong>CTA Graphical Link</strong></td>
   <td><i>eventData.clicktroughImageLabel</i> <br /> </td>
   <td>The title of the CTA image </td>
  </tr>
  <tr>
   <td><br type="_moz" /> </td>
   <td><i>eventData.clicktroughImageTarget</i> <br /> </td>
   <td>Målet dit du är tagen när du klickar på bilden som innehåller en länk</td>
  </tr>
  <tr>
   <td><br type="_moz" /> </td>
   <td><i>eventData.clicktroughImageAsset</i> <br /> </td>
   <td>Sökvägen till bildresursen i databasen </td>
  </tr>
  <tr>
   <td><br type="_moz" /> </td>
   <td><i>eventData.events.clicktroughImageClick</i> <br /> </td>
   <td>Klickhändelsen</td>
  </tr>
 </tbody>
</table>

1. Mappa dessa exponerade attribut med eventuella Adobe Analytics-variabler från innehållssökaren. Ramverket är nu klart att användas.
1. Nu kan du skapa en landningssida eller öppna en befintlig landningssida med befintliga CTA-komponenter och klicka på fliken **Molntjänster** i **Sidegenskaper** i den vänstra knappen (i det pekoptimerade användargränssnittet väljer du **Öppna egenskaper** och klickar på **Molntjänster**) och konfigurerar ramverket för landningssidan. Välj ramverket i listrutan.

   ![chlimage_1-25](assets/chlimage_1-25a.png)

1. När du har konfigurerat ramverket med landningssidan kan du nu använda de instrumenterade komponenterna och alla klick på CTA registreras i Adobe Analytics.
