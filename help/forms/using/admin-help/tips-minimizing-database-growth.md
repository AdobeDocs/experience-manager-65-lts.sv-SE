---
title: Tips för att minimera databastillväxt
description: I långvariga processer lagras processdata i AEM blankettdatabas. Tillväxten i AEM blankettdatabas kan minimeras med några enkla processkonfigurationsstrategier.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_aem_forms_database
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Tips för att minimera databastillväxt {#tips-for-minimizing-database-growth}

I långvariga processer lagras processdata i AEM blankettdatabas. Tillväxten i AEM blankettdatabas kan minimeras med några enkla processkonfigurationsstrategier.

## Processdesigntips {#process-design-tips}

Använd kortvariga processer när det är möjligt. Kortlivade processer lagrar inte processdata i databasen. Nackdelen med kortlivade processer är att deras status och tillstånd inte spåras i administrationskonsolen och att det inte finns någon historik över processen.

Vissa tjänståtgärder, till exempel Tilldela uppgift (användartjänst), kräver att de används i långvariga processer. I det här fallet kan du segmentera processen i flera underprocesser och göra dem kortvariga när det är möjligt. Om du använder den här strategin bör kortlivade underprocesser hantera stora dataobjekt, till exempel dokumentvärden.

Använd variabler sparsamt. När du använder långvariga processer tilldelas utrymme för varje processinstans i databasen för varje variabel i processen. Strategisk användning av variabler kan spara mycket utrymme. Du kan till exempel skriva över variabelvärden när gamla värden inte längre behövs i processen. Och ta bort alla variabler som du har skapat och inte använder. Du kan validera processen för att hitta oanvända variabler.

Använd enkla variabeltyper (till exempel sträng eller int) och undvik att använda komplexa variabeltyper när det är möjligt. Databasutrymme tilldelas för variabler även när de inte innehåller något värde. Komplexa variabler kräver vanligtvis mer utrymme än enkla.

## Tips för produktadministration {#product-administration-tips}

Använd global dokumentlagring (GDS) effektivt. GDS-katalogen på Forms Server används för att lagra bland annat filer som skickas till tjänster som ingår i AEM-formulär i processer. För att förbättra prestandan lagras mindre dokument i stället i minnet och sparas i databasen.

administrationskonsolen visar egenskapen Maximal intern dokumentstorlek för standarddokument för att konfigurera den maximala storleken på dokument som lagras i minnet och som finns kvar i databasen. (Se [Konfigurera allmänna inställningar för AEM-formulär](/help/forms/using/admin-help/configure-general-aem-forms-settings.md#configure-general-aem-forms-settings).) Om du anger den här egenskapen till ett lågt värde sparas de flesta dokument i GDS-katalogen i stället för i databasen. Fördelen är att du enklare kan ta bort filerna när de inte längre behövs när de lagras i GDS-katalogen.
