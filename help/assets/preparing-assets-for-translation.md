---
title: Förbered resurser för översättning
description: Skapa rotmappar för språk för att förbereda resurser för översättning för stöd av flerspråkiga resurser.
contentOwner: AG
role: User, Admin
feature: Projects
solution: Experience Manager, Experience Manager Assets
exl-id: de9f266b-a167-4eba-be2c-8f6a0457265f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Förbered resurser för översättning {#preparing-assets-for-translation}

Flerspråkiga resurser innebär resurser med binärfiler, metadata och taggar på flera språk. I allmänhet finns binära filer, metadata och taggar för resurser på ett språk, som sedan översätts till andra språk för användning i flerspråkiga projekt.

I [!DNL Adobe Experience Manager Assets] inkluderas flerspråkiga resurser i mappar, där varje mapp innehåller resurserna på ett annat språk.

Varje språkmapp kallas för en språkkopia. Rotmappen för en språkkopia, som kallas språkrot, identifierar språket för innehållet i språkkopian. */content/dam/it* är till exempel den italienska språkroten för den italienska språkkopian. Språkkopior måste använda en [korrekt konfigurerad språkrot](preparing-assets-for-translation.md#creating-a-language-root) så att rätt språk används när översättningar av källresurser utförs.

Språkkopian som du ursprungligen lade till resurser för är det primära språket. Det primära språket är källan som översätts till andra språk. En exempelmapphierarki innehåller flera språkrötter:

```shell
/content
    /- dam
        |- en
        |- fr
        |- de
        |- es
        |- it
        |- ja
        |- zh
```

Utför följande steg för att förbereda dina resurser för översättning:

1. Skapa språkroten för det primära språket. Språkroten för den engelska språkkopian i exempelmapphierarkin är till exempel `/content/dam/en`. Kontrollera att språkroten är korrekt konfigurerad enligt informationen i [Skapa en språkrot](preparing-assets-for-translation.md#creating-a-language-root).

1. Lägg till resurser på ditt primära språk.
1. Skapa språkroten för varje målspråk som du behöver en språkkopia för.

## Skapa en språkrot {#creating-a-language-root}

Om du vill skapa språkroten skapar du en mapp och använder en ISO-språkkod som värde för egenskapen Namn. När du har skapat språkroten kan du skapa en språkkopia på valfri nivå i språkroten.

Rotsidan för den italienska språkkopian av exempelhierarkin har till exempel `it` som namnegenskap. Egenskapen Namn används som namn på objektnoden i databasen och avgör därför sökvägen till resurserna. (`https://[aem_server]:[port]/assets.html/content/dam/it/`).

1. Klicka på **[!UICONTROL Create]** i [!DNL Assets]-konsolen och välj **[!UICONTROL Folder]** på menyn.

   ![Skapa mapp](assets/Create-folder.png)

1. I fältet **[!UICONTROL Name]** skriver du landskoden i formatet `<language-code>`.

   ![Lägg till språkkod i mappen](assets/Add-language-code-in-folder.png)

1. Klicka på **[!UICONTROL Create]**. Språkroten skapas i konsolen [!DNL Assets].

## Visa språkrötter {#viewing-language-roots}

Gränssnittet [!DNL Experience Manager] innehåller en **[!UICONTROL References]**-panel som visar en lista med språkrötter som har skapats i [!DNL Assets].

1. I konsolen [!DNL Assets] väljer du det primära språk som du vill skapa språkkopior för.
1. Välj alternativet **[!UICONTROL References]** i den vänstra listen för att öppna rutan [!UICONTROL Reference].

   ![chlimage_1-122](assets/chlimage_1-122.png)

1. Klicka på **[!UICONTROL Language Copies]** i rutan Referenser. På panelen [!UICONTROL Language Copies] visas språkkopior av resurserna.

   ![språkkopior](assets/lang-copy2.png)
