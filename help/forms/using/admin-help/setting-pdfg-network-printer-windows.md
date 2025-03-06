---
title: Konfigurera en PDFG-nätverksskrivare (endast Windows)
description: Lär dig hur du konfigurerar en PDFG-nätverksskrivare (endast Windows)
feature: PDF Generator
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 6e9c42d9-fb1d-432b-95b9-6e21706b2a3e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# Konfigurera en PDFG-nätverksskrivare (endast Windows) {#setting-up-a-pdfg-network-printer-windows-only}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Med PDFG Network Printer kan man generera PDF-dokument från alla program som stöder utskrift. När en användare har installerat PDFG Network Printer visas en ny skrivare med namnet *PDF generator* i skrivaravsnittet på Kontrollpanelen i Windows. Om det redan finns en skrivare med samma namn uppmanas användaren att ange ett annat namn.

Om du skriver ut på den här skrivaren från ett program skickas dokumentet (i PostScript-format) till PDF Generator, som konverterar PostScript-filen till PDF. Beroende på hur du har konfigurerat PDF Generator skickas PDF-dokumentet till användaren som en bilaga till ett e-postmeddelande, PDF-dokumentet vidarebefordras till en angiven AEM-formulärtjänst eller -process, eller båda åtgärderna utförs.

Följande steg krävs för att konfigurera en PDFG-nätverksskrivare:

1. Konfigurera e-postinställningar. (Se [Konfigurera e-postinställningar för PDFG-nätverksskrivare](setting-pdfg-network-printer-windows.md#configure-email-settings-for-pdfg-network-printer).)
1. Konfigurera inställningarna för PDFG Network Printer i administrationskonsolen. (Se [Konfigurera inställningarna för PDFG-nätverksskrivaren](setting-pdfg-network-printer-windows.md#configure-the-pdfg-network-printer-settings).)
1. Se till att dina användare är konfigurerade med en giltig e-postadress i AEM formulärdatabas och tilldela PDFGUserPermission till varje användare. <!-- Fix broken link See Setting up and organizing users -->
1. Kontrollera att 32-bitars JRE6 är installerat på användarnas datorer.
1. Installera skrivaren på användarnas datorer. (Se [Installera PDFG-nätverksskrivare på en användares dator](setting-pdfg-network-printer-windows.md#install-pdfg-network-printer-on-a-user-s-computer).)

## Konfigurera e-postinställningar för PDFG-nätverksskrivare {#configure-email-settings-for-pdfg-network-printer}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. På sidan Tjänsthantering klickar du på provider.email_sendmail_service, anger SMTP-inställningarna och klickar på Spara.

## Konfigurera inställningar för PDFG-nätverksskrivare {#configure-the-pdfg-network-printer-settings}

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > PDFG-nätverksskrivare
1. I listorna Adobe PDF-inställningar och Skyddsinställningar väljer du de alternativ som ska gälla för den genererade PDF. Mer information om de här inställningarna finns i [Konfigurera Adobe PDF-inställningar](/help/forms/using/admin-help/configuring-pdf-settings.md#configuring-adobe-pdf-settings) och [Konfigurera säkerhetsinställningar](/help/forms/using/admin-help/configuring-security-settings.md#configuring-security-settings).
1. Om du vill skicka tillbaka de konverterade PDF-filerna till användarna markerar du alternativet E-posta den konverterade PDF-filen tillbaka till användaren och anger följande information:

   * E-postadressen som ska användas för att skicka PDF-filer till användarna
   * Ämnet för e-postmeddelandet
   * E-postmeddelandets sidhuvud, brödtext och sidfot. I e-postmeddelandet ersätts &lt;receiverName> med det fullständiga namnet på den användare som skrev ut dokumentet.

1. Om du vill skicka de konverterade PDF-filerna till en AEM-formulärtjänst eller -process väljer du alternativet Vidarebefordra den konverterade PDF till den angivna AEM-formulärtjänsten eller -processen och anger följande information:

   * Namnet på tjänsten som ska anropas
   * Namnet på åtgärden för tjänsten som ska anropas
   * Namnet på indataparametern, enligt specifikationen i filen component.xml för tjänsten eller processen. PDF-dokumentet används som ett värde för den indataparametern.

1. Klicka på Spara.

Om du vill återgå till den ursprungliga standardtexten för e-post klickar du på Återställ e-postinnehåll.

## Installera PDFG-nätverksskrivare på en användares dator {#install-pdfg-network-printer-on-a-user-s-computer}

Användare som har rollen PDFG-administratör eller PDFG-användare kan installera en PDFG-nätverksskrivare. Du måste ha en 32-bitars JDK installerad på datorn.

1. (PDFG-administratörer) I administrationskonsolen klickar du på Tjänster > PDF Generator > PDFG-nätverksskrivare.

   (PDFG-användare) Gå till `http(s)://[host]:'port'/pdfgui` och klicka på länken under Installation av PDFG-nätverksskrivare.

1. Klicka på länken under Installation av PDFG-nätverksskrivare. När du uppmanas att ange användarkontouppgifter anger du användarnamnet och lösenordet som du använde i steg 1 för att logga in. Ett meddelande om att skrivaren har installerats visas.

   ***Obs!**Om användarens lösenord ändras måste användarna installera om PDFG-nätverksskrivaren på sina datorer. Du kan inte uppdatera lösenordet från administrationskonsolen.*

1. Klicka på OK.
