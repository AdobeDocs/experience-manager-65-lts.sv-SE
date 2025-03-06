---
title: Visa komponenter baserat på den mall som används
description: När du skapar ett formulär ska du lära dig hur du kan aktivera komponenter i sidofältet baserat på den valda mallen.
contentOwner: sashanka
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: customization
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms,Foundation Components
exl-id: e0986f82-a049-44d4-bf4c-e2f020315ce5
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Visa komponenter baserat på den mall som används{#displaying-components-based-on-the-template-used}

När en formulärförfattare skapar ett anpassat formulär med hjälp av en [mall](../../forms/using/template-editor.md) kan formulärförfattaren se och använda specifika komponenter som är baserade på mallprincipen. Du kan ange en innehållsprincip för mallar där du kan välja en grupp komponenter som formulärförfattaren ser när formuläret skapas.

## Ändra innehållsprincipen för en mall {#changing-the-content-policy-of-a-template}

När du skapar en mall skapas den under `/conf` i innehållsdatabasen. Baserat på de mappar du har skapat i katalogen `/conf` är sökvägen till mallen: `/conf/<your-folder>/settings/wcm/templates/<your-template>`.

Följ de här stegen för att visa komponenterna i sidofältet baserat på en malls innehållsprincip:

1. Öppna CRXDE lite.\
   URL: `https://<server>:<port>/crx/de/index.jsp`
1. I CRXDE navigerar du till mappen där mallen skapas.

   Till exempel: `/conf/<your-folder>/`

1. I CRXDE navigerar du till: `/conf/<your-folder>/settings/wcm/policies/fd/af/layouts/gridFluidLayout/`

   Om du vill välja en grupp komponenter krävs en ny innehållsprincip. Om du vill skapa en profil kopierar och klistrar du in standardprincipen och byter namn på den.

   Sökvägen till standardinnehållsprincipen är: `/conf/<your-folder>/settings/wcm/policies/fd/af/layouts/gridFluidLayout/default`

   Kopiera och klistra in standardprincipen i mappen `gridFluidLayout` och byt namn på den. Exempel: `myPolicy`.

   ![Kopierar standardprinciper](assets/crx-default1.png)

1. Välj den nya profilen som du skapar och välj egenskapen **components** på den högra panelen med typen `string[]`.

   När du markerar och öppnar komponentegenskapen visas dialogrutan Redigera komponenter. I dialogrutan Redigera komponenter kan du lägga till eller ta bort komponentgrupper med knapparna **+** och **-**. Du kan lägga till komponentgrupper som innehåller komponenter som du vill att författarna ska använda.

   ![Lägg till eller ta bort komponenter i principen](assets/add-components-list1.png)

   När du har lagt till en komponentgrupp klickar du på **OK** för att uppdatera listan och sedan på **Spara alla** ovanför adressfältet för CRXDE och uppdatera.

1. I mallen ändrar du innehållsprincipen från standard till den nya profilen som du skapade. ( `myPolicy` i detta exempel.)

   Om du vill ändra principen går du till `/conf/<your-folder>/settings/wcm/templates/<your-template>/policies/jcr:content/guideContainer/rootPanel/items` i CRXDE.

   I egenskapen `cq:policy` ändrar du `default` till det nya principnamnet ( `myPolicy`).

   ![Uppdaterad princip för mallinnehåll](assets/updated-policy.png)

   När du skapar ett formulär som du skapar med hjälp av mallen kan du se de komponenter som lagts till i sidofältet.
