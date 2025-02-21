---
title: Återanvända adaptiva formulär
description: Du kan återanvända ett befintligt adaptivt formulär för att skapa nya adaptiva formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author
discoiquuid: ef564750-f107-41cb-887e-fc6d22b7d32e
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---

# Återanvända adaptiva formulär {#reusing-adaptive-forms}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/manage-metadata/reusing-adaptive-forms.html) |
| AEM 6.5 | Den här artikeln |

## Introduktion {#introduction}

Om du vill använda vissa av egenskaperna i ett befintligt adaptivt formulär för att generera ett nytt, behöver du bara använda funktionen för att kopiera och klistra in. Dessutom kan du klistra in det nya adaptiva formuläret i önskad mappsökväg. Alla metadataegenskaper replikeras och XFA- och XSD-värdena för XFA- och XSD-baserade adaptiva formulär kopieras också.

>[!NOTE]
>
>Status och granskningsinformation kopieras inte. Om det adaptiva formuläret till exempel publiceras och sedan kopieras, är det inklistrade adaptiva formuläret inte publicerat. På samma sätt gäller att om en kopierad resurs håller på att granskas är den inklistrade resursen inte under samma granskning.

### Kopiera ett anpassat formulär {#copy-an-adaptive-form}

Kopiera ett anpassat formulär på något av följande sätt:

1. Klicka på ikonen Kopiera ![aem6forms_copy](assets/aem6forms_copy.png) från snabbåtgärder.

   >[!NOTE]
   >
   >Snabbåtgärder är de åtgärdsobjekt som visas över en miniatyrbild när du håller muspekaren.

1. Markera det adaptiva formuläret. Markeringsprocessen är annorlunda för olika vyer.

   Om du är i kortvyn går du till markeringsläget genom att klicka på ikonen ![aem6forms_check-circle](assets/aem6forms_check-circle.png) och klickar på alla adaptiva formulär som du vill kopiera.

   Om du är i listvyn markerar du kryssrutorna för alla adaptiva formulär.

   >[!NOTE]
   >
   >Alla markerade resurser måste vara adaptiva formulär eftersom funktionen kopiera och klistra in bara stöds för adaptiva formulär, och alla resurser som är markerade måste finnas i samma mapp.

   När du har markerat resurserna klickar du på ikonen för kopiering av ![aem6forms_copy](assets/aem6forms_copy.png) i verktygsfältet för att kopiera det valda adaptiva formuläret.

### Klistra in ett anpassat formulär {#paste-an-adaptive-form}

Om du klickar på kopieringsåtgärden avslutas markeringsläget automatiskt och ikonen för att klistra in ![aem6forms_paste](assets/aem6forms_paste.png) visas. Gå nu till önskad mappsökväg och klicka på ikonen Klistra in ![aem6forms_paste](assets/aem6forms_paste.png) för att klistra in det kopierade adaptiva formuläret.

Om du klistrar in i samma mapp eller en annan fil med samma nodnamn (som den lagras i CRX-databasen med) finns i den här målmappen läggs 1 till i suffixet (till exempel blir myaf1 och om myaf1 finns på samma plats blir myaf2. Alla andra egenskaper är desamma som det ursprungliga adaptiva formuläret.

När du har klickat på ikonen Klistra in ![aem6forms_paste](assets/aem6forms_paste.png) döljs den igen. Du kan bara klistra in en gång. Om du vill skapa en kopia av samma resurs kopierar du den igen.

### Ändra innehållet i det nya adaptiva formuläret {#change-contents-of-new-adaptive-form}

Innehållet i en inklistrad adaptiv form kan ändras på följande sätt så att det skiljer sig från det kopierade formuläret:

1. **Ändra metadataegenskaper:**

   Du kan ändra metadataegenskaperna för det adaptiva formuläret, till exempel rubrik och beskrivning. Mer information om metadataegenskaper och hur de kan ändras finns i [Hantera formulärmetadata](/help/forms/using/manage-form-metadata.md)

1. **Ändra XFA/XSD för XFA/XSD-baserad anpassad Forms:**

   Du kan ändra den XFA/XSD som används i adaptiva formulär. Mer information om hur dessa adaptiva formulär kan ändras finns i [Hantera formulärmetadata](/help/forms/using/manage-form-metadata.md)

1. **Publicera igen:**

   Den inklistrade resursen skiljer sig från den kopierade. Du kan publicera den som en ny resurs för att göra den tillgänglig för slutanvändare. Mer information om hur du publicerar en resurs finns i [Publicera och avpublicera formulär](/help/forms/using/publishing-unpublishing-forms.md)
