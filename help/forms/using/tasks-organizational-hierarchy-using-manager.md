---
title: Hantera uppgifter i en organisationshierarki med hjälp av hanterarvyn
description: Hur chefer och organisationschefer kan komma åt och arbeta med uppgifter i sina direkta och indirekta rapporter på fliken Att göra på arbetsytan i AEM Forms.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: 2fcb0241-8018-4bdd-b89d-44b8fc063ff3
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# Hantera uppgifter i en organisationshierarki med hjälp av hanterarvyn{#managing-tasks-in-an-organizational-hierarchy-using-manager-view}

På AEM Forms arbetsyta kan chefer nu komma åt uppgifter som tilldelats alla i deras hierarki - direkta eller indirekta rapporter - och utföra olika åtgärder på dem. Uppgifterna är tillgängliga på fliken Att göra på arbetsytan i AEM Forms. De åtgärder som stöds för de direkta rapporterna är:

**Vidarebefordra** - Vidarebefordra en aktivitet från direkt rapport till valfri användare.

**Anspråk** - Gör anspråk på en aktivitet i en direkt rapport.

**Anspråk och öppna** - Gör anspråk på en aktivitet i en direkt rapport och öppna den automatiskt i listan Att göra i hanteraren.

**Avvisa** - Avvisa en aktivitet som vidarebefordrats till en direkt rapport av en annan användare. Det här alternativet är tillgängligt för uppgifter som vidarebefordras av andra användare till en direkt rapport.

AEM Forms begränsar en användares åtkomst till endast de uppgifter som användaren har åtkomstkontroll för. En sådan kontroll ser till att en användare bara kan hämta de uppgifter som användaren har åtkomstbehörighet för. Med hjälp av webbtjänster och implementeringar från tredje part för att definiera hierarkin kan en organisation anpassa definitionen av chef och dirigera rapporter efter deras behov.

1. Skapa en DSC. Mer information finns i avsnittet&quot;Utveckla komponenter för AEM Forms&quot; i guiden [Programmering med AEM Forms](https://www.adobe.com/go/learn_aemforms_programming_63).
1. I DSC definierar du en ny SPI för hierarkihantering för att definiera direkta rapporter och hierarkier inom AEM Forms-användarna. Här följer ett exempel på Java™-kodfragment.

   ```java
   public class MyHierarchyMgmtService
   {
        /*
       Input : Principal Oid for a livecycle user
       Output : Returns true when the user is either the service invoker OR his direct/indirect report.
       */
       boolean isInHierarchy(String principalOid) {
   
       }
   
       /*
       Input : Principal Oid for a livecycle user
       Output : List of principal Oids for direct reports of the livecycle user
       A user may get direct reports only for himself OR his direct/indirect reports.
       So the API is functionally equivalent to -
       isInHierarchy(principalOid) ? <return direct reports> : <return empty list>
       */
       List<String> getDirectReports(String principalOid) {
   
       }
   
       /*
       Returns whether a livecycle user has direct reports or not.
       It is functionally equivalent to -
       getDirectReports(principalOid).size()>0
       */
       boolean isManager(String principalOid) {
   
       }
   }
   ```

1. Skapa en component.xml-fil. Kontrollera att spec-id är samma som i kodutdraget nedan. Nedan följer ett exempel på ett kodfragment som du kan återanvända.

   ```xml
   <component xmlns="https://adobe.com/idp/dsc/component/document">
       <component-id>com.adobe.sample.SampleDSC</component-id>
       <version>1.1</version>
       <supports-export>false</supports-export>
         <descriptor-class>com.adobe.idp.dsc.component.impl.DefaultPOJODescriptorImpl</descriptor-class>
         <services>
           <service name="MyHierarchyMgmtService" title="My hierarchy management service" orchestrateable="false">
           <auto-deploy service-id="MyHierarchyMgmtService" category-id="Sample DSC" major-version="1" minor-version="0" />
           <description>Service for resolving hierarchy management.</description>
            <specifications>
            <specification spec-id="com.adobe.idp.taskmanager.dsc.enterprise.HierarchyManagementProvider"/>
            </specifications>
           <specification-version>1.0</specification-version>
           <implementation-class>com.adobe.sample.hierarchymanagement.MyHierarchyMgmtService</implementation-class>
           <request-processing-strategy>single_instance</request-processing-strategy>
           <supported-connectors>default</supported-connectors>
           <operation-config>
               <operation-name>*</operation-name>
               <transaction-type>Container</transaction-type>
               <transaction-propagation>supports</transaction-propagation>
               <!--transaction-timeout>3000</transaction-timeout-->
           </operation-config>
           <operations>
               <operation anonymous-access="true" name="isInHierarchy" method="isInHierarchy">
                   <input-parameter name="principalOid" type="java.lang.String" />
                   <output-parameter name="result" type="java.lang.Boolean"/>
               </operation>
               <operation anonymous-access="true" name="getDirectReports" method="getDirectReports">
                   <input-parameter name="principalOid" type="java.lang.String" />
                   <output-parameter name="result" type="java.util.List"/>
               </operation>
               <operation anonymous-access="true" name="isManager" method="isManager">
                   <input-parameter name="principalOid" type="java.lang.String" />
                   <output-parameter name="result" type="java.lang.Boolean"/>
               </operation>
               </operations>
               </service>
         </services>
   </component>
   ```

1. Distribuera DSC via Workbench. Starta om tjänsten `ProcessManagementTeamTasksService`.
1. Du kan behöva uppdatera webbläsaren eller logga ut/logga in med användaren igen.

Följande skärm visar hur du får åtkomst till uppgifter i direkta rapporter och tillgängliga åtgärder.

![cu_manager_view](assets/cu_manager_view.png)

Få tillgång till uppgifter som hör till underställda och agera på dessa
