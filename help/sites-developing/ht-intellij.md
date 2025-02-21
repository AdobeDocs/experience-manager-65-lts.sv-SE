---
title: Utveckla AEM-projekt med IntelliJ IDEA
description: Lär dig använda IntelliJ IDEA för att utveckla Adobe Experience Manager-projekt.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 1%

---

# Utveckla AEM-projekt med IntelliJ IDEA{#how-to-develop-aem-projects-using-intellij-idea}

## Ökning {#overview}

För att komma igång med AEM-utveckling på IntelliJ krävs följande steg.

Varje steg förklaras mer ingående i resten av detta avsnitt.

* Installera IntelliJ
* Konfigurera ditt AEM-projekt baserat på Maven
* Förbered JSP-stöd för IntelliJ i Maven POM
* Importera Maven Project till IntelliJ

>[!NOTE]
>
>Den här guiden bygger på IntelliJ IDEA Ultimate Edition 12.1.4 och AEM 5.6.1.

### Installera IntelliJ IDEA {#install-intellij-idea}

Hämta IntelliJ IDEA från [hämtningssidan på JetBrains](https://www.jetbrains.com/idea/download/).

Följ sedan installationsanvisningarna på den sidan.

### Konfigurera ditt AEM-projekt baserat på Maven {#set-up-your-aem-project-based-on-maven}

Konfigurera sedan projektet med Maven enligt beskrivningen i [Så här skapar du AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md).

Om du vill börja arbeta med AEM-projekt i IntelliJ IDEA räcker grundinställningen i [Komma igång om 5 minuter](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html).

### Förbered JSP-stöd för IntelliJ IDEA {#prepare-jsp-support-for-intellij-idea}

IntelliJ IDEA kan även ge stöd vid arbete med JSP, till exempel:

* automatisk komplettering av taggbibliotek
* medvetenhet om objekt som definierats av `<cq:defineObjects />` och `<sling:defineObjects />`

För att det ska fungera följer du anvisningarna i [Så här fungerar du med JSP:er](/help/sites-developing/ht-projects-maven.md#how-to-work-with-jsps) i [Så här skapar du AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md).

### Importera projektet Maven {#import-the-maven-project}

1. Öppna dialogrutan **Importera** i IntelliJ IDEA av

   * markera **Importera projekt** på välkomstskärmen om du inte har något projekt öppet än
   * välja **Arkiv > Importera projekt** på huvudmenyn

1. Välj POM-filen för projektet i dialogrutan Importera.

   ![chlimage_1-45](assets/chlimage_1-45a.png)

1. Fortsätt med standardinställningarna enligt dialogrutan nedan.

   ![chlimage_1-46](assets/chlimage_1-46a.png)

1. Fortsätt genom följande dialogrutor genom att klicka på **Nästa** och **Slutför**.
1. Du är nu konfigurerad för AEM Development med IntelliJ IDEA

   ![chlimage_1-47](assets/chlimage_1-47a.png)

### Felsöka JSP:er med IntelliJ IDEA {#debugging-jsps-with-intellij-idea}

Följande steg är nödvändiga för att felsöka JSP:er med IntelliJ IDEA

* Konfigurera en webbaspekt i projektet
* Installera JSR45 support-plugin
* Konfigurera en felsökningsprofil
* Konfigurera AEM för felsökningsläge

#### Konfigurera en webbaspekt i projektet {#set-up-a-web-facet-in-the-project}

IntelliJ IDEA måste förstå var JSP:er för felsökning ska hittas. Eftersom IDEA inte kan tolka inställningarna för `content-package-maven-plugin` måste de konfigureras manuellt.

1. Gå till **Arkiv > Projektstruktur**
1. Markera modulen **Innehåll**
1. Klicka på **+** ovanför listan med moduler och välj **Webb**
1. Som webbresurskatalog väljer du `content/src/main/content/jcr_root subdirectory` för ditt projekt enligt skärmbilden nedan.

![chlimage_1-48](assets/chlimage_1-48a.png)

#### Installera JSR45 support-plugin {#install-the-jsr-support-plugin}

1. Gå till rutan **Plugins** i IntelliJ IDEA-inställningarna
1. Navigera till plugin-programmet **JSR45-integrering** och markera kryssrutan bredvid det
1. Klicka på **Använd**
1. Starta om IntelliJ IDEA när du ombeds att

![chlimage_1-49](assets/chlimage_1-49a.png)

#### Konfigurera en felsökningsprofil {#configure-a-debug-profile}

1. Gå till **Kör > Redigera konfigurationer**
1. Tryck på **+** och välj **JSR45 Remote**
1. I konfigurationsdialogrutan väljer du **Konfigurera** bredvid **Programserver** och konfigurerar en allmän server
1. Ange en lämplig URL för startsidan om du vill öppna en webbläsare när du startar felsökningen
1. Ta bort alla **Före start**-uppgifter om du använder VLT-autosynkronisering, eller konfigurera lämpliga Maven-uppgifter om du inte gör det
1. Justera porten om det behövs i rutan **Start/Anslutning**
1. Kopiera de kommandoradsargument som IntelliJ IDEA föreslår

![chlimage_1-50](assets/chlimage_1-50a.png) ![chlimage_1-51](assets/chlimage_1-51a.png)

#### Konfigurera AEM för felsökningsläge {#configure-aem-for-debug-mode}

Det sista steget är att starta AEM med de JVM-alternativ som IntelliJ IDEA föreslår.

Starta AEM jar-filen direkt och lägg till dessa alternativ med följande kommandorad:

`java -Xdebug -Xrunjdwp:transport=dt_socket,address=58242,suspend=n,server=y -Xmx1024m -jar cq-quickstart-6.5.0.jar`

Du kan också lägga till dessa alternativ i startskriptet i `crx-quickstart/bin/start` enligt nedan.

```shell
# ...

# default JVM options
if [ -z "$CQ_JVM_OPTS" ]; then
 CQ_JVM_OPTS='-server -Xmx1024m -Djava.awt.headless=true'
fi

CQ_JVM_OPTS="$CQ_JVM_OPTS -Xdebug -Xrunjdwp:transport=dt_socket,address=58242,suspend=n,server=y"

# ...
```

#### Starta felsökning {#start-debugging}

Du är nu redo att felsöka JSP:er i AEM.

1. Välj **Kör > Felsök > Felsökningsprofil**
1. Ange brytpunkter i komponentkoden
1. Öppna en sida i webbläsaren

![chlimage_1-52](assets/chlimage_1-52a.png)

### Felsökningspaket med IntelliJ IDEA {#debugging-bundles-with-intellij-idea}

Kod i paket kan felsökas med en allmän fjärrfelsökningsanslutning som standard. Du kan följa [Jetbrain-dokumentationen för fjärrfelsökning](https://www.jetbrains.com/help/idea/remote-debugging-with-product.html#remote-interpreter).
