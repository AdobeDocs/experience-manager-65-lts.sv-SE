---
title: Konfigurera dynamiska media - Scene7-läge
description: Lär dig konfigurera läget Dynamic Media - Scene7.
role: User, Admin
mini-toc-levels: 4
feature: Configuration,Scene7 Mode
solution: Experience Manager, Experience Manager Assets
exl-id: 98bd0c24-6c5e-4b96-a3aa-a3e4ef802baf
source-git-commit: 2e0cbe62754866d31de69547f9af1f2f63930f2c
workflow-type: tm+mt
source-wordcount: '6104'
ht-degree: 1%

---

# Konfigurera dynamiska media - Scene7-läge{#configuring-dynamic-media-scene-mode}

Om du använder Adobe Experience Manager konfigurerade för olika miljöer, som utveckling, testning och produktion, konfigurerar du Dynamic Media Cloud Services för var och en av dessa miljöer.

## Arkitektur för Dynamic Media - Scene7-läge {#architecture-diagram-of-dynamic-media-scene-mode}

I följande arkitekturdiagram beskrivs hur läget Dynamic Media - Scene7 fungerar.

Med den nya arkitekturen ansvarar Experience Manager för primära källfiler och synkningar med Dynamic Media för bearbetning och publicering av material:

1. När den primära källresursen överförs till Experience Manager replikeras den till Dynamic Media. I det läget hanterar Dynamic Media all materialbearbetning och återgivningsgenerering, till exempel videokodning och dynamiska varianter av en bild.
(I läget Dynamic Media - Scene7 är standardstorleken för överföring 2 GB eller mindre. Om du vill aktivera filstorlekar på 2 GB upp till 15 GB läser du [ (Valfritt) Konfigurera dynamiska media - Scen7-läge för överföring av resurser som är större än 2 GB](#optional-config-dms7-assets-larger-than-2gb).)
1. När återgivningarna har genererats kan Experience Manager på ett säkert sätt få åtkomst till och förhandsgranska de dynamiska fjärråtergivningarna (inga binärfiler skickas tillbaka till Experience Manager-instansen).
1. När innehållet är klart att publiceras och godkännas utlöses Dynamic Media-tjänsten att skicka ut innehållet till leveransservrar och cachelagra innehållet på CDN (Content Delivery Network).

![chlimage_1-550](assets/chlimage_1-550.png)

>[!IMPORTANT]
>
>Följande funktionslista kräver att du använder det färdiga CDN som medföljer Adobe Experience Manager - Dynamic Media. Andra anpassade CDN stöds inte med dessa funktioner.
>
>* [Smart bildbehandling](/help/assets/imaging-faq.md)
>* [Cacheogiltigförklaring](/help/assets/invalidate-cdn-cache-dynamic-media.md)
>* [Hotlink-skydd](/help/assets/hotlink-protection.md)
>* [HTTP/2-leverans av innehåll](/help/assets/http2.md)
>* URL-omdirigering på CDN-nivå
>* Akamai ChinaCDN (för optimal leverans i Kina)

## Aktivera Dynamic Media i Scene7-läge {#enabling-dynamic-media-in-scene-mode}

[Dynamiska media](https://business.adobe.com/products/experience-manager/assets/dynamic-media.html) är inaktiverat som standard. För att kunna utnyttja Dynamic Media-funktionerna måste du aktivera dem.

>[!WARNING]
>
>Dynamiska media - Scene7-läget är endast avsett för *Experience Manager Author-instansen*. Därför måste du konfigurera `runmode=dynamicmedia_scene7` på Experience Manager Author-instansen, *inte*, Experience Manager Publish-instansen.

Om du vill aktivera Dynamic Media startar du Experience Manager med körningsläget `dynamicmedia_scene7` från kommandoraden genom att ange följande i ett terminalfönster (exempelporten som används är 4502):

```shell {.line-numbers}
java -Xms4096m -Xmx4096m -Doak.queryLimitInMemory=500000 -Doak.queryLimitReads=500000 -jar cq-quickstart-6.5.0.jar -gui -r author,dynamicmedia_scene7 -p 4502
```

## (Valfritt) Migrera förinställningar och konfigurationer för dynamiska media från 6.3 till 6.5 Zero Downtime {#optional-migrating-dynamic-media-presets-and-configurations-from-to-zero-downtime}

Uppgradering av Experience Manager Dynamic Media från 6.3 till 6.4 eller 6.5 innefattar nu möjligheten till driftsättning utan driftstopp. Om du vill migrera alla dina förinställningar och konfigurationer från `/etc` till `/conf` i CRXDE Lite måste du köra följande kommando.

>[!NOTE]
>
>Om du kör din Experience Manager-instans i kompatibilitetsläge, d.v.s. har kompatibilitetspaketet installerat, behöver du inte köra dessa kommandon.

För alla uppgraderingar, antingen med eller utan kompatibilitetspaketet, kan du kopiera de förinställda visningsprogrammen som ursprungligen levererades med Dynamic Media genom att köra följande kommando för Linux®:

`curl -u admin:admin -X POST https://<server_address>:<server_port>/libs/settings/dam/dm/presets/viewer.pushviewerpresets.json`

Om du vill migrera anpassade förinställningar och konfigurationer för visningsprogram som du har skapat från `/etc` till `/conf` kör du följande kommando för Linux®-kontroll:

`curl -u admin:admin -X POST https://<server_address>:<server_port>/libs/settings/dam/dm/presets.migratedmcontent.json`

## Installera funktionspaket 18912 för migrering av gruppresurser {#installing-feature-pack-for-bulk-asset-migration}

Installationen av funktionspaket 18912 är *valfri*.

Med funktionspaketet 18912 kan du antingen importera resurser gruppvis via FTP eller migrera resurser från antingen Dynamic Media - hybrid-läge eller Dynamic Media Classic till Dynamic Media - Scene7-läge i Experience Manager. Den är tillgänglig från [Adobe Professional Services](https://business.adobe.com/customers/consulting-services/main.html).

Mer information finns i [Installera funktionspaket 18912 för migrering av gruppresurser](/help/assets/bulk-ingest-migrate.md).

## Skapa en dynamisk mediekonfiguration i molntjänster {#configuring-dynamic-media-cloud-services}

<!-- **Before you configure Dynamic Media** - After you receive your provisioning email with Dynamic Media credentials, you must open the [Dynamic Media Classic desktop application](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/signing-out.html#getting-started), then sign in to your account to change your password. The password provided in the provisioning email is system-generated and intended to be a temporary password only. It is important that you update the password so that Dynamic Media Cloud Service is set up with the correct credentials.

   ![dynamicmediaconfiguration2updated](assets/dynamicmediaconfiguration2updated.png)

**To create a Dynamic Media Configuration in Cloud Services:** -->

1. I Experience Manager Author mode väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och väljer sedan Verktyg-ikonen. Gå sedan till **[!UICONTROL Cloud Services]** > **[!UICONTROL Dynamic Media Configuration]**.
1. På sidan Dynamic Media Configuration Browser väljer du **[!UICONTROL global]** i den vänstra rutan (markera inte mappikonen till vänster om **[!UICONTROL global]**) och sedan **[!UICONTROL Create]**.
1. På sidan **[!UICONTROL Create Dynamic Media Configuration]** anger du en titel, e-postadress för Dynamic Media-kontot, lösenord och väljer sedan region. Den här informationen tillhandahålls av Adobe i e-postmeddelandet om etablering. Kontakta Adobe kundsupport om du inte fått e-postmeddelandet.

   Välj **[!UICONTROL Connect to Dynamic Media]**.

1. I dialogrutan **[!UICONTROL Change Password]** anger du ett nytt lösenord som består av 8-25 tecken i fältet **[!UICONTROL New Password]**. Lösenordet måste innehålla minst ett av följande:

   * Versaler
   * Gemener
   * Nummer
   * Specialtecken: `# $ & . - _ : { }`

   Fältet **[!UICONTROL Current Password]** är avsiktligt ifyllt och dolt för interaktion.

   Om det behövs kan du kontrollera stavningen av ett lösenord som du har skrivit eller skrivit in igen genom att markera lösenordsikonen för att visa lösenordet. Klicka på ikonen igen om du vill dölja lösenordet.

1. Skriv det nya lösenordet igen i fältet **[!UICONTROL Repeat Password]** och välj sedan **[!UICONTROL Done]**.

   Det nya lösenordet sparas när du väljer **[!UICONTROL Save]** i det övre högra hörnet på sidan **[!UICONTROL Create Dynamic Media Configuration]**.

   Om du valde **[!UICONTROL Cancel]** i dialogrutan **[!UICONTROL Change Password]** måste du fortfarande ange ett nytt lösenord när du sparar den nya Dynamic Media-konfigurationen.

   Se även [Ändra lösenordet till Dynamic Media](#change-dm-password).

1. Ange följande när anslutningen lyckas. Rubriker med asterisk (*) krävs:

   * **[!UICONTROL Company]** - namnet på Dynamic Media-kontot.
     >[!IMPORTANT]
     >
     >Endast en dynamisk mediekonfiguration i molntjänster stöds i en instans av Experience Manager. Lägg inte till mer än en konfiguration. Flera dynamiska mediekonfigurationer i en Experience Manager-instans stöds _inte_ eller rekommenderas av Adobe.

     <!-- CQDOC-19579 and CQDOC-19612 -->

     Se även [Konfigurera företaget Dynamic Media alias ](/help/assets/dm-alias-account.md).

   * **[!UICONTROL Company Root Folder Path]**

   * **[!UICONTROL Publishing Assets]** - Du kan välja mellan följande tre alternativ:
      * **[!UICONTROL Immediately]** betyder att när resurser överförs, importeras resurserna och URL/Embed anges omedelbart. Ingen användaråtgärd krävs för att publicera resurser.
      * **[!UICONTROL Upon Activation]** betyder att du måste publicera resursen explicit innan en URL/Embed-länk anges.<br><!-- CQDOC-17478, Added March 9, 2021-->Från och med Experience Manager 6.5.8 återspeglar Experience Manager Publish-instansen korrekta värden för dynamiska mediametadata, som `dam:scene7Domain` och `dam:scene7FileStatus` i **[!UICONTROL Upon Activation]** publiceringsläge. Gå till Sling Config Manager. Hitta konfigurationen för `Scene7ActivationJobConsumer Component` eller skapa en ny). Markera kryssrutan **[!UICONTROL Replicate Metadata after Dynamic Media publishing]** och välj sedan **[!UICONTROL Save]**.

        ![Kryssrutan Replikera metadata efter publicering av dynamiska media](assets-dm/replicate-metadata-setting.png)

      * **[!UICONTROL Selective Publish]** Med det här alternativet kan du styra vilka mappar som publiceras i Dynamic Media. Du kan använda funktioner som smart beskärning eller dynamiska återgivningar, eller avgöra vilka mappar som publiceras exklusivt i Experience Manager för förhandsgranskning. Samma resurser publiceras *inte* i Dynamic Media för leverans i den offentliga domänen.<br>Du kan ange det här alternativet här i **[!UICONTROL Dynamic Media Cloud Configuration]** eller, om du vill, du kan välja att ange det här alternativet på mappnivå i en mapps **[!UICONTROL Properties]**.<br>Se [Arbeta med selektiv publicering i dynamiska media](/help/assets/selective-publishing.md).<br>Om du senare ändrar den här konfigurationen, eller ändrar den senare på mappnivå, påverkar ändringarna bara nya resurser som du överför från den punkten och framåt. Publiceringsläget för befintliga resurser i mappen ändras inte förrän du ändrar dem manuellt från antingen **[!UICONTROL Quick Publish]** eller dialogrutan **[!UICONTROL Manage Publication]**.

   * **[!UICONTROL Secure Preview Server]** - gör att du kan ange URL-sökvägen till förhandsgranskningsservern för säkra återgivningar. Det innebär att när renderingar har skapats kan Experience Manager på ett säkert sätt komma åt och förhandsgranska de dynamiska fjärrrenderingarna (inga binärfiler skickas tillbaka till Experience Manager-instansen).
Om du inte har en särskild lösning för att använda ditt företags server eller en speciell server rekommenderar Adobe att du låter den här inställningen vara kvar.

   * **[!UICONTROL Sync all content]** - <!-- NEW OPTION, CQDOC-15371, Added March 4, 2020-->Markerad som standard. Avmarkera det här alternativet om du vill inkludera eller exkludera resurser från synkroniseringen till dynamiska media. Om du avmarkerar det här alternativet kan du välja mellan följande två synkroniseringslägen för dynamiska media:

   * **[!UICONTROL Dynamic Media sync mode]**
      * **[!UICONTROL Enabled by default]** - Konfigurationen används som standard på alla mappar, såvida du inte markerar en mapp som är specifikt för undantag. <!-- you can then deselect the folders that you do not want the configuration applied to.-->
      * **[!UICONTROL Disabled by default]** - Konfigurationen tillämpas inte på någon mapp förrän du uttryckligen markerar en markerad mapp för synkronisering till Dynamic Media.
Om du vill markera en markerad mapp för synkronisering till dynamiska media väljer du en resursmapp och sedan **[!UICONTROL Properties]** i verktygsfältet. På fliken **[!UICONTROL Details]** i listrutan **[!UICONTROL Dynamic Media sync mode]** väljer du bland följande tre alternativ. Välj **[!UICONTROL Save]** när du är klar. *Kom ihåg: dessa tre alternativ är inte tillgängliga om du valde **[!UICONTROL Sync all content]**tidigare.* Se även [Arbeta med selektiv publicering på mappnivå i Dynamic Media](/help/assets/selective-publishing.md).
         * **[!UICONTROL Inherited]** - Inget explicit synkroniseringsvärde för mappen. I stället ärver mappen synkroniseringsvärdet från en av dess överordnade mappar eller standardläget i molnkonfigurationen. Detaljerad status för ärvda program via ett verktygstips.
         * **[!UICONTROL Enable for subfolders]** - Inkludera allt i det här underträdet för synkronisering till dynamiska media. De mappspecifika inställningarna åsidosätter standardläget i molnkonfigurationen.
         * **[!UICONTROL Disabled for subfolders]** - Uteslut allt i det här underträdet från synkronisering till Dynamic Media.

   >[!NOTE]
   >
   >Det finns inget stöd för versionshantering i Dynamic Media - Scene7-läge. Dessutom gäller fördröjd aktivering endast om **[!UICONTROL Publish Assets]** på sidan Redigera Dynamic Media-konfiguration är inställd på **[!UICONTROL Upon Activation]** och då endast tills resursen aktiveras första gången.
   >
   >När en mediefil har aktiverats publiceras alla uppdateringar direkt till S7 Delivery.

1. Välj **[!UICONTROL Save]**.
1. För att på ett säkert sätt förhandsgranska dynamiskt medieinnehåll innan det publiceras använder Experience Manager Author tokenbaserad validering och Experience Manager Author förhandsgranskar alltså dynamiskt medieinnehåll som standard. Du kan dock tillåtslista fler IP-adresser för att ge användarna tillgång till säkert förhandsgranskningsmaterial. Information om hur du konfigurerar den här åtgärden i Experience Manager finns i [Konfigurera inställningar för dynamisk mediepublicering för bildserver - säkerhetsflik](/help/assets/dm-publish-settings.md#security-tab).

Om du vill anpassa konfigurationen ytterligare, t.ex. aktivera ACL-behörigheter (åtkomstkontrollista), kan du utföra alla åtgärder under [(Valfritt) Konfigurera avancerade inställningar i läget Dynamic Media - Scene7 ](#optional-configuring-advanced-settings-in-dynamic-media-scene-mode).

<!-- 1. To securely preview Dynamic Media content before it gets published, Experience Manager uses token-based validation and hence Experience Manager Author previews Dynamic Media content by default. However, you can *allowlist* more IPs to provide users access to securely preview content. To set up this action in Experience Manager, see [Configure Dynamic Media Publish Setup for Image Server - Security tab](/help/assets/dm-publish-settings.md#security-tab).     * In Experience Manager Author mode, select the Experience Manager logo to access the global navigation console.
    * In the left rail, select the **[!UICONTROL Tools]** icon, then go to **[!UICONTROL Assets]** > **[!UICONTROL Dynamic Media Publish Setup]**.
    * On the Dynamic Media Image Server page, in the **[!UICONTROL Publish Context]** drop-down list, select **[!UICONTROL Test Image Serving]**.
    * Select the **[!UICONTROL Security]** tab.
    * For the **[!UICONTROL Client address]**, select **[!UICONTROL Add]**.
    * Enter the IP address of the Experience Manager Author instance (not Dispatcher IP).
    * In the upper-right corner of the page, select **[!UICONTROL Save]**. -->

Du är nu klar med den grundläggande konfigurationen. Du är redo att använda läget Dynamic Media - Scene7.

### Ändra lösenordet till Dynamic Media {#change-dm-password}

Lösenordets giltighetstid i Dynamic Media är inställd på 100 år från det aktuella systemdatumet.

Lösenordet måste innehålla minst ett av följande:

* Versaler
* Gemener
* Nummer
* Specialtecken: `# $ & . - _ : { }`

Om det behövs kan du kontrollera stavningen av ett lösenord som du har skrivit eller skrivit in igen genom att markera lösenordsikonen för att visa lösenordet. Klicka på ikonen igen om du vill dölja lösenordet.

Det ändrade lösenordet sparas när du väljer **[!UICONTROL Save]** i det övre högra hörnet på sidan **[!UICONTROL Edit Dynamic Media Configuration]**.

**Så här ändrar du lösenordet till Dynamic Media:**

1. I Experience Manager Author mode väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen.
1. Till vänster om konsolen väljer du verktygsikonen och går till **[!UICONTROL Cloud Services]>[!UICONTROL Dynamic Media Configuration]**.
1. På sidan Dynamic Media Configuration Browser väljer du **[!UICONTROL global]** i den vänstra rutan. Välj inte mappikonen till vänster om **[!UICONTROL global]**. Välj sedan **[!UICONTROL Edit]**.
1. Välj **[!UICONTROL Change Password]** på sidan **[!UICONTROL Edit Dynamic Media Configuration]**, direkt under fältet **[!UICONTROL Password]**.
1. Gör följande i dialogrutan **[!UICONTROL Change Password]**:

   * Ange ett nytt lösenord i fältet **[!UICONTROL New Password]**.

     Fältet **[!UICONTROL Current Password]** är avsiktligt ifyllt och dolt för interaktion.

   * Skriv det nya lösenordet igen i fältet **[!UICONTROL Repeat Password]** och välj sedan **[!UICONTROL Done]**.

1. I det övre högra hörnet på sidan **[!UICONTROL Edit Dynamic Media Configuration]** väljer du **[!UICONTROL Save]** och sedan **[!UICONTROL OK]**.

## (Valfritt) Konfigurera avancerade inställningar i Dynamic Media - Scene7-läge {#optional-configuring-advanced-settings-in-dynamic-media-scene-mode}

Om du vill anpassa konfigurationen och konfigurationen av läget Dynamic Media - Scene7 ytterligare, eller optimera dess prestanda, kan du utföra en eller flera av följande *valfria* åtgärder:

* [(Valfritt) Aktivera ACL-behörigheter i Dynamic Media - Scene7-läge](#optional-enable-acl)

* [(Valfritt) Konfigurera dynamiska media - Scene7-läge för överföring av resurser som är större än 2 GB](#optional-config-dms7-assets-larger-than-2gb)

* [(Valfritt) Installation och konfiguration av Dynamic Media - Scene7-lägesinställningar](#optional-setup-and-configuration-of-dynamic-media-scene7-mode-settings)

* [(Valfritt) Justera prestanda för Dynamic Media - Scene7-läge](#optional-tuning-the-performance-of-dynamic-media-scene-mode)

* [(Valfritt) Filtrera resurser för replikering](#optional-filtering-assets-for-replication)

### (Valfritt) Aktivera behörigheter för åtkomstkontrollistor i läget Dynamic Media - Scene7 {#optional-enable-acl}

När du kör Dynamic Media - Scene7-läget på AEM vidarebefordrar det för närvarande `/is/image` begäranden till säker förhandsvisning av bildservrar utan att kontrollera ACL-behörigheter (Access Control List) på PlatformServerServlet. Du kan dock *aktivera* ACL-behörigheter. Detta vidarebefordrar de auktoriserade `/is/image`-förfrågningarna. Om en användare inte har behörighet att komma åt resursen visas ett &quot;403 - Ej tillåtet&quot;-fel.

**Så här aktiverar du ACL-behörigheter i Dynamic Media - Scene7-läge:**

1. Navigera från Experience Manager till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.

   ![2019-08-02_16-13-14](assets/2019-08-02_16-13-14.png)

1. En ny flik i webbläsaren öppnas på sidan **[!UICONTROL Adobe Experience Manager Web Console Configuration]**.

   ![2019-08-02_16-17-29](assets/2019-08-02_16-17-29.png)

1. Bläddra till namnet *Adobe CQ Scene7 PlatformServer* på sidan.

1. Till höger om namnet väljer du pennikonen (**[!UICONTROL Edit the configuration values]**).

1. Markera kryssrutan för följande två inställningar på sidan **com.adobe.cq.dam.s7imaging.impl.ps.PlatformServerServlet.name**:

   * `com.adobe.cq.dam.s7imaging.impl.ps.PlatformServerServlet.cache.enable.name` - När den här inställningen är aktiverad cachelagras behörigheterna i 120 sekunder (två minuter) (standard) för att sparas.
   * `com.adobe.cq.dam.s7imaging.impl.ps.PlatformServerServlet.validate.userAccess.name` - När den här inställningen är aktiverad valideras en användares åtkomst medan han/hon förhandsgranskar resurser via Dynamic Media Image Server.

   ![Aktivera inställningar för åtkomstkontrollistor i Dynamic Media - Scene7-läge](/help/assets/assets-dm/acl.png)

1. Välj **[!UICONTROL Save]** i sidans nedre högra hörn.

### (Valfritt) Konfigurera dynamiska media - Scene7-läge för överföring av resurser som är större än 2 GB {#optional-config-dms7-assets-larger-than-2gb}

I Dynamic Media - Scene7-läge är standardfilstorleken för överföring av resurser 2 GB eller mindre. Du kan dock välja att konfigurera överföring av resurser som är större än 2 GB och upp till 15 GB.

Om du tänker använda den här funktionen bör du vara medveten om följande krav och punkter:

* Du måste köra Experience Manager 6.5 LTS i läget Dynamic Media - Scene7.
* Den här stora överföringsfunktionen stöds bara för [*Managed Services*](https://business.adobe.com/products/experience-manager/managed-services.html)-kunder.
* Kontrollera att din Experience Manager-instans är konfigurerad med Amazon S3 eller Microsoft® Azure Blob Storage.

  >[!NOTE]
  >
  >Konfigurera Azure Blob-lagringen med en åtkomstnyckel och en hemlig nyckel eftersom den här stora överföringsfunktionen inte stöds med AzureSas i Blob-lagringskonfigurationen.

* Oak [Direct Binary Access-hämtning](https://jackrabbit.apache.org/oak/docs/features/direct-binary-access.html) har aktiverats (Oak *Direct Binary Access-överföring* krävs inte).

  Om du vill aktivera hämtning av direkt binär åtkomst anger du egenskapen `presignedHttpDownloadURIExpirySeconds > 0` i datalagerkonfigurationen. Värdet måste vara tillräckligt långt för att ladda ned större binärfiler och eventuellt försöka igen.

* Assets som är större än 15 GB överförs inte. (Storleksgränsen anges i steg 8 nedan.)
* När arbetsflödet för **[!UICONTROL Dynamic Media Reprocess]**-resurser aktiveras för en mapp, bearbetas alla stora resurser som redan är synkroniserade med Dynamic Media-företaget om. Om stora resurser ännu inte har synkroniserats i mappen överförs inte resursen. Om du vill synkronisera befintliga stora resurser i Dynamic Media kan du därför köra arbetsflödet för **[!UICONTROL Dynamic Media Reprocess]**-resurser på enskilda resurser.

**Så här konfigurerar du läget Dynamic Media - Scene7 för överföring av resurser som är större än 2 GB:**

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och går sedan till **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL CRXDE Lite]**.

1. Gör något av följande i CRXDE Lite-fönstret:

   * Navigera till följande sökväg i den vänstra listen:

     `/libs/dam/gui/content/assets/jcr:content/actions/secondary/create/items/fileupload`

   * Kopiera och klistra in sökvägen ovanför i CRXDE Lite sökvägsfält nedanför verktygsfältet och tryck sedan på `Enter`.

1. Högerklicka `fileupload` i den vänstra listen och välj sedan **[!UICONTROL Overlay Node]** på snabbmenyn.

   ![Alternativet Täcka över nod](/help/assets/assets-dm/uploadassets15gb_a.png)

1. I dialogrutan Overlay Node markerar du kryssrutan **[!UICONTROL Match Node Types]** för att aktivera (aktivera) alternativet och väljer sedan **[!UICONTROL OK]**.

   ![Dialogrutan Overlay Node](/help/assets/assets-dm/uploadassets15gb_b.png)

1. Gör något av följande i CRXDE Lite-fönstret:

   * Navigera till följande nodsökväg för övertäckning i den vänstra listen:

     `/apps/dam/gui/content/assets/jcr:content/actions/secondary/create/items/fileupload`

   * Kopiera och klistra in sökvägen ovanför i CRXDE Lite sökvägsfält nedanför verktygsfältet och tryck sedan på `Enter`.

1. Gå till fliken **[!UICONTROL Properties]**, under kolumnen **[!UICONTROL Name]**, leta upp `sizeLimit`.
1. Till höger om namnet `sizeLimit` dubbelklickar du på värdefältet under kolumnen **[!UICONTROL Value]**.
1. Ange lämpligt värde i byte så att du kan öka storleksgränsen till den önskade överföringsstorleken. Om du till exempel vill öka storleken på överföringsresursen till 10 GB anger du `10737418240` i värdefältet.
Du kan ange ett värde på upp till 15 GB (`2013265920` byte). I det här fallet överförs inte överförda resurser som är större än 15 GB.

   ![Gränsvärde för storlek](/help/assets/assets-dm/uploadassets15gb_c.png)

1. I det övre vänstra hörnet av CRXDE Lite-fönstret väljer du **[!UICONTROL Save All]**.

   *Ange nu timeout för arbetsflödets hanterare för externa processer i Adobe Granite genom att göra följande:*

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen.
1. Gör något av följande:

   * Navigera till följande URL-sökväg:

     `localhost:4502/system/console/configMgr/com.adobe.granite.workflow.core.job.ExternalProcessJobHandler`

   * Kopiera och klistra in sökvägen ovanför i URL-fältet i webbläsaren. Se till att du ersätter `localhost:4502` med din egen Experience Manager-instans.

1. I dialogrutan **[!UICONTROL Adobe Granite Workflow External Process Job Handler]** anger du värdet till `18000` sekunder (fem timmar) i fältet **[!UICONTROL Max Timeout]**. Standardvärdet är 10 800 sekunder (tre timmar).

   ![Högsta timeout-värde](/help/assets/assets-dm/uploadassets15gb_d.png)

1. Välj **[!UICONTROL Save]** i dialogrutans nedre högra hörn.

   *Ange nu tidsgränsen för steget för processen Direktöverföring av binär fil i Scene7 genom att göra följande:*

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen.
1. Navigera till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. Välj **[!UICONTROL Dynamic Media Encode Video]** på sidan Arbetsflödesmodeller.
1. Välj **[!UICONTROL Edit]** i verktygsfältet.
1. Dubbelklicka på processsteget **[!UICONTROL Scene7 Direct Binary Upload]** på arbetsflödessidan.
1. I dialogrutan **[!UICONTROL Step Properties]**, under fliken **[!UICONTROL Common]**, under rubriken **[!UICONTROL Advanced Settings]** i fältet **[!UICONTROL Timeout]**, anger du värdet `18000` sekunder (fem timmar). Standardvärdet är `3600` sekunder (en timme).
1. Välj **[!UICONTROL OK]**.
1. Välj **[!UICONTROL Sync]**.
1. Upprepa steg 14-21 för arbetsflödesmodellen **[!UICONTROL DAM Update Asset]** och arbetsflödesmodellen **[!UICONTROL Dynamic Media Reprocess]**.

### (Valfritt) Installation och konfiguration av Dynamic Media - Scene7-lägesinställningar {#optional-setup-and-configuration-of-dynamic-media-scene7-mode-settings}

<!-- When you are in run mode `dynamicmedia_scene7`, use the Dynamic Media Classic user interface to change your Dynamic Media settings. -->

* [Konfigurera Dynamic Media Publish Setup för Image Server](/help/assets/dm-publish-settings.md)
* [Konfigurera allmänna inställningar för dynamiska media](/help/assets/dm-general-settings.md)
* [Konfigurera färghantering](#configuring-color-management)
* [Redigera MIME-typer för format som stöds](#editing-mime-types-for-supported-formats)
* [Lägg till MIME-typer för format som inte stöds](#adding-mime-types-for-unsupported-formats)
* [Skapa gruppuppsättningsförinställningar för automatisk generering av bilduppsättningar och snurpuppsättningar](#creating-batch-set-presets-to-auto-generate-image-sets-and-spin-sets) (i Dynamic Media Classic användargränssnitt)

#### Konfigurera Dynamic Media Publish Setup för Image Server {#publishing-setup-for-image-server}

På sidan Dynamic Media Publish Setup anges standardinställningar som avgör hur resurser levereras från Adobe Dynamic Media-servrar till webbplatser eller program.

Se [Konfigurera inställningar för dynamisk mediepublicering för bildserver](/help/assets/dm-publish-settings.md).

#### Konfigurera allmänna inställningar för dynamiska media {#configuring-application-general-settings}

Konfigurera URL:en för det dynamiska mediet **[!UICONTROL Publish Server Name]** och URL:en för **[!UICONTROL Origin Server Name]**. Du kan också ange **[!UICONTROL Upload to Application]**-inställningar och **[!UICONTROL Default Upload Options]** alla baserat på ditt specifika användningsfall.

Se [Konfigurera allmänna inställningar för dynamiska media](/help/assets/dm-general-settings.md).

#### Konfigurera färghantering {#configuring-color-management}

Med färghanteringen i Dynamic Media kan du färgkorrigera resurser. Med färgkorrigering behåller inkapslade resurser färgmodellen (RGB, CMYK, Grå) och den inbäddade färgprofilen. När du begär en dynamisk återgivning korrigeras bildfärgen till målfärgrymden med hjälp av CMYK-, RGB- eller gråskaleutdata.

Se [Konfigurera bildförinställningar](/help/assets/managing-image-presets.md).

>[!NOTE]
>
>Som standard visas 15 renderingar när du väljer **[!UICONTROL Renditions]** och 15 visningsförinställningar när du väljer **[!UICONTROL Viewers]** i resursens detaljvy. Du kan öka den här gränsen. Se [Öka antalet bildförinställningar som visas](/help/assets/managing-image-presets.md#increasing-or-decreasing-the-number-of-image-presets-that-display) eller [Öka antalet visningsförinställningar som visas](/help/assets/managing-viewer-presets.md#increasing-the-number-of-viewer-presets-that-display).

#### Redigera MIME-typer för format som stöds {#editing-mime-types-for-supported-formats}

Du kan definiera vilka resurstyper som ska bearbetas av Dynamic Media och anpassa avancerade parametrar för resurshantering. Du kan till exempel ange parametrar för tillgångsbearbetning för att göra följande:

* Konvertera en Adobe PDF till en eCatalog-resurs.
* Konvertera ett Adobe Photoshop-dokument (.PSD) till en bannermallsresurs för personalisering.
* Rastrera en Adobe Illustrator-fil (.AI) eller en Adobe Photoshop Encapsulated PostScript®-fil (.EPS).
* [Videoprofiler](/help/assets/video-profiles.md) och [Bildprofiler](/help/assets/image-profiles.md) kan användas för att definiera bearbetning av videoklipp och bilder.

Se [Överför Assets](/help/assets/manage-assets.md#uploading-assets).

**Så här redigerar du MIME-typer för de format som stöds:**

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och går sedan till **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL CRXDE Lite]**.
1. Navigera till följande i den vänstra listen:

   `/conf/global/settings/cloudconfigs/dmscene7/jcr:content/mimeTypes`

   ![MIME-typer](assets/mimetypes.png)

1. Välj en mime-typ i mappen mimeTypes.
1. Till höger på CRXDE Lite-sidan i den nedre delen:

   * Dubbelklicka på fältet **[!UICONTROL enabled]**. Som standard är alla resursens MIME-typer aktiverade (inställda på **[!UICONTROL true]**), vilket innebär att resurserna synkroniseras till Dynamic Media för bearbetning. Om du vill utesluta den här resursens MIME-typ från bearbetningen ändrar du den här inställningen till **[!UICONTROL false]**.

   * Dubbelmarkera **[!UICONTROL jobParam]** om du vill öppna det associerade textfältet. I [MIME-typer som stöds](/help/assets/assets-formats.md#supported-mime-types) finns en lista över tillåtna värden för bearbetningsparametrar som du kan använda för en viss MIME-typ.

1. Gör något av följande:

   * Upprepa steg 3-4 för att redigera fler MIME-typer.
   * Välj **[!UICONTROL Save All]** på menyraden på CRXDE Lite-sidan.

1. I det övre vänstra hörnet av sidan väljer du **[!UICONTROL CRXDE Lite]** för att återgå till Experience Manager.

#### Lägga till MIME-typer för format som inte stöds {#adding-mime-types-for-unsupported-formats}

Du kan lägga till anpassade MIME-typer för format som inte stöds i Experience Manager Assets. Kontrollera att alla nya noder som du lägger till i CRXDE Lite inte tas bort av Experience Manager genom att flytta MIME-typen före `image_`. Kontrollera också att dess aktiverade värde är **[!UICONTROL false]**.

**Så här lägger du till MIME-typer för format som inte stöds:**

1. Navigera från Experience Manager till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.

   ![2019-08-02_16-13-14](assets/2019-08-02_16-13-14.png)

1. En ny flik i webbläsaren öppnas på sidan **[!UICONTROL Adobe Experience Manager Web Console Configuration]**.

   ![2019-08-02_16-17-29](assets/2019-08-02_16-17-29.png)

1. På sidan bläddrar du nedåt till namnet *Adobe CQ Scene7 Asset MIME type Service* enligt följande skärmbild. Till höger om namnet väljer du pennikonen **[!UICONTROL Edit the configuration values]**.

   ![2019-08-02_16-44-56](assets/2019-08-02_16-44-56.png)

1. På sidan **Adobe CQ Scene7 Asset MIME type Service** väljer du en plusteckenikon &lt;+>. Platsen i tabellen där du väljer plustecknet för att lägga till den nya mime-typen är enkel.

   ![2019-08-02_16-27-27](assets/2019-08-02_16-27-27.png)

1. Skriv `DWG=image/vnd.dwg` i det tomma textfältet som du just lade till.

   Exemplet `DWG=image/vnd.dwg` är endast till för demonstrationssyften. MIME-typen som du lägger till här kan vara ett annat format som inte stöds.

   ![2019-08-02_16-36-36](assets/2019-08-02_16-36-36.png)

1. Välj **[!UICONTROL Save]** i sidans nedre högra hörn.

   Nu kan du stänga webbläsarfliken som har den öppna konfigurationssidan för Adobe Experience Manager Web Console.

1. Gå tillbaka till webbläsarfliken som har din öppna Experience Manager-konsol.
1. Navigera från Experience Manager till **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL CRXDE Lite]**.

   ![2019-08-02_16-55-41](assets/2019-08-02_16-55-41.png)

1. Navigera till följande i den vänstra listen:

   `conf/global/settings/cloudconfigs/dmscene7/jcr:content/mimeTypes`

1. Dra MIME-typen `image_vnd.dwg` och släpp den direkt ovanför `image_` i trädet enligt följande skärmbild.

   ![crxdelite_cqdoc-14627](assets/crxdelite_cqdoc-14627.png)

1. Med mime-typen `image_vnd.dwg` fortfarande markerad dubbelmarkerar du värdet som ska öppnas i listrutan **[!UICONTROL Value]** på fliken **[!UICONTROL Properties]** i raden **[!UICONTROL enabled]** under kolumnrubriken **[!UICONTROL Value]**.
1. Skriv `false` i fältet (eller välj **[!UICONTROL false]** i listrutan).

   ![2019-08-02_16-60-30](assets/2019-08-02_16-60-30.png)

1. I det övre vänstra hörnet på CRXDE Lite-sidan väljer du **[!UICONTROL Save All]**.

#### Skapa gruppuppsättningsförinställningar för automatisk generering av bilduppsättningar och snurpuppsättningar {#creating-batch-set-presets-to-auto-generate-image-sets-and-spin-sets}

Använd förinställningar för gruppuppsättningar för att automatisera skapandet av bilduppsättningar eller snurruppsättningar medan resurser överförs till Dynamic Media.

Definiera först namnkonventionen för hur resurser grupperas i en uppsättning. Skapa sedan en förinställning för gruppuppsättning som är en unik, självständig uppsättning instruktioner. Det måste definiera hur uppsättningen ska skapas med bilder som matchar de definierade namnkonventionerna i förinställningsreceptet.

När du överför filer skapar Dynamic Media automatiskt en uppsättning med alla filer som matchar den definierade namnkonventionen i de aktiva förinställningarna.

##### Konfigurera standardnamn

Skapa en standardnamnkonvention som används i alla förinställda gruppuppsättningar. Den standardnamnkonvention som valts i definitionen av gruppuppsättningsförinställningen är troligen allt som ditt företag behöver för att gruppgenerera uppsättningar. En gruppuppsättningsförinställning skapas för att använda den standardnamnkonvention som du definierar. Du kan skapa så många gruppuppsättningsförinställningar med alternativa, anpassade namnkonventioner som behövs för en viss uppsättning innehåll om det finns ett undantag från den företagsdefinierade standardnamngivningen.

När det inte krävs någon standardnamnkonvention för att använda funktionen för gruppuppsättningsförinställningar bör du använda standardnamnkonventionen. Här kan du definiera så många element i namnkonventionen som du vill gruppera i en uppsättning så att du kan effektivisera skapandet av gruppuppsättningar.

Som ett alternativ kan du använda **[!UICONTROL View Code]** utan tillgängliga formulärfält. I den här vyn skapar du namnkonventionens definitioner helt med hjälp av reguljära uttryck.

Det finns två element för definition, Matcha och Basnamn. Med dessa fält kan du definiera alla element i en namnkonvention och identifiera den del av konventionen som används för att namnge den uppsättning i vilken de finns. Ett företags namnkonvention använder ofta en eller flera definitionsrader för vart och ett av dessa element. Du kan använda så många rader för din unika definition och gruppera dem i distinkta element, t.ex. för Huvudbild, Färgelement, Alternativa vyer och Färgruteelement.

**Så här konfigurerar du standardnamn:**

1. Öppna [Dynamic Media Classic-datorprogrammet](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/signing-out.html#getting-started) och logga sedan in på ditt konto.

   Dina inloggningsuppgifter och inloggningsuppgifter tillhandahölls av Adobe vid tidpunkten för etableringen. Om du inte har den här informationen kan du kontakta Adobe kundsupport.

1. Navigera till **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL Batch Set Presets]** > **[!UICONTROL Default Naming]** i navigeringsfältet uppe på sidan.
1. Välj **[!UICONTROL View Form]** eller **[!UICONTROL View Code]** för att ange hur du vill visa och ange information om varje element.

   Du kan markera kryssrutan **[!UICONTROL View Code]** om du vill visa värdeuppbyggnaden för reguljära uttryck tillsammans med dina formulärval. Du kan ange eller ändra dessa värden för att underlätta definitionen av elementen i namnkonventionen, om formulärvyn begränsar dig av någon anledning. Om dina värden inte kan tolkas i formulärvyn blir formulärfälten inaktiva.

   >[!NOTE]
   >
   >Inaktiverade formulärfält utför ingen validering av att reguljära uttryck är korrekta. Resultatet av det reguljära uttryck som du skapar för varje element efter resultatraden visas. Det fullständiga reguljära uttrycket visas längst ned på sidan.

1. Expandera varje element efter behov och ange de namnkonventioner som du vill använda.
1. Gör något av följande om det behövs:

   * Välj **[!UICONTROL Add]** om du vill lägga till en annan namnkonvention för ett element.
   * Välj **[!UICONTROL Remove]** om du vill ta bort en namnkonvention för ett element.

1. Gör något av följande:

   * Välj **[!UICONTROL Save As]** och ange ett namn för förinställningen.
   * Välj **[!UICONTROL Save]** om du redigerar en befintlig förinställning.

##### Skapa en förinställning för gruppuppsättning

I Dynamic Media används gruppuppsättningsförinställningar för att ordna resurser i uppsättningar med bilder (alternativa bilder, färgalternativ, 360-rotation) för visning i visningsprogram. Förinställningarna för gruppuppsättningar körs automatiskt tillsammans med överföringsprocesserna för resurser i Dynamic Media.

Du kan skapa, redigera och hantera dina gruppuppsättningsförinställningar. Det finns två typer av förinställda definitioner för gruppuppsättningar: en för en standardnamnkonvention som du kan konfigurera och en för anpassade namnkonventioner som du skapar direkt.

Du kan antingen använda formulärfältsmetoden för att definiera en gruppuppsättningsförinställning eller kodmetoden, som gör att du kan använda reguljära uttryck. Precis som i Standardnamn kan du välja Visa kod samtidigt som du definierar i formulärvyn och använda reguljära uttryck för att skapa definitioner. Du kan också avmarkera en vy om du vill använda den ena eller den andra enbart.

**Så här skapar du en gruppuppsättningsförinställning:**

1. Öppna [Dynamic Media Classic-datorprogrammet](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/signing-out.html#getting-started) och logga sedan in på ditt konto.

   Dina inloggningsuppgifter och inloggningsuppgifter tillhandahölls av Adobe vid tidpunkten för etableringen. Om du inte har den här informationen kan du kontakta Adobe kundsupport.

1. Navigera till **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL Batch Set Presets]** > **[!UICONTROL Batch Set Preset]** i navigeringsfältet uppe på sidan.

   **[!UICONTROL View Form]**, som anges i det övre högra hörnet på detaljsidan, är standardvyn.

1. På panelen Förinställningslista väljer du **[!UICONTROL Add]** för att aktivera definitionsfälten på panelen Detaljer till höger på skärmen.
1. Skriv ett namn på förinställningen i fältet Förinställningsnamn på panelen Detaljer.
1. Välj en förinställningstyp i listrutan Gruppuppsättningstyp.
1. Gör något av följande:

   * Om du använder en standardnamnkonvention som du tidigare ställt in under **[!UICONTROL Application Setup]** > **[!UICONTROL Batch Set Presets]** > **[!UICONTROL Default Naming]** expanderar du **[!UICONTROL Asset Naming Conventions]** och väljer **[!UICONTROL Default]** i listrutan Namnge fil.

   * Om du vill definiera en ny namnkonvention när du konfigurerar förinställningen expanderar du **[!UICONTROL Asset Naming Conventions]** och väljer sedan **[!UICONTROL Custom]** i listrutan Namnge fil.

1. I Sekvensordning definierar du i vilken ordning bilderna ska visas efter att uppsättningen har grupperats i Dynamic Media.

   Som standard sorteras dina resurser alfanumeriskt. Du kan dock använda en kommaavgränsad lista med reguljära uttryck för att definiera ordningen.

1. Ange suffixet eller prefixet till basnamnet som du definierade i konventionen om namngivning av tillgångar för Ange namngivning och skapande av konvention. Definiera också var uppsättningen skapas i mappstrukturen Dynamiska media.

   Om du definierar ett stort antal uppsättningar ska uppsättningarna hållas åtskilda från de mappar som innehåller själva resurserna. Skapa till exempel en mapp för bilduppsättningar och placera genererade uppsättningar här.

1. Välj **[!UICONTROL Save]** på detaljpanelen.
1. Välj **[!UICONTROL Active]** bredvid den nya förinställningens namn.

   När du aktiverar förinställningen används den för att generera uppsättningen när du överför resurser till Dynamic Media.

##### Skapa en gruppuppsättningsförinställning för automatisk generering av en 2D-snurpuppsättning

Du kan använda gruppuppsättningstypen **[!UICONTROL Multi-Axis Spin Set]** för att skapa ett recept som automatiserar genereringen av 2D-snurpuppsättningar. Vid gruppering av bilder används reguljära uttryck för rad och kolumn så att bildresurserna justeras korrekt på motsvarande plats i den flerdimensionella arrayen. Det finns inget minsta eller högsta antal rader eller kolumner som du måste ha i en rotationsuppsättning med flera axlar.

Anta till exempel att du vill skapa en fleraxelsnurvuppsättning med namnet `spin-2dspin`. Du har en uppsättning bilder med snurra uppsättningar som innehåller tre rader, med 12 bilder per rad. Bilderna får följande namn:

```xml {.line-numbers}
spin-01-01
 spin-01-02
 …
 spin-01-12
 spin-02-01
 …
 spin-03-12
```

Med den här informationen kan du skapa ditt recept för gruppuppsättningstyp enligt följande:

![chlimage_1-560](assets/chlimage_1-560.png)

Gruppering för den delade resursnamndelen i rotationsuppsättningen läggs till i fältet **[!UICONTROL Match]** (markerat). Variabeldelen av resursnamnet som innehåller raden och kolumnen läggs till i fälten **[!UICONTROL Row]** och **[!UICONTROL Column]**.

När rotationsuppsättningen överförs och publiceras aktiverar du namnet på det 2D-rotationskopia som visas under **[!UICONTROL Batch Set Presets]** i dialogrutan **[!UICONTROL Upload Job Options]**.

**Så här skapar du en gruppuppsättningsförinställning för automatisk generering av en 2D-snurpuppsättning:**

1. Öppna [Dynamic Media Classic-datorprogrammet](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/signing-out.html#getting-started) och logga sedan in på ditt konto.

   Dina inloggningsuppgifter och inloggningsuppgifter tillhandahölls av Adobe vid tidpunkten för etableringen. Om du inte har den här informationen kan du kontakta Adobe kundsupport.

1. Navigera till **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL Batch Set Presets]** > **[!UICONTROL Batch Set Preset]** i navigeringsfältet uppe på sidan.

   **[!UICONTROL View Form]**, som anges i det övre högra hörnet på detaljsidan, är standardvyn.

1. På panelen Förinställningslista väljer du **[!UICONTROL Add]** för att aktivera definitionsfälten på panelen Detaljer till höger på skärmen.
1. Skriv ett namn på förinställningen i fältet Förinställningsnamn på panelen Detaljer.
1. I listrutan Gruppuppsättningstyp väljer du **[!UICONTROL Asset Set]**.
1. Välj **[!UICONTROL Multi-Axis Spin Set]** i listrutan Undertyp.
1. Expandera **[!UICONTROL Asset Naming Conventions]** och välj sedan **[!UICONTROL Custom]** i listrutan Namnge fil.
1. Använd attributen **[!UICONTROL Match]** och eventuellt **[!UICONTROL Base Name]** för att definiera ett reguljärt uttryck för namngivning av bildresurser som utgör grupperingen.

   Det reguljära uttrycket för literal Match kan till exempel se ut så här:

   `(w+)-w+-w+`

1. Expandera **[!UICONTROL Row Column Position]** och definiera sedan namnformatet för bildresursens position i 2D-rotationsmatrisen.

   Använd parentesen för att omsluta rad- eller kolumnpositionen i filnamnet.

   För en rad med ett reguljärt uttryck kan det se ut så här:

   `\w+-R([0-9]+)-\w+`

   eller

   `\w+-(\d+)-\w+`

   För det reguljära uttrycket i kolumnen kan det se ut så här:

   `\w+-\w+-C([0-9]+)`

   eller

   `\w+-\w+-C(\d+)`

   Proverna ovan är endast avsedda som exempel. Du kan skapa det reguljära uttrycket hur du vill.

   >[!NOTE]
   >
   >Om kombinationen av reguljära uttryck för rader och kolumner inte kan avgöra positionen för resursen i den flerdimensionella rotationsinställningsarrayen, läggs resursen inte till i uppsättningen. Ett fel loggas också.

1. Ange suffixet eller prefixet till basnamnet som du definierade i konventionen om namngivning av tillgångar för Ange namngivning och skapande av konvention.

   Du kan även definiera var rotationsuppsättningen ska skapas i mappstrukturen för Dynamic Media Classic.

   Om du definierar ett stort antal uppsättningar ska uppsättningarna hållas åtskilda från de mappar som innehåller själva resurserna. Du kan till exempel skapa en mapp för snurruppsättningar där du kan placera genererade uppsättningar här.

1. Välj **[!UICONTROL Save]** på detaljpanelen.
1. Välj **[!UICONTROL Active]** bredvid den nya förinställningens namn.

   När du aktiverar förinställningen används den för att generera uppsättningen när du överför resurser till Dynamic Media.

### (Valfritt) Justera prestanda för Dynamic Media - Scene7-läge {#optional-tuning-the-performance-of-dynamic-media-scene-mode}

För att Dynamic Media - Scene7-läget ska fungera smidigt rekommenderar Adobe följande finjusteringstips för synkroniseringsprestanda/skalbarhet:

* Uppdaterar de fördefinierade jobbparametrarna för bearbetning av olika filformat.
* Uppdaterar de fördefinierade arbetstrådarna för Granite-arbetsflödet (videoresurser).
* Uppdaterar det fördefinierade tillfälliga Granite-arbetsflödet (bilder och icke-videomaterial) för köarbetstrådar.
* Uppdaterar de maximala överföringsanslutningarna till Dynamic Media Classic-servern.

#### Uppdatera fördefinierade jobbparametrar för bearbetning av olika filformat

Du kan justera jobbparametrar för snabbare bearbetning när du överför filer. Om du till exempel överför PSD-filer, men inte vill bearbeta dem som mallar, kan du ange att lagerextraheringen ska vara false (av). I så fall visas den justerade jobbparametern enligt följande: `process=None&createTemplate=false`.

Om du vill aktivera mallskapande använder du följande parametrar: `process=MaintainLayers&layerNaming=AppendName&createTemplate=true`.

<!-- THIS PARAGRAPH WAS REPLACED WITH THE TWO PARAGRAPHS DIRECTLY ABOVE BASED ON CQDOC-17657 You can tune job parameters for faster processing when you upload files. For example, if you are uploading PSD files, but do not want to process them as templates, you can set layer extraction to false (off). In such case, the tuned job parameter would appear as `process=None&createTemplate=false`. -->

Adobe rekommenderar att du använder följande&quot;justerade&quot; jobbparametrar för PDF-, PostScript®- och PSD-filer:

<!-- OLD PDF JOB PARAMETERS `pdfprocess=Rasterize&resolution=150&colorspace=Auto&pdfbrochure=false&keywords=false&links=false` -->

<!-- OLD POSTSCRIPT JOB PARAMETERS `psprocess=Rasterize&psresolution=150&pscolorspace=Auto&psalpha=false&psextractsearchwords=false&aiprocess=Rasterize&airesolution=150&aicolorspace=Auto&aialpha=false` -->

| Filtyp | Rekommenderade jobbparametrar |
| ---| ---|
| PDF | `pdfprocess=Thumbnail&resolution=150&colorspace=Auto&pdfbrochure=false&keywords=false&links=false` |
| PostScript® | `psprocess=Rasterize&psresolution=150&pscolorspace=Auto&psalpha=false&psextractsearchwords=false&aiprocess=Thumbnail&airesolution=150&aicolorspace=Auto&aialpha=false` |
| PSD | `process=None&layerNaming=AppendName&anchor=Center&createTemplate=false&extractText=false&extendLayers=false` |

<!-- CQDOC-17657 for PSD entry in table above -->

Om du vill uppdatera någon av de här parametrarna följer du stegen i [Aktivera stöd för MIME-typbaserade Assets/Dynamic Media Classic-överföringsjobbparametrar](/help/sites-administering/scene7.md#enabling-mime-type-based-assets-scene-upload-job-parameter-support).

#### Uppdatera den tillfälliga arbetsflödeskön för Granite {#updating-the-granite-transient-workflow-queue}

Bevilja transittjänstens arbetsflödeskö används för arbetsflödet **[!UICONTROL DAM Update Asset]**. I Dynamic Media används den för bildinläsning och bearbetning.

**Så här uppdaterar du den tillfälliga Bevilja-arbetsflödeskön:**

1. Navigera till [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr) och sök efter **Kö: Bevilja tillfällig arbetsflödeskö**.

   >[!NOTE]
   >
   >En textsökning är nödvändig i stället för en direkt URL eftersom OSGi PID genereras dynamiskt.

1. Ändra talet till det önskade värdet i fältet **[!UICONTROL Maximum Parallel Jobs]**.

   Du kan öka **[!UICONTROL Maximum Parallel Jobs]** om du vill ha tillräckligt stöd för stor överföring av filer till Dynamic Media. Det exakta värdet beror på maskinvarukapaciteten. I vissa scenarier, d.v.s. en inledande migrering eller en massöverföring som görs en gång, kan du använda ett stort värde. Tänk dock på att användning av ett stort värde (till exempel två gånger antalet kärnor) kan ha negativa effekter på andra samtidiga aktiviteter. Testa och justera värdet utifrån ditt specifika användningsfall.

<!--    By default, the maximum number of parallel jobs depends on the number of available CPU cores. For example, on a 4-core server, it assigns 2 worker threads. (A value between 0.0&ndash;1.0 is ratio based, or any numbers greater than 1 will assign the number of worker threads.)

   Adobe recommends that 32 **[!UICONTROL Maximum Parallel Jobs]** be configured to adequately support heavy upload of files to Dynamic Media Classic (Scene7). -->

![chlimage_1](assets/chlimage_1.jpeg)

1. Välj **[!UICONTROL Save]**.

#### Uppdatera arbetsflödeskön för Granite {#updating-the-granite-workflow-queue}

Beviljad arbetsflödeskö används för icke-tillfälliga arbetsflöden. I Dynamic Media brukade den bearbeta video med arbetsflödet **[!UICONTROL Dynamic Media Encode Video]**.

**Så här uppdaterar du arbetsflödeskön Granite:**

1. Navigera till `https://<server>/system/console/configMgr` och sök efter **Kö: Begränsa arbetsflödeskö**.

   >[!NOTE]
   >
   >En textsökning är nödvändig i stället för en direkt URL eftersom OSGi PID genereras dynamiskt.

1. Ändra talet till det önskade värdet i fältet **[!UICONTROL Maximum Parallel Jobs]**.

   Du kan öka det maximala antalet parallella jobb för att få tillräckligt stöd för överföring av filer till Dynamic Media. Det exakta värdet beror på maskinvarukapaciteten. I vissa scenarier, d.v.s. en inledande migrering eller en massöverföring som görs en gång, kan du använda ett stort värde. Tänk dock på att användning av ett stort värde (till exempel två gånger antalet kärnor) kan ha negativa effekter på andra samtidiga aktiviteter. Testa och justera värdet utifrån ditt specifika användningsfall.

   ![chlimage_1-1](assets/chlimage_1-1.jpeg)

1. Välj **[!UICONTROL Save]**.

#### Uppdatera Dynamic Media Classic överföringsanslutning {#updating-the-scene-upload-connection}

Inställningen Scene7 Upload Connection synkroniserar Experience Manager-resurser med Dynamic Media Classic-servrar.

**Så här uppdaterar du Dynamic Media Classic överföringsanslutning:**

1. Navigera till `https://<server>/system/console/configMgr/com.day.cq.dam.scene7.impl.Scene7UploadServiceImpl`
1. I fältet **[!UICONTROL Number of connections]** och/eller fältet **[!UICONTROL Active job timeout]** ändrar du numret efter behov.

   Inställningen **[!UICONTROL Number of connections]** styr det maximala antalet HTTP-anslutningar som tillåts för överföring av Experience Manager till dynamiska media. Normalt är det fördefinierade värdet på tio anslutningar tillräckligt.

   Inställningen **[!UICONTROL Active job timeout]** avgör väntetiden för överförda Dynamic Media-resurser som ska publiceras på leveransservern. Det här värdet är som standard 2 100 sekunder (35 minuter).

   I de flesta fall räcker det med inställningen 2 100.

   ![chlimage_1-2](assets/chlimage_1-2.jpeg)

1. Välj **[!UICONTROL Save]**.

### (Valfritt) Filtrera resurser för replikering {#optional-filtering-assets-for-replication}

I distributioner av icke-dynamiska media replikerar du *alla*-resurser (både bilder och video) från din Experience Manager-redigeringsmiljö till Experience Manager Publish-noden. Det här arbetsflödet är nödvändigt eftersom Experience Manager Publish-servrarna också levererar resurserna.

I Dynamic Media-distributioner behöver du dock inte replikera samma resurser till Experience Manager publiceringsnoder eftersom resurserna levereras via Cloud Service. Ett sådant&quot;hybridpubliceringsarbetsflöde&quot; undviker extra lagringskostnader och längre bearbetningstider för att replikera resurser. Annat innehåll, t.ex. webbplatssidor, fortsätter att hanteras från Experience Manager publiceringsnoder.

Med filtren kan du *exkludera* resurser från att replikeras till Experience Manager publiceringsnod.

#### Använd standardfiltren för replikering {#using-default-asset-filters-for-replication}

Om du använder Dynamic Media för bilder, video eller båda, kan du använda de standardfilter som Adobe tillhandahåller i befintligt skick. Följande filter är aktiva som standard:

|   | Filter | Mime-typ | Återgivningar |
| --- | --- | --- | --- |
| Dynamic Media Image Delivery | filter-bild<br>filteruppsättningar | Börjar med **image/**<br> Innehåller **program/** och slutar med **set**. | De färdiga&quot;filterbilderna&quot; (gäller för enstaka bildresurser, inklusive interaktiva bilder) och&quot;filteruppsättningar&quot; (gäller för snurruppsättningar, bilduppsättningar, blandade medieuppsättningar och Carousel-uppsättningar) kommer att:<br> ・ Uteslut från replikering av den ursprungliga bilden och statiska bildåtergivningar. |
| Dynamic Media Video Delivery | filter-video | Börjar med **video/** | Den färdiga filtervideon:<br> ・ Uteslut replikering av den ursprungliga videon och statiska miniatyråtergivningar. |

>[!NOTE]
>
>Filter gäller för MIME-typer och kan inte vara sökvägsspecifika.

#### Anpassa resursfilter för replikering {#customizing-asset-filters-for-replication}

1. I Experience Manager väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen och navigerar till **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL CRXDE Lite]**.
1. Navigera till `/etc/replication/agents.author/publish/jcr:content/damRenditionFilters` i det vänstra mappträdet för att granska filtren.

   ![chlimage_1-17](assets/chlimage_1-2.png)

1. Du definierar Mime-typen för filtret genom att leta reda på Mime-typen enligt följande:

   Expandera `content > dam > <locate_your_asset> > jcr:content > metadata` i den vänstra listen och gå sedan till `dc:format` i tabellen.

   Följande bild är ett exempel på en resurs sökväg till `dc:format`.

   ![chlimage_1-18](assets/chlimage_1-3.png)

   Observera att `dc:format` för resursen `Fiji Red.jpg` är `image/jpeg`.

   Om du vill att det här filtret ska gälla för alla bilder, oavsett format, anger du värdet till `image/*` där `*` är ett reguljärt uttryck som används för alla bilder i alla format.

   Om du bara vill att filtret ska gälla för bilder av typen JPEG anger du värdet `image/jpeg`.

1. Definiera vilka renderingar du vill inkludera eller exkludera från replikering.

   Följande tecken kan användas för att filtrera replikering:

   | Tecken som ska användas | Så här filtrerar du resurser för replikering |
   | --- | --- |
   | * | Jokertecken |
   | + | Innehåller resurser för replikering |
   | - | Exkluderar resurser från replikering |

   Navigera till `content/dam/<locate your asset>/jcr:content/renditions`.

   Följande grafik är ett exempel på en resurs återgivningar.

   ![chlimage_1-4](assets/chlimage_1-4.png)

   Om du bara vill replikera originalet anger du `+original`.
