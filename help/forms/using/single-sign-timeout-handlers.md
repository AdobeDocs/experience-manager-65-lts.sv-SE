---
title: Hanterare för enkel inloggning och timeout
description: Så här anger du timeout-värdet för sessionen för arbetsytan i AEM Forms.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: c6bdfa6f-0d9b-4473-a2e1-6cad73fbd1ed
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 0%

---

# Hanterare för enkel inloggning och timeout {#single-sign-on-and-timeout-handlers}

AEM Forms arbetsyta är enkel inloggning aktiverad. Om en användare har loggat in på ett AEM Forms-program som Forms Manager eller PDF Generator och använder AEM Forms arbetsyta i samma webbläsarsession, loggas användaren in på AEM Forms arbetsyta och omvänt.

## Hantera servertimeout i AEM Forms arbetsyta {#handling-server-timeout-in-nbsp-aem-forms-workspace}

Sessionstimeout för en användare kan konfigureras i administrationskonsolen.

Om du vill ange tidsgränsen loggar du in på `https://'[server]:[port]'/adminui`, går till **Inställningar > Användarhantering > Konfiguration > Konfigurera avancerade systemattribut** och gör önskade inställningar.

I AEM Forms hanteras timeout för arbetsytan som:

* Sessionstiden för en användare är tillgänglig som svar på `initialize`-anrop som initierar användarsession.
* En popup-dialogruta meddelar användaren om att sessionen kommer att upphöra 15 sekunder innan sessionen går ut.

I den här popup-dialogrutan:

* Klicka på OK för att avsluta användarsessionen.
* Klicka på Avbryt om du vill initiera om användarsessionen.

>[!NOTE]
>
>Om ingen åtgärd vidtas loggas användaren automatiskt ut från AEM Forms-arbetsytan tre sekunder innan sessionen upphör.
