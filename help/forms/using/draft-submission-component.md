---
title: Komponenten Utkast och inskickat material
description: Komponenten Utkast och inskickningar listar formulär som är i utkastläge och som redan har skickats. Du kan anpassa komponentens utseende och stil.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: publish
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Forms Portal
role: Admin, User, Developer
exl-id: 3d4ff4d1-aab6-47b9-9804-2a0f3438332d
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Komponenten Utkast och inskickat material{#drafts-and-submissions-component}

Komponenten Utkast och inskickningar visar alla formulär som är i utkastläge och de formulär som redan har skickats. Komponenten har separata avsnitt (flikar) för utkast och skickade formulär. Användarna kan bara visa sina utkast och skickade formulär.

## Konfigurera komponenten {#configuring-the-component}

Komponenten Utkast och inskickningar har två flikar: Utkast och inskickningar.

Om du vill att ett anpassat formulär ska kunna skickas på fliken Skicka anger du **åtgärden Skicka** till **[Forms Portal-åtgärden &#x200B;](../../forms/using/configuring-submit-actions.md). Alternativt kan** aktivera alternativet Skicka i Forms Portal. När en användare skickar formuläret läggs formuläret till på fliken Skicka.

Funktionen Utkast är aktiverad direkt. När en användare klickar på **Spara** i ett anpassat formulär läggs formuläret till på fliken Utkast.

Utför följande steg för att lägga till och konfigurera en komponent för utkast och överföringar:

1. Dra och släpp komponenten **Utkast och överföringar** under kategorin Dokumenttjänster i komponentwebbläsaren till sidan.
1. Markera komponenten och välj sedan ![settings_icon](assets/settings_icon.png) för att öppna dialogrutan Redigera för komponenten.

   ![Komponenten Utkast och överföring](assets/drafts-submissions-edit.png)

1. Ange följande information i dialogrutan Redigera och välj **Klar** för att spara inställningarna.

<table>
 <tbody>
  <tr>
   <th>Tabb</th>
   <th>Konfiguration</th>
   <th>Beskrivning</th>
  </tr>
  <tr>
   <td>Allmänt</td>
   <td>Totalt resultat</td>
   <td>Anger det maximala antalet resultat som ska visas. Om antalet resultat ökar gränsen för totalt resultat visas länken <strong>Mer </strong>längst ned i komponenten. Om du klickar på <strong>Mer </strong>visas alla formulär. </td>
  </tr>
  <tr>
   <td> </td>
   <td>Formattyp</td>
   <td>Anger komponentens format. Du kan ange <strong>Inget format</strong>, <strong>Standardformat</strong> eller <strong>Anpassat format</strong> för att visa formulären. För alternativet Eget format kan du ange sökvägen till en anpassad CSS-fil i fältet <strong>Anpassad formatsökväg </strong><strong>.</strong></td>
  </tr>
  <tr>
   <td> </td>
   <td>Anpassad formatsökväg</td>
   <td>Om du väljer alternativet <strong>Egen stil</strong> i fältet <strong>Formattyp</strong> använder du fältet <strong>Anpassad formatsökväg</strong> för att ange sökvägen till den anpassade CSS-filen. </td>
  </tr>
  <tr>
   <td> </td>
   <td>Visningsalternativ</td>
   <td><p>Anger vilka flikar som ska visas. Du kan välja att visa formulärutkast, skickade formulär eller både och. </p> <p><strong>Obs!</strong>:<em> Om du väljer ett annat alternativ än <strong>Båda</strong> för <strong>visningsalternativ</strong> används inte fältalternativet <strong>Standardflik</strong>.</em></p> </td>
  </tr>
  <tr>
   <td> </td>
   <td>Standardflik</td>
   <td>Anger fliken som ska visas när formulärportalsidan läses in. Du kan välja mellan <strong>Utkast av Forms-flik</strong> och <strong>Skickat på Forms-flik</strong>.</td>
  </tr>
  <tr>
   <td>Utkast till Forms Tab Configuration</td>
   <td>Egen titel</td>
   <td>Anger rubriken på fliken <strong>Utkast-Forms</strong>. Standardvärdet är <strong>Draft Forms.</strong></td>
  </tr>
  <tr>
   <td> </td>
   <td>Layoutmall</td>
   <td>Anger layouten som ska användas för listan med utkast till Forms.</td>
  </tr>
  <tr>
   <td>Inskickad Forms-flikkonfiguration</td>
   <td>Egen titel </td>
   <td>Anger rubrik på fliken <strong>Skickat Forms </strong>. Standardvärdet är <strong>Skickat Forms.</strong></td>
  </tr>
  <tr>
   <td> </td>
   <td>Layoutmall</td>
   <td>Anger den layout som ska användas för den skickade Forms<strong> </strong>listan. </td>
  </tr>
 </tbody>
</table>

## Anpassa lagringen {#customizing-the-storage}

När du använder Forms Portal-åtgärden eller aktiverar alternativet Lagra data i formulärportalen i anpassat format, lagras formulärdata i AEM-databasen. I en produktionsmiljö bör du inte lagra utkast eller inskickade formulärdata i AEM-databasen. Istället måste ni integrera utkasten och skicka-komponenten med en säker lagringsplats, t.ex. en företagsdatabas, för att lagra utkast och skickade formulärdata.

Med Forms Portal kan du lagra data i en lokal AEM-databas, AEM fjärrdatabas eller i en databas. Med AEM Forms kan ni anpassa implementeringen av lagring av användardata för utkast och inskickade data. Du kan åsidosätta standardmetoder för att ange hur utkast- och inskickningsdata ska lagras på ett valfritt lagringsutrymme. Du kan till exempel lagra data i ett datalager som är implementerat i din organisation.

Forms Portal innehåller färdiga API:er för lagring av data i krx-databaser för AEM Forms publiceringsinstanser lokalt och på fjärrbasis. Du kan ersätta standardimplementeringarna, som beskrivs i artikeln [Konfigurera lagringstjänster för utkast och överföringar](/help/forms/using/configuring-draft-submission-storage.md), med anpassade implementeringar som ersätter standardfunktionerna. Mer information om vilka metoder som krävs i en anpassad implementering för att lagra innehåll på en skyddad plats finns i [Anpassa tjänster för utkast- och överföringsdata](/help/forms/using/custom-draft-submission-data-services.md) och [Anpassad lagring för komponenter för utkast och inskickning.](/help/forms/using/adding-custom-storage-provider-forms.md)

I AEM Forms-dokumentationen finns ett [exempel för integrering av utkast och inskickningskomponenter i databasen](integrate-draft-submission-database.md). Du kan använda exempelimplementeringen för att utveckla en egen anpassad implementering.

## Relaterade artiklar

* [Aktivera komponenter i formulärportalen](/help/forms/using/enabling-forms-portal-components.md)
* [Skapa portalsida för formulär](/help/forms/using/creating-form-portal-page.md)
* [Visa formulär på en webbsida med API:er](/help/forms/using/listing-forms-webpage-using-apis.md)
* [Använda komponenten Utkast och inskickat material](/help/forms/using/draft-submission-component.md)
* [Anpassa lagring av utkast och inskickade formulär](/help/forms/using/draft-submission-component.md)
* [Exempel för att integrera komponent för utkast och inlämning med databas](/help/forms/using/integrate-draft-submission-database.md)
* [Anpassa mallar för komponenter i formulärportalen](/help/forms/using/customizing-templates-forms-portal-components.md)
* [Introduktion till att publicera formulär på en portal](/help/forms/using/introduction-publishing-forms.md)
