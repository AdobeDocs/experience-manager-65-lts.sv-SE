---
title: Använda elektroniska signaturer i ett formulär med hjälp av klottersignaturer
description: Lär dig hur du signerar AEM Adaptive Forms med hjälp av en signatur. Du kan använda steget för att skriva signatur och signatur för att rita signaturen i ett formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 9d1a22da-2eb3-4c79-8c4d-4d0a3ed7fe3b
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# Använda elektroniska signaturer i ett formulär med hjälp av klottersignaturer{#apply-electronic-signatures-to-a-form-using-deprecated-scribble-signatures}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>


## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/add-components-to-an-adaptive-form/signing-forms-using-scribble.html?lang=sv-SE).


Du kan använda komponenten **Skriptsignatur** och komponenten **Signatursteg** för att rita (Klottra) signatur i ett anpassat formulär. Underskriftsstegkomponenten visar en PDF-version av det adaptiva formuläret. Du måste aktivera alternativet Dokument för post eller formulärmallsbaserade adaptiva formulär för att kunna använda signaturstegskomponenten.

![Dialogrutan Klottertecken](/help/forms/using/assets/scribble-signature.png)

## Olika alternativ finns i signaturfönstret

* **A:** Klicka på ikonen **Målarpensel** för att rita din signatur på arbetsytan.
* **B:** Klicka på ikonen **Rensa** för att rensa signaturen på arbetsytan.
* **C:** Klicka på ikonen **Geolocation** för att lägga till geopositionering tillsammans med signaturen.
* **D:** Klicka på ikonen **Tangentbord** för att skriva ditt namn på arbetsytan.

När du har markerat ikonen Klar![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) i fönstret Klottsignatur kan du inte redigera signaturen. Om du vill redigera signaturen måste du bortse från den aktuella signaturen och signera igen med alternativet Målningspensel/Tangentbord ovan.

Du kan välja ikonen **Konfigurera** ![konfigurera](assets/configure.png) om du vill ange proportioner för arbetsytan för klottersignaturer.
* När proportionerna för arbetsytan Klottra signatur är mindre än 1 läggs platsinformationen till längst ned på arbetsytan Klottra signatur.

* När proportionerna för arbetsytan Klottra signatur är större än 1 läggs geopositioneringsinformationen till till höger på arbetsytan Klottra signatur.

![scribble signature-bottom](/help/forms/using/assets/scribble-signature-aspectratio.PNG)


>[!NOTE]
>
>Signaturer sparas alltid i PNG-format.
>

## Konfigurera ett anpassningsbart formulär att använda en skriptsignatur {#configure-an-adaptive-form-to-use-scribble-signature}

1. Alternativet Skapa ett postdokument aktiverat eller ett anpassat formulär som bygger på en formulärmall. Stegvis information finns i [Skapa ett anpassat formulär](../../forms/using/creating-adaptive-form.md).
1. Dra och släpp komponenten **Klottsignatur** från komponentwebbläsaren till det adaptiva formuläret.
1. Välj ikonen **Konfigurera** ![konfigurera](assets/configure.png) . Egenskaper öppnas i webbläsaren och egenskaper för komponenten Skriptsignatur visas. Konfigurera egenskaper för komponenten Scribble Signature.
1. Dra och släpp signaturstegskomponenten från komponentwebbläsaren till det anpassningsbara formuläret.

   >[!NOTE]
   >
   >Komponenten Signatursteg får full bredd som är tillgänglig för formuläret. Vi rekommenderar att du inte har någon annan komponent i avsnittet som innehåller komponenten Signatursteg.
   >

1. Välj **Formulärbehållare** i innehållsläsaren och välj ikonen **Konfigurera** ![konfigurera](/help/forms/using/assets/configure.png). Egenskaper öppnas i webbläsaren och egenskaper för behållare för adaptiva formulär visas. Navigera till **Adaptiv formulärbehållare** > **Elektronisk signatur** och avmarkera alternativet **Aktivera Adobe Sign**. Välj ikonen Klar ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att spara ändringarna.

   >[!NOTE]
   >
   >När du lägger till en komponent för signatursteg i ett anpassat formulär markeras alternativet Aktivera Adobe Sign automatiskt.
   >

1. Välj ikonen **Konfigurera** ![konfigurera](assets/configure.png) . Egenskaper öppnas i webbläsaren och egenskaper för signatursteg visas. Konfigurera följande egenskaper:

   * **Elementnamn**: Ange komponentens namn.

   * **Titel:** Ange komponentens unika titel.
   * **Mallmeddelande:** Ange meddelandet som ska visas när signaturen PDF läses in. Adobe Sign-tjänster tar lite tid att förbereda och läsa in signaturer i PDF.
   * **Signeringstjänst:** Välj alternativet **Klottsignatur** .

   * **CSS-klass**: Ange CSS-klass för klientbiblioteket, om sådan finns. Använd [teman](../../forms/using/themes.md) och [ infogade format](../../forms/using/inline-style-adaptive-forms.md) i stället för CSS-klassen.

   Välj ikonen Klar ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att spara ändringarna. Signaturen har konfigurerats.

   När du fyller i ett formulär visas nu en PDF-version av anpassningsbara formulär och alternativ för att signera PDF-dokumentet. Mer information finns i [Signera ett anpassat formulär med Klottsignatur](../../forms/using/signing-forms-using-scribble.md#sign-an-adaptive-form-using-scribble-signature).

## Signera ett anpassat formulär med Scribble Signature {#sign-an-adaptive-form-using-scribble-signature}

1. När du har fyllt i ett anpassat formulär och kommit till sidan Signatursteg visas signaturskärmen.

   ![Dialogrutan Klottertecken](/help/forms/using/assets/esignscribblesign.jpg)

1. Klicka på **[!UICONTROL Sign]**. Dialogrutan för klottersignering visas. Signera formuläret och klicka på ikonen Klar ![aem_6_3_forms_save](assets/aem_6_3_forms_save.png) för att spara signaturen.

   ![Dialogrutan Klottertecken](/help/forms/using/assets/scribblewidget.png)

1. Klicka på Slutför för att slutföra signeringsprocessen.

   ![Slutför signeringsprocessen](/help/forms/using/assets/scribblecomplete.jpg)

Signaturerna läggs till i formuläret och formulärkontrollen flyttas till nästa panel.
