---
title: Lägg till anpassad åtgärd i resurslista-vyn
description: I den här artikeln lär du dig hur du lägger till anpassade åtgärder i vyn Resurslista
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
docset: aem65
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: a27c4469-6430-4a68-ba2c-7fb0ddebb263
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1333'
ht-degree: 0%

---

# Lägg till anpassad åtgärd i resurslista-vyn{#add-custom-action-to-the-asset-listing-view}

## Ökning {#overview}

Med Correspondence Management-lösningen kan du lägga till anpassade åtgärder i användargränssnittet i Hantera Assets.

Du kan lägga till en anpassad åtgärd i resurslista för:

* En eller flera resurstyper eller bokstäver
* Körning (åtgärd/kommando blir aktivt) vid val av enstaka, flera resurser/bokstäver, eller utan markering

Den här anpassningen visas med scenariot som lägger till kommandot &quot;Hämta platt PDF&quot; i resurslista för brev. Med det här anpassningsscenariot kan användarna ladda ned PDF som ett enstaka valt brev.

### Förutsättningar {#prerequisites}

Om du vill slutföra följande scenario eller liknande behöver du känna till:

* CRX
* JavaScript
* Java™

## Scenario: Lägg till ett kommando i användargränssnittet i listan Bokstäver för att ladda ned en platt PDF-version av ett brev {#addcommandtoletters}

Stegen nedan lägger till kommandot &quot;Ladda ned platta PDF&quot; i vyn Resurslista för brev och gör det möjligt för användarna att ladda ned platta PDF av det valda brevet. Med dessa steg och rätt kod och parametrar kan du lägga till andra funktioner för en annan resurs, till exempel dataordlistor eller texter.

Följ de här stegen för att anpassa Correspondence Management så att användarna kan hämta ett vanligt PDF-brev:

1. Gå till `https://'[server]:[port]'/[ContextPath]/crx/de` och logga in som administratör.

1. I mappen apps skapar du en mapp med namnet items med en sökväg/struktur som liknar mappen items i en urvalsmapp enligt följande:

   1. Högerklicka på mappen **items** på följande sökväg och välj **Overlay Node**:

      `/libs/fd/cm/ma/gui/content/cmassets/jcr:content/body/content/header/items/selection/items`

      >[!NOTE]
      >
      >Den här banan är specifik för att skapa ett funktionsmakro som fungerar med markering av en eller flera resurser/bokstäver. Om du vill skapa en åtgärd som fungerar utan markering skapar du i stället en överläggsnod för följande sökväg och slutför de återstående stegen därefter:
      >
      >
      >`/libs/fd/cm/ma/gui/content/cmassets/jcr:content/body/content/header/items/default/items`

      ![Skapa nod](assets/1_itemscreatenode.png)

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/ma/gui/content/cmassets/jcr:content/body/content/header/items/selection/items

      **Plats:** /apps/

      **Matcha nodtyper:** valda

      ![Överläggsnod](assets/2_createnodedownloadflatpdf.png)

   1. Klicka på **OK**. Mappstrukturen skapas i programmappen.

      Klicka på **Spara alla**.

1. Lägg till en nod för den anpassade knappen/åtgärden i en viss resurs (Exempel: downloadFlatPDF) i den nya objektmappen enligt följande steg:

   1. Högerklicka på mappen **items** och välj **Create** > **Create Node**.

   1. Se till att dialogrutan Skapa nod har följande värden och klicka på **OK**:

      **Namn:** downloadFlatPDF (eller namnet som du vill ge till den här egenskapen)

      **Typ:** inte:ostrukturerad

   1. Klicka på den nya noden som du har skapat (här downloadFlatPDF). CRX visar nodens egenskaper.

   1. Lägg till följande egenskaper i noden (här downloadFlatPDF) och klicka på **Spara alla**:

      <table>
        <tbody>
        <tr>
        <td><strong>Namn</strong></td>
        <td><strong>Typ</strong></td>
        <td><strong>Värde och beskrivning</strong></td>
        </tr>
        <tr>
        <td>class</td>
        <td>Sträng</td>
        <td>Foundation-collection-action</td>
        </tr>
        <tr>
        <td>Foundation-collection-action</td>
        <td>Sträng</td>
        <td><p>{"target": ".cq-manageasset-admin-childpages", "activeSelectionCount": "single","type": "LETTER"}<br /> <br /> <br /> <strong>activeSelectionCount</strong> kan vara ett eller flera för att tillåta val av en eller flera resurser som den anpassade åtgärden utförs på.</p> <p><strong>type</strong> kan vara en eller flera (kommaavgränsade flera poster) av följande: LETTER,TEXT,LIST,CONDITION,DATADICTIONARY</p> </td>
        </tr>
        <tr>
        <td>icon</td>
        <td>Sträng</td>
        <td>icon-download<br /> <br /> Den ikon som Correspondence Management visar till vänster om kommandot/menyn. Olika ikoner och inställningar finns i <a href="https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=sv-SE" target="_blank">dokumentationen för CoralUI-ikoner</a>.<br /> </td>
        </tr>
        <tr>
        <td>jcr:primärType</td>
        <td>Namn</td>
        <td>nt:ostrukturerad</td>
        </tr>
        <tr>
        <td>rel</td>
        <td>Sträng</td>
        <td>download-flat-pdf-button</td>
        </tr>
        <tr>
        <td>sling:resourceType</td>
        <td>Sträng</td>
        <td>granite/ui/components/endor/actionbar/button</td>
        </tr>
        <tr>
        <td>text</td>
        <td>Sträng</td>
        <td>Hämta platt PDF (eller någon annan etikett)<br /> <br /> Kommandot som visas i gränssnittet Resurslista</td>
        </tr>
        <tr>
        <td>title</td>
        <td>Sträng</td>
        <td>Hämta en platt PDF av det markerade brevet (eller annan etikett/alternativ text) <br /> <br /> Titeln är den alt-text som Correspondence Management visar när användaren hovrar över det anpassade kommandot.</td>
        </tr>
        </tbody>
       </table>

1. I mappen apps skapar du en mapp med namnet js med en sökväg/struktur som liknar mappen items i mappen admin med följande steg:

   1. Högerklicka på mappen **js** på följande sökväg och välj **Överläggsnod**:

      `/libs/fd/cm/ma/gui/components/admin/clientlibs/admin/js`

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/ma/gui/components/admin/clientlibs/admin/js

      **Plats:** /apps/

      **Matcha nodtyper:** valda

   1. Klicka på **OK**. Mappstrukturen skapas i programmappen. Klicka på **Spara alla**.

1. I mappen js skapar du en fil med namnet formaction.js med koden för knappens åtgärder enligt följande steg:

   1. Högerklicka på mappen **js** på följande sökväg och välj **Skapa > Skapa fil**:

      `/apps/fd/cm/ma/gui/components/admin/clientlibs/admin/js`

      Namnge filen som formaction.js.

   1. Dubbelklicka på filen för att öppna den i CRX.
   1. I filen formaction.js (under grenen /apps) kopierar du koden från filen formaction.js på följande plats:

      `/libs/fd/cm/ma/gui/components/admin/clientlibs/admin/js/formaction.js`

      Lägg sedan till följande kod i slutet av filen formaction.js (under grenen /apps) och klicka på **Spara alla**:

      ```javascript
      /* Action url for xml file to be added.*/
      var ACTION_URL = "/apps/fd/cm/ma/gui/content/commons/actionhandlers/items/letterpdfdownloader.html";
      
      /* File upload handling*/
      var fileSelectedHandler = function(e){
          if(e && e.target && e.target.value)
              $(".downloadLetterPDFBtn").removeAttr('disabled');
          else
              $(".downloadLetterPDFBtn").attr('disabled','disabled');
      }
      
      /*Handing of Download button in pop up.*/
      var downloadClickHandler = function(){
          $('#downloadLetterPDFDilaog').modal("hide");
          var element = $('.foundation-selections-item');
          var path = $(element).data("path");
          $("#fileUploadForm").attr('action', ACTION_URL + "?letterId="+path).submit();
      }
      
      /*Click handling on action button.*/
      $(document).on("click",'.download-flat-pdf-button',function(e){
          $("#uploadSamepledata").val("");
           if($('#downloadLetterPDFDilaog').length == 0){
              $(document).on("click",".downloadLetterPDFBtn",downloadClickHandler);
              $(document).on("change","#uploadSamepledata",fileSelectedHandler);
              $("body").append(downloadLetterPDFDilaog);
          }
            $('#downloadLetterPDFDilaog').modal("show");
      });
      
      /*Download popup.*/
      var downloadLetterPDFDilaog = '<div id="downloadLetterPDFDilaog" class="coral-Modal notice " role="dialog"  aria-hidden="true">'+
          '<form id="fileUploadForm" method="POST" enctype="multipart/form-data">'+
              '<div class="coral-Modal-header">'+
                  '<h2 class="coral-Modal-title coral-Heading coral-Heading--2" id="modal-header1443020790107-label" tabindex="0">Download Letter as PDF.</h2>'+
                  '<button type="button" class="coral-MinimalButton coral-Modal-closeButton" data-dismiss="modal">×</button>'+
              '</div>'+
              '<div class="coral-Modal-body" id="modal-header1443020790107-message" role="document" tabindex="0">'+
                  '<div class="coral-Modal-message">'+
                      '<p></p>'+
                  '</div>'+
                  '<div class="coral-Modal-uploader">'+
                      '<p>Select sample data for letter.</p>'+
                      '<input type="file" id="uploadSamepledata" name="file" accept=".xml" size="70px">'+
                  '</div>'+
              '</div>'+
           '</form>'+
              '<div class="coral-Modal-footer">'+
                  '<button type="button" class="coral-Button" data-dismiss="modal">Cancel</button>'+
                  '<button type="button" class="coral-Button coral-Button--primary downloadLetterPDFBtn" disabled="disabled">Download</button>'+
              '</div>'+
      '</div>';
      ```

      Koden som du lägger till i det här steget åsidosätter koden under libs-mappen, så kopiera den föregående koden till formaction.js-filen i /apps-grenen. Om du kopierar koden från grenen /libs till grenen /apps säkerställer du att den tidigare funktionen också fungerar.

      Koden ovan gäller bokstavsspecifik åtgärdshantering för det kommando som skapas i den här proceduren. Ändra JavaScript-koden om du vill hantera andra resurser.

1. I mappen apps skapar du en mapp med namnet items med en sökväg/struktur som liknar mappen items i mappen actionhandlers med följande steg:

   1. Högerklicka på mappen **items** på följande sökväg och välj **Overlay Node**:

      `/libs/fd/cm/ma/gui/content/commons/actionhandlers/items/`

   1. Kontrollera att dialogrutan Overlay Node har följande värden:

      **Sökväg:** /libs/fd/cm/ma/gui/content/commons/actionhandlers/items/

      **Plats:** /apps/

      **Matcha nodtyper:** valda

   1. Klicka på **OK**. Mappstrukturen skapas i programmappen.

   1. Klicka på **Spara alla**.

1. Under noden för nyligen skapade objekt lägger du till en nod för den anpassade knappen/åtgärden i en viss resurs (Exempel: letterpdfdownloader) enligt följande steg:

   1. Högerklicka på objektmappen och välj **Skapa > Skapa nod**.

   1. Se till att dialogrutan Skapa nod har följande värden och klicka på **OK**:

      **Namn:** letterpdfdownloader (eller namnet som du vill ge den här egenskapen) måste vara unikt. Om du använder ett annat namn här anger du samma namn i formaction.js-filens ACTION_URL-variabel.)

      **Typ:** inte:ostrukturerad

   1. Klicka på den nya noden som du har skapat (här downloadFlatPDF). CRX visar nodens egenskaper.

   1. Lägg till följande egenskap i noden (här letterpdfdownloader) och klicka på **Spara alla**:

      | **Namn** | **Typ** | **Värde** |
      |---|---|---|
      | sling:resourceType | Sträng | fd/cm/ma/gui/components/admin/clientlibs/admin |

1. Skapa en fil med namnet POST.jsp och koden för hur kommandot hanteras på följande plats:

   /apps/fd/cm/ma/gui/components/admin/clientlibs/admin

   1. Högerklicka på mappen **admin** på följande sökväg och välj **Skapa > Skapa fil**:

      /apps/fd/cm/ma/gui/components/admin/clientlibs/admin

      Ge filen namnet POST.jsp. (Filnamnet behöver bara vara POST.jsp.)

   1. Dubbelklicka på filen **POST.jsp** för att öppna den i CRX.
   1. Lägg till följande kod i filen POST.jsp och klicka på **Spara alla**:

      Den här koden är specifik för bokstavsåtergivningstjänsten. För alla andra resurser lägger du till resursens Java™-bibliotek i den här koden. Mer information om AEM Forms API:er finns i [AEM Forms API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=sv-SE).

      Mer information om AEM-bibliotek finns i AEM [Komponenter](/help/sites-developing/components.md).

      ```xml
      /*Import libraries. Here we are downloading letter flat pdf with input xml data so we require letterRender Api. For any other Module functionality we need to first import that library. */
      <%@include file="/libs/foundation/global.jsp"%>
      <!DOCTYPE html lang="en" PUBLIC "-//W3C//DTD XHTML 1.1//EN" "https://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
      <%@page import="com.adobe.icc.ddg.api.*"%>
      <%@page import="com.adobe.icc.dbforms.obj.*"%>
      <%@page import="com.adobe.icc.render.obj.*" %>
      <%@page import="com.adobe.icc.services.api.*" %>
      <%@page import="org.apache.sling.api.resource.*" %>
      <%@page import="java.io.File" %>
      <%@page import="java.util.*" %>
      <%@page import="com.adobe.livecycle.content.appcontext.AppContextManager"%>
      <%@page import=" com.adobe.icc.dbforms.exceptions.ICCException"%>
      <%@page import="java.io.InputStream" %>
      <%@page import="java.io.FileInputStream" %>
      <%@page import="org.apache.commons.io.IOUtils" %>
      <%@page session="false" contentType="text/html; charset=utf-8"%>
      <%@taglib prefix="sling" uri="https://sling.apache.org/taglibs/sling/1.0"%>
      <%@taglib prefix="cq" uri="https://www.day.com/taglibs/cq/1.0" %>
       <%@page session="false" contentType="text/html; charset=utf-8"%>
      <%
         AppContextManager.setCurrentAppContext("/content/apps/cm");
         /*Get letter id sent in js file.*/
          String letterId = request.getParameter("letterId");
          if(letterId.lastIndexOf("?") != -1)
              letterId = letterId.substring(0, letterId.indexOf("?"));
          String fileName = null;
          String letterName = null;
          InputStream inputStream = null;
          /*Get xml file data*/
          if (slingRequest.getRequestParameter("file") != null)
              inputStream = slingRequest.getRequestParameter("file").getInputStream();
          if(letterId != null){
              String xmlData = null;
              try{
                  xmlData = IOUtils.toString(inputStream, "UTF-8");
              }
              catch (Exception e) {
                  log.error("Xml data does not exists.");
              }
              /*letter Name from letter letter id.*/
              letterName = letterId.substring(letterId.lastIndexOf("/")+1);
              /*Invoking letter render services API.*/
              LetterRenderService letterRenderService = sling.getService(LetterRenderService.class);
              /*using CM renderLetter api to get pdfbytes.*/
              PDFResponseType  pdfResponseType= letterRenderService.renderLetter(letterId,xmlData,true,false,false,false);
              byte[] bytes = null;
              /*Downloading pdf bytes as pdf.*/
              if(pdfResponseType != null && pdfResponseType.getFile() != null){
                  bytes = pdfResponseType.getFile().getDocument();
                  /*set the response header to enable download*/
                  response.setContentType("application/OCTET-STREAM");
                  response.setHeader("Content-Disposition", "attachment;filename=\"" + letterName + ".pdf\"");
                  response.setHeader("Pragma", "cache");
                  response.setHeader("Cache-Control", "private");
                  out.clear();
                  response.getOutputStream().write(bytes);
              }
          }
          else{
              log.error("Letter id does not exists.");
          }
      %>
      ```

## Ladda ned en PDF-fil av ett brev med de anpassade funktionerna {#download-flat-pdf-of-a-letter-using-the-custom-functionality}

När du har lagt till en anpassad funktion för att ladda ned en PDF-version av breven kan du använda följande steg för att ladda ned en platt PDF-version av det brev du valt:

1. Gå till `https://'[server]:[port]'/[ContextPath]/projects.html` och logga in.

1. Välj **Forms > Bokstäver**. Correspondence Management listar bokstäverna som finns i systemet.
1. Klicka på **Markera** och sedan på en bokstav för att markera den.
1. Välj **Mer** > **&lt;Hämta platta PDF>** (Den anpassade funktionaliteten som skapas med instruktionerna i den här artikeln). Dialogrutan Hämta brev som PDF visas.

   Menyalternativets namn, funktion och alt-text följer den anpassning som har skapats i [Scenario: Lägg till ett kommando i användargränssnittet för bokstavslistan för att ladda ned en platt PDF-version av ett brev.](#addcommandtoletters)

   ![Anpassade funktioner: Ladda ned platta PDF](assets/5_downloadflatpdf.png)

1. I dialogrutan Hämta brev som PDF väljer du den XML-kod som du vill fylla i data från i PDF.

   >[!NOTE]
   >
   >Innan du laddar ned bokstaven som en platt PDF kan du skapa XML-filen med informationen i brevet med alternativet **Skapa rapport** .

   ![Hämta brev som PDF](assets/6_downloadflatpdf.png)

   Brevet laddas ned till din dator som en platt PDF.
