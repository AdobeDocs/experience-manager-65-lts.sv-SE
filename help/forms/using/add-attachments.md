---
title: Lägga till bilagor
description: Lägga till foton och anteckningar som anteckningar till dina uppgifter i AEM Forms-appen
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 0%

---

# Lägga till bilagor{#adding-attachments}

## Lägga till bilagor i formulär som har synkroniserats med AEM Forms Workflow Server (AEM Forms on JEE) {#adding-annotations}

Med AEM Forms-appen kan du bifoga bilder, anteckningar och textanteckningar i ditt formulär som synkroniseras med AEM Forms JEE-servern. Om formuläret läses in från en AEM Forms Workflow-server läggs dina bilagor till i formuläret. Du kan välja knappen för bifogade filer ![för bifogade filer-app](assets/attachments-app.png) om du vill visa alla bifogade filer i ett formulär tillsammans. Det röda meddelandet anger antalet bilagor i formuläret. Om det inte finns några bifogade filer i formuläret kan du inte se knappen med röda meddelanden. Om det inte finns några bifogade filer i formuläret får du alternativ för att bifoga foton eller klotter när du markerar knappen ![Bifoga](assets/attch.png).

Dina alternativ är:

* **Galleri**: Gör att du kan lägga till en bild från de bilder som har sparats på enheten.

* **Kamera**: Gör att du kan ta en bild och lägga till den i formuläret.

* **Anteckningar**: Gör att du kan lägga till en klocka eller en textanteckning. Använd ![klottra](assets/scribble.png) om du vill lägga till ett klottecken och ![tangentbord](assets/keyboard.png) om du vill lägga till en textanteckning.

>[!NOTE]
>
>Bifogade filer som läggs till av en användare visas för andra AEM Forms-appanvändare. Andra användare kan inte ta bort bilagor som lagts till av en användare.
>

### Skärmen Bifogade filer {#the-attachments-screen}

Om du vill visa alla bilagor på en plats väljer du ![attachments-app](assets/attachments-app.png). Du kan lägga till, byta namn på och ta bort bifogade filer här.

![Alla bifogade filer på en plats](assets/attachments-screen.png)

Du kan använda knappen **+** på skärmen Bifogade filer om du vill bifoga en annan bild, klottra eller text.

### Lägga till ett foto {#adding-a-photograph}

Du kan använda kameran på din mobila enhet eller sparade bilder på enheten för att bifoga en bild i formuläret.

1. Välj bilageknappen ![Attch](assets/attch.png) längst ned i fönstret.
1. Välj **Galleri** eller **Kamera** i popup-fönstret som visas.
1. Utför följande beroende på vilket alternativ du väljer:

   1. Om du väljer **Kamera**.

      Ta ett foto. Välj sedan knappen **Använd** ![use-pic](assets/use-pic.png) .

      Du kan också markera knappen **Återta** ![om](assets/retake.png) om du vill ta fotot igen.

   1. Om du väljer **Galleri**.

      Enhetens bildläsare visas. Markera den bild som du vill bifoga i bildwebbläsaren på enheten.

### Lägga till en anteckning {#adding-a-note}

Med alternativet **Anteckningar** kan du lägga till frihandsskript och textbilagor i formuläret.

1. Välj bilageknappen ![Attch](assets/attch.png) längst ned i fönstret.
1. Välj **Anteckningar** i popup-fönstret som visas.
1. Hämta ett frihandsskript i det användargränssnitt som startas.

   ![Klottra gränssnitt](assets/scribble-ui.png)

   Klottra

   Du kan använda följande alternativ i gränssnittet Klottra:

   * **Rensa**: Rensar skärmen.
   * **Klar knapp**: Kopplar det aktuella skriptet.
   * **Avbryt-knapp**: Ignorerar det aktuella skriptet och avslutar användargränssnittet i klotterformat.
   * ![tangentbord](assets/keyboard.png): Rensar skriptet och låter dig lägga till en textanteckning.

   ![Tangentbord i AEM Forms-appklocka](assets/keyboard-inapp.png)

## Bifogade filer i formulär som synkroniseras med AEM Forms-servrar utan AEM Forms Workflow (AEM Forms on OSGi) {#attachments-in-forms-synced-with-the-aem-forms-servers-without-aem-forms-workflow-aem-forms-on-osgi}

Bifogade filer för mobilformulär som synkroniseras med AEM Forms OSGi-servrar fungerar på samma sätt som AEM Forms JEE-servrar.

Bilagor på formulärnivå stöds inte för adaptiva formulär som läses in i appen från en AEM Forms OSGi-server. Om du vill bifoga bilder eller textanteckningar aktiverar du bilagor på fältnivå i formuläret när du redigerar det. Dra och släpp den bifogade filkomponenten från komponentwebbläsaren i fältet.

Om det finns adaptiva formulär kan du visa de bifogade filerna i postdokumentet (DoR). Se [Skapa postdokument för icke-XFA adaptiva formulär](../../forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.md).
