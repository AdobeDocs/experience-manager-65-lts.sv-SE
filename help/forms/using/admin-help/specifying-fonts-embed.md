---
title: Ange teckensnitt som ska bäddas in
description: Lär dig hur du anger teckensnitt som ska bäddas in i ett anpassat formulär. Du kan ange vilka teckensnitt som är inbäddade eller aldrig inbäddade med formulär som genereras av Forms-tjänsten.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: c73dced8-7242-465c-85bc-9315a9a08605
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Ange teckensnitt som ska bäddas in {#specifying-fonts-to-embed}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan ange vilka teckensnitt som alltid är inbäddade eller aldrig inbäddade med de formulär som genereras av Forms-tjänsten. När du bäddar in teckensnitt ökar formulärens filstorlek. Bädda in ovanliga teckensnitt som användarna sällan har i sina system. Bädda inte in vanliga teckensnitt som de vanligtvis har installerade.

>[!NOTE]
>
>Om du har angett en anpassad XCI-fil för Forms åsidosätter alternativet för att bädda in teckensnitt i XCI-filen dessa inställningar. (Se [Konfigurera platser för Forms](/help/forms/using/admin-help/configuring-locations-forms.md#configuring-locations-for-forms).)

1. Klicka på **[!UICONTROL Services > Forms]** i administrationskonsolen.
1. I rutan **[!UICONTROL Font Embedding Settings]** skriver du namnen på teckensnitten som ska bäddas in med formulären, avgränsade med kommatecken. **[!UICONTROL Always Embed Fonts]** De teckensnitt du anger bäddas bara in i det genererade formuläret om de används i formuläret. Den här inställningen ignoreras om alternativet för att bädda in teckensnitt har aktiverats i XCI-filen som skickas till tjänsten, eftersom alla teckensnitt som används i PDF alltid bäddas in.
1. I rutan **[!UICONTROL Never Embed Fonts]** anger du namnen på teckensnitten som inte ska bäddas in med formulären, avgränsade med kommatecken. De teckensnitt du anger bäddas inte in i PDF, även om de används i den genererade PDF. Den här inställningen ignoreras om alternativet för inbäddning av teckensnitt har inaktiverats i XCI-filen som skickas till tjänsten, eftersom inga av teckensnitten som används i PDF bäddas in.
1. Klicka på **[!UICONTROL Save]**.
