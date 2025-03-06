---
title: Hämta resurser
description: Lär dig hur du hämtar resurser från [!DNL Adobe Experience Manager] och aktiverar eller inaktiverar hämtningsfunktionen.
contentOwner: AG
role: User
feature: Asset Management,Asset Distribution
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: 89b14351-c689-42a6-bd89-cc258f601898
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 0%

---

# Hämta resurser från [!DNL Adobe Experience Manager] {#download-assets-from-aem}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/download-assets-from-aem.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Du kan hämta resurser, inklusive statiska och dynamiska återgivningar. Du kan också skicka e-postmeddelanden med länkar till resurser direkt från [!DNL Adobe Experience Manager Assets]. Hämtade resurser paketeras i en ZIP-fil. Den komprimerade ZIP-filen har en maximal filstorlek på 1 GB för exportjobbet. Högst 500 resurser per exportjobb tillåts.

>[!NOTE]
>
>Alla användare som har läsbehörighet på platsen `/var/dam/share` har åtkomst till den nedladdningslänk som delas i e-postmeddelandet.
>
>Alla användare som har läsbehörighet till platsen `/var/dam/jobs/download` kan hämta resurser.
>
>Det går inte att hämta resurstyperna - Bilduppsättningar, Snurra uppsättningar, Blandade medieuppsättningar och Carousel-uppsättningar.

<!--
OLD content of the above NOTE, changed wrt CQDOC-18661.
>The email recipients must be members of the `dam-users` group to access the ZIP download link in the email message.
>
-->

**Så här hämtar du resurser:**

1. Klicka på logotypen i det övre vänstra hörnet. Klicka på **[!UICONTROL Navigation]** i den vänstra listen.
1. På sidan [!UICONTROL Navigation] klickar du på **[!UICONTROL Assets]** > **[!UICONTROL Files]**.
1. Navigera till en mapp som innehåller resurser som du vill hämta.
1. Markera mappen eller välj en eller flera resurser i mappen.
1. Klicka på **[!UICONTROL Download]** i verktygsfältet.
1. I dialogrutan Hämta väljer du de hämtningsalternativ som du vill använda.

   | Alternativet Exportera eller ladda ned | Beskrivning |
   |---|---|
   | **[!UICONTROL Create separate folder for each asset]** | Välj det här alternativet om du vill inkludera varje resurs som du hämtar, inklusive resurser, i underordnade mappar som är kapslade under resursens överordnade mapp i en mapp på den lokala datorn. När det här alternativet inte är markerat ignoreras mapphierarkin som standard och alla resurser hämtas till en mapp på den lokala datorn. |
   | **[!UICONTROL Email]** | Ett e-postmeddelande skickas till användaren. Standardmallar för e-post finns på följande platser:<ul><li>`/libs/settings/dam/workflow/notification/email/downloadasset`.</li><li>`/libs/settings/dam/workflow/notification/email/transientworkflowcompleted`.</li></ul> Mallar som du anpassar under distributionen finns på följande platser: <ul><li>`/apps/settings/dam/workflow/notification/email/downloadasset`.</li><li>`/apps/settings/dam/workflow/notification/email/transientworkflowcompleted`.</li></ul>Du kan lagra klientspecifika anpassade mallar på följande platser:<ul><li>`/conf/<tenant_specific_config_root>/settings/dam/workflow/notification/email/downloadasset`.</li><li>`/conf/<tenant_specific_config_root>/settings/dam/workflow/notification/email/transientworkflowcompleted`.</li></ul> |
   | **[!UICONTROL Asset(s)]** | Välj det här alternativet om du vill hämta resursen i dess ursprungliga form utan några återgivningar.<br>Alternativet för delresurser är tillgängligt om den ursprungliga resursen har delresurser. |
   | **[!UICONTROL Rendition(s)]** | En återgivning är den binära representationen av en resurs. Assets har en primär representation - den överförda filen. De kan ha ett valfritt antal representationer. <br> Med det här alternativet kan du välja de återgivningar du vill hämta. Vilka återgivningar som är tillgängliga beror på vilken resurs du väljer. Alternativet är tillgängligt om resursen har några återgivningar. |
   | **[!UICONTROL Smart Crops]** | Välj det här alternativet om du vill hämta alla smarta beskärningsåtergivningar av den valda resursen från AEM. En ZIP-fil med renderingarna Smart Crop skapas och hämtas till din lokala dator. |
   | **[!UICONTROL Dynamic Rendition(s)]** | Välj det här alternativet om du vill generera en serie alternativa återgivningar i realtid. När du väljer det här alternativet väljer du också de återgivningar som du vill skapa dynamiskt genom att välja i listan [Bildförinställning](image-presets.md). <br>Du kan dessutom välja storlek och måttenhet, format, färgrymd, upplösning och valfria bildmodifierare som att invertera bilden. Alternativet är bara tillgängligt om du har [!DNL Dynamic Media] aktiverat. |

1. Klicka på **[!UICONTROL Download]** i dialogrutan.

När du väljer en mapp att hämta hämtas hela resurshierarkin under mappen. Om du vill inkludera varje resurs som du hämtar (inklusive resurser i underordnade mappar som är kapslade under den överordnade mappen) i en enskild mapp väljer du **[!UICONTROL Create separate folder for each asset]**.

## Aktivera resurshämtningsserver {#enable-asset-download-servlet}

Standardservern i [!DNL Experience Manager] gör att autentiserade användare kan utfärda godtyckligt stora, samtidiga hämtningsbegäranden för att skapa ZIP-filer med resurser som är synliga för dem och som kan överbelasta servern och nätverket. För att minska de potentiella DoS-riskerna som orsakas av den här funktionen är `AssetDownloadServlet` OSGi-komponenten inaktiverad som standard för publiceringsinstanser.

Om du vill tillåta hämtning av resurser från DAM, till exempel när du använder Assets Share Commons eller någon annan portalliknande implementering, aktiverar du servleten manuellt med hjälp av en OSGi-konfiguration. Adobe rekommenderar att du anger en så låg hämtningsstorlek som möjligt utan att det påverkar kraven för den dagliga hämtningen. Ett högt värde kan påverka prestandan.

1. Skapa en mapp med en namnkonvention som anger publiceringsmiljön (`config.publish`) som mål: `/apps/<your-app-name>/config.publish`. Mer information om hur du definierar konfigurationsegenskaper för ett körningsläge finns i [Körningslägen](/help/sites-deploying/configure-runmodes.md#defining-configuration-properties-for-a-run-mode).
1. Skapa en fil av typen `nt:file` med namnet `com.day.cq.dam.core.impl.servlet.AssetDownloadServlet.config` i konfigurationsmappen.
1. Fyll i `com.day.cq.dam.core.impl.servlet.AssetDownloadServlet.config` med följande: Anger en maximal storlek (i byte) för hämtningen som värdet `asset.download.prezip.maxcontentsize`. Nedanstående exempel konfigurerar den maximala storleken för ZIP-nedladdningen till högst 100 kB.

   ```conf
   enabled=B"true"
   asset.download.prezip.maxcontentsize=I"102400"
   ```

Som standard har `GET` förfrågningar om att hämta filer, vilket innebär att [!DNL Experience Manager] har en begränsning på 50 MB för ZIP-arkivets hämtningsstorlek. Hämtningar som initierats via `POST`-begäranden eller användargränssnittet påverkas inte av den här gränsen.

## Inaktivera resurshämtningsserver {#disable-asset-download-servlet}

`Asset Download Servlet` kan inaktiveras på en [!DNL Experience Manager] Publish-instans genom att uppdatera dispatcherns konfiguration för att blockera eventuella hämtningsbegäranden. Servern kan även inaktiveras manuellt via OSGi-konsolen direkt.

1. Om du vill blockera resurshämtningsbegäranden via en dispatcherkonfiguration redigerar du `dispatcher.any`-konfigurationen och lägger till en regel i [filteravsnittet](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#defining-a-filter). `/0100 { /type "deny" /url "*.assetdownload.zip/assets.zip*" }`

1. Om du vill inaktivera OSGi-komponenten på en publiceringsinstans öppnar du OSGi-konsolen på `http://[aem_server]:[port]/system/console/components`. Leta reda på `com.day.cq.dam.core.impl.servlet.AssetDownloadServlet` och klicka på **[!UICONTROL Disable]**.

>[!MORELIKETHIS]
>
>* [Hämta resurser med Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/download/brand-portal-download-assets.html)
>* [Hämta DRM-skyddade resurser](drm.md).
>* [Hämta resurser med Experience Manager skrivbordsapp på Windows- eller Mac-datorprogrammet](https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html#download-assets).
>* [Hämta resurser med Adobe Assets Link från de Adobe Creative Cloud-appar som stöds](https://helpx.adobe.com/se/enterprise/using/manage-assets-using-adobe-asset-link.html).
