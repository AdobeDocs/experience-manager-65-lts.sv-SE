---
title: Använd metadata i ett e-postmeddelande
description: Använd metadata för att fylla i information i e-postmeddelanden i formulärarbetsflödet
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: 64d4ef01-ee33-4c8b-977f-0c9b31755820
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '868'
ht-degree: 0%

---

# Använd metadata i ett e-postmeddelande {#use-metadata-in-an-email-notification}

Du kan använda steget Tilldela uppgift för att skapa och tilldela uppgifter till en användare eller grupp. När en uppgift tilldelas en användare eller grupp skickas ett e-postmeddelande till den angivna användaren eller till varje medlem i den definierade gruppen. Ett vanligt [e-postmeddelande](../../forms/using/use-custom-email-template-assign-task-step.md) innehåller en länk till den tilldelade aktiviteten och information som är relaterad till aktiviteten.

Du kan använda metadata i en e-postmall för att dynamiskt fylla i information i ett e-postmeddelande. Värdet på titeln, beskrivningen, förfallodatumet, prioriteten, arbetsflödet och det sista datumet i följande e-postmeddelande väljs dynamiskt vid körningen (när ett e-postmeddelande genereras).

![Standardmall för e-post](assets/default_email_template_metadata_new.png)

Metadata lagras i nyckelvärdepar. Du kan ange nyckeln i e-postmallen och nyckeln ersätts med ett värde vid körningen (när ett e-postmeddelande genereras). I exemplet nedan är &quot;$ {workitem_title}&quot; en nyckel. Den ersätts med värdet&quot;Lånebegäran&quot; vid körningen.

```html
subject=Task Assigned - ${workitem_title}

message=<html><body>\n\
 <table style="width: 480px; font-family: Helvetica, Arial, sans-serif; border: 0; padding: 0; vertical-align: top; text-align: left; word-wrap: break-word; margin: 16px auto; color:#323232; background-color:#FFFCF9; border-collapse: collapse;">\n\
  <tbody>\n\
   <tr>\n\
    <td style="height: 100px; width: 480px; background-color: #FFE0CB; border-top: 5pt solid black; font-family: Helvetica, Arial, sans-serif; font-weight: bold; font-size: 15px; line-height: 20px; padding: 12px; color: #707070;">\n\
      Sample Company\n\
    </td>\n\
   </tr>\n\
   <tr>\n\
    <td style="font-family: Helvetica, Arial, sans-serif; height: auto; background-color: #FFFCF9; padding: 32px 16px 20px 16px; ">\n\
     <pre style="font-size: 13px; font-family: Helvetica, Arial, sans-serif;  font-weight: normal; color: #323232;"> Hello ${workitem_assignee},\n\
 The following task has been assigned to you:</pre>\n\
    </td>\n\
   </tr>\n\
   <tr>\n\
    <td style="width: 480px;">\n\
     <table style="height: auto; width: 480px; background-color:#FFFBF9; font-family: Helvetica, Arial, sans-serif; border-collapse: collapse;">\n\
      <tbody>\n\
       <tr style="border-bottom: solid 2px #FFFCF9;">\n\
        <td style="font-family: Helvetica, Arial, sans-serif; width: auto; height: auto; background-color:#FFF5EF; font-weight: bold; font-size: 11px; line-height: 20px; padding: 12px; color: #707070;"> TITLE</td>\n\
        <td style="font-family: Helvetica, Arial, sans-serif; background-color:#FFF5EF; text-align: left; vertical-align: middle; height: auto; font-weight: normal; font-size: 13px; line-height: 20px; padding: 10px 16px 10px 32px; color: #323232;">\n\
         <p>${workitem_title}</p>\n\
        </td>\n\
       </tr>\n\
                            <tr style="border-bottom: solid 2px #FFFCF9;">\n\
        <td style="font-family: Helvetica, Arial, sans-serif; width: auto; height: auto; background-color:#FFF5EF; font-weight: bold; font-size: 11px; line-height: 20px; padding: 12px; color: #707070;"> DESCRIPTION</td>\n\
        <td style="font-family: Helvetica, Arial, sans-serif; background-color:#FFF5EF; text-align: left; vertical-align: middle; height: auto; font-weight: normal; font-size: 13px; line-height: 20px; padding: 10px 16px 10px 32px; color: #323232;">\n\
         <p>${workitem_description}</p>\n\
        </td>\n\
       </tr>\n\
       <tr style="border-bottom: solid 2px #FFFCF9;">\n\
        <td style="font-family: Helvetica, Arial, sans-serif; width: auto; height: auto; background-color:#FFF5EF; font-weight: bold; font-size: 11px; line-height: 20px; padding: 12px; color: #707070;"> DUE DATE</td>\n\
        <td style="font-family: Helvetica, Arial, sans-serif; background-color:#FFF5EF; text-align: left; vertical-align: middle; height: auto; font-weight: normal; font-size: 13px; line-height: 20px; padding: 10px 16px 10px 32px; color: #323232;">\n\
         <p>${workitem_due_date}</p>\n\
        </td>\n\
       </tr>\n\
       <tr style="border-bottom: solid 2px #FFFCF9;">\n\
        <td style="font-family: Helvetica, Arial, sans-serif; width: auto; height: auto; background-color:#FFF5EF; font-weight: bold; font-size: 11px; line-height: 20px; padding: 12px; color: #707070;"> PRIORITY</td>\n\
        <td style="font-family: Helvetica, Arial, sans-serif; background-color:#FFF5EF; text-align: left; vertical-align: middle; height: auto; font-weight: normal; font-size: 13px; line-height: 20px; padding: 10px 16px 10px 32px; color: #323232;">\n\
         <p>${workitem_priority}</p>\n\
        </td>\n\
       </tr>\n\
       <tr>\n\
        <td style="font-family: Helvetica, Arial, sans-serif; width: auto; height: auto; background-color:#FFF5EF; font-weight: bold; font-size: 11px; line-height: 20px; padding: 12px; color: #707070;"> WORKFLOW</td>\n\
        <td style="font-family: Helvetica, Arial, sans-serif; background-color:#FFF5EF; text-align: left; vertical-align: middle; height: auto; font-weight: normal; font-size: 13px; line-height: 20px; padding: 10px 16px 10px 32px; color: #323232;">\n\
         <p>${workitem_workflow}</p>\n\
        </td>\n\
       </tr>\n\
      </tbody>\n\
     </table>\n\
    </td>\n\
   </tr>\n\
   <tr style = "text-align: center; vertical-align: middle;">\n\
    <td style="padding:48px 0 72px 0;"> \n\
     <a href="${workitem_url}" target="_blank" style="background-color: #1EBBBB; font-size: 18px; line-height: 25px; font-weight: bold; color: #FFFFFF; text-decoration: none; padding: 15px 15px 15px 15px;">Open Task</a>\n\
    </td>\n\
   </tr>\n\
   <tr>\n\
    <td style="border-top: solid 1px #EDEAE7; padding: 16px;">\n\
     <p><span style="font-size: 12px; font-weight: normal; font-style: italic; color: #919191;">This is an automatically generated email. Do not reply to this email.</code></p>\n\
    </td>\n\
   </tr>\n\
  </tbody>\n\
 </table>\n\
</body>\n\
</html>\n\
```

## Använda systemgenererade metadata i ett e-postmeddelande {#using-system-generated-metadata-in-an-email-notification}

Ett AEM Forms-program innehåller flera metadatavariabler (nyckelvärdepar). Du kan använda dessa variabler i en e-postmall. Variabelns värde baseras på det associerade formulärprogrammet. I följande tabell visas alla metadatavariabler som finns i kartongen:

<table>
 <tbody> 
  <tr> 
   <td>Nyckel</td> 
   <td>Beskrivning</td> 
  </tr> 
  <tr> 
   <td>workitem_title</td> 
   <td>Titel på det associerade formulärprogrammet.</td> 
  </tr> 
  <tr> 
   <td>workitem_url</td> 
   <td>URL för åtkomst till det associerade formulärprogrammet.</td> 
  </tr> 
  <tr> 
   <td>workitem_description</td> 
   <td>Beskrivning av det associerade formulärprogrammet.</td> 
  </tr> 
  <tr> 
   <td>workitem_priority</td> 
   <td>Prioritet har angetts för det associerade formulärprogrammet.</td> 
  </tr> 
  <tr> 
   <td>workitem_due_date</td> 
   <td>Senaste datum för att agera på det associerade formulärprogrammet.</td> 
  </tr> 
  <tr> 
   <td>workitem_workflow</td> 
   <td>Namn på arbetsflödet som är associerat med formulärprogrammet.</td> 
  </tr> 
  <tr> 
   <td>workitem_assign_timestamp</td> 
   <td>Datum och tid när arbetsflödesobjektet tilldelades den nuvarande tilldelade personen.</td> 
  </tr> 
  <tr> 
   <td>workitem_assigner</td> 
   <td>Den nuvarande tilldelades namn.</td> 
  </tr> 
  <tr> 
   <td>host_prefix</td> 
   <td>URL för författarservern. Till exempel https://10.41.42.66:4502<br /> </td> 
  </tr> 
  <tr> 
   <td>publish_prefix</td> 
   <td>Publiceringsserverns URL. Till exempel https://10.41.42.66:4503</td> 
  </tr> 
 </tbody> 
</table>

## Använda anpassade metadata i ett e-postmeddelande {#using-custom-metadata-in-an-email-notification}

Du kan också använda anpassade metadata i ett e-postmeddelande. Anpassade metadata innehåller information utöver systemgenererade metadata. Exempel: principinformation som hämtats från en databas. Du kan använda ett ECMAScript- eller OSGi-paket för att lägga till anpassade metadata i crx-databasen:

### Använd ECMAScript för att lägga till anpassade metadata  {#use-ecmascript-to-add-custom-metadata}

[ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) är ett skriptspråk. Det används för skript och serverprogram på klientsidan. Utför följande steg för att använda ECMAScript för att lägga till anpassade metadata för en e-postmall:

1. Logga in på CRX DE med ett administratörskonto. URL:en är https://&#39;[server]:[port]&#39;/crx/de/index.jsp

1. Gå till /apps/fd/dashboard/scripts/metadataScripts. Skapa en fil med filnamnstillägget .ecma. Exempel: usermetadata.ecma

   Om den ovannämnda sökvägen inte finns skapar du den.

1. Lägg till kod i .ecma-filen som har logik att generera anpassade metadata i nyckelvärdepar. Följande ECMAScript-kod genererar anpassade metadata för en försäkringsprofil:

   ```javascript
   function getUserMetaData()  {
       //Commented lines below provide an overview on how to set user metadata in map and return it.
       var HashMap = Packages.java.util.HashMap;
       var valuesMap = new HashMap();
       valuesMap.put("policyNumber", "2017568972695");
       valuesMap.put("policyHolder", "Adobe Systems");
   
       return valuesMap;
   }
   ```

1. Klicka på Spara alla. Skriptet kan nu väljas i AEM arbetsflödesmodell.

   ![tilldelningsmetadata](assets/assigntask-metadata.png)

1. (Valfritt) Ange skriptets titel:

   Om du inte anger titeln visas den fullständiga sökvägen till ECMAScript-filen i fältet Egna metadata. Utför följande steg för att ange en beskrivande titel för skriptet:

   1. Expandera skriptnoden, högerklicka på noden **[!UICONTROL jcr:content]** och klicka på **[!UICONTROL Mixins]**.
   1. Skriv mix:title i dialogrutan Redigera mixar och klicka på **+**.
   1. Lägg till en egenskap med följande värden.

      | Namn | jcr:title |
      |---|---|
      | Typ | Sträng |
      | Värde | Ange skriptets titel. Anpassade metadata för principhållaren. Det angivna värdet visas i tilldelningssteget. |

### Använd ett OSGi-paket och Java-gränssnitt för att lägga till anpassade metadata {#use-an-osgi-bundle-and-java-interface-to-add-custom-metadata}

Du kan använda Java-gränssnittet WorkitemUserMetadataService för att lägga till anpassade metadata för e-postmallar. Du kan skapa ett OSGi-paket som använder Java-gränssnittet WorkitemUserMetadataService och distribuera det till AEM Forms-servern. Metadata blir tillgängliga för val i steget Tilldela uppgift.

Om du vill skapa ett OSGi-paket med Java-gränssnitt lägger du till filerna [AEM Forms Client SDK](https://helpx.adobe.com/se/aem-forms/kb/aem-forms-releases.html) jar och [granite jar](https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.workflow.api/1.0.2/) som externa beroenden till OSGi-paketprojektet. Du kan använda vilken Java-utvecklingsmiljö som helst för att skapa ett OSGi-paket. I följande procedur beskrivs hur du använder Eclipse för att skapa ett OSGi-paket:

1. Öppna Eclipse IDE. Navigera till Arkiv > Nytt projekt.

1. På skärmen Välj en guide väljer du Maven Project och klickar på Next.

1. I New Maven Project, keep defaults, and click Next. Markera en arketyp och klicka på Nästa. Exempel: maven-arketype-quickstart. Ange grupp-ID, artefakt-ID, version och paket för projektet och klicka på Slutför. Projektet skapas.

1. Öppna filen pom.xml och redigera och ersätt allt innehåll i filen med följande:

1. Lägg till källkod som använder Java-gränssnittet WorkitemUserMetadataService för att lägga till anpassade metadata för e-postmallar. En exempelkod visas nedan:

   ```java
   package com.aem.impl;
   
   import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
   import org.apache.felix.scr.annotations.Component;
   import org.apache.felix.scr.annotations.Properties;
   import org.apache.felix.scr.annotations.Property;
   import org.apache.felix.scr.annotations.Service;
   import org.osgi.framework.Constants;
   
   import java.util.HashMap;
   import java.util.Map;
   
   @Component
   @Service
   @Properties({
           @Property(name = Constants.SERVICE_DESCRIPTION, value = "A sample implementation of a user metadata service."),
           @Property(name = WorkitemUserMetadataService.SERVICE_PROPERTY_LABEL, value = "Default User Metadata Service")})
   
   public class WorkitemUserMetadataServiceImpl
     implements WorkitemUserMetadataService
   {
     public WorkitemUserMetadataServiceImpl() {}
   
     public Map<String, String> getUserMetadataMap()
     {
       HashMap<String, String> metadataMap = null;
       metadataMap = new HashMap();
       metadataMap.put("test_metadata", "tested-interface implementation");
       return metadataMap;
     }
   }
   ```

1. Öppna en kommandotolk och navigera till katalogen som innehåller OSGi-paketprojektet. Använd följande kommando för att skapa OSGi-paketet:

   `mvn clean install`

1. Överför paketet till en AEM Forms-server. Du kan använda AEM Package Manager för att importera paketet till AEM Forms-servern.

När paketet har importerats kan du välja metadata i steget Tilldela uppgift och använda det som en e-postmall.
