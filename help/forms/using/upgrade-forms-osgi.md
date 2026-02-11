---
title: Uppgradera till AEM 6.5 Forms LTS i OSGi
description: Du kan utföra en direkt uppgradering från AEM 6.5.22.0 Forms till AEM 6.5 Forms LTS.
content-type: reference
role: Admin, User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms, AEM Forms on OSGi, AEM Forms Upgrade
exl-id: 9233d4b7-441c-4cbd-86f8-2c52b99c3330
source-git-commit: b7aa877f9e782b0568adc7baa440dc630c690454
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 0%

---

# Uppgradera till AEM 6.5 Forms LTS i OSGi {#upgrade-to-aem-forms-osgi}

Om du vill [uppgradera från AEM 6.5 till AEM 6.5 LTS](/help/sites-deploying/upgrade.md) uppgraderar du till AEM 6.5.22.0 Forms eller senare. En direkt uppgradering från AEM 6.5.22.0 till AEM 6.5 Forms LTS stöds.

Om du använder AEM 6.0 Forms, AEM 6.1 Forms, AEM 6.2 Forms, AEM 6.3 Forms, AEM 6.4 Forms eller AEM 6.5 Forms finns det ingen direkt uppgradering till AEM 6.5 Forms LTS. Detaljerade uppgraderingsalternativ finns i dokumentationen för [uppgraderingssökvägar](/help/forms/using/upgrade.md).

När du har uppgraderat till Service Pack AEM Forms 6.5.22.0 följer du de här stegen för att uppgradera till AEM 6.5 LTS Forms:

1. Installera AEM Forms tilläggspaket. Stegen listas nedan:

   1. Öppna [Programvarudistribution](https://experience.adobe.com/downloads). Du behöver en Adobe ID för att logga in på Software Distribution.
   1. Välj **[!UICONTROL Adobe Experience Manager]** som finns på rubrikmenyn.
   1. I avsnittet **[!UICONTROL Filters]**:
      1. Välj **[!UICONTROL Forms]** i listrutan **[!UICONTROL Solution]**.
      1. Välj version och typ för paketet. Du kan också använda alternativet **[!UICONTROL Search Downloads]** för att filtrera resultaten.
   1. Välj det paketnamn som gäller för ditt operativsystem, välj **[!UICONTROL Accept EULA Terms]** och välj **[!UICONTROL Download]**.
   1. Öppna [Pakethanteraren](/help/sites-administering/package-manager.md) och klicka på **[!UICONTROL Upload Package]** för att överföra paketet.
   1. Markera paketet och klicka på **[!UICONTROL Install]**.

      Du kan också hämta paketet med hjälp av den direktlänk som finns i artikeln [AEM Forms-utgåvor](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases) .

      När paketet har installerats uppmanas du att starta om AEM-instansen. **Stoppa inte servern omedelbart.** Innan du stoppar AEM Forms-servern väntar du tills meddelandena ServiceEvent REGISTERED och ServiceEvent UNREGISTERED inte visas i filen &lt;crx-database>/error.log och loggen är stabil. Observera också att ett fåtal paket kan vara i installerat läge. Du kan ignorera dessa paketen.


      **Starta om AEM-instansen med följande JVM-kommandoradsparametrar**:
      `--add-opens java.base/java.util=ALL-UNNAMED --add-exports=java.xml/com.sun.org.apache.xml.internal.serialize=ALL-UNNAMED`

      Om servern startas via ett skript eller en tjänst ska du uppdatera den så att den innehåller ovanstående så att de även gäller efter efterföljande omstarter.

      >[!NOTE]
      >
      > Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

1. Utför efterinstallationsaktiviteter.

   * **Kör migreringsverktyg**

     Migreringsverktyget gör att anpassningsbara formulär och korrespondenshanteringsresurser i tidigare versioner blir kompatibla med AEM 6.5-formulär. Du kan hämta verktyget från AEM Software Distribution. Stegvis information om hur du konfigurerar och använder migreringsverktyget finns i [migreringsverktyget](../../forms/using/migration-utility.md).

     Om du använder [Exempel för att integrera utkast och inskickskomponent](https://helpx.adobe.com/experience-manager/6-3/forms/using/integrate-draft-submission-database.html) med databasen och uppgradera från en tidigare version kör du följande SQL-frågor när du har utfört uppgraderingen:

     ```sql
     UPDATE metadata m, additionalmetadatatable am
     SET m.dataType = am.value
     WHERE m.id = am.id
     AND am.key = 'dataType'
     ```

     ```sql
     DELETE from additionalmetadatatable
     WHERE `key` = 'dataType'
     ```

   * **(Om du uppgraderar från AEM 6.2 Forms eller tidigare versioner) Konfigurera om Adobe Sign**

     Om du har konfigurerat Adobe Sign i den tidigare versionen av AEM Forms konfigurerar du om Adobe Sign från AEM Cloud-tjänster. Mer information finns i [Integrera Adobe Sign med AEM Forms](../../forms/using/adobe-sign-integration-adaptive-forms.md).

   * **Stöd för jQuery**

     I AEM 6.5 Forms uppdateras jQuery till 3.2.1 och jQuery UI-versionen uppdateras till 1.12.1. AEM Form använder JQuery i **noConflict** -läge. Om du använder någon annan jQuery-version visas inga problem när du uppgraderar. När du uppgraderar till AEM 6.5 Forms:

      * Kontrollera att eventuella anpassade komponenter är kompatibla med jQuery-versioner som stöds.
      * Ta bort API:er som inte stöds från anpassade komponenter. Se [uppgraderingsguiden](https://jquery.com/upgrade-guide/3.0/) för en lista över borttagna API:er. Stöd för API:erna load(), .unload() och .error() har tagits bort. Använd metoden .on() i stället för tidigare nämnda API:er. Ändra till exempel $(&quot;img&quot;).load(fn) till $(&quot;img&quot;).on(&quot;load&quot;, fn).

   * **(Om du uppgraderar från AEM 6.2 Forms eller tidigare versioner) Konfigurera om analyser och rapporter**

     I AEM 6.4 Forms är trafikvariabeln source och success event för intryckt inte tillgänglig. Så när du uppgraderar från AEM 6.2 Forms eller tidigare versioner slutar AEM Forms skicka data till Adobe Analytics server och analysrapporter för adaptiva formulär är inte tillgängliga. Dessutom introducerar AEM 6.4 Forms trafikvariabler för den version av formuläranalysen och händelsen success för den tid som läggs på ett fält. Så konfigurera om analyser och rapporter för er AEM Forms-miljö. Mer information finns i [Konfigurera analyser och rapporter](../../forms/using/configure-analytics-forms-documents.md).

1. Kontrollera att servern har uppgraderats, att alla data har migrerats och att den fungerar som vanligt.

   * **Verifiera paketens status:** Kontrollera att alla paket är i aktivt läge.
   * **Verifiera replikering och omvänd replikering:** Publicera, fyll i och skicka några migrerade formulär. Verifiera också skickade data.
   * **Verifiera åtkomst till användargränssnitt för administratörer och utvecklare:** Logga in på en AEM-instans från ett administratörskonto och verifiera att du har åtkomst till följande URL:er:

      * `https://'[server]:[port]'/crx/packmgr`
      * `https://'[server]:[port]'/crx/de`
      * `https://'[server]:[port]'/aem/forms.html/content/dam/formsanddocuments`

   >[!NOTE]
   >
   >I AEM 6.4 Forms har strukturen för crx-database ändrats. Om du uppgraderar från 6.3 Forms till AEM 6.5 Forms kan du använda de nya anpassningsmöjligheterna som du skapar på nytt. En fullständig lista över ändrade sökvägar finns i [Omstrukturering av Forms-databaser i AEM](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/deploying/restructuring/forms-repository-restructuring-in-aem-6-5).


## Distribuera AEM på JBoss EAP 8 (Windows)

### Ökning

Den här guiden innehåller steg-för-steg-instruktioner för att distribuera Adobe Experience Manager (AEM) som en fristående OSGi WAR-fil på JBoss Enterprise Application Platform (EAP) 8 i en Windows-miljö med JDK 21.

### Systemkrav

Innan du börjar distribuera bör du kontrollera att miljön uppfyller följande krav:

| Komponent | Krav |
|-----------|-------------|
| Operativsystem | Windows Server 2016 eller senare (64-bitars) |
| Java Development Kit | JDK 21 (Oracle eller OpenJDK) |
| Programserver | JBoss EAP 8.x |
| AEM Distribution | AEM WAR-fil (hämtas från Adobe) |

>[!NOTE]
>
> Kontrollera att miljövariabeln `JAVA_HOME` pekar på installationskatalogen för JDK 21.

### Steg 1: Installera JBoss EAP 8

#### Hämta JBoss EAP

1. Gå till Red Hat Developer Portal:\
   [https://developers.redhat.com/products/eap/download](https://developers.redhat.com/products/eap/download)

2. Ladda ned JBoss EAP 8 ZIP-distributionen för Windows.

#### Extrahera JBoss EAP

1. Extrahera den hämtade ZIP-filen till den installationskatalog du föredrar.

2. Observera den här katalogsökvägen som `<JBOSS_HOME>` som kan användas i den här guiden.

   **Exempel:**\
   ```C:\jboss-eap-8.0```

### Steg 2: Förbered AEM WAR-filen

#### Hämta AEM WAR

Hämta AEM WAR-filen från Adobe Software Distribution eller från din Adobe-representant.

#### Byt namn på WAR-fil

Byt namn på WAR-filen så att den återspeglar den önskade URL-kontextsökvägen:

```
cq-quickstart.war
```

>[!IMPORTANT]
>
> WAR-filnamnet avgör programmets URL-kontext. `cq-quickstart.war` kan till exempel nås på `/cq-quickstart`.


### Steg 3: Konfigurera AEM WAR

Alla konfigurationsändringar måste slutföras **före**-distributionen till JBoss.

#### Skapa arbetskatalog

1. Skapa en tillfällig arbetskatalog:

   ```
   C:\aem\war-config
   ```

2. Kopiera `cq-quickstart.war` till den här katalogen.

#### Extrahera WAR-innehåll

1. Öppna **Kommandotolken** och navigera till din arbetskatalog:

   ```cmd
   cd C:\aem\war-config
   ```

2. Extrahera WAR-filen:

   ```cmd
   jar -xvf cq-quickstart.war
   ```

   Detta skapar en katalogstruktur med `WEB-INF` och andra programfiler.

### Steg 4: Konfigurera JBoss-distributionsbeskrivare

#### Skapa distributionsstrukturfil

1. Navigera till katalogen `WEB-INF` i din extraherade WAR:

   ```cmd
   cd WEB-INF
   ```

2. Skapa en ny fil med namnet `jboss-deployment-structure.xml`.

3. Lägg till följande XML-innehåll:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jboss-deployment-structure xmlns="urn:jboss:deployment-structure:1.2">
       <deployment>
           <dependencies>
               <module name="jdk.unsupported" />
           </dependencies>
       </deployment>
   </jboss-deployment-structure>
   ```

4. Spara och stäng filen.

**Syfte:** Den här konfigurationen ger åtkomst till interna JDK-moduler som krävs av AEM.

### Steg 5: Konfigurera inställningar för multipart-överföring

>[!NOTE]
>
> Steg 5 gäller endast **AEM Forms**. Om du konfigurerar **endast AEM** kan du hoppa över det här steget.


#### Ändra web.xml

1. Öppna `WEB-INF\web.xml` i en textredigerare.

2. Leta reda på avsnittet `<servlet>` som innehåller konfigurationen för körningsläget:

   ```xml
   <!-- Set the runmode per default to author -->
   <init-param>
       <param-name>sling.run.modes</param-name>
       <param-value>author</param-value>
   </init-param>
   <load-on-startup>100</load-on-startup>
   </servlet>
   ```

3. Ersätt den avslutande `</servlet>`-taggen och föregående rad med:

   ```xml
   <init-param>
       <param-name>sling.run.modes</param-name>
       <param-value>author</param-value>
   </init-param>
   <multipart-config>
       <max-file-size>1048576000</max-file-size>
       <max-request-size>1048576000</max-request-size>
       <file-size-threshold>0</file-size-threshold>
   </multipart-config>
   <load-on-startup>100</load-on-startup>
   </servlet>
   ```

4. Spara och stäng `web.xml`.

**Syfte:** De här inställningarna möjliggör stora filöverföringar (upp till 1 GB) för AEM Forms och Digital Asset Management.

### Steg 6: Paketera om WAR-filen

När du är klar med alla konfigurationsändringar packar du om WAR-filen.

1. Gå tillbaka till arbetskatalogen som innehåller det extraherade innehållet:

   ```cmd
   cd C:\aem\war-config
   ```

2. Skapa den nya WAR-filen:

   ```cmd
   jar -cvf cq-quickstart.war *
   ```

>[!IMPORTANT]
>
> Utför bara det här steget en gång när alla konfigurationer är klara.

### Steg 7: Distribuera och starta AEM

#### Distribuera WAR till JBoss

1. Kopiera den ompaketerade `cq-quickstart.war` till JBoss-distributionskatalogen:

   ```
   <JBOSS_HOME>\standalone\deployments
   ```

   **Exempel:**
   ```C:\jboss-eap-8.0\standalone\deployments```

#### Konfigurera JVM-inställningar (valfritt men rekommenderat)

Konfigurera JVM-minnesinställningar innan du startar JBoss:

1. Öppna `<JBOSS_HOME>\bin\standalone.conf.bat` i en textredigerare.

1. Ändra eller lägg till följande rad för att ange stackminne:

   ```batch
   set "JAVA_OPTS=-Xms4096m -Xmx4096m -XX:MaxMetaspaceSize=512m"
   ```

>[!NOTE]
>
> Justera minnesvärden baserat på din serverkapacitet och AEM krav.

1. Spara och stäng filen.

#### Starta JBoss EAP

1. Öppna **Kommandotolken** som **administratör**.

1. Navigera till bin-katalogen för JBoss:

   ```cmd
   cd <JBOSS_HOME>\bin
   ```

   **Exempel:**
   ```cmd cd C:\jboss-eap-8.0\bin```

1. Starta JBoss-servern:

   ```cmd
   standalone.bat -b 0.0.0.0 -bmanagement 0.0.0.0
   ```

   **Parametrar:**
   * `-b 0.0.0.0` - binder servern till alla nätverksgränssnitt
   * `-bmanagement 0.0.0.0` - binder hanteringsgränssnittet till alla nätverksgränssnitt

#### Övervaka driftsättning

Titta på konsolutdata för distributionsmeddelanden. Slutförd distribution anges av:

```
Deployed "cq-quickstart.war" (runtime-name : "cq-quickstart.war")
```

### Steg 8: Få tillgång till AEM

När distributionen är klar och AEM har startat:

**URL för AEM-författare:**
```http://<server-ip>:8080/cq-quickstart```

**Standardautentiseringsuppgifter:**

* Användarnamn: `admin`
* Lösenord: `admin`

**Viktigt!** Ändra standardlösenordet omedelbart efter första inloggningen.

### Felsökning

#### Vanliga problem

| Problem | Lösning |
|-------|----------|
| Distributionen misslyckas med ClassNotFoundException | Verifiera att `jboss-deployment-structure.xml` är korrekt konfigurerad |
| OutOfMemoryError under start | Öka stackminnet i `standalone.conf.bat` |
| AEM startar inte efter distributionen | Kontrollera JBoss-loggar i `<JBOSS_HOME>\standalone\log\server.log` |
| Kan inte komma åt AEM i webbläsaren | Verifiera att brandväggsinställningarna tillåter port 8080 |

#### Loggfiler

* **JBoss-serverlogg:** `<JBOSS_HOME>\standalone\log\server.log`
* **AEM-fellogg:** Tillgänglig via AEM Web Console efter start på\
  `http://<server-ip>:8080/cq-quickstart/system/console`

### Ytterligare konfiguration

#### Konfigurera körningslägen

Om du vill ändra AEM körningslägen (författare/publicera) ändrar du parametern `sling.run.modes` i `WEB-INF\web.xml` innan du paketerar om WAR:

```xml
<init-param>
    <param-name>sling.run.modes</param-name>
    <param-value>publish</param-value>
</init-param>
```

#### Produktionsrekommendationer

För produktionsmiljöer:

* Konfigurera SSL-/TLS-certifikat i JBoss
* Konfigurera AEM replikeringsagenter
* Konfigurera dispatcher för belastningsutjämning
* Aktivera automatiserad säkerhetskopiering
* Implementera övervakning och varningar

### Relaterad dokumentation

* [JBoss EAP 8-dokumentation](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/8.0)
* [Adobe Experience Manager-dokumentation](https://experienceleague.adobe.com/docs/experience-manager-65.html)
* [AEM installations- och distributionshandbok](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html)

### Dokumentinformation

| Fält | Värde |
|-------|-------|
| Senast uppdaterad | Februari 2026 |
| AEM Version | 6,5+ (LTS) |
| JBoss-version | EAP 8.x |
| JDK-version | 21 |
| Plattform | Windows |


