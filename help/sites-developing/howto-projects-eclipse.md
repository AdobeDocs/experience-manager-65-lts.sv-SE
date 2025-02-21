---
title: Utveckla AEM-projekt med Eclipse
description: I den här guiden beskrivs hur du använder Eclipse för att utveckla AEM-baserade projekt
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 1%

---

# Utveckla AEM-projekt med Eclipse{#how-to-develop-aem-projects-using-eclipse}

I den här guiden beskrivs hur du använder Eclipse för att utveckla AEM-baserade projekt.

>[!NOTE]
>
>Adobe tillhandahåller nu [AEM Development Tools för Eclipse](/help/sites-developing/aem-eclipse.md) som hjälper dig att utveckla AEM-lösningar med Eclipse.

## Ökning {#overview}

För att komma igång med AEM-utveckling i Eclipse krävs följande steg.

Var och en av dem förklaras mer ingående i resten av denna handledning.

* Installera Eclipse 4.3 (Kepler)
* Konfigurera ditt AEM-projekt baserat på Maven
* Förbered JSP-stöd för Eclipse i Maven POM
* Importera Maven Project till Eclipse

>[!NOTE]
>
>Den här handboken är baserad på Eclipse 4.3 (Kepler) och AEM 5.6.1.

## Installera Eclipse {#install-eclipse}

Hämta Eclipse IDE för Java EE-utvecklare från [Eclipse-nedladdningssidan](https://www.eclipse.org/downloads/).

Installera Eclipse enligt [installationsanvisningarna](https://wiki.eclipse.org/Eclipse/Installation).

## Konfigurera ditt AEM-projekt baserat på Maven {#set-up-your-aem-project-based-on-maven}

Konfigurera sedan projektet med Maven enligt beskrivningen i [Så här skapar du AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md).

## Förbered JSP-stöd för Eclipse {#prepare-jsp-support-for-eclipse}

Eclipse kan även ge stöd vid arbete med JSP, till exempel

* automatisk komplettering av taggbibliotek
* Eclipse-medvetenhet om objekt som definieras av &lt;cq:defineObjects /> och &lt;sling:defineObjects />

Så fungerar det:

1. Följ instruktionerna på [Så här arbetar du med JSP:er](/help/sites-developing/ht-projects-maven.md#how-to-work-with-jsps) i [Så här skapar du AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md).
1. Lägg till följande i avsnittet &lt;build /> i innehållsmodulens POM.

   Eclipse&#39;s Maven support plugin, m2e, ger inte stöd för maven-jspc-plugin, och den här konfigurationen anger för m2e att ignorera plugin-programmet och den relaterade uppgiften att rensa upp de tillfälliga kompileringsresultaten.

   Detta är inget problem: som beskrivs i [Så här fungerar JSP:er](/help/sites-developing/ht-projects-maven.md#how-to-work-with-jsps) används plugin-programmet maven-jspc-plugin i den här konfigurationen bara för att validera att JSP:er kompileras som en del av byggprocessen. Eclipse rapporterar redan om JSP-problem och förlitar sig inte på denna Maven-plugin för att kunna göra det.

   **myproject/content/pom.xml**

   ```xml
   <build>
     <!-- ... -->
     <pluginManagement>
       <plugins>
         <!--This plugin's configuration is used to store Eclipse m2e settings only. It has no influence on the Maven build itself.-->
         <plugin>
           <groupId>org.eclipse.m2e</groupId>
           <artifactId>lifecycle-mapping</artifactId>
           <version>1.0.0</version>
           <configuration>
             <lifecycleMappingMetadata>
               <pluginExecutions>
                 <pluginExecution>
                   <pluginExecutionFilter>
                     <groupId>org.apache.sling</groupId>
                     <artifactId>maven-jspc-plugin</artifactId>
                     <versionRange>[2.0.6,)</versionRange>
                     <goals>
                       <goal>jspc</goal>
                     </goals>
                   </pluginExecutionFilter>
                   <action>
                     <ignore/>
                   </action>
                 </pluginExecution>
                 <pluginExecution>
                   <pluginExecutionFilter>
                     <groupId>org.apache.maven.plugins</groupId>
                     <artifactId>maven-clean-plugin</artifactId>
                     <versionRange>[2.4.1,)</versionRange>
                     <goals>
                       <goal>clean</goal>
                     </goals>
                   </pluginExecutionFilter>
                   <action>
                     <ignore/>
                   </action>
                 </pluginExecution>
               </pluginExecutions>
             </lifecycleMappingMetadata>
           </configuration>
         </plugin>
       </plugins>
     </pluginManagement>
   </build>
   ```

### Importera Maven Project till Eclipse {#import-the-maven-project-into-eclipse}

1. I Eclipse väljer du Arkiv > Importera...
1. I dialogrutan Importera väljer du Maven > Befintliga Maven-projekt och klickar sedan på Nästa.

   ![chlimage_1-41](assets/chlimage_1-41a.png)

1. Ange sökvägen till projektets mapp på den översta nivån och klicka sedan på Markera alla och Slutför.

   ![chlimage_1-42](assets/chlimage_1-42a.png)

1. Nu kan du börja använda Eclipse för att utveckla ditt AEM-projekt, inklusive JSP autocomplete.

   ![chlimage_1-43](assets/chlimage_1-43a.png)

   >[!NOTE]
   >
   >Om du inkluderar `/libs/foundation/global.jsp` eller andra JSP:er i `/libs` måste du kopiera den till ditt projekt så att Eclipse kan lösa inkluderingen. Samtidigt måste ni se till att den inte paketeras i ert innehållspaket av Maven. Hur du uppnår detta beskrivs i [Så här skapar du AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md).
