---
title: Förifyll anpassningsbara formulärfält
description: Använd befintliga data för att förifylla fält i ett anpassat formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
feature: Adaptive Forms,Foundation Components
discoiquuid: 7139a0e6-0e37-477c-9e0b-aa356991d040
docset: aem65
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 69734a2b-7f9d-4661-a1e9-3bf6e362c272
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---

# Förifyll anpassningsbara formulärfält{#prefill-adaptive-form-fields}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/prepopulate-adaptive-form-fields.html).

## Introduktion {#introduction}

Du kan förifylla fälten i ett anpassat formulär med befintliga data. När en användare öppnar ett formulär är värdena för dessa fält förifyllda. Om du vill förifylla data i ett anpassat formulär gör du användardata tillgängliga som förifylld XML/JSON i det format som följer datastrukturen för förifyllda formulär.

## Struktur för förifyllda data {#the-prefill-structure}

Ett anpassningsbart formulär kan ha en blandning av bundna och obundna fält. Bundna fält är fält som dras från fliken Innehållssökare och som innehåller icke-tomma `bindRef`-egenskapsvärden i dialogrutan för fältredigering. Obundna fält dras direkt från komponentwebbläsaren i Sidekick och har ett tomt `bindRef`-värde.

Du kan förifylla både bundna och obundna fält i ett anpassat formulär. Prefill-data innehåller avsnitten afBoundData och afUnBoundData för att förifylla både bundna och obundna fält i ett adaptivt formulär. Avsnittet `afBoundData` innehåller förifyllda data för bundna fält och paneler. Dessa data måste vara kompatibla med det associerade formulärmodellschemat:

* För adaptiva formulär som använder [XFA-formulärmallen](../../forms/using/prepopulate-adaptive-form-fields.md) använder du den förifyllda XML-filen som är kompatibel med XFA-mallens dataschema.
* För adaptiva formulär som använder [XML-schema](#xml-schema-af) använder du den förifyllda XML-filen som är kompatibel med XML-schemastrukturen.
* För adaptiva formulär som använder [JSON-schema](#json-schema-based-adaptive-forms) använder du JSON-prefyllnad som är kompatibel med JSON-schemat.
* För anpassningsbara formulär med FDM-schema använder du JSON-funktionen för förifyllnad som är kompatibel med FDM-schemat.
* Det finns inga bundna data för adaptiva formulär med [ingen formulärmodell](#adaptive-form-with-no-form-model). Varje fält är ett obundet fält och är förifyllt med den obundna XML-koden.

### Exempel på XML-struktur för förifyllning {#sample-prefill-xml-structure}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<afData>
  <afBoundData>
     <employeeData>
        .
     </employeeData>
  </afBoundData>

  <afUnboundData>
    <data>
      <textbox>Hello World</textbox>
         .
         .
      <numericbox>12</numericbox>
         . 
         .              
    </data>
  </afUnboundData>
</afData>
```

### Exempel på JSON-struktur för förifyllning {#sample-prefill-json-structure}

```javascript
{
   "afBoundData": {
      "employeeData": { }
   },
   "afUnboundData": {
      "data": {
         "textbox": "Hello World",
         "numericbox": "12"
      }
   }
}
```

För bundna fält med samma bindref-fält eller obundna fält med samma namn fylls data som anges i XML-taggen eller JSON-objektet i alla fält. Två fält i ett formulär mappas till exempel till namnet `textbox` i förifyllda data. Om det första textrutefältet innehåller&quot;A&quot; fylls&quot;A&quot; automatiskt i den andra textrutan under körningen. Denna länkning kallas live-länkning av anpassningsbara formulärfält.

### Anpassat formulär med XFA-formulärmall {#xfa-based-af}

Strukturen för förifylld XML och inskickad XML för XFA-baserade adaptiva formulär är följande:

* **Förifyll XML-struktur**: XML-förifyllnad för XFA-baserade adaptiva formulär måste vara kompatibelt med XFA-formulärmallens dataschema. Om du vill förifylla obundna fält omsluter du XML-strukturen för förifyllning till taggen `/afData/afBoundData`.

* **Skickad XML-struktur**: När ingen förifylld XML används innehåller den skickade XML-filen data för både bundna och obundna fält i `afData`-wrapper-taggen. Om du använder en XML-förifyllning har den skickade XML-filen samma struktur som XML-förifyllningen. Om XML-förifyllningen börjar med rottaggen `afData` har XML-utdata också samma format. Om XML-förifyllningen inte har `afData/afBoundData`wrapper och i stället startar direkt från schemarottaggen som `employeeData`, börjar den skickade XML-filen också med taggen `employeeData`.

Prefill-Submit-Data-ContentPackage.zip

[Hämta fil](assets/prefill-submit-data-contentpackage.zip)
Exempel som innehåller förifyllda data och inlämnade data

### XML-schemabaserade adaptiva formulär  {#xml-schema-af}

Strukturen för förifylld XML och inskickad XML för adaptiva formulär baserade på XML-schema är följande:

* **Förifyll XML-struktur**: XML-förifyllning måste vara kompatibel med associerat XML-schema. Om du vill förifylla obundna fält omsluter du XML-strukturen för förifyllning i taggen /afData/afBoundData.
* **Skickad XML-struktur**: Om ingen förifylld XML används innehåller den skickade XML-filen data för både bundna och obundna fält i `afData`-wrapper-taggen. Om XML-förifyllning används har den skickade XML-filen samma struktur som XML-förifyllningen. Om förifylld XML börjar med rottaggen `afData` har utdata-XML samma format. Om XML-förifyllningen inte har `afData/afBoundData`-omslutning och i stället börjar direkt från schemarottaggen som `employeeData`, börjar den skickade XML-koden också med taggen `employeeData`.

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<xs:schema targetNamespace="https://adobe.com/sample.xsd"
            xmlns="https://adobe.com/sample.xsd"
            xmlns:xs="https://www.w3.org/2001/XMLSchema">
 
    <xs:element name="sample" type="SampleType"/>
         
    <xs:complexType name="SampleType">
        <xs:sequence>
            <xs:element name="noOfProjectsAssigned" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>
</xs:schema>
```

För fält vars modell är XML-schema är data förifyllda i taggen `afBoundData`, vilket visas i exemplet på XML nedan. Den kan användas för att förifylla ett anpassningsbara formulär med ett eller flera obundna textfält.

```xml
<?xml version="1.0" encoding="UTF-8"?><afData>
  <afUnboundData>
    <data>
      <textbox>Ignorance is bliss :) </textbox>
    </data>
  </afUnboundData>
  <afBoundData>
    <data>
      <noOfProjectsAssigned>twelve</noOfProjectsAssigned>
    </data>
  </afBoundData>
</afData>
```

>[!NOTE]
>
>Vi rekommenderar att du inte använder obundna fält i bundna paneler (paneler med `bindRef` som inte är tomma och som har skapats genom att dra komponenter från Sidekick eller fliken Datakällor). Det kan orsaka dataförlust för dessa obundna fält. Vi rekommenderar dessutom att fältnamnen är unika i hela formuläret, särskilt för obundna fält.

#### Ett exempel utan afData och afBoundData-wrapper {#an-example-without-afdata-and-afbounddata-wrapper}

```xml
<?xml version="1.0" encoding="UTF-8"?><config>
 <assignmentDetails descriptionOfAssignment="Some Science Project" durationOfAssignment="34" financeRelatedProject="1" name="Lisa" numberOfMentees="1"/>
 <assignmentDetails descriptionOfAssignment="Kidding, right?" durationOfAssignment="4" financeRelatedProject="1" name="House" numberOfMentees="3"/>
</config>
```

### JSON schemabaserade adaptiva formulär {#json-schema-based-adaptive-forms}

För adaptiva formulär baserade på JSON-schema beskrivs strukturen för JSON-förifyllnad och skickad JSON nedan. Mer information finns i [Skapa adaptiva formulär med JSON-schema](../../forms/using/adaptive-form-json-schema-form-model.md).

* **Förifyll JSON-struktur**: JSON för förifyllning måste vara kompatibel med det associerade JSON-schemat. Alternativt kan den kapslas in i /afData/afBoundData-objektet om du även vill förifylla obundna fält.
* **Skickad JSON-struktur**: Om ingen JSON för förifyllning används innehåller den skickade JSON data för både bundna och obundna fält i afData-wrapper-tagg. Om JSON för förifyllning används har den inskickade JSON samma struktur som JSON för förifyllnad. Om JSON för förifyllning börjar med afData-rotobjektet har utdata-JSON samma format. Om JSON-funktionen för förifyllning inte har wrapper afData/afBoundData och i stället startar direkt från schemarotobjektet, till exempel användaren, börjar den skickade JSON-filen också med användarobjektet.

```json
{
    "id": "https://some.site.somewhere/entry-schema#",
    "$schema": "https://json-schema.org/draft-04/schema#",
    "type": "object",
    "properties": {
        "address": {
            "type": "object",
            "properties": { 
    "name": {
     "type": "string"
    },
    "age": {
     "type": "integer"
}}}}}
```

För fält som använder JSON-schemamodell är data förifyllda i afBoundData-objektet, vilket visas i exemplet på JSON nedan. Den kan användas för att förifylla ett anpassningsbara formulär med ett eller flera obundna textfält. Nedan visas ett exempel på data med `afData/afBoundData`-wrapper:

```json
{
  "afData": {
    "afUnboundData": {
      "data": { "textbox": "Ignorance is bliss :) " }
    },
    "afBoundData": {
      "data": { {
   "user": {
    "address": {
     "city": "Noida",
     "country": "India"
}}}}}}}
```

Nedan visas ett exempel utan `afData/afBoundData`-wrapper:

```json
{
 "user": {
  "address": {
   "city": "Noida",
   "country": "India"
}}}
```

>[!NOTE]
>
>Användning av obundna fält i bundna paneler (paneler med icke-tomma bindRef som har skapats genom att dra komponenter från fliken Sidekick eller Datakällor) rekommenderas **inte** eftersom det kan orsaka dataförlust i de obundna fälten. Du bör ha unika fältnamn i hela formuläret, särskilt för obundna fält.

### Adaptiv form utan formulärmodell {#adaptive-form-with-no-form-model}

För adaptiva formulär utan formulärmodell ligger data för alla fält under taggen `<data>` för `<afUnboundData> tag`.

Observera även följande:

XML-taggarna för användardata som skickas för olika fält genereras med fältnamnet. Därför måste fältnamnen vara unika.

```xml
<?xml version="1.0" encoding="UTF-8"?><afData>
  <afUnboundData>
    <data>
      <radiobutton>2</radiobutton>
      <repeatable_panel_no_form_model>
        <numericbox>12</numericbox>
      </repeatable_panel_no_form_model>
      <repeatable_panel_no_form_model>
        <numericbox>21</numericbox>
      </repeatable_panel_no_form_model>
      <checkbox>2</checkbox>
      <textbox>Nopes</textbox>
    </data>
  </afUnboundData>
  <afBoundData/>
</afData>
```

## Konfigurera förifyllningstjänsten med Configuration Manager {#configuring-prefill-service-using-configuration-manager}

Om du vill aktivera förifyllningstjänsten anger du standardkonfigurationen för förifyllningstjänsten i AEM Web Console Configuration. Gör så här för att konfigurera förifyllningstjänsten:

>[!NOTE]
>
>Konfiguration av förifyllningstjänsten kan användas för adaptiva formulär, HTML5-formulär och HTML5-formuläruppsättningar.

1. Öppna **[!UICONTROL Adobe Experience Manager Web Console Configuration]** med URL:en:\
   https://&lt;server>:/system/console/configMgr
1. Sök och öppna **[!UICONTROL Default Prefill Service Configuration]**.

   ![Prefill configuration](assets/prefill_config_new.png)

1. Ange dataplatsen eller en region (reguljärt uttryck) för **datafilernas platser**. Exempel på giltiga platser för datafiler är:

   * file:///C:/Users/public/Document/Prefill/.&#42;
   * https://localhost:8000/somesamplexmlfile.xml

   >[!NOTE]
   >
   >Som standard är förifyllning tillåtet via crx-filer för alla typer av adaptiva Forms (XSD, XDP, JSON, FDM och utan formulärmodellbaserad). Förifyll tillåts bara med JSON- och XML-filer.

1. Förifyllningstjänsten har nu konfigurerats för ditt formulär.

   >[!NOTE]
   >
   >CRX-protokollet hanterar förfylld datasäkerhet och är därför tillåtet som standard. Förifyllnad via andra protokoll med generisk regex kan orsaka sårbarhet. I konfigurationen anger du en säker URL-konfiguration för att skydda dina data.

## Det nyfikna fallet med repeterbara paneler {#the-curious-case-of-repeatable-panels}

Vanligtvis skapas bundna (formulärschema) och obundna fält i samma adaptiva form, men följande undantag görs om bindningen är repeterbar:

* Obundna upprepningsbara paneler stöds inte för adaptiva formulär med XFA-formulärmallen, XSD-, JSON-schemat eller FDM-schemat.
* Använd inte obundna fält i bundna repeterbara paneler.

>[!NOTE]
>
>Som tumregel ska du inte blanda bundna och obundna fält om de korsas i data som fylls i av slutanvändaren i obundna fält. Om det är möjligt bör du ändra schemat eller XFA-formulärmallen och lägga till en post för obundna fält, så att den också blir bunden och dess data är tillgängliga som andra fält i skickade data.

## Protokoll som stöds för förifyllning av användardata {#supported-protocols-for-prefilling-user-data}

Anpassningsbara formulär kan förifyllas med användardata i förifyllda dataformat via följande protokoll när de konfigureras med giltig regex:

### crx:// {#the-crx-protocol}

```http
https://localhost:4502/content/forms/af/xml.html?wcmmode=disabled&dataRef=crx:///tmp/fd/af/myassets/sample.xml
```

Den angivna noden måste ha en egenskap med namnet `jcr:data` och innehålla data.

### file://  {#the-file-protocol-nbsp}

```http
https://localhost:4502/content/forms/af/someAF.html?wcmmode=disabled&dataRef=file:///C:/Users/form-user/Downloads/somesamplexml.xml
```

Den refererade filen måste finnas på samma server.

### https:// {#the-http-protocol}

```http
https://localhost:4502/content/forms/af/xml.html?wcmmode=disabled&dataRef=https://localhost:8000/somesamplexmlfile.xml
```

### service:// {#the-service-protocol}

```http
https://localhost:4502/content/forms/af/abc.html?wcmmode=disabled&dataRef=service://[SERVICE_NAME]/[IDENTIFIER]
```

* SERVICE_NAME refererar till namnet på OSGI-förifyllningstjänsten. Se [Skapa och kör en förifyllningstjänst](../../forms/using/prepopulate-adaptive-form-fields.md#create-and-run-a-prefill-service).
* IDENTIFIER avser alla metadata som krävs av OSGI-förifyllningstjänsten för att hämta förifyllda data. En identifierare för den inloggade användaren är ett exempel på metadata som kan användas.

>[!NOTE]
>
>Det går inte att skicka autentiseringsparametrar.

### Ställer in dataattribut i slingRequest {#setting-data-attribute-in-slingrequest}

Du kan också ange attributet `data` i `slingRequest`, där attributet `data` är en sträng som innehåller XML eller JSON, vilket visas i exempelkoden nedan (Exempel är för XML):

```javascript
<%
           String dataXML="<afData>" +
                            "<afUnboundData>" +
                                "<data>" +
                                    "<first_name>"+ "Tyler" + "</first_name>" +
                                    "<last_name>"+ "Durden " + "</last_name>" +
                                    "<gender>"+ "Male" + "</gender>" +
                                    "<location>"+ "Texas" + "</location>" +
                                    "</data>" +
                            "</afUnboundData>" +
                        "</afData>";
        slingRequest.setAttribute("data", dataXML);
%>
```

Du kan skriva en enkel XML- eller JSON-sträng som innehåller alla data och ange den i slingRequest. Detta kan enkelt göras i JSP-återgivningsfilen för alla komponenter som du vill inkludera på sidan där du kan ange dataattributet slingRequest.

Om du till exempel vill ha en särskild design för sidan med en viss typ av sidhuvud. För att uppnå detta kan du skriva din egen `header.jsp`, som du kan inkludera i sidkomponenten och ange attributet `data`.

Ett annat bra exempel är ett användningsexempel där du vill förifylla data vid inloggning via sociala konton som Facebook, Twitter eller LinkedIn. I det här fallet kan du inkludera en enkel JSP i `header.jsp`, som hämtar data från användarkontot och ställer in dataparametern.

prefill-page component.zip

[Hämta fil](assets/prefill-page-component.zip)
Exempel på prefill.jsp i sidkomponent

## Anpassad förifyllningstjänst för AEM Forms {#aem-forms-custom-prefill-service}

Du kan använda en anpassad förifyllningstjänst för scenarierna, där du hela tiden läser data från en fördefinierad källa. Förifyllningstjänsten läser data från definierade datakällor och fyller i fälten i det adaptiva formuläret med innehållet i datafilen för förifyllnad. Det hjälper dig även att permanent koppla förfyllda data till ett anpassat formulär.

### Skapa och köra en förifyllningstjänst {#create-and-run-a-prefill-service}

Förifyllningstjänsten är en OSGi-tjänst och paketeras via OSGi-paketet. Du skapar OSGi-paketet, överför det och installerar det i AEM Forms-paket. Innan du börjar skapa paketet:

* [Hämta AEM Forms Client SDK](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html)
* Hämta mallpaketet

* Placera datafilen (förifyllda data) i crx-databasen. Du kan placera filen på valfri plats i mappen \contents i crx-database.

[Hämta fil](assets/prefill-sumbit-xmlsandcontentpackage.zip)

#### Skapa en förifyllningstjänst {#create-a-prefill-service}

Mallpaketet (exempelpaketet för förifyllningstjänsten) innehåller exempelimplementering av AEM Forms förifyllningstjänst. Öppna mallpaketet i en kodredigerare. Öppna till exempel mallprojektet i Eclipse för redigering. När du har öppnat mallpaketet i en kodredigerare gör du följande för att skapa tjänsten.

1. Öppna src\main\java\com\adobe\test\Prefill.java för redigering.
1. I koden anger du värdet:

   * `nodePath:` Nodsökvägsvariabeln som pekar på platsen för crx-databasen innehåller sökvägen till datafilen (prefill). Till exempel /content/prefilldata.xml
   * `label:` Etikettparametern anger tjänstens visningsnamn. Exempel: Standardtjänst för förifyllnad

1. Spara och stäng filen `Prefill.java`.
1. Lägg till paketet `AEM Forms Client SDK` i standardmallprojektets byggsökväg.
1. Kompilera projektet och skapa .jar-filen för paketet.

#### Starta och använda förifyllningstjänsten {#start-and-use-the-prefill-service}

Starta förifyllningstjänsten genom att överföra JAR-filen till AEM Forms Web Console och aktivera tjänsten. Nu börjar tjänsten visas i en anpassad formulärredigerare. Så här associerar du en förifyllningstjänst till ett anpassat formulär:

1. Öppna det adaptiva formuläret i Forms Editor och öppna egenskapspanelen för formulärbehållaren.
1. Gå till AEM Forms container > Basic > Prefill Service i egenskapskonsolen.
1. Välj standardtjänsten för förifyllnad och klicka på **[!UICONTROL Save]**. Tjänsten är kopplad till formuläret.

## Förifyll data på klienten {#prefill-at-client}

När du fyller i ett anpassat formulär i förväg sammanfogar AEM Forms-servern data med ett anpassat formulär och skickar det ifyllda formuläret till dig. Som standard utförs datasammanfogningsåtgärden på servern.

Du kan konfigurera AEM Forms-servern så att den utför datasammanfogningsåtgärden på klienten i stället för på servern. Det minskar avsevärt den tid som krävs för att förifylla och återge anpassningsbara formulär. Som standard är funktionen inaktiverad. Du kan aktivera det från Configuration Manager eller kommandoraden.

* Så här aktiverar eller inaktiverar du konfigurationshanteraren:
   1. Öppna AEM Configuration Manager.
   1. Leta reda på och öppna Adaptive Form and Interactive Communication Web Channel Configuration
   1. Aktivera alternativet Configuration.af.clientside.datamerge.enabled.name
* Så här aktiverar eller inaktiverar du från kommandoraden:
   * Aktivera genom att köra följande cURL-kommando:
     `curl -u admin:admin -X POST -d apply=true \ -d propertylist=af.clientside.datamerge.enabled \ -d af.clientside.datamerge.enabled=true \ http://${crx.host}:${crx.port}/system/console/configMgr/Adaptive%20Form%20and%20Interactive%20Communication%20Web%20Channel%20Configuration`

   * Om du vill inaktivera kör du följande cURL-kommando:
     `curl -u admin:admin -X POST -d apply=true \ -d propertylist=af.clientside.datamerge.enabled \ -d af.clientside.datamerge.enabled=false \ http://${crx.host}:${crx.port}/system/console/configMgr/Adaptive%20Form%20and%20Interactive%20Communication%20Web%20Channel%20Configuration`

  Om du vill dra nytta av det förifyllda datavärdet på klienten ska du uppdatera förifyllningstjänsten så att [FileAttachmentMap](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/forms/common/service/PrefillData.html) och [CustomContext](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/forms/common/service/PrefillData.html) returneras
