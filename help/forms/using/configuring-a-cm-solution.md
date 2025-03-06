---
title: Konfigurera en Correspondence Management-lösning
description: Lär dig hur du konfigurerar en Correspondence Management-lösning i en AEM Forms-miljö.
topic-tags: correspondence-management
content-type: reference
products: SG_EXPERIENCEMANAGER/6.3/FORMS
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: da668935-9d16-49e1-8e7a-772fc4040c1d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Konfigurera en Correspondence Management-lösning {#configuring-a-correspondence-management-solution}

## Definiera författarinstansens URL för VersionRestoreManagerImpl {#defining-author-instance-url-for-versionrestoremanagerimpl}

Följ de här stegen för att definiera en URL för författarinstansen för återställning av författarinstansversion:

1. Gå till *https://:&lt;PublishHost>:&lt;PublishPort>/lc/system/console/configMgr*. Logga in med användarautentiseringsuppgifter för OSGi Management Console. Standardautentiseringsuppgifterna är admin/admin.
1. Sök och klicka på ikonen **[!UICONTROL Edit]** bredvid inställningen **[!UICONTROL com.adobe.livecycle.content.activate.impl.VersionRestoreManagerImpl.name]**.
1. I fältet **[!UICONTROL VersionRestoreManager Author URL]** anger du URL:en för författarinstansen av VersionRestoreManager.

   **URL-sträng**:

   `https://<hostname>:<port>:/libs/fd/fdm/content/crud/lc.content.remote.activate.VersionRestoreManager`

   >[!NOTE]
   >
   >Om det finns flera författarinstanser (grupperade) som föregås av en belastningsutjämnare anger du URL:en till belastningsutjämnaren i fältet **[!UICONTROL VersionRestoreManager Author URL]**.

1. Klicka på **[!UICONTROL Save]**.

## Definiera publiceringsinstansens URL för ActivationManagerImpl (hanteraren för aktivering av offentlig instans) {#defining-the-publish-instance-url-for-activationmanagerimpl-public-instance-activation-manager}

Följ de här stegen för att definiera publiceringsinstansens URL för aktiveringshanteraren för den offentliga instansen:

1. Gå till *https://:&lt;authorHost>:&lt;authorPort>/lc/system/console/configMgr*. Logga in med användarautentiseringsuppgifter för OSGi Management Console. Standardautentiseringsuppgifterna är admin/admin.
1. Sök och klicka på ikonen **[!UICONTROL Edit]** bredvid inställningen **[!UICONTROL com.adobe.livecycle.content.activate.impl.ActivationManagerImpl.name]**.
1. I fältet **[!UICONTROL ActivationManager Publish URL]** anger du URL:en för åtkomst till Publish-instansen ActivationManager. Du kan ange följande URL:er.

   * **URL för belastningsutjämnare (rekommenderas)**: Ange URL för belastningsutjämnare, om du har en webbserver som fungerar som belastningsutjämnare framför publiceringsgruppen (flera icke-grupperade publiceringsinstanser).
   * **Publiceringsinstans-URL**: Ange en URL för publiceringsinstans. Om du har en publiceringsinstans eller webbservern där publiceringsgruppen finns är den inte tillgänglig från författarmiljön på grund av begränsningar. Om den angivna publiceringsinstansen är nere finns det en reservmekanism att hantera på författarsidan.
   * **URL-sträng**:

     `https://<hostname>:<port>:/libs/fd/fdm/content/crud/lc.content.remote.activate.activationManager`

1. Klicka på **[!UICONTROL Save]**.

Mer information om hur du konfigurerar Correspondence Management finns i [Egenskaper för Correspondence Management-konfiguration](https://helpx.adobe.com/aem-forms/6-2/cm-configuration-properties.html).
