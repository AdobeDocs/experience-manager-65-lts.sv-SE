---
title: Skapa översättningsprojekt för innehållsfragment
description: Lär dig hur du översätter innehållsfragment i Adobe Experience Manager.
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: managing-assets
content-type: reference
feature: Content Fragments
role: User, Admin
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 5%

---

# Skapa översättningsprojekt för innehållsfragment {#creating-translation-projects-for-content-fragments}

Förutom resurser stöder Adobe Experience Manager (AEM) Assets arbetsflöden för språkkopiering för [innehållsfragment](/help/assets/content-fragments/content-fragments.md) (inklusive varianter). Ingen ytterligare optimering krävs för att köra språkkopieringsarbetsflöden på innehållsfragment. I varje arbetsflöde skickas hela innehållsfragmentet för översättning.

De typer av arbetsflöden som du kan köra på innehållsfragment liknar exakt de arbetsflödestyper som du kör för resurser. Alternativen som är tillgängliga för varje arbetsflödestyp matchar alternativen som är tillgängliga under motsvarande arbetsflödestyper för resurser.

Du kan köra följande typer av arbetsflöden för språkkopiering på innehållsfragment:

**Skapa och översätt**

I det här arbetsflödet kopieras innehållsfragment som ska översättas till språkroten för det språk som du vill översätta till. Beroende på vilka alternativ du väljer skapas dessutom ett översättningsprojekt för innehållsfragmenten i projektkonsolen. Beroende på inställningarna kan översättningsprojektet startas manuellt eller köras automatiskt så fort översättningsprojektet skapas.

**Uppdatera språkkopior**

När källinnehållets fragment uppdateras eller ändras kräver motsvarande språk-/språkspecifika innehållsfragment omöversättning. Arbetsflödet för att uppdatera språkkopior översätter ytterligare en grupp innehållsfragment och inkluderar det i en språkkopia för en viss språkinställning. I det här fallet läggs de översatta innehållsfragmenten till i målmappen som redan innehåller tidigare översatta innehållsfragment.

## Skapa och översätta arbetsflöde {#create-and-translate-workflow}

Arbetsflödet Skapa och översätt innehåller följande alternativ. Procedurstegen som är kopplade till varje alternativ liknar dem som är kopplade till motsvarande alternativ för tillgångar.

* Skapa endast struktur: Mer information om procedurer finns i [Skapa struktur endast för resurser](translation-projects.md#create-structure-only).
* Skapa ett översättningsprojekt: Om du vill veta mer om procedurer kan du läsa [Skapa ett översättningsprojekt för resurser](translation-projects.md#create-a-new-translation-project).
* Lägg till i det befintliga översättningsprojektet: Om du vill veta mer om procedurer kan du läsa [Lägg till i det befintliga översättningsprojektet för resurser](translation-projects.md#add-to-existing-translation-project).

## Uppdatera arbetsflödet för språkkopior {#update-language-copies-workflow}

Arbetsflödet för att uppdatera språkkopior innehåller följande alternativ. Procedurstegen som är kopplade till varje alternativ liknar dem som är kopplade till motsvarande alternativ för tillgångar.

* Skapa ett översättningsprojekt: Om du vill veta mer om procedurer kan du läsa [Skapa ett översättningsprojekt för resurser](translation-projects.md#create-a-new-translation-project) (uppdatera arbetsflöde).
* Lägg till i befintligt översättningsprojekt: Om du vill ha information om procedurer läser du [Lägg till i befintligt översättningsprojekt för resurser](translation-projects.md#add-to-existing-translation-project) (uppdatera arbetsflöde).

Du kan också skapa tillfälliga språkkopior för fragment som liknar det sätt som du skapar tillfälliga kopior för resurser. Mer information finns i [Skapa tillfälliga språkkopior för resurser](translation-projects.md#creating-temporary-language-copies).

## Översätta blandade mediefragment {#translating-mixed-media-fragments}

Med AEM kan du översätta innehållsfragment som innehåller olika typer av medieresurser och samlingar. Om du översätter ett innehållsfragment som innehåller textbundna resurser lagras de översatta kopiorna av dessa resurser under målspråkets rot.

Om innehållsfragmentet innehåller en samling, översätts resurserna i samlingen tillsammans med innehållsfragmentet. De översatta kopiorna av resurserna lagras i rätt målspråksrot på en plats som matchar den fysiska platsen för källresurserna under källspråkets rot.

Om du vill kunna översätta innehållsfragment som innehåller blandade media måste du först redigera standardöversättningsramverket för att aktivera översättning av textbundna resurser och samlingar som är kopplade till innehållsfragment.

1. Klicka på AEM logotyp och navigera till **[!UICONTROL Tools > Deployment > Cloud Services]**.
1. Leta upp **[!UICONTROL Translation Integration]** under **[!UICONTROL Adobe Marketing Cloud]** och klicka på **[!UICONTROL Show Configurations]**.

   ![chlimage_1-444](assets/chlimage_1-444.png)

1. Öppna sidan **[!UICONTROL Default configuration]** genom att klicka på **[!UICONTROL Default configuration (Translation Integration configuration)]** i listan över tillgängliga konfigurationer.

   ![chlimage_1-445](assets/chlimage_1-445.png)

1. Klicka på **[!UICONTROL Edit]** i verktygsfältet för att visa dialogrutan **[!UICONTROL Translation Config]**.

   ![chlimage_1-446](assets/chlimage_1-446.png)

1. Navigera till fliken **[!UICONTROL Assets]** och välj **[!UICONTROL Inline Media Assets and Associated Collections]** i listan **[!UICONTROL Translate Content Fragment Assets]**. Klicka på **[!UICONTROL OK]** om du vill spara ändringarna.

   ![chlimage_1-447](assets/chlimage_1-447.png)

1. Öppna ett innehållsfragment i den engelska rotmappen.

   ![chlimage_1-448](assets/chlimage_1-448.png)

1. Klicka på ikonen **[!UICONTROL Insert Asset]**.

   ![chlimage_1-449](assets/chlimage_1-449.png)

1. Infoga en resurs i innehållsfragmentet.

   ![infoga resurs i innehållsfragment](assets/column-view.png)

1. Klicka på ikonen **[!UICONTROL Associate Content]**.

   ![chlimage_1-451](assets/chlimage_1-451.png)

1. Klicka på **[!UICONTROL Associate Content]**.

   ![chlimage_1-452](assets/chlimage_1-452.png)

1. Markera en samling och ta med den i innehållsfragmentet. Klicka på **[!UICONTROL Save]**.

   ![chlimage_1-453](assets/chlimage_1-453.png)

1. Markera innehållsfragmentet och klicka på ikonen **[!UICONTROL GlobalNav]**.
1. Välj **[!UICONTROL References]** på menyn för att visa rutan **[!UICONTROL References]**.

   ![chlimage_1-454](assets/chlimage_1-454.png)

1. Klicka **[!UICONTROL Language Copies]** under **[!UICONTROL Copies]** för att visa språkkopiorna.

   ![chlimage_1-455](assets/chlimage_1-455.png)

1. Klicka på **[!UICONTROL Create & Translate]** längst ned på panelen för att visa dialogrutan **[!UICONTROL Create & Translate]**.

   ![chlimage_1-456](assets/chlimage_1-456.png)

1. Välj målspråk i listan **[!UICONTROL Target Languages]**.

   ![chlimage_1-457](assets/chlimage_1-457.png)

1. Välj översättningsprojekttypen i listan **[!UICONTROL Project]**.

   ![chlimage_1-458](assets/chlimage_1-458.png)

1. Ange projektets namn i rutan **[!UICONTROL Project Title]** och klicka sedan på **Skapa**.

   ![chlimage_1-459](assets/chlimage_1-459.png)

1. Navigera till **[!UICONTROL Projects]**-konsolen och öppna projektmappen för det översättningsprojekt som du skapade.

   ![chlimage_1-460](assets/chlimage_1-460.png)

1. Klicka på projektpanelen för att öppna sidan med projektinformation.

   ![chlimage_1-461](assets/chlimage_1-461.png)

1. Kontrollera antalet resurser som ska översättas i rutan Översättningsjobb.
1. Starta översättningsjobbet från rutan **[!UICONTROL Translation Job]**.

   ![chlimage_1-462](assets/chlimage_1-462.png)

1. Klicka på ellipserna längst ned i rutan Översättningsjobb för att visa översättningsjobbets status.

   ![chlimage_1-463](assets/chlimage_1-463.png)

1. Klicka på innehållsfragmentet för att kontrollera sökvägen till de översatta associerade resurserna.

   ![chlimage_1-464](assets/chlimage_1-464.png)

1. Granska språkkopian för samlingen i konsolen Samlingar.

   ![chlimage_1-465](assets/chlimage_1-465.png)

   Observera att endast innehållet i samlingen översätts. Själva samlingen är inte översatt.

1. Navigera till sökvägen för den översatta associerade resursen. Observera att den översatta resursen lagras under målspråkets rot.

   ![chlimage_1-466](assets/chlimage_1-466.png)

1. Navigera till de resurser i samlingen som har översatts tillsammans med innehållsfragmentet. Observera att de översatta kopiorna av resurserna lagras i rätt målspråksrot.

   ![chlimage_1-467](assets/chlimage_1-467.png)

   >[!NOTE]
   >
   >Procedurerna för att lägga till ett innehållsfragment i ett befintligt projekt eller för att utföra uppdateringsarbetsflöden liknar motsvarande procedurer för resurser. Vägledning om dessa förfaranden finns i de förfaranden som beskrivs för tillgångar.
