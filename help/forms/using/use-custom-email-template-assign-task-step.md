---
title: Använda anpassade e-postmallar i steget Tilldela uppgift
description: Anpassade e-postmallar för e-postmeddelanden i formulärarbetsflödet
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# Använda anpassade e-postmallar i steget Tilldela uppgift{#use-custom-email-templates-in-an-assign-task-step}

Du kan använda steget Tilldela uppgift för att skapa och tilldela uppgifter till en användare eller grupp. När en uppgift tilldelas en användare eller grupp skickas ett e-postmeddelande till den angivna användaren eller till varje medlem i den definierade gruppen. Ett vanligt e-postmeddelande innehåller en länk till den tilldelade uppgiften och information om uppgiften. I följande bild visas ett exempel på ett e-postmeddelande:

![E-postmeddelande med mall utanför rutan](do-not-localize/default_email_template_new.png)

Du kan anpassa utseendet och använda anpassade metadata i ett e-postmeddelande. AEM Forms tillhandahåller en färdig mall för e-postmeddelanden. Du kan anpassa mallen som inte finns i kartongen eller skapa en helt ny mall.

Mallar för e-postmeddelanden baseras på [HTML-e-post](https://en.wikipedia.org/wiki/HTML_email). Dessa e-postmeddelanden anpassas efter olika e-postklienter och skärmstorlekar. Dessutom definieras formatet för e-postmeddelandet i mallen.

I följande bild visas ett anpassat e-postmeddelande:

![E-postmeddelande med anpassad mall](do-not-localize/customized-email.png)

## Anpassa den befintliga mallen {#customize-the-existing-template}

När allt är klart tillhandahåller AEM Forms en mall för e-postmeddelanden. Mallen innehåller titelbeskrivning, förfallodatum, prioritet, arbetsflödets namn och länk för den tilldelade uppgiften. Du kan anpassa mallen för att ändra utseendet. Anpassa mallen genom att utföra följande steg:

1. Logga in på CRXDE med administratörskonto.

1. Gå till /libs/fd/dashboard/templates/email.

1. Öppna filen htmlEmailTemplate.txt. Den innehåller standardmallen.

1. Ersätt innehållet i filen htmlEmailTemplate.txt med anpassat innehåll.

   En e-postmeddelandemall är ett [HTML-e-postmeddelande](https://en.wikipedia.org/wiki/HTML_email). Du kan ersätta den befintliga HTML-koden med din egen kod om du vill ändra mallens utseende.

1. Spara filen. Nu är den anpassade mallen klar att användas.

## Skapa en e-postmall {#create-an-email-template}

När allt är klart tillhandahåller AEM Forms en mall för e-postmeddelanden. Mallen innehåller titelbeskrivning, förfallodatum, prioritet, arbetsflödets namn och länk för den tilldelade uppgiften. Du kan också lägga till en anpassad e-postmall (din egen mall) för Tilldela uppgiftssteg. Gör så här för att lägga till en anpassad e-postmall:

1. Logga in på CRXDE med administratörskonto.

1. Gå till /libs/fd/dashboard/templates/email.

1. Skapa en TXT-fil. Exempel: EmailOnTaskAssign.txt.

1. Lägg till anpassad HTML-kod i filen.

   En e-postmeddelandemall är ett [HTML-e-postmeddelande](https://en.wikipedia.org/wiki/HTML_email). Du kan lägga till anpassad HTML-kod i filen för att skapa en mall.

1. Spara filen. Mallen är klar att användas i steget Tilldela uppgift.

## Använda en e-postmall i steget Tilldela uppgift {#use-an-email-template-in-an-assign-task-step}

Utför åtgärden Tilldela är konfigurerad att använda standardmallen htmlEmailTemplate.txt. Du kan välja att använda en anpassad mall. Så här ändrar du mallen:

1. Öppna steget Tilldela uppgift.

1. Navigera till Tilldelad > E-postmall för HTML.

1. Markera den nya e-postmallen för HTML.

1. Klicka på OK. Mallen ändras.

Ett e-postmeddelande använder också [metadata](../../forms/using/use-metadata-in-email-notifications.md). Exempel: förfallodatum, prioritet, arbetsflödets namn med mera. Du kan också konfigurera mallen så att den använder [anpassade metadata](../../forms/using/use-metadata-in-email-notifications.md#using-custom-metadata-in-an-email-notification).
