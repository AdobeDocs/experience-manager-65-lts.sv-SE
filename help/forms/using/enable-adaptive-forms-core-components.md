---
title: Hur aktiverar man adaptiva Forms Core-komponenter i AEM 6.5 Forms?
description: Stegvisa anvisningar som hjälper dig att aktivera adaptiva Forms Core-komponenter i en AEM 6.5 Forms-miljö.
keywords: Aktivera kärnkomponenter, kärnkomponenter adaptiva Forms, kärnkomponenter i 6.5, adaptiva Forms Core-komponenter i AEM 6.5, AF Core-komponenter i AEM 6.5, AEM 6.5 Forms Core-komponenter
role: Admin, Developer
feature: Adaptive Forms,Core Components
solution: Experience Manager, Experience Manager Forms
exl-id: a163598d-0a6e-45a8-b3b2-1f260007952b
source-git-commit: f21858421f2e84643fab1add5116b033cb6dd122
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# Aktivera adaptiva Forms Core-komponenter i AEM 6.5 Forms {#enable-adaptive-forms-core-components}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/setup-configure-migrate/enable-adaptive-forms-core-components.html) |
| AEM 6.5 | Denna artikel |

<!--**Applies to:** ✅ Adaptive Form Core Components ❎ [Adaptive Form Foundation Components](/help/forms/using/create-adaptive-form.md).-->

Om du aktiverar adaptiva Forms Core-komponenter kan du börja skapa, publicera och leverera [Core-baserade adaptiva Forms](create-an-adaptive-form-core-components.md) och [Headless Adaptive Forms](https://experienceleague.adobe.com/docs/experience-manager-headless-adaptive-forms/using/overview.html) från din AEM 6.5 Forms-miljö.

Om du vill aktivera adaptiva Forms Core-komponenter i din AEM 6.5 Forms-miljö ska du konfigurera och distribuera ett [AEM Archetype 41 eller senare](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)-baserat projekt (med formuläralternativ aktiverade) på alla dina författarinstanser och publiceringsinstanser.

Den här artikeln innehåller detaljerade anvisningar om hur du konfigurerar och distribuerar AEM Archetype 4.1 eller senare baserat på ditt AEM 6.5 Forms-projekt för att aktivera adaptiva Forms Core-komponenter. Du kan läsa listan nedan för **AEM 6.5**-kompatibla versioner för att aktivera Forms Core-komponenter:

## Förutsättningar {#prerequisites}

Innan du aktiverar adaptiva Forms Core-komponenter i en AEM 6.5 LTS Forms-miljö:

* Installera den senaste versionen av [Apache Maven](https://maven.apache.org/download.cgi).

* Installera en vanlig textredigerare. Exempel: Microsoft Visual Studio Code.

## Skapa och driftsätt de senaste AEM Archetype-baserade projekten

Så här skapar du ett AEM Archetype 41- eller [senare](https://github.com/adobe/aem-project-archetype)-baserat projekt och distribuerar det till alla dina Author- och Publish-instanser:

1. Logga in på datorn som värd och kör AEM 6.5 Forms-instansen som administratör.
1. Öppna kommandotolken eller avsluta och kör följande kommando för att skapa AEM Archetype-projekt (med formuläralternativ aktiverade):

   * Microsoft Windows

   ```Shell
      mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate ^
      -D archetypeGroupId=com.adobe.aem ^
      -D archetypeArtifactId=aem-project-archetype ^
      -D archetypeVersion=41 ^
      -D appTitle="My Form" ^
      -D appId="myform" ^
      -D groupId="com.myform" ^
      -D includeFormsenrollment="y" ^
      -D aemVersion="6.5.15" 
   ```

   * Linux eller Apple macOS

   ```Shell
      mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
      -D archetypeGroupId=com.adobe.aem \
      -D archetypeArtifactId=aem-project-archetype \
      -D archetypeVersion=41 \
      -D appTitle="My Form" \
      -D appId="myform" \
      -D groupId="com.myform" \
      -D includeFormsenrollment="y" \
      -D aemVersion="6.5.15" 
   ```

   Tänk på följande när du kör kommandot ovan:

   * Ändra inte värdet för egenskapen `aemVersion` från `6.5.15.0` till något annat.

   * Ange egenskapen `archetypeVersion` till `41` eller senare. Den senaste versionen finns i avsnittet om systemkrav i dokumentationen för [AEM Project Archetype](https://github.com/adobe/aem-project-archetype).

   * Uppdatera kommandot så att det återspeglar de specifika värdena för miljön, inklusive `appTitle`, `appId` och `groupId`. Ange också värdet `includeFormsenrollment` för egenskapen `y`. Om du använder Forms Portal ska du ställa in alternativet `includeExamples=y` så att Forms Portal Core Components inkluderas i ditt projekt.


1. (Endast för Arketype version 4.1-baserade projekt) När AEM Archetype-projektet har skapats kan du aktivera teman för Core Components-baserade Adaptive Forms. Så här aktiverar du teman:

   1. Öppna [projektmappen AEM Archetype]/ui.apps/src/main/content/jcr_root/apps/__appId__/components/adaptiveForm/page/customheaderlibs.html för redigering:

   1. Lägg till följande kod på rad 21:

      ```XML
      <sly data-sly-use.clientlib="core/wcm/components/commons/v1/templates/clientlib.html"
      data-sly-use.formstructparser="com.adobe.cq.forms.core.components.models.form.FormStructureParser"
      data-sly-test.themeClientLibRef="${formstructparser.themeClientLibRefFromFormContainer}">
      <sly data-sly-test="${themeClientLibRef}" data-sly-call="${clientlib.css @ categories=themeClientLibRef}"/>
      </sly>
      ```

      ![Lägg till ovanstående kod på rad 21](/help/forms/using/assets/code-to-enable-themes.png)

   1. Spara och stäng filen.

1. Uppdatera projektet så att det innehåller den senaste versionen av Forms Core Components:

   1. Öppna [projektmappen AEM Archetype]/pom.xml för redigering.
   1. Ange version av `core.forms.components.version` och `core.forms.components.af.version` till den [senaste Forms Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/version.html#aem-as-form-version-history)-versionen och se till att båda har samma version som **Forms Core Components** som nämns i tabellen, och ange version av `core.wcm.components.version` enligt [WCM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/versions.html) .

      >[!WARNING]
      >
      >* När du skapar ett Arketype-projekt med version 45 ställer `[AEM Archetype Project Folder]/pom.xml` först in versionen för formulärets kärnkomponenter till 1.1.28. Innan du bygger eller driftsätter Archetype-projektet ska du uppdatera formulärkärnkomponentens version till 1.1.26. Den senaste versionen finns i [ AEM 6.5 Forms-versionshistoriken ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/version.html#aem-as-form-version-history) .

      >[!NOTE]
      >
      >* Om du konfigurerar någon annan topologi måste du lägga till webbadresserna för Skicka, Förifyll och andra till tillåtelselista i Dispatcher-lagret.

   1. Spara och stäng filen.


1. När AEM Archetype-projektet har skapats kan du skapa distributionspaketet för din miljö. Så här skapar du paketet:

   1. Navigera till rotkatalogen för ditt AEM Archetype-projekt.

   1. Kör följande kommando för att skapa AEM Archetype-projektet för din miljö:

      ```Shell
      mvn clean install
      ```

      ![archietypebuild-success](/help/forms/using/assets/corecomponent-build-successful.png)


   När AEM Archetype-projektet har byggts skapas ett AEM Package. Paketet finns i [AEM Archetype Project Folder]\all\target\[appid].all-[version].zip

1. Använd [Package Manager](/help/sites-administering/package-manager.md) för att distribuera [AEM Archetype Project Folder]\all\target\[appid].all-[version].zip-paketet på alla Author- och Publish-instanser.

>[!NOTE]
>
>
>
> * Om du får problem med att komma åt inloggningsdialogrutan på en publiceringsinstans kan du försöka att logga in med URL:en `http://[Publish Server URL]:[PORT]/system/console` för att installera paketet via pakethanteraren. På så sätt kan du komma åt inloggningssidan i en Publish-instans och fortsätta med installationsprocessen.
> * Ta inte bort eller ignorera Arketype-projektet efter att du distribuerat det till din miljö. Arketype-projektet krävs för att du ska kunna lägga till anpassade och nya adaptiva Forms Core Components-teman i din miljö.

Kärnkomponenterna är aktiverade för din miljö. En tom Core Components-baserad Adaptive Form-mall och Canvas 3.0-tema distribueras till din miljö, vilket gör att du kan [skapa Core Components-baserade Adaptive Forms](create-an-adaptive-form-core-components.md).

## Vanliga frågor

### Vad är kärnkomponenter?

[Kärnkomponenterna](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) är en uppsättning standardiserade WCM-komponenter (Web Content Management) för AEM som snabbar upp utvecklingstiden och minskar underhållskostnaderna för dina webbplatser.

### Vad finns det för funktioner för att aktivera kärnkomponenter?


När de adaptiva Forms Core-komponenterna är aktiverade för din miljö läggs en tom Core Components-baserad Adaptive Form-mall och Canvas 3.0-tema till i din miljö. När du har aktiverat adaptiva Forms Core-komponenter för din miljö kan du:

* Skapa grundkomponentbaserade adaptiva Forms.
* Skapa grundkomponentbaserade adaptiva formulärmallar.
* Skapa egna teman för grundkomponentbaserade adaptiva formulärmallar.
* Servera Core Component-baserade Adaptive Form JSON-representationer för kanaler som mobiler, webben, inbyggda appar och tjänster som kräver att ett formulär visas utan rubrik.

## What&#39;s Next

* [Skapa en grundkomponentbaserad adaptiv form](/help/forms/using/create-an-adaptive-form-core-components.md)
* [Skapa eller lägga till ett anpassat formulär på en AEM Sites-sida eller ett Experience Fragment](create-or-add-an-adaptive-form-to-aem-sites-page.md)
* [Skapa teman för grundkomponentbaserade adaptiva Forms](create-or-customize-themes-for-adaptive-forms-core-components.md)
* [Skapa en mall för Core Components-baserade Adaptive Forms](template-editor.md)
