---
title: Krypteringsstöd för konfigurationsegenskaper
description: Lär dig mer om krypteringsstöd för konfigurationsegenskaper i AEM.
contentOwner: User
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: security
solution: Experience Manager, Experience Manager Sites
feature: Security
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Krypteringsstöd för konfigurationsegenskaper{#encryption-support-for-configuration-properties}

## Ökning {#overview}

Med den här funktionen kan alla OSGi-konfigurationsegenskaper lagras i en skyddad krypterad form i stället för i klartext. Formuläret i webbkonsolens användargränssnitt används för att skapa krypterad text från klartext med hjälp av den systemomfattande krypteringsnyckeln.

Stöd för OSGi Configuration Plugin har lagts till för att dekryptera egenskapen innan den används av en tjänst.

>[!NOTE]
>
>Tjänster som förväntar sig ett krypterat värde måste använda kontrollen IsProtected för att se om värdet är krypterat innan det försöker dekryptera det, eftersom det redan kan ha dekrypterats.

## Aktivera krypteringsstöd {#enabling-encryption-support}

De här stegen visar hur du krypterar SMTP-lösenordet för e-posttjänsten. Du kan slutföra de här stegen för en OSGI-egenskap som du vill kryptera.

1. Gå till AEM webbkonsol på *https://&lt;serveradress>:&lt;serverport>/system/console/configMgr*
1. Gå till **Main - Crypto Support** i det övre vänstra hörnet

   ![chlimage_1-325](assets/chlimage_1-325.png)

1. Sidan **Adobe Experience Manager Web Console Crypto Support** visas.

   ![screen_shot_2018-08-01at113417am](assets/screen_shot_2018-08-01at113417am.png)

1. I fältet **Oformaterad text** anger du texten i känsliga data som du vill skydda.
1. Välj **Skydda**. Den skyddade texten visas som krypterad text.

   ![screen_shot_2018-08-01at113844am](assets/screen_shot_2018-08-01at113844am.png)

1. Kopiera den skyddade texten från steg 5 och klistra in den i OSGI-formulärvärdet. I det här exemplet läggs det krypterade **SMTP-lösenordet** till i *Day CQ Mail Service*.

   ![screen_shot_2016-12-18at105809pm](assets/screen_shot_2016-12-18at105809pm.png)

1. Spara egenskaperna för daglig CQ Mail Service. SMTP-lösenordet skickas nu som ett krypterat värde.

## Stöd för dekryptering {#decryption-support}

AEM har nu ett Configuration Plugin-program för dekryptering av konfigurationsegenskaper. Denna AEM-plugin dekrypterar och hämtar automatiskt textegenskaperna.
