---
title: Konfigurera AEM-formulär för information i prefetchdomain
description: Konfigurera AEM-formulär för att förhämta domäninformation om du får en långsammare svarstid på grund av djupt inkapslade grupper eller om du är medlem i många grupper.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Konfigurera AEM-formulär för information i prefetchdomain {#configure-aem-forms-to-prefetchdomain-information}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Användare kan få en långsammare svarstid om de tillhör många grupper (till exempel 500 eller fler) eller om grupperna är djupt inkapslade (till exempel 30 nivåer). Om du får det här problemet kan du konfigurera AEM-formulär så att information från vissa domäner hämtas i förväg.

1. Klicka på **[!UICONTROL Settings > User Management > Configuration > Import And Export Configuration Files]** i administrationskonsolen.
1. Om du vill exportera den aktuella konfigurationsinställningen till en fil klickar du på **[!UICONTROL Export]** och sparar konfigurationsfilen på en annan plats.
1. Lägg till följande nod (markerad med fet stil):

   ```xml
    <node name="UM">
    <map/>
    <node name="PrincipalCache">
        <map>
            <entry key="principalCacheSize" value="1000"/>
            <entry key="principalCacheBatchFetchSize" value="10"/>
            <entry key="rebuildCacheAfterSync" value="true />
            <entry key="enableFullPrefetch" value="false"/>
            <entry key="principalCacheDomains" value="Domain_Name1/Domain_Name2/Domain_Name3"/>
        <map>
    </node>
    <node name="APSAuditService">
   ```

   I det här exemplet har flera domäner konfigurerats för förhämtning. Domännamnen avgränsas med &quot;/&quot;. Detta visas i exemplet ovan med *Domain_Name1*, *Domain_Name2* och *Domain_Name3*.

1. Om du vill importera den uppdaterade filen klickar du på **[!UICONTROL Configuration > Import And Export Configuration Files]** i Användarhantering.
1. Klicka på **[!UICONTROL Browse]** för att hitta filen, klicka på Importera och sedan på **[!UICONTROL OK]**.
