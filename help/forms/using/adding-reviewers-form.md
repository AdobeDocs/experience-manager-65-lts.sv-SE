---
title: Koppla granskare som skickar in svar till ett formulär
description: Lär dig hur du associerar granskare med ett formulär i AEM Forms. Associerade granskare granskar ett formulär som skickats via formulärportalen.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: fbddbd62-2f21-4eb6-8926-90647846ee8b
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Koppla granskare som skickar in svar till ett formulär {#associating-submission-reviewers-with-a-form}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

När du skapar ett formulär kan du ange vilka användare som ska granska inskickade formulär via formulärportalen och ge feedback. Din organisation kan samla in feedback och omarbeta de inskickade formulären.

Med AEM Forms kan du associera en granskargrupp med ett formulär. Användare som läggs till i en granskningsgrupp i ett formulär kan se inskickade formulär och ge feedback.

Granskningsgrupper som tilldelats ett formulär kan bara granska inskickade svar från det angivna formuläret.

## Förutsättning {#prerequisite}

### Aktivera gruppegenskap för granskare som skickar in formulär med hjälp av schemaredigeraren för metadata {#enabling-submission-reviewer-groups-property-for-adaptive-forms-using-metadata-schema-editor}

Om du vill associera en granskargrupp med ett formulär redigerar du metadatarammet för anpassningsbara formulär. Som standard kan du inte lägga till en granskningsgrupp i ett skickat formulär.

Så här redigerar du metadataschema:

1. I redigeringsläget, under Experience Manager, klickar du på **Verktyg** > **Assets** > **Metadata Schemas**.
1. Gå till **Forms** > **Forms som är skapad i AEM.** på Forms-sidan Schema.

   Sidans URL är:

   ```html
   https://<hostname>:<port>/mnt/overlay/dam/gui/content/metadataschemaeditor/
    schemalist.html/forms/aem-authored
   ```

1. Välj **Adaptivt formulär** och klicka på **Redigera**.
1. Klicka på **Avancerat** på sidan Redigera formulär.
1. Dra och släpp komponenten **Enkelradig text** som finns under Skapa formulär på fliken Avancerat.
1. Markera den tillagda textkomponenten för att se dess inställningar.

   Under Inställningar anger du `./jcr:content/metadata/form-submission-reviewer-group` i fältet Mappa till egenskap.

   Fältet för gruppen Skicka granskare i de avancerade egenskaperna för ditt adaptiva formulär aktiveras med det namn du anger under Fältetikett.

## Koppla granskare som skickar in svar till ett formulär {#associating-submission-reviewers-with-a-form-1}

Om du vill associera granskare med ett anpassat formulär skapar du en granskargrupp och lägger till användare i den. Lägg till den skapade granskargruppen under fältet för att skicka in formulär i formulärets avancerade egenskaper.
Med användargrupper kan du associera olika uppsättningar av granskare med olika anpassningsbara formulär. Den här funktionen förhindrar att obehöriga skickar in granskningar.

Innan du utför följande steg ska du läsa [Förutsättning](../../forms/using/adding-reviewers-form.md#prerequisite).

Om du vill skapa en grupp och lägga till medlemmar i den går du till **Verktyg** > **Åtgärder** > **Säkerhet** > **Grupper**.
Mer information finns i [Användaradministration och -tjänster](/help/sites-administering/security.md).
Se till att du lägger till gruppen som du skapar som medlem i den körklara användargruppen: **forms-submission-reviewers**. Den här användargruppen levereras med AEM Forms och ser till att användare läggs till som granskare.

Så här associerar du användargrupper med ett anpassat formulär:

1. Gå till **Forms** > **Forms &amp; Documents** i redigeringsläget.
1. Använd alternativet **Välj &#x200B;** för att välja ett anpassat formulär och klicka på **Visa egenskaper**.
1. Klicka på **Redigera** i fönstret Egenskaper för formuläret och klicka sedan på **AVANCERAT**.
1. Ange gruppen i gruppfältet för granskare som ska skicka in och klicka på **Klar**.

   Fältet för granskningsgruppen som ska skickas visas med det namn som du angav i det redigerade metadataschemat för anpassade formulär.

>[!NOTE]
>
>Replikera användare och formulär för att säkerställa att de är tillgängliga för användare och formulär vid fjärrimplementeringen av AEM Forms.
>
>Se till att alla användare replikeras som granskningsmedlemmar i användargrupperna i fjärrimplementeringen.
