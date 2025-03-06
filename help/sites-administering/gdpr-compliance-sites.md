---
title: AEM Sites - GDPR-beredskap
description: Lär dig mer om hur du hanterar GDPR-förfrågningar i AEM Sites och hur du använder dem.
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Compliance
role: Admin, Architect, Developer, Leader, User, Data Architect, Data Engineer
exl-id: 826dafb8-db6c-4fe4-8b3d-edf7215dc571
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '837'
ht-degree: 0%

---

# AEM Sites - GDPR-beredskap{#aem-sites-gdpr-readiness}

>[!IMPORTANT]
>
>GDPR används som exempel i avsnitten nedan, men de ingående detaljerna gäller alla dataskydds- och sekretessbestämmelser, som GDPR, CCPA och så vidare.

Europeiska unionens allmänna dataskyddsförordning om integritetsskydd får verkan från och med maj 2018.

AEM Sites är redo att hjälpa kunderna med deras GDPR-efterlevnadsskyldigheter. På den här sidan får kunderna hjälp med hur de hanterar GDPR-förfrågningar i AEM Sites. Den beskriver platsen för privata data som lagras och hur du tar bort dem manuellt eller med kod.

Mer information finns på sidan [GDPR på Adobe Privacy Center](https://www.adobe.com/privacy/general-data-protection-regulation.html).

>[!NOTE]
>
>Mer information finns i [AEM GDPR-beredskap](/help/managing/data-protection-and-privacy.md).

## Författarserver {#author-server}

Användarkonton och UGC-innehåll på författarservern beskrivs i [plattformens GDPR-dokumentation](/help/managing/data-protection-and-privacy.md).

## Publish Server {#publish-server}

Användarkonton som används för att autentisera besökare på webbplatsen och UGC-innehåll på publiceringsservern beskrivs i [plattformens GDPR-dokumentation](/help/managing/data-protection-and-privacy.md).

Som standard lagrar inte AEM Sites-komponenter formulärdata som anges av besökare på publiceringsservern. Vi rekommenderar att du vidarebefordrar data till ett tredjepartssystem eller Adobe Campaign för vidare bearbetning.

## Opt-In/Opt-Out {#opt-in-opt-out}

AEM har en [tjänst för cookie-avanmälan](/help/sites-developing/cookie-optout.md) som kan användas för att hantera avanmälan/avanmälan för användare.

## Förbättrade insikter från Analytics {#enhanced-insights-by-analytics}

AEM Sites innehåller en valfri integrering med Enhanced Insights by Analytics som använder funktioner i Adobe Analytics On-Demand Service.

Mer information om hur du hanterar förfrågningar om registrerade GDPR-data relaterade till Adobe Analytics finns i [Adobe Analytics och GDPR](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/an-gdpr-overview.html).

## Förbättrade Personalization efter Target {#enhanced-personalization-by-target}

AEM Sites innehåller en valfri integrering med Enhanced Personalization by Target som använder funktioner i Adobe Target On-Demand Service.

Mer information om hur du hanterar förfrågningar från registrerade GDPR-data relaterade till Adobe Target finns i [Adobe Target - Sekretess och allmänna dataskyddsförordningen](https://developer.adobe.com/target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation/?lang=en).

## ContextHub {#contexthub}

AEM tillhandahåller ett valfritt datalager med [ContextHub](/help/sites-developing/contexthub.md). På så sätt behålls besökarspecifika data i webbläsaren som ska användas för regelbaserad personalisering.

Som standard lagras inte dessa besökardata i AEM. AEM skickar regler till datalagret för att fatta personaliseringsbeslut i webbläsaren.

>[!NOTE]
>
>Före Adobe AEM (CQ) 5.6 skickade ClientContext (en tidigare version av ContextHub) data till servern, men lagrade dem inte.
>
>Adobe AEM 6.4 och tidigare versioner är nu EOL och omfattas inte av denna dokumentation. Se [Äldre versioner av Adobe Experience Manager, CQ och CRX dokumentation](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions).

### Implementera anmälan/avanmälan {#implementing-opt-in-opt-out}

Webbplatsägaren måste implementera en avanmälningskomponent enligt följande riktlinjer.

I dessa riktlinjer används anmälan som standard. Därför måste en besökare på webbplatsen tydligt samtycka till detta innan personuppgifter lagras i webbläsarens (klientsidan) beständighet.

* Avanmälningskomponenten ska inkluderas varje gång ContextHub-komponenten inkluderas.
* De villkor som gäller GDPR för webbplatsen måste visas för webbplatsbesökaren så att de kan

   * acceptera
   * avvisa
   * ändra sitt föregående val

* Om en besökare godkänner villkoren för webbplatsen bör cookien för ContextHub-avanmälan tas bort:

  ```java
  ContextHub.Utils.Cookie.removeItem('cq-opt-out');
  ```

* Om en besökare inte accepterar webbplatsens villkor ska cookie för ContextHub-avanmälan anges:

  ```java
  ContextHub.Utils.Cookie.setItem('cq-opt-out', 1);
  ```

* För att kontrollera om ContextHub körs i avanmälningsläge bör följande anrop göras i webbläsarens konsol:

  ```java
  var isOptedOut = ContextHub.isOptedOut(true) === true;
  // if isOptedOut is true, ContextHub is running in opt-out mode
  ```

### Förhandsgranska Persistence för ContextHub {#previewing-persistence-of-contexthub}

Om du vill förhandsgranska den beständiga ContextHub som används kan användaren:

* Använd webbläsarens konsol, till exempel:

   * Chrome:

      * Öppna Utvecklarverktyg > Program > Lagring:

         * Lokal lagring > (webbplats) > ContextHubPersistence
         * Sessionslagring > (webbplats) > ContextHubPersistence
         * Cookies > (website) > SessionPersistence

   * Firefox:

      * Öppna Utvecklarverktyg > Lagring:

         * Lokal lagring > (webbplats) > ContextHubPersistence
         * Sessionslagring > (webbplats) > ContextHubPersistence
         * Cookies > (website) > SessionPersistence

   * Safari:

      * Öppna Inställningar > Avancerat > Visa menyn Framkalla i menyraden
      * Öppna Framkalla > Visa JavaScript Console

         * Konsol > Lagring > Lokal lagring > (webbplats) > ContextHubPersistence
         * Konsol > Lagring > Sessionslagring > (webbplats) > ContextHubPersistence
         * Konsol > Lagring > Cookies > (webbplats) > ContextHubPersistence

   * Internet Explorer:

      * Öppna Utvecklarverktyg > Konsol

         * localStorage.getItem(&#39;ContextHubPersistence&#39;)
         * sessionStorage.getItem(&#39;ContextHubPersistence&#39;)
         * document.cookie

* Använd ContextHub-API:t i webbläsarens konsol:

   * ContextHub tillhandahåller följande beständiga datalager:

      * ContextHub.Utils.Persistence.Modes.LOCAL (standard)
      * ContextHub.Utils.Persistence.Modes.SESSION
      * ContextHub.Utils.Persistence.Modes.COOKIE
      * ContextHub.Utils.Persistence.Modes.WINDOW

     ContextHub-arkivet definierar vilket beständigt lager som används, och för att visa det aktuella läget för beständigheten bör alla lager kontrolleras.

Så här visar du data som lagras i localStorage:

Om du vill förhandsgranska den beständiga ContextHub som används kan användaren:

* Använd webbläsarens konsol:

   * Chrome - öppna Developer Tools > Application > Storage:

      * Lokal lagring > (webbplats) > ContextHubPersistence
      * Sessionslagring > (webbplats) > ContextHubPersistence
      * Cookies > (website) > SessionPersistence

   * Firefox - öppna Utvecklarverktyg > Lagring:

      * Lokal lagring > (webbplats) > ContextHubPersistence
      * Sessionslagring > (webbplats) > ContextHubPersistence
      * Cookies > (website) > SessionPersistence

* Använd ContextHub-API:t i webbläsarens konsol:

   * ContextHub tillhandahåller följande beständiga datalager:

      * ContextHub.Utils.Persistence.Modes.LOCAL (standard)
      * ContextHub.Utils.Persistence.Modes.SESSION
      * ContextHub.Utils.Persistence.Modes.COOKIE
      * ContextHub.Utils.Persistence.Modes.WINDOW

     ContextHub-arkivet definierar vilket beständigt lager som används, och för att visa det aktuella läget för beständigheten bör alla lager kontrolleras.

Så här visar du data som lagras i localStorage:

```java
var storage = new ContextHub.Utils.Persistence({ mode: ContextHub.Utils.Persistence.Modes.LOCAL });
console.log(storage.getTree());
```

### Rensar Persistence för ContextHub {#clearing-persistence-of-contexthub}

Så här rensar du ContextHub-beständighet:

* Så här rensar du beständighet för inlästa arkiv:

  ```java
  // to be able to fully access persistence layer, Opt-Out must be turned off
  ContextHub.Utils.Cookie.removeItem('cq-opt-out');
  
  // following call asks all currently loaded stores to clear their data
  ContextHub.cleanAllStores();
  
  // following call asks all currently loaded stores to set back default values (provided in their configs)
  ContextHub.resetAllStores();
  ```

* Så här rensar du ett visst beständigt lager, till exempel sessionStorage:

  ```java
  var storage = new ContextHub.Utils.Persistence({ mode: ContextHub.Utils.Persistence.Modes.SESSION });
  storage.setItem('/store', null);
  storage.setItem('/_', null);
  
  // to confirm that nothing is stored:
  console.log(storage.getTree());
  ```

* Om du vill ta bort alla ContextHub-beständiga lager måste rätt kod anropas för alla lager:

   * ContextHub.Utils.Persistence.Modes.LOCAL (standard)
   * ContextHub.Utils.Persistence.Modes.SESSION
   * ContextHub.Utils.Persistence.Modes.COOKIE
   * ContextHub.Utils.Persistence.Modes.WINDOW
