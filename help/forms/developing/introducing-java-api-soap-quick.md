---
title: Introduktion till Java&handel; API QuickStart
description: Lär dig hur AEM Forms-åtgärder kan utföras med AEM Forms Java&trade; strongly typed API enabled with SOAP connection.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop, development-tools
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
hide: true
hidefromtoc: true
exl-id: a5ae164d-d0c0-40d0-baeb-0e646fc71f55
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 0%

---

# Introduktion till Java™ API - snabbstart {#introducing-java-api-quickstart}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Med Adobe AEM Forms API Quick Start kan du snabba upp arbetet med att utveckla program som interagerar med AEM Forms tjänster. *Snabbstart* är fullständiga program som du kan kopiera och klistra in i dina egna projekt och använda som startpunkt. Du kan köra en snabbstart för att se hur den fungerar och ändra den efter dina egna behov.

AEM Forms-åtgärder kan utföras med AEM Forms starkt typbestämda API och anslutningsläget bör anges till SOAP.

Java™-API Quick Start innehåller en lista över JAR-filer som krävs för att köra Java™-programmet. De flesta Java™ Quick Starts är konsolprogram som körs inom `main`. Men Forms Java™-snabbstart av starkt typifierad API är implementerat som en Java™-server som körs i ett webbprogram.

JAR-fillistan finns i ett kommentarsavsnitt i början av snabbstarten. Följande kommentar är till exempel i en snabbstart för utdata och är en typisk JAR-fillista som finns i varje Java™ snabbstart.

```java
 /*
     * This Java Quick Start uses the SOAP mode and contains the following JAR files
     * in the class path:
     * 1. adobe-output-client.jar
     * 2. adobe--client.jar
     * 3. adobe-usermanager-client.jar
     *
     * These JAR files are in the following path:
     * <install directory>/Adobe/Adobe_Experience_Manager_forms/SDK/client-libs/common
     *
     * The adobe-utilities.jar file is in the following path:
     * <install directory>/Adobe/Adobe_Experience_Manager_forms/SDK/client-libs/jboss
     *
     * The jboss-client.jar file is in the following path:
     * <install directory>/Adobe/Adobe_Experience_Manager_forms/jboss/bin/client
     *
     * If you want to invoke a remote AEM Forms instance and there is a
     * firewall between the client application and AEM Forms, then it is
     * recommended that you use the SOAP mode. When using the SOAP mode,
     * you have to include additional JAR files in the following
     * path
     * <install directory>/Adobe/Adobe_Experience_Manager_forms/SDK/client-libs/thirdparty
     *
     * For information about the SOAP
     * mode and the additional JAR files that need to be included,
     * see "Setting connection properties" in Programming
     * with AEM Forms
     *
     * For complete details about the location of the AEM Forms JAR files,
     * see "Including AEM Forms library files" in Programming
     * with AEM Forms
     */
```

## Snabbstart för flera tjänster {#multiple-services-quick-start}

De flesta snabbstarter i *Programmering med AEM Forms på JEE* anropar en specifik tjänst för att utföra en åtgärd. Vissa snabbstarter anropar dock flera AEM Forms-tjänster för ett visst arbetsflöde. Följande lista innehåller Java™-snabbstarter som anropar fler än en AEM Forms-tjänst:

[Snabbstart (SOAP-läge): Skickar ett dokument i AEM Forms-databasen till utdatatjänsten med Java™-API:t &#x200B;](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-passing-a-document-located-in-the-repository-to-the-output-service-using-the-java-api) (anropar tjänsten Databas och Utdata)

[Snabbstart (SOAP-läge): Skapa ett PDF-dokument baserat på fragment med Java™ API](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-creating-a-pdf-document-based-on-fragments-using-the-java-api) (anropar Assembler- och Output-tjänsten)

[Snabbstart (SOAP-läge): Skapa PDF-dokument med skickade XML-data med Java™-API:t &#x200B;](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-creating-pdf-documents-with-submitted-xml-data-using-the-java-api) (anropar Forms-, Output- och Document Management-tjänsten)

[Snabbstart (SOAP-läge): Skickar dokument till Forms-tjänsten med Java™-API:t &#x200B;](/help/forms/developing/forms-service-api-quick-starts.md#quick-start-soap-mode-passing-documents-to-the-forms-service-using-the-java-api) (anropar Forms- och Document Management-tjänsten)

[Snabbstart (SOAP-läge): Signera ett XFA-baserat formulär digitalt med Java™ API](/help/forms/developing/signature-service-java-api-quick.md#quick-start-soap-mode-digitally-signing-a-xfa-based-form-using-the-java-api) (anropar Forms- och Signature-tjänsten)

[Snabbstart (SOAP-läge): Hantera roller och behörigheter med Java™ API](/help/forms/developing/user-manager-java-api-quick.md#quick-start-soap-mode-managing-roles-and-permissions-using-the-java-api) (anropar DirectoryManager och tjänsten AuthorizationManager)

[Snabbstart (SOAP-läge): Skickar dokument till utdatatjänsten med Java™-API:t &#x200B;](/help/forms/developing/output-service-java-api-quick.md#quick-start-soap-mode-passing-documents-to-the-output-service-using-the-java-api) (anropar utdata- och dokumenthanteringstjänsten)

>[!NOTE]
>
>Quick Starts in Programming with AEM Forms är baserat på att AEM Forms körs på JBoss® Application Server och operativsystemet Microsoft® Windows®. Om du använder ett annat operativsystem, till exempel UNIX®, ska du ersätta Windows-specifika sökvägar med sökvägar som stöds av det aktuella operativsystemet. På samma sätt måste du ange giltiga anslutningsegenskaper om du använder en annan J2EE-programserver. (Se [Ange anslutningsegenskaper](/help/forms/developing/invoking-aem-forms-using-java.md#setting-connection-properties).)

>[!NOTE]
>
>De flesta snabbstarter för webbtjänster skrivs i C# och använder .NET-ramverket. Du kan dock skapa klientprogramlogik som kan anropa AEM Forms-tjänster i alla utvecklingsmiljöer som stöder SOAP-standarder. (Se [Anropa AEM Forms med webbtjänster](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-web-services).)
