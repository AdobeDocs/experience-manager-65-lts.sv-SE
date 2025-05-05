---
title: Markera en användare eller grupp dynamiskt för AEM Forms-centrerade arbetsflödessteg
description: Lär dig hur du väljer en användare eller grupp för ett AEM Forms-arbetsflöde vid körning.
content-type: troubleshooting
topic-tags: publish
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms,Document Services
exl-id: b3b3567f-df0a-4a24-849c-dcc0b745de63
source-git-commit: 79cce324382bada2e9aec107b8e494723bf490e9
workflow-type: tm+mt
source-wordcount: '853'
ht-degree: 0%

---

# Markera en användare eller grupp dynamiskt för AEM Forms-centrerade arbetsflödessteg {#dynamically-select-a-user-or-group-for-aem-forms-centric-workflow-steps}

Lär dig hur du väljer en användare eller grupp för ett AEM Forms-arbetsflöde vid körning.

I stora organisationer finns det krav på att dynamiskt välja användare för en process. Du kan till exempel välja en fältagent som ska betjäna en kund baserat på hur nära kunden är agenten. I så fall väljs agenten dynamiskt.

Tilldela uppgifter och Adobe Sign-steg för [Forms-centrerade arbetsflöden i OSGi](/help/forms/using/aem-forms-workflow.md) innehåller alternativ för att dynamiskt välja en användare. Du kan använda ECMAScript- eller OSGi-paket för att dynamiskt välja en tilldelare för steget Tilldela uppgift eller för att välja signerare för steget Signera dokument.

## Använd ECMAScript för att dynamiskt välja en användare eller grupp {#use-ecmascript-to-dynamically-select-a-user-or-group}

ECMAScript är ett skriptspråk. Det används för skript och serverprogram på klientsidan. Utför följande steg för att dynamiskt välja en användare eller grupp med ECMAScript:

1. Öppna CRXDE Lite. URL:en är `https://'[server]:[port]'/crx/de/index.jsp`
1. Skapa en fil med filtillägget .ecma i följande sökväg. Om sökvägen (nodstrukturen) inte finns skapar du den:

   * (Sökväg för steget Tilldela uppgift) `/apps/fd/dashboard/scripts/participantChooser`
   * (Sökväg för signatursteg) `/apps/fd/workflow/scripts/adobesign`

1. Lägg till ECMAScript som har logik att dynamiskt välja en användare i .ecma-filen. Klicka på **[!UICONTROL Save All]**.

   Exempel på skript finns i [Exempel på ECMAScript för dynamiskt val av användare eller grupp](/help/forms/using/dynamically-select-a-user-or-group-for-aem-workflow.md#sample-ecmascripts-to-dynamically-choose-a-user-or-a-group).

1. Lägg till skriptets visningsnamn. Det här namnet visas i arbetsflödessteg. Så här anger du namnet:

   1. Expandera skriptnoden, högerklicka på noden **[!UICONTROL jcr:content]** och klicka på **[!UICONTROL Mixins]**.
   1. Lägg till egenskapen `mix:title` i dialogrutan Redigera mixar och klicka på **OK**.
   1. Lägg till följande egenskap i jcr:content-noden i skriptet:

      | Namn | Typ | Värde |
      |--- |--- |--- |
      | jcr:title | Sträng | Ange namnet på skriptet. Välj till exempel närmaste fältagent. Det här namnet visas i Tilldela uppgift och Signera dokument. |

   1. Klicka på **Spara alla**. Skriptet blir tillgängligt för val i komponenterna i AEM Workflow.

      ![skript](assets/script.png)

### Exempel på ECMAScript för att dynamiskt välja en användare eller grupp {#sample-ecmascripts-to-dynamically-choose-a-user-or-a-group}

I följande exempel på ECMAScript väljs en tilldelad användare dynamiskt för steget Tilldela uppgift. I det här skriptet väljs en användare baserat på sökvägen till nyttolasten. Innan du använder det här skriptet måste du kontrollera att alla användare som nämns i skriptet finns i AEM. Om de användare som anges i skriptet inte finns i AEM kan den relaterade processen misslyckas.

```javascript
function getParticipant() {

var workflowData = graniteWorkItem.getWorkflowData();

if (workflowData.getPayloadType() == "JCR_PATH") { 

var path = workflowData.getPayload().toString(); 
     if (path.indexOf("/content/geometrixx/en") == 0) {
    return "user1";
    } 
   else {
              return "user2";
            }
}
}
```

I följande exempel på ECMAScript väljs dynamiskt en tilldelad för Adobe Sign-steget. Innan du använder skriptet nedan måste du se till att användarinformationen (e-postadresser och telefonnummer) som anges i skriptet är korrekt. Om användarinformationen som anges i skriptet är felaktig kan den relaterade processen misslyckas.

>[!NOTE]
>
>När du använder ECMAScript för Adobe Sign måste skriptet finnas i crx-database på /apps/fd/workflow/scripts/adobesign/, och ska ha en funktion som heter getAdobeSignRecipients för att returnera en lista över användarna.

```javascript
function getAdobeSignRecipients() {

    var recipientSetInfos = new Packages.java.util.ArrayList();

    var recipientInfoSet = new com.adobe.aem.adobesign.recipient.RecipientSetInfo();
    var recipientInfoList = new Packages.java.util.ArrayList();
    var recipientInfo = new com.adobe.aem.adobesign.recipient.RecipientInfo();

    var email;
    var recipientAuthenticationMethod = com.adobe.aem.adobesign.recipient.RecipientAuthenticationMethod.PHONE;  
    //var recipientAuthenticationMethod = com.adobe.aem.adobesign.recipient.RecipientAuthenticationMethod.NONE;
    var securityOptions = null;

    var phoneNumber = "123456789";
    var countryCode = "+1";
    var recipientPhoneInfo = new Array();
    recipientPhoneInfo.push(new com.adobe.aem.adobesign.recipient.RecipientPhoneInfo(phoneNumber, countryCode));

     securityOptions = new com.adobe.aem.adobesign.recipient.RecipientSecurityOption(recipientAuthenticationMethod, recipientPhoneInfo , null);
    
    email = "example@example.com";
    
    recipientInfo.setEmail(email);
    recipientInfo.setSecurityOptions(securityOptions);
    
    recipientInfoList.add(recipientInfo);
    recipientInfoSet.setMemberInfos(recipientInfoList);
    recipientSetInfos.add(recipientInfoSet);

    return recipientSetInfos;

}
```

## Använd Java-gränssnittet för att dynamiskt välja en användare eller grupp {#use-java-interface-to-dynamically-choose-a-user-or-group}

Du kan använda Java-gränssnittet [RecipientInfoSpecifier](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/workflow/adobesign/api/RecipientInfoSpecifier.html) för att dynamiskt välja en användare eller en grupp för steg i Adobe Sign och Tilldela uppgift. Du kan skapa ett OSGi-paket som använder Java-gränssnittet [RecipientInfoSpecifier](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/workflow/adobesign/api/RecipientInfoSpecifier.html) och distribuera det till AEM Forms-servern. Det gör alternativet tillgängligt för val i komponenterna Tilldela uppgift och Adobe Sign i AEM Workflow.

Du behöver filer för [AEM Forms Client SDK](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE) jar och [granite jar](https://repo1.maven.org/maven2/com/adobe/granite/com.adobe.granite.workflow.api/1.0.2/) för att kompilera kodexemplet som anges nedan. Lägg till dessa jar-filer som externa beroenden i OSGi-paketprojektet. Du kan använda vilken Java-utvecklingsmiljö som helst för att skapa ett OSGi-paket. I följande procedur beskrivs hur du använder Eclipse för att skapa ett OSGi-paket:

1. Öppna Eclipse IDE. Navigera till **[!UICONTROL File]**> **[!UICONTROL New Project]**.
1. På skärmen Välj en guide väljer du **[!UICONTROL Maven Project]** och klickar på **[!UICONTROL Next]**.
1. I New Maven-projektet behåller du standardinställningarna och klickar på **[!UICONTROL Next]**. Välj en arketyp och klicka på **[!UICONTROL Next]**. Exempel: maven-arketype-quickstart. Ange **[!UICONTROL Group Id]**, **[!UICONTROL Artifact Id]**, **[!UICONTROL version]** och **[!UICONTROL package]** för projektet och klicka på **[!UICONTROL Finish]**. Projektet skapas.
1. Öppna filen pom.xml och redigera och ersätt allt innehåll i filen med följande:

   ```xml
   <project xmlns="https://maven.apache.org/POM/4.0.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="https://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>getAgent</groupId>
       <artifactId>assignToAgent</artifactId>
       <version>1.0</version>
       <packaging>bundle</packaging><!-- packaging type bundle is must -->
   
       <name>assignToAgent</name>
       <url>https://maven.apache.org</url>
       <repositories>
           <repository>
               <id>adobe</id>
               <name>Adobe Public Repository</name>
               <url>https://repo1.maven.org/maven2/com/adobe/</url>
               <layout>default</layout>
           </repository>
       </repositories>
       <pluginRepositories>
           <pluginRepository>
               <id>adobe</id>
               <name>Adobe Public Repository</name>
               <url>https://repo1.maven.org/maven2/com/adobe/</url>
               <layout>default</layout>
           </pluginRepository>
       </pluginRepositories>
   
       <dependencies>
           <dependency>
               <groupId>com.adobe.aemfd</groupId>
               <artifactId>aemfd-client-sdk</artifactId>
               <version>6.0.138</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.granite</groupId>
               <artifactId>com.adobe.granite.workflow.api</artifactId>
               <version>1.0.0</version>
           </dependency>
   
           <dependency>
               <groupId>org.osgi</groupId>
               <artifactId>org.osgi.core</artifactId>
               <version>4.2.0</version>
               <scope>provided</scope>
           </dependency>
   
           <dependency>
               <groupId>org.apache.felix</groupId>
               <artifactId>org.apache.felix.scr.annotations</artifactId>
               <version>1.7.0</version>
   
           </dependency>
   
           <dependency>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.api</artifactId>
               <version>2.2.0</version>
   
           </dependency>
   
       </dependencies>
   
       <!-- ====================================================================== -->
       <!-- B U I L D D E F I N I T I O N -->
       <!-- ====================================================================== -->
       <build>
           <plugins>
   
               <plugin>
                   <groupId>org.apache.felix</groupId>
                   <artifactId>maven-bundle-plugin</artifactId>
                   <extensions>true</extensions>
                   <configuration>
                       <instructions>
                           <Bundle-SymbolicName>com.aem.assigntoAgent-bundle</Bundle-SymbolicName>
                       </instructions>
                   </configuration>
               </plugin>
   
               <plugin>
                   <groupId>org.apache.felix</groupId>
                   <artifactId>maven-scr-plugin</artifactId>
                   <version>1.9.0</version>
                   <executions>
                       <execution>
                           <id>generate-scr-descriptor</id>
                           <goals>
                               <goal>scr</goal>
                           </goals>
                       </execution>
                   </executions>
               </plugin>
           </plugins>
       </build>
   
   </project>
   ```

1. Lägg till källkod som använder Java-gränssnittet [RecipientInfoSpecifier](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/workflow/adobesign/api/RecipientInfoSpecifier.html) för att dynamiskt välja en användare eller en grupp för tilldelningssteget. Exempelkod finns i [Exempel på dynamiskt val av användare eller grupp med Java-gränssnittet](#-sample-scripts-for).
1. Öppna en kommandotolk och navigera till katalogen som innehåller OSGi-paketprojektet. Använd följande kommando för att skapa OSGi-paketet:

   `mvn clean install`

1. Överför paketet till en AEM Forms-server. Du kan använda AEM Package Manager för att importera paketet till AEM Forms-servern.

När paketet har importerats blir alternativet att välja Java-gränssnittet för att dynamiskt välja en användare eller grupp tillgängligt i steg för Adobe Sign och Tilldela uppgift.

### Exempelkod Java för att dynamiskt välja en användare eller grupp {#sample-java-code-to-dynamically-choose-a-user-or-a-group}

Följande exempelkod väljer dynamiskt en tilldelad för Adobe Sign-steget. Du använder koden i ett OSGi-paket. Innan du använder koden nedan måste du se till att den användarinformation (e-postadresser och telefonnummer) som anges i koden är korrekt. Om användarinformationen som anges i koden är felaktig kan den relaterade processen misslyckas.

```java
/*************************************************************************

 *
 * ADOBE CONFIDENTIAL
 * __________________
 *
 * Copyright 2016 Adobe Systems Incorporated
 * All Rights Reserved.
 *
 * NOTICE:  All information contained herein is, and remains
 * the property of Adobe Systems Incorporated and its suppliers,
 * if any.  The intellectual and technical concepts contained
 * herein are proprietary to Adobe Systems Incorporated and its
 * suppliers and are protected by trade secret or copyright law.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe Systems Incorporated.
 **************************************************************************/
 
package com.aem.impl;

import java.util.ArrayList;
import java.util.List;

import com.adobe.aem.adobesign.recipient.RecipientAuthenticationMethod;
import com.adobe.aem.adobesign.recipient.RecipientInfo;
import com.adobe.aem.adobesign.recipient.RecipientPhoneInfo;
import com.adobe.aem.adobesign.recipient.RecipientSecurityOption;
import com.adobe.aem.adobesign.recipient.RecipientSetInfo;
import com.adobe.fd.workflow.adobesign.api.RecipientInfoSpecifier;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;

/**
 * <code>DummyRecipientInfoSpecifier implementation. A sample code to write implementation of RecipientInfoSpecifier to choose recipients/code>...
 */
@Service

@Component(metatype = false)
public class DummyRecipientChoser implements RecipientInfoSpecifier {
    public List<RecipientSetInfo> getAdobeSignRecipients(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {

        List<RecipientSetInfo> recipientSetInfos = new ArrayList<RecipientSetInfo>();

                //First Recipient

                RecipientSetInfo recipientInfoSet1 = new RecipientSetInfo();
                List<RecipientInfo> recipientInfoList = new ArrayList<RecipientInfo>();
                RecipientInfo recipientInfo1 = new RecipientInfo();//Member to first recipient

                String email;

                RecipientAuthenticationMethod recipientAuthenticationMethod = RecipientAuthenticationMethod.WEB_IDENTITY;
                RecipientSecurityOption securityOptions = null;

                String phoneNumber = "123456789";
                String countryCode = "+1";
                RecipientPhoneInfo[] recipientPhoneInfo = new RecipientPhoneInfo[1];  //if multiple phone numbers, size>1
                recipientPhoneInfo[0] = new RecipientPhoneInfo(phoneNumber, countryCode);
                securityOptions = new RecipientSecurityOption(recipientAuthenticationMethod, recipientPhoneInfo , null);
                 
                email = "example@example.com";

                recipientInfo1.setEmail(email);
                recipientInfo1.setSecurityOptions(securityOptions);

                recipientInfoList.add(recipientInfo1);  //Add member

                recipientInfoSet1.setMemberInfos(recipientInfoList);

                //Second Recipient

                RecipientSetInfo recipientInfoSet2 = new RecipientSetInfo();
                List<RecipientInfo> recipientInfoList2 = new ArrayList<RecipientInfo>();

                recipientAuthenticationMethod = RecipientAuthenticationMethod.PHONE;
                securityOptions = null;
                 
                phoneNumber = "987654321";//"0123456789";

                countryCode = "+1";
                RecipientPhoneInfo[] recipientPhoneInfo_1 = new RecipientPhoneInfo[1];
                recipientPhoneInfo_1[0] = new RecipientPhoneInfo(phoneNumber, countryCode);
                securityOptions = new RecipientSecurityOption(recipientAuthenticationMethod, recipientPhoneInfo_1 , null);
                 
                email = "example2@example.com";//"dummymail2@domain.com";

                RecipientInfo recipientInfo2  = new RecipientInfo();
                recipientInfo2.setEmail(email);
                recipientInfo2.setSecurityOptions(securityOptions);

                recipientInfoList2.add(recipientInfo2);  //Add member

                recipientInfoSet2.setMemberInfos(recipientInfoList2);

                //*********************************

                recipientSetInfos.add(recipientInfoSet1); 
                recipientSetInfos.add(recipientInfoSet2);

        return recipientSetInfos;

    }

}
```
