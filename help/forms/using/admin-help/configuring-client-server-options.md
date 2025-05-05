---
title: Konfigurera klient- och serveralternativ
description: Lär dig hur du kan konfigurera olika klient- och serveralternativ, som serverkonfigurationsinställningar, säkerhetsroller för dokument och händelsegranskning.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 72c31f40-d1b0-47ae-bdeb-e9b92c3d27e1
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '10278'
ht-degree: 0%

---

# Konfigurera dokumentsäkerhetsservern {#configure-the-document-security-server}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Tjänster > dokumentsäkerhet > Konfiguration > Serverkonfiguration.
1. Konfigurera inställningarna och klicka på OK.

## Inställningar för serverkonfiguration {#server-configuration-settings}

**Bas-URL:** Bas-dokumentets säkerhets-URL, som innehåller servernamnet och porten. Den information som läggs till i basen skapar anslutnings-URL:er. Till exempel läggs /edc/Main.do till för att få åtkomst till webbsidorna. Användarna besvarar även inbjudningar till extern användarregistrering via denna URL.

Om du använder IPv6 anger du bas-URL:en som datornamn eller DNS-namn. Om du använder en numerisk IP-adress kan Acrobat inte öppna principskyddade filer. Använd även HTTP-säker URL (HTTPS) för servern.

>[!NOTE]
>
>Bas-URL:en är inbäddad i principskyddade filer. Klientprogram använder bas-URL:en för att ansluta tillbaka till servern. Skyddade filer innehåller även fortsättningsvis bas-URL:en, även om den ändras senare. Om du ändrar bas-URL:en måste konfigurationsinformationen uppdateras för alla anslutande klienter.

**Standardperiod för offlinelån:** Standardtiden som en användare kan använda ett skyddat dokument offline. Den här inställningen avgör det inledande värdet för inställningen för den automatiska offlinelåneperioden när du skapar en profil. (Se Skapa och redigera profiler.) När låneperioden löper ut måste mottagaren synkronisera dokumentet igen för att kunna fortsätta använda det.

Mer information om hur offlinelån och synkronisering fungerar finns i [Primer om konfigurering av offlinelån och synkronisering](https://blogs.adobe.com/security/2009/05/primer_on_configuring_offline.html).

**Standardperiod för offlinesynkronisering:** Den längsta tid som ett dokument kan användas offline från när det ursprungligen är skyddat.

**Tidsgräns för klientsession:** Den tid, i minuter, efter vilken dokumentsäkerhet kopplas från om en användare som är inloggad via ett klientprogram inte interagerar med dokumentsäkerhet.

**Tillåt anonyma användare åtkomst:** Välj det här alternativet om du vill aktivera möjligheten att skapa delade och personliga profiler som tillåter anonyma användare att öppna principskyddade dokument. (Användare som inte har konton kan komma åt dokumentet, men de kan inte logga in på dokumentsäkerhet eller använda andra profilskyddade dokument.)

**Inaktivera åtkomst till Version 7-klienter:** Anger om användare kan använda Acrobat eller Reader 7.0 för att ansluta till servern. När det här alternativet är markerat måste användare använda Acrobat eller Reader 8.0 eller senare för att slutföra dokumentsäkerhetsåtgärder för PDF-dokument. Om en profil kräver att Acrobat eller Reader 8.0 eller senare måste köras i certifierat läge när principskyddade dokument öppnas bör du inaktivera åtkomsten till Acrobat eller Reader 7. (Se Ange dokumentbehörigheter för användare och grupper.)

**Tillåt offlineåtkomst per dokument** Välj det här alternativet om du vill ange offlineåtkomst per dokument. Om den här inställningen är aktiverad har användaren bara offline-åtkomst till de dokument som användaren har öppnat online minst en gång.

**Tillåt lösenordsautentisering med användarnamn:** Välj det här alternativet om du vill att klientprogram ska kunna använda autentisering med användarnamn/lösenord vid anslutning till servern.

**Tillåt Kerberos-autentisering:** Välj det här alternativet om du vill att klientprogram ska kunna använda Kerberos-autentisering när de ansluter till servern.

**Tillåt klientcertifikatautentisering:** Välj det här alternativet om du vill att klientprogram ska kunna använda certifikatautentisering när de ansluter till servern.

**Tillåt utökad autentisering** Välj om du vill aktivera utökad autentisering och ange sedan den utökade URL:en för autentiseringslandning.

Om du väljer det här alternativet kan klientprogram använda utökad autentisering. Utökad autentisering ger anpassade autentiseringsprocesser och olika autentiseringsalternativ som konfigurerats på AEM Forms Server. Användare kan nu till exempel använda SAML-baserad autentisering i stället för AEM-formulär med användarnamn/lösenord från Acrobat och Reader Client. Som standard innehåller landnings-URL:en *localhost* som servernamn. Ersätt servernamnet med ett fullständigt kvalificerat värdnamn. Värdnamnet i landnings-URL fylls automatiskt i från bas-URL:en om utökad autentisering inte har aktiverats ännu. Se [Lägg till den utökade autentiseringsprovidern](configuring-client-server-options.md#add-the-extended-authentication-provider).

***Obs!**: Utökad autentisering stöds i Apple Mac OS X med Adobe Acrobat version 11.0.6 och senare.*

**Önskad HTML-kontrollbredd för utökad autentisering** Ange bredden på den utökade autentiseringsdialogrutan som öppnas i Acrobat för att ange användarautentiseringsuppgifter.

**Önskad kontrollhöjd för utökad autentisering för HTML** Ange höjden på den utökade autentiseringsdialogrutan som öppnas i Acrobat för att ange användarautentiseringsuppgifter.

***Obs!**: Gränserna för bredd och höjd för den här dialogrutan är följande:*
Bredd: Minsta = 400, högsta = 900

Höjd: Minst = 450; Högst = 800

**Aktivera cachelagring av klientautentiseringsuppgifter:** Välj det här alternativet om du vill tillåta användare att cachelagra sina autentiseringsuppgifter (användarnamn och lösenord). När användarens inloggningsuppgifter är cachelagrade behöver de inte ange sina inloggningsuppgifter varje gång de öppnar ett dokument eller när de klickar på Uppdatera på sidan Hantera skyddsprofiler i Adobe Acrobat. Du kan ange antalet dagar innan användarna måste ange sina inloggningsuppgifter igen. Om du anger värdet 0 för antal dagar kan autentiseringsuppgifter cachelagras på obestämd tid.

## Konfigurera användare och administratörer för dokumentsäkerhet {#configuring-document-security-users-and-administrators}

### Tilldela dokumentsäkerhetsroller till administratörer {#assigning-document-security-roles-to-administrators}

Din AEM-formulärmiljö innehåller en eller flera administratörsanvändare som har behörighet att skapa användare och grupper. Om din organisation använder dokumentskydd måste minst en administratör också tilldelas behörighet att hantera inbjudna och lokala användare.

Administratörerna måste också ha administratörskonsolens användarroll för att komma åt administrationskonsolen. (Se [Skapa och konfigurera roller](/help/forms/using/admin-help/creating-configuring-roles.md#creating-and-configuring-roles).)

### Konfigurera synliga användare och grupper {#configuring-visible-users-and-groups}

Om du vill visa användare och grupper i valda domäner under principanvändarsökningar måste en superadministratör eller administratör för principuppsättningen markera och lägga till domäner (skapade i användarhantering) i listan över synliga användare och grupper för varje principuppsättning.

Listan med synliga användare och grupper är synlig för principuppsättningens koordinator och används för att begränsa vilka domäner slutanvändaren kan bläddra i när han eller hon väljer användare eller grupper att lägga till i profiler. Om den här åtgärden inte utförs kommer principuppsättningskoordinatorn inte att hitta några användare eller grupper att lägga till i principen. Det kan finnas fler än en principuppsättningskoordinator för en given principuppsättning.

1. När du har installerat och konfigurerat din AEM-formulärmiljö med dokumentsäkerhet konfigurerar du alla lämpliga domäner i Användarhantering. <!-- Fix broken link (See Setting up and managing domains) -->

   ***Obs!**&#x200B;Du måste skapa domäner innan du kan skapa profiler.*

1. I administrationskonsolen klickar du på Tjänster > Dokumenthantering > Profiler och sedan på fliken Principuppsättningar.
1. Välj Global principuppsättning och klicka sedan på fliken Synliga användare och grupper.
1. Klicka på Lägg till domän(er) och lägg till befintliga domäner efter behov.
1. Gå till Tjänster > Dokumentsäkerhet > Konfiguration > Mina principer och klicka på fliken Synliga användare och grupper.
1. Klicka på Lägg till domän(er) och lägg till befintliga domäner efter behov.

## Lägg till utökad autentiseringsprovider {#add-the-extended-authentication-provider}

AEM-formulär har en exempelkonfiguration som du kan anpassa för din miljö. Utför följande steg:

>[!NOTE]
>
>Utökad autentisering stöds i Apple Mac OS X med Adobe Acrobat version 11.0.6 och senare.

1. Hämta WAR-exempelfilen som distribuerar den. Se installationshandboken för din programserver.
1. Kontrollera att Forms Server har ett fullständigt kvalificerat namn i stället för IP-adresser som bas-URL och att det är en HTTPS-URL. Se [Serverkonfigurationsinställningar](configuring-client-server-options.md#server-configuration-settings).
1. Aktivera utökad autentisering på sidan Serverkonfiguration. Se [Serverkonfigurationsinställningar](configuring-client-server-options.md#server-configuration-settings).
1. Lägg till nödvändiga URL:er för omdirigering av enkel inloggning i konfigurationsfilen för användarhantering. Se [Lägg till omdirigerings-URL:er för enkel inloggning för utökad autentisering](configuring-client-server-options.md#add-sso-redirect-urls-for-extended-authentication).

### Lägg till omdirigerings-URL:er för enkel inloggning för utökad autentisering {#add-sso-redirect-urls-for-extended-authentication}

Med utökad autentisering aktiverat får användare som öppnar ett policyskyddat dokument i Acrobat XI eller Reader XI en dialogruta för autentisering. Den här dialogrutan läser in den HTML-sida som du angav som landnings-URL för utökad autentisering i inställningarna för dokumentsäkerhetsservern. Se [Serverkonfigurationsinställningar](configuring-client-server-options.md#server-configuration-settings).

>[!NOTE]
>
>Utökad autentisering stöds i Apple Mac OS X med Adobe Acrobat version 11.0.6 och senare.

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Importera och exportera konfigurationsfiler.
1. Klicka på Exportera och spara konfigurationsfilen på disken.
1. Öppna filen i en redigerare och leta upp noden AllowedUrls.
1. Lägg till följande rader i noden `AllowedUrls`: `<entry key="sso-l" value="/ssoexample/login.jsp"/> <entry key="sso-s" value="/ssoexample"/> <entry key="sso-o" value="/ssoexample/logout.jsp"/>`

   ```xml
   <entry key="sso-l" value="/ssoexample/login.jsp"/>
   <entry key="sso-s" value="/ssoexample"/>
   <entry key="sso-o" value="/ssoexample/logout.jsp"/>
   ```

1. Spara filen och importera sedan den uppdaterade filen från sidan Manuell konfiguration: I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Importera och exportera konfigurationsfiler.

## Konfigurera offlinesäkerhet {#configuring-offline-security}

dokumentsäkerhet gör det möjligt att använda principskyddade dokument offline utan Internet- eller nätverksanslutning. Den här funktionen kräver att principen tillåter offlineåtkomst, vilket beskrivs i [Ange dokumentbehörigheter för användare och grupper](/help/forms/using/admin-help/creating-policies.md#specify-the-document-permissions-for-users-and-groups). Innan ett dokument med en sådan profil kan användas offline måste mottagaren öppna dokumentet online och aktivera åtkomst offline genom att klicka på Ja när du uppmanas till det. Mottagaren kan också bli ombedd att autentisera sin identitet. Mottagaren kan sedan använda dokument offline under den offlinelåneperiod som anges i policyn.

När låneperioden är slut måste mottagaren synkronisera igen med dokumentsäkerheten antingen genom att öppna ett dokument online eller genom att använda ett menykommando på Acrobat- eller Acrobat Reader DC-tilläggen för att synkronisera. (Se *Acrobat-hjälpen* eller *Acrobat Reader DC-tilläggen*.)

Eftersom dokument som tillåter offlineåtkomst kräver cachelagring av viktigt material på den dator där filerna lagras offline, kan filen eventuellt komprometteras om en obehörig användare kan få tillgång till nyckelmaterialet. För att kompensera för den här möjligheten finns schemalagda och manuella nyckelrollover-alternativ som du kan konfigurera för att förhindra att obehöriga använder nyckeln för att få åtkomst till dokumentet.

### Ange standardperiod för offlineleasing {#set-a-default-offline-lease-period}

Mottagare av principskyddade dokument kan ta dokumenten offline under det antal dagar som anges i profilen. När dokumentet har synkroniserats med dokumentsäkerheten kan mottagaren använda det offline tills offlineleasingperioden har löpt ut. När låneperioden är slut måste mottagaren ta dokumentet online och logga in för att synkronisera med dokumentsäkerheten för att kunna fortsätta använda dokumentet.

Du kan konfigurera en standardlåneperiod offline. Låneperioden kan ändras från standardperioden när någon skapar eller redigerar en profil.

1. På dokumentsäkerhetssidan klickar du på Konfiguration > Serverkonfiguration.
1. I rutan Standardperiod för offlineleasing anger du antalet dagar för offlineleasingperioden.
1. Klicka på OK.

### Hantera viktiga överrullningar {#manage-key-rollovers}

Dokumentsäkerhet använder krypteringsalgoritmer och licenser för att skydda dokument. När dokumentskyddet krypterar ett dokument genereras och hanteras en dekrypteringsnyckel som kallas *DocKey* och skickas till klientprogrammet. Om profilen som skyddar ett dokument tillåter åtkomst offline, genereras även en offlinenyckel som kallas *huvudnyckel* för varje användare som har offlineåtkomst till dokumentet.

>[!NOTE]
>
>Om det inte finns någon huvudnyckel genereras en för att skydda ett dokument.

Om du vill öppna ett principskyddat dokument offline måste användarens dator ha rätt huvudnyckel. Datorn hämtar huvudnyckeln när användaren synkroniserar med dokumentsäkerhet (öppnar ett skyddat dokument online). Om den här huvudnyckeln är komprometterad kan alla dokument som användaren har offlineåtkomst till också bli komprometterade.

Ett sätt att minska hotet mot offlinedokument är att undvika att ge offlineåtkomst till särskilt känsliga dokument. En annan metod är att regelbundet föra över huvudnycklarna. När dokumentskyddet rullar tangenten över kan inte befintliga nycklar komma åt de profilskyddade dokumenten längre. Om en gärningsman t.ex. får en huvudnyckel från en stulen bärbar dator, kan den nyckeln inte användas för att få tillgång till dokument som är skyddade efter överrullningen. Om du misstänker att en viss huvudnyckel har komprometterats kan du manuellt föra över nyckeln.

Men en tangentöverrullning påverkar alla huvudnycklar, inte bara en. Det minskar också systemets skalbarhet eftersom klienterna måste lagra fler nycklar för offlineåtkomst. Standardfrekvensen för överföring av nycklar är 20 dagar. Vi rekommenderar att du inte anger det här värdet under 14 dagar eftersom användare kan hindras från att visa offlinedokument och systemprestanda kan påverkas.

I följande exempel är Key1 den äldre av de två huvudnycklarna och Key2 den nyare. När du klickar på knappen Rollover Keys Now första gången blir Key1 ogiltig och en senare, giltig huvudnyckel (Key3) genereras. Användare får tillgång till tangent 3 när de synkroniserar med dokumentsäkerhet, vanligtvis genom att öppna ett skyddat dokument online. Användarna behöver dock inte synkronisera med dokumentsäkerheten förrän de når den maximala offlinelåneperioden som anges i en profil. Efter den första nyckelåterställningen kan användare som är offline fortfarande öppna offlinedokument, även de som skyddas av Key3, tills de når den maximala offlineleasingperioden. När du klickar på knappen Rollover Keys Now en andra gång blir Key2 ogiltigt och Key4 skapas. Användare som är offline under de två överrullningarna kan inte öppna dokument som är skyddade med Key3 eller Key4 förrän de synkroniserar med dokumentsäkerheten.

**Ändra överrullningsfrekvens för nyckel**

Av sekretesskäl ger dokumentskyddet ett automatiskt nyckelrolloveralternativ med en standardfrekvenstid på 20 dagar när du använder offlinedokument. Du kan ändra överrullningsfrekvensen, men undvik att ange ett lägre värde än 14 dagar eftersom användare kan hindras från att visa offlinedokument och systemprestanda kan påverkas.

1. På dokumentsäkerhetssidan klickar du på Konfiguration > Nyckelhantering.
1. I rutan Tangentrollover-frekvens anger du antalet dagar för rollover-perioden.
1. Klicka på OK.

**Överför huvudnycklar manuellt**

För att sekretessen för offlinedokument ska kunna bevaras kan du manuellt föra över huvudnycklar. Du kan behöva rulla över en nyckel manuellt (t.ex. om nyckeln komprometteras av någon som hämtar den från en dator där den cache-lagras för att kunna aktivera offlineåtkomst till ett dokument).

>[!NOTE]
>
>Undvik ofta manuell överrullning eftersom det gör att alla huvudnycklar förs över, inte bara en, och det kan tillfälligt hindra användare från att visa nya dokument offline.

Huvudnycklarna måste rullas över två gånger innan befintliga nycklar på klientdatorer blir ogiltiga. Klientdatorer som har ogiltiga huvudnycklar måste synkroniseras på nytt med dokumentsäkerhetstjänsten för att kunna hämta de nya huvudnycklarna.

1. På dokumentsäkerhetssidan klickar du på Konfiguration > Nyckelhantering.
1. Klicka på Överrullningsnycklar nu och sedan på OK.
1. Vänta ca 10 minuter. Följande loggmeddelande visas i serverloggen: `Done RightsManagement key rollover for`*N* `principals`. Där *N* är antalet användare i dokumentsäkerhetssystemet.
1. Klicka på Överrullningsnycklar nu och sedan på OK.
1. Vänta ca 10 minuter.

## Konfigurera händelsegranskning och sekretessinställningar {#configuring-event-auditing-and-privacy-settings}

Dokumentsäkerhet kan granska och registrera information om händelser som rör interaktion med policyskyddade dokument, profiler, administratörer och servern. Du kan konfigurera händelsegranskning och du kan ange vilka typer av händelser som ska granskas. Om du vill granska händelser för ett visst dokument måste granskningsalternativet för profilen också aktiveras.

När granskning är aktiverat kan du visa information om de granskade händelserna på sidan Händelser. dokumentsäkerhetsanvändare kan även visa händelser som är specifikt relaterade till de profilskyddade dokument som de använder eller skapar.

Du kan välja den här typen av händelser för granskning:

* Policyskyddade dokumenthändelser, t.ex. försök av behöriga eller obehöriga användare att öppna dokument
* Politiska händelser, som att skapa, ändra, ta bort, aktivera och inaktivera principer
* Användarhändelser, som externa användarinbjudningar och registreringar, aktiverade och inaktiverade användarkonton, ändringar av användarlösenord och profiluppdateringar
* AEM blanketthändelser, t.ex. felmatchningar av versioner, otillgänglig katalogserver och auktoriseringsleverantörer samt ändringar i serverkonfigurationen

### Aktivera eller inaktivera händelsegranskning {#enable-or-disable-event-auditing}

Du kan aktivera och inaktivera granskning av händelser relaterade till servern, principskyddade dokument, profiler, principuppsättningar och användare. När du aktiverar händelsegranskning kan du välja att granska alla möjliga händelser eller välja specifika händelser att granska.

När du aktiverar servergranskning kan du visa de granskade händelserna på sidan Händelser.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Gransknings- och sekretessinställningar.
1. Om du vill konfigurera servergranskning väljer du Ja eller Nej under Aktivera servergranskning.
1. Om du valde Ja, under varje händelsekategori, gör du något av följande för att välja vilka alternativ som ska granskas:

   * Om du vill granska alla händelser i kategorin väljer du Alla.
   * Om du bara vill granska vissa händelser avmarkerar du Alla och markerar sedan kryssrutorna bredvid de händelser du vill granska.

     (Se [Alternativ för händelsegranskning](configuring-client-server-options.md#event-auditing-options).)

1. Klicka på OK.

>[!NOTE]
>
>När du arbetar med webbsidor bör du undvika att använda webbläsarknapparna, t.ex. bakåtknappen, uppdateringsknappen och bakåtpilen eller framåtpilen eftersom den här åtgärden kan orsaka oönskad datainhämtning och problem med visningen av data.

### Aktivera eller inaktivera sekretessmeddelanden {#enable-or-disable-privacy-notification}

Du kan aktivera och inaktivera ett sekretessmeddelande. När du aktiverar sekretessmeddelanden visas ett meddelande när en mottagare försöker öppna ett policyskyddat dokument. Meddelandet informerar användaren om att dokumentanvändningen håller på att granskas. Du kan också ange en URL som användaren kan använda för att visa din sekretesspolicysida, om en sådan finns tillgänglig.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Gransknings- och sekretessinställningar.
1. Om du vill konfigurera sekretessmeddelandet väljer du Ja eller Nej under Aktivera sekretessmeddelande.

   Om principen som bifogas till ett dokument tillåter anonym användaråtkomst och Aktivera sekretessmeddelande är inställd på Nej, uppmanas användaren inte att logga in och sekretessmeddelandet visas inte.

   Om principen som bifogas till ett dokument inte tillåter anonym användaråtkomst, kommer användaren att se meddelandet om sekretessmeddelanden.

1. Ange URL:en till din integritetspolicy i rutan Sekretessadress (URL). Om rutan Sekretess-URL lämnas tom visas sekretesssidan från adobe.com.
1. Klicka på OK.

>[!NOTE]
>
>Om du inaktiverar sekretessmeddelandet inaktiveras inte granskning av dokumentanvändning. Granskningsåtgärder och anpassade åtgärder som stöds via utökad användningsspårning kan fortfarande samla in information om användarbeteenden.

### Importera en anpassad granskningshändelsetyp {#import-a-custom-audit-event-type}

Om du använder ett program som har aktiverats för dokumentsäkerhet och som stöder granskning av ytterligare händelser, t.ex. händelser som är specifika för en viss filtyp, kan en Adobe-partner förse dig med anpassade granskningshändelser som du kan importera till dokumentsäkerhet. Använd bara den här funktionen om du har fått anpassade händelsetyper från en Adobe-partner.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Händelsehantering.
1. Klicka på Bläddra för att gå till XML-filen som ska importeras och klicka på Importera.
1. Vid import skrivs befintliga anpassade granskningshändelsetyper över på servern om identiska händelsekod- och namnutrymmeskombinationer hittas.
1. Klicka på OK.

### Ta bort en anpassad granskningshändelsetyp {#delete-a-custom-audit-event-type}

1. I administrationskonsolen klickar du på Tjänster > dokumentsäkerhet > Konfiguration > Händelsehantering.
1. Markera kryssrutan bredvid den anpassade granskningshändelsetypen som ska tas bort och klicka på Ta bort.
1. Klicka på OK.

### Exportera granskningshändelser {#export-audit-events}

Du kan exportera granskningshändelser till en fil i arkiveringssyfte.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Händelsehantering.
1. Redigera inställningarna under Exportera granskningshändelser efter behov. Du kan ange:

   * minimiåldern för revisionshändelser som ska exporteras
   * det maximala antalet granskningshändelser som ska inkluderas i en enda fil. Servern genererar en eller flera filer baserat på det här värdet.
   * mappen där filen ska skapas. Den här mappen finns på Forms Server. Om mappsökvägen är relativ är den relativ till programserverns rotkatalog.
   * filprefixet som ska användas för granskningshändelsefilerna
   * filens format, antingen en kommaavgränsad värdefil (CSV) som är kompatibel med Microsoft Excel eller en XML-fil.

1. Klicka på Exportera. Om du vill avbryta exporten klickar du på Avbryt export. Om en annan användare har schemalagt en export är knappen Avbryt export inte tillgänglig förrän exporten är klar. Knappen Avbryt export är inte tillgänglig om en annan användare har schemalagt en export. Om du vill kontrollera om en schemalagd export eller borttagning har startats eller slutförts klickar du på Uppdatera.

### Ta bort granskningshändelser {#delete-audit-events}

Du kan ta bort granskningshändelser som är äldre än ett visst antal dagar.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Händelsehantering.
1. Under Ta bort granskningshändelser anger du antalet dagar i rutan Ta bort granskningshändelser som är äldre än.
1. Klicka på Ta bort. Klicka på Exportera. Om du vill avbryta borttagningen klickar du på Avbryt borttagning. Om en annan användare har schemalagt en borttagning är knappen Avbryt borttagning inte tillgänglig förrän exporten är klar. Knappen Avbryt borttagning är inte tillgänglig om en annan användare har schemalagt en export. Om du vill kontrollera om en schemalagd borttagning har startats eller avslutats klickar du på Uppdatera.

### Alternativ för händelsegranskning {#event-auditing-options}

Du kan aktivera och inaktivera händelsegranskning och ange vilka typer av händelser som ska granskas.

**Dokumenthändelser**

**Visa dokument:** En mottagare visar ett policyskyddat dokument.

**Stäng dokument:** En mottagare stänger ett principskyddat dokument.

**Skriv ut låg upplösning** En mottagare skriver ut ett principskyddat dokument med det angivna alternativet för låg upplösning.

**Hög utskriftsupplösning:** En mottagare skriver ut ett policyskyddat dokument med ett alternativ för hög upplösning angivet.

**Lägg till anteckning i dokument:** En mottagare lägger till en anteckning i ett PDF-dokument.

**Återkalla dokument:** En användare eller administratör återkallar åtkomst till ett principskyddat dokument.

**Återkalla dokument:** En användare eller administratör återställer åtkomsten till ett principskyddat dokument.

**Formulärfyllning:** En mottagare anger information i ett PDF-dokument som är ett ifyllbart formulär.

**Borttagen princip:** En utgivare tar bort en princip från ett dokument för att återkalla säkerhetsskyddet.

**Ändra URL för dokumentåterkallning:** Ett anrop från API-nivån ändrar den spärr-URL som har angetts för åtkomst till ett nytt dokument som ersätter ett återkallat dokument.

**Ändra dokument:** En mottagare ändrar innehållet i ett policyskyddat dokument.

**Signera dokument:** En mottagare signerar ett dokument.

**Skydda ett nytt dokument:** En användare tillämpar en profil för att skydda ett dokument.

**Byt princip på dokument:** En användare eller administratör växlar den princip som är kopplad till ett dokument.

**Publicera dokument som:** Ett nytt dokument vars documentName och license är identiska med ett befintligt dokument är registrerat på servern och dokumenten har ingen överordnad-underordnad relation. Den här händelsen kan utlösas med AEM-formulären SDK.

**Upprepa dokument:** Ett nytt dokument vars documentName och license är identiska med ett befintligt dokument är registrerat på servern och dokumenten har en överordnad-underordnad relation. Den här händelsen kan utlösas med AEM-formulären SDK.

**Principhändelser**

**Princip:** En användare eller administratör skapar en princip.

**Aktiverad princip:** En administratör tillgängliggör en princip.

**Ändrad princip:** En användare eller administratör ändrar en princip.

**Inaktiverad princip:** En administratör gör att en princip inte är tillgänglig.

**Borttagen princip:** En användare eller administratör tar bort en princip.

**Ändra principägare:** Ett anrop från API-nivån ändrar principägaren.

**Användarhändelser**

**Borttagen användare:** En administratör tar bort ett användarkonto.

**Registrera inbjuden användare:** En extern användare registrerar sig med dokumentsäkerhet.

**Inloggningen lyckades:** Administratörer eller användare har gjort ett inloggningsförsök.

**Inbjudna användare:** Dokumentsäkerhet bjuder in en användare att registrera sig.

**Aktiverade användare:** Externa användare aktiverar sina konton med hjälp av URL:en i aktiveringsmeddelandet, eller så aktiverar en administratör ett konto.

**Ändra lösenord:** Inbjudna användare ändrar sina lösenord eller en administratör återställer ett lösenord för en lokal användare.

**Misslyckad inloggning:** Administratörer eller användare misslyckades med inloggningsförsök.

**Inaktiverade användare:** En administratör inaktiverar ett lokalt användarkonto.

**Profiluppdatering:** Inbjudna användare ändrar namn, organisationsnamn och lösenord.

**Konto låst:** En administratör låser ett konto.

**Principuppsättningshändelser**

**Skapad
Principuppsättning:** En administratör eller principuppsättningskoordinator skapar en principuppsättning.

**Borttagen principuppsättning:** En administratör eller koordinator för principuppsättningen tar bort en principuppsättning.

**Ändrad principuppsättning:** En administratör eller koordinator för principuppsättningen ändrar en principuppsättning.

**Systemhändelser**

**katalog
Synkroniseringen är klar:** Den här informationen är inte tillgänglig från sidan Händelser. Den aktuella katalogsynkroniseringsinformationen, inklusive det aktuella synkroniseringstillståndet och tidpunkten för den senaste synkroniseringen, visas på sidan Domänhantering. Om du vill få åtkomst till sidan Domänhantering i administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.

**Klient som aktiverar offlineåtkomst:** En användare har aktiverat offlineåtkomst till dokument som är skyddade mot servern på användarens dator.

**Synkroniserad klient** Klientprogrammet måste synkronisera information med servern för att tillåta offlineåtkomst.

**Versionsmatchningsfel:** En version av AEM Forms SDK som inte är kompatibel med servern försökte ansluta till servern.

**Katalogsynkroniseringsinformation:** Den här informationen är inte tillgänglig från sidan Händelser. Den aktuella katalogsynkroniseringsinformationen, inklusive det aktuella synkroniseringstillståndet och tidpunkten för den senaste synkroniseringen, visas på sidan Domänhantering. Om du vill få åtkomst till sidan Domänhantering i administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering.

**Serverkonfigurationsändring:** Ändringar i serverkonfigurationen som görs antingen via webbsidorna eller manuellt genom att en config.xml-fil importeras. Detta inkluderar ändringar av bas-URL:en, timeout-problem för sessioner, inloggningslås, kataloginställningar, tangentöverrullningar, SMTP-serverinställningar för extern registrering, konfiguration av vattenstämplar, visningsalternativ osv.

## Konfigurera utökad användningsspårning {#configuring-extended-usage-tracking}

Dokumentsäkerhet kan spåra olika anpassade händelser som kan utföras på ett skyddat dokument. Du kan aktivera spårning av händelser från dokumentsäkerhetsservern på global nivå eller på en principnivå. Du kan sedan ställa in en JavaScript för att fånga specifika åtgärder som utförts i det skyddade PDF-dokumentet, som att klicka på en knapp eller spara dokumentet. Dessa användningsdata skickas som en XML-fil i nyckelvärdepar som du kan använda för ytterligare analys. Slutanvändare som har åtkomst till skyddade dokument kan tillåta eller neka sådan spårning från klientprogrammet.

Om spårning är aktiverat på global nivå kan du åsidosätta den här inställningen på principnivå och inaktivera den för en viss princip. Åsidosättning på principnivå är inte möjlig om spårning är inaktiverat på global nivå. Listan över spårade händelser skickas automatiskt till servern när antalet händelser når 25 eller när dokumentet stängs. Du kan också konfigurera skriptet så att händelselistan skickas som du vill. Du kan anpassa händelsespårningen genom att komma åt egenskaperna och metoderna för dokumentsäkerhetsobjektet.

När du har aktiverat spårning aktiveras spårning som standard för alla profiler som skapas. Profiler som skapas innan spårning aktiveras på servern måste uppdateras manuellt.

### Aktivera eller inaktivera utökad användningsspårning {#enable-or-disable-extended-usage-tracking}

Kontrollera att Servergranskning är aktiverat innan du börjar. Mer information om granskning finns i [Konfigurera händelsegranskning och sekretessinställningar](configuring-client-server-options.md#configuring-event-auditing-and-privacy-settings).

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Gransknings- och sekretessinställningar.
1. Om du vill konfigurera utökad användningsspårning väljer du Ja eller Nej under Aktivera spårning.
1. Om du vill ange markeringen för kryssrutan Tillåt insamling av detaljerade användningsdata på inloggningssidan väljer du Ja eller Nej under Aktivera spårningsstandard.

Om du vill visa spårade händelser kan du använda filtret Dokumenthändelser på sidan Händelser. De händelser som spåras med JavaScript är märkta som Detaljerad användningsspårning. Mer information om händelser finns i [Övervaka händelser](/help/forms/using/admin-help/monitoring-events.md#monitoring-events).

## Konfigurera visningsinställningar för dokumentsäkerhet {#configure-document-security-display-settings}

1. I administrationskonsolen klickar du på Tjänster > dokumentsäkerhet > Konfiguration > Visningsalternativ.
1. Konfigurera inställningarna och klicka på OK.

### Visningsinställningar {#display-settings}

**Rader att visa för sökresultat:** Antal rader som visas på en sida när sökningar utförs.

**Anpassning för klientinloggningsdialogrutan**

Dessa inställningar styr texten som visas i inloggningsprompten som visas när en användare loggar in på dokumentsäkerhet via ett klientprogram.

**Välkomsttext:** Välkomstmeddelandetexten, till exempel &quot;Logga in med ditt användarnamn och lösenord&quot;. Välkomstmeddelandet bör innehålla information om hur du loggar in på dokumentsäkerhet och hur du kontaktar en administratör eller annan utsedd supportperson i organisationen för att få hjälp. Externa användare kan till exempel behöva kontakta en administratör om de glömmer bort sina lösenord eller behöver hjälp med registreringen eller inloggningen. Den maximala längden på välkomsttexten är 512 tecken.

**Användarnamnstext:** Textetiketten för användarnamnrutan.

**Lösenordstext:** Textetiketten för lösenordsrutan.

**Anpassning för dialogrutan för autentisering av klientcertifikat**

De här inställningarna styr texten som visas i dialogrutan för certifikatautentisering.

**Välj
Autentiseringstyp:** Den text som visas för att instruera en användare att välja en autentiseringstyp.

**Välj certifikattext:** Den text som visas för att instruera en användare att välja en certifikattyp.

**Certifikat är inte tillgängliga, feltext:** Meddelande på upp till 512 tecken som ska visas när det valda certifikatet inte är tillgängligt.

**Anpassning för visning av klientcertifikat**

**Visa endast pålitliga autentiseringsutfärdare:** När det här alternativet är markerat ger klientprogrammet bara användaren certifikat från autentiseringsutfärdare som AEM-formulär har konfigurerats att lita på (se Hantera certifikat och autentiseringsuppgifter). När det här alternativet inte är markerat visas en lista med alla certifikat i användarens system.

## Konfigurera dynamiska vattenstämplar {#configure-dynamic-watermarks}

Med dokumentskydd kan du konfigurera standardinställningar för alternativet för dynamisk vattenstämpel som du kan använda när du skapar profiler. En *vattenstämpel* är en bild som placeras ovanpå text i dokumentet. Det är användbart för att spåra innehållet i ett dokument och kan hjälpa till att identifiera olaglig användning av innehållet.

En dynamisk vattenstämpel kan bestå av antingen text som består av definierade variabler som användar-ID, datum och anpassad text, eller avancerat innehåll i en PDF. Du kan konfigurera vattenstämplar med flera element där var och en har sin egen placering och formatering.

Vattenstämplar kan inte redigeras och är därför ett säkrare sätt att säkerställa sekretessen för dokumentinnehållet. Dynamiska vattenstämplar ser också till att en vattenstämpel visar tillräckligt med användarspecifik information för att avskräcka från ytterligare distribution av dokumentet.

Den vattenstämpel som en profil anger visas i det profilskyddade dokumentet när en mottagare visar eller skriver ut dokumentet. Till skillnad från permanenta vattenstämplar sparas aldrig en dynamisk vattenstämpel i dokumentet, vilket ger den flexibilitet som krävs när du distribuerar ett dokument i en intranätmiljö för att se till att visningsprogrammet visar identiteten för den specifika användaren. Om ett dokument har flera användare innebär användningen av den dynamiska vattenstämpeln att du kan använda ett dokument i stället för flera versioner, där var och en har olika vattenstämpel. Vattenstämpeln som visas återspeglar den aktuella användarens identitet.

Observera att dynamiska vattenstämplar skiljer sig från de vattenstämplar som användare kan lägga till direkt i dokumentet i Acrobat. Resultatet blir att du kan ha två vattenstämplar i ett policyskyddat dokument.

### Att tänka på när du skapar vattenstämplar {#considerations-when-creating-watermarks}

Du kan skapa dynamiska vattenstämplar med flera vattenstämpelelement där varje element anges som antingen text eller PDF. Du kan inkludera upp till fem element i en vattenstämpel.

Om du väljer en textbaserad vattenstämpel kan du ange flera element i vattenstämpeln med flera textposter och ange placeringen av varje element. Tilldela beskrivande namn till dessa element, t.ex. sidhuvud, sidfot och så vidare.

Om du till exempel vill ange olika text som vattenstämpel i sidhuvudet, sidfoten, marginalerna och över dokumentet, skapar du flera vattenstämpelelement och anger deras positioner. Om du vill att användar-ID:t för användaren och det aktuella åtkomstdatumet för dokumentet ska visas i sidhuvudet, principnamnet i högermarginalen och den anpassade texten&quot;CONFIDENTIAL&quot; ska visas diagonalt i dokumentet, definierar du separata vattenstämpelelement med text som typ och anger formatering och placering. När vattenstämpeln används i ett dokument, tillämpas alla element i vattenstämpeln samtidigt i dokumentet i den ordning som de läggs till i vattenstämpeln.

Normalt använder du PDF-baserade vattenstämplar för att inkludera grafikinnehåll som logotyper eller specialsymboler som copyright eller registrerat varumärke.

Du kan ändra gränsen för antalet vattenstämpelelement och PDF-filstorleken genom att ändra dokumentets säkerhetskonfigurationsfil. Se [Ändra konfigurationsparametrar för vattenstämpel](configuring-client-server-options.md#change-the-watermark-configuration-parameters).

Tänk på följande när du konfigurerar vattenstämplar:

* Du kan inte använda ett lösenordsskyddat PDF-dokument som vattenstämpelelement. Om vattenstämpeln som du skapar innehåller andra element som inte är lösenordsskyddade kommer de att användas som en del av vattenstämpeln.
* Du kan ändra den maximala filstorlek för PDF som du vill använda som vattenstämpelelement. Stora PDF-dokument som används som vattenstämplar försämrar prestanda vid offlinesynkronisering av dokument som används med sådana vattenstämplar. Se [Ändra konfigurationsparametrar för vattenstämpel](configuring-client-server-options.md#change-the-watermark-configuration-parameters).
* Endast den första sidan i den valda PDF-filen används som vattenstämpel. Kontrollera att den information som du vill ska visas som vattenstämpel är tillgänglig på själva sidan.
* Även om du kan ange skalning för PDF-dokumentet bör du överväga sidstorlek och layout för PDF om du tänker använda det som en vattenstämpel i sidhuvudet, sidfoten eller marginalerna.
* Ange rätt namn när du anger teckensnittsnamnet. AEM-formulär ersätter det teckensnitt som du har angett om det inte finns i klientdatorn där dokumentet öppnas.
* Om du har markerat text som vattenstämpelinnehåll fungerar inte alternativet Anpassa till sida för sidor som har olika bredd.
* När du anger placeringen av vattenstämpelelementen måste du se till att inte mer än ett element har samma placering. Om två vattenstämpelelement har samma placering, t.ex. mittpunkt, visas de överlappade i dokumentet och i den ordning som de lades till i vattenstämpeln.
* När du anger teckensnittsstorlek och -typ måste du se till att textlängden är helt synlig på sidan. Textinnehållet förs över till nya rader, så att vattenstämpelinnehållet som du vill ska finnas i marginalerna kan överlappa innehållsområdet på sidorna. Om dokumentet öppnas i Acrobat 9 kortas texten utanför den enskilda raden av.

### Begränsningar för dynamiska vattenstämplar {#limitations-of-dynamic-watermarks}

Vissa klientprogram kanske inte stöder dynamiska vattenstämplar. Se lämplig hjälp för Acrobat Reader DC-tillägg. Tänk dessutom på följande när det gäller de versioner av Acrobat som stöder dynamiska vattenstämplar:

* Du kan inte använda ett lösenordsskyddat PDF-dokument som vattenstämpelelement.
* Tidigare versioner av Acrobat och Adobe Reader än 10 har inte stöd för följande vattenstämpelfunktioner:

   * PDF vattenstämplar
   * Flera element i vattenstämpeln (Text/PDF)
   * Avancerade alternativ som sidintervall eller visningsalternativ
   * Textformateringsalternativ som angivet teckensnitt, teckensnittsnamn och färg. I tidigare versioner av Acrobat och Reader visas emellertid textinnehållet med standardteckensnitt och -färg.

* Acrobat 9.0 och tidigare versioner: Acrobat 9.0 och tidigare stöder inte principnamn i dynamiska vattenstämplar. Om Acrobat 9.0 öppnar ett policyskyddat dokument med en dynamisk vattenstämpel som innehåller ett principnamn och andra dynamiska data, visas vattenstämpeln utan principnamnet. Om den dynamiska vattenstämpeln bara innehåller principnamnet visas ett felmeddelande i Acrobat

### Lägga till en dynamisk vattenstämpelmall {#add-a-dynamic-watermark-template}

Du kan skapa dynamiska vattenstämpelmallar. Mallarna är fortfarande tillgängliga som konfigurationsalternativ för profiler som administratörer eller användare skapar.

>[!NOTE]
>
>Konfigurationsinformation för dynamiska vattenstämplar registreras inte med annan konfigurationsinformation när du exporterar en konfigurationsfil.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Vattenstämplar.
1. Klicka på Ny.
1. Skriv ett namn för den nya vattenstämpeln i rutan Namn.

   ***Obs!**: Du kan inte använda vissa specialtecken i namn eller beskrivningar för vattenstämplar eller vattenstämpelelement. Se begränsningarna i [Att tänka på vid redigering av profiler](/help/forms/using/admin-help/creating-policies.md#considerations-for-editing-policies).*

1. Under Namn, bredvid plustecknet, anger du ett beskrivande namn för vattenstämpelelementet, till exempel Rubrik, och lägger till en beskrivning. Expandera plustecknet för att visa alternativen.
1. Under Source väljer du typ av vattenstämpel som antingen Text eller PDF.
1. Om du markerade Text gör du följande:

   * Välj de vattenstämpeltyper som ska inkluderas. Om du väljer Egen text skriver du den text som ska visas för vattenstämpeln i den intilliggande rutan. Tänk på den textlängd som kommer att visas som vattenstämpel.
   * Ange textformateringsegenskaper som teckensnittsnamn, teckenstorlek, förgrundsfärg och bakgrundsfärg för textinnehållet i vattenstämpeltexten. Ange förgrunds- och bakgrundsfärgen som hexadecimala värden.

     ***Obs!**&#x200B;Om du väljer skalförändringsalternativet Anpassa till sidan är egenskapen för teckensnittsstorlek inte tillgänglig för redigering.*

1. Om du har valt PDF som alternativ för vattenstämpel klickar du på **Bläddra** bredvid Välj PDF för vattenstämpel för att välja det PDF-dokument som du vill använda som vattenstämpel.

   ***Obs!**&#x200B;Använd inte ett lösenordsskyddat PDF-dokument. Om du anger en lösenordsskyddad PDF som vattenstämpelelement används inte vattenstämpeln.*

1. Välj antingen Ja eller Nej under Använd som bakgrund.

   **Obs!**: För närvarande visas vattenstämpeln i förgrunden oavsett den här inställningen.

1. Konfigurera alternativen Lodrät justering och Vågrät justering för att styra var vattenstämpeln visas i dokumentet.
1. Välj Anpassa till sidan eller välj % och ange en procentsats i rutan. Värdet måste vara ett heltal, inte ett bråk. Om du vill konfigurera vattenstämpelstorleken kan du använda ett värde som är procentandelen av sidan eller ställa in vattenstämpeln så att den passar sidans storlek.
1. I rutan Rotation anger du med hur många grader vattenstämpeln ska roteras. Intervallet är mellan -180 och 180. Använd ett negativt värde om du vill rotera vattenstämpeln motsols. Värdet måste vara ett heltal, inte ett bråk.
1. Ange ett procentvärde i rutan Opacitet. Använd ett heltal, inte en bråkdel.
1. Ange följande under Avancerade alternativ:

   **Alternativ för sidintervall**

   Ange det sidintervall där vattenstämpeln ska visas. Ange startsidan som 1 och slutsidan som -1 om du vill att alla sidor ska ha vattenstämpeln.

   **Visningsalternativ**

   Välj var du vill att vattenstämpeln ska visas. Som standard visas vattenstämpeln både på en mjuk kopia (online) och på en papperskopia (utskrift).

1. Klicka på **Nytt** under vattenstämpelelement om du vill lägga till fler vattenstämpelelement om det behövs.
1. Klicka på OK.

### Redigera en dynamisk vattenstämpelmall {#edit-a-dynamic-watermark-template}

1. I administrationskonsolen klickar du på Tjänster > dokumentsäkerhet > Konfiguration > Vattenstämplar.
1. Klicka på önskad vattenstämpel i listan.
1. Ändra inställningarna på sidan Redigera vattenstämplar.
1. Klicka på OK.

### Ta bort en dynamisk vattenstämpelmall {#delete-a-dynamic-watermark-template}

När du tar bort en dynamisk vattenstämpel är den inte längre tillgänglig att lägga till i en ny profil. Vattenstämpeln finns dock kvar i befintliga profiler som använder den, och dokument som skyddas av den aktuella profilen fortsätter att visa den dynamiska vattenstämpeln tills du eller en användare redigerar den princip som innehåller den borttagna vattenstämpeln. När profilen har redigerats används inte längre vattenstämpeln. Ett meddelande visas som anger att den befintliga vattenstämpeln tas bort från profilen och att användaren kan välja en annan som ska ersätta den.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Vattenstämplar.
1. Markera kryssrutan bredvid önskad vattenstämpel och klicka på Ta bort.
1. Klicka på OK.

## Konfigurerar inbjuden användarregistrering {#configuring-invited-user-registration}

Användare som är externa i din organisation kan registrera med dokumentsäkerhet. Inbjudna användare som registrerar och aktiverar sina konton kan logga in på dokumentsäkerhet med sin e-postadress och det lösenord de skapar när de registrerar sig. Registrerade inbjudna användare kan använda principskyddade dokument som de har behörighet till.

När inbjudna användare aktiveras blir de lokala användare. Lokala användare kan konfigureras och hanteras med området Inbjudna och Lokala användare. (Se [Hantera inbjudna och lokala användarkonton](/help/forms/using/admin-help/invited-local-user-accounts.md#managing-invited-and-local-user-accounts).)

Beroende på vilka funktioner du aktiverar för inbjudna användare kan de även använda följande dokumentsäkerhetsfunktioner:

* Tillämpa profiler på dokument
* Skapa profiler
* Lägg till inbjudna användare till profiler

Dokumentsäkerhet genererar automatiskt en registreringsinbjudan via e-post när följande händelser inträffar, såvida inte användaren redan finns i LDAP-källkatalogen eller tidigare har bjudits in att registrera:

* En befintlig användare lägger till en inbjuden användare i en profil
* En administratör lägger till ett inbjudet användarkonto på sidan Inbjuden användarregistrering

E-postmeddelandet innehåller en länk till en registreringssida och information om hur du registrerar dig. När den inbjudna användaren har registrerat sig utfärdar dokumentsäkerheten ett aktiveringsmejl med en länk till en aktiveringssida. När det är aktiverat fortsätter kontot att gälla tills du inaktiverar eller tar bort det.

Om du aktiverar den inbyggda registreringen anger du SMTP-servern, e-postinformation för registrering, åtkomstfunktioner och återställer e-postinformation för lösenord endast en gång. Innan du aktiverar den inbyggda registreringen bör du kontrollera att du har skapat en lokal domän i Användarhantering som har tilldelat rollen&quot;Bjud in användare för dokumentsäkerhet&quot; till lämpliga användare och grupper i organisationen. (Se [Lägg till en lokal domän](/help/forms/using/admin-help/adding-domains.md#add-a-local-domain) och [Skapa och konfigurera roller](/help/forms/using/admin-help/creating-configuring-roles.md#creating-and-configuring-roles).) Om du inte använder inbyggd registrering måste du ha ett eget användarregistreringssystem som skapats med AEM-formulären SDK. Mer information finns i hjälpen om utveckling av SPI för AEM-formulär i [Programmering med AEM-formulär](/help/forms/developing/introducing-java-api-soap-quick.md). Om du inte använder alternativet Inbyggd registrering rekommenderar vi att du konfigurerar ett meddelande i aktiveringsmeddelandet och på klientinloggningsskärmen för att informera användarna om hur de kontaktar administratören för ett nytt lösenord eller för annan information.

**Aktivera och konfigurera registrering av inbjudna användare**

Som standard är den inbjudna användarregistreringen inaktiverad. Du kan aktivera och inaktivera inbjudna användarregistreringar för dokumentsäkerhet efter behov.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Inbjuden användarregistrering.
1. Välj Aktivera registrering av inbjudna användare.
1. (Valfritt) Uppdatera inställningarna för den inbjudna användarregistreringen efter behov:

   * [Uteslut eller inkludera en extern användare eller grupp](configuring-client-server-options.md#exclude-or-include-an-external-user-or-group)
   * [Parametrar för server- och registreringskonton](configuring-client-server-options.md#server-and-registration-account-parameters)
   * [E-postinställningar för registreringsinbjudan](configuring-client-server-options.md#registration-invitation-email-settings)
   * [E-postinställningar för aktivering](configuring-client-server-options.md#activation-email-settings)
   * [Konfigurera en e-postadress för återställning av lösenord](configuring-client-server-options.md#configure-a-password-reset-email)

1. (Valfritt) Välj Ja under Inbyggd registrering för att aktivera det här alternativet. Om du inte aktiverar den inbyggda registreringen måste du skapa ett eget användarregistreringssystem.
1. Klicka på OK.

### Uteslut eller inkludera en extern användare eller grupp {#exclude-or-include-an-external-user-or-group}

Du kan begränsa registreringen med dokumentsäkerhet för vissa externa användare eller användargrupper. Det här alternativet är användbart om du till exempel vill tillåta åtkomst till en viss användargrupp, men exkludera specifika personer som ingår i gruppen.

Följande inställningar finns under Filter för e-postbegränsning på sidan Inbjuden användarregistrering.

**Undantag:** Ange e-postadressen till en användare eller grupp som ska uteslutas. Om du vill exkludera flera användare eller grupper skriver du varje e-postadress på en ny rad. Om du vill utesluta alla användare som tillhör en viss domän anger du ett jokertecken och domännamnet. Om du till exempel vill utesluta alla användare i domänen example.com anger du &ast;.example.com.

**Inkludering:** Ange e-postadressen till en användare eller grupp som ska inkluderas. Om du vill inkludera flera användare eller grupper skriver du varje e-postadress på en ny rad. Om du vill inkludera alla användare som tillhör en viss domän anger du ett jokertecken och domännamnet. Om du till exempel vill ta med alla användare i domänen example.com anger du &ast;.example.com.

### Parametrar för server- och registreringskonton {#server-and-registration-account-parameters}

Följande inställningar finns i området Allmänna inställningar på sidan Inbjuden användarregistrering.

**SMTP-värd:** SMTP-serverns värdnamn. SMTP-servern hanterar meddelandena om utgående e-post för att registrera och aktivera inbjudna användarkonton.

Om det behövs av SMTP-värden anger du nödvändig information i rutorna SMTP-serverkontonamn och SMTP-serverkontolösenord för att ansluta till SMTP-servern. Vissa organisationer tillämpar inte detta krav. Kontakta systemadministratören om du behöver information.

**Namn på socketklass för SMTP-server:** Socket-klassnamn för SMTP-servern. Exempel: javax.net.ssl.SSLSocketFactory.

**Innehållstyp för e-post:** Accepterad MIME-typ som text/plain eller text/html.

**E-postkodning:** Kodningsformat som ska användas när e-postmeddelanden skickas. Du kan ange valfri kodning, till exempel UTF-8 för Unicode eller ISO-8859-1 för Latin. Standardvärdet är UTF-8.

**E-postadress för omdirigering:** När du anger en e-postadress för den här inställningen skickas alla nya inbjudningar till den angivna adressen. Den här inställningen kan vara användbar i testsyfte.

**Använd lokala domäner:** Välj lämplig domän. Kontrollera att du har skapat domänen med hjälp av Användarhantering vid en ny installation. Om detta är en uppgradering skapades en extern användardomän under uppgraderingen och kan användas.

**Använd SSL för SMTP-server:** Välj det här alternativet om du vill aktivera SSL för SMTP-servern.

**Visa inloggningslänk på registreringssidan:** Visar en inloggningslänk på registreringssidan som visas för inbjudna användare.

**Så här aktiverar du TLS (Transport Layer Security) för SMTP-servern**

1. Öppna administrationskonsolen.

   Standardplatsen för administrationskonsolen är `https://<server>:<port>/adminui`.

1. Navigera till Hem > Tjänster > Dokumentsäkerhet > Konfiguration > Inbjuden användarregistrering.
1. På Inbjuden användarregistrering anger du alla konfigurationsinställningar och klickar sedan på OK.

   >[!NOTE]
   >
   >Om du använder Microsoft Office 365 som SMTP-server för att skicka inbjudningar till användarregistrering använder du följande inställningar:
   >
   >**SMTP-värd:** smtp.office365.com
   >**Port:** 587

1. Därefter måste du uppdatera config.xml. Se [Konfiguration för att aktivera SMTP för TLS (Transport Layer Security)](configuring-client-server-options.md#configuration-to-enable-smtp-for-transport-layer-security-tls)

>[!NOTE]
>
>Om du ändrar något i alternativen för Inbjuden användarregistrering skrivs filen config.xml över och TLS inaktiveras. Om du skriver över ändringarna måste du utföra ovanstående steg för att återaktivera TLS-stödet för Inbjuden användarregistrering.

### E-postinställningar för registreringsinbjudan {#registration-invitation-email-settings}

Dokumentsäkerheten skickar automatiskt en registreringsinbjudan via e-post när du skapar ett inbjudet användarkonto eller när en befintlig användare lägger till en extern mottagare som inte tidigare har registrerat sig eller har bjudits in att registrera sig för en profil. E-postmeddelandet innehåller en länk som mottagaren kan använda för att komma åt registreringssidan och ange information om ditt personliga konto, inklusive användarnamn och lösenord. Lösenordet kan bestå av en kombination av åtta tecken.

När mottagaren aktiverar kontot blir användaren en lokal användare.

Följande inställningar finns under E-postkonfiguration för inbjudan på sidan Inbjuden användarregistrering.

**Från:** E-postadressen som inbjudan skickas från. Standardformatet för Från-e-postadressen är postmaster@[your_installation_domain].com.

**Ämne:** Standardämne för e-postmeddelandet med inbjudan.

**Tidsgräns:** Antalet dagar efter vilket registreringsinbjudan upphör om den externa användaren inte registrerar sig. Standardvärdet är 30 dagar.

**Meddelande:** Den text som visas i meddelandetexten och som uppmanar användaren att registrera sig.

### E-postinställningar för aktivering {#activation-email-settings}

Dokumentsäkerheten skickar ett aktiveringsmejl när inbjudna användare har registrerat sig. E-postmeddelandet om aktivering innehåller en länk till kontoaktiveringssidan där användarna kan aktivera sina konton. När kontona är aktiverade kan användare logga in på dokumentsäkerhet med hjälp av sin e-postadress och det lösenord de skapade när de registrerade.

När mottagaren aktiverar användarkontot blir användaren en lokal användare.

Följande inställningar finns i området E-postkonfiguration för aktivering på sidan Inbjuden användarregistrering.

>[!NOTE]
>
>Vi rekommenderar även att du konfigurerar ett meddelande på inloggningsskärmen för att ge externa användare råd om hur de ska kontakta sin administratör för att få ett nytt lösenord eller annan information.

**Från:** E-postadressen som aktiveringsmeddelandet skickas från. Den här e-postadressen tar emot meddelanden om misslyckad leverans från registrantens e-postvärd och även meddelanden som mottagaren skickar som svar på registreringsmeddelandet. Standardformatet för Från-e-postadressen är postmaster@[your_installation_domain].com.

**Ämne:** Standardämne för e-postmeddelandet om aktivering.

**Timeout:** Antalet dagar efter vilket aktiveringsinbjudan upphör om användaren inte aktiverar kontot. Standardvärdet är 30 dagar.

**Meddelande:** Den text som visas i meddelandetexten i ett meddelande som anger att mottagarens användarkonto måste aktiveras. Du kan även inkludera information om hur du kontaktar en administratör för att få ett nytt lösenord.

### Konfigurera en e-postadress för återställning av lösenord {#configure-a-password-reset-email}

Om du måste återställa en inbjuden användares lösenord skapas ett bekräftelsemeddelande som uppmanar användaren att välja ett nytt lösenord. Det går inte att fastställa en användares lösenord. Om användaren glömmer bort det måste du återställa det.

Följande inställningar finns i området Återställ e-post för lösenord på sidan Inbjuden användarregistrering.

**Från:** E-postadressen som e-postmeddelandet för lösenordsåterställning skickas från. Standardformatet för Från-e-postadressen är postmaster@[your_installation_domain].com.

**Ämne:** Standardämne för e-postmeddelandet som återställs.

**Meddelande:** Den text som visas i meddelandetexten i ett meddelande som anger att mottagarens externa användarlösenord har återställts.

## Möjliggör för användare och grupper att skapa profiler {#enable-users-and-groups-to-create-policies}

På konfigurationssidan finns en länk till sidan Mina principer, där du anger vilka användare som kan skapa mina principer och vilka användare och grupper som visas i sökresultaten. Sidan Mina principer har två flikar:

**Fliken Skapa principer:** Används för att konfigurera användarbehörigheter för att skapa anpassade profiler.

**Fliken Synliga användare och grupper:** Används för att kontrollera vilka användare och grupper som visas i användarsökresultaten. Den överordnade användaren eller administratören för principuppsättningen måste välja och lägga till domäner, skapade med användarhantering, i den synliga användaren och gruppen för varje principuppsättning. Den här listan är synlig för principuppsättningens koordinator och används för att ange gränser för vilka domäner som principuppsättningens koordinator kan bläddra i när användaren väljer att lägga till i profiler.

Innan du ger användarna behörighet att skapa anpassade profiler bör du tänka på hur mycket åtkomst eller kontroll du vill att enskilda användare ska ha. Tänk också på hur exponerad du vill att användare och grupper ska vara när du gör dem synliga för sökningar.

### Ange användare och grupper som kan skapa profiler {#specify-users-and-groups-who-can-create-policies}

Som administratör anger du vilka användare och grupper som kan skapa anpassade profiler. Den här behörigheten kan anges på användar- och gruppnivå. Sökfunktionen söker i databasen för användarhantering efter användare och grupper.

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet > Konfiguration > Mina principer.
1. På sidan Mina profiler klickar du på fliken Skapa profiler och sedan på Lägg till användare och grupper.
1. I rutan Sök skriver du användarnamnet eller e-postadressen för den användare eller grupp som du söker efter. Om du inte har den här informationen lämnar du rutan tom. Du kan också skriva ett partiellt namn eller en e-postadress, till exempel när du bara känner till de två första bokstäverna i ett användarnamn.
1. Välj sökparametrarna Namn eller E-post i listan Använda.
1. Välj Grupp eller Användare i typlistan för att begränsa sökningen.
1. I listan In väljer du den domän du vill söka i. Om du inte känner till användarens eller gruppens domän väljer du Alla domäner.
1. I visningslistan anger du antalet sökresultat som ska visas per sida och klickar sedan på Sök.
1. Om du vill lägga till Mina profiler för användare och grupper markerar du kryssrutan för varje användare och grupp som ska läggas till.
1. Klicka på Lägg till och sedan på OK.

Dina valda användare och grupper har nu behörighet att skapa anpassade profiler.

### Ta bort behörigheten Skapa anpassade profiler från en användare eller grupp {#remove-the-create-custom-policies-permission-from-a-user-or-group}

1. På dokumentsäkerhetssidan klickar du på Konfiguration > Mina principer.
1. Klicka på fliken Skapa profiler på sidan Mina profiler. Användare och grupper med behörigheter för att skapa anpassade profiler visas.
1. Markera kryssrutan bredvid de användare och grupper som du vill ta bort från den här behörigheten.
1. Klicka på Ta bort och sedan på OK.

### Ange användare och grupper som visas i sökningar {#specify-users-and-groups-that-are-visible-in-searches}

När användare hanterar sina egna profiler kan de söka efter användare och grupper att lägga till i sina profiler. Ange de domäner från vilka användare och grupper visas i sökningarna.

1. På dokumentsäkerhetssidan klickar du på Konfiguration > Mina principer.
1. Klicka på fliken Synliga användare och grupper på sidan Mina profiler.
1. Om du vill göra användare och grupper i en domän synliga klickar du på Lägg till domäner, markerar domänerna och klickar på Lägg till. Om du vill ta bort en domän markerar du kryssrutan bredvid domännamnet och klickar på Ta bort.

## Redigera dokumentets säkerhetskonfigurationsfil manuellt {#manually-editing-the-document-security-configuration-file}

Du kan importera och exportera den konfigurationsinformation som lagras i dokumentsäkerhetsdatabasen. Du kan till exempel skapa en säkerhetskopia av konfigurationsinformationen när du går från en mellanlagring till en produktionsmiljö, eller redigera avancerade alternativ som bara kan konfigureras för att redigera den här filen.

Du kan göra följande ändringar med hjälp av konfigurationsfilen:

[Visa CATIA-behörigheter när du skapar och redigerar principer](configuring-client-server-options.md#display-catia-permissions-when-creating-and-editing-policies)

[Ange en tidsgräns för offlinesynkronisering](configuring-client-server-options.md#specify-a-timeout-period-for-offline-synchronization)

[Avvisa dokumentsäkerhetstjänster för specifika program](configuring-client-server-options.md#denying-document-security-services-for-specific-applications)

[Ändra konfigurationsparametrar för vattenstämpel](configuring-client-server-options.md#change-the-watermark-configuration-parameters)

[Inaktivera externa länkar](configuring-client-server-options.md#disabling-external-links)

>[!NOTE]
>
>Om du importerar konfigurationsfilen konfigureras systemet om baserat på informationen i filen. Undantagen är dynamisk vattenstämpelkonfiguration och anpassad händelseinformation, som inte sparas med den exporterade konfigurationsfilen. Konfigurera informationen manuellt i det nya systemet. Endast systemadministratörer eller konsulter som är vana vid dokumentsäkerhet och XML bör ändra innehållet i en konfigurationsfil, t.ex. för att konfigurera om en skadad inställning eller för att justera parametrar för ett visst företagsscenario.

**Exportera en konfigurationsfil**

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet 11 > Konfiguration > Manuell konfiguration.
1. Klicka på Exportera och spara konfigurationsfilen på en annan plats. Standardfilnamnet är config.xml.
1. Klicka på OK.
1. Innan du ändrar konfigurationsfilen bör du skapa en säkerhetskopia om du skulle behöva återställa den.

**Importera en konfigurationsfil**

1. I administrationskonsolen klickar du på Tjänster > Dokumentsäkerhet 11 > Konfiguration > Manuell konfiguration.
1. Klicka på Bläddra för att gå till konfigurationsfilen och klicka sedan på Importera. Du kan inte skriva sökvägen direkt i rutan Filnamn.
1. Klicka på OK.

### Ange en tidsgräns för offlinesynkronisering {#specify-a-timeout-period-for-offline-synchronization}

Dokumentsäkerhet gör att användare kan öppna och använda skyddade dokument när de inte är anslutna till dokumentsäkerhetsservern. Användarens klientprogram måste regelbundet synkronisera med servern för att dokument ska kunna användas offline. Första gången användare öppnar ett skyddat dokument tillfrågas de om deras dator ska ha behörighet att utföra periodisk klientsynkronisering.

Som standard sker synkroniseringen automatiskt var fjärde timme och vid behov när en användare är ansluten till dokumentsäkerhetsservern. Om offlineperioden för ett dokument förfaller när användaren är offline måste användaren ansluta till servern igen för att klientprogrammet ska kunna synkronisera med servern.

I dokumentets säkerhetskonfigurationsfil kan du ange standardfrekvensen för den automatiska bakgrundssynkroniseringen. Den här inställningen fungerar som klientprogram för standardtimeout-perioden, såvida inte klienten uttryckligen anger sitt eget timeout-värde.

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp noden `PolicyServer`. Under den noden letar du reda på noden `ServerSettings`.
1. Lägg till följande post i noden `ServerSettings` och spara sedan filen:

   `<entry key="BackgroundSyncFrequency" value="`*time* `"/>`

   där *time* är antalet sekunder mellan automatiska bakgrundssynkroniseringar. Om du skickade det här värdet till `0` sker alltid synkronisering. Standardvärdet är `14400` sekunder (var fjärde timme).

1. Importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)

### Avvisa dokumentsäkerhetstjänster för specifika program {#denying-document-security-services-for-specific-applications}

Du kan konfigurera dokumentsäkerhet för att neka tjänster till program som uppfyller specifika villkor. Kriterierna kan ange ett enskilt attribut, t.ex. ett plattformsnamn, eller ange flera uppsättningar attribut. Med den här funktionen kan du styra vilka krav som dokumentsäkerheten måste hantera. Här är några program med den här funktionen:

* **Intäktsskydd:** Du kanske vill neka åtkomst till klientprogram som inte stöder dina intäktskonventioner.
* **Programkompatibilitet:** En del program kan vara inkompatibla med reglerna eller beteendet för dokumentsäkerhetsservern.

När klientprogram försöker skapa en länk med dokumentsäkerhet, tillhandahåller de program-, version- och plattformsinformation. Dokumentsäkerhet jämför den här informationen med Neka-inställningar som hämtas från dokumentets säkerhetskonfigurationsfil.

Inställningarna för Neka kan innehålla flera uppsättningar villkor för Neka. Om alla attribut i en uppsättning matchar, nekas det begärande programmet åtkomst till dokumentets säkerhetstjänster.

Funktionen för denial of service kräver att klientprogram använder dokumentsäkerheten C++ Client SDK version 8.2 eller senare. Följande Adobe-produkter tillhandahåller produktinformation vid begäran om dokumentsäkerhetstjänster:

* Adobe Acrobat 9.0 Professional/Acrobat 9.0 Standard och senare
* Adobe Reader 9.0 och senare
* Acrobat Reader DC-tillägg för Microsoft Office 8.2 och senare

Klientprogram använder klient-API:t från SDK för dokumentsäkerhet C++ för att begära tjänster från dokumentsäkerhet. Klient-API-begäranden innehåller plattforms- och SDK-versionsinformation (som kompilerats i klient-API:t) och produktinformation som hämtats från klientprogrammet.

Klientprogram eller plugin-program tillhandahåller produktinformation när de implementerar en callback-funktion. Programmet innehåller följande information:

* Integratornamn
* Integratorversion
* Programfamilj
* Programnamn
* Programversion

Om någon information inte är tillämplig lämnar klientprogrammet motsvarande fält tomt.

Flera Adobe-program innehåller produktinformation när dokumentsäkerhetstjänster begärs, bland annat Acrobat, Adobe Reader och Acrobat Reader DC-tillägg för Microsoft Office.

**Acrobat och Adobe Reader**

När Acrobat eller Adobe Reader begär en tjänst från dokumentsäkerhet, tillhandahålls följande produktinformation:

* **Integrator:** Adobe Systems, Inc.
* **Integratorversion:** 1.0
* **Programfamilj:** Acrobat
* **Programnamn:** Acrobat
* **Programversion:** 9.0.0

**Acrobat Reader DC-tillägg för Microsoft Office**

Acrobat Reader DC-tillägg för Microsoft Office är en plugin som används med Microsoft Office-produkterna Microsoft Word, Microsoft Excel och Microsoft PowerPoint. När den begär en tjänst tillhandahåller den följande information:

* **Integrator:** Adobe Systems Incorporated
* **Integratorversion:** 8.2
* **Programfamilj:** Acrobat Reader DC-tillägg för Microsoft Office
* **Programnamn:** Microsoft Word, Microsoft Excel eller Microsoft PowerPoint
* **Programversion:** 2003 eller 2007

**Konfigurera dokumentsäkerhet för att neka tjänster för specifika program**

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp noden `PolicyServer`. Lägg till en `ClientVersionRules`-nod som direkt underordnad noden `PolicyServer`, om det inte finns någon:

   ```xml
    <node name="ClientVersionRules">
        <map>
            <entry key="infoURL" value="URL"/>
        </map>
        <node name="Denials">
            <map/>
            <node name="MyEntryName">
                <map>
                    <entry key="SDKPlatforms" value="platforms"/>
                    <entry key="SDKVersions" value="versions"/>
                    <entry key="AppFamilies" value="families"/>
                    <entry key="AppNames" value="names"/>
                    <entry key="AppVersions" value="versions"/>
                    <entry key="Integrators" value="integrators"/>
                    <entry key="IntegratorVersions" value="versions"/>
                </map>
            </node>
            <node name="MyOtherEntryName"
                <map>
                    [...]
                </map>
            </node>
            [...]
        </node>
    </node>
   ```

   där:

   `SDKPlatforms` anger plattformen som är värd för klientprogrammet. Möjliga värden är:

   * Microsoft Windows
   * APPLE OS X
   * Sun Solaris
   * HP-UX

   `SDKVersions` anger vilken version av dokumentets säkerhets-C++-klient-API som används av klientprogrammet. Exempel: `"8.2"`.

   `APPFamilies` definieras av klient-API:t.

   `AppName` anger namnet på klientprogrammet. Kommandon används som namnavgränsare. Om du vill ta med ett kommatecken i ett namn kan du undvika det med ett omvänt snedstreck (\). Till exempel *&quot;Adobe Systems\, Inc.&quot;*.

   `AppVersions` anger klientprogrammets version.

   `Integrators` anger namnet på företaget eller gruppen som utvecklade plugin-programmet eller det integrerade programmet.

   `IntegratorVersions` är den version som plugin-programmet eller det integrerade programmet har.

1. Lägg till ytterligare ett *MyEntryName*-element för varje ytterligare uppsättning med denial-data.
1. Spara konfigurationsfilen.
1. Importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)

**Exempel**

I det här exemplet nekas alla Windows-klienter åtkomst.

```xml
 <node name="ClientVersionRules">
     <map>
         <entry key="infoURL" value="https://www.dont.use/windows.html"/>
     </map>
     <node name="Denials">
         <map/>
         <node name="Entry_1">
             <map>
                 <entry key="SDKPlatforms" value="Microsoft Windows"/>
             </map>
         </node>
     </node>
 </node>
```

I det här exemplet nekas åtkomst till My Application version 3.0 och My Other Application version 2.0. Samma URL för information om avslag används oavsett orsak till nekande.

```xml
 <node name="ClientVersionRules">
     <map>
         <entry key="infoURL" value="https://get.a.new/version.html"/>
     </map>
     <node name="Denials">
         <map/>
         <node name="FirstDenialSettings">
             <map>
                 <entry key="AppNames" value="My Application"/>
                 <entry key="AppVersions" value="3.0"/>
             </map>
         </node>
         <node name="SecondDenialSettings">
             <map>
                 <entry key="AppNames" value="My Other Application"/>
                 <entry key="AppVersions" value="2.0"/>
             </map>
         </node>
     </node>
 </node>
```

I det här exemplet nekas alla begäranden från en Microsoft PowerPoint 2007- eller Microsoft PowerPoint 2010-installation av Acrobat Reader DC-tillägg för Microsoft Office.

```xml
 <node name="ClientVersionRules">
     <map>
         <entry key="infoURL" value="https://get.a.new/version.html"/>
     </map>
     <node name="Denials">
         <map/>
         <node name="Entry_1">
             <map>
                 <entry key="AppFamilies" value=
     "document security Extension for Microsoft Office"/>
                 <entry key="AppNames" value= "Microsoft PowerPoint"/>
                 <entry key="AppVersions" value="2007,2010"/>
             </map>
         </node>
     </node>
 </node
```

### Ändra konfigurationsparametrar för vattenstämpel {#change-the-watermark-configuration-parameters}

Som standard kan du ange högst fem element i en vattenstämpel. Dessutom är den maximala filstorleken för PDF-dokumentet som du vill använda som vattenstämpel begränsad till 100 kB. Du kan ändra de här parametrarna i filen config.xml.

***Obs!**&#x200B;Du bör ändra de här parametrarna med försiktighet.*

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp noden `ServerSettings`.
1. Lägg till följande poster i noden `ServerSettings` och spara sedan filen: `<entry key="maximumSizeOfWatermarkElement" value="max filesize in KB"/> <entry key="maximumWatermarkElementsPerWatermark" value="max elements"/>`

   Den första posten, *maximal filstorlek*, är den maximala filstorleken (i kB) som tillåts för ett PDF-vattenstämpelelement. Standardvärdet är 100 kB.

   Den andra posten, *max elements*, är det maximala antalet element som tillåts i en vattenstämpel. Standardvärdet är 5.

   ```xml
   <entry key="maximumSizeOfWatermarkElement" value="max filesize in KB"/>
   <entry key="maximumWatermarkElementsPerWatermark" value="max elements"/>
   ```

1. Importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)

### Inaktivera externa länkar {#disabling-external-links}

Många dokumentsäkerhetsanvändare har inte åtkomst till externa länkar som **www.adobe.com** när de använder rätt användargränssnitt för hantering:

* `https://[host]:'port'/adminui`
* `https://[host]:'port'/edc`.

Följande ändringar av config.xml inaktiverar alla externa länkar från användargränssnitten för högerhantering.

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp noden `DisplaySettings`.
1. Om du vill inaktivera alla externa länkar lägger du till följande post i noden `DisplaySettings` och sparar sedan filen: `<entry key="ExternalLinksAllowed" value="false"/>`

   ```xml
   <entry key="ExternalLinksAllowed" value="false"/>
   ```

1. Importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)

### Konfiguration för att aktivera SMTP för TLS (Transport Layer Security) {#configuration-to-enable-smtp-for-transport-layer-security-tls}

Följande ändringar av config.xml aktiverar TLS-stöd för funktionen Inbjuden användarregistrering.

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp noden `DisplaySettings`.
1. Hitta följande nod: `<node name="ExternalUser">`

   ```xml
   <node name="ExternalUser">
   ```

1. Ange värdet för nyckeln `SmtpUseTls` i noden `ExternalUser` som **true**.
1. Ange värdet för nyckeln `SmtpUseSsl` i noden `ExternalUser` till **false**.
1. Spara `config.xml`.
1. Importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)

### Inaktivera SOAP-slutpunkter för dokumentsäkerhetsdokument {#disable-soap-endpoints-for-document-security-documents}

Följande ändringar av config.xml för att inaktivera SOAP-slutpunkter för dokumentsäkerhetsdokument.

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp följande nod: `<node name="DRM">`

   ```xml
   <node name="DRM">
   ```

1. Leta reda på noden `entry` i DRM-noden:

   `<entry key="AllowUnencryptedVoucher" value="true"/>`

1. Om du vill inaktivera SOAP-slutpunkter för dokumentsäkerhetsdokument anger du värdeattributet till **false**.

   ```xml
   <node name="DRM">
       <map>
           <entry key="AllowUnencryptedVoucher" value="false"/>
       </map>
   </node>
   ```

1. Spara `config.xml`.
1. Importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)

### Ökad skalbarhet för dokumentsäkerhetsservern {#increasingscalability}

När ett dokument synkroniseras för offlineanvändning, tillsammans med informationen för det aktuella dokumentet, hämtar dokumentsäkerhetsklienterna som standard profiler, vattenstämplar, licenser och spärrning information för alla andra dokument som användaren har åtkomst till. Om uppdateringarna och informationen inte synkroniseras med klienten kan ett dokument som öppnas i offlineläge fortfarande öppnas med äldre princip-, vattenstämpel- och återkallningsinformation.

Du kan öka skalbarheten för dokumentsäkerhetsservern genom att begränsa den information som skickas till klienten. Den minskade mängden information som skickas till klienten resulterar i förbättrad skalbarhet, kortare svarstid och bättre prestanda för servern. Utför följande steg för att öka skalbarheten:

1. Exportera konfigurationsfilen för dokumentsäkerhet. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
1. Öppna konfigurationsfilen i en redigerare och leta upp noden ServerSettings.
1. I noden ServerSettings anger du värdet `true` för egenskapen `DisableGlobalOfflineSynchronizationData`.

   `<entry key="DisableGlobalOfflineSynchronizationData" value="true"/>`

   Om värdet är true skickar dokumentsäkerhetsservern endast information för det aktuella dokumentet och informationen för resten av dokumenten (de andra dokumenten som en användare har åtkomst till) skickas inte till klienten.

   >[!NOTE]
   >
   >Som standard är värdet för tangenten `DisableGlobalOfflineSynchronizationData` inställt på `false`.

1. Spara och importera konfigurationsfilen. (Se [Redigera konfigurationsfilen för dokumentsäkerhet manuellt](/help/forms/using/admin-help/configuring-client-server-options.md#manually-editing-the-document-security-configuration-file).)
