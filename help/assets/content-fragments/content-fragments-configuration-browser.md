---
title: Content Fragments - Configuration Browser
description: Lär dig hur du aktiverar vissa Content Fragment-funktioner i Configuration Browser så att du kan använda Adobe Experience Manager kraftfulla headless-leveransfunktioner.
feature: Content Fragments
role: User
solution: Experience Manager, Experience Manager Assets
exl-id: b526cd3a-9b04-403a-a6f4-6abe973aaeac
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 12%

---

# Content Fragments - Configuration Browser{#content-fragments-configuration-browser}

Lär dig hur du aktiverar vissa Content Fragment-funktioner i Configuration Browser så att du kan använda Adobe Experience Manager (AEM) kraftfulla headless-leveransfunktioner.

## Aktivera funktionen för innehållsfragment för instansen {#enable-content-fragment-functionality-instance}

Använd **Konfigurationsläsaren** för att aktivera följande innan du använder innehållsfragment:

* **Modeller för innehållsfragment** - obligatoriskt
* **GraphQL - beständiga frågor** - valfritt

>[!CAUTION]
>
>Om du inte aktiverar **modeller för innehållsfragment**:
>
>* alternativet **Skapa** är inte tillgängligt för att skapa modeller.
>* Du kan inte [välja platskonfigurationen för att skapa den relaterade slutpunkten](/help/sites-developing/headless/graphql-api/graphql-endpoint.md#enabling-graphql-endpoint).

Om du vill aktivera funktionen för innehållsfragment måste du göra följande:

* Aktivera användning av innehållsfragmentsfunktioner via konfigurationsläsaren
* Använd konfigurationen i din Assets-mapp

### Aktivera funktionen för innehållsfragment i konfigurationsläsaren {#enable-content-fragment-functionality-in-configuration-browser}

Om du vill [använda vissa funktioner för innehållsfragment](#creating-a-content-fragment-model) måste **du** först aktivera dem via **konfigurationsläsaren**:

>[!NOTE]
>
>Mer information finns i [Konfigurationsläsaren:](/help/sites-administering/configurations.md#using-configuration-browser).

1. Navigera till **Verktyg**, **Allmänt** och öppna sedan **Konfigurationsläsaren**.

1. Använd **Skapa** för att öppna dialogrutan där du:

   1. Ange en **titel**.
   1. Om du vill aktivera deras användning väljer du
      * **Modeller för innehållsfragment**
      * **GraphQL - beständiga frågor**

      ![Definiera konfiguration](assets/cfm-conf-01.png)

1. Välj **Skapa** om du vill spara definitionen.

<!-- 1. Select the location appropriate to your website. -->

### Använd konfigurationen i din Assets-mapp {#apply-the-configuration-to-your-assets-folder}

När konfigurationen **global** är aktiverad för innehållsfragmentfunktioner gäller detta alla Assets-mappar.

Om du vill använda andra konfigurationer (d.v.s. inte globala) med en jämförbar Assets-mapp måste du definiera anslutningen. Detta gör du genom att välja lämplig **konfiguration** på fliken **Cloud Services** i **Mappegenskaper** för rätt mapp.

![Använd konfiguration](assets/cfm-conf-02.png)
