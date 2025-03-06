---
title: Publicera ett e-postmeddelande till e-postleverantörer
description: Du kan publicera nyhetsbrev till e-posttjänster som ExactTarget och Silverpop Engage.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: personalization
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Personalization
role: User
exl-id: 03890f75-bfbc-4f73-85ae-07e991728115
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1106'
ht-degree: 0%

---

# Publicera ett e-postmeddelande till e-postleverantörer{#publishing-an-email-to-email-service-providers}

Du kan publicera nyhetsbrev till e-posttjänster som ExactTarget och Silverpop Engage. I det här dokumentet beskrivs hur du konfigurerar AEM att publicera ett nyhetsbrev till dessa e-posttjänster.

>[!NOTE]
>
>Du måste konfigurera tjänsteleverantören innan du kan skapa och publicera ett e-postmeddelande. Mer information finns i [Konfigurera ExactTarget](/help/sites-administering/exacttarget.md) och [Konfigurera Silverpop Engage](/help/sites-administering/silverpop.md).

Om du vill publicera din e-post till e-postleverantören måste du utföra följande steg:

1. Skapa ett mejl.
1. Använd e-posttjänstkonfigurationen för e-postmeddelandet.
1. Publicera e-postmeddelandet.

>[!NOTE]
>
>Om du uppdaterar e-postleverantörer, gör ett flygtest eller skickar ett nyhetsbrev misslyckas dessa åtgärder om nyhetsbrevet inte publiceras till publiceringsinstansen först eller om publiceringsinstansen inte är tillgänglig. Glöm inte att publicera nyhetsbrevet och kontrollera att Publish-instansen körs.

## Skapa ett e-postmeddelande {#creating-an-email}

Ett e-postmeddelande eller nyhetsbrev som du vill publicera till en e-posttjänst kan skapas under en kampanj med mallen **Geometrixx Newsletter** . Du kan också använda mallen **Geometrixx Outdoors E-mail**. Exempel på e-post/nyhetsbrev som är baserade på mallen **Geometrixx Outdoors E-Mail** finns på `https://<hostname>:<port>/cf#/content/campaigns/geometrixx-outdoors/e-mails.html`.

Så här skapar du ett e-postmeddelande som publiceras till den konfigurerade e-posttjänsten:

1. Gå till **Webbplatser** och sedan till **Kampanjer**. Välj en kampanj.
1. Klicka på **Ny** för att öppna fönstret **Skapa sida**.
1. Ange titel, namn och välj mallen **Geometrixx Newsletter** i listan över tillgängliga mallar.
1. Klicka på **Skapa**.
1. Öppna det skapade e-postmeddelandet.
1. Växla till designläge för att välja de komponenter som du vill visa i sidosparken.
1. Växla till redigeringsläge och börja lägga till innehåll (text, bilder, [e-postverktyg](#adding-exacttarget-email-tools-to-your-email), [personaliseringsvariabler](#adding-text-and-personalization-tool-to-your-e-mail) och så vidare) i e-postmeddelandet.

### Lägga till e-postverktyg för ExactTarget i e-postmeddelandet {#adding-exacttarget-email-tools-to-your-email}

>[!NOTE]
>
>Det här avsnittet är specifikt för tjänsten ExactTarget.

Komponenten **E-postverktyg** för ExactTarget kan lägga till fler e-postfunktioner i e-post/nyhetsbrev.

1. Öppna ett e-postmeddelande som ska publiceras till ExactTarget.
1. Lägg till komponenten **ET - E-postverktyg** på sidan med hjälp av sidosparken. Öppna komponenten i redigeringsläge.

   ![chlimage_1](assets/chlimage_1.gif)

1. Välj ett alternativ på menyn **Alternativ**:

<table>
 <tbody>
  <tr>
   <td>Fysisk e-postadress (obligatoriskt)</td>
   <td>Den här komponenten infogar den fysiska e-postadressen till din organisation i ditt e-postmeddelande.</td>
  </tr>
  <tr>
   <td>Profilcenter (obligatoriskt)</td>
   <td>Profilcentret är en webbsida där prenumeranter kan ange och underhålla den personliga information som du sparar om dem.</td>
  </tr>
  <tr>
   <td>Visa e-post som en webbsida</td>
   <td>Med den här komponenten kan användaren visa e-postmeddelandet som en webbsida.</td>
  </tr>
  <tr>
   <td>Integritetspolicy</td>
   <td>Den här komponenten infogar länken till din sekretesspolicy i e-postmeddelandet.<br /> </td>
  </tr>
  <tr>
   <td>Avbeställ Center</td>
   <td>Ger användaren möjlighet att avbryta prenumerationen på din sändlista.</td>
  </tr>
  <tr>
   <td>Prenumerationscentral</td>
   <td>En prenumerationscentral är en webbsida där en prenumerant kan styra vilka meddelanden han eller hon får från din organisation.</td>
  </tr>
  <tr>
   <td>Spåra e-post öppnas</td>
   <td>En dold komponent som gör att du kan använda spårningsfunktionen ExactTarget.<br /> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Listrutan **Alternativ** fylls bara i om ExactTarget-konfigurationen används i e-postmeddelandet. Mer information finns i [Tillämpar e-posttjänstkonfiguration på e-postinställningar](#applying-e-mail-service-configuration-to-e-mail-settings).

1. Publicera e-postmeddelandet till ExactTarget.

   E-postmeddelandet med e-postverktygen är tillgängligt för användning i det konfigurerade ExactTarget-kontot.

>[!NOTE]
>
>* URL-adresserna i e-postverktygen ersätts (i det mottagna e-postmeddelandet) med sina faktiska värden endast när ett e-postmeddelande skickas med **Enkel sändning** eller **Guidad sändning**, men inte **Testa skicka**.
>
>* Två av e-postverktygen krävs: **Fysisk e-postadress (obligatoriskt)** och **Profilcenter (obligatoriskt)**. När e-postmeddelandet publiceras på ExactTarget läggs dessa två e-postverktyg till längst ned i varje e-post som standard.
>

### Lägga till text och Personalization i e-postmeddelanden {#adding-text-and-personalization-tool-to-your-e-mail}

Du kan lägga till anpassade fält i ett e-postmeddelande genom att lägga till komponenten **Text och Personalization** på sidan:

1. Öppna det e-postmeddelande som ska publiceras till din e-posttjänst.
1. Om du vill aktivera anpassningsfältet från din e-posttjänst lägger du till ramverkskonfigurationen när du konfigurerar e-posttjänsten. Mer information finns i [konfigurera Silverpop Engage](/help/sites-administering/silverpop.md) och [konfigurera exakt mål](/help/sites-administering/exacttarget.md).
1. Lägg till komponenten **Text och Personalization** från sidosparken. Den här komponenten ingår i gruppen nyhetsbrev. Öppna den här komponenten i redigeringsläget.

   ![chlimage_1-110](assets/chlimage_1-110a.png)

1. Lägg till det anpassade fältet till texten genom att markera fältet i listrutan och klicka på **Infoga**.
1. Klicka på **OK** för att slutföra.

## Tillämpar e-posttjänstkonfiguration för e-postinställningar {#applying-e-mail-service-configuration-to-e-mail-settings}

Så här använder du e-posttjänstkonfigurationen i ett nyhetsbrev:

1. Skapa en konfiguration för e-posttjänsten.
1. Öppna e-post/nyhetsbrev.
1. Öppna inställningarna för e-post/nyhetsbrev genom att antingen klicka på **Inställningar** eller genom att klicka på **Sidegenskaper i** sidosparken.
1. Klicka på **Lägg till tjänst** på fliken **Molntjänster**. Du ser listan över tjänster. Välj önskad konfiguration - antingen **ExactTarget** eller **Silverpop** - från listan i listrutan.

   ![chlimage_1-5](assets/chlimage_1-5a.jpeg)

1. Klicka på **OK**.

## Publicera e-postmeddelanden till e-posttjänsten {#publishing-emails-to-email-service}

E-post/nyhetsbrev kan publiceras till din e-posttjänst genom att följa dessa steg:

1. Öppna mejlet.
1. Innan du publicerar ett e-postmeddelande måste du kontrollera att du har tillämpat rätt konfiguration i e-postmeddelandet.
1. Klicka på **Publicera**. Då öppnas fönstret **Publicera nyhetsbrev till e-postleverantör**.
1. Fyll i fältet **Namn på nyhetsbrev**. E-postmeddelandet/nyhetsbrevet publiceras till en e-postleverantör med det här namnet. Om inget e-postnamn anges publiceras e-postmeddelandet med det nya nyhetsbrevet i AEM.
1. Klicka på **Publicera**.

   ![chlimage_1-6](assets/chlimage_1-6.jpeg)

   Om det lyckas bekräftar AEM att du kan visa e-postmeddelandet i ExactTarget eller Silverpop Engage.

   Om det finns ExactTarget kan det publicerade e-postmeddelandet ha visats genom att klicka på **Visa publicerad e-post**. Detta tar dig direkt till det publicerade nyhetsbrevet i ExactTarget ([https://members.exacttarget.com/](https://members.exacttarget.com/).).

>[!NOTE]
>
>Om ett e-postmeddelande/nyhetsbrev publiceras med samma namn som ett redan publicerat e-postmeddelande/nyhetsbrev ersätts inte det tidigare e-postmeddelandet/nyhetsbrevet. I stället skapas ett nytt e-postnyhetsbrev med samma namn (ID:n för två nyhetsbrev är dock olika).
>
>När du publicerar e-post/nyhetsbrev till e-postleverantören publiceras även e-post/nyhetsbrevet till AEM publiceringsinstans.
>

### Uppdatera ett publicerat e-postmeddelande {#updating-a-published-e-mail}

Med knappen **Uppdatera** i dialogrutan Publicera kan du uppdatera ett nyhetsbrev som redan har publicerats till en e-postleverantör. Om nyhetsbrevet ännu inte har publicerats och knappen **Uppdatera** klickas visas inget **nyhetsbrev** .

Så här uppdaterar du ett publicerat e-postmeddelande:

1. Öppna e-postmeddelandet/nyhetsbrevet som tidigare har publicerats hos en e-postleverantör som du vill publicera igen när du har gjort ändringar i e-postmeddelandet/nyhetsbrevet.
1. Klicka på **Publicera**. Fönstret **Publicera nyhetsbrev till e-postleverantör** visas. Klicka på **Uppdatera**.

   Om du vill kontrollera om e-postmeddelandet/nyhetsbrevet har uppdaterats på ExactTarget klickar du på **Visa publicerad e-post**. Detta tar dig till det publicerade e-postmeddelandet i ExactTarget.

   Om du vill kontrollera om e-post/nyhetsbrev har uppdaterats på Silverpop Email Service går du till webbplatsen Silverpop Engage.
