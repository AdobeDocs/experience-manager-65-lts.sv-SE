---
title: Arbeta med projektarbetsflöden
description: Det finns en mängd olika projektarbetsflöden att välja mellan.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: projects
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Workflow
role: User,Admin,Architect,Developer
exl-id: eb19a269-8b3f-476b-b22b-8116dce70388
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 0%

---

# Arbeta med projektarbetsflöden {#working-with-project-workflows}

De projektarbetsflöden som är tillgängliga från paketet innehåller följande:

* **Arbetsflöde för projektgodkännande** - Med det här arbetsflödet kan du tilldela innehåll till en användare, granska och sedan godkänna.
* **Begär start** - Ett arbetsflöde som begär start.
* **Begär landningssida** - Den här arbetsflödet begär en landningssida.
* **Begär e-post** - Arbetsflöde för att begära e-post.
* **Produktfoto och produktfoto (Commerce)** - Mappar resurser med produkter
* **DAM Skapa och översätt kopia och DAM Skapa språkkopia** - Skapar översatta binärfiler, metadata och taggar för resurser och mappar.

Beroende på vilken projektmall du väljer finns det vissa arbetsflöden:

|   | **Enkelt projekt** | **Medieprojekt** | **Fotoprojekt för produkt** | **Översättningsprojekt** |
|---|:-:|:-:|:-:|:-:|
| Begär kopia |  | x |  |  |
| Fotofotografering |  | x | x |  |
| Fotofoto (Commerce) |  |  | x |  |
| Projektgodkännande | x |  |  |  |
| Begär start | x |  |  |  |
| Begär landningssida | x |  |  |  |
| Begär e-post | x |  |  |  |
| DAM - skapa språkkopia&ast; |  |  |  | x |
| DAM Skapa och översätt språkkopia&ast; |  |  |  | x |

>[!NOTE]
>
>&ast; dessa arbetsflöden startas inte från rutan **Arbetsflöde** i Projekt. Se [Skapa språkkopior för Assets.](/help/sites-administering/tc-manage.md)

Stegen för att starta och slutföra arbetsflöden är desamma oavsett vilket arbetsflöde du väljer. Bara stegen ändras.

Du startar ett arbetsflöde direkt i Projekt (förutom för DAM Create Language Copy eller DAM Create och Translate Language Copy). Information om väntande uppgifter i ett projekt visas i rutan **Uppgifter**. Meddelanden om uppgifter som behöver slutföras visas bredvid användarikonen.

Mer information om hur du arbetar med arbetsflöden i AEM finns i följande dokument:

* [Delta i arbetsflöden](/help/sites-authoring/workflows-participating.md)
* [Tillämpa arbetsflöden på sidor](/help/sites-authoring/workflows-applying.md)
* [Konfigurera arbetsflöden](/help/sites-administering/workflows.md)

I det här avsnittet beskrivs de arbetsflöden som är tillgängliga för projekt.

## Begär kopieringsarbetsflöde {#request-copy-workflow}

Med det här arbetsflödet kan du begära ett manuskript från en användare och sedan godkänna det. Så här startar du arbetsflödet för begärandekopia:

1. I ett medieprojekt klickar du på nedåtpilen längst upp till höger i rutan **Arbetsflöden** och väljer **Starta arbetsflöde**.
1. Välj **Begär kopia** i arbetsflödesguiden och klicka på **Nästa**.
1. Ange ett manuskriptnamn och en kort sammanfattning av vad du begär. Ange ett målordsantal, uppgiftsprioritet och ett förfallodatum om tillämpligt.

   ![Begär kopieringsarbetsflöde](assets/project-request-copy-workflow.png)

1. Klicka på **Skicka**.

Arbetsflödet startar. Uppgiften visas på kortet **Åtgärder**.

## Arbetsflöde för fotografering {#product-photo-shoot-workflow}

Arbetsflödena för **produktfototagning** (både handel och utan handel) beskrivs mer ingående i dokumentet [Creative Projects](/help/sites-authoring/managing-product-information.md)

## Arbetsflöde för projektgodkännande {#project-approval-workflow}

I arbetsflödet **Projektgodkännande** tilldelar du innehåll till en användare, granskar och godkänner sedan innehållet.

1. I ett enkelt projekt klickar du på nedåtpilen längst upp till höger i rutan **Arbetsflöden** och väljer **Starta arbetsflöde**.
1. Välj **Arbetsflöde för projektgodkännande** i arbetsflödesguiden och klicka på **Nästa**.
1. Ange en titel och välj vem du vill tilldela den till. Ange en beskrivning, innehållssökväg, uppgiftsprioritet och ett förfallodatum om tillämpligt.

   ![Arbetsflöde för projektgodkännande](assets/project-approval-workflow.png)

1. Klicka på **Skicka**.

Arbetsflödet startar. Uppgiften visas på kortet **Åtgärder**.

## Begär startarbetsflöde {#request-launch-workflow}

Med det här arbetsflödet kan du begära en start.

1. I ett enkelt projekt klickar du på nedåtpilen längst upp till höger i rutan **Arbetsflöden** och väljer **Starta arbetsflöde**.
1. Välj **Begär startarbetsflöde** i arbetsflödesguiden och klicka på **Nästa**.
1. Ange en rubrik för startprogrammet och ange startkällans sökväg. Du kan också lägga till en beskrivning och ett live-datum, om du vill. Välj Ärv källsidans livedata eller exkludera undersidor beroende på hur du vill att startsidan ska fungera.

   ![Begär startarbetsflöde](assets/project-request-launch-workflow.png)

1. Klicka på **Skicka**.

Arbetsflödet startar. Arbetsflödet visas i listan **Arbetsflöden**.

## Begär arbetsflöde för landningssida {#request-landing-page-workflow}

Med det här arbetsflödet kan du begära en landningssida.

1. I ett enkelt projekt klickar du på nedåtpilen längst upp till höger i rutan **Arbetsflöden** och väljer **Starta arbetsflöde**.
1. Välj **Begär landningssida** i arbetsflödesguiden och klicka på **Nästa**.
1. Ange en rubrik för landningssidan och den överordnade sökvägen. Ange eventuellt ett live-datum eller välj en fil för landningssidan.

   ![Begär arbetsflöde för landningssida](assets/project-request-landing-page-workflow.png)

1. Klicka på **Skicka**.

Arbetsflödet startar. Uppgiften visas på kortet **Åtgärder**.

## Begär e-postarbetsflöde {#request-email-workflow}

Med det här arbetsflödet kan du begära ett e-postmeddelande. Det är samma arbetsflöde som visas i rutan **E-post**.

1. I ett enkelt projekt klickar du på nedåtpilen längst upp till höger i rutan **Arbetsflöden** och väljer **Starta arbetsflöde**.
1. Välj **Begär e-post** i arbetsflödesguiden och klicka på **Nästa**.
1. Ange en e-posttitel samt kampanj- och mallsökvägar. Dessutom kan du ange namn, beskrivning och live-datum.

   ![Begär e-postarbetsflöde](assets/project-request-email-workflow.png)

1. Klicka på **Skicka**.

Arbetsflödet startar. Uppgiften visas på kortet **Åtgärder**.

## Skapa (och översätt) språkkopieringsarbetsflöde för Assets {#create-and-translate-language-copy-workflow-for-assets}

Arbetsflödena **Skapa språkkopia** och **Skapa och översätt språkkopia** beskrivs närmare i dokumentet [Skapa språkkopior för Assets.](/help/assets/translation-projects.md)
