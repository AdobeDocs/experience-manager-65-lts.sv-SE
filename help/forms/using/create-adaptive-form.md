---
title: 'Självstudiekurs: Skapa ett anpassat formulär'
description: Lär dig skapa, layouta och förhandsgranska ett anpassat formulär. Lär dig även att konfigurera skicka-åtgärder.
feature: Adaptive Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 87e03ff2-1324-42bd-b4da-54a0c17ce98e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1258'
ht-degree: 0%

---

# Självstudiekurs: Skapa ett anpassat formulär {#do-not-publish-tutorial-create-an-adaptive-form}

![02-create-adaptive-form-main-image](assets/02-create-adaptive-form-main-image.png)

Den här självstudiekursen är ett steg i serien [Create Your First Adaptive Form](/help/forms/using/create-your-first-adaptive-form.md) . Vi rekommenderar att du följer serien i kronologisk ordning för att förstå, utföra och demonstrera det fullständiga självstudiekurserna.

## Om självstudiekursen {#about-the-tutorial}

Adaptiva formulär är nya generationer som är dynamiska och responsiva. Ni kan använda adaptiva formulär för att leverera personaliserade upplevelser. Du kan även integrera adaptiva formulär med [!DNL Adobe Analytics] för användningsstatistik och [!DNL Adobe Campaign] för kampanjhantering. Mer information om funktioner för adaptiva formulär finns i [Introduktion till utveckling av adaptiva formulär](/help/forms/using/introduction-forms-authoring.md).

Det är enklare att skapa och hantera formulär när man följer en lämplig process. I den här artikeln får du lära dig att:

* [Skapa ett anpassat formulär som gör att en kund kan lägga till en leveransadress](/help/forms/using/create-adaptive-form.md#step-create-the-adaptive-form)

* [Layoutfält i ett anpassningsbart formulär som ska visas och accepteras från en kund](/help/forms/using/create-adaptive-form.md#step-add-header-and-footer)

* [Skapa en Skicka-åtgärd för att skicka ett e-postmeddelande med formulärinnehåll](/help/forms/using/create-adaptive-form.md#step-add-components-to-capture-and-display-information)
* [Förhandsgranska och skicka ett anpassat formulär](/help/forms/using/create-adaptive-form.md)

Du kommer att ha ett formulär som liknar följande i slutet av artikeln:\
[![Förhandsgranska formulär i mobilen](do-not-localize/form-preview-mobile.gif)](do-not-localize/form-preview-mobile.gif)

## Steg 1: Skapa det anpassade formuläret {#step-create-the-adaptive-form}

1. Logga in på AEM författarinstans och navigera till **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Forms]** > **[!UICONTROL Forms & Documents]**. Standardwebbadressen är [http://localhost:4502/aem/forms.html/content/dam/formsanddocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
1. Välj **[!UICONTROL Create]** och välj **[!UICONTROL Adaptive Form]**. Ett alternativ för att välja en mall visas. Markera mallen **[!UICONTROL Blank]** för att markera den och välj **[!UICONTROL Next]**.

1. Ett alternativ för **[!UICONTROL Add Properties]** visas. Fälten **[!UICONTROL Title]** och **[!UICONTROL Name]** är obligatoriska:

   * **Titel:** Ange `Add new or update shipping address` i fältet **[!UICONTROL Title]**. Titelfältet anger formulärets visningsnamn. Titeln hjälper dig att identifiera formuläret i användargränssnittet för AEM [!DNL Forms].
   * **Namn:** Ange `shipping-address-add-update-form` i fältet **[!UICONTROL Name]**. Fältet Namn anger formulärets namn. En nod med det angivna namnet skapas i databasen. När du börjar skriva en titel genereras värdet för namnfältet automatiskt. Du kan ändra det föreslagna värdet. Namnfältet får endast innehålla alfanumeriska tecken, bindestreck och understreck. Alla ogiltiga indata ersätts med ett bindestreck.

1. Välj **[!UICONTROL Create]**. Ett anpassat formulär skapas och en dialogruta öppnas där du kan öppna formuläret för redigering. Välj **[!UICONTROL Open]** om du vill öppna det nya formuläret på en ny flik. Formuläret öppnas för redigering. Här visas också sidlisten där du kan anpassa det nya formuläret efter behov.

   Mer information om gränssnittet för att skapa adaptiva formulär och tillgängliga komponenter finns i [Introduktion till att skapa adaptiva formulär](/help/forms/using/creating-adaptive-form.md).

   ![Ett anpassat formulär har skapats.](assets/newly-created-adaptive-form.png)

## Steg 2: Lägg till sidhuvud och sidfot {#step-add-header-and-footer}

AEM [!DNL Forms] innehåller många komponenter som du kan använda för att visa information i ett anpassat formulär. Komponenterna för sidhuvud och sidfot ger formuläret ett enhetligt utseende och en enhetlig känsla. En rubrik innehåller vanligtvis ett företags logotyp, formulärets rubrik och sammanfattning. En sidfot innehåller vanligtvis copyrightinformation och länkar till andra sidor.

1. Välj ![växlingspanel](assets/toggle-side-panel.png) > ![treexpandall](assets/treeexpandall.png). Komponentwebbläsaren öppnas. Dra **[!UICONTROL Header]**-komponenten från komponentwebbläsaren till det adaptiva formuläret.
1. Välj **[!UICONTROL Logo]**. Verktygsfältet visas. Välj ![aem_6_3_edit](assets/aem_6_3_edit.png) i verktygsfältet, skriv **We.Retail** och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

1. Välj Bild. Verktygsfältet visas. Välj ![cmppr](assets/cmppr.png). Egenskapsläsaren öppnas till vänster på skärmen. **[!UICONTROL Browse]** och överför logotypbilden. Välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png). Bilden visas i sidhuvudet.

   Du kan välja Hämta fil för att hämta logotypen som används i den här artikeln om du inte har någon.

[Hämta fil](assets/logo.png)

1. Dra **[!UICONTROL Footer]**-komponenten från ![treeexpandall](assets/treeexpandall.png) till det adaptiva formuläret. I det här skedet ser formuläret ut så här:

   ![adaptive-form-with-headers-and-footers](assets/adaptive-form-with-headers-and-footers.png)

## Steg 3: Lägga till komponenter för att hämta och visa information {#step-add-components-to-capture-and-display-information}

Komponenter är byggstenar i en adaptiv form. AEM [!DNL Forms] innehåller många komponenter för att hämta och visa information i en adaptiv form. Du kan dra komponenterna från ![treexpandall](assets/treeexpandall.png) till ett formulär. Mer information om tillgängliga komponenter och motsvarande funktioner finns i [Introduktion till utveckling av adaptiva formulär](/help/forms/using/introduction-forms-authoring.md).

1. Dra **[!UICONTROL Numeric Box component]** till det adaptiva formuläret. Placera den före sidfotskomponenten. Öppna komponentens egenskaper, ändra **[!UICONTROL Title]** för komponenten till **`Customer ID`**, ändra **[!UICONTROL Element Name]** till **`customer_ID`**, aktivera alternativet **[!UICONTROL Required Field]**, aktivera alternativet **[!UICONTROL Use HTML5 Number Input Type]** och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).
1. Dra tre textrutekomponenter till det adaptiva formuläret. Placera dessa före sidfotskomponenten. Ange följande egenskaper för dessa textrutor.:

   <table> 
    <tbody> 
     <tr> 
      <td><b>Egenskap</b></td> 
      <td><b>Textruta 1<br/></b></td> 
      <td><b>Textruta 2<br/></b></td> 
      <td><b>Textruta 3</b></td> 
     </tr> 
     <tr> 
      <td>Titel</td> 
      <td>Namn <br /> </td> 
      <td>Leveransadress</td> 
      <td>Läge</td> 
     </tr> 
     <tr> 
      <td>Elementnamn</td> 
      <td>customer_Name<br /> </td> 
      <td>customer_Shipping_Address</td> 
      <td>customer_State</td> 
     </tr> 
     <tr> 
      <td>Obligatoriskt fält</td> 
      <td>Aktiverad</td> 
      <td>Aktiverad</td> 
      <td>Aktiverad</td> 
     </tr> 
     <tr> 
      <td>Tillåt flera rader <br /> </td> 
      <td>Handikappade</td> 
      <td>Aktiverad</td> 
      <td>Handikappade</td> 
     </tr> 
    </tbody> 
   </table>

1. Dra en **[!UICONTROL Numeric Box]**-komponent före sidfotskomponenten. Öppna komponentens egenskaper, ange värden i tabellen nedan, välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   | Egenskap | Värde |
   |---|---|
   | Titel | Postnummer |
   | Elementnamn | customer_ZIPCode |
   | Maximalt antal siffror | 6 |
   | Obligatoriskt fält | Aktiverad |
   | Visa mönstertyp | Inget mönster |

1. Dra en **[!UICONTROL Email]**-komponent före sidfotskomponenten. Öppna komponentens egenskaper, ange värden i tabellen nedan och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   | Egenskap | Värde |
   |---|---|
   | Titel | E-post |
   | Elementnamn | customer_Email |
   | Obligatoriskt fält | Aktiverad |

1. Dra en **[!UICONTROL File Attachment]**-komponent före sidfotskomponenten. Öppna komponentens egenskaper, ange värden i tabellen nedan och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   <table> 
    <tbody> 
     <tr> 
      <td><b>Egenskap</b></td> 
      <td><b>Värde</b></td> 
     </tr> 
     <tr> 
      <td>Titel</td> 
      <td>Adressbevis som godkänts av myndigheter<br /> </td> 
     </tr> 
     <tr> 
      <td>Elementnamn</td> 
      <td>customer_Address_Proof</td> 
     </tr> 
     <tr> 
      <td>Obligatoriskt fält</td> 
      <td>Aktiverad</td> 
     </tr> 
    </tbody> 
   </table>

1. Dra en **[!UICONTROL Submit Button]**-komponent till det adaptiva formuläret. Placera den före sidfotskomponenten. Öppna komponentens egenskaper, ändra elementnamn till `address_addition_update_submit`, välj ![&#x200B; aem_6_3_forms_save](assets/aem_6_3_forms_save.png). Formulärets layout är fullständig och formuläret ser ut så här:

   ![adaptive-form-with-all-the-components](assets/adaptive-form-with-all-the-components.png)

## Steg 4: Konfigurera skicka-åtgärd för det adaptiva formuläret {#step-configure-submit-action-for-the-adaptive-form}

En skicka-åtgärd aktiveras när en användare trycker på Skicka-knappen på ett anpassat formulär. Du kan använda en skicka-åtgärd för att spara formulärdata i den lokala databasen, skicka formulärdata till en REST-slutpunkt, skicka formulärdata som e-post och mycket mer. Adaptiva formulär innehåller några fler färdiga åtgärder för att skicka. Mer information finns i [Konfigurera åtgärden Skicka](/help/forms/using/configuring-submit-actions.md).

Med hjälp av följande steg kan du konfigurera e-poståtgärd för att skicka och demo för formuläret:

1. Konfigurera e-postservern. Mer information finns i [Konfigurera e-postmeddelande](/help/sites-administering/notification.md).


1. Välj **[!UICONTROL Form Container]** i innehållsläsaren och välj ![cmpr](assets/cmppr.png). Egenskapsläsaren öppnas till vänster.
1. Gå till **[!UICONTROL Submission]** > **[!UICONTROL Submit Action]**. Välj **[!UICONTROL Send Email]**. Ange följande värden och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   | Egenskap | Värde |
   |--- |--- |
   | Från | `donotreply@weretail.com` |
   | Till | `${customer_Email}` |
   | Ämne | Bekräftelse: Du har lagt till leveransadress på webbplatsen We.Retail. |
   | E-postmall | Hej `${customer_Name}`! Följande adress läggs till som leveransadress för ditt konto: <br>`${customer_Name}`, `${customer_Shipping_Address}`, `${customer_State}`, `${customer_ZIPCode}`<br> Med vänlig hälsning, We.Retail |
   | Inkludera bifogade filer | Aktiverad |

   Formuläret är klart. Nu kan du förhandsgranska formuläret och testa funktionen. Om du har använt namnet som nämns i självstudiekursen och har åtkomst till formuläret på den dator där AEM [!DNL Forms]-servern körs, finns formuläret på [http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html](http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html).

## Steg 5: Förhandsgranska och skicka det anpassade formuläret {#step-preview-and-submit-the-adaptive-form}

Du kan använda **[!UICONTROL Preview option]** för att utvärdera ett formulärs utseende och beteende. Du kan skicka ett formulär i förhandsgranskningsläge och även kontrollera valideringar som används i ett formulär. Om till exempel ett fel visas när ett obligatoriskt fält lämnas tomt.

Med adaptiva formulär kan du också emulera upplevelsen av ett formulär för olika enheter. Exempel: iPhone, iPad och Desktop. Du kan använda alternativen **[!UICONTROL Preview]** och **[!UICONTROL Emulator]** ![linjal](assets/ruler.png) tillsammans för att förhandsgranska ett formulär för enheter med olika skärmstorlekar.

1. Välj alternativet **[!UICONTROL Preview]** till höger om formulärredigeraren. Formuläret öppnas i förhandsgranskningsläget. Om du har använt namnet som nämns i självstudiekursen är URL:en för förhandsgranskning av formuläret [http://localhost:4502/content/dam/formsanddocuments/shipping-address-add-update-form/jcr:content?wcmmode=disabled](http://localhost:4502/content/dam/formsanddocuments/shipping-address-addition-updation-form/jcr:content?wcmmode=disabled)
1. Använd ![linjal](assets/ruler.png) om du vill visa hur formuläret ser ut på olika enheter.
1. Fyll i formulärfälten och välj **[!UICONTROL Submit]**. Formuläret har skickats och du omdirigeras till standardsidan **Tack**. Du kan också ange en anpassad tacksida. Mer information finns i [Konfigurera omdirigeringssidan](/help/forms/using/configuring-redirect-page.md).

Det adaptiva formuläret för att lägga till en adress är klart. Om du har använt det namn som anges i självstudiekursen och har åtkomst till formuläret på den dator där AEM Forms-servern körs, finns formuläret på [http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html](http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html).
