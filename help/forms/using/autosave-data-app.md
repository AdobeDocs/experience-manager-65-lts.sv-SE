---
title: Använda automatiskt sparade i AEM Forms-appen
description: Lär dig hur du använder funktionen Spara automatiskt i AEM Forms-appen för att undvika dataförlust.
contentOwner: sashanka
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 8f504453-1009-46d9-83a5-d4a8531d7e2c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Använda automatiskt sparade i AEM Forms-appen{#using-autosave-in-aem-forms-app}

När en användare matar in data i Adobe Experience Manager Forms-appen sparar funktionen automatiskt dem med regelbundna intervall. Funktionen Spara automatiskt i AEM Forms-appen hjälper dig att undvika dataförluster om appen stängs av misstag.

Ditt program kan stängas av misstag:

* Om enheten stängs av på grund av för lite batteri
* Om användaren dödar appen
* Om en oväntad krasch inträffar

Du kan ange de intervall efter vilka appen sparar de angivna data.

>[!NOTE]
>
>Välj autosparfrekvens med omdöme. Automatiska åtgärder som sparas ofta kan ha en märkbar inverkan på enhetens prestanda.

Så här använder du funktionen Spara automatiskt i AEM Forms-appen:

1. Logga in på appen och gå till **Inställningar > Allmänt**.
1. På skärmen Allmänt använder du alternativet **Spara frekvens automatiskt** för att välja de intervall med vilka du vill att appen ska spara angivna data.
   [![Anger automatiskt sparad frekvens](assets/using-autosave-freq-07.png)](assets/using-autosave-freq-07-1.png)

1. När du startar om programmet och loggar in med samma användare uppmanas du att återställa uppgiften i dialogrutan Återställ osparad uppgift. Klicka på **OK** i dialogrutan Återställ osparad uppgift om du vill fortsätta arbeta med den sparade uppgiften. Du kan klicka på **Avbryt** om du vill ta bort de sparade data som motsvarar den senast aktiverade autosparfunktionen och börja arbeta med en ny uppgift.

   När du klickar på **OK** återställs aktiviteten med de data som motsvarar den senaste autosparfunktionen som utlöstes innan appen kraschade. Den innehåller formulärdata och alla bilagor som är associerade med uppgiften.
   [![Hämtning av en uppgift återskapad ](assets/autosave-flow.png)](assets/using-autosave-freq-06.png)**A.** Ett pågående arbetsformulär **B.** Programmet stängdes med tvång **C.** Programmet startades om med dialogrutan Återställ osparad aktivitet **D.** Formuläret återställdes med originaldata
