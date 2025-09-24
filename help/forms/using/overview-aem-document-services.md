---
title: Översikt över AEM Document Services
description: AEM Document Services är en uppsättning OSGi-tjänster för att skapa, sammanställa och skydda PDF-dokument.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: document_services
docset: aem65
feature: Document Services,Reader Extensions, Forms Service,PDF Generator
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 03e87c5a-c106-4b4c-9b42-8ce7a04d9c0c
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '1422'
ht-degree: 0%

---

# Översikt över AEM Document Services{#overview-of-aem-document-services}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/using-communications/aem-forms-cloud-service-communications-introduction.html?lang=sv-SE).


AEM Document Services är en uppsättning OSGi-tjänster för att skapa, sammanställa och skydda PDF-dokument. Dokumenttjänster innehåller följande tjänster:

## Output Service {#output-service}

Med Output Service kan du skapa dokument i olika format, t.ex. PDF, laserskrivarformat och etikettskrivarformat. Laserskrivarformat är PostScript och Printer Control Language (PCL). I följande lista anges etikettskrivarformat:

* Zebra (ZPL)
* Intermec (IPL)
* Datamax (DPL)
* TecToshiba (TPCL)

Ett dokument kan skickas till en nätverksskrivare, en lokal skrivare eller en fil i filsystemet. Output-tjänsten sammanfogar XML-formulärdata med en formulärdesign för att generera ett dokument. Utdatatjänsten kan generera ett dokument utan att sammanfoga XML-formulärdata i dokumentet. Det primära arbetsflödet sammanfogar emellertid data i dokumentet.

>[!NOTE]
>
>En formulärdesign skapas vanligtvis med Designer. Mer information om hur du skapar formulärdesigner för Output-tjänsten finns i hjälpen för Designer.

När du använder Output-tjänsten för att sammanfoga XML-data med en formulärdesign blir resultatet ett icke-interaktivt PDF-dokument. Ett icke-interaktivt PDF-dokument tillåter inte att användare anger data i sina fält. Du kan däremot använda tjänsten Forms för att skapa ett interaktivt PDF-formulär där användarna kan ange data i fälten.

Följande fyra utdatatjänster kan användas:

* **generatePDFOuput**: Sammanfogar en formulärdesign med data för att generera ett PDF-dokument
* **generatePrintedOutput**: Sammanfogar en formulärdesign med formulärdata för att generera ett dokument som ska skickas till antingen en laserskrivare eller en nätverksskrivare för etiketter

* **generatePDFOutputBatch**: Sammanfogar flera mallar med flera dataposter i ett enda anrop för att generera en grupp med PDF-filer. Det finns också ett alternativ för att skapa en enda PDF genom att kombinera alla PDF-filer
* **generatePrintedOutputBatch**: Sammanfogar flera mallar med flera dataposter i ett enda anrop för att generera en grupp av utskriftsdokument (PS,PCL,ZPL,DPL,IPL,TPCL). Det finns också ett alternativ för att generera ett enda utskriftsdokument.

## Assembler Service {#assembler-service}

Med Assembler-tjänsten kan du kombinera, ordna om och förstärka PDF- och XDP-dokument och få information om PDF-dokument. Varje jobb som skickas till Assembler-tjänsten innehåller ett DX-dokument (Document Description XML), källdokument och externa resurser (strängar och grafik). DDX-dokumentet innehåller anvisningar om hur du använder källdokumenten för att skapa en uppsättning resulterande dokument.

Utöver ovanstående funktioner kan Assembler-tjänsten:

* Konverterar PDF-dokument till PDF/A-standard.
* Transformerar PDF forms, XML-formulär (skapade i Designer) och PDF forms (skapade i Acrobat) till PDF/A-1b, PDF/A-2b och PDFA/A-3b.
* Konverterar signerade eller osignerade PDF-dokument (digitala signaturer krävs).
* Validerar kompatibiliteten för en PDF/A-fil och konverterar den vid behov.

### Om DDX {#about-ddx}

När du använder Assembler-tjänsten ska du använda ett XML-baserat språk som kallas för dokumentbeskrivnings-XML (DDX) för att beskriva de utdata du vill använda. DDX är ett deklarativt kodspråk vars element representerar byggstenar i dokument. Dessa byggstenar omfattar PDF-dokument, XDP-dokument, XDP-formulärfragment och andra element som kommentarer, bokmärken och formaterad text.

DDX-dokument kan ange resulterande dokument med följande egenskaper:

* PDF-dokument som sammanställs från flera PDF-dokument
* Flera PDF-dokument som är uppdelade i ett och samma PDF-dokument
* PDF Portfolio som har ett komplett användargränssnitt och flera PDF- och icke-PDF-dokument
* XDP-dokument som sammanställs från flera XDP-dokument
* XDP-dokument som innehåller XML-fragment som infogas dynamiskt i ett XDP-dokument
* PDF-dokument som paketerar ett XDP-dokument
* XML-filer som rapporterar om ett PDF-dokuments egenskaper. De rapporterade egenskaperna omfattar text, kommentarer, formulärdata, bifogade filer, filer som används i PDF-portföljer, bokmärken och PDF-egenskaper. PDF-egenskaperna innehåller formuläregenskaper, sidrotation och dokumentförfattare.

Du kan använda DDX för att utöka PDF-dokument som en del av dokumentsammanställning eller dokumentdemontering. Du kan ange valfri kombination av följande effekter:

* Lägg till eller ta bort vattenstämplar eller bakgrunder på markerade sidor.
* Lägg till eller ta bort sidhuvuden och sidfötter på markerade sidor.
* Tar bort strukturen och navigeringsfunktionerna i ett PDF-paket eller PDF Portfolio. Resultatet blir en enda PDF-fil.
* Numrera om sidetiketter. Sidetiketter används vanligtvis för sidnumrering.
* Importera metadata från ett annat källdokument.
* Lägg till eller ta bort bifogade filer, bokmärken, länkar, kommentarer och JavaScript.
* Ange egenskaper för den inledande vyn och optimera för visning på webben.
* Ange behörigheter för krypterad PDF.
* Rotera sidor eller rotera och flytta innehåll på sidor.
* Ändra storlek på markerade sidor.
* Sammanfoga data med en PDF som är XFA-baserad.

Du kan använda en enkel indatamappning för att ange plats för källdokument och resulterande dokument. Du kan även använda följande externa data-URL-typer:

* Fil
* FTP
* HTTP/HTTPS

## Doc Assurance Service {#doc-assurance-service}

Med Doc Assurance Service kan du kryptera och dekryptera dokument, utöka funktionaliteten i Adobe Reader med ytterligare användarrättigheter och lägga in digitala signaturer i dokumenten. Användarna kan enkelt interagera med PDF forms och dokument, samtidigt som ni förbättrar säkerheten, arkiveringen och regelefterlevnaden.

Tjänsten Doc Assurance innehåller tre tjänster: signatur, kryptering och läsartillägg.

### Signaturtjänst {#signature-service}

Med signaturtjänsten kan du arbeta med digitala signaturer och dokument på AEM-servern. Signaturtjänsten används till exempel vanligtvis i följande situationer:

* AEM-servern certifierar ett formulär innan det skickas till en användare för att öppnas med Acrobat eller Adobe Reader.
* AEM-servern validerar en signatur som har lagts till i ett formulär med Acrobat eller Adobe Reader.
* AEM-servern signerar ett formulär för en offentlig notarius publicus.

Signaturtjänsten får åtkomst till certifikat och autentiseringsuppgifter som lagras i förtroendearkivet.

### Krypteringstjänst {#encryption-service}

Med krypteringstjänsten kan du kryptera och dekryptera dokument. När ett dokument är krypterat blir innehållet oläsligt. Du kan kryptera hela PDF-dokumentet (inklusive dess innehåll, metadata och bilagor), allt annat än dess metadata eller bara de bifogade filerna. En behörig användare kan dekryptera dokumentet för att få åtkomst till dess innehåll. Om ett PDF-dokument är krypterat med ett lösenord måste användaren ange det öppna lösenordet innan dokumentet kan visas i Adobe Reader eller Acrobat. Om ett PDF-dokument är krypterat med ett certifikat måste användaren dekryptera PDF-dokumentet med en privat nyckel (certifikat). Den privata nyckel som används för att dekryptera PDF-dokumentet måste motsvara den offentliga nyckel som används för att kryptera det.

### Reader Extension Service {#reader-extension-service}

Med tjänsten Reader Extensions kan man enkelt utbyta interaktiva PDF-dokument genom att utöka funktionaliteten i Adobe Reader med ytterligare användarrättigheter. Tjänsten Reader Extensions fungerar med Adobe Reader 7.0 eller senare. Tjänsten lägger till användarrättigheter i ett PDF-dokument. Den här åtgärden aktiverar funktioner som normalt inte är tillgängliga när ett PDF-dokument öppnas med Adobe Reader, t.ex. för att lägga till kommentarer i ett dokument, fylla i formulär och spara dokumentet. Tredjepartsanvändare behöver inte ytterligare programvara eller plugin-program för att kunna arbeta med upphovsrättsaktiverade dokument.

När PDF-dokument har rätt användarbehörighet kan mottagarna göra följande i Adobe Reader:

* Fyll i PDF-dokument och -blanketter online eller offline så att mottagarna kan spara kopior lokalt och behålla den information de lagt in.
* Spara PDF-dokument på en lokal hårddisk för att bevara originaldokumentet och eventuella ytterligare kommentarer, data eller bilagor
* Bifoga filer och medieklipp i PDF-dokument
* Signera, certifiera och autentisera PDF-dokument genom att använda digitala signaturer med PKI-teknik (public key infrastructure) som är branschstandard
* Skicka ifyllda eller kommenterade PDF-dokument elektroniskt
* Använd PDF-dokument och -blanketter som en intuitiv utvecklingsmiljö för interna databaser och webbtjänster
* Utbyt PDF-dokument med andra så att granskarna kan kommentera med de intuitiva kommentarverktygen. Bland dessa verktyg finns elektroniska notisar, stämplar, högdagrar och textöverstrykning. Samma funktioner är tillgängliga i Acrobat.
* Stöd för avkodning av streckkodade formulär.

Dessa specialfunktioner aktiveras automatiskt när ett rättighetsaktiverat PDF-dokument öppnas i Adobe Reader. När användaren har arbetat klart med ett rättighetsaktiverat dokument inaktiveras dessa funktioner på nytt i Adobe Reader. De är inaktiverade tills användaren får ett annat rättighetsaktiverat PDF-dokument.

Tjänsten DocAssurance är inte tillgänglig för användning. Mer information om hur du konfigurerar tjänsten DocAssurance finns i [Installera och konfigurera Document Services](../../forms/using/install-configure-document-services.md).

## Skicka till skrivartjänst {#send-to-printer-service}

Tjänsten Skicka till skrivare tillhandahåller API för att skicka dokument till angiven skrivare för utskrift.
