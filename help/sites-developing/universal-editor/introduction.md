---
title: Universell redigerare
description: Läs om flexibiliteten i Universal Editor och hur det kan hjälpa dig att skapa en headless-upplevelse med AEM 6.5 LTS.
feature: Developing
role: Developer
exl-id: 495df631-5bdd-456b-b115-ec8561f33488
source-git-commit: 24bd1f57da3f9ce613ee28276d1ae9465b6dfba6
workflow-type: tm+mt
source-wordcount: '1166'
ht-degree: 0%

---

# Om den universella redigeraren {#universal-editor}

Läs om flexibiliteten i Universal Editor och hur det kan hjälpa dig att skapa en headless-upplevelse med AEM 6.5 LTS.

## Ökning {#overview}

Universal Editor är en mångsidig visuell editor som ingår i Adobe Experience Manager Sites. Man kan göra vad man vill - se-is-what-you-get (WYSIWYG)-redigering av headlessupplevelser.

* Författare drar nytta av den universella redigerarens flexibilitet. Det har stöd för samma enhetliga visuella redigering för alla former av AEM headless-innehåll.
* Utvecklarna drar nytta av universella redigerares mångsidighet eftersom den även stöder en sann frikoppling av implementeringen. Det gör att utvecklare kan använda praktiskt taget vilket ramverk eller vilken arkitektur de vill, utan att det medför några begränsningar för SDK eller teknik.

Mer information finns i [AEM as a Cloud Service-dokumentationen för Universal Editor](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction).

## Arkitektur {#architecture}

Universell redigerare är en tjänst som fungerar tillsammans med AEM för att skapa innehåll utan problem.

* Den universella redigeraren finns på `https://experience.adobe.com/#/aem/editor/canvas` och kan redigera sidor som återges av AEM 6.5 LTS.
* Universal Editor läser AEM-sidan via Dispatcher från AEM författarinstans.
* Universal Editor-tjänsten, som körs på samma värd som Dispatcher, skriver tillbaka ändringarna till AEM författarinstans.

![Författarflöde med den universella redigeraren](assets/author-flow.png)

## Krav {#requirements}

Följande har stöd för Universal Editor:

* AEM 6.5 LTS GA
   * Både lokal värdtjänst och Adobe Managed Services (AMS) stöds.
* [AEM 6.5](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/headless/universal-editor/introduction)
   * Både lokala värdtjänster och AMS-värdtjänster stöds.
* [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction) (release `2023.8.13099` eller senare)

Det här dokumentet fokuserar på AEM 6.5 LTS-stöd i Universal Editor. Om du vill använda den universella redigeraren med AEM 6.5 LTS behöver du följande:

* AEM 6.5 LTS GA
* Dispatcher är korrekt konfigurerat

## Inställningar {#setup}

Så här använder du den universella redigeraren:

1. [Konfigurera tjänster på din AEM-redigeringsinstans.](#configure-aem)
1. [Konfigurera en lokal Universal Editor-tjänst.](#set-up-ue)
1. [Justera din Dispatcher så att den universella redigeringstjänsten kan användas.](#update-dispatcher)

När du har slutfört konfigurationen kan du [instrumentera dina program så att de använder den universella redigeraren.](#instrumentation)

### Konfigurera tjänster {#configure-aem}

Den universella redigeraren använder ett antal tjänster som måste konfigureras.

#### Ange samma webbplatsattribut för cookien `login-token`. {#samesite-attribute}

1. Öppna Configuration Manager.
   * `http://<host>:<port>/system/console/configMgr`
1. Leta reda på **Autentiseringshanteraren för Adobe Granite-token** i listan och klicka på **Ändra konfigurationsvärdena**.
1. I dialogrutan ändrar du attributet **SameSite för cookie-filen för inloggningstoken** (`token.samesite.cookie.attr`) till `Partitioned`.
1. Klicka på **Spara**.

#### Ta bort alternativet `SAMEORIGIN` sidhuvuden i X-bildrutan. {#sameorigin}

1. Öppna Configuration Manager.
   * `http://<host>:<port>/system/console/configMgr`
1. Leta reda på **Huvudservern för Apache Sling** i listan och klicka på **Redigera konfigurationsvärdena**.
1. Ta bort värdet `X-Frame-Options=SAMEORIGIN` från attributet **Additional response headers** (`sling.additional.response.headers`) om det finns.
1. Klicka på **Spara**.

#### Konfigurera autentiseringshanteraren för Adobe Granite-frågeparametern {#query-parameter}

1. Öppna Configuration Manager.
   * `http://<host>:<port>/system/console/configMgr`
1. Leta reda på **Autentiseringshanteraren för Adobe Granite-frågeparametern** i listan och klicka på **Redigera konfigurationsvärdena**.
1. I fältet **Sökväg** (`path`) lägger du till `/` för att aktivera.
   * Ett tomt värde inaktiverar autentiseringshanteraren.
1. Klicka på **Spara**.

#### Definiera vilka innehållssökvägar eller `sling:resourceTypes` som ska vara öppna i den universella redigeraren {#paths}

1. Öppna Configuration Manager.
   * `http://<host>:<port>/system/console/configMgr`
1. Leta reda på **URL-tjänsten för den universella redigeraren** i listan och klicka på **Redigera konfigurationsvärdena**.
1. Definiera för vilka innehållssökvägar eller `sling:resourceTypes` den universella redigeraren ska öppnas.
   * Ange sökvägarna som den universella redigeraren öppnas för i fältet **Öppna mappning för den universella redigeraren**.
   * Ange en lista med resurser som den universella redigeraren öppnar direkt i fältet **Sling:resourceTypes som ska öppnas av den universella redigeraren**.
1. Klicka på **Spara**.
1. Kontrollera din [Externalizer-konfiguration](/help/sites-developing/externalizer.md) och se till att du har minst den lokala miljön, författaren och publiceringsmiljön inställd som i följande exempel:

   ```text
   "local $[env:AEM_EXTERNALIZER_LOCAL;default=http://localhost:4502]",
   "author $[env:AEM_EXTERNALIZER_AUTHOR;default=http://localhost:4502]",
   "publish $[env:AEM_EXTERNALIZER_PUBLISH;default=http://localhost:4503]"
   ```

När dessa konfigurationssteg är klara öppnar AEM den universella redigeraren för sidor i följande ordning:

1. AEM kontrollerar mappningarna under `Universal Editor Opening Mapping` och om innehållet finns under sökvägar som definierats där öppnas Universell redigerare för det.

1. För innehåll utanför de sökvägar som definieras i `Universal Editor Opening Mapping` kontrollerar AEM om innehållet `resourceType` matchar en post i **Sling:resourceTypes som ska öppnas av Universal Editor**. Om den matchar öppnar AEM innehållet i den universella redigeraren på `${author}${path}.html`.
1. I annat fall öppnas sidredigeraren.

Följande variabler är tillgängliga för att definiera dina mappningar under `Universal Editor Opening Mapping`.

* `path`: Innehållssökvägen för resursen som ska öppnas
* `localhost`: Externalizer-post för `localhost` utan schema, till exempel `localhost:4502`
* `author`: Externalizer-post för författare utan schema, till exempel `localhost:4502`
* `publish`: Externalizer-post för publicering utan schema, till exempel `localhost:4503`
* `preview`: Externalizer-post för förhandsgranskning utan schema, till exempel `localhost:4504`
* `env`: `prod`, `stage`, `dev` baserat på definierade körningslägen för Sling
* `token`: Frågetoken krävs för `QueryTokenAuthenticationHandler`

Exempelmappningar:

* Öppna alla sidor under `/content/foo` på AEM Author:
   * `/content/foo:${author}${path}.html?login-token=${token}`
   * Resultat när `https://localhost:4502/content/foo/x.html?login-token=<token>` öppnas
* Öppna alla sidor under `/content/bar` på en fjärr-NextJS-server, med alla variabler som information
   * `/content/bar:nextjs.server${path}?env=${env}&author=https://${author}&publish=https://${publish}&login-token=${token}`
   * Resultat när `https://nextjs.server/content/bar/x?env=prod&author=https://localhost:4502&publish=https://localhost:4503&login-token=<token>` öppnas

### Konfigurera Universal Editor-tjänsten {#set-up-ue}

Med AEM uppdaterat och konfigurerat kan du skapa en lokal universell redigeringstjänst för lokal utveckling och testning.

1. Installera Node.js version >=20.
1. Hämta och packa upp den senaste universella redigeringstjänsten från [Programvarudistribution](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home)
1. Konfigurera den universella redigeringstjänsten med hjälp av miljövariabler eller filen `.env`.
   * [Mer information finns i dokumentationen för AEM as a Cloud Service Universal Editor.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/local-dev#setting-up-service)
   * Observera att du kan behöva använda alternativet `UES_MAPPING` om intern IP-omskrivning krävs.
1. Kör `universal-editor-service.cjs`

### Uppdatera Dispatcher {#update-dispatcher}

Om AEM är konfigurerat och en lokal Universal Editor-tjänst körs måste du tillåta en omvänd proxy för den nya tjänsten [i Dispatcher.](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)

1. Justera författarinstansens värdfil så att den innehåller en omvänd proxy.

   ```html
   <IfModule mod_proxy.c>
    ProxyPass "/universal-editor" "http://localhost:8080"
    ProxyPassReverse "/universal-editor" "http://localhost:8080"
   </IfModule>
   ```

   >[!NOTE]
   >
   >8080 är standardporten. Om du ändrade detta med parametern `UES_PORT` i [din `.env`-fil ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/local-dev#setting-up-service) måste du justera portvärdet här i enlighet med detta.

1. Starta om Apache.

## Instrumentera din app {#instrumentation}

Med AEM uppdaterad och en lokal Universal Editor-tjänst kan du börja redigera headless-innehåll med hjälp av Universell Editor.

Ditt program måste dock vara instrumenterat för att du ska kunna använda den universella redigeraren. Det innefattar att inkludera metataggar för att instruera redigeraren hur och var innehållet ska sparas. Information om den här instrumenteringen finns i [Universal Editor-dokumentationen för AEM as a Cloud Service.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/getting-started#instrument-page)

Observera att följande dokumentation för Universal Editor med AEM as a Cloud Service gäller när du använder den med AEM 6.5 LTS.

* Protokollet i meta-taggen måste vara `aem65` i stället för `aem`.

  ```html
  <meta name="urn:adobe:aue:system:aemconnection" content={`aem65:${getAuthorHost()}`}/>
  ```

* Slutpunkten för Universal Editor-tjänsten måste tillkännages via en metatagg.

  ```html
  <meta name="urn:adobe:aue:config:service" content={`${getAuthorHost()}/universal-editor`}/>
  ```

* I avsnittet `plugins` i komponentdefinitionen måste `aem65` användas i stället för `aem`.

>[!TIP]
>
>En omfattande utvecklarguide till Universal Editor finns i [Universal Editor Overview for AEM Developers](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/developer-overview) i AEM as a Cloud Service-dokumentationen. Observera ändringarna i AEM 6.5 LTS som beskrivs i detta avsnitt.

## Skillnader mellan AEM 6.5 LTS och AEM as a Cloud Service {#differences}

Universal Editor i AEM 6.5 LTS fungerar ungefär på samma sätt som i AEM as a Cloud Service, inklusive användargränssnittet och mycket av installationsprogrammet. Det finns dock skillnader som du bör notera.

* Universal Editor i 6.5 LTS stöder endast headless use case.
* Inställningarna för den universella redigeraren varierar något för 6,5 LTS ([enligt beskrivningen](#setup) i det aktuella dokumentet).
* Universell redigerare i 6.5 LTS använder en annan resursväljare och en annan Content Fragment-väljare än AEM as a Cloud Service.
