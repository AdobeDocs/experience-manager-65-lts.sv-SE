---
title: Universell redigerare
description: Läs om flexibiliteten i Universal Editor och hur den kan hjälpa till att ge kraft åt dina headless-upplevelser med AEM 6.5.
feature: Developing
role: Developer
exl-id: 495df631-5bdd-456b-b115-ec8561f33488
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1264'
ht-degree: 0%

---

# Universell redigerare {#universal-editor}

Läs om flexibiliteten i Universal Editor och hur den kan hjälpa till att ge kraft åt dina headless-upplevelser med AEM 6.5.

## Ökning {#overview}

Universal Editor är en mångsidig visuell editor som ingår i Adobe Experience Manager Sites. Man kan redigera det man redan gjort - se-is-what-you-get (WYSIWYG) - oavsett headless Experience.

* Den universella redigeraren har stor flexibilitet eftersom den har stöd för samma enhetliga visuella redigering för alla former av AEM headless-innehåll.
* Utvecklarna drar nytta av universella redigerares mångsidighet eftersom den även stöder en sann frikoppling av implementeringen. Det gör att utvecklare kan utnyttja praktiskt taget vilket ramverk eller vilken arkitektur de vill utan att införa några begränsningar för SDK eller teknik.

Mer information finns i [AEM as a Cloud Service-dokumentationen för Universal Editor](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction).

## Arkitektur {#architecture}

Universell redigerare är en tjänst som fungerar tillsammans med AEM för att skapa innehåll utan problem.

* Den universella redigeraren finns på `https://experience.adobe.com/#/aem/editor/canvas` och du kan redigera sidor som återges av AEM 6.5.
* AEM-sidan läses av Universal Editor via dispatchern från AEM författarinstans.
* Universal Editor-tjänsten, som körs på samma värd som Dispatcher, skriver tillbaka ändringarna till AEM författarinstans.

![Författarflöde med den universella redigeraren](assets/author-flow.png)

## Krav {#requirements}

Universal Editor stöds av:

* AEM 6.5 LTS
   * Både lokala värdtjänster och AMS-värdtjänster stöds.
* [AEM 6.5](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/headless/universal-editor/introduction) (Service Pack 21 eller 22 plus ett funktionspaket)
   * Både lokala värdtjänster och AMS-värdtjänster stöds.
* [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction) (release `2023.8.13099` eller senare)

Det här dokumentet fokuserar på AEM 6.5 LTS-stöd i Universal Editor.

## Inställningar {#setup}

Om du vill testa den universella redigeraren måste du:

1. [Uppdatera och konfigurera din AEM-redigeringsinstans.](#update-configure-aem)
1. [Konfigurera en lokal Universal Editor-tjänst.](#set-up-ue)
1. [Justera din dispatcher så att den kan användas i den universella redigeringstjänsten.](#update-dispatcher)

När du har slutfört konfigurationen kan du [instrumentera dina program så att de använder den universella redigeraren.](#instrumentation)

### Uppdatera AEM {#update-aem}

Service Pack 21 eller 22 och ett funktionspaket för AEM krävs för att du ska kunna använda Universal Editor med AEM 6.5.

#### Använd senaste Service Pack {#latest}

Kontrollera att du kör minst Service Pack 21 eller 22 för AEM 6.5. Du kan hämta det senaste Service Pack-meddelandet från [Programvarudistribution.](https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html)

#### Installera Universal Editor Feature Pack {#feature-pack}

Installera **Universal Editor Feature Pack för AEM 6.5** [som finns tillgängligt för programdistribution.](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/cq-6.5.21-universal-editor-1.0.0.zip)

Om du redan kör Service Pack 23 eller senare är funktionspaketet inte nödvändigt.

### Konfigurera tjänster {#configure-services}

Funktionspaketet installerar ett antal nya paket för vilka ytterligare konfiguration krävs.

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

#### Konfigurera autentiseringshanteraren för Adobe Granite-frågeparametern. {#query-parameter}

1. Öppna Configuration Manager.
   * `http://<host>:<port>/system/console/configMgr`
1. Leta reda på **Autentiseringshanteraren för Adobe Granite-frågeparametern** i listan och klicka på **Redigera konfigurationsvärdena**.
1. I fältet **Sökväg** (`path`) lägger du till `/` för att aktivera.
   * Ett tomt värde inaktiverar autentiseringshanteraren.
1. Klicka på **Spara**.

#### Definiera för vilka innehållssökvägar eller `sling:resourceTypes` den universella redigeraren ska öppnas. {#paths}

1. Öppna Configuration Manager.
   * `http://<host>:<port>/system/console/configMgr`
1. Leta reda på **URL-tjänsten för den universella redigeraren** i listan och klicka på **Redigera konfigurationsvärdena**.
1. Definiera för vilka innehållssökvägar eller `sling:resourceTypes` den universella redigeraren ska öppnas.
   * Ange sökvägarna som den universella redigeraren öppnas för i fältet **Öppna mappning för den universella redigeraren**.
   * I fältet **Sling:resourceTypes, som ska öppnas av Universell redigerare**, anger du en lista över resurser som ska öppnas direkt av Universell redigerare.
1. Klicka på **Spara**.
1. Kontrollera din [externaliserarkonfiguration](/help/sites-developing/externalizer.md) och se till att du har minst den lokala miljön, författarmiljön och publiceringsmiljön inställd som i följande exempel.

   ```text
   "local $[env:AEM_EXTERNALIZER_LOCAL;default=http://localhost:4502]",
   "author $[env:AEM_EXTERNALIZER_AUTHOR;default=http://localhost:4502]",
   "publish $[env:AEM_EXTERNALIZER_PUBLISH;default=http://localhost:4503]"
   ```

När dessa konfigurationssteg är klara öppnas AEM Universell redigerare för sidor i följande ordning.

1. AEM kontrollerar mappningarna under `Universal Editor Opening Mapping` och om innehållet finns under sökvägar som definierats där öppnas Universell redigerare för det.
1. För innehåll som inte finns under sökvägar som definieras i `Universal Editor Opening Mapping`, kontrollerar AEM om `resourceType` för innehållet matchar de som definieras i **Sling:resourceTypes, som ska öppnas av Universal Editor**, och om innehållet matchar någon av dessa typer, öppnas Universell redigerare för det på `${author}${path}.html`.
1. I annat fall öppnas sidredigeraren.

Följande variabler är tillgängliga för att definiera dina mappningar under `Universal Editor Opening Mapping`.

* `path`: Innehållssökvägen för resursen som ska öppnas
* `localhost`: Post för externalisering för `localhost` utan schema, t.ex. `localhost:4502`
* `author`: Externalizer-post för författare utan schema, t.ex. `localhost:4502`
* `publish`: Post för externalisering för publicering utan schema, t.ex. `localhost:4503`
* `preview`: Externalizer-post för förhandsgranskning utan schema, t.ex. `localhost:4504`
* `env`: `prod`, `stage`, `dev` baserat på definierade körningslägen för Sling
* `token`: Frågetoken krävs för `QueryTokenAuthenticationHandler`

Exempelmappningar:

* Öppna alla sidor under `/content/foo` på AEM Author:
   * `/content/foo:${author}${path}.html?login-token=${token}`
   * Detta resulterar i att `https://localhost:4502/content/foo/x.html?login-token=<token>` öppnas
* Öppna alla sidor under `/content/bar` på en fjärr-NextJS-server, med alla variabler som information
   * `/content/bar:nextjs.server${path}?env=${env}&author=https://${author}&publish=https://${publish}&login-token=${token}`
   * Detta resulterar i att `https://nextjs.server/content/bar/x?env=prod&author=https://localhost:4502&publish=https://localhost:4503&login-token=<token>` öppnas

### Konfigurera Universal Editor-tjänsten {#set-up-ue}

Med AEM uppdaterat och konfigurerat kan du skapa en lokal universell redigeringstjänst för lokal utveckling och testning.

1. Installera Node.js version >=20.
1. Hämta och packa upp den senaste universella redigeringstjänsten från [Programvarudistribution](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home)
1. Konfigurera den universella redigeringstjänsten via miljövariabler eller filen `.env`.
   * [Mer information finns i dokumentationen för AEM as a Cloud Service Universal Editor.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/local-dev#setting-up-service)
   * Observera att du kan behöva använda alternativet `UES_MAPPING` om intern IP-omskrivning krävs.
1. Kör `universal-editor-service.cjs`

### Uppdatera Dispatcher {#update-dispatcher}

Om AEM är konfigurerat och en lokal Universal Editor-tjänst körs måste du tillåta en omvänd proxy för den nya tjänsten [ i dispatchern.](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)

1. Justera värdfilen för författarinstansen så att den innehåller en omvänd proxy.

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

Ditt program måste dock vara instrumenterat för att du ska kunna använda den universella redigeraren. Det innebär att du inkluderar metataggar för att instruera redigeraren hur och var innehållet ska sparas. Information om den här instrumenteringen finns i [Universal Editor-dokumentationen för AEM as a Cloud Service.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/getting-started#instrument-page)

Observera att följande dokumentation för Universal Editor med AEM as a Cloud Service gäller när du använder den med AEM 6.5.

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
>En utförlig guide till hur utvecklare kommer igång med Universal Editor finns i dokumentet [Universal Editor Overview for AEM Developers](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/developer-overview) i AEM as a Cloud Service-dokumentationen, med hänsyn tagen till de ändringar som krävs för stöd för AEM 6.5 enligt detta avsnitt.

## Skillnader mellan AEM 6.5 och AEM as a Cloud Service {#differences}

Universal Editor i AEM 6.5 fungerar ungefär på samma sätt som i AEM as a Cloud Service, inklusive användargränssnittet och mycket av installationsprogrammet. Det finns dock skillnader som bör noteras.

* Universal Editor i 6.5 stöder endast headless-användning.
* Inställningarna för den universella redigeraren varierar något för 6.5 ([enligt beskrivningen](#setup) i det aktuella dokumentet).
* Universal Editor i 6.5 använder en annan resursväljare och en annan Content Fragment-väljare än AEM as a Cloud Service.
