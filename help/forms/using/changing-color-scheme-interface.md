---
title: Ändra gränssnittets färgschema
description: Hur man selektivt ändrar färgschemat i användargränssnittet i AEM Forms arbetsyta.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 887dc1d6d7e11672b62ef5ca5463ea6181ff0320
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Ändra gränssnittets färgschema {#changing-the-color-scheme-of-the-interface}

Du kan ändra färgschemat för användargränssnittets delar i AEM Forms arbetsyta så att de passar dina behov. Nedan följer några exempel på representativa färgschemaanpassningar. Förutom de steg som beskrivs i den här artikeln finns mer information i [Allmänna steg för anpassning av arbetsytan i AEM Forms](/help/forms/using/generic-steps-html-workspace-customization.md).

## Övre navigeringsfältet {#top-navigation-bar}

### Använda bakgrundsbild {#using-background-image}

Uppdatera navigeringsfältet högst upp på arbetsytan i AEM Forms.

1. Skapa en bakgrundsbild för att uppdatera färgen. Ge filen namnet newBackground.jpg.
1. Överför bakgrundsbildfilen i mappen /apps/ws/images med en WebDAV-klient.

   >[!NOTE]
   >
   >Mer information finns i [WebDAV-åtkomst](https://experienceleague.adobe.com/docs/experience-manager-65-2025/administering/contentmanagement/webdav-access.html?lang=en).

1. Referera till den nya bakgrundsbilden i /apps/ws/css/newStyle.css genom att lägga till följande format.

   ```css
   #header {
       background:#292929 url(../images/newBackground.jpg) repeat-x;
   }
   ```

### Använda egenskapen color i CSS {#using-color-property-in-css}

1. Lägg till följande format i newStyle.css på /apps/ws/css

   ```css
   #header {
   background : none;
   background-color: gray;
   }
   ```

## Kategorikomponent {#category-component}

Kategorikomponenten visar de olika kategorierna för dina uppgifter i den vänstra panelen. Om du vill ändra färgen definierar du bakgrundsfärgen i elementet `.category` i CSS-filen.

## Aktivitetskomponent {#task-component}

Uppgifter visas på den mittersta panelen som kallas TaskList-komponent. Om du vill ändra färgen ändrar du formatet som är associerat med aktivitetsväljaren i formatmallen.
