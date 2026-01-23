---
title: Skapa Adobe Campaign Forms i Adobe Experience Manager
description: Med Adobe Experience Manager kan du skapa och använda formulär som interagerar med Adobe Campaign på din webbplats
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: personalization
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization,Integration
role: User,Admin,Architect,Developer
exl-id: 6a72ba56-8222-4853-adc6-ee8f3d395d9d
source-git-commit: 3bc6f5aab61b6ea0ae1d6d909bf2d36fd7e4dbe4
workflow-type: tm+mt
source-wordcount: '1280'
ht-degree: 0%

---

# Skapa Adobe Campaign Forms i AEM {#creating-adobe-campaign-forms-in-aem}

Med AEM kan du skapa och använda formulär som interagerar med Adobe Campaign på din webbplats. Specifika fält kan infogas i formulären och mappas till Adobe Campaign-databasen.

Du kan hantera nya kontaktprenumerationer, avbeställningar och användarprofildata samtidigt som du integrerar deras data i din Adobe Campaign-databas.

Om du vill använda Adobe Campaign-formulär i AEM måste du följa de här stegen som beskrivs i det här dokumentet:

1. Gör en mall tillgänglig.
1. Skapa ett formulär.
1. Redigera formulärinnehåll.

Tre typer av formulär, som är specifika för Adobe Campaign, är tillgängliga som standard:

* Spara en profil
* Prenumerera på en tjänst
* Avbeställ en tjänst

Dessa formulär definierar en URL-parameter som godkänner den krypterade primärnyckeln för en Adobe Campaign-profil. Baserat på den här URL-parametern uppdaterar formuläret data i den associerade Adobe Campaign-profilen.

Även om du skapar de här formulären oberoende av varandra, genererar du i ett vanligt fall en personlig länk till en formulärsida i nyhetsbrevets innehåll, så att mottagarna kan öppna länken och göra ändringar i profildata (oavsett om de avregistrerar, prenumererar eller uppdaterar deras profil).

Formuläret uppdateras automatiskt baserat på användaren. Mer information finns i [Redigera formulärinnehåll](#editing-form-content).

## Göra en mall tillgänglig {#making-a-template-available}

Innan du kan skapa formulär som är specifika för Adobe Campaign måste du göra de olika mallarna tillgängliga i ditt AEM-program.

Om du vill göra det läser du [malldokumentationen](/help/sites-developing/templates.md#template-availability).

## Skapa ett formulär {#creating-a-form}

Först och främst bör du kontrollera anslutningen mellan författaren och publicera instanser så fungerar Adobe Campaign. Se [Integrera med Adobe Campaign Standard](/help/sites-administering/campaignstandard.md) eller [Integrera med Adobe Campaign Classic](/help/sites-administering/campaignonpremise.md).

>[!NOTE]
>
>Kontrollera att egenskapen **acMapping** på sidans **jcr:content**-nod är inställd på **mapRecipient** eller **profile** när du använder Adobe Campaign Classic respektive Adobe Campaign Standard
>

1. I AEM navigerar du till den plats där du vill skapa en sida i Sites.
1. Skapa en sida och välj **Adobe Campaign Classic-profil** eller **Adobe Campaign Standard-profil** och klicka på **Nästa**.

   ![chlimage_1-43](assets/chlimage_1-43a.png)

   >[!NOTE]
   >
   >Om den önskade mallen inte är tillgänglig läser du [Malltillgänglighet](/help/sites-developing/templates.md#template-availability).

1. Lägg till sidans namn i fältet **Namn**. Det måste vara ett giltigt JCR-namn.
1. Ange en titel i fältet **Titel** och klicka på **Skapa**.
1. Öppna sidan och välj **Öppna egenskaper**. I Cloud Services lägger du till Adobe Campaign-konfigurationen och markerar kryssrutan för att spara ändringarna.

   ![chlimage_1-44](assets/chlimage_1-44a.png)

1. På sidan i komponenten **Formulärstart** väljer du vilken typ av formulär det är - **Prenumerera, Avsluta prenumeration,** eller **Spara profil**. Du kan bara ha en typ per formulär. Du kan nu [redigera formulärets innehåll](#editing-form-content).

## Redigera formulärinnehåll {#editing-form-content}

Forms som riktar sig till Adobe Campaign har specifika komponenter. Dessa komponenter har ett alternativ som gör att du kan länka varje fält i formuläret till ett fält i Adobe Campaign-databasen.

>[!NOTE]
>
>Om den önskade mallen inte är tillgänglig går du till [Göra en mall tillgänglig.](/help/sites-authoring/campaign.md)

I det här avsnittet beskrivs endast specifika länkar till Adobe Campaign. Mer information om hur du använder formulär i Adobe Experience Manager finns i [Komponenter i redigeringsläge](/help/sites-authoring/default-components-foundation.md).

1. Välj **Öppna egenskaper** och lägg till Adobe Campaign-konfigurationen i molntjänster och markera bockmarkeringen för att spara ändringarna.

   ![chlimage_1-45](assets/chlimage_1-45a.png)

1. Klicka på ikonen Konfiguration på sidan i komponenten **Formulärstart** .

   ![chlimage_1-46](assets/chlimage_1-46a.png)

1. Klicka på fliken **Avancerat** och välj den typ av formulär det är - **Prenumerera, avsluta abonnemang,** eller **Spara profil** och klicka på **OK.** Du kan bara ha en typ per formulär.

   * **Adobe Campaign: Spara profil**: gör att du kan skapa eller uppdatera en mottagare i Adobe Campaign (standardvärde).
   * **Adobe Campaign: Prenumerera på tjänster**: gör att du kan hantera prenumerationer för en mottagare i Adobe Campaign.
   * **Adobe Campaign: Avbeställ tjänster**: gör att du kan avbryta prenumerationen på en mottagare i Adobe Campaign.

1. Du måste ha en **krypterad primärnyckel** på varje formulär. Den här komponenten definierar vilken URL-parameter som används för att acceptera den krypterade primärnyckeln för en Adobe Campaign-profil. I Komponenter väljer du Adobe Campaign så att bara de komponenterna visas.
1. Dra komponenten **Krypterad primärnyckel** till formuläret (var som helst) och klicka på ikonen **Konfiguration** . Ange ett namn för URL-parametern på fliken **Adobe Campaign** . Klicka på bockmarkeringen för att spara ändringarna.

   Genererade länkar till det här formuläret måste använda den här URL-parametern och tilldela den krypterade primärnyckeln till en Adobe Campaign-profil. Den krypterade primärnyckeln måste vara rätt URL-kodad (procent).

   ![chlimage_1-47](assets/chlimage_1-47a.png)

1. Lägg till komponenter i formuläret efter behov, t.ex. textfält, datumfält, kryssrutefält, alternativfält osv. Mer information om de olika komponenterna finns i [Adobe Campaign Form Components](/help/sites-authoring/adobe-campaign-components.md).
1. Klicka på ikonen Konfiguration för att öppna komponenten. I komponenten **Textfält (Campaign)** kan du till exempel ändra titeln och texten.

   Klicka på **Adobe Campaign** för att mappa formulärfältet till en Adobe Campaign-metadatavariabel. När du skickar formuläret uppdateras det mappade fältet i Adobe Campaign. Endast fält med matchande typer är tillgängliga i variabelväljaren (till exempel strängvariabler för textfält).

   ![chlimage_1-48](assets/chlimage_1-48a.png)

   >[!NOTE]
   >
   >Du kan lägga till/ta bort fält som visas i mottagartabellen genom att följa instruktionerna här: [https://blogs.adobe.com/experiencedelivers/experience-management/aem-campaign-integration/](https://blogs.adobe.com/experiencedelivers/experience-management/aem-campaign-integration/)

1. Klicka på **Publicera sida**. Sidan aktiveras på din webbplats. Du kan visa den genom att gå till publiceringsinstansen för AEM. Du kan också [testa ett formulär](#testing-a-form).

   >[!CAUTION]
   >
   >Du måste ange läsbehörighet för den anonyma användaren i molntjänsten för att kunna använda formulär vid publicering. Tänk dock på de potentiella säkerhetsproblemen med att ge läsbehörigheter till den anonyma användaren och se till att minska risken genom att till exempel konfigurera dispatchern.

## Testa ett formulär {#testing-a-form}

När du har skapat ett formulär och redigerat formulärinnehållet kanske du vill testa att formuläret fungerar som det ska.

>[!NOTE]
>
>Du måste ha en **krypterad primärnyckel** på varje formulär. I Komponenter väljer du Adobe Campaign så att bara de komponenterna visas.
>
>I den här proceduren anger du telefonnumret manuellt, men i praktiken får användarna en länk till den här sidan (om de vill avbeställa, prenumerera eller uppdatera din profil) i ett nyhetsbrev. Paketet uppdateras automatiskt beroende på användaren.
>
>Om du vill skapa den länken använder du variabeln **Huvudresursidentifierare** (Adobe Campaign Standard) eller **Krypterad identifierare** (Adobe Campaign Classic) (till exempel i en **Text- och Personalization-komponent (Campaign)**) som är länkad till sidan i Adobe Campaign.

Om du vill göra det måste du hämta EPK-filen för en Adobe Campaign-profil manuellt och sedan bifoga den till webbadressen:

1. Så här hämtar du den krypterade primärnyckeln (EPK) för en Adobe Campaign-profil:

   * I Adobe Campaign Standard - Navigera till **Profiler och målgrupper** > **Profiler** som visar de befintliga profilerna. Se till att tabellen visar fältet **Huvudresursidentifierare** i en kolumn (detta kan konfigureras genom att klicka/peka på **Konfigurera lista**). Kopiera huvudresursidentifieraren för den önskade profilen.
   * I Adobe Campaign Classic går du till **Profiler och mål** > **Mottagare** som visar de befintliga profilerna. Kontrollera att tabellen visar fältet **Krypterad identifierare** i en kolumn (detta kan konfigureras genom att högerklicka på en post och välja **Konfigurera lista..**). Kopiera den krypterade identifieraren för den önskade profilen.

1. I AEM öppnar du formulärsidan på publiceringsinstansen och lägger till EPK från steg 1 som en URL-parameter: använd samma namn som du tidigare definierade i EPK-komponenten när du redigerar formuläret (till exempel: `?epk=...`)
1. Formuläret kan nu användas för att ändra data och prenumerationer som är kopplade till den länkade Adobe Campaign-profilen. När du har ändrat vissa fält och skickat in formuläret kan du verifiera i Adobe Campaign att data har uppdaterats.

Data i Adobe Campaign-databasen uppdateras när ett formulär har validerats.
