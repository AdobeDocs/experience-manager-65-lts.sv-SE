---
title: Uppdaterar allmänna inställningar
description: Uppdatera inställningar för AEM Forms-appar som hemskärmen och hämta starpunkter och alternativ för bilagor
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Uppdaterar allmänna inställningar{#updating-general-settings}

Med de allmänna inställningarna för AEM Forms-appen kan du ange inställningar som hämtningsbilagor, offlineläge, landningsskärm, standardkategori och frekvens för autosparande.

## Uppdatera de allmänna inställningarna i din app {#working-with-the-form}

När du synkroniserar din app med AEM Forms-servern hämtas alla formulär och definierade uppgifter till din mobila enhet.

Den färdiga AEM Forms-applösningen hämtar inte de bilagor som är kopplade till varje formulär när appen synkroniseras.

Ändra inställningar för hämtning, offlineläge, landningsskärm, automatiskt sparande och synkronisering på fliken Allmänt. Du kan ändra [startskärmen](../../forms/using/home-screen.md) för din app.

**Navigera till fliken Allmänt på inställningsskärmen**

1. Om du vill gå till inställningsskärmen väljer du menyknappen i det övre vänstra hörnet av startskärmen och sedan **Inställningar**.
1. Välj fliken Allmänt på skärmen Inställningar.

   ![Allmänna inställningar i AEM Forms-appen](assets/gen-settings-1.png)

   Skärmen Allmänna inställningar

   >[!NOTE]
   >
   >Alternativen kan visas på olika mobila enheter.

### Allmänna inställningar {#general-settings}

Du kan göra följande ändringar i inställningarna för ditt program.

* **Hämta uppgiftsbilagor**: Om du vill ange om de associerade bilagorna ska hämtas eller inte när varje uppgift hämtas till din app.
* **Offlineläge**: Om du vill aktivera eller inaktivera offlinetjänsten för AEM Forms-programmet. Mer information finns i [Arbeta i offlineläge](/help/forms/using/work-offline-mode.md).
* **Startskärm**: Anger startplats ([Startskärm](../../forms/using/home-screen.md)) för programmet.
Tillgängliga alternativ

   * Forms
   * Uppgifter
   * Favoriter

* **Standardkategori**: Här kan du välja vilken formulärkategori som ska visas på hemskärmen. När du väljer Alla kan du se alla formulär på hemskärmen. Kategorierna fylls i baserat på de formulär som läses in i appen. Forms är tillgängligt i appen baserat på de formulärinställningar som har angetts på AEM Forms-servern.

* **Spara automatiskt**: Anger hur ofta [mobilappen sparar formulärdata lokalt](../../forms/using/autosave-data-app.md).
* **Synkroniseringsfrekvens**: Anger hur ofta din [mobilapp synkroniseras](../../forms/using/sync-app.md) med AEM Forms-servern i onlineläge.
  **Rensa lokala data**: Rensa databasen, inklusive inställningar och lokala data för alla användare och fillagring från enheten.

>[!NOTE]
>
>Om du rensar cachen loggas du omedelbart ut från appen.
>
>Du uppmanas dock att bekräfta rensningen av cachen.
