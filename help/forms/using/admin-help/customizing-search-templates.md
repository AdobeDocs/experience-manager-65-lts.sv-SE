---
title: Anpassa sökmallar
description: Du kan skapa sökmallar som ska användas i Workspace för att söka efter instanser av processer från To Do- och Tracking-sidorna. Du kan också redigera eller ta bort befintliga sökmallar.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_workspace
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 7e6346ec-3cab-4f88-91b3-b111bd19983e
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 0%

---

# Anpassa sökmallar {#customizing-search-templates}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan skapa sökmallar som ska användas i Workspace för att söka efter instanser av processer från To Do- och Tracking-sidorna. Du kan också redigera eller ta bort befintliga sökmallar.

När du skapar eller redigerar en sökmall kan du ange sökresultatens layout och sorteringsordning. Användare kan dock ändra de här inställningarna i Workspace när sökresultaten visas.

Du kan skapa så många sökmallar som behövs.

>[!NOTE]
>
>När du sparar en sökmall måste du ge den ett unikt namn. Annars kan en befintlig mall skrivas över utan ett varningsmeddelande.

## Skapa en enkel sökmall {#create-a-simple-search-template}

1. I administrationskonsolen klickar du på Tjänster > Workspace > Sökmallar.
1. Ange syftet med mallen i rutan Sökmallsbeskrivning på fliken Identifiering.
1. (Valfritt) Klicka på fliken Kriterier och ange sökvillkoren för mallen.
1. Klicka på fliken Spara, ange ett unikt namn för mallen och klicka sedan på Spara.

## Skapa eller redigera en sökmall {#create-or-edit-a-search-template}

1. I administrationskonsolen klickar du på Tjänster > Workspace > Sökmallar.
1. (Valfritt) Om du redigerar en befintlig mall eller använder en befintlig mall som bas för en ny mall, väljer du mallen i listan Sökmallsnamn.
1. Ange syftet med mallen i rutan Sökmallsbeskrivning.
1. (Valfritt) I rutan Användarinstruktioner anger du eventuella instruktioner som kan vara till hjälp när du använder mallen. Dessa instruktioner visas i Workspace när en användare väljer sökmallen.
1. Klicka på fliken Villkor. Här definierar du ett eller flera sökvillkor. Så här lägger du till ett sökvillkor:

   * Välj ett processelement eller ett aktivitetselement högst upp på fliken Villkor.

     **Tips**: *Om du tidigare har markerat processnamnselementet och angett en process kan även alla processvariabler som definieras i den processen väljas.*

     **Tips**: *Om du väljer elementet Task Visible kan användarna ta bort slutförda uppgifter från sökresultaten.*

     Sökvillkorsfälten för det markerade elementet visas längst ned på fliken Kriterier.

   * Fyll i motsvarande sökfält längst ned på fliken Villkor för varje processelement, aktivitetselement och processvariabel som du väljer:

      * Välj en relationsoperator (till exempel &quot;vara lika med&quot;) i listan och ange operandvärdet i rutan bredvid.
      * (Valfritt) Om du vill att användare ska kunna ändra operandvärdet i Workspace väljer du Tillåt användaren att ändra operanden.
      * (Valfritt) Om du vill att användare ska kunna ändra relationsoperatorn markerar du Tillåt användaren att välja en annan relationsoperator. I listan som visas väljer du de operatorer som ska vara tillgängliga för användaren.

     **Tips**: *Om du valde Processnamn som element kan du klicka på ikonen bredvid operandfältet för att visa en lista där du kan välja en process som körs på Forms Server. När du har valt en process är alla processvariabler som definieras i den processen tillgängliga för val under Processvariabler i det övre avsnittet på fliken Kriterier.*

     **Tips**: *Du kan ta bort ett element från sökmallen genom att klicka på ikonen Ta bort bredvid elementets sökvillkor.*

1. (Valfritt) För varje kolumnrubrik som ska visas i sökresultaten klickar du på fliken Layout och utför följande steg:

   * Markera en process eller ett uppgiftselement och klicka på högerpilen för att flytta det till listan Kolumner att rapportera.
   * Markera process- eller uppgiftselementet i listan Kolumner att rapportera och klicka på uppilen eller nedpilen för att flytta det till sin plats i kolumnordningen. Kolumnrubrikerna i sökresultaten visas i den ordning som de listas här.
   * (Valfritt) Om du vill ändra namnet på elementet för kolumnrubriken markerar du elementet i listan Kolumner att rapportera och anger det nya namnet.

   >[!NOTE]
   >
   >Layouten som anges i sökmallen åsidosätter användarens inställningar som anges för kolumnrubriker i Workspace.

1. (Valfritt) För varje kolumn som du vill sortera i sökresultaten klickar du på fliken Sortera och utför följande steg:

   * Markera en process eller ett uppgiftselement och klicka på högerpilen för att flytta det till listan Sorteringsordning.
   * I listan Sorteringsordning markerar du process- eller uppgiftselementet och klickar på uppilen eller nedpilen för att flytta det till dess plats i sorteringsordningen. Kolumnerna i sökresultaten sorteras i den ordning som de listas här.
   * (Valfritt) Om du vill sortera en kolumn i fallande ordning markerar du kryssrutan bredvid elementnamnet. Om kryssrutan inte är markerad sorteras kolumnen i stigande ordning.

1. Klicka på fliken Spara.
1. (Valfritt) Om du skapar en sökmall ger du den ett unikt namn. Om du inte anger ett unikt namn kan du skriva över en befintlig mall.
1. Klicka på knappen Spara.

## Ta bort en sökmall {#delete-a-search-template}

1. Välj ett namn i listan Sökmallsnamn på fliken Identifiering.
1. Klicka på Ta bort den här mallen och klicka på OK.
