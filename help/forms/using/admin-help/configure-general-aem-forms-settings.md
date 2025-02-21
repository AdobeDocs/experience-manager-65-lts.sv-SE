---
title: Allmänna inställningar för AEM Forms
description: Lär dig att konfigurera inställningarna på sidan Core Configurations i administrationskonsolen som kan förbättra systemets prestanda.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/get_started_with_administering_aem_forms_on_jee
products: SG_EXPERIENCEMANAGER/6.4/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 1444b0fc0811cbb187d2a4d83b626444e44ef73f
workflow-type: tm+mt
source-wordcount: '1768'
ht-degree: 0%

---

# Allmänna inställningar för AEM Forms {#general-aem-forms-settings}

Sidan Core Configurations i administrationskonsolen innehåller inställningar som kan förbättra systemets prestanda. När du har konfigurerat eller uppdaterat inställningarna startar du om programservern.

>[!NOTE]
>
> * Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.
> * Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

Mer information om hur du aktiverar läget för säker säkerhetskopiering finns i [Aktivera och inaktivera läget för säker säkerhetskopiering](/help/forms/using/admin-help/enabling-disabling-safe-backup-mode.md#enabling-and-disabling-safe-backup-mode).


>[!NOTE]
>
>Filerna i den tillfälliga katalogen och de långlivade dokumenten i GDS-rotkatalogen (Global Document Storage) kan innehålla känslig användarinformation, t.ex. information som kräver särskilda autentiseringsuppgifter när de används med API:er eller användargränssnitt. Därför är det viktigt att den här katalogen skyddas på rätt sätt genom att använda de metoder som är tillgängliga för operativsystemet. Vi rekommenderar att endast det operativsystemskonto som används för att köra programservern har läs- och skrivåtkomst till den här katalogen.


1. Välj **[!UICONTROL Settings > Core System Settings > Configurations]** i administrationskonsolen.
1. Ändra alternativen efter behov på sidan Core Configurations och välj **[!UICONTROL OK]**. Mer information om alternativen finns i [Alternativ för kärnkonfigurationer](configure-general-aem-forms-settings.md#core-configurations-options).


## Alternativ för kärnkonfigurationer {#core-configurations-options}

**Plats för tillfällig katalog** Katalogsökvägen där AEM-formulär skapar temporära produktfiler. Om värdet för den här inställningen är tomt används systemets tillfälliga katalog som standard. Kontrollera att den tillfälliga katalogen är en skrivbar mapp.

>[!NOTE]
>
>Kontrollera att den tillfälliga katalogen finns i det lokala filsystemet. AEM-formulär har inte stöd för en tillfällig katalog på en fjärrplats.

**Rotkatalog för global dokumentlagring** *ndash; rotkatalogen för global dokumentlagring (GDS) används i följande syften:

* Lagra långlivade dokument. Långa dokument har ingen förfallotid och finns kvar tills de tas bort (t.ex. de PDF-filer som används i en arbetsflödesprocess). De långvariga dokumenten utgör en kritisk del av det övergripande systemtillståndet. Om några eller alla dessa dokument förloras eller skadas kan Forms Server bli instabil. Därför är det viktigt att den här katalogen lagras på en RAID-enhet.
* Lagra temporära dokument som behövs under bearbetningen.

>[!NOTE]
>
>Du kan även aktivera dokumentlagring i AEM formulärdatabas. Systemprestanda är dock bättre när du använder GDS.

* Överför dokument mellan noder i ett kluster. Om du kör AEM-formulär i en klustermiljö måste katalogen vara tillgänglig från alla noder i klustret.
* Tar emot inkommande parametrar från fjärr-API-anrop.

Om du inte anger någon GDS-rotkatalog används en programserverkatalog som standard:

* `[JBOSS_HOME]/server/<server>/svcnative/DocumentStorage`
* `[WEBSPHERE_HOME]/installedApps/adobe/'server'/DocumentStorage`
* `[WEBLOGIC_HOME]/user_projects/<domain>/'server'/adobe/AEMformsserver/DocumentStorage`

>[!NOTE]
>
>Om du ändrar värdet för GDS-rotkataloginställningen bör du vara särskilt försiktig. GDS-katalogen används för att lagra både långlivade filer som används i en process och viktiga produktkomponenter för AEM-formulär. Att ändra platsen för GDS-katalogen är en stor systemändring. Om GDS-katalogen konfigureras på ett felaktigt sätt kommer AEM-formulären att fungera och AEM-formulären kan behöva installeras om helt. Om du anger en ny plats för GDS-katalogen måste programservern stängas av och data migreras innan servern kan startas om. Systemadministratören måste flytta alla filer från den gamla platsen till den nya, men behålla den interna katalogstrukturen.

>[!NOTE]
>
>Ange inte samma katalog för den tillfälliga katalogen och GDS-katalogen.

Mer information om GDS-katalogen finns i [Installera AEM-formulär (en server)](https://www.adobe.com/go/learn_aemforms_prepareInstallsingle_63).

**Plats för Adobe Server Fonts-katalog** *ndash; Ange sökvägen till den katalog som innehåller Adobe serverteckensnitt. Dessa teckensnitt installeras med AEM-formulär. Standardplatsen för teckensnitten är katalogen [aem-forms root]/fonts. Om den här katalogen inte är tillgänglig kan du kopiera teckensnitten någon annanstans och använda den här inställningen för att ange den nya platsen.

**Plats för kundteckensnittskatalogen** *ndash; Ange sökvägen till en katalog som innehåller ytterligare teckensnitt som du vill använda.

***Obs!**: Teckensnitt hämtas från Windows-systemets teckensnittscache och en systemomstart krävs för att uppdatera cachen. När du har angett kundens teckensnittskatalog måste du starta om datorn där AEM-formulär är installerade.*

**Plats för System Fonts-katalog** *ndash; Ange sökvägen till teckensnittskatalogen som operativsystemet tillhandahåller. Flera kataloger kan läggas till, avgränsade med semikolon **;**.

**Plats för Data Services-konfigurationsfilen** *ndash; Anger platsen för services-config.xml-filen. Som standard är den här filen inbäddad i filen adobe-core-appserver.ear och är inte användartillgänglig. En kopia av standardfilen services-config.xml finns i [aem-forms root]\sdk\misc\DataServices\Server-Configuration. Om du har ändrat den här filen och flyttat den anger du den nya platsen i det här fältet.

Konfigurationsfilen för Data Services gör det möjligt att anpassa datatjänster, till exempel autentiseringstyp och felsökningsutdata.

Den här inställningen är tom som standard.

**Maximal intern dokumentstorlek (byte)** *ndash; Maximalt antal byte som finns i minnet när dokument skickas mellan olika AEM-formulärkomponenter. Använd den här inställningen för prestandajustering. Dokument som är mindre än det här antalet lagras i minnet och sparas i databasen. Dokument som överskrider det högsta tillåtna antalet lagras på hårddisken.

Den här inställningen är obligatorisk. Standardvärdet är 65 536 byte.

**Standardtidsgräns för borttagning av dokument (sekunder)** *ndash; Den maximala tiden (i sekunder) under vilken ett dokument som skickas mellan olika AEM-formulärkomponenter anses vara aktiv. När den här tiden har gått kan filer som används för att lagra det här dokumentet tas bort. Använd den här inställningen för att kontrollera hur mycket diskutrymme som används.

Den här inställningen är obligatorisk. Standardvärdet är 600 sekunder.

**Intervall för dokumentsvepning (sekunder)** *ndash; Den tid, i sekunder, mellan försök att ta bort filer som inte längre behövs och som användes för att skicka dokumentdata mellan tjänster.

Den här inställningen är obligatorisk. Standardvärdet är 30 sekunder.

**Aktivera FIPS** *ndash; välj det här alternativet om du vill aktivera FIPS-läget. Federal Information Processing Standard (FIPS) 140-2 är en kryptologistandard som definierats av USA:s regering. När AEM-formulär körs i FIPS-läge begränsar de dataskyddet till godkända FIPS 140-2-algoritmer genom att använda krypteringsmodulen RSA BSAFE Crypto-C 2.1.

FIPS-läget stöder inte krypteringsalgoritmer som används i tidigare Adobe Acrobat®-versioner än 7.0. Om FIPS-läget är aktiverat och du använder krypteringstjänsten för att kryptera PDF med ett lösenord med kompatibilitetsnivån inställd på Acrobat 5, misslyckas krypteringsförsöket med ett fel.

När FIPS är aktiverat används vanligtvis inte lösenordskryptering på något dokument. Om du försöker göra det genereras ett FIPSModeException-undantag som anger att lösenordskryptering inte tillåts i FIPS-läge. Dessutom stöds inte elementet PDFsFromBookmarks (DX) i dokumentbeskrivningens XML-element i FIPS-läge när basdokumentet är lösenordskrypterat.

>[!NOTE]
>
>AEM formulärprogram validerar inte koden för att säkerställa FIPS-kompatibiliteten. Den tillhandahåller ett FIPS-driftläge så att FIPS-godkända algoritmer används för kryptografiska tjänster från FIPS-godkända bibliotek (RSA).

**Aktivera WSDL** *ndash; Välj det här alternativet om du vill aktivera WSDL-generering (Web Service Definition Language) för alla tjänster som ingår i AEM-formulär.

Aktivera det här alternativet i utvecklingsmiljöer, där utvecklare använder WSDL-generering för att skapa sina klientprogram. Du kan välja att inaktivera WSDL-generering i en produktionsmiljö för att undvika att visa tjänstens interna information.

**Aktivera dokumentlagring i databasen** *ndash; välj det här alternativet om du vill lagra långlivade dokument i AEM formulärdatabas. Om du aktiverar det här alternativet tas inte behovet av en GDS-katalog bort. Om du väljer det här alternativet förenklas dock säkerhetskopieringen av AEM-formulär. Om du bara använder GDS-systemet innebär en säkerhetskopiering att du placerar AEM-formulärsystemet AEM-formulärsystemet i säkerhetskopieringsläge och sedan slutför säkerhetskopieringen av databasen och GDS-systemet. Om du väljer databasalternativet innebär säkerhetskopieringen att du slutför databassäkerhetskopieringen för en ny installation eller slutför databassäkerhetskopieringen och engångskopieringen av GDS för en uppgradering. Ytterligare hantering av databasen kan behövas för att rensa jobb och data jämfört med en GDS-konfiguration. (Se Alternativ för säkerhetskopiering när databasen används för dokumentlagring.)

**Aktivera DSC-anropsstatistik** *ndash. När det här alternativet är markerat spårar AEM-formulär startstatistik som antal anrop, hur lång tid det tar att anropa och antalet fel i anrop. Denna information lagras i en JMX-böna så att du kan använda Java™ JConsole eller tredjepartsprogram för att ta en titt på statistiken. Om du inte vill se den här statistiken avmarkerar du det här alternativet för att förbättra prestanda för AEM-formulär.

**Aktivera RDS** *ndash. Om du väljer det här alternativet aktiveras RDS-servern (Remote Development Services) i AEM-formulär. När det här alternativet är aktiverat kan verktyg på klientsidan interagera med datatjänster för att göra saker som att driftsätta eller avdistribuera modeller för att skapa destinationer och slutpunkter, eller för att ta reda på vilka modeller som har distribuerats till slutpunkter. Som standard är det här alternativet inte markerat.

**Tillåt oskyddad RDS-begäran** *ndash. När det här alternativet är markerat behöver RDS-begäranden inte använda https. Som standard är det här alternativet inte markerat och all kommunikation till datatjänster måste vara https-begäranden.

**Tillåt oskyddad dokumentöverföring från Flex-program** *ndash. Filöverföringsservern som används för att överföra dokument från Adobe Flex®-program till AEM-formulär kräver att användare är autentiserade och auktoriserade innan de kan överföra dokument. Användaren måste tilldelas rollen som användare av Document Upload Application eller en annan roll som innefattar behörigheten Dokumentöverföring. Detta förhindrar att obehöriga laddar upp dokument till AEM Forms Server. Välj det här alternativet om du vill inaktivera den här säkerhetsfunktionen i en utvecklingsmiljö eller för bakåtkompatibilitet med tidigare versioner av AEM-formulär. Som standard är det här alternativet inte markerat. Mer information finns i&quot;Anropa AEM-formulär med AEM Forms Remoting&quot; i Programmering med AEM-formulär.

**Tillåt oskyddad dokumentöverföring från Java SDK-program** *ndash; HTTP DocumentManager-överföringar måste vara skyddade. Som standard kräver HTTP-överföringar att användare autentiseras och auktoriseras innan de kan överföra dokument. Användaren måste tilldelas rollen Tjänstanvändare eller en annan roll som innehåller behörigheten Tjänstanrop. Detta förhindrar att obehöriga laddar upp dokument till Forms Server. Välj det här alternativet om du vill inaktivera den här säkerhetsfunktionen i en utvecklingsmiljö, för bakåtkompatibilitet med tidigare versioner av AEM-formulär eller baserat på din brandvägg. Som standard är det här alternativet inte markerat. Mer information finns i&quot;Anropa AEM-formulär med Java API&quot; i Programmering med AEM-formulär.
