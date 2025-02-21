---
title: Lägg till anpassad åtgärd/knapp i användargränssnittet Skapa korrespondens
description: Lär dig hur du lägger till en anpassad åtgärd/knapp i användargränssnittet för att skapa korrespondens
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
docset: aem65
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1854'
ht-degree: 0%

---

# Lägga till en anpassad åtgärdsknapp i användargränssnittet för Skapa korrespondens {#add-custom-action-button-in-create-correspondence-ui}

## Ökning {#overview}

Med Correspondence Management-lösningen kan du lägga till anpassade åtgärder i användargränssnittet Skapa korrespondens.

Scenariot i det här dokumentet förklarar hur du kan skapa en knapp i användargränssnittet Skapa korrespondens för att dela ett brev som en granskning som PDF bifogat till ett e-postmeddelande.

### Förutsättningar {#prerequisites}

Du behöver följande för att slutföra det här scenariot:

* Kunskap om CRX och JavaScript
* LiveCycle Server

## Scenario: Skapa knappen i användargränssnittet för Create Correspondence om du vill skicka ett brev för granskning {#scenario-create-the-button-in-the-create-correspondence-user-interface-to-send-a-letter-for-review}

Om du lägger till en knapp med en åtgärd (här skickar du ett brev för granskning) i användargränssnittet Skapa korrespondens innehåller följande:

1. Lägga till knappen i användargränssnittet Skapa korrespondens
1. Lägga till åtgärdshantering för knappen
1. Lägga till LiveCycle-processen för att aktivera åtgärdshantering

### Lägg till knappen i användargränssnittet Skapa korrespondens {#add-the-button-to-the-create-correspondence-user-interface}

1. Gå till `https://'[server]:[port]'/[ContextPath]/crx/de` och logga in som administratör.
1. I appmappen skapar du en mapp med namnet `defaultApp` med en sökväg/struktur som liknar defaultApp-mappen (i config-mappen). Så här skapar du mappen:

   1. Högerklicka på mappen **defaultApp** på följande sökväg och välj **Overlay Node**:

      /libs/fd/cm/config/defaultApp/

      ![Överläggsnod](assets/1_defaultapp.png)

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/config/defaultApp/

      **Plats för övertäckning:** /apps/

      **Matcha nodtyper:** Markerade

      ![Överläggsnod](assets/2_defaultappoverlaynode.png)

   1. Klicka på **OK**.
   1. Klicka på **Spara alla**.

1. Skapa en kopia av filen acmExtensionsConfig.xml (finns under grenen /libs) under grenen /apps.

   1. Gå till&quot;/libs/fd/cm/config/defaultApp/acmExtensionsConfig.xml&quot;

   1. Högerklicka på filen acmExtensionsConfig.xml och välj **Kopiera**.

      ![Kopiera acmExtensionsConfig.xml](assets/3_acmextensionsconfig_xml_copy.png)

   1. Högerklicka på mappen **defaultApp** på &quot;/apps/fd/cm/config/defaultApp/&quot; och välj **Klistra in**.
   1. Klicka på **Spara alla**.

1. Dubbelklicka på den kopia av acmExtentionsConfig.xml som du nyss skapade i mappen apps. Filen öppnas för redigering.
1. Leta reda på följande kod:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <extensionsConfig>
       <modelExtensions>
           <modelExtension type="LetterInstance">
     <customAction name="Preview" label="loc.letterInstance.preview.label" tooltip="loc.letterInstance.preview.tooltip" styleName="previewButton"/>
               <customAction name="Submit" label="loc.letterInstance.submit.label" tooltip="loc.letterInstance.submit.tooltip" styleName="submitButton" permissionName="forms-users"/>
               <customAction name="SaveAsDraft" label="loc.letterInstance.saveAsDraft.label" tooltip="loc.letterInstance.saveAsDraft.tooltip" styleName="submitButton" permissionName="forms-users"/>
               <customAction name="Close" label="loc.letterInstance.close.label" tooltip="loc.letterInstance.close.tooltip" styleName="closeButton"/>
           </modelExtension>
       </modelExtensions>
   </extensionsConfig>
   ```

1. Du kan använda LiveCycle Forms Workflow för att skicka e-postbrev. Lägg till en customAction-tagg under taggen modelExtension i acmExtensionsConfig.xml enligt följande:

   ```xml
    <customAction name="Letter Review" label="Letter Review" tooltip="Letter Review" styleName="" permissionName="forms-users" actionHandler="CM.domain.CCRCustomActionHandler">
         <serviceName>Forms Workflow -> SendLetterForReview/SendLetterForReviewProcess</serviceName>
       </customAction>
   ```

   ![customAction-tagg](assets/5_acmextensionsconfig_xml.png)

   Taggen modelExtension har en uppsättning med customAction-underordnade taggar som konfigurerar åtgärden, behörigheterna och utseendet för åtgärdsknappen. Här följer en lista över konfigurationstaggar för customAction:

   | **Namn** | **Beskrivning** |
   |---|---|
   | name | Det alfanumeriska namnet på åtgärden som ska utföras. Värdet för den här taggen är obligatoriskt, måste vara unikt (inom taggen modelExtension) och måste börja med ett alfabet. |
   | label | Etiketten som ska visas på åtgärdsknappen |
   | knappbeskrivning | Knappens knappbeskrivning, som visas när användaren håller muspekaren över knappen. |
   | styleName | Namnet på det anpassade format som används på åtgärdsknappen. |
   | permissionName | Motsvarande åtgärd visas bara om användaren har den behörighet som anges av permissionName. När du anger permissionName som `forms-users` får alla användare åtkomst till det här alternativet. |
   | actionHandler | Fullständigt kvalificerat namn på ActionHandler-klassen som anropas när användaren klickar på knappen. |

   Förutom ovanstående parametrar kan det finnas ytterligare konfigurationer associerade med en customAction. Dessa ytterligare konfigurationer görs tillgängliga för hanteraren via objektet CustomAction.

   | **Namn** | **Beskrivning** |
   |---|---|
   | serviceName | Om en customAction innehåller en underordnad tagg med namnet serviceName anropas en process med namnet serviceName-taggen när du klickar på den relevanta knappen/länken. Se till att den här processen har samma signatur som PostProcess för brev. Lägg till prefixet&quot;Forms Workflow ->&quot; i tjänstnamnet. |
   | Parametrar som innehåller prefixet cm_ i taggnamnet | Om en customAction innehåller en underordnad tagg som börjar med namnet cm_ är de här parametrarna tillgängliga i post-processen (vare sig det är A4 Post Process eller den speciella process som representeras av serviceName-taggen) under den aktuella taggen med cm_-prefixet borttaget. |
   | actionName | När en inläggsprocess beror på ett klick innehåller inskickad XML en speciell tagg med namnet under taggen med namnet på användaråtgärden. |

1. Klicka på **Spara alla**.

#### Skapa en lokal mapp med egenskapsfilen i grenen /apps {#create-a-locale-folder-with-properties-file-in-the-apps-branch}

Filen ACMExtensionsMessages.properties innehåller etiketter och knappbeskrivningsmeddelanden för olika fält i användargränssnittet Create Correspondence. Gör en kopia av den här filen i grenen /apps för att de anpassade åtgärderna/knapparna ska fungera.

1. Högerklicka på mappen **locale** på följande sökväg och välj **Overlay Node**:

   /libs/fd/cm/config/defaultApp/locale

1. Kontrollera att dialogrutan Overlay Node har följande värden:

   **Sökväg:** /libs/fd/cm/config/defaultApp/locale

   **Plats för övertäckning:** /apps/

   **Matcha nodtyper:** Markerade

1. Klicka på **OK**.
1. Klicka på **Spara alla**.
1. Högerklicka på följande fil och välj **Kopiera**:

   `/libs/fd/cm/config/defaultApp/locale/ACMExtensionsMessages.properties`

1. Högerklicka på mappen **locale** på följande sökväg och välj **Klistra in**:

   `/apps/fd/cm/config/defaultApp/locale/`

   Filen ACMExtensionsMessages.properties kopieras i mappen locale.

1. Om du vill lokalisera etiketterna för den nya anpassade åtgärden/knappen skapar du filen ACMExtensionsMessages.properties för den aktuella språkinställningen i `/apps/fd/cm/config/defaultApp/locale/`.

   Om du till exempel vill lokalisera den anpassade åtgärd/knapp som skapas i den här artikeln skapar du en fil med namnet ACMExtensionsMessages_fr.properties med följande post:

   `loc.letterInstance.letterreview.label=Revue De Lettre`

   På samma sätt kan du lägga till fler egenskaper, t.ex. för verktygstips och format, i den här filen.

1. Klicka på **Spara alla**.

#### Starta om paketet Adobe Asset Composer Building Block {#restart-the-adobe-asset-composer-building-block-bundle}

När du har gjort alla ändringar på serversidan startar du om paketet Adobe Asset Composer Building Block. I det här scenariot redigeras filerna acmExtensionsConfig.xml och ACMExtensionsMessages.properties på serversidan, och därför måste Adobe Asset Composer Building Block-paketet startas om.

>[!NOTE]
>
>Du kan behöva rensa webbläsarens cache.

1. Gå till `https://[host]:'port'/system/console/bundles`. Logga in som administratör om det behövs.

1. Leta reda på paketet Adobe Asset Composer Building Block. Starta om paketet: klicka på Stopp och sedan på Start.

   ![Adobe Asset Composer-byggblock](assets/6_assetcomposerbuildingblockbundle.png)

När du har startat om paketet Adobe Asset Composer Building Block visas den anpassade knappen i användargränssnittet Create Correspondence. Du kan öppna ett brev i Skapa korrespondensanvändargränssnitt om du vill förhandsgranska den anpassade knappen.

### Lägga till åtgärdshantering till knappen {#add-action-handling-to-the-button}

Användargränssnittet Create Correspondence har som standard implementering av ActionHandler i filen cm.domain.js på följande plats:

/libs/fd/cm/ccr/gui/components/admin/clientlibs/ccr/js/cm.domain.js

Om du vill hantera anpassade åtgärder skapar du en övertäckning av filen cm.domain.js i grenen /apps i CRX.

Hanteringen av åtgärd/knapp vid klickning innehåller logik för:

* Gör den nyligen tillagda åtgärden synlig/osynlig: gör du genom att åsidosätta funktionen actionVisible().
* Aktivera/inaktivera nyligen tillagda åtgärder: det görs genom att funktionen actionEnabled() åsidosätts.
* Faktisk hantering av åtgärden när användaren klickar på knappen: utförd genom att åsidosätta implementeringen av funktionen handleAction().

1. Gå till `https://'[server]:[port]'/[ContextPath]/crx/de`. Logga in som administratör om det behövs.

1. I appmappen skapar du en mapp med namnet `js` i grenen /apps i CRX med en struktur som liknar den i följande mapp:

   `/libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/js`

   Så här skapar du mappen:

   1. Högerklicka på mappen **js** på följande sökväg och välj **Överläggsnod**:

      `/libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/js`

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/js

      **Plats för övertäckning:** /apps/

      **Matcha nodtyper:** Markerade

   1. Klicka på **OK**.
   1. Klicka på **Spara alla**.

1. I mappen js skapar du en fil med namnet crcustomization.js med koden för knappens åtgärdshantering enligt följande:

   1. Högerklicka på mappen **js** på följande sökväg och välj **Skapa > Skapa fil**:

      `/apps/fd/cm/ccr/gui/components/admin/clientlibs/ccrui/js`

      Ge filen namnet crcustomization.js.

   1. Dubbelklicka på filen crcustomization.js för att öppna den i CRX.
   1. Klistra in följande kod i filen och klicka på **Spara alla**:

      ```javascript
      /* for adding and handling custom actions in Extensible Toolbar.
        * One instance of handler will be created for each action.
        * CM.domain.CCRCustomActionHandler is actionHandler class.
        */
      var CCRCustomActionHandler;
          CCRCustomActionHandler = CM.domain.CCRCustomActionHandler = new Class({
              className: 'CCRCustomActionHandler',
              extend: CCRDefaultActionHandler,
              construct : function(action,model){
              }
          });
          /**
           * Called when user user click an action
           * @param extraParams additional arguments that may be passed to handler (For future use)
           */
          CCRCustomActionHandler.prototype.handleAction = function(extraParams){
              if (this.action.name == CCRCustomActionHandler.SEND_FOR_REVIEW) {
                  var sendForReview = function(){
                      var serviceName = this.action.actionConfig["serviceName"];
                      var inputParams = {};
                      inputParams["dataXML"] = this.model.iccData.data;
                      inputParams["letterId"] = this.letterVO.id;
                      inputParams["letterName"] = this.letterVO.name;
                      inputParams["mailId"] = $('#email').val();
                      /*function to invoke the LivecyleService */
                      ServiceDelegate.callJSONService(this,"lc.icc.renderlib.serviceInvoker.json","invokeProcess",[serviceName,inputParams],this.onProcessInvokeComplete,this.onProcessInvokeFail);
                      $('#ccraction').modal("hide");
                  }
                  if($('#ccraction').length == 0){
                      /*For first click adding popup & setting letterName.*/
                      $("body").append(popUp);
                      $("input[id*='letterName']").val(this.letterVO.name);
                      $(document).on('click',"#submitLetter",$.proxy( sendForReview, this ));
                  }
                  $('#ccraction').modal("show");
              }
          };
          /**
           * Should the action be enabled in toolbar
           * @param extraParams additional arguements that may be passed to handler (For future use)
           * @return flag indicating whether the action should be enabled
           */
         CCRCustomActionHandler.prototype.actionEnabled = function(extraParams){
                  /*can be customized as per user requirement*/
                  return true;
          };
          /**
           * Should the action be visible in toolbar
           * @param extraParams additional arguments that may be passed to handler (For future use)
           * @return flag indicating whether the action should be enabled
           */
          CCRCustomActionHandler.prototype.actionVisible = function(extraParams){
              /*Check can be enabled for Non-Preview Mode.*/
              return true;
          };
          /*SuccessHandler*/
          CCRCustomActionHandler.prototype.onProcessInvokeComplete = function(response) {
              ErrorHandler.showSuccess("Letter Sent for Review");
          };
          /*FaultHandler*/
          CCRCustomActionHandler.prototype.onProcessInvokeFail = function(event) {
              ErrorHandler.showError(event.message);
          };
          CCRCustomActionHandler.SEND_FOR_REVIEW  = "Letter Review";
      /*For PopUp*/
          var popUp = '<div class="modal fade" id="ccraction" tabindex="-1" role="dialog" aria-hidden="true">'+
          '<div class="modal-dialog modal-sm">'+
              '<div class="modal-content">' +
                  '<div class="modal-header">'+
                      '<button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</code></button>'+
                      '<h4 class="modal-title"> Send Review </h4>'+
                  '</div>'+
                  '<div class="modal-body">'+
                      '<form>'+
                          '<div class="form-group">'+
                              '<label class="control-label">Email Id</label>'+
                              '<input type="text" class="form-control" id="email">'+
                          '</div>'+
                          '<div class="form-group">'+
                              '<label  class="control-label">Letter Name</label>'+
                              '<input id="letterName" type="text" class="form-control" readonly>'+
                          '</div>'+
                          '<div class="form-group">'+
                              '<input id="letterData" type="text" class="form-control hide" readonly>'+
                          '</div>'+
                      '</form>'+
                  '</div>'+
                  '<div class="modal-footer">'+
                     '<button type="button" class="btn btn-default" data-dismiss="modal"> Cancel </button>'+
                     '<button type="button" class="btn btn-primary" id="submitLetter"> Submit </button>'+
                  '</div>'+
              '</div>'+
          '</div>'+
      '</div>';
      ```

### Lägg till LiveCycle-processen för att aktivera åtgärden <span class="acrolinxCursorMarker"></code>hantering {#add-the-livecycle-process-to-enable-action-span-class-acrolinxcursormarker-span-handling}

I det här scenariot aktiverar du följande komponenter, som är en del av den bifogade filen components.zip:

* DSC-komponent jar (DSCSample.jar)
* Skicka brev för granskning LCA (SendLetterForReview.lca)

Ladda ned och zippa upp filen components.zip för att hämta filerna DSCSample.jar och SendLetterForReview.lca. Använd dessa filer enligt följande procedurer.
[Hämta fil](assets/components.zip)

#### Konfigurera LiveCycle Server för körning av LCA-processen {#configure-the-livecycle-server-to-run-the-lca-process}

>[!NOTE]
>
>Det här steget krävs bara om du använder en OSGI-konfiguration och LC-integrering krävs för den typ av anpassning som du implementerar.

LCA-processen körs på LiveCycle-servern och kräver serveradressen och inloggningsuppgifterna.

1. Gå till `https://'[server]:[port]'/system/console/configMgr` och logga in som administratör.
1. Leta upp Adobe LiveCycle Client SDK Configuration och klicka på **Edit** (redigeringsikonen). Panelen Konfigurationer öppnas.

1. Ange följande information och klicka på **Spara**:

   * **Server-URL**: URL för den LC-server vars Send for Review-tjänst åtgärdshanterarkoden använder.
   * **Användarnamn**: Administratörsanvändarnamn för LC-servern
   * **Lösenord**: Lösenord för administratörens användarnamn

   ![Adobe LiveCycle Client SDK Configuration](assets/3_clientsdkconfiguration.png)

#### Installera LiveCycle Archive (LCA) {#install-livecycle-archive-lca}

Den LiveCycle-process som krävs för e-posttjänstprocessen.

>[!NOTE]
>
>Om du vill se vad den här processen gör eller skapa en egen liknande process behöver du Workbench.

1. Logga in som administratör för LiveCycle® Server-administratörer på `https:/[lc server]/:[lc port]/adminui`.

1. Navigera till **Hem > Tjänster > Program och tjänster > Programhantering**.

1. Om programmet SendLetterForReview redan finns hoppar du över de återstående stegen i den här proceduren, annars fortsätter du till nästa steg.

   ![SendLetterForReview-programmet i användargränssnittet](assets/12_applicationmanagementlc.png)

1. Klicka på **Importera**.

1. Klicka på **Välj Arkiv** och välj SendLetterForReview.lca.

   ![Välj filen SendLetterForReview.lca](assets/14_sendletterforreview_lca.png)

1. Klicka på **Förhandsgranska**.

1. Välj **Distribuera resurser till körningsmiljön när importen är klar**.

1. Klicka på **Importera**.

#### Lägger till ServiceName i listan över tjänster i Tillåtelselista {#adding-servicename-to-the-allowlist-service-list}

I Experience Manager-servern anger du vilka LiveCycle-tjänster du vill få åtkomst till Experience Manager-servern.

1. Logga in som administratör på `https:/[host]:'port'/system/console/configMgr`.

1. Leta upp och klicka på **Adobe LiveCycle Client SDK Configuration**. Konfigurationspanelen för Adobe LiveCycle Client SDK visas.
1. Klicka på +-ikonen i listan Tjänstnamn och lägg till ett serviceName **SendLetterForReview/SendLetterForReviewProcess**.

1. Klicka på **Spara**.

#### Konfigurera e-posttjänsten {#configure-the-email-service}

I det här fallet måste du konfigurera e-posttjänsten i LiveCycle-servern för att Correspondence Management ska kunna skicka ett e-postmeddelande.

1. Logga in med administratörsuppgifter för LiveCycle Server-administratörer på `https:/[lc server]:[lc port]/adminui`.

1. Navigera till **Hem > Tjänster > Program och tjänster > Tjänsthantering**.

1. Leta upp och klicka på **EmailService**.

1. Konfigurera e-posttjänsten i **SMTP-värden**.

1. Klicka på **Spara**.

#### Konfigurera DSC-tjänsten {#configure-the-dsc-service}

Om du vill använda Correspondence Management API hämtar du DSCSample.jar (som bifogas i det här dokumentet som en del av components.zip) och överför den till LiveCycle-servern. När filen DSCSample.jar har överförts till LiveCycle-servern använder Experience Manager-servern filen DSCSample.jar för att komma åt API:t renderLetter.

Mer information finns i [Ansluta AEM Forms till Adobe LiveCycle](/help/forms/using/aem-livecycle-connector.md).

1. Uppdatera Experience Manager server-URL:en i cmsa.properties i DSCSample.jar, som finns på följande plats:

   DSCSample.jar\com\adobe\livecycle\cmsa.properties

1. Ange följande parametrar i konfigurationsfilen:

   * **crx.serverUrl**=https:/host:port/[kontextsökväg]/[AEM URL]
   * **crx.username**= Experience Manager-användarnamn
   * **crx.password**= Experience Manager-lösenord
   * **crx.appRoot**=/content/apps/cm

   >[!NOTE]
   >
   >Varje gång du gör några ändringar på servern startar du om LiveCycle Server.

   Filen DSCSample.jar använder API:t renderLetter. Mer information om API:t renderLetter finns i [Interface LetterRenderService](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/icc/ddg/api/LetterRenderService.html).

#### Importera DSC till LiveCyle {#import-dsc-to-livecyle}

DSCSample.jar-filen använder API:t renderLetter för att återge bokstaven som PDF bytes från XML-data som DSC anger som indata. Mer information om renderLetter och andra API:er finns i [tjänsten Letter Render](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/icc/ddg/api/LetterRenderService.html).

1. Starta Workbench och logga in.
1. Välj **Fönster > Visa vyer > Komponenter**. Vyn Komponenter läggs till i Workbench ES2.

1. Högerklicka på **Komponenter** och välj **Installera komponent**.

1. Markera filen **DSCSample.jar** i filläsaren och klicka på **Öppna**.
1. Högerklicka på **RenderWrapper** och välj **Start Component**. Om komponenten startar visas en grön pil bredvid komponentnamnet.

## Skicka brev för granskning {#send-letter-for-review}

När du har konfigurerat åtgärden och knappen för att skicka brevet för granskning:

1. Rensa webbläsarcachen.

1. Klicka på **Bokstavsgranskning** i användargränssnittet för att skapa korrespondens och ange granskarens e-post-ID.

1. Klicka på **Skicka**.

![sendreview](assets/sendreview.png)

Granskaren får ett e-postmeddelande från systemet med brevet som en bifogad PDF-fil.
