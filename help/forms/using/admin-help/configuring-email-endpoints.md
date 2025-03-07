---
title: Konfigurera e-postslutpunkter
description: Lär dig hur du konfigurerar e-postslutpunkter. Med e-postslutpunkter kan du anropa en tjänst genom att skicka ett eller flera dokument till ett angivet e-postkonto.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_endpoints
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 56dac3e2-e330-47c9-a32e-db947272a632
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '3808'
ht-degree: 0%

---

# Konfigurera e-postslutpunkter {#configuring-email-endpoints}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Med e-postslutpunkter kan användare anropa en tjänst genom att skicka ett eller flera dokument (som e-postbilagor) till ett angivet e-postkonto. Inkorgen för e-post fungerar som en samlingspunkt för de bifogade filerna. Tjänsten övervakar inkorgen och bearbetar de bifogade filerna. Resultatet av konverteringen vidarebefordras till den användare som är definierad i slutpunkten.

För e-postslutpunkter kan behöriga användare anropa en process genom att skicka filer till rätt konto. Resultatet returneras till den avsändande användaren (som standard) eller till den användare som definierats i slutpunktsinställningarna.

Innan du konfigurerar en e-postslutpunkt skapar du ett POP3- eller IMAP-e-postkonto som ska användas av slutpunkten. Skapa ett separat konto för varje typ av konvertering. Ett konto kan till exempel konfigureras för att generera PDF-standarddokument från inkommande bifogade filer, och ett annat konto kan konfigureras för att generera säkra PDF-dokument.

>[!NOTE]
>
>Varje e-postadress får bara mappas till en e-postslutpunkt. Du kan inte konfigurera flera e-postslutpunkter till en enda e-postadress, även om de ytterligare e-postslutpunkterna är inaktiverade.

Alla e-postslutpunkter har konfigurerats med ett auktoriserat användarnamn och lösenord för e-postinkorgen, vilket krävs när tjänsten anropas. E-postkontot skyddas av e-postserversystemet som det är konfigurerat på.

Om dina användare skickar dokument med västerländska europeiska språktecken i namn på fil- och konverteringssökvägar måste de använda ett e-postprogram som har stöd för de kodningstyper som krävs (Latin1 [ISO-8859-1], Västerländsk [Windows] eller UTF-8). Mer information finns i dokumentet *Installera och distribuera AEM-formulär* för programservern.

Konfigurera e-posttjänsten innan du konfigurerar en e-postslutpunkt. (Se [Konfigurera standardinställningar för e-postslutpunkter](configuring-email-endpoints.md#configure-default-email-endpoint-settings).) E-posttjänstens konfigurationsparametrar har två syften:

* Konfigurera attribut som är gemensamma för alla e-postslutpunkter
* Ange standardvärden för alla e-postslutpunkter

## Konfigurera SSL för en e-postslutpunkt {#configure-ssl-for-an-email-endpoint}

Du kan konfigurera POP3, IMAP eller SMTP så att Secure Sockets Layer (SSL) används för en e-postslutpunkt.

1. På e-postservern aktiverar du SSL för POP3, IMAP eller SMTP enligt tillverkarens dokumentation.
1. Exportera ett klientcertifikat från e-postservern.
1. Använd nyckelverktygsprogrammet för att importera klientcertifikatfilen till programserverns JVM-certifikatarkiv (Java Virtual Machine). Hur det här steget utförs beror på sökvägarna till JVM och klientinstallation.

   Om du till exempel använder en standardinstallation av Oracle WebLogic Server med JDK 1.5.0 i Microsoft Windows Server® 2003 skriver du följande text i en kommandotolk:

   `keytool -import -file client_certificate -alias myalias -keystore BEA_HOME\jdk150_04\jre\security\cacerts`

1. Ange lösenordet när du uppmanas till det (för Java är standardlösenordet `changeit`). Du får ett meddelande om att certifikatet har importerats.
1. Använd administrationskonsolen för att lägga till e-postslutpunkten till tjänsten.
1. Skapa e-postslutpunkten i administrationskonsolen. När du konfigurerar slutpunktsinställningarna väljer du POP3/IMAP SSL aktiverat för inkommande meddelanden och SMTP SSL aktiverat för utgående meddelanden, och ändrar portegenskaperna därefter.

>[!NOTE]
>
>Tips! Om du får problem med SSL kan du använda en e-postklient som Microsoft Outlook för att kontrollera om den har åtkomst till e-postservern med SSL. Om e-postklienten inte har åtkomst till e-postservern är problemet relaterat till konfigurationen av antingen ditt certifikat eller e-postservern.

## Konfigurera standardinställningar för e-postslutpunkt {#configure-default-email-endpoint-settings}

Du kan använda sidan Tjänsthantering för att konfigurera attribut som är gemensamma för alla e-postslutpunkter och för att ange standardvärden för alla e-postslutpunkter.

För att formulärarbetsflöden ska kunna ta emot och hantera inkommande e-postmeddelanden från användare måste du skapa en e-postslutpunkt för tjänsten Complete Task. Den här e-postslutpunkten kräver ytterligare inställningar, vilket beskrivs i [Skapa en e-postslutpunkt för tjänsten Fullständig aktivitet](configuring-email-endpoints.md#create-an-email-endpoint-for-the-complete-task-service).

### Ändra standardvärden för e-postslutpunkter {#change-the-default-values-for-email-endpoints}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. På sidan Tjänsthantering klickar du på E-post: 1.0 (komponent-ID:t är com.adobe.idp.dsc.provider.service.email.Email).
1. Ange standardinställningarna för e-postslutpunkten på fliken Konfiguration och klicka sedan på Spara.

### Standardinställningar för e-postslutpunkt {#default-email-endpoint-settings}

**Kronuttryck:** Kronuttrycket som används av kvarts för att schemalägga avsökningen av indatakatalogen.

**Intervall för upprepning:** Antalet gånger som katalogavsökningen upprepas. Standardintervallet för upprepning är i sekunder om det här värdet inte anges i slutpunktskonfigurationen. Standardvärdet är 10. Värdet får inte vara mindre än 10.

**Antal upprepningar:** Antalet gånger som indatakatalogen avsöks. Standardantalet för upprepning som ska användas om det här värdet inte anges i slutpunktskonfigurationen. Värdet -1 anger att katalogen genomsöks oändligt. Standardvärdet är -1.

**Fördröjning när jobbet startar:** Standardvärdet, i sekunder, för fördröjningen innan jobbet börjar genomsöka slutpunkten. Standardvärdet är 0.

**Batchstorlek:** Antalet e-postmeddelanden som mottagarprocesserna utför per sökning för optimala prestanda. Värdet -1 anger alla e-postmeddelanden. Standardvärdet är 2.

**Asynkron:** Identifierar anropstypen som asynkron eller synkron. Övergående och synkrona processer kan bara anropas synkront. Standardvärdet är asynkront.

**Domänmönster:** Domännamnsmönstret som används för att filtrera inkommande e-post. Om du till exempel använder adobe.com kommer endast e-post från adobe.com att bearbetas, och e-post från andra domäner ignoreras.

**Filmönster:** Inkommande mönster för bifogade filer som accepteras av providern. Detta inkluderar filer som har specifika tillägg (&amp;ast;.dat, &amp;ast;.xml), specifika namn (data) och sammansatta uttryck i namnet och tillägget (.[dD][aA]&#39;port&#39;). Standardvärdet är &amp;ast;.&amp;ast;..

**Jobbets mottagare lyckades:** En eller flera e-postadresser som används för att skicka e-post för att ange slutförda jobb. Som standard skickas alltid ett meddelande om att jobbet lyckades till avsändaren av det ursprungliga jobbet. Stöd för upp till 100 mottagare. Om du vill inaktivera den här inställningen lämnar du det här fältet tomt.

**Jobbets mottagare misslyckades:** En eller flera e-postadresser som används för att skicka e-post för att ange misslyckade jobb. Som standard skickas alltid ett meddelande om misslyckat jobb till avsändaren som skickade det ursprungliga jobbet. Stöd för upp till 100 mottagare. Om du vill inaktivera den här inställningen lämnar du det här fältet tomt.

**Inkorgsvärd:** Inkorgsvärdnamnet eller IP-adressen för e-postprovidern som ska genomsökas.

**Inkorgsport:** Inkorgsportnumret för e-postprovidern som ska genomsökas. Om värdet är 0 används IMAP- eller POP3-standardporten.

**Inkorgsprotokoll:** E-postprotokollet för e-postslutpunkten som ska användas för att skanna inkorgen. Alternativen är IMAP eller POP3. Inkorgens värdserver för e-post måste stödja dessa protokoll.

**Inkorgstimeout:** Anger hur lång tid slutpunkten ska vänta innan den avbryts när den försöker ansluta till inkorgen. Om ingen anslutning hämtas innan timeout-värdet har nåtts avsöks inte inkorgen.

**Inkorgsanvändare:** Användarnamnet som krävs för att logga in på e-postkontot. Beroende på e-postservern och konfigurationen kan det här namnet endast vara användarnamnsdelen i e-postmeddelandet eller den fullständiga e-postadressen.

**Inkorgslösenord:** Lösenordet för användaren i Inkorgen.

**POP3/IMAP SSL är aktiverat:** Aktivera SSL när det är markerat.

**SMTP-värd:** Värdnamnet på e-postservern som e-postprovidern använder för att skicka resultat och felmeddelanden. Exempel: mail.example.com.

**SMTP-port:** Den port som används för att ansluta till e-postservern. Standardvärdet är 25.

**SMTP-användare:** Användarkontot som e-postprovidern ska använda när den skickar e-post för resultat och fel.

**SMTP-lösenord:** Lösenordet för SMTP-kontot. Vissa e-postservrar kräver inget SMTP-lösenord.

**Skicka från:** E-postadressen (till exempel user@company.com) som används för att skicka e-postmeddelanden om resultat och fel. Om du inte anger något Skicka från-värde försöker e-postservern att fastställa e-postadressen genom att kombinera värdet som anges i inställningen SMTP-användare med en standarddomän som konfigurerats på e-postservern. Om e-postservern inte har någon standarddomän och du inte anger något värde för Skicka från, kan fel uppstå. Om du vill vara säker på att e-postmeddelandena har rätt från-adress anger du ett värde för inställningen Skicka från.

**SMTP SSL är aktiverat:** Om det här alternativet är markerat aktiveras SSL över SMTP.

**Inkludera den ursprungliga e-posttexten som en bifogad fil:** När du skickar ett e-postmeddelande till Forms Server inkluderas den ursprungliga texten i meddelandet som standard. Välj det här alternativet om du i stället vill ta med texten som en bifogad fil.

**Använd den ursprungliga ämnesraden för resultatmeddelanden:** Som standard använder Forms-servern de värden som anges i inställningarna Ämne för e-post och Ämne för fel som ämnesrad när resultatmeddelanden skickas. Välj det här alternativet om du i stället vill använda samma ämnesrad som det ursprungliga e-postmeddelandet som skickades till servern.

**Ämne för lyckad e-post:** När du har skickat ett e-postmeddelande till en e-postslutpunkt för att starta eller fortsätta en process får du ett returnerat e-postmeddelande från AEM Forms Server. Om e-postmeddelandet lyckas får du ett e-postmeddelande om att det lyckades. Om e-postmeddelandet misslyckas får du ett felmeddelande som informerar om varför det misslyckades. Med den här inställningen kan du ange ämnesraden för e-postmeddelanden om lyckade åtgärder som skickas för den här slutpunkten.

**E-postbrödtext som slutförs:** Gör att du kan ange brödtexten i e-postmeddelanden som skickas för den här slutpunkten.

**Fel Ämnesprefix för e-post:** Gör att du kan ange texten som används i början av ämnesraden i e-postmeddelanden om fel som skickas för den här slutpunkten.

**Felmeddelandeämne:** Du kan ange ämnesraden för e-postmeddelanden om fel som skickas för den här slutpunkten. Den här texten visas efter Error Email Subject Prefix.

**Felmeddelandetext:** Gör att du kan ange den första raden i brödtexten för felmeddelanden som skickas för den här slutpunkten.

**Sammanfattningsinformation för e-post:** Varje meddelande om att åtgärden lyckades eller misslyckades innehåller ett avsnitt med den ursprungliga e-posttexten som du skickade till Forms Server. Den här inställningen anger texten som visas ovanför avsnittet.

**Verifiera inkorgen innan den här slutpunkten skapas/uppdateras:** När det här alternativet är markerat kontrollerar Forms Server om dina SMTP/POP3-inställningar är korrekta innan slutpunkten skapas. När du klickar på Lägg till visas ett meddelande som anger om inkorgskontot är giltigt. Om det här alternativet inte är markerat skapas slutpunkten utan att inkorgen valideras.

**Teckenuppsättningskodning:** Det kodningsformat som ska användas för e-postmeddelandet. Standardvärdet är UTF-8, som de flesta användare utanför Japan kommer att använda. Användare i en japansk miljö kan välja ISO2022-JP.

**Det gick inte att skicka e-post:** Anger en katalog där resultaten ska lagras om SMTP-e-postservern inte fungerar.

## Inställningar för e-postslutpunkt {#email-endpoint-settings}

Använd följande inställningar för att konfigurera en e-postslutpunkt.

**Namn:** En obligatorisk inställning som identifierar slutpunkten. Ta inte med ett &lt;-tecken eftersom namnet som visas i Workspace trunkeras. Om du anger en URL som namn på slutpunkten kontrollerar du att den överensstämmer med de syntaxregler som anges i RFC1738.

**Beskrivning:** En beskrivning av slutpunkten. Ta inte med ett &lt;-tecken eftersom det kortar av beskrivningen som visas i Workspace.

**Cron-uttryck:** Ange ett cron-uttryck om e-postmeddelandet måste schemaläggas med ett cron-uttryck.

**Antal upprepningar:** Antal gånger som e-postslutpunkten söker igenom mappen eller katalogen. Värdet -1 anger oändlig skanning. Standardvärdet är -1.

**Intervall för upprepning:** Den skanningsfrekvens som mottagaren använder för att kontrollera inkommande e-post.

**Fördröjning när jobbet startar:** Den tid det tar att vänta på genomsökningen efter att schemaläggaren har startats.

**Batchstorlek:** Antalet e-postmeddelanden som mottagarprocesserna utför per sökning för optimala prestanda. Värdet -1 anger alla e-postmeddelanden. Standardvärdet är 2.

**Användarnamn:** En obligatorisk inställning, som är det användarnamn som används när en måltjänst anropas från e-post. Standardvärdet är SuperAdmin.

**Domännamn:** En obligatorisk inställning som är användarens domän. Standardvärdet är DefaultDom.

**Domänmönster:** Anger domänmönster för inkommande e-post som accepteras av providern. Om du till exempel använder adobe.com bearbetas bara e-post från adobe.com. E-post från andra domäner ignoreras.

**Filmönster:** Anger mönster för inkommande bifogade filer som accepteras av providern. Detta inkluderar filer som har specifika tillägg (&amp;ast;.dat, &amp;ast;.xml), specifika namn (data) eller sammansatta uttryck i namnet och tillägget (&amp;ast;.[dD][aA]&#39;port&#39;).

**Slutförda jobbmottagare:** En e-postadress dit meddelanden skickas för att ange slutförda jobb. Som standard skickas alltid ett meddelande om att jobbet lyckades till avsändaren. Om du skriver avsändare skickas e-postresultaten till avsändaren. Stöd för upp till 100 mottagare. Ange ytterligare mottagare med e-postadresser, avgränsade med kommatecken (,).

Om du vill inaktivera den här inställningen lämnar du inställningen tom. I vissa fall vill du utlösa en process och inte få ett e-postmeddelande om resultatet.

**Jobbets mottagare misslyckades:** En e-postadress dit meddelanden skickas för att ange misslyckade jobb. Som standard skickas alltid ett meddelande om misslyckat jobb till avsändaren. Om du skriver avsändare skickas e-postresultaten till avsändaren. Stöd för upp till 100 mottagare. Ange ytterligare mottagare med e-postadresser, avgränsade med kommatecken (,).

Om du vill inaktivera den här inställningen lämnar du inställningen tom. I vissa fall vill du utlösa en process och inte få ett e-postmeddelande om resultatet.

**Inkorgsvärd:** Inkorgsvärdnamnet eller IP-adressen för e-postprovidern som ska genomsökas.

**Inkorgsport:** Den port som e-postservern använder. Standardvärdet för POP3 är 110 och standardvärdet för IMAP är 143. Om SSL är aktiverat är standardvärdet för POP3 995 och standardvärdet för IMAP är 993.

**Inkorgsprotokoll:** E-postprotokollet för e-postslutpunkten som ska användas för att skanna inkorgen. Värdena är IMAP eller POP3. Inkorgens värdserver för e-post måste stödja dessa protokoll.

**Inkorgstimeout:** Tidsgränsen i sekunder för e-postprovidern att vänta på inkorgssvar.

**Inkorgsanvändare:** Användarnamnet som krävs för att logga in på e-postkontot. Beroende på e-postservern och konfigurationen kan det här värdet vara endast användarnamnsdelen i e-postmeddelandet eller den fullständiga e-postadressen.

**Inkorgslösenord:** Inkorgsanvändarens lösenord.

**POP3/IMAP SSL har aktiverats:** Välj den här inställningen för att tvinga e-postprovidern att använda SSL för att skanna inkorgen. Kontrollera att e-postservern stöder SSL.

**SMTP-värd:** Värdnamnet på e-postservern som e-postprovidern använder för att skicka resultat och felmeddelanden.

**SMTP-port:** Standardvärdet för SMTP-porten är 25.

**SMTP-användare:** E-postleverantörens användarkonto som ska användas när e-postmeddelanden om resultat och fel skickas.

**SMTP-lösenord:** Lösenordet för SMTP-kontot. Vissa e-postservrar kräver inget SMTP-lösenord.

**Skicka från:** E-postadressen (till exempel user@company.com) som används för att skicka e-postmeddelanden om resultat och fel. Om du inte anger något Skicka från-värde försöker e-postservern att fastställa e-postadressen genom att kombinera värdet som anges i inställningen SMTP-användare med en standarddomän som konfigurerats på e-postservern. Om e-postservern inte har någon standarddomän och du inte anger något värde för Skicka från, kan fel uppstå. Om du vill vara säker på att e-postmeddelandena har rätt från-adress anger du ett värde för inställningen Skicka från.

**SMTP SSL är aktiverat:** Välj den här inställningen för att tvinga e-postprovidern att använda SSL för att skanna inkorgen. Kontrollera att e-postservern stöder SSL.

**Det gick inte att skicka e-post:** Anger en katalog där resultaten ska lagras om SMTP-e-postservern inte fungerar.

**asynkron:** Om den är synkron bearbetas alla indatadokument och ett enda svar returneras. När inställningen är asynkron skickas ett svar för varje dokument som bearbetas.

En e-postslutpunkt skapas till exempel för en tjänst som tar ett enstaka Word-dokument och returnerar det dokumentet som en PDF-fil. Ett e-postmeddelande kan skickas till slutpunktens inkorg som innehåller flera (3) Word-dokument. När alla tre dokument har bearbetats och slutpunkten har konfigurerats som synkron, skickas ett e-postmeddelande med alla tre bifogade dokument. Om slutpunkten är asynkron skickas ett e-postmeddelande när varje Word-dokument har konverterats till PDF. Resultatet är tre e-postmeddelanden, vart och ett med en enda bifogad PDF-fil.

Standardvärdet är asynkront.

**Inkludera det ursprungliga e-postmeddelandet som en bifogad fil:** När du skickar ett e-postmeddelande till Forms Server inkluderas den ursprungliga texten i meddelandet som standard. Välj det här alternativet om du i stället vill ta med texten som en bifogad fil.

**Använd den ursprungliga ämnesraden för resultatmeddelanden:** Som standard använder Forms-servern de värden som anges i inställningarna Ämne för lyckad e-post och Ämne för fel som ämnesrad när resultatmeddelanden skickas. Välj det här alternativet om du i stället vill använda samma ämnesrad som det ursprungliga e-postmeddelandet som skickades till servern.

**Ämne för lyckad e-post:** När du har skickat ett e-postmeddelande till en e-postslutpunkt för att starta eller fortsätta en process får du ett returnerat e-postmeddelande från AEM Forms Server. Om e-postmeddelandet lyckas får du ett e-postmeddelande om att det lyckades. Om e-postmeddelandet misslyckas får du ett felmeddelande som informerar om varför det misslyckades. Med den här inställningen kan du ange ämnesraden för e-postmeddelanden om lyckade åtgärder som skickas för den här slutpunkten.

**E-postbrödtext som slutförs:** Gör att du kan ange brödtexten i e-postmeddelanden som skickas för den här slutpunkten.

**Fel Ämnesprefix för e-post:** Gör att du kan ange texten som används i början av ämnesraden i e-postmeddelanden om fel som skickas för den här slutpunkten.

**Felmeddelandeämne:** Du kan ange ämnesraden för e-postmeddelanden om fel som skickas för den här slutpunkten. Den här texten visas efter Error Email Subject Prefix.

**Felmeddelandetext:** Gör att du kan ange den första raden i brödtexten för felmeddelanden som skickas för den här slutpunkten.

**Sammanfattningsinformation för e-post:** Varje meddelande om att åtgärden lyckades eller misslyckades innehåller ett avsnitt med den ursprungliga e-posttexten som du skickade till Forms Server. Den här inställningen anger texten som visas ovanför avsnittet.

**Verifiera inkorgen innan du skapar/uppdaterar den här slutpunkten:** När det här alternativet är markerat kontrollerar Forms Server om dina SMTP/POP3-inställningar är korrekta innan slutpunkten skapas. När du klickar på Lägg till visas ett meddelande som anger om inkorgskontot är giltigt. Om det här alternativet inte är markerat skapas slutpunkten utan att inkorgen valideras.

**Åtgärdsnamn:** Den här inställningen är obligatorisk. En lista över åtgärder som kan tilldelas e-postslutpunkten. Den åtgärd som du väljer här avgör vilka fält som visas i avsnitten Mappningar av indataparameter och Utdataparameter.

**Mappningar av indataparameter:** Används för att konfigurera indata som krävs för att bearbeta tjänsten och åtgärden. De två typerna av indata är literala och variabla:

**Litteral:** E-postmeddelandet använder det värde som anges i fältet när det visas.

**Variabel:** Du kan mappa en sträng från e-postmeddelandets ämne, brödtext, huvud eller avsändarens e-postadress. Om du vill göra det använder du något av följande nyckelord: %SUBJECT%, %BODY%, %HEADER% eller %SENDER%. Om du till exempel använder %SUBJECT% används innehållet i e-postämnet som indataparameter. Om du vill hämta bifogade filer anger du ett filmönster som e-postslutpunkten kan använda för att välja bifogade dokument. Om du till exempel anger &amp;ast;.pdf väljs alla bifogade dokument som har filnamnstillägget .pdf. Ange &amp;stämpel;ast; markerar ett bifogat dokument. Om du anger example.pdf väljs alla bifogade dokument som heter example.pdf.

**Mappningar av utdataparameter:** Används för att konfigurera utdata för tjänsten och åtgärden. Följande tecken i mappningsvärdena för utdataparametrar utökas i filnamnet för den bifogade filen:

**%F** Representerar källfilens filnamn (inte ett tillägg).

**%E** Representerar källfilens tillägg.

Alla förekomster av det omvända snedstrecket (\) ersätts med %%.

***Obs!**Om tjänstbegärandemeddelandet innehåller flera bifogade filer kan du inte använda parametrarna %F och %E för egenskapen Mappningar av utdataparameter för slutpunkten. Om tjänstsvaret returnerar flera bifogade filer kan du inte ange samma filnamn för fler än en bifogad fil. Om du inte följer dessa rekommendationer skapar den anropade tjänsten namnen för de returnerade filerna, och namnen är inte förutsägbara.*

Följande värden är tillgängliga:

**Ett objekt:** E-postprovidern har inte källmappens mål. Resultat returneras som bilagor. Mönstret är Result/%F.ps och returnerar Result%%sourcefilename.ps som bifogad fil.

**Lista:** Mönstret är Result/%F/ och returnerar Result%%sourcefilename%%file1 som bifogad fil.

**Karta:** Mönstret är Result/%F/ och källmålet är Result%%sourcefilename%%file1 och Result%%sourcefilename%%file2. Om kartan innehåller mer än ett objekt och mönstret är Result/%F.ps, är de bifogade svarsfilerna Result%%sourcefilename1.ps (output 1) och Result%%sourcefilename2.ps (output 2).

## Skapa en e-postslutpunkt för tjänsten Slutför uppgift {#create-an-email-endpoint-for-the-complete-task-service}

För att formulärarbetsflöden ska kunna ta emot och hantera inkommande e-postmeddelanden från användare måste du skapa en e-postslutpunkt för tjänsten Complete Task.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. På sidan Tjänsthantering klickar du på tjänsten Complete Task.
1. Välj E-post i listrutan på fliken Slutpunkter och klicka på Lägg till.
1. Ange värdnamnet eller IP-adressen för e-postservern i rutan Inkorgsvärd.
1. I rutan Inkorgsanvändare skriver du det användarnamn som krävs för att logga in på det e-postkonto som du skapade för att hantera formuläröverföringar. Beroende på e-postservern och konfigurationen kan det här namnet endast vara användarnamnsdelen i e-postmeddelandet eller den fullständiga e-postadressen.
1. Skriv lösenordet för Inkorgen-användaren i rutan Inkorgslösenord.
1. I rutan SMTP-värd anger du värdnamnet eller IP-adressen för den e-postserver som e-postleverantören skickar resultat- och felmeddelanden från.
1. I rutan SMTP-användare anger du användarkontot för e-postprovidern som ska användas när den skickar e-post för resultat och fel. Det här användarkontot kan vara samma värde som du använde för Inkorgen.
1. Ange lösenordet för SMTP-kontot i rutan SMTP-lösenord.
1. Välj Anropa i listan Åtgärdsnamn.
1. Välj Variabel i listan attachmentMap och skriv `*.*` i den intilliggande rutan. Detta skickar alla bilagor från inkommande e-postmeddelanden till en mappningsvariabel för processen Slutför uppgift.
1. I listan mailBody väljer du variabeln och skriver `%BODY%` i den intilliggande rutan.
1. Välj Variabel i listan mailFrom och skriv `%SENDER%` i den intilliggande rutan. Detta mappar avsändaradressen till processdata för en fullständig uppgift.
1. Skriv `results` i resultatrutan. Detta gör att en resultatsträng returneras av den fullständiga uppgiften eller Starta process.
1. Klicka på Lägg till.
