---
title: Konfigurera analyser och rapporter
description: Lär dig hur du konfigurerar Adobe Analytics för att upptäcka interaktionsmönster och problem som användare ställs inför när de använder adaptiva formulär, adaptiva dokument och HTML5-formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: integrations
docset: aem65
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
feature: Adaptive Forms
exl-id: befc6b96-517b-4ca3-8007-2aa0fd6ed2cb
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1531'
ht-degree: 0%

---

# Analyser med Cloud Service Framework {#analyticsusingcloudframework}

AEM Forms kan integreras med Analytics så att ni kan hämta in och spåra prestandamått för era publicerade formulär och dokument. Syftet med att analysera dessa värden är att fatta välgrundade beslut baserat på uppgifter om de ändringar som krävs för att göra formulär eller dokument mer användbara.

>[!NOTE]
>
>Analysfunktionen i AEM Forms ingår i AEM Forms tilläggspaket. Mer information om hur du installerar tilläggspaketet finns i [Installera och konfigurera AEM Forms](../../forms/using/installing-configuring-aem-forms-osgi.md).
>
>Utöver tilläggspaketet behöver du ett Adobe Analytics-konto och administratörsbehörighet för AEM-instansen. Information om lösningen finns i [Adobe Analytics](https://www.adobe.com/solutions/digital-analytics.html).

Du kan också utföra analyser med Adobe Launch. Mer information om hur du integrerar AEM Forms med Adobe Launch finns i [Analyser med Adobe Launch](/help/forms/using/integrate-aem-forms-with-adobe-analytics.md).

## Ökning {#overview}

Du kan använda Adobe Analytics för att upptäcka interaktionsmönster och problem som användare möter när de använder adaptiva formulär, HTML5-formulär och interaktiv kommunikation. Adobe analytics håller reda på och lagrar information om följande parametrar:

* **Genomsnittlig fyllningstid**: Genomsnittlig tid för att fylla i formuläret.
* **Återgivningar**: Antal gånger ett formulär öppnas.
* **Utkast**: Antal gånger ett formulär sparas i utkastläget.
* **Inlämningar**: Antal gånger ett formulär skickas.
* **Avbryt**: Antal gånger som användarna lämnar utan att fylla i formuläret.

Du kan anpassa Adobe Analytics för att lägga till/ta bort fler parametrar. Tillsammans med ovanstående information innehåller rapporten följande information om samtliga paneler i HTML5 och anpassningsbara formulär:

* **Tid**: Tid som har ägnats åt panelen och fälten i panelen.
* **Fel**: Antal fel som påträffats på panelen och i panelens fält.
* **Hjälp**: Antal gånger en användare öppnar hjälpen för en panel och fälten på panelen.

## Skapar rapportsvit {#creating-report-suite}

Analysdata lagras i kundspecifika databaser som kallas rapportsviter. Om du vill skapa en rapportserie och använda Adobe Analytics måste du ha ett giltigt Adobe Marketing Cloud-konto. Kontrollera att du har ett giltigt Adobe Marketing Cloud-konto innan du utför följande steg.

Utför följande steg för att skapa en rapportserie.

1. Logga in på [https://sc.omniture.com/login/](https://sc.omniture.com/login/)
1. I Marketing Cloud väljer du **Admin** > **Admin Console** > **Rapportsviter**.
1. Välj **Skapa ny** > **Rapportsvit** i Report Suite Manager.

   ![Skapa ny rapportserie](assets/newreportsuite_new.png)

   Skapa ny rapportsvit

1. Kontrollera att den första listrutan är inställd på **Skapa från en mall** och välj sedan **Commerce**.
1. Leta reda på fältet **Report Suite ID** och lägg till ett nytt Report Suite-ID. Till exempel JEsquire. Ett rapportsvit-ID visas under fältet för rapportsvitens-ID. Det innehåller ett automatiskt prefix, som ofta är företagsnamnet.
1. Lägg till ny **webbplatstitel**. JJEsquire Getting Started Suite. Den här titeln används i analysgränssnittet. Använd rapportsvitens ID i koden.
1. Välj en **tidszon** i listrutan. Alla data som ingår i den här rapportsviten registreras baserat på den definierade tidszonen.
1. Lämna fälten **Bas-URL** och **Standardsida** tomma. Dessa två värden används bara från Adobe Marketing Cloud gränssnitt för att länka till din webbplats.
1. Låt **Go Live Date** vara i dag. Startdatumet avgör vilken dag rapportsviten aktiveras.
1. I fältet **Uppskattad sidvisning per dag** skriver du 100. Använd det här fältet för att uppskatta antalet sidvisningar som du förväntar dig för din webbplats per dag. Denna uppskattning gör att Adobe kan installera den maskinvara som behövs för att bearbeta de data som ska samlas in.
1. Välj en **basvaluta** i listrutan. Alla valutadata som ingår i den här rapportsviten konverteras och lagras i det här valutaformatet.
1. Klicka på **Skapa rapportsviten**. Du bör se uppdateringen av sidan med ett meddelande om att rapportsviten har skapats.
1. Välj den nya rapportsviten. Navigera till **Redigera inställningar** > **Allmänt** > **Allmänna kontoinställningar**.

   ![Allmänna kontoinställningar](assets/geographic_settings.png)

   Allmänna kontoinställningar

1. Aktivera **Geografisk rapportering** på skärmen för allmänna kontoinställningar och klicka på **Spara.**
1. Navigera till **Redigera inställningar** > **Trafik** > **Trafikvariabler**.
1. Konfigurera och aktivera följande trafikvariabler i rapportsviten.

   * **formName**: Identifierare för ett adaptivt formulär.
   * **formInstance**: Identifierare för en adaptiv formulärinstans. Aktivera sökvägsrapporter för variabeln.
   * **fieldName**: Identifierare för ett adaptivt formulärfält. Aktivera sökvägsrapporter för variabeln.
   * **panelName**: Identifierare för en adaptiv formulärpanel. Aktivera sökvägsrapporter för variabeln.
   * **formTitle**: Formulärets titel.
   * **fieldTitle**: Formulärfältets titel.
   * **panelTitle**: Formulärpanelens namn.
   * **analyticsVersion**: Version av formuläranalys.

1. Navigera till **Redigera inställningar** > **Konvertering** > **Slutförda händelser**. Definiera och aktivera följande lyckade händelser:

   | Händelsen Slutfört | Typ |
   |---|---|
   | överge | Räknare |
   | återge | Räknare |
   | panelBesök | Räknare |
   | fieldVisit | Räknare |
   | spara | Räknare |
   | fel | Räknare |
   | help | Räknare |
   | skicka | Räknare |
   | timeSpent | Numeriskt |

   >[!NOTE]
   >
   >Ett händelsenummer och ett prop-nummer som används för att konfigurera AEM Forms-analys måste skilja sig från det händelsenummer och det prop-nummer som används i konfigurationen för [AEM Analytics](/help/sites-administering/adobeanalytics.md).

1. Logga ut från Adobe Marketing Cloud-kontot.

## Skapar Cloud Service-konfiguration {#creating-cloud-service-configuration}

Cloud Service-konfigurationen är information om ditt Adobe Analytics-konto. Med konfigurationen kan Adobe Experience Manager (AEM) ansluta till Adobe Analytics. Skapa en separat konfiguration för varje Analytics-konto som du använder.

1. Logga in som administratör på din AEM-författarinstans.
1. Klicka på **Adobe Experience Manager** > **Verktyg** ![hammarikon](/help/forms/using/assets/tools.png) > **Molntjänster** > **Äldre molntjänster** i det övre vänstra hörnet.
1. Leta reda på ikonen **Adobe Analytics**. Klicka på **Visa konfigurationer** och fortsätt sedan att klicka på **[+]** för att lägga till en ny konfiguration.

   Om du är förstagångsanvändare klickar du på **Konfigurera nu**.

1. Lägg till en titel i den nya konfigurationen (det är valfritt att fylla i fältet Namn). Till exempel Min analyskonfiguration. Klicka på **Skapa**.

1. När panelen Redigera öppnas på konfigurationssidan fyller du i fälten:

   * **Företag**: Ditt företags namn som det är angivet för Adobe Analytics.
   * **Användarnamn**: Det namn som används för att logga in på Adobe Analytics.
   * **Lösenord**: Adobe Analytics-lösenordet för ovanstående konto.
   * **Datacenter**: Datacentret för ditt Adobe Analytics-konto.

1. Klicka på **Anslut till analys**. En dialogruta visas med ett meddelande om att anslutningen lyckades. Klicka på **OK**.

## Skapar Cloud Service Framework {#creating-cloud-service-framework}

Ett Adobe Analytics-ramverk är en uppsättning mappningar mellan Adobe Analytics-variabler och AEM-variabler. Använd ett ramverk för att konfigurera hur formulären fyller i data i Adobe Analytics-rapporter. Ramverk är kopplade till en Adobe Analytics-konfiguration. Du kan skapa flera ramverk för varje konfiguration.

1. På AEM molntjänstkonsol klickar du på **Visa konfigurationer** under Adobe Analytics.
1. Klicka på länken **[+]** bredvid Analytics-konfigurationen.

   ![Adobe Analytics-konfiguration](assets/adobe-analytics-cloud-services.png)

   Adobe Analytics-konfiguration

1. Ange en **rubrik** och **namn** för ramverket, välj **Adobe Analytics** Framework och klicka på **Skapa**. Ramverket öppnas för redigering.
1. Klicka på **Lägg till objekt** i delen Rapportsviter på sidopanelen och använd sedan listrutan för att välja det Report Suite-ID (till exempel JEsquire) som ramverket ska interagera med.
1. Bredvid Report Suite-ID markerar du de serverinstanser som du vill skicka information till Report Suite.

   ![information_to_send_to_report_suite](assets/information_to_send_to_report_suite.png)

1. Dra en **Form Analytics-komponent** från kategorin **other** från Sidekick till ramverket.
1. Om du vill mappa Analytics-variabler med variabler som är definierade i komponenten drar du en variabel från AEM Content Finder till ett fält i spårningskomponenten.

   ![Mappa AEM-variabler med Adobe Analytics-variabler](assets/analytics_new.png)

1. Aktivera ramverket med fliken **page** i sidosparken och klicka på **Aktivera ramverk**.

## Konfigurationstjänsten för AEM Forms Analytics konfigureras {#configuring-aem-forms-analytics-configuration-service}

1. Öppna konfigurationshanteraren för AEM Web Console på `https://<server>:<port>;/system/console/configMgr` om du har en författarinstans.
1. Leta rätt på och öppna AEM Forms Analytics-konfigurationen

   ![Konfigurationstjänsten för AEM Forms Analytics](assets/analytics_configuration.png)

   Konfigurationstjänst för AEM Forms Analytics

1. Ange lämpliga värden för följande fält och klicka på **Spara**.

   * **SiteCatalyst Framework**: Välj ramverket/konfigurationen som du definierade i avsnittet Konfigurera ett ramverk för spårning.
   * **Början för spårning av fälttid**: Ange efter hur lång tid (i sekunder) som fältbesöket måste spåras. Standardvärdet är 0. När värdet är större än 0 (noll) skickas två separata spårningshändelser till Adobe Analytics-servern. Den första händelsen instruerar analysservern att sluta spåra det avslutade fältet. Den andra händelsen skickas efter att den angivna längden har gått. Den andra händelsen instruerar analysservern att börja spåra det besökta fältet. Genom att använda två olika händelser kan du mäta tiden som läggs på ett fält på ett korrekt sätt. När värdet är 0 (noll) skickas en enda spårningshändelse till Adobe Analytics-servern.

   * **Synkroniseringscron för analysrapport**: Ange cron-uttryck för hämtning av rapporter från Adobe Analytics. Standardvärdet är 0 0 2 ? &#42; &#42;.

   * **Tidsgräns för hämtning av rapport:** Ange hur länge (i sekunder) som servern ska vänta på att analysrapporten ska svara. Standardtiden är 120 sekunder.

   >[!NOTE]
   >
   >Det kan ta upp till 10 sekunder till att tidsgränsen för hämtning av rapporter överskrids, sedan det angivna antalet sekunder.

1. Upprepa steg 1-3 på publiceringsinstansen för att konfigurera analyser.

Nu kan ni aktivera analyser för formulär och generera en analysrapport.

## Aktivera analys för ett formulär eller dokument {#enabling-analytics-for-a-form-or-document}

1. Logga in på AEM portal på `https://[hostname]:'port'`.
1. Klicka på **Forms > Forms &amp; Documents**, markera ett formulär eller dokument och klicka på **Aktivera analys**. Analysen är aktiverad.

   ![Aktivera analys för ett formulär eller dokument](assets/enable-analytics-1.png)

   Aktivera analys för ett formulär

   **A.** Knappen Aktivera analys **B.** Markerat formulär

   Mer information om hur du visar rapporter för formuläranalys finns i [Visa och förstå AEM Forms analysrapporter](../../forms/using/view-understand-aem-forms-analytics-reports.md).
