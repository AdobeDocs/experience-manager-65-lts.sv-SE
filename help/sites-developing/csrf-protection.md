---
title: CSRF Protection Framework
description: Ramverket använder tokens för att garantera att kundens begäran är berättigad
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: d6bd4028-56c9-4e09-9bba-1199a41b41b8
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# CSRF Protection Framework{#the-csrf-protection-framework}

Förutom referensfiltret för Apache Sling tillhandahåller Adobe även ett nytt CSRF-skyddsramverk som skyddar mot den här typen av attacker.

Ramverket använder tokens för att garantera att kundens begäran är berättigad. Token genereras när formuläret skickas till klienten och valideras när formuläret skickas tillbaka till servern.

>[!NOTE]
>
>Det finns inga token för publiceringsinstanserna för anonyma användare.

## Krav {#requirements}

### Beroenden {#dependencies}

Alla komponenter som är beroende av `granite.jquery`-beroendet kan automatiskt dra nytta av CSRF Protection Framework. Om inte, för någon av dina komponenter, måste du deklarera ett beroende för `granite.csrf.standalone` innan du kan använda ramverket.

### Replikerar krypteringsnyckeln {#replicating-crypto-keys}

Om du vill använda token måste du replikera HMAC-binärfilen till alla instanser i distributionen. Mer information finns i [Replikera HMAC-nyckeln](/help/sites-administering/encapsulated-token.md#replicating-the-hmac-key).

>[!NOTE]
>
>Se även till att du gör de nödvändiga konfigurationsändringarna för Dispatcher för att använda CSRF Protection Framework:
>
>* [Konfigurera Adobe Experience Manager Dispatcher för att förhindra CSRF-attacker](https://experienceleague.adobe.com/sv/docs/experience-manager-dispatcher/using/configuring/configuring-dispatcher-to-prevent-csrf)
>* [Dispatcher - översikt](https://experienceleague.adobe.com/sv/docs/experience-manager-dispatcher/using/dispatcher)

>[!NOTE]
>
>Om du använder manifestcachen med ditt webbprogram måste du lägga till **&ast;** i manifestet för att se till att token inte tar CSRF-tokengenereringsanropet offline. Mer information finns i den här [länken](https://www.w3.org/TR/offline-webapps/).
>
>Mer information om CSRF-attacker och hur du kan mildra dem finns på OWASP-sidan [Cross-Site Request Forgery](https://owasp.org/www-community/attacks/csrf).
