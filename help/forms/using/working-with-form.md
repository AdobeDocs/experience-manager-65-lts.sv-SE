---
title: Arbeta med ett formulär
description: Visa och uppdatera formuläret som är kopplat till en uppgift eller startpunkt i AEM Forms-appen
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Arbeta med ett formulär {#working-with-a-form}

Om ett formulär har aktiverats för synkronisering i formulärappen hämtas formuläret och du kan arbeta med det direkt.

Formulären hämtas till din app och är tillgängliga offline. Du driver t.ex. ett bankföretag och en kund fyller i en ansökan på er webbplats. Programmet är ett anpassat formulär som tar emot information från dina kunder och lagrar den för granskning. Administratören granskar formuläret och skapar ett verifieringsformulär i AEM författarinstans. Administratören aktiverar synkronisering av formuläret med appen AEM Forms. Om verifieringsformuläret finns i AEM Forms-appen kan fältagenten använda en mobilenhet för att verifiera kundens information. Den mobila enheten synkroniseras med servern och verifieringsformuläret läses in i appen. Din fältrepresentant kan besöka kunden, verifiera informationen, spara data som utkast eller skicka bekräftelseformuläret. Formuläret synkroniseras med servern varje gång appen är online.

Så här synkroniserar du formuläret i AEM Forms-appen:

1. Markera ett formulär i författarinstansen och klicka på **Visa egenskaper**.
1. Klicka på **Avancerat.** på egenskapssidan.
1. Aktivera alternativet **Synkronisera med AEM Forms App** under Avancerat och välj **Spara**.

Om du vill synkronisera flera formulär i författarinstansen markerar du flera formulär i formulärhanteraren och väljer **Synkronisera med AEM Forms App**. När formuläret publiceras kan AEM Forms-appen ansluta till publiceringsservern och hämta formulären.

Om ditt AFA-program (AEM Form Application) från Android inte kan synkroniseras utför du följande steg för att åtgärda synkroniseringsproblemet:

1. Gå till **https://[server]:[port]/system/console/configMgr**.
1. Sök efter **[!UICONTROL Adobe Granite Token Authentication Handler]** och klicka på **[!UICONTROL Edit]**.
1. Välj alternativet **[!UICONTROL None]** i listrutan för attributet **[!UICONTROL SameSite attribute for the login-token cookie]**.
1. Klicka på **[!UICONTROL Save]**.

![Synkronisera bild med AFA Android-appen](/help/forms/using/assets/afaandroid.png)

>[!NOTE]
>
>Formulär som stöds:
>
>* Anpassningsbara formulär (utan lazy loading)
>* Mobila formulär
>
>Bifogade filer på formulärnivå stöds inte i de adaptiva formulär som hämtas i AEM Forms-appen som synkroniseras med AEM Forms OSGi-servern. Användare kan bifoga filer i ett fält om författaren har aktiverat bilagor på fältnivå när formuläret redigeras.


**Öppna och uppdatera ett formulär**

1. Om du vill öppna ett formulär väljer du **[!UICONTROL Form]** på hemskärmen.
1. Du kan uppdatera fälten i formuläret, lägga till bilagor, spara som utkast och skicka det.
