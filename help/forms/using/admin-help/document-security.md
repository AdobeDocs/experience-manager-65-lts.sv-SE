---
title: Vad är dokumentsäkerhet?
description: Lär dig hur du kan skapa, lagra och använda fördefinierade sekretessinställningar och distribuera information på ett säkert sätt med dokumentsäkerhet.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 5e7fe85e-3c7f-4a37-8f65-5c0ad4bbd66c
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '3219'
ht-degree: 0%

---

# Om dokumentsäkerhet {#about-document-security}

Dokumentsäkerheten säkerställer att bara behöriga användare kan använda dina dokument. Med dokumentsäkerhet kan du distribuera all information som du har sparat i ett format som stöds. Filformat som stöds är:

* Adobe PDF-filer
* Microsoft® Word, Excel och PowerPoint

Mer information om hur profiler skyddar filtyper som stöds finns i [mer dokumentsäkerhetsinformation](https://experienceleague.adobe.com/docs/experience-manager-65-lts/forms/use-document-security/document-security-offerings.html?lang=en).

Med dokumentsäkerhet kan du enkelt skapa, lagra och använda fördefinierade sekretessinställningar i dina dokument. För att förhindra att information sprids utanför räckhåll kan du också övervaka och styra hur mottagarna använder dokumenten när du har distribuerat dem.

Du kan skydda dokument med hjälp av profiler. En *princip* är en samling information som innehåller sekretessinställningar och en lista över behöriga användare. De sekretessinställningar som du anger i en profil avgör hur en mottagare kan använda ett dokument som du tillämpar profilen på. Du kan till exempel ange om mottagarna ska kunna skriva ut eller kopiera text, redigera text eller lägga till signaturer och kommentarer i skyddade dokument.

Dokumentsäkerhetsanvändare skapar profiler via slutanvändarens webbsidor. Administratörer använder webbsidorna för dokumentsäkerhet för att skapa profiluppsättningar som innehåller delade profiler som är tillgängliga för alla behöriga användare.

Även om profiler lagras i dokumentsäkerhet kan du tillämpa dem på dokument via ditt klientprogram. Hur du tillämpar profiler på PDF-dokument beskrivs i detalj i *Acrobat-hjälpen*. Regler som tillämpas med andra program, t.ex. Microsoft® Office, beskrivs i *hjälpen för Acrobat Reader DC-tillägg* för programmet.

När du tillämpar en profil på ett dokument skyddar sekretessinställningarna som anges i profilen den information som dokumentet innehåller. Sekretessinställningarna skyddar även alla filer (text, ljud eller video) i ett PDF-dokument. Du kan distribuera det profilskyddade dokumentet till mottagare som har behörighet enligt profilen.

**Åtkomstkontroll och granskning av dokument**

Genom att använda en skyddsprofil får du full kontroll över dokumentet, även sedan du distribuerat det. Du kan övervaka dokumentet, ändra profilen, hindra användare från att fortsätta få åtkomst till dokumentet och växla den profil som tillämpas på dokumentet.

Med dokumentsäkerhet kan du övervaka skyddsskyddade dokument och spåra händelser, t.ex. när en behörig eller obehörig användare försöker öppna dokumentet.

**Komponenter**

Dokumentsäkerhet består av en server och ett användargränssnitt:

**Server:** Den centrala komponent genom vilken dokumentsäkerhet utför transaktioner som användarautentisering, hantering av principer i realtid och tillämpning av sekretess. Servern har också ett centralt arkiv för policyer, granskningsposter och annan relaterad information.

**Webbsidor:** Gränssnittet där du skapar principer, hanterar dina profilskyddade dokument och övervakar händelser som är kopplade till principskyddade dokument. Administratörer kan också konfigurera globala alternativ som användarautentisering, granskning och meddelanden för inbjudna användare samt hantera inbjudna användarkonton.

![rm_psworkflow](assets/rm_psworkflow.png)

Stegen i bilden är följande:

1. Dokumentägaren skapar profiler med hjälp av webbsidorna. Dokumentägare kan skapa egna profiler som bara är tillgängliga för dem. Administratörer och koordinatorer för principuppsättningar kan skapa delade principer i principuppsättningar som är tillgängliga för behöriga användare.
1. Dokumentägaren tillämpar profilen och sparar och distribuerar sedan dokumentet. Dokumentet kan distribueras via e-post, via en nätverksmapp eller på en webbplats.
1. Mottagaren öppnar dokumentet i lämpligt klientprogram. Mottagaren kan använda dokumentet enligt sin policy.
1. Dokumentägaren, principuppsättningskoordinatorn eller administratören kan spåra dokument och ändra åtkomsten till dem via webbsidorna.

## Om dokumentsäkerhetsanvändare {#about-document-security-users}

Olika typer av användare arbetar med dokumentsäkerhet för att utföra olika uppgifter:

* Systemadministratören eller annan person som arbetar med informationssystem installerar och konfigurerar dokumentsäkerhet. Den här personen kan även vara ansvarig för att konfigurera globala inställningar för servern, webbsidor, profiler och dokument.

  Dessa inställningar kan t.ex. innehålla en grundläggande säkerhets-URL, gransknings- och sekretessmeddelanden, inbjudna användarregistreringsmeddelanden och standardavtalsperioder.

* Dokumentsäkerhetsadministratörer kan skapa profiler och uppsättningar av profiler samt hantera profilskyddade dokument för användare efter behov. De skapar även inbjudna användarkonton och övervakar system, dokument, användare, policy, principuppsättning och anpassade händelser. De kan också ansvara för att konfigurera den globala servern samt webbsidan och principinställningarna med en systemadministratör.

  Administratörer kan tilldela användare följande roller i området för användarhantering i administrationskonsolen. Användare som tilldelas de här rollerna utför sina uppgifter i dokumentets säkerhetsgränssnitt i administrationskonsolen.

  **Superadministratör för dokumentsäkerhet**

  Användare med den här rollen har åtkomst till alla dokumentsäkerhetsinställningar i administrationskonsolen. Dessa behörigheter är associerade med rollen:

   * Hantera konfiguration
   * Hantera princip
   * Hantera principuppsättningar
   * Hantera dokument
   * Hantera dokumentutgivare
   * Hantera inbjudna och lokala användare
   * Visa händelser
   * Delegera
   * Bjud in externa användare

  **Dokumentsäkerhetsadministratör**

  Användare med den här rollen kan konfigurera dokumentsäkerhetsservern med hjälp av sidan Konfiguration i dokumentsäkerhetsavsnittet i administrationskonsolen. Den här behörigheten är associerad med rollen Hantera konfiguration.

  >[!NOTE]
  >
  >Användare med den här rollen måste också ha administratörskonsolens användarroll för att kunna logga in på administrationskonsolen och redigera alla konfigurationsrelaterade inställningar.

  **Ställ in administratör för dokumentsäkerhetsprincip**

  Användare med den här rollen kan använda dokumentsäkerhetsdelen i administrationskonsolen för att redigera andra användares profiler och för att skapa, redigera och ta bort principuppsättningar. När en administratör för en principuppsättning skapar en principuppsättning kan de tilldela en principuppsättningskoordinator till den uppsättningen. Dessa behörigheter är associerade med rollen:

   * Hantera princip
   * Hantera principuppsättningar
   * Hantera dokument
   * Hantera dokumentutgivare
   * Visa händelser
   * Delegera

  >[!NOTE]
  >
  >Användare med den här rollen måste också ha administratörskonsolens användarroll för att kunna logga in på administrationskonsolen och redigera alla konfigurationsrelaterade inställningar.

  **Dokumentsäkerhet hanterar inbjudna och lokala användare**

  Användare med den här rollen kan utföra de uppgifter som krävs för att hantera alla inbjudna och lokala användare på relevanta dokumentsäkerhetswebbsidor. Dessa behörigheter är associerade med rollen:

   * Hantera inbjudna och lokala användare
   * Bjud in externa användare
   * Åtkomst till slutanvändarens webbsidor

  >[!NOTE]
  >
  >Användare med den här rollen måste också ha administratörskonsolens användarroll för att kunna logga in på administrationskonsolen och redigera alla konfigurationsrelaterade inställningar.

  **Inbjudan till dokumentsäkerhet - användare**

  Användare med den här rollen kan bjuda in användare. Dessa behörigheter är associerade med rollen:

   * Bjud in externa användare
   * Åtkomst till slutanvändarens webbsidor

  **Slutanvändare för dokumentsäkerhet**

  Användare med den här rollen har åtkomst till slutanvändarens webbsidor för dokumentsäkerhet. Den här rollen kan även tilldelas administratörer så att administratörer kan skapa profiler med hjälp av slutanvändarsidorna. Den här behörigheten är associerad med rollen Åtkomst till slutanvändarens webbsidor.

* Användare inom organisationen som har giltiga dokumentsäkerhetskonton skapar egna profiler, använder profiler för att skydda dokument, spåra och hantera sina profilskyddade dokument och övervakar händelser som är relaterade till deras dokument.
* Koordinatorer för principuppsättningar hanterar dokument, visar händelser och hanterar andra koordinatorer för principuppsättningar (baserat på deras behörigheter). Administratörer utser användare till principuppsättningskoordinatorer för särskilda principuppsättningar.
* Användare som är externa för din organisation (t.ex. en affärspartner) kan använda profilskyddade dokument om de finns i dokumentets säkerhetskatalog, om administratören skapar ett konto åt dem eller om de registrerar sig för dokumentsäkerhet via en automatiserad e-postinbjudan. Beroende på hur administratören aktiverar åtkomstinställningarna kan de inbjudna användarna även ha behörighet att tillämpa profiler på dokument, att skapa, ändra och ta bort sina profiler samt bjuda in andra externa användare att använda sina profilskyddade dokument.
* Utvecklare använder AEM Forms SDK för att integrera anpassade program med dokumentsäkerhet.

Administratörer för dokumentsäkerhet kan skapa anpassade roller med följande behörigheter i Användarhantering:

* Konfiguration av hantering av dokumentsäkerhet
* Dokumentsäkerhet Hantera inbjudna och lokala användare
* Dokumentsäkerhet Hantera principuppsättningar
* Dokumentsäkerhet Hantera principuppsättningar
* Vyserverhändelser för dokumentsäkerhet
* Ägare av dokumentskyddsändringsprincip

## Profiler och policyskyddade dokument {#policies-and-policy-protected-documents}

En *princip* definierar en uppsättning sekretessinställningar och användare som kan komma åt ett dokument som principen tillämpas på. En profil gör det även möjligt att ändra behörigheter för ett dokument dynamiskt. Den person som skyddar dokumentet får behörighet att ändra sekretessinställningarna för att återkalla åtkomst till dokumentet eller för att ändra profilen.

Policyskydd kan tillämpas på ett PDF-dokument med Adobe Acrobat® Pro och Acrobat Standard. Skyddet av profilen kan tillämpas på andra filtyper, t.ex. Microsoft® Word-, Excel- och PowerPoint-filer, genom att använda klientprogrammet med rätt Acrobat Reader DC-tillägg installerat.

### Hur policyer fungerar {#how-policies-work}

Profilerna innehåller information om behöriga användare och sekretessinställningar som ska gälla för dokumenten. Användare kan vara vem som helst i din organisation och personer som är externa i din organisation och som har ett konto. Om administratören aktiverar funktionen för användarinbjudan går det till och med att lägga till nya användare i profiler, vilket initierar en e-postprocess för registreringsinbjudan.

Sekretessinställningarna i en profil avgör hur mottagarna kan använda dokumentet. Du kan till exempel ange om mottagarna ska kunna skriva ut eller kopiera text, göra ändringar eller lägga till signaturer och kommentarer i skyddade dokument. Samma policy kan även ange olika sekretessinställningar för specifika användare.

>[!NOTE]
>
>Sekretessinställningar som tillämpas via en profil åsidosätter inställningar som kan ha tillämpats på ett PDF-dokument i Acrobat med hjälp av säkerhetsalternativen för lösenord och certifikat. (Mer information finns i hjälpen för Acrobat.)

Användare och administratörer kan skapa profiler via webbsidorna för dokumentsäkerhet. Endast en profil åt gången kan tillämpas på ett dokument. Du kan tillämpa en profil på något av följande sätt:

* Öppna dokumentet i Acrobat eller något annat klientprogram och välj en skyddsprofil för dokumentet.
* Skicka ett dokument som en e-postbilaga i Microsoft® Outlook. I så fall kan du välja en profil i en lista med profiler. Du kan också välja en automatiskt genererad profil som Acrobat skapar med en standarduppsättning med sekretessinställningar för att skydda dokumentet enbart för e-postmeddelandets mottagare.

En profil kan tas bort från ett dokument med hjälp av klientprogrammet.

![rm_psonline_policy](assets/rm_psonline_policy.png)

Stegen i diagrammet är följande:

1. Dokumentägaren skyddar dokumentet från ett klientprogram som stöds med en profil som tillåter onlineanvändning.
1. Dokumentsäkerhet skapar en dokumentlicens och dokumentnycklar och krypterar profilen. Dokumentlicensen, den krypterade regeln och dokumentnyckeln returneras till klientprogrammet.
1. Dokumentet krypteras med dokumentnyckeln och dokumentnyckeln ignoreras. Dokumentet bäddar nu in licensen och policyn. Dessa åtgärder utförs i det klientprogram som stöds.

När du tillämpar en profil på ett dokument skyddas informationen i det, inklusive eventuella filer (text, ljud eller video) i PDF-dokument, av de sekretessinställningar som anges i profilen. Dokumentskydd genererar licens- och krypteringsinformation som sedan bäddas in i dokumentet. När du distribuerar dokumentet kan dokumentskyddet autentisera de mottagare som försöker öppna dokumentet och ge behörighet enligt de behörigheter som anges i profilen.

Om offlineanvändning är aktiverad kan mottagarna även använda principskyddade dokument offline (utan en aktiv Internet- eller nätverksanslutning) under den tidsperiod som anges i profilen.

### Hur policyskyddade dokument fungerar {#how-policy-protected-documents-work}

Om du vill öppna och använda profilskyddade dokument måste profilen innehålla ditt namn som mottagare och du måste ha ett giltigt dokumentsäkerhetskonto. För PDF-dokument behöver du Acrobat eller Adobe Reader®. För andra filtyper måste du ha rätt program för filen med Acrobat Reader DC Extensions installerat.

När du öppnar ett policyskyddat dokument ansluter Acrobat, Adobe Reader eller Acrobat Reader DC Extensions till dokumentskyddet för att autentisera dig. Sedan kan du logga in. Om dokumentanvändningen granskas visas ett meddelande. När dokumentskyddet har fastställt vilka dokumentbehörigheter som ska beviljas, hanteras dekrypteringen av dokumentet. Du kan sedan använda dokumentet enligt inställningarna för sekretesspolicyn.

![rm_psopen_online](assets/rm_psopen_online.png)

Stegen i diagrammet är följande:

1. Dokumentanvändaren öppnar dokumentet i ett klientprogram som stöds och autentiserar med servern. Dokumentets identifierare skickas till dokumentets säkerhetsserver.
1. Dokumentsäkerhet autentiserar användarna, kontrollerar principen för auktorisering och skapar en voucher. Verifikationen (som innehåller dokumentnyckeln och behörigheterna) returneras till klientprogrammet.
1. Dokumentet dekrypteras med dokumentnyckeln och dokumentnyckeln ignoreras. Dokumentet kan sedan användas i enlighet med sekretessinställningarna för profilen. Dessa åtgärder utförs i det klientprogram som stöds.

Du kan fortsätta använda ett dokument under följande förhållanden:

* Oändligt eller för giltighetsperioden som anges i policyn
* Tills administratören eller den person som tillämpade policyn återkallar åtkomsten till dokumentet eller ändrar policyn

Du kan också använda principskyddade dokument offline (utan Internet- eller nätverksanslutning) om profilen tillåter åtkomst offline. Logga först in på dokumentsäkerhet för att synkronisera dokumentet. Du kan sedan använda dokumentet under den offlinelåneperiod som anges i profilen.

När offlinelåneperioden är slut synkroniserar du dokumentet igen med dokumentsäkerhet, antingen genom att gå online och öppna ett policyskyddat dokument eller genom att använda ett kommando i klientprogrammet. Mer information finns i *hjälpen för Acrobat* eller i *hjälpen för Acrobat Reader DC-tillägg* .

Om du sparar en kopia av ett principskyddat dokument med menykommandot Spara eller Spara som, används profilen automatiskt för det nya dokumentet. Händelser som försök att öppna det nya dokumentet granskas också och registreras för det ursprungliga dokumentet.

## Principuppsättningar {#policy-sets}

*Principuppsättningar* används för att gruppera en uppsättning principer som har ett gemensamt affärssyfte. Dessa principuppsättningar görs sedan tillgängliga för en delmängd av användarna i systemet.

Varje principuppsättning kan ha en eller flera associerade principuppsättningskoordinatorer. Principuppsättningens koordinator är en administratör eller en användare som har fler behörigheter. *Principuppsättningskoordinatorn* är vanligtvis en specialist i organisationen som bäst kan skapa policyer i en viss principuppsättning.

Koordinatorer för principuppsättningar kan utföra följande uppgifter:

* Skapa profiler
* Redigera och ta bort profiler i principuppsättningen
* Redigera inställningar för principuppsättning
* Lägga till och ta bort koordinatorer för principuppsättningar
* Visa policy- och dokumenthändelser för alla profiler eller dokument i principuppsättningen
* Återkalla åtkomst till dokument
* Byt profil för dokumentet.

>[!NOTE]
>
>Du kan hämta högst 1 000 principuppsättningsnamn från databasen med API:t `getAllPolicysetnames()`.

Policyuppsättningar skapas och tas bort på webbsidorna för dokumentsäkerhetsadministration av administratörer och koordinatorer för principuppsättningar som har behörighet att göra det.

Policyuppsättningar är tillgängliga för ett begränsat antal användare genom att ange vilka användare eller grupper inom en domän som kan använda profilerna från den principuppsättningen för att skydda dokument.

När dokumentsäkerhet är installerat skapas en standardprincipuppsättning med namnet *Global principuppsättning*. Administratören som installerade programvaran hanterar den här principinställningen.

## God praxis {#best-practices}

Profiler är återanvändbara behörighetsgrupper och användargrupper som kan användas i olika dokument. För de skyddade dokumenten. Dessa profiler säkerställer att endast behöriga användare kan använda tillåtna funktioner. Antalet policyer och uppsättningar förväntas öka med olika användarroller och dokument på en avdelning. Här följer några överväganden och bästa metoder för att skapa och hantera principer:

* **Skapa återanvändbara profiler:** Adobe rekommenderar återanvändning av profiler i olika dokument. Det hjälper till att hålla antalet profiler så få som möjligt, ger optimala prestanda och gör det enklare att hantera profilerna. Så här skapar du en återanvändbar profil:

1. Identifiera och definiera krav på åtkomstkontroll på avdelnings- och organisationsnivå.

1. Skapa användargrupper och lägg till användare i dessa grupper.

1. Skapa en principuppsättning.

1. Öppna profiluppsättningen och skapa en profil. Lägg till användargrupper och ange sekretessinställningar (åtkomstkontroll) för profilen.

Lägg till användargrupper i profiler i stället för enskilda användare. Det gör det enklare att hantera och tillämpa principer för många användare.

* **Skapa anpassade principuppsättningar:** En principuppsättning kombinerar flera principer till en hanterbar enhet. Skapa anpassade principuppsättningar för din organisation eller avdelning, använd dem för att gruppera relaterade principer och göra dem tillgängliga för en delmängd av användarna i systemet.

  Med hjälp av principuppsättningar blir det enklare att tilldela och hantera relaterade profiler till specifika användare i en organisation eller avdelning. Exempelvis kan olika uppsättningar av policyer för ekonomi och personaladministration underlätta hanteringen och tillämpningen av relaterade policyer på dokument avsedda för motsvarande avdelningar.

* **Använd en extern auktoriserare för att tillämpa behörigheter dynamiskt:** Du kan använda [extern auktoriserare](https://help.adobe.com/en_US/livecycle/11.0/ProgramLC/WS624e3cba99b79e12e69a9941333732bac8-6f26.2.html) för att utvärdera och dynamiskt tillämpa behörigheter baserat på externt villkor. När behörigheterna utvärderas dynamiskt utifrån ett externt villkor kan du:

   * centralt åtkomsthantering för dokument i er organisation.

   * Styr åtkomsten till profilskyddade dokument genom att dynamiskt avgöra om en användare har åtkomst till ett profilskyddat dokument. Avgör till exempel dynamiskt om en användare kan skriva ut ett profilskyddat dokument.

   * Använd en åtkomstkontrollsfunktion som ert innehållshanteringssystem använder, utöver den vanliga utvärderingsprocessen för principer. När tjänsten till exempel avgör om en användare kan skriva ut ett policyskyddat dokument kan den använda standardprocessen för utvärdering av profiler. Och ni kan också använda åtkomstkontrollsmekanismen som ert innehållshanteringssystem använder.

  Även om det är möjligt att helt ersätta dokumentsäkerhetsprincipens utvärderingsprocess med en extern behörighetshanterare, rekommenderar vi att du använder en extern behörighetshanterare tillsammans med principutvärderingsprocessen. Detta innebär att dokumentåtkomsten kan styras av samma kontrollmekanism som används i innehållshanteringssystemet. När dokumentskyddstjänsten till exempel avgör om en användare kan skriva ut ett policyskyddat dokument, används standardprocessen för utvärdering av skyddsprofiler. Den använder också åtkomstkontrollsmekanismen som används i innehållshanteringssystemet. Mer information finns i [Skapa externa auktoriseringshanterare](https://help.adobe.com/en_US/livecycle/11.0/ProgramLC/WS624e3cba99b79e12e69a9941333732bac8-6f26.2.html).

* **Behåll principuppsättningarna till ett begränsat antal:** Flera faktorer leder till en konstant ökning av principer och principuppsättningar. Några vanliga faktorer är:

   * Öka antalet användarroller, avdelningar och dokument inom en organisation under en period.
   * En organisations avdelningar arbetar isolerat och har noggrann kontroll över de avdelningsspecifika reglerna. Det leder till identiska policyer inom en organisation.

  Adobe rekommenderar att antalet policyer och uppsättningar minimeras. Det underlättar hanteringen av policyer och uppsättningar och ger bättre prestanda. Så här håller du talet till ett minimum:

   * Skapa återanvändbara profiler. Dessa profiler kan delas av flera avdelningar.
   * Det kan vara bra att skapa policyer som omfattar hela organisationen om vissa policyer gäller för flera avdelningar i stället för en enskild principuppsättning för varje avdelning.
   * Grupprelaterade principer i en principuppsättning. Skapa inte en separat principuppsättning för varje princip.
   * Använd en extern auktoriserare för att dynamiskt styra användarbehörigheter.

  >[!NOTE]
  >
  >Du kan använda API:t [getAllPolicyListennames()](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/programlc/javadoc/com/adobe/livecycle/rightsmanagement/client/PolicyManager.html) för att hämta maximalt 1 000 principuppsättningsnamn. Internt hämtar API:t upp till 1 000 principer som API-anroparen har dokumentutgivarbehörighet för och skapar och returnerar sedan en lista med unika principuppsättningsnamn som är associerade med hämtade profiler till dig. Om API:t till exempel hämtar 1 000 principer och de hämtade profilerna är kopplade till totalt 200 principuppsättningar, returnerar API bara 200 principuppsättningsnamn.
