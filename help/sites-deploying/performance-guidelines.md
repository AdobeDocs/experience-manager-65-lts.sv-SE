---
title: Riktlinjer för prestanda
description: Den här artikeln innehåller allmänna riktlinjer för hur du optimerar prestandan för din AEM-distribution.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: configuring
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2937'
ht-degree: 1%

---

# Riktlinjer för prestanda{#performance-guidelines}

Den här sidan innehåller allmänna riktlinjer för hur du optimerar prestandan för din AEM-distribution. Om du inte har använt AEM tidigare bör du granska följande sidor innan du börjar läsa riktlinjerna för prestanda:

* [AEM Basic Concepts](/help/sites-deploying/deploy.md#basic-concepts)
* [Översikt över lagring i AEM](/help/sites-deploying/storage-elements-in-aem-6.md#overview-of-storage-in-aem)
* [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md)
* [Tekniska krav](/help/sites-deploying/technical-requirements.md)

Illustrator below are the deployment options available for AEM (scroll to view all the options):

<table>
 <tbody>
  <tr>
   <td><p><strong>AEM</strong></p> <p><strong>Produkt</strong></p> </td>
   <td><p><strong>Topologi</strong></p> </td>
   <td><p><strong>Operativsystem</strong></p> </td>
   <td><p><strong>Programserver</strong></p> </td>
   <td><p><strong>JRE</strong></p> </td>
   <td><p><strong>Dokumentskydd</strong></p> </td>
   <td><p><strong>Micro Kernel</strong></p> </td>
   <td><p><strong>Datastore</strong></p> </td>
   <td><p><strong>Indexering</strong></p> </td>
   <td><p><strong>Webbserver</strong></p> </td>
   <td><p><strong>Webbläsare</strong></p> </td>
   <td><p><strong>Experience Cloud</strong></p> </td>
  </tr>
  <tr>
   <td><p>Sites</p> </td>
   <td><p>Ej-HA</p> </td>
   <td><p>Windows</p> </td>
   <td><p>CQSE</p> </td>
   <td><p>Oracle</p> </td>
   <td><p>LDAP</p> </td>
   <td><p>tar</p> </td>
   <td><p>Segment</p> </td>
   <td><p>Egenskap</p> </td>
   <td><p>Apache</p> </td>
   <td><p>Edge</p> </td>
   <td><p>Mål</p> </td>
  </tr>
  <tr>
   <td><p>Assets</p> </td>
   <td><p>Publish-HA</p> </td>
   <td><p>Solaris™</p> </td>
   <td><p>WebLogic</p> </td>
   <td><p>IBM®</p> </td>
   <td><p>SAML</p> </td>
   <td><p>MongoDB</p> </td>
   <td><p>Fil</p> </td>
   <td><p>Lucene</p> </td>
   <td><p>IIS</p> </td>
   <td><p>IE</p> </td>
   <td><p>Analyser</p> </td>
  </tr>
  <tr>
   <td><p>Communities</p> </td>
   <td><p>Author-CS</p> </td>
   <td><p>Red Hat®</p> </td>
   <td><p>WebSphere®</p> </td>
   <td><p>HP</p> </td>
   <td><p>Oauth</p> </td>
   <td><p>RDB/Oracle</p> </td>
   <td><p>S3/Azure</p> </td>
   <td><p>Solr</p> </td>
   <td><p>iPlanet</p> </td>
   <td><p>FireFox</p> </td>
   <td><p>Campaign</p> </td>
  </tr>
  <tr>
   <td><p>Forms</p> </td>
   <td><p>Författaravlastning</p> </td>
   <td><p>HP-UX</p> </td>
   <td><p>Tomcat</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>RDB/DB2</p> </td>
   <td><p>MongoDB</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>Chrome</p> </td>
   <td><p>Social</p> </td>
  </tr>
  <tr>
   <td><p>Mobil</p> </td>
   <td><p>Författarkluster</p> </td>
   <td><p>IBM® AIX®</p> </td>
   <td><p>JBoss®</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>RDB/MySQL</p> </td>
   <td><p>RDBMS</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>Safari</p> </td>
   <td><p>Målgrupp</p> </td>
  </tr>
  <tr>
   <td><p>Flera platser</p> </td>
   <td><p>ASRP</p> </td>
   <td><p>SUSE®</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>RDB/SQLServer</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>Assets</p> </td>
  </tr>
  <tr>
   <td><p>Commerce</p> </td>
   <td><p>MSRP</p> </td>
   <td><p>APPLE OS</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>Aktivering</p> </td>
  </tr>
  <tr>
   <td><p>Dynamiska medier</p> </td>
   <td><p>JSRP</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p>Mobil</p> </td>
  </tr>
  <tr>
   <td><p>Brand Portal</p> </td>
   <td><p>J2E</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p>AoD</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p>LiveFyre</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p>Screens</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p>Dokumentsäkerhet</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p>Processhantering</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
  <tr>
   <td><p>datorprogram</p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
   <td><p> </p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Riktlinjerna gäller i huvudsak AEM Sites.

## När prestandarådarna ska användas {#when-to-use-the-performance-guidelines}

Använd riktlinjerna för prestanda i följande situationer:

* **Första gången du distribuerar**: När du planerar att distribuera AEM Sites eller Assets för första gången är det viktigt att du förstår vilka alternativ som är tillgängliga. Särskilt när du konfigurerar Micro Kernel, Node Store och Data Store (jämfört med standardinställningarna). Om du till exempel ändrar standardinställningarna för datalagret för tarMK till fildatalagret.
* **Uppgraderar till en ny version**: När du uppgraderar till en ny version är det viktigt att du förstår prestandaskillnaderna jämfört med körningsmiljön. Du kan till exempel uppgradera från AEM 6.1 till 6.2 eller från AEM 6.0 CRX2 till 6.2 OAK.
* **Svarstiden är långsam**: När den valda Nodestore-arkitekturen inte uppfyller dina krav är det viktigt att förstå prestandaskillnaderna jämfört med andra topologialternativ. Du kan till exempel distribuera tarMK i stället för MongoMK, eller använda en File Data Store i stället för ett Amazon S3- eller Microsoft® Azure-datalager.
* **Lägger till fler författare**: Om den rekommenderade TjärMK-topologin inte uppfyller prestandakraven och om redigeringsnoden har uppnått den maximala tillgängliga kapaciteten, måste du förstå prestandaskillnaderna. Jämför med att använda MongoMK med tre eller fler Author-noder. Du kan till exempel distribuera MongoMK i stället för tarMK.
* **Lägga till mer innehåll**: När den rekommenderade datalagerarkitekturen inte uppfyller dina krav är det viktigt att förstå prestandaskillnaderna jämfört med andra datalageralternativ. Exempel: använda Amazon S3 eller Microsoft® Azure Data Store i stället för ett File Data Store.

## Introduktion {#introduction}

I det här kapitlet ges en allmän översikt över AEM-arkitekturen och dess viktigaste komponenter. Den innehåller också riktlinjer för utveckling och beskriver de testscenarier som används i prestandatesterna TjärMK och MongoMK.

### AEM Platform {#the-aem-platform}

AEM-plattformen består av följande komponenter:

![chlimage_1](assets/chlimage_1a.png)

Mer information om AEM finns i [Vad är AEM](/help/sites-deploying/deploy.md#what-is-aem).

### AEM-arkitekturen {#the-aem-architecture}

Det finns tre viktiga byggstenar för en driftsättning av AEM. **Författarinstansen** som används av innehållsförfattare, redigerare och godkännare för att skapa och granska innehåll. När innehållet godkänns publiceras det till en andra instanstyp med namnet **Publish Instance** som slutanvändarna kommer åt det från. Det tredje byggblocket är **Dispatcher** som är en modul som hanterar cachelagring och URL-filtrering och som är installerad på webbservern. Mer information om AEM-arkitekturen finns i [Vanliga distributionsscenarier](/help/sites-deploying/deploy.md#typical-deployment-scenarios).

![chlimage_1-1](assets/chlimage_1-1a.png)

### Micro Kernels {#micro-kernels}

Micro Kernels fungerar som persistencehanterare i AEM. Det finns tre typer av Micro Kernels som används med AEM: tarMK, MongoDB och Relational Database (med begränsat stöd). Vilken som passar bäst beror på instansens syfte och vilken distributionstyp du överväger. Mer information om Micro Kernels finns på sidan [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md).

![chlimage_1-2](assets/chlimage_1-2a.png)

### Nodestore {#nodestore}

I AEM kan binära data lagras oberoende av innehållsnoder. Platsen där binära data lagras kallas **datalagret**, medan platsen för innehållsnoderna och -egenskaperna kallas **nodarkivet**.

>[!NOTE]
>
>Adobe rekommenderar att TARMK är standardbeständighetstekniken som används av kunder för både AEM Author och Publish.

>[!CAUTION]
>
>Mikrokärnan i relationsdatabasen har begränsat stöd. Kontakta [Adobe kundtjänst](https://experienceleague.adobe.com/?support-solution=General&amp;support-tab=home#support) innan du använder den här typen av mikrokärna.

![chlimage_1-3](assets/chlimage_1-3a.png)

### Datalager {#data-store}

När du hanterar ett stort antal binära filer bör du använda ett externt datalager i stället för standardnodarkiven för att maximera prestandan. Om ditt projekt till exempel kräver många medieresurser och du lagrar dem under Arkiv eller Azure/S3 Data Store blir det snabbare att komma åt dem än att lagra dem direkt i en MongoDB.

Mer information om tillgängliga konfigurationsalternativ finns i [Konfigurera nod- och datalager](/help/sites-deploying/data-store-config.md).

>[!NOTE]
>
>Adobe rekommenderar att du väljer att distribuera AEM på Azure eller Amazon Web Services (AWS) med Adobe Managed Services. Kunderna får dra nytta av ett team som har erfarenhet och kompetens av att driftsätta och driva AEM i dessa molnmiljöer. Se [ytterligare dokumentation om Adobe Managed Services](https://business.adobe.com/products/experience-manager/managed-services.html?aemClk=t).
>
>Adobe rekommenderar att du arbetar direkt med molnleverantören för rekommendationer om hur du distribuerar AEM på Azure eller AWS utanför Adobe Managed Services. Eller samarbeta med en av Adobe partners som stöder driftsättningen av AEM i den molnmiljö du föredrar. Den valda molnleverantören eller partnern ansvarar för storleksspecifikationerna, designen och implementeringen av den arkitektur de stöder för att uppfylla just dina krav på prestanda, belastning, skalbarhet och säkerhet.
>
>>Se även sidan [Tekniska krav](/help/sites-deploying/technical-requirements.md#supported-platforms).

### Sök {#search-features}

I det här avsnittet finns de anpassade indexproviders som används med AEM. Mer information om indexering finns i [Oak-frågor och indexering](/help/sites-deploying/queries-and-indexing.md).

>[!NOTE]
>
>För de flesta installationer rekommenderar Adobe att du använder Lucene-index. Använd Solr endast för skalbarhet vid specialiserad och komplex driftsättning.

![chlimage_1-4](assets/chlimage_1-4a.png)

### Utvecklingsriktlinjer {#development-guidelines}

Utveckla för AEM med sikte på **prestanda och skalbarhet**. Här följer några tips som du kan följa:

**GÖR**

* Separera presentation, logik och innehåll
* Använd befintliga AEM API:er (t.ex. Sling) och verktyg (t.ex. replikering)
* Utveckla i rätt sammanhang
* Utveckla för optimal tillgänglighet
* Minimera antalet besparingar (t.ex. genom att använda tillfälliga arbetsflöden)
* Kontrollera att alla HTTP-slutpunkter är RESTful
* Begränsa omfattningen av JCR-observation
* Tänk på asynkron tråd

**GÖR INTE**

* Använd inte JCR-API:er direkt om du kan
* Ändra inte /libs, utan använd i stället övertäckningar
* Använd inte frågor där det är möjligt
* Använd inte Sling Bindings för att få OSGi-tjänster i Java™-kod, utan använd istället:

   * @Referens i en DS-komponent
   * @Injicera i en körningsmodell
   * sling.getService() i en lättanvänd klass
   * sling.getService() i en JSP
   * en ServiceTracker
   * direktåtkomst till OSGi-tjänstregistret

Mer information om utveckling för AEM finns i [Utveckla - Grunderna](/help/sites-developing/the-basics.md). Mer information finns i [Bästa metoder för utveckling](/help/sites-developing/best-practices.md).

### Benchmark-scenarier {#benchmark-scenarios}

>[!NOTE]
>
>Alla test som visas på den här sidan har utförts i laboratoriemiljö.

De testscenarier som beskrivs nedan används för prestandatestavsnitten i kapitlen TjärMK, MongoMk och TjärMK jämfört med MongoMk. Läs avsnittet Scenario i tabellen [Tekniska specifikationer](/help/sites-deploying/performance-guidelines.md#tarmk-performance-benchmark) om du vill se vilket scenario som användes för ett visst test.

**Scenario för en produkt**

AEM Assets:

* Användarinteraktioner: Bläddra i Assets/Sök i Assets/Hämta resurs/Läs resursmetadata/Uppdatera resursmetadata/Överför resurs/Kör arbetsflöde för överföring av resurs
* Körningsläge: samtidiga användare, en interaktion per användare

**Scenario för blandade produkter**

AEM Sites + Assets:

* Webbplatsanvändarinteraktioner: Läs artikelsida / Läs sida / Skapa stycke / Redigera stycke / Skapa innehållssida / Aktivera innehållssida / Författarsökning
* Assets användarinteraktioner: Bläddra i Assets/Sök i Assets/Hämta resurs/Läs resursmetadata/Uppdatera resursmetadata/Överför resurs/Kör arbetsflöde för överföring av resurs
* Körningsläge: samtidiga användare, blandade interaktioner per användare

**Vertikalt scenario för användning**

Media:

* `Read Article Page (27.4%), Read Page (10.9%), Create Session (2.6%), Activate Content Page (1.7%), Create Content Page (0.4%), Create Paragraph (4.3%), Edit Paragraph (0.9%), Image Component (0.9%), Browse Assets (20%), Read Asset Metadata (8.5%), Download Asset (4.2%), Search Asset (0.2%), Update Asset Metadata (2.4%), Upload Asset (1.2%), Browse Project (4.9%), Read Project (6.6%), Project Add Asset (1.2%), Project Add Site (1.2%), Create Project (0.1%), Author Search (0.4%)`
* Körningsläge: samtidiga användare, blandade interaktioner per användare

## tarMK {#tarmk}

I det här kapitlet finns allmänna riktlinjer för prestanda för TjäraMK som specificerar minimikraven för arkitektur och inställningskonfigurationen. Riktmärkestester tillhandahålls också för ytterligare förtydliganden.

Adobe rekommenderar att TjäraMK är standardbeständighetstekniken som används av kunder i alla distributionsscenarier, både för AEM Author och Publish.

Mer information om TjärMK finns i [Distributionsscenarier](/help/sites-deploying/recommended-deploys.md#deployment-scenarios) och [Tjärlagring](/help/sites-deploying/storage-elements-in-aem-6.md#tar-storage).

### TaMK - riktlinjer för minimiarkitektur {#tarmk-minimum-architecture-guidelines}

>[!NOTE]
>
>De riktlinjer för minimiarkitektur som presenteras nedan gäller produktionsmiljöer och stora trafikplatser. Riktlinjerna är **inte** de [lägsta specifikationerna](/help/sites-deploying/technical-requirements.md#prerequisites) som ska köra AEM.

För att få goda prestanda när du använder tarMK bör du utgå från följande arkitektur:

* En författarinstans
* Två publiceringsinstanser
* Två utskickare

Nedan visas riktlinjerna för arkitektur för AEM webbplatser och AEM Assets.

>[!NOTE]
>
>Binärfri replikering ska vara **ON** om fildatalagret delas.

**Riktlinjer för Tjärarkitektur för AEM Sites**

![chlimage_1-5](assets/chlimage_1-5a.png)

**Riktlinjer för Tjärarkitektur för AEM Assets**

![chlimage_1-6](assets/chlimage_1-6a.png)

### Riktlinje för inställningar för TARMK {#tarmk-settings-guideline}

För bästa prestanda bör du följa riktlinjerna nedan. Instruktioner om hur du ändrar inställningarna finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).

<table>
 <tbody>
  <tr>
   <td><strong>Inställning</strong></td>
   <td><strong>Parameter</strong></td>
   <td><strong>Värde</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>Sling-jobbköer</td>
   <td><code>queue.maxparallel</code></td>
   <td>Ange värdet till hälften av antalet CPU-kärnor. </td>
   <td>Som standard är antalet samtidiga trådar per jobbkö lika med antalet CPU-kärnor.</td>
  </tr>
  <tr>
   <td>Bevilja tillfällig arbetsflödeskö</td>
   <td><code>Max Parallel</code></td>
   <td>Ange värdet till hälften av antalet CPU-kärnor</td>
   <td> </td>
  </tr>
  <tr>
   <td>JVM-parametrar</td>
   <td><p><code>Doak.queryLimitInMemory</code></p> <p><code>Doak.queryLimitReads</code></p> <p><code>Dupdate.limit</code></p> <p><code>Doak.fastQuerySize</code></p> </td>
   <td><p>500000</p> <p>100000</p> <p>250000</p> <p>True</p> </td>
   <td>Om du vill förhindra att expanderande frågor överbelastar systemen lägger du till dessa JVM-parametrar i AEM startskript.</td>
  </tr>
  <tr>
   <td>Lucene-indexkonfiguration</td>
   <td><p><code>CopyOnRead</code></p> <p><code>CopyOnWrite</code></p> <p><code>Prefetch Index Files</code></p> </td>
   <td><p>Aktiverad</p> <p>Aktiverad</p> <p>Aktiverad</p> </td>
   <td>Mer information om tillgängliga parametrar finns på <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html">den här sidan</a>.</td>
  </tr>
  <tr>
   <td>Datalager = S3 Datastore</td>
   <td><p><code>maxCachedBinarySize</code></p> <p><code>cacheSizeInMB</code></p> </td>
   <td><p>1048576 (1 MB) eller mindre</p> <p>2-10 % av maximal stackstorlek</p> </td>
   <td>Se även <a href="/help/sites-deploying/data-store-config.md#data-store-configurations">Datalagerkonfigurationer</a>.</td>
  </tr>
  <tr>
   <td>Arbetsflöde för DAM-uppdatering</td>
   <td><code>Transient Workflow</code></td>
   <td>checked</td>
   <td>Det här arbetsflödet hanterar uppdateringen av resurser.</td>
  </tr>
  <tr>
   <td>DAM MetaData Writeback</td>
   <td><code>Transient Workflow</code></td>
   <td>checked</td>
   <td>Det här arbetsflödet hanterar XMP-återskrivning till den ursprungliga binärfilen och anger det senaste ändringsdatumet i JCR.</td>
  </tr>
 </tbody>
</table>

### Resultatjämförelse för tarMK {#tarmk-performance-benchmark}

#### Tekniska specifikationer {#technical-specifications}

Testerna utfördes på följande specifikationer:

| | **Författarnod** |
|---|---|
| Server | Maskinvara av barrmetall (HP) |
| Operativsystem | Red Hat® Linux® |
| CPU / Kärnor | Intel(R) Xeon(R) CPU E5-2407 @2,40 GHz, 8 kärnor |
| RAM | 32 GB |
| Skiva | Magnetiskt |
| Java™ | Oracle JRE version 8 |
| JVM-heap | 16 GB |
| Produkt | AEM 6.2 |
| Nodestore | tarMK |
| Datastore | Fil-DS |
| Scenario | En produkt: Assets/30 samtidiga trådar |

#### Resultat av prestandatest {#performance-benchmark-results}

>[!NOTE]
>
>Siffrorna nedan har normaliserats till 1 som baslinje och är inte de faktiska flödenumren.

![chlimage_1-7](assets/chlimage_1-7a.png) ![chlimage_1-8](assets/chlimage_1-8a.png)

## MongoMK {#mongomk}

Den främsta anledningen till att du väljer MongoMK-beständighetsbackend framför tarMK är att skalförändra instanserna vågrätt. Detta innebär att två eller flera aktiva författarinstanser alltid körs och att MongoDB används som beständigt lagringssystem. Behovet av att köra mer än en författarinstans beror i allmänhet på att CPU och minneskapaciteten på en enda server, som stöder alla samtidiga redigeringsaktiviteter, inte längre är hållbara.

Mer information om TjärMK finns i [Distributionsscenarier](/help/sites-deploying/recommended-deploys.md#deployment-scenarios) och [Mongo-lagring](/help/sites-deploying/storage-elements-in-aem-6.md#mongo-storage).

### Riktlinjer för minimiarkitektur i MongoMK {#mongomk-minimum-architecture-guidelines}

För att få bra prestanda när du använder MongoMK bör du utgå från följande arkitektur:

* Tre författarinstanser
* Två publiceringsinstanser
* Tre MongoDB-instanser
* Två utskickare

>[!NOTE]
>
>I produktionsmiljöer används MongoDB alltid som en replikuppsättning med en primär och två sekundära. Läsningar och skrivningar går till huvudlistan och läsningar kan gå till de sekundära. Om lagring inte är tillgängligt kan en av de sekundära ersättas med en skiljedomare, men MongoDB-replikuppsättningar måste alltid bestå av ett ojämnt antal instanser.

>[!NOTE]
>
>Binärfri replikering ska vara **ON** om fildatalagret delas.

![chlimage_1-9](assets/chlimage_1-9a.png)

### Riktlinjer för MongoMK-inställningar {#mongomk-settings-guidelines}

För bästa prestanda bör du följa riktlinjerna nedan. Instruktioner om hur du ändrar inställningarna finns i [Prestandaoptimering](/help/sites-deploying/configuring-performance.md).

<table>
 <tbody>
  <tr>
   <td><strong>Inställning</strong></td>
   <td><strong>Parameter</strong></td>
   <td><strong>Värde (standard)</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>Sling-jobbköer</td>
   <td><code>queue.maxparallel</code></td>
   <td>Ange värdet till hälften av antalet CPU-kärnor. </td>
   <td>Som standard är antalet samtidiga trådar per jobbkö lika med antalet CPU-kärnor.</td>
  </tr>
  <tr>
   <td>Bevilja tillfällig arbetsflödeskö</td>
   <td><code>Max Parallel</code></td>
   <td>Ange värdet till hälften av antalet CPU-kärnor.</td>
   <td> </td>
  </tr>
  <tr>
   <td>JVM-parametrar</td>
   <td><p><code>Doak.queryLimitInMemory</code></p> <p><code>Doak.queryLimitReads</code></p> <p><code>Dupdate.limit</code></p> <p><code>Doak.fastQuerySize</code></p> <p><code>Doak.mongo.maxQueryTimeMS</code></p> </td>
   <td><p>500000</p> <p>100000</p> <p>250000</p> <p>True</p> <p>60000</p> </td>
   <td>Om du vill förhindra att expanderande frågor överbelastar systemen lägger du till dessa JVM-parametrar i AEM startskript.</td>
  </tr>
  <tr>
   <td>Lucene-indexkonfiguration</td>
   <td><p><code>CopyOnRead</code></p> <p><code>CopyOnWrite</code></p> <p><code>Prefetch Index Files</code></p> </td>
   <td><p>Aktiverad</p> <p>Aktiverad</p> <p>Aktiverad</p> </td>
   <td>Mer information om tillgängliga parametrar finns på <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html">den här sidan</a>.</td>
  </tr>
  <tr>
   <td>Datalager = S3 Datastore</td>
   <td><p><code>maxCachedBinarySize</code></p> <p><code>cacheSizeInMB</code></p> </td>
   <td><p>1048576 (1 MB) eller mindre</p> <p>2-10 % av maximal stackstorlek</p> </td>
   <td>Se även <a href="/help/sites-deploying/data-store-config.md#data-store-configurations">Datalagerkonfigurationer</a>.</td>
  </tr>
  <tr>
   <td>DocumentNodeStoreService</td>
   <td><p><code>cache</code></p> <p><code>nodeCachePercentage</code></p> <p><code>childrenCachePercentage</code></p> <p><code>diffCachePercentage</code></p> <p><code>docChildrenCachePercentage</code></p> <p><code>prevDocCachePercentage</code></p> <p><code>persistentCache</code></p> </td>
   <td><p>2048</p> <p>35 (25)</p> <p>20 (10)</p> <p>30 (5)</p> <p>10 (3)</p> <p>4 (4)</p> <p>./cache,size=2048,binary=0,-compact,-compress</p> </td>
   <td><p>Standardstorleken för cachen är 256 MB.</p> <p>Påverkar hur lång tid det tar att göra cachen ogiltig.</p> </td>
  </tr>
  <tr>
   <td>ek-observation</td>
   <td><p><code>thread pool</code></p> <p><code>length</code></p> </td>
   <td><p>min &amp; max = 20</p> <p>50000</p> </td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Prestandatest för MongoMK {#mongomk-performance-benchmark}

### Tekniska specifikationer {#technical-specifications-1}

Testerna utfördes på följande specifikationer:

| | **Författarnod** | **MongoDB-nod** |
|---|---|---|
| Server | Maskinvara av barrmetall (HP) | Maskinvara av barrmetall (HP) |
| Operativsystem | Red Hat® Linux® | Red Hat® Linux® |
| CPU / Kärnor | Intel(R) Xeon(R) CPU E5-2407 @2,40 GHz, 8 kärnor | Intel(R) Xeon(R) CPU E5-2407 @2,40 GHz, 8 kärnor |
| RAM | 32 GB | 32 GB |
| Skiva | Magnetiskt - >1 k IOPS | Magnetiskt - >1 k IOPS |
| Java™ | Oracle JRE version 8 | Ej tillämpligt |
| JVM-heap | 16 GB | Ej tillämpligt |
| Produkt | AEM 6.2 | MongoDB 3.2 WiredTiger |
| Nodestore | MongoMK | Ej tillämpligt |
| Datastore | Fil-DS | Ej tillämpligt |
| Scenario | En produkt: Assets/30 samtidiga trådar | En produkt: Assets/30 samtidiga trådar |

### Resultat av prestandatest {#performance-benchmark-results-1}

>[!NOTE]
>
>Siffrorna nedan har normaliserats till 1 som baslinje och är inte de faktiska flödenumren.

![chlimage_1-10](assets/chlimage_1-10a.png) ![chlimage_1-11](assets/chlimage_1-11a.png)

## tarMK jämfört med MongoMK {#tarmk-vs-mongomk}

Den grundläggande regeln som ska beaktas när du väljer mellan de två är att tarMK är utformat för prestanda, medan MongoMK används för skalbarhet. Adobe rekommenderar att TjäraMK är standardbeständighetstekniken som används av kunder i alla distributionsscenarier, både för AEM Author och Publish.

Den främsta anledningen till att du väljer MongoMK-beständighetsbackend framför tarMK är att skalförändra instanserna vågrätt. Den här funktionen innebär att två eller flera aktiva författarinstanser alltid körs och att MongoDB används som beständigt lagringssystem. Behovet av att köra mer än en författarinstans beror i allmänhet på att CPU och minneskapaciteten på en enda server, som stöder alla samtidiga redigeringsaktiviteter, inte längre är hållbara.

Mer information om TjäraMK och MongoMK finns i [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md#microkernels-which-one-to-use).

### Riktlinjer för tarMK jämfört med MongoMk {#tarmk-vs-mongomk-guidelines}

**Fördelar med tarMK**

* Ändamålsenlig för content management-program
* Filerna är alltid konsekventa och kan säkerhetskopieras med valfritt filbaserat säkerhetskopieringsverktyg
* Tillhandahåller en redundansmekanism - mer information finns i [Cold Standby](/help/sites-deploying/tarmk-cold-standby.md)
* Ger höga prestanda och tillförlitlig datalagring med minimal driftkostnad
* Lägre ägandekostnader (total ägandekostnad)

**Villkor för val av MongoMK**

* Antal namngivna användare som är anslutna under en dag: i tusental eller mer
* Antal samtidiga användare: i hundratals eller fler
* Volym för tillgångsförslag per dag: i hundratusentals eller fler
* Antal sidredigeringar per dag: hundratusentals eller fler
* Antal sökningar per dag: tiotusentals eller fler

### Benchmarks för tarMK jämfört med MongoMK {#tarmk-vs-mongomk-benchmarks}

>[!NOTE]
>
>Siffrorna nedan har normaliserats till 1 som baslinje och är inte faktiska flödesnummer.

### Tekniska specifikationer för scenario 1 {#scenario-technical-specifications}

<table>
 <tbody>
  <tr>
   <td><strong> </strong></td>
   <td><strong>Skapa OAK Node</strong></td>
   <td><strong>MongoDB-nod</strong></td>
   <td> </td>
  </tr>
  <tr>
   <td>Server</td>
   <td>Maskinvara av barrmetall (HP)</td>
   <td>Maskinvara av barrmetall (HP)</td>
   <td> </td>
  </tr>
  <tr>
   <td>Operativsystem</td>
   <td>Red Hat® Linux®</td>
   <td>Red Hat® Linux®</td>
   <td> </td>
  </tr>
  <tr>
   <td>CPU / Kärnor</td>
   <td>Intel(R) Xeon(R) CPU E5-2407 @2,40 GHz, 8 kärnor</td>
   <td>Intel(R) Xeon(R) CPU E5-2407 @2,40 GHz, 8 kärnor</td>
   <td> </td>
  </tr>
  <tr>
   <td>RAM</td>
   <td>32 GB</td>
   <td>32 GB</td>
   <td> </td>
  </tr>
  <tr>
   <td>Skiva</td>
   <td>Magnetiskt - &gt;1 k IOPS</td>
   <td>Magnetiskt - &gt;1 k IOPS</td>
   <td> </td>
  </tr>
  <tr>
   <td>Java™</td>
   <td>Oracle JRE version 8</td>
   <td>Ej tillämpligt</td>
   <td> </td>
  </tr>
  <tr>
   <td>JVM Heap16 GB</td>
   <td>16 GB</td>
   <td>Ej tillämpligt</td>
   <td> </td>
  </tr>
  <tr>
   <td>Produkt </td>
   <td>AEM 6.2</td>
   <td>MongoDB 3.2 WiredTiger</td>
   <td> </td>
  </tr>
  <tr>
   <td>Nodestore</td>
   <td>tarMK eller MongoMK</td>
   <td>Ej tillämpligt</td>
   <td> </td>
  </tr>
  <tr>
   <td>Datastore</td>
   <td>Fil-DS </td>
   <td>Ej tillämpligt</td>
   <td> </td>
  </tr>
  <tr>
   <td>Scenario</td>
   <td><p><br /> En produkt: Assets/30 samtidiga trådar per körning</p> </td>
   <td> </td>
   <td> </td>
  </tr>
 </tbody>
</table>

### Resultat av prestandatest för scenario 1 {#scenario-performance-benchmark-results}

![chlimage_1-12](assets/chlimage_1-12a.png)

### Tekniska specifikationer för scenario 2 {#scenario-technical-specifications-1}

>[!NOTE]
>
>Om du vill aktivera samma antal författare med MongoDB som med ett tarMK-system behöver du ett kluster med två AEM-noder. Ett mongoDB-kluster med fyra noder kan hantera 1,8 gånger så många författare som en tarMK-instans. Ett åtta-nods MongoDB-kluster kan hantera 2,3 gånger så många författare som en tarMK-instans.

<table>
 <tbody>
  <tr>
   <td><strong> </strong></td>
   <td><strong>Författare tarMK-nod</strong></td>
   <td><strong>Författare MongoMK-nod</strong></td>
   <td><strong>MongoDB-nod</strong></td>
  </tr>
  <tr>
   <td>Server</td>
   <td>AWS c3.8xlarge</td>
   <td>AWS c3.8xlarge</td>
   <td>AWS c3.8xlarge</td>
  </tr>
  <tr>
   <td>Operativsystem</td>
   <td>Red Hat® Linux®</td>
   <td>Red Hat® Linux®</td>
   <td>Red Hat® Linux®</td>
  </tr>
  <tr>
   <td>CPU / Kärnor</td>
   <td>32</td>
   <td>32</td>
   <td>32</td>
  </tr>
  <tr>
   <td>RAM</td>
   <td>60 GB</td>
   <td>60 GB</td>
   <td>60 GB</td>
  </tr>
  <tr>
   <td>Skiva</td>
   <td>SSD - 10 000 IOPS</td>
   <td>SSD - 10 000 IOPS</td>
   <td>SSD - 10 000 IOPS</td>
  </tr>
  <tr>
   <td>Java™</td>
   <td>Oracle JRE version 8</td>
   <td><br /> Oracle JRE version 8</td>
   <td>Ej tillämpligt</td>
  </tr>
  <tr>
   <td>JVM Heap16 GB</td>
   <td>30 GB</td>
   <td>30 GB</td>
   <td>Ej tillämpligt</td>
  </tr>
  <tr>
   <td>Produkt </td>
   <td>AEM 6.2</td>
   <td>AEM 6.2</td>
   <td><br /> MongoDB 3.2 WiredTiger</td>
  </tr>
  <tr>
   <td>Nodestore</td>
   <td>tarMK </td>
   <td>MongoMK</td>
   <td><br /> Ej tillämpligt</td>
  </tr>
  <tr>
   <td>Datastore</td>
   <td>Fil-DS </td>
   <td><br /> Fil-DS</td>
   <td><br /> Ej tillämpligt</td>
  </tr>
  <tr>
   <td>Scenario</td>
   <td><p><br /> <br /> Vertikalt användningsfall: Media/2000 samtidiga trådar</p> </td>
   <td></td>
   <td></td>
  </tr>
 </tbody>
</table>

### Resultat av prestandatest för scenario 2 {#scenario-performance-benchmark-results-1}

![chlimage_1-13](assets/chlimage_1-13a.png)

### Riktlinjer för arkitekturskalbarhet för AEM Sites och Assets {#architecture-scalability-guidelines-for-aem-sites-and-assets}

![chlimage_1-14](assets/chlimage_1-14a.png)

## Sammanfattning av riktlinjer för prestanda  {#summary-of-performance-guidelines}

Riktlinjerna på denna sida kan sammanfattas enligt följande:

* **tarMK med fildatastore** - Den rekommenderade arkitekturen för de flesta kunder:

   * Minimitopologi: en författarinstans, två publiceringsinstanser, två utskickare
   * Binärfri replikering aktiveras om fildatalagret delas

* **MongoMK med File DataStore** - Den rekommenderade arkitekturen för vågrät skalbarhet för Author-nivån:

   * Minimitopologi: tre författarinstanser, tre MongoDB-instanser, två publiceringsinstanser, två utskickare
   * Binärfri replikering aktiveras om fildatalagret delas

* **Nodestore** - lagras på den lokala disken, inte ett nätverksanslutet lagringsutrymme (NAS)
* När du använder **Amazon S3**:

   * Amazon S3-datalagret delas mellan författaren och publiceringsskiktet
   * Binär replikering måste vara aktiverad
   * Datastore-skräpinsamlingen kräver en första körning på alla författar- och publiceringsnoder, och sedan en andra körning på författare

* **Anpassat index ska skapas utöver indexvärdet i rutan** - baserat på de vanligaste sökningarna

   * Lucene-index bör användas för anpassade index

* **Om du anpassar arbetsflödet kan prestandan förbättras avsevärt** - Ta bort videomsteget i arbetsflödet Uppdatera resurs, inaktivera avlyssnare som inte används och så vidare.

Mer information finns också på sidan [Rekommenderade distributioner](/help/sites-deploying/recommended-deploys.md).
