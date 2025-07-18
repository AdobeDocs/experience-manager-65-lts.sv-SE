---
title: Driftsättning och underhåll
description: Lär dig hur du kommer igång med AEM-installationen.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
exl-id: 4a2ada26-b859-4a32-9ab0-2d4c2b695245
source-git-commit: 929a2175449a371ecf81226fedb98a0c5c6d7166
workflow-type: tm+mt
source-wordcount: '1363'
ht-degree: 0%

---

# Driftsättning och underhåll{#deploying-and-maintaining}

På den här sidan hittar du:

* [Grundläggande begrepp](#basic-concepts)

   * [Vad är AEM?](#what-is-aem)
   * [Vanliga distributioner](#typical-deployment-scenarios)

      * [Lokalt](#on-premise)
      * [Managed Services med Cloud Manager](#managed-services-using-cloud-manager)

* [Komma igång](#getting-started)

   * [Förutsättningar](#prerequisites)
   * [Hämta programvaran](#getting-the-software)
   * [Lokal standardinstallation](#default-local-install)
   * [Skapa och publicera installationer](#author-and-publish-installs)
   * [Opackad installationskatalog](#unpacked-install-directory)
   * [Starta och stoppa](#starting-and-stopping)

När du har lärt dig grunderna hittar du mer avancerad och detaljerad information på följande undersidor:

* [Tekniska krav](/help/sites-deploying/technical-requirements.md)
* [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md)
* [Anpassad fristående installation](/help/sites-deploying/custom-standalone-install.md)
* [Installation av programserver](/help/sites-deploying/application-server-install.md)
* [Kommandoradens start och stopp](/help/sites-deploying/command-line-start-and-stop.md)
* [Konfigurerar](/help/sites-deploying/configuring.md)
* [Uppgradera till AEM 6.5 LTS](/help/sites-deploying/upgrade.md)
* [Instruktionsartiklar för konfiguration](/help/sites-deploying/ht-deploy.md)
* [Webbkonsol](/help/sites-deploying/web-console.md)
* [Felsökning av replikering](/help/sites-deploying/troubleshoot-rep.md)
* [Bästa praxis](/help/sites-deploying/best-practices.md)
* [Introduktion till AEM Platform](/help/sites-deploying/platform.md)

## Grundläggande begrepp {#basic-concepts}

### Vad är AEM? {#what-is-aem}

Adobe Experience Manager är ett webbaserat klient-server-system för att bygga, hantera och driftsätta kommersiella webbplatser och tillhörande tjänster. Den kombinerar flera funktioner på infrastruktur- och applikationsnivå i ett enda integrerat paket.

På infrastrukturnivå tillhandahåller AEM följande:

* **Webbprogramserver**: AEM kan distribueras i fristående läge (det innehåller en integrerad Jetty-webbserver) eller som ett webbprogram i en tredjepartsprogramserver.
* **Web Application Framework**: AEM innehåller Sling Web Application Framework som förenklar skrivandet av RESTful, innehållsorienterade webbprogram.
* **Innehållsdatabas**: AEM innehåller en Java™ Content Repository (JCR), en typ av hierarkisk databas som utformats speciellt för ostrukturerade och halvstrukturerade data. I databasen lagras inte bara användarriktat innehåll utan även all kod, mallar och interna data som används av programmet.

Baserat på detta har AEM även flera funktioner på applikationsnivå för att hantera:

* **Webbplatser**
* **Digitala publikationer**
* **Forms &amp; Documents**
* **Digital Assets**

Slutligen kan kunderna använda dessa byggstenar på infrastruktur- och applikationsnivå för att skapa anpassade lösningar genom att bygga egna applikationer.

AEM-servern är **Java-baserad** och körs på de flesta operativsystem som stöder den plattformen. All klientinteraktion med AEM sker via en **webbläsare**.

>[!NOTE]
>
>Funktionen Adaptive Forms, som finns i AEM 6.5 LTS QuickStart, är endast avsedd för prospekterings- och utvärderingsändamål. För produktion krävs en giltig licens för AEM Forms, eftersom Adaptive Forms-funktionaliteten kräver rätt licensiering.

### Vanliga distributionsscenarier {#typical-deployment-scenarios}

I AEM-terminologi är &quot;instance&quot; en kopia av AEM som körs på en server. AEM-installationer omfattar vanligtvis minst två instanser, som vanligtvis körs på olika datorer:

* **Författare**: En AEM-instans användes för att skapa, överföra och redigera innehåll och för att administrera webbplatsen. När innehållet är klart att publiceras replikeras det till publiceringsinstansen.
* **Publicera**: En AEM-instans som skickar det publicerade innehållet till allmänheten.

De här instanserna är identiska vad gäller installerad programvara. De skiljer sig bara åt genom konfiguration. Dessutom använder de flesta installationer en Dispatcher:

* **Dispatcher**: En statisk webbserver (Apache httpd, Microsoft® IIS o.s.v.) utökad med modulen AEM Dispatcher. Den cachelagrar webbsidor som skapats av publiceringsinstansen för att förbättra prestandan.

Det finns många avancerade alternativ och lösningar, men det grundläggande mönstret för författare, publicering och Dispatcher är kärnan i de flesta installationer. Vi börjar med att fokusera på en enkel konfiguration. Här följer diskussioner om avancerade driftsättningsalternativ.

I följande avsnitt beskrivs båda scenarierna:

* **Lokal**: AEM har distribuerats och hanterats i din företagsmiljö.

* **Managed Services - Cloud Manager för Adobe Experience Manager**: AEM distribueras och hanteras av Adobe Managed Services.

### Lokalt {#on-premise}

Du kan installera AEM på servrar i din företagsmiljö. Typiska installationsinstanser är: Utvecklings-, testnings- och publiceringsmiljöer. Se [Komma igång](#getting-started) för grundläggande information om hur du får AEM-programmet att installera det lokalt.

<!-- To learn more about the typical on-premises deployments, see [Recommended Deployments](/help/sites-deploying/recommended-deploys.md). -->

### Managed Services med Cloud Manager {#managed-services-using-cloud-manager}

<i>Anmäl dig snart.</i>

## Komma igång {#getting-started}

### Förutsättningar {#prerequisites}

Medan produktionsinstanser körs på dedikerade datorer som kör ett operativsystem som stöds officiellt (se [Tekniska krav](/help/sites-deploying/technical-requirements.md)), kommer Experience Manager-servern att köras på alla system som stöder [**Java™ Standard Edition 17**](https://www.oracle.com/java/technologies/downloads/#java17).

För att kunna bli bekant och utveckla i AEM är det vanligt att använda en instans som är installerad på din dator och som kör Apple OS X eller skrivbordsversioner av Microsoft® Windows eller Linux®.

På klientsidan fungerar AEM med alla moderna webbläsare (**Microsoft® Edge**, **Chrome 51+**, **Firefox 47+**, **Safari 8+**) på både datorer och surfplattor. Mer information finns i [Klientplattformar som stöds](/help/sites-deploying/technical-requirements.md#supported-client-platforms).

### Hämta programvaran {#getting-the-software}

Kunder med ett giltigt underhålls- och supportavtal bör ha fått ett e-postmeddelande med en kod och kunna hämta AEM från [**Adobe licenswebbplats**](https://licensing.adobe.com/). Affärspartners kan begära hämtningsåtkomst från [**spphelp@adobe.com**](mailto:spphelp@adobe.com).

AEM programpaket finns i två former:

* **CQ AEM 6.5 LTS jar:** En fristående körbar *jar* -fil som innehåller allt du behöver för att komma igång.

* **CQ AEM 6.5 LTS war:** En *war*-fil för distribution i en tredjepartsprogramserver.

I följande avsnitt beskriver vi den **fristående installationen**. Mer information om hur du installerar AEM på en programserver finns i [Installation av programserver](/help/sites-deploying/application-server-install.md).

### Lokal standardinstallation {#default-local-install}

1. Skapa en installationskatalog på den lokala datorn. Till exempel:

   Installationsplats för UNIX®: **/opt/aem**

   Installationsplats för Windows: **`C:\Program Files\aem`**

   Det är också vanligt att du installerar exempelinstanser i en mapp direkt på skrivbordet. I vilket fall som helst avser Adobe denna plats i allmänhet som:

   `<aem-install>`

   *Sökvägen till filkatalogen får endast bestå av ASCII-tecken från USA.*

1. Placera filerna **jar** och **license** i den här katalogen:

   ```shell
   <aem-install>/
       <aem-65-lts>.jar
       license.properties
   ```

   Om du inte anger någon `license.properties`-fil dirigerar AEM om webbläsaren till en **välkomstskärm** vid start, där du kan ange en licensnyckel. Du måste begära en giltig licensnyckel från Adobe om du inte redan har en.

1. Om du vill starta instansen i en GUI-miljö dubbelklickar du på filen **`<aem-65-lts>.jar`**.

   Du kan även starta AEM från kommandoraden:

   ```shell
       java -Xmx1024M -jar <aem-65-lts>.jar
   ```

AEM tar några minuter att packa upp burkfilen, installera sig själv och starta. Ovannämnda procedur ger följande resultat:

* en **AEM-författarinstans**
* körs på **localhost**
* på port **4502**

Om du vill komma åt instansen pekar du i webbläsaren på:

**`https://localhost:4502`**

Resultatet i författarinstansen konfigureras automatiskt för anslutning till en **publiceringsinstans** på **`localhost:4503`**.

### Skapa och publicera installationer {#author-and-publish-installs}

Standardinstallationen (en **författarinstans** på **`localhost:4502`**) kan ändras genom att namnet på filen `jar` ändras innan den startas för första gången. Namnmönstret är:

**`cq-<instance-type>-p<port-number>.jar`**

Byt namn på filen till

**`cq-author-p4502.jar`**

Om du startar programmet körs en författarinstans på **`localhost:4502`**.

På samma sätt byter du namn på och startar filen

**`cq-publish-p4503.jar`**

Resultat i en publiceringsinstans som körs på **`localhost:4503`**.

Du installerar de här två instanserna i

`<aem-install>/author` och

**`<aem-install>/publish`**

Mer information om hur du anpassar installationen finns i:

* [Anpassad fristående installation](/help/sites-deploying/custom-standalone-install.md)
<!-- * [Run Modes](/help/sites-deploying/configure-runmodes.md) -->

### Opackad installationskatalog {#unpacked-install-directory}

När snabbstartsburken startas för första gången packas den upp i samma katalog under en ny underkatalog med namnet `crx-quickstart`. Du bör ha följande:

```xml
<aem-install>/
    license.properties
    <aem-65-lts>.jar
    crx-quickstart/
        app/
        bin/
        conf/
        launchpad/
        logs/
        metrics/
        monitoring/
        opt/
        repository/
        threaddumps/
        eula-de_DE.html
        eula-en_US.html
        eula-fr_FR.html
        eula-ja_JP.html
        readme.txt
```

Om instansen installerades från användargränssnittet öppnas ett webbläsarfönster automatiskt och ett skrivbordsprogramfönster öppnas med instansens värd och port och en på/av-knapp:

![startskärmen](assets/screen_shot_.png)

### Starta och stoppa {#starting-and-stopping}

När AEM har packat upp sig själv och startat för första gången startar du instansen genom att dubbelklicka på burkfilen i installationskatalogen, utan att installera om den.

Om du vill stoppa instansen från det grafiska användargränssnittet klickar du på **på/av** i skrivbordsprogramfönstret.

Du kan också stoppa och starta AEM från kommandoraden. Om du redan har installerat instansen för första gången är **kommandoradsskripten** här:

**`<aem-install>/crx-quickstart/bin/`**

Den här mappen innehåller följande UNIX®-baserade gränssnittsskript:

* **`start`**: Startar instansen
* `stop`: Stoppar instansen
* **`status`**: Rapporterar status för instansen
* **`quickstart`**: Används för att konfigurera startinformation, om det behövs.

Det finns även motsvarande **`bat`** filer för Windows. Mer detaljerad information finns i:

* [Kommandoradens start och stopp](/help/sites-deploying/command-line-start-and-stop.md)

AEM startar och dirigerar automatiskt om webbläsaren till rätt sida, vanligtvis inloggningssidan, till exempel:

`https://localhost:4502/`

![inloggningsskärmen](assets/screen_shot_2019-04-08at83533am.png)


När du har loggat in har du tillgång till AEM. Mer information, beroende på din roll, finns i:

* [Redigering](/help/sites-authoring/first-steps.md)
* [Administratör](/help/sites-administering/home.md)
* [Utvecklar](/help/sites-developing/getting-started.md)
* [Hantera](/help/managing/best-practices.md)

## Avancerad distribution {#advanced-deployment}

Ovanstående avsnitt bör ge dig en god förståelse för grunderna i AEM-installationen. Att installera ett komplett produktionssystem av AEM kan dock medföra en betydligt större komplexitet. Mer information om avancerad installation finns i följande undersidor:

* [Tekniska krav](/help/sites-deploying/technical-requirements.md)
* [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md)
* [Anpassad fristående installation](/help/sites-deploying/custom-standalone-install.md)
* [Installation av programserver](/help/sites-deploying/application-server-install.md)
* [Kommandoradens start och stopp](/help/sites-deploying/command-line-start-and-stop.md)
* [Konfigurerar](/help/sites-deploying/configuring.md)
* [Uppgradera till AEM 6.5 LTS](/help/sites-deploying/upgrade.md)
* [Instruktionsartiklar för konfiguration](/help/sites-deploying/ht-deploy.md)
* [Webbkonsol](/help/sites-deploying/web-console.md)
* [Felsökning av replikering](/help/sites-deploying/troubleshoot-rep.md)
* [Bästa praxis](/help/sites-deploying/best-practices.md)
* [Introduktion till AEM Platform](/help/sites-deploying/platform.md)

