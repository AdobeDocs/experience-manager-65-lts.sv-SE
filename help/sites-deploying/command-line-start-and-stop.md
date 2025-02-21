---
title: Kommandoradens start och stopp
description: Lär dig hur du startar och stoppar Adobe Experience Manager från kommandoraden.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# Kommandoradens start och stopp{#command-line-start-and-stop}

## Starta Adobe Experience Manager från kommandoraden {#starting-adobe-experience-manager-from-the-command-line}

Skriptet `start` är tillgängligt i katalogen *&lt;cq-installation>/bin*. Det finns både UNIX®- och Windows-versioner. Skriptet startar instansen som är installerad i katalogen *&lt;cq-installation>*.

Dessa två versioner har stöd för en lista med miljövariabler som kan användas för att starta och justera Adobe Experience Manager-instansen (AEM).

<table>
 <tbody>
  <tr>
   <td><strong>Miljövariabel </strong></td>
   <td><strong>Beskrivning </strong></td>
  </tr>
  <tr>
   <td>CQ_PORT</td>
   <td>TCP-port som används för stopp- och statusskript <br /> </td>
  </tr>
  <tr>
   <td>CQ_HOST</td>
   <td>Värdnamn <br /> </td>
  </tr>
  <tr>
   <td>CQ_INTERFACE</td>
   <td>Gränssnitt som servern ska lyssna på <br /> </td>
  </tr>
  <tr>
   <td>CQ_RUNMODE</td>
   <td>Körningslägen avgränsade med kommatecken <br /> </td>
  </tr>
  <tr>
   <td>CQ_JARFILE</td>
   <td>Namn på jarfile<br /> </td>
  </tr>
  <tr>
   <td>CQ_USE_JAAS</td>
   <td>Användning av JAAS (om true)<br /> </td>
  </tr>
  <tr>
   <td>CQ_JAAS_CONFIG</td>
   <td>Sökväg till JAAS-konfigurationen <br /> </td>
  </tr>
  <tr>
   <td>CQ_JVM_OPTS</td>
   <td>Standardalternativ för JVM <br /> </td>
  </tr>
 </tbody>
</table>

>[!CAUTION]
>
>Vissa körningslägen, bland annat författare och publicering, måste anges innan AEM startas första gången och kan inte ändras efteråt. Innan du konfigurerar en AEM-instans som används i produktionen bör du läsa [dokumentationen för körningslägen](/help/sites-deploying/configure-runmodes.md) för mer information.

### Exempel på Windows-plattformen start.bat-skript {#windows-platform-start-bat-script-example}

```shell
SET CQ_PORT=1234 & ./start.bat
```

### Exempel på startskript för UNIX®-plattformen {#unix-platform-start-script-example}

```shell
CQ_PORT=1234 ./start
```

>[!NOTE]
>
>Startskriptet startar AEM Quickstart som installeras i mappen *&lt;cq-installation>/app*.

## Stoppar Adobe Experience Manager {#stopping-adobe-experience-manager}

Om du vill stoppa AEM gör du något av följande:

* Beroende på vilken plattform du använder:

   * Om du har startat AEM från ett skript eller kommandoraden trycker du på **Ctrl+C** för att stänga servern.
   * Om du har använt startskriptet på UNIX® måste du använda stoppskriptet för att stoppa AEM.

* Om du startade AEM genom att dubbelklicka på burkfilen klickar du på knappen **På** i startfönstret (knappen ändras sedan till **Av**) för att stänga servern.

  ![chlimage_1-63](assets/chlimage_1-63.png)

## Stoppa Adobe Experience Manager från kommandoraden {#stopping-adobe-experience-manager-from-the-command-line}

Skriptet `stop` är tillgängligt i katalogen *&lt;cq-installation>/bin*. Det finns både UNIX®- och Windows-versioner. Skriptet stoppar den instans som körs och som är installerad i katalogen *&lt;cq-installation>*.

### Exempel på stoppskript för UNIX®-plattformen {#unix-platform-stop-script-example}

```shell
./stop
```

### Exempel på stop.bat-skript för Windows-plattformen {#windows-platform-stop-bat-script-example}

```shell
./stop.bat
```

Om du bara vill förkonfigurera databasen (utan att flytta den) behöver du bara:

* Extrahera `repository.xml` till önskad plats

* uppdatera `repository.xml` efter behov

* skapa `bootstrap.properties` och definiera `repository.config`

Återigen, innan du startar den faktiska installationen.
