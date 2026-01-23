---
title: Ansluta till Adobe Analytics och skapa ramverk
description: Lär dig hur du ansluter AEM till SiteCatalyst och skapar ramverk.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: a39ed93e-4276-48ff-ba49-d0f630409222
source-git-commit: abda4a719676f45388e91bbdec1421152433fce8
workflow-type: tm+mt
source-wordcount: '1487'
ht-degree: 0%

---

# Ansluta till Adobe Analytics och skapa ramverk {#connecting-to-adobe-analytics-and-creating-frameworks}

Om du vill spåra webbdata från dina AEM-sidor i Adobe Analytics skapar du en Adobe Analytics Cloud Services-konfiguration och ett Adobe Analytics-ramverk:

* **Adobe Analytics-konfiguration:** Information om ditt Adobe Analytics-konto. Med Adobe Analytics-konfigurationen kan AEM ansluta till Adobe Analytics. Skapa en Adobe Analytics-konfiguration för varje konto som du använder.
* **Adobe Analytics Framework:** En uppsättning mappningar mellan egenskaper för Adobe Analytics-rapportsviten och CQ-variabler. Använd ett ramverk för att konfigurera hur webbplatsdata fyller i dina Adobe Analytics-rapporter. Ramverk är kopplade till en Adobe Analytics-konfiguration. Du kan skapa flera ramverk för varje konfiguration.

När du associerar en webbsida med ett ramverk utför ramverket spårning för den sidan och de underordnade sidorna för den sidan. Sidvyer kan sedan hämtas från Adobe Analytics och visas i webbplatskonsolen.

## Förutsättningar {#prerequisites}

### Adobe Analytics-konto {#adobe-analytics-account}

Om du vill spåra AEM-data i Adobe Analytics måste du ha ett giltigt Adobe Experience Cloud Adobe Analytics-konto.

Adobe Analytics-kontot måste

* Har **administratörsbehörighet**
* Bli tilldelad användargruppen **Webbtjänståtkomst**.

>[!CAUTION]
>
>Det räcker inte att ange **administratörsbehörighet** (i Adobe Analytics) för att en användare ska kunna ansluta från AEM till Adobe Analytics. Kontot måste också ha behörighet för **webbtjänståtkomst**.

![chlimage_1-67](assets/chlimage_1-67.png)

Innan du fortsätter bör du kontrollera att du kan logga in på Adobe Analytics med dina inloggningsuppgifter. Med hjälp av något av följande:

* [Adobe Experience Cloud Sign In](https://experience.adobe.com/#/@login/home)

* [Adobe Analytics Sign In](https://sc.omniture.com/login/)

### Konfigurera AEM för användning av Adobe Analytics datacenter {#configuring-aem-to-use-your-adobe-analytics-data-centers}

Adobe Analytics [datacenter](https://experienceleague.adobe.com/docs/analytics/analyze/reports-analytics/reporting-interface/overview-data-collection.html) samlar in, bearbetar och lagrar data som är kopplade till din Adobe Analytics-rapportserie. Konfigurera AEM för att använda det datacenter som är värd för Adobe Analytics rapporteringsprogram. Datacentret omnämns i ditt avtal. Kontakta en administratör i organisationen om du vill ha den här informationen.

Använd följande om det behövs för att dirigera till rätt datacenter: `https://api.omniture.com/`.

Om din organisation kräver datainsamling eller datahämtning från ett specifikt datacenter använder du följande:

| Datacenter | URL |
|---|---|
| London | `https://api3.omniture.com/` |
| Singapore | `https://api4.omniture.com/` |
| Oregon | `https://api5.omniture.com/` |

Använd [webbkonsolen för att konfigurera OSGi-paketet](/help/sites-deploying/configuring-osgi.md#osgi-configuration-with-the-web-console) **Adobe AEM Analytics HTTP Client**. Lägg till **URL:en för datacenter** för det datacenter som är värd för en rapportsvit som dina AEM-sidor samlar in data för.

![aa-07](assets/aa-07.png)

1. Öppna webbkonsolen i webbläsaren. ([https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr))
1. Ange dina inloggningsuppgifter för att komma åt konsolen.

   >[!NOTE]
   >
   >Om du vill ta reda på om du har tillgång till den här konsolen kontaktar du webbplatsadministratören.

1. Markera konfigurationsobjektet **Adobe AEM Analytics HTTP Client**.
1. Om du vill lägga till URL:en för ett datacenter trycker du på plusknappen (+) bredvid listan **URL:er för datacenter** och skriver URL:en i rutan.

1. Om du vill ta bort en URL från listan klickar du på knappen - bredvid URL:en.
1. Klicka på Spara.

## Konfigurera anslutningen till Adobe Analytics {#configuring-the-connection-to-adobe-analytics}

>[!CAUTION]
>
>På grund av säkerhetsändringar i Adobe Analytics API är det inte längre möjligt att använda den version av Activity Map som ingår i AEM.
>
>Plugin-programmet [ActivityMap som tillhandahålls av Adobe Analytics](https://experienceleague.adobe.com/docs/analytics/analyze/activity-map/getting-started/get-started-users/activitymap-install.html) bör nu användas.

## Konfigurera för Activity Map {#configuring-for-the-activity-map}

>[!CAUTION]
>
>På grund av säkerhetsändringar i Adobe Analytics API är det inte längre möjligt att använda den version av Activity Map som ingår i AEM.
>
>Plugin-programmet [ActivityMap som tillhandahålls av Adobe Analytics](https://experienceleague.adobe.com/docs/analytics/analyze/activity-map/getting-started/get-started-users/activitymap-install.html) bör nu användas.

## Skapa ett Adobe Analytics Framework {#creating-a-adobe-analytics-framework}

För det Report Suite-ID (RSID) som du använder kan du styra vilka serverinstanser (författare, publicera eller båda) som ska bidra med data till Report Suite:

* **Alla**: Information från både författaren och publiceringsinstansen fyller i rapportsviten.
* **Författare**: Rapportsviten fylls bara i med information från författarinstansen.
* **Publicera**: Rapportsviten fylls bara i med information från publiceringsinstansen.

>[!NOTE]
>
>När du väljer typ av serverinstans begränsas inte anrop till Adobe Analytics, utan bara vilka anrop som innehåller RSID.
>
>Ett ramverk är till exempel konfigurerat att använda rapportsviten *diweretail* och författaren är den valda serverinstansen. När sidor publiceras tillsammans med ramverket anropas fortfarande Adobe Analytics, men dessa anrop innehåller inte RSID. Endast anrop från författarinstansen innehåller RSID.

1. Använd **Navigering**, välj **Verktyg**, **Molntjänster** och sedan **Äldre molntjänster**.
1. Bläddra till **Adobe Analytics** och välj **Visa konfigurationer**.
1. Klicka på länken **[+]** bredvid din Adobe Analytics-konfiguration.

1. I dialogrutan **Skapa ramverk**:

   * Ange en **titel**.
   * Du kan också ange **Namn** för noden som lagrar ramverksinformationen i databasen.
   * Välj **Adobe Analytics Framework**

   Klicka sedan på **Skapa**.

   Ramverket öppnas för redigering.

1. Klicka på **Lägg till objekt** i avsnittet **Rapportsviter** på sidopanelen (höger sida av huvudpanelen). Använd sedan listrutan för att välja det Report Suite-ID (till exempel `geometrixxauth`) som ramverket interagerar med.

   >[!NOTE]
   >
   >Innehållssökaren till vänster fylls i med Adobe Analytics-variabler (SiteCatalyst-variabler) när du väljer ett Report Suite-ID.

1. Om du vill välja vilka serverinstanser du vill skicka information till rapportsviten använder du listrutan **Körningsläge** (bredvid rapportsvitens ID).

   ![aa-framework-01](assets/aa-framework-01.png)

1. Klicka på **Aktivera ramverk på fliken** Sida **i sidosparken för att göra ramverket tillgängligt på publiceringsinstansen för din webbplats.**

### Konfigurera serverinställningar för Adobe Analytics {#configuring-server-settings-for-adobe-analytics}

Med ramverkssystemet kan du ändra serverinställningarna i varje Adobe Analytics-ramverk.

>[!CAUTION]
>
>De här inställningarna avgör var dina data skickas och hur, så det är viktigt att du *inte ändrar inställningarna* och låter Adobe Analytics-representanten konfigurera dem i stället.

Börja med att öppna panelen. Tryck på nedåtpilen bredvid **Servrar**:

![server_001](assets/server_001.png)

* **Spårningsserver**

   * innehåller den URL som används för att skicka Adobe Analytics-samtal

      * `cname` - används som standard för Adobe Analytics-kontots *företagsnamn*
      * `d1` - motsvarar datacentret som informationen skickas till (antingen `d1`, `d2` eller `d3`)
      * `sc.omtrdc.net` - domännamn

* **Säker spårningsserver**

   * Har samma segment som spårningsservern
   * Används för att skicka data från säkra sidor (`https://`)

* **Namnområde för besökare**

   * Namnutrymmet bestämmer den första delen av spårnings-URL:en.
   * Om du till exempel ändrar namnutrymmet till **CNAME** ser anropen till Adobe Analytics ut så att de ser ut som **CNAME.d1.omtrdc.net** i stället för som standard.

## Koppla en sida till ett Adobe Analytics Framework {#associating-a-page-with-a-adobe-analytics-framework}

När en sida är kopplad till ett Adobe Analytics-ramverk skickar sidan data till Adobe Analytics när sidan läses in. Variabler som fylls i på sidan mappas och hämtas från Adobe Analytics-variabler i ramverket. Sidvyer hämtas till exempel från Adobe Analytics.

Underordnade till sidan ärver kopplingen till ramverket. Om du till exempel associerar platsens rotsida med ett ramverk, kopplas alla sidor på webbplatsen till ramverket.

1. I konsolen **Platser** väljer du den sida som du vill konfigurera med spårning.
1. Öppna **[Sidegenskaperna](/help/sites-authoring/editing-page-properties.md)**, antingen direkt från konsolen eller sidredigeraren.
1. Öppna fliken **&#x200B; Cloud Services**.

1. Använd listrutan **Lägg till konfiguration** för att välja **Adobe Analytics** bland de tillgängliga alternativen. Om arv är plats inaktiverar du det innan väljaren blir tillgänglig.

1. Den nedrullningsbara väljaren för **Adobe Analytics** bifogas till de tillgängliga alternativen. Välj nödvändig ramverkskonfiguration.

1. Välj **Spara och stäng**.
1. Om du vill aktivera sidan och alla anslutna konfigurationer/filer **[Publicera](/help/sites-authoring/publishing-pages.md)** sidan.
1. Det sista steget är att gå till sidan i publiceringsinstansen och söka efter ett nyckelord (till exempel getFin) med komponenten **Search**.
1. Du kan sedan kontrollera anropen till Adobe Analytics med ett lämpligt verktyg, till exempel [Adobe Experience Cloud Debugger](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html).
1. I exemplet som anges ska anropet innehålla det värde som anges (dvs. eggplant) i eVar7 och händelselistan ska innehålla event3.

### Sidvyer {#page-views}

När en sida är kopplad till ett Adobe Analytics-ramverk kan antalet sidvyer visas i listvyn i webbplatskonsolen.

Mer information finns i [Visa sidanalysdata](/help/sites-authoring/page-analytics-using.md).

### Konfigurera importintervallet {#configuring-the-import-interval}

Konfigurera lämplig instans av tjänsten **Adobe AEM Analytics Report Sling Importer**:

* **Hämtningsförsök**:
Antal försök att hämta en rapport i kö.
Standardvärdet är `6`.

* **Hämtningsfördröjning**:
Antalet millisekunder mellan försök att hämta en rapport i kö.
Standardvärdet är `10000`. Eftersom detta är i millisekunder motsvarar det 10 sekunder.

* **Hämtningsfrekvens**:
Ett `cron` -uttryck som avgör frekvensen för hämtning av analysrapporten.
Standardvärdet är `0 0 0/12 * * ?`. Det motsvarar 12 hämtningar varje timme.

Om du vill konfigurera den här OSGi-tjänsten kan du antingen använda [webbkonsolen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-with-the-web-console) eller en [osgiConfig-nod i databasen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-in-the-repository) (tjänstens PID är `com.day.cq.analytics.sitecatalyst.impl.importer.ReportImporterScheduler`).

## Redigera Adobe Analytics-konfigurationer och/eller ramverk {#editing-adobe-analytics-configurations-and-or-frameworks}

På samma sätt som när du skapar en Adobe Analytics-konfiguration eller ett ramverk går du till (äldre) skärmen **Cloud Services**. Välj **Visa konfigurationer** och klicka sedan på länken till den specifika konfiguration som du vill uppdatera.

När du redigerar en Adobe Analytics-konfiguration trycker du på **Redigera** på konfigurationssidan för att öppna dialogrutan **Redigera komponent** .

## Ta bort Adobe Analytics-ramverk {#deleting-adobe-analytics-frameworks}

Om du vill ta bort ett Adobe Analytics-ramverk [öppnar du det först för redigering](#editing-adobe-analytics-configurations-and-or-frameworks).

Välj sedan **Ta bort ramverk** på fliken **Sida** i sidosparken.
