---
title: Configuring Connector for Microsoft SharePoint
description: Konfigurera Connector for Microsoft SharePoint för att möjliggöra kommunikation mellan AEM-formulär och Microsoft SharePoint.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/connecting_to_a_content_management_system
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms
hide: true
hidefromtoc: true
exl-id: d1575576-3a05-496c-b683-bb5badc02711
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Configuring Connector for Microsoft SharePoint {#configuring-connector-for-microsoft-sharepoint}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Koppling för Microsoft SharePoint möjliggör kommunikation mellan AEM-formulär och Microsoft SharePoint. Mer bakgrundsinformation finns i &quot;Connectors for ECM&quot; i [Services Reference](https://www.adobe.com/go/learn_aemforms_services_63).

1. I administrationskonsolen klickar du på Tjänster > Koppling för Microsoft SharePoint.
1. Ange följande inställningar för din SharePoint-server:

   **Värdnamn för SharePoint-server:** Webbprogrammets värdnamn på SharePoint-servern, i formatet `[hostname]:'port'`.

   **Användarnamn:** Användarkontot som används för att ansluta till SharePoint-servern.

   **Lösenord:** Lösenord för användarkontot som används för att ansluta till SharePoint-servern

   **Domännamn:** Domän där SharePoint-servern finns.

1. Klicka på Spara.

## Konfigurationstjänst för Microsoft SharePoint {#microsoft-sharepoint-configuration-service}

Med konfigurationstjänsten `(MSSharePointConfigService)` för Microsoft SharePoint kan du ange autentiseringsuppgifter för den användare av AEM-formulär som har personifieringsbehörigheter. Mer information om personifieringsbehörigheter finns i [Konfigurera Connector för Microsoft SharePoint](https://help.adobe.com/en_US/AEMForms/6.1/SharePointConfig/index.html). Följ de här stegen för att ange inställningar för `MSSharePointConfigService`:

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. Navigera i listan med tjänster och klicka på `MSSharePointConfigService`.
1. Ange följande inställningar på konfigurationssidan:

   * Användarnamn för en användare med personifieringsbehörigheter
   * Lösenord för ovanstående användare

1. Klicka på Spara.
