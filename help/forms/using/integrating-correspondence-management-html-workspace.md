---
title: Integrera tredjepartsprogram i AEM Forms arbetsyta
description: Integrera tredjepartsprogram som Correspondence Management i AEM Forms.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: Admin, User, Developer
exl-id: b383fa80-1643-41f3-bc6c-176727fea688
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 0%

---

# Integrera tredjepartsprogram i AEM Forms arbetsyta{#integrating-third-party-applications-in-aem-forms-workspace}

På AEM Forms arbetsyta kan man hantera uppgifter och fylla i blanketter och dokument. Dessa formulär och dokument kan vara XDP Forms-, Flex®-formulär eller stödlinjer (borttagna) som har återgetts i XDP-, PDF-, HTML- eller Flex-format.

Dessa funktioner har förbättrats ytterligare. AEM Forms har nu stöd för samarbete med tredjepartsprogram som stöder funktioner som liknar AEM Forms arbetsyta. En vanlig del av den här funktionen är arbetsflödet för tilldelning och efterföljande godkännande av en uppgift. AEM Forms ger en enhetlig upplevelse för AEM Forms Enterprise-användare så att alla sådana tilldelningar eller godkännanden av de program som stöds kan hanteras via arbetsytan i AEM Forms.

Låt oss som exempel se Correspondence Management som ett kandidatexempel för integrering med AEM Forms arbetsyta. Correspondence Management har begreppet&quot;Letter&quot; som kan återges och möjliggöra åtgärder.

## Skapa Correspondence Management-resurser {#create-correspondence-management-assets}

Börja med att skapa en Correspondence Management-exempelmall som renderas i AEM Forms arbetsyta. Mer information finns i [Skapa en brevmall](../../forms/using/create-letter.md).

Gå till Correspondence Management-mallen på dess URL för att kontrollera om Correspondence Management-mallen kan återges korrekt. URL:en har ett mönster som liknar `https://'[server]:[port]'/lc/content/cm/createcorrespondence.html?cmLetterId=encodedLetterId&cmUseTestData=1&cmPreview=0;`

Där `encodedLetterId` är det URL-kodade brev-ID:t. Ange samma bokstav-ID när du definierar återgivningsprocessen för arbetsyteaktiviteten i Workbench.

## Skapa en uppgift att återge och skicka ett brev i AEM Workspace {#create-a-task-to-render-and-submit-a-letter-in-aem-workspace}

Innan du utför dessa steg måste du kontrollera att du är medlem i följande grupper:

* cm-agent-users
* Workspace-användare

Mer information finns i [Lägga till och konfigurera användare](/help/forms/using/admin-help/adding-configuring-users.md).

Följ de här stegen för att skapa en uppgift att återge och skicka ett brev i AEM Workspace:

1. Starta Workbench. Logga in på localhost som administratör.
1. Klicka på Arkiv > Nytt > Program. Ange `CMDemoSample` i fältet Programnamn och klicka sedan på Slutför.
1. Välj `CMDemoSample/1.0` och högerklicka på `NewProcess`. Ange `CMRenderer` i namnfältet och klicka sedan på Slutför.
1. Dra aktivitetsväljaren för startpunkten och konfigurera den:

   1. I Presentationsdata väljer du Använd en CRX-resurs.

      ![useacrxasset](assets/useacrxasset.png)

   1. Bläddra efter en resurs. I dialogrutan Välj formulärresurs visas alla bokstäver på servern på fliken Bokstäver.

      ![Fliken Bokstaven](assets/letter_tab_new.png)

   1. Välj lämplig bokstav och klicka på **OK**.

1. Klicka på Hantera åtgärdsprofiler. Dialogrutan Hantera åtgärdsprofil visas. Kontrollera att Återgivningsprocessen och Skicka process är korrekt markerade.
1. Om du vill öppna brevet med en XML-datafil bläddrar du till och väljer lämplig datafil i Förbered dataprocess.
1. Klicka på OK.
1. Definiera variablerna för startpunktsutdata och uppgiftsbilagor. De definierade variablerna innehåller data för startpunktsutdata och uppgiftsbilagor.
1. (Valfritt) Om du vill lägga till en annan användare i arbetsflödet drar du en aktivitetsväljare, konfigurerar den och tilldelar den till en användare. Skriv en anpassad wrapper (exemplet nedan) eller ladda ned och installera DSC (se nedan) för att extrahera Letter-mallen, startpunktsutdata och uppgiftsbilaga.

   Ett exempel på en anpassad wrapper visas nedan:

   ```javascript
   public LetterInstanceInfo getLetterInstanceInfo(Document dataXML) throws Exception {
   try {
   if(dataXML == null)
   throw new Exception("dataXML is missing");
   
   CoreService coreService = getRemoteCoreService();
   if (coreService == null)
   throw new Exception("Unable to retrive service. Please verify connection details.");
   Map<String, Object> result = coreService.getLetterInstanceInfo(IOUtils.toString(dataXML.getInputStream(), "UTF-8"));
   LetterInstanceInfo letterInstanceInfo = new LetterInstanceInfo();
   
   List<Document> attachmentDocs = new ArrayList<Document>();
   List<byte[]> attachments = (List<byte[]>)result.get(CoreService.ATTACHMENT_KEY);
   if (attachments != null){
   for (byte[] attachment : attachments)
   { attachmentDocs.add(new Document(attachment)); }
   
   }
   letterInstanceInfo.setLetterAttachments(attachmentDocs);
   
   byte[] updateLayout = (byte[])result.get(CoreService.LAYOUT_TEMPLATE_KEY);
   if (updateLayout != null)
   { letterInstanceInfo.setLetterTemplate(new Document(updateLayout)); }
   
   else
   { throw new Exception("template bytes missing while getting Letter instance Info."); }
   
   return letterInstanceInfo;
   } catch (Exception e)
   { throw new Exception(e); }
   
   }
   ```

   [Hämta fil](assets/dscsample.zip)
Hämta DSC: Ett exempel på DSC finns i filen DSCSample.zip som bifogas ovan. Hämta och zippa upp filen DSCSample.zip. Innan du använder DSC-tjänsten måste du konfigurera den. Se [Konfigurera DSC-tjänsten](../../forms/using/add-action-button-in-create-correspondence-ui.md#p-configure-the-dsc-service-p).

   I dialogrutan Definiera aktivitet väljer du lämplig aktivitet, till exempel getLetterInstanceInfo, och klickar på **OK**.

1. Distribuera programmet. Om du uppmanas att checka in och spara resurserna.
1. Logga in på arbetsytan för AEM-formulär på https://&#39;[server]:[port]/lc/content/ws.
1. Öppna den uppgift du lagt till, CMRenderer. Correspondence Management-brevet visas.

   ![cminworkspace](assets/cminworkspace.png)

1. Fyll i de uppgifter som krävs och skicka brevet. Fönstret stängs. I den här processen tilldelas uppgiften den användare som anges i arbetsflödet i steg 9.

   >[!NOTE]
   >
   >Knappen Skicka är inte aktiverad förrän alla obligatoriska variabler i brevet har fyllts i.
