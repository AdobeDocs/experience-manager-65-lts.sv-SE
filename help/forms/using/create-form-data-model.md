---
title: 'Självstudiekurs: Skapa formulärdatamodell '
description: Lär dig hur du konfigurerar MySQL som datakälla, skapar en formulärdatamodell (FDM), konfigurerar den och testar för AEM Forms.
contentOwner: khsingh
products: SG_EXPERIENCEMANAGER/6.3/FORMS
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Form Data Model
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 0%

---

# Självstudiekurs: Skapa formulärdatamodell {#tutorial-create-form-data-model}

![04-skapa-formulär-data-modell-huvud](assets/04-create-form-data-model-main.png)

Den här självstudiekursen är ett steg i serien [Create Your First Adaptive Form](../../forms/using/create-your-first-adaptive-form.md) . Adobe rekommenderar att du följer serien i kronologisk ordning för att förstå, utföra och demonstrera det fullständiga självstudiekurserna.

## Om självstudiekursen {#about-the-tutorial}

Med dataintegreringsmodulen för AEM [!DNL Forms] kan du skapa en formulärdatamodell från olika backend-datakällor, till exempel AEM användarprofil, RESTful-webbtjänster, SOAP-baserade webbtjänster, OData-tjänster och relationsdatabaser. Du kan konfigurera datamodellsobjekt och datatjänster i en formulärdatamodell och koppla den till ett anpassat formulär. Anpassningsbara formulärfält är bundna till objektegenskaper för datamodell. Med tjänsterna kan du förifylla det adaptiva formuläret och skriva skickade formulärdata tillbaka till datamodellobjektet.

Mer information om integrering av formulärdata och formulärdatamodell finns i [AEM Forms-dataintegrering](../../forms/using/data-integration.md).

I den här självstudiekursen får du hjälp med att förbereda, skapa, konfigurera och associera en formulärdatamodell med ett adaptivt formulär. I slutet av den här självstudiekursen kan du:

* [Konfigurera MySQL-databasen som datakälla](#config-database)
* [Skapa formulärdatamodell med MySQL-databas](#create-fdm)
* [Konfigurera formulärdatamodell](#config-fdm)
* [Testa formulärdatamodell](#test-fdm)

Formulärdatamodellen ser ut ungefär så här:

![form-data-model_l](assets/form-data-model_l.png)

**A.** Konfigurerade datakällor **B.** Datakällscheman **C.** Tillgängliga tjänster **D.** Datamodellobjekt **E.** Konfigurerade tjänster

## Förutsättningar {#prerequisites}

Kontrollera att du har följande innan du börjar:

* [!DNL MySQL]-databas med exempeldata enligt avsnittet Krav i [Skapa ditt första adaptiva formulär](../../forms/using/create-your-first-adaptive-form.md)
* OSGi-paket för [!DNL MySQL] JDBC-drivrutin enligt beskrivningen i [Paketera JDBC-databasdrivrutinen](/help/sites-developing/jdbc.md#bundling-the-jdbc-database-driver)
* Anpassat formulär enligt den första självstudiekursen [Skapa ett anpassat formulär](/help/forms/using/create-adaptive-form.md)

## Steg 1: Konfigurera MySQL-databasen som datakälla {#config-database}

Du kan konfigurera olika typer av datakällor för att skapa en formulärdatamodell. I den här självstudiekursen konfigurerar du MySQL-databasen som du har konfigurerat och fyllt i med exempeldata. Mer information om andra datakällor som stöds och hur du konfigurerar dem finns i [AEM Forms-dataintegrering](../../forms/using/data-integration.md).

Gör följande för att konfigurera din [!DNL MySQL]-databas:

1. Installera JDBC-drivrutin för databasen [!DNL MySQL] som ett OSGi-paket:

   1. Hämta [!DNL MySQL] OSGi-paket med JDBC-drivrutin från `http://www.java2s.com/ref/jar/download-orgosgiservicejdbc100jar-file.html`. <!-- This URL is an insecure link but using https is not possible -->
   1. Logga in på AEM [!DNL Forms] Author-instansen som administratör och gå till AEM webbkonsolpaket. Standard-URL:en är [https://localhost:4502/system/console/bundles](https://localhost:4502/system/console/bundles).

   1. Välj **[!UICONTROL Install/Update]**. En [!UICONTROL Upload / Install Bundles] dialogruta visas.

   1. Välj **[!UICONTROL Choose File]** om du vill bläddra och välja [!DNL MySQL] OSGi-paketet för JDBC-drivrutinen. Välj **[!UICONTROL Start Bundle]** och **[!UICONTROL Refresh Packages]** och välj **[!UICONTROL Install or Update]**. Kontrollera att JDBC-drivrutinen [!DNL Oracle Corporation's] för [!DNL MySQL] är aktiv. Drivrutinen är installerad.

1. Konfigurera databasen [!DNL MySQL] som en datakälla:

   1. Gå till AEM webbkonsol på [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr).
   1. Leta reda på konfigurationen för **Apache Sling Connection Pooled DataSource**. Välj det här alternativet om du vill öppna konfigurationen i redigeringsläge.
   1. Ange följande information i konfigurationsdialogrutan:

      * **Namn på datakälla:** Du kan ange vilket namn som helst. Ange till exempel **WeRetailMySQL**.
      * **Egenskapsnamn för DataSource-tjänsten**: Ange namnet på den tjänsteegenskap som innehåller namnet på DataSource. Den anges när datakällinstansen registreras som OSGi-tjänst. Till exempel **datakälla.namn**.
      * **JDBC-drivrutinsklass**: Ange Java™-klassnamn för JDBC-drivrutinen. För [!DNL MySQL] databas anger du **com.mysql.jdbc.Driver**.
      * **JDBC-anslutnings-URI**: Ange anslutnings-URL för databasen. För [!DNL MySQL]-databaser som körs på port 3306 och schema `weretail` är URL:en: `jdbc:mysql://'server':3306/weretail?autoReconnect=true&useUnicode=true&characterEncoding=utf-8`

      >[!NOTE]
      >
      > När databasen [!DNL MySQL] ligger bakom en brandvägg är databasvärdnamnet inte en publik DNS. IP-adressen för databasen måste läggas till i *filen /etc/hosts* på den AEM värddatorn.

      * **Användarnamn:** Databasens användarnamn. Det krävs för att JDBC-drivrutinen ska kunna upprätta en anslutning till databasen.
      * **Lösenord:** Lösenord för databasen. Det krävs för att JDBC-drivrutinen ska kunna upprätta en anslutning till databasen.

      >[!NOTE]
      >
      >AEM Forms stöder inte NT-autentisering för [!DNL MySQL]. Gå till AEM webbkonsol på [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr) och sök efter Apache Sling Connection Pooled Datasource. För egenskapen JDBC-anslutning-URI anger du värdet för IntegratedSecurity som False och använder det skapade användarnamnet och lösenordet för att ansluta till databasen [!DNL MySQL].

      * **Testa vid köp:** Aktivera alternativet **[!UICONTROL Test on Borrow]**.
      * **Testa vid retur:** Aktivera alternativet **[!UICONTROL Test on Return]**.
      * **Valideringsfråga:** Ange en SELECT-fråga för SQL för att validera anslutningar från poolen. Frågan måste returnera minst en rad. **Välj till exempel &#42; från kundinformation**.
      * **Transaktionsisolering**: Ange värdet **READ_COMMTED**.

        Lämna övriga egenskaper med standardvärdena [och ](https://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html) och välj **[!UICONTROL Save]**.

        En konfiguration som liknar följande skapas.

        ![relational-database-data-source-configuration](assets/relational-database-data-source-configuration.png)

## Steg 2: Skapa formulärdatamodell {#create-fdm}

AEM [!DNL Forms] har ett intuitivt användargränssnitt för att [skapa en formulärdatamodell](data-integration.md) från konfigurerade datakällor. Du kan använda flera datakällor i en formulärdatamodell. I det här användningsfallet kan du använda den konfigurerade [!DNL MySQL] datakällan.

Gör följande för att skapa en formulärdatamodell:

1. I AEM-författarinstans går du till **[!UICONTROL Forms]** > **[!UICONTROL Data Integrations]**.
1. Välj **[!UICONTROL Create]** > **[!UICONTROL Form Data Model]**.
1. I dialogrutan Skapa formulärdatamodell anger du ett **namn** för formulärdatamodellen. Exempel: **kundleveransfaktureringsinformation**. Välj **[!UICONTROL Next]**.
1. På skärmen Välj datakälla visas alla konfigurerade datakällor. Välj datakällan **WeRetailMySQL** och välj **[!UICONTROL Create]**.

   ![data-source-selection](assets/data-source-selection.png)

Formulärdatamodellen **Customer-shipping-billing-details** har skapats.

## Steg 3: Konfigurera formulärdatamodell {#config-fdm}

I konfigurationen av formulärdatamodellen ingår:

* lägga till datamodellobjekt och datatjänster
* konfigurera läs- och skrivtjänster för datamodellobjekt

Gör följande för att konfigurera formulärdatamodellen:

1. Gå till **[!UICONTROL Forms]** > **[!UICONTROL Data Integrations]** på AEM-författarinstansen. Standardwebbadressen är [https://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm](https://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm).
1. Formulärdatamodellen **Customer-shipping-billing-details** som du skapade tidigare visas här. Öppna den i redigeringsläge.

   Den valda datakällan **WeRetailMySQL** har konfigurerats i formulärdatamodellen.

   ![default-fdm](assets/default-fdm.png)

1. Expandera trädet för datakällan WeRailMySQL. Välj följande datamodellsobjekt och -tjänster från schemat **werdetail** > **customer details** så att du kan skapa en formulärdatamodell:

   * **Datamodellsobjekt**:

      * id
      * name
      * shippingAddress
      * stad
      * läge
      * postnummer

   * **Tjänster:**

      * få
      * uppdatera

   Välj **Lägg till markerad** om du vill lägga till markerade datamodellsobjekt och tjänster i formulärdatamodellen.

   ![WeRetail-schema](assets/weretail_schema_new.png)

   >[!NOTE]
   >
   >Standardtjänsterna för hämtning, uppdatering och infogning av JDBC-datakällor levereras med formulärdatamodell direkt.

1. Konfigurera läs- och skrivtjänster för datamodellobjektet.

   1. Markera datamodellobjektet **kundinformation** och välj **[!UICONTROL Edit Properties]**.
   1. Välj **[!UICONTROL get]** i listrutan Lästjänst. Argumentet **id**, som är den primära nyckeln i datamodellobjektet för kundinformation, läggs till automatiskt. Välj ![aem_6_3_edit](assets/aem_6_3_edit.png) och konfigurera argumentet enligt följande.

      ![read-default](assets/read-default.png)

   1. Välj på liknande sätt **[!UICONTROL update]** som skrivtjänst. Objektet **customerdetails** läggs automatiskt till som ett argument. Argumentet är konfigurerat enligt följande.

      ![write-default](assets/write-default.png)

      Lägg till och konfigurera argumentet **id** enligt följande.

      ![id-arg](assets/id-arg.png)

   1. Välj **[!UICONTROL Done]** om du vill spara datamodellens objektegenskaper. Välj **[!UICONTROL Save]** sedan för att spara formulärdatamodellen.

      Tjänsterna **[!UICONTROL get]** och **[!UICONTROL update]** läggs till som standardtjänster för datamodellsobjektet.

      ![data-modell-objekt](assets/data-model-object.png)

1. Gå till fliken **[!UICONTROL Services]** och konfigurera **[!UICONTROL get]**- och **[!UICONTROL update]**-tjänster.

   1. Välj tjänsten **[!UICONTROL get]** och välj **[!UICONTROL Edit Properties]**. Dialogrutan Egenskaper öppnas.
   1. Ange följande i dialogrutan Redigera egenskaper:

      * **Titel**: Ange tjänstens titel. Exempel: Hämta leveransadress.
      * **Beskrivning**: Ange en beskrivning som innehåller detaljerad funktionalitet för tjänsten. Till exempel:

        Den här tjänsten hämtar leveransadressen och annan kundinformation från databasen [!DNL MySQL]

      * **Utdatamodellobjekt**: Välj schema som innehåller kunddata. Till exempel:

        kundinformationsschema

      * **Returmatris**: Inaktivera alternativet **Retur-matris**.
      * **Argument**: Välj argument med namnet **ID.**

      Välj **[!UICONTROL Done]**. Tjänsten för att hämta kundinformation från MySQL-databasen har konfigurerats.

      ![shiiping-address-retrieval](assets/shiiping-address-retrieval.png)

   1. Välj tjänsten **[!UICONTROL update]** och välj **[!UICONTROL Edit Properties]**. Dialogrutan Egenskaper öppnas.

   1. Ange följande i dialogrutan [!UICONTROL Edit Properties]:

      * **Titel**: Ange tjänstens titel. Exempel: Uppdatera leveransadress.
      * **Beskrivning**: Ange en beskrivning som innehåller detaljerad funktionalitet för tjänsten. Till exempel:

        Den här tjänsten uppdaterar leveransadress och relaterade fält i MySQL-databasen

      * **Indatamodellobjekt**: Välj schema som innehåller kunddata. Till exempel:

        kundinformationsschema

      * **Utdatatyp**: Välj **BOOLEAN**.

      * **Argument**: Välj argumentnamnet **ID** och **kundinformation**.

      Välj **[!UICONTROL Done]**. Tjänsten **[!UICONTROL update]** för att uppdatera kundinformation i databasen [!DNL MySQL] har konfigurerats.

      ![shiiping-address-update](assets/shiiping-address-update.png)

Datamodellsobjektet och -tjänsterna i formulärdatamodellen har konfigurerats. Nu kan du testa formulärdatamodellen.

## Steg 4: Testa formulärdatamodell {#test-fdm}

Du kan testa datamodellsobjektet och datatjänsterna för att verifiera att formulärdatamodellen är korrekt konfigurerad.

Gör följande för att köra testet:

1. Gå till fliken **[!UICONTROL Model]**, markera datamodellobjektet **customerdetails** och välj **[!UICONTROL Test Model Object]**.
1. I fönstret [!UICONTROL Test Model/Service] väljer du **[!UICONTROL Read model object]** i listrutan **[!UICONTROL Select Model/Service]**.
1. I avsnittet **kundinformation** anger du ett värde för argumentet **id** som finns i den konfigurerade [!DNL MySQL]-databasen och väljer **[!UICONTROL Test]**.

   Kundinformationen som är kopplad till det angivna ID:t hämtas och visas i avsnittet **[!UICONTROL Output]** enligt nedan.

   ![test-read-model](assets/test-read-model.png)

1. På samma sätt kan du testa Write-modellobjektet och tjänsterna.

   I följande exempel uppdaterar uppdateringstjänsten adressinformationen för ID 7102715 i databasen.

   ![test-write-model](assets/test-write-model.png)

   Om du testar läsmodelltjänsten igen för ID 7107215 hämtas och visas den uppdaterade kundinformationen enligt nedan.

   ![läsuppdaterad](assets/read-updated.png)


>[!NOTE]
>
> Du kan skapa och använda SharePoint List-konfigurationen med hjälp av formulärdatamodellen i ett adaptivt formulär för att spara data eller skapa ett postdokument i en SharePoint-lista. Mer information finns i [Ansluta ett adaptivt formulär till Microsoft® SharePoint List](/help/forms/using/configuring-submit-actions.md#create-a-sharepoint-list-configuration).