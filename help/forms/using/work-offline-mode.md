---
title: Arbeta i offline-läge
description: Ta din mobila enhet offline utanför AEM Forms nätverksområde eller i offlineläge och arbeta med AEM Forms-appen
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 0%

---

# Arbeta i offline-läge {#working-in-the-offline-mode}

Med offline-läget i AEM Forms-appen kan du arbeta smidigt även om appen är offline. Du kan öppna, uppdatera och skicka ett formulär utan att behöva ansluta till nätverket.

Du börjar arbeta med AEM Forms genom att synkronisera appen med AEM Forms-servern. Alla formulär som du har tilldelats hämtas i appen. För AEM Forms i JEE hämtas uppgifter på fliken Åtgärder och startpunkter som är kopplade till formulär och andra formulär på fliken Forms. För AEM Forms i OSGi läses endast Forms in på fliken Forms.

Mer information om hur du synkroniserar appen finns i [Synkronisera appen](/help/forms/using/sync-app.md).

## Göra Forms tillgängligt offline {#making-forms-available-offline}

När du synkroniserar din app med AEM Forms-servern hämtas formulären till din mobila enhet. Som standard hämtas dock inte de bilagor som är kopplade till formuläret. Det innebär att om du är online kan du visa de bifogade filerna. För att du ska kunna se den bifogade filen i offlineläge måste du dock ändra standardinställningarna i programmet.

Om du vill vara säker på att de associerade bilagorna hämtas med varje formulär anger du ON för Hämta bilagor. Mer information finns i [Uppdatera allmänna inställningar](/help/forms/using/update-general-settings.md).

Eftersom nedladdning av data på den mobila enheten kan påverka enhetens prestanda är inställningen Hämta bilagor inställd på AV som standard. Bifogade filer hämtas till enheten för varje åtgärd som hämtas från servern efter att inställningen har uppdaterats till PÅ. I offlineläget kan en användare sedan arbeta med alla uppgifter som hämtas till enheten efter att ha angett alternativen **Hämta bilagor** till PÅ.

## Konfigurera offlinetjänst för AEM Forms-program {#configuring-offline-service-for-aem-forms-app-br}

AEM Forms app offline-tjänst identifierar de resurser som används i ett formulär. AEM Forms-appen använder den här tjänsten för att få information om formulärberoenden. Information om formulärberoenden krävs för att aktivera offlinefunktioner. Offlinetjänsten för AEM Forms-programmet cachelagrar sökvägarna eller URL:erna för resurserna som används i ett formulär. Cachen uppdateras baserat på ändringarna i formuläret och giltighetsperioden som konfigurerats för offlinetjänsten. Cachelagring av sökvägar eller URL:er för resurserna som används i ett formulär förbättrar prestandan på serversidan.

Så här konfigurerar du offlinekomponenten på serversidan i AEM Forms-programmet:

1. I författarinstansen går du till **Adobe Experience Manager** >**Verktyg** > **Forms** > **Konfigurera Forms App Offline Service**.

   URL: `https://<server>:<port>/<context-path>/libs/fd/workspace-offline/gui/content/config.html`

1. Under Allmänna inställningar kan du göra följande:

   * **Rensa cache**: Rensar serversidans cache för formulärberoenden.
   * **Återställ konfiguration**: Återställer offlinekonfigurationen för AEM Forms-programmet.
   * **Cachegiltighet**: Anger giltighetsperioden för offlinecachen på serversidan.
   * **Sökvägar för resursobservation**: Anger sökvägar där offlinetjänsten övervakar resursändringar. Om några ändringar görs i de angivna sökvägarna uppdateras offlinecachen för alla beroende formulär. Exempel: `/etc/clientlibs/fd,/content/dam/images`.

1. På fliken **Manuell resurscache** anger du att det inte går att identifiera formulärberoenden som är offline. Du kan ange resurser, till exempel bilder som läses in från JavaScript. AEM Forms laddar också ned dessa resurser för offlineläget.
