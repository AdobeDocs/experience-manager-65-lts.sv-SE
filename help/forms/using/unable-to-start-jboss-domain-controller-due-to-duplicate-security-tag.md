---
title: Det går inte att starta JBoss-domänkontrollanten
description: I AEM Forms 6.5.1 LTS-klusterdistributioner med JBoss EAP 8 kan konfigurationsfilen innehålla en dubbletttagg.
solution: Experience Manager
feature: Deploying
role: User,Admin,Developer
hide: true
index: false
hidefromtoc: true
source-git-commit: 5d020671efaa4527a5f6dbb4b779c7a3351888a4
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Det går inte att starta JBoss-domänkontrollanten

## Problem

I **AEM Forms 6.5.1 LTS** klusterdistributioner med **JBoss EAP 8**, konfigurationsfilen
`<JBOSS_HOME>/domain/configuration/domain_oracle.xml` (och databasspecifika varianter) kan innehålla en **duplicerad starttagg `<security>`** .

Detta orsakar en **ogiltig XML-konfiguration**, vilket resulterar i ett **JBoss-domänkontrollantstartfel** och förhindrar att klustret initieras.

## Gäller för

* **Produkt:** AEM Forms 6.5.1 LTS
* **Distributionstyp:** kluster
* **Programserver:** JBoss EAP 8.x
* **Konfigurationsfiler:**

   * `<JBOSS_HOME>/domain/configuration/domain_oracle.xml`
   * `<JBOSS_HOME>/domain/configuration/domain_mysql.xml`
   * `<JBOSS_HOME>/domain/configuration/domain_mssql.xml`

## Felsökningssteg

1. Följande fel kan uppstå när domänkontrollanten startas:

   * `WFLYCTL0198: Unexpected element 'security'`
   * `IJ010061: Unexpected element: security`

2. Öppna den relevanta konfigurationsfilen:

   ```
   <JBOSS_HOME>/domain/configuration/domain_oracle.xml
   (or domain_mysql.xml / domain_mssql.xml)
   ```

3. Leta reda på den duplicerade öppningstaggen `<security>`.

   **Felaktig konfiguration:**

   ```xml
   <security>
       <security>
           <user-name>adobe</user-name>
           <credential-reference store="db-creds" alias="EncryptDBPassword"/>
       </security>
   ```

4. Ta bort den extra öppningstaggen `<security>` så att konfigurationen korrigeras enligt nedan:

   **Korrekt konfiguration:**

   ```xml
   <security>
       <user-name>adobe</user-name>
       <credential-reference store="db-creds" alias="EncryptDBPassword"/>
   </security>
   ```

5. Spara filen och starta JBoss-domänkontrollanten.

6. Se till att samma validerade konfiguration används på samma sätt i alla klusternoder.
