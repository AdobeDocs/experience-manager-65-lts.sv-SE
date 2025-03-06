---
title: Dokumentinformation för återgivning
description: Konceptuell information om hur renderingar fungerar på arbetsytan i AEM Forms för att återge de olika formulär och filtyper som stöds.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: HTML5 Forms,Adaptive Forms,Mobile Forms
role: Admin, User, Developer
exl-id: e1612298-721d-499c-96c9-fd4418725106
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '666'
ht-degree: 0%

---

# Dokumentinformation för återgivning {#document-details-for-renderer}

## Introduktion {#introduction}

I AEM Forms-arbetsytan stöds flera formulärtyper sömlöst. Bland dessa finns:

* PDF forms (XDP/Acroform/platta PDF-filer)
* Nya HTML-formulär
* Bilder
* Tredjepartsprogram (till exempel Korrespondenshantering)

I det här dokumentet förklaras hur dessa renderare fungerar när det gäller semantisk anpassning/återanvändning av komponenter, så att kundens krav uppfylls utan att renderingen bryts. Även om AEM Forms arbetsyta tillåter användargränssnitt/semantiska ändringar rekommenderar vi att återgivningslogiken för olika formulärtyper inte ändras, annars kan resultatet bli oförutsägbart. Det här dokumentet är avsett som vägledning/kunskap för att ge stöd för återgivning av samma formulär, med samma arbetsytekomponenter i olika portaler, och inte för att ändra själva återgivningslogiken.

## PDF forms {#pdf-forms}

PDF forms återges av `PdfTaskForm View`.

När ett XDP-formulär återges som PDF läggs en `FormBridge` JavaScript™ till av tjänsten FormsAugmenter. Denna JavaScript™ (i PDF-formuläret) hjälper till att utföra åtgärder som att skicka formulär, spara formulär eller ta formulär offline.

I AEM Forms-arbetsytan kommunicerar PDFTaskForm-vyn med `FormBridge`JavaScript via en mellanliggande HTML som finns på `/lc/libs/ws/libs/ws/pdf.html`. Flödet är:

**Vyn PDFTaskForm - pdf.html**

Kommunicerar med `window.postMessage` / `window.attachEvent('message')`

Den här metoden är standardmetoden för kommunikation mellan en överordnad bildruta och en iframe. De befintliga händelseavlyssnarna från tidigare öppnade PDF forms tas bort innan en ny läggs till. Denna rensning tar även hänsyn till växlingen mellan fliken Formulär och fliken Historik i vyn med aktivitetsinformation.

**pdf.html - `FormBridge`JavaScript inuti den återgivna PDF**

Kommunicerar med `pdfObject.postMessage` / `pdfObject.messageHandler`

Den här metoden är standardmetoden för kommunikation med en PDFJavaScript från en HTML. Vyn PdfTaskForm hanterar också en platt PDF och renderar den.

>[!NOTE]
>
>Vi rekommenderar inte att du redigerar innehållet i pdf.html / i PDFTaskForm-vyn.

## Nya HTML Forms {#new-html-forms}

Nya HTML-formulär återges i vyn NewHTMLTaskForm.

När ett XDP-formulär återges som HTML med mobilformulärspaketet som distribueras på CRX, läggs även ytterligare `FormBridge`JavaScript till i formuläret, som visar olika metoder för att spara och skicka formulärdata.

Denna JavaScript skiljer sig från den som nämns i PDF forms ovan, men har ett liknande syfte.

>[!NOTE]
>
>Adobe rekommenderar inte att du redigerar innehållet i vyn NewHTMLTaskForm.

## Flex Forms och stödlinjer {#flex-forms-and-guides}

Flex Forms återges av SWFTaskForm och stödlinjer återges av HTMLTaskForm-vyer.

I AEM Forms-arbetsytan kommunicerar dessa vyer med den faktiska SWF som utgör formuläret/guiden för Flex® med hjälp av en mellanhand i SWF som finns på `/lc/libs/ws/libs/ws/WSNextAdapter.swf`

Kommunikationen sker med `swfObject.postMessage` / `window.flexMessageHandler`.

Detta protokoll definieras av `WsNextAdapter.swf`. Det befintliga `flexMessageHandlers` i fönsterobjektet, från tidigare öppnade SWF-formulär, tas bort innan ett nytt läggs till. Logiken tar också hänsyn till växlingen mellan fliken Formulär och fliken Historik i vyn med uppgiftsdetaljer. `WsNextAdapter.swf` används för att utföra olika formuläråtgärder som att spara eller skicka.

>[!NOTE]
>
>Du bör inte ändra `WSNextAdapter.swf` eller innehållet i vyn SWfTaskForm/HtmlTaskForm.

## Tredjepartsprogram (till exempel Korrespondenshantering) {#third-party-applications-for-example-correspondence-management}

Tredjepartsprogram återges med vyn ExtAppTaskForm.

**Tredjepartsprogram för kommunikation på AEM Forms-arbetsytan**

AEM Forms arbetsyta lyssnar på `window.global.postMessage([Message],[Payload])`

[Meddelande] kan vara en sträng som anges som `SubmitMessage`| `CancelMessage`| `ErrorMessage`| `actionEnabledMessage` i `runtimeMap` . Tredjepartsprogram måste använda det här gränssnittet för att meddela AEM Forms-arbetsytan efter behov. Det är obligatoriskt att använda det här gränssnittet eftersom AEM Forms-arbetsytan måste känna till att när uppgiften skickas så att den kan rensa upp aktivitetsfönstret.

**AEM Forms arbetsyta till kommunikation mellan program från tredje part**

Om direktåtgärdsknapparna för AEM Forms-arbetsytan visas anropas `window.[External-App-Name].getMessage([Action])`, där `[Action]` läses från `routeActionMap`. Tredjepartsprogrammet måste lyssna på det här gränssnittet och sedan meddela AEM Forms-arbetsytan via API:t `postMessage ()`.

Ett Flex-program kan till exempel definiera `ExternalInterface.addCallback('getMessage', listener)` som stöd för den här kommunikationen. Om tredjepartsprogrammet vill hantera formulärsändning med sina egna knappar, bör du ange `hideDirectActions = true() in the runtimeMap` och du kan hoppa över den här lyssnaren. Denna konstruktion är alltså valfri.

Du kan läsa mer om integrering av tredjepartsprogram för Korrespondenshantering på [Integrera korrespondenshantering i AEM Forms arbetsyta](/help/forms/using/integrating-correspondence-management-html-workspace.md).
