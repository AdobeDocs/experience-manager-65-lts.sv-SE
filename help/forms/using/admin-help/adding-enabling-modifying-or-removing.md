---
title: Lägga till, aktivera, ändra eller ta bort slutpunkter
description: Lär dig hur du lägger till, aktiverar, ändrar och tar bort slutpunkter.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_endpoints
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 14264788-a05a-4a8d-b485-33ae1caac094
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Lägga till, aktivera, ändra eller ta bort slutpunkter {#adding-enabling-modifying-or-removing-endpoints}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

## Lägga till en slutpunkt i en tjänst {#add-an-endpoint-to-a-service}

Slutpunkter kan bara läggas till i tjänster. En slutpunkt får inte finnas ensam. Den måste vara associerad med en tjänst.

>[!NOTE]
>
>Du bör använda unika namn när du lägger till slutpunkter.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Tjänsthantering.
1. Klicka på den tjänst du vill konfigurera på sidan Tjänsthantering.
1. Välj den typ av slutpunkt som ska läggas till i listan på fliken Slutpunkter och klicka sedan på Lägg till.
1. Beroende på slutpunktstypen konfigurerar du ytterligare slutpunktsinställningar.

[Slutpunktsinställningar för bevakad mapp](/help/forms/using/admin-help/configuring-watched-folder-endpoints.md#watched-folder-endpoint-settings)

[Inställningar för e-postslutpunkt](/help/forms/using/admin-help/configuring-email-endpoints.md#email-endpoint-settings)

[Konfigurera slutpunkter för Aktivitetshanteraren](/help/forms/using/admin-help/configuring-task-manager-endpoints.md#configuring-task-manager-endpoints)

[Tar bort slutpunktsinställningar](/help/forms/using/admin-help/configuring-remoting-endpoints.md#remoting-endpoint-settings)

1. Klicka på Lägg till.

## Aktivera eller inaktivera en slutpunkt {#enable-or-disable-an-endpoint}

Som standard aktiveras nya slutpunkter automatiskt. Men om du har inaktiverat en slutpunkt måste du aktivera den för att den ska fungera.

Om du har problem med tjänster kan du inaktivera de associerade slutpunkterna för att felsöka problemet bättre. Du kan även inaktivera slutpunkter under regelbundet systemunderhåll eller när du uppgraderar en tjänst.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Endpoint Management.
1. Markera kryssrutan för slutpunkten på sidan Slutpunktshantering för att aktivera eller inaktivera och klicka på Aktivera eller Inaktivera.

## Ändra en slutpunkt {#modify-an-endpoint}

>[!NOTE]
>
>De ändringar du gör i en slutpunktskonfiguration med administrationskonsolen återspeglas inte i programdesigntidskopiorna. Om du distribuerar om ett program kommer alla ändringar som du har gjort i slutpunkterna med administrationskonsolen att gå förlorade.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Endpoint Management.
1. Klicka på slutpunkten som du vill ändra på sidan Slutpunktshantering.
1. Ändra slutpunktens namn, beskrivning och inställningar på sidan Uppdatera slutpunkt.

   >[!NOTE]
   >
   >Ta inte med ett &lt;-tecken i namnet eller beskrivningen eftersom det kortar av namnet eller beskrivningen som visas i Workspace.

1. Klicka på Uppdatera om du vill spara ändringarna.

Du kan även utföra den här uppgiften från sidan Tjänsthantering genom att markera en tjänst och sedan klicka på fliken Slutpunkter.

## Ta bort en slutpunkt {#remove-an-endpoint}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Endpoint Management.
1. Markera kryssrutan för slutpunkten som ska tas bort på sidan Slutpunktshantering och klicka på Ta bort. Slutpunkten visas inte längre.
