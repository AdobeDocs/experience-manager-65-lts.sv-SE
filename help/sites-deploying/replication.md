---
title: Replikering
description: Lär dig hur du konfigurerar och övervakar replikeringsagenter i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: configuring
docset: aem65
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: b840d970-9365-4df3-8467-e34abd940074
source-git-commit: 262b73813a0e1ccb9c45a4e099461d4dd8eccd00
workflow-type: tm+mt
source-wordcount: '3261'
ht-degree: 0%

---

# Replikering{#replication}

Replikeringsagenter är centrala för Adobe Experience Manager (AEM) eftersom den mekanism som används för att:

* [Publicera (aktivera)](/help/sites-authoring/publishing-pages.md#activatingcontent) innehåll från en författare till en publiceringsmiljö.
* Rensa innehåll explicit från Dispatcher-cachen.
* Returnera användarindata (till exempel formulärindata) från publiceringsmiljön till författarmiljön (under kontroll av författarmiljön).

Begäranden är [köade](/help/sites-deploying/osgi-configuration-settings.md#apacheslingjobeventhandler) till lämplig agent för bearbetning.

>[!NOTE]
>
>Användardata (användare, användargrupper och användarprofiler) replikeras inte mellan författare- och publiceringsinstanser.
>
>För flera publiceringsinstanser distribueras användardata när [användarsynkronisering](/help/sites-administering/sync.md) är aktiverat.

## Replikerar från författare till publicering {#replicating-from-author-to-publish}

Replikering till en Publish-instans eller Dispatcher utförs i flera steg:

* författaren begär att visst innehåll ska publiceras (aktiveras). Detta kan initieras av en manuell begäran eller av automatiska utlösare som har förkonfigurerats.
* begäran skickas till rätt standardreplikeringsagent. En miljö kan ha flera standardagenter som alltid är valda för sådana åtgärder.
* replikeringsagenten&quot;paketerar&quot; innehållet och placerar det i replikeringskön.
* på fliken Webbplatser är den [färgade statusindikatorn](/help/sites-authoring/publishing-pages.md#determiningpagepublicationstatus) inställd för de enskilda sidorna.
* innehållet tas bort från kön och transporteras till publiceringsmiljön med det konfigurerade protokollet, vanligtvis HTTP.
* en tjänstlet i publiceringsmiljön tar emot begäran och publicerar det mottagna innehållet. Standardserverpaketet är `https://localhost:4503/bin/receive`.

* flera redigerings- och publiceringsmiljöer kan konfigureras.

![chlimage_1-21](assets/chlimage_1-21.png)

### Replikerar från publicera till författare {#replicating-from-publish-to-author}

Vissa funktioner tillåter användare att ange data i en Publish-instans.

Ibland behövs en typ av replikering som kallas omvänd replikering för att returnera dessa data till redigeringsmiljön från vilken de distribueras till andra publiceringsmiljöer. Av säkerhetsskäl måste all trafik från publicering till redigeringsmiljön vara strikt kontrollerad.

Omvänd replikering använder en agent i publiceringsmiljön som refererar till redigeringsmiljön. Den här agenten placerar data i en utkorg. Utkorgen matchas med replikeringslyssnare i redigeringsmiljön. Avlyssnarna avsöker utkorgarna för att samla in alla data som anges och sedan distribuera dem efter behov. Detta garanterar att redigeringsmiljön styr all trafik.

### Replikering - utanför lådan {#replication-out-of-the-box}

Skapa en sida genom att följa [Skapa och ordna sidor](/help/sites-authoring/managing-pages.md).

[Installera AEM](/help/sites-deploying/deploy.md) med följande om du vill följa det här exemplet och använda standardreplikeringsagenterna:


* författarmiljön på port `4502`
* publiceringsmiljön på port `4503`

Den här replikeringen utförs från redigeringsmiljön av:

* **Standardagent (publicera)**
Den här agenten replikerar innehåll till standardpubliceringsinstansen.
Information om detta (konfiguration och loggar) finns på verktygskonsolen i redigeringsmiljön, eller:
  `http://localhost:4502/etc/replication/agents.author/publish.html`.

>[!NOTE]
>
>Aktiverad som standard:
>
>* Agenter på författare: Om inte standardagenten (publicera) måste du aktivera den innan du fortsätter.
>
>Inaktiverat som standard (från och med AEM 6.1):
>
>* Agenter på författare: Omvänd replikeringsagent (publish_reverse)
>* Agenter vid publicering: Omvänd replikering (utkorg)
>
>Om du vill kontrollera status för agenten eller kön använder du konsolen **Verktyg**.
>Se [Övervaka dina replikeringsagenter](#monitoring-your-replication-agents).

#### Replikering (författare att publicera) {#replication-author-to-publish}

1. Gå till supportsidan i redigeringsmiljön.
   **https://localhost:4502/content/site1/test.html** `<pi>`
1. Redigera sidan så att du kan lägga till ny text.
1. **Aktivera sidan** så att du kan publicera ändringarna.
1. Öppna supportsidan i publiceringsmiljön:
   **https://localhost:4503/content/site1/test.html**
1. Du kan nu se ändringarna som du har angett för Författare.


#### Replikeringsagenter - utanför lådan {#replication-agents-out-of-the-box}

Följande agenter finns i en standardinstallation av AEM:

* [Standardagent](#replication-author-to-publish)
Används för replikering från författare till publicering.

* Dispatcher Flush
Detta används för att hantera Dispatcher-cachen. Mer information finns i [Invaliderar Dispatcher-cache från redigeringsmiljön](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/page-invalidate.html#invalidating-dispatcher-cache-from-the-authoring-environment) och [Invaliderar Dispatcher-cache från en publiceringsinstans](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/page-invalidate.html#invalidating-dispatcher-cache-from-a-publishing-instance).

* [Omvänd replikering](#configuring-reverse-replication)
Används för replikering från Publicera till Författare. Omvänd replikering används inte för communityfunktioner som forum, bloggar och kommentarer. Den är inaktiverad eftersom utkorgen inte är aktiverad. Användning av omvänd replikering kräver anpassad konfiguration.

* Statisk agent
Detta är en&quot;agent som lagrar en statisk representation av en nod i filsystemet.&quot;
Med standardinställningarna lagras till exempel innehållssidor och dammresurser under `/tmp`, antingen som HTML eller lämpligt resursformat. Se flikarna `Settings` och `Rules` för konfigurationen.
Detta begärdes så att innehållet kan ses när sidan begärs direkt från programservern. Detta är en specialagent och (troligen) krävs inte för de flesta instanser.

## Replikeringsagenter - konfigurationsparametrar {#replication-agents-configuration-parameters}

När du konfigurerar en replikeringsagent från verktygskonsolen är fyra flikar tillgängliga i dialogrutan:

### Inställningar {#settings}

* **Namn**

  Ett unikt namn för replikeringsagenten.

* **Beskrivning**

  En beskrivning av syftet med denna replikeringsagent.

* **Aktiverad**

  Anger om replikeringsagenten är aktiverad.

  När agenten är **aktiverad** visas kön som:

   * **Aktiv** när objekt bearbetas.
   * **Inaktiv** när kön är tom.
   * **Blockerad** när objekt finns i kön, men inte kan bearbetas, till exempel om mottagande kö är inaktiverad.

* **Serialiseringstyp**

  Typ av serialisering:

   * **Standard**: Ange om agenten ska väljas automatiskt.
   * **Dispatcher Flush**: Välj det här alternativet om agenten ska användas för tömning av Dispatcher-cachen.

* **Återförsöksfördröjning**

  Fördröjningen (i millisekunder) mellan två försök om ett problem skulle uppstå.

  Standard: `60000`

* **Agentanvändar-ID**

  Beroende på miljön använder agenten det här användarkontot för att:

   * samla in och paketera innehåll från redigeringsmiljön
   * skapa och skriva innehåll i publiceringsmiljön

  Lämna det här fältet tomt om du vill använda systemanvändarkontot (det konto som definierats i sling som administratörsanvändare. Som standard är det `admin`).

  >[!CAUTION]
  >
  >För en agent i redigeringsmiljön måste det här kontot *ha läsåtkomst till alla sökvägar som du vill ha replikerade.*

  >[!CAUTION]
  >
  >För en agent i publiceringsmiljön måste det här kontot *ha den behörighet att skapa/skriva som krävs för att replikera innehållet.*

  >[!NOTE]
  >
  >Detta kan användas som en mekanism för att välja specifikt innehåll för replikering.

* **Loggnivå**

  Anger den detaljnivå som ska användas för loggmeddelanden.

   * `Error`: endast fel loggas
   * `Info`: fel, varningar och andra informationsmeddelanden loggas
   * `Debug`: en hög detaljnivå används i meddelandena, främst för felsökningssyften

  Standard: `Info`

* **Använd för omvänd replikering**

  Anger om den här agenten används för omvänd replikering, returnerar användarindata från publicerings- till författarmiljön.

* **Aliasuppdatering**

  Om du väljer det här alternativet aktiveras ogiltiga aliassökvägar eller ogiltiga sökvägar till Dispatcher. Se även [Konfigurera en Dispatcher Flush Agent](/help/sites-deploying/replication.md#configuring-a-dispatcher-flush-agent).

#### Transport {#transport}

* **URI**

  Detta anger den mottagande servern på målplatsen. Du kan särskilt ange värdnamnet (eller aliaset) och kontextsökvägen till målinstansen här.

  Till exempel:

   * En standardagent kan replikera till `https://localhost:4503/bin/receive`
   * En Dispatcher Flush-agent kan replikera till `https://localhost:8000/dispatcher/invalidate.cache`

  Det protokoll som anges här (HTTP eller HTTPS) avgör transportmetoden.

  För Dispatcher Flush-agenter används URI-egenskapen endast om du använder sökvägsbaserade virtualhost-poster för att skilja mellan servergrupper, använder du det här fältet för att göra servergruppen ogiltig. Servergrupp nr 1 har till exempel en virtuell värd på `www.mysite.com/path1/*` och servergrupp nr 2 har en virtuell värd på `www.mysite.com/path2/*`. Du kan använda URL:en `/path1/invalidate.cache` för att ange den första servergruppen som mål och `/path2/invalidate.cache` för den andra servergruppen.

* **Användare**

  Användarnamnet för kontot som ska användas för att komma åt målet.

* **Lösenord**

  Lösenord för kontot som ska användas för att komma åt målet.

* **NTLM-domän**

  Domän för NTML-autentisering.

* **NTLM-värd**

  Värd för NTML-autentisering.

* **Aktivera relaxerad SSL**

  Aktivera om du vill att självcertifierade SSL-certifikat ska godkännas.

* **Tillåt utgångna certifikat**

  Aktivera om du vill att utgångna SSL-certifikat ska godkännas.

#### Proxy {#proxy}

Följande inställningar behövs bara om en proxy behövs:

* **Proxyvärd**

  Värdnamn för proxyn som används för transport.

* **Proxyport**

  Proxyns port.

* **Proxyanvändare**

  Användarnamnet för kontot som ska användas.

* **Proxylösenord**

  Lösenord för kontot som ska användas.

* **NTLM-proxydomän**

  NTLM-proxydomänen.

* **NTLM-värd för proxy**

  NTLM-proxydomänen.

#### Utökad {#extended}

* **Gränssnitt**

  Här kan du definiera socketgränssnittet som du vill binda till.

  Detta anger den lokala adress som ska användas när anslutningar skapas. Om detta inte anges används standardadressen. Detta är användbart när du vill ange vilket gränssnitt som ska användas på system med flera hem eller kluster.

* **HTTP-metod**

  HTTP-metoden som ska användas.

  För en Dispatcher Flush-agent är detta nästan alltid GET och bör inte ändras (POST skulle vara ett annat möjligt värde).

* **HTTP-huvuden**

  Dessa används för Dispatcher Flush-agenter och anger element som måste tömmas.

  För en Dispatcher Flush-agent behöver de tre standardposterna inte ändras:

   * `CQ-Action:{action}`
   * `CQ-Handle:{path}`
   * `CQ-Path:{path}`

  Dessa används, beroende på vad som är lämpligt, för att ange vilken åtgärd som ska användas när handtaget eller banan töms. Underparametrarna är dynamiska:

   * `{action}` indikerar en replikeringsåtgärd

   * `{path}` anger en bana

  De ersätts av den sökväg/åtgärd som är relevant för begäran och behöver därför inte&quot;hårdkodas&quot;:

  >[!NOTE]
  >
  >Om du har installerat AEM i en annan kontext än den rekommenderade måste du registrera kontexten i HTTP-rubrikerna. Till exempel:
  >`CQ-Handle:/<*yourContext*>{path}`

* **Stäng anslutning**

  Aktivera så att du kan stänga anslutningen efter varje begäran.

* **Tidsgräns för anslutning**

  Timeout (i millisekunder) som ska användas vid försök att upprätta en anslutning.

* **Sockettimeout**

  Timeout (i millisekunder) som ska användas vid väntan på trafik efter att en anslutning har upprättats.

* **Protokollversion**

  Protokollets version. Exempel: `1.0` för HTTP/1.0.

#### Utlösare {#triggers}

De här inställningarna används för att definiera utlösare för automatiserad replikering:

* **Ignorera standard**

  Om det här alternativet är markerat utesluts agenten från standardreplikeringen. Det innebär att den inte används om en innehållsförfattare utfärdar en replikeringsåtgärd.

* **Vid ändring**

  Här aktiveras en replikering från den här agenten automatiskt när en sida ändras. Används för Dispatcher Flush-agenter, men även för omvänd replikering.

* **Vid distribution**

  Om det här alternativet är markerat kopieras automatiskt allt innehåll som är markerat för distribution när det ändras.

* **På-/offtime uppnådd**

  Detta utlöser automatisk replikering (för att aktivera eller inaktivera en sida efter behov) när de tider eller förskjutningar som har definierats för en sida inträffar. Detta används främst för Dispatcher Flush-agenter.

* **Vid mottagning**

  Om det här alternativet är markerat replikeras agentkedjorna när replikeringshändelser tas emot.

* **Ingen statusuppdatering**

  När det här alternativet är markerat tvingar agenten inte fram någon uppdatering av replikeringsstatusen.

* **Ingen versionshantering**

  När det här alternativet är markerat tvingar agenten inte fram versionshantering av aktiverade sidor.

## Konfigurera replikeringsagenter {#configuring-your-replication-agents}

### Konfigurera dina replikeringsagenter från författarmiljön {#configuring-your-replication-agents-from-the-author-environment}

På fliken Verktyg i redigeringsmiljön kan du konfigurera replikeringsagenter som finns i antingen författarmiljön (**Agenter på författare**) eller publiceringsmiljön (**Agenter på publicering**). Följande procedurer illustrerar konfigurationen av en agent för författarmiljön, men kan användas för båda.

>[!NOTE]
>
>När en Dispatcher hanterar HTTP-begäranden för författare- eller publiceringsinstanser måste HTTP-begäran från replikeringsagenten innehålla PATH-huvudet. Utöver följande procedur måste du lägga till PATH-rubriken i Dispatcher-listan med klientrubriker. Se [/klienthuvuden (klienthuvuden)](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-http-headers-to-pass-through-clientheaders).
>

1. Gå till fliken **Verktyg** i AEM.
1. Klicka på **Replikering** (vänster ruta för att öppna mappen).
1. Dubbelklicka på **Agenter på författare** (antingen den vänstra eller den högra rutan).
1. Klicka på lämpligt agentnamn (som är en länk) för att visa detaljerad information om agenten.
1. Klicka på **Redigera** så att konfigurationsdialogrutan öppnas:

   ![chlimage_1-22](assets/chlimage_1-22.png)

1. De angivna värdena ska vara tillräckliga för en standardinstallation. Om du gör ändringar klickar du på **OK** för att spara dem (mer information om enskilda parametrar finns i [Replikeringsagenter - konfigurationsparametrar](#replication-agents-configuration-parameters)).

>[!NOTE]
>
>En standardinstallation av AEM anger `admin` som användare för transportautentiseringsuppgifter inom standardreplikeringsagenterna.
>
>Detta bör ändras till ett platsspecifikt replikeringsanvändarkonto med behörighet att replikera de nödvändiga sökvägarna.

### Konfigurerar omvänd replikering {#configuring-reverse-replication}

Omvänd replikering används för att hämta användarinnehåll som genererats i en Publish-instans tillbaka till en Author-instans. Detta används ofta för funktioner som undersökningar och registreringsformulär.

Av säkerhetsskäl tillåter de flesta nätverkstopologier inte anslutningar *från* den &quot;Demilitarized Zone&quot; (ett undernätverk som exponerar de externa tjänsterna för ett icke betrott nätverk som Internet).

Eftersom publiceringsmiljön vanligtvis finns i DMZ måste anslutningen initieras från Author-instansen för att innehållet ska kunna skickas tillbaka till redigeringsmiljön. Detta görs med:

* en *utkorg* i publiceringsmiljön där innehållet placeras.
* en agent (publicera) i redigeringsmiljön som regelbundet frågar efter nytt innehåll i utkorgen.

För att göra detta behöver du:

**En agent för omvänd replikering i redigeringsmiljön** - Fungerar som den aktiva komponenten för att samla in information från utkorgen i publiceringsmiljön:

Om du vill använda omvänd replikering kontrollerar du att agenten är aktiverad.

![chlimage_1-23](assets/chlimage_1-23.png)

**En omvänd replikeringsagent i publiceringsmiljön (en utkorg)** - Det passiva elementet fungerar som en utkorg. Användarindata placeras här, från vilket agenten samlar in dem i författarmiljön.

![chlimage_1-1](assets/chlimage_1-1.jpeg)

### Konfigurera replikering för flera publiceringsinstanser {#configuring-replication-for-multiple-publish-instances}

>[!NOTE]
>
>Endast innehåll replikeras - användardata är inte det (användare, användargrupper och användarprofiler).
>
>Aktivera [Användarsynkronisering](/help/sites-administering/sync.md) om du vill synkronisera användardata mellan flera publiceringsinstanser.

Efter installationen har en standardagent redan konfigurerats för replikering av innehåll till en Publish-instans som körs på port 4503 på den lokala värden.

Om du vill konfigurera replikering av innehåll för ytterligare en Publish-instans skapar och konfigurerar du en ny replikeringsagent:

1. Öppna fliken **Verktyg** i AEM.
1. Välj **Replikering** och sedan **Agenter på författare** i den vänstra panelen.
1. Välj **Ny..**.
1. Ange **Titel** och **Namn** och välj sedan **Replikeringsagent**.
1. Klicka på **Skapa** så att du kan skapa agenten.
1. Dubbelklicka på det nya agentobjektet så att konfigurationspanelen öppnas.
1. Klicka på **Redigera** - dialogrutan **Agentinställningar** öppnas - **Serialiseringstypen** är redan definierad som standard. Detta måste finnas kvar.

   * På fliken **Inställningar**:

      * Aktivera **Aktiverad**.
      * Ange en **beskrivning**.
      * Ange **Fördröjning för nytt försök** till `60000`.

      * Låt **serialiseringstypen** vara `Default`.

   * På fliken **Transport**:

      * Ange den URI som krävs för den nya Publish-instansen, till exempel
        `https://localhost:4504/bin/receive`.

      * Ange det platsspecifika användarkonto som används för replikering.
      * Du kan konfigurera andra parametrar efter behov.

1. Klicka på **OK**.

Du kan sedan testa åtgärden genom att uppdatera och sedan publicera en sida i redigeringsmiljön.

Uppdateringarna visas på alla publiceringsinstanser som har konfigurerats enligt ovan.

Om du får problem kan du kontrollera loggarna på författarinstansen. Beroende på vilken detaljnivå som krävs kan du även ställa in **loggnivån** till `Debug` med dialogrutan **Agentinställningar** enligt ovan.

>[!NOTE]
>
>Detta kan kombineras med användning av [agentens användar-ID](#agentuserid) för att välja ett annat innehåll som ska replikeras till de enskilda publiceringsmiljöerna. För varje publiceringsmiljö:
>
>1. Konfigurera en replikeringsagent för replikering till den publiceringsmiljön.
>1. Konfigurera ett användarkonto med de åtkomsträttigheter som krävs för att läsa innehållet som replikeras till den specifika publiceringsmiljön.
>1. Tilldela användarkontot som **agentens användar-ID** för replikeringsagenten.
>

### Konfigurera en Dispatcher Flush-agent {#configuring-a-dispatcher-flush-agent}

Standardagenter ingår i installationen. En viss konfiguration behövs dock fortfarande, och det samma gäller om du definierar en ny agent:

1. Öppna fliken **Verktyg** i AEM.
1. Klicka på **Distribution**.
1. Välj **Replikering** och sedan **Agenter vid publicering**.
1. Dubbelklicka på **Dispatcher Flush** för att öppna översikten.
1. Klicka på **Redigera** - dialogrutan **Agentinställningar** öppnas:

   * På fliken **Inställningar**:

      * Aktivera **Aktiverad**.
      * Ange en **beskrivning**.
      * Lämna **serialiseringstypen** som `Dispatcher Flush` eller ange den som sådan om du skapar en agent.

      * (valfritt) Markera **Aliasuppdatering** om du vill aktivera annulleringsbegäranden för alias eller ogiltighetssökvägen för Dispatcher.

   * På fliken **Transport**:

      * Ange den URI som krävs för den nya Publish-instansen, till exempel
        `https://localhost:80/dispatcher/invalidate.cache`.

      * Ange det platsspecifika användarkonto som används för replikering.
      * Du kan konfigurera andra parametrar efter behov.

   För Dispatcher Flush-agenter används URI-egenskapen endast om du använder sökvägsbaserade virtualhost-poster för att skilja mellan servergrupper, använder du det här fältet för att göra servergruppen ogiltig. Servergrupp nr 1 har till exempel en virtuell värd på `www.mysite.com/path1/*` och servergrupp nr 2 har en virtuell värd på `www.mysite.com/path2/*`. Du kan använda URL:en `/path1/invalidate.cache` för att ange den första servergruppen som mål och `/path2/invalidate.cache` för den andra servergruppen.

   >[!NOTE]
   >
   >Om du har installerat AEM i en annan kontext än den rekommenderade, konfigurerar du [HTTP Headers](#extended) på fliken **Extended**.

1. Klicka på **OK**.
1. Gå tillbaka till fliken **Verktyg** och **Aktivera** agenten **Dispatcher Flush** (**Agenter vid publicering**) härifrån.

Replikeringsagenten **Dispatcher Flush** är inte aktiv på författaren. Du kan komma åt samma sida i publiceringsmiljön med motsvarande URI, till exempel `https://localhost:4503/etc/replication/agents.publish/flush.html`.

### Kontrollera åtkomst till replikeringsagenter {#controlling-access-to-replication-agents}

Åtkomst till de sidor som används för att konfigurera replikeringsagenterna kan styras med användar- och/eller gruppsidbehörigheter på noden `etc/replication`.

>[!NOTE]
>
>Inställningen påverkar inte användare som replikerar innehåll (t.ex. från webbplatskonsolen eller sidosparsalternativet). Replikeringsramverket använder inte den aktuella användarens användarsession för att komma åt replikeringsagenter när sidor replikeras.

### Konfigurera dina replikeringsagenter från CRXDE Lite {#configuring-your-replication-agents-from-crxde-lite}

>[!NOTE]
>
>Det går bara att skapa replikeringsagenter på databasplatsen `/etc/replication`. Detta krävs för att de tillhörande åtkomstkontrollistorna ska hanteras på rätt sätt. Om du skapar en replikeringsagent på en annan plats i trädet kan det leda till obehörig åtkomst.

Olika parametrar för dina replikeringsagenter kan konfigureras med CRXDE Lite.

Om du navigerar till `/etc/replication` kan du se följande tre noder:

* `agents.author`
* `agents.publish`
* `treeactivation`

De två `agents` innehåller konfigurationsinformation om lämplig miljö och är bara aktiva när den miljön körs. `agents.publish` används till exempel bara i publiceringsmiljön. På följande skärmbild visas Publish Agent i redigeringsmiljön, som i AEM WCM:

![chlimage_1-24](assets/chlimage_1-24.png)

## Övervaka replikeringsagenter {#monitoring-your-replication-agents}

Så här övervakar du en replikeringsagent:

1. Gå till fliken **Verktyg** i AEM.
1. Klicka på **Replication**.
1. Dubbelklicka på länken till agenterna för lämplig miljö (antingen vänster eller höger ruta). Exempel: **Agenter på författare**.

   I det resulterande fönstret visas en översikt över alla dina replikeringsagenter för redigeringsmiljön, inklusive mål och status.

1. Klicka på lämpligt agentnamn (som är en länk) för att visa detaljerad information om agenten:

   ![chlimage_1-2](assets/chlimage_1-2.jpeg)

   Här kan du göra följande:

   * Se om agenten är aktiverad.
   * Se målet för alla replikeringar.
   * Kontrollera om replikeringskön är aktiv (aktiverad).
   * Se om det finns några objekt i kön.
   * **Uppdatera** eller **Rensa** om du vill uppdatera visningen av köposter. Detta gör att du lättare ser att objekt kommer in i och lämnar kön.

   * **Visa logg** om du vill komma åt loggen för eventuella åtgärder från replikeringsagenten.
   * **Testa anslutning** till målinstansen.
   * **Tvinga nya försök** för alla köobjekt om det behövs.

   >[!CAUTION]
   >
   >Använd inte länken Testa anslutning för den omvända replikeringsutkorgen på en publiceringsinstans.
   >
   >
   >Om ett replikeringstest utförs för en Utkorgskö bearbetas alla objekt som är äldre än testreplikeringen om med varje omvänd replikering.
   >
   >
   >Om sådana objekt finns i en kö kan de hittas med följande XPath JCR-fråga och bör tas bort.
   >
   >
   >`/jcr:root/var/replication/outbox//*[@cq:repActionType='TEST']`

## Batchreplikering {#batch-replication}

Gruppreplikeringen replikerar inte enskilda sidor eller resurser, men väntar på att det första tröskelvärdet för de två sidorna, baserat på tid eller storlek, ska aktiveras.

Därefter paketeras alla replikeringsobjekt i ett paket som sedan replikeras som en enda fil till utgivaren.

Publisher packar upp alla objekt, sparar dem och rapporterar tillbaka till författaren.

### Konfigurerar batchreplikering {#configuring-batch-replication}

1. Gå till `http://serveraddress:serverport/siteadmin`
1. Tryck på ikonen **[!UICONTROL Tools]** längst upp på skärmen
1. Gå till **[!UICONTROL Replication - Agents on Author]** från den vänstra navigeringslisten och dubbelklicka på **[!UICONTROL Default Agent]**.
   * Du kan även nå standardagenten för publiceringsreplikering genom att gå direkt till `http://serveraddress:serverport/etc/replication/agents.author/publish.html`
1. Tryck på knappen **[!UICONTROL Edit]** ovanför replikeringskön.
1. Gå till fliken **[!UICONTROL Batch]** i följande fönster:
   ![gruppreplikering](assets/batchreplication.png)
1. Konfigurera agenten.

### Parametrar {#parameters}

* `[!UICONTROL Enable Batch Mode]` - aktiverar eller inaktiverar gruppreplikeringsläge
* `[!UICONTROL Max Wait Time]` - Maximal väntetid i sekunder tills en gruppbegäran startas. Standardvärdet är 2 sekunder.
* `[!UICONTROL Trigger Size]` - Startar batchreplikering när den här storleksgränsen överskrids

## Ytterligare resurser {#additional-resources}

Mer information om felsökning finns på sidan [Felsökning av replikering](/help/sites-deploying/troubleshoot-rep.md).
