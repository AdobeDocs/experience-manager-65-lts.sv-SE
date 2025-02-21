---
title: Uppdatera licenstypen för distributionen
description: Uppdatera licenstypen för distributionen genom att använda sidan Ändra licens i administrationskonsolen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/get_started_with_administering_aem_forms_on_jee
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Uppdatera licenstypen för distributionen {#update-the-license-type-for-the-deployment}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Som en del av installationsprocessen för AEM-formulär använde du Configuration Manager för att konfigurera och distribuera de AEM-formulärmoduler som du behöver. Som standard är dessa moduler konfigurerade med en 60-dagars utvärderingslicens. Använd sidan Ändra licens i administrationskonsolen för att ändra licenstypen för distributionen. De moduler som är distribuerade visas på sidan Ändra licens.

På sidan Ändra licens visas information om din licens:

* Den aktuella licenstypen
* Datum och tid då licensen senast uppdaterades
* Vem utförde den senaste uppdateringen?
* Licensens aktuella status
* Det datum då AEM-formulär installerades
* Det datum då den aktuella licensen upphör att gälla

>[!NOTE]
>
>Licensändringen gäller alla distribuerade moduler. Innan du ändrar licenstypen ska du ta bort distributionen av moduler som inte är licensierade. Välj inte typen av produktionslicens om listan med distribuerade moduler innehåller andra moduler än de som du har köpt från Adobe.

## Uppdatera licenstypen {#update-the-license-type}

1. Klicka på Licensiering i administrationskonsolen.
1. Läs slutanvändarlicensavtalet för AEM-formulär, välj Jag accepterar om du godkänner villkoren i avtalet och klicka sedan på Nästa.
1. Välj en licenstyp på sidan Ändra licens:

   * **EVAL:** 60-dagars utvärderingslicens
   * **DEV:** Permanent utvecklingslicens
   * **NFR:** Tvåårig utvärderingslicens
   * **IDEV:** Ettårsprenumeration på Adobe Developer Program
   * **Produktion:** Permanent licens

1. Välj Ja, licensändringen är giltig för alla distribuerade moduler.
1. Klicka på Bekräfta licensändring. Ett meddelande om att licensen har uppdaterats visas.
