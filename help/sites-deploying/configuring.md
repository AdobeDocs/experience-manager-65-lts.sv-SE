---
title: Grundläggande konfigurationskoncept
description: Lär dig hur du konfigurerar Adobe Experience Manager för dina egna specifika behov.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: 338ea82e-c248-4118-9d42-e268d6396e65
source-git-commit: f145e5f0d70662aa2cbe6c8c09795ba112e896ea
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Grundläggande konfigurationskoncept{#basic-configuration-concepts}

Adobe Experience Manager (AEM) installeras med standardinställningar för alla parametrar som gör att programmet kan köras &quot;i körklart läge&quot;. Du kan dock konfigurera AEM efter dina egna specifika krav.

Det finns många aspekter av AEM som kan konfigureras:

* Vissa är [vanligtvis konfigurerade för varje projektinstallation](#primary-configuration-considerations) och måste granskas för att bekräfta om de är tillämpliga på ditt projekt.
* [Ytterligare konfigurationer](#further-configuration-considerations) kan vara vanliga, men inte absolut nödvändiga. De är relaterade till funktioner, systemprestanda och stabilitet.
* Andra krävs bara för vissa valfria funktioner i AEM (dessa dokumenteras tillsammans med lämplig funktion).

Beroende på den specifika konfigurationen kan dessa ändringar göras med något av följande:

* **Adobe CQ Web Console**

  Det här är en standardplats för att konfigurera OSGi-paket och -tjänster.

  Mer information och rekommenderade metoder finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

* **Databas**

  En delmängd av OSGi-konfigurationer är tillgänglig i databasen. Detta garanterar att identiska konfigurationer återskapas när databasinnehåll kopieras eller replikeras. Du kan också lägga till egna konfigurationer, beroende på körningsläge, i databasen.

  Mer information finns i [OSGi-konfiguration i databasen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-in-the-repository) och särskilt i [Lägga till en ny konfiguration i databasen](/help/sites-deploying/configuring-osgi.md#adding-a-new-configuration-to-the-repository).

* **Filsystem**

  Några konfigurationsfiler finns i filsystemet.

* **AEM WCM**

  Olika aspekter kan konfigureras i själva AEM WCM, många med konsolen [Tools](/help/sites-administering/tools-consoles.md) och till exempel replikeringsagenter.

>[!NOTE]
>
>När du arbetar med Adobe Experience Manager finns det flera metoder för att hantera konfigurationsinställningarna för OSGi-tjänster (konsol- eller databasnoder).
>
>Mer information finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

>[!NOTE]
>
>Det är enkelt att konfigurera AEM. Vissa ändringar kan dock ha stor effekt på programmen. Se därför till att du har den erfarenhet och kunskap som krävs innan du börjar konfigurera AEM, och gör bara de ändringar som du vet är nödvändiga. Alla ändringar som görs via OSGi-konsolen tillämpas **omedelbart** på det system som körs (ingen omstart krävs).

## Överväganden om primär konfiguration {#primary-configuration-considerations}

Den här listan innehåller information om de primära områden som vanligtvis konfigureras för varje nytt projekt. Alla behövs inte, men listan måste läsas och granskas för att se vad som gäller för ditt projekt.

Listan innehåller en kort översikt över varje konfigurationsaspekt, tillsammans med länkar till sidorna med fullständig information.

### Säkerhetschecklista {#security-checklist}

Flera viktiga konfigurationsproblem visas i listan [Säkerhetskontroller](/help/sites-administering/security-checklist.md). Läs detta och vidta eventuella åtgärder som krävs för installationen.

### Konfigurera standardgränssnittet - Touchoptimerat eller Classic {#configuring-the-default-ui-touch-optimized-or-classic}

Det finns två användargränssnitt som kan användas i AEM:

* Det pekoptimerade användargränssnittet
* Det klassiska användargränssnittet

Du kan konfigurera det användargränssnitt du behöver med [rotmappning](/help/sites-deploying/osgi-configuration-settings.md).

>[!NOTE]
>
>Mer information om hur du väljer användargränssnittet finns under [Välja användargränssnittet](/help/sites-authoring/select-ui.md).

### IPv4 och IPv6 {#ipv-and-ipv}

Alla element i AEM (till exempel databasen och Dispatcher) kan installeras i både IPv4- och IPv6-nätverk.

Åtgärden är smidig eftersom ingen speciell konfiguration krävs. När det behövs kan du bara ange en IP-adress med det format som passar din nätverkstyp.

Det innebär att när en IP-adress måste anges kan du välja (efter behov) bland:

* en IPv6-adress

  till exempel `https://[ab12::34c5:6d7:8e90:1234]:4502`

* en IPv4-adress

  till exempel `https://123.1.1.4:4502`

* ett servernamn

  till exempel `https://www.yourserver.com:4502`

* standardfallet för `localhost` tolkas för både IPv4- och IPv6-nätverksinstallationer

  till exempel `http://localhost:4502`

### Rensning av version {#version-purging}

I en standardinstallation skapar AEM en version av en sida eller nod när du aktiverar en sida (efter att innehållet har uppdaterats). Du kan också skapa ytterligare versioner på begäran med hjälp av fliken **Versioning** i sidosparken. Alla dessa versioner lagras i databasen och kan återställas om det behövs.

Dessa versioner rensas aldrig, så databasstorleken ökar med tiden och måste därför hanteras.

Mer information finns i [Rensning av version](/help/sites-deploying/version-purging.md), särskilt [versionshanteraren](/help/sites-deploying/version-purging.md#version-manager), om hur du konfigurerar AEM att rensa äldre versioner när en ny version skapas.

### Loggning {#logging}

AEM ger dig möjlighet att konfigurera:

* globala parametrar för den centrala loggningstjänsten
* begär dataloggning; en särskild loggningskonfiguration för begärandeinformation
* specifika inställningar för de enskilda tjänsterna, till exempel en enskild loggfil och ett format för loggmeddelandena

Mer information finns i [Loggning](/help/sites-deploying/configure-logging.md).

### Körningslägen {#run-modes}

Med körningslägena kan du trimma din AEM-instans för ett specifikt ändamål. Du kan till exempel skriva eller publicera, testa, utveckla eller intranäta och så vidare.

Detta görs genom att definiera samlingar av konfigurationsparametrar för varje körningsläge. En grundläggande uppsättning konfigurationsparametrar används för alla körningslägen, och du kan sedan justera ytterligare uppsättningar efter syftet med den specifika miljön. Dessa används sedan efter behov.

Alla konfigurationsinställningar lagras i en databas och aktiveras genom att du anger **körningsläget**.

Mer information finns i [Körningslägen](/help/sites-deploying/configure-runmodes.md).

### Enkel inloggning {#single-sign-on}

Med enkel inloggning (SSO) kan en användare få åtkomst till flera system efter att ha angett inloggningsuppgifter (till exempel användarnamn och lösenord) en gång. Ett separat system (kallas betrodd autentiserare) utför autentiseringen och förser Experience Manager med inloggningsuppgifterna. Experience Manager kontrollerar och verkställer användarens åtkomstbehörigheter (d.v.s. avgör vilka resurser användaren har åtkomst till).

Mer information finns i [Enkel inloggning](/help/sites-deploying/single-sign-on.md).

### Resursmappning {#resource-mapping}

Resursmappning används för att definiera omdirigeringar, tillfälliga URL:er och virtuella värdar för AEM.

Du kan till exempel använda dessa mappningar för:

* Lägg till prefix för alla begäranden med `/content` så att den interna strukturen döljs för besökarna på webbplatsen.
* Definiera en omdirigering så att alla begäranden till sidan `/content/en/gateway` på webbplatsen omdirigeras till `https://gbiv.com/`.

Mer information finns i [Resursmappning](/help/sites-deploying/resource-mapping.md).

### Replikerings-, omvänd replikering- och replikeringsagenter {#replication-reverse-replication-and-replication-agents}

Replikeringsagenterna är centrala för AEM eftersom mekanismen som används för att:

* [Publicera (aktivera)](/help/sites-authoring/publishing-pages.md) innehåll från en författare till en publiceringsmiljö.
* Rensa innehåll explicit från Dispatcher-cachen.
* Returnera användarindata (till exempel formulärindata) från publiceringsmiljön till författarmiljön (under kontroll av författarmiljön).

Mer information finns i [Replikering](/help/sites-deploying/replication.md).

### Konfigurationsinställningar för OSGi {#osgi-configuration-settings}

[OSGi](https://www.osgi.org/) är ett grundläggande element i AEM teknikstack. Det används för att styra de sammansatta paketen av AEM och deras konfiguration.

I [OSGi-konfigurationsinställningar](/help/sites-deploying/osgi-configuration-settings.md) finns en lista med de olika paket som är relevanta för projektimplementering (listade enligt paket). Alla inställningar som visas behöver inte justeras, vissa anges för att du ska förstå hur AEM fungerar.

När du arbetar med AEM finns det flera metoder för att hantera konfigurationsinställningarna för sådana tjänster. Mer information och rekommenderade tillvägagångssätt finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

### Konfigurerar LDAP {#configuring-ldap}

LDAP-autentisering krävs för att autentisera användare som lagras i en (central) LDAP-katalog som Active Directory. Detta minskar den arbetsinsats som krävs för att hantera användarkonton.

LDAP-autentisering sker på databasnivå, så den hanteras direkt av databasen. Mer information finns i [Konfigurera LDAP med AEM](/help/sites-administering/ldap-config.md).

Mer information om användarhantering i AEM (inklusive tilldelning av åtkomsträttigheter) finns i [Användaradministration och -säkerhet](/help/sites-administering/security.md).

### Konfigurera Dispatcher {#configuring-the-dispatcher}

Dispatcher är Adobe Experience Manager verktyg för cachning, belastningsutjämning eller både och. Den kan användas med en webbserver i företagsklass.

Mer information finns i [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html), särskilt [Konfigurera Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html) för mer konfigurationsinformation.

### Konfigurera AEM LiveCycle Connector {#configuring-aem-livecycle-connector}

I och med lanseringen av AEM Doc Services och AEM Doc Security kan AEM nu anropa LiveCycle doc-tjänsterna för att återge ett XFA-formulär, konvertera ett dokument till PDF och skydda ett dokument med regler. Mer information finns i [AEM LiveCycle Connector](https://helpx.adobe.com/livecycle/help/aem/aem-livecycle-connector.html).

### Jobbavlastning och topologiadministration {#job-offloading-and-topology-administration}

[Avlastning](/help/sites-deploying/offloading.md) distribuerar bearbetningsåtgärder mellan Experience Manager-instanser i en topologi. Med avlastning kan du använda specifika Experience Manager-instanser för att utföra vissa typer av bearbetning. Specialiserad bearbetning gör att du kan maximera användningen av tillgängliga serverresurser.

Topologier är löst kopplade Experience Manager-kluster som deltar i avlastning. Ett kluster består av en eller flera Experience Manager-serverinstanser (en instans betraktas som ett kluster).

Mer information om hur du visar eller ändrar topologimedlemskap finns i avsnittet [Administrera topologier](/help/sites-deploying/offloading.md#administering-topologies).

### Konfigurera välkomstkonsolen {#configuring-the-welcome-console}

Välkomstkonsolen för det klassiska användargränssnittet innehåller en lista med länkar till de olika konsolerna och funktionerna i AEM.

Du kan konfigurera synliga länkar i [Konfigurera välkomstkonsolen](/help/sites-developing/customizing-the-welcome-console.md) för mer information.

### Konfigurera för prestanda {#configuring-for-performance}

[Prestanda](/help/sites-deploying/configuring-performance.md) är nyckeln till ditt projekt. Vissa aspekter av AEM (och/eller den underliggande databasen) kan konfigureras för att optimera prestanda.

Mer information finns i [Konfigurera för prestanda](/help/sites-deploying/configuring-performance.md#configuring-for-performance).

<!--delete ### Scaling {#scaling}

Scaling a CQ installation correctly depends greatly on the details of your particular use case. A detailed discussion of solution patterns for various situations can be found in [Scaling CQ](/help/sites-deploying/scaling.md).-->

### Delat datalager {#shared-data-store}

Databasens datalager används för att avlasta lagringen av stora binärfiler från databasen till ett separat område, så att flera instanser av samma binära (till exempel en bild) i databasträdet bara lagras en gång.

Den här funktionen&quot;store-once, reference-many-times&quot; kan utökas så att den inte bara innehåller ett enda databasträd utan helt separata databaser, genom att konfigurera datalagret för varje databas så att den refererar till samma delade filsystemplats.

Ett sådant datalager kan delas mellan olika noder i samma kluster, olika publicerings- och/eller författarinstanser i samma installation eller till och med helt separata instanser i olika installationer.

Mer information finns i [Konfigurera datalager och nodarkiv](/help/sites-deploying/data-store-config.md).

## Ytterligare konfigurationsöverväganden {#further-configuration-considerations}

### Aktivera HTTP över SSL {#enabling-http-over-ssl}

Du kan aktivera HTTP över SSL för att använda säkrare anslutningar till dina servrar.

Mer information finns i [Aktivera HTTP över SSL](/help/sites-administering/ssl-by-default.md).

### AEM Portaler och Portlets {#aem-portals-and-portlets}

En portal är ett webbprogram som erbjuder personalisering, samlad inloggning, innehållsintegrering från olika källor och som är värd för informationssystemens presentationsskikt. Med portletkomponenten kan du även bädda in en portlet på sidan. För att få tillgång till innehåll som tillhandahålls av CQ5 WCM kan portalservern utrustas med CQ5 Portal Director Portlet. Du kan göra detta genom att installera, konfigurera och lägga till portleten på portalsidan.

Mer information finns i [Portal och Portlets](/help/sites-administering/aem-as-portal.md).

### Förfallotid för statiska objekt {#expiration-of-static-objects}

Statiska objekt (till exempel ikoner) ändras inte. Därför bör systemet konfigureras så att det inte upphör att gälla (under en rimlig tidsperiod) och på så sätt minskar onödig trafik.

Mer information finns i [Förfallotid för statiska objekt](/help/sites-deploying/expiration-static-objects.md).

### Öppna filer i Java™-processen {#open-files-in-the-java-process}

Varje Java™-process kan komma åt filer - detta kräver systemresurser. Av den anledningen definieras en övre gräns för hur många filer varje process har åtkomst till samtidigt. Om detta överskrids kan ett undantagsfel uppstå.

Om AEM-processen överskrider det högsta tillåtna antalet visas meddelandet `too many open files` i `error.log`.

Så här undviker du sådana undantag:

1. Kontrollera hur många öppna filer som används i AEM-processen.

   Den här kontrollen beror på vilken plattform instansen körs på. Verktyg som LSOF (UNIX®) eller Process Explorer (Windows) kan användas.

   Detta värde bör övervakas under utveckling och testning för att

   * bekräfta att filer stängs efter behov
   * fastställa det högsta värde som behövs (under olika omständigheter)

1. Ange högsta tillåtna värde.

   Det nya värdet bör ta hänsyn till både aktuella behov och framtida toppar, så det är tillrådligt att fördubbla nuvarande behov.

   Som standard konfigurerar `serverctl` `CQ_MAX_OPEN_FILES` till `8192`. Detta bör räcka för de flesta scenarier.

### Konfigurera RTF-redigeraren {#configuring-the-rich-text-editor}

**RTF-redigeraren** (**RTE**) ger författare ett brett urval av [funktioner](/help/sites-authoring/rich-text-editor.md) för att redigera textinnehåll. De får ikoner, markeringsrutor och menyer för en WYSIWYG-upplevelse.

Mer information finns i [Konfigurera RTF-redigeraren](/help/sites-administering/rich-text-editor.md).

### Konfigurera Ångra för sidredigering {#configuring-undo-for-page-editing}

Det finns flera egenskaper som styr hur kommandona Ångra och Gör om fungerar när du redigerar sidor. Dessa kan konfigureras. Mer information finns i [Konfigurera Ångra för sidredigering](/help/sites-administering/config-undo.md).

### Konfigurera videokomponenten {#configuring-the-video-component}

Med [videokomponenten](/help/sites-authoring/default-components-foundation.md#video) kan du placera ett fördefinierat videoelement direkt på sidan.

För att korrekt omkodning ska ske måste administratören [installera MPEG](/help/sites-administering/config-video.md#install-ffmpeg) separat. De kan även [konfigurera dina videoprofiler](/help/sites-administering/config-video.md#configure-video-profiles) för användning med html5-element.

### Konfigurera och anpassa rapporter {#configuring-and-customizing-reports}

CQ ger dig möjlighet att övervaka och analysera instansens status och innehåller ett urval standardrapporter som kan konfigureras för dina individuella behov:

Mer information finns i [Grunderna för anpassning av rapporter](/help/sites-administering/reporting.md#the-basics-of-report-customization).

### Konfigurerar e-postmeddelande {#configuring-email-notification}

CQ skickar e-postmeddelanden till användare som:

* Prenumerera på sidhändelser, till exempel ändringar eller replikering.
* Prenumerera på forumevent.
* Måste utföra ett steg i ett arbetsflöde.

Mer information finns i [Konfigurera e-postmeddelande](/help/sites-administering/notification.md).

### Aktivera sidavbildningar {#enabling-page-impressions}

Sidavbildningar visas i kolumnen **Impressions** i den klassiska användargränssnittskonsolen. Om du vill kunna fånga upp sidvisningar konfigurerar du följande:

* I publiceringsinstansen:

   * [Dagens CQ WCM-sidstatistik](/help/sites-deploying/osgi-configuration-settings.md)

* På författarinstansen:

   * [Adobe Page Impressions Tracker](/help/sites-deploying/osgi-configuration-settings.md)

>[!CAUTION]
>
>Konfigurationen av spårningsfunktionen för Adobe Page Impressions Tracker i författarmiljön tillåter anonyma begäranden till spårningstjänsten.
