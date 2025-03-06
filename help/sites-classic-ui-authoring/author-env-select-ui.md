---
title: Välja användargränssnitt
description: För att underlätta för redigeringsanvändarna går det att växla till det klassiska användargränssnittet med det pekaktiverade gränssnittet när det behövs.
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
exl-id: 781b580a-e4d1-419e-afb1-884c8fb634b9
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Välja användargränssnitt{#selecting-your-ui}

Eftersom det beröringskänsliga användargränssnittet ersätter det klassiska användargränssnittet måste AEM-instansens användare eller administratör aktivt välja att fortsätta använda det klassiska användargränssnittet. Eftersom det klassiska användargränssnittet inte längre underhålls går det inte att helt enkelt växla från det klassiska användargränssnittet till motsvarande i det beröringskänsliga användargränssnittet.

För att underlätta för redigeringsanvändarna går det att växla till det klassiska användargränssnittet med det pekaktiverade gränssnittet när det behövs. Mer information finns i [Välja användargränssnitt](/help/sites-authoring/select-ui.md) i standarddokumentationen för redigering.

>[!NOTE]
>
>Instanser som uppgraderats från en tidigare version behåller det klassiska användargränssnittet för sidredigering.
>
>Efter uppgraderingen växlas inte sidredigering automatiskt till det beröringsaktiverade användargränssnittet, men du kan konfigurera detta med [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) i **tjänsten för användargränssnittsläge för WCM-redigering** ( `AuthoringUIMode`-tjänsten). Se [Gränssnittsåsidosättningar för redigeraren](#uioverridesfortheeditor).

## Konfigurera standardgränssnittet för din instans {#configuring-the-default-ui-for-your-instance}

En systemadministratör kan konfigurera användargränssnittet som visas vid start och inloggning med [Rotmappning](/help/sites-deploying/osgi-configuration-settings.md#daycqrootmapping).

Detta kan åsidosättas av användarens standardinställningar eller sessionsinställningar.
