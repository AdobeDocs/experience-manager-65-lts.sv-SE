---
title: Förbättra och skydda AEM-formulär i OSGi-miljö
description: Lär dig rekommendationer och bästa praxis för att skydda AEM Forms på OSGi-server.
topic-tags: Security
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Document Security,Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 0%

---

# Förbättra och skydda AEM-formulär i OSGi-miljö {#hardening-and-securing-aem-forms-on-osgi-environment}

Lär dig rekommendationer och bästa praxis för att skydda AEM Forms på OSGi-server.

Att skydda en servermiljö är av yttersta vikt för en organisation. I den här artikeln beskrivs rekommendationer och bästa praxis för att skydda servrar som kör AEM Forms. Det här är inte ett omfattande dokument för värdskydd för ditt operativsystem. I den här artikeln beskrivs i stället olika säkerhetsinställningar som du bör implementera för att förbättra säkerheten i det distribuerade programmet. För att programservrarna ska förbli säkra bör du dock även implementera säkerhetsövervakning, identifiering och svarsprocedurer utöver de rekommendationer som ges i den här artikeln. Dokumentet innehåller även bästa praxis och riktlinjer för att säkra PII (Personally Identiitable Information).

Artikeln är avsedd för konsulter, säkerhetsexperter, systemarkitekter och IT-personal som ansvarar för planering av program- eller infrastrukturutveckling och driftsättning av AEM Forms. De här rollerna innehåller följande vanliga roller:

* IT- och driftstekniker som måste driftsätta säkra webbapplikationer och servrar i sin egen eller kundens organisation.
* Arkitekter och planerare som ansvarar för att planera arkitekturarbetet för sina kunder i organisationen.
* IT-säkerhetsspecialister som fokuserar på att tillhandahålla säkerhet på olika plattformar inom organisationen.
* Konsulter från Adobe och partners som behöver detaljerade resurser för kunder och partners.

Följande bild visar komponenter och protokoll som används i en typisk AEM Forms-distribution, inklusive rätt brandväggstopologi:

![normal-arkitektur](assets/typical-architecture.png)

AEM Forms är mycket anpassningsbart och kan fungera i många olika miljöer. Vissa av rekommendationerna kanske inte gäller för din organisation.

## Säkert transportlager {#secure-transport-layer}

Säkerhetsluckor i transportskiktet är bland de första hoten mot alla Internetanslutna eller intranätriktade programservrar. I det här avsnittet beskrivs processen att härja värdar i nätverket mot dessa sårbarheter. Den behandlar nätverkssegmentering, TCP/IP-stackhärdning (Transmission Control Protocol/Internet Protocol) och användning av brandväggar för värdskydd.

### Begränsa öppna slutpunkter  {#limit-open-endpoints}

En organisation kan ha en extern brandvägg som begränsar åtkomsten mellan en slutanvändare och AEM Forms publiceringsgrupp. Organisationen kan också ha en intern brandvägg som begränsar åtkomsten mellan en publiceringsgrupp och andra element i organisationen (till exempel författarinstans, bearbetningsinstans, databaser). Tillåt att brandväggar ger åtkomst till ett begränsat antal AEM Forms URL:er för slutanvändare och inom organisationselement:

#### Konfigurera extern brandvägg  {#configure-external-firewall}

Du kan konfigurera en extern brandvägg så att vissa AEM Forms-URL:er kan ansluta till Internet. Du måste ha tillgång till dessa URL:er för att fylla i eller skicka ett anpassningsbart formulär, HTML5, brev om korrespondenshantering eller för att logga in på en AEM Forms-server:

<table> 
 <tbody>
  <tr>
   <td>Komponent</td> 
   <td>URI</td> 
  </tr>
  <tr>
   <td>Anpassningsbara formulär</td> 
   <td>
    <ul> 
     <li>/content/dam/formsanddocuments/AF_PATH/jcr:content</li> 
     <li>/etc/clientlibs/fd/</li> 
     <li>/content/forms/af/AF_PATH</li> 
     <li>/libs/granite/csrf/</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>HTML5-formulär</td> 
   <td>
    <ul> 
     <li>/content/forms/formsets/profiles/</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>Korrespondenshantering </td> 
   <td>
    <ul> 
     <li>/aem/forms/createcorrespondence* </li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>Forms Portal </td> 
   <td>
    <ul> 
     <li>/content/forms/portal/</li> 
     <li>/libs/cq/ui/widgets*</li> 
     <li>/libs/cq/security/</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td> AEM Forms App</td> 
   <td>
    <ul> 
     <li>/j_security_check*</li> 
     <li>/soap/services/AuthenticationManagerService</li> 
    </ul> </td> 
  </tr>
 </tbody>
</table>

#### Konfigurera intern brandvägg  {#configure-internal-firewall}

Du kan konfigurera den interna brandväggen så att vissa AEM Forms-komponenter (till exempel författarinstans, bearbetningsinstans, databaser) kan kommunicera med publiceringsservergruppen och andra interna komponenter som omnämns i topologidiagrammet:

<table> 
 <tbody>
  <tr>
   <td>Värd<br /> </td> 
   <td>URI</td> 
  </tr>
  <tr>
   <td>Publiceringsgrupp (publicera noder)</td> 
   <td>/bin/receive</td> 
  </tr>
  <tr>
   <td>Bearbetar server</td> 
   <td>/content/forms/fp/*</td> 
  </tr>
  <tr>
   <td>Forms Workflow tilläggsserver (AEM Forms på JEE-server)</td> 
   <td>/soap/sdk</td> 
  </tr>
 </tbody>
</table>

#### Konfigurera databasbehörigheter och åtkomstkontrollistor {#setup-repository-permissions-and-access-control-lists-acls}

Som standard är resurser som är tillgängliga på publiceringsnoderna tillgängliga för alla. Skrivskyddad åtkomst är aktiverat för alla resurser. Det krävs för att aktivera anonym åtkomst. Om du tänker begränsa formulärvyn och skicka in åtkomst endast till autentiserade användare kan du använda en gemensam grupp för att endast tillåta autentiserade användare att ha skrivskyddad åtkomst till resurserna som finns på publiceringsnoderna. Följande platser/kataloger innehåller formulärresurser som kräver skärpa (skrivskyddad åtkomst för autentiserade användare):

* /content/&amp;ast;
* /etc.clientlibs/fd/&amp;ast;
* /libs/fd/&amp;ast;

## Hantera blankettdata säkert  {#securely-handle-forms-data}

AEM Forms lagrar data till fördefinierade platser och tillfälliga mappar. Ni bör skydda uppgifterna för att förhindra obehörig användning.

### Ställ in periodisk rensning av temporär mapp {#setup-periodic-cleanup-of-temporary-folder}

När du konfigurerar formulär för bifogade filer, verifiera eller förhandsgranska komponenter, lagras motsvarande data på publiceringsnoderna på /tmp/fd/. Data rensas regelbundet. Du kan ändra standardjobbet för datarensning så att det blir mer aggressivt. Om du vill ändra det schemalagda jobbet för att rensa data öppnar du AEM Web Console, öppnar AEM Forms Temporary Storage Cleaning Task och ändrar Cron-uttrycket.

I ovanstående scenarier sparas data endast för autentiserade användare. Dessutom skyddas uppgifterna med åtkomstkontrollistor. Att ändra datarensningen är alltså ytterligare ett steg för att skydda informationen.

### Skydda data som sparats av formulärportalens överföringsåtgärd {#secure-data-saved-by-forms-portal-submit-action}

Som standard sparar formulärportalens åtgärd för att skicka adaptiva formulär data i den lokala databasen på publiceringsnoden. Informationen sparas på /content/forms/fp. **Du bör inte lagra data på publiceringsinstansen.**

Du kan konfigurera lagringstjänsten så att den skickar över nätverket till det kluster som bearbetas, utan att något sparas lokalt på publiceringsnoden. Bearbetningsklustret ligger i en säker zon bakom den privata brandväggen och data förblir säkra.

Använd autentiseringsuppgifterna för bearbetningsservern för inställningstjänsten för AEM DS för att skicka data från publiceringsnoden till bearbetningsservern. Använd inloggningsuppgifterna för en begränsad icke-administrativ användare med läs- och skrivbehörighet till databasen för bearbetningsservern. Mer information finns i [Konfigurera lagringstjänster för utkast och överföringar](/help/forms/using/configuring-draft-submission-storage.md).

### Säkra data som hanteras av formulärdatamodell (FDM) {#secure-data-handled-by-form-data-model-fdm}

Använd användarkonton med nödvändig minimibehörighet för att konfigurera datakällor för formulärdatamodell (FDM). Med administratörskontot kan obehöriga få tillgång till metadata och schemaentiteter.\
Dataintegrering innehåller även metoder för att godkänna FDM-tjänstförfrågningar. Du kan infoga auktoriseringsmekanismer för före och efter körning för att validera en begäran. Tjänstförfrågningar genereras när ett formulär fylls i i förväg, när ett formulär skickas och tjänster anropas via en regel.

**Auktorisering för förbehandling:** Du kan använda auktoriseringen för förbearbetning för att validera en begärans autenticitet innan du kör den. Du kan använda indata, tjänster och information om förfrågningar för att tillåta eller stoppa körningen av begäran. Du kan returnera ett dataintegreringsundantag för OPERATION_ACCESS_DENIED om körningen stoppas. Du kan också ändra klientbegäran innan du skickar den för körning. Du kan till exempel ändra indata och lägga till ytterligare information.

**Auktorisering efter bearbetning:** Du kan använda auktoriseringen efter bearbetning för att validera och kontrollera resultaten innan du returnerar resultaten till den som gjorde begäran. Du kan också filtrera, rensa och infoga ytterligare data i resultaten.

### Begränsa användaråtkomst {#limit-user-access}

En annan uppsättning användarprofiler krävs för att skapa, publicera och bearbeta instanser. Kör inte någon instans med administratörsautentiseringsuppgifter.

**På en publiceringsinstans:**

* Endast användare i en grupp med formuläranvändare kan förhandsgranska, skapa utkast och skicka formulär.
* Endast användare i gruppen cm-user-agent kan förhandsgranska korrespondenshanteringsbrev.
* Inaktivera all icke nödvändig anonym åtkomst.

**På en författarinstans:**

* Det finns olika fördefinierade grupper med specifik behörighet för varje person. Tilldela användare att gruppera.

   * Användare av användargrupp för formulär:

      * kan skapa, fylla i, publicera och skicka in ett formulär.
      * kan inte skapa ett XDP-baserat adaptivt formulär.
      * har inte behörighet att skriva skript för adaptiva formulär.
      * kan inte importera XDP eller paket som innehåller XDP

   * Användare av kraftfulla blanketter kan skapa, fylla i, publicera och skicka alla typer av blanketter, skriva skript för adaptiva blanketter och importera paket som innehåller XDP.
   * En användare av mallar-författare och mallanvändare kan förhandsgranska och skapa en mall.
   * En användare av formulärförfattare kan skapa och ändra en formulärdatamodell.
   * En användare av cm-user-agent-gruppen kan skapa, förhandsgranska och publicera brev för korrespondenshantering.
   * En användare i en grupp för arbetsflödesredigerare kan skapa ett inkorgsprogram och en arbetsflödesmodell.

**Vid bearbetning av författare:**

* Vid användning av fjärrspara och skicka skapar du en användare med behörighet att läsa, skapa och ändra innehåll/formulär/fp-sökvägen för crx-databasen.
* Lägg till användare i användargruppen för arbetsflöden så att en användare kan använda AEM inkorgsprogram.

## Säkra intranätselement i en AEM Forms-miljö {#secure-intranet-elements-of-an-aem-forms-environment}

Vanligtvis körs Bearbetningskluster och Forms Workflow-tillägg (AEM Forms på JEE) bakom en brandvägg. De här betraktas alltså som säkra. Du kan fortfarande utföra några steg för att förbättra dessa miljöer:

### Kluster för säker bearbetning {#secure-processing-cluster}

Ett bearbetningskluster körs i redigeringsläge, men använder det inte för utvecklingsaktiviteter. Tillåt inte att en normal användare inkluderas i innehållsförfattare och formuläranvändargrupper i ett bearbetningskluster.

### Använd AEM bästa praxis för att skydda en AEM Forms-miljö {#use-aem-best-practices-to-secure-an-aem-forms-environment}

Det här dokumentet innehåller instruktioner som är specifika för AEM Forms-miljön. Du bör se till att den underliggande AEM-installationen är säker vid driftsättningen. Mer information finns i dokumentationen för [AEM Security Checklist](/help/sites-administering/security-checklist.md).
