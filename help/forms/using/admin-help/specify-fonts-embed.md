---
title: Ange teckensnitt som ska bäddas in
description: Lär dig hur du anger teckensnitt som ska bäddas in i ett anpassat formulär. Du kan ange vilka teckensnitt som är inbäddade eller aldrig inbäddade med formulär som genereras av Forms-tjänsten.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_output
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 374f9425-b596-4481-8fd0-6df07c521a19
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Ange teckensnitt som ska bäddas in{#specify-fonts-to-embed}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan ange vilka teckensnitt som alltid är inbäddade eller aldrig inbäddade med de formulär som används i Output. När du bäddar in teckensnitt ökar formulärens filstorlek. Bädda in ovanliga teckensnitt som användarna troligtvis inte har i sina system och bädda inte in vanliga teckensnitt som de har installerat.

>[!NOTE]
>
>Om du har angett en anpassad XCI-fil för Output åsidosätter alternativet för inbäddning av teckensnitt i XCI-filen dessa inställningar. (Se [Ange filplatser för utdata](/help/forms/using/admin-help/specify-file-locations-output.md#specify-file-locations-for-output).)

1. Klicka på Tjänster > Utdata i administrationskonsolen.
1. Under Inställningar för teckensnittsinbäddning skriver du namnen på teckensnitten som ska bäddas in med formulären, avgränsade med kommatecken, i rutan Inkludera alltid teckensnitt. De teckensnitt du anger bäddas bara in i det genererade formuläret om de används i formuläret. Den här inställningen ignoreras om alternativet för att bädda in teckensnitt har aktiverats i XCI-filen som skickas till tjänsten. I så fall bäddas alla teckensnitt som används i PDF alltid in.
1. I rutan Inkludera aldrig teckensnitt skriver du namnen på de teckensnitt som inte ska bäddas in med formulären, avgränsade med kommatecken. De teckensnitt du anger bäddas inte in i PDF, även om de används i den genererade PDF. Den här inställningen ignoreras om alternativet för att bädda in teckensnitt har inaktiverats i XCI-filen som skickas till tjänsten. I så fall bäddas inget av teckensnitten som används i PDF in.
1. Klicka på Spara.
