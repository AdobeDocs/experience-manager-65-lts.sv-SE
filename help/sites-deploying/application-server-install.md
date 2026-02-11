---
title: Installation av programserver
description: Lär dig hur du installerar Adobe Experience Manager med en programserver.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: deploying
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
exl-id: 09d54b52-485a-453c-a2d0-535adead9e6c
source-git-commit: 1d0fe9ee81a2e38f7887b6f884a13d1ba1462304
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 0%

---

# Installation av programserver{#application-server-install}

>[!NOTE]
>
>`JAR` och `WAR` är de filtyper som Adobe Experience Manager (AEM) släpps i. Dessa format genomgår kvalitetssäkring för de supportnivåer som Adobe har åtagit sig.
>

I det här avsnittet beskrivs hur du installerar Adobe Experience Manager (AEM) med en programserver. Gå till avsnittet [Plattformar som stöds](/help/sites-deploying/technical-requirements.md#servlet-engines-application-servers) om du vill läsa mer om de specifika supportnivåer som finns för de enskilda programservrarna.

Installationsstegen för följande programservrar beskrivs:

* [WebSphere](#websphere)
* [Tomcat 10.0.x/10.1.x](#tomcat)
* [JBoss EAP 8](/help/forms/using/upgrade-forms-osgi.md)

Mer information om hur du installerar webbprogram, serverkonfigurationer och hur du startar och stoppar servern finns i dokumentationen för respektive programserver.

<!-- >[!NOTE]
>
>If you are using Dynamic Media in a WAR deployment, see [Dynamic Media documentation](/help/assets/config-dynamic.md#enabling-dynamic-media). -->

## Allmän beskrivning {#general-description}

### Standardbeteende vid installation av AEM i en programserver {#default-behaviour-when-installing-aem-in-an-application-server}

AEM levereras som en enda krigsfil att driftsätta.

Om den distribueras händer följande som standard:

* Körningsläget är `author`
* Instansen (Repository, Felix OSGI environment, bundles, osv.) är installerad i `${user.dir}/crx-quickstart`, där `${user.dir}` är den aktuella arbetskatalogen. Den här sökvägen till crx-quickstart kallas `sling.home`

* Kontextroten är krigsfilens namn. Exempel: `aem-65-lts`.

#### Konfiguration {#configuration}

Du kan ändra standardbeteendet på följande sätt:

* körningsläge: konfigurera parametern `sling.run.modes` i filen `WEB-INF/web.xml` i AEM krigsfil före distribution

* sling.home: konfigurera parametern `sling.home` i filen `WEB-INF/web.xml` i AEM-krigsfilen före distributionen

* kontextrot: ändra namn på AEM krigsfil

#### Publicera installation {#publish-installation}

Om du vill distribuera en publiceringsinstans måste du ange att körningsläget ska publiceras:

* Packa upp `WEB-INF/web.xml` från AEM krigsfil
* Ändra parametern `sling.run.modes` till publicering
* Repetera filen `web.xml` i AEM krigsfil
* Distribuera AEM krigsfil

#### Installationskontroll {#installation-check}

Om du vill kontrollera om allt är installerat kan du:

* svansen för filen `error.log` för att se att allt innehåll är installerat
* kontrollera i `/system/console` att alla paket är installerade

#### Två instanser på samma programserver {#two-instances-on-the-same-application-server}

I demonstrationssyfte kan det vara lämpligt att installera både författare och publiceringsinstans på en programserver. För att uppnå det måste du:

1. Ändra variabeln `sling.home` och variabeln `sling.run.modes` i publiceringsinstansen
1. Packa upp filen `WEB-INF/web.xml` från AEM krigsfil
1. Ändra parametern `sling.home` till en annan sökväg (absoluta och relativa sökvägar är möjliga)
1. Ändra `sling.run.modes` till `publish` för publiceringsinstansen
1. Svara på filen `web.xml`
1. Byt namn på krigsfilerna så att de har olika namn. Byt till exempel namn på det ena till `aemauthor.war` och det andra till `aempublish.war`
1. Använd högre minnesinställningar. AEM-standardinstanser använder till exempel `-Xmx3072m`
1. Distribuera de två webbprogrammen
1. Stoppa de två webbprogrammen efter distributionen
1. Kontrollera att egenskapen `sling.properties` är inställd på `felix.service.urlhandlers` i filen `false` i både författare- och publiceringsinstanser. (Standardinställningen är att den är inställd på `true`).
1. Starta de två webbprogrammen igen.

## Installationsprocedurer för programservrar {#application-servers-installation-procedures}

### WebSphere® 24.0.0.7 {#websphere}

Läs [Allmän beskrivning](#general-description) ovan före distributionen.

**Serverförberedelse**

* Låt Basic Auth Headers gå igenom:

   * Ett sätt att låta AEM autentisera en användare är att inaktivera den globala administrativa säkerheten för WebSphere®-servern. Om du vill göra det går du till **Säkerhet > Global säkerhet** och avmarkerar kryssrutan **Aktivera administrativ säkerhet**, sparar och startar om servern.

* Ange `"JAVA_OPTS= -Xmx2048m"`
* Om du vill installera AEM med kontextroten = /, ändrar du kontextroten för det befintliga standardwebbprogrammet.

**Distribuera AEM webbprogram**

* Ladda ned AEM krigsfil
* Gör dina konfigurationer i filen `web.xml` om det behövs. Mer information finns i [Allmän beskrivning](#general-description) ovan.

   * Packa upp filen `WEB-INF/web.xml`
   * Ändra parametern `sling.run.modes` till `publish`
   * Avkommentera den inledande `sling.home`-parametern och ange den här sökvägen efter behov
   * Upprepa filen `web.xml`.

* Distribuera AEM krigsfil

   * Välj en kontextrot. Om du vill ange de körningslägen för sling som du behöver för att välja de detaljerade stegen i distributionsguiden anger du dem i steg 6 i guiden.

* Starta AEM webbprogram

#### Tomcat 10.0.x/10.1.x {#tomcat}

Läs [Allmän beskrivning](#general-description) ovan före en distribution.

* **Förbered Tomcat Server**

   * Öka inställningarna för virtuellt minne:

      * I `bin/catalina.bat` (svara `catalina.sh` på UNIX®) lägger du till följande inställning:

        ```
        set "JAVA_OPTS= -Xmx2048m`
        ```

   * Tomcat aktiverar inte administratörs- eller hanteraråtkomst vid installationen. Därför måste du redigera `tomcat-users.xml` manuellt för att tillåta åtkomst för dessa konton:

      * Redigera `tomcat-users.xml` om du vill inkludera åtkomst för administratör och hanterare. Konfigurationen bör se ut ungefär som i följande exempel:

        ```xml
        <?xml version='1.0' encoding='utf-8'?>
        <tomcat-users>
          <role rolename="manager"/>
          <role rolename="tomcat"/>
          <role rolename="admin"/>
          <role rolename="role1"/>
          <role rolename="manager-gui"/>
          <user username="both" password="tomcat" roles="tomcat,role1"/>
          <user username="tomcat" password="tomcat" roles="tomcat"/>
          <user username="admin" password="admin" roles="admin,manager-gui"/>
          <user username="role1" password="tomcat" roles="role1"/>
        </tomcat-users>
        ```

   * Om du vill distribuera AEM med kontextroten &quot;/&quot; måste du ändra kontextroten för den befintliga ROOT-webbappen:

      * Stoppa och avdistribuera ROOT-webbprogrammet
      * Byt namn på mappen `ROOT.war` i Tomcat-mappen för webbprogram
      * Starta webbprogrammet igen

   * Om du installerar AEM webbprogram med hjälp av hanteraren-gui måste du öka den maximala storleken för en överförd fil, eftersom standardinställningen endast tillåter 50 MB uppladdningsstorlek. För att lyckas med att öppna `web.xml` för hanterarens webbprogram:

     `webapps/manager/WEB-INF/web.xml`

     och öka `max-file-size` och `max-request-size` till minst 500 MB. Se följande `multipart-config` i en exempelfil `web.xml` nedan:

     ```xml
     <multipart-config>
     <!-- 500MB max -->
     <max-file-size>524288000</max-file-size>
     <max-request-size>524288000</max-request-size>
     <file-size-threshold>0</file-size-threshold>
     </multipart-config>
     ```

* **Distribuera AEM webbprogram**

   * Ladda ned AEM krigsfil.
   * Gör dina konfigurationer i filen `web.xml` om det behövs.

      * Packa upp filen `WEB-INF/web.xml`
      * Ändra parametern `sling.run.modes` till `publish`
      * Avkommentera den inledande `sling.home`-parametern och ange den här sökvägen efter behov
      * Upprepa filen `web.xml`.

   * Byt namn på AEM krigsfil till `ROOT.war` om du vill distribuera den som en rotwebbapp. Byt namn på den till `aemauthor.war` om du vill ha `aemauthor` som kontextrot.
   * Kopiera den till Tomcat&#39;s webapps folder
   * Vänta tills AEM har installerats.
