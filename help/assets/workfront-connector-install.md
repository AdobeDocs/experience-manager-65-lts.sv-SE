---
title: Installera [!DNL Workfront for Experience Manager enhanced connector]
description: Installera [!DNL Workfront for Experience Manager enhanced connector]
role: Admin
feature: Workfront Integrations and Apps
hide: true
solution: Experience Manager, Workfront
exl-id: dd6eec1e-fa63-410a-bcd3-61892861fd0c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 1%

---

# Installera [!DNL Workfront for Experience Manager enhanced connector] {#assets-integration-overview}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/integrations/workfront-connector-install.html?lang=en) |
| AEM 6.5 | Den här artikeln |

En användare med administratörsåtkomst i [!DNL Adobe Experience Manager] installerar den utökade anslutningen. Granska plattformsstödet och andra [krav för anslutningen](https://one.workfront.com/s/csh?context=2467&amp;pubname=the-new-workfront-experience) innan du installerar.

>[!IMPORTANT]
>
>* Adobe kräver att [!DNL Adobe Workfront for Experience Manager enhanced connector] distribueras och konfigureras endast via certifierade partners eller [!DNL Adobe Professional Services]. Om den distribueras och konfigureras utan en certifierad partner eller [!DNL Adobe Professional Services] stöds den inte av Adobe.
>
>* Adobe kan släppa uppdateringar av [!DNL Adobe Workfront] och [!DNL Adobe Experience Manager] som gör den här kopplingen överflödig. Om detta inträffar kan kunderna behöva gå över från att använda den här anslutningen.
>
>* Adobe har stöd för utökade anslutningsversioner 1.7.4 och senare. Tidigare förhandsversioner och anpassade versioner stöds inte. Om du vill kontrollera den utökade anslutningsversionen går du till gruppen `digital.hoodoo` som finns i den vänstra rutan i [Pakethanteraren](https://experienceleague.adobe.com/docs/experience-manager-65-lts/administering/contentmanagement/package-manager.html?lang=en).
>
>* Se [Partnercertifieringsprov för Workfront för Experience Manager Assets förbättrade anslutningsprogram](https://solutionpartners.adobe.com/solution-partners/home/applications/experience_cloud/workfront/journey/dev_core.html). Mer information om provet finns i [Handbok för prov](https://express.adobe.com/page/Tc7Mq6zLbPFy8/).

Så här installerar du kopplingen:

1. Hämta kopplingen från [[!DNL Software Distribution] link](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/product/assets/workfront-tools.ui.apps.zip).
1. [Konfigurera brandväggen](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FAdministration_and_Setup%2FGet_started-WF_administration%2Fconfigure-your-firewall.html).
1. På Dispatcher tillåter du HTTP-huvuden med namnen `authorization`, `username` och `apikey`. Tillåt `GET`, `POST` och `PUT` förfrågningar till `/bin/workfront-tools`.
1. Kontrollera att följande sökvägar inte finns i databasen [!DNL Experience Manager]:

   * `/apps/dam/gui/coral/components/admin/schemaforms/formbuilder`
   * `/apps/dam/gui/coral/components/admin/folderschemaforms/formbuilder`
   * `/apps/dam/gui/content/foldermetadataschemaeditor`
   * `/apps/dam/cfm/models/editor/components/datatypeproperties`
   * `/apps/settings/dam/cfm/models/formbuilderconfig`

1. Installera paketet med [!UICONTROL Package Manager]. Mer information om hur du installerar paket finns i [Pakethanterarens dokumentation](/help/sites-administering/package-manager.md).
1. Skapa `wf-workfront-users` i [!DNL Experience Manager]-användargruppen och tilldela behörigheten `jcr:all` till `/content/dam`.
1. Lägg till en anpassad egenskap i indexdefinitionen utanför rutan för **`ntFolderDamLucene(/oak:index/ntFolderDamLucene)`**. Utför följande steg:
   * Lägg till en **`nt:unstructured`**-egenskap med namnet **`wfReferenceNumber`** i:

     `/oak:index/ntFolderDamLucene/indexRules/nt:folder/properties/wfReferenceNumber`.
   * Indexera om `index /oak:index/ntFolderDamLucene` genom att vända indexeringsflaggan till `true`.

En systemanvändare `workfront-tools` skapas automatiskt och de behörigheter som krävs hanteras automatiskt. Alla användare från [!DNL Workfront] som använder anslutningen läggs till automatiskt som en del av den här gruppen.

## Konfigurera anslutningen mellan [!DNL Experience Manager] och [!DNL Workfront] {#configure-connection}

Så här skapar du en anslutning till Workfront:

1. I [!DNL Experience Manager] väljer du **[!UICONTROL Tools]** > **[!UICONTROL Cloud Services]** > **[!UICONTROL Workfront Tools Configuration]**.

1. Välj `workfront-tools` i den vänstra panelen och välj alternativet **[!UICONTROL Create]** i sidans övre högra del.

1. I dialogrutan **[!UICONTROL Workfront Connection]** anger du nödvändig information om din [!DNL Workfront]-distribution och väljer alternativet **[!UICONTROL Connect to Workfront]**. När anslutningen är klar skapas den anpassade integrationen av dokumentet [!DNL Workfront] automatiskt i miljön [!DNL Workfront].

   ![Anslut [!DNL Experience Manager] och [!DNL Workfront]](/help/assets/assets/wf-connection-config.png)

1. Om du vill verifiera anslutningen öppnar du den i [!DNL Workfront] och kontrollerar att API-nyckeln är densamma och att anslutningen är **[!UICONTROL Enabled]**. Om du vill göra det väljer du **[!UICONTROL Setup]** > **[!UICONTROL Documents]** > **[!UICONTROL Custom Integrations]** i [!DNL Workfront].

## Uppdatera [!DNL Workfront for Experience Manager enhanced connector] {#update-enhanced-connector-for-workfront}

Med Experience Manager Assets kan du uppdatera [!DNL Workfront for Experience Manager enhanced connector] från en tidigare version till den senaste versionen.

Uppdatera [!DNL Workfront for Experience Manager enhanced connector] till den senaste versionen:

1. Hämta den senaste versionen av den utökade anslutningen från [[!DNL Software Distribution] link](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/cq650/product/assets/workfront-tools.ui.apps.zip).
1. Installera paketet med [!UICONTROL Package Manager]. Mer information om hur du installerar paket finns i [Pakethanterarens dokumentation](/help/sites-administering/package-manager.md).
