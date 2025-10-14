---
title: Lägga till domäner
description: Lär dig hur du lägger till en företagsdomän, lokal domän eller hybriddomän med hjälp av inställningarna för Domänhantering och allmänna överväganden för domännamn och ID:n.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 70b0472e-2547-4887-b761-b200dc7b714a
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 0%

---

# Lägga till domäner {#adding-domains}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

## Lägg till en företagsdomän {#add-an-enterprise-domain}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på Ny företagsdomän.
1. Skriv en unik identifierare för domänen i rutan ID och skriv ett beskrivande namn för domänen i rutan Namn. (Se [Viktigt att tänka på när det gäller domännamn och ID](adding-domains.md#important-considerations-for-domain-names-and-ids).)
1. Ange om kontolåsning ska aktiveras. (Se [Konfigurera inställningar för låsning av konto](/help/forms/using/admin-help/configure-account-locking-settings.md#configure-account-locking-settings).) Som standard är Aktivera låsning av konto markerat.
1. Klicka på Lägg till autentisering och välj en leverantör i listan Autentiseringsprovider, beroende på vilken autentiseringsmetod din organisation använder. Möjliga värden är LDAP, Kerberos, SAML eller en anpassad autentiseringsprovider.

   Om du väljer LDAP kan du använda den LDAP-server som anges i katalogkonfigurationen, eller så kan du välja en annan LDAP-server att använda för autentisering. Om du väljer en annan server måste användarna finnas på båda LDAP-servrarna.

1. Ange eventuell ytterligare information som krävs på sidan. (Se [Autentiseringsinställningar](/help/forms/using/admin-help/configuring-authentication-providers.md#authentication-settings).)
1. Lägg till en katalog eller ett anpassat SPI (Service Provider Interface). (Se [Lägga till kataloger eller anpassade SPI:er](/help/forms/using/admin-help/configuring-directories.md#adding-directories-or-custom-spis).)
1. Klicka på Slutför och sedan på OK.

När du har skapat en företagsdomän synkroniserar du katalogen manuellt eller skapar en utlösare som ska utföra en synkronisering innan användarhantering kan använda den. Du kan sedan ställa in ett katalogsynkroniseringsschema och utföra manuell synkronisering efter behov. (Se [Synkronisera kataloger](/help/forms/using/admin-help/synchronizing-directories.md#synchronizing-directories).)

## Lägg till en lokal domän {#add-a-local-domain}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på Ny lokal domän.
1. I rutan ID anger du en unik identifierare för domänen och skriver ett beskrivande namn för domänen i rutan Namn. (Se [Viktigt att tänka på när det gäller domännamn och ID](adding-domains.md#important-considerations-for-domain-names-and-ids).)
1. Ange om kontolåsning ska aktiveras och klicka sedan på OK. (Se [Konfigurera inställningar för låsning av konto](/help/forms/using/admin-help/configure-account-locking-settings.md#configure-account-locking-settings).) Som standard är Aktivera låsning av konto markerat.

## Lägg till en hybriddomän {#add-a-hybrid-domain}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.
1. Klicka på Ny hybrid-domän.
1. I rutan ID anger du en unik identifierare för domänen och skriver ett beskrivande namn för domänen i rutan Namn. (Se [Viktigt att tänka på när det gäller domännamn och ID](adding-domains.md#important-considerations-for-domain-names-and-ids).)
1. Klicka på Lägg till autentisering och välj en leverantör i listan Autentiseringsprovider, beroende på vilken autentiseringsmetod din organisation använder. Möjliga värden är LDAP, Kerberos, SAML eller en anpassad autentiseringsprovider.
1. Ange eventuell ytterligare information som krävs på sidan. (Se [Autentiseringsinställningar](/help/forms/using/admin-help/configuring-authentication-providers.md#authentication-settings).)
1. Klicka på OK och sedan på OK igen.

## Viktigt att tänka på för domännamn och ID {#important-considerations-for-domain-names-and-ids}

Tänk på följande när du väljer ett domännamn och ett ID:

### Allmänna överväganden {#general-considerations}

* När du använder en annan databasleverantör än DB2 kan domän-ID:t innehålla upp till 50 byte. Om du använder ASCII-tecken med en byte är gränsen 50 tecken. Om domänidentifieraren innehåller flerbytetecken reduceras den här gränsen. Om du till exempel skapar en domän vars identifierare innehåller 3-byte-tecken är gränsen 16 tecken. Du kan inte heller skapa domäner som innehåller 4-byte-tecken. Om du skapar ett domän-ID som överskrider den här gränsen kommer AEM-formulär att vara i ett instabilt tillstånd. Information om hur du återställer det här instabila läget finns i [Ta bort en domän som innehåller tecken med utökade eller flera byte &#x200B;](adding-domains.md#remove-a-domain-that-contains-extended-or-multi-byte-characters) på den här sidan.
* Antalet företagsdomäner och lokala domäner som kan skapas i AEM-formulär beror på längden på varje domän-ID. När du lägger till en företagsdomän eller hybriddomän uppdaterar Hantering av användare strängen configInstance i noden AuthProviders i konfigurationsfilen för AEM-formulär (config.xml). Strängen configInstance innehåller en kolonavgränsad lista med absoluta sökvägar för alla domäner som är associerade med auktoriseringsprovidern. Strängen får innehålla högst 8 192 tecken. När den gränsen nås kan du inte skapa fler domäner.

### Att tänka på när du använder DB2 {#considerations-when-using-db2}

När du använder DB2 för din AEM-formulärdatabas beror den maximala tillåtna längden för domän-ID på vilken typ av tecken som används:

* 100 enkelbyte (ASCII) (t.ex. tecken som används på engelska, franska eller tyska språk)
* 50 dubbelbyte (till exempel tecken som används på kinesiska, japanska eller koreanska språk)
* 25 fyrbyte (t.ex. tecken som används i traditionell kinesiska)

### Att tänka på när du använder MySQL {#considerations-when-using-mysql}

När du använder MySQL som AEM-formulärdatabas gäller följande begränsningar:

* Använd endast ASCII-tecken (single-byte) för domän-ID och domännamn. Om du använder utökade ASCII-tecken kommer AEM-formulär att vara i ett instabilt tillstånd och kan generera ett undantag om du försöker ta bort domänen. Information om hur du återställer det här instabila läget finns i avsnittet [Ta bort en domän som innehåller tecken som består av flera byte eller flera byte &#x200B;](adding-domains.md#remove-a-domain-that-contains-extended-or-multi-byte-characters) på den här sidan.
* Du kan inte skapa två domäner som har samma namn men som skiljer sig åt. Om du till exempel försöker skapa en domän med namnet *Adobe* när det redan finns en domän med namnet *adobe* uppstår ett fel.
* Användarhantering kan inte skilja mellan två domännamn som bara skiljer sig åt när utökade tecken används. Om du till exempel skapar en domän med namnet *abcde* och en domän med namnet *âbcdè* betraktas de som samma.

### Ta bort en domän som innehåller utökade tecken eller flerbytetecken {#remove-a-domain-that-contains-extended-or-multi-byte-characters}

1. Exportera konfigurationsfilen enligt beskrivningen i [Importera och exportera konfigurationsfilen](/help/forms/using/admin-help/importing-exporting-configuration-file.md#importing-and-exporting-the-configuration-file).
1. Öppna konfigurationsfilen och under noden Domäner letar du reda på den nod vars namnattribut matchar namnet på domänen som skapats med tecken som är utökade eller består av flera byte. Ta bort hela noden som hör till den domänen.
1. I din databas söker du efter domänen i tabellen edcPrincipalDomainEntity:

   * Välj `*` från edcPrincipalDomainEntity.
   * Leta reda på domännamnet som innehåller tecken som består av flera eller fler byte och ställ in statusen på OBSOLETE.

1. Importera den uppdaterade konfigurationsfilen enligt beskrivningen i [Importera och exportera konfigurationsfilen](/help/forms/using/admin-help/importing-exporting-configuration-file.md#importing-and-exporting-the-configuration-file).
