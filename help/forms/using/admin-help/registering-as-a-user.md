---
title: Registrera som användare
description: Lär dig hur du kan använda policyskyddade dokument som du får från en dokumentsäkerhetsanvändare, även om du är extern i förhållande till användarens organisation.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '695'
ht-degree: 0%

---

# Registrera som användare {#registering-as-a-user}

Du kan använda policyskyddade dokument som du får från en dokumentsäkerhetsanvändare, även om du är extern i förhållande till användarens organisation. Om du vill använda ett profilskyddat dokument måste du registrera dig med dokumentskydd. Om du inte tidigare har bjudits in att registrera dig, initierar dokumentsäkerhet registreringsprocessen när dessa händelser inträffar:

* En dokumentsäkerhetsanvändare som vill skicka ett policyskyddat dokument till dig lägger till dig i en profil.
* En dokumentsäkerhetsadministratör skapar ett konto åt dig.

  När du har registrerat och aktiverat ditt konto kan du använda profilskyddade dokument som du har behörighet att använda via en profil. Om dokumentsäkerhetsadministratören aktiverar dessa funktioner för inbjudna användare kan du ha behörighet att göra följande:

* Skydda dokumenten med dokumentskyddsprofiler.
* Skapa egna användarprofiler som du kan tillämpa på dina dokument.
* Bjud in andra externa användare att använda ett policyskyddat dokument genom att lägga till dem i profilen.

  När du registrerar dig behöver du inte registrera dig igen för att använda andra profilskyddade dokument. Ditt konto är aktivt tills en principadministratör inaktiverar eller tar bort det.

>[!NOTE]
>
>Om du får ett policyskyddat dokument men inte får någon e-postinbjudan kontaktar du den person som skickade dokumentet för mer information.

## Registrera dig som en inbjuden användare {#register-as-an-invited-user}

Om du är en inbjuden användare och får ett e-postregistreringsmeddelande från dokumentskyddet, kan du registrera dig genom att använda URL:en i meddelandet för att öppna onlineregistreringssidan. När du har registrerat dig får du ett andra meddelande om hur du aktiverar ditt konto.

1. Öppna e-postmeddelandet om dokumentsäkerhetsregistrering. Den URL som meddelandet innehåller är en länk till registreringssidan för externa användare i dokumentsäkerhet.
1. Klicka på URL:en eller kopiera den och klistra in den i webbläsaren. Sidan External Users Registration (Registrering av externa användare) visas.
1. Skriv ditt namn, telefonnummer, adress, organisation och lösenord i rutorna och skriv sedan in lösenordet igen i rutan Bekräfta lösenord. Lösenordet kan bestå av en kombination av åtta tecken.
1. Klicka på Spara. Ett tack-meddelande visas som informerar dig om att du behöver ett aktiveringsmeddelande via e-post. Aktivera ditt konto för att slutföra registreringsprocessen.

## Aktivera ditt inbjudna användarkonto {#activate-your-invited-user-account}

När du har registrerat dig får du ett aktiveringsmejl. Aktivera ditt konto med hjälp av URL:en i meddelandet. Du kan sedan logga in på dokumentsäkerhet för att använda profilskyddade dokument som du har tillgång till. Beroende på vilka funktioner administratören aktiverar för externa användare kan du ha behörighet att skapa profiler, tillämpa profiler på dokument och lägga till andra externa användare till dina profiler.

Ditt konto är aktivt tills administratören inaktiverar eller tar bort det.

1. Öppna bekräftelsemeddelandet om dokumentsäkerhetsregistrering.
1. Klicka på den URL som visas i meddelandet. Sidan för aktivering av dokumentsäkerhet visas.
1. Klicka på ordet här för att gå till inloggningssidan.
1. I rutan Användarnamn skriver du den e-postadress som du registrerade under med dokumentsäkerhet. Den här e-postadressen är ditt standardanvändarnamn för dokumentsäkerhet.
1. I rutan Lösenord skriver du lösenordet som du skapade när du registrerade dig och klickar sedan på Logga in.

## Återställ lösenordet {#reset-your-password}

Om du glömmer bort lösenordet kan principadministratören återställa det åt dig. När du återställer ett lösenord genereras ett e-postmeddelande som uppmanar dig att logga in med ett tillfälligt lösenord. Sedan kan du skapa ett till lösenord.

Information om hur du kontaktar en dokumentsäkerhetsadministratör för att få ett nytt lösenord finns i aktiveringsmeddelandet eller andra meddelanden från den organisation som har bjudit in dig att registrera dig.

1. Meddela principadministratören att du behöver ett nytt lösenord.
1. När du får e-postmeddelandet om dokumentsäkerhetslösenordet öppnar du det och hämtar det nya tillfälliga lösenordet.
1. Logga in på dokumentsäkerhet med det nya tillfälliga lösenordet.
1. Klicka på Alternativ längst upp till höger på sidan. Sidan Externa användare visas.
1. Välj Ändra lösenord och skriv det tillfälliga lösenordet i rutan Gammalt lösenord.
1. Skriv ett nytt lösenord i rutan Nytt lösenord och skriv det sedan i rutan Bekräfta lösenord igen.
