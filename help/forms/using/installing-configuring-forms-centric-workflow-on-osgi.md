---
title: Installera och konfigurera ett Forms-centrerat arbetsflöde i OSGi
description: Installera och konfigurera AEM Forms Interactive Communications för att skapa affärskorrespondenser, dokument, kontoutdrag, förmånsmeddelanden, marknadsföringsmejl, fakturor och välkomstpaket.
topic-tags: installing
docset: aem65
role: Admin, User, Developer
solution: Experience Manager, Experience Manager Forms
feature: Interactive Communication,AEM Forms on OSGi
source-git-commit: 887dc1d6d7e11672b62ef5ca5463ea6181ff0320
workflow-type: tm+mt
source-wordcount: '1601'
ht-degree: 0%

---

# Installera och konfigurera ett Forms-centrerat arbetsflöde i OSGi{#installing-and-configuring-forms-centric-workflow-on-osgi}

## Introduktion {#introduction}

Företag samlar in och bearbetar data från flera formulär, backend-system och andra datakällor. Behandlingen av data omfattar gransknings- och godkännandeförfaranden, repetitiva uppgifter och arkivering av data. Du kan till exempel granska ett formulär och konvertera det till ett PDF-dokument. När de görs manuellt kan de repetitiva uppgifterna ta mycket tid och många resurser.

Du kan använda [Forms-centrerat arbetsflöde i OSGi](../../forms/using/aem-forms-workflow.md) för att snabbt skapa anpassningsbara formulärbaserade arbetsflöden. Dessa arbetsflöden kan hjälpa dig att automatisera arbetsflöden för granskning och godkännande, arbetsflöden för affärsprocesser och andra repetitiva uppgifter. Dessa arbetsflöden hjälper också till att bearbeta dokument (skapa, sammanställa, distribuera och arkivera PDF-dokument, lägga till digitala signaturer för att begränsa åtkomsten till dokument, avkoda streckkodsformulär med mera) och använda Adobe Signs signaturarbetsflöde med formulär och dokument.

När de har konfigurerats kan dessa arbetsflöden utlösas manuellt för att slutföra en definierad process eller köras programmatiskt när användare skickar ett formulär eller interaktiv kommunikation. Funktionen ingår i AEM Forms tilläggspaket.

AEM Forms är en kraftfull plattform för större företag. Forms-centrerat arbetsflöde i OSGi är bara en av funktionerna i AEM Forms. En fullständig lista över funktioner finns i [Introduktion till AEM Forms](introduction-aem-forms.md).

>[!NOTE]
>
>Med Forms-centrerat arbetsflöde i OSGi kan du snabbt skapa och distribuera arbetsflöden för olika uppgifter i OSGi-stacken, utan att behöva installera den fullständiga processhanteringsfunktionen i JEE-stacken. Se en [jämförelse](capabilities-osgi-jee-workflows.md) av de Forms-centrerade AEM arbetsflödena i OSGi och Processhantering i JEE för att lära dig skillnaden och likheterna i funktionerna.
>
>Om du väljer att installera processhanteringsfunktionen på JEE-stacken efter jämförelsen kan du läsa [Installera eller uppgradera AEM Forms på JEE](/help/forms/using/introduction-aem-forms.md) för detaljerad information om hur du installerar och konfigurerar JEE-stacken och processhanteringsfunktionerna.

## Topologi för distribution {#deployment-topology}

AEM Forms tilläggspaket är ett program som distribueras till AEM. Du behöver bara minst en instans av AEM Author eller Processing (produktionsförfattare) för att köra det Forms-centrerade arbetsflödet på OSGi-funktionen. En bearbetningsinstans är en [härdad AEM Author](/help/forms/using/hardening-securing-aem-forms-environment.md)-instans. Gör inga riktiga redigeringsfunktioner, som att skapa arbetsflöden eller anpassningsbara formulär, på produktionsförfattaren.

Följande topologi är en indikativ topologi för att köra AEM Forms Interactive Communications, Correspondence Management, AEM Forms datainhämtning och Forms-Centric-arbetsflöden för OSGi-funktioner. Mer information om topologin finns i [Arkitektur och distributionstopologier för AEM Forms](/help/forms/using/aem-forms-architecture-deployment.md).

![rekommenderad-topologi](assets/recommended-topology.png)

AEM Forms Forms-baserade arbetsflöden i OSGi kör AEM Inbox och AEM Workflow Model som genererar användargränssnitt i Author-instansen av AEM Forms.

## Systemkrav {#system-requirements}

>[!NOTE]
>
>Gå till avsnittet [Nästa steg](../../forms/using/installing-configuring-forms-centric-workflow-on-osgi.md#next-steps) i dokumentet om du redan har installerat AEM Forms på OSGi enligt anvisningarna i artikeln [Installera och konfigurera datainhämtningsfunktioner](../../forms/using/installing-configuring-aem-forms-osgi.md) .

Innan du börjar installera och konfigurera ett Forms-centrerat arbetsflöde i OSGi måste du se till att:

* Maskinvaru- och programvaruinfrastruktur finns på plats. En detaljerad lista över maskinvara och programvara som stöds finns i [Tekniska krav](/help/sites-deploying/technical-requirements.md).

* Installationssökvägen för AEM-instansen innehåller inte blanksteg.
* En AEM-instans körs. I AEM-terminologi är &quot;instance&quot; en kopia av AEM som körs på en server i författar- eller publiceringsläge. Du behöver minst en instans av AEM (Författare eller Bearbetning) för att köra Forms-centrerade arbetsflöden på OSGi:

   * **Författare**: En AEM-instans användes för att skapa, överföra och redigera innehåll och för att administrera webbplatsen. När innehållet är klart att publiceras replikeras det till publiceringsinstansen.
   * **Bearbetning:** En bearbetningsinstans är en [härdad AEM Author](/help/forms/using/hardening-securing-aem-forms-environment.md)-instans. Du kan ställa in en Author-instans och göra den oskarp efter att du har utfört installationen.

   * **Publicera**: En AEM-instans som skickar det publicerade innehållet till allmänheten via Internet eller ett internt nätverk.

* Minneskraven är uppfyllda. AEM Forms tilläggspaket kräver:

   * 15 GB temporärt utrymme för Microsoft Windows-baserade installationer.
   * 6 GB temporärt utrymme för UNIX-baserade installationer.

* Extra krav för UNIX-baserade system: Om du använder det UNIX-baserade operativsystemet installerar du följande paket från installationsmediet för respektive operativsystem.

<table>
 <tbody>
  <tr>
   <td>exponat</td>
   <td>libxcb</td>
   <td>freetype</td>
   <td>libXau</td>
  </tr>
  <tr>
   <td>libSM</td>
   <td>zlib</td>
   <td>libICE</td>
   <td>libuuid</td>
  </tr>
  <tr>
   <td>glibc</td>
   <td>libXext</td>
   <td><p>nss-softokn-free-bl</p> </td>
   <td>fontconfig</td>
  </tr>
  <tr>
   <td>libX11</td>
   <td>libXrender</td>
   <td>libXrandr</td>
   <td>libXinerama</td>
  </tr>
 </tbody>
</table>

## Installera AEM Forms-tilläggspaket {#install-aem-forms-add-on-package}

AEM Forms tilläggspaket är ett program som distribueras till AEM. Paketet innehåller ett Forms-orienterat arbetsflöde för OSGi och andra funktioner. Så här installerar du tilläggspaketet:

1. Öppna [Programvarudistribution](https://experience.adobe.com/downloads). Du behöver en Adobe ID för att logga in på Software Distribution.
1. Välj **[!UICONTROL Adobe Experience Manager]** som finns på rubrikmenyn.
1. I avsnittet **[!UICONTROL Filters]**:
   1. Välj **[!UICONTROL Forms]** i listrutan **[!UICONTROL Solution]**.
   2. Välj version och typ för paketet. Du kan också använda alternativet **[!UICONTROL Search Downloads]** för att filtrera resultaten.
1. Välj det paketnamn som gäller för ditt operativsystem, välj **[!UICONTROL Accept EULA Terms]** och välj **[!UICONTROL Download]**.
1. Öppna [Pakethanteraren](https://experienceleague.adobe.com/docs/experience-manager-65-2025/administering/contentmanagement/package-manager.html) och klicka på **[!UICONTROL Upload Package]** för att överföra paketet.
1. Markera paketet och klicka på **[!UICONTROL Install]**.

   Du kan också hämta paketet via direktlänken i artikeln [AEM Forms releaser](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html) .

1. När paketet har installerats uppmanas du att starta om AEM. **Starta inte om servern omedelbart.** Innan du stoppar AEM Forms-servern väntar du tills meddelandena ServiceEvent REGISTERED och ServiceEvent UNREGISTERED slutar visas i [filen AEM-Installation-Directory]/crx-quickstart/logs/error.log och loggen är stabil.

   >[!NOTE]
   >
   > Vi rekommenderar att du använder kommandot &quot;Ctrl + C&quot; för att starta om SDK:n. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

1. Upprepa steg 1-7 för alla författare- och publiceringsinstanser.

## Konfiguration efter installation {#post-installation-configurations}

AEM Forms har några obligatoriska och valfria konfigurationer. De obligatoriska konfigurationerna inkluderar konfigurering av BouncyCastle-bibliotek och serialiseringsagent. De valfria konfigurationerna inkluderar konfigurering av dispatcher och Adobe Target.

### Obligatoriska konfigurationer efter installationen {#mandatory-post-installation-configurations}

#### Konfigurera RSA- och BouncyCastle-bibliotek  {#configure-rsa-and-bouncycastle-libraries}

Utför följande steg på alla författare- och publiceringsinstanser för att starta delegeringen av biblioteken:

1. Stoppa den underliggande AEM-instansen.
1. Öppna [AEM installationskatalog]\crx-quickstart\conf\sling.properties för redigering.

   Om du använde [AEM installationskatalog]\crx-quickstart\bin\start.bat för att starta AEM redigerar du sling.properties på [AEM_root]\crx-quickstart\.

1. Lägg till följande egenskaper i filen sling.properties:

   ```shell
   sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*  
   ```

1. Spara och stäng filen och starta AEM-instansen.
1. Upprepa steg 1-4 för alla författarinstanser och publiceringsinstanser.

#### Konfigurera serialiseringsagenten {#configure-the-serialization-agent}

Utför följande steg på alla Author- och Publish-instanser för att lägga till paketet i tillåtelselista:

1. Öppna AEM Configuration Manager i ett webbläsarfönster. Standardwebbadressen är https://&#39;[server]:[port]&#39;/system/console/configMgr.
1. Sök efter och öppna **Konfiguration av brandvägg för deserialisering**.
1. Lägg till paketet **sun.util.calendar** i fältet **tillåtelselista**. Klicka på Spara.
1. Upprepa steg 1-3 för alla författare- och publiceringsinstanser.

### Ytterligare konfigurationer efter installation {#optional-post-installation-configurations}

#### Konfigurera Dispatcher {#configure-dispatcher}

Dispatcher är ett verktyg för cachelagring och lastbalansering för AEM. AEM Dispatcher skyddar också AEM-servern mot attacker. Du kan öka säkerheten för din AEM-instans genom att använda Dispatcher tillsammans med en webbserver i företagsklass. Om du använder [Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html) ska du göra följande konfigurationer för AEM Forms:

1. Konfigurera åtkomst för AEM Forms:

   Öppna filen dispatcher.any för redigering. Navigera till filteravsnittet och lägg till följande filter i filteravsnittet:

   `/0025 { /type "allow" /glob "* /bin/xfaforms/submitaction*" } # to enable AEM Forms submission`

   Spara och stäng filen. Mer information om filter finns i [Dispatcher-dokumentation](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html).

1. Konfigurera tjänsten för refererarfilter:

   Logga in som administratör i konfigurationshanteraren för Apache Felix. Konfigurationshanterarens standard-URL är https://&#39;server&#39;:[port_number]/system/console/configMgr. Välj alternativet **Refererarfilter för Apache Sling** på menyn **Konfigurationer**. I fältet Tillåt värdar anger du avsändarens värdnamn så att det kan användas som referent och klickar på **Spara**. Posten har formatet `https://'[server]:[port]'`.

#### Konfigurera cache {#configure-cache}

Cachelagring är en mekanism som förkortar dataåtkomsttider, minskar fördröjningen och förbättrar in-/utdatahastigheter (I/O). Cacheminnet för adaptiva formulär lagrar endast HTML-innehåll och JSON-strukturen i ett adaptivt formulär utan att några förfyllda data sparas. Det minskar tiden som krävs för att återge ett anpassat formulär.

* När du använder cacheminnet för adaptiva formulär använder du [AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html) för att cachelagra klientbibliotek (CSS och JavaScript) för ett adaptivt formulär.
* När du utvecklar anpassade komponenter bör du se till att cachen för anpassade formulär är inaktiverad på servern som används för utveckling.

Utför följande steg för att konfigurera cachen för adaptiva formulär:

1. Gå till konfigurationshanteraren för AEM webbkonsol på `https://'[server]:[port]'/system/console/configMgr`.
1. Klicka på **[!UICONTROL Adaptive Form and Interactive Communication Web Channel Configuration]** om du vill redigera dess konfigurationsvärden. I dialogrutan Redigera konfigurationsvärden anger du det maximala antalet formulär eller dokument som en instans av AEM Forms-servern kan cachelagra i fältet **Antal adaptiva Forms**. Standardvärdet är 100. Klicka på **Spara**.

   >[!NOTE]
   >
   >Om du vill inaktivera cachen anger du värdet **0** i fältet Antal adaptiva Forms. Cacheminnet återställs och alla formulär och dokument tas bort från cacheminnet när du inaktiverar eller ändrar cachekonfigurationen.

#### Konfigurera Adobe Sign {#configure-adobe-sign}

Adobe Sign möjliggör e-signaturarbetsflöden för anpassningsbara formulär. E-signaturer förbättrar arbetsflödena för att bearbeta dokument inom juridik, försäljning, löneadministration, personaladministration och många andra områden.

I ett typiskt Adobe Sign- och Forms-orienterat arbetsflöde i OSGi-scenarier fyller en användare i ett anpassat formulär som **ska användas för en tjänst**. Till exempel kan en kreditkortsansökan och en medborgare få förmåner. När en användare fyller i, skickar och signerar ansökningsformuläret startas ett arbetsflöde för godkännande/avvisning. Tjänsteleverantören granskar programmet i AEM Inbox och använder Adobe Sign för att signera programmet elektroniskt. Om du vill aktivera liknande arbetsflöden för elektroniska signaturer kan du integrera Adobe Sign med AEM Forms.

[Integrera Adobe Sign med AEM Forms](../../forms/using/adobe-sign-integration-adaptive-forms.md) om du vill använda Adobe Sign med AEM Forms.

## Nästa steg {#next-steps}

Du har konfigurerat en miljö för att använda ett Forms-centrerat arbetsflöde på OSGi-funktioner. Stegen för att använda funktionerna är nu:

* [Använda ett Forms-orienterat arbetsflöde på OSGi](../../forms/using/aem-forms-workflow.md)
* [Referens för arbetsflödessteg](/help/sites-developing/workflows-step-ref.md)
* [Efterbearbetning av brev och interaktiv kommunikation](../../forms/using/submit-letter-topostprocess.md)
