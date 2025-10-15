---
title: Installera funktionspaket 18912 för migrering av gruppresurser
description: Med funktionspaketet 18912 kan du antingen importera stora mängder mediefiler via FTP eller migrera mediefiler från Dynamic Media Classic till Dynamic Media i Adobe Experience Manager. Detta tillvalspaket finns tillgängligt från Adobe support.
contentOwner: Rick Brough
topic-tags: dynamic-media
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
content-type: reference
docset: aem65
feature: Asset Management
role: User, Admin
solution: Experience Manager, Experience Manager Assets
exl-id: 9d49d64b-fe90-4da6-a2db-19a69d1dc12c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Installera funktionspaket 18912 för migrering av gruppresurser{#installing-feature-pack-for-bulk-asset-migration}

Installationen av funktionspaket 18912 är *valfri*.

Med funktionspaketet 18912 kan du massimportera mediefiler direkt till Dynamic Media - Scene7-läge på Adobe Experience Manager via FTP. Du kan också migrera resurser från Dynamic Media Classic till Dynamic Media - Scene7-läge i Experience Manager. Funktionspaketet är tillgängligt från [Adobe Professional Services](https://business.adobe.com/se/customers/consulting-services/main.html).

>[!IMPORTANT]
>
>Du kan använda funktionspaketet för att migrera resurser på egen hand från Dynamic Media Classic till Dynamic Media - Scene7 i Experience Manager. Du kan också massmigrera resurser med FTP-funktionen i Dynamic Media Classic. Adobe rekommenderar *inte* att du använder någon av dessa metoder på grund av komplexiteten.
>
>Det här paketet med flyttningsfunktioner stöds *endast* som en del av ett migreringsprojekt när det görs via [Adobe Professional Services](https://business.adobe.com/se/customers/consulting-services/main.html).

Innan du installerar funktionspaketet skapar du en tjänstanvändare och skickar informationen till Adobe support.

Se även [Konfigurera dynamiska media - Scene7-läge](/help/assets/config-dms7.md).

**Så här installerar du funktionspaket 18912 för migrering av gruppresurser:**

1. I din Experience Manager-instans går du till **[!UICONTROL Tool]** > **[!UICONTROL Security]** > **[!UICONTROL Users]** och väljer **[!UICONTROL Create User]**. Den här tjänstanvändaren måste ha *läs-/skrivbehörighet* till `/content/dam.`
1. I fälten **[!UICONTROL ID]** och **[!UICONTROL Password]** anger du ett användarnamn och ett lösenord, till exempel **FTP-användare**. Det här namnet visas på tidslinjen som den användare som skapade resursen. När en resurs överförs från FTP betraktas en resurs som skapad när den överförs till FTP-servern och överförs till Experience Manager.
1. Kontakta [Adobe kundsupport för Experience Manager](https://experienceleague.adobe.com/sv?support-solution=General#support) om du vill ha tillgång till funktionspaketet 18912 för hämtning. Du kan behöva följande information när du kontaktar supporten:

   * Serverns IP-adress för din Author-instans, inklusive portnumret (portnumret är som standard 4502.)
   * Användarnamn och lösenord för Experience Manager-tjänsten från föregående steg.

1. Adobe kundsupport för Experience Manager ger dig FTP-inloggningsuppgifter och tillgång till funktionspaketet 18912.
1. Installera funktionspaketet 18912 när du får det.

   Mer information om hur du använder programdistribution och paket i Experience Manager finns i [Arbeta med paket](/help/sites-administering/package-manager.md).
