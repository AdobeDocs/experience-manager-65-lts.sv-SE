---
title: Ytterligare steg för att få e-post med bilagor
description: Lär dig hur du åtgärdar felet när du inte kan hämta e-post med bilagor för AEM Forms på JEE-plattformar.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Det går inte att hämta e-post med bilagor för AEM Forms på JEE-plattformar{#unable-to-get-email-with-attachments}

Problemet gäller följande version:

* Experience Manager 6.5 Forms

## Problem {#issue}

Användaren kan inte utföra åtgärder som Skicka PDF via e-post eller Inkludera bilagor med inskickningskonfiguration.

## Lösning {#solution}

1. Ladda ned jar som [java.mail-1.0.jar](/help/forms/using/java.mail-1.0.jar) och zippa upp den nedladdade jar-filen för att få fram manifestfilen.

1. Använd manifestfilen för `java.mail-1.0.jar` som hämtats från steg 1 för att skapa en anpassad jar-fil, till exempel `java.mail-1.5.jar`.

1. Öppna manifestfilen och ersätt alla förekomster av `1.5.0` med `1.5.6` och `Bundle-Version: 1.0` med `Bundle-Version:1.5`

1. Skapa en anpassad jar-fil (`java.mail-1.5.jar`) med följande kommando i mappen `C:\Adobe\Adobe_Experience_Manager_Forms\java\jdk\bin` som:
   `jar -cfm java.mail-1.5.jar manifest.mf`

   I ovanstående kommando är *manifest.mf* namnet på manifestfilen och *java.mail-1.5.jar* namnet på filen som skulle skapas när ovanstående kommando kördes.

1. Hämta [javax.mail-1.5.6.redhat-1.jar](https://mvnrepository.com/artifact/com.sun.mail/javax.mail/1.5.6.redhat-1).

1. Navigera till `http://<server name>:<port>/lc/system/console/bundles` och ta bort paketet med namnet `JavaMail API (com.sun.mail.javax.mail) version 1.6.2`.

1. Installera `java.mail-1.5.jar` från steg 3. Det här steget startar om slingegenskaperna för JEE-distributionen. Vänta tills de installerade paketen på `http://<server name>:<port>/lc/system/console/bundles` visar status som **Aktiv**.

   >Om statusen fortfarande är **InActive** startar du om   **JBoss®** från **tjänstkonsolen** .


1. Installera filen `javax.mail-1.5.6.redhat-1.jar` som hämtats med steg 5.

1. Stoppa **JBoss®** från **tjänstkonsolen** och lägg till följande egenskaper i filen **Sling.properties**:
   * `org.osgi.framework.system.packages.extra=javax.activation; version\=1.2.0`
   * `sling.bootdelegation.activation=javax.activation.*`

1. Starta om **JBoss®**.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.
