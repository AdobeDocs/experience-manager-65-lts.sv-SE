---
title: Använd batch-API för att generera flera interaktiva kommunikationer
description: Använd batch-API för att generera flera interaktiva kommunikationer
contentOwner: khsingh
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: interactive-communication
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 168cb023768ff3139937ab7f437ab7d00185bca0
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Generera flera interaktiva dokument med Batch API {#use-batch-api-to-generate-multiple-ic}

Du kan använda batch-API:t för att skapa flera interaktiva dokument från en mall. Mallen är en interaktiv kommunikation utan data. Batch-API:t kombinerar data med en mall för att skapa en interaktiv kommunikation. API:t är användbart vid massproduktion av interaktiv kommunikation. Till exempel telefonräkningar, kreditkortsutdrag för flera kunder.

Batch-API:t accepterar poster (data) i JSON-format och från en formulärdatamodell. Antalet skapade interaktiva kommunikationer är lika med de poster som anges i JSON-indatafilen i den konfigurerade formulärdatamodellen. Du kan använda API:t för att producera både utskrift och webb. Alternativet PRINT skapar ett PDF-dokument och alternativet WEB skapar data i JSON-format för varje enskild post.

## Använda batch-API {#using-the-batch-api}

Du kan använda batch-API:t med bevakade mappar eller som ett fristående Rest-API. Du konfigurerar en mall, utdatatyp (HTML, PRINT eller Båda), språkinställning, förifyllningstjänst och namn för den genererade interaktiva kommunikationen att använda API:t för gruppering.

Du kombinerar en post med en interaktiv kommunikationsmall för att skapa en interaktiv kommunikation. Batch-API:er kan läsa poster (data för interaktiva kommunikationsmallar) direkt från en JSON-fil eller från en extern datakälla som nås via formulärdatamodellen. Du kan behålla varje post i en separat JSON-fil eller skapa en JSON-array för att behålla alla poster i en enda fil.

**En enskild post i en JSON-fil**

```json
{
   "employee": {
       "name": "Sara",
       "id": 3,
       "mobileNo": 9871996463,
       "age": 37
   }
}
```

**Flera poster i en JSON-fil**

```json
[{
   "employee": {
       "name": "John",
       "id": 1,
       "mobileNo": 9871996461,
       "age": 39
   }
},{
   "employee": {
       "name": "Jacob",
       "id": 2,
       "mobileNo": 9871996462,
       "age": 38
   }
},{
   "employee": {
       "name": "Sara",
       "id": 3,
       "mobileNo": 9871996463,
       "age": 37
   }
}]
```

### Använda batch-API:t med bevakade mappar {#using-the-batch-api-watched-folders}

För att det ska vara enkelt att uppleva API:t tillhandahåller AEM Forms en tjänst för bevakad mapp som är konfigurerad att använda batch-API:t. Du kan få åtkomst till tjänsten via AEM Forms UI för att generera flera interaktiva kommunikationer. Du kan också skapa anpassade tjänster efter dina behov. Du kan använda metoderna nedan för att använda Batch-API med bevakad mapp:

* Ange indata (poster) i JSON-filformat så att du kan skapa en interaktiv kommunikation.
* Använd indata (poster) som sparats i en extern datakälla och som nås via en formulärdatamodell för att skapa en interaktiv kommunikation.

#### Ange indataposter i JSON-filformat så att du kan skapa en interaktiv kommunikation {#specify-input-data-in-JSON-file-format}

Du kombinerar en post med en interaktiv kommunikationsmall för att skapa en interaktiv kommunikation. Du kan skapa en separat JSON-fil för varje post eller skapa en JSON-array för att behålla alla poster i en enda fil:

Så här skapar du interaktiv kommunikation från poster som sparats i en JSON-fil:

1. Skapa en [bevakad mapp](/help/forms/using/creating-configure-watched-folder.md) och konfigurera den så att den använder batch-API:t:
   1. Logga in på AEM Forms författarinstans.
   1. Navigera till **[!UICONTROL Tools]** > **[!UICONTROL Forms]** > **[!UICONTROL Configure Watched Folder]**. Välj **[!UICONTROL New]**.
   1. Ange **[!UICONTROL Name]** och fysisk **[!UICONTROL Path]** för mappen. Exempel: `c:\batchprocessing`.
   1. Välj alternativet **[!UICONTROL Service]** i fältet **[!UICONTROL Process File Using]**.
   1. Välj tjänsten **[!UICONTROL com.adobe.fd.ccm.multichannel.batch.impl.service.InteractiveCommunicationBatchServiceImpl]** i fältet **[!UICONTROL Service Name]**.
   1. Ange en **[!UICONTROL Output File Pattern]**. Till exempel anger %F/ [pattern](https://experienceleague.adobe.com/docs/experience-manager-65-lts/content/forms/administrator-help/configuring-watched-folder-endpoints.html?lang=en#about-file-patterns) att den bevakade mappen kan hitta indatafiler i en undermapp till mappen Bevakade mappar\indata.
1. Konfigurera avancerade parametrar:
   1. Öppna fliken **[!UICONTROL Advanced]** och lägg till följande anpassade egenskaper:

      | Egenskap | Typ | Beskrivning |
      |--- |--- |--- |
      | templatePath | Sträng | Ange sökvägen till den interaktiva kommunikationsmall som ska användas. Exempel: `/content/dam/formsanddocuments/testsample/mediumic`. Det är en obligatorisk egenskap. |
      | recordPath | Sträng | Värdet i fältet recordPath hjälper till att ange namnet på en interaktiv kommunikation. Du kan ange sökvägen till ett fält i en post som värde för fältet recordPath. Om du till exempel anger /employee/Id blir värdet på id-fältet namn för motsvarande interaktiva kommunikation. Standardvärdet är ett [slumpmässigt UUID](https://docs.oracle.com/javase/7/docs/api/java/util/UUID.html#randomUUID()). |
      | usePrefillService | Boolean | Ställ in värdet på Falskt. Du kan använda parametern usePrefillService för att förifylla interaktiv kommunikation med data som hämtats från förifyllningstjänsten som konfigurerats för motsvarande interaktiv kommunikation. När usePrefillService är inställt på true behandlas indata-JSON-data (för varje post) som FDM-argument. Standardvärdet är false. |
      | batchType | Sträng | Ange värdet PRINT, WEB eller WEB_AND_PRINT. Standardvärdet är WEB_AND_PRINT. |
      | locale | Sträng | Ange språkinställningen för interaktiv kommunikation vid utdata. Körklara tjänster använder inte språkområdesalternativet, men du kan skapa en anpassad tjänst för att generera lokaliserad interaktiv kommunikation. Standardvärdet är en_US |

   1. Välj **[!UICONTROL Create]**.
1. Använd den bevakade mappen för att generera interaktiv kommunikation:
   1. Öppna den bevakade mappen. Navigera till indatamappen.
   1. Skapa en mapp i indatamappen och placera JSON-filen i den nya mappen.
   1. Vänta tills den bevakade mappen bearbetar filen. När bearbetningen startar flyttas indatafilen och undermappen som innehåller filen till mellanlagringsmappen.
   1. Öppna utdatamappen så att du kan visa utdata:
      * När du anger alternativet PRINT i Konfiguration av bevakad mapp, genereras PDF-utdata för den interaktiva kommunikationen.
      * När du anger alternativet WEB i Konfiguration av bevakad mapp, skapas en JSON-fil per post. Du kan använda JSON-filen för att [förifylla en webbmall](#web-template).
      * När du anger både PRINT- och WEB-alternativ genereras både PDF-dokument och en JSON-fil per post.

#### Använd indata som sparats i en extern datakälla och som nås via formulärdatamodell för att skapa en interaktiv kommunikation {#use-fdm-as-data-source}

Du kombinerar data (poster) som sparats i en extern datakälla med en interaktiv kommunikationsmall för att skapa en interaktiv kommunikation. När du skapar en interaktiv kommunikation ansluter du den till en extern datakälla via en FDM (Form Data Model) för att komma åt data. Du kan konfigurera batchbearbetningstjänsten för bevakade mappar så att data hämtas med samma formulärdatamodell från en extern datakälla. Så här [skapar du en interaktiv kommunikation från poster som har sparats i en extern datakälla](/help/forms/using/work-with-form-data-model.md):

1. Konfigurera mallens formulärdatamodell:
   1. Öppna formulärdatamodellen som är kopplad till en interaktiv kommunikationsmall.
   1. Markera MODELLOBJEKTET PÅ ÖVERSTA NIVÅ och välj Redigera egenskaper.
   1. Välj din hämtning eller få en tjänst från fältet Läs tjänst i rutan Redigera egenskaper.
   1. Välj pennikonen för lästjänstargumentet om du vill binda argumentet till ett Request Attribute och ange bindningsvärdet. Det binder tjänstargumentet till det angivna bindningsattributet eller det literala värdet, som skickas till tjänsten som ett argument för att hämta information som är associerad med det angivna värdet från datakällan.

      I det här exemplet tar id-argumentet värdet för användarprofilens id-attribut och skickar det som ett argument till lästjänsten. Den läser och returnerar värden för associerade egenskaper från medarbetardatamodellobjektet för det angivna ID:t. Om du anger 00250 i id-fältet i formuläret läser lästjänsten information om medarbetaren med 00250 employee-id.

      ![Konfigurera attribut för begäran](assets/request-attribute.png)

   1. Spara egenskaper och formulärdatamodell.
1. Konfigurera värde för begärandeattribut:
   1. Skapa en .json-fil i filsystemet och öppna den för redigering.
   1. Skapa en JSON-array och ange det primära attributet så att du kan hämta data från formulärdatamodellen. Följande JSON begär till exempel att FDM skickar data för poster där id är 27126 eller 27127:

      ```json
          [
              {
                  "id": 27126
              },
              {
                  "id": 27127
              }
          ]
      ```

   1. Spara och stäng filen.

1. Skapa en [bevakad mapp](/help/forms/using/creating-configure-watched-folder.md) och konfigurera den så att den använder API-tjänsten för grupp:
   1. Logga in på AEM Forms författarinstans.
   1. Navigera till **[!UICONTROL Tools]** > **[!UICONTROL Forms]** > **[!UICONTROL Configure Watched Folder]**. Välj **[!UICONTROL New]**.
   1. Ange **[!UICONTROL Name]** och fysisk **[!UICONTROL Path]** för mappen. Exempel: `c:\batchprocessing`.
   1. Välj alternativet **[!UICONTROL Service]** i fältet **[!UICONTROL Process File Using]**.
   1. Välj tjänsten **[!UICONTROL com.adobe.fd.ccm.multichannel.batch.impl.service.InteractiveCommunicationBatchServiceImpl]** i fältet **[!UICONTROL Service Name]**.
   1. Ange en **[!UICONTROL Output File Pattern]**. Till exempel anger %F/ [pattern](https://experienceleague.adobe.com/docs/experience-manager-65-lts/content/forms/administrator-help/configuring-watched-folder-endpoints.html?lang=en#about-file-patterns) att den bevakade mappen kan hitta indatafiler i en undermapp till mappen Bevakade mappar\indata.
1. Konfigurera avancerade parametrar:
   1. Öppna fliken **[!UICONTROL Advanced]** och lägg till följande anpassade egenskaper:

      | Egenskap | Typ | Beskrivning |
      |--- |--- |--- |
      | templatePath | Sträng | Ange sökvägen till den interaktiva kommunikationsmall som ska användas. Exempel: /content/dam/formsanddocuments/testsample/mediumic. Det är en obligatorisk egenskap. |
      | recordPath | Sträng | Värdet i fältet recordPath hjälper till att ange namnet på en interaktiv kommunikation. Du kan ange sökvägen till ett fält i en post som värde för fältet recordPath. Om du till exempel anger /employee/Id blir värdet på id-fältet namn för motsvarande interaktiva kommunikation. Standardvärdet är ett [slumpmässigt UUID](https://docs.oracle.com/javase/7/docs/api/java/util/UUID.html#randomUUID()). |  |
      | usePrefillService | Boolean | Ange värdet som Sant. Standardvärdet är false. När värdet är true läser batch-API:t data från den konfigurerade formulärdatamodellen och fyller i dem till den interaktiva kommunikationen. När usePrefillService är inställt på true behandlas indata-JSON-data (för varje post) som FDM-argument. |
      | batchType | Sträng | Ange värdet PRINT, WEB eller WEB_AND_PRINT. Standardvärdet är WEB_AND_PRINT. |
      | locale | Sträng | Ange språkinställningen för interaktiv kommunikation vid utdata. Körklara tjänster använder inte språkområdesalternativet, men du kan skapa en anpassad tjänst för att generera lokaliserad interaktiv kommunikation. Standardvärdet är en_US. |

   1. Välj **[!UICONTROL Create]**.
1. Använd den bevakade mappen för att generera interaktiv kommunikation:
   1. Öppna den bevakade mappen. Navigera till indatamappen.
   1. Skapa en mapp i indatamappen. Placera JSON-filen som skapades i steg 2 i den nya mappen.
   1. Vänta tills den bevakade mappen bearbetar filen. När bearbetningen startar flyttas indatafilen och undermappen som innehåller filen till mellanlagringsmappen.
   1. Öppna utdatamappen så att du kan visa utdata:
      * När du anger alternativet PRINT i Konfiguration av bevakad mapp, genereras PDF-utdata för den interaktiva kommunikationen.
      * När du anger alternativet WEB i Konfiguration av bevakad mapp, skapas en JSON-fil per post. Du kan använda JSON-filen för att [förifylla en webbmall](#web-template).
      * När du anger både PRINT- och WEB-alternativ genereras både PDF-dokument och en JSON-fil per post.

## Anropa batch-API:t med REST-begäranden

Du kan anropa [batch-API:t](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html) via REST-begäranden (Representational State Transfer). Med den kan du ge andra användare en REST-slutpunkt för åtkomst till API:t och konfigurera egna metoder för bearbetning, lagring och anpassning av interaktiv kommunikation. Du kan utveckla en egen anpassad Java™-server för att distribuera API:t på din AEM-instans.

Innan du distribuerar Java™-servern måste du se till att du har en interaktiv kommunikation och att motsvarande datafiler är klara. Så här skapar och distribuerar du Java™-servleten:

1. Logga in på din AEM-instans och skapa en interaktiv kommunikation. [Klicka här](assets/SimpleMediumIC.zip) om du vill använda den interaktiva kommunikation som anges i exempelkoden nedan.
1. [Skapa och distribuera ett AEM-projekt med Apache Maven](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html) på din AEM-instans.
1. Lägg till [AEM Forms Client SDK version 6.0.12 eller senare](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html) i listan över beroenden för POM-filen i ditt AEM-projekt. Exempel:

   ```xml
       <dependency>
           <groupId>com.adobe.aemfd</groupId>
           <artifactId>aemfd-client-sdk</artifactId>
           <version>6.0.122</version>
       </dependency>
   ```

1. Öppna Java™-projektet och skapa en Java-fil, till exempel CCMBatchServlet.java. Lägg till följande kod i filen:

   ```java
           package com.adobe.fd.ccm.multichannel.batch.integration;
   
           import java.io.File;
           import java.io.FileInputStream;
           import java.io.FileOutputStream;
           import java.io.IOException;
           import java.io.InputStream;
           import java.io.PrintWriter;
           import java.util.List;
           import javax.servlet.Servlet;
           import org.apache.commons.io.IOUtils;
           import org.apache.sling.api.SlingHttpServletRequest;
           import org.apache.sling.api.SlingHttpServletResponse;
           import org.apache.sling.api.servlets.SlingAllMethodsServlet;
           import org.json.JSONArray;
           import org.json.JSONObject;
           import org.osgi.service.component.annotations.Component;
           import org.osgi.service.component.annotations.Reference;
   
           import com.adobe.fd.ccm.multichannel.batch.api.builder.BatchConfigBuilder;
           import com.adobe.fd.ccm.multichannel.batch.api.factory.BatchComponentBuilderFactory;
           import com.adobe.fd.ccm.multichannel.batch.api.model.BatchConfig;
           import com.adobe.fd.ccm.multichannel.batch.api.model.BatchInput;
           import com.adobe.fd.ccm.multichannel.batch.api.model.BatchResult;
           import com.adobe.fd.ccm.multichannel.batch.api.model.BatchType;
           import com.adobe.fd.ccm.multichannel.batch.api.model.RecordResult;
           import com.adobe.fd.ccm.multichannel.batch.api.model.RenditionResult;
           import com.adobe.fd.ccm.multichannel.batch.api.service.BatchGeneratorService;
           import com.adobe.fd.ccm.multichannel.batch.util.BatchConstants;
           import java.util.Date;
   
   
           @Component(service=Servlet.class,
           property={
                   "sling.servlet.methods=GET",
                   "sling.servlet.paths="+ "/bin/batchServlet"
           })
           public class CCMBatchServlet extends SlingAllMethodsServlet {
   
               @Reference
               private BatchGeneratorService batchGeneratorService;
               @Reference
               private BatchComponentBuilderFactory batchBuilderFactory;
               public void doGet(SlingHttpServletRequest req, SlingHttpServletResponse resp) {
                   try {
                       executeBatch(req,resp);
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
               private void executeBatch(SlingHttpServletRequest req, SlingHttpServletResponse resp) throws Exception {
                   int count = 0;
                   JSONArray inputJSONArray = new JSONArray();
                   String filePath = req.getParameter("filePath");
                   InputStream is = new FileInputStream(filePath);
                   String data = IOUtils.toString(is);
                   try {
                       // If input file is json object, then create json object and add in json array, if not then try for json array
                       JSONObject inputJSON = new JSONObject(data);
                       inputJSONArray.put(inputJSON);
                   } catch (Exception e) {
                       try {
                           // If input file is json array, then iterate and add all objects into inputJsonArray otherwise throw exception
                           JSONArray inputArray = new JSONArray(data);
                           for(int i=0;i<inputArray.length();i++) {
                               inputJSONArray.put(inputArray.getJSONObject(i));
                           }
                       } catch (Exception ex) {
                           throw new Exception("Invalid JSON Data. File name : " + filePath, ex);
                       }
                   }
                   BatchInput batchInput = batchBuilderFactory.getBatchInputBuilder().setData(inputJSONArray).setTemplatePath("/content/dam/formsanddocuments/[path of the interactive communcation]").build();
                   BatchConfig batchConfig = batchBuilderFactory.getBatchConfigBuilder().setBatchType(BatchType.WEB_AND_PRINT).build();
                   BatchResult batchResult = batchGeneratorService.generateBatch(batchInput, batchConfig);
                   List<RecordResult> recordList = batchResult.getRecordResults();
                   JSONObject result = new JSONObject();
                   for (RecordResult recordResult : recordList) {
                       String recordId = recordResult.getRecordID();
                       for (RenditionResult renditionResult : recordResult.getRenditionResults()) {
                           if (renditionResult.isRecordPassed()) {
                               InputStream output = renditionResult.getDocumentStream().getInputStream();
                               result.put(recordId +"_"+renditionResult.getContentType(), output);
   
                               Date date= new Date();
                               long time = date.getTime();
   
                               // Print output
                               if(getFileExtension(renditionResult.getContentType()).equalsIgnoreCase(".json")) {
                                   File file = new File(time + getFileExtension(renditionResult.getContentType()));
                                   copyInputStreamToFile(output, file);
                               } else
                               {
                                   File file = new File(time + getFileExtension(renditionResult.getContentType()));
                                   copyInputStreamToFile(output, file);
                               }
                           }
                       }
                   }
                   PrintWriter writer = resp.getWriter();
                   JSONObject resultObj = new JSONObject();
                   resultObj.put("result", result);
                   writer.write(resultObj.toString());
               }
   
   
               private static void copyInputStreamToFile(InputStream inputStream, File file)
                       throws IOException {
   
                       try (FileOutputStream outputStream = new FileOutputStream(file)) {
   
                           int read;
                           byte[] bytes = new byte[1024];
   
                           while ((read = inputStream.read(bytes)) != -1) {
                               outputStream.write(bytes, 0, read);
                           }
   
                       }
   
                   }
   
   
               private String getFileExtension(String contentType) {
                   if (contentType.endsWith(BatchConstants.JSON)) {
                       return ".json";
                   } else return ".pdf";
               }
   
   
           }
   ```

1. Ersätt mallsökvägen (setTemplatePath) med mallsökvägen och ange värdet för setBatchType-API:t i ovanstående kod:
   * När du anger alternativet PDF för UTSKRIFT genereras utdata för den interaktiva kommunikationen.
   * När du anger WEB-alternativet genereras en JSON-fil per post. Du kan använda JSON-filen för att [förifylla en webbmall](#web-template).
   * När du anger både PRINT- och WEB-alternativ genereras både PDF-dokument och en JSON-fil per post.

1. [Använd maven för att distribuera den uppdaterade koden till din AEM-instans](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html).
1. Om du vill generera den interaktiva kommunikationen anropar du batch-API:t. Batch-API-utskriften returnerar en ström av PDF- och JSON-filer beroende på antalet poster. Du kan använda JSON-filen för att [förifylla en webbmall](#web-template). Om du använder ovanstående kod distribueras API:t på `http://localhost:4502/bin/batchServlet`. Koden skriver ut och returnerar en ström av en PDF- och en JSON-fil.

### Fyll i en webbmall i förväg {#web-template}

När du anger batchType för att återge webbkanalen genererar API:t en JSON-fil för varje datapost. Du kan använda följande syntax för att sammanfoga JSON-filen med motsvarande webbkanal för att generera en interaktiv kommunikation:

**Syntax**
`http://host:port/<template-path>/jcr:content?channel=web&mode=preview&guideMergedJsonPath=<guide-merged-json-path>`

**Exempel**
Om JSON-filen finns på `C:\batch\mergedJsonPath.json` och du använder den interaktiva kommunikationsmallen nedan: `http://host:port/content/dam/formsanddocuments/testsample/mediumic/jcr:content?channel=web`

Sedan visas webbkanalen för den interaktiva kommunikationen i följande URL på noden publish
`http://host:port/<path-to-ic>/jcr:content?channel=web&mode=preview&guideMergedJsonPath=file:///C:/batch/mergedJsonData.json`

Förutom att spara data i filsystemet kan du lagra JSON-filer i CRX-databasen, filsystemet, webbservern eller få åtkomst till data via OSGI-förifyllningstjänsten. Syntax för att sammanfoga data med olika protokoll är:

* **CRX-protokoll**
  `http://host:port/<path-to-ic>/jcr:content?channel=web&mode=preview&guideMergedJsonPath=crx:///tmp/fd/af/mergedJsonData.json`

* **Filprotokoll**
  `http://host:port/<path-to-ic>/jcr:content?channel=web&mode=preview&guideMergedJsonPath=file:///C:/Users/af/mergedJsonData.json`

* **Prefill Service-protokoll**
  `http://host:port/<path-to-ic>/jcr:content?channel=web&mode=preview&guideMergedJsonPath=service://[SERVICE_NAME]/[IDENTIFIER]`

  SERVICE_NAME refererar till namnet på OSGI-förifyllningstjänsten. Se Skapa och kör en förifyllningstjänst.

  IDENTIFIER avser alla metadata som krävs av OSGI-förifyllningstjänsten för att hämta förifyllda data. En identifierare för den inloggade användaren är ett exempel på metadata som kan användas.

* **HTTP-protokoll**
  `http://host:port/<path-to-ic>/jcr:content?channel=web&mode=preview&guideMergedJsonPath=http://localhost:8000/somesamplexmlfile.xml`

>[!NOTE]
>
>Endast CRX-protokoll är aktiverat som standard. Information om hur du aktiverar andra protokoll som stöds finns i [Konfigurera förifyllningstjänsten med Configuration Manager](https://experienceleague.adobe.com/docs/experience-manager-65-lts/content/forms/adaptive-forms-advanced-authoring/prepopulate-adaptive-form-fields.html?lang=en).
