---
title: Exempel för att integrera komponent för utkast och inlämning med databas
description: Referera till implementering av anpassade data- och metadatatjänster för att integrera komponenter för utkast och inlämning i en databas.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish
solution: Experience Manager, Experience Manager Forms
feature: Forms Portal
role: Admin, User, Developer
exl-id: b9b989e3-f204-4929-a03a-857cbb786185
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1448'
ht-degree: 0%

---

# Exempel för att integrera komponent för utkast och inlämning med databas {#sample-for-integrating-drafts-submissions-component-with-database}

## Exempelöversikt {#sample-overview}

Med AEM Forms portalutkast och skicka-komponenter kan användarna spara sina formulär som utkast och skicka dem senare från vilken enhet som helst. Användarna kan även se de inskickade formulären på portalen. För att aktivera den här funktionen tillhandahåller AEM Forms data- och metadatatjänster för att lagra data som fyllts i av en användare i formuläret och de formulärmetadata som är kopplade till utkast och skickade formulär. Dessa data lagras som standard i CRX-databasen. När man interagerar med blanketter via AEM publiceringsinstans, som vanligtvis ligger utanför brandväggen, kan man vilja anpassa datalagringen för att den ska vara säkrare och tillförlitligare.

Exemplet, som behandlas i det här dokumentet, är en referensimplementering av anpassade data- och metadatatjänster för att integrera komponenter för utkast och inskickning med en databas. Databasen som används i exempelimplementeringen är **MySQL 5.6.24**. Du kan emellertid integrera komponenterna för utkast och inskickning med valfri databas.

>[!NOTE]
>
>* De exempel och konfigurationer som beskrivs i det här dokumentet är enligt MySQL 5.6.24 och du måste ersätta dem på lämpligt sätt för ditt databassystem.
>* Kontrollera att du har installerat den senaste versionen av AEM Forms tilläggspaket. En lista över tillgängliga paket finns i artikeln [AEM Forms-utgåvor](https://helpx.adobe.com/se/aem-forms/kb/aem-forms-releases.html).
>* Exempelpaketet fungerar bara med adaptiva Forms-sändningsåtgärder.

## Konfigurera och konfigurera exemplet {#set-up-and-configure-the-sample}

Utför följande steg på alla författare- och publiceringsinstanser för att installera och konfigurera exemplet:

1. Hämta följande **aem-fp-db-integration-sample-pkg-6.1.2.zip** -paket till filsystemet.

   Exempelpaket för databasintegrering

[Hämta fil](assets/aem-fp-db-integration-sample-pkg-6.1.2.zip)

1. Gå till AEM pakethanterare på https://[*host*]:[*port*]/crx/packmgr/.
1. Klicka på **[!UICONTROL Upload Package]**.

1. Bläddra till paketet **aem-fp-db-integration-sample-pkg-6.1.2.zip** och klicka på **[!UICONTROL OK]**.
1. Klicka på **[!UICONTROL Install]** bredvid paketet för att installera paketet.
1. Gå till **[!UICONTROL AEM Web Console Configuration]**
på https://[*host*]:[*port*]/system/console/configMgr.
1. Klicka för att öppna **[!UICONTROL Forms Portal Draft and Submission Configuration]** i redigeringsläge.

1. Ange värdena för egenskaperna enligt följande tabell:

   | **Egenskap** | **Beskrivning** | **Värde** |
   |---|---|---|
   | Forms Portal Draft Data Service | Identifierare för datatjänsten för utkast | formsportal.sampledataservice |
   | Forms Portal Draft Metadata Service | Identifierare för metadatatjänsten för utkast | formsportal.samplemetadataservice |
   | Forms Portal Submit Data Service | Identifierare för Skicka datatjänst | formsportal.sampledataservice |
   | Forms Portal Submit Metadata Service | Identifierare för tjänsten Skicka metadata | formsportal.samplemetadataservice |
   | Datatjänst för Forms Portal som väntar på signering | Identifierare för datatjänsten för väntande signering | formsportal.sampledataservice |
   | Metadatatjänst för Forms Portal som väntar på signering | Identifierare för metadatatjänsten för väntande signering | formsportal.samplemetadataservice |

   >[!NOTE]
   >
   >Tjänsterna matchas av deras namn som anges som värde för nyckeln `aem.formsportal.impl.prop` enligt följande:

   ```java
   @Service(value = {SubmitDataService.class, DraftDataService.class})
   @Property(name = "aem.formsportal.impl.prop", value = "formsportal.sampledataservice")
   @Service(value = { SubmitMetadataService.class, DraftMetadataService.class })
   @Property(name = "aem.formsportal.impl.prop", value = "formsportal.samplemetadataservice")
   ```

   Du kan ändra namn på data- och metadatatabellerna.

   Så här anger du ett annat namn för metadatatabellen:

   * I Web Console Configuration söker du efter och klickar på Exempel på implementering av Forms Portal Metadata Service. Du kan ändra värdena för datakälla, metadata eller ytterligare metadatatabellnamn.

   Så här anger du ett annat namn för datatabellen:

   * I Web Console Configuration söker du efter och klickar på Exempel på implementering av Forms Portal Data Service. Du kan ändra värdena för datakällan och datatabellnamnet.

   >[!NOTE]
   >
   >Om du ändrar tabellnamnen anger du dem i formulärportalskonfigurationen.

1. Låt andra konfigurationer vara som de är och klicka på **[!UICONTROL Save]**.

1. Databasanslutningen kan göras via Apache Sling Connection Pooled Data Source.
1. För Apache Sling-anslutningen söker du efter och klickar för att öppna **[!UICONTROL Apache Sling Connection Pooled DataSource]** i redigeringsläge i webbkonsolkonfigurationen. Ange värdena för egenskaperna enligt följande tabell:

<table>
 <tbody>
  <tr>
   <td><strong>Egenskap</strong></td>
   <td><strong>Värde</strong></td>
  </tr>
  <tr>
   <td>Datakällans namn</td>
   <td><p>Ett datakällnamn för filtrering av drivrutiner från datakällpoolen</p> <p><strong>Obs! </strong><em>Exempelimplementeringen använder FormsPortal som datakällnamn.</em></p> </td>
  </tr>
  <tr>
   <td>JDBC-drivrutinsklass</td>
   <td>com.mysql.jdbc.Driver</td>
  </tr>
  <tr>
   <td>JDBC-anslutnings-URI<br /> </td>
   <td>jdbc:mysql://[<em>host</em>]:[<em>port</em>]/[<em>schema_name</em>]</td>
  </tr>
  <tr>
   <td>Användarnamn</td>
   <td>Ett användarnamn för att autentisera och utföra åtgärder på databastabeller</td>
  </tr>
  <tr>
   <td>Lösenord</td>
   <td>Lösenord som är kopplat till användarnamnet</td>
  </tr>
  <tr>
   <td>Transaktionsisolering</td>
   <td>READ_COMMTED</td>
  </tr>
  <tr>
   <td>Maximalt antal aktiva anslutningar</td>
   <td>1000</td>
  </tr>
  <tr>
   <td>Maximalt antal inaktiva anslutningar</td>
   <td>100</td>
  </tr>
  <tr>
   <td>Minsta antal inaktiva anslutningar</td>
   <td>10</td>
  </tr>
  <tr>
   <td>Inledande storlek</td>
   <td>10</td>
  </tr>
  <tr>
   <td>Max. väntan</td>
   <td>100000</td>
  </tr>
  <tr>
   <td>Testa om Borgen</td>
   <td>Markerad</td>
  </tr>
  <tr>
   <td>Testa vid inaktivitet</td>
   <td>Markerad</td>
  </tr>
  <tr>
   <td>Valideringsfråga</td>
   <td>Exempelvärden är SELECT 1(mysql), välj 1 från dual(oracle), SELECT 1(MS Sql Server) (validationQuery)</td>
  </tr>
  <tr>
   <td>Timeout för verifieringsfråga</td>
   <td>10000</td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>* JDBC-drivrutinen för MySQL ingår inte i exemplet. Se till att du har etablerat dig för den och ange den information som krävs för att konfigurera JDBC-anslutningspoolen.
>* Peka författaren och publicera instanser för att använda samma databas. Värdet för URI-fältet för JDBC-anslutningen måste vara samma för alla författare- och publiceringsinstanser.

1. Låt andra konfigurationer vara som de är och klicka på **[!UICONTROL Save]**.

1. Om du redan har en tabell i databasschemat går du vidare till nästa steg.

   Om du inte redan har en tabell i databasschemat kör du följande SQL-satser för att skapa separata tabeller för data, metadata och ytterligare metadata i databasschemat:

   >[!NOTE]
   >
   >Du behöver inte olika databaser för författare och publiceringsinstanser. Använd samma databas på alla författare- och publiceringsinstanser.

   **SQL-sats för datatabell**

   ```sql
   CREATE TABLE `data` (
   `owner` varchar(255) DEFAULT NULL,
   `data` longblob,
   `metadataId` varchar(45) DEFAULT NULL,
   `id` varchar(45) NOT NULL,
   PRIMARY KEY (`id`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   ```

   **SQL-sats för metadatatabell**

   ```sql
   CREATE TABLE `metadata` (
   `formPath` varchar(1000) DEFAULT NULL,
   `formType` varchar(100) DEFAULT NULL,
   `description` text,
   `formName` varchar(255) DEFAULT NULL,
   `owner` varchar(255) DEFAULT NULL,
   `enableAnonymousSave` varchar(45) DEFAULT NULL,
   `renderPath` varchar(1000) DEFAULT NULL,
   `nodeType` varchar(45) DEFAULT NULL,
   `charset` varchar(45) DEFAULT NULL,
   `userdataID` varchar(45) DEFAULT NULL,
   `status` varchar(45) DEFAULT NULL,
   `formmodel` varchar(45) DEFAULT NULL,
   `markedForDeletion` varchar(45) DEFAULT NULL,
   `showDorClass` varchar(255) DEFAULT NULL,
   `sling:resourceType` varchar(1000) DEFAULT NULL,
   `attachmentList` longtext,
   `draftID` varchar(45) DEFAULT NULL,
   `submitID` varchar(45) DEFAULT NULL,
   `id` varchar(60) NOT NULL,
   `profile` varchar(255) DEFAULT NULL,
   `submitUrl` varchar(1000) DEFAULT NULL,
   `xdpRef` varchar(1000) DEFAULT NULL,
   `agreementId` varchar(255) DEFAULT NULL,
   `nextSigners` varchar(255) DEFAULT NULL,
   `eSignStatus` varchar(45) DEFAULT NULL,
   `pendingSignID` varchar(45) DEFAULT NULL,
   `agreementDataId` varchar(255) DEFAULT NULL,
   `enablePortalSubmit` varchar(45) DEFAULT NULL,
   `submitType` varchar(45) DEFAULT NULL,
   `dataType` varchar(45) DEFAULT NULL,
   `jcr:lastModified` varchar(45) DEFAULT NULL,
   PRIMARY KEY (`id`),
   UNIQUE KEY `ID_UNIQUE` (`id`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   ```

   **SQL-sats för ytterligare metadatakan**

   ```sql
   CREATE TABLE `additionalmetadatatable` (
   `value` text,
   `key` varchar(255) NOT NULL,
   `id` varchar(60) NOT NULL,
   PRIMARY KEY (`id`,`key`),
   CONSTRAINT 'additionalmetadatatable_fk' FOREIGN KEY (`id`) REFERENCES `metadata` (`id`) ON DELETE CASCADE
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   ```

   **SQL-sats för kommentartabell**

   ```sql
   CREATE TABLE `commenttable` (
   `commentId` varchar(255) DEFAULT NULL,
   `comment` text DEFAULT NULL,
   `ID` varchar(255) DEFAULT NULL,
   `commentowner` varchar(255) DEFAULT NULL,
   `time` varchar(255) DEFAULT NULL);
   ```

1. Om du redan har tabellerna (data, metadata och ytterligare metadata) i databasschemat kör du följande ändringsfrågor:

   **SQL-sats för att ändra datatabellen**

   ```sql
   ALTER TABLE `data` CHANGE `owner` `owner` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL;
   ```

   **SQL-sats för att ändra metadatatabellen**

   ```sql
   ALTER TABLE metadata add markedForDeletion varchar(45) DEFAULT NULL
   ```

   >[!NOTE]
   >
   >ALTER TABLE-metadatatilläggsfrågan misslyckas om du redan har kört den och kolumnen markedforDeletion finns i tabellen.

   ```sql
   ALTER TABLE metadata add agreementId varchar(255) DEFAULT NULL,
   add nextSigners varchar(255) DEFAULT NULL,
   add eSignStatus varchar(45) DEFAULT NULL,
   add pendingSignID varchar(45) DEFAULT NULL,
   add agreementDataId varchar(255) DEFAULT NULL,
   add enablePortalSubmit varchar(45) DEFAULT NULL,
   add submitType varchar(45) DEFAULT NULL,
   add dataType varchar(45) DEFAULT NULL;
   ```

   ```sql
   ALTER TABLE `metadata` CHANGE `formPath` `formPath` VARCHAR(1000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `formType` `formType` VARCHAR(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `description` `description` TEXT CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `formName` `formName` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `owner` `owner` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `renderPath` `renderPath` VARCHAR(1000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `showDorClass` `showDorClass` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `sling:resourceType` `sling:resourceType` VARCHAR(1000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `profile` `profile` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `submitUrl` `submitUrl` VARCHAR(1000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
   CHANGE `xdpRef` `xdpRef` VARCHAR(1000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL;
   ```

   **SQL-sats för att ändra tabellen med ytterligare metadata**

   ```sql
   ALTER TABLE `additionalmetadatatable` CHANGE `value` `value` TEXT CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL, CHANGE `key` `key` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL;
   ```

Exempelimplementeringen är nu konfigurerad, som du kan använda för att lista dina utkast och överföringar medan du lagrar alla data och metadata i en databas. Nu ska vi se hur data och metadatatjänster är konfigurerade i exemplet.

## Installera filen mysql-connector-java-5.1.39-bin.jar {#install-mysql-connector-java-bin-jar-file}

Utför följande steg på alla författare- och publiceringsinstanser för att installera filen mysql-connector-java-5.1.39-bin.jar:

1. Navigera till `https://'[server]:[port]'/system/console/depfinder` och sök efter paketet com.mysql.jdbc.
1. I kolumnen Exporterad av kontrollerar du om paketet exporteras av något paket.

   Fortsätt om paketet inte exporteras av något paket.

1. Navigera till `https://'[server]:[port]'/system/console/bundles` och klicka på **[!UICONTROL Install/Update]**.
1. Klicka på **[!UICONTROL Choose File]** och bläddra till filen mysql-connector-java-5.1.39-bin.jar. Markera även kryssrutorna **[!UICONTROL Start Bundle]** och **[!UICONTROL Refresh Packages]**.
1. Klicka på **[!UICONTROL Install or Update]**. Starta om servern när du är klar.
1. (*Endast Windows*) Inaktivera systemets brandvägg för ditt operativsystem.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

## Exempelkod för formulärportaldata och metadatatjänst {#sample-code-for-forms-portal-data-and-metadata-service}

Följande ZIP-adress innehåller `FormsPortalSampleDataServiceImpl` och `FormsPortalSampleMetadataServiceImpl` (implementeringsklasser) för gränssnitt för data- och metadatatjänster. Dessutom innehåller den alla klasser som krävs för kompilering av ovannämnda implementeringsklasser.

[Hämta fil](assets/sample_package.zip)

## Verifiera längden på filnamnet  {#verify-length-of-the-file-name}

Databasimplementeringen av Forms Portal använder ytterligare metadatatabell. Tabellen har en sammansatt primärnyckel baserad på kolumnerna Key och id i tabellen. MySQL tillåter primärnycklar upp till 255 tecken. Du kan använda följande valideringsskript på klientsidan för att kontrollera längden på filnamnet som är kopplat till filwidgeten. Valideringen körs när en fil bifogas. Skriptet som ges i följande procedur visar ett meddelande när filnamnet är större än 150 (inklusive filtillägg). Du kan ändra skriptet för att kontrollera om det innehåller ett annat antal tecken.

Utför följande steg för att skapa [ett klientbibliotek](/help/sites-developing/clientlibs.md) och använda skriptet:

1. Logga in på CRXDE och navigera till /etc/clientlibs/
1. Skapa en nod av typen **cq:ClientLibraryFolder** och ange namnet på noden. Exempel: `validation`.

   Klicka på **[!UICONTROL Save All]**.

1. Högerklicka på noden, klicka på **[!UICONTROL create new file]** och skapa en fil med tillägget .txt. `js.txt`Lägg till exempel till följande kod i den nyligen skapade TXT-filen och klicka på **[!UICONTROL Save All]**.

   ```javascript
   #base=util
    util.js
   ```

   I ovanstående kod är `util` namnet på mappen och `util.js` namnet på filen i mappen `util`. Mappen `util` och filen `util.js` skapas i följande steg.

1. Högerklicka på noden `cq:ClientLibraryFolder` som skapades i steg 2 och välj Skapa > Skapa mapp. Skapa en mapp med namnet `util`. Klicka på **[!UICONTROL Save All]**. Högerklicka på mappen `util` och välj Skapa > Skapa fil. Skapa en fil med namnet `util.js`. Klicka på **[!UICONTROL Save All]**.

1. Lägg till följande kod i filen util.js och klicka på **[!UICONTROL Save All]**. Koden verifierar längden på filnamnet.

   ```javascript
   /*
    * ADOBE CONFIDENTIAL
    * ___________________
    *
    * Copyright 2016 Adobe Systems Incorporated
    * All Rights Reserved.
    *
    * NOTICE:  All information contained herein is, and remains
    * the property of Adobe Systems Incorporated and its suppliers,
    * if any.  The intellectual and technical concepts contained
    * herein are proprietary to Adobe Systems Incorporated and its
    * suppliers and may be covered by U.S. and Foreign Patents,
    * patents in process, and are protected by trade secret or copyright law.
    * Dissemination of this information or reproduction of this material
    * is strictly forbidden unless prior written permission is obtained
    * from Adobe Systems Incorporated.
    *
    */
   (function () {
       var connectWithGuideBridge = function (gb) {
           gb.connect(function () {
               //For first time load
               window.guideBridge.on("elementValueChanged" , function(event, payload) {
           var component = payload.target; // Field whose value has changed
                   if(component.name == 'fileAttachment' && component.parent) {
                       var fileItems = $('#'+payload.target.parent.id).find(".guide-fu-fileItem");
                       for (i = 0;i<fileItems.length;i++) {
                           var filename = $(fileItems[i]).find(".guide-fu-fileName").text();
                           //check whether it is previously attached file or a newly  attached one
                           if(filename.length > 150 && filename.indexOf("fp.attach.jsp") < 0) {
                               window.alert("filename is larger than 150 : "+filename);
                                $(fileItems[i]).find(".guide-fu-fileClose.close").click();
                           }
                       }
                   }
   
      });
           });
       };
   
       if (window.guideBridge) {
           connectWithGuideBridge(window.guideBridge);
       } else {
           window.addEventListener("bridgeInitializeStart", function (event) {
               connectWithGuideBridge(event.detail.guideBridge);
           });
       }
   })();
   ```

   >[!NOTE]
   >
   >Skriptet är avsett för widgetkomponenten för bifogad fil som inte finns i kartongen. Om du har anpassat den färdiga widgeten för bifogade filer ändrar du skriptet ovan så att respektive ändringar införs.

1. Lägg till följande egenskap i mappen som skapades i steg 2 och klicka på **[!UICONTROL Save All]**.

   * **[!UICONTROL Name:]** kategorier

   * **[!UICONTROL Type:]**-sträng

   * **[!UICONTROL Value:]** fp.validation

   * **[!UICONTROL multi option:]** har aktiverats

1. Navigera till `/libs/fd/af/runtime/clientlibs/guideRuntime` och lägg till värdet `fp.validation` till egenskapen embed.

1. Navigera till /libs/fd/af/runtime/clientlibs/guideRuntimeWithXFA och lägg till värdet `fp.validation` för att bädda in egenskapen.

   >[!NOTE]
   >
   >Om du använder anpassade klientbibliotek i stället för klientbiblioteken guideRuntime och guideRuntimeWithXfa använder du kategorinamnet för att bädda in klientbiblioteket som skapas i den här proceduren i dina anpassade bibliotek som läses in vid körning.

1. Klicka på **[!UICONTROL Save All.]**. När filnamnet är större än 150 tecken (inklusive filtillägg) visas ett meddelande.
