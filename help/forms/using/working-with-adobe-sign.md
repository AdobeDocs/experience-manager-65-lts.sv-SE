---
title: Använda Adobe Sign i ett anpassat formulär
description: Möjliggör arbetsflöden för e-signaturer (Adobe Sign) för ett anpassningsbart formulär för att automatisera signeringsarbetsflöden, förenkla processer för enstaka signaturer och för att signera formulär elektroniskt från mobila enheter.
contentOwner: khsingh
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
feature: Adaptive Forms,Foundation Components,Acrobat Sign
discoiquuid: f79828d8-2230-4477-8ffa-eeb6a0413acd
docset: aem65
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '3673'
ht-degree: 0%

---

# Använda [!DNL Adobe Sign] i ett anpassat formulär{#using-adobe-sign-in-an-adaptive-form}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/use-adobe-sign/working-with-adobe-sign.html) |
| AEM 6.5 | Den här artikeln |


[!DNL Adobe Sign] aktiverar e-signaturarbetsflöden för anpassningsbara formulär. E-signaturer förbättrar arbetsflödena för att bearbeta dokument inom juridik, försäljning, löneadministration, personaladministration och andra områden.

I ett typiskt [!DNL Adobe Sign]- och anpassningsbara formulärscenario fyller en användare i ett anpassat formulär som kan användas för en tjänst. Till exempel kräver en ansökan om inteckning och kreditkort juridiska signaturer från alla låntagare och medsökande. Om du vill aktivera arbetsflöden för elektroniska signaturer för liknande scenarier kan du integrera [!DNL Adobe Sign] med AEM [!DNL Forms]. Ytterligare några exempel är: du kan använda [!DNL Adobe Sign] för att:

* Slut avtal från vilken enhet som helst med fullt automatiserade processer för förslag, offerter och kontrakt.
* Slutför HR-processerna snabbare och ge medarbetarna de digitala upplevelserna.
* Minska kontraktscyklerna och anlita era leverantörer snabbare.
* Skapa digitala arbetsflöden som automatiserar vanliga processer.

[!DNL Adobe Sign]-integrering med AEM [!DNL Forms] har stöd för:

* Arbetsflöden för signering för en och flera användare
* Sekventiella och samtidiga signeringsarbetsflöden
* Underteckna i form och i form
* Signera formulär som anonym eller inloggad användare
* Dynamiska signeringsprocesser (integrering med AEM [!DNL Forms])
* Autentisering via kunskapsbas, telefon och sociala profiler

Lär dig de [bästa sätten att använda Adobe Sign med adaptiva formulär](https://medium.com/adobetech/using-adobe-sign-to-e-sign-an-adaptive-form-heres-the-best-way-to-do-it-dc3e15f9b684) för att skapa bättre signeringsupplevelser.

## Förutsättningar {#prerequisites}

Innan du använder [!DNL Adobe Sign] i en anpassningsbar form:

* Kontrollera att molntjänsten AEM [!DNL Forms] har konfigurerats att använda [!DNL Adobe Sign]. Mer information finns i [Integrera Adobe Sign med AEM Forms](../../forms/using/adobe-sign-integration-adaptive-forms.md).
* Håll listan med signerare klar. Du måste ange minst en e-postadress för varje signerare.

## Konfigurera [!DNL Adobe Sign] för ett adaptivt formulär {#configure-adobe-sign-for-an-adaptive-form}

Utför följande steg för att konfigurera [!DNL Adobe Sign] för ett anpassat formulär:

1. [Redigera anpassningsbara formuläregenskaper för Adobe Sign](../../forms/using/working-with-adobe-sign.md#enableadobesign)
1. [Lägga till Adobe Sign-fält i ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#addadobesignfieldstoanadaptiveform)
1. [Aktivera Adobe Sign för ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#enableadobsignforanadaptiveform)
1. [Välj Adobe Sign Cloud Service för ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#selectadobesigncloudserviceforanadaptiveform)

1. [Lägga till Adobe Sign-signerare i ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#addsignerstoanadaptiveform)
1. [Välj Skicka åtgärd för ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#selectsubmitactionforanadaptiveform)

![Information om signerare](assets/signer_details_new.png)

### Redigera adaptiva formuläregenskaper för [!DNL Adobe Sign] {#enableadobesign}

Konfigurera anpassningsbara formuläregenskaper för [!DNL Adobe Sign] för ett befintligt eller nytt anpassbart formulär.

[Skapa ett anpassat formulär för Adobe Sign](../../forms/using/working-with-adobe-sign.md#create-an-adaptive-form-for-adobe-sign) beskriver stegen för att skapa ett enkelt anpassat formulär. Se [Skapa ett adaptivt formulär](../../forms/using/creating-adaptive-form.md) för andra alternativ som är tillgängliga när du skapar ett adaptivt formulär.

#### Skapa ett anpassat formulär för [!DNL Adobe Sign] {#create-an-adaptive-form-for-adobe-sign}

Gör så här för att skapa ett signeringsaktiverat anpassat formulär:

1. Navigera till **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Forms]** > **[!UICONTROL Forms & Documents]**.
1. Välj **[!UICONTROL Create]** och välj **[!UICONTROL Adaptive Form]**. En lista med mallar visas. Markera mallen och välj **[!UICONTROL Next]**.
1. På fliken **[!UICONTROL Basic]**:

   1. Ange **[!UICONTROL Name]** och **[!UICONTROL Title]** för det adaptiva formuläret.

   1. Markera [konfigurationsbehållaren](../../forms/using/adobe-sign-integration-adaptive-forms.md#configure-adobe-sign-with-aem-forms) som skapades när [!DNL Adobe Sign] konfigurerades med AEM [!DNL Forms].

      >[!NOTE]
      >
      >Listrutan **[!UICONTROL Adobe Sign Cloud Service]** visar de molntjänster som är konfigurerade i den konfigurationsbehållare som du väljer i det här fältet. Listrutan **[!UICONTROL Adobe Sign Cloud Service]** är tillgänglig i avsnittet **[!UICONTROL Electronic Signature]** i de adaptiva formuläregenskaperna när du väljer alternativet **[!UICONTROL Enable Adobe Sign]**.

1. Välj något av följande alternativ på fliken **[!UICONTROL Form Model]**:

   * Välj alternativet **[!UICONTROL Associate form template as the Document of Record template]** och välj en dokumentmall. Om du använder ett formulärmallsbaserat adaptivt formulär visar dokumenten som skickas för signering endast de fält som är baserade på den associerade formulärmallen. Alla fält i det adaptiva formuläret visas inte.

   * Välj alternativet **[!UICONTROL Generate Document of Record]**. Om du använder ett anpassat formulär med alternativet Dokument för post aktiverat, visas alla fält i det adaptiva formuläret i det dokument som skickats för signering.

1. Välj **[!UICONTROL Create.]** Ett signeringsaktiverat anpassat formulär skapas, som kan användas för att lägga till [!DNL Adobe Sign]-fält.

#### Redigera ett anpassat formulär för [!DNL Adobe Sign] {#editafsign}

Utför följande steg för att använda [!DNL Adobe Sign] i ett befintligt anpassat formulär:

1. Navigera till **[!UICONTROL Adobe Experience Manager]** > **[!UICONTROL Forms]** > **[!UICONTROL Forms & Documents]**.
1. Markera det adaptiva formuläret och välj **[!UICONTROL Properties]**.
1. På fliken **[!UICONTROL Basic]** väljer du [konfigurationsbehållaren](../../forms/using/adobe-sign-integration-adaptive-forms.md#configure-adobe-sign-with-aem-forms) som skapades när [!DNL Adobe Sign] konfigurerades med AEM [!DNL Forms].
1. Välj något av följande alternativ på fliken **[!UICONTROL Form Model]**:

   * Välj alternativet **[!UICONTROL Associate form template as the Document of Record template]** och välj en dokumentmall. Om du använder ett formulärmallsbaserat adaptivt formulär visar dokumenten som skickas för signering endast de fält som är baserade på den associerade formulärmallen. Alla fält i det adaptiva formuläret visas inte.

   * Välj alternativet **[!UICONTROL Generate Document of Record]**. Om du använder ett anpassat formulär med alternativet Dokument för post aktiverat, visas alla fält i det adaptiva formuläret i det dokument som skickats för signering.

1. Välj **[!UICONTROL Save & Close]**. Det adaptiva formuläret har aktiverats för [!DNL Adobe Sign].

### Lägga till Adobe Sign-fält i ett anpassat formulär {#addadobesignfieldstoanadaptiveform}

[!DNL Adobe Sign] har olika fält som kan placeras i ett anpassat formulär. Dessa fält accepterar olika typer av data som signaturer, initialer, företag eller titel och hjälper till att samla in extra information vid signering, tillsammans med signaturerna. Du kan använda [!DNL Adobe Sign]-blockkomponenten för att placera [!DNL Adobe Sign]-fält på olika platser i ett anpassat formulär.

Utför följande steg för att lägga till fält i ett adaptivt formulär och anpassa olika alternativ för dessa fält:

1. Dra och släpp **[!UICONTROL Adobe Sign Block]**-komponenten från komponentwebbläsaren till det adaptiva formuläret. Blockkomponenten [!DNL Adobe Sign] har alla [!DNL Adobe Sign]-fält som stöds. Som standard läggs ett **Signature**-fält till i det adaptiva formuläret.

   ![Signeringsblock](assets/sign_block_new.png)

   Blocket [!DNL Adobe Sign] är som standard inte synligt i det publicerade adaptiva formuläret. Den är bara synlig i signeringsdokumenten. Du kan ändra synligheten för [!DNL Adobe Sign] block från egenskaperna för blockkomponenten [!DNL Adobe Sign].

   >[!NOTE]
   >
   >    * Det är inte obligatoriskt att använda blocket [!DNL Adobe Sign] för att använda [!DNL Adobe Sign] i ett anpassat format. Om du inte använder [!DNL Adobe Sign]-block och lägger till fält för signerare, visas standardsignaturfältet längst ned i signeringsdokumenten.
   >    * Använd bara blocket [!DNL Adobe Sign] för de adaptiva formulär som automatiskt genererar arkivdokument. Om du använder en anpassad XDP för att generera ett dokument för post eller ett formulärmallsbaserat anpassat formulär stöds inte [!DNL Adobe Sign]-block.
   >
   >

1. Markera komponenten **[!UICONTROL Adobe Sign Block]** och välj ikonen **Redigera** ![aem_6_3_edit](assets/aem_6_3_edit.png) . Här visas alternativ för att lägga till fält och formatera utseende för ett fält.

   ![adobe-sign-block-select-fields](assets/adobe-sign-block-select-fields.png)

   **A.** Markera och lägg till [!DNL Adobe Sign] fält. **B.** Expandera [!DNL Adobe Sign]-blocket till helskärmsläge

1. Välj ikonen **[!UICONTROL Adobe Sign]Field** ![ aem_6_3_adobesign](assets/aem_6_3_adobesign.png) . Här visas alternativ för att markera och lägga till [!DNL Adobe Sign] fält.

   Expandera det nedrullningsbara fältet **[!UICONTROL Type]** för att markera ett [!DNL Adobe Sign]-fält och välj ikonen Klar ![ aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att lägga till det markerade fältet i [!DNL Adobe Sign] -blocket. Listrutan **[!UICONTROL Type]** innehåller fälttyperna Signatur, Signerarinformation och Data. [!DNL Adobe Sign]-integrering med AEM [!DNL Forms]-supportfält visas endast i listrutan [!UICONTROL Type]. Mer information om [!DNL Adobe Sign] fält finns i [Adobe Sign-dokumentation](https://helpx.adobe.com/sign/help/field-types.html).

   ![adobe-sign-block-fields-options](assets/adobe-sign-block-fields-options.png)

   Du måste ange ett unikt namn för ett fält. Du kan också markera ett obligatoriskt fält genom att välja önskat alternativ. Förutom alternativet **[!UICONTROL Name]** och **[!UICONTROL Required]** har vissa [!DNL Adobe Sign]-fält fler alternativ. Till exempel mask och flera rader. Ange dessutom ett unikt namn för varje [!DNL Adobe Sign]-fält om fälten finns i samma eller olika [!DNL Adobe Sign]-block.

   Om du väljer **[!UICONTROL Digital Signature]** i listrutan kan du använda digitala signaturer i det adaptiva formuläret:

   * Online med molnsignaturer för att signera med ett [digitalt ID](https://helpx.adobe.com/sign/kb/digital-certificate-providers.html) som finns hos en betrodd tjänsteleverantör.
   * Hämta dokumentet lokalt med Adobe Acrobat eller Reader med ett smartkort, en USB-token eller ett filbaserat digitalt ID.

### Aktivera [!DNL Adobe Sign] för ett anpassat formulär {#enableadobsignforanadaptiveform}

[!DNL Adobe Sign] är inte aktiverat för ett anpassat formulär. Gör så här för att aktivera den:

1. Markera **[!UICONTROL Form Container]** i innehållsläsaren och välj ikonen **[!UICONTROL Configure]** ![configure](assets/configure.png) . Egenskaper öppnas i webbläsaren och egenskaper för behållare för adaptiva formulär visas.
1. Utöka dragspelsfliken **[!UICONTROL Electronic Signature]** i egenskapswebbläsaren och välj alternativet **[!UICONTROL Enable Adobe Sign]**. Det aktiverar [!DNL Adobe Sign] för ett anpassat formulär.

### Välj [!DNL Adobe Sign] Cloud Service och signeringsordning {#selectadobesigncloudserviceforanadaptiveform}

Du kan konfigurera flera [!DNL Adobe Sign]-tjänster för en instans av AEM [!DNL Forms]. Det är tillrådligt att ha en separat uppsättning tjänster för varje funktion (personal, ekonomi med mera). Det gör det enklare att spåra och rapportera signerade dokument. En bank har till exempel flera avdelningar. Du kan ha en separat konfiguration för varje avdelning för bättre spårning av dokumenten.

Ett dokument kan också ha flera signerare. En kreditkortsansökan kan t.ex. ha flera sökande. En bank kräver signaturer från alla sökande innan ansökan behandlas. I scenarier med flera signerare kan du välja att signera dokumentet i sekventiell eller samtidig ordning.

Gör så här för att välja en molntjänst och signeringsordning:

![molntjänst](assets/cloud-service.png)

1. Markera **[!UICONTROL Form Container]** i innehållsläsaren och välj ikonen **[!UICONTROL Configure]** ![configure](assets/configure.png) . Egenskaper öppnas i webbläsaren och egenskaper för behållare för adaptiva formulär visas.
1. Utöka dragspelsfliken **[!UICONTROL Electronic Signature]** i egenskapswebbläsaren och välj alternativet **[!UICONTROL Enable Adobe Sign]**. Det aktiverar [!DNL Adobe Sign] för ett anpassat formulär.
1. Välj en molntjänst i den redan konfigurerade listan över [!DNL Adobe Sign] molntjänster.

   Om listan **[!UICONTROL Adobe Sign Cloud Service]** är tom följer du artikeln [Konfigurera Adobe Sign med AEM Forms](../../forms/using/adobe-sign-integration-adaptive-forms.md) för att konfigurera tjänsten.

   I listrutan visas de molntjänster som finns i mappen `global` i Verktyg > **[!UICONTROL Cloud Services]** > **[!UICONTROL Adobe Sign]**. Dessutom listas de molntjänster som finns i mappen som du väljer i fältet **[!UICONTROL Configuration Container]** när du skapar ett anpassat formulär.

1. Välj signeringsordning i dialogrutan **[!UICONTROL Signers can Sign]**. [!DNL Adobe Sign] signerare kan signera ett anpassat formulär **[!UICONTROL Sequentially]** - en efter en annan signerare, eller **[!UICONTROL Simultaneously]** - i vilken ordning som helst.

   I sekventiell ordning tar en signerare emot formuläret för signering i taget. När en signerare har slutfört signeringen av dokumentet skickas formuläret till nästa signerare och så vidare.

   Flera signerare kan signera ett formulär samtidigt i en och samma ordning.

1. [Lägg till signerare i ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#addsignerstoanadaptiveform) och välj ikonen Klar ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att spara ändringarna.


### Lägga till signerare i ett anpassat formulär {#addsignerstoanadaptiveform}

Du kan bara ha en eller flera signerare för ett anpassat formulär. När du lägger till en signerare kan du även konfigurera autentiseringsinformation för signeraren. Du kan också välja om formuläranvändaren och signeraren ska vara samma person. Utför följande steg för att lägga till och ange olika detaljer om en signerare:

1. Markera **[!UICONTROL Form Container]** i innehållsläsaren och välj ikonen **[!UICONTROL Configure]** ![configure](assets/configure.png) . Egenskapsläsaren öppnas med egenskaper för adaptiv formulärbehållare.
1. Utöka dragspelsfliken **[!UICONTROL Electronic Signature]** i egenskapswebbläsaren och välj alternativet **[!UICONTROL Enable Adobe Sign]**. Det aktiverar [!DNL Adobe Sign] för ett anpassat formulär.
1. Välj **[!UICONTROL Add Signer]** under **[!UICONTROL Signer Configuration]**. Den lägger till en signerare i det adaptiva formuläret. Du kan lägga till flera [!DNL Adobe Sign]-signerare i ett anpassat formulär.
   ![telefoninformation](assets/phone-details.png)

1. Klicka på ikonen **Redigera** ![aem_6_3_edit](assets/aem_6_3_edit.png) för att ange följande information om signeraren:

   * **[!UICONTROL Title]:** Ange en titel som unikt identifierar en signerare.

   * **[!UICONTROL Is the signer and the person filling the form same?]:** Välj **Ja** om formuläranvändaren och den första signeraren är samma person. Om alternativet är inställt på **Nej,** ska du inte använda signaturstegskomponenten i det adaptiva formuläret. Om formuläret innehåller en komponent för signatursteg ställs fältet automatiskt in på Ja.

   * **[!UICONTROL Signer Email address]:** Ange signerarens e-postadress. Signeraren får signerade dokument/formulär på den angivna e-postadressen. Du kan välja att använda en e-postadress som finns i ett formulärfält, i AEM-användarprofilen för den inloggade användaren eller manuellt ange en e-postadress. Det är ett obligatoriskt steg. Kontrollera att e-postadressen för den första signeraren eller den enda signeraren (om det finns en signerare) inte är identisk med det [!DNL Adobe Sign]-konto som används för att konfigurera AEM molntjänster.

   * **[!UICONTROL Signer Authentication Method]:** Ange metoden för att autentisera en användare innan ett formulär öppnas för signering. Du kan välja mellan telefon, kunskapsbas och social ID-baserad autentisering. För Adobe Acrobat Sign Solutions for Government finns endast telefon- och kunskapsbaserad autentisering.

   >[!NOTE]
   >
   >    * Som standard har den sociala identitetsbaserade autentiseringen ett alternativ för att autentisera med Facebook, Google och LinkedIn. Du kan kontakta [!DNL Adobe Sign]-supporten för att aktivera andra leverantörer av social autentisering.
   >
   >

   * **[!DNL Adobe Sign]fält att fylla i eller signera:** Välj [!DNL Adobe Sign] fält för signeraren. Ett anpassat formulär kan ha flera [!DNL Adobe Sign] fält. Du kan välja att aktivera specifika fält för en signerare. Fältet visar alla tillgängliga [!DNL Adobe Sign] block. När du markerar ett block markeras alla fält i blocket. Du kan använda X-ikonen för att avmarkera ett fält.

   ![signerarinformation](assets/signer-details.png)

   Bilden ovan har två exempel på [!DNL Adobe Sign]-block: Personlig information och Office-information

   Välj ikonen Klar ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) . Undertecknaren läggs till och konfigureras.

### Välj Skicka åtgärd för ett anpassat formulär {#selectsubmitactionforanadaptiveform}

När du har lagt till [!DNL Adobe Sign]-fält i ett anpassat formulär aktiverar du [!DNL Adobe Sign] från formulärbehållaren, väljer [!DNL Adobe Sign] Cloud Service och lägger till [!DNL Adobe Sign] signerare. Välj sedan en lämplig överföringsåtgärd för det anpassade formuläret. Detaljerad information om hur du skickar formulär med adaptiva format finns i [Konfigurera åtgärden Skicka](../../forms/using/configuring-submit-actions.md).

Dessutom skickas ett anpassat formulär som har aktiverats av [!DNL Adobe Sign] endast efter att alla signerare har signerat formuläret. Du kan hitta delvis signerade formulär i avsnittet Väntande signering på formulärportalen. Konfigurationstjänsten [!DNL Adobe Sign] fortsätter avfråga [!DNL Adobe Sign]-servern med [regelbundna intervall](../../forms/using/adobe-sign-integration-adaptive-forms.md) för att verifiera signaturernas status. Om alla signerare signerar formuläret, startas tjänsten för att skicka och formuläret skickas. Om du använder en anpassad sändningsåtgärd och formuläret använder [!DNL Adobe Sign], ska du uppdatera din anpassade sändningsåtgärd så att den använder sändningsåtgärdstjänsten.

<!-- Remove when forms portal goes live
>[!NOTE]
>
>Data of the adaptive form is stored temporarily on Forms Portal. Use [custom storage for Forms Portal](/help/forms/using/configuring-draft-submission-storage.md). It ensures that the PII (personally identifiable information) data is not stored on AEM servers. 
-->

Din formulärsigneringsupplevelse är klar. Du kan förhandsgranska formuläret för att verifiera signeringsprocessen. På det publicerade formuläret visas [!DNL Adobe Sign] blockeringsfält när en signerare tar emot formuläret för signering via ett e-postmeddelande. Den här upplevelsen kallas även signeringsupplevelse som inte är i form. Du kan också konfigurera en signeringsupplevelse i formulär för den första signeraren. Mer information finns i [Skapa signeringsupplevelse i formulär](../../forms/using/working-with-adobe-sign.md#create-in-form-signing-experience).

## Konfigurera molnsignaturer för ett anpassat formulär {#configure-cloud-signatures-for-an-adaptive-form}

Molnbaserade digitala signaturer eller fjärrsignaturer är en ny generation digitala signaturer som fungerar på både dator, mobil och webben - och som uppfyller de högsta efterlevnads- och säkerhetsnivåerna för autentisering av signerare. Du kan signera ett anpassat formulär med molnbaserade digitala signaturer.

När du har [redigerat anpassningsbara formuläregenskaper för Adobe Sign](../../forms/using/working-with-adobe-sign.md#enableadobesign) utför du följande steg för att lägga till molnsignaturfält i ett anpassat formulär:

1. Dra och släpp **[!UICONTROL Adobe Sign Block]**-komponenten från komponentwebbläsaren till det adaptiva formuläret. Komponenten [!UICONTROL Adobe Sign Block] har alla [!DNL Adobe Sign]-fält som stöds. Som standard läggs ett **[!UICONTROL Signature]**-fält till i det adaptiva formuläret.

   ![Signeringsblock](assets/sign-block-new.png)

1. Markera komponenten **[!UICONTROL Adobe Sign Block]** och välj ikonen **Redigera** ![aem_6_3_edit](assets/aem_6_3_edit.png) . Här visas alternativ för att lägga till fält och formatera utseende för ett fält.

   ![adobe-sign-block-select-fields](assets/adobe-sign-block-select-fields.png)

   **A.** Markera och lägg till [!DNL Adobe Sign] fält. **B.** Expandera [!DNL Adobe Sign]-blocket till helskärmsläge

1. Välj ikonen **[!UICONTROL Adobe Sign Field]** ![aem_6_3_adobesign](assets/aem_6_3_adobesign.png) . Här visas alternativ för att markera och lägga till [!DNL Adobe Sign] fält.

   Expandera det nedrullningsbara fältet **[!UICONTROL Type]** för att markera **[!UICONTROL Digital Signature]** och markera ikonen **Klar** för att lägga till det markerade fältet i blocket [!DNL Adobe Sign].

   ![Digitala signaturer](assets/digital_signatures_new.png)

   Du måste ange ett unikt namn för ett fält.

   Använd digitala signaturer på det anpassade formuläret med:

   * Molnsignaturer: Signera med ett [digitalt ID](https://helpx.adobe.com/sign/kb/digital-certificate-providers.html) som finns hos en betrodd tjänsteleverantör. Molnsignaturalternativet är inte tillgängligt för Adobe Acrobat Sign Solutions för myndigheter.

   * Adobe Acrobat eller Reader: Hämta och öppna dokumentet med Adobe Acrobat eller Reader för att signera med ett smartkort, en USB-token eller ett filbaserat digitalt ID.

   När du har lagt till fältet för molnsignatur i det adaptiva formuläret utför du följande steg för att slutföra konfigurationsprocessen:

   * [Aktivera Adobe Sign för ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#enableadobsignforanadaptiveform)
   * [Välj Adobe Sign Cloud Service för ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#selectadobesigncloudserviceforanadaptiveform)
   * [Lägga till Adobe Sign-signerare i ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#addsignerstoanadaptiveform)
   * [Välj Skicka åtgärd för ett anpassat formulär](../../forms/using/working-with-adobe-sign.md#selectsubmitactionforanadaptiveform)

## Skapa signeringsupplevelser i form av formulär {#create-in-form-signing-experience}

Användaren kan också signera ett anpassat formulär medan han/hon fyller i formuläret. Den här upplevelsen kallas även signering i form av formulär. Signeringsfunktionen i form är bara tillgänglig för den första signeraren i en miljö med flera signerare. Utför följande steg för att skapa en signeringsupplevelse i ett anpassat formulär:

1. [Lägg till och konfigurera signaturstegskomponenten](../../forms/using/working-with-adobe-sign.md#add-and-configure-the-signature-step-component).
1. [Lägg till komponenten Sammanfattningssteg](../../forms/using/working-with-adobe-sign.md#configure-the-thank-you-page-or-summary-step-component).

![Signering i formulär](assets/in_form_signing_experience_new.png)

### Lägga till och konfigurera komponenten Signatursteg {#add-and-configure-the-signature-step-component}

Använd signaturstegskomponenten för att ange ett område där det ifyllda formuläret ska signeras elektroniskt. När avsnittet med signaturstegskomponenten återges visas en signerbar PDF-version av det ifyllda formuläret. Komponenten Signatursteg får full bredd som är tillgänglig för formuläret. Vi rekommenderar att du inte har någon annan komponent i avsnittet som innehåller komponenten Signatursteg.

Utför följande steg för att konfigurera signaturstegskomponenten:

1. Dra och släpp komponenten **[!UICONTROL Signature Step]** från komponentwebbläsaren till formuläret.
1. Markera den nyligen tillagda signaturstegskomponenten och välj ikonen **Konfigurera** ![konfigurera](assets/configure.png) . Egenskaper öppnas i webbläsaren och egenskaper för signatursteg visas. Konfigurera följande egenskaper:

   * **[!UICONTROL Name]**: Ange komponentens namn.

   * **[!UICONTROL Title]:** Ange komponentens unika titel.
   * **[!UICONTROL Template message]:** Ange meddelandet som ska visas när signaturen för PDF läses in. [!DNL Adobe Sign] tjänster tar lite tid att förbereda och läsa in signaturens PDF.
   * **[!UICONTROL Signing Service]:** Välj alternativet **[!DNL Adobe Sign]**.

   * **[!UICONTROL Use legacy E-sign component]**: Om du använder respektive adaptiva formulär i [ AEM Forms Workspace](../../forms/using/introduction-html-workspace.md), appen AEM [!DNL Forms] eller det underliggande adaptiva formuläret har en äldre e-signeringskomponent, väljer du alternativet **Använd äldre e-signeringskomponent**.

   * **[!UICONTROL Configuration]**: Välj en konfiguration ([!DNL Adobe Sign] Cloud Service). Listrutan är bara tillgänglig om alternativet **Använd äldre e-signeringskomponent** är aktiverat.

   * **[!UICONTROL CSS Class]**: Ange CSS-klassen för komponenten.

   Välj ikonen Klar ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att spara ändringarna.

   ![Underskriftssteg](assets/signature_step_new.png)

   >[!NOTE]
   >
   >* När du drar och släpper komponenten **[!UICONTROL Signature Step]** i formuläret ställs alternativet **[!UICONTROL Is the signer and the person filling the form same?]** automatiskt in på **Ja** . Det krävs för att formuläret ska fungera.
   >* Använd komponenten Sammanfattningssteg efter signatursteget för att få en så bra upplevelse som möjligt. Sammanfattningssteget skickar automatiskt och omedelbart formuläret när du har signerat ett formulär i signeringssteget. Om du inte använder sammanfattningssteget aktiveras en automatisk överföring endast efter det intervall som angetts med [Adobe Sign-konfigurationstjänsten](../../forms/using/adobe-sign-integration-adaptive-forms.md#configure-adobe-sign-scheduler-to-sync-the-signing-status).
   >
   >Några av de bästa sätten är:
   >
   >* Anpassad formulärpanel som innehåller signeringssteget finns alltid i den sista eller andra panelen i ett anpassat formulär. Det kan bara vara den andra sista panelen när den sista panelen innehåller steget Sammanfattning.
   >* Panelen som innehåller signatur- eller sammanfattningssteget får inte innehålla några andra komponenter.
   >* Anpassningsbara formulär som innehåller signatursteget kan inte ha en skicka-knapp.
   >* Inlämningen av anpassade formulär som innehåller signeringssteget hanteras via en bakgrundstjänst eller sammanfattningssteget. Om det finns en konfigurerad signerare som också fyller i formuläret, är fördelen med att hantera den adaptiva formuläröverföringen med steget Sammanfattning att den omedelbart utvärderar att signeraren har signerat formuläret och anropar åtgärden skicka. En bakgrundstjänst tar längre tid att utvärdera om alla konfigurerade signerare har signerat formuläret och fördröjer överföringen av det adaptiva formuläret.
   >* Utforma formuläret så att användaren inte kan navigera tillbaka från en panel som innehåller signatur- eller sammanfattningssteget.


### Konfigurera tacksidan eller sammanfattningssteget {#configure-the-thank-you-page-or-summary-step-component}

Komponenten **Sammanfattningssteg** skickar automatiskt formuläret, fyller i informationen på den anpassade sammanfattningssidan och visar sammanfattningen av det skickade formuläret. Den hämtar även den information som krävs i returkartan. Komponenten Sammanfattningssteg får full bredd som är tillgänglig för formuläret. Vi rekommenderar att du inte har någon annan komponent i avsnittet som innehåller komponenten Sammanfattningssteg.

Nu är signeringsupplevelsen i form av formulär klar. Du kan förhandsgranska formuläret för att verifiera signeringsprocessen.

## Frågor och svar {#frequently-asked-questions}

**Q:** Du kan bädda in ett adaptivt formulär i ett annat adaptivt formulär. Kan det inbäddade adaptiva formuläret [!DNL Adobe Sign] aktiveras?
**Ans:** Nej, AEM [!DNL Forms] stöder inte användning av ett adaptivt formulär som bäddar in ett adaptivt formulär som kan signeras med [!DNL Adobe Sign]

**Q:** När jag skapar ett adaptivt formulär med den avancerade mallen och öppnar det för redigering visas felmeddelandet&quot;Elektronisk signatur eller signerare är inte korrekt konfigurerade&quot;. visas. Hur löser jag felmeddelandet?
**Ans:** Adaptivt formulär som skapats med den avancerade mallen har konfigurerats att använda [!DNL Adobe Sign]. Lös felet genom att skapa och välja en [!DNL Adobe Sign]-molnkonfiguration och konfigurera en [!DNL Adobe Sign]-signerare för det adaptiva formuläret.

**Q:** Kan jag använda [!DNL Adobe Sign]-texttaggar i en statisk textkomponent i ett adaptivt formulär?
**Ans:** Ja, du kan använda texttaggar i en textkomponent för att lägga till [!DNL Adobe Sign] fält i ett anpassat formulär som har aktiverats med alternativet [Postdokument](../../forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.md) (endast automatiskt genererat postdokument). Mer information om proceduren och reglerna för att skapa en texttagg finns i [Adobe Sign-dokumentation](https://helpx.adobe.com/sign/using/text-tag.html). Dessutom har adaptiva formulär begränsat stöd för texttaggar. Du kan använda texttaggarna för att skapa endast de fält som [Adobe Sign-blocket](../../forms/using/working-with-adobe-sign.md#configure-cloud-signatures-for-an-adaptive-form) har stöd för.

**Q:** AEM [!DNL Forms] innehåller både [!UICONTROL Adobe Sign block]- och signaturstegskomponenter. Kan dessa användas samtidigt i anpassningsbar form?
**Ans:** Du kan använda båda komponenterna samtidigt i ett formulär. Här följer några rekommendationer för hur du använder dessa komponenter:

**Adobe Sign-block:** Du kan använda [!UICONTROL Adobe Sign Block] för att lägga till [!UICONTROL Adobe Sign] fält var som helst i det adaptiva formuläret. Det hjälper även till att tilldela specifika fält till signerare. När ett anpassat formulär förhandsgranskas eller publiceras är [!UICONTROL Adobe Sign]-blocket inte synligt som standard. De här blocken aktiveras bara i signeringsdokumentet. I signeringsdokumentet aktiveras bara de fält som tilldelats en signerare. [!UICONTROL Adobe Sign]-blocket kan användas med första och efterföljande signerare.

**Underskriftsstegskomponent:** Du kan använda signaturstegskomponenten för att skapa signeringsupplevelser i formulär. Endast den första signeraren kan signera medan formuläret fylls i. När avsnittet med signaturstegskomponenten återges visas en signerbar PDF-version av formuläret. Det är vanligtvis det sista eller näst sista avsnittet följt av en sammanfattande komponent i ett formulär.

## Felsökning {#troubleshoot}

### [!DNL Adobe Sign] avtalsfel {#adobe-sign-agreement-failures}

**Utgåva**
När [!DNL Adobe Sign] -tjänsten har konfigurerats för ett adaptivt formulär kan tjänsten inte skapa ett [!DNL Adobe Sign] -avtal för det underliggande adaptiva formuläret.

**Upplösning**

* Kontrollera [konfigurationen av Adobe Sign-molntjänsten](../../forms/using/adobe-sign-integration-adaptive-forms.md) som används i det adaptiva formuläret.
* Kontrollera att API-programmet på [!DNL Adobe Sign]-servern som används för att konfigurera [!DNL Adobe Sign] Cloud-tjänsten har nödvändiga behörigheter.
* Om du använder flera [!DNL Adobe Sign]-molntjänster pekar du **[!UICONTROL oAuth URL]** för alla tjänster på samma **[!UICONTROL Adobe Sign Shard]**.

* Använd separata e-postadresser för att konfigurera [!DNL Adobe Sign]-kontot och för den första signeraren och den första signeraren. E-postadressen till den första signeraren eller den enda signeraren (om det finns en signerare) kan inte vara identisk med det [!DNL Adobe Sign]-konto som används för att konfigurera AEM molntjänster.

### AEM [!DNL Forms]-arbetsflödet som konfigurerats för ett adaptivt formulär som har aktiverats för [!DNL Adobe Sign] startar inte {#adobe-sign-aem-form-workflow-failures}

**Utgåva**
När [!DNL Adobe Sign] har konfigurerats för ett anpassningsbart formulär startar inte arbetsflödet som har konfigurerats med alternativet Anropa [!DNL Forms] arbetsflöde.

**Upplösning**

* När du använder [!DNL Adobe Sign] utan signatursteget eller formuläret kräver signaturer från flera personer väntar AEM [!DNL Forms]-servern på att schemaläggaren ska bekräfta att alla personer har signerat formuläret. Schemaläggaren skickar det adaptiva formuläret först när alla personer har slutfört signeringen och arbetsflödet startar först när det adaptiva formuläret har skickats. Du kan korta ned intervallet för [schemaläggaren](adobe-sign-integration-adaptive-forms.md) om du vill kontrollera statusen för formulärsignering med snabba intervall och snabba formulärskickningar.


## Relaterade artiklar {#related-articles}

* [Integrera Adobe Sign med AEM Forms](../../forms/using/adobe-sign-integration-adaptive-forms.md)
* [Bästa tillvägagångssätt för att använda Adobe Sign med adaptiva formulär](https://medium.com/adobetech/using-adobe-sign-to-e-sign-an-adaptive-form-heres-the-best-way-to-do-it-dc3e15f9b684)
* [Använda Adobe Sign med AEM Forms (video)](https://helpx.adobe.com/experience-manager/kt/forms/using/adobe-sign-integration-feature-video.html)
