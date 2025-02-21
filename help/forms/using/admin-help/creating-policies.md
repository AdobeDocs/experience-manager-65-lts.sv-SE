---
title: Skapa och hantera profiler
description: En profil är en uppsättning sekretessinställningar och användare som kan komma åt ett dokument som profilen tillämpas på. Du kan skapa och hantera olika typer av profiler med hjälp av AEM-formulär.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '4725'
ht-degree: 0%

---

# Skapa och hantera profiler {#creating-and-managing-policies}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

En *princip* definierar en uppsättning sekretessinställningar och användare som kan komma åt ett dokument som principen tillämpas på. En *principuppsättning* används för att gruppera en uppsättning principer som har ett gemensamt affärssyfte. Dessa principuppsättningar görs sedan tillgängliga för en delmängd av användarna i systemet. Mer information om profiler finns i [Profiler och policyskyddade dokument](/help/forms/using/admin-help/document-security.md#policies-and-policy-protected-documents).

## Typer av profiler {#types-of-policies}

Dokumentsäkerhet innehåller följande typer av profiler.

**Personliga profiler**

Användare kan skapa, redigera, kopiera, ta bort och tillämpa egna profiler med inställningar som passar en viss situation. Det är bara den person som skapar en princip och administratören som har åtkomst till den personliga principen. Personliga profiler visas på fliken Mina principer på sidan Profiler.

Inbjudna användare kan också skapa, redigera, kopiera och ta bort personliga profiler om administratören aktiverar den funktionen.

**Delade principer**

Administratörer och samordnare för principuppsättningar skapar delade profiler baserat på de sekretessregler som din organisation identifierar för olika typer av dokument och användare. Delade profiler finns i principuppsättningar och är tillgängliga för alla behöriga användare (dokumentutgivare, principuppsättningskoordinatorer och dokumentmottagare) för en viss principuppsättning. Administratörer och koordinatorer för principuppsättningar kan aktivera och inaktivera delade principer. Delade profiler visas i principuppsättningar på fliken Principuppsättningar på sidan Profiler.

När du först installerar dokumentsäkerhet innehåller den en delad princip med namnet *Begränsa till alla principer*. När den här profilen tillämpas på ett dokument kan alla användare som kan logga in på dokumentsäkerhet få åtkomst till dokumentet. Den här principen finns i principuppsättningen *Global principuppsättning*. Den här principen är som standard inte aktiverad. Du kan aktivera det om det passar din organisations behov.

**Automatiskt genererade profiler för Microsoft® Outlook**

Med Acrobat kan du tillämpa profiler på dokument som du skickar som e-postbilagor i Microsoft® Outlook. I Outlook kan du skydda ett dokument med hjälp av en befintlig profil. Du kan också använda en automatiskt genererad profil som Acrobat genererar med standardsekretessinställningar och som gäller för dokumentet som bifogas till ett e-postmeddelande. (Se *[Hjälp om Acrobat](https://help.adobe.com/en_US/acrobat/pro/using/index.html)*.)

>[!NOTE]
>
>För att en profil ska vara tillgänglig i Outlook måste du ange den som en favorit i Acrobat. Alla andra profiler, inklusive de som du är utgivare där, visas inte i Outlook.

## Vem kan skapa och hantera policyer och uppsättningar {#who-can-create-and-manage-policies-and-policy-sets}

Hur du interagerar med policyer och uppsättningar beror på din roll inom organisationen:

**Användare:** Användare kan skapa, redigera och ta bort sina personliga profiler. Inbjudna användare kan också skapa personliga profiler om administratören aktiverar den här funktionen.

**Koordinatorer för principuppsättningar:** Koordinatorer för principuppsättningar kan skapa och hantera delade principer i de principuppsättningar där de har utsetts till koordinator. En principuppsättningskoordinator är vanligtvis en specialist i organisationen som bäst kan skapa policyer i en viss principuppsättning.

**Administratörer:** Administratörer kan redigera alla användares personliga profiler. De kan skapa delade profiler. De kan också skapa, redigera och ta bort principuppsättningar och utse samordnare för principuppsättningar.

Mer information om de olika säkerhetsrollerna för dokument finns i [Om användare av dokumentsäkerhet](/help/forms/using/admin-help/document-security.md#about-document-security-users).

## Skapa och redigera profiler {#creating-and-editing-policies}

Användare kan skapa eller redigera personliga profiler för eget bruk. Administratörer och koordinatorer för principuppsättningar kan skapa eller redigera delade profiler för din organisation.

### Att tänka på vid redigering av profiler {#considerations-for-editing-policies}

När du redigerar en profil påverkar ändringarna dokument som den aktuella profilen skyddar och dokument som skyddsprofilen därefter skyddar. Om du till exempel tar bort mottagare från en profil som tillämpas på ett dokument, kan mottagarna inte längre öppna dokumentet.

Dokumentets status avgör när ändringen träder i kraft:

* Om dokumentet är online tillämpas ändringarna omedelbart, såvida inte användaren har dokumentet öppet. I så fall måste användaren stänga dokumentet för att ändringarna ska börja gälla.
* Om en mottagare använder dokumentet offline (till exempel på en bärbar dator) träder ändringarna i kraft nästa gång mottagaren öppnar dokumentet online. Sedan synkroniseras den med dokumentsäkerheten genom att öppna ett policyskyddat dokument.

>[!NOTE]
>
>Profiler som Acrobat automatiskt genererar för mottagare av dokument som är bifogade till e-postmeddelanden i Microsoft® Outlook visas inte i principlistan. Du kan bara visa de här profilerna genom att öppna dokumentinformationssidan för det associerade dokumentet.

När du redigerar profiler gäller dessa begränsningar:

* Inbjudna användare kan bara redigera profiler om administratören aktiverar den här funktionen. Om du inte kan redigera profiler är alternativet Redigera inte tillgängligt.
* Koordinatorer för principuppsättningar kan bara redigera profiler i principuppsättningar om de har rätt behörigheter. Administratören för superanvändaren eller principuppsättningen anger dessa behörigheter i dokumentets säkerhetsadministratörsgränssnitt.
* Om principen har en vattenstämpel som har konfigurerats som administratören har tagit bort sedan principen skapades, kommer den här vattenstämpeln inte längre att användas för dokument om du redigerar och sparar profilen. Borttagna vattenstämplar gäller bara för befintliga profiler så länge du inte redigerar profilen. Om du redigerar profilen måste du välja en annan vattenstämpel som ska ersätta den borttagna.
* Du kan inte ge anonym åtkomst till ett dokument genom att redigera profilen som används. Om du redigerar profilen måste användarna fortfarande logga in för att få åtkomst till dokumentet. Om du vill använda anonym åtkomst till det här dokumentet tar du först bort principen i klientprogrammet och tillämpar sedan en annan princip som tillåter anonym åtkomst.
* Profiler som Acrobat automatiskt genererar för mottagarna av ett dokument som är kopplat till ett e-postmeddelande i Microsoft Outlook visas inte i principlistan. Om du vill komma åt den här profilen letar du reda på dokumentet på sidan Dokument, öppnar sidan Dokumentinformation och klickar på principnamnet i listan med dokumentinformation.

**Skapa eller redigera en profil**

1. Klicka på Profiler på dokumentsäkerhetssidan och sedan på någon av följande flikar:

   * Om du vill skapa eller redigera en personlig profil klickar du på fliken Min policy.
   * Om du har behörighet att skapa eller redigera en delad profil klickar du på fliken Principuppsättningar och sedan på lämpligt namn på principuppsättningen. Klicka sedan på fliken Profiler.

1. Klicka på Ny eller välj den profil som du vill redigera i listan.
1. Skriv ett namn som unikt identifierar profilen i rutan Namn. Beskriv vad profilen gör och när den ska användas i rutan Beskrivning. Om principen finns i en principuppsättning visas namnet och beskrivningen i principlistan för alla angivna användare. Personliga profiler är bara tillgängliga för användare och administratörer.

   Följande tecken kan inte användas i namnet eller beskrivningen:

   * mindre än-tecken (&lt;)
   * större än-tecken (>)
   * et-tecken (&amp;)
   * enkelt citattecken (&#39;)
   * citattecken (&quot;)
   * omvänt snedstreck (\)
   * snedstreck (/)

   Om du har använt följande tecken i namnet eller beskrivningen konverteras de till blanksteg:

   * vagnretur (ASCII-tecken 13)
   * ny rad (ASCII-tecken 10).

   >[!NOTE]
   >
   >Du kan skapa ett principnamn som innehåller utökade tecken, men när en jämförelse görs mellan två strängar anses både accenterade och icke-accenterade tecken som &quot;e&quot; och &quot;é&quot; vara desamma. När någon skapar en profil görs en jämförelse för att kontrollera om det finns en princip med samma namn. Jämförelsen kan inte skilja mellan namn som är samma förutom för tecken med accent. Vi antar att principen redan har lagts till i databasen och att den nya inte har lagts till.

1. Lägg till användare och grupper i profilen och ange lämpliga behörigheter. (Se [Användare och grupper](creating-policies.md#users-and-groups).)
1. Välj lämpliga alternativ under Allmänna inställningar. (Se [Allmänna inställningar](creating-policies.md#general-settings).)
1. (Valfritt) Välj en extern auktoriseringsleverantör och ange dess egenskaper om tillämpligt. Om du inte vill använda en extern auktoriseringsleverantör klickar du på Ta bort standardprovider.

   En extern auktoriseringsprovider används för att ställa in egenskaper i profilen och när den väljs används den externa auktoriseringsprovidern för att utvärdera principen. De tillgängliga egenskaperna konfigureras av administratören och den person som installerar programvaran.

1. Välj lämpliga alternativ under Avancerade inställningar. (Se [Avancerade inställningar](creating-policies.md#advanced-settings).)
1. Välj lämpliga alternativ under Ej ändringsbara avancerade inställningar. (Se [Avancerade inställningar som inte kan ändras](creating-policies.md#unchangeable-advanced-settings).)
1. Klicka på Spara. Profilen visas i principlistan. En ikon med en röd cirkel visas bredvid den nya profilen, vilket anger att den fortfarande är inaktiverad.

   Aktivera profilen om du vill göra den tillgänglig för användare. (Se [Aktivera eller inaktivera delade profiler](creating-policies.md#enable-or-disable-shared-policies).)

### Användare och grupper {#users-and-groups}

I området Användare och grupper anger du vilka användare som har åtkomst till dokument som är skyddade med profilen. För varje användare eller grupp som du anger anger du även behörigheter för dokumentanvändning.

>[!NOTE]
>
>Dokumentets utgivare är den användare som skyddar dokumentet med profilen. Den här användaren ingår alltid som standard i en princip med fullständig åtkomstbehörighet, inklusive återkallnings- och principväxlingsfunktioner. Administratörer kan dock ändra dokumentutgivarens åtkomsträttigheter för delade profiler. Administratören kan t.ex. hindra dokumentutgivaren från att återkalla dokumentåtkomst eller ändra profilen.

**Lägg till användare eller grupp:** Om du vill lägga till en användare eller grupp med användare klickar du på Lägg till användare eller grupp och sedan på Avancerad sökning så att du kan hitta användare eller grupper. Exempel på användare är interna användare i organisationen och inbjudna användare som har registrerat sig för dokumentsäkerhet. När du väljer det här alternativet visas sidan Lägg till användare eller grupp:

* Skriv användar- eller gruppnamnet eller e-postadressen i rutan Sök.
* Välj Namn eller E-post i listan Använda.
* Välj Användare eller Grupp i listan Typ.
* Välj den domän som du vill söka i listan In och klicka på Sök.
* När resultaten returneras markerar du den användare eller grupp som du vill lägga till och klickar på Lägg till.

>[!NOTE]
>
>Om du anger ett korrekt inbjudet användarnamn eller en e-postadress och inget resultat returneras, kanske användaren inte har registrerat sig än, eller så kan kontot tas bort. Du kan försöka lägga till användaren som en inbjuden användartyp eller kontakta administratören.

**Bjud in ny användare:** Om du vill lägga till en inbjuden användare klickar du på Bjud in ny användare, skriver användarens e-postadress i rutan som visas och klickar på Bjud in. Det här alternativet är bara tillgängligt om administratören har aktiverat det. När du lägger till nya inbjudna användare till en profil skickas ett e-postmeddelande med en registreringsinbjudan om användarna inte redan har bjudits in att registrera sig. Användarna måste använda länken i e-postmeddelandet för att skapa ett konto, och sedan måste de aktivera kontot.

Efter registrering kan inbjudna användare använda principskyddade dokument som de har behörighet för. Beroende på vilka funktioner administratören har aktiverat kan externa användare ha behörighet att tillämpa profiler på dokument, skapa, redigera och ta bort profiler samt lägga till andra externa användare till profiler.

**Lägg till anonym användare:** Klicka på Lägg till anonym användare om du vill tillåta anonym åtkomst. Det här alternativet är bara tillgängligt om administratören har aktiverat anonym användaråtkomst för dokumentsäkerhet. (Se Konfigurera dokumentsäkerhetsservern.) Det här alternativet ger alla åtkomst till dokument som skyddas av den här profilen, oavsett om de har ett dokumentsäkerhetskonto eller inte. Om du väljer det här alternativet kan du inte lägga till andra typer av användare i profilen.

>[!NOTE]
>
>Om du vill tillåta anonym åtkomst till ett policyskyddat dokument som för närvarande inte har det, tar du bort den befintliga policyn och tillämpar sedan en policy som tillåter anonym åtkomst. Om du byter eller ändrar den befintliga profilen måste användarna fortfarande logga in för att få åtkomst till dokumentet.

#### Ange dokumentbehörigheter för användare och grupper {#specify-the-document-permissions-for-users-and-groups}

Du kan ange dokumentbehörigheter för en användare eller grupp åt gången, eller så kan du markera flera användare och grupper i listan och ändra deras behörigheter med alternativen i området för kolumnrubriker.

Som standard har alla principskyddade dokument en behörighet som tillåter användare att öppna dem online.

Fliken Behörigheter och alternativ visas i dokumentskydd.

Dessa dokumentbehörigheter är tillgängliga på fliken Behörigheter. Du kan tillämpa dessa behörigheter på PDF-, PTC Pro/E- och Microsoft Office-filer.

**Skriv ut:** Tillåter användaren att skriva ut ett dokument som är skyddat med den här profilen. För Office- och Pro/E-filer kan du markera kryssrutan Skriv ut om du vill tillåta utskrift eller avmarkera den om du vill förhindra utskrift. Om du markerar kryssrutan Visa anpassade behörigheter för PDF kan du välja bland följande alternativ:

**Inte tillåtet:** Användaren får inte skriva ut PDF.

**Tillåten:** Användaren får skriva ut PDF.

**Låg upplösning. Endast:** Användare får skriva ut PDF med låg upplösning.

**Ändra:** Tillåter användaren att ändra ett dokument som är skyddat med den här principen. För Office- och Pro/E-filer kan du markera kryssrutan Ändra om du vill tillåta ändringar, eller avmarkera den om du vill förhindra ändringar. Om du markerar kryssrutan Visa anpassade behörigheter för PDF kan du välja bland följande alternativ:

**Inte tillåtet:** Användaren får inte ändra PDF.

**Valfri:** Användare kan ändra PDF.

**Samarbeta:** Användaren får samarbeta med andra med hjälp av Samarbeta i Adobe Acrobat. Med den här behörigheten kan användaren kopiera formulärdata även om behörigheten Kopiera inte uttryckligen anges i principen.

**Ändra sidor:** Användaren kan lägga till och ta bort sidor och redigera innehåll i PDF.

**Fyll i och signera:** Användaren får fylla i formulärfält på PDF och signera det.

**Kopiera:** Tillåter användaren att kopiera text från ett dokument som är skyddat med den här principen.

**Skärm-Reader:** Den här behörigheten visas om du markerar kryssrutan Visa anpassade behörigheter för PDF. När det här alternativet är markerat har Adobe Acrobat behörighet att lägga till tillfälliga taggar i PDF för att förbättra läsbarheten med en skärmläsare.

Dessa dokumentbehörigheter är tillgängliga på fliken Alternativ. Du kan tillämpa dessa behörigheter på PDF-, PTC Pro/E- och Microsoft Office-filer:

**Offline:** Tillåter användaren att visa ett dokument offline som är skyddat med den här principen.

**Behörighetsgiltighet:** Välj behörigheter är alltid giltiga eller ange en giltighetsperiod för dokumentbehörighet. Om du väljer en giltighetsperiod klickar du på kalenderikonen för att välja ett datum och använder pilarna för att ange tiden i 24-timmarsformat.

För delade profiler kan administratörer inaktivera följande behörigheter för dokumentutgivaren (den användare som tillämpar profilen på ett dokument):

**Återkalla:** Tillåter dokumentutgivaren att återkalla dokumentbehörigheter.

**Växla:** Tillåter dokumentutgivaren att växla principbehörighet.

### Allmänna inställningar {#general-settings}

Området Allmänna inställningar innehåller följande inställningar:

**Giltighetsperiod:** Den tidsperiod under vilken det profilskyddade dokumentet är tillgängligt för behöriga mottagare. Du kan välja mellan följande alternativ för giltighetsperiod:

**Dokumentet kommer inte att vara giltigt efter:** Dokumentet är tillgängligt under det angivna antalet dagar från när dokumentet skyddades.

**Dokumentet kommer inte att vara giltigt efter detta datum:** Dokumentet är giltigt från det datum då principen tillämpas på dokumentet till det slutdatum som anges.

**Giltig från, till:** Dokumentet är giltigt under de angivna datumen. Du kan använda kalendern för att välja ett datum, där det är tillämpligt, genom att klicka på kalenderikonen.

**Dokumentet är alltid giltigt:** Dokumentets giltighetsperiod upphör inte att gälla.

>[!NOTE]
>
>Giltighetsdatumen baseras på tidszonen i dokumentsäkerhetssystemet, inte på tidszonen på den lokala datorn.

**Granskning:** Aktivera eller inaktivera granskning av händelser som är associerade med ett principskyddat dokument. Dokumentsäkerhet kan t.ex. registrera händelser som försök att öppna ett dokument. Granskade händelser visas i listan på sidan Händelser. Om du inte väljer det här alternativet registreras inte händelser för dokument som är kopplade till profilen.

>[!NOTE]
>
>Administratören måste även aktivera servergranskning på konfigurationssidan för granskning och sekretessinställningar för att granskningsfunktionen ska fungera.

**Spårning av utökad användning:** Aktivera eller inaktivera spårning av utökad användning. Dokumentsäkerhet har stöd för spårning av användarhändelser som är kopplade till olika åtgärder som utförs på en PDF-fil. Dokumentsäkerhetsobjektet kan nås med ett Java-skript. En knappklickning, en multimediefil som spelas upp eller sparandet av en fil är några exempel på händelser som utlöses från en principskyddad PDF. Med dokumentsäkerhetsobjektet kan du även hämta användarinformation. Händelsespårning kan aktiveras från dokumentsäkerhetsservern på global nivå eller på principnivå.

**Leasingperiod automatiskt offline:** Det maximala antal dagar som mottagaren kan använda det principskyddade dokumentet offline (utan en aktiv Internet- eller nätverksanslutning). När låneperioden löper ut måste mottagaren synkronisera dokumentet igen för att kunna fortsätta använda det.

### Externa auktoriseringsleverantörer {#external-authorization-providers}

Välj externa autentiseringsproviders om du redan har konfigurerat några. Tillgängliga leverantörer visas.

### Autentiseringsinställningar {#authentication-settings}

Du kan åsidosätta de autentiseringsinställningar som du har konfigurerat på servern och ange de autentiseringsalternativ som är relevanta för den här principen. Välj Åsidosätt globala autentiseringsinställningar och välj sedan de autentiseringsalternativ som är relevanta för den här principen. Följande autentiseringsalternativ är tillgängliga:

**Tillåt lösenordsautentisering med användarnamn:** Välj det här alternativet om du vill att klientprogram ska kunna använda autentisering med användarnamn/lösenord vid anslutning till servern.

**Tillåt Kerberos-autentisering:** Välj det här alternativet om du vill att klientprogram ska kunna använda Kerberos-autentisering när de ansluter till servern.

**Tillåt klientcertifikatautentisering:** Välj det här alternativet om du vill att klientprogram ska kunna använda certifikatautentisering när de ansluter till servern.

**Tillåt utökad autentisering** Välj om du vill aktivera utökad autentisering. Om du väljer det här alternativet kan klientprogram använda utökad autentisering. Utökad autentisering möjliggör anpassade autentiseringsprocesser och olika autentiseringsalternativ som konfigurerats på dokumentsäkerhetsservern

Om du åsidosätter de globala autentiseringsinställningarna kan du välja de autentiseringsalternativ som är relevanta för den här principen. Om du t.ex. har aktiverat tre autentiseringsalternativ (användarnamn och lösenord, klientcertifikat och utökad autentisering) på servern kan du åsidosätta den globala inställningen och endast välja utökad autentisering för den här principen. Kontrollera att det autentiseringsalternativ du väljer här redan har konfigurerats på servern. I det här exemplet kan du inte välja Kerberos som autentiseringsalternativ eftersom det inte har konfigurerats på servern.

>[!NOTE]
>
>Utökad autentisering stöds i Apple Mac OS X med Adobe Acrobat version 11.0.6 och senare.

### Avancerade inställningar {#advanced-settings}

Området Avancerade inställningar innehåller följande inställningar:

**Dynamisk vattenstämpel:** Välj en vattenstämpel som ska visas dynamiskt på sidorna i ett dokument (till exempel när en mottagare skriver ut dokumentet). Dynamiska vattenstämplar identifierar unikt ett dokument, vilket kan säkerställa dokumentets sekretess och förhindra upphovsrättsintrång. Administratören kan till exempel konfigurera en dynamisk vattenstämpel som visar aktuellt datum, användarnamn eller identifierare för den person som använder dokumentet. Eller namnet på profilen som används för att skydda dokumentet. En vattenstämpel kan även visa egna text- eller grafikelement, om de är konfigurerade. Administratörer konfigurerar alternativen för vattenstämpel och administratörer och användare kan tillämpa dem på profiler.

(Se [Konfigurera dynamiska vattenstämplar](/help/forms/using/admin-help/configuring-client-server-options.md#configure-dynamic-watermarks).)

Om du redigerar en profil och administratören har tagit bort en konfigurerad vattenstämpel som du tidigare har valt för den här principen, visas en anteckning på sidan Redigera princip. Om du i det här fallet sparar det redigerade dokumentet, väljer du en ny vattenstämpel om du vill att den ska visas i dokumentet.

>[!NOTE]
>
>För profiler som ger anonym användaråtkomst visas inte användarnamnet och identifieraren för en anonym användare som en vattenstämpel, även om du väljer den här typen av vattenstämpel.

**Använd endast certifierade Acrobat-plugin-program för PDF:** Om du väljer det här alternativet för en profil måste Acrobat 8.0 och senare köras i certifierat läge när dokument som är skyddade med profilen öppnas. När Acrobat körs i certifierat läge läses inga plugin-program från tredje part in.

Välj det här alternativet om du är orolig för att en dokumentmottagare ska skriva ett plugin-program som kan kringgå något av dokumentskyddet i Acrobat 8.0 eller senare. Välj inte det här alternativet om dokumentmottagarna måste använda plugin-program från tredje part i Acrobat för att interagera med dokument.

Det här alternativet aktiverar endast det certifierade läget i Acrobat 8.0 eller senare. Administratören måste inaktivera åtkomsten till Acrobat 7.0.

(Se [Konfigurera dokumentsäkerhetsservern](/help/forms/using/admin-help/configuring-client-server-options.md#configure-the-document-security-server).)

Det här alternativet gäller inte Adobe Reader.

**Felmeddelande om nekad åtkomst:** Ett meddelande som visas för alla som försöker öppna ett principskyddat dokument utan behörighet. Meddelandet visas i Acrobat. Klienter som inte kan visa det här meddelandet visar ett standardmeddelande som anger att åtkomst nekas.

### Avancerade inställningar som inte kan ändras {#unchangeable-advanced-settings}

Området Avancerade inställningar som inte kan ändras innehåller följande inställningar. Du kan inte ändra de här inställningarna när du har sparat profilen.

**Krypteringsalgoritm och nyckellängd:** Används för att skydda dina dokument. Du kan välja mellan följande alternativ:

* 128-bitars AES
* 256-bitars AES. Endast Acrobat 9.0 och senare stöder det här alternativet. Om du vill använda AES 256-kryptering för PDF-filer hämtar och installerar du Java Cryptography Extension (JCE) Unlimited Strength Jurisdential Policy-filer. Dessa filer ersätter filerna local_policy.jar och US_export_policy.jar i mappen [JAVE_HOME]/lib/security. Om du till exempel använder Sun JDK 1.6 kopierar du de hämtade filerna till mappen [dep root]/Java/jdk1.6.0_26/lib/security. Du kan hämta dessa filer från [Java SE Downloads](https://java.sun.com/javase/downloads/index.jsp).
* Ingen kryptering. Acrobat 9.0 och senare stöder det här alternativet. Om du väljer det här alternativet inaktiveras alternativen Dokumentbegränsningar. Det här alternativet kan vara användbart om du vill använda dokumentskydd för dokumentgranskning eller versionskontroll, men inte vill kryptera dokumentet.

**Dokumentbegränsningar:** Markera de PDF-dokumentkomponenter som ska krypteras. Andra klientprogram krypterar hela dokumentet men inte länkade eller inbäddade filer. Du kan välja mellan följande alternativ:

* Hela dokumentet, inklusive dess bilagor och metadata. *Metadata* är information om dokumentet och dess innehåll som du kan visa via dialogrutan Dokumentegenskaper eller på menyn Avancerat i Acrobat. I Acrobat kan du bifoga filer av olika typer (till exempel text-, ljud- och videofiler) till PDF-dokument.
* Dokumentet och dess bilagor, men inte metadata.
* Endast dokumentbilagor. Du kan kryptera de bifogade filerna till en PDF-fil utan att behöva kryptera dokumentinnehållet.

## Aktivera eller inaktivera delade principer {#enable-or-disable-shared-policies}

Om du vill göra en delad princip tillgänglig måste administratören eller koordinatorn för principuppsättningen aktivera den. Du kan aktivera nya profiler eller tidigare inaktiverade profiler. En delad princip som du inaktiverar tillämpas fortfarande för dokument som är skyddade med den principen.

Ett rött X visas bredvid en inaktiverad princip.

>[!NOTE]
>
>Administratörer kan inte inaktivera personliga profiler och användare kan inte aktivera och inaktivera egna profiler.

1. Klicka på Profiler på dokumentsäkerhetssidan och sedan på fliken Principuppsättningar.
1. Klicka på lämpligt namn på principuppsättningen och klicka på fliken Profiler.
1. Markera kryssrutan bredvid rätt profil, klicka på Aktivera eller Inaktivera och klicka sedan på OK.

## Visa information om en profil {#view-information-about-a-policy}

På fliken Mina profiler kan du söka efter personliga profiler.

De principuppsättningar som administratörer skapar visas på fliken Principuppsättningar på sidan Profiler. De innehåller information om principuppsättningen, inklusive namn, datum då den skapades och ändrades samt en beskrivning. Klicka på ett namn på en principuppsättning så att du kan se informationen om den. Koordinatorer för principuppsättningar som har behörighet att hantera principer kan skapa delade principer i en viss principuppsättning.

När du skapar eller redigerar en profil visas en sida där du kan konfigurera principnamn, behörighetsnivåer, sekretessinställningar och vilka mottagare som ska ingå i profilen.

Administratören kan konfigurera följande sekretessinställningar för en princip:

* Allmänna alternativ för dokumentsekretess, t.ex. dokumentets giltighetsperiod och offlineleasingperiod
* De behöriga användarna samt dokumentbegränsningar och behörigheter för var och en av dem
* Avancerade alternativ för dokumentsekretess, inklusive dynamiska vattenstämplar och dokumentkryptering

Användarna kan visa de profiler de har skapat och eventuella delade profiler som de har tillgång till. Administratörer kan visa alla delade och personliga policyer som ingår i dokumentsäkerheten.

Du kan visa mer detaljerad information om en profil som visas i listan, inklusive de användare eller grupper som ingår i profilen och de sekretessinställningar som har angetts för dessa användare.

>[!NOTE]
>
>Profiler som Acrobat automatiskt genererar för mottagare av dokument som är bifogade till e-postmeddelanden i Microsoft Outlook visas inte i principlistan. Du kan bara visa de här profilerna genom att öppna dokumentinformationssidan för det associerade dokumentet.

1. Klicka på Profiler på dokumentsäkerhetssidan och sedan på fliken Mina profiler.
1. Fyll i sökinformationen så att du kan söka efter personliga profiler.
1. Välj lämplig profil i listan.
1. På sidan Policyinformation kan du visa information om policyn, redigera policyn eller visa händelser relaterade till den.

## Sök efter profiler {#search-for-policies}

Administratörer kan söka efter delade profiler och personliga profiler som har skapats av andra användare.

1. Om du vill söka efter en delad profil klickar du på Profiler och sedan på fliken Principuppsättningar. Klicka på en principuppsättning i listan och sedan på fliken Profiler.

   Om du vill söka efter en personlig profil klickar du på Profiler på dokumentsäkerhetssidan och sedan på fliken Mina profiler.

1. Välj något av följande alternativ i söklistan:

   **Princip-ID:** Det principidentifieringsnummer som skapas när användaren skapar principen. Ange det exakta princip-ID:t.

   **Principnamn:** Principens namn. Du kan söka efter delar av principnamnet eller hela det.

1. Skriv motsvarande värde i textrutan. Om du till exempel har valt Principnamn skriver du det principnamn du söker efter.
1. I visningslistan väljer du antalet resultat som ska visas och klickar sedan på Sök. Sökresultaten visas.
1. (Valfritt) Om du vill visa profilinformation klickar du på profilen.

## Kopiera en profil {#copy-a-policy}

Du kan kopiera en befintlig profil och spara den med ett nytt namn och en ny beskrivning. Kopiering av profiler är ett effektivt sätt att skapa profiler med hjälp av befintliga inställningar.

Externa användare kan bara kopiera profiler om administratören aktiverar den här funktionen. Om du inte kan skapa profiler är alternativet Kopiera inte tillgängligt.

1. Klicka på Profiler på dokumentsäkerhetssidan och sedan på fliken Min profil.
1. Välj lämplig profil i listan.
1. Klicka på Kopiera på sidan Principinformation.
1. Skriv det nya principnamnet i rutan Nytt principnamn. Du kan även skriva en ny beskrivning.

   Följande tecken kan inte användas i namnet eller beskrivningen:

   * mindre än-tecken (&lt;)
   * större än-tecken (>)
   * et-tecken (&amp;)
   * enkelt citattecken (&#39;)
   * citattecken (&quot;)
   * omvänt snedstreck (\)
   * snedstreck (/)

   Om du har använt följande tecken i namnet eller beskrivningen konverteras de till blanksteg:

   * vagnretur (ASCII-tecken 13)
   * ny rad (ASCII-tecken 10).

   >[!NOTE]
   >
   >Du kan skapa ett principnamn som innehåller utökade tecken, men när en jämförelse görs mellan två strängar anses både accenterade och icke-accenterade tecken som &quot;e&quot; och &quot;é&quot; vara desamma. När någon skapar en profil görs en jämförelse för att kontrollera om det finns en princip med samma namn. Jämförelsen kan inte skilja mellan namn som är samma förutom för tecken med accent. Vi antar att principen redan har lagts till i databasen och att den nya inte har lagts till.

1. Klicka på OK.

## Ta bort en profil {#delete-a-policy}

Du kan ta bort profiler som du har skapat. Administratörer kan ta bort profiler som alla användare har skapat. Koordinatorer för principuppsättningar kan ta bort profiler i sina principuppsättningar. En princip som du tar bort gäller fortfarande för dokument som är skyddade med den profilen. Du kan ta bort flera profiler åt gången.

Inbjudna användare kan bara ta bort profiler om administratören aktiverar den här funktionen. Om du inte kan ta bort profiler är borttagningsalternativet inte tillgängligt.

1. Klicka på Profiler på dokumentsäkerhetssidan.
1. Klicka på fliken Min policy.
1. Om du vill ta bort en delad profil klickar du på fliken Principuppsättningar och sedan på lämpligt namn på principuppsättningen.
1. Markera kryssrutan bredvid rätt profil och klicka på Ta bort. Klicka sedan på OK.

>[!NOTE]
>
>Använd klientprogrammet för att ta bort profiler från dokument. (Se hjälpen för Acrobat eller motsvarande Acrobat Reader DC-tillägg.)

## Sortera principlistan {#sort-the-policy-list}

Du kan sortera policylistan efter kolumnrubriker för att enklare hitta profiler. En triangelikon bredvid kolumnrubriken anger vilken kolumn som används för att sortera. En uppåtriktad triangel visar stigande ordning, medan en nedåtriktad triangel anger fallande ordning.

1. Klicka på Profiler på dokumentsäkerhetssidan och sedan på fliken Principuppsättning.
1. Välj en principuppsättning och klicka sedan på fliken Profiler.
1. Klicka på lämplig kolumnrubrik.
1. Om du vill ändra sorteringsordningen klickar du på kolumnen igen.
