---
title: Asynkron inlämning av adaptiva formulär
description: Lär dig att konfigurera asynkron överföring för adaptiva formulär.
contentOwner: vishgupt
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: df2621db-59a1-4031-8856-7c2de766c099
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Asynkron inlämning av adaptiva formulär{#asynchronous-submission-of-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/configure-submit-actions-and-metadata-submission/asynchronous-submissions-adaptive-forms.html?lang=sv-SE).

Som standard är webbformulär konfigurerade att skicka synkront. När användare skickar ett formulär omdirigeras de i synkron överföring till en bekräftelsesida, en tacksida eller en felsida om det uppstår ett överföringsfel. Moderna webbupplevelser som single page-applikationer blir dock allt populärare, där webbsidan är statisk medan klient-server-interaktion sker i bakgrunden. Du kan nu ge den här upplevelsen tillgång till adaptiva formulär genom att konfigurera asynkron överföring.

När en användare skickar in ett formulär i asynkron form plugin-program för formulärutvecklare, som omdirigering till ett annat formulär eller till ett separat avsnitt på webbplatsen. Författaren kan också plugin-program för olika tjänster, som att skicka data till ett annat datalager eller lägga till en anpassad analysmotor. Om det finns asynkron överföring fungerar ett adaptivt formulär som ett program med en sida, eftersom formuläret inte läses in igen eller dess URL inte ändras när skickade formulärdata valideras på servern.

Läs vidare för mer information om asynkron överföring i adaptiva formulär.

## Konfigurera asynkron överföring {#configure}

Så här konfigurerar du asynkron sändning för ett adaptivt formulär:

1. I redigeringsläget för anpassningsbara formulär markerar du objektet Formulärbehållare och väljer ![cmpr1](assets/cmppr1.png) för att öppna dess egenskaper.
1. Aktivera **[!UICONTROL Submission]** i egenskapsavsnittet för **[!UICONTROL Use asynchronous submission]**.
1. I avsnittet **[!UICONTROL On Submit]** väljer du något av följande alternativ för att skicka formulär.

   * **[!UICONTROL Redirect to URL]**: Omdirigerar till angiven URL eller sida när formulär skickas. Du kan ange en URL eller bläddra för att välja sökvägen till en sida i fältet **[!UICONTROL Redirect URL/Path]**.
   * **[!UICONTROL Show Message]**: Visar ett meddelande om att formulär har skickats. Du kan skriva ett meddelande i textfältet under alternativet Visa meddelande. Textfältet har stöd för RTF-formatering.

1. Välj ![check-button1](assets/check-button1.png) för att spara egenskaperna.

## Hur asynkron inlämning fungerar {#how-asynchronous-submission-works}

AEM Forms har färdiga funktioner och felhanterare för att skicka in formulär. Hanterare är funktioner på klientsidan som körs baserat på serversvaret. När ett formulär skickas skickas data till servern för validering, som returnerar ett svar till klienten med information om om huruvida överföringen lyckades eller inte. Informationen skickas som parametrar till den relevanta hanteraren för att köra funktionen.

Dessutom kan formulärförfattare och utvecklare skriva regler på formulärnivå för att åsidosätta standardhanterare. Mer information finns i [Åsidosätta standardhanterare med regler](#custom).

Låt oss först granska serversvaret för att se om det har lyckats eller inte.

### Serversvar för lyckad sändning {#server-response-for-submission-success-event}

Strukturen för serversvaret för händelsen att överföringen lyckades är följande:

```json
{
  contentType : "<xmlschema or jsonschema>",
  data : "<dataXML or dataJson>" ,
  thankYouOption : <page/message>,
  thankYouContent : "<thank you page url/thank you message>"
}
```

Serversvaret om det finns ett godkänt formulär innehåller:

* Typ av formulärdataformat: XML eller JSON
* Formulärdata i XML- eller JSON-format
* Markerat alternativ för omdirigering till en sida eller för att visa ett meddelande som konfigurerats i formuläret
* Sidans URL- eller meddelandeinnehåll som konfigurerats i formuläret

Hanteraren för lyckade åtgärder läser serversvaret och dirigerar därför om till den konfigurerade sidans URL eller visar ett meddelande.

### Serversvar för en felhändelse för överföring {#server-response-for-submission-error-event}

Strukturen för serversvaret för en felhändelse vid överföring är följande:

```json
{
   errorCausedBy : "<CAPTCHA_VALIDATION or SERVER_SIDE_VALIDATION>",

   errors : [
               { "somExpression" : "<SOM Expression>",
                 "errorMessage"  : "<Error Message>"
               },
               ...
             ]
 }
```

Om ett fel uppstår när formulär skickas innehåller svaret:

* Orsak till felet, misslyckades med CAPTCHA eller validering på serversidan
* Lista över felobjekt, som innehåller SOM-uttrycket för fältet som inte kunde valideras och motsvarande felmeddelande

Felhanteraren läser serversvaret och visar därför felmeddelandet i formuläret.

## Åsidosätta standardhanterare med regler {#custom}

Formulärutvecklare och författare kan skriva regler på formulärnivå i kodredigeraren för att åsidosätta standardhanterare. Serversvaret för lyckade händelser och felhändelser visas på formulärnivå, som utvecklare kan komma åt med `$event.data` i regler.

Utför följande steg för att skriva regler i kodredigeraren för att hantera lyckade händelser och felhändelser.

1. Öppna det adaptiva formuläret i redigeringsläge, markera ett formulärobjekt och välj ![edit-rules1](assets/edit-rules1.png) för att öppna regelredigeraren.
1. Välj **[!UICONTROL Form]** i trädet Formulärobjekt och välj **[!UICONTROL Create]**.
1. Välj **[!UICONTROL Code Editor]** i listrutan för lägesval.
1. Välj **[!UICONTROL Edit Code]** i kodredigeraren. Välj **[!UICONTROL Edit]** i bekräftelsedialogrutan.
1. Välj **[!UICONTROL Successful Submission]** eller **[!UICONTROL Error in Submission]** i listrutan **[!UICONTROL Event]**.
1. Skriv en regel för den valda händelsen och välj **[!UICONTROL Done]** för att spara regeln.
