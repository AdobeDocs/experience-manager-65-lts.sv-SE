---
title: Installationsanvisningar för AEM Forms Patch för AEM Forms
description: Installationsanvisningar för AEM Forms Service Pack för OSGi- och JEE-miljö
solution: Experience Manager
feature: Release Information
role: User,Admin,Architect,Developer
hide: true
hidefromtoc: true
source-git-commit: 8097d48b797759cc99e0b299a5dbca2d9f8194af
workflow-type: tm+mt
source-wordcount: '1685'
ht-degree: 0%

---

# Installationsanvisningar för AEM 6.5 Forms Service Pack {#aem-form-patch-installation-instructions}

## Versionsinformation

| Produkt | Adobe Experience Manager 6.5 Forms |
|---|---|
| Version | 6.5.22.0 |
| Typ | Service Pack-version |
| Datum | 29 november 2024 |
| Hämta URL | [De senaste AEM Forms-versionerna](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE) |

>[!NOTE]
>
>I den senaste [AEM Service Pack-versionsinformationen](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=sv-SE) finns en fullständig lista över åtgärdade problem.

## Vad ingår i Experience Manager Forms 6.5

<span class="preview"> Om du uppgraderar till AEM Forms Service Pack 6.5 LTS, se [Uppgradera till AEM Forms 6.5 LTS på OSGi](/help/forms/using/upgrade-forms-osgi.md). </span>

Adobe Experience Manager (AEM) Forms Service Pack innehåller nya och uppgraderade funktioner, som viktiga kundefterfrågade förbättringar, prestanda, stabilitet och säkerhetsförbättringar. AEM Forms lanserar Service Pack med regelbundna intervall för att ge tillgång till de senaste funktionerna och förbättringarna. Beroende på vilken teknologi du har väljer du en av följande sökvägar för att hämta och installera Service Pack i din miljö:

* [Hämta och installera Service Pack på ett AEM-formulär i JEE-miljö](#download-and-install-for-jee-service-pack)
* [Hämta och installera Service Pack på ett AEM-formulär i OSGi-miljö](#download-and-install-for-osgi-service-pack)

>[!NOTE]
>
> * Adobe släpper ett fullständigt installationsprogram var sjätte service pack. AEM 6.5 Forms Service Pack 18 (6.5.18.0) är det senaste fullständiga JEE-installationsprogrammet. Det fullständiga installationsprogrammet har stöd för nya plattformar medan det vanliga installationsprogrammet för Service Pack innehåller nya funktioner, felkorrigerade och allmänna förbättringar. Om du gör en ny installation eller planerar att använda den senaste programvaran för AEM 6.5 Forms i JEE-miljö rekommenderar Adobe att du använder AEM 6.5.18.0 Forms i JEE-fullversionen som släpptes den 31 augusti 2023 i stället för AEM 6.5 Forms-installationsprogrammet som släpptes den 8 april 2019 eller AEM 6.5.12.0 Forms installationsprogrammet som släpptes den 3 mars 202. Installera det senaste Service Pack-paketet när du har använt det fullständiga installationsprogrammet.
> * AEM Forms-funktionen, som Adaptiv Forms, som finns i [AEM 6.5 QuickStart](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=sv-SE), är endast avsedd för utforsknings- och utvärderingsändamål. För produktion krävs en giltig licens för AEM Forms.

<!--

## Prerequisites {#prerequisites}

From AEM Service Pack 6.5.19.0 and onwards, XMLFM (XML output) will be available in 64-bit only, therefore you require the latest [Microsoft Visual C++ Redistributable (2015-2022) 64-bit](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170) to be installed on Windows Server prior to JEE or OSGi installation.

>[!NOTE]
> This prerequisite is required in addition to the already existing Microsoft Visual C++ Redistributable 32-bit.

-->

## Hämta och installera Service Pack på ett AEM-formulär i JEE-miljö {#download-and-install-for-jee-service-pack}

<!--
![JEE Installation](/help/forms/using/assets/jeeinstallation.png) -->

+++&#x200B;1. Säkerhetskopiera din befintliga miljö

1. Säkerhetskopiera din [CRX-databas, databasschema och GDS (global dokumentlagring)](https://experienceleague.adobe.com/docs/experience-manager-65/forms/administrator-help/aem-forms-backup-recovery/backing-aem-forms-data.html?lang=sv-SE).
1. Säkerhetskopiera mappen &lt;*AEM_forms_root*>/deploy.

>[!NOTE]
>
> Innan du kör installationsprogrammet för AEM Service Pack bör du kontrollera att du har skrivbehörighet för AEM installationskatalog.

+++

+++&#x200B;2. Ladda ned den programvara som krävs

* [AEM Forms på JEE Service Pack](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE)

* [Fragmentserver](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=%2Fcontent%2Fsoftware-distribution%2Fen%2Fdetails.html%2Fcontent%2Fdam%2Faem%2Fpublic%2Fadobe%2Fpackages%2Fcq650%2Ffeaturepack%2Forg.apache.felix.http.servlet-api-1.2.0_fragment_full.jar)

* [AEM Service Pack](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=sv-SE)
  <!--* [Forms add-on package](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE)-->


+++

+++&#x200B;3. Installera Microsoft Visual C++ Redistributable-paket

* Hämta och installera [64-bitarsversionen av Microsoft Visual C++ Redistributable-paket för Visual Studio 2015, 2017, 2019 och 2022](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022) på den dator där AEM 6.5 Forms är installerat.

>[!NOTE]
>
> Kontrollera att du har installerat Redistributable även om en tidigare version är installerad, för att säkerställa att den senaste versionen är tillgänglig.

+++

+++&#x200B;4. Installera AEM Forms på JEE-Service Pack:

1. Stoppa programservern.
1. Extrahera installationsarkivet för **AEM Forms på JEE Service Pack** till hårddisken:

   * **Windows**
Navigera till rätt katalog på installationsmediet eller mappen på hårddisken där du kopierade     installationsprogrammet och dubbelklicka på `aemforms65_cfp_install.exe` -filen.

      * (32-bitars Windows) `Windows\Disk1\InstData\VM`
      * (64-bitars Windows) `Windows_64Bit`\ `Disk1\InstData\VM`

   * **Linux®**
Navigera till rätt katalog och från ett skal och typ `./aem65_cfp_install.bin` .

      * (Linux®) `Linux/Disk1/InstData/NoVM`

   Då startas en installationsguide som vägleder dig genom installationen.

1. Klicka på **[!UICONTROL Next]** på introduktionspanelen.
1. På skärmen **Välj installationsmapp** kontrollerar du att den standardplats som visas är korrekt för den befintliga installationen. Du kan också klicka på **[!UICONTROL Browse]** och välja en alternativ mapp där AEM-formulär är installerade. Klicka sedan på **[!UICONTROL Next]**.
1. Läs sammanfattningsinformationen för Service Pack och klicka på **[!UICONTROL Next]**.
1. Läs informationen om sammanfattning av förinstallation och klicka på **[!UICONTROL Install]**.
1. När installationen är klar klickar du på **[!UICONTROL Next]** för att tillämpa snabbkorrigeringsuppdateringarna på de installerade filerna.
1. **[För endast Windows]:** Utför något av följande steg:

   * Avmarkera alternativet **Starta Configuration Manager** innan du klickar på **[!UICONTROL Done]**. Kör **Configuration Manager** med filen **ConfigurationManager.bat** i `[aem-forms root]\configurationManager\bin`.

   * Du kan också avmarkera alternativet **Starta Configuration Manager** innan du klickar på **[!UICONTROL Done]**. Innan du kör **Configuration Manager** med **ConfigurationManager.exe** eller **ConfigurationManager_IPv6.exe** navigerar du till katalogen *`<AEMForms_Install_Dir>\configurationManager\bin`* och ersätter katalogen **ConfigurationManager.lax** och **ConfigurationManager_IPV6.lax** med den senaste [ConfigurationManager.lax](/help/assets/ConfigurationManager.lax) och [ConfigurationManager_IPV6.lax](/help/assets/ConfigurationManager_IPv6.lax) -filer, söker och ersätter **axis-1.4.1.1.jar** med **axis-1.4.1.2.jar** i dessa två filer.

     >[!NOTE]
     >
     >* Genom att uppdatera eller ersätta filen **ConfigurationManager.bat** kan du undvika att uppdatera .lax-filerna manuellt.

1. **[Kryssrutan ]Starta Configuration Manager** är markerad som standard för Unix-baserad endast **:**. Klicka på **[!UICONTROL Done]** om du vill köra Configuration Manager direkt eller **Configuration Manager** senare, avmarkera alternativet **Starta Configuration Manager** innan du klickar på **[!UICONTROL Done]**. Du kan starta **Configuration Manager** senare med lämpligt skript i katalogen `[AEM_forms_root]/configurationManager/bin`.

1. Beroende på vilken programserver du använder väljer du något av följande dokument och följer instruktionerna i avsnittet *Konfigurera och distribuera AEM-formulär*.

   * [Installera och distribuera AEM-formulär för JBoss®](https://www.adobe.com/go/learn_aemforms_installJBoss_65)
   * [Installera och distribuera AEM-formulär för WebSphere®](https://www.adobe.com/go/learn_aemforms_installWebSphere_65)
   * [Installera och distribuera AEM Forms för WebLogic](https://www.adobe.com/go/learn_aemforms_installWebLogic_65)
   * [Installera och distribuera AEM-formulär för JBoss® Cluster](https://helpx.adobe.com/content/dam/help/en/experience-manager/6-5/forms/pdf/install-cluster-jboss.pdf)
   * [Installera och distribuera AEM-formulär för WebSphere® Cluster](https://helpx.adobe.com/content/dam/help/en/experience-manager/6-5/forms/pdf/install-cluster-websphere.pdf)
   * [Installera och distribuera AEM Forms för WebLogic-kluster](https://helpx.adobe.com/content/dam/help/en/experience-manager/6-5/forms/pdf/install-cluster-weblogic.pdf)


>[!NOTE]
>
>* När du har installerat AEM Forms på JEE-Service Pack måste du ta bort Forms-tilläggspaketet från mappen `crx-repository\install` innan du startar om appservern. Hämta det senaste Forms-tilläggspaketet från [Software Distribution Portal](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE).
>* Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

+++

+++&#x200B;5. Installera serverletsfragmentet om det inte är installerat (**Obligatoriskt steg**)

<!-- >[!NOTE] > > * If you are upgrading from **AEM Service Pack 6.5.15.0**, the installation of the **servlet fragment** is not required. For versions **AEM Service Pack 6.5.14.0** or earlier, it is **mandatory to install** the servlet fragment. -->

Så här hämtar och installerar du serverletsfragmentet:

1. Om du inte har hämtat fragmentet hämtar du det från [Programvarudistribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/featurepack/org.apache.felix.http.servlet-api-1.2.0_fragment_full.jar).

2. Starta programservern, vänta på att loggarna ska stabiliseras och kontrollera paketläget.

3. Öppna Web Console Bundles. Standardwebbadressen är `http://[Server]:[Port]/system/console/bundles`.

4. Klicka på Installera/Uppdatera. Välj det hämtade fragmentet, `org.apache.felix.http.servlet-api-1.2.0_fragment_full.jar`. Klicka på **Installera** eller **Uppdatera**. Vänta tills programservern har stabiliserats

5. Stoppa programservern.

+++

+++&#x200B;6. Installera AEM Service Pack.

1. Starta om instansen innan installationen om instansen är i uppdateringsläge (när instansen uppdaterades från en tidigare version). Adobe rekommenderar att du startar om om den aktuella upptiden för en instans är hög.
1. Ta en ögonblicksbild eller en ny säkerhetskopia av din [!DNL Experience Manager]-instans innan du installerar.
1. Hämta Service Pack från [Programvarudistribution](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE). <!-- UPDATE FOR EACH NEW RELEASE -->
1. Öppna Pakethanteraren och välj sedan **[!UICONTROL Upload Package]** för att överföra paketet. Mer information finns i [Pakethanteraren](/help/sites-administering/package-manager.md).
1. Markera paketet och välj sedan **[!UICONTROL Install]**.

**Automatisk installation**

Det finns två olika metoder som du kan använda för att automatiskt installera [!DNL ExperienceManager] Service Pack.<!--       UPDATE FOR EACH NEW RELEASE -->

* Placera paketet i mappen `../crx-quickstart/install` när servern är tillgänglig online.
Paketet är      installeras automatiskt.

* Använd [HTTP-API:t från Package Manager](https://experienceleague.adobe.com/docs/experience-manager-65/administering/contentmanagement/package-manager.html?lang=sv-SE). Använd `cmd=install&recursive=true` så att kapslade paket installeras.

  >[!NOTE]
  >
  >Experience Manager Service Pack stöder inte installation av Bootstrap. <!-- UPDATE FOR EACH NEW RELEASE -->

  **Verifiera installationen**

  Information om vilka plattformar som är certifierade för att fungera med den här versionen finns i [tekniska krav](/help/sites-deploying/technical-requirements.md).

   1. Produktinformationssidan (`/system/console/productinfo`) visar den uppdaterade versionssträngen `Adobe Experience Manager (spversion)` under [!UICONTROL Installed Products].<!-- UPDATE FOR EACH NEW RELEASE -->
   1. Alla OSGi-paket är antingen **[!UICONTROL ACTIVE]** eller **[!UICONTROL FRAGMENT]** i OSGi-konsolen (använd webben)     Konsol: `/system/console/bundles`).
   1. OSGi-paketet `org.apache.jackrabbit.oak-core` är version 1.2.14 eller senare (Använd WebConsole: `/system/console/bundles`).

+++

+++&#x200B;7. Installera AEM Experience Manager Forms-tilläggspaketet

1. Kontrollera att du har installerat Service Pack [!DNL Experience Manager].
1. Hämta motsvarande Forms-tilläggspaket som finns i [AEM Forms-utgåvor](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE) för ditt operativsystem.
1. Installera Forms tilläggspaket enligt beskrivningen i [Installera AEM Forms tilläggspaket](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE).
1. Om du använder bokstäver i Experience Manager 6.5 Forms installerar du det [senaste AEMFD-kompatibilitetspaketet](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE).

+++

## Hämta och installera Service Pack på en AEM-blankett i OSGi-miljö {#download-and-install-for-osgi-service-pack}


<!-- ![OSGi Installation Steps](/help/forms/using/assets/osgiinstallation.png)
-->

+++&#x200B;1. Säkerhetskopiera din befintliga miljö

1. Säkerhetskopiera din [CRX-databas och databasschema](https://experienceleague.adobe.com/docs/experience-manager-65/forms/administrator-help/aem-forms-backup-recovery/backing-aem-forms-data.html?lang=sv-SE).

>[!NOTE]
>
> Om du installerar AEM Forms Service Pack för relationsdatabas måste du säkerhetskopiera DB_schema.

+++

+++&#x200B;2. Ladda ned den programvara som krävs

* [AEM Service Pack](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=sv-SE)
* [Forms-tilläggspaket](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE)

+++

+++ &#x200B;3. Installera Microsoft Visual C++ Redistributable-paket

* Hämta och installera [64-bitarsversionen av Microsoft Visual C++ Redistributable-paket för Visual Studio 2015, 2017, 2019 och 2022](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022) på den dator där AEM 6.5 Forms är installerat.

>[!NOTE]
>
>
> Kontrollera att du har installerat Redistributable även om en tidigare version är installerad, för att säkerställa att den senaste versionen är tillgänglig.

+++

+++&#x200B;4. Installera AEM Service Pack

1. Starta om instansen innan installationen om instansen är i uppdateringsläge (när instansen uppdaterades från en tidigare version). Adobe rekommenderar att du startar om om den aktuella upptiden för en instans är hög.
1. Ta en ögonblicksbild eller en ny säkerhetskopia av din [!DNL Experience Manager]-instans innan du installerar.
1. Hämta Service Pack från [Programvarudistribution](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE). <!-- UPDATE FOR EACH NEW RELEASE -->
1. Öppna Pakethanteraren och välj sedan **[!UICONTROL Upload Package]** för att överföra paketet. Mer information finns i [Pakethanteraren](/help/sites-administering/package-manager.md).
1. Markera paketet och välj sedan **[!UICONTROL Install]**.

**Automatisk installation**

Det finns två olika metoder som du kan använda för att automatiskt installera [!DNL Experience Manager] Service Pack.<!--  UPDATE FOR EACH NEW RELEASE -->

* Placera paketet i mappen `../crx-quickstart/install` när servern är tillgänglig online. Paketet är      installeras automatiskt.
* Använd [HTTP-API:t från Package Manager](https://experienceleague.adobe.com/docs/experience-manager-65/administering/contentmanagement/package-manager.html?lang=sv-SE). Använd `cmd=install&recursive=true` så att kapslade paket installeras.

  >[!NOTE]
  >
  >Experience Manager Service Pack stöder inte installation av Bootstrap. <!-- UPDATE FOR EACH NEW RELEASE -->

  **Verifiera installationen**

  Information om vilka plattformar som är certifierade för att fungera med den här versionen finns i [tekniska krav](/help/sites-deploying/technical-requirements.md).

   1. Produktinformationssidan (`/system/console/productinfo`) visar den uppdaterade versionssträngen `Adobe Experience Manager (spversion)` under [!UICONTROL Installed Products]. <!-- UPDATE FOR EACH NEW RELEASE -->

   1. Alla OSGi-paket är antingen **[!UICONTROL ACTIVE]** eller **[!UICONTROL FRAGMENT]** i OSGi-konsolen (använd webbkonsol: `/system/console/bundles`).

      1. OSGi-paketet `org.apache.jackrabbit.oak-core` är version 1.2.14 eller senare (Använd webbkonsol: `/system/console/bundles`).

+++

+++&#x200B;5. Installera tilläggspaketet Adobe Experience Manager Forms (AEM)

1. Kontrollera att du har installerat Service Pack [!DNL Experience Manager].
1. Hämta motsvarande Forms-tilläggspaket som finns i [AEM Forms-utgåvor](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE) för ditt operativsystem.
1. Installera Forms tilläggspaket enligt beskrivningen i [Installera AEM Forms tilläggspaket](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE).
1. Om du använder bokstäver i Experience Manager 6.5 Forms installerar du det [senaste AEMFD-kompatibilitetspaketet](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=sv-SE).

+++

## Felsökning

* Om **dialogrutan i pakethanterarens gränssnitt** avslutas under installationen av Service Pack väntar du tills felloggarna har stabiliserats innan du får åtkomst till distributionen. Vänta på de specifika loggarna för avinstallationen av uppdateringspaketet innan du försäkrar dig om att installationen lyckas. Vanligtvis inträffar det här problemet i webbläsaren Safari, men det kan inträffa i alla webbläsare.

* Kontrollera övervakningsloggarna (error.log) när installationen är klar för att se om det finns några aktiviteter. Vänta i några minuter tills loggarna inte har någon aktivitet. Starta om AEM.

* Om du får ett **tjänstfel som inte är tillgängligt** efter installation av AEM Forms 6.5.15.0 eller senare Service Pack, [installerar du servletsfragmentet och paketet](/help/forms/using/aem-service-pack-installation-solution.md) för att åtgärda felet.
