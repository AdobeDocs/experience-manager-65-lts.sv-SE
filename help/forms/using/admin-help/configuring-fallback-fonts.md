---
title: Konfigurera reservteckensnitt
description: Lär dig hur du konfigurerar grundteckensnitt för AEM Forms. Du kan använda filen FontManagerResources.properties om du vill mappa standardteckensnitten till grundteckensnitt manuellt.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_pdf_generator
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: PDF Generator
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: d11bb8dc-d0fe-4182-88dd-9ef1ecf687db
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Konfigurera reservteckensnitt {#configuring-fallback-fonts}

Du kan konfigurera filen FontManagerResources.properties manuellt så att standardteckensnitten för AEM-formulär mappas till reserv (eller ersätt) om standardteckensnitten inte är tillgängliga på servern. Den här egenskapsfilen finns i filen adobe-fontmanager.jar.

>[!NOTE]
>
>Konfiguration av reservteckensnitt gäller även för monteringsverktyget.

1. Navigera till filen adobe-livecycle-*`[appserver]`*.ear i katalogen *`[aem-forms root]`*/configurationManager/export, skapa en säkerhetskopia och packa upp originalet.
1. Leta reda på filen adobe-fontmanager.jar och packa upp den.
1. Leta reda på filen FontManagerResources.properties och öppna den i en textredigerare.
1. Ändra teckensnittsplatserna och namnen för generiska och reservalternativ efter behov och spara filen.

   Teckensnittsposterna i filen FontManagerResources.properties är relativa till katalogen *`[aem-forms root]`*/fonts. Om du anger teckensnitt som inte är standardteckensnitt för AEM-formulär måste du installera dessa teckensnitt i den här katalogstrukturen (antingen i en befintlig katalog eller i en ny).

   >[!NOTE]
   >
   >Om det angivna teckensnittet eller standardteckensnittet inte innehåller ett visst Unicode-tecken, eller om det inte är tillgängligt, hämtas tecknet från ett reservteckensnitt enligt följande prioritet:

   * Språkspecifikt teckensnitt
   * ROOT-teckensnitt om språkområdet inte är inställt
   * Generiskt teckensnitt, söks igenom efter ordningsföljd i reservtabellen

1. Paketera om filen adobe-fontmanager.jar.
1. Paketera om filen adobe-livecycle-*`[appserver]`*.ear och distribuera den sedan manuellt eller genom att köra Configuration Manager.

>[!NOTE]
>
>Använd inte Configuration Manager för att paketera om filen adobe-livecycle-`[appserver]`.ear eftersom den kommer att skriva över dina ändringar med standardvärdena för AEM-formulär.
