---
title: AEM Developer Tools for Eclipse
description: Läs om Adobe Experience Manager utvecklingsverktyg för Eclipse.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 2%

---

# AEM Developer Tools for Eclipse{#aem-developer-tools-for-eclipse}

![Cirkulärt bildmotiv för AEM Developer Tools för Eclipse.](do-not-localize/chlimage_1-9.png)

## Ökning {#overview}

&quot;AEM Developer Tools&quot; är en Eclipse-plugin som baseras på [Eclipse-pluginen för Apache Sling](https://sling.apache.org/documentation/development/ide-tooling.html) som släppts under Apache License 2.

Den har flera funktioner som underlättar utvecklingen av AEM:

* Smidig integrering med AEM-instanser via Eclipse Server Connector.
* Synkronisering för både innehåll och OSGI-paket.
* Felsökningsstöd med möjlighet att byta kod under drift.
* Enkelt Bootstrap för AEM-projekt med en särskild projektguide.
* Enkel redigering av JCR-egenskaper.

## Krav {#requirements}

Innan du använder AEM Developer Tools ska du göra följande:

* Hämta och installera [Eclipse IDE för Java™ EE-utvecklare](https://www.eclipse.org/downloads/packages/release/luna/r/eclipse-ide-java-ee-developers). AEM Developer Tools har för närvarande stöd för Eclipse Kepler eller senare

* Kan användas med AEM version 5.6.1 eller senare
* Konfigurera din förmörkande installation för att säkerställa att du har minst 1 GB stackminne genom att redigera konfigurationsfilen `eclipse.ini` enligt beskrivningen i [Vanliga frågor om Eclipse](https://wiki.eclipse.org/FAQ_How_do_I_increase_the_heap_size_available_to_Eclipse%3F).

>[!NOTE]
>
>I macOS högerklickar du på **Eclipse.app** och väljer sedan **Visa paketinnehåll** för att hitta `eclipse.ini`.

## Installera AEM Developer Tools för Eclipse {#how-to-install-the-aem-developer-tools-for-eclipse}

När du har uppfyllt [kraven](#requirements) ovan kan du installera plugin-programmet på följande sätt:

1. Gå till webbplatsen **AEM Developer Tools** på `https://eclipse.adobe.com/aem/dev-tools/`.

1. Kopiera **installationslänken**.

   Du kan även hämta ett arkiv i stället för att använda installationslänken. Om du gör det kan du installera offline, men du saknar meddelanden om automatiska uppdateringar.

1. Öppna menyn **Hjälp** i Eclipse.
1. Klicka på **Installera ny programvara**.
1. Klicka på **Lägg till..**.
1. I **Namn** skriver du AEM Developer Tools.
1. I **Plats** kopierar du installations-URL:en.
1. Klicka på **OK**.
1. Kontrollera både **AEM**- och **Sling**-plugin-program.
1. Klicka på **Nästa**.
1. Klicka på **Nästa**.
1. Acceptera de länkade avtalen och klicka på **Slutför**.
1. Klicka på **Ja** för att starta om Eclipse.

## Importera befintliga projekt {#how-to-import-existing-projects}

>[!NOTE]
>
>Se [Arbeta med ett paket i Eclipse när det hämtades från AEM](https://stackoverflow.com/questions/29699726/how-to-work-with-a-bundle-in-eclipse-when-it-was-downloaded-from-aem/29705407#29705407).

## AEM Perspective {#the-aem-perspective}

AEM utvecklingsverktyg för Eclipse har ett perspektiv som ger dig full kontroll över dina AEM-projekt och instanser.

![chlimage_1-2](assets/chlimage_1-2a.jpeg)

## Exempel på flermodulsprojekt {#sample-multi-module-project}

&quot;AEM Developer Tools&quot; innehåller ett exempel på ett flermodulsprojekt som hjälper dig att snabbt komma igång med projektkonfigurationen i Eclipse. Det är också en metodguide till flera AEM-funktioner. [Läs mer om projekttypen](https://github.com/adobe/aem-project-archetype).

Så här skapar du exempelprojektet:

1. Gå till avsnittet **AEM** på menyn **Arkiv** > **Nytt** > **Projekt** och välj **AEM Sample Multi-Module Project**.

   ![chlimage_1-69](assets/chlimage_1-69a.png)

1. Klicka på **Nästa**.

   >[!NOTE]
   >
   >Det här steget kan ta en stund eftersom m2eclipse måste skanna arkivtypskatalogerna.

   ![chlimage_1-70](assets/chlimage_1-70a.png)

1. Välj **com.adobe.granite.archietypes : sample-project-architype : (högsta antal)** på menyn och klicka sedan på **Nästa**.

   ![chlimage_1-71](assets/chlimage_1-71a.png)

1. Fyll i ett **namn**, **grupp-ID** och ett **artefakt-ID** för exempelprojektet. Du kan också välja att ange vissa avancerade egenskaper.

   ![chlimage_1-72](assets/chlimage_1-72a.png)

1. Konfigurera nu en AEM-server som Eclipse kan ansluta till.

   Om du vill använda felsökningsfunktionen måste du ha startat AEM i felsökningsläge, vilket du kan göra genom att lägga till följande på kommandoraden:

   ```
       -nofork -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=10123
   ```

   ![chlimage_1-73](assets/chlimage_1-73a.png)

1. Klicka på **Slutför**. Projektstrukturen skapas.

   >[!NOTE]
   >
   >I en ny installation (närmare bestämt: när större beroenden aldrig har hämtats) kan du få projektet skapat med fel. I det här fallet följer du proceduren som beskrivs i [Lösa ogiltig projektdefinition](#resolving-invalid-project-definition).

## Felsökning {#troubleshooting}

### Löser ogiltig projektdefinition {#resolving-invalid-project-definition}

Så här löser du ogiltiga beroenden och projektdefinitioner:

1. Markera alla skapade projekt.
1. Högerklicka. Välj **Uppdatera projekt** på menyn **Maven**.
1. Kontrollera **Tvinga uppdateringar av ögonblicksbild/släppningar**.
1. Klicka på **OK**. Eclipse försöker hämta nödvändiga beroenden.

### Aktivera automatisk komplettering av taggbibliotek i JSP-filer {#enabling-tag-library-autocompletion-in-jsp-files}

Automatisk komplettering av taggbibliotek går inte att utföra eftersom rätt beroenden läggs till i projektet. Det finns ett känt fel när du använder AEM Uber Jar, som inte innehåller de tld- och TagExtraInfo-filer som behövs.

Se till att artefakten org.apache.sling.scripting.jsp.taglib finns i klassökvägen före AEM Uber Jar för att undvika problemet. För Maven-projekt placerar du följande beroende i pom.xml före Uber Jar.

```xml
<dependency>
  <groupId>org.apache.sling</groupId>
  <artifactId>org.apache.sling.scripting.jsp.taglib</artifactId>
  <scope>provided</scope>
</dependency>
```

Se till att du lägger till rätt version för din distribution av AEM.

## Mer information {#more-information}

Den officiella versionen av Apache Sling IDE-verktygen för Eclipse-webbplatsen innehåller användbar information:

* [**Apache Sling IDE-verktygen för Eclipse** Användarhandbok](https://sling.apache.org/documentation/development/ide-tooling.html) hjälper dig igenom de övergripande begreppen, serverintegrationen och distributionsfunktionerna som stöds av AEM utvecklingsverktyg.
* Avsnittet [Felsökning](https://sling.apache.org/documentation/development/ide-tooling.html#troubleshooting).
* Listan [Kända fel](https://sling.apache.org/documentation/development/ide-tooling.html#known-issues).

Följande officiella [Eclipse](https://www.eclipse.org/)-dokumentation kan hjälpa dig att konfigurera miljön:

* [Komma igång med Eclipse](https://eclipseide.org/getting-started/)
* [Eclipse Luna Help System](https://help.eclipse.org/latest/index.jsp)
* [Maven Integration (m2eclipse)](https://www.eclipse.org/m2e/)
