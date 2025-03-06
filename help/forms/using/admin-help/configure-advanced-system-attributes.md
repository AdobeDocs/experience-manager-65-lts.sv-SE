---
title: Konfigurera avancerade systemattribut
description: Använd sidan Konfigurera avancerade systemattribut om du vill ändra vissa inställningar i konfigurationsfilen utan att behöva exportera, redigera och importera filen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: f1a68461-c66a-4ea4-902b-644c620ea3f6
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Konfigurera avancerade systemattribut {#configure-advanced-system-attributes}

Använd sidan Konfigurera avancerade systemattribut om du vill ändra vissa inställningar i konfigurationsfilen utan att behöva exportera, redigera och importera filen. (Se [Importera och exportera konfigurationsfilen](/help/forms/using/admin-help/importing-exporting-configuration-file.md#importing-and-exporting-the-configuration-file).)

1. Klicka på **[!UICONTROL Settings > User Management > Configuration > Configure Advanced System Attributes]** i administrationskonsolen.
1. (Valfritt) Ändra något av följande sessionsattribut:

   **Tidsgräns för session (minuter):** Den tid, i minuter, som en användare loggas ut från systemet automatiskt. Som standard tar AEM fram blankettkomponenter som Workbench efter två timmar, oavsett aktivitet eller inaktivitet, och användaren måste logga in igen. Giltiga värden är `1` till `1440`. Standardvärdet är `120` (2 timmar). Den här inställningen uppdaterar `SAML/Producer/assertionValidityInMinutes`-postnyckeln i konfigurationsfilen.

   >[!NOTE]
   >
   >Du bör inte ange en tidsgräns för sessioner under 10 minuter eftersom systemet kanske inte fungerar som det ska. Det rekommenderade värdet är 10-120 (minuter).

   **Kontrolltröskel (sekunder):** En bufferttid för att kompensera fördröjningar på grund av systemtidsskillnader mellan AEM-formulärprogramservrar i ett kluster. AEM blanketterar användarens inloggningstid med den tid (i sekunder) som anges i den här egenskapen. Giltiga värden är `0` till `3600`. Standardvärdet är `60`. Den här inställningen uppdaterar `SAML/Producer/assertionThresholdInSeconds`-postnyckeln i konfigurationsfilen.

   **Maximalt antal tillåtna förnyelser av en försäkran:** Det maximala antalet gånger en användarsession kan förnyas utan genomskinlighet utan att en inloggning krävs. Giltiga värden är `0` till `9999`. Värdet `0` innebär att försäkringar inte förnyas. Standardvärdet är 10. Den här inställningen uppdaterar `SAML/Producer/maxAssertionRenewalCount`-postnyckeln i konfigurationsfilen.

1. (Valfritt) Ändra följande attribut för katalogsynkronisering:

   **Loggning av synkroniseringsstatistik:** Anger om användarhantering loggar detaljerad statistik under synkroniseringsprocessen. (Se [Aktivera eller inaktivera detaljerad loggning under synkronisering](/help/forms/using/admin-help/synchronizing-directories.md#enable-or-disable-detailed-logging-during-synchronization).)

   **Cron-uttryck för synkroniseringsslutbehandlare:** Intervallet där användarhanteringsförsök misslyckades med synkroniseringar. (Se [Konfigurera alternativet för nytt försök med katalogsynkronisering](/help/forms/using/admin-help/synchronizing-directories.md#configure-the-directory-synchronization-retry-option).)

   **Tidsgräns för klusterjobbutlås i minuter:** Används i klustrade miljöer. Om synkroniseringen på en nod misslyckas och klusterlåset inte frisläpps, anger det här värdet antalet minuter som en annan nod väntar innan låset tvångsförvärvas. Standardvärdet är `15` minuter. Giltiga värden är `1` till `1440` minuter.

1. (Valfritt) Ändra följande attribut och klicka sedan på **[!UICONTROL OK]**:

   **Händelsegranskning för användarhanterare:** Välj det här alternativet om du vill aktivera granskning av katalogsynkroniseringshändelser och autentiseringshändelser som lyckade, misslyckade och utelåsta. Som standard är det här alternativet inte markerat om du inte har installerat en komponent som kräver granskning, t.ex. Rights Management. Den här inställningen uppdaterar `APSAuditService`-postnyckeln i konfigurationsfilen.

   **Skapa dynamiska grupper automatiskt:** Gör att dynamiska grupper automatiskt kan skapas baserat på e-postdomäner. (Se [Skapa en dynamisk grupp](/help/forms/using/admin-help/creating-configuring-groups.md#create-a-dynamic-group).)

Du kan även återgå till de ursprungliga inställningarna för användarhantering genom att klicka på Läs in igen.
