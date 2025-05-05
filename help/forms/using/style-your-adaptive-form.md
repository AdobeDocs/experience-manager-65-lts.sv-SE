---
title: Formatera ditt anpassningsbara formulär
description: Lär dig att skapa ett anpassat tema, formatera enskilda komponenter och använda Web Fonts i ett tema.
topic-tags: introduction
feature: Adaptive Forms
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 5b3beaa6-ca0a-454e-85ee-c3653dd423fe
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1948'
ht-degree: 0%

---

# Formatera ditt anpassningsbara formulär {#do-not-publish-style-your-adaptive-form}

Lär dig att skapa ett anpassat tema, formatera enskilda komponenter och använda Web Fonts i ett tema.

![hjältebild](do-not-localize/08-style_your_adaptiveformmain.png)

Den här självstudiekursen är ett steg i serien [Create Your First Adaptive Form](https://helpx.adobe.com/se/experience-manager/6-3/forms/using/create-your-first-adaptive-form.html) . Adobe rekommenderar att du följer serien i kronologisk ordning för att förstå, utföra och demonstrera det fullständiga självstudiekurserna.

## Om självstudiekursen  {#about-the-tutorial}

Du kan använda teman för att ge ett anpassat formulär ett unikt utseende och en unik stil. Du kan använda färdiga teman som medföljer den anpassningsbara formulärredigeraren eller skapa egna teman. AEM [!DNL Forms] tillhandahåller en [temaredigerare](https://helpx.adobe.com/se/experience-manager/6-3/forms/using/themes.html) för att skapa anpassade teman. Ett och samma tema kan ge olika utseenden till samma adaptiva formulär som öppnas på mobilen, surfplattan eller datorn. Du behöver inte ha någon tidigare kunskap om CSS eller LESS för att kunna använda temaredigeraren, men du vill ha den.

I slutet av självstudiekursen bör du kunna göra följande:

* Använda ett tema i ett anpassat formulär
* Skapa ett tema för anpassningsbara formulär med temaredigeraren
* Formatera enskilda komponenter
* Bonusavsnitt: Använd Web Fonts i ett anpassat tema

Formuläret ska se ut ungefär så här när du är klar med självstudiekursen:

![Formulär med anpassat tema](assets/styled-adaptive-form.png)

## Innan du börjar {#before-you-start}

Ladda ned rubrikbilderna och logotypbilderna nedan på din dator. Rubriken för det anpassade formuläret `shipping-address-add-update-form` använder rubrikstilen och logotypbilderna. Bilden i sidhuvudsstil visas till höger om sidhuvudet.

[Hämta fil](assets/header-style.png)

[Hämta fil](assets/logo-1.png)

## Steg 1: Använd ett tema i det anpassade formuläret {#step-apply-a-theme-to-your-adaptive-form}

Adaptiv formulärredigerare har flera färdiga teman. Om du inte tänker använda en anpassad stil för ditt adaptiva formulär kan du även publicera dina adaptiva formulär med ett användbart tema. Temana är oberoende av anpassningsbara formulär. Du kan använda samma tema för flera adaptiva formulär.

**Så här använder du ett tema i ditt adaptiva formulär:**

1. Öppna det adaptiva formuläret för redigering.

   [http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html](http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html)

1. Öppna egenskaper för **[!UICONTROL Adaptive Form container]**. Navigera till **[!UICONTROL Basic]** > **[!UICONTROL Adaptive Form Theme]** i egenskapsläsaren. I fältet **[!UICONTROL Adaptive Form Theme]** visas alla färdiga och anpassade teman. Som standard används Canvas-temat.
1. Välj ett tema i fältet **[!UICONTROL Adaptive Form Theme]**. Exempel: **Undersökningstema**. Välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) så att du kan använda det valda temat.

   ![Anpassat formulär med standardtemat](assets/default-adaptive-form.png)

   **Figur:** *Anpassat formulär med standardtemat*

   ![Anpassat formulär med undersökningstemat](assets/adaptive-form-with-survey-theme.png)

   **Figur:** *Anpassat formulär med undersökningstemat*

## Steg 2: Uppdatera ditt anpassningsbara formulär {#step-update-your-adaptive-form}

Den design som visas ovan kräver ändringar i platshållartext och logotyp i ditt befintliga adaptiva formulär.

**Så här uppdaterar du ditt adaptiva formulär:**

1. Ändra den befintliga logotypen och texten för sidhuvudet. Så här tar du bort logotypen:

   1. Öppna formuläret i formulärredigeraren.

      [http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html](http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html)

   1. Välj logotypbild i komponenten [!UICONTROL header] och välj ![cmpr](assets/cmppr.png) **[!UICONTROL properties]**. I egenskapen [!UICONTROL image] väljer du X för att ta bort den befintliga logotypbilden.
   1. Välj **[!UICONTROL upload]**, välj logo.png och välj ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att spara ändringarna. Bilden hämtades i avsnittet [Innan du börjar](/help/forms/using/style-your-adaptive-form.md#before-you-start).
   1. Markera rubriktext, `We.Retail`, och välj ![ aem_6_3_edit](assets/aem_6_3_edit.png) **[!UICONTROL edit]**. Ändra rubriktext till `we retail`. Använd endast fet stil på `we` i `we retail`.

      ![we-retail-logo-text](assets/we-retail-logo-text.png)

1. Ta bort rubrik och lägg till platshållartext:

   1. Markera fältet Kund-ID och välj ![cmpr](assets/cmppr.png) -egenskaper.
   1. Kopiera innehållet i fältet **[!UICONTROL Title]** till fältet **[!UICONTROL Placeholder Text]**.
   1. Ta bort innehållet i fältet **[!UICONTROL Title]** och välj ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png).
   1. Upprepa de tre föregående stegen för alla textrutor, numeriska rutor och e-postfält i formuläret.

      ![updated-adaptive-form](assets/updated-adaptive-form.png)

## Steg 3: Skapa ett anpassat tema för ditt anpassade formulär {#step-create-a-custom-theme-for-your-adaptive-form}

Du kan använda [temaredigeraren](/help/forms/using/themes.md) för att skapa anpassade teman. Temaeditorn är en kraftfull WYSIWYG-editor. Det är en visuell metod att använda CSS på olika komponenter i ett adaptivt formulär. Den ger finare kontroller för att formatera komponenter och paneler i ett adaptivt formulär.

Ett tema är en separat enhet som anpassningsbara formulär. Den innehåller format (CSS) för komponenterna och panelerna i ett anpassat formulär. Formaten innehåller CSS-egenskaper som bakgrundsfärger, lägesfärger, genomskinlighet, justering och storlek. När du använder ett tema används det angivna formatet på motsvarande komponenter i ett anpassat formulär.

I den här självstudiekursen formaterar du sidhuvud och sidfot, text och numeriska komponenter, bilagekomponenter och knappar. Låt oss börja med att skapa ett tema:

### Skapa ett tema {#create-a-theme}

1. Logga in på AEM författarinstans och navigera till **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Forms]** > **[!UICONTROL Themes]**. Standardwebbadressen är [http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-themes](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-themes).
1. Välj **[!UICONTROL Create]** och välj **[!UICONTROL Theme]**. Sidan [!UICONTROL Create Theme] med de fält som krävs för att skapa ett tema visas. Fälten **[!UICONTROL Title]** och **[!UICONTROL Name]** är obligatoriska:

   * **Titel:** Ange temats titel. Exempel: **Global Theme.** Titeln hjälper dig att identifiera temat från listan med teman.
   * **Namn:** Ange namnet på temat. Exempel: **Global-Theme.** En nod med det angivna namnet skapas i databasen. När du börjar skriva en titel genereras namnfältets värde automatiskt. Du kan ändra det föreslagna värdet. Namnfältet får endast innehålla alfanumeriska tecken, bindestreck och understreck. Alla ogiltiga indata ersätts med ett bindestreck.

1. Välj **[!UICONTROL Create]**. Ett tema skapas och en dialogruta visas där du kan öppna formuläret för redigering. Välj **[!UICONTROL Open]** om du vill öppna det nya temat på en ny flik. Temat öppnas i temaredigeraren. För formatering används ett anpassat formulär som levereras med AEM [!DNL Forms].

   Mer information om hur du använder gränssnittet för temaredigeraren finns i [Om temaredigeraren](/help/forms/using/themes.md#aboutthethemeeditor).

1. Välj **[!UICONTROL Theme Options]** ![temaalternativ](assets/theme-options.png) > **[!UICONTROL Configure]**. I fältet **[!UICONTROL Preview Form]** markerar du det adaptiva formuläret **shipping-address-add-update-form**, väljer ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png) och väljer **[!UICONTROL Save]**. Nu är temaredigeraren konfigurerad att använda ditt eget adaptiva formulär i stället för det adaptiva standardformatet. Välj **[!UICONTROL Cancel]** om du vill gå tillbaka till temaredigeraren.

   ![anpassat tema](assets/custom-theme.png)

   **Figur:** *Temeredigeraren med anpassat formulär för leverans-address-add-update-form*

   ![create-a-theme](assets/create-a-theme.png)

   **Figur:** *Anpassat formulär med standardformuläret*

### Formatera sidhuvud och sidfot {#style-header-and-footer}

Sidhuvud och sidfot ger ett konsekvent och distinkt utseende i en adaptiv form. I allmänhet innehåller sidhuvudet organisationens logotyp och namn, sidfoten innehåller copyrightinformation och dessa är identiska i flera former i en organisation. Så här formaterar du sidhuvud och sidfot i anpassat formulär för leverans-address-add-update-form:

1. Navigera till alternativet **[!UICONTROL Header]** > **[!UICONTROL Text]** på panelen Väljare. Väljarpanelen är till vänster om temaredigeraren. Om panelen inte visas väljer du ![växlingspanel](assets/toggle-side-panel.png) växlingspanel.

1. Ange följande egenskaper i dragspelet **[!UICONTROL Text]** och välj ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   | Egenskap | Värde |
   |---|---|
   | Font Family | Arial® |
   | Teckenfärg | FFFFFF |
   | Teckenstorlek | 54 px |

1. Markera [!UICONTROL header]-widgeten och välj **[!UICONTROL Header]**. Alternativen för att formatera sidhuvudswidgeten visas till vänster. Expandera dragspelet **[!UICONTROL Dimensions & Position]**, ange **[!UICONTROL Height]** till `120px` och välj ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png).
1. Expandera dragspelswidgeten **[!UICONTROL Background]** och ställ in **[!UICONTROL Background Color]** på `F6921E.`

   Hovra över **[!UICONTROL Image & Gradient]** > **[!UICONTROL + Add]**, välj **[!UICONTROL Image]**. Ange följande egenskaper och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   | Egenskap | Värde |
   |---|---|
   | image | Överför header-style.png. Bilden hämtades i avsnittet [Innan du börjar](/help/forms/using/style-your-adaptive-form.md#before-you-start). |
   | Position | Höger nerifrån |
   | Passram | Ingen upprepning |

1. Välj logotypen i rubriken i temaredigeraren och välj **[!UICONTROL Header Logo]**. Expandera dragspelet Dimensioner och position, ange följande egenskaper och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   <table> 
    <tbody> 
     <tr> 
      <td><b>Marginal</b></td> 
      <td><b>Värde</b></td> 
     </tr> 
     <tr> 
      <td>Marginal</td> 
      <td> 
       <ul> 
        <li>Överkant: 1,5 rem</li> 
        <li>Nederkant: -35px</li> 
        <li>Vänster: 1rem<strong><br /> </strong></li> 
       </ul> <p><strong>Tips!</strong> Välj länkikonen <img src="assets/link.png"> om du vill ange olika värden för varje fält.<br /> </p> </td> 
     </tr> 
     <tr> 
      <td>Höjd</td> 
      <td>4,75rem</td> 
     </tr> 
    </tbody> 
   </table>

1. Markera sidfotswidgeten och välj **[!UICONTROL Footer]**. Expandera dragspelet **[!UICONTROL Background]**, ange **[!UICONTROL Background Color]** till `F6921E` och välj ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

### Formatera datainhämtningskomponenten och använd en bakgrund i det anpassade formuläret {#style-the-data-capture-component-and-apply-a-background-to-the-adaptive-form}

Du kan använda flera komponenter i ett adaptivt formulär för att hämta data. Till exempel textruta och numerisk ruta. Du kan ange ett identiskt format för alla datainhämtningskomponenter eller ett separat format för varje komponent. I den här självstudiekursen används ett identiskt format för numeriska rutor (Kund-ID, Postnummer) och textrutor (Kund-ID, Namn, Leveransadress, Tillstånd, E-post). Så här formaterar du komponenter för datainhämtning:

1. Markera fältet **[!UICONTROL Customer ID]** och välj alternativet **[!UICONTROL Field Widget]**. Ange följande egenskaper och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   <table> 
    <tbody> 
     <tr> 
      <td><b>Dragspel</b></td> 
      <td><b>Egenskap</b></td> 
      <td><b>Värde</b></td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantfärg</td> 
      <td>A7A9AC</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantradie </td> 
      <td> 
       <ul> 
        <li>Överkant: 7 px<br /> </li> 
        <li>Höger: 7 px<br /> </li> 
        <li>Nederkant: 7 px<br /> </li> 
        <li>Vänster: 7 px<br /> </li> 
       </ul> </td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Font Family</td> 
      <td>Arial®</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Teckenfärg</td> 
      <td>939598<br /> </td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Teckenstorlek</td> 
      <td>18 px</td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Bredd</td> 
      <td>60 %</td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Marginal</td> 
      <td> 
       <ul> 
        <li>Vänster: 10 rem</li> 
       </ul> </td> 
     </tr> 
    </tbody> 
    </table>

1. Markera det tomma området ovanför fältet **[!UICONTROL Customer ID]** och välj **[!UICONTROL Responsive Panel Container]**. Ange **[!UICONTROL Background]** > **[!UICONTROL Background Color]** som F1F2F2. Välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   ![Behållare för responsiv panel](do-not-localize/responsive-panel-container.png)

### Formatera knapparna {#style-the-buttons}

Du kan använda ett anpassat tema för att tillämpa ett identiskt format på alla knappar i det adaptiva formuläret och [intern formatering](/help/forms/using/inline-style-adaptive-forms.md) för att tillämpa ett format på en viss knapp. Så här formaterar du knapparna:

1. Markera knappen **[!UICONTROL Submit]** och välj alternativet **[!UICONTROL Button]**. Ange följande egenskaper och välj ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png).

   <table> 
    <tbody> 
     <tr> 
      <td><b>Dragspel</b></td> 
      <td><b>Egenskap</b></td> 
      <td><b>Värde</b></td> 
     </tr> 
     <tr> 
      <td>Bakgrund</td> 
      <td>Bakgrundsfärg</td> 
      <td>F6921E</td> 
     </tr> 
     <tr> 
      <td>Kant <br /> </td> 
      <td>Kantfärg</td> 
      <td>F6921E</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantradie </td> 
      <td> 
       <ul> 
        <li>Överkant: 7 px<br /> </li> 
        <li>Höger: 7 px<br /> </li> 
        <li>Nederkant: 7 px<br /> </li> 
        <li>Vänster: 7 px</li> 
       </ul> </td> 
     </tr> 
     <tr> 
      <td>Text <br /> </td> 
      <td>Font Family</td> 
      <td>Arial®</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Teckenfärg</td> 
      <td>FFFFFF</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Teckenstorlek</td> 
      <td>18 px</td> 
     </tr> 
    </tbody> 
   </table>

1. [Använd det anpassade temat ](/help/forms/using/style-your-adaptive-form.md#step-apply-a-theme-to-your-adaptive-form), Global tema, på ditt adaptiva formulär. Om formatet inte återspeglas i det adaptiva formuläret rensar du webbläsarcachen och försöker igen.

   ![style-data-capture-components](assets/style-data-capture-components.png)

## Steg 4: Formatera enskilda komponenter {#step-style-individual-components}

Vissa format gäller bara för en viss komponent. Sådana komponenter är formaterade i en adaptiv formulärredigerare.

1. Öppna det adaptiva formuläret för redigering. [http://localhost:4502/editor.html/content/forms/af/shipping-address-add-update-form.html](http://localhost:4502/editor.html/content/forms/af/change-billing-shipping-address.html)
1. Välj alternativet **[!UICONTROL Style]** i det övre fältet.

   ![style-option](assets/style-option.png)

1. Markera knappen **[!UICONTROL Attach]** och välj ikonen ![aem_6_3_edit](assets/aem_6_3_edit.png) . Ange följande egenskaper i dragspelet **[!UICONTROL Dimensions and Position]**:

   | Egenskap | Värde |
   |---|---|
   | Float | Vänster |
   | Bredd | 10 % |

1. Markera alternativet **[!UICONTROL Government approved address proof]** och välj ikonen ![aem_6_3_edit](assets/aem_6_3_edit.png) . Ange följande egenskaper:

   <table> 
    <tbody> 
     <tr> 
      <td><b>Dragspel</b></td> 
      <td><b>Egenskap</b></td> 
      <td><b>Värde</b></td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Float</td> 
      <td>Vänster</td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Bredd</td> 
      <td>73 %</td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Utfyllnad</td> 
      <td> 
       <ul> 
        <li>Vänster: 10 px</li> 
       </ul> </td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Höjd</td> 
      <td>40 px</td> 
     </tr> 
     <tr> 
      <td>Dimensioner och position <br /> </td> 
      <td>Marginal</td> 
      <td><br /> 
       <ul> 
        <li>Höger: 2 rem</li> 
        <li>Vänster: 10 rem </li> 
       </ul> </td> 
     </tr> 
     <tr> 
      <td>Bakgrund</td> 
      <td>Bakgrundsfärg</td> 
      <td>FFFFFF</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantbredd</td> 
      <td>1 px</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantstil</td> 
      <td>Heldragen</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantfärg</td> 
      <td>A7A9AC</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantradie</td> 
      <td>7 px</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Font Family</td> 
      <td>Arial®</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Teckenfärg</td> 
      <td>BCBEC0</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Teckenstorlek</td> 
      <td>18 px</td> 
     </tr> 
     <tr> 
      <td>Text</td> 
      <td>Radhöjd</td> 
      <td>2</td> 
     </tr> 
     </tr> 
    </tbody> 
   </table>

1. Markera knappen **[!UICONTROL Submit]** och välj ikonen ![aem_6_3_edit](assets/aem_6_3_edit.png) . Ange följande egenskaper:

   <table> 
    <tbody> 
     <tr> 
      <td><b>Dragspel</b></td> 
      <td><b>Egenskap</b></td> 
      <td><b>Värde</b></td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Float</td> 
      <td>Höger</td> 
     </tr> 
     <tr> 
      <td>Mått och position</td> 
      <td>Marginal</td> 
      <td> 
       <ul> 
        <li>Överkant: 5 rem</li> 
        <li>Höger: 14 rem</li> 
        <li>Nederkant: 20 px</li> 
        <li>Vänster: 20 px<br /> </li> 
       </ul> </td> 
     </tr> 
     <tr> 
      <td>Bakgrund</td> 
      <td>Bakgrundsfärg</td> 
      <td>F6921E</td> 
     </tr> 
     <tr> 
      <td>Kant</td> 
      <td>Kantfärg</td> 
      <td>F6921E</td> 
     </tr> 
    </tbody> 
   </table>

   ![styled-adaptive-form-1](assets/styled-adaptive-form-1.png)

## Steg 5: Bonusavsnitt: Använda Web Fonts i ett anpassat tema {#step-bonus-section-using-web-fonts-in-a-custom-theme}

Du kan använda olika teckensnitt för att utforma ett anpassat formulär. Alla enheter som det adaptiva formuläret visas på kanske inte har de teckensnitt som används för att utforma det adaptiva formuläret. Du kan använda en webbteckensnittstjänst för att leverera nödvändiga teckensnitt till målenheten.

[!DNL Adobe Fonts] är en Web Fonts-tjänst. Du kan konfigurera och använda tjänsten med adaptiva formulär. Så här använder du [!DNL Adobe Fonts] i en adaptiv form:
1. Bläddra i [biblioteket med Adobe-teckensnitt](https://fonts.adobe.com/) och välj teckensnitt för att formatera formuläret.
<!--
>[!NOTE]
>
>![typekit-to-adobe-fonts](assets/typekit-to-adobe-fonts.png) [!DNL Typekit] is now called Adobe Fonts and is included with Creative Cloud and other subscriptions. [Learn more](https://fonts.adobe.com/).-->

>[!NOTE]
>
> Du kan lägga till taggar eller filter för att förfina teckensnittslistan.

1. Klicka på knappen &lt;/> för att lägga till familjen i ett webbprojekt, om du skulle hitta ett typsnitt du gillar.

   ![select-font-from-font-libary](assets/select-font-from-font-library.png)

   Dialogrutan Lägg till teckensnitt i ett webbprojekt visas.

   >[!NOTE]
   >
   > Du kan bara lägga till teckensnitt i ditt webbprojekt om knappen &lt;/> är tillgänglig.

2. Ge webbprojektet ett namn.
3. Markera kryssrutorna för de teckensnittsvikter och format som du vill använda.

   ![lägg till ett teckensnittsbibliotek](assets/add-a-font-window.png)

4. Välj **Klicka på** för att skapa projektet.
5. Kopiera inbäddningskoden och URL-adressen från skärmen.
   ![bädda in kod och URL](assets/font-add-url.png)

6. Klicka på **Klar** för att stänga webbprojektsfönstret.
7. Logga in på din AEM-instans och gå till URL `http://server:port/crx/de/index.jsp#`
8. Skapa en mappstruktur i CRXDE, till exempel `/apps/[fontslibrary]/[customlibrary(clientlibrary)]`.
9. Gå till den nyligen skapade mappen `clientlibs` och lägg till egenskaperna `allowProxy` och `categories`.
10. Navigera till `/apps/[fontslibrary]/[customlibrary(clientlibrary)]` och skapa en css-mapp.
11. Gå till den skapade CSS-mappen och skapa en fil. Skapa till exempel en fil som `fonts.css` och klistra in inbäddningskoden tillsammans med URL:en.
    ![Mappstruktur](/help/forms/using/assets/fonts-add-in-crxde.png)
12. Spara ändringarna.

>[!NOTE]
>
> Om du vill använda de tillagda anpassade teckensnitten i ett adaptivt formulär måste du se till att klientbiblioteksnamnet i **[!UICONTROL Client Library Category]** justeras mot namnet som anges i kategorialternativet i mappen clientlib.

De medföljande teckensnitten är nu tillgängliga för det adaptiva formuläret via följande anpassade teckensnittsklientbibliotek.


<!--
Create Adobe Fonts Configuration

1. To create a API Token, go to **login** > **API Token** > **Make me a new API token**.

   ![API token](/help/forms/using/assets/fonts-api-token.png)

2. Once, you click **Make me a new API token**, a new token is generated. 
3. Copy the generated token for future use.
4. Now login to your AEM  author instance. On the author instance, go to **[!UICONTROL Tools]**>**[!UICONTROL Cloud Services]**> **[!UICONTROL Adobe Fonts]**.
5. Select the configuration container and click **Create**. **[UICONTROL Create Adobe Fonts Configuration]** screen appears.
    ![API token](/help/forms/using/adobe-font-configuration-screen.png)

6. Spceify the name and paste the API token in the **[!UICONTROL Kit ID]** textbox.
7. Click **Create**.



The fonts added to the **[!UICONTROL Adobe Fonts]** are available for selection in the **[!UICONTROL Text]** accordion of all the components.
1. In the theme editor, navigate to **[!UICONTROL Theme Options]** ![theme-options](assets/theme-options.png) > **[!UICONTROL Configure]**. 
2. In the **[!UICONTROL Adobe Fonts Configuration]** field, select the kit, and click **[!UICONTROL Save]**.


1. Create an [Adobe Fonts](https://fonts.adobe.com/?ref=tk.com) account, create a kit, add font Myriad Pro to the kit, publish the kit, and obtain the Kit ID. It is required to use [!DNL Adobe Fonts] (Web Fonts) in an adaptive form. 
1. In the AEM [!DNL Forms] Server, navigate to ![adobeexperiencemanager](assets/adobeexperiencemanager.png) **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Tools]** ![hammer](assets/hammer.png) > **[!UICONTROL Adobe Fonts]**. Now, open a configuration folder. If a configuration is already available, click the **[!UICONTROL Create]** button to create an instance.

   On the Create Configuration dialog, specify a **Title** for the configuration, and click **[!UICONTROL Create]**. You are redirected to the configuration page. In the [!UICONTROL Edit Component] dialog that appears, provide your **Kit ID** and click **[!UICONTROL OK]**. -->
