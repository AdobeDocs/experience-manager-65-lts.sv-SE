---
title: Skapa och använda jobb för avlastning
description: Funktionen Apache Sling Discovery innehåller ett Java-API som gör att du kan skapa JobManager-jobb och JobConsumer-tjänster som använder dem
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: cb3826ca-6724-47a0-8454-5f737b215760
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---

# Skapa och använda jobb för avlastning{#creating-and-consuming-jobs-for-offloading}

Funktionen Apache Sling Discovery tillhandahåller ett Java-API som gör att du kan skapa JobManager-jobb och JobConsumer-tjänster som använder dem.

Mer information om hur du skapar avlastning av topologier och konfigurerar ämnesförbrukning finns i [Avlastning av jobb](/help/sites-deploying/offloading.md).

## Hantera jobbnyttolaster {#handling-job-payloads}

Avlastningsramverket definierar två jobbegenskaper som du använder för att identifiera jobbnyttolasten. Replikeringsagenterna som avlastar använder dessa egenskaper för att identifiera de resurser som ska replikeras till instanserna i topologin:

* `offloading.job.input.payload`: En kommaavgränsad lista med innehållssökvägar. Innehållet replikeras till den instans som kör jobbet.
* `offloading.job.output.payload`: En kommaavgränsad lista med innehållssökvägar. När jobbkörningen är klar replikeras jobbnyttolasten till de här sökvägarna i instansen som skapade jobbet.

Använd uppräkningen `OffloadingJobProperties` för att referera till egenskapsnamnen:

* `OffloadingJobProperties.INPUT_PAYLOAD.propertyName()`
* `OffloadingJobProperties.OUTPUT_PAYLOAD.propetyName()`

Jobb kräver inte nyttolaster. Nyttolasten är dock nödvändig om jobbet kräver ändring av en resurs och jobbet avlastas till en dator som inte skapade jobbet.

## Skapar jobb för avlastning {#creating-jobs-for-offloading}

Skapa en klient som anropar metoden JobManager.addJob för att skapa ett jobb som automatiskt körs av en JobConsumer. Ange följande information för att skapa jobbet:

* Ämne: Jobbämnet.
* Namn: (valfritt)
* Egenskapskarta: Ett `Map<String, Object>`-objekt som innehåller valfritt antal egenskaper, till exempel indatanyttolastsökvägar och utdatanyttolastsökvägar. Det här Map-objektet är tillgängligt för det JobConsumer-objekt som kör jobbet.

I följande exempeltjänst skapas ett jobb för ett givet ämne och en angiven nyttolastsökväg.

```java
package com.adobe.example.offloading;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.apache.felix.scr.annotations.Reference;

import java.util.HashMap;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.JobManager;

import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.resource.ResourceResolver;

import com.adobe.granite.offloading.api.OffloadingJobProperties;

@Component
@Service
public class JobGeneratorImpl implements JobGenerator  {

 @Reference
 private JobManager jobManager;
 @Reference ResourceResolverFactory resolverFactory;

 public String createJob(String topic, String payload) throws Exception {
  Job offloadingJob;

  ResourceResolver resolver = resolverFactory.getResourceResolver(null);
  if(resolver.getResource(payload)!=null){

   HashMap<String, Object> jobprops = new HashMap<String, Object>();
   jobprops.put(OffloadingJobProperties.INPUT_PAYLOAD.propertyName(), payload);

   offloadingJob = jobManager.addJob(topic, null, jobprops);
  } else {
   throw new Exception("Payload for job cannot be found");
  }
  if (offloadingJob == null){
   throw new Exception ("Offloading job could not be created");
  }
  return offloadingJob.getId();
 }
}
```

Loggen innehåller följande meddelande när JobGeneratorImpl.createJob anropas för `com/adobe/example/offloading`-avsnittet och `/content/geometrixx/de/services`-nyttolasten:

```shell
10.06.2013 15:43:33.868 *INFO* [JobHandler: /etc/workflow/instances/2013-06-10/model_1554418768647484:/content/geometrixx/en/company] com.adobe.example.offloading.JobGeneratorImpl Received request to make job for topic com/adobe/example/offloading and payload /content/geometrixx/de/services
```

## Utveckla en jobbkonsument {#developing-a-job-consumer}

Om du vill förbruka jobb utvecklar du en OSGi-tjänst som implementerar gränssnittet `org.apache.sling.event.jobs.consumer.JobConsumer`. Identifiera med ämnet som ska användas med egenskapen `JobConsumer.PROPERTY_TOPICS`.

Följande exempel på JobConsumer-implementering registreras med ämnet `com/adobe/example/offloading`. Konsumenten ställer helt enkelt in egenskapen Förbrukad för noden med nyttolastinnehåll på true.

```java
package com.adobe.example.offloading;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Property;
import org.apache.felix.scr.annotations.Reference;
import org.apache.felix.scr.annotations.Service;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.JobManager;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.jcr.Node;

import com.adobe.granite.offloading.api.OffloadingJobProperties;

@Component
@Service
public class MyJobConsumer implements JobConsumer {

 public static final String TOPIC = "com/adobe/example/offloading";

 @Property(value = TOPIC)
 static final String myTopic = JobConsumer.PROPERTY_TOPICS;

 @Reference
 private ResourceResolverFactory resolverFactory;

 @Reference
 private JobManager jobManager;

 private final Logger log = LoggerFactory.getLogger(getClass());

 public JobResult process(Job job) {
  JobResult result = JobResult.FAILED;
  String topic = job.getTopic();
  log.info("Consuming job of topic: {}", topic);
  String payloadIn =  (String) job.getProperty(OffloadingJobProperties.INPUT_PAYLOAD.propertyName());
  String payloadOut =  (String) job.getProperty(OffloadingJobProperties.OUTPUT_PAYLOAD.propertyName());

  log.info("Job has Input Payload {} and Output Payload {}",payloadIn, payloadOut);

  ResourceResolver resolver = null;
  try {
   resolver = resolverFactory.getAdministrativeResourceResolver(null);
   Session session = resolver.adaptTo(Session.class);
   Node inNode = session.getNode(payloadIn);
   inNode.getNode(Node.JCR_CONTENT).setProperty("consumed",true);
   result = JobResult.OK;
  }catch (Exception e){
   log.info("ERROR -- JOB RESULT IS FAILURE " + e.getMessage());
   result = JobResult.FAILED;
  }
  log.info("Job OK for payload {}",payloadIn);
  return result;
 }
}
```

Klassen MyJobConsumer genererar följande loggmeddelanden för indatanyttolast för /content/geometrixx/de/services:

```shell
10.06.2013 16:02:40.803 *INFO* [pool-7-thread-17-<main queue>(com/adobe/example/offloading)] com.adobe.example.offloading.MyJobConsumer Consuming job of topic: com/adobe/example/offloading
10.06.2013 16:02:40.803 *INFO* [pool-7-thread-17-<main queue>(com/adobe/example/offloading)] com.adobe.example.offloading.MyJobConsumer Job has Input Payload /content/geometrixx/de/services and Output Payload /content/geometrixx/de/services
10.06.2013 16:02:40.884 *INFO* [pool-7-thread-17-<main queue>(com/adobe/example/offloading)] com.adobe.example.offloading.MyJobConsumer Job OK for payload /content/geometrixx/de/services
```

Egenskapen Konsumerad kan observeras med CRXDE Lite:

![chlimage_1-25](assets/chlimage_1-25a.png)

## Maven Dependencies {#maven-dependencies}

Lägg till följande beroendedefinitioner i filen pom.xml så att Maven kan lösa de avlastningsrelaterade klasserna.

```xml
<dependency>
   <groupId>org.apache.sling</groupId>
   <artifactId>org.apache.sling.event</artifactId>
   <version>3.1.5-R1485539</version>
   <scope>provided</scope>
</dependency>
<dependency>
   <groupId>com.adobe.granite</groupId>
   <artifactId>com.adobe.granite.offloading.core</artifactId>
   <version>1.0.4</version>
   <scope>provided</scope>
</dependency>
```

I de föregående exemplen krävdes även följande beroendedefinitioner:

```xml
<dependency>
   <groupId>org.apache.sling</groupId>
   <artifactId>org.apache.sling.api</artifactId>
   <version>2.4.3-R1488084</version>
   <scope>provided</scope>
</dependency>

<dependency>
   <groupId>org.apache.sling</groupId>
   <artifactId>org.apache.sling.jcr.jcr-wrapper</artifactId>
   <version>2.0.0</version>
   <scope>provided</scope>
</dependency>
```
