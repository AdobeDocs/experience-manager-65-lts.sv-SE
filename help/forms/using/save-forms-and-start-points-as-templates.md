---
title: Spara formulär som mallar
description: Lär dig hur du skapar mallar från formulär med data som krävs upprepade gånger.
contentOwner: khsingh
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 5e5ce783-8d0c-421c-b938-7020215682a0
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---

# Spara formulär som mallar {#save-forms-as-templates}

När användaren fyller i ett formulär ändras ibland inmatningen i ett fåtal fält. För sådana instanser kan du fylla i fält som kräver identiska värden i varje instans och spara formuläret eller utkastet som en mall. Varje gång du skapar en instans av mallen fylls nu de angivna fälten redan med värden som anges i mallen. Det hjälper dig att spara tid och arbete som krävs för att fylla i formuläret.

Så här skapar du en mall:

1. Öppna ett formulär och markera eller fyll i fält som har nästan identiska värden varje gång du använder det. Du kan inkludera en bifogad fil i mallen som du vanligtvis lägger till när du fyller i formuläret.
1. Välj ikonen **Spara som mall** ![save_as_template](assets/save_as_template.png). En dialogruta där du anger namnet på mallen visas.
1. Ange namnet på mallen och välj **Spara**. Mallen visas i mallmappen.

   Om det finns en mall med samma namn visas en dialogruta som bekräftar att den befintliga mallen skrivs över. Om du vill ersätta den befintliga mallen med en ny mall väljer du **Fortsätt** eller **Avbryt** om du vill spara mallen med ett annat namn.

Nu kan du öppna den sparade mallen. Varje gång en mall öppnas skapas ett nytt formulär eller en ny uppgift och formuläret visar sparade data och alternativ. Med mallar kan du redigera förfyllda data, lägga till en bifogad fil, spara som utkast, skicka uppgiften eller skapa en annan mall med hjälp av den. Mallar är specifika för mobila enheter och synkroniseras inte med Adobe Experience Manager Forms-servern.

Du kan också ta bort en mall om den inte längre behövs. Om du vill ta bort en mall går du till mallmappen, markerar ellipsen och väljer **Ta bort mall**.

>[!NOTE]
>
>En mall är tillgänglig lokalt och synkroniseras inte med servern. Om du rensar programmets lokala data tas mallen bort.
