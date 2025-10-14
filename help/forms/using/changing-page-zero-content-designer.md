---
title: Ändra sidans nollinnehåll i Designer
description: Vet du hur du kan ändra meddelandet som visas på sidnoll i en XFA PDF när du visar det i ett visningsprogram som inte kommer från Adobe PDF?
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Forms Designer,Designer
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: f966f5fb-338a-4d8e-91c6-aa0eddd03420
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Ändra sidans nollinnehåll i Designer {#changing-page-zero-content-in-designer}

Sidnollinnehåll visas som standard när ett visningsprogram som inte är från Adobe PDF, t.ex. PDF standardvisningsprogram i [!DNL Chrome] eller [!DNL Firefox], inte kan läsa innehållet i PDF/XFA-formuläret. Standardmeddelandet Sidan är noll visas nedan.

![standardsida0meddelande](assets/defaultpage0message.png)

[!DNL AEM Forms]-versionen av Designer låter dig ändra meddelandet som visas på sidan noll. Gör så här om du vill ändra sidans nollmeddelande:

1. Kontrollera att du har [!DNL AEM Forms]-versionen av Designer installerad. Du kan kontrollera versionen från Om-skärmen i Designer.

1. Öppna formuläret som du vill ändra sidans nollinnehåll för.

1. Klicka på **[!UICONTROL File]** > **[!UICONTROL Form Properties]**.

1. I dialogrutan [!UICONTROL Form Properties] klickar du på ![&#x200B; plus](assets/plus.png) (plusikonen) för att lägga till en anpassad egenskap.

1. Ange **_pagezerocontent** som egenskapens namn.
1. Lägg till det nya sidans nollmeddelande i RTF-format som värde. Till exempel:


   `<body xmlns="http://www.w3.org/1999/xhtml" xmlns:xfa="http://www.xfa.org/schema/xfa-data/1.0/"><p style="font-family:'Times';font-size:12pt;letter-spacing:0in"><span style="xfa-spacerun:yes"> </span></p><p style="font-family:'Times';font-size:12pt;letter-spacing:0in">You are seeing this message maybe because you are using a non Adobe PDF Viewer or an Old version of Adobe Reader. You can upgrade to the latest version of Adobe Reader for Windows, Mac, or Linux by visiting <span style="xfa-spacerun:yes"> </span>https://www.adobe.com/go/reader_download_se.</p><p style="font-family:'Times';font-size:12pt;letter-spacing:0in"><span style="xfa-spacerun:yes"> </span></p><p style="font-family:'Times';font-size:12pt;letter-spacing:0in">For more assistance with Adobe Reader visit <span style="xfa-spacerun:yes"> </span>https://www.adobe.com/go/acrreader.</p></body>`

1. Spara formuläret som PDF.

1. Visa PDF-formuläret i webbläsaren för att bekräfta att meddelandet har uppdaterats. Exempelvärdet ovan ser ut så här:

   ![ändrat meddelande](assets/changedmessage.png)

>[!NOTE]
>
>Den anpassade egenskap som du skapade kanske inte visas korrekt i dialogrutan Formuläregenskaper när du öppnar formuläret igen. Det fungerar dock bra och formuläret visar det uppdaterade sidnollmeddelandet.
