---
title: Skapa nya mappar för att kategorisera formulär
description: Använd mappar för att ordna formulärmallar, PDF-filer, resurser och anpassningsbara formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-manager
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Skapa nya mappar för att kategorisera formulär {#create-new-folders-to-categorize-forms}

Du kan ordna dina resurser bättre med hjälp av mappar. Eftersom AEM Forms har stöd för flera typer av resurser - formulärmallar, PDF-filer, dokument, resurser och anpassningsbara formulär med olika metadata - kan du använda mappar för att kategorisera formulären baserat på önskade kriterier.

Med AEM Forms kan du ändra en mapps namn. Titeln är inte samma som namnet på noden som mappen lagras i databasen under. I stället bevaras titeln som metadata för mappen. Om du ändrar namnet på en mapp påverkas inte sökvägen till resurserna i mappen.

## Skapa en mapp {#create-a-folder}

Du kan skapa en mapp i AEM Forms på något av följande sätt:

* Överför en ZIP-fil som innehåller resurser i den önskade mappstrukturen (se [Hämta XDP- och PDF-dokument i AEM Forms](/help/forms/using/get-xdp-pdf-documents-aem.md))

* Skapa en tom mapp

1. Logga in på AEM Forms användargränssnitt på `https://<server>:<port>/aem/forms.html`.
1. Navigera till den plats där du vill skapa en mapp.
1. Klicka på ikonen ![aem6forms_add](assets/aem6forms_add.png) i verktygsfältet och välj sedan **[!UICONTROL Create Folder]**.

1. Ange följande information:

   * **Titel:** Visningsnamn för mappen
   * **Namn:** *(Obligatoriskt)* Det nodnamn under vilket du vill lagra mappen i databasen

   >[!NOTE]
   >
   >Som standard fylls värdet för namnfältet automatiskt i från titeln. Namnet får bara innehålla alfanumeriska tecken eller bindestreck (-) och understreck (_). Alla andra specialtecken som anges i titeln ersätts automatiskt med ett bindestreck och du uppmanas att bekräfta det nya namnet. Du kan välja att fortsätta med det föreslagna namnet eller redigera det ytterligare.

1. Klicka på **[!UICONTROL Submit].**

   En ny mapp med den titel du har definierat visas på den aktuella platsen i resurslistan.

   Om det finns en mapp med det angivna namnet misslyckas överföringen. Du kan visa felmeddelandet genom att hovra över ikonen ![aem6forms_error_alert](assets/aem6forms_error_alert.png) som visas bredvid namnfältet.

### Redigera mapptiteln {#edit-the-folder-title-br}

1. Markera den mapp vars titel du vill redigera.
1. Klicka på ikonen Redigera ![aem6forms_edit](assets/aem6forms_edit.png) i verktygsfältet.
1. Ange den nya titeln. Textfältet är förifyllt med det aktuella värdet för mapptiteln. Du kan ändra det till ett nytt värde.
1. Klicka på **[!UICONTROL Submit].**
