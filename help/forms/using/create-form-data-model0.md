---
title: 'Självstudie: Skapa formulärdatamodell i AEM Forms'
description: Skapa formulärdatamodell för interaktiv kommunikation
contentOwner: anujkapo
products: SG_EXPERIENCEMANAGER/6.5/FORMS
docset: aem65
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2684'
ht-degree: 0%

---

# Självstudie: Skapa formulärdatamodell i AEM Forms{#tutorial-create-form-data-model}

![04-create-form-data-model-main](assets/04-create-form-data-model-main.png)

Den här självstudiekursen är ett steg i [Skapa din första interaktiva kommunikationsserie](/help/forms/using/create-your-first-interactive-communication.md). Vi rekommenderar att du följer serien i kronologisk ordning för att förstå, utföra och demonstrera det fullständiga självstudiekurserna.

## Om självstudiekursen {#about-the-tutorial}

Med AEM Forms dataintegreringsmodul kan du skapa en formulärdatamodell från olika backend-datakällor, till exempel AEM användarprofil, RESTful-webbtjänster, SOAP-baserade webbtjänster, OData-tjänster och relationsdatabaser. Du kan konfigurera datamodellsobjekt och datatjänster i en formulärdatamodell och koppla den till ett anpassat formulär. Anpassningsbara formulärfält är bundna till objektegenskaper för datamodell. Med tjänsterna kan du förifylla det adaptiva formuläret och skriva skickade formulärdata tillbaka till datamodellobjektet.

Mer information om integrering av formulärdata och formulärdatamodell finns i [AEM Forms-dataintegrering](https://helpx.adobe.com/experience-manager/6-3/forms/using/data-integration.html).

I den här självstudiekursen får du hjälp med att förbereda, skapa, konfigurera och koppla en formulärdatamodell till en interaktiv kommunikation. I slutet av den här självstudiekursen kan du:

* [Konfigurera databasen](../../forms/using/create-form-data-model0.md#step-set-up-the-database)
* [Konfigurera MySQL-databasen som datakälla](../../forms/using/create-form-data-model0.md#step-configure-mysql-database-as-data-source)
* [Skapa formulärdatamodell](../../forms/using/create-form-data-model0.md#step-create-form-data-model)
* [Konfigurera formulärdatamodell](../../forms/using/create-form-data-model0.md#step-configure-form-data-model)
* [Testa formulärdatamodell](../../forms/using/create-form-data-model0.md#step-test-form-data-model-and-services)

Formulärdatamodellen ser ut ungefär så här:

![Formulärdatamodell](assets/form_data_model_callouts_new.png)

**A.** Konfigurerade datakällor **B.** Datakällscheman **C.** Tillgängliga tjänster **D.** Datamodellobjekt **E.** Konfigurerade tjänster

## Förutsättningar {#prerequisites}

Kontrollera att du har följande innan du börjar:

* MySQL-databas med exempeldata enligt avsnittet [Konfigurera databasen](../../forms/using/create-form-data-model0.md#step-set-up-the-database).
* OSGi-paket för MySQL JDBC-drivrutin enligt beskrivningen i [Paketera JDBC-databasdrivrutinen](https://helpx.adobe.com/experience-manager/6-3/help/sites-developing/jdbc.html#bundling-the-jdbc-database-driver)

## Steg 1: Konfigurera databasen {#step-set-up-the-database}

En databas är nödvändig för att skapa en interaktiv kommunikation. I den här självstudiekursen används en databas för att visa formulärdatamodellens och beständighetsfunktionerna i interaktiv kommunikation. Konfigurera en databas som innehåller kundregister, räkningar och samtalstabeller.
Följande bild visar exempeldata för kundtabellen:

![sample_data_cust](assets/sample_data_cust.png)

Använd följande DDL-sats för att skapa tabellen **customer** i databasen.

```sql
CREATE TABLE `customer` (
   `mobilenum` int(11) NOT NULL,
   `name` varchar(45) NOT NULL,
   `address` varchar(45) NOT NULL,
   `alternatemobilenumber` int(11) DEFAULT NULL,
   `relationshipnumber` int(11) DEFAULT NULL,
   `customerplan` varchar(45) DEFAULT NULL,
   PRIMARY KEY (`mobilenum`),
   UNIQUE KEY `mobilenum_UNIQUE` (`mobilenum`)
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

Använd följande DDL-sats för att skapa tabellen **Bill** i databasen.

```sql
CREATE TABLE `bills` (
   `billplan` varchar(45) NOT NULL,
   `latepayment` decimal(4,2) NOT NULL,
   `monthlycharges` decimal(4,2) NOT NULL,
   `billdate` date NOT NULL,
   `billperiod` varchar(45) NOT NULL,
   `prevbal` decimal(4,2) NOT NULL,
   `callcharges` decimal(4,2) NOT NULL,
   `confcallcharges` decimal(4,2) NOT NULL,
   `smscharges` decimal(4,2) NOT NULL,
   `internetcharges` decimal(4,2) NOT NULL,
   `roamingnational` decimal(4,2) NOT NULL,
   `roamingintnl` decimal(4,2) NOT NULL,
   `vas` decimal(4,2) NOT NULL,
   `discounts` decimal(4,2) NOT NULL,
   `tax` decimal(4,2) NOT NULL,
   PRIMARY KEY (`billplan`)
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

Använd följande DDL-sats för att skapa tabellen **call** i databasen.

```sql
CREATE TABLE `calls` (
   `mobilenum` int(11) DEFAULT NULL,
   `calldate` date DEFAULT NULL,
   `calltime` varchar(45) DEFAULT NULL,
   `callnumber` int(11) DEFAULT NULL,
   `callduration` varchar(45) DEFAULT NULL,
   `callcharges` decimal(4,2) DEFAULT NULL,
   `calltype` varchar(45) DEFAULT NULL
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

Tabellen **call** innehåller samtalsinformation som samtalsdatum, samtalstid, samtalsnummer, samtalslängd och samtalsavgifter. Tabellen **kund** är länkad till samtalstabellen med hjälp av fältet Mobilnummer (mobilnummer). För varje mobilnummer i tabellen **customer** finns det flera poster i tabellen **call**. Du kan till exempel hämta samtalsinformationen för mobilnumret **1457892541** genom att referera till tabellen **call** .

Tabellen **Fakturor** innehåller fakturainformation som faktureringsdatum, faktureringsperiod, månadsavgifter och samtalsavgifter. Tabellen **kund** är länkad till tabellen **Bill** med hjälp av fältet Faktureringsplan. Det finns en plan som är associerad med varje kund i tabellen **kund**. Tabellen **Bill** innehåller prisinformation för alla befintliga planer. Du kan till exempel hämta avtalsinformationen för **Sarah** från tabellen **customer** och använda dessa detaljer för att hämta prisinformation från tabellen **Bill**.

## Steg 2: Konfigurera MySQL-databasen som datakälla {#step-configure-mysql-database-as-data-source}

Du kan konfigurera olika typer av datakällor för att skapa en formulärdatamodell. I den här självstudiekursen konfigurerar du MySQL-databasen som är konfigurerad och ifylld med exempeldata. Mer information om andra datakällor som stöds och hur du konfigurerar dem finns i [AEM Forms-dataintegrering](https://helpx.adobe.com/experience-manager/6-3/forms/using/data-integration.html).

Gör följande för att konfigurera MySQL-databasen:

1. Installera JDBC-drivrutin för MySQL-databas som ett OSGi-paket:

   1. Logga in på AEM Forms Author Instance som administratör och gå till AEM webbkonsolpaket. Standardwebbadressen är [https://localhost:4502/system/console/bundles](https://localhost:4502/system/console/bundles).
   1. Välj **Installera/uppdatera**. Dialogrutan **Överför/installera programpaket** visas.

   1. Välj **Välj Arkiv** om du vill bläddra och välja OSGi-paketet för MySQL JDBC-drivrutinen. Välj **Starta paket** och **Uppdatera paket** och välj **Installera** eller **Uppdatera**. Kontrollera att Oracle Corporations JDBC Driver för MySQL är aktiverad. Drivrutinen är installerad.

1. Konfigurera MySQL-databasen som en datakälla:

   1. Gå till AEM webbkonsol på [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr).
   1. Leta reda på konfigurationen för **Apache Sling Connection Pooled DataSource**. Välj det här alternativet om du vill öppna konfigurationen i redigeringsläge.
   1. Ange följande information i konfigurationsdialogrutan:

      * **Namn på datakälla:** Du kan ange vilket namn som helst. Ange till exempel **MySQL**.

      * **Egenskapsnamn för DataSource-tjänsten**: Ange namnet på den tjänsteegenskap som innehåller namnet på DataSource. Den anges när datakällinstansen registreras som OSGi-tjänst. Till exempel **datakälla.namn**.

      * **JDBC-drivrutinsklass**: Ange Java-klassnamn för JDBC-drivrutinen. Ange **com.mysql.jdbc.Driver** för MySQL-databasen.

      * **JDBC-anslutnings-URI**: Ange anslutnings-URL för databasen. För MySQL-databaser som körs på port 3306 och schematecken är URL: `jdbc:mysql://'server':3306/teleca?autoReconnect=true&useUnicode=true&characterEncoding=utf-8`
      * **Användarnamn:** Användarnamn för databasen. Det krävs för att JDBC-drivrutinen ska kunna upprätta en anslutning till databasen.
      * **Lösenord:** Lösenord för databasen. Det krävs för att JDBC-drivrutinen ska kunna upprätta en anslutning till databasen.
      * **Testa vid köp:** Aktivera alternativet **Testa vid köp**.

      * **Testa vid retur:** Aktivera alternativet **Testa vid retur**.

      * **Valideringsfråga:** Ange en SELECT-fråga för SQL för att validera anslutningar från poolen. Frågan måste returnera minst en rad. Exempel: **välj &#42; från kund**.

      * **Transaktionsisolering**: Ange värdet **READ_COMMTED**.

   Lämna övriga egenskaper med standardvärdena [för ](https://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html) och välj **Spara**.

   En konfiguration som liknar följande skapas.

   ![Apache-konfiguration](assets/apache_configuration_new.png)

## Steg 3: Skapa formulärdatamodell {#step-create-form-data-model}

AEM Forms har ett intuitivt användargränssnitt för att [skapa ett formulärdataläge](https://helpx.adobe.com/experience-manager/6-3/forms/using/data-integration.html#main-pars_header_1524967585)l från konfigurerade datakällor. Du kan använda flera datakällor i en formulärdatamodell. I den här självstudiekursen använder du MySQL som datakälla.

Gör följande för att skapa formulärdatamodell:

1. I AEM-författarinstans går du till **Forms** > **Dataintegreringar**.
1. Välj **Skapa** > **Formulärdatamodell**.
1. I guiden Skapa formulärdatamodell anger du **namn** för formulärdatamodellen. Till exempel **FDM_Create_First_IC**. Välj **Nästa**.
1. På skärmen Välj datakälla visas alla konfigurerade datakällor. Välj datakällan **MySQL** och välj **Create**.

   ![MYSQL-datakälla](assets/fdm_mysql_data_source_new.png)

1. Klicka på **Klar**. Formulärdatamodellen **FDM_Create_First_IC** har skapats.

## Steg 4: Konfigurera formulärdatamodell {#step-configure-form-data-model}

I konfigurationen av formulärdatamodellen ingår:

* [lägga till datamodellsobjekt och -tjänster](#add-data-model-objects-and-services)
* [skapa beräknade underordnade egenskaper för datamodellobjekt](#create-computed-child-properties-for-data-model-object)
* [lägga till associationer mellan datamodellobjekt](#add-associations-between-data-model-objects)
* [redigera objektegenskaper för datamodell](#edit-data-model-object-properties)
* [konfigurera tjänster för datamodellobjekt](#configure-services)

### Lägga till datamodellsobjekt och -tjänster {#add-data-model-objects-and-services}

1. I AEM-författarinstans går du till **Forms** > **Dataintegreringar**. Standardwebbadressen är [https://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm](https://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm).
1. Den formulärdatamodell **FDM_Create_First_IC** som du skapade tidigare visas här. Markera den och välj **Redigera**.

   Den markerade datakällan **MySQL** visas i rutan **Datakällor**.

   ![MYSQL-datakälla för FDM](assets/mysql_fdm_new.png)

1. Expandera datakällträdet **MySQL**. Välj följande datamodellsobjekt och -tjänster från schemat **teleca**:

   * **Datamodellsobjekt**:

      * växlar
      * samtal
      * kund

   * **Tjänster:**

      * get
      * uppdatera

   Välj **Lägg till markerad** om du vill lägga till markerade datamodellsobjekt och tjänster i formulärdatamodellen.

   ![Välj datamodellsobjekttjänster](assets/select_data_model_object_services_new.png)

   De räkningar, anrop och kunddatamodellsobjekt visas i den högra rutan på fliken **Modell**. Hämtnings- och uppdateringstjänsterna visas på fliken **Tjänster**.

   ![Datamodellsobjekt](assets/data_model_objects_new.png)

### Skapa beräknade underordnade egenskaper för datamodellobjekt {#create-computed-child-properties-for-data-model-object}

En beräknad egenskap är den vars värde beräknas baserat på en regel eller ett uttryck. Med hjälp av en regel kan du ange värdet för en beräknad egenskap till en litteral sträng, ett tal, resultatet av ett matematiskt uttryck eller värdet för en annan egenskap i formulärdatamodellen.

Baserat på användningsfallet skapar du den underordnade beräknade egenskapen **usagecharges** i datamodellsobjektet **Bill** med hjälp av följande matematiska uttryck:

* Användningsavgifter = samtalsavgifter + konferenssamtalsavgifter + SMS-avgifter + mobilinternetavgifter + roaming nationellt + roaming internationellt + VAS (alla dessa egenskaper finns i räkningens datamodell)
Mer information om den underordnade beräknade egenskapen **usagecharges** finns i [Planera interaktiv kommunikation](/help/forms/using/planning-interactive-communications.md).

Utför följande steg för att skapa beräknade underordnade egenskaper för datamodellobjektet för räkningar:

1. Markera kryssrutan högst upp i datamodellsobjektet **Bill** för att markera den och välj **Skapa underordnad egenskap**.
1. I rutan **Skapa underordnad egenskap**:

   1. Ange **usagecharges** som namn på den underordnade egenskapen.
   1. Aktivera **Beräknad**.
   1. Välj **Float** som typ och välj **Done** om du vill lägga till den underordnade egenskapen i datamodellobjektet **Bill**.

   ![Skapa underordnad egenskap](assets/create_child_property_new.png)

1. Välj **Redigera regel** för att öppna regelredigeraren.
1. Välj **Skapa**. Regelfönstret **Ange värde** öppnas.
1. Välj **Matematiskt uttryck** i listrutan Välj alternativ.

   ![Regelredigerare för användningsavgifter](assets/usage_charges_rule_editor_new.png)

1. I det matematiska uttrycket väljer du **callCharts** och **conccallCharts** som första respektive andra objekt. Välj **plus** som operator. Välj i det matematiska uttrycket och välj **Utöka uttryck** om du vill lägga till **bilder**, **internetavgifter**, **roamingNational**, **roamingInl** och **vas** -objekt i uttrycket.

   Följande bild visar det matematiska uttrycket i regelredigeraren:

   ![Användningsavgiftsregel](assets/usage_charges_rule_all_new.png)

1. Välj **Klar**. Regeln skapas i regelredigeraren.
1. Välj **Stäng** om du vill stänga regelredigeringsfönstret.

### Lägga till associationer mellan datamodellsobjekt {#add-associations-between-data-model-objects}

När datamodellsobjekten har definierats kan du skapa associationer mellan dem. Associationen kan vara en-till-en eller en-till-många. Det kan till exempel finnas flera beroenden som är kopplade till en medarbetare. Den kallas en-till-många-association och avbildas med 1:n på linjen som förbinder associerade datamodellsobjekt. Om en association returnerar ett unikt medarbetarnamn för ett givet medarbetar-ID kallas den en-till-en-association.

När du lägger till associerade datamodellobjekt i en datakälla i en formulärdatamodell behålls deras associationer och visas som kopplade med pilrader.

Baserat på användningsfallet skapar du följande associationer mellan datamodellsobjekten:

| Association | Datamodellsobjekt |
|---|---|
| 1:n | kund:samtal (flera samtal kan kopplas till en kund i en månadsfaktura) |
| 1:1 | kund:växlar (en faktura är kopplad till en kund för en viss månad) |

Utför följande steg för att skapa associationer mellan datamodellsobjekt:

1. Markera kryssrutan högst upp i datamodellobjektet **customer** för att markera det och välj **Lägg till association**. Egenskapsrutan **Lägg till association** öppnas.
1. I rutan **Lägg till association**:

   * Ange en titel för associationen. Det är ett valfritt fält.
   * Välj **Ett till många** i listrutan **Typ**.

   * Välj **anrop** i listrutan **Modellobjekt**.

   * Välj **get** i listrutan **Service**.

   * Välj **Lägg till** om du vill länka datamodellobjektet **customer** till datamodellobjektet **call** med hjälp av en egenskap. Baserat på användningsfallet måste anropsdatamodellsobjektet länkas till mobilnummeregenskapen i kunddatamodellsobjektet. Dialogrutan **Lägg till argument** öppnas.

   ![Lägg till association](assets/add_association_new.png)

1. I dialogrutan **Lägg till argument**:

   * Välj **mobilenum** i listrutan **Namn**. Egenskapen för mobilnummer är en vanlig egenskap som är tillgänglig i kunden och anropar datamodellsobjekt. Det innebär att det används för att skapa en association mellan kund- och anropsdatamodellsobjekt.
För varje mobilnummer som är tillgängligt i kunddatamodellobjektet finns det flera samtalsposter tillgängliga i samtalstabellen.

   * Ange en valfri titel och beskrivning för argumentet.
   * Välj **kund** i listrutan **Bindning till**.

   * Välj **mobilenum** i listrutan **Bindningsvärde**.

   * Välj **Lägg till**.

   ![Lägg till association för ett argument](assets/add_association_argument_new.png)

   Mobile-egenskapen visas i avsnittet **Arguments**.

   ![Lägg till argumentassociation](assets/add_argument_association_new.png)

1. Välj **Klar** om du vill skapa en 1:n-association mellan kund- och anropsdatamodellsobjekt.

   När du har skapat en association mellan kund- och anropsdatamodellsobjekt skapar du en 1:1-association mellan kunden och faktureringsdatamodellsobjekten.

1. Markera kryssrutan högst upp i datamodellobjektet **customer** för att markera det och välj **Lägg till association**. Egenskapsrutan **Lägg till association** öppnas.
1. I rutan **Lägg till association**:

   * Ange en titel för associationen. Det är ett valfritt fält.
   * Välj **En till en** i listrutan **Typ**.

   * Välj **strukturer** i listrutan **Modellobjekt**.

   * Välj **get** i listrutan **Service**. Egenskapen **billplan**, som är den primära nyckeln för räkningstabellen, är redan tillgänglig i avsnittet **Argument**.
Fakturorna och modellobjekten för kunddata länkas med egenskaperna för faktureringsplanen (räkningarna) respektive kundplanen (kunden). Skapa en bindning mellan de här egenskaperna för att hämta avtalsinformationen för alla kunder som är tillgängliga i MySQL-databasen.

   * Välj **kund** i listrutan **Bindning till**.

   * Välj **kundplan** i listrutan **Bindningsvärde**.

   * Välj **Klar** om du vill skapa en bindning mellan egenskaperna för faktureringsplanen och kundplanen.

   ![Lägg till association för kundfaktura](assets/add_association_customer_bills_new.png)

   I följande bild visas associationerna mellan datamodellsobjekten och egenskaperna som används för att skapa associationer mellan dem:

   ![fdm_association](assets/fdm_associations.gif)

### Redigera objektegenskaper för datamodell {#edit-data-model-object-properties}

När du har skapat associationer mellan kunden och andra datamodellsobjekt kan du redigera kundegenskaperna för att definiera den egenskap som data hämtas från datamodellsobjektet. Baserat på användningsfallet används mobilnummer som egenskap för att hämta data från kunddatamodellsobjektet.

1. Markera kryssrutan högst upp i datamodellobjektet **customer** för att markera det och välj **Redigera egenskaper**. Fönstret **Redigera egenskaper** öppnas.
1. Ange **kund** som **modellobjekt på översta nivån**.
1. Välj **get** i listrutan **Lästjänst**.
1. I avsnittet **Argument**:

   * Välj **Begär attribut** i listrutan **Bindning till**.

   * Ange **mobilenum** som bindningsvärde.

1. Välj **update** i listrutan **Write** Service.
1. I avsnittet **Argument**:

   * För egenskapen **mobilenum** väljer du **customer** i listrutan **Bindning till**.

   * Välj **mobilenum** i listrutan **Bindningsvärde**.

1. Välj **Klar** om du vill spara egenskaperna.

   ![Konfigurera tjänster](assets/configure_services_customer_new.png)

1. Markera kryssrutan högst upp i datamodellobjektet **call** för att markera det och välj **Redigera egenskaper**. Fönstret **Redigera egenskaper** öppnas.
1. Inaktivera **översta nivåmodellobjektet** för datamodellobjektet **call**.
1. Välj **Klar**.

   Upprepa steg 8-10 för att konfigurera egenskaperna för datamodellobjektet **Bill**.

### Konfigurera tjänster {#configure-services}

1. Gå till fliken **Tjänster**.
1. Välj tjänsten **get** och välj **Redigera egenskaper**. Fönstret **Redigera egenskaper** öppnas.
1. I rutan **Redigera egenskaper**:

   * Ange en valfri titel och beskrivning.
   * Välj **kund** i listrutan **Utdatamodellsobjekt**.

   * Välj **Klar** om du vill spara egenskaperna.

   ![Redigera egenskaper](assets/edit_properties_get_details_new.png)

1. Markera **uppdateringstjänsten** och välj **Redigera egenskaper**. Fönstret **Redigera egenskaper** öppnas.
1. I rutan **Redigera egenskaper**:

   * Ange en valfri titel och beskrivning.
   * Välj **kund** i listrutan **Indatamodellsobjekt**.

   * Välj **Klar**.
   * Välj **Spara** om du vill spara formulärdatamodellen.

   ![Uppdatera tjänstegenskaper](assets/update_service_properties_new.png)

## Steg 5: Testa formulärdatamodell och -tjänster {#step-test-form-data-model-and-services}

Du kan testa datamodellsobjektet och datatjänsterna för att verifiera att formulärdatamodellen är korrekt konfigurerad.

Gör följande för att köra testet:

1. Gå till fliken **Modell**, markera datamodellobjektet **customer** och välj **Testmodellobjekt**.
1. I fönstret **Testa formulärdatamodell** väljer du **Läs modellobjekt** i listrutan **Välj modell/tjänst**.
1. I avsnittet **Indata** anger du ett värde för egenskapen **mobilenum** som finns i den konfigurerade MySQL-databasen och väljer **Test**.

   Kundinformationen som är associerad med den angivna mobilegenskapen hämtas och visas i utdataavsnittet enligt nedan. Stäng dialogrutan.

   ![Testa datamodell](assets/test_data_model_new.png)

1. Gå till fliken **Tjänster**.
1. Välj tjänsten **get** och välj **Testtjänst.**
1. I avsnittet **Indata** anger du ett värde för egenskapen **mobilenum** som finns i den konfigurerade MySQL-databasen och väljer **Test**.

   Kundinformationen som är associerad med den angivna mobilegenskapen hämtas och visas i utdataavsnittet enligt nedan. Stäng dialogrutan.

   ![Testtjänst](assets/test_service_new.png)

### Redigera och spara exempeldata {#edit-and-save-sample-data}

Med formulärdatamodellredigeraren kan du generera exempeldata för alla datamodellsobjektsegenskaper, inklusive beräknade egenskaper, i en formulärdatamodell. Det är en uppsättning slumpmässiga värden som överensstämmer med den datatyp som konfigurerats för varje egenskap. Du kan också redigera och spara data, som behålls även om du genererar om exempeldata.

Gör följande för att generera, redigera och spara exempeldata:

1. Välj **Redigera exempeldata** på formulärdatamodellsidan. Den genererar och visar exempeldata i fönstret Redigera exempeldata.

   ![Redigera exempeldata](assets/edit_sample_data_new.png)

1. I fönstret **Redigera exempeldata** redigerar du data efter behov och väljer **Spara**. Stäng fönstret.
