---
title: Konfigurera säkerhetsinställningar
description: Lär dig hur du konfigurerar säkerhetsinställningar. Du kan skydda PDF-dokument genom att begränsa åtkomsten. Du kan kryptera, certifiera eller lösenordsskydda dokumentet.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_pdf_generator
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: PDF Generator,Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '1421'
ht-degree: 0%

---

# Konfigurera säkerhetsinställningar{#configuring-security-settings}

Du kan begränsa åtkomsten till PDF-dokument genom att ange lösenord och begränsa vissa funktioner, som utskrift och redigering. När ett PDF-dokument har begränsade funktioner är verktyg och menyalternativ som är relaterade till dessa funktioner nedtonade. Du kan också använda andra metoder för att skapa säkra dokument, till exempel kryptera eller certifiera ett dokument. En säkerhetsinställning innehåller lösenordet och specifika alternativ som kan användas för vissa PDF-konverteringar.

På sidan Skyddsinställningar kan du göra följande:

## Skapa eller redigera en säkerhetsinställning {#create-or-edit-a-security-setting}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

En *säkerhetsinställning* styr säkerhet och behörigheter för filer som konverteras med den säkerhetsinställningen.

1. I administrationskonsolen klickar du på Tjänster > PDF Generator > Skyddsinställningar.
1. Klicka på Ny eller klicka på namnet på en säkerhetsinställning.
1. Fyll i den information som krävs för säkerhetsinställningen på sidan Ny/Redigera säkerhetsinställning. (Se [Konfigurera filtypsinställningar](/help/forms/using/admin-help/configuring-file-type-settings.md#configuring-file-type-settings).)
1. Klicka på Spara och skriv ett namn för inställningen i dialogrutan som visas och klicka sedan på OK.

### Säkerhetsinställningar {#security-settings}

De här inställningarna konfigurerar kompatibilitet och kryptering. Instruktioner om hur du får åtkomst till teckensnittsinställningarna finns i [Skapa eller redigera en säkerhetsinställning](configuring-security-settings.md#create-or-edit-a-security-setting).

**Kompatibilitet:** Anger krypteringstypen för att öppna ett lösenordsskyddat dokument. För alternativet Acrobat 3.0 och senare används en låg krypteringsnivå, men för de andra alternativen används en hög krypteringsnivå:

**Acrobat 3.0 och senare:** Använder låg krypteringsnivå (40-bitars RC4).

**Acrobat 5.0 och senare:** Använder hög kryptering (128-bitars RC4).

**Acrobat 6.0 och senare:** Använder hög kryptering (128-bitars RC4). Med det här alternativet kan du aktivera metadata för sökning.

**Acrobat 7.0 och senare:** Använder hög kryptering (128-bitars AES). Med det här alternativet kan du aktivera metadata för sökning och kryptering av endast bifogade filer.

**Acrobat 9.0 och senare:** Använder hög kryptering (256-bitars AES). Med det här alternativet kan du aktivera metadata för sökning och kryptering av endast bifogade filer.

En tidigare version av Acrobat kan inte öppna ett PDF-dokument med en högre kompatibilitetsinställning. Om du t.ex. väljer alternativet Acrobat 7.0 och senare kan du inte öppna dokumentet i Acrobat 6.0 eller tidigare.

Kontrollera att kompatibilitetsnivån är konsekvent med PDF-kompatibilitetsnivån för samma källa. Om du till exempel har en bevakad mapp som är konfigurerad att använda standardinställningen för PDF, som är kompatibel med Acrobat 5.0 eller senare, får din säkerhetsnivå inte vara högre än Acrobat 5.0.

**Dokumentbegränsning:** Vilka dokumentbegränsningar som är tillgängliga beror på vilket kompatibilitetsalternativ du har valt.

**Ingen kryptering:** Krypterar inte någon del av dokumentet.

**Kryptera allt dokumentinnehåll:** Krypterar dokumentet och dokumentets metadata. När det här alternativet är markerat kan sökmotorer inte komma åt dokumentets metadata.

**Kryptera allt innehåll i dokumentet utom metadata (Acrobat
6 (och senare kompatibelt):** Krypterar innehållet i ett dokument men tillåter ändå sökmotorer att komma åt dokumentets metadata. Det här alternativet är endast tillgängligt när alternativet Kompatibilitet är inställt på Acrobat 6.0 eller senare, Acrobat 7.0 eller senare eller Acrobat 9.0 eller senare.

**Kryptera endast bifogade filer (Acrobat 7 och senare)
Kompatibel):** Användare kan öppna dokumentet utan lösenord men måste ange ett lösenord för att kunna öppna bifogade filer. Det här alternativet är bara tillgängligt när du har valt Acrobat 7.0 eller senare eller Acrobat 9.0 eller senare.

De här inställningarna konfigurerar lösenordsskyddet:

>[!NOTE]
>
>Om du glömmer bort lösenordet kan det inte återställas från dokumentet. Vi rekommenderar att du lagrar lösenord på en annan säker plats ifall du skulle glömma dem. Spara också en säkerhetskopia av dokumentet som inte är lösenordsskyddat.

**Kräv ett lösenord för att öppna dokumentet:** Aktiverar lösenordsalternativen.

**Lösenord för dokumentöppning:** Förhindrar användare från att öppna dokumentet om de inte anger det lösenord som du anger. Lösenord är skiftlägeskänsliga. Acrobat använder RC4-säkerhetsmetoden från RSA Security Inc. för att lösenordsskydda PDF-dokument. Om du begränsar utskrift och redigering rekommenderar vi att du lägger till ett lösenord för dokumentöppning för att öka säkerheten.

**Lösenord för att öppna Retype-dokument:** Ser till att lösenordet för dokumentöppning är korrekt.

**Kräv ett lösenord för att öppna bifogade filer:** Aktiverar lösenordsalternativen. Det här alternativet är endast tillgängligt när alternativet Kompatibilitet är inställt på Acrobat 7.0 eller senare eller på Acrobat 9.0 eller senare och alternativet Dokumentbegränsning är inställt på Kryptera bara bifogade filer.

**Lösenord för att öppna bifogad fil:** Ser till att ett lösenord krävs för att öppna en bifogad fil. Användarna kan öppna dokumentet utan lösenord. Det här alternativet är endast tillgängligt när alternativet Kompatibilitet är inställt på Acrobat 7.0 eller senare eller på Acrobat 9.0 eller senare och alternativet Dokumentbegränsning är inställt på Kryptera bara bifogade filer.

**Bifogad Retype-fil:** Ser till att lösenordet är korrekt. Det här alternativet är endast tillgängligt när alternativet Kompatibilitet är inställt på Acrobat 7.0 eller senare eller på Acrobat 9.0 eller senare och alternativet Dokumentbegränsning är inställt på Kryptera bara bifogade filer.

De här alternativen konfigurerar behörigheter:

**Använd ett lösenord för att begränsa utskrift och redigering av
Dokumentet och dess säkerhetsinställningar:** Aktiverar behörighetsbegränsningar.

**Lösenord för behörighet:** Begränsar användare från att skriva ut och redigera. Användarna kan inte ändra de här säkerhetsinställningarna såvida de inte skriver det lösenord som du anger. Du kan inte använda samma lösenord som används för lösenord för att öppna dokument. När du anger ett behörighetslösenord kan bara de personer som skriver lösenordet ändra säkerhetsinställningarna. Om PDF-dokumentet har båda typerna av lösenord öppnas det med båda lösenorden. En användare kan dock bara ange eller ändra de begränsade funktionerna med behörighetslösenordet. Om PDF-dokumentet bara har behörighetslösenordet eller om en användare öppnar dokumentet med hjälp av lösenordet för dokumentöppning, visas lösenordsmeddelandet när användaren försöker ändra säkerhetsinställningarna.

**Lösenord för Retype-behörighet:** Ser till att behörighetslösenordet är korrekt.

**Tillåtna utskrifter:** Anger utskriftskvaliteten för PDF-dokumentet:

**Inget:** Hindrar användare från att skriva ut dokumentet.

**Låg upplösning (150 dpi):** Användare kan skriva ut dokumentet med en upplösning som inte överstiger 150 dpi. Utskriften kan ta längre tid eftersom varje sida skrivs ut som en bitmappsbild. Det här alternativet är bara tillgängligt om du har valt en hög krypteringsnivå (Acrobat 5.0, 6.0, 7.0 eller 9.0).

**Hög upplösning:** Användare kan skriva ut med valfri upplösning, vilket dirigerar högkvalitativa vektorutdata till PostScript och andra skrivare som stöder avancerade högkvalitativa utskriftsfunktioner.

**Tillåtna ändringar:** Definierar vilka redigeringsåtgärder som tillåts i PDF-dokumentet:

**Inget:** Hindrar användare från att ändra dokumentet, inklusive att fylla i signatur- och formulärfält.

**Infoga, ta bort och rotera sidor:** Användare kan infoga, ta bort och rotera sidor samt skapa bokmärken och miniatyrbilder. Det här alternativet är bara tillgängligt om du har valt en hög krypteringsnivå (Acrobat 5.0, 6.0, 7.0 eller 9.0).

**Fylla i formulärfält och signera befintlig signatur
Fält:** Användare kan fylla i formulär och lägga till digitala signaturer. Användarna kan dock inte lägga till kommentarer eller skapa formulärfält. Det här alternativet är bara tillgängligt om du har valt en hög krypteringsnivå (Acrobat 5.0, 6.0, 7.0 eller 9.0).

**Kommentera, fylla i formulärfält och signera befintliga
Signaturfält:** Användare kan fylla i formulär och lägga till digitala signaturer och kommentarer.

**Sidlayout, slutredigering, fylla i formulärfält och signera
Befintliga signaturfält:** Användare kan infoga, rotera eller ta bort sidor och skapa bokmärken eller miniatyrbilder, fylla i formulär och lägga till digitala signaturer. Det här alternativet tillåter inte användare att skapa formulärfält. Det här alternativet är bara tillgängligt om du har valt en låg krypteringsnivå (Acrobat 3.0).

**Alla utom sidor som extraheras:** Användare kan ändra dokumentet med någon metod i Tillåtelselista Ändringar, förutom att ta bort sidor.

**Aktivera kopiering av text, bilder och annat innehåll:** Användare kan markera och kopiera innehållet i PDF-dokumentet. Man kan också ge program som behöver åtkomst till innehållet i en PDF-fil, t.ex. Acrobat Catalog, tillgång till innehållet. Det här alternativet är bara tillgängligt om du har valt en hög krypteringsnivå.

**Aktivera textåtkomst för skärmläsarenheter från Reader för
Visuellt försämrad:** Gör att användare med synnedsättning kan läsa dokumentet med skärmläsare. Användarna kan dock inte kopiera eller extrahera dokumentinnehållet. Det här alternativet är bara tillgängligt om du har valt en hög krypteringsnivå.

## Ta bort en säkerhetsinställning {#delete-a-security-setting}

Du kan ta bort en säkerhetsinställning om den inte längre behövs. Förkonfigurerade säkerhetsinställningar kan dock inte tas bort.

1. Klicka på **[!UICONTROL Services > PDF Generator > Security Settings]** i administrationskonsolen.
1. Markera kryssrutan bredvid inställningen som ska tas bort. Du kan välja flera inställningar.
1. Klicka på **[!UICONTROL Delete]** och klicka på **[!UICONTROL Delete]** igen på sidan **[!UICONTROL Delete Confirmation]**.
