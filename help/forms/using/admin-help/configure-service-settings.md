---
title: Konfigurera tjänstinställningar
description: Lär dig hur du konfigurerar tjänstinställningar. Du kan använda sidan Tjänsthantering för att konfigurera inställningarna för var och en av de tjänster som ingår i AEM-formulär.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_services
products: SG_EXPERIENCEMANAGER/6.4/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Workbench
role: User, Developer
hide: true
hidefromtoc: true
exl-id: a2586a1e-0e7f-4ea4-87ec-fbd82df3ec4c
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '10836'
ht-degree: 0%

---

# Konfigurera tjänstinställningar {#configure-service-settings}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan använda sidan Tjänsthantering för att konfigurera inställningar för var och en av de tjänster som ingår i AEM-formulär. De tillgängliga inställningarna varierar beroende på vilken tjänst som konfigureras.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. Stoppa tjänsten innan du ändrar den. (Se [Starta och stoppa tjänster](/help/forms/using/admin-help/starting-stopping-services.md#starting-and-stopping-services).)
1. Klicka på namnet på den tjänst som du vill konfigurera.
1. Om tjänsten har en konfigurationsflik använder du den för att ändra inställningarna för tjänsten. Se listan med länkar nedan för mer information.

   >[!NOTE]
   >
   >Alla tjänster som listas på sidan Tjänsthantering har inte fliken Konfiguration. För processer som du har skapat visas bara fliken Konfiguration om du har lagt till en konfigurationsparameter till processen i Workbench. (Se&quot;Konfigurationsparametrar&quot; i [Workbench-hjälpen](https://www.adobe.com/go/learn_aemforms_workbench_63) .)


1. Klicka på fliken Säkerhet och ange säkerhetsinställningarna för tjänsten. Se [Ändra säkerhetsinställningar för en tjänst](configure-service-settings.md#modifying-security-settings-for-a-service).
1. Om tjänsten har en fliken Slutpunkter använder du den för att ändra slutpunktsinställningarna. Se [Hantera slutpunkter](/help/forms/using/admin-help/adding-enabling-modifying-or-removing.md).
1. Klicka på fliken Pooling och ange poolinställningar. Se [Konfigurera poolning för en tjänst](configure-service-settings.md#configuring-pooling-for-a-service).
1. Klicka på Spara om du vill spara ändringarna eller klicka på Avbryt om du vill ignorera dem.
1. Markera kryssrutan bredvid tjänstnamnet och klicka på Starta för att starta om tjänsten.

## Granska inställningar för arbetsflödestjänst {#audit-workflow-service-settings}

Workbench ger möjlighet att spela in processinstanser när de körs under körning och sedan spela upp dem för att observera processens beteende. (Se [Workbench-hjälpen](https://www.adobe.com/go/learn_aemforms_workbench_63).) Om du vill spara utrymme i filsystemet på Forms Server kan du begränsa mängden lagrade processinspelningsdata. Du kan konfigurera följande egenskaper för tjänsten Granskningsarbetsflöde ( `AuditWorkflowService`):

**maxNumberOfRecordingInstances:** Det maximala antalet inspelningar som lagras. När det högsta antalet lagras tas den äldsta inspelningen bort från filsystemet när en ny inspelning skapas. Den här egenskapen är användbar om du tenderar att skapa många inspelningar och vill ta bort gamla inspelningar automatiskt. Standardvärdet är 50.

**MaxNumberOfRecordingPoster:** Det maximala antalet dataposter som kan lagras för varje inspelning. Dataposter är information om åtgärder som utförs i processen. Flera poster lagras för varje körning av en åtgärd, till exempel om åtgärden har startats, om åtgärden har slutförts och om den väg som leder till åtgärden är slutförd. Den här egenskapen är användbar när processer kan innehålla många körningar av åtgärder, till exempel när en oändlig slinga påträffas. Standardvärdet är 50.

## streckkodsinställningar för formulärtjänst {#barcoded-forms-service-settings}

Streckkodad formulärtjänst `(BarcodedFormsService)` extraherar streckkodsdata från skannade bilder. Tjänsten accepterar ett streckkodsformulär (TIFF eller PDF) som indata och extraherar maskinvaruåtergivningen av data som kodats av streckkoden.

Följande inställningar är tillgängliga för den streckkodade formulärtjänsten.

**Läs åt vänster:** När du väljer det här alternativet skannas streckkodsbilder vågrätt från höger till vänster.

**Läs åt höger:** När du väljer det här alternativet skannas streckkodsbilder vågrätt från vänster till höger.

**Uppläsning:** När du väljer det här alternativet skannas streckkodsbilder lodrätt nedifrån och upp.

**Nedläsning:** När du väljer det här alternativet skannas streckkodsbilder lodrätt uppifrån och ned.

>[!NOTE]
>
>Som standard är alla alternativ markerade. Avmarkera bara ett alternativ om du är säker på att inga streckkoder visas på det sättet i formulären.

**Bas filsökväg:** Den filsökväg som är relativ till vilka parametrarna för batchindata och utdatafiler för åtgärderna Kör XML-filjobb och Kör platta filjobb tolkas. I klustrade konfigurationer måste sökvägen till basfilen vara en delad filsystemplats som alla klusternoder har läs-/skrivåtkomst till.

**Data-Source-namn:** Namnet på datakällan som används för att underhålla status- och historikinformation om batchbearbetningsjobb. Den angivna datakällan måste ha stöd för globala (XA) transaktioner.

## Inställningar för Bridge-tjänsten för central migrering (inaktuell) {#central-migration-bridge-service-settings}

Bridge-tjänsten för central migrering ( `CentralMigrationBridge`) anropar en delmängd av funktionerna i Adobe Central Pro Output Server (Central), som innehåller kommandona JFMERGE, JFTRANS och XMLIMPORT. Med Bridge-tjänståtgärder för central migrering kan du återanvända följande centrala resurser i AEM-formulär:

* malldesign (&amp;ast;.ifd)
* utdatamallar (&amp;ast;.mdf)
* datafiler (&amp;ast;.dat-filer)
* inledningsfiler (&amp;ast;.pre-filer)
* datadefinitionsfiler (&amp;ast;.tdf)

Följande inställning är tillgänglig för Bridge-tjänsten för central migrering.

**Katalog för central installation:** Katalogen där Adobe Central 5.7 är installerat.

## Content Repository Connector for EMC Documentum-tjänstinställningar {#content-repository-connector-for-emc-documentum-service-settings}

Med Content Repository Connector for EMC Documentum-tjänsten ( `EMCDocumentumContentRepositoryConnector`) kan du skapa processer som interagerar med innehåll som lagras i en Documentum-databas.

Följande inställning är tillgänglig för tjänsten Content Repository Connector for EMC Documentum.

**Standardsökväg för resurslänksobjekt:** Standarddelen av sökvägen i Documentum-databasen för lagring av resurslänksobjektet. Den faktiska sökvägen består av standardsökvägen och platsen för formulärmallen i AEM formulärdatabas.

Om standardsökvägen till exempel är `/LiveCycleES/ConnectorforEMCDocumentum/AssetLinkObjects` och formulärmallen lagras i en mapp `/Docbase/forms/`, lagras resurslänksobjektet på följande plats:

`/LiveCycleES/ConnectorforEMCDocumentum/AssetLinkObjects/Docbase/forms/`

Standardvärdet för inställningen är `/LiveCycleES/ConnectorforEMCDocumentum/AssetLinkObjects`.

## Content Repository Connector for IBM FileNet service settings {#content-repository-connector-for-ibm-filenet-service-settings}

Med Content Repository Connector för IBM FileNet kan du skapa processer som interagerar med innehåll som lagras i en IBM FileNet-databas.

Följande inställning är tillgänglig för tjänsten Content Repository Connector för IBM FileNet.

**Standardsökväg för resurslänksobjekt:** Standarddelen av sökvägen i IBM FileNet-databasen för lagring av resurslänksobjektet. Den faktiska sökvägen består av standardsökvägen och platsen för formulärmallen i AEM formulärdatabas.

Om standardsökvägen till exempel är `/LiveCycleES/ConnectorforIBMFileNet/AssetLinkObjects` och formulärmallen lagras i en mapp `/Docbase/forms/`, lagras resurslänksobjektet på följande plats:

`/LiveCycleES/ConnectorforIBMFileNet/AssetLinkObjects/Docbase/forms/`

Standardvärdet för inställningen är `/LiveCycleES/ConnectorforIBMFileNet/AssetLinkObjects`.

## Konvertera tjänstinställningar för PDF {#convert-pdf-service-settings}

Med tjänsten Konvertera PDF ( `ConvertPdfService`) kan du konvertera PDF-dokument till PostScript och flera bildformat (JPEG, JPEG 2000, PNG och TIFF). Att konvertera ett PDF-dokument till PostScript är användbart för oövervakad serverbaserad utskrift på alla PostScript-skrivare. Att konvertera ett PDF-dokument till en flersidig TIFF-fil är praktiskt när man arkiverar dokument i content management-system som saknar stöd för PDF-dokument.

Följande inställningar är tillgängliga för tjänsten Convert PDF.

**Transaktionstyp:** Anger hur en transaktionskontext ska spridas till en åtgärd.

**Obligatoriskt:** Stöder en transaktionskontext om en sådan finns, annars skapas en ny transaktionskontext. Det här är standardvärdet.

**Kräver nytt:** Skapar alltid en ny transaktionskontext. Om det finns en aktiv transaktionskontext pausas den.

**Överföringstid (i sek):** Antalet sekunder som den underliggande transaktionsprovidern ska vänta innan en transaktion som omsluter den här åtgärden återställs. Det här värdet ignoreras om en befintlig transaktionskontext sprids. Standardvärdet är 180.

**Tröskelupplösning för utjämning (i dpi):** Den bildupplösning under vilken utjämning (eller kantutjämning) tillämpas på text, teckningar och bilder, om du har valt alternativen &quot;Tillämpa utjämning på&quot; för dessa element.

**Använd utjämning på text:** Styr kantutjämning av text. Avmarkera den här kryssrutan om du vill inaktivera utjämning av text och göra texten skarpare och lättare att läsa med en skärmförstorare.

**Använd utjämning på LineArt:** Använder utjämning för att ta bort plötsliga vinklar i linjer.

**Använd utjämning på bilder:** Använder utjämning för att minimera abrupta bildändringar.

## Distiller tjänstinställningar {#distiller-service-settings}

Distiller-tjänsten ( `DistillerService`) konverterar PostScript-, Encapsulated PostScript- (EPS) och PRN-filer till PDF-filer via ett nätverk.

Följande inställningar är tillgängliga för tjänsten Distiller.

**Adobe PDF-inställningar:** Följande förkonfigurerade inställningar används för den genererade PDF:

* Högkvalitativ utskrift
* Stora sidor
* PDFA1b 2005 CMYK
* PDFA1b 2005 RGB
* PDFX1a 2001
* PDFX3 2002
* Tryckkvalitet
* Minsta filstorlek
* Standard

Nya inställningar kan skapas via PDF Generator användargränssnitt.

**Säkerhetsinställningar:** Förkonfigurerade säkerhetsinställningar som används för genererade PDF-dokument. Standardvärdet är Ingen säkerhet. Skapa skyddsinställningar med PDF Generator och ange sedan inställningen här.

**Poolstorlek:** Poolens ursprungliga storlek. När Distiller-tjänsten distribueras används det här numret för att avgöra hur många instanser av tjänstimplementering som skapas och tilldelas den kostnadsfria poolen som väntar på anrop. Tjänstbehållaren kan sedan svara direkt på anropsbegäranden utan att först initiera en tjänstinstans.

## Inställningar för dokumenthanteringstjänsten {#document-management-service-settings}

>[!NOTE]
>
>Adobe® LiveCycle® Content Services ES (utgått) är ett innehållshanteringssystem som installeras med LiveCycle. Det gör det möjligt för användarna att utforma, hantera, övervaka och optimera humancentrerade processer. Supporten för innehållstjänster (borttaget) upphör 2014-12-31. Se [Adobe livscykeldokument](https://www.adobe.com/support/products/enterprise/eol/eol_matrix.html).

Tjänsten Document Management ( `DocumentManagementService`) gör det möjligt för processer att använda innehållshanteringsfunktionen som tillhandahålls av Content Services (Borttagen). Dokumenthanteringsåtgärder innehåller grundläggande uppgifter som krävs för att underhålla utrymme och innehåll i innehållshanteringssystemet. Exempel på sådana uppgifter är kopiera, ta bort, flytta, hämta och lagra innehåll, skapa blanksteg och associationer samt hämta och ange innehållsattribut.

Följande inställningar är tillgängliga för dokumenthanteringstjänsten.

**Butiksschema:** Schemat för den butik där innehållet finns. Standardvärdet är arbetsyta.

**HTTP-port:** Porten som används för åtkomst till innehållstjänster (borttagen). Standardvärdet är 8080.

## Inställningar för e-posttjänst {#email-service-settings}

E-post används ofta för att distribuera innehåll eller tillhandahålla statusinformation som en del av en automatiserad process. Med e-posttjänsten ( `EmailService`) kan processer ta emot e-postmeddelanden från en POP3- eller IMAP-server och skicka e-postmeddelanden till en SMTP-server.

En process använder till exempel e-posttjänsten för att skicka ett e-postmeddelande med en bifogad PDF-formulärfil. E-posttjänsten ansluter till en SMTP-server för att skicka e-postmeddelandet med den bifogade filen. PDF-formuläret är utformat så att mottagaren kan klicka på Skicka när formuläret har fyllts i. Åtgärden gör att formuläret returneras som en bifogad fil till den angivna e-postservern. E-posttjänsten hämtar det returnerade e-postmeddelandet och lagrar det ifyllda formuläret i en processdataformulärvariabel.

Följande inställningar är tillgängliga för e-posttjänsten.

**SMTP-värd:** IP-adressen eller URL:en för SMTP-servern som ska användas för att skicka e-post.

**SMTP-portnummer:** Porten som används för att ansluta till SMTP-servern.

**SMTP-autentisering:** Välj om användarautentisering krävs för att ansluta till SMTP-servern.

**SMTP-användare:** Användarnamnet för det användarkonto som ska användas för att logga in på SMTP-servern.

**SMTP-lösenord:** Lösenordet som är associerat med SMTP-användarkontot.

**SMTP-transportsäkerhet:** Säkerhetsprotokollet som ska användas för att ansluta till SMTP-servern:

* Välj Ingen om inget protokoll används (data skickas i klartext)
* Välj SSL om Secure Sockets Layer-protokollet används.
* Välj TLS om transportlagersäkerhet används.

**POP3/IMAP-värd:** IP-adressen eller URL-adressen för POP3- eller IMAP-servern som ska användas för att skicka e-post.

**POP3/IMAP-användarnamn:** Användarnamnet för det användarkonto som ska användas för att logga in på POP3- eller IMAP-servern.

**POP3/IMAP-lösenord:** Lösenordet som är associerat med POP3- eller IMAP-användarkontot.

**POP3/IMAP-portnummer:** Porten som används för att ansluta till POP3- eller IMAP-servern.

**POP3/IMAP:** Det protokoll som ska användas för att skicka och ta emot e-post.

**Ta emot transportsäkerhet:** Säkerhetsprotokollet som ska användas för att ansluta till SMTP-servern:

* Välj Ingen om inget protokoll används (data skickas i klartext).
* Välj SSL om Secure Sockets Layer-protokollet används.
* Välj TLS om transportlagersäkerhet används.

## Inställningar för krypteringstjänst {#encryption-service-settings}

Med krypteringstjänsten ( `EncryptionService`) kan du kryptera och dekryptera dokument. När ett dokument är krypterat blir innehållet oläsligt. En behörig användare kan dekryptera dokumentet för att få åtkomst till innehållet. Om ett PDF-dokument är krypterat med ett lösenord måste användaren ange det öppna lösenordet innan dokumentet kan visas i Adobe Reader eller Adobe Acrobat. Om ett PDF-dokument är krypterat med ett certifikat måste användaren dekryptera PDF-dokumentet med den offentliga nyckel som motsvarar det certifikat (privat nyckel) som användes för att kryptera PDF-dokumentet.

Följande inställningar är tillgängliga för krypteringstjänsten.

**LDAP-standardserver som ska anslutas till:** Värdnamn för LDAP-servern som används för att hämta certifikat för dokumentkryptering.

**LDAP-standardport att ansluta till:** LDAP-serverns portnummer.

**LDAP-standardanvändarnamn:** Om LDAP-servern kräver autentisering anger du det användarnamn som ska användas för att ansluta till LDAP-servern.

**LDAP-standardlösenord:** Om LDAP-servern kräver autentisering anger du lösenordet som motsvarar användarnamnet som ska användas för att ansluta till LDAP-servern.

>[!NOTE]
>
>Använd endast enkel autentisering (användarnamn och lösenord) när anslutningen skyddas via SSL (med LDAPS).

<!-- **Compatibility Mode:**-->

## FTP-tjänstinställningar {#ftp-service-settings}

FTP-tjänsten ( `FTP`) gör att processer kan interagera med en FTP-server. FTP-tjänståtgärder kan hämta filer från FTP-servern, skicka filer på FTP-servern och ta bort filer från FTP-servern. Till exempel kan dokument som genereras från en process lagras på en FTP-server för distribution. Eller så kan ett externt system generera filer baserat på tidigare steg i en process. I ett efterföljande steg i processen kan filerna överföras till en fjärrplats.

Följande inställningar är tillgängliga för FTP-tjänsten.

**Standardvärd:** FTP-serverns IP-adress eller URL.

**Standardport:** Den port som används för att ansluta till FTP-servern. Standardvärdet är 21.

**Standardanvändarnamn:** Namnet på användarkontot som du kan använda för att komma åt FTP-servern. Användarkontot måste ha tillräcklig behörighet för att utföra de FTP-åtgärder som krävs för den här tjänsten.

**Standardlösenord:** Lösenordet som ska användas med det angivna användarnamnet för autentisering med FTP-servern.

## Generera inställningar för PDF-tjänster {#generate-pdf-service-settings}

Tjänsten Generate PDF ( `GeneratePDFService`) konverterar filer i olika format till PDF-dokument och konverterar PDF-dokument till flera filformat.

Följande inställningar är tillgängliga för tjänsten Generera PDF.

**Adobe PDF-inställningar:** Namnet på de förkonfigurerade Adobe PDF-inställningarna som ska tillämpas på ett konverteringsjobb, om dessa inställningar inte anges som en del av API-anropsparametrarna. Adobe PDF-inställningarna konfigureras i administrationskonsolen genom att klicka på Tjänster > PDF Generator > Adobe PDF-inställningar. De här inställningarna gäller endast för PDFMaker-baserade konverteringar.

**Säkerhetsinställningar:** Namnet på de förkonfigurerade säkerhetsinställningarna som ska tillämpas på ett konverteringsjobb, om dessa inställningar inte anges som en del av API-anropsparametrarna. Säkerhetsinställningarna konfigureras i administrationskonsolen genom att klicka på Tjänster > PDF Generator > Säkerhetsinställningar.

**Filtypsinställningar:** Namnet på den förkonfigurerade filtypsinställningen som ska användas för ett konverteringsjobb, om dessa inställningar inte anges som en del av API-anropsparametrarna. Filtypsinställningarna konfigureras i administrationskonsolen genom att klicka på Tjänster > PDF Generator > Filtypsinställningar.

**Använd WebCapture (endast Windows):** När den här inställningen är true använder tjänsten Generate PDF Acrobat för alla konverteringar från HTML till PDF. Detta kan förbättra kvaliteten på de PDF-filer som skapas från HTML, men prestandan kan vara något lägre. Standardvärdet är false.

**Primär konverterare för konverteringar från HTML till PDF:** Tjänsten Generate PDF erbjuder flera vägar för konvertering av HTML-filer till PDF-dokument: Webkit, WebCapture (endast Windows) och WebToPDF. Med den här inställningen kan användaren välja den primära konverteraren för att konvertera HTML till PDF. Som standard är WebToPDF markerad.

**Återställningskonverterare för konverteringar från HTML till PDF:** Ange konverteraren för konverteringar från HTML till PDF om den primära konverteraren misslyckas. Som standard är WebCapture (endast Windows) markerad.

**Använd Acrobat Image Conversion (endast Windows):** När den här inställningen är true använder tjänsten Generate PDF Acrobat för alla Image to PDF-konverteringar. Den här inställningen är bara användbar om standardkonverteringsfunktionen för ren Java inte kan konvertera en stor del av indatabilderna korrekt. Standardvärdet är false.

**Aktivera Acrobat-baserade AutoCAD-konverteringar (endast Windows):** När den här inställningen är true använder tjänsten Generate PDF Acrobat för alla DWG till PDF-konverteringar. Den här inställningen är bara användbar om AutoCAD inte är installerat på servern eller om AutoCAD-konverteringsfunktionen inte kan konvertera filer.

**Reguljära uttryck för att hitta förbjudna specialuttryck
Tecken i användarnamn (endast Windows):** Anger tecken som stör Export PDF- och Optimera PDF-åtgärder när tecknen visas i en användares namn.

**Poolstorlek för ImageToPDF:** Poolstorleken för standardkonverteraren (ren Java) Image-to-PDF i tjänsten Generate PDF. Den här inställningen styr de maximala samtidiga Image-to-PDF-konverteringar som tjänsten Generate PDF kan utföra. Standardvärdet för den här inställningen (rekommenderas för enprocessorsystem) är 3, som du kan öka på flerprocessorsystem.

**Poolstorlek för HTML till PDF:** Poolstorleken för HTML till PDF-konverteraren i tjänsten Generate PDF. Den här inställningen styr det maximala antalet samtidiga HTML-till-PDF-konverteringar som tjänsten Generate PDF kan utföra. Standardvärdet för den här inställningen (rekommenderas för enprocessorsystem) är 3, som du kan öka på flerprocessorsystem.

**OCR-poolstorlek:** Poolstorleken för den PaperCaptureService som PDF Generator använder för OCR. Standardvärdet för den här inställningen (rekommenderas för enprocessorsystem) är 3, som du kan öka på flerprocessorsystem. Den här inställningen är endast giltig i Windows-system.

**ImageToPDF max pages in memory for TIFF conversions:** Den här inställningen avgör det maximala antalet sidor från en TIFF-bild som kan finnas kvar i minnet innan de tömts på disk under konverteringen till PDF. Standardvärdet för den här inställningen är 500, vilket kan ökas om ytterligare minne tilldelas ImageToPDF-konverteringsprocessen.

**Reservteckensnittsfamilj för HTML till PDF-konverteringar:** Namnet på teckensnittsfamiljen som ska användas i PDF-dokument när teckensnittet som användes i den ursprungliga HTML inte är tillgängligt för AEM Forms Server. Ange en teckensnittsfamilj om du förväntar dig att konvertera HTML-sidor som använder teckensnitt som inte är tillgängliga. På sidor som skapats på regionala språk kan t.ex. otillgängliga teckensnitt användas.

**Återförsökslogik för systemspecifika konverteringar** styr PDF-genereringsförsök om det första konverteringsförsöket har misslyckats:

* **Inget nytt försök**

  Försök inte konvertera PDF igen om det första konverteringsförsöket har misslyckats

* **Försök igen**

  Försök konvertera PDF igen oavsett om timeout-tröskelvärdet har uppnåtts eller inte. Standardtidsgränsen för det första försöket är 270-tal.

* **Försök igen om tiden tillåter**

  Försök konvertera PDF igen om den tid som förbrukats för det första konverteringsförsöket var kortare än den angivna tidsgränsen. Om tidsgränsen till exempel är 270 och det första försöket är 200-tal försöker PDF Generator konvertera igen. Om det första försöket självt förbrukade 270-talet kommer konverteringen inte att försökas igen.

## Guides ES4 Utilities service settings {#guides-es4-utilities-service-settings}

När du skapar en stödlinje bäddas vissa resurser, till exempel en definition av stödlinjen, in i stödlinjen. Resurser kan också finnas som referenser till programresurser som lagras lokalt eller på AEM Forms Server. Handboken innehåller inga data, och värdena för sändningsplatsen och indata passar inte för alla externa miljöer.

I de flesta fall räcker standardåtergivningstjänsterna för stödlinjer för att förbereda en guide för användning i Workspace eller andra externa miljöer. (I vyn Tjänster i Workbench är standardtjänsten Guides (system)/Processes/Render Guide - 1.0.) Med tjänsten Guide Utilities ( `GuidesUtility`) kan du skapa en anpassad process för återgivning av en guide, om det behövs.

Med guideverktygen kan du lägga till följande guideåtergivningsåtgärder i en process:

* Avgör om det finns data att fylla i guiden med
* Bädda in stödlinjedata eller konvertera dem till en länk
* Konvertera refererat innehåll till URL:er som är externt tillgängliga
* Ersätt värden i ett HTML-dokument eller en annan wrapper, eller konvertera dem till externt tillgängliga URL:er
* Ange sändningsplats
* Ange indatavärden
* Skapa en parameter som representerar refererat innehåll
* Om det finns tillgängliga variationer anger du en variant

Standardvärdena för tjänsten Guide Utilities har stöd för de flesta fall. Om det behövs kan du dock ändra följande värden.

**publicPaths:** Det här alternativet har tagits bort. Använd inte det här alternativet för AEM-formulär.

**pathInfoExpiryInSeconds:** Intervallet efter vilket en begäran om sökvägsinformation från en klient upphör att gälla. Standardvärdet är 1.

**ateralExpiryInSeconds:** Intervallet efter vilket en begäran om säkerhet från en klient upphör att gälla. Standardvärdet är 315576000.

**mismatchExpiryInSeconds:** Intervallet efter vilket en begäran om säkerhet från en klient upphör att gälla när eTag (enhetstagg) inte matchar. (En e-tagg är en HTTP-svarshuvud.) Standardvärdet är 1.

**guideContext:** The context root of the Guides web application. Matchar det värde som angetts med webbprogrammet för stödlinjer. Standardvärdet är /Guides/.

**secureRandomAlgorithm:** Den algoritm som ska användas när nycklar och identifierare genereras. Det här värdet skickas till metoden getInstance i klassen SecureRandom Java. Standardvärdet är SHA1PRNG.

**idBytes:** Antalet slumpmässiga byte som ska användas för en nyckelidentifierare. Standardvärdet är 6.

**macAlgorithm:** Den MAC-algoritm (meddelandelegitimeringskod) som ska användas för att verifiera ingående URL-adresser. Den här metoden skickas till metoden getInstance i klassen Mac. Standardvärdet är HmacSHA1.

**macRefreshIntervalInMinutes:** Den tid en nyckel är aktiv. När en nyckel har varit aktiv för det här intervallet genereras en ny nyckel. Den nya tangenten blir aktiv. Den tidigare aktiva nyckeln behålls för 10 % av uppdateringsintervallet. Detta beteende gör att URL:er som genereras med den gamla nyckeln kan fortsätta att fungera i hela nyckelbytet. Standardvärdet är 144000.

**macOverlapIntervalInMinutes:** Den tid som föregående nyckel ska förbli giltig efter att en ny har genererats. Standardvärdet är 1 440 minuter (1 dag).

**macKeySeed:** Ett dirigeringsvärde för generering av den säkra URL:en. När det här alternativet är valt uppdateras aldrig nyckeln. Om du anger samma startvärde på olika servrar genereras säkra URL:er som är kompatibla. Detta kan vara användbart om flera formulärservrar används bakom en belastningsutjämnare. Ange en slumpmässig sekvens med tecken och siffror som startvärde.

### Använda stödlinjer i ett serverkluster {#using-guides-in-a-server-cluster}

Rendering a Guide in a server Cluster that does not use sticky sessions fails with a NullPointerException. En begäran om stödlinjer använder säkra URL:er som som standard är unika för den server som de genereras på. I ett kluster som använder kladdiga sessioner dirigeras alla efterföljande begäranden för den sessionen eller användaren enbart till servern efter att en begäran träffar en nod i klustret, och allt är ok. I ett kluster som inte använder klibbiga sessioner kan efterföljande begäranden påverka alla servrar i klustret. Om servern som begäran träffar på inte är den ursprungliga servern, kan de inte matcha den säkra URL:en.

Om du använder stödlinjer i ett serverkluster som inte använder kladdiga sessioner, anger du värdet macKeySeed för tjänsten GuidesUtility och stoppar och startar sedan klustret.

Värdet macKeySeed är startvärdet för den slumpmässiga talgeneratorn som används för att generera säkra URL:er. Om du anger det här värdet initierar varje klusternod den slumpmässiga talgeneratorn på samma sätt och får tillgång till samma säkra URL:er. Du kan använda valfri slumpmässig sträng för detta dirigerade värde.

Ändra värdet för macKeySeed när du behöver uppdatera säkra URL:er. Uppdatering av säkra URL:er beror på din säkerhetsprincip och liknar uppdateringsprincipen för ändring av huvudrotlösenordet för servern. macSeedValue är detsamma som huvudlösenordet för säkra URL:er, eftersom det används för att generera ett nytt unikt slumpmässigt nummer som kan användas för säker URL-generering och hämtning.

Starta om klustret eftersom macSeedValue är skrivskyddat när systemet startas. Alla noder måste startas om för att kunna läsa värdet, eftersom de använder det oberoende av varandra för att initiera sina interna slumptal med startvärdet.

## JDBC-tjänstinställningar {#jdbc-service-settings}

JDBC-tjänsten ( `JdbcService`) gör det möjligt för processer att interagera med databaser.

Följande inställning är tillgänglig för JDBC-tjänsten.

**datakällans namn:** Ett strängvärde som representerar JNDI-namnet på datakällan som ska användas för att ansluta till databasservern. Datakällan måste definieras på den programserver som är värd för Forms Server. Standardvärdet är JNDI-namnet på datakällan för AEM-formulärdatabasen.

## JMS-tjänstinställningar {#jms-service-settings}

JMS-tjänsten ( `JMS`) aktiverar interaktion med Java Messaging System-providers (JMS) som implementerar både peka-till-punkt-meddelanden och publicera/prenumerera-meddelanden.

Konfigurera JMS-tjänsten med standardegenskaper så att tjänståtgärderna kan ansluta till och interagera med en JMS-provider och en associerad JNDI-tjänst. Värdena för tjänsteegenskaperna ställs in på standardvärden baserat på JBoss Application Server. Ändra dessa värden om du använder en annan programserver som värd för AEM-formulär.

Följande inställningar är tillgängliga för JMS-tjänsten.

**Provider-URL:** URL:en för JNDI-tjänstprovidern. Standardvärdet baseras på JBoss-programservern. Följande URL är standardvärden för de programservrar som AEM-formulär har stöd för:

**JBoss:** `<server name>:1099`

**WebLogic:** `<server name>:7001`

**WebSphere:** `<server name>:2809`

**JNDI-användarnamn:** Användarnamnet för kontot som ska användas för autentisering med JNDI-tjänstprovidern som används för att söka efter namn på köer och ämnen. Standardvärdet är gäst.

**JNDI-lösenord:** Lösenordet som är associerat med användarnamnet som har angetts för JNDI-användarnamn. Standardvärdet är gäst.

**Initial Context Factory:** Den Java-klass som ska användas som inledande kontextfabrik. JMS-tjänsten använder den här klassen för att skapa en inledande kontext, som är utgångspunkten för att matcha namn på ämnen och köer. Standardvärdet är den inledande kontextfabriken för JMS-tjänsten på JBoss. Följande klasser är de inledande kontextfabrikerna för de programservrar som AEM-formulär har stöd för:

**JBoss:** org.jnp.interfaces.NamingContextFactory

**WebLogic:** weblogic.jndi.WLInitialContextFactory

**WebSphere:**.com.ibm.websphere.naming.WsnInitialContextFactory

**Användarnamn för anslutning:** Lösenordet som är associerat med användarnamnet som har angetts för anslutningsanvändarnamnet. Standardvärdet är gäst.

**Anslutningslösenord:** Lösenordet som är associerat med användarnamnet som har angetts för anslutningsanvändarnamnet. Standardvärdet är gäst.

**Andra egenskaper:** Egenskapsnamn och värdepar som du kan skicka till JNDI-tjänstprovidern. Dessa egenskaper beror på implementeringen och konfigurationen av providern som du använder.

Egenskapsnamnet och värdepar avgränsas med semikolon **;**. I följande text visas det värde som skulle ha angetts för två egenskaper med namnen name1 och name2, med värdena value1 och value2:

`name1=value1;name2=value2`

## LDAP-tjänstinställningar {#ldap-service-settings}

LDAP-tjänsten ( `LDAPService`) innehåller åtgärder för att fråga efter LDAP-kataloger. LDAP-kataloger används vanligtvis för att lagra information om personer, grupper och tjänster i en organisation.

Följande inställningar är tillgängliga för LDAP-tjänsten.

**Initial Context Factory:** Den Java-klass som ska användas som kontextfabrik. Den här klassen används för att skapa en anslutning till LDAP-servern. Standardvärdet är com.sun.jndi.ldap.LdapCtxFactory, vilket är lämpligt för de flesta LDAP-servrar.

**Provider-URL:** Den URL som ska användas för att ansluta till LDAP-tjänsten. Värdets format är `ldap://server name:port`

*servernamn* är namnet på den dator som är värd för LDAP-servern

*port* är den kommunikationsport som LDAP-tjänsten använder. Standardvärdet är 389, som är standardporten som används för LDAP-anslutningar.

**Användarnamn:** Användarnamnet för det användarkonto som ska användas för att logga in på LDAP-servern. Användarkontot måste ha behörighet att ansluta till servern och läsa informationen i LDAP-katalogen.

Beroende på LDAP-servern kan användarnamnet vara ett enkelt användarnamn, till exempel `myname` eller ett unikt namn, till exempel `cn=myname,cn=users,dc=myorg`.

**Lösenord:** Lösenordet som motsvarar användarnamnet som anges för inställningen Användarnamn.

**Andra egenskaper:** Ett strängvärde som representerar andra egenskaper och deras motsvarande värden som du kan ange för LDAP-servern. Värdet har följande format:

`property=value;property=value;...`

## Inställningar för konfigurationstjänsten för Microsoft SharePoint {#microsoft-sharepoint-configuration-service-settings}

Med konfigurationstjänsten `(MSSharePointConfigService)`för Microsoft SharePoint kan du ange autentiseringsuppgifter för den användare av AEM-formulär som har personifieringsbehörigheter. Mer information om personifieringsbehörigheter finns i [Konfigurera anslutningsprogrammet för Microsoft SharePoint](https://help.adobe.com/en_US/AEMForms/6.1/SharePointConfig/index.html).

Följande inställningar är tillgängliga för konfigurationstjänsten för Microsoft SharePoint:

* Användarnamn för en användare med personifieringsbehörigheter
* Lösenord för ovanstående användare

**Aktivera SSL (HTTPS):**

**TTL:** Tiden, i sekunder, som den här provisioneringsprofilen är giltig och cachelagrad på klienten. Standardvärdet är 86400 (24 timmar). När ett klientprogram synkroniseras med servern och den angivna tiden har gått, begär klientprogrammet en ny provisioneringsprofil från servern.

**Kryptering:** Anger om data som lagras på den mobila enheten ska krypteras.

**Forms-program:** Aktiverar Forms-funktionen i mobilklientprogrammen. När det här alternativet är markerat kan användare öppna formulär och initiera processer från sina mobila enheter.

**Aktivitetsprogram:** Aktiverar aktivitetsfunktionen i mobilklientprogrammen. När det här alternativet är markerat kan användare komma åt sina uppgiftslistor och slutföra uppgifter från sina mobila enheter.

**Content Services-program:** Aktiverar Content Services-funktionen i mobilklientprogrammet. Den här funktionen är endast tillgänglig för iOS. När det här alternativet är markerat kan iPhone- och iPad-användare komma åt filer som lagras i organisationens WebDAV-server.

**Offlinesupport:** Gör det möjligt för användare att fortsätta använda mobilklientprogrammen även när de inte har någon anslutning till servern (t.ex. när de är utanför cellområdet eller i flygplansläge). Användarna måste även aktivera offlinesupport på sina mobila enheter. Den här funktionen är tillgänglig för enheter med Android och iOS. Som standard är den här funktionen inaktiverad.

>[!NOTE]
>
>Om offlinesupport har aktiverats och sedan inaktiverats uppdateras användarnas provisioneringsprofiler omedelbart, eller så snart de är online. Om en användare har arbetat offline återgår alla väntande uppgifter till uppgiftslistan och alla objekt i kön, inklusive väntande formulär, uppgifter och formulär som innehåller valideringsfel, tas bort från kön.

**Android:** Tillåter Android-enheter att ansluta till servern.

**Apple iOS:** Tillåter iPhone- och iPad-enheter att ansluta till servern.

**AIR:** Tillåter att enheter som kör program som är baserade på Adobe AIR® ansluter till servern.

**BlackBerry:** Tillåter BlackBerry-enheter att ansluta till servern.

**Android Microsoft Exchange ActiveSync krävs:** Anger om Microsoft Exchange ActiveSync Policy Manager (EAS) måste vara installerat och aktivt på Android-enheter. När det här alternativet är markerat måste EAS användas på Android-enheten. När det här alternativet inte är markerat utförs ingen kontroll, även om andra krav fortfarande gäller.

**Minsta PIN-kodslängd för Android:** Android-enheter måste ha en global inställning som tvingar PIN-koden eller lösenordet att vara minst den här längden. Det räcker inte att bara ha en PIN-kod med den angivna längden. PIN-kodens längd måste framtvingas av systemet så att användare inte kan ta bort eller korta ned PIN-koden senare. Standardvärdet är 4.

**Android Maximum Password Retries Before Wipe:** Android-enheter har en global inställning som rensar systemet efter ett angivet antal ogiltiga lösenordsförsök. Den här globala inställningen är aktiverad och lika med eller lägre än det värde som anges här. Standardvärdet är 5.

**Android-rensning vid borttagning:** Anger vad som ska hända när en principöverträdelse inträffar på en Android-enhet. När det här alternativet är markerat tas kontot bort. När det här alternativet inte är markerat tas lösenordet för det lagrade kontot och cachelagrade data bort. Inga fler synkroniseringsförsök görs förrän användaren åtgärdar principöverträdelsen.

## Inställningar för utdatatjänst {#output-service-settings}

Med utdatatjänsten `(OutputService)` kan du sammanfoga XML-formulärdata med en formulärdesign som skapats i AEM-formulär i Designer och skapa en dokumentutdataström i något av följande format:

* Ett PDF- eller PDF/A-dokumentutdataflöde.
* En Adobe PostScript-utdataström.
* En PCL-utdataström (Printer Control Language).
* En ZPL-utdataström (Zebra Programming Language).

Utdataströmmen kan skickas till en nätverksskrivare, en lokal skrivare eller en diskfil. När du använder utdatatjänsten som en del av en process kan du även skicka utdataströmmen till en e-postmottagare som en bifogad fil.

Följande inställningar är tillgängliga för utdatatjänsten.

**Transaktionstyp:** Anger hur en transaktionskontext ska spridas till en åtgärd:

**Obligatoriskt:** stöder en transaktionskontext om det redan finns en. I annat fall skapas en ny transaktionskontext. Det här är standardvärdet.

**Kräver nytt:** Skapar alltid en ny transaktionskontext. Om det finns en aktiv transaktionskontext pausas den.

**Överföringstid (i sek):** Antalet sekunder som den underliggande transaktionsprovidern väntar innan en transaktion som omsluter den här åtgärden återställs. Det här värdet ignoreras om en befintlig transaktionskontext sprids.

När du bearbetar stora datafiler eller arbetar på en server med många operativsystem kan det vara nödvändigt att öka tidsgränsen för Output-tjänsten. Om du vill ändra timeout-värdet kontrollerar du att maskinvaruservrarna har tillräckligt med minne och att minnet är tillgängligt för Java Application Server-heap. Standardvärdet är `180`.

## Inställningar för PDFG Config-tjänsten {#pdfg-config-service-settings}

Följande inställningar är tillgängliga för PDFG Config-tjänsten ( `PDFGConfigService`).

**Katalog för användarjobbalternativ:** Sökvägen till filsystemsmappen där c-tjänsten skriver de jobbalternativsfiler som är tillgängliga för Acrobat Pro Extended. Standardvärdet är [user.home]/Application Data/Adobe/Adobe PDF/Settings.

**PS-startkatalog:** Sökvägen till filsystemmappen där startfilerna som krävs för Adobe Acrobat Distiller sparas. Standardvärdet är [user.home]/Application Data/Adobe/Adobe PDF/Distiller/Start.

**PS-startfil:** Namnet på startfilen som krävs av Adobe Acrobat Distiller. Standardvärdet är example.ps.

**Tidsgräns för serverkonvertering:** Den maximala tidsgränsen för jobbkonvertering (i sekunder) för tjänsten Generate PDF och Distiller. Den här inställningen begränsar den maximala konverteringstimeout som kan anges i filen config.xml och på administrationskonsolsidorna för PDF Generator. Standardvärdet är 270.

**Serverns globala tidsgräns:** Vid PDF-konvertering tar en Forms-server hänsyn till tidsgränsen. Konfigurera timeout-värdet för att lösa problemet.

**Prefix för jobbalternativ:** Ett prefix som används av tjänsten Generate PDF för att lägga till en kort sträng i jobbalternativfilerna som skapas temporärt för användning av Acrobat Distiller. Standardvärdet är pdfg.

**Program som inte är Unicode:** En kommaavgränsad lista över programnamn som är kända för att vara Unicode-inkompatibla. Den här listan är förifylld med namnen på flera program, som har stöd för i förväg konfigurerats i PDF Generator. Om du väljer att lägga till stöd för PDF-konverteringar via andra tredjepartsprogram som inte kan hantera Unicode måste du lägga till dem i den här listan. Standardvärdet är AutoCAD, Excel, PowerPoint, Project, Publisher, Visio, Word, WordPerfect.

**Antal servertrådpooler:** Styr storleken på den trådpool som används internt av tjänsten Generate PDF för att hantera konverteringsbegäranden från HTML till PDF som innefattar spikning (konvertering av länkade sidor som är tillgängliga från huvudsidan). Standardvärdet är 20.

**Sekunder för PDFG-rensningsgenomsökning:** Mer information finns i avsnittet Sekunder för jobbförfallodatum.

**Förfallotid i sekunder för jobb:** Tjänsten Generate PDF tar bort indatafiler så snart de konverteras. Utdatafilerna sparas tillfälligt under en tid som bestäms av inställningarna Sekunder för PDFG-rensning och Sekunder för jobbförfallodatum.

Inställningen Sekunder för förfallodatum för jobb anger hur gammal en fil eller tom mapp måste vara innan den kan tas bort. Inställningen Sekunder för PDFG-rensningsgenomsökning anger hur ofta en rensningstråd söker igenom de tillfälliga mapparna efter filer som kan tas bort.

Om t.ex. Förfallotid i sekunder för jobb är inställt på 100 och PDFG-rensningsgenomsökningssekunder är inställt på 200, körs rensningstråden var 200:e sekund och tar bort filer som är 100 sekunder eller äldre.

Standardvärdet för sekundära PDFG-rensningsgenomsökningar är `43200` (12 timmar). Standardvärdet för Förfallotid för jobb är `86400` (24 timmar).

**Standardspråk:** Används för att åsidosätta standardspråket (land + språk) på servern där tjänsten Generate PDF distribueras. Om den här parametern inte är angiven bestäms standardspråket av det operativsystem som tjänsten distribueras till. Den här parametern styr vilket språk felmeddelandena returneras till API:erna.

## inställningar för datatjänsttjänst för formulärarbetsflöde {#forms-workflow-data-services-service-settings}

Följande tjänster utökar Data Services och visar sammansättningar som Workspace använder för att kommunicera med servern. Ändra inte konfigurationsalternativen för de här tjänsterna om du inte har fått instruktioner om att göra det av Adobe Support. Dessa tjänster är inte avsedda för direkt åtkomst:

* `ProcessManagementLcdsAttachmentService`
* `ProcessManagementLcdsPropertyService`
* `ProcessManagementLcdsTaskService`

## Fjärrtjänstinställningar {#remoting-service-settings}

De flesta tjänster är konfigurerade så att du kan komma åt dem via AEM Forms Remoting (borttaget för AEM-formulär). Mer information om (Borttagning för AEM-formulär) AEM-formulär finns i [Programmering med AEM-formulär](https://adobe.com/go/learn_aemforms_programming_63).

Följande inställningar är tillgängliga för tjänsten Remoting.

**Flex klientautentiseringsmetod:** Bestämmer vilken typ av svar som servern skickar tillbaka till klienten när den anropade tjänsten är säkerhetsaktiverad, den anropade åtgärden inte stöder anonyma anrop och klienten skickar antingen inga eller ogiltiga autentiseringsuppgifter. Välj Anpassad eller Grundläggande. Standardvärdet är Grundläggande.

**Tillåt serialisering av icke-serialiserbara klasser:** De flesta slutpunkter för AEM-formulär tillåter endast serialiserbara klasser att användas för anrop. I äldre versioner tillät slutpunkten Remoting icke-serialiserbara klasser att användas för anrop från Flex-baserade klienter. För att förhindra en säkerhetslucka som beskrivs i APS11-15 har detta ändrats. Markera den här kryssrutan om du vill fortsätta använda icke-serialiserbara klasser med slutpunkten för Flex Remoting.

## Inställningar för databastjänst {#repository-service-settings}

Databastjänsten ( `RepositoryService`) tillhandahåller tjänster för lagring och hantering av resurser i AEM-formulär. När utvecklare skapar ett program kan de distribuera resurserna i databasen i stället för i ett filsystem. Materialet kan innehålla alla typer av material, inklusive XML-formulär, PDF forms (inklusive Acrobat-formulär), formulärfragment, bilder, profiler, profiler, SWF-filer, DDX-filer, XML-scheman, WSDL-filer och testdata.

Du kan använda standarddatabasen som ingår i AEM-formulär eller en tredjepartsdatabas (EMC Documentum Content Server, IBM FileNet Content Manager eller IBM Content Manager).

Databasprovidertjänsten är en tjänstdelegat som fungerar som gränssnitt till en providertjänst. Detta gör att du kan ansluta till ett vanligt API och inte behöver vara medveten om vilken providertjänst som implementerar lagringsfunktionerna. Databasprovidertjänsten tillhandahåller databaslagring för resurserna för databastjänsten.

Följande inställning är tillgänglig för databastjänsten.

**Providertjänst:** Namnet på tjänsten som används som lagringsprovider. Standardvärdet är RepositoryProviderService.

## Inställningar för signaturtjänst {#signature-service-settings}

Signaturtjänsten ( `SignatureService`) gör det möjligt för din organisation att skydda säkerheten och sekretessen för Adobe PDF-dokument som den distribuerar och tar emot. Den här tjänsten använder digitala signaturer och certifiering för att säkerställa att dokumenten inte ändras. Om du ändrar ett dokument bryts signaturen. Eftersom säkerhetsfunktionerna tillämpas på själva dokumentet förblir dokumentet säkert och kontrollerat under hela sin livscykel, utanför brandväggen, när det laddas ned offline och när det skickas tillbaka till organisationen.

Följande inställningar är tillgängliga för signaturtjänsten.

**Namnet på fjärr-HSM SPI-tjänsten:** Det här alternativet används när HSM är installerat på en fjärrdator. Ange det här alternativet när AEM-formulär installeras i ett 64-bitars Windows och du använder HSM-enheter för signering.

**URL för fjärr-HSM-webbtjänsten:** Ange det här alternativet när AEM-formulär installeras i 64-bitars Windows och du använder HSM-enheter för signering.

**Certifiering för att inkludera formulärinläsningsändringar:** När det här alternativet är markerat certifieras XFA-formulärstatusen utöver XFA-mallen. Observera att aktivering av det här alternativet kan ha en negativ inverkan på prestandan. Standardvärdet är true.

**Kör JavaScript-skript för dokument:** Anger om JavaScript-skript för dokument ska köras under signeringsåtgärder. Standardvärdet är false.

**Bearbeta dokument med Acrobat 9-kompatibilitet:** Anger om Acrobat 9-kompatibilitet ska aktiveras. Om du till exempel väljer det här alternativet aktiveras Synlig certifiering i dynamiska PDF-filer. Standardvärdet är false.

**Bädda in spärrinformation vid signering:** Anger om spärrinformation bäddas in när PDF-dokumentet signeras. Standardvärdet är false.

**Bädda in återkallningsinformation vid certifiering:** Anger om återkallningsinformationen bäddas in när PDF-dokumentet certifieras. Standardvärdet är false.

**Tvinga inbäddning av spärrinformation för alla certifikat
Under signering/certifiering:** Anger om en signerings- eller certifieringsåtgärd misslyckas om giltig återkallningsinformation för alla certifikat inte bäddas in. Observera att om ett certifikat inte innehåller någon CRL- eller OCSP-information, anses det vara giltigt, även om ingen återkallningsinformation hämtas. Standardvärdet är false.

**Återkallningskontrollordning:** Anger ordningen för spärrkontroll när det är möjligt att kontrollera detta med hjälp av både certifikatåterkallningslistan (CRL) och OCSP-mekanismer (Online Certificate Status Protocol). Standardvärdet är OCSPFfirst.

**Maximal storlek på information om återkallningsarkivering:** Den maximala storleken på information om återkallningsarkivering i kilobyte. AEM-formulär försöker lagra så mycket spärrinformation som möjligt utan att överskrida gränsen. Standardvärdet är 10 kB.

**Stödsignaturer har skapats från PreRelease-versioner av
Adobe-produkter:** När det här alternativet är markerat valideras signaturer som skapats med en förhandsversion av Adobe-produkter korrekt. Standardvärdet är false.

**Alternativ för verifieringstid:** Anger tidpunkten för verifiering av en signerares certifikat. Standardvärdet är Säker tid för annan aktuell tid.

**Använd återkallningsinformation som arkiverats i signaturen under
Validering:** Anger om den spärrinformation som arkiveras med signaturen används för spärrkontroll. Standardvärdet är true.

**Använd verifieringsinformation som finns lagrad i dokumentet för
Validering av signaturer:** När det här alternativet är markerat används den valideringsinformation (inklusive information om återkallning och tidsstämpling) som är inbäddad i dokumentet för att validera signaturer. Standardvärdet är true.

**Högsta tillåtna antal kapslade verifieringssessioner:** Det högsta tillåtna antalet kapslade verifieringssessioner. AEM-formulär använder det här värdet för att förhindra en oändlig slinga när OCSP- eller CRL-signerarcertifikaten verifieras när OCSP- eller CRL-certifikatet inte är korrekt konfigurerat. Standardvärdet är 10.

**Maximal klockförvrängning för verifiering:** Den maximala tiden, i minuter, som signeringstiden kan vara efter valideringstiden. Om klockskevningen är större än det här värdet är signaturen inte giltig. Standardvärdet är 65 minuter.

**Cacheminne för certifikatets livstid:** Livslängden för ett certifikat som har hämtats online eller på annat sätt i cacheminnet. Standardvärdet är 1 dag.

### Transportalternativ {#transport-options}

**Proxyvärd:** URL:en för proxyvärden. Används endast om ett giltigt värde anges. Inget standardvärde.

**Proxyport:** Proxyporten. Ange ett giltigt portnummer mellan 0 och 65535. Standardvärdet är 80.

**Användarnamn för proxyinloggning:** Användarnamn för proxyinloggning. Används bara om ett giltigt värde har angetts för proxyvärden och proxyport. Inget standardvärde.

**Lösenord för proxyinloggning:** Lösenordet för proxyinloggning. Används bara om ett giltigt värde anges för proxyvärden, proxyporten och användarnamnet för proxyinloggning. Inget standardvärde.

**Maximal hämtningsgräns:** Den maximala mängden data, i MB, som kan tas emot per anslutning. Det minsta värdet är 1 MB och det högsta värdet är 1 024 MB. Standardvärdet är 16 MB.

**Anslutningens timeout:** Den längsta väntetiden, i sekunder, för att upprätta en ny anslutning. Det minsta värdet är 1 och det högsta värdet är 300. Standardvärdet är 5.

**Sockettimeout:** Den längsta väntetiden, i sekunder, innan en sockettimeout (i väntan på dataöverföring) inträffar. Det minsta värdet är 1 och det högsta värdet är 3 600. Standardvärdet är 30.

### Alternativ för validering av sökväg {#path-validation-options}

**Kräv explicit princip:** Anger om sökvägen måste vara giltig för minst en av certifikatprofilerna som är associerad med signerarcertifikatets förtroendeankare. Standardvärdet är false.

**Hindra EN princip:** Anger om objektidentifieraren för princip (OID) ska behandlas om den ingår i ett certifikat. Standardvärdet är false.

**Hindra principmappning:** Anger om principmappning tillåts i certifieringssökvägen. Standardvärdet är false.

**Kontrollera alla sökvägar:** Anger om alla sökvägar ska valideras eller om valideringen ska avbrytas efter att den första giltiga sökvägen har hittats. Välj true eller false. Standardvärdet är false.

**LDAP-server:** LDAP-servern som används för att söka efter certifikat för sökvägsvalidering. Inget standardvärde.

**Följ URI:er i AIA-certifikat:** Anger om URI:er (Uniform Resource Identifiers) i AIA-certifikat behandlas vid sökvägsidentifiering. Standardvärdet är false.

**Tillägg för grundläggande begränsningar krävs i certifikatutfärdarcertifikat:** Anger om certifikatutfärdarens certifikattillägg för grundläggande begränsningar måste finnas för certifikatutfärdarcertifikat. Vissa tidiga tyska certifierade rotcertifikat (7 och tidigare) är inte kompatibla med RFC 3280 och innehåller inte det grundläggande begränsningstillägget. Om det är känt att en användares EE-certifikat kan härledas till en sådan tysk rot avmarkerar du den här kryssrutan. Standardvärdet är true.

**Kräv giltig certifikatsignatur under kedjeskapande:** Anger om kedjebyggaren kräver giltiga signaturer för certifikat som används för att skapa kedjor. När den här kryssrutan är markerad kommer kedjebyggaren inte att skapa kedjor med ogiltiga RSA-signaturer på certifikat. Överväg kedja CA > ICA > EE där certifikatutfärdarens signatur på ett ICA inte är giltig. Om den här inställningen är true stoppas kedjebyggnaden vid ICA och CA inkluderas inte i kedjan. Om den här inställningen är false skapas den fullständiga 3-certifikatskedjan. Den här inställningen påverkar inte DSA-signaturer. Standardvärdet är false.

### Alternativ för tidsstämpelleverantör {#timestamp-provider-options}

**TSP-serverns URL:** URL:en för standardtidsstämpelprovidern. Används endast om ett giltigt värde anges. Inget standardvärde.

**TSP-serverns användarnamn:** Användarnamnet, om nödvändigt, av tidsstämpelprovidern. Används bara om ett giltigt värde har angetts för URL:en. Inget standardvärde.

**TSP-serverlösenord:** Lösenordet för det ovanstående användarnamnet om det behövs av tidsstämpelprovidern. Används bara om ett giltigt värde har angetts för URL:en och användarnamnet. Inget standardvärde.

**Begär hash-algoritm:** Anger den hash-algoritm som ska användas när begäran för tidsstämpelprovidern skapas. Standardvärdet är SHA1.

**Format för spärrkontroll:** Anger det spärrkontrollsformat som används för att fastställa pålitlighetsstatus för tidsstämpelproviderns certifikat från den spärrstatus som observerats. Standardvärdet är BestEffort.

**Skicka en gång:** Anger om ett namn skickas med tidsstämpelproviderbegäran. Ett omedelbart kan vara en tidsstämpel, en besöksräknare på en webbsida eller en speciell markör som är avsedd att begränsa eller förhindra otillåten kopiering eller reproduktion av en fil. Standardvärdet är true.

**Använd utgångna tidsstämplar under validering:** När det här alternativet är markerat kan utgångna tidsstämplar användas för att hämta valideringstider för signaturer. Standardvärdet är true.

**TSP-svarsstorlek:** Beräknad storlek i byte för TSP-svaret. Det här värdet bör representera den maximala storleken för tidsstämpelsvaret som den konfigurerade tidsstämpelprovidern kan returnera. Ändra inte detta om du inte är helt säker. Det minsta värdet är 60 B och det högsta värdet är 10240 B. Standardvärdet är 4096B.

**Ignorera tidsstämpelservertillägg**: Markera alternativet **Ignorera tidsstämpelservertillägg** om du inte vill att AEM Forms-servern ska kontakta den angivna tidsstämpelservern. Om du väljer det här alternativet undviker du processfel som inträffar på grund av timeout i anslutningen mellan AEM Forms och tidsstämpelservrar.

### Alternativ för listan över spärrade certifikat {#certificate-revocation-list-options}

**Konsult-URI först:** Anger om den CRL-plats som anges i lokal URI eller CRL-sökning ska ges företräde framför någon plats som anges i ett certifikat för spärrkontroll. Standardvärdet är false.

**Lokal URI för CRL-sökning:** URL för den lokala CRL-providern. Det här värdet används bara om inställningen Konsult lokal URI första är true. Inget standardvärde.

**Format för spärrkontroll:** Anger det format för spärrkontroll som används för att fastställa pålitlighetsstatus för CRL-providerns certifikat från den status som observerats för spärrkontroll. Standardvärdet är BestEffort.

**LDAP-server för CRL-sökning:** LDAP-servern använde för att hämta CRL:er (som www.ldap.com). Alla DN-baserade frågor om listor över återkallade certifikat dirigeras till den här servern. Inget standardvärde.

**Anslut:** Anger om du ska ansluta för att hämta en lista över återkallade certifikat. Om värdet är false genomsöks bara cachelagrade listor över återkallade certifikat (på den lokala hårddisken eller sådana som är inbäddade med signatur). Standardvärdet är true.

**Ignorera giltighetsdatum:** Anger om svarets thisUpdate- och nextUpdate-tider ska ignoreras, vilket förhindrar att dessa tider har en negativ effekt på svarets giltighet. Standardvärdet är false.

**Kräv AKI-tillägg i CRL:** Anger om tillägget för identifierare av utfärdarnyckel måste inkluderas i en CRL. Standardvärdet är false.

### Alternativ för statusprotokoll för onlinecertifikat {#online-certificate-status-protocol-options}

**URL för OCSP-server:** för OCSP-standardservern. Om OCSP-servern som anges via den här URL:en används beror på inställningen för alternativet URL till rådgivning. Inget standardvärde.

**URL till konsult-alternativ:** Styr listan och ordningen för de OCSP-servrar som används för att utföra statuskontrollen. Standardvärdet är UseAIAInCert.

**Format för spärrkontroll:** Anger den stil för spärrkontroll som används vid verifiering av OCSP-serverns certifikat. Standardvärdet är CheckIfAvailable.

**Skicka en gång:** Anger om ett namn skickas med OCSP-begäran. Ett omedelbart kan vara en tidsstämpel, en besöksräknare på en webbsida eller en speciell markör som är avsedd att begränsa eller förhindra otillåten kopiering eller reproduktion av en fil. Standardvärdet är true.

**Maximal tid för skevning av klocka:** Maximal tillåten skevning, i minuter, mellan svarstid och lokal tid. Det minsta värdet är 0 och det högsta värdet är 2147483647m. Standardvärdet är 5 m.

**Tid för svarsfärdighet:** Maximal tid, i minuter, för vilken ett förkonstruerat OCSP-svar anses vara giltigt. Det minsta värdet är 1 m och det högsta tillåtna värdet är 2147483647. Standardvärdet är 525600 (ett år).

**Signera OCSP-begäran:** Anger om OCSP-begäran ska signeras. Standardvärdet är false.

**Begär autentiseringsuppgifter för signerare alias:** Anger det autentiseringsalias som ska användas för att signera OCSP-begäran om signering är aktiverat. Används bara om signering av OCSP-begäran är aktiverat. Inget standardvärde.

**Anslut:** Anger om du ska ansluta för att göra en spärrkontroll. Standardvärdet är true.

**Ignorera svarets thisUpdate- och nextUpdate-tider:** Anger om svarets thisUpdate- och nextUpdate-tider ska ignoreras, vilket förhindrar att dessa tider får en negativ effekt på svarets giltighet. Standardvärdet är false.

**Tillåt OCSPNoCheck-tillägg:** Anger om OCSPNoCheck-tillägget tillåts i svarssigneringscertifikatet. Standardvärdet är true.

**Kräv OCSP ISIS-MTT CertHash-tillägg:** Anger om ett hash-tillägg för certifikatets offentliga nyckel måste inkluderas i OCSP-svar. Standardvärdet är false.

### Felhanteringsalternativ för felsökning {#error-handling-options-for-debugging}

**Rensa certifikatcache vid nästa API-anrop:** Anger om certifikatcachen ska rensas när nästa signaturtjänståtgärd anropas. När åtgärden har anropats återställs det här alternativet till false. Standardvärdet är false.

**Rensa CRL-cache vid nästa API-anrop:** Anger om CRL-cachen ska rensas när nästa signaturtjänståtgärd anropas. När åtgärden har anropats återställs det här alternativet till false. Standardvärdet är false.

**Rensa OCSP-cache vid nästa API-anrop:** Anger om OCSP-cachen ska rensas när nästa signaturtjänståtgärd anropas. När åtgärden har anropats återställs det här alternativet till false. Standardvärdet är false.

## Inställningar för tjänsten Bevakade mappar {#watched-folder-service-settings}

Tjänsten Bevakad mapp ( `WatchedFolder`) konfigurerar attribut som är gemensamma för alla bevakade mappslutpunkter. Det innehåller också standardvärden för bevakade mappslutpunkter. (Se [Konfigurera bevakade mappslutpunkter](/help/forms/using/admin-help/configuring-watched-folder-endpoints.md#configuring-watched-folder-endpoints).) Den anropas inte av externa klientprogram eller används i processer som skapats i Workbench.

Följande inställningar är tillgängliga för tjänsten Bevakade mappar.

**Kronuttryck:** Kronuttrycket som används av kvarts för att schemalägga avsökningen av indatakatalogen.

**Antal upprepningar:** Antalet gånger som indatakatalogen avsöks. Standardantalet för upprepning som ska användas om det här värdet inte anges i slutpunktskonfigurationen. Värdet -1 anger att katalogen genomsöks oändligt. Standardvärdet är -1.

**Intervall för upprepning:** Standardvärdet för antal sekunder mellan varje omröstning. Det här värdet används som upprepningsintervall såvida inte ett annat värde anges i konfigurationen för bevakad mappslutpunkt. Standardvärdet är 5. Mer information finns i beskrivningen av inställningen Gruppstorlek.

**Asynkron:** Identifierar anropstypen som asynkron eller synkron. Övergående och synkrona processer kan bara anropas synkront. Standardvärdet är asynkront.

**Väntetid:** Standardvärdet för tid, i sekunder, efter vilket filerna hämtas från indatamapparna. Om filen eller mappen är äldre än den tid som anges i väntetiden hämtas den för bearbetning. Standardvärdet är 0.

**Batchstorlek:** Standardvärdet för antalet filer eller mappar som bearbetas per skanning. Standardvärdet är 2.

Inställningarna Intervall för upprepning och Gruppstorlek avgör hur många filer i Bevakade mappar som tas upp vid varje sökning. Bevakad mapp använder en Quartz-trådpool för att skanna indatamappen. Trådpoolen delas med andra tjänster. Om skanningsintervallet är litet genomsöks indatamappen ofta av trådarna. Om filer ofta placeras i den bevakade mappen bör du hålla sökintervallet litet. Om filerna tas bort sällan bör du använda ett större inläsningsintervall så att de andra tjänsterna kan använda trådarna.

Om det finns en stor mängd filer som tas bort gör du gruppstorleken stor. Om till exempel tjänsten som anropas av den bevakade mappens slutpunkt kan bearbeta 700 filer per minut, och användarna släpper filer i indatamappen med samma hastighet, och du sedan anger värdet 350 för Gruppstorlek och intervallet för upprepning till 30 sekunder, kommer prestandan för bevakad mapp att bli bättre utan att kostnaden för att skanna den bevakade mappen för ofta påverkas.

När filer släpps i den bevakade mappen visas filerna i indata, vilket kan försämra prestanda om skanningen sker varje sekund. Om du ökar skanningsintervallet kan prestandan förbättras. Om volymen för de filer som tas bort är liten justerar du batchstorleken och upprepningsintervallet därefter. Om till exempel 10 filer tas bort varje sekund, kan du försöka med att ange intervallet för upprepning till 1 sekund och gruppstorleken till 10.

I en klusterkonfiguration skalas inte gruppstorleken för en bevakad mappslutpunkt till flera klusternoder. Om batchstorleken till exempel är `2` för ett kluster med två noder och alternativet Begränsning är markerat bearbetar noderna alla filer gruppvis i stället för i varje nod som bearbetar två filer samtidigt.

**Skriv över duplicerade filnamn:** En boolesk sträng som anger om den bevakade mappen skriver över duplicerade resultatfilnamn och om bevarade dokument med samma namn ska skrivas över.

**Bevara mapp:** Standardvärdet för den reserverade mappen. Den här mappen används för att kopiera källfilerna till om bearbetningen av indata lyckas. Värdet kan vara en tom, relativ eller absolut sökväg med ett filmönster enligt beskrivningen för inställningen Resultatmapp.

**Felmapp:** Namnet på mappen där felfilerna kopieras. Värdet kan vara en tom, relativ eller absolut sökväg med ett filmönster enligt beskrivningen för inställningen Resultatmapp.

**Resultatmapp:** Standardnamnet för resultatmappen. Den här mappen används för att kopiera resultatfilerna till. Värdet kan vara en tom, relativ eller absolut sökväg med följande filmönster.

* %F = filnamnsprefix
* %E = filnamnstillägg
* %Y = år (full)
* %y = år (de två sista siffrorna)
* %M = månad
* %D = dag i månaden
* %d = dag på året
* %H = timme (24-timmars klocka)
* %h = timme (12-timmars klocka)
* %m = minut
* %s = sekund
* %l = millisekund
* %R = slumptal (från 0 till 9)
* %P = process- eller jobb-ID

Om det till exempel är 2009-08-17 och du anger `C:/Test/WF0/failure/%Y/%M/%D/%H/` blir resultatmappen `C:/Test/WF0/failure/2009/07/17/20`.

Om sökvägen inte är absolut men relativ skapas mappen i den bevakade mappen. Mer information om filmönster finns i [Om filmönster](/help/forms/using/admin-help/configuring-watched-folder-endpoints.md#about-file-patterns).

>[!NOTE]
>
>Ju mindre resultatmapparna är, desto bättre prestanda blir Bevakade mappar. Om den beräknade inläsningen för den bevakade mappen till exempel är 1 000 filer varje timme kan du prova ett mönster som `result/%Y%M%D%H` så att en ny undermapp skapas varje timme. Om inläsningen är mindre (till exempel 1 000 filer per dag) kan du använda ett mönster som `result/%Y%M%D`.

**Scenmapp:** Standardnamnet på scenmappen i den bevakade mappen.

**Indatamapp:** Standardnamnet på indatamappen i den bevakade mappen.

**Bevara vid fel:** Om värdet är true bevaras originalfilerna i felmappen vid fel.

**Begränsning:** När det här alternativet är markerat begränsas antalet bevakade mappjobb som bearbetas i AEM-formulär vid en given tidpunkt. Värdet för Gruppstorlek avgör det maximala antalet jobb (se Om begränsning).

## Inställningar för webbtjänst {#web-service-service-settings}

Webbtjänsten ( `WebService`) aktiverar processer för att anropa webbtjänståtgärder.

Med webbtjänsten kan processer starta webbtjänståtgärder. En organisation kan till exempel vilja integrera en process för att lagra och hämta information som kontakt- och kontoinformation genom att anropa en tjänsteleverantörs exponerade webbtjänster. Webbtjänsten anropar en angiven webbtjänst och skickar värden för var och en av dess parametrar. Returvärdena från åtgärden sparas sedan i en angiven variabel i en process.

Webbtjänsten interagerar med webbtjänster genom att skicka och ta emot SOAP-meddelanden. Tjänsten har även stöd för att skicka MIME-, MTOM- och SwaRef-bilagor med SOAP-meddelanden via protokollet WS-Attachment. Webbtjänstens interaktioner är kompatibla med SAP-system och .NET-webbtjänster.

Följande inställningar är tillgängliga för webbtjänsten.

**Nyckelarkiv:** Den fullständiga sökvägen till nyckelfilen som innehåller den privata nyckel som ska användas för autentisering. Forms Server måste kunna komma åt filen.

**Lösenord för nyckelbehållare:** Lösenordet för nyckelfilen.

**Nyckellagringstyp:** Nyckellagringstypen. Ange inget värde för att använda standardnyckelbehållartypen som är konfigurerad för den JVM som kör Forms Server. Annars anger du något av följande värden:

* jks
* pkcs12
* cms
* jceks

**Förtroendearkiv:** Den fullständiga sökvägen till förtroendearkivfilen som innehåller den offentliga nyckeln för webbtjänstservern.

**Lösenord för förtroendearkivet:** Lösenordet för förtroendearkivfilen.

**Pålitlig lagringstyp:** Typ av förtroendearkiv. Ange inget värde för att använda standardnyckelbehållartypen som är konfigurerad för den JVM som kör Forms Server. Annars anger du något av följande värden:

* jks
* pkcs12
* cms
* jceks

## Inställningar för XSLT-omvandlingstjänsten {#xslt-transformation-service-settings}

Tjänsten XSLT-omvandling ( `XSLTService`) aktiverar processer för att tillämpa XSLT (Extensible Stylesheet Language Transformations) på XML-dokument.

Följande inställning är tillgänglig för tjänsten XSLT-omvandling.

**Fabriksnamn:** Det fullständiga kvalificerade namnet på Java-klassen som ska användas för att utföra XSLT-omvandlingar. Om inget värde anges används standardfabriken som är konfigurerad i Java Virtual Machine som kör Forms Server.

## Ändra säkerhetsinställningar för en tjänst {#modifying-security-settings-for-a-service}

Med Forms Server kan du konfigurera säkerhetsinställningar för varje tjänst, vilket gör att du kan konfigurera en detaljerad åtkomstkontroll på en tjänst-för-tjänst-nivå.

Standardsäkerhetsprofiler installeras, som sedan kan konfigureras för att uppfylla dina systembehov. Varje säkerhetsprofil har en associerad domän och skapas antingen på användarnivå eller gruppnivå.

### Ändra säkerhetsinställningar för en tjänst {#modify-security-settings-for-a-service}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. Klicka på den tjänst du vill konfigurera på sidan Tjänsthantering.
1. Klicka på fliken Säkerhet.
1. I listan Kräv att anropare autentiserar väljer du antingen Ja eller Nej för att ange om tjänsten kan anropas med eller utan autentiseringsuppgifter.

   Om du väljer Ja måste anroparen autentiseras och användarens huvudnamn för anroparen måste ha behörighet att anropa tjänsten. I annat fall nekas anropet.

   Om du väljer Nej kan anroparen av tjänsten vara autentiserad eller inte. Anropet av tjänsten lyckas alltid eftersom det inte finns någon auktoriseringskontroll.

1. För tjänster som innehåller en eller flera åtgärder som flaggats för anonym åtkomst markerar eller avmarkerar du alternativet Anonym åtkomst tillåten. När anonym åtkomst är aktiverat kan alla användare i systemet anropa åtgärder för tjänsten. Om anonym åtkomst är inaktiverad måste användarna ges behörighet att anropa tjänsten och anropa åtgärder. Användare beviljas dessa behörigheter antingen direkt eller som en del av en grupp som har sådana behörigheter.
1. För vissa tjänster påverkar det användarkonto som utför åtgärden resultatet. I Innehållstjänster (Borttagen) blir till exempel användaren som lagrar innehåll ägare till innehållet, vilket påverkar vem som kan komma åt innehållet senare. Om du använder en process för att lagra innehåll bör du tänka på vilken användare som används för att köra dokumenthanteringstjänsten, eftersom den användaren kommer att äga det lagrade innehållet.

   Om du vill ange den körtidsidentitet som används av en tjänst för att köra åtgärder väljer du Ange Kör som, väljer ett alternativ i den associerade listan och klickar sedan på Spara. Välj bland följande alternativ:

   **Anroparen:** Använder samma identitet som användaren som anropade tjänsten.

   **System:** Använder systemanvändaren för att köra tjänsten med fullständig behörighet.

   **Namngiven användare:** Gör att du kan köra tjänsten som en specifik användare. När du väljer det här alternativet klickar du på Välj användare för att visa sidan Välj huvudnamn, där du kan söka efter och välja användaren.

   Om du inte väljer Ange Kör som används standardbeteendet.

   >[!NOTE]
   >
   >Renderings- och skicka-tjänster som används med variablerna xfaForm, Document Form och Form körs alltid med användarkontot i System.

1. Klicka på Lägg till huvudnamn för att ange behörigheter som användare och grupper har för tjänsten.
1. På skärmen Välj huvudnamn visas de användare och grupper som är konfigurerade i Användarhantering. Om användaren eller gruppen som du vill använda inte visas använder du sökfunktionen för att hitta den. Klicka på ett användar- eller gruppnamn.
1. På skärmen Lägg till behörigheter väljer du de behörigheter som ska tilldelas till användaren eller gruppen för den här tjänsten:

   * **INVOKE_PERM:** Så här anropar du alla åtgärder i tjänsten
   * **ÄNDRA_CONFIG_PERM:** Så här ändrar du konfigurationen för en tjänst
   * **SUPERVISOR_PERM:** Så här visar du processinstansdata för en tjänst som skapats från en process
   * **START_STOP_PERM:** Starta och stoppa en tjänst
   * **ADD_REMOVE_ENDPOINTS_PERM:** Lägga till, ta bort och ändra slutpunkter för en tjänst
   * **CREATE_VERSION_PERM:** Skapa en version av tjänsten
   * **DELETE_VERSION_PERM:** Ta bort en version av tjänsten
   * **ÄNDRA_VERSION_PERM:** Så här ändrar du en version av tjänsten
   * **READ_PERM:** Så här visar du tjänsten
   * **PROCESS_OWNER_PERM:** För användning i framtida versioner av AEM-formulär. Använd inte denna behörighet.
   * **SERVICE_MANAGER_PERM:** För användning i framtida versioner av AEM-formulär. Använd inte denna behörighet.
   * **SERVICE_AGENT_PERM:** För användning i framtida versioner av AEM-formulär. Använd inte denna behörighet.

1. Klicka på Lägg till.

### Ta bort säkerhetsobjektet från en säkerhetsprofil {#remove-the-principal-from-a-security-profile}

1. Välj den tjänst som ska konfigureras på sidan Tjänsthantering.
1. Klicka på fliken **Dokumentskydd**, markera säkerhetsprofilen som ska tas bort och klicka på **Ta bort**.

## Konfigurera poolning för en tjänst {#configuring-pooling-for-a-service}

Varje tjänst kan utnyttja poolfunktionerna för att hantera inkommande anrop. Om du använder en tjänstpool försäkrar du dig om att tjänstinstanser anropas av en enda tråd i taget och återanvänds i alla anropsbegäranden, vilket kan leda till bättre prestanda. Du kan också använda poolning för att ange alternativet Maximalt antal asynkrona tjänstinstanser, som tillåter tjänster att begränsa antalet begäranden som hanteras parallellt.

### Aktivera poolning {#enable-pooling}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. Klicka på den tjänst du vill konfigurera på sidan Tjänsthantering.
1. Klicka på fliken Pooling.
1. Välj Sammanslagna instanser för alla begäranden i listan över strategier för behandling av begäranden.
1. Ange poolens inledande storlek i rutan Poolstorlek för initialtjänstinstans. När tjänsten distribueras används det här numret för att fastställa antalet tjänstimplementeringsinstanser som skapas och tilldelas till den kostnadsfria poolen, i väntan på anropsbegäranden. Detta gör att tjänstbehållaren kan svara direkt på anropsbegäranden utan att först behöva initiera en tjänstinstans.
1. I rutan Maximal storlek för tjänstinstanspool anger du maximalt antal instanser i poolen för en viss tjänst. Den här inställningen styr antalet trådar som kan köra en viss tjänst vid en given tidpunkt. Standardvärdet är 0, vilket ger obegränsad poolstorlek.
1. I rutan Maximalt antal asynkrona tjänstinstanser anger du maximalt antal instanser från poolen som kan användas för att hantera asynkrona begäranden vid en given tidpunkt. Med den här inställningen kan tjänsten begränsa antalet begäranden som kan hanteras parallellt.
1. I rutan Timeout för anropsväntetid anger du antalet millisekunder som ska vänta på att en tjänst ska bli tillgänglig för en anropsbegäran. Om du inte anger något värde för den här inställningen är standardvärdet 0, vilket ger ingen väntetid.
1. Klicka på Spara.

### Ta bort poolning {#remove-pooling}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. Klicka på den tjänst du vill konfigurera på sidan Tjänsthantering.
1. Klicka på fliken Pooling.
1. Välj antingen Ny instans för varje begäran eller En instans för alla begäranden i listan över strategier för behandling av begäranden.

   **En instans för alla begäranden:** En tjänstinstans skapas och cachelagras när den första begäran kommer in i behållaren. Varje begäran efter den begäran använder samma tjänstinstans för att hantera begäran.

   **Ny instans för varje begäran:** En ny tjänstinstans skapas för varje mottaget anrop.

1. Klicka på Spara.
