---
title: Proxyutveckling för [!DNL Assets]
description: En proxy är en  [!DNL Experience Manager] -instans som använder proxyarbetare för att bearbeta jobb. Lär dig hur du konfigurerar en  [!DNL Experience Manager] proxy, åtgärder som stöds, proxykomponenter och hur du utvecklar en anpassad proxyarbetare.
contentOwner: AG
role: Admin, Architect
solution: Experience Manager, Experience Manager Assets
feature: Proxy Workers
exl-id: 8de16e9d-40b6-49d2-9e6b-1aba13137d78
source-git-commit: a869ffbc6015fd230285838d260434d9c0ffbcb0
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# Proxyutveckling för [!DNL Assets] {#assets-proxy-development}

[!DNL Adobe Experience Manager Assets] använder en proxy för att distribuera bearbetning för vissa uppgifter.

En proxy är en specifik (och ibland separat) Experience Manager-instans som använder proxyarbetare som processorer som hanterar ett jobb och skapar ett resultat. En proxyarbetare kan användas för en mängd olika uppgifter. Om det finns en [!DNL Assets]-proxy kan den användas för att läsa in resurser för återgivning inom Assets. [IDS-proxyarbetaren](indesign.md) använder till exempel en [!DNL Adobe InDesign]-server för att bearbeta filer som ska användas i Assets.

När proxyn är en separat [!DNL Experience Manager]-instans minskar detta inläsningen för [!DNL Experience Manager] -redigeringsinstansen. Som standard kör [!DNL Assets] resurshanteringsuppgifterna i samma JVM (externaliserat via Proxy) för att minska belastningen på [!DNL Experience Manager]-redigeringsinstansen.

## Proxy (HTTP Access) {#proxy-http-access}

En proxy är tillgänglig via HTTP-servern när den är konfigurerad att acceptera bearbetningsjobb på: `/libs/dam/cloud/proxy`. Den här servern skapar ett snedjobb av de bokförda parametrarna. Detta läggs sedan till i proxyjobbkön och ansluts till lämplig proxyarbetare.

### Åtgärder som stöds {#supported-operations}

* `job`

  **Krav**: Parametern `jobevent` måste anges som en serialiserad värdekarta. Detta används för att skapa en `Event` för en jobbprocessor.

  **Resultat**: Lägger till ett nytt jobb. Om det lyckas returneras ett unikt jobb-ID.

```shell
curl -u admin:admin -F":operation=job" -F"someproperty=xxxxxxxxxxxx"
    -F"jobevent=serialized value map" http://localhost:4502/libs/dam/cloud/proxy
```

* `result`

  **Krav**: parametern `jobid` måste anges.

  **Resultat**: Returnerar en JSON-representation av den resulterande noden som skapats av jobbprocessorn.

```shell
curl -u admin:admin -F":operation=result" -F"jobid=xxxxxxxxxxxx"
    http://localhost:4502   /libs/dam/cloud/proxy
```

* `resource`

  **Krav**: Parametern jobid måste anges.

  **Resultat**: Returnerar en resurs som är associerad med det angivna jobbet.

```shell
curl -u admin:admin -F":operation=resource" -F"jobid=xxxxxxxxxxxx"
    -F"resourcePath=something.pdf" http://localhost:4502/libs/dam/cloud/proxy
```

* `remove`

  **Krav**: Parametern jobid måste anges.

  **Resultat**: Tar bort ett jobb om det hittas.

```shell
curl -u admin:admin -F":operation=remove" -F"jobid=xxxxxxxxxxxx"
    http://localhost:4502/libs/dam/cloud/proxy
```

### Proxyarbetare {#proxy-worker}

En proxyarbetare är en processor som hanterar ett jobb och skapar ett resultat. Arbetare finns på proxyinstansen och måste implementera [snedande JobProcessor](https://sling.apache.org/site/eventing-and-jobs.html) för att identifieras som en proxyarbetare.

>[!NOTE]
>
>Arbetaren måste implementera [sling JobProcessor](https://sling.apache.org/site/eventing-and-jobs.html) för att identifieras som en proxyarbetare.

### Klient-API {#client-api}

[`JobService`](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/index.html) är tillgänglig som en OSGi-tjänst som tillhandahåller metoder för att skapa jobb, ta bort jobb och hämta resultat från dessa jobb. Standardimplementeringen av den här tjänsten (`JobServiceImpl`) använder HTTP-klienten för att kommunicera med fjärrproxyservern.

Nedan följer ett exempel på API-användning:

```java
@Reference
 JobService proxyJobService;

 // to create a job
 final Hashtable props = new Hashtable();
 props.put("someproperty", "some value");
 props.put(JobUtil.PROPERTY_JOB_TOPIC, "myworker/job"); // this is an identifier of the worker
 final String jobId = proxyJobService.addJob(props, new Asset[]{asset});

 // to check status (returns JobService.STATUS_FINISHED or JobService.STATUS_INPROGRESS)
 int status = proxyJobService.getStatus(jobId)

 // to get the result
 final String jsonString = proxyJobService.getResult(jobId);

 // to remove job and cleanup
 proxyJobService.removeJob(jobId);
```

### Cloud Service-konfigurationer {#cloud-service-configurations}

<!-- TBD: Cannot find com.day.cq.dam.api.proxy at https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/index.html which were generated in May 2020. Hiding this broken link for now.
>[!NOTE]
>
>Reference documentation for the proxy API is available under [`com.day.cq.dam.api.proxy`](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/dam/api/proxy/package-summary.html).
-->

Både proxy- och proxyarbetarkonfigurationer är tillgängliga via molntjänster som tillgängliga från [!DNL Assets] **Tools**-konsolen eller under `/etc/cloudservices/proxy`. Varje proxyarbetare förväntas lägga till en nod under `/etc/cloudservices/proxy` för arbetarspecifik konfigurationsinformation (till exempel `/etc/cloudservices/proxy/workername`).

>[!NOTE]
>
>Mer information finns i [InDesign Server Proxy Worker-konfiguration](indesign.md#configuring-the-proxy-worker-for-indesign-server) och [Cloud Services-konfiguration](../sites-developing/extending-cloud-config.md).

Nedan följer ett exempel på API-användning:

```java
@Reference(policy = ReferencePolicy.STATIC)
 ProxyConfig proxyConfig;

 // to get proxy config
 Configuration cloudConfig = proxyConfig.getConfiguration();
 final String value = cloudConfig.get("someProperty", "defaultValue");

 // to get worker config
 Configuration cloudConfig = proxyConfig.getConfiguration("workername");
 final String value = cloudConfig.get("someProperty", "defaultValue");
```

### Utveckla en anpassad proxyarbetare {#developing-a-customized-proxy-worker}

[IDS-proxyarbetaren](indesign.md) är ett exempel på en [!DNL Assets]-proxyarbetare som redan har tillhandahållits utanför ramarna för att lägga ut bearbetningen av InDesign-resurser på entreprenad.

Du kan också utveckla och konfigurera din egen [!DNL Assets]-proxyarbetare för att skapa en specialarbetare som skickar och lägger ut [!DNL Assets]-bearbetningsuppgifterna på entreprenad.

Om du konfigurerar en egen anpassad proxyarbetare måste du:

* Konfigurera och implementera (med Sling-händelse):

   * ett anpassat jobbämne
   * en anpassad jobbhändelsehanterare

* Använd sedan JobService API för att:

   * skicka ditt anpassade jobb till proxyn
   * hantera ditt jobb

* Om du vill använda utkastet från ett arbetsflöde måste du implementera ett anpassat externt steg med WorkflowExternalProcess API och JobService API.

Följande diagram och steg visar hur du fortsätter:

![chlimage_1-249](assets/chlimage_1-249.png)

>[!NOTE]
>
>I följande steg anges InDesign-motsvarigheter som referensexempel.

1. Ett [delningsjobb](https://sling.apache.org/site/eventing-and-jobs.html) används, så du måste definiera ett jobbämne för ditt användningsfall.

   Se till exempel `IDSJob.IDS_EXTENDSCRIPT_JOB` för IDS-proxyarbetaren.

1. Det externa steget används för att utlösa händelsen och sedan vänta tills det är klart. Detta görs genom att avfråga ID:t. Utveckla egna steg för att implementera nya funktioner.

   Implementera en `WorkflowExternalProcess`, använd sedan JobService API och ditt jobbämne för att förbereda en jobbhändelse och skicka den till JobService (en OSGi-tjänst).

   Se till exempel `INDDMediaExtractProcess`.java för IDS-proxyarbetaren.

1. Implementera en jobbhanterare för ditt ämne. Hanteraren kräver utveckling så att den utför din specifika åtgärd och anses vara implementeringen av arbetaren.

   Se till exempel `IDSJobProcessor.java` för IDS-proxyarbetaren.

1. Använd `ProxyUtil.java` i dammkompositioner. På så sätt kan du skicka jobb till arbetare med dammproxyn.

>[!NOTE]
>
>Det som proxyramverket [!DNL Assets] inte tillhandahåller är poolmekanismen.
>
>Integrationen [!DNL InDesign] ger åtkomst till en pool med [!DNL InDesign] servrar (IDSPool). Den här poolen är specifik för [!DNL InDesign]-integrering och ingår inte i [!DNL Assets]-proxyramverket.

>[!NOTE]
>
>Synkronisering av resultat:
>
>Om ingen instans använder samma proxy stannar bearbetningsresultatet kvar hos proxyn. Det är klientens (Experience Manager Author) uppgift att begära resultatet med samma unika jobb-ID som det som anges för klienten när jobbet skapas. Proxyservern utför jobbet och ser till att resultatet är klart att begäras.
