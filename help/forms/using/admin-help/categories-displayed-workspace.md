---
title: Hantera de kategorier som visas i Workspace
description: I Workspace visas de processer som en användare kan starta i kategorier i den vänstra navigeringsrutan. Lär dig hur du kan hantera de här kategorierna som visas i Workspace.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_workspace
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: f9ffbe56-757b-4fd0-b33a-2522695aed35
source-git-commit: 66696da39b1b790b2155b2ec08d936371f87b979
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 0%

---

# Hantera de kategorier som visas i Workspace {#managing-the-categories-displayed-in-workspace}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

I Workspace visas de processer som en användare kan starta i kategorier i den vänstra navigeringsrutan. Du kan konfigurera kategorierna i administrationskonsolen eller så kan processdesigners konfigurera dem i Workbench. När processdesigners skapar processer tilldelar de dem till kategorier.

När du anger kategorinamn skapar du dem så att de visas korrekt i navigeringsrutan i Workspace. Som standard har den vänstra navigeringsrutan en fast bredd på 210 pixlar, vilket är ungefär 24 tecken. Om kategorinamnet som du anger är för långt för att få plats inom den fasta bredden i den vänstra navigeringsrutan, trunkeras det. Det fullständiga namnet visas bara när muspekaren är placerad över det. Undvik kategorinamn som kortas av. I följande exempel visas kategorinamn som passar och de som är trunkerade:

**Kategorinamn som passar:** Närvaro och lämna

**Kategorinamn som har trunkerats:** Närvaro och ledighet (USA)

I Workspace visas processer i en kategori vanligtvis som kort på sidan Starta process. I allmänhet kan sex kort visas på skärmen för en kategori innan användaren måste rulla för att visa återstående kort. Eftersom rullning gör det svårare att hitta en process bör du överväga att begränsa varje kategori till sex processer eller, beroende på upplösningen, begränsa antalet processer som kan visas på skärmen utan att någon bläddring behövs.

Om du använder MySQL som AEM-formulärdatabas kan administrationskonsolen inte skilja mellan två kategorinamn som bara skiljer sig åt när utökade tecken används. Om du t.ex. skapar en kategori med namnet abcde och en kategori med namnet âbcdè, betraktas de som samma.

## Lägg till en kategori {#add-a-category}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Kategorihantering.
1. Klicka på Lägg till. Om du vill lägga till en underkategori markerar du en kategori och klickar sedan på Lägg till.
1. Skriv ett namn för kategorin i rutan Namn och skriv en beskrivning av kategorin i rutan Beskrivning.
1. Klicka på Lägg till. Kategorin visas på sidan Kategorihantering.

   ***Obs!**: Du kan bara lägga till upp till fem nivåer i hierarkin när du skapar kategorier.*

## Redigera en kategori {#edit-a-category}

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Kategorihantering.
1. Markera kategorin som du vill redigera och klicka på Redigera. Du kan också dubbelklicka på en kategori som du vill redigera.
1. Redigera namnet på kategorin i rutan Namn.

## Ta bort en kategori {#remove-a-category}

Du kan bara ta bort de kategorier som inte används.

1. I administrationskonsolen klickar du på Tjänster > Program och tjänster > Kategorihantering.
1. Markera kryssrutan för kategorin som ska tas bort på sidan Kategorihantering och klicka på Ta bort. Kategorin visas inte längre.
