---
title: Hantera inbjudna och lokala användarkonton
description: Med dokumentsäkerhet kan du söka efter, visa, redigera, låsa, låsa upp och ta bort inbjudna och lokala användarkonton.
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 194ad425-ca1a-4a6e-9f4e-094c4577cde9
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1208'
ht-degree: 0%

---

# Hantera inbjudna och lokala användarkonton {#managing-invited-and-local-user-accounts}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Använd sidan Inbjudna och lokala användare för att hantera inbjudna och lokala användare. Den här sidan visas bara om följande krav uppfylls:

* Du är en administratör som har tilldelats rollen Hantera inbjudna och lokala användare och användarrollen för administrationskonsolen. (Se [Skapa och konfigurera roller](/help/forms/using/admin-help/creating-configuring-roles.md#creating-and-configuring-roles).)
* Inbjuden användarregistrering är aktiverat. (Se [Konfigurera registrering av inbjudna användare](/help/forms/using/admin-help/configuring-client-server-options.md#configuring-invited-user-registration).)

Sidan Inbjudna och lokala användare innehåller två flikar som du kan använda för att söka efter, visa, redigera, låsa, låsa upp och ta bort inbjude och lokala användarkonton.

Du kan även skicka registrerings-e-postmeddelanden manuellt till dina inbjudna användare. Du kanske vill göra detta, till exempel om registreringsperioden som e-postmeddelandet har godkänt avslutas och användaren inte kan använda URL:en för registrering. I så fall kan du skicka om ett registrerings-e-postmeddelande till den inbjudna användaren. När den inbjudna användaren registrerar och aktiverar kontot blir användaren en lokal användare.

>[!NOTE]
>
>Inbjudna användare kan också läggas till direkt via LDAP-katalogen som dokumentsäkerhetsreferenser, eller när en användare eller administratör bjuder in en ny användare när de skapar eller redigerar en profil, vilket innebär att ett e-postmeddelande om registreringsinbjudan initieras. Användare kan lägga till nya inbjudna användare till profiler om du aktiverar alternativet Aktivera registrering av inbjudna användare på sidan Inbjuden användarregistrering.

## Lägg till en inbjuden användare {#add-an-invited-user}

Du kan lägga till ett eller flera inbjudna användarkonton i dokumentskyddet samtidigt. Om du vill lägga till ett inbjudet användarkonto behöver du användarens e-postadress. När du lägger till en användare skickas ett registrerings-e-postmeddelande med en inbjudan till användaren att registrera sig.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på Bjud in ny användare.
1. Skriv e-postadresserna till de användare som du vill bjuda in. Ange flera adresser på en rad, avgränsade med kommatecken.

   Meddelandet som du skapade när du aktiverade registrering för inbjudna användare skickas till användarna. (Se [Konfigurera registrering av inbjudna användare](/help/forms/using/admin-help/configuring-client-server-options.md#configuring-invited-user-registration).)

1. Klicka på OK.

## Visa information om en lokal användare {#view-information-about-a-local-user}

Du kan visa information om lokala användare, inklusive namn, e-postadress, organisation, registreringsstatus och domän.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på Bjud in ny användare.
1. Klicka på fliken Lokala användare och klicka på e-postadressen till den användare som du vill visa på sidan Hantera lokala användare.

   Användarinformationen visas och du kan återställa användarens lösenord och inaktivera kontot.

## Skicka ett e-postmeddelande till en oregistrerad extern användare {#send-an-email-to-an-unregistered-external-user}

När du lägger till en inbjuden användare skickar dokumentsäkerheten automatiskt en registreringsbegäran via e-post. Du kan också generera ett registrerings-e-postmeddelande manuellt för att skicka till en inbjuden användare som ännu inte har registrerat sig. Du kanske vill göra detta, till exempel för att skicka en ny inbjudan om den inbjudna användarens registrerings-e-postadress upphör.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare.
1. I användarlistan markerar du kryssrutan för varje användare som ett registrerings-e-postmeddelande ska skickas till och klickar sedan på Skicka inbjudan igen.
1. Granska listan över valda användare och klicka på OK.

## Återställ ett lokalt användarlösenord {#reset-a-local-user-password}

Du kan återställa lösenord för aktiverade inbjudna användare som har registrerat sig med dokumentsäkerhet men har glömt sitt lösenord. När du återställer ett lösenord skapas ett e-postmeddelande som innehåller ett nytt, tillfälligt lösenord för användaren.

När du aktiverade den inbjudna användarregistreringsprocessen skapade du ett e-postmeddelande som kommer att skickas till användarna som ber dem att återställa sina lösenord. (Se [Konfigurera registrering av inbjudna användare](/help/forms/using/admin-help/configuring-client-server-options.md#configuring-invited-user-registration).)

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på fliken Lokala användare.
1. Välj lämplig användare i användarlistan.
1. På sidan Hantera lokal användare klickar du på Återställ lösenord och sedan på OK. Ett e-postmeddelande om återställning av lösenord med det nya lösenordet skickas till användaren.

## Aktivera eller inaktivera ett användarkonto {#enable-or-disable-a-user-account}

Du kan inaktivera lokala användarkonton om du tillfälligt vill hindra en användare från att logga in på dokumentsäkerhet. När du inaktiverar kontot kan användaren inte använda principskyddade dokument eller skapa eller tillämpa profiler.

Du kan aktivera ett lokalt användarkonto som är inaktiverat. Du kan inte aktivera ett inbjudet användarkonto som är listat som registrerat. Registreringsstatusen anger att den inbjudna användaren är registrerad men ännu inte har aktiverat kontot via länken i aktiveringsmeddelandet.

**Begränsa ett användarkonto**

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på fliken Lokala användare.
1. Välj lämplig användare i användarlistan.
1. Klicka på Inaktivera konto på sidan Lokal användarinformation.

**Återskapa ett användarkonto**

1. Klicka på Inbjudna och lokala användare och klicka på fliken Lokala användare.
1. Välj lämplig användare i användarlistan.
1. Klicka på Aktivera konto på sidan Lokal användarinformation.

## Ta bort ett inbjudet användarkonto {#remove-an-invited-user-account}

Du kan ta bort inbjudna användarkonton från dokumentskyddet. Du kanske vill ta bort ett konto, till exempel när en användare ändrar sin personliga e-postkontoinformation.

Om du tar bort ett användarkonto kan bara du eller en annan administratör återskapa kontot genom att välja alternativet Lägg till inbjuden användare på sidan Inbjudna användare. Användare kan inte lägga till det borttagna användarkontot i en princip och ingen inbjudningsprocess kan initieras av den metoden.

>[!NOTE]
>
>Inbjudna användare som har tagits bort via AEM-formulärens användarhanteringsgränssnitt kan inte bjudas in igen förrän de har tagits bort igen enligt följande procedur.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på fliken Inbjudna användare.
1. Markera kryssrutan bredvid en eller flera användare, klicka på Ta bort och sedan på OK.

## Sök efter ett inbjudet användarkonto {#search-for-an-invited-user-account}

Du kan söka efter inbjudna användarkonton med en e-postadress.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare.
1. Skriv användarens e-postadress i rutan Sök efter e-post och klicka sedan på Sök.

## Sök efter ett lokalt användarkonto {#search-for-a-local-user-account}

Du kan söka efter en lokal användare med användarens e-postadress eller namn och domän.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på fliken Lokala användare.
1. Ange sökvillkoren i rutan Sök, välj Namn eller E-post och klicka sedan på Sök.

## Ta bort ett lokalt användarkonto {#remove-a-local-user-account}

Du kan ta bort lokala användarkonton från dokumentskyddet. Du kanske vill ta bort konton, till exempel när användare ändrar sin personliga e-postkontoinformation.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare och sedan på fliken Lokala användare.
1. Markera kryssrutan bredvid en eller flera användare, klicka på Ta bort och sedan på OK.

## Sortera användarlistan {#sort-the-user-list}

Du kan lättare hitta användare genom att sortera användarlistan efter kolumnrubrik. Triangelikoner bredvid kolumnrubriken anger vilken kolumn som används för att sortera:

* En uppåtriktad triangel visar stigande ordning.
* En triangel som pekar nedåt visar fallande ordning.

   1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Inbjudna och lokala användare.
   1. Om du vill sortera inbjudna användare klickar du på fliken Inbjudna användare och sedan på lämplig kolumnrubrik.
   1. Om du vill sortera lokala användare klickar du på fliken Lokala användare och sedan på lämplig kolumnrubrik.
